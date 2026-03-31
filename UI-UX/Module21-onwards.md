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

### Ticket Header

| Field Name        | Description                                           |
| ----------------- | ----------------------------------------------------- |
| Back to Dashboard | Navigates user back to the main dashboard             |
| Ticket ID         | Unique identifier of the ticket                       |
| Print / PDF       | Generates printable or downloadable version of ticket |


### Ticket Summary

| Field Name          | Description                                        |
| ------------------- | -------------------------------------------------- |
| Subject             | Short title describing the issue                   |
| Customer            | Name of the customer associated with the ticket    |
| SO No               | Reference number of the related sales order        |
| Task ID             | Linked task created from the ticket                |
| Type                | Category of the ticket (Complaint, Service, Query) |
| Created Date & Time | Date and time when the ticket was created          |
| Caller Name & Phone | Contact person who raised the issue                |
| Description         | Detailed explanation of the issue                  |


### SLA & Status

| Field Name       | Description                                             |
| ---------------- | ------------------------------------------------------- |
| Status           | Current state of the ticket                             |
| Priority         | Urgency level of the ticket                             |
| Assigned To      | Support agent responsible for the ticket                |
| Response SLA     | Indicates if response time target was met               |
| Resolution SLA   | Indicates if resolution time target was met or breached |
| Escalation Level | Current escalation stage based on SLA                   |


### Actions

| Field Name        | Description                             |
| ----------------- | --------------------------------------- |
| Assign / Reassign | Assigns or changes the responsible user |
| Add Note / Reply  | Adds internal notes or replies          |
| Convert to Task   | Converts the ticket into a task         |
| Pause Ticket      | Temporarily pauses the ticket           |
| Mark Resolved     | Marks the issue as resolved             |
| Close Ticket      | Closes the ticket permanently           |


### Activity Timeline

| Field Name           | Description                                |
| -------------------- | ------------------------------------------ |
| Timestamp            | Time when the activity occurred            |
| Activity Icon        | Visual indicator of activity type          |
| Activity Description | Details of the activity performed          |
| User/System          | Indicates who performed the action         |
| Tags                 | Labels like internal note or system update |




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

| Section                     | Field Name                 | Description                                                                 |
|----------------------------|----------------------------|-----------------------------------------------------------------------------|
| Header                     | Back to My Requests        | Navigates user back to request list                                        |
| Header                     | Request ID                 | Unique identifier of the petty cash request                                |
| Header                     | Status                     | Current status of the request (Pending, Approved, Rejected, etc.)          |
| Expense Details            | Category                   | Type of expense (e.g., Local Conveyance, Materials, etc.)                  |
| Expense Details            | Expense Date               | Date or date range when the expense occurred                               |
| Expense Details            | Amount                     | Total expense amount requested                                             |
| Expense Details            | Description                | Detailed explanation of the expense                                        |
| Expense Details            | Related Task               | Linked task reference for which expense was incurred                       |
| Expense Details            | Related SO                 | Linked sales order reference                                               |
| Supporting Documents       | Bills / Receipts           | Uploaded proof documents for the expense                                   |
| Supporting Documents       | Justification              | Reason explaining why the expense was necessary                            |
| Bank / Payment Details     | Payment Mode (Requested)   | Preferred method for reimbursement (Bank Transfer, Cash, etc.)             |
| Bank / Payment Details     | Account Holder Name        | Name of the bank account holder                                            |
| Bank / Payment Details     | Bank Name                  | Name of the bank                                                           |
| Bank / Payment Details     | Account Number             | Bank account number (masked/unmasked)                                      |
| Bank / Payment Details     | IFSC Code                  | Bank branch identification code                                            |
| Prior Approval             | Pre-Approved               | Indicates whether the expense was approved in advance                      |
| Approval & Payment Status  | Approval Status            | Current approval stage of the request                                      |
| Approval & Payment Status  | Reviewed By                | Name of the reviewer/approver                                              |
| Approval & Payment Status  | Review Date                | Date when the request was reviewed                                         |
| Approval & Payment Status  | Approved Amount            | Final approved reimbursement amount                                        |
| Approval & Payment Status  | Reviewer Remarks           | Comments given by the approver                                             |
| Approval & Payment Status  | Payment Status             | Status of reimbursement processing                                         |
| Approval & Payment Status  | Payment Mode (Processed)   | Mode used for payment processing                                           |
| Approval & Payment Status  | Transaction Reference      | Reference number of the transaction                                        |
| Approval & Payment Status  | Payment Date               | Date when payment was made                                                 |
| Submission Info            | Submitted By               | Name and role of the person who submitted the request                      |
| Submission Info            | Submitted Date             | Date and time of submission                                                |
| Submission Info            | Branch                     | Branch/location from where request was raised                              |
| Submission Info            | Sent To                    | Users or roles to whom the request was sent for approval                   |
| Actions                    | Back                       | Navigates back to previous screen                                          |
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

# 🎯 MODULE 25: HRM (HUMAN RESOURCE MANAGEMENT)

## Overview

HRM module manages Employee Data, Salary, Attendance, and Leave Management within the ERP system. It acts as a central hub for HR operations — providing a unified view of employee records, enabling month-wise salary management, tracking attendance via calendar integration with Task Management, and handling leave applications with approval workflows.

**Module Connections:**

- **Depends on:** Module 6 (Role Salary & Leave Configuration — role-wise salary structure, incentive config, statutory deductions, leave policy — **PRIMARY CONFIG SOURCE**), Module 8 (Employee Details — employee-level overrides of Module 6 defaults), Module 21 (Task Management — attendance integration via task execution logs), Module 7 (Branch), Module 5 (Roles)
- **Used by:** Payroll processing, Attendance reports, Leave balance tracking, Management dashboards
- **Role-Based Access:** Admin / HR roles have full access; Employees can submit leave requests via application

**Dynamic Configuration Flow:**
```
Module 6 (Role Config)          Module 8 (Employee Creation)       Module 25 (HRM)
─────────────────────           ────────────────────────────       ──────────────────
Admin defines role-wise    →    Role selected → System auto-  →   Salary View auto-fetches
salary structure, incentive      fetches Module 6 config →         employee's salary config
config, statutory deductions,    Admin can OVERRIDE per             (from Mod 8, which inherited
leave policy (CL/SL/PL),        employee → Saved to Employee       from Mod 6). HR can further
carry forward, approval          record                             edit month-by-month.
authority, reset cycle
```

> **Key Principle:** Module 6 sets **role-level defaults**. Module 8 allows **employee-level overrides**. Module 25 uses the **final employee-level values** for month-wise salary processing, attendance tracking, and leave management.

---

The module contains the following screens:

- 25.1 Tab 1: Employee Management (Table View)
- 25.2 Salary View (Per Employee)
- 25.2.1 Salary Upload via Excel/CSV
- 25.3 Attendance View (Per Employee — Calendar)
- 25.3.1 Attendance Day Popup (View/Edit)
- 25.3.2 Attendance Upload via Excel/CSV
- 25.4 Leave Entry Form (Per Employee)
- 25.5 Tab 2: Leave Requests (All Requests Dashboard)
- 25.5.1 Leave Request Action Popup

---

================================================================================

# 25.1 Tab 1: Employee Management

**Description:**
Default landing screen displaying all employees in a table view. Reuses employee fields from Module 8 (Employee Details) with three additional action columns — Salary, Attendance, and Leave — that open detailed sub-screens for each employee.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     HRM - HUMAN RESOURCE MANAGEMENT                          │
│                                                                              │
│  [Tab 1: Employee Management ●]           [Tab 2: Leave Requests]           │
│                                                                              │
│  ┌─ FILTERS ──────────────────────────────────────────────────────────────┐  │
│  │ Branch      : [▼ All Branches ▼]       Department : [▼ Dropdown ▼]    │  │
│  │ Designation : [▼ Dropdown ▼]           Role       : [▼ Dropdown ▼]    │  │
│  │ Reporting Manager : [🔍 Searchable ▼]                                  │  │
│  │ Status      : [▼ All / Active / Inactive ▼]                            │  │
│  │ Created Date: [📅 From] — [📅 To]                                      │  │
│  │                                                        [Reset Filters] │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  Search: [🔍 Employee ID / Name / Email / Contact Number_______________]    │
│                                                                              │
│  EMPLOYEE TABLE                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Emp ID│Employee Name│Email ID│Contact No│Designation│Department│Role     ││
│  │──────┼─────────────┼────────┼──────────┼───────────┼──────────┼─────────││
│  │EMP01 │John Doe     │j@..    │9876....  │Manager    │Sales     │Admin    ││
│  │EMP02 │Jane Smith   │js@..   │9987....  │Technician │Operations│Staff    ││
│  │EMP03 │Rahul Patel  │rp@..   │9765....  │Executive  │HR        │User     ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Branch│Reporting Manager│Status│Created Date│Salary│Attendance│Leave    ││
│  │──────┼─────────────────┼──────┼────────────┼──────┼──────────┼─────────││
│  │HSR   │Suraj Sharma     │Active│24-Jan-2026 │[💰]  │[📅]      │[🏖️]    ││
│  │BTM   │Rohit Mehta      │Active│21-Jan-2026 │[💰]  │[📅]      │[🏖️]    ││
│  │Indira│Anil Kumar       │Inact.│20-Jan-2026 │[💰]  │[📅]      │[🏖️]    ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   10   Next                        │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Employee Table Fields (Reused from Module 8)

| Field             | Type    | Source   | Notes                      |
| ----------------- | ------- | -------- | -------------------------- |
| Emp ID            | Text    | Mod 8    | Unique employee identifier |
| Employee Name     | Text    | Mod 8    | Full name                  |
| Email ID          | Email   | Mod 8    | Employee email             |
| Contact Number    | Phone   | Mod 8    | Employee mobile            |
| Designation       | Text    | Mod 8    | Job title                  |
| Department        | Text    | Mod 8    | Employee department        |
| Role              | Text    | Mod 8    | System role assigned       |
| Branch            | Text    | Mod 8    | Assigned branch            |
| Reporting Manager | Text    | Mod 8    | Direct reporting manager   |
| Status            | Badge   | Mod 8    | Active / Inactive          |
| Created Date      | Date    | Mod 8    | Employee creation date     |

## HRM-Specific Action Columns

| Column     | Type   | Action                                      |
| ---------- | ------ | ------------------------------------------- |
| Salary     | Button | Opens Salary View (Screen 25.2)             |
| Attendance | Button | Opens Attendance Calendar View (Screen 25.3)|
| Leave      | Button | Opens Leave Entry Form (Screen 25.4)        |

---

## Filters (Same as Module 8)

| Filter             | Type         | Required | Description                                |
| ------------------ | ------------ | -------- | ------------------------------------------ |
| Branch             | Multi-select | No       | Filter employees by assigned branch        |
| Department         | Dropdown     | No       | Filter by department                       |
| Designation        | Dropdown     | No       | Filter by designation                      |
| Role               | Dropdown     | No       | Filter by system role                      |
| Reporting Manager  | Searchable   | No       | Filter by reporting manager                |
| Status             | Dropdown     | No       | Active / Inactive                          |
| Created Date Range | Date Range   | No       | Filter employees created within date range |

---

## Search

| Field         | Type | Description                                        |
| ------------- | ---- | -------------------------------------------------- |
| Global Search | Text | Search by Employee ID, Name, Email, Contact Number |

---

## System Behaviour

| Event         | System Response                         |
| ------------- | --------------------------------------- |
| Apply Filters | Table refreshes with filtered results   |
| Search        | Filters records based on search keyword |
| Click Salary  | Opens Salary View for that employee     |
| Click Attend. | Opens Attendance Calendar for employee  |
| Click Leave   | Opens Leave Entry Form for employee     |

---

================================================================================

# 25.2 Salary View (Per Employee)

**Description:**
Detailed salary view for a specific employee. Displays month-wise salary breakdown with editable components, payment status tracking, and salary slip download. Accessible via Salary button in the Employee Management table.

**Dynamic Data Source:** Salary components are auto-fetched from the employee record (Module 8 Step 3), which itself inherits defaults from the Role Configuration (Module 6 Step 2). HR can override any value month-by-month in this view. The Salary Type (CTC / Fixed / Hourly), Incentive Configuration, and Statutory Deduction applicability are all driven by the role config.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Employee List]        SALARY: EMP01 — John Doe                  │
│                                   Role: Sales Manager                        │
│                                                                              │
│  ┌─ MONTH FILTER ────────────────────────────────────────────────────────┐  │
│  │ Month : [▼ March ▼]    Year : [▼ 2026 ▼]         [Apply]            │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ─── SALARY INFO (Auto from Module 6 → Module 8) ───────────────────────  │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ Salary Type     : CTC  (from Role Config — Mod 6 Step 2)               ││
│  │ Salary Eff From : 01-Jan-2026     Salary Eff To : 31-Dec-2026          ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ─── SALARY BREAKDOWN (March 2026) ──────────────────────────────────────  │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ Component              │ Type        │ Amount (₹)                       ││
│  │────────────────────────┼─────────────┼──────────────────────────────────││
│  │ EARNINGS                                                                ││
│  │ Basic Salary           │ Fixed       │ [₹ 25,000____]                   ││
│  │ HRA                    │ Fixed       │ [₹ 10,000____]                   ││
│  │ Other Allowance        │ Fixed       │ [₹  5,000____]                   ││
│  │ Incentive              │ Variable    │ [₹  3,000____]                   ││
│  │ Deductions (General)   │ Fixed       │ [₹  2,000____]                   ││
│  │────────────────────────┼─────────────┼──────────────────────────────────││
│  │ STATUTORY DEDUCTIONS (shown only if applicable — from Mod 6 config)     ││
│  │ PF                     │ Statutory   │ [₹  3,000____]  ☑ Applicable    ││
│  │ ESI                    │ Statutory   │ [₹    750____]  ☑ Applicable    ││
│  │ TDS                    │ Statutory   │ [₹  2,000____]  ☑ Applicable    ││
│  │ Other Deductions       │ Variable    │ [₹      0____]                   ││
│  │────────────────────────┼─────────────┼──────────────────────────────────││
│  │ **GROSS SALARY**       │ Auto        │ ₹ 43,000                         ││
│  │ **TOTAL DEDUCTIONS**   │ Auto        │ ₹  7,750                         ││
│  │ **NET SALARY**         │ Auto        │ ₹ 35,250                         ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ─── INCENTIVE CONFIGURATION (Dynamic — from Module 6 Step 2) ───────────  │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ Holiday Work Incentive : ☑ Applicable                                   ││
│  │   • Type               : Fixed                                          ││
│  │   • Amount             : [₹ 500____]                                    ││
│  │                                                                          ││
│  │ Overtime               : ☑ Applicable                                   ││
│  │   • Type               : Per Hour                                       ││
│  │   • Shift Type         : Night Shift                                    ││
│  │   • Shift Incentive    : [₹ ____]                                       ││
│  │   • Per Hour Pay       : [₹ 150____]                                    ││
│  │   • Max OT Hours/Month : 40                                             ││
│  │                                                                          ││
│  │ OT Hours This Month    : [____] hrs     OT Amount: ₹ ____ (auto-calc)  ││
│  │ Holiday Days Worked    : [____] days    Holiday Amt: ₹ ____ (auto-calc) ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ─── STATUS ─────────────────────────────────────────────────────────────  │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ Payment Status  : [▼ Unpaid ▼]  (Paid / Unpaid / Due)                  ││
│  │ Reason          : [Late joining — prorated salary________________]      ││
│  │ Payment Date    : [📅 ________]  (Auto-filled when marked Paid)         ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  [💾 Save Changes]  [✅ Mark as Paid]  [📄 Download Salary Slip]            │
│  [📤 Upload Salary Data]                                                     │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Salary Info Fields (Read-only — from Module 6 → Module 8)

| Field             | Type     | Source   | Description                                        |
| ----------------- | -------- | -------- | -------------------------------------------------- |
| Salary Type       | Display  | Mod 6 S2 | CTC / Fixed / Hourly (from Role Config)            |
| Salary Eff. From  | Display  | Mod 6 S2 | Salary rule start date                             |
| Salary Eff. To    | Display  | Mod 6 S2 | Salary rule end date (optional)                    |

> These fields are **read-only** in Module 25. They are configured in Module 6 (Step 2) and inherited via Module 8.

---

## Salary Component Fields (Editable per Month)

| Field            | Type     | Required | Source          | Description                          |
| ---------------- | -------- | -------- | --------------- | ------------------------------------ |
| Basic Salary     | Currency | Yes      | Mod 6 S2→Mod 8  | Base salary (role default, editable) |
| HRA              | Currency | No       | Mod 6 S2→Mod 8  | House Rent Allowance                 |
| Other Allowance  | Currency | No       | Mod 6 S2→Mod 8  | Additional allowances                |
| Incentive        | Currency | No       | Mod 6 S2→Mod 8  | Performance incentive                |
| Deductions (Gen.)| Currency | No       | Mod 6 S2→Mod 8  | Standard deductions from role config |
| Other Deductions | Currency | No       | HRM (Mod 25)    | Additional month-specific deductions |
| Gross Salary     | Currency | Auto     | Calculated      | Sum of all earnings                  |
| Total Deductions | Currency | Auto     | Calculated      | Sum of all deductions + statutory    |
| Net Salary       | Currency | Auto     | Calculated      | Gross − Total Deductions             |

---

## Statutory Deductions (Conditional — from Module 6 Step 2)

> These fields are **shown only if the corresponding checkbox is marked as applicable** in Module 6 Step 2 role configuration. If not applicable for the employee's role, the row is hidden.

| Field          | Type     | Required    | Source   | Shown When              |
| -------------- | -------- | ----------- | -------- | ----------------------- |
| PF Deduction   | Currency | Conditional | Mod 6 S2 | PF Applicable = ☑ Yes   |
| ESI Deduction  | Currency | Conditional | Mod 6 S2 | ESI Applicable = ☑ Yes  |
| TDS Deduction  | Currency | Conditional | Mod 6 S2 | TDS Applicable = ☑ Yes  |

---

## Incentive Configuration (Dynamic — from Module 6 Step 2)

> Incentive fields are **shown only if the corresponding incentive type is marked as applicable** in Module 6 Step 2. HR can edit the monthly values (OT hours, holiday days worked).

| Field                             | Type     | Required    | Source          | Description                              |
| --------------------------------- | -------- | ----------- | --------------- | ---------------------------------------- |
| Holiday Work Incentive Applicable | Display  | Auto        | Mod 6 S2        | Yes/No (from role config)                |
| Holiday Work Incentive Type       | Display  | Auto        | Mod 6 S2        | Fixed / Per Day / Per Hour               |
| Holiday Work Incentive Amount     | Currency | Auto        | Mod 6 S2        | Default amount from role config          |
| Holiday Days Worked (this month)  | Number   | Cond.       | HRM (Mod 25)    | HR enters actual holiday days worked     |
| Holiday Incentive Amount          | Currency | Auto        | Calculated      | Days × Amount (auto-calculated)          |
| Overtime Applicable               | Display  | Auto        | Mod 6 S2        | Yes/No (from role config)                |
| Overtime Type                     | Display  | Auto        | Mod 6 S2        | Per Hour / Per Shift                     |
| Overtime Shift Type               | Display  | Auto        | Mod 6 S2        | Shift description                        |
| Overtime Shift Incentive          | Currency | Auto        | Mod 6 S2        | Shift incentive amount                   |
| Per Hour Pay                      | Currency | Auto        | Mod 6 S2        | Hourly overtime rate                     |
| Max OT Hours/Month                | Number   | Auto        | Mod 6 S2        | Monthly OT limit from role config        |
| OT Hours This Month               | Number   | Cond.       | HRM (Mod 25)    | HR enters actual OT hours worked         |
| OT Amount                         | Currency | Auto        | Calculated      | Hours × Per Hour Pay (auto-calculated)   |

> **Final Net Salary Calculation:**
> `Net Salary = (Basic + HRA + Allowance + Incentive + Holiday Incentive Amt + OT Amount) − (Deductions + PF + ESI + TDS + Other Deductions)`

> **Note:** Initial salary values are auto-fetched from the employee record (Module 8 Step 3), which itself inherits defaults from Role Configuration (Module 6 Step 2). HR can override any editable value month-by-month. Read-only config fields (Salary Type, Statutory applicability, Incentive applicability) can only be changed via Module 6.

---

## Status Fields

| Field          | Type     | Required | Options / Validation                    |
| -------------- | -------- | -------- | --------------------------------------- |
| Payment Status | Dropdown | Yes      | Paid / Unpaid / Due                     |
| Reason         | Textarea | Cond.    | Required when status = Unpaid or Due    |
| Payment Date   | Date     | Cond.    | Auto-filled on "Mark as Paid"; editable |

---

## Form Actions

| Action                 | Condition       | Description                                  |
| ---------------------- | --------------- | -------------------------------------------- |
| **Save Changes**       | Always          | Saves edited salary components and status    |
| **Mark as Paid**       | Status ≠ Paid   | Sets status to Paid, records payment date    |
| **Download Salary Slip** | Status = Paid | Downloads PDF salary slip for selected month |
| **Upload Salary Data** | Always          | Opens Upload Popup (Screen 25.2.1)           |

---

## Validation Rules

| Field          | Rule                                              |
| -------------- | ------------------------------------------------- |
| Basic Salary   | Required, numeric, minimum 0                      |
| All amounts    | Numeric, minimum 0                                |
| Net Salary     | Auto-calculated, cannot be negative               |
| Payment Status | Required                                          |
| Reason         | Required if status = Unpaid or Due, min 10 chars  |
| Salary Slip    | Download enabled only when status = Paid          |

---

================================================================================

# 25.2.1 Salary Upload via Excel/CSV

**Description:**
Popup modal for bulk uploading salary data via Excel/CSV file. Provides a sample template download for reference.

---

## Popup Layout

```
┌───────────────────────────────────────────────────────────────┐
│                  UPLOAD SALARY DATA                  [X Close] │
│                                                               │
│  Upload salary data for multiple employees at once.          │
│                                                               │
│  ┌───────────────────────────────────────────────────────┐   │
│  │                                                       │   │
│  │       📁 Drag & Drop file here                       │   │
│  │       or [Browse Files]                              │   │
│  │                                                       │   │
│  │       Supported: .xlsx, .xls, .csv                   │   │
│  │       Max size: 10MB                                 │   │
│  │                                                       │   │
│  └───────────────────────────────────────────────────────┘   │
│                                                               │
│  📥 Download Sample Excel Sheet                              │
│                                                               │
│  Selected File: salary_march_2026.xlsx  ✅                    │
│                                                               │
│                    [Cancel]    [Upload & Process]             │
└───────────────────────────────────────────────────────────────┘
```

---

## Upload Fields

| Field               | Type        | Required | Validation                        |
| ------------------- | ----------- | -------- | --------------------------------- |
| File Upload         | File Picker | Yes      | .xlsx / .xls / .csv, max 10MB    |
| Download Sample     | Link/Button | —        | Downloads pre-formatted template  |

> [!IMPORTANT]
> To ensure correct data processing and avoid system errors, always **Download the Sample Excel Sheet** and enter your data into that pre-formatted template before uploading.

---
## Sample Excel Columns

| Column             | Description                              |
| ------------------ | ---------------------------------------- |
| Emp ID             | Employee ID (must exist)                 |
| Month              | Salary month (e.g., Mar-2026)            |
| Basic Salary       | Basic pay amount                         |
| HRA                | House Rent Allowance                     |
| Other Allowance    | Additional allowances                    |
| Incentive          | Incentive amount                         |
| Deductions         | General deductions                       |
| PF                 | PF deduction (if applicable)             |
| ESI                | ESI deduction (if applicable)            |
| TDS                | TDS deduction (if applicable)            |
| Other Deductions   | Other deduction amount                   |
| OT Hours           | Overtime hours worked (if applicable)    |
| Holiday Days Worked| Holiday days worked (if applicable)      |
| Payment Status     | Paid / Unpaid / Due                      |
| Reason             | Reason (if Unpaid/Due)                   |

## System Behaviour

| Event              | Response                                          |
| ------------------ | ------------------------------------------------- |
| Upload & Process   | Validates file → shows preview → confirms import  |
| Invalid Emp ID     | Row flagged with error; skipped                   |
| Duplicate month    | Warning: existing data will be overwritten         |
| Success            | Toast: "X records imported successfully"          |

---

================================================================================

# 25.3 Attendance View (Per Employee — Calendar)

**Description:**
Calendar-based attendance view for a specific employee. Integrates data from Task Management (Module 21 — task execution logs) and Leave records. Each date cell shows attendance status with colour coding. Clicking a date opens a popup for viewing/editing punch-in/out details.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Employee List]      ATTENDANCE: EMP01 — John Doe               │
│                                                                              │
│  ┌─ MONTH FILTER ────────────────────────────────────────────────────────┐  │
│  │ Month : [▼ March ▼]    Year : [▼ 2026 ▼]         [Apply]            │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │  [◄ Prev Month]         MARCH 2026          [Next Month ►]           │  │
│  │                                                                       │  │
│  │  MON    TUE    WED    THU    FRI    SAT    SUN                        │  │
│  │  ┌──────┬──────┬──────┬──────┬──────┬──────┬──────┐                   │  │
│  │  │      │      │      │      │      │      │  1   │                   │  │
│  │  │      │      │      │      │      │      │ 🟡WO │                   │  │
│  │  ├──────┼──────┼──────┼──────┼──────┼──────┼──────┤                   │  │
│  │  │  2   │  3   │  4   │  5   │  6   │  7   │  8   │                   │  │
│  │  │ 🟢P  │ 🟢P  │ 🟢P  │ 🔴A  │ 🟢P  │ 🟢P  │ 🟡WO │                   │  │
│  │  ├──────┼──────┼──────┼──────┼──────┼──────┼──────┤                   │  │
│  │  │  9   │ 10   │ 11   │ 12   │ 13   │ 14   │ 15   │                   │  │
│  │  │ 🟢P  │ 🟢P  │ 🔵L  │ 🔵L  │ 🟢P  │ HD   │ 🟡WO │                   │  │
│  │  ├──────┼──────┼──────┼──────┼──────┼──────┼──────┤                   │  │
│  │  │ 16   │ 17   │ 18   │ 19   │ 20   │ 21   │ 22   │                   │  │
│  │  │ 🟢P  │ 🟢P  │ 🟢P  │ 🟢P  │ 🟢P  │ 🟢P  │ 🟡WO │                   │  │
│  │  ├──────┼──────┼──────┼──────┼──────┼──────┼──────┤                   │  │
│  │  │ 23●  │ 24   │ 25   │ 26   │ 27   │ 28   │ 29   │                   │  │
│  │  │ 🟢P  │      │      │      │      │      │      │                   │  │
│  │  └──────┴──────┴──────┴──────┴──────┴──────┴──────┘                   │  │
│  │                                                                       │  │
│  │  Legend: 🟢P Present  🔴A Absent  🔵L Leave  🟡WO Week Off  HD Holiday│  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  MONTHLY SUMMARY                                                             │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ Working Days: 22 │ Present: 18 │ Absent: 1 │ Leave: 2 │ Holiday: 1    ││
│  │ Week Off: 4      │                                                    ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  [+ Add Manual Entry]    [📤 Upload Attendance Data]                         │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Calendar Filters

| Filter | Type     | Required | Description                                |
| ------ | -------- | -------- | ------------------------------------------ |
| Month  | Dropdown | Yes      | Select month to view attendance calendar   |
| Year   | Dropdown | Yes      | Select year to view attendance calendar    |

---

## Calendar Cell Interaction

| Action              | Result                                                |
| ------------------- | ----------------------------------------------------- |
| **Click on a date** | Opens Attendance Day Popup (Screen 25.3.1)            |
| **Hover on date**   | Tooltip: Punch In/Out times, task count from Mod 21   |

---

## Attendance Status Types

| Status    | Code | Color  | Description                           |
| --------- | ---- | ------ | ------------------------------------- |
| Present   | P    | 🟢     | Employee was present                  |
| Absent    | A    | 🔴     | Employee was absent (no leave/task)   |
| Leave     | L    | 🔵     | Approved leave for this date          |
| Week Off  | WO   | 🟡     | Weekly off day (from Module 8 config) |
| Holiday   | HD   | Grey   | Company holiday                       |
| Half Day  | HD   | Orange | Half-day attendance                   |

---

## Data Sources for Attendance

| Source                  | Data Pulled                                    |
| ----------------------- | ---------------------------------------------- |
| Module 21 (Tasks)       | Task execution logs — Started At, Completed At |
| Module 25 (Leave)       | Approved leave records → mark as Leave         |
| Module 8 (Employee)     | Weekly off configuration                       |
| Manual Entry            | HR manually entered punch-in/out               |

---

## Monthly Summary Fields

| Field            | Type   | Description                                |
| ---------------- | ------ | ------------------------------------------ |
| Working Days     | Number | Business days in the month                 |
| Present          | Number | Days employee was present                  |
| Absent           | Number | Days absent (unmarked, no leave)           |
| Leave            | Number | Approved leave days                        |
| Holiday          | Number | Company holidays                           |
| Week Off         | Number | Weekly off count                           |

---

## Form Actions

| Action                   | Description                                   |
| ------------------------ | --------------------------------------------- |
| **+ Add Manual Entry**   | Opens Attendance Day Popup for manual entry    |
| **Upload Attendance**    | Opens Upload Popup (Screen 25.3.2)            |

---

================================================================================

# 25.3.1 Attendance Day Popup (View/Edit)

**Description:**
Popup triggered on clicking any date cell in the attendance calendar. Shows punch-in/out times and allows HR to edit or add attendance data manually.

---

## Popup Layout

```
┌───────────────────────────────────────────────────────────────┐
│            ATTENDANCE — 23 March 2026 (Monday)       [X Close]│
│            Employee: EMP01 — John Doe                         │
│                                                               │
│  Status: 🟢 Present                                          │
│                                                               │
│  ─── PUNCH DETAILS ───────────────────────────────────────   │
│  Punch In Time*  : [▼ 09:00 AM ▼]                           │
│  Punch Out Time* : [▼ 06:30 PM ▼]                           │
│  Total Hours     : 9h 30m (auto-calculated)                  │
│                                                               │
│  ─── TASK INTEGRATION (from Module 21) ───────────────────   │
│  Tasks Assigned  : 3                                         │
│  Tasks Completed : 2                                         │
│  Tasks Pending   : 1                                         │
│                                                               │
│  ─── NOTES ───────────────────────────────────────────────   │
│  Description/Notes : [Field work at client site__________]   │
│                                                               │
│                     [Cancel]    [Save]                        │
└───────────────────────────────────────────────────────────────┘
```

---

## Popup Fields

| Field          | Type     | Required | Validation                        |
| -------------- | -------- | -------- | --------------------------------- |
| Date           | Display  | Auto     | Selected date (read-only)         |
| Employee       | Display  | Auto     | Employee name & ID (read-only)    |
| Punch In Time  | Time     | Yes      | Must be before Punch Out          |
| Punch Out Time | Time     | Yes      | Must be after Punch In            |
| Total Hours    | Display  | Auto     | Punch Out − Punch In              |
| Tasks Assigned | Display  | Auto     | Count from Module 21              |
| Tasks Completed| Display  | Auto     | Count from Module 21              |
| Tasks Pending  | Display  | Auto     | Count from Module 21              |
| Description    | Textarea | No       | Max 500 characters                |
| status         | Badge    | Auto     | Present, Absent, Leave.           |

---

================================================================================

# 25.3.2 Attendance Upload via Excel/CSV

**Description:**
Popup modal for bulk uploading monthly attendance data. Same structure as Salary Upload (25.2.1).

---

## Popup Layout

