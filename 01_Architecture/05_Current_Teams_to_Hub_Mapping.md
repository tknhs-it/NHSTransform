# Nossal Teams Structure → Nossal Hub (Practical Mapping)

**Version:** 1.0  
**Date:** January 29, 2026  
**Purpose:** Show exactly what changes and where things move

---

## Current State (Your Screenshots)

### "Staff of Nossal HS" Team — Status Quo (Too Many Channels)

| Current Channel | Channel Type | Current Use |
|---|---|---|
| General | Discussion | General chat |
| Announcements | Broadcast | School news |
| Advice, handbooks & instructions | **Content** | How-to links, FAQs |
| Careers and Pathways | **Content** | Career info |
| City Week | Discussion | Event chat |
| Co-Curricular | Discussion | Activities coordination |
| Course Confirmation | Discussion | Subject selection help |
| Data | **Content** | Data reporting (admin) |
| Digital Development | Discussion | Tech updates |
| Diversions and Social Information | Discussion | Pastoral updates |
| Events - Whole School | **Content** | Event calendar |
| Forms and Templates | **Content** | Shared forms, documents |
| Human Resources | **Content** | HR policies, info |
| IRC | Discussion | Counselor updates |
| Journal Articles of Interest | Discussion | Reading list |
| Occupational Health and Safety | **Content** | OHS policies |
| Operations and Daily Orga... | Discussion | Ops notices |
| Parent Communication | **Content** | Comms templates |
| Policies | **Content** | School policies |
| Reporting | **Content** | Grade reporting help |
| Roles and Responsibilities | **Content** | Staff directory |
| Sport and Carnivals | Discussion | Sports updates |
| Staff Development | Discussion | PD opportunities |
| Staff Forum | Discussion | General chat (duplicate?) |
| Strategic and Annual Planning | **Content** | Planning docs |
| Swaps and Support | Discussion | Duty swaps, support requests |
| Teaching and Learning | Discussion | Curriculum ideas |
| Tutoral Teachers | Discussion | Year advisor coordination |
| VCE Teachers | Discussion | VCE-specific chat |
| Wellbeing | **Content** | Wellbeing resources |

**Problem:** ~13 content channels should NOT be channels. They clutter Teams and are hard to maintain.

---

## Transformation (Content Out, Hub In)

### What Moves to Hub (SharePoint)

| **Current Channel** | **New Location in Hub** | **Type** |
|---|---|---|
| Advice, handbooks & instructions | Knowledge Base → Advice & Handbooks page | Page + library |
| Careers and Pathways | Knowledge Base → Careers page | Page |
| Data | Admin portal → Data & Reporting | Page (admin-only) |
| Events - Whole School | Knowledge Base → Events calendar | Page |
| Forms and Templates | Resources library → Forms folder | Library |
| Human Resources | Knowledge Base → HR policies | Page |
| Occupational Health and Safety | Knowledge Base → OHS policies | Page |
| Parent Communication | Knowledge Base → Comms templates | Library |
| Policies | Knowledge Base → Policies folder | Library |
| Reporting | Knowledge Base → Grade reporting help | Page |
| Roles and Responsibilities | Knowledge Base → Roles & structure | Library |
| Strategic and Annual Planning | Admin portal → Strategic docs | Page (leadership-only) |
| Wellbeing | Knowledge Base → Wellbeing resources | Page |

**Result:** 13 content channels → 3 SharePoint pages + 5 libraries

---

## New Team Structure (After Rationalization)

### "Staff of Nossal HS" Team — Streamlined (8 Channels)

```
Staff of Nossal HS
├─ 📢 #announcements (moderated)
│  Purpose: Leadership updates, school-wide news
│  Owner: Communications Team
│
├─ 📋 #daily-ops (moderated)
│  Purpose: Urgent notices, daily alerts, building closures
│  Owner: Operations Lead
│
├─ 📚 #teaching-learning (open discussion)
│  Purpose: Curriculum ideas, resource sharing, teaching tips
│  Owner: Teaching & Learning Lead
│
├─ 💙 #wellbeing-support (open discussion)
│  Purpose: Pastoral updates, student support ideas, welfare
│  Owner: Student Services Lead
│
├─ 🎉 #events-programs (open discussion)
│  Purpose: School events, excursions, co-curricular updates
│  Owner: Events Coordinator
│
├─ 💬 #staff-forum (open discussion)
│  Purpose: General chat, ideas, feedback, social
│  Owner: Unmoderated / organic
│
├─ 🆘 #help-requests (tabs only, no chat)
│  Tabs:
│  ├─ Nossal Hub (link)
│  ├─ Log IT Request (Power App)
│  ├─ Maintenance Request (Power App)
│  ├─ Request Access (Power App)
│  ├─ Equipment Loan (Power App)
│  └─ Purchase Request (Power App)
│  Purpose: Single menu for all requests
│  Owner: IT Lead
│
└─ 📖 #resources (tabs only, no chat)
   Tabs:
   ├─ Nossal Hub (link)
   ├─ Forms & Templates (library)
   ├─ Policies (library)
   └─ Knowledge Base (link)
   Purpose: Directory for all resources
   Owner: IT Lead
```

