# Compass Integration Strategy — Implementation Roadmap

**Version:** 1.0  
**Date:** January 29, 2026  
**Scope:** How to integrate Compass (system-of-record) with Microsoft 365 (front door + workflows)

---

## Overview: The Partnership Model

**Compass** = authoritative source for school operations (rosters, attendance, learning tasks, wellbeing, finance)

**Microsoft 365** = unified front door (Viva Connections), workflow engine (Power Apps/Automate), collaboration platform (Teams)

**Integration goal:** Single sign-on + seamless navigation, no data duplication, clear ownership boundaries.

---

## Level 1: SAML/SSO Single Sign-On (Sprint 1 — 2-3 Days)

### What It Does

User logs into Microsoft Teams once. They click a Compass tile in Nossal Hub. Compass recognizes them via SAML SSO from Entra ID—no re-login required.

### Why It Matters

- **Friction removed:** Users don't get "access denied" messages when switching between tools
- **User adoption:** People use Compass more if it's one click from Teams
- **Audit clean:** Entra ID logs all Compass logins; DLP/Defender can monitor Compass access via identity

### Implementation Steps

#### 1. Compass Configuration (Compass admin, ~1 day)

1. **Enable SAML in Compass**
   - Compass admin logs into Compass settings
   - Navigate to: Integration → SAML Configuration
   - Enable SAML SSO

2. **Create Entra ID app registration**
   - M365 admin creates "Compass" enterprise app in Entra ID
   - Generate SAML signing certificate (2048-bit X.509)
   - Configure Assertion Consumer Service (ACS) URL: `https://compass.nossal.vic.edu.au/saml/acs`
   - Configure Single Sign-On (SSO) URL: `https://compass.nossal.vic.edu.au/saml/sso`
   - Entity ID: `https://compass.nossal.vic.edu.au/saml/metadata`

3. **Upload certificate to Compass**
   - Download SAML metadata from Entra ID
   - Upload certificate to Compass SSO config
   - Configure name mappings:
     - `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` → Compass User ID
     - `http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname` → Compass email

4. **Test SSO with pilot group**
   - Ask 5-10 staff members to:
     - Log into Teams
     - Click "Learning Tasks" tile in Nossal Hub
     - Verify they land in Compass without re-authenticating
   - Collect feedback: "Did it work? Did you notice you didn't log in again?"

5. **Troubleshoot common issues**
   - **"Access Denied"** — Compass user account not provisioned in Entra ID. Solution: Sync staff/student accounts from directory.
   - **"SAML assertion invalid"** — Clock skew or certificate mismatch. Solution: Verify clocks synchronized, re-upload certificate.
   - **"Email doesn't match"** — Entra ID email ≠ Compass email. Solution: Normalize email addresses (lowercase, domain match).

#### 2. SharePoint Hub Configuration (M365 admin, ~0.5 day)

1. **Update Service Catalogue list**
   - Add Compass tiles with LaunchType = "DeepLink"
   - Add Compass integration level = "Level1_SSO"
   - LaunchTarget URL: `https://compass.nossal.vic.edu.au/[path]`

2. **Update Viva Connections dashboard**
   - Tile 1: "Mark Attendance" → https://compass.nossal.vic.edu.au/school_attendance/manage
   - Tile 2: "Learning Tasks" → https://compass.nossal.vic.edu.au/learning/tasks
   - Tile 3: "Wellbeing" → https://compass.nossal.vic.edu.au/student/wellbeing
   - Tile 4: "Events" → https://compass.nossal.vic.edu.au/events/calendar

3. **Test Viva Connections**
   - Tiles appear in Teams Nossal Hub
   - Click a tile → Opens Compass (SSO, no login)
   - Verify in Teams on mobile & desktop

#### 3. Security & Audit (IT admin, ~0.5 day)

1. **Enable Compass audit logging**
   - Compass admin enables audit logs (login events, page access, data changes)
   - Export logs weekly for compliance review

2. **Set Conditional Access policy (optional but recommended)**
   - Users accessing Compass from unmanaged devices must use MFA
   - Policy: App: Compass | Condition: Device unmanaged | Action: Require MFA

