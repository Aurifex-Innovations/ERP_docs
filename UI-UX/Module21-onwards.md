# 🎯 MODULE 21: TASK MANAGEMENT

## Overview

Task Management module enables the **Technician Manager** to plan, assign, and track service tasks for technicians. Supports both **daily task allocation** and **monthly pre-planning**, primarily driven by Sales Order (SO) data. Includes conflict-free scheduling, technician assignment with primary/secondary roles, material tracking, and post-service completion logging.

**Module Connections:**

- **Depends on:** Module 20 (Sales Orders — primary task source), Module 19 (Contract — service schedule), Module 18 (Customer — client info), Module 17 (GMA — service details & materials), Module 11 (Stock — material deduction), Module 8 (Employee — technician list & roles)
- **Used by:** Module 20 Tab 2 (Execution & Delivery Status), Module 11 (Stock deduction on task completion), Module 18.3.3 (Service History Excel export)
- **Future Integration:** Customer Support Ticket module (Re-Task source)

---

The module contains the following screens:

- 21.1 Task Calendar Dashboard (Default)
- 21.2 Daily Task View (Date-click grid)
- 21.3 Add / Create Task (From SO / From Customer Tickets)
- 21.4 View Task Detail (Read-only)
- 21.5 Edit Task
- 21.6 Task Completion & Material Log
- 21.7 Reschedule / Reassign Task

---

================================================================================

# 21.1 Task Calendar Dashboard

**Description:**
The default landing screen for Module 21. Displays a **monthly calendar view** with task counts per day, colour-coded by status. Allows managers to quickly identify workload distribution, overdue tasks, and navigate to any date for detailed task management.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        TASK MANAGEMENT                                       │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                │  │
│  │                                                                        │  │
│  │ Branch     : [▼ All Branches ▼]                                       │  │
│  │ Technician : [🔍 Search Technician ▼]                                 │  │
│  │ Task Type  : [☑ Normal (SO) ☑ Re-Task (Ticket)]                      │  │
│  │ Status     : [☑ Pending ☑ In Progress ☑ Completed ☑ Overdue]        │  │
│  │                                                                        │  │
│  │ Search: [________________________] (Task ID / Customer / SO Number)   │  │
│  │                                                 [Reset Filters]       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │  [◄ Prev Month]         MARCH 2026          [Next Month ►]           │  │
│  │                                                                       │  │
│  │  MON    TUE    WED    THU    FRI    SAT    SUN                        │  │
│  │  ┌──────┬──────┬──────┬──────┬──────┬──────┬──────┐                   │  │
│  │  │      │      │      │      │      │      │  1   │                   │  │
│  │  │      │      │      │      │      │      │ 2📋  │                   │  │
│  │  ├──────┼──────┼──────┼──────┼──────┼──────┼──────┤                   │  │
│  │  │  2   │  3   │  4   │  5   │  6   │  7   │  8   │                   │  │
│  │  │ 5📋  │ 3📋  │ 4📋  │ 6📋  │ 2📋  │ 1📋  │      │                   │  │
│  │  ├──────┼──────┼──────┼──────┼──────┼──────┼──────┤                   │  │
│  │  │  9   │ 10   │ 11   │ 12   │ 13   │ 14   │ 15   │                   │  │
│  │  │ 4📋  │ 🔴3  │ 5📋  │ 3📋  │ 4📋  │      │      │                   │  │
│  │  ├──────┼──────┼──────┼──────┼──────┼──────┼──────┤                   │  │
│  │  │ 16   │ 17   │ 18   │ 19   │ 20   │ 21   │ 22   │                   │  │
│  │  │ 3📋  │ 5📋  │ 4📋  │ 2📋  │ 6📋  │ 1📋  │      │                   │  │
│  │  ├──────┼──────┼──────┼──────┼──────┼──────┼──────┤                   │  │
│  │  │ 23●  │ 24   │ 25   │ 26   │ 27   │ 28   │ 29   │                   │  │
│  │  │ 4📋  │ 3📋  │ 5📋  │ 2📋  │ 4📋  │ 1📋  │      │                   │  │
│  │  ├──────┼──────┼──────┼──────┼──────┼──────┼──────┤                   │  │
│  │  │ 30   │ 31   │      │      │      │      │      │                   │  │
│  │  │ 3📋  │ 2📋  │      │      │      │      │      │                   │  │
│  │  └──────┴──────┴──────┴──────┴──────┴──────┴──────┘                   │  │
│  │                                                                       │  │
│  │  Legend: ● Today  📋 Tasks  🔴 Overdue  ✅ All Completed              │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ⚠️ OVERDUE ALERTS                                                          │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ 🔴 10 Mar — 3 tasks overdue (TASK-2026-0088, TASK-2026-0089, ...)      ││
│  │ 🔴 08 Mar — 1 task overdue (TASK-2026-0071)                            ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  MONTHLY SUMMARY                                                             │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ Total Tasks: 85  │ Completed: 42  │ In Progress: 18  │ Pending: 21     ││
│  │ Overdue: 4       │ Re-Tasks: 6    │ Avg Tasks/Day: 3.4                 ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Calendar Interaction

| Action                   | Result                                                        |
| ------------------------ | ------------------------------------------------------------- |
| **Click on a date cell** | Navigates to **Daily Task View (21.2)** for that date        |
| **Click on overdue alert** | Navigates to the overdue date's Daily Task View             |
| **Hover on date cell**   | Shows tooltip: task count breakdown by status                |

---

## Filters

| Filter     | Type         | Options                                                       |
| ---------- | ------------ | ------------------------------------------------------------- |
| Branch     | Dropdown     | All Branches / Specific Branch (from Module 7)               |
| Technician | Search       | Search by technician name (from Module 8)                    |
| Task Type  | Multi-select | Normal (SO) / Re-Task (Ticket)                               |
| Status     | Multi-select | Pending / In Progress / Completed / Overdue                  |
| Search     | Text         | Free text search (Task ID / Customer / SO Number)             |

---

## Monthly Summary Fields

| Field           | Type    | Description                                          |
| --------------- | ------- | ---------------------------------------------------- |
| Total Tasks     | Number  | Count of all tasks in the displayed month            |
| Completed       | Number  | Tasks with status = Completed                        |
| In Progress     | Number  | Tasks currently being executed                       |
| Pending         | Number  | Tasks not yet started                                |
| Overdue         | Number  | Past-date tasks still not completed                  |
| Re-Tasks        | Number  | Tasks created from customer complaints/tickets       |
| Avg Tasks/Day   | Number  | Average daily task load for the month                |

---

## Form Actions

| Action                     | Description                                               |
| -------------------------- | --------------------------------------------------------- |
| **+ Add Task**             | Opens the **Add / Create Task Form** (Screen 21.3)        |

---

================================================================================

# 21.2 Daily Task View

**Description:**
When a user clicks on a specific date in the Calendar Dashboard, this screen opens showing all tasks for that day in a **table/grid format**. Displays time slots, assigned technicians, task details, and status. Supports one-to-many (one technician → multiple tasks) and many-to-one (multiple technicians → one task) relationships.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Calendar]        TASKS FOR: 23 MARCH 2026 (Monday)              │
│                                                        [+ Add Task]         │
│                                                                              │
│  TASK GRID                                                                   │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Task ID       │Time Slot      │Customer       │Service        │Site       ││
│  │──────────────┼───────────────┼───────────────┼───────────────┼───────────││
│  │TASK-2026-0201│08:00 – 10:00  │ABC Corporation│Cockroach Treat│Head Office││
│  │TASK-2026-0202│10:30 – 12:00  │XYZ Hotel      │Rodent Control │Lobby      ││
│  │TASK-2026-0203│14:00 – 16:00  │PQR Foods      │Termite Control│Warehouse  ││
│  │TASK-2026-0204│16:30 – 18:00  │ABC Corporation│General Pest   │Showroom   ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Primary Tech   │Support Techs    │Type     │Status       │Actions        ││
│  │───────────────┼─────────────────┼─────────┼─────────────┼───────────────││
│  │Ravi S.        │Anjali M.        │Normal   │✅ Completed │[View]         ││
│  │Suresh K.      │—                │Normal   │🕐 In Progrs │[View] [Edit]  ││
│  │Ravi S.        │Amit T., Priya D.│Re-Task  │⏳ Pending   │[View] [Edit]  ││
│  │Anjali M.      │Suresh K.        │Normal   │⏳ Pending   │[View] [Edit]  ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  DAILY SUMMARY                                                               │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ Total: 4  │ Completed: 1  │ In Progress: 1  │ Pending: 2  │ Overdue: 0 ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  TECHNICIAN AVAILABILITY                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ Ravi S.     │ ██████░░░░░░░░░░  2 tasks │ Free: 12:00 – 14:00          ││
│  │ Anjali M.   │ ████░░░░░░░░░░░░  2 tasks │ Free: 10:00 – 16:30          ││
│  │ Suresh K.   │ ████░░░░░░░░░░░░  2 tasks │ Free: 08:00 – 10:30          ││
│  │ Amit T.     │ ██░░░░░░░░░░░░░░  1 task  │ Free: 08:00 – 14:00          ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Legend: ⏳ Pending  🕐 In Progress  ✅ Completed  🔴 Overdue               │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Task Grid Fields

