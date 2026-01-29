# Nossal Digital Hub — Information Architecture

**Version:** 1.0  
**Date:** January 29, 2026  
**Context:** SharePoint Home Site + Viva Connections navigation structure

---

## 1. Top-Level Navigation (The Hub)

Users will navigate to the Nossal Digital Hub in Teams (Viva Connections app) or browse the SharePoint Home Site directly. The navigation is clean, discoverable, and extensible.

### Main Navigation Tiles & Pages

| Tile | Purpose | Audience | Owned By |
|------|---------|----------|----------|
| **Home** | Dashboard, news, quick links, announcements | Everyone | Comms |
| **Request Something** | Service catalogue grid with all available request types | Everyone | IT + Facilities |
| **IT & Devices** | IT services (support, asset loans, account access, Wi-Fi) | Staff & Students | IT |
| **Facilities & Maintenance** | Maintenance requests, repairs, access notes | Staff & Students | Facilities |
| **Teaching & Learning** | Curriculum resources, classroom tech, AV support | Teaching Staff | Curriculum |
| **Student Services** | Absence, approvals, pastoral requests | Students & Coordinators | Student Services |
| **Admin & HR** | Policies, forms, leave, HR links | Staff | Admin & HR |
| **Knowledge Base** | How-to guides, runbooks, FAQs, troubleshooting | Everyone | IT + support staff |
| **Dashboards** | Service health, request volumes, SLA attainment | IT, Facilities, Admin (role-based) | Data Team |

---

## 2. Service Catalogue (The Magic)

The **Service Catalogue** is a SharePoint List that drives:
1. What appears in the "Request Something" tile grid
2. Routing rules for Power Automate flows
3. SLA targets and ownership
4. Audience visibility (staff-only, students-only, etc.)

### Service Catalogue List Schema

| Field Name | Type | Purpose | Example |
|------------|------|---------|---------|
| **ServiceName** | Single Line Text | Display name | "Report an IT Issue" |
| **ServiceDescription** | Multi-line Text | What it does | "Report a problem with your device, Wi-Fi, email, or classroom tech" |
| **Category** | Choice | Top-level grouping | IT / Facilities / Admin / Student Services |
| **Subcategory** | Choice | Secondary grouping | (IT: Printing, Wi-Fi, Account, Classroom AV, Laptop) |
| **Audience** | Choice | Who can request | Staff / Students / Both |
| **Icon** | URL / Lookup | Tile icon (optional) | Link to icon asset |
| **FormAppLink** | URL | Power App form link | https://apps.powerapps.com/play/... |
| **OwnerTeam** | Lookup | Team responsible | IT / Facilities / Admin |
| **RoutingTags** | Multi-select | Flow routing metadata | Category + subcategory combo |
| **SLATarget** | Choice | Expected response time | 4h / 1 day / 3 days / 5 days |
| **IsActive** | Yes/No | Show in catalogue? | Yes |
| **SortOrder** | Number | Display priority | 1, 2, 3... |

### Sample Service Catalogue Entries

```
Service Name: Report an IT Issue
Category: IT
Subcategory: General Support
Audience: Both
Owner Team: IT
SLA Target: 4h (critical), 1 day (standard)
Routing Tags: "IT", "Support", "Triage"
Form Link: [IT Support Power App]

---

Service Name: Request Equipment Loan
Category: IT
Subcategory: Equipment
Audience: Staff
Owner Team: IT
SLA Target: Same-day
Routing Tags: "IT", "Asset", "Loan"
Form Link: [Asset Loan Power App]

---

Service Name: Report a Maintenance Issue
Category: Facilities
Subcategory: Repairs
Audience: Both
Owner Team: Facilities
SLA Target: 3 days
Routing Tags: "Facilities", "Maintenance", "Repair"
Form Link: [Maintenance Request Power App]

---

Service Name: Request Access or Permissions
Category: IT
Subcategory: Access
Audience: Staff
Owner Team: IT
SLA Target: 1 day
Routing Tags: "IT", "Access", "Approval"
Form Link: [Access Request Power App]

---

Service Name: Absence / Leave Request
Category: Student Services
Subcategory: Absence
Audience: Students
Owner Team: Student Services
SLA Target: Same-day
Routing Tags: "Student", "Absence"
Form Link: [Absence Power App]
```

---

## 3. SharePoint Hub Navigation

The SharePoint Home Site acts as a **hub** that anchors the Viva Connections experience. Its pages include:

### Page Architecture

#### Home Page
- Welcome message
- Quick-link tiles (same as Viva nav)
- Latest news/announcements
- Service status widget

#### Request Something Page
- **Grid/Gallery view** of active services from the Service Catalogue list
- Filter by Category (IT, Facilities, Admin, etc.)
- Each tile links to the corresponding Power App form

#### Knowledge Base Page
- Searchable library of articles
- Categories: Getting Started, How-To, Troubleshooting, Policy
- Tagging for discoverability
- Staff-authored content (runbooks, FAQs)

#### Category Pages (IT, Facilities, etc.)
- **IT & Devices**: Sub-services, asset loan info, Wi-Fi setup, account creation, classroom AV support
- **Facilities & Maintenance**: Submit requests, building maps, access hours, emergency contacts
- **Teaching & Learning**: Curriculum links, tech resources, classroom bookings, AV support
- **Student Services**: Absence form, absence policy, support contacts
- **Admin & HR**: Staff policy documents, links to leave portal, HR contacts

#### Service Health Dashboard Page
- Tile showing:
  - Today's request count (by category)
  - Avg. response time (by category)
  - Open ticket count
  - SLA attainment % (e.g., "85% of requests answered within SLA")

---

## 4. Knowledge Base Structure

