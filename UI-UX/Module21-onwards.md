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

| Action                     | Result                                                |
| -------------------------- | ----------------------------------------------------- |
| **Click on a date cell**   | Navigates to **Daily Task View (21.2)** for that date |
| **Click on overdue alert** | Navigates to the overdue date's Daily Task View       |
| **Hover on date cell**     | Shows tooltip: task count breakdown by status         |

---

## Filters

| Filter     | Type         | Options                                           |
| ---------- | ------------ | ------------------------------------------------- |
| Branch     | Dropdown     | All Branches / Specific Branch (from Module 7)    |
| Technician | Search       | Search by technician name (from Module 8)         |
| Task Type  | Multi-select | Normal (SO) / Re-Task (Ticket)                    |
| Status     | Multi-select | Pending / In Progress / Completed / Overdue       |
| Search     | Text         | Free text search (Task ID / Customer / SO Number) |

---

## Monthly Summary Fields

| Field         | Type   | Description                                    |
| ------------- | ------ | ---------------------------------------------- |
| Total Tasks   | Number | Count of all tasks in the displayed month      |
| Completed     | Number | Tasks with status = Completed                  |
| In Progress   | Number | Tasks currently being executed                 |
| Pending       | Number | Tasks not yet started                          |
| Overdue       | Number | Past-date tasks still not completed            |
| Re-Tasks      | Number | Tasks created from customer complaints/tickets |
| Avg Tasks/Day | Number | Average daily task load for the month          |

---

## Form Actions

| Action         | Description                                        |
| -------------- | -------------------------------------------------- |
| **+ Add Task** | Opens the **Add / Create Task Form** (Screen 21.3) |

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

| Field         | Type    | Description                                                   |
| ------------- | ------- | ------------------------------------------------------------- |
| Task ID       | Link    | System-generated unique ID (e.g., TASK-2026-0201); opens 21.4 |
| Time Slot     | Display | Scheduled start – end time                                    |
| Customer      | Display | Customer name from Module 18                                  |
| Service       | Display | Service type (e.g., Cockroach Treatment)                      |
| Site          | Display | Site name and location                                        |
| Primary Tech  | Display | Main responsible technician                                   |
| Support Techs | Display | Additional assigned technicians (comma-separated)             |
| Type          | Badge   | Normal / Re-Task                                              |
| Status        | Badge   | Pending / In Progress / Completed / Overdue                   |
| Actions       | Buttons | [View] [Edit] [Reschedule] [Complete]                         |

---

## Actions (Table Row)

| Action         | Condition                      | Description                                    |
| -------------- | ------------------------------ | ---------------------------------------------- |
| **View**       | All statuses                   | Opens Task Detail (Screen 21.4)                |
| **Edit**       | Status = Pending / In Progress | Opens Edit Task form (Screen 21.5)             |
| **Reschedule** | Status = Pending               | Opens Reschedule / Reassign form (Screen 21.7) |
| **Complete**   | Status = In Progress           | Opens Task Completion form (Screen 21.6)       |

---

## Conflict Detection

| Rule                        | Behaviour                                                |
| --------------------------- | -------------------------------------------------------- |
| Overlapping time slots      | System highlights conflict in red; blocks assignment     |
| Same technician, same time  | Warning: "Ravi S. is already assigned to TASK-2026-0201" |
| Tech availability exhausted | Greyed out in Technician Availability panel              |

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
│  │  │  Site Contact*     : [Rajesh_______________]                        │   │ │
│  │  │  Contact Mobile*   : [+919876543210        ]                        │   │ │
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
│  │  │  Site Contact*     : [Amit Kumar___________]                        │   │ │
│  │  │  Contact Mobile*   : [+919988776655        ]                        │   │ │
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
│  │  Site Contact*     : [Amit Kumar___________] (Editable)                 │ │
│  │  Contact Mobile*   : [+919988776655        ] (Editable)                 │ │
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

## Tab 1 — From Sales Order (SO): Field Descriptions

### Step 1: Source & Service Selection

| Field     | Type                  | Required | Validation                | Description                                                                                                                                     |
| --------- | --------------------- | -------- | ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Branch    | Dropdown              | Yes      | From Module 7 branch list | Select the servicing branch. Filters the SO search to show only orders under the selected branch.                                               |
| Search SO | Search (Autocomplete) | Yes      | Must select a valid SO    | Searchable dropdown — type SO Number or Customer Name. On selection, the Service Grid below populates with all service line items from that SO. |

#### Service Grid (Multi-Select from Selected SO)

> Appears after an SO is selected. The manager checks ☑ one or more rows to create tasks from them. Each checked row becomes an independent task card in Step 2.

| Column       | Type     | Description                                                                                                          |
| ------------ | -------- | -------------------------------------------------------------------------------------------------------------------- |
| ☑ (Checkbox) | Checkbox | Select / deselect a service line item. At least one must be selected to proceed.                                     |
| SO Number    | Display  | Sales Order number (e.g. SO-2026-0112). Read-only — from Module 20.                                                  |
| Customer     | Display  | Customer name linked to the SO (from Module 18). Read-only.                                                          |
| Service      | Display  | Service type / name (e.g. Cockroach Treatment, Termite Control). Read-only — from GMA / Module 17.                   |
| Site         | Display  | Site or location name where the service is to be performed (e.g. Head Office, Lobby). Read-only — from SO line item. |
| Frequency    | Display  | Service frequency as defined in the contract / SO (e.g. Weekly, Monthly, Quarterly). Read-only.                      |

**Footer Indicator:** `Selected: N service line items` — live count of checked rows.

---

### Step 2: Schedule, Assign & Materials (Per Selected SO in above section)

> A **separate task card** is generated for each selected service line item from Step 1. All fields below repeat per task card. The section header shows the service name, site, and SO number for clarity (e.g. _"TASK 1: Cockroach Treatment @ Head Office (SO-2026-0112)"_).

#### Sub-Section 2A: Service Information (Read-Only)

| Field        | Type    | Required | Source                                  | Description                                                                                                |
| ------------ | ------- | -------- | --------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| Category     | Display | Auto     | GMA / SO service definition (Module 17) | Service category (e.g. General Pest Control). Read-only — auto-filled from the selected service line item. |
| Sub-category | Display | Auto     | GMA / SO service definition (Module 17) | Service sub-category (e.g. Cockroach Management). Read-only — auto-filled.                                 |
| Area (SQFT)  | Display | Auto     | GMA / SO service definition (Module 17) | Total area in square feet to be treated. Read-only — fetched from GMA sheet or SO line item.               |

#### Sub-Section 2B: Schedule & Contact (Editable)

| Field          | Type          | Required | Validation                                     | Description                                                                                                                   |
| -------------- | ------------- | -------- | ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Scheduled Date | Date Picker   | Yes      | Cannot be a past date                          | Date on which the task is to be executed. Defaults to the current calendar date.                                              |
| Start Time     | Time Dropdown | Yes      | Must be before End Time                        | Scheduled start time for this task (e.g. 08:00).                                                                              |
| End Time       | Time Dropdown | Yes      | Must be after Start Time                       | Scheduled end time for this task (e.g. 10:00).                                                                                |
| Duration       | Display       | Auto     | Auto-calculated (End Time − Start Time)        | Computed duration displayed as hours/minutes (e.g. "2 hours"). Read-only.                                                     |
| Site Contact   | Text Input    | Yes      | Auto-fetched from SO/Customer record; editable | Name of the on-site point of contact for the technician. Pre-filled from the SO customer record but the manager can override. |
| Contact Mobile | Phone Input   | Yes      | Valid 10-digit mobile number                   | Mobile number of the site contact. Pre-filled from SO/Customer; editable.                                                     |

#### Sub-Section 2C: Technician Assignment

| Field               | Type                    | Required | Validation                                              | Description                                                                                                                                                                                                                   |
| ------------------- | ----------------------- | -------- | ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Role                | Dropdown                | Yes      | Must select a role from Module 8 role definitions       | **Cascading filter:** On selection (e.g. Technician, Senior Technician, Supervisor), the "Available Employees" list below refreshes to show only employees with that role who are active and free on the scheduled date/time. |
| Available Employees | Multi-Select Checkboxes | Yes      | At least one employee must be selected                  | Cascading from _Role_ — shows only technicians of the selected role who have no conflicting tasks on the scheduled date/time. Multiple employees can be selected (one primary + others as support).                           |
| Primary Technician  | Dropdown                | Yes      | Must be one of the selected employees above             | Designates the main responsible technician for the task. The dropdown is populated only from the employees already checked in "Available Employees".                                                                          |
| Conflict Check      | Indicator (Auto)        | Auto     | System checks the schedules of all selected technicians | Visual status indicator. Displays ✅ **No conflicts detected** or ⚠ **Conflict: "[Tech Name] is assigned to TASK-XXXX at this time"**. If conflicts are detected, the system blocks task creation until resolved.             |

#### Sub-Section 2D: Materials / Chemicals (Auto-Fetched, Editable)

> **Data Source:** When a SO line item is selected, the system auto-fetches the chemical/product list from the **GMA Sheet (Module 17)** and **SO line items (Module 20)**. HSN, UOM, and Standard Qty are pre-filled; the manager can **edit Req Qty, add new rows, or remove rows**.

| Field                | Type         | Required | Validation                                            | Description                                                                                                                         |
| -------------------- | ------------ | -------- | ----------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| Chemical / Product   | Display      | Auto     | From GMA/SO service definition                        | Name of the chemical or product (e.g. Alpha Cypermethrin, Fipronil Gel, Bait Station). Read-only — auto-fetched.                    |
| HSN                  | Display      | Auto     | From Module 10 master data                            | HSN/SAC code for taxation (e.g. 3808, 3926). Read-only.                                                                             |
| UOM                  | Display      | Auto     | From Module 10 master data                            | Unit of measurement (e.g. ml, tube, nos). Read-only.                                                                                |
| Std Qty              | Display      | Auto     | From GMA/SO                                           | Standard quantity required per service as defined in the GMA/SO (e.g. 120 ml). Read-only — serves as a reference for the manager.   |
| Req Qty              | Number Input | Yes      | Must be ≥ 0; numeric only                             | **Editable** — the actual quantity the manager wants to allocate for this task. Defaults to Std Qty but can be adjusted up or down. |
| [+ Add Material]     | Button       | —        | Added chemical must exist in Module 10/11 master data | Opens a search/select dialog to manually add extra materials not in the auto-fetched list.                                          |
| [🗑 Remove Selected] | Button       | —        | Cannot remove if only 1 material row remains          | Removes selected material row(s) from the list. Disabled when only one row exists.                                                  |

---

### Step 3: Additional Notes

| Field      | Type     | Required | Validation                         | Description                                                                                                                              |
| ---------- | -------- | -------- | ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Task Notes | Textarea | No       | Max 500 characters                 | Free-text area for additional instructions, special care notes, or site-specific information (e.g. "Special care for server room area"). |
| Priority   | Dropdown | Yes      | Values: Normal / Urgent / Critical | Task priority level. Determines sort order and visual emphasis in the Daily Task View. Default: Normal.                                  |

---

## Tab 2 — From Customer Tickets (Re-Task): Field Descriptions

### Step 1: Select Source

#### Source Type Selection

| Field       | Type         | Required | Validation      | Description                                                                                                                                                                                |
| ----------- | ------------ | -------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Source Type | Radio Button | Yes      | Must select one | Determines how the Re-Task is sourced. Two options: **(•) Customer Ticket** or **( ) Manual Entry**. Default: Customer Ticket. Selecting an option conditionally shows/hides fields below. |

---

#### Condition A — When Source Type = **Customer Ticket**

> All fields below auto-fill from the selected ticket and its linked SO/service records. These are **read-only display** fields.

