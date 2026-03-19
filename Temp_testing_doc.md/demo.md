# 🎯 MODULE 10: INVENTORY & SERVICES

## Overview

Manages all items and services offered by the business. Helps organize products, track stock levels, and maintain records of inventory movements. Ensures accurate stock availability and supports smooth operations. **Requires Module 9 (Tax Management) to be configured first.**

**Module Connections:**

- Depends on: Module 9 (Tax Types and HSN Codes)
- Uses: Module 9.6-9.8 (HSN Codes for tax auto-population)
- Sub-modules: Product Catalog, My Services, Stock Management, Stock Log

---

# 10.1 Product Management – Table View

```
┌──────────────────────────────────────────────────────────────────────────────────────────────┐
│                                   PRODUCT MANAGEMENT                                          │
│                                                                                               │
│  [+ ADD PRODUCT] (Head Ops Only)                                                               │
│                                                                                               │
│  ┌─────────────────────────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                                 │  │
│  │                                                                                        │  │
│  │ Category: [☑ Multi-select ▼]         Sub-Type: [▼ Cascading Dropdown ▼]                │  │
│  │ Company / Brand: [🔍 Search Dropdown ▼]                                                 │  │
│  │ HSN Code: [__________]                 Status: [☑ Active] [☑ Inactive]                 │  │
│  │ Created Date: [📅 From] - [📅 To]                                                       │  │
│  │                                                                                        │  │
│  │ Search: [__________________________] (Product Code / Name / Brand / HSN Code)          │  │
│  │                                                                                        │  │
│  │ [Reset Filters]                                                                        │  │
│  └─────────────────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                               │
│  PRODUCTS TABLE                                                                               │
│  ┌─────────────────────────────────────────────────────────────────────────────────────────┐  │
│  │Img│Product Code│Product Name│Category│Company / Brand│HSN Code│Base│Package│Default ₹│Status│
│  │   │            │            │        │                │        │UOM │Type   │Purchase │      │
│  │───┼────────────┼────────────┼────────┼────────────────┼────────┼────┼───────┼─────────┼──────│
│  │📷 │PRD001      │Chem X      │Chemical│ABC Co          │1234    │Ltr │Bottle │500      │Active│
│  │📷 │PRD002      │Sprayer A   │Sprayer │AgroTech        │8424    │Nos │Box    │1200     │Active│
│  │📷 │PRD003      │Electric P1 │Machine │PowerTools      │8501    │Nos │Set    │3500     │Inactive│
│  └─────────────────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                               │
│ ┌───────────────────────────────────────────────────────────────────────────────────────────┐ │
│ │Created Date│Created By│Actions                                                             │ │
│ │────────────┼──────────┼──────────────────────────────────────────────────────────────────│ │
│ │02-Jan-2026 │Admin     │[👁 View] [✏ Edit] [🗑 Delete]                                       │ │
│ │02-Jan-2026 │Admin     │[👁 View] [✏ Edit] [🗑 Delete]                                       │ │
│ │03-Jan-2026 │Admin     │[👁 View] [✏ Edit] [🗑 Delete]                                       │ │
│ └───────────────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                               │
│ Pagination:  Previous   1   2   3   ...   10   Next                                           │
│                                                                                               │
└──────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

# Table View Fields

| Field                      | Type     | Required | Description                                                        |
| -------------------------- | -------- | -------- | ------------------------------------------------------------------ |
| Product Code               | Text     | Auto     | Auto-generated unique SKU                                          |
| Cover Image                | Image    | Optional | Product thumbnail                                                  |
| Product Name               | Text     | Yes      | Product full name                                                  |
| Category                   | Dropdown | Yes      | Chemical / Sprayer / Electric Pump / Machine / Trap / Tool / Other |
| Company / Brand            | Dropdown | Yes      | Manufacturer or brand                                              |
| HSN Code                   | Link     | Yes      | Clickable tax code                                                 |
| Base UOM                   | Dropdown | Yes      | Nos / Ltr / Kg / Gram / ml / Set / Pkt                             |
| Package Type               | Dropdown | Yes      | Bottle / Packet / Box / Bag / Pouch / Can / Set                    |
| Default Purchase Price (₹) | Currency | Yes      | Reference purchase price                                           |
| Status                     | Badge    | Yes      | Active / Inactive                                                  |
| Created Date               | Date     | Auto     | Record creation date                                               |
| Created By                 | Text     | Auto     | User who created product                                           |

---

# Actions

| Action | Role Access   | Behavior                        |
| ------ | ------------- | ------------------------------- |
| View   | All Roles     | Opens product detail drawer     |
| Edit   | Head Ops Only | Opens Edit Product form         |
| Delete | Head Ops Only | Soft delete (Status → Inactive) |

---

# Filters

| Filter          | Type               | Description                                                        |
| --------------- | ------------------ | ------------------------------------------------------------------ |
| Category        | Multi-select       | Chemical / Sprayer / Electric Pump / Machine / Trap / Tool / Other |
| package-Type    | Cascading Dropdown | Changes based on Category                                          |
| Status          | Multi-select       | Active / Inactive                                                  |
| Created Date    | Date Range         | Filter by creation date                                            |

---

# Search

Global search supports:

| Search Field    | Type    |
| --------------- | ------- |
| Product Code    | Text    |
| Product Name    | Text    |
| HSN Code        | Numeric |
| Company / Brand | Text    |

---

# 10.2 Add Product

```
┌────────────────────────────────────────────────────────────────┐
│                         ADD PRODUCT                             │
│                                                                │
│  Description: Create a new product in the Product Master with  │
│  basic details, packaging, variants, and tax configuration.   │
│  Stock will be managed separately in the Add Stock module.    │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│ SECTION 1: BASIC PRODUCT INFORMATION                           │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ Product Name*           : [________________________]      │ │
│ │ Product Code            : [________________________]      │ │
│ │ (Auto-generate or manual entry)                          │ │
│ │                                                          │ │
│ │ Category*               : [▼ Select Category ▼]          │ │
│ │   Chemical / Sprayer / Electric Pump / Machine           │ │
│ │   Trap / Tool / Other                                    │ │
│ │                                                          │ │
│ │ Sub-Type               : [▼ Cascading Dropdown ▼]        │ │
│ │ (Based on selected category)                             │ │
│ │                                                          │ │
│ │ Company / Brand*        : [🔍 Search Dropdown ▼]         │ │
│ │ [+ Add New Brand]                                       │ │
│ │                                                          │ │
│ │ Description             : [___________________________]  │ │
│ │                           [ Text Area ]                 │ │
│ │                                                          │ │
│ │ Status                  : [☑ Active] Toggle             │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│ SECTION 2: PRODUCT MEDIA                                       │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ Product Images         : [📎 Upload Images]               │ │
│ │ (Maximum 5 images allowed)                                │ │
│ │ Supported Format       : JPG / PNG                        │ │
│ │ Max Size               : 2MB per image                    │ │
│ │                                                          │ │
│ │ Image Preview          : [Thumbnail Preview Area]        │ │
│ │                                                          │ │
│ │ Primary Image          : [Select Cover Image ▼]          │ │
│ │ (Choose one image as product cover)                      │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│ SECTION 3: UNITS & PACKAGING                                   │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ Measurement Units                                          │ │
│ │                                                          │ │
│ │ Base UOM*             : [▼ Nos / Ltr / Kg / Gram / ml / Set / Pkt] │
│ │                                                          │ │
│ │ Secondary UOM         : [▼ Same Options ▼]               │ │
│ │                                                          │ │
│ │ Packaging Details                                          │ │
│ │                                                          │ │
│ │ Package Type          : [▼ Bottle / Packet / Pouch / Box │ │
│ │                         Bag / Can / Set ▼]               │ │
│ │                                                          │ │
│ │ Quantity Per Package* : [________]                       │ │
│ │ Example: 1 Ltr per bottle                                │ │
│ │                                                          │ │
│ │ Units Per Package     : [________]                       │ │
│ │ Example: 12 bottles per box                              │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│ SECTION 4: PRODUCT VARIANTS                                    │
│ (Used when the same product has multiple packing sizes)       │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ [+ ADD VARIANT]                      [Delete]              │ │
│ │                                                          │ │
│ │ Variant Name*         : [________________________]       │ │
│ │ Example: 100 ml / 250 ml / 1 Ltr                         │ │
│ │                                                          │ │
│ │ Variant SKU           : [________________________]       │ │
│ │ (Auto-generate or manual entry)                          │ │
│ │                                                          │ │
│ │ Variant Package Type  : [▼ Bottle / Packet / Box / Pouch]│ │
│ │                                                          │ │
│ │ Variant Quantity*     : [________]                       │ │
│ │ (Base unit conversion value)                             │ │
│ │                                                          │ │
│ │ Barcode / QR Code     : [________________________]       │ │
│ │ (Scanner or manual entry)                                │ │
│ │                                                          │ │
│ │ Variant Status        : [☑ Active] Toggle                │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│ SECTION 5: TAX CONFIGURATION                                   │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ HSN Code*             : [🔍 Search Dropdown ▼]            │ │
│ │ (Select from Tax Module)                                  │ │
│ │                                                          │ │
│ │ Auto-filled Tax Information                              │ │
│ │                                                          │ │
│ │ Tax Type              : [Read Only]                      │ │
│ │ CGST Rate (%)         : [Read Only]                      │ │
│ │ SGST Rate (%)         : [Read Only]                      │ │
│ │ IGST Rate (%)         : [Read Only]                      │ │
│ │ CESS Rate (%)         : [Read Only]                      │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│ SECTION 6: DEFAULT PRICING                                     │
│ (Used as reference price during stock purchase)               │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ Purchase Price*       : ₹ [________]                       │ │
│ │                                                          │ │
│ │ Selling Price         : ₹ [________]                       │ │
│ │                                                          │ │
│ │ Base Price            : ₹ [________] (Pre-tax price)      │ │
│ │                                                          │ │
│ │ Tax Amount            : ₹ [Auto Calculated]               │ │
│ │                                                          │ │
│ │ Total Cost            : ₹ [Auto Calculated]               │ │
│ │                                                          │ │
│ │ [ SAVE PRODUCT ]     [ CANCEL ]                           │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

