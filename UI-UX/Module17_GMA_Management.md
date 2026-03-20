# 🎯 MODULE 17: GROSS MARGIN ANALYSIS (GMA) MANAGEMENT

## Overview

A dynamic cost-and-margin calculation system that enables sales personnel to build detailed Gross Margin (GM) sheets for service proposals before contract creation. Supports multi-site, multi-service configurations with consumable chemical selection from the Products module, technician costing, revisit charges, and additional operational costs. Implements a three-tier approval workflow based on the automatically calculated gross margin percentage, ensuring profitability controls across all service agreements.

**Module Connections:**

- **Depends on:** Module 10 (Product Master – for consumable chemicals), Module 4 (Customer & Site Management), Module 5 (Service Templates), Module 8 (Employee / Technician Management), Module 9 (Tax Management)
- **Used by:** Module 18 (Contract Management – GMA approval is prerequisite for contract creation), Module 15 (Lead / Sales Pipeline), Module 16 (Quotation Management)
- **Prerequisites:** Products module must have consumable chemicals configured; service templates must be active; customer and site records must exist before creating a GMA sheet.

---

The module screen contains three tabs:

- → **Tab 1: GMA Entries** — View all GMA sheets accessible to the user (read-only overview)
- → **Tab 2: My Requests** — Track and manage GM sheets raised by the logged-in sales person
- → **Tab 3: Received Requests** — Approve or reject GM sheet requests received by managers / approvers

Each tab provides a different view based on the user's role in the GMA workflow.

---

# 17.1 Tab 1: GMA Dashboard – Table View

**Description:**
A consolidated view displaying all GMA sheets accessible to the user, filtered by status, service type, customer, branch, or date. Sales persons see their own entries; managers see all entries within their scope. Provides a summary overview without modification capability.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         GMA MANAGEMENT                                       │
│                                                                              │
│  [Tab 1: GMA Entries ●]  [Tab 2: My Requests]  [Tab 3: Received Requests]   │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                │  │
│  │                                                                        │  │
│  │ Customer       : [🔍 Search / Select ▼]                                │  │
│  │ Service Type   : [▼ All / AMC / One-Time / Quarterly / Fogging]        │  │
│  │ Status         : [☑ Auto-Approved ☑ Pending ☑ Approved ☑ Rejected]    │  │
│  │ Branch         : [▼ All Branches ▼]                                    │  │
│  │ Created By     : [▼ All / My Entries]                                  │  │
│  │ Date Range     : [📅 From] – [📅 To]                                   │  │
│  │                                                                        │  │
│  │ Search: [________________________] (GMA ID / Customer / Service Type)  │  │
│  │                                                    [Reset Filters]     │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  GMA TABLE                                                                   │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │GMA ID    │Customer    │Service Type│Branch│Sites│Total Cost│Sale Price│GM%│││
│  │──────────┼────────────┼────────────┼──────┼─────┼──────────┼──────────┼───││
│  │GMA-00091 │XYZ Hotel   │Cockroach   │MUM   │  3  │₹11,000   │₹19,000   │42%││
│  │GMA-00088 │ABC Pharma  │Rodent AMC  │BLR   │  1  │₹8,500    │₹11,000   │23%││
│  │GMA-00085 │DEF Mall    │General Pest│HYD   │  2  │₹14,200   │₹15,000   │6% ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Status         │Created By    │Created Date   │Actions                    ││
│  │───────────────┼──────────────┼───────────────┼───────────────────────────││
│  │🟢 Auto-Approved│Ravi Sharma  │18 Mar 2026    │[View]                     ││
│  │🟡 Pending Mgr  │Anjali M.    │17 Mar 2026    │[View]                     ││
│  │🔴 Pending CEO  │Suresh K.    │15 Mar 2026    │[View]                     ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   Next                              │
│                                                                              │
│  Legend: 🟢 Auto-Approved  🟡 Pending Manager  🔴 Pending CEO  ✅ Approved  ❌ Rejected │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table View Fields

| Field           | Type         | Description                                              |
| --------------- | ------------ | -------------------------------------------------------- |
| GMA ID          | Text (Auto)  | System-generated unique ID (e.g., GMA-00091)             |
| Customer        | Text         | Customer name from Customer Master                       |
| Service Type    | Text         | Service category (AMC / One-Time / Quarterly / etc.)     |
| Branch          | Text         | Branch responsible for this service proposal             |
| No. of Sites    | Number       | Total locations included in this GMA sheet               |
| Total Cost (₹)  | Currency     | Sum of all chemical + labor + revisit costs across sites |
| Sale Price (₹)  | Currency     | Total proposed price billed to customer                  |
| Gross Margin %  | Percentage   | Auto-calculated: `(Price – Cost) / Price × 100`          |
| Status          | Badge        | Auto-Approved / Pending Manager / Pending CEO / Approved / Rejected |
| Created By      | Text         | Sales person who created the GMA sheet                   |
| Created Date    | Date         | Date of GMA sheet creation                               |
| Actions         | Button Group | [View]                                                   |

---

## Filters

| Filter       | Type            | Options                                               |
| ------------ | --------------- | ----------------------------------------------------- |
| Customer     | Search/Dropdown | Search by name or select from Customer Master         |
| Service Type | Dropdown        | All / AMC / One-Time / Quarterly / Fogging / etc.     |
| Status       | Multi-select    | Auto-Approved / Pending / Approved / Rejected         |
| Branch       | Dropdown        | All Branches / Specific Branch                        |
| Created By   | Dropdown        | All / My Entries                                      |
| Date Range   | Date Range      | From – To (GMA creation date)                         |

---

## Search

Searchable by:
- GMA ID
- Customer Name
- Service Type

---

## Actions (Table Row)

| Action   | Description                                           |
| -------- | ----------------------------------------------------- |
| **View** | Opens the GMA sheet in read-only mode (Screen 17.1.1) |

---

================================================================================

# 17.1.1 View GMA Sheet (Tab 1 — Read Only)

**Description:**
Full read-only view of any GMA sheet accessible to the logged-in user. Identical to Screen 17.2.2 but without the Revoke option. Used for auditing, reference, and management review.

> Layout is identical to **17.2.2 View GMA Sheet (My Requests)**. Refer to that section for the full screen layout and field tables.

---

================================================================================

# 17.2 Tab 2: My Requests

**Description:**
Displays all GMA sheets created by the currently logged-in sales person. Includes an **Add GMA Sheet** button to create a new GM calculation. Users can view details of their submitted sheets or revoke those still in Pending status.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      MY GMA REQUESTS                                         │
│                                                                              │
│  [Tab 1: GMA Entries]  [Tab 2: My Requests ●]  [Tab 3: Received Requests]   │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Search: [______________________]      Status: [▼ All ▼]               │  │
│  │                                                                        │  │
│  │                                           [+ ADD GMA SHEET]           │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  MY REQUESTS TABLE                                                           │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │GMA ID    │Customer   │Service Type│GM%│Submitted Date│Status  │Actions   ││
│  │──────────┼───────────┼────────────┼───┼──────────────┼────────┼──────────││
│  │GMA-00091 │XYZ Hotel  │Cockroach   │42%│ 18 Mar 2026  │🟢 Auto │[View]    ││
│  │GMA-00088 │ABC Pharma │Rodent AMC  │23%│ 17 Mar 2026  │🟡 Pend │[V][Revoke]│
│  │GMA-00085 │DEF Mall   │General Pest│6% │ 15 Mar 2026  │🔴 Pend │[V][Revoke]│
│  │GMA-00080 │KLM Ind.   │Termite AMC │38%│ 10 Mar 2026  │✅ Apprd │[View]   ││
│  │GMA-00077 │PQR Office │Fogging     │8% │ 05 Mar 2026  │❌ Rejct │[View]   ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   Next                              │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table Fields

| Field          | Description                                              |
| -------------- | -------------------------------------------------------- |
| GMA ID         | System-generated unique reference                        |
| Customer       | Customer name linked to the GMA sheet                    |
| Service Type   | Type of pest control service                             |
| GM %           | Gross margin percentage (auto-calculated)                |
| Submitted Date | Date the GMA sheet was submitted for approval            |
| Status         | Auto-Approved / Pending Manager / Pending CEO / Approved / Rejected |
| Actions        | View / Revoke                                            |

---

## Actions

| Action     | Condition                | Description                                                   |
| ---------- | ------------------------ | ------------------------------------------------------------- |
| **View**   | All statuses             | Opens GMA sheet in read-only mode (Screen 17.2.2)            |
| **Revoke** | Status = Pending only    | Cancels the submitted request and resets it to Draft status   |

---

## Form Actions

| Action              | Description                                                          |
| ------------------- | -------------------------------------------------------------------- |
| **+ Add GMA Sheet** | Opens the **Add GMA Sheet Form** (Screen 17.2.1) to create a new sheet |

---

================================================================================

# 17.2.1 Add GMA Sheet

**Description:**
A multi-part form that allows a sales person to build a complete Gross Margin Analysis sheet for a service proposal. The form mirrors Module 16's source selection pattern (From Lead / From Customer / Add New) and calculates GM% using the PESTMED GMA template structure:

- **Section 1:** Source Selection (identical to Module 16 Add Quotation)
- **Section 2:** Service & Contract Configuration
- **Section 3:** Site-Wise Cost Breakdown (Service Visit Cost + Manpower Cost + Chemical Cost + Weekends/Nights Surcharge + Documentation Cost)
- **Section 4:** Overall GM Summary with automatic approval routing