```
┌───────────────────────────────────────────────────────────────┐
│              UPLOAD ATTENDANCE DATA                  [X Close] │
│                                                               │
│  Upload monthly attendance data for this employee.           │
│                                                               │
│  ┌───────────────────────────────────────────────────────┐   │
│  │       📁 Drag & Drop file here                       │   │
│  │       or [Browse Files]                              │   │
│  │                                                       │   │
│  │       Supported: .xlsx, .xls, .csv                   │   │
│  │       Max size: 10MB                                 │   │
│  └───────────────────────────────────────────────────────┘   │
│                                                               │
│  📥 Download Sample Excel Sheet                              │
│                                                               │
│                    [Cancel]    [Upload & Process]             │
└───────────────────────────────────────────────────────────────┘
```

---

## Popup Fields

| Field           | Type        | Required | Validation                              | Description                        |
| --------------- | ----------- | -------- | --------------------------------------- | ---------------------------------- |
| File Upload     | File Picker | Yes      | .xlsx / .xls / .csv; Max 10MB           | Drag & drop or browse to select    |
| Download Sample | Link        | —        | —                                       | Download the pre-formatted template|

> [!IMPORTANT]
> To ensure correct data processing and avoid system errors, always **Download the Sample Excel Sheet** and enter your data into that pre-formatted template before uploading.

---

## Sample Excel Columns(For Backend to create sample Excel)

| Column         | Description                             |
| -------------- | --------------------------------------- |
| Emp ID         | Employee ID (must exist in Module 8)    |
| Date           | Attendance date (DD-MM-YYYY)            |
| Punch In Time  | Punch in time (HH:MM AM/PM)            |
| Punch Out Time | Punch out time (HH:MM AM/PM)           |
| Status         | Present / Absent / Half Day / Week Off  |
| Notes          | Optional description or remarks         |

---

================================================================================

# 25.4 Leave Entry Form (Per Employee)

**Description:**
Form to apply for leave on behalf of an employee. Triggered from the Leave button in Tab 1 (Employee Management). Creates a leave record that flows into Tab 2 (Leave Requests) for approval.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Employee List]        LEAVE APPLICATION                          │
│                                    Employee: EMP01 — John Doe                │
│                                                                              │
│  ─── LEAVE BALANCE (from Module 6 Step 3 → Module 8 Step 4) ─────────────  │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ Casual Leave (CL) : 8 / 12 remaining                                   ││
│  │ Sick Leave (SL)   : 5 / 6 remaining                                    ││
│  │ Paid Leave (PL)   : 10 / 15 remaining                                  ││
│  │ Annual Allocation : 37 days                                             ││
│  │ Total Available   : 23 days                                             ││
│  │                                                                          ││
│  │ Carry Forward    : ☑ Allowed (Max 10 days)                              ││
│  │ Approval Auth.   : Director  (from Role Config)                         ││
│  │ Reset Cycle      : Yearly                                               ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ─── LEAVE DETAILS ──────────────────────────────────────────────────────  │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ Leave Type*     : [▼ Casual Leave / Sick Leave / Paid Leave ▼]         ││
│  │ From Date*      : [📅 ____________]                                     ││
│  │ To Date*        : [📅 ____________]                                     ││
│  │ Total Days      : 2 (auto-calculated, excludes week offs & holidays)    ││
│  │                                                                          ││
│  │ Description*    : [__________________________________________________] │  │
│  │                   [Textarea — reason for leave]                          │  │
│  │                                                                          │  │
│  │ Status*         : [▼ Approved / Pending / Rejected ▼]                  │  │
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ─── LEAVE HISTORY ──────────────────────────────────────────────────────  │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Leave ID  │Type │From       │To         │Days│Status   │Reason          ││
│  │──────────┼─────┼───────────┼───────────┼────┼─────────┼────────────────││
│  │LV-0045   │CL   │11-Mar-26  │12-Mar-26  │2   │✅ Appr. │Family function ││
│  │LV-0038   │SL   │05-Mar-26  │05-Mar-26  │1   │✅ Appr. │Fever           ││
│  │LV-0032   │PL   │20-Feb-26  │22-Feb-26  │3   │❌ Rej.  │Travel — denied ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│       [Submit Leave]                                        [Cancel]         │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Leave Form Fields

| Field         | Type     | Required | Validation                                        |
| ------------- | -------- | -------- | ------------------------------------------------- |
| Employee ID   | Display  | Auto     | Pre-filled (Read-only)                            |
| Employee Name | Display  | Auto     | Pre-filled (Read-only)                            |
| Leave Type    | Dropdown | Yes      | Casual Leave / Sick Leave / Paid Leave             |
| From Date   | Date     | Yes      | Cannot be past date                               |
| To Date     | Date     | Yes      | Must be ≥ From Date                               |
| Total Days  | Display  | Auto     | Calculated: To − From + 1 (excl. week offs/holidays) |
| Description | Textarea | Yes      | Reason for leave, min 10 characters               |
| Status      | Dropdown | Yes      | Select status (Approved / Pending / Rejected)      |

## Leave Balance Fields (Read-only — from Module 6 Step 3 → Module 8 Step 4)

> Leave entitlements are configured role-wise in Module 6 (Step 3) and inherited into the employee record via Module 8 (Step 4). Module 25 displays the **current remaining balance** after deducting approved leaves.

| Field                      | Type    | Source          | Description                                |
| -------------------------- | ------- | --------------- | ------------------------------------------ |
| Casual Leave (CL)         | Display | Mod 6 S3→Mod 8  | Remaining / Total CL days per year         |
| Sick Leave (SL)           | Display | Mod 6 S3→Mod 8  | Remaining / Total SL days per year         |
| Paid Leave (PL)           | Display | Mod 6 S3→Mod 8  | Remaining / Total PL days per year         |
| Annual Leave Allocation    | Display | Mod 6 S3→Mod 8  | Total annual leave allocation              |
| Total Available            | Display | Calculated      | Sum of all remaining leaves                |
| Carry Forward Allowed      | Display | Mod 6 S3        | Yes / No (from role config)                |
| Max Carry Forward Days     | Display | Mod 6 S3        | Max days that can be carried (if allowed)  |
| Leave Approval Authority   | Display | Mod 6 S3        | Role responsible for approving leaves      |
| Leave Reset Cycle          | Display | Mod 6 S3        | Yearly / Monthly / Custom (From–To)        |

## Leave History Fields (Read-only)

| Field    | Type    | Description                                      |
| -------- | ------- | ------------------------------------------------ |
| Leave ID | Display | Unique Leave Request ID                          |
| Type     | Badge   | Leave type (CL / SL / PL)                        |
| From     | Display | Start date of leave                              |
| To       | Display | End date of leave                                |
| Days     | Display | Total working days on leave                      |
| Status   | Badge   | Current status (Approved / Rejected / Pending)    |
| Reason   | Display | Description / Reason provided by user            |

## Validation Rules

| Rule                         | Description                                       |
| ---------------------------- | ------------------------------------------------- |
| Leave balance check          | Cannot apply if requested days > available balance |
| Duplicate dates              | Cannot overlap with existing approved leaves       |
| From ≤ To                    | From Date must be on or before To Date            |
| Description required         | Minimum 10 characters                             |
| Weekend/Holiday exclusion    | Auto-exclude from total day count                 |

---

## Form Actions

| Action           | Description                                           |
| ---------------- | ----------------------------------------------------- |
| **Submit Leave** | Creates leave record with selected status. **Note:** If status = Pending, it flows to Tab 2 for approval. If Approved/Rejected, it records directly in history. |
| **Cancel**       | Discards and returns to Employee List                 |

---

================================================================================

# 25.5 Tab 2: Leave Requests

**Description:**
Dashboard for HR/Admin to view and manage leave requests **submitted by employees via the Mobile App / Self-Service portal**. Manual entries added by HR in Screen 25.4 bypass this queue if marked as Approved/Rejected.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     HRM - HUMAN RESOURCE MANAGEMENT                          │
│                                                                              │
│  [Tab 1: Employee Management]           [Tab 2: Leave Requests ●]           │
│                                                                              │
│  ┌─ FILTERS ──────────────────────────────────────────────────────────────┐  │
│  │ Branch      : [▼ All Branches ▼]       Department : [▼ Dropdown ▼]    │  │
│  │ Status      : [▼ All / Pending / Approved / Rejected ▼]               │  │
│  │ Leave Type  : [▼ All / CL / SL / PL ▼]                                │  │
│  │ Date Range  : [📅 From] — [📅 To]                                      │  │
│  │                                                        [Reset Filters] │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  Search: [🔍 Employee Name / Leave ID / Department_______________]          │
│                                                                              │
│  LEAVE REQUESTS TABLE                                                        │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Leave ID │Emp Name    │Department│Leave Type│From       │To         │Days ││
│  │─────────┼────────────┼──────────┼──────────┼───────────┼───────────┼─────││
│  │LV-0050  │John Doe    │Sales     │CL        │25-Mar-26  │26-Mar-26  │2    ││
│  │LV-0049  │Jane Smith  │Operations│SL        │24-Mar-26  │24-Mar-26  │1    ││
│  │LV-0048  │Rahul Patel │HR        │PL        │27-Mar-26  │31-Mar-26  │3    ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Description            │Status     │Submitted Date    │Actions           ││
│  │───────────────────────┼───────────┼──────────────────┼──────────────────││
│  │Family function        │⏳ Pending  │23-Mar-26 10:30   │[View][✅][❌][⏳] ││
│  │Fever and cold         │⏳ Pending  │23-Mar-26 09:15   │[View][✅][❌][⏳] ││
│  │Annual vacation trip   │✅ Approved │22-Mar-26 14:00   │[View]            ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   10   Next                        │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table Fields

| Field          | Type     | Description                                   |
| -------------- | -------- | --------------------------------------------- |
| Leave ID       | Link     | Unique leave request ID (e.g., LV-0050)       |
| Employee Name  | Display  | Name of employee who submitted leave          |
| Department     | Display  | Employee's department                         |
| Leave Type     | Badge    | CL / SL / PL                                  |
| From Date      | Date     | Leave start date                              |
| To Date        | Date     | Leave end date                                |
| Days           | Number   | Total leave days (excl. week offs & holidays)  |
| Description    | Display  | Reason for leave                              |
| Status         | Badge    | Pending / Approved / Rejected                 |
| Submitted Date | DateTime | When the request was submitted                |
| Actions        | Buttons  | View / Approve / Reject / Mark Pending        |

---

## Actions (Table Row)

| Action           | Icon | Condition         | Description                              |
| ---------------- | ---- | ----------------- | ---------------------------------------- |
| **View**         | 👁   | All statuses      | Opens Leave Request Popup (Screen 25.5.1)|
| **Approve**      | ✅   | Status = Pending  | Approves leave request                   |
| **Reject**       | ❌   | Status = Pending  | Opens popup with mandatory reason        |
| **Mark Pending** | ⏳   | Status = Approved/Rejected | Reverts status to Pending       |

---

## Filters

| Filter      | Type     | Options                               |
| ----------- | -------- | ------------------------------------- |
| Branch      | Dropdown | All Branches / Specific Branch        |
| Department  | Dropdown | All / Specific Department             |
| Status      | Dropdown | All / Pending / Approved / Rejected   |
| Leave Type  | Dropdown | All / CL / SL / PL                   |
| Date Range  | Date     | Custom From – To range                |

## Search

| Field         | Type | Description                                        |
| ------------- | ---- | -------------------------------------------------- |
| Global Search | Text | Search by Employee Name, Leave ID, Department      |

---

================================================================================

# 25.5.1 Leave View Action Popup

**Description:**
Popup opened on View/Action click from the Leave Requests table. Shows full leave details and allows HR/Admin to update status and provide reason.

---

## Popup Layout

```
┌───────────────────────────────────────────────────────────────┐
│               LEAVE REQUEST DETAILS                  [X Close] │
│               Leave ID: LV-0050                               │
│                                                               │
│  ─── EMPLOYEE INFORMATION ────────────────────────────────   │
│  Employee Name : John Doe                                    │
│  Employee ID   : EMP01                                       │
│  Department    : Sales                                       │
│  Branch        : HSR                                         │
│                                                               │
│  ─── LEAVE DETAILS ──────────────────────────────────────    │
│  Leave Type    : Casual Leave                                │
│  From Date     : 25-Mar-2026                                 │
│  To Date       : 26-Mar-2026                                 │
│  Total Days    : 2                                           │
│  Description   : Family function — need 2 days off           │
│                                                               │
│  ─── LEAVE BALANCE (from Module 6 → Module 8) ──────────────────────────    │
│  CL Remaining  : 8 / 12                                     │
│  SL Remaining  : 5 / 6                                      │
│  PL Remaining  : 10 / 15                                    │
│  Annual Alloc. : 37 days                                    │
│  Carry Forward : Allowed (Max 10)                           │
│  Approval Auth.: Director                                   │
│                                                               │
│  ─── ACTION ─────────────────────────────────────────────    │
│  Status*       : [▼ Pending / Approved / Rejected ▼]        │
│  Reason*       : [__________________________________________]│
│                  (Mandatory if Status = Rejected)            │
│  Reviewed By   : Auto: Current Logged-in User               │
│  Review Date   : Auto: Current Date & Time                   │
│                                                               │
│           [Cancel]    [Mark Pending]   [Reject]   [Approve]  │
└───────────────────────────────────────────────────────────────┘
```

---

## Popup Fields

| Field        | Type     | Editable | Validation                             |
| ------------ | -------- | -------- | -------------------------------------- |
| Employee Name| Display  | No       | From employee record                   |
| Employee ID  | Display  | No       | From employee record                   |
| Department   | Display  | No       | From employee record                   |
| Branch       | Display  | No       | From employee record                   |
| Leave Type   | Display  | No       | CL / SL / PL                          |
| From Date    | Display  | No       | Leave start date                       |
| To Date      | Display  | No       | Leave end date                         |
| Total Days   | Display  | No       | Auto-calculated                        |
| Description  | Display  | No       | Employee's reason                      |
| Casual Leave (CL) Bal| Display  | No       | Current CL balance (Mod 6 S3 → Mod 8 S4) |
| Sick Leave (SL) Bal  | Display  | No       | Current SL balance (Mod 6 S3 → Mod 8 S4) |
| Paid Leave (PL) Bal  | Display  | No       | Current PL balance (Mod 6 S3 → Mod 8 S4) |
| Annual Alloc.   | Display  | No       | Total annual allocation (Mod 6 S3)       |
| Carry Forward   | Display  | No       | Allowed/Not (Mod 6 S3)                   |
| Approval Auth.  | Display  | No       | Role who approves (Mod 6 S3)             |
| Status          | Dropdown | Yes      | Pending / Approved / Rejected            |
| Reason          | Textarea | Cond.    | **Mandatory** when Status = Rejected, min 10 chars |
| Reviewed By     | Display  | Auto     | Logged-in user (auto-captured)           |
| Review Date     | Display  | Auto     | Current date/time (auto-recorded)        |

---

## Popup Actions

| Action           | Behaviour                                                      |
| ---------------- | -------------------------------------------------------------- |
| **Approve**      | Status → Approved; deducts from leave balance; updates calendar |
| **Reject**       | Status → Rejected; reason required; no balance change          |
| **Mark Pending** | Status → Pending; resets review info                           |
| **Cancel**       | Closes popup without changes                                   |

---

================================================================================

# 🔐 Role-Based Access Control (RBAC)

| Feature                    | Super Admin | Admin/HR | Manager | Employee (App User) |
| -------------------------- | ----------- | -------- | ------- | ------------------- |
| View Employee Table        | ✅          | ✅       | ✅ (own branch) | ❌           |
| Edit Salary                | ✅          | ✅       | ❌       | ❌                  |
| Upload Salary Data         | ✅          | ✅       | ❌       | ❌                  |
| Download Salary Slip       | ✅          | ✅       | ❌       | ✅ (own only)       |
| View/Edit Attendance       | ✅          | ✅       | ✅ (own branch) | ❌           |
| Upload Attendance Data     | ✅          | ✅       | ❌       | ❌                  |
| Apply Leave (for others)   | ✅          | ✅       | ❌       | ❌                  |
| Submit Leave Request (own) | ✅          | ✅       | ✅       | ✅                  |
| Approve/Reject Leave       | ✅          | ✅       | ✅ (own team) | ❌             |
| View Leave Requests Tab    | ✅          | ✅       | ✅ (own team) | ❌             |

---

================================================================================

# 📋 User Flow Descriptions

## Flow 1: View Employee Salary
```
HR logs in → Module 25 → Tab 1 (Employee Management)
  → Click [💰 Salary] on employee row
  → Salary View opens → Select Month/Year
  → View/Edit salary components
  → Mark as Paid / Download Salary Slip
```

## Flow 2: Record Attendance
```
HR logs in → Module 25 → Tab 1 → Click [📅 Attendance]
  → Calendar opens → Click on a date
  → Popup opens → Enter Punch In / Punch Out / Notes
  → Save → Calendar updates with status
```

## Flow 3: Apply Leave (HR on behalf of Employee)
```
HR logs in → Module 25 → Tab 1 → Click [🏖️ Leave]
  → Leave Form opens → Select Leave Type, From/To, Description, and **Status**
  → Submit → If status = Approved, leave balance deducted & attendance updated immediately.
```

## Flow 4: Employee Submits Leave (via App)
```
Employee (App User) logs in → Leave Section
  → Fill From Date, To Date, Description
  → Submit → Request appears in Module 25, Tab 2
```

## Flow 5: Approve/Reject Leave Request
```
HR/Admin → Module 25 → Tab 2 (Leave Requests)
  → View pending requests → Click [View] or action buttons
  → Popup opens → Review details
  → Approve: Leave balance deducted, Attendance calendar updated
  → Reject: Reason mandatory, no balance change
```

## Flow 6: Bulk Upload Salary Data
```
HR → Module 25 → Tab 1 → Click [💰 Salary] on any employee
  → Click [📤 Upload Salary Data]
  → Popup opens → Download sample sheet → Fill data
  → Upload file → System validates & imports
```

---
====================================================================================


# 🎯 MODULE 26: TECHNICIAN PERFORMANCE & PRODUCTIVITY

## Overview

The Technician Performance & Productivity module provides **Operations Managers, Branch Managers**, and **Company Admins** with a centralized analytics dashboard to evaluate field technician performance. This module is entirely **read-only** — it **does not create new data**. Instead, it aggregates and calculates KPIs from existing operational modules to produce actionable performance insights.

**Module Connections:**

- **Depends on:** Module 21 (Task Management — tasks assigned, completed, overdue, re-tasks), Module 20 (Sales Orders — revenue data), Module 25 (HRM — attendance, working hours), Module 11 (Stock Management — material usage), Module 8 (Employee Master — technician details & roles), Module 7 (Branch — branch assignment)
- **Used by:** Management Reporting, HR Reviews, Incentive / Bonus Calculations
- **Data Nature:** 100% computed from existing modules. No manual data entry screens.

---

The module contains the following screens:

- 26.1 Performance Dashboard — Employee Table View (Default)
- 26.2 Individual Employee Performance View (Detailed)

---

================================================================================

# 26.1 Performance Dashboard — Employee Table View

**Description:**
The default landing screen for Module 26. Displays a **datatable of all technicians** with computed performance and productivity KPIs. Managers can filter by branch, role, and date range to compare technicians side-by-side. Includes summary KPI cards at the top for a quick organizational overview.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                   TECHNICIAN PERFORMANCE & PRODUCTIVITY                      │
│                                                                              │
│  ┌─ FILTERS ──────────────────────────────────────────────────────────────┐  │
│  │ Branch    : [▼ All Branches ▼]     Role  : [▼ All Roles ▼]            │  │
│  │ Period    : [▼ This Month ▼]       Date  : [📅 01 Mar] – [📅 31 Mar]  │  │
│  │ Search    : [🔍 Employee Name / ID___________]     [Reset Filters]    │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ ORG-LEVEL SUMMARY CARDS ──────────────────────────────────────────────┐  │
│  │ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐   │  │
│  │ │ 📊 Total     │ │ ✅ Avg       │ │ ⏱ Avg        │ │ ⭐ Avg       │   │  │
│  │ │ Technicians  │ │ Completion   │ │ Utilization  │ │ Customer     │   │  │
│  │ │              │ │ Rate         │ │ Rate         │ │ Rating       │   │  │
│  │ │    24        │ │   87.5%      │ │   72.3%      │ │   4.2 / 5    │   │  │
│  │ └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘   │  │
│  │ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐   │  │
│  │ │ 📋 Total     │ │ 💰 Total     │ │ 🔄 Avg       │ │ 🏆 Top       │   │  │
│  │ │ Tasks        │ │ Revenue      │ │ Re-Task      │ │ Performer    │   │  │
│  │ │ Completed    │ │ Generated    │ │ Rate         │ │              │   │  │
│  │ │    312       │ │  ₹14,85,000  │ │   4.2%       │ │  Ravi S.     │   │  │
│  │ └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘   │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ EMPLOYEE PERFORMANCE TABLE ───────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │ Rank│ Employee     │ Branch  │ Role        │ Tasks    │ Tasks     │    │  │
│  │     │              │         │             │ Assigned │ Completed │    │  │
│  │─────┼──────────────┼─────────┼─────────────┼──────────┼───────────┼    │  │
│  │  1  │ Ravi S.      │ Mumbai  │ Sr. Tech    │    18    │    17     │    │  │
│  │  2  │ Anjali M.    │ Mumbai  │ Technician  │    16    │    15     │    │  │
│  │  3  │ Suresh K.    │ Pune    │ Technician  │    14    │    12     │    │  │
│  │  4  │ Amit T.      │ Delhi   │ Technician  │    15    │    12     │    │  │
│  │  5  │ Priya D.     │ Mumbai  │ Sr. Tech    │    12    │    10     │    │  │
│  │                                                                        │  │
│  │ Completion│ Utiliz. │ Tasks/ │ Revenue     │ Avg     │ Re-Task │      │  │
│  │ Rate      │ Rate    │ Day    │ Contributed │ Rating  │ Rate    │Score │  │
│  │───────────┼─────────┼────────┼─────────────┼─────────┼─────────┼──────│  │
│  │  94.4%    │  78.5%  │  3.4   │ ₹2,85,000   │ 4.6⭐   │  2.1%   │ 92  │  │
│  │  93.8%    │  75.2%  │  3.0   │ ₹2,40,000   │ 4.4⭐   │  3.5%   │ 88  │  │
│  │  85.7%    │  70.1%  │  2.4   │ ₹1,80,000   │ 4.2⭐   │  5.0%   │ 78  │  │
│  │  80.0%    │  68.3%  │  2.4   │ ₹1,95,000   │ 3.8⭐   │  6.2%   │ 72  │  │
│  │  83.3%    │  65.0%  │  2.0   │ ₹1,50,000   │ 4.0⭐   │  4.8%   │ 75  │  │
│  │                                                                        │  │
│  │ Actions: [👁 View] per row                                              │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│    Shows 1 to 5 of 24 technicians.                [ < Previous | Next > ]    │
│                                                                              │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Filters

| Filter   | Type                | Options                                                                     |
| -------- | ------------------- | --------------------------------------------------------------------------- |
| Branch   | Dropdown            | All Branches / Specific Branch (from Module 7)                             |
| Role     | Dropdown            | All Roles / Technician / Senior Technician (from Module 8)                 |
| Period   | Dropdown            | This Week / This Month / This Quarter / Custom Range                       |
| Date     | Date Range Picker   | From Date – To Date (enabled when Period = Custom Range)                   |
| Search   | Text (Autocomplete) | Search by Employee Name or Employee ID                                     |

---

## Org-Level Summary Cards (Read-Only)

> These cards display **aggregated KPIs across all filtered technicians** for the selected date range. All values are **auto-calculated**.

| Card                  | Type    | Description                                                                         | Calculation Source                         |
| --------------------- | ------- | ----------------------------------------------------------------------------------- | ------------------------------------------ |
| Total Technicians     | Number  | Count of active technicians matching the current filter criteria                     | Module 8 (Active employees with Tech role) |
| Avg Completion Rate   | Percent | Average of all individual technician completion rates                                | Module 21 (Task statuses)                  |
| Avg Utilization Rate  | Percent | Average of all individual technician utilization rates                                | Module 21 + Module 25 (Attendance)         |
| Avg Customer Rating   | Rating  | Average customer rating across all technicians' completed tasks                      | Module 21.6 (Customer Feedback)            |
| Total Tasks Completed | Number  | Sum of all completed tasks across all filtered technicians                           | Module 21 (Status = Completed)             |
| Total Revenue         | Currency| Sum of revenue generated from all completed tasks                                   | Module 20 (SO line item values)            |
| Avg Re-Task Rate      | Percent | Average re-task percentage across all technicians                                    | Module 21 (Type = Re-Task)                 |
| Top Performer         | Name    | Technician with the highest Performance Score in the selected period                 | Calculated (see Backend Logic)             |

---

## Employee Performance Table — Columns

| Column              | Type           | Sortable | Description                                                                              | Data Source                      |
| ------------------- | -------------- | -------- | ---------------------------------------------------------------------------------------- | -------------------------------- |
| Rank                | Number         | No       | Auto-calculated rank based on Performance Score (highest first)                           | Calculated                       |
| Employee Name       | Display (Link) | Yes      | Technician's full name. Click → opens Individual View (Screen 26.2)                      | Module 8                         |
| Employee ID         | Display        | Yes      | Unique employee ID (e.g., EMP-00124)                                                     | Module 8                         |
| Branch              | Display        | Yes      | Assigned branch name                                                                     | Module 7 → Module 8             |
| Role                | Badge          | Yes      | Technician / Senior Technician                                                           | Module 8                         |
| Tasks Assigned      | Number         | Yes      | Total tasks assigned to this technician in the selected period                            | Module 21                        |
| Tasks Completed     | Number         | Yes      | Total tasks with Status = Completed                                                      | Module 21                        |
| Tasks Pending       | Number         | Yes      | Total tasks with Status = Pending or In Progress                                         | Module 21                        |
| Tasks Overdue       | Number         | Yes      | Tasks past scheduled date with Status ≠ Completed                                       | Module 21                        |
| Completion Rate (%) | Percent        | Yes      | (Tasks Completed ÷ Tasks Assigned) × 100                                                 | Calculated                       |
| Utilization Rate (%)| Percent        | Yes      | (Total Task Hours ÷ Total Working Hours) × 100                                           | Module 21 + Module 25            |
| Tasks / Day         | Number         | Yes      | Tasks Completed ÷ Total Working Days                                                     | Calculated                       |
| Revenue Contributed | Currency       | Yes      | Total revenue from tasks where this technician was Primary or Support                    | Module 20 → Module 21           |
| Avg Customer Rating | Rating (⭐)    | Yes      | Average of all customer ratings for this technician's completed tasks                    | Module 21.6                      |
| Re-Task Rate (%)    | Percent        | Yes      | (Re-Tasks Count ÷ Tasks Completed) × 100                                                | Module 21                        |
| Material Efficiency | Percent        | Yes      | (Total Std Qty ÷ Total Actually Used Qty) × 100 — capped at 100%                        | Module 21.6 + Module 11          |
| Performance Score   | Score (0-100)  | Yes      | Weighted composite score (see Backend Calculation Logic)                                 | Calculated                       |
| Actions             | Button         | No       | [👁 View] — Opens Individual Employee Performance View (Screen 26.2)                    | —                                |

---

## Table Row Actions

| Action   | Icon | Condition | Description                                           |
| -------- | ---- | --------- | ----------------------------------------------------- |
| **View** | 👁   | Always    | Opens Individual Performance View (Screen 26.2)       |

---



================================================================================

# 26.2 Individual Employee Performance View

**Description:**
A comprehensive, read-only performance profile of a single technician. Shows personal details, KPI scorecards, task breakdown, revenue details, material usage analysis, customer feedback history, and re-task records. All data is scoped to the selected date range filter.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Dashboard]              EMPLOYEE PERFORMANCE PROFILE            │
│                                                                              │
│  Filters: Period: [▼ This Month ▼]   Date: [📅 01 Mar] – [📅 31 Mar 2026]  │
│                                                                              │
│  ┌─ EMPLOYEE INFO ────────────────────────────────────────────────────────┐  │
│  │  ┌─────┐                                                               │  │
│  │  │ 👤  │  Ravi S.                  EMP-00124                          │  │
│  │  │Photo│  Senior Technician        Mumbai Branch                      │  │
│  │  │     │  📱 9876543210            📧 ravi.s@company.com              │  │
│  │  └─────┘  Joining Date: 15 Jun 2024   Status: 🟢 Active              │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ KPI SCORECARDS ───────────────────────────────────────────────────────┐  │
│  │ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐   │  │
│  │ │ 🏆 Overall   │ │ ✅ Completion│ │ ⏱ Utilization│ │ 📊 Tasks/Day │   │  │
│  │ │ Score        │ │ Rate         │ │ Rate         │ │              │   │  │
│  │ │              │ │              │ │              │ │              │   │  │
│  │ │   92 / 100   │ │   94.4%      │ │   78.5%      │ │    3.4       │   │  │
│  │ │  🟢 Excellent │ │  🟢 Above Avg│ │  🟢 Good     │ │  🟢 High     │   │  │
│  │ └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘   │  │
│  │ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐   │  │
│  │ │ ⭐ Avg       │ │ 💰 Revenue   │ │ 🧪 Material  │ │ 🔄 Re-Task  │   │  │
│  │ │ Customer     │ │ Contribution │ │ Efficiency   │ │ Rate         │   │  │
│  │ │ Rating       │ │              │ │              │ │              │   │  │
│  │ │  4.6 / 5.0   │ │  ₹2,85,000   │ │   96.2%      │ │   2.1%       │   │  │
│  │ │  🟢 Excellent │ │  🟢 Top 10%  │ │  🟢 Optimal  │ │  🟢 Low      │   │  │
│  │ └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘   │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ TASK PERFORMANCE BREAKDOWN ───────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Total Assigned : 18          Completed    : 17                        │  │
│  │  Pending        : 1           Overdue      : 0                         │  │
│  │  Normal Tasks   : 15          Re-Tasks     : 2                         │  │
│  │  On-Time Rate   : 94.1%      (16 of 17 completed within schedule)     │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ TIME & ATTENDANCE ANALYSIS ───────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Working Days     : 22         Present Days  : 20                      │  │
│  │  Leave Days       : 1          Absent Days   : 0                       │  │
│  │  Week Offs        : 8          Late Arrivals : 1                       │  │
│  │  Attendance Rate  : 95.5%     (20 of 21 working days excl. leave)     │  │
│  │                                                                        │  │
│  │  Total Working Hrs: 168h       Task Execution Hrs: 131.9h             │  │
│  │  Avg Hrs / Day    : 8h 24m     Avg Task Time     : 1h 45m             │  │
│  │  Travel Time (est): 36.1h      Idle / Gap Time   : 0h                 │  │
│  │  Utilization Rate : 78.5%     (Task Hrs ÷ Working Hrs × 100)          │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ REVENUE CONTRIBUTION ─────────────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Total Revenue Generated : ₹2,85,000                                   │  │
│  │  As Primary Technician   : ₹2,10,000  (12 tasks)                      │  │
│  │  As Support Technician   : ₹75,000    (5 tasks, shared proportionally) │  │
│  │  Avg Revenue / Task      : ₹16,765                                     │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ MATERIAL USAGE EFFICIENCY ────────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │ Chemical / Product    │ Std Qty  │ Actually Used │ Variance │ Eff. %   │  │
│  │───────────────────────┼──────────┼───────────────┼──────────┼──────────│  │
│  │ Alpha Cypermethrin     │ 2,040 ml │ 1,870 ml      │ -170 ml  │ 109.1%  │  │
│  │ Fipronil Gel           │ 34 tubes │ 34 tubes      │ 0        │ 100.0%  │  │
│  │ Bromadiolone Cake      │ 48 nos   │ 52 nos        │ +4 nos   │ 92.3%   │  │
│  │ Glue Trap Board        │ 80 nos   │ 78 nos        │ -2 nos   │ 102.6%  │  │
│  │─────────────────────────────────────────────────────────────────────────│  │
│  │ Overall Material Efficiency: 96.2%                                     │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ CUSTOMER SATISFACTION ────────────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Average Rating       : ⭐ 4.6 / 5.0                                   │  │
│  │  Total Feedback Count : 17                                              │  │
│  │                                                                        │  │
│  │  Rating Breakdown:                                                      │  │
│  │  5-Star: 12  |  4-Star: 3  |  3-Star: 2  |  2-Star: 0  |  1-Star: 0   │  │
│  │                                                                        │  │
│  │  Recent Feedback:                                                       │  │
│  │  ┌──────────────────────────────────────────────────────────────┐      │  │
│  │  │ TASK-2026-0201 │ ABC Corp │ 23 Mar │ ⭐⭐⭐⭐⭐ │ "Excellent" │      │  │
│  │  │ TASK-2026-0198 │ PQR Foods│ 22 Mar │ ⭐⭐⭐⭐   │ "Good work" │      │  │
│  │  │ TASK-2026-0195 │ XYZ Hotel│ 21 Mar │ ⭐⭐⭐⭐⭐ │ "Thorough"  │      │  │
│  │  └──────────────────────────────────────────────────────────────┘      │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ RE-TASK / REWORK TRACKING ────────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Re-Tasks Assigned  : 2            Re-Task Rate : 2.1%                 │  │
│  │  (Re-Tasks as Primary ÷ Total Completed as Primary × 100)             │  │
│  │                                                                        │  │
│  │  Re-Task Details:                                                       │  │
│  │  ┌──────────────────────────────────────────────────────────────┐      │  │
│  │  │ Task ID         │ Original Task │ Customer │ Date    │Status │      │  │
│  │  │─────────────────┼───────────────┼──────────┼─────────┼───────│      │  │
│  │  │ TASK-2026-0205  │ TASK-2026-0180│ ABC Corp │ 25 Mar  │ Done  │      │  │
│  │  │ TASK-2026-0215  │ TASK-2026-0199│ LMN Pvt  │ 28 Mar  │Pendng │      │  │
│  │  └──────────────────────────────────────────────────────────────┘      │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ PERFORMANCE SCORE BREAKDOWN ──────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Factor                 │ Weight │ Raw Value │ Weighted Score           │  │
│  │─────────────────────────┼────────┼───────────┼──────────────────────────│  │
│  │ Task Completion Rate    │  30%   │  94.4%    │  28.3 / 30              │  │
│  │ Time Utilization Rate   │  15%   │  78.5%    │  11.8 / 15              │  │
│  │ Customer Satisfaction   │  25%   │  4.6 / 5  │  23.0 / 25              │  │
│  │ Revenue Contribution    │  10%   │  Top 10%  │   9.5 / 10              │  │
│  │ Material Efficiency     │  10%   │  96.2%    │   9.6 / 10              │  │
│  │ Re-Task Penalty         │  10%   │  2.1%     │   9.8 / 10              │  │
│  │─────────────────────────┼────────┼───────────┼──────────────────────────│  │
│  │ TOTAL PERFORMANCE SCORE │ 100%   │           │  92.0 / 100  🟢         │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  Score Rating: 🟢 Excellent (90-100) | 🔵 Good (75-89) |                    │
│               🟡 Average (60-74) | 🔴 Below Average (<60)                   │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Employee Info Section (Read-Only)