---

# Add Product Field Summary

| Section | Field                | Type               | Required | Notes                                                              |
| ------- | -------------------- | ------------------ | -------- | ------------------------------------------------------------------ |
| 1       | Product Name         | Text               | Yes      | Max 100 characters                                                 |
| 1       | Product Code         | Text               | No       | Auto-generate or manual                                            |
| 1       | Category             | Dropdown           | Yes      | Chemical / Sprayer / Electric Pump / Machine / Trap / Tool / Other |
| 1       | Sub-Type             | Cascading Dropdown | No       | Based on category                                                  |
| 1       | Company / Brand      | Search Dropdown    | Yes      | With Add New option                                                |
| 1       | Description          | Text Area          | No       | Product information                                                |
| 1       | Status               | Toggle             | No       | Active / Inactive                                                  |
| 2       | Product Images       | Multi Upload       | No       | Max 5 images                                                       |
| 2       | Primary Image        | Selection          | No       | Cover image                                                        |
| 3       | Base UOM             | Dropdown           | Yes      | Nos / Ltr / Kg / Gram / ml / Set / Pkt                             |
| 3       | Secondary UOM        | Dropdown           | No       | Same options                                                       |
| 3       | Package Type         | Dropdown           | No       | Bottle / Packet / Pouch / Box / Bag / Can / Set                    |
| 3       | Quantity Per Package | Number             | Yes      | Example: 1 Ltr per bottle                                          |
| 3       | Units Per Package    | Number             | No       | Example: 12 bottles per box                                        |
| 4       | Variant Name         | Text               | Yes      | Required when variant added                                        |
| 4       | Variant SKU          | Text               | No       | Auto or manual                                                     |
| 4       | Variant Package Type | Dropdown           | No       | Packaging type                                                     |
| 4       | Variant Quantity     | Number             | Yes      | Base conversion                                                    |
| 4       | Barcode / QR Code    | Text               | No       | Scanner input supported                                            |
| 4       | Variant Status       | Toggle             | No       | Active / Inactive                                                  |
| 5       | HSN Code             | Search Dropdown    | Yes      | From Tax Module                                                    |
| 5       | Tax Rates            | Auto               | System   | Read-only                                                          |
| 6       | Purchase Price       | Currency           | Yes      | Reference purchase price                                           |
| 6       | Selling Price        | Currency           | No       | Optional                                                           |
| 6       | Base Price           | Currency           | No       | Pre-tax                                                            |
| 6       | Tax Amount           | Auto               | System   | Calculated                                                         |
| 6       | Total Cost           | Auto               | System   | Calculated                                                         |

