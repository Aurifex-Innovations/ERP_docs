# 🎯 MODULE 20: SALES ORDER (SO) MANAGEMENT

## Overview

The official financial and operational document that confirms revenue boundaries and acts as a trigger to the Operations team to dispatch technicians or products for a job. Sales Orders can be generated automatically (cron-based for AMC contracts) or manually for one-time services and standalone product sales. Supports three distinct order types — Service AMC, One-Time Service, and Product Sale — each with its own source workflow and line-item structure.

**Module Connections:**

- **Depends on:** Module 19 (Contract Management – for AMC-based SO auto-generation), Module 17 (GMA Management – for one-time service SO source), Module 18 (Customer Management – customer & site data), Module 16 (Quotation Management – approved quotation for one-time SO), Module 10 (Product Master – for product sale line items), Module 9 (Tax Management – HSN/SAC tax calculation)
- **Used by:** Operations & Dispatch (Job Card generation), Invoicing & Billing, Inventory (chemical/product issuance from Module 11)
- **Prerequisites:** For AMC SOs, an **Active Contract** (Module 19) must exist. For One-Time Service SOs, an **Approved GMA/Quotation** (Module 17/16) is needed. For Product Sale SOs, the Product Master (Module 10) must be configured.

---

# 20.1 Sales Order Master List View

**Description:**
A comprehensive dashboard displaying all Sales Orders (execution mandates) in the system. Provides filtering by order type, status, and branch, along with search and row-level actions.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      SALES ORDER MANAGEMENT                                  │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                │  │
│  │                                                                        │  │
│  │ Order Type : [☑ Service AMC  ☑ One-Time Service  ☑ Product Sale]      │  │
│  │ Status     : [☑ Draft ☑ Open ☑ Fulfilled ☑ Billed ☑ Cancelled]      │  │
│  │ Branch     : [▼ All Branches ▼]                                       │  │
│  │ Customer   : [🔍 Search / Select ▼]                                   │  │
│  │ Date Range : [📅 From] – [📅 To]  (SO Date)                          │  │
│  │                                                                        │  │
│  │ Search: [________________________] (SO Number / Customer / Contract)  │  │
│  │                                                 [Reset Filters]       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│                                              [+ GENERATE SALES ORDER]        │
│                                                                              │
│  SALES ORDER TABLE                                                           │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │SO Number    │Customer        │Order Type     │Total Value│SO Date  │Status││
│  │─────────────┼────────────────┼───────────────┼───────────┼─────────┼──────││
│  │SO-2026-0112 │ABC Corporation │Service AMC    │₹ 51,000   │01 Apr 26│✅ Open││
│  │SO-2026-0087 │ABC Corporation │Service AMC    │₹ 51,000   │01 Jan 26│✅ Fulf││
│  │SO-2026-0043 │XYZ Hotel       │One-Time Svc   │₹ 8,500    │15 Jan 26│✅ Fulf││
│  │SO-2026-0038 │PQR Foods       │Product Sale   │₹ 12,200   │10 Jan 26│💰 Blld││
│  │SO-2026-0022 │DEF Mall        │Service AMC    │₹ 15,000   │01 Jan 26│❌ Cncl││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Actions                                                                   ││
│  │──────────────────────────────────────────────────────────────────────────││
│  │[View] [Edit] [Cancel]                                                    ││
│  │[View]                                                                    ││
│  │[View]                                                                    ││
│  │[View]                                                                    ││
│  │[View]                                                                    ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   Next                              │
│                                                                              │
│  Legend: 📋 Draft  ✅ Open  ✅ Fulfilled  💰 Billed  ❌ Cancelled             │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table View Fields

| Field       | Type         | Description                                                   |
| ----------- | ------------ | ------------------------------------------------------------- |
| SO Number   | Text (Auto)  | System-generated unique ID (e.g., SO-2026-0112)               |
| Customer    | Text         | Customer name from Module 18                                  |
| Order Type  | Badge        | Service AMC / One-Time Service / Product Sale                 |
| Total Value | Currency     | Total SO amount including taxes (₹)                           |
| SO Date     | Date         | Date the SO was generated                                     |
| Status      | Badge        | Draft / Open / Fulfilled / Billed / Cancelled                 |
| Actions     | Button Group | [View] [Edit] [Cancel]                                        |

---

## Filters

| Filter     | Type         | Options                                                       |
| ---------- | ------------ | ------------------------------------------------------------- |
| Order Type | Multi-select | Service AMC / One-Time Service / Product Sale                 |
| Status     | Multi-select | Draft / Open / Fulfilled / Billed / Cancelled                 |
| Branch     | Dropdown     | All Branches / Specific Branch (from Module 7)                |
| Customer   | Search       | Search by Customer Name / ID from Module 18                   |
| Date Range | Date Range   | From – To (SO creation date)                                  |

---

## Search

Searchable by:
- SO Number
- Customer Name
- Contract ID (for AMC-linked SOs)

---

## Actions (Table Row)

| Action     | Condition                                       | Description                                           |
| ---------- | ----------------------------------------------- | ----------------------------------------------------- |
| **View**   | All statuses                                    | Opens the SO Detail dashboard (Screen 20.3)           |
| **Edit**   | Status = Draft or Open; no Job Cards generated  | Opens the Edit SO form (Screen 20.4)                  |
| **Cancel** | Status = Draft or Open; no execution begun      | Opens the Cancellation dialog (Screen 20.5)           |

---

## Form Actions

| Action                     | Description                                               |
| -------------------------- | --------------------------------------------------------- |
| **+ Generate Sales Order** | Opens the **Add / Generate SO Form** (Screen 20.2)        |

---

================================================================================

# 20.2 Add / Generate Sales Order Form

**Description:**
A multi-section form to generate a Sales Order. Can be triggered automatically via cron (for AMC contracts based on the payment schedule) or created manually here. Supports three distinct source workflows: from an Active Contract (AMC), from an Approved Quotation/GMA (One-Time), or as a Standalone Product Sale.