| Field            | Type    | Description                                                        |
| ---------------- | ------- | ------------------------------------------------------------------ |
| Task ID          | Link    | System-generated unique ID (e.g., TASK-2026-0201); opens 21.4    |
| Time Slot        | Display | Scheduled start – end time                                        |
| Customer         | Display | Customer name from Module 18                                      |
| Service          | Display | Service type (e.g., Cockroach Treatment)                          |
| Site             | Display | Site name and location                                            |
| Primary Tech     | Display | Main responsible technician                                       |
| Support Techs    | Display | Additional assigned technicians (comma-separated)                 |
| Type             | Badge   | Normal / Re-Task                                                  |
| Status           | Badge   | Pending / In Progress / Completed / Overdue                       |
| Actions          | Buttons | [View] [Edit] [Reschedule] [Complete]                             |

---

## Actions (Table Row)

| Action           | Condition                          | Description                                       |
| ---------------- | ---------------------------------- | ------------------------------------------------- |
| **View**         | All statuses                       | Opens Task Detail (Screen 21.4)                  |
| **Edit**         | Status = Pending / In Progress     | Opens Edit Task form (Screen 21.5)               |
| **Reschedule**   | Status = Pending                   | Opens Reschedule / Reassign form (Screen 21.7)   |
| **Complete**     | Status = In Progress               | Opens Task Completion form (Screen 21.6)         |

---

## Conflict Detection

| Rule                                 | Behaviour                                                   |
| ------------------------------------ | ----------------------------------------------------------- |
| Overlapping time slots              | System highlights conflict in red; blocks assignment         |
| Same technician, same time          | Warning: "Ravi S. is already assigned to TASK-2026-0201"    |
| Tech availability exhausted         | Greyed out in Technician Availability panel                  |

---

================================================================================

# 21.3 Add / Create Task

**Description:**
A multi-step form to create service tasks. Opens with **two tabs** — tasks sourced from Sales Orders (Normal) and tasks sourced from Customer Support Tickets (Re-Task). The user selects the source, picks service line items, assigns technicians via a **Role → Employee cascading dropdown** (multi-select), and schedules the task.

---

## Screen Layout (Tab 1: From Sales Order)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Daily View]          CREATE TASK          Date: 23 Mar 2026     │
│                                                                              │
│  [Tab 1: From Sales Order (SO) ●]    [Tab 2: From Customer Tickets]         │
│                                                                              │
│  STEP 1: SELECT SALES ORDER & SERVICE LINE ITEMS                            │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Branch*           : [▼ Mumbai ▼]                                       │ │
│  │  Search SO         : [🔍 SO Number / Customer Name ▼]                   │ │
│  │                                                                         │ │
│  │  AVAILABLE SERVICE LINE ITEMS (from selected SO)                        │ │
│  │  ┌──────────────────────────────────────────────────────────────────┐   │ │
│  │  │☑│SO Number    │Customer       │Service          │Site    │Freq   │   │ │
│  │  │─┼─────────────┼───────────────┼─────────────────┼────────┼───────│   │ │
│  │  │☑│SO-2026-0112 │ABC Corporation│Cockroach Treat. │Head Off│Weekly │   │ │
│  │  │☐│SO-2026-0112 │ABC Corporation│Termite Control  │Head Off│Qrtly  │   │ │
│  │  │☑│SO-2026-0087 │XYZ Hotel      │Rodent Control   │Lobby   │Monthly│   │ │
│  │  └──────────────────────────────────────────────────────────────────┘   │ │
│  │  Selected: 2 service line items                                         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  STEP 2: SCHEDULE, ASSIGN & MATERIALS (per selected item)                   │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  ── TASK 1: Cockroach Treatment @ Head Office (SO-2026-0112) ──         │ │
│  │  ┌──────────────────────────────────────────────────────────────────┐   │ │
│  │  │  Category          : General Pest Control                         │   │ │
│  │  │  Sub-category      : Cockroach Management                        │   │ │
│  │  │  Scheduled Date*   : [📅 23 Mar 2026]                            │   │ │
│  │  │  Start Time*       : [▼ 08:00 ▼]                                 │   │ │
│  │  │  End Time*         : [▼ 10:00 ▼]                                 │   │ │
│  │  │  Duration          : 2 hours (auto-calculated)                    │   │ │
│  │  │  Area (SQFT)       : 3,500                                       │   │ │
│  │  │                                                                  │   │ │
│  │  │  ── Technician Assignment ──                                     │   │ │
│  │  │  Role*             : [▼ Select Role ▼]                            │   │ │
│  │  │                      • Technician                                │   │ │
│  │  │                      • Senior Technician                         │   │ │
│  │  │                      • Supervisor                                │   │ │
│  │  │  Available Employees: [☑ Ravi S. ☑ Anjali M. ☐ Suresh K.]       │   │ │
│  │  │  (Multi-select — cascading from Role; shows only free techs)     │   │ │
│  │  │  Primary Technician*: [▼ Ravi S. ▼]                              │   │ │
│  │  │                                                                  │   │ │
│  │  │  ⚠ Conflict Check: ✅ No conflicts detected                     │   │ │
│  │  │                                                                  │   │ │
│  │  │  ── Materials / Chemicals (Auto-fetched from GMA/SO) ──          │   │ │
│  │  │  ┌──────────────────┬───────┬──────┬──────────┬────────┐         │   │ │
│  │  │  │Chemical / Product│ HSN   │ UOM  │ Std Qty  │Req Qty*│         │   │ │
│  │  │  ├──────────────────┼───────┼──────┼──────────┼────────┤         │   │ │
│  │  │  │Alpha Cypermethrin│ 3808  │ ml   │ 120      │[120__] │         │   │ │
│  │  │  │Fipronil Gel      │ 3808  │ tube │ 2        │[2____] │         │   │ │
│  │  │  │Bait Station      │ 3926  │ nos  │ 4        │[4____] │         │   │ │
│  │  │  └──────────────────┴───────┴──────┴──────────┴────────┘         │   │ │
│  │  │  [+ Add Material]  [🗑 Remove Selected]                          │   │ │
│  │  │  (Std Qty = from GMA/SO; Req Qty = editable by manager)          │   │ │
│  │  └──────────────────────────────────────────────────────────────────┘   │ │
│  │                                                                         │ │
│  │  ── TASK 2: Rodent Control @ Lobby (SO-2026-0087) ──                    │ │
│  │  ┌──────────────────────────────────────────────────────────────────┐   │ │
│  │  │  Category          : Specialized Service                         │   │ │
│  │  │  Sub-category      : Rodent Management                           │   │ │
│  │  │  Scheduled Date*   : [📅 23 Mar 2026]                            │   │ │
│  │  │  Start Time*       : [▼ 10:30 ▼]   End Time*: [▼ 12:00 ▼]       │   │ │
│  │  │  Role*             : [▼ Technician ▼]                             │   │ │
│  │  │  Available Employees: [☑ Suresh K. ☐ Amit T.]                    │   │ │
│  │  │  Primary Technician*: [▼ Suresh K. ▼]                            │   │ │
│  │  │  ⚠ Conflict Check: ✅ No conflicts detected                     │   │ │
│  │  │                                                                  │   │ │
│  │  │  ── Materials / Chemicals (Auto-fetched from GMA/SO) ──          │   │ │
│  │  │  ┌──────────────────┬───────┬──────┬──────────┬────────┐         │   │ │
│  │  │  │Chemical / Product│ HSN   │ UOM  │ Std Qty  │Req Qty*│         │   │ │
│  │  │  ├──────────────────┼───────┼──────┼──────────┼────────┤         │   │ │
│  │  │  │Bromadiolone Cake │ 3808  │ nos  │ 6        │[6____] │         │   │ │
│  │  │  │Glue Trap Board   │ 3926  │ nos  │ 10       │[10___] │         │   │ │
│  │  │  └──────────────────┴───────┴──────┴──────────┴────────┘         │   │ │
│  │  │  [+ Add Material]  [🗑 Remove Selected]                          │   │ │
│  │  └──────────────────────────────────────────────────────────────────┘   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  STEP 3: ADDITIONAL NOTES                                                    │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Task Notes    : [Special care for server room area______________]     │ │
│  │  Priority*     : [▼ Normal ▼]  (Normal / Urgent / Critical)           │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [CREATE TASKS]                                       [CANCEL]         │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Screen Layout (Tab 2: From Customer Tickets — Re-Task)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Tab 1: From Sales Order (SO)]    [Tab 2: From Customer Tickets ●]         │
│                                                                              │
│  STEP 1: SELECT SOURCE                                                      │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Source Type       : (•) Customer Ticket   ( ) Manual Entry             │ │
│  │  Search Ticket     : [🔍 Ticket ID / Customer ▼]                       │ │
│  │  Linked SO (if any): SO-2026-0087 (Auto-filled, Read-only)             │ │
│  │  Customer          : XYZ Hotel (Auto-filled)                            │ │
│  │  Category          : General Pest Control                               │ │
│  │  Sub-category      : Cockroach Management                               │ │
│  │  Original Service  : Rodent Control (Auto-filled)                       │ │
│  │  Site              : Lobby (Auto-filled)                                │ │
│  │  Complaint Reason  : "Rodents seen again within 2 days" (Read-only)    │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  STEP 2: TASK DETAILS (Editable)                                            │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Service Type*     : [▼ Rodent Control ▼] (Pre-filled; editable)       │ │
│  │  Site*             : [▼ Lobby ▼] (Pre-filled; editable)                │ │
│  │  Scheduled Date*   : [📅 24 Mar 2026]                                   │ │
│  │  Start Time*       : [▼ 09:00 ▼]    End Time*: [▼ 11:00 ▼]            │ │
│  │                                                                         │ │
│  │  ── Technician Assignment ──                                            │ │
│  │  Role*             : [▼ Senior Technician ▼]                            │ │
│  │  Available Employees: [☑ Ravi S. ☐ Anjali M.]                          │ │
│  │  Primary Technician*: [▼ Ravi S. ▼]                                    │ │
│  │                                                                         │ │
│  │  ── Materials / Chemicals (Auto-fetched from Original SO/Service) ──    │ │
│  │  ┌──────────────────┬───────┬──────┬──────────┬────────┐               │ │
│  │  │Chemical / Product│ HSN   │ UOM  │ Std Qty  │Req Qty*│               │ │
│  │  ├──────────────────┼───────┼──────┼──────────┼────────┤               │ │
│  │  │Bromadiolone Cake │ 3808  │ nos  │ 6        │[6____] │               │ │
│  │  │Glue Trap Board   │ 3926  │ nos  │ 10       │[10___] │               │ │
│  │  └──────────────────┴───────┴──────┴──────────┴────────┘               │ │
│  │  [+ Add Material]  [🗑 Remove Selected]                                │ │
│  │  (Auto-filled from linked SO service; Qty is editable)                  │ │
│  │                                                                         │ │
│  │  Task Notes    : [Re-service due to complaint___]                      │ │
│  │  Priority*     : [▼ Urgent ▼]                                          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [CREATE RE-TASK]                                     [CANCEL]         │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Step 1: Source & Service Selection