### Service Teams (Keep As-Is)

```
Staff IT Services (or "Staff IT & Teams Support")
├─ General
├─ Issues & Requests (triage channel)
├─ Announcements
└─ [team-specific channels]

Student Tech Support
├─ General
├─ Requests
└─ [team-specific channels]

Staff Facilities and Operations
├─ General
├─ Maintenance (work orders)
├─ Announcements
└─ [team-specific channels]

Staff Communications Team
├─ General
├─ Drafts & Planning
├─ Social Media
└─ [team-specific channels]

Staff Digital Development/ICT
├─ General
├─ Projects
├─ Architecture & Design
└─ [team-specific channels]

Staff AI Team, Staff Pathways, AEU, ES
[Keep as-is — these are working teams]
```

---

## Nossal Hub (SharePoint Home Site) — The New Front Door

### Left Rail in Teams (What Staff See)

```
Teams
├─ [Favorites]
├─ Nossal Hub          ← NEW: Viva Connections app
├─ Staff of Nossal HS
├─ Staff IT Services
├─ Staff Facilities and Operations
├─ Staff Communications Team
├─ Staff Digital Development/ICT
└─ [Other teams...]
```

### Hub Dashboard (Viva Connections)

```
NOSSAL HUB (Home Page)
├─ Welcome + Quick Links
├─ Latest Announcements (pulled from #announcements)
│
├─ COMPASS SHORTCUTS (Tile group)
│  ├─ Mark Attendance
│  ├─ Learning Tasks & Results
│  ├─ Student Wellbeing / Profile
│  ├─ Events & Calendar
│  └─ Parent Communications
│
├─ REQUEST SOMETHING (Tile group)
│  ├─ Log IT Ticket (Power App)
│  ├─ Maintenance Request (Power App)
│  ├─ Request Access / Permissions (Power App)
│  ├─ Equipment Loan (Power App)
│  └─ Purchase Request (Power App)
│
├─ KNOWLEDGE BASE (Tile group)
│  ├─ Policies & Compliance
│  ├─ How-To Guides & Runbooks
│  ├─ Forms & Templates
│  ├─ Roles & Responsibilities
│  ├─ HR & Wellbeing
│  └─ OHS & Safety
│
└─ DASHBOARDS (Tile group)
   ├─ IT Metrics (request volume, SLA)
   ├─ Maintenance Metrics
   └─ System Health
```

### Sub-Pages in Hub

```
Nossal Hub (Home Site)
├─ Home (dashboard with tiles)
├─ Compass Shortcuts (Compass links + SSO info)
├─ Request Something (Power Apps gallery)
├─ Knowledge Base
│  ├─ Advice & How-Tos
│  ├─ Policies & Compliance
│  ├─ Forms & Templates
│  ├─ Roles & Responsibilities
│  ├─ HR & Wellbeing
│  ├─ OHS & Safety
│  └─ Career & Development
├─ Resources (library links)
├─ Data & Analytics (admin-only)
└─ System Status (admin-only)
```

---

## Content Migration Map (Specific Items)

### Advice, Handbooks & Instructions → Hub "Knowledge Base / Advice & How-Tos"

| Item | Current Location | New Location |
|---|---|---|
| Wi-Fi setup guide | Channel description | KB page "How to Connect to School Wi-Fi" |
| Printer setup | Channel description | KB page "How to Print" |
| AV equipment guide | Channel description | KB page "How to Use AV in Classrooms" |
| Email setup | Channel description | KB page "Email Setup Guide" |

### Forms and Templates → Hub "Resources / Forms & Templates"

| Item | Current Location | New Location |
|---|---|---|
| Leave request form | Pinned in channel | Forms library → Staff folder |
| Incident report form | Pinned in channel | Forms library → Admin folder |
| Lesson plan template | Pinned in channel | Templates library → Teaching folder |
| Budget request form | Pinned in channel | Forms library → Finance folder |

### Policies → Hub "Knowledge Base / Policies & Compliance"

| Item | Current Location | New Location |
|---|---|---|
| Code of Conduct | Pinned in #Policies | Policies library → Staff folder |
| Privacy Policy | Pinned in #Policies | Policies library → Compliance folder |
| Duty of Care | Pinned in #Policies | Policies library → Duty folder |
| IT Policy | Pinned in #Policies | Policies library → IT folder |

---

## What Happens to Old Channels (Safe Archive Plan)

### Day 1 of Hub Launch (Feb 27)

