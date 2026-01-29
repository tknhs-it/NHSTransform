# Nossal Hub + Teams Rationalization — Launch Playbook

**Version:** 1.0  
**Date:** January 29, 2026  
**Audience:** Teams Admin + IT Lead  
**Timeline:** Weeks 1-3 of Sprint 1 (Jan 29 - Feb 27)

---

## Quick Summary

You're launching a "Nossal Hub" (Viva Connections dashboard in Teams) and rationalizing the chaotic "Staff of Nossal HS" team from 25+ channels down to 8 core channels.

| Phase | Timeline | Owner | Effort |
|-------|----------|-------|--------|
| **Phase 1: Build** | Weeks 1-2 | SharePoint + Teams Admin | 3-5 days |
| **Phase 2: Test** | Week 2 | IT + 10 pilot staff | 1 day active |
| **Phase 3: Launch** | Week 3 | All teams | 1 day + monitoring |

**Release Date:** February 27, 2026

---

## Phase 1: Build Hub (Behind Scenes)

### Step 1.1: Create SharePoint Home Site (1 day)

**Owner:** SharePoint Admin  
**Tools:** SharePoint admin center

```powershell
# In SharePoint Admin Center:
1. Go to "Home sites" → Create a new home site
2. Site name: "Nossal Digital Hub"
3. Site URL: nossal.sharepoint.com/sites/digitalhub
4. Hub: None (this IS the hub)
5. Template: Default (blank, customize later)
6. Owners: IT Lead, Digital Transformation Lead
7. Create → Wait for provisioning (2-3 min)
```

**After creation:**
- [ ] Site provisioned successfully
- [ ] Owners can access site
- [ ] Default home page visible

### Step 1.2: Set Up Home Page (1 day)

**Owner:** IT Lead + SharePoint Admin  
**Tools:** SharePoint page editor

```
Home Page Layout:
┌─────────────────────────────────────┐
│  NOSSAL DIGITAL HUB                 │
│  Welcome! Your single front door    │
│                                     │
│  🔗 Quick Links:                    │
│  [Compass] [IT] [Facilities] [Help] │
├─────────────────────────────────────┤
│  📢 Latest News (from #announcements)│
│  • School news item 1               │
│  • School news item 2               │
├─────────────────────────────────────┤
│  COMPASS SHORTCUTS                  │
│  [Attendance] [Learning] [Wellbeing]│
│  [Events] [Parent Comms]            │
├─────────────────────────────────────┤
│  REQUEST SOMETHING                  │
│  [IT] [Maintenance] [Access]        │
│  [Equipment] [Purchase]             │
├─────────────────────────────────────┤
│  KNOWLEDGE BASE                     │
│  [Policies] [How-To] [Forms] [Roles]│
└─────────────────────────────────────┘
```

**Steps:**
1. Open home page in edit mode
2. Add title: "Nossal Digital Hub"
3. Add subtitle: "Your single front door for Compass, requests, and knowledge"
4. Add hero image (optional, school branding)
5. Add quick links (navigation section)
6. Add news section (pulls from SharePoint news)
7. Add Viva Connections tiles (will configure in Step 1.4)
8. Publish

**Checklist:**
- [ ] Title + subtitle visible
- [ ] Hero image loads (if used)
- [ ] Quick links section present
- [ ] News section configured
- [ ] Page is responsive (test on mobile)
- [ ] Page load time < 2 sec

### Step 1.3: Create Knowledge Base List + Pages (1 day)

**Owner:** IT Lead + Content contributor  
**Tools:** SharePoint list + modern pages

```
Structure:
SharePoint Home Site
└─ Knowledge Base (Library)
   ├─ Policies (folder)
   │  ├─ Code of Conduct.pdf
   │  ├─ Privacy Policy.pdf
   │  └─ IT Policy.pdf
   │
   ├─ How-To Guides (folder)
   │  ├─ Wi-Fi Setup.pdf
   │  ├─ Printing Guide.pdf
   │  └─ AV Equipment Guide.pdf
   │
   ├─ Forms & Templates (folder)
   │  ├─ Leave Request.docx
   │  ├─ Incident Report.docx
   │  └─ Lesson Plan Template.docx
   │
   └─ Roles & Responsibilities (folder)
      ├─ Staff Directory.xlsx
      └─ Team Contacts.xlsx
```

**Steps:**
1. Create library: "Knowledge Base"
2. Create sub-folders: Policies, How-To, Forms, Roles
3. Upload documents from old "Policies", "Forms", "How-To" channels
4. For each document:
   - [ ] Title is descriptive
   - [ ] Description explains what it is
   - [ ] Sensitivity label: "Staff-Only" or "Public"
   - [ ] Retention policy: Keep 7 years (or per school policy)

**Checklist:**
- [ ] Library created with folders
- [ ] All old channel documents migrated
- [ ] Sensitivity labels applied
- [ ] Search indexing working (test search)
- [ ] Permissions: All staff can read, only IT can edit

