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
| Branch       | Dropdown     | All My Branches / Central / BLR / HYD / BOM / etc. |
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

## Add Stock (Multi-Mode)

\*-On click of Add Stock button in Stock Dashboard add dropdown to select this

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

### 11.1.1 Mode: Add to Central Stock (Head Ops)

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

---

# 11.1.2 Mode: Edit Central Stock Entry (Head Ops)

**Description:** Modify an existing Central Stock entry for correction or operational updates. Procurement information and generated asset IDs remain protected to maintain inventory audit integrity.

---

# Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     EDIT CENTRAL STOCK ENTRY                                 │
│                                                                             │
│  Entry Reference ID : CSTK-000482  [System Generated]                       │
│  Created On         : 12 Feb 2026                                           │
│  Created By         : Head Ops User                                         │
│                                                                             │
│  Section 1: Product Information (Read Only)                                  │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Product               : Brass Sprayer                                  │ │
│  │  Product Code          : BSP3                                           │ │
│  │  HSN Code              : 84242000                                       │ │
│  │  Current Central Stock : 132                                            │ │
│  │  Base UOM              : Units                                          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│  Section 2: Stock Type Allocation                                           │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Total Quantity Received : 20  [Locked after creation]                 │ │
│  │                                                                         │ │
│  │  Assets Qty              : [ 10 ]                                       │ │
│  │  Consumable Qty          : [ 5 ]                                        │ │
│  │  Resell Qty              : [ 5 ]                                        │ │
│  │                                                                         │ │
│  │  Validation: Assets + Consumable + Resell = Total ✓                    │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│  Section 3: Asset Details (Visible if Assets Qty > 0)                       │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Asset ID Generation : Auto  [Locked]                                   │ │
│  │                                                                         │ │
│  │  Default Assignment* : [▼ Central Warehouse / Branch ▼]                │ │
│  │  Assignment Type*    : [▼ Branch Pool / Direct to Employee ▼]          │ │
│  │                                                                         │ │
│  │  ASSET TABLE (Editable Assignment Only)                                │ │
│  │                                                                         │ │
│  │  ┌───────────┬─────────────────┬─────────────┬─────────┬────────┐      │ │
│  │  │ Asset ID  │ Product Name    │ Assigned To │ Branch  │ Status │      │ │
│  │  │───────────┼─────────────────┼─────────────┼─────────┼────────│      │ │
│  │  │ BSP3/0026 │ Brass Sprayer   │ [Select ▼]  │ BLR     │ Avail. │      │ │
│  │  │ BSP3/0027 │ Brass Sprayer   │ [Select ▼]  │ BLR     │ Avail. │      │ │
│  │  │ BSP3/0028 │ Brass Sprayer   │ [Select ▼]  │ HYD     │ Avail. │      │ │
│  │  └───────────┴─────────────────┴─────────────┴─────────┴────────┘      │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│  Section 4: Purchase & Tax Details                                          │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Supplier / Vendor      : ABC Industrial Supplies                       │ │
│  │  Purchase Order Ref     : PO-23874                                       │ │
│  │  Invoice Number         : INV-78912                                      │ │
│  │  Invoice Date           : 10 Feb 2026                                     │ │
│  │                                                                         │ │
│  │  Invoice Amount (₹)     : ₹ 25,000                                       │ │
│  │  Tax Amount (₹)         : ₹ 4,500                                        │ │
│  │  Total with Tax (₹)     : ₹ 29,500                                       │ │
│  │                                                                         │ │
│  │  Replace Invoice Copy   : [📎 Upload New File]                           │ │
│  │                                                                         │ │
│  │  Batch Number           : [Edit if Consumables]                          │ │
│  │  Manufacturing Date     : [📅 Editable]                                   │ │
│  │  Expiry Date            : [📅 Editable]                                   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│  Section 5: Branch Allocation (Optional Update)                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Current Branch Transfers                                               │ │
│  │                                                                         │ │
│  │  BLR Branch Qty       : [ 4 ]                                           │ │
│  │  HYD Branch Qty       : [ 3 ]                                           │ │
│  │  BOM Branch Qty       : [ 2 ]                                           │ │
│  │                                                                         │ │
│  │  Remaining at Central : 11 (Auto Calculated)                            │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│                     [UPDATE]   [CANCEL]                                      │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

#### Section 1: Product Information Fields

| Field                 | Type    | Editable | Notes                       |
| --------------------- | ------- | -------- | --------------------------- |
| Product               | Display | No       | Locked after entry creation |
| Product Code          | Display | No       | System reference            |
| HSN Code              | Display | No       | Used for tax calculation    |
| Current Central Stock | Display | No       | Informational               |
| Base UOM              | Display | No       | Product master reference    |

---

#### Section 2: Stock Allocation Fields

| Field                   | Type   | Editable | Validation                 |
| ----------------------- | ------ | -------- | -------------------------- |
| Total Quantity Received | Number | No       | Locked for audit integrity |
| Assets Qty              | Number | Yes      | ≥0                         |
| Consumable Qty          | Number | Yes      | ≥0                         |
| Resell Qty              | Number | Yes      | ≥0                         |

**Validation Rule**

```
Assets Qty + Consumable Qty + Resell Qty = Total Quantity Received
```

---

#### Section 3: Asset Details Fields

| Field              | Type     | Editable    | Validation                       |
| ------------------ | -------- | ----------- | -------------------------------- |
| Asset ID           | System   | No          | Generated during creation        |
| Assignment Type    | Dropdown | Yes         | Branch Pool / Direct to Employee |
| Default Assignment | Dropdown | Conditional | Branch Pool only                 |
| Branch             | Dropdown | Conditional | Required if Direct Assignment    |
| Role               | Dropdown | Conditional | Employee role                    |
| Person             | Dropdown | Conditional | Select employee                  |

---

#### Editable Asset Table Behavior

Allowed edits:

- Change **Branch**
- Change **Assigned To**
- Change **Status** (if allowed)

Not allowed:

- Modify **Asset ID**
- Modify **Product**

---

#### Section 4: Purchase & Tax Details

| Field              | Editable | Notes                             |
| ------------------ | -------- | --------------------------------- |
| Supplier           | No       | Locked to maintain purchase trace |
| Purchase Order Ref | No       | Historical reference              |
| Invoice Number     | No       | Accounting record                 |
| Invoice Date       | No       | Financial integrity               |
| Invoice Amount     | No       | Locked                            |
| Tax Amount         | No       | Auto from HSN                     |
| Total with Tax     | No       | Calculated                        |
| Invoice Copy       | Yes      | Can upload replacement            |

---

#### Section 5: Branch Allocation Fields

| Field                | Type   | Editable |
| -------------------- | ------ | -------- |
| Branch Qty           | Number | Yes      |
| Remaining at Central | Auto   | No       |

Validation:

```
Total Branch Transfer ≤ Total Quantity Received
```

---

## Additional System Rules

### 1. Edit Restrictions

The system **should prevent editing** if:

- Assets already **issued to employees**
- Assets **transferred to branches**
- Assets **sold or consumed**

---

### 2. Audit Trail

Every edit must log:

| Field           | Value     |
| --------------- | --------- |
| Edited By       | User ID   |
| Edited Date     | Timestamp |
| Changed Fields  | List      |
| Previous Values | Stored    |

---

# 11.1.3 View Central Stock Entry (Head Ops)

**Description:**
Display complete details of a Central Stock entry in **read-only mode**. This screen allows Head Ops, Admins, and authorized users to review procurement details, stock allocation, asset generation, and branch transfers without allowing modifications.

This screen is mainly used for:

- **Stock audit**
- **Procurement verification**
- **Asset tracking**
- **Inventory reconciliation**

---

# Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     VIEW CENTRAL STOCK ENTRY                                 │
│                                                                             │
│  Entry Reference ID : CSTK-000482                                           │
│  Created On         : 12 Feb 2026                                           │
│  Created By         : Head Ops User                                         │
│                                                                             │
│  Section 1: Product Information                                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Product               : Brass Sprayer                                  │ │
│  │  Product Code          : BSP3                                           │ │
│  │  HSN Code              : 84242000                                       │ │
│  │  Base UOM              : Units                                          │ │
│  │  Current Central Stock : 132                                            │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│  Section 2: Stock Type Allocation                                           │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Total Quantity Received : 20                                           │ │
│  │                                                                         │ │
│  │  Assets Qty              : 10                                           │ │
│  │  Consumable Qty          : 5                                            │ │
│  │  Resell Qty              : 5                                            │ │
│  │                                                                         │ │
│  │  Validation Status       : ✓ Allocation Balanced                        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│  Section 3: Asset Details                                                   │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Asset ID Generation : Auto                                             │ │
│  │  Assignment Type     : Branch Pool                                      │ │
│  │  Default Assignment  : Central Warehouse                                │ │
│  │                                                                         │ │
│  │  GENERATED ASSET TABLE                                                  │ │
│  │                                                                         │ │
│  │  ┌───────────┬─────────────────┬─────────────┬─────────┬────────────┐  │ │
│  │  │ Asset ID  │ Product Name    │ Assigned To │ Branch  │ Status     │  │ │
│  │  │───────────┼─────────────────┼─────────────┼─────────┼────────────│  │ │
│  │  │ BSP3/0026 │ Brass Sprayer   │ Unassigned  │ Central │ Available  │  │ │
│  │  │ BSP3/0027 │ Brass Sprayer   │ Unassigned  │ Central │ Available  │  │ │
│  │  │ BSP3/0028 │ Brass Sprayer   │ Rahul Shah  │ BLR     │ Issued     │  │ │
│  │  │ BSP3/0029 │ Brass Sprayer   │ Unassigned  │ HYD     │ Available  │  │ │
│  │  └───────────┴─────────────────┴─────────────┴─────────┴────────────┘  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│  Section 4: Purchase & Tax Details                                          │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Supplier / Vendor      : ABC Industrial Supplies                       │ │
│  │  Purchase Order Ref     : PO-23874                                       │ │
│  │  Invoice Number         : INV-78912                                      │ │
│  │  Invoice Date           : 10 Feb 2026                                     │ │
│  │                                                                         │ │
│  │  Invoice Amount (₹)     : ₹ 25,000                                       │ │
│  │  Tax Amount (₹)         : ₹ 4,500                                        │ │
│  │  Total with Tax (₹)     : ₹ 29,500                                       │ │
│  │                                                                         │ │
│  │  Invoice Copy           : [View / Download]                              │ │
│  │                                                                         │ │
│  │  Batch Number           : BATCH-7823                                     │ │
│  │  Manufacturing Date     : 01 Jan 2026                                     │ │
│  │  Expiry Date            : 01 Jan 2028                                     │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│  Section 5: Branch Allocation Summary                                       │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Branch Transfers                                                        │ │
│  │                                                                         │ │
│  │  BLR Branch Qty       : 4                                               │ │
│  │  HYD Branch Qty       : 3                                               │ │
│  │  BOM Branch Qty       : 2                                               │ │
│  │                                                                         │ │
│  │  Remaining at Central : 11                                              │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│   [EDIT]   [TRANSFER STOCK]   [VIEW ASSET HISTORY]   [DOWNLOAD INVOICE]    │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# Section 1: Product Information Fields

| Field                 | Type    | Editable | Description                      |
| --------------------- | ------- | -------- | -------------------------------- |
| Product               | Display | No       | Product name from Product Master |
| Product Code          | Display | No       | Unique product identifier        |
| HSN Code              | Display | No       | Used for tax calculation         |
| Base UOM              | Display | No       | Unit of measurement              |
| Current Central Stock | Display | No       | Current stock reference          |

---

# Section 2: Stock Allocation Fields

| Field                   | Type   | Description                       |
| ----------------------- | ------ | --------------------------------- |
| Total Quantity Received | Number | Quantity received during purchase |
| Assets Qty              | Number | Quantity allocated as assets      |
| Consumable Qty          | Number | Quantity allocated as consumables |
| Resell Qty              | Number | Quantity allocated for resale     |

Validation displayed:

```
Assets + Consumable + Resell = Total Quantity
```

---

# Section 3: Asset Details Fields

| Field        | Type    | Description                                |
| ------------ | ------- | ------------------------------------------ |
| Asset ID     | System  | Unique generated asset identifier          |
| Product Name | Display | Asset product                              |
| Assigned To  | Display | Employee assigned                          |
| Branch       | Display | Asset branch                               |
| Status       | Display | Available / Issued / Maintenance / Retired |

---

# Section 4: Purchase & Tax Details Fields

| Field              | Type     | Description                 |
| ------------------ | -------- | --------------------------- |
| Supplier           | Display  | Vendor from supplier master |
| Purchase Order Ref | Display  | Reference to PO             |
| Invoice Number     | Display  | Vendor invoice number       |
| Invoice Date       | Display  | Invoice date                |
| Invoice Amount     | Currency | Pre-tax amount              |
| Tax Amount         | Currency | Tax calculated using HSN    |
| Total with Tax     | Currency | Invoice + Tax               |
| Invoice Copy       | File     | View or download            |
| Batch Number       | Display  | For consumables             |
| Manufacturing Date | Date     | For consumables             |
| Expiry Date        | Date     | For consumables             |

---

# Section 5: Branch Allocation Fields

| Field                | Type    | Description          |
| -------------------- | ------- | -------------------- |
| Branch Name          | Display | Destination branch   |
| Quantity             | Number  | Quantity transferred |
| Remaining at Central | Auto    | Remaining stock      |

---

# Actions Available

| Button             | Action                                    |
| ------------------ | ----------------------------------------- |
| Edit               | Opens **11.1.2 Edit Central Stock Entry** |
| Transfer Stock     | Initiates stock transfer workflow         |
| View Asset History | Shows lifecycle of assets                 |
| Download Invoice   | Download uploaded invoice                 |
| Back               | Return to stock list                      |

---

# System Behavior

### 1. Read-Only Mode

All fields are **non-editable**.

### 2. Permission Control

Only the following roles can access:

- Head Ops
- Inventory Admin
- Finance Auditor

---

# Audit & Traceability

Each record displays:

| Field        | Description            |
| ------------ | ---------------------- |
| Created By   | User who created entry |
| Created On   | Timestamp              |
| Last Updated | Last modification      |
| Updated By   | User who edited record |

---

================================================================================

# 11.2 Tab 2: My Requests

**Description**

Track all stock and transfer requests created by the user or related to the user's branch.

This tab allows users to monitor the **entire lifecycle of requests** including:

- Creation
- Approval
- Dispatch
- Transit
- Final Receipt

Users can also **confirm stock receipt** when the request reaches **In Transit status**.

If stock is damaged, missing, or incorrect, users can **report issues during receipt confirmation**.

---

# Screen Layout

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│                              STOCK DASHBOARD                                  │
│                                                                              │
│ [Tab 1: All Products] [Tab 2: My Requests] [Tab 3: Received Requests]        │
│                                                                              │
│ ───────────── Request Type Tabs ─────────────                                │
│ [A) Stock Requests]   [B) Branch Transfers]                                  │
│                                                                              │
│ Status Filter: [All] [Pending] [Approved] [Rejected] [Dispatch]              │
│                [In Transit] [Received] [Issue Reported]                      │
│                                                                              │
│ [+ Add Request]                                                              │
│                                                                              │
│ REQUEST TABLE                                                                │
│ ┌──────────────────────────────────────────────────────────────────────────┐ │
│ │Request ID │Type │Direction│From│To │Items│Total│Assets│Status │Priority │ │
│ │───────────┼─────┼─────────┼────┼───┼─────┼─────┼──────┼───────┼──────── │ │
│ │SR-BLR-001 │Stock│Inward   │CEN │BLR│ 3   │ 25  │ 5    │Pending│High     │ │
│ │SR-BLR-002 │Stock│Inward   │CEN │BLR│ 2   │ 10  │ 0    │Approved│Normal  │ │
│ └──────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│ ┌──────────────────────────────────────────────────────────────────────────┐ │
│ │Requested By │Requested Date & Time │Action                                │ │
│ │─────────────┼──────────────────────┼──────────────────────────────────── │ │
│ │John Doe     │15 Jan 2024 10:45 AM  │[View] [Revoke]                       │ │
│ │Jane Smith   │16 Jan 2024 02:30 PM  │[View]                                │ │
│ └──────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│ Pagination: Previous 1 2 3 ... 10 Next                                       │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

# Table Fields (Applicable to Both Tabs)

| Field                 | Type     | Description                        |
| --------------------- | -------- | ---------------------------------- |
| Request ID            | Text     | Unique identifier (SR / TR format) |
| Request Type          | Badge    | Stock Request / Transfer Request   |
| Direction             | Icon     | Inward or Outward                  |
| From                  | Text     | Source branch or warehouse         |
| To                    | Text     | Destination branch                 |
| Items Count           | Number   | Number of product lines            |
| Total Qty             | Number   | Total quantity requested           |
| Assets Count          | Number   | Number of asset items              |
| Status                | Badge    | Current lifecycle status           |
| Priority              | Badge    | Low / Normal / High / Urgent       |
| Requested By          | Text     | User who created the request       |
| Requested Date & Time | DateTime | Timestamp of request creation      |
| Action                | Button   | View / Revoke / Receive            |

---

# Actions

| Action  | Available When      | Description                |
| ------- | ------------------- | -------------------------- |
| View    | All statuses        | Opens request details      |
| Revoke  | Draft / Pending     | Cancels request            |
| Receive | Status = In Transit | Opens receipt confirmation |

---

# Request Type Behavior

| Request Type     | Direction | Example                                    |
| ---------------- | --------- | ------------------------------------------ |
| Stock Request    | Inward    | Branch requesting stock from warehouse     |
| Transfer Request | Inward    | Branch receiving stock from another branch |
| Transfer Request | Outward   | Branch sending stock to another branch     |

# Filters

| Filter Name       | Type                | Default      | Options                                                                     | Purpose                          |
| ----------------- | ------------------- | ------------ | --------------------------------------------------------------------------- | -------------------------------- |
| Request Type      | Dropdown            | All          | Stock Request, Transfer Request                                             | Separate stock vs transfer flows |
| Status            | Multi-Select        | All          | Pending, Approved, Rejected, Dispatch, In Transit, Received, Issue Reported | Track lifecycle stage            |
| Branch(from - to) | Searchable Dropdown | User Branch  | All Branches / Specific Branch                                              | Filter by source/destination     |
| Date Range        | Date Picker         | Last 30 Days | Custom Range                                                                | Filter based on request creation |
| Priority          | Dropdown            | All          | Low, Normal, High, Urgent                                                   | Focus on critical requests       |

### Search -Global

---

# Benefits of This Design

✔ Clear separation of **Warehouse Requests vs Branch Transfers**
✔ Same table structure → **simpler backend APIs**
✔ Cleaner UI navigation
✔ Easier future expansion (Assets / Returns / Repairs)
✔ Scales well for **multi-branch ERP**

---

# 11.2.1 Stock Request (Branch to Central)

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
│                    [SAVE DRAFT]        [SUBMIT REQUEST-> opens 11.2.1.1]     │
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

# 11.2.1.1 Select Recipients for Stock Request (Popup)

**Description:**
This popup appears after the user clicks **[Submit Request]** in **Module 11.2.1 Stock Request**.
It allows the requester to select **one or multiple recipients** (approvers or responsible personnel) to whom the stock request notification will be sent.

By default, **“All” recipients are selected**, meaning the request will be sent to all authorized users configured to receive stock requests (such as Admin, HR Manager, Operations Head, etc.).
The user may alternatively **select specific individuals** if the request needs to be routed to a particular person.

After confirming, the system sends the request notification and updates the request workflow to **Pending Approval / Review**.

---

### Popup Layout

