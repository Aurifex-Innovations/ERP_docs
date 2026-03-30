# 🎯 MODULE 30: PAYMENTS (RECEIPTS & VOUCHERS)

## Overview

Payments module manages all **Cash and Bank transactions** — recording money received from customers (Receipts) and money paid to vendors/expenses (Payments). Supports multiple payment modes (Cash, Bank Transfer, UPI, Cheque, Card). Links every transaction to specific Invoices (Module 28) or Bills (Module 29) for reconciliation. Handles Contra entries (inter-bank transfers) and Journal entries (non-cash adjustments).

**Module Connections:**

- **Depends on:** Module 28 (Invoicing — receipt allocation against sales invoices), Module 29 (Bills — payment allocation against purchase bills), Module 31 (Ledger — party balance), Module 32 (Chart of Accounts — bank/cash account heads)
- **Used by:** Module 31 (Ledger — balance reduction on payment/receipt), Module 33 (Reports — cash flow, bank reconciliation)
- **Prerequisites:** Module 28 or Module 29 must have at least one approved document

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
│  ┌────────────┬──────────────┬──────────────────────────────────────────┐    │
│  │Mode        │Allocated To  │Actions                                   │    │
│  │────────────┼──────────────┼──────────────────────────────────────────│    │
│  │UPI         │INV-10024     │[View] [PDF] [Print Receipt]              │    │
│  │Cheque      │BILL-5524     │[View] [PDF] [Print Voucher]             │    │
│  │Bank Trfr   │HDFC → SBI    │[View]                                   │    │
│  │Adjustment  │TDS Adj.      │[View]                                   │    │
│  └────────────┴──────────────┴──────────────────────────────────────────┘    │
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
Form to record money received from a customer. The amount is allocated against one or more pending invoices from Module 28. If the amount exceeds all pending invoices, the excess is marked as **"On Account" (Advance)**. Supports all payment modes. Auto-updates Customer Ledger upon saving.

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
│  │ Customer*        : [🔍 Search Customer ▼]                            │  │
│  │ Current Balance  : ₹ 45,000 (Owed by customer)     [DISPLAY]         │  │
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
│                                                                              │
│  ALLOCATION SUMMARY                                                          │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Amount Received        : ₹ 25,000                                     │  │
│  │ Allocated to Invoices  : ₹ 20,000                                     │  │
│  │ ──────────────────────────────────────                                 │  │
│  │ Unallocated (Advance)  : ₹ 5,000                                     │  │
│  │ Carry to Advance?      : [☑ Yes]                                      │  │
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
| Customer        | Search  | Yes      | Select customer (fetches pending invoices)         |
| Current Balance | Display | Auto     | Total outstanding from Customer Ledger             |

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
| Invoice #    | Display  | Auto     | Invoice number from Module 28                     |
| Invoice Date | Display  | Auto     | Date of original invoice                          |
| Total Amount | Display  | Auto     | Original invoice amount                           |
| Pending Amt  | Display  | Auto     | Remaining unpaid amount                           |
| Allocate Amt | Number   | Cond.    | Amount to allocate (editable, cannot exceed pending)|

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
| Customer        | Must exist in Module 18                                       |
| Payment Mode    | Must select one option                                        |
| Bank Account    | Required if Mode is Bank/UPI/Cheque/Card                      |
| Ref / UTR       | Required for Bank/UPI modes. Must be unique                   |
| Cheque Date     | Required if Mode = Cheque. Cannot be older than 3 months      |
| Amount Received | Must be greater than 0                                        |
| Allocate Amount | Cannot exceed Pending Amount of individual invoice             |
| Total Allocation| Sum of all allocations cannot exceed Amount Received           |

---

## Business Rules

| Rule                            | Description                                                  |
| ------------------------------- | ------------------------------------------------------------ |
| FIFO Allocation default         | System auto-suggests allocation to oldest invoice first      |
| Partial allocation allowed      | An invoice can be partially paid across multiple receipts    |
| Advance handling                | Unallocated amount saved as "On Account" in Customer Ledger  |
| Cheque clearance                | Cheque receipts marked "Subject to Clearance" until confirmed|
| TDS by customer                 | If customer deducted TDS, it is recorded separately          |

---

## System Behavior

| Event                        | System Action                                                |
| ---------------------------- | ------------------------------------------------------------ |
| Customer selected            | Fetches all pending invoices and current balance             |
| Amount entered               | Auto-suggests FIFO allocation                                |
| Save clicked                 | Voucher created, Customer Ledger credited, Invoice updated   |
| Full allocation on invoice   | Invoice status → Paid                                        |
| Partial allocation           | Invoice status → Partial, pending reduced                    |
| Advance (unallocated)        | Amount recorded as "On Account" in Ledger                    |

---

================================================================================

# 30.3 Payment Entry (Money OUT — to Vendor)

