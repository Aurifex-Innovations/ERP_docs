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

| Filter Name  | Type                | Default      | Options                                                                     | Purpose                          |
| ------------ | ------------------- | ------------ | --------------------------------------------------------------------------- | -------------------------------- |
| Request Type | Dropdown            | All          | Stock Request, Transfer Request                                             | Separate stock vs transfer flows |
| Status       | Multi-Select        | All          | Pending, Approved, Rejected, Dispatch, In Transit, Received, Issue Reported | Track lifecycle stage            |
| Direction    | Dropdown            | All          | Inward, Outward                                                             | Identify incoming vs outgoing    |
| Branch       | Searchable Dropdown | User Branch  | All Branches / Specific Branch                                              | Filter by source/destination     |
| Date Range   | Date Picker         | Last 30 Days | Custom Range                                                                | Filter based on request creation |
| Priority     | Dropdown            | All          | Low, Normal, High, Urgent                                                   | Focus on critical requests       |

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
│  │ Purpose               : Monthly AMC Services                            ││
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
│ Purpose                : Monthly AMC Services                               │
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
│ │Img  │Service ID  │Service Name     │Category  │Pest Type   │Price Type │ │
│ ├─────┼────────────┼─────────────────┼──────────┼────────────┼───────────┤ │
│ │🐜   │SVC-001     │Termite Control  │Res.      │Termite     │Fixed      │ │
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
| Category      | Text            | Required | Residential / Commercial          |
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
│  (Integrated with Module 10 Product Master)                                       │
│                                                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ [Search Product from Product Master...]                                       │  │
│  │                                                                              │  │
│  │ Selected Chemicals                                                           │  │
│  │                                                                              │  │
│  │ ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ │ Product Name │ Product Code │ UOM │ Standard Usage │ Required Qty │     │ │
│  │ ├──────────────┼──────────────┼─────┼────────────────┼──────────────┤     │ │
│  │ │ Alpha Cypermethrin │ P-001 │ ml │ 10 ml │ [____] ml │              │ │
│  │ │ Chlorpyriphos │ P-002 │ ml │ 20 ml │ [____] ml │                     │ │
│  │ └─────────────────────────────────────────────────────────────────────────┘ │
│  │                                                                              │
│  │ [+ Add Custom Chemical]                                                      │
│  └──────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  PRICING CONFIGURATION                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ Price Type                                                                   │  │
│  │ (•) Fixed Price                                                              │  │
│  │ ( ) Area Based                                                               │  │
│  │ ( ) Inspection Based                                                         │  │
│  │                                                                              │  │
│  │ Residential Pricing                                                          │  │
│  │ 1BHK [₹____] 2BHK [₹____] 3BHK [₹____] 4BHK+ [₹____]                         │  │
│  │ [+ Add Custom Property Type]                                                 │  │
│  │                                                                              │  │
│  │ Commercial Pricing                                                           │  │
│  │ Small Office [₹____]                                                         │  │
│  │ Medium Office [₹____]                                                        │  │
│  │ Large Office [₹____]                                                         │  │
│  │ Warehouse [₹____]                                                            │  │
│  │ [+ Add Custom Commercial Type]                                               │  │
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

| Field             | Type                  | Required | Validation                                          |
| ----------------- | --------------------- | -------- | --------------------------------------------------- |
| Service Name      | Text                  | Yes      | Minimum 3 characters, must be unique                |
| Service Category  | Multi Select Checkbox | Yes      | At least one category must be selected              |
| Custom Category   | Text                  | No       | Appears only if user clicks **Add Custom Category** |
| Service Code      | Auto Generated        | Yes      | System generated format: `SRV-XXXX`                 |
| Pest Type Covered | Multi Select Checkbox | Yes      | Minimum one pest must be selected                   |
| Custom Pest Type  | Text                  | No       | Enabled when **Add Custom Pest Type** selected      |
| Description       | Text Area             | Yes      | Minimum 10 characters                               |
| Service Duration  | Number + UOM          | Yes      | Value must be > 0                                   |
| Service Status    | Radio                 | Yes      | Active / Inactive                                   |

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

_(Integrated with Module 10 Product Master)_

| Field           | Type   | Required | Validation                       |
| --------------- | ------ | -------- | -------------------------------- |
| Product Name    | Lookup | Yes      | Must exist in Product Master     |
| Product Code    | Auto   | Yes      | Auto fetched                     |
| UOM             | Auto   | Yes      | From product master              |
| Standard Usage  | Auto   | Yes      | Based on product configuration   |
| Required Qty    | Number | Yes      | Must be ≥ 0                      |
| Custom Chemical | Text   | No       | Allowed if product not available |

**Business Rule**

- When a **product is selected**, the following fields auto populate
  - Product Code
  - UOM
  - Standard Usage

