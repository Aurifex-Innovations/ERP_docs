
# 🎯 MODULE 12: Service Management

## 12.1 Service Dashboard Table View



## Service Management Screen

  ### Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                               SERVICE MANAGEMENT                            │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌────────────────────────────── FILTERS ────────────────────────────────┐  │
│  │                                                                     │  │
│  │  Category:       [☑ Residential] [☑ Commercial]                     │  │
│  │                                                                     │  │
│  │  Pest Type:      [☑ Termite] [☑ Cockroach] [☑ Rodent] [☑ Bed Bug]   │  │
│  │                  [☑ Mosquito] [☑ Ants] [☑ Other]                    │  │
│  │                                                                     │  │
│  │  Price Type:     [☑ Fixed] [☑ Area Based] [☑ Inspection]            │  │
│  │                                                                     │  │
│  │  Status:         [☑ Active] [☑ Inactive]                            │  │
│  │                                                                     │  │
│  │  Warranty:       [☐ Yes] [☐ No]                                     │  │
│  │                                                                     │  │
│  │  Created Date:   [📅 From] - [📅 To]                                │  │
│  │                                                                     │  │
│  │  ---------------------------------------------------------------    │  │
│  │                                                                     │  │
│  │  Search: [________________________]                                 │  │
│  │          (Service Name / Pest Type / Service ID)                    │  │
│  │                                                                     │  │
│  │  [Reset Filters]                                 [+ ADD SERVICE]    │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│                          SERVICE OVERVIEW TABLE                             │
│                                                                             │
│  ┌─────┬────────────┬─────────────────┬──────────┬────────────┬────────┐   │
│  │ Img │ Service ID │ Service Name    │ Category │ Pest Type  │ Price  │   │
│  │     │            │                 │          │            │ Type   │   │
│  ├─────┼────────────┼─────────────────┼──────────┼────────────┼────────┤   │
│  │ 🐜  │ SVC-001    │ Termite Control │ Res.     │ Termite    │ Fixed  │   │
│  │ 🪳  │ SVC-002    │ Cockroach Gel   │ Comm.    │ Cockroach  │ Area   │   │
│  │ 🐀  │ SVC-003    │ Rodent Baiting  │ Res.     │ Rodent     │ Insp.  │   │
│  └─────┴────────────┴─────────────────┴──────────┴────────────┴────────┘   │
│                                                                             │
│  ┌──────────┬─────────────┬────────┬───────────────────────────────────┐   │
│  │ Duration │ Warranty    │ Status │ Actions                           │   │
│  ├──────────┼─────────────┼────────┼───────────────────────────────────┤   │
│  │ 2 Hrs    │ 6 Mo (2R)   │  🟢    │ [View] [Edit] [Delete]            │   │
│  │ 1 Hr     │ No Warranty │  🟢    │ [View] [Edit] [Delete]            │   │
│  │ 3 Hrs    │ 3 Mo (1R)   │  🔴    │ [View] [Edit] [Delete]            │   │
│  └──────────┴─────────────┴────────┴───────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```



## Field Display Format

| Field            | Format Example                     |
| ---------------- | ---------------------------------- |
| **Service ID**   | `SVC-001`, `SVC-002`               |
| **Service Name** | Max 20 chars, truncate with `...`  |
| **Category**     | `Res.` / `Comm.` (abbreviated)     |
| **Price Type**   | `Fixed` / `Area` / `Insp.`         |
| **Duration**     | `45 Min`, `1 Hr`, `2.5 Hrs`        |
| **Warranty**     | `6 Mo (2R)` = 6 Months, 2 Revisits |
| **Status**       | 🟢 / 🔴 Dot indicator              |


## Filters

| Filter               | Type               | Details                                         |
| -------------------- | ------------------ | ----------------------------------------------- |
| **Price Type**       | Multi-select       | Fixed Price, Area Based, Inspection Based       |
| **Price Range**      | Dual Slider/Inputs | Min ₹ \_\_\_ to Max ₹ \_\_\_                    |
| **Duration**         | Range Select       | 30 min → 2+ hours                               |
| **Warranty Period**  | Checkboxes         | No Warranty, 3 Months, 6 Months, 1 Year, Custom |
| **Treatment Method** | Multi-select       | Spray, Gel, Fumigation, Baiting                 |
| **Created Date**     | Date Range         | From \_\_\_ To \_\_\_                           |
| **Has Free Revisit** | Toggle             | Yes / No / All                                  |



## 12.2 Add Service



## Add Service Screen

  ### Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                     │
│  [← Back to Services]                 ADD NEW SERVICE                        [Save] │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│  ┌──────────────────────────── 🔴 REQUIRED FIELDS (MANDATORY) ────────────────────┐ │
│  │                                                                                │ │
│  │  Basic Service Information                                                     │ │
│  │  ┌──────────────────────────────────────────────────────────────────────────┐  │ │
│  │  │                                                                          │  │ │
│  │  │  Service Name:      [________________________________________] *        │  │ │
│  │  │                     Example: Termite Control, Cockroach Control         │  │ │
│  │  │                                                                          │  │ │
│  │  │  Service Category:  (•) Residential Pest Control (Internal)             │  │ │
│  │  │                     ( ) Commercial Pest Control (External)              │  │ │
│  │  │                                                                          │  │ │
│  │  │  Pest Type:         [☑] Termite    [☑] Cockroach   [☑] Rodent            │  │ │
│  │  │                     [☑] Bed Bug    [☑] Mosquito    [☑] Ants              │  │ │
│  │  │                     [+ Add New Pest Type]                                │  │ │
│  │  │                                                                          │  │ │
│  │  │  Description:       [                                                    │  │ │
│  │  │                     |                                                    │  │ │
│  │  │                     |________________________________________] *         │  │ │
│  │  │                     Summary of the service                               │  │ │
│  │  │                                                                          │  │ │
│  │  │  Base Price /       [₹ ______________________________________] *        │  │ │
│  │  │  Starting Price:                                                         │  │ │
│  │  │                                                                          │  │ │
│  │  │  Service Duration:  [__________] [Minutes ▼] *                           │  │ │
│  │  │                     Example: 30 minutes / 1 hour / 2 hours               │  │ │
│  │  │                                                                          │  │ │
│  │  │  Service Status:    (•) Active    ( ) Inactive                           │  │ │
│  │  │                                                                          │  │ │
│  │  └──────────────────────────────────────────────────────────────────────────┘  │ │
│  └────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌──────────────────────────── 🟡 OPTIONAL FIELDS ────────────────────────────────┐ │
│  │                                                                                │ │
│  │  Service Details                                                               │ │
│  │  ┌──────────────────────────────────────────────────────────────────────────┐  │ │
│  │  │                                                                          │  │ │
│  │  │  Service Image / Icon:   [📁 Choose File]  [No file chosen]              │  │ │
│  │  │  Recommended: 200x200px PNG/SVG                                          │  │ │
│  │  │                                                                          │  │ │
│  │  │  Service Banner Image:  [📁 Choose File]  [No file chosen]               │  │ │
│  │  │  Recommended: 1200x400px JPG/PNG                                         │  │ │
│  │  │                                                                          │  │ │
│  │  └──────────────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                                │ │
│  │  Pricing Configuration                                                         │ │
│  │  ┌──────────────────────────────────────────────────────────────────────────┐  │ │
│  │  │                                                                          │  │ │
│  │  │  Price Type:        (•) Fixed Price                                      │  │ │
│  │  │                     ( ) Area Based                                        │  │ │
│  │  │                     ( ) Inspection Based                                  │  │ │
│  │  │                                                                          │  │ │
│  │  │  ┌──────── If Area Based Selected ────────────────────────────────────┐  │  │ │
│  │  │  │                                                                    │  │  │ │
│  │  │  │  Residential Pricing:                                              │  │  │ │
│  │  │  │  [☑] 1BHK  [₹________]    [☑] 2BHK  [₹________]                   │  │  │ │
│  │  │  │  [☑] 3BHK  [₹________]    [☑] 4BHK+ [₹________]                   │  │  │ │
│  │  │  │                                                                    │  │  │ │
│  │  │  │  Commercial Pricing:                                               │  │  │ │
│  │  │  │  [☑] Small Office    [₹________]                                   │  │  │ │
│  │  │  │  [☑] Medium Office   [₹________]                                   │  │  │ │
│  │  │  │  [☑] Large Office    [₹________]                                   │  │  │ │
│  │  │  │                                                                    │  │  │ │
│  │  │  │  Area Range:                                                       │  │  │ │
│  │  │  │  Min Area: [________] sq.ft    Max Area: [________] sq.ft          │  │  │ │
│  │  │  │                                                                    │  │  │ │
│  │  │  └────────────────────────────────────────────────────────────────────┘  │  │ │
│  │  │                                                                          │  │ │
│  │  └──────────────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                                │ │
│  │  Warranty / Guarantee                                                          │ │
│  │  ┌──────────────────────────────────────────────────────────────────────────┐  │ │
│  │  │                                                                          │  │ │
│  │  │  Warranty Period:   [__________] [Months ▼]                               │  │ │
│  │  │  Example: 3 Months / 6 Months / 1 Year                                    │  │ │
│  │  │                                                                          │  │ │
│  │  │  Free Revisit Included: [☑] Included   Quantity: [________]               │  │ │
│  │  │                                                                          │  │ │
│  │  └──────────────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                                │ │
│  │  Chemicals / Treatment Info                                                    │ │
│  │  ┌──────────────────────────────────────────────────────────────────────────┐  │ │
│  │  │                                                                          │  │ │
│  │  │  Treatment Method:   [☑] Spray   [☑] Gel   [☑] Fumigation   [☑] Baiting   │  │ │
│  │  │  [+ Link Asset Product]                                                   │  │ │
│  │  │                                                                          │  │ │
│  │  │  Chemicals Used:                                                          │  │ │
│  │  │  [Search Chemicals...]  [+ Add Multiple]                                  │  │ │
│  │  │                                                                          │  │ │
│  │  │  Selected:                                                                │  │ │
│  │  │   • Chemical X (Qty: [__])  [🗑️]                                         │  │ │
│  │  │   • Chemical Y (Qty: [__])  [🗑️]                                         │  │ │
│  │  │                                                                          │  │ │
│  │  └──────────────────────────────────────────────────────────────────────────┘  │ │
│  └────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌──────────────────────────── 🔵 SYSTEM FIELDS ─────────────────────────────────┐ │
│  │                                                                                │ │
│  │  ┌─────────────────┬─────────────────┬─────────────────┬─────────────────┐    │ │
│  │  │ Service ID:     │ Created Date:   │ Created By:     │ Display Order:  │    │ │
│  │  │ [SVC-Auto]      │ [Auto]          │ [Admin Name]    │ [________]      │    │ │
│  │  │                 │                 │                 │                 │    │ │
│  │  │ Updated Date:   │ Updated By:     │                 │                 │    │ │
│  │  │ [Auto]          │ [Auto]          │                 │                 │    │ │
│  │  └─────────────────┴─────────────────┴─────────────────┴─────────────────┘    │ │
│  │                                                                                │ │
│  │  ⚠️ These fields are automatically populated by the system                    │ │
│  └────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │  [Cancel]                                     [Save as Draft]      [Save]   │  │
│  └──────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                     │
└─────────────────────────────────────────────────────────────────────────────────────┘
```