**Description:**
Form to record money paid to a vendor/supplier. The amount is allocated against one or more pending bills from Module 29. Handles TDS deduction at source. Layout mirrors the Receipt Entry (30.2) but with vendor-specific fields.

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
│  │ Vendor*          : [🔍 Search Vendor ▼]                              │  │
│  │ Current Balance  : ₹ 1,05,300 (We owe vendor)    [DISPLAY]           │  │
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
│                                                                              │
│  TDS DEDUCTION                                                               │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ TDS Applicable    : Yes — Section 194C                                │  │
│  │ TDS Rate          : 1%                                                │  │
│  │ TDS Amount        : ₹ 850     (Auto-calculated on taxable amount)     │  │
│  │ Net Payable       : ₹ 84,150                                         │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ALLOCATION SUMMARY                                                          │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Amount Paid           : ₹ 85,000                                      │  │
│  │ TDS Deducted          : ₹ 850                                         │  │
│  │ Allocated to Bills    : ₹ 85,000                                      │  │
│  │ ──────────────────────────────────────                                 │  │
│  │ Unallocated (Advance) : ₹ 0                                          │  │
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
| Vendor         | Search   | Yes      | Select vendor (fetches pending bills)             |
| Current Balance| Display  | Auto     | Total outstanding from Vendor Ledger              |

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

| Field        | Type    | Required | Description                                        |
| ------------ | ------- | -------- | -------------------------------------------------- |
| TDS Applicable| Display| Auto     | Fetched from vendor master (Module 11)             |
| TDS Rate     | Display | Auto     | Based on TDS section (194C = 1%, 194J = 10%, etc.) |
| TDS Amount   | Number  | Auto     | Auto-calculated on taxable amount                  |
| Net Payable  | Number  | Auto     | Amount Paid - TDS Deducted                         |

---

## Validation Rules

| Field           | Rule                                                          |
| --------------- | ------------------------------------------------------------- |
| Payment Date    | Cannot be a future date                                       |
| Vendor          | Must exist in Module 11 vendor list                           |
| Payment Mode    | Must select one option                                        |
| Bank Account    | Required for non-cash modes                                   |
| Ref / UTR       | Required for Bank/UPI. Must be unique across all vouchers     |
| Amount Paid     | Must be greater than 0                                        |
| Allocate Amount | Cannot exceed Pending Amount of individual bill               |
| TDS             | Auto-calculated; cannot be manually overridden unless Manager |

---

## System Behavior

| Event                      | System Action                                                |
| -------------------------- | ------------------------------------------------------------ |
| Vendor selected            | Fetches all pending bills and current balance                |
| TDS auto-calculated        | Based on vendor TDS section and rate                         |
| Save clicked               | Voucher created, Vendor Ledger debited, Bill updated         |
| Full allocation on bill    | Bill status → Paid                                           |
| Partial allocation         | Bill status → Partial, pending reduced                       |

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

| Event          | System Action                                              |
| -------------- | ---------------------------------------------------------- |
| Save clicked   | Source account debited, Destination account credited        |
| Voucher created| CNT-XXX number generated with audit log                    |

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
| Save clicked          | JRN-XXX number generated; all affected Ledgers updated  |

---

================================================================================

# 30.6 View Voucher Detail (Read-only)

**Description:**
Read-only view of any voucher (Receipt / Payment / Contra / Journal). Shows the full transaction details, linked invoices/bills, and audit trail.

---

## View Fields

| Field          | Type    | Description                                              |
| -------------- | ------- | -------------------------------------------------------- |
| Voucher #      | Text    | System-generated number                                  |
| Type           | Badge   | Receipt / Payment / Contra / Journal                     |
| Date           | Date    | Transaction date                                         |
| Party          | Text    | Customer / Vendor (or Self / Internal)                   |
| Amount         | Number  | Transaction amount                                       |
| Mode           | Text    | Cash / Bank / UPI / Cheque / Card                        |
| Ref / UTR      | Text    | Transaction reference number                             |
| Allocation     | Table   | Invoices/Bills adjusted with amounts                     |
| TDS Details    | Summary | If applicable, TDS section and amount                    |
| Notes          | Text    | Remarks                                                  |
| Audit Log      | List    | Created by, Date, Time                                   |

---

## Actions (View Screen)

| Action              | Type   | Description                                      |
| ------------------- | ------ | ------------------------------------------------ |
| **Download PDF**    | Button | Download formatted voucher PDF                   |
| **Print**           | Button | Print receipt or voucher                         |
| **Back to Register**| Button | Returns to Payment Register (30.1)              |

---

## Status Flow (Module 30 Overall)

```
  RECEIPT FLOW:
  Customer Pays → Receipt Created → Allocated to Invoice(s) → Ledger Updated
                                  → Unallocated → On Account (Advance)

  PAYMENT FLOW:
  We Pay Vendor → Payment Created → Allocated to Bill(s) → Ledger Updated
                                  → TDS Deducted → Net Payable reduced

  CONTRA FLOW:
  Cash → Bank / Bank → Cash / Bank → Bank → Both accounts updated

  JOURNAL FLOW:
  Debit Account(s) + Credit Account(s) → All Ledgers updated
  Rule: Total Dr = Total Cr (Always balanced)
```

---