- Required Qty must follow **UOM measurement**.

Example

| Product    | UOM | Required Qty |
| ---------- | --- | ------------ |
| Chemical A | ml  | 50 ml        |
| Powder B   | gm  | 100 gm       |

---

### **Section 5: Pricing Configuration**

| Field                  | Type   | Required    | Validation                                |
| ---------------------- | ------ | ----------- | ----------------------------------------- |
| Price Type             | Radio  | Yes         | Fixed / Area Based / Inspection Based     |
| Residential Pricing    | Number | Conditional | Required if Residential category selected |
| Commercial Pricing     | Number | Conditional | Required if Commercial category selected  |
| Custom Property Type   | Text   | No          | Allowed via Add Custom                    |
| Custom Commercial Type | Text   | No          | Allowed via Add Custom                    |

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
┌──────────────────────────────────────────────────────────────────────────────┐
│ [← Back to Services]                 EDIT SERVICE                     [Save] │
├──────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│ BASIC SERVICE INFORMATION                                                    │
│                                                                              │
│ Service Name:        [Pre-filled]                                            │
│ Service Category:    [Residential] [Commercial]                              │
│ Service Code:        SRV-0001 (Read Only)                                    │
│                                                                              │
│ Pest Types Covered:  Rodent, Cockroach                                       │
│ Description:         [Pre-filled description]                                │
│ Service Duration:    [60] Minutes                                            │
│                                                                              │
│ Service Status: (•) Active  ( ) Inactive                                     │
│                                                                              │
│ If Inactive Selected →                                                        │
│ Inactive Reason:                                                             │
│ [________________________________________________________]                   │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘


Remaining sections remain same as **Add Service**

* Pest Species
* Treatment Methods
* Chemicals Used
* Pricing
* Warranty
```

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
│ │ Product Name │ Product Code │ UOM │ Standard Usage │ Required Qty             │
│ │ Alpha Cypermethrin │ P-001 │ ml │ 10 ml │ 20 ml                                │
│ │ Chlorpyriphos │ P-002 │ ml │ 20 ml │ 30 ml                                     │
│ └───────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                     │
│ PRICING CONFIGURATION                                                              │
│ ┌───────────────────────────────────────────────────────────────────────────────┐
│ │ Price Type: Fixed Price                                                       │
│ │ Residential Pricing                                                           │
│ │ 1BHK: ₹1200   2BHK: ₹1500   3BHK: ₹1800   4BHK+: ₹2200                         │
│ │ Commercial Pricing                                                            │
│ │ Small Office: ₹2500                                                           │
│ │ Medium Office: ₹3500                                                          │
│ │ Large Office: ₹5000                                                           │
│ │ Warehouse: ₹7000                                                              │
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

| Field             | Description                                       |
| ----------------- | ------------------------------------------------- |
| Service Name      | Name of the service                               |
| Service Code      | Unique auto-generated code                        |
| Service Category  | Residential / Commercial / Industrial / Warehouse |
| Pest Type Covered | Types of pests handled by the service             |
| Description       | Service description                               |
| Service Duration  | Estimated duration                                |
| Service Status    | Active / Inactive                                 |

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

| Field             | Description                   |
| ----------------- | ----------------------------- |
| Product Name      | Chemical/product name         |
| Product Code      | Product master code           |
| UOM               | Unit of measurement           |
| Standard Usage    | Default usage quantity        |
| Required Quantity | Quantity required per service |

---

# **Section 5: Pricing Configuration Fields**

| Field               | Description                           |
| ------------------- | ------------------------------------- |
| Price Type          | Fixed / Area Based / Inspection Based |
| Residential Pricing | Pricing based on property type        |
| Commercial Pricing  | Pricing for offices and warehouses    |

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

# 13.1 Vendor Dashboard – Table View

## Description

The **Vendor Dashboard** displays a complete list of vendors registered in the system.

It provides:

- Vendor overview
- Quick vendor search
- Advanced filtering
- Vendor actions
- Contract visibility

Administrators and procurement teams can quickly view and manage vendor records.

---

# Screen Layout

```
┌───────────────────────────────────────────────────────────────────────────────────────────┐
│                                   VENDOR MANAGEMENT                                        │
├───────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                           │
│  FILTER PANEL (Final list mention below)                                                  │
│                                                                                           │
│  Vendor Type        : [☑ Supplier] [☑ Service Provider] [☑ Both]                          │
│  Vendor Category    : [All Categories ▼]                                                   │
│  Contract Type      : [☑ Annual] [☑ Project] [☑ One Time]                                 │
│  Vendor Status      : [☑ Active] [☑ Inactive]                                             │
│  Vendor Rating      : [⭐1+] [⭐⭐2+] [⭐⭐⭐3+] [⭐⭐⭐⭐4+] [⭐⭐⭐⭐⭐5]                              │
│                                                                                           │
│  Search : [_________________________________________________________]                     │
│           (Vendor ID / Vendor Name / Contact / Phone / Product / GST)                    │
│                                                                                           │
│  [Reset Filters]                                              [+ Add Vendor]              │
│                                                                                           │
├───────────────────────────────────────────────────────────────────────────────────────────┤
│                                   VENDOR TABLE                                            │
├────────┬──────────────────┬────────────┬────────────┬──────────────┬─────────────┬───────┤
│ Vdr ID │ Vendor Name      │ Type       │ Category   │ Contact      │ Phone       │ City  │
├────────┼──────────────────┼────────────┼────────────┼──────────────┼─────────────┼───────┤
│ V-001  │ ABC Chemicals    │ Supplier   │ Chemical   │ Rajesh Kumar │ 9876543210  │ Pune  │
│ V-002  │ PestPro Services │ Service    │ Fumigation │ Priya Sharma │ 8765432109  │ BLR   │
│ V-003  │ Global Equip     │ Both       │ Equipment  │ Amit Singh   │ 7654321987  │ Delhi │
└────────┴──────────────────┴────────────┴────────────┴──────────────┴─────────────┴───────┘

