# 🎯 MODULE 9: TAX MANAGEMENT

## Overview

Allows the client (Company Admin) to configure and manage all tax-related settings before inventory and billing operations begin. Ensures proper GST structure setup, HSN code mapping with tax rates, automatic tax calculation in inventory, tax validation during purchase, and compliance-ready reporting. **Must be configured before adding inventory (Module 10).**

**Module Connections:**

- Required by: Module 10 (Inventory - Tax calculation)
- Uses: Module 9.6-9.9 (HSN Codes with Tax Types)
- Prerequisites: Must complete before Module 10 activation

---

Below is the **refactored version of 9.1 Tax Types Master – Table View** using your **final table fields** and keeping the layout **simple and clean**.

---

# 9.1 Tax Types Master – Table View

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                TAX TYPES                                     │
│                                                                             │
│  [+ ADD TAX TYPE]                                                            │
│                                                                             │
│  ┌──────────────────────────────────────────────────────────────────────┐   │
│  │ Filters                                                              │   │
│  │                                                                      │   │
│  │ Search: [____________________] (Tax Name / Category)                 │   │
│  │ Status: [▼ Active / Inactive ▼]     Applicability: [▼ Goods/Services/Both ▼] │
│  │ Created Date: [📅 From] - [📅 To]                                      │   │
│  │ Sort By: [▼ Latest / Rate High-Low / Rate Low-High ▼]                 │   │
│  │                                                                      │   │
│  │ [Reset Filters]                                                      │   │
│  └──────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  TAX TYPES TABLE                                                            │
│  ┌──────────────────────────────────────────────────────────────────────┐   │
│  │Tax Type│Tax Name│Tax Category│Default│Applicability│Status│Created By│   │
│  │ID      │        │            │Rate % │             │      │          │   │
│  │────────┼────────┼────────────┼───────┼─────────────┼──────┼──────────│   │
│  │T01     │CGST    │Central     │9%     │Both         │Active│Admin     │   │
│  │T02     │SGST    │State       │9%     │Both         │Active│Admin     │   │
│  │T03     │IGST    │Integrated  │18%    │Both         │Active│Admin     │   │
│  └──────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│ ┌─────────────────────────────────────────────────────────────────────────┐ │
│ │Created Date│Last Modified│Actions                                       │ │
│ │────────────┼─────────────┼─────────────────────────────────────────────│ │
│ │02-Jan-2026 │05-Jan-2026  │[👁 View] [✏ Edit] [🗑 Delete]                 │ │
│ │02-Jan-2026 │05-Jan-2026  │[👁 View] [✏ Edit] [🗑 Delete]                 │ │
│ │02-Jan-2026 │05-Jan-2026  │[👁 View] [✏ Edit] [🗑 Delete]                 │ │
│ └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│ Pagination:  Previous   1   2   3   ...   10   Next                         │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# Tax Types Table Fields

| Field         | Type           | Required | Description                             |
| ------------- | -------------- | -------- | --------------------------------------- |
| Tax Type ID   | Text           | Yes      | Unique tax identifier                   |
| Tax Name      | Text           | Yes      | Name of the tax (CGST, SGST, IGST etc.) |
| Tax Category  | Dropdown       | Yes      | Central / State / Integrated            |
| Default Rate  | Percentage     | Yes      | Default tax percentage                  |
| Applicability | Dropdown       | Yes      | Goods / Services / Both                 |
| Status        | Toggle / Badge | Yes      | Active / Inactive                       |
| Created By    | Text           | Auto     | User who created the tax                |
| Created Date  | Date           | Auto     | Date tax was created                    |
| Last Modified | Date           | Auto     | Last update date                        |
| Action        | Buttons        | —        | View / Edit / Delete                    |

---

# Filters & Search

| Filter             | Type       | Description                            |
| ------------------ | ---------- | -------------------------------------- |
| Search             | Text       | Search by Tax Name or Category         |
| Status             | Dropdown   | Active / Inactive                      |
| Applicability      | Dropdown   | Goods / Services / Both                |
| Created Date Range | Date Range | Filter by created date                 |
| Sort By            | Dropdown   | Latest / Rate High-Low / Rate Low-High |

---

# Actions

| Action | Behavior                            |
| ------ | ----------------------------------- |
| View   | Opens Tax Type details popup        |
| Edit   | Opens Tax Type edit form            |
| Delete | Deletes tax type after confirmation |

---

## 9.2 Add Tax Type

