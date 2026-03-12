# 🎯 MODULE 11: STOCK MANAGEMENT

## Overview

Comprehensive stock management system that handles inventory operations across multiple branches with centralized control. Manages three distinct stock types — **Assets** (individually tracked with Asset IDs), **Consumables** (bulk quantity management), and **Resell** (inventory for sale). Supports branch-wise stock distribution with Head Office as the central hub, stock requests and transfers between branches, and complete audit trails for all inventory movements.

**Module Connections:**

- **Depends on:** Module 9 (Tax Management for purchase calculations), Module 10 (Product Master for stock items)
- **Used by:** Module 8 (Employee assignment for Assets), Service Operations (Consumable usage), Billing (Resell items)
- **Prerequisites:** Module 9 and Module 10 must be configured before activating stock operations

---

## 11.1 Stock Dashboard – Table View

**Description:** Unified stock view showing product-wise quantities across accessible branches with type-wise breakdown and filtering. Provides real-time visibility into stock levels, in-transit items, reserved quantities, and asset assignments.

### Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           STOCK DASHBOARD                                    │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                │  │
│  │                                                                        │  │
│  │ Branch View: [▼ All My Branches / Central / BLR / HYD / BOM ▼]        │  │
│  │ Category: [☑ Chemical ☑ Sprayer ☑ Machine ☑ Trap ☑ Tool ☑ Other]    │  │
│  │ Stock Type: [☑ Assets ☑ Consumable ☑ Resell]                          │  │
│  │ Status: [☑ Available ☑ Low ☑ Out ☑ Inactive]                          │  │
│  │ HSN Code: [__________]          Has Assets: [☐ Yes ☐ No]              │  │
│  │ Created Date: [📅 From] - [📅 To]                                      │  │
│  │                                                                        │  │
│  │ Search: [____________________] (Product Code / Name / HSN / Asset ID) │  │
│  │                                                                        │  │
│  │ [Reset Filters]                                    [+ ADD STOCK]       │  │
│  │                                                                        │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  STOCK OVERVIEW TABLE                                                        │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │Img│Product Code│Product Name│Category│Brand│HSN│Assets│Cons.│Resell│My  │ │
│  │   │            │            │        │     │   │Qty   │Qty  │Qty   │Br. │ │
│  │───┼────────────┼────────────┼────────┼─────┼───┼──────┼─────┼──────┼────│ │
│  │📷 │BSP3-001    │Brass Sprayr│Sprayer │Agro │842│ 5    │ 15  │ 0    │ 8  │ │
│  │📷 │CH-3808-001 │Chemical X  │Chemical│ABC  │380│ 0    │ 50  │ 20   │ 30 │ │
│  │📷 │RBS-001     │Rodent Box  │Trap    │Pest │732│ 10   │ 25  │ 0    │ 12 │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Other│Central│In-Transit│Reserved│Available│Base│Status│Actions           ││
│  │Br.  │Stock  │Qty       │Qty     │to Req.  │UOM │      │                  ││
│  │─────┼───────┼──────────┼────────┼─────────┼────┼──────┼──────────────────││
│  │ 12  │ 20    │ 2        │ 5      │ 13      │Nos │🟢    │[View][Req][Trans]││
│  │ 40  │ 100   │ 0        │ 10     │ 90      │Ltr │🟡    │[View][Req][Trans]││
│  │ 15  │ 30    │ 5        │ 0      │ 25      │Nos │🟢    │[View][Req][Trans]││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   10   Next                         │
│                                                                              │
│  Legend: 🟢 Available  🟡 Low Stock  🔴 Out of Stock  📦 In-Transit  ⏳ Reserved│
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Table View Fields

| Field                    | Type            | Required | Description                                        |
| ------------------------ | --------------- | -------- | -------------------------------------------------- |
| Cover Image              | Image Thumbnail | Auto     | Product photo from Product Master                  |
| Product Code             | Text            | Auto     | Unique SKU from Product Master                     |
| Product Name             | Text            | Auto     | Full product name                                  |
| Category                 | Text            | Auto     | Chemical / Sprayer / Machine / Trap / Tool / Other |
| Company / Brand          | Text            | Auto     | Manufacturer name                                  |
| HSN Code                 | Link            | Auto     | Clickable link to view tax details                 |
| **Assets Qty**           | Number          | Auto     | Individual trackable units count                   |
| **Consumable Qty**       | Number          | Auto     | Bulk usable stock quantity                         |
| **Resell Qty**           | Number          | Auto     | Stock available for sale                           |
| **My Branch Stock**      | Number          | Auto     | Total at user's primary branch                     |
| **Other Branches Stock** | Number          | Auto     | Sum of other accessible branches                   |
| **Central Stock**        | Number          | Auto     | Available at Head Office                           |
| **In-Transit Qty**       | Number          | Auto     | Dispatched but not yet received                    |
| **Reserved Qty**         | Number          | Auto     | Allocated to pending requests                      |
| **Available to Request** | Number          | Auto     | Calculated: Central - Reserved - In-Transit        |
| Base UOM                 | Text            | Auto     | Unit of measure from Product Master                |
| Status                   | Badge           | Auto     | Available / Low / Out / Inactive                   |
| Actions                  | Button Group    | —        | View / Request Stock / Transfer / History          |

### Visual Indicators

| Indicator | Meaning                      | Color  |
| --------- | ---------------------------- | ------ |
| 🟢        | Stock healthy (> Min Level)  | Green  |
| 🟡        | Low stock (≤ Min Level)      | Yellow |
| 🔴        | Out of stock (= 0)           | Red    |
| 📦        | In-transit to branch         | Blue   |
| ⏳        | Reserved for request         | Orange |
| 🔧        | Assets assigned to employees | Purple |

### Actions (RBAC Based)

| Action          | Head Ops            | Branch Manager          | Branch User           |
| --------------- | ------------------- | ----------------------- | --------------------- |
| View Details    | ✅                  | ✅                      | ✅                    |
| Request Stock   | ✅ (Central→Branch) | ✅ (For their branch)   | ✅ (For their branch) |
| Transfer Stock  | ✅ (Any→Any)        | ✅ (Their branch→Other) | ❌                    |
| Add Stock       | ✅ (To Central)     | ✅ (Receive & Allocate) | ❌                    |
| Edit Allocation | ✅                  | ✅ (Their branch only)  | ❌                    |
| View History    | ✅ (All)            | ✅ (Their branches)     | ✅ (Their branch)     |

### Filters

| Filter       | Type         | Options                                            |
| ------------ | ------------ | -------------------------------------------------- |
| Branch View  | Dropdown     | All My Branches / Central / BLR / HYD / BOM / etc. |
| Category     | Multi-select | Chemical / Sprayer / Machine / Trap / Tool / Other |
| Stock Type   | Multi-select | Assets / Consumable / Resell                       |
| Status       | Multi-select | Available / Low / Out / Inactive                   |
| HSN Code     | Search       | Numeric search                                     |
| Has Assets   | Toggle       | Yes / No                                           |
| Created Date | Date Range   | From - To                                          |

### Search

Searchable by: Product Code, Product Name, HSN Code, Asset ID (if asset tracking enabled)

---

## 11.1.1 Tab 2: My Requests

**Description:** Track all stock requests and transfer requests initiated by or assigned to the user. Provides complete visibility into request lifecycle from draft to receipt.

### Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           STOCK DASHBOARD                                    │
│                                                                              │
│  [Tab 1: All Products]  [Tab 2: My Requests]  [Tab 3: Approvals & Receipts]  │
│                                                                              │
│  Segmented Control: [All] [Draft] [Pending] [Approved] [In-Transit] [Received]│
│                     [Rejected] [On Hold]                                      │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                │  │
│  │ Request Type: [☑ Stock Request ☑ Branch Transfer ☑ Asset Assignment]  │  │
│  │ Direction: [☑ Inward ☑ Outward]                                       │  │
│  │ Date Range: [📅 From] - [📅 To]                                        │  │
│  │ Priority: [☑ Low ☑ Normal ☑ High ☑ Urgent]                            │  │
│  │                                                                        │  │
│  │ Search: [____________________] (Request ID / Product / Branch)        │  │
│  │                                                                        │  │
│  │ [Reset Filters]                                                        │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  MY REQUESTS TABLE                                                           │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │Request ID  │Type │Direction│From│To │Items│Total│Assets│Status│Priority│Exp.│
│  │            │     │         │    │   │Count│Qty  │Count │      │        │Date│
│  │────────────┼─────┼─────────┼────┼───┼─────┼─────┼──────┼──────┼────────┼────│
│  │SR-BLR-001  │Stock│Inward   │CEN │BLR│ 3   │ 25  │ 5    │Pend. │High    │18Jan│
│  │TR-2024-056 │Trans│Inward   │HYD │BLR│ 2   │ 15  │ 3    │In-Tr │Normal  │20Jan│
│  │TR-2024-042 │Trans│Outward  │BLR │BOM│ 1   │ 10  │ 0    │Pend. │Normal  │22Jan│
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Requested By│Requested Date│Action                                        ││
│  │────────────┼──────────────┼──────────────────────────────────────────────││
│  │John Doe    │15 Jan 2024   │[View] [Cancel]                               ││
│  │Jane Smith  │16 Jan 2024   │[View] [Receive]                              ││
│  │John Doe    │17 Jan 2024   │[View] [Dispatch] [Cancel]                    ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   10   Next                         │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Table Fields

| Field          | Type   | Description                                            |
| -------------- | ------ | ------------------------------------------------------ |
| Request ID     | Text   | SR-XXXX-XXXX (Stock Request) / TR-XXXX-XXXX (Transfer) |
| Request Type   | Badge  | Stock Request / Branch Transfer / Asset Assignment     |
| Direction      | Icon   | Inward / Outward indicator                             |
| From           | Text   | Source location                                        |
| To             | Text   | Destination location                                   |
| Items Count    | Number | Product lines in request                               |
| Total Qty      | Number | Units requested                                        |
| Assets Count   | Number | Individual asset units (if applicable)                 |
| Status         | Badge  | Current workflow stage                                 |
| Priority       | Badge  | Low / Normal / High / Urgent                           |
| Expected Date  | Date   | Promised delivery date                                 |
| Requested By   | Text   | User who created request                               |
| Requested Date | Date   | Creation timestamp                                     |
| Action         | Button | Context-based actions                                  |

### Request Types & Actions