### Step 1.4: Create ServiceCatalogue List (1 day)

**Owner:** IT Lead + Builder  
**Tools:** SharePoint list editor

```
Service Catalogue Schema:

List name: ServiceCatalogue
Columns:
├─ Title (text) — Service name (e.g., "Mark Attendance")
├─ Description (text) — What it is
├─ Category (choice) — Compass / IT / Facilities / Admin / Teaching
├─ SystemOfRecord (choice) — Compass / Microsoft
├─ LaunchType (choice) — DeepLink / PowerApp / Page
├─ LaunchTarget (URL) — https://compass.nossal.vic.edu.au/attendance
├─ Audience (choice) — All Staff / Teaching Staff / Admin / Students
├─ Icon (text) — emoji or icon name
├─ IsActive (yes/no) — Is this tile currently live?
├─ SortOrder (number) — Display order in Hub
├─ CompassIntegrationLevel (choice) — Level 1 / Level 2 / Level 3 (for reference)
└─ RoutingTags (text) — Tags for filtering/search
```

**Sample Data to Add:**

| Title | Category | SystemOfRecord | LaunchType | LaunchTarget | IsActive |
|-------|----------|---|---|---|---|
| Mark Attendance | Compass | Compass | DeepLink | https://compass.nossal.vic.edu.au/attendance | ✓ |
| Learning Tasks | Compass | Compass | DeepLink | https://compass.nossal.vic.edu.au/learning | ✓ |
| Student Wellbeing | Compass | Compass | DeepLink | https://compass.nossal.vic.edu.au/wellbeing | ✓ |
| Events & Calendar | Compass | Compass | DeepLink | https://compass.nossal.vic.edu.au/events | ✓ |
| Parent Comms | Compass | Compass | DeepLink | https://compass.nossal.vic.edu.au/parent-portal | ✓ |
| Log IT Request | IT | Microsoft | PowerApp | [Power App URL] | ✓ (launch Sprint 2) |
| Maintenance Request | Facilities | Microsoft | PowerApp | [Power App URL] | ✓ (launch Sprint 2) |
| Request Access | IT | Microsoft | PowerApp | [Power App URL] | ✓ (launch Sprint 2) |
| Equipment Loan | Facilities | Microsoft | PowerApp | [Power App URL] | ✓ (launch Sprint 2) |
| Purchase Request | Admin | Microsoft | PowerApp | [Power App URL] | ✓ (launch Sprint 3) |
| Policies | Knowledge | Microsoft | Page | https://nossal.sharepoint.com/.../Policies | ✓ |
| How-To Guides | Knowledge | Microsoft | Page | https://nossal.sharepoint.com/.../How-To | ✓ |
| Forms & Templates | Knowledge | Microsoft | Page | https://nossal.sharepoint.com/.../Forms | ✓ |

**Steps:**
1. Create ServiceCatalogue list (modern list)
2. Add columns (match schema above)
3. Add sample data (Compass + Knowledge tiles)
4. Power Apps tiles will be added in Sprint 2
5. Set up view: "Tile view" sorted by Category, then SortOrder
6. Test filtering by Category, SystemOfRecord, Audience

**Checklist:**
- [ ] List created with all columns
- [ ] Sample data added (Compass + Knowledge tiles)
- [ ] Tile view set up and working
- [ ] All links are valid (test by clicking)
- [ ] Permissions: All staff can read, only IT Lead can edit

### Step 1.5: Configure Viva Connections (1 day)

**Owner:** Teams Admin + Digital Transformation Lead  
**Tools:** Teams admin center + Viva Connections manager