> **Note:** For AMC contracts, Sales Orders are typically **auto-generated** by the system cron based on the contract's payment schedule (Module 19). This form is primarily used for manual SO creation — one-time services, product sales, or ad-hoc AMC SOs.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to SO List]            GENERATE SALES ORDER                         │
│                                                                              │
│  SO Number: SO-XXXX (Auto-generated on save)                                 │
│                                                                              │
│  SECTION 1: ORDER SOURCE & TYPE                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Order Type*:                                                           │ │
│  │  (•) Service AMC    ( ) One-Time Service    ( ) Product Sale            │ │
│  │                                                                         │ │
│  │  ━━━ IF SERVICE AMC ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │ │
│  │  Source*           : From Active Contract                               │ │
│  │  Select Contract*  : [🔍 Search Contract by ID / Customer ▼]          │ │
│  │                                                                         │ │
│  │  AUTO-FILLED FROM CONTRACT:                                             │ │
│  │  Contract ID       : CON-2026-0041  (Read-only)                        │ │
│  │  Customer Name     : ABC Corporation (Read-only)                       │ │
│  │  Customer ID       : CUST-00245  (Read-only)                           │ │
│  │  Service Type      : Cockroach Treatment (Read-only)                   │ │
│  │  Contract Value    : ₹ 2,04,000 (Read-only)                           │ │
│  │  Billing Period*   : [▼ Q2 Apr–Jun 2026 ▼]                            │ │
│  │                                                                         │ │
│  │  ━━━ IF ONE-TIME SERVICE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │ │
│  │  Source*           : From Approved Quotation / GMA                      │ │
│  │  Select Source*    : [🔍 Search Quotation / GMA by ID / Customer ▼]   │ │
│  │                                                                         │ │
│  │  AUTO-FILLED FROM QUOTATION / GMA:                                      │ │
│  │  Reference ID      : QT-2026-00088 / GMA-00091 (Read-only)            │ │
│  │  Customer Name     : XYZ Hotel (Read-only)                             │ │
│  │  Service Type      : General Pest Treatment (Read-only)                │ │
│  │  Quoted Value      : ₹ 8,500 (Read-only)                              │ │
│  │                                                                         │ │
│  │  ━━━ IF PRODUCT SALE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │ │
│  │  Source*           : Standalone                                         │ │
│  │  Select Customer*  : [🔍 Search Customer by Name / ID ▼]              │ │
│  │                                                                         │ │
│  │  AUTO-FILLED FROM CUSTOMER:                                             │ │
│  │  Customer ID       : CUST-00312 (Read-only)                            │ │
│  │  Customer Name     : PQR Foods (Read-only)                             │ │
│  │  Billing Address   : Koramangala, Bengaluru (Read-only)                │ │
│  │                                                                         │ │
│  │  SO Date*          : [📅 Date Picker] (Default: Today)                 │ │
│  │  Branch*           : [▼ Auto from source / Select ▼]                   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 2: LINE ITEMS                                                       │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │                                                                         │ │
│  │  ━━━ FOR SERVICES (AMC / One-Time) ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │ │
│  │  (Auto-fetched from Contract / Quotation / GMA source)                 │ │
│  │                                                                         │ │
│  │  ┌────────────────────────────────────────────────────────────────────┐│ │
│  │  │#│Service          │Site      │SQFT │Qty   │Unit     │HSN/SAC│Tax% ││ │
│  │  │ │                 │          │     │(Visits│Price(₹) │       │     ││ │
│  │  │─┼─────────────────┼──────────┼─────┼──────┼─────────┼───────┼─────││ │
│  │  │1│Cockroach Treatmt│Head Off. │3,500│ 12   │₹ 3,250  │996490 │18%  ││ │
│  │  │2│Cockroach Treatmt│Warehouse │8,000│ 3    │₹ 2,333  │996490 │18%  ││ │
│  │  │3│Cockroach Treatmt│Showroom  │1,200│ 12   │₹ 1,267  │996490 │18%  ││ │
│  │  └────────────────────────────────────────────────────────────────────┘│ │
│  │                                                                         │ │
│  │  ┌────────────────────────────────────────────────────────────────────┐│ │
│  │  │ Tax Amt(₹)│Line Total(₹)│Actions                                  ││ │
│  │  │───────────┼─────────────┼─────────────────────────────────────────││ │
│  │  │₹ 7,020    │₹ 46,020     │[Edit Qty] [Remove]                      ││ │
│  │  │₹ 1,260    │₹ 8,260      │[Edit Qty] [Remove]                      ││ │
│  │  │₹ 2,736    │₹ 17,936     │[Edit Qty] [Remove]                      ││ │
│  │  └────────────────────────────────────────────────────────────────────┘│ │
│  │                                                                         │ │
│  │  ━━━ FOR PRODUCTS (Product Sale) ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │ │
│  │  [+ Add Product Line Item]                                              │ │
│  │                                                                         │ │
│  │  ┌────────────────────────────────────────────────────────────────────┐│ │
│  │  │#│Product          │Code    │UOM │Qty│Unit Price(₹)│HSN  │Tax%│Tax ││ │
│  │  │─┼─────────────────┼────────┼────┼───┼─────────────┼─────┼────┼────││ │
│  │  │1│Alpha Cypermethrin│P-001   │Ltr │ 5 │₹ 1,200      │3808 │18% │₹1080│ │
│  │  │2│Fipronil Gel     │P-003   │Tube│ 20│₹ 220        │3808 │18% │₹ 792│ │
│  │  └────────────────────────────────────────────────────────────────────┘│ │
│  │                                                                         │ │
│  │  ┌────────────────────────────────────────────────────────────────────┐│ │
│  │  │ Line Total(₹)│Actions                                              ││ │
│  │  │──────────────┼─────────────────────────────────────────────────────││ │
│  │  │₹ 7,080       │[Edit] [Remove]                                      ││ │
│  │  │₹ 5,192       │[Edit] [Remove]                                      ││ │
│  │  └────────────────────────────────────────────────────────────────────┘│ │
│  │                                                                         │ │
│  │  ═══ ORDER TOTAL SUMMARY ═══════════════════════════════════════════  │ │
│  │  Sub-Total (₹)    : ₹ 61,300                                          │ │
│  │  Discount (₹)     : ₹ 0  [Apply Discount ▼]                           │ │
│  │  Tax Total (₹)    : ₹ 11,016                                          │ │
│  │  ─────────────────────────────────────────────────────────────────     │ │
│  │  GRAND TOTAL (₹)  : ₹ 72,316                                          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 3: DELIVERY / EXECUTION TERMS                                       │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Execution Notes:                                                       │ │
│  │  [________________________________________________________]            │ │
│  │  [________________________________________________________]            │ │
│  │  (e.g., "Weekend service only", "Dispatch product to Site B")          │ │
│  │                                                                         │ │
│  │  Delivery Address  : [▼ Select Site Address / Custom ▼]                │ │
│  │                      (Auto from Customer's registered sites)           │ │
│  │                                                                         │ │
│  │  IF CUSTOM ADDRESS:                                                     │ │
│  │  Address Line 1*  : [________________________________]                 │ │
│  │  Address Line 2   : [________________________________]                 │ │
│  │  City*            : [________________]                                 │ │
│  │  State*           : [▼ Select State ▼]                                 │ │
│  │  Pincode          : [________]                                         │ │
│  │                                                                         │ │
│  │  Priority Level   : [▼ Normal / Urgent / Critical ▼]                   │ │
│  │  Expected Delivery: [📅 Date Picker]                                   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [SAVE AS DRAFT]      [SAVE & OPEN]      [CANCEL]                       │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Section 1: Order Source & Type Fields

| Field              | Type            | Required    | Options / Validation                                                                | Notes                                          |
| ------------------ | --------------- | ----------- | ----------------------------------------------------------------------------------- | ---------------------------------------------- |
| Order Type         | Radio           | Yes         | Service AMC / One-Time Service / Product Sale                                       | Determines which source + line-item fields show|
| Select Contract    | Search Dropdown | Conditional | Active contracts from Module 19                                                     | Required if Type = Service AMC                 |
| Billing Period     | Dropdown        | Conditional | Available billing periods from contract's payment schedule                          | Required if Type = Service AMC                 |
| Select Quotation/GMA | Search Dropdown | Conditional | Approved Quotations (Module 16) or Approved GMAs (Module 17)                     | Required if Type = One-Time Service            |
| Select Customer    | Search Dropdown | Conditional | Active customers from Module 18                                                     | Required if Type = Product Sale                |
| Contract ID        | Auto-filled     | System      | Read-only                                                                           | Populated from contract selection              |
| Customer Name      | Auto-filled     | System      | Read-only                                                                           | From source record                             |
| Customer ID        | Auto-filled     | System      | Read-only                                                                           | From source record                             |
| Service Type       | Auto-filled     | System      | Read-only (for service orders)                                                      | Pest / service category                        |
| Contract / Quoted Value | Auto-filled | System      | Read-only                                                                           | Reference value from source                    |
| SO Date            | Date            | Yes         | Default: Today; cannot be future > 30 days                                          | Sales Order creation date                      |
| Branch             | Dropdown        | Yes         | Auto from source; editable — active branches from Module 7                          | Servicing / dispatch branch                    |

> **Note:** For **Service AMC** orders, the system only shows contracts with unused billing periods. If all billing periods already have SOs, the contract will not appear in the search. For **One-Time Service**, only Quotations / GMAs that have not yet been converted to an SO are shown.

---

## Section 2: Line Items Fields

### Service Line Items (AMC / One-Time)

| Field           | Type            | Required | Validation / Notes                                                              |
| --------------- | --------------- | -------- | ------------------------------------------------------------------------------- |
| Service Name    | Auto-filled     | System   | Service description from GMA / Contract (e.g., "Cockroach Treatment")           |
| Site            | Auto-filled     | System   | Site name where service is to be performed                                      |
| SQFT            | Auto-filled     | System   | Area from customer's site record (Module 18)                                    |
| Qty (Visits)    | Number          | Yes      | Number of visits for this billing period; editable, must be > 0                 |
| Unit Price (₹)  | Currency        | Yes      | Price per visit; auto from contract/GMA; editable for overrides                 |
| HSN/SAC Code    | Auto-filled     | System   | Service Accounting Code for tax calculation (from Module 9)                     |
| Tax %           | Auto-filled     | System   | GST rate from HSN/SAC code (Module 9)                                           |
| Tax Amount (₹)  | Auto-calculated | System   | `Qty × Unit Price × Tax%`                                                       |
| Line Total (₹)  | Auto-calculated | System   | `(Qty × Unit Price) + Tax Amount`                                               |

### Product Line Items (Product Sale)

| Field              | Type            | Required | Validation / Notes                                                       |
| ------------------ | --------------- | -------- | ------------------------------------------------------------------------ |
| Product Name       | Search Dropdown | Yes      | Select from Module 10 Product Master (active products only)              |
| Product Code       | Auto-filled     | System   | Read-only; from Module 10                                                |
| UOM                | Auto-filled     | System   | Base unit of measure from Module 10 (ml / Ltr / gm / kg / Nos / Tube)   |
| Qty                | Number          | Yes      | Must be > 0                                                              |
| Unit Price (₹)     | Currency        | Yes      | Auto from Module 10 sale price; editable for custom pricing              |
| HSN Code           | Auto-filled     | System   | From Module 10 product master → Module 9 tax config                      |
| Tax %              | Auto-filled     | System   | GST rate from HSN code (Module 9)                                        |
| Tax Amount (₹)     | Auto-calculated | System   | `Qty × Unit Price × Tax%`                                                |
| Line Total (₹)     | Auto-calculated | System   | `(Qty × Unit Price) + Tax Amount`                                        |

### Order Total Summary (Auto-Calculated)

| Field          | Type            | Description                                         |
| -------------- | --------------- | --------------------------------------------------- |
| Sub-Total (₹)  | Auto-calculated | Sum of all line items before tax                    |
| Discount (₹)   | Currency        | Optional; flat or percentage discount               |
| Tax Total (₹)  | Auto-calculated | Sum of all line-item tax amounts                    |
| Grand Total (₹)| Auto-calculated | `Sub-Total – Discount + Tax Total`                  |

> **Note:** For AMC SOs, line items are auto-fetched from the contract's sites and services. The user can adjust quantities (e.g., visits in the period) but cannot add entirely new service types not covered by the contract. For Product Sale SOs, the user has full control to add/remove products from Module 10.

---

## Section 3: Delivery / Execution Terms Fields

| Field             | Type      | Required | Options / Validation                                         | Notes                                           |
| ----------------- | --------- | -------- | ------------------------------------------------------------ | ----------------------------------------------- |
| Execution Notes   | Text Area | No       | Max 500 characters                                           | Operational instructions for the dispatch team  |
| Delivery Address  | Dropdown  | No       | Customer's registered sites (Module 18) / Custom             | Default: primary site                           |
| Custom Address    | Text      | Cond.    | Required if Delivery Address = Custom; min 10 chars          | Full address for non-registered delivery        |
| City              | Text      | Cond.    | Required if Custom; min 3 chars                              | Delivery city                                   |
| State             | Dropdown  | Cond.    | Indian states list                                           | Delivery state                                  |
| Pincode           | Number    | No       | 6-digit numeric                                              | Optional                                        |
| Priority Level    | Dropdown  | No       | Normal / Urgent / Critical                                   | Determines dispatch priority in Operations      |
| Expected Delivery | Date      | No       | Must be ≥ SO Date                                            | Target date for service / product delivery      |

---

## Form Actions

| Button          | Description                                                                          |
| --------------- | ------------------------------------------------------------------------------------ |
| **Save as Draft** | Saves the SO without releasing. Status = Draft. Can be edited freely.              |
| **Save & Open** | Validates all sections, saves the SO with Status = Open, and makes it available for Operations dispatch. |
| **Cancel**      | Discards all entries and returns to the SO Master List.                               |

---

## Validation Rules

| Validation                        | Rule                                                                      |
| --------------------------------- | ------------------------------------------------------------------------- |
| Order Type Required               | Must select Service AMC, One-Time, or Product Sale                        |
| Source Required                   | Must select a valid contract / quotation / customer based on type         |
| At Least 1 Line Item              | Minimum one service or product line item required                         |
| Qty > 0                          | All line item quantities must be positive                                 |
| Unit Price > 0                   | All line item prices must be positive                                     |
| Grand Total > 0                  | Final order value must be positive after discounts                        |
| No Duplicate Billing Period       | For AMC, cannot create two SOs for the same billing period and contract   |
| SO Date Required                 | Must have a valid SO date                                                 |
| Branch Required                  | Must have an assigned branch                                              |

---

## System Behaviours on Save

| Trigger                         | System Action                                                                     |
| ------------------------------- | --------------------------------------------------------------------------------- |
| Contract selected (AMC)         | Auto-populates customer details, sites, services, and pricing from Module 19      |
| Quotation/GMA selected (One-Time)| Auto-populates service details and pricing from Module 16/17                      |
| Customer selected (Product)     | Auto-populates billing address from Module 18                                     |
| Product selected                | Fetches UOM, HSN Code, Sale Price from Module 10 Product Master                   |
| Save & Open clicked             | Status → Open; SO visible to Operations for dispatch                              |
| AMC SO created                  | Linked to contract billing log (Module 19 → Tab 2); fulfilment tracker updated   |
| One-Time SO created             | Quotation / GMA marked as "SO Generated" to prevent duplicate SOs                |
| Tax calculation                 | HSN/SAC codes auto-fetch tax rates; tax amounts computed in real-time             |

---

================================================================================

# 20.3 View Sales Order Details

**Description:**
A clear, read-only layout detailing exactly what needs to be billed and executed. Split into two tabs — SO Summary & Line Items, and Execution & Delivery Status.

---

## Screen Layout (Header)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to SO List]                              [Edit SO]  [Cancel SO]    │
│                                                                              │
│  SALES ORDER: SO-2026-0112                     Status: ✅ OPEN               │
│  Customer : ABC Corporation (CUST-00245)       │  Branch: Mumbai             │
│  Order Type: Service AMC                       │  Contract: CON-2026-0041    │
│  SO Date  : 01 Apr 2026                        │  Period: Q2 Apr–Jun 2026    │
│  Grand Total: ₹ 72,316                         │  Priority: Normal           │
│                                                                              │
│  [Tab 1: SO Summary & Line Items ●]  [Tab 2: Execution & Delivery Status]  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

================================================================================

# 20.3.1 Tab 1: SO Summary & Line Items

**Description:**
Displays the complete order breakdown — header details, source reference, and a detailed line-item grid with HSN/SAC codes, quantities, taxes, and final amounts.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Tab 1: SO Summary & Line Items ●]  [Tab 2: Execution & Delivery Status] │
│                                                                              │
│  ─── ORDER HEADER ────────────────────────────────────────────────────────  │
│  SO Number     : SO-2026-0112           SO Date      : 01 Apr 2026         │
│  Customer      : ABC Corporation        Customer ID  : CUST-00245          │
│  Order Type    : Service AMC            Contract Ref : CON-2026-0041       │
│  Billing Period: Q2 Apr–Jun 2026        Branch       : Mumbai              │
│  Priority      : Normal                 Expected Date: 30 Jun 2026         │
│                                                                              │
│  ─── LINE ITEMS ──────────────────────────────────────────────────────────  │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │#│Description           │Site      │Qty  │UOM   │Unit Price│HSN/SAC│Tax% ││
│  │─┼──────────────────────┼──────────┼─────┼──────┼──────────┼───────┼─────││
│  │1│Cockroach Treatment   │Head Off. │ 12  │Visits│₹ 3,250   │996490 │18%  ││
│  │2│Cockroach Treatment   │Warehouse │ 3   │Visits│₹ 2,333   │996490 │18%  ││
│  │3│Cockroach Treatment   │Showroom  │ 12  │Visits│₹ 1,267   │996490 │18%  ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ Tax Amount(₹)│ Line Total(₹)                                             ││
│  │──────────────┼───────────────────────────────────────────────────────────││
│  │ ₹ 7,020      │ ₹ 46,020                                                 ││
│  │ ₹ 1,260      │ ₹ 8,260                                                  ││
│  │ ₹ 2,736      │ ₹ 17,936                                                 ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ─── ORDER TOTAL ─────────────────────────────────────────────────────────  │
│  Sub-Total   : ₹ 61,300                                                     │
│  Discount    : ₹ 0.00                                                       │
│  Tax Total   : ₹ 11,016                                                     │
│  ───────────────────────────────────────                                     │
│  GRAND TOTAL : ₹ 72,316                                                     │
│                                                                              │
│  ─── EXECUTION NOTES ─────────────────────────────────────────────────────  │
│  "Service visits to be scheduled on Wednesdays and Saturdays.               │
│   Warehouse site: Monthly service on 1st Monday only."                      │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Order Header Fields (Read-Only)

| Field          | Type     | Description                                              |
| -------------- | -------- | -------------------------------------------------------- |
| SO Number      | Display  | Unique SO identifier                                     |
| SO Date        | Date     | Date the SO was generated                                |
| Customer       | Display  | Customer name from Module 18                             |
| Customer ID    | Display  | Unique customer reference                                |
| Order Type     | Display  | Service AMC / One-Time Service / Product Sale            |
| Contract Ref   | Link     | Contract ID (AMC only); navigates to Module 19           |
| Billing Period | Display  | Period this SO covers (AMC only)                         |
| Branch         | Display  | Servicing / dispatch branch                              |
| Priority       | Display  | Normal / Urgent / Critical                               |
| Expected Date  | Date     | Target completion date                                   |

---

## Line Item Grid Fields (Read-Only)

| Field           | Type     | Description                                             |
| --------------- | -------- | ------------------------------------------------------- |
| #               | Number   | Line item sequence number                               |
| Description     | Display  | Service name or Product name                            |
| Site            | Display  | Site for service delivery (service orders only)         |
| Qty             | Number   | Visits (service) or units (product)                     |
| UOM             | Display  | Visits / Ltr / Nos / Tube / kg etc.                     |
| Unit Price (₹)  | Currency | Per-unit price                                          |
| HSN/SAC Code    | Display  | Tax classification code                                 |
| Tax %           | Display  | GST percentage                                          |
| Tax Amount (₹)  | Currency | Computed tax for this line                              |
| Line Total (₹)  | Currency | `(Qty × Unit Price) + Tax`                              |

---

## Order Total Fields (Read-Only)

| Field         | Type     | Description                          |
| ------------- | -------- | ------------------------------------ |
| Sub-Total     | Currency | Sum of all pre-tax line totals       |
| Discount      | Currency | Any applied discount                 |
| Tax Total     | Currency | Sum of all line-item taxes           |
| Grand Total   | Currency | Final payable amount                 |

---

================================================================================

# 20.3.2 Tab 2: Execution & Delivery Status

**Description:**
Tracks the downstream release of the Sales Order. Shows whether Job Cards (for services) or Delivery Challans (for products) have been generated against this specific SO, providing an end-to-end visibility from order to execution.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Tab 1: SO Summary & Line Items]  [Tab 2: Execution & Delivery Status ●]  │
│                                                                              │
│  ─── SERVICE EXECUTION STATUS ────────────────────────────────────────────  │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Job Card ID  │Site      │Scheduled Date│Technician  │Status    │Actions   ││
│  │─────────────┼──────────┼──────────────┼────────────┼──────────┼──────────││
│  │JC-2026-0441 │Head Off. │05 Apr 2026   │Ravi S.     │✅ Done   │[View]    ││
│  │JC-2026-0442 │Showroom  │05 Apr 2026   │Anjali M.   │✅ Done   │[View]    ││
│  │JC-2026-0443 │Head Off. │12 Apr 2026   │Ravi S.     │🕐 Planned│[View]    ││
│  │JC-2026-0444 │Warehouse │01 May 2026   │Suresh K.   │⏳ Pending│[View]    ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ─── PRODUCT DELIVERY STATUS ─────────────────────────────────────────────  │
│  (Visible only for Product Sale or mixed orders)                             │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Challan ID   │Product       │Qty │Dispatch Date│Delivery Add │Status     ││
│  │─────────────┼──────────────┼────┼─────────────┼─────────────┼───────────││
│  │DC-2026-0101 │Alpha Cyper.  │ 5L │12 Apr 2026  │Koramangala  │🚚 In Transit│
│  │DC-2026-0102 │Fipronil Gel  │20T │12 Apr 2026  │Koramangala  │⏳ Pending ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ─── EXECUTION SUMMARY ──────────────────────────────────────────────────  │
│  Total Job Cards     : 4         Completed: 2    Pending: 2                  │
│  Total Challans      : 2         Dispatched: 1   Pending: 1                  │
│  Overall Progress    : ██████████░░░░░░░░░░  50%                            │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Service Execution Grid Fields (Read-Only)

