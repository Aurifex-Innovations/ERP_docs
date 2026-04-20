# 🎯 MODULE 16: Quotation Management

## Overview

Quotation Management handles the creation, tracking, and lifecycle of quotations for **Services** (contract-based / one-time job) and **Product Sales**. Quotations can be generated from existing Leads (Module 15), Customers (Module 9), or created for new prospects directly.

**Module Connections:**

- Depends on: Module 12 (Service Catalog & Pricing), Module 10 (Product Master & Tax), Module 9 (Tax Management)
- Uses: Module 15 (Lead Data), Module 9 (Customer Data), Module 7 (Branch Management)
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

| Field              | Type     | Required | Description                                      |
| ------------------ | -------- | -------- | ------------------------------------------------ |
| Quotation ID       | Text     | Auto     | Unique identifier (QT-YYYY-SEQ format)           |
| Source             | Badge    | Yes      | From Lead / From Customer / New Prospect         |
| Client Name        | Text     | Yes      | Lead name, customer name, or new prospect name   |
| Quotation Type     | Badge    | Yes      | Service / Product / Combined                     |
| Branch             | Text     | Yes      | Assigned branch for quotation                    |
| Total Amount (₹)   | Currency | Auto     | Grand total including tax                        |
| Status             | Badge    | Auto     | Draft / Sent / Viewed / Accepted / Rejected / Expired / Revised |
| Valid Till          | Date     | Yes      | Quotation expiry date                            |
| Created Date & Time| DateTime | Auto     | Timestamp of quotation creation                  |
| Created By         | Text     | Auto     | User who created the quotation                   |
| Actions            | Buttons  | —        | View / Delete (Revoke) / Download                |

---

# Actions

| Action   | Icon | Available When          | Description                                    |
| -------- | ---- | ----------------------- | ---------------------------------------------- |
| View     | 👁   | All statuses            | Opens quotation details in read-only mode      |
| Delete   | 🗑   | Status = Draft only     | Opens delete confirmation popup (16.4)         |
| Download | 📥   | All statuses            | Downloads quotation as PDF                     |

**Note:** Delete button is **hidden/disabled** when quotation status is anything other than "Draft" (i.e., once sent, it cannot be deleted).

---

# Filters

| Filter         | Type              | Default    | Options                                                              |
| -------------- | ----------------- | ---------- | -------------------------------------------------------------------- |
| Status         | Multi-select      | All        | Draft / Sent / Viewed / Accepted / Rejected / Expired / Revised     |
| Quotation Type | Multi-select      | All        | Service / Product / Combined                                         |
| Source         | Multi-select      | All        | From Lead / From Customer / New Prospect                             |
| Branch         | Searchable Dropdown| All       | All branches from Module 7                                           |
| Created By     | Searchable Dropdown| All       | Sales team members                                                   |
| Created Date   | Date Range        | Last 30 Days| Custom range                                                        |
| Amount Range   | Number Range      | All        | Min – Max amount filter                                              |

---

# Search

Global search supports:

| Search Field    | Type    |
| --------------- | ------- |
| Quotation ID    | Text    |
| Client Name     | Text    |
| Phone Number    | Numeric |
| Lead ID         | Text    |

---

# 16.2 Add Quotation Form