```
┌─────────────────────────────────────────────────────────────┐
│              ADD TAX TYPE                                    │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Tax Name*:                [____________]           │    │
│  │                                                      │    │
│  │  Tax Category*:            [▼ Dropdown ▼]           │    │
│  │    • Central Tax                                     │    │
│  │    • State Tax                                       │    │
│  │    • Integrated Tax                                  │    │
│  │    • Cess                                            │    │
│  │                                                      │    │
│  │  Default Rate (%)*:        [____]%                  │    │
│  │  (Max 2 decimal places)                             │    │
│  │                                                      │    │
│  │  Applicability*:           [▼ Dropdown ▼]           │    │
│  │    • Goods Only                                      │    │
│  │    • Services Only                                   │    │
│  │    • Both                                            │    │
│  │                                                      │    │
│  │  Description:              [____________________]   │    │
│  │                            [Textarea]               │    │
│  │                                                      │    │
│  │  Effective From*:          [📅 Date Picker]         │    │
│  │                                                      │    │
│  │  Status:                   [☑ Active] (Toggle)      │    │
│  │                                                      │    │
│  │  [SAVE]  [CANCEL]                                    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Add Tax Type Fields

| Field          | Type       | Required | Notes                           |
| -------------- | ---------- | -------- | ------------------------------- |
| Tax Name       | Text       | Yes      | Unique tax name                 |
| Tax Category   | Dropdown   | Yes      | Central/State/Integrated/Cess   |
| Default Rate   | Percentage | Yes      | Numeric, max 2 decimal places   |
| Applicability  | Dropdown   | Yes      | Goods/Services/Both             |
| Description    | Textarea   | No       | Additional details              |
| Effective From | Date       | Yes      | When tax becomes active         |
| Status         | Toggle     | Yes      | Active/Inactive, default Active |

### Validation Rules

| Field          | Rule                                                 |
| -------------- | ---------------------------------------------------- |
| Tax Name       | Required, unique across company, min 3 characters    |
| Tax Category   | Required, must be valid option                       |
| Default Rate   | Required, numeric, 0-100, max 2 decimal places       |
| Applicability  | Required, must be Goods/Services/Both                |
| Description    | Optional, max 500 characters                         |
| Effective From | Required, valid date, cannot be past if already used |
| Status         | Required, boolean                                    |

**Business Rules:**

- Tax Name must be unique
- Only one active version allowed for same tax name per date range
- Cannot delete tax if mapped to HSN (show warning)

---

## 9.3 Edit Tax Type

```
┌─────────────────────────────────────────────────────────────┐
│              EDIT TAX TYPE                                   │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Tax Name:                 [DISABLED - Value]       │    │
│  │  Tax Category:             [DISABLED - Value]       │    │
│  │                                                      │    │
│  │  Default Rate (%):         [____]%                  │    │
│  │  Applicability:            [▼ Dropdown ▼]           │    │
│  │  Description:              [____________________]   │    │
│  │  Effective From:           [📅 Date Picker]         │    │
│  │  Status:                   [☑ Active]               │    │
│  │                                                      │    │
│  │  Change Reason:            [____________________]   │    │
│  │  (Required if rate modified)                        │    │
│  │                                                      │    │
│  │  ⚠️ If tax mapped to HSN:                           │    │
│  │     Rate change creates new version (history saved) │    │
│  │                                                      │    │
│  │  [SAVE]  [CANCEL]                                    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Edit Tax Type Fields

| Field          | Type       | Required    | Notes                     |
| -------------- | ---------- | ----------- | ------------------------- |
| Tax Name       | Text       | No          | Disabled, cannot edit     |
| Tax Category   | Dropdown   | No          | Disabled if mapped to HSN |
| Default Rate   | Percentage | Yes         | Editable                  |
| Applicability  | Dropdown   | Yes         | Editable                  |
| Description    | Textarea   | No          | Editable                  |
| Effective From | Date       | Yes         | Editable                  |
| Status         | Toggle     | Yes         | Editable                  |
| Change Reason  | Textarea   | Conditional | Required if rate changed  |

### Validation Rules

| Field         | Rule                                                                   |
| ------------- | ---------------------------------------------------------------------- |
| Tax Name      | Cannot be edited                                                       |
| Tax Category  | Cannot change if mapped to HSN                                         |
| Default Rate  | Required, numeric, 0-100, max 2 decimals                               |
| Change Reason | Required if Default Rate changed, min 10 characters                    |
| Status        | Deactivation allowed only if not actively used in current transactions |

