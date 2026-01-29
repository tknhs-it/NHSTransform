# Nossal Digital Transformation — Compass Integration Summary

**Date:** January 29, 2026  
**Version:** 1.0 (Compass-inclusive architecture)

---

## What Changed: From "Microsoft-Only" to "Compass + Microsoft Partnership"

### Original Plan (Jan 29 morning)
Build everything in Microsoft 365:
- Service catalogue, workflows, dashboards — all in M365
- Risk: Compass remains siloed, no single front door

### Updated Plan (Jan 29 afternoon)
**Compass = system-of-record. Microsoft 365 = unified front door + workflow engine.**

- Compass stays authoritative for: rosters, attendance, learning tasks, wellbeing, finance
- Microsoft 365 becomes: portal (Viva Connections), workflows (IT tickets, access requests), collaboration (Teams)
- Integration: Single sign-on (SSO), class roster sync (SDS), data dashboards (CSV exports → Power BI)

---

## The 5 Integration Levels (Progressive, A5-First)

| Level | What | Timeline | Effort | Why |
|-------|------|----------|--------|-----|
| **1. SAML/SSO** | Users log into Teams once, click Compass tile, no re-login | Sprint 1 (week 2-3) | 2-3 days | Remove friction, improve adoption |
| **2. SDS** | Compass rosters → Teams auto-create class groups + sync students | Sprint 2 (week 1-3) | 3-5 days | Teachers have Teams channels instantly |
| **3. iCal** | Compass calendar events → Outlook calendars (read-only) | Anytime (1 day setup) | 1 day | Visibility, no double-booking |
| **4. Data Exports** | Compass CSVs → SharePoint → Power BI dashboards (merged view) | Sprint 2+ ongoing | 3-4 days setup + 30 min/month | Cross-system analytics (attendance vs. IT issues) |
| **5. REST API** | Real-time Compass ↔ M365 syncing | 2027+ (if demand) | 2-4 weeks + AIP approval | Advanced workflows, only if justified |

**MVP approach:** Levels 1-4 only. Level 5 deferred (premium licensing required, higher risk).

---

## What You Get (Concrete Outcomes)

### For Staff
- One login to Teams, then seamless access to:
  - Compass (attendance, learning, wellbeing)
  - IT request forms
  - Class Teams channels (auto-created from Compass rosters)
  - Timetable in Outlook calendar
  - Unified Knowledge Base

### For IT
- No duplication of student/staff data
- Clear ownership: Compass = truth, M365 = workflow layer
- Audit trail: Compass SSO logins, IT requests tracked in M365

### For Leadership
- Dashboards showing "Compass metrics + M365 workflow metrics" side-by-side
- Example: "When attendance dropped week of [date], IT issues decreased (students not here = no tech problems)"

---

## Key Architecture Changes

### Service Catalogue (New Fields)

```
ServiceName         (e.g., "Mark Attendance" or "Report IT Issue")
SystemOfRecord      (Compass vs. Microsoft365)
LaunchType          (DeepLink vs. PowerApp vs. TeamsChannel vs. SharePointPage)
LaunchTarget        (URL to Compass page, Power App, Teams channel, etc.)
```

**Example:**
- "Mark Attendance" → SystemOfRecord: Compass → LaunchType: DeepLink → Opens Compass
- "Report IT Issue" → SystemOfRecord: Microsoft365 → LaunchType: PowerApp → Opens IT Support form

### Request Lists (Compass Cross-References)

All Microsoft request lists now have optional fields:
- `CompassPersonID` — Related student/staff in Compass (e.g., "S12345")
- `CompassEventID` — Related event/class/excursion (e.g., "TRIP-2026-01")
- `CompassLink` — Hyperlink to Compass record (audit trail, one-click lookup)

**Benefit:** IT tech fixing a device issue can click the CompassLink and see the student's timetable/attendance to understand context.

---

## Sprint-by-Sprint Integration

### Sprint 1: Portal + Compass SSO (5 weeks)

**New deliverable:** SAML/SSO setup
- [ ] Configure Compass SAML ← Entra ID
- [ ] Test with pilot group (5-10 staff)
- [ ] Update Service Catalogue with Compass tiles
- [ ] Add "Mark Attendance", "Learning Tasks", "Wellbeing", "Events" tiles to Nossal Hub

**Outcome:** Staff click Nossal Hub → see both Compass + Microsoft tiles → click Compass tile → land in Compass (no re-login)

### Sprint 2: IT Workflow + Compass SDS (6 weeks)

**New deliverable:** School Data Sync (rosters → Teams)
- [ ] Compass admin exports rosters (School, Staff, Student, Class, Enrollment CSVs)
- [ ] M365 admin configures SDS sync
- [ ] Test with 2-3 classes (verify Teams auto-created, staff/students added)
- [ ] Full rollout (all classes get Teams channels)

**Also in Sprint 2:**
- IT Support workflow (Power App + Planner + triage flow)
- Compass data exports setup (monthly: attendance, people, events → SharePoint library)
- Power BI dashboard mockup (M365 IT metrics + Compass data)