┌──────────┬─────────────┬─────────────┬──────────┬─────────┬──────────────────────────────┐
│ Contract │ Billing     │ Payment     │ Status   │ Rating  │ Actions                      │
├──────────┼─────────────┼─────────────┼──────────┼─────────┼──────────────────────────────┤
│ Annual   │ Per Service │ Net 30      │ Active   │ ⭐⭐⭐⭐    │ View | Edit |          │
│ Project  │ Monthly     │ Advance     │ Active   │ ⭐⭐⭐     │ View | Edit |          │
│ One Time │ Project     │ Net 15      │ Blocked  │ ⭐⭐      │ View | Edit |          │
└──────────┴─────────────┴─────────────┴──────────┴─────────┴──────────────────────────────┘

```

---

# Table Fields

| Field                    | Type         | Required | Description                                |
| ------------------------ | ------------ | -------- | ------------------------------------------ |
| Vendor ID                | Text         | Auto     | Unique vendor identifier (V-001, V-002...) |
| Vendor Name              | Text         | Yes      | Company or vendor name                     |
| Vendor Type              | Badge        | Yes      | Supplier / Service Provider / Both         |
| Vendor Category          | Text         | Yes      | Chemical / Equipment / Service / Other     |
| Service/Product Provided | Text         | Yes      | Main service or product                    |
| Contact Person           | Text         | Yes      | Primary contact name                       |
| Phone Number             | Link         | Yes      | Click-to-call phone number                 |
| Email ID                 | Link         | Yes      | Vendor email                               |
| City                     | Text         | Yes      | Vendor location                            |
| State                    | Text         | Yes      | Vendor state                               |
| Contract Type            | Badge        | Yes      | Annual / Project / One Time                |
| Contract Start Date      | Date         | Yes      | Contract start date                        |
| Contract End Date        | Date         | No       | Contract expiry date                       |
| Billing Type             | Text         | Yes      | Per Service / Monthly / Project            |
| Payment Terms            | Text         | Yes      | Advance / Net 15 / Net 30 / Net 45         |
| Vendor Status            | Badge        | Yes      | Active / Inactive / Blocked                |
| Vendor Rating            | Rating       | No       | Vendor performance rating                  |
| GST Number               | Text         | No       | Vendor GST identification                  |
| Created Date             | Date         | Auto     | Vendor record creation date                |
| Created By               | Text         | Auto     | User who created vendor                    |
| Actions                  | Button Group | —        | View / Edit                                |

---

# Search

| Field         | Type       | Scope                                                              |
| ------------- | ---------- | ------------------------------------------------------------------ |
| Global Search | Text Input | Vendor ID, Vendor Name, Contact Person, Phone, Email, GST, Product |

---

# Filters

| Filter Name       | Type          | Options                            |
| ----------------- | ------------- | ---------------------------------- |
| Vendor Type       | Multi Select  | Supplier / Service Provider / Both |
| Vendor Category   | Dropdown      | Chemical / Equipment / Service     |
| Contract Type     | Multi Select  | Annual / Project / One Time        |
| Vendor Status     | Multi Select  | Active / Inactive                  |
| Vendor Rating     | Rating Filter | ⭐1+ to ⭐5                        |
| State             | Multi Select  | State list                         |
| City              | Text Search   | City name                          |
| Contract End Date | Date Range    | From – To                          |

---

# Actions

| Action | Description           |
| ------ | --------------------- |
| View   | Open vendor profile   |
| Edit   | Modify vendor details |

---

# System Behavior

- Vendor ID is automatically generated.
- Vendors linked to purchase orders or contracts cannot be deleted.
- Blocked vendors cannot be used in new transactions.
- Vendor ratings are updated from vendor performance feedback.

---

# Audit Trail

The system logs the following events:

- Vendor created
- Vendor updated
- Vendor status changed
- Vendor deleted

Each log includes:

- User
- Timestamp
- Action performed

---

# Permissions

| Role                | Permission              |
| ------------------- | ----------------------- |
| Admin               | Full access             |
| Procurement Manager | Create / Edit / View    |
| Accounts Team       | View only               |
| Operations Team     | View vendor information |

# 13.2 Add Vendor – Vendor Registration Screen

## Description

The **Add Vendor Screen** allows administrators or procurement managers to register a new vendor in the system.

This screen captures complete vendor information including:

- Vendor basic information
- Address details
- Tax and compliance information
- Bank details
- Contract details
- Billing configuration
- Payment terms
- Vendor performance details

Vendor records created here are used across:

- Procurement
- Purchase Orders
- Vendor Payments
- Contract Management

---

# Screen Layout

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                                ADD NEW VENDOR                                │
└──────────────────────────────────────────────────────────────────────────────┘


BASIC INFORMATION
──────────────────────────────────────────────────────────────────────────────

Vendor ID* (Auto Generated)            Vendor Status*       [ Active ▼ ]

Vendor Name*                           [________________________________]

Vendor Type*                           [ Supplier ▼ ]

Vendor Category*                       [ Chemical Supplier ▼ ]

Service / Product Provided*            [________________________________]

Contact Person*                        [________________________________]

Phone Number*                          [________________________________]

Email ID*                              [________________________________]


ADDRESS DETAILS
──────────────────────────────────────────────────────────────────────────────

Address*                               [________________________________]
                                       [________________________________]

City*               [____________]     State*             [ Telangana ▼ ]

Pincode*            [____________]     Country*           [ India ▼ ]


TAX & COMPLIANCE
──────────────────────────────────────────────────────────────────────────────

GST Number                             [________________________________]

PAN Number                             [________________________________]

Vendor Registration Type               [ Registered ▼ ]


BANK DETAILS
──────────────────────────────────────────────────────────────────────────────

Bank Name                              [________________________________]

Account Holder Name                    [________________________________]

Account Number                         [________________________________]

IFSC Code                              [________________________________]


CONTRACT DETAILS
──────────────────────────────────────────────────────────────────────────────

Contract Type*                         [ Annual ▼ ]

Contract Start Date*                   [ 📅 Select Date ]

Contract End Date                      [ 📅 Select Date ]

SLA Agreement                          [ Yes ▼ ]

Contract Document Upload               [ Upload File ]


BILLING TERMS
──────────────────────────────────────────────────────────────────────────────

Billing Type*                          [ Monthly ▼ ]

Billing Cycle                          [ Monthly ▼ ]

Custom Billing Start Date              [ 📅 Select Date ]

Custom Billing End Date                [ 📅 Select Date ]

Tax Applicable*                        [ Yes ▼ ]

Invoice Submission Method              [ Email ▼ ]


PAYMENT DETAILS
──────────────────────────────────────────────────────────────────────────────

Payment Terms*                         [ Net 30 ▼ ]

Credit Days                            [______]

Payment Method*                        [ Bank Transfer ▼ ]

Currency*                              [ INR ▼ ]


PERFORMANCE & NOTES
──────────────────────────────────────────────────────────────────────────────

Vendor Rating                          [ 1 – 5 ]

Remarks / Notes                        [________________________________]
                                       [________________________________]


SYSTEM INFORMATION
──────────────────────────────────────────────────────────────────────────────

Created Date (Auto)

Updated Date (Auto)

Created By (Auto)


┌────────────────────────────────────────────────────────────┐
│ [ Cancel ]                                     [ Save Vendor ] │
└────────────────────────────────────────────────────────────┘
```