---

## 9.4 View Tax Type

```
┌─────────────────────────────────────────────────────────────┐
│              VIEW TAX TYPE                                   │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  All fields displayed in READ-ONLY mode              │    │
│  │                                                      │    │
│  │  TAX TYPE ID:              [Value]                  │    │
│  │  Tax Name:                 [Value]                  │    │
│  │  Tax Category:             [Value]                  │    │
│  │  Default Rate (%):         [Value]%                 │    │
│  │  Applicability:            [Value]                  │    │
│  │  Description:              [Value]                  │    │
│  │  Effective From:           [Value]                  │    │
│  │  Status:                   [Active/Inactive]        │    │
│  │                                                      │    │
│  │  Created By:               [Name]                   │    │
│  │  Created Date:             [Date]                   │    │
│  │  Last Modified By:         [Name]                   │    │
│  │  Last Modified Date:       [Date]                   │    │
│  │  Change Reason:            [If applicable]          │    │
│  │                                                      │    │
│  │  [CLOSE]                                             │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

## 9.5 Delete Tax Type

```
┌─────────────────────────────────────────────────────────────┐
│              DELETE TAX TYPE                                 │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  SOFT DELETE (Mark as Inactive)                      │    │
│  │                                                      │    │
│  │  IF HSN MAPPED:                                      │    │
│  │  ❌ "Cannot delete. This tax type is mapped to HSN   │    │
│  │      codes. First update the HSN codes, then delete."│    │
│  │                                                      │    │
│  │  IF NOT MAPPED:                                      │    │
│  │  ⚠️ "This tax type will be marked as inactive.      │    │
│  │      Existing transactions will retain historical data."│   │
│  │                                                      │    │
│  │  [CANCEL]  [MARK AS INACTIVE]                        │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

Below is the **refactored version of 9.6 HSN Code Master – Table View** using your **final table fields** and keeping the layout **simple and clean**.

---

# 9.6 HSN Code Master – Table View

