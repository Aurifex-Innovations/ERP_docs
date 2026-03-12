# 🎯 MODULE 11: STOCK MANAGEMENT

## Overview

Comprehensive stock management system that handles inventory operations across multiple branches with centralized control. Manages three distinct stock types — **Assets** (individually tracked with Asset IDs), **Consumables** (bulk quantity management), and **Resell** (inventory for sale). Supports branch-wise stock distribution with Head Office as the central hub, stock requests and transfers between branches, and complete audit trails for all inventory movements.

**Module Connections:**

- **Depends on:** Module 9 (Tax Management for purchase calculations), Module 10 (Product Master for stock items)
- **Used by:** Module 8 (Employee assignment for Assets), Service Operations (Consumable usage), Billing (Resell items)
- **Prerequisites:** Module 9 and Module 10 must be configured before activating stock operations

---

-The screen contains three tabs:

->View Entries

->My Requests

->Received Requests

Each tab provides a different view for managing stock and request workflows.

# 11.1 Tab 1:Stock Dashboard – Table View

**Description:**
A unified stock view displaying product-wise quantities across accessible branches with category and stock-type filtering. The dashboard provides real-time visibility into total stock levels, in-transit items, reserved quantities, and asset assignments.

---

# Screen Layout

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
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  STOCK OVERVIEW TABLE                                                        │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │Img│Product Code│Product Name│Category│Brand│HSN│Assets│Cons.│Resell│Tot │ │
│  │   │            │            │        │     │   │Qty   │Qty  │Qty   │Stock│ │
│  │───┼────────────┼────────────┼────────┼─────┼───┼──────┼─────┼──────┼─────│ │
│  │📷 │BSP3-001    │Brass Sprayr│Sprayer │Agro │842│ 5    │ 15  │ 0    │ 20 │ │
│  │📷 │CH-3808-001 │Chemical X  │Chemical│ABC  │380│ 0    │ 50  │ 20   │ 70 │ │
│  │📷 │RBS-001     │Rodent Box  │Trap    │Pest │732│ 10   │ 25  │ 0    │ 35 │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │In-Transit│Reserved│Base UOM│Status│Actions                               ││
│  │Qty       │Qty     │        │      │                                      ││
│  │──────────┼────────┼────────┼──────┼──────────────────────────────────────││
│  │2         │5       │Nos     │🟢    │[View] [Edit] [Delete]                 ││
│  │0         │10      │Ltr     │🟡    │[View] [Edit] [Delete]                 ││
│  │5         │0       │Nos     │🟢    │[View] [Edit] [Delete]                 ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   10   Next                         │
│                                                                              │
│  Legend: 🟢 Available  🟡 Low Stock  🔴 Out of Stock  📦 In-Transit  ⏳ Reserved│
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# Table View Fields

| Field              | Type            | Required | Description                                            |
| ------------------ | --------------- | -------- | ------------------------------------------------------ |
| Cover Image        | Image Thumbnail | Auto     | Product photo from Product Master                      |
| Product Code       | Text            | Auto     | Unique SKU from Product Master                         |
| Product Name       | Text            | Auto     | Full product name                                      |
| Category           | Text            | Auto     | Chemical / Sprayer / Machine / Trap / Tool / Other     |
| Company / Brand    | Text            | Auto     | Manufacturer or brand name                             |
| HSN Code           | Link            | Auto     | Clickable link to view tax details                     |
| **Assets Qty**     | Number          | Auto     | Count of individually trackable asset units            |
| **Consumable Qty** | Number          | Auto     | Quantity of consumable stock                           |
| **Resell Qty**     | Number          | Auto     | Quantity available for resale                          |
| **Total Stock**    | Number          | Auto     | Sum of Assets + Consumable + Resell                    |
| **In-Transit Qty** | Number          | Auto     | Stock dispatched but not yet received                  |
| **Reserved Qty**   | Number          | Auto     | Quantity reserved for pending requests                 |
| Base UOM           | Text            | Auto     | Unit of measure from Product Master                    |
| Status             | Badge           | Auto     | Available,Low Stock,Out of Stock,Inactive,Discontinued |
| Actions            | Button Group    | —        | View / Edit / Delete                                   |