---

# Section Wise Field Specifications

---

# 1. Basic Information

| Field                      | Type        | Required | Options                                                                                           | Validation          |
| -------------------------- | ----------- | -------- | ------------------------------------------------------------------------------------------------- | ------------------- |
| Vendor ID                  | Text (Auto) | Auto     | System Generated                                                                                  | Unique              |
| Vendor Status              | Dropdown    | Yes      | Active, Inactive                                                                                  | Default Active      |
| Vendor Name                | Text        | Yes      | —                                                                                                 | Min 3 characters    |
| Vendor Type                | Dropdown    | Yes      | Supplier, Service Provider, Both                                                                  | Required            |
| Vendor Category            | Dropdown    | Yes      | Chemical Supplier, Equipment Vendor, Service Partner, Logistics Vendor, Maintenance Vendor, Other | Required            |
| Service / Product Provided | Text        | Yes      | —                                                                                                 | Max 150 characters  |
| Contact Person             | Text        | Yes      | —                                                                                                 | Alphabets only      |
| Phone Number               | Phone       | Yes      | —                                                                                                 | 10 digit validation |
| Email ID                   | Email       | Yes      | —                                                                                                 | Valid email format  |

---

# 2. Address Details

| Field   | Type     | Required | Options                | Validation         |
| ------- | -------- | -------- | ---------------------- | ------------------ |
| Address | Textarea | Yes      | —                      | Max 250 characters |
| City    | Text     | Yes      | —                      | Alphabets only     |
| State   | Dropdown | Yes      | State List             | Required           |
| Pincode | Number   | Yes      | —                      | 6 digit validation |
| Country | Dropdown | Yes      | India (default), Other | Required           |