| Field          | Type    | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| Job Card ID    | Link    | Unique job card reference; navigates to Operations module    |
| Site           | Display | Site where service is performed                              |
| Scheduled Date | Date    | Planned service date                                         |
| Technician     | Display | Assigned technician name                                     |
| Status         | Badge   | Planned / In Progress / Done / Cancelled                     |
| Actions        | Button  | [View] — opens the job card detail in Operations module      |

---

## Product Delivery Grid Fields (Read-Only)

| Field          | Type    | Description                                           |
| -------------- | ------- | ----------------------------------------------------- |
| Challan ID     | Link    | Delivery Challan reference; navigates to dispatch     |
| Product        | Display | Product name from the SO line item                    |
| Qty            | Number  | Quantity dispatched                                   |
| Dispatch Date  | Date    | Date of product dispatch                              |
| Delivery Addr  | Display | Destination address                                   |
| Status         | Badge   | Pending / In Transit / Delivered / Returned           |

---

## Execution Summary Fields (Auto-Calculated)

| Field            | Type     | Description                                     |
| ---------------- | -------- | ----------------------------------------------- |
| Total Job Cards  | Number   | Count of all generated job cards                |
| Completed        | Number   | Job cards with Status = Done                    |
| Total Challans   | Number   | Count of all delivery challans                  |
| Dispatched       | Number   | Challans with Status = In Transit or Delivered  |
| Overall Progress | Progress | Percentage of completed execution items          |