---

# Actions

| Action | Description                                    |
| ------ | ---------------------------------------------- |
| View   | Open detailed product stock view               |
| Edit   | Modify stock details or allocations            |
| Delete | Remove stock entry (based on permission rules) |

---

## Actions (Table Row)

| Action     | Type   | Description                                          |
| ---------- | ------ | ---------------------------------------------------- |
| **View**   | Button | Opens stock details in read-only mode.               |
| **Edit**   | Button | Allows authorized users to modify stock information. |
| **Delete** | Button | Removes the stock record (permission based).         |

---

## Form Actions

These actions open separate forms.
**Detailed form structure will be defined in the upcoming modules.**

| Action            | Description                                                                                 |
| ----------------- | ------------------------------------------------------------------------------------------- |
| **Add Stock**     | Opens the **Add Stock Form** to add inventory for a selected product.                       |
| **Request Stock** | Opens the **Stock Request Form** to request stock from another branch or central warehouse. |

---

# Filters

| Filter       | Type         | Options                                            |
| ------------ | ------------ | -------------------------------------------------- |
| Branch View  | Dropdown     | All My Branches / Central / BLR / HYD / BOM / etc. |
| Category     | Multi-select | Chemical / Sprayer / Machine / Trap / Tool / Other |
| Stock Type   | Multi-select | Assets / Consumable / Resell                       |
| Status       | Multi-select | Available / Low / Out / Inactive,Discontinued      |
| Created Date | Date Range   | From – To                                          |

---

# Search

Searchable by:

- Product Code
- Product Name
- HSN Code
- Asset ID _(if asset tracking is enabled)_

---

# 11.1.1 Tab 2: My Requests

**Description:**
Track all stock and transfer requests created by the user or related to the user's branch. This tab provides visibility into the request lifecycle from creation to final receipt.

---

# Screen Layout

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│                           STOCK DASHBOARD                                    │
│                                                                              │
│  [Tab 1: All Products]  [Tab 2: My Requests]  [Tab 3: Received Requests]     │
│                                                                              │
│  Status Filter: [All] [Pending] [Approved] [Rejected] [Dispatch]             │
│                 [In Transit] [Received]                                      │
│                                                                              │
│  MY REQUESTS TABLE                                                           │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │Request ID │Type │Direction│From│To │Items│Total│Assets│Status │Priority│ │
│  │───────────┼─────┼─────────┼────┼───┼─────┼─────┼──────┼───────┼────────│ │
│  │SR-BLR-001 │Stock│Inward   │CEN │BLR│ 3   │ 25  │ 5    │Pending│High    │ │
│  │TR-2024-056│Tran │Inward   │HYD │BLR│ 2   │ 15  │ 3    │InTran │Normal  │ │
│  │TR-2024-042│Tran │Outward  │BLR │BOM│ 1   │ 10  │ 0    │Approved│Normal │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Requested By │Requested Date & Time │Action                               ││
│  │─────────────┼──────────────────────┼────────────────────────────────────││
│  │John Doe     │15 Jan 2024 10:45 AM  │[View]                               ││
│  │Jane Smith   │16 Jan 2024 02:30 PM  │[View]                               ││
│  │John Doe     │17 Jan 2024 09:15 AM  │[View]                               ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination: Previous 1 2 3 ... 10 Next                                      │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# Table Fields

| Field                 | Type     | Description                                                      |
| --------------------- | -------- | ---------------------------------------------------------------- |
| Request ID            | Text     | Unique request identifier (SR-XXXX / TR-XXXX format)             |
| Request Type          | Badge    | Stock Request / Transfer Request / Asset Assignment              |
| Direction             | Icon     | Indicates Inward or Outward request                              |
| From                  | Text     | Source branch or warehouse                                       |
| To                    | Text     | Destination branch                                               |
| Items Count           | Number   | Number of product lines in the request                           |
| Total Qty             | Number   | Total quantity requested                                         |
| Assets Count          | Number   | Count of asset items (if applicable)                             |
| Status                | Badge    | Pending / Approved / Rejected / Dispatch / In Transit / Received |
| Priority              | Badge    | Low / Normal / High / Urgent                                     |
| Requested By          | Text     | User+Role who created the request                                |
| Requested Date & Time | DateTime | Request creation timestamp                                       |
| Action                | Button   | View request details                                             |

