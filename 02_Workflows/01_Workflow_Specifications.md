# Nossal First 5 Workflows — Detailed Specifications

**Version:** 1.0  
**Date:** January 29, 2026  
**Scope:** High-volume, low-regret workflows built with A5-native tools (no premium connectors)

---

## Overview: Workflow Pattern

All 5 workflows follow the same architecture:

1. **User Entry Point** → Power App form (in Teams or SharePoint)
2. **Data Storage** → SharePoint List (request record)
3. **Routing & Triage** → Power Automate flow (reads RoutingTags from Service Catalogue)
4. **Team Notification** → Post to Teams channel (triage queue)
5. **Task Creation** → Create Planner task (assigned to owner team)
6. **Approval Gate** (if needed) → Approvals connector (manager, budget owner, etc.)
7. **Action & Status Updates** → IT/Facilities/Admin act, update list status
8. **Notification Loop** → Auto-email/Teams message on status change
9. **Closure & Archive** → Move to "Closed" status, retain per policy

---

## Workflow 1: IT Support Request (Tickets MVP)

### Purpose
User reports an IT issue. It's automatically triaged, assigned, and tracked.

### User Experience

**Where to start:**
- Viva Connections tile: "Report an IT Issue"
- Or: "Request Something" → "IT & Devices" → "Report an IT Issue"

**Entry point:** Power App (mobile-optimized)
- Quick form: Category (dropdown), Impact (choice), Description, Attachments
- "Submit" → auto-creates ticket, posts to IT triage channel, creates Planner task

### Data Model: ITRequests List

| Field Name | Type | Purpose | Validation |
|------------|------|---------|-----------|
| **Title** | Single Line Text | Auto-number: IT-2026-000001 | Formula: ="IT-"&YEAR(TODAY())&"-"&TEXT(ID,"000000") |
| **Requester** | Person | Who submitted | Required, defaults to current user |
| **RequesterEmail** | Email | For notifications | Auto-filled from Requester |
| **RequesterPhone** | Single Line Text | For escalation | Optional |
| **Location** | Lookup / Choice | Building/Room | Required, linked to Locations list |
| **Category** | Choice | Type of issue | Printing / Wi-Fi / Account / Classroom AV / Laptop / Other |
| **Subcategory** | Choice | More specific | (depends on Category) |
| **Impact** | Choice | Severity | Critical (whole class blocked) / High (individual blocked) / Medium / Low |
| **Description** | Multi-line Text | What's wrong? | Required, min 10 chars |
| **Attachments** | Attachment | Photos, screenshots, documents | Multiple files OK |
| **Status** | Choice | Workflow state | New → Triaged → In Progress → Waiting on User → Resolved → Closed |
| **TicketAge (days)** | Calculated | =TODAY()-Created | For SLA monitoring |
| **DateCreated** | Date | Auto-generated | Formula: TODAY() |
| **DateTriaged** | Date | When triaged | Auto-fill on transition to "Triaged" |
| **DateResolved** | Date | When resolved | Auto-fill on transition to "Resolved" |
| **DateClosed** | Date | When closed | Auto-fill on transition to "Closed" |
| **AssignedTo** | Person | IT tech owner | Set by triage flow |
| **TargetResolutionDate** | Date | SLA target | Formula: =DateCreated + (4 if Impact="Critical" else 1 day) |
| **SLAMet** | Yes/No | Did we meet SLA? | Formula: =(DateResolved < TargetResolutionDate) |
| **ResolutionNotes** | Multi-line Text | How it was fixed | Filled in by IT staff |
| **IsEscalated** | Yes/No | Flagged for senior tech? | Manual toggle |
| **RelatedTickets** | Lookup | Similar/linked tickets | Multi-select lookup to ITRequests |

### Power App: IT Support Form