| Field               | Type      | Required | Description                                     |
| ------------------- | --------- | -------- | ----------------------------------------------- |
| Branch              | Dropdown  | Yes      | Select servicing branch                         |
| Search SO           | Search    | Yes      | Search by SO Number or Customer Name            |
| Service Grid        | Grid      | Yes      | Select line items (SO, Customer, Service, Site, Freq) |

---

## Step 2: Schedule & Assignment Fields

| Field               | Type      | Required | Validation                              | Description                                     |
| ------------------- | --------- | -------- | --------------------------------------- | ----------------------------------------------- |
| Category            | Display   | Auto     | From GMA/SO service definition          | Service Category (e.g. General Pest Control)    |
| Sub-category        | Display   | Auto     | From GMA/SO service definition          | Service Sub-category (e.g. Cockroach Mgmt)      |
| Area (SQFT)         | Display   | Auto     | From GMA/SO service definition          | Total area to be treated (read-only)            |
| Scheduled Date      | Date      | Yes      | Cannot be past date                     | Task execution date                             |
| Start Time          | Time      | Yes      | Must be before End Time                 | Task start time                                 |
| End Time            | Time      | Yes      | Must be after Start Time                | Task end time                                   |
| Duration            | Display   | Auto     | Auto-calculated                         | End Time – Start Time                           |
| Role                | Dropdown  | Yes      | From Module 8 role definitions          | Filters the employee list (cascading)           |
| Available Employees | Multi-sel | Yes      | At least one must be selected           | Cascading from Role; shows only available techs |
| Primary Technician  | Dropdown  | Yes      | Must be one of selected employees       | Main responsible person for the task            |
| Conflict Check      | Indicator | Auto     | System checks tech schedules            | Visual indicator (✅ No conflicts / ⚠ Conflict) |
| Task Notes          | Textarea  | No       | Max 500 chars                           | Additional instructions (Step 3)                |
| Priority            | Dropdown  | Yes      | Normal / Urgent / Critical              | Task priority level (Step 3)                    |

---

## Materials / Chemicals Fields (Auto-Fetched, Editable)

> **Data Source:** When a SO line item is selected (Tab 1) or a service is identified from a ticket (Tab 2), the system auto-fetches the chemical/product list from the **GMA Sheet (Module 17)** and **SO line items (Module 20)**. HSN, UOM, and Standard Qty are pre-filled; the manager can **edit Req Qty, add new rows, or remove rows**.

| Field               | Type      | Required | Validation                             | Description                                     |
| ------------------- | --------- | -------- | -------------------------------------- | ----------------------------------------------- |
| Chemical / Product  | Display   | Auto     | From GMA/SO service definition         | Auto-fetched chemical or product name           |
| HSN                 | Display   | Auto     | From Module 10 master data             | HSN/SAC code (read-only)                        |
| UOM                 | Display   | Auto     | From Module 10 master data             | Unit of measurement (read-only)                 |
| Std Qty             | Display   | Auto     | From GMA/SO                            | Standard quantity per service (read-only)       |
| Req Qty             | Number    | Yes      | Must be ≥ 0; numeric only             | Editable — manager can adjust per task need    |
| + Add Material      | Button    | —        | Chemical must exist in Module 10/11    | Manually add extra materials not in auto-list   |
| 🗑 Remove Selected  | Button    | —        | Cannot remove if only 1 row remains    | Remove unnecessary materials from the list      |

---

## Re-Task Specific Fields

| Field               | Type      | Required | Description                                     |
| ------------------- | --------- | -------- | ----------------------------------------------- |
| Source Type         | Radio     | Yes      | Customer Ticket / Manual Entry                  |
| Ticket ID           | Search    | Cond.    | Required if Source = Customer Ticket            |
| Linked SO           | Display   | Auto     | Original SO reference (read-only)               |
| Customer            | Display   | Auto     | Customer Name from linked source                |
| Category            | Display   | Auto     | Service Category from source service            |
| Sub-category        | Display   | Auto     | Service Sub-cat from source service             |
| Original Service    | Display   | Auto     | The service name linked to the ticket           |
| Site                | Display   | Auto     | Site name linked to the original service        |
| Complaint Reason    | Display   | Auto     | The reason text from the customer ticket        |

---

## Validation Rules

| Validation                       | Rule                                                                  |
| -------------------------------- | --------------------------------------------------------------------- |
| Time slot overlap                | System checks if selected technician has a conflicting task           |
| Date validity                    | Cannot be in the past                                                 |
| Start < End                      | Start time must be before end time                                    |
| Role → Employee cascade          | Employee list filtered by role; only active employees shown          |
| At least 1 technician            | Minimum one technician must be assigned                               |
| Primary in selected list         | Primary must be one of the multi-selected employees                  |
| SO line not yet tasked           | Warns if service line already has a task for the same date           |
| Material Qty ≥ 0                 | Required qty cannot be negative                                      |
| Material from master data        | Added materials must exist in Module 10/11                           |

---

## Form Actions

| Button            | Description                                                                     |
| ----------------- | ------------------------------------------------------------------------------- |
| **Create Tasks**  | Validates, checks conflicts, creates task records, returns to Daily Task View   |
| **Create Re-Task**| Same as above but marks task type as Re-Task                                    |
| **Cancel**        | Discards form and returns to previous screen                                    |

---

================================================================================

# 21.4 View Task Detail

**Description:**
Read-only screen showing the complete breakdown of a task — customer info, service details, assigned technicians, scheduled time, materials required/used, execution status, and customer feedback (if completed). Includes download/print option.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Daily View]                [Edit] [Reschedule] [Print / PDF]    │
│                                                                              │
│  TASK: TASK-2026-0201                          Status: ✅ COMPLETED          │
│  Task Type: Normal (From SO)                   Priority: 🟢 Normal          │
│                                                                              │
│  ─── SOURCE INFORMATION ────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  SO Number      : SO-2026-0112        Contract Ref  : CON-2026-0041   │ │
│  │  Customer       : ABC Corporation     Customer ID   : CUST-00245      │ │
│  │  Site           : Head Office (SITE-00312)                             │ │
│  │  Address        : Andheri East, Mumbai, Maharashtra                    │ │
│  │  Country        : India               Google Map    : 🔗 View on Maps │ │
│  │  Branch         : Mumbai                                               │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── SERVICE DETAILS ───────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Category       : General Pest Control    Subcategory   : Cockroach Mgmt   │ │
│  │  Service Type   : Cockroach Treatment    Service Mode  : Contract Base   │ │
│  │  Pest(s) Treated : Cockroaches             Area (SQFT)   : 3,500           │ │
│  │  Frequency      : Weekly (52/yr)         Estimated Dur.: 2 hours           │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── SCHEDULE & TECHNICIANS ────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Scheduled Date : 23 Mar 2026 (Monday)                                 │ │
│  │  Time Slot      : 08:00 – 10:00 (2 hours)                             │ │
│  │  Primary Tech   : Ravi S. (Senior Technician)                          │ │
│  │  Support Techs  : Anjali M. (Technician)                               │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── MATERIALS / CHEMICALS ─────────────────────────────────────────────── │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ Chemical Name          │ HSN    │ UOM   │ Required Qty │ Used Qty       ││
│  │────────────────────────┼────────┼───────┼──────────────┼────────────────││
│  │ Alpha Cypermethrin      │ 3808   │ ml    │ 120 ml       │ 110 ml         ││
│  │ Fipronil Gel            │ 3808   │ tube  │ 2 tubes      │ 2 tubes        ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ─── EXECUTION LOG ─────────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Started At     : 08:05 AM           Completed At  : 09:45 AM          │ │
│  │  Actual Duration: 1h 40m                                               │ │
│  │  Completion Note: "Treated all areas including server room."           │ │
│  │  Photos         : [📷 Before] [📷 After] [📷 Treatment Area]          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── CUSTOMER FEEDBACK ─────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Rating         : ⭐⭐⭐⭐⭐ (5/5)                                       │ │
│  │  Feedback       : "Excellent service. Very thorough."                  │ │
│  │  Feedback Date  : 23 Mar 2026, 02:30 PM                               │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── TASK HISTORY / AUDIT LOG ──────────────────────────────────────────── │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ Timestamp           │ Action              │ By            │ Details      ││
│  │─────────────────────┼─────────────────────┼───────────────┼──────────────││
│  │ 22 Mar, 10:00 AM    │ Task Created        │ Priya D.      │ From SO-0112 ││
│  │ 22 Mar, 10:05 AM    │ Technicians Assigned│ Priya D.      │ Ravi + Anjali││
│  │ 23 Mar, 08:05 AM    │ Task Started        │ Ravi S.       │ —            ││
│  │ 23 Mar, 09:45 AM    │ Task Completed      │ Ravi S.       │ Materials log││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Source Information Fields (Read-Only)