| Field          | Type    | Description                                               | Source           |
| -------------- | ------- | --------------------------------------------------------- | ---------------- |
| Profile Photo  | Image   | Employee photograph                                       | Module 8         |
| Employee Name  | Display | Full name of the technician                               | Module 8         |
| Employee ID    | Display | Unique ID (e.g., EMP-00124)                               | Module 8         |
| Role           | Badge   | Technician / Senior Technician                            | Module 8         |
| Branch         | Display | Assigned branch name                                      | Module 7 → 8    |
| Phone          | Display | Registered mobile number                                  | Module 8         |
| Email          | Display | Registered email address                                  | Module 8         |
| Joining Date   | Date    | Date of employment start                                  | Module 8         |
| Status         | Badge   | 🟢 Active / 🔴 Inactive                                   | Module 8         |

---

## KPI Scorecards (Read-Only)

| Card                   | Type         | Description                                                       | Calculation                                          |
| ---------------------- | ------------ | ----------------------------------------------------------------- | ---------------------------------------------------- |
| Overall Score          | Score / 100  | Weighted composite performance score                               | See Backend Calculation Logic                        |
| Score Grade            | Badge        | 🟢 Excellent / 🔵 Good / 🟡 Average / 🔴 Below Average            | Based on score range thresholds                      |
| Completion Rate        | Percent      | Percentage of assigned tasks that were completed                   | (Completed ÷ Assigned) × 100                         |
| Utilization Rate       | Percent      | Percentage of working hours spent on actual task execution          | (Task Hours ÷ Working Hours) × 100                   |
| Tasks / Day            | Number       | Average tasks completed per working day                            | Completed ÷ Working Days Present                     |
| Avg Customer Rating    | Rating (⭐)  | Mean of all customer ratings from completed tasks                  | Sum(ratings) ÷ Count(rated tasks)                    |
| Revenue Contribution   | Currency     | Total revenue from tasks where this technician participated        | Sum of SO line item values (split logic applied)     |
| Material Efficiency    | Percent      | How closely actual material usage matches the standard requirement | (Std Qty ÷ Actually Used Qty) × 100 — capped at 100 |
| Re-Task Rate           | Percent      | Percentage of tasks that required a re-service                     | (Re-Tasks as Primary ÷ Completed as Primary) × 100  |

---

## Task Performance Breakdown (Read-Only)

| Field                | Type    | Description                                                   | Source              |
| -------------------- | ------- | ------------------------------------------------------------- | ------------------- |
| Total Assigned       | Number  | All tasks assigned to this technician in the selected period  | Module 21           |
| Completed            | Number  | Tasks with Status = Completed                                 | Module 21           |
| Pending              | Number  | Tasks with Status = Pending or In Progress                    | Module 21           |
| Overdue              | Number  | Past-date tasks with Status ≠ Completed                       | Module 21           |
| Normal Tasks         | Number  | Tasks with Type = Normal (from SO)                            | Module 21           |
| Re-Tasks             | Number  | Tasks with Type = Re-Task (from tickets)                      | Module 21           |
| On-Time Rate         | Percent | Completed tasks that were finished within their scheduled slot | Module 21           |

---

## Time & Attendance Analysis (Read-Only)

| Field                | Type    | Description                                                                     | Source                      |
| -------------------- | ------- | ------------------------------------------------------------------------------- | --------------------------- |
| Working Days         | Number  | Total working days in the period (excluding week offs and holidays)              | Module 25 + Module 6        |
| Present Days         | Number  | Days with a Punch-In record                                                     | Module 25 (Attendance)      |
| Leave Days           | Number  | Approved leave days in the period                                                | Module 25 (Leave)           |
| Absent Days          | Number  | Working days with no Punch-In and no approved leave                              | Module 25 (Attendance)      |
| Week Offs            | Number  | Weekends / configured week-off days                                              | Module 6 (Config)           |
| Late Arrivals        | Number  | Days where Punch-In was after designated shift start time                        | Module 25 (Attendance)      |
| Attendance Rate      | Percent | (Present Days ÷ Effective Working Days) × 100 — excl. approved leave            | Calculated                  |
| Total Working Hours  | Hours   | Sum of (Punch Out − Punch In) across all present days                           | Module 25 (Attendance)      |
| Task Execution Hours | Hours   | Sum of (Actual End Time − Actual Start Time) for all completed tasks            | Module 21.6                 |
| Avg Hours / Day      | Hours   | Total Working Hours ÷ Present Days                                              | Calculated                  |
| Avg Task Time        | Hours   | Task Execution Hours ÷ Tasks Completed                                          | Calculated                  |
| Travel Time (est.)   | Hours   | Total Working Hours − Task Execution Hours (approximate travel + transit time)  | Calculated                  |
| Utilization Rate     | Percent | (Task Execution Hours ÷ Total Working Hours) × 100                              | Calculated                  |


---

## Revenue Contribution (Read-Only)

| Field                     | Type     | Description                                                                                                | Source                    |
| ------------------------- | -------- | ---------------------------------------------------------------------------------------------------------- | ------------------------- |
| Total Revenue Generated   | Currency | Sum of SO line item values for all completed tasks involving this technician                                | Module 20 → Module 21    |
| As Primary Technician     | Currency | Revenue from tasks where this technician was the Primary (100% attribution)                                 | Module 21 (Primary role)  |
| Primary Task Count        | Number   | Count of completed tasks as Primary                                                                         | Module 21                 |
| As Support Technician     | Currency | Revenue from tasks where this technician was a Support (proportional share)                                 | Module 21 (Support role)  |
| Support Task Count        | Number   | Count of completed tasks as Support                                                                          | Module 21                 |
| Avg Revenue / Task        | Currency | Total Revenue ÷ Total Tasks Completed                                                                       | Calculated                |


---

## Material Usage Efficiency (Read-Only)

| Column              | Type    | Description                                                                  | Source                   |
| ------------------- | ------- | ---------------------------------------------------------------------------- | ------------------------ |
| Chemical / Product  | Display | Name of chemical/product used across all tasks                                | Module 12 → Module 21.6 |
| Std Qty (Total)     | Number  | Sum of standard/required quantities across all tasks for this chemical        | Module 21 (Task Plan)    |
| Actually Used (Total)| Number | Sum of actually used quantities logged by the technician                      | Module 21.6              |
| Variance            | Number  | Std Qty − Actually Used. Positive = saved, Negative = overused               | Calculated               |
| Efficiency %        | Percent | (Std Qty ÷ Actually Used) × 100. Capped at 100% for underuse                | Calculated               |
| Overall Efficiency  | Percent | Weighted average efficiency across all chemicals (weighted by Std Qty value) | Calculated               |

---

## Customer Satisfaction (Read-Only)

| Field                | Type    | Description                                                        | Source              |
| -------------------- | ------- | ------------------------------------------------------------------ | ------------------- |
| Average Rating       | Rating  | Mean rating across all rated completed tasks                       | Module 21.6         |
| Total Feedback Count | Number  | Count of tasks with customer ratings                                | Module 21.6         |

| 5-Star Count         | Number  | Tasks rated 5 stars                                                 | Module 21.6         |
| 4-Star Count         | Number  | Tasks rated 4 stars                                                 | Module 21.6         |
| 3-Star Count         | Number  | Tasks rated 3 stars                                                 | Module 21.6         |
| 2-Star Count         | Number  | Tasks rated 2 stars                                                 | Module 21.6         |
| 1-Star Count         | Number  | Tasks rated 1 star                                                  | Module 21.6         |
| Recent Feedback List | Table   | Last 5 feedback entries showing Task ID, Customer, Date, Rating, Comment | Module 21.6   |

### Recent Feedback Table Columns

| Column           | Type    | Description                                  |
| ---------------- | ------- | -------------------------------------------- |
| Task ID          | Link    | Task identifier — click opens Task Detail    |
| Customer         | Display | Customer business name                       |
| Date             | Date    | Service completion date                      |
| Rating           | Stars   | Customer rating (1-5)                        |
| Feedback Comment | Display | Written feedback (truncated to 50 chars)     |

---

## Re-Task / Rework Tracking (Read-Only)

| Field              | Type    | Description                                                                 | Source              |
| ------------------ | ------- | --------------------------------------------------------------------------- | ------------------- |
| Re-Tasks Assigned  | Number  | Count of Re-Task type tasks assigned to this technician (as Primary)         | Module 21           |
| Re-Task Rate       | Percent | (Re-Tasks as Primary ÷ Total Completed as Primary) × 100                    | Calculated          |
| Re-Task Details    | Table   | List of all re-tasks with linked original task details                       | Module 21           |

### Re-Task Detail Table Columns

| Column         | Type    | Description                                      |
| -------------- | ------- | ------------------------------------------------ |
| Task ID        | Link    | Re-Task identifier — click opens Task Detail     |
| Original Task  | Link    | Original task that triggered the re-service      |
| Customer       | Display | Customer name                                    |
| Date           | Date    | Scheduled date of re-task                        |
| Status         | Badge   | Completed / Pending                              |

---

## Performance Score Breakdown (Read-Only)

| Field                  | Type       | Description                                                             |
| ---------------------- | ---------- | ----------------------------------------------------------------------- |
| Factor Name            | Display    | Name of the KPI factor (e.g., Task Completion Rate)                     |
| Weight                 | Percent    | Assigned weight in the performance formula                              |
| Raw Value              | Display    | The actual calculated value (e.g., 94.4%, 4.6/5, etc.)                 |
| Weighted Score         | Score      | (Normalized Raw Value × Weight) — the contribution to the total score  |
| Total Performance Score| Score /100 | Sum of all weighted scores                                              |
| Score Rating Badge     | Badge      | 🟢 Excellent / 🔵 Good / 🟡 Average / 🔴 Below Average                 |

---

## Form Actions

| Action              | Description                                                        |
| ------------------- | ------------------------------------------------------------------ |
| **Back**            | Returns to Performance Dashboard (Screen 26.1)                     |

---

================================================================================

# ⚙️ Backend Calculation Logic

> This section documents the **exact formulas and logic** used by the backend to compute all performance metrics. This is the single source of truth for the development team.

---

## 1. Task Performance Calculations

### 1.1 Completion Rate

```
Completion Rate (%) = (Tasks Completed ÷ Tasks Assigned) × 100
```

| Variable         | Source                            | Filter                                      |
| ---------------- | --------------------------------- | ------------------------------------------- |
| Tasks Completed  | Module 21 → Task records          | Status = "Completed" AND Technician = X AND Date within range |
| Tasks Assigned   | Module 21 → Task records          | All tasks where Technician = X (Primary OR Support) AND Date within range |

**Edge Cases:**
- If Tasks Assigned = 0, Completion Rate = 0% (not null/error)
- Both Primary and Support assignments count as "Assigned"
- Only tasks where technician is Primary count toward "Completed" attribution

---

### 1.2 On-Time Completion Rate

```
On-Time Rate (%) = (On-Time Completed ÷ Tasks Completed) × 100
```

| Variable          | Definition                                                                 |
| ----------------- | -------------------------------------------------------------------------- |
| On-Time Completed | Tasks where `Actual End Time` ≤ `Scheduled End Time + Buffer (30 min)`     |

**Business Rule:** A 30-minute buffer is allowed beyond the scheduled end time before a task is considered "delayed".

---

### 1.3 Overdue Identification

```
IF Task.Scheduled_Date < TODAY
   AND Task.Status ∉ ['Completed', 'Cancelled']
THEN Task.Status = 'Overdue'
```

---

## 2. Time & Utilization Calculations

### 2.1 Total Working Hours

```
Total Working Hours = SUM(Punch_Out_Time − Punch_In_Time)
    for all days where Attendance.Status = 'Present'
    within the selected date range
```

**Source:** Module 25 → Attendance records

---

### 2.2 Task Execution Hours

```
Task Execution Hours = SUM(Actual_End_Time − Actual_Start_Time)
    for all tasks where Status = 'Completed'
    AND Technician = X (Primary or Support)
    within the selected date range
```

**Source:** Module 21.6 → Completion log

---

### 2.3 Utilization Rate

```
Utilization Rate (%) = (Task Execution Hours ÷ Total Working Hours) × 100
```

**Edge Cases:**
- If Total Working Hours = 0, Utilization Rate = 0%
- Capped at 100% (in rare cases where task time exceeds attendance time due to overtime)

---

### 2.4 Tasks Per Day (Productivity)

```
Tasks Per Day = Tasks Completed ÷ Present Days
```

| Variable      | Source                                              |
| ------------- | --------------------------------------------------- |
| Present Days  | Module 25 → Days with Punch-In within date range   |

---

### 2.5 Average Task Duration

```
Avg Task Duration = Task Execution Hours ÷ Tasks Completed
```

---

## 3. Revenue Contribution Calculations

### 3.1 Revenue Attribution Logic

Revenue is attributed to technicians based on their role in the task:

#### Case A: Single Technician (Primary Only, No Support)

```
Technician Revenue = SO Line Item Value (100%)
```

#### Case B: Primary + Support Technician(s)

```
Primary Technician Revenue = SO Line Item Value × 60%
Support Technician Revenue  = (SO Line Item Value × 40%) ÷ Number of Support Technicians
```

**Example:**
- SO Line Item Value = ₹10,000
- 1 Primary (Ravi) + 2 Support (Anjali, Amit)
- Ravi gets: ₹10,000 × 60% = ₹6,000
- Anjali gets: (₹10,000 × 40%) ÷ 2 = ₹2,000
- Amit gets: (₹10,000 × 40%) ÷ 2 = ₹2,000

---

### 3.2 Total Revenue for a Technician

```
Total Revenue = SUM(Revenue Attribution per completed task)
    where Technician = X
    AND Task.Status = 'Completed'
    within date range
```

---

### 3.3 Revenue Percentile Ranking

```
Revenue Percentile = (Number of technicians with lower revenue ÷ Total technicians) × 100
```

Used for the "Top 10%", "Top 25%" labels on the KPI card.

---

## 4. Material Efficiency Calculations

### 4.1 Per-Chemical Efficiency

```
Efficiency (%) = (Std Qty ÷ Actually Used Qty) × 100
```

**Interpretation:**
- \> 100% = Under-usage (technician used less than required) — capped at 100% in score
- = 100% = Exact usage (perfect efficiency)
- < 100% = Over-usage (technician used more than required)

---

### 4.2 Overall Material Efficiency (Weighted)

```
Overall Material Efficiency = 
    SUM(Std_Qty_i × Efficiency_i) ÷ SUM(Std_Qty_i)
    for all chemicals i used by Technician X within date range
```

**Rationale:** Chemicals with higher standard quantities have more weight, so a small overuse on a high-volume chemical matters more than on a low-volume one.

**Capping Rule:** Overall efficiency is capped at 100% — a technician cannot score above 100% by under-using materials (as under-use can also indicate incomplete treatment).

---

## 5. Customer Satisfaction Calculations

### 5.1 Average Customer Rating

```
Avg Rating = SUM(Customer_Rating) ÷ COUNT(Rated Tasks)
    where Task.Technician = X (as Primary)
    AND Task.Status = 'Completed'
    AND Customer_Rating IS NOT NULL
    within date range
```

**Note:** Only tasks where the technician is Primary count toward their rating. Support technicians do not receive individual ratings.

---

### 5.2 Rating Distribution

```
For each star level (1 to 5):
    Count = COUNT(tasks where Customer_Rating = star_level)
    Percentage = (Count ÷ Total Rated Tasks) × 100
```

---

## 6. Re-Task / Rework Calculations

### 6.1 Re-Task Rate

```
Re-Task Rate (%) = (Re-Task Count ÷ Completed as Primary) × 100
```

| Variable             | Definition                                                                   |
| -------------------- | ---------------------------------------------------------------------------- |
| Re-Task Count        | Tasks with Type = "Re-Task" where the **original task's Primary** = this technician |
| Completed as Primary | Tasks completed where this technician was the Primary                        |

**Important Logic:** Re-tasks are attributed to the technician who was Primary on the **original** task (not the re-task itself). This ensures the rework is tracked against the person responsible for the initial service.

---

## 7. Final Performance Score Calculation

### 7.1 Weight Distribution

| Factor                  | Weight | Max Score |
| ----------------------- | ------ | --------- |
| Task Completion Rate    | 30%    | 30        |
| Customer Satisfaction   | 25%    | 25        |
| Time Utilization Rate   | 15%    | 15        |
| Revenue Contribution    | 10%    | 10        |
| Material Efficiency     | 10%    | 10        |
| Re-Task Penalty (Inv.)  | 10%    | 10        |
| **Total**               | **100%** | **100** |

---

### 7.2 Normalization & Scoring Formulas

Each factor is normalized to a 0–1 scale before applying weights:

#### Task Completion Rate Score

```
Normalized = Completion Rate ÷ 100
Weighted Score = Normalized × 30
```

#### Customer Satisfaction Score

```
Normalized = Avg Rating ÷ 5.0
Weighted Score = Normalized × 25
```

#### Time Utilization Score

```
Normalized = Utilization Rate ÷ 100
Weighted Score = Normalized × 15
```

#### Revenue Contribution Score

```
Revenue Percentile = (Rank from bottom ÷ Total technicians) × 100
Normalized = Revenue Percentile ÷ 100
Weighted Score = Normalized × 10
```

#### Material Efficiency Score

```
Normalized = Overall Material Efficiency ÷ 100
Weighted Score = Normalized × 10
```

#### Re-Task Penalty Score (Inverted — lower re-task rate = higher score)

```
Normalized = MAX(0, (1 − (Re-Task Rate ÷ 20)))
Weighted Score = Normalized × 10
```

**Explanation:** A 0% re-task rate gets full score (10/10). A 20%+ re-task rate gets 0/10. Linear scale in between.

---

### 7.3 Final Score Assembly

```
Performance Score = 
    (Completion_Normalized × 30) +
    (Satisfaction_Normalized × 25) +
    (Utilization_Normalized × 15) +
    (Revenue_Normalized × 10) +
    (Material_Normalized × 10) +
    (ReTask_Normalized × 10)
```

**Result Range:** 0 to 100

---

### 7.4 Score Rating Thresholds

| Score Range | Rating         | Badge |
| ----------- | -------------- | ----- |
| 90 – 100    | Excellent      | 🟢    |
| 75 – 89     | Good           | 🔵    |
| 60 – 74     | Average        | 🟡    |
| 0 – 59      | Below Average  | 🔴    |

---

### 7.5 Ranking Logic (Table View)

```
Technicians are ranked in DESCENDING order of Performance Score.
If two technicians have the same score, tie-breaking order:
    1. Higher Completion Rate
    2. Higher Customer Rating
    3. Lower Re-Task Rate
    4. Alphabetical by Name (last resort)
```

---

================================================================================

# Access Control (RBAC)

| Role                     | Permissions                                                                       |
| ------------------------ | --------------------------------------------------------------------------------- |
| **Company Admin**        | Full access: View all technicians across all branches, export data                |
| **Operations Manager**   | Full access: View all technicians across all branches, export data                |
| **Branch Manager**       | View technicians belonging to their assigned branch only                          |
| **Technician Manager**   | View technicians belonging to their assigned branch only                          |
| **Technician**           | ❌ No access (technicians cannot view their own or others' performance scores)    |
| **Senior Technician**    | ❌ No access                                                                      |

---

================================================================================

# Business Rules

| Rule                            | Description                                                                                |
| ------------------------------- | ------------------------------------------------------------------------------------------ |
| Data is Read-Only               | This module does not create, edit, or delete any records. It only reads and calculates.    |
| Date Range Filter Required      | All metrics are calculated within the user-selected date range. No default "all-time" view. |
| Minimum Data Threshold          | Performance Score is calculated only if technician has ≥ 5 completed tasks in the period.  |
| Below Threshold Display         | If < 5 tasks, Score column shows "—" with tooltip: "Insufficient data for scoring."       |
| Real-Time Calculation           | All KPIs are recalculated on page load / filter change (no cached scores).                 |
| Revenue Split: 60/40            | Primary gets 60%, Support share 40% equally. Configurable in admin settings.               |
| Re-Task Attribution             | Re-tasks are attributed to the Primary technician of the **original** task, not the re-task. |
| Material Efficiency Cap         | Capped at 100% to prevent gaming by intentional under-usage.                               |
| Overdue Auto-Marking            | Tasks past scheduled date with Status ≠ Completed are auto-flagged as Overdue.             |
| Score Weight Configurability    | The 30/25/15/10/10/10 weight distribution is configurable via admin settings.              |
| Export Includes All Columns     | Excel/PDF exports include all visible columns plus hidden fields (Employee ID, Branch ID). |
| No Data Periods                 | If technician was on leave the entire period, all metrics show "N/A" with explanation.     |

---

> **Note:** This module is designed as a management-only analytics tool. Technicians do not have visibility into their scores or comparative rankings through the mobile app or this ERP module.

---
=================================================================================================

# 🎯 MODULE 27: USER PROFILE

## Overview

The User Profile module provides a **360-degree, read-only view of any employee or technician** within the Pest Control ERP-CRM system. It consolidates personal details, organization hierarchy, salary configuration, banking details, uploaded documents, and leave summaries into a single, section-wise profile page.

Additionally, for users with the **CEO** role, the profile includes a **Company Profile** section that displays and allows editing of the company details originally submitted during Onboarding (Module 2), along with additional branding fields such as Company Logo, Website, and Tagline.

All data displayed in this module is **sourced from existing modules** — no new data entry occurs here. The profile can be **edited only by users with appropriate RBAC permissions** through the underlying source modules (Employee Management, IAM, HRM).

**Module Connections:**

- **Depends on:** Module 1 (IAM — Account ID, password, login status), Module 2 (Company Onboarding — company details, documents), Module 7 (Branch Management — branch assignment), Module 8 (Employee Master — personal details, role, designation, documents), Module 6 (Configuration — leave types, salary structure, shift settings), Module 25 (HRM — attendance, leave, salary)
- **Used by:** All ERP users (self-view), HR Managers (employee review), Company Admins (full access), CEO (company profile management)
- **Data Nature:** Read-only profile view. Editable only through source modules with RBAC control. Company Profile section is editable only by CEO.

---

The module contains the following screens:

- 27.1 User Profile – View Mode (Default)
- 27.2 User Profile – Edit Mode (RBAC-controlled)
- 27.3 Company Profile – View & Edit (CEO Only)

---

================================================================================

# 27.1 User Profile – View Mode

**Description:**
A comprehensive, section-wise profile page displaying all relevant information about an employee or technician. The page is rendered as a single scrollable view with collapsible sections. Accessible by the employee themselves (self-view) or by authorized managers/admins.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back]              USER PROFILE                         [✏️ Edit]       │
│                                                                              │
│  ┌─ SECTION 1: BASIC USER INFORMATION ────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Profile Photo  : ┌─────┐                                              │  │
│  │                   │ 👤  │                                              │  │
│  │                   └─────┘                                              │  │
│  │                                                                        │  │
│  │  EMP ID         : EMP-00124                                            │  │
│  │  First Name     : Ravi                                                 │  │
│  │  Last Name      : Sharma                                               │  │
│  │  Full Name      : Ravi Sharma (auto-generated)                         │  │
│  │  Email          : ravi.s@company.com                                   │  │
│  │  Contact Number : 9876543210                                           │  │
│  │  Alternate Number: 9123456789                                          │  │
│  │  Account ID     : ravi.s                                               │  │
│  │  Password       : ●●●●●●●●                                            │  │
│  │  Status         : 🟢 Active                                            │  │
│  │  Date of Joining: 15 Jun 2024                                          │  │
│  │  Employment Type: Permanent                                            │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 2: ORGANIZATION INFORMATION ──────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Department    : Operations                                            │  │
│  │  Designation   : Senior Pest Control Technician                        │  │
│  │  Role          : Senior Technician                                     │  │
│  │  Branch        : Mumbai — Andheri                                      │  │
│  │  Reporting Mgr : Anil K. (Branch Manager)                              │  │
│  │  App User      : ✅ Yes (Mobile App Access Enabled)                    │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 3: ADDRESS INFORMATION ───────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  ── Current Address ──────────────────────────────                      │  │
│  │  Address Line 1     : 42, Shanti Nagar, Andheri West                   │  │
│  │  Address Line 2     : Near City Mall                                   │  │
│  │  City               : Mumbai                                           │  │
│  │  State              : Maharashtra                                      │  │
│  │  Country            : India                                            │  │
│  │  Pincode            : 400058                                           │  │
│  │                                                                        │  │
│  │  ☑ Same as Current Address                                             │  │
│  │                                                                        │  │
│  │  ── Permanent Address ────────────────────────────                      │  │
│  │  Address Line 1     : 42, Shanti Nagar, Andheri West  (auto-filled)    │  │
│  │  Address Line 2     : Near City Mall                  (auto-filled)    │  │
│  │  City               : Mumbai                         (auto-filled)    │  │
│  │  State              : Maharashtra                    (auto-filled)    │  │
│  │  Country            : India                          (auto-filled)    │  │
│  │  Pincode            : 400058                         (auto-filled)    │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 4: SALARY INFORMATION ────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Salary Type    : CTC                                                  │  │
│  │  Basic Salary   : ₹20,000                                              │  │
│  │  HRA            : ₹5,000                                               │  │
│  │  Other Allowance: ₹3,000                                               │  │
│  │  Incentive      : ₹2,000                                               │  │
│  │  Deductions     : ₹2,500                                               │  │
│  │                                                                        │  │
│  │  PF Applicable  : ✅ Yes                                                │  │
│  │  ESI Applicable : ✅ Yes                                                │  │
│  │  TDS Applicable : ❌ No                                                 │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 5: BANK INFORMATION ──────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Bank Name          : State Bank of India                              │  │
│  │  Account Number     : ●●●●●●●●4321  (masked)                          │  │
│  │  Account Holder Name: Ravi Sharma                                      │  │
│  │  IFSC Code          : SBIN0001234                                      │  │
│  │  UPI ID             : ravi.s@sbi                                       │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 6: DOCUMENTS ─────────────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Document Type          │ File Name           │ Status    │ Actions    │  │
│  │────────────────────────┼─────────────────────┼───────────┼────────────│  │
│  │  Government ID Proof    │ aadhaar_ravi.pdf    │ ✅ Uploaded│ [📥 ][👁] │  │
│  │  Address Proof          │ utility_bill.pdf    │ ✅ Uploaded│ [📥 ][👁] │  │
│  │  Employment Contract    │ contract_ravi.pdf   │ ✅ Uploaded│ [📥 ][👁] │  │
│  │  Education Certificates │ degree_cert.pdf     │ ✅ Uploaded│ [📥 ][👁] │  │
│  │  Other Documents        │ —                   │ ❌ Pending │     —      │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 7: LEAVE SUMMARY ─────────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  CL Balance     : 8 / 12                                               │  │
│  │  SL Balance     : 5 / 6                                                │  │
│  │  PL Balance     : 10 / 15                                              │  │
│  │  Total Leaves Taken : 4                                                │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 8: COMPANY PROFILE (CEO Only) ─────────────────────────────────┐ │
│  │  ⚠️ This section is ONLY visible when the logged-in user has CEO role   │ │
│  │                                                                        │  │
│  │  Company Logo   : ┌─────┐                                              │  │
│  │                   │ 🏢  │                                              │  │
│  │                   └─────┘                                              │  │
│  │                                                                        │  │
│  │  Company Name   : Acme Pest Solutions Pvt. Ltd.                        │  │
│  │  Tagline        : "Protecting Homes Since 2010"                        │  │
│  │  Industry Type  : Pest Control                                         │  │
│  │  Website        : www.acmepest.com                                     │  │
│  │  Founding Year  : 2010                                                 │  │
│  │                                                                        │  │
│  │  ── Contact Person ──────────────────────────────                      │  │
│  │  Name           : John Doe                                             │  │
│  │  Email          : john@acmepest.com                                    │  │
│  │  Phone          : 9988776655                                           │  │
│  │                                                                        │  │
│  │  ── Legal & Tax Information ─────────────────────                      │  │
│  │  GST Number     : 27AAAAA0000A1Z5                                      │  │
│  │  PAN Number     : ABCDE1234F                                           │  │
│  │  License Number : PCO-MH-2024-1234 (Optional)                          │  │
│  │                                                                        │  │
│  │  ── Registered Address ──────────────────────────                      │  │
│  │  Address Line 1 : Tower A, IT Park                                     │  │
│  │  Address Line 2 : —                                                    │  │
│  │  City           : Noida                                                │  │
│  │  State          : Uttar Pradesh                                        │  │
│  │  Pincode        : 201301                                               │  │
│  │                                                                        │  │
│  │  ── Company Documents ───────────────────────────                      │  │
│  │  GST Certificate         │ gst_cert.pdf     │ ✅ Uploaded │ [📥][👁]  │  │
│  │  PAN Card                │ pan_card.pdf     │ ✅ Uploaded │ [📥][👁]  │  │
│  │  Business Registration   │ biz_reg.pdf      │ ✅ Uploaded │ [📥][👁]  │  │
│  │                                                                        │  │
│  │  ── Onboarding Status ───────────────────────────                      │  │
│  │  Status         : ✅ Approved                                          │  │
│  │  Company Code   : ACME-PINE-456 (read-only)                            │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Section 1: Basic User Information (Read-Only)

