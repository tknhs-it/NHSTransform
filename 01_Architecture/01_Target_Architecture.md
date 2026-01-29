# Nossal Digital Transformation — Target Architecture

**Version:** 1.0  
**Date:** January 29, 2026  
**Scope:** One Front Door via Microsoft 365 (A5-first approach)

---

## 1. Architectural Model

### Core Principle
**Compass is the system-of-record for school operations.** Microsoft 365 becomes the unified front door, workflow engine, and collaboration platform. Single sign-on (SSO) connects them seamlessly.

### Systems of Record (Don't Duplicate)

**Compass (authoritative source)**
- Timetables, attendance, learning tasks & assessment
- Wellbeing, pastoral notes, parent comms
- Finance (if applicable)
- Student/staff/class directory
- Events, excursions, calendar

**Microsoft 365 (front door + operational workflows)**
- IT support tickets, asset loans, maintenance requests (things Compass doesn't handle)
- Access/permissions workflows (Entra ID groups, shared mailboxes, admin roles)
- Procurement & purchase approvals
- Internal collaboration (Teams channels, shared documents)
- Unified search & discovery (Viva Connections hub)

### Layer Stack

#### Experience Layer
- **Teams** (default workspace for all staff & students)
- **Viva Connections app** (embedded in Teams) — the Nossal Digital Hub dashboard
  - Tiles for Compass services (deep links via SSO)
  - Tiles for Microsoft 365 workflows (Power Apps)
  - News & announcements
  - Integrated search (spans Compass + M365)
- **SharePoint Home Site + Hub** (powers navigation, service catalogue, knowledge base)
- **Compass** (launched via SAML/SSO from Hub, users never leave the flow)

#### Workflow Layer
- **Power Apps** — forms & apps (IT requests, maintenance, access, asset loans, purchases)
- **Power Automate** — triage, approvals, notifications, task routing to Planner
- **Microsoft Approvals** — structured approval gates (manager, budget owner, IT)
- **Compass integrations** — SSO, SDS (School Data Sync), iCal exports, CSV data feeds

#### Data Layer

**Microsoft 365 Lists** (transactional workflows)
- ITRequests, MaintenanceRequests, AccessRequests, AssetLoans, PurchaseRequests
- ServiceCatalogue (new: Compass-aware with SystemOfRecord + LaunchType fields)
- KnowledgeBase

**Compass Lists/Exports** (authoritative, not duplicated)
- Rosters, attendance, learning tasks (via SDS → Teams classes)
- Timetable (via iCal or calendar export)
- People/contact directory
- Events

**Power BI Data Sources** (merged view)
- M365 lists (request metrics, SLA, volumes)
- Compass CSV exports (attendance trends, engagement, cohort analysis)
- Combined dashboards for operational insights

**SharePoint Libraries** (attachments, documents)
- Request attachments (IT tickets, maintenance photos, quotes)
- Knowledge base media

#### Security & Governance Layer
- **Entra ID** — identity, MFA, Conditional Access
- **Compass SAML/SSO** — seamless single sign-on to Compass from Microsoft Hub
- **Purview** — retention, sensitivity labels, DLP (especially student/staff data)
- **Defender** — endpoint, email baseline hygiene
- **Power Platform Admin Center** — environment strategy (DEV/PROD separation)

---

## 2. Compass Integration Strategy

### Why Compass + Microsoft 365 Together?

**Compass** is purpose-built for school operations (rosters, attendance, learning tasks, wellbeing). **Microsoft 365** excels at collaboration, workflows, and discovery. Don't duplicate data—integrate smartly.

Compass supports:
- SAML/SSO integration (seamless login)
- Microsoft School Data Sync (SDS) for roster/class sync
- REST API (with AIP governance for deeper integrations)
- iCal feeds (calendar export)
- CSV exports (attendance, people, events)

### Integration Levels (Implement Progressively)

#### Level 1: Single Sign-On (Sprint 1 — Do This First)

**What:** Entra ID ↔ Compass SAML/SSO

**Outcome:** User logs into Teams once, then clicks a Compass tile in Nossal Hub and lands in Compass without re-authenticating.

**Implementation:**
- Configure Compass to trust Entra ID as SAML identity provider
- Create Entra ID app registration for Compass
- Test with pilot group
- Update Nossal Hub to include Compass deep links (Attendance, Learning Tasks, Wellbeing, Events)

**Effort:** 2-3 days (IT + Compass admin)

#### Level 2: Rosters → Teams (Sprint 2 — Unlocks Teaching Structure)

**What:** Compass → Microsoft SDS → Auto-create Teams class groups

**Outcome:** Teachers have Teams channels for each class automatically. No manual setup.

**Implementation:**
- Enable SDS in Compass (Compass lists SDS v2.1 as integration)
- Configure SDS connector in Microsoft 365 admin center
- Map Compass classes → Microsoft class groups
- Teams auto-creates per class
- Staff + students auto-added to class teams

**Effort:** 3-5 days (Compass admin + M365 admin)

**Note:** SDS can also feed attendance, assignment data (if configured), but MVP is just class/roster sync.

#### Level 3: Calendar Surfaces (Ongoing — Display Only)

**What:** Compass iCal feeds into Outlook/Teams calendar

**Outcome:** Staff see timetable + school events in their Teams calendar (read-only).

**Implementation:**
- Extract iCal feed from Compass (per user or per event category)
- Add iCal feed to Outlook calendar (personal or organizational)
- Caveat: ICS-based timetable shows as "Free" time (don't rely on calendar for true scheduling—Compass is still the truth)

**Effort:** 1 day (M365 admin)

#### Level 4: Data for Dashboards (Sprint 2+ — Ongoing)

**What:** Compass CSV exports → SharePoint library → Power BI dashboards

**Outcome:** Combine Compass metrics (attendance trends, engagement) with M365 metrics (IT request volumes, procurement SLA) in unified dashboards.

**Implementation:**
- Compass admin exports attendance/people/events as CSV monthly
- Upload to SharePoint "Compass Data Exports" library
- Power BI connects to library, merges with M365 lists
- Dashboards show "Student engagement vs. IT issues" type insights

**Effort:** 2 days (setup), then manual monthly (automate later if needed)

#### Level 5: "Real" Compass API Integration (Later — Deliberate)

**What:** Direct REST API calls from Power Automate to Compass

**Outcome:** Auto-create Compass events from M365 requests, or fetch real-time rosters.

**Implementation:**
- Apply for Compass AIP (Application Integration Partner) pathway
- Develop custom Power Automate connectors
- Example: "When asset loan is approved in M365, log entry in Compass for audit"

**Effort:** Significant (requires AIP approval, custom dev)

**Recommendation:** Start with Level 1-3, revisit Level 5 in 2027 if demand justifies it.

---

## 3. Identity Reality

### Admin vs. Curricular Directory

**Current state:** Does Nossal have one M365 tenant, or separate admin/curricular forests?

#### Option A: Single Tenant Hub (Recommended)
- All staff, students, systems in one M365 tenant
- Best UX for discovery, search, cross-service experience
- Compass SSO works seamlessly
- Simpler governance, audit trail, identity sync

#### Option B: Multi-tenant with Cross-Tenant Access
- Separate admin/curricular forests
- More complex SSO / Compass integration
- Requires explicit cross-tenant trust setup

**Next step:** Confirm current identity topology, then lock in approach.

---

## 3. Licensing (A5-First Strategy)

### What Works Within A5 (Microsoft 365 Education A5)

✅ **Included / Standard Connectors**
- SharePoint, Teams, Outlook, OneDrive
- Planner, Approvals, Forms
- Power Apps (canvas & model-driven)
- Power Automate (standard triggers/actions)
- Power BI Pro
- Entra ID + Conditional Access + MFA
- Purview (retention, sensitivity labels, DLP)
- Defender (endpoint, email)

### Premium Connectors & Traps to Avoid (trigger licensing)

❌ **Avoid unless you budget for Premium per-flow licensing**
- HTTP / Graph API calls (unprofessional connectors)
- Third-party SaaS connectors (Slack, Salesforce, etc.)
- Dataverse as primary backend (requires separate licensing)
- Many AI/ML advanced connectors

### MVP Strategy

**Use:**
- SharePoint Lists as system-of-record
- Standard Microsoft 365 connectors only
- Power Apps & Power Automate for basic forms & flows
- Power BI Pro dashboards (included in A5)

**Avoid (v1):**
- Custom code (Azure Functions, Azure Logic Apps)
- Premium connectors
- Dataverse
- Advanced analytics platform (Fabric) — save for v2

---

## 4. System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                      NOSSAL HUB (UX)                             │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Teams (Desktop/Mobile)                                      ││
│  │ ├─ Nossal Hub (Viva Connections App)                        ││
│  │ │  ├─ Dashboard (tiles: Request, KnowledgeBase, etc.)      ││
│  │ │  ├─ Integrated Search (content + requests)               ││
│  │ │  └─ News & Announcements                                 ││
│  │ ├─ IT Support Channel (triage + updates)                   ││
│  │ ├─ Facilities Channel (maintenance alerts)                 ││
│  │ └─ Planner (task tracking for staff)                       ││
│  └─────────────────────────────────────────────────────────────┘│
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ SharePoint Home Site (Web, in browser or Teams embed)       ││
│  │ ├─ Navigation Hub (Service Catalogue, KnowledgeBase)        ││
│  │ ├─ Service Catalogue pages (filtered views)                 ││
│  │ └─ Knowledge Base (runbooks, FAQs, how-to)                  ││
│  └─────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
                              ↓↑
┌─────────────────────────────────────────────────────────────────┐
│               WORKFLOW & APPLICATION LAYER                       │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Power Apps (Canvas Apps embedded in Teams & SharePoint)     ││
│  │ ├─ IT Support Form                                          ││
│  │ ├─ Maintenance Request Form                                 ││
│  │ ├─ Access Request Form                                      ││
│  │ ├─ Asset Loan Form                                          ││
│  │ └─ Purchase Request Form                                    ││
│  └─────────────────────────────────────────────────────────────┘│
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Power Automate (Cloud Flows)                                ││
│  │ ├─ IT Triage Flow (route by category)                       ││
│  │ ├─ Approval Flows (manager, budget, IT)                     ││
│  │ ├─ Notification Flows (Teams, email)                        ││
│  │ ├─ Planner Integration (create tasks)                       ││
│  │ └─ Status Update Flows                                      ││
│  └─────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
                              ↓↑
┌─────────────────────────────────────────────────────────────────┐
│                    DATA LAYER (SharePoint)                       │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ SharePoint Lists (System-of-Record)                         ││
│  │ ├─ ITRequests (tickets, routing, status)                    ││
│  │ ├─ MaintenanceRequests (location, issue type, photos)       ││
│  │ ├─ AccessRequests (permissions, approvals, audit)           ││
│  │ ├─ AssetLoans (equipment, borrower, dates, condition)       ││
│  │ ├─ PurchaseRequests (quote, approvals, delivery)            ││
│  │ ├─ ServiceCatalogue (service metadata, routing rules)        ││
│  │ └─ KnowledgeBase (articles, categories, search metadata)    ││
│  └─────────────────────────────────────────────────────────────┘│
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ SharePoint Libraries (Documents, Attachments)               ││
│  │ ├─ Request Attachments (photos, PDFs, quotes)               ││
│  │ └─ Knowledge Base Media (images, videos)                    ││
│  └─────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
                              ↓↑
┌─────────────────────────────────────────────────────────────────┐
│              SECURITY & GOVERNANCE LAYER                         │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Entra ID (Identity & Access)                                ││
│  │ ├─ User provisioning (staff, students, contractors)         ││
│  │ ├─ Group membership (team owners, approvers, etc.)          ││
│  │ ├─ MFA & Conditional Access                                 ││
│  │ └─ Privileged Identity Management (PIM for admins)          ││
│  └─────────────────────────────────────────────────────────────┘│
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Purview & Compliance                                        ││
│  │ ├─ Retention Policies (requests: 7y, responses: 3y)          ││
│  │ ├─ Sensitivity Labels (Staff-Only, Confidential, etc.)       ││
│  │ ├─ DLP Policies (no consumer connectors, no external share)   ││
│  │ └─ Audit Log (request creation, approvals, access changes)  ││
│  └─────────────────────────────────────────────────────────────┘│
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Power Platform Governance                                   ││
│  │ ├─ DEV Environment (builder sandbox)                        ││
│  │ ├─ PROD Environment (published apps/flows only)             ││
│  │ ├─ Change Control (who can deploy to PROD)                  ││
│  │ └─ Connector Policy (block consumer connectors)              ││
│  └─────────────────────────────────────────────────────────────┘│
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Defender (Baseline Hygiene)                                 ││
│  │ ├─ Endpoint Protection (device management)                  ││
│  │ └─ Email Security (phishing, malware, DLP)                  ││
│  └─────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
```

---

## 5. Key Design Decisions

| Decision | Rationale |
|----------|-----------|
| **Viva Connections Hub** | Native Teams integration, no custom portal friction, built-in search |
| **SharePoint Lists (not Dataverse v1)** | A5 licensing, no premium connector costs, good enough for MVP |
| **Power Apps for forms** | Beats manual form collection, embed in Teams, mobile-ready |
| **Power Automate for routing** | Replaces manual triage, integrates with Planner, teams, approvals |
| **Planner as task tracking** | Familiar to staff, integrated with Teams, no premium licensing |
| **Power BI Pro dashboards** | Included in A5, real metrics on request volumes, SLA attainment |
| **Purview from day 1** | Schools must manage student/staff data; retention + labels are baseline |
| **DEV/PROD separation** | Prevents "oops" moment of broken flows in production |

---

## 6. Next Steps

1. **Confirm identity topology** (single tenant vs. multi-tenant)
2. **List current "pain points"** (which requests are most chaotic today?)
3. **Identify IT + Facilities champions** (who owns the workflows?)
4. **Plan Sprint 1** (SharePoint Home Site setup, Viva Connections, Service Catalogue)
5. **Assign Power Platform admins** (who manages DEV/PROD, change control?)

---

**Ownership:** Digital Transformation Team  
**Last Updated:** January 29, 2026  
**Status:** Approved for Sprint 1 Initiation