| Field           | Type     | Description                                              |
| --------------- | -------- | -------------------------------------------------------- |
| SO Number       | Link     | Sales Order reference; navigates to Module 20            |
| Contract Ref    | Link     | Contract reference; navigates to Module 19               |
| Customer        | Display  | Customer name (from Module 18)                           |
| Customer ID     | Display  | Unique ID (e.g. CUST-00245)                              |
| Site & Site ID  | Display  | Site name and ID (e.g. SITE-00312)                       |
| Address         | Display  | Site address from source                                 |
| Country         | Display  | Country of the site                                      |
| Google Map URL  | Link     | Clickable Google Maps link                               |
| Branch          | Display  | Servicing branch                                         |
| Category        | Display  | Service Category (e.g. General Pest Control)             |
| Subcategory     | Display  | Service Sub-Category (e.g. Cockroach Management)         |

---

## Task Execution Fields (Read-Only)

| Field           | Type     | Description                                              |
| --------------- | -------- | -------------------------------------------------------- |
| Task ID         | Display  | System-generated unique task ID                          |
| Task Type       | Badge    | Normal / Re-Task                                         |
| Service Mode    | Badge    | Contract Base / One-Time Service                         |
| Pest(s) Treated | Display  | Target pests (e.g. Cockroaches, Termites)                |
| Area (SQFT)     | Display  | Total area covered/treated                               |
| Status          | Badge    | Pending / In Progress / Completed / Overdue              |
| Priority        | Badge    | Normal / Urgent / Critical                               |
| Scheduled Date  | Date     | Planned execution date                                   |
| Time Slot       | Display  | Start – End time                                         |
| Primary Tech    | Display  | Main technician + role                                   |
| Support Techs   | Display  | Additional technicians                                   |
| Started At      | DateTime | Actual start timestamp                                   |
| Completed At    | DateTime | Actual completion timestamp                              |
| Actual Duration | Display  | Real time taken                                          |
| Completion Note | Text     | Technician's service notes                              |
| Photos          | Image    | Before/after/treatment images                            |
| Customer Rating | Display  | Star rating (1–5)                                       |
| Customer Feedback| Text    | Written feedback                                         |

---

## Form Actions

| Button            | Condition                     | Description                                        |
| ----------------- | ----------------------------- | -------------------------------------------------- |
| **Reschedule**    | Status = Pending              | Opens Reschedule/Reassign form (21.7)             |
| **Print / PDF**   | All statuses                  | Downloads/prints task detail as PDF                |

---

## Materials / Chemicals Detail (View)

| Field               | Type      | Description                                     |
| ------------------- | --------- | ----------------------------------------------- |
| Chemical Name       | Display   | Name of the material used                       |
| HSN                 | Display   | HSN code for tax reporting                      |
| UOM                 | Display   | Unit of measurement                             |
| Required Qty        | Display   | Planned quantity from task creation             |
| Used Qty            | Display   | Actual quantity logged by technician            |

---

## Execution & Audit Log

| Field               | Type      | Description                                     |
| ------------------- | --------- | ----------------------------------------------- |
| Started At          | DateTime  | Actual task start time                          |
| Completed At        | DateTime  | Actual task completion time                     |
| Actual Duration     | Display   | Time elapsed (Completed At - Started At)        |
| Photos              | Image     | Click to view Before/After/Area photos          |
| Audit Timestamp     | DateTime  | When the change occurred                        |
| Audit Action        | Display   | Created / Assigned / Started / Completed        |
| Audit By            | Display   | Name of the user who performed the action      |

---

================================================================================

# 21.5 Edit Task

**Description:**
Editable form for modifying a task that has not yet been completed. Allows changes to scheduling, technician assignment (with Role → Employee cascading), priority, and notes. Cannot edit source SO/customer data (read-only).

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Task Detail]              EDIT TASK: TASK-2026-0203             │
│                                                                              │
│  ─── SOURCE (Read-Only) ────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  SO Number      : SO-2026-0087        Customer      : PQR Foods       │ │
│  │  Category       : General Pest Control Subcategory   : Cockroach Mgmt  │ │
│  │  Service         : Termite Control     Site          : Warehouse       │ │
│  │  Task Type       : Normal (SO)         Created By    : Priya D.        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── SCHEDULE (Editable) ───────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Scheduled Date*   : [📅 23 Mar 2026]                                   │ │
│  │  Start Time*       : [▼ 14:00 ▼]                                       │ │
│  │  End Time*         : [▼ 16:00 ▼]                                       │ │
│  │  Duration          : 2 hours (auto)                                     │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── TECHNICIAN ASSIGNMENT (Editable) ──────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Role*             : [▼ Technician ▼]                                   │ │
│  │  Available Employees: [☑ Ravi S. ☐ Anjali M. ☑ Amit T.]               │ │
│  │  (Multi-select — cascading from Role; conflict-aware)                   │ │
│  │  Primary Technician*: [▼ Ravi S. ▼]                                    │ │
│  │                                                                         │ │
│  │  ⚠ Conflict Check: ✅ No conflicts detected                            │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── ADDITIONAL DETAILS ────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Priority*     : [▼ Urgent ▼]                                          │ │
│  │  Task Notes    : [Updated instructions for warehouse___________]       │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [SAVE CHANGES]                                       [CANCEL]         │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Source Details (Read-only)

| Field           | Type     | Description                                              |
| --------------- | -------- | -------------------------------------------------------- |
| SO Number       | Display  | Original Sales Order reference                           |
| Customer        | Display  | Customer name                                            |
| Category        | Display  | Service Category                                         |
| Subcategory     | Display  | Service Subcategory                                      |
| Service         | Display  | Specific service name                                    |
| Site            | Display  | Target site location                                     |
| Task Type       | Badge    | Normal / Re-Task                                         |
| Created By      | Display  | User who originally created the task                     |

---

## Editable Fields

| Field               | Type      | Required | Validation                               | Description                          |
| ------------------- | --------- | -------- | ---------------------------------------- | ------------------------------------ |
| Scheduled Date      | Date      | Yes      | Cannot be past date                      | Task execution date                  |
| Start Time          | Time      | Yes      | Must be before End Time                  | Task start time                      |
| End Time            | Time      | Yes      | Must be after Start Time                 | Task end time                        |
| Role                | Dropdown  | Yes      | From Module 8 role definitions           | Cascading filter for employee list   |
| Available Employees | Multi-sel | Yes      | At least one selected                    | Cascading from Role; multi-select    |
| Primary Technician  | Dropdown  | Yes      | Must be one of selected employees        | Main responsible person              |
| Priority            | Dropdown  | Yes      | Normal / Urgent / Critical               | Task priority                        |
| Task Notes          | Textarea  | No       | Max 500 chars                            | Additional instructions              |

---

## Form Actions

| Button            | Description                                                           |
| ----------------- | --------------------------------------------------------------------- |
| **Save Changes**  | Validates, checks conflicts, saves, logs change in audit trail       |
| **Cancel**        | Discards changes and returns to Task Detail                          |

---

================================================================================

# 21.6 Task Completion & Material Log

