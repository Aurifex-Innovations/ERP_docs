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
| Invoice #    | Text   | Auto     | System-generated unique invoice number (INV-XXXXX)             |
| Invoice Date | Date   | Auto     | Date when invoice was created                                  |
| Customer     | Text   | Auto     | Customer name from Module 18                                   |
| SO #         | Link   | Auto     | Linked Sales Order number (clickable → opens Module 20)        |
| Invoice Amt  | Number | Auto     | Total invoice amount including taxes                           |
| Pending Amt  | Number | Auto     | Remaining unpaid amount (Invoice Amt - Received Amt)           |
| Due Date     | Date   | Auto     | Payment due date (Invoice Date + Credit Period)                |
| Status       | Badge  | Auto     | Draft / Sent / Partial / Paid / Overdue / Cancelled            |
| Actions      | Buttons| —        | View / Edit / Delete / Record Payment / Send / PDF / Approve   |

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
| Customer     | Search       | Search by customer name (from Module 18)               |
| Status       | Multi-select | Draft / Sent / Partial / Paid / Overdue / Cancelled    |
| Invoice Type | Multi-select | Tax Invoice / Proforma / Credit Note                   |
| Date Range   | Date Range   | From – To                                              |
| SO Number    | Text         | Filter by linked Sales Order number                    |

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
| **Approve & Send** | Button | Draft only             | Finalizes invoice, updates Ledger, sends to customer   |
| **Record Payment** | Button | Sent / Partial / Overdue | Redirects to Module 30 with invoice pre-selected      |
| **Send**           | Button | Sent / Overdue         | Re-send invoice via Email / WhatsApp                   |
| **PDF**            | Button | Sent / Paid            | Download invoice as PDF                                |
| **Receipt**        | Button | Paid                   | View/download payment receipt from Module 30           |

---

## Form Actions

