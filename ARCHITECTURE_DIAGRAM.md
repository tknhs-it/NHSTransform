# Nossal Digital Transformation — Architecture Overview Diagram

**Visual Summary:** How Compass + Microsoft 365 Work Together

---

## High-Level Architecture

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                          NOSSAL DIGITAL HUB (VIVA CONNECTIONS)               │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │ Dashboard Tiles (Service Catalogue-driven)                           │  │
│  │                                                                      │  │
│  │  [Compass]                    [Microsoft]           [Knowledge]      │  │
│  │  • Mark Attendance            • Report IT Issue      • How-To Guides │  │
│  │  • Learning Tasks             • Request Equipment   • Policies      │  │
│  │  • Student Wellbeing          • Request Access      • Troubleshoot  │  │
│  │  • School Events              • Maintenance Request                 │  │
│  │                               • Purchase Request                    │  │
│  │                                                                      │  │
│  │  Click → SSO to Compass       Click → Power App    Click → SharePoint│  │
│  │  (no re-login)                in Teams              Page            │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
                                    ↓↓↓
        ┌────────────────────────────┼────────────────────────────┐
        │                            │                            │
        ▼                            ▼                            ▼
   ┌─────────────┐          ┌──────────────────┐        ┌──────────────────┐
   │   COMPASS   │          │  MICROSOFT 365   │        │   KNOWLEDGE BASE │
   │  (Back-end) │          │   (Workflows)    │        │   (SharePoint)   │
   ├─────────────┤          ├──────────────────┤        ├──────────────────┤
   │ ✓ Rosters   │          │ Power Apps       │        │ How-to articles  │
   │ ✓ Attendance│   SSSO   │ Power Automate   │        │ Runbooks         │
   │ ✓ Learning  │ ←─────→  │ Planner          │        │ FAQs             │
   │ ✓ Wellbeing │ (Level1) │ Teams Channels   │        │ Policies         │
   │ ✓ Events    │          │ SharePoint Lists │        │                  │
   │ ✓ Finance   │          │ Approvals        │        │ Searchable via   │
   │             │          │                  │        │ Teams + Outlook  │
   │ SDS Export  │          │ Lists:           │        │                  │
   │ (Level 2)   │          │ • ITRequests     │        │ Sensitivity:     │
   │      ↓      │          │ • Maintenance    │        │ • Public         │
   │ → Teams     │          │ • AccessRequest  │        │ • Staff-Only     │
   │ class sync  │          │ • AssetLoans     │        │ • Admin-Only     │
   │             │          │ • PurchaseReq    │        │                  │
   │ CSV Export  │          │ • ServiceCatalog │        │                  │
   │ (Level 4)   │          │ • KnowledgeBase  │        │                  │
   │      ↓      │          │                  │        │                  │
   │ → Power BI  │          │ Dashboards:      │        │                  │
   │   (merged)  │          │ • IT metrics     │        │                  │
   │             │          │ • SLA tracking   │        │                  │
   └─────────────┘          │ • Request volumes│        └──────────────────┘
                            │ • Approvals      │
                            │ • Finance        │
                            └──────────────────┘
```

---

## Integration Levels (Timeline)

```
SPRINT 1                        SPRINT 2                        SPRINT 3+
┌─────────────────────┐    ┌────────────────────┐    ┌─────────────────┐
│  Level 1: SSO       │    │  Level 2: SDS      │    │ Level 3: iCal   │
│  (Week 2-3)         │    │  (Week 1-3)        │    │ (Anytime)       │
│  ✓ SAML ← Entra ID  │    │  ✓ Rosters → Teams │    │ ✓ Calendar feed │
│  ✓ Compass tiles    │    │  ✓ Class groups    │    │ ✓ In Outlook    │
│  ✓ No re-login      │    │  ✓ Auto-sync staff │    │ ✓ Read-only     │
│                     │    │  ✓ Auto-sync stud  │    │                 │
│ Release: Feb 27     │    │                    │    │ Level 4: Exports│
└─────────────────────┘    │ Release: Apr 10    │    │ (Sprint 2+)     │
                           │                    │    │ ✓ CSV monthly   │
                           └────────────────────┘    │ ✓ Power BI      │
                                                     │ ✓ Merged view   │
                                                     │                 │
                                                     │ Level 5: API    │
                                                     │ (2027, if TBD)  │
                                                     │ ✓ Real-time     │
                                                     │ ✓ Bi-directional│
                                                     └─────────────────┘