```
┌─────────────────────────────────────────────────────────────┐
│  POPUP: SELECT RECIPIENTS                                    │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Send to specific person(s):                         │    │
│  │  ☑ All (Default checked)                            │    │
│  │  ☐ Raj Kumar (Admin)                                │    │
│  │  ☐ Mike Smith (HR Manager)                          │    │
│  │  ☐ Sarah Jones (Operations Head)                    │    │
│  │                                                      │    │
│  │  [CONFIRM SEND]  [CANCEL]                            │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

# 11.2.2 Stock Request – View (Branch / Head Ops)

## Description

This screen displays the **complete details of a submitted Stock Request** in **read-only mode**.
It allows **Branch Users, Head Office Operations, Approvers, and Inventory Managers** to review the request across the **entire workflow lifecycle**.

The screen provides full visibility into:

- Request creation
- Approval workflow
- Dispatch & logistics movement
- Transit status
- Final stock receipt confirmation
- Issue reporting (if stock not received or damaged)

This screen is primarily used for:

- Request tracking
- Approval verification
- Inventory planning
- Logistics monitoring
- Audit trail reference
- Operational transparency

---

# Workflow Status Coverage

The view screen dynamically displays information depending on the **current request status**.

| Status                  | Meaning                                  |
| ----------------------- | ---------------------------------------- |
| Draft                   | Request saved but not submitted          |
| Pending Approval        | Waiting for approval from manager / HO   |
| Approved                | Approved for processing                  |
| Rejected                | Request rejected                         |
| Dispatch                | Stock prepared and dispatched            |
| In Transit              | Stock currently moving between locations |
| Partially Received      | Some items received                      |
| Received                | Fully received                           |
| Delivery Issue Reported | Stock not received or damaged            |
| Revoked                 | Request cancelled by requester           |

---

# Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           VIEW STOCK REQUEST                                 │
│                                                                              │
│  Request ID : SR-BLR-2026-0045                                               │
│  Status     : Pending Approval                                               │
│                                                                              │
│ ───────────────── REQUEST TIMELINE ─────────────────                         │
│  Draft → Submitted → Approved → Dispatch → In Transit → Received            │
│                                                                              │
│ ───────────────── REVIEW INFORMATION ─────────────────                       │
│ Status                 : Pending Approval                                    │
│ Reviewed By            : —                                                   │
│ Review Date            : —                                                   │
│                                                                              │
│ IF REJECTED:                                                                 │
│ Rejection Reason       : —                                                   │
│                                                                              │
│ IF APPROVED:                                                                 │
│ Approved By            : Head Operations                                     │
│ Approved Date          : 02-Jan-2026                                         │
│                                                                              │
│ ───────────────── SUBMISSION INFORMATION ─────────────────                   │
│ Sent To                : HR Manager                                          │
│ Submitted Date         : 02-Jan-2026                                         │
│ Last Updated           : 03-Jan-2026                                         │
│                                                                              │
│ ┌─────────────────────────────────────────────────────────────────────────┐ │
│ │ HEADER INFORMATION                                                      │ │
│ │                                                                         │ │
│ │ Requesting Branch    : BLR Branch                                       │ │
│ │ Requested By         : Rahul Shah                                       │ │
│ │ Request Date         : 15 Feb 2026                                       │ │
│ │ Priority             : High                                              │ │
│ │ Required By Date     : 20 Feb 2026                                       │ │
│ │ Purpose              :                                                   │ │
│ │ Required for technician equipment replenishment and field operations    │ │
│ └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│ ┌─────────────────────────────────────────────────────────────────────────┐ │
│ │ ITEMS TABLE                                                             │ │
│ │                                                                         │ │
│ │ Product      │Code │Current Stock│Assets│Consumable│Resell│Total Qty    │ │
│ │──────────────┼─────┼─────────────┼──────┼──────────┼──────┼─────────────│ │
│ │ Brass Sprayer│BSP3 │3            │5     │10        │0     │15           │ │
│ │ Gloves Pack  │GLV1 │12           │0     │20        │0     │20           │ │
│ │ Spray Motor  │SM2  │2            │10    │0         │5     │15           │ │
│ └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│ ┌─────────────────────────────────────────────────────────────────────────┐ │
│ │ ITEM DETAILS TABLE                                                      │ │
│ │                                                                         │ │
│ │ Total Qty │UOM │Est Cost │Tax │Item Purpose                             │ │
│ │───────────┼────┼─────────┼────┼────────────────────────────────────────│ │
│ │ 15        │Unit│7500     │1350│Field usage                              │ │
│ │ 20        │Pack│2000     │360 │Safety gear                              │ │
│ │ 15        │Unit│15000    │2700│Sales stock                              │ │
│ └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│ ┌─────────────────────────────────────────────────────────────────────────┐ │
│ │ LOGISTICS INFORMATION (Visible after approval)                          │ │
│ │                                                                         │ │
│ │ Dispatch Warehouse      : Central Warehouse                             │ │
│ │ Dispatch Date           : 05-Jan-2026                                    │ │
│ │ Dispatch By             : Warehouse Manager                             │ │
│ │ Transport Mode          : Courier                                       │ │
│ │ Tracking ID             : TRK-987654                                     │ │
│ │ Expected Delivery Date  : 08-Jan-2026                                    │ │
│ └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│ ┌─────────────────────────────────────────────────────────────────────────┐ │
│ │ RECEIVING INFORMATION (Visible after dispatch)                          │ │
│ │                                                                         │ │
│ │ Received By           : Rahul Shah                                      │ │
│ │ Received Date         : 08-Jan-2026                                      │ │
│ │ Received Condition    : Good / Damaged / Missing                        │ │
│ │ Remarks               : All items verified                               │ │
│ └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│ ┌─────────────────────────────────────────────────────────────────────────┐ │
│ │ ISSUE REPORT (If delivery problem occurs)                               │ │
│ │                                                                         │ │
│ │ Issue Type          : Not Received / Damaged / Missing Items            │ │
│ │ Reported By         : Rahul Shah                                        │ │
│ │ Report Date         : 09-Jan-2026                                        │ │
│ │ Issue Description   : Courier delayed shipment                           │ │
│ │ Resolution Status   : Pending / Resolved                                │ │
│ └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│ ┌─────────────────────────────────────────────────────────────────────────┐ │
│ │ SUMMARY SECTION                                                         │ │
│ │                                                                         │ │
│ │ Total Products              : 3                                         │ │
│ │ Total Assets Requested      : 15                                        │ │
│ │ Total Consumables Requested : 30                                        │ │
│ │ Total Resell Requested      : 5                                         │ │
│ │                                                                         │ │
│ │ Total Estimated Value       : ₹ 24,500                                  │ │
│ │                                                                         │ │
│ │ Notes for Approver          :                                           │ │
│ │ Urgent requirement due to new service contracts                         │ │
│ └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│ ───────────────── STATUS HISTORY / AUDIT LOG ─────────────────              │
│ Date & Time       │ Action               │ User               │ Remarks      │
│───────────────────┼──────────────────────┼────────────────────┼────────────│
│ 01 Jan 10:30 AM   │ Draft Created        │ Rahul Shah         │ —           │
│ 01 Jan 11:10 AM   │ Submitted            │ Rahul Shah         │ Sent to HR  │
│ 02 Jan 09:15 AM   │ Approved             │ Head Operations    │ Approved    │
│ 05 Jan 03:40 PM   │ Dispatched           │ Warehouse Manager  │ Courier     │
│ 08 Jan 12:30 PM   │ Received             │ Rahul Shah         │ Verified    │
│                                                                              │
│                                  [BACK]                                      │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# Header Information Fields

| Field             | Type           | Description                  |
| ----------------- | -------------- | ---------------------------- |
| Request ID        | Auto Generated | Unique stock request number  |
| Requesting Branch | Display        | Branch raising request       |
| Requested By      | Display        | Employee creating request    |
| Request Date      | Date           | Request creation date        |
| Priority          | Badge          | Low / Normal / High / Urgent |
| Required By Date  | Date           | Target fulfillment date      |
| Purpose           | Text           | Business justification       |

---

# Review Information Fields

| Field            | Type    | Description            |
| ---------------- | ------- | ---------------------- |
| Status           | System  | Current approval state |
| Reviewed By      | Display | Approver name          |
| Review Date      | Date    | Date of review         |
| Rejection Reason | Text    | Visible if rejected    |

---

# Submission Information Fields

| Field          | Type    | Description                      |
| -------------- | ------- | -------------------------------- |
| Sent To        | Display | Role or person receiving request |
| Submitted Date | Date    | Date request submitted           |
| Last Updated   | Date    | Latest modification              |

---

# Logistics Fields (After Dispatch)

| Field                  | Description                  |
| ---------------------- | ---------------------------- |
| Dispatch Warehouse     | Warehouse sending stock      |
| Dispatch Date          | Date stock dispatched        |
| Dispatch By            | Warehouse operator           |
| Transport Mode         | Courier / Internal transport |
| Tracking ID            | Shipment tracking reference  |
| Expected Delivery Date | Estimated arrival date       |

---

# Receiving Fields

| Field              | Description               |
| ------------------ | ------------------------- |
| Received By        | Person confirming receipt |
| Received Date      | Date of stock arrival     |
| Received Condition | Good / Damaged / Missing  |
| Remarks            | Additional notes          |

---

# Issue Reporting Fields

| Field             | Description                        |
| ----------------- | ---------------------------------- |
| Issue Type        | Not Received / Damaged / Missing   |
| Reported By       | Person reporting issue             |
| Report Date       | Issue report date                  |
| Issue Description | Details of issue                   |
| Resolution Status | Pending / Investigating / Resolved |

---

# Status History / Audit Log

Tracks the **complete request lifecycle** for compliance and operational transparency.

| Field       | Description                 |
| ----------- | --------------------------- |
| Date & Time | Timestamp of event          |
| Action      | Status change               |
| User        | Person who performed action |
| Remarks     | Optional comments           |

---

# Actions

| Button | Action                     |
| ------ | -------------------------- |
| Back   | Return to My Requests list |

---

# 11.2.3 Receive Stock / Asset Allocation/ Receive (Destination Branch)

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
| Upload Receipt Photo | File Upload | No       | JPG/PNG, max 5MB                           |
| Remarks              | Text Area   | No       | Max 500 characters                         |
| Confirm Receipt      | Checkbox    | Yes      | Must be checked                            |

**Post-Receipt Actions:**

- Assets activated at destination branch
- New assignments recorded
- Consumables added to branch stock
- Source branch notified
- Activity log updated with both branch metadata

---

=======================

# 11.3 Tab 3: Received Requests

**Description:**
For **Branch Managers and Head Operations** to review incoming requests, approve stock transfers, and confirm stock receipt. This tab provides validation indicators for stock availability before approval or receipt confirmation.

Requests may be created and saved using **Save Draft** before final submission. Draft requests are **not visible to approvers or other branches** until they are formally submitted.

Users can later open a draft request, update details, and **Submit** it for approval workflow.

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
│  [+ Allocate Stock]                                                          │
│                                                                              │
│  RECEIVED REQUESTS TABLE                                                     │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │Request ID │Type│From│To │Items│Total Value│Requested By│Validation│Action│ │
│  │───────────┼────┼────┼───┼─────┼───────────┼────────────┼──────────┼──────│ │
│  │SR-BLR-001 │Appr│BLR │CEN│ 3   │ ₹45,000   │John Doe    │✅ Avail. │👁 ✏ ✔ │ │
│  │SR-HYD-002 │Appr│HYD │CEN│ 2   │ ₹28,500   │Jane Smith  │⚠️ Partial│👁 ✏ ✔ │ │
│  │TR-2024-056│Rcpt│CEN │BLR│ 2   │ ₹32,000   │Rajesh K.   │⏳ Pending│👁 ✏ ✔ │ │
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

| Field                 | Type     | Description                                                                                       |
| --------------------- | -------- | ------------------------------------------------------------------------------------------------- |
| Request ID            | Text     | Unique identifier (SR-XXXX / TR-XXXX format)                                                      |
| Request Type          | Badge    | Stock Request / Transfer Request / Asset Assignment                                               |
| From                  | Text     | Requesting branch or location                                                                     |
| To                    | Text     | Receiving branch or central warehouse                                                             |
| Items                 | Number   | Number of products included in the request                                                        |
| Total Value (₹)       | Currency | Total value of requested items including tax                                                      |
| Requested By          | Text     | User and role who created the request                                                             |
| Valid stock           | Icon     | ✅ Available / ⚠️ Partial / ❌ Insufficient / ⏳ Pending                                          |
| Requested Date & Time | DateTime | Original request creation timestamp                                                               |
| Expected Date         | Date     | Expected delivery or receipt date                                                                 |
| Reviewed By           | Text     | Name of approving authority                                                                       |
| Review Date & Time    | DateTime | Timestamp when the request was reviewed                                                           |
| Status                | Badge    | Draft / Pending / Approved / Rejected / Dispatch / In Transit / Received / Revoked/Issue Reported |

---

# Action Buttons

| Action Button | Icon | Description                                          |
| ------------- | ---- | ---------------------------------------------------- |
| View          | 👁   | Opens request details in **read-only mode**          |
| Edit          | ✏    | Allows editing of request details (permission based) |
| Approve       | ✔    | Redirects to **Approval Form Screen**                |

---

# Conditional Action Rules

| Request Status       | Available Actions          |
| -------------------- | -------------------------- |
| Draft                | View, Edit, Submit         |
| Pending Approval     | View, Edit, Approve        |
| Approved             | View, Edit, Allocate Stock |
| In Transit           | View                       |
| Completed / Received | View                       |
| Rejected             | View, Edit                 |

---

# Form Submission Options

When creating or editing a request, the form provides the following options:

| Button     | Action                                                                |
| ---------- | --------------------------------------------------------------------- |
| Save Draft | Saves request without starting approval workflow                      |
| Submit     | Sends request for approval and changes status to **Pending Approval** |
| Cancel     | Discards changes and returns to list screen                           |

---

# Filters

| Filter         | Type         | Options                                     |
| -------------- | ------------ | ------------------------------------------- |
| Request Type   | Multi Select | Request for Approval / Receipt Confirmation |
| From-To Branch | Dropdown     | List of available branches                  |
| Date Range     | Date Range   | From – To                                   |

---

# Search

Searchable by:

- Request ID
- Product Name
- Requested By

---

# **11.3.1 Request Approval Form (Branch Manager / Head Operations)**

## **Description**

The **Request Approval Form** enables authorized approvers (Branch Manager or Head Operations) to review and process stock requests submitted by branches.

The approver can:

- Review request details
- Validate stock availability in the **Central Warehouse**
- Modify approved quantities
- Approve, partially approve, reject, or place requests on hold
- Initiate **alternative sourcing** if stock is insufficient

Alternative sourcing options:

- **Other Branch Transfer**
- **Purchase Order**

When **Alternative Source = Other Branch**, the **Branch Transfer Planning section becomes visible** within the same screen.
This prepares a **transfer draft**, which continues in **Module 11.5 Branch Transfer**.

---

# **Navigation**

```
Module 11.3 → Received Requests → Action → Review / Approve
```

---

# **Screen Layout**

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          REQUEST APPROVAL FORM                               │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐│
│  │ REQUEST DETAILS (Read-only)                                             ││
│  │                                                                         ││
│  │ Request ID            : SR-BLR-2024-001234                              ││
│  │ Request Type          : Stock Request                                   ││
│  │ From Branch           : BLR                                             ││
│  │ Requested By          : John Doe                                        ││
│  │ Requested Date        : 15 Jan 2024 10:30 AM                            ││
│  │ Priority              : HIGH                                            ││
│  │ Required By           : 18 Jan 2024                                     ││
│  │ Purpose               : Monthly Contract Services                            ││
│  └─────────────────────────────────────────────────────────────────────────┘│
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐│
│  │ REQUESTED ITEMS WITH STOCK VALIDATION                                   ││
│  │                                                                         ││
│  │ ┌───────────┬──────────┬──────────┬──────────┬────────────┬───────────┐ ││
│  │ │ Product   │Assets Req│Cons. Req │Resell Req│Central Avl │ Status    │ ││
│  │ ├───────────┼──────────┼──────────┼──────────┼────────────┼───────────┤ ││
│  │ │BSP3-001   │ 5        │ 10       │ 0        │A:20,C:50   │ ✅ Avail. │ ││
│  │ │CH-3808-001│ 0        │ 20       │ 0        │A:0,C:15    │ ⚠ Partial │ ││
│  │ │RBS-001    │ 10       │ 0        │ 0        │A:5,C:100   │ ⚠ Partial │ ││
│  │ └───────────┴──────────┴──────────┴──────────┴────────────┴───────────┘ ││
│                                                                             │
│ APPROVED QUANTITIES                                                         │
│                                                                             │
│ ┌───────────┬───────────┬───────────┬───────────┬────────────────────────┐ │
│ │ Product   │Appr.Assets│Appr.Cons. │Appr.Resell│ Alternative Source     │ │
│ ├───────────┼───────────┼───────────┼───────────┼────────────────────────┤ │
│ │BSP3-001   │ [ 5 ]     │ [ 10 ]    │ [ 0 ]     │ —                      │ │
│ │CH-3808-001│ [ 0 ]     │ [ 15 ]    │ [ 0 ]     │ [Other Branch ▼]       │ │
│ │RBS-001    │ [ 5 ]     │ [ 0 ]     │ [ 0 ]     │ —                      │ │
│ └───────────┴───────────┴───────────┴───────────┴────────────────────────┘ │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐│
│  │ APPROVAL DECISION                                                       ││
│  │                                                                         ││
│  │ Approval Type*        : [Full Approval ▼]                               ││
│  │ Alternative Source    : [None ▼]                                        ││
│  │ Dispatch Date*        : [📅 Date Picker]                                ││
│  │ Expected Delivery*    : [📅 Date Picker]                                ││
│  │ Carrier*              : [Search Carrier ▼]                              ││
│  │ LR Number             : [____________________]                          ││
│  │ Asset ID Assignment   : ○ Auto Generate  ○ Manual Assignment           ││
│  │ Remarks*              : [________________________________________]      ││
│  └─────────────────────────────────────────────────────────────────────────┘│
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐│
│  │ BRANCH TRANSFER PLANNING (Visible when Alternative Source = Other Branch)││
│  │                                                                         ││
│  │ Transfer ID          : TR-YYYY-SEQ                                      ││
│  │ From Branch          : Central Warehouse                                ││
│  │ To Branch            : BLR                                              ││
│  │ Transfer Type        : [Emergency ▼]                                    ││
│  │ Reference Request    : SR-BLR-2024-001234                               ││
│  │                                                                         ││
│  │ ┌───────────┬──────────────┬───────────┬───────────┬───────────┐      ││
│  │ │ Product   │ SourceBranch │ AssetsQty │ ConsQty   │ ResellQty │      ││
│  │ ├───────────┼──────────────┼───────────┼───────────┼───────────┤      ││
│  │ │CH-3808-001│ HYD          │ 0         │ [5]       │ 0         │      ││
│  │ │RBS-001    │ BOM          │ [5]       │ 0         │ 0         │      ││
│  │ └───────────┴──────────────┴───────────┴───────────┴───────────┘      ││
│                                                                             │
│ Transfer Strategy                                                           │
│ ○ Single Branch Transfer                                                    │
│ ○ Split Transfer Across Branches                                            │
│                                                                             │
│  └─────────────────────────────────────────────────────────────────────────┘│
│                                                                             │
│                     [HOLD]   [REJECT]   [APPROVE]                           │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# **Section 1 — Request Details Fields**

| Field          | Type     | Required | Values                         |
| -------------- | -------- | -------- | ------------------------------ |
| Request ID     | Text     | Auto     | System generated               |
| Request Type   | Text     | Auto     | Stock Request, Branch Transfer |
| From Branch    | Text     | Auto     | Branch code                    |
| Requested By   | Text     | Auto     | Employee name                  |
| Requested Date | DateTime | Auto     | System timestamp               |
| Priority       | Badge    | Auto     | High, Normal, Low              |
| Required By    | Date     | Auto     | Delivery deadline              |
| Purpose        | Text     | Auto     | Request purpose                |

---

# **Section 2 — Approved Quantities Fields**

| Field              | Type     | Required    | Values                             |
| ------------------ | -------- | ----------- | ---------------------------------- |
| Appr. Assets       | Number   | Conditional | ≥0                                 |
| Appr. Consumables  | Number   | Conditional | ≥0                                 |
| Appr. Resell       | Number   | Conditional | ≥0                                 |
| Alternative Source | Dropdown | Conditional | None, Other Branch, Purchase Order |

---

# **Section 3 — Approval Decision Fields**

| Field               | Type            | Required    | Values                                           |
| ------------------- | --------------- | ----------- | ------------------------------------------------ |
| Approval Type       | Dropdown        | Yes         | Full Approval, Partial Approval, Reject, On Hold |
| Alternative Source  | Dropdown        | Conditional | None, Other Branch, Purchase Order               |
| Dispatch Date       | Date            | Yes         | ≥ Today                                          |
| Expected Delivery   | Date            | Yes         | ≥ Dispatch Date                                  |
| Carrier             | Search Dropdown | Yes         | Data from Carrier Master                         |
| LR Number           | Text            | Optional    | Alphanumeric                                     |
| Asset ID Assignment | Radio           | Conditional | Auto Generate, Manual Assignment                 |
| Remarks             | Text Area       | Yes         | Minimum 10 characters                            |

---

# **Section 4 — Branch Transfer Planning Fields**

| Field             | Type     | Required | Values                        |
| ----------------- | -------- | -------- | ----------------------------- |
| Transfer ID       | Text     | Auto     | TR-YYYY-SEQ                   |
| From Branch       | Text     | Auto     | Central Warehouse             |
| To Branch         | Text     | Auto     | Requesting branch             |
| Transfer Type     | Dropdown | Yes      | Emergency, Regular, Scheduled |
| Reference Request | Text     | Auto     | Request ID                    |

---

# **Branch Transfer Product Fields**

| Field          | Type     | Required    | Values       |
| -------------- | -------- | ----------- | ------------ |
| Product        | Text     | Auto        | Product Code |
| Source Branch  | Dropdown | Yes         | Branch List  |
| Assets Qty     | Number   | Conditional | ≥0           |
| Consumable Qty | Number   | Conditional | ≥0           |
| Resell Qty     | Number   | Conditional | ≥0           |

---

# **Transfer Strategy Options**

| Option                         |
| ------------------------------ |
| Single Branch Transfer         |
| Split Transfer Across Branches |

---

# **System Actions**

| Button  | Function            |
| ------- | ------------------- |
| APPROVE | Approves request    |
| REJECT  | Reject request      |
| HOLD    | Put request on hold |

---

# **System Behavior**

### If Approved

- Request status → **Approved**
- Inventory reserved
- Dispatch workflow triggered

### If Approved with Branch Transfer

- Transfer Draft created
- Linked to **Module 11.5 Branch Transfer**
- Logistics notified

### If Rejected

- Request status → **Rejected**
- Requester notified

### If On Hold

- Request status → **On Hold**

---

# **11.3.2 Edit Request Approval (Branch Manager / Head Operations)**

## **Description**

The **Edit Request Approval** screen allows authorized approvers (Branch Manager or Head Operations) to **modify previously approved, partially approved, or on-hold stock requests** before final dispatch or transfer execution.

This functionality is used when:

- Approved quantities need adjustment
- Dispatch schedule needs modification
- Carrier/logistics details change
- Alternative sourcing needs to be updated
- Transfer planning requires correction

The system maintains **full audit history of all changes**.

Possible actions in edit mode:

- Modify approved quantities
- Update dispatch details
- Change approval type
- Change alternative source
- Update branch transfer planning
- Save draft changes
- Re-approve or re-submit

---

# **Navigation**

```
Module 11.3 → Received Requests → Approved Requests → Action → Edit
```

---

# **Screen Layout**

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        EDIT REQUEST APPROVAL FORM                            │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐│
│  │ REQUEST DETAILS (Read-only)                                             ││
│  │                                                                         ││
│  │ Request ID            : SR-BLR-2024-001234                              ││
│  │ Request Type          : Stock Request                                   ││
│  │ From Branch           : BLR                                             ││
│  │ Requested By          : John Doe                                        ││
│  │ Requested Date        : 15 Jan 2024 10:30 AM                            ││
│  │ Priority              : HIGH                                            ││
│  │ Required By           : 18 Jan 2024                                     ││
│  │ Current Status        : PARTIALLY APPROVED                              ││
│  │ Last Updated By       : Branch Manager                                  ││
│  │ Last Updated Date     : 16 Jan 2024 11:40 AM                            ││
│  └─────────────────────────────────────────────────────────────────────────┘│
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐│
│  │ REQUESTED ITEMS WITH STOCK VALIDATION                                   ││
│  │                                                                         ││
│  │ ┌───────────┬──────────┬──────────┬──────────┬────────────┬───────────┐ ││
│  │ │ Product   │Assets Req│Cons. Req │Resell Req│Central Avl │ Status    │ ││
│  │ ├───────────┼──────────┼──────────┼──────────┼────────────┼───────────┤ ││
│  │ │BSP3-001   │ 5        │ 10       │ 0        │A:20,C:50   │ ✅ Avail. │ ││
│  │ │CH-3808-001│ 0        │ 20       │ 0        │A:0,C:15    │ ⚠ Partial │ ││
│  │ │RBS-001    │ 10       │ 0        │ 0        │A:5,C:100   │ ⚠ Partial │ ││
│  │ └───────────┴──────────┴──────────┴──────────┴────────────┴───────────┘ ││
│                                                                             │
│ APPROVED QUANTITIES (Editable)                                              │
│                                                                             │
│ ┌───────────┬───────────┬───────────┬───────────┬────────────────────────┐ │
│ │ Product   │Appr.Assets│Appr.Cons. │Appr.Resell│ Alternative Source     │ │
│ ├───────────┼───────────┼───────────┼───────────┼────────────────────────┤ │
│ │BSP3-001   │ [ 5 ]     │ [ 10 ]    │ [ 0 ]     │ —                      │ │
│ │CH-3808-001│ [ 0 ]     │ [ 15 ]    │ [ 0 ]     │ [Other Branch ▼]       │ │
│ │RBS-001    │ [ 5 ]     │ [ 0 ]     │ [ 0 ]     │ —                      │ │
│ └───────────┴───────────┴───────────┴───────────┴────────────────────────┘ │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐│
│  │ APPROVAL DECISION (Editable)                                            ││
│  │                                                                         ││
│  │ Approval Type*        : [Partial Approval ▼]                            ││
│  │ Alternative Source    : [Other Branch ▼]                                ││
│  │ Dispatch Date*        : [📅 17 Jan 2024]                                ││
│  │ Expected Delivery*    : [📅 20 Jan 2024]                                ││
│  │ Carrier*              : [BlueDart Logistics ▼]                          ││
│  │ LR Number             : [LR93849201]                                    ││
│  │ Asset ID Assignment   : ○ Auto Generate  ○ Manual Assignment           ││
│  │ Remarks*              : [Stock shortage handled via transfer]           ││
│  └─────────────────────────────────────────────────────────────────────────┘│
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐│
│  │ BRANCH TRANSFER PLANNING (Editable if Alternative Source = Other Branch)││
│  │                                                                         ││
│  │ Transfer ID          : TR-2024-00982                                    ││
│  │ From Branch          : Central Warehouse                                ││
│  │ To Branch            : BLR                                              ││
│  │ Transfer Type        : [Regular ▼]                                      ││
│  │ Reference Request    : SR-BLR-2024-001234                               ││
│  │                                                                         ││
│  │ ┌───────────┬──────────────┬───────────┬───────────┬───────────┐      ││
│  │ │ Product   │ SourceBranch │ AssetsQty │ ConsQty   │ ResellQty │      ││
│  │ ├───────────┼──────────────┼───────────┼───────────┼───────────┤      ││
│  │ │CH-3808-001│ HYD          │ 0         │ [5]       │ 0         │      ││
│  │ │RBS-001    │ BOM          │ [5]       │ 0         │ 0         │      ││
│  │ └───────────┴──────────────┴───────────┴───────────┴───────────┘      ││
│                                                                             │
│ Transfer Strategy                                                           │
│ ○ Single Branch Transfer                                                    │
│ ● Split Transfer Across Branches                                            │
│                                                                             │
│  └─────────────────────────────────────────────────────────────────────────┘│
│                                                                             │
│                    [SAVE DRAFT]   [UPDATE APPROVAL]   [CANCEL]              │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# **Section 1 — Request Details Fields**

| Field             | Type     | Required | Values                                |
| ----------------- | -------- | -------- | ------------------------------------- |
| Request ID        | Text     | Auto     | System generated                      |
| Request Type      | Text     | Auto     | Stock Request, Branch Transfer        |
| From Branch       | Text     | Auto     | Branch Code                           |
| Requested By      | Text     | Auto     | Employee Name                         |
| Requested Date    | DateTime | Auto     | System timestamp                      |
| Priority          | Badge    | Auto     | High, Normal, Low                     |
| Required By       | Date     | Auto     | Delivery deadline                     |
| Current Status    | Badge    | Auto     | Approved, Partially Approved, On Hold |
| Last Updated By   | Text     | Auto     | User name                             |
| Last Updated Date | DateTime | Auto     | Timestamp                             |

---

# **Section 2 — Approved Quantities Fields**

| Field              | Type     | Required    | Values                             |
| ------------------ | -------- | ----------- | ---------------------------------- |
| Appr. Assets       | Number   | Conditional | ≥0                                 |
| Appr. Consumables  | Number   | Conditional | ≥0                                 |
| Appr. Resell       | Number   | Conditional | ≥0                                 |
| Alternative Source | Dropdown | Conditional | None, Other Branch, Purchase Order |

---

# **Section 3 — Approval Decision Fields**

| Field               | Type            | Required    | Values                                           |
| ------------------- | --------------- | ----------- | ------------------------------------------------ |
| Approval Type       | Dropdown        | Yes         | Full Approval, Partial Approval, Reject, On Hold |
| Alternative Source  | Dropdown        | Conditional | None, Other Branch, Purchase Order               |
| Dispatch Date       | Date            | Yes         | ≥ Today                                          |
| Expected Delivery   | Date            | Yes         | ≥ Dispatch Date                                  |
| Carrier             | Search Dropdown | Yes         | Data from Carrier Master                         |
| LR Number           | Text            | Optional    | Alphanumeric                                     |
| Asset ID Assignment | Radio           | Conditional | Auto Generate, Manual Assignment                 |
| Remarks             | Text Area       | Yes         | Minimum 10 characters                            |

---

# **Section 4 — Branch Transfer Planning Fields**

| Field             | Type     | Required | Values                        |
| ----------------- | -------- | -------- | ----------------------------- |
| Transfer ID       | Text     | Auto     | TR-YYYY-SEQ                   |
| From Branch       | Text     | Auto     | Central Warehouse             |
| To Branch         | Text     | Auto     | Requesting branch             |
| Transfer Type     | Dropdown | Yes      | Emergency, Regular, Scheduled |
| Reference Request | Text     | Auto     | Request ID                    |

---

# **Branch Transfer Product Fields**

| Field          | Type     | Required    | Values       |
| -------------- | -------- | ----------- | ------------ |
| Product        | Text     | Auto        | Product Code |
| Source Branch  | Dropdown | Yes         | Branch List  |
| Assets Qty     | Number   | Conditional | ≥0           |
| Consumable Qty | Number   | Conditional | ≥0           |
| Resell Qty     | Number   | Conditional | ≥0           |

---

# **Transfer Strategy Options**

| Option                         |
| ------------------------------ |
| Single Branch Transfer         |
| Split Transfer Across Branches |

---

# **System Actions**

| Button          | Function                             |
| --------------- | ------------------------------------ |
| SAVE DRAFT      | Saves changes without final approval |
| UPDATE APPROVAL | Updates the approval decision        |
| CANCEL          | Discards changes                     |

---

# **System Behavior**

### When Updated

- Approval record is modified
- Inventory allocation recalculated
- Dispatch schedule updated
- Transfer plan updated if applicable

### When Saved as Draft

- Status → **Pending Update**
- No stock movement triggered

### Audit Tracking

The system logs:

- Previous approval quantities
- Updated quantities
- User making changes
- Timestamp

---

# **11.3.4 View Request Approval**

## **Description**

The **View Request Approval** screen allows users to view the **complete lifecycle and approval status** of a stock request in a **read-only format**.

This screen provides:

- Request details
- Requested and approved quantities
- Dispatch and logistics details
- Branch transfer information (if applicable)
- **Status timeline tracking**
- **Reviewer and approval summary**
- **Submission information**
- **Audit history**

The screen helps operations teams **track request progress and approval flow across departments**.

---

# **Navigation**

```
Module 11.3 → Received Requests → Action → View
```

---

# **Screen Layout**

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          VIEW REQUEST APPROVAL                               │
│                                                                             │
│  Request ID : SR-BLR-2026-0045                                              │
│  Status     : Pending Approval                                              │
│                                                                             │
│ ───────────────────────── REQUEST TIMELINE ──────────────────────────────── │
│                                                                             │
│  Draft → Submitted → Reviewed → Approved → Dispatch → In Transit → Received│
│                                                                             │
│  Current Stage : Submitted                                                  │
│                                                                             │
│ ───────────────────────── REVIEW INFORMATION ───────────────────────────── │
│                                                                             │
│ Status                 : Pending Approval                                   │
│ Reviewed By            : —                                                  │
│ Review Date            : —                                                  │
│ Approval Level         : Branch Manager                                    │
│                                                                             │
│ IF REJECTED                                                               │
│ Rejection Reason       : —                                                  │
│                                                                             │
│ IF APPROVED                                                               │
│ Approved By            : Head Operations                                   │
│ Approved Date          : 02-Jan-2026                                        │
│                                                                             │
│ ──────────────────────── SUBMISSION INFORMATION ───────────────────────── │
│                                                                             │
│ Requested By           : John Doe                                           │
│ From Branch            : BLR                                                │
│ Sent To                : Branch Manager                                     │
│ Submitted Date         : 02-Jan-2026                                        │
│ Last Updated           : 03-Jan-2026                                        │
│                                                                             │
│ ───────────────────────── REQUEST DETAILS ─────────────────────────────── │
│                                                                             │
│ Request Type           : Stock Request                                      │
│ Priority               : High                                               │
│ Required By            : 05-Jan-2026                                        │
│ Purpose                : Monthly Contract Services                               │
│                                                                             │
│ ───────────────────────── REQUESTED ITEMS ─────────────────────────────── │
│                                                                             │
│ ┌───────────┬──────────┬──────────┬──────────┬────────────┐                 │
│ │ Product   │Assets Req│Cons. Req │Resell Req│ Central Avl│                 │
│ ├───────────┼──────────┼──────────┼──────────┼────────────┤                 │
│ │BSP3-001   │5         │10        │0         │A:20,C:50   │                 │
│ │CH-3808-001│0         │20        │0         │A:0,C:15    │                 │
│ │RBS-001    │10        │0         │0         │A:5,C:100   │                 │
│ └───────────┴──────────┴──────────┴──────────┴────────────┘                 │
│                                                                             │
│ ───────────────────────── APPROVED QUANTITIES ─────────────────────────── │
│                                                                             │
│ ┌───────────┬───────────┬───────────┬───────────┬─────────────────────┐    │
│ │ Product   │Assets Appr│Cons. Appr │Resell Appr│Alternative Source   │    │
│ ├───────────┼───────────┼───────────┼───────────┼─────────────────────┤    │
│ │BSP3-001   │5          │10         │0          │ —                   │    │
│ │CH-3808-001│0          │15         │0          │Other Branch         │    │
│ │RBS-001    │5          │0          │0          │ —                   │    │
│ └───────────┴───────────┴───────────┴───────────┴─────────────────────┘    │
│                                                                             │
│ ───────────────────────── DISPATCH DETAILS ───────────────────────────── │
│                                                                             │
│ Dispatch Date        : 04-Jan-2026                                          │
│ Expected Delivery    : 06-Jan-2026                                          │
│ Carrier              : BlueDart Logistics                                   │
│ LR Number            : LR93849201                                           │
│ Asset Assignment     : Auto Generated                                       │
│ Remarks              : Stock shortage handled via transfer                  │
│                                                                             │
│ ───────────────────── BRANCH TRANSFER DETAILS (If Applicable) ─────────── │
│                                                                             │
│ Transfer ID         : TR-2026-0021                                          │
│ From Branch         : Central Warehouse                                     │
│ To Branch           : BLR                                                   │
│ Transfer Type       : Regular                                               │
│ Transfer Strategy   : Split Transfer                                        │
│                                                                             │
│ ┌───────────┬──────────────┬───────────┬───────────┬───────────┐            │
│ │ Product   │Source Branch │AssetsQty  │ConsQty    │ResellQty  │            │
│ ├───────────┼──────────────┼───────────┼───────────┼───────────┤            │
│ │CH-3808-001│HYD           │0          │5          │0          │            │
│ │RBS-001    │BOM           │5          │0          │0          │            │
│ └───────────┴──────────────┴───────────┴───────────┴───────────┘            │
│                                                                             │
│ ───────────────────────── APPROVAL HISTORY ───────────────────────────── │
│                                                                             │
│ ┌───────────────┬───────────────┬───────────────┬─────────────────────┐     │
│ │ User          │ Role          │ Action        │ Date & Time         │     │
│ ├───────────────┼───────────────┼───────────────┼─────────────────────┤     │
│ │John Doe       │Requester      │Submitted      │02-Jan-2026 09:30 AM │     │
│ │Rajesh Kumar   │Branch Manager │Reviewed       │02-Jan-2026 03:10 PM │     │
│ │Anita Sharma   │Head Operations│Approved       │02-Jan-2026 05:40 PM │     │
│ └───────────────┴───────────────┴───────────────┴─────────────────────┘     │
│                                                                             │
│                                [CLOSE]                                      │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# **Section 1 — Request Timeline**

| Field            | Type            | Description                |
| ---------------- | --------------- | -------------------------- |
| Request Timeline | Visual Progress | Shows lifecycle stages     |
| Current Stage    | Text            | Current status in workflow |

### Timeline Stages

| Stage      |
| ---------- |
| Draft      |
| Submitted  |
| Reviewed   |
| Approved   |
| Dispatch   |
| In Transit |
| Received   |

---

# **Section 2 — Review Information**

| Field            | Type     | Description            |
| ---------------- | -------- | ---------------------- |
| Status           | Badge    | Current request status |
| Reviewed By      | Text     | First reviewer         |
| Review Date      | DateTime | Review timestamp       |
| Approval Level   | Text     | Current approval level |
| Rejection Reason | Text     | Visible if rejected    |
| Approved By      | Text     | Final approver         |
| Approved Date    | Date     | Approval date          |

---

# **Section 3 — Submission Information**

| Field          | Type | Description          |
| -------------- | ---- | -------------------- |
| Requested By   | Text | Request creator      |
| From Branch    | Text | Branch code          |
| Sent To        | Text | Assigned approver    |
| Submitted Date | Date | Submission timestamp |
| Last Updated   | Date | Last modification    |

---

# **Section 4 — Request Details**

| Field        | Type  | Values              |
| ------------ | ----- | ------------------- |
| Request Type | Text  | Stock Request       |
| Priority     | Badge | High / Normal / Low |
| Required By  | Date  | Delivery deadline   |
| Purpose      | Text  | Request purpose     |

---

# **Section 5 — Approval History**

| Field       | Description                                |
| ----------- | ------------------------------------------ |
| User        | Action performer                           |
| Role        | Requester / Manager / Operations           |
| Action      | Submitted / Reviewed / Approved / Rejected |
| Date & Time | Timestamp                                  |

---

# **System Behavior**

### Timeline Update

The system automatically updates the **timeline stage** based on request status.

### Audit Compliance

Every request records:

- Submission
- Review stages
- Approval decisions
- Dispatch information
- Transfer activity

---

## 11.4 Branch Transfer

**Description:** Transfer stock between branches with asset re-assignment, dispatch tracking, and receipt confirmation.

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

## Section 1: Transfer Details

| Field             | Type           | Required | Dropdown Options                              | Validation                   | Description                    |
| ----------------- | -------------- | -------- | --------------------------------------------- | ---------------------------- | ------------------------------ |
| Transfer ID       | Auto Generated | System   | —                                             | Format `TR-YYYY-SEQ`         | Unique identifier for transfer |
| From Branch       | Dropdown       | Yes      | List of branches accessible to logged-in user | Must have available stock    | Source branch                  |
| To Branch         | Dropdown       | Yes      | All active branches                           | Cannot equal **From Branch** | Destination branch             |
| Transfer Type     | Dropdown       | Yes      | Regular / Emergency / Scheduled               | Must select one              | Priority of transfer           |
| Reference Request | Link Selector  | No       | Approved Request IDs                          | Must exist in system         | Linked stock request           |
| Transfer Date     | Date           | Auto     | System date                                   | Read-only                    | Transfer creation date         |
| Created By        | User           | Auto     | Logged-in user                                | Read-only                    | Transfer creator               |

---

# Section 2: Product Selection

This section appears when **Add Product** is clicked.

| Field                 | Type            | Required | Validation           | Description                 |
| --------------------- | --------------- | -------- | -------------------- | --------------------------- |
| Product               | Search Dropdown | Yes      | Only active products | Product to transfer         |
| From Branch Stock     | Number          | Auto     | Read-only            | Total available stock       |
| Assets Available      | Number          | Auto     | Read-only            | Total assets available      |
| Consumables Available | Number          | Auto     | Read-only            | Total consumables available |
| Resell Available      | Number          | Auto     | Read-only            | Total resell stock          |

---

# Section 3: Transfer Quantity Allocation

| Field                   | Type            | Required    | Validation                     | Description                  |
| ----------------------- | --------------- | ----------- | ------------------------------ | ---------------------------- |
| Transfer Assets Qty     | Number          | Conditional | ≤ Assets Available             | Number of assets to transfer |
| Transfer Consumable Qty | Number          | Conditional | ≤ Consumables Available        | Quantity of consumables      |
| Transfer Resell Qty     | Number          | Conditional | ≤ Resell Available             | Quantity of resale items     |
| Total Transfer Qty      | Auto Calculated | System      | Sum of all transfer quantities | Total transfer quantity      |

### Validation Rules

1.

```
Transfer Assets ≤ Assets Available
```

2.

```
Transfer Consumable ≤ Consumable Available
```

3.

```
Transfer Resell ≤ Resell Available
```

4.

At least **one quantity must be > 0**

```
Transfer Assets + Transfer Consumable + Transfer Resell > 0
```

---

# Section 4: Asset Selection (Visible when Assets > 0)

When **Transfer Assets Qty > 0**, the system loads all available assets.

| Field              | Type     | Required | Dropdown Options                                           | Validation                                   | Description                     |
| ------------------ | -------- | -------- | ---------------------------------------------------------- | -------------------------------------------- | ------------------------------- |
| Select Asset       | Checkbox | Yes      | —                                                          | Must select equal to **Transfer Assets Qty** | Asset selection                 |
| Asset ID           | Text     | Auto     | —                                                          | Read-only                                    | Unique asset identifier         |
| Current Assignment | Text     | Auto     | Branch Pool / Employee Name                                | Read-only                                    | Current holder                  |
| Condition          | Dropdown | Yes      | Good / Repair Needed / Damaged                             | Must select                                  | Asset condition before transfer |
| Transfer With      | Dropdown | Yes      | Reassign at Destination / Assign to Employee / Branch Pool | Must select                                  | Asset handling rule             |

---

# Transfer With Options

| Option                  | Description                                                  |
| ----------------------- | ------------------------------------------------------------ |
| Reassign at Destination | Asset will be available for assignment at destination branch |
| Assign to Employee      | Asset directly assigned to an employee at destination        |
| Branch Pool             | Asset added to destination branch inventory                  |

---

# Section 5: Destination Employee Assignment (Conditional)

Visible when **Transfer With = Assign to Employee**

| Field      | Type     | Required | Validation                        | Description              |
| ---------- | -------- | -------- | --------------------------------- | ------------------------ |
| Employee   | Dropdown | Yes      | Must belong to destination branch | Employee receiving asset |
| Department | Auto     | Auto     | Pulled from employee record       | Employee department      |
| Notes      | Text     | Optional | Max 200 chars                     | Additional information   |

---

# Section 6: Product Table (Final Transfer Summary)

| Field                | Type   | Description                         |
| -------------------- | ------ | ----------------------------------- |
| Product Code         | Text   | Product identifier                  |
| Transfer Assets      | Number | Total assets being transferred      |
| Transfer Consumables | Number | Total consumables being transferred |
| Transfer Resell      | Number | Total resell items                  |
| Selected Assets      | Number | Number of selected assets           |

---

# System Validations

### Stock Validation

Before submission:

```
Source Branch Stock ≥ Total Transfer Quantity
```

---

### Asset Selection Validation

```
Selected Assets Count = Transfer Assets Qty
```

If mismatch:

```
Error: Please select the correct number of assets.
```

---

### Branch Validation

```
From Branch ≠ To Branch
```

---

# Actions

| Action                 | Description                   |
| ---------------------- | ----------------------------- |
| Add Product            | Add new product row           |
| Remove Product         | Remove product from transfer  |
| Next: Dispatch Details | Moves to shipment information |
| Cancel                 | Cancel transfer creation      |

---

---

# Workflow of Stock :

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                         MODULE 11: STOCK MANAGEMENT                              │
│                         ─── PURE DATA FLOW ───                                   │
└─────────────────────────────────────────────────────────────────────────────────┘

PHASE 1: CENTRAL PROCUREMENT (Head Operations Only)
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  Product Master │────▶│  Add to Central │────▶│  Asset ID Gen   │
│  (Module 10)    │     │  Stock Form     │     │  (Auto/Manual)  │
└─────────────────┘     └─────────────────┘     └────────┬────────┘
                                                       │
                              ┌────────────────────────┘
                              ▼
                    ┌─────────────────┐
                    │ 3-Way Split:    │
                    │ • Assets (tracked)│
                    │ • Consumables     │
                    │ • Resell          │
                    └────────┬────────┘
                             │
         ┌───────────────────┼───────────────────┐
         ▼                   ▼                   ▼
    ┌─────────┐        ┌─────────┐         ┌─────────┐
    │  Asset  │        │Consumable│         │ Resell  │
    │  Table  │        │  Stock   │         │  Stock  │
    │(Individual│       │ (Bulk)   │         │(For Sale)│
    │  IDs)   │        │          │         │         │
    └────┬────┘        └────┬────┘         └────┬────┘
         │                  │                   │
         └──────────────────┼───────────────────┘
                            ▼
                    ┌─────────────────┐
                    │ Central Warehouse│
                    │    (CEN)         │
                    └────────┬────────┘
                             │
PHASE 2: REQUEST & APPROVAL FLOW                    ┌─────────────────┐
                                                    │  Tax Management │
┌─────────────────┐     ┌─────────────────┐          │  (Module 9)     │
│  Branch User    │────▶│  Stock Request  │◄─────────│  HSN/Tax Calc   │
│  Creates Request│     │  (SR-XXX Form)  │          └─────────────────┘
└─────────────────┘     └────────┬────────┘
                               │
                    ┌──────────┴──────────┐
                    ▼                     ▼
            ┌─────────────┐       ┌─────────────┐
            │  Save Draft │       │   Submit    │
            │ (Invisible) │       │   Request   │
            └─────────────┘       └──────┬──────┘
                                         │
                              ┌──────────┴──────────┐
                              ▼                     ▼
                    ┌─────────────┐       ┌─────────────────────┐
                    │   POPUP:    │       │  Head Operations /  │
                    │   Select    │──────▶│  Branch Manager     │
                    │  Recipients │       │  (Approval Queue)   │
                    └─────────────┘       └──────────┬──────────┘
                                                     │
                                        ┌────────────┼────────────┐
                                        ▼            ▼            ▼
                                  ┌─────────┐  ┌─────────┐  ┌─────────┐
                                  │  HOLD   │  │ REJECT  │  │ APPROVE │
                                  │         │  │         │  │         │
                                  └─────────┘  └─────────┘  └────┬────┘
                                                                  │
PHASE 3: ALLOCATION & DISPATCH                                    ▼
                                                    ┌─────────────────────┐
                                                    │  Stock Validation   │
                                                    │  (Real-time Check)  │
                                                    └──────────┬──────────┘
                                                               │
                                                    ┌──────────┴──────────┐
                                                    ▼                     ▼
                                          ┌─────────────┐       ┌─────────────────┐
                                          │  Available  │       │  Insufficient   │
                                          │  (Full Appr)│       │  (Partial/Alt)  │
                                          └──────┬──────┘       └────────┬────────┘
                                                 │                       │
                                                 ▼                       ▼
                                        ┌─────────────┐         ┌─────────────────┐
                                        │  Dispatch   │         │ Alternative     │
                                        │  Workflow   │         │ Sourcing:       │
                                        │             │         │ • Other Branch  │
                                        │ • Carrier   │         │ • Purchase Order│
                                        │ • LR Number │         │ • Transfer Plan │
                                        │ • Asset Asgn│         └────────┬────────┘
                                        └──────┬──────┘                  │
                                               │                          │
                                               ▼                          ▼
                                        ┌─────────────┐         ┌─────────────────┐
                                        │  In-Transit │         │ Branch Transfer │
                                        │  Status     │         │ Planning (TR)   │
                                        │             │         │                 │
                                        │ Reserved Qty│         │ • Source Branch │
                                        │ Locked      │         │ • Split Strategy│
                                        └──────┬──────┘         └─────────────────┘
                                               │
PHASE 4: RECEIPT & ASSIGNMENT                  │
                                               ▼
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  Destination    │◄────│  Receive Stock  │◄────│  In-Transit     │
│  Branch User    │     │  Form           │     │  Arrival        │
│  (Confirms)     │     │                 │     │                 │
└────────┬────────┘     │ • Verify Qty    │     └─────────────────┘
         │              │ • Check Cond.   │
         │              │ • Asset Assign  │
         │              │ • Upload Photo  │
         │              └────────┬────────┘
         │                       │
         │          ┌────────────┼────────────┐
         │          ▼            ▼            ▼
         │   ┌─────────┐   ┌─────────┐  ┌─────────┐
         │   │  GOOD   │   │ DAMAGED │  │ MISSING │
         │   │ Confirm │   │ Report  │  │ Report  │
         │   └────┬────┘   └────┬────┘  └────┬────┘
         │        │             │            │
         │        ▼             └────────────┘
         │   ┌─────────┐              │
         │   │  Issue  │◄─────────────┘
         │   │ Reported│
         │   │ Status  │
         │   └─────────┘
         │
         ▼
┌─────────────────┐
│  Asset Activated│
│  at Destination │
│  (or Branch Pool)│
└─────────────────┘

PHASE 5: BRANCH TRANSFER (Direct)
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  Source Branch  │────▶│  Create Transfer│────▶│  Asset Selection│
│  (TR-XXX Form)  │     │  (Emergency/    │     │  (If Assets>0)  │
│                 │     │   Regular)      │     │                 │
└─────────────────┘     └────────┬────────┘     └─────────────────┘
                                 │
                    ┌────────────┴────────────┐
                    ▼                         ▼
            ┌─────────────┐           ┌─────────────┐
            │  Dispatch   │           │  Receive    │
            │  (Same as   │──────────▶│  (Same as   │
            │   Phase 3)  │           │   Phase 4)  │
            └─────────────┘           └─────────────┘
```

---

-REAL-WORLD SCENARIO: Complete Lifecycle

- Scenario: BLR Branch needs 10 Brass Sprayers for new client contract starting 25-Feb-2026.

```
TIMELINE:
─────────────────────────────────────────────────────────────────────────────

15-Feb  09:00 AM  Rahul (BLR) creates request → DRAFT
        10:30 AM  Saves draft, adds 2 more items → DRAFT (modified)
        11:15 AM  Submits request → PENDING APPROVAL
                Notification to: Head Ops, Branch Manager

15-Feb  02:45 PM  Head Ops reviews → Stock check: ✅ Available
        03:00 PM  Approves full qty → APPROVED
                Inventory reserved: 10 BSP3 locked at Central

16-Feb  09:30 AM  Warehouse prepares dispatch → DISPATCH
        04:00 PM  Hands to BlueDart, LR-93849201 → IN TRANSIT
                Expected: 20-Feb (4 days)

18-Feb  11:20 AM  BLR receives package
        11:35 AM  Opens "Receive Transfer" form
        11:40 AM  Verifies: 10 units, condition: Good
        11:45 AM  Assigns: 5 to Employee:Amit, 5 to Employee:Priya
        11:50 AM  Uploads receipt photo, clicks CONFIRM → RECEIVED
                Assets activated at BLR. Central stock reduced.

20-Feb  09:00 AM  System auto-archives request. Audit log complete.
─────────────────────────────────────────────────────────────────────────────
```

========================================================================================

# 🎯 MODULE 12: Service Management

Service Management allows administrators to configure pest control services, define pricing models, link chemicals from inventory, configure warranty and revisit policies, and manage treatment methods.

---

# 12.1 Service Dashboard – Table View

**Description:**
The Service Dashboard displays a list of all configured pest control services including their category, pest type, pricing model, service duration, warranty details, and operational status. The dashboard enables administrators to quickly manage services and view service configuration details.

---

# Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           SERVICE DASHBOARD                                  │
│                                                                             │
│  ┌──────────────────────────────── FILTERS ──────────────────────────────┐  │
│  │                                                                       │  │
│  │ Category: [☑ Residential ☑ Commercial]                                │  │
│  │ Pest Type: [☑ Termite ☑ Cockroach ☑ Rodent ☑ Bed Bug ☑ Mosquito]     │  │
│  │ Price Type: [☑ Fixed ☑ Area Based ☑ Inspection]                       │  │
│  │ Status: [☑ Active ☑ Inactive]                                         │  │
│  │                                                                       │  │
│  │ Created Date: [📅 From] - [📅 To]                                      │  │
│  │                                                                       │  │
│  │ Search: [________________________]                                     │  │
│  │        (Service Name / Pest Type / Service ID)                         │  │
│  │                                                                       │  │
│  │ [Reset Filters]                                   [+ ADD SERVICE]     │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│                          SERVICE OVERVIEW TABLE                              │
│                                                                             │
│ ┌─────┬────────────┬─────────────────┬──────────┬────────────┬───────────┐ │
│ │Img  │Service ID  │Service Name     │Category,Sub Category  │Pest Type   │Price Type │ │
│ ├─────┼────────────┼─────────────────┼──────────┼────────────┼───────────┤ │
│ │🐜   │SVC-001     │Termite Control  │Res. ,External     │Termite     │Fixed      │ │
│ │🪳   │SVC-002     │Cockroach Gel    │Comm.     │Cockroach   │Area Based │ │
│ │🐀   │SVC-003     │Rodent Baiting   │Res.      │Rodent      │Inspection │ │
│ └─────┴────────────┴─────────────────┴──────────┴────────────┴───────────┘ │
│                                                                             │
│ ┌──────────┬──────────────┬────────┬──────────────────────────────────────┐ │
│ │Duration  │Warranty      │Status  │Actions                               │ │
│ ├──────────┼──────────────┼────────┼──────────────────────────────────────┤ │
│ │2 Hrs     │6 Mo (2R)     │🟢      │[View] [Edit]                          │ │
│ │1 Hr      │No Warranty   │🟢      │[View] [Edit]                          │ │
│ │3 Hrs     │3 Mo (1R)     │🔴      │[View] [Edit]                          │ │
│ └──────────┴──────────────┴────────┴──────────────────────────────────────┘ │
│                                                                             │
│ Pagination: Previous   1   2   3   ...   10   Next                          │
│                                                                             │
│ Legend: 🟢 Active   🔴 Inactive                                             │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# Table View Fields