> **Note:** This tab is **read-only**. Job Cards and Delivery Challans are created and managed by the **Operations & Dispatch** module. This tab provides the commercial team visibility into whether the SO's execution has started, which gates the Edit and Cancel actions.

---

================================================================================

# 20.4 Edit Sales Order Form

**Description:**
Allows modification of a Sales Order before it is released to Operations or while still in Draft/Open status. Editing is fully blocked once execution has begun (Job Cards printed or Challans dispatched) or the SO status is Fulfilled/Billed.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to SO Detail]         EDIT SALES ORDER   SO-2026-0112              │
│                                                                              │
│  SECTION 1: ORDER HEADER (Read-Only)                                         │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  SO Number     : SO-2026-0112      Status    : ✅ Open                  │ │
│  │  Customer      : ABC Corporation   Order Type: Service AMC              │ │
│  │  Contract Ref  : CON-2026-0041     SO Date   : 01 Apr 2026             │ │
│  │  Branch        : Mumbai                                                 │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 2: LINE ITEMS (Editable)                                            │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  ┌───────────────────────────────────────────────────────────────────┐  │ │
│  │  │#│Description          │Site     │Qty    │Unit Price│Tax%│Line Tot│  │ │
│  │  │─┼─────────────────────┼─────────┼───────┼──────────┼────┼────────│  │ │
│  │  │1│Cockroach Treatment  │Head Off.│[12]   │[₹ 3,250] │18% │₹46,020│  │ │
│  │  │2│Cockroach Treatment  │Warehouse│[ 3]   │[₹ 2,333] │18% │₹ 8,260│  │ │
│  │  │3│Cockroach Treatment  │Showroom │[12]   │[₹ 1,267] │18% │₹17,936│  │ │
│  │  └───────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                         │ │
│  │  Editable Fields: Qty, Unit Price                                       │ │
│  │  Substitute Chemical: [🔍 Select alternate from approved list ▼]       │ │
│  │  (Only if chemical substitution is permitted for this service)          │ │
│  │                                                                         │ │
│  │  Discount*         : [▼ None / Flat ₹ / Percentage % ▼]  [________]    │ │
│  │                                                                         │ │
│  │  Revised Sub-Total : ₹ 61,300                                          │ │
│  │  Revised Tax       : ₹ 11,016                                          │ │
│  │  Revised Grand Total: ₹ 72,316                                         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 3: EXECUTION NOTES (Editable)                                       │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Execution Notes: [Weekend service only__________________________]     │ │
│  │  Priority Level : [▼ Normal ▼]                                          │ │
│  │  Expected Date  : [📅 30 Jun 2026]                                      │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [SAVE CHANGES]                                    [CANCEL]             │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Editable vs. Locked Fields