```

---

## Data Flows

### User Request → IT Issue (Example)

```
User in Teams
  │
  ├─ Clicks "Report IT Issue" tile in Nossal Hub
  │
  ├─ Opens IT Support Power App
  │
  ├─ Fills form: Category, Location, Description, Photo
  │
  ├─ Hits Submit
  │
  └─→ Power Automate Flow Triggered
       │
       ├─ Creates ITRequests list item
       │  - Auto-number: IT-2026-000123
       │  - Add optional CompassLink (if student/class-related)
       │
       ├─ Routes to correct Teams channel (by category)
       │  - Wi-Fi issue → IT Network channel
       │  - Printing → IT Printing channel
       │  - Etc.
       │
       ├─ Creates Planner task
       │  - Assigned to IT on-call
       │  - Due date = SLA (4h critical, 1 day standard)
       │
       ├─ Sends email to requester
       │  - Confirmation + ticket number
       │
       └─→ Dashboard updates real-time
            - Total open tickets +1
            - Avg triage time updates

Status updates → Requester gets email + Teams notification
Resolved → SLA calculated + dashboard shows "Met" or "Missed"
Closed → Task archived, ticket retained per policy (7 years)
```

---

## Service Catalogue Example

### Compass Tiles (Launch via SSO)

| Tile | Owner | LaunchType | Target | Integration Level |
|------|-------|-----------|--------|------------------|
| Mark Attendance | Compass Admin | DeepLink | https://compass/.../attendance | Level 1 (SSO) |
| Learning Tasks | Curriculum | DeepLink | https://compass/.../learning | Level 1 (SSO) |
| Wellbeing Profile | Student Services | DeepLink | https://compass/.../wellbeing | Level 1 (SSO) |
| Events Calendar | Compass Admin | DeepLink | https://compass/.../events | Level 1 (SSO) + Level 3 (iCal export) |

### Microsoft Tiles (Launch via Power App)

| Tile | Owner | LaunchType | Target | SLA |
|------|-------|-----------|--------|-----|
| Report IT Issue | IT | PowerApp | Power App form | 4h/1 day |
| Request Equipment | IT | PowerApp | Power App form | Same-day |
| Maintenance Request | Facilities | PowerApp | Power App form | 3 days |
| Request Access | IT | PowerApp | Power App form + approval | 1 day |
| Purchase Request | Finance | PowerApp | Power App form + approval | 2-5 days |

---

## Security Model

```
┌────────────────────────────────────────────────┐
│         ENTRA ID (Identity Foundation)          │
│  • User provisioning (staff + students)         │
│  • MFA (Authenticator app, phone sign-in)       │
│  • Conditional Access (risk-based policies)     │
│  • PIM (privileged role activation)             │
└────────────────────────────────────────────────┘
        ↓                              ↓
  ┌──────────────┐          ┌──────────────────┐
  │  COMPASS     │          │  MICROSOFT 365   │
  │  (via SAML)  │          │  (Native)        │
  ├──────────────┤          ├──────────────────┤
  │ • Audit logs │          │ • SharePoint     │
  │ • SSO logins │          │   sensitivity    │
  │              │          │   labels         │
  │              │          │ • DLP policies   │
  │              │          │   (block PII)    │
  │              │          │ • Retention      │
  │              │          │   policies       │
  └──────────────┘          │ • Purview audit  │
                            │ • Defender       │
                            │   (endpoint/    │
                            │    email)        │
                            └──────────────────┘
