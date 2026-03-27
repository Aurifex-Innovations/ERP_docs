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

*End of Module 26 — Technician Performance & Productivity*