| Field         | Type            | Required | Description                       |
| ------------- | --------------- | -------- | --------------------------------- |
| Service Image | Image Thumbnail | Optional | Icon representing pest type       |
| Service ID    | Text            | Auto     | Unique service identifier         |
| Service Name  | Text            | Required | Name of pest control service      |
| Category,Sub Category      | Text,Text            | Required | Residential / Commercial,External/Internal      |
| Pest Type     | Multi Tag       | Required | Pest category linked with service |
| Price Type    | Text            | Required | Fixed / Area Based / Inspection   |
| Duration      | Text            | Required | Service duration                  |
| Warranty      | Text            | Optional | Warranty with revisit count       |
| Status        | Badge           | Auto     | Active / Inactive                 |
| Actions       | Button Group    | —        | View / Edit                       |

---

# Actions

| Action | Description                                             |
| ------ | ------------------------------------------------------- |
| View   | Opens full service details                              |
| Edit   | Allows authorized users to modify service configuration |

---

## Actions (Table Row)

| Action   | Type   | Description                                 |
| -------- | ------ | ------------------------------------------- |
| **View** | Button | Opens service information in read-only mode |
| **Edit** | Button | Allows editing of service configuration     |

---

## Form Actions

| Action          | Description                                                         |
| --------------- | ------------------------------------------------------------------- |
| **Add Service** | Opens the **Add Service Form** to create a new pest control service |

---

# Filters

| Filter       | Type         | Options                                                  |
| ------------ | ------------ | -------------------------------------------------------- |
| Category     | Multi-select | Residential / Commercial                                 |
| Pest Type    | Multi-select | Termite / Cockroach / Rodent / Bed Bug / Mosquito / Ants |
| Price Type   | Multi-select | Fixed Price / Area Based / Inspection                    |
| Status       | Multi-select | Active / Inactive                                        |
| Created Date | Date Range   | From – To                                                |

---

# Search

Searchable by:

- Service ID
- Service Name
- Pest Type

---

# 12.2 Add Service Form

**Description:**
The Add Service form allows administrators to create and configure pest control services. The form captures service information, pest categories, treatment methods, chemicals used, pricing configuration, and warranty policies. The service configuration integrates with the **Product Master (Module 10)** to automatically fetch chemicals, product codes, and units of measure (UOM).

---

# Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Services]                 ADD NEW SERVICE                        [Save] │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│  BASIC SERVICE INFORMATION                                                          │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ Service Name:        [____________________________________] *               │  │
│  │ Example: Rodocon Service, Roachcon Service                                  │  │
│  │                                                                              │  │
│  │ Service Category:                                                            │  │
│  │ [☑] Residential  [☑] Commercial  [☑] Industrial  [☑] Warehouse               │  │
│  │ [+ Add Custom Category]                                                      │  │
│  │                                                                              │  │
│  │ Service Sub Category:                                                        │  │
│  │ [☑] Internal  [☑] External                                                   │  │
│  │                                                                              │  │
│  │ Service Code:        [Auto Generated]                                        │  │
│  │                                                                              │  │
│  │ Pest Type Covered:                                                           │  │
│  │ [☑] Rodent   [☑] Cockroach   [☑] Mosquito                                    │  │
│  │ [☑] Termite  [☑] Fly        [☑] Ant                                           │  │
│  │ [☑] Bed Bug  [☑] Spider     [☑] Lizard                                        │  │
│  │ [+ Add Custom Pest Type]                                                      │  │
│  │                                                                              │  │
│  │ Description:                                                                 │  │
│  │ [____________________________________________________________] *             │  │
│  │                                                                              │  │
│  │ Service Duration: [____] [Minutes / Hours ▼]                                 │  │
│  │                                                                              │  │
│  │ Service Status: (•) Active  ( ) Inactive                                     │  │
│  └──────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                     │
│  PEST SPECIES COVERED                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ Add Specific Pest Species                                                     │  │
│  │                                                                              │  │
│  │ Example:                                                                     │  │
│  │ • Roof Rat – Rattus                                                           │  │
│  │ • Norway Rat – Rattus Norvegicus                                             │  │
│  │ • German Cockroach – Blatella Germanica                                      │  │
│  │                                                                              │  │
│  │ [+ Add Species]                                                              │  │
│  └──────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                     │
│  METHOD OF CONTROL / TREATMENT                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ Available Treatment Methods                                                  │  │
│  │                                                                              │  │
│  │ [☑] Gel Baiting                                                              │  │
│  │ [☑] Rodent Baiting                                                           │  │
│  │ [☑] Residual Spray                                                           │  │
│  │ [☑] Dusting Cracks & Crevices                                                │  │
│  │ [☑] Trapping                                                                 │  │
│  │ [☑] Monitoring                                                               │  │
│  │ [☑] Fogging (Thermal / Cold)                                                 │  │
│  │ [☑] Foam Treatment                                                           │  │
│  │ [☑] Soil Treatment                                                           │  │
│  │ [☑] Sticky Pad Trapping                                                      │  │
│  │ [☑] Disinfection                                                             │  │
│  │ [+ Add Custom Treatment Method]                                              │  │
│  └──────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                     │
│  CHEMICALS / PRODUCTS USED                                                         │
│  (Integrated with Module 10 Product Master — prices auto-fetched on selection)    │
│                                                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ [🔍 Search Product from Product Master...]                                    │  │
│  │                                                                              │  │
│  │ Selected Chemicals                                                           │  │
│  │                                                                              │  │
│  │ ┌──────────────┬──────┬──────┬──────────┬──────────────┬────────────┬──────────────┬──────────────────┬──────────────────┬──────┐ │
│  │ │ Product Name │ Code │ UOM  │ Dilution │Coverage(SQFT)│ Req. Qty   │ Price/UOM(₹) │ Cost/Visit(₹)    │ Est. Cost/Month  │      │ │
│  │ ├──────────────┼──────┼──────┼──────────┼──────────────┼────────────┼──────────────┼──────────────────┼──────────────────┼──────┤ │
│  │ │ Alpha Cyperm.│P-001 │ ml   │ 10 ml    │ [____] SQFT  │ [____] ml  │ ₹4.20 (Auto) │ ₹____ (Auto)     │ ₹____ (Auto)     │ [🗑] │ │
│  │ │ Chlorpyriphos│P-002 │ ml   │ 20 ml    │ [____] SQFT  │ [____] ml  │ ₹3.50 (Auto) │ ₹____ (Auto)     │ ₹____ (Auto)     │ [🗑] │ │
│  │ └──────────────┴──────┴──────┴──────────┴──────────────┴────────────┴──────────────┴──────────────────┴──────────────────┴──────┘ │
│  │                                                                              │
│  │  Visits/Month (Reference): [Auto from Service Frequency above]               │
│  │                                                                              │
│  │  ─── CHEMICAL COST SUMMARY ──────────────────────────────────────────────── │
│  │  Total Chemical Cost / Visit  : ₹ [Auto-sum of all Cost/Visit rows]          │
│  │  Total Chemical Cost / Month  : ₹ [Auto-sum of all Est. Cost/Month rows]     │
│  │  (Based on Visits/Month × Qty × Price/UOM per chemical)                      │
│  │                                                                              │
│  │ [+ Add Custom Chemical]                                                      │
│  └──────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  PRICING CONFIGURATION                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ Price Type                                                                   │  │
│  │ (•) Fixed Price  ( ) Area Based  ( ) Inspection Based                        │  │
│  │                                                                              │  │
│  │ --- Dynamic Form Based on Price Type Selection ---                           │  │
│  │                                                                              │  │
│  │ [IF FIXED PRICE SELECTED]                                                    │  │
│  │ Conditional Pricing (Based on Category & Sub Category)                       │  │
│  │ ▼ Residential (Internal/External)                                            │  │
│  │   1BHK [₹____] 2BHK [₹____] 3BHK [₹____] 4BHK+ [₹____]                       │  │
│  │   [+ Add Custom Property Type]                                               │  │
│  │                                                                              │  │
│  │ ▼ Commercial (Internal/External)                                             │  │
│  │   Small Office [₹____] Medium Office [₹____] Large Office [₹____]            │  │
│  │   Warehouse [₹____]                                                          │  │
│  │   [+ Add Custom Commercial Type]                                             │  │
│  │                                                                              │  │
│  │ [IF AREA BASED SELECTED]                                                     │  │
│  │ Conditional Pricing (Based on Category & Sub Category)                       │  │
│  │ ▼ Residential (Internal/External)                                            │  │
│  │   Base Price: [₹____] + [₹____] per [___]SQFT                                     │  │
│  │                                                                              │  │
│  │ ▼ Commercial (Internal/External)                                             │  │
│  │   Base Price: [₹____] + [₹____] per [___]SQFT                                     │  │
│  │                                                                              │  │
│  │ [IF INSPECTION BASED SELECTED]                                               │  │
│  │ Inspection Fee: [₹____] (Final price quoted after visit)                     │  │
│  │                                                                              │  │
│  │ [+ Add Pricing for Custom Service Category]                                  │  │
│  │   Category: [________▼] Sub Category: [________▼]                            │  │
│  │   Field Name: [____________] Price Type Config: [____] [+ Add Field]         │  │
│  └──────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  WARRANTY / SERVICE GUARANTEE                                                      │
│  ┌──────────────────────────────────────────────────────────────────────────────┐
│  │ Warranty Period: [____] Months                                                │
│  │ Free Revisit Included: [☑] Yes  Qty: [____]                                   │
│  └──────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  SYSTEM FIELDS                                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────────┐
│  │ Service ID: [Auto Generated]                                                  │
│  │ Created Date: [Auto]                                                          │
│  │ Created By: [Admin]                                                           │
│  │ Updated Date: [Auto]                                                          │
│  │ Updated By: [Auto]                                                            │
│  │ Display Order: [____]                                                         │
│  └──────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  [Cancel]                             [Save Draft]                         [Save] │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

---

### **Section 1: Basic Service Information**

| Field                | Type                  | Required | Validation                                          |
| -------------------- | --------------------- | -------- | --------------------------------------------------- |
| Service Name         | Text                  | Yes      | Minimum 3 characters, must be unique                |
| Service Category     | Multi Select Checkbox | Yes      | At least one category must be selected              |
| Custom Category      | Text                  | No       | Appears only if user clicks **Add Custom Category** |
| Service Sub Category | Multi Select Checkbox | Yes      | Internal / External                                 |
| Service Code         | Auto Generated        | Yes      | System generated format: `SRV-XXXX`                 |
| Pest Type Covered    | Multi Select Checkbox | Yes      | Minimum one pest must be selected                   |
| Custom Pest Type     | Text                  | No       | Enabled when **Add Custom Pest Type** selected      |
| Description          | Text Area             | Yes      | Minimum 10 characters                               |
| Service Duration     | Number + UOM          | Yes      | Value must be > 0                                   |
| Service Status       | Radio                 | Yes      | Active / Inactive                                   |

---

### **Section 2: Pest Species Covered**

| Field             | Type   | Required | Validation                         |
| ----------------- | ------ | -------- | ---------------------------------- |
| Pest Species Name | Text   | No       | Must allow multiple entries        |
| Scientific Name   | Text   | No       | Optional scientific classification |
| Add Species       | Button | No       | Adds row dynamically               |

---

### **Section 3: Method of Control / Treatment**

| Field            | Type     | Required | Validation                                     |
| ---------------- | -------- | -------- | ---------------------------------------------- |
| Treatment Method | Checkbox | Yes      | At least one treatment method required         |
| Custom Treatment | Text     | No       | Allowed when user clicks **Add Custom Method** |

---

### **Section 4: Chemicals / Products Used**

_(Integrated with Module 10 Product Master — product price auto-fetched on selection)_

| Field                    | Type            | Required | Validation / Notes                                          |
| ------------------------ | --------------- | -------- | ----------------------------------------------------------- |
| Product Name             | Search Dropdown | Yes      | Must exist in Product Master (active consumables only)      |
| Product Code             | Auto-filled     | System   | Auto-fetched from Product Master on selection               |
| UOM                      | Auto-filled     | System   | Base UOM from Product Master (ml / Ltr / gm / kg / Nos)    |
| Dilution                 | Number          | Yes      | Standard dilution dose per treatment (e.g., 10 ml)         |
| Coverage (SQFT)          | Number          | Yes      | Area covered per dilution dose (e.g., 100 SQFT)            |
| Required Qty             | Number          | Yes      | Quantity needed per service visit; must be > 0              |
| **Price / UOM (₹)**      | Auto-filled     | System   | **Purchase Price from Module 10 Product Master** (editable) |
| **Cost / Visit (₹)**     | Auto-calculated | System   | `Required Qty × Price per UOM`                              |
| **Est. Cost / Month (₹)**| Auto-calculated | System   | `Cost per Visit × Visits per Month`                         |
| Custom Chemical          | Text            | No       | Allowed if product does not exist in Product Master         |

**System Behavior — On Product Selection**

When a product is selected from the search dropdown, the system automatically fetches and fills:

| Field Auto-Filled  | Source in Module 10     |
| ------------------ | ----------------------- |
| Product Code       | Product Master record   |
| UOM                | Base UOM from Product   |
| Dilution           | Standard Usage (if set) |
| Price / UOM (₹)    | **Purchase Price**      |

> The **Price / UOM** is editable — the admin can override the fetched price if a negotiated rate applies for this service.

**Calculation Rules**

| Calculation            | Formula                                          |
| ---------------------- | ------------------------------------------------ |
| Cost per Visit (₹)     | `Required Qty × Price per UOM`                   |
| Est. Cost / Month (₹)  | `Cost per Visit × Visits per Month`              |
| Total Chemical Cost    | Sum of all chemical rows' Est. Cost / Month      |

**Chemical Cost Summary** (displayed below the table, read-only)

| Summary Field                    | Value                                          |
| -------------------------------- | ---------------------------------------------- |
| Total Chemical Cost / Visit (₹)  | Sum of all Cost/Visit across chemical rows     |
| Total Chemical Cost / Month (₹)  | Sum of all Est. Cost/Month across chemical rows |

> This total is shown to guide the admin when setting the final service price in the **Pricing Configuration** section below.

**Example**

| Product             | UOM | Req. Qty | Price/UOM | Cost/Visit | Visits/Month | Cost/Month |
| ------------------- | --- | -------- | --------- | ---------- | ------------ | ---------- |
| Alpha Cypermethrin  | ml  | 50 ml    | ₹4.20     | ₹210       | 4            | ₹840       |
| Chlorpyriphos       | ml  | 30 ml    | ₹3.50     | ₹105       | 4            | ₹420       |
| **TOTAL**           |     |          |           | **₹315**   |              | **₹1,260** |

---

### **Section 5: Pricing Configuration**

> **💡 Pricing Reference:** The **Chemical Cost Summary** calculated in **Section 4** (Total Chemical Cost / Month) is displayed as a reference banner at the top of this section, so the admin can set service prices that comfortably cover chemical costs and ensure profitability.

| Field                          | Type               | Required    | Validation                                                            |
| ------------------------------ | ------------------ | ----------- | --------------------------------------------------------------------- |
| Price Type                     | Radio              | Yes         | Fixed Price / Area Based / Inspection Based                           |
| **[FIXED PRICE CONFIG]**       | —                  | —           | Only if Price Type = Fixed Price                                      |
| Residential (BHK Pricing)      | Number             | Conditional | 1BHK, 2BHK, 3BHK, 4BHK+ pricing; should exceed Chemical Cost/Month  |
| Custom Property Type           | Text               | No          | Added via [+ Add Custom Property Type]                                |
| Commercial (Office Pricing)    | Number             | Conditional | Small, Medium, Large Office, Warehouse pricing                        |
| Custom Commercial Type         | Text               | No          | Added via [+ Add Custom Commercial Type]                              |
| **[AREA BASED CONFIG]**        | —                  | —           | Only if Price Type = Area Based                                       |
| Base Price                     | Number             | Conditional | Minimum charge; recommended ≥ Total Chemical Cost / Visit            |
| Price per SQFT                 | Number             | Conditional | Charge per specified SQFT area                                        |
| **[INSPECTION BASED CONFIG]**  | —                  | —           | Only if Price Type = Inspection Based                                 |
| Inspection Fee                 | Number             | Conditional | Fee charged for visit; final price quoted after inspection            |
| **[CUSTOM CATEGORY CONFIG]**   | —                  | —           | [+ Add Pricing for Custom Service Category]                           |
| Category                       | Dropdown           | No          | Selected from Service Categories                                      |
| Sub Category                   | Dropdown           | No          | Selected from Sub Categories                                          |
| Field Name                     | Text               | No          | Label for the custom pricing field                                    |
| Price Type Config              | Text               | No          | Configuration for the custom pricing                                  |

**Pricing Reference Banner** (shown above the price inputs, read-only)

| Reference Item                       | Value                                         |
| ------------------------------------ | --------------------------------------------- |
| Total Chemical Cost / Visit (₹)      | Auto-calculated from Section 4 chemicals      |
| Total Chemical Cost / Month (₹)      | Auto-calculated from Section 4 chemicals      |
| Recommended Minimum Service Price    | Total Cost / Visit + Labour / Overhead margin |

> Setting the service price **below** the Chemical Cost / Month will trigger a **⚠️ warning** to alert the admin of a potential loss-making configuration.

- For reference check the Preview of Screen Layout (12.2)

---

### **Section 6: Warranty / Service Guarantee**

| Field                 | Type     | Required    | Validation                           |
| --------------------- | -------- | ----------- | ------------------------------------ |
| Warranty Period       | Number   | No          | Value must be ≥ 0                    |
| Free Revisit Included | Checkbox | No          | If checked quantity must be provided |
| Free Revisit Quantity | Number   | Conditional | Required when revisit enabled        |

---

### **Section 7: System Fields**

| Field         | Type   | Required | Validation                        |
| ------------- | ------ | -------- | --------------------------------- |
| Service ID    | Auto   | Yes      | System generated                  |
| Created Date  | Auto   | Yes      | System timestamp                  |
| Created By    | Auto   | Yes      | Logged user                       |
| Updated Date  | Auto   | Yes      | Updated automatically             |
| Updated By    | Auto   | Yes      | Logged user                       |
| Display Order | Number | No       | Used for service listing priority |

---

# 12.3 Edit Service

### **Description**

Allows administrators to **modify existing services**, including pricing, chemicals used, pest types, and treatment methods.

The form structure remains **same as Add Service**, but fields are **pre-populated with existing data**.

---

## **Screen Layout**

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Services]                 EDIT SERVICE                           [Save] │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│  BASIC SERVICE INFORMATION                                                          │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ Service Name:        [Rodocon Service                     ] *               │  │
│  │                                                                              │  │
│  │ Service Category:                                                            │  │
│  │ [☑] Residential  [☑] Commercial  [ ] Industrial  [ ] Warehouse               │  │
│  │ [+ Add Custom Category]                                                      │  │
│  │                                                                              │  │
│  │ Service Sub Category:                                                        │  │
│  │ [☑] Internal  [ ] External                                                   │  │
│  │                                                                              │  │
│  │ Service Code:        SRV-0001 (Read Only)                                    │  │
│  │                                                                              │  │
│  │ Pest Type Covered:                                                           │  │
│  │ [☑] Rodent   [☑] Cockroach   [ ] Mosquito                                    │  │
│  │ [+ Add Custom Pest Type]                                                      │  │
│  │                                                                              │  │
│  │ Description:                                                                 │  │
│  │ [Rodent control treatment using baiting method              ] *             │  │
│  │                                                                              │  │
│  │ Service Duration: [60  ] [Minutes         ▼]                                 │  │
│  │                                                                              │  │
│  │ Service Status: (•) Active  ( ) Inactive                                     │  │
│  └──────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                     │
│  PEST SPECIES COVERED                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ • Roof Rat – Rattus rattus                                   [🗑]             │  │
│  │ • Norway Rat – Rattus norvegicus                             [🗑]             │  │
│  │ [+ Add Species]                                                              │  │
│  └──────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                     │
│  METHOD OF CONTROL / TREATMENT                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ [☑] Gel Baiting      [☑] Trapping                                            │  │
│  │ [☑] Rodent Baiting   [☑] Monitoring                                          │  │
│  │ [+ Add Custom Treatment Method]                                              │  │
│  └──────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                     │
│  CHEMICALS / PRODUCTS USED                                                         │
│  (Integrated with Module 10 Product Master — prices auto-fetched on selection)    │
│                                                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ [🔍 Search Product from Product Master...]                                    │  │
│  │                                                                              │  │
│  │ Selected Chemicals                                                           │  │
│  │                                                                              │  │
│  │ ┌──────────────┬──────┬──────┬──────────┬──────────────┬────────────┬──────────────┬──────────────────┬──────────────────┬──────┐ │
│  │ │ Product Name │ Code │ UOM  │ Dilution │Coverage(SQFT)│ Req. Qty   │ Price/UOM(₹) │ Cost/Visit(₹)    │ Est. Cost/Month  │      │ │
│  │ ├──────────────┼──────┼──────┼──────────┼──────────────┼────────────┼──────────────┼──────────────────┼──────────────────┼──────┤ │
│  │ │ Alpha Cyperm.│P-001 │ ml   │ 10 ml    │ 100 SQFT     │ [20  ] ml  │ ₹4.20 (Auto) │ ₹84 (Auto)       │ ₹336 (Auto)      │ [🗑] │ │
│  │ └──────────────┴──────┴──────┴──────────┴──────────────┴────────────┴──────────────┴──────────────────┴──────────────────┴──────┘ │
│  │                                                                              │
│  │  Visits/Month (Reference): [Auto from Service Frequency above]               │
│  │                                                                              │
│  │  ─── CHEMICAL COST SUMMARY ──────────────────────────────────────────────── │
│  │  Total Chemical Cost / Visit  : ₹ [Auto-sum of all Cost/Visit rows]          │
│  │  Total Chemical Cost / Month  : ₹ [Auto-sum of all Est. Cost/Month rows]     │
│  │  (Based on Visits/Month × Qty × Price/UOM per chemical)                      │
│  │                                                                              │
│  │ [+ Add Custom Chemical]                                                      │
│  └──────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  PRICING CONFIGURATION                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ Price Type                                                                   │  │
│  │ ( ) Fixed Price  (•) Area Based  ( ) Inspection Based                        │  │
│  │                                                                              │  │
│  │ [IF AREA BASED SELECTED]                                                     │  │
│  │ ▼ Residential (Internal)                                                     │  │
│  │   Base Price: [₹500 ] + [₹2   ] per [100]SQFT                                     │  │
│  │                                                                              │  │
│  │ [+ Add Pricing for Custom Service Category]                                  │  │
│  └──────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  WARRANTY / SERVICE GUARANTEE                                                      │
│  ┌──────────────────────────────────────────────────────────────────────────────┐
│  │ Warranty Period: [3   ] Months                                                │
│  │ Free Revisit Included: [☑] Yes  Qty: [2   ]                                   │
│  └──────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  SYSTEM FIELDS                                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────────┐
│  │ Service ID: SRV-0001 (Read Only)                                            │
│  │ Created Date: 02-Feb-2026 (Read Only)                                       │
│  │ Created By: Admin (Read Only)                                                │
│  │ Updated Date: [Auto]                                                          │
│  │ Updated By: [Auto]                                                            │
│  │ Display Order: [3   ]                                                         │
│  └──────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  [Cancel]                             [Save Draft]                         [Save] │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

---

# **Field Editability Matrix**

| Section                   | Field                 | Editable | Notes                                            |
| ------------------------- | --------------------- | -------- | ------------------------------------------------ |
| **Basic Information**     | Service Name          | Yes      | —                                                |
|                           | Service Category      | Yes      | —                                                |
|                           | Service Sub Category  | Yes      | —                                                |
|                           | Service Code          | **No**   | Permanently assigned on creation                 |
|                           | Pest Type Covered     | Yes      | —                                                |
|                           | Description           | Yes      | —                                                |
|                           | Service Duration      | Yes      | —                                                |
|                           | Service Status        | Yes      | Triggers Inactive Reason if changed to Inactive  |
| **Pest Species**          | All Fields            | Yes      | Rows can be added/removed                        |
| **Treatment Methods**     | All Fields            | Yes      | Checkboxes and custom additions                  |
| **Chemicals / Products**  | Product Name          | Yes      | Items can be added/removed; searchable from Product Master |
|                           | Price / UOM (₹)       | Yes      | Auto-fetched from Module 10; editable override   |
|                           | Cost / Visit (₹)      | **Auto** | `Required Qty × Price per UOM`                   |
|                           | Est. Cost / Month (₹) | **Auto** | `Cost per Visit × Visits per Month`              |
| **Pricing Configuration** | Price Type            | Yes      | —                                                |
|                           | Pricing Values        | Yes      | BHK rates, Base price, SQFT rates, etc.          |
|                           | Custom Pricing Config | Yes      | New fields can be added                          |
| **Warranty**              | All Fields            | Yes      | Period and revisits                              |
| **System Fields**         | Service ID            | **No**   | Auto-generated                                   |
|                           | Created Date/By       | **No**   | Fixed upon creation                              |
|                           | Updated Date/By       | **Auto** | Managed by system on save                        |
|                           | Display Order         | Yes      | Used for sorting                                 |
|                           | Inactive Reason       | Yes      | Mandatory only if status becomes Inactive        |

---

# **Inactive Reason Rules**

| Field           | Type      | Required    | Validation                       |
| --------------- | --------- | ----------- | -------------------------------- |
| Service Status  | Radio     | Yes         | Active / Inactive                |
| Inactive Reason | Text Area | Conditional | Mandatory when status = Inactive |
| Inactive Date   | Auto      | Yes         | System timestamp                 |
| Inactivated By  | Auto      | Yes         | Logged user                      |

---

# **Audit Log Tracking**

Every modification to the service must be recorded.

### **Audit Fields**

| Field             | Description                  |
| ----------------- | ---------------------------- |
| Created By        | User who created service     |
| Created Date      | Creation timestamp           |
| Last Updated By   | Last editor                  |
| Last Updated Date | Last modification time       |
| Change Type       | Create / Update / Deactivate |
| Change Notes      | Optional admin comment       |

---

### **Audit Log Example**

| Date        | User  | Action      | Field Changed           |
| ----------- | ----- | ----------- | ----------------------- |
| 02-Feb-2026 | Admin | Created     | New Service Added       |
| 10-Feb-2026 | Admin | Updated     | Price Updated           |
| 15-Feb-2026 | Admin | Deactivated | Service marked inactive |

# **12.4 View Service**

### **Description**

The **View Service** page displays complete details of a configured service in **read-only mode**.

All information entered in **Add Service (12.2)** is shown section-wise for easy review.

Users can:

- Review service configuration
- Check chemicals used
- Review pricing structure
- View treatment methods
- Check audit history

The **Edit button** redirects to **12.3 Edit Service**.

---

# **Screen Layout**

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ [← Back to Services]                    VIEW SERVICE                         [Edit] │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ BASIC SERVICE INFORMATION                                                           │
│ ┌───────────────────────────────────────────────────────────────────────────────┐  │
│ │ Service Name: Rodocon Service                                                 │  │
│ │ Service Code: SRV-0001                                                        │  │
│ │ Service Category: Residential, Commercial                                     │  │
│ │ Service Sub Category: Internal                                                │  │
│ │ Pest Type Covered: Rodent, Cockroach                                          │  │
│ │ Description: Rodent control treatment using baiting method                    │  │
│ │ Service Duration: 60 Minutes                                                  │  │
│ │ Service Status: Active                                                        │  │
│ └───────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                     │
│ PEST SPECIES COVERED                                                               │
│ ┌───────────────────────────────────────────────────────────────────────────────┐  │
│ │ Roof Rat – Rattus rattus                                                      │  │
│ │ Norway Rat – Rattus norvegicus                                                │  │
│ │ German Cockroach – Blattella germanica                                        │  │
│ └───────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                     │
│ METHOD OF CONTROL / TREATMENT                                                      │
│ ┌───────────────────────────────────────────────────────────────────────────────┐  │
│ │ Gel Baiting                                                                   │  │
│ │ Rodent Baiting                                                                │  │
│ │ Trapping                                                                      │  │
│ │ Monitoring                                                                    │  │
│ └───────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                     │
│ CHEMICALS / PRODUCTS USED                                                          │
│ ┌───────────────────────────────────────────────────────────────────────────────┐  │
│ │ Product Name    │ Code  │ UOM │ Dilution │ Coverage │ Req. Qty │ Price/UOM │ Cost/Visit │ Cost/Month │
│ │ Alpha Cyperm.   │ P-001 │ ml  │ 10 ml    │ 100 SQFT │ 20 ml    │ ₹4.20     │ ₹84        │ ₹336       │
│ │ Chlorpyriphos   │ P-002 │ ml  │ 20 ml    │ 100 SQFT │ 30 ml    │ ₹3.50     │ ₹105       │ ₹420       │
│ ├─────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ │ Total Chemical Cost / Visit : ₹189  │  Total Chemical Cost / Month : ₹756                         │
│ └───────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                     │
│ PRICING CONFIGURATION                                                              │
│ ┌───────────────────────────────────────────────────────────────────────────────┐
│ │ Price Type: Fixed Price                                                       │
│ │                                                                               │
│ │ Residential Pricing (Internal)                                                │
│ │ 1BHK: ₹1200 | 2BHK: ₹1800 | 3BHK: ₹2500 | 4BHK+: ₹3200                        │
│ │                                                                               │
│ │ Commercial Pricing (Internal)                                                 │
│ │ Small Office: ₹3000 | Medium Office: ₹5000 | Large Office: ₹8000              │
│ │ Warehouse: ₹12000                                                             │
│ │                                                                               │
│ │ --- Example for Area Based ---                                                │
│ │ Price Type: Area Based                                                        │
│ │ Residential: Base ₹500 + ₹2 per 100 SQFT                                      │
│ │                                                                               │
│ │ --- Example for Inspection Based ---                                          │
│ │ Price Type: Inspection Based                                                  │
│ │ Inspection Fee: ₹500                                                          │
│ └───────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│ WARRANTY / SERVICE GUARANTEE                                                       │
│ ┌───────────────────────────────────────────────────────────────────────────────┐
│ │ Warranty Period: 3 Months                                                     │
│ │ Free Revisit Included: Yes                                                    │
│ │ Number of Revisits: 2                                                         │
│ └───────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│ SYSTEM INFORMATION                                                                 │
│ ┌───────────────────────────────────────────────────────────────────────────────┐
│ │ Service ID: SRV-0001                                                          │
│ │ Created Date: 02-Feb-2026                                                     │
│ │ Created By: Admin                                                             │
│ │ Updated Date: 10-Feb-2026                                                     │
│ │ Updated By: Admin                                                             │
│ │ Display Order: 3                                                              │
│ └───────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│ AUDIT HISTORY                                                                      │
│ ┌───────────────────────────────────────────────────────────────────────────────┐
│ │ Date │ User │ Action │ Notes                                                   │
│ │ 02-Feb-2026 │ Admin │ Created │ New Service Added                              │
│ │ 10-Feb-2026 │ Admin │ Updated │ Pricing Updated                                │
│ │ 15-Feb-2026 │ Admin │ Deactivated │ Service Temporarily Stopped                │
│ └───────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│ [Close]                              [Edit Service]                               │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

---

# **Section 1: Basic Service Information Fields**

| Field                | Description                                       |
| -------------------- | ------------------------------------------------- |
| Service Name         | Name of the service                               |
| Service Code         | Unique auto-generated code                        |
| Service Category     | Residential / Commercial / Industrial / Warehouse |
| Service Sub Category | Internal / External                               |
| Pest Type Covered    | Types of pests handled by the service             |
| Description          | Service description                               |
| Service Duration     | Estimated duration                                |
| Service Status       | Active / Inactive                                 |

---

# **Section 2: Pest Species Covered Fields**

| Field           | Description               |
| --------------- | ------------------------- |
| Species Name    | Name of pest species      |
| Scientific Name | Scientific classification |

---

# **Section 3: Method of Control / Treatment Fields**

| Field            | Description                                 |
| ---------------- | ------------------------------------------- |
| Treatment Method | Methods used to control pests               |
| Custom Method    | Additional treatment methods added manually |

---

# **Section 4: Chemicals / Products Used Fields**

| Field                     | Description                                              |
| ------------------------- | -------------------------------------------------------- |
| Product Name              | Chemical/product name (from Product Master)              |
| Product Code              | Auto-fetched product master code                         |
| UOM                       | Unit of measurement (ml / Ltr / gm / kg / Nos)          |
| Dilution                  | Mixing ratio (e.g., 10 ml per treatment)                 |
| Coverage (SQFT)           | Area covered per dilution dose                           |
| Required Quantity         | Quantity required per service visit                      |
| **Price / UOM (₹)**       | Purchase price from Module 10 (read-only in View)        |
| **Cost / Visit (₹)**      | `Required Qty × Price per UOM` (auto-calculated)         |
| **Est. Cost / Month (₹)** | `Cost per Visit × Visits per Month` (auto-calculated)    |
| **Total Cost / Visit**    | Sum of all chemical Cost/Visit rows (Chemical Summary)   |
| **Total Cost / Month**    | Sum of all chemical Cost/Month rows (Chemical Summary)   |

---

# **Section 5: Pricing Configuration Fields**

| Field                   | Description                                          |
| ----------------------- | ---------------------------------------------------- |
| Price Type              | Fixed / Area Based / Inspection Based                |
| Residential Pricing     | Pricing based on property type (Fixed) or SQFT (Area) |
| Commercial Pricing      | Pricing for offices (Fixed) or SQFT (Area)           |
| Base Price              | Minimum charge for area-based services               |
| Per SQFT Price          | Scaling charge for area-based services               |
| Inspection Fee          | Fee charged for inspection before final quote        |
| Custom Category Pricing | Specific pricing rules for custom service categories |

---

# **Section 6: Warranty / Service Guarantee Fields**

| Field                 | Description                 |
| --------------------- | --------------------------- |
| Warranty Period       | Service guarantee duration  |
| Free Revisit Included | Whether revisit is included |
| Number of Revisits    | Allowed free revisits       |

---

# **Section 7: System Information Fields**

| Field         | Description               |
| ------------- | ------------------------- |
| Service ID    | Unique service identifier |
| Created Date  | Service creation date     |
| Created By    | User who created service  |
| Updated Date  | Last update timestamp     |
| Updated By    | User who updated service  |
| Display Order | Order for listing         |

---

# **Section 8: Audit History Fields**

| Field  | Description                     |
| ------ | ------------------------------- |
| Date   | Action timestamp                |
| User   | User who performed action       |
| Action | Created / Updated / Deactivated |
| Notes  | Admin notes                     |

---

==================================================================================================

# 🎯 MODULE 13: Vendor Management

## Overview

The **Vendor Management Module** maintains all **Supplier and Service Provider records** used by the system.

It stores vendor profile information, contract details, billing configuration, and payment terms.

This module allows the organization to:

- Maintain centralized vendor records
- Track vendor contract types
- Manage payment terms and billing configuration
- Monitor vendor status and rating
- Filter vendors based on operational parameters

---

## Module Connections

### Depends On

- Procurement Module
- Inventory Module
- Finance / Accounting Module

### Used By

- Purchase Orders
- Service Procurement
- Contract Management
- Vendor Payments
- Inventory Supply Tracking

---

## Key Features

- Centralized vendor database
- Vendor contract tracking
- Billing configuration management
- Payment terms management
- Vendor performance rating
- Advanced filtering and search
- Vendor status control
- Vendor audit tracking

---

Below is the **refactored version of Module 13 Vendor Management** based on your requirement:

- Fix mismatch in **Vendor Create Form**
- Add **dynamic contract logic**
- If vendor **has contract → fetch products + recurring billing + stock supply**
- If vendor **no contract → simple vendor profile**
- Maintain **documentation style same as your module file**.
  Base structure referenced from Vendor module section.

---

# **13.1 Vendor Dashboard – Table View**

## **Description**

The **Vendor Dashboard** provides a centralized view of all vendors registered in the system.