3. **DLP: Monitor Compass access**
   - Tag Compass access as "Sensitive System"
   - Alert if staff downloads attendance lists to personal device

### Success Criteria

- [ ] All staff can access Compass via Teams Nossal Hub without re-logging in
- [ ] Audit logs show SAML login attempts (success + failures)
- [ ] Zero "access denied" complaints (or < 2, quickly resolved)
- [ ] Pilot group NPS: 8+/10 ("It's seamless")

### Timeline

**Sprint 1, Week 2-3:** Compass admin + M365 admin working in parallel  
**Sprint 1, Week 4:** Testing + feedback loops  
**Sprint 1, Week 5:** Go-live

---

## Level 2: Rosters → Teams via SDS (Sprint 2 — 3-5 Days)

### What It Does

Compass class/roster data automatically syncs to Microsoft Teams, creating class groups & auto-adding staff + students.

### Why It Matters

- **Teaching structure:** Teachers have Teams channels for each class without manual setup
- **Learning continuity:** Assignments (if using Teams Assignments) show in Teams for each class
- **No duplication:** Roster = truth in Compass; SDS is a read-only sync

### Compass School Data Sync (SDS) Overview

**SDS** = Microsoft tool that ingests school data (rosters, users, classes) and auto-creates Groups, Teams, and channel structures.

**Compass + SDS:** Compass lists SDS v2.1 as a supported integration (check Compass docs: "Integrations Outside AIP").

### Implementation Steps

#### 1. Preparation (Compass admin + M365 admin, ~1 day)

1. **Compass: Generate SDS export file**
   - Compass admin navigates to: Integration → SDS
   - Configure export format:
     - School.csv (school info)
     - Staff.csv (teachers, admins)
     - Student.csv (enrolled students)
     - Class.csv (classes/sections)
     - Enrollment.csv (student-to-class mappings)
   - Test export by downloading one file manually

2. **M365: Prepare SDS connector**
   - M365 admin logs into SDS admin center
   - Create new sync profile: "Compass Rosters"
   - Choose upload method:
     - Option A: CSV upload (manual, weekly) — easier for MVP
     - Option B: SFTP auto-sync (automated) — requires Compass SFTP access setup
   - For MVP, use **Option A (manual CSV upload)**

3. **Data validation**
   - Verify no missing students/staff
   - Check email addresses match across Compass + Entra ID
   - Ensure class codes/IDs are consistent

#### 2. SDS Configuration (M365 admin, ~1-2 days)

1. **Upload Compass CSV files to SDS**
   - Download Compass exports (School, Staff, Student, Class, Enrollment CSVs)
   - Upload to SDS sync profile
   - SDS validates data (checks for duplicates, missing fields, etc.)

2. **Map Compass fields to SDS schema**
   - School ID → SchoolID
   - Staff ID → SourcedId / Email
   - Student ID → SourcedId / Email
   - Class code → SourcedId / Name
   - Enrollment (Student + Class) → Enrollment mappings

3. **Configure Team/Group creation rules**
   - For each class:
     - Auto-create Microsoft 365 Group
     - Auto-create Teams channel
     - Add staff (teacher) as owner
     - Add students as members
   - Example: Class "10A Maths" → Group "10A Maths (2026)" → Teams auto-created with all enrolled students

4. **Test with small group**
   - Run SDS sync on 2-3 classes
   - Verify in Teams:
     - Teams exist
     - Staff are owners
     - Students are members
     - Channel is populated

5. **Troubleshoot common issues**
   - **"User not found"** — Entra ID doesn't have staff/student account. Solution: Bulk create in Entra ID or sync via AAD Connect.
   - **"Duplicate group"** — SDS tried to create a group that already exists. Solution: Delete old group, re-run sync.
   - **"Class has no teacher"** — Enrollment data missing teacher. Solution: Update Compass enrollment, re-export, re-upload to SDS.

#### 3. Rollout (M365 admin, ~1-2 days)

1. **Full Compass → SDS sync**
   - Run SDS for all classes in the school
   - Monitor SDS job status (typically 30 min - 2h depending on school size)