| Field              | Type                  | Required | Validation                                      | Description                                                                                                                              |
| ------------------ | --------------------- | -------- | ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Search Ticket      | Search (Autocomplete) | Yes      | Must select a valid ticket                      | Searchable dropdown — type Ticket ID or Customer Name. On selection, all fields below auto-populate from the linked ticket and SO data.  |
| Linked SO (if any) | Display               | Auto     | Auto-filled from ticket record                  | The original Sales Order number linked to this complaint (e.g. SO-2026-0087). Read-only. If no SO is linked, displays "—".               |
| Customer           | Display               | Auto     | Auto-filled from ticket/SO                      | Customer name linked to the ticket source (e.g. XYZ Hotel). Read-only.                                                                   |
| Category           | Display               | Auto     | Auto-filled from source service (GMA/Module 17) | Service category of the original service (e.g. General Pest Control). Read-only.                                                         |
| Sub-category       | Display               | Auto     | Auto-filled from source service (GMA/Module 17) | Service sub-category of the original service (e.g. Cockroach Management). Read-only.                                                     |
| Original Service   | Display               | Auto     | Auto-filled from ticket/SO line item            | The specific service name linked to the ticket (e.g. Rodent Control). Read-only.                                                         |
| Site               | Display               | Auto     | Auto-filled from ticket/SO                      | Site name where the original service was performed (e.g. Lobby). Read-only.                                                              |
| Complaint Reason   | Display               | Auto     | Auto-filled from ticket                         | The customer's complaint text from the ticket (e.g. "Rodents seen again within 2 days"). Read-only — shown as reference for the manager. |

---

#### Condition B — When Source Type = **Manual Entry**

> When no customer ticket exists (e.g. internal re-service decision, follow-up visit), the manager can manually enter all details. All fields below become **editable inputs** instead of auto-filled displays.

| Field              | Type                  | Required | Validation                                           | Description                                                                                                                              |
| ------------------ | --------------------- | -------- | ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Customer           | Search (Autocomplete) | Yes      | Must select a valid customer from Module 18          | Searchable dropdown to select the customer. On selection, the Site dropdown below filters to show only sites belonging to this customer. |
| Site               | Dropdown              | Yes      | Filtered by selected Customer (from Module 18 sites) | Select the site/location where the re-service will be performed. Options are filtered based on the selected Customer.                    |
| Category           | Dropdown              | Yes      | From GMA / Module 17 service definitions             | Select the service category (e.g. General Pest Control, Specialized Service).                                                            |
| Sub-category       | Dropdown              | Yes      | Cascading from Category (Module 17)                  | Select the service sub-category. Options filter based on the selected Category (e.g. Cockroach Management under General Pest Control).   |
| Original Service   | Dropdown              | Yes      | Cascading from Sub-category (Module 17)              | Select the specific service (e.g. Rodent Control). Options filter based on the selected Sub-category.                                    |
| Linked SO (if any) | Search (Autocomplete) | No       | Must be a valid SO for the selected Customer         | Optional — if re-service is related to an existing Sales Order, the manager can search and link it. Leave blank if not applicable.       |
| Complaint / Reason | Textarea              | No       | Max 500 characters                                   | Free-text to describe the reason for the manual re-task (e.g. "Follow-up visit as per manager instruction", "Preventive re-service").    |

---

### Step 2: Task Details (Editable)

> Whether the source is a Customer Ticket or Manual Entry, Step 2 fields are always **editable**. When sourced from a ticket, these fields are **pre-filled from the ticket/SO data** but can be changed. When sourced via Manual Entry, fields are populated from Step 1 selections.

#### Sub-Section 2A: Service & Location

| Field        | Type     | Required | Validation                                           | Description                                                                                                                                                                                                                    |
| ------------ | -------- | -------- | ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Service Type | Dropdown | Yes      | From Module 17 service definitions                   | The service to be performed for the Re-Task. **Pre-filled** from source ticket/Step 1 selection but editable — the manager may change the service type if needed (e.g. upgrading Rodent Control to a comprehensive treatment). |
| Site         | Dropdown | Yes      | Sites belonging to the selected customer (Module 18) | The location where the re-service will be performed. **Pre-filled** from source but editable.                                                                                                                                  |

#### Sub-Section 2B: Schedule & Contact

| Field          | Type          | Required | Validation                                     | Description                                                                              |
| -------------- | ------------- | -------- | ---------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Site Contact   | Text Input    | Yes      | Auto-fetched from SO/Customer record; editable | Name of the on-site point of contact. Pre-filled from source data; manager can override. |
| Contact Mobile | Phone Input   | Yes      | Valid 10-digit mobile number                   | Mobile number of site contact. Pre-filled from source data; editable.                    |
| Scheduled Date | Date Picker   | Yes      | Cannot be a past date                          | Date on which the re-task is to be executed.                                             |
| Start Time     | Time Dropdown | Yes      | Must be before End Time                        | Scheduled start time for the re-task.                                                    |
| End Time       | Time Dropdown | Yes      | Must be after Start Time                       | Scheduled end time for the re-task.                                                      |

#### Sub-Section 2C: Technician Assignment

| Field               | Type                    | Required | Validation                                        | Description                                                                                                                  |
| ------------------- | ----------------------- | -------- | ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Role                | Dropdown                | Yes      | Must select a role from Module 8 role definitions | Cascading filter for technician selection (e.g. Senior Technician, Technician, Supervisor).                                  |
| Available Employees | Multi-Select Checkboxes | Yes      | At least one employee must be selected            | Cascading from _Role_ — shows only technicians of the selected role who are active and available on the scheduled date/time. |
| Primary Technician  | Dropdown                | Yes      | Must be one of the selected employees above       | Main responsible technician for the re-task. Populated only from checked employees.                                          |

#### Sub-Section 2D: Materials / Chemicals (Auto-Fetched, Editable)

> **Data Source:** Auto-fetched from the **original SO/service linked** to the ticket (Condition A) or from the **GMA sheet based on Step 1 service selections** (Condition B). HSN, UOM, and Standard Qty are pre-filled; the manager can **edit Req Qty, add new rows, or remove rows**.

| Field                | Type         | Required | Validation                          | Description                                                           |
| -------------------- | ------------ | -------- | ----------------------------------- | --------------------------------------------------------------------- |
| Chemical / Product   | Display      | Auto     | From GMA/SO service definition      | Auto-fetched chemical or product name. Read-only.                     |
| HSN                  | Display      | Auto     | From Module 10 master data          | HSN/SAC code (read-only).                                             |
| UOM                  | Display      | Auto     | From Module 10 master data          | Unit of measurement (read-only).                                      |
| Std Qty              | Display      | Auto     | From GMA/SO                         | Standard quantity per service (read-only).                            |
| Req Qty              | Number Input | Yes      | Must be ≥ 0; numeric only           | **Editable** — manager can adjust per task need. Defaults to Std Qty. |
| [+ Add Material]     | Button       | —        | Chemical must exist in Module 10/11 | Manually add extra materials not in auto-list.                        |
| [🗑 Remove Selected] | Button       | —        | Cannot remove if only 1 row remains | Remove unnecessary materials from the list.                           |

#### Sub-Section 2E: Additional Notes

| Field      | Type     | Required | Validation                         | Description                                                                                                 |
| ---------- | -------- | -------- | ---------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| Task Notes | Textarea | No       | Max 500 characters                 | Additional instructions for the re-task (e.g. "Re-service due to complaint").                               |
| Priority   | Dropdown | Yes      | Values: Normal / Urgent / Critical | Task priority level. Default: **Urgent** for ticket-sourced re-tasks; **Normal** for manual entry re-tasks. |

---

## Validation Rules

| Validation                                             | Rule                                                                                                          |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------- |
| Time slot overlap                                      | System checks if selected technician has a conflicting task                                                   |
| Date validity                                          | Cannot be in the past                                                                                         |
| Start < End                                            | Start time must be before end time                                                                            |
| Role → Employee cascade                                | Employee list filtered by role; only active employees shown                                                   |
| At least 1 technician                                  | Minimum one technician must be assigned                                                                       |
| Primary in selected list                               | Primary must be one of the multi-selected employees                                                           |
| SO line not yet tasked                                 | Warns if service line already has a task for the same date                                                    |
| Material Qty ≥ 0                                       | Required qty cannot be negative                                                                               |
| Material from master data                              | Added materials must exist in Module 10/11                                                                    |
| **[Tab 2 — Customer Ticket]** Ticket required          | If Source Type = Customer Ticket, a valid ticket must be selected                                             |
| **[Tab 2 — Manual Entry]** Customer & Service required | If Source Type = Manual Entry, Customer, Site, Category, Sub-category, and Original Service are all mandatory |
| **[Tab 2 — Manual Entry]** Cascading dropdowns         | Sub-category cascades from Category; Original Service cascades from Sub-category                              |

---

## Form Actions

| Button             | Applicable Tab       | Description                                                                                                                                       |
| ------------------ | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Create Tasks**   | Tab 1 (From SO)      | Validates all task cards, checks technician conflicts, creates task records (one per selected service line item), and returns to Daily Task View. |
| **Create Re-Task** | Tab 2 (From Tickets) | Same as above but marks the task type as **Re-Task** and links it to the source ticket/manual reason.                                             |
| **Cancel**         | Both Tabs            | Discards form data and returns to the previous screen (Daily Task View or Calendar Dashboard).                                                    |

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
│  │  Site Contact   : Rajesh              Mobile        : +919876543210   │ │
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

| Field          | Type    | Description                                                      |
| -------------- | ------- | ---------------------------------------------------------------- |
| SO Number      | Link    | **[Auto-fetched]** Sales Order reference; navigates to Module 20 |
| Contract Ref   | Link    | **[Auto-fetched]** Contract reference; navigates to Module 19    |
| Customer       | Display | **[Auto-fetched]** Customer name (from Module 18)                |
| Customer ID    | Display | **[Auto-fetched]** Unique ID (e.g. CUST-00245)                   |
| Site & Site ID | Display | **[Auto-fetched]** Site name and ID (e.g. SITE-00312)            |
| Address        | Display | **[Auto-fetched]** Site address from source                      |
| Country        | Display | **[Auto-fetched]** Country of the site                           |
| Google Map URL | Link    | **[Auto-fetched]** Clickable Google Maps link                    |
| Site Contact   | Display | **[Configured at Creation]** POC Name for the tech               |
| Contact Mobile | Display | **[Configured at Creation]** POC Phone for the tech              |
| Branch         | Display | **[Auto-fetched]** Servicing branch                              |
| Category       | Display | **[Auto-fetched]** Service Category (e.g. General Pest Control)  |
| Subcategory    | Display | **[Auto-fetched]** Service Sub-Category (e.g. Cockroach Mgmt)    |

---

## Task Execution Fields (Read-Only)

| Field             | Type     | Description                                                     |
| ----------------- | -------- | --------------------------------------------------------------- |
| Task ID           | Display  | **[Auto-generated]** Unique task ID                             |
| Task Type         | Badge    | **[System-set]** Normal / Re-Task                               |
| Service Mode      | Badge    | **[Auto-fetched]** Contract Base / One-Time Service             |
| Pest(s) Treated   | Display  | **[Auto-fetched]** Target pests                                 |
| Area (SQFT)       | Display  | **[Auto-fetched]** Total area covered/treated                   |
| Status            | Badge    | **[System-driven]** Pending / In Progress / Completed / Overdue |
| Priority          | Badge    | **[Auto/Manual]** Normal / Urgent / Critical                    |
| Scheduled Date    | Date     | **[Manual Update]** Planned execution date                      |
| Time Slot         | Display  | **[Manual Update]** Start – End time                            |
| Primary Tech      | Display  | **[Manual Update]** Main technician + role                      |
| Support Techs     | Display  | **[Manual Update]** Additional technicians                      |
| Started At        | DateTime | **[System-captured]** Actual start timestamp                    |
| Completed At      | DateTime | **[System-captured]** Actual completion timestamp               |
| Actual Duration   | Display  | **[System-calculated]** Real time taken                         |
| Completion Note   | Text     | **[Tech Input]** Technician's service notes                     |
| Photos            | Image    | **[Tech Input]** Before/after/treatment images                  |
| Customer Rating   | Display  | **[Customer Input]** Star rating (1–5)                          |
| Customer Feedback | Text     | **[Customer Input]** Written feedback                           |

---

## Form Actions

| Button          | Condition        | Description                           |
| --------------- | ---------------- | ------------------------------------- |
| **Reschedule**  | Status = Pending | Opens Reschedule/Reassign form (21.7) |
| **Print / PDF** | All statuses     | Downloads/prints task detail as PDF   |

---

## Materials / Chemicals Detail (View)