```id="b0smig"
┌─────────────────────────────────────────────────────────────────────────────┐
│                              HSN CODE MASTER                                 │
│                                                                             │
│  [+ ADD HSN CODE]                                                            │
│                                                                             │
│  ┌──────────────────────────────────────────────────────────────────────┐   │
│  │ Filters                                                              │   │
│  │                                                                      │   │
│  │ Search: [____________________] (HSN Code / Description)              │   │
│  │ Product Category: [▼ Dropdown ▼]      Tax Type: [▼ Dropdown ▼]       │   │
│  │ Chapter: [__________]              Status: [▼ Active / Inactive ▼]   │   │
│  │ Created Date: [📅 From] - [📅 To]                                      │   │
│  │                                                                      │   │
│  │ [Reset Filters]                                                      │   │
│  └──────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  HSN CODES TABLE                                                            │
│  ┌──────────────────────────────────────────────────────────────────────┐   │
│  │HSN Code│Description│Chapter│Product Category│Tax Type│Status│Created By│ │
│  │────────┼───────────┼───────┼────────────────┼────────┼──────┼──────────│ │
│  │1234    │Chemical X │28     │Chemicals       │CGST,SGST│Active│Admin     │ │
│  │5678    │Machine Y  │84     │Machinery       │IGST     │Active│Admin     │ │
│  │9983    │IT Service │99     │Services        │IGST     │Active│Admin     │ │
│  └──────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│ ┌─────────────────────────────────────────────────────────────────────────┐ │
│ │Created Date│Actions                                                      │ │
│ │────────────┼───────────────────────────────────────────────────────────│ │
│ │02-Jan-2026 │[👁 View] [✏ Edit] [🗑 Delete]                                │ │
│ │02-Jan-2026 │[👁 View] [✏ Edit] [🗑 Delete]                                │ │
│ │02-Jan-2026 │[👁 View] [✏ Edit] [🗑 Delete]                                │ │
│ └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│ Pagination: Previous   1   2   3   ...   10   Next                          │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# HSN Code Table Fields

| Field            | Type           | Required | Description                                  |
| ---------------- | -------------- | -------- | -------------------------------------------- |
| HSN Code         | Text / Number  | Yes      | Unique HSN identifier                        |
| Description      | Text           | Yes      | Description of goods/services                |
| Chapter          | Number         | Yes      | HSN chapter number                           |
| Product Category | Dropdown       | Yes      | Product category mapping                     |
| Tax Type         | Multi-select   | Yes      | Applicable tax types (CGST, SGST, IGST etc.) |
| Status           | Toggle / Badge | Yes      | Active / Inactive                            |
| Created By       | Text           | Auto     | User who created record                      |
| Created Date     | Date           | Auto     | Date of record creation                      |
| Action           | Buttons        | —        | View / Edit / Delete                         |

---

# Filters

| Filter             | Type       | Description                       |
| ------------------ | ---------- | --------------------------------- |
| Search             | Text       | Search by HSN Code or Description |
| Product Category   | Dropdown   | Filter by product category        |
| Tax Type           | Dropdown   | Filter by mapped tax              |
| Chapter            | Text       | Filter by chapter number          |
| Status             | Dropdown   | Active / Inactive                 |
| Created Date Range | Date Range | Filter by created date            |

---

# Actions

| Action | Behavior                            |
| ------ | ----------------------------------- |
| View   | Opens HSN Code detail popup         |
| Edit   | Opens edit form                     |
| Delete | Deletes HSN code after confirmation |

---

## 9.7 Add HSN Code

```
┌─────────────────────────────────────────────────────────────┐
│              ADD HSN CODE                                    │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  HSN Code*:                [____________]           │    │
│  │  (4/6/8 digits only)                                │    │
│  │                                                      │    │
│  │  Description*:             [____________________]   │    │
│  │                            [Textarea]               │    │
│  │                                                      │    │
│  │  Chapter:                  [____________]           │    │
│  │  (Numeric + Text)                                   │    │
│  │                                                      │    │
│  │  Product Category*:        [▼ Dropdown ▼]           │    │
│  │    • Assets                                          │    │
│  │    • Consumables                                     │    │
│  │    • Resale                                          │    │
│  │                                                      │    │
│  │  Product Subcategory:      [▼ Dropdown ▼]           │    │
│  │    • Chemicals / Machine / Sprayer / Powder         │    │
│  │                                                      │    │
│  │  TAX CONFIGURATION                                   │    │
│  │  ──────────────────                                  │    │
│  │  Tax Type*:                [▼ Multi-select ▼]       │    │
│  │    (Select from active tax types)                   │    │
│  │                                                      │    │
│  │  Based on selection, show:                           │    │
│  │  CGST (%)*:                [____]%                  │    │
│  │  SGST (%)*:                [____]%                  │    │
│  │  IGST (%)*:                [____]%                  │    │
│  │  CESS (%):                 [____]% (Optional)       │    │
│  │                                                      │    │
│  │  Effective From*:          [📅 Date Picker]         │    │
│  │                                                      │    │
│  │  Status:                   [☑ Active] (Toggle)      │    │
│  │                                                      │    │
│  │  [SAVE]  [CANCEL]                                    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Add HSN Code Fields

| Field               | Type         | Required | Notes                            |
| ------------------- | ------------ | -------- | -------------------------------- |
| HSN Code            | Text         | Yes      | 4/6/8 digits, numeric only       |
| Description         | Textarea     | Yes      | Product description              |
| Chapter no.         | Text         | No       | Chapter classification           |
| Product Category    | Dropdown     | Yes      | Assets/Consumables/Resale        |
| Product Subcategory | Dropdown     | No       | Chemicals/Machine/Sprayer/Powder |
| Tax Type            | Multi-select | Yes      | Select active tax types          |
| CGST Rate           | Percentage   | Yes      | If CGST selected                 |
| SGST Rate           | Percentage   | Yes      | If SGST selected                 |
| IGST Rate           | Percentage   | Yes      | If IGST selected                 |
| CESS Rate           | Percentage   | No       | Optional, default 0              |
| Effective From      | Date         | Yes      | Validity start date              |
| Status              | Toggle       | Yes      | Active/Inactive                  |

### Validation Rules

| Field               | Rule                                                        |
| ------------------- | ----------------------------------------------------------- |
| HSN Code            | Required, numeric only, length 4/6/8, unique                |
| Description         | Required, min 5 characters                                  |
| Chapter             | Optional, alphanumeric                                      |
| Product Category    | Required, must be valid option                              |
| Product Subcategory | Optional, must be valid option                              |
| Tax Type            | Required, at least one active tax type                      |
| CGST Rate           | Required if CGST selected, 0-100, max 2 decimals            |
| SGST Rate           | Required if SGST selected, 0-100, max 2 decimals            |
| IGST Rate           | Required if IGST selected, 0-100, max 2 decimals            |
| CESS Rate           | Optional, 0-100, max 2 decimals                             |
| CGST + SGST = IGST  | Must equal IGST rate                                        |
| Effective From      | Required, valid date, cannot be earlier than system if used |