**Description:**
The Add Quotation form allows sales team members to create new quotations. The form dynamically adjusts based on the selected source (Lead / Customer / New Prospect) and quotation type (Service / Product / Combined). Pricing is dynamically fetched from Module 12 (Services) and Module 10 (Products) based on selected items and property type. Multiple service locations can be added with per-location pricing.

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
│  │  Property Type:       3BHK Apartment (Read-only)                        │ │
│  │  Address:             HSR Layout, Bengaluru (Read-only)                  │ │
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
│  │  Property Type*:      [▼ 1BHK / 2BHK / 3BHK / 4BHK+ / Villa /         │ │
│  │                        Small Office / Medium Office / Large Office /     │ │
│  │                        Warehouse / Industrial ▼]                        │ │
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
│  │  (•) One-Time Service    ( ) AMC (Annual Maintenance Contract)          │ │
│  │                                                                         │ │
│  │  IF AMC SELECTED:                                                       │ │
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
│  │  │ Property Type*:  [▼ 1BHK / 2BHK / 3BHK / 4BHK+ / Villa /       │  │ │
│  │  │                   Small Office / Medium Office / Large Office /   │  │ │
│  │  │                   Warehouse / Industrial ▼]                      │  │ │
│  │  │ Area (sqft)*:    [1200]                                          │  │ │
│  │  │ Assign Branch*:  [▼ BLR-HSR Branch ▼]                           │  │ │
│  │  │                  (Nearest branch auto-suggested)                  │  │ │
│  │  └──────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                         │ │
│  │  LOCATION 2                                                             │ │
│  │  ┌──────────────────────────────────────────────────────────────────┐  │ │
│  │  │ Address*:        [________________________________]              │  │ │
│  │  │ City*:           [________________________________]              │  │ │
│  │  │ State*:          [▼ Select State ▼]                              │  │ │
│  │  │ Property Type*:  [▼ Select Property Type ▼]                      │  │ │
│  │  │ Area (sqft)*:    [________]                                      │  │ │
│  │  │ Assign Branch*:  [▼ Select Branch ▼]                             │  │ │
│  │  │ [Remove Location]                                                │  │ │
│  │  └──────────────────────────────────────────────────────────────────┘  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 4: SERVICE SELECTION (Visible if Type = Service / Combined)         │
│  (Integrated with Module 12 – Service Management)                            │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  PER LOCATION SERVICE ASSIGNMENT                                        │ │
│  │                                                                         │ │
│  │  ── LOCATION 1: HSR Layout, Bengaluru (3BHK) ──                        │ │
│  │  [🔍 Search Service from Service Master...]                             │ │
│  │                                                                         │ │
│  │  ┌────────────┬──────────┬─────────┬──────────┬─────────┬────────────┐│ │
│  │  │Service Name│Pest Type │Category │Price Type│Rate (₹) │Warranty    ││ │
│  │  │────────────┼──────────┼─────────┼──────────┼─────────┼────────────││ │
│  │  │Termite Ctrl│Termite   │Resident.│Fixed     │₹1,800   │6Mo (2 Rev) ││ │
│  │  │  (SVC-001) │          │3BHK     │         │(Auto)   │(Auto)      ││ │
│  │  │────────────┼──────────┼─────────┼──────────┼─────────┼────────────││ │
│  │  │Cockroach   │Cockroach │Resident.│Fixed     │₹1,200   │3Mo (1 Rev) ││ │
│  │  │Gel (SVC-002│          │3BHK     │         │(Auto)   │(Auto)      ││ │
│  │  └────────────┴──────────┴─────────┴──────────┴─────────┴────────────┘│ │
│  │                                                                         │ │
│  │  ┌────────────────────┬────────────┬──────────┬───────────────────────┐│ │
│  │  │Frequency           │Qty/Visits  │Total (₹) │Action                 ││ │
│  │  │────────────────────┼────────────┼──────────┼───────────────────────││ │
│  │  │One-Time            │ 1          │₹1,800    │[Remove]               ││ │
│  │  │────────────────────┼────────────┼──────────┼───────────────────────││ │
│  │  │Monthly (12 visits) │ 12         │₹14,400   │[Remove]               ││ │
│  │  └────────────────────┴────────────┴──────────┴───────────────────────┘│ │
│  │                                                                         │ │
│  │  Location 1 Service Subtotal: ₹16,200                                   │ │
│  │                                                                         │ │
│  │  ── LOCATION 2: Whitefield, Bengaluru (Small Office) ──                 │ │
│  │  [🔍 Search Service...]                                                 │ │
│  │  (Same table structure as Location 1)                                   │ │
│  │                                                                         │ │
│  │  Location 2 Service Subtotal: ₹8,500                                    │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 5: PRODUCT SELECTION (Visible if Type = Product / Combined)         │
│  (Integrated with Module 10 – Product Master)                                │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  [🔍 Search Product from Product Master...]                             │ │
│  │                                                                         │ │
│  │  ┌────────────┬──────────┬─────┬──────────┬─────────┬──────────┬─────┐│ │
│  │  │Product Name│Prod. Code│UOM  │HSN Code  │Unit ₹   │Qty       │Total││ │
│  │  │────────────┼──────────┼─────┼──────────┼─────────┼──────────┼─────││ │
│  │  │Brass Sprayer│BSP3-001 │Nos  │8424      │₹1,200   │[  5  ]   │₹6,000│ │
│  │  │  (Auto)    │(Auto)    │(Auto│(Auto)    │(Auto)   │          │(Auto)│ │
│  │  │────────────┼──────────┼─────┼──────────┼─────────┼──────────┼─────││ │
│  │  │Chemical X  │CH-001    │Ltr  │1234      │₹500     │[ 10  ]   │₹5,000│ │
│  │  │  (Auto)    │(Auto)    │(Auto│(Auto)    │(Auto)   │          │(Auto)│ │
│  │  └────────────┴──────────┴─────┴──────────┴─────────┴──────────┴─────┘│ │
│  │                                                                         │ │
│  │  ┌──────────┬──────────┬──────────┬──────────────────────────────────┐ │ │
│  │  │CGST (₹)  │SGST (₹)  │IGST (₹)  │Line Total (₹)                   │ │ │
│  │  │──────────┼──────────┼──────────┼──────────────────────────────────│ │ │
│  │  │₹540      │₹540      │—         │₹7,080                            │ │ │
│  │  │──────────┼──────────┼──────────┼──────────────────────────────────│ │ │
│  │  │₹450      │₹450      │—         │₹5,900                            │ │ │
│  │  └──────────┴──────────┴──────────┴──────────────────────────────────┘ │ │
│  │                                                                         │ │
│  │  Product Subtotal: ₹12,980                                              │ │
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
│  │  │ HSR Layout, BLR    │ ₹16,200      │ —            │ ₹16,200       ││ │
│  │  │ Whitefield, BLR    │ ₹8,500       │ —            │ ₹8,500        ││ │
│  │  │ Products (General) │ —            │ ₹12,980      │ ₹12,980       ││ │
│  │  └────────────────────────────────────────────────────────────────────┘│ │
│  │                                                                         │ │
│  │  Services Subtotal         : ₹24,700                                    │ │
│  │  Products Subtotal         : ₹11,000                                    │ │
│  │  ──────────────────────────────────────────────────────────────────     │ │
│  │  Subtotal (Before Tax)     : ₹35,700                                    │ │
│  │  Tax (CGST + SGST / IGST)  : ₹1,980                                    │ │
│  │  ──────────────────────────────────────────────────────────────────     │ │
│  │  Total Before Discount     : ₹37,680                                    │ │
│  │  Discount Type:            [▼ Percentage / Flat Amount ▼]               │ │
│  │  Discount Value:           [________]                                   │ │
│  │  Discount Amount           : - ₹3,768                                   │ │
│  │  ──────────────────────────────────────────────────────────────────     │ │
│  │  ★ GRAND TOTAL             : ₹33,912                                   │ │
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

| Field            | Type            | Required    | Options/Validation                                                                 | Notes                                  |
| ---------------- | --------------- | ----------- | ---------------------------------------------------------------------------------- | -------------------------------------- |
| Source Type      | Radio           | Yes         | From Lead / From Customer / Add New                                                | Determines which sub-fields appear     |
| Select Lead      | Search Dropdown | Conditional | Active leads from Module 15 (Status ≥ QUALIFIED)                                  | Required if Source = From Lead         |
| Select Customer  | Search Dropdown | Conditional | Active customers from Module 9                                                    | Required if Source = From Customer     |
| Full Name        | Text            | Conditional | Min 3 characters                                                                   | Required if Source = Add New           |
| Phone            | Number          | Conditional | 10-digit Indian mobile                                                             | Required if Source = Add New           |
| Email            | Email           | No          | Valid email format                                                                 | Optional for Add New                   |
| Company Name     | Text            | No          | Max 100 characters                                                                 | Optional for Add New                   |
| Property Type    | Dropdown        | Conditional | 1BHK / 2BHK / 3BHK / 4BHK+ / Villa / Small Office / Medium Office / Large Office / Warehouse / Industrial | Required if Source = Add New |
| Address          | Text            | Conditional | Min 10 characters                                                                  | Required if Source = Add New           |
| City             | Text            | Conditional | Min 3 characters                                                                   | Required if Source = Add New           |
| State            | Dropdown        | Conditional | Indian states list                                                                 | Required if Source = Add New           |
| Pincode          | Number          | No          | 6-digit                                                                            | Optional                               |

---

## Section 2: Quotation Type Fields

| Field             | Type     | Required    | Options/Validation                                    | Notes                                  |
| ----------------- | -------- | ----------- | ----------------------------------------------------- | -------------------------------------- |
| Quotation Type    | Radio    | Yes         | Service / Product / Combined                          | Controls which sections are visible    |
| Service Mode      | Radio    | Conditional | One-Time / AMC                                        | Visible if Type = Service or Combined  |
| AMC Frequency     | Dropdown | Conditional | Monthly / Quarterly / Half-Yearly / Yearly            | Required if Mode = AMC                 |
| Contract Duration | Dropdown | Conditional | 6 Months / 1 Year / 2 Years / 3 Years                | Required if Mode = AMC                 |
| Proposed Start    | Date     | Conditional | ≥ Today                                               | Required if Mode = AMC                 |

---

## Section 3: Location & Branch Assignment Fields

| Field         | Type              | Required | Options/Validation                                      | Notes                                |
| ------------- | ----------------- | -------- | ------------------------------------------------------- | ------------------------------------ |
| Address       | Text              | Yes      | Min 10 characters                                       | Service delivery address             |
| City          | Text              | Yes      | Min 3 characters                                        | City name                            |
| State         | Dropdown          | Yes      | Indian states list                                      | State selection                      |
| Property Type | Dropdown          | Yes      | Residential / Commercial types (same as Module 12 pricing) | Determines service pricing           |
| Area (sqft)   | Number            | Yes      | Must be > 0                                             | Used for area-based pricing          |
| Assign Branch | Searchable Dropdown| Yes     | Active branches from Module 7                           | Nearest branch auto-suggested        |

**Business Rules:**
- First location is auto-populated from lead/customer address if available
- Additional locations can be added via "Add Location" button
- Each location gets independent service selection and pricing
- Branch assignment determines which branch handles that location

---

## Section 4: Service Selection Fields

_(Integrated with Module 12 – Service Management)_

| Field            | Type            | Required | Validation                                     | Notes                                      |
| ---------------- | --------------- | -------- | ---------------------------------------------- | ------------------------------------------ |
| Service Name     | Search Dropdown | Yes      | Active services from Module 12                 | Fetches service configuration              |
| Pest Type        | Auto-filled     | System   | From service master                            | Read-only                                  |
| Category         | Auto-filled     | System   | Residential / Commercial matched to location   | Read-only                                  |
| Price Type       | Auto-filled     | System   | Fixed / Area Based / Inspection                | From service master                        |
| Rate (₹)         | Auto-calculated | System   | Based on Property Type from Module 12 pricing  | E.g., 3BHK = ₹1,800 for Termite Control   |
| Warranty         | Auto-filled     | System   | Period + revisit count from service config     | Read-only                                  |
| Frequency        | Dropdown        | Yes      | One-Time / Monthly / Quarterly / Half-Yearly / Yearly | Overrides global if needed         |
| Qty / Visits     | Auto-calculated | System   | Based on frequency × duration                  | E.g., Monthly × 1 Year = 12               |
| Total (₹)        | Auto-calculated | System   | Rate × Qty/Visits                              | Per-service total                          |

**Business Rules:**
- When service is selected, pricing auto-populates based on **Property Type of the location**
  - Residential: 1BHK / 2BHK / 3BHK / 4BHK+ pricing from Module 12
  - Commercial: Small Office / Medium Office / Large Office / Warehouse pricing from Module 12
- If Price Type = "Area Based", rate = per sqft rate × area
- For AMC, total = rate × number of visits in contract duration
- Multiple services can be added per location

---

## Section 5: Product Selection Fields

_(Integrated with Module 10 – Product Master)_

| Field         | Type            | Required | Validation                          | Notes                       |
| ------------- | --------------- | -------- | ----------------------------------- | --------------------------- |
| Product Name  | Search Dropdown | Yes      | Active products from Module 10      | Fetches product details     |
| Product Code  | Auto-filled     | System   | From product master                 | Read-only                   |
| UOM           | Auto-filled     | System   | From product master                 | Read-only                   |
| HSN Code      | Auto-filled     | System   | From product master                 | Read-only                   |
| Unit Price (₹)| Auto-filled     | System   | Selling price from Module 10        | Editable (override allowed) |
| Quantity      | Number          | Yes      | Must be ≥ 1                         | User input                  |
| Total (₹)     | Auto-calculated | System   | Unit Price × Quantity               | Pre-tax total               |
| CGST (₹)      | Auto-calculated | System   | From HSN → Tax Module (Module 9)   | Intra-state tax             |
| SGST (₹)      | Auto-calculated | System   | From HSN → Tax Module (Module 9)   | Intra-state tax             |
| IGST (₹)      | Auto-calculated | System   | From HSN → Tax Module (Module 9)   | Inter-state tax             |
| Line Total (₹)| Auto-calculated | System   | Total + applicable tax              | Final line amount           |

**Business Rules:**
- Product selection auto-populates code, UOM, HSN, and pricing from Module 10
- Tax calculation auto-applies based on HSN code from Module 9
- CGST+SGST applies for same-state; IGST for inter-state (based on branch vs client state)
- Unit price is editable to allow custom pricing per quotation

---

## Section 6: Pricing Summary Fields

| Field                   | Type            | Required | Notes                                    |
| ----------------------- | --------------- | -------- | ---------------------------------------- |
| Services Subtotal       | Auto-calculated | System   | Sum of all service line totals           |
| Products Subtotal       | Auto-calculated | System   | Sum of all product pre-tax totals        |
| Subtotal (Before Tax)   | Auto-calculated | System   | Services + Products subtotals            |
| Tax Total               | Auto-calculated | System   | Sum of all tax amounts                   |
| Total Before Discount   | Auto-calculated | System   | Subtotal + Tax                           |
| Discount Type           | Dropdown        | No       | Percentage / Flat Amount                 |
| Discount Value          | Number          | No       | % value or fixed ₹ amount               |
| Discount Amount         | Auto-calculated | System   | Computed from type and value             |
| Grand Total             | Auto-calculated | System   | Total Before Discount − Discount Amount  |

---

## Section 7: Terms & Validity Fields

| Field                    | Type      | Required | Validation                      | Notes                            |
| ------------------------ | --------- | -------- | ------------------------------- | -------------------------------- |
| Quotation Valid Till      | Date      | Yes      | ≥ Today, default: Today + 15   | Expiry date                      |
| Payment Terms            | Dropdown  | Yes      | 100% Advance / 50-50 / Net 15 / Net 30 / Custom | Custom shows text area |
| Custom Payment Terms     | Text Area | Conditional | Min 10 characters             | Visible only if "Custom"         |
| Special Terms/Conditions | Text Area | No       | Max 1000 characters             | Additional T&C                   |

---

## Section 8: Attachments & Notes Fields

| Field              | Type        | Required | Validation                         | Notes                      |
| ------------------ | ----------- | -------- | ---------------------------------- | -------------------------- |
| Upload Attachments | Multi File  | No       | Max 5 files, 5MB each, PDF/JPG/PNG/DOC | Supporting documents   |
| Internal Notes     | Text Area   | No       | Max 500 characters                 | Not on customer-facing PDF |

---

## Actions

| Action         | Behavior                                                                                   |
| -------------- | ------------------------------------------------------------------------------------------ |
| Save Draft     | Saves quotation with Status = Draft, no notifications sent                                 |
| Send Quotation | Validates all fields, generates PDF, sends to client via email/WhatsApp, Status → Sent     |
| Cancel         | Discards changes, returns to Quotation Dashboard                                           |

---

## Automatic System Behaviors

| Trigger                          | System Action                                                        |
| -------------------------------- | -------------------------------------------------------------------- |
| Source = From Lead               | Auto-populate lead details, link quotation to lead record            |
| Source = From Customer           | Auto-populate customer details, link quotation to customer record    |
| Service Selected                 | Auto-fetch pricing from Module 12 based on property type             |
| Product Selected                 | Auto-fetch pricing, UOM, HSN from Module 10; tax from Module 9      |
| Property Type Changed            | Recalculate service pricing for affected location                    |
| Quantity / Visits Changed        | Recalculate line totals and grand total                              |
| Discount Applied                 | Recalculate grand total                                              |
| Send Quotation                   | Generate PDF, send notification, update lead status to PROPOSAL      |
| Quotation Accepted               | Trigger Contract creation workflow (Module 18), update lead to WON   |
| Validity Date Passed             | Auto-expire quotation, notify sales rep                              |

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
│  │  Property Type:       3BHK Apartment                                    │ │
│  │  Address:             HSR Layout, Sector 2, Bengaluru, Karnataka        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  QUOTATION CONFIGURATION                                                     │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Quotation Type:      Service Quotation                                 │ │
│  │  Service Mode:        AMC (Annual Maintenance Contract)                 │ │
│  │  Frequency:           Monthly                                           │ │
│  │  Contract Duration:   1 Year                                            │ │
│  │  Proposed Start:      01-Apr-2026                                       │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SERVICE DETAILS BY LOCATION                                                 │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  📍 LOCATION 1: HSR Layout, Bengaluru | 3BHK | 1200 sqft               │ │
│  │     Assigned Branch: BLR-HSR Branch                                     │ │
│  │                                                                         │ │
│  │  ┌────────────┬──────────┬─────────┬─────────┬────────┬────────────┐  │ │
│  │  │Service     │Pest Type │Rate (₹) │Frequency│Visits  │Total (₹)   │  │ │
│  │  │────────────┼──────────┼─────────┼─────────┼────────┼────────────│  │ │
│  │  │Termite Ctrl│Termite   │₹1,800   │Monthly  │12      │₹21,600     │  │ │
│  │  │Cockroach   │Cockroach │₹1,200   │Quarterly│4       │₹4,800      │  │ │
│  │  └────────────┴──────────┴─────────┴─────────┴────────┴────────────┘  │ │
│  │  Location 1 Subtotal: ₹26,400                                          │ │
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

| Section              | Field                  | Type            | Description                                 |
| -------------------- | ---------------------- | --------------- | ------------------------------------------- |
| **Status Bar**       | Status Timeline        | Progress Bar    | Visual status progression                   |
|                      | Current Status         | Badge           | Active status highlighted                   |
|                      | Status Timestamps      | DateTime        | When each status was reached                |
| **Client Info**      | Source                 | Badge + Link    | Lead/Customer/New with clickable reference  |
|                      | Client Name            | Text            | Full name of client                         |
|                      | Phone                  | Text            | Contact number                              |
|                      | Email                  | Text            | Email address                               |
|                      | Property Type          | Text            | Type of property                            |
|                      | Address                | Text            | Full address                                |
| **Configuration**    | Quotation Type         | Text            | Service / Product / Combined                |
|                      | Service Mode           | Text            | One-Time / AMC                              |
|                      | Frequency              | Text            | AMC frequency if applicable                 |
|                      | Contract Duration      | Text            | AMC duration if applicable                  |
|                      | Proposed Start         | Date            | AMC start date if applicable                |
| **Services**         | Per-location table     | Table           | Service, rate, frequency, visits, total     |
|                      | Location Subtotal      | Currency        | Per-location total                          |
| **Products**         | Product table          | Table           | Product, qty, unit price, tax, line total   |
|                      | Product Subtotal       | Currency        | Total product amount                        |
| **Pricing Summary**  | All summary fields     | Currency        | Subtotal, tax, discount, grand total        |
| **Terms**            | Valid Till              | Date            | Expiry date                                 |
|                      | Payment Terms          | Text            | Payment conditions                          |
|                      | Special Terms          | Text            | Additional T&C                              |
| **Attachments**      | Files                  | File List       | Downloadable attached files                 |
| **Audit**            | Created                | DateTime + User | Creation info                               |
|                      | Last Modified          | DateTime + User | Last edit info                              |
|                      | Sent                   | DateTime        | When quotation was sent                     |
|                      | Viewed                 | DateTime        | When client viewed (tracking)               |

---

## Actions

| Action              | Available When     | Behavior                                                  |
| ------------------- | ------------------ | --------------------------------------------------------- |
| Close               | Always             | Returns to Quotation Dashboard                            |
| Edit                | Status = Draft     | Opens Edit Quotation form (same as Add, pre-populated)    |
| Download PDF        | Always             | Downloads customer-facing quotation PDF                   |
| Resend              | Status = Sent      | Resends quotation to client via email/WhatsApp            |
| Convert to Contract | Status = Accepted  | Triggers Contract creation workflow in Module 18          |

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
│  │  ⚠️  DELETE QUOTATION                               │    │
│  │                                                      │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │                                                      │    │
│  │  Are you sure you want to delete this quotation?    │    │
│  │                                                      │    │
│  │  Quotation ID:    QT-2026-00142                     │    │
│  │  Client Name:     Rahul Sharma                      │    │
│  │  Type:            Service Quotation                 │    │
│  │  Amount:          ₹28,037                           │    │
│  │  Status:          Draft                             │    │
│  │                                                      │    │
│  │  ─────────────────────────────────────────────────   │    │
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

| Field            | Type      | Required    | Options/Validation                                                                      | Notes                                |
| ---------------- | --------- | ----------- | --------------------------------------------------------------------------------------- | ------------------------------------ |
| Quotation ID     | Display   | System      | Read-only                                                                               | Identifies the quotation             |
| Client Name      | Display   | System      | Read-only                                                                               | From quotation record                |
| Type             | Display   | System      | Read-only                                                                               | Service / Product / Combined         |
| Amount           | Display   | System      | Read-only                                                                               | Grand total from quotation           |
| Status           | Display   | System      | Read-only (always "Draft")                                                              | Confirms deletion eligibility        |
| Deletion Reason  | Dropdown  | Yes         | Created by Mistake / Duplicate Quotation / Client Withdrew Interest / Pricing Error / Other | Reason for deletion              |
| Specify Reason   | Text Area | Conditional | Min 10 characters, required if "Other" selected                                         | Custom reason                        |

---

## Actions

| Action         | Behavior                                                                           |
| -------------- | ---------------------------------------------------------------------------------- |
| Cancel         | Closes popup, returns to dashboard with no changes                                 |
| Confirm Delete | Permanently deletes quotation, logs deletion reason in audit, returns to dashboard |

---

## Post-Delete System Behaviors

| Trigger              | System Action                                                        |
| -------------------- | -------------------------------------------------------------------- |
| Quotation Deleted    | Remove quotation from all listings                                   |
| Linked to Lead       | Update lead record (remove quotation reference), status remains same |
| Linked to Customer   | Update customer record (remove quotation reference)                  |
| Audit Log            | Record deletion with quotation ID, user, timestamp, and reason       |
| Notification         | Notify sales manager of quotation deletion                           |
