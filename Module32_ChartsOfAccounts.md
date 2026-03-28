# 🎯 MODULE 32: CHART OF ACCOUNTS (COA)

## Overview

Chart of Accounts module defines the **hierarchical structure of all financial accounts** used across the ERP. Think of it as the **"Folder System"** for organizing every type of money — Income, Expenses, Assets, Liabilities, and Capital. Every Ledger (Module 31) is classified under one of these account heads. The COA follows the Indian accounting standard with 5 primary groups and unlimited sub-groups. Supports custom account heads per company.

**Module Connections:**

- **Depends on:** Module 2 (Company Onboarding — initial COA setup), Module 7 (Branch — branch-wise account visibility)
- **Used by:** Module 31 (Ledger — account group assignment), Module 30 (Payments — bank/cash account selection), Module 33 (Reports — P&L and Balance Sheet structure), Module 28 (Income classification), Module 29 (Expense classification)
- **Prerequisites:** Must be configured before creating any Ledgers or Financial Transactions

---

The module contains the following screens:

- 32.1 COA Tree View (Default)
- 32.2 COA List View
- 32.3 Add / Edit Account Head
- 32.4 View Account Head Detail

---

================================================================================

# 32.1 COA Tree View (Default)

**Description:**
The default landing screen for Module 32. Displays the entire account hierarchy as an **expandable tree/folder structure**. Users can expand/collapse groups to see sub-groups and ledgers underneath. Color-coded by account type. Supports drag-and-drop reordering for custom arrangement.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       CHART OF ACCOUNTS                                      │
│                                                                              │
│  VIEW: [🌳 TREE VIEW]  [📋 LIST VIEW]                                       │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Search: [____________________] (Account Name / Code)                  │  │
│  │ Filter: [☑ Assets ☑ Liabilities ☑ Income ☑ Expense ☑ Capital]      │  │
│  │                                                       [Expand All]   │  │
│  │                                                      [Collapse All]  │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [+ ADD ACCOUNT HEAD]                                                        │
│                                                                              │
│  ACCOUNT TREE                                                                │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                                                                       │  │
│  │  📁 ASSETS                                                            │  │
│  │  ├── 📁 Current Assets                                                │  │
│  │  │   ├── 📁 Bank Accounts                                             │  │
│  │  │   │   ├── 📄 HDFC Current A/C (1234)            ₹ 7,00,000 Dr     │  │
│  │  │   │   ├── 📄 SBI Savings A/C (5678)             ₹ 1,50,000 Dr     │  │
│  │  │   │   └── 📄 ICICI Current A/C (9012)           ₹ 2,30,000 Dr     │  │
│  │  │   ├── 📁 Cash-in-Hand                                              │  │
│  │  │   │   └── 📄 Petty Cash                         ₹ 50,000 Dr       │  │
│  │  │   ├── 📁 Sundry Debtors (Trade Receivables)                        │  │
│  │  │   │   ├── 📄 ABC Corp Ltd                       ₹ 35,000 Dr       │  │
│  │  │   │   ├── 📄 XYZ Hotels Pvt Ltd                 ₹ 22,000 Dr       │  │
│  │  │   │   └── 📄 Global Biz Solutions               ₹ 15,000 Dr       │  │
│  │  │   └── 📁 Stock-in-Hand                                             │  │
│  │  │       └── 📄 Inventory (Module 11)              ₹ 3,50,000 Dr     │  │
│  │  ├── 📁 Fixed Assets                                                  │  │
│  │  │   ├── 📄 Office Equipment                       ₹ 2,00,000 Dr     │  │
│  │  │   ├── 📄 Vehicles                               ₹ 5,00,000 Dr     │  │
│  │  │   └── 📄 Pest Control Machines (Module 11)      ₹ 1,80,000 Dr     │  │
│  │  └── 📁 Investments                                                   │  │
│  │                                                                       │  │
│  │  📁 LIABILITIES                                                       │  │
│  │  ├── 📁 Current Liabilities                                           │  │
│  │  │   ├── 📁 Sundry Creditors (Trade Payables)                         │  │
│  │  │   │   ├── 📄 Industrial Chemicals Pvt Ltd       ₹ 1,05,300 Cr     │  │
│  │  │   │   ├── 📄 Agro Chem Products                 ₹ 45,000 Cr       │  │
│  │  │   │   └── 📄 Office Mart Supplies               ₹ 5,000 Cr        │  │
│  │  │   ├── 📁 Duties & Taxes                                            │  │
│  │  │   │   ├── 📄 CGST Payable                       ₹ 15,000 Cr       │  │
│  │  │   │   ├── 📄 SGST Payable                       ₹ 15,000 Cr       │  │
│  │  │   │   ├── 📄 IGST Payable                       ₹ 8,000 Cr        │  │
│  │  │   │   ├── 📄 TDS Payable (Various Sections)     ₹ 5,500 Cr        │  │
│  │  │   │   └── 📄 CGST Input Credit                  ₹ 12,000 Dr       │  │
│  │  │   └── 📁 Provisions                                                │  │
│  │  │       └── 📄 Salary Payable                     ₹ 2,50,000 Cr     │  │
│  │  └── 📁 Long-term Liabilities                                         │  │
│  │      └── 📄 Term Loan (Bank)                       ₹ 15,00,000 Cr    │  │
│  │                                                                       │  │
│  │  📁 INCOME (Revenue)                                                  │  │
│  │  ├── 📁 Direct Income                                                 │  │
│  │  │   ├── 📄 Service Income (Pest Control)          ₹ 18,50,000 Cr    │  │
│  │  │   └── 📄 Product Sales (Resell)                 ₹ 2,20,000 Cr     │  │
│  │  └── 📁 Indirect Income                                               │  │
│  │      ├── 📄 Interest Received                      ₹ 15,000 Cr       │  │
│  │      └── 📄 Other Income                           ₹ 5,000 Cr        │  │
│  │                                                                       │  │
│  │  📁 EXPENSES                                                          │  │
│  │  ├── 📁 Direct Expenses                                               │  │
│  │  │   ├── 📄 Chemical Cost                          ₹ 4,50,000 Dr     │  │
│  │  │   ├── 📄 Equipment Cost                         ₹ 1,20,000 Dr     │  │
│  │  │   └── 📄 Fuel & Transportation                  ₹ 80,000 Dr       │  │
│  │  └── 📁 Indirect Expenses                                             │  │
│  │      ├── 📄 Salaries & Wages                       ₹ 6,00,000 Dr     │  │
│  │      ├── 📄 Rent                                   ₹ 1,50,000 Dr     │  │
│  │      ├── 📄 Electricity                            ₹ 25,000 Dr       │  │
│  │      ├── 📄 Office Supplies                        ₹ 15,000 Dr       │  │
│  │      └── 📄 Professional Fees                      ₹ 50,000 Dr       │  │
│  │                                                                       │  │
│  │  📁 CAPITAL                                                           │  │
│  │  └── 📄 Owner's Capital                            ₹ 25,00,000 Cr    │  │
│  │                                                                       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Tree Node Types