| Field                   | Editable? | Notes                                                              |
| ----------------------- | --------- | ------------------------------------------------------------------ |
| SO Number               | ❌ Locked  | System-generated; immutable                                        |
| Customer / Contract Ref | ❌ Locked  | Source data cannot be changed                                      |
| Order Type              | ❌ Locked  | Cannot change the order type after creation                        |
| SO Date                 | ❌ Locked  | Historical record; cannot be modified                              |
| Branch                  | ❌ Locked  | Cannot change once created                                         |
| Line Item Qty           | ✅ Yes     | Can increase or decrease visit counts or product quantities        |
| Line Item Unit Price    | ✅ Yes     | Can adjust pricing (within approved variance limits)               |
| Chemical Substitution   | ✅ Yes     | Can swap a chemical if permitted by service configuration          |
| Discount                | ✅ Yes     | Can add / modify flat or percentage discount                       |
| Execution Notes         | ✅ Yes     | Free-text operational instructions                                 |
| Priority Level          | ✅ Yes     | Can escalate or de-escalate                                        |
| Expected Delivery Date  | ✅ Yes     | Can adjust target completion date                                  |

---

## Validation Rules

| Validation                          | Rule                                                                    |
| ----------------------------------- | ----------------------------------------------------------------------- |
| Status must be Draft or Open        | Edit fails if Status = Fulfilled, Billed, or Cancelled                  |
| No Job Cards generated              | Edit is **completely blocked** if any Job Cards exist for this SO       |
| No Delivery Challans dispatched     | Edit is **completely blocked** if any Challans exist for this SO        |
| Qty > 0                            | All line item quantities must remain positive                           |
| Unit Price > 0                     | All prices must remain positive                                         |
| Grand Total > 0                    | Order value must remain positive after discounts                        |
| Chemical substitution allowed       | System checks if substitution is permitted in service config (Module 12)|

