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