## Field by field brak down

### Section 1 : Requierd Fields

| Field                | Input Type        | Validation              | Helper Text                                   |
| -------------------- | ----------------- | ----------------------- | --------------------------------------------- |
| **Service Name**     | Text Input        | Required, Max 100 chars | "Example: Termite Control, Cockroach Control" |
| **Service Category** | Radio Button      | Required                | Single select: Residential / Commercial       |
| **Pest Type**        | Checkbox Group    | Min 1 required          | Multi-select + "Add New" button               |
| **Description**      | Textarea          | Required, Max 500 chars | "Summary of the service"                      |
| **Base Price**       | Currency Input    | Required, Number only   | Indian Rupee (₹) format                       |
| **Service Duration** | Number + Dropdown | Required                | Value + Unit (Min/Hrs)                        |
| **Service Status**   | Radio Toggle      | Required                | Active / Inactive                             |



### Section 2 : Optional Fields
#### Service Details
| Field                  | Input Type  | Specifications               |
| ---------------------- | ----------- | ---------------------------- |
| **Service Image/Icon** | File Upload | 200x200px, PNG/SVG, Max 2MB  |
| **Service Banner**     | File Upload | 1200x400px, JPG/PNG, Max 5MB |


#### Pricing Configuration
| Field                   | Input Type          | Conditional Logic                 |
| ----------------------- | ------------------- | --------------------------------- |
| **Price Type**          | Radio Button        | Controls visibility of sub-fields |
| **Residential Pricing** | Checkbox + Currency | Visible if "Area Based" selected  |
| **Commercial Pricing**  | Checkbox + Currency | Visible if "Area Based" selected  |
| **Area Range**          | Number Inputs       | Min/Max square feet               |