---

# 3. Tax & Compliance

| Field                    | Type     | Required    | Options                  | Validation            |
| ------------------------ | -------- | ----------- | ------------------------ | --------------------- |
| GST Number               | Text     | Conditional | —                        | GST Format Validation |
| PAN Number               | Text     | Optional    | —                        | PAN Format Validation |
| Vendor Registration Type | Dropdown | Yes         | Registered, Unregistered | Required              |

Validation Rule:

If **Vendor Registration Type = Registered → GST Required**

---

# 4. Bank Details

| Field               | Type   | Required | Validation             |
| ------------------- | ------ | -------- | ---------------------- |
| Bank Name           | Text   | Optional | Alphabets only         |
| Account Holder Name | Text   | Optional | Alphabets              |
| Account Number      | Number | Optional | 9–18 digits            |
| IFSC Code           | Text   | Optional | IFSC format validation |

---

# 5. Contract Details

| Field                    | Type        | Required    | Options                   | Validation            |
| ------------------------ | ----------- | ----------- | ------------------------- | --------------------- |
| Contract Type            | Dropdown    | Yes         | Annual, Project, One Time | Required              |
| Contract Start Date      | Date Picker | Yes         | —                         | Cannot be future date |
| Contract End Date        | Date Picker | Conditional | —                         | Must be > Start Date  |
| SLA Agreement            | Dropdown    | No          | Yes, No                   | Default No            |
| Contract Document Upload | File Upload | Optional    | PDF, DOCX                 | Max 10MB              |

---

# 6. Billing Terms

| Field                     | Type     | Required    | Options                            | Validation                         |
| ------------------------- | -------- | ----------- | ---------------------------------- | ---------------------------------- |
| Billing Type              | Dropdown | Yes         | Per Service, Monthly, Project      | Required                           |
| Billing Cycle             | Dropdown | Optional    | Weekly, Monthly, Quarterly, Custom | Required for recurring billing     |
| Custom Billing Start Date | Date     | Conditional | —                                  | Required if Billing Cycle = Custom |
| Custom Billing End Date   | Date     | Conditional | —                                  | Must be after start date           |
| Tax Applicable            | Dropdown | Yes         | Yes, No                            | Required                           |
| Invoice Submission Method | Dropdown | Optional    | Email, Portal, Physical            | Default Email                      |

---

# 7. Payment Details

| Field          | Type     | Required | Options                                 | Validation   |
| -------------- | -------- | -------- | --------------------------------------- | ------------ |
| Payment Terms  | Dropdown | Yes      | Advance, Net 15, Net 30, Net 45, Net 60 | Required     |
| Credit Days    | Number   | Optional | —                                       | Numeric only |
| Payment Method | Dropdown | Yes      | Bank Transfer, UPI, Cheque, Cash        | Required     |

---

# 8. Performance & Notes

| Field           | Type     | Required | Validation         |
| --------------- | -------- | -------- | ------------------ |
| Vendor Rating   | Number   | Optional | Range 1–5          |
| Remarks / Notes | Textarea | Optional | Max 300 characters |

---

# 9. System Information

| Field        | Type           | Description                      |
| ------------ | -------------- | -------------------------------- |
| Created Date | DateTime       | Timestamp when record is created |
| Updated Date | DateTime       | Last updated timestamp           |
| Created By   | User Reference | System user creating vendor      |

---

# Form Validations

| Validation Rule      | Description                          |
| -------------------- | ------------------------------------ |
| Mandatory Fields     | Fields marked with \* must be filled |
| Email Validation     | Must follow standard email format    |
| Phone Number         | Must contain exactly 10 digits       |
| GST Validation       | Must follow GSTIN format             |
| PAN Validation       | Must follow PAN format               |
| Contract End Date    | Must be after Contract Start Date    |
| Billing Custom Dates | Required if Billing Cycle = Custom   |

---

# Form Actions

| Action      | Description                        |
| ----------- | ---------------------------------- |
| Cancel      | Discards vendor creation           |
| Save Vendor | Validates and stores vendor record |

