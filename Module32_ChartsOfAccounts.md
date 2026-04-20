# 🎯 MODULE 32: CHART OF ACCOUNTS (COA)

## Overview

Chart of Accounts (COA) defines the **account groups and account heads** used to classify every financial posting in the ERP. In Phase 1, COA should be **simple to configure and easy to pick from** in Modules 28–31.

In practical terms:
- **Module 31 (Ledger)** uses COA to assign each ledger to an **Account Group** (e.g., Sundry Debtors, Bank Accounts, Direct Income).
- **Module 30 (Payments)** uses COA to select **Bank/Cash** accounts.
- **Modules 28/29 (Invoice/Bill)** use COA for **Income/Expense/Tax/TDS/ITC classification** (as per your accounting design).
- **Module 33 (Reports)** uses COA as the **structure** to group totals into P&L and Balance Sheet.

**Module Connections:**

- **Depends on:** Module 2 (Company Onboarding — initial COA setup), Module 7 (Branch — branch-wise account visibility)
- **Used by:** Module 31 (Ledger — account group assignment), Module 30 (Payments — bank/cash account selection), Module 33 (Reports — report grouping), Module 28 (Income/GST classification), Module 29 (Expense/ITC/TDS classification)
- **Prerequisites:** Configure basic COA before starting posting in Modules 28–30 and before creating internal ledgers in Module 31.

---

The module contains the following screens:

- 32.1 COA List View (Default — Phase 1)
- 32.2 Add / Edit Account Head (Group)
- 32.3 View Account Head Detail
- 32.4 COA Tree View (Phase 2 — Optional Enhancement)

---

================================================================================

# 32.1 COA List View (Default — Phase 1)

**Description:**
Phase‑1 default screen. Shows COA heads in a **simple sortable table** with filters and search.

This avoids building a complex dynamic tree while still supporting:
- Fast selection of account heads in Modules 28–31
- Consistent grouping for Module 33 reports

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       CHART OF ACCOUNTS (PHASE 1)                            │
│                                                                              │
│  VIEW: [📋 LIST VIEW]  [🌳 TREE VIEW (Phase 2)]                              │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Search: [____________________] (Account Name / Code / Path)           │  │
│  │ Primary Group: [▼ All ▼]  Type: [▼ All (Group/Ledger) ▼]              │  │
│  │ Status: [☑ Active ☑ Inactive]   Branch: [▼ All Branches ▼]           │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [+ ADD ACCOUNT HEAD]   [📥 EXPORT CSV]                                      │
│                                                                              │
│  COA LIST TABLE                                                              │
│  ┌──────────┬──────────────────────┬──────────────┬──────────────────┬─────┐  │
│  │Code      │Account Head Name     │Primary Group │Parent Group       │Type │  │
│  │──────────┼──────────────────────┼──────────────┼──────────────────┼─────│  │
│  │A-001     │Bank Accounts         │Assets        │Current Assets     │Group│  │
│  │A-001-001 │HDFC Current A/C      │Assets        │Bank Accounts      │Ledger│ │
│  │A-003     │Sundry Debtors        │Assets        │Current Assets     │Group│  │
│  │L-001     │Sundry Creditors      │Liabilities   │Current Liab.      │Group│  │
│  │I-001     │Service Income        │Income        │Direct Income      │Ledger│ │
│  │E-001     │Electricity Expense   │Expense       │Indirect Expense   │Ledger│ │
│  └──────────┴──────────────────────┴──────────────┴──────────────────┴─────┘  │
│                                                                              │
│  Actions: [View] [Edit] (per row)                                            │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table Fields (Phase 1)

| Field         | Type     | Required | Description |
|--------------|----------|----------|-------------|
| Code         | Text     | Auto     | Hierarchical code (editable as per business rule) |
| Account Name | Text     | Yes      | Group/head name |
| Primary Group| Dropdown | Yes      | Assets / Liabilities / Income / Expense / Capital |
| Parent Group | Dropdown | Yes      | Parent group under which this head exists |
| Type         | Badge    | Auto     | Group / Ledger |
| Nature       | Text     | Yes      | Default Dr / Cr side (used in reports + validations) |
| Status       | Badge    | Yes      | Active / Inactive |
| Branch Scope | Badge    | No       | All branches or restricted branch |

---

## Filters

| Filter        | Type         | Options |
|--------------|--------------|---------|
| Search       | Text         | Code / Account Name / Path |
| Primary Group| Dropdown     | Assets / Liabilities / Income / Expense / Capital |
| Type         | Dropdown     | All / Group / Ledger |
| Status       | Multi-select | Active / Inactive |
| Branch       | Dropdown     | All Branches / Specific Branch |

---

## Actions (Phase 1)

