# 🎯 MODULE 31: LEDGER MANAGEMENT

## Overview

Ledger module is the **central account book ("Bahi Khata")** of the ERP system. Every Customer, Vendor, Bank Account, and internal account (Expenses, Income) has a Ledger showing the complete transaction history and current balance. All financial transactions from Invoicing (Module 28), Bills (Module 29), and Payments (Module 30) automatically post to the relevant Ledger. Supports opening balances, credit limits, ageing analysis, and account reconciliation.

**Module Connections:**

- **Depends on:** Module 18 (Customer Master — customer details), Module 11 (Vendor Master — vendor details), Module 32 (Chart of Accounts — account group classification)
- **Fed by:** Module 28 (Invoices → Customer Ledger Debit), Module 29 (Bills → Vendor Ledger Credit), Module 30 (Payments → Both Ledgers)
- **Used by:** Module 33 (Reports — Balance Sheet, P&L, Ageing), Module 28 (Outstanding check), Module 30 (Balance display)

---

The module contains the following screens:

- 31.1 Ledger Dashboard (Table View)
- 31.2 Create / Edit Ledger
- 31.3 Ledger Statement View (Account Passbook)

---

================================================================================

# 31.1 Ledger Dashboard (Table View)

**Description:**
The default landing screen for Module 31. Displays all ledger accounts in a **table/list format** with tab-based grouping: **All / Customers (Sundry Debtors) / Vendors (Sundry Creditors) / Bank & Cash / Income & Expenses**. Shows total receivable, payable, and cash position.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          LEDGER MANAGEMENT                                   │
│                                                                              │
│  TABS: [ ALL ] [ CUSTOMERS ] [ VENDORS ] [ BANK & CASH ] [ INCOME/EXPENSE ] │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                │  │
│  │                                                                        │  │
│  │ Branch        : [▼ All Branches ▼]                                    │  │
│  │ Account Group : [▼ Sundry Debtors / Sundry Creditors / Bank / etc ▼] │  │
│  │ Balance Type  : [☑ Debit (Dr) ☑ Credit (Cr) ☑ Zero]                │  │
│  │ Status        : [☑ Active ☑ Inactive]                                │  │
│  │                                                                        │  │
│  │ Search: [____________________] (Ledger Name / GSTIN / PAN)           │  │
│  │                                                 [Reset Filters]       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [+ CREATE LEDGER]   [📥 EXPORT ALL]   [📊 AGEING REPORT]                   │
│                                                                              │
│  SUMMARY CARDS                                                               │
│  ┌──────────────┬──────────────┬──────────────┬──────────────┐               │
│  │ Total        │ Total        │ Cash &       │ Overdue      │               │
│  │ Receivable   │ Payable      │ Bank Balance │ (>30 days)   │               │
│  │ ₹ 12,50,000  │ ₹ 8,20,000   │ ₹ 4,80,000   │ ₹ 2,10,000   │               │
│  └──────────────┴──────────────┴──────────────┴──────────────┘               │
│                                                                              │
│  LEDGER LIST TABLE                                                           │
│  ┌──────────────────┬──────────────────┬─────────────┬──────────────────┐    │
│  │Ledger Name       │Account Group     │GSTIN / PAN  │Opening Bal.      │    │
│  │──────────────────┼──────────────────┼─────────────┼──────────────────│    │
│  │ABC Corp Ltd      │Sundry Debtors    │27AAACB1234F │₹ 5,000 Dr        │    │
│  │Industrial Chem X │Sundry Creditors  │29AABCI1234F │₹ 15,000 Cr       │    │
│  │HDFC Current A/C  │Bank Accounts     │—            │₹ 3,50,000 Dr     │    │
│  │Petty Cash        │Cash-in-Hand      │—            │₹ 25,000 Dr       │    │
│  │Service Income    │Income (Direct)   │—            │₹ 0                │    │
│  │Electricity Exp   │Expenses(Indirect)│—            │₹ 0                │    │
│  └──────────────────┴──────────────────┴─────────────┴──────────────────┘    │
│                                                                              │
│  ┌──────────────┬──────────────┬─────────────┬──────────────────────────┐    │
│  │Total Dr      │Total Cr      │Closing Bal  │Actions                   │    │
│  │──────────────┼──────────────┼─────────────┼──────────────────────────│    │
│  │₹ 1,50,000    │₹ 1,20,000    │₹ 35,000 Dr  │[View] [Edit] [Statement] │    │
│  │₹ 50,000      │₹ 1,55,000    │₹ 1,05,000 Cr│[View] [Edit] [Statement] │    │
│  │₹ 8,50,000    │₹ 5,00,000    │₹ 7,00,000 Dr│[View] [Statement]       │    │
│  │₹ 1,00,000    │₹ 75,000      │₹ 50,000 Dr  │[View] [Statement]       │    │
│  │—             │₹ 18,50,000   │₹ 18,50,000Cr│[View] [Statement]       │    │
│  │₹ 2,50,000    │—             │₹ 2,50,000 Dr│[View] [Statement]       │    │
│  └──────────────┴──────────────┴─────────────┴──────────────────────────┘    │
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   10   Next                         │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table View Fields