1. **Old content channels become read-only**
   ```
   Moderation ON (owners only can post)
   Description updated: 
   "This content has moved to Nossal Hub. 
    Click here to access → [link]"
   ```

2. **Add migration tab to each archived channel**
   ```
   Tab name: "Content Moved to Hub"
   Tab links to: Specific Hub page or library
   Example: #Policies → links to Hub "Policies & Compliance"
   ```

3. **Staff see locked channels with clear guidance**
   - Channel still visible (no surprise)
   - Can read old discussions (no data loss)
   - Cannot post (prevents sprawl)
   - Tab clearly points to Hub

### Month 1-3 (Feb 27 - Apr 27)

- Monitor for complaints ("I can't find X")
- Help staff navigate Hub
- No one should be trying to post to old channels

### Month 6 (July 27)

- **Decision point:** Have staff forgotten about old channels?
  - If yes → delete them (safe, no one using)
  - If no → keep them archived longer

---

## Staff Communication Plan

### Email (Launch Day)

```
Subject: Welcome to Nossal Hub! 🎉

Starting today, Nossal Hub is your single front door for:

✅ Compass (Attendance, Learning, Wellbeing, Events)
✅ Request Something (IT, Maintenance, Access, Equipment, Purchase)
✅ Knowledge Base (Policies, How-Tos, Forms, Roles)

HOW TO ACCESS:
1. Open Teams
2. Click "Nossal Hub" in the left rail (new!)
3. Explore the tiles

TEAMS CHANNELS HAVE CHANGED:
Your "Staff of Nossal HS" team now has just 8 channels:
- #announcements (school news)
- #daily-ops (urgent notices)
- #teaching-learning (curriculum chat)
- #wellbeing-support (pastoral updates)
- #events-programs (events chat)
- #staff-forum (general chat)
- #help-requests (tab menu for requests)
- #resources (tab menu for knowledge)

OLD CHANNELS (policies, forms, etc.) are now in Nossal Hub.
They're still in Teams (locked) with a link → Hub.

QUESTIONS?
👉 Click "Help" in Nossal Hub
👉 Email IT: [address]
👉 Ask your faculty/team lead

Let's make this smooth. See you in the Hub!
```

### FAQ Handout (Printed + in Hub)

```
Q: Where's the Policies channel?
A: Policies are now in Nossal Hub → Knowledge Base → Policies
   (Easier to find, version-controlled, searchable)

Q: Why are old channels locked?
A: They're archived to keep Teams clean. You can still read them,
   but new content is in Nossal Hub (single source of truth).

Q: How do I request something?
A: Click #help-requests in Teams (has tabs for all forms)
   OR go to Nossal Hub → "Request Something"

Q: What if I can't find something?
A: Use the search bar in Nossal Hub (top of page) or ask IT.

Q: Can my team keep our own channels?
A: Yes! This is just for "Staff of Nossal HS" team.
   Your working teams' channels stay as they are.
```

---

## Success Checklist (Day 1)

- [ ] Nossal Hub appears in all staff Teams (left rail)
- [ ] All old channels are locked (read-only)
- [ ] All locked channels have migration tab (points to Hub)
- [ ] Hub is responsive (loads < 2 sec)
- [ ] Compass tiles open via SSO (no re-login)
- [ ] Power Apps placeholders work (or show "Coming Soon")
- [ ] Staff can find Policies, Forms, Advice in Hub (< 2 clicks)
- [ ] IT receives < 5 "where is X?" emails
- [ ] NPS feedback: 7+/10 (from UAT)

---

## What This Achieves

| Benefit | Outcome |
|---|---|
| **Teams is usable again** | 8 channels instead of 25+; easy to follow |
| **Single front door** | Hub is where staff look first for anything |
| **Compass isn't duplicated** | Staff use Compass via SSO; M365 handles workflows only |
| **Content is maintained** | Hub pages are versioned; less "out of date" channel pins |
| **Safe transition** | Old channels still visible (no surprises); easy to undo |
| **IT workload ↓** | Less "where do I find X?" questions |
| **Adoption ↑** | Hub is in Teams (where staff already are) |

---

## Next Step (This Week)

1. **Show this map to:**
   - Comms team (they'll manage #announcements, #daily-ops)
   - IT Lead (they'll set up Hub, tabs, moderation)
   - Teaching & Learning Lead (they'll seed #teaching-learning)

2. **Decisions needed:**
   - Lock legacy channels day 1 or gradually? (Day 1 is cleaner)
   - Delete after 6 months or keep? (Keep first 6 months is safer)
   - Any channels we missed in the mapping? (Check with Comms)

3. **Timeline:**
   - Weeks 1-2 of Sprint 1: Build Hub + new channels (behind scenes)
   - Week 3 of Sprint 1: Launch (Feb 27)

---

**Document Status:** Ready for stakeholder review  
**Owner:** IT Lead + Digital Transformation Lead  
**Version Control:** See Git for history
