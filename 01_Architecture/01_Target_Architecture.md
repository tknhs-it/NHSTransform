# Nossal Digital Transformation — Target Architecture

**Version:** 1.0  
**Date:** January 29, 2026  
**Scope:** One Front Door via Microsoft 365 (A5-first approach)

---

## 1. Architectural Model

### Core Principle
Teams is where people live → Viva Connections becomes the "Nossal Digital Hub" → Everything orchestrated through Microsoft 365 native tools.

### Layer Stack

#### Experience Layer
- **Teams** (default workspace for all staff & students)
- **Viva Connections app** (embedded in Teams) — the Nossal Hub dashboard
  - Links to services
  - News & announcements
  - Integrated search
- **SharePoint Home Site + Hub** (powers content, navigation, service catalogue, knowledge base)

#### Workflow Layer
- **Power Apps** — forms & apps (IT requests, maintenance, access, bookings, purchases)
- **Power Automate** — triage, approvals, notifications, task routing to Planner
- **Microsoft Approvals** — structured approval gates (manager, budget owner, IT)

#### Data Layer (MVP)
- **SharePoint Lists** (system-of-record)
  - ITRequests
  - MaintenanceRequests
  - AccessRequests
  - AssetLoans
  - PurchaseRequests
  - ServiceCatalogue
  - KnowledgeBase
- **SharePoint Libraries** (attachments, photos, documents)

#### Security & Governance Layer
- **Entra ID** — identity, MFA, Conditional Access
- **Purview** — retention, sensitivity labels, DLP (especially student/staff data)
- **Defender** — endpoint, email baseline hygiene
- **Power Platform Admin Center** — environment strategy (DEV/PROD separation)

---

## 2. Identity Reality

### Admin vs. Curricular Directory

**Current state:** Does Nossal have one M365 tenant, or separate admin/curricular forests?

#### Option A: Single Tenant Hub (Recommended if unified)
- All staff, students, systems in one M365 tenant
- Best UX for discovery, search, cross-service experience
- Simpler governance, audit trail, identity sync

#### Option B: Multi-tenant with Cross-Tenant Access
- Separate admin/curricular forests
- Users can be members of both via cross-tenant access policies
- More complex identity/search experience
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