| Field          | Type   | Required | Description                                                |
| -------------- | ------ | -------- | ---------------------------------------------------------- |
| Ledger Name    | Text   | Auto     | Name of the party / account                                |
| Account Group  | Text   | Auto     | Classification from Module 32 (Debtors/Creditors/Bank etc) |
| GSTIN / PAN    | Text   | Auto     | Tax identification (for customers/vendors)                 |
| Opening Balance| Number | Auto     | Starting balance when ERP was initialized                  |
| Total Debit    | Number | Auto     | Sum of all debit entries                                   |
| Total Credit   | Number | Auto     | Sum of all credit entries                                  |
| Closing Balance| Number | Auto     | Opening + Total Dr - Total Cr (or vice versa depending on nature) |
| Actions        | Buttons| —        | View / Edit / Statement                                    |

---

## Summary Card Fields

| Field           | Type   | Description                                              |
| --------------- | ------ | -------------------------------------------------------- |
| Total Receivable| Number | Sum of all Sundry Debtors' Dr balances                   |
| Total Payable   | Number | Sum of all Sundry Creditors' Cr balances                 |
| Cash & Bank     | Number | Sum of all bank and cash account Dr balances              |
| Overdue (>30d)  | Number | Receivable + Payable amounts older than 30 days          |

---

## Filters

| Filter        | Type         | Options                                              |
| ------------- | ------------ | ---------------------------------------------------- |
| Branch        | Dropdown     | All Branches / Specific Branch                       |
| Account Group | Dropdown     | Sundry Debtors / Sundry Creditors / Bank / Cash / Income / Expenses |
| Balance Type  | Multi-select | Debit (Dr) / Credit (Cr) / Zero                     |
| Status        | Multi-select | Active / Inactive                                    |

---

## Search

Searchable by:

- Ledger Name
- GSTIN
- PAN Number

---

## Actions (Table Row)

| Action           | Type   | Description                                              |
| ---------------- | ------ | -------------------------------------------------------- |
| **View**         | Button | Opens ledger detail in read-only mode                    |
| **Edit**         | Button | Edit ledger details (name, opening balance, credit limit)|
| **Statement**    | Button | Opens Ledger Statement View (Screen 31.3)                |

---

## Form Actions

| Action              | Description                                             |
| ------------------- | ------------------------------------------------------- |
| **+ Create Ledger** | Opens Create Ledger form (Screen 31.2)                  |
| **Export All**       | Export ledger list as Excel/CSV                          |
| **Ageing Report**   | Opens ageing analysis for all debtors/creditors         |

---

================================================================================

# 31.2 Create / Edit Ledger

