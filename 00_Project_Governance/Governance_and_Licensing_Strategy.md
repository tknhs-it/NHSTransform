# Nossal Digital Transformation — Governance & Licensing Strategy

**Version:** 1.0  
**Date:** January 29, 2026  
**Scope:** A5-first approach, avoiding premium licensing traps, compliance for schools

---

## 1. Licensing Overview: What M365 A5 Education Includes

### What You Get (No Extra Cost)

| Component | Capability | Use in Nossal Hub |
|-----------|-----------|---|
| **SharePoint** | 5TB per tenant + libraries | Portal, lists, document storage |
| **Teams** | Unlimited conversations, channels | Hub workspace, triage channels |
| **Outlook** | Email, calendar, distribution groups | Notifications, approvals |
| **Planner** | Task management, buckets, assignments | Task tracking for workflows |
| **Microsoft Forms** | Surveys & quizzes | Knowledge base feedback (optional) |
| **Power Apps** | Canvas & model-driven apps (100 apps per tenant) | Request forms, dashboards |
| **Power Automate** | Cloud flows (standard connectors) | Routing, approvals, notifications |
| **Power BI Pro** | Published dashboards, reports | Service metrics dashboards |
| **Entra ID** | Identity, groups, MFA, CA, PIM | Identity & security foundation |
| **Purview** | Retention policies, sensitivity labels, DLP | Data protection, compliance |
| **Defender** | Endpoint, email, threat protection | Security baseline |
| **Viva Connections** | Hub experience, dashboard | Nossal Digital Hub app |

### What Costs Extra (Premium Licensing)

| Scenario | Trigger | Cost | Workaround |
|----------|---------|------|-----------|
| **HTTP Connector in Power Automate** | Calling external REST APIs | $15/month per flow (or per user) | Use SharePoint webhooks instead |
| **Premium Connectors** | Calling Salesforce, SAP, etc. | $15/month per flow | Avoid unless truly needed (v2+) |
| **Dataverse** | Complex data model, relationships | $40-100/month per tenant | Use SharePoint Lists for v1 MVP |
| **Custom Functions in Power Fx** | Advanced formulas beyond UI | Included in Power Apps | Keep formulas simple |
| **Power Automate Approval History** | Storing approval audit trail | Included (in list items) | Attach to SharePoint list for audit |

---

## 2. How to Stay A5-Only (MVP Strategy)

### ✅ DO (A5-Safe)

**Power Apps**
- ✅ Canvas apps with SharePoint List data sources
- ✅ Simple form controls (text, choice, person, date, attachment)
- ✅ Gallery displays of list items
- ✅ Buttons that call Power Automate flows
- ✅ Embed in Teams and SharePoint pages

**Power Automate**
- ✅ Cloud flows triggered by list changes
- ✅ SharePoint list actions (create, update, read items)
- ✅ Teams connectors (post message, create channel, etc.)
- ✅ Outlook connectors (send email, create event)
- ✅ Approvals connector (structured approval gates)
- ✅ Planner connectors (create tasks, update tasks)
- ✅ Standard connectors (defined by Microsoft as "free")

**SharePoint Lists**
- ✅ Lists as system-of-record (no Dataverse needed)
- ✅ Multiple lists with lookups between them
- ✅ Calculated columns (formulas)
- ✅ Choice fields, person fields, date fields, attachments
- ✅ Views and filters for different audiences
- ✅ Libraries for document storage (retaining old files per policy)