---

## Form Actions

| Button            | Description                                                                     |
| ----------------- | ------------------------------------------------------------------------------- |
| **Save Changes**  | Validates all edits, recalculates totals, saves the updated SO                  |
| **Cancel**        | Discards unsaved changes and returns to the SO Detail view                      |

---

================================================================================

# 20.5 Delete (Cancel) Sales Order

**Description:**
A mechanism to void a Sales Order. Cancellation is only possible before execution begins. The SO record is preserved with a "Cancelled" status for audit — hard deletion is not permitted.

---

## Screen Layout

```
┌───────────────────────────────────────────────────────┐
│           CANCEL SALES ORDER                           │
│                                                        │
│  SO Number  : SO-2026-0022                             │
│  Customer   : DEF Mall (CUST-00189)                    │
│  Order Type : Service AMC                              │
│  SO Date    : 01 Jan 2026                              │
│  Grand Total: ₹ 15,000                                 │
│  Status     : ✅ Open                                   │
│                                                        │
│  ⚠️  WARNING: Cancelling this SO will:                 │
│  • Remove it from the Operations dispatch queue        │
│  • Mark it as permanently Cancelled                    │
│  • Free the billing period for re-generation (AMC)     │
│                                                        │
│  ELIGIBILITY CHECK:                                    │
│  Job Cards Generated  : ✅  None                       │
│  Challans Dispatched  : ✅  None                       │
│  Pushed to Invoicing  : ✅  No                         │
│                                                        │
│  Cancellation Reason*:                                 │
│  [▼ Select Reason ▼]                                   │
│      • Customer Request                                │
│      • Duplicate Order                                 │
│      • Incorrect Line Items                            │
│      • Contract Terminated                             │
│      • Service No Longer Required                      │
│      • Other                                           │
│                                                        │
│  Additional Remarks:                                   │
│  [________________________________________]            │
│  [________________________________________]            │
│                                                        │
│  [CONFIRM CANCELLATION]        [GO BACK]               │
└───────────────────────────────────────────────────────┘
```