| Type             | Direction | Description                  | Action Buttons                       |
| ---------------- | --------- | ---------------------------- | ------------------------------------ |
| Stock Request    | Inward    | Branch → Head Office         | View / Cancel (if pending) / Receive |
| Transfer Request | Inward    | Other Branch → My Branch     | View / Receive / Report Issue        |
| Transfer Request | Outward   | My Branch → Other Branch     | View / Dispatch / Cancel             |
| Asset Assignment | Inward    | Stock → Employee (My Branch) | View / Acknowledge                   |
| Asset Return     | Outward   | Employee → Stock             | View / Confirm Return                |

---

## 11.1.2 Tab 3: Approvals & Receipts

**Description:** For Branch Managers and Head Ops to approve incoming requests and confirm receipts. Provides validation indicators for stock availability.

### Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           STOCK DASHBOARD                                    │
│                                                                              │
│  [Tab 1: All Products]  [Tab 2: My Requests]  [Tab 3: Approvals & Receipts]  │
│                                                                              │
│  Segmented Control: [Pending Approval] [Pending Receipt] [Completed Today] [All History]│
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                │  │
│  │ Type: [☑ Request for Approval ☑ Receipt Confirmation]                 │  │
│  │ From Branch: [▼ Dropdown ▼]     To Branch: [▼ Dropdown ▼]             │  │
│  │ Requested By: [🔍 Search ▼]     Date Range: [📅 From] - [📅 To]       │  │
│  │                                                                        │  │
│  │ [Reset Filters]                                                        │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  APPROVALS & RECEIPTS TABLE                                                  │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │Request ID  │Type│From│To │Items│Total Value│Requested By│Validation│Action│
│  │            │    │    │   │     │(₹)        │            │          │      │
│  │────────────┼────┼────┼───┼─────┼───────────┼────────────┼──────────┼──────│
│  │SR-BLR-001  │Appr│BLR │CEN│ 3   │ ₹45,000   │John Doe    │✅ Avail. │Approve│
│  │SR-HYD-002  │Appr│HYD │CEN│ 2   │ ₹28,500   │Jane Smith  │⚠️ Partial│Approve│
│  │TR-2024-056 │Rcpt│CEN │BLR│ 2   │ ₹32,000   │Rajesh K.   │⏳ Pending│Receive│
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Request Date│Expected Date│Reviewed By│Review Date│Status                ││
│  │────────────┼─────────────┼───────────┼───────────┼──────────────────────││
│  │15 Jan 2024 │18 Jan 2024  │—          │—          │Pending Approval      ││
│  │16 Jan 2024 │19 Jan 2024  │—          │—          │Pending Approval      ││
│  │16 Jan 2024 │20 Jan 2024  │—          │—          │In-Transit            ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   10   Next                         │
│                                                                              │
│  Validation Legend: ✅ Available  ⚠️ Partial  ❌ Insufficient  ⏳ Pending     │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Table Fields

| Field           | Type     | Description                                              |
| --------------- | -------- | -------------------------------------------------------- |
| Request ID      | Text     | SR-XXXX-XXXX / TR-XXXX-XXXX                              |
| Type            | Badge    | Request for Approval / Receipt Confirmation              |
| From            | Text     | Requesting party                                         |
| To              | Text     | Receiving party (your branch/central)                    |
| Items           | Number   | Product count                                            |
| Total Value (₹) | Currency | With tax included                                        |
| Requested By    | Text     | User + Date                                              |
| Validation      | Icon     | ✅ Available / ⚠️ Partial / ❌ Insufficient / ⏳ Pending |
| Request Date    | Date     | Original request date                                    |
| Expected Date   | Date     | Promised delivery                                        |
| Reviewed By     | Text     | Approver name (if any)                                   |
| Review Date     | Date     | Approval/review timestamp                                |
| Status          | Badge    | Current state                                            |
| Action          | Button   | Approve / Reject / Hold / Receive                        |

---

## 11.2 Add Stock (Multi-Mode)

**Description:** Multi-mode interface for adding stock to Central Branch or receiving and allocating to specific branches with type-wise distribution. Supports three distinct workflows based on user role and context.

### Mode Selection Screen

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              ADD STOCK                                       │
│                                                                              │
│  Select Mode:                                                                │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │   [📦 ADD TO CENTRAL STOCK]                                             │ │
│  │   Head Operations only. Add new purchase directly to Central Branch.    │ │
│  │   Includes asset ID generation and initial allocation options.          │ │
│  │   [SELECT]                                                              │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │   [📥 RECEIVE & ALLOCATE]                                               │ │
│  │   Branch Manager. Receive approved request and allocate to branch       │ │
│  │   stock or assign as assets to employees.                               │ │
│  │   [SELECT]                                                              │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │   [🔄 DIRECT BRANCH TRANSFER]                                           │ │
│  │   Head Operations. Transfer between branches without prior request.     │ │
│  │   For emergency stock redistribution or asset reallocation.             │ │
│  │   [SELECT]                                                              │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 11.2.1 Mode: Add to Central Stock (Head Ops)

**Description:** Add new purchase directly to Central Branch (Head Office) with full procurement details, tax calculations, and initial asset ID generation.

#### Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      ADD TO CENTRAL STOCK                                    │
│                                                                              │
│  Section 1: Product Selection                                                │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Product*              : [🔍 Search Dropdown ▼]                        │ │
│  │  Product Code          : [Auto-filled] [Read Only]                     │ │
│  │  HSN Code              : [Auto-filled] [Read Only]                     │ │
│  │  Current Central Stock : [Auto-fetched] [Read Only]                    │ │
│  │  Base UOM              : [Auto-filled] [Read Only]                     │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  Section 2: Stock Type Allocation                                            │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Total Quantity Received* : [________] [Base UOM]                      │ │
│  │                                                                         │ │
│  │  ALLOCATE AS:                                                           │ │
│  │  ┌─────────────────┬─────────────────┬─────────────────┐               │ │
│  │  │ Assets Qty      │ Consumable Qty  │ Resell Qty      │               │ │
│  │  │ [________]      │ [________]      │ [________]      │               │ │
│  │  │ Individual      │ Bulk stock      │ For sale        │               │ │
│  │  │ trackable units │ for use         │ stock           │               │ │
│  │  └─────────────────┴─────────────────┴─────────────────┘               │ │
│  │                                                                         │ │
│  │  Validation: Assets + Consumable + Resell = Total ✓                     │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  Section 3: Asset Details (Visible if Assets Qty > 0)                        │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Asset ID Generation:  ○ Auto  ○ Manual                                │ │
│  │                                                                         │ │
│  │  IF AUTO:                                                               │ │
│  │  Asset ID Prefix*      : [________] (e.g., "BSP3")                     │ │
│  │  Starting Sequence*    : [________] (e.g., 0026)                       │ │
│  │  Asset ID Preview      : BSP3/0026, BSP3/0027, BSP3/0028...            │ │
│  │                                                                         │ │
│  │  Default Assignment*   : [▼ Central Warehouse / Specific Branch ▼]     │ │
│  │  Assignment Type*      : [▼ Branch Pool / Direct to Employee ▼]        │ │
│  │                                                                         │ │
│  │  GENERATED ASSET TABLE:                                                 │ │
│  │  ┌───────────┬─────────────────┬─────────────┬─────────┬────────┐      │ │
│  │  │ Asset ID  │ Product Name    │ Assigned To │ Branch  │ Status │      │ │
│  │  │───────────┼─────────────────┼─────────────┼─────────┼────────│      │ │
│  │  │ BSP3/0026 │ Brass Sprayer   │ [Unassigned]│ Central │ Avail. │      │ │
│  │  │ BSP3/0027 │ Brass Sprayer   │ [Unassigned]│ Central │ Avail. │      │ │
│  │  │ BSP3/0028 │ Brass Sprayer   │ [Unassigned]│ Central │ Avail. │      │ │
│  │  └───────────┴─────────────────┴─────────────┴─────────┴────────┘      │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  Section 4: Purchase & Tax Details                                           │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Supplier / Vendor*    : [🔍 Search Dropdown ▼]                        │ │
│  │  Purchase Order Ref    : [________________] (Optional)                 │ │
│  │  Invoice Number*       : [________________]                            │ │
│  │  Invoice Date*         : [📅 Date Picker]                              │ │
│  │                                                                         │ │
│  │  Invoice Amount (₹)*   : ₹ [________] (Pre-tax total)                  │ │
│  │  Tax Amount (₹)*       : ₹ [Auto-calculated from HSN]                  │ │
│  │  Total with Tax (₹)*   : ₹ [Auto-calculated]                           │ │
│  │                                                                         │ │
│  │  Invoice Copy*         : [📎 Upload PDF/JPG] (Max 5MB)                 │ │
│  │                                                                         │ │
│  │  Batch Number          : [________________] (For consumables)          │ │
│  │  Manufacturing Date    : [📅 Date Picker] (For consumables)            │ │
│  │  Expiry Date           : [📅 Date Picker] (For consumables, > Mfg)     │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  Section 5: Initial Allocation (Optional)                                    │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Immediate Transfer to Branch: [☑ BLR ☑ HYD ☑ BOM ☑ CHN]              │ │
│  │                                                                         │ │
│  │  IF BRANCHES SELECTED:                                                  │ │
│  │  BLR Qty               : [________]                                    │ │
│  │  HYD Qty               : [________]                                    │ │
│  │  BOM Qty               : [________]                                    │ │
│  │  CHN Qty               : [________]                                    │ │
│  │                                                                         │ │
│  │  Remain at Central     : [Auto-calculated]                             │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│                              [SAVE]  [CANCEL]                                │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### Section 1: Product Selection Fields

| Field                 | Type            | Required | Validation                                 |
| --------------------- | --------------- | -------- | ------------------------------------------ |
| Product               | Search Dropdown | Yes      | Must be active product from Product Master |
| Product Code          | Auto-filled     | System   | Read-only                                  |
| HSN Code              | Auto-filled     | System   | Read-only                                  |
| Current Central Stock | Auto-fetched    | System   | Read-only reference                        |
| Base UOM              | Auto-filled     | System   | Read-only                                  |

#### Section 2: Stock Type Allocation Fields

| Field                   | Type   | Required | Validation                |
| ----------------------- | ------ | -------- | ------------------------- |
| Total Quantity Received | Number | Yes      | Must be > 0               |
| Assets Qty              | Number | No       | ≥ 0, sum must equal Total |
| Consumable Qty          | Number | No       | ≥ 0, sum must equal Total |
| Resell Qty              | Number | No       | ≥ 0, sum must equal Total |

> **Validation Rule:** Assets Qty + Consumable Qty + Resell Qty = Total Quantity Received

#### Section 3: Asset Details Fields (Conditional)