| Field             | Type              | Required | Validation                    | Description                                                        | Source              |
| ----------------- | ----------------- | -------- | ----------------------------- | ------------------------------------------------------------------ | ------------------- |
| Profile Photo     | Display (Image)   | No       | JPG/PNG, Max 2MB              | Employee's profile photograph                                       | Module 8            |
| EMP ID            | Display           | Auto     | System-generated, unique      | Unique employee identifier (e.g., EMP-00124)                       | Module 8            |
| First Name        | Display           | Yes      | Min 2 chars, Max 50 chars     | Employee's first name                                               | Module 8            |
| Last Name         | Display           | Yes      | Min 1 char, Max 50 chars      | Employee's last name                                                | Module 8            |
| Full Name         | Display (Auto)    | Auto     | Auto-generated                | Concatenation of First Name + Last Name                             | Calculated          |
| Email             | Display           | Yes      | Valid email format, unique     | Official email used for login and communication                     | Module 8            |
| Contact Number    | Display           | Yes      | Valid 10-digit mobile number   | Primary phone number                                                | Module 8            |
| Alternate Number  | Display           | No       | Valid 10-digit mobile number   | Backup contact number                                               | Module 8            |
| Account ID        | Display           | Auto     | Unique, from IAM               | Login credential used for application authentication               | Module 1 (IAM)      |
| Password          | Display (Masked)  | —        | ●●●●●●●● (always masked)      | Login password — never shown in plain text                          | Module 1 (IAM)      |
| Status            | Display (Badge)   | Auto     | 🟢 Active / 🔴 Inactive       | Current employment status                                           | Module 8            |
| Date of Joining   | Display           | Yes      | Cannot be future date          | Date the employee joined the company                                | Module 8            |
| Employment Type   | Display (Badge)   | Yes      | Permanent / Contract / Intern  | Type of employment engagement                                       | Module 8            |

---

## Section 2: Organization Information (Read-Only)

| Field                      | Type            | Required | Validation                         | Description                                                                                   | Source           |
| -------------------------- | --------------- | -------- | ---------------------------------- | --------------------------------------------------------------------------------------------- | ---------------- |
| Department                 | Display         | Yes      | Must select from configured list   | Business unit (Operations, Sales, Admin, etc.)                                                | Module 8         |
| Designation                | Display         | Yes      | Must select from configured list   | Job title (e.g., Senior Pest Control Technician)                                              | Module 8         |
| Role                       | Display         | Yes      | Must select from system roles      | System role controlling RBAC permissions                                                       | Module 1 (IAM)   |
| Branch                     | Display         | Yes      | Must select from active branches   | Assigned branch for operations                                                                 | Module 7 → 8     |
| Reporting Manager          | Display         | Yes      | Must be an active employee         | Direct supervisor or manager                                                                   | Module 8         |
| Is Application User        | Display (Yes/No)| —        | Boolean (Yes/No)                   | If enabled, user accesses only the mobile app; ERP module permissions are disabled             | Module 1 (IAM)   |

---

## Section 3: Address Information (Read-Only)

| Field                       | Type       | Required    | Validation              | Description                                              | Source    |
| --------------------------- | ---------- | ----------- | ----------------------- | -------------------------------------------------------- | --------- |
| Current Address Line 1      | Display    | Yes         | Min 5 chars, Max 200    | Primary address line                                     | Module 8  |
| Current Address Line 2      | Display    | No          | Max 200 chars           | Additional address information (landmark, floor, etc.)   | Module 8  |
| City                        | Display    | Yes         | Min 2 chars             | City of residence                                        | Module 8  |
| State                       | Display    | Yes         | From predefined list    | State / Province                                         | Module 8  |
| Country                     | Display    | Yes         | Default: India          | Country of residence                                     | Module 8  |
| Pincode                     | Display    | Yes         | 6-digit numeric         | Postal code                                              | Module 8  |
| Same as Current (Checkbox)  | Display    | —           | Boolean                 | If checked, auto-copies current address to permanent     | Module 8  |
| Permanent Address Line 1    | Display    | Conditional | Required if checkbox off| Permanent residential address line 1                     | Module 8  |
| Permanent Address Line 2    | Display    | No          | Max 200 chars           | Additional permanent address details                     | Module 8  |
| Permanent City              | Display    | Conditional | Required if checkbox off| Permanent address city                                   | Module 8  |
| Permanent State             | Display    | Conditional | Required if checkbox off| Permanent address state                                  | Module 8  |
| Permanent Country           | Display    | Conditional | Default: India          | Permanent address country                                | Module 8  |
| Permanent Pincode           | Display    | Conditional | 6-digit numeric         | Permanent address postal code                            | Module 8  |

---

## Section 4: Salary Information (Read-Only)

| Field            | Type              | Required | Validation              | Description                                              | Source                |
| ---------------- | ----------------- | -------- | ----------------------- | -------------------------------------------------------- | --------------------- |
| Salary Type      | Display           | Yes      | CTC / Fixed / Hourly    | Payment model for this employee                          | Module 6 → Module 25  |
| Basic Salary     | Display (Currency)| Yes      | ≥ 0                     | Base monthly salary                                      | Module 25 (Salary)    |
| HRA              | Display (Currency)| No       | ≥ 0                     | House Rent Allowance                                     | Module 25 (Salary)    |
| Other Allowance  | Display (Currency)| No       | ≥ 0                     | Additional company allowances                            | Module 25 (Salary)    |
| Incentive        | Display (Currency)| No       | ≥ 0                     | Performance-based bonus                                  | Module 25 (Salary)    |
| Deductions       | Display (Currency)| No       | ≥ 0                     | Total deductions (tax, benefits, etc.)                   | Module 25 (Salary)    |
| PF Applicable    | Display (Yes/No)  | —        | ✅ Yes / ❌ No           | Whether Provident Fund deduction applies                 | Module 6 → Module 25  |
| ESI Applicable   | Display (Yes/No)  | —        | ✅ Yes / ❌ No           | Whether Employee State Insurance applies                 | Module 6 → Module 25  |
| TDS Applicable   | Display (Yes/No)  | —        | ✅ Yes / ❌ No           | Whether Tax Deduction at Source applies                  | Module 6 → Module 25  |

---

## Section 5: Bank Information (Read-Only)

| Field              | Type             | Required | Validation               | Description                                  | Source    |
| ------------------ | ---------------- | -------- | ------------------------ | -------------------------------------------- | --------- |
| Bank Name          | Display          | Yes      | Min 3 chars              | Bank used for salary payments                | Module 8  |
| Account Number     | Display (Masked) | Yes      | Partially masked display | Employee's bank account number               | Module 8  |
| Account Holder Name| Display          | Yes      | Min 2 chars              | Name as per bank records                     | Module 8  |
| IFSC Code          | Display          | Yes      | 11-char alphanumeric     | Bank branch identification code              | Module 8  |
| UPI ID             | Display          | No       | Valid UPI format         | Digital payment identifier (optional)        | Module 8  |

---

## Section 6: Documents (Read-Only)

| Field                    | Type        | Required | Validation                   | Description                                      | Source    |
| ------------------------ | ----------- | -------- | ---------------------------- | ------------------------------------------------ | --------- |
| Government ID Proof      | Display (Link)| Yes      | PDF/JPG/PNG, Max 5MB         | Aadhaar / PAN / Voter ID or equivalent           | Module 8  |
| Address Proof            | Display (Link)| Yes      | PDF/JPG/PNG, Max 5MB         | Utility bill, rental agreement, etc.             | Module 8  |
| Employment Contract      | Display (Link)| Yes      | PDF only, Max 10MB           | Signed employment agreement                       | Module 8  |
| Education Certificates   | Display (Link)| No       | PDF/JPG/PNG, Max 5MB         | Qualification / degree certificates               | Module 8  |
| Other Documents          | Display (Link)| No       | PDF/JPG/PNG, Max 5MB each    | Any additional employee documentation             | Module 8  |

### Document Actions (View Mode)

| Action       | Icon | Description                                           |
| ------------ | ---- | ----------------------------------------------------- |
| **Download** | 📥   | Download the uploaded document to local device         |
| **View**     | 👁   | Open document in browser preview (PDF/Image viewer)   |

> **Note:** Upload and Delete actions are only available in **Edit Mode** (Screen 27.2).

---

## Section 7: Leave Summary (Read-Only)

| Field              | Type    | Description                                                        | Source              |
| ------------------ | ------- | ------------------------------------------------------------------ | ------------------- |
| CL Balance         | Display | Casual Leave: Used / Total allocation                              | Module 6 → Module 25|
| SL Balance         | Display | Sick Leave: Used / Total allocation                                | Module 6 → Module 25|
| PL Balance         | Display | Paid Leave: Used / Total allocation                                | Module 6 → Module 25|
| Total Leaves Taken | Number  | Total leave days consumed in current year                          | Module 25 (Leave)   |

---

## Section 8: Company Profile (CEO Only)

> **Visibility Rule:** This entire section is **only visible** when the logged-in user has the **CEO** role. For all other roles, this section is completely hidden.

### 8A. Company Identity

| Field              | Type              | Required | Validation                         | Description                                                        | Source              |
| ------------------ | ----------------- | -------- | ---------------------------------- | ------------------------------------------------------------------ | ------------------- |
| Company Logo       | Display (Image)   | No       | JPG/PNG, Max 2MB, Recommended 200×200px | Company logo image used across the ERP and invoices           | **New** (Module 27) |
| Company Name       | Display           | Yes      | Min 3 chars, Max 150 chars         | Registered company name                                            | Module 2            |
| Tagline            | Display           | No       | Max 200 chars                      | Company motto or short branding text                               | **New** (Module 27) |
| Industry Type      | Display (Badge)   | Yes      | Must be from predefined list       | Business sector (Grocery / Pest Control / Clothing)                | Module 2            |
| Website            | Display           | No       | Valid URL format                   | Company website URL                                                | **New** (Module 27) |
| Founding Year      | Display           | No       | 4-digit year, cannot be future     | Year the company was established                                   | **New** (Module 27) |

### 8B. Contact Person

| Field                | Type     | Required | Validation                    | Description                                                | Source    |
| -------------------- | -------- | -------- | ----------------------------- | ---------------------------------------------------------- | --------- |
| Contact Person Name  | Display  | Yes      | Min 2 chars, Max 120 chars    | Primary contact person for the company                     | Module 2  |
| Contact Person Email | Display  | Yes      | Valid email format            | Contact person's email address                             | Module 2  |
| Contact Person Phone | Display  | Yes      | Exactly 10 digits, numeric   | Contact person's phone number                              | Module 2  |

### 8C. Legal & Tax Information

| Field          | Type     | Required | Validation                              | Description                                    | Source    |
| -------------- | -------- | -------- | --------------------------------------- | ---------------------------------------------- | --------- |
| GST Number     | Display  | Yes      | 15-char valid GSTIN format              | Goods & Services Tax Identification Number     | Module 2  |
| PAN Number     | Display  | Yes      | 10-char alphanumeric (AAAAA9999A)       | Permanent Account Number                       | Module 2  |
| License Number | Display  | No       | Alphanumeric if provided                | Business or pest control license number        | Module 2  |

### 8D. Registered Address

| Field            | Type     | Required | Validation             | Description                                    | Source    |
| ---------------- | -------- | -------- | ---------------------- | ---------------------------------------------- | --------- |
| Address Line 1   | Display  | Yes      | Min 5 chars, Max 200   | Primary company address line                   | Module 2  |
| Address Line 2   | Display  | No       | Max 200 chars          | Additional address info (floor, landmark, etc.)| Module 2  |
| City             | Display  | Yes      | Min 2 chars, alphabets | City of registered office                      | Module 2  |
| State            | Display  | Yes      | Min 2 chars, alphabets | State of registered office                     | Module 2  |
| Pincode          | Display  | Yes      | 6-digit numeric        | Postal code of registered office               | Module 2  |

### 8E. Company Documents

| Field                          | Type          | Required | Validation                 | Description                                   | Source    |
| ------------------------------ | ------------- | -------- | -------------------------- | --------------------------------------------- | --------- |
| GST Certificate                | Display (Link)| Yes      | PDF/JPG/PNG, Max 5MB       | Uploaded GST certificate document             | Module 2  |
| PAN Card                       | Display (Link)| Yes      | PDF/JPG/PNG, Max 5MB       | Uploaded PAN card document                    | Module 2  |
| Business / Registration Doc    | Display (Link)| Yes      | PDF/JPG/PNG, Max 10MB      | Trade license or incorporation document       | Module 2  |

### 8F. Onboarding Status (Read-Only)

| Field              | Type            | Required | Description                                              | Source    |
| ------------------ | --------------- | -------- | -------------------------------------------------------- | --------- |
| Onboarding Status  | Display (Badge) | Auto     | 🟢 Approved / 🟡 Pending / 🔴 Rejected                  | Module 2  |
| Company Code       | Display         | Auto     | System-generated unique company identifier (read-only)   | Module 2  |

---

## Page-Level Actions

| Action   | Condition                              | Behaviour                                                     |
| -------- | -------------------------------------- | ------------------------------------------------------------- |
| **Back** | Always visible                         | Returns to previous page (Employee List or Dashboard)         |
| **Edit** | Visible only if user has Edit RBAC     | Opens Edit Mode (Screen 27.2) — toggles fields to editable   |

---

================================================================================

# 27.2 User Profile – Edit Mode

**Description:**
Same layout as View Mode, but applicable fields become editable. Accessible by the user themselves (self-edit for limited fields) or by authorized managers/admins (full edit). Certain fields remain read-only even in Edit Mode (EMP ID, Account ID, Status).

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Cancel]            EDIT PROFILE                         [💾 Save]       │
│                                                                              │
│  ┌─ SECTION 1: BASIC USER INFORMATION ────────────────────────────────────┐  │
│  │  ┌─────┐                                                               │  │
│  │  │ 👤  │  EMP ID : EMP-00124 (read-only)                              │  │
│  │  │Photo│                                                               │  │
│  │  │     │  First Name  : [Ravi_______________]                          │  │
│  │  └─────┘  Last Name   : [Sharma______________]                         │  │
│  │           Full Name   : Ravi Sharma (auto)                             │  │
│  │                                                                        │  │
│  │  Email          : [ravi.s@company.com_____]                            │  │
│  │  Contact Number : [9876543210_____________]                            │  │
│  │  Alt Number     : [9123456789_____________]                            │  │
│  │  Account ID     : ravi.s (read-only)                                   │  │
│  │  Password       : ●●●●●●●● (read-only)                                │  │
│  │  Status         : 🟢 Active (read-only)                                │  │
│  │  Date of Joining: [📅 15 Jun 2024________]                             │  │
│  │  Employment Type: [▼ Permanent ▼_________]                             │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 2: ORGANIZATION INFORMATION ──────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Department    : [▼ Operations ▼_________]                             │  │
│  │  Designation   : [▼ Senior Pest Control Technician ▼]                  │  │
│  │  Role          : [▼ Senior Technician ▼__]                             │  │
│  │  Branch        : [▼ Mumbai — Andheri ▼___]                             │  │
│  │  Reporting Mgr : [🔍 Anil K.____________]                              │  │
│  │  App User      : [☑] Yes                                               │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 3: ADDRESS INFORMATION ───────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  ── Current Address ──────────────────────────────                      │  │
│  │  Address Line 1 : [42, Shanti Nagar, Andheri West__]                   │  │
│  │  Address Line 2 : [_______________________________]                    │  │
│  │  City           : [Mumbai_____________________]                        │  │
│  │  State          : [▼ Maharashtra ▼____________]                        │  │
│  │  Country        : [▼ India ▼__________________]                        │  │
│  │  Pincode        : [400058_____________________]                        │  │
│  │                                                                        │  │
│  │  [☑] Same as Current Address                                           │  │
│  │                                                                        │  │
│  │  ── Permanent Address ────────────────────────────  (disabled)          │  │
│  │  (Auto-filled from Current Address)                                    │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 4: SALARY INFORMATION ────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Salary Type    : [▼ CTC ▼________________]                            │  │
│  │  Basic Salary   : [₹ 20,000_______________]                            │  │
│  │  HRA            : [₹ 5,000________________]                            │  │
│  │  Other Allowance: [₹ 3,000________________]                            │  │
│  │  Incentive      : [₹ 2,000________________]                            │  │
│  │  Deductions     : [₹ 2,500________________]                            │  │
│  │                                                                        │  │
│  │  PF Applicable  : [☑] Yes                                              │  │
│  │  ESI Applicable : [☑] Yes                                              │  │
│  │  TDS Applicable : [☐] No                                               │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 5: BANK INFORMATION ──────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Bank Name      : [State Bank of India____]                            │  │
│  │  Account Number : [123456784321___________]                            │  │
│  │  Account Holder : [Ravi Sharma____________]                            │  │
│  │  IFSC Code      : [SBIN0001234____________]                            │  │
│  │  UPI ID         : [ravi.s@sbi_____________]                            │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 6: DOCUMENTS ─────────────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Document Type          │ File Name           │ Status    │ Actions    │  │
│  │────────────────────────┼─────────────────────┼───────────┼────────────│  │
│  │  Government ID Proof    │ aadhaar_ravi.pdf    │ ✅ Uploaded│[📥][👁][🗑]│  │
│  │  Address Proof          │ utility_bill.pdf    │ ✅ Uploaded│[📥][👁][🗑]│  │
│  │  Employment Contract    │ contract_ravi.pdf   │ ✅ Uploaded│[📥][👁][🗑]│  │
│  │  Education Certificates │ degree_cert.pdf     │ ✅ Uploaded│[📥][👁][🗑]│  │
│  │  Other Documents        │ —                   │ ❌ Pending │ [📤 Upload]│  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 7: LEAVE SUMMARY ─────────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Leave Balance  : CL: 8/12  |  SL: 5/6  |  PL: 10/15  (read-only)    │  │
│  │  Leaves Taken   : 4                                     (read-only)    │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 8: COMPANY PROFILE (CEO Only) ─────────────────────────────────┐ │
│  │  ⚠️ Visible only to CEO role. Editable only by CEO.                     │ │
│  │                                                                        │  │
│  │  Company Logo   : ┌─────┐  [📤 Upload Logo]                            │  │
│  │                   │ 🏢  │                                              │  │
│  │                   └─────┘                                              │  │
│  │                                                                        │  │
│  │  Company Name   : [Acme Pest Solutions Pvt. Ltd.__]                    │  │
│  │  Tagline        : [Protecting Homes Since 2010____]                    │  │
│  │  Industry Type  : [▼ Pest Control ▼_______________]                    │  │
│  │  Website        : [www.acmepest.com_______________]                    │  │
│  │  Founding Year  : [2010___________________________]                    │  │
│  │                                                                        │  │
│  │  ── Contact Person ──────────────────────────────                      │  │
│  │  Name           : [John Doe_______________________]                    │  │
│  │  Email          : [john@acmepest.com______________]                    │  │
│  │  Phone          : [9988776655_____________________]                    │  │
│  │                                                                        │  │
│  │  ── Legal & Tax Information ─────────────────────                      │  │
│  │  GST Number     : [27AAAAA0000A1Z5________________]                    │  │
│  │  PAN Number     : [ABCDE1234F_____________________]                    │  │
│  │  License Number : [PCO-MH-2024-1234_______________]                    │  │
│  │                                                                        │  │
│  │  ── Registered Address ──────────────────────────                      │  │
│  │  Address Line 1 : [Tower A, IT Park_______________]                    │  │
│  │  Address Line 2 : [______________________________]                     │  │
│  │  City           : [Noida_________________________]                     │  │
│  │  State          : [Uttar Pradesh_________________]                     │  │
│  │  Pincode        : [201301________________________]                     │  │
│  │                                                                        │  │
│  │  ── Company Documents ────────────────────────── (read-only)           │  │
│  │  GST Certificate       │ gst_cert.pdf   │ ✅ Uploaded │  [📥][👁]    │  │
│  │  PAN Card              │ pan_card.pdf   │ ✅ Uploaded │  [📥][👁]    │  │
│  │  Business Registration │ biz_reg.pdf    │ ✅ Uploaded │  [📥][👁]    │  │
│  │                                                                        │  │
│  │  ── Onboarding Status ──────────────────────────── (read-only)         │  │
│  │  Status         : ✅ Approved                                          │  │
│  │  Company Code   : ACME-PINE-456                                        │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌────────────────────────────────────────────────────────────────────────┐  │
│  │                     [💾 Save]        [✖ Cancel]                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Editable vs Read-Only Rules (Admin / HR Manager Edit)

| Section                  | Editable Fields                                                          | Always Read-Only Fields                                   |
| ------------------------ | ------------------------------------------------------------------------ | --------------------------------------------------------- |
| 1. Basic Info            | First Name, Last Name, Email, Contact, Alt Number, Employment Type       | EMP ID, Full Name (auto), Account ID, Password, Status ,Date of joining    |
| 2. Organization Info     | Department, Designation, Role, Branch, Reporting Mgr, App User           | —                                                         |
| 3. Address               | All address fields                                                       | —                                                         |
| 4. Salary                | All salary fields                                                        | —                                                         |
| 5. Bank Info             | All bank fields                                                          | —                                                         |
| 6. Documents             | Upload / Delete documents                                                | —                                                         |
| 7. Leave Summary         | —                                                                        | All fields (managed via Module 25 HRM)                    |
| 8. Company Profile       | Company Logo, Name, Tagline, Industry Type, Website, Founding Year, Contact Person (Name/Email/Phone), GST Number, PAN Number, License Number, Address fields | Onboarding Status, Company Code, Company Documents (always read-only once uploaded) |

---

## Self-Edit Rules (Employee editing their own profile)

> When a user opens their own profile and taps **Edit**, only the following fields are editable. All other fields remain read-only.

| Section                  | Self-Editable Fields                                                          |
| ------------------------ | ----------------------------------------------------------------------------- |
| 1. Basic Info            | Contact Number, Alternate Number                                              |
| 2. Organization Info     | — (Read-Only)                                                                 |
| 3. Address               | All current & permanent address fields                                        |
| 4. Salary                | — (Read-Only, view own salary only)                                           |
| 5. Bank Info             | Bank Name, Account Number, Account Holder Name, IFSC Code, UPI ID            |
| 6. Documents             | Upload own documents (Gov ID, Address Proof, Education Certs, Other Docs)    |
| 7. Leave Summary         | — (Read-Only)                                                                 |
| 8. Company Profile       | All fields (CEO role only — see Section 8 details)                           |

> **Note:** Employment Contract can only be uploaded by HR Manager or Admin — not by the employee.
> **Note:** Section 8 (Company Profile) is only visible and editable for users with the CEO role.

### Document Actions (Edit Mode)

| Action       | Icon | Condition                              | Description                                           |
| ------------ | ---- | -------------------------------------- | ----------------------------------------------------- |
| **Upload**   | 📤   | Edit Mode only                         | Upload a new document (opens file picker)             |
| **Download** | 📥   | Always (also in View Mode)             | Download the uploaded document to local device         |
| **View**     | 👁   | Always (also in View Mode)             | Open document in browser preview (PDF/Image viewer)   |
| **Delete**   | 🗑   | Edit Mode only (Admin/HR only)         | Remove uploaded document (confirmation required)       |

---

## Edit Mode Actions

| Action     | Trigger    | System Behaviour                                                                                      |
| ---------- | ---------- | ----------------------------------------------------------------------------------------------------- |
| **Save**   | Tap button | Validates all modified fields → Saves changes to respective source modules → Logs changes in audit trail → Shows success toast: "Profile updated successfully." → Returns to View Mode |
| **Cancel** | Tap button | Discards all unsaved changes → Returns to View Mode. Confirmation dialog: "Discard unsaved changes?" |

---

## Validation Rules (Edit Mode)

| Field            | Validation                                                             |
| ---------------- | ---------------------------------------------------------------------- |
| Email            | Must be valid email format. Must be unique across all employees.       |
| Contact Number   | Must be valid 10-digit Indian mobile number.                           |
| Pincode          | Must be 6-digit numeric.                                               |
| IFSC Code        | Must be 11-character alphanumeric (standard bank format).              |
| UPI ID           | Must follow `name@bank` format if provided.                            |
| Basic Salary     | Must be ≥ 0. Cannot be blank.                                          |
| Documents        | File size limits enforced. Only PDF/JPG/PNG accepted.                  |
| Reporting Manager| Must be an active employee. Cannot be self.                            |

### Section 8 — Company Profile Validation (CEO Only)

| Field                     | Validation                                                                  |
| ------------------------- | --------------------------------------------------------------------------- |
| Company Logo              | JPG/PNG only. Max 2MB. Recommended 200×200px. Optional.                     |
| Company Name              | Min 3, Max 150 chars. Alphanumeric and spaces allowed.                      |
| Tagline                   | Max 200 chars. Optional.                                                    |
| Industry Type             | Must select from predefined dropdown (Grocery / Pest Control / Clothing).   |
| Website                   | Must be a valid URL format if provided. Optional.                           |
| Founding Year             | Must be a 4-digit year. Cannot be in the future. Optional.                  |
| Contact Person Name       | Min 2, Max 120 chars. No numbers or special characters.                     |
| Contact Person Email      | Must be valid email format.                                                 |
| Contact Person Phone      | Exactly 10 digits, numeric only.                                            |
| GST Number                | 15-character valid GSTIN format (2 digits + PAN + 3 chars).                 |
| PAN Number                | 10 characters, format: AAAAA9999A.                                          |
| License Number            | Alphanumeric if provided. Optional.                                         |
| Company Address Line 1    | Min 5 chars, Max 200 chars.                                                 |
| Company City              | Min 2 chars, alphabets only.                                                |
| Company State             | Min 2 chars, alphabets only.                                                |
| Company Pincode           | 6-digit numeric.                                                            |

> **Note:** Company Documents (GST Certificate, PAN Card, Business Registration) are read-only in the User Profile and are not editable or re-uploadable. They can only be managed through Module 2 (Onboarding).

---

================================================================================

# Access Control (RBAC)

| Role                   | View Own Profile | View Others' Profile | Edit Profile | Upload Documents | View Salary | Company Profile (Sec 8) |
| ---------------------- | ---------------- | -------------------- | ------------ | ---------------- | ----------- | ----------------------- |
| **CEO**                | ✅               | ✅ (All employees)   | ✅ (All)     | ✅                | ✅           | ✅ View & Edit           |
| **Company Admin**      | ✅               | ✅ (All employees)   | ✅ (All)     | ✅                | ✅           | ❌ Hidden                |
| **HR Manager**         | ✅               | ✅ (All employees)   | ✅ (All)     | ✅                | ✅           | ❌ Hidden                |
| **Branch Manager**     | ✅               | ✅ (Own branch only) | ✅ (Branch)  | ✅ (Branch)       | ❌           | ❌ Hidden                |
| **Technician Manager** | ✅               | ✅ (Own branch only) | ❌           | ❌                | ❌           | ❌ Hidden                |
| **Operations Manager** | ✅               | ✅ (All employees)   | ❌           | ❌                | ❌           | ❌ Hidden                |
| **Technician**         | ✅ (Self only)   | ❌                   | ❌           | ✅ (Own docs)     | ✅ (Own)     | ❌ Hidden                |
| **Senior Technician**  | ✅ (Self only)   | ❌                   | ❌           | ✅ (Own docs)     | ✅ (Own)     | ❌ Hidden                |

---

================================================================================

# Business Rules

| Rule                        | Description                                                                                  |
| --------------------------- | -------------------------------------------------------------------------------------------- |
| Self-View Default           | When a user opens Profile without specifying an employee, their own profile is displayed.     |
| Password Never Exposed      | Password is always displayed as masked (●●●●●●●●). Cannot be viewed or copied from Profile. |
| Bank Account Masking         | Account Number is partially masked (e.g., ●●●●●●●●4321) in View Mode. Full number visible only in Edit Mode for authorized users. |
| App User Logic              | If "Is Application User" = Yes, the user only accesses the mobile app; ERP module-level permissions are managed via IAM. |
| Address Auto-Copy           | If "Same as Current Address" checkbox is enabled, permanent address fields are auto-populated and disabled. |
| Document Requirements       | Government ID Proof, Address Proof, and Employment Contract are mandatory for profile completion. Missing documents show ❌ Pending status. |
| Status Change Restriction    | Employee Status (Active/Inactive) can only be changed by HR Manager or Company Admin. Deactivating an employee also deactivates their IAM account. |
| Salary Visibility           | Salary section is hidden for users without salary view permission. Technicians and Senior Technicians can only see their own salary. |
| Role Change Cascade         | Changing an employee's Role automatically updates their Module Permissions in Module 1 (IAM). |
| Profile Completeness        | A profile completion indicator (e.g., "85% Complete") can be shown based on how many required fields and documents are filled. |
| Company Profile Visibility  | Section 8 (Company Profile) is **only rendered** when the logged-in user's role = CEO. For all other roles, the section DOM element is not rendered at all. |
| Company Profile Source      | Fields in Section 8 are populated from the Module 2 (Onboarding) company details API (`GET /api/v1/company-details`). New fields (Logo, Tagline, Website, Founding Year) are stored as extensions to the company profile. |
| Company Logo Usage          | The uploaded Company Logo is used on invoices (Module 28), quotations, and other customer-facing documents generated by the ERP. |
| Company Doc Read-Only     | Once company documents (GST Certificate, PAN Card, Business Registration) are uploaded during onboarding (Module 2), they become **permanently read-only** in the User Profile. The CEO can only Download (📥) and View (👁) them — no re-upload or delete is allowed. Any document changes must go through Module 2 onboarding flow. |
| Onboarding Status Read-Only | The Onboarding Status and Company Code fields are always read-only and cannot be modified by any user, including the CEO. They reflect the current verification state from Module 2 → Module 3.

---

> **Note:** This module acts as a consolidated view and editing interface for employee data that is primarily stored in Module 1 (IAM), Module 8 (Employee Master), Module 25 (HRM), and Module 6 (Configuration). Changes made through this profile are written back to the respective source modules.


=============================================================================================


# 🎯 MODULE 28: INVOICING (SALES)

## Overview

Invoicing module manages all **Accounts Receivable** operations — creating, tracking, and managing sales invoices sent to customers. Supports both **Sales Order (SO) linked invoices** and **direct/ad-hoc invoices**. Integrates with Contract billing terms (advance, monthly, per-service) for automated invoice generation. Handles GST compliance including E-Invoice generation, Credit Notes, and multi-branch tax logic (CGST+SGST vs IGST).

**Module Connections:**