**Description:**
Form to create a new ledger account or edit an existing one. Used for adding new parties (Customers, Vendors), bank accounts, or internal account heads. Customer and Vendor ledgers are auto-created when a new Customer (Module 18) or Vendor (Module 11) is added — this form is for manual creation or modification.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        CREATE / EDIT LEDGER                                  │
│                                                                              │
│  BASIC DETAILS                                                               │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Ledger Name*       : [________________________]                       │  │
│  │ Account Group*     : [▼ Sundry Debtors ▼]                             │  │
│  │                     (Sundry Debtors / Sundry Creditors / Bank /       │  │
│  │                      Cash / Fixed Assets / Income / Expense /         │  │
│  │                      Duties & Taxes / Capital / Loans)                │  │
│  │ Branch             : [▼ Mumbai ▼]                                     │  │
│  │ Status             : (•) Active    ( ) Inactive                       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  PARTY DETAILS (For Sundry Debtors / Creditors only)                         │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Linked Customer/Vendor: [🔍 Search ▼] (Auto-links from Mod 18/11)    │  │
│  │ GSTIN               : [________________________]                      │  │
│  │ PAN                 : [________________________]                      │  │
│  │ Contact Person      : [________________________]                      │  │
│  │ Phone               : [________________________]                      │  │
│  │ Email               : [________________________]                      │  │
│  │ Address             : [________________________________________________]│  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  BANK DETAILS (For Bank Accounts only)                                       │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Bank Name           : [________________________]                      │  │
│  │ Account Number      : [________________________]                      │  │
│  │ IFSC Code           : [________________________]                      │  │
│  │ Account Type        : [▼ Current / Savings ▼]                         │  │
│  │ Branch Name         : [________________________]                      │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  FINANCIAL SETTINGS                                                          │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Opening Balance*    : ₹ [________]      Type: (•) Dr  ( ) Cr          │  │
│  │ As on Date          : [📅 01 Apr 2025]  (Financial year opening)      │  │
│  │ Credit Limit        : ₹ [________]      (0 = No Limit)               │  │
│  │ Credit Period (Days): [30]              (Default payment terms)       │  │
│  │ TDS Applicable      : [☐ Yes]           Section: [▼ 194C ▼]          │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [SAVE]    [CANCEL]                                                          │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Screen Fields: Basic Details

| Field         | Type     | Required | Description                                          |
| ------------- | -------- | -------- | ---------------------------------------------------- |
| Ledger Name   | Text     | Yes      | Party or account name                                |
| Account Group | Dropdown | Yes      | Classification from Module 32 hierarchy              |
| Branch        | Dropdown | No       | Associate with specific branch (or All)              |
| Status        | Radio    | Yes      | Active / Inactive (default: Active)                  |

---

## Screen Fields: Party Details

| Field             | Type    | Required | Description                                     |
| ----------------- | ------- | -------- | ----------------------------------------------- |
| Linked Party      | Search  | No       | Auto-link to Customer (Mod 18) / Vendor (Mod 11)|
| GSTIN             | Text    | Cond.    | Mandatory for GST-registered parties            |
| PAN               | Text    | Cond.    | Mandatory for TDS-applicable parties            |
| Contact Person    | Text    | No       | Primary contact name                            |
| Phone             | Phone   | No       | Contact phone (10 digits)                       |
| Email             | Email   | No       | Contact email                                   |
| Address           | Textarea| No       | Full address                                    |

---

## Screen Fields: Bank Details

| Field          | Type     | Required | Description                                      |
| -------------- | -------- | -------- | ------------------------------------------------ |
| Bank Name      | Text     | Cond.    | Required if Account Group = Bank                 |
| Account Number | Text     | Cond.    | Required if Account Group = Bank                 |
| IFSC Code      | Text     | Cond.    | Required if Account Group = Bank                 |
| Account Type   | Dropdown | Cond.    | Current / Savings                                |
| Branch Name    | Text     | No       | Bank branch name                                 |

---

## Screen Fields: Financial Settings

