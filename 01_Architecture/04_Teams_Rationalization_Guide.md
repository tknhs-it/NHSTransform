# Nossal Teams Rationalization + Hub Launch (Practical Implementation Guide)

**Version:** 1.0  
**Date:** January 29, 2026  
**Context:** Apply Hub strategy to your existing Teams structure

---

## Current State (Your Screenshots)

### "Staff of Nossal HS" Team
**Problem:** Overloaded with content channels (~25 channels), acting as an intranet rather than workspace

| Current Channel | Type | Should Be |
|---|---|---|
| General | Discussion | Keep |
| Announcements | Broadcast | Keep (moderated) |
| Advice, handbooks & instructions | Content | → SharePoint Hub page |
| Careers and Pathways | Content | → SharePoint Hub page |
| Data | Content | → SharePoint Hub page |
| Forms and Templates | Content | → SharePoint Hub library |
| Human Resources | Content | → SharePoint Hub page |
| Policies | Content | → SharePoint Hub library |
| Reporting | Content | → SharePoint Hub page |
| Roles and Responsibilities | Content | → SharePoint Hub library |
| Staff Development | Discussion | Keep |
| Teaching and Learning | Discussion | Keep |
| Wellbeing | Discussion | Keep |
| **...and 12+ others** | Mixed | → Archive or move to Hub |

**Outcome:** Too many channels → users can't find things → friction.

### Service Teams (Good Separation)
- Staff IT Services / IT & Teams Support ✅
- Student Tech Support ✅
- Staff Facilities and Operations ✅
- Staff Communications Team ✅
- Staff Digital Development/ICT ✅
- Staff AI Team ✅

**These are right.** Keep them as service delivery teams (triage channels, task planning, internal work).

---

## Target State (After Hub Launch)

### "Staff of Nossal HS" Team (Rationalized)

**8 core channels:**

1. **#announcements** (Moderated: Comms team only)
   - Leadership updates, school-wide news
   - Policy: Only @Comms can post

2. **#daily-ops** (Moderated: Comms + Ops leads)
   - Daily notices, urgent alerts, building closures
   - Quick turnaround comms

3. **#teaching-learning** (Open discussion)
   - Curriculum ideas, resource sharing, teaching tips
   - Unmoderated, staff-driven

4. **#wellbeing-support** (Open discussion)
   - Pastoral updates, student support ideas, welfare checks
   - Owned by Student Services lead

5. **#events-programs** (Open discussion)
   - School events, excursions, co-curricular updates
   - Owned by Events coordinator

6. **#staff-forum** (Open discussion)
   - General staff chat, ideas, feedback, social
   - Unmoderated