2. **Verify Teams channel creation**
   - Navigate to each class team
   - Spot-check roster (staff, students present)
   - Test: Staff can post in channel, students can see messages

3. **Staff communication**
   - Email staff: "Your class Teams channels are now live! Click [link] to access."
   - Highlight: "All students are already added. You can start using Teams for your class immediately."

4. **Student onboarding**
   - Post announcement in each Teams: "Welcome to [Class]! This is where we'll share materials and communicate."
   - Add class description + pinned resources

### Ongoing: Keeping SDS in Sync (Maintenance)

**Monthly ritual:**
- Compass admin exports roster CSVs
- M365 admin uploads to SDS
- SDS runs sync (usually overnight)
- Check for sync errors/issues
- Communicate changes to staff (new student added, removed, etc.)

**Or (future):** Automate via SFTP feed (Compass → SFTP → SDS auto-pulls weekly)

### Limitations & Caveats

- **SDS is roster sync only** (not real-time). If a student drops a class on Friday, they'll still see the Teams channel until the next SDS sync (typically weekly/monthly).
- **Teams ownership:** Teachers are group owners, but don't have "admin" rights in Microsoft 365 (can't delete the group, etc.). This is intentional (prevents chaos).
- **Calendar**: SDS can also sync class calendars (from Compass timetable), but this is separate step (Level 3).

### Success Criteria

- [ ] All classes have Teams channels
- [ ] All staff are owners of their class teams
- [ ] All enrolled students are members of their class teams
- [ ] Zero duplicate groups
- [ ] SDS sync completes without errors
- [ ] Staff can open Teams and see their classes immediately

### Timeline

**Sprint 2, Week 1:** Compass + M365 admin planning, data validation  
**Sprint 2, Week 2:** SDS configuration + small test  
**Sprint 2, Week 3:** Full rollout  
**Sprint 2, Week 4:** Monitoring + troubleshooting

---

## Level 3: Calendar Surfaces (Ongoing — Simple, 1 Day Setup)

### What It Does

Compass iCal feeds (timetable, events) appear in Outlook/Teams calendars (read-only).

### Why It Matters

- **Visibility:** Staff see school events in their Teams calendar
- **No double-booking:** Can see when class is scheduled before booking a meeting
- **Non-disruptive:** iCal is one-way (M365 doesn't update Compass), so no sync chaos

### Important Caveat

**iCal-based timetables often show as "Free" in Outlook** (depending on how Compass exports the iCal). This means:
- Staff CAN see the timetable event
- But Outlook doesn't block meeting invites during that time
- **Solution:** Make it clear to staff: "Timetable is for reference only. Don't schedule meetings during class times."

### Implementation

#### 1. Compass Configuration (Compass admin, ~0.5 day)

1. **Generate iCal feeds**
   - Compass admin creates iCal feeds for:
     - School events (assemblies, excursions, holidays)
     - Staff timetable (per staff member)
     - Department calendars (optional)
   - Compass generates unique iCal URLs (e.g., `https://compass.nossal.vic.edu.au/ical/events/[token]`)

2. **Share iCal URLs securely**
   - Send URLs to M365 admin (or IT lead who distributes to staff)
   - URLs should include authentication token (so only staff with access see their own data)

#### 2. M365 Configuration (M365 admin or staff, ~0.5 day)

1. **Add iCal to Outlook calendars**
   - Option A (staff self-serve):
     - Staff log into Outlook Web
     - Settings → Calendars → Add Calendar → Subscribe from web
     - Paste Compass iCal URL
     - Done; iCal events appear in Outlook

   - Option B (M365 admin bulk deploy):
     - Admin creates shared calendar in M365
     - Admin adds all staff as members
     - Admin subscribes shared calendar to Compass iCal
     - Staff auto-see school timetable in Teams calendar

2. **Test**
   - Open Outlook → check calendar
   - Look for Compass events (school events, timetable)
   - Verify they're read-only (can't edit them)

### Success Criteria