| Action              | Type   | Description |
|---------------------|--------|-------------|
| **+ Add Account Head** | Button | Opens Add/Edit form (32.2) |
| **View**            | Button | Opens detail screen (32.3) |
| **Edit**            | Button | Opens Add/Edit screen (32.2) |
| **Export CSV**      | Button | Exports current filtered table |

---

================================================================================

# 32.2 Add / Edit Account Head (Group)

**Description:**
Create or edit a COA **group/head**. In Phase 1, keep it simple: COA manages **groups**, and ledger accounts are created/managed in Module 31.

Notes for alignment with Modules 28–31:
- When creating ledgers in **Module 31.2**, users must be able to pick an **Account Group** from this COA.
- When posting from **Module 30**, users must be able to pick **Bank/Cash** ledgers (which are still Ledger accounts in Module 31, but grouped here).

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    ADD / EDIT COA ACCOUNT HEAD (GROUP)                       │
│                                                                              │
│  Account Name*   : [________________________]                                │
│  Primary Group*  : [▼ Assets/Liabilities/Income/Expense/Capital ▼]           │
│  Parent Group*   : [▼ Select Parent Group ▼]                                 │
│  Nature*         : (•) Dr   ( ) Cr                                           │
│                                                                              │
│  Code            : [AUTO] (Editable, must remain unique)                     │
│  Branch Scope    : [▼ All Branches ▼]                                        │
│  Affects GP?     : [☐ Yes] (Only relevant for P&L grouping)                  │
│  Status          : (•) Active   ( ) Inactive                                 │
│                                                                              │
│  [SAVE]  [CANCEL]                                                            │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Fields

| Field         | Type     | Required | Description |
|--------------|----------|----------|-------------|
| Account Name | Text     | Yes      | Group/head name |
| Primary Group| Dropdown | Yes      | Assets/Liabilities/Income/Expense/Capital |
| Parent Group | Dropdown | Yes      | Parent group (cannot be self) |
| Nature       | Radio    | Yes      | Default Dr/Cr side |
| Code         | Text     | Auto     | Unique code (system generated, editable) |
| Branch Scope | Dropdown | No       | All / specific branch (if branch-wise COA needed) |
| Affects GP   | Checkbox | No       | For P&L grouping (Direct vs Indirect) |
| Status       | Radio    | Yes      | Active / Inactive |

---

## Validation Rules (Phase 1)

| Rule | Description |
|------|-------------|
| Unique code | `Code` must be unique across the company |
| Unique name under same parent | Same `Account Name` cannot repeat under same `Parent Group` |
| Root groups locked | Primary groups (Assets/Liabilities/Income/Expense/Capital) cannot be deleted |
| Deletion | Group can be deleted only if it has no child groups and no ledgers assigned |
| Nature consistency | `Nature` should match expected default for primary group (configurable override allowed) |

---

## Business Rules (Aligned with Modules 28–31)

| Rule | Why it matters |
|------|----------------|
| COA before postings | Modules 28/29/30 should not post if required COA heads are missing |
| Ledger creation happens in Module 31 | COA creates **groups/heads**; Ledger accounts are managed in Module 31 |
| Bank/Cash heads required | Module 30 requires bank/cash ledgers grouped under COA Bank/Cash |
| Income/Expense mapping | Module 28/29 must map postings to correct COA heads for reporting |

---

## System Behavior (Phase 1)

| Event | System Action |
|------|---------------|
| Open Module 32 | Loads list view with filters |
| Save new head | Creates COA head and makes it selectable in Module 31.2 account group dropdown |
| Inactivate head | Prevent selection in new postings; existing ledgers remain classified (read-only impact) |

---

================================================================================

# 32.3 View Account Head Detail

**Description:**
Read-only view of a COA head showing its details and where it is used (ledgers assigned under it).

---

## View Fields

| Field | Type | Description |
|------|------|-------------|
| Account Name | Display | COA head name |
| Code | Display | Unique code |
| Primary Group | Display | Assets/Liabilities/Income/Expense/Capital |
| Parent Group | Display | Parent grouping |
| Nature | Display | Dr/Cr |
| Status | Display | Active/Inactive |
| Used By Ledgers | Table | List of ledgers (from Module 31) classified under this head |

---

## Actions

| Action | Type | Description |
|--------|------|-------------|
| Edit | Button | Opens Add/Edit (32.2) |
| Back to List | Button | Returns to List View (32.1) |

---

================================================================================

# 32.4 COA Tree View (Phase 2 — Optional Enhancement)

**Description:**
Phase‑2 enhancement. If needed later, render COA as an expandable tree.

For Phase 1 delivery, the **List View (32.1)** + **View Detail (32.3)** already supports the full accounting flow required for Modules 28–31 and reporting in Module 33.

---