The **Knowledge Base** is a SharePoint List + Library of articles. Content is:
- Written by subject-matter experts (IT techs, facilities, admin staff)
- Published/draft workflow (simple approval)
- Searchable and discoverable in Teams search + Viva search
- Categorized and tagged

### Knowledge Base List Schema

| Field | Type | Purpose |
|-------|------|---------|
| **ArticleTitle** | Single Line Text | "How to Reset Your Password" |
| **ArticleBody** | Rich Text / HTML | Step-by-step instructions |
| **Category** | Choice | Getting Started / How-To / Troubleshooting / Policy |
| **Tags** | Multi-select | "password", "account", "IT", "self-service" |
| **Author** | Person | Who wrote it |
| **LastUpdated** | Date | When was it last reviewed |
| **Status** | Choice | Draft / Published / Archived |
| **Audience** | Choice | Staff / Students / Both |
| **SortOrder** | Number | Display priority |
| **RelatedServices** | Lookup | Link to related Service Catalogue entries |

### Knowledge Base Categories & Sample Articles

**Getting Started**
- "First Day: Setting Up Your Device"
- "Teams: Your Home for Work"
- "Where to Find Help"

**How-To**
- "How to Reset Your Password"
- "How to Join a Teams Channel"
- "How to Request an Equipment Loan"
- "How to Report a Maintenance Issue"
- "How to Access the Learning Hub"

**Troubleshooting**
- "Wi-Fi Won't Connect — Try This"
- "Email Is Slow — Troubleshooting Steps"
- "My Device Won't Start — What to Do"
- "Can't Access a File in SharePoint"

**Policy**
- "Device Acceptable Use Policy"
- "Data Classification & Sensitive Information"
- "Password Policy & Security Standards"
- "Absence & Leave Procedures"

---

## 5. Audience & Permissions Model

### Viva Connections App

- **Available to:** Everyone (staff & students)
- **Show/Hide:** Service tiles dynamically based on user's audience group
  - E.g., "Request Access" only shows to staff
  - "Absence Request" only shows to students

### SharePoint Home Site

- **Default audience:** Everyone (site browseable)
- **Content sensitivity labels:**
  - "Public" pages (home, request catalogue, basic KB)
  - "Staff-Only" pages (HR policies, sensitive KB articles)
  - "Admin-Only" pages (dashboard, admin links)

### Knowledge Base Article Visibility

- Uses Audience field + view filters
- E.g., "Device Acceptable Use Policy" shows in Staff view, hidden in Student view

---

## 6. Search & Discoverability

### Microsoft Search Integration (included in M365)

**What's searchable:**
- SharePoint pages + libraries
- Teams conversations & files
- List items (with metadata)
- Knowledge Base articles

**Search scopes (for power users):**
- "Find a Service" → searches ServiceCatalogue + KB articles
- "Find a Runbook" → filters KB by Category = "How-To"
- "Find an IT Solution" → filters by Category = "IT"

**Metadata that makes search work:**
- Article tags (in Knowledge Base)
- Service category + subcategory (in Service Catalogue)
- List item titles + descriptions

---

## 7. Workflow Routing Rules (How Data Flows)

The **RoutingTags** in the Service Catalogue drive Power Automate flow logic:

```
IF user submits "IT Support Request" (routing tag: "IT+Support")
  → Route to IT Triage Teams channel
  → Create Planner task in "IT Requests" bucket
  → Assign to IT on-call
  → Set SLA reminder for 4h

IF user submits "Maintenance Request" (routing tag: "Facilities+Maintenance")
  → Route to Facilities Triage Teams channel
  → Create Planner task in "Maintenance" bucket
  → Attach photo from request
  → Check if "Urgent" → add approval gate

IF user submits "Access Request" (routing tag: "IT+Access")
  → Create task in "Access Approvals" queue
  → Route to user's manager (via Entra ID manager lookup)
  → On approval → assign to IT for action
  → Audit trail logged to list
```

---

## 8. MVP Content Inventory

For **Sprint 1**, prioritize:

### SharePoint Pages to Build
- [ ] Home Page (hero, quick links, news)
- [ ] Request Something (service catalogue grid)
- [ ] Knowledge Base (top 20 articles: passwords, Wi-Fi, account setup, devices, classroom AV)
- [ ] IT & Devices (sub-page with IT services)
- [ ] Facilities (sub-page with maintenance info)

### SharePoint Lists to Create
- [ ] ServiceCatalogue (seed with 5 core services)
- [ ] KnowledgeBase (seed with 20 articles)

### Viva Connections Configuration
- [ ] Import tiles from SharePoint pages
- [ ] Configure Audience filters on tiles
- [ ] Enable search in Viva

---

## 9. Governance & Maintenance

| Responsibility | Owner | Frequency |
|---|---|---|
| Update Service Catalogue (add/remove services) | IT + Facilities lead | Quarterly review |
| Update Knowledge Base articles | Subject-matter experts | Monthly (evergreen) |
| Review/archive old KB articles | IT | Quarterly |
| Update home page news | Comms team | Weekly |
| Monitor search effectiveness | Data team | Monthly analytics review |
| Update Service SLAs | IT + Facilities lead | Annual |

---

## 10. Success Metrics

| Metric | Target | Owner |
|--------|--------|-------|
| KB article discovery rate (% finding answers without submitting request) | 40% | IT |
| Service catalogue findability (% finding the right request form) | 95% | IT |
| Viva Connections adoption (% of staff/students using it) | 80%+ | Comms |
| Average request triage time | ↓ from 1 day to 2h | IT |
| Customer satisfaction (NPS) | 7+/10 | Data team |

---

**Ownership:** Digital Transformation Team  
**Last Updated:** January 29, 2026  
**Status:** Ready for Sprint 1 Implementation