*When cancellation is blocked:*

```
┌───────────────────────────────────────────────────────┐
│           CANCEL SALES ORDER                           │
│                                                        │
│  SO Number  : SO-2026-0087                             │
│  Status     : ✅ Open                                   │
│                                                        │
│  ELIGIBILITY CHECK:                                    │
│  Job Cards Generated  : ❌  2 Job Cards exist          │
│  Pushed to Invoicing  : ❌  Invoice INV-2026-0019      │
│                                                        │
│  ──  Cancellation is BLOCKED.  ──                      │
│  Execution has already begun or this SO has been       │
│  pushed to invoicing. Please contact Operations        │
│  or Finance to resolve.                                │
│                                                        │
│              [OK — Go Back]                            │
└───────────────────────────────────────────────────────┘
```

---

## Cancellation Prerequisite Check

| Check                   | Condition to Block                                          | Error Message                                               |
| ----------------------- | ----------------------------------------------------------- | ----------------------------------------------------------- |
| Job Cards Generated     | Any Job Card exists for this SO                             | "X Job Card(s) already generated. Cannot cancel."           |
| Challans Dispatched     | Any Delivery Challan exists for this SO                     | "Products have been dispatched. Cannot cancel."              |
| Pushed to Invoicing     | SO has been linked to an invoice                            | "SO is linked to Invoice. Contact Finance."                 |
| Status = Fulfilled/Billed| SO already completed                                       | "Fulfilled/Billed orders cannot be cancelled."              |

---

## Cancellation Form Fields

| Field                | Type     | Required    | Options / Validation                                                                 |
| -------------------- | -------- | ----------- | ------------------------------------------------------------------------------------ |
| Cancellation Reason  | Dropdown | Yes         | Customer Request / Duplicate Order / Incorrect Line Items / Contract Terminated / Service No Longer Required / Other |
| Additional Remarks   | Text Area| No          | Max 500 characters; required if Reason = Other                                       |

---

## Validation Rules

| Validation                    | Rule                                                            |
| ----------------------------- | --------------------------------------------------------------- |
| No Job Cards                  | Cancellation blocked if any Job Card exists for this SO         |
| No Challans                   | Cancellation blocked if any Delivery Challan dispatched         |
| Not in Invoicing              | Cancellation blocked if SO linked to an invoice                 |
| Status = Draft or Open        | Only Draft and Open SOs can be cancelled                        |
| Reason Required               | Must select a reason from the dropdown                          |
| Remarks (if Other)            | Mandatory free-text when "Other" is selected                    |

---

## System Behaviour on Cancellation

| Trigger                         | System Action                                                                |
| ------------------------------- | ---------------------------------------------------------------------------- |
| Confirm Cancellation clicked    | SO status set to **Cancelled**                                                |
| AMC billing period freed        | For AMC SOs, the billing period becomes available for re-generation          |
| Operations notified             | SO removed from dispatch queue; no future Job Cards generated                |
| Contract billing log updated    | Module 19 → Tab 2 reflects the cancelled SO with updated fulfilment tracker  |
| Customer history updated        | Module 18 → Tab 3 reflects the cancelled status                              |
| Audit log                       | Cancellation event logged with Reason, Timestamp, and user who acted         |

---

================================================================================

## RBAC – Role-Based Access Control

| Role              | List View (20.1)         | Generate SO (20.2) | View Details (20.3) | Edit SO (20.4)  | Cancel SO (20.5)  |
| ----------------- | ------------------------ | ------------------ | ------------------- | --------------- | ----------------- |
| Sales Person      | View (own branch)        | ✅                  | ✅                   | ✅ (own SOs)     | ❌                 |
| Sales Manager     | View (own branch / team) | ✅                  | ✅                   | ✅               | ✅                 |
| Branch Manager    | View (own branch)        | ✅                  | ✅                   | ✅               | ✅                 |
| Head Ops / Admin  | View (all branches)      | ✅                  | ✅                   | ✅               | ✅                 |
| Finance Auditor   | View (all – read-only)   | ❌                  | ✅                   | ❌               | ❌                 |
| System (Cron)     | —                        | ✅ (Auto AMC)       | —                   | —               | —                 |