| Field         | Type    | Description                                            |
| ------------- | ------- | ------------------------------------------------------ |
| Chemical Name | Display | **[Auto-fetched]** Name of the material used           |
| HSN           | Display | **[Auto-fetched]** HSN code for tax reporting          |
| UOM           | Display | **[Auto-fetched]** Unit of measurement                 |
| Required Qty  | Display | **[Auto-fetched]** Planned quantity from task creation |
| Used Qty      | Display | **[Tech Input]** Actual quantity logged by technician  |

---

## Execution & Audit Log

| Field           | Type     | Description                               |
| --------------- | -------- | ----------------------------------------- |
| Started At      | DateTime | Actual task start time                    |
| Completed At    | DateTime | Actual task completion time               |
| Actual Duration | Display  | Time elapsed (Completed At - Started At)  |
| Photos          | Image    | Click to view Before/After/Area photos    |
| Audit Timestamp | DateTime | When the change occurred                  |
| Audit Action    | Display  | Created / Assigned / Started / Completed  |
| Audit By        | Display  | Name of the user who performed the action |

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
│  ─── SCHEDULE & CONTACT (Editable) ─────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Scheduled Date*   : [📅 23 Mar 2026]                                   │ │
│  │  Start Time*       : [▼ 14:00 ▼]                                       │ │
│  │  End Time*         : [▼ 16:00 ▼]                                       │ │
│  │  Duration          : 2 hours (auto)                                     │ │
│  │  Site Contact*     : [Rajesh_______________]                        │ │
│  │  Contact Mobile*   : [+919876543210        ]                        │ │
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

| Field       | Type    | Description                                             |
| ----------- | ------- | ------------------------------------------------------- |
| SO Number   | Display | **[Auto-fetched]** Original Sales Order reference       |
| Customer    | Display | **[Auto-fetched]** Customer name                        |
| Category    | Display | **[Auto-fetched]** Service Category                     |
| Subcategory | Display | **[Auto-fetched]** Service Subcategory                  |
| Service     | Display | **[Auto-fetched]** Specific service name                |
| Site        | Display | **[Auto-fetched]** Target site location                 |
| Task Type   | Badge   | **[System-set]** Normal / Re-Task                       |
| Created By  | Display | **[Auto-fetched]** User who originally created the task |

---

## Editable Fields (Manual Update)

| Field               | Type      | Required | Validation                        | Description                                        |
| ------------------- | --------- | -------- | --------------------------------- | -------------------------------------------------- |
| Scheduled Date      | Date      | Yes      | Cannot be past date               | **[Manual Update]** Task execution date            |
| Start Time          | Time      | Yes      | Must be before End Time           | **[Manual Update]** Task start time                |
| End Time            | Time      | Yes      | Must be after Start Time          | **[Manual Update]** Task end time                  |
| Site Contact        | Text      | Yes      | Required                          | **[Manual Update]** Site point of contact          |
| Contact Mobile      | Phone     | Yes      | Valid 10-digit number             | **[Manual Update]** Mobile string for dispatcher   |
| Role                | Dropdown  | Yes      | From Module 8 role definitions    | **[Manual Update]** Cascading filter for employees |
| Available Employees | Multi-sel | Yes      | At least one selected             | **[Auto-filtered/Manual Select]** Multi-select     |
| Primary Technician  | Dropdown  | Yes      | Must be one of selected employees | **[Manual Update]** Main responsible person        |
| Priority            | Dropdown  | Yes      | Normal / Urgent / Critical        | **[Manual Update]** Task priority                  |
| Task Notes          | Textarea  | No       | Max 500 chars                     | **[Manual Update]** Additional instructions        |

---

## Form Actions

| Button           | Description                                                    |
| ---------------- | -------------------------------------------------------------- |
| **Save Changes** | Validates, checks conflicts, saves, logs change in audit trail |
| **Cancel**       | Discards changes and returns to Task Detail                    |

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

| Field          | Type    | Description                     |
| -------------- | ------- | ------------------------------- |
| Customer / ID  | Display | Customer Name and unique ID     |
| Site / ID      | Display | Site Name and unique ID         |
| Category       | Display | Service Category                |
| Subcategory    | Display | Service Subcategory             |
| Service        | Display | Specific service name           |
| Service Mode   | Badge   | Contract Base / One-Time        |
| Scheduled Date | Display | Planned date                    |
| Time Slot      | Display | Scheduled start – end           |
| Assigned Techs | Display | Primary and Support technicians |

---

## Completion Fields

| Field             | Type        | Required | Validation                       | Description                             |
| ----------------- | ----------- | -------- | -------------------------------- | --------------------------------------- |
| Actual Start Time | Time        | Yes      | Must be ≤ Actual End Time        | When the technician started             |
| Actual End Time   | Time        | Yes      | Must be ≥ Actual Start Time      | When the technician finished            |
| Actual Duration   | Display     | Auto     | Auto-calculated                  | End – Start                             |
| Actually Used Qty | Number      | Yes      | Must be ≥ 0; cannot exceed stock | Quantity of each chemical actually used |
| Before Photo      | File Upload | Yes      | JPG, PNG (Max 5MB)               | Mandatory photo before service          |
| After Photo       | File Upload | Yes      | JPG, PNG (Max 5MB)               | Mandatory photo after service           |
| Treatment Photo   | File Upload | No       | JPG, PNG (Max 5MB)               | Optional photo of treated area          |
| Completion Notes  | Textarea    | Yes      | Min 10 chars, Max 1000 chars     | Technician's summary of work            |
| Customer Rating   | Rating      | Yes      | 1 to 5 Stars                     | Customer's satisfaction level           |
| Client Feedback   | Textarea    | No       | Max 500 chars                    | Written feedback from customer          |

---

## Stock Deduction Logic

| Event                | System Behaviour                                                  |
| -------------------- | ----------------------------------------------------------------- |
| Form submitted       | "Actually Used" quantities deducted from branch stock (Module 11) |
| Extra material added | System checks availability before allowing submission             |
| Qty exceeds stock    | Warning: "Insufficient stock for Alpha Cypermethrin at Mumbai"    |
| Qty = 0              | No deduction; material row kept for record purposes               |

---

## Form Actions

| Button                | Description                                                        |
| --------------------- | ------------------------------------------------------------------ |
| **Mark as Completed** | Validates, deducts stock, updates task status, logs in audit trail |
| **Cancel**            | Returns to task detail without changes                             |

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

| Field         | Type    | Description                                 |
| ------------- | ------- | ------------------------------------------- |
| Customer      | Display | Customer name                               |
| Service       | Display | Service type                                |
| Category      | Display | Service Category                            |
| Sub-category  | Display | Service Sub-Category                        |
| Site          | Display | Site location                               |
| Branch        | Display | Responsible branch                          |
| Current Date  | Date    | Existing scheduled date                     |
| Time Slot     | Display | Existing time slot                          |
| Primary Tech  | Display | Existing primary assigned technician        |
| Support Techs | Display | Existing additional technicians assignments |

---

## Reschedule Fields

| Field               | Type      | Required | Validation                        | Description                           |
| ------------------- | --------- | -------- | --------------------------------- | ------------------------------------- |
| New Date            | Date      | Yes      | Cannot be past date               | Rescheduled execution date            |
| New Start Time      | Time      | Yes      | Must be before New End Time       | New start time                        |
| New End Time        | Time      | Yes      | Must be after New Start Time      | New end time                          |
| Reassign Checkbox   | Checkbox  | No       | Default: unchecked                | Toggle technician reassignment        |
| Role                | Dropdown  | Cond.    | Required if Reassign checked      | Cascading filter for employee list    |
| Available Employees | Multi-sel | Cond.    | Required if Reassign checked      | Filtered by Role + new date/time slot |
| Primary Technician  | Dropdown  | Cond.    | Must be one of selected employees | New primary technician                |
| Reason for Change   | Dropdown  | Yes      | Must select from list             | Categorised reason for change         |
| Additional Notes    | Textarea  | No       | Max 500 chars                     | Free-text explanation                 |

---

## Validation Rules

| Validation                 | Rule                                                                  |
| -------------------------- | --------------------------------------------------------------------- |
| Date validity              | Cannot be in the past                                                 |
| Time validity              | New Start < New End                                                   |
| Conflict check on new slot | System validates no overlap for selected technicians on new date/time |
| Reason required            | Must select a reason category                                         |
| Cascade on Reassign        | If Reassign checked, Role + Employees + Primary are all required      |

---

## Form Actions

| Button                 | Description                                                    |
| ---------------------- | -------------------------------------------------------------- |
| **Confirm Reschedule** | Validates, updates task, logs old vs new values in audit trail |
| **Cancel**             | Returns to task detail without changes                         |

---

================================================================================

# Access Control (RBAC)

| Role                   | Permissions                                                             |
| ---------------------- | ----------------------------------------------------------------------- |
| **Technician Manager** | Full access: Create, Edit, Reschedule, Assign, Complete, View all tasks |
| **Operations Head**    | Full access: Same as Technician Manager + reporting                     |
| **Senior Technician**  | View own tasks, Mark completion, Log materials                          |
| **Technician**         | View own tasks, Mark completion, Log materials                          |
| **Company Admin**      | View all tasks, Reports (read-only)                                     |

> **Note:** No approval workflow is required. Tasks are created and managed directly by the Technician Manager without managerial sign-off.

---

================================================================================

# Business Rules

| Rule                             | Description                                                       |
| -------------------------------- | ----------------------------------------------------------------- |
| Task ID Format                   | `TASK-YYYY-NNNN` (auto-generated, sequential per year)            |
| No scheduling conflicts          | Same technician cannot be double-booked in overlapping time slots |
| Stock deduction on completion    | Materials used are deducted from branch stock in Module 11        |
| Overdue auto-marking             | Tasks past their scheduled date with status ≠ Completed → Overdue |
| Re-Task linkage                  | Re-Tasks maintain link to original SO and Customer Ticket         |
| Audit trail                      | All create, edit, reschedule, and complete actions are logged     |
| Role → Employee cascade          | Employee list is always filtered by selected Role (Module 8)      |
| Primary must be in selected list | Primary Technician must be one of the multi-selected employees    |

---

> **Future Module:** Customer Support Ticket creation and management module (referenced in Re-Task flow, Tab 2 of Screen 21.3).

---

---

# ================================================================================

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

# 22.1 Live Tracking Dashboard

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
│  │ Date  : [📅 Today ▼]    │ │              [ MAP VIEW ]    [refresh button]││
│  │ Branch: [▼ All ▼]       │ │                                              ││
│  │ Techs : [🔍 Search ▼]   │ │       📍(Ravi)               📍(Anjali)      ││
│  │                         │ │                                              ││
│  │ [All] [🟢 Active] [⚪ Offline]│ │                                              ││
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

| Field              | Type    | Description                                                  |
| ------------------ | ------- | ------------------------------------------------------------ |
| Date Selector      | Date    | Default: Today (Live). Select past dates for historical Map. |
| Technician Name    | Link    | Clicks through to Technician Travel Log (22.2).              |
| Current Location   | Display | Nearest address or specific Site Name (from Module 21).      |
| Feed Tabs          | Tab     | Toggle between Active techs map vs Offline techs list        |
| Current Status     | Badge   | Travelling / On Site / Idle / Offline.                       |
| Customer & Service | Display | Linked Customer Name and specific Service Type.              |
| Active Task        | Link    | Current Module 21 Task ID (if On Site or Travelling to it).  |

---

## Map Interactions

| Action                 | Result                                                               |
| ---------------------- | -------------------------------------------------------------------- |
| **Select Past Date**   | Updates map to show all drawn routes for selected techs on that day. |
| **Hover on Map Pin**   | Shows mini-tooltip with Name, Speed/Time, and Assigned Task.         |
| **Click on Feed Item** | Pans/Zooms map to the selected technician's specific location.       |

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
│  │ Total Distance: 42.5 KM              │ │            [refresh button]   ││
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

| Field           | Type     | Description                                                                   |
| --------------- | -------- | ----------------------------------------------------------------------------- |
| Period Selector | Dropdown | **[Manual Select]** Select Daily, Weekly, or Monthly view.                    |
| Date Selector   | Control  | **[Manual Select]** Selects the specific date, week, or month to analyze.     |
| Summary Stats   | Display  | **[Auto-calculated]** Aggregated Distance, Time, and Tasks for chosen period. |

