# 🎯 MODULE 29: BILLS (PURCHASES)

## Overview

Bills module manages all **Accounts Payable** operations — recording, tracking, and managing purchase bills received from vendors/suppliers. Supports both **Purchase Order (PO) linked bills** and **direct expense bills** (rent, utilities, office). Implements **3-Way Matching** (PO → GRN → Bill) for stock purchases to ensure integrity. Handles vendor TDS deduction, GST Input Tax Credit (ITC), and Debit Notes for purchase returns.

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
- 29.5 Debit Note (Purchase Return)

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
│  │ Status     : [☑ Draft ☑ Pending ☑ Approved ☑ Paid ☑ Overdue]       │  │
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
│  │ Total        │ Overdue      │ Paid         │ Pending      │               │
│  │ Payable      │              │ (This Month) │ Approval     │               │
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
│  ┌──────────┬──────────────┬─────────────┬───────────────────────────────┐   │
│  │Due Date  │Status        │3-Way Match  │Actions                        │   │
│  │──────────┼──────────────┼─────────────┼───────────────────────────────│   │
│  │10 Apr 26 │🟡 PENDING    │✅ Matched   │[View] [Approve] [Send Back]  │   │
│  │12 Apr 26 │🟢 PAID       │✅ Matched   │[View] [PDF] [Payment Ref]    │   │
│  │—         │⚪ DRAFT      │— (Expense)  │[View] [Edit] [Delete]        │   │
│  └──────────┴──────────────┴─────────────┴───────────────────────────────┘   │
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   10   Next                         │
│                                                                              │
│  Legend: ⚪ Draft  🟡 Pending  🟢 Paid  🟠 Partial  🔴 Overdue  ⛔ Rejected │
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
| Status       | Badge  | Auto     | Draft / Pending Approval / Approved / Paid / Partial / Overdue / Rejected |
| 3-Way Match  | Badge  | Auto     | Matched ✅ / Mismatch ⚠️ / N/A (for expense bills)          |
| Actions      | Buttons| —        | View / Edit / Delete / Approve / Send Back / PDF             |

---

## Summary Card Fields

| Field           | Type   | Description                                               |
| --------------- | ------ | --------------------------------------------------------- |
| Total Payable   | Number | Sum of all unpaid bill amounts                            |
| Overdue         | Number | Sum of bills past due date and not fully paid             |
| Paid (Month)    | Number | Total vendor payments this month                          |
| Pending Approval| Number | Count of bills awaiting manager approval                  |

---

## Filters

| Filter     | Type         | Options                                              |
| ---------- | ------------ | ---------------------------------------------------- |
| Branch     | Dropdown     | All Branches / Specific Branch (from Module 7)       |
| Vendor     | Search       | Search by vendor name (from Module 11)               |
| Status     | Multi-select | Draft / Pending / Approved / Paid / Overdue / Rejected |
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
| **Approve**      | Button | Pending Approval     | Approves bill, updates Vendor Ledger                    |
| **Send Back**    | Button | Pending Approval     | Rejects with remarks, sends back to creator             |
| **Make Payment** | Button | Approved / Overdue   | Redirects to Module 30 with bill pre-selected           |
| **PDF**          | Button | Approved / Paid      | Download bill copy as PDF                               |
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
| Draft bills do not affect Ledger    | Only Approved bills update the Vendor Ledger                 |
| Approval required for > ₹10,000    | Bills above ₹10,000 require manager approval                |
| 3-Way Match for PO-linked bills     | PO quantity/amount vs GRN received vs Bill charged must match |
| Auto-Draft from Stock Entry         | When stock is added in Module 11, a Draft Bill is auto-created |
| Overdue auto-detection              | Bills marked Overdue when Due Date passes without full payment |
| TDS auto-calculation                | If vendor is TDS applicable, TDS is auto-deducted            |

---

## System Behavior