The table allows procurement teams, finance teams, and administrators to:

- View vendor contract status
- Identify vendors linked with **products or services**
- Monitor **billing configuration and payment terms**
- Track **vendor status and rating**
- Search and filter vendors for procurement operations

This screen acts as the **primary entry point for vendor management**.

---

# Screen Layout

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                             VENDOR MANAGEMENT                                 │
│                                                                              │
│  FILTER PANEL                                                                │
│                                                                              │
│ Vendor Type:      [☑ Supplier ☑ Service Provider ☑ Both]                    │
│ Vendor Category:  [▼ Chemical / Equipment / Service / Logistics ▼]          │
│ Contract Type:    [☑ Annual ☑ Project ☑ One Time ☑ No Contract]             │
│ Billing Type:     [☑ Per Service ☑ Monthly ☑ Project]                       │
│ Vendor Status:    [☑ Active ☑ Inactive ☑ Blocked]                           │
│ Vendor Rating:    [⭐ 1+ to ⭐ 5]                                            │
│ State:            [Multi Select]                                             │
│ Contract End:     [📅 From] - [📅 To]                                        │
│                                                                              │
│ Search: [_____________________________________________]                     │
│ (Vendor Name / Vendor ID / Contact / GST / Product)                         │
│                                                                              │
│ [Reset Filters]                                     [+ ADD VENDOR]          │
│                                                                              │
│────────────────────────────────────────────────────────────────────────────│
│ VENDOR TABLE                                                                 │
│                                                                              │
│ID│Vendor Name│Category│Type│Contract│Products│Billing│Payment│Status│Action │
│──┼───────────┼────────┼────┼────────┼────────┼───────┼───────┼──────┼───────│
│V101│ABC Chem │Chemical│Supp│Annual  │12      │Monthly│Net30  │Active│View/Edit│
│V102│TechServ │Service │SP  │Project │5       │Service│Advance│Active│View/Edit│
│V103│EquipPro │Equip   │Both│None    │—       │—      │Net15  │Active│View/Edit│
│                                                                              │
│Pagination:  Previous   1   2   3   ...   Next                                │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

# Table Fields

| Field           | Type     | Required | Description                                |
| --------------- | -------- | -------- | ------------------------------------------ |
| Vendor ID       | Auto ID  | System   | Unique vendor identifier                   |
| Vendor Name     | Text     | Yes      | Vendor company name                        |
| Vendor Category | Dropdown | Yes      | Chemical / Equipment / Service / Logistics |
| Vendor Type     | Dropdown | Yes      | Supplier / Service Provider / Both         |
| Contract Type   | Badge    | Yes      | Annual / Project / One Time / None         |
| Linked Products | Number   | Auto     | Count of products/services under contract  |
| Billing Type    | Text     | Yes      | Per Service / Monthly / Project            |
| Payment Terms   | Text     | Yes      | Advance / Net 15 / Net 30 / Net 45         |
| Vendor Status   | Badge    | Auto     | Active / Inactive / Blocked                |
| Vendor Rating   | Rating   | Optional | Performance rating                         |
| GST Number      | Text     | Optional | Vendor tax identification                  |
| Created Date    | Date     | Auto     | Record creation date                       |
| Created By      | Text     | Auto     | User who created vendor                    |
| Actions         | Button   | —        | View / Edit                                |

---

# Filters

| Filter            | Type          | Options                                    |
| ----------------- | ------------- | ------------------------------------------ |
| Vendor Type       | Multi Select  | Supplier / Service Provider / Both         |
| Category          | Dropdown      | Chemical / Equipment / Service / Logistics |
| Contract Type     | Multi Select  | Annual / Project / One Time / None         |
| Status            | Multi Select  | Active / Inactive / Blocked                |
| Rating            | Rating Filter | ⭐1+ to ⭐5                                |
| State             | Multi Select  | State List                                 |
| Contract End Date | Date Range    | From – To                                  |

---

# Search

Searchable fields:

- Vendor ID
- Vendor Name
- Contact Person
- Phone Number
- Email
- GST Number
- Product Name

---

# Table Actions

| Action | Description              |
| ------ | ------------------------ |
| View   | Open full vendor profile |
| Edit   | Modify vendor details    |

---

# **13.2 Add Vendor – Vendor Registration Screen**

## **Description**

The **Add Vendor Screen** allows administrators or procurement managers to register a new vendor in the system.

This screen captures complete vendor information including:

- Vendor basic information
- Address details
- Tax and compliance information
- Bank details
- Contract details
- Product supply configuration
- Billing configuration
- Payment terms
- Vendor performance details

The system supports **two vendor configurations**:

### **1. Contract Vendor**

Vendors supplying **products under contract terms**.

The system enables:

- Product supply mapping
- Quantity and UOM configuration
- Delivery schedule
- Billing configuration
- Payment terms

### **2. Non-Contract Vendor**

Used for vendors that provide:

- One-time procurement
- Ad-hoc purchases

Only **basic vendor details** are required.

When **Has Contract = No**, the following sections remain hidden:

- Contract Details
- Product Supply Configuration
- Billing Configuration

---

# **Screen Layout**

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                                ADD NEW VENDOR                                │
└──────────────────────────────────────────────────────────────────────────────┘


SECTION 1: BASIC INFORMATION
──────────────────────────────────────────────────────────────────────────────

Vendor ID (Auto Generated)

Vendor Name*
Vendor Type*                   [ Supplier / Service Provider / Both ]
Vendor Category*               [ Chemical Supplier / Equipment Vendor / Logistics Vendor ]

Product Supplied*
Contact Person*
Phone Number*
Email ID*

Vendor Status*                 [ Active / Inactive ]

Has Contract?*                 [ Yes / No ]


SECTION 2: ADDRESS DETAILS
──────────────────────────────────────────────────────────────────────────────

Address*
City*
State*
Pincode*
Country*


SECTION 3: TAX & COMPLIANCE
──────────────────────────────────────────────────────────────────────────────

Vendor Registration Type*      [ Registered / Unregistered ]

GST Number                     (Conditional)
PAN Number                     (Optional)


SECTION 4: BANK DETAILS
──────────────────────────────────────────────────────────────────────────────

Bank Name
Account Holder Name
Account Number
IFSC Code


SECTION 5: CONTRACT DETAILS
(Visible if Has Contract = YES)
──────────────────────────────────────────────────────────────────────────────

Contract Type*                 [ Annual / Project / One Time ]

Contract Start Date*
Contract End Date

SLA Agreement                  [ Yes / No ]

Contract Document Upload


SECTION 6: PRODUCT SUPPLY CONFIGURATION
(Visible if Has Contract = YES)
──────────────────────────────────────────────────────────────────────────────

[ + ADD PRODUCT ]


┌──────────────────────────────── PRODUCT SUPPLY TABLE ────────────────────────────────┐
│ Product │ Category │ Supply Qty │ UOM │ Unit Rate │ Min Order Qty │ Delivery Frequency │ Lead Time │ Tax │ Action │
│─────────┼──────────┼────────────┼─────┼───────────┼───────────────┼────────────────────┼───────────┼─────┼────────│
│ Termite Chemical │ Chemical │ 50 │ Liters │ ₹1200 │ 10 │ Monthly │ 3 Days │ Yes │ Edit/Delete │
│ Rodent Bait │ Chemical │ 25 │ Kg │ ₹600 │ 5 │ Quarterly │ 5 Days │ Yes │ Edit/Delete │
└───────────────────────────────────────────────────────────────────────────────────────┘


SECTION 7: BILLING CONFIGURATION
──────────────────────────────────────────────────────────────────────────────

Billing Type*                  [ Per Service / Monthly / Project ]

Billing Cycle                  [ Weekly / Monthly / Quarterly / Custom ]

Custom Billing Start Date
Custom Billing End Date

Invoice Submission Method      [ Email / Portal / Physical ]


SECTION 8: PAYMENT TERMS
──────────────────────────────────────────────────────────────────────────────

Payment Terms*                 [ Advance / Net15 / Net30 / Net45 ]

Advance Payment %
Late Payment Penalty


SECTION 9: PERFORMANCE TRACKING
──────────────────────────────────────────────────────────────────────────────

Vendor Rating
Remarks


[ SAVE VENDOR ]                [ CANCEL ]
```

---

# **SECTION 1: BASIC INFORMATION – Fields**

| Field            | Field Type        | Required | Options                                                                              | Notes                                   |
| ---------------- | ----------------- | -------- | ------------------------------------------------------------------------------------ | --------------------------------------- |
| Vendor ID        | Auto Generated    | System   | —                                                                                    | Unique system generated ID              |
| Vendor Name      | Text              | Yes      | —                                                                                    | Vendor company name                     |
| Vendor Type      | Dropdown          | Yes      | Supplier / Service Provider / Both                                                   | Defines vendor role                     |
| Vendor Category  | Dropdown          | Yes      | Chemical Supplier / Equipment Vendor / Logistics Vendor / Maintenance Vendor / Other | Vendor classification                   |
| Product Supplied | Text              | Yes      | —                                                                                    | Product or service description          |
| Contact Person   | Text              | Yes      | —                                                                                    | Vendor representative                   |
| Phone Number     | Phone             | Yes      | —                                                                                    | 10 digit number                         |
| Email ID         | Email             | Yes      | —                                                                                    | Vendor email                            |
| Vendor Status    | Dropdown          | Yes      | Active / Inactive                                                                    | Vendor operational status               |
| Has Contract     | Toggle / Dropdown | Yes      | Yes / No                                                                             | Determines contract sections visibility |

---

# **SECTION 2: ADDRESS DETAILS – Fields**

| Field   | Field Type | Required | Options                 | Notes             |
| ------- | ---------- | -------- | ----------------------- | ----------------- |
| Address | Textarea   | Yes      | —                       | Vendor address    |
| City    | Text       | Yes      | —                       | City name         |
| State   | Dropdown   | Yes      | State List              | System state list |
| Pincode | Number     | Yes      | —                       | 6 digit           |
| Country | Dropdown   | Yes      | India (Default) / Other | Default India     |

---

# **SECTION 3: TAX & COMPLIANCE – Fields**

| Field                    | Field Type | Required    | Options                   | Notes                      |
| ------------------------ | ---------- | ----------- | ------------------------- | -------------------------- |
| Vendor Registration Type | Dropdown   | Yes         | Registered / Unregistered | Determines GST requirement |
| GST Number               | Text       | Conditional | —                         | Required if Registered     |
| PAN Number               | Text       | Optional    | —                         | Vendor PAN                 |

---

# **SECTION 4: BANK DETAILS – Fields**

| Field               | Field Type | Required | Options | Notes            |
| ------------------- | ---------- | -------- | ------- | ---------------- |
| Bank Name           | Text       | Optional | —       | Vendor bank      |
| Account Holder Name | Text       | Optional | —       | Name as per bank |
| Account Number      | Number     | Optional | —       | 9–18 digits      |
| IFSC Code           | Text       | Optional | —       | Valid IFSC       |

---

# **SECTION 5: CONTRACT DETAILS – Fields**

| Field                    | Field Type  | Required    | Options                     | Notes                    |
| ------------------------ | ----------- | ----------- | --------------------------- | ------------------------ |
| Contract Type            | Dropdown    | Yes         | Annual / Project / One Time | Vendor contract type     |
| Contract Start Date      | Date Picker | Yes         | —                           | Contract start           |
| Contract End Date        | Date Picker | Conditional | —                           | Must be after start date |
| SLA Agreement            | Dropdown    | No          | Yes / No                    | Service agreement        |
| Contract Document Upload | File Upload | Optional    | PDF / DOCX                  | Max 10MB                 |

---

# **SECTION 6: PRODUCT SUPPLY CONFIGURATION – Fields**

| Field                  | Field Type      | Required | Options                               | Notes                    |
| ---------------------- | --------------- | -------- | ------------------------------------- | ------------------------ |
| Product                | Search Dropdown | Yes      | Product Master List                   | Linked to Module 10      |
| Product Category       | Auto            | System   | —                                     | Auto from Product Master |
| Supply Quantity        | Number          | Yes      | —                                     | Default supply quantity  |
| UOM                    | Dropdown        | Yes      | From Product Master UOM list          | Unit of measurement      |
| Unit Supply Rate       | Currency        | Yes      | —                                     | Vendor product price     |
| Minimum Order Quantity | Number          | Optional | —                                     | MOQ for purchase         |
| Delivery Frequency     | Dropdown        | Yes      | Weekly / Monthly / Quarterly / Custom | Delivery schedule        |
| Delivery Lead Time     | Number          | Optional | —                                     | Delivery days            |
| Tax Applicable         | Dropdown        | Yes      | Yes / No                              | GST applicable           |

---

# **SECTION 7: BILLING CONFIGURATION – Fields**

| Field                     | Field Type | Required    | Options                               | Notes                    |
| ------------------------- | ---------- | ----------- | ------------------------------------- | ------------------------ |
| Billing Type              | Dropdown   | Yes         | Per Service / Monthly / Project       | Billing structure        |
| Billing Cycle             | Dropdown   | Optional    | Weekly / Monthly / Quarterly / Custom | Recurring billing        |
| Custom Billing Start Date | Date       | Conditional | —                                     | Required if cycle custom |
| Custom Billing End Date   | Date       | Conditional | —                                     | Must be after start date |
| Invoice Submission Method | Dropdown   | Optional    | Email / Portal / Physical             | Default Email            |

---

# **SECTION 8: PAYMENT TERMS – Fields**

| Field                | Field Type | Required | Options                         | Notes                |
| -------------------- | ---------- | -------- | ------------------------------- | -------------------- |
| Payment Terms        | Dropdown   | Yes      | Advance / Net15 / Net30 / Net45 | Vendor payment terms |
| Advance Payment %    | Number     | Optional | —                               | Percentage advance   |
| Late Payment Penalty | Text       | Optional | —                               | Financial penalty    |

---

# **SECTION 9: PERFORMANCE TRACKING – Fields**

| Field         | Field Type | Required | Options | Notes              |
| ------------- | ---------- | -------- | ------- | ------------------ |
| Vendor Rating | Rating     | Optional | ⭐1–⭐5 | Performance rating |
| Remarks       | Text Area  | Optional | —       | Admin notes        |

---

# **Validation Rules**

| Validation           | Description                              |
| -------------------- | ---------------------------------------- |
| Mandatory Fields     | Fields marked with \* cannot be empty    |
| Phone Number         | Must contain exactly 10 digits           |
| Email Format         | Must follow valid email format           |
| GST Format           | Must match GSTIN pattern                 |
| PAN Format           | Must match PAN format                    |
| Contract End Date    | Must be greater than Contract Start Date |
| Billing Custom Dates | Required if Billing Cycle = Custom       |
| Supply Quantity      | Must be greater than 0                   |
| Unit Supply Rate     | Must be positive number                  |

---

# **13.3 Edit Vendor – Vendor Update Screen**

---

# **Screen Layout**

```id="zj9z9i"
┌──────────────────────────────────────────────────────────────────────────────┐
│                                 EDIT VENDOR                                  │
└──────────────────────────────────────────────────────────────────────────────┘


SECTION 1: BASIC INFORMATION
──────────────────────────────────────────────────────────────────────────────

Vendor ID                       (Read Only)

Vendor Name
Vendor Type
Vendor Category

Product Supplied
Contact Person
Phone Number
Email ID

Vendor Status

Has Contract


SECTION 2: ADDRESS DETAILS
──────────────────────────────────────────────────────────────────────────────

Address
City
State
Pincode
Country


SECTION 3: TAX & COMPLIANCE
──────────────────────────────────────────────────────────────────────────────

Vendor Registration Type
GST Number
PAN Number


SECTION 4: BANK DETAILS
──────────────────────────────────────────────────────────────────────────────

Bank Name
Account Holder Name
Account Number
IFSC Code


SECTION 5: CONTRACT DETAILS
──────────────────────────────────────────────────────────────────────────────

Contract Type
Contract Start Date
Contract End Date

SLA Agreement
Contract Document Upload


SECTION 6: PRODUCT SUPPLY CONFIGURATION
──────────────────────────────────────────────────────────────────────────────

Product Supply Table

Product | Category | Supply Qty | UOM | Unit Rate | MOQ | Delivery Frequency | Lead Time | Tax | Action


SECTION 7: BILLING CONFIGURATION
──────────────────────────────────────────────────────────────────────────────

Billing Type
Billing Cycle

Custom Billing Start Date
Custom Billing End Date

Invoice Submission Method


SECTION 8: PAYMENT TERMS
──────────────────────────────────────────────────────────────────────────────

Payment Terms
Advance Payment %
Late Payment Penalty


SECTION 9: PERFORMANCE TRACKING
──────────────────────────────────────────────────────────────────────────────

Vendor Rating
Remarks


[ UPDATE VENDOR ]                [ CANCEL ]
```

---

# **SECTION 1: BASIC INFORMATION – Fields**

| Field            | Editable | Notes                                           |
| ---------------- | -------- | ----------------------------------------------- |
| Vendor ID        | No       | System generated identifier                     |
| Vendor Name      | Yes      | Editable if no active contracts linked          |
| Vendor Type      | No       | Changing may affect procurement configuration   |
| Vendor Category  | Yes      | Allowed if not used in category filtering rules |
| Product Supplied | Yes      | Informational field                             |
| Contact Person   | Yes      | Contact detail update allowed                   |
| Phone Number     | Yes      | Vendor contact update                           |
| Email ID         | Yes      | Vendor contact update                           |
| Vendor Status    | Yes      | Active / Inactive / Blocked                     |
| Has Contract     | No       | Cannot change once contract exists              |

---

# **SECTION 2: ADDRESS DETAILS – Fields**

| Field   | Editable | Notes                   |
| ------- | -------- | ----------------------- |
| Address | Yes      | Operational information |
| City    | Yes      | Vendor location         |
| State   | Yes      | Vendor location         |
| Pincode | Yes      | Address update          |
| Country | Yes      | Default India           |

---

# **SECTION 3: TAX & COMPLIANCE – Fields**

| Field                    | Editable | Notes                          |
| ------------------------ | -------- | ------------------------------ |
| Vendor Registration Type | No       | Impacts tax calculation        |
| GST Number               | Yes      | Editable if vendor GST changes |
| PAN Number               | Yes      | Compliance update              |

---

# **SECTION 4: BANK DETAILS – Fields**

| Field               | Editable | Notes                 |
| ------------------- | -------- | --------------------- |
| Bank Name           | Yes      | Payment configuration |
| Account Holder Name | Yes      | Vendor bank details   |
| Account Number      | Yes      | Financial account     |
| IFSC Code           | Yes      | Bank identification   |

---

# **SECTION 5: CONTRACT DETAILS – Fields**

| Field                    | Editable | Notes                            |
| ------------------------ | -------- | -------------------------------- |
| Contract Type            | No       | Impacts billing configuration    |
| Contract Start Date      | No       | Locked after contract activation |
| Contract End Date        | Yes      | Can extend contract              |
| SLA Agreement            | Yes      | Operational configuration        |
| Contract Document Upload | Yes      | Updated contract file            |

---

# **SECTION 6: PRODUCT SUPPLY CONFIGURATION – Fields**

| Field                  | Editable | Notes                         |
| ---------------------- | -------- | ----------------------------- |
| Product                | No       | Linked to procurement history |
| Product Category       | No       | Auto from Product Master      |
| Supply Quantity        | Yes      | Vendor supply configuration   |
| UOM                    | No       | Defined in Product Master     |
| Unit Supply Rate       | Yes      | Vendor pricing update         |
| Minimum Order Quantity | Yes      | Procurement flexibility       |
| Delivery Frequency     | Yes      | Delivery scheduling           |
| Delivery Lead Time     | Yes      | Vendor logistics update       |
| Tax Applicable         | Yes      | GST applicability             |

Actions:

| Action      | Description                           |
| ----------- | ------------------------------------- |
| Add Product | Allowed if vendor contract active     |
| Edit Row    | Update supply configuration           |
| Delete Row  | Allowed if product not used in orders |

---

# **SECTION 7: BILLING CONFIGURATION – Fields**

| Field                     | Editable | Notes                           |
| ------------------------- | -------- | ------------------------------- |
| Billing Type              | No       | Impacts financial configuration |
| Billing Cycle             | Yes      | Billing schedule adjustment     |
| Custom Billing Start Date | Yes      | If custom cycle                 |
| Custom Billing End Date   | Yes      | Billing configuration           |
| Invoice Submission Method | Yes      | Email / Portal / Physical       |

---

# **SECTION 8: PAYMENT TERMS – Fields**

| Field                | Editable | Notes                 |
| -------------------- | -------- | --------------------- |
| Payment Terms        | Yes      | Financial agreement   |
| Advance Payment %    | Yes      | Payment configuration |
| Late Payment Penalty | Yes      | Financial policy      |

---

# **SECTION 9: PERFORMANCE TRACKING – Fields**

| Field         | Editable | Notes                        |
| ------------- | -------- | ---------------------------- |
| Vendor Rating | Yes      | Updated based on performance |
| Remarks       | Yes      | Administrative notes         |

---

# **System Rules**

1. Vendor ID cannot be edited.
2. Vendor type cannot be changed after vendor creation.
3. Contract structure cannot be modified once active.
4. Products already used in **purchase orders cannot be removed**.
5. Vendor status **Inactive or Blocked prevents new procurement transactions**.

---

# **Audit Log Tracking**

Every modification to the vendor record is tracked.

| Field          | Description        |
| -------------- | ------------------ |
| Updated By     | User making change |
| Updated Date   | Timestamp          |
| Field Changed  | Modified field     |
| Previous Value | Old value          |
| New Value      | Updated value      |

---

Below is **Module 13.4 – View Vendor Details Screen** written in the **same specification style as 13.2 and 13.3**, but since it is a **view screen**, all fields are **read-only** and meant only for **information display** without allowing modification.

---

# **13.4 View Vendor – Vendor Detail Screen**

# **Screen Layout**

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                              VENDOR DETAILS                                  │
└──────────────────────────────────────────────────────────────────────────────┘


SECTION 1: BASIC INFORMATION
──────────────────────────────────────────────────────────────────────────────

Vendor ID
Vendor Name
Vendor Type
Vendor Category
Product Supplied

Contact Person
Phone Number
Email ID

Vendor Status
Vendor Rating
Has Contract


SECTION 2: ADDRESS DETAILS
──────────────────────────────────────────────────────────────────────────────

Address
City
State
Pincode
Country


SECTION 3: TAX & COMPLIANCE
──────────────────────────────────────────────────────────────────────────────

Vendor Registration Type
GST Number
PAN Number


SECTION 4: BANK DETAILS
──────────────────────────────────────────────────────────────────────────────

Bank Name
Account Holder Name
Account Number
IFSC Code


SECTION 5: CONTRACT DETAILS
──────────────────────────────────────────────────────────────────────────────

Contract Type
Contract Start Date
Contract End Date
SLA Agreement
Contract Document


SECTION 6: PRODUCT SUPPLY DETAILS
──────────────────────────────────────────────────────────────────────────────

┌──────────────────────────── PRODUCT SUPPLY TABLE ─────────────────────────────┐
│ Product │ Category │ Supply Qty │ UOM │ Unit Rate │ MOQ │ Delivery Frequency │
│─────────┼──────────┼────────────┼─────┼───────────┼─────┼────────────────────│
│ Termite Chemical │ Chemical │ 50 │ Liters │ ₹1200 │ 10 │ Monthly │
│ Rodent Bait │ Chemical │ 25 │ Kg │ ₹600 │ 5 │ Quarterly │
└──────────────────────────────────────────────────────────────────────────────┘


SECTION 7: BILLING CONFIGURATION
──────────────────────────────────────────────────────────────────────────────

Billing Type
Billing Cycle
Custom Billing Start Date
Custom Billing End Date
Invoice Submission Method


SECTION 8: PAYMENT TERMS
──────────────────────────────────────────────────────────────────────────────

Payment Terms
Advance Payment %
Late Payment Penalty


SECTION 9: PERFORMANCE DETAILS
──────────────────────────────────────────────────────────────────────────────

Vendor Rating
Remarks


SECTION 10: AUDIT HISTORY
──────────────────────────────────────────────────────────────────────────────

┌──────────────────────────── AUDIT LOG TABLE ─────────────────────────────┐
│ Date │ User │ Action │ Field Changed │ Old Value │ New Value │
│──────┼──────┼────────┼───────────────┼───────────┼───────────│
│ 02 Feb 2026 │ Admin │ Created │ Vendor Created │ — │ — │
│ 10 Feb 2026 │ Admin │ Updated │ Contact Person │ A Kumar │ R Kumar │
│ 15 Feb 2026 │ Admin │ Updated │ Payment Terms │ Net15 │ Net30 │
└─────────────────────────────────────────────────────────────────────────┘


[ BACK TO VENDOR LIST ]
```

---

# **SECTION 1: BASIC INFORMATION – Fields**

| Field            | Field Type | Editable | Notes                              |
| ---------------- | ---------- | -------- | ---------------------------------- |
| Vendor ID        | Text       | No       | System generated                   |
| Vendor Name      | Text       | No       | Vendor company name                |
| Vendor Type      | Text       | No       | Supplier / Service Provider / Both |
| Vendor Category  | Text       | No       | Vendor classification              |
| Product Supplied | Text       | No       | Product description                |
| Contact Person   | Text       | No       | Vendor representative              |
| Phone Number     | Phone      | No       | Vendor contact                     |
| Email ID         | Email      | No       | Vendor email                       |
| Vendor Status    | Badge      | No       | Active / Inactive / Blocked        |
| Vendor Rating    | Rating     | No       | Vendor performance                 |
| Has Contract     | Text       | No       | Yes / No                           |

---

# **SECTION 2: ADDRESS DETAILS – Fields**

| Field   | Field Type | Editable | Notes          |
| ------- | ---------- | -------- | -------------- |
| Address | Text       | No       | Vendor address |
| City    | Text       | No       | Vendor city    |
| State   | Text       | No       | Vendor state   |
| Pincode | Number     | No       | Postal code    |
| Country | Text       | No       | Default India  |

---

# **SECTION 3: TAX & COMPLIANCE – Fields**

| Field                    | Field Type | Editable | Notes                     |
| ------------------------ | ---------- | -------- | ------------------------- |
| Vendor Registration Type | Text       | No       | Registered / Unregistered |
| GST Number               | Text       | No       | Vendor GST                |
| PAN Number               | Text       | No       | Vendor PAN                |

---

# **SECTION 4: BANK DETAILS – Fields**

| Field               | Field Type | Editable | Notes             |
| ------------------- | ---------- | -------- | ----------------- |
| Bank Name           | Text       | No       | Vendor bank       |
| Account Holder Name | Text       | No       | Bank account name |
| Account Number      | Number     | No       | Vendor account    |
| IFSC Code           | Text       | No       | Bank code         |

---

# **SECTION 5: CONTRACT DETAILS – Fields**

| Field               | Field Type | Editable | Notes                       |
| ------------------- | ---------- | -------- | --------------------------- |
| Contract Type       | Text       | No       | Annual / Project / One Time |
| Contract Start Date | Date       | No       | Contract start              |
| Contract End Date   | Date       | No       | Contract expiry             |
| SLA Agreement       | Text       | No       | Yes / No                    |
| Contract Document   | File Link  | No       | Downloadable file           |

---

# **SECTION 6: PRODUCT SUPPLY DETAILS – Fields**

| Field                  | Field Type | Editable | Notes                        |
| ---------------------- | ---------- | -------- | ---------------------------- |
| Product                | Text       | No       | Product name                 |
| Product Category       | Text       | No       | From Product Master          |
| Supply Quantity        | Number     | No       | Default supply quantity      |
| UOM                    | Text       | No       | Unit of measure              |
| Unit Supply Rate       | Currency   | No       | Vendor price                 |
| Minimum Order Quantity | Number     | No       | Procurement limit            |
| Delivery Frequency     | Text       | No       | Weekly / Monthly / Quarterly |
| Delivery Lead Time     | Number     | No       | Delivery days                |

---

# **SECTION 7: BILLING CONFIGURATION – Fields**

| Field                     | Field Type | Editable | Notes                           |
| ------------------------- | ---------- | -------- | ------------------------------- |
| Billing Type              | Text       | No       | Per Service / Monthly / Project |
| Billing Cycle             | Text       | No       | Weekly / Monthly / Quarterly    |
| Custom Billing Start Date | Date       | No       | Custom billing start            |
| Custom Billing End Date   | Date       | No       | Custom billing end              |
| Invoice Submission Method | Text       | No       | Email / Portal / Physical       |

---

# **SECTION 8: PAYMENT TERMS – Fields**

| Field                | Field Type | Editable | Notes                           |
| -------------------- | ---------- | -------- | ------------------------------- |
| Payment Terms        | Text       | No       | Advance / Net15 / Net30 / Net45 |
| Advance Payment %    | Number     | No       | Advance percentage              |
| Late Payment Penalty | Text       | No       | Financial rule                  |

---

# **SECTION 9: PERFORMANCE DETAILS – Fields**

| Field         | Field Type | Editable | Notes                |
| ------------- | ---------- | -------- | -------------------- |
| Vendor Rating | Rating     | No       | Performance score    |
| Remarks       | Text Area  | No       | Administrative notes |

---

# **SECTION 10: AUDIT HISTORY**

The system records all vendor record activities.

| Field         | Description                        |
| ------------- | ---------------------------------- |
| Date          | Timestamp of activity              |
| User          | User who performed action          |
| Action        | Created / Updated / Status Changed |
| Field Changed | Modified field                     |
| Old Value     | Previous value                     |
| New Value     | Updated value                      |

---

==================================================================================================

# 🎯 MODULE 14: Purchase Order

## Overview

The **Purchase Order (PO) Module** manages the formal process of procuring goods or services from vendors. It tracks the entire lifecycle from PO generation and approval to vendor fulfillment and stock receipt.

This module ensures transparent procurement, budget control, and seamless integration with inventory management.

---

## Module Connections

### Depends On

- **Module 13: Vendor Management**: For supplier details, payment terms, and product mapping.
- **Module 10: Product Master**: For selecting catalog items and SKUs.
- **Module 9: Tax Management**: For HSN/SAC and GST calculations.

### Used By

- **Module 11: Stock Management**: Received PO items trigger "Add to Central Stock" workflows.
- **Finance Module**: For invoice matching and accounts payable.

---

# **14.1 Purchase Order – Table View**

## **Description**

Displays all Purchase Orders with their current lifecycle status.
Used to track procurement from creation → approval → ordering → receiving.

---

## **Screen Layout**

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│                          PURCHASE ORDER DASHBOARD                            │
│                                                                              │
│ Vendor ▼ | Status ▼ | Date Range 📅 | Branch ▼ | [Reset]                      │
│                                                                              │
│ [+ CREATE PURCHASE ORDER]                                                    │
│                                                                              │
│ PURCHASE ORDER TABLE                                                         │
│ ┌──────────────────────────────────────────────────────────────────────────┐ │
│ │PO ID │Vendor │PO Date │Delivery │Items│Total Amount│Status │Action       │ │
│ │──────┼───────┼────────┼─────────┼─────┼────────────┼────────┼────────────│ │
│ │PO-001│ABC Ltd│12 Mar  │15 Mar   │  5  │ ₹50,000    │Draft   │[Edit][Del] │ │
│ │PO-002│XYZ Co │13 Mar  │18 Mar   │  3  │ ₹20,000    │Approved│[View]      │ │
│ │PO-003│ABC Ltd│14 Mar  │20 Mar   │  8  │ ₹75,000    │Ordered │[View]      │ │
│ └──────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│ Pagination: Previous 1 2 3 ... Next                                           │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## **Table Fields**

| Field Name    | Type     | Description                      |
| ------------- | -------- | -------------------------------- |
| PO ID         | Text     | Unique Purchase Order identifier |
| Vendor        | Text     | Vendor selected for the PO       |
| PO Date       | Date     | Date of PO creation              |
| Delivery Date | Date     | Expected delivery date           |
| Items Count   | Number   | Total number of product lines    |
| Total Amount  | Currency | Grand total including taxes      |
| Status        | Badge    | Current PO lifecycle status      |
| Action        | Buttons  | View / Edit / Delete / Download  |

---

## **Filters (Professional & Minimal)**

| Filter Name | Type            | Description                |
| ----------- | --------------- | -------------------------- |
| Vendor      | Dropdown Search | Filter PO by vendor        |
| Status      | Dropdown        | Filter by lifecycle status |
| Date Range  | Date Picker     | Filter based on PO Date    |
| Branch      | Dropdown        | Filter by branch           |

---

## **Actions**

| Action | Available When           | Description               |
| ------ | ------------------------ | ------------------------- |
| View   | All statuses             | Open PO in read-only mode |
| Edit   | Draft / Pending Approval | Modify PO details         |
| Delete | Draft only               | Remove PO permanently     |

---

## **Status Indicators**

| Status             | Meaning              |
| ------------------ | -------------------- |
| Draft              | PO not finalized     |
| Pending Approval   | Waiting for approval |
| Approved           | Approved internally  |
| Ordered            | Sent to vendor       |
| Partially Received | Some items received  |
| Received           | Fully received       |
| Cancelled          | PO cancelled         |

---

## **Notes (ERP Behavior)**

- Once PO reaches **Ordered**, editing and deletion are restricted
- **Status-driven control** ensures data integrity
- Table structure aligns with:
  - Stock (Module 11)
  - Services (Module 12)
  - Vendors (Module 13)

---

# **14.2 Create Purchase Order**

## **Description**

Create a Purchase Order by selecting a vendor and adding products.
System auto-fetches vendor, product, and tax details while the user provides required inputs.

---

## **Screen Layout**