---

# System Behavior

- Vendor ID is automatically generated.
- Vendor cannot be saved without mandatory fields.
- Vendor becomes available for **Procurement & Purchase Orders after creation**.
- Uploaded contract documents are stored in **Vendor Document Repository**.

# 13.3 Edit Vendor – Vendor Update Screen

## Description

The **Edit Vendor Screen** allows authorized users to modify existing vendor details.

This screen is used to:

- Update vendor contact information
- Modify billing and payment configurations
- Update contract details
- Maintain vendor compliance information
- Track historical changes through **Audit Logs**

All updates made through this screen are recorded in the **Audit Log Tracking system** to ensure transparency and traceability.

---

# Screen Layout

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                               EDIT VENDOR DETAILS                             │
│                         Vendor ID : V-001  |  Status : Active                 │
└──────────────────────────────────────────────────────────────────────────────┘


BASIC INFORMATION
──────────────────────────────────────────────────────────────────────────────

Vendor ID (Read Only)                 V-001

Vendor Status*                        [ Active ▼ ]

Vendor Name*                          [ ABC Chemicals Pvt Ltd ]

Vendor Type*                          [ Supplier ▼ ]

Vendor Category*                      [ Chemical Supplier ▼ ]

Service / Product Provided*           [ Industrial Chemicals ]

Contact Person*                       [ Rajesh Kumar ]

Phone Number*                         [ 9876543210 ]

Email ID*                             [ rajesh@abcchem.com ]


ADDRESS DETAILS
──────────────────────────────────────────────────────────────────────────────

Address*                              [ Plot 21 Industrial Area ]
                                      [ Phase 2 ]

City*               [ Hyderabad ]     State*          [ Telangana ▼ ]

Pincode*            [ 500081 ]        Country*        [ India ▼ ]


TAX & COMPLIANCE
──────────────────────────────────────────────────────────────────────────────

GST Number                            [ 36ABCDE1234F1Z5 ]

PAN Number                            [ ABCDE1234F ]

Vendor Registration Type              [ Registered ▼ ]


BANK DETAILS
──────────────────────────────────────────────────────────────────────────────

Bank Name                             [ HDFC Bank ]

Account Holder Name                   [ ABC Chemicals Pvt Ltd ]

Account Number                        [ 234567890123 ]

IFSC Code                             [ HDFC0001234 ]


CONTRACT DETAILS
──────────────────────────────────────────────────────────────────────────────

Contract Type*                        [ Annual ▼ ]

Contract Start Date*                  [ 📅 01-Jan-2026 ]

Contract End Date                     [ 📅 31-Dec-2026 ]

SLA Agreement                         [ Yes ▼ ]

Contract Document Upload              [ Replace File ]


BILLING TERMS
──────────────────────────────────────────────────────────────────────────────

Billing Type*                         [ Monthly ▼ ]

Billing Cycle                         [ Monthly ▼ ]

Custom Billing Start Date             [ 📅 Select Date ]

Custom Billing End Date               [ 📅 Select Date ]

Tax Applicable*                       [ Yes ▼ ]

Invoice Submission Method             [ Email ▼ ]


PAYMENT DETAILS
──────────────────────────────────────────────────────────────────────────────

Payment Terms*                        [ Net 30 ▼ ]

Credit Days                           [ 30 ]

Payment Method*                       [ Bank Transfer ▼ ]

Currency                              [ INR ▼ ]


PERFORMANCE & NOTES
──────────────────────────────────────────────────────────────────────────────

Vendor Rating                         [ 4 ]

Remarks / Notes                       [ Long term supplier with good delivery ]
                                      [ Maintain monthly supply agreement ]


SYSTEM INFORMATION
──────────────────────────────────────────────────────────────────────────────

Created Date                          02-Feb-2026

Updated Date                          10-Feb-2026

Created By                            Admin