**Outcome:** Teachers have class Teams channels. IT workflow live. Dashboards show cross-system metrics.

### Sprint 3: Maintenance + Access Workflows (6 weeks)

**No new Compass integration, but:**
- Maintenance requests can link to Compass event (e.g., "building closed for [event]")
- Access requests can link to Compass person (audit trail)

### Sprint 4+: Optional API Integration (2027+)

**If justified by demand:**
- Real-time sync between Compass + M365
- Example: When absence approved in M365, flag in Compass attendance
- **Requires:** AIP (Application Integration Partner) approval from Compass, custom dev

---

## Decision Points (For You)

### 1. Identity: Single Tenant or Multi-Tenant?

**Assumption:** Nossal uses one Microsoft 365 tenant (admin + curriculum together)

**If separate:** SSO becomes more complex. Tell us, we'll adjust.

### 2. SDS Timing: Automate Now or Later?

**MVP (Sprint 2):** Manual CSV upload monthly (Compass admin exports, M365 admin uploads to SDS)

**Later (v1.1):** Automate via SFTP feed (Compass → SFTP server → SDS auto-pulls weekly)

**Recommend:** Start manual, automate in Q3 2026 if admin burden is high.

### 3. iCal Adoption: Outlook or Teams?

**Option A:** Staff add iCal in Outlook Web (self-serve)

**Option B:** M365 admin adds to organization's shared calendar (staff see it automatically)

**Recommend:** Option B (less friction, staff don't need to know about iCal)

### 4. Power BI Dashboards: Compass Data Only or Merged?

**MVP:** Power BI shows M365 metrics (IT request volumes, SLA, etc.)

**v1.1:** Add Compass CSV exports (attendance, people) → merged dashboards

**Recommend:** MVP is fine. Revisit in Q2 2026 if leadership wants cross-system analytics.

---

## Security & Compliance

### SAML/SSO
- ✅ Entra ID authenticates Compass logins
- ✅ Audit logs all SSO attempts (success + failures)
- ✅ Compass can set conditional access (e.g., MFA required from unmanaged devices)

### SDS
- ✅ Read-only roster sync (no sensitive data written back)
- ✅ Encrypted in transit (HTTPS)
- ✅ SDS sync logs monitored
- ✅ Students added to class teams (fine-grained permissions)

### Data Exports
- ✅ CSVs labeled "Staff-Only" in SharePoint
- ✅ DLP prevents external sharing of attendance data
- ✅ Retention policy (delete exports after 12 months)

### API (Level 5, Future)
- ✅ Requires AIP approval (vetting by Compass)
- ✅ Rate-limited to prevent abuse
- ✅ All calls logged + auditable
- ✅ Only available to approved integrator

---

## File Structure (In Repo)

```
01_Architecture/
├─ 01_Target_Architecture.md          (Updated: Compass as system-of-record)
├─ 02_Information_Architecture.md      (Updated: Compass-aware Service Catalogue)
└─ 03_Compass_Integration_Strategy.md  (NEW: 5 levels, detailed specs, troubleshooting)

02_Workflows/
└─ 01_Workflow_Specifications.md       (Updated: Added CompassPersonID, CompassEventID, CompassLink fields)

03_Implementation_Plan/
└─ Sprint_Execution_Plan.md            (Updated: Sprint 1 includes Compass SSO, Sprint 2 includes SDS)

00_Project_Governance/
└─ Governance_and_Licensing_Strategy.md (No changes needed; applies to both Compass + M365)
```

---

## Next Steps (For You)

1. **Verify assumptions:**
   - Single M365 tenant or separate admin/curriculum?
   - Compass version? (Planning assumes recent version with SAML + SDS support)
   - Current Compass authentication (AD, SAML, other)?

2. **Identify champions:**
   - Compass admin (owns SSO setup, SDS config, monthly exports)
   - M365 admin (owns Entra ID app registration, SDS profile, Power BI)
   - IT Lead (owns Nossal Hub, Power App workflows)

3. **Schedule Sprint 1 kickoff:**
   - Compass SSO pilot starts Week 2 of Sprint 1
   - Service Catalogue + Viva Connections setup runs in parallel
   - Portal goes live Feb 27, 2026

4. **Plan SDS rollout (Sprint 2):**
   - Week 1: Data validation (rosters, enrollments, email addresses)
   - Week 2: SDS configuration + small test
   - Week 3: Full rollout (staggered: Grade 7-9, Grade 10-12, Staff)

---

## Questions?

Refer to:
- [Target Architecture](01_Architecture/01_Target_Architecture.md) for high-level overview
- [Compass Integration Strategy](01_Architecture/03_Compass_Integration_Strategy.md) for detailed Level 1-5 specs
- [Information Architecture](01_Architecture/02_Information_Architecture.md) for Service Catalogue + Viva design
- [Sprint Plan](03_Implementation_Plan/Sprint_Execution_Plan.md) for timeline + deliverables

---

**Status:** Ready for Sprint 1 kickoff  
**Last Updated:** January 29, 2026  
**Owner:** Digital Transformation Team