---

================================================================================

## Navigation Flow Summary

```
MODULE 20: SALES ORDER MANAGEMENT
│
├── 20.1 Sales Order Master List
│   ├── [+ Generate Sales Order] → 20.2 Add / Generate SO Form
│   │     ├── Service AMC → Select Active Contract (Module 19)
│   │     │     └── Auto-fetch customer, sites, services, pricing
│   │     ├── One-Time Service → Select Approved Quotation/GMA (Module 16/17)
│   │     │     └── Auto-fetch service details and pricing
│   │     ├── Product Sale → Select Customer (Module 18)
│   │     │     └── Add products from Module 10 Product Master
│   │     ├── [Save as Draft]   → SO created (Draft)
│   │     └── [Save & Open]     → ✅ SO created (Open)
│   │           ├── Available for Operations dispatch
│   │           └── Appears in Module 19 → Tab 2 (Billing Log)
│   │           └── Appears in Module 18 → Tab 3 (SO History)
│   │
│   ├── [View] → 20.3 View SO Details
│   │     ├── Tab 1: SO Summary & Line Items
│   │     │     └── Complete order breakdown with HSN/SAC, taxes, totals
│   │     └── Tab 2: Execution & Delivery Status
│   │           ├── Job Card tracker (services)
│   │           └── Delivery Challan tracker (products)
│   │
│   ├── [Edit] → 20.4 Edit SO Form
│   │     ├── Blocked if: Status = Fulfilled/Billed, Job Cards exist
│   │     ├── Modify: Qty, Unit Price, Chemical substitution, Discount
│   │     └── Recalculates totals on save
│   │
│   └── [Cancel] → 20.5 Cancellation Dialog
│         ├── Blocked if: Job Cards exist, Challans dispatched, Invoiced
│         ├── Reason Code + Remarks
│         └── SO status → Cancelled; billing period freed (AMC)
│
│  ═══ DOWNSTREAM HANDOFF ═══════════════════════════════════════════
│  Once SO reaches "Open" status, the baton is passed to:
│  → Operations & Dispatch (Job Cards, Technician Routing)
│  → Inventory (Chemical / Product Issuance from Module 11)
│  → Invoicing & Billing
```

---

================================================================================

## Module Dependency Map

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                                                                                  │
│   MODULE 19           MODULE 17         MODULE 16         MODULE 18              │
│   Contract Mgmt       GMA Mgmt          Quotation Mgmt   Customer Mgmt          │
│   ┌──────────┐       ┌──────────┐      ┌──────────┐     ┌──────────┐           │
│   │ Active   │       │ Approved │      │ Approved │     │ Customer │           │
│   │ Contract │       │ GMA      │      │ Quotation│     │ ID, Name,│           │
│   │ (AMC SO  │       │ (One-Time│      │ (One-Time│     │ Sites,   │           │
│   │  source) │       │  source) │      │  source) │     │ Billing  │           │
│   └─────┬────┘       └─────┬────┘      └─────┬────┘     └─────┬────┘           │
│         │                  │                  │               │                 │
│         └──────────┐   ┌───┘       ┌──────────┘    ┌──────────┘                 │
│                    │   │           │               │                            │
│                    ▼   ▼           ▼               ▼                            │
│             ╔═══════════════════════════════════════════════════╗               │
│             ║       MODULE 20: SALES ORDER MANAGEMENT          ║               │
│             ║                                                    ║               │
│             ║  • AMC Source: Active Contract (M19)               ║               │
│             ║  • One-Time Source: Approved GMA/Quotation (M17/16)║               │
│             ║  • Product Source: Product Master (M10)            ║               │
│             ║  • Customer & Sites (M18)                         ║               │
│             ║  • Tax: HSN/SAC codes (M9)                        ║               │
│             ║  • Line Items, Taxes, Totals                      ║               │
│             ╚═══════════════╤═══════════════════════════════════╝               │
│                             │                                                   │
│   MODULE 10                 │                                                   │
│   Product Master            ▼                                                   │
│   ┌──────────┐   ╔═══════════════════════════════════╗                          │
│   │ Products,│   ║  DOWNSTREAM CONSUMERS              ║                          │
│   │ Prices,  │──►║  • Operations & Dispatch (Job Cards)║                          │
│   │ HSN, UOM │   ║  • Inventory (Module 11 – Issuance) ║                          │
│   └──────────┘   ║  • Invoicing & Billing              ║                          │
│                  ╚═══════════════════════════════════╝                          │
│   MODULE 9                                                                      │
│   Tax Management                                                                │
│   ┌──────────┐                                                                  │
│   │ HSN/SAC  │────► Tax rate lookup for line items                              │
│   │ Tax rates│                                                                  │
│   └──────────┘                                                                  │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## Data Flow Table

| Source Module  | Data Provided                                                 | Used In (SO Management)                                      |
| -------------- | ------------------------------------------------------------- | ------------------------------------------------------------ |
| **Module 19**  | Active Contract (customer, sites, services, payment schedule) | Section 1: AMC source; auto-generates SOs per billing period |
| **Module 17**  | Approved GMA (services, costs, sites, GM%)                    | Section 1: One-Time Service source                           |
| **Module 16**  | Approved Quotation (services, pricing)                        | Section 1: One-Time Service source                           |
| **Module 18**  | Customer ID, Name, Sites, Billing Address                     | Section 1: Customer details; Section 3: Delivery address     |
| **Module 10**  | Product Name, Code, UOM, Sale Price, HSN Code                 | Section 2: Product line items for Product Sale orders        |
| **Module 9**   | HSN/SAC codes, GST rates                                      | Section 2: Tax calculation on all line items                 |
| **Module 7**   | Branch list (Active branches)                                 | Section 1: Branch assignment                                 |
| **Module 20**  | SO Number, Status, Line Items, Execution Status               | Operations (Job Cards), Invoicing, Module 18 Tab 3, Module 19 Tab 2 |