---

# Request Types

| Type             | Direction | Description                                         |
| ---------------- | --------- | --------------------------------------------------- |
| Stock Request    | Inward    | Request stock from Head Office or central warehouse |
| Transfer Request | Inward    | Stock transferred from another branch               |
| Transfer Request | Outward   | Stock transferred to another branch                 |
| Asset Assignment | Inward    | Assign asset from stock to employee                 |
| Asset Return     | Outward   | Employee returns asset back to stock                |

---

# Status Options

| Status     | Description                              |
| ---------- | ---------------------------------------- |
| Pending    | Request created and awaiting approval    |
| Approved   | Request approved for processing          |
| Rejected   | Request rejected by approver             |
| Dispatch   | Stock prepared and dispatched            |
| In Transit | Stock currently moving between locations |
| Received   | Stock received and confirmed             |

---

# Filters

| Filter       | Type         | Options                                             |
| ------------ | ------------ | --------------------------------------------------- |
| Request Type | Multi Select | Stock Request / Transfer Request / Asset Assignment |
| Direction    | Multi Select | Inward / Outward                                    |
| Priority     | Multi Select | Low / Normal / High / Urgent                        |
| Date Range   | Date Range   | From – To                                           |

---

# Search

Searchable by:

- Request ID
- Product Name
- Source Branch
- Destination Branch

---

# 11.1.2 Tab 3: Received Requests

**Description:**
For **Branch Managers and Head Operations** to review incoming requests, approve stock transfers, and confirm stock receipt. This tab provides validation indicators for stock availability before approval or receipt confirmation.

---

# Screen Layout

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│                           STOCK DASHBOARD                                    │
│                                                                              │
│  [Tab 1: All Products]  [Tab 2: My Requests]  [Tab 3: Received Requests]     │
│                                                                              │
│  Segmented Control: [Pending Approval] [Pending Receipt] [Completed Today]   │
│                     [All History]                                            │
│                                                                              │
│  RECEIVED REQUESTS TABLE                                                     │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │Request ID │Type│From│To │Items│Total Value│Requested By│Validation│Action│ │
│  │───────────┼────┼────┼───┼─────┼───────────┼────────────┼──────────┼──────│ │
│  │SR-BLR-001 │Appr│BLR │CEN│ 3   │ ₹45,000   │John Doe    │✅ Avail. │View  │ │
│  │SR-HYD-002 │Appr│HYD │CEN│ 2   │ ₹28,500   │Jane Smith  │⚠️ Partial│View  │ │
│  │TR-2024-056│Rcpt│CEN │BLR│ 2   │ ₹32,000   │Rajesh K.   │⏳ Pending│View  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Request Date & Time│Expected Date│Reviewed By│Review Date & Time│Status   ││
│  │───────────────────┼─────────────┼───────────┼──────────────────┼────────││
│  │15 Jan 2024 10:30  │18 Jan 2024  │—          │—                 │Pending ││
│  │16 Jan 2024 11:15  │19 Jan 2024  │—          │—                 │Pending ││
│  │16 Jan 2024 09:50  │20 Jan 2024  │—          │—                 │InTran  ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination: Previous 1 2 3 ... 10 Next                                      │
│                                                                              │
│  Validation Legend: ✅ Available  ⚠️ Partial  ❌ Insufficient  ⏳ Pending     │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# Table Fields