| Event                           | System Action                                                |
| ------------------------------- | ------------------------------------------------------------ |
| Bill Approved                   | Vendor Ledger credited, Bill status → Approved               |
| Full payment made               | Bill status → Paid, Pending Amt → 0                          |
| Partial payment made            | Bill status → Partial, Pending Amt reduced                   |
| Due date passes (unpaid)        | Bill status → Overdue, Notification to accounts team         |
| 3-Way Mismatch detected         | Warning icon ⚠️ shown, Approval blocked until resolved       |
| Bill Rejected (Send Back)       | Status → Rejected, Creator receives notification             |

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
│  3-WAY MATCH STATUS (For PO-linked bills)                                    │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ PO Quantity    : 100 Ltr    │ GRN Received : 100 Ltr  │ ✅ Match     │  │
│  │ PO Amount      : ₹ 80,000   │ Bill Amount  : ₹ 85,000 │ ⚠️ Mismatch │  │
│  │                                                                       │  │
│  │ ⚠️ Bill amount exceeds PO by ₹ 5,000 — Requires manager approval     │  │
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
│  [SAVE AS DRAFT]    [SUBMIT FOR APPROVAL]    [CANCEL]                        │
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

## 3-Way Match Logic

| Check             | PO Value   | GRN Value  | Bill Value | Result         |
| ----------------- | ---------- | ---------- | ---------- | -------------- |
| Quantity           | 100 Ltr    | 100 Ltr    | 100 Ltr    | ✅ Match       |
| Quantity           | 100 Ltr    | 95 Ltr     | 100 Ltr    | ⚠️ GRN Short   |
| Amount             | ₹80,000    | —          | ₹80,000    | ✅ Match       |
| Amount             | ₹80,000    | —          | ₹85,000    | ⚠️ Bill Exceeds|

**Mismatch Handling:**
- System shows warning banner with the mismatch details.
- Bill can be saved as Draft but CANNOT be approved until mismatch is resolved or overridden by Manager.

---

## Business Rules

| Rule                         | Description                                                     |
| ---------------------------- | --------------------------------------------------------------- |
| Duplicate Bill Check         | System warns if same Vendor Bill # exists for the same vendor   |
| TDS Deduction                | If vendor has TDS flag, TDS auto-deducted before payable calc   |
| GST ITC (Input Tax Credit)   | Tax paid on purchase bills is recorded as Input Credit           |
| Auto-Draft from Module 11    | Stock entry in Module 11 auto-creates a Draft bill              |
| Approval Hierarchy           | Bills > ₹10,000 need Accounts Manager approval                 |
| Expense categorization       | Expense bills must be mapped to COA account head (Module 32)    |

---

## Form Actions

| Action                     | Description                                                   |
| -------------------------- | ------------------------------------------------------------- |
| **Save as Draft**          | Saves bill without submitting. No Ledger impact               |
| **Submit for Approval**    | Sends bill to manager queue. Status → Pending Approval        |
| **Cancel**                 | Discards changes and returns to Dashboard (29.1)              |

---

## System Behavior

| Event                        | System Action                                                 |
| ---------------------------- | ------------------------------------------------------------- |
| PO Selected (Fetch)          | Auto-populates Vendor, Line Items, GRN quantities             |
| 3-Way Match runs             | Compares PO vs GRN vs Bill; shows match/mismatch status       |
| Submit for Approval clicked  | Bill moves to approval queue; notification to manager         |
| Bill Approved                | Vendor Ledger credited; GST ITC recorded; Status → Approved   |
| Bill Rejected (Send Back)    | Status → Rejected; Creator notified with remarks              |

---

================================================================================

# 29.3 View Bill Detail (Read-only)

**Description:**
A read-only screen showing the complete bill with all details — vendor info, line items, tax/TDS breakdown, 3-way match status, payment history, and audit trail. Shows linked PO, GRN references, and Debit Notes if any.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       BILL DETAIL — BILL-5524                                │
│                                                                              │
│  Status: 🟡 PENDING APPROVAL        Due Date: 10 Apr 2026                   │
│  3-Way Match: ⚠️ Mismatch (₹5,000 excess)                                  │
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
│  PAYMENT HISTORY                                                             │
│  ┌──────────┬──────────┬──────────┬────────────┬──────────────────────────┐  │
│  │Voucher # │Date      │Amount    │Mode        │Status                    │  │
│  │──────────┼──────────┼──────────┼────────────┼──────────────────────────│  │
│  │(No payments made yet)                                                  │  │
│  └──────────┴──────────┴──────────┴────────────┴──────────────────────────┘  │
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
│  │ 10 Mar 2026 10:00 — Submitted for Approval by Suresh Kumar         │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  [📥 DOWNLOAD PDF]  [✅ APPROVE]  [🔙 SEND BACK]  [🧾 MAKE PAYMENT]       │
│  [📄 DEBIT NOTE]  [🔙 BACK TO LIST]                                        │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## View Fields