```
Steps:

1. Enable Viva Connections for organization
   ├─ Teams Admin Center
   ├─ Teams apps → Manage apps → Viva Connections
   └─ Allow usage for your organization

2. Create Viva Connections experience
   ├─ Go to Viva Connections manager
   ├─ Create new experience
   ├─ Name: "Nossal Hub"
   ├─ Home site: Point to SharePoint home site (created in Step 1.2)
   ├─ Dashboard: Tile layout (pull from ServiceCatalogue list)
   └─ Save

3. Configure dashboard tiles
   Dashboard layout:

   ROW 1: COMPASS SHORTCUTS
   ├─ [Attendance icon] Mark Attendance → Opens Compass link (SSO)
   ├─ [Book icon] Learning Tasks → Opens Compass link (SSO)
   └─ [Heart icon] Student Wellbeing → Opens Compass link (SSO)

   ROW 2: COMPASS SHORTCUTS (continued)
   ├─ [Calendar icon] Events & Calendar → Opens Compass link (SSO)
   └─ [Parent icon] Parent Communications → Opens Compass link (SSO)

   ROW 3: REQUEST SOMETHING
   ├─ [Wrench icon] Log IT Request → Opens Power App (Sprint 2)
   ├─ [Tools icon] Maintenance Request → Opens Power App (Sprint 2)
   ├─ [Key icon] Request Access → Opens Power App (Sprint 2)
   ├─ [Box icon] Equipment Loan → Opens Power App (Sprint 2)
   └─ [Shopping icon] Purchase Request → Opens Power App (Sprint 3)

   ROW 4: KNOWLEDGE BASE
   ├─ [Document icon] Policies → SharePoint page
   ├─ [Question icon] How-To Guides → SharePoint page
   ├─ [Folder icon] Forms & Templates → SharePoint page
   └─ [Users icon] Roles & Responsibilities → SharePoint page

4. Set audience rules (who sees what)
   Example:
   ├─ "Mark Attendance" → visible to Teaching staff + Admin (not students)
   ├─ "Log IT Request" → visible to All staff + students
   ├─ "Purchase Request" → visible to Admin + Facilities (not teaching staff)
   └─ Default: Show to All

5. Test in desktop Teams
   ├─ Open Teams
   ├─ Go to Viva Connections (should appear in left rail)
   ├─ Verify dashboard loads
   ├─ Click a tile → verify it opens correct page/link
   └─ Test on mobile Teams (if available)

6. Deploy to organization
   ├─ Create app setup policy
   ├─ Pin Viva Connections for all staff
   ├─ Push update to all Teams clients
   └─ Roll out gradually (10% of staff first, monitor)
```

**Checklist:**
- [ ] Viva Connections enabled for organization
- [ ] Dashboard created and configured
- [ ] All tiles point to correct targets
- [ ] Compass tiles test with SSO (click and verify)
- [ ] App loads in desktop Teams
- [ ] App loads in mobile Teams
- [ ] Performance: Dashboard loads < 2 sec
- [ ] Audience rules working (test with different user roles)

### Step 1.6: Document Admin Runbook (1 day)

**Owner:** Digital Transformation Lead  
**Tools:** Word / Markdown editor

```
Runbook: Managing Nossal Hub

Topic: How to add a new service tile to the Hub

Steps:
1. Go to ServiceCatalogue list in SharePoint
2. New item → Fill in:
   - Title: [Service name]
   - Description: [What is it?]
   - Category: [Compass / IT / Facilities / etc.]
   - SystemOfRecord: [Compass or Microsoft]
   - LaunchType: [DeepLink / PowerApp / Page]
   - LaunchTarget: [URL]
   - IsActive: [Yes]
   - SortOrder: [pick a number higher than existing items]
3. Save
4. Viva dashboard will auto-refresh within 5 min
5. New tile appears on Nossal Hub

Topic: How to update a Knowledge Base article

Steps:
1. Go to Knowledge Base library
2. Find article folder (Policies / How-To / Forms)
3. Upload new version of document
4. Replace old version (or archive old version to "Archive" folder)
5. Clear Teams cache on your device (settings → cache)
6. Staff will see new version next time they visit Hub

Topic: How to lock/archive an old Teams channel

Steps:
1. Open Teams
2. Right-click channel name → Edit channel
3. Moderation tab → Turn ON moderation
4. Allowed posters: Only channel owners
5. Add a tab: Link to Hub
6. Channel becomes read-only
7. Add description: "This content has moved to Nossal Hub. Click tab below."

Topic: How to troubleshoot "Compass tile not opening"

Symptoms: User clicks Compass tile, gets "Access denied"
Solution:
1. Verify user is in Entra ID
2. Verify user is in Compass (ask Compass admin)
3. Verify SAML trust is configured between Entra & Compass
4. Check Compass audit logs: Did SAML authentication succeed?
5. Check Teams error logs: Does Teams have connectivity to Compass?
6. Try incognito window (clear cache)
7. Escalate to Compass admin if SAML failing
```

**Checklist:**
- [ ] Runbook covers: Adding tiles, Updating KB, Locking channels, Troubleshooting
- [ ] Each topic has clear step-by-step instructions
- [ ] Contact info for escalations (Compass admin, etc.)
- [ ] Screenshots (if possible)
- [ ] Version control (date, owner)

---

## Phase 2: Test Hub with Pilot Group (Week 2)

### Step 2.1: Create New Channels in Staff of Nossal HS (1 day)

**Owner:** Teams Admin  
**Tools:** Teams admin center