**Description:**
Post-service form submitted when a task is marked as completed. Captures actual start/end times, materials/chemicals used, service photos, and completion notes. On submission, **stock is automatically deducted** from Module 11 (Stock Management).

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Task Detail]         COMPLETE TASK: TASK-2026-0201              │
│                                                                              │
│  ─── SOURCE INFORMATION (Read-only) ────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Customer       : ABC Corporation     Customer ID   : CUST-00245      │ │
│  │  Site           : Head Office (SITE-00312)                             │ │
│  │  Category       : General Pest Control Subcategory   : Cockroach Mgmt  │ │
│  │  Service         : Cockroach Treat.    Service Mode  : Contract Base   │ │
│  │  Scheduled Date : 23 Mar 2026         Time Slot     : 08:00 – 10:00   │ │
│  │  Primary Tech    : Ravi S.             Support Techs : Anjali M.       │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── ACTUAL EXECUTION ──────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Actual Start Time* : [▼ 08:05 ▼]                                      │ │
│  │  Actual End Time*   : [▼ 09:45 ▼]                                      │ │
│  │  Actual Duration    : 1h 40m (auto-calculated)                          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── MATERIALS / CHEMICALS USED ────────────────────────────────────────── │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ Chemical Name         │ HSN   │ UOM   │ Required │ Actually Used*       ││
│  │───────────────────────┼───────┼───────┼──────────┼──────────────────────││
│  │ Alpha Cypermethrin     │ 3808  │ ml    │ 120 ml   │ [110____] ml         ││
│  │ Fipronil Gel           │ 3808  │ tube  │ 2 tubes  │ [2______] tubes      ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│  [+ Add Extra Material Used]                                                 │
│                                                                              │
│  ⚠ Stock Alert: Materials will be auto-deducted from Module 11 on submit.   │
│                                                                              │
│  ─── SERVICE PHOTOS ────────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Before Photo*     : [📎 Upload Photo]   ✅ Before_1.jpg                 │ │
│  │  After Photo*      : [📎 Upload Photo]   ✅ After_1.jpg                  │ │
│  │  Treatment Photo   : [📎 Upload Photo]   (Optional)                      │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── COMPLETION NOTES ──────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Completion Notes* : [Treated all areas including server room.______________]│
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── CUSTOMER FEEDBACK (Voice of Customer) ──────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Customer Rating*  : [⭐] [⭐] [▼ Rating (1-5) ▼]                        │ │
│  │  Client Feedback   : [Happy with the service._________________________]  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [MARK AS COMPLETED]                                  [CANCEL]         │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Source & Task Summary (Read-only)

| Field           | Type     | Description                                              |
| --------------- | -------- | -------------------------------------------------------- |
| Customer / ID   | Display  | Customer Name and unique ID                              |
| Site / ID       | Display  | Site Name and unique ID                                  |
| Category        | Display  | Service Category                                         |
| Subcategory     | Display  | Service Subcategory                                      |
| Service         | Display  | Specific service name                                    |
| Service Mode    | Badge    | Contract Base / One-Time                                 |
| Scheduled Date  | Display  | Planned date                                             |
| Time Slot       | Display  | Scheduled start – end                                    |
| Assigned Techs  | Display  | Primary and Support technicians                          |

---

## Completion Fields

| Field               | Type        | Required | Validation                                | Description                              |
| ------------------- | ----------- | -------- | ----------------------------------------- | ---------------------------------------- |
| Actual Start Time   | Time        | Yes      | Must be ≤ Actual End Time                | When the technician started              |
| Actual End Time     | Time        | Yes      | Must be ≥ Actual Start Time              | When the technician finished             |
| Actual Duration     | Display     | Auto     | Auto-calculated                           | End – Start                              |
| Actually Used Qty   | Number      | Yes      | Must be ≥ 0; cannot exceed stock         | Quantity of each chemical actually used  |
| Before Photo        | File Upload | Yes      | JPG, PNG (Max 5MB)                        | Mandatory photo before service           |
| After Photo         | File Upload | Yes      | JPG, PNG (Max 5MB)                        | Mandatory photo after service            |
| Treatment Photo     | File Upload | No       | JPG, PNG (Max 5MB)                        | Optional photo of treated area          |
| Completion Notes    | Textarea    | Yes      | Min 10 chars, Max 1000 chars              | Technician's summary of work            |
| Customer Rating     | Rating      | Yes      | 1 to 5 Stars                              | Customer's satisfaction level           |
| Client Feedback     | Textarea    | No       | Max 500 chars                             | Written feedback from customer          |


---

## Stock Deduction Logic

| Event                     | System Behaviour                                                    |
| ------------------------- | ------------------------------------------------------------------- |
| Form submitted            | "Actually Used" quantities deducted from branch stock (Module 11)  |
| Extra material added      | System checks availability before allowing submission              |
| Qty exceeds stock         | Warning: "Insufficient stock for Alpha Cypermethrin at Mumbai"     |
| Qty = 0                   | No deduction; material row kept for record purposes                |

---

## Form Actions

| Button                  | Description                                                             |
| ----------------------- | ----------------------------------------------------------------------- |
| **Mark as Completed**   | Validates, deducts stock, updates task status, logs in audit trail     |
| **Cancel**              | Returns to task detail without changes                                 |

---

================================================================================

# 21.7 Reschedule / Reassign Task

**Description:**
A dedicated form for rescheduling a task to a different date/time and/or reassigning it to different technicians. Uses the same **Role → Employee cascading** multi-select pattern. Available only for tasks with status = Pending.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Task Detail]       RESCHEDULE / REASSIGN TASK                   │
│                                                                              │
│  TASK: TASK-2026-0203            Current Status: ⏳ PENDING                  │
│                                                                              │
│  ─── CURRENT ASSIGNMENT (Read-Only) ────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Customer       : PQR Foods             Service  : Termite Control     │ │
│  │  Category       : Specialized Service   Sub-cat  : Rodent Management   │ │
│  │  Site           : Warehouse              Branch   : Mumbai             │ │
│  │  Current Date   : 23 Mar 2026           Time Slot: 14:00 – 16:00      │ │
│  │  Primary Tech   : Ravi S.              Support   : Amit T., Priya D.  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── NEW SCHEDULE ──────────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  New Date*         : [📅 25 Mar 2026]                                   │ │
│  │  New Start Time*   : [▼ 10:00 ▼]                                       │ │
│  │  New End Time*     : [▼ 12:00 ▼]                                       │ │
│  │  Duration          : 2 hours (auto-calculated)                          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── REASSIGN TECHNICIANS ──────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  [☑ Reassign Technicians]   (uncheck to keep current assignment)       │ │
│  │                                                                         │ │
│  │  Role*             : [▼ Technician ▼]                                   │ │
│  │                      • Technician                                       │ │
│  │                      • Senior Technician                                │ │
│  │                      • Supervisor                                       │ │
│  │                                                                         │ │
│  │  Available Employees: [☑ Suresh K. ☑ Anjali M. ☐ Ravi S. ☐ Amit T.]  │ │
│  │  (Multi-select — cascading from Role; filtered by new date/time)       │ │
│  │                                                                         │ │
│  │  Primary Technician*: [▼ Suresh K. ▼]                                  │ │
│  │                                                                         │ │
│  │  ⚠ Conflict Check: ✅ No conflicts on 25 Mar 10:00–12:00              │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── REASON ────────────────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Reason for Change* : [▼ Technician Unavailable ▼]                     │ │
│  │                       • Technician Unavailable                          │ │
│  │                       • Customer Request                                │ │
│  │                       • Weather / External Factor                       │ │
│  │                       • Scheduling Conflict                             │ │
│  │                       • Other                                           │ │
│  │  Additional Notes   : [Ravi on leave, reassigning to Suresh_______]    │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [CONFIRM RESCHEDULE]                                 [CANCEL]         │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Current Assignment Info (Read-only)

| Field           | Type     | Description                                              |
| --------------- | -------- | -------------------------------------------------------- |
| Customer        | Display  | Customer name                                            |
| Service         | Display  | Service type                                             |
| Category        | Display  | Service Category                                         |
| Sub-category    | Display  | Service Sub-Category                                     |
| Site            | Display  | Site location                                            |
| Branch          | Display  | Responsible branch                                       |
| Current Date    | Date     | Existing scheduled date                                  |
| Time Slot       | Display  | Existing time slot                                       |
| Primary Tech    | Display  | Existing primary assigned technician                      |
| Support Techs   | Display  | Existing additional technicians assignments                |

---

## Reschedule Fields

| Field               | Type      | Required | Validation                               | Description                               |
| ------------------- | --------- | -------- | ---------------------------------------- | ----------------------------------------- |
| New Date            | Date      | Yes      | Cannot be past date                      | Rescheduled execution date               |
| New Start Time      | Time      | Yes      | Must be before New End Time              | New start time                            |
| New End Time        | Time      | Yes      | Must be after New Start Time             | New end time                              |
| Reassign Checkbox   | Checkbox  | No       | Default: unchecked                       | Toggle technician reassignment            |
| Role                | Dropdown  | Cond.    | Required if Reassign checked             | Cascading filter for employee list        |
| Available Employees | Multi-sel | Cond.    | Required if Reassign checked             | Filtered by Role + new date/time slot     |
| Primary Technician  | Dropdown  | Cond.    | Must be one of selected employees        | New primary technician                    |
| Reason for Change   | Dropdown  | Yes      | Must select from list                    | Categorised reason for change             |
| Additional Notes    | Textarea  | No       | Max 500 chars                            | Free-text explanation                     |

---

## Validation Rules

| Validation                       | Rule                                                                  |
| -------------------------------- | --------------------------------------------------------------------- |
| Date validity                    | Cannot be in the past                                                 |
| Time validity                    | New Start < New End                                                   |
| Conflict check on new slot       | System validates no overlap for selected technicians on new date/time |
| Reason required                  | Must select a reason category                                        |
| Cascade on Reassign              | If Reassign checked, Role + Employees + Primary are all required     |

---

## Form Actions

| Button                    | Description                                                           |
| ------------------------- | --------------------------------------------------------------------- |
| **Confirm Reschedule**    | Validates, updates task, logs old vs new values in audit trail       |
| **Cancel**                | Returns to task detail without changes                               |

---

================================================================================

# Access Control (RBAC)

