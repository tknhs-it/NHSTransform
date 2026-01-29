# Nossal Digital Transformation — Sprint Execution Plan

**Version:** 1.0  
**Date:** January 29, 2026  
**Timeline:** Sprints 1-4 (Q1-Q3 2026)  
**Team:** IT + Digital Transformation + Facilities + Admin

---

## Overview: Build Roadmap

```
Sprint 1 (Jan 29 - Feb 27)      Sprint 2 (Feb 28 - Apr 10)      Sprint 3 (Apr 11 - May 23)      Sprint 4+ (May 24+)
────────────────────────        ──────────────────────────      ──────────────────────────      ──────────────
Portal Foundation               IT Tickets MVP                  Maintenance + Access            Integration & Analytics
├─ Home Site                    ├─ ITRequests list              ├─ Maintenance Requests         ├─ API Integrations
├─ Hub                          ├─ IT Support form              ├─ Access Request form          ├─ Custom connectors
├─ Viva Connections app         ├─ IT Triage flow               ├─ Corresponding flows          ├─ Fabric / advanced BI
├─ Service Catalogue list       ├─ Planner integration          └─ Governance refine            └─ Student Info System (SIS)
├─ KB skeleton                  ├─ IT dashboard                                                    integration (optional)
├─ Governance setup             └─ Change control process                                       
└─ Security baseline                                            

End Sprint 1: ~5 days effort      ~15 days effort                ~12 days effort                 Ongoing
                                                                                                 Maintenance & evolution
```

---

## Sprint 1: Portal Foundation (Jan 29 - Feb 27, 2026)

### Goal
Get the "front door" open. Staff & students can see the Nossal Digital Hub in Teams. No workflows live yet, but the canvas is ready.

### Deliverables

#### 1.1 SharePoint Home Site Setup (3 days)
- [ ] Create Home Site (managed by IT Lead)
- [ ] Design + build Home page (welcome, tiles, news section)
- [ ] Create sub-pages: Request, Knowledge Base, IT, Facilities, Admin, Dashboards
- [ ] Set up navigation hub links
- [ ] Apply sensitivity labels (Public / Staff-Only / Admin-Only)
- [ ] Enable Teams integration (can embed SharePoint pages in Teams)

**Owner:** IT Lead + SharePoint admin (1-2 days)  
**Acceptance Criteria:**
- Home Site is accessible to all staff + students
- Pages load in < 2s
- Mobile-responsive design works on phones
- Navigation menu works in Teams & desktop

#### 1.2 Viva Connections App in Teams (2 days)
- [ ] Enable Viva Connections license (included in A5, admin toggles it on)
- [ ] Create Viva app pointing to Home Site
- [ ] Configure dashboard tiles (Request Something, IT, Facilities, KB, Dashboards)
- [ ] Set up Audience rules (hide tiles based on group membership)
- [ ] Test on Teams desktop + mobile