```
In "Staff of Nossal HS" team, create these 8 channels:

1. #announcements
   ├─ Description: "Leadership updates and school-wide news"
   ├─ Moderation: ON (only owners can post)
   └─ Icon: 📢

2. #daily-ops
   ├─ Description: "Urgent notices, building closures, daily alerts"
   ├─ Moderation: ON (only owners can post)
   └─ Icon: 📋

3. #teaching-learning
   ├─ Description: "Curriculum ideas, resource sharing, teaching tips"
   ├─ Moderation: OFF (open discussion)
   └─ Icon: 📚

4. #wellbeing-support
   ├─ Description: "Pastoral updates, student support, welfare checks"
   ├─ Moderation: OFF (open discussion)
   └─ Icon: 💙

5. #events-programs
   ├─ Description: "School events, excursions, co-curricular updates"
   ├─ Moderation: OFF (open discussion)
   └─ Icon: 🎉

6. #staff-forum
   ├─ Description: "General chat, ideas, feedback, social"
   ├─ Moderation: OFF (open discussion)
   └─ Icon: 💬

7. #help-requests
   ├─ Description: "Tabs only — menu for all requests and help"
   ├─ Moderation: OFF (tabs only, no chat)
   └─ Icon: 🆘

8. #resources
   ├─ Description: "Tabs only — directory for policies, forms, knowledge"
   ├─ Moderation: OFF (tabs only, no chat)
   └─ Icon: 📖
```

**Steps for each channel:**
1. Right-click "Staff of Nossal HS" team → Manage team → Channels
2. Add channel → Fill in name, description, icon
3. For #announcements & #daily-ops:
   - [ ] Right-click channel → Edit channel
   - [ ] Moderation tab → Turn ON member approval
   - [ ] Bypass moderation: Add Comms Team members
4. For #help-requests & #resources:
   - [ ] Right-click channel → Edit channel
   - [ ] Channel settings → Pinned posts are for tabs only
   - [ ] Hide chat box (optional, depends on Teams settings)

**Checklist:**
- [ ] All 8 channels created
- [ ] Descriptions match above
- [ ] Icons set
- [ ] Moderation enabled on #announcements & #daily-ops
- [ ] #help-requests and #resources are tab-focused

### Step 2.2: Add Tabs to #help-requests and #resources (1 day)

**Owner:** Teams Admin + IT Lead

**#help-requests Tabs:**

```
Tab 1: Nossal Hub
├─ Display name: "Nossal Hub"
├─ Link: https://teams.microsoft.com/l/app/[viva-app-id]
└─ Icon: House

Tab 2: Log IT Request
├─ Display name: "Log IT Request"
├─ Link: https://apps.powerapps.com/play/[IT-APP-ID] (add in Sprint 2)
└─ Icon: Wrench

Tab 3: Request Maintenance
├─ Display name: "Request Maintenance"
├─ Link: https://apps.powerapps.com/play/[MAINT-APP-ID] (add in Sprint 2)
└─ Icon: Tools

Tab 4: Request Access
├─ Display name: "Request Access / Permissions"
├─ Link: https://apps.powerapps.com/play/[ACCESS-APP-ID] (add in Sprint 2)
└─ Icon: Key

Tab 5: Equipment Loan (Optional, add in Sprint 2)
Tab 6: Purchase Request (Optional, add in Sprint 3)
```

**#resources Tabs:**

```
Tab 1: Nossal Hub
├─ Display name: "Nossal Hub"
├─ Link: https://teams.microsoft.com/l/app/[viva-app-id]
└─ Icon: House

Tab 2: Forms & Templates
├─ Display name: "Forms & Templates"
├─ Link: https://nossal.sharepoint.com/sites/digitalhub/Forms
└─ Icon: Document

Tab 3: Policies & Compliance
├─ Display name: "Policies & Compliance"
├─ Link: https://nossal.sharepoint.com/sites/digitalhub/Policies
└─ Icon: Shield

Tab 4: Knowledge Base
├─ Display name: "Knowledge Base"
├─ Link: https://nossal.sharepoint.com/sites/digitalhub/KB
└─ Icon: Question mark

Tab 5: Roles & Responsibilities (Optional)
├─ Display name: "Roles & Responsibilities"
├─ Link: https://nossal.sharepoint.com/sites/digitalhub/Roles
└─ Icon: Users
```

**How to add a tab:**
1. Open Teams → #help-requests channel
2. Click + (add tab) next to existing tabs
3. Choose "Web site" or "Custom app"
4. Paste URL, set display name, choose icon
5. Save
6. Test: Click tab → verify it opens correct page

**Checklist:**
- [ ] All tabs in #help-requests created
- [ ] All tabs in #resources created
- [ ] Each tab opens correct page (test by clicking)
- [ ] Tabs are in logical order
- [ ] No broken links

### Step 2.3: Invite Pilot Group (1 day)

**Owner:** IT Lead  
**Participants:** 10 staff volunteers