#### Warranty Section
| Field               | Input Type        | Notes                   |
| ------------------- | ----------------- | ----------------------- |
| **Warranty Period** | Number + Dropdown | Months/Years selection  |
| **Free Revisit**    | Checkbox + Number | Toggle + quantity input |


#### Treatment Info
| Field                | Input Type            | Integration                           |
| -------------------- | --------------------- | ------------------------------------- |
| **Treatment Method** | Checkbox Group        | Links to Asset Products               |
| **Chemicals Use**    | Search + Multi-select | Links to Consumable Products with qty |



### Section 3 : System Fields

| Field                | Value                    | Editable |
| -------------------- | ------------------------ | -------- |
| **Service ID**       | Auto-generated (SVC-001) | ❌ No     |
| **Created Date**     | Current timestamp        | ❌ No     |
| **Created By**       | Logged admin name        | ❌ No     |
| **Updated Date**     | Auto-updated             | ❌ No     |
| **Updated By**       | Auto-updated             | ❌ No     |
| **Display Priority** | Manual number input      | ✅ Yes    |


## 12.3 View Service



## View Service Screen

  ### Screen Layout

```md
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                     │
│  [← Back to Services]              VIEW SERVICE DETAILS                      [Edit] │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│  ┌──────────────────────────────── SERVICE HEADER ───────────────────────────────┐ │
│  │                                                                                │ │
│  │  ┌─────────┐  ┌─────────────────────────────────────────────────────────────┐ │ │
│  │  │         │  │  Service Name:     TERMITE CONTROL                          │ │ │
│  │  │   🐜    │  │  Service ID:       SVC-001                                  │ │ │
│  │  │  (Icon) │  │  Status:           🟢 ACTIVE                                │ │ │
│  │  │         │  │  Category:         🏠 Residential (Internal)                │ │ │
│  │  └─────────┘  └─────────────────────────────────────────────────────────────┘ │ │
│  │                                                                                │ │
│  └────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌──────────────────────────────── BASIC INFORMATION ────────────────────────────┐ │
│  │                                                                                │ │
│  │  ┌────────────────────────────┬─────────────────────────────────────────────┐ │ │
│  │  │ Pest Type:                 │ 🐜 Termite                                  │ │ │
│  │  ├────────────────────────────┼─────────────────────────────────────────────┤ │ │
│  │  │ Description:               │ Complete termite control treatment         │ │ │
│  │  │                            │ including soil treatment and wood          │ │ │
│  │  │                            │ protection for residential properties.     │ │ │
│  │  ├────────────────────────────┼─────────────────────────────────────────────┤ │ │
│  │  │ Base Price:                │ ₹ 2,500 (Starting Price)                   │ │ │
│  │  ├────────────────────────────┼─────────────────────────────────────────────┤ │ │
│  │  │ Service Duration:          │ 2 Hours                                    │ │ │
│  │  └────────────────────────────┴─────────────────────────────────────────────┘ │ │
│  │                                                                                │ │
│  └────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌──────────────────────────────── PRICING DETAILS ─────────────────────────────┐ │
│  │                                                                                │ │
│  │  Price Type:        📌 FIXED PRICE                                            │ │
│  │                                                                                │ │
│  │  ┌────────────────────────────────────────────────────────────────────────┐  │ │
│  │  │ Base Amount:     ₹ 2,500                                               │  │ │
│  │  │ (Fixed rate for all standard treatments)                               │  │ │
│  │  └────────────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                                │ │
│  └────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌────────────────────────────── WARRANTY INFORMATION ───────────────────────────┐ │
│  │                                                                                │ │
│  │  ┌────────────────────────────┬─────────────────────────────────────────────┐ │ │
│  │  │ Warranty Period:           │ 6 Months                                    │ │ │
│  │  ├────────────────────────────┼─────────────────────────────────────────────┤ │ │
│  │  │ Free Revisits:             │ 2 Revisits Included                         │ │ │
│  │  └────────────────────────────┴─────────────────────────────────────────────┘ │ │
│  │                                                                                │ │
│  └────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌──────────────────────────── TREATMENT & CHEMICALS ────────────────────────────┐ │
│  │                                                                                │ │
│  │  Treatment Methods:        🚿 Spray, 🧴 Gel                                   │ │
│  │                                                                                │ │
│  │  Chemicals Used:                                                               │ │
│  │                                                                                │ │
│  │  ┌─────────────────┬─────────────┬─────────────┬───────────────────────────┐ │ │
│  │  │ Product Code    │ Name        │ Type        │ Quantity Required         │ │ │
│  │  ├─────────────────┼─────────────┼─────────────┼───────────────────────────┤ │ │
│  │  │ CH-3808-001     │ Chemical X  │ Consumable  │ 500 ml                    │ │ │
│  │  │ CH-3809-002     │ Chemical Y  │ Consumable  │ 250 ml                    │ │ │
│  │  └─────────────────┴─────────────┴─────────────┴───────────────────────────┘ │ │
│  │                                                                                │ │
│  └────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌──────────────────────────────── MEDIA ASSETS ─────────────────────────────────┐ │
│  │                                                                                │ │
│  │  Service Icon:   [🐜 200x200px Icon Preview]                                  │ │
│  │                                                                                │ │
│  │  Banner Image:   [📷 1200x400px Banner Preview]                               │ │
│  │                                                                                │ │
│  └────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌──────────────────────────── SYSTEM INFORMATION ───────────────────────────────┐ │
│  │                                                                                │ │
│  │  ┌────────────────────┬────────────────────┬───────────────────────────────┐ │ │
│  │  │ Created Date       │ 15 Mar 2026        │ 10:30 AM                       │ │ │
│  │  ├────────────────────┼────────────────────┼───────────────────────────────┤ │ │
│  │  │ Created By         │ Admin User         │ (admin@pestcontrol.com)       │ │ │
│  │  ├────────────────────┼────────────────────┼───────────────────────────────┤ │ │
│  │  │ Last Updated       │ 16 Mar 2026        │ 02:15 PM                       │ │ │
│  │  ├────────────────────┼────────────────────┼───────────────────────────────┤ │ │
│  │  │ Updated By         │ Admin User         │ (admin@pestcontrol.com)       │ │ │
│  │  ├────────────────────┼────────────────────┼───────────────────────────────┤ │ │
│  │  │ Display Priority   │ 1                  │ (Order in service list)       │ │ │
│  │  └────────────────────┴────────────────────┴───────────────────────────────┘ │ │
│  │                                                                                │ │
│  └────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────────┐ │
│  │ [← Back]   [Print]   [Duplicate]   [Delete]                     [Edit Service]│ │
│  └──────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

### 📋 SECTION-BY-SECTION BREAKDOWN
#### 1. SERVICE HEADER

| Element            | Display Format                 |
| ------------------ | ------------------------------ |
| **Service Icon**   | Large 100x100px preview        |
| **Service Name**   | H1 heading, uppercase          |
| **Service ID**     | Monospace font, gray color     |
| **Status Badge**   | 🟢 Active / 🔴 Inactive pill   |
| **Category Badge** | 🏠 Residential / 🏢 Commercial |


#### 2. BASIC INFORMATION
| Field           | Display Rule                                    |
| --------------- | ----------------------------------------------- |
| **Pest Type**   | Icon + Label, multiple if applicable            |
| **Description** | Full text, wrap to 3 lines max with "Read more" |
| **Base Price**  | Currency format with "Starting Price" label     |
| **Duration**    | Human-readable (1.5 Hours vs 90 Minutes)        |


#### 3. WARRANTY INFORMATION
| Scenario         | Display                                |
| ---------------- | -------------------------------------- |
| **Has Warranty** | Period + Revisit count with checkmarks |
| **No Warranty**  | ❌ "No Warranty" in gray                |



## 12.4 Edit Service



## Edit Service Screen

  ### Screen Layout

```md
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                     │
│  [← Back to View]                     EDIT SERVICE                          [Update]│
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│  ┌──────────────────────────── 🔴 REQUIRED FIELDS (MANDATORY) ───────────────────┐ │
│  │                                                                              │ │
│  │  [Same as Add Service Screen - See Below for Structure]                     │ │
│  │                                                                              │ │
│  │  Includes:                                                                   │ │
│  │   • Service Name                                                             │ │
│  │   • Service Category                                                         │ │
│  │   • Pest Type                                                                │ │
│  │   • Description                                                              │ │
│  │   • Base / Starting Price                                                    │ │
│  │   • Service Duration                                                         │ │
│  │   • Service Status                                                           │ │
│  │                                                                              │ │
│  └──────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌──────────────────────────── 🟡 OPTIONAL FIELDS ───────────────────────────────┐ │
│  │                                                                              │ │
│  │  [Same as Add Service Screen - See Below for Structure]                     │ │
│  │                                                                              │ │
│  │  Includes:                                                                   │ │
│  │   • Service Image / Icon                                                     │ │
│  │   • Service Banner Image                                                     │ │
│  │   • Pricing Configuration (Fixed / Area Based / Inspection)                 │ │
│  │   • Warranty Period                                                          │ │
│  │   • Free Revisit Settings                                                    │ │
│  │   • Treatment Method                                                         │ │
│  │   • Chemicals / Consumables Used                                             │ │
│  │                                                                              │ │
│  └──────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌──────────────────────────── 🔵 SYSTEM FIELDS ────────────────────────────────┐ │
│  │                                                                              │ │
│  │  [Same as Add Service Screen - See Below for Structure]                     │ │
│  │                                                                              │ │
│  │  ┌─────────────────┬─────────────────┬─────────────────┬─────────────────┐ │ │
│  │  │ Service ID:     │ Created Date:   │ Created By:     │ Display Order:  │ │ │
│  │  │ [SVC-Auto]      │ [Auto]          │ [Admin Name]    │ [________]      │ │ │
│  │  │                 │                 │                 │                 │ │ │
│  │  │ Updated Date:   │ Updated By:     │                 │                 │ │ │
│  │  │ [Auto]          │ [Auto]          │                 │                 │ │ │
│  │  └─────────────────┴─────────────────┴─────────────────┴─────────────────┘ │ │
│  │                                                                              │ │
│  │  ⚠️ These fields are automatically maintained by the system                 │ │
│  │                                                                              │ │
│  └──────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌──────────────────────────── 🟣 AUDIT LOG ────────────────────────────────────┐ │
│  │                                                                              │ │
│  │  ┌────────────────────────────┬───────────────────────────────────────────┐ │ │
│  │  │ Edited By:                 │ [Auto-detect: Current User]               │ │ │
│  │  │                            │ Admin User (admin@pestcontrol.com)        │ │ │
│  │  ├────────────────────────────┼───────────────────────────────────────────┤ │ │
│  │  │ Edited Date:               │ [Auto-detect: Current Date & Time]        │ │ │
│  │  │                            │ 16 Mar 2026, 03:45 PM                     │ │ │
│  │  └────────────────────────────┴───────────────────────────────────────────┘ │ │
│  │                                                                              │ │
│  │  ⚠️ These fields will automatically update when "Update Service" is clicked │ │
│  │      and all the fields are filled automatically                             │ │
│  └──────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────────┐ │
│  │  [Cancel]      [Reset Changes]                           [Update Service]   │ │
│  └──────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
└─────────────────────────────────────────────────────────────────────────────────────┘
```


#### 🎨 AUDIT LOG FIELD SPECIFICATIONS
| Field               | Behavior                                     | Display Format            |
| ------------------- | -------------------------------------------- | ------------------------- |
| **Edited By**       | Auto-populate from logged-in user session    | Name + Email + Role badge |
| **Edited Date**     | Auto-capture system datetime on Update click | Date + Time (24hr format) |
| **Change Tracking** | Compare old vs new values                    | Highlight modified fields |


## 12.4 Edit Service



## Edit Service Screen

  ### Screen Layout

```md
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│                          ⚠️ CONFIRMATION                         │
│                                                                 │
│                                                                 │
│           Are you sure you want to mark this service            │
│                        as inactive?                             │
│                                                                 │
│                                                                 │
│              [Cancel]              [Mark as Inactive]           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```