- **Depends on:** Module 18 (Customer Master — billing address, GSTIN, state), Module 19 (Contract — billing terms & schedule), Module 20 (Sales Order — pricing, line items), Module 21 (Task Management — service completion trigger), Module 9 (Tax Master — GST rates, HSN codes), Module 7 (Branch — branch state for tax logic)
- **Used by:** Module 30 (Payments — receipt adjustment against invoices), Module 31 (Ledger — customer balance update), Module 33 (Reports — revenue, GST returns, ageing)
- **Prerequisites:** Module 9 (Tax), Module 18 (Customer), Module 20 (Sales Order) must be configured

---

The module contains the following screens:

- 28.1 Invoice Dashboard (Table View)
- 28.2 Create Invoice (From SO / Direct)
- 28.3 View Invoice Detail (Read-only)
- 28.4 Edit Invoice (Draft Only)
- 28.5 Credit Note

---

================================================================================

# 28.1 Invoice Dashboard (Table View)

**Description:**
The default landing screen for Module 28. Displays all sales invoices in a **table/list format** with summary cards showing total receivable, overdue, paid, and draft counts. Allows filtering, searching, and quick actions on invoices. Supports batch PDF export and Tally-compatible export.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           INVOICING (SALES)                                  │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                │  │
│  │                                                                        │  │
│  │ Branch     : [▼ All Branches ▼]                                       │  │
│  │ Customer   : [🔍 Search Customer ▼]                                   │  │
│  │ Status     : [☑ Draft ☑ Sent ☑ Partial ☑ Paid ☑ Overdue ☑ Cancelled]│  │
│  │ Invoice Type: [☑ Tax Invoice ☑ Proforma ☑ Credit Note]              │  │
│  │ Date Range : [📅 From] - [📅 To]                                      │  │
│  │ SO Number  : [____________________]                                    │  │
│  │                                                                        │  │
│  │ Search: [____________________] (Invoice # / Customer / SO #)          │  │
│  │                                                 [Reset Filters]       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [+ CREATE INVOICE]   [📥 EXPORT PDF BATCH]   [📊 TALLY EXPORT]             │
│                                                                              │
│  INVOICE SUMMARY CARDS                                                       │
│  ┌──────────────┬──────────────┬──────────────┬──────────────┐               │
│  │ Total        │ Overdue      │ Paid         │ Drafts       │               │
│  │ Receivable   │              │ (This Month) │              │               │
│  │ ₹ 4,50,000   │ ₹ 85,000     │ ₹ 12,20,000  │ 12 Invoices  │               │
│  └──────────────┴──────────────┴──────────────┴──────────────┘               │
│                                                                              │
│  INVOICE LIST TABLE                                                          │
│  ┌──────────┬──────────┬─────────────┬──────────┬───────────┬──────────────┐ │
│  │Invoice # │Inv. Date │Customer     │SO #      │Inv. Amt   │Pending Amt   │ │
│  │──────────┼──────────┼─────────────┼──────────┼───────────┼──────────────│ │
│  │INV-10024 │15 Mar 26 │ABC Corp Ltd │SO-2045   │₹ 15,000   │₹ 15,000      │ │
│  │INV-10025 │16 Mar 26 │XYZ Hotels   │SO-2048   │₹  8,500   │₹  0          │ │
│  │INV-10026 │16 Mar 26 │Global Biz   │—         │₹ 22,000   │₹ 22,000      │ │
│  └──────────┴──────────┴─────────────┴──────────┴───────────┴──────────────┘ │
│                                                                              │
│  ┌──────────┬──────────────┬──────────────────────────────────────────────┐  │
│  │Due Date  │Status        │Actions                                       │  │
│  │──────────┼──────────────┼──────────────────────────────────────────────│  │
│  │30 Mar 26 │🟡 SENT       │[View] [Edit] [Record Payment] [Send] [PDF]  │  │
│  │31 Mar 26 │🟢 PAID       │[View] [PDF] [Receipt]                       │  │
│  │—         │⚪ DRAFT      │[View] [Edit] [Delete] [Approve & Send]      │  │
│  └──────────┴──────────────┴──────────────────────────────────────────────┘  │
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   10   Next                         │
│                                                                              │
│  Legend: ⚪ Draft  🟡 Sent  🟠 Partial  🟢 Paid  🔴 Overdue  ⛔ Cancelled   │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table View Fields

| Field        | Type   | Required | Description                                                    |
| ------------ | ------ | -------- | -------------------------------------------------------------- |
| Invoice No   | Text   | Auto     | System-generated unique invoice number (INV-XXXXX)             |
| Invoice Date | Date   | Auto     | Date when invoice was created                                  |
| Customer     | Text   | Auto     | Customer name from Module 18                                   |
| SO No        | Link   | Auto     | Linked Sales Order number (clickable → opens Module 20)        |
| Invoice Amt  | Number | Auto     | Total invoice amount including taxes                           |
| Pending Amt  | Number | Auto     | Remaining unpaid amount (Invoice Amt - Received Amt)           |
| Due Date     | Date   | Auto     | Payment due date (Invoice Date + Credit Period)                |
| Status       | Badge  | Auto     | Draft / Sent / Partial / Paid / Overdue / Cancelled            |
| Actions      | Buttons| —        | View / Edit / Delete / Record Payment / download pdf / download receipt |

---

## Summary Card Fields

| Field           | Type   | Description                                              |
| --------------- | ------ | -------------------------------------------------------- |
| Total Receivable| Number | Sum of all unpaid invoice amounts (Sent + Partial + Overdue) |
| Overdue         | Number | Sum of invoices past due date and not fully paid          |
| Paid (Month)    | Number | Total amount received this month against invoices         |
| Drafts          | Number | Count of invoices in Draft status                         |

---

## Filters

| Filter       | Type         | Options                                                |
| ------------ | ------------ | ------------------------------------------------------ |
| Branch       | Dropdown     | All Branches / Specific Branch (from Module 7)         |
| Status       | Multi-select | Draft / Sent / Partial / Paid / Overdue / Cancelled    |
| Date Range   | Date Range   | From – To                                              |

---

## Search

Searchable by:

- Invoice Number
- Customer Name
- Sales Order Number

---

## Actions (Table Row)

| Action             | Type   | Condition              | Description                                            |
| ------------------ | ------ | ---------------------- | ------------------------------------------------------ |
| **View**           | Button | All statuses           | Opens invoice detail in read-only mode (Screen 28.3)   |
| **Edit**           | Button | Draft only             | Opens invoice in edit mode (Screen 28.4)               |
| **Delete**         | Button | Draft only             | Deletes draft invoice after confirmation               |
| **Record Payment** | Button | Sent / Partial / Overdue | Redirects to Module 30 with invoice pre-selected      |
| **Download PDF**   | Button | Sent / Paid            | Download invoice as PDF                                |
| **Download Receipt**| Button | Paid                   | View/download payment receipt from Module 30           |

---

## Form Actions

| Action              | Description                                               |
| ------------------- | --------------------------------------------------------- |
| **+ Create Invoice**| Opens the **Create Invoice Form** (Screen 28.2)           |

---

## Business Rules

| Rule                                  | Description                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| Draft invoices do not affect Ledger   | Only Sent/Approved invoices update the Customer Ledger       |
| Overdue auto-detection                | System marks invoices as Overdue when Due Date passes        |
| Deletion restricted to Draft          | Only Draft invoices can be deleted; Sent invoices need Credit Note |
| Branch-based access                   | Users see invoices for their assigned branches only          |
| Numbering is sequential per branch    | Invoice numbers follow branch-wise sequence (INV-BLR-10001) |

---

## System Behavior

| Event                        | System Action                                                  |
| ---------------------------- | -------------------------------------------------------------- |
| Full payment received        | Invoice status → Paid, Pending Amt → 0                        |
| Partial payment received     | Invoice status → Partial, Pending Amt reduced                 |
| Due date passes (unpaid)     | Invoice status → Overdue, Notification to accounts team       |
| Credit Note issued           | Original invoice amount reduced, Ledger adjusted              |

---

================================================================================

# 28.2 Create Invoice (From SO / Direct)

**Description:**
Form screen to create a new sales invoice. Supports two creation modes: **(1) From Sales Order** — auto-populates line items, pricing, and customer details from the linked SO, **(2) Direct** — manual entry for ad-hoc services or products not tied to an SO. Automatically calculates taxes based on HSN codes and inter-state/intra-state logic.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        CREATE INVOICE                                        │
│                                                                              │
│  INVOICE SOURCE                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Creation Mode*: (•) From Sales Order    ( ) Direct Invoice           │  │
│  │                                                                       │  │
│  │ ── If "From Sales Order" selected ──                                  │  │
│  │ Sales Order*  : [🔍 Search SO # / Customer ▼]     [FETCH DETAILS]    │  │
│  │               (Shows only SOs with status = Confirmed/In Progress)    │  │
│  │                                                                       │  │
│  │ ── If "Direct Invoice" selected ──                                    │  │
│  │ Customer*     : [🔍 Search Customer Name / Code ▼]  [FETCH DETAILS]  │  │
│  │               (Fetches billing details from Module 18)                │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  INVOICE DETAILS                                                             │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Invoice Type*    : [▼ Tax Invoice ▼]                                  │  │
│  │ Invoice Date*    : [📅 28 Mar 2026]       (Default: Today)            │  │
│  │ Credit Period*   : [30] days              Due Date: 27 Apr 2026       │  │
│  │ Branch*          : [▼ Mumbai ▼]           (Auto from SO if linked)    │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  CUSTOMER DETAILS (Auto-fetched from Module 18)                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Customer Name    : ABC Corp Ltd.                                      │  │
│  │ GSTIN            : 27AAACB1234F1Z5                                    │  │
│  │ Billing Address  : 45 MG Road, Fort, Mumbai 400001                    │  │
│  │ State            : Maharashtra                        [Change ▼]      │  │
│  │ Contact Person   : Mr. Ravi Sharma — 9876543210                       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  LINE ITEMS (Editable Grid)                                                  │
│  ┌───┬──────┬────────────┬────────┬─────┬──────┬──────┬──────┬─────┬───────┐  │
│  │Sr │Type  │Description │HSN/SAC │Qty  │UOM   │Rate  │Disc% │Tax% │Amount │  │
│  │───┼──────┼────────────┼────────┼─────┼──────┼──────┼──────┼─────┼───────│  │
│  │ 1 │Svc   │Cockroach   │998531  │  1  │ —    │2,500 │ 0%   │18%  │₹2,950│  │
│  │   │      │Treatment   │        │     │      │      │      │     │      │  │
│  │ 2 │Svc   │Termite     │998531  │  1  │ —    │5,000 │ 5%   │18%  │₹5,605│  │
│  │   │      │Control     │        │     │      │      │      │     │      │  │
│  │ 3 │Prod  │Rodent Bait │392690  │  5  │PKT   │  200 │ 0%   │12%  │₹1,120│  │
│  │   │      │Box         │        │     │      │      │      │     │      │  │
│  └───┴──────┴────────────┴────────┴─────┴──────┴──────┴──────┴─────┴───────┘  │
│  [+ ADD LINE ITEM]    [🗑 REMOVE SELECTED]                                   │
│                                                                              │
│  TAX BREAKDOWN                                                               │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Subtotal (Before Tax)                              ₹ 9,200            │  │
│  │ Discount                                           - ₹ 250            │  │
│  │ ──────────────────────────────────────                                 │  │
│  │ Taxable Amount                                     ₹ 8,950            │  │
│  │ CGST (9%)                                          ₹ 805.50           │  │
│  │ SGST (9%)                                          ₹ 805.50           │  │
│  │ IGST (if inter-state)                              —                   │  │
│  │ ══════════════════════════════════════                                 │  │
│  │ GRAND TOTAL                                        ₹ 10,561           │  │
│  │ (In Words: Rupees Ten Thousand Five Hundred Sixty-One Only)           │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ADDITIONAL DETAILS                                                          │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Notes / Terms    : [_____________________________________________]    │  │
│  │ Internal Remarks : [_____________________________________________]    │  │
│  │ Attachment       : [📎 Upload Supporting Document]                    │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [SAVE AS DRAFT]    [APPROVE & SEND]    [CANCEL]                             │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Screen Fields: Invoice Header

| Field          | Type         | Required | Description                                           |
| -------------- | ------------ | -------- | ----------------------------------------------------- |
| Creation Mode  | Radio        | Yes      | From Sales Order / Direct Invoice                     |
| Sales Order    | Search + dropdown      | Cond.    | Required if mode = From SO. Fetches SO details. Hidden if mode = Direct |
| Customer       | Search + dropdown     | Cond.    | Required if mode = Direct. Search by Name/Code. Fetches billing details from Module 18. Hidden if mode = From SO |
| Invoice Type   | Dropdown     | Yes      | Tax Invoice / Proforma Invoice                        |
| Invoice Date   | Date Picker  | Yes      | Defaults to today. Cannot be future date              |
| Credit Period  | Number       | Yes      | Days allowed for payment (default from Contract)      |
| Due Date       | Date (Auto)  | Auto     | Calculated: Invoice Date + Credit Period              |
| Branch         | Dropdown     | Yes      | Auto-filled from SO, editable for direct invoices     |

---

## Screen Fields: Customer Details

| Field           | Type     | Required | Description                                         |
| --------------- | -------- | -------- | --------------------------------------------------- |
| Customer Name   | Display  | Auto     | Fetched from Module 18 — Customer Master            |
| GSTIN           | Display  | Auto     | Customer's GST number from Module 18                |
| Billing Address | Display  | Auto     | Default billing address from Module 18              |
| State           | Dropdown | Auto     | **Default:** Auto-fetched from Module 18 billing address. **User can override** via `[Change ▼]` to select a different state (e.g., for inter-branch billing). Changing state **re-triggers tax logic** (CGST/SGST ↔ IGST). Options: All Indian states & UTs from Module 9 (Tax Config) |
| Contact Person  | Display (name + mobile) | Auto     | Primary contact from Module 18                      |

> **Data Source:** The entire Customer Details section is auto-populated when a Sales Order is fetched (mode = From SO) or when a Customer is selected (mode = Direct). All data originates from **Module 18 — Customer Master** (billing address, GSTIN, contact). The State field additionally references **Module 9 — Tax Configuration** for the dropdown list and for determining CGST/SGST vs IGST split.

---

## Screen Fields: Line Items Grid

| Field       | Type     | Required | Description                                           |
| ----------- | -------- | -------- | ----------------------------------------------------- |
| Sr. No      | Number   | Auto     | Sequential row number                                 |
| Type        | Tag/Badge| Auto     | Indicates item source: **Svc** (Service — Module 12) or **Prod** (Product — Module 10). Auto-determined when item is added from SO; user selects when adding manually |
| Description | Text     | Yes      | **Product:** `productName` from Module 10. **Service:** `serviceName` from Module 12. Auto-filled from SO line items if linked; manual entry for direct invoices |
| HSN/SAC     | Text     | Yes      | **Product:** `hsnCode` from Module 10. **Service:** SAC code from Module 12 service category. Auto-fetched based on selected item |
| Qty         | Number   | Yes      | Quantity of items. Default 1 for services. For products, must be ≤ available stock (Module 10) |
| UOM         | Dropdown | Auto     | **Product only:** `baseUom` from Module 10 — Options: `LTR` / `KG` / `GRAM` / `ML` / `SET` / `PKT`. **Service:** UOM is not applicable (shows `—`), as services are billed per visit/contract from Module 12 pricing model |
| Rate        | Number   | Yes      | **Product:** `sellingPrice` from Module 10. **Service:** Rate determined by Module 12 `priceType` (Fixed / Area-Based / Inspection). Auto-filled from SO, editable by user |
| Discount %  | Number   | No       | Line-level discount percentage (default 0)            |
| Tax %       | Number   | Auto     | GST rate from Module 9 (Tax Config) based on HSN/SAC code |
| Amount      | Number   | Auto     | Calculated: (Qty × Rate − Discount) + Tax            |

> **Data Source Mapping:**
> - **From SO (Mode = From Sales Order):** Line items auto-populated from **Module 20 — Sales Order** line items. Each SO line item already references either a Product (Module 10) or Service (Module 12), so all fields (Description, HSN/SAC, UOM, Rate) are pre-filled.
> - **Direct Invoice (Mode = Direct):** User manually searches and selects items from **Module 10 — Product Master** or **Module 12 — Service Master**. On selection, Description, HSN/SAC, UOM, and Rate are auto-fetched from the respective module.
> - **UOM Note:** UOM applies only to products (physical goods measured in LTR, KG, GRAM, ML, SET, PKT as defined in Module 10). Services are priced per visit/contract via Module 12 pricing models and do not use physical measurement UOM.

**Grid Action Buttons (Below the line items table):**

| Button                | Action                                                                        |
| --------------------- | ----------------------------------------------------------------------------- |
| **[+ ADD LINE ITEM]** | Opens the **Add Line Item Modal** (see below) to search and select from Module 10/12. |
| **[🗑 REMOVE SELECTED]** | Removes the currently selected/highlighted row(s). Disabled if no row is selected. At least 1 line item must remain (cannot remove all rows) |

### Add Line Item Modal (Popup)

**Description:**
Triggered by clicking `[+ ADD LINE ITEM]`. Allows the user to specify whether they are adding a Service or a Product, search the respective master data (Module 12 or Module 10), preview the details, and add it to the invoice grid.

**Modal Wireframe:**
```
┌──────────────────────────────────────────────────────────────────┐
│  ADD LINE ITEM                                               [X] │
│  ──────────────────────────────────────────────────────────────  │
│                                                                  │
│  Item Type*  :  (•) Service          ( ) Product                 │
│                 (Fetches Mod 12)     (Fetches Mod 10)            │
│  Search Item*:  [🔍 Search by Name / Code ▼]                     │
│                                                                  │
│  ────────────────── ITEM DETAILS & PRICING ────────────────────  │
│  Name           : Termite Barrier                                │
│  SAC/HSN        : 998531                                         │
│  Tax %          : 18%                                            │
│                                                                  │
│  ── Dynamic Form Based on Price Type (From Mod 12) ────────────  │
│                                                                  │
│  == [IF Service Price Type = FIXED_PRICE] =====================  │
│  Category       : [▼ Residential (Internal/External) ]           │
│  Property Type  : [▼ 1BHK ]                                      │
│  Predefined Rate: ₹ 1,500                                        │
│  ==============================================================  │
│                                                                  │
│  == [IF Service Price Type = AREA_BASED] ======================  │
│  Category       : [▼ Commercial (Internal/External)  ]           │
│  Base Price     : ₹ 500.00                                       │
│  Rate Per Sq.Ft : ₹ 2.00                                         │
│  Input Area*    : [   1000  ] SQFT                               │
│  Calculated     : ₹ 500 + (1000 × ₹ 2) = ₹ 2,500                 │
│  ==============================================================  │
│                                                                  │
│  == [IF Service Price Type = INSPECTION_BASED] ================  │
│  Inspection Fee : ₹ 500 (Final price quoted after visit)         │
│  ==============================================================  │
│                                                                  │
│  == [IF Service Price Type = CUSTOM] ==========================  │
│  Config Name    : [▼ Select Custom Config (e.g. Warehouse) ]     │
│  Rate           : — (Manual Entry Required)                      │
│  ==============================================================  │
│                                                                  │
│  == [IF Item Type = Product] ==================================  │
│  UOM            : LTR / KG / Nos (From Mod 10)                   │
│  Selling Price  : ₹ 1,200        (From Mod 10)                   │
│  Stock Available: 50             (From Mod 11 Inventory)         │
│  ==============================================================  │
│                                                                  │
│  Final Rate (₹)*: [  2500  ] (Editable override / manual input)  │
│  Quantity*      : [  1     ]                                     │
│  ──────────────────────────────────────────────────────────────  │
│                                                                  │
│                   [CANCEL]    [ADD TO INVOICE]                   │
└──────────────────────────────────────────────────────────────────┘
```

**Modal Fields & Actions:**

| Field / Action | Description |
| -------------- | ----------- |
| **Item Type** | Radio buttons. Defaults to Service. Dictates which API is called for the search field below. |
| **Search Item** | Auto-complete search. If Type=Service, searches **Module 12** active services. If Type=Product, searches **Module 10** active inventory products. |
| **Item Details** | Read-only preview showing Name, SAC/HSN, UOM, and Tax%. |
| **Pricing Calculation** | **Dynamic Form Based on Price Type (From Mod 12):**<br><br>• **FIXED PRICE:** Renders dropdown for Category (Residential/Commercial) and Property Type (1BHK, 2BHK, Small Office). Auto-fetches the predefined rate.<br><br>• **AREA_BASED:** Renders dropdown for Category. Shows the predefined `Base Price` + `Rate per SQFT`. Renders an `Input Area (SQFT)` field. Auto-calculates `Base Price + (Area × Rate)`.<br><br>• **INSPECTION_BASED:** Shows only the flat `Inspection Fee`.<br><br>• **CUSTOM:** Renders custom configuration dropdown fields mapped from Module 12. Prompt user for manual rate entry. |
| **Final Rate (₹)** | **Editable Input.** Auto-populated based on the `Pricing Calculation` logic above. The user can keep the calculated rate or manually override it (e.g., negotiated discount). If a Product is selected, it defaults to the `sellingPrice` (Mod 10). |
| **Quantity** | Input field for quantity. Defaults to 1. For products, system validates against available stock in Module 10 (or warns about negative inventory). |
| **[ADD TO INVOICE]**| Closes modal and appends the selected item, calculated/overridden rate, and quantity as a new row at the Invoice Line Items grid. |
| **[CANCEL] / [X]** | Closes modal without making changes to the grid. |

---

## Screen Fields: Tax Breakdown

| Field          | Type    | Description                                             |
| -------------- | ------- | ------------------------------------------------------- |
| Subtotal       | Number  | Sum of (Qty × Rate) for all line items                  |
| Discount       | Number  | Sum of all line-level discounts                         |
| Taxable Amount | Number  | Subtotal - Discount                                     |
| CGST           | Number  | Central GST (if Customer State = Branch State)          |
| SGST           | Number  | State GST (if Customer State = Branch State)            |
| IGST           | Number  | Integrated GST (if Customer State ≠ Branch State)       |
| Grand Total    | Number  | Taxable Amount + CGST + SGST (or IGST)                  |
| Amount in Words| Text    | Auto-generated text representation of Grand Total       |

---

## Screen Fields: Additional Details

| Field            | Type        | Required | Description                                    |
| ---------------- | ----------- | -------- | ---------------------------------------------- |
| Notes / Terms    | Textarea    | No       | Payment terms, warranty info shown on invoice  |
| Internal Remarks | Textarea    | No       | Internal notes (not printed on invoice)        |
| Attachment       | File Upload | No       | Supporting documents (PDF/JPG/PNG, max 5MB)    |

---

## Validation Rules

| Field          | Rule                                                         |
| -------------- | ------------------------------------------------------------ |
| Sales Order    | Must exist and have status = Confirmed or In Progress        |
| Invoice Date   | Cannot be a future date. Cannot be before SO date            |
| Credit Period  | Must be a positive number (1–365 days)                       |
| Customer GSTIN | If B2B, GSTIN is mandatory. Validated against GST portal     |
| Line Items     | Minimum 1 line item required                                 |
| Qty            | Must be greater than 0                                       |
| Rate           | Must be greater than 0                                       |
| Discount %     | Must be between 0 and 100                                    |
| HSN/SAC Code   | Must be a valid code from Module 9 Tax configuration         |
| Attachment     | Optional. Max 5MB. Allowed: PDF, JPG, PNG                    |

---

## Business Rules

| Rule                           | Description                                                     |
| ------------------------------ | --------------------------------------------------------------- |
| SO Partial Invoicing           | An SO can have multiple invoices (monthly billing, milestone)   |
| Tax Logic (Intra-state)        | If Customer State = Branch State → CGST + SGST (50/50 split)   |
| Tax Logic (Inter-state)        | If Customer State ≠ Branch State → IGST (full rate)            |
| E-Invoice                      | If B2B invoice > ₹50,000 → E-Invoice with IRN is mandatory    |
| Contract Billing Terms         | If linked to Contract (Module 19), billing mode auto-applied   |
| Duplicate Prevention           | System warns if another invoice exists for same SO + same month|
| Round-off                      | Grand Total rounded to nearest ₹1                              |

---

## Form Actions

| Action             | Description                                                      |
| ------------------ | ---------------------------------------------------------------- |
| **Save as Draft**  | Saves invoice without finalizing. Does NOT update Ledger         |
| **Cancel**         | Discards all changes and returns to Dashboard (28.1)             |

---

## System Behavior

| Event                      | System Action                                                   |
| -------------------------- | --------------------------------------------------------------- |
| SO Selected (Fetch)        | Auto-populates Customer, Line Items, Pricing, Tax               |
| Approve & Send clicked     | Generates Invoice #, updates Customer Ledger (Dr), sends PDF    |
| Save as Draft              | Saves record with status = Draft, no Ledger impact              |
| E-Invoice triggered        | Generates IRN via GST portal API, adds QR code to invoice PDF   |
| Customer GSTIN missing     | Warning: "Customer GSTIN not found. B2C invoice will be created"|

---

================================================================================

# 28.3 View Invoice Detail (Read-only)

**Description:**
A read-only screen showing the complete invoice with all details — customer info, line items, tax breakdown, payment history, and audit trail. Displays the invoice in a print-ready format. Shows linked SO number, payment receipts, and credit notes if any.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       INVOICE DETAIL — INV-10024                             │
│                                                                              │
│  Status: 🟡 SENT                     Due Date: 30 Mar 2026                   │
│                                                                              │
│  ┌──────────────────────────────┬────────────────────────────────────────┐  │
│  │ FROM                          │ TO                                     │  │
│  │ Pest Shield Services Pvt Ltd │ ABC Corp Ltd                           │  │
│  │ GSTIN: 27AAAPS1234F1Z5       │ GSTIN: 27AAACB1234F1Z5               │  │
│  │ 12 Business Park, Andheri    │ 45 MG Road, Fort                      │  │
│  │ Mumbai 400058                │ Mumbai 400001                          │  │
│  │ Maharashtra                   │ Maharashtra                            │  │
│  └──────────────────────────────┴────────────────────────────────────────┘  │
│                                                                              │
│  Invoice #     : INV-10024               Invoice Date : 15 Mar 2026         │
│  SO Reference  : SO-2045 (Clickable)     Credit Period: 15 Days             │
│  Contract Ref  : CON-1008 (Clickable)    Due Date     : 30 Mar 2026        │
│                                                                              │
│  LINE ITEMS                                                                  │
│  ┌───┬────────────────────┬────────┬─────┬──────┬──────┬──────┬──────────┐  │
│  │Sr │Description         │HSN/SAC │Qty  │Rate  │Disc% │Tax%  │Amount    │  │
│  │───┼────────────────────┼────────┼─────┼──────┼──────┼──────┼──────────│  │
│  │ 1 │Cockroach Treatment │998531  │  1  │2,500 │ 0%   │ 18%  │₹ 2,950  │  │
│  │ 2 │Termite Control     │998531  │  1  │5,000 │ 5%   │ 18%  │₹ 5,605  │  │
│  │ 3 │Rodent Bait Box     │392690  │  5  │  200 │ 0%   │ 12%  │₹ 1,120  │  │
│  └───┴────────────────────┴────────┴─────┴──────┴──────┴──────┴──────────┘  │
│                                                                              │
│  TAX SUMMARY                                                                 │
│  ┌──────────────────────────────────────────────────────────────┐            │
│  │ Subtotal          : ₹ 9,200       CGST (9%)  : ₹ 805.50    │            │
│  │ Discount          : - ₹ 250       SGST (9%)  : ₹ 805.50    │            │
│  │ Taxable Amount    : ₹ 8,950       IGST       : —            │            │
│  │ ────────────────────────────────────────────────             │            │
│  │ GRAND TOTAL       : ₹ 10,561                                │            │
│  └──────────────────────────────────────────────────────────────┘            │
│                                                                              │
│  TRANSACTION LEDGER (PAYMENTS & CREDIT NOTES)                                │
│  ┌──────────┬───────────┬────────────┬──────────────────┬──────────────┬──────────────┬────────┐  │
│  │Date      │Type       │Reference # │Description       │Credit Amount │Running Bal   │Action  │  │
│  │──────────┼───────────┼────────────┼──────────────────┼──────────────┼──────────────┼────────│  │
│  │ 15 Mar   │ Invoice   │INV-10024   │Goods Sold        │     —        │ ₹ 10,000     │.       │  │
│  │ 18 Mar   │ Payment   │RCPT-8021   │Bank Transfer     │ ₹ 5,000      │ ₹ 5,000      │[View]  │  │
│  │ 20 Mar   │Credit Note│CN-5001     │Discount Applied  │ ₹ 5,000      │ ₹ 0          │[View]  │  │
│  └──────────┴───────────┴────────────┴──────────────────┴──────────────┴──────────────┴────────┘  │
│  [+ RECORD PAYMENT] (To Mod 30)   [+ ISSUE CREDIT NOTE] (To 28.5)            │
│                                                                              │
│  AUDIT LOG                                                                   │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │ 15 Mar 2026 10:30 — Created by Amit Shah (Draft)                    │    │
│  │ 15 Mar 2026 11:15 — Approved & Sent by Priya Patel                 │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  [📥 DOWNLOAD PDF]  [✉ RESEND]  [🧾 RECORD PAYMENT]                        │
│  [🔙 BACK TO LIST]                                                          │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 1. HEADER

| Field Name    | Description                                            |
| ------------- | ------------------------------------------------------ |
| Invoice Title | Displays invoice detail heading with invoice number    |
| Status        | Current status of invoice (Draft, Sent, Paid, Overdue) |
| Due Date      | Final date by which payment should be completed        |


### 2. FROM (Company Details)
| Field Name     | Description                                      |
| -------------- | ------------------------------------------------ |
| Company Name   | Name of the service provider issuing the invoice |
| GSTIN (From)   | GST identification number of the issuing company |
| Address (From) | Full address of the issuing company              |
| State (From)   | State of the issuing company                     |


### 3. TO (Customer Details)
| Field Name    | Description                               |
| ------------- | ----------------------------------------- |
| Customer Name | Name of the client receiving the invoice  |
| GSTIN (To)    | GST identification number of the customer |
| Address (To)  | Full address of the customer              |
| State (To)    | State of the customer                     |


### 4. Invoice Info
| Field Name              | Description                            |
| ----------------------- | -------------------------------------- |
| Invoice Number          | Unique identifier of the invoice       |
| Invoice Date            | Date when the invoice is generated     |
| SO Reference            | Linked sales order reference           |
| Credit Period           | Allowed payment duration in days       |
| Contract Reference      | Linked contract reference              |
| Due Date (Info Section) | Calculated or defined payment due date |


### 5. Line Items
| Field Name   | Description                          |
| ------------ | ------------------------------------ |
| Sr No        | Serial number of line item           |
| Description  | Service or product description       |
| HSN/SAC Code | Tax classification code              |
| Quantity     | Number of units                      |
| Rate         | Price per unit                       |
| Discount %   | Discount applied on item             |
| Tax %        | Applicable tax percentage            |
| Amount       | Final calculated amount for the item |


### 6. Tax Summary

| Field Name     | Description                           |
| -------------- | ------------------------------------- |
| Subtotal       | Total amount before discount and tax  |
| Discount       | Total discount applied                |
| Taxable Amount | Amount after discount before tax      |
| CGST           | Central GST amount                    |
| SGST           | State GST amount                      |
| IGST           | Integrated GST amount (if applicable) |
| Grand Total    | Final payable amount                  |


### 7. Transaction Ledger

| Field Name     | Description                                               |
| -------------- | --------------------------------------------------------- |
| Date           | Date of the transaction                                   |
| Type           | Source of transaction. Determines the view action. Options: **Invoice** (current document), **Payment** (money received), **Credit Note** (adjustment). |
| Reference #    | Identifying number (INV #, RCPT #, CN #)                  |
| Description    | Reason or mode (e.g., "Bank Transfer", or Credit Note reason from Mod 28.5) |
| Credit Amount  | The amount credited (paid or adjusted) against the invoice|
| Running Bal    | The remaining pending invoice amount after this line      |
| Action         | Dynamic view button. **If Payment:** opens Mod 30 receipt. **If Credit Note:** opens Mod 28.5 view. |
| **[+ RECORD PAYMENT]** | Button that redirects to **Module 30**           |
| **[+ ISSUE CREDIT NOTE]** | Button that redirects to **Screen 28.5 (Credit Note)** |

### 8. Audit Log

| Field Name           | Description                                           |
| -------------------- | ----------------------------------------------------- |
| Activity Date & Time | Timestamp of action performed                         |
| Activity Description | Description of action (Created, Approved, Sent, etc.) |
| Performed By         | User who performed the action                         |
| Status Change        | Status associated with the activity                   |

---

## Actions (View Screen)

| Action              | Type   | Condition          | Description                                      |
| ------------------- | ------ | ------------------ | ------------------------------------------------ |
| **Download PDF**    | Button | Sent / Paid        | Download formatted invoice PDF                   |
| **send**          | Button | Sent / Overdue     | Re-send via Email or WhatsApp                    |
| **Record Payment**  | Button | Sent / Partial     | Redirect to Module 30 with this invoice          |
| **Back to List**    | Button | All                | Returns to Invoice Dashboard (28.1)              |

---

================================================================================

# 28.4 Edit Invoice (Draft Only)

**Description:**
Allows editing of an invoice that is still in **Draft** status. Once an invoice is Approved & Sent, it cannot be edited — a Credit Note must be issued instead. The edit form uses the exact same UI, components, and dynamic logic as the **Create Invoice form (28.2)**. 

*Note for Developers: Re-use the 28.2 UI layout strictly. The "Add Line Item" modal with the dynamic `FIXED_PRICE`/`AREA_BASED` configurations from Module 12 behaves identically here, with all existing data pre-populated.*

---

## Business Rules

| Rule                           | Description                                              |
| ------------------------------ | -------------------------------------------------------- |
| Edit allowed only for Draft    | Sent / Paid / Overdue invoices cannot be modified        |
| Audit trail maintained         | Every edit is logged with user name and timestamp        |
| Re-save as Draft               | Changes are saved without affecting the Ledger           |
| Approve & Send from Edit       | Draft can be finalized directly from the edit screen     |

---

## System Behavior

| Event                    | System Action                                              |
| ------------------------ | ---------------------------------------------------------- |
| User opens Edit          | All fields loaded from saved Draft data                    |
| User modifies line items | Tax breakdown auto-recalculates                            |
| Save as Draft pressed    | Updates existing draft, no Ledger change                   |
| Approve & Send pressed   | Finalizes, generates Invoice #, sends, updates Ledger      |

---

================================================================================

# 28.5 Credit Note (Adjustment)

**Description:**
Used to reduce the pending value of an issued invoice. This is a simplified manual adjustment ledger entry. It does not require selecting individual line items or an approval workflow. It can be issued manually here, or auto-generated by **Module 30 (Payments)** when a user records a short payment and chooses to "Settle & Close" the remaining balance.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         ISSUE CREDIT NOTE / ADJUSTMENT                       │
│                                                                              │
│  ORIGINAL INVOICE                                                            │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Invoice #      : INV-10024                                            │  │
│  │ Customer       : ABC Corp Ltd                                         │  │
│  │ Invoice Amount : ₹ 10,561                                             │  │
│  │ Pending Amount : ₹ 10,561                                             │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ADJUSTMENT DETAILS                                                          │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Credit Note Date* : [📅 28 Mar 2026]                                  │  │
│  │ Reason*           : [▼ Select Reason ▼]                               │  │
│  │                     (Payment Settlement / Pricing Error / Service      │  │
│  │                      Issue / Full Cancellation / Other)                │  │
│  │ Other Reason*     : [_____________________________________________]   │  │
│  │                     (Visible only when Reason = "Other")               │  │
│  │ Remarks*           : [_____________________________________________]   │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ADJUSTMENT AMOUNT                                                           │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Adjust Credit Amt*: [ ₹ 10,000 ]                                       │  │
│  │                                                                       │  │
│  │ ── Summary ────────────────────────────────────────────────────────── │  │
│  │ New Pending Amt   : ₹ 10,561 - ₹ 10,000 = ₹ 561                      │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [ISSUE CREDIT NOTE]    [CANCEL]                                             │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 1. ORIGINAL INVOICE
| Field Name     | Description                                                              |
| -------------- | ------------------------------------------------------------------------ |
| Invoice Number | Reference of the original invoice for which credit note is being created |
| Customer Name  | Name of the customer associated with the invoice                         |
| Invoice Amount | Total amount of the original invoice                                     |
| Pending Amount | Remaining unpaid amount of the invoice                                   |


### 2. Adjustment Details
| Field Name       | Type      | Required | Description                                                                             |
| ---------------- | --------- | -------- | --------------------------------------------------------------------------------------- |
| Credit Note Date | Date      | Yes      | Date on which the credit note is issued                                                 |
| Reason           | Dropdown  | Yes      | Reason for the adjustment. Options: Payment Settlement / Pricing Error / Service Issue / Full Cancellation / Other |
| Other Reason     | Text      | Cond.    | **Visible only when Reason = "Other"**. Free-text reason. Required if Reason = Other    |
| Remarks          | Textarea  | Yes       | Additional comments or explanation for the credit note                                  |
| Adjust Credit Amt| Number    | Yes      | The total manual amount to credit against the invoice's pending balance                 |

---

## Validation Rules

| Field            | Rule                                                       |
| ---------------- | ---------------------------------------------------------- |
| Credit Note Date | Cannot be before original invoice date                     |
| Reason           | Must select from dropdown                                  |
| Other Reason     | Required if Reason = "Other". Cannot be blank whitespace   |
| Credit Amount    | Cannot exceed the current `Pending Amount` of the invoice  |

---

## System Behavior & Automation

| Event                      | System Action                                             |
| -------------------------- | --------------------------------------------------------- |
| Credit Note issued         | Auto-approved immediately. CN number generated (CN-XXXXX) |
| Ledger adjustment          | Customer Ledger credited by CN amount                     |
| Invoice pending reduced    | Original invoice's pending amount reduced by CN amount    |
| **Auto-Generate via Payment**| If Payment (Module 30) is ₹561 against ₹10,561, user can choose "Settle". System auto-creates a ₹10,000 CN internally. |

---

## Status Flow (Module 28 Overall)

```
                    ┌──────────┐
                    │  DRAFT   │
                    └────┬─────┘
                         │ (Approve & Send)
                         ▼
                    ┌──────────┐
                    │   SENT   │
                    └────┬─────┘
                    ┌────┴─────┐
                    │          │
                    ▼          ▼
             ┌──────────┐ ┌──────────┐
             │ PARTIAL  │ │ OVERDUE  │
             │ (Payment)│ │(Due Date)│
             └────┬─────┘ └────┬─────┘
                  │            │ (Payment received)
                  ▼            ▼
             ┌──────────────────┐
             │      PAID        │
             └──────────────────┘

  Side flows:
  DRAFT → CANCELLED (Delete)
  SENT/PAID → CREDIT NOTE ISSUED (Partial/Full)
```

---


=========================================================================================


# 🎯 MODULE 29: BILLS (PURCHASES)

## Overview

Bills module manages all **Accounts Payable** operations — recording, tracking, and managing purchase bills received from vendors/suppliers. Supports both **Purchase Order (PO) linked bills** and **direct expense bills** (rent, utilities, office). Handles vendor TDS deduction, GST Input Tax Credit (ITC), and Debit Notes for purchase returns.

**Module Connections:**

- **Depends on:** Module 11 (Stock Management — PO, GRN, vendor details), Module 10 (Product Master — item prices, HSN), Module 9 (Tax Master — GST rates, TDS rates), Module 7 (Branch — branch state for tax logic)
- **Used by:** Module 30 (Payments — vendor payment adjustment), Module 31 (Ledger — vendor balance update), Module 33 (Reports — expenses, GST ITC, ageing)
- **Prerequisites:** Module 9 (Tax), Module 10 (Product Master), Module 11 (Stock) must be configured

---

The module contains the following screens:

- 29.1 Bills Dashboard (Table View)
- 29.2 Add Purchase Bill (From PO / Direct)
- 29.3 View Bill Detail (Read-only)
- 29.4 Edit Bill (Draft Only)
- 29.5 Debit Note (Adjustment)

---

================================================================================

# 29.1 Bills Dashboard (Table View)

**Description:**
The default landing screen for Module 29. Displays all purchase bills in a **table/list format** with summary cards showing total payable, overdue, paid, and drafts. Supports filtering by vendor, branch, status, date range, and bill type. Includes Vendor Aging Report shortcut.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                            BILLS (PURCHASES)                                 │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                │  │
│  │                                                                        │  │
│  │ Branch     : [▼ All Branches ▼]                                       │  │
│  │ Vendor     : [🔍 Search Vendor ▼]                                     │  │
│  │ Status     : [☑ Draft ☑ Pending ☑ Paid ☑ Overdue ☑ Cancelled]      │  │
│  │ Bill Type  : [☑ Purchase Bill ☑ Expense Bill]                        │  │
│  │ Date Range : [📅 From] - [📅 To]                                      │  │
│  │ PO Number  : [____________________]                                    │  │
│  │                                                                        │  │
│  │ Search: [____________________] (Bill # / Vendor / PO #)              │  │
│  │                                                 [Reset Filters]       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [+ ADD PURCHASE BILL]   [📊 VENDOR AGING REPORT]   [📁 BULK UPLOAD]        │
│                                                                              │
│  BILLS SUMMARY CARDS                                                         │
│  ┌──────────────┬──────────────┬──────────────┬──────────────┐               │
│  │ Total        │ Overdue      │ Paid         │ Drafts       │               │
│  │ Payable      │              │ (This Month) │              │               │
│  │ ₹ 8,20,000   │ ₹ 1,40,000   │ ₹ 5,50,000   │ 8 Bills      │               │
│  └──────────────┴──────────────┴──────────────┴──────────────┘               │
│                                                                              │
│  BILLS LIST TABLE                                                            │
│  ┌──────────┬──────────┬─────────────┬──────────┬───────────┬──────────────┐ │
│  │Bill #    │Bill Date │Vendor       │PO #      │Bill Amt   │Pending Amt   │ │
│  │──────────┼──────────┼─────────────┼──────────┼───────────┼──────────────│ │
│  │BILL-5524 │10 Mar 26 │Industrial X │PO-3012   │₹ 85,000   │₹ 85,000      │ │
│  │BILL-5525 │12 Mar 26 │Agro Chem P  │PO-3015   │₹ 1,20,000 │₹ 0           │ │
│  │BILL-5526 │14 Mar 26 │Office Mart  │—         │₹ 5,000    │₹ 5,000       │ │
│  └──────────┴──────────┴─────────────┴──────────┴───────────┴──────────────┘ │
│                                                                              │
│  ┌──────────┬──────────────┬───────────────────────────────────────┐         │
│  │Due Date  │Status        │Actions                                │         │
│  │──────────┼──────────────┼───────────────────────────────────────│         │
│  │10 Apr 26 │🟡 PENDING    │[View] [Make Payment]                  │         │
│  │12 Apr 26 │🟢 PAID       │[View] [PDF] [Payment Ref]             │         │
│  │—         │⚪ DRAFT      │[View] [Edit] [Delete] [Confirm]       │         │
│  └──────────┴──────────────┴───────────────────────────────────────┘         │
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   10   Next                         │
│                                                                              │
│  Legend: ⚪ Draft  🟡 Pending  🟢 Paid  🟠 Partial  🔴 Overdue  ⛔ Cancelled │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table View Fields

| Field        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| Bill #       | Text   | Auto     | System-generated bill number (BILL-XXXXX)                    |
| Bill Date    | Date   | Auto     | Date on the vendor's physical invoice                        |
| Vendor       | Text   | Auto     | Vendor/Supplier name from Module 11                          |
| PO #         | Link   | Auto     | Linked Purchase Order number (clickable → Module 11)         |
| Bill Amount  | Number | Auto     | Total bill amount including taxes                            |
| Pending Amt  | Number | Auto     | Remaining unpaid amount                                      |
| Due Date     | Date   | Auto     | Bill Date + Credit Period                                    |
| Status       | Badge  | Auto     | Draft / Pending / Paid / Partial / Overdue / Cancelled       |
| Actions      | Buttons| —        | View / Edit / Delete / Confirm / PDF / Payment Ref           |

---

## Summary Card Fields

| Field           | Type   | Description                                               |
| --------------- | ------ | --------------------------------------------------------- |
| Total Payable   | Number | Sum of all unpaid bill amounts                            |
| Overdue         | Number | Sum of bills past due date and not fully paid             |
| Paid (Month)    | Number | Total vendor payments this month                          |
| Drafts          | Number | Count of bills in Draft status                            |

---

## Filters

| Filter     | Type         | Options                                              |
| ---------- | ------------ | ---------------------------------------------------- |
| Branch     | Dropdown     | All Branches / Specific Branch (from Module 7)       |
| Vendor     | Search       | Search by vendor name (from Module 11)               |
| Status     | Multi-select | Draft / Pending / Paid / Overdue / Cancelled           |
| Bill Type  | Multi-select | Purchase Bill / Expense Bill                         |
| Date Range | Date Range   | From – To                                            |
| PO Number  | Text         | Filter by linked Purchase Order number               |

---

## Search

Searchable by:

- Bill Number
- Vendor Name
- Purchase Order Number

---

## Actions (Table Row)

| Action           | Type   | Condition            | Description                                             |
| ---------------- | ------ | -------------------- | ------------------------------------------------------- |
| **View**         | Button | All statuses         | Opens bill detail in read-only mode (Screen 29.3)       |
| **Edit**         | Button | Draft only           | Opens bill in edit mode (Screen 29.4)                   |
| **Delete**       | Button | Draft only           | Deletes draft bill after confirmation                   |
| **Confirm**      | Button | Draft only           | Finalizes bill, updates Vendor Ledger                   |
| **Make Payment** | Button | Pending / Overdue    | Redirects to Module 30 with bill pre-selected           |
| **PDF**          | Button | Pending / Paid       | Download bill copy as PDF                               |
| **Payment Ref**  | Button | Paid                 | View linked payment voucher from Module 30              |

---

## Form Actions

| Action                 | Description                                              |
| ---------------------- | -------------------------------------------------------- |
| **+ Add Purchase Bill**| Opens the **Add Bill Form** (Screen 29.2)                |
| **Vendor Aging Report**| Opens aging summary grouped by vendor                    |
| **Bulk Upload**        | Upload multiple vendor bills via CSV/Excel template      |

---

## Business Rules

| Rule                                | Description                                                  |
| ----------------------------------- | ------------------------------------------------------------ |
| Draft bills do not affect Ledger    | Only Confirmed bills update the Vendor Ledger                |
| Auto-Draft from Stock Entry         | When stock is added in Module 11, a Draft Bill is auto-created |
| Overdue auto-detection              | Bills marked Overdue when Due Date passes without full payment |
| TDS auto-calculation                | If vendor is TDS applicable, TDS is auto-deducted            |

---

## System Behavior

| Event                           | System Action                                                |
| ------------------------------- | ------------------------------------------------------------ |
| Bill Confirmed                  | Vendor Ledger credited, Bill status → Pending                |
| Full payment made               | Bill status → Paid, Pending Amt → 0                          |
| Partial payment made            | Bill status → Partial, Pending Amt reduced                   |
| Due date passes (unpaid)        | Bill status → Overdue, Notification to accounts team         |

---

================================================================================

# 29.2 Add Purchase Bill (From PO / Direct)

**Description:**
Form screen to record a new purchase bill received from a vendor. Supports two modes: **(1) From Purchase Order** — auto-populates line items from linked PO and validates against GRN, **(2) Direct Expense** — for bills not tied to a PO (rent, utilities, subscriptions). Calculates GST Input Tax Credit and TDS automatically.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        ADD PURCHASE BILL                                     │
│                                                                              │
│  BILL SOURCE                                                                 │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Bill Type*    : (•) Purchase Bill (PO Linked)   ( ) Expense Bill     │  │
│  │                                                                       │  │
│  │ Purchase Order: [🔍 Search PO # / Vendor ▼]       [FETCH DETAILS]    │  │
│  │               (Shows only POs with GRN completed / partially received)│  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  BILL HEADER                                                                 │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Vendor Bill #*  : [________________________]  (Vendor's invoice no.)  │  │
│  │ Bill Date*      : [📅 10 Mar 2026]            (Date on vendor bill)   │  │
│  │ Credit Period*  : [30] days                   Due Date: 10 Apr 2026   │  │
│  │ Branch*         : [▼ Mumbai ▼]                                        │  │
│  │ Expense Category: [▼ Chemical / Equipment / Rent / Utilities / Other] │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  VENDOR DETAILS (Auto-fetched from Module 11)                                │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Vendor Name      : Industrial Chemicals Pvt Ltd                       │  │
│  │ GSTIN            : 29AABCI1234F1Z5                                    │  │
│  │ Vendor State     : Karnataka                                          │  │
│  │ TDS Applicable   : Yes — Section 194C (1%)                            │  │
│  │ Payment Terms    : Net 30                                             │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  LINE ITEMS (Auto-populated from PO, editable)                               │
│  ┌───┬────────────┬────────┬─────┬──────┬──────┬──────┬─────┬────────────┐  │
│  │Sr │Description │HSN     │Qty  │UOM   │Rate  │Disc% │Tax% │Amount      │  │
│  │───┼────────────┼────────┼─────┼──────┼──────┼──────┼─────┼────────────│  │
│  │ 1 │Chemical X  │380890  │ 50  │Ltr   │1,200 │ 0%   │18%  │₹ 70,800   │  │
│  │ 2 │Chemical Y  │380890  │ 50  │Ltr   │  600 │ 0%   │18%  │₹ 35,400   │  │
│  └───┴────────────┴────────┴─────┴──────┴──────┴──────┴─────┴────────────┘  │
│  [+ ADD LINE ITEM]    [🗑 REMOVE SELECTED]                                   │
│                                                                              │
│  TAX & TDS BREAKDOWN                                                         │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Subtotal (Before Tax)                              ₹ 90,000          │  │
│  │ Discount                                           - ₹ 0             │  │
│  │ ──────────────────────────────────────                                │  │
│  │ Taxable Amount                                     ₹ 90,000          │  │
│  │ CGST (9%)                                          —                  │  │
│  │ SGST (9%)                                          —                  │  │
│  │ IGST (18%)  (Inter-state: MH → KA)                ₹ 16,200          │  │
│  │ ──────────────────────────────────────                                │  │
│  │ Total Before TDS                                   ₹ 1,06,200        │  │
│  │ TDS (1% u/s 194C)                                  - ₹ 900           │  │
│  │ ══════════════════════════════════════                                │  │
│  │ NET PAYABLE                                        ₹ 1,05,300        │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ATTACHMENTS                                                                 │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Vendor Bill Copy* : [📎 Upload Vendor Invoice]  (MANDATORY)           │  │
│  │ GRN Document      : [📎 Upload GRN]             (Optional)            │  │
│  │ Internal Remarks  : [_____________________________________________]   │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [SAVE AS DRAFT]    [CONFIRM BILL]    [CANCEL]                               │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Screen Fields: Bill Header

| Field            | Type        | Required | Description                                          |
| ---------------- | ----------- | -------- | ---------------------------------------------------- |
| Bill Type        | Radio       | Yes      | Purchase Bill (PO linked) / Expense Bill             |
| Purchase Order   | Search      | Cond.    | Required if Bill Type = Purchase Bill                |
| Vendor Bill #    | Text        | Yes      | The invoice number printed on the vendor's bill      |
| Bill Date        | Date Picker | Yes      | Date mentioned on the vendor's invoice               |
| Credit Period    | Number      | Yes      | Payment terms in days (default from vendor master)   |
| Due Date         | Date (Auto) | Auto     | Calculated: Bill Date + Credit Period                |
| Branch           | Dropdown    | Yes      | Branch receiving the goods/services                  |
| Expense Category | Dropdown    | Cond.    | Required if Bill Type = Expense Bill                 |

---

## Screen Fields: Vendor Details

| Field         | Type    | Required | Description                                          |
| ------------- | ------- | -------- | ---------------------------------------------------- |
| Vendor Name   | Display | Auto     | Fetched from Module 11 Vendor Master                 |
| GSTIN         | Display | Auto     | Vendor's GST number                                  |
| Vendor State  | Display | Auto     | Determines CGST/SGST vs IGST                        |
| TDS Applicable| Display | Auto     | Whether TDS applies and under which section          |
| Payment Terms | Display | Auto     | Default credit period from vendor master             |

---

## Screen Fields: Line Items Grid

| Field       | Type    | Required | Description                                          |
| ----------- | ------- | -------- | ---------------------------------------------------- |
| Sr. No      | Number  | Auto     | Sequential row number                                |
| Description | Text    | Yes      | Item name (auto-filled from PO if linked)            |
| HSN         | Text    | Yes      | HSN code from Product Master                         |
| Qty         | Number  | Yes      | Quantity received (validated against GRN)             |
| UOM         | Text    | Auto     | Unit from Product Master                             |
| Rate        | Number  | Yes      | Per-unit cost from vendor bill                       |
| Discount %  | Number  | No       | Line-level discount (default 0)                      |
| Tax %       | Number  | Auto     | GST rate from Module 9 based on HSN code             |
| Amount      | Number  | Auto     | Calculated: (Qty × Rate - Discount) + Tax            |

---

## Screen Fields: Tax & TDS Breakdown

| Field          | Type   | Description                                              |
| -------------- | ------ | -------------------------------------------------------- |
| Subtotal       | Number | Sum of (Qty × Rate) for all line items                   |
| Discount       | Number | Sum of all line-level discounts                          |
| Taxable Amount | Number | Subtotal - Discount                                      |
| CGST           | Number | Central GST (if Vendor State = Branch State)             |
| SGST           | Number | State GST (if Vendor State = Branch State)               |
| IGST           | Number | Integrated GST (if Vendor State ≠ Branch State)          |
| TDS            | Number | Auto-deducted based on vendor's TDS section and rate     |
| Net Payable    | Number | Taxable Amount + GST - TDS                               |

---

## Screen Fields: Attachments

| Field           | Type        | Required | Description                                    |
| --------------- | ----------- | -------- | ---------------------------------------------- |
| Vendor Bill Copy| File Upload | Yes      | Scanned copy of vendor's physical bill         |
| GRN Document    | File Upload | No       | Goods Receipt Note (if applicable)             |
| Internal Remarks| Textarea    | No       | Notes for internal accounting team             |

---

## Validation Rules

| Field          | Rule                                                          |
| -------------- | ------------------------------------------------------------- |
| Vendor Bill #  | Must be unique per vendor (no duplicate bill numbers)         |
| Bill Date      | Cannot be a future date                                       |
| Credit Period  | Must be a positive number (1–365 days)                        |
| Purchase Order | Must exist and have GRN status = Received / Partial          |
| Qty            | Cannot exceed GRN received quantity                           |
| Rate           | Must be greater than 0                                        |
| Line Items     | Minimum 1 line item required                                  |
| Vendor Bill Copy| Mandatory upload. Max 10MB. Allowed: PDF, JPG, PNG           |
| HSN Code       | Must be valid from Module 9                                   |

---

## Business Rules

| Rule                         | Description                                                     |
| ---------------------------- | --------------------------------------------------------------- |
| Duplicate Bill Check         | System warns if same Vendor Bill # exists for the same vendor   |
| TDS Deduction                | If vendor has TDS flag, TDS auto-deducted before payable calc   |
| GST ITC (Input Tax Credit)   | Tax paid on purchase bills is recorded as Input Credit           |
| Auto-Draft from Module 11    | Stock entry in Module 11 auto-creates a Draft bill              |
| Expense categorization       | Expense bills must be mapped to COA account head (Module 32)    |

---

## Form Actions

| Action                     | Description                                                   |
| -------------------------- | ------------------------------------------------------------- |
| **Save as Draft**          | Saves bill without finalizing. No Ledger impact               |
| **Confirm Bill**           | Confirms the bill and moves status to Pending. Updates Ledger |
| **Cancel**                 | Discards changes and returns to Dashboard (29.1)              |

---

## System Behavior

| Event                        | System Action                                                 |
| ---------------------------- | ------------------------------------------------------------- |
| PO Selected (Fetch)          | Auto-populates Vendor, Line Items, GRN quantities             |
| Confirm Bill clicked         | Bill status → Pending; Vendor Ledger credited; GST recorded   |

---

================================================================================

# 29.3 View Bill Detail (Read-only)

**Description:**
A read-only screen showing the complete bill with all details — vendor info, line items, tax/TDS breakdown, payment history, and audit trail. Shows linked PO, GRN references, and Debit Notes if any.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       BILL DETAIL — BILL-5524                                │
│                                                                              │
│  Status: 🟡 PENDING                 Due Date: 10 Apr 2026                   │
│                                                                              │
│  ┌──────────────────────────────┬────────────────────────────────────────┐  │
│  │ FROM (Vendor)                │ TO (Our Company)                       │  │
│  │ Industrial Chemicals Pvt Ltd │ Pest Shield Services Pvt Ltd          │  │
│  │ GSTIN: 29AABCI1234F1Z5      │ GSTIN: 27AAAPS1234F1Z5               │  │
│  │ Bangalore, Karnataka        │ Mumbai, Maharashtra                    │  │
│  └──────────────────────────────┴────────────────────────────────────────┘  │
│                                                                              │
│  Vendor Bill #  : VEN-INV-2026-456      Bill Date    : 10 Mar 2026          │
│  Our Bill #     : BILL-5524             Credit Period: 30 Days              │
│  PO Reference   : PO-3012 (Clickable)  Due Date     : 10 Apr 2026         │
│  GRN Reference  : GRN-1150 (Clickable)                                     │
│                                                                              │
│  LINE ITEMS                                                                  │
│  ┌───┬────────────────────┬────────┬─────┬──────┬──────┬──────┬──────────┐  │
│  │Sr │Description         │HSN     │Qty  │Rate  │Disc% │Tax%  │Amount    │  │
│  │───┼────────────────────┼────────┼─────┼──────┼──────┼──────┼──────────│  │
│  │ 1 │Chemical X          │380890  │ 50  │1,200 │ 0%   │ 18%  │₹ 70,800 │  │
│  │ 2 │Chemical Y          │380890  │ 50  │  600 │ 0%   │ 18%  │₹ 35,400 │  │
│  └───┴────────────────────┴────────┴─────┴──────┴──────┴──────┴──────────┘  │
│                                                                              │
│  TAX & TDS SUMMARY                                                           │
│  ┌──────────────────────────────────────────────────────────────┐            │
│  │ Taxable Amount    : ₹ 90,000     IGST (18%)  : ₹ 16,200    │            │
│  │ TDS (1% u/s 194C) : - ₹ 900                                 │            │
│  │ ────────────────────────────────────────────────             │            │
│  │ NET PAYABLE       : ₹ 1,05,300                              │            │
│  └──────────────────────────────────────────────────────────────┘            │
│                                                                              │
│  TRANSACTION LEDGER (PAYMENTS & DEBIT NOTES)                                 │
│  ┌──────────┬───────────┬────────────┬──────────────────┬──────────────┬──────────────┬────────┐  │
│  │Date      │Type       │Reference # │Description       │Debit Amount  │Running Bal   │Action  │  │
│  │──────────┼───────────┼────────────┼──────────────────┼──────────────┼──────────────┼────────│  │
│  │ 10 Mar   │ Bill      │BILL-5524   │Purchase          │     —        │ ₹ 1,05,300   │[View]  │  │
│  │ 12 Mar   │ Payment   │RCPT-8021   │Bank Transfer     │ ₹ 50,000     │ ₹ 55,300     │[View]  │  │
│  │ 15 Mar   │Debit Note │DN-5001     │Discount Applied  │ ₹ 10,000     │ ₹ 45,300     │[View]  │  │
│  └──────────┴───────────┴────────────┴──────────────────┴──────────────┴──────────────┴────────┘  │
│  [+ RECORD PAYMENT] (To Mod 30)   [+ ISSUE DEBIT NOTE] (To Mod 29.5)         │
│                                                                              │
│  ATTACHED DOCUMENTS                                                          │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │ 📎 Vendor_Invoice_VEN-INV-2026-456.pdf    [👁 View] [📥 Download]   │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  AUDIT LOG                                                                   │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │ 10 Mar 2026 09:00 — Created by Suresh Kumar (Draft)                 │    │
│  │ 10 Mar 2026 09:30 — Auto-linked to PO-3012 and GRN-1150            │    │
│  │ 10 Mar 2026 10:00 — Confirmed by Suresh Kumar                      │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  [📥 DOWNLOAD PDF]  [🧾 MAKE PAYMENT]  [📄 DEBIT NOTE]                     │
│  [🔙 BACK TO LIST]                                                          │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## View Fields

### 1. HEADER
| Field Name    | Description                                            |
| ------------- | ------------------------------------------------------ |
| Bill Title    | Displays bill detail heading with our bill number      |
| Status        | Current status of bill (Pending, Paid, Overdue, etc.)  |
| Due Date      | Final date by which payment should be completed        |

### 2. FROM (Vendor Details)
| Field Name     | Description                                      |
| -------------- | ------------------------------------------------ |
| Vendor Name    | Name of the vendor issuing the bill              |
| GSTIN (From)   | GST identification number of the vendor          |
| Address (From) | Address of the vendor                            |

### 3. TO (Company Details)
| Field Name     | Description                               |
| -------------- | ----------------------------------------- |
| Company Name   | Name of our company receiving the bill    |
| GSTIN (To)     | Our GST identification number             |
| Address (To)   | Our billing address                       |

### 4. Bill Info
| Field Name              | Description                            |
| ----------------------- | -------------------------------------- |
| Vendor Bill #           | Vendor's own invoice number            |
| Our Bill #              | System-generated internal bill number  |
| Bill Date               | Date printed on the vendor's bill      |
| Credit Period           | Credit days agreed with vendor         |
| PO Reference            | Linked Purchase Order (Clickable)      |
| GRN Reference           | Linked Goods Receipt Note (Clickable)  |

### 5. Line Items
| Field Name       | Description                              |
| ---------------- | ---------------------------------------- |
| Sr / Description | Name of product/service                  |
| HSN              | HSN/SAC code of the item                 |
| Qty / Rate       | Quantity and per-unit rate               |
| Disc% / Tax%     | Discount and tax rates applied to row    |
| Amount           | Total amount for the row                 |

### 6. Tax & TDS Summary
| Field Name     | Description                           |
| -------------- | ------------------------------------- |
| Taxable Amount | Total amount before tax but after discount |
| SGST/CGST/IGST | Applicable tax amounts                |
| TDS            | TDS section and deducted amount       |
| Net Payable    | Final payable amount to vendor        |

### 7. Transaction Ledger
| Field Name     | Description                                               |
| -------------- | --------------------------------------------------------- |
| Date           | Date of the transaction                                   |
| Type           | Source of transaction. Determines the view action. Options: **Bill** (current doc), **Payment** (money paid), **Debit Note** (adjustment). |
| Reference #    | Identifying number (BILL #, RCPT #, DN #)                 |
| Description    | Reason or mode (e.g., "Bank Transfer", or DN reason)      |
| Debit Amount   | The amount debited (paid or adjusted) against the bill    |
| Running Bal    | The remaining pending bill amount                         |
| Action         | Dynamic view button. **If Payment:** opens Mod 30 view. **If Debit Note:** opens Mod 29.5 view. |
| **[+ RECORD PAYMENT]** | Button that redirects to **Module 30**           |
| **[+ ISSUE DEBIT NOTE]** | Button that redirects to **Screen 29.5 (Debit Note)** |

### 8. Attached Documents
| Field Name           | Description                                           |
| -------------------- | ----------------------------------------------------- |
| Original Bill File   | Downloadable/viewable vendor invoice attachment       |

### 9. Audit Log
| Field Name           | Description                                           |
| -------------------- | ----------------------------------------------------- |
| Activity Date & Time | Timestamp of action performed                         |
| Activity Description | Description of action (Created, Linked, Confirmed, etc.) |
| Performed By         | User who performed the action                         |

---

## Actions (View Screen)

| Action           | Type   | Condition          | Description                                      |
| ---------------- | ------ | ------------------ | ------------------------------------------------ |
| **Make Payment** | Button | Pending / Overdue  | Redirect to Module 30 with bill pre-selected     |
| **Debit Note**   | Button | Pending / Paid     | Opens Debit Note form (Screen 29.5)              |
| **Download PDF** | Button | All except Draft   | Download bill details as PDF                     |
| **Back to List** | Button | All                | Returns to Bills Dashboard (29.1)                |

---

================================================================================

# 29.4 Edit Bill (Draft Only)

**Description:**
Allows editing of a bill that is still in **Draft** status. Once the bill is confirmed, it cannot be edited. The edit form has the same layout as Add Bill (29.2) with all fields pre-populated.

---

## Business Rules

| Rule                            | Description                                              |
| ------------------------------- | -------------------------------------------------------- |
| Edit allowed only for Draft     | Pending/Paid bills cannot be modified                    |
| Audit trail maintained          | Every edit is logged with user name and timestamp        |
| Re-save as Draft                | Changes saved without affecting the Ledger               |
| Confirm from Edit               | Draft can be confirmed directly from the edit screen     |

---

## System Behavior

| Event                     | System Action                                              |
| ------------------------- | ---------------------------------------------------------- |
| User opens Edit           | All fields loaded from saved Draft data                    |
| User modifies line items  | Tax/TDS breakdown auto-recalculates                        |
| Save as Draft pressed     | Updates existing draft, no Ledger change                   |
| Confirm Bill              | Bill is confirmed, vendor ledger updated, status → Pending |

---

================================================================================

# 29.5 Debit Note (Adjustment)

**Description:**
A form to issue a Debit Note against an existing purchase bill. Used to reduce the vendor's payable balance in scenarios like purchase returns, billing errors, or post-purchase discounts. Supports auto-generation during the payment settlement process (Module 30).

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           ISSUE DEBIT NOTE                                   │
│                                                                              │
│  REFERENCE DETAILS                                                           │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Select Bill*      : [🔍 Search Vendor / Bill # ▼]                     │  │
│  │                     (Shows only Pending or Paid bills)                │  │
│  │                                                                       │  │
│  │ Vendor            : Industrial Chemicals Pvt Ltd                      │  │
│  │ Bill Amount       : ₹ 1,05,300                                       │  │
│  │ Pending Amount    : ₹ 55,300                                         │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ADJUSTMENT DETAILS                                                          │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Date*             : [📅 15 Mar 2026]                                  │  │
│  │ Reason*           : [▼ Purchase Return / Discount / Error / Other]    │  │
│  │ Adjust Debit Amt* : [₹ 10,000      ]                                  │  │
│  │                     (Must be ≤ Pending Amount. Current max: ₹ 55,300)│  │
│  │ Remarks           : [_______________________________________________] │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ATTACHMENTS                                                                 │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Supporting Doc    : [📎 Upload Return LR / Vendor Email]              │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│                                              [ISSUE DEBIT NOTE] [CANCEL]     │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Screen Fields

| Field            | Type        | Required | Description                                                  |
| ---------------- | ----------- | -------- | ------------------------------------------------------------ |
| Select Bill      | Search/Drop | Yes      | Select the original bill against which Debit Note is issued  |
| Date             | Date Picker | Yes      | Date of issuing the Debit Note                               |
| Reason           | Dropdown    | Yes      | Reason for adjustment                                        |
| Adjust Debit Amt | Number      | Yes      | The flat amount by which the bill value is reduced           |
| Remarks          | Textarea    | No       | Notes regarding the return/adjustment                        |
| Supporting Doc   | File Upload | No       | Proof of return, vendor communication, etc.                  |

---

## Validation Rules

| Field            | Rule                                                          |
| ---------------- | ------------------------------------------------------------- |
| Adjust Debit Amt | Must be greater than 0                                        |
| Adjust Debit Amt | CANNOT exceed the current Pending Amount of the selected bill |
| Selected Bill    | Must be in Pending or Paid status (cannot be Draft/Cancelled) |

---

## Business Rules

| Rule                               | Description                                                    |
| ---------------------------------- | -------------------------------------------------------------- |
| Flat Adjustment                    | Affects the overall bill value; does not require line-item selection |
| GST / Tax Reversal                 | Expected to be handled manually or proportionally applied at the accounting layer |
| Auto-Generation (Module 30)        | When a payment is settled for less than the pending amount and marked "Settle & Close," a Debit Note is auto-generated for the shortfall. |

---

## System Behavior

| Event                           | System Action                                                |
| ------------------------------- | ------------------------------------------------------------ |
| "Issue Debit Note" Clicked      | Debit Note created (DN-XXXXX)                                |
| Ledger Effect                   | Vendor's payable balance is reduced by the Debit Amount      |
| Bill Record Update              | Original bill's Pending Amount is reduced by the Debit Amount|
| Status Update (If Fully Adjusted)| If the Debit Note reduces the Pending Amount to 0, bill status changes to Paid |

---

## Status Flow (Module 29 Overall)

```
                    ┌──────────┐
                    │  DRAFT   │
                    └────┬─────┘
                         │ (Confirm Bill)
                         ▼
                    ┌──────────┐
                    │ PENDING  │
                    └────┬─────┘
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
       ┌──────────┐          ┌──────────┐
       │ PARTIAL  │          │ OVERDUE  │
       │ (Payment)│          │(Due Date)│
       └────┬─────┘          └────┬─────┘
            │                     │
            ▼                     ▼
       ┌─────────────────────────────┐
       │           PAID              │
       └─────────────────────────────┘

  Side flows:
  DRAFT → CANCELLED
  PENDING/PAID → DEBIT NOTE ISSUED (Adjustment)
```

---

====================================================================================================


# 🎯 MODULE 30: PAYMENTS (RECEIPTS & VOUCHERS)

## Overview

Payments module manages all **Cash and Bank transactions** — recording money received from customers (Receipts) and money paid to vendors/expenses (Payments). Supports multiple payment modes (Cash, Bank Transfer, UPI, Cheque, Card). Links every transaction to specific Invoices (Module 28) or Bills (Module 29) for reconciliation. Handles Contra entries (inter-bank transfers), Journal entries (non-cash adjustments), **Shortfall Settlement** (auto-generates Credit/Debit Notes), and **Advance Adjustment** (auto-allocates "On Account" balances against new documents).

**Module Connections:**

- **Depends on:** Module 28 (Invoicing — receipt allocation against sales invoices), Module 29 (Bills — payment allocation against purchase bills), Module 18 (Customer Master — customer details), Module 14 (Vendor Master — vendor details, TDS config), Module 31 (Ledger — party balance), Module 32 (Chart of Accounts — bank/cash account heads)
- **Used by:** Module 31 (Ledger — balance reduction on payment/receipt), Module 33 (Reports — cash flow, bank reconciliation)
- **Triggers:** Module 28.5 (Credit Note — auto-generated on receipt shortfall settlement), Module 29.5 (Debit Note — auto-generated on payment shortfall settlement)
- **Deep-link Sources:** Module 28.3 `[+ RECORD PAYMENT]` button pre-populates Receipt Entry (30.2), Module 29.3 `[+ MAKE PAYMENT]` button pre-populates Payment Entry (30.3)

---

The module contains the following screens:

- 30.1 Payment Register Dashboard (Table View)
- 30.2 Receipt Entry (Money IN — from Customer)
- 30.3 Payment Entry (Money OUT — to Vendor)
- 30.4 Contra Entry (Inter-Bank Transfer)
- 30.5 Journal Entry (Non-Cash Adjustments)
- 30.6 View Voucher Detail (Read-only)

---

================================================================================

# 30.1 Payment Register Dashboard (Table View)

**Description:**
The default landing screen for Module 30. Displays all payment and receipt vouchers in a unified table. Shows summary cards for total receipts, total payments, and net cash flow for the selected period. Supports tab-based filtering: **All / Receipts / Payments / Contra / Journal**.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        PAYMENT REGISTER                                      │
│                                                                              │
│  TABS: [ ALL ]  [ RECEIPTS ]  [ PAYMENTS ]  [ CONTRA ]  [ JOURNAL ]         │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                │  │
│  │                                                                        │  │
│  │ Branch     : [▼ All Branches ▼]                                       │  │
│  │ Party      : [🔍 Search Customer / Vendor ▼]                         │  │
│  │ Mode       : [☑ Cash ☑ Bank ☑ UPI ☑ Cheque ☑ Card]                 │  │
│  │ Date Range : [📅 From] - [📅 To]                                      │  │
│  │ Bank A/C   : [▼ All Bank Accounts ▼]                                  │  │
│  │                                                                        │  │
│  │ Search: [____________________] (Voucher # / Party / Ref #)           │  │
│  │                                                 [Reset Filters]       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [+ RECEIPT]  [+ PAYMENT]  [+ CONTRA]  [+ JOURNAL]                          │
│                                                                              │
│  SUMMARY CARDS (For selected period)                                         │
│  ┌──────────────┬──────────────┬──────────────┬──────────────┐               │
│  │ Total        │ Total        │ Net Cash     │ Unallocated  │               │
│  │ Receipts     │ Payments     │ Flow         │ Advances     │               │
│  │ ₹ 8,50,000   │ ₹ 5,20,000   │ ₹ 3,30,000   │ ₹ 45,000     │               │
│  └──────────────┴──────────────┴──────────────┴──────────────┘               │
│                                                                              │
│  VOUCHER LIST TABLE                                                          │
│  ┌──────────┬──────┬──────────┬──────────────┬──────────┬──────────────┐     │
│  │Voucher # │Type  │Date      │Party         │Ref #     │Amount        │     │
│  │──────────┼──────┼──────────┼──────────────┼──────────┼──────────────│     │
│  │RCP-3001  │🟢 IN │25 Mar 26 │ABC Corp Ltd  │UTR123456 │₹ 15,000      │     │
│  │PAY-4001  │🔴 OUT│26 Mar 26 │Industrial X  │CHQ-78901 │₹ 85,000      │     │
│  │CNT-101   │🔵 CNT│27 Mar 26 │— (Self)      │—         │₹ 2,00,000    │     │
│  │JRN-201   │⚙️ JRN│28 Mar 26 │— (Internal)  │—         │₹ 5,000       │     │
│  └──────────┴──────┴──────────┴──────────────┴──────────┴──────────────┘     │
│                                                                              │
│  ┌────────────┬──────────────┬───────────┬─────────────────────────────┐     │
│  │Mode        │Allocated To  │Settlement │Actions                      │     │
│  │────────────┼──────────────┼───────────┼─────────────────────────────│     │
│  │UPI         │INV-10024     │—          │[View] [PDF] [Print Receipt] │     │
│  │Cheque      │BILL-5524     │DN-5001    │[View] [PDF] [Print Voucher]│     │
│  │Bank Trfr   │HDFC → SBI    │—          │[View]                      │     │
│  │Adjustment  │TDS Adj.      │—          │[View]                      │     │
│  └────────────┴──────────────┴───────────┴─────────────────────────────┘     │
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   10   Next                         │
│                                                                              │
│  Legend: 🟢 Receipt  🔴 Payment  🔵 Contra  ⚙️ Journal                      │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table View Fields

| Field         | Type   | Required | Description                                               |
| ------------- | ------ | -------- | --------------------------------------------------------- |
| Voucher #     | Text   | Auto     | System-generated (RCP-XXXX / PAY-XXXX / CNT-XXX / JRN-XXX)|
| Type          | Badge  | Auto     | Receipt (IN) / Payment (OUT) / Contra / Journal           |
| Date          | Date   | Auto     | Transaction date                                          |
| Party         | Text   | Auto     | Customer / Vendor name (or "Self" / "Internal")           |
| Ref #         | Text   | Auto     | UTR / Cheque # / Transaction ID                          |
| Amount        | Number | Auto     | Transaction amount                                        |
| Mode          | Text   | Auto     | Cash / Bank / UPI / Cheque / Card / Adjustment            |
| Allocated To  | Link   | Auto     | Invoice # / Bill # it was adjusted against (clickable)    |
| Settlement    | Link   | Auto     | Linked CN # / DN # if auto-generated (clickable)         |
| Actions       | Buttons| —        | View / PDF / Print                                        |

---

## Summary Card Fields

| Field            | Type   | Description                                             |
| ---------------- | ------ | ------------------------------------------------------- |
| Total Receipts   | Number | Sum of all receipt vouchers in the period                |
| Total Payments   | Number | Sum of all payment vouchers in the period                |
| Net Cash Flow    | Number | Total Receipts - Total Payments                         |
| Unallocated Adv. | Number | Money received but not allocated to any specific invoice |

---

## Filters

| Filter     | Type         | Options                                               |
| ---------- | ------------ | ----------------------------------------------------- |
| Branch     | Dropdown     | All Branches / Specific Branch (from Module 7)        |
| Party      | Search       | Customer / Vendor name                                |
| Mode       | Multi-select | Cash / Bank / UPI / Cheque / Card                     |
| Date Range | Date Range   | From – To                                             |
| Bank A/C   | Dropdown     | All / HDFC / SBI / etc. (from Module 32)              |

---

## Search

Searchable by:

- Voucher Number
- Party Name
- Reference Number (UTR / Cheque #)

---

## Actions (Table Row)

| Action            | Type   | Condition | Description                                       |
| ----------------- | ------ | --------- | ------------------------------------------------- |
| **View**          | Button | All       | Opens voucher in read-only mode (Screen 30.6)     |
| **PDF**           | Button | All       | Download voucher as PDF                           |
| **Print Receipt** | Button | Receipts  | Print formatted receipt for customer              |
| **Print Voucher** | Button | Payments  | Print payment voucher for vendor records          |

---

## Form Actions

| Action         | Description                                               |
| -------------- | --------------------------------------------------------- |
| **+ Receipt**  | Opens Receipt Entry form (Screen 30.2)                    |
| **+ Payment**  | Opens Payment Entry form (Screen 30.3)                    |
| **+ Contra**   | Opens Contra Entry form (Screen 30.4)                     |
| **+ Journal**  | Opens Journal Entry form (Screen 30.5)                    |

---

================================================================================

# 30.2 Receipt Entry (Money IN — from Customer)

**Description:**
Form to record money received from a customer. The amount is allocated against one or more pending invoices from Module 28. If the amount exceeds all pending invoices, the excess is marked as **"On Account" (Advance)**. If the amount is less than the total pending, the user can choose to **Settle & Close** (auto-generates Credit Note via Module 28.5 for the shortfall) or **Keep Open** (invoice remains Partial). Supports all payment modes. Auto-updates Customer Ledger upon saving.

**Deep-Link:** When opened via `[+ RECORD PAYMENT]` from Module 28.3, the `Customer` and target `Invoice` are pre-selected and the invoice is auto-checked in the allocation grid. The customer field is locked (read-only).

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        RECEIPT ENTRY (MONEY IN)                              │
│                                                                              │
│  RECEIPT DETAILS                                                             │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Receipt Date*    : [📅 28 Mar 2026]        (Default: Today)           │  │
│  │ Branch*          : [▼ Mumbai ▼]                                       │  │
│  │ Customer*        : [🔍 Search Customer ▼]  (🔒 if deep-linked)       │  │
│  │ Current Balance  : ₹ 45,000 (Owed by customer)     [DISPLAY]         │  │
│  │ Advance Balance  : ₹ 2,000 (On Account)             [DISPLAY]        │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ADVANCE ADJUSTMENT (Visible only if Advance Balance > 0)                    │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Adjust Advance?  : [☑ Yes]                                            │  │
│  │ Advance Applied  : ₹ 2,000 (Auto-applied to oldest invoice first)    │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  PAYMENT INFORMATION                                                         │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Payment Mode*    : [▼ Bank Transfer ▼]                                │  │
│  │ Bank Account*    : [▼ HDFC Current A/C - 1234 ▼]                     │  │
│  │ Ref / UTR / Chq# : [________________________]                        │  │
│  │ Cheque Date      : [📅 ________]          (Only if Mode = Cheque)     │  │
│  │ Amount Received* : ₹ [________]                                       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  INVOICE ALLOCATION (Adjust against pending invoices)                        │
│  ┌───┬────────────┬────────────┬────────────┬────────────┬───────────────┐  │
│  │ ☑ │Invoice #   │Inv. Date   │Total Amt   │Pending Amt │Allocate Amt   │  │
│  │───┼────────────┼────────────┼────────────┼────────────┼───────────────│  │
│  │ ☑ │INV-10024   │15 Mar 26   │₹ 15,000    │₹ 15,000    │₹ [15,000]    │  │
│  │ ☑ │INV-10022   │01 Mar 26   │₹ 12,000    │₹ 5,000     │₹ [5,000]     │  │
│  │ ☐ │INV-10018   │15 Feb 26   │₹ 8,000     │₹ 8,000     │₹ [      ]    │  │
│  └───┴────────────┴────────────┴────────────┴────────────┴───────────────┘  │
│  Note: Only invoices with status Pending / Partial / Overdue are shown.      │
│  If deep-linked from Mod 28.3, the source invoice is pre-checked.            │
│                                                                              │
│  ALLOCATION SUMMARY                                                          │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Advance Applied       : ₹ 2,000                                       │  │
│  │ Amount Received        : ₹ 25,000                                     │  │
│  │ Total Settled          : ₹ 27,000                                     │  │
│  │ Allocated to Invoices  : ₹ 20,000                                     │  │
│  │ ──────────────────────────────────────                                 │  │
│  │ Unallocated (Advance)  : ₹ 7,000                                     │  │
│  │ Carry to Advance?      : [☑ Yes]                                      │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  SETTLEMENT ACTION (Visible when allocated < pending of checked invoices)    │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Invoice INV-10024: Pending ₹15,000 → Allocated ₹13,000               │  │
│  │ Shortfall: ₹ 2,000                                                    │  │
│  │                                                                       │  │
│  │ What to do with shortfall?                                            │  │
│  │ (•) Keep Open — Invoice stays as "Partial"                            │  │
│  │ ( ) Settle & Close — Auto-generate Credit Note (CN) for ₹2,000       │  │
│  │     Reason: [▼ Payment Settlement ▼]                                  │  │
│  │     → Invoice INV-10024 will be marked PAID                           │  │
│  │     → CN-XXXXX will be created in Module 28.5                         │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ADDITIONAL                                                                  │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ TDS Deducted by Customer : ₹ [______]     (If customer deducted TDS) │  │
│  │ Notes                    : [_________________________________]        │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [SAVE & PRINT RECEIPT]    [SAVE]    [CANCEL]                                │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Screen Fields: Receipt Details

| Field           | Type    | Required | Description                                        |
| --------------- | ------- | -------- | -------------------------------------------------- |
| Receipt Date    | Date    | Yes      | Date of receipt (default: today)                   |
| Branch          | Dropdown| Yes      | Branch receiving the payment                       |
| Customer        | Search  | Yes      | Select customer (fetches pending invoices). **Locked if deep-linked from Mod 28.3** |
| Current Balance | Display | Auto     | Total outstanding from Customer Ledger (Module 31) |
| Advance Balance | Display | Auto     | Existing "On Account" advance from Customer Ledger |

---

## Screen Fields: Advance Adjustment

| Field           | Type    | Required | Description                                        |
| --------------- | ------- | -------- | -------------------------------------------------- |
| Adjust Advance? | Toggle  | No       | If ON, applies existing advance to oldest invoice first (FIFO) |
| Advance Applied | Display | Auto     | Amount of advance being applied in this transaction |

> **Visibility:** This section is visible ONLY when Customer has Advance Balance > 0.

---

## Screen Fields: Payment Information

| Field          | Type     | Required | Description                                       |
| -------------- | -------- | -------- | ------------------------------------------------- |
| Payment Mode   | Dropdown | Yes      | Cash / Bank Transfer / UPI / Cheque / Card        |
| Bank Account   | Dropdown | Cond.    | Required if Mode ≠ Cash. From Module 32           |
| Ref / UTR      | Text     | Cond.    | Required if Mode = Bank/UPI. Transaction reference|
| Cheque Date    | Date     | Cond.    | Required if Mode = Cheque                         |
| Amount Received| Number   | Yes      | Total amount received from customer               |

---

## Screen Fields: Invoice Allocation Grid

| Field        | Type     | Required | Description                                       |
| ------------ | -------- | -------- | ------------------------------------------------- |
| Select       | Checkbox | No       | Select invoices to allocate against               |
| Invoice #    | Display  | Auto     | Invoice number from Module 28 (clickable link)    |
| Invoice Date | Display  | Auto     | Date of original invoice                          |
| Total Amount | Display  | Auto     | Original invoice amount                           |
| Pending Amt  | Display  | Auto     | Remaining unpaid amount                           |
| Allocate Amt | Number   | Cond.    | Amount to allocate (editable, cannot exceed pending)|

> **Grid Filter:** Only invoices with status `Pending`, `Partial`, or `Overdue` are listed. Sorted oldest-first (FIFO default).

> **Deep-Link Behavior:** If opened from Module 28.3 `[+ RECORD PAYMENT]`, the source invoice row is pre-checked with `Allocate Amt` pre-filled to its full `Pending Amt`.

---

## Screen Fields: Settlement Action

| Field             | Type     | Required | Description                                       |
| ----------------- | -------- | -------- | ------------------------------------------------- |
| Shortfall Amount  | Display  | Auto     | Per-invoice: Pending Amt − Allocate Amt           |
| Settlement Choice | Radio    | Cond.    | **Keep Open** (default) / **Settle & Close**      |
| Reason            | Dropdown | Cond.    | Required if "Settle & Close". Options: Payment Settlement / Pricing Error / Service Issue / Other |

> **Visibility:** This section appears ONLY when at least one checked invoice has `Allocate Amt < Pending Amt`. It shows a row for each such invoice.

> **Settle & Close Effect:** On Save, the system auto-generates a Credit Note (CN-XXXXX) in Module 28.5 for the shortfall amount with `Reason = Payment Settlement` and links it to this receipt voucher.

---

## Screen Fields: Additional

| Field               | Type   | Required | Description                                    |
| ------------------- | ------ | -------- | ---------------------------------------------- |
| TDS Deducted        | Number | No       | If customer deducted TDS before paying         |
| Carry to Advance    | Toggle | Auto     | If unallocated amount exists, carry as advance |
| Notes               | Text   | No       | Remarks for internal reference                 |

---

## Validation Rules

| Field           | Rule                                                          |
| --------------- | ------------------------------------------------------------- |
| Receipt Date    | Cannot be a future date                                       |
| Customer        | Must exist in Module 18 (Customer Master)                     |
| Payment Mode    | Must select one option                                        |
| Bank Account    | Required if Mode is Bank/UPI/Cheque/Card (from Module 32)     |
| Ref / UTR       | Required for Bank/UPI modes. Must be unique across all vouchers|
| Cheque Date     | Required if Mode = Cheque. Cannot be older than 3 months      |
| Amount Received | Must be greater than 0                                        |
| Allocate Amount | Cannot exceed Pending Amount of individual invoice             |
| Total Allocation| Sum of all allocations cannot exceed (Amount Received + Advance Applied) |
| Settle & Close  | Reason is required when "Settle & Close" is selected          |

---

## Business Rules

| Rule                            | Description                                                  |
| ------------------------------- | ------------------------------------------------------------ |
| FIFO Allocation default         | System auto-suggests allocation to oldest invoice first      |
| Partial allocation allowed      | An invoice can be partially paid across multiple receipts    |
| Advance handling                | Unallocated amount saved as "On Account" in Customer Ledger  |
| Advance adjustment              | If customer has existing advance, it can be auto-applied before cash allocation |
| Cheque clearance                | Cheque receipts marked "Subject to Clearance" until confirmed|
| TDS by customer                 | If customer deducted TDS, it is recorded separately          |
| Settle & Close auto-CN          | When selected, auto-generates Credit Note (CN) in Module 28.5 for shortfall amount |
| Deep-link from Mod 28           | Customer & Invoice pre-selected when redirected from Mod 28.3 `[+ RECORD PAYMENT]` |

---

## System Behavior

| Event                        | System Action                                                |
| ---------------------------- | ------------------------------------------------------------ |
| Customer selected            | Fetches all pending invoices (Pending/Partial/Overdue) and current balance from Module 31 |
| Customer has advance         | Shows Advance Adjustment section with existing advance amount |
| Amount entered               | Auto-suggests FIFO allocation across checked invoices        |
| Allocation < Pending         | Settlement Action section appears for each shortfall invoice |
| "Settle & Close" selected    | On Save: Auto-generates CN-XXXXX in Module 28.5 with `Reason = Payment Settlement` and `Amount = Shortfall` |
| Save clicked                 | Voucher RCP-XXXX created                                     |
| **Ledger posting**           | **Dr** Bank/Cash Account (Module 32) → **Cr** Customer A/C (Sundry Debtor) |
| Full allocation on invoice   | Invoice status → **Paid** (Module 28)                        |
| Partial allocation           | Invoice status → **Partial** (Module 28), pending reduced    |
| Settle & Close on invoice    | Invoice status → **Paid** (via CN auto-adjustment)           |
| Advance (unallocated)        | Amount recorded as "On Account" in Customer Ledger (Module 31)|
| **Mod 28.3 Ledger entry**    | This receipt appears as a new row in the invoice's Transaction Ledger: `Type = Payment, Ref = RCP-XXXX, Credit Amount = allocated, Running Bal = reduced` |

---

================================================================================

# 30.3 Payment Entry (Money OUT — to Vendor)

**Description:**
Form to record money paid to a vendor/supplier. The amount is allocated against one or more pending bills from Module 29. Handles TDS deduction at source. If the amount paid is less than total pending, user can **Settle & Close** (auto-generates Debit Note via Module 29.5) or **Keep Open** (bill remains Partial). Layout mirrors the Receipt Entry (30.2) but with vendor-specific fields.

**Deep-Link:** When opened via `[+ MAKE PAYMENT]` from Module 29.3, the `Vendor` and target `Bill` are pre-selected and the bill is auto-checked in the allocation grid. The vendor field is locked (read-only).

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        PAYMENT ENTRY (MONEY OUT)                             │
│                                                                              │
│  PAYMENT DETAILS                                                             │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Payment Date*    : [📅 28 Mar 2026]        (Default: Today)           │  │
│  │ Branch*          : [▼ Mumbai ▼]                                       │  │
│  │ Vendor*          : [🔍 Search Vendor ▼]   (🔒 if deep-linked)        │  │
│  │ Current Balance  : ₹ 1,05,300 (We owe vendor)    [DISPLAY]           │  │
│  │ Advance Balance  : ₹ 0 (Advance paid to vendor)  [DISPLAY]           │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ADVANCE ADJUSTMENT (Visible only if Advance Balance > 0)                    │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Adjust Advance?  : [☑ Yes]                                            │  │
│  │ Advance Applied  : ₹ 0                                                │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  PAYMENT INFORMATION                                                         │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Payment Mode*    : [▼ Bank Transfer (NEFT) ▼]                         │  │
│  │ Bank Account*    : [▼ HDFC Current A/C - 1234 ▼]                     │  │
│  │ Ref / UTR / Chq# : [________________________]                        │  │
│  │ Cheque Date      : [📅 ________]          (Only if Mode = Cheque)     │  │
│  │ Amount Paid*     : ₹ [________]                                       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  BILL ALLOCATION (Adjust against pending bills)                              │
│  ┌───┬────────────┬────────────┬────────────┬────────────┬───────────────┐  │
│  │ ☑ │Bill #      │Bill Date   │Total Amt   │Pending Amt │Allocate Amt   │  │
│  │───┼────────────┼────────────┼────────────┼────────────┼───────────────│  │
│  │ ☑ │BILL-5524   │10 Mar 26   │₹ 1,05,300  │₹ 1,05,300  │₹ [85,000]   │  │
│  │ ☐ │BILL-5520   │01 Mar 26   │₹ 25,000    │₹ 25,000    │₹ [      ]   │  │
│  └───┴────────────┴────────────┴────────────┴────────────┴───────────────┘  │
│  Note: Only bills with status Pending / Partial / Overdue are shown.         │
│  If deep-linked from Mod 29.3, the source bill is pre-checked.              │
│                                                                              │
│  TDS DEDUCTION                                                               │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ TDS Applicable    : Yes — Section 194C (from Vendor Master Mod 14)    │  │
│  │ TDS Rate          : 1%                                                │  │
│  │ TDS Amount        : ₹ 850     (Auto-calculated on taxable amount)     │  │
│  │ Net Payable       : ₹ 84,150                                         │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ALLOCATION SUMMARY                                                          │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Advance Applied       : ₹ 0                                           │  │
│  │ Amount Paid           : ₹ 85,000                                      │  │
│  │ TDS Deducted          : ₹ 850                                         │  │
│  │ Total Settled          : ₹ 85,850                                     │  │
│  │ Allocated to Bills    : ₹ 85,850                                      │  │
│  │ ──────────────────────────────────────                                 │  │
│  │ Unallocated (Advance) : ₹ 0                                          │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  SETTLEMENT ACTION (Visible when allocated < pending of checked bills)       │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Bill BILL-5524: Pending ₹1,05,300 → Allocated ₹85,850                │  │
│  │ Shortfall: ₹ 19,450                                                   │  │
│  │                                                                       │  │
│  │ What to do with shortfall?                                            │  │
│  │ (•) Keep Open — Bill stays as "Partial"                               │  │
│  │ ( ) Settle & Close — Auto-generate Debit Note (DN) for ₹19,450       │  │
│  │     Reason: [▼ Payment Settlement ▼]                                  │  │
│  │     → Bill BILL-5524 will be marked PAID                              │  │
│  │     → DN-XXXXX will be created in Module 29.5                         │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  Notes: [_____________________________________________]                      │
│                                                                              │
│  [SAVE & PRINT VOUCHER]    [SAVE]    [CANCEL]                                │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Screen Fields: Payment Details

| Field          | Type     | Required | Description                                       |
| -------------- | -------- | -------- | ------------------------------------------------- |
| Payment Date   | Date     | Yes      | Date of payment (default: today)                  |
| Branch         | Dropdown | Yes      | Branch making the payment                         |
| Vendor         | Search   | Yes      | Select vendor (fetches pending bills). **Locked if deep-linked from Mod 29.3** |
| Current Balance| Display  | Auto     | Total outstanding from Vendor Ledger (Module 31)  |
| Advance Balance| Display  | Auto     | Existing "On Account" advance from Vendor Ledger  |

---

## Screen Fields: Payment Information

| Field        | Type     | Required | Description                                       |
| ------------ | -------- | -------- | ------------------------------------------------- |
| Payment Mode | Dropdown | Yes      | Cash / Bank Transfer (NEFT/RTGS) / UPI / Cheque   |
| Bank Account | Dropdown | Cond.    | Required if Mode ≠ Cash. From Module 32           |
| Ref / UTR    | Text     | Cond.    | Required if Mode = Bank/UPI                       |
| Cheque Date  | Date     | Cond.    | Required if Mode = Cheque                         |
| Amount Paid  | Number   | Yes      | Total amount being paid to vendor                 |

---

## Screen Fields: TDS Deduction

| Field         | Type    | Required | Description                                        |
| ------------- | ------- | -------- | -------------------------------------------------- |
| TDS Applicable| Display | Auto     | Fetched from Vendor Master (Module 14)             |
| TDS Rate      | Display | Auto     | Based on TDS section (194C = 1%, 194J = 10%, etc.) |
| TDS Amount    | Number  | Auto     | Auto-calculated on taxable amount                  |
| Net Payable   | Number  | Auto     | Amount Paid − TDS Deducted                         |

---

## Screen Fields: Settlement Action

| Field             | Type     | Required | Description                                       |
| ----------------- | -------- | -------- | ------------------------------------------------- |
| Shortfall Amount  | Display  | Auto     | Per-bill: Pending Amt − Allocate Amt              |
| Settlement Choice | Radio    | Cond.    | **Keep Open** (default) / **Settle & Close**      |
| Reason            | Dropdown | Cond.    | Required if "Settle & Close". Options: Payment Settlement / Purchase Return / Discount / Error / Other |

> **Visibility:** This section appears ONLY when at least one checked bill has `Allocate Amt < Pending Amt`.

> **Settle & Close Effect:** On Save, the system auto-generates a Debit Note (DN-XXXXX) in Module 29.5 for the shortfall amount with `Reason = Payment Settlement` and links it to this payment voucher.

---

## Validation Rules

| Field           | Rule                                                          |
| --------------- | ------------------------------------------------------------- |
| Payment Date    | Cannot be a future date                                       |
| Vendor          | Must exist in Module 14 (Vendor Master)                       |
| Payment Mode    | Must select one option                                        |
| Bank Account    | Required for non-cash modes (from Module 32)                  |
| Ref / UTR       | Required for Bank/UPI. Must be unique across all vouchers     |
| Amount Paid     | Must be greater than 0                                        |
| Allocate Amount | Cannot exceed Pending Amount of individual bill               |
| Total Allocation| Cannot exceed (Amount Paid + Advance Applied − TDS)           |
| Settle & Close  | Reason is required when selected                              |
| TDS             | Auto-calculated; cannot be manually overridden unless Manager role |

---

## Business Rules

| Rule                            | Description                                                  |
| ------------------------------- | ------------------------------------------------------------ |
| FIFO Allocation default         | System auto-suggests allocation to oldest bill first         |
| Partial allocation allowed      | A bill can be partially paid across multiple payments        |
| TDS auto-deduction              | Based on Vendor Master (Mod 14) TDS section config           |
| Settle & Close auto-DN          | When selected, auto-generates Debit Note (DN) in Mod 29.5   |
| Deep-link from Mod 29           | Vendor & Bill pre-selected from Mod 29.3 `[+ MAKE PAYMENT]` |
| Advance handling                | Unallocated excess saved as "On Account" in Vendor Ledger    |
| Advance adjustment              | If vendor has existing advance, it can be applied before cash|

---

## System Behavior

| Event                      | System Action                                                |
| -------------------------- | ------------------------------------------------------------ |
| Vendor selected            | Fetches all pending bills (Pending/Partial/Overdue) and current balance from Module 31 |
| Vendor has advance         | Shows Advance Adjustment section                             |
| TDS auto-calculated        | Based on vendor TDS section and rate (Module 14)             |
| Allocation < Pending       | Settlement Action section appears for each shortfall bill    |
| "Settle & Close" selected  | On Save: Auto-generates DN-XXXXX in Module 29.5             |
| Save clicked               | Voucher PAY-XXXX created                                     |
| **Ledger posting**         | **Dr** Vendor A/C (Sundry Creditor) → **Cr** Bank/Cash Account (Module 32) |
| **If TDS deducted**        | Additional: **Dr** Vendor A/C → **Cr** TDS Payable A/C      |
| Full allocation on bill    | Bill status → **Paid** (Module 29)                           |
| Partial allocation         | Bill status → **Partial** (Module 29), pending reduced       |
| Settle & Close on bill     | Bill status → **Paid** (via DN auto-adjustment)              |
| **Mod 29.3 Ledger entry**  | This payment appears as a row in the bill's Transaction Ledger: `Type = Payment, Ref = PAY-XXXX, Debit Amount = allocated, Running Bal = reduced` |

---

================================================================================

# 30.4 Contra Entry (Inter-Bank Transfer)

**Description:**
Records transfer of funds between the company's own bank accounts or between cash and bank. No external party is involved. Used for Cash Deposit to Bank, Bank Withdrawal, and Inter-bank Fund Transfers.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          CONTRA ENTRY                                        │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Date*           : [📅 28 Mar 2026]                                    │  │
│  │ Branch*         : [▼ Mumbai ▼]                                        │  │
│  │                                                                       │  │
│  │ Transfer From*  : [▼ Cash-in-Hand ▼]                                  │  │
│  │ Transfer To*    : [▼ HDFC Current A/C - 1234 ▼]                      │  │
│  │ Amount*         : ₹ [________]                                        │  │
│  │ Reference       : [________________________]                          │  │
│  │ Notes           : [________________________]                          │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [SAVE]    [CANCEL]                                                          │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Screen Fields

| Field         | Type     | Required | Description                                       |
| ------------- | -------- | -------- | ------------------------------------------------- |
| Date          | Date     | Yes      | Date of transfer (default: today)                 |
| Branch        | Dropdown | Yes      | Branch performing the transfer                    |
| Transfer From | Dropdown | Yes      | Source: Cash / Bank Account (from Module 32)      |
| Transfer To   | Dropdown | Yes      | Destination: Cash / Bank Account (from Module 32) |
| Amount        | Number   | Yes      | Transfer amount                                   |
| Reference     | Text     | No       | Internal reference or bank slip number            |
| Notes         | Text     | No       | Additional remarks                                |

---

## Validation Rules

| Field         | Rule                                                       |
| ------------- | ---------------------------------------------------------- |
| Transfer From | Cannot be same as Transfer To                              |
| Amount        | Must be greater than 0, cannot exceed source balance       |
| Date          | Cannot be a future date                                    |

---

## System Behavior

| Event              | System Action                                              |
| ------------------ | ---------------------------------------------------------- |
| Save clicked       | Voucher CNT-XXX created                                    |
| **Ledger posting** | **Dr** Destination Account → **Cr** Source Account         |
| Audit log          | Entry logged with user, timestamp, and both accounts       |

---

================================================================================

# 30.5 Journal Entry (Non-Cash Adjustments)

**Description:**
Used for internal adjustments that do not involve actual cash movement. Examples: TDS adjustments, depreciation entries, interest accrual, salary accrual, GST set-off, and write-offs. Follows standard double-entry: **Total Debits = Total Credits**.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          JOURNAL ENTRY                                       │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Date*         : [📅 28 Mar 2026]                                      │  │
│  │ Branch*       : [▼ Mumbai ▼]                                          │  │
│  │ Narration*    : [_____________________________________________]       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  JOURNAL LINES                                                               │
│  ┌───┬──────────────────────────┬──────────────┬──────────────┐             │
│  │Sr │Account Head              │Debit (Dr)    │Credit (Cr)   │             │
│  │───┼──────────────────────────┼──────────────┼──────────────│             │
│  │ 1 │TDS Receivable (194C)     │₹ 900         │—             │             │
│  │ 2 │Sundry Creditors - Vendor │—             │₹ 900         │             │
│  └───┴──────────────────────────┴──────────────┴──────────────┘             │
│  [+ ADD LINE]    [🗑 REMOVE SELECTED]                                       │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Total Debit  : ₹ 900       Total Credit : ₹ 900       ✅ Balanced    │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [SAVE]    [CANCEL]                                                          │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Screen Fields

| Field        | Type     | Required | Description                                        |
| ------------ | -------- | -------- | -------------------------------------------------- |
| Date         | Date     | Yes      | Journal date                                       |
| Branch       | Dropdown | Yes      | Branch for the entry                               |
| Narration    | Textarea | Yes      | Description/reason for the journal entry           |
| Account Head | Search   | Yes      | Account from Chart of Accounts (Module 32)         |
| Debit (Dr)   | Number   | Cond.    | Debit amount (mutually exclusive with Credit)      |
| Credit (Cr)  | Number   | Cond.    | Credit amount (mutually exclusive with Debit)      |

---

## Validation Rules

| Field        | Rule                                                         |
| ------------ | ------------------------------------------------------------ |
| Date         | Cannot be a future date                                      |
| Narration    | Cannot be empty                                              |
| Lines        | Minimum 2 lines required                                     |
| Balance      | Total Debits MUST equal Total Credits                        |
| Account Head | Must exist in Module 32                                      |
| Each line    | Must have either Debit OR Credit, not both                   |

---

## System Behavior

| Event                 | System Action                                           |
| --------------------- | ------------------------------------------------------- |
| Line added            | Dr/Cr balance auto-recalculated                         |
| Imbalance detected    | Save button disabled; "Unbalanced" warning shown        |
| Save clicked          | JRN-XXX number generated                                |
| **Ledger posting**    | Each line creates entry in the respective ledger account|

---

================================================================================

# 30.6 View Voucher Detail (Read-only)

**Description:**
Read-only view of any voucher (Receipt / Payment / Contra / Journal). Shows the full transaction details, linked invoices/bills, settlement notes (auto-generated CN/DN), double-entry ledger posting, and audit trail.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       VOUCHER DETAIL — RCP-3001                              │
│                                                                              │
│  Voucher Type : 🟢 Receipt (IN)       Date: 25 Mar 2026                     │
│  Party        : ABC Corp Ltd          Mode: Bank Transfer (UPI)              │
│  Amount       : ₹ 15,000              Ref : UTR123456                        │
│                                                                              │
│  INVOICE ALLOCATION                                                          │
│  ┌────────────┬──────────────┬──────────────┬──────────────┐                │
│  │Invoice #   │Total Amt     │Allocated Amt │Status After  │                │
│  │────────────┼──────────────┼──────────────┼──────────────│                │
│  │INV-10024   │₹ 15,000      │₹ 13,000      │Partial       │                │
│  │INV-10022   │₹ 12,000      │₹ 2,000       │Partial       │                │
│  └────────────┴──────────────┴──────────────┴──────────────┘                │
│                                                                              │
│  LINKED SETTLEMENT NOTE (If auto-generated)                                  │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ ⚠️ No settlement note generated (Kept Open)                          │  │
│  │ OR                                                                    │  │
│  │ ✅ Credit Note CN-5001 auto-generated for ₹2,000 (Payment Settlement)│  │
│  │    → INV-10024 status changed to PAID                                 │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  LEDGER POSTING (Double Entry)                                               │
│  ┌──────────────────────────────┬──────────────┬──────────────┐             │
│  │Account Head                  │Debit (Dr)    │Credit (Cr)   │             │
│  │──────────────────────────────┼──────────────┼──────────────│             │
│  │HDFC Current A/C (Bank)       │₹ 15,000      │—             │             │
│  │ABC Corp Ltd (Sundry Debtor)  │—             │₹ 15,000      │             │
│  └──────────────────────────────┴──────────────┴──────────────┘             │
│                                                                              │
│  TDS DETAILS (If applicable)                                                 │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ TDS Section: —    TDS Amount: ₹ 0    Net Amount: ₹ 15,000            │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  Notes: Payment against March invoices                                       │
│                                                                              │
│  AUDIT LOG                                                                   │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │ 25 Mar 2026 14:30 — Created by Priya Patel                          │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  [📥 DOWNLOAD PDF]  [🖨 PRINT]  [🔙 BACK TO REGISTER]                     │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## View Fields

| Field                | Type    | Description                                              |
| -------------------- | ------- | -------------------------------------------------------- |
| Voucher #            | Text    | System-generated number                                  |
| Type                 | Badge   | Receipt / Payment / Contra / Journal                     |
| Date                 | Date    | Transaction date                                         |
| Party                | Text    | Customer / Vendor (or Self / Internal)                   |
| Amount               | Number  | Transaction amount                                       |
| Mode                 | Text    | Cash / Bank / UPI / Cheque / Card                        |
| Ref / UTR            | Text    | Transaction reference number                             |
| Allocation Table     | Table   | Invoices/Bills adjusted with amounts & resulting status  |
| Settlement Note      | Display | Linked CN/DN number if auto-generated, with amount       |
| Ledger Posting       | Table   | Double-entry showing Dr/Cr accounts and amounts          |
| TDS Details          | Summary | If applicable, TDS section and amount                    |
| Notes                | Text    | Remarks                                                  |
| Audit Log            | List    | Created by, Date, Time                                   |

---

## Actions (View Screen)

| Action              | Type   | Description                                      |
| ------------------- | ------ | ------------------------------------------------ |
| **Download PDF**    | Button | Download formatted voucher PDF                   |
| **Print**           | Button | Print receipt or voucher                         |
| **Back to Register**| Button | Returns to Payment Register (30.1)              |

---

================================================================================

# Ledger Integration — Double-Entry Posting Reference

Every transaction in Module 30 creates an automatic double-entry posting in Module 31 (Ledger). Below is the complete posting reference:

## Receipt (30.2) — Money IN from Customer

| Debit Account (Dr)              | Credit Account (Cr)             | Trigger                       |
| ------------------------------- | ------------------------------- | ----------------------------- |
| Bank / Cash Account (Mod 32)    | Customer A/C — Sundry Debtor    | Receipt saved                 |
| TDS Receivable (if applicable)  | Customer A/C — Sundry Debtor    | Customer deducted TDS         |

**Effect on Module 28:** The allocated amount appears as a new row in Invoice's Transaction Ledger (28.3) → `Type = Payment, Ref = RCP-XXXX, Credit Amount = X, Running Balance = reduced`. Invoice status updates to `Partial` or `Paid`.

**If Settle & Close:** Auto-generated CN appears as additional row → `Type = Adjustment, Ref = CN-XXXXX`.

---

## Payment (30.3) — Money OUT to Vendor

| Debit Account (Dr)              | Credit Account (Cr)             | Trigger                       |
| ------------------------------- | ------------------------------- | ----------------------------- |
| Vendor A/C — Sundry Creditor    | Bank / Cash Account (Mod 32)    | Payment saved                 |
| Vendor A/C — Sundry Creditor    | TDS Payable (if applicable)     | TDS deducted at source        |

**Effect on Module 29:** The allocated amount appears as a new row in Bill's Transaction Ledger (29.3) → `Type = Payment, Ref = PAY-XXXX, Debit Amount = X, Running Balance = reduced`. Bill status updates to `Partial` or `Paid`.

**If Settle & Close:** Auto-generated DN appears as additional row → `Type = Debit Note, Ref = DN-XXXXX`.

---

## Contra (30.4) — Internal Transfer

| Debit Account (Dr)              | Credit Account (Cr)             |
| ------------------------------- | ------------------------------- |
| Destination Bank/Cash (Mod 32)  | Source Bank/Cash (Mod 32)       |

---

## Journal (30.5) — Non-Cash Adjustment

| Debit Account (Dr)              | Credit Account (Cr)             |
| ------------------------------- | ------------------------------- |
| As entered per journal line     | As entered per journal line     |

> **Rule:** Total Debit = Total Credit (always balanced)

---

================================================================================

# Status Flow (Module 30 & Impact on Module 28 / 29)

```
  ══════════════════════════════════════════════════════════════
  RECEIPT FLOW (30.2):
  ══════════════════════════════════════════════════════════════

  Customer Pays
       ↓
  Receipt Created (RCP-XXXX)
       ↓
  ┌────────────────────────────────────────────────────────┐
  │ Allocated to Invoice(s)?                                │
  │                                                        │
  │ YES (Full)    → Invoice status → PAID                  │
  │ YES (Partial) → Invoice status → PARTIAL               │
  │                 ┌──────────────────────────────────┐   │
  │                 │ Settlement Action?                │   │
  │                 │ • Keep Open → stays PARTIAL       │   │
  │                 │ • Settle & Close →                │   │
  │                 │   Auto-CN (28.5) → Invoice → PAID│   │
  │                 └──────────────────────────────────┘   │
  │ NO (Excess)   → On Account (Advance) in Ledger        │
  └────────────────────────────────────────────────────────┘
       ↓
  Ledger Updated: Dr Bank → Cr Customer

  ══════════════════════════════════════════════════════════════
  PAYMENT FLOW (30.3):
  ══════════════════════════════════════════════════════════════

  We Pay Vendor
       ↓
  Payment Created (PAY-XXXX)
       ↓
  ┌────────────────────────────────────────────────────────┐
  │ Allocated to Bill(s)?                                   │
  │                                                        │
  │ YES (Full)    → Bill status → PAID                     │
  │ YES (Partial) → Bill status → PARTIAL                  │
  │                 ┌──────────────────────────────────┐   │
  │                 │ Settlement Action?                │   │
  │                 │ • Keep Open → stays PARTIAL       │   │
  │                 │ • Settle & Close →                │   │
  │                 │   Auto-DN (29.5) → Bill → PAID   │   │
  │                 └──────────────────────────────────┘   │
  │ NO (Excess)   → On Account (Advance) in Vendor Ledger  │
  └────────────────────────────────────────────────────────┘
       ↓
  Ledger Updated: Dr Vendor → Cr Bank
  (+ Dr Vendor → Cr TDS Payable if TDS applicable)

  ══════════════════════════════════════════════════════════════
  CONTRA FLOW (30.4):
  ══════════════════════════════════════════════════════════════

  Cash → Bank / Bank → Cash / Bank → Bank
       ↓
  Both accounts updated: Dr Destination → Cr Source

  ══════════════════════════════════════════════════════════════
  JOURNAL FLOW (30.5):
  ══════════════════════════════════════════════════════════════

  Debit Account(s) + Credit Account(s)
       ↓
  All Ledgers updated
  Rule: Total Dr = Total Cr (Always balanced)
```

---

================================================================================

# Module 28 to Module 30 Data Flow (End-to-End)

This section details the exact data flow and system behavior when moving from an Invoice (Module 28) to a Payment Receipt (Module 30), covering all possible payment scenarios.

## Scenario 1: Full Payment (Exact Match)

**1. Module 28 (Invoicing)**
- Invoice `INV-100` created for **₹10,000**. Status: `PENDING`.
- User clicks `[+ RECORD PAYMENT]` on INV-100 in Screen 28.3.

**2. Data Passed to Module 30 (Receipt Entry 30.2)**
- `Customer`: Auto-filled and locked.
- `Allocation Grid`: `INV-100` row is auto-checked. 
- `Allocate Amt`: Auto-filled with `₹10,000`.

**3. Action in Module 30**
- User enters `Amount Received`: **₹10,000**.
- `Settlement Action`: Hidden (since Allocated = Pending).
- User clicks `[SAVE]`. Voucher `RCP-101` created.

**4. System Updates & Data Flow Back**
- **Ledger (Mod 31):** Dr Bank ₹10k → Cr Customer ₹10k.
- **Module 28:** 
  - `INV-100` Pending Amount becomes **₹0**.
  - `INV-100` Status changes to `PAID`.
  - Transaction Ledger on `INV-100` gets new row: `[Payment | RCP-101 | Cr ₹10,000]`.

---

## Scenario 2: Partial Payment (Keep Open)

**1. Module 28 (Invoicing)**
- Invoice `INV-101` created for **₹10,000**. Status: `PENDING`.
- User clicks `[+ RECORD PAYMENT]`.

**2. Data Passed to Module 30**
- `Customer` locked, `INV-101` auto-checked, `Allocate Amt` pre-filled ₹10,000.

**3. Action in Module 30**
- User realizes customer only sent ₹6,000.
- User enters `Amount Received`: **₹6,000**.
- User manually changes `Allocate Amt` for INV-101 to **₹6,000**.
- `Settlement Action` appears because Allocate (₹6k) < Pending (₹10k). Shortfall = ₹4,000.
- User selects **Keep Open**.
- User clicks `[SAVE]`. Voucher `RCP-102` created.

**4. System Updates & Data Flow Back**
- **Ledger (Mod 31):** Dr Bank ₹6k → Cr Customer ₹6k.
- **Module 28:** 
  - `INV-101` Pending Amount reduces to **₹4,000**.
  - `INV-101` Status changes to `PARTIAL`.
  - Transaction Ledger on `INV-101` gets new row: `[Payment | RCP-102 | Cr ₹6,000]`.

---

## Scenario 3: Shortfall Payment (Settle & Close)

**1. Module 28 (Invoicing)**
- Invoice `INV-102` created for **₹10,500**. Status: `PENDING`.
- User clicks `[+ RECORD PAYMENT]`.

**2. Data Passed to Module 30**
- `Customer` locked, `INV-102` auto-checked, `Allocate Amt` pre-filled ₹10,500.

**3. Action in Module 30**
- Customer paid ₹10,000 flat, deducting ₹500 for a minor dispute or discount.
- User enters `Amount Received`: **₹10,000**.
- User changes `Allocate Amt` to **₹10,000**.
- `Settlement Action` appears. Shortfall = ₹500.
- User selects **Settle & Close**. Selects Reason: `Payment Settlement`.
- User clicks `[SAVE]`. Voucher `RCP-103` created.

**4. System Updates & Data Flow Back**
- **Module 28.5 (Credit Note):** 
  - System auto-generates `CN-001` for **₹500** against INV-102.
- **Ledger (Mod 31):** 
  - From Receipt: Dr Bank ₹10k → Cr Customer ₹10k.
  - From Auto-CN: Dr Discount/Adjustment A/C ₹500 → Cr Customer ₹500.
- **Module 28 (Invoicing):** 
  - `INV-102` Pending Amount reduces to ₹500 (via payment), then to **₹0** (via CN).
  - `INV-102` Status changes to `PAID`.
  - Transaction Ledger on `INV-102` gets TWO rows:
    - 1. `[Payment | RCP-103 | Cr ₹10,000]`
    - 2. `[Adjustment| CN-001 | Cr ₹500]`

---

## Scenario 4: Overpayment / Advance (Carry Forward)

**1. Module 28 (Invoicing)**
- Invoice `INV-103` created for **₹10,000**. Status: `PENDING`.
- User clicks `[+ RECORD PAYMENT]`.

**2. Data Passed to Module 30**
- `Customer` locked, `INV-103` checked, `Allocate Amt` pre-filled ₹10,000.

**3. Action in Module 30**
- Customer accidentally transferred ₹12,000.
- User enters `Amount Received`: **₹12,000**.
- `Allocate Amt` remains ₹10,000 (cannot exceed pending).
- `Allocation Summary` shows: Allocated = ₹10,000, Unallocated = ₹2,000.
- `Carry to Advance?` is checked `[ON]`.
- User clicks `[SAVE]`. Voucher `RCP-104` created.

**4. System Updates & Data Flow Back**
- **Ledger (Mod 31):** Dr Bank ₹12k → Cr Customer ₹12k. (Customer ledger now shows ₹2,000 credit balance).
- **Module 28:** 
  - `INV-103` Pending Amount becomes **₹0**. Status changes to `PAID`.
  - Transaction Ledger on `INV-103` gets new row: `[Payment | RCP-104 | Cr ₹10,000]`.
- **Future Flow:** When the next invoice (`INV-104`) is generated and user clicks `[+ RECORD PAYMENT]`, the `Advance Adjustment` section in Module 30 will automatically appear, showing the ₹2,000 balance available to apply before asking for a new bank transfer amount.

---

## Scenario 5: Adjusting Existing Advance

**1. Module 28 (Invoicing)**
- Invoice `INV-104` created for **₹8,000**. Status: `PENDING`.
- Customer has an existing advance balance of **₹2,000** (from Scenario 4).
- User clicks `[+ RECORD PAYMENT]`.

**2. Data Passed to Module 30**
- `Customer` locked, `INV-104` checked.
- `Advance Balance` shows **₹2,000**.
- `Adjust Advance?` toggle is `[ON]`.
- `Advance Applied` shows **₹2,000**.
- `Allocate Amt` pre-filled with remaining ₹6,000 (after advance).

**3. Action in Module 30**
- User enters `Amount Received`: **₹6,000** (the actual new bank transfer).
- `Allocation Summary` shows: Advance Applied = ₹2,000, Amount Received = ₹6,000, Total Settled = ₹8,000.
- User clicks `[SAVE]`. Voucher `RCP-105` created.

**4. System Updates & Data Flow Back**
- **Ledger (Mod 31):** Dr Bank ₹6k → Cr Customer ₹6k. (Customer advance balance reduces from ₹2k to ₹0).
- **Module 28:** 
  - `INV-104` Pending Amount becomes **₹0**, Status changes to `PAID`.
  - Transaction Ledger on `INV-104` gets TWO rows: 
    - `[Advance Adj | RCP-105 | Cr ₹2,000]`
    - `[Payment     | RCP-105 | Cr ₹6,000]`

---

================================================================================

# Module 29 to Module 30 Data Flow (End-to-End)

This section details the exact data flow and system behavior when moving from a Purchase Bill (Module 29) to a Payment Entry (Module 30), covering all possible payment scenarios.

## Scenario 1: Full Payment (Exact Match)

**1. Module 29 (Bills)**
- Bill `BILL-200` created for **₹50,000**. Status: `PENDING`.
- User clicks `[+ MAKE PAYMENT]` on BILL-200 in Screen 29.3.

**2. Data Passed to Module 30 (Payment Entry 30.3)**
- `Vendor`: Auto-filled and locked.
- `Allocation Grid`: `BILL-200` row is auto-checked. 
- `Allocate Amt`: Auto-filled with `₹50,000`.

**3. Action in Module 30**
- `TDS Deduction`: Checked. Assumes 0% for this example.
- User enters `Amount Paid`: **₹50,000**.
- `Settlement Action`: Hidden.
- User clicks `[SAVE]`. Voucher `PAY-201` created.

**4. System Updates & Data Flow Back**
- **Ledger (Mod 31):** Dr Vendor ₹50k → Cr Bank ₹50k.
- **Module 29:** 
  - `BILL-200` Pending Amount becomes **₹0**.
  - `BILL-200` Status changes to `PAID`.
  - Transaction Ledger on `BILL-200` gets new row: `[Payment | PAY-201 | Dr ₹50,000]`.

---

## Scenario 2: Partial Payment (Keep Open)

**1. Module 29 (Bills)**
- Bill `BILL-201` created for **₹50,000**. Status: `PENDING`.
- User clicks `[+ MAKE PAYMENT]`.

**2. Data Passed to Module 30**
- `Vendor` locked, `BILL-201` checked, `Allocate Amt` pre-filled ₹50,000.

**3. Action in Module 30**
- User decides to pay only ₹20,000 now.
- User enters `Amount Paid`: **₹20,000**.
- User manually changes `Allocate Amt` to **₹20,000**.
- `Settlement Action` appears because Allocate (₹20k) < Pending (₹50k). Shortfall = ₹30,000.
- User selects **Keep Open**.
- User clicks `[SAVE]`. Voucher `PAY-202` created.

**4. System Updates & Data Flow Back**
- **Ledger (Mod 31):** Dr Vendor ₹20k → Cr Bank ₹20k.
- **Module 29:** 
  - `BILL-201` Pending Amount reduces to **₹30,000**.
  - `BILL-201` Status changes to `PARTIAL`.
  - Transaction Ledger gets new row: `[Payment | PAY-202 | Dr ₹20,000]`.

---

## Scenario 3: Shortfall Payment (Settle & Close)

**1. Module 29 (Bills)**
- Bill `BILL-202` created for **₹50,500**. Status: `PENDING`.
- User clicks `[+ MAKE PAYMENT]`.

**2. Data Passed to Module 30**
- `Vendor` locked, `BILL-202` checked, `Allocate Amt` pre-filled ₹50,500.

**3. Action in Module 30**
- Vendor agreed to a ₹500 discount, so we are paying ₹50,000 flat.
- User enters `Amount Paid`: **₹50,000**.
- User changes `Allocate Amt` to **₹50,000**.
- `Settlement Action` appears. Shortfall = ₹500.
- User selects **Settle & Close**. Selects Reason: `Discount`.
- User clicks `[SAVE]`. Voucher `PAY-203` created.

**4. System Updates & Data Flow Back**
- **Module 29.5 (Debit Note):** 
  - System auto-generates `DN-001` for **₹500** against BILL-202.
- **Ledger (Mod 31):** 
  - From Payment: Dr Vendor ₹50k → Cr Bank ₹50k.
  - From Auto-DN: Dr Vendor ₹500 → Cr Discount Received A/C ₹500.
- **Module 29 (Bills):** 
  - `BILL-202` Pending Amount reduces to ₹500 (via payment), then to **₹0** (via DN).
  - `BILL-202` Status changes to `PAID`.
  - Transaction Ledger on `BILL-202` gets TWO rows:
    - 1. `[Payment    | PAY-203 | Dr ₹50,000]`
    - 2. `[Debit Note | DN-001  | Dr ₹500]`

---

## Scenario 4: Overpayment / Advance (Carry Forward)

**1. Module 29 (Bills)**
- Bill `BILL-203` created for **₹50,000**. Status: `PENDING`.
- User clicks `[+ MAKE PAYMENT]`.

**2. Data Passed to Module 30**
- `Vendor` locked, `BILL-203` checked, `Allocate Amt` pre-filled ₹50,000.

**3. Action in Module 30**
- We pre-transfer ₹60,000 for this bill and future orders.
- User enters `Amount Paid`: **₹60,000**.
- `Allocate Amt` remains ₹50,000 (cannot exceed pending).
- `Allocation Summary` shows: Allocated = ₹50,000, Unallocated = ₹10,000.
- `Carry to Advance?` is automatically handled.
- User clicks `[SAVE]`. Voucher `PAY-204` created.

**4. System Updates & Data Flow Back**
- **Ledger (Mod 31):** Dr Vendor ₹60k → Cr Bank ₹60k. (Vendor ledger now shows ₹10,000 debit balance / advance paid).
- **Module 29:** 
  - `BILL-203` Pending Amount becomes **₹0**, Status changes to `PAID`.
  - Transaction Ledger on `BILL-203` gets new row: `[Payment | PAY-204 | Dr ₹50,000]`.

---

## Scenario 5: Adjusting Existing Advance

**1. Module 29 (Bills)**
- Bill `BILL-204` created for **₹25,000**. Status: `PENDING`.
- Vendor has an existing advance balance of **₹10,000** (from Scenario 4).
- User clicks `[+ MAKE PAYMENT]`.

**2. Data Passed to Module 30**
- `Vendor` locked, `BILL-204` checked.
- `Advance Balance` shows **₹10,000**.
- `Adjust Advance?` toggle is `[ON]`.
- `Advance Applied` shows **₹10,000**.
- `Allocate Amt` pre-filled with remaining ₹15,000.

**3. Action in Module 30**
- User enters `Amount Paid`: **₹15,000** (the actual new bank transfer).
- `Allocation Summary` shows: Advance Applied = ₹10,000, Amount Paid = ₹15,000, Total Settled = ₹25,000.
- User clicks `[SAVE]`. Voucher `PAY-205` created.

**4. System Updates & Data Flow Back**
- **Ledger (Mod 31):** Dr Vendor ₹15k → Cr Bank ₹15k. (Vendor advance balance reduces from ₹10k to ₹0).
- **Module 29:** 
  - `BILL-204` Pending Amount becomes **₹0**, Status changes to `PAID`.
  - Transaction Ledger on `BILL-204` gets TWO rows: 
    - `[Advance Adj | PAY-205 | Dr ₹10,000]`
    - `[Payment     | PAY-205 | Dr ₹15,000]`

---