**Form fields (in order):**
1. Category (required, dropdown) — triggers subcategory visibility
2. Subcategory (required, dynamic)
3. Impact (required, choice: Critical / High / Medium / Low)
4. Location (required, dropdown — pre-populate from user's department)
5. Description (required, rich text, min 10 chars, hint: "Be specific: what are you seeing?")
6. Attachments (optional, camera button for phone, upload for desktop)
7. Phone (optional, pre-fill from directory lookup)
8. [Submit] button

**On submit:**
- Validate all required fields
- Create ITRequests list item
- Call Power Automate flow: "IT Triage Flow"
- Show success: "Your ticket is IT-2026-000123. You'll get updates in Teams and email."

### Power Automate: IT Triage Flow

**Trigger:** When a Power App form submits to ITRequests list

**Actions:**

```
1. Read the ITRequests item that was just created
2. Look up RoutingTags from Service Catalogue:
   IF Subcategory = "Printing" → route to IT Printing channel
   IF Subcategory = "Wi-Fi" → route to IT Network channel
   IF Subcategory = "Account" → route to IT Accounts channel
   IF Subcategory = "Classroom AV" → route to IT AV Support channel
   ELSE → route to IT General Support channel

3. Post to appropriate Teams channel:
   Title: "New Ticket: IT-2026-000123"
   Body: 
   - Requester: [name]
   - Location: [building/room]
   - Impact: [severity]
   - Description: [text]
   - Ticket Link: [item URL]
   
4. IF Impact = "Critical":
   - Mention @ITLead to escalate immediately
   
5. Create Planner task in IT board:
   - Title: "IT-2026-000123: [brief description]"
   - Bucket: [category name]
   - Assigned to: IT tech on-call (lookup from ServiceCatalogue.OwnerTeam)
   - Due date: +4 hours (if Critical) or +1 day (standard)
   - Label: "Triage", "New"
   
6. Send email to Requester:
   Subject: "Your IT Ticket #IT-2026-000123"
   Body: "We've received your request. You'll get updates in Teams and email. Expected response: [SLA time]"
   
7. Update ITRequests item:
   - Status: "Triaged" (optional: set by on-call tech, or auto-set if not manual)
```

### Ongoing: Status Update Flows

**When IT tech updates Planner task or list status:**

Power Automate flow triggered: "ITRequest Status Changed"

```
IF Status changed TO "In Progress":
  - Email to Requester: "We're working on your ticket"
  - Post to Teams: "Ticket IT-2026-000123 is now In Progress"
  
IF Status changed TO "Waiting on User":
  - Email to Requester: "We need more info from you to proceed"
  - Planner label: "Waiting"
  
IF Status changed TO "Resolved":
  - Email to Requester: "Your ticket is resolved. Please reply if the issue is fixed or we need to reopen."
  - Set DateResolved = TODAY()
  - Calculate SLAMet = (DateResolved < TargetResolutionDate)
  - Post to IT channel: "Ticket IT-2026-000123 marked Resolved (SLA: MET/MISSED)"
  
IF Status changed TO "Closed":
  - Email to Requester: "Your ticket is closed. Please contact IT if you need anything else."
  - Move Planner task to "Closed" bucket
  - Retain list item per retention policy (7 years)
```

### Dashboard: IT Tickets

**Power BI dashboard (1 page, published to SharePoint):**
- Tile: Total open tickets (by category)
- Tile: Avg. time to triage (target: 2h)
- Tile: Avg. time to resolve (target: 1 day)
- Tile: % SLA Met (target: 85%+)
- Chart: Tickets by category (bar)
- Chart: Trend: tickets created per week (line)
- Chart: Response time distribution (histogram)

---

## Workflow 2: Maintenance Request (Facilities)

### Purpose
User reports a facility issue (plumbing, electrical, lock, cleaning, etc.). It's routed to Facilities, photo is captured, and tracked.

### User Experience

**Entry point:** Power App (mobile-optimized with camera)

**Form fields:**
1. Location (required, dropdown — building/room/area)
2. Issue Type (required, choice: Plumbing / Electrical / Lock / Cleaning / Carpentry / Other)
3. Urgency (required, choice: Urgent (safety/health) / High (blocks use) / Normal)
4. Description (required, rich text)
5. Photo(s) (required for Urgent, optional otherwise — camera integration)
6. Access Notes (optional, "Is the room open?" / "Key is with X")
7. [Submit]

### Data Model: MaintenanceRequests List

| Field | Type | Purpose |
|-------|------|---------|
| **Title** | Single Line Text | Auto-number: MR-2026-000001 |
| **Requester** | Person | Who reported it |
| **RequesterPhone** | Single Line Text | For callback |
| **Location** | Choice/Lookup | Building/Room |
| **IssueType** | Choice | Plumbing / Electrical / Lock / Cleaning / Carpentry / Other |
| **Urgency** | Choice | Urgent / High / Normal |
| **Description** | Multi-line Text | What's the issue? |
| **Photos** | Attachment | Multiple photos OK |
| **AccessNotes** | Single Line Text | When is the room accessible? |
| **Status** | Choice | New → Scheduled → In Progress → Completed → Closed |
| **DateCreated** | Date | Auto-generated |
| **DateScheduled** | Date | When Facilities scheduled it |
| **DateCompleted** | Date | When work was done |
| **AssignedTo** | Person | Facilities tech/team |
| **ContractorRequired** | Yes/No | Does this need external contractor? |
| **ContractorName** | Single Line Text | If yes, who? |
| **EstimatedCost** | Currency | For budget tracking |
| **ActualCost** | Currency | Final cost |
| **CompletionNotes** | Multi-line Text | What was done? |
| **FollowUpNeeded** | Yes/No | Is there a next step? |

### Power Automate: Maintenance Triage Flow

**Trigger:** MaintenanceRequests item created

```
1. Post to Facilities Teams channel:
   Title: "New Maintenance Request: MR-2026-000001"
   Body: [Location, IssueType, Urgency, Description]
   Attachment: [photos]
   
2. IF Urgency = "Urgent":
   - Mention @FacilitiesLead
   - Set Priority = High
   - Create Planner task with due date = TODAY()
   
3. ELSE:
   - Create Planner task with due date = TODAY() + 3 days
   
4. IF IssueType = "Electrical" AND Urgency = "Urgent":
   - Approval gate: FacilitiesLead approval required before proceeding
   - Add label to Planner: "Approval Pending"
   
5. IF ContractorRequired (manual flag):
   - Route to Facilities procurement queue
   - Create separate "Get Quotes" task
   - Pause main flow until approval
   
6. Email to Requester:
   Subject: "We've received your maintenance request MR-2026-000001"
   Body: "Location: [x]. Our team will address this by [target date]. You'll get updates here."
```

### Follow-up: Completion & Closure

When Facilities tech completes the work:
- Update Status → "Completed"
- Fill in CompletionNotes + ActualCost
- Power Automate flow triggers:
  - Email to Requester: "Your maintenance request is complete. Please contact us if follow-up is needed."
  - Post to Facilities channel: "MR-2026-000001 completed by [tech name]"
  - Move Planner task to "Closed"

---

## Workflow 3: Access Request / Permissions Change

### Purpose
User needs a permission or account change. It's approved by their manager/IT and actioned by IT.

### User Experience

**Entry point:** Power App

**Form fields:**
1. Request Type (required, choice):
   - Shared Mailbox Access
   - Teams Channel/Group Membership
   - Admin Rights (sensitive, approval gate)
   - SharePoint Library/Folder Access
   - Application Access
2. Resource Name (required, text — "Which mailbox / team / app?")
3. Reason (required, rich text — "Why do you need this?")
4. StartDate (required, date)
5. EndDate (optional, date — for temporary access)
6. RequesterManager (auto-lookup from Entra ID, read-only)
7. [Submit]

### Data Model: AccessRequests List

| Field | Type | Purpose |
|-------|------|---------|
| **Title** | Single Line Text | Auto-number: AR-2026-000001 |
| **Requester** | Person | User requesting access |
| **RequestType** | Choice | Mailbox / Team / Admin / SharePoint / App |
| **ResourceName** | Single Line Text | Which resource? |
| **Reason** | Multi-line Text | Why needed? |
| **StartDate** | Date | When access needed |
| **EndDate** | Date | Optional: when to revoke |
| **RequesterManager** | Person | Auto-lookup from Entra ID |
| **Status** | Choice | New → Awaiting Manager Approval → Awaiting IT Review → Approved → Actioned → Closed |
| **ManagerApprovedDate** | Date | When manager approved |
| **ITApprovedDate** | Date | When IT approved |
| **ActionedDate** | Date | When IT completed the action |
| **ITNotes** | Multi-line Text | How was it granted? (audit trail) |
| **SensitivityLevel** | Choice | Standard / Sensitive (for admin requests) |
| **IsTemporary** | Yes/No | Temporary or permanent? |

### Power Automate: Access Request Flow

**Trigger:** AccessRequests item created

```
1. Post to IT Approvals Teams channel:
   Title: "New Access Request: AR-2026-000001"
   Body: [Requester, RequestType, ResourceName, Reason, Manager]
   
2. Route to RequesterManager for approval:
   - Approvals connector
   - Message: "Can [Requester] have [RequestType] access to [ResourceName] for [reason]?"
   - Due: 2 days
   - On Approval: continue
   - On Rejection: Email requester "Your access request was not approved by your manager"
   
3. IF RequestType = "Admin Rights":
   - Route to IT Lead for secondary approval (security gate)
   - Create task in "Admin Approvals" queue
   - Due: 1 day
   
4. IF approved:
   - Create IT task: "Grant [RequesterName] access to [ResourceName]"
   - Assigned to: appropriate IT tech
   - Due: same day
   - Update Status → "Approved"
   
5. IT tech grants access (manual action in Azure AD / Teams / SharePoint)
   - Then updates AccessRequests status to "Actioned"
   
6. Power Automate on "Actioned" transition:
   - Email to Requester: "Your access to [resource] is now active."
   - Email to IT (audit): "Access granted: [Requester] → [Resource] on [Date]"
   - Add to audit log (list item retains full history)
   
7. IF IsTemporary = TRUE:
   - Create recurring reminder (Power Automate scheduled flow):
     - 1 week before EndDate: "Reminder: [Requester]'s access to [Resource] expires on [EndDate]"
     - On EndDate: "Revoke [Requester]'s access to [Resource]"
     - Require IT acknowledgment
```

---

## Workflow 4: Asset Loan / Equipment Booking

### Purpose
User borrows school equipment (projector, laptop, adapter, camera). It's tracked, due reminders sent, and returns managed.

### User Experience

**Entry point:** Power App

**Form fields:**
1. Asset Type (required, choice) — lookup from Assets list
2. Quantity (required, number)
3. StartDate (required, date)
4. ReturnDate (required, date)
5. Reason (required, text — "What will you use it for?")
6. [Submit]

### Data Model: AssetLoans List

| Field | Type | Purpose |
|-------|------|---------|
| **Title** | Single Line Text | Auto-number: AL-2026-000001 |
| **Borrower** | Person | Who's borrowing? |
| **AssetType** | Choice | Projector / Laptop / Adapter / Camera / Other |
| **AssetID** | Lookup | Specific asset (from Assets list) |
| **Quantity** | Number | How many? |
| **StartDate** | Date | Pickup date |
| **ReturnDate** | Date | Expected return date |
| **Reason** | Multi-line Text | Purpose of loan |
| **ActualReturnDate** | Date | When actually returned |
| **Status** | Choice | New → Checked Out → On Loan → Overdue → Returned → Closed |
| **CheckedOutBy** | Person | IT tech who handed it over |
| **ConditionOnCheckout** | Choice | Excellent / Good / Fair / Damaged |
| **ConditionOnReturn** | Choice | Excellent / Good / Fair / Damaged |
| **DamageNotes** | Multi-line Text | Any damage reported on return? |
| **ReplacementCost** | Currency | If damaged, cost to replace |

### Power Automate: Asset Loan Flow

**Trigger:** AssetLoans item created

```
1. Create reminder task in Planner: "[Borrower] has [AssetType] due [ReturnDate]"
   
2. Set up scheduled reminder flow:
   - 2 days before ReturnDate: Teams message to Borrower "Your equipment is due [Date]"
   - 1 day after ReturnDate (if not returned): Escalate to IT lead
   - 3 days overdue: Block further loans for this borrower (optional)
   
3. When Borrower returns equipment:
   - Update Status → "Returned"
   - IT tech inspects + updates ConditionOnReturn
   - IF ConditionOnReturn = "Damaged":
     - Flag for IT review
     - Create cost estimate task
   - Update ActualReturnDate = TODAY()
   
4. Power Automate on return:
   - Email to Borrower: "Thank you for returning [AssetType]. Condition recorded as [status]"
   - If damaged: "A damage report has been filed. You may be contacted for details."
   - Post to IT channel: "[Borrower] returned [AssetType]. Condition: [status]"
```

---

## Workflow 5: Purchase Request / Quote Approval

### Purpose
User/team needs to buy something. Request → manager approval → budget owner approval → procurement action.

### User Experience

**Entry point:** Power App

**Form fields:**
1. Item Description (required, text)
2. Quantity & Unit (required)
3. Estimated Unit Cost (required, currency)
4. Justification (required, rich text — "Why do we need this?")
5. Budget Code (required, lookup)
6. RequestingDepartment (auto-populate from user's Entra ID)
7. Quote Attachment (optional, PDF/image)
8. [Submit]

### Data Model: PurchaseRequests List

| Field | Type | Purpose |
|-------|------|---------|
| **Title** | Single Line Text | Auto-number: PR-2026-000001 |
| **Requester** | Person | Who's requesting? |
| **ItemDescription** | Multi-line Text | What do you want to buy? |
| **Quantity** | Number | How many? |
| **UnitCost** | Currency | Cost per unit |
| **TotalCost** | Calculated | =Quantity * UnitCost |
| **Justification** | Multi-line Text | Why is this needed? |
| **BudgetCode** | Lookup | Department budget code |
| **RequestingDepartment** | Lookup | Auto from Requester's dept |
| **Status** | Choice | New → Awaiting Manager Approval → Awaiting Budget Approval → Approved → Ordered → Delivered → Closed |
| **ManagerApprovedDate** | Date | When manager signed off |
| **BudgetOwnerApprovedDate** | Date | When budget owner signed off |
| **OrderedDate** | Date | When procurement placed order |
| **DeliveryDate** | Date | When item(s) arrived |
| **VendorName** | Single Line Text | Who we're buying from |
| **QuoteDocument** | Attachment | PDF of quote |
| **InvoiceNumber** | Single Line Text | For accounting |
| **Notes** | Multi-line Text | Any special requests? |

### Power Automate: Purchase Approval Flow

**Trigger:** PurchaseRequests item created

```
1. Post to Procurement Teams channel:
   Title: "New Purchase Request: PR-2026-000001"
   Body: [Item, Quantity, Total Cost, Justification, Department]
   
2. Route to Requester's Manager for approval:
   - Approvals connector
   - Message: "Can [Requester]'s department buy [Item] for $[TotalCost]? Reason: [Justification]"
   - Due: 3 days
   
3. IF Manager approved AND TotalCost < $500:
   - Auto-approve (skip budget owner, proceed to order)
   
4. IF Manager approved AND TotalCost >= $500:
   - Route to Budget Owner for approval
   - Approvals connector
   - Due: 2 days
   
5. IF either approval rejected:
   - Email to Requester: "Your purchase request was not approved."
   - Update Status → "Rejected"
   
6. IF both approvals received:
   - Update Status → "Approved"
   - Create task in Procurement queue: "Order [Item] from [Vendor] - Quote attached"
   - Assigned to: Procurement officer
   - Due: 1 day
   
7. Once Procurement orders:
   - Update Status → "Ordered"
   - Update OrderedDate + VendorName
   - Set delivery reminder for estimated arrival
   
8. Once delivered:
   - IT/Procurement updates: Status → "Delivered", DeliveryDate = TODAY()
   - Email to Requester: "Your item([s]) have arrived."
   - Post to Procurement channel: "PR-2026-000001 delivered"
```

---

## 6. Cross-Workflow Patterns

### Email Notifications (Standard Template)

All workflows use this format:

```
Subject: [Workflow Type] #[ID]: [Status Change]

Hi [Requester],

[Action]: Your [workflow] request [ID] has been [status change].

Details:
- Submitted: [date]
- Current Status: [status]
- Next Step: [what happens next]
- Expected Timeline: [when to expect resolution]

You can track your request here: [item link]

Questions? Contact [owner team] at [email/phone]

Best,
Nossal Digital Hub
```

### Planner Integration

All workflows create Planner tasks:
- Task title includes ticket ID
- Bucket matches workflow type (IT Support, Maintenance, Access, etc.)
- Due date = target resolution date
- Labels: New, Triage, In Progress, Waiting, Completed
- Assigned to: owner team member
- Description includes request details + requester contact info

### Status Transition Logic

All workflows follow this state machine:

```
New (just created)
  ↓
Triage / Initial Review (human touches it)
  ↓
Approval (if needed, e.g., manager sign-off)
  ↓
In Progress / Action (work is being done)
  ↓
Waiting on User / Hold (if info is needed, stalled, etc.)
  ↓
Completed / Resolved (work is done)
  ↓
Closed (archived, retention policy applies)
```

---

## 7. MVP Success Criteria

| Metric | Target | Workflow |
|--------|--------|----------|
| Time to triage | < 2h | IT, Maintenance, Access |
| Time to first resolution | 1 day (standard), 4h (critical) | IT |
| Manager approval time | < 2 days | Access, Purchase |
| Equipment return rate | 98% | Asset Loan |
| SLA attainment | 85%+ | All |
| User satisfaction | 7+/10 NPS | All |

---

**Ownership:** Digital Transformation Team + IT + Facilities + Admin  
**Last Updated:** January 29, 2026  
**Status:** Ready for Sprint 2 Implementation