```

---

## Governance Model

### Environments

```
DEV (Sandbox)                    PROD (Production)
├─ Users: Builders              ├─ Users: Everyone
├─ Power Apps: Draft            ├─ Power Apps: Certified
├─ Flows: Test                  ├─ Flows: Live only
├─ Lists: Test data             ├─ Lists: Real data
├─ Change control: None         ├─ Change control: Mandatory
└─ Retention: 30 days           └─ Retention: Per policy
   (ephemeral)                      (7y for requests)

        Deploy via        ┌──────────────────────┐
        Change Control    │ Change Control Board  │
             ↓            │ (Monthly Review)      │
                          └──────────────────────┘
```

### Change Control Process

```
Builder finishes app in DEV
        ↓
Creates Change Control ticket
("Deploy [App] to Production")
        ↓
IT Lead reviews:
 • Tests app in DEV
 • Checks for premium connectors
 • Reviews data retention
 • Checks compliance
        ↓
Approves or requests changes
        ↓
On approval:
 • Schedule deployment window
 • Export from DEV
 • Import to PROD
 • Smoke test
 • Release
        ↓
Monitor for errors (2 weeks)
        ↓
If error: Rollback
If success: Done, document
```

---

## Team Roles (Sprint 1-4)

| Role | Sprint 1 | Sprint 2 | Sprint 3 | Sprint 4+ | Notes |
|------|----------|----------|----------|-----------|-------|
| **IT Director** | 0.2 | 0.2 | 0.1 | 0.1 | Governance, budget |
| **IT Lead / PM** | 0.8 | 0.8 | 0.6 | 0.4 | Coordination, oversight |
| **Compass Admin** | 0.3 | 0.3 | 0.1 | 0.1 | SSO setup, SDS, exports |
| **M365 Admin** | 0.3 | 0.3 | 0.1 | 0.1 | Entra ID, SDS, governance |
| **Power Platform Builder** | 0.5 | 1.0 | 1.0 | 0.5 | Apps, flows, dashboards |
| **Power BI Analyst** | 0.2 | 0.5 | 0.5 | 0.3 | Dashboards, metrics |
| **Facilities Lead** | 0.2 | 0.2 | 0.5 | 0.2 | Maintenance workflows |
| **Admin/Finance Lead** | 0.1 | 0.2 | 0.3 | 0.1 | Purchase approvals |

**Total Year 1:** ~4-5 FTE equivalent

---

## Cost Model (Year 1)

| Item | Cost | Notes |
|------|------|-------|
| **M365 A5 Education** | Included | Assumed in school contract |
| **Staff labor** | ~$250-350k | 4-5 FTE @ blended rates |
| **External training/consulting** | $0-20k | Optional |
| **Premium licenses (v2+)** | $0 (MVP) | Revisit if API integration needed |
| **Total Year 1** | **~$250-370k** | Mostly labor |

---

## Success Metrics (End of Sprint 4)

| Metric | Target | Owner |
|--------|--------|-------|
| **Adoption** | 80%+ staff/student use hub at least once | Comms |
| **Compass SSO** | 95%+ login success rate | IT |
| **Class Teams** | 100% of classes have Teams channels | M365 Admin |
| **IT Workflow** | 95%+ form submission success | IT Lead |
| **Avg triage time** | < 2h | IT |
| **Avg resolution time** | 1 day (standard) | IT |
| **SLA attainment** | 85%+ | Dashboard |
| **User NPS** | 7+/10 | Feedback surveys |
| **Zero data breaches** | ✓ | Security audit |
| **Compliance audited** | ✓ | Governance review |

---

## Next Steps (For You)

1. **Confirm Compass version & current auth** (SAML-ready? SDS supported?)
2. **Assign team leads** (Compass admin, M365 admin, IT lead, Power Platform builder)
3. **Schedule Sprint 1 kickoff** (Target: Week of Feb 3, 2026)
4. **Verify identity topology** (Single M365 tenant or separate admin/curriculum?)
5. **Gather requirements** (Which Compass data do you want in Power BI dashboards?)

---

**Status:** Ready for implementation  
**Last Updated:** January 29, 2026  
**Approval:** Pending IT Director sign-off