| Field           | Type    | Description                                               |
| --------------- | ------- | --------------------------------------------------------- |
| Status          | Badge   | Current bill status                                       |
| 3-Way Match     | Badge   | Match / Mismatch indicator with details                   |
| From / To       | Display | Vendor details and Company details side by side           |
| Vendor Bill #   | Text    | Vendor's own invoice number                               |
| Our Bill #      | Text    | Our system-generated number                               |
| PO Reference    | Link    | Clickable link to Purchase Order (Module 11)              |
| GRN Reference   | Link    | Clickable link to Goods Receipt Note                      |
| Line Items      | Table   | All billed items with HSN, rate, tax, and amount          |
| Tax & TDS       | Summary | Tax breakdown with TDS deduction                          |
| Payment History | Table   | Linked payment vouchers from Module 30                    |
| Attached Docs   | Links   | Uploaded bill copies with View/Download                   |
| Audit Log       | List    | Chronological log of all actions                          |

---

## Actions (View Screen)

| Action           | Type   | Condition          | Description                                      |
| ---------------- | ------ | ------------------ | ------------------------------------------------ |
| **Approve**      | Button | Pending Approval   | Approve bill, update Vendor Ledger               |
| **Send Back**    | Button | Pending Approval   | Reject with remarks                              |
| **Make Payment** | Button | Approved / Overdue | Redirect to Module 30 with bill pre-selected     |
| **Debit Note**   | Button | Approved / Paid    | Opens Debit Note form (Screen 29.5)              |
| **Download PDF** | Button | All except Draft   | Download bill details as PDF                     |
| **Back to List** | Button | All                | Returns to Bills Dashboard (29.1)                |

---

================================================================================

# 29.4 Edit Bill (Draft Only)

**Description:**
Allows editing of a bill that is still in **Draft** status. Once submitted for approval, the bill cannot be edited — it must be Sent Back by the manager first. The edit form has the same layout as Add Bill (29.2) with all fields pre-populated.

---

## Business Rules

| Rule                            | Description                                              |
| ------------------------------- | -------------------------------------------------------- |
| Edit allowed only for Draft     | Pending/Approved/Paid bills cannot be modified           |
| Audit trail maintained          | Every edit is logged with user name and timestamp        |
| Re-save as Draft                | Changes saved without affecting the Ledger               |
| Submit from Edit                | Draft can be submitted for approval from edit screen     |

---

## System Behavior

| Event                     | System Action                                              |
| ------------------------- | ---------------------------------------------------------- |
| User opens Edit           | All fields loaded from saved Draft data                    |
| User modifies line items  | Tax/TDS breakdown auto-recalculates                        |
| 3-Way Match re-runs       | If PO linked, match status updates in real-time            |
| Save as Draft pressed     | Updates existing draft, no Ledger change                   |
| Submit for Approval       | Moves to approval queue, manager notified                  |

---

================================================================================

# 29.5 Debit Note (Purchase Return)