**Power BI**
- ✅ Published dashboards pinned to SharePoint
- ✅ Connecting to SharePoint Lists as data source
- ✅ Simple visualizations (tiles, charts, tables)
- ✅ Row-level security (staff only see their dept's data)
- ✅ Refresh daily or weekly

### ❌ DON'T (Premium Trigger)

**Power Apps / Power Automate**
- ❌ HTTP connector (calling external APIs)
- ❌ SQL Server connector
- ❌ Salesforce, SAP, Slack connectors
- ❌ Custom code in Azure Functions
- ❌ Dataverse as primary backend (different licensing model)

**Complex Integrations (v2+ only)**
- ❌ Real-time sync with Facilities Management System
- ❌ Integration with Student Information System (SIS)
- ❌ Business intelligence platform (Fabric/Analytics)

---

## 3. Power Platform Environment Strategy

### Why Separate DEV/PROD?

**Risk:** A broken flow in production breaks a service for all users.  
**Solution:** Test in DEV, promote to PROD only after validation.

### Environment Structure

```
┌─────────────────────────────────────────┐
│     Power Platform Admin Center          │
├─────────────────────────────────────────┤
│                                          │
│  DEV Environment (Sandbox)               │
│  ├─ Users: IT admins + Power Platform   │
│  ├─    Builders                          │
│  ├─ Power Apps: Draft & test            │
│  ├─ Power Automate: Test flows          │
│  ├─ SharePoint: DEV lists (copy of     │
│  │    production schema)                 │
│  └─ Retention: 30 days (ephemeral)      │
│                                          │
│  PROD Environment (Production)           │
│  ├─ Users: Everyone                     │
│  ├─ Power Apps: Published & certified   │
│  ├─ Power Automate: Live flows only     │
│  ├─ SharePoint: Real business data      │
│  ├─ Change Control: Only approved       │
│  │    changes via formal request        │
│  └─ Retention: Per company policy (7y+) │
│                                          │
└─────────────────────────────────────────┘
```

### DEV Environment Setup

**Owner:** IT Digital Transformation lead + 1-2 Power Platform builders

**Users:**
- IT Director / Digital Lead
- 2-3 Power Platform app builders
- IT Project Manager

**Rules:**
- Anyone in DEV can create & test apps
- No approval needed (sandbox experimentation)
- DEV lists mirror PROD schema but are empty
- Test data only
- Flows can be half-built, broken, work-in-progress
- 30-day retention (auto-delete unused items)

**Promotion process DEV → PROD:**
1. App is built & tested in DEV
2. Creates ticket in PROD change control: "Promote [App Name]"
3. IT Lead reviews + validates
4. On approval, re-create app in PROD environment
5. Test again in PROD with small group
6. Release to all users

### PROD Environment Setup

**Owner:** IT Director + IT Service Manager

**Users:**
- Everyone using the Nossal Hub
- Service admins for list maintenance

**Rules:**
- No direct app creation in PROD
- All changes go through formal change control ticket
- Flows are locked (no casual edits)
- Read-only access to flows for audit
- Backup: daily SharePoint backup (included in M365)
- Retention: per school record retention policy (7 years for requests)

**Access Control:**
- Entra ID groups define who can use each app
- E.g., IT Support app available to all staff/students
- Access Request app available to staff only
- Maintenance Request app available to all staff/students
- Purchase Request app available to staff only (authorization-based)

---

## 4. Data Protection & Compliance (School-Grade)

### Why It Matters

Schools hold sensitive data:
- **Student information** (name, date of birth, family contact, medical needs, behavior records)
- **Staff data** (employment history, performance reviews, payroll)
- **Facilities** (building access, CCTV)

If compromised, schools face regulatory fines + reputational damage. A5 gives you the tools.

### Retention Policy: What to Keep, for How Long

**IT Requests:**
- Keep: 7 years (audit trail, dispute resolution, pattern analysis)
- Destroy: After 7 years (delete from SharePoint list)
- Exceptions: Critical security incidents (save indefinitely)

**Maintenance Requests:**
- Keep: 3 years (contractor disputes, warranty claims, facility history)
- Destroy: After 3 years

**Access Requests:**
- Keep: 7 years (audit compliance, regulatory review)
- Destroy: After 7 years

**Purchase Requests:**
- Keep: 7 years (accounting, budget audits, fraud investigation)
- Destroy: After 7 years

**Knowledge Base Articles:**
- Keep: Until superseded (evergreen content)
- Archive: Old versions (keep for 1 year in case rollback needed)

**Implementation:**
- Create retention labels in Purview: "IT Records 7y", "Maintenance 3y", "Confidential Staff-Only"
- Apply labels to SharePoint lists automatically (via metadata)
- Enable retention policies that auto-archive to archive list after X years
- Allow IT admin to hard-delete after retention expires

### Sensitivity Labels (Who Sees What)

**Public** (default)
- Knowledge Base public articles
- Home page announcements
- General service info
- Audience: Everyone

**Staff-Only** (default for most workflows)
- IT Requests (staff member name, device details, access requests)
- Maintenance Requests (room/location info, damage notes)
- Access Requests (names, permissions granted)
- Knowledge Base articles on policies
- Audience: Staff only (students can't see staff names on requests)

**Admin-Only** (most sensitive)
- Dashboards (staff names, request volumes, SLA metrics)
- Budget/cost data (Purchase Request costs)
- IT escalations & security incidents
- Audience: IT + Finance admins only

**Implementation in Purview:**
1. Create 3 sensitivity labels: Public, Staff-Only, Admin-Only
2. Each label has automatic DLP rules:
   - Public: Allow external sharing (monitored)
   - Staff-Only: Block external sharing, block if downloaded to unmanaged device
   - Admin-Only: Block external sharing, audit only (no external users at all)
3. Apply labels to SharePoint sites:
   - Nossal Hub (Public): Default = Staff-Only, can override to Public per page
   - Admin Dashboards: Default = Admin-Only
4. Teams channels inherit from parent site sensitivity

---

## 5. Data Loss Prevention (DLP) Policy

### Problem
User copies a list of student names + ID numbers and emails it to a personal Gmail account. GDPR breach.

### Solution: DLP Rule

**Rule: Block sensitive content leaving school systems**

```
IF content contains:
   - Student ID number (pattern: S + 5 digits)
   - Staff member name + email together
   - Classroom/building names + access codes
   
AND destination is:
   - External email (not @school domain)
   - OneDrive (personal, not work)
   - Consumer SaaS (Gmail, personal Dropbox)
   
THEN:
   - Block the action (user gets error: "This content is sensitive. Cant share externally.")
   - Log the attempt (audit trail)
   - Alert IT if > 5 attempts in 1 hour
```

**Exceptions (approved):**
- Staff can email internal stakeholders (other staff)
- External contractors with NDA (whitelisted)
- Parent communications (processed via official channels)

**Implementation:** Set up DLP in Purview > Data Loss Prevention > Create custom rule

---

## 6. Entra ID & MFA (Identity Security)

### Users in the System

**Staff** (all have M365 accounts)
- Full Nossal Hub access
- Can submit IT, Maintenance, Access, Purchase requests
- Can approve requests (manager role)
- Can view own request status

**Students** (M365 accounts or federated)
- Reduced Nossal Hub access (student-only tiles)
- Can submit IT & Maintenance requests
- Can request Absence (not Purchase)
- Cannot view staff data
- Cannot access admin dashboards

**Contractors / Visitors** (B2B guests if needed)
- Limited access (specific SharePoint sites only)
- No Power Apps access (read-only to KB)
- Temporary (expires after X days)

### MFA Policy

**Requirement:** All staff + students must use MFA when accessing M365 from non-managed devices

**Implementation (Conditional Access):**
1. Group: All Staff & Students
2. Cloud app: All Microsoft 365 services
3. Condition: Sign-in risk HIGH or Device unmanaged
4. Action: Require MFA
5. Exclude: Teacher aides, student workers (risk assessment)

**Setup:**
- Entra ID > Security > Conditional Access > Create new policy
- Use Authenticator app, phone sign-in, or SMS
- Don't use security questions (easy to guess)

### Admin Access & Privileged Identity Management (PIM)

**Problem:** Every IT admin has permanent rights to change passwords, reset access, etc. If one admin account is compromised, attacker has full control.

**Solution:** PIM (included in M365 A5)

**Implementation:**
- Define "high-risk" roles: Global Admin, Security Admin, Entra Admin, SharePoint Admin
- These roles require "just-in-time" activation
- Admin must request 8h access, provide justification, manager approves
- Access expires after 8h
- All actions logged + audited
- Multi-factor authentication required to activate

**Setup:** Entra ID > PIM > Roles > Enable PIM for high-risk roles

---

## 7. Defender: Baseline Hygiene

### What It Does

**Defender for Endpoint** (all school devices)
- Antivirus + anti-malware
- Detects suspicious behavior
- Blocks known threats
- Auto-quarantines suspicious files
- Reports to IT dashboard

**Defender for Office 365**
- Scans inbound email for phishing + malware
- Blocks external emails from spoofing school domain
- Detects ransomware attachments
- Auto-remediation (quarantine suspicious emails)
- Reports phishing attempts

### Enablement

**Automatic:** Included with M365 A5 Education. Enabled by default.

**Configuration:**
- Entra ID > Security > Threat Protection > Microsoft Defender
- Enable: Phishing protection, Malware protection, External email spoofing block
- Set: Auto-remediation ON (quarantine suspicious email immediately)
- Alert IT on: 3+ failed logins, impossible travel, password spray attacks

---

## 8. Audit & Compliance Logging

### Why It Matters

**Audit trail:** Who did what, when, where.
- Accessing student data
- Approving a $5k purchase
- Granting admin rights
- Deleting a request

### What's Logged (Free in A5)

- User sign-ins (success + failures)
- File access in SharePoint (who opened, deleted, shared)
- Power Automate flow runs (trigger, actions, outputs)
- Planner task changes
- Approvals (who approved, when, decision)
- Entra ID changes (group membership, password reset)

### How to Export & Review

**Purview > Audit > Search audit logs**
- Filter: User, Activity, Date range, IP address
- Example: "Who accessed student names in the IT Requests list in the last 30 days?"
- Export to CSV for investigation

**Set retention:** 90 days by default (sufficient for incident investigation)

---

## 9. Change Control Process (Preventing Chaos)

### Why It Matters

**Broken flow deployed to production** → 500 staff can't submit IT requests for 2 hours → chaos.

### The Process

```
1. Builder finishes app in DEV, tests thoroughly
   
2. Creates ticket in PROD change control:
   - Title: "Deploy [App Name] to Production"
   - Description: What it does, why it's needed, who tested it
   - Risk level: Low / Medium / High
   - Rollback plan: How to undo if it breaks
   
3. IT Lead reviews:
   - Tests again in DEV
   - Reviews Power Automate flows for premium connectors
   - Checks SharePoint lists for compliance + retention labels
   - Approves or requests changes
   
4. On approval:
   - Schedule deployment window (e.g., Friday 4-5pm, lower-traffic time)
   - Notify stakeholders: "We're releasing [App] at [time]"
   
5. Deploy:
   - Export app from DEV
   - Import app to PROD
   - Repoint to PROD lists / connections
   - Run smoke tests (can I open it? Can I submit a request?)
   
6. Release:
   - Update Viva Connections tiles to show new app
   - Post Teams message: "New app now available: [App Name]"
   - Monitor for errors in Planner/Teams channels
   
7. If error detected:
   - Rollback: Disable app or revert to previous version
   - Post: "We're investigating an issue with [App]. Should be back online in 1h"
   - Root cause analysis + post-mortem
```

### Change Control Board (Quarterly)

**Attendees:**
- IT Director
- Facilities Manager
- Admin Manager
- Digital Transformation Lead
- Power Platform Builder

**Agenda:**
- Review closed change tickets (approvals rate, rollbacks)
- Upcoming changes in next quarter
- Resource planning (builder time, training needs)
- Compliance/audit review

---

## 10. Security Baseline Checklist

- [ ] **Entra ID MFA enabled** for all staff + students
- [ ] **Conditional Access** blocks high-risk sign-ins
- [ ] **PIM enabled** for admin roles
- [ ] **DLP policies** block sensitive data leaving school systems
- [ ] **Sensitivity labels** applied to all lists/sites
- [ ] **Retention policies** set for request/record types
- [ ] **Defender for Office 365** enabled + auto-remediation ON
- [ ] **Defender for Endpoint** deployed on all school devices
- [ ] **Audit logging** enabled + exported monthly
- [ ] **SharePoint backup** policy in place (built-in daily)
- [ ] **Power Platform connector policy** blocks consumer connectors in PROD
- [ ] **Change control process** documented + enforced
- [ ] **Incident response plan** drafted (who to contact if breach?)
- [ ] **Data classification guide** published (what's public vs. confidential?)
- [ ] **Staff security training** annual (phishing, password hygiene)

---

## 11. Licensing Trap Summary

| Trap | How to Avoid |
|------|-----------|
| Accidental premium connector use | Audit Power Automate flows quarterly; block in PROD |
| Dataverse "trial" converting to billing | Use SharePoint Lists for MVP; only move to Dataverse if explicitly budgeted |
| Uncontrolled Power App proliferation (100-app limit) | Govern Power Platform; retire old apps; reuse components |
| Per-flow licensing costs spiraling | Count flows before building; consolidate where possible |
| Audit nightmare (no change control) | Enforce DEV/PROD separation; log all deployments |
| Data breach (no DLP/labels) | Implement Purview policies before first request submitted |
| Compliance violation (no retention) | Tag lists with retention labels; auto-archive after X years |

---

## 12. Budget Summary (Annual)

### M365 A5 Education (included)
- SharePoint, Teams, Outlook, Planner, Power Apps, Power Automate, Power BI Pro, Entra ID, Purview, Defender
- **Cost per user:** Typically covered by school's existing M365 contract
- **Nossal hub effort:** ~2 FTE (IT builder + IT admin for 12 months)

### Premium Add-Ons (Optional, v2+)
- Premium Power Automate connectors (if HTTP calls needed): ~$15-30/month per flow
- Dataverse (if complex relationships needed): ~$40-100/month
- Power BI Premium (if 50+ users viewing dashboards): ~$4,500/month (typically overkill for school)

### Recommendation for Nossal MVP
- **Year 1:** $0 premium licensing (A5 + builder labor only)
- **Year 2+:** Revisit based on scale (if > 200 concurrent users or complex integrations, consider Power BI Premium)

---

**Ownership:** IT Director + Digital Transformation Lead  
**Last Updated:** January 29, 2026  
**Status:** Ready for Sprint 1 Security & Governance Setup