---

## 10.3 Edit Product

```
┌─────────────────────────────────────────────────────────────┐
│              EDIT PRODUCT                                    │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Same form as Add Product                            │    │
│  │                                                      │    │
│  │  DISABLED FIELDS:                                    │    │
│  │  • Product Code (if auto-generated)                  │    │
│  │                                                      │    │
│  │  [SAVE CHANGES]  [CANCEL]                            │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Edit Product Fields

Same as Add Product with Product Code disabled if auto-generated.

### Validation Rules

Same as Add Product, plus:

- Product Code cannot change if auto-generated
- HSN Code change updates tax rates from Module 9

---

Here is the **improved 10.4 View Product screen** with **Audit Information added** and structured in a way that is typical for **ERP read-only detail views**.

---

# 10.4 View Product

```
┌────────────────────────────────────────────────────────────────┐
│                         VIEW PRODUCT                            │
│                                                                │
│  Product information is displayed in READ-ONLY mode.           │
│  Structure follows the same layout as the Add Product screen. │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│ SECTION 1: BASIC PRODUCT INFORMATION                           │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ Product Name            : Chemical X                      │ │
│ │ Product Code            : PRD001                          │ │
│ │ Category                : Chemical                        │ │
│ │ Sub-Type                : Insecticide                     │ │
│ │ Company / Brand         : ABC Agro                        │ │
│ │ Description             : Pest control chemical product   │ │
│ │ Status                  : Active                          │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│ SECTION 2: PRODUCT MEDIA                                       │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ Product Images           : [Image Thumbnails]              │ │
│ │ Primary Image            : [Cover Image]                   │ │
│ │ Supported Formats        : JPG / PNG                       │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│ SECTION 3: UNITS & PACKAGING                                   │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ Base UOM                : Ltr                              │ │
│ │ Secondary UOM           : ml                               │ │
│ │                                                          │ │
│ │ Package Type            : Bottle                           │ │
│ │ Quantity Per Package    : 1 Ltr                            │ │
│ │ Units Per Package       : 12                               │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│ SECTION 4: PRODUCT VARIANTS                                    │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ Variant Name │ Variant SKU │ Package Type │ Quantity │Status│ │
│ │──────────────┼─────────────┼──────────────┼──────────┼──────│ │
│ │ 100 ml       │ PRD001-V1   │ Bottle       │ 0.1 Ltr  │Active│ │
│ │ 250 ml       │ PRD001-V2   │ Bottle       │ 0.25 Ltr │Active│ │
│ │ 1 Ltr        │ PRD001-V3   │ Bottle       │ 1 Ltr    │Active│ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│ SECTION 5: TAX CONFIGURATION                                   │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ HSN Code               : 1234                              │ │
│ │ Tax Type               : GST                               │ │
│ │ CGST Rate              : 9%                                │ │
│ │ SGST Rate              : 9%                                │ │
│ │ IGST Rate              : 18%                               │ │
│ │ CESS Rate              : 0%                                │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│ SECTION 6: DEFAULT PRICING                                     │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ Purchase Price          : ₹ 500                             │ │
│ │ Selling Price           : ₹ 650                             │ │
│ │ Base Price              : ₹ 550                             │ │
│ │ Tax Amount              : ₹ 99                              │ │
│ │ Total Cost              : ₹ 649                             │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│ SECTION 7: AUDIT INFORMATION                                   │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ Created By              : Admin                            │ │
│ │ Created Date            : 02 Jan 2026 10:35 AM             │ │
│ │ Last Updated By         : Head Ops                         │ │
│ │ Last Updated Date       : 05 Jan 2026 03:12 PM             │ │
│ │ Record Status           : Active                           │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                │
│ [ CLOSE ]                         [ EDIT ] (Head Ops Only)     │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

---

# Audit Fields

| Field             | Type     | Description                        |
| ----------------- | -------- | ---------------------------------- |
| Created By        | Text     | User who created the product       |
| Created Date      | DateTime | When product was created           |
| Last Updated By   | Text     | User who last modified the product |
| Last Updated Date | DateTime | Last modification timestamp        |

## 10.5 Delete Product

```
┌─────────────────────────────────────────────────────────────┐
│              DELETE PRODUCT                                  │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  SOFT DELETE (Mark as Inactive)                      │    │
│  │                                                      │    │
│  │  ⚠️ "This product will be marked as inactive.       │    │
│  │      Existing stock records will remain unchanged."  │    │
│  │                                                      │    │
│  │  [CANCEL]  [MARK AS INACTIVE]                        │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