```
Invite criteria:
├─ Mix of roles: Teachers, Admin, IT, Facilities
├─ Tech comfort level: Mix of advanced and less-tech-savvy
├─ Influence: At least 2-3 people who are "influencers" (others will follow them)
└─ Time: They'll test for 1 week (3-5 hours total)

Invitation email:
---
Subject: Help us test Nossal Hub (Week of [DATE]) 

Hi [Names],

We're launching Nossal Hub on Feb 27 — our new digital front door in Teams.

We'd like you to help us test it first (confidential pilot, week of [DATE]).

What we need:
✓ Spend 1-2 hours exploring the Hub
✓ Try clicking Compass tiles (do they open?)
✓ Try finding a policy or form in the Knowledge Base
✓ Try using #help-requests tab (are tabs clear?)
✓ Fill out a quick NPS survey (takes 2 min)
✓ Report any bugs or confusing stuff

What you'll see:
• New "Nossal Hub" app in Teams (left rail)
• Streamlined "Staff of Nossal HS" team (8 channels instead of 25+)
• Compass shortcuts (Attendance, Learning, Wellbeing, Events)
• Request forms (coming soon in Sprint 2)
• Knowledge Base (Policies, How-To, Forms)

Timeline:
├─ Mon-Fri [DATE RANGE]: You test
├─ Friday EOD: You submit feedback/NPS survey
├─ Mon Feb 27: We launch to all staff
└─ Tue Feb 28 - Sun Mar 6: We monitor and help staff

Questions? Ping me.

Thanks for helping us launch this!
---
```

**Checklist:**
- [ ] 10 diverse staff invited
- [ ] They understand what to test
- [ ] They know deadline for feedback
- [ ] Feedback form is ready (Google Form or Teams form)

### Step 2.4: Gather Feedback (1 week)

**Owner:** IT Lead + Digital Transformation Lead  
**Feedback form (Google Form or Teams form):**

```
Question 1: How easy was it to find Nossal Hub in Teams?
  ☐ Very easy (1-click)
  ☐ Easy (2-3 clicks)
  ☐ Hard (more than 3 clicks)
  ☐ Couldn't find it

Question 2: On a scale of 1-10, how likely are you to use the Hub regularly?
  ☐ 1 (not at all)
  ☐ 2-3 (unlikely)
  ☐ 4-6 (maybe)
  ☐ 7-8 (likely)
  ☐ 9-10 (very likely)

Question 3: Did Compass tiles open successfully (without re-login)?
  ☐ Yes, seamless
  ☐ Yes, but had to login again
  ☐ No, got error
  ☐ Didn't try

Question 4: Could you find [Policy] in the Knowledge Base?
  ☐ Yes, < 2 clicks
  ☐ Yes, 3-5 clicks
  ☐ Yes, but had to search
  ☐ No, couldn't find it

Question 5: Are the new Teams channels clear (8 channels instead of 25)?
  ☐ Yes, much clearer
  ☐ Yes, slightly clearer
  ☐ Same as before
  ☐ More confusing

Question 6: What's the #1 thing we should improve before launch?
  [Open text box]

Question 7: Any bugs or broken links?
  [Open text box]

Question 8: What would make this a 10/10 for you?
  [Open text box]
```

**Checklist:**
- [ ] Feedback form sent to pilot group
- [ ] Responses collected by Friday of Week 2
- [ ] Issues logged and prioritized
- [ ] Quick fixes made before launch (Mon-Tue of Week 3)

### Step 2.5: Fix Bugs + Polish (1 day)

**Owner:** IT Lead + SharePoint Admin  
**Based on pilot feedback:**

```
Common issues to watch for:
├─ Compass tiles not opening → Check SSO config
├─ Knowledge Base page loads slow → Check SharePoint indexing
├─ Teams app crash → Update Viva manifest
├─ Tabs don't appear in #resources → Check permissions
├─ Broken links → Update URL in ServiceCatalogue list
├─ Mobile app doesn't work → Test responsive design
└─ Users confused about old channels → Improve descriptions + lock them

For each issue:
1. Identify root cause
2. Fix (if quick)
3. Test
4. Document (for future reference)
```

**Checklist:**
- [ ] Top 3 bugs fixed
- [ ] Dashboard performance acceptable
- [ ] All links verified
- [ ] Mobile tested
- [ ] Ready for launch

---

## Phase 3: Launch Hub (Week 3 — Feb 27)

### Step 3.1: Release Day (Feb 27) — Morning

**Owner:** All teams (IT, Comms, Teams Admin)  
**Timeline:** 1-2 hours

**Sequence:**

**8:00 AM — Go/No-Go Meeting**
```
Attendees: IT Director, IT Lead, Comms Lead, Digital Transformation Lead, Teams Admin
Duration: 15 min

Agenda:
1. Pilot feedback summary (any blockers?)
2. QA sign-off (all systems green?)
3. Go decision
   ☐ Go → proceed to launch
   ☐ No-Go → delay launch (explain why)

Decision: ☐ GO ☐ NO-GO (if NO-GO, re-schedule for [DATE])
```