| Field          | Type    | Required | Description                                       |
| -------------- | ------- | -------- | ------------------------------------------------- |
| Opening Balance| Number  | Yes      | Starting balance (default: 0)                     |
| Balance Type   | Radio   | Yes      | Dr (Debit) or Cr (Credit)                         |
| As on Date     | Date    | Yes      | Date of opening balance (usually FY start)        |
| Credit Limit   | Number  | No       | Maximum credit allowed (0 = unlimited)            |
| Credit Period  | Number  | No       | Default payment terms in days                     |
| TDS Applicable | Checkbox| No       | Whether TDS applies to this party                 |
| TDS Section    | Dropdown| Cond.    | Required if TDS = Yes (194C / 194J / 194H etc.)  |

---

## Validation Rules

| Field           | Rule                                                        |
| --------------- | ----------------------------------------------------------- |
| Ledger Name     | Must be unique within the same Account Group                |
| Account Group   | Must select from Module 32 categories                       |
| GSTIN           | If provided, must be valid 15-character GSTIN format        |
| PAN             | If provided, must be valid 10-character PAN format          |
| Opening Balance | Must be >= 0                                                |
| Credit Limit    | Must be >= 0                                                |
| IFSC Code       | If Bank account, must be valid 11-character IFSC            |
| Account Number  | If Bank account, must be provided                           |

---

## Business Rules

| Rule                              | Description                                               |
| --------------------------------- | --------------------------------------------------------- |
| Auto-creation from Module 18/11   | When new Customer or Vendor is created, Ledger is auto-created |
| Credit Limit enforcement          | System warns when new invoice exceeds customer's credit limit |
| Inactive Ledger                   | Cannot post new transactions to inactive ledgers          |
| Opening Balance only once         | Can only be set during creation or FY opening; locked after first transaction |
| Edit restrictions                 | Ledger Name and Account Group cannot be changed after first transaction |

---

## System Behavior

| Event                           | System Action                                             |
| ------------------------------- | --------------------------------------------------------- |
| Save clicked (new)              | Ledger created with opening balance entry                 |
| Customer created in Module 18   | Auto-creates Sundry Debtor ledger                         |
| Vendor created in Module 11     | Auto-creates Sundry Creditor ledger                       |
| Credit limit exceeded           | Warning popup when creating new Invoice/Bill              |

---

================================================================================

# 31.3 Ledger Statement View (Account Passbook)

**Description:**
The statement screen shows a **chronological transaction history** for a specific ledger, similar to a bank passbook. Displays every Debit and Credit entry with running balance. Supports date-range filtering, PDF export, and email to party.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    LEDGER STATEMENT — ABC Corp Ltd                            │
│                    Account Group: Sundry Debtors                             │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Date Range : [📅 01 Apr 2025] - [📅 28 Mar 2026]                     │  │
│  │                                                    [GENERATE]        │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  PARTY DETAILS                                                               │
│  ┌──────────────────────────────┬────────────────────────────────────────┐  │
│  │ Name    : ABC Corp Ltd       │ GSTIN     : 27AAACB1234F1Z5           │  │
│  │ Address : 45 MG Road, Fort,  │ Credit Limit : ₹ 2,00,000            │  │
│  │           Mumbai 400001      │ Credit Period: 15 Days                │  │
│  └──────────────────────────────┴────────────────────────────────────────┘  │
│                                                                              │
│  ACCOUNT SUMMARY                                                             │
│  ┌──────────────┬──────────────┬──────────────┬──────────────┐               │
│  │ Opening Bal  │ Total Debit  │ Total Credit │ Closing Bal  │               │
│  │ ₹ 5,000 Dr   │ ₹ 1,50,000   │ ₹ 1,20,000   │ ₹ 35,000 Dr  │               │
│  └──────────────┴──────────────┴──────────────┴──────────────┘               │
│                                                                              │
│  TRANSACTION HISTORY                                                         │
│  ┌──────────┬──────────┬──────────────────┬──────────┬──────────┬─────────┐  │
│  │Date      │Ref #     │Particulars       │Debit(Dr) │Credit(Cr)│Balance  │  │
│  │──────────┼──────────┼──────────────────┼──────────┼──────────┼─────────│  │
│  │01 Apr 25 │—         │Opening Balance   │₹  5,000  │—         │₹  5,000 │  │
│  │          │          │                  │          │          │   Dr    │  │
│  │15 Jun 25 │INV-10001 │Sales Invoice     │₹ 25,000  │—         │₹ 30,000 │  │
│  │          │          │(Monthly Service) │          │          │   Dr    │  │
│  │25 Jun 25 │RCP-3001  │Receipt (UPI)     │—         │₹ 25,000  │₹  5,000 │  │
│  │          │          │                  │          │          │   Dr    │  │
│  │15 Jul 25 │INV-10005 │Sales Invoice     │₹ 25,000  │—         │₹ 30,000 │  │
│  │          │          │(Monthly Service) │          │          │   Dr    │  │
│  │20 Jul 25 │RCP-3010  │Receipt (Cheque)  │—         │₹ 20,000  │₹ 10,000 │  │
│  │          │          │Subject to Clr    │          │          │   Dr    │  │
│  │15 Mar 26 │INV-10024 │Sales Invoice     │₹ 15,000  │—         │₹ 35,000 │  │
│  │          │          │(Termite+Cockroach│          │          │   Dr    │  │
│  │          │          │ Treatment)       │          │          │         │  │
│  └──────────┴──────────┴──────────────────┴──────────┴──────────┴─────────┘  │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Closing Balance: ₹ 35,000 Dr (ABC Corp Ltd owes us ₹ 35,000)        │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [📥 DOWNLOAD PDF]  [📧 EMAIL TO PARTY]  [🖨 PRINT]  [🔙 BACK TO LIST]    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Statement Fields