- [ ] School events visible in Outlook calendars
- [ ] Staff timetable visible (if configured)
- [ ] Events are read-only (staff don't accidentally modify)
- [ ] iCal feed updates when Compass changes

### Timeline

**Anytime after Sprint 1:** Very low effort  
**Expected:** 1 day to implement

---

## Level 4: Data Exports for Dashboards (Sprint 2+ — Ongoing)

### What It Does

Compass exports CSV/PDF reports (attendance, people, events). M365 imports into Power BI dashboards for unified operational insights.

### Why It Matters

- **Cross-system visibility:** Dashboard shows "Attendance trends vs. IT request volumes" (can we correlate?)
- **No API calls:** Uses simple CSV export/import (no premium connectors needed)
- **Manual at first, automate later:** Start with monthly exports, automate via SFTP/automation if needed

### Implementation

#### 1. Compass Exports (Compass admin, ~0.5 day setup + 15 min/month ongoing)

1. **Configure Compass exports**
   - Attendance summary (daily/weekly)
   - People directory (staff + students)
   - Events calendar
   - Finance (if applicable)
   - Each export: CSV format, sortable columns

2. **Schedule exports**
   - Monthly: First Monday of each month, 8am
   - Compass auto-generates CSV, places in shared folder or emails admin

3. **Document export process**
   - What fields are included?
   - Any privacy considerations (PII in exports)?
   - Who has access to exports?

#### 2. SharePoint Library (M365 admin, ~1 day setup + 15 min/month)

1. **Create "Compass Data Exports" library**
   - SharePoint site: Nossal Digital Hub
   - Library name: "Compass Data Exports"
   - Sensitivity: Staff-Only (contains attendance, people data)

2. **Upload Compass CSVs monthly**
   - Attendance-2026-01.csv
   - Attendance-2026-02.csv
   - ...
   - People-Directory-2026-01.csv
   - Events-2026-01.csv

3. **Organize by folder**
   - /Attendance/2026/
   - /Directory/2026/
   - /Events/2026/

#### 3. Power BI Dashboards (Power BI analyst, ~2-3 days)

1. **Create Power BI report**
   - Data source: SharePoint "Compass Data Exports" library
   - Connect to CSV files

2. **Design dashboards**
   - **Attendance Dashboard:**
     - Weekly attendance rate by year/grade
     - Trend (is attendance improving?)
     - Compare to baseline
   
   - **Engagement Dashboard:**
     - Students with low attendance (flag for follow-up)
     - Students missing learning tasks
   
   - **Operations Dashboard (M365 + Compass merged):**
     - IT request volumes vs. attendance (any correlation when systems down?)
     - Maintenance requests by location
     - Purchase requests by department

3. **Publish to SharePoint**
   - Power BI dashboards embedded in SharePoint "Dashboards" page
   - Refresh daily (uses latest Compass export if uploaded)
   - Row-level security: Staff see only their dept/class data

### Ongoing: Monthly Updates

- [ ] Compass admin exports CSVs (1st of month)
- [ ] M365 admin uploads to SharePoint library
- [ ] Power BI auto-refreshes (or manual refresh if data source changed)
- [ ] Leadership reviews dashboards (in Change Control Board meetings)

### Success Criteria

- [ ] Power BI dashboards load without errors
- [ ] Data is current (within 1 month)
- [ ] Dashboards provide actionable insights (e.g., "Attendance dropped week of [date]")
- [ ] Sensitive data is labeled "Staff-Only"

### Timeline

**Sprint 2+:** Can run in parallel with IT workflows  
**Expected:** 3-4 days initial setup, then 30 min/month

---

## Level 5: REST API Integration (Later — Deliberate)

### What It Does

Direct REST API calls from Power Automate to Compass for real-time data access.

### Why It Matters (Later)

- **Real-time integration:** Create an M365 purchase request → log entry in Compass for audit
- **Bi-directional:** M365 can read/write Compass data (with governance)
- **Advanced workflows:** E.g., "When absence is approved in M365, update Compass attendance flag"

### Why NOT to do this in MVP

- Requires **AIP (Application Integration Partner) approval** from Compass (not trivial)
- Requires premium Power Automate connector OR custom HTTP logic
- Higher risk (bugs can corrupt Compass data)
- **Recommendation:** Start with Level 1-4, revisit in 2027 if demand justifies

### When to Consider Level 5

- "We want real-time sync, not monthly exports"
- "We're building 10+ workflows that need Compass data"
- "Budget allows for custom development"

### Process (If Needed Later)

1. **Contact Compass support**
   - Request AIP application
   - Compass reviews use case + security requirements
   - Approval typically takes 2-4 weeks

2. **Develop custom connector**
   - M365 developer builds Power Automate connector for Compass API
   - Authenticates via OAuth or API key
   - Implements specific workflows (e.g., log audit entries)

3. **Test in DEV environment**
   - Compass provides sandbox API endpoint
   - Test connector with sample data
   - Verify no data corruption

4. **Deploy to PROD**
   - Move to production Compass API
   - Monitor flow logs for errors
   - Update documentation

---

## Integration Roadmap Summary

| Level | Effort | Timeline | Outcome | Notes |
|-------|--------|----------|---------|-------|
| **Level 1: SSO** | Low (2-3 days) | Sprint 1, Week 2-3 | Single login + Compass tiles in Teams | Do this first |
| **Level 2: SDS** | Medium (3-5 days) | Sprint 2, Week 1-3 | Class Teams auto-created + rosters synced | High-impact for teaching |
| **Level 3: iCal** | Low (1 day) | Anytime after Sprint 1 | Calendar events visible in Outlook | Nice-to-have |
| **Level 4: Exports** | Medium (3-4 days) | Sprint 2+ ongoing | Power BI dashboards with Compass + M365 data | Ongoing effort (30 min/mo) |
| **Level 5: API** | High (2-4 weeks) | 2027+ | Real-time two-way integration | Only if justified by demand |

---

## Security & Compliance Checklist

- [ ] **SSO (Level 1):** SAML certificate rotated annually, audit logs enabled
- [ ] **SDS (Level 2):** Roster data encrypted in transit, SDS sync logs monitored
- [ ] **iCal (Level 3):** iCal URLs include authentication tokens, not publicly accessible
- [ ] **Exports (Level 4):** CSV exports labeled "Staff-Only", retention policy enforced
- [ ] **API (Level 5):** AIP approval documented, API calls logged, rate-limited

---

## Troubleshooting Common Issues

### SSO Issues

| Problem | Cause | Solution |
|---------|-------|----------|
| "SAML assertion failed" | Clock skew or certificate expired | Sync server clocks, renew certificate |
| "User not in Compass" | Entra ID account not provisioned | Create account in Compass or sync via directory |
| "Infinite redirect loop" | ACS URL misconfigured | Verify Compass ACS URL matches Entra ID setting |

### SDS Issues

| Problem | Cause | Solution |
|---------|-------|----------|
| "Sync failed: duplicate group" | Group already exists in M365 | Delete old group, re-run SDS |
| "Students not added to teams" | Enrollment mapping incorrect | Check Student-Class enrollment in CSV |
| "Teachers not showing as owners" | Staff not synced to M365 | Ensure staff accounts exist in Entra ID |

### Exports Issues

| Problem | Cause | Solution |
|---------|-------|----------|
| "CSV file won't upload to SharePoint" | File too large or format error | Check CSV encoding (UTF-8), split if >100MB |
| "Power BI can't read CSV" | Column names changed | Verify column headers match Power BI model |
| "Data is stale" | Manual upload forgotten | Automate upload or set reminder |

---

## Next Steps

1. **Sprint 1:** Implement Level 1 (SAML SSO) + update Service Catalogue with Compass tiles
2. **Sprint 2:** Implement Level 2 (SDS) + Level 4 (Exports for dashboards)
3. **Ongoing:** Monthly Compass exports, quarterly review of integration effectiveness
4. **2027+:** Evaluate Level 5 (API) based on operational demand

---

**Ownership:** Digital Transformation Lead + Compass Admin + M365 Admin  
**Last Updated:** January 29, 2026  
**Status:** Ready for Sprint 1 Implementation