---

## Timeline & Tabular Log Fields (Read-Only)

| Field            | Type    | Description                                                                |
| ---------------- | ------- | -------------------------------------------------------------------------- |
| Date             | Display | **[System-generated]** Tracking Date.                                      |
| Departure Point  | Display | **[Auto-fetched]** Origin site or generic location (e.g., Branch).         |
| Destination      | Display | **[Auto-fetched]** Destination Site Name or Geo-location address.          |
| Customer         | Display | **[Auto-fetched]** Associated Customer Name and specific Service Type.     |
| Task ID          | Modal   | **[Auto-fetched]** Click Task to view details (Status, Category, Techs).   |
| Distance         | Display | **[System-calculated]** GPS calculated travel distance for that segment.   |
| Start & End Time | Display | **[System-captured]** Timestamps marking the departure and arrival bounds. |
| Duration         | Display | **[System-calculated]** Total time computed between Start and End.         |
| Status           | Badge   | **[System-driven]** On-Site, Travelling, Idle.                             |

---

================================================================================

# Business Rules & Logic (Module 22)

| Rule                     | Description                                                                  |
| ------------------------ | ---------------------------------------------------------------------------- |
| **GPS Polling Rate**     | Technician mobile app sends GPS coordinates every X minutes/meters.          |
| **Task Auto-Arrival**    | Proximity to Module 21 Site Lat/Long automatically logs an 'Arrived' event.  |
| **Idle Alert**           | If GPS is stationary for > Y minutes not near a task site, flag as 'Idle'.   |
| **Background Tracking**  | GPS tracking must only be active between Clock-in and Clock-out events.      |
| **Distance Calculation** | System calculates route segments and aggregates them for Daily/Weekly stats. |

---

================================================================================

# 🎯 MODULE 23: CUSTOMER SUPPORT MANAGEMENT (SLA-DRIVEN)

## Overview

Live Location & Travel Tracking provided real-time operational oversight. **Module 23** provides enterprise-grade customer support by enabling the recording, tracking, and resolution of customer complaints and requests via a strict **SLA (Service Level Agreement)** framework.

This module guarantees time-bound responses and automatically escalates tickets that breach resolution times. It tightly integrates with **Module 21 (Task Management)** to dispatch technicians for re-services stemming from support tickets.

**Module Connections:**

- **Depends on:** Module 18 (Customer Master), Module 20 (Sales Orders), Module 21 (Task Management — linked tasks/re-services), Module 8 (Employee).
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
│  │ ID   │ Customer │ SO No  │ Task ID        │ Priority│ Status  │      │  │
│  │──────┼──────────┼────────┼────────────────┼─────────┼─────────┼──────│  │
│  │ T001 │ ABC Corp │ SO-101 │ TASK-2026-0180 │🔴 Urgent│ Open    │      │  │
│  │ T002 │ PQR Ind  │ SO-089 │ TASK-2026-0192 │🟡 High  │ Open    │      │  │
│  │ T003 │ XYZ Hotel│ SO-144 │ —              │🟢 Normal│ In Prog │      │  │
│  │ T004 │ LMN Pvt  │ SO-201 │ TASK-2026-0201 │🟢 Normal│ Open    │      │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│  ┌────────────────────────────────────────────────────────────────────────┐  │
│  │ SLA Stage │ SLA Timer │ Actions                                       │  │
│  │───────────┼───────────┼───────────────────────────────────────────────│  │
│  │ 🔴 Breach │ -02h 15m  │ [👁 View]                                     │  │
│  │ 🟡 Risk   │ 00h 45m   │ [👁 View]                                     │  │
│  │ 🟢 Safe   │ 22h 10m   │ [👁 View]                                     │  │
│  │ 🟢 Safe   │ 47h 00m   │ [👁 View]                                     │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│    Shows 1 to 4 of 42 tickets.                     [ < Previous | Next > ]   │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Filter Fields

| Field            | Type       | Description                                                                    |
| ---------------- | ---------- | ------------------------------------------------------------------------------ |
| Branch Name      | Dropdown   | Filter by servicing branch: `[All, Head Office, North, South, East, West]`.    |
| Date Range       | Date Range | Filter by Ticket Creation Date.                                                |
| Status           | Dropdown   | Standard lifecycle: `[Open, Assigned, In Progress, Paused, Resolved, Closed]`. |
| Priority         | Dropdown   | Ticket Priority: `[All, Normal, High, Urgent, Critical]`.                      |
| SLA Status       | Dropdown   | SLA Health: `[Safe (🟢), At Risk (🟡), Breached (🔴)]`.                        |
| Escalation Level | Dropdown   | Triggered SLA: `[None, L1 (Soft), L2 (Manager), L3 (Critical)]`.               |

---

## Search Fields

| Field         | Type   | Description                                                                 |
| ------------- | ------ | --------------------------------------------------------------------------- |
| Ticket ID     | Search | Direct lookup by `TKT-YYYY-NNNN`. Navigates straight to 23.3 Detail View.   |
| Customer Name | Search | Global free-text search against Customer Master (Module 18).                |
| SO Number     | Search | Search by linked Sales Order `SO-YYYY-NNNN` to find all associated tickets. |
| Task ID       | Search | Search by linked Task `TASK-YYYY-NNNN` from Module 21.                      |

---

## Table Columns

| Field         | Type    | Description                                                                                                  |
| ------------- | ------- | ------------------------------------------------------------------------------------------------------------ |
| Ticket ID     | Link    | `TKT-YYYY-NNNN`. Clicks to 23.3 Detail View.                                                                 |
| Customer Name | Display | Name of the Customer.                                                                                        |
| Branch Name   | Display | Name of Branch                                                                                               |
| SO No         | Display | Associated Sales Order (if applicable).                                                                      |
| Task ID       | Link    | Linked Task from Module 21 (`TASK-YYYY-NNNN`). Clicks to Module 21 Task Detail. Shows `—` if no task linked. |
| Priority      | Display | Ticket Priority.                                                                                             |
| Status        | Display | Standard lifecycle: `[Open, Assigned, In Progress, Paused, Resolved, Closed]`.                               |
| SLA Stage     | Badge   | Color-coded visual of SLA health (Safe/Risk/Breach).                                                         |
| SLA Timer     | Display | Live countdown (e.g. `22h 10m` remaining, `-02h 15m` overdue).                                               |
| Actions       | Button  | `[👁 View]` — Opens Ticket Detail View (Screen 23.3).                                                        |

## Actions (Table Row)

| Action   | Icon | Condition    | Description                            |
| -------- | ---- | ------------ | -------------------------------------- |
| **View** | 👁   | All statuses | Opens Ticket Detail View (Screen 23.3) |

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
│  │ Related Task  : [▼ TASK-2026-0180 ▼] (Optional — from Module 21)      │ │
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

> [!NOTE]
> **Cascading Selection Logic:**
>
> - **Customer Select** ➔ Fetches all **Sales Orders (SO)** of that customer.
> - **SO Select** ➔ Fetches all **Tasks** of that specific SO from Module 21.

---

## Field Table

| Field         | Type     | Required | Validation                        | Description                                                                                                                                   |
| ------------- | -------- | -------- | --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Customer      | Search   | Yes      | Must select from Module 18        | Linked Customer Profile.                                                                                                                      |
| Related SO    | Dropdown | No       | Filtered by selected Customer     | Links ticket to Sales Order context: `[List of user's active SO-YYYY-NNNN]`.                                                                  |
| Related Task  | Dropdown | No       | Filtered by selected Customer/SO  | Links ticket to Task context from Module 21: `[List of TASK-YYYY-NNNN for selected Customer/SO]`. Shows tasks that are Completed/In Progress. |
| Reported By   | Text     | Yes      | —                                 | Name of the person reporting the issue.                                                                                                       |
| Phone Number  | Text     | Yes      | Valid Phone format                | Contact number for callbacks.                                                                                                                 |
| Ticket Type   | Dropdown | Yes      | Values from Admin config          | Issue category: `[Complaint - Re-emergence, Complaint - Staff, Query - Billing, Query - Service]`.                                            |
| Priority      | Dropdown | Yes      | Auto-sets based on Type, editable | Drives the SLA timer: `[Normal, High, Urgent, Critical]`.                                                                                     |
| Subject       | Text     | Yes      | Max 100 chars                     | Short summary of the issue.                                                                                                                   |
| Description   | Textarea | Yes      | Max 1000 chars                    | Full details of the complaint.                                                                                                                |
| Expected Date | Date     | Yes      | Cannot be past date               | Manager manually asserts proper resolution date.                                                                                              |
| Expected Time | Time     | Yes      | —                                 | Manager manually asserts proper resolution time.                                                                                              |
| Attachments   | File     | No       | Max 5 files, 5MB each             | Image/PDF evidence of the issue.                                                                                                              |

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
│  │ Task ID : TASK-2026-0180         │ │                                   │ │
│  │ Type    : Complaint              │ │ ⏱ Response : ✅ Met (09:15 AM)    │ │
│  │ Created : 24 Mar 2026, 09:00 AM  │ │ ⏱ Resolut. : 🔴 -02h 15m (Breach) │ │
│  │ Caller  : Suresh (9876543210)    │ │ 🚨 Esc. Lvl : Level 2 (Manager)   │ │
│  │ Desc    : Treated 2 days ago...  │ │                                   │ │
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

| Component         | Description                                                                   |
| ----------------- | ----------------------------------------------------------------------------- |
| **SLA & Status**  | Live updates of the dual-timers. Flags if the SLA has escalated to L1/L2/L3.  |
| **Actions Menu**  | Direct triggers for popups covering Assignment, Task mapping, and Resolution. |
| **Activity Line** | Immutable audit log of all status changes, notes, calls, and task creations.  |

---

## Ticket Details (Read-Only)

| Field       | Type    | Description                                                   |
| ----------- | ------- | ------------------------------------------------------------- |
| Subject     | Display | **[Auto-fetched]** Short description of the issue.            |
| Customer    | Display | **[Auto-fetched]** Customer name (from Module 18).            |
| SO No       | Link    | **[Auto-fetched]** Linked Sales Order.                        |
| Task ID     | Link    | **[Auto-fetched]** Linked Module 21 Task ID.                  |
| Type        | Display | **[Auto-fetched]** Complaint / Request / Inquiry.             |
| Created     | Display | **[System-generated]** Timestamp of ticket creation.          |
| Caller      | Display | **[Auto-fetched]** Contact person and number.                 |
| Description | Text    | **[Auto-fetched]** Full issue details.                        |
| Status      | Badge   | **[System-driven]** Current ticket status.                    |
| Priority    | Badge   | **[System-driven]** Priority level (determines SLA).          |
| Assigned To | Display | **[Auto-fetched]** Current Support Agent handling the ticket. |
| Esc. Lvl    | Badge   | **[System-driven]** Current Escalation Level (L1/L2/L3).      |

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

| Field              | Type     | Required | Description                                                                                  |
| ------------------ | -------- | -------- | -------------------------------------------------------------------------------------------- |
| Assign To (Role)   | Dropdown | Yes      | Destination Role: `[Support Agent, Senior Technician, Operations Manager, Quality Control]`. |
| Assign To (Person) | Dropdown | Yes      | Cascading list based on Role: `[List of active Employee Names]`.                             |
| Assignment Note    | Textarea | No       | Optional handover notes                                                                      |

---

================================================================================

# 23.5 Convert Ticket to Task (Modal / Flow)

**Description:**
The crucial bridge between Customer Support and Operations. Converts the complaint into an actionable Re-Task in Module 21.

**SLA Impact:**

- Converting to a task **does not stop** the Ticket's Resolution SLA.
- The SLA only stops when the Technician _completes_ the Re-Task in the field AND the Support Agent _resolves_ the ticket.

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

