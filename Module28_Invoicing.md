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
| Invoice No   | Text   | Auto     | System-generated unique invoice number (INV-XXXXX)             |
| Invoice Date | Date   | Auto     | Date when invoice was created                                  |
| Customer     | Text   | Auto     | Customer name from Module 18                                   |
| SO No        | Link   | Auto     | Linked Sales Order number (clickable → opens Module 20)        |
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
| **Resend**           | Button | Sent / Overdue         | Re-send invoice via Email / WhatsApp                   |
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
│  │ ── If "From Sales Order" selected ──                                  │  │
│  │ Sales Order*  : [🔍 Search SO # / Customer ▼]     [FETCH DETAILS]    │  │
│  │               (Shows only SOs with status = Confirmed/In Progress)    │  │
│  │                                                                       │  │
│  │ ── If "Direct Invoice" selected ──                                    │  │
│  │ Customer*     : [🔍 Search Customer Name / Code ▼]  [FETCH DETAILS]  │  │
│  │               (Fetches billing details from Module 18)                │  │
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
│  ┌───┬──────┬────────────┬────────┬─────┬──────┬──────┬──────┬─────┬───────┐  │
│  │Sr │Type  │Description │HSN/SAC │Qty  │UOM   │Rate  │Disc% │Tax% │Amount │  │
│  │───┼──────┼────────────┼────────┼─────┼──────┼──────┼──────┼─────┼───────│  │
│  │ 1 │Svc   │Cockroach   │998531  │  1  │ —    │2,500 │ 0%   │18%  │₹2,950│  │
│  │   │      │Treatment   │        │     │      │      │      │     │      │  │
│  │ 2 │Svc   │Termite     │998531  │  1  │ —    │5,000 │ 5%   │18%  │₹5,605│  │
│  │   │      │Control     │        │     │      │      │      │     │      │  │
│  │ 3 │Prod  │Rodent Bait │392690  │  5  │PKT   │  200 │ 0%   │12%  │₹1,120│  │
│  │   │      │Box         │        │     │      │      │      │     │      │  │
│  └───┴──────┴────────────┴────────┴─────┴──────┴──────┴──────┴─────┴───────┘  │
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
| Sales Order    | Search       | Cond.    | Required if mode = From SO. Fetches SO details. Hidden if mode = Direct |
| Customer       | Search       | Cond.    | Required if mode = Direct. Search by Name/Code. Fetches billing details from Module 18. Hidden if mode = From SO |
| Invoice Type   | Dropdown     | Yes      | Tax Invoice / Proforma Invoice                        |
| Invoice Date   | Date Picker  | Yes      | Defaults to today. Cannot be future date              |
| Credit Period  | Number       | Yes      | Days allowed for payment (default from Contract)      |
| Due Date       | Date (Auto)  | Auto     | Calculated: Invoice Date + Credit Period              |
| Branch         | Dropdown     | Yes      | Auto-filled from SO, editable for direct invoices     |

---

## Screen Fields: Customer Details

| Field           | Type     | Required | Description                                         |
| --------------- | -------- | -------- | --------------------------------------------------- |
| Customer Name   | Display  | Auto     | Fetched from Module 18 — Customer Master            |
| GSTIN           | Display  | Auto     | Customer's GST number from Module 18                |
| Billing Address | Display  | Auto     | Default billing address from Module 18              |
| State           | Dropdown | Auto     | **Default:** Auto-fetched from Module 18 billing address. **User can override** via `[Change ▼]` to select a different state (e.g., for inter-branch billing). Changing state **re-triggers tax logic** (CGST/SGST ↔ IGST). Options: All Indian states & UTs from Module 9 (Tax Config) |
| Contact Person  | Display  | Auto     | Primary contact from Module 18                      |

> **Data Source:** The entire Customer Details section is auto-populated when a Sales Order is fetched (mode = From SO) or when a Customer is selected (mode = Direct). All data originates from **Module 18 — Customer Master** (billing address, GSTIN, contact). The State field additionally references **Module 9 — Tax Configuration** for the dropdown list and for determining CGST/SGST vs IGST split.

---

## Screen Fields: Line Items Grid

<<<<<<< HEAD
| Field       | Type    | Required | Description                                           |
| ----------- | ------- | -------- | ----------------------------------------------------- |
| Sr. No      | Number  | Auto     | Sequential row number                                 |
| Description | Text    | Yes      | Service/Product name (auto-filled from SO if linked)  |
| HSN/SAC     | Text    | Yes      | HSN code for products, SAC code for services          |
| Qty         | Number  | Yes      | Quantity (default 1 for services)                     |
| UOM         | Text    | Auto     | Unit from Product Master                              |
| Rate        | Number  | Yes      | Per-unit price (auto from SO, editable)               |
| Discount %  | Number  | No       | Line-level discount percentage (default 0)            |
| Tax %       | Number  | Auto     | GST rate from Module 9 based on HSN/SAC code         |
| Amount      | Number  | Auto     | Calculated: (Qty × Rate - Discount) + Tax            |
=======
| Field       | Type     | Required | Description                                           |
| ----------- | -------- | -------- | ----------------------------------------------------- |
| Sr. No      | Number   | Auto     | Sequential row number                                 |
| Type        | Tag/Badge| Auto     | Indicates item source: **Svc** (Service — Module 12) or **Prod** (Product — Module 10). Auto-determined when item is added from SO; user selects when adding manually |
| Description | Text     | Yes      | **Product:** `productName` from Module 10. **Service:** `serviceName` from Module 12. Auto-filled from SO line items if linked; manual entry for direct invoices |
| HSN/SAC     | Text     | Yes      | **Product:** `hsnCode` from Module 10. **Service:** SAC code from Module 12 service category. Auto-fetched based on selected item |
| Qty         | Number   | Yes      | Quantity of items. Default 1 for services. For products, must be ≤ available stock (Module 10) |
| UOM         | Dropdown | Auto     | **Product only:** `baseUom` from Module 10 — Options: `LTR` / `KG` / `GRAM` / `ML` / `SET` / `PKT`. **Service:** UOM is not applicable (shows `—`), as services are billed per visit/contract from Module 12 pricing model |
| Rate        | Number   | Yes      | **Product:** `sellingPrice` from Module 10. **Service:** Rate determined by Module 12 `priceType` (Fixed / Area-Based / Inspection). Auto-filled from SO, editable by user |
| Discount %  | Number   | No       | Line-level discount percentage (default 0)            |
| Tax %       | Number   | Auto     | GST rate from Module 9 (Tax Config) based on HSN/SAC code |
| Amount      | Number   | Auto     | Calculated: (Qty × Rate − Discount) + Tax            |

> **Data Source Mapping:**
> - **From SO (Mode = From Sales Order):** Line items auto-populated from **Module 20 — Sales Order** line items. Each SO line item already references either a Product (Module 10) or Service (Module 12), so all fields (Description, HSN/SAC, UOM, Rate) are pre-filled.
> - **Direct Invoice (Mode = Direct):** User manually searches and selects items from **Module 10 — Product Master** or **Module 12 — Service Master**. On selection, Description, HSN/SAC, UOM, and Rate are auto-fetched from the respective module.
> - **UOM Note:** UOM applies only to products (physical goods measured in LTR, KG, GRAM, ML, SET, PKT as defined in Module 10). Services are priced per visit/contract via Module 12 pricing models and do not use physical measurement UOM.

**Grid Action Buttons (Below the line items table):**

| Button                | Action                                                                        |
| --------------------- | ----------------------------------------------------------------------------- |
| **[+ ADD LINE ITEM]** | Opens the **Add Line Item Modal** (see below) to search and select from Module 10/12. |
| **[🗑 REMOVE SELECTED]** | Removes the currently selected/highlighted row(s). Disabled if no row is selected. At least 1 line item must remain (cannot remove all rows) |

### Add Line Item Modal (Popup)

**Description:**
Triggered by clicking `[+ ADD LINE ITEM]`. Allows the user to specify whether they are adding a Service or a Product, search the respective master data (Module 12 or Module 10), preview the details, and add it to the invoice grid.

**Modal Wireframe:**
```
┌──────────────────────────────────────────────────────────────────┐
│  ADD LINE ITEM                                               [X] │
│  ──────────────────────────────────────────────────────────────  │
│                                                                  │
│  Item Type*  :  (•) Service          ( ) Product                 │
│                 (Fetches Mod 12)     (Fetches Mod 10)            │
│  Search Item*:  [🔍 Search by Name / Code ▼]                     │
│                                                                  │
│  ────────────────── ITEM DETAILS & PRICING ────────────────────  │
│  Name           : Termite Barrier                                │
│  SAC/HSN        : 998531                                         │
│  Tax %          : 18%                                            │
│                                                                  │
│  ── Dynamic Form Based on Price Type (From Mod 12) ────────────  │
│                                                                  │
│  == [IF Service Price Type = FIXED_PRICE] =====================  │
│  Category       : [▼ Residential (Internal/External) ]           │
│  Property Type  : [▼ 1BHK ]                                      │
│  Predefined Rate: ₹ 1,500                                        │
│  ==============================================================  │
│                                                                  │
│  == [IF Service Price Type = AREA_BASED] ======================  │
│  Category       : [▼ Commercial (Internal/External)  ]           │
│  Base Price     : ₹ 500.00                                       │
│  Rate Per Sq.Ft : ₹ 2.00                                         │
│  Input Area*    : [   1000  ] SQFT                               │
│  Calculated     : ₹ 500 + (1000 × ₹ 2) = ₹ 2,500                 │
│  ==============================================================  │
│                                                                  │
│  == [IF Service Price Type = INSPECTION_BASED] ================  │
│  Inspection Fee : ₹ 500 (Final price quoted after visit)         │
│  ==============================================================  │
│                                                                  │
│  == [IF Service Price Type = CUSTOM] ==========================  │
│  Config Name    : [▼ Select Custom Config (e.g. Warehouse) ]     │
│  Rate           : — (Manual Entry Required)                      │
│  ==============================================================  │
│                                                                  │
│  == [IF Item Type = Product] ==================================  │
│  UOM            : LTR / KG / Nos (From Mod 10)                   │
│  Selling Price  : ₹ 1,200        (From Mod 10)                   │
│  Stock Available: 50             (From Mod 11 Inventory)         │
│  ==============================================================  │
│                                                                  │
│  Final Rate (₹)*: [  2500  ] (Editable override / manual input)  │
│  Quantity*      : [  1     ]                                     │
│  ──────────────────────────────────────────────────────────────  │
│                                                                  │
│                   [CANCEL]    [ADD TO INVOICE]                   │
└──────────────────────────────────────────────────────────────────┘
```

**Modal Fields & Actions:**

| Field / Action | Description |
| -------------- | ----------- |
| **Item Type** | Radio buttons. Defaults to Service. Dictates which API is called for the search field below. |
| **Search Item** | Auto-complete search. If Type=Service, searches **Module 12** active services. If Type=Product, searches **Module 10** active inventory products. |
| **Item Details** | Read-only preview showing Name, SAC/HSN, UOM, and Tax%. |
| **Pricing Calculation** | **Dynamic Form Based on Price Type (From Mod 12):**<br><br>• **FIXED PRICE:** Renders dropdown for Category (Residential/Commercial) and Property Type (1BHK, 2BHK, Small Office). Auto-fetches the predefined rate.<br><br>• **AREA_BASED:** Renders dropdown for Category. Shows the predefined `Base Price` + `Rate per SQFT`. Renders an `Input Area (SQFT)` field. Auto-calculates `Base Price + (Area × Rate)`.<br><br>• **INSPECTION_BASED:** Shows only the flat `Inspection Fee`.<br><br>• **CUSTOM:** Renders custom configuration dropdown fields mapped from Module 12. Prompt user for manual rate entry. |
| **Final Rate (₹)** | **Editable Input.** Auto-populated based on the `Pricing Calculation` logic above. The user can keep the calculated rate or manually override it (e.g., negotiated discount). If a Product is selected, it defaults to the `sellingPrice` (Mod 10). |
| **Quantity** | Input field for quantity. Defaults to 1. For products, system validates against available stock in Module 10 (or warns about negative inventory). |
| **[ADD TO INVOICE]**| Closes modal and appends the selected item, calculated/overridden rate, and quantity as a new row at the Invoice Line Items grid. |
| **[CANCEL] / [X]** | Closes modal without making changes to the grid. |
>>>>>>> 697ebee (Module 28 is done)

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
│  TRANSACTION LEDGER (PAYMENTS & ADJUSTMENTS)                                 │
│  ┌──────────┬────────────┬──────────────────┬──────────────┬──────────────┐  │
│  │Date      │Type        │Reference #       │Credit Amount │Running Bal   │  │
│  │──────────┼────────────┼──────────────────┼──────────────┼──────────────│  │
│  │ 15 Mar   │ Invoice    │ INV-10024        │     —        │ ₹ 10,000      │  │
│  │ 18 Mar   │ Payment    │ RCPT-8021        │ ₹ 5,000      │ ₹ 5,000       │  │
│  │ 20 Mar   │ Adjustment │ CN-5001          │ ₹ 5,000      │ ₹ 0           │  │
│  └──────────┴────────────┴──────────────────┴──────────────┴──────────────┘  │
│  [+ RECORD PAYMENT] (To Mod 30)   [+ ISSUE CREDIT NOTE] (To 28.5)            │
│                                                                              │
│  AUDIT LOG                                                                   │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │ 15 Mar 2026 10:30 — Created by Amit Shah (Draft)                    │    │
│  │ 15 Mar 2026 11:15 — Approved & Sent by Priya Patel                 │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  [📥 DOWNLOAD PDF]  [✉ RESEND]  [🧾 RECORD PAYMENT]                        │
│  [🔙 BACK TO LIST]                                                          │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 1. HEADER

| Field Name    | Description                                            |
| ------------- | ------------------------------------------------------ |
| Invoice Title | Displays invoice detail heading with invoice number    |
| Status        | Current status of invoice (Draft, Sent, Paid, Overdue) |
| Due Date      | Final date by which payment should be completed        |


### 2. FROM (Company Details)
| Field Name     | Description                                      |
| -------------- | ------------------------------------------------ |
| Company Name   | Name of the service provider issuing the invoice |
| GSTIN (From)   | GST identification number of the issuing company |
| Address (From) | Full address of the issuing company              |
| State (From)   | State of the issuing company                     |


### 3. TO (Customer Details)
| Field Name    | Description                               |
| ------------- | ----------------------------------------- |
| Customer Name | Name of the client receiving the invoice  |
| GSTIN (To)    | GST identification number of the customer |
| Address (To)  | Full address of the customer              |
| State (To)    | State of the customer                     |


### 4. Invoice Info
| Field Name              | Description                            |
| ----------------------- | -------------------------------------- |
| Invoice Number          | Unique identifier of the invoice       |
| Invoice Date            | Date when the invoice is generated     |
| SO Reference            | Linked sales order reference           |
| Credit Period           | Allowed payment duration in days       |
| Contract Reference      | Linked contract reference              |
| Due Date (Info Section) | Calculated or defined payment due date |


### 5. Line Items
| Field Name   | Description                          |
| ------------ | ------------------------------------ |
| Sr No        | Serial number of line item           |
| Description  | Service or product description       |
| HSN/SAC Code | Tax classification code              |
| Quantity     | Number of units                      |
| Rate         | Price per unit                       |
| Discount %   | Discount applied on item             |
| Tax %        | Applicable tax percentage            |
| Amount       | Final calculated amount for the item |


### 6. Tax Summary

| Field Name     | Description                           |
| -------------- | ------------------------------------- |
| Subtotal       | Total amount before discount and tax  |
| Discount       | Total discount applied                |
| Taxable Amount | Amount after discount before tax      |
| CGST           | Central GST amount                    |
| SGST           | State GST amount                      |
| IGST           | Integrated GST amount (if applicable) |
| Grand Total    | Final payable amount                  |


### 7. Transaction Ledger

| Field Name     | Description                                               |
| -------------- | --------------------------------------------------------- |
| Date           | Date of the transaction                                   |
| Type           | Source of transaction (Invoice / Payment / Adjustment)    |
| Reference #    | Identifying number (INV #, RCPT #, CN #). Clickable.      |
| Credit Amount  | The amount credited (paid or adjusted) against the invoice|
| Running Bal    | The remaining pending invoice amount after this line      |
| **[+ RECORD PAYMENT]** | Button that redirects to **Module 30**           |
| **[+ ISSUE CREDIT NOTE]** | Button that redirects to **Screen 28.5 (Credit Note)** |

### 8. Audit Log

| Field Name           | Description                                           |
| -------------------- | ----------------------------------------------------- |
| Activity Date & Time | Timestamp of action performed                         |
| Activity Description | Description of action (Created, Approved, Sent, etc.) |
| Performed By         | User who performed the action                         |
| Status Change        | Status associated with the activity                   |

---

## Actions (View Screen)

| Action              | Type   | Condition          | Description                                      |
| ------------------- | ------ | ------------------ | ------------------------------------------------ |
| **Download PDF**    | Button | Sent / Paid        | Download formatted invoice PDF                   |
| **Resend**          | Button | Sent / Overdue     | Re-send via Email or WhatsApp                    |
| **Record Payment**  | Button | Sent / Partial     | Redirect to Module 30 with this invoice          |
| **Back to List**    | Button | All                | Returns to Invoice Dashboard (28.1)              |

---

================================================================================

# 28.4 Edit Invoice (Draft Only)

**Description:**
Allows editing of an invoice that is still in **Draft** status. Once an invoice is Approved & Sent, it cannot be edited — a Credit Note must be issued instead. The edit form uses the exact same UI, components, and dynamic logic as the **Create Invoice form (28.2)**. 

*Note for Developers: Re-use the 28.2 UI layout strictly. The "Add Line Item" modal with the dynamic `FIXED_PRICE`/`AREA_BASED` configurations from Module 12 behaves identically here, with all existing data pre-populated.*

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

# 28.5 Credit Note (Adjustment)

**Description:**
Used to reduce the pending value of an issued invoice. This is a simplified manual adjustment ledger entry. It does not require selecting individual line items or an approval workflow. It can be issued manually here, or auto-generated by **Module 30 (Payments)** when a user records a short payment and chooses to "Settle & Close" the remaining balance.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         ISSUE CREDIT NOTE / ADJUSTMENT                       │
│                                                                              │
│  ORIGINAL INVOICE                                                            │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Invoice #      : INV-10024                                            │  │
│  │ Customer       : ABC Corp Ltd                                         │  │
│  │ Invoice Amount : ₹ 10,561                                             │  │
│  │ Pending Amount : ₹ 10,561                                             │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ADJUSTMENT DETAILS                                                          │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Credit Note Date* : [📅 28 Mar 2026]                                  │  │
│  │ Reason*           : [▼ Select Reason ▼]                               │  │
│  │                     (Payment Settlement / Pricing Error / Service      │  │
│  │                      Issue / Full Cancellation / Other)                │  │
│  │ Other Reason*     : [_____________________________________________]   │  │
│  │                     (Visible only when Reason = "Other")               │  │
│  │ Remarks*           : [_____________________________________________]   │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ADJUSTMENT AMOUNT                                                           │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Adjust Credit Amt*: [ ₹ 10,000 ]                                       │  │
│  │                                                                       │  │
│  │ ── Summary ────────────────────────────────────────────────────────── │  │
│  │ New Pending Amt   : ₹ 10,561 - ₹ 10,000 = ₹ 561                      │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [ISSUE CREDIT NOTE]    [CANCEL]                                             │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 1. ORIGINAL INVOICE
| Field Name     | Description                                                              |
| -------------- | ------------------------------------------------------------------------ |
| Invoice Number | Reference of the original invoice for which credit note is being created |
| Customer Name  | Name of the customer associated with the invoice                         |
| Invoice Amount | Total amount of the original invoice                                     |
| Pending Amount | Remaining unpaid amount of the invoice                                   |


### 2. Adjustment Details
| Field Name       | Type      | Required | Description                                                                             |
| ---------------- | --------- | -------- | --------------------------------------------------------------------------------------- |
| Credit Note Date | Date      | Yes      | Date on which the credit note is issued                                                 |
| Reason           | Dropdown  | Yes      | Reason for the adjustment. Options: Payment Settlement / Pricing Error / Service Issue / Full Cancellation / Other |
| Other Reason     | Text      | Cond.    | **Visible only when Reason = "Other"**. Free-text reason. Required if Reason = Other    |
| Remarks          | Textarea  | Yes       | Additional comments or explanation for the credit note                                  |
| Adjust Credit Amt| Number    | Yes      | The total manual amount to credit against the invoice's pending balance                 |

---

## Validation Rules

| Field            | Rule                                                       |
| ---------------- | ---------------------------------------------------------- |
| Credit Note Date | Cannot be before original invoice date                     |
| Reason           | Must select from dropdown                                  |
| Other Reason     | Required if Reason = "Other". Cannot be blank whitespace   |
| Credit Amount    | Cannot exceed the current `Pending Amount` of the invoice  |

---

## System Behavior & Automation

| Event                      | System Action                                             |
| -------------------------- | --------------------------------------------------------- |
| Credit Note issued         | Auto-approved immediately. CN number generated (CN-XXXXX) |
| Ledger adjustment          | Customer Ledger credited by CN amount                     |
| Invoice pending reduced    | Original invoice's pending amount reduced by CN amount    |
| **Auto-Generate via Payment**| If Payment (Module 30) is ₹561 against ₹10,561, user can choose "Settle". System auto-creates a ₹10,000 CN internally. |

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
