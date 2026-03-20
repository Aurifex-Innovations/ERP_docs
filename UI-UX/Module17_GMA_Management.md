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
A multi-part form that allows a sales person to build a complete Gross Margin sheet for a customer and service proposal. The form is structured in three parts:
- **Part 1:** Service & Customer Information
- **Part 2:** Site-Wise Cost Breakdown (chemicals, labor, revisit, other costs)
- **Part 3:** Overall Summary with automatic GM% calculation and approval status

Chemical products are pulled from the Products Module (consumable type). Quantities and rates can be edited manually. The system auto-calculates all sub-totals, total cost, total price, and the final Gross Margin percentage. Upon save, the Approval Matrix is applied automatically.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          ADD GMA SHEET                                       │
│                                                                              │
│  GMA Reference ID : [Auto-generated on Save]                                 │
│  Created By       : [Logged-in User]        Date: [Today's Date]             │
│                                                                              │
│  ══════════════════════════════════════════════════════════════════════════  │
│  PART 1: SERVICE & CUSTOMER INFORMATION                                       │
│  ══════════════════════════════════════════════════════════════════════════  │
│                                                                              │
│  Customer*           : [🔍 Search & Select Customer ▼]                       │
│  Contact Person      : [Auto-filled from Customer Master] (Read-only)        │
│  Phone               : [Auto-filled] (Read-only)                             │
│  Email               : [Auto-filled] (Read-only)                             │
│                                                                              │
│  Service Type*       : [▼ Select Service (AMC / One-Time / Quarterly / etc.)]│
│  Contract Duration   : [________] Months   Start Date: [📅 Date Picker]     │
│  Service Frequency   : [▼ Monthly / Weekly / Quarterly / Custom]             │
│  No. of Visits/Month : [________] (auto-filled from template, editable)      │
│                                                                              │
│  Branch*             : [▼ Select Branch ▼]                                   │
│  Prepared By         : [Auto-filled: Logged-in User]                         │
│  Prepared Date       : [Auto-filled: Today]                                  │
│  Remarks / Notes     : [___________________________________________________] │
│                                                                              │
│  ══════════════════════════════════════════════════════════════════════════  │
│  PART 2: SITE-WISE COST BREAKDOWN                                             │
│  [+ ADD SITE]                                                                 │
│  ══════════════════════════════════════════════════════════════════════════  │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │  SITE 1                                                   [Remove Site]│  │
│  │                                                                        │  │
│  │  Site Name / Location*  : [_________________________________]          │  │
│  │  Address / City         : [_________________________________]          │  │
│  │  Area (sq. ft.)*        : [________]                                   │  │
│  │  Visits/Month*          : [________]  (inherited from Part 1)          │  │
│  │                                                                        │  │
│  │  ─── 2A: CONSUMABLE CHEMICALS (from Products Module) ──────────────── │  │
│  │  [+ ADD CHEMICAL]                                                      │  │
│  │                                                                        │  │
│  │  ┌──────────────┬──────────────┬──────┬──────────┬────────┬─────────┐  │  │
│  │  │Chemical/Prod.│ UOM (Auto)   │ Qty  │Rate/UOM  │ Total  │[Remove] │  │  │
│  │  │[🔍 Select ▼] │[Auto-filled] │[Edit]│[Auto/Ed.]│[Auto]  │         │  │  │
│  │  │[🔍 Select ▼] │[Auto-filled] │[Edit]│[Auto/Ed.]│[Auto]  │         │  │  │
│  │  └──────────────┴──────────────┴──────┴──────────┴────────┴─────────┘  │  │
│  │  Chemical Cost Sub-total (A) : ₹ [Auto-calculated]                     │  │
│  │                                                                        │  │
│  │  ─── 2B: TECHNICIAN / LABOR COST ──────────────────────────────────── │  │
│  │  No. of Technicians*    : [________]                                   │  │
│  │  Rate per Visit (₹)*    : [________]                                   │  │
│  │  Visits/Month           : [Auto from above]                            │  │
│  │  Labor Cost/Month (B)   : ₹ [Auto: Technicians × Rate × Visits]        │  │
│  │                                                                        │  │
│  │  ─── 2C: REVISIT / CALLBACK CHARGES ───────────────────────────────── │  │
│  │  Expected Revisits/Month : [________]                                  │  │
│  │  Cost per Revisit (₹)   : [________]                                   │  │
│  │  Revisit Cost/Month (C)  : ₹ [Auto-calculated]                         │  │
│  │                                                                        │  │
│  │  ─── 2D: OTHER / MISCELLANEOUS COSTS (Optional) ───────────────────── │  │
│  │  [+ Add Row]  Description: [______________]  Amount (₹): [________]   │  │
│  │  Other Costs Sub-total (D): ₹ [Auto-calculated]                        │  │
│  │                                                                        │  │
│  │  ─── SITE 1 SUMMARY ────────────────────────────────────────────────  │  │
│  │  SITE 1 TOTAL COST/MONTH  : ₹ [A + B + C + D]  (Auto-calculated)      │  │
│  │  SITE 1 PROPOSED PRICE    : ₹ [Manual Entry*]                          │  │
│  │  SITE 1 GROSS MARGIN (%)  : [Auto: (Price–Cost)/Price × 100] %         │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [+ ADD SITE]   (Repeat the above block for each site)                       │
│                                                                              │
│  ══════════════════════════════════════════════════════════════════════════  │
│  PART 3: OVERALL SUMMARY                                                      │
│  ══════════════════════════════════════════════════════════════════════════  │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │  Site                  │ Total Cost/Month │ Sale Price/Month │ GM %   │  │
│  │  ──────────────────────┼──────────────────┼─────────────────┼────────│  │
│  │  Site 1: Mumbai HQ     │  ₹4,800          │  ₹8,000         │ 40%    │  │
│  │  Site 2: Pune Warehouse│  ₹2,800          │  ₹5,000         │ 44%    │  │
│  │  Site 3: Delhi Office  │  ₹3,400          │  ₹6,000         │ 43%    │  │
│  │  ─────────────────────────────────────────────────────────────────── │  │
│  │  TOTAL MONTHLY COST    │  ₹11,000                                     │  │
│  │  TOTAL MONTHLY PRICE   │  ₹19,000                                     │  │
│  │  OVERALL GROSS MARGIN  │  42%  ✅ AUTO-APPROVED (≥ 35%)               │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  Approval Status (Auto-determined based on GM%):                             │
│  ┌──────────────────────────────────────────────────────────────────┐       │
│  │  GM ≥ 35%          → ✅ Auto-Approved       (Immediate)           │       │
│  │  GM 10% – 34.99%   → 🟡 Requires Sales Manager Approval (24 hrs) │       │
│  │  GM < 10%          → 🔴 Requires CEO/Ops Head Approval (48 hrs)   │       │
│  └──────────────────────────────────────────────────────────────────┘       │
│                                                                              │
│                    [SAVE & SUBMIT]   [SAVE AS DRAFT]   [CANCEL]             │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Part 1: Service & Customer Information Fields

| Field               | Type            | Required | Validation / Notes                                      |
| ------------------- | --------------- | -------- | ------------------------------------------------------- |
| Customer            | Search Dropdown | Yes      | Must be an active customer from Customer Master         |
| Contact Person      | Auto-filled     | System   | Read-only; from Customer Master                         |
| Phone               | Auto-filled     | System   | Read-only; from Customer Master                         |
| Email               | Auto-filled     | System   | Read-only; from Customer Master                         |
| Service Type        | Dropdown        | Yes      | AMC / One-Time / Quarterly / Fogging / Fumigation / etc.|
| Contract Duration   | Number          | No       | In months; applicable for AMC contracts                 |
| Start Date          | Date Picker     | No       | Cannot be in the past                                   |
| Service Frequency   | Dropdown        | Yes      | Monthly / Weekly / Quarterly / Custom                   |
| No. of Visits/Month | Number          | Yes      | Auto-filled from service template; user can edit        |
| Branch              | Dropdown        | Yes      | Branch handling this service                            |
| Prepared By         | Auto-filled     | System   | Logged-in user; read-only                               |
| Prepared Date       | Auto-filled     | System   | Today's date; read-only                                 |
| Remarks / Notes     | Text Area       | No       | Free-text for any special instructions                  |

---

## Part 2: Site-Wise Breakdown

### 2A — Site Information

| Field              | Type    | Required | Validation                           |
| ------------------ | ------- | -------- | ------------------------------------- |
| Site Name/Location | Text    | Yes      | Free-text name (e.g., "Mumbai HQ")    |
| Address / City     | Text    | No       | Location description                  |
| Area (sq. ft.)     | Number  | Yes      | Must be > 0                           |
| Visits/Month       | Number  | Yes      | Inherited from Part 1; site-editable  |

---

### 2A — Consumable Chemicals (from Products Module)

> Each site supports adding one or more chemical rows. Chemical products are sourced from **Module 10 – Product Master** (Consumable type only). Quantity and Rate can be edited manually.

| Field              | Type            | Required | Validation / Notes                                       |
| ------------------ | --------------- | -------- | -------------------------------------------------------- |
| Chemical / Product | Search Dropdown | Yes      | Pulls from Product Master (Consumable chemicals only)    |
| UOM                | Auto-filled     | System   | Base UOM from Product Master (Litre / ML / Kg / Nos etc.)|
| Quantity Needed    | Number          | Yes      | User enters quantity per service visit (editable)        |
| Rate per UOM (₹)   | Currency        | Yes      | Auto-filled from Product Master; user can override       |
| Total Cost (₹)     | Auto-calculated | System   | `Quantity × Rate × Visits/Month`                         |

**Rules:**
- At least one chemical row is required per site.
- Multiple chemicals can be added using **[+ Add Chemical]**.
- Chemical Sub-total (A) = Sum of all chemical row totals for that site.

---

### 2B — Technician / Labor Cost

| Field              | Type     | Required | Validation / Notes                                |
| ------------------ | -------- | -------- | ------------------------------------------------- |
| No. of Technicians | Number   | Yes      | Minimum 1                                         |
| Rate per Visit (₹) | Currency | Yes      | Manually entered cost per technician per visit    |
| Visits/Month       | Display  | Auto     | Inherited from site-level setting                 |
| Labor Cost/Month   | Auto     | System   | `Technicians × Rate/Visit × Visits/Month` = (B)   |

---

### 2C — Revisit / Callback Charges

| Field                   | Type     | Required | Notes                              |
| ----------------------- | -------- | -------- | ---------------------------------- |
| Expected Revisits/Month | Number   | No       | Estimated follow-up / callback visits |
| Cost per Revisit (₹)    | Currency | No       | Flat cost per revisit trip          |
| Revisit Cost/Month      | Auto     | System   | `Revisits × Cost per Revisit` = (C) |

---

### 2D — Other / Miscellaneous Costs (Optional)

| Field       | Type     | Required | Notes                          |
| ----------- | -------- | -------- | ------------------------------ |
| Description | Text     | No       | Label for the additional cost  |
| Amount (₹)  | Currency | No       | Manually entered value         |

> Multiple rows can be added using **[+ Add Row]**. Other Costs Sub-total (D) = Sum of all entries.

---

### Site Cost Summary (Auto-calculated per Site)

| Field                     | Formula                                                       |
| ------------------------- | ------------------------------------------------------------- |
| Site Total Cost/Month     | A (Chemicals) + B (Labor) + C (Revisit) + D (Other Costs)    |
| Site Proposed Price/Month | Manually entered by the sales person                          |
| Site Gross Margin %       | `(Proposed Price – Total Cost) / Proposed Price × 100`        |

---

## Part 3: Overall Summary (Auto-calculated)

| Field                  | Value                                                         |
| ---------------------- | ------------------------------------------------------------- |
| Each site's cost row   | Site Name / Total Cost / Sale Price / Site GM%                |
| Total Monthly Cost     | Sum of all site total costs                                   |
| Total Monthly Price    | Sum of all site proposed prices                               |
| Overall Gross Margin % | `(Total Price – Total Cost) / Total Price × 100`              |
| Approval Determination | Auto-applied based on the GM% and Approval Matrix             |

---

## Approval Matrix

| Gross Margin %  | Approver               | Timeline  | Outcome                               |
| --------------- | ---------------------- | --------- | ------------------------------------- |
| ≥ 35%           | Auto-Approved (System) | Immediate | Proceeds to contract creation         |
| 10% – 34.99%    | Sales Manager          | 24 hours  | Sent to Sales Manager for review      |
| < 10%           | CEO / Operations Head  | 48 hours  | Sent to CEO/Ops Head for review       |

> The GMA sheet status is updated automatically based on the calculated GM%. No manual selection of approver is required.

---

## Form Actions

| Button            | Description                                                                  |
| ----------------- | ---------------------------------------------------------------------------- |
| **Save & Submit** | Validates all fields, calculates final GM%, applies approval matrix, submits |
| **Save as Draft** | Saves the sheet without submitting; not sent for approval                    |
| **Cancel**        | Discards all entries and returns to My Requests table                        |

---

## Validation Rules

| Validation                    | Rule                                                          |
| ----------------------------- | ------------------------------------------------------------- |
| Customer Required             | Must select a valid active customer                           |
| Service Type Required         | Must select at least one service type                         |
| At Least 1 Site Required      | Minimum one site must be added                                |
| At Least 1 Chemical per Site  | Minimum one chemical row per site                             |
| Proposed Price > 0            | Each site must have a proposed price greater than 0           |
| Chemical Qty > 0              | All chemical rows must have quantity > 0                      |
| Technician Count ≥ 1          | Must have at least 1 technician per site                      |
| Rate per Visit > 0            | Technician rate cannot be zero                                |

---

## System Behaviors on Save & Submit

| Trigger                    | System Action                                                         |
| -------------------------- | --------------------------------------------------------------------- |
| GM ≥ 35%                   | Status → Auto-Approved; no approval routing needed                    |
| GM 10–34.99%               | Status → Pending Manager; Sales Manager notified                      |
| GM < 10%                   | Status → Pending CEO; CEO/Ops Head notified                           |
| Save as Draft              | Status → Draft; no notifications sent                                 |
| Chemical selected          | Auto-fill UOM and Rate/UOM from Product Master                        |
| Rate overridden by user    | System retains overridden value; does not reset on re-selection       |

---

================================================================================

# 17.2.2 View GMA Sheet (My Requests)

**Description:**
Read-only view of a GMA sheet submitted by the logged-in user. Displays all service information, site-wise cost breakdowns, chemical details, labor and revisit costs, and the final gross margin summary with approval status and timeline.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       VIEW GMA SHEET  –  GMA-00091                          │
│  [← Back to My Requests]                                                     │
│                                                                              │
│  ─── HEADER INFORMATION ──────────────────────────────────────────────────  │
│  GMA ID          : GMA-00091         Status     : ✅ AUTO-APPROVED           │
│  Customer        : XYZ Hotel Chain   Service    : Cockroach AMC              │
│  Branch          : Mumbai            Created By : Ravi Sharma                │
│  Submitted On    : 18 Mar 2026       Approved On: 18 Mar 2026 (Immediate)   │
│                                                                              │
│  ─── PART 1: SERVICE INFORMATION ─────────────────────────────────────────  │
│  Duration: 12 Months   Start: 01 Apr 2026   Frequency: Monthly   Visits: 4  │
│  Contact: Ramesh Shah   Phone: +91 98765 00001   Email: ramesh@xyz.com       │
│                                                                              │
│  ─── PART 2: SITE-WISE BREAKDOWN ─────────────────────────────────────────  │
│                                                                              │
│  SITE 1: Mumbai HQ (5,000 sq ft) — 4 Visits/Month                           │
│  ┌────────────────────────────────────────────────────────────────────────┐ │
│  │  2A — CHEMICALS:                                                        │ │
│  │  Ficam 80WP   │ 200 g  │ ₹8/g   │ 4 visits → ₹6,400                   │ │
│  │  Biflex SC    │ 100 ml │ ₹4/ml  │ 4 visits → ₹1,600                   │ │
│  │  Chemical Sub-total (A)                         : ₹1,600 /month         │ │
│  │                                                                         │ │
│  │  2B — LABOR:  2 Technicians × ₹400/visit × 4   : ₹3,200 /month        │ │
│  │  2C — REVISIT: 1 revisit × ₹0                  : ₹0                    │ │
│  │  2D — OTHER:   —                                : ₹0                    │ │
│  │                                                                         │ │
│  │  SITE 1 TOTAL COST     : ₹4,800 /month                                  │ │
│  │  SITE 1 PROPOSED PRICE : ₹8,000 /month                                  │ │
│  │  SITE 1 GROSS MARGIN   : 40%                                             │ │
│  └────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SITE 2: Pune Warehouse (10,000 sq ft) — 2 Visits/Month                     │
│  [Collapsed – Click to expand]                                               │
│                                                                              │
│  SITE 3: Delhi Office (3,000 sq ft) — 4 Visits/Month                        │
│  [Collapsed – Click to expand]                                               │
│                                                                              │
│  ─── PART 3: OVERALL SUMMARY ─────────────────────────────────────────────  │
│  ┌──────────────────┬──────────────┬──────────────┬──────┐                  │
│  │ Site             │ Total Cost   │ Sale Price   │  GM% │                  │
│  │ Mumbai HQ        │ ₹4,800       │ ₹8,000       │  40% │                  │
│  │ Pune Warehouse   │ ₹2,800       │ ₹5,000       │  44% │                  │
│  │ Delhi Office     │ ₹3,400       │ ₹6,000       │  43% │                  │
│  │ ─────────────────────────────────────────────────────  │                  │
│  │ TOTAL            │ ₹11,000      │ ₹19,000      │  42% │                  │
│  └──────────────────┴──────────────┴──────────────┴──────┘                  │
│                                                                              │
│  APPROVAL STATUS  : ✅ Auto-Approved (GM 42% ≥ 35%)                         │
│  APPROVED ON      : 18 Mar 2026 (Immediate)                                 │
│  APPROVER         : System (Auto)                                            │
│                                                                              │
│  ─── AUDIT TRAIL ──────────────────────────────────────────────────────────  │
│  ┌──────────────────┬─────────────────────┬──────────────┬──────────────┐   │
│  │ Date & Time      │ Action              │ User         │ Remarks      │   │
│  │──────────────────┼─────────────────────┼──────────────┼──────────────│   │
│  │ 18 Mar 09:30 AM  │ Draft Created       │ Ravi Sharma  │ —            │   │
│  │ 18 Mar 10:00 AM  │ Submitted           │ Ravi Sharma  │ —            │   │
│  │ 18 Mar 10:00 AM  │ Auto-Approved       │ System       │ GM 42% ≥ 35% │   │
│  └──────────────────┴─────────────────────┴──────────────┴──────────────┘   │
│                                                                              │
│                    [BACK]   [DOWNLOAD PDF]                                  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Fields Displayed

| Section               | Field                                                               | Display Type   |
| --------------------- | ------------------------------------------------------------------- | -------------- |
| **Header**            | GMA ID, Customer, Service Type, Branch, Status, Created By, Date   | Static Display |
| **Part 1**            | Duration, Start Date, Frequency, Visits/Month, Contact, Phone, Email | Static Display |
| **Part 2 (per site)** | Site Name, Area, Chemicals Table, Labor, Revisit, Other Costs, Site Totals, Site GM% | Read-only |
| **Part 3**            | Site-wise summary table, Total Cost, Total Price, Overall GM%       | Auto / Read-only |
| **Approval**          | Status, Approval Type, Approver, Approved/Rejected Date, Remarks   | Read-only      |
| **Audit Trail**       | Timestamp, Action, User, Remarks                                    | Read-only      |

---

## Actions

| Button           | Description                                       |
| ---------------- | ------------------------------------------------- |
| **Back**         | Returns to My Requests tab                        |
| **Download PDF** | Exports the GMA sheet as a formatted PDF document |

---

================================================================================

# 17.3 Tab 3: Received Requests

**Description:**
Displays GMA sheets received by the logged-in manager or approver (Sales Manager or CEO/Ops Head) for review. Users can view full sheet details and approve or reject based on the margin analysis.

> **Visibility Rule:**
> - Sales Manager sees: GM sheets with 10% – 34.99% margin
> - CEO / Ops Head sees: GM sheets with < 10% margin

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
Full read-only view of a received GMA sheet for the approver to review all details — service info, site costs, chemicals used, labor breakdown, and margin summary — before taking an approval or rejection action.

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
│  Total Monthly Cost   : ₹8,500                                               │
│  Total Monthly Price  : ₹11,000                                              │
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

# 17.4 System Behavior & Business Rules

## 1. Gross Margin Calculation Formula

```
Gross Margin % = ( Sale Price – Total Cost ) / Sale Price × 100

Where:
  Total Cost  = Σ (Chemical Costs + Labor Costs + Revisit Costs + Other Costs) per site
  Sale Price  = Σ (Proposed Prices entered by sales person) per site
```

## 2. Chemical Cost Calculation (per Site per Month)

```
Chemical Cost/Month = Quantity per Visit × Rate per UOM × Visits per Month
```
Applied per chemical row; summed for the site's total chemical cost.

## 3. Labor Cost Calculation

```
Labor Cost/Month = No. of Technicians × Rate per Visit × Visits per Month
```

## 4. Site Total Cost Calculation

```
Site Total Cost = Chemical Sub-total (A) + Labor Cost (B) + Revisit Cost (C) + Other Costs (D)
```

## 5. Approval Matrix

| GM %           | Approver               | Timeline  | Post-Action                                |
| -------------- | ---------------------- | --------- | ------------------------------------------ |
| ≥ 35%          | Auto-Approved (System) | Immediate | Available for Contract module immediately  |
| 10% – 34.99%   | Sales Manager          | 24 hours  | Sheet visible in Sales Manager's Tab 3     |
| < 10%          | CEO / Ops Head         | 48 hours  | Sheet visible in CEO's Received Requests   |

## 6. Status Lifecycle

```
Draft → Submitted → Pending Approval → Approved / Rejected
                       (Auto-Approved if GM ≥ 35%)
```

## 7. Revoke Rules

- Sales person can **Revoke** only when status = **Pending** (not yet actioned by approver)
- Revoked sheets return to **Draft** status and can be edited and re-submitted
- Revoke is **not available** for Auto-Approved, Approved, or Rejected sheets

## 8. Notifications

| Event              | Recipient        | Channel                  |
| ------------------ | ---------------- | ------------------------ |
| GMA Submitted      | Approver         | In-app + Email           |
| Auto-Approved      | Sales Person     | In-app notification      |
| Approved           | Sales Person     | In-app + Email           |
| Rejected           | Sales Person     | In-app + Email + Remarks |
| Deadline Reminder  | Approver         | Auto-reminder at 50% of timeline |

## 9. PDF Export

- Approved GMA sheets can be exported as a PDF summarizing service info, site-wise costs, chemicals used, labor breakdown, and the overall margin analysis.
- PDF is suitable for internal review or management records.

## 10. Module Dependencies

- **GMA Approval is a prerequisite for Contract Creation** (Module 18)
- Products / Chemicals are pulled from **Module 10 – Product Master** (Consumable type)
- Customer and Contact info pulled from **Module 4 – Customer Management**
- Service templates and visit frequency pulled from **Module 5 – Service Management**

---

================================================================================

# 17.5 RBAC – Role-Based Access Control

| Role              | Tab 1 (GMA Entries) | Tab 2 (My Requests)    | Tab 3 (Received Requests) | Add GMA | Approve/Reject   |
| ----------------- | ------------------- | ---------------------- | ------------------------- | ------- | ---------------- |
| Sales Person      | View (own)          | View / Add / Revoke    | —                         | ✅       | ❌               |
| Sales Manager     | View (team)         | View (own)             | View / Approve / Reject   | ✅       | ✅ (10–34.99%)   |
| CEO / Ops Head    | View (all)          | View (own)             | View / Approve / Reject   | ✅       | ✅ (< 10%)       |
| Admin             | View (all)          | View (all)             | View (all)                | ✅       | ❌               |

---

================================================================================

# 17.6 Navigation Flow Summary

```
MODULE 17: GMA MANAGEMENT
│
├── Tab 1: GMA Entries
│   └── [View] → 17.1.1 View GMA Sheet (Read-only)
│
├── Tab 2: My Requests
│   ├── [+ Add GMA Sheet] → 17.2.1 Add GMA Sheet Form
│   │     ├── [Save & Submit] → GM Calculated
│   │     │       ├── GM ≥ 35%       → ✅ Auto-Approved → Contract Creation Unlocked
│   │     │       ├── GM 10–34.99%   → 🟡 Sent to Sales Manager (Tab 3 of Manager)
│   │     │       └── GM < 10%       → 🔴 Sent to CEO/Ops Head (Tab 3 of CEO)
│   │     └── [Save as Draft] → Status = Draft (no notification)
│   ├── [View] → 17.2.2 View My GMA Sheet (Read-only + PDF Download)
│   └── [Revoke] → Resets sheet to Draft (if Pending only)
│
└── Tab 3: Received Requests (Approvers only)
    ├── [View] → 17.3.1 View Received GMA Sheet (Full detail + Approval section)
    └── [Approve] → 17.3.2 Approve / Reject GMA Sheet Form
          ├── [Approve] → GMA Approved + Sales person notified + Contract unlocked
          └── [Reject]  → GMA Rejected + Sales person notified with remarks
```

---

================================================================================

# 17.7 Audit Trail

Every GMA sheet action is tracked for compliance and operational transparency.

| Field      | Description                     |
| ---------- | ------------------------------- |
| Date & Time | Timestamp of the action         |
| Action     | Status change / event           |
| User       | Person who performed the action |
| Remarks    | Optional comment or reason      |

Tracked events include:
- Draft Created
- Submitted
- Auto-Approved
- Sent for Approval
- Approved
- Rejected (with reason)
- Revoked
- PDF Downloaded