```text id="po14create_refined"
┌──────────────────────────────────────────────────────────────────────────────┐
│                         CREATE PURCHASE ORDER                                │
│                                                                              │
│  [← Back to PO List]                                                         │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 1: PO BASIC INFO                                                     │
├──────────────────────────────────────────────────────────────────────────────┤
│ PO Number           : AUTO GENERATED                                         │
│ PO Date *           : [📅 Select Date]                                        │
│ Status *            : Draft (Default)                                         │
│ GST Number (PO) *   : AUTO FETCH (Company)                                    │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 2: VENDOR DETAILS (Module 13)                                        │
├──────────────────────────────────────────────────────────────────────────────┤
│ Vendor Name *       : [Search ▼]                                              │
│ Vendor Address *    : AUTO FETCH                                              │
│ Vendor GST *        : AUTO FETCH                                              │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 3: ITEM DETAILS                                                      │
├──────────────────────────────────────────────────────────────────────────────┤
│ [ + ADD ITEM ]                                                               │
│                                                                              │
│ ┌──────────────────────────────────────────────────────────────────────────┐ │
│ │Sl│Product │Qty│UOM│Price│GST%│Tax Amt│Total Amt│Action                    │ │
│ │──┼────────┼───┼───┼─────┼────┼───────┼─────────┼───────────────           │ │
│ │1 │[▼]     │ 5 │Auto│100 │18% │ 90    │ 590     │[Remove]                  │ │
│ └──────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│ ➤ On Product Select → Auto Fetch all product details (hidden metadata)       │
│ ➤ No duplicate fields shown (clean ERP design)                               │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 4: SUMMARY                                                           │
├──────────────────────────────────────────────────────────────────────────────┤
│ Subtotal            : AUTO CALCULATED                                         │
│ Total Tax           : AUTO CALCULATED                                         │
│ Grand Total *       : AUTO CALCULATED                                         │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 5: DELIVERY DETAILS                                                  │
├──────────────────────────────────────────────────────────────────────────────┤
│ Delivery Address *  : [Text Area]                                             │
│ Contact Person *    : [Text]                                                  │
│ Contact Number *    : [Number]                                                │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 6: AUTHORIZATION                                                     │
├──────────────────────────────────────────────────────────────────────────────┤
│ Authorized Person * : Logged-in User                                          │
│ Designation *       : [Text]                                                  │
│ Note                : Thank you                                               │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ ACTIONS                                                                      │
├──────────────────────────────────────────────────────────────────────────────┤
│ [ SAVE AS DRAFT ]   [ SUBMIT FOR APPROVAL ]   [ CANCEL ]                      │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## **Section 1: PO Basic Info**

| Field           | Field Type  | Required | Options                    | Notes                                        |
| --------------- | ----------- | -------- | -------------------------- | -------------------------------------------- |
| PO Number       | Auto Number | Yes      | System Generated           | Unique PO ID (Read-only)                     |
| PO Date         | Date Picker | Yes      | Past / Current Date        | Cannot be future date                        |
| Status          | Dropdown    | Yes      | Draft (Default), Submitted | Controlled by system workflow                |
| GST Number (PO) | Text (Auto) | Yes      | Company GST                | Auto fetched from Company Master (Read-only) |

---

## **Section 2: Vendor Details**

| Field          | Field Type      | Required | Options            | Notes                     |
| -------------- | --------------- | -------- | ------------------ | ------------------------- |
| Vendor Name    | Search Dropdown | Yes      | Vendor Master List | On select → fetch details |
| Vendor Address | Text (Auto)     | Yes      | From Vendor Master | Read-only                 |
| Vendor GST     | Text (Auto)     | Yes      | From Vendor Master | Read-only                 |

---

## **Section 3: Item Details (Row-Based Table)**

| Field          | Field Type       | Required | Options               | Notes                                       |
| -------------- | ---------------- | -------- | --------------------- | ------------------------------------------- |
| Product        | Dropdown         | Yes      | Product Master        | On select → auto fetch UOM, Price, GST      |
| Quantity       | Number           | Yes      | > 0                   | User input                                  |
| UOM            | Text (Auto)      | Yes      | From Product Master   | Read-only                                   |
| Purchase Price | Number           | Yes      | ≥ 0                   | Auto-filled, editable (based on permission) |
| GST %          | Dropdown (Auto)  | Yes      | 0%, 5%, 12%, 18%, 28% | Auto from Tax/HSN                           |
| Tax Amount     | Calculated Field | Yes      | System Calculated     | Qty × Price × GST %                         |
| Total Amount   | Calculated Field | Yes      | System Calculated     | (Qty × Price) + Tax                         |
| Action         | Button           | No       | Add / Remove Row      | Manage items dynamically                    |

---

## **Section 4: Summary**

| Field       | Field Type       | Required | Options | Notes                        |
| ----------- | ---------------- | -------- | ------- | ---------------------------- |
| Subtotal    | Calculated Field | Yes      | System  | Sum of all item base amounts |
| Total Tax   | Calculated Field | Yes      | System  | Sum of all tax values        |
| Grand Total | Calculated Field | Yes      | System  | Final payable amount         |

---

## **Section 5: Delivery Details**

| Field            | Field Type | Required | Options         | Notes                      |
| ---------------- | ---------- | -------- | --------------- | -------------------------- |
| Delivery Address | Text Area  | Yes      | Manual Entry    | Can be prefilled if needed |
| Contact Person   | Text       | Yes      | Manual Entry    | Receiver name              |
| Contact Number   | Number     | Yes      | 10-digit Mobile | Numeric only               |

---

## **Section 6: Authorization**

| Field             | Field Type  | Required | Options          | Notes                |
| ----------------- | ----------- | -------- | ---------------- | -------------------- |
| Authorized Person | Text (Auto) | Yes      | Logged-in User   | System captured      |
| Designation       | Text        | Yes      | Manual Entry     | Role of user         |
| Note              | Text Area   | No       | Default / Custom | Default: “Thank you” |

---

# **Validations (Professional ERP Standard)**

## **General Validations**

- All mandatory (\*) fields must be filled before submission
- System should prevent submission if no items are added
- At least one item row is required

---

## **PO Basic Info**

- PO Date cannot be a future date
- Status cannot be manually changed beyond allowed workflow
- GST Number must be valid format (GSTIN validation)

---

## **Vendor Details**

- Vendor must be selected from master (no manual entry)
- Vendor GST must be valid and active
- Vendor must not be inactive/blocked

---

## **Item Details**

- Product selection is mandatory per row
- Quantity must be greater than 0
- Purchase Price cannot be negative
- GST % must match predefined tax slabs
- Duplicate product entries should be allowed/blocked based on business rule
- Total Amount auto recalculates on any change

---

## **System / Workflow Validations**

- Draft → Editable
- Submitted → Locked for editing
- Only authorized roles can approve/reject
- Audit log must capture:
  - Created By
  - Created Date
  - Last Updated By
  - Last Updated Date

---

# **14.3 Edit Purchase Order**

## **Description**

Allows modification of Purchase Order based on status.
Editing is restricted to maintain data integrity after submission and approval.

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                         EDIT PURCHASE ORDER                                  │
│                                                                              │
│  [← Back to PO List]     [PO Number: PO-XXXX]     [Status: Draft/Approved]    │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 1: PO BASIC INFO                                                     │
├──────────────────────────────────────────────────────────────────────────────┤
│ PO Number           : AUTO GENERATED (READ-ONLY)                              │
│ PO Date *           : [📅 Select Date] (Editable if Draft)                    │
│ Status *            : Draft / Submitted / Approved (Controlled)              │
│ GST Number (PO) *   : AUTO FETCH (READ-ONLY)                                  │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 2: VENDOR DETAILS                                                    │
├──────────────────────────────────────────────────────────────────────────────┤
│ Vendor Name *       : [Search ▼] (Editable if Draft)                          │
│ Vendor Address *    : AUTO FETCH (READ-ONLY)                                  │
│ Vendor GST *        : AUTO FETCH (READ-ONLY)                                  │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 3: ITEM DETAILS                                                      │
├──────────────────────────────────────────────────────────────────────────────┤
│ [ + ADD ITEM ] (Only in Draft)                                                │
│                                                                              │
│ ┌──────────────────────────────────────────────────────────────────────────┐ │
│ │Sl│Product │Qty│UOM│Price│GST%│Tax Amt│Total Amt│Action                    │ │
│ │──┼────────┼───┼───┼─────┼────┼───────┼─────────┼───────────────           │ │
│ │1 │[▼]     │ 5 │Auto│100 │18% │ 90    │ 590     │[Remove]                  │ │
│ └──────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│ ➤ Product / Qty / Price editable only in Draft                               │
│ ➤ UOM, GST%, Tax Amt, Total Amt → READ-ONLY                                  │
│ ➤ Row Add/Remove disabled after submission                                   │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 4: SUMMARY                                                           │
├──────────────────────────────────────────────────────────────────────────────┤
│ Subtotal            : AUTO CALCULATED (READ-ONLY)                             │
│ Total Tax           : AUTO CALCULATED (READ-ONLY)                             │
│ Grand Total *       : AUTO CALCULATED (READ-ONLY)                             │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 5: DELIVERY DETAILS                                                  │
├──────────────────────────────────────────────────────────────────────────────┤
│ Delivery Address *  : [Text Area] (Editable if Draft)                         │
│ Contact Person *    : [Text] (Editable if Draft)                              │
│ Contact Number *    : [Number] (Editable if Draft)                            │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 6: AUTHORIZATION                                                     │
├──────────────────────────────────────────────────────────────────────────────┤
│ Authorized Person * : Logged-in User (READ-ONLY)                              │
│ Designation *       : [Text] (Editable if Draft)                              │
│ Note                : [Text Area] (Editable)                                  │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 7: AUDIT TRAIL                                                       │
├──────────────────────────────────────────────────────────────────────────────┤
│ Last Edited By     : User ID                                                  │
│ Last Edited Date   : DD-MM-YYYY HH:MM                                         │
│ Changed Fields     : Field1, Field2, Field3                                   │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ ACTIONS                                                                      │
├──────────────────────────────────────────────────────────────────────────────┤
│ [ UPDATE ]   [ SAVE AS DRAFT ]   [ CANCEL ]                                   │
│                                                                              │
│ ➤ If Status = Draft → Full Actions                                           │
│ ➤ If Submitted/Approved → Limited / Disabled                                 │
└──────────────────────────────────────────────────────────────────────────────┘
```

## **Field Editability**

### **Section 1: PO Basic Info**

| Field           | Editable   | Notes                                    |
| --------------- | ---------- | ---------------------------------------- |
| PO Number       | ❌ No      | System generated, always read-only       |
| PO Date         | ✅ Yes     | Editable only in Draft                   |
| Status          | ⚠️ Limited | Can change based on workflow permissions |
| GST Number (PO) | ❌ No      | Auto from Company, not editable          |

---

### **Section 2: Vendor Details**

| Field          | Editable | Notes                   |
| -------------- | -------- | ----------------------- |
| Vendor Name    | ✅ Yes   | Editable only in Draft  |
| Vendor Address | ❌ No    | Auto from Vendor Master |
| Vendor GST     | ❌ No    | Auto from Vendor Master |

---

### **Section 3: Item Details**

| Field          | Editable | Notes                                   |
| -------------- | -------- | --------------------------------------- |
| Product        | ✅ Yes   | Editable only in Draft                  |
| Quantity       | ✅ Yes   | Editable only in Draft                  |
| UOM            | ❌ No    | Auto from Product Master                |
| Purchase Price | ✅ Yes   | Editable in Draft (based on permission) |
| GST %          | ❌ No    | Auto from Tax/HSN                       |
| Tax Amount     | ❌ No    | System calculated                       |
| Total Amount   | ❌ No    | System calculated                       |
| Action (Row)   | ✅ Yes   | Add/Remove allowed only in Draft        |

---

### **Section 4: Summary**

| Field       | Editable | Notes           |
| ----------- | -------- | --------------- |
| Subtotal    | ❌ No    | Auto calculated |
| Total Tax   | ❌ No    | Auto calculated |
| Grand Total | ❌ No    | Auto calculated |

---

### **Section 5: Delivery Details**

| Field            | Editable | Notes                  |
| ---------------- | -------- | ---------------------- |
| Delivery Address | ✅ Yes   | Editable only in Draft |
| Contact Person   | ✅ Yes   | Editable only in Draft |
| Contact Number   | ✅ Yes   | Editable only in Draft |

---

### **Section 6: Authorization**

| Field             | Editable | Notes                            |
| ----------------- | -------- | -------------------------------- |
| Authorized Person | ❌ No    | Logged-in user (system captured) |
| Designation       | ✅ Yes   | Editable in Draft                |
| Note              | ✅ Yes   | Editable until Approved          |

---

# **Status-Based Editing Rules**

| Status             | Editing Allowed |
| ------------------ | --------------- |
| Draft              | Full Edit       |
| Pending Approval   | ❌ No Edit      |
| Approved           | ⚠️ Status only  |
| Ordered            | ❌ Locked       |
| Partially Received | ❌ Locked       |
| Received           | ❌ Closed       |
| Cancelled          | ❌ Closed       |

---

# **Audit Trail (Mandatory)**

Every edit must be tracked for compliance and history.

| Field           | Value Description                |
| --------------- | -------------------------------- |
| Edited By       | Logged-in User ID                |
| Edited Date     | System Timestamp                 |
| Changed Fields  | List of modified fields          |
| Previous Values | Stored before update (for audit) |

---

# **14.3 View Purchase Order**

## **Description**

Displays complete Purchase Order details in **read-only mode**.
Provides full visibility of vendor, items, pricing, and audit history without allowing any modification.

---

## **Screen Layout**