| Role                  | Permissions                                                                |
| --------------------- | -------------------------------------------------------------------------- |
| **Technician Manager**| Full access: Create, Edit, Reschedule, Assign, Complete, View all tasks   |
| **Operations Head**   | Full access: Same as Technician Manager + reporting                       |
| **Senior Technician** | View own tasks, Mark completion, Log materials                            |
| **Technician**        | View own tasks, Mark completion, Log materials                            |
| **Company Admin**     | View all tasks, Reports (read-only)                                       |

> **Note:** No approval workflow is required. Tasks are created and managed directly by the Technician Manager without managerial sign-off.

---

================================================================================

# Business Rules

| Rule                                  | Description                                                           |
| ------------------------------------- | --------------------------------------------------------------------- |
| Task ID Format                        | `TASK-YYYY-NNNN` (auto-generated, sequential per year)               |
| No scheduling conflicts               | Same technician cannot be double-booked in overlapping time slots     |
| Stock deduction on completion          | Materials used are deducted from branch stock in Module 11            |
| Overdue auto-marking                  | Tasks past their scheduled date with status ≠ Completed → Overdue    |
| Re-Task linkage                       | Re-Tasks maintain link to original SO and Customer Ticket            |
| Audit trail                           | All create, edit, reschedule, and complete actions are logged         |
| Role → Employee cascade               | Employee list is always filtered by selected Role (Module 8)         |
| Primary must be in selected list      | Primary Technician must be one of the multi-selected employees       |

---

> **Future Module:** Customer Support Ticket creation and management module (referenced in Re-Task flow, Tab 2 of Screen 21.3).

---
---

================================================================================
================================================================================

# 🎯 MODULE 22: LIVE LOCATION & TRAVEL TRACKING

## Overview

Live Location & Travel Tracking provides the **Operations Manager** and **Branch Manager** with real-time visibility into the field workforce. It directly builds on **Module 21 (Task Management)** by correlating a technician's GPS location with their scheduled tasks, calculating actual travel distances, and maintaining auditable travel logs.

**Module Connections:**

- **Depends on:** Module 21 (Tasks — locations, schedules, status), Module 8 (Employee — technician data), Module 7 (Branch — start/end depot points).
- **Used by:** Reporting modules, Payroll (for travel claim validations).

---

The module contains the following screens:

- 22.1 Fleet Tracking Dashboard (Live & Historical Map)
- 22.2 Technician Logistics & Travel Log (Timeline & Tabular Reports)

---

================================================================================

# 22.1 Fleet Tracking Dashboard

**Description:**
The primary command center for tracking the workforce. By default, it shows **Live Tracking** for Today. Managers can use the Date Calendar to select **any previous date** to view the historical map and routes of all active technicians on that specific day. 

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          FLEET TRACKING DASHBOARD                           │
│                                                                              │
│  ┌─────────────────────────┐ ┌──────────────────────────────────────────────┐│
│  │ FILTERS                 │ │                                              ││
│  │ Date  : [📅 Today ▼]    │ │              [ MAP VIEW ]                    ││
│  │ Branch: [▼ All ▼]       │ │                                              ││
│  │ Techs : [🔍 Search ▼]   │ │       📍(Ravi)               📍(Anjali)      ││
│  │ Status: [▼ Active ▼]    │ │                                              ││
│  │                         │ │                                              ││
│  │ [🟢 Active] [⚪ Offline]│ │                                              ││
│  │                         │ │                                              ││
│  │ 🟢 Ravi S.              │ │                                              ││
│  │ 📍 Head Office (Andheri)│ │                              📍(Amit)        ││
│  │ ⏱ On Site (Since 08:05) │ │                                              ││
│  │ 👤 ABC Corp (Cockroach) │ │                                              ││
│  │ 📋 TASK-2026-0201       │ │                                              ││
│  │                         │ │                                              ││
│  │ ⚪ Amit T. (Offline)   │ │                                              ││
│  │ 📍 Last: Branch Office  │ │                                              ││
│  │ ⏱ Offline since 11:30  │ │         📍(Suresh)                         ││
│  │                         │ │                                              ││
│  │                         │ │                                              ││
│  │ [Load More Data...]     │ │   [🔍 Zoom]  [🗺 Terrain]  [⚙ Map Settings]   ││
│  └─────────────────────────┘ └──────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Dashboard Filters & Feed Fields

| Field              | Type      | Description                                                |
| ------------------ | --------- | ---------------------------------------------------------- |
| Date Selector      | Date      | Default: Today (Live). Select past dates for historical Map.|
| Technician Name    | Link      | Clicks through to Technician Travel Log (22.2).            |
| Current Location   | Display   | Nearest address or specific Site Name (from Module 21).    |
| Feed Tabs          | Tab       | Toggle between Active techs map vs Offline techs list      |
| Current Status     | Badge     | Travelling / On Site / Idle / Offline.                     |
| Customer & Service | Display   | Linked Customer Name and specific Service Type.            |
| Active Task        | Link      | Current Module 21 Task ID (if On Site or Travelling to it).|

---

## Map Interactions

| Action                        | Result                                                              |
| ----------------------------- | ------------------------------------------------------------------- |
| **Select Past Date**          | Updates map to show all drawn routes for selected techs on that day.|
| **Hover on Map Pin**          | Shows mini-tooltip with Name, Speed/Time, and Assigned Task.        |
| **Click on Feed Item**        | Pans/Zooms map to the selected technician's specific location.      |

---

================================================================================

# 22.2 Technician Logistics & Travel Log

**Description:**
A unified, comprehensive profile of a specific technician's logistics data. Managers can select the view range (**Daily, Weekly, Monthly**) to see aggregated map routes, an activity timeline, and the full tabular travel log data (previously 22.3) in one organized place. 

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Dashboard]         LOGISTICS PROFILE: Ravi S.                   │
│                                                                              │
│  Filters: Period: [▼ Daily ▼]  Date: [📅 23 Mar 2026 ▼]                     │
│                                                                              │
│  ┌─ SUMMARY & TIMELINE ─────────────────┐ ┌─ EVENT MAP ───────────────────┐│
│  │ Total Distance: 42.5 KM              │ │                               ││
│  │ Total Active  : 8h 30m               │ │      (T1) ---------- (T2)     ││
│  │ Tasks Done    : 3                    │ │      /                 \      ││
│  │                                      │ │     /                   \     ││
│  │ 08:30 AM: Arrived                    │ │   (B)                    (T3) ││
│  │ 📍 Head Office (Site)                │ │     \                   /     ││
│  │ 👤 ABC Corp (Cockroach)              │ │      \                 /      ││
│  │ 📋 TASK-2026-0201                    │ │       ------- (B) -----       ││
│  │                                      │ │                               ││
│  │ 08:30 - 10:15 (On Site)              │ │ [📥 Map] [Replay Route ▶]     ││
│  │                                      │ └───────────────────────────────┘│
│  │ 10:15 AM: Departed                   │                                  │
│  │ 📍 Head Office                       │                                  │
│  └──────────────────────────────────────┘                                  │
│                                                                            │
│  ── TABULAR TRAVEL LOG ─────────────────────────────────────────────────── │
│  ┌───────────────────────────────────────────────────────────────────────┐ │
│  │ Date  │ Departure Pt │ Destination  │ Customer │ Task ID│ Dist │ Start│ │
│  │───────┼──────────────┼──────────────┼──────────┼────────┼──────┼──────│ │
│  │ 23 Mar│ Branch Off.  │ ABC: Head Off│ ABC Corp │ T-0201 │ 15.2 │ 08:00│ │
│  │ 23 Mar│ ABC: Head Off│ PQR: Warehse │ PQR Ind. │ T-0202 │ 12.8 │ 10:15│ │
│  └───────────────────────────────────────────────────────────────────────┘ │
│    [⬅ Scroll Right to see: End Time, Duration, Status ➡]                    │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Profile Filter Fields

| Field            | Type     | Description                                             |
| ---------------- | -------- | ------------------------------------------------------- |
| Period Selector  | Dropdown | Select Daily, Weekly, or Monthly view.                  |
| Date Selector    | Control  | Selects the specific date, week, or month to analyze.   |
| Summary Stats    | Display  | Aggregated Distance, Time, and Tasks for chosen period. |

---

## Timeline & Tabular Log Fields

| Field             | Type    | Description                                             |
| ----------------- | ------- | ------------------------------------------------------- |
| Date              | Display | Tracking Date.                                          |
| Departure Point   | Display | Origin site or generic location (e.g., Branch).         |
| Destination       | Display | Destination Site Name or Geo-location address.          |
| Customer          | Display | Associated Customer Name and specific Service Type.     |
| Task ID           | Modal   | Click Task to view details (Status, Category, Techs).   |
| Distance          | Display | GPS calculated travel distance for that segment.        |
| Start & End Time  | Display | Timestamps marking the departure and arrival bounds.    |
| Duration          | Display | Total time computed between Start and End.              |
| Status            | Badge   | On-Site, Travelling, Idle.                              |

---

================================================================================

# Business Rules & Logic (Module 22)

| Rule                          | Description                                                                 |
| ----------------------------- | --------------------------------------------------------------------------- |
| **GPS Polling Rate**          | Technician mobile app sends GPS coordinates every X minutes/meters.         |
| **Task Auto-Arrival**         | Proximity to Module 21 Site Lat/Long automatically logs an 'Arrived' event. |
| **Idle Alert**                | If GPS is stationary for > Y minutes not near a task site, flag as 'Idle'.  |
| **Background Tracking**       | GPS tracking must only be active between Clock-in and Clock-out events.     |
| **Distance Calculation**      | System calculates route segments and aggregates them for Daily/Weekly stats.|