| Node Type   | Icon | Description                                            |
| ----------- | ---- | ------------------------------------------------------ |
| Group       | 📁   | A category/folder containing sub-groups or ledgers     |
| Ledger      | 📄   | An actual account (linked to Module 31)                |

---

## Tree Node Information

| Field        | Type    | Description                                             |
| ------------ | ------- | ------------------------------------------------------- |
| Account Name | Text    | Name of the group or ledger                             |
| Balance      | Number  | Current balance with Dr/Cr indicator (for ledgers only) |
| Count        | Number  | Number of child accounts (for groups only)              |

---

## Filters

| Filter  | Type         | Options                                               |
| ------- | ------------ | ----------------------------------------------------- |
| Type    | Multi-select | Assets / Liabilities / Income / Expense / Capital     |
| Search  | Text         | Search by account name or code                        |

---

## Actions (Tree Node)

| Action        | Type   | Condition       | Description                                       |
| ------------- | ------ | --------------- | ------------------------------------------------- |
| **Expand**    | Click  | Group nodes     | Shows child groups and ledgers                    |
| **Collapse**  | Click  | Group nodes     | Hides child nodes                                 |
| **View**      | Button | Ledger nodes    | Opens ledger statement (Module 31.3)              |
| **Edit**      | Button | All nodes       | Opens Edit Account Head form (32.3)               |
| **Add Child** | Button | Group nodes     | Opens Add Account Head under this group           |
| **Delete**    | Button | Empty groups    | Deletes group only if no children exist           |