**Owner:** Digital Transformation Lead (1-2 days)  
**Acceptance Criteria:**
- Viva app appears in Teams left sidebar as "Nossal Hub" icon
- Dashboard loads
- Tiles are clickable and route to SharePoint pages
- Audience filtering works (students don't see staff-only tiles)

#### 1.3 Service Catalogue List + Page (2 days)
- [ ] Create ServiceCatalogue SharePoint List
- [ ] Add 5 core services (IT Support, Maintenance, Access Request, Asset Loan, Purchase)
- [ ] Build "Request Something" page (gallery view, filtered by category)
- [ ] Add Viva Connections tile pointing to it
- [ ] Link tiles to placeholder Power Apps (not live yet, just links)

**Owner:** IT Lead + Builder (1-2 days)  
**Acceptance Criteria:**
- Service Catalogue list has 5+ services with all schema fields populated
- "Request Something" page shows grid of service tiles
- Each tile has icon, description, owner, SLA target visible
- Can filter by category
- Links go to Power Apps (will be live in Sprint 2)

#### 1.4 Knowledge Base Skeleton (2 days)
- [ ] Create Knowledge Base SharePoint List
- [ ] Seed with top 20 articles (writing by IT + admin staff):
  - **Getting Started (5):** First day setup, Teams, email, Wi-Fi, password reset
  - **How-To (8):** Request forms, access teams, join a channel, upload files, etc.
  - **Troubleshooting (4):** Wi-Fi problems, email slow, device won't start, can't access files
  - **Policy (3):** Device acceptable use, data classification, password standards
- [ ] Create KB page in SharePoint (searchable list + filter by category)
- [ ] Add Viva Connections tile
- [ ] Enable article status workflow (Draft → Published → Archived)

**Owner:** IT + Admin staff (2 days writing, 1 day list setup)  
**Acceptance Criteria:**
- 20 articles published + indexed
- Articles are searchable in Teams Search
- Categories + tags work
- Can filter by audience (Staff-Only vs. Public)

#### 1.5 Governance & Security Foundation (3 days)
- [ ] **Power Platform Setup:**
  - [ ] Create DEV environment (sandbox for builders)
  - [ ] Create PROD environment (locked down)
  - [ ] Set environment admin roles (IT Lead = admin, Builders = creators in DEV only)
  - [ ] Enable connector policy: Block consumer connectors in PROD
  
- [ ] **Entra ID & MFA:**
  - [ ] Audit current staff/student user count
  - [ ] Confirm MFA is enabled (Authenticator app on school devices)
  - [ ] Create security groups: Staff, Students, IT Admins, Facilities, Approval Managers
  
- [ ] **Purview Compliance:**
  - [ ] Create retention labels: "IT Records 7y", "Maintenance 3y", "Purchase 7y"
  - [ ] Create sensitivity labels: "Public", "Staff-Only", "Admin-Only"
  - [ ] Apply labels to Nossal Hub site (default: Staff-Only)
  - [ ] Enable DLP: Block sensitive data (student ID, staff names) from external email
  
- [ ] **Defender & Audit:**
  - [ ] Confirm Defender for Office 365 is enabled
  - [ ] Confirm Defender for Endpoint is enabled on school devices
  - [ ] Enable audit logging (SharePoint, Power Automate, Entra ID)
  - [ ] Export audit logs monthly (automate if possible)
  
- [ ] **Change Control Process:**
  - [ ] Create Change Control list in SharePoint (for tracking PROD deployments)
  - [ ] Draft Change Control SOP (1-page guide for IT team)
  - [ ] Schedule Change Control Board meetings (monthly, first Friday)

**Owner:** IT Director + IT Admin (3 days)  
**Acceptance Criteria:**
- DEV & PROD environments created + isolated
- MFA working for staff/students
- Retention labels applied to lists
- DLP rule logs policy violations (no blocks yet, audit-only)
- Audit logs exportable
- Change Control list ready for Sprint 2 deployments

#### 1.6 Documentation & Knowledge Transfer (1 day)
- [ ] Document all setup steps (for future IT staff)
- [ ] Create admin runbook: "How to Manage Nossal Hub" (add users, update KB, etc.)
- [ ] Train 2-3 IT staff on Viva Connections + SharePoint admin

**Owner:** Digital Transformation Lead (1 day)

### Sprint 1 Testing & QA

- [ ] **Functional Testing:** All pages load, no broken links, mobile works
- [ ] **Security Testing:** DLP blocks test email with student data, MFA prompt works, permission filtering works
- [ ] **User Acceptance Testing (UAT):** 10 staff volunteers test portal for 3 days
  - Feedback: Is it easy to find services? Is navigation clear?
  - Collect NPS score (target: 7+/10)
  - Fix critical issues before release

### Sprint 1 Release

**Release Date:** Feb 27, 2026  
**Who:** IT Lead + Digital Transformation Lead  
**Process:**
1. Final QA sign-off
2. Announce to staff: "Welcome to Nossal Digital Hub! New way to request IT/Facilities help."
3. Soft launch to pilot group (IT staff + 20 volunteers) for 3 days
4. Full launch to all staff + students
5. Monitor Planner + Teams for questions (first week)
6. Fix bugs/feedback within 48h

### Sprint 1 Success Metrics

| Metric | Target | Owner |
|--------|--------|-------|
| Portal launch on-time | ✓ | IT Lead |
| All pages load < 2s | 95%+ | Performance review |
| Mobile responsive | ✓ | QA testing |
| Security baseline checklist complete | ✓ | IT Director |
| UAT NPS score | 7+/10 | User feedback |
| Staff/student awareness | 50%+ aware of Nossal Hub exists | Comms follow-up |

---

## Sprint 2: IT Tickets MVP (Feb 28 - Apr 10, 2026)

### Goal
First workflow goes live. IT staff now receive & track all support requests in one place.

### Deliverables

#### 2.1 ITRequests List & Power App (5 days)
- [ ] Create ITRequests SharePoint List (schema: all fields from Workflow Specs)
- [ ] Create IT Support Power App (canvas app, form layout, mobile-first)
- [ ] Embed app in Teams (as a tab in IT Support channel)
- [ ] Embed app in SharePoint (Request Something page)
- [ ] Add camera integration (mobile screenshot capture)
- [ ] Test form submission → list item creation

**Owner:** Power Platform Builder (3-4 days)  
**Acceptance Criteria:**
- Form opens in Teams + SharePoint without errors
- All form fields present + working
- Submit button creates list item with auto-number IT-2026-000001
- Form clears on successful submit
- User gets success message with ticket number

#### 2.2 IT Triage Power Automate Flow (5 days)
- [ ] Build flow: Triggered on ITRequests item created
- [ ] Flow reads RoutingTags + routes to correct Teams channel
- [ ] Posts summary to Teams (auto-mentions on-call IT lead if Critical)
- [ ] Creates Planner task (bucket by category, assigns to IT, sets due date = SLA)
- [ ] Sends email to requester (confirmation + ticket number)
- [ ] Tests flow with sample data (5 test cases: different categories, impact levels)

**Owner:** Power Automate Flow Builder (4-5 days)  
**Acceptance Criteria:**
- Flow runs without errors
- Teams message posts within 2 min of form submit
- Planner task auto-created + assigned
- Email sends to requester (test with IT staff first)
- SLA due date calculated correctly (Critical = 4h, Standard = 1 day)

#### 2.3 IT Status Update Flow (3 days)
- [ ] Build flow: Triggered when ITRequests status changes
- [ ] On "In Progress": Email requester, post to Teams
- [ ] On "Waiting on User": Email requester, add Planner label
- [ ] On "Resolved": Calculate SLAMet, email requester, post to IT channel
- [ ] On "Closed": Archive Planner task, send final notification

**Owner:** Power Automate Flow Builder (2-3 days)  
**Acceptance Criteria:**
- Status transitions trigger emails + Teams posts
- SLA calculation is correct
- Notifications are timely (< 1 min after status change)
- No duplicate notifications

#### 2.4 IT Dashboard (Power BI) (3 days)
- [ ] Create Power BI report: Connect to ITRequests list as data source
- [ ] Add visualizations:
  - Tile: Total open tickets + breakdown by category
  - Tile: Avg time to triage (target: 2h)
  - Tile: Avg time to resolve (target: 1 day)
  - Tile: % SLA Met (target: 85%+)
  - Chart: Tickets created per week (trend)
  - Chart: Distribution by impact level
  - Chart: Top issues (by category)
- [ ] Publish to SharePoint (Dashboards page)
- [ ] Add row-level security (IT staff can see all, other staff can see only their own tickets)
- [ ] Set refresh: Daily at 6am

**Owner:** Power BI analyst (2-3 days)  
**Acceptance Criteria:**
- Dashboard loads < 3s
- All tiles show correct data
- Charts are readable + insightful
- Data refreshes daily
- Mobile view works

#### 2.5 Change Control Process Testing (2 days)
- [ ] Perform dry run: Deploy IT Support app from DEV to PROD
- [ ] Test rollback scenario: Disable app, then re-enable
- [ ] Document steps in Change Control runbook
- [ ] Train IT team on approval process

**Owner:** IT Lead (1-2 days)  
**Acceptance Criteria:**
- App deployment takes < 30 min
- Rollback works
- Zero downtime during deployment
- Team understands change control

#### 2.6 IT Staff Training (1 day)
- [ ] Train 5-10 IT staff on:
  - How to triage tickets in Planner
  - How to update status in list
  - How to read the dashboard
  - How to escalate critical issues
- [ ] Create quick reference card (laminated, desk-side)

**Owner:** IT Manager (1 day)

### Sprint 2 Testing & QA

**UAT Plan (2 weeks before release):**
1. Select 3 IT tech volunteers
2. Have them use IT Support form to submit test requests
3. IT Manager triages + resolves the requests
4. Collect feedback: "Is the form easy? Is Planner clear? Is email helpful?"
5. Iterate based on feedback

**Load Testing:**
- Simulate 50 concurrent form submissions (peak scenario)
- Monitor flow performance (target: < 2s per request processing)
- Monitor Teams notifications (ensure no flooding)

### Sprint 2 Release

**Release Date:** Apr 10, 2026  
**Who:** IT Lead + Builder  
**Process:**
1. Deploy to PROD via Change Control
2. Soft launch to IT team (use for 1 week before full rollout)
3. Announce to staff: "New way to report IT issues — use Nossal Hub → Request Something → IT Support"
4. Monitor for 2 weeks (daily standup, track issues)
5. Celebrate! First workflow live.

### Sprint 2 Success Metrics

| Metric | Target | Owner |
|--------|--------|-------|
| Form submission success rate | 95%+ | Flow monitoring |
| Avg triage time | < 2h | Planner tracking |
| Avg resolution time | 1 day (standard) | Dashboard |
| % SLA Met | 85%+ | Dashboard |
| User satisfaction (NPS) | 7+/10 | Survey after 2 weeks |
| Zero critical bugs in first 2 weeks | ✓ | QA |

---

## Sprint 3: Maintenance + Access Requests (Apr 11 - May 23, 2026)

### Goal
Replicate IT Tickets pattern for Maintenance & Access. Add approval gates for sensitive requests.

### Deliverables

#### 3.1 Maintenance Requests Workflow (5 days)
- [ ] Create MaintenanceRequests list (schema from Workflow Specs)
- [ ] Build Maintenance Request Power App
- [ ] Embed in Teams + SharePoint
- [ ] Build Maintenance Triage flow (routes by location + issue type)
- [ ] Build status update flow
- [ ] Create Maintenance dashboard (similar to IT)
- [ ] Train Facilities staff

**Owner:** Power Platform team (reuse IT pattern) (4-5 days)

#### 3.2 Access Request Workflow with Manager Approval (5 days)
- [ ] Create AccessRequests list
- [ ] Build Access Request Power App
- [ ] Build Access Request flow with manager approval gate:
  - Route to Requester's manager (via Entra ID lookup)
  - Manager approves/rejects via Approvals connector
  - If approved, IT action task created
  - If admin request, secondary IT Lead approval gate
- [ ] Build status update flow
- [ ] Create dashboard (approval times, pending requests, completion rate)
- [ ] Train IT + managers on approval workflow

**Owner:** Power Platform team (4-5 days)  
**Acceptance Criteria:**
- Manager receives approval request within 1 min of form submit
- Approval takes < 5 min to complete
- Email confirmation sent to requester on approval/rejection
- IT task created only after approval

#### 3.3 Asset Loan Workflow (3 days)
- [ ] Create AssetLoans list
- [ ] Build Asset Loan Power App
- [ ] Build scheduling + reminder flows:
  - 2 days before due: Reminder to borrower
  - 1 day overdue: Escalation to IT
- [ ] Create dashboard (utilization, overdue rate)

**Owner:** Power Platform team (2-3 days)

#### 3.4 Purchase Request Workflow with Multi-Stage Approval (4 days)
- [ ] Create PurchaseRequests list
- [ ] Build Purchase Request Power App
- [ ] Build approval flow:
  - Manager approval if < $500
  - Manager + Budget Owner if >= $500
  - Create procurement task on approval
- [ ] Create dashboard (approval times, budget utilization by dept)

**Owner:** Power Platform team (3-4 days)

#### 3.5 Governance Refinement (2 days)
- [ ] Review DEV/PROD separation (are we following it?)
- [ ] Audit Power Automate flows for premium connectors (should be none)
- [ ] Validate retention labels are applied to new lists
- [ ] Validate DLP is logging (not blocking yet)
- [ ] Update Change Control process based on Sprint 2 experience

**Owner:** IT Director + Governance lead (2 days)

#### 3.6 Knowledge Base Update (1 day)
- [ ] Add 10 new KB articles specific to new workflows:
  - "How to request access"
  - "How to submit a maintenance request"
  - "When can I borrow equipment?"
  - etc.

**Owner:** Admin + Facilities staff (1 day)

### Sprint 3 Testing & QA

- UAT for each new workflow (similar to IT)
- Test approval flows with real managers
- Load test: 100 concurrent requests
- Security test: DLP blocking + audit logging

### Sprint 3 Release

**Release Date:** May 23, 2026  
**Process:**
1. Staggered rollout: Maintenance first (no approvals, lower risk)
2. Then Access Requests (with manager approval gate)
3. Then Asset Loans
4. Then Purchase Requests (complex approval flow, release last)
5. Celebrate! 5 core workflows live.

### Sprint 3 Success Metrics

- All 4 workflows launched on-time
- Approval SLA met (manager approval < 2 days)
- Zero critical incidents in first 2 weeks
- User satisfaction > 7/10 NPS
- Dashboard shows service metrics improving (SLA attainment, response times)

---

## Sprint 4+: Integration & Evolution (May 24+, 2026)

### Goal
Evolve beyond MVP. Integrate with systems of record, add advanced analytics, plan for next phase.

### Possible Initiatives (TBD, based on demand)

#### 4.1 Facilities Management System Integration
- Sync maintenance requests to external CMMS (Computerized Maintenance Management System)
- Real-time status sync back to Nossal Hub
- Requires HTTP connector or Logic Apps (premium licensing discuss with budget owner)

#### 4.2 Student Information System (SIS) Integration
- Sync student/staff data from SIS to Entra ID
- Auto-update manager assignments when staff changes
- Requires API integration (may need professional services)

#### 4.3 Advanced Analytics & Fabric
- Migrate from Power BI to Fabric for real-time analytics
- Predictive models: "Which IT categories are trending? Will we have bottleneck next month?"
- Requires Fabric license (~$4,500/month or on-demand)

#### 4.4 Student Portal Expansion
- Student self-service (absence requests, library bookings, schedule changes)
- Parent communication portal (grades, attendance, events)
- Requires additional Power Apps + flows

#### 4.5 Mobile App
- Native mobile app for iOS/Android (vs. web forms in Teams)
- Offline form submission + sync
- Requires Power Apps native experience or custom development

### Ongoing Operations (Every Sprint)

- **Monthly Change Control Board:** Review new features, bugs, roadmap
- **Quarterly Review:** Metrics review, feedback from users, plan next quarter
- **Annual Audit:** Security, compliance, licensing verification
- **Continuous KB Updates:** Evergreen content refresh
- **User Training:** Onboarding for new staff/students

---

## Cross-Sprint Activities

### Comms & Change Management (Throughout)

| Milestone | Audience | Message | Channel |
|-----------|----------|---------|---------|
| Sprint 1 Launch | All staff + students | "Welcome to Nossal Digital Hub" | Email + Teams announcement |
| Sprint 2 Launch | All staff + students | "New IT Support form now live" | Nossal Hub banner + email |
| Sprint 3 Launch (Maintenance) | All staff + students | "Report maintenance issues quickly" | Nossal Hub banner |
| Sprint 3 Launch (Access) | IT + managers | "New approval workflow for access requests" | Teams + training session |
| Quarterly Review | IT leadership | "Q1 metrics: [SLA, NPS, adoption %, issues fixed]" | Change Control Board |

### User Training & Documentation

| Sprint | Training Topic | Format | Audience |
|--------|---|---|---|
| 1 | Nossal Hub basics (navigation, search, KB) | Video + quick ref card | All staff/students |
| 2 | "How to report an IT issue" | Video + KB article + form demo | All staff/students |
| 3 | "How to request access" (manager approval) | Training session + KB article | Managers + staff |
| 3 | "How to approve access requests" | Runbook + email guide | Managers |
| 4+ | Advanced features (dashboards, analytics) | Optional training | IT + admin staff |

### Metrics & Monitoring

**Dashboard: Nossal Hub Health (published to SharePoint, updated daily)**

| Metric | Target | Current | Trend |
|--------|--------|---------|-------|
| Portal uptime | 99.9% | - | - |
| Page load time (avg) | < 2s | - | - |
| Form submission success rate | 95%+ | - | - |
| Avg triage time (IT) | < 2h | - | - |
| Avg resolution time (IT) | 1 day | - | - |
| SLA attainment (IT) | 85%+ | - | - |
| User satisfaction (NPS) | 7+/10 | - | - |
| KB article views/month | 500+ | - | - |
| New feature adoption | 80%+ (2 weeks post-launch) | - | - |

**Review cadence:**
- Weekly (IT team): Planner board + flow health
- Monthly (Change Control Board): Trends + roadmap
- Quarterly (Leadership): Executive summary + budget review

---

## Budget & Resources

### Team Composition (Full-Time Equivalent, 12 months)

| Role | Sprint 1 | Sprint 2 | Sprint 3 | Sprint 4+ | Notes |
|------|---------|---------|---------|-----------|-------|
| **IT Director** | 0.2 | 0.2 | 0.1 | 0.1 | Governance, decisions |
| **IT Lead / PM** | 0.8 | 0.8 | 0.6 | 0.4 | Coordination, SharePoint, training |
| **Power Platform Builder** | 0.5 | 1.0 | 1.0 | 0.5 | Apps, flows, dashboards |
| **Power Automate Dev** | 0.5 | 1.0 | 1.0 | 0.5 | Complex flows, integrations |
| **Power BI / Analytics** | 0.2 | 0.5 | 0.5 | 0.3 | Dashboards, metrics |
| **IT Admin / Operations** | 0.3 | 0.3 | 0.3 | 0.2 | Governance, security, lists |
| **Facilities / Admin SME** | 0.2 | 0.2 | 0.5 | 0.2 | Requirements, workflows, training |

**Total effort:** ~4-5 FTE for 12 months (year 1)

### Costs

| Item | Cost | Notes |
|------|------|-------|
| **M365 A5 Education** | Included | Assumed in existing contract |
| **Staff labor (FTE)** | ~$250-350k | 4-5 FTE @ blended rates |
| **External training / consulting** | $0-20k | Optional, if needed |
| **Premium licenses (v2+)** | $0 (MVP), $200-500/mo (later) | HTTP connectors, advanced analytics |
| **Total Year 1** | **~$250-370k** | Mostly labor |

---

## Risk Mitigation

| Risk | Impact | Mitigation |
|------|--------|-----------|
| **Power Platform licensing surprise** | Medium | Review Premium Connectors list monthly; audit flows |
| **Low user adoption** | High | Strong comms + training; make forms easy; showcase quick wins |
| **Data breach / compliance violation** | Critical | DLP + retention policies + audit logging from Sprint 1 |
| **Critical bug in production** | High | DEV/PROD separation + change control + UAT |
| **Scope creep** | Medium | Monthly Change Control Board gates new requests |
| **Staff burnout** | Medium | Realistic timelines; assign dedicated resources; celebrate wins |
| **Identity/access issues (admin vs. curriculum)** | Medium | Clarify upfront (single tenant vs. multi-tenant); design access controls early |

---

## Success Criteria (End of Sprint 4)

### Launch Goals (✓ = achieved)
- [ ] All 5 core workflows live & stable
- [ ] 80%+ staff/student awareness of Nossal Hub
- [ ] 60%+ staff/student using Nossal Hub at least once
- [ ] Zero critical security breaches
- [ ] Full audit compliance (retention + DLP + labels)

### Performance Goals
- [ ] Avg form submission: < 3 min per request (data entry + submit)
- [ ] Avg triage time: < 2h (IT requests)
- [ ] Avg resolution time: 1 day (standard), 4h (critical)
- [ ] SLA attainment: 85%+
- [ ] Portal uptime: 99.9%
- [ ] Page load time: < 2s (p95)

### Satisfaction Goals
- [ ] User NPS: 7+/10
- [ ] IT staff satisfaction: "Workflows save us time" (qualitative)
- [ ] Manager satisfaction: "Approvals are streamlined" (qualitative)

### Sustainability Goals
- [ ] Change Control process is routine (no drama)
- [ ] Knowledge Base is up-to-date (> 80% of articles reviewed in past 6 mo)
- [ ] Governance policies are documented + followed
- [ ] Training is repeatable (new staff can self-serve KB)
- [ ] Metrics dashboard is live + reviewed monthly

---

## Next Steps (Post-Sprint 4)

1. **Retrospective:** What worked? What didn't? What would you do differently?
2. **Vision 2.0:** "What's the next wave of digital transformation?"
   - SIS integration?
   - Advanced analytics?
   - Student portal?
3. **Budget planning:** Justify any premium licensing for Phase 2
4. **Roadmap 2026-2027:** Rolling 12-month plan
5. **Team evolution:** Do we need to hire? Upskill existing team?

---

**Ownership:** Digital Transformation Lead + IT Director  
**Last Updated:** January 29, 2026  
**Status:** Ready for Sprint 1 Kickoff