| Action              | Description                                               |
| ------------------- | --------------------------------------------------------- |
| **+ Create Invoice**| Opens the **Create Invoice Form** (Screen 28.2)           |
| **Export PDF Batch** | Download multiple selected invoices as ZIP of PDFs        |
| **Tally Export**     | Export invoice data in Tally-compatible XML/JSON format   |

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
| Invoice Approved & Sent      | Customer Ledger debited, Invoice status → Sent                 |
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
│  │ Sales Order*  : [🔍 Search SO # / Customer ▼]     [FETCH DETAILS]    │  │
│  │               (Shows only SOs with status = Confirmed/In Progress)    │  │
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
│  ┌───┬────────────┬────────┬─────┬──────┬──────┬──────┬─────┬────────────┐  │
│  │Sr │Description │HSN/SAC │Qty  │UOM   │Rate  │Disc% │Tax% │Amount      │  │
│  │───┼────────────┼────────┼─────┼──────┼──────┼──────┼─────┼────────────│  │
│  │ 1 │Cockroach   │998531  │  1  │Visit │2,500 │ 0%   │18%  │₹ 2,950    │  │
│  │   │Treatment   │        │     │      │      │      │     │            │  │
│  │ 2 │Termite     │998531  │  1  │Visit │5,000 │ 5%   │18%  │₹ 5,605    │  │
│  │   │Control     │        │     │      │      │      │     │            │  │
│  │ 3 │Rodent Bait │392690  │  5  │Nos   │  200 │ 0%   │12%  │₹ 1,120    │  │
│  │   │Box         │        │     │      │      │      │     │            │  │
│  └───┴────────────┴────────┴─────┴──────┴──────┴──────┴─────┴────────────┘  │
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
| Sales Order    | Search       | Cond.    | Required if mode = From SO. Fetches SO details        |
| Invoice Type   | Dropdown     | Yes      | Tax Invoice / Proforma Invoice                        |
| Invoice Date   | Date Picker  | Yes      | Defaults to today. Cannot be future date              |
| Credit Period  | Number       | Yes      | Days allowed for payment (default from Contract)      |
| Due Date       | Date (Auto)  | Auto     | Calculated: Invoice Date + Credit Period              |
| Branch         | Dropdown     | Yes      | Auto-filled from SO, editable for direct invoices     |

---

## Screen Fields: Customer Details

| Field           | Type    | Required | Description                                         |
| --------------- | ------- | -------- | --------------------------------------------------- |
| Customer Name   | Display | Auto     | Fetched from Module 18                              |
| GSTIN           | Display | Auto     | Customer's GST number from Module 18                |
| Billing Address | Display | Auto     | Default billing address from Module 18              |
| State           | Display | Auto     | Customer state (determines CGST/SGST vs IGST)      |
| Contact Person  | Display | Auto     | Primary contact from Module 18                      |

---

## Screen Fields: Line Items Grid

| Field       | Type    | Required | Description                                           |
| ----------- | ------- | -------- | ----------------------------------------------------- |
| Sr. No      | Number  | Auto     | Sequential row number                                 |
| Description | Text    | Yes      | Service/Product name (auto-filled from SO if linked)  |
| HSN/SAC     | Text    | Yes      | HSN code for products, SAC code for services          |
| Qty         | Number  | Yes      | Quantity (default 1 for services)                     |
| UOM         | Text    | Auto     | Unit from Product Master (Nos / Ltr / Visit / Kg)    |
| Rate        | Number  | Yes      | Per-unit price (auto from SO, editable)               |
| Discount %  | Number  | No       | Line-level discount percentage (default 0)            |
| Tax %       | Number  | Auto     | GST rate from Module 9 based on HSN/SAC code         |
| Amount      | Number  | Auto     | Calculated: (Qty × Rate - Discount) + Tax            |

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
| **Approve & Send** | Finalizes invoice, generates PDF, updates Ledger, sends to customer |
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
│  PAYMENT HISTORY                                                             │
│  ┌──────────┬──────────┬──────────┬────────────┬──────────────────────────┐  │
│  │Receipt # │Date      │Amount    │Mode        │Remarks                   │  │
│  │──────────┼──────────┼──────────┼────────────┼──────────────────────────│  │
│  │(No payments received yet)                                              │  │
│  └──────────┴──────────┴──────────┴────────────┴──────────────────────────┘  │
│                                                                              │
│  AUDIT LOG                                                                   │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │ 15 Mar 2026 10:30 — Created by Amit Shah (Draft)                    │    │
│  │ 15 Mar 2026 11:15 — Approved & Sent by Priya Patel                 │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  [📥 DOWNLOAD PDF]  [✉ RESEND]  [🧾 RECORD PAYMENT]  [📄 CREDIT NOTE]     │
│  [🔙 BACK TO LIST]                                                          │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## View Fields

| Field         | Type    | Description                                                |
| ------------- | ------- | ---------------------------------------------------------- |
| Status        | Badge   | Current invoice status                                     |
| From / To     | Display | Company details and Customer details side by side          |
| Invoice #     | Text    | System-generated number                                    |
| SO Reference  | Link    | Clickable link to Sales Order (Module 20)                  |
| Contract Ref  | Link    | Clickable link to Contract (Module 19)                     |
| Line Items    | Table   | All billed items with HSN, rate, tax, and amount           |
| Tax Summary   | Summary | Subtotal, Discounts, CGST/SGST/IGST breakdown             |
| Payment History| Table  | Linked receipts from Module 30                             |
| Audit Log     | List    | Chronological log of all actions on this invoice           |

---

## Actions (View Screen)

| Action              | Type   | Condition          | Description                                      |
| ------------------- | ------ | ------------------ | ------------------------------------------------ |
| **Download PDF**    | Button | Sent / Paid        | Download formatted invoice PDF                   |
| **Resend**          | Button | Sent / Overdue     | Re-send via Email or WhatsApp                    |
| **Record Payment**  | Button | Sent / Partial     | Redirect to Module 30 with this invoice          |
| **Credit Note**     | Button | Sent / Paid        | Opens Credit Note form (Screen 28.5)             |
| **Back to List**    | Button | All                | Returns to Invoice Dashboard (28.1)              |

---

================================================================================

# 28.4 Edit Invoice (Draft Only)

**Description:**
Allows editing of an invoice that is still in **Draft** status. Once an invoice is Approved & Sent, it cannot be edited — a Credit Note must be issued instead. The edit form has the same layout as the Create Invoice form (28.2) with all fields pre-populated.

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

# 28.5 Credit Note

**Description:**
Used to reduce the value of a previously issued and sent invoice. Common scenarios: customer dispute, partial return, pricing error, or service cancellation. The Credit Note links to the original invoice and adjusts the Customer Ledger.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         CREATE CREDIT NOTE                                   │
│                                                                              │
│  ORIGINAL INVOICE                                                            │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Invoice #      : INV-10024                                            │  │
│  │ Customer       : ABC Corp Ltd                                         │  │
│  │ Invoice Amount : ₹ 10,561                                             │  │
│  │ Pending Amount : ₹ 10,561                                             │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  CREDIT NOTE DETAILS                                                         │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Credit Note Date* : [📅 28 Mar 2026]                                  │  │
│  │ Reason*           : [▼ Select Reason ▼]                               │  │
│  │                     (Pricing Error / Service Issue / Partial Return /  │  │
│  │                      Full Cancellation / Customer Dispute / Other)     │  │
│  │ Remarks           : [_____________________________________________]   │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  LINE ITEMS TO CREDIT                                                        │
│  ┌───┬────────────────────┬─────┬──────┬──────┬──────────┬──────────────┐  │
│  │ ☑ │Description         │Qty  │Rate  │Tax%  │Orig Amt  │Credit Amt    │  │
│  │───┼────────────────────┼─────┼──────┼──────┼──────────┼──────────────│  │
│  │ ☑ │Cockroach Treatment │  1  │2,500 │ 18%  │₹ 2,950   │₹ 2,950      │  │
│  │ ☐ │Termite Control     │  1  │5,000 │ 18%  │₹ 5,605   │—             │  │
│  │ ☐ │Rodent Bait Box     │  5  │  200 │ 12%  │₹ 1,120   │—             │  │
│  └───┴────────────────────┴─────┴──────┴──────┴──────────┴──────────────┘  │
│                                                                              │
│  CREDIT SUMMARY                                                              │
│  ┌──────────────────────────────────────────────────────────────┐            │
│  │ Total Credit Amount  : ₹ 2,950                               │            │
│  │ Adjusted Invoice Amt : ₹ 10,561 - ₹ 2,950 = ₹ 7,611        │            │
│  └──────────────────────────────────────────────────────────────┘            │
│                                                                              │
│  [ISSUE CREDIT NOTE]    [CANCEL]                                             │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Screen Fields

| Field            | Type       | Required | Description                                        |
| ---------------- | ---------- | -------- | -------------------------------------------------- |
| Original Invoice | Display    | Auto     | Reference invoice details (read-only)              |
| Credit Note Date | Date       | Yes      | Date of credit note (cannot be before invoice date)|
| Reason           | Dropdown   | Yes      | Predefined reasons for credit                      |
| Remarks          | Textarea   | No       | Additional explanation                             |
| Line Item Select | Checkbox   | Yes      | Select which items to credit (min 1)               |
| Credit Amount    | Number     | Auto     | Auto-calculated based on selected items            |

---

## Validation Rules

| Field            | Rule                                                       |
| ---------------- | ---------------------------------------------------------- |
| Credit Note Date | Cannot be before original invoice date                     |
| Reason           | Must select from dropdown                                  |
| Line Items       | At least 1 item must be selected                           |
| Credit Amount    | Cannot exceed original invoice amount                      |

---

## System Behavior

| Event                      | System Action                                             |
| -------------------------- | --------------------------------------------------------- |
| Credit Note issued         | CN number generated (CN-XXXXX)                            |
| Ledger adjustment          | Customer Ledger credited by CN amount                     |
| Invoice pending reduced    | Original invoice's pending amount reduced                 |
| GST reversal               | Tax components reversed proportionally                    |

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