---

## 9.8 Edit HSN Code

```
┌─────────────────────────────────────────────────────────────┐
│              EDIT HSN CODE                                   │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  HSN Code:                 [DISABLED - Value]       │    │
│  │                                                      │    │
│  │  Description:              [____________________]   │    │
│  │  Chapter:                  [____________]           │    │
│  │  Product Category:         [▼ Dropdown ▼]           │    │
│  │  Product Subcategory:      [▼ Dropdown ▼]           │    │
│  │                                                      │    │
│  │  Tax Type:                 [▼ Multi-select ▼]       │    │
│  │  CGST (%):                 [____]%                  │    │
│  │  SGST (%):                 [____]%                  │    │
│  │  IGST (%):                 [____]%                  │    │
│  │  CESS (%):                 [____]%                  │    │
│  │                                                      │    │
│  │  Effective From:           [📅 Date Picker]         │    │
│  │  Status:                   [☑ Active]               │    │
│  │                                                      │    │
│  │  ⚠️ If HSN used in inventory:                       │    │
│  │     Tax rate update creates new effective version   │    │
│  │     Historical records preserved                    │    │
│  │                                                      │    │
│  │  [SAVE]  [CANCEL]                                    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Edit HSN Code Fields

| Field               | Type         | Required | Notes                 |
| ------------------- | ------------ | -------- | --------------------- |
| HSN Code            | Text         | No       | Disabled, cannot edit |
| Description         | Textarea     | Yes      | Editable              |
| Chapter             | Text         | No       | Editable              |
| Product Category    | Dropdown     | Yes      | Editable              |
| Product Subcategory | Dropdown     | No       | Editable              |
| Tax Type            | Multi-select | Yes      | Editable              |
| CGST Rate           | Percentage   | Yes      | Editable              |
| SGST Rate           | Percentage   | Yes      | Editable              |
| IGST Rate           | Percentage   | Yes      | Editable              |
| CESS Rate           | Percentage   | No       | Editable              |
| Effective From      | Date         | Yes      | Editable              |
| Status              | Toggle       | Yes      | Editable              |

### Validation Rules

| Field              | Rule                                                |
| ------------------ | --------------------------------------------------- |
| HSN Code           | Cannot be modified                                  |
| CGST + SGST = IGST | Must be equal                                       |
| Effective From     | Must be future or current date if tax rates changed |
| Status             | Deactivation warning if linked to active products   |

---

## 9.9 View HSN Code

```
┌─────────────────────────────────────────────────────────────┐
│              VIEW HSN CODE                                   │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  All fields in READ-ONLY mode                        │    │
│  │                                                      │    │
│  │  HSN Code:                 [Value]                  │    │
│  │  Description:              [Value]                  │    │
│  │  Chapter:                  [Value]                  │    │
│  │  Product Category:         [Value]                  │    │
│  │  Product Subcategory:      [Value]                  │    │
│  │  Tax Type Mapped:          [Value]                  │    │
│  │  CGST Rate:                [Value]%                 │    │
│  │  SGST Rate:                [Value]%                 │    │
│  │  IGST Rate:                [Value]%                 │    │
│  │  CESS Rate:                [Value]%                 │    │
│  │  Effective From:           [Value]                  │    │
│  │  Status:                   [Value]                  │    │
│  │                                                      │    │
│  │  Created By:               [Name]                   │    │
│  │  Created Date:             [Date]                   │    │
│  │  Last Modified By:         [Name]                   │    │
│  │  Last Modified Date:       [Date]                   │    │
│  │                                                      │    │
│  │  [CLOSE]                                             │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

## 9.10 Delete HSN Code

```
┌─────────────────────────────────────────────────────────────┐
│              DELETE HSN CODE                                 │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  SOFT DELETE (Mark as Inactive)                      │    │
│  │                                                      │    │
│  │  ⚠️ "This HSN code will be marked as inactive.      │    │
│  │      Existing inventory records will remain unchanged."│   │
│  │                                                      │    │
│  │  [CANCEL]  [MARK AS INACTIVE]                        │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

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
| Sub-Type        | Cascading Dropdown | Changes based on Category                                          |
| Company / Brand | Search Dropdown    | Manufacturer filter                                                |
| HSN Code        | Text / Numeric     | Search by tax code                                                 |
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

