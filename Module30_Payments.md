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