| Field          | Type    | Description                                               |
| -------------- | ------- | --------------------------------------------------------- |
| Date           | Date    | Transaction date                                          |
| Ref #          | Link    | Clickable reference (Invoice / Receipt / Bill / Payment)  |
| Particulars    | Text    | Description of the transaction                            |
| Debit (Dr)     | Number  | Amount increasing the balance (for debtors)               |
| Credit (Cr)    | Number  | Amount decreasing the balance (for debtors)               |
| Balance        | Number  | Running cumulative balance with Dr/Cr indicator           |

---

## Account Summary Fields

| Field          | Type   | Description                                               |
| -------------- | ------ | --------------------------------------------------------- |
| Opening Balance| Number | Balance at the start of selected period                   |
| Total Debit    | Number | Sum of all Debit entries in the period                    |
| Total Credit   | Number | Sum of all Credit entries in the period                   |
| Closing Balance| Number | Opening + Debits - Credits (for Debtors)                  |

---

## Actions (Statement Screen)

| Action              | Type   | Description                                          |
| ------------------- | ------ | ---------------------------------------------------- |
| **Download PDF**    | Button | Download formatted statement as PDF                  |
| **Email to Party**  | Button | Send statement to party's registered email           |
| **Print**           | Button | Print formatted statement                            |
| **Back to List**    | Button | Returns to Ledger Dashboard (31.1)                   |

---

## Business Rules

| Rule                          | Description                                                |
| ----------------------------- | ---------------------------------------------------------- |
| Running balance               | Each row shows cumulative balance (not just trans. amount) |
| Clickable references          | Ref # links open the original document (Invoice/Receipt)  |
| Date range filter             | Default: Current financial year (01 Apr - 31 Mar)         |
| Dr / Cr logic per group       | Debtors: Dr = Owes more, Cr = Paid. Creditors: Reversed  |
| Interest calculation (Future) | Optional auto-interest on overdue balances                |

---

## System Behavior

| Event                      | System Action                                             |
| -------------------------- | --------------------------------------------------------- |
| Ledger selected            | Fetches all transactions in default date range            |
| Date range changed         | Re-fetches transactions, recalculates opening/closing     |
| PDF Downloaded             | Formatted with company letterhead and party details       |
| Email sent                 | PDF attached, sent to party's email, logged in audit      |

---