---

================================================================================

# 🎯 MODULE 23: CUSTOMER SUPPORT MANAGEMENT (SLA-DRIVEN)

## Overview

Live Location & Travel Tracking provided real-time operational oversight. **Module 23** provides enterprise-grade customer support by enabling the recording, tracking, and resolution of customer complaints and requests via a strict **SLA (Service Level Agreement)** framework.

This module guarantees time-bound responses and automatically escalates tickets that breach resolution times. It tightly integrates with **Module 21 (Task Management)** to dispatch technicians for re-services stemming from support tickets.

**Module Connections:**

- **Depends on:** Module 18 (Customer Master), Module 20 (Sales Orders), Module 8 (Employee).
- **Used by:** Module 21 (Re-Tasks), Customer History, Analytics.

---

The module contains the following screens:

- 23.1 Ticket Dashboard (SLA-Driven)
- 23.2 Raise New Ticket
- 23.3 Ticket Detail View
- 23.4 Assign / Reassign Ticket
- 23.5 Convert Ticket to Task
- 23.6 Ticket Resolution

---

================================================================================

# 23.1 Ticket Dashboard (SLA-Driven)

**Description:**
The primary command center for the Support Team. Provides a high-visibility datatable of all active tickets. Critically powered by live countdown timers and color-coded SLA status indicators to prioritize agent workflows instantly.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          SUPPORT TICKET DASHBOARD                           │
│                                                                              │
│  [+ Raise New Ticket]                                                       │
│                                                                              │
│  ┌─ FILTERS ──────────────────────────────────────────────────────────────┐  │
│  │ Branch  : [▼ All ▼]       Customer : [🔍 Search ▼] Date : [📅 Range ▼] │  │
│  │ Status  : [▼ Open ▼]      Priority : [▼ All ▼]     Type : [▼ All ▼]    │  │
│  │ SLA Stat: [▼ At Risk ▼]   Esc. Lvl : [▼ L2 ▼]                          │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌────────────────────────────────────────────────────────────────────────┐  │
│  │ ID   │ Customer │ SO No  │ Priority│ Status  │ SLA Stage │ SLA Timer │  │
│  │──────┼──────────┼────────┼─────────┼─────────┼───────────┼───────────│  │
│  │ T001 │ ABC Corp │ SO-101 │🔴 Urgent│ Open    │ 🔴 Breach │ -02h 15m  │  │
│  │ T002 │ PQR Ind  │ SO-089 │🟡 High  │ Open    │ 🟡 Risk   │ 00h 45m   │  │
│  │ T003 │ XYZ Hotel│ SO-144 │🟢 Normal│ In Prog │ 🟢 Safe   │ 22h 10m   │  │
│  │ T004 │ LMN Pvt  │ SO-201 │🟢 Normal│ Open    │ 🟢 Safe   │ 47h 00m   │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│    Shows 1 to 4 of 42 tickets.                     [ < Previous | Next > ]   │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Filter Fields

| Field            | Type       | Description                                                 |
| ---------------- | ---------- | ----------------------------------------------------------- |
| Branch Name      | Dropdown   | Filter by servicing branch: `[All, Head Office, North, South, East, West]`. |
| Date Range       | Date Range | Filter by Ticket Creation Date.                             |
| Status           | Dropdown   | Standard lifecycle: `[Open, Assigned, In Progress, Paused, Resolved, Closed]`. |
| Priority         | Dropdown   | Ticket Priority: `[All, Normal, High, Urgent, Critical]`.   |
| SLA Status       | Dropdown   | SLA Health: `[Safe (🟢), At Risk (🟡), Breached (🔴)]`.      |
| Escalation Level | Dropdown   | Triggered SLA: `[None, L1 (Soft), L2 (Manager), L3 (Critical)]`. |

---

## Search Fields

| Field         | Type   | Description                                                                 |
| ------------- | ------ | --------------------------------------------------------------------------- |
| Ticket ID     | Search | Direct lookup by `TKT-YYYY-NNNN`. Navigates straight to 23.3 Detail View.  |
| Customer Name | Search | Global free-text search against Customer Master (Module 18).                |
| SO Number     | Search | Search by linked Sales Order `SO-YYYY-NNNN` to find all associated tickets. |


---

## Table Columns

| Field          | Type      | Description                                                                 |
| -------------- | --------- | --------------------------------------------------------------------------- |
| Ticket ID      | Link      | `TKT-YYYY-NNNN`. Clicks to 23.3 Detail View.                                |
| Customer Name      | Display   | Name of the Customer.                                                       |
|Branch Name | Display | Name of Branch     | 
| SO No          | Display   | Associated Sales Order (if applicable).                                     |
| Priority       | Display   | Ticket Priority.                                                            |
| Status         | Display   | Current lifecycle state.                                                    |
| SLA Stage      | Badge     | Color-coded visual of SLA health (Safe/Risk/Breach).                        |
| SLA Timer      | Display   | Live countdown (e.g. `22h 10m` remaining, `-02h 15m` overdue).              |

---

================================================================================

# 23.2 Raise New Ticket

**Description:**
The data-entry screen for new complaints or service requests. Auto-calculates SLAs and expected resolution times based on the selected Priority and Ticket Type.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                            RAISE NEW TICKET                                 │
│                                                                              │
│  ── CUSTOMER & SOURCE ──────────────────────────────────────────────────── │
│  ┌───────────────────────────────────────────────────────────────────────┐ │
│  │ Customer*     : [🔍 Search: "ABC Corp" ─────────▼]                    │ │
│  │ Related SO    : [▼ SO-2026-00101 ▼] (Optional)                        │ │
│  │ Reported By*  : [Suresh (Facility Manager)________]                   │ │
│  │ Phone Number* : [+91 9876543210___________________]                   │ │
│  └───────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ── TICKETING DETAILS ──────────────────────────────────────────────────── │
│  ┌───────────────────────────────────────────────────────────────────────┐ │
│  │ Ticket Type*  : [▼ Complaint - Re-emergence ▼]                        │ │
│  │ Priority*     : [▼ Urgent ▼]                                          │ │
│  │ Subject*      : [Cockroaches seen again in lobby__]                   │ │
│  │ Description*  : [Treated 2 days ago, but active   ]                   │ │
│  │                 [infestation noticed near the     ]                   │ │
│  │                 [registration desk.               ]                   │ │
│  │                                                                       │ │
│  │ Expected Date*: [📅 25 Mar 2026]   Time*: [▼ 17:00 ▼]                 │ │
│  │                                                                       │ │
│  │ Attachments   : [+ Upload Photos/Emails]                              │ │
│  └───────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [RAISE TICKET]                                        [CANCEL]         │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Field Table

| Field          | Type     | Required | Validation                          | Description                                         |
| -------------- | -------- | -------- | ----------------------------------- | --------------------------------------------------- |
| Customer       | Search   | Yes      | Must select from Module 18          | Linked Customer Profile.                            |
| Related SO     | Dropdown | No       | Filtered by selected Customer       | Links ticket to context: `[List of user's active SO-YYYY-NNNN]`. |
| Reported By    | Text     | Yes      | —                                   | Name of the person reporting the issue.             |
| Phone Number   | Text     | Yes      | Valid Phone format                  | Contact number for callbacks.                       |
| Ticket Type    | Dropdown | Yes      | Values from Admin config            | Issue category: `[Complaint - Re-emergence, Complaint - Staff, Query - Billing, Query - Service]`. |
| Priority       | Dropdown | Yes      | Auto-sets based on Type, editable   | Drives the SLA timer: `[Normal, High, Urgent, Critical]`. |
| Subject        | Text     | Yes      | Max 100 chars                       | Short summary of the issue.                         |
| Description    | Textarea | Yes      | Max 1000 chars                      | Full details of the complaint.                      |
| Expected Date  | Date     | Yes      | Cannot be past date                 | Manager manually asserts proper resolution date.    |
| Expected Time  | Time     | Yes      | —                                   | Manager manually asserts proper resolution time.    |
| Attachments    | File     | No       | Max 5 files, 5MB each               | Image/PDF evidence of the issue.                    |

---

================================================================================

# 23.3 Ticket Detail View