Chemical products are pulled from the **Products Module (Module 10 — consumable type)**. The chemical cost for each month is entered via a 12-month input grid (as per PESTMED Chemical Input sheet), and the system auto-calculates full-year cost. Service visit rate, manpower hourly rate, and frequency are user-configurable. The final GM% determines the approval workflow.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to My Requests]              ADD GMA SHEET                         │
│                                                                              │
│  GMA ID: GMA-2026-XXXX (Auto-generated on save)                             │
│                                                                              │
│  SECTION 1: SOURCE SELECTION                                                 │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Create GMA For*:                                                       │ │
│  │  (•) From Lead    ( ) From Customer    ( ) Add New                      │ │
│  │                                                                         │ │
│  │  IF "FROM LEAD" SELECTED:                                               │ │
│  │  Select Lead*:        [🔍 Search Lead by Name / ID / Phone ▼]          │ │
│  │                                                                         │ │
│  │  AUTO-FILLED FROM LEAD:                                                 │ │
│  │  Lead ID:             LD-2026-00142 (Read-only)                         │ │
│  │  Contact Person:      Rahul Sharma (Read-only)                          │ │
│  │  Phone:               +91 98765 43210 (Read-only)                       │ │
│  │  Email:               rahul@example.com (Read-only)                     │ │
│  │  Lead Type:           Service (Read-only)                               │ │
│  │  Lead Status:         QUALIFIED ✅ (Read-only)                          │ │
│  │                                                                         │ │
│  │  IF "FROM CUSTOMER" SELECTED:                                           │ │
│  │  Select Customer*:    [🔍 Search Customer by Name / ID / Phone ▼]      │ │
│  │                                                                         │ │
│  │  AUTO-FILLED FROM CUSTOMER:                                             │ │
│  │  Customer ID:         CUST-00245 (Read-only)                            │ │
│  │  Customer Name:       ABC Corporation (Read-only)                       │ │
│  │  Phone:               +91 98765 12345 (Read-only)                       │ │
│  │  Email:               contact@abc.com (Read-only)                       │ │
│  │  Customer Type:       Service (Read-only)                               │ │
│  │  Address:             Koramangala, Bengaluru (Read-only)                 │ │
│  │                                                                         │ │
│  │  IF "ADD NEW" SELECTED:                                                 │ │
│  │  Full Name*:          [________________________________]                │ │
│  │  Phone*:              [________________________________]                │ │
│  │  Email:               [________________________________]                │ │
│  │  Address*:            [________________________________]                │ │
│  │  City*:               [________________________________]                │ │
│  │  State*:              [▼ Select State ▼]                                │ │
│  │  Pincode:             [________]                                        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 2: SERVICE & CONTRACT CONFIGURATION                                 │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Pest Type*:  [▼ Cockroach / Termite / Rodent / General Pest │ │
│  │                          / Fogging / Fumigation / Mosquito / Other ▼]   │ │
│  │                                                                         │ │
│  │  Service Mode*:         (•) Contract base    ( ) One-Time                         │ │
│  │                                                                         │ │
│  │  IF AMC SELECTED:                                                       │ │
│  │  Contract Duration*:   [▼ 6 Months / 1 Year / 2 Years / 3 Years ▼/ custom]     │ │
│  │  Proposed Start Date*: [📅 Date Picker]                                 │ │
│  │  Frequency*:           [▼ Weekly / Fortnightly / Monthly / Quarterly / Custom ▼] │ │
│  │  Annual Frequency*:    [ 52 ] (Auto-calculated from Frequency/for custom editable)          │ 
│  │                                                                         │ │
│  │  Branch*:              [▼ Select Branch ▼]                              │ │
│  │  Prepared By:          [Auto: Logged-in User] (Read-only)               │ │
│  │  Prepared Date:        [Auto: Today's Date] (Read-only)                 │ │
│  │  Remarks / Notes:      [___________________________________________]    │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 3: COST BREAKDOWN                                                    │
│  [+ ADD SITE]                                                                │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  SITE 1 (Site Name / Location*)                       [Remove Site]     │ │
│  │  ┌──────────────────────────────────────────────────────────────────┐  │ │
│  │  │ Address:  [________________________________]                     │  │ │
│  │  │ City:    [________________]  State: [▼ Select State ▼]        │  │ │
│  │  │ Category*:     [▼ Residential / Commercial / Industrial ▼]      │  │ │
│  │  │ Sub-Category*: [▼ Internal / External ▼]                        │  │ │
│  │  │ Area (sqft)*: [________]   Visits/Month*: [4] (from Section 2)  │  │ │
│  │  └──────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                         │ │
│  │  ─── 3A: SERVICE VISIT COST ────────────────────────────────────────── │ │
│  │  ┌─────────────────────────────────────────────────────────────────┐   │ │
│  │  │ Rate per Visit (₹)*:  [ 300   ]                                │   │ │
│  │  │ Annual Frequency:     [ 52    ] (from Section 2, read-only)    │   │ │
│  │  │ ─────────────────────────────────────────────────────────────── │   │ │
│  │  │ SERVICE VISIT COST/YEAR (A) : ₹ 15,600  (Rate × Frequency)    │   │ │
│  │  │ SERVICE VISIT COST/MONTH    : ₹ 1,300   (A ÷ 12)              │   │ │
│  │  └─────────────────────────────────────────────────────────────────┘   │ │
│  │                                                                         │ │
│  │  ─── 3B: MANPOWER / LABOR COST ────────────────────────────────────── │ │
│  │  ┌─────────────────────────────────────────────────────────────────┐   │ │
│  │  │ No. of Hours per Visit*: [ 4     ]                             │   │ │
│  │  │ Annual Frequency:        [ 52    ] (read-only)                 │   │ │
│  │  │ Rate per Hour (₹)*:      [ 100   ]                             │   │ │
│  │  │ ─────────────────────────────────────────────────────────────── │   │ │
│  │  │ MANPOWER COST/YEAR (B): ₹ 20,800 (Hours × Freq × Rate/Hr)    │   │ │
│  │  │ MANPOWER COST/MONTH   : ₹ 1,733  (B ÷ 12)                    │   │ │
│  │  └─────────────────────────────────────────────────────────────────┘   │ │
│  │                                                                         │ │
│  │  ─── 3C: CHEMICAL / PRODUCT COST ─────────────────────────────────── │ │
│  │  (Auto-fetched from Module 12 based on selected Pest/Service Type)     │ │
│  │  (Product details & prices auto-fetched from Module 10 Product Master) │ │
│  │                                                                         │ │
│  │  ┌──────────────┬──────┬─────┬─────────┬──────────────┬────────────┬──────────────┬──────────────────┬──────────────────┬──────┐│ │
│  │  │ Product Name │ Code │ UOM │Dilution │Coverage(SQFT)│ Req. Qty   │ Price/UOM(₹) │ Cost/Visit(₹)    │ Est.Cost/Month   │      ││ │
│  │  │ (Auto)       │(Auto)│(Auto│ (Auto)  │ [User Entry] │ [User Ent.]│ (Auto-M10)   │ (Auto)           │ (Auto)           │      ││ │
│  │  ├──────────────┼──────┼─────┼─────────┼──────────────┼────────────┼──────────────┼──────────────────┼──────────────────┼──────┤│ │
│  │  │Alpha Cyperm. │P-001 │ ml  │ 10 ml   │ [1200] SQFT  │ [120] ml   │ ₹4.20 (Auto) │ ₹504 (Auto)      │ ₹2,016 (Auto)    │ [🗑] ││ │
│  │  │Chlorpyriphos │P-002 │ ml  │ 20 ml   │ [1200] SQFT  │ [60] ml    │ ₹3.50 (Auto) │ ₹210 (Auto)      │ ₹840 (Auto)      │ [🗑] ││ │
│  │  │Fipronil Gel  │P-003 │ tube│ 1 tube  │ [1200] SQFT  │ [2] tubes  │ ₹220 (Auto)  │ ₹440 (Auto)      │ ₹1,760 (Auto)    │ [🗑] ││ │
│  │  └──────────────┴──────┴─────┴─────────┴──────────────┴────────────┴──────────────┴──────────────────┴──────────────────┴──────┘│ │
│  │                                                                         │ │
│  │  [+ Add Custom Chemical]  (for items not in service config)             │ │
│  │                                                                         │ │
│  │  Visits/Month (Reference): [4] (Auto from Section 2)                    │ │
│  │                                                                         │ │
│  │  ─── CHEMICAL COST SUMMARY ─────────────────────────────────────────── │ │
│  │  Total Chemical Cost / Visit  : ₹ 1,154  (Sum of all Cost/Visit rows)  │ │
│  │  Total Chemical Cost / Month  : ₹ 4,616  (Sum of all Cost/Month rows)  │ │
│  │  CHEMICAL COST/YEAR (C)       : ₹ 55,392 (Cost/Month × 12)             │ │
│  │                                                                         │ │
│  │  ─── 3D: WEEKENDS / NIGHTS SURCHARGE ──────────────────────────────── │ │
│  │  ┌─────────────────────────────────────────────────────────────────┐   │ │
│  │  │ Applicable?        (•) No    ( ) Yes                           │   │ │
│  │  │                                                                │   │ │
│  │  │ IF YES: 25% surcharge on (Service Visit + Manpower Costs)      │   │ │
│  │  │ Surcharge Base     : ₹ 36,400  (A + B)                         │   │ │
│  │  │ Surcharge Rate     : 25%                                       │   │ │
│  │  │ SURCHARGE COST (D) : ₹ 0  (Not Applicable)                    │   │ │
│  │  └─────────────────────────────────────────────────────────────────┘   │ │
│  │                                                                         │ │
│  │  ─── 3E: DOCUMENTATION COST (Optional) ────────────────────────────── │ │
│  │  ┌─────────────────────────────────────────────────────────────────┐   │ │
│  │  │ Applicable?        ( ) No    (•) Yes                           │   │ │
│  │  │ Cost per Document (₹): [ 100  ]                                │   │ │
│  │  │ Docs per Month      : [ 1    ] (e.g., monthly service docket)  │   │ │
│  │  │ DOCUMENTATION COST/YEAR (E): ₹ 1,200 (Cost × Docs × 12)       │   │ │
│  │  └─────────────────────────────────────────────────────────────────┘   │ │
│  │                                                                         │ │
│  │  ═══ SITE 1 COST SUMMARY ════════════════════════════════════════════ │ │
│  │  ┌─────────────────────────────────────────────────────────────────┐   │ │
│  │  │ Cost Component          │ Annual (₹)     │ Monthly (₹)         │   │ │
│  │  │ ────────────────────────┼────────────────┼─────────────────────│   │ │
│  │  │ A. Service Visit Cost   │ ₹ 15,600       │ ₹ 1,300             │   │ │
│  │  │ B. Manpower Cost        │ ₹ 20,800       │ ₹ 1,733             │   │ │
│  │  │ C. Chemical Cost        │ ₹ 55,392       │ ₹ 4,616             │   │ │
│  │  │ D. Weekend/Night Surch. │ ₹ 0            │ ₹ 0                 │   │ │
│  │  │ E. Documentation Cost   │ ₹ 1,200        │ ₹ 100               │   │ │
│  │  │ ─────────────────────────────────────────────────────────────── │   │ │
│  │  │ TOTAL COST              │ ₹ 92,992       │ ₹ 7,749             │   │ │
│  │  └─────────────────────────────────────────────────────────────────┘   │ │
│  │                                                                         │ │
│  │  SITE 1 PROPOSED SALE PRICE/YEAR* : ₹ [ 1,56,000 ] (Manual Entry)      │ │
│  │  SITE 1 PROPOSED SALE PRICE/MONTH : ₹ 13,000  (Auto ÷ 12)              │ │
│  │  ──────────────────────────────────────────────────────────────────     │ │
│  │  ★ SITE 1 GROSS MARGIN %         : 40.4%  (Auto-calculated)            │ │
│  │    Formula: (Sale Price – Total Cost) / Sale Price × 100                │ │
│  │    = (₹1,56,000 – ₹92,992) / ₹1,56,000 × 100 = 40.4%                  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  [+ ADD SITE]  (Repeat above block for each additional site)                 │
│                                                                              │
│  SECTION 4: OVERALL GM SUMMARY                                               │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  ┌─────────────────────────────────────────────────────────────────┐   │ │
│  │  │ Site                │ Total Cost/Yr │ Sale Price/Yr │ GM %     │   │ │
│  │  │ ────────────────────┼───────────────┼───────────────┼─────────│   │ │
│  │  │ Site 1: Mumbai HQ   │ ₹ 92,992      │ ₹ 1,56,000    │ 40.4%  │   │ │
│  │  │ Site 2: Pune WH     │ ₹ 28,000      │ ₹ 48,000      │ 41.7%  │   │ │
│  │  │ ─────────────────────────────────────────────────────────────  │   │ │
│  │  │ TOTAL ANNUAL COST   │ ₹ 1,20,992                               │   │ │
│  │  │ TOTAL ANNUAL PRICE  │ ₹ 2,04,000                               │   │ │
│  │  │ ─────────────────────────────────────────────────────────────  │   │ │
│  │  │ ★ OVERALL GROSS MARGIN : 40.7%                                │   │ │
│  │  └─────────────────────────────────────────────────────────────────┘   │ │
│  │                                                                         │ │
│  │  GM% without Documentation : 41.3%                                      │ │
│  │  GM% with Documentation    : 40.7%                                      │ │
│  │                                                                         │ │
│  │  Approval Status (Manual Selection if < 40%):                           │ │
│  │  ┌──────────────────────────────────────────────────────────────────┐  │ │
│  │  │ GM ≥ 40%        → ✅ Auto-Approved       (Immediate)             │  │ │
│  │  │ GM < 40%        → 🟡 Requires Manual Approver Selection          │  │ │
│  │  └──────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                         │ │
│  │  CURRENT STATUS: ✅ AUTO-APPROVED (GM 40.7% ≥ 40%)                      │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  Sales Authority Matrix:                                                     │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Authorizer           │ Contracts        │ Jobs/Products   │ Signature  │ │
│  │  ─────────────────────┼──────────────────┼─────────────────┼──────────│ │
│  │  Auto (System)        │ GM ≥ 40%         │ GM ≥ 40%        │ [Auto]   │ │
│  │  Sales Manager        │ GM 20% – 39.99%  │ GM 20% – 39.99% │ [_____] │ │
│  │  CEO / Ops Head       │ GM < 20%         │ GM < 20%        │ [_____]  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│              [SAVE & SUBMIT (Auto)]  [SAVE & REQUEST APPROVAL]  [CANCEL]     │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Section 1: Source Selection Fields

| Field           | Type            | Required    | Options/Validation                                                              | Notes                              |
| --------------- | --------------- | ----------- | ------------------------------------------------------------------------------- | ---------------------------------- |
| Source Type     | Radio           | Yes         | From Lead / From Customer / Add New                                             | Determines which sub-fields appear |
| Select Lead     | Search Dropdown | Conditional | Active leads from Module 15 (Status ≥ QUALIFIED)                                | Required if Source = From Lead     |
| Select Customer | Search Dropdown | Conditional | Active customers from Module 4                                                  | Required if Source = From Customer |
| Full Name       | Text            | Conditional | Min 3 characters                                                                | Required if Source = Add New       |
| Phone           | Number          | Conditional | 10-digit Indian mobile                                                          | Required if Source = Add New       |
| Email           | Email           | No          | Valid email format                                                              | Optional for Add New               |
| Company Name    | Text            | No          | Max 100 characters                                                              | Optional for Add New               |
| Address         | Text            | Conditional | Min 10 characters                                                               | Required if Source = Add New       |
| City            | Text            | Conditional | Min 3 characters                                                                | Required if Source = Add New       |
| State           | Dropdown        | Conditional | Indian states list                                                              | Required if Source = Add New       |
| Pincode         | Number          | No          | 6-digit                                                                         | Optional                           |

---

## Section 2: Service & Contract Configuration Fields

| Field              | Type        | Required    | Options/Validation                                            | Notes                                    |
| ------------------ | ----------- | ----------- | ------------------------------------------------------------- | ---------------------------------------- |
| Pest / Service Type| Dropdown    | Yes         | Cockroach / Termite / Rodent / General Pest / Fogging / Fumigation / Mosquito / Other | Linked to Module 12 service list |
| Service Mode       | Radio       | Yes         | AMC / One-Time                                                | Determines frequency fields              |
| Frequency          | Dropdown    | Conditional | Weekly / Fortnightly / Monthly / Quarterly                    | Required if Mode = AMC                   |
| Annual Frequency   | Number      | Auto        | Auto-calculated (Weekly=52, Fortnightly=26, Monthly=12, Quarterly=4) | Read-only                         |
| Contract Duration  | Dropdown    | Conditional | 6 Months / 1 Year / 2 Years / 3 Years                        | Required if Mode = AMC                   |
| Proposed Start Date| Date        | Conditional | ≥ Today                                                       | Required if Mode = AMC                   |
| Branch             | Dropdown    | Yes         | Active branches from Module 7                                 | Nearest branch auto-suggested            |
| Prepared By        | Auto-filled | System      | Logged-in user                                                | Read-only                                |
| Prepared Date      | Auto-filled | System      | Today's date                                                  | Read-only                                |
| Remarks / Notes    | Text Area   | No          | Max 500 characters                                            | Free-text for special instructions       |

---

## Section 3: Cost Breakdown (Per Site)

### Site Information

| Field              | Type            | Required | Validation                                                | Notes                                |
| ------------------ | --------------- | -------- | --------------------------------------------------------- | ------------------------------------ |
| Site Name/Location | Text            | Yes      | Free-text name (e.g., "Mumbai HQ")                        | Identifies this site in summary      |
| Address            | Text            | No       | Min 10 characters if provided                             | Optional; service delivery address   |
| City               | Text            | Yes      | Min 3 characters                                          | City name                            |
| State              | Dropdown        | Yes      | Indian states list                                        | State selection                      |
| Category           | Dropdown        | Yes      | Residential / Commercial / Industrial                     | Linked to Module 12 pricing          |
| Sub-Category       | Dropdown        | Yes      | Internal / External                                       | Linked to Module 12                  |
| Area (sqft)        | Number          | Yes      | Must be > 0                                               | Used for area-based chemical calc    |
| Visits/Month       | Number          | Yes      | Inherited from Section 2; site-editable                   | Can be overridden per site           |

---

### 3A — Service Visit Cost

| Field              | Type            | Required | Validation / Notes                                                |
| ------------------ | --------------- | -------- | ----------------------------------------------------------------- |
| Rate per Visit (₹) | Currency        | Yes      | Manually entered; cost charged per visit (e.g., ₹300)             |
| Annual Frequency   | Display         | Auto     | From Section 2 (e.g., Weekly = 52)                                |
| Cost/Year (A)      | Auto-calculated | System   | `Rate per Visit × Annual Frequency`                               |
| Cost/Month         | Auto-calculated | System   | `A ÷ 12`                                                         |

---

### 3B — Manpower / Labor Cost

| Field                  | Type            | Required | Validation / Notes                                            |
| ---------------------- | --------------- | -------- | ------------------------------------------------------------- |
| No. of Hours per Visit | Number          | Yes      | Hours of technician work per visit (e.g., 4)                  |
| Annual Frequency       | Display         | Auto     | Same as Section 2 (read-only)                                 |
| Rate per Hour (₹)      | Currency        | Yes      | Hourly rate for technician labor (e.g., ₹100)                 |
| Cost/Year (B)          | Auto-calculated | System   | `Hours × Annual Frequency × Rate/Hour`                        |
| Cost/Month             | Auto-calculated | System   | `B ÷ 12`                                                     |

---

### 3C — Chemical / Product Cost (Auto-fetched from Module 12 → Module 10)

> When a **Pest / Service Type** is selected in Section 2, the system automatically fetches all chemicals/products configured for that service in **Module 12 (Service Management → Section 4: Chemicals/Products Used)**. Product details (Code, UOM, Dilution, Coverage, Price/UOM) are auto-populated from **Module 10 (Product Master)**.
>
> The user only needs to enter **SQFT** (coverage area for this site) and **Required Quantity**. All pricing and cost calculations happen dynamically.

| Field                    | Type            | Required | Validation / Notes                                                          |
| ------------------------ | --------------- | -------- | --------------------------------------------------------------------------- |
| Product Name             | Auto-filled     | System   | Auto-fetched from Module 12 service config → Module 10 Product Master      |
| Product Code             | Auto-filled     | System   | Auto-fetched from Module 10 (e.g., P-001)                                  |
| UOM                      | Auto-filled     | System   | Base UOM from Module 10 (ml / Ltr / gm / kg / Nos / tube)                 |
| Dilution                 | Auto-filled     | System   | Standard dilution dose from Module 12 config (e.g., 10 ml per 100 SQFT)   |
| Coverage (SQFT)          | Number          | Yes      | **User enters** — area to be treated (pre-filled from site Area if set)    |
| Required Qty             | Number          | Yes      | **User enters** — quantity needed per visit; must be > 0                   |
| Price / UOM (₹)          | Auto-filled     | System   | **Purchase Price from Module 10** Product Master (editable for override)   |
| Cost / Visit (₹)         | Auto-calculated | System   | `Required Qty × Price per UOM`                                             |
| Est. Cost / Month (₹)    | Auto-calculated | System   | `Cost per Visit × Visits per Month`                                        |
| Custom Chemical          | Text            | No       | Allowed via [+ Add Custom Chemical] if not in service config               |

**System Behavior — On Service Selection (Section 2):**

When a Pest / Service Type is selected, the system:

| Auto-Action                        | Source                                                     |
| ---------------------------------- | ---------------------------------------------------------- |
| Pre-populate chemical rows         | Module 12 → Service → Chemicals/Products Used              |
| Fill Product Name, Code            | Module 10 → Product Master record                          |
| Fill UOM                           | Module 10 → Base UOM                                       |
| Fill Dilution                      | Module 12 → Standard Usage (if configured)                 |
| Fill Price / UOM (₹)               | Module 10 → Purchase Price                                 |
| Pre-fill Coverage (SQFT)           | From Site's Area (sqft) field (user can adjust)            |

> The **Price / UOM** is editable — the sales person can override the fetched price if a negotiated rate applies.

**Calculation Rules:**

| Calculation                  | Formula                                          |
| ---------------------------- | ------------------------------------------------ |
| Cost per Visit (₹)           | `Required Qty × Price per UOM`                   |
| Est. Cost / Month (₹)        | `Cost per Visit × Visits per Month`              |
| Total Chemical Cost / Visit  | Sum of all chemical rows' Cost/Visit             |
| Total Chemical Cost / Month  | Sum of all chemical rows' Est. Cost/Month        |
| Chemical Cost / Year (C)     | `Total Chemical Cost / Month × 12`               |

**UOM-Based Dynamic Calculation Examples:**

| UOM    | Scenario                                                              | Calculation                                           |
| ------ | --------------------------------------------------------------------- | ----------------------------------------------------- |
| ml     | Alpha Cypermethrin, 1200 SQFT, 10ml/100SQFT dilution → 120ml needed  | 120 ml × ₹4.20/ml = ₹504/visit                       |
| tube   | Fipronil Gel, 1200 SQFT, 1 tube/600SQFT → 2 tubes needed            | 2 tubes × ₹220/tube = ₹440/visit                     |
| grams  | Bromadiolone, 1200 SQFT, 100g/site → 100g needed                    | 100 g × ₹0.25/g = ₹25/visit                          |
| Ltr    | Chlorpyriphos 20% EC, 1200 SQFT, 1L/1000SQFT → 1.2L needed         | 1.2 L × ₹290/L = ₹348/visit                          |
| Nos    | Glue Trap, 1200 SQFT, 1 per 200SQFT → 6 needed                     | 6 Nos × ₹15/No = ₹90/visit                           |

**Rules:**
- Chemicals are auto-populated when service is selected; user can remove unwanted items.
- At least one chemical row is required per site.
- Additional chemicals can be added via **[+ Add Custom Chemical]** for items not in the service config.
- Chemical Cost/Year (C) = Total Chemical Cost / Month × 12.
- User can override `Price / UOM` for custom pricing; system retains overridden value.

---

### 3D — Weekends / Nights Surcharge

| Field                | Type            | Required | Validation / Notes                                                    |
| -------------------- | --------------- | -------- | --------------------------------------------------------------------- |
| Applicable?          | Radio (Yes/No)  | Yes      | If "Yes", a 25% surcharge is applied on `(Service Visit + Manpower)` |
| Surcharge Percentage | Display         | Auto     | Fixed at 25% (configurable by admin)                                 |
| Surcharge Cost (D)   | Auto-calculated | System   | `IF Yes → (A + B) × 25%  ELSE → ₹0`                                 |

---

### 3E — Documentation Cost (Optional)

| Field                   | Type            | Required | Validation / Notes                                          |
| ----------------------- | --------------- | -------- | ----------------------------------------------------------- |
| Applicable?             | Radio (Yes/No)  | No       | If "Yes", documentation cost fields appear                  |
| Cost per Document (₹)   | Currency        | Cond.    | ₹100 default (e.g., service docket submission cost)         |
| Documents per Month     | Number          | Cond.    | Number of docs submitted monthly (e.g., 1)                  |
| Documentation Cost (E)  | Auto-calculated | System   | `Cost per Doc × Docs/Month × 12`                            |

---

### Site Cost Summary (Auto-calculated per Site)

| Component                  | Formula                                                                |
| -------------------------- | ---------------------------------------------------------------------- |
| A. Service Visit Cost/Year | `Rate per Visit × Annual Frequency`                                    |
| B. Manpower Cost/Year      | `Hours per Visit × Annual Frequency × Rate per Hour`                   |
| C. Chemical Cost/Year      | `Σ (Req Qty × Price/UOM × Visits/Month) × 12` for all chemicals       |
| D. Weekend/Night Surcharge | `IF Applicable → (A + B) × 25%  ELSE → 0`                             |
| E. Documentation Cost/Year | `IF Applicable → Cost/Doc × Docs/Month × 12  ELSE → 0`               |
| **Site Total Cost/Year**   | **A + B + C + D + E**                                                  |
| Site Proposed Price/Year   | Manually entered by the sales person                                   |
| **Site Gross Margin %**    | **`(Proposed Price – Total Cost) / Proposed Price × 100`**             |

---

## Section 4: Overall GM Summary (Auto-calculated)

| Field                       | Value                                                               |
| --------------------------- | ------------------------------------------------------------------- |
| Each site's cost row        | Site Name / Total Cost/Yr / Sale Price/Yr / Site GM%                |
| Total Annual Cost           | Sum of all site total costs                                         |
| Total Annual Price          | Sum of all site proposed prices                                     |
| GM% without Documentation   | `(Price – (A+B+C+D)) / Price × 100`                                |
| GM% with Documentation      | `(Price – (A+B+C+D+E)) / Price × 100`                              |
| Overall Gross Margin %      | `(Total Price – Total Cost) / Total Price × 100`                    |
| Approval Determination      | Auto-applied based on the GM% and Approval Matrix                   |

---

## Approval Matrix

| Gross Margin %  | Approver               | Timeline  | Outcome                               |
## Approval Matrix

| Authorizer         | Contracts        | Jobs / Products  | Signature Required |
| ------------------ | ---------------- | ---------------- | ------------------ |
| Auto (System)      | GM ≥ 40%         | GM ≥ 40%         | Not required       |
| Sales Manager      | GM 20% – 39.99%  | GM 20% – 39.99%  | Required           |
| CEO / Ops Head     | GM < 20%         | GM < 20%         | Required           |

> If the calculated GM% is ≥ 40%, the GMA sheet will be auto-approved. If the GM% is < 40%, the user must manually select an approver from a popup list to route the request successfully.

---

## Form Actions

| Button                       | Description                                                                  |
| ---------------------------- | ---------------------------------------------------------------------------- |
| **Save & Submit (Auto)**     | Appears if GM ≥ 40%. Validates, saves, and Auto-Approves the GMA.              |
| **Save & Request Approval**  | Appears if GM < 40%. Validates data and triggers the "Select Approver" popup.  |
| **Cancel**                   | Discards all entries and returns to My Requests table.                         |

---

## Select Approver Popup (If GM < 40%)

When the user clicks **Save & Request Approval**, this popup overlay appears:

```
┌────────────────────────────────────────────────────────┐
│  SELECT APPROVER                                       │
│                                                        │
│  Overall GM% is 23%. This requires approval.           │
│  Please select the appropriate authority based on      │
│  the Sales Authority Matrix.                           │
│                                                        │
│  Approver*: [ ▼ Select User (Sales Mgr, CEO) ▼ ]       │
│                                                        │
│             [SEND REQUEST]     [CANCEL]                │
└────────────────────────────────────────────────────────┘
```
- Clicking **Send Request** routes the GMA to the selected user's *Received Requests* tab and changes status to *Pending*.

---

## Validation Rules

| Validation                         | Rule                                                          |
| ---------------------------------- | ------------------------------------------------------------- |
| Source Required                    | Must select From Lead, From Customer, or Add New              |
| Lead/Customer Required             | Must select a valid active lead or customer (if applicable)   |
| Service Type Required              | Must select a pest / service type                             |
| At Least 1 Site Required           | Minimum one site must be added                                |
| At Least 1 Chemical per Site       | Minimum one chemical row per site                             |
| Coverage (SQFT) > 0               | Each chemical row must have SQFT > 0                          |
| Required Qty > 0                   | Each chemical row must have Required Qty > 0                  |
| Proposed Sale Price > 0            | Each site must have a proposed price greater than 0           |
| Rate per Visit > 0                 | Service visit rate cannot be zero                             |
| Hours per Visit ≥ 1               | Must have at least 1 hour per visit                           |
| Rate per Hour > 0                  | Manpower hourly rate cannot be zero                           |

---

## System Behaviors on Submission

| Trigger                     | System Action                                                         |
| --------------------------- | --------------------------------------------------------------------- |
| GM ≥ 40%                    | Status → Auto-Approved; no approval routing needed                    |
| GM < 40% + Approver Picked  | Status → Pending; Notification sent to selected Approver              |
| Chemical selected           | Auto-fill UOM, Base Price, +15%, +18% GST from Product Master         |
| Rate overridden by user     | System retains overridden value; does not reset on re-selection       |
| Source = From Lead          | Auto-populate lead details; link GMA to lead record                   |
| Source = From Customer      | Auto-populate customer details; link GMA to customer record           |
| Weekend/Night = Yes         | Automatically add 25% surcharge to Service Visit + Manpower costs     |

---

================================================================================

# 17.2.2 View GMA Sheet (My Requests)

**Description:**
Read-only view of a GMA sheet submitted by the logged-in user. Displays source information, service & contract configuration, site-wise cost breakdowns (Service Visit, Manpower, Chemical/Product, Surcharges, Documentation), and the final gross margin summary with approval status and timeline.

> All fields are displayed in **READ-ONLY** mode. Structure mirrors the Add GMA Sheet form (17.2.1) with identical sections.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       VIEW GMA SHEET  –  GMA-00091                          │
│  [← Back to My Requests]                                                     │
│                                                                              │
│  ─── HEADER ──────────────────────────────────────────────────────────────  │
│  GMA ID          : GMA-00091         Status     : ✅ AUTO-APPROVED           │
│  Created By      : Ravi Sharma       Created On : 18 Mar 2026               │
│  Submitted On    : 18 Mar 2026       Approved On: 18 Mar 2026 (Immediate)   │
│                                                                              │
│  ─── SECTION 1: SOURCE INFORMATION ──────────────────────────────────────  │
│  Source Type     : From Customer                                             │
│  Customer ID     : CUST-4021         Customer Name : XYZ Hotel Chain         │
│  Phone           : +91 98765 00001   Email : ramesh@xyz.com                  │
│  Customer Type   : Commercial        Address : Andheri East, Mumbai          │
│                                                                              │
│  ─── SECTION 2: SERVICE & CONTRACT CONFIGURATION ────────────────────────  │
│  Pest Type       : Cockroach                                                 │
│  Service Mode    : Contract base     Contract Duration : 1 Year              │
│  Proposed Start  : 01 Apr 2026       Frequency : Weekly (52 visits/year)     │
│  Branch          : Mumbai            Prepared By : Ravi Sharma               │
│  Prepared Date   : 18 Mar 2026                                               │
│  Remarks         : Annual pest control for hotel chain                       │
│                                                                              │
│  ─── SECTION 3: SITE-WISE COST BREAKDOWN ────────────────────────────────  │
│                                                                              │
│  SITE 1: Mumbai HQ                                                           │
│  ┌────────────────────────────────────────────────────────────────────────┐ │
│  │  City: Mumbai   State: Maharashtra   Category: Commercial              │ │
│  │  Sub-Category: Internal   Area: 1200 sqft   Visits/Month: 4           │ │
│  │                                                                         │ │
│  │  ─── 3A: SERVICE VISIT COST ──────────────────────────────────────── │ │
│  │  Rate per Visit : ₹300   Annual Frequency : 52                         │ │
│  │  Cost/Year (A)  : ₹15,600   Cost/Month : ₹1,300                       │ │
│  │                                                                         │ │
│  │  ─── 3B: MANPOWER / LABOR COST ──────────────────────────────────── │ │
│  │  Hours/Visit : 4   Annual Frequency : 52   Rate/Hour : ₹100            │ │
│  │  Cost/Year (B)  : ₹20,800   Cost/Month : ₹1,733                       │ │
│  │                                                                         │ │
│  │  ─── 3C: CHEMICAL / PRODUCT COST ────────────────────────────────── │ │
│  │  (Auto-fetched from Module 12 → Module 10)                              │ │
│  │  ┌──────────────┬──────┬─────┬─────────┬──────┬────────┬──────────┬──────────┐│
│  │  │ Product Name │ Code │ UOM │Dilution │ SQFT │Req.Qty │Price/UOM │Cost/Visit││
│  │  ├──────────────┼──────┼─────┼─────────┼──────┼────────┼──────────┼──────────┤│
│  │  │Alpha Cyperm. │P-001 │ ml  │ 10 ml   │ 1200 │ 120 ml │ ₹4.20   │ ₹504     ││
│  │  │Chlorpyriphos │P-002 │ ml  │ 20 ml   │ 1200 │ 60 ml  │ ₹3.50   │ ₹210     ││
│  │  │Fipronil Gel  │P-003 │ tube│ 1 tube  │ 1200 │ 2 tubes│ ₹220    │ ₹440     ││
│  │  └──────────────┴──────┴─────┴─────────┴──────┴────────┴──────────┴──────────┘│
│  │  Total Chemical Cost/Visit : ₹1,154   Cost/Month : ₹4,616              │ │
│  │  Chemical Cost/Year (C)    : ₹55,392                                    │ │
│  │                                                                         │ │
│  │  ─── 3D: WEEKENDS/NIGHTS SURCHARGE ──────────────────────────────── │ │
│  │  Applicable : No   Surcharge Cost (D) : ₹0                             │ │
│  │                                                                         │ │
│  │  ─── 3E: DOCUMENTATION COST ─────────────────────────────────────── │ │
│  │  Applicable : Yes   ₹100/doc × 1 doc/month × 12                        │ │
│  │  Documentation Cost/Year (E) : ₹1,200                                  │ │
│  │                                                                         │ │
│  │  ═══ SITE 1 COST SUMMARY ════════════════════════════════════════    │ │
│  │  ┌─────────────────────────┬──────────────┬──────────────┐            │ │
│  │  │ Cost Component          │ Annual (₹)   │ Monthly (₹)  │            │ │
│  │  │ ────────────────────────┼──────────────┼──────────────│            │ │
│  │  │ A. Service Visit Cost   │ ₹15,600      │ ₹1,300       │            │ │
│  │  │ B. Manpower Cost        │ ₹20,800      │ ₹1,733       │            │ │
│  │  │ C. Chemical Cost        │ ₹55,392      │ ₹4,616       │            │ │
│  │  │ D. Weekend/Night Surch. │ ₹0           │ ₹0           │            │ │
│  │  │ E. Documentation Cost   │ ₹1,200       │ ₹100         │            │ │
│  │  │ ───────────────────────────────────────────────────────│            │ │
│  │  │ TOTAL COST              │ ₹92,992      │ ₹7,749       │            │ │
│  │  └─────────────────────────┴──────────────┴──────────────┘            │ │
│  │                                                                         │ │
│  │  Proposed Sale Price/Year  : ₹1,56,000                                 │ │
│  │  Proposed Sale Price/Month : ₹13,000                                   │ │
│  │  ★ SITE 1 GROSS MARGIN %  : 40.4%                                     │ │
│  └────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SITE 2: Pune WH  [▸ Click to Expand]                                       │
│  SITE 3: Delhi Office  [▸ Click to Expand]                                   │
│                                                                              │
│  ─── SECTION 4: OVERALL GM SUMMARY ──────────────────────────────────────  │
│  ┌──────────────────┬──────────────┬──────────────┬──────┐                  │
│  │ Site             │ Total Cost/Yr│ Sale Price/Yr│  GM% │                  │
│  │──────────────────┼──────────────┼──────────────┼──────│                  │
│  │ Site 1: Mumbai HQ│ ₹92,992      │ ₹1,56,000    │ 40.4%│                  │
│  │ Site 2: Pune WH  │ ₹28,000      │ ₹48,000      │ 41.7%│                  │
│  │ ──────────────────────────────────────────────────────  │                  │
│  │ TOTAL ANNUAL COST  │ ₹1,20,992                          │                  │
│  │ TOTAL ANNUAL PRICE │ ₹2,04,000                          │                  │
│  │ ──────────────────────────────────────────────────────  │                  │
│  │ ★ OVERALL GROSS MARGIN : 40.7%                          │                  │
│  └────────────────────────────────────────────────────────┘                  │
│                                                                              │
│  GM% without Documentation : 41.3%                                           │
│  GM% with Documentation    : 40.7%                                           │
│                                                                              │
│  APPROVAL STATUS  : ✅ Auto-Approved (GM 40.7% ≥ 35%)                       │
│  APPROVED ON      : 18 Mar 2026 (Immediate)                                 │
│  APPROVER         : System (Auto)                                            │
│                                                                              │
│  Sales Authority Matrix:                                                      │
│  ┌──────────────────────┬──────────────────┬─────────────────┬──────────┐    │
│  │ Authorizer           │ Contracts        │ Jobs/Products   │ Signature│    │
│  │──────────────────────┼──────────────────┼─────────────────┼──────────│    │
│  │ Auto (System)        │ GM ≥ 40%         │ GM ≥ 40%        │ [Auto]  │    │
│  │ Sales Manager        │ GM 20% – 39.99%  │ GM 20% – 39.99% │ [_____]│    │
│  │ CEO / Ops Head       │ GM < 20%         │ GM < 20%        │ [_____] │    │
│  └──────────────────────┴──────────────────┴─────────────────┴──────────┘    │
│                                                                              │
│  ─── AUDIT TRAIL ──────────────────────────────────────────────────────────  │
│  ┌──────────────────┬─────────────────────┬──────────────┬──────────────┐   │
│  │ Date & Time      │ Action              │ User         │ Remarks      │   │
│  │──────────────────┼─────────────────────┼──────────────┼──────────────│   │
│  │ 18 Mar 09:30 AM  │ Draft Created       │ Ravi Sharma  │ —            │   │
│  │ 18 Mar 10:00 AM  │ Submitted           │ Ravi Sharma  │ —            │   │
│  │ 18 Mar 10:00 AM  │ Auto-Approved       │ System       │ GM 40.7%≥35% │   │
│  └──────────────────┴─────────────────────┴──────────────┴──────────────┘   │
│                                                                              │
│              [BACK]   [EDIT] (if Draft)   [DOWNLOAD PDF]                   │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Fields Displayed

| Section                  | Field                                                                                      | Display Type     |
| ------------------------ | ------------------------------------------------------------------------------------------ | ---------------- |
| **Header**               | GMA ID, Status, Created By, Created On, Submitted On, Approved On                         | Static Display   |
| **Section 1 (Source)**   | Source Type, Customer/Lead ID, Name, Phone, Email, Type, Address                           | Read-only        |
| **Section 2 (Config)**   | Pest Type, Service Mode, Contract Duration, Start Date, Frequency, Branch, Prepared By/Date | Read-only       |
| **Section 3A (per site)**| Rate per Visit, Annual Frequency, Cost/Year (A), Cost/Month                                | Read-only        |
| **Section 3B (per site)**| Hours/Visit, Annual Frequency, Rate/Hour, Cost/Year (B), Cost/Month                       | Read-only        |
| **Section 3C (per site)**| Chemical Table: Product Name, Code, UOM, Dilution, SQFT, Req.Qty, Price/UOM, Cost/Visit   | Read-only        |
| **Section 3D (per site)**| Applicable flag, Surcharge Cost (D)                                                         | Read-only        |
| **Section 3E (per site)**| Applicable flag, Cost/Doc, Docs/Month, Doc Cost/Year (E)                                   | Read-only        |
| **Site Summary**         | Cost breakdown table (A+B+C+D+E), Total Cost, Proposed Price, Site GM%                     | Read-only        |
| **Section 4 (Overall)**  | Site-wise summary, Total Annual Cost/Price, Overall GM%, GM with/without Documentation     | Auto / Read-only |
| **Approval**             | Status, Approval Type, Approver, Approved/Rejected Date, Remarks, Authority Matrix         | Read-only        |
| **Audit Trail**          | Timestamp, Action, User, Remarks                                                            | Read-only        |

---

## Actions

| Button           | Condition              | Description                                              |
| **Back**         | Always                 | Returns to My Requests tab                               |
| **Download PDF** | Status ≠ Draft         | Exports the GMA sheet as a formatted PDF document        |

---

================================================================================

# 17.3 Tab 3: Received Requests

**Description:**
Displays GMA sheets received by the logged-in manager or approver (Sales Manager or CEO/Ops Head) for review. Users can view full sheet details and approve or reject based on the margin analysis.

> **Visibility Rule:** Appears only for users with approval authority (Sales Managers, CEO / Ops Head). Users will only see the requests explicitly routed to them.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      RECEIVED REQUESTS                                       │
│                                                                              │
│  [Tab 1: GMA Entries]  [Tab 2: My Requests]  [Tab 3: Received Requests ●]   │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Search: [________________________]     Status: [▼ Pending / All ▼]    │  │
│  │ Date Range: [📅 From] – [📅 To]                                        │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  RECEIVED REQUESTS TABLE                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │GMA ID    │Customer   │Submitted By│GM%│Submitted On  │Deadline │Actions  ││
│  │──────────┼───────────┼────────────┼───┼──────────────┼─────────┼─────────││
│  │GMA-00088 │ABC Pharma │Ravi Sharma │23%│ 17 Mar 2026  │18 Mar   │[V][Appr]││
│  │GMA-00085 │DEF Mall   │Anjali M.   │6% │ 15 Mar 2026  │17 Mar   │[V][Appr]││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Status    │                                                                ││
│  │──────────│                                                                ││
│  │🟡 Pending│                                                                ││
│  │🔴 Pending│                                                                ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   Next                              │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table Fields

| Field          | Description                                                    |
| -------------- | -------------------------------------------------------------- |
| GMA ID         | System-generated reference                                     |
| Customer       | Customer name linked to the GMA sheet                          |
| Submitted By   | Sales person who created this GMA sheet                        |
| GM %           | Overall gross margin (auto-calculated)                         |
| Submitted On   | Date the sheet was submitted for approval                      |
| Deadline       | Approval deadline (24 hrs for Manager, 48 hrs for CEO)        |
| Status         | Pending / Approved / Rejected                                  |
| Actions        | View / Approve                                                 |

---

## Filters

| Filter     | Type       | Options                       |
| ---------- | ---------- | ----------------------------- |
| Status     | Dropdown   | Pending / Approved / Rejected / All |
| Date Range | Date Range | From – To (submission date)   |

---

## Actions

| Action      | Condition         | Description                                      |
| ----------- | ----------------- | ------------------------------------------------ |
| **View**    | All statuses      | Opens the GMA sheet in read-only review mode     |
| **Approve** | Status = Pending  | Opens the approval / rejection form (Screen 17.3.2) |

---

================================================================================

# 17.3.1 View Received GMA Sheet

**Description:**
Full read-only view of a received GMA sheet for the approver to review all details — source info, service config, site-wise cost breakdown (Service Visit, Manpower, Chemical/Product, Surcharges, Documentation), and margin summary — before taking an approval or rejection action.

The layout is **identical to Screen 17.2.2 (View GMA Sheet)** with an additional **Approval Decision** section appended at the bottom.

---

## Additional Approval Section (at the bottom of View screen)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  APPROVAL DECISION                                                            │
│                                                                              │
│  GM %         : 23%  →  Sales Manager Approval Required                     │
│  Deadline     : By 18 Mar 2026 10:00 AM (24-hour window)                    │
│                                                                              │
│  Decision     : ○ Approve   ○ Reject                                         │
│  Remarks*     : [_______________________________________________]            │
│                (Required if Rejected; Optional if Approved)                  │
│                                                [SUBMIT DECISION]            │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

================================================================================

# 17.3.2 Approve / Reject GMA Sheet

**Description:**
A focused action screen where the approver confirms their decision to approve or reject the GMA sheet. If rejected, a remark is mandatory. If approved, the GMA status is updated, the sales person is notified, and contract creation is unlocked.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    APPROVE / REJECT GMA SHEET – GMA-00088                   │
│                                                                              │
│  Customer      : ABC Pharma                                                  │
│  Service Type  : Rodent AMC                                                  │
│  Branch        : Bangalore                                                   │
│  Submitted By  : Ravi Sharma (17 Mar 2026)                                   │
│  Overall GM%   : 23%  🟡 Pending Sales Manager                               │
│  Deadline      : 18 Mar 2026 10:00 AM                                        │
│                                                                              │
│  ─── QUICK SUMMARY ─────────────────────────────────────────────────────── │
│  Total Annual Cost    : ₹1,02,000                                            │
│  Total Annual Price   : ₹1,32,000                                            │
│  Overall GM %         : 23%                                                  │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │  YOUR DECISION                                                         │  │
│  │                                                                        │  │
│  │  ● Approve     ○ Reject                                                │  │
│  │                                                                        │  │
│  │  Remarks / Notes*                                                      │  │
│  │  ┌─────────────────────────────────────────────────────────────────┐  │  │
│  │  │                                                                  │  │  │
│  │  └─────────────────────────────────────────────────────────────────┘  │  │
│  │  (Required if Rejected; Optional if Approved — max 500 characters)    │  │
│  │                                                                        │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│                    [CONFIRM DECISION]   [CANCEL]                             │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Fields

| Field     | Type      | Required           | Validation                            |
| --------- | --------- | ------------------ | ------------------------------------- |
| Decision  | Radio     | Yes                | Approve / Reject                      |
| Remarks   | Text Area | Required if Reject | Free-text; max 500 characters         |

---

## System Actions on Submission

| Decision    | System Behavior                                                                         |
| ----------- | --------------------------------------------------------------------------------------- |
| **Approve** | GMA status → Approved; sales person notified in-app & email; contract creation unlocked |
| **Reject**  | GMA status → Rejected; sales person notified with rejection remarks; sheet returned to submitter |

---

## Actions

| Button               | Description                                           |
| -------------------- | ----------------------------------------------------- |
| **Confirm Decision** | Saves the decision and triggers notification workflow  |
| **Cancel**           | Returns to Received Requests tab without any action   |

---

================================================================================

## System Behavior & Business Rules

## 1. Gross Margin Calculation Formula

```
Gross Margin % = ( Sale Price – Total Cost ) / Sale Price × 100

Where:
  Total Cost  = Σ (A + B + C + D + E) per site
  A = Service Visit Cost / Year  (Rate per Visit × Annual Frequency)
  B = Manpower Cost / Year       (Hours × Annual Frequency × Rate/Hour)
  C = Chemical Cost / Year       (Σ Chemical Cost/Month × 12)
  D = Weekend/Night Surcharge    (IF Applicable → (A + B) × 25%)
  E = Documentation Cost / Year  (IF Applicable → Cost/Doc × Docs/Month × 12)
  Sale Price  = Σ (Proposed Sale Prices entered by sales person) per site
```

## 2. Chemical Cost Calculation (per Site per Visit)

```
Chemical Cost / Visit = Required Qty × Price per UOM  (per chemical row)
Chemical Cost / Month = Cost per Visit × Visits per Month
Chemical Cost / Year (C) = Chemical Cost / Month × 12
```
- Chemicals are auto-fetched from **Module 12** (Service Config → Section 4: Chemicals/Products Used)
- Product Name, Code, UOM, Dilution, Price/UOM are auto-filled from **Module 10** (Product Master)
- User enters **Coverage (SQFT)** and **Required Qty**
- Applied per chemical row; summed for the site's total chemical cost.

## 3. Service Visit Cost Calculation

```
Service Visit Cost / Year (A) = Rate per Visit × Annual Frequency
Service Visit Cost / Month = A ÷ 12
```

## 4. Manpower / Labor Cost Calculation

```
Manpower Cost / Year (B) = Hours per Visit × Annual Frequency × Rate per Hour
Manpower Cost / Month = B ÷ 12
```

## 5. Site Total Cost Calculation

```
Site Total Cost = A (Service Visit) + B (Manpower) + C (Chemical) + D (Surcharge) + E (Documentation)
```

## 6. Sales Authority & Approval Matrix

| Authorizer         | Margin Rule      | Approval Routing                         |
| ------------------ | ---------------- | ---------------------------------------- |
| Auto (System)      | GM ≥ 40%         | **Automatic**: No manual routing needed |
| Sales Manager      | GM 20% – 39.99%  | **Manual Selection**: User must select  |
| CEO / Ops Head     | GM < 20%         | **Manual Selection**: User must select  |

> If GM% < 40%, the user is required to select the appropriate approver from a popup list and send the request manually.

## 7. Status Lifecycle

```
Submitted → Pending Approval → Approved / Rejected
               (Auto-Approved if GM ≥ 40%)
```

## 8. Revoke Rules

- Sales person can **Revoke** only when status = **Pending** (not yet actioned by approver).
- Revoked sheets are cancelled and must be recreated if needed.
- Revoke is **not available** for Auto-Approved, Approved, or Rejected sheets.

## 10. Notifications

| Event              | Recipient        | Channel                  |
| ------------------ | ---------------- | ------------------------ |
| GMA Submitted      | Approver         | In-app + Email           |
| Auto-Approved      | Sales Person     | In-app notification      |
| Approved           | Sales Person     | In-app + Email           |
| Rejected           | Sales Person     | In-app + Email + Remarks |
| Deadline Reminder  | Approver         | Auto-reminder at 50% of timeline |

## 11. PDF Export

- Approved GMA sheets can be exported as a PDF summarizing source info, service config, site-wise costs (A through E), chemicals used, and the overall margin analysis.
- PDF is suitable for internal review or management records.

## 12. Module Dependencies

| Dependency Module                            | What It Provides to GMA                                                   |
| -------------------------------------------- | ------------------------------------------------------------------------- |
| **Module 4** – Customer Management           | Customer data for "From Customer" source selection                        |
| **Module 7** – Branch Management             | Branch selection dropdown                                                  |
| **Module 10** – Product Master (Inventory)   | Chemical/product details: Name, Code, UOM, Purchase Price                 |
| **Module 12** – Service Management           | Service-to-chemical mapping; pest types; treatment methods; dilution data |
| **Module 15** – Lead Management              | Lead data for "From Lead" source selection                                |
| **Module 18** – Contract Management          | GMA Approval is a **prerequisite** for Contract Creation                  |

## 11. System Behaviors on Submission

| Trigger                     | System Action                                                                   |
| --------------------------- | ------------------------------------------------------------------------------- |
| GM ≥ 40%                    | Status → Auto-Approved; no approval routing needed                              |
| GM < 40% + Approver Picked  | Status → Pending; Notification sent to selected Approver                        |
| Service Type selected       | Auto-fetch chemicals from Module 12 → Module 10; populate chemical rows         |
| Chemical SQFT/Qty changed   | Cost/Visit, Cost/Month, Cost/Year recalculated dynamically                      |
| Price/UOM overridden        | System retains overridden value; does not reset on re-selection                 |
| Frequency changed           | Annual Frequency updated; cascades to all site cost calculations (A, B)         |
| Source = From Lead          | Auto-populate lead details; link GMA to lead record                             |
| Source = From Customer      | Auto-populate customer details; link GMA to customer record                     |
| Weekend/Night = Yes         | Automatically add 25% surcharge to Service Visit + Manpower costs               |

---

================================================================================

## RBAC – Role-Based Access Control

| Role              | Tab 1 (GMA Entries) | Tab 2 (My Requests)    | Tab 3 (Received Requests) | Add GMA | Approve/Reject   |
| ----------------- | ------------------- | ---------------------- | ------------------------- | ------- | ---------------- |
| Sales Person      | View (own)          | View / Add / Revoke    | —                         | ✅       | ❌               |
| Sales Manager     | View (team)         | View (own)             | View / Approve / Reject   | ✅       | ✅ (Route explicitly)  |
| CEO / Ops Head    | View (all)          | View (own)             | View / Approve / Reject   | ✅       | ✅ (Route explicitly)  |
| Admin             | View (all)          | View (all)             | View (all)                | ✅       | ❌               |

---

================================================================================

## Navigation Flow Summary

```
MODULE 17: GMA MANAGEMENT
│
├── Tab 1: GMA Entries
│   └── [View] → 17.1.1 View GMA Sheet (Read-only)
│
├── Tab 2: My Requests
│   ├── [+ Add GMA Sheet] → 17.2.1 Add GMA Sheet Form
│   │     ├── GM ≥ 40% → [Save & Submit (Auto)] → ✅ Auto-Approved → Contract unlocking
│   │     └── GM < 40% → [Save & Request] → [Manual Select] → � Sent to selected Approver
│   ├── [View] → 17.2.2 View My GMA Sheet (Read-only + PDF Download)
│   └── [Revoke] → Cancels the pending sheet (if not yet approved)
│
└── Tab 3: Received Requests (Approvers only)
    ├── [View] → 17.3.1 View Received GMA Sheet (Full detail + Approval section)
    └── [Approve] → 17.3.2 Approve / Reject GMA Sheet Form
          ├── [Approve] → GMA Approved + Sales person notified + Contract unlocked
          └── [Reject]  → GMA Rejected + Sales person notified with remarks
```

---

================================================================================
================================================================================

# Complete Data Flow — Module 17 GMA Management

## ASCII: Module-to-Module Data Flow

```
┌──────────────────────────────────────────────────────────────────────────────────────┐
│                     MODULE 17: GMA MANAGEMENT — COMPLETE DATA FLOW                    │
│                                                                                       │
│  ╔═══════════════════╗                                                                │
│  ║  ENTRY POINTS     ║                                                                │
│  ╚═══════════════════╝                                                                │
│                                                                                       │
│  ┌──────────────────┐   ┌──────────────────┐   ┌──────────────────┐                  │
│  │  FROM LEAD       │   │  FROM CUSTOMER   │   │  ADD NEW         │                  │
│  │  (Module 15)     │   │  (Module 4)      │   │  (Manual Entry)  │                  │
│  │                  │   │                  │   │                  │                  │
│  │  Lead ID, Name,  │   │  Cust ID, Name,  │   │  Full Name,      │                  │
│  │  Phone, Email,   │   │  Phone, Email,   │   │  Phone, Email,   │                  │
│  │  Lead Type,      │   │  Customer Type,  │   │  Address, City,  │                  │
│  │  Lead Status     │   │  Address         │   │  State, Pincode  │                  │
│  └────────┬─────────┘   └────────┬─────────┘   └────────┬─────────┘                  │
│           │                      │                      │                             │
│           └──────────────────────┼──────────────────────┘                             │
│                                  ▼                                                    │
│  ╔═══════════════════════════════════════════════════════════════════════════╗         │
│  ║                    17.2.1 ADD GMA SHEET FORM                             ║         │
│  ╚═══════════════════════════════════════════════════════════════════════════╝         │
│                                                                                       │
│  ┌────────────────────────────────────────────────────────────────────────────────┐   │
│  │  SECTION 1: SOURCE SELECTION                                                    │   │
│  │  ┌─────────────────┐                                                            │   │
│  │  │ Source radio     │──────► Auto-fill fields from Module 15 / Module 4          │   │
│  │  └─────────────────┘                                                            │   │
│  │                                                                                  │   │
│  │  SECTION 2: SERVICE & CONTRACT CONFIGURATION                                     │   │
│  │  ┌─────────────────┐     ┌──────────────────────────────────────┐               │   │
│  │  │ Pest Type*      │────►│ MODULE 12: SERVICE MANAGEMENT       │               │   │
│  │  │ dropdown        │     │ Fetches: Service config,             │               │   │
│  │  └─────────────────┘     │ treatment methods, chemicals linked, │               │   │
│  │                          │ dilution rates, coverage standards    │               │   │
│  │  ┌─────────────────┐     └──────────────────────────────────────┘               │   │
│  │  │ Branch*         │────► MODULE 7: BRANCH MANAGEMENT                            │   │
│  │  │ dropdown        │     (Active branches list)                                  │   │
│  │  └─────────────────┘                                                            │   │
│  │                                                                                  │   │
│  │  Frequency → Annual Frequency auto-calc:                                         │   │
│  │  Weekly=52, Fortnightly=26, Monthly=12, Quarterly=4, Custom=editable            │   │
│  │                                                                                  │   │
│  │  SECTION 3: COST BREAKDOWN (Per Site)                                            │   │
│  │  ┌───────────────────────────────────────────────────────────────────────────┐   │   │
│  │  │  SITE n                                                                    │   │   │
│  │  │  ┌────────────────────────────────────────────────────────────────────┐   │   │   │
│  │  │  │ 3A: SERVICE VISIT COST                                             │   │   │   │
│  │  │  │ Rate/Visit × Annual Freq = Cost/Year (A)                           │   │   │   │
│  │  │  ├────────────────────────────────────────────────────────────────────┤   │   │   │
│  │  │  │ 3B: MANPOWER / LABOR COST                                          │   │   │   │
│  │  │  │ Hours × Annual Freq × Rate/Hour = Cost/Year (B)                    │   │   │   │
│  │  │  ├────────────────────────────────────────────────────────────────────┤   │   │   │
│  │  │  │ 3C: CHEMICAL / PRODUCT COST                                        │   │   │   │
│  │  │  │                                                                     │   │   │   │
│  │  │  │  ┌─────────────────────────┐   ┌──────────────────────────────┐   │   │   │   │
│  │  │  │  │ MODULE 12               │   │ MODULE 10                    │   │   │   │   │
│  │  │  │  │ Service Management      │──►│ Product Master               │   │   │   │   │
│  │  │  │  │                         │   │                              │   │   │   │   │
│  │  │  │  │ • Chemicals linked to   │   │ • Product Name, Code         │   │   │   │   │
│  │  │  │  │   selected service      │   │ • Base UOM (ml/Ltr/gm/kg/   │   │   │   │   │
│  │  │  │  │ • Dilution config       │   │   Nos/tube)                  │   │   │   │   │
│  │  │  │  │ • Coverage standards    │   │ • Purchase Price (₹/UOM)     │   │   │   │   │
│  │  │  │  └─────────────────────────┘   │ • HSN Code / Tax Config      │   │   │   │   │
│  │  │  │                                 └──────────────────────────────┘   │   │   │   │
│  │  │  │                                                                     │   │   │   │
│  │  │  │  USER ENTERS: Coverage SQFT + Required Qty                          │   │   │   │
│  │  │  │  SYSTEM CALCULATES:                                                 │   │   │   │
│  │  │  │    Cost/Visit = Req Qty × Price/UOM                                 │   │   │   │
│  │  │  │    Cost/Month = Cost/Visit × Visits/Month                           │   │   │   │
│  │  │  │    Cost/Year (C) = Cost/Month × 12                                  │   │   │   │
│  │  │  ├────────────────────────────────────────────────────────────────────┤   │   │   │
│  │  │  │ 3D: WEEKENDS/NIGHTS SURCHARGE                                      │   │   │   │
│  │  │  │ IF Yes → (A + B) × 25% = Cost (D)                                 │   │   │   │
│  │  │  ├────────────────────────────────────────────────────────────────────┤   │   │   │
│  │  │  │ 3E: DOCUMENTATION COST                                             │   │   │   │
│  │  │  │ IF Yes → Cost/Doc × Docs/Month × 12 = Cost (E)                    │   │   │   │
│  │  │  ├────────────────────────────────────────────────────────────────────┤   │   │   │
│  │  │  │ SITE TOTAL = A + B + C + D + E                                     │   │   │   │
│  │  │  │ SITE GM% = (Sale Price – Total Cost) / Sale Price × 100            │   │   │   │
│  │  │  └────────────────────────────────────────────────────────────────────┘   │   │   │
│  │  └───────────────────────────────────────────────────────────────────────────┘   │   │
│  │                                                                                  │   │
│  │  SECTION 4: OVERALL GM SUMMARY                                                   │   │
│  │  Overall GM% = (Σ Sale Prices – Σ Total Costs) / Σ Sale Prices × 100            │   │
│  └────────────────────────────────────────────────────────────────────────────────┘   │
│                                                                                       │
│  ╔══════════════════════════════════════════════════════════════╗                     │
│  ║                    APPROVAL ROUTING                          ║                     │
│  ╚══════════════════════════════════════════════════════════════╝                     │
│                                                                                       │
│                         ┌─────────────────────┐                                       │
│                         │  GM% Calculated      │                                       │
│                         └──────────┬──────────┘                                       │
│                                    │                                                   │
              ┌─────────────────────┼─────────────────────┐                             │
│              ▼                                         ▼                             │
│  ┌──────────────────┐                      ┌─────────────────────────┐               │
│  │ GM ≥ 40%         │                      │ GM < 40%                │               │
│  │                  │                      │                         │               │
│  │ ✅ AUTO-APPROVED │                      │ 🟡 REGULAR FLOW         │               │
│  │ (Immediate)      │                      │ User Manually Selects   │               │
│  │                  │                      │ Approver (Popup)        │               │
│  └────────┬─────────┘                      └────────┬────────────────┘               │
│           │                                         │                                │
│           │                                         ▼                                │
│           │                               ┌──────────────────┐                       │
│           │                               │ Tab 3: Received  │                       │
│           │                               │ Requests         │                       │
│           │                               │ (Selected User)  │                       │
│           │                               └────────┬─────────┘                       │
│           │                                        │                                 │
│           │                               ┌────────┴──────────────────────┘          │
│           │                               ▼                                          │
│           │                      ┌──────────────────┐                                │
│           │                      │ APPROVE / REJECT │                                │
│           │                      │ (17.3.2)         │                                │
│           │                      └──┬──────────┬────┘                                │
│           │                         │          │                                     │
│           │                         ▼          ▼                                     │
│           │                      ✅ Approved  ❌ Rejected                            │
│           │                         │          │                                     │
│           │                         │          └──► Returned to Sender               │
│           │                         │               (Rejection Remarks)              │
│           │     │                                                                      │
│           └─────┤                                                                      │
│                 ▼                                                                      │
│  ╔══════════════════════════════════════════════════════════════╗                     │
│  ║            MODULE 18: CONTRACT MANAGEMENT                    ║                     │
│  ║            (GMA Approval unlocks contract creation)          ║                     │
│  ╚══════════════════════════════════════════════════════════════╝                     │
│                                                                                       │
└──────────────────────────────────────────────────────────────────────────────────────┘
```

---

## Module Dependency Map (Summary)

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                                                                                  │
│   MODULE 4                MODULE 7              MODULE 10             MODULE 12  │
│   Customer Mgmt           Branch Mgmt           Product Master       Service Mgmt│
│   ┌──────────┐           ┌──────────┐          ┌──────────┐        ┌──────────┐ │
│   │Customer  │           │ Branch   │          │ Product  │        │ Service  │ │
│   │ID, Name, │           │ list for │          │ Name,    │◄───────│ Chemical │ │
│   │Phone,    │           │ dropdown │          │ Code,    │  Maps  │ mappings,│ │
│   │Email,    │           │ selection│          │ UOM,     │  to    │ Dilution,│ │
│   │Type,     │           │          │          │ Purchase │        │ Coverage │ │
│   │Address   │           │          │          │ Price,   │        │ config   │ │
│   └─────┬────┘           └─────┬────┘          │ HSN Code │        └─────┬────┘ │
│         │                      │               └─────┬────┘              │      │
│         │                      │                     │                   │      │
│         │                      │                     │                   │      │
│         └──────────────┐  ┌────┘          ┌──────────┘    ┌──────────────┘      │
│                        │  │               │               │                     │
│                        ▼  ▼               ▼               ▼                     │
│                 ╔═══════════════════════════════════════════════╗                │
│                 ║       MODULE 17: GMA MANAGEMENT               ║                │
│                 ║                                                ║                │
│                 ║  • Source Selection (M4, M15)                  ║                │
│                 ║  • Branch (M7)                                 ║                │
│                 ║  • Chemicals auto-fetch (M12 → M10)           ║                │
│                 ║  • Cost Calculation (A+B+C+D+E)               ║                │
│                 ║  • GM% Calculation & Approval Routing          ║                │
│                 ╚═══════════════════════════════╤═══════════════╝                │
│                                                 │                                │
│   MODULE 15                                     │                                │
│   Lead Mgmt                                     ▼                                │
│   ┌──────────┐                  ╔═══════════════════════════════╗                │
│   │ Lead ID, │                  ║  MODULE 18: CONTRACT MGMT     ║                │
│   │ Name,    │                  ║  (Requires GMA Approval)      ║                │
│   │ Phone,   │──────────────►   ╚═══════════════════════════════╝                │
│   │ Email,   │     Source                                                        │
│   │ Type,    │     Selection                                                     │
│   │ Status   │     (Add GMA)                                                     │
│   └──────────┘                                                                   │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## Data Flow Table

| Source Module | Data Provided                                                      | Used In (GMA)                                    |
| ------------- | ------------------------------------------------------------------ | ------------------------------------------------ |
| **Module 4**  | Customer ID, Name, Phone, Email, Type, Address                     | Section 1: Source Selection (From Customer)      |
| **Module 7**  | Branch list (Active branches)                                      | Section 2: Branch dropdown                       |
| **Module 10** | Product Name, Code, Base UOM, Purchase Price, HSN Code             | Section 3C: Chemical/Product auto-fill via M12   |
| **Module 12** | Service → Chemical mappings, Dilution, Coverage, Treatment methods | Section 3C: Auto-fetch chemicals on service pick |
| **Module 15** | Lead ID, Name, Phone, Email, Type, Status                          | Section 1: Source Selection (From Lead)          |
| **Module 17** | Approved GMA Sheet (GM%, costs, service config)                    | **Module 18**: Contract Creation prerequisite    |