**9:00 AM — Soft Launch (Internal IT Team)**
```
Scope: 15 staff (IT team only, first hour)

Action:
1. Deploy Viva Connections app (Teams admin center)
2. Pin Nossal Hub for IT team (group-based deployment)
3. Wait 15 min for app distribution
4. IT team opens Teams → verifies Nossal Hub appears
5. IT team tests all tiles:
   ☐ Compass tiles (click, verify SSO works)
   ☐ KB pages (click, verify load)
   ☐ Power Apps placeholders (click, show "Coming soon")
6. Collect quick feedback (chat in Teams)
7. Log any last-minute issues

Success criteria:
✓ App appears in left rail for all 15 staff
✓ No immediate crashes
✓ Compass SSO works for at least 95%
✓ No critical errors in Teams admin logs

If issues: Fix while scope is small
```

**10:00 AM — Full Launch (All Staff)**
```
Action:
1. Deploy Viva Connections app to all staff (group-based deployment)
2. Teams admin: Pin Nossal Hub in left rail for all users
   → Users will see a blue notification: "New app pinned: Nossal Hub"
3. Wait 30 min for app distribution (some devices slower than others)
4. IT team monitors Teams admin center for errors
```

**10:30 AM — Comms Announcement**
```
Sender: Communications Team
Channel: #announcements in Staff of Nossal HS (moderated, so Comms can post)
Message:

---
🎉 WELCOME TO NOSSAL HUB! 🎉

Starting NOW, Nossal Hub is your single front door for:

✅ Compass (Attendance, Learning, Wellbeing, Events) — via SSO
✅ Request Something (IT, Maintenance, Access, Equipment, Purchase)
✅ Knowledge Base (Policies, How-To, Forms, Roles)

👉 OPEN TEAMS → Click "Nossal Hub" in the left rail

What changed:
📊 Your "Staff of Nossal HS" team is now streamlined (8 channels):
  • #announcements — school news
  • #daily-ops — urgent notices
  • #teaching-learning — curriculum chat
  • #wellbeing-support — pastoral updates
  • #events-programs — events
  • #staff-forum — general chat
  • #help-requests — tab menu (requests)
  • #resources — tab menu (knowledge)

Old channels (Policies, Forms, How-To, etc.) have moved to Nossal Hub.
They're still in Teams (read-only) with a link to the Hub.

❓ Questions?
→ Click "Help" in Nossal Hub
→ Email IT: [email]
→ Ask your team lead

Let's make this smooth. See you in the Hub!

P.S. On mobile? Nossal Hub works there too!
---
```

**11:00 AM — Send Email to All Staff**
```
From: IT Director
To: All Staff & Students
Subject: Welcome to Nossal Hub! Your new digital front door

Dear Nossal Community,

Starting today, we're launching Nossal Hub — a single front door for everything you need:

🔗 COMPASS SHORTCUTS (Attendance, Learning, Wellbeing, Events)
   → Just click and you're in (no re-login needed!)

📋 REQUEST SOMETHING (IT help, Maintenance, Access requests)
   → Easy forms instead of email

📚 KNOWLEDGE BASE (Policies, How-To, Forms, Roles & Responsibilities)
   → Searchable, organized, always up-to-date

HOW TO ACCESS:
1. Open Microsoft Teams
2. Look in the left rail for "Nossal Hub" (new!)
3. Click it
4. Explore the tiles

WHAT CHANGED IN TEAMS:
Your "Staff of Nossal HS" team is now simpler (8 channels instead of many):
• #announcements, #daily-ops, #teaching-learning, #wellbeing-support, #events-programs, #staff-forum, #help-requests, #resources

Old channels (Policies, Forms, etc.) are now in Nossal Hub.

QUESTIONS?
👉 Use the "Help" button in Nossal Hub
👉 Email IT: [email]
👉 Check the FAQ at the bottom of this email

Thanks for your patience as we modernize our digital workspace.
Let's make Nossal Hub amazing together!

— IT Team

---

FAQ (below)

Q: Where's the [Policy] channel?
A: It's now in Nossal Hub → Knowledge Base. Easier to find!

Q: Why are old channels locked?
A: They're archived to keep Teams clean. New content is in Nossal Hub.

Q: How do I request [something]?
A: Click #help-requests in Teams (has tabs) or Nossal Hub → Request Something

Q: Can I still see the old channels?
A: Yes, they're still there (locked/read-only). But everything new is in Nossal Hub.

Q: Will I lose access to old documents?
A: No! Old channel documents are now in Nossal Hub Knowledge Base. You'll have better access.

---
```

**Checklist for 9-11 AM:**
- [ ] Go/No-Go meeting completed (DECISION: GO ✓)
- [ ] Soft launch to IT team successful
- [ ] No critical blockers
- [ ] Viva app deployed to all staff
- [ ] #announcements post sent
- [ ] Email sent to all staff
- [ ] Initial monitoring shows no major errors
- [ ] IT team standing by for issues

### Step 3.2: Monitor & Support (Feb 27, Afternoon + Feb 28-Mar 6)

**Owner:** IT Lead + Support team  
**Duration:** First week daily check-ins, then as-needed