**Description:**
The core workspace for Support Agents. It presents the entire context of the ticket, complete with live SLA health, a chronological communication timeline, and multi-action capability.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Dashboard]         TKT-2026-0042                  [Print/PDF 🖨]│
│                                                                              │
│  ┌─ TICKET SUMMARY ─────────────────┐ ┌─ SLA & STATUS ────────────────────┐ │
│  │ Subject : Cockroaches in Lobby   │ │ Status     : [ In Progress ]      │ │
│  │ Customer: ABC Corp               │ │ Priority   :  🔴 Urgent           │ │
│  │ SO No   : SO-2026-00101          │ │ Assigned To:  Anjali M. (Support) │ │
│  │ Type    : Complaint              │ │                                   │ │
│  │ Created : 24 Mar 2026, 09:00 AM  │ │ ⏱ Response : ✅ Met (09:15 AM)    │ │
│  │ Caller  : Suresh (9876543210)    │ │ ⏱ Resolut. : 🔴 -02h 15m (Breach) │ │
│  │ Desc    : Treated 2 days ago...  │ │ 🚨 Esc. Lvl : Level 2 (Manager)   │ │
│  └──────────────────────────────────┘ └───────────────────────────────────┘ │
│                                                                              │
│  ┌─ ACTIONS ──────────────────────────────────────────────────────────────┐ │
│  │ [👤 Assign/Reassign] [📝 Add Note / Reply] [🛠 Convert to Task]         │ │
│  │ [⏸ Pause Ticket]     [✅ Mark Resolved]    [🛑 Close Ticket]            │ │
│  └────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─ ACTIVITY TIMELINE ────────────────────────────────────────────────────┐ │
│  │ 🔴 13:00 (Sys): SLA Resolution Breached! Esc. L2 (Manager Alerted).    │ │
│  │ 🛠 10:30 (Anj): Re-task created: TASK-2026-0205 (Assigned to Ravi).    │ │
│  │ 📝 09:15 (Anj): Called Suresh. Re-service needed today. [Int. Note]    │ │
│  │ 👤 09:10 (Sys): Ticket assigned to Anjali M.                           │ │
│  │ 🟢 09:00 (Sys): Ticket TKT-2026-0042 Raised by Admin.                  │ │
│  └────────────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Screen Components

| Component         | Description                                                                 |
| ----------------- | --------------------------------------------------------------------------- |
| **SLA & Status**  | Live updates of the dual-timers. Flags if the SLA has escalated to L1/L2/L3.|
| **Actions Menu**  | Direct triggers for popups covering Assignment, Task mapping, and Resolution.|
| **Activity Line** | Immutable audit log of all status changes, notes, calls, and task creations.|

---

================================================================================

# 23.4 Assign / Reassign Ticket (Modal)

**Description:**
Popup to change the primary Support Agent handling the communication. Note: This does **not** assign a field technician (that happens in 23.5 Task Conversion).

## Screen Layout

```
┌─────────────────────────────────────────────────────────┐
│  REASSIGN TICKET: TKT-2026-0042                         │
│                                                          │
│  Current Agent: Anjali M. (Support)                      │
│                                                          │
│  Assign To (Role)*  : [▼ Operations Manager ▼]          │
│  Assign To (Person)*: [▼ Kamal R. ▼]                    │
│                                                          │
│  Assignment Note    : [Customer escalated to manager  ] │
│                       [due to repeated failure.       ] │
│                                                          │
│       [CONFIRM REASSIGNMENT]           [CANCEL]          │
└─────────────────────────────────────────────────────────┘
```

| Field              | Type     | Required | Description                                  |
| ------------------ | -------- | -------- | -------------------------------------------- |
| Assign To (Role)   | Dropdown | Yes      | Destination Role: `[Support Agent, Senior Technician, Operations Manager, Quality Control]`. |
| Assign To (Person) | Dropdown | Yes      | Cascading list based on Role: `[List of active Employee Names]`. |
| Assignment Note    | Textarea | No       | Optional handover notes                      |

---

================================================================================

# 23.5 Convert Ticket to Task (Modal / Flow)

**Description:**
The crucial bridge between Customer Support and Operations. Converts the complaint into an actionable Re-Task in Module 21.

**SLA Impact:**
- Converting to a task **does not stop** the Ticket's Resolution SLA.
- The SLA only stops when the Technician *completes* the Re-Task in the field AND the Support Agent *resolves* the ticket.

---

## Screen Integration

Clicking `[Convert to Task]` on Screen 23.3 directly opens **Module 21 → Screen 21.3 (Add Re-Task) → Tab 2: From Customer Tickets**.

The following fields are **auto-filled** from the active ticket context:

| Field               | Auto-filled Value                                |
| ------------------- | ------------------------------------------------ |
| Source Type         | Customer Ticket (pre-selected, locked)           |
| Ticket ID           | Current `TKT-YYYY-NNNN`                          |
| Customer            | Linked Customer from Ticket                      |
| Original Service    | From the linked Sales Order / Service            |
| Site                | From the linked Sales Order / Service            |
| Complaint Reason    | Ticket Description (Read-only)                   |
| Priority            | Inherited from Ticket Priority                   |
| Materials/Chemicals | Auto-fetched from original SO service (editable) |

> 📌 *For the complete Re-Task form layout, field validations, and technician assignment logic — **see Module 21 → Screen 21.3 → Tab 2**.*

---

================================================================================

# 23.6 Ticket Resolution (Modal)

**Description:**
Fired by the Support Agent once the customer is satisfied and/or the associated field tasks are completed. 

## Screen Layout

```
┌─────────────────────────────────────────────────────────┐
│  RESOLVE TICKET: TKT-2026-0042                          │
│                                                          │
│  🚨 Pre-Check: Linked Task (TASK-2026-0205) is          │
│  marked [Completed] by Technician Ravi.                 │
│                                                          │
│  Resolution Code* : [▼ Service Resolved Success ▼]      │
│  Resolution Notes*: [Technician treated lobby area.   ] │
│                     [Customer confirmed rodents gone. ] │
│                     [SLA Timer will be stopped.       ] │
│                                                          │
│  Attachments      : [+ Upload Customer Sign-off PDF]    │
│                                                          │
│       [RESOLVE TICKET & STOP SLA]       [CANCEL]         │
└─────────────────────────────────────────────────────────┘
```

## Screen Logic & Validation
1. **Validation:** System checks Module 21. If any Linked Tasks (`TASK-2026-NNNN`) are NOT marked "Completed", the system **blocks** resolution:
   > ❌ *Error: Cannot resolve ticket. Linked Task (TASK-2026-0205) is currently 'In Progress'.*
2. **Success:** If no pending tasks, stops the Resolution SLA Timer.

| Field             | Type     | Required | Description                                  |
| ----------------- | -------- | -------- | -------------------------------------------- |
| Resolution Code   | Dropdown | Yes      | Reason for closure: `[Service Resolved Success, False Alarm / Not Found, Duplicate Ticket, Unresolved (Closed)]`. |
| Resolution Notes  | Textarea | Yes      | Detailed explanation of how issue was solved |
| Attachments       | File     | No       | Sign-offs, final photos, emails              |

---

---

================================================================================

# ⚙️ Dual SLA & Escalation Engine Logic

### 1. Dual SLA Architecture
The system independently tracks two timers for every ticket:
*   **Response SLA Timer:** Starts immediately when `Created`. Stops completely the moment an agent triggers `[Add Note / Reply]` or `[Assign]`.
*   **Resolution SLA Timer:** Starts immediately when `Created`. Does not stop until the ticket is marked `[Resolved]`.

### 2. SLA Pausing
*   If a ticket requires input from the Customer (e.g., waiting on them to approve a re-visit time), the agent can click `[Pause Ticket]`.
*   Both SLA timers **pause**.
*   When the customer replies, or the agent clicks `[Resume]`, timers start from where they left off.

### 3. Automated 3-Level Escalation Engine
A cron/background service constantly checks active timers against the SLA Configuration Matrix (23.7).

*   **Trigger L1 (Soft):** Response SLA breaches (Fixed at 2 hours post-creation with no agent action).
    *   *Action:* Ticket changes color to 🟡. Email sent to assigning manager.
*   **Trigger L2 (Manager Alert):** The ticket reaches **80% of the duration** between `Created Date` and the manually entered `Expected Date/Time`.
    *   *Action:* Email alert sent to Operations Head. Flagged in Dashboards.
*   **Trigger L3 (Critical Breach):** Current time exceeds the manually entered `Expected Date/Time`.
    *   *Action:* Ticket status changed to "SLA Breached" 🔴. Alert sent to Company Admin / CEO roles.

### 4. SLA Status Conditional Logic
The SLA Status indicator (🟢, 🟡, 🔴) found throughout the ticketing dashboards automatically updates its visual state by evaluating the `Current Time` against the `Expected Date/Time` entered in **Screen 23.2**.

*   **Safe (🟢)**: 
    *   **Condition:** `Current Time` < `80% of Completion Window`.
    *   **Definition:** The ticket is comfortably within the resolution timeline set by the manager.
*   **At Risk (🟡)**: 
    *   **Condition:** `Current Time` >= `80% of Completion Window` AND `Current Time` < `Expected Date/Time`.
    *   **Definition:** The ticket has entered the final 20% of its allotted time. It needs immediate attention.
*   **Breached (🔴)**: 
    *   **Condition:** `Current Time` >= `Expected Date/Time`.
    *   **Definition:** Overdue. The ticket failed to be resolved by the manually asserted deadline.

---

================================================================================

# Flow Status Definitions

## Ticket Lifecycle Matrix

| Status      | Description                                     | Next Allowed Statuses         | SLA Timer Running? |
| ----------- | ----------------------------------------------- | ----------------------------- | ------------------ |
| Open        | Fresh ticket, unassigned.                       | Assigned, In Progress         | YES                |
| Assigned    | Support Agent tagged, no action taken yet.      | In Progress                   | YES                |
| In Progress | Being worked on, or Module 21 task dispatched.  | Paused, Resolved              | YES                |
| Paused      | Waiting indefinitely on Customer.               | In Progress                   | NO (Paused)        |
| Resolved    | Issue fixed, final confirmation pending.        | Closed, Reopened              | NO (Stopped)       |
| Closed      | Final end-state. Locked.                        | Reopened (rare case)          | NO (Stopped)       |

---

================================================================================