┌────────────────────────────────────────────────────────────┐
│ [ Cancel ]                                   [ Update Vendor ] │
└────────────────────────────────────────────────────────────┘
```

---

- keep form same as Add but refer below instrcutions.

# Editable Field Specifications

| Field            | Editable | Notes                              |
| ---------------- | -------- | ---------------------------------- |
| Vendor ID        | ❌ No    | System generated                   |
| Vendor Status    | ✅ Yes   | Active / Inactive(Inactive reason) |
| Vendor Name      | ✅ Yes   | Editable                           |
| Vendor Type      | ✅ Yes   | Supplier / Service Provider / Both |
| Vendor Category  | ✅ Yes   | Vendor classification              |
| Contact Person   | ✅ Yes   | Vendor representative              |
| Phone Number     | ✅ Yes   | Must remain unique                 |
| Email ID         | ✅ Yes   | Valid email required               |
| Address          | ✅ Yes   | Full vendor location               |
| GST Number       | ✅ Yes   | Editable for compliance            |
| Bank Details     | ✅ Yes   | Can be updated                     |
| Contract Details | ✅ Yes   | Contract renewal or update         |
| Billing Terms    | ✅ Yes   | Billing configuration              |
| Payment Terms    | ✅ Yes   | Financial agreement                |
| Vendor Rating    | ✅ Yes   | Updated based on performance       |
| Created Date     | ❌ No    | System generated                   |
| Updated Date     | ❌ No    | Auto updated                       |

---

# Validation Rules

| Validation           | Description                           |
| -------------------- | ------------------------------------- |
| Mandatory Fields     | Fields marked with \* cannot be empty |
| Phone Number         | Must contain exactly 10 digits        |
| Email Format         | Must be valid email format            |
| Contract End Date    | Must be greater than start date       |
| GST Format           | Must match GSTIN pattern              |
| PAN Format           | Must match PAN validation format      |
| Billing Custom Dates | Required if billing cycle = Custom    |

---

# **Audit Log Tracking**

Every modification to the vendor must be recorded.

### **Audit Fields**

| Field             | Description                         |
| ----------------- | ----------------------------------- |
| Created By        | User who created vendor             |
| Created Date      | Creation timestamp                  |
| Last Updated By   | Last editor of vendor record        |
| Last Updated Date | Last modification time              |
| Change Type       | Create / Update / Deactivate        |
| Change Notes      | Optional admin comment about change |

---

### **Audit Log Example**

| Date        | User  | Action      | Field Changed                     |
| ----------- | ----- | ----------- | --------------------------------- |
| 02-Feb-2026 | Admin | Created     | New Vendor Added                  |
| 10-Feb-2026 | Admin | Updated     | Contact Person Updated            |
| 12-Feb-2026 | Admin | Updated     | Payment Terms changed Net15→Net30 |
| 15-Feb-2026 | Admin | Deactivated | Vendor marked inactive            |

---

# Form Actions

| Action        | Description                                      |
| ------------- | ------------------------------------------------ |
| Cancel        | Discards changes and returns to Vendor Dashboard |
| Update Vendor | Saves the updated vendor details                 |

---

# System Behavior

- Vendor ID remains **non-editable**.
- Changes to **contract or billing fields** may affect procurement workflows.
- Any modification automatically creates a **new audit log entry**.
- Vendor cannot be set **Inactive if active purchase orders exist**.

---

# 13.4 View Vendor Details – Vendor Profile Screen

## Description

The **View Vendor Details Screen** provides a complete profile of a vendor in **read-only mode**.  
It allows users to view all information related to the vendor without modifying data.

This screen acts as the **central vendor profile page**, displaying:

- Vendor basic details
- Address and compliance information
- Contract details
- Billing and payment configuration
- Vendor performance rating
- Vendor documents
- Audit log history

This screen is typically accessed from:

- **Vendor Dashboard**
- **Procurement Module**
- **Purchase Orders**
- **Vendor Payments**

---

# Screen Layout

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                              VENDOR PROFILE                                  │
│                     Vendor ID : V-001  |  Status : Active                    │
└──────────────────────────────────────────────────────────────────────────────┘


VENDOR SUMMARY
──────────────────────────────────────────────────────────────────────────────

Vendor Name            : ABC Chemicals Pvt Ltd
Vendor Type            : Supplier
Vendor Category        : Chemical Supplier
Service/Product        : Industrial Chemicals
Vendor Rating          : ⭐⭐⭐⭐


CONTACT INFORMATION
──────────────────────────────────────────────────────────────────────────────

Contact Person         : Rajesh Kumar
Phone Number           : +91 9876543210
Email ID               : rajesh@abcchem.com


ADDRESS DETAILS
──────────────────────────────────────────────────────────────────────────────

Address                : Plot 21 Industrial Area
                         Phase 2

City                   : Hyderabad
State                  : Telangana
Pincode                : 500081
Country                : India


TAX & COMPLIANCE
──────────────────────────────────────────────────────────────────────────────

Vendor Registration    : Registered
GST Number             : 36ABCDE1234F1Z5
PAN Number             : ABCDE1234F


BANK DETAILS
──────────────────────────────────────────────────────────────────────────────

Bank Name              : HDFC Bank
Account Holder Name    : ABC Chemicals Pvt Ltd
Account Number         : ********90123
IFSC Code              : HDFC0001234


CONTRACT DETAILS
──────────────────────────────────────────────────────────────────────────────

Contract Type          : Annual
Contract Start Date    : 01-Jan-2026
Contract End Date      : 31-Dec-2026
SLA Agreement          : Yes
Contract Document      : View / Download


BILLING CONFIGURATION
──────────────────────────────────────────────────────────────────────────────

Billing Type           : Monthly
Billing Cycle          : Monthly
Tax Applicable         : Yes
Invoice Submission     : Email


PAYMENT DETAILS
──────────────────────────────────────────────────────────────────────────────

Payment Terms          : Net 30
Credit Days            : 30
Payment Method         : Bank Transfer
Currency               : INR


PERFORMANCE & NOTES
──────────────────────────────────────────────────────────────────────────────

Vendor Rating          : 4 / 5
Remarks / Notes        : Long term supplier with reliable delivery performance.


SYSTEM INFORMATION
──────────────────────────────────────────────────────────────────────────────

Created Date           : 02-Feb-2026
Updated Date           : 10-Feb-2026
Created By             : Admin


┌────────────────────────────────────────────────────────────┐
│ [ Back ]                    [ Edit Vendor ]   [ Download ]  │
└────────────────────────────────────────────────────────────┘
```