---

## Form Actions

| Action                 | Description                                            |
| ---------------------- | ------------------------------------------------------ |
| **+ Add Account Head** | Opens Add Account Head form (Screen 32.3)              |
| **Expand All**         | Expands all tree nodes                                 |
| **Collapse All**       | Collapses all tree nodes to root level                 |

---

================================================================================

# 32.2 COA List View

**Description:**
Alternative flat-list view of all account heads in a sortable table format. Useful for bulk export, quick search, and data analysis. Shows the same accounts as the Tree View but without hierarchical nesting.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       CHART OF ACCOUNTS (LIST VIEW)                          │
│                                                                              │
│  VIEW: [🌳 TREE VIEW]  [📋 LIST VIEW] ← Active                             │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Search: [____________________]    Filter: [▼ All Types ▼]            │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [+ ADD ACCOUNT HEAD]   [📥 EXPORT CSV]                                     │
│                                                                              │
│  ┌──────────┬──────────────────────┬──────────────────┬───────┬──────┬────┐  │
│  │Code      │Account Name          │Parent Group       │Type   │Nature│Bal │  │
│  │──────────┼──────────────────────┼──────────────────┼───────┼──────┼────│  │
│  │A-001     │Bank Accounts         │Current Assets     │Group  │Dr   │—   │  │
│  │A-001-001 │HDFC Current A/C      │Bank Accounts      │Ledger │Dr   │7L  │  │
│  │A-001-002 │SBI Savings A/C       │Bank Accounts      │Ledger │Dr   │1.5L│  │
│  │A-002     │Sundry Debtors        │Current Assets     │Group  │Dr   │—   │  │
│  │A-002-001 │ABC Corp Ltd          │Sundry Debtors     │Ledger │Dr   │35K │  │
│  │L-001     │Sundry Creditors      │Current Liabilities│Group  │Cr   │—   │  │
│  │L-001-001 │Industrial Chemicals  │Sundry Creditors   │Ledger │Cr   │1.05L│ │
│  │I-001     │Service Income        │Direct Income      │Ledger │Cr   │18.5L│ │
│  │E-001     │Chemical Cost         │Direct Expenses    │Ledger │Dr   │4.5L│  │
│  └──────────┴──────────────────────┴──────────────────┴───────┴──────┴────┘  │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │Actions                                                               │    │
│  │──────────────────────────────────────────────────────────────────────│    │
│  │[Edit]                                                                │    │
│  │[View] [Edit]                                                         │    │
│  │[View] [Edit]                                                         │    │
│  │[Edit]                                                                │    │
│  │[View] [Edit]                                                         │    │
│  │[Edit]                                                                │    │
│  │[View] [Edit]                                                         │    │
│  │[View] [Edit]                                                         │    │
│  │[View] [Edit]                                                         │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   10   Next                         │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table View Fields

| Field        | Type   | Required | Description                                             |
| ------------ | ------ | -------- | ------------------------------------------------------- |
| Code         | Text   | Auto     | Hierarchical account code (A-001, A-001-001, etc.)      |
| Account Name | Text   | Auto     | Name of the account head or group                       |
| Parent Group | Text   | Auto     | Parent group name in the hierarchy                      |
| Type         | Badge  | Auto     | Group / Ledger                                          |
| Nature       | Text   | Auto     | Dr (Debit) / Cr (Credit) — default balance side         |
| Balance      | Number | Auto     | Current balance (for ledgers only)                      |
| Actions      | Buttons| —        | View / Edit                                             |

---

================================================================================

# 32.3 Add / Edit Account Head

