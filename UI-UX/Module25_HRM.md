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

# 25.5.1 Leave Request Action Popup

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