**Day 1 (Feb 27) — Afternoon**
```
Timeline: 2-5 PM

Actions:
├─ IT team monitors Teams admin logs for errors
├─ Monitor email for "I can't find..." or "This isn't working..." messages
├─ Respond to support requests within 2 hours
├─ Document issues in a shared tracker (OneNote or Excel)
├─ Daily standup: 4 PM — IT Lead reviews issues, prioritizes fixes
└─ End of day: Prepare for next day (any fixes needed overnight?)

Success target:
├─ < 10 support emails on day 1
├─ < 2 critical bugs
├─ 95%+ of staff can see Nossal Hub
└─ Compass SSO success rate 90%+
```

**Days 2-7 (Feb 28 - Mar 6) — Daily Standup**
```
Time: 9:00 AM daily
Duration: 15 min
Attendees: IT Lead, Digital Transformation Lead, Comms Lead (if comms issues)

Agenda:
1. New issues reported yesterday?
2. Fixes deployed?
3. Staff feedback (email, teams chat, NPS)?
4. Any pattern (e.g., "lots of people can't find X")?
5. What's the priority for today?

Tracking:
├─ Issue tracker (OneNote or Excel)
│  ├─ Issue
│  ├─ Status (New / In Progress / Fixed / Won't Fix)
│  ├─ Owner
│  └─ Due date
├─ Support request log
│  ├─ Issue description
│  ├─ Resolution
│  └─ Category (Compass SSO / KB not found / Broken link / etc.)
└─ NPS tracking
   ├─ Responses collected
   ├─ Average NPS score (target: 7+/10)
   └─ Common feedback
```

**Common Issues You'll Get (Prepare Responses):**

```
Issue: "I can't find Nossal Hub"
Solution:
  1. Open Teams
  2. Look in left rail (below "Chat")
  3. You might need to scroll down
  4. If not there, try restarting Teams or reloading browser
  5. If still not there, contact IT (might need admin deployment)

Issue: "Compass tile is asking me to login again"
Solution:
  1. Check if Compass SSO is working (IT: check SAML trust in Entra)
  2. User: Try incognito window (clears cache)
  3. User: Try logout + login to Teams
  4. IT: Verify user is in Compass (ask Compass admin)
  5. IT: Check Compass SAML logs

Issue: "Where did [Policy channel] go?"
Solution:
  1. It's not gone — it's locked (read-only)
  2. New content is in Nossal Hub
  3. Click the "Content Moved to Hub" tab in the old channel

Issue: "I can't find [Policy] in the Knowledge Base"
Solution:
  1. Try the search box at the top of Nossal Hub
  2. Or browse folders: Policies folder → Staff folder
  3. Still stuck? Email IT with the specific policy name

Issue: "[Power App tile] says Coming Soon"
Solution:
  1. That's correct — Power Apps launch in Sprint 2 (next month)
  2. For now, use #help-requests channel and email IT directly
```

**Checklist for First Week:**
- [ ] Daily standup completed
- [ ] < 20 support tickets total (for a school of ~1000 staff)
- [ ] Compass SSO success rate 95%+
- [ ] Zero critical bugs
- [ ] NPS feedback: 7+/10 average
- [ ] Staff can find policies/forms in < 3 clicks (80%+ self-service)
- [ ] No staff complaints about "can't do my job"
- [ ] Systems stable for Week 2 + beyond

### Step 3.3: Archive Legacy Channels (Feb 27, End of Day)

**Owner:** Teams Admin  
**Timeline:** After successful soft launch (mid-afternoon, ~2 PM)