```text id="po14view_refined"
┌──────────────────────────────────────────────────────────────────────────────┐
│                         VIEW PURCHASE ORDER                                  │
│                                                                              │
│  [← Back to PO List]     [PO Number: PO-XXXX]     [Status: Approved]          │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 1: PO BASIC INFO                                                     │
├──────────────────────────────────────────────────────────────────────────────┤
│ PO Number           : PO-XXXX                                                 │
│ PO Date             : DD-MM-YYYY                                              │
│ Status              : Draft / Submitted / Approved                            │
│ GST Number (PO)     : 22AAAAA0000A1Z5                                         │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 2: VENDOR DETAILS                                                    │
├──────────────────────────────────────────────────────────────────────────────┤
│ Vendor Name         : ABC Suppliers                                           │
│ Vendor Address      : Full Address Display                                    │
│ Vendor GST          : 27BBBBB1111B2Z6                                         │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 3: ITEM DETAILS                                                      │
├──────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│ ┌──────────────────────────────────────────────────────────────────────────┐ │
│ │Sl│Product │Qty│UOM│Price│GST%│Tax Amt│Total Amt                          │ │
│ │──┼────────┼───┼───┼─────┼────┼───────┼─────────                          │ │
│ │1 │Item A  │ 5 │Nos│100 │18% │ 90    │ 590                               │ │
│ │2 │Item B  │ 2 │Nos│200 │18% │ 72    │ 472                               │ │
│ └──────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│ ➤ All values displayed as saved (no editing)                                  │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 4: SUMMARY                                                           │
├──────────────────────────────────────────────────────────────────────────────┤
│ Subtotal            : XXXX                                                    │
│ Total Tax           : XXXX                                                    │
│ Grand Total         : XXXX                                                    │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 5: DELIVERY DETAILS                                                  │
├──────────────────────────────────────────────────────────────────────────────┤
│ Delivery Address    : Full Address                                            │
│ Contact Person      : Name                                                    │
│ Contact Number      : 9876543210                                              │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 6: AUTHORIZATION                                                     │
├──────────────────────────────────────────────────────────────────────────────┤
│ Authorized Person   : User Name                                               │
│ Designation         : Manager                                                 │
│ Note                : Thank you                                               │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ SECTION 7: AUDIT TRAIL                                                       │
├──────────────────────────────────────────────────────────────────────────────┤
│ Created By          : User ID                                                 │
│ Created Date        : DD-MM-YYYY HH:MM                                        │
│ Last Edited By      : User ID                                                 │
│ Last Edited Date    : DD-MM-YYYY HH:MM                                        │
│ Change History      : View Full Log [🔍]                                       │
└──────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────────────────┐
│ ACTIONS                                                                      │
├──────────────────────────────────────────────────────────────────────────────┤
│ [ EDIT ]   [ PRINT ]   [ DOWNLOAD PDF ]   [ CLOSE ]                           │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## **Fields (Name / Description)**

### **Section 1: PO Basic Info**

| Field           | Description                  |
| --------------- | ---------------------------- |
| PO Number       | Unique Purchase Order ID     |
| PO Date         | Date when PO was created     |
| Status          | Current workflow status      |
| GST Number (PO) | Company GST used for this PO |

---

### **Section 2: Vendor Details**

| Field          | Description               |
| -------------- | ------------------------- |
| Vendor Name    | Selected vendor name      |
| Vendor Address | Vendor registered address |
| Vendor GST     | Vendor GST number         |

---

### **Section 3: Item Details**

| Field        | Description                |
| ------------ | -------------------------- |
| Product      | Product name               |
| Quantity     | Ordered quantity           |
| UOM          | Unit of Measurement        |
| Price        | Purchase price per unit    |
| GST %        | Applicable tax percentage  |
| Tax Amount   | Calculated tax value       |
| Total Amount | Final amount including tax |

---

### **Section 4: Summary**

| Field       | Description          |
| ----------- | -------------------- |
| Subtotal    | Total before tax     |
| Total Tax   | Total tax amount     |
| Grand Total | Final payable amount |

---

### **Section 5: Delivery Details**

| Field            | Description          |
| ---------------- | -------------------- |
| Delivery Address | Delivery location    |
| Contact Person   | Receiver name        |
| Contact Number   | Contact phone number |

---

### **Section 6: Authorization**

| Field             | Description                  |
| ----------------- | ---------------------------- |
| Authorized Person | User who created/approved PO |
| Designation       | Role of the user             |
| Note              | Additional remarks           |

---

### **Section 7: Audit Trail**

| Field            | Description                 |
| ---------------- | --------------------------- |
| Created By       | User who created PO         |
| Created Date     | Creation timestamp          |
| Last Edited By   | Last modified user          |
| Last Edited Date | Last modification timestamp |
| Change History   | Full audit log access       |

---

# **14.4 Delete action PopUP(permanent delete)**

```
┌──────────────────────────────────────────────────────────────────────────────┐
│ DELETE PURCHASE ORDER                                                        │
├──────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ⚠️  Are you sure you want to permanently delete this Purchase Order?        │
│                                                                              │
│  ┌────────────────────────────────────────────────────────────────────────┐ │
│  │ [ DELETE ]                                [ CANCEL ]                     │ │
│  └────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```



===================================================================================================


# 🎯 MODULE 15: LEADS & FOLLOW-UP MANAGEMENT

## Overview

Comprehensive lead lifecycle management module that captures potential customer inquiries, tracks follow-up activities, and converts qualified leads into quotations or service contracts. Manages the complete journey from initial contact through qualification, nurturing, and conversion to GMA (General Measurement & Assessment) or direct quotation.

**Module Connections:**

- **Depends on:** Module 8 (Employee Management for lead assignment), Module 10 (Product Master for quotation creation)
- **Used by:** Module 17 (GMA Management), Module 16 (Quotation Management), Module 18 (Contract Management)

---

# 15.1 Lead Dashboard – Table View

**Description:**
Centralized lead management interface displaying all leads with filtering, search, and quick action capabilities. Provides real-time visibility into lead pipeline, status distribution, and follow-up requirements.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           LEAD MANAGEMENT                                      │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ FILTERS                                                                │  │
│  │                                                                        │  │
│  │ Lead Status:     [☑ New ☑ Qualified ☑ Quotation send                 │  │
│  │                  ☑ Negotiation ☑ Lost ☑ Converted]                   │  │
│  │ Lead Source:     [☑ Website ☑ Referral ☑ Walk-in ☑ Cold Call         │  │
│  │                  ☑ Social Media ☑ Exhibition ☑ Partner]               │  │
│  │ Priority:        [☑ Low ☑ Normal ☑ High ☑ Urgent]                    │  │
│  │ Assigned To:     [▼ All Sales Reps ▼]                                 │  │
│  │ Category:        [☑ Residential ☑ Commercial ☑ Industrial]           │  │
│  │ Pest Type:       [☑ Termite ☑ Cockroach ☑ Rodent ☑ Bed Bug           │  │
│  │                  ☑ Mosquito ☑ Ant ☑ Other]                            │  │
│  │ Date Range:      [📅 From] - [📅 To]                                  │  │
│  │                                                                        │  │
│  │ Search: [________________________] (Lead ID / Name / Mobile / Email) │  │
│  │                                                                        │  │
│  │ [Reset Filters]                                    [+ ADD NEW LEAD]   │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  LEAD OVERVIEW TABLE                                                         │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │Lead ID│Lead Name│Contact Info│Pest Type │Status│Priority│Next F/U Date│ │
│  │───────┼─────────┼────────────┼──────────┼──────┼────────┼─────────────│ │
│  │LD-001 │Rahul S. │98XXXX1234  │Termite   │🟡 Qual.│High    │20-Mar       │ │
│  │LD-002 │Priya K. │99XXXX5678  │Cockroach │🟢 Quot.│Normal  │22-Mar       │ │
│  │LD-003 │Amit V.  │97XXXX9012  │Rodent    │🔴 New  │Urgent  │18-Mar (Ovr) │ │
│  └────────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Source│Created Date│Last F/U Date│Actions         ││
│  │──────┼────────────┼─────────────┼────────────────││
│  │Website│15-Mar-2026 │18-Mar-2026  │[👁 View] [✏ Edit]│
│  │Refer. │14-Mar-2026 │19-Mar-2026  │[➕ Follow-up]    ││
│  │Walk-in│10-Mar-2026 │—            │[👁 View] [✏ Edit]│
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   10   Next                           │
│                                                                              │
│  Legend: 🔵 New  🟡 Qualified  🟠 Quotation send  🟣 Negotiation  ⚫ Lost/Converted │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table View Fields

| Field          | Type           | Required | Description                                                       |
| -------------- | -------------- | -------- | ----------------------------------------------------------------- |
| Lead ID        | Text           | Auto     | Unique lead identifier (LD-YYYY-SEQ)                              |
| Lead Name      | Text           | Yes      | Primary contact person name                                       |
| Contact Info   | Text           | Yes      | Mobile number and email                                           |
| Lead Type      | Badge          | Yes      | Product / Service / Both                                          |
| Status         | Status Badge   | Yes      | New / Qualified / Quotation send / Negotiation / Lost / Converted |
| GMA Status     | Status Badge   | Yes      | Draft / Under review / Approved / Rejected                        |
| Priority       | Priority Badge | Yes      | Low / Normal / High / Urgent                                      |
| Next Follow-up | Date           | Auto     | Scheduled next follow-up date                                     |
| Lead Source    | Text           | Yes      | Origin of lead inquiry                                            |
| Created Date   | Date           | Auto     | Lead creation timestamp                                           |
| Last F/U Date  | Date           | Auto     | Most recent follow-up activity                                    |
| Actions        | Icon           | —        | View / Edit / Add Follow-up                                       |

---

## Actions

| Action        | Icon | Description                                        | Available When            |
| ------------- | ---- | -------------------------------------------------- | ------------------------- |
| View          | 👁   | Opens lead details in read-only mode (2 tabs)      | All statuses              |
| Edit          | ✏    | Opens lead edit form                               | All except Converted/Lost |
| Add Follow-up | ➕   | Quick action to add follow-up without opening lead | All statuses              |

---

## Filters

| Filter      | Type         | Options                                                                        |
| ----------- | ------------ | ------------------------------------------------------------------------------ |
| Lead Status | Multi-select | New / Qualified / Quotation send / Negotiation / Lost / Converted              |
| Lead Source | Multi-select | Website / Referral / Walk-in / Cold Call / Social Media / Exhibition / Partner |
| Priority    | Multi-select | Low / Normal / High / Urgent                                                   |
| Lead Type   | Multi-select | Service / Product                                                              |
| Date Range  | Date Range   | Created date filter                                                            |
| Branch Name | Dropdown     | All Branches / Central / BLR / HYD / BOM                                       |

---

## Search

Searchable by:

- Lead ID
- Lead Name
- Mobile Number
- Email Address

---

# 15.2 Add Lead Form

**Description:**
Initial lead capture form for registering new customer inquiries. Captures essential contact information, service requirements, and initial qualification data to establish the lead record and trigger follow-up workflow.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           ADD NEW LEAD                                      │
│                                                                             │
│  [← Back to Lead List]                                       [Save Draft]   │
│                                                                             │
│  Lead ID:              [AUTO GENERATED - Read Only]                         │
│  Lead Date:            [📅 Auto: Current Date]                              │
│  Lead Source*:         [▼ Website / Referral / Walk-in / Cold Call           │
│                         / Social Media / Exhibition / Partner ▼]            │
│  Branch Name*:         [▼ Select Branch ▼]                                  │
│  Priority*:            [▼ Low / Normal / High / Urgent ▼]                   │
│                                                                             │
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─   │
│                                                                             │
│  Lead Name*:           [____________] (First + Last Name)                   │
│  Mobile Number*:       [____________] (10 digits, unique check)             │
│  Alternate Number:     [____________] (Optional)                            │
│  Email ID:             [____________] (Valid email format)                   │
│                                                                             │
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─   │
│                                                                             │
│  Lead Type*:           [▼ Product / Service ▼]                              │
│                                                                             │
│  IF Lead Type = Service:                                                    │
│  Service Type*:        [▼ Contract / Product Purchase / Jobbing ▼]          │
│                                                                             │
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─   │
│                                                                             │
│  Budget Range:         [▼ <₹5K / ₹5K-10K / ₹10K-25K / ₹25K-50K /          │
│                         ₹50K+ / Not Discussed ▼]                            │
│                                                                             │
│  Lead Description*:    [________________________________________]           │
│                         (Detailed requirement description)                  │
│                         [Min 20 characters]                                 │
│                                                                             │
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─   │
│                                                                             │
│  Created By:           [Auto: Current User]                                 │
│  Created Date:         [Auto: System Timestamp]                             │
│  Status:               [Auto: NEW]                                          │
│  Next Follow-up Date*: [📅 Date Picker] (Manually set by user)             │
│                                                                             │
│                  [SAVE AS DRAFT]      [SUBMIT LEAD]      [CANCEL]           │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Form Fields

| Field               | Type           | Required    | Options/Validation                                                             | Notes                                                             |
| ------------------- | -------------- | ----------- | ------------------------------------------------------------------------------ | ----------------------------------------------------------------- |
| Lead ID             | Auto Generated | System      | Format: LD-YYYY-XXXXX (e.g., LD-2026-00042)                                    | Read-only, unique sequential                                      |
| Lead Date           | Date           | System      | Current date, editable if needed                                               | Lead creation date                                                |
| Lead Source         | Dropdown       | Yes         | Website / Referral / Walk-in / Cold Call / Social Media / Exhibition / Partner | Track origin                                                      |
| Branch Name         | Dropdown       | Yes         | Select from active branches                                                    | Lead assignment branch                                            |
| Priority            | Dropdown       | Yes         | Low / Normal / High / Urgent                                                   | Determines follow-up SLA                                          |
| Lead Name           | Text           | Yes         | Min 3 characters, alphabets and spaces                                         | Primary contact person                                            |
| Mobile Number       | Phone          | Yes         | Exactly 10 digits, unique across leads                                         | Primary contact number                                            |
| Alternate Number    | Phone          | No          | Exactly 10 digits, different from primary                                      | Secondary contact                                                 |
| Email ID            | Email          | No          | Valid email format, unique check                                               | For email communications                                          |
| Lead Type           | Dropdown       | Yes         | Product / Service                                                              | Determines if Service Type dropdown appears                       |
| Service Type        | Dropdown       | Conditional | Contract / Product Purchase / Jobbing                                          | Visible & required only when Lead Type = "Service"                |
| Budget Range        | Dropdown       | No          | <₹5K / ₹5K-10K / ₹10K-25K / ₹25K-50K / ₹50K+ / Not Discussed                   | Qualification data                                                |
| Lead Description    | Text Area      | Yes         | Min 20 characters                                                              | Detailed requirements                                             |
| Created By          | Auto           | System      | Current logged-in user                                                         | System field                                                      |
| Created Date        | Auto           | System      | System timestamp                                                               | System field                                                      |
| Status              | Auto           | System      | Default: NEW                                                                   | New / Qualified / Quotation send / Negotiation /Lost/ Converted |
Lost Reason	|          Text Area	|Conditional	  |    Required if Status = Lost  ||	Reason for lead loss
| Next Follow-up Date | Date & Time    | Yes         | Must be today or future date                                                   | Manually entered by user (not auto-calculated)                    |

---

## Conditional Logic

| Condition             | Behavior                                             |
| --------------------- | ---------------------------------------------------- |
| Lead Type = "Service" | "Service Type" dropdown becomes visible and required |
| Lead Type = "Product" | "Service Type" dropdown is hidden                    |

---

## Validation Rules

| Validation          | Rule                                                            |
| ------------------- | --------------------------------------------------------------- |
| Mobile Uniqueness   | Mobile number must not exist in active leads (check duplicates) |
| Email Uniqueness    | Email must not exist in active leads (if provided)              |
| Service Type        | Required only when Lead Type = "Service"                        |
| Next Follow-up Date | Must be today or a future date, not past                        |

---

## Actions

| Action        | Behavior                                                                 |
| ------------- | ------------------------------------------------------------------------ |
| Save as Draft | Saves without validation, status = DRAFT, no notifications sent          |
| Submit Lead   | Validates all required fields, status = NEW, notifies assigned sales rep |
| Cancel        | Discards all changes, returns to lead list                               |

---

# 15.3 Edit Lead Form

**Description:**
Modifies existing lead information with status-aware field editability. Maintains audit trail of all changes and restricts modifications based on lead lifecycle stage to preserve data integrity.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           EDIT LEAD                                         │
│                                                                             │
│  [← Back to Lead List]     Lead ID: LD-2026-00142     Status: QUALIFIED     │
│                                                                             │
│  Lead ID:              [LD-2026-00142 - Read Only]                          │
│  Lead Date:            [15-Mar-2026 - Read Only]                            │
│  Lead Source:          [Website - Read Only]                                 │
│  Branch Name:          [▼ Select Branch ▼] ✅                               │
│  Priority:             [▼ High / Normal / Low / Urgent ▼] ✅                │
│  Status:               [▼ Qualified ▼] (Controlled workflow)                │
│                                                                             │
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─   │
│                                                                             │
│  Lead Name:            [Rahul Sharma] ✅                                    │
│  Mobile Number:        [9876543210 - Read Only]                             │
│                         (Locked - primary identifier)                       │
│  Alternate Number:     [9123456789] ✅                                      │
│  Email ID:             [rahul.s@email.com] ✅                               │
│                                                                             │
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─   │
│                                                                             │
│  Lead Type:            [▼ Service ▼] ✅                                     │
│                                                                             │
│  IF Lead Type = Service:                                                    │
│  Service Type:         [▼ Contract ▼] ✅                                    │
│                                                                             │
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─   │
│                                                                             │
│  Budget Range:         [▼ ₹10K-25K ▼] ✅                                   │
│                                                                             │
│  Lead Description:     [Termite treatment required for entire...] ✅        │
│                         (Detailed requirement description)                  │
│                         [Min 20 characters]                                 │
│                                                                             │
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─   │
│                                                                             │
│  Created By:           [Admin User]                                         │
│  Created Date:         [15-Mar-2026 10:30 AM]                               │
│  Last Updated By:      [Rajesh Kumar]                                       │
│  Last Updated Date:    [18-Mar-2026 03:45 PM]                               │
│  Status:               [QUALIFIED]                                          │
│  Next Follow-up Date:  [📅 20-Mar-2026] ✅ (Manually editable)             │
│                                                                             │
│  CHANGE HISTORY:                                                            │
│  ┌─────────────┬─────────────┬─────────────────┬─────────────────────┐      │
│  │ Date        │ User        │ Field Changed   │ Change Summary      │      │
│  ├─────────────┼─────────────┼─────────────────┼─────────────────────┤      │
│  │ 18-Mar-2026 │ Rajesh K.   │ Status          │ New → Qualified     │      │
│  │ 18-Mar-2026 │ Rajesh K.   │ Priority        │ Normal → High       │      │
│  │ 17-Mar-2026 │ Rajesh K.   │ Budget Range    │ Not Discussed → ₹   │      │
│  └─────────────┴─────────────┴─────────────────┴─────────────────────┘      │
│                                                                             │
│            [UPDATE LEAD]        [CANCEL]        [VIEW FOLLOW-UPS]           │
│                                                                             │
│  ⚠️ NOTE: Mobile Number and Lead Source cannot be changed after creation.   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Field Editability Matrix

| Field Category      | Field Name          | New | Qualified | Quotation send | Negotiation | Lost/Converted | Notes                            |
| ------------------- | ------------------- | --- | --------- | -------------- | ----------- | -------------- | -------------------------------- |
| **Basic Info**      | Lead ID             | ❌  | ❌        | ❌             | ❌          | ❌             | System generated, never editable |
|                     | Lead Date           | ❌  | ❌        | ❌             | ❌          | ❌             | Creation timestamp               |
|                     | Lead Source         | ❌  | ❌        | ❌             | ❌          | ❌             | Origin tracking, locked          |
|                     | Branch Name         | ✅  | ✅        | ✅             | ⚠️          | ❌             | Editable until Proposal stage    |
|                     | Priority            | ✅  | ✅        | ✅             | ⚠️          | ❌             | Editable until Proposal stage    |
|                     | Status              | ⚠️  | ⚠️        | ⚠️             | ⚠️          | ❌             | Workflow controlled              |
| **Contact Info**    | Lead Name           | ✅  | ✅        | ⚠️             | ❌          | ❌             | Editable until Qualified         |
|                     | Mobile Number       | ❌  | ❌        | ❌             | ❌          | ❌             | Primary key, never editable      |
|                     | Alternate Number    | ✅  | ✅        | ✅             | ✅          | ❌             | Always editable                  |
|                     | Email ID            | ✅  | ✅        | ✅             | ⚠️          | ❌             | Editable until Proposal          |
| **Lead Type**       | Lead Type           | ✅  | ✅        | ⚠️             | ❌          | ❌             | Editable until Qualified         |
|                     | Service Type        | ✅  | ✅        | ⚠️             | ❌          | ❌             | Conditional on Lead Type         |
| **Additional Info** | Budget Range        | ✅  | ✅        | ✅             | ⚠️          | ❌             | Editable until Proposal          |
|                     | Lead Description    | ✅  | ✅        | ✅             | ⚠️          | ❌             | Editable until Proposal          |
| **System Fields**   | Next Follow-up Date | ✅  | ✅        | ✅             | ✅          | ❌             | Manually editable                |

**Legend:** ✅ Fully Editable | ⚠️ Restricted/Approval Required | ❌ Not Editable

---

## Validation Rules (Edit Mode)

| Validation                  | Rule                                                              |
| --------------------------- | ----------------------------------------------------------------- |
| Status Change Authorization | Only assigned user or manager can change status                   |
| Mandatory Lost Reason       | Required when changing status to LOST                             |
| Reactivation Approval       | Manager approval required to move from LOST back to active status |
| Audit Trail                 | All field changes logged with user, timestamp, old/new values     |
| Duplicate Prevention        | Mobile/email uniqueness check maintained on edit (exclude self)   |
| Service Type                | Required only when Lead Type = "Service"                          |

---

## Actions

| Action      | Behavior                                           |
| ----------- | -------------------------------------------------- |
| Update Lead | Saves changes, logs audit trail, updates timestamp |
| Cancel      | Discards changes, returns to lead list             |

---

# 15.4 View Lead Information (2 Tabs)

**Description:**
Comprehensive lead detail view with dual-tab interface. Tab 1 displays complete lead information in read-only format. Tab 2 shows chronological follow-up history with quick actions for creating new follow-ups, GMA sheets, or quotations.

---

## Screen Layout (Tab 1: Basic Lead Information)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           VIEW LEAD DETAILS                                    │
│                                                                              │
│  [← Back to Lead List]     Lead ID: LD-2026-00142     [✏ Edit] [➕ Follow-up] │
│                                                                              │
│  [TAB 1: Basic Information]  [TAB 2: Follow-up Log]                          │
│                                                                              │
│  ─────────────────────────────────────────────────────────────────────────   │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Lead Status:          🟡 Qualified                                     │ │
│  │  Priority:             🔴 HIGH                                          │ │
│  │  Lead Source:          Website                                          │ │
│  │  Branch Name:          Main Branch                                      │ │
│  │  Created:              15-Mar-2026 by Admin                              │ │
│  │  Last Updated:         18-Mar-2026 by Rajesh Kumar                       │ │
│  │  Next Follow-up:       20-Mar-2026 (Due in 2 days)                        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Lead Name:            Rahul Sharma                                     │ │
│  │  Mobile Number:        +91 98765 43210                                  │ │
│  │  Alternate Number:     +91 91234 56789                                  │ │
│  │  Email ID:             rahul.sharma@email.com                           │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Lead Type:            Service                                          │ │
│  │  Service Type:         Contract                                         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Budget Range:         ₹10,000 - ₹25,000                                  │ │
│  │                                                                              │
│  │  Lead Description:                                                        │ │
│  │  Customer reported termite infestation in wooden furniture and flooring.   │ │
│  │  Requires comprehensive termite treatment with annual maintenance.        │ │
│  │  Currently using competitor service but dissatisfied with results.        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │                                                                              │ │
│  │  [📋 CREATE GMA SHEET]    [📄 CREATE QUOTATION]                             │ │
│  │                                                                              │ │
│  │  GMA Status:           Not Created                                        │ │
│  │  Quotation Status:     Not Created                                        │ │
│  │  Last Follow-up:       18-Mar-2026: Site visit scheduled for 22-Mar      │ │
│  │                                                                              │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│                              [CLOSE]    [EDIT]    [ADD FOLLOW-UP]            │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Basic Lead Information Fields

| Section                 | Field Name       | Type           | Description                                   |
| ----------------------- | ---------------- | -------------- | --------------------------------------------- |
| **Lead Summary**        | Lead Status      | Status Badge   | Current lifecycle stage with visual indicator |
|                         | Priority         | Priority Badge | Urgency level with color coding               |
|                         | Lead Source      | Text           | Origin of inquiry                             |
|                         | Branch Name      | Text           | Lead assignment branch                        |
|                         | Created          | DateTime       | Creation timestamp with creator name          |
|                         | Last Updated     | DateTime       | Last modification timestamp                   |
|                         | Next Follow-up   | Date           | Scheduled next activity with countdown        |
| **Contact Info**        | Lead Name        | Text           | Primary contact person                        |
|                         | Mobile Number    | Phone          | Primary contact (click-to-call enabled)       |
|                         | Alternate Number | Phone          | Secondary contact                             |
|                         | Email ID         | Email          | Electronic contact (click-to-mail enabled)    |
| **Lead Type & Service** | Lead Type        | Text           | Product or Service classification             |
|                         | Service Type     | Text           | Contract, Product Purchase, or Jobbing        |
| **Additional Info**     | Budget Range     | Currency Range | Customer budget indication                    |
|                         | Lead Description | Text Area      | Detailed requirements                         |

---

## 15.4.1Follow-up Log Table

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           VIEW LEAD DETAILS - TAB 2                            │
│                                                                              │
│  [← Back to Lead List]     Lead ID: LD-2026-00142     [✏ Edit] [➕ Follow-up] │
│                                                                              │
│  [TAB 1: Basic Information]  [TAB 2: Follow-up Log] ◄── ACTIVE              │
│                                                                              │
│  ─────────────────────────────────────────────────────────────────────────   │
│                                                                              │
│  QUICK FILTERS: [All] [Calls] [Meetings] [Site Visits] [Emails] [WhatsApp]   │
│                                                                              │
│  FOLLOW-UP LOG TABLE                                                         │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │F/U #│Date      │Type│Contact│Outcome│Next Action│Status│Assigned│Action│ │
│  │     │          │    │Mode   │       │           │      │To      │      │ │
│  │─────┼──────────┼────┼───────┼───────┼───────────┼──────┼────────┼──────│ │
│  │#5   │18-Mar    │Call│Mobile │Pos.   │Site visit │Comp. │Rajesh K│👁    │ │
│  │     │          │    │       │       │22-Mar     │      │        │      │ │
│  │#4   │17-Mar    │WA  │WhatsApp│Info  │Share      │Comp. │Rajesh K│👁    │ │
│  │     │          │    │       │Req.   │brochure   │      │        │      │ │
│  │#3   │16-Mar    │Call│Mobile │Not    │Callback   │Comp. │Rajesh K│👁    │ │
│  │     │          │    │       │Avail. │17-Mar     │      │        │      │ │
│  │#2   │15-Mar    │Email│Email │Opened │Call to    │Comp. │Rajesh K│👁    │ │
│  │     │          │    │       │       │discuss    │      │        │      │ │
│  │#1   │15-Mar    │Auto│System │Lead   │Qualify    │Comp. │System  │👁    │ │
│  │     │          │    │       │Created│req.       │      │        │      │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  PENDING FOLLOW-UPS                                                          │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │F/U #│Scheduled│Type│Purpose│Assigned To│Status│Overdue│Action          │ │
│  │─────┼─────────┼────┼───────┼───────────┼──────┼───────┼────────────────│ │
│  │#6   │20-Mar   │Site│Initial│Rajesh K.  │Pend. │—      │[Complete] [Resch]│
│  │     │         │Visit│survey │           │      │       │                │ │
│  │#7   │18-Mar   │Call│Follow-│Rajesh K.  │Pend. │⚠️ 2d  │[Complete] [Resch]│
│  │     │         │    │up     │           │      │       │                │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  GENERAL BUTTONS:                                                            │
│                                                                              │
│  [➕ NEW FOLLOW-UP]    [📋 CREATE GMA SHEET]    [📄 CREATE QUOTATION]         │
│                                                                              │
│  CONVERSION STATUS:                                                          │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ GMA Sheet:    Not Created          [Create GMA →]                        │ │
│  │ Quotation:    Not Created          [Create Quote →]                       │ │
│  │ Contract:     Not Applicable       (Requires Converted status)            │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Follow-up Log Table Fields

| Field        | Type         | Description                                                       |
| ------------ | ------------ | ----------------------------------------------------------------- |
| F/U #        | Number       | Sequential follow-up identifier                                   |
| Date & Time  | Date & Time  | When follow-up was conducted or scheduled                         |
| Contact Type | Badge        | Call / Meeting / Site Visit / Email / WhatsApp / Other            |
| Lead Type    | Badge        | Product / Service / Both                                          |
| Outcome      | Text         | Result of the follow-up interaction                               |
| Next Action  | Text         | Agreed next step from follow-up                                   |
| Status       | Status Badge | New / Qualified / Quotation send / Negotiation / Lost / Converted |
| Action       | Button       | View follow-up details                                            |

---

## General Buttons (Tab 2)

| Button              | Action                                             | Redirects To                  | Condition                         |
| ------------------- | -------------------------------------------------- | ----------------------------- | --------------------------------- |
| ➕ New Follow-up    | Opens Add Follow-up Form                           | 15.5 Add Follow-up Form       | Always available                  |
| 📋 Create GMA Sheet | Opens GMA creation with lead data pre-filled       | Module 17: Add GMA Form       | Lead status = Qualified or higher |
| 📄 Create Quotation | Opens Quotation creation with lead data pre-filled | Module 16: Add Quotation Form | Lead status = Qualified or higher |

---

## Conversion Status Logic

| Status         | GMA Creation                  | Quotation Creation         | Contract Creation        |
| -------------- | ----------------------------- | -------------------------- | ------------------------ |
| New            | ⚠️ Manager approval           | ❌ Not allowed             | ❌ Not allowed           |
| Qualified      | ✅ Allowed                    | ✅ Allowed                 | ❌ Not allowed           |
| Quotation send | ✅ Allowed                    | ✅ Allowed (edit existing) | ❌ Not allowed           |
| Negotiation    | ✅ Allowed (re-GMA if needed) | ✅ Allowed (revised quote) | ❌ Not allowed           |
| Converted      | ❌ Not applicable             | ❌ Not applicable          | ✅ Auto-trigger Contract |
| Lost           | ❌ Not applicable             | ❌ Not applicable          | ❌ Not applicable        |

---

# 15.5 Add Follow-up Form

**Description:**
Captures a brief summary of the interaction, allows the user to update the Lead's overall status, and log the time, date, and reason for the next scheduled follow-up.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           ADD FOLLOW-UP                                     │
│                                                                             │
│  [← Back]                   Lead: Rahul Sharma | Current Status: New        │
│                             Lead Type : Service  | Branch Name: Main Branch │
│  ─────────────────────────────────────────────────────────────────────────  │
│                                                                             │
│  Interaction Summary*:  [________________________________________]          │
│                         (Details of the conversation or action taken)       │
│                                                                             │
│  Update Status*:   [▼ New / Qualified / Quotation send              │
│                          / Negotiation / Converted / Lost ▼]                │
│                         (Changes the overall status of this lead)           │
│                          (if Lost then Loast reason field will appear)      │
│  ─────────────────────────────────────────────────────────────────────────  │
│  NEXT FOLLOW-UP PLANNING                                                    │
│                                                                             │
│  Schedule Next Action*: [☑ Yes ☐ No (Close Lead)]                         │
│                                                                             │
│  IF YES:                                                                    │
│  Next Follow-up Date*:  [📅 22-Mar-2026]                                    │
│  Next Follow-up Time:   [🕐 10:00]                                         │
│  Reason / Agenda*:      [________________________________________]          │
│                         (Objective for the next interaction)                │
│                                                                             │
│                    [SAVE FOLLOW-UP]        [CANCEL]                         │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Follow-up Form Fields

| Field                | Type      | Required    | Options/Validation                                           | Notes                                  |
| -------------------- | --------- | ----------- | ------------------------------------------------------------ | -------------------------------------- |
| Lead Name            | Text      | Auto        |                                                              |                                        |
| Lead Type            | Dropdown  | Auto        | Service, Product, Contract                                        |                                        |
| Branch Name          | Dropdown  | Auto        |                                                              |                                        |
| Current Status       | Badge     | Auto        | New, Qualified, Quotation send, Negotiation, Converted, Lost | Updates the main lead status           |
| Interaction Summary  | Text Area | Yes         | Min 10 characters                                            | Details of the current interaction     |
| Update Status        | Dropdown  | Yes         | New, Qualified, Quotation send, Negotiation, Converted, Lost | Updates the main lead status           |
| Lost Reason          | Text Area | Conditional | Required if Status = Lost                                    | Reason for lead loss                   |
| Schedule Next Action | Checkbox  | Yes         | Yes/No toggle                                                | Determines if a future task is created |
| Next Follow-up Date  | Date      | Conditional | Must be a future date (Required if Yes)                      | When to follow up                      |
| Next Follow-up Time  | Time      | No          | 24-hour format                                               | Time of next follow up                 |
| Reason / Agenda      | Text Area | Conditional | Required if Yes                                              | Purpose of the next contact            |

---

## Actions & System Behaviors

| Action / Trigger       | Behavior                                                                       |
| ---------------------- | ------------------------------------------------------------------------------ |
| **Save Follow-up**     | Saves record, updates lead status, schedules next action, returns to lead view |
| **Cancel**             | Discards changes, returns to previous screen                                   |
| **Status = Qualified** | Enables GMA and Quotation creation buttons in the View Lead screen             |
| **Status = Converted** | Triggers Contract creation workflow in Module 18                               |

---

# 15.6 View Follow-up Detail

**Description:**
Read-only detailed view of a specific follow-up interaction showing complete conversation history, attachments, status changes, and linked next actions. Provides audit trail for sales activity review and coaching.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           VIEW FOLLOW-UP DETAIL                             │
│                                                                             │
│  [← Back to Lead: LD-2026-00142]     Follow-up #5 of Lead: Rahul Sharma     │
│                                                                             │
│  ─────────────────────────────────────────────────────────────────────────  │
│                                                                             │
│  FOLLOW-UP HEADER (auto-filled)                                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐│
│  │  Follow-up Number:     #5                                               ││
│  │  Date Executed:          18-Mar-2026, 15:30 IST                         ││
│  │  Conducted By:           Rajesh Kumar (Sales Executive)                 ││
│  └─────────────────────────────────────────────────────────────────────────┘│
│                                                                             │
│  INTERACTION DETAILS                                                        │
│  ┌─────────────────────────────────────────────────────────────────────────┐│
│  │                                                                         ││
│  │  INTERACTION SUMMARY:                                                   ││
│  │  ─────────────────────────────────────────────────────────────────────  ││
│  │  Called customer to discuss termite treatment requirements. Customer    ││
│  │  confirmed infestation in 3 rooms and wooden furniture. Currently using ││
│  │  PestGuard but unhappy with results. Interested in our Contract package.     ││
│  │  Asked for site visit to assess extent of problem.                      ││
│  │                                                                         ││
│  │  LEAD STATUS UPDATED:      New → Qualified                              ││
│  └─────────────────────────────────────────────────────────────────────────┘│
│                                                                             │
│  NEXT FOLLOW-UP PLANNED                                                     │
│  ┌─────────────────────────────────────────────────────────────────────────┐│
│  │  Scheduled Action:       Yes                                            ││
│  │  Date:                   22-Mar-2026                                    ││
│  │  Time:                   10:00 AM                                       ││
│  │  Reason / Agenda:        Initial site survey and termite infestation    ││
│  │                          assessment. Prepare GMA.                       ││
│  └─────────────────────────────────────────────────────────────────────────┘│
│                                                                             │
│  AUDIT TRAIL                                                                │
│  ┌─────────────────────────────────────────────────────────────────────────┐│
│  │  Created:              18-Mar-2026 15:45 by Rajesh Kumar                ││
│  │  System Updates:         • Next action scheduled                        ││
│  │                          • Lead status auto-updated to Qualified        ││
│  │                          • GMA creation enabled                         ││
│  └─────────────────────────────────────────────────────────────────────────┘│
│                                                                             │
│                              [CLOSE]    [PRINT]                             │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## View Follow-up Detail Fields

| Section                 | Field               | Type            | Description                                  |
| ----------------------- | ------------------- | --------------- | -------------------------------------------- |
| **Header**              | Follow-up Number    | Text            | Sequential identifier                        |
|                         | Lead Name           | Text            | Name of the associated lead                  |
|                         | Date Executed       | DateTime        | When follow-up was submitted                 |
|                         | Conducted By        | Text            | User who logged the follow-up                |
| **Interaction Details** | Interaction Summary | Text Area       | Full conversation notes from the interaction |
|                         | Lead Status Updated | Text            | Before → After status change                 |
| **Next Action**         | Scheduled Action    | Boolean         | Whether a next action was scheduled (Yes/No) |
|                         | Date                | Date            | Date of the next follow-up                   |
|                         | Time                | Time            | Time of the next follow-up                   |
|                         | Reason / Agenda     | Text Area       | Purpose for the next contact                 |
| **Audit**               | Created             | DateTime + User | Registration timestamp                       |
|                         | System Updates      | Bullet List     | Automatic actions taken as a result          |

---

## Actions

| Action | Behavior                            |
| ------ | ----------------------------------- |
| Close  | Returns to lead detail view (Tab 2) |

---

===============================================================

# 🎯 MODULE 16: Quotation Management

## Overview

Quotation Management handles the creation, tracking, and lifecycle of quotations for **Services** (contract-based / one-time job) and **Product Sales**. Quotations can be generated from existing Leads (Module 15), Customers (Module 9), or created for new prospects directly.

**Module Connections:**

- Depends on: Module 12 (Service Catalog & Pricing), Module 10 (Product Master & Tax), Module 9 (Tax Management)
- Uses: Module 15 (Lead Data)
- Triggers: Module 18 (Contract Creation on Acceptance)

---

# 16.1 Quotation Table View (Dashboard)

**Description:**
The Quotation Dashboard displays all quotations with filtering, searching, and quick actions. Users can view, delete (revoke), and download quotations. The delete action is only available when quotation status is "Not Sent".

---

# Screen Layout

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                          QUOTATION MANAGEMENT                                 │
│                                                                              │
│  ┌──────────────────────────────── FILTERS ──────────────────────────────┐  │
│  │                                                                       │  │
│  │ Status:           [☑ Draft ☑ Sent ☑ Viewed ☑ Accepted ☑ Rejected     │  │
│  │                    ☑ Expired ☑ Revised]                               │  │
│  │ Quotation Type:   [☑ Service ☑ Product ☑ Combined]                   │  │
│  │ Source:           [☑ From Lead ☑ From Customer ☑ New Prospect]        │  │
│  │ Branch:           [🔍 Search Dropdown ▼]                              │  │
│  │ Created By:       [🔍 Search Dropdown ▼]                              │  │
│  │ Created Date:     [📅 From] - [📅 To]                                 │  │
│  │ Amount Range:     [₹ Min] - [₹ Max]                                   │  │
│  │                                                                       │  │
│  │ Search: [__________________________________________________]         │  │
│  │        (Quotation ID / Client Name / Phone / Lead ID)                │  │
│  │                                                                       │  │
│  │ [Reset Filters]                              [+ ADD QUOTATION]        │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│                          QUOTATION TABLE                                     │
│                                                                              │
│ ┌──────────┬──────────┬────────────┬──────────┬────────┬──────────┬───────┐ │
│ │Quot. ID  │Source    │Client Name │Type      │Branch  │Total (₹) │Status │ │
│ ├──────────┼──────────┼────────────┼──────────┼────────┼──────────┼───────┤ │
│ │QT-2026-001│From Lead│Rahul Sharma│Service  │BLR     │₹18,500   │🟢 Sent│ │
│ │QT-2026-002│Customer │ABC Corp    │Product  │HYD     │₹45,200   │🟡 Draft│ │
│ │QT-2026-003│New      │Priya Patel │Combined │MUM     │₹32,800   │🔵 Viewed│ │
│ │QT-2026-004│From Lead│Suresh K.   │Service  │BLR     │₹12,000   │🟢 Accepted│ │
│ │QT-2026-005│Customer │XYZ Ltd     │Product  │DEL     │₹78,500   │🔴 Rejected│ │
│ └──────────┴──────────┴────────────┴──────────┴────────┴──────────┴───────┘ │
│                                                                              │
│ ┌──────────────────────────────────────────────────────────────────────────┐ │
│ │Valid Till  │Created Date & Time    │Created By    │Actions               │ │
│ │────────────┼───────────────────────┼──────────────┼──────────────────────│ │
│ │25-Mar-2026 │15 Mar 2026 10:30 AM   │Rajesh Kumar  │[👁 View] [📥 Download]│ │
│ │30-Mar-2026 │16 Mar 2026 02:15 PM   │Anita Sharma  │[👁 View] [🗑 Delete] [📥]│ │
│ │28-Mar-2026 │16 Mar 2026 04:00 PM   │Rohit Mehta   │[👁 View] [📥 Download]│ │
│ │20-Mar-2026 │14 Mar 2026 11:00 AM   │Rajesh Kumar  │[👁 View] [📥 Download]│ │
│ │22-Mar-2026 │15 Mar 2026 09:45 AM   │Anita Sharma  │[👁 View] [📥 Download]│ │
│ └──────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│ Pagination:  Previous   1   2   3   ...   10   Next                          │
│                                                                              │
│ Status Legend: 🟡 Draft  🟢 Sent  🔵 Viewed  🟢 Accepted                    │
│               🔴 Rejected  ⚪ Expired  🟠 Revised                            │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

# Table View Fields

| Field               | Type     | Required | Description                                    |
| ------------------- | -------- | -------- | ---------------------------------------------- |
| Quotation ID        | Text     | Auto     | Unique identifier (QT-YYYY-SEQ format)         |
| Source              | Badge    | Yes      | From Lead / From Customer / New Prospect       |
| Client Name         | Text     | Yes      | Lead name, customer name, or new prospect name |
| Quotation Type      | Badge    | Yes      | Service / Product / Combined                   |
| Branch              | Text     | Yes      | Assigned branch for quotation                  |
| Total Amount (₹)    | Currency | Auto     | Grand total including tax                      |
| Status              | Badge    | Auto     | Draft / Sent / Accepted / Rejected / Expired   |
| Valid Till          | Date     | Yes      | Quotation expiry date                          |
| Created Date & Time | DateTime | Auto     | Timestamp of quotation creation                |
| Created By          | Text     | Auto     | User who created the quotation                 |
| Actions             | Buttons  | —        | View / Delete (Revoke) / Download              |

---

# Actions

| Action   | Icon | Available When      | Description                               |
| -------- | ---- | ------------------- | ----------------------------------------- |
| View     | 👁   | All statuses        | Opens quotation details in read-only mode |
| Delete   | 🗑   | Status = Draft only | Opens delete confirmation popup (16.4)    |
| Download | 📥   | All statuses        | Downloads quotation as PDF                |

**Note:** Delete button is **hidden/disabled** when quotation status is anything other than "Draft" (i.e., once sent, it cannot be deleted).

---

# Filters

| Filter         | Type                | Default      | Options                                                         |
| -------------- | ------------------- | ------------ | --------------------------------------------------------------- |
| Status         | Multi-select        | All          | Draft / Sent / Viewed / Accepted / Rejected / Expired / Revised |
| Quotation Type | Multi-select        | All          | Service / Product / Combined                                    |
| Branch         | Searchable Dropdown | All          | All branches from Module 7                                      |
| Created By     | Searchable Dropdown | All          | Sales team members                                              |
| Created Date   | Date Range          | Last 30 Days | Custom range                                                    |
| Amount Range   | Number Range        | All          | Min – Max amount filter                                         |

---

# Search

Global search supports:

| Search Field | Type |
| ------------ | ---- |
| Quotation ID | Text |
| Client Name  | Text |

---

# 16.2 Add Quotation Form

**Description:**
The Add Quotation form allows sales team members to create new quotations. The form dynamically adjusts based on the selected source (Lead / Customer / New Prospect) and quotation type (Service / Product / Combined). Pricing is dynamically fetched from Module 12 (Services) and Module 10 (Products) based on selected items and dynamic pricing selection. Multiple service locations can be added with per-location pricing.

---

# Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Quotations]              ADD QUOTATION                          │
│                                                                              │
│  Quotation ID: QT-2026-XXXX (Auto-generated on save)                        │
│                                                                              │
│  SECTION 1: SOURCE SELECTION                                                 │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Create Quotation For*:                                                 │ │
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
│  │  Lead Type:           Service/Product/Both (Read-only)                    │ │
│  │                       *based on lead type below options enable           │ │
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
│  │  Company Name:        [________________________________]                │ │
│  │  Address*:            [________________________________]                │ │
│  │  City*:               [________________________________]                │ │
│  │  State*:              [▼ Select State ▼]                                │ │
│  │  Pincode:             [________]                                        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 2: QUOTATION TYPE                                                   │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Quotation Type*:                                                       │ │
│  │  (•) Service Quotation    ( ) Product Quotation    ( ) Combined         │ │
│  │                                                                         │ │
│  │  Service Mode (if Service/Combined):                                    │ │
│  │  (•) One-Time Service    ( ) Contract (Annual Maintenance Contract)          │ │
│  │                                                                         │ │
│  │  IF Contract SELECTED:                                                       │ │
│  │  Frequency*:          [▼ Monthly / Quarterly / Half-Yearly / Yearly ▼]  │ │
│  │  Contract Duration*:  [▼ 6 Months / 1 Year / 2 Years / 3 Years ▼]      │ │
│  │  Proposed Start Date*: [📅 Date Picker]                                 │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 3: LOCATION & BRANCH ASSIGNMENT                                     │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  [+ ADD LOCATION]                                                       │ │
│  │                                                                         │ │
│  │  LOCATION 1 (Auto-filled if source has address)                         │ │
│  │  ┌──────────────────────────────────────────────────────────────────┐  │ │
│  │  │ Address*:        [HSR Layout, Sector 2, Bengaluru]               │  │ │
│  │  │ City*:           [Bengaluru]                                     │  │ │
│  │  │ State*:          [Karnataka]                                     │  │ │
│  │  │ Category*:       [▼ Residential / Commercial / Industrial ▼]     │  │ │
│  │  │ Sub-Category*:   [▼ Internal / External ▼]                       │  │ │
│  │  │ Area (sqft)*:    [1200]                                          │  │ │
│  │  │ Assign Branch*:  [▼ BLR-HSR Branch ▼]                           │  │ │
│  │  │                                                                  │  │ │
│  │  │ *Note: Specific Property Types (like BHK/Office Size) are selected│  │ │
│  │  │  under Service Selection to allow per-service pricing logic.      │  │ │
│  │  └──────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                         │ │
│  │  LOCATION 2                                                             │ │
│  │  ┌──────────────────────────────────────────────────────────────────┐  │ │
│  │  │ Address*:        [________________________________]              │  │ │
│  │  │ City*:           [________________________________]              │  │ │
│  │  │ State*:          [▼ Select State ▼]                              │  │ │
│  │  │ Category*:       [▼ Select Category ▼]                           │  │ │
│  │  │ Sub-Category*:   [▼ Select Sub-Category ▼]                       │  │ │
│  │  │ Area (sqft)*:    [________]                                      │  │ │
│  │  │ Assign Branch*:  [▼ Select Branch ▼]                             │  │ │
│  │  │ [Remove Location]                                                │  │ │
│  │  └──────────────────────────────────────────────────────────────────┘  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│  SECTION 4: SERVICE SELECTION                                               │
│  (DYNAMIC VISIBILITY: Visible if Quotation Type = "Service" or "Combined")   │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  ── LOCATION 1: HSR Layout | Resi | Internal | 1200 SQFT ──              │ │
│  │                                                                         │ │
│  │  [🔍 Search and Add Service for this Location...              ] [+ Add] │ │
│  │                                                                         │ │
│  │  ┌───────────────┬────────────┬────────────┬────────────┬─────────────┐ │ │
│  │  │ SERVICE NAME  │ PRICE TYPE │ RATE (₹)   │ TOTAL (₹)  │ ACTIONS     │ │ │
│  │  │ (SVC Code)    │ (Logic)    │ (Per Visit)│ (Line Ttl) │             │ │ │
│  │  ├───────────────┼────────────┼────────────┼────────────┼─────────────┤ │ │
│  │  │ Termite Ctrl  │ Fixed      │ ₹ 1,800    │ ₹ 21,600   │ [Edit][Rem] │ │ │
│  │  │ (SVC-001)     │ (3 BHK)    │            │ (12 Vis)   │             │ │ │
│  │  ├───────────────┼────────────┼────────────┼────────────┼─────────────┤ │ │
│  │  │ Cockroach Gel │ Area Based │ ₹ 2,900    │ ₹ 2,900    │ [Edit][Rem] │ │ │
│  │  │ (SVC-002)     │ (1200 SQFT)│            │ (1 Vis)    │             │ │ │
│  │  └───────────────┴────────────┴────────────┴────────────┴─────────────┘ │ │
│  │                                                                         │ │
│  │  ┌───── DYNAMIC SERVICE CONFIGURATION (Auto-opens on Add/Edit) ─────┐   │ │
│  │  │                                                                  │   │ │
│  │  │ SERVICE: [SVC-001] Termite Control (FETCHED FROM MODULE 12)       │   │ │
│  │  │ ──────────────────────────────────────────────────────────────── │   │ │
│  │  │                                                                  │   │ │
│  │  │ [IF FIXED PRICE DETECTED]:                                       │   │ │
│  │  │ Select Property Size:                                            │   │ │
│  │  │ ( ) 1 BHK    ( ) 2 BHK    (●) 3 BHK    ( ) 4 BHK+   ( ) Villa    │   │ │
│  │  │                                                                  │   │ │
│  │  │ [IF AREA BASED DETECTED]:                                        │   │ │
│  │  │ Base Price: [₹ 500  ]   Rate/SQFT: [₹ 2.00 ]   Area: [1200 ] SQFT│   │ │
│  │  │                                                                  │   │ │
│  │  │ [IF Contract MODE DETECTED (from Section 2)]:                         │   │ │
│  │  │ Vis. Frequency: [▼ Monthly (12) ▼]  Total Visits: [ 12 ]         │   │ │
│  │  │                                                                  │   │ │
│  │  │ ──────────────────────────────────────────────────────────────── │   │ │
│  │  │ RESULTING LINE TOTAL: ₹ 21,600                                   │   │ │
│  │  │                   [ UPDATE SERVICE ]    [ CANCEL ]               │   │ │
│  │  └──────────────────────────────────────────────────────────────────┘   │ │
│  │                                                                         │ │
│  │  Location 1 Service Subtotal: ₹ 24,500                                  │ │
│  │                                                                         │ │
│  │  ── LOCATION 2: Whitefield | Comm | External | 5000 SQFT ──              │ │
│  │  [🔍 Search and Add Service for Location 2...                 ] [+ Add] │ │
│  │                                                                         │ │
│  │  ┌───────────────┬────────────┬────────────┬────────────┬─────────────┐ │ │
│  │  │ SERVICE NAME  │ PRICE TYPE │ RATE (₹)   │ TOTAL (₹)  │ ACTIONS     │ │ │
│  │  ├───────────────┼────────────┼────────────┼────────────┼─────────────┤ │ │
│  │  │ Rodent Control│ Area Based │ ₹ 11,200   │ ₹ 11,200   │ [Edit][Rem] │ │ │
│  │  │ (SVC-003)     │ (5000 SQFT)│            │ (1 Vis)    │             │ │ │
│  │  └───────────────┴────────────┴────────────┴────────────┴─────────────┘ │ │
│  │                                                                         │ │
│  │  Location 2 Service Subtotal: ₹ 11,200                                  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 5: PRODUCT SELECTION                                                │
│  (DYNAMIC VISIBILITY: Visible if Quotation Type = "Product" or "Combined")   │
│  (Integrated with Module 10 – Product Master)                                │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  [🔍 Search Product from Product Master...]                             │ │
│  │                                                                         │ │
│  │  ┌────────────┬──────────┬─────┬──────────┬─────────┬──────────┬─────┐│ │
│  │  │Product Name│Prod. Code│UOM  │HSN Code  │Unit ₹   │Qty       │Total││ │
│  │  │────────────┼──────────┼─────┼──────────┼─────────┼──────────┼─────││ │
│  │  │Brass Sprayer│BSP3-001 │Nos  │8424      │₹ 1,200  │[  5  ]   │₹ 6k │ │
│  │  │Chemical X  │CH-001    │Ltr  │1234      │₹ 500    │[ 10  ]   │₹ 5k │ │
│  │  │└───────────┴──────────┴─────┴──────────┴─────────┴──────────┴─────┘│ │
│  │                                                                         │ │
│  │  ┌──────────┬──────────┬──────────┬──────────────────────────────────┐ │ │
│  │  │CGST (₹)  │SGST (₹)  │IGST (₹)  │Line Total (₹)                   │ │ │
│  │  │──────────┼──────────┼──────────┼──────────────────────────────────│ │ │
│  │  │₹ 540      │₹ 540      │—         │₹ 7,080                            │ │ │
│  │  │₹ 450      │₹ 450      │—         │₹ 5,900                            │ │ │
│  │  │└─────────┴──────────┴──────────┴──────────────────────────────────┘ │ │
│  │                                                                         │ │
│  │  Product Subtotal: ₹ 12,980                                              │ │
│  │  [+ Add Product]                                                        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 6: PRICING SUMMARY                                                  │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  ┌────────────────────────────────────────────────────────────────────┐│ │
│  │  │ LOCATION-WISE BREAKDOWN                                           ││ │
│  │  │                                                                    ││ │
│  │  │ Location           │ Services (₹) │ Products (₹) │ Subtotal (₹)  ││ │
│  │  │ ───────────────────┼──────────────┼──────────────┼───────────────││ │
│  │  │ HSR Layout, BLR    │ ₹ 24,500     │ —            │ ₹ 24,500      ││ │
│  │  │ Whitefield, BLR    │ ₹ 11,200     │ —            │ ₹ 11,200      ││ │
│  │  │ Products (General) │ —            │ ₹ 12,980     │ ₹ 12,980      ││ │
│  │  │ └──────────────────┴──────────────┴──────────────┴───────────────┘│ │
│  │                                                                         │ │
│  │  Services Subtotal         : ₹ 35,700                                    │ │
│  │  Products Subtotal         : ₹ 12,980                                    │ │
│  │  ──────────────────────────────────────────────────────────────────     │ │
│  │  Subtotal (Before Tax)     : ₹ 48,680                                    │ │
│  │  Tax (CGST + SGST / IGST)  : ₹ 8,762                                    │ │
│  │  ──────────────────────────────────────────────────────────────────     │ │
│  │  Total Before Discount     : ₹ 57,442                                    │ │
│  │  Discount Type:            [▼ Percentage / Flat Amount ▼]               │ │
│  │  Discount Value:           [ 10 ]%                                      │ │
│  │  Discount Amount           : - ₹ 5,744                                   │ │
│  │  ──────────────────────────────────────────────────────────────────     │ │
│  │  ★ GRAND TOTAL             : ₹ 51,698                                   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 7: TERMS & VALIDITY                                                 │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Quotation Valid Till*:    [📅 Date Picker]                             │ │
│  │                            (Default: 15 days from creation)             │ │
│  │  Payment Terms*:          [▼ 100% Advance / 50% Advance + 50% on       │ │
│  │                            Completion / Net 15 / Net 30 / Custom ▼]     │ │
│  │  Custom Payment Terms:    [________________________________________]    │ │
│  │                            (Visible only if "Custom" selected)          │ │
│  │  Special Terms/Conditions: [________________________________________]   │ │
│  │                            [Text Area]                                  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 8: ATTACHMENTS & NOTES                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Upload Attachments:      [📎 Browse] (Max 5 files, 5MB each)          │ │
│  │                            Supported: PDF / JPG / PNG / DOC             │ │
│  │  Internal Notes:          [________________________________________]    │ │
│  │                            (Not visible on customer-facing quotation)   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│                    [SAVE DRAFT]      [SEND QUOTATION]       [CANCEL]         │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Section 1: Source Selection Fields

| Field           | Type            | Required    | Options/Validation                                                                                        | Notes                              |
| --------------- | --------------- | ----------- | --------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| Source Type     | Radio           | Yes         | From Lead / From Customer / Add New                                                                       | Determines which sub-fields appear |
| Select Lead     | Search Dropdown | Conditional | Active leads from Module 15 (Status ≥ QUALIFIED)                                                          | Required if Source = From Lead     |
| Select Customer | Search Dropdown | Conditional | Active customers from Module 9                                                                            | Required if Source = From Customer |
| Full Name       | Text            | Conditional | Min 3 characters                                                                                          | Required if Source = Add New       |
| Phone           | Number          | Conditional | 10-digit Indian mobile                                                                                    | Required if Source = Add New       |
| Email           | Email           | yes          | Valid email format                                                                                        | Optional for Add New               |
| Company Name    | Text            | No          | Max 100 characters                                                                                        | Optional for Add New               |
| Address         | Text            | Conditional | Min 10 characters                                                                                         | Required if Source = Add New       |
| City            | Text            | Conditional | Min 3 characters                                                                                          | Required if Source = Add New       |
| State           | Dropdown        | Conditional | Indian states list                                                                                        | Required if Source = Add New       |
| Pincode         | Number          | No          | 6-digit                                                                                                   | Optional                           |

---

## Section 2: Quotation Type Fields

| Field             | Type     | Required    | Options/Validation                         | Notes                                 |
| ----------------- | -------- | ----------- | ------------------------------------------ | ------------------------------------- |
| Quotation Type    | Radio    | Yes         | Service / Product / Combined               | Controls which sections are visible   |
| Service Mode      | Radio    | Conditional | One-Time / Contract                             | Visible if Type = Service or Combined |
| Contract Frequency     | Dropdown | Conditional | Monthly / Quarterly / Half-Yearly / Yearly | Required if Mode = Contract                |
| Contract Duration | Dropdown | Conditional | 6 Months / 1 Year / 2 Years / 3 Years      | Required if Mode = Contract                |
| Proposed Start    | Date     | Conditional | ≥ Today                                    | Required if Mode = Contract                |

---

## Section 3: Location & Branch Assignment Fields

| Field         | Type                | Required | Options/Validation                                         | Notes                         |
| ------------- | ------------------- | -------- | ---------------------------------------------------------- | ----------------------------- |
| Address       | Text                | Yes      | Min 10 characters                                                                                         | Service delivery address      |
| City          | Text                | Yes      | Min 3 characters                                                                                                         | City name                     |
| State         | Dropdown            | Yes      | Indian states list                                                                                                       | State selection               |
| Category      | Dropdown            | Yes      | Residential / Commercial / Industrial / Warehouse                                                                        | Linked to Module 12 Categories |
| Sub-Category  | Dropdown            | Yes      | Internal / External                                                                                                      | Linked to Module 12           |
| Area (sqft)   | Number              | Yes      | Must be > 0                                                                                                              | Used for area-based pricing   |
| Assign Branch | Searchable Dropdown | Yes      | Active branches from Module 7                                                                                            | Nearest branch auto-suggested |

**Business Rules:**

- First location is auto-populated from lead/customer address if available
- Additional locations can be added via "Add Location" button
- Each location gets independent service selection and pricing
- Branch assignment determines which branch handles that location

---

## Section 4: Service Selection Fields

_(Integrated with Module 12 – Service Management)_

| Field               | Type                | Required | Options/Validation                                                                                                       | Notes                                   |
| ------------------- | ------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------ | --------------------------------------- |
| Service Name        | Search Dropdown     | Yes      | Active services from Module 12                                                                                           | Triggers Dynamic Pricing Config         |
| Pest Type           | Auto-filled         | System   | From service master                                                                                                      | Read-only                               |
| Pricing Configuration| Modal / Inline Block| Auto     | User enters BHK size or Area SQFT based on fetched logic                                                                 | Per-service selection/calc              |
| Rate (₹)            | Auto-calculated     | System   | Based on Configuration Selection (BHK) or Area (SQFT)                                                                    | Dynamic fetch from Module 12            |
| Frequency / Visits  | Dropdown            | Yes      | Single / Monthly / Quarterly / Contract                                                                                       | Determines revisit cycle                |
| Total (₹)           | Number              | Auto     | `Rate × Total Visits`                                                                                                    | Auto-calculated                         |
| Actions             | Button(s)           | —        | Edit Pricing / Remove Service                                                                                            | Per service row                         |

**Business Rules:**

- **Service Selection Trigger**: When a service is selected via search, a dynamic configuration block auto-opens to capture pricing inputs (BHK for Fixed, SQFT for Area-Based).
- **Fixed Price**: Matches BHK/Office size pricing from Module 12 based on selection in the configuration block.
- **Area Based**: Pulls SQFT from Section 3 but allows user override; calculates `Base Price + (Rate per SQFT × Total SQFT)`.
- **Contract Mode Multiplier**: If Section 2 is set to Contract, the Line Total is automatically multiplied by the number of visits (e.g., Monthly = Rate x 12).
- **Multi-Service Support**: Multiple services can be added under a single location; each is independently configured and calculated.
- **Independence**: Removing a service or location recalculates only the affected subtotal and the global grand total.

---

## Section 5: Product Selection Fields

_(Integrated with Module 10 – Product Master)_

| Field          | Type            | Required | Validation                       | Notes                       |
| -------------- | --------------- | -------- | -------------------------------- | --------------------------- |
| Product Name   | Search Dropdown | Yes      | Active products from Module 10   | Fetches product details     |
| Product Code   | Auto-filled     | System   | From product master              | Read-only                   |
| UOM            | Auto-filled     | System   | From product master              | Read-only                   |
| HSN Code       | Auto-filled     | System   | From product master              | Read-only                   |
| Unit Price (₹) | Auto-filled     | System   | Selling price from Module 10     | Editable (override allowed) |
| Quantity       | Number          | Yes      | Must be ≥ 1                      | User input                  |
| Total (₹)      | Auto-calculated | System   | Unit Price × Quantity            | Pre-tax total               |
| CGST (₹)       | Auto-calculated | System   | From HSN → Tax Module (Module 9) | Intra-state tax             |
| SGST (₹)       | Auto-calculated | System   | From HSN → Tax Module (Module 9) | Intra-state tax             |
| IGST (₹)       | Auto-calculated | System   | From HSN → Tax Module (Module 9) | Inter-state tax             |
| Line Total (₹) | Auto-calculated | System   | Total + applicable tax           | Final line amount           |

**Business Rules:**

- Product selection auto-populates code, UOM, HSN, and pricing from Module 10
- Tax calculation auto-applies based on HSN code from Module 9
- CGST+SGST applies for same-state; IGST for inter-state (based on branch vs client state)
- Unit price is editable to allow custom pricing per quotation

---

## Section 6: Pricing Summary Fields

| Field                 | Type            | Required | Notes                                   |
| --------------------- | --------------- | -------- | --------------------------------------- |
| Services Subtotal     | Auto-calculated | System   | Sum of all service line totals          |
| Products Subtotal     | Auto-calculated | System   | Sum of all product pre-tax totals       |
| Subtotal (Before Tax) | Auto-calculated | System   | Services + Products subtotals           |
| Tax Total             | Auto-calculated | System   | Sum of all tax amounts                  |
| Total Before Discount | Auto-calculated | System   | Subtotal + Tax                          |
| Discount Type         | Dropdown        | No       | Percentage / Flat Amount                |
| Discount Value        | Number          | No       | % value or fixed ₹ amount               |
| Discount Amount       | Auto-calculated | System   | Computed from type and value            |
| Grand Total           | Auto-calculated | System   | Total Before Discount − Discount Amount |

---

## Section 7: Terms & Validity Fields

| Field                    | Type      | Required    | Validation                                      | Notes                    |
| ------------------------ | --------- | ----------- | ----------------------------------------------- | ------------------------ |
| Quotation Valid Till     | Date      | Yes         | ≥ Today, default: Today + 15                    | Expiry date              |
| Payment Terms            | Dropdown  | Yes         | 100% Advance / 50-50 / Net 15 / Net 30 / Custom | Custom shows text area   |
| Custom Payment Terms     | Text Area | Conditional | Min 10 characters                               | Visible only if "Custom" |
| Special Terms/Conditions | Text Area | No          | Max 1000 characters                             | Additional T&C           |

---

## Section 8: Attachments & Notes Fields

| Field              | Type       | Required | Validation                             | Notes                      |
| ------------------ | ---------- | -------- | -------------------------------------- | -------------------------- |
| Upload Attachments | Multi File | No       | Max 5 files, 5MB each, PDF/JPG/PNG/DOC | Supporting documents       |
| Internal Notes     | Text Area  | No       | Max 500 characters                     | Not on customer-facing PDF |

---

## Actions

| Action         | Behavior                                                                               |
| -------------- | -------------------------------------------------------------------------------------- |
| Save Draft     | Saves quotation with Status = Draft, no notifications sent                             |
| Send Quotation | Validates all fields, generates PDF, sends to client via email/WhatsApp, Status → Sent |
| Cancel         | Discards changes, returns to Quotation Dashboard                                       |

---

## Automatic System Behaviors

| Trigger                   | System Action                                                      |
| ------------------------- | ------------------------------------------------------------------ |
| Source = From Lead        | Auto-populate lead details, link quotation to lead record          |
| Source = From Customer    | Auto-populate customer details, link quotation to customer record  |
| Service Selected          | Auto-fetch pricing from Module 12 based on selection in Dynamic Pricing block |
| Dynamic Selection Changed | Recalculate service pricing for affected location                  |
| Quantity / Visits Changed | Recalculate line totals and grand total                            |
| Discount Applied          | Recalculate grand total                                            |
| Send Quotation            | Generate PDF, send notification, update lead status to PROPOSAL    |
| Quotation Accepted        | Trigger Contract creation workflow (Module 18), update lead to WON |
| Validity Date Passed      | Auto-expire quotation, notify sales rep                            |

---

# 16.3 View Quotation Details

**Description:**
Read-only detailed view of a quotation showing complete pricing breakdown, service/product details per location, client information, terms, attachments, and status history. Provides a comprehensive audit trail for sales review.

---

# Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          VIEW QUOTATION DETAILS                              │
│                                                                              │
│  [← Back to Quotations]     Quotation: QT-2026-00142                        │
│                                                                              │
│  STATUS BAR                                                                  │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  ● Draft ──── ● Sent ──── ● Viewed ──── ○ Accepted/Rejected            │ │
│  │               ▲                                                         │ │
│  │          Current Status                                                 │ │
│  │  Sent On: 15-Mar-2026 10:30 AM | Viewed: 16-Mar-2026 02:15 PM          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  CLIENT INFORMATION                                                          │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Source:              From Lead (LD-2026-00142)                          │ │
│  │  Client Name:         Rahul Sharma                                      │ │
│  │  Phone:               +91 98765 43210                                   │ │
│  │  Email:               rahul@example.com                                 │ │
│  │  Address:             HSR Layout, Sector 2, Bengaluru, Karnataka        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  QUOTATION CONFIGURATION                                                     │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Quotation Type:      Service Quotation                                 │ │
│  │  Service Mode:        Contract (Annual Maintenance Contract)                 │ │
│  │  Frequency:           Monthly                                           │ │
│  │  Contract Duration:   1 Year                                            │ │
│  │  Proposed Start:      01-Apr-2026                                       │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SERVICE DETAILS BY LOCATION                                                 │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  📍 LOCATION 1: HSR Layout, Bengaluru | 1200 sqft                       │ │
│  │     Assigned Branch: BLR-HSR Branch                                     │ │
│  │                                                                         │ │
│  │  ┌────────────┬────────────┬─────────┬───────────┬────────┬────────────┐│ │
│  │  │Service     │Pricing Type│Rate (₹) │Frequency  │Visits  │Total (₹)   ││ │
│  │  │────────────┼────────────┼─────────┼───────────┼────────┼────────────││ │
│  │  │Termite Ctrl│Fixed (3BHK)│₹1,800   │Monthly    │12      │₹21,600     ││ │
│  │  │Cockroach   │Area Based  │₹2,900   │One-Time   │1       │₹2,900      ││ │
│  │  └────────────┴────────────┴─────────┴───────────┴────────┴────────────┘│ │
│  │  Location 1 Subtotal: ₹24,500                                          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  📍 LOCATION 2: Whitefield, Bengaluru | 5000 sqft                          │ │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  ┌────────────┬────────────┬─────────┬───────────┬────────┬────────────┐│ │
│  │  │Service     │Pricing Type│Rate (₹) │Frequency  │Visits  │Total (₹)   ││ │
│  │  │────────────┼────────────┼─────────┼───────────┼────────┼────────────││ │
│  │  │Rodent Ctrl │Area Based  │₹11,200  │One-Time   │1       │₹11,200     ││ │
│  │  └────────────┴────────────┴─────────┴───────────┴────────┴────────────┘│ │
│  │  Location 2 Subtotal: ₹11,200                                          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  PRICING SUMMARY                                                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Services Subtotal         : ₹26,400                                    │ │
│  │  Products Subtotal         : ₹0                                         │ │
│  │  Subtotal (Before Tax)     : ₹26,400                                    │ │
│  │  Tax (CGST + SGST)         : ₹4,752                                    │ │
│  │  Discount (10%)            : - ₹3,115                                   │ │
│  │  ──────────────────────────────────────────────────────────────────     │ │
│  │  ★ GRAND TOTAL             : ₹28,037                                   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  TERMS & VALIDITY                                                            │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Valid Till:           30-Mar-2026                                       │ │
│  │  Payment Terms:        50% Advance + 50% on Completion                  │ │
│  │  Special Terms:        Includes 2 free revisits per service per quarter │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ATTACHMENTS                                                                 │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  📄 Site_Survey_Report.pdf (1.2 MB) [Download]                          │ │
│  │  📷 Property_Photos.jpg (800 KB) [Download]                             │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  AUDIT TRAIL                                                                 │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Created:             15-Mar-2026 09:30 AM by Rajesh Kumar              │ │
│  │  Last Modified:       15-Mar-2026 10:25 AM by Rajesh Kumar              │ │
│  │  Sent:                15-Mar-2026 10:30 AM (via Email)                  │ │
│  │  Viewed by Client:    16-Mar-2026 02:15 PM                              │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  [CLOSE]  [EDIT] (Draft only)  [📥 DOWNLOAD PDF]  [📧 RESEND]               │
│           [CONVERT TO CONTRACT] (Accepted only)                              │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## View Quotation Detail Fields

| Section             | Field              | Type            | Description                                |
| ------------------- | ------------------ | --------------- | ------------------------------------------ |
| **Status Bar**      | Status Timeline    | Progress Bar    | Visual status progression                  |
|                     | Current Status     | Badge           | Active status highlighted                  |
|                     | Status Timestamps  | DateTime        | When each status was reached               |
| **Client Info**     | Source             | Badge + Link    | Lead/Customer/New with clickable reference |
|                     | Client Name        | Text            | Full name of client                        |
|                     | Phone              | Text            | Contact number                             |
|                     | Email              | Text            | Email address                              |
|                     | Address            | Text            | Full address                               |
| **Configuration**   | Quotation Type     | Text            | Service / Product / Combined               |
|                     | Service Mode       | Text            | One-Time / Contract                             |
|                     | Frequency          | Text            | Contract frequency if applicable                |
|                     | Contract Duration  | Text            | Contract duration if applicable                 |
|                     | Proposed Start     | Date            | Contract start date if applicable               |
| **Services**        | Per-location table | Table           | Service, rate, frequency, visits, total    |
|                     | Location Subtotal  | Currency        | Per-location total                         |
| **Products**        | Product table      | Table           | Product, qty, unit price, tax, line total  |
|                     | Product Subtotal   | Currency        | Total product amount                       |
| **Pricing Summary** | All summary fields | Currency        | Subtotal, tax, discount, grand total       |
| **Terms**           | Valid Till         | Date            | Expiry date                                |
|                     | Payment Terms      | Text            | Payment conditions                         |
|                     | Special Terms      | Text            | Additional T&C                             |
| **Attachments**     | Files              | File List       | Downloadable attached files                |
| **Audit**           | Created            | DateTime + User | Creation info                              |
|                     | Last Modified      | DateTime + User | Last edit info                             |
|                     | Sent               | DateTime        | When quotation was sent                    |
|                     | Viewed             | DateTime        | When client viewed (tracking)              |

---

## Actions

| Action              | Available When    | Behavior                                               |
| ------------------- | ----------------- | ------------------------------------------------------ |
| Close               | Always            | Returns to Quotation Dashboard                         |
| Edit                | Status = Draft    | Opens Edit Quotation form (same as Add, pre-populated) |
| Download PDF        | Always            | Downloads customer-facing quotation PDF                |
| Resend              | Status = Sent     | Resends quotation to client via email/WhatsApp         |
| Convert to Contract | Status = Accepted | Triggers Contract creation workflow in Module 18       |

---

# 16.4 Delete Confirmation Popup

**Description:**
Warning popup displayed when a user attempts to delete (revoke) a quotation. Only accessible when quotation status is "Draft". Provides a clear warning with quotation and client details before permanent deletion.

---

# Popup Layout

```
┌─────────────────────────────────────────────────────────────┐
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                                                      │    │
│  │  ⚠️ This quotation has not been sent to the client  │    │
│  │  and will be permanently deleted. This action        │    │
│  │  cannot be undone.                                   │    │
│  │                                                      │    │
│  │  Deletion Reason*: [▼ Created by Mistake /           │    │
│  │                      Duplicate Quotation /           │    │
│  │                      Client Withdrew Interest /      │    │
│  │                      Pricing Error /                 │    │
│  │                      Other ▼]                        │    │
│  │                                                      │    │
│  │  IF "OTHER" SELECTED:                                │    │
│  │  Specify Reason*:  [____________________________]   │    │
│  │                                                      │    │
│  │          [CANCEL]              [CONFIRM DELETE]      │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Popup Fields