> 📌 _For the complete Re-Task form layout, field validations, and technician assignment logic — **see Module 21 → Screen 21.3 → Tab 2**._

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
│  Customer Rating* : [⭐] [⭐] [▼ Rating (1-5) ▼]        │
│  Cust. Feedback*  : [Customer satisfied with prompt   ] │
│                     [response.                        ] │
│                                                          │
│  Attachments      : [+ Upload Customer Sign-off PDF]    │
│                                                          │
│       [RESOLVE TICKET & STOP SLA]       [CANCEL]         │
└─────────────────────────────────────────────────────────┘
```

## Screen Logic & Validation

1. **Validation:** System checks Module 21. If any Linked Tasks (`TASK-2026-NNNN`) are NOT marked "Completed", the system **blocks** resolution:
   > ❌ _Error: Cannot resolve ticket. Linked Task (TASK-2026-0205) is currently 'In Progress'._
2. **Success:** If no pending tasks, stops the Resolution SLA Timer.

| Field            | Type     | Required | Description                                                                                                       |
| ---------------- | -------- | -------- | ----------------------------------------------------------------------------------------------------------------- |
| Resolution Code  | Dropdown | Yes      | Reason for closure: `[Service Resolved Success, False Alarm / Not Found, Duplicate Ticket, Unresolved (Closed)]`. |
| Resolution Notes | Textarea | Yes      | Detailed explanation of how issue was solved                                                                      |
| Customer Rating  | Dropdown | Yes      | Support agent asks customer to rate service (1-5 Stars).                                                          |
| Cust. Feedback   | Textarea | Yes      | Description of customer's actual remarks and feedback on resolution.                                              |
| Attachments      | File     | No       | Sign-offs, final photos, emails                                                                                   |

---

================================================================================

# 23.7 Close Ticket (Modal)

**Description:**
Fired by the Support Agent or Manager as the final, immutable step to permanently lock the ticket. Can only be triggered when Status is `Resolved` or under administrative override.

## Screen Layout

```
┌─────────────────────────────────────────────────────────┐
│  CLOSE TICKET: TKT-2026-0042                            │
│                                                          │
│  🚨 Warning: This action cannot be undone. Closed        │
│  tickets can only be reopened by a System Admin.         │
│                                                          │
│  Close Reason*    : [▼ Resolved to Customer Satisfaction ▼] │
│                     • Resolved to Customer Satisfaction   │
│                     • Duplicate Request                   │
│                     • Customer Non-Responsive             │
│                     • Out of Scope                        │
│                                                          │
│  Closure Remarks* : [Final follow up done. Customer   ] │
│                     [is satisfied with outcome.       ] │
│                                                          │
│       [PERMANENTLY CLOSE TICKET]        [CANCEL]         │
└─────────────────────────────────────────────────────────┘
```

## Screen Logic & Validation

| Field           | Type     | Required | Description                                                         |
| --------------- | -------- | -------- | ------------------------------------------------------------------- |
| Close Reason    | Dropdown | Yes      | Standardized categorical reason for closing the ticket permanently. |
| Closure Remarks | Textarea | Yes      | Final notes wrapping up the entire ticket lifecycle.                |

---

---

================================================================================

# ⚙️ Dual SLA & Escalation Engine Logic

### 1. Dual SLA Architecture

The system independently tracks two timers for every ticket:

- **Response SLA Timer:** Starts immediately when `Created`. Stops completely the moment an agent triggers `[Add Note / Reply]` or `[Assign]`.
- **Resolution SLA Timer:** Starts immediately when `Created`. Does not stop until the ticket is marked `[Resolved]`.

### 2. SLA Pausing

- If a ticket requires input from the Customer (e.g., waiting on them to approve a re-visit time), the agent can click `[Pause Ticket]`.
- Both SLA timers **pause**.
- When the customer replies, or the agent clicks `[Resume]`, timers start from where they left off.

### 3. Automated 3-Level Escalation Engine

A cron/background service constantly checks active timers against the SLA Configuration Matrix (23.7).

- **Trigger L1 (Soft):** Response SLA breaches (Fixed at 2 hours post-creation with no agent action).
  - _Action:_ Ticket changes color to 🟡. Email sent to assigning manager.
- **Trigger L2 (Manager Alert):** The ticket reaches **80% of the duration** between `Created Date` and the manually entered `Expected Date/Time`.
  - _Action:_ Email alert sent to Operations Head. Flagged in Dashboards.
- **Trigger L3 (Critical Breach):** Current time exceeds the manually entered `Expected Date/Time`.
  - _Action:_ Ticket status changed to "SLA Breached" 🔴. Alert sent to Company Admin / CEO roles.

### 4. SLA Status Conditional Logic

The SLA Status indicator (🟢, 🟡, 🔴) found throughout the ticketing dashboards automatically updates its visual state by evaluating the `Current Time` against the `Expected Date/Time` entered in **Screen 23.2**.

- **Safe (🟢)**:
  - **Condition:** `Current Time` < `80% of Completion Window`.
  - **Definition:** The ticket is comfortably within the resolution timeline set by the manager.
- **At Risk (🟡)**:
  - **Condition:** `Current Time` >= `80% of Completion Window` AND `Current Time` < `Expected Date/Time`.
  - **Definition:** The ticket has entered the final 20% of its allotted time. It needs immediate attention.
- **Breached (🔴)**:
  - **Condition:** `Current Time` >= `Expected Date/Time`.
  - **Definition:** Overdue. The ticket failed to be resolved by the manually asserted deadline.

---

================================================================================

# Flow Status Definitions

## Ticket Lifecycle Matrix

| Status      | Description                                    | Next Allowed Statuses | SLA Timer Running? |
| ----------- | ---------------------------------------------- | --------------------- | ------------------ |
| Open        | Fresh ticket, unassigned.                      | Assigned, In Progress | YES                |
| Assigned    | Support Agent tagged, no action taken yet.     | In Progress           | YES                |
| In Progress | Being worked on, or Module 21 task dispatched. | Paused, Resolved      | YES                |
| Paused      | Waiting indefinitely on Customer.              | In Progress           | NO (Paused)        |
| Resolved    | Issue fixed, final confirmation pending.       | Closed, Reopened      | NO (Stopped)       |
| Closed      | Final end-state. Locked.                       | Reopened (rare case)  | NO (Stopped)       |

---

================================================================================

# 🎯 MODULE 24: PETTY CASH MANAGEMENT

## Overview

Petty Cash Management handles day-to-day operational expenses incurred by technicians and employees during service execution. In a pest control business, technicians often spend money on-site for purchasing materials, travel, or minor service-related costs. This module ensures such expenses are properly recorded, submitted for approval, verified with supporting documents, and reimbursed in a controlled and transparent manner.

**Module Connections:**

- **Depends on:** Module 8 (Employee — user details, bank/UPI info, roles), Module 7 (Branch — branch association)
- **Used by:** Finance / Accounting (reimbursement tracking), Reporting modules (expense analytics)

---

The module contains the following screens:

- 24.1 Tab 1: All Expenses (Admin / Finance Dashboard)
- 24.2 Tab 2: My Requests (Employee — submit & track own claims)
- 24.2.1 Add Petty Cash Request
- 24.2.2 View My Request (Read-only detail)
- 24.3 Tab 3: Received Requests (Manager / Finance — review & approve)
- 24.3.1 Request Review & Approval Form
- 24.3.2 Payment Processing Form

---

================================================================================

# 24.1 Tab 1: All Expenses

**Description:**
Master dashboard providing a company-wide view of all petty cash requests across branches. Accessible to **Admin, Finance Team, and Operations Head**. Provides summary cards for quick insight and a filterable datatable for detailed tracking.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        PETTY CASH MANAGEMENT                                 │
│                                                                              │
│  [Tab 1: All Expenses ●]  [Tab 2: My Requests]  [Tab 3: Received Requests] │
│                                                                              │
│                                                                              │
│  ┌─ FILTERS ──────────────────────────────────────────────────────────────┐  │
│  │ Branch   : [▼ All Branches ▼]     Status   : [▼ All ▼]                │  │
│  │ Category : [▼ All Categories ▼]   Employee : [🔍 Search ▼]            │  │
│  │ Date     : [📅 From] — [📅 To]    Amount   : [₹ Min] — [₹ Max]       │  │
│  │                                                        [Reset Filters] │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  Search: [🔍 Request ID / Employee Name / Description_______________]       │
│                                                                              │
│  ALL EXPENSES TABLE                                                          │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Request ID   │Employee    │Branch │Category      │Amount(₹)│Date Range         │  ││
│  │─────────────┼────────────┼───────┼──────────────┼─────────┼───────────────────│  ││
│  │PC-2026-0045 │Ravi S.     │Mumbai │Local Convey. │ 1,250   │23–23 Mar 2026     │  ││
│  │PC-2026-0044 │Anjali M.   │Pune   │Chemical      │ 3,800   │20–22 Mar 2026     │  ││
│  │PC-2026-0043 │Suresh K.   │Mumbai │Vendor Payment│ 5,500   │22–22 Mar 2026     │  ││
│  │PC-2026-0042 │Amit T.     │Delhi  │Stationery    │   850   │19–21 Mar 2026     │  ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Status        │Prior Appr. │Submitted Date     │Actions                  ││
│  │──────────────┼────────────┼───────────────────┼─────────────────────────││
│  │⏳ Pending     │No          │23 Mar 2026 10:30  │[View]                   ││
│  │✅ Approved    │Yes         │22 Mar 2026 14:15  │[View]                   ││
│  │💰 Paid        │No          │22 Mar 2026 09:00  │[View]                   ││
│  │❌ Rejected    │No          │21 Mar 2026 16:45  │[View]                   ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Shows 1 to 4 of 142 entries.                      [ < Previous | Next > ]  │
│                                                                              │
│  [📥 Export to Excel]                                                        │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Filter Fields

| Filter     | Type         | Options                                                                                                                                                                                                                                                                                                              |
| ---------- | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Branch     | Dropdown     | All Branches / Specific Branch (from Module 7)                                                                                                                                                                                                                                                                       |
| Status     | Dropdown     | All / Draft / Pending / Approved / Rejected / Returned / Paid                                                                                                                                                                                                                                                        |
| Category   | Dropdown     | All / Asset Purchase / Chemical / Fuel / Internet & Telephone / Local Conveyance / Office Expenses / Salary Advance / Staff Welfare / Stationery / Statutory & License / Travel Expenses / Vehicle Maintenance / Vendor Payment / Rent / Office Deposit / Promoter Incentive / Overtime / Transportation / Petrocard |
| Date Range | Date Picker  | Custom From – To date range                                                                                                                                                                                                                                                                                          |
| Amount     | Number Range | Min – Max amount filter                                                                                                                                                                                                                                                                                              |

---

## Search

| Field         | Type   | Description                                         |
| ------------- | ------ | --------------------------------------------------- |
| Request ID    | Search | Direct lookup by `PC-YYYY-NNNN`                     |
| Employee Name | Search | Free-text search against Employee Master (Module 8) |

---

## Table Columns

| Field          | Type     | Description                                             |
| -------------- | -------- | ------------------------------------------------------- |
| Request ID     | Link     | `PC-YYYY-NNNN`. Clicks to view request detail           |
| Employee Name  | Display  | Name of employee who submitted the claim                |
| Branch Name    | Display  | Employee's branch (from Module 7)                       |
| Category       | Badge    | From 19 category types (see Business Rules)             |
| Amount (₹)     | Currency | Total claimed amount                                    |
| Date Range     | Date     | Expense date range (From – To)                          |
| Status         | Badge    | Draft / Pending / Approved / Rejected / Returned / Paid |
| Prior Approval | Badge    | Yes / No — whether expense was pre-approved             |
| Submitted Date | DateTime | When the request was submitted                          |
| Actions        | Button   | [View] — Opens request detail                           |

---

## Table Actions

| Action   | Condition    | Description                         |
| -------- | ------------ | ----------------------------------- |
| **View** | All statuses | Opens read-only request detail view |

---

================================================================================

# 24.2 Tab 2: My Requests

**Description:**
Personal expense tracker for the logged-in employee. Shows all petty cash requests submitted by the current user. Users can create new requests, track approval status, and view details of past claims.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        PETTY CASH MANAGEMENT                                 │
│                                                                              │
│  [Tab 1: All Expenses]  [Tab 2: My Requests ●]  [Tab 3: Received Requests] │
│                                                                              │
│  Status Filter: [All] [Draft] [Pending] [Approved] [Rejected]               │
│                 [Returned] [Paid]                                            │
│                                                                              │
│  [+ Add Request]                                                             │
│                                                                              │
│  MY REQUESTS TABLE                                                           │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Request ID   │Category      │Amount(₹)│Date Range         │Status     │Actions  ││
│  │─────────────┼──────────────┼─────────┼───────────────────┼───────────┼─────────││
│  │PC-2026-0045 │Local Convey. │ 1,250   │23–23 Mar 2026     │⏳ Pending  │[View]   ││
│  │PC-2026-0038 │Chemical      │ 2,400   │20–22 Mar 2026     │✅ Approved│[View]   ││
│  │PC-2026-0031 │Office Exp.   │ 800     │18–18 Mar 2026     │💰 Paid    │[View]   ││
│  │PC-2026-0025 │Local Convey. │ 1,100   │15–16 Mar 2026     │❌ Rejected│[View]   ││
│  │PC-2026-0020 │Vendor Payment│ 4,500   │12–12 Mar 2026     │🔄 Returned│[View][Edit]││
│  │PC-2026-0019 │Office Exp.   │ 600     │11–11 Mar 2026     │📝 Draft   │[View][Edit][Revoke]││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Prior Approval│Submitted Date     │Sent To            │Reviewed By       ││
│  │──────────────┼───────────────────┼───────────────────┼──────────────────││
│  │No            │23 Mar 2026 10:30  │All Managers       │—                 ││
│  │Yes           │20 Mar 2026 09:00  │Priya D. (Manager) │Priya D.          ││
│  │No            │18 Mar 2026 11:15  │All                │Kamal R.          ││
│  │No            │15 Mar 2026 14:00  │Kamal R.           │Kamal R.          ││
│  │No            │12 Mar 2026 16:30  │All                │Priya D.          ││
│  │No            │—                  │—                  │—                 ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Shows 1 to 6 of 18 entries.                       [ < Previous | Next > ]  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table Fields

| Field          | Type     | Description                                             |
| -------------- | -------- | ------------------------------------------------------- |
| Request ID     | Link     | `PC-YYYY-NNNN`; clicks to View detail (24.2.2)          |
| Category       | Badge    | Expense category                                        |
| Amount (₹)     | Currency | Total claimed amount                                    |
| Date Range     | Date     | Expense date range (From – To)                          |
| Status         | Badge    | Draft / Pending / Approved / Rejected / Returned / Paid |
| Prior Approval | Badge    | Yes / No                                                |
| Submitted Date | DateTime | Timestamp of submission                                 |
| Sent To        | Display  | Recipient(s) of the request                             |
| Reviewed By    | Display  | Manager who reviewed the request                        |
| Actions        | Buttons  | View/Edit/Revoke                                        |

---

## Actions (Table Row)

| Action     | Available When            | Description                            |
| ---------- | ------------------------- | -------------------------------------- |
| **View**   | All statuses              | Opens request detail (24.2.2)          |
| **Edit**   | Status = Draft / Returned | Opens edit form to modify and resubmit |
| **Revoke** | Status = Pending          | Cancels a submitted request            |

---

## Filter Fields

| Filter     | Type         | Options                                                                                                                                                                                                                                                                                                              |
| ---------- | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Status     | Dropdown     | All / Draft / Pending / Approved / Rejected / Returned / Paid                                                                                                                                                                                                                                                        |
| Category   | Dropdown     | All / Asset Purchase / Chemical / Fuel / Internet & Telephone / Local Conveyance / Office Expenses / Salary Advance / Staff Welfare / Stationery / Statutory & License / Travel Expenses / Vehicle Maintenance / Vendor Payment / Rent / Office Deposit / Promoter Incentive / Overtime / Transportation / Petrocard |
| Date Range | Date Picker  | Custom From – To date range                                                                                                                                                                                                                                                                                          |
| Amount     | Number Range | Min – Max amount filter                                                                                                                                                                                                                                                                                              |

## Search

| Field         | Type   | Description                     |
| ------------- | ------ | ------------------------------- |
| Request ID    | Search | Direct lookup by `PC-YYYY-NNNN` |
| Global Search | Search | Search by Each field            |

================================================================================

# 24.2.1 Add Petty Cash Request

**Description:**
Form for employees to submit a new petty cash expense claim. Captures expense details, supporting documents, employee bank/UPI information for reimbursement, and optional prior approval reference. Upon submission, a recipient selection popup allows the user to choose specific approver(s).

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to My Requests]            ADD PETTY CASH REQUEST                   │
│                                                                              │
│  Request ID: PC-2026-XXXX (Auto-generated on Submit)                         │
│                                                                              │
│  ─── SECTION 1: EXPENSE DETAILS ─────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Category*            : [▼ Select Category ▼]                           │ │
│  │                         • Asset Purchase        • Chemical              │ │
│  │                         • Fuel                  • Internet & Telephone  │ │
│  │                         • Local Conveyance      • Office Expenses       │ │
│  │                         • Salary Advance        • Staff Welfare         │ │
│  │                         • Stationery            • Statutory & License   │ │
│  │                         • Travel Expenses       • Vehicle Maintenance   │ │
│  │                         • Vendor Payment        • Rent                  │ │
│  │                         • Office Deposit        • Promoter Incentive    │ │
│  │                         • Overtime              • Transportation        │ │
│  │                         • Petrocard                                     │ │
│  │                                                                         │ │
│  │  Expense Date (From)* : [📅 20 Mar 2026]                                │ │
│  │  Expense Date (To)*   : [📅 23 Mar 2026]                                │ │
│  │  Amount (₹)*          : [₹ 1,250_________]                             │ │
│  │                                                                         │ │
│  │  Description*         : [Purchased pest bait from local vendor during ] │ │
│  │                         [service at ABC Corp Head Office.              ] │ │
│  │                                                                         │ │
│  │  Related Task (Opt.)  : [🔍 Search Task ID ▼]  (From Module 21)        │ │
│  │  Related SO (Opt.)    : [🔍 Search SO No. ▼]   (From Module 20)        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── SECTION 2: SUPPORTING DOCUMENTS ────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Bill / Receipt*      : [📎 Upload File]   ✅ receipt_1.jpg             │ │
│  │                         (PDF, JPG, PNG — Max 5MB per file)             │ │
│  │                         [📎 Upload More]   (Up to 5 files)             │ │
│  │                                                                         │ │
│  │                                                                         │ │
│  │  Justification Note   : [________________________________]             │ │
│  │                         (Optional — additional context for approver)    │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── SECTION 3: BANK / PAYMENT DETAILS ──────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Payment Mode*        : (•) Bank Transfer   ( ) UPI                    │ │
│  │                                                                         │ │
│  │  ── If Bank Transfer ──                                                 │ │
│  │  Account Holder Name  : [Ravi Sharma__________] (Auto from Module 8)   │ │
│  │  Bank Name            : [State Bank of India__] (Auto from Module 8)   │ │
│  │  Account Number       : [XXXX XXXX 4521______] (Auto from Module 8)   │ │
│  │  IFSC Code            : [SBIN0001234__________] (Auto from Module 8)   │ │
│  │                                                                         │ │
│  │  ── If UPI ──                                                           │ │
│  │  UPI ID               : [ravi.s@upi___________] (Auto from Module 8)   │ │
│  │                                                                         │ │
│  │  ⚠ Bank/UPI details auto-filled from your employee profile.            │ │
│  │    You may edit if different payment method is preferred.               │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── SECTION 4: PRIOR APPROVAL ──────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Was this expense pre-approved?                                         │ │
│  │  [☑ Yes]  [☐ No]                                                       │ │
│  │                                                                         │ │
│  │  ── If Yes ──                                                           │ │
│  │  Approved By*         : [🔍 Search Manager / Supervisor ▼]             │ │
│  │  Approval Reference   : [Verbal approval on 22 Mar_______]             │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [SAVE DRAFT]          [SUBMIT REQUEST → opens 24.2.1.1]    [CANCEL]   │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Section 1: Expense Details Fields

| Field             | Type     | Required | Validation                                           | Description                           |
| ----------------- | -------- | -------- | ---------------------------------------------------- | ------------------------------------- |
| Category          | Dropdown | Yes      | Must select from 19 categories(mentioned in Preview) | Type of expense (see Business Rules)  |
| Expense Date From | Date     | Yes      | Cannot be future date (max = today)                  | Start date of the expense period      |
| Expense Date To   | Date     | Yes      | ≥ From date; cannot be future date                   | End date of the expense period        |
| Amount (₹)        | Currency | Yes      | Must be > 0; max ₹50,000                             | Total expense amount                  |
| Description       | Textarea | Yes      | Min 10 chars, Max 500 chars                          | What, when, how — expense explanation |
| Related Task      | Search   | No       | Must exist in Module 21 (if provided)                | Link to a specific service task       |
| Related SO        | Search   | No       | Must exist in Module 20 (if provided)                | Link to a specific Sales Order        |

---

## Section 2: Supporting Documents Fields

| Field              | Type        | Required | Validation                              | Description                                                          |
| ------------------ | ----------- | -------- | --------------------------------------- | -------------------------------------------------------------------- |
| Bill / Receipt     | File Upload | Yes      | Min 1 file; PDF, JPG, PNG; Max 5MB each | Proof of expense (up to 5 files)                                     |
| Upload More        | Button      | Yes      | Min 1 file; PDF, JPG, PNG; Max 5MB each | Can be add more file when 1 file is already uploaded (up to 5 files) |
| Justification Note | Textarea    | No       | Max 500 chars                           | Additional context for the approver                                  |

---

## Section 3: Bank / Payment Details Fields

| Field               | Type  | Required | Validation                   | Description                    |
| ------------------- | ----- | -------- | ---------------------------- | ------------------------------ |
| Payment Mode        | Radio | Yes      | Bank Transfer / UPI          | Preferred reimbursement method |
| Account Holder Name | Text  | Cond.    | Auto from Module 8; editable | Name on bank account           |
| Bank Name           | Text  | Cond.    | Auto from Module 8; editable | Bank name                      |
| Account Number      | Text  | Cond.    | Numeric; auto from Module 8  | Bank account number            |
| IFSC Code           | Text  | Cond.    | 11 chars; auto from Module 8 | Bank branch IFSC code          |
| UPI ID              | Text  | Cond.    | Valid UPI format (xx@upi)    | UPI address for direct payment |

> **Conditional:** Bank fields required if Payment Mode = Bank Transfer. UPI field required if Payment Mode = UPI.

---

## Section 4: Prior Approval Fields

| Field              | Type            | Required | Validation                               | Description                             |
| ------------------ | --------------- | -------- | ---------------------------------------- | --------------------------------------- |
| Pre-Approved?      | Checkbox        | Yes      | Default: No                              | Whether expense was approved beforehand |
| Approved By        | Search/dropdown | Cond.    | Must be manager/supervisor from Module 8 | Person who gave prior approval          |
| Approval Reference | Text            | No       | Max 200 chars                            | Verbal/written approval reference       |

---

## Form Actions

| Button             | Description                                                      |
| ------------------ | ---------------------------------------------------------------- |
| **Save Draft**     | Saves without validation, no notifications sent. Status = Draft  |
| **Submit Request** | Validates all fields, opens Recipient Selection popup (24.2.1.1) |
| **Cancel**         | Discards form and returns to My Requests list                    |

---

================================================================================

# 24.2.1.1 Select Recipients for Petty Cash Request (Popup)

**Description:**
Popup that appears after clicking **[Submit Request]** in 24.2.1. Allows the requester to select one or multiple recipients (manager / supervisor / finance) who will receive the request for approval. By default, **"All"** is selected.

---

## Popup Layout

```
┌─────────────────────────────────────────────────────────┐
│  POPUP: SELECT RECIPIENTS                                │
│  ┌─────────────────────────────────────────────────┐    │
│  │  Send to specific person(s):                     │    │
│  │  ☑ All (Default checked)                        │    │
│  │  ☐ Priya D. (Branch Manager — Mumbai)           │    │
│  │  ☐ Kamal R. (Operations Head)                   │    │
│  │  ☐ Neha S. (Finance Manager)                    │    │
│  │                                                  │    │
│  │  [CONFIRM SEND]  [CANCEL]                        │    │
│  └─────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────┘
```

---

## Fields

| Field      | Type      | Required | Description                                           |
| ---------- | --------- | -------- | ----------------------------------------------------- |
| All        | Checkbox  | Default  | Sends to all authorized approvers                     |
| Recipients | Multi-sel | Cond.    | Individual managers/supervisors (cascading from Role) |

## System Behavior

| Event        | Action                                                        |
| ------------ | ------------------------------------------------------------- |
| Confirm Send | Request status → **Pending**. Notification sent to recipients |
| Cancel       | Returns to form without submitting                            |

---

================================================================================

# 24.2.2 View My Request (Read-Only Detail)

**Description:**
Read-only detail screen showing the complete breakdown of a petty cash request. Includes expense info, supporting documents, bank details, prior approval info, approval status, payment status, and submission info.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to My Requests]                                                    │
│                                                                              │
│  REQUEST: PC-2026-0045                       Status: ⏳ PENDING APPROVAL     │
│                                                                              │
│  ─── EXPENSE DETAILS ───────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Category         : Local Conveyance                                    │ │
│  │  Expense Date     : 20 Mar 2026 — 23 Mar 2026                           │ │
│  │  Amount (₹)       : ₹ 1,250                                            │ │
│  │  Description      : Purchased pest bait from local vendor during        │ │
│  │                     service at ABC Corp Head Office.                     │ │
│  │  Related Task     : TASK-2026-0201                                      │ │
│  │  Related SO       : SO-2026-0112                                        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── SUPPORTING DOCUMENTS ──────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Bills / Receipts  : [📄 receipt_1.jpg] [📄 receipt_2.pdf]             │ │
│  │  Justification     : Urgent purchase required during site visit.        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── BANK / PAYMENT DETAILS ───────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Payment Mode      : Bank Transfer                                      │ │
│  │  Account Holder    : Ravi Sharma                                        │ │
│  │  Bank Name         : State Bank of India                                │ │
│  │  Account Number    : XXXX XXXX 4521                                     │ │
│  │  IFSC Code         : SBIN0001234                                        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── PRIOR APPROVAL ───────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Pre-Approved?     : No                                                 │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── APPROVAL & PAYMENT STATUS ─────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Approval Status   : ⏳ Pending                                        │ │
│  │  Reviewed By       : —                                                  │ │
│  │  Review Date       : —                                                  │ │
│  │  Approved Amount   : —                                                  │ │
│  │  Reviewer Remarks  : —                                                  │ │
│  │                                                                         │ │
│  │  Payment Status    : ⏳ Not Processed                                   │ │
│  │  Payment Mode      : —                                                  │ │
│  │  Transaction Ref   : —                                                  │ │
│  │  Payment Date      : —                                                  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── SUBMISSION INFO ───────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Submitted By      : Ravi S. (Senior Technician)                       │ │
│  │  Submitted Date    : 23 Mar 2026, 10:30 AM                             │ │
│  │  Branch            : Mumbai                                             │ │
│  │  Sent To           : All Managers                                       │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│                                                                              │
│                                        [BACK]                                │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## View-Only Fields

| Field            | Type     | Description                                                       |
| ---------------- | -------- | ----------------------------------------------------------------- |
| Request ID       | Display  | **[System-generated]** Unique request ID                          |
| Status           | Badge    | **[System-driven]** Current lifecycle status                      |
| Category         | Display  | **[Auto-fetched]** Expense category                               |
| Expense Date     | Date     | **[Auto-fetched]** Expense date range (From – To)                 |
| Amount (₹)       | Currency | **[Auto-fetched]** Claimed amount                                 |
| Description      | Text     | **[Auto-fetched]** Expense description                            |
| Related Task     | Link     | **[Auto-fetched]** Task reference (navigates to Module 21)        |
| Related SO       | Link     | **[Auto-fetched]** Sales Order reference (navigates to Module 20) |
| Bills / Receipts | File     | **[Auto-fetched]** Click to view/download uploaded documents      |
| Justification    | Text     | **[Auto-fetched]** Context for the expense                        |
| Payment Mode     | Display  | **[Auto-fetched]** Bank Transfer / UPI (from Section 3)           |
| Account Holder   | Display  | **[Auto-fetched]** Name on account                                |
| Bank Name        | Display  | **[Auto-fetched]** Employee's bank                                |
| Account Number   | Display  | **[Auto-fetched]** Bank account number                            |
| IFSC Code        | Display  | **[Auto-fetched]** Bank branch IFSC code                          |
| Pre-Approved?    | Badge    | **[Auto-fetched]** Yes / No (from Section 4)                      |
| Approval Status  | Badge    | **[System-driven]** Pending / Approved / Rejected / Returned      |
| Reviewed By      | Display  | **[Auto-fetched]** Name of the reviewing manager                  |
| Review Date      | Date     | **[Auto-fetched]** When the review was performed                  |
| Approved Amount  | Currency | **[Auto-fetched]** Amount approved (may differ from requested)    |
| Reviewer Remarks | Text     | **[Auto-fetched]** Notes from the approver                        |
| Payment Status   | Badge    | **[System-driven]** Not Processed / Processed                     |
| Payment Mode     | Display  | **[Auto-fetched]** Actual mode used for payment (from Finance)    |
| Transaction Ref  | Display  | **[Auto-fetched]** Payment transaction reference (after payment)  |
| Payment Date     | Date     | **[Auto-fetched]** When payment was made                          |
| Submitted By     | Display  | **[Auto-fetched]** Name and role of requester                     |
| Submitted Date   | DateTime | **[System-captured]** When the request was submitted              |
| Branch name      | Display  | **[Auto-fetched]** Requester's branch                             |
| Sent To          | Display  | **[Auto-fetched]** Recipients of the request                      |

---

================================================================================

# 24.3 Tab 3: Received Requests

**Description:**
For **Branch Managers, Operations Head, and Finance Team** to review incoming petty cash requests. Provides segmented views for different workflow stages and action buttons for approve, reject, return, and payment processing.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        PETTY CASH MANAGEMENT                                 │
│                                                                              │
│  [Tab 1: All Expenses]  [Tab 2: My Requests]  [Tab 3: Received Requests ●] │
│                                                                              │
│  Segmented Control: [Pending Approval] [Pending Payment] [Completed Today]  │
│                     [All History]                                            │
│                                                                              │
│  ┌─ FILTERS ──────────────────────────────────────────────────────────────┐  │
│  │ Branch   : [▼ All ▼]       Category: [▼ All ▼]     Date : [📅 Range]  │  │
│  │ Employee : [🔍 Search ▼]   Amount  : [₹ Min] — [₹ Max]               │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  Search: [🔍 Request ID / Employee Name________________________]            │
│                                                                              │
│  RECEIVED REQUESTS TABLE                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Request ID   │Employee    │Branch │Category       │Amount(₹)│Date Range        ││
│  │─────────────┼────────────┼───────┼───────────────┼─────────┼─────────────────││
│  │PC-2026-0045 │Ravi S.     │Mumbai │Local Convey.  │ 1,250   │23–23 Mar 2026   ││
│  │PC-2026-0044 │Anjali M.   │Pune   │Chemical       │ 3,800   │20–22 Mar 2026   ││
│  │PC-2026-0043 │Suresh K.   │Mumbai │Vendor Payment │ 5,500   │22–22 Mar 2026   ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Prior Appr.│Submitted Date     │Bills│Status           │Actions          ││
│  │───────────┼───────────────────┼─────┼─────────────────┼─────────────────││
│  │No         │23 Mar 2026 10:30  │ 2   │⏳ Pending Appr. │[View] [Review]  ││
│  │Yes        │22 Mar 2026 14:15  │ 1   │✅ Approved      │[View] [Pay]     ││
│  │No         │22 Mar 2026 09:00  │ 3   │💰 Paid          │[View]           ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Shows 1 to 3 of 23 entries.                       [ < Previous | Next > ]  │
│                                                                              │
│  [📥 Export to Excel]                                                        │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Segmented Control

| Segment          | Description                                   |
| ---------------- | --------------------------------------------- |
| Pending Approval | Requests awaiting manager review              |
| Pending Payment  | Approved requests awaiting finance processing |
| Completed Today  | Requests paid today                           |
| All History      | Full historical view of all received requests |

---

## Table Columns

| Field          | Type     | Description                                     |
| -------------- | -------- | ----------------------------------------------- |
| Request ID     | Link     | `PC-YYYY-NNNN`. Clicks to view request detail   |
| Employee Name  | Display  | Requesting employee's name                      |
| Branch         | Display  | Employee's branch                               |
| Category       | Badge    | Expense category                                |
| Amount (₹)     | Currency | Claimed amount                                  |
| Date Range     | Date     | Expense date range (From – To)                  |
| Prior Approval | Badge    | Yes / No                                        |
| Submitted Date | DateTime | When the request was submitted                  |
| Bills          | Number   | Count of attached bill/receipt files            |
| Status         | Badge    | Pending / Approved / Rejected / Returned / Paid |
| Actions        | Buttons  | Context-sensitive (View / Review / Pay)         |

---

## Actions (Table Row)

| Action     | Condition         | Role Required             | Description                            |
| ---------- | ----------------- | ------------------------- | -------------------------------------- |
| **View**   | All statuses      | All                       | Opens read-only detail (24.2.2)        |
| **Review** | Status = Pending  | Manager / Operations Head | Opens Approval Form (24.3.1)           |
| **Pay**    | Status = Approved | Finance Team              | Opens Payment Processing Form (24.3.2) |

---

## Filters

| Filter | Type | Options  
| Branch | Dropdown | All / Specific Branch |
| Category | Dropdown | All / Asset Purchase / Chemical / Fuel / Internet & Telephone / Local Conveyance / Office Expenses / Salary Advance / Staff Welfare / Stationery / Statutory & License / Travel Expenses / Vehicle Maintenance / Vendor Payment / Rent / Office Deposit / Promoter Incentive / Overtime / Transportation / Petrocard | |
| Date Range | Date Picker | Custom From – To |
| Amount | Number Range | Min – Max amount filter |

---

## Search

# | global search |

# 24.3.1 Request Review & Approval Form

**Description:**
Dedicated approval screen for managers and supervisors to review a petty cash request. Shows all submitted details in read-only mode and provides an approval decision panel. The approver can approve, reject, or return the request for correction, with an optional adjustment to the approved amount.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Received Requests]          REVIEW REQUEST: PC-2026-0045        │
│                                                                              │
│  ─── REQUEST DETAILS (Read-Only) ───────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Request ID       : PC-2026-0045                                        │ │
│  │  Employee         : Ravi S. (Senior Technician)                         │ │
│  │  Branch           : Mumbai                                              │ │
│  │  Submitted On     : 23 Mar 2026, 10:30 AM                              │ │
│  │                                                                         │ │
│  │  Category         : Travel Expenses                                     │ │
│  │  Expense Date     : 20 Mar 2026 — 23 Mar 2026                           │ │
│  │  Amount (₹)       : ₹ 1,250                                            │ │
│  │  Description      : Purchased pest bait from local vendor during        │ │
│  │                     service at ABC Corp Head Office.                     │ │
│  │                                                                         │ │
│  │  Related Task     : TASK-2026-0201                                      │ │
│  │  Related SO       : SO-2026-0112                                        │ │
│  │  Prior Approval   : No                                                  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── ATTACHED DOCUMENTS ────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Bills / Receipts  : [📄 receipt_1.jpg — View/Download]                │ │
│  │                      [📄 receipt_2.pdf — View/Download]                │ │
│  │  Justification     : Urgent purchase required during site visit.        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── BANK DETAILS ──────────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Payment Mode      : Bank Transfer                                      │ │
│  │  Account Holder    : Ravi Sharma       Account No : XXXX XXXX 4521     │ │
│  │  Bank              : SBI               IFSC       : SBIN0001234        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── APPROVAL DECISION ─────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Decision*         : (•) Approve   ( ) Reject   ( ) Return for Correct.│ │
│  │                                                                         │ │
│  │  ── If Approve ──                                                       │ │
│  │  Approved Amount*  : [₹ 1,250_________]                                │ │
│  │                     (Pre-filled with requested amount; editable)        │ │
│  │  Remarks           : [Verified against task assignment.__________]      │ │
│  │                                                                         │ │
│  │  ── If Reject ──                                                        │ │
│  │  Rejection Reason* : [▼ Select Reason ▼]                               │ │
│  │                       • Insufficient Documentation                      │ │
│  │                       • Exceeds Policy Limit                            │ │
│  │                       • Not Authorized                                  │ │
│  │                       • Duplicate Claim                                 │ │
│  │                       • Other                                           │ │
│  │  Remarks*          : [________________________________________]         │ │
│  │                                                                         │ │
│  │  ── If Return for Correction ──                                         │ │
│  │  Correction Notes* : [Please attach original bill, uploaded copy is  ]  │ │
│  │                      [not readable.                                  ]  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [CONFIRM DECISION]                                     [CANCEL]        │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Request Details (Read-Only)

| Field          | Type     | Description                                          |
| -------------- | -------- | ---------------------------------------------------- |
| Request ID     | Display  | **[Auto-fetched]** Unique request reference          |
| Employee       | Display  | **[Auto-fetched]** Name, role of requesting employee |
| Branch         | Display  | **[Auto-fetched]** Employee's branch                 |
| Submitted On   | DateTime | **[Auto-fetched]** When the request was submitted    |
| Category       | Display  | **[Auto-fetched]** Expense category                  |
| Expense Date   | Date     | **[Auto-fetched]** Expense date range (From – To)    |
| Amount (₹)     | Currency | **[Auto-fetched]** Claimed amount                    |
| Description    | Text     | **[Auto-fetched]** Expense description               |
| Related Task   | Link     | **[Auto-fetched]** Task reference (if linked)        |
| Related SO     | Link     | **[Auto-fetched]** SO reference (if linked)          |
| Prior Approval | Display  | **[Auto-fetched]** Yes/No + approver name if Yes     |
| Bills/Receipts | Files    | **[Auto-fetched]** Clickable document links          |

---

## Approval Decision Fields

| Field            | Type     | Required | Condition                    | Validation                         | Description                   |
| ---------------- | -------- | -------- | ---------------------------- | ---------------------------------- | ----------------------------- |
| Decision         | Radio    | Yes      | Always                       | Must select one option             | Approve / Reject / Return     |
| Approved Amount  | Currency | Cond.    | Decision = Approve           | Must be > 0; ≤ Requested Amount    | Approved reimbursement amount |
| Rejection Reason | Dropdown | Cond.    | Decision = Reject            | Must select from list              | Categorised rejection reason  |
| Remarks          | Textarea | Cond.    | Decision = Approve or Reject | Max 500 chars; required for Reject | Reviewer notes                |
| Correction Notes | Textarea | Cond.    | Decision = Return            | Min 10 chars; Max 500 chars        | Instructions for the employee |

---

## Validation Rules

| Validation                  | Rule                                     |
| --------------------------- | ---------------------------------------- |
| Decision required           | Must select Approve, Reject, or Return   |
| Approved Amount ≤ Requested | Cannot approve more than requested       |
| Rejection reason required   | Must provide reason when rejecting       |
| Correction notes required   | Must provide instructions when returning |
| At least one bill reviewed  | System warns if no documents were opened |

---

## Form Actions

| Button               | Description                                                       |
| -------------------- | ----------------------------------------------------------------- |
| **Confirm Decision** | Validates, saves decision, notifies employee, logs in audit trail |
| **Cancel**           | Returns to Received Requests without changes                      |

---

## System Behavior on Decision

| Decision                | System Action                                                          |
| ----------------------- | ---------------------------------------------------------------------- |
| **Approve**             | Status → Approved. Notification sent to employee + Finance Team        |
| **Reject**              | Status → Rejected. Notification sent to employee with reason           |
| **Return for Correct.** | Status → Returned. Notification sent to employee with correction notes |

---

================================================================================

# 24.3.2 Payment Processing Form

**Description:**
Finance team form to process payment for approved petty cash requests. Captures payment mode, transaction details, and marks the request as paid. Only accessible for requests with **Status = Approved**.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Received Requests]      PROCESS PAYMENT: PC-2026-0044           │
│                                                                              │
│  ─── APPROVED REQUEST SUMMARY (Read-Only) ──────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Request ID       : PC-2026-0044                                        │ │
│  │  Employee         : Anjali M. (Technician)                              │ │
│  │  Branch           : Pune                                                │ │
│  │  Category         : Material Purchases                                  │ │
│  │  Expense Date     : 20 Mar 2026 — 22 Mar 2026                           │ │
│  │  Requested Amount : ₹ 3,800                                            │ │
│  │  Approved Amount  : ₹ 3,500                                            │ │
│  │  Approved By      : Priya D. (Branch Manager)                          │ │
│  │  Approved On      : 22 Mar 2026, 03:00 PM                              │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── EMPLOYEE PAYMENT DETAILS (Read-Only) ──────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Payment Mode      : Bank Transfer                                      │ │
│  │  Account Holder    : Anjali M.          Account No : XXXX XXXX 7890    │ │
│  │  Bank              : HDFC Bank          IFSC       : HDFC0001234       │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── PAYMENT PROCESSING ────────────────────────────────────────────────── │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Payment Mode*        : [▼ Bank Transfer ▼]                             │ │
│  │                         • Bank Transfer (NEFT/IMPS/RTGS)               │ │
│  │                         • UPI                                           │ │
│  │                         • Cash                                          │ │
│  │                         • Cheque                                        │ │
│  │                                                                         │ │
│  │  Amount to Pay        : ₹ 3,500 (From Approved Amount — Read-Only)     │ │
│  │  Transaction Ref.*    : [NEFT-20260322-78456_______]                    │ │
│  │  Payment Date*        : [📅 22 Mar 2026]                                │ │
│  │                                                                         │ │
│  │  Upload Proof (Opt.)  : [📎 Upload Payment Screenshot]                 │ │
│  │  Finance Remarks      : [Payment processed via NEFT.__________]         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [MARK AS PAID]                                         [CANCEL]        │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Approved Request Summary (Read-Only)

| Field            | Type     | Description                                       |
| ---------------- | -------- | ------------------------------------------------- |
| Request ID       | Display  | **[Auto-fetched]** Unique request reference       |
| Employee         | Display  | **[Auto-fetched]** Employee name and role         |
| Branch           | Display  | **[Auto-fetched]** Employee's branch              |
| Category         | Display  | **[Auto-fetched]** Expense type                   |
| Expense Date     | Date     | **[Auto-fetched]** Expense date range (From – To) |
| Requested Amount | Currency | **[Auto-fetched]** Original claimed amount        |
| Approved Amount  | Currency | **[Auto-fetched]** Amount approved by manager     |
| Approved By      | Display  | **[Auto-fetched]** Name of the approving manager  |
| Approved On      | DateTime | **[Auto-fetched]** Date/time of approval          |

---

## Payment Processing Fields

| Field            | Type        | Required | Validation                          | Description                        |
| ---------------- | ----------- | -------- | ----------------------------------- | ---------------------------------- |
| Payment Mode     | Dropdown    | Yes      | Bank Transfer / UPI / Cash / Cheque | How the payment will be made       |
| Amount to Pay    | Currency    | Auto     | Read-only; from Approved Amount     | Disbursement amount                |
| Transaction Ref. | Text        | Yes      | Min 5 chars; unique                 | Bank/UPI transaction reference ID  |
| Payment Date     | Date        | Yes      | Cannot be future date (max = today) | Date payment was processed         |
| Upload Proof     | File Upload | No       | JPG, PNG, PDF; Max 5MB              | Payment confirmation screenshot    |
| Finance Remarks  | Textarea    | No       | Max 500 chars                       | Additional notes from finance team |

---

## Form Actions

| Button           | Description                                                            |
| ---------------- | ---------------------------------------------------------------------- |
| **Mark as Paid** | Validates, marks status → Paid, notifies employee, logs in audit trail |
| **Cancel**       | Returns to Received Requests without changes                           |

---

## System Behavior

| Event           | Action                                                                      |
| --------------- | --------------------------------------------------------------------------- |
| Mark as Paid    | Status → **Paid**. Employee notified. Payment details stored. Audit logged. |
| Transaction Ref | Stored permanently for finance reconciliation and audit                     |

---

================================================================================

# Access Control (RBAC)

| Role                  | Tab 1 (All Expenses) | Tab 2 (My Requests)  | Tab 3 (Received Requests) | Add Request | Approve/Reject | Process Payment |
| --------------------- | -------------------- | -------------------- | ------------------------- | ----------- | -------------- | --------------- |
| **Technician**        | ❌ No access         | ✅ Own requests only | ❌ No access              | ✅ Yes      | ❌ No          | ❌ No           |
| **Senior Technician** | ❌ No access         | ✅ Own requests only | ❌ No access              | ✅ Yes      | ❌ No          | ❌ No           |
| **Branch Manager**    | ✅ Branch only       | ✅ Own requests only | ✅ Branch requests        | ✅ Yes      | ✅ Yes         | ❌ No           |
| **Operations Head**   | ✅ All branches      | ✅ Own requests only | ✅ All branches           | ✅ Yes      | ✅ Yes         | ❌ No           |
| **Finance Team**      | ✅ All branches      | ✅ Own requests only | ✅ All branches           | ✅ Yes      | ❌ No          | ✅ Yes          |
| **Company Admin**     | ✅ All branches      | ✅ Own requests only | ✅ All branches           | ✅ Yes      | ✅ Yes         | ✅ Yes          |

---

================================================================================

# Business Rules

| Rule                          | Description                                                                |
| ----------------------------- | -------------------------------------------------------------------------- |
| Request ID Format             | `PC-YYYY-NNNN` (auto-generated, sequential per year)                       |
| Max Single Expense            | ₹50,000 per request (configurable by Admin)                                |
| Expense Date Validation       | Cannot be a future date; max age configurable (e.g., 30 days old)          |
| Mandatory Receipts            | At least one bill/receipt must be uploaded                                 |
| Bank Details Auto-fill        | Pre-filled from employee profile (Module 8); editable per request          |
| Prior Approval Flag           | If marked, approver name is required for verification                      |
| Approved Amount ≤ Requested   | Manager cannot approve more than the claimed amount                        |
| Partial Approval              | Manager can reduce the approved amount with remarks                        |
| Return for Correction         | Employee must re-edit and re-submit; retains original Request ID           |
| Draft Visibility              | Draft requests are visible ONLY to the creator                             |
| Duplicate Detection           | System warns if same employee, same amount, same date already exists       |
| Notification on Status Change | Employee notified on every status change (Approved/Rejected/Returned/Paid) |
| Audit Trail                   | All actions (create, submit, approve, reject, return, pay) are logged      |

---

================================================================================

# Request Status Lifecycle

| Status   | Description                          | Next Allowed Statuses        | Who Triggers      |
| -------- | ------------------------------------ | ---------------------------- | ----------------- |
| Draft    | Saved but not submitted              | Pending, Deleted             | Employee          |
| Pending  | Submitted, awaiting manager review   | Approved, Rejected, Returned | Employee (submit) |
| Approved | Manager approved, awaiting payment   | Paid                         | Manager           |
| Rejected | Manager denied the request           | — (Final state)              | Manager           |
| Returned | Sent back to employee for correction | Pending (after re-edit)      | Manager           |
| Paid     | Finance processed the payment        | — (Final state)              | Finance           |
| Revoked  | Employee cancelled before approval   | — (Final state)              | Employee          |

---

## Status Flow Diagram

```
                    ┌─────────┐
                    │  DRAFT  │
                    └────┬────┘
                         │ Submit
                         ▼
               ┌─────────────────┐
       ┌───── │    PENDING       │ ─────┐
       │      └────────┬────────┘      │
       │               │               │
 Reject│         Approve│         Return│
       │               │               │
       ▼               ▼               ▼
 ┌──────────┐   ┌──────────┐   ┌──────────┐
 │ REJECTED │   │ APPROVED │   │ RETURNED │
 │ (Final)  │   └────┬─────┘   └────┬─────┘
 └──────────┘        │              │
                     │ Pay          │ Re-edit & Re-submit
                     ▼              │
               ┌──────────┐        │
               │   PAID   │        ▼
               │  (Final) │   Back to PENDING
               └──────────┘

  Employee can REVOKE from PENDING → REVOKED (Final)
```

---

================================================================================