---

# Section Wise Data Display

---

# 1. Vendor Summary

| Field           | Description                        |
| --------------- | ---------------------------------- |
| Vendor ID       | Unique vendor identifier           |
| Vendor Name     | Official vendor company name       |
| Vendor Type     | Supplier / Service Provider / Both |
| Vendor Category | Vendor classification              |
| Service/Product | Main service or product supplied   |
| Vendor Rating   | Performance rating                 |

---

# 2. Contact Information

| Field          | Description            |
| -------------- | ---------------------- |
| Contact Person | Primary vendor contact |
| Phone Number   | Vendor contact phone   |
| Email ID       | Vendor email address   |

---

# 3. Address Details

| Field   | Description             |
| ------- | ----------------------- |
| Address | Vendor business address |
| City    | Vendor city             |
| State   | Vendor state            |
| Pincode | Postal code             |
| Country | Vendor country          |

---

# 4. Tax & Compliance

| Field                    | Description               |
| ------------------------ | ------------------------- |
| Vendor Registration Type | Registered / Unregistered |
| GST Number               | Vendor GST identification |
| PAN Number               | Vendor PAN identification |

---

# 5. Bank Details

| Field               | Description                |
| ------------------- | -------------------------- |
| Bank Name           | Vendor bank                |
| Account Holder Name | Account holder name        |
| Account Number      | Masked bank account number |
| IFSC Code           | Bank branch code           |

---

# 6. Contract Details

| Field               | Description                 |
| ------------------- | --------------------------- |
| Contract Type       | Annual / Project / One Time |
| Contract Start Date | Contract start date         |
| Contract End Date   | Contract expiry date        |
| SLA Agreement       | Indicates SLA availability  |
| Contract Document   | Download contract file      |

---

# 7. Billing Configuration

| Field                     | Description                     |
| ------------------------- | ------------------------------- |
| Billing Type              | Per Service / Monthly / Project |
| Billing Cycle             | Weekly / Monthly / Quarterly    |
| Tax Applicable            | Indicates tax applicability     |
| Invoice Submission Method | Email / Portal / Physical       |

---

# 8. Payment Details

| Field          | Description                        |
| -------------- | ---------------------------------- |
| Payment Terms  | Advance / Net 15 / Net 30 / Net 45 |
| Credit Days    | Allowed payment days               |
| Payment Method | Bank Transfer / UPI / Cheque       |
| Currency       | Payment currency                   |

---

# 9. Performance & Notes

| Field           | Description                  |
| --------------- | ---------------------------- |
| Vendor Rating   | Vendor performance score     |
| Remarks / Notes | Additional internal comments |

---

# **Audit Log Tracking**

Every modification to the vendor record is logged for audit and compliance purposes.

### **Audit Fields**

| Field             | Description                  |
| ----------------- | ---------------------------- |
| Created By        | User who created vendor      |
| Created Date      | Creation timestamp           |
| Last Updated By   | Last editor                  |
| Last Updated Date | Last modification time       |
| Change Type       | Create / Update / Deactivate |
| Change Notes      | Optional admin comment       |

---

### **Audit Log Example**

| Date        | User  | Action      | Field Changed                     |
| ----------- | ----- | ----------- | --------------------------------- |
| 02-Feb-2026 | Admin | Created     | New Vendor Added                  |
| 10-Feb-2026 | Admin | Updated     | Contact Person Updated            |
| 12-Feb-2026 | Admin | Updated     | Payment Terms changed Net15→Net30 |
| 15-Feb-2026 | Admin | Deactivated | Vendor marked inactive            |

---

# System Behavior

- All fields on this screen are **read-only**.
- Sensitive fields such as **bank account numbers are partially masked**.
- Vendor documents are available for **download only**.
- Audit logs help administrators **track historical changes to vendor records**.
- Vendor profile data is shared across **Procurement, Finance, and Contract Management modules**.

---