| Field           | Type      | Required    | Options/Validation                                                                          | Notes                         |
| --------------- | --------- | ----------- | ------------------------------------------------------------------------------------------- | ----------------------------- |
| Deletion Reason | Dropdown  | Yes         | Created by Mistake / Duplicate Quotation / Client Withdrew Interest / Pricing Error / Other | Reason for deletion           |
| Specify Reason  | Text Area | Conditional | Min 10 characters, required if "Other" selected                                             | Custom reason                 |

---

## Actions

| Action         | Behavior                                                                           |
| -------------- | ---------------------------------------------------------------------------------- |
| Cancel         | Closes popup, returns to dashboard with no changes                                 |
| Confirm Delete | Permanently deletes quotation, logs deletion reason in audit, returns to dashboard |

---

## Post-Delete System Behaviors

| Trigger            | System Action                                                        |
| ------------------ | -------------------------------------------------------------------- |
| Quotation Deleted  | Remove quotation from all listings                                   |
| Linked to Lead     | Update lead record (remove quotation reference), status remains same |
| Linked to Customer | Update customer record (remove quotation reference)                  |
| Audit Log          | Record deletion with quotation ID, user, timestamp, and reason       |
| Notification       | Notify sales manager of quotation deletion                           |



---
=========================================================================================================


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
│  │ Service Type   : [▼ All / Contract / One-Time / Quarterly / Fogging]        │  │
│  │ Status         : [☑ Draft ☑ Approved ☑ Pending ☑ Rejected]           │  │
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
│  │GMA-00088 │ABC Pharma  │Rodent Contract  │BLR   │  1  │₹8,500    │₹11,000   │23%││
│  │GMA-00085 │DEF Mall    │General Pest│HYD   │  2  │₹14,200   │₹15,000   │6% ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Status         │Created By    │Created Date   │Actions                    ││
│  │───────────────┼──────────────┼───────────────┼───────────────────────────││
│  │✅ Approved    │Ravi Sharma  │18 Mar 2026    │[View]                     ││
│  │🟡 Pending     │Anjali M.    │17 Mar 2026    │[View]                     ││
│  │� Draft       │Suresh K.    │15 Mar 2026    │[View]                     ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   Next                              │
│                                                                              │
│  Legend: � Draft  ✅ Approved  🟡 Pending  ❌ Rejected                        │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table View Fields

| Field           | Type         | Description                                              |
| --------------- | ------------ | -------------------------------------------------------- |
| GMA ID          | Text (Auto)  | System-generated unique ID (e.g., GMA-00091)             |
| Customer/Lead Name | Text         | Customer name from Customer Master                       |
| Service Type    | Text         | Service category (Contract / One-Time)     |
| Branch name     | Text         | Branch responsible for this service proposal             |
| No. of Sites    | Number       | Total locations included in this GMA sheet               |
| Total Cost (₹)  | Currency     | Sum of all chemical + labor + revisit costs across sites |
| Sale Price (₹)  | Currency     | Total proposed price billed to customer                  |
| Gross Margin %  | Percentage   | Auto-calculated: `(Price – Cost) / Price × 100`          |
| Status          | Badge        | Draft / Approved / Pending / Rejected |
| Created By      | Text         | Sales person who created the GMA sheet                   |
| Created Date    | Date         | Date of GMA sheet creation                               |
| Actions         | Button Group | [View]                                                   |

---

## Filters

| Filter       | Type            | Options                                               |
| ------------ | --------------- | ----------------------------------------------------- |
| Service Type | Dropdown        | Contract / One-Time   |
| Status       | Multi-select    | Draft / Approved / Pending / Rejected         |
| Branch name  | Dropdown        | All Branches / Specific Branch                        |
| Date Range   | Date Range      | From – To (GMA creation date)                         |

---

## Search

Searchable by:
- GMA ID
- Service Type
- Customer/lead name
---

## Actions (Table Row)

| Action   | Description                                           |
| -------- | ----------------------------------------------------- |
| **View** | Opens the GMA sheet in read-only mode (Screen 17.1.1) |

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
│  │GMA-00091 │XYZ Hotel  │Cockroach   │42%│ 18 Mar 2026  │✅ Apprd │[View]    ││
│  │GMA-00088 │ABC Pharma │Rodent Contract  │23%│ 17 Mar 2026  │🟡 Pend │[V][Revoke]│
│  │GMA-00085 │DEF Mall   │General Pest│6% │ 15 Mar 2026  │� Draft │[View]    ││
│  │GMA-00080 │KLM Ind.   │Termite Contract │38%│ 10 Mar 2026  │✅ Apprd │[View]   ││
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
| Status         | Draft / Approved / Pending / Rejected |
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
=====================================================================================
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
│  │� Pending│                                                                ││
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