**Description:**
Form to add a new account group or sub-group to the Chart of Accounts, or to edit an existing one. Ledgers are created through Module 31; this form is specifically for **account groups** (folders in the hierarchy).

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     ADD / EDIT ACCOUNT HEAD                                   │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Account Name*     : [________________________]                        │  │
│  │ Account Code      : [_________]   (Auto-generated, editable)          │  │
│  │ Primary Group*    : [▼ Assets / Liabilities / Income / Expense / Capital] │
│  │ Parent Group*     : [▼ Current Assets ▼]  (Sub-group under this)      │  │
│  │ Nature*           : (•) Debit (Dr)    ( ) Credit (Cr)                 │  │
│  │ Description       : [_____________________________________________]   │  │
│  │                                                                       │  │
│  │ Affects Gross Profit : [☐ Yes]   (For P&L grouping)                   │  │
│  │ Status              : (•) Active   ( ) Inactive                       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [SAVE]    [CANCEL]                                                          │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Screen Fields

| Field          | Type     | Required | Description                                          |
| -------------- | -------- | -------- | ---------------------------------------------------- |
| Account Name   | Text     | Yes      | Name of the new group or sub-group                   |
| Account Code   | Text     | Auto     | Auto-generated hierarchical code (editable)          |
| Primary Group  | Dropdown | Yes      | Root category: Assets/Liabilities/Income/Expense/Capital |
| Parent Group   | Dropdown | Yes      | Immediate parent in the tree hierarchy               |
| Nature         | Radio    | Yes      | Default balance side (Debit or Credit)               |
| Description    | Textarea | No       | Brief description of what this group contains        |
| Affects GP     | Checkbox | No       | Whether this group is for Gross Profit calculation   |
| Status         | Radio    | Yes      | Active / Inactive (default: Active)                  |

---

## Validation Rules

| Field          | Rule                                                        |
| -------------- | ----------------------------------------------------------- |
| Account Name   | Must be unique within the same Parent Group                 |
| Account Code   | Must be unique across the entire COA                        |
| Primary Group  | Must select one of the 5 root groups                        |
| Parent Group   | Must be a valid existing group                              |
| Nature         | Must match the Primary Group's default nature               |
| Delete         | Group can only be deleted if it has no child groups/ledgers |

---

## Business Rules

| Rule                            | Description                                                |
| ------------------------------- | ---------------------------------------------------------- |
| Default COA on company creation | System creates a standard Indian COA when a company signs up |
| Nature inheritance              | Sub-groups inherit the Nature (Dr/Cr) from parent           |
| Rename restricted               | Group names with transactions cannot be renamed             |
| No duplicate codes              | Account codes must be unique across the entire COA          |
| System groups locked            | Root-level groups (Assets, Liabilities, etc.) cannot be deleted |

---

## System Behavior

| Event                    | System Action                                              |
| ------------------------ | ---------------------------------------------------------- |
| Save clicked (new)       | New group added to tree, code auto-assigned                |
| Parent Group changed     | Account code prefix updated hierarchically                 |
| Delete attempted         | Checks for child accounts; blocks if children exist        |
| Company created (Mod 2)  | Default Indian COA auto-generated                          |

---

================================================================================

# 32.4 View Account Head Detail

**Description:**
Read-only view of an account head showing its complete details, child accounts, and aggregate balance of all ledgers under it.

---

## View Fields

| Field          | Type    | Description                                              |
| -------------- | ------- | -------------------------------------------------------- |
| Account Name   | Display | Group name                                               |
| Account Code   | Display | Hierarchical code                                        |
| Primary Group  | Display | Root category                                            |
| Parent Group   | Display | Immediate parent                                         |
| Nature         | Display | Dr / Cr                                                  |
| Description    | Display | Group description                                        |
| Child Accounts | Table   | List of sub-groups and ledgers under this group          |
| Aggregate Bal  | Number  | Sum of all ledger balances under this group              |

---

## Actions

| Action           | Type   | Description                                         |
| ---------------- | ------ | --------------------------------------------------- |
| **Edit**         | Button | Opens edit form (32.3)                              |
| **Add Child**    | Button | Opens Add form with this group as parent            |
| **View Statement**| Button| Opens aggregate statement of all child ledgers     |
| **Back to Tree** | Button | Returns to COA Tree View (32.1)                     |

---