7. **#help-requests** (Tabs only, no chat)
   - **Tabs:**
     - "Log IT Request" (link to IT Power App)
     - "Maintenance Request" (link to Maintenance form)
     - "Request Access" (link to Access Power App)
     - "Nossal Hub" (link to Viva app / SharePoint Home)
     - "Knowledge Base" (link to Hub KB)
   - **Purpose:** Single place staff go to "request something"
   - **No chat in this channel** (it's a menu)

8. **#resources** (Tabs + library)
   - **Tabs:**
     - "SharePoint Hub" (main library)
     - "Forms & Templates" (shared library)
     - "Policies" (shared library)
   - **Purpose:** Access to school resources
   - **No chat** (it's a directory)

### Legacy Channels → Archive

**Action:** Turn on moderation, lock, add "This content has moved" tab

```
#advice-handbooks → Pin tab: 
"This content is now in Nossal Hub. Click below to access."
→ Link to SharePoint page

#policies → Pin tab:
"All policies are now in the Policies library in Nossal Hub."
→ Link to library

#forms-templates → Pin tab:
"Forms and templates are in Nossal Hub > Resources > Forms & Templates."
→ Link

[Repeat for all content channels]
```

**Timeline:** Lock day 1 of Hub launch, delete after 6 months (if no one complains).

---

## Nossal Hub (Viva Connections) — The Front Door

### Dashboard Tiles (Service Catalogue-driven)

```
┌─────────────────────────────────────────────────────────────┐
│                    NOSSAL HUB (Viva Connections)            │
│                                                             │
│  COMPASS SHORTCUTS                                          │
│  ┌──────────────┬──────────────┬──────────────┐            │
│  │   Attendance │ Learning     │   Wellbeing  │            │
│  │     Mark     │   Tasks &    │  Student     │            │
│  │              │   Results    │  Profile     │            │
│  └──────────────┴──────────────┴──────────────┘            │
│  ┌──────────────┬──────────────┐                           │
│  │    Events    │  Parent      │                           │
│  │  Calendar    │  Comms       │                           │
│  └──────────────┴──────────────┘                           │
│                                                             │
│  REQUEST SOMETHING (M365 WORKFLOWS)                        │
│  ┌──────────────┬──────────────┬──────────────┐            │
│  │      IT      │ Maintenance  │    Access /  │            │
│  │    Support   │   Request    │  Permissions │            │
│  └──────────────┴──────────────┴──────────────┘            │
│  ┌──────────────┬──────────────┐                           │
│  │   Equipment  │   Purchase   │                           │
│  │     Loan     │   Request    │                           │
│  └──────────────┴──────────────┘                           │
│                                                             │
│  KNOWLEDGE & RESOURCES                                     │
│  ┌──────────────┬──────────────┬──────────────┐            │
│  │   Policies   │  How-To      │   Forms &    │            │
│  │   & Roles    │  Guides      │  Templates   │            │
│  └──────────────┴──────────────┴──────────────┘            │
│  ┌──────────────┐                                          │
│  │   Roles &    │                                          │
│  │ Responsiblts │                                          │
│  └──────────────┘                                          │
└─────────────────────────────────────────────────────────────┘
```

### SharePoint Home Site Structure

```
SharePoint Home Site (Nossal Digital Hub)
├─ Home Page
│  ├─ Welcome message
│  ├─ Quick links (Compass, IT, Facilities, etc.)
│  └─ Latest announcements
│
├─ Compass Shortcuts (Page)
│  ├─ Link to Compass Attendance
│  ├─ Link to Compass Learning Tasks
│  ├─ Link to Compass Wellbeing
│  ├─ Link to Compass Events
│  └─ Link to Compass Parent Comms
│
├─ Requests (Page)
│  ├─ "Request IT Help" tile → Power App
│  ├─ "Maintenance Request" tile → Power App
│  ├─ "Request Access" tile → Power App
│  ├─ "Equipment Loan" tile → Power App
│  └─ "Purchase Request" tile → Power App
│
├─ Knowledge Base (Library + Page)
│  ├─ Policies folder
│  ├─ How-To guides folder
│  ├─ Forms & Templates folder
│  ├─ Roles & Responsibilities folder
│  └─ Advice & Handbooks folder
│
├─ Resources (Library)
│  ├─ Forms folder (fillable PDFs, etc.)
│  ├─ Templates folder (lesson plans, reports, etc.)
│  └─ Shared documents folder
│
└─ Dashboards (Page)
   ├─ IT metrics (request volume, SLA)
   ├─ Maintenance metrics
   └─ Cross-system health
```

### Viva Connections Setup

**In Teams admin center:**
1. Enable Viva Connections for organization
2. Create Viva app pointing to SharePoint Home Site
3. Configure dashboard to pull from ServiceCatalogue list
4. Push "Nossal Hub" app to all staff (group-based deployment)

**Result:** Staff see "Nossal Hub" app in Teams left rail, click it, see dashboard tiles.

---

## Implementation Plan (3-Phase, Low Disruption)

### Phase 1: Build Hub (Behind Scenes, Week 1)

**Who:** SharePoint admin + IT Lead  
**Time:** 3-5 days  
**Do NOT announce yet**

- [ ] Create SharePoint Home Site
- [ ] Create ServiceCatalogue list (populate with Compass tiles + M365 tile placeholders)
- [ ] Create Knowledge Base library (move content from channel descriptions)
- [ ] Build Viva Connections dashboard
- [ ] Build Home page + navigation
- [ ] Test in DEV environment (invite 2-3 staff to test)
- [ ] Fix bugs / polish
- [ ] **Do not release yet**

### Phase 2: Prepare Staff of Nossal HS (Week 2, Dry Run)

**Who:** Teams admin + Comms lead  
**Time:** 1-2 days  
**Communicate:** Send email to staff: "Nossal Hub is coming Feb 27. Here's what changes."

- [ ] Create 8 new channels in "Staff of Nossal HS" (empty at first):
  - `#announcements` (moderated)
  - `#daily-ops` (moderated)
  - `#teaching-learning`
  - `#wellbeing-support`
  - `#events-programs`
  - `#staff-forum`
  - `#help-requests` (tabs only)
  - `#resources` (tabs only)

- [ ] Add tabs to #help-requests:
  - Nossal Hub app (link)
  - IT Request form (link to Power App)
  - Maintenance form (link)
  - Access request (link)

- [ ] Add tabs to #resources:
  - SharePoint hub (link)
  - Forms & Templates (link to library)
  - Policies (link to library)

- [ ] Test with small group (10 staff, 1 week)
  - Feedback: "Can you find what you need?"
  - Fix navigation issues

- [ ] **Do not archive legacy channels yet**

### Phase 3: Launch Hub + Transition (Week 3, Release Day)

**Who:** All teams  
**Time:** 1 day release + 1 week monitoring  
**Communication:** Launch announcement

**Release sequence (morning of Feb 27):**

1. **Pin Nossal Hub for all staff** (Teams admin policy)
   - Everyone sees it in their Teams left rail
   - Push notification: "Nossal Hub is now available"

2. **Email all staff** (Comms team)
   ```
   Subject: Welcome to Nossal Hub!
   
   Starting today, Nossal Hub is your single front door for:
   
   ✓ Compass shortcuts (Attendance, Learning, Wellbeing, Events)
   ✓ Request something (IT, Maintenance, Access, Equipment, Purchase)
   ✓ Knowledge & resources (Policies, How-To, Forms, Roles)
   
   In Teams, click "Nossal Hub" in the left rail.
   
   "Staff of Nossal HS" team is now streamlined:
   - #announcements — school news
   - #daily-ops — urgent notices
   - #teaching-learning — discussion
   - #help-requests — tab with links to request forms
   - [and 3 others]
   
   Old content channels (e.g., "Policies", "Forms") have moved to Nossal Hub.
   They're still in Teams (locked) with a tab pointing you to the Hub.
   
   Questions? Click Help in the Hub or email IT.
   ```

3. **Monitor for issues** (Week 1)
   - "Can't find X" → help staff navigate
   - "Why are old channels still there?" → explain archive plan
   - Collect NPS feedback (target: 7+/10)

4. **Archive legacy channels** (Friday of launch week, after stabilization)
   - Turn on moderation (owners only can post)
   - Lock all channels (members can read, not post)
   - Add a tab to each: "This content has moved to Nossal Hub →" with link
   - Post announcement: "This channel is now read-only. New content is in Nossal Hub."

5. **Delete legacy channels** (6 months later, if no complaints)
   - Archive is safer than delete (can recover)
   - Delete only once leadership confirms "no one is missing it"

---

## Channel Moderation (Announcement Policies)

### #announcements (High Control)

```
OWNER: Communications Team
POSTING POLICY: Only @Comms can post

How to enforce in Teams:
1. Right-click channel → Edit channel
2. Moderation tab → Toggle "Require member approval for posts"
3. Add members who can approve posts without waiting
4. Allow Comms team → bypass moderation
```

### #daily-ops (Medium Control)

```
OWNER: Communications + Operations
POSTING POLICY: Open posting, but pin important items

How to set it up:
1. Open description: "For urgent school notices. Important items pinned."
2. Encourage staff to use #staff-forum for non-urgent chat
```

### Others (Open)

```
No moderation, free chat
Just ask team members to be respectful
```

---

## Tab Setup (Quick Reference)

### #help-requests Channel Tabs

**Tab 1: Nossal Hub**
- Display name: "Nossal Hub"
- Link to: `https://nossal.sharepoint.com/sites/digitalhub/SitePages/Home.aspx`
- Icon: House

**Tab 2: Log IT Request**
- Display name: "Log IT Request"
- Link to: `https://apps.powerapps.com/play/[IT-APP-ID]`
- Icon: Wrench

**Tab 3: Request Maintenance**
- Display name: "Request Maintenance"
- Link to: `https://apps.powerapps.com/play/[MAINT-APP-ID]`
- Icon: Tools

**Tab 4: Request Access**
- Display name: "Request Access/Permissions"
- Link to: `https://apps.powerapps.com/play/[ACCESS-APP-ID]`
- Icon: Key

*(Add others as they launch in Sprint 2-3)*

**How to add a tab:**
1. Right-click channel name → Edit channel
2. Tabs section → Add a tab
3. Choose app or website
4. Paste URL, set display name
5. Save

---

## FAQ: Staff Questions You'll Get

### "Why are old channels locked?"

**Answer:**
> Content channels (Policies, Forms, etc.) weren't really "channels"—they're just content libraries. Nossal Hub is designed for that. They're locked to prevent confusion; you can still read them, and they link you to the Hub where all content is now maintained.

### "Where do I find [X policy/form/template]?"

**Answer:**
> Click Nossal Hub → Resources tab → [item type]. Everything is searchable and organized. If you can't find it, use the search bar in SharePoint (top of Hub) or email IT.

### "Can I request [X] via the Hub?"

**Answer:**
> If it's in the "Request Something" section of the Hub, yes. Click the tile, fill the form, and it goes straight to the right team. If it's not there, ask your manager or email IT.

### "I still use the old channels for my team. Can I keep them?"

**Answer:**
> If you manage a team (e.g., Digital Development team), keep your own channels as you like. We're only rationalizing the school-wide "Staff of Nossal HS" team. Your team's channels are yours.

---

## Success Checklist (Phase 3, End of Week 1)

- [ ] Nossal Hub visible in all staff's Teams left rail
- [ ] At least 50% of staff have opened the Hub
- [ ] Zero "access denied" errors (Viva permissions working)
- [ ] IT receives < 5 "where do I find X" emails (good onboarding)
- [ ] NPS feedback: 7+/10
- [ ] Legacy channels successfully locked + tabbed
- [ ] Zero critical bugs
- [ ] Staff understand new channel structure

---

## Integration with Sprint Plan

### Sprint 1 (Jan 29 - Feb 27)

**Add to deliverables:**

**1.7 Hub + Teams Rationalization** (4 days, replaces some of previous SharePoint setup)

- [ ] Phase 1: Build Hub (behind scenes) — 3 days
- [ ] Phase 2: Prepare Staff of Nossal HS — 1 day
- [ ] Phase 3: Launch (release day + 1 week monitoring) — included in "Release week"

**Effort:** Mostly already planned (SharePoint Home Site + Viva); just adds Teams channel cleanup.

---

## What This Does For You

| Benefit | Outcome |
|---------|---------|
| **Single front door** | Staff click Nossal Hub, see all requests, knowledge, Compass links in one place |
| **Reduced chaos in Teams** | Staff of Nossal HS is streamlined; easy to follow |
| **Content is maintained** | Knowledge Base in SharePoint (versioned, searchable) vs. buried in channels |
| **Compass isn't duplicated** | Links to Compass, not replacing it |
| **Workflows are clear** | "Request something" tiles vs. "email IT and hope" |
| **Safe transition** | Lock legacy channels first, delete later; no data loss |
| **Adoption is high** | Hub is frictionless; staff use it because it's where they already are (Teams) |

---

## Decisions You Need to Make

1. **Archive timeline:** Lock legacy channels day 1, delete after 6 months? (Recommended) Or delete immediately? (Riskier)

2. **#resources vs. #help-requests:** Should we keep separate tabs for Request forms + Resources, or combine? (Separate is cleaner)

3. **Announcement moderation:** Should only Comms post to #announcements? Or allow department heads? (Only Comms is safer)

4. **Compass tiles:** Should we show all Compass modules (attendance, learning, wellbeing, events, parent comms) or just the ones staff use most? (Show all, let staff ignore what they don't need)

5. **Hub branding:** Should Nossal Hub have a custom logo/color scheme, or use default? (Doesn't matter much, but custom is nicer)

---

**Status:** Ready to implement Phase 1  
**Owner:** SharePoint admin + IT Lead + Teams admin  
**Timeline:** Start immediately, launch Feb 27 with Portal Foundation  
**Risk:** Low (Hub is behind-the-scenes; no changes visible to staff until Feb 27)