| Field               | Type     | Required            | Validation                          |
| ------------------- | -------- | ------------------- | ----------------------------------- |
| Asset ID Generation | Radio    | Yes (if Assets > 0) | Auto / Manual                       |
| Asset ID Prefix     | Text     | Yes (if Auto)       | Alphanumeric, 2–10 chars            |
| Starting Sequence   | Number   | Yes (if Auto)       | Numeric, 4–6 digits                 |
| Asset ID Preview    | Auto     | System              | Read-only preview                   |
| Default Assignment  | Dropdown | Yes (if Assets > 0) | Central Warehouse / Specific Branch |
| Assignment Type     | Dropdown | Yes (if Assets > 0) | Branch Pool / Direct to Employee    |

#### Section 4: Purchase & Tax Details Fields

| Field              | Type            | Required    | Validation                                   |
| ------------------ | --------------- | ----------- | -------------------------------------------- |
| Supplier / Vendor  | Search Dropdown | Yes         | Must be active supplier                      |
| Purchase Order Ref | Text            | No          | Alphanumeric, max 50 chars                   |
| Invoice Number     | Text            | Yes         | Unique, alphanumeric, max 50 chars           |
| Invoice Date       | Date            | Yes         | Cannot be future date                        |
| Invoice Amount (₹) | Currency        | Yes         | Numeric, > 0, max 2 decimals                 |
| Tax Amount (₹)     | Auto            | System      | Calculated from HSN code rates               |
| Total with Tax (₹) | Auto            | System      | Invoice Amount + Tax Amount                  |
| Invoice Copy       | File Upload     | Yes         | PDF/JPG/PNG, max 5MB                         |
| Batch Number       | Text            | Conditional | Required for consumables, max 50 chars       |
| Manufacturing Date | Date            | Conditional | Required for consumables                     |
| Expiry Date        | Date            | Conditional | Required for consumables, must be > Mfg Date |

#### Section 5: Initial Allocation Fields

| Field                        | Type         | Required    | Validation                               |
| ---------------------------- | ------------ | ----------- | ---------------------------------------- |
| Immediate Transfer to Branch | Multi-select | No          | Select from active branches              |
| [Branch] Qty                 | Number       | Conditional | Required if branch selected, sum ≤ Total |

---

### 11.2.2 Mode: Receive & Allocate (Branch Manager)

**Description:** Receive approved stock request and allocate to branch inventory with asset assignment to employees.

#### Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        RECEIVE & ALLOCATE                                    │
│                                                                              │
│  Section 1: Request Reference (Read-only)                                    │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Request ID            : SR-BLR-2024-001234                            │ │
│  │  Approved By           : Rajesh Kumar (Head Ops)                       │ │
│  │  Approved Date         : 15 Jan, 2024                                  │ │
│  │  Dispatch Date         : 16 Jan, 2024                                  │ │
│  │  Carrier               : DTDC                                          │ │
│  │  LR Number             : DTDC-7845123456                               │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  Section 2: Receipt Verification                                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Received Date*        : [📅 Date Picker]                              │ │
│  │  Received By*          : [Auto-filled: Current User]                   │ │
│  │  Package Condition*    : [▼ Good / Damaged / Missing Items ▼]          │ │
│  │                                                                         │ │
│  │  ITEMS VERIFICATION TABLE:                                              │ │
│  │  ┌─────────────┬──────────┬──────────┬───────┬─────────────────┐       │ │
│  │  │ Product     │ Expected │ Received │ Match │ Notes           │       │ │
│  │  │─────────────┼──────────┼──────────┼───────┼─────────────────│       │ │
│  │  │ BSP3-001    │ 5        │ [ 5 ]    │ ✅    │                 │       │ │
│  │  │ CH-3808-001 │ 10       │ [ 10 ]   │ ✅    │                 │       │ │
│  │  │ RBS-001     │ 20       │ [ 18 ]   │ ⚠️    │ 2 missing       │       │ │
│  │  └─────────────┴──────────┴──────────┴───────┴─────────────────┘       │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  Section 3: Stock Type Allocation                                            │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  For each received product, allocate to types:                          │ │
│  │  ┌─────────────┬───────────────┬────────────┬────────────┬───────────┐ │ │
│  │  │ Product     │ Total Received│ Assets Qty │ Consumable │ Resell Qty│ │ │
│  │  │             │               │            │ Qty        │           │ │ │
│  │  │─────────────┼───────────────┼────────────┼────────────┼───────────│ │ │
│  │  │ BSP3-001    │ 5             │ [ 2 ]      │ [ 3 ]      │ [ 0 ]     │ │ │
│  │  │ CH-3808-001 │ 10            │ [ 0 ]      │ [ 10 ]     │ [ 0 ]     │ │ │
│  │  │ RBS-001     │ 18            │ [ 5 ]      │ [ 13 ]     │ [ 0 ]     │ │ │
│  │  └─────────────┴───────────────┴────────────┴────────────┴───────────┘ │ │
│  │  Validation: Assets + Consumable + Resell = Total Received ✓            │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  Section 4: Asset Assignment (Visible if Assets Qty > 0)                     │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  ┌───────────┬─────────┬────────────┬──────────┬───────────┬──────────┐│ │
│  │  │ Asset ID  │ Product │ Assign To* │ Employee │ Condition │ Location ││ │
│  │  │───────────┼─────────┼────────────┼──────────┼───────────┼──────────││ │
│  │  │ BSP3/0156 │BSP3-001 │ Employee   │[Select ▼]│ New       │ [Auto]   ││ │
│  │  │ BSP3/0157 │BSP3-001 │Branch Pool │    —     │ New       │BLR Whse  ││ │
│  │  │ RBS/0089  │RBS-001  │ Employee   │[Select ▼]│ New       │ [Auto]   ││ │
│  │  │ RBS/0090  │RBS-001  │Branch Pool │    —     │ New       │BLR Whse  ││ │
│  │  └───────────┴─────────┴────────────┴──────────┴───────────┴──────────┘│ │
│  │                                                                         │ │
│  │  Assignment Options:                                                    │ │
│  │  • Employee: Direct assignment to technician/staff                      │ │
│  │  • Branch Pool: Available for assignment later                          │ │
│  │  • Central Reserve: Keep at branch central location                     │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  Section 5: Acknowledgment                                                   │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Upload Receipt Photo* : [📎 Upload] (Required)                        │ │
│  │  Upload Package Photo  : [📎 Upload] (Optional)                        │ │
│  │  Remarks               : [___________________________]                 │ │
│  │  Confirm Receipt*      : [☐ I confirm receipt of items as verified]   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│                              [CONFIRM RECEIPT]  [CANCEL]                     │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### Section 2: Receipt Verification Fields

| Field              | Type        | Required | Validation                        |
| ------------------ | ----------- | -------- | --------------------------------- |
| Received Date      | Date        | Yes      | Cannot be future date             |
| Received By        | Auto-filled | System   | Current user                      |
| Package Condition  | Dropdown    | Yes      | Good / Damaged / Missing Items    |
| Items Verification | Table       | Yes      | Expected vs Actual quantity match |

#### Section 3: Stock Type Allocation Fields

| Field          | Type   | Required    | Validation                       |
| -------------- | ------ | ----------- | -------------------------------- |
| Assets Qty     | Number | Conditional | Sum per product = Total Received |
| Consumable Qty | Number | Conditional | Sum per product = Total Received |
| Resell Qty     | Number | Conditional | Sum per product = Total Received |

#### Section 4: Asset Assignment Fields

| Field     | Type            | Required    | Validation                               |
| --------- | --------------- | ----------- | ---------------------------------------- |
| Asset ID  | Auto            | System      | Pre-generated                            |
| Product   | Auto            | System      | From allocation                          |
| Assign To | Dropdown        | Yes         | Employee / Branch Pool / Central Reserve |
| Employee  | Search Dropdown | Conditional | Required if Assign To = Employee         |
| Condition | Dropdown        | Yes         | New / Good / Fair / Needs Repair         |
| Location  | Auto/Text       | Yes         | Auto if Employee, manual if Pool         |

#### Section 5: Acknowledgment Fields

| Field                | Type        | Required | Validation         |
| -------------------- | ----------- | -------- | ------------------ |
| Upload Receipt Photo | File Upload | Yes      | JPG/PNG, max 5MB   |
| Upload Package Photo | File Upload | No       | JPG/PNG, max 5MB   |
| Remarks              | Text Area   | No       | Max 500 characters |
| Confirm Receipt      | Checkbox    | Yes      | Must be checked    |

**Post-Receipt Actions:**

- Stock added to branch inventory
- Assets created with IDs and assignments
- Central stock reduced
- In-transit cleared
- Notification sent to Head Ops
- Activity log updated

---

## 11.3 Stock Request (Branch to Central)

**Description:** Branch users request stock from Head Office with type-wise requirement specification and priority setting.

### Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           STOCK REQUEST                                      │
│                                                                              │
│  Request ID: SR-[BRANCH]-YYYY-SEQ (Auto-generated)                           │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ HEADER INFORMATION                                                      │ │
│  │                                                                         │ │
│  │  Requesting Branch*    : [Auto-filled: Current User's Branch]          │ │
│  │  Requested By*         : [Auto-filled: Current User]                   │ │
│  │  Request Date*         : [Auto-filled: Current Date]                   │ │
│  │  Priority*             : [▼ Low / Normal / High / Urgent ▼]            │ │
│  │  Required By Date*     : [📅 Date Picker]                              │ │
│  │  Purpose*              : [____________________________________]        │ │
│  │                          [Text Area - Business justification]           │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ ITEMS TABLE                                                             │ │
│  │                                                                         │ │
│  │  [+ ADD ITEM]                                                           │ │
│  │                                                                         │ │
│  │  ┌───────────┬─────────────┬─────────────┬───────────┬───────────┬─────┐│ │
│  │  │ Product*  │ Product Code│Current Stock│ Assets    │Consumable │Resell│
│  │  │           │             │(Reference)  │ Req. Qty* │ Req. Qty* │Req.*│
│  │  │───────────┼─────────────┼─────────────┼───────────┼───────────┼─────│ │
│  │  │[Search ▼] │[Auto-filled]│[Auto-fetched]│[   5   ] │[   10   ] │[ 0 ]│
│  │  │[Search ▼] │[Auto-filled]│[Auto-fetched]│[   0   ] │[   20   ] │[ 0 ]│
│  │  │[Search ▼] │[Auto-filled]│[Auto-fetched]│[   10  ] │[   0    ] │[ 5 ]│
│  │  └───────────┴─────────────┴─────────────┴───────────┴───────────┴─────┘│ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ ITEMS TABLE (CONTINUED)                                                 │ │
│  │                                                                         │ │
│  │  ┌───────────┬─────────────┬─────────────┬─────────────┬─────────────┐ │ │
│  │  │ Total Qty │ UOM         │ Est. Cost(₹)│ Tax (₹)     │ Purpose/Item│ │ │
│  │  │───────────┼─────────────┼─────────────┼─────────────┼─────────────│ │ │
│  │  │[Auto-sum] │[Auto-filled]│[Base×Qty]   │[Tax×Qty]    │[__________] │ │ │
│  │  │[Auto-sum] │[Auto-filled]│[Base×Qty]   │[Tax×Qty]    │[__________] │ │ │
│  │  │[Auto-sum] │[Auto-filled]│[Base×Qty]   │[Tax×Qty]    │[__________] │ │ │
│  │  └───────────┴─────────────┴─────────────┴─────────────┴─────────────┘ │ │
│  │  [Remove] buttons per row                                               │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ SUMMARY SECTION                                                         │ │
│  │                                                                         │ │
│  │  Total Products              : [Auto-count]                            │ │
│  │  Total Assets Requested      : [Auto-sum]                              │ │
│  │  Total Consumables Requested : [Auto-sum]                              │ │
│  │  Total Resell Requested      : [Auto-sum]                              │ │
│  │  ───────────────────────────────────────────────────────────────────── │ │
│  │  Total Estimated Value (₹)   : [Auto-sum]                              │ │
│  │                                                                         │ │
│  │  Notes for Approver          : [____________________________________]  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│                    [SAVE DRAFT]        [SUBMIT REQUEST]                      │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Form Fields

| Field             | Type           | Required | Validation                   |
| ----------------- | -------------- | -------- | ---------------------------- |
| Request ID        | Auto-generated | System   | SR-[BRANCH]-YYYY-SEQ format  |
| Requesting Branch | Auto-filled    | System   | Current user's branch        |
| Requested By      | Auto-filled    | System   | Current user                 |
| Request Date      | Auto-filled    | System   | Current date                 |
| Priority          | Dropdown       | Yes      | Low / Normal / High / Urgent |
| Required By Date  | Date           | Yes      | ≥ Today + 1 day              |
| Purpose           | Text Area      | Yes      | Min 20 characters            |

### Items Table Fields

| Field                | Type            | Required | Validation           |
| -------------------- | --------------- | -------- | -------------------- |
| Product              | Search Dropdown | Yes      | Active products only |
| Product Code         | Auto-filled     | System   | Read-only            |
| Current Branch Stock | Auto-fetched    | System   | Reference only       |
| Assets Qty           | Number          | No       | ≥ 0                  |
| Consumable Qty       | Number          | No       | ≥ 0                  |
| Resell Qty           | Number          | No       | ≥ 0                  |
| Total Qty            | Auto-sum        | System   | Sum of three types   |
| UOM                  | Auto-filled     | System   | From Product Master  |
| Estimated Cost (₹)   | Auto-calculated | System   | Base × Total Qty     |
| Tax (₹)              | Auto-calculated | System   | From HSN code        |
| Purpose per Item     | Text            | No       | Max 200 characters   |

### Summary Section Fields

| Field                       | Type       | Required | Notes                 |
| --------------------------- | ---------- | -------- | --------------------- |
| Total Products              | Auto-count | System   | Number of line items  |
| Total Assets Requested      | Auto-sum   | System   | Sum of Assets Qty     |
| Total Consumables Requested | Auto-sum   | System   | Sum of Consumable Qty |
| Total Resell Requested      | Auto-sum   | System   | Sum of Resell Qty     |
| Total Estimated Value (₹)   | Auto-sum   | System   | Sum of all costs      |
| Notes for Approver          | Text Area  | No       | Max 500 characters    |

**Actions:**

- **[Save Draft]** — Saves without validation, no notifications sent
- **[Submit Request]** — Validates all fields, notifies Head Ops

---

## 11.4 Approval Form (Head Ops / Branch Manager)

**Description:** Review stock requests with availability validation, approval decision, and alternative sourcing options.

### Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          APPROVAL FORM                                       │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ REQUEST DETAILS (Read-only)                                             │ │
│  │                                                                         │ │
│  │  Request ID            : SR-BLR-2024-001234                            │ │
│  │  Branch                : BLR                                            │ │
│  │  Requested By          : John Doe                                       │ │
│  │  Priority              : HIGH                                           │ │
│  │  Required By           : 18 Jan, 2024                                   │ │
│  │  Purpose               : Monthly AMC services                           │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ ITEMS WITH STOCK VALIDATION                                             │ │
│  │                                                                         │ │
│  │  ┌───────────┬──────────┬──────────┬──────────┬───────────┬───────────┐│ │
│  │  │ Product   │Assets Req│Cons. Req │Resell Req│Central Avl│  Status   ││ │
│  │  │───────────┼──────────┼──────────┼──────────┼───────────┼───────────││ │
│  │  │BSP3-001   │ 5        │ 10       │ 0        │A:20,C:50  │ ✅ Avail. ││ │
│  │  │CH-3808-001│ 0        │ 20       │ 0        │A:0,C:15   │ ⚠️ Partial││ │
│  │  │RBS-001    │ 10       │ 0        │ 0        │A:5,C:100  │ ⚠️ Partial││ │
│  │  └───────────┴──────────┴──────────┴──────────┴───────────┴───────────┘│ │
│  │                                                                         │ │
│  │  ┌───────────┬───────────┬───────────┬───────────┬─────────────────────┐│ │
│  │  │ Product   │Appr.Assets│Appr.Cons. │Appr.Resell│ Alternative Source  ││ │
│  │  │───────────┼───────────┼───────────┼───────────┼─────────────────────││ │
│  │  │BSP3-001   │ [ 5 ]     │ [ 10 ]    │ [ 0 ]     │ —                   ││ │
│  │  │CH-3808-001│ [ 0 ]     │ [ 15 ]    │ [ 0 ]     │ [Transfer from HYD] ││ │
│  │  │RBS-001    │ [ 5 ]     │ [ 0 ]     │ [ 0 ]     │ —                   ││ │
│  │  └───────────┴───────────┴───────────┴───────────┴─────────────────────┘│ │
│  │                                                                         │ │
│  │  Validation Legend: ✅ Available  ⚠️ Partial  ❌ Insufficient           │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ APPROVAL DECISION                                                       │ │
│  │                                                                         │ │
│  │  Approval Type*        : [▼ Full / Partial / Reject / On Hold ▼]       │ │
│  │                                                                         │ │
│  │  IF PARTIAL:                                                            │ │
│  │  Approved Quantities   : [Editable table above]                        │ │
│  │  Alternative Source    : [▼ Other Branch / Purchase Order / None ▼]    │ │
│  │                                                                         │ │
│  │  IF BRANCH TRANSFER SELECTED:                                           │ │
│  │  ┌─────────────────────────────────────────────────────────────────┐   │ │
│  │  │ INITIATE BRANCH TRANSFER                                        │   │ │
│  │  │ Product: CH-3808-001 (Consumable)                               │   │ │
│  │  │ Required: 20 Ltr | Central: 15 Ltr | Shortage: 5 Ltr            │   │ │
│  │  │                                                                 │   │ │
│  │  │ SOURCE BRANCH SELECTION                                         │   │ │
│  │  │ ┌────────┬───────────┬─────────────┐                           │   │ │
│  │  │ │ Branch │ Available │ Can Transfer│                           │   │ │
│  │  │ ├────────┼───────────┼─────────────┤                           │   │ │
│  │  │ │ HYD    │ 25 Ltr    │ [Transfer 5]│                           │   │ │
│  │  │ │ BOM    │ 10 Ltr    │ [Transfer 5]│                           │   │ │
│  │  │ └────────┴───────────┴─────────────┘                           │   │ │
│  │  │ [Transfer from HYD] [Split: HYD+BOM]                            │   │ │
│  │  └─────────────────────────────────────────────────────────────────┘   │ │
│  │                                                                         │ │
│  │  Dispatch Date*        : [📅 Date Picker]                              │ │
│  │  Expected Delivery*    : [📅 Date Picker]                              │ │
│  │  Carrier*              : [🔍 Search Dropdown ▼]                        │ │
│  │  LR Number             : [________________]                            │ │
│  │                                                                         │ │
│  │  Asset ID Assignment   : ○ Auto  ○ Manual                              │ │
│  │  (If Assets approved)                                                   │ │
│  │                                                                         │ │
│  │  Remarks*              : [____________________________________]        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│                    [HOLD]  [REJECT]  [APPROVE]                               │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Section 1: Request Details Fields

| Field        | Type  | Required | Notes     |
| ------------ | ----- | -------- | --------- |
| Request ID   | Text  | Auto     | Read-only |
| Branch       | Text  | Auto     | Read-only |
| Requested By | Text  | Auto     | Read-only |
| Priority     | Badge | Auto     | Read-only |
| Required By  | Date  | Auto     | Read-only |
| Purpose      | Text  | Auto     | Read-only |

### Section 2: Items with Validation Fields

| Field              | Type     | Required    | Notes                        |
| ------------------ | -------- | ----------- | ---------------------------- |
| Product            | Text     | Auto        | Read-only                    |
| Assets Req         | Number   | Auto        | Read-only                    |
| Cons. Req          | Number   | Auto        | Read-only                    |
| Resell Req         | Number   | Auto        | Read-only                    |
| Central Avl        | Text     | Auto        | Available stock display      |
| Status             | Icon     | Auto        | ✅ / ⚠️ / ❌                 |
| Approve Assets     | Number   | Conditional | Editable if Partial approval |
| Approve Cons.      | Number   | Conditional | Editable if Partial approval |
| Approve Resell     | Number   | Conditional | Editable if Partial approval |
| Alternative Source | Dropdown | Conditional | If stock is insufficient     |

### Section 3: Approval Decision Fields

| Field               | Type            | Required    | Validation                                          |
| ------------------- | --------------- | ----------- | --------------------------------------------------- |
| Approval Type       | Dropdown        | Yes         | Full / Partial / Reject / On Hold / Branch Transfer |
| Approved Quantities | Table           | Conditional | Required if Partial                                 |
| Alternative Source  | Dropdown        | Conditional | Other Branch / Purchase Order / None                |
| Dispatch Date       | Date            | Yes         | Must be ≥ Today                                     |
| Expected Delivery   | Date            | Yes         | Must be ≥ Dispatch Date                             |
| Carrier             | Search Dropdown | Yes         | Must be active carrier                              |
| LR Number           | Text            | No          | Alphanumeric, max 50 chars                          |
| Asset ID Assignment | Radio           | Conditional | Auto / Manual (if Assets approved)                  |
| Remarks             | Text Area       | Yes         | Min 10 characters                                   |

---

## 11.5 Branch Transfer

**Description:** Transfer stock between branches with asset re-assignment, dispatch tracking, and receipt confirmation.

### 11.5.1 Initiate Transfer

#### Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        INITIATE BRANCH TRANSFER                              │
│                                                                              │
│  Transfer ID: TR-YYYY-SEQ (Auto-generated)                                   │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ TRANSFER DETAILS                                                        │ │
│  │                                                                         │ │
│  │  From Branch*          : [▼ Dropdown ▼] (User's accessible branches)   │ │
│  │  To Branch*            : [▼ Dropdown ▼] (≠ From Branch)                │ │
│  │  Transfer Type*        : [▼ Emergency / Regular / Scheduled ▼]         │ │
│  │  Reference Request     : [Link to originating request, if any]         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ PRODUCT SELECTION WITH TYPE SPLIT                                       │ │
│  │                                                                         │ │
│  │  [+ ADD PRODUCT]                                                        │ │
│  │                                                                         │ │
│  │  ┌───────────┬────────────────┬───────────┬───────────┬───────────┐    │ │
│  │  │ Product   │ From Branch    │ Assets    │ Cons.     │ Resell    │    │ │
│  │  │           │ Stock          │ Avl.      │ Avl.      │ Avl.      │    │ │
│  │  │───────────┼────────────────┼───────────┼───────────┼───────────│    │ │
│  │  │BSP3-001   │ 15             │ 5         │ 10        │ 0         │    │ │
│  │  │CH-3808-001│ 50             │ 0         │ 50        │ 0         │    │ │
│  │  └───────────┴────────────────┴───────────┴───────────┴───────────┘    │ │
│  │                                                                         │ │
│  │  ┌───────────┬───────────┬───────────┬───────────┬───────────────────┐ │ │
│  │  │ Product   │ Transfer  │ Transfer  │ Transfer  │ Asset Selection   │ │ │
│  │  │           │ Assets    │ Cons.     │ Resell    │ (If Assets > 0)   │ │ │
│  │  │───────────┼───────────┼───────────┼───────────┼───────────────────│ │ │
│  │  │BSP3-001   │ [ 2 ]     │ [ 5 ]     │ [ 0 ]     │ [Select Assets ▼] │ │ │
│  │  │CH-3808-001│ [ 0 ]     │ [ 20 ]    │ [ 0 ]     │ —                 │ │ │
│  │  └───────────┴───────────┴───────────┴───────────┴───────────────────┘ │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ ASSET SELECTION (If Assets > 0)                                         │ │
│  │                                                                         │ │
│  │  Product: BSP3-001 | Transferring: 2 Assets                             │ │
│  │                                                                         │ │
│  │  ┌───────────┬───────────┬─────────────────┬───────────┬──────────────┐│ │
│  │  │ Select    │ Asset ID  │ Current Assign. │ Condition │ Transfer With││ │
│  │  │───────────┼───────────┼─────────────────┼───────────┼──────────────││ │
│  │  │ [☑]       │BSP3/0156  │Employee:Ramesh  │ Good      │[Reassign ▼] ││ │
│  │  │ [☑]       │BSP3/0157  │Branch Pool      │ Good      │[Assign ▼]   ││ │
│  │  └───────────┴───────────┴─────────────────┴───────────┴──────────────┘│ │
│  │                                                                         │ │
│  │  Transfer With Options:                                                 │ │
│  │  • Reassign at destination / Assign to specific employee / Branch Pool  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│                              [NEXT: DISPATCH DETAILS]  [CANCEL]              │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### Fields

| Field             | Type            | Required    | Validation                      |
| ----------------- | --------------- | ----------- | ------------------------------- |
| Transfer ID       | Auto-generated  | System      | TR-YYYY-SEQ format              |
| From Branch       | Dropdown        | Yes         | User's accessible branches      |
| To Branch         | Dropdown        | Yes         | ≠ From Branch                   |
| Transfer Type     | Dropdown        | Yes         | Emergency / Regular / Scheduled |
| Reference Request | Link            | No          | If triggered from approval      |
| Product           | Search Dropdown | Yes         | Active products with stock      |
| Transfer Assets   | Number          | No          | ≤ Available Assets              |
| Transfer Cons.    | Number          | No          | ≤ Available Consumable          |
| Transfer Resell   | Number          | No          | ≤ Available Resell              |
| Asset Selection   | Multi-select    | Conditional | Required if Transfer Assets > 0 |

---

### 11.5.2 Dispatch (Source Branch)

#### Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           DISPATCH STOCK                                     │
│                                                                              │
│  Transfer ID: TR-2024-0056                                                   │
│  From: BLR → To: BOM                                                         │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ DISPATCH DETAILS                                                        │ │
│  │                                                                         │ │
│  │  Dispatch Date*        : [📅 Date Picker]                              │ │
│  │  Dispatched By*        : [Auto-filled: Current User]                   │ │
│  │  Carrier*              : [🔍 Search Dropdown ▼]                        │ │
│  │  LR Number*            : [________________]                            │ │
│  │  Package Count         : [________]                                    │ │
│  │  Expected Delivery     : [📅 Date Picker]                              │ │
│  │                                                                         │ │
│  │  Asset Transfer Note   : [Auto-generated PDF with Asset ID list]       │ │
│  │  [Preview] [Download]                                                   │ │
│  │                                                                         │ │
│  │  Dispatch Remarks      : [____________________________________]        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ DISPATCH CHECKLIST                                                      │ │
│  │                                                                         │ │
│  │  ☐ Items packed and labeled                                            │ │
│  │  ☐ Asset IDs verified and documented                                   │ │
│  │  ☐ Carrier documents received                                          │ │
│  │  ☐ Photos taken (optional)                                             │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│                              [CONFIRM DISPATCH]  [BACK]                      │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### Fields

| Field               | Type            | Required | Validation                   |
| ------------------- | --------------- | -------- | ---------------------------- |
| Dispatch Date       | Date            | Yes      | Must be ≤ Today              |
| Dispatched By       | Auto-filled     | System   | Current user                 |
| Carrier             | Search Dropdown | Yes      | Active carrier               |
| LR Number           | Text            | Yes      | Alphanumeric, unique, max 50 |
| Package Count       | Number          | Yes      | ≥ 1                          |
| Expected Delivery   | Date            | Yes      | ≥ Dispatch Date              |
| Asset Transfer Note | Auto-generated  | System   | PDF with Asset ID list       |
| Dispatch Remarks    | Text Area       | No       | Max 500 characters           |

**Stock Impact on Dispatch:**

- Source branch stock reduced immediately
- Assets marked "In Transit" with new location
- Consumables/Resell marked "In Transit"
- Destination branch receives "Incoming" notification

---

### 11.5.3 Receive (Destination Branch)

#### Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          RECEIVE TRANSFER                                    │
│                                                                              │
│  Transfer ID: TR-2024-0056                                                   │
│  From: BLR → To: BOM                                                         │
│  Dispatched: 17 Jan 2024 | Expected: 20 Jan 2024                             │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ RECEIPT DETAILS                                                         │ │
│  │                                                                         │ │
│  │  Received Date*        : [📅 Date Picker]                              │ │
│  │  Received By*          : [Auto-filled: Current User]                   │ │
│  │  Package Condition*    : [▼ Good / Damaged / Missing Items ▼]          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ ASSET VERIFICATION                                                      │ │
│  │                                                                         │ │
│  │  ┌───────────┬───────────┬──────────────────┬──────────────────┬──────┐│ │
│  │  │ Asset ID  │ Product   │ Expected Cond.   │ Received Cond.   │Status││ │
│  │  │───────────┼───────────┼──────────────────┼──────────────────┼──────││ │
│  │  │BSP3/0156  │BSP3-001   │ Good             │ [Good ▼]         │ ✅   ││ │
│  │  │BSP3/0157  │BSP3-001   │ Good             │ [Damaged ▼]      │ ⚠️   ││ │
│  │  └───────────┴───────────┴──────────────────┴──────────────────┴──────┘│ │
│  │  Condition Options: New / Good / Fair / Damaged / Needs Repair         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ ASSET ASSIGNMENT AT DESTINATION                                         │ │
│  │                                                                         │ │
│  │  ┌───────────┬───────────┬───────────┬────────────────────────────────┐│ │
│  │  │ Asset ID  │ Product   │ Status    │ Assign To                      ││ │
│  │  │───────────┼───────────┼───────────┼────────────────────────────────││ │
│  │  │BSP3/0156  │BSP3-001   │ ✅ Received│ [Employee ▼] / Branch Pool    ││ │
│  │  │BSP3/0157  │BSP3-001   │ ⚠️ Issue   │ [Branch Pool ▼] (Quarantine)  ││ │
│  │  └───────────┴───────────┴───────────┴────────────────────────────────┘│ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ QUANTITY VERIFICATION                                                   │ │
│  │                                                                         │ │
│  │  ┌───────────┬───────────┬───────────┬───────────┬───────────────────┐ │ │
│  │  │ Product   │ Type      │ Expected  │ Received  │ Match             │ │ │
│  │  │───────────┼───────────┼───────────┼───────────┼───────────────────│ │ │
│  │  │CH-3808-001│Consumable │ 20 Ltr    │ [ 20 ]    │ ✅ Match          │ │ │
│  │  │RBS-001    │Resell     │ 10 Nos    │ [ 10 ]    │ ✅ Match          │ │ │
│  │  └───────────┴───────────┴───────────┴───────────┴───────────────────┘ │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ RECEIPT CONFIRMATION                                                    │ │
│  │                                                                         │ │
│  │  Upload Receipt Photo* : [📎 Upload]                                   │ │
│  │  Remarks               : [____________________________________]        │ │
│  │  ☐ Confirm receipt of all items as verified*                           │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│                              [CONFIRM RECEIPT]  [REPORT ISSUE]               │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### Fields

| Field                | Type        | Required | Validation                                 |
| -------------------- | ----------- | -------- | ------------------------------------------ |
| Received Date        | Date        | Yes      | ≤ Today                                    |
| Received By          | Auto-filled | System   | Current user                               |
| Package Condition    | Dropdown    | Yes      | Good / Damaged / Missing Items             |
| Asset ID             | Auto        | System   | From dispatch                              |
| Product              | Auto        | System   | From dispatch                              |
| Expected Condition   | Auto        | System   | From dispatch                              |
| Received Condition   | Dropdown    | Yes      | New / Good / Fair / Damaged / Needs Repair |
| Status               | Auto        | System   | ✅ / ⚠️ based on condition match           |
| Assign To            | Dropdown    | Yes      | Employee / Branch Pool / Quarantine        |
| Expected (Qty)       | Auto        | System   | From dispatch                              |
| Received (Qty)       | Number      | Yes      | Must match expected or report issue        |
| Upload Receipt Photo | File Upload | Yes      | JPG/PNG, max 5MB                           |
| Remarks              | Text Area   | No       | Max 500 characters                         |
| Confirm Receipt      | Checkbox    | Yes      | Must be checked                            |

**Post-Receipt Actions:**

- Assets activated at destination branch
- New assignments recorded
- Consumables added to branch stock
- Source branch notified
- Activity log updated with both branch metadata

---

## 11.6 Product Detail View (Stock Perspective)

**Description:** Comprehensive view of product stock across all locations with individual asset tracking and complete activity logs.

### Screen Layout

┌─────────────────────────────────────────────────────────────────────────────┐
│ PRODUCT: Brass Sprayer Pump 3.5L │
│ │
│ Code: BSP3-001 | HSN: 8414 | GST: 18% [Edit] [History] │
│ │
│ [Stock Overview] [Assets List] [Activity Log] [Transfers] │
│ │
├─────────────────────────────────────────────────────────────────────────────┤
│ TAB 1: STOCK OVERVIEW │
│ │
│ ┌─────────────────────────────────────────────────────────────────────────┐ │
│ │ LOCATION WISE STOCK BREAKDOWN │ │
│ │ │ │
│ │ ┌───────────┬────────┬────────────┬────────┬────────────┬────────────┐│ │
│ │ │ Location │ Assets │Consumable │ Resell │ In-Transit │ Reserved ││ │
│ │ │ │ │ │ │ │ ││ │
│ │ │───────────┼────────┼────────────┼────────┼────────────┼────────────││ │
│ │ │ Central │ 20 │ 50 │ 0 │ 0 │ 10 ││ │
│ │ │ BLR │ 5 │ 15 │ 0 │ 2 │ 0 ││ │
│ │ │ HYD │ 8 │ 25 │ 0 │ 0 │ 5 ││ │
│ │ │ BOM │ 3 │ 10 │ 0 │ 0 │ 0 ││ │
│ │ │───────────┼────────┼────────────┼────────┼────────────┼────────────││ │
│ │ │ TOTAL │ 36 │ 100 │ 0 │ 2 │ 15 ││ │
│ │ └───────────┴────────┴────────────┴────────┴────────────┴────────────┘│ │
│ │ │ │
│ │ Total Valuation: ₹ 4,25,000 (at purchase price) │ │
│ │ │ │
│ └─────────────────────────────────────────────────────────────────────────┘ │
│ │
├─────────────────────────────────────────────────────────────────────────────┤
│ TAB 2: ASSETS LIST │
│ │
│ ┌─────────────────────────────────────────────────────────────────────────┐ │
│ │ ASSET INVENTORY │ │
│ │ │ │
│ │ Filters: Location: [▼ All ▼] Status: [▼ All ▼] Condition: [▼ All ▼] │ │
│ │ │ │
│ │ ┌───────────┬─────────────┬────────────────┬───────────┬─────────────┐│ │
│ │ │ Asset ID │ Location │ Assigned To │ Employee │ Condition ││ │
│ │ │───────────┼─────────────┼────────────────┼───────────┼─────────────││ │
│ │ │BSP3/0001 │ BLR │ Employee │ Ramesh K. │ Good ││ │
│ │ │BSP3/0002 │ BLR │ Branch Pool │ — │ Good ││ │
│ │ │BSP3/0015 │ HYD │ Employee │ Suresh M. │ Needs Repair││ │
│ │ │BSP3/0026 │ Central │ Unassigned │ — │ New ││ │
│ │ │BSP3/0156 │ In Transit │ BLR (Incoming) │ — │ Good ││ │
│ │ └───────────┴─────────────┴────────────────┴───────────┴─────────────┘│ │
│ │ │ │
│ │ ┌───────────┬───────────┬───────────┬─────────────────────────────────┐│ │
│ │ │ Status │ Since │ Actions │ [View] [Reassign] [Transfer] ││ │
│ │ │───────────┼───────────┼───────────┼─────────────────────────────────││ │
│ │ │ Active │ 15 Jan 24 │ │ [Maintenance] [Damaged] [Lost] ││ │
│ │ │ Available │ — │ │ ││ │
│ │ │ Maintenance│10 Jan 24 │ │ ││ │
│ │ │ Available │ — │ │ ││ │
│ │ │ Dispatched│ 16 Jan 24 │ │ ││ │
│ │ └───────────┴───────────┴───────────┴─────────────────────────────────┘│ │
│ │ │ │
│ │ Actions per Asset: View history | Reassign | Mark for maintenance │ │
│ │ Mark as damaged/lost | Transfer to other branch │ │
│ │ │ │
│ └─────────────────────────────────────────────────────────────────────────┘ │
│ │
├─────────────────────────────────────────────────────────────────────────────┤
│ TAB 3: ACTIVITY LOG (CONTINUED) │
│ │
│ ┌─────────────────────────────────────────────────────────────────────────┐ │
│ │ STOCK MOVEMENT HISTORY (CONTINUED) │ │
│ │ │ │
│ │ ┌───────────┬─────────────┬───────────┬─────┬──────────┬─────────────┐│ │
│ │ │ Date │ Activity │ Type │ Qty │ By │ Branch ││ │
│ │ │───────────┼─────────────┼───────────┼─────┼──────────┼─────────────││ │
│ │ │16 Jan 14:30│Stock Added │Consumable │ +50 │ Rajesh K.│ Central ││ │
│ │ │16 Jan 16:00│Asset Created│Assets │ +20 │ Rajesh K.│ Central ││ │
│ │ │17 Jan 09:00│Transfer Out │Assets │ -5 │ Rajesh K.│ Central ││ │
│ │ │17 Jan 09:00│Transfer Out │Consumable │ -10 │ Rajesh K.│ Central ││ │
│ │ │17 Jan 14:00│Transfer In │Assets │ +5 │ John D. │ BLR ││ │
│ │ │17 Jan 14:00│Transfer In │Consumable │ +10 │ John D. │ BLR ││ │
│ │ │17 Jan 15:30│Asset Assigned│Assets │ -1 │ John D. │ BLR ││ │
│ │ │18 Jan 10:00│Stock Request│Consumable │ -5 │ Ramesh K.│ BLR ││ │
│ │ │18 Jan 14:00│Asset Returned│Assets │ +1 │ John D. │ BLR ││ │
│ │ │19 Jan 09:00│Maintenance │Assets │ — │ Suresh M.│ HYD ││ │
│ │ │ │ (Mark) │ │ │ │ ││ │
│ │ └───────────┴─────────────┴───────────┴─────┴──────────┴─────────────┘│ │
│ │ │ │
│ │ Filters: Activity Type: [▼ All ▼] Date Range: [📅 From] - [📅 To] │ │
│ │ User: [🔍 Search ▼] Branch: [▼ All ▼] │ │
│ │ │ │
│ │ [Export to Excel] [Export to PDF] │ │
│ │ │ │
│ └─────────────────────────────────────────────────────────────────────────┘ │
│ │
│ Click any row to view detailed transaction information │
│ │
└─────────────────────────────────────────────────────────────────────────────┘

### **Tab 3: Activity Log (Continued)**

```
├─────────────────────────────────────────────────────────────────────────────┤
│ TAB 3: ACTIVITY LOG (CONTINUED)                                            │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ STOCK MOVEMENT HISTORY (CONTINUED)                                      │ │
│  │                                                                         │ │
│  │  ┌───────────┬─────────────┬───────────┬─────┬──────────┬─────────────┐│ │
│  │  │ Date      │ Activity    │ Type      │ Qty │ By       │ Branch      ││ │
│  │  │───────────┼─────────────┼───────────┼─────┼──────────┼─────────────││ │
│  │  │16 Jan 14:30│Stock Added  │Consumable │ +50 │ Rajesh K.│ Central     ││ │
│  │  │16 Jan 16:00│Asset Created│Assets     │ +20 │ Rajesh K.│ Central     ││ │
│  │  │17 Jan 09:00│Transfer Out │Assets     │ -5  │ Rajesh K.│ Central     ││ │
│  │  │17 Jan 09:00│Transfer Out │Consumable │ -10 │ Rajesh K.│ Central     ││ │
│  │  │17 Jan 14:00│Transfer In  │Assets     │ +5  │ John D.  │ BLR         ││ │
│  │  │17 Jan 14:00│Transfer In  │Consumable │ +10 │ John D.  │ BLR         ││ │
│  │  │17 Jan 15:30│Asset Assigned│Assets     │ -1  │ John D.  │ BLR         ││ │
│  │  │18 Jan 10:00│Stock Request│Consumable │ -5  │ Ramesh K.│ BLR         ││ │
│  │  │18 Jan 14:00│Asset Returned│Assets    │ +1  │ John D.  │ BLR         ││ │
│  │  │19 Jan 09:00│Maintenance  │Assets     │ —   │ Suresh M.│ HYD         ││ │
│  │  │           │ (Mark)      │           │     │          │             ││ │
│  │  └───────────┴─────────────┴───────────┴─────┴──────────┴─────────────┘│ │
│  │                                                                         │ │
│  │  Filters: Activity Type: [▼ All ▼]  Date Range: [📅 From] - [📅 To]    │ │
│  │           User: [🔍 Search ▼]  Branch: [▼ All ▼]                       │ │
│  │                                                                         │ │
│  │  [Export to Excel]  [Export to PDF]                                      │ │
│  │                                                                         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  Click any row to view detailed transaction information                      │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### **Activity Log Fields**

| Field     | Type     | Description                                                                                                                               |
| --------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Date      | DateTime | Timestamp of activity                                                                                                                     |
| Activity  | Text     | Stock Added / Asset Created / Transfer Out / Transfer In / Asset Assigned / Stock Request / Asset Returned / Maintenance / Damaged / Lost |
| Type      | Badge    | Assets / Consumable / Resell                                                                                                              |
| Qty       | Number   | Positive for in, negative for out, — for no quantity change                                                                               |
| By        | Text     | User who performed action                                                                                                                 |
| Branch    | Text     | Location where activity occurred                                                                                                          |
| Reference | Link     | PO number, Transfer ID, Request ID, etc.                                                                                                  |
| Notes     | Text     | Additional details                                                                                                                        |

---

### **Tab 4: Transfer History**

```
├─────────────────────────────────────────────────────────────────────────────┤
│ TAB 4: TRANSFER HISTORY                                                    │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ BRANCH TRANSFER LOG                                                     │ │
│  │                                                                         │ │
│  │  Filters: From: [▼ All ▼]  To: [▼ All ▼]  Status: [▼ All ▼]          │ │
│  │           Date Range: [📅 From] - [📅 To]                                │ │
│  │                                                                         │ │
│  │  ┌───────────┬─────────────┬─────────────┬────────┬────────┬─────────┐│ │
│  │  │ Date      │ Transfer ID │ Type        │ From   │ To     │ Assets  ││ │
│  │  │───────────┼─────────────┼─────────────┼────────┼────────┼─────────││ │
│  │  │17 Jan 2024│TR-2024-0056 │Branch Trans.│Central │ BLR    │ 5       ││ │
│  │  │15 Jan 2024│TR-2024-0042 │Branch Trans.│HYD     │ BOM    │ 2       ││ │
│  │  │12 Jan 2024│TR-2024-0038 │Emergency    │BLR     │ HYD    │ 0       ││ │
│  │  └───────────┴─────────────┴─────────────┴────────┴────────┴─────────┘│ │
│  │                                                                         │ │
│  │  ┌───────────┬───────────┬───────────┬─────────────────────────────────┐│ │
│  │  │ Cons. Qty │ Resell Qty│ Status    │ By                              ││ │
│  │  │───────────┼───────────┼───────────┼─────────────────────────────────││ │
│  │  │ 10        │ 0         │Completed  │Rajesh K. → John D.              ││ │
│  │  │ 5         │ 0         │In Transit │Suresh M. → Pending              ││ │
│  │  │ 20        │ 10        │Completed  │Amit V. → Ramesh K.              ││ │
│  │  └───────────┴───────────┴───────────┴─────────────────────────────────┘│ │
│  │                                                                         │ │
│  │  Status Legend: ● Completed  ◐ In Transit  ○ Draft  ✕ Cancelled      │ │
│  │                                                                         │ │
│  │  [View Details] [Download Report]                                        │ │
│  │                                                                         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### **Transfer History Fields**

| Field       | Type   | Description                                |
| ----------- | ------ | ------------------------------------------ |
| Date        | Date   | Transfer date                              |
| Transfer ID | Link   | Click to view full details                 |
| Type        | Badge  | Branch Transfer / Emergency / Scheduled    |
| From        | Text   | Source branch                              |
| To          | Text   | Destination branch                         |
| Assets Qty  | Number | Assets transferred                         |
| Cons. Qty   | Number | Consumables transferred                    |
| Resell Qty  | Number | Resell items transferred                   |
| Status      | Badge  | Completed / In Transit / Draft / Cancelled |
| By          | Text   | Dispatched by → Received by                |

---

## 11.7 Asset Management Views

### **11.7.1 Individual Asset Detail View**

**Description:** Complete lifecycle view of a single asset with assignment history, maintenance records, and current status.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      ASSET DETAIL: BSP3/0156                                 │
│                                                                              │
│  Product: Brass Sprayer Pump 3.5L (BSP3-001)          [Edit] [History] [Print]│
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ CURRENT STATUS                                                          │ │
│  │                                                                         │ │
│  │  Asset ID          : BSP3/0156                                           │ │
│  │  Status            : ● Active (Assigned to Employee)                    │ │
│  │  Condition         : Good                                                │ │
│  │  Location          : BLR - HSR Layout Warehouse                          │ │
│  │  Assigned To       : Ramesh Kumar (EMP-0123)                            │ │
│  │  Assignment Date   : 17 Jan 2024                                        │ │
│  │  Assignment Type   : Direct Assignment                                  │ │
│  │                                                                         │ │
│  │  Purchase Info     : PO-2024-0089 | Invoice: INV-78456 | Date: 15 Jan 2024│ │
│  │  Warranty Status   : ● Under Warranty (Expires: 15 Jan 2025)            │ │
│  │                                                                         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  [Assignment History] [Maintenance Log] [Transfer History] [Documents]       │
│                                                                              │
├─────────────────────────────────────────────────────────────────────────────┤
│ TAB 1: ASSIGNMENT HISTORY                                                    │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │                                                                         │ │
│  │  ┌───────────┬─────────────┬─────────────┬─────────────┬────────────┐│ │
│  │  │ Date      │ Action      │ From        │ To          │ By         ││ │
│  │  │───────────┼─────────────┼─────────────┼─────────────┼────────────││ │
│  │  │15 Jan 2024│Created      │Central      │Central      │Rajesh K.   ││ │
│  │  │           │             │(Unassigned) │(Unassigned) │            ││ │
│  │  │17 Jan 2024│Transferred  │Central      │BLR          │Rajesh K.   ││ │
│  │  │17 Jan 2024│Assigned     │BLR Pool     │Ramesh Kumar │John D.     ││ │
│  │  │           │             │             │(EMP-0123)   │(Manager)   ││ │
│  │  └───────────┴─────────────┴─────────────┴─────────────┴────────────┘│ │
│  │                                                                         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
├─────────────────────────────────────────────────────────────────────────────┤
│ TAB 2: MAINTENANCE LOG                                                       │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │                                                                         │ │
│  │  [+ LOG MAINTENANCE]                                                    │ │
│  │                                                                         │ │
│  │  ┌───────────┬─────────────┬─────────────┬─────────────┬────────────┐│ │
│  │  │ Date      │ Type        │ Description │ Cost (₹)    │ Status     ││ │
│  │  │───────────┼─────────────┼─────────────┼─────────────┼────────────││ │
│  │  │20 Feb 2024│Scheduled    │Regular      │ 500         │Completed   ││ │
│  │  │           │Service      │servicing    │             │            ││ │
│  │  │15 Mar 2024│Repair       │Nozzle       │ 1,200       │Completed   ││ │
│  │  │           │             │replacement  │             │            ││ │
│  │  │10 Apr 2024│Inspection   │Quarterly    │ 0           │Scheduled   ││ │
│  │  │           │             │check        │             │            ││ │
│  │  └───────────┴─────────────┴─────────────┴─────────────┴────────────┘│ │
│  │                                                                         │ │
│  │  Next Scheduled: 10 Apr 2024                                               │ │
│  │                                                                         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│                              [BACK TO PRODUCT]                               │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### **Asset Detail Fields**

| Field           | Type  | Description                                                  |
| --------------- | ----- | ------------------------------------------------------------ |
| Asset ID        | Text  | Unique identifier                                            |
| Status          | Badge | Active / In Transit / Maintenance / Damaged / Lost / Retired |
| Condition       | Badge | New / Good / Fair / Needs Repair / Damaged                   |
| Location        | Text  | Current branch/warehouse                                     |
| Assigned To     | Text  | Employee name and ID                                         |
| Assignment Date | Date  | When assigned                                                |
| Assignment Type | Text  | Direct / Branch Pool / Transfer                              |
| Purchase Info   | Text  | PO and Invoice references                                    |
| Warranty Status | Badge | Under Warranty / Expired / No Warranty                       |

---

## 11.8 Stock Reports & Analytics

### **11.8.1 Stock Summary Report**

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        STOCK SUMMARY REPORT                                  │
│                                                                              │
│  Period: [📅 From] - [📅 To]              [Generate] [Export]               │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ EXECUTIVE SUMMARY                                                       │ │
│  │                                                                         │ │
│  │  Total SKUs: 156    Total Assets: 342    Total Consumables: 12,450 Ltr   │ │
│  │  Total Value: ₹ 24,56,780    In-Transit: ₹ 3,45,200                      │ │
│  │                                                                         │ │
│  │  Low Stock Alerts: 12 items    Out of Stock: 3 items                     │ │
│  │                                                                         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ BRANCH WISE BREAKDOWN                                                   │ │
│  │                                                                         │ │
│  │  ┌───────────┬──────┬───────────┬────────────┬───────────┬───────────┐│ │
│  │  │ Branch    │Assets│Consumables│ Resell     │ In-Transit│ Valuation ││ │
│  │  │           │      │           │            │           │ (₹)       ││ │
│  │  │───────────┼──────┼───────────┼────────────┼───────────┼───────────││ │
│  │  │ Central   │ 120  │ 5,000 Ltr │ 0          │ 0         │ 8,45,000  ││ │
│  │  │ BLR       │ 85   │ 3,200 Ltr │ 150        │ 25        │ 6,32,400  ││ │
│  │  │ HYD       │ 72   │ 2,800 Ltr │ 200        │ 18        │ 5,48,600  ││ │
│  │  │ BOM       │ 65   │ 1,450 Ltr │ 100        │ 12        │ 4,30,780  ││ │
│  │  └───────────┴──────┴───────────┴────────────┴───────────┴───────────┘│ │
│  │                                                                         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ CATEGORY WISE DISTRIBUTION                                              │ │
│  │                                                                         │ │
│  │  Chemicals: 45%    Sprayers: 25%    Machines: 15%    Traps: 10%    Other: 5%│ │
│  │                                                                         │ │
│  │  [View Detailed Breakdown]                                               │ │
│  │                                                                         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 11.9 RBAC Summary Matrix

| Feature                | Head Ops            | Branch Manager             | Branch User     | Warehouse Staff      |
| ---------------------- | ------------------- | -------------------------- | --------------- | -------------------- |
| **Stock Dashboard**    |
| View All Branch Stocks | ✅                  | Own branches only          | Own branch only | Assigned warehouse   |
| View Central Stock     | ✅                  | ❌                         | ❌              | ❌                   |
| View In-Transit        | ✅                  | Own branches               | Own branch      | Assigned transfers   |
| **Add Stock**          |
| Add to Central         | ✅                  | ❌                         | ❌              | ❌                   |
| Receive & Allocate     | ✅ (Any)            | ✅ (Their branch)          | ❌              | ✅ (Their warehouse) |
| **Stock Requests**     |
| Create Request         | ✅ (Central→Branch) | ✅                         | ✅              | ❌                   |
| Approve Requests       | ✅ (All)            | ✅ (Their branch incoming) | ❌              | ❌                   |
| **Transfers**          |
| Initiate Transfer      | ✅ (Any→Any)        | ✅ (Their branch→Other)    | ❌              | ❌                   |
| Dispatch               | ✅                  | ✅ (Their branch)          | ❌              | ✅                   |
| Receive                | ✅                  | ✅ (Their branch)          | ❌              | ✅                   |
| **Asset Management**   |
| Create Asset IDs       | ✅                  | ✅ (on receipt)            | ❌              | ❌                   |
| Assign to Employees    | ✅                  | ✅                         | ❌              | ❌                   |
| View Asset List        | ✅ All              | Own branches               | Own branch      | Assigned assets      |
| Transfer Assets        | ✅                  | ✅                         | ❌              | ❌                   |
| Mark Maintenance       | ✅                  | ✅                         | Own assigned    | ❌                   |
| Mark Damaged/Lost      | ✅                  | ✅                         | Own assigned    | ❌                   |
| **Reports & Logs**     |
| View Activity Logs     | ✅ All              | Own branches               | Own branch      | Own warehouse        |
| Export Reports         | ✅ All              | Own branches               | ❌              | ❌                   |
| View Valuation         | ✅                  | ✅ (Their branches)        | ❌              | ❌                   |

---

## 📊 COMPLETE WORKFLOW FLOWCHART

### **Module 11: Stock Management - System Workflow**

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    MODULE 11: STOCK MANAGEMENT WORKFLOW                        │
│                                                                              │
│  ENTRY POINTS:                                                               │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐             │
│  │ Head Ops Login  │  │ Branch Manager  │  │ Branch User     │             │
│  │ (Module 1.2)    │  │ (Module 1.6)    │  │ (Module 1.6)    │             │
│  └────────┬────────┘  └────────┬────────┘  └────────┬────────┘             │
│           │                    │                    │                        │
│           ▼                    ▼                    ▼                        │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │                    STOCK DASHBOARD (11.1)                               │ │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐                     │ │
│  │  │ Tab 1: All  │  │ Tab 2: My   │  │ Tab 3: Appr.  │                     │ │
│  │  │ Products    │  │ Requests    │  │ & Receipts  │                     │ │
│  │  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘                     │ │
│  │         │                │                │                              │ │
│  │         ▼                ▼                ▼                              │ │
│  │  [View Stock]      [Track Requests]    [Approve/Receive]                 │ │
│  │  [Request Stock]   [Create Request]                                      │ │
│  │  [Transfer Stock]  [Cancel Request]                                        │ │
│  │  [View History]                                                          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
├─────────────────────────────────────────────────────────────────────────────┤
│  WORKFLOW 1: ADD STOCK TO CENTRAL (Head Ops)                                 │
│                                                                              │
│  11.2 Add Stock ──► Select "Add to Central" ──► 11.2.1 Add to Central      │
│       │                                           Stock Form                │
│       │                                                  │                   │
│       │              ┌─────────────────────────────────────┘                   │
│       │              ▼                                                      │
│       │    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐ │
│       │    │ 1. Select       │───►│ 2. Allocate     │───►│ 3. Asset Details │ │
│       │    │    Product      │    │    Stock Types  │    │ (If Assets > 0) │ │
│       │    │    (From Module │    │    (Assets/     │    │    Generate IDs │ │
│       │    │     10)         │    │    Consumable/   │    │    Set Assignment│ │
│       │    └─────────────────┘    │    Resell)      │    └────────┬────────┘ │
│       │                           └─────────────────┘             │          │
│       │                                                           ▼          │
│       │    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐ │
│       └───►│ 4. Purchase &   │───►│ 5. Initial      │───►│ [SAVE] ──►      │ │
│            │    Tax Details  │    │    Allocation   │    │ Stock Added     │ │
│            │    (Uses Module │    │    (Optional    │    │ Assets Created  │ │
│            │     9 for tax)  │    │    Branch       │    │ Notifications   │ │
│            └─────────────────┘    │    Transfer)    │    │ Activity Log    │ │
│                                   └─────────────────┘    └─────────────────┘ │
│                                                                              │
├─────────────────────────────────────────────────────────────────────────────┤
│  WORKFLOW 2: BRANCH REQUESTS STOCK                                           │
│                                                                              │
│  Branch User/Manager ──► 11.3 Stock Request ──► Create Request              │
│       │                                          │                          │
│       │         ┌────────────────────────────────┘                          │
│       │         ▼                                                             │
│       │    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐ │
│       │    │ 1. Fill Header  │───►│ 2. Add Items    │───►│ 3. Review &     │ │
│       │    │    (Priority,   │    │    (Products,   │    │    Submit       │ │
│       │    │     Required By)│    │    Type Split,  │    │                 │ │
│       │    └─────────────────┘    │    Quantities)  │    └────────┬────────┘ │
│       │                           └─────────────────┘             │          │
│       │                                                             ▼          │
│       │                                                    ┌─────────────────┐ │
│       │                                                    │ Request Sent to │ │
│       │                                                    │ Head Ops        │ │
│       │                                                    │ (Status: Pending)│ │
│       │                                                    └────────┬────────┘ │
│       │                                                             │          │
│       │    ┌────────────────────────────────────────────────────────┘          │
│       │    ▼                                                                 │
│       └─► Head Ops ──► 11.4 Approval Form ──► Review & Validate Stock       │
│                              Availability                                   │
│                                   │                                          │
│                    ┌──────────────┼──────────────┐                           │
│                    ▼              ▼              ▼                           │
│              ┌─────────┐   ┌─────────┐   ┌─────────┐                       │
│              │ APPROVE │   │ PARTIAL │   │ REJECT  │                       │
│              │  (Full) │   │(Approve  │   │         │                       │
│              └────┬────┘   │ partial, │   └────┬────┘                       │
│                   │        │ transfer │        │                           │
│                   │        │  rest)   │        │                           │
│                   │        └────┬────┘        │                           │
│                   │             │             │                           │
│                   └─────────────┴───────────────┘                           │
│                                 │                                          │
│                                 ▼                                          │
│                    ┌─────────────────────────┐                              │
│                    │ Generate Transfer/      │                              │
│                    │ Dispatch Order          │                              │
│                    │ (11.5.2 Dispatch)       │                              │
│                    └────────────┬────────────┘                              │
│                                 │                                          │
│                                 ▼                                          │
│                    ┌─────────────────────────┐                              │
│                    │ Branch Receives           │                              │
│                    │ (11.2.2 or 11.5.3)        │                              │
│                    │ • Verify quantities       │                              │
│                    │ • Allocate stock types    │                              │
│                    │ • Assign assets           │                              │
│                    │ • Confirm receipt         │                              │
│                    └─────────────────────────┘                              │
│                                                                              │
├─────────────────────────────────────────────────────────────────────────────┤
│  WORKFLOW 3: BRANCH TRANSFER (Emergency/Planned)                             │
│                                                                              │
│  Head Ops/Branch Manager ──► 11.5 Branch Transfer ──► 11.5.1 Initiate    │
│       │                                                    │                 │
│       │         ┌──────────────────────────────────────────┘                 │
│       │         ▼                                                            │
│       │    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐│
│       │    │ 1. Select From/  │───►│ 2. Select       │───►│ 3. Select       ││
│       │    │    To Branches   │    │    Products &   │    │    Assets (if   ││
│       │    │    Transfer Type │   │    Quantities   │    │    applicable)  ││
│       │    └─────────────────┘    └─────────────────┘    └────────┬────────┘│
│       │                                                            │         │
│       │    ┌────────────────────────────────────────────────────────┘         │
│       │    ▼                                                                │
│       └───► 11.5.2 Dispatch ──► Source Branch Dispatches                     │
│                              • Reduce stock immediately                      │
│                              • Mark assets "In Transit"                      │
│                              • Generate LR/Tracking                          │
│                                   │                                          │
│                                   ▼                                          │
│                         11.5.3 Receive ──► Destination Receives              │
│                              • Verify condition                              │
│                              • Confirm quantities                            │
│                              • Assign assets                                 │
│                              • Update stock                                    │
│                                   │                                          │
│                                   ▼                                          │
│                         ┌─────────────────┐                                  │
│                         │ Transfer Complete │                                │
│                         │ Activity Logged   │                                │
│                         │ Both Notified     │                                │
│                         └─────────────────┘                                  │
│                                                                              │
├─────────────────────────────────────────────────────────────────────────────┤
│  WORKFLOW 4: ASSET LIFECYCLE MANAGEMENT                                      │
│                                                                              │
│  Asset Created (11.2.1) ──► Central Pool                                      │
│       │                                                                       │
│       ▼                                                                       │
│  Transfer to Branch (11.5) ──► Branch Pool                                    │
│       │                                                                       │
│       ▼                                                                       │
│  Assign to Employee ──► Active Use ──► [Reassign] [Transfer] [Maintenance]  │
│       │                              [Mark Damaged] [Mark Lost] [Return]     │
│       │                                                                       │
│       ▼                                                                       │
│  Asset Returned ──► Branch Pool ──► [Reassign] [Transfer to Other Branch]     │
│       │                                                                       │
│       ▼                                                                       │
│  Mark Damaged ──► Repair/Retire ──► [Repair] [Retire] [Dispose]               │
│       │                                                                       │
│       ▼                                                                       │
│  Asset Retired ──► Archive Record                                             │
│                                                                              │
│  All states viewable in: 11.6 Product Detail ──► Tab 2: Assets List          │
│                         11.7.1 Individual Asset Detail View                   │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 📋 MODULE 11 NAVIGATION STRUCTURE

```
MODULE 11: STOCK MANAGEMENT
│
├── 11.1 Stock Dashboard
│   ├── Tab 1: All Products (Stock overview with type breakdown)
│   ├── Tab 2: My Requests (Track requests & transfers)
│   └── Tab 3: Approvals & Receipts (Action pending items)
│
├── 11.2 Add Stock (Multi-mode)
│   ├── 11.2.1 Add to Central Stock (Head Ops)
│   └── 11.2.2 Receive & Allocate (Branch Manager)
│
├── 11.3 Stock Request (Branch → Central)
│
├── 11.4 Approval Form (Review & decision)
│
├── 11.5 Branch Transfer
│   ├── 11.5.1 Initiate Transfer
│   ├── 11.5.2 Dispatch (Source)
│   └── 11.5.3 Receive (Destination)
│
├── 11.6 Product Detail View
│   ├── Tab 1: Stock Overview (Branch-wise)
│   ├── Tab 2: Assets List (Individual tracking)
│   ├── Tab 3: Activity Log (All movements)
│   └── Tab 4: Transfer History
│
├── 11.7 Asset Management
│   └── 11.7.1 Individual Asset Detail View
│
└── 11.8 Stock Reports & Analytics
    └── 11.8.1 Stock Summary Report
```

---

## 🔗 MODULE INTEGRATION MAP

| Module                  | Connection Type | Description                                   |
| ----------------------- | --------------- | --------------------------------------------- |
| **Module 9 (Tax)**      | Prerequisite    | Tax rates auto-populate in purchase forms     |
| **Module 10 (Product)** | Prerequisite    | Product Master provides SKUs, HSN codes, UOMs |
| **Module 7 (Branch)**   | Used By         | Branch list for transfers and allocations     |
| **Module 8 (Employee)** | Used By         | Employee list for asset assignments           |
| **Module 5 (Roles)**    | Used By         | RBAC controls access to stock functions       |
| **Billing Module**      | Consumer        | Resell stock used for invoicing               |
| **Service Module**      | Consumer        | Consumable stock used for service delivery    |