**Description:**
Used when goods received are damaged, incorrect, or need to be returned to the vendor. The Debit Note reduces the value of the original Purchase Bill and adjusts the Vendor Ledger. Similar to Credit Note in Module 28 but for the purchase side.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         CREATE DEBIT NOTE                                    │
│                                                                              │
│  ORIGINAL BILL                                                               │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Bill #        : BILL-5524                                             │  │
│  │ Vendor        : Industrial Chemicals Pvt Ltd                          │  │
│  │ Bill Amount   : ₹ 1,06,200                                           │  │
│  │ Pending Amount: ₹ 1,05,300 (after TDS)                               │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  DEBIT NOTE DETAILS                                                          │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Debit Note Date* : [📅 28 Mar 2026]                                   │  │
│  │ Reason*          : [▼ Select Reason ▼]                                │  │
│  │                    (Damaged Goods / Wrong Item / Quality Issue /       │  │
│  │                     Pricing Error / Short Received / Other)            │  │
│  │ Remarks          : [_____________________________________________]    │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  LINE ITEMS TO DEBIT                                                         │
│  ┌───┬────────────────────┬─────┬──────┬──────┬──────────┬──────────────┐  │
│  │ ☑ │Description         │Qty  │Rate  │Tax%  │Orig Amt  │Debit Amt     │  │
│  │───┼────────────────────┼─────┼──────┼──────┼──────────┼──────────────│  │
│  │ ☑ │Chemical X          │ 10  │1,200 │ 18%  │₹ 14,160  │₹ 14,160     │  │
│  │ ☐ │Chemical Y          │ 50  │  600 │ 18%  │₹ 35,400  │—             │  │
│  └───┴────────────────────┴─────┴──────┴──────┴──────────┴──────────────┘  │
│                                                                              │
│  DEBIT SUMMARY                                                               │
│  ┌──────────────────────────────────────────────────────────────┐            │
│  │ Total Debit Amount    : ₹ 14,160                             │            │
│  │ Adjusted Bill Payable : ₹ 1,06,200 - ₹ 14,160 = ₹ 92,040   │            │
│  └──────────────────────────────────────────────────────────────┘            │
│                                                                              │
│  [ISSUE DEBIT NOTE]    [CANCEL]                                              │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Screen Fields

| Field            | Type       | Required | Description                                        |
| ---------------- | ---------- | -------- | -------------------------------------------------- |
| Original Bill    | Display    | Auto     | Reference bill details (read-only)                 |
| Debit Note Date  | Date       | Yes      | Cannot be before original bill date                |
| Reason           | Dropdown   | Yes      | Predefined reasons for debit                       |
| Remarks          | Textarea   | No       | Additional explanation                             |
| Line Item Select | Checkbox   | Yes      | Select which items to debit (min 1)                |
| Return Qty       | Number     | Yes      | Qty being returned (cannot exceed original)        |
| Debit Amount     | Number     | Auto     | Auto-calculated based on selected items and qty    |

---

## Validation Rules

| Field            | Rule                                                       |
| ---------------- | ---------------------------------------------------------- |
| Debit Note Date  | Cannot be before original bill date                        |
| Reason           | Must select from dropdown                                  |
| Line Items       | At least 1 item must be selected                           |
| Return Qty       | Cannot exceed original billed quantity                     |
| Debit Amount     | Cannot exceed original bill amount                         |

---

## System Behavior

| Event                      | System Action                                             |
| -------------------------- | --------------------------------------------------------- |
| Debit Note issued          | DN number generated (DN-XXXXX)                            |
| Ledger adjustment          | Vendor Ledger debited by DN amount                        |
| Bill pending reduced       | Original bill's pending amount reduced                    |
| GST ITC reversal           | Input Tax Credit reversed proportionally                  |
| Stock return               | If physical return, stock adjusted in Module 11           |

---

## Status Flow (Module 29 Overall)

```
                    ┌──────────┐
                    │  DRAFT   │
                    └────┬─────┘
                         │ (Submit for Approval)
                         ▼
                    ┌───────────────┐
                    │   PENDING     │
                    │   APPROVAL    │
                    └────┬────┬─────┘
                         │    │
                    ┌────┘    └────┐
                    ▼              ▼
             ┌──────────┐  ┌──────────┐
             │ APPROVED │  │ REJECTED │
             └────┬─────┘  └──────────┘
                  │
             ┌────┴─────┐
             ▼          ▼
      ┌──────────┐ ┌──────────┐
      │ PARTIAL  │ │ OVERDUE  │
      │ (Payment)│ │(Due Date)│
      └────┬─────┘ └────┬─────┘
           │            │
           ▼            ▼
      ┌──────────────────┐
      │      PAID        │
      └──────────────────┘

  Side flows:
  DRAFT → DELETED
  APPROVED/PAID → DEBIT NOTE ISSUED
```

---