# 17.1.1 Add GMA Sheet

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
│  SECTION 2: GENERAL CONFIGURATION                                            │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Contract Duration:    [▼ 1 Year / 2 Years / Custom ▼] (Optional)       │ │
│  │  Proposed Start Date*: [📅 Date Picker]                                 │ │
│  │  Branch*:              [▼ Select Branch ▼]                              │ │
│  │  Prepared By:          [Auto: Logged-in User] (Read-only)               │ │
│  │  Prepared Date:        [Auto: Today's Date] (Read-only)                 │ │
│  │  Remarks / Notes:      [___________________________________________]    │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 3: COST BREAKDOWN (PER SITE, PER SERVICE)                           │
│  [+ ADD SITE]                                                                │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  SITE 1 (Site Name / Location*)                       [Remove Site]     │ │
│  │  ┌──────────────────────────────────────────────────────────────────┐  │ │
│  │  │ Address:  [________________________________]                     │  │ │
│  │  │ City:    [________________]  State: [▼ Select State ▼]        │  │ │
│  │  │ Category*:     [▼ Residential / Commercial / Industrial ▼]      │  │ │
│  │  │ Sub-Category*: [▼ Internal / External ▼]                        │  │ │
│  │  │ Area (sqft)*: [________]                                         │  │ │
│  │  └──────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                         │ │
│  │  [+ ADD SERVICE TO THIS SITE]                                           │ │
│  │  ┌──────────────────────────────────────────────────────────────────┐  │ │
│  │  │ SERVICE 1: [▼ Cockroach Treatment ▼]             [Remove Service]│  │ │
│  │  │                                                                  │  │ │
│  │  │ Service Mode*: [▼ Contract base ▼]                               │  │ │
│  │  │ Frequency*:    [▼ Monthly ▼]   Annual Frequency*: [ 12 ]         │  │ │
│  │  │ Visits/Month : [ 1 ] (Auto-calculated from Frequency)            │  │ │
│  │  │                                                                  │  │ │
│  │  │ ─── 3A: SERVICE VISIT COST ───────────────────────────────────── │  │ │
│  │  │ ┌──────────────────────────────────────────────────────────────┐ │  │ │
│  │  │ │ Rate per Visit (₹)*:  [ 300   ]                              │ │  │ │
│  │  │ │ Annual Frequency:     [ 12    ] (read-only)                  │ │  │ │
│  │  │ │ ──────────────────────────────────────────────────────────── │ │  │ │
│  │  │ │ SERVICE VISIT COST/YEAR (A) : ₹ 3,600  (Rate × Frequency)    │ │  │ │
│  │  │ │ SERVICE VISIT COST/MONTH    : ₹ 300    (A ÷ 12)              │ │  │ │
│  │  │ └──────────────────────────────────────────────────────────────┘ │  │ │
│  │  │                                                                  │  │ │
│  │  │ ─── 3B: MANPOWER / LABOR COST ────────────────────────────────── │  │ │
│  │  │ ┌──────────────────────────────────────────────────────────────┐ │  │ │
│  │  │ │ No. of Hours per Visit*: [ 4     ]                           │ │  │ │
│  │  │ │ Annual Frequency:        [ 12    ] (read-only)               │ │  │ │
│  │  │ │ Rate per Hour (₹)*:      [ 100   ]                           │ │  │ │
│  │  │ │ ──────────────────────────────────────────────────────────── │ │  │ │
│  │  │ │ MANPOWER COST/YEAR (B): ₹ 4,800  (Hours × Freq × Rate/Hr)    │ │  │ │
│  │  │ │ MANPOWER COST/MONTH   : ₹ 400    (B ÷ 12)                    │ │  │ │
│  │  │ └──────────────────────────────────────────────────────────────┘ │  │ │
│  │  │                                                                  │  │ │
│  │  │ ─── 3C: CHEMICAL / PRODUCT COST ──────────────────────────────── │  │ │
│  │  │ (Auto-fetched from Module 12 based on selected Pest/Service Type)│  │ │
│  │  │ ┌────────────┬─────┬────┬────────┬──────────────┬──────┬───────┐ │  │ │
│  │  │ │Product Name│Code │UOM │Coverage│ Req. Qty     │Price │Cost/Mo│ │  │ │
│  │  │ ├────────────┼─────┼────┼────────┼──────────────┼──────┼───────┤ │  │ │
│  │  │ │Alpha Cyper.│P-001│ ml │ 1200   │ [120] ml     │₹4.20 │₹504   │ │  │ │
│  │  │ ├────────────┼─────┼────┼────────┼──────────────┼──────┼───────┤ │  │ │
│  │  │ │Fipronil Gel│P-003│tube│ 1200   │ [2] tubes    │₹220  │₹440   │ │  │ │
│  │  │ └────────────┴─────┴────┴────────┴──────────────┴──────┴───────┘ │  │ │
│  │  │ [+ Add Custom Chemical]                                          │  │ │
│  │  │                                                                  │  │ │
│  │  │ Total Chemical Cost / Month  : ₹ 944                             │  │ │
│  │  │ CHEMICAL COST/YEAR (C)       : ₹ 11,328 (Cost/Month × 12)        │  │ │
│  │  │                                                                  │  │ │
│  │  │ ─── SERVICE COST TOTAL (A + B + C) ───────────────────────────── │  │ │
│  │  │ TOTAL SERVICE COST/YEAR      : ₹ 19,728                          │  │ │
│  │  └──────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                         │ │
│  │  [+ ADD SERVICE TO THIS SITE]                                           │ │
│  │                                                                         │ │
│  │  ─── 3D: WEEKENDS / NIGHTS SURCHARGE (Site-Level) ─────────────────── │ │
│  │  ┌─────────────────────────────────────────────────────────────────┐   │ │
│  │  │ Applicable?        (•) No    ( ) Yes                           │   │ │
│  │  │ IF YES: 25% surcharge on all Service Visit + Manpower Costs    │   │ │
│  │  │ SURCHARGE COST (D) : ₹ 0                                      │   │ │
│  │  └─────────────────────────────────────────────────────────────────┘   │ │
│  │                                                                         │ │
│  │  ─── 3E: DOCUMENTATION COST (Site-Level) ──────────────────────────── │ │
│  │  ┌─────────────────────────────────────────────────────────────────┐   │ │
│  │  │ Applicable?        ( ) No    (•) Yes                           │   │ │
│  │  │ Cost per Document (₹): [ 100  ]                                │   │ │
│  │  │ Docs per Month      : [ 1    ]                                 │   │ │
│  │  │ DOCUMENTATION COST/YEAR (E): ₹ 1,200                          │   │ │
│  │  └─────────────────────────────────────────────────────────────────┘   │ │
│  │                                                                         │ │
│  │  ═══ SITE 1 COST SUMMARY ════════════════════════════════════════════ │ │
│  │  ┌─────────────────────────────────────────────────────────────────┐   │ │
│  │  │ Cost Component          │ Annual (₹)     │ Monthly (₹)         │   │ │
│  │  │ ────────────────────────┼────────────────┼─────────────────────│   │ │
│  │  │ A+B+C (All Services)    │ ₹ 19,728       │ ₹ 1,644             │   │ │
│  │  │ D. Weekend/Night Surch. │ ₹ 0            │ ₹ 0                 │   │ │
│  │  │ E. Documentation Cost   │ ₹ 1,200        │ ₹ 100               │   │ │
│  │  │ ─────────────────────────────────────────────────────────────── │   │ │
│  │  │ TOTAL SITE COST         │ ₹ 20,928       │ ₹ 1,744             │   │ │
│  │  └─────────────────────────────────────────────────────────────────┘   │ │
│  │                                                                         │ │
│  │  SITE 1 PROPOSED SALE PRICE/YEAR* : ₹ [ 50,000 ] (Manual Entry)        │ │
│  │  SITE 1 PROPOSED SALE PRICE/MONTH : ₹ 4,166   (Auto ÷ 12)              │ │
│  │  ──────────────────────────────────────────────────────────────────     │ │
│  │  ★ SITE 1 GROSS MARGIN %         : 58.1%  (Auto-calculated)            │ │
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
│  │  │ GM ≥ 40%        → ✅ Approved            (Immediate)             │  │ │
│  │  │ GM < 40%        → 🟡 Requires Manual Approver Selection          │  │ │
│  │  └──────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                         │ │
│  │  CURRENT STATUS: ✅ APPROVED (GM 40.7% ≥ 40%)                           │ │
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
│        [SAVE AS DRAFT]  [SAVE & SUBMIT]  [SAVE & REQUEST APPROVAL]  [CANCEL] │
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

## Section 2: General Configuration Fields

| Field              | Type        | Required    | Options/Validation                                            | Notes                                    |
| ------------------ | ----------- | ----------- | ------------------------------------------------------------- | ---------------------------------------- |
| Contract Duration  | Dropdown    | No          | 6 Months / 1 Year / 2 Years / 3 Years / Custom                 | Optional top-level duration                |
| Proposed Start Date| Date        | Yes         | ≥ Today                                                       | Earliest service start across all sites  |
| Branch             | Dropdown    | Yes         | Active branches from Module 7                                 | Nearest branch auto-suggested            |
| Prepared By        | Auto-filled | System      | Logged-in user                                                | Read-only                                |
| Prepared Date      | Auto-filled | System      | Today's date                                                  | Read-only                                |
| Remarks / Notes    | Text Area   | No          | Max 500 characters                                            | Free-text for special instructions       |

---

## Section 3: Cost Breakdown (Per Site, Per Service)

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

---

### Service Configuration (Inside Each Site)

> Users click **[+ ADD SERVICE TO THIS SITE]** to add multiple treatments under one site block.

| Field              | Type        | Required    | Options/Validation                                            | Notes                                    |
| ------------------ | ----------- | ----------- | ------------------------------------------------------------- | ---------------------------------------- |
| Service Type       | Dropdown    | Yes         | Cockroach / Termite / Rodent / Fogging / etc.                 | Determines the chemical/costings grid    |
| Service Mode       | Dropdown    | Yes         |  / One-Time                                                | Determines frequency requirement         |
| Frequency          | Dropdown    | Conditional | Weekly / Fortnightly / Monthly / Quarterly / Custom           | Required if Mode = Contract                   |
| Annual Frequency   | Number      | Auto        | Auto-calculated (Weekly=52, Monthly=12, Quarterly=4)          | Derived from Frequency                   |
| Visits/Month       | Display     | Auto        | Auto-calculated (Annual ÷ 12)                                 | Used for chemical usage calculations     |

---

### 3A — Service Visit Cost

| Field              | Type            | Required | Validation / Notes                                                |
| ------------------ | --------------- | -------- | ----------------------------------------------------------------- |
| Rate per Visit (₹) | Currency        | Yes      | Manually entered; cost charged per visit (e.g., ₹300)             |
| Annual Frequency   | Display         | Auto     | From Service config (e.g., Monthly = 12)                          |
| Cost/Year (A)      | Auto-calculated | System   | `Rate per Visit × Annual Frequency`                               |
| Cost/Month         | Auto-calculated | System   | `A ÷ 12`                                                         |

---

### 3B — Manpower / Labor Cost

| Field                  | Type            | Required | Validation / Notes                                            |
| ---------------------- | --------------- | -------- | ------------------------------------------------------------- |
| No. of Hours per Visit | Number          | Yes      | Hours of technician work per visit (e.g., 4)                  |
| Rate per Hour (₹)      | Currency        | Yes      | Hourly rate for technician labor (e.g., ₹100)                 |
| Cost/Year (B)          | Auto-calculated | System   | `Hours × Annual Frequency × Rate/Hour`                        |
| Cost/Month             | Auto-calculated | System   | `B ÷ 12`                                                     |

---

### 3C — Chemical / Product Cost

> Automatically fetched from **Module 12** → **Module 10** based on the specific **Service Type** selected for this block.

| Field                    | Type            | Required | Validation / Notes                                                          |
| ------------------------ | --------------- | -------- | --------------------------------------------------------------------------- |
| Product Name             | Auto-filled     | System   | Auto-fetched from Module 12 service config                                |
| Coverage (SQFT)          | Number          | Yes      | **User enters** — area to be treated (defaults to Site Area)               |
| Required Qty             | Number          | Yes      | **User enters** — quantity needed per visit; must be > 0                   |
| Price / UOM (₹)          | Auto-filled     | System   | **Purchase Price from Module 10** (editable override)                      |
| Est. Cost / Month (₹)    | Auto-calculated | System   | `(Required Qty × Price per UOM) × Visits per Month`                        |

**Chemical Cost Rules:**
- Calculated individually per service added.
- `Total Chemical Cost / Year (C) = Sum of all Chemical Est.Cost/Month × 12` within this service block.

---

### 3D & 3E — Surcharges and Documentation (Site-Level)
These costs apply to the *entire site* (across all services):

| Field                     | Validation / Notes                                                              |
| ------------------------- | ------------------------------------------------------------------------------- |
| Weekend/Night Surcharge   | If Yes: 25% applied to the SUM of all Service Visit Cost (A) + Manpower Cost (B)  |
| Documentation Cost        | If Yes: `Cost/Doc × Docs/Month × 12` added to the total site cost               |

---

### Site Cost Summary (Auto-calculated per Site)

| Component                  | Formula                                                                |
| -------------------------- | ---------------------------------------------------------------------- |
| A+B+C (All Services)       | Sum of (`A + B + C`) for every service configured under this site      |
| D. Surcharge Cost          | Applied globally to A+B per site if applicable                         |
| E. Documentation Cost      | Flat site-level document cost addition                                 |
| **Site Total Cost/Year**   | **∑ (All Services A+B+C) + D + E**                                     |
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
| **Save as Draft**            | Saves the current progress without submitting. Status becomes Draft.         |
| **Save & Submit**            | Appears if GM ≥ 40%. Validates, saves, and sets status to Approved.          |
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
| At Least 1 Site Required           | Minimum one site must be added under Section 3                |
| At Least 1 Service per Site        | Every site block must have at least one Service configured    |
| Service Type Required              | Must select a service type inside each Service Block          |
| At Least 1 Chemical per Service    | Minimum one chemical row per configured service               |
| Coverage (SQFT) > 0               | Each chemical row must have SQFT > 0                          |
| Required Qty > 0                   | Each chemical row must have Required Qty > 0                  |
| Proposed Sale Price > 0            | Each site must have a proposed price greater than 0           |
| Rate per Visit > 0                 | Service visit rate cannot be zero                             |
| Hours per Visit ≥ 1               | Must have at least 1 hour per visit                           |
| Rate per Hour > 0                  | Manpower hourly rate cannot be zero                           |

---

## 💡 Note: How `[+ Add Site]` and `[+ Add Service]` Works

- **`[+ Add Site]`**: A single GMA record can cover an unlimited number of physical locations (sites) for the customer. Adding a new site creates a new, independent set of `Section 3` blocks. Pricing and Gross Margins are rolled up per-site.
- **`[+ Add Service to this Site]`**: Within a single Site, a customer may require multiple different treatments (e.g., General Pest Control + Advanced Termite Protection). Adding a service inside a site spins up a fresh cost breakdown (Service Visit Cost, Manpower, and Chemicals) strictly for that specific treatment. The system logically sums the A+B+C expenses of all services placed inside the site block to generate a combined Total Site Cost.

---

## System Behaviors on Submission

| Trigger                     | System Action                                                         |
| --------------------------- | --------------------------------------------------------------------- |
| GM ≥ 40%                    | Status → Approved; no approval routing needed                         |
| GM < 40% + Approver Picked  | Status → Pending; Notification sent to selected Approver              |
| Service Type Selected       | Auto-fill applicable chemicals from Module 12 into the Service block  |
| Chemical auto-added         | Auto-fill UOM, Base Price, +15%, +18% GST from Product Master         |
| Rate overridden by user     | System retains overridden value; does not reset on re-selection       |
| Source = From Lead          | Auto-populate lead details; link GMA to lead record                   |
| Source = From Customer      | Auto-populate customer details; link GMA to customer record           |
| Weekend/Night = Yes         | Automatically add 25% surcharge to the sum of all Manpower and Visit costs in that site |

---
====================================================================================================
# 17.1.2 View GMA Sheet (Tab 1 — Read Only)

**Description:**
Full read-only view of any GMA sheet accessible to the logged-in user. Used for auditing, reference, and management review.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       VIEW GMA SHEET  –  GMA-00091                          │
│  [← Back]                                                                    │
│                                                                              │
│  ─── HEADER ──────────────────────────────────────────────────────────────  │
│  GMA ID          : GMA-00091         Status     : ✅ APPROVED                │
│  Created By      : Ravi Sharma       Created On : 18 Mar 2026               │
│  Submitted On    : 18 Mar 2026       Approved On: 18 Mar 2026 (Immediate)   │
│                                                                              │
│  ─── SECTION 1: SOURCE INFORMATION ──────────────────────────────────────  │
│  Source Type     : From Customer                                             │
│  Customer ID     : CUST-4021         Customer Name : XYZ Hotel Chain         │
│  Phone           : +91 98765 00001   Email : ramesh@xyz.com                  │
│  Customer Type   : Commercial        Address : Andheri East, Mumbai          │
│                                                                              │
│  ─── SECTION 2: GENERAL CONFIGURATION ───────────────────────────────────  │
│  Contract Duration : 1 Year          Proposed Start : 01 Apr 2026            │
│  Branch          : Mumbai            Prepared By    : Ravi Sharma            │
│  Prepared Date   : 18 Mar 2026                                               │
│  Remarks         : Annual pest control for hotel chain                       │
│                                                                              │
│  ─── SECTION 3: SITE-WISE COST BREAKDOWN ────────────────────────────────  │
│                                                                              │
│  SITE 1: Mumbai HQ                                                           │
│  ┌────────────────────────────────────────────────────────────────────────┐ │
│  │  City: Mumbai   State: Maharashtra   Category: Commercial              │ │
│  │  Sub-Category: Internal   Area: 1200 sqft                              │ │
│  │                                                                        │ │
│  │  ─── SERVICE 1: Cockroach Treatment ────────────────────────────────── │ │
│  │  Service Mode   : Contract base     Frequency         : Monthly        │ │
│  │  Annual Freq.   : 12                Visits/Month      : 1              │ │
│  │                                                                        │ │
│  │  ─── 3A: SERVICE VISIT COST ──────────────────────────────────────── │ │
│  │  Rate per Visit : ₹300   Annual Frequency : 12                         │ │
│  │  Cost/Year (A)  : ₹3,600    Cost/Month : ₹300                         │ │
│  │                                                                         │ │
│  │  ─── 3B: MANPOWER / LABOR COST ──────────────────────────────────── │ │
│  │  Hours/Visit : 4   Annual Frequency : 12   Rate/Hour : ₹100            │ │
│  │  Cost/Year (B)  : ₹4,800    Cost/Month : ₹400                         │ │
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
│  │  Total Chemical Cost/Visit : ₹1,154   Cost/Month : ₹1,154              │ │
│  │  Chemical Cost/Year (C)    : ₹13,848                                    │ │
│  │                                                                         │ │
│  │  TOTAL SERVICE 1 COST/YEAR (A+B+C) : ₹22,248                            │ │
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
│  APPROVAL STATUS  : ✅ Approved (GM 40.7% ≥ 40%)                            │
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
│  │ 18 Mar 10:00 AM  │ Approved            │ System       │ GM 40.7%≥40% │   │
│  └──────────────────┴─────────────────────┴──────────────┴──────────────┘   │
│                                                                              │
│              [BACK]   [DOWNLOAD PDF]                                       │
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



================================================================================

# 17.2.1 View GMA Sheet (My Requests)

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
│  GMA ID          : GMA-00091         Status     : ✅ APPROVED                │
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
│  APPROVAL STATUS  : ✅ Approved (GM 40.7% ≥ 40%)                            │
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
│  │ 18 Mar 10:00 AM  │ Approved            │ System       │ GM 40.7%≥40% │   │
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
│  ─── HEADER ──────────────────────────────────────────────────────────────  │
│  GMA ID          : GMA-00088         Status     : 🟡 PENDING                 │
│  Created By      : Ravi Sharma       Created On : 17 Mar 2026               │
│  Submitted On    : 17 Mar 2026       Deadline   : 18 Mar 2026 10:00 AM      │
│                                                                              │
│  ─── SECTION 1: SOURCE INFORMATION ──────────────────────────────────────  │
│  Source Type     : From Customer                                             │
│  Customer ID     : CUST-4021         Customer Name : ABC Pharma              │
│  Phone           : +91 98765 00001   Email : facilities@abcpharma.com        │
│  Customer Type   : Commercial        Address : Peenya, Bangalore             │
│                                                                              │
│  ─── SECTION 2: SERVICE & CONTRACT CONFIGURATION ────────────────────────  │
│  Pest Type       : Rodent Contract                                                │
│  Service Mode    : Contract base     Contract Duration : 1 Year              │
│  Proposed Start  : 01 Apr 2026       Frequency : Weekly (52 visits/year)     │
│  Branch          : Bangalore         Prepared By : Ravi Sharma               │
│  Prepared Date   : 17 Mar 2026                                               │
│                                                                              │
│  ─── SECTION 3: SITE-WISE COST BREAKDOWN ────────────────────────────────  │
│                                                                              │
│  SITE 1: Bangalore Plant                                                     │
│  ┌────────────────────────────────────────────────────────────────────────┐ │
│  │  City: Bangalore   State: Karnataka   Category: Commercial             │ │
│  │  Sub-Category: Internal   Area: 5000 sqft   Visits/Month: 4           │ │
│  │                                                                         │ │
│  │  ─── 3A: SERVICE VISIT COST ──────────────────────────────────────── │ │
│  │  Rate per Visit : ₹384.6 Annual Frequency : 52   Cost/Year: ₹20,000    │ │
│  │                                                                         │ │
│  │  ─── 3B: MANPOWER / LABOR COST ──────────────────────────────────── │ │
│  │  Hours/Visit : 4   Rate/Hour : ₹144.2   Cost/Year (B) : ₹30,000         │ │
│  │                                                                         │ │
│  │  ─── 3C: CHEMICAL / PRODUCT COST ────────────────────────────────── │ │
│  │  Total Chemical Cost/Visit : ₹923     Cost/Month : ₹3,692              │ │
│  │  Chemical Cost/Year (C)    : ₹44,300                                    │ │
│  │                                                                         │ │
│  │  ─── 3D: WEEKENDS/NIGHTS SURCHARGE ──────────────────────────────── │ │
│  │  Applicable : No   Surcharge Cost (D) : ₹0                             │ │
│  │                                                                         │ │
│  │  ─── 3E: DOCUMENTATION COST ─────────────────────────────────────── │ │
│  │  Documentation Cost/Year (E) : ₹7,700                                  │ │
│  │                                                                         │ │
│  │  ═══ SITE 1 COST SUMMARY ════════════════════════════════════════    │ │
│  │  TOTAL ANNUAL COST (A+B+C+D+E) : ₹1,02,000                             │ │
│  │  Proposed Sale Price/Year      : ₹1,32,000                             │ │
│  │  ★ SITE 1 GROSS MARGIN %      : 22.7%                                 │ │
│  └────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ─── SECTION 4: OVERALL GM SUMMARY ──────────────────────────────────────  │
│  ┌──────────────────┬──────────────┬──────────────┬──────┐                  │
│  │ Site             │ Total Cost/Yr│ Sale Price/Yr│  GM% │                  │
│  │──────────────────┼──────────────┼──────────────┼──────│                  │
│  │ Site 1: Bangalore│ ₹1,02,000    │ ₹1,32,000    │ 22.7%│                  │
│  │ ──────────────────────────────────────────────────────  │                  │
│  │ TOTAL ANNUAL COST  │ ₹1,02,000                          │                  │
│  │ TOTAL ANNUAL PRICE │ ₹1,32,000                          │                  │
│  │ ★ OVERALL GROSS MARGIN : 23%                            │                  │
│  └────────────────────────────────────────────────────────┘                  │
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

## Fields Displayed (Read-Only)

| Section                  | Field                                                                                      | Display Type     |
| ------------------------ | ------------------------------------------------------------------------------------------ | ---------------- |
| **Header**               | GMA ID, Status, Created By, Created On, Submitted On, Deadline                            | Static Display   |
| **Section 1 (Source)**   | Source Type, Customer/Lead ID, Name, Phone, Email, Type, Address                           | Read-only        |
| **Section 2 (Config)**   | Pest Type, Service Mode, Contract Duration, Start Date, Frequency, Branch, Prepared By/Date | Read-only       |
| **Section 3A (per site)**| Rate per Visit, Annual Frequency, Cost/Year (A), Cost/Month                                | Read-only        |
| **Section 3B (per site)**| Hours/Visit, Annual Frequency, Rate/Hour, Cost/Year (B), Cost/Month                       | Read-only        |
| **Section 3C (per site)**| Chemical Table: Product Name, Code, UOM, Dilution, SQFT, Req.Qty, Price/UOM, Cost/Visit   | Read-only        |
| **Section 3D (per site)**| Applicable flag, Surcharge Cost (D)                                                         | Read-only        |
| **Section 3E (per site)**| Applicable flag, Cost/Doc, Docs/Month, Doc Cost/Year (E)                                   | Read-only        |
| **Site Summary**         | Cost breakdown table (A+B+C+D+E), Total Cost, Proposed Price, Site GM%                     | Read-only        |
| **Section 4 (Overall)**  | Site-wise summary, Total Annual Cost/Price, Overall GM%                                    | Read-only        |

---

## Action Fields

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
Draft → Pending → Approved / Rejected
               (Approved automatically if GM ≥ 40%)
```

## 8. Revoke Rules

- Sales person can **Revoke** only when status = **Pending** (not yet actioned by approver).
- Revoked sheets are cancelled and must be recreated if needed.
- Revoke is **not available** for Draft, Approved, or Rejected sheets.

## 10. Notifications

| Event              | Recipient        | Channel                  |
| ------------------ | ---------------- | ------------------------ |
| GMA Submitted      | Approver         | In-app + Email           |
| Approved (Auto)    | Sales Person     | In-app notification      |
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
| GM ≥ 40%                    | Status → Approved; no approval routing needed                                   |
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
│   │     ├── [Save as Draft] → Status becomes Draft
│   │     ├── GM ≥ 40% → [Save & Submit] → ✅ Approved → Contract unlocking
│   │     └── GM < 40% → [Save & Request] → [Manual Select] → 🟡 Pending → Sent to Approver
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
│  │ ✅ APPROVED      │                      │ 🟡 REGULAR FLOW         │               │
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

=========================================================================


# 🎯 MODULE 18: CUSTOMER MANAGEMENT

## Overview

A master module acting as the central repository for all onboarded clients. Converts approved leads into revenue-generating entities and provides a "360-degree view" of a customer's entire lifetime relationship — spanning active sites, contracts, and service history. Serves as the foundational data source for all downstream commercial and operational modules.

**Module Connections:**

- **Depends on:** Module 15 (Lead Management – for lead-to-customer conversion), Module 7 (Branch Management), Module 9 (Tax Management – for PAN / GST validation)
- **Used by:** Module 17 (GMA Management – "From Customer" source selection), Module 19 (Contract Management – customer is mandatory prerequisite), Module 20 (Sales Order Management), Module 16 (Quotation Management)
- **Prerequisites:** Branch must be configured (Module 7) before onboarding a customer. For lead-based conversion, the lead must be in **Qualified** or **Won** status in Module 15.

---

# 18.1 Customer Master List View

**Description:**
A paginated data table displaying all customers registered in the system. Provides key filters, search, and row-level actions. Acts as the entry point into all customer sub-screens.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         CUSTOMER MANAGEMENT                                  │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                │  │
│  │                                                                        │  │
│  │ Customer Type : [▼ All / Contract / One-time / Product ▼]             │  │
│  │ Status        : [▼ All / Active / Inactive ▼]                         │  │
│  │ Branch        : [▼ All Branches ▼]                                     │  │
│  │ Date Range    : [📅 From] – [📅 To]   (Onboarding Date)               │  │
│  │                                                                        │  │
│  │ Search: [________________________] (Customer ID / Name / Phone / GST) │  │
│  │                                                [Reset Filters]         │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│                                                  [+ ADD CUSTOMER]            │
│                                                                              │
│  CUSTOMER TABLE                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Cust ID    │Name              │Type        │Primary Contact│Branch│Status  ││
│  │───────────┼──────────────────┼────────────┼───────────────┼──────┼────────││
│  │CUST-00245 │ABC Corporation   │Contract    │+91 98765 12345│MUM   │🟢 Active││
│  │CUST-00244 │Sharma Residence  │One-time    │+91 99001 23456│BLR   │🟢 Active││
│  │CUST-00243 │XYZ Mall Pvt Ltd  │Product     │+91 98001 34567│HYD   │🔴 Inact.││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Total Sites│Created Date │Created By      │Actions                         ││
│  │───────────┼─────────────┼────────────────┼────────────────────────────────││
│  │3          │15 Jan 2026  │System Admin    │[View] [Edit] [Deactivate]      ││
│  │1          │10 Feb 2026  │Rahul Sharma    │[View] [Edit] [Deactivate]      ││
│  │2          │05 Mar 2026  │Priya Kumar     │[View] [Edit] [Activate]        ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   Next                              │
│                                                                              │
│  Legend: 🟢 Active   🔴 Inactive                                              │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table View Fields

| Field             | Type         | Description                                                      |
| ----------------- | ------------ | ---------------------------------------------------------------- |
| Customer ID       | Text (Auto)  | System-generated unique ID (e.g., CUST-00245)                    |
| Name              | Text         | Company name (Contract/Product) or Full name (One-time)          |
| Customer Type     | Badge        | Contract / One-time / Product                                       |
| Primary Contact   | Text         | Primary phone number of the account contact                      |
| Primary Email     | Text         | Primary email address of the account contact                      |
| Branch Name       | Text         | Servicing branch assigned to this customer                       |
| Status            | Badge        | Active / Inactive                                                |
| Total Sites       | Number       | Total number of sites                                            |
| Created Date      | Date         | Date of customer creation                                        |
| Created By        | Text         | User who created the customer                                    |
| Actions           | Button Group | [View] [Edit] [Deactivate / Activate]                            |

---

## Filters

| Filter        | Type       | Options                                          |
| ------------- | ---------- | ------------------------------------------------ |
| Customer Type | Dropdown   | All / Contract / One-time / Product              |
| Status        | Dropdown   | All / Active / Inactive                          |
| Branch        | Dropdown   | All Branches / Specific Branch (from Module 7)   |
| Date Range    | Date Range | From – To (Customer onboarding / creation date)  |

---

## Search

Searchable by:
- Customer ID
- Customer Name
- Phone Number

---

## Actions (Table Row)

| Action          | Condition          | Description                                                 |
| --------------- | ------------------ | ----------------------------------------------------------- |
| **View**        | All statuses       | Opens the customer detail dashboard (Screen 18.3)           |
| **Edit**        | Active customers   | Opens the Edit Customer form (Screen 18.4)                  |
| **Deactivate**  | Status = Active    | Opens the deactivation confirmation dialog (Screen 18.5)    |
| **Activate**    | Status = Inactive  | Re-activates the customer record, sets status back to Active |

---

## Form Actions

| Action            | Description                                                          |
| ----------------- | -------------------------------------------------------------------- |
| **+ Add Customer**| Opens the **Add Customer Form** (Screen 18.2)                        |

---

================================================================================

# 18.2 Add Customer Form

**Description:**
A multi-section form to register a new customer — either by converting an existing qualified Lead (auto-fill) or by entering details manually. Captures entity information, billing & contact details, and the first physical service site.

> **Note:** This screen is also triggered from the Lead Management module (Module 15) via the **[Convert to Customer]** action on a Qualified lead.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Customer List]           ADD CUSTOMER                            │
│                                                                              │
│  Customer ID: CUST-XXXX (Auto-generated on save)                             │
│                                                                              │
│  SECTION 1: SOURCE                                                           │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Entry Mode*:                                                           │ │
│  │  (•) Import from Lead    ( ) Manual Entry                               │ │
│  │                                                                         │ │
│  │  IF "IMPORT FROM LEAD" SELECTED:                                        │ │
│  │  Select Lead*:    [🔍 Search Lead by Name / ID / Phone ▼]              │ │
│  │                                                                         │ │
│  │  AUTO-FILLED FROM LEAD:                                                 │ │
│  │  Lead ID         : LD-2026-00142  (Read-only)                          │ │
│  │  Lead Name       : Rahul Sharma   (Read-only)                          │ │
│  │  Phone           : +91 98765 43210 (Read-only)                         │ │
│  │  Email           : rahul@example.com (Read-only)                       │ │
│  │  Lead Type       : Contract  (Read-only)                               │ │
│  │  Lead Status     : QUALIFIED ✅ (Read-only)                             │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 2: ENTITY INFORMATION                                               │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Customer Type*  : (•) Contract    ( ) One-time    ( ) Product         │ │
│  │                                                                         │ │
│  │  Entity / Full Name* : [________________________________]              │ │
│  │  Industry Type   : [▼ Hospitality / Healthcare / Retail / Other ▼]     │ │
│  │  PAN Number      : [__________] (e.g., AAAAA0000A)                    │ │
│  │  GST Number      : [__________________] (e.g., 22AAAAA0000A1Z5)       │ │
│  │  Contact Person* : [________________________________]                  │ │
│  │  Designation     : [________________________________]                  │ │
│  │  Phone*          : [__________________]                                │ │
│  │  Alternate Phone : [__________________]                                │ │
│  │  Email*          : [________________________________]                  │ │
│  │  Branch*         : [▼ Select Branch ▼]                                 │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 3: BILLING & CONTACT DETAILS                                        │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Billing Address Line 1*  : [________________________________]         │ │
│  │  Billing Address Line 2   : [________________________________]         │ │
│  │  City*                    : [________________]                         │ │
│  │  State*                   : [▼ Select State ▼]                         │ │
│  │  Pincode*                 : [________]                                 │ │
│  │                                                                         │ │
│  │  ── Finance / Accounts Point of Contact ──                             │ │
│  │  Finance Contact Name*    : [________________________________]         │ │
│  │  Finance Contact Phone*   : [__________________]                       │ │
│  │  Finance Contact Email    : [________________________________]         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [SAVE AS DRAFT]         [SAVE & SUBMIT]         [CANCEL]               │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Section 1: Source Fields

| Field       | Type            | Required    | Options / Validation                                                    | Notes                                           |
| ----------- | --------------- | ----------- | ----------------------------------------------------------------------- | ----------------------------------------------- |
| Entry Mode  | Radio           | Yes         | Import from Lead / Manual Entry                                         | Determines which sub-fields are shown below     |
| Select Lead | Search Dropdown | Conditional | Leads with Status = Qualified or Won (from Module 15)                   | Required if Entry Mode = Import from Lead       |
| Lead ID     | Auto-filled     | System      | Read-only                                                               | Populated on lead selection                     |
| Lead Name   | Auto-filled     | System      | Read-only                                                               | Populated on lead selection                     |
| Phone       | Auto-filled     | System      | Read-only                                                               | Populated on lead selection                     |
| Email       | Auto-filled     | System      | Read-only                                                               | Populated on lead selection                     |
| Lead Type   | Auto-filled     | System      | Read-only                                                               | Determines default Customer Type in Section 2   |
| Lead Status | Auto-filled     | System      | Read-only                                                               | Must be Qualified or Won to allow conversion    |

> **Note:** Only leads with status **Qualified** or **Won** appear in the Lead search dropdown. Leads in Pending, New, or Lost status cannot be converted to customers.

---

## Section 2: Entity Information Fields

| Field               | Type      | Required | Validation                                              | Notes                                   |
| ------------------- | --------- | -------- | ------------------------------------------------------- | --------------------------------------- |
| Customer Type       | Radio     | Yes      | Contract / One-time / Product                           | Customer categorization metric          |
| Entity / Full Name  | Text      | Yes      | Min 3 chars, Max 100 chars                              | Legal entity name or individual's name    |
| Industry Type       | Dropdown  | No       | Hospitality / Healthcare / Education / IT / Other       | Used for segmentation                   |
| PAN Number          | Text      | No       | Format: 5 alpha + 4 digits + 1 alpha (e.g., AAAAA0000A) | Validated against standard PAN regex    |
| GST Number          | Text      | No       | Format: 15-char alphanumeric (e.g., 22AAAAA0000A1Z5)  | Optional; validated if provided         |
| Contact Person      | Text      | Yes      | Min 3 chars, Max 100 chars                              | Primary decision-maker contact          |
| Designation         | Text      | No       | Max 100 chars                                           | Job title of contact person             |
| Phone               | Number    | Yes      | 10-digit Indian mobile number                           | Primary contact number                  |
| Alternate Phone     | Number    | No       | 10-digit Indian mobile number                           | Optional secondary contact              |
| Email               | Email     | Yes      | Valid email format                                      | Used for all communications & invoices  |
| Branch              | Dropdown  | Yes      | Active branches from Module 7                           | Servicing branch for this customer      |

---

## Section 3: Billing & Contact Details Fields

| Field                  | Type     | Required | Validation                            | Notes                                       |
| ---------------------- | -------- | -------- | ------------------------------------- | ------------------------------------------- |
| Billing Address Line 1 | Text     | Yes      | Min 10 chars, Max 200 chars           | Street, building, locality                  |
| Billing Address Line 2 | Text     | No       | Max 200 chars                         | Landmark, area                              |
| City                   | Text     | Yes      | Min 3 chars                           | Billing city                                |
| State                  | Dropdown | Yes      | Indian states list                    | Billing state                               |
| Pincode                | Number   | Yes      | 6-digit numeric                       | India PIN code                              |
| Finance Contact Name   | Text     | Yes      | Min 3 chars, Max 100 chars            | Accounts/Finance department point-of-contact|
| Finance Contact Phone  | Number   | Yes      | 10-digit Indian mobile number         | Finance contact's phone                     |
| Finance Contact Email  | Email    | No       | Valid email format                    | Finance contact's email (for invoices)      |

---

## Form Actions

| Button             | Description                                                                  |
| ------------------ | ---------------------------------------------------------------------------- |
| **Save as Draft**  | Saves the form without finalising. Customer appears in list with Draft status.|
| **Save & Submit**  | Validates all required fields and creates the Customer record as Active.      |
| **Cancel**         | Discards all entries and returns to the Customer Master List.                 |

---

## Validation Rules

| Validation                            | Rule                                                                    |
| ------------------------------------- | ----------------------------------------------------------------------- |
| Entry Mode Required                   | Must select Import from Lead or Manual Entry                            |
| Lead Selection Required               | If Entry Mode = Import from Lead, a valid qualified lead must be chosen |
| Customer Type Required                | Must select Contract, One-time, or Product                              |
| PAN Format                            | If provided, must match AAAAA0000A pattern                              |
| GST Format                            | If provided, must be 15-char alphanumeric starting with valid state code|
| Duplicate Phone Check                 | System warns if phone number already exists on another customer record  |
| Duplicate GST Check                   | System blocks creation if GST number already registered                 |
| Billing Address Required              | At least Line 1, City, State, Pincode must be filled                   |
| Finance Contact Required              | Name and Phone are mandatory                                            |
| Branch Required                       | Must select an active branch                                            |

---

## System Behaviors on Save

| Trigger                    | System Action                                                                   |
| -------------------------- | ------------------------------------------------------------------------------- |
| Entry Mode = Import Lead   | Auto-populates all lead fields; links created customer back to the Lead record  |
| Save & Submit              | Customer status set to Active; Customer ID generated (e.g., CUST-00245)          |
| Duplicate GST detected     | Blocks save and shows error: "GST already registered under another customer"     |
| Lead converted             | Lead status updated to **Customer** (Won) in Module 15                           |

---

================================================================================

# 18.3 View Customer Details

**Description:**
A unified, read-only dashboard for a specific customer. Provides a 360-degree view split across three logical tabs — Basic Details & Sites, Contract Logs, and Sales Orders & Service History. All data is display-only; modifications happen via the Edit action.

================================================================================

# 18.3.1 Tab 1: Basic Details

**Description:**
Displays the full entity profile — entity info and billing address for this customer.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Tab 1: Basic Details ●]          [Tab 2: Contract Logs]                   │
│  [Tab 3: Sales Orders & Service History]                                     │
│                                                                              │
│  ─── ENTITY INFORMATION ──────────────────────────────────────────────────  │
│  Entity / Name   : ABC Corporation               Type: Contract              │
│  Industry        : Hospitality                   PAN : AAAAA0000A            │
│  GST             : 27AAAAA0000A1Z5               Branch: Mumbai              │
│  Contact Person  : Rahul Mehta  │  Designation: General Manager            │
│                                                                              │
│  ─── BILLING ADDRESS ─────────────────────────────────────────────────────  │
│  Address         : 4th Floor, Crystal Tower, Andheri East, Mumbai           │
│  City            : Mumbai    │  State: Maharashtra    │  PIN: 400069         │
│  Finance Contact : Priya Sharma   │  Phone: +91 99001 23456                 │
│  Finance Email   : accounts@abccorp.com                                     │
│                                                                              │
│  Pagination: Previous  1  2  Next                                            │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Entity Information Fields (Read-Only)

| Field              | Type    | Description                                            |
| ------------------ | ------- | ------------------------------------------------------ |
| Entity / Name      | Display | Legal entity name or individual's name                 |
| Customer Type      | Display | Contract / One-time / Product                          |
| Industry           | Display | Sector classification (if applicable)                  |
| PAN Number         | Display | Tax ID                                                 |
| GST Number         | Display | GST registration (if provided)                         |
| Branch             | Display | Assigned servicing branch                              |
| Contact Person     | Display | Primary decision-maker                                 |
| Designation        | Display | Job title                                              |
| Phone              | Display | Primary contact number                                 |
| Alternate Phone    | Display | Secondary contact (if provided)                        |
| Email              | Display | Email address                                          |
| LTV                | Currency| Lifetime Value — auto-calculated sum of all SO values   |
| Onboarded Date     | Date    | Date when customer was created / activated             |

---

## Billing Address Fields (Read-Only)

| Field                | Type    | Description                              |
| -------------------- | ------- | ---------------------------------------- |
| Billing Address      | Display | Full billing address                     |
| City / State / PIN   | Display | Location details                         |
| Finance Contact Name | Display | Accounts department contact              |
| Finance Contact Phone| Display | Finance phone                            |
| Finance Contact Email| Display | Finance email                            |

---



================================================================================

# 18.3.2 Tab 2: Contract Logs

**Description:**
A chronological grid listing all agreements (both historical and active) associated with this customer. Provides a snapshot of contract status and links to the full contract record in Module 19.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Tab 1: Basic Details]          [Tab 2: Contract Logs ●]                   │
│  [Tab 3: Sales Orders & Service History]                                     │
│                                                                              │
│  CONTRACTS GRID                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Contract ID  │Start Date  │End Date    │Contract Value│GMA ID  │Status    ││
│  │─────────────┼────────────┼────────────┼──────────────┼────────┼──────────││
│  │CON-2026-0041│01 Jan 2026 │31 Dec 2026 │₹ 1,56,000    │GMA-091 │✅ Active  ││
│  │CON-2025-0018│01 Jan 2025 │31 Dec 2025 │₹ 1,10,000    │GMA-044 │📁 Expired ││
│  │CON-2024-0009│01 Jul 2024 │15 Sep 2024 │₹ 45,000      │GMA-021 │❌ Termin. ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination: Previous  1  2  Next                                            │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Contracts Grid Fields

| Field          | Type    | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| Contract ID    | Link    | System ID; clickable — navigates to Module 19 contract detail|
| Start Date     | Date    | Contract commencement date                                   |
| End Date       | Date    | Contract scheduled end date                                  |
| Contract Value | Currency| Total value of the contract (₹)                             |
| GMA ID         | Link    | Source GMA sheet; clickable — navigates to Module 17 GMA view|
| Status         | Badge   | Active / Expired / Terminated / Draft                        |

> **Note:** This tab is **read-only**. To create a new contract for this customer, navigate to **Module 19 (Contract Management)** and select this customer as the source. A valid approved GMA (Module 17) is required before a contract can be created.

---

================================================================================

# 18.3.3 Tab 3: Sales Orders & Service History

**Description:**
A consolidated grid showing all Sales Orders and their real-time service execution status linked to this customer. Bridges the commercial (sales) and operational (execution) views.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Tab 1: Basic Details]          [Tab 2: Contract Logs]                     │
│  [Tab 3: Sales Orders & Service History ●]                                   │
│                                                                              │
│  SALES ORDERS & SERVICE HISTORY                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │SO Number   │SO Date    │Linked Contract│Order Type│Total Value│SO Status ││
│  │────────────┼───────────┼───────────────┼──────────┼───────────┼──────────││
│  │SO-2026-0112│10 Mar 2026│CON-2026-0041  | Contract      │₹ 13,000   │✅ Open   ││
│  │SO-2026-0087│10 Feb 2026│CON-2026-0041  | Contract       │₹ 13,000   │✅ Fulfld ││
│  │SO-2026-0043│15 Jan 2026│—              │One-Time  │₹ 8,500    │✅ Fulfld ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Service Status                                                            ││
│  │──────────────────────────────────────────────────────────────────────────││
│  │🕐 Scheduled                                                              ││
│  │✅ Completed                                                              ││
│  │✅ Completed                                                              ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination: Previous  1  2  3  Next                                         │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Sales Orders Grid Fields

| Field            | Type    | Description                                                        |
| ---------------- | ------- | ------------------------------------------------------------------ |
| SO Number        | Link    | System ID for the Sales Order; navigates to Module 20 SO detail    |
| SO Date          | Date    | Date the Sales Order was generated                                 |
| Linked Contract  | Link    | Contract ID the SO was generated from (blank for standalone SOs)   |
| Order Type       | Text    |  Contract / One-Time Service / Product Sale                     |
| Total Value (₹)  | Currency| Total invoiced value of this Sales Order                           |
| SO Status        | Badge   | Draft / Open / Fulfilled / Billed / Cancelled                      |
| Service Status   | Badge   | Scheduled / In Progress / Completed / Pending                      |

> **Note:** This tab is **read-only**. Sales Orders are created and managed in **Module 20 (Sales Order Management)**. Service Status is updated by the Operations module once Job Cards are dispatched.

---

================================================================================

# 18.4 Edit Customer Form

**Description:**
Pre-filled form mirroring the Add Customer form (18.2). Allows authorised users to update master data — billing address, contact persons, entity details. All changes are logged in the Master Data Audit Log.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Customer Profile]     EDIT CUSTOMER   CUST-00245                │
│                                                                              │
│  SECTION 2: ENTITY INFORMATION  (Pre-filled; editable fields below)         │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Customer Type   : Contract  [Locked — cannot change after creation]    │ │
│  │  Entity / Name*  : [ABC Corporation________________]                    │ │
│  │  Industry Type   : [▼ Hospitality ▼]                                    │ │
│  │  PAN Number      : AAAAA0000A  [Locked — audit-sensitive]               │ │
│  │  GST Number      : [27AAAAA0000A1Z5__________] (Editable)               │ │
│  │  Contact Person* : [Rahul Mehta__________]                              │ │
│  │  Designation     : [General Manager________________]                    │ │
│  │  Phone*          : [+91 98765 12345_______________]                     │ │
│  │  Alternate Phone : [________________________________]                   │ │
│  │  Email*          : [rahul@abccorp.com_______________]                   │ │
│  │  Branch*         : [▼ Mumbai ▼]                                          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 3: BILLING & CONTACT DETAILS (Pre-filled; fully editable)          │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Billing Address Line 1*  : [4th Floor, Crystal Tower______]            │ │
│  │  Billing Address Line 2   : [Andheri East____________________]          │ │
│  │  City*                    : [Mumbai______________]                      │ │
│  │  State*                   : [▼ Maharashtra ▼]                           │ │
│  │  Pincode*                 : [400069_]                                   │ │
│  │  Finance Contact Name*    : [Priya Sharma___________]                   │ │
│  │  Finance Contact Phone*   : [+91 99001 23456________]                   │ │
│  │  Finance Contact Email    : [accounts@abccorp.com___]                   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │

│                                                                              │
│       [SAVE CHANGES]                                          [CANCEL]      │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Editable vs. Locked Fields

| Field                  | Editable? | Notes                                                          |
| ---------------------- | --------- | -------------------------------------------------------------- |
| Customer Type          | ❌ Locked  | Cannot change Contract/Product → One-time or vice versa        |
| Entity / Name          | ✅ Yes     | Can correct spelling or reflect legal name change              |
| Industry Type          | ✅ Yes     | Dropdown update allowed                                        |
| PAN Number             | ❌ Locked  | Tax-sensitive; requires admin override with audit note         |
| GST Number             | ✅ Yes     | Can be added or updated                                        |
| Contact Person         | ✅ Yes     | Contact person can change                                      |
| Designation            | ✅ Yes     | Free text update allowed                                       |
| Phone / Alternate      | ✅ Yes     | Duplicate phone check applies on save                          |
| Email                  | ✅ Yes     | Valid email format check                                       |
| Branch                 | ✅ Yes     | Can reassign to another active branch                          |
| Billing Address        | ✅ Yes     | Fully editable                                                 |
| Finance Contact        | ✅ Yes     | Fully editable                                                 |


---

## Validation Rules

| Validation                   | Rule                                                              |
| ---------------------------- | ----------------------------------------------------------------- |
| Required fields remain filled| All previously required fields must still have valid values       |
| GST Format (if edited)       | Must be valid 15-char alphanumeric                               |
| Duplicate Phone Check        | Warns if phone number already exists on another active customer   |
| Duplicate GST Check          | Blocks save if updated GST matches another registered customer    |
| Branch Active                | Selected branch must be Active in Module 7                        |

---

## Audit Log Behavior

Every save triggers an **audit log entry** recording:

| Log Field      | Value                              |
| -------------- | ---------------------------------- |
| Changed By     | Logged-in user ID and name         |
| Changed On     | Timestamp                          |
| Fields Changed | Array of changed field names       |
| Previous Value | Before-state for each changed field|
| New Value      | After-state for each changed field |

> **Note:** The customer **Master Data Audit Log** can be viewed by Admin and Finance Auditor roles. It is accessible from the Customer Detail page under the audit trail link.

---

## Form Actions

| Button            | Description                                                                   |
| ----------------- | ----------------------------------------------------------------------------- |
| **Save Changes**  | Validates all required fields, saves updates, records audit log entry          |
| **Cancel**        | Discards all unsaved changes and returns to the Customer Detail view           |

---

================================================================================

# 18.5 Delete (Deactivate) Customer

**Description:**
A soft-delete mechanism that marks a customer as **Inactive** rather than permanently removing them from the system. All historical data (contracts, sales orders, sites) is preserved for audit and reporting. Hard deletion is not permitted.

---

## Screen Layout

```
┌───────────────────────────────────────────────────────┐
│           DEACTIVATE CUSTOMER                          │
│                                                        │
│                                                        │
│  ⚠️  WARNING: This action will deactivate the customer │
│  record. Active contracts and pending sales orders     │
│  must be resolved before deactivation.                 │
│                                                        │
│  ELIGIBILITY CHECK:                                    │
│  Active Contracts   : ❌  2 Active Contracts found     │
│  Open Sales Orders  : ❌  1 Open Sales Order found     │
│                                                        │
│  ──  Deactivation is BLOCKED.  ──                      │
│  Please close or terminate all active contracts        │
│  and sales orders before deactivating this customer.   │
│                                                        │
│              [OK — Go Back]                            │
└───────────────────────────────────────────────────────┘
```

*When eligible (no active contracts / open SOs):*

```
┌───────────────────────────────────────────────────────┐
│           DEACTIVATE CUSTOMER                          │
│                                                        │
│  Customer   : DEF Industries (CUST-00243)              │
│                                                        │
│  ELIGIBILITY CHECK:                                    │
│  Active Contracts   : ✅  None                         │
│  Open Sales Orders  : ✅  None                         │
│                                                        │
│  Reason for Deactivation*:                             │
│  [▼ Select Reason ▼]                                   │
│      • Customer Relocated                              │
│      • Business Closure                               │
│      • Non-Renewal                                     │
│      • Payment Default                                 │
│      • Other                                           │
│                                                        │
│  [CONFIRM DEACTIVATE]        [CANCEL]                  │
└───────────────────────────────────────────────────────┘
```

---

## Deactivation Prerequisite Check

| Check                   | Condition to Block                                       | Error Message Shown                                       |
| ----------------------- | -------------------------------------------------------- | --------------------------------------------------------- |
| Active Contracts        | Customer has ≥ 1 contract with Status = Active           | "X Active Contract(s) must be terminated before deactivation" |
| Open Sales Orders       | Customer has ≥ 1 SO with Status = Open or Draft          | "X Open Sales Order(s) must be fulfilled or cancelled first" |

---

## Deactivation Form Fields

| Field                | Type     | Required    | Options / Validation                                                                   |
| -------------------- | -------- | ----------- | -------------------------------------------------------------------------------------- |
| Reason for Deactivation | Dropdown | Yes      | Customer Relocated / Business Closure / Non-Renewal / Payment Default / Other          |

---

## Validation Rules

| Validation                    | Rule                                                          |
| ----------------------------- | ------------------------------------------------------------- |
| No Active Contracts           | Deactivation fails if any contract has Status = Active        |
| No Open Sales Orders          | Deactivation fails if any SO has Status = Open or Draft       |
| Reason Required               | A reason must be selected from the dropdown                   |
| Remarks Required (if Other)   | Free-text remark is mandatory when "Other" reason is selected |

---

## System Behavior on Deactivation

| Trigger                      | System Action                                                             |
| ---------------------------- | ------------------------------------------------------------------------- |
| Confirm Deactivate clicked   | Customer status set to **Inactive**                                        |
| Status change applied        | Customer no longer appears in default Active listing (remains searchable) |
| Sites linked                 | All linked sites are also flagged Inactive automatically                   |
| Audit log                    | Deactivation event logged with Reason, Timestamp, and user who acted      |
| Reactivation possible        | Admin can reactivate the customer from the list by clicking [Activate]    |

---

================================================================================

## RBAC – Role-Based Access Control

| Role              | List View (18.1)          | Add Customer (18.2) | View Details (18.3) | Edit (18.4) | Deactivate (18.5) |
| ----------------- | ------------------------- | ------------------- | ------------------- | ----------- | ----------------- |
| Sales Person      | View (own branch)         | ✅                   | ✅                   | ✅           | ❌                 |
| Sales Manager     | View (own branch / team)  | ✅                   | ✅                   | ✅           | ✅                 |
| Branch Manager    | View (own branch)         | ✅                   | ✅                   | ✅           | ✅                 |
| Head Ops / Admin  | View (all branches)       | ✅                   | ✅                   | ✅           | ✅                 |
| Finance Auditor   | View (all – read-only)    | ❌                   | ✅                   | ❌           | ❌                 |

---

================================================================================

## Navigation Flow Summary

```
MODULE 18: CUSTOMER MANAGEMENT
│
├── 18.1 Customer Master List
│   ├── [+ Add Customer] → 18.2 Add Customer Form
│   │     ├── [Save as Draft]   → Customer created (Draft)
│   │     └── [Save & Submit]   → ✅ Customer created (Active)
│   │           └── Lead → Customer link established in Module 15
│   │
│   ├── [View] → 18.3 View Customer Details
│   │     ├── Tab 1: Basic Details & Sites
│   │     │     └── [+ Add Site] → Site sub-form (inline)
│   │     ├── Tab 2: Contract Logs
│   │     │     └── [Contract ID link] → Module 19 Contract Detail
│   │     └── Tab 3: Sales Orders & Service History
│   │           └── [SO Number link] → Module 20 Sales Order Detail
│   │
│   ├── [Edit] → 18.4 Edit Customer Form
│   │     ├── Modify master data → Audit log recorded
│   │     └── [+ Add New Site] → New site linked to customer
│   │
│   └── [Deactivate] → 18.5 Deactivation Dialog
│         ├── Eligibility Check → Block if active contracts / SOs
│         └── Confirm with Reason → Customer set to Inactive
```

---

================================================================================

## Module Dependency Map

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                                                                                  │
│   MODULE 15             MODULE 7             MODULE 9                            │
│   Lead Management       Branch Management    Tax Management                      │
│   ┌──────────┐         ┌──────────┐         ┌──────────┐                        │
│   │ Lead ID, │         │ Branch   │         │ PAN /    │                        │
│   │ Name,    │         │ list for │         │ GST      │                        │
│   │ Phone,   │         │ dropdown │         │ Validation│                       │
│   │ Type,    │         │ selection│         │ rules    │                        │
│   │ Status   │         └─────┬────┘         └─────┬────┘                        │
│   └─────┬────┘               │                    │                             │
│         │                    │                    │                             │
│         └─────────────┐  ┌───┘        ────────────┘                             │
│                       │  │                                                      │
│                       ▼  ▼                                                      │
│               ╔═══════════════════════════════════╗                             │
│               ║    MODULE 18: CUSTOMER MANAGEMENT  ║                             │
│               ║                                    ║                             │
│               ║  • Entity Info (Contract/One-time) ║                             │
│               ║  • Billing & Contact Details       ║                             │
│               ║  • Sites Master (Site ID, SQFT)    ║                             │
│               ║  • LTV Tracking                    ║                             │
│               ╚══════════╤════════════════╤════════╝                             │
│                          │                │                                     │
│              ┌───────────┘        ┌───────┘                                     │
│              ▼                    ▼                                             │
│   ╔══════════════════╗  ╔═══════════════════════╗                               │
│   ║  MODULE 17       ║  ║  MODULE 19             ║                               │
│   ║  GMA Management  ║  ║  Contract Management   ║                               │
│   ║  (From Customer  ║  ║  (Customer prerequisite║                               │
│   ║   source)        ║  ║   for contract)        ║                               │
│   ╚══════════════════╝  ╚═══════════════════════╝                               │
│                                   │                                             │
│                                   ▼                                             │
│                        ╔═══════════════════════╗                                │
│                        ║  MODULE 20             ║                                │
│                        ║  Sales Order Mgmt      ║                                │
│                        ╚═══════════════════════╝                                │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## Data Flow Table

| Source Module  | Data Provided                                           | Used In (Customer Mgmt)                              |
| -------------- | ------------------------------------------------------- | ---------------------------------------------------- |
| **Module 15**  | Lead ID, Name, Phone, Email, Type, Status               | Section 1: Source Selection (Import from Lead)       |
| **Module 7**   | Branch list (Active branches)                           | Section 2: Branch dropdown                           |
| **Module 9**   | PAN / GST validation rules                              | Section 2: Entity Information (format validation)    |
| **Module 18**  | Customer ID, Site IDs, Billing Address                  | Module 17 (GMA), Module 19 (Contract), Module 20 (SO)|
| **Module 19**  | Contract counts and status per customer                 | 18.3 Tab 2: Contract Logs & LTV calculation          |
| **Module 20**  | Sales Order counts and service history per customer     | 18.3 Tab 3: Sales Orders & Service History           |



=======================================================================================