| Field                 | Type     | Description                                                      |
| --------------------- | -------- | ---------------------------------------------------------------- |
| Request ID            | Text     | Unique identifier (SR-XXXX / TR-XXXX format)                     |
| Request Type          | Badge    | Stock Request / Transfer Request / Asset Assignment              |
| From                  | Text     | Requesting branch or location                                    |
| To                    | Text     | Receiving branch or central warehouse                            |
| Items                 | Number   | Number of products included in the request                       |
| Total Value (₹)       | Currency | Total value of requested items including tax                     |
| Requested By          | Text     | User and role who created the request                            |
| Valid stock           | Icon     | ✅ Available / ⚠️ Partial / ❌ Insufficient / ⏳ Pending         |
| Requested Date & Time | DateTime | Original request creation timestamp                              |
| Expected Date         | Date     | Expected delivery or receipt date                                |
| Reviewed By           | Text     | Name of approving authority                                      |
| Review Date & Time    | DateTime | Timestamp when the request was reviewed                          |
| Status                | Badge    | Pending / Approved / Rejected / Dispatch / In Transit / Received |
| Action                | Button   | View / Edit (permission based)                                   |

---

# Filters

| Filter       | Type         | Options                                     |
| ------------ | ------------ | ------------------------------------------- |
| Request Type | Multi Select | Request for Approval / Receipt Confirmation |
| From Branch  | Dropdown     | List of available branches                  |
| To Branch    | Dropdown     | Destination branch or central warehouse     |
| Date Range   | Date Range   | From – To                                   |

---

# Search

Searchable by:

- Request ID
- Product Name
- Requesting Branch
- Destination Branch
- Requested By

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

| Field                   | Type     | Required | Validation                   |
| ----------------------- | -------- | -------- | ---------------------------- |
| Total Quantity Received | Number   | Yes      | Must be > 0                  |
| Select Type:            | Checkbox | Yes      | Assets / Consumable / Resell |
| Assets Qty              | Number   | No       | ≥ 0, sum must equal Total    |
| Consumable Qty          | Number   | No       | ≥ 0, sum must equal Total    |
| Resell Qty              | Number   | No       | ≥ 0, sum must equal Total    |

> **Validation Rule:** Assets Qty + Consumable Qty + Resell Qty = Total Quantity Received

---

#### Section 3: Asset Details Fields _(Conditional – Visible only if **Assets Qty > 0**)_

| Field               | Type     | Required            | Validation / Options                           |
| ------------------- | -------- | ------------------- | ---------------------------------------------- |
| Asset ID Generation | Radio    | Yes (if Assets > 0) | Auto / Manual                                  |
| Assignment Type     | Dropdown | Yes (if Assets > 0) | Branch Pool / Direct to Employee               |
| Default Assignment  | Dropdown | Conditional         | Central Warehouse / Specific Branch            |
| Branch              | Dropdown | Conditional         | Select branch (required if Direct to Employee) |
| Role                | Dropdown | Conditional         | Employee role in selected branch               |
| Person              | Dropdown | Conditional         | Select employee                                |

---

### Field Logic

| Assignment Type        | Visible Fields       | Description                                                                     |
| ---------------------- | -------------------- | ------------------------------------------------------------------------------- |
| **Branch Pool**        | Default Assignment   | Assets are assigned to a branch pool (Central Warehouse or Specific Branch).    |
| **Direct to Employee** | Branch, Role, Person | Assets are directly assigned to a specific employee within the selected branch. |

---

### Conditional Behavior

- If **Assignment Type = Branch Pool**
  - Show **Default Assignment**
  - Hide **Branch, Role, Person**

- If **Assignment Type = Direct to Employee**
  - Show **Branch → Role → Person selection**
  - Hide **Default Assignment**

---

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
| Invoice Copy       | File Upload     | No          | PDF/JPG/PNG, max 5MB                         |
| Batch Number       | Text            | Conditional | Required for consumables, max 50 chars       |
| Manufacturing Date | Date            | Conditional | Required for consumables                     |
| Expiry Date        | Date            | Conditional | Required for consumables, must be > Mfg Date |

#### Section 5: Initial Allocation Fields

| Field                         | Type            | Description           |
| ----------------------------- | --------------- | --------------------- |
| Immediate Transfer to Branchs | Multi-select    | Distribute on receipt |
| Branch1 Qty                   | Number          | If selected           |
| Branch2 Qty                   | Number          | If selected           |
| Remain at Central             | Auto-calculated | Balance               |

## \*all non required fields are optional