**Action per legacy channel (e.g., #Policies, #Forms):**

```
For each old content channel:

1. Right-click channel → Edit channel
2. Moderation tab:
   ☐ Turn ON "Require member approval for posts"
   ☐ Add "Bypass moderation" members: [owners only]
   ☐ Result: Channel becomes read-only (members can't post)

3. Add migration tab:
   ☐ Click + (add tab)
   ☐ Choose "Web site"
   ☐ URL: https://nossal.sharepoint.com/sites/digitalhub/KB [relevant section]
   ☐ Display name: "Content Moved to Nossal Hub →"
   ☐ Pin it (move it to first tab)

4. Update description:
   ☐ Edit channel
   ☐ Description: 
      "This channel is now read-only. 
       New content is in Nossal Hub. 
       Click the 'Content Moved to Hub' tab above to access."

5. Post announcement in channel:
   ☐ "Good news! This content has moved to Nossal Hub for better organization.
       Click the 'Content Moved to Hub' tab to access it."
```

**Channels to archive on Day 1:**
```
✓ #Advice, handbooks & instructions
✓ #Careers and Pathways
✓ #Data (admin-only)
✓ #Events - Whole School
✓ #Forms and Templates
✓ #Human Resources
✓ #Occupational Health and Safety
✓ #Parent Communication
✓ #Policies
✓ #Reporting
✓ #Roles and Responsibilities
✓ #Strategic and Annual Planning
✓ #Wellbeing
```

**Checklist:**
- [ ] All 13 legacy channels locked (read-only)
- [ ] All 13 legacy channels have migration tab
- [ ] All 13 legacy channels have updated description
- [ ] Staff can still read old posts (just can't post new ones)
- [ ] Migration tabs link to correct Nossal Hub page

### Step 3.4: Delete Legacy Channels (Month 6)

**Owner:** Teams Admin  
**Timeline:** July 27, 2026 (6 months later)

**Decision point:** Have staff forgotten about old channels?
```
Check:
├─ Any new posts in last 2 months? (probably not, they're locked)
├─ Any complaints about deleted channels? (email IT)
├─ NPS feedback: Do staff like the new structure? (yes)
└─ Decision:
    ├─ If no complaints → Delete old channels (safe)
    └─ If complaints → Keep archived another month, reassess
```

**If proceed with deletion:**
```
For each legacy channel:
1. Right-click → Delete channel
2. Confirm deletion
3. Archive is complete (Teams keeps a backup for 30 days if needed)
```

---

## Success Checklist (End of Week 3)

- [ ] **Day 1:**
  - [ ] Nossal Hub visible in all staff Teams (left rail)
  - [ ] All old channels locked (read-only)
  - [ ] All locked channels have migration tab
  - [ ] Email + #announcements sent successfully
  - [ ] < 10 critical issues

- [ ] **Day 2-7:**
  - [ ] Daily standup completed
  - [ ] < 20 total support requests
  - [ ] Compass SSO working 95%+
  - [ ] Knowledge Base searchable and organized
  - [ ] NPS feedback 7+/10 (from UAT + launch week)
  - [ ] Staff can find "Policies" in < 3 clicks
  - [ ] Staff can submit request via #help-requests
  - [ ] Power Apps placeholders show "Coming soon"

- [ ] **End of Week 3:**
  - [ ] Portal is stable (no major outages)
  - [ ] Documentation complete (admin runbook, FAQ, etc.)
  - [ ] IT team trained on managing Hub + Teams
  - [ ] Transition is "smooth" (feedback is positive)
  - [ ] Ready for Sprint 2 (IT workflows launch)

---

## Rollback Plan (If Major Issues)

**If something is seriously broken during launch:**

```
Option 1: Soft Rollback (Same Day)
├─ Unpin Nossal Hub for users (Teams admin policy)
├─ Direct staff back to old Teams structure temporarily
├─ Legacy channels remain locked (staff can still access old content)
├─ Investigate issue
├─ Fix + re-launch next day

Option 2: Hard Rollback (If Critical)
├─ Delete Viva app
├─ Unlock legacy channels
├─ Admit we need more time
├─ Re-launch in 1 week (after fixes)
```

**Triggers for rollback:**
```
☐ Compass SSO broken (> 50% users getting login error)
☐ Knowledge Base completely inaccessible
☐ Teams crashes when opening Nossal Hub
☐ Data loss or security issue
☐ If > 100 support emails in first 2 hours
```

---

## Post-Launch (Week 4+)

### Ongoing Tasks

```
Weekly:
├─ Monitor Nossal Hub usage (Power BI dashboard, optional)
├─ Respond to support requests (< 24 hour response time)
└─ Update Knowledge Base (new policies, how-to guides, etc.)

Monthly:
├─ Review support request trends (any patterns?)
├─ Update Service Catalogue (new tiles, deactivate old ones)
├─ Maintain team permissions (leavers/joiners)
└─ Check SharePoint health (no errors in logs)

Quarterly:
├─ NPS survey (how satisfied are staff with Hub?)
├─ Add new pages to Knowledge Base (based on requests)
└─ Plan improvements for next quarter
```

### Success Metrics (30-Day Post-Launch)

| Metric | Target | Measure |
|--------|--------|---------|
| Hub adoption | 70%+ staff have opened it | Analytics/Power BI |
| Compass SSO success | 95%+ | Login audit logs |
| Knowledge Base usage | 50%+ of support questions self-resolved | Support ticket tracking |
| NPS score | 7+/10 | Survey responses |
| Support requests | < 30/month (staff self-serve via Hub) | Ticket system |
| Page load time | < 2 sec | Performance monitoring |
| Uptime | 99.9% | System monitoring |

---

## Questions Before Launch?

```
Contact:
├─ Architecture/design questions → Digital Transformation Lead
├─ Viva Connections/SharePoint questions → IT Lead
├─ Teams moderation/policies → Teams Admin
├─ Compass SSO issues → M365 admin + Compass admin
└─ Communications/change management → Comms Lead
```

---

**Document Status:** Ready to implement  
**Version:** 1.0  
**Last Updated:** January 29, 2026  
**Next Review:** After Phase 3 (post-launch, ~Mar 6)
