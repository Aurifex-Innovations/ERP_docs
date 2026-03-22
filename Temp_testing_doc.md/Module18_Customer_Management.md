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


# 🎯 MODULE 19: CONTRACT MANAGEMENT

## Overview

A transactional module handling the legal and service-level agreements (SLA) for recurring (Contract) and one-time services. Uses the approved GMA sheet (Module 17) to lock in commercial terms — auto-inheriting 80% of the contract data — reducing manual entry and ensuring pricing integrity. Manages the full contract lifecycle from Draft through Active, Expiring, Expired, and Terminated statuses.

**Module Connections:**

- **Depends on:** Module 17 (GMA Management – approved GMA is the mandatory prerequisite for contract creation), Module 18 (Customer Management – customer record and sites), Module 7 (Branch Management), Module 8 (Employee / Technician Management – for technician group assignment)
- **Used by:** Module 20 (Sales Order Management – contracts trigger SO generation), Operations & Dispatch (service scheduling from contract terms)
- **Prerequisites:** An **Approved** GMA sheet (Module 17) must exist before a contract can be created. The customer and site records must be active in Module 18.

---

# 19.1 Contract Master List View

**Description:**
A paginated ledger displaying all contracts in the system. Provides status-based filtering, search, and row-level actions. Acts as the entry point for all contract operations.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         CONTRACT MANAGEMENT                                  │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                │  │
│  │                                                                        │  │
│  │ Status     : [☑ Draft ☑ Active ☑ Expiring Soon ☑ Terminated ☑ Expired]│  │
│  │ Customer   : [🔍 Search / Select ▼]                                   │  │
│  │ Branch     : [▼ All Branches ▼]                                       │  │
│  │ Date Range : [📅 From] – [📅 To]  (Contract Start / End Date)        │  │
│  │                                                                        │  │
│  │ Search: [________________________] (Contract ID / Customer / GMA ID)  │  │
│  │                                                 [Reset Filters]       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│                                                 [+ CREATE CONTRACT]          │
│                                                                              │
│  CONTRACT TABLE                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Contract ID   │Customer Name  │GMA ID   │Contract Value│Start Date│End Date││
│  │──────────────┼───────────────┼─────────┼──────────────┼──────────┼────────││
│  │CON-2026-0041 │ABC Corporation│GMA-00091│₹ 2,04,000    │01 Jan 26 │31 Dec 26││
│  │CON-2026-0039 │XYZ Hotel      │GMA-00088│₹ 1,32,000    │01 Feb 26 │31 Jan 27││
│  │CON-2025-0018 │PQR Foods      │GMA-00077│₹ 85,000      │01 Jul 25 │30 Jun 26││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Status           │Actions                                                 ││
│  │─────────────────┼────────────────────────────────────────────────────────││
│  │✅ Active         │[View] [Edit / Amend] [Terminate]                      ││
│  │🟠 Expiring Soon  │[View] [Edit / Amend] [Terminate]                      ││
│  │📁 Expired        │[View]                                                 ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   Next                              │
│                                                                              │
│  Legend: 📋 Draft  ✅ Active  🟠 Expiring Soon  ❌ Terminated  📁 Expired     │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table View Fields

| Field          | Type         | Description                                                          |
| -------------- | ------------ | -------------------------------------------------------------------- |
| Contract ID    | Text (Auto)  | System-generated unique ID (e.g., CON-2026-0041)                     |
| Customer Name  | Text         | Customer name from Module 18 Customer Master                         |
| GMA ID         | Link         | Source GMA sheet ID; clickable — navigates to Module 17 GMA detail   |
| Contract Value | Currency     | Total agreed sale value for the contract period (₹)                   |
| Start Date     | Date         | Contract commencement date                                           |
| End Date       | Date         | Contract scheduled end date                                          |
| Status         | Badge        | Draft / Active / Expiring Soon / Terminated / Expired                |
| Actions        | Button Group | [View] [Edit / Amend] [Terminate]                                    |

---

## Filters

| Filter     | Type         | Options                                                         |
| ---------- | ------------ | --------------------------------------------------------------- |
| Status     | Multi-select | Draft / Active / Expiring Soon / Terminated / Expired           |
| Customer   | Search       | Search by Customer Name / ID from Module 18                     |
| Branch     | Dropdown     | All Branches / Specific Branch (from Module 7)                  |
| Date Range | Date Range   | From – To (contract start or end date)                          |

---

## Search

Searchable by:
- Contract ID
- Customer Name
- GMA ID

---

## Actions (Table Row)

| Action           | Condition                          | Description                                               |
| ---------------- | ---------------------------------- | --------------------------------------------------------- |
| **View**         | All statuses                       | Opens the contract detail dashboard (Screen 19.3)         |
| **Edit / Amend** | Status = Draft or Active           | Opens the Edit / Amend Contract form (Screen 19.4)        |
| **Terminate**    | Status = Active or Expiring Soon   | Opens the Termination dialog (Screen 19.5)                |

---

## Form Actions

| Action               | Description                                                    |
| -------------------- | -------------------------------------------------------------- |
| **+ Create Contract**| Opens the **Add Contract Form** (Screen 19.2)                  |

> **Note:** The [+ Create Contract] button is only enabled when at least one **Approved** GMA sheet exists. If no approved GMA is available, the button is greyed out with a tooltip: *"An approved GMA is required to create a contract."*

---

================================================================================

# 19.2 Add Contract Form

**Description:**
A multi-section form to formalize a service agreement. Inherits approximately 80% of data directly from an approved GMA sheet — auto-populating customer details, sites, services, and cost breakdowns. The user configures contract-specific terms: validity period, service schedule, and payment & commercial terms.

> **Note:** An **Approved GMA** (Module 17) is the mandatory prerequisite. No contract can be created without a linked, approved GMA sheet.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Contract List]          ADD CONTRACT                             │
│                                                                              │
│  Contract ID: CON-XXXX (Auto-generated on save)                              │
│                                                                              │
│  SECTION 1: SOURCE SELECTION (FROM APPROVED GMA)                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Select Approved GMA*:  [🔍 Search GMA by ID / Customer Name ▼]       │ │
│  │                                                                         │ │
│  │  AUTO-POPULATED FROM GMA:                                               │ │
│  │  GMA ID            : GMA-00091   (Read-only)                           │ │
│  │  Customer Name     : ABC Corporation (Read-only)                       │ │
│  │  Customer ID       : CUST-00245  (Read-only)                           │ │
│  │  No. of Sites      : 2 (Read-only)                                     │ │
│  │  Total Cost (₹)    : ₹ 1,20,992 (Read-only from GMA)                  │ │
│  │  Total Sale Price   : ₹ 2,04,000 (Read-only from GMA)                  │ │
│  │  Overall GM%       : 40.7% (Read-only)                                 │ │
│  │  Branch            : Mumbai (Read-only)                                │ │
│  │                                                                         │ │
│  │  SITES INHERITED FROM GMA:                                              │ │
│  │  ┌──────────┬─────────────┬──────────────────┬──────────┬────────────┐ │ │
│  │  │ Site ID  │ Site Name   │ Address          │Area(SQFT)│ Category   │ │ │
│  │  │──────────┼─────────────┼──────────────────┼──────────┼────────────│ │ │
│  │  │SITE-00312│ Head Office │ Andheri East     │  3,500   │ Commercial │ │ │
│  │  │SITE-00313│ Warehouse   │ Bhiwandi, Thane  │  8,000   │ Industrial │ │ │
│  │  └──────────┴─────────────┴──────────────────┴──────────┴────────────┘ │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 2: CONTRACT TERMS                                                   │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Contract Duration*  : [▼ 1 Year / 2 Years / 3 Years / Custom ▼]        │ │
│  │  Start Date*         : [📅 Date Picker]                                │ │
│  │  End Date*           : [📅 Auto-calculated / Manual if Custom]         │ │
│  │  Total Sale Value (₹)*: [₹ 2,04,000] (Auto from GMA, editable)        │ │
│  │                                                                         │ │
│  │  Contract Reference  : [________________] (Optional internal reference)│ │
│  │  Renewal Type        : [▼ Auto-Renew / Manual / Non-Renewable ▼]       │ │
│  │  Legal Notes / SLA   : [___________________________________________]   │ │
│  │                        [___________________________________________]   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 3: SITE-WISE SERVICE & SCHEDULE CONFIGURATION                       │
│  (Configured based on inherited properties from GMA, editable for scheduling)│
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  SITE 1: Head Office (SITE-00312)                      [Remove Site]    │ │
│  │  ┌──────────────────────────────────────────────────────────────────┐   │ │
│  │  │ Address        : Andheri East, Mumbai                            │   │ │
│  │  │ Area / Category: 3,500 SQFT | Commercial                         │   │ │
│  │  │                                                                  │   │ │
│  │  │ [+ ADD SERVICE TO THIS SITE]                                     │   │ │
│  │  │ ┌──────────────────────────────────────────────────────────────┐ │   │ │
│  │  │ │ SERVICE 1: [▼ Cockroach Treatment ▼]         [Remove Service]│ │   │ │
│  │  │ │                                                              │ │   │ │
│  │  │ │ Contract Mode* : [▼ Contract Base ▼]                           │ │   │ │
│  │  │ │ Frequency*     : [▼ Weekly ▼] (52 visits/yr)                 │ │   │ │
│  │  │ │                                                              │ │   │ │
│  │  │ │ Preferred Days*: [☑ Wed] [☑ Sat] [☐ Mon] [☐ Tue] [☐ Thu]     │ │   │ │
│  │  │ │ Preferred Time*: [▼ Morning (8-12) ▼]                        │ │   │ │
│  │  │ │ Assigned Team : [🔍 Quick Response Team A ▼]                 │ │   │ │
│  │  │ │                                                              │ │   │ │
│  │  │ │ Service Sale Value: [₹ 55,000] (Editable - Validated vs GMA) │ │   │ │
│  │  │ └──────────────────────────────────────────────────────────────┘ │   │ │
│  │  │                                                                  │   │ │
│  │  │ TOTAL SITE 1 SALE VALUE: ₹ 55,000                                │   │ │
│  │  └──────────────────────────────────────────────────────────────────┘   │ │
│  │                                                                         │ │
│  │  SITE 2: Warehouse (SITE-00313)                        [Remove Site]    │ │
│  │  ┌──────────────────────────────────────────────────────────────────┐   │ │
│  │  │ Address        : Bhiwandi, Thane                                 │   │ │
│  │  │ Area / Category: 8,000 SQFT | Industrial                         │   │ │
│  │  │                                                                  │   │ │
│  │  │ [+ ADD SERVICE TO THIS SITE]                                     │   │ │
│  │  │ ┌──────────────────────────────────────────────────────────────┐ │   │ │
│  │  │ │ SERVICE 1: [▼ Rodent Control ▼]              [Remove Service]│ │   │ │
│  │  │ │                                                              │ │   │ │
│  │  │ │ Contract Mode* : [▼ Contract Base ▼]                           │ │   │ │
│  │  │ │ Frequency*     : [▼ Monthly ▼] (12 visits/yr)                │ │   │ │
│  │  │ │                                                              │ │   │ │
│  │  │ │ Preferred Days*: [☑ Mon] [☐ Tue] [☐ Wed] [☐ Thu] [☐ Fri]     │ │   │ │
│  │  │ │ Preferred Time*: [▼ Afternoon (12-5) ▼]                      │ │   │ │
│  │  │ │ Assigned Team* : [🔍 Quick Response Team B ▼]                 │ │   │ │
│  │  │ │                                                              │ │   │ │
│  │  │ │ Service Sale Value: [₹ 1,54,000] (Editable - Validated vs GMA) │ │   │ │
│  │  │ └──────────────────────────────────────────────────────────────┘ │   │ │
│  │  │                                                                  │   │ │
│  │  │ TOTAL SITE 2 SALE VALUE: ₹ 1,54,000                              │   │ │
│  │  └──────────────────────────────────────────────────────────────────┘   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 4: PAYMENT & COMMERCIAL TERMS                                       │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Payment Schedule Type*: [▼ Select Payment Schedule ▼]                 │ │
│  │      • 100% Advance                                                    │ │
│  │      • Monthly Post-paid                                               │ │
│  │      • Quarterly Post-paid                                             │ │
│  │      • Half-Yearly Post-paid                                           │ │
│  │      • Milestone-based                                                 │ │
│  │      • Custom                                                          │ │
│  │      * For Custom set one more field                                     │ │
│  │  IF 100% ADVANCE:                                                       │ │
│  │  Payment Due Date*   : [📅 Date Picker]  (Before or on Start Date)     │ │
│  │  Amount (₹)          : ₹ 2,04,000 (Full contract value)               │ │
│  │                                                                         │ │
│  │  IF QUARTERLY POST-PAID:                                                │ │
│  │  ┌──────────────┬──────────────┬──────────────┬──────────────┐         │ │
│  │  │ Quarter      │ Period       │ Amount (₹)   │ Due Date     │         │ │
│  │  │──────────────┼──────────────┼──────────────┼──────────────│         │ │
│  │  │ Q1           │ Jan – Mar    │ ₹ 51,000     │ 31 Mar 2026  │         │ │
│  │  │ Q2           │ Apr – Jun    │ ₹ 51,000     │ 30 Jun 2026  │         │ │
│  │  │ Q3           │ Jul – Sep    │ ₹ 51,000     │ 30 Sep 2026  │         │ │
│  │  │ Q4           │ Oct – Dec    │ ₹ 51,000     │ 31 Dec 2026  │         │ │
│  │  └──────────────┴──────────────┴──────────────┴──────────────┘         │ │
│  │                                                                         │ │
│  │  IF MILESTONE-BASED(Editable):                                                    │ │
│  │  [+ Add Milestone]                                                     │ │
│  │  ┌──────────────┬─────────────────┬──────────┬──────────────┐          │ │
│  │  │ Milestone    │ Description     │Amount (₹)│ Due Date     │          │ │
│  │  │──────────────┼─────────────────┼──────────┼──────────────│          │ │
│  │  │ M1           │ Contract Kick-off│₹ 60,000 │ 15 Jan 2026  │          │ │
│  │  │ M2           │ Mid-year Review │₹ 72,000  │ 01 Jul 2026  │          │ │
│  │  │ M3           │ Year-end Close  │₹ 72,000  │ 31 Dec 2026  │          │ │
│  │  └──────────────┴─────────────────┴──────────┴──────────────┘          │ │
│  │  Validation: Sum of milestones = Total Sale Value ✓                    │ │
│  │                                                                         │ │
│  │  Invoicing Frequency*  : [▼ Monthly / Quarterly / Half-Yearly /        │ │
│  │                            Annually / On Milestone ▼/Service completion]                  │ │
│  │                                                                         │ │
│  │  Legal SLA Remarks     : [___________________________________________] │ │
│  │                          [___________________________________________] │ │
│  │  (e.g., "Response within 4 hours for emergency pest issues")           │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [SAVE AS DRAFT]         [ACTIVATE CONTRACT]         [CANCEL]           │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Section 1: Source Selection Fields

| Field              | Type            | Required | Options / Validation                                                     | Notes                                          |
| ------------------ | --------------- | -------- | ------------------------------------------------------------------------ | ---------------------------------------------- |
| Select Approved GMA| Search Dropdown | Yes      | Only GMA sheets with Status = **Approved** appear                        | Primary data source for the contract           |
| GMA ID             | Auto-filled     | System   | Read-only                                                                | Linked GMA reference                           |
| Customer Name      | Auto-filled     | System   | Read-only; populated from GMA → Module 18                                | Customer whose contract is being created       |
| Customer ID        | Auto-filled     | System   | Read-only                                                                | Unique customer reference                      |
| No. of Sites       | Auto-filled     | System   | Read-only; count of sites in the GMA sheet                               | Summary count                                  |
| Total Cost (₹)     | Auto-filled     | System   | Read-only; sum of all site costs from GMA                                | Internal cost reference                        |
| Total Sale Price (₹)| Auto-filled    | System   | Read-only; sum of all site proposed prices from GMA                      | Commercial price reference                     |
| Overall GM%        | Auto-filled     | System   | Read-only; auto-calculated from GMA                                      | Gross Margin percentage                        |
| Branch             | Auto-filled     | System   | Read-only; branch from GMA                                               | Servicing branch                               |
| Sites Grid         | Auto-filled     | System   | Read-only; all sites inherited from GMA                                  | Site ID, Name, Address, SQFT, Category         |

> **Note:** Once a GMA is selected, all the above fields are auto-populated and **cannot be modified** in this form. To change the underlying commercial terms, the GMA itself must be revised and re-approved in Module 17.

---

## Section 2: Contract Terms Fields

| Field              | Type      | Required    | Options / Validation                                                    | Notes                                            |
| ------------------ | --------- | ----------- | ----------------------------------------------------------------------- | ------------------------------------------------ |
| Contract Duration  | Dropdown  | Yes         | 6 Months / 1 Year / 2 Years / 3 Years / Custom                         | Determines the End Date auto-calculation         |
| Start Date         | Date      | Yes         | Must be ≥ Today                                                         | Contract commencement                            |
| End Date           | Date      | Yes         | Auto-calculated from Start Date + Duration; manual if Custom selected   | Must be > Start Date                             |
| Total Sale Value (₹)| Currency | Yes         | Pre-filled from GMA Total Sale Price; editable                          | If modified, triggers re-approval workflow        |
| Contract Reference | Text      | No          | Max 50 chars, alphanumeric                                              | Optional internal/legal reference number         |
| Renewal Type       | Dropdown  | No          | Auto-Renew / Manual / Non-Renewable                                     | Determines behaviour at contract expiry          |
| Legal Notes / SLA  | Text Area | No          | Max 1000 characters                                                     | Free-text for SLA terms, special conditions       |

> **Note:** If **Total Sale Value** is modified from the GMA's original value (increase or decrease), the system flags this as a **commercial amendment** and may trigger a re-approval workflow based on the variance percentage. A variance > 10% requires Sales Manager approval.

---

## Section 3: Site-Wise Service & Schedule Configuration Fields

This section breaks down the contract deliverables on a per-site basis. Each site inherited from the GMA appears as a block, where multiple services can be specifically scheduled.

### Site Information

| Field              | Type           | Required | Options / Validation                                 | Notes                                        |
| ------------------ | -------------- | -------- | ---------------------------------------------------- | -------------------------------------------- |
| Site ID & Name     | Display        | System   | Read-only; inherited from GMA                        | Identifies the site block                    |
| Address & Area     | Display        | System   | Read-only                                            | Context for the service                      |
| Total Site Sale Value | Calculation   | System   | Sum of all 'Service Sale Values' inside this site    | Displayed at the footer of the site block    |

---

### Service Configuration (Inside Each Site)

> Users click **[+ ADD SERVICE TO THIS SITE]** to nest multiple service configurations within a single physical location block. Each service schedule is managed independently.

| Field              | Type           | Required | Options / Validation                                 | Notes                                        |
| ------------------ | -------------- | -------- | ---------------------------------------------------- | -------------------------------------------- |
| Service Type       | Dropdown       | Yes      | Inherited from GMA; editable if multi-service        | The pest control treatment being rendered    |
| Contract Mode      | Dropdown       | Yes      | Contract Base / One-Time                             | Determines frequency applicability           |
| Frequency          | Dropdown       | Yes      | Weekly / Fortnightly / Monthly / Quarterly / Custom  | Inherits GMA frequency by default            |
| Preferred Days     | Multi-checkbox | Yes      | Mon – Sun                                            | Sets the recurring schedule pattern          |
| Preferred Time     | Dropdown       | Yes      | Morning (8–12) / Afternoon (12–5) / Evening (5–8)    | Time window for visits                       |
| Assigned Team      | Search Dropdown| Yes      | Active technician teams from Module 8                | Team responsible for this specific treatment |
| Service Sale Value (₹)| Currency       | Yes      | Inherited from GMA proposed price for this service   | Validated against Total Contract Value       |

---

## Section 4: Payment & Commercial Terms Fields

| Field                  | Type     | Required | Options / Validation                                                              | Notes                                            |
| ---------------------- | -------- | -------- | --------------------------------------------------------------------------------- | ------------------------------------------------ |
| Payment Schedule Type  | Dropdown | Yes      | 100% Advance / Monthly Post-paid / Quarterly Post-paid / Half-Yearly / Milestone-based / Custom | Determines payment grid structure; * For Custom set one more field  |
| Payment Due Date       | Date     | Cond.    | Required if 100% Advance; must be ≤ Start Date                                    | Single payment date                              |
| Payment Grid           | Table    | Cond.    | Auto-generated for Quarterly/Monthly/Half-Yearly; manual for Milestone(Editable)/Custom     | Must sum to Total Sale Value                     |
| Invoicing Frequency    | Dropdown | Yes      | Monthly / Quarterly / Half-Yearly / Annually / On Milestone / Service completion      | Determines invoice generation cadence            |
| Legal SLA Remarks      | Text Area| No       | Max 1000 characters                                                               | Service-level commitments, penalties, terms       |

### Payment Grid Fields (for Quarterly / Milestone / Custom)

| Field          | Type     | Required | Validation                               | Notes                              |
| -------------- | -------- | -------- | ---------------------------------------- | ---------------------------------- |
| Period / Label | Text     | Yes      | Auto-generated for periodic; manual for milestone | e.g., "Q1", "M1 – Kick-off"  |
| Description    | Text     | No       | Max 100 chars                            | Optional description               |
| Amount (₹)     | Currency | Yes      | Must be > 0                              | Payment amount for this period     |
| Due Date       | Date     | Yes      | Must be within contract Start–End range  | When payment is due                |

> **Validation:** Sum of all payment grid amounts **must equal** the Total Sale Value. The system shows a validation error if there is a mismatch.

---

## Form Actions

| Button               | Description                                                                      |
| -------------------- | -------------------------------------------------------------------------------- |
| **Save as Draft**    | Saves the contract without activating. Contract appears in list with Draft status.|
| **Activate Contract**| Validates all sections, creates the contract as Active, and triggers initial SO generation for the first billing period.|
| **Cancel**           | Discards all entries and returns to the Contract Master List.                     |

---

## Validation Rules

| Validation                          | Rule                                                                  |
| ----------------------------------- | --------------------------------------------------------------------- |
| Approved GMA Required               | Must select a valid GMA with Status = Approved                        |
| Start Date ≥ Today                  | Contract cannot start in the past                                     |
| End Date > Start Date               | End date must be after start date                                     |
| Total Sale Value > 0                | Contract value must be positive                                       |
| Site Values Equal Total             | Sum of all 'Site Sale Values' must equal 'Total Sale Value'           |
| Payment Sum = Total Value           | Sum of all payment amounts must equal the Total Sale Value             |
| Technician Group Required           | Every site/service block must have an assigned Technician Team        |
| Service Frequency Required          | Every Contract service block must have a valid service frequency           |
| No Duplicate GMA Contract           | Same GMA cannot be used for more than one Active contract             |
| Value Variance Check                | If Sale Value differs from GMA by > 10%, triggers re-approval alert   |

---

## System Behaviors on Save

| Trigger                          | System Action                                                                     |
| -------------------------------- | --------------------------------------------------------------------------------- |
| Approved GMA selected            | Auto-populates all Section 1 fields from GMA + Module 18 customer data           |
| Duration selected                | Auto-calculates End Date from Start Date + Duration selection                     |
| Activate Contract clicked        | Status → Active; Contract ID generated; linked to customer in Module 18           |
| First SO triggered               | System auto-generates the first Sales Order (Module 20) based on payment schedule|
| GMA marked as consumed           | The selected GMA is flagged as "Contract Created" to prevent reuse               |
| Contract activated               | Appears in Module 18 → Tab 2 (Contract Logs) for the linked customer            |

---

================================================================================

# 19.3 View Contract Details

**Description:**
A read-only dashboard displaying the full breakdown of an active or historical contract. Split into two logical tabs — Contract Terms, Scope & Sites, and the Sales Order Schedule (Billing Log).

---

## Screen Layout (Header)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Contract List]                 [Edit / Amend]  [Terminate]      │
│                                                                              │
│  CONTRACT: CON-2026-0041                    Status: ✅ ACTIVE                 │
│  Customer: ABC Corporation (CUST-00245)     │  Branch: Mumbai                │
│  GMA Ref : GMA-00091                        │  No. of Sites: 2               │
│  Start   : 01 Jan 2026   │  End: 31 Dec 2026  │  Duration: 1 Year           │
│  Value   : ₹ 2,04,000    │  GM%: 40.7%                                      │
│                                                                              │
│  [Tab 1: Contract Terms, Scope & Sites ●]  [Tab 2: Sales Order Schedule]    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

================================================================================

# 19.3.1 Tab 1: Contract Terms, Scope & Sites

**Description:**
Displays the complete contract configuration — commercial terms, payment schedule, SLA notes, and the sites & services grid showing every physical location covered under this agreement along with pest types and service frequency.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Tab 1: Contract Terms, Scope & Sites ●] [Tab 2: Sales Order Schedule]    │
│                                                                              │
│  ─── SECTION 1: SOURCE INFO (READ-ONLY) ──────────────────────────────────  │
│  GMA ID         : GMA-00091               Branch      : Mumbai              │
│  Customer Name  : ABC Corporation         Customer ID : CUST-00245          │
│  No. of Sites   : 2                       Total Cost  : ₹ 1,20,992          │
│  Total Sale Price: ₹ 2,04,000              Overall GM% : 40.7%               │
│                                                                              │
│  ─── SECTION 2: CONTRACT TERMS (COMMERCIAL) ──────────────────────────────  │
│  Contract ID    : CON-2026-0041            Duration    : 1 Year             │
│  Start Date     : 01 Jan 2026             End Date    : 31 Dec 2026         │
│  Total Value    : ₹ 2,04,000              Renewal Type: Manual              │
│  Contract Ref   : REF-INT-2026-041        Payment Freq: Quarterly Post-paid │
│                                                                              │
│  ─── SECTION 3: SCOPE OF WORK (SITES & SERVICES) ─────────────────────────  │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ ▼ SITE 1: Head Office [SITE-312] | Andheri East, Mumbai | 3,500 SQFT     ││
│  │   Site Sale Value: ₹ 55,000 | Site Cost: ₹ 29,800 | Site GM%: 45.8%      ││
│  │                                                                          ││
│  │   ├─ SERVICE 1: Cockroach Treatment | Contract Base | Sale Val: ₹ 35,000 ││
│  │   │  Frequency: Weekly (52/yr) | Tech: Team Alpha                        ││
│  │   │  Schedule : Wed, Sat [Morning (8-12)]                                ││
│  │   │                                                                      ││
│  │   └─ SERVICE 2: Termite Control | Contract Base | Sale Val: ₹ 20,000     ││
│  │      Frequency: Quarterly (4/yr) | Tech: Team Beta                       ││
│  │      Schedule : 1st Mon [Afternoon (12-5)]                               ││
│  │                                                                          ││
│  │ ▼ SITE 2: Warehouse [SITE-313] | Bhiwandi, Thane | 8,000 SQFT            ││
│  │   Site Sale Value: ₹ 1,54,000 | Site Cost: ₹ 89,782 | Site GM%: 41.7%    ││
│  │                                                                          ││
│  │   └─ SERVICE 1: Rodent Control | Contract Base | Sale Val: ₹ 1,54,000    ││
│  │      Frequency: Monthly (12/yr) | Tech: Team Beta                        ││
│  │      Schedule : Mon [Afternoon (12-5)]                                   ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ─── SECTION 4: PAYMENT & COMMERCIAL TERMS ───────────────────────────────  │
│  Payment Type: Quarterly Post-paid         Invoicing Freq: Quarterly        │
│  ┌──────────┬──────────────┬──────────────┬──────────────┬──────────┐       │
│  │ Period   │ Duration     │ Amount (₹)   │ Due Date     │ Status   │       │
│  │──────────┼──────────────┼──────────────┼──────────────┼──────────│       │
│  │ Q1       │ Jan – Mar    │ ₹ 51,000     │ 31 Mar 2026  │ ✅ Paid  │       │
│  │ Q2       │ Apr – Jun    │ ₹ 51,000     │ 30 Jun 2026  │ 🟡 Due   │       │
│  │ Q3       │ Jul – Sep    │ ₹ 51,000     │ 30 Sep 2026  │ ⏳ Upcoming│     │
│  │ Q4       │ Oct – Dec    │ ₹ 51,000     │ 31 Dec 2026  │ ⏳ Upcoming│     │
│  │ └────────┴──────────────┴──────────────┴──────────────┴──────────┘       │
│                                                                              │
│  Legal SLA Notes: "Response within 4 hours for emergency pest issues.       │
│   Re-treatment guaranteed within 48 hours if infestation recurrence."       │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Contract Terms Fields (Read-Only)

| Field           | Type     | Description                                              |
| --------------- | -------- | -------------------------------------------------------- |
| Contract ID     | Display  | Unique contract identifier                               |
| GMA Ref         | Display  | Linked GMA ID (Read-only source)                         |
| Start Date      | Date     | Contract commencement                                    |
| End Date        | Date     | Contract expiry                                          |
| Duration        | Display  | e.g., 1 Year, 6 Months                                  |
| No. of Sites    | Display  | Total physical sites covered                            |
| Total Value (₹) | Currency | Total agreed sale value                                  |
| Total Cost (₹)  | Currency | Estimated internal cost from GMA                        |
| Overall GM%     | Display  | Gross margin percentage                                  |
| Renewal Type    | Display  | Auto-Renew / Manual / Non-Renewable                      |
| Contract Ref    | Display  | Optional internal reference number                       |

---

## Payment Schedule Fields (Read-Only)

| Field            | Type     | Description                                             |
| ---------------- | -------- | ------------------------------------------------------- |
| Payment Type     | Display  | 100% Advance / Monthly / Quarterly / Milestone / Custom |
| Invoicing Freq.  | Display  | Monthly / Quarterly / Annually / Service Completion     |
| Period / Label   | Display  | Quarter / Month / Milestone identifier                  |
| Duration         | Display  | Date range for the period                               |
| Amount (₹)       | Currency | Installment amount                                      |
| Due Date         | Date     | Payment due date                                        |
| Payment Status   | Badge    | Paid / Due / Overdue / Upcoming                          |

---

## Scope of Work (Sites & Services) Fields (Read-Only)

| Field            | Type     | Description                                              |
| ---------------- | -------- | -------------------------------------------------------- |
| Site ID & Name   | Display  | Unique site reference and friendly label                 |
| Address & Area   | Display  | Physical address and SQFT                                |
| Site Sale/Cost/GM| Display  | Financial summary rolled up for the entire site          |
| Service Name     | Display  | Pest / service category mapped under the site            |
| Contract Mode    | Display  | Contract Base / One-Time                                 |
| Service Sale Val | Currency | Assigned revenue value for this specific service line    |
| Frequency        | Display  | Service visit frequency (e.g. Weekly)                    |
| Technician Group | Display  | Assigned team from Module 8                              |
| Schedule Details | Display  | Preferred days and time slot for this recurring service  |

> **Note:** All data in this tab is derived from the **GMA sheet (Module 17)** and the **contract creation form (19.2)**. Modifications can only be made via the **Edit / Amend** form (19.4).

---

================================================================================

# 19.3.2 Tab 2: Sales Order Schedule (Billing Log)-- it is similar to Module 20 table view.

**Description:**
A chronological grid showing all Sales Orders linked to this contract — both auto-generated (from the payment schedule) and manually created. Tracks fulfilment vs. the original agreement, providing a billing audit trail.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Tab 1: Contract Terms, Scope & Sites] [Tab 2: Sales Order Schedule ●]    │
│                                                                              │
│  SALES ORDER SCHEDULE                                                        │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │SO Number    │SO Date    │Period     │SO Value (₹)│SO Status │Svc Status  ││
│  │─────────────┼───────────┼───────────┼────────────┼──────────┼────────────││
│  │SO-2026-0042 │01 Jan 2026│Q1 Jan–Mar │₹ 51,000    │✅ Fulfilled│✅ Completed││
│  │SO-2026-0087 │01 Apr 2026│Q2 Apr–Jun │₹ 51,000    │✅ Open     │🕐 Scheduled││
│  │SO-2026-0XXX │01 Jul 2026│Q3 Jul–Sep │₹ 51,000    │📋 Draft   │⏳ Pending  ││
│  │SO-2026-0XXX │01 Oct 2026│Q4 Oct–Dec │₹ 51,000    │📋 Draft   │⏳ Pending  ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  BILLING SUMMARY                                                             │
│  Total Contract Value  : ₹ 2,04,000                                         │
│  Total SO Value Raised : ₹ 1,02,000  (Q1 + Q2)                              │
│  Remaining to Invoice  : ₹ 1,02,000  (Q3 + Q4)                              │
│  Fulfillment Progress  : ██████████░░░░░░░░░░  50%                          │
│                                                                              │
│  Pagination: Previous  1  2  Next                                            │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## SO Schedule Grid Fields

| Field          | Type    | Description                                                            |
| -------------- | ------- | ---------------------------------------------------------------------- |
| SO Number      | Link    | Sales Order ID; clickable — navigates to Module 20 SO Detail           |
| SO Date        | Date    | Date the SO was generated                                              |
| Period         | Text    | Billing period / milestone label this SO covers                        |
| SO Value (₹)   | Currency| Value of this Sales Order                                              |
| SO Status      | Badge   | Draft / Open / Fulfilled / Billed / Cancelled                          |
| Service Status | Badge   | Scheduled / In Progress / Completed / Pending                          |

---

## Billing Summary Fields (Auto-Calculated)

| Field                | Type     | Description                                          |
| -------------------- | -------- | ---------------------------------------------------- |
| Total Contract Value | Currency | Total agreed sale value from Section 2               |
| Total SO Value Raised| Currency | Sum of all generated SO values                       |
| Remaining to Invoice | Currency | Contract Value – Total SO Value Raised               |
| Fulfillment Progress | Progress | Percentage bar of SOs Fulfilled vs. Total expected   |

> **Note:** Sales Orders are auto-generated by the system based on the **Payment Schedule** configured in Section 4 of the contract. Manual SOs can also be created from **Module 20** and linked to this contract. The billing log provides a single view to reconcile what was agreed vs. what has been invoiced.

---

================================================================================

# 19.4 Edit / Amend Contract Form

**Description:**
An interface to handle mid-cycle changes to an active contract. Pre-filled with existing contract data. Supports adding new sites, upgrading services, and adjusting commercial values. Significant amendments (e.g., value change > 10%) may trigger a re-approval workflow.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Contract Detail]   EDIT / AMEND CONTRACT   CON-2026-0041       │
│                                                                              │
│  SECTION 1: CONTRACT HEADER (Read-Only)                                      │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Contract ID    : CON-2026-0041    Status    : ✅ Active                │ │
│  │  Customer       : ABC Corporation  GMA Ref   : GMA-00091               │ │
│  │  No. of Sites   : 2                Total Cost: ₹ 1,20,992              │ │
│  │  Original Value : ₹ 2,04,000      Original Duration: 1 Year            │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 2: AMEND CONTRACT TERMS (Editable)                                  │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  End Date           : [📅 31 Dec 2026 — Editable]                       │ │
│  │  Revised Sale Value*: [₹ 2,04,000_____] (Editable — triggers review)   │ │
│  │  Renewal Type       : [▼ Manual ▼]                                      │ │
│  │  Legal Notes / SLA  : [___________________________________________]    │ │
│  │                                                                         │ │
│  │  ⚠️  Value Change: If Revised Value differs from Original by > 10%,    │ │
│  │  the amendment will require Sales Manager approval.                     │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 3: AMEND SCOPE OF WORK (SITES & SERVICES)                           │ │
│  (Existing sites/services are pre-filled; all scheduling fields are editable)│ │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  [+ ADD NEW SITE TO CONTRACT]                                         │ │
│  │                                                                         │ │
│  │  SITE 1: Head Office (SITE-00312)                      [Remove Site]    │ │
│  │  ┌──────────────────────────────────────────────────────────────────┐   │ │
│  │  │ Address        : Andheri East, Mumbai                            │   │ │
│  │  │ Area / Category: 3,500 SQFT | Commercial                         │   │ │
│  │  │                                                                  │   │ │
│  │  │ [+ ADD SERVICE TO THIS SITE]                                     │   │ │
│  │  │ ┌──────────────────────────────────────────────────────────────┐ │   │ │
│  │  │ │ SERVICE 1: [▼ Cockroach Treatment ▼]         [Remove Service]│ │   │ │
│  │  │ │                                                              │ │   │ │
│  │  │ │ Contract Mode  : [▼ Contract Base ▼]                           │ │   │ │
│  │  │ │ Frequency*     : [▼ Weekly ▼] (52 visits/yr)                 │ │   │ │
│  │  │ │                                                              │ │   │ │
│  │  │ │ Preferred Days*: [☑ Wed] [☑ Sat] [☐ Mon] [☐ Tue] [☐ Thu]     │ │   │ │
│  │  │ │ Preferred Time*: [▼ Morning (8-12) ▼]                        │ │   │ │
│  │  │ │ Assigned Team  : [🔍 Quick Response Team A ▼]                 │ │   │ │
│  │  │ │                                                              │ │   │ │
│  │  │ │ Service Sale Value: [₹ 55,000] (Editable)                    │ │   │ │
│  │  │ └──────────────────────────────────────────────────────────────┘ │   │ │
│  │  │                                                                  │   │ │
│  │  │ TOTAL SITE 1 SALE VALUE: ₹ 55,000                                │   │ │
│  │  └──────────────────────────────────────────────────────────────────┘   │ │
│  │                                                                         │ │
│  │  SITE 2: Warehouse (SITE-00313)                        [Remove Site]    │ │
│  │  ┌──────────────────────────────────────────────────────────────────┐   │ │
│  │  │ ... (Content follows the same nested structure as above)          │   │ │
│  │  └──────────────────────────────────────────────────────────────────┘   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 4: AMEND PAYMENT SCHEDULE (Editable if Value Changes)               │ │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Revised Payment Type: [▼ Quarterly Post-paid ▼]                        │ │
│  │  ┌──────────┬──────────────┬──────────────┬──────────────┐              │ │
│  │  │ Period   │ Duration     │ Amount (₹)   │ Due Date     │              │ │
│  │  │──────────┼──────────────┼──────────────┼──────────────│              │ │
│  │  │ Q1       │ Jan – Mar    │ ₹ 51,000     │ 31 Mar 2026  │ (Paid)       │ │
│  │  │ Q2       │ Apr – Jun    │ [₹ 55,000__] │ [30 Jun 2026]│ (Revised)    │ │
│  │  │ Q3       │ Jul – Sep    │ [₹ 55,000__] │ [30 Sep 2026]│ (Pending)    │ │
│  │  │ Q4       │ Oct – Dec    │ [₹ 55,000__] │ [31 Dec 2026]│ (Pending)    │ │
│  │  └──────────┴──────────────┴──────────────┴──────────────┘              │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  AMENDMENT REASON                                                            │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Reason for Amendment*:  [▼ Select Reason ▼]                           │ │
│  │      • Site Addition           • Service Upgrade                       │ │
│  │      • Value Adjustment        • Schedule Change                       │ │
│  │      • SLA Modification        • Other                                 │ │
│  │                                                                         │ │
│  │  Remarks: [___________________________________________]                │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [SAVE AMENDMENT]              [CANCEL]                                 │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Amendment Section Details

### Section 4: Amend Payment Schedule
*   **Recalculation Trigger**: This section becomes mandatory if the **Revised Sale Value** or **End Date** is modified.
*   **Historical Data**: Installments already marked as **Paid** (e.g., Q1 in the ASCII above) are locked. Only future/unpaid installments can be revised.
*   **Balance Validation**: The sum of all revised installments plus already paid ones must exactly equal the `Revised Sale Value`.

---

## Editable vs. Locked Fields

| Field                   | Editable? | Reason for Lock / Edit                                                                 |
| ----------------------- | --------- | -------------------------------------------------------------------------------------- |
| Contract ID             | ❌ Locked  | System-generated; acts as a unique transaction identifier for the entire lifecycle.     |
| Customer / GMA Ref      | ❌ Locked  | The contract is legally bound to the specific customer and approved GMA source document. |
| Original Value          | ❌ Locked  | Kept as a baseline reference to calculate variance and trigger approval workflows.      |
| Start Date              | ❌ Locked  | Contract has already commenced; backdating or shifting start date is not permitted.   |
| Renewal Type            | ✅ Yes     | Renewal strategy can be adjusted anytime before contract expiry.                        |
| End Date                | ✅ Yes     | Allows for contract extensions or mid-cycle adjustments to the duration.                |
| Service Selection       | ✅ Yes     | Dynamic scope management; allows upgrading or downgrading services per site.            |
| Add/Remove Site         | ✅ Yes     | Accommodates physical expansion or decommissioning of customer locations.             |
| Revised Sale Value      | ✅ Yes     | Mandatory for financial amendments; must stay positive and align with total scope.      |
| Service Frequency       | ✅ Yes     | Operational flexibility to change visit cadence (e.g., Monthly to Weekly).              |
| Service Days / Time     | ✅ Yes     | Client-driven schedule adjustments for technician visits.                              |
| Technician Group        | ✅ Yes     | Resource management; allows reassignment of teams for better service delivery.         |
| Payment Schedule        | ✅ Yes     | Must be recalculated if total contract value or duration changes.                      |
| Amendment Reason        | ✅ Required| Essential for audit trail and tracking the "Why" behind legal/financial changes.       |

---

## Validation Rules

| Validation                         | Rule                                                              |
| ---------------------------------- | ----------------------------------------------------------------- |
| End Date > Today                   | Cannot set End Date to past or today                              |
| Revised Value > 0                  | Sum of all service sale values must be positive                   |
| Value Variance Check               | If |Revised – Original| > 10% of Original, triggers re-approval  |
| Site / Service Selection           | Must have at least one active site and service                    |
| Service Sale Value Alignment        | Individual service values must sum to the revised contract total  |
| Amendment Reason Required          | Must select a reason from the dropdown                            |
| Remarks Required (if Other)        | Free-text remark mandatory for "Other" reason                     |

---

## Audit Log Behaviour

Every amendment triggers an **audit log entry** recording:

| Log Field         | Value                                             |
| ----------------- | ------------------------------------------------- |
| Amended By        | Logged-in user ID and name                        |
| Amended On        | Timestamp                                         |
| Amendment Type    | Selected reason (Site Addition, Value Adjustment, etc.) |
| Fields Changed    | List of changed fields with old → new values      |
| Approval Required | Yes / No (based on variance threshold)            |

---

## Form Actions

| Button              | Description                                                                     |
| ------------------- | ------------------------------------------------------------------------------- |
| **Save Amendment**  | Validates changes, saves the amendment, records audit log. Triggers re-approval if value variance > 10%. |
| **Cancel**          | Discards changes and returns to the Contract Detail view.                       |

---

================================================================================

# 19.5 Delete (Terminate) Contract

**Description:**
An early closure mechanism for terminating an active contract before its scheduled end date. Changes the contract status to **Terminated** and halts all future Sales Order auto-generation. The contract record and its history are preserved for audit purposes — hard deletion is not permitted.

---

## Screen Layout

```
┌───────────────────────────────────────────────────────┐
│           TERMINATE CONTRACT                           │
│                                                        │
│  Contract    : CON-2026-0041                           │
│  Customer    : ABC Corporation (CUST-00245)            │
│  Status      : ✅ Active                                │
│  Period      : 01 Jan 2026 – 31 Dec 2026               │
│  Value       : ₹ 2,04,000                              │
│                                                        │
│  ⚠️  WARNING: Terminating this contract will:          │
│  • Stop all future Sales Order generation              │
│  • Halt service scheduling for linked sites            │
│  • Mark the contract as permanently Terminated         │
│                                                        │
│  ELIGIBILITY CHECK:                                    │
│  Open Sales Orders  : ⚠️  1 Open SO (SO-2026-0087)     │
│    Action: Open SOs will be auto-cancelled on          │
│    termination or must be manually resolved.           │
│                                                        │
│  Effective Closure Date*:                              │
│  [📅 Date Picker]  (Must be ≥ Today, ≤ End Date)       │
│                                                        │
│  Reason Code*:                                         │
│  [▼ Select Reason ▼]                                   │
│      • Customer Relocated                              │
│      • Payment Default                                 │
│      • Service Dissatisfaction                         │
│      • Mutual Agreement                                │
│      • Business Closure                                │
│      • Other                                           │
│                                                        │
│  Additional Remarks:                                   │
│  [________________________________________]            │
│  [________________________________________]            │
│                                                        │
│  [CONFIRM TERMINATION]        [CANCEL]                 │
└───────────────────────────────────────────────────────┘
```

---

## Termination Fields

| Field                    | Type     | Required | Options / Validation                                                          |
| ------------------------ | -------- | -------- | ----------------------------------------------------------------------------- |
| Effective Closure Date   | Date     | Yes      | Must be ≥ Today and ≤ Original End Date                                       |
| Reason Code              | Dropdown | Yes      | Customer Relocated / Payment Default / Service Dissatisfaction / Mutual Agreement / Business Closure / Other |
| Additional Remarks       | Text Area| No       | Max 500 characters; required if Reason = Other                                |

---

## Validation Rules

| Validation                    | Rule                                                                   |
| ----------------------------- | ---------------------------------------------------------------------- |
| Closure Date ≥ Today          | Cannot backdate the termination                                        |
| Closure Date ≤ End Date       | Cannot terminate beyond the original contract end                      |
| Reason Code Required          | Must select a reason from the dropdown                                 |
| Remarks (if Other)            | Mandatory free-text when "Other" is selected                           |
| Open SO Resolution            | System warns about open SOs; user must acknowledge or resolve          |

---

## System Behaviour on Termination

| Trigger                        | System Action                                                              |
| ------------------------------ | -------------------------------------------------------------------------- |
| Confirm Termination clicked    | Contract status set to **Terminated**                                       |
| Effective Closure Date applied | All future scheduled SOs after this date are cancelled or prevented         |
| Open SOs                       | Existing Open SOs can be auto-cancelled or left for manual resolution       |
| Service scheduling halted      | Operations notified to stop dispatching for this contract's sites           |
| Future SO generation stopped   | System cron no longer creates SOs for this contract                         |
| Audit log                      | Termination event logged with Reason, Closure Date, user, and timestamp     |
| Module 18 updated              | Contract status change reflected in Customer's Tab 2 (Contract Logs)        |

---

================================================================================

## RBAC – Role-Based Access Control

| Role              | List View (19.1)         | Create Contract (19.2) | View Details (19.3) | Edit / Amend (19.4) | Terminate (19.5) |
| ----------------- | ------------------------ | ---------------------- | ------------------- | ------------------- | ---------------- |
| Sales Person      | View (own branch)        | ✅                      | ✅                   | ❌                   | ❌                |
| Sales Manager     | View (own branch / team) | ✅                      | ✅                   | ✅                   | ✅                |
| Branch Manager    | View (own branch)        | ✅                      | ✅                   | ✅                   | ✅                |
| Head Ops / Admin  | View (all branches)      | ✅                      | ✅                   | ✅                   | ✅                |
| Finance Auditor   | View (all – read-only)   | ❌                      | ✅                   | ❌                   | ❌                |

---

================================================================================

## Navigation Flow Summary

```
MODULE 19: CONTRACT MANAGEMENT
│
├── 19.1 Contract Master List
│   ├── [+ Create Contract] → 19.2 Add Contract Form
│   │     ├── Select Approved GMA → Auto-populate from Module 17 + Module 18
│   │     ├── [Save as Draft]      → Contract created (Draft)
│   │     └── [Activate Contract]  → ✅ Contract created (Active)
│   │           ├── GMA marked as consumed (no reuse)
│   │           ├── First SO auto-generated (Module 20)
│   │           └── Appears in Module 18 → Tab 2 (Contract Logs)
│   │
│   ├── [View] → 19.3 View Contract Details
│   │     ├── Tab 1: Contract Terms, Scope & Sites
│   │     │     ├── Payment Schedule with status tracking
│   │     │     └── Sites & Services Grid (from GMA)
│   │     └── Tab 2: Sales Order Schedule (Billing Log)
│   │           └── [SO Number link] → Module 20 Sales Order Detail
│   │
│   ├── [Edit / Amend] → 19.4 Edit / Amend Contract Form
│   │     ├── Modify terms, schedule, technician assignment
│   │     ├── [+ Add Site] → Add site from customer's Module 18 sites
│   │     ├── Value variance > 10% → Re-approval required
│   │     └── Amendment audit log recorded
│   │
│   └── [Terminate] → 19.5 Termination Dialog
│         ├── Effective Closure Date + Reason Code
│         ├── Future SOs stopped
│         └── Contract status → Terminated
```

---

================================================================================

## Module Dependency Map

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                                                                                  │
│   MODULE 17             MODULE 18             MODULE 7            MODULE 8       │
│   GMA Management        Customer Management   Branch Mgmt        Employee Mgmt  │
│   ┌──────────┐         ┌──────────┐          ┌──────────┐       ┌──────────┐   │
│   │ Approved │         │ Customer │          │ Branch   │       │Technician│   │
│   │ GMA Sheet│         │ ID, Name,│          │ list for │       │ Teams,   │   │
│   │ (GM%,    │         │ Sites,   │          │ dropdown │       │ Roles,   │   │
│   │  Costs,  │         │ Billing, │          │ selection│       │ Groups   │   │
│   │  Services│         │ Contacts │          │          │       │          │   │
│   │  Sites)  │         │          │          │          │       │          │   │
│   └─────┬────┘         └─────┬────┘          └─────┬────┘       └─────┬────┘   │
│         │                    │                     │                  │         │
│         └─────────┐    ┌─────┘        ┌────────────┘    ┌─────────────┘         │
│                   │    │              │                  │                       │
│                   ▼    ▼              ▼                  ▼                       │
│            ╔═══════════════════════════════════════════════════╗                │
│            ║       MODULE 19: CONTRACT MANAGEMENT              ║                │
│            ║                                                    ║                │
│            ║  • Source: Approved GMA (M17)                      ║                │
│            ║  • Customer & Sites (M18)                         ║                │
│            ║  • Branch (M7)                                     ║                │
│            ║  • Technician Groups (M8)                         ║                │
│            ║  • Contract Terms, Payment, SLA                   ║                │
│            ║  • Service Schedule                               ║                │
│            ╚══════════════════╤════════════════════════════════╝                │
│                               │                                                │
│                               ▼                                                │
│                    ╔═══════════════════════╗                                    │
│                    ║  MODULE 20             ║                                    │
│                    ║  Sales Order Mgmt      ║                                    │
│                    ║  (Auto SO generation   ║                                    │
│                    ║   from contract)       ║                                    │
│                    ╚═══════════════════════╝                                    │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## Data Flow Table

| Source Module  | Data Provided                                                 | Used In (Contract Mgmt)                                |
| -------------- | ------------------------------------------------------------- | ------------------------------------------------------ |
| **Module 17**  | Approved GMA (Customer, Sites, Services, Costs, GM%, Branch)  | Section 1: Auto-population of all GMA-sourced fields   |
| **Module 18**  | Customer ID, Name, Billing Address, Sites                     | Section 1: Customer reference; Section 4: Add Site pool|
| **Module 7**   | Branch list (Active branches)                                 | Branch reference from GMA                              |
| **Module 8**   | Technician Teams / Groups                                     | Section 3: Assigned Technician Group dropdown          |
| **Module 19**  | Contract ID, Terms, Payment Schedule, Sites                   | Module 20: Source for auto-SO generation               |
| **Module 20**  | Sales Orders linked to this contract                          | 19.3 Tab 2: SO Schedule (Billing Log)                  |


=================================================================================


# 🎯 MODULE 20: SALES ORDER (SO) MANAGEMENT

## Overview

The official financial and operational document that confirms revenue boundaries and acts as a trigger to the Operations team to dispatch technicians or products for a job. Sales Orders can be generated automatically (cron-based for Contract contracts) or manually for one-time services and standalone product sales. Supports three distinct order types — Service Contract, One-Time Service, and Product Sale — each with its own source workflow and line-item structure.

**Module Connections:**

- **Depends on:** Module 19 (Contract Management – for Contract-based SO auto-generation), Module 17 (GMA Management – for one-time service SO source), Module 18 (Customer Management – customer & site data), Module 16 (Quotation Management – approved quotation for one-time SO), Module 10 (Product Master – for product sale line items), Module 9 (Tax Management – HSN/SAC tax calculation)
- **Used by:** Operations & Dispatch (Job Card generation), Invoicing & Billing, Inventory (chemical/product issuance from Module 11)
- **Prerequisites:** For Contract SOs, an **Active Contract** (Module 19) must exist. For One-Time Service SOs, an **Approved GMA/Quotation** (Module 17/16) or a valid Customer record (for standalone) is needed. For Product Sale SOs, the Product Master (Module 10) must be configured.

---

# 20.1 Sales Order Master List View

**Description:**
A comprehensive dashboard displaying all Sales Orders (execution mandates) in the system. Provides filtering by order type, status, and branch, along with search and row-level actions.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      SALES ORDER MANAGEMENT                                  │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                │  │
│  │                                                                        │  │
│  │ Order Type : [☑ Service Contract  ☑ One-Time Service  ☑ Product Sale]      │  │
│  │ Status     : [☑ Draft ☑ Open ☑ Fulfilled ☑ Billed ☑ Cancelled]      │  │
│  │ Branch     : [▼ All Branches ▼]                                       │  │
│  │ Customer   : [🔍 Search / Select ▼]                                   │  │
│  │ Date Range : [📅 From] – [📅 To]  (SO Date)                          │  │
│  │                                                                        │  │
│  │ Search: [________________________] (SO Number / Customer / Contract)  │  │
│  │                                                 [Reset Filters]       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│                                              [+ GENERATE SALES ORDER]        │
│                                                                              │
│  SALES ORDER TABLE                                                           │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │SO Number    │Customer        │Order Type     │Total Value│SO Date  │Status││
│  │─────────────┼────────────────┼───────────────┼───────────┼─────────┼──────││
│  │SO-2026-0112 │ABC Corporation │Svc Contract   │₹ 51,000   │01 Apr 26│✅ Open││
│  │SO-2026-0087 │ABC Corporation │Svc Contract   │₹ 51,000   │01 Jan 26│✅ Fulf││
│  │SO-2026-0043 │XYZ Hotel       │One-Time Svc   │₹ 8,500    │15 Jan 26│✅ Fulf││
│  │SO-2026-0038 │PQR Foods       │Product Sale   │₹ 12,200   │10 Jan 26│💰 Blld││
│  │SO-2026-0022 │DEF Mall        │Svc Contract   │₹ 15,000   │01 Jan 26│❌ Cncl││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Actions                                                                   ││
│  │──────────────────────────────────────────────────────────────────────────││
│  │[View] [Edit] [Cancel]                                                    ││
│  │[View]                                                                    ││
│  │[View]                                                                    ││
│  │[View]                                                                    ││
│  │[View]                                                                    ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   Next                              │
│                                                                              │
│  Legend: 📋 Draft  ✅ Open  ✅ Fulfilled  💰 Billed  ❌ Cancelled             │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table View Fields

| Field       | Type         | Description                                                   |
| ----------- | ------------ | ------------------------------------------------------------- |
| SO Number   | Text (Auto)  | System-generated unique ID (e.g., SO-2026-0112)               |
| Customer Name   | Text     | Customer name from Module 18                                  |
| Branch Name | Text         | Branch name from Module 7                                     |
| Order Type  | Badge        | Service Contract / One-Time Service / Product Sale                 |
| Total Value | Currency     | Total SO amount including taxes (₹)                           |
| SO Date     | Date         | Date the SO was generated                                     |
| Status      | Badge        | Draft / Open / Fulfilled / Billed / Cancelled                 |
| Actions     | Button Group | [View] [Edit] [Cancel]                                        |

---

## Filters

| Filter     | Type         | Options                                                       |
| ---------- | ------------ | ------------------------------------------------------------- |
| Order Type | Multi-select | Service Contract / One-Time Service / Product Sale                 |
| Status     | Multi-select | Draft / Open / Fulfilled / Billed / Cancelled                 |
| Date Range | Date Range   | From – To (SO creation date)                                  |

---

## Search

Searchable by:
- SO Number
- Customer Name
- Branch Name

---

## Actions (Table Row)

| Action     | Condition                                       | Description                                           |
| ---------- | ----------------------------------------------- | ----------------------------------------------------- |
| **View**   | All statuses                                    | Opens the SO Detail dashboard (Screen 20.3)           |
| **Edit**   | Status = Draft or Open; no Job Cards generated  | Opens the Edit SO form (Screen 20.4)                  |
| **Cancel** | Status = Draft or Open; no execution begun      | Opens the Cancellation dialog (Screen 20.5)           |

---

## Form Actions

| Action                     | Description                                               |
| -------------------------- | --------------------------------------------------------- |
| **+ Generate Sales Order** | Opens the **Add / Generate SO Form** (Screen 20.2)        |

---

================================================================================

# 20.2 Add / Generate Sales Order Form

**Description:**
A multi-section form to generate a Sales Order. Can be triggered automatically via cron (for Service Contracts based on the payment schedule) or created manually here. Supports three distinct source workflows: from an Active Contract (Linked), from an Approved Quotation / GMA (Linked), or as an Individual / Standalone Order (Direct entry for One-Time Service or Product Sales).

> **Note:** For Service Contracts, Sales Orders are typically **auto-generated** by the system cron based on the contract's payment schedule (Module 19). This form is used for manual/individual SO creation — one-time services, product sales, or ad-hoc Contract SOs.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to SO List]            GENERATE SALES ORDER                         │
│                                                                              │
│  SO Number: SO-XXXX (Auto-generated on save)                                 │
│                                                                              │
│  SECTION 1: ORDER SOURCE & TYPE                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Order Type*:                                                           │ │
│  │  (•) Service Contract    ( ) One-Time Service    ( ) Product Sale           │ │
│  │                                                                         │ │
│  │  ━━━ IF SERVICE CONTRACT ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │ │
│  │  Source Type*      : From Active Contract (Fixed)                       │ │
│  │  Select Contract*  : [🔍 Search Contract by ID / Customer ▼]            │ │
│  │                                                                         │ │
│  │  AUTO-FILLED FROM CONTRACT:                                             │ │
│  │  Contract ID       : CON-2026-0041  (Read-only)                        │ │
│  │  Customer Name     : ABC Corporation (Read-only)                       │ │
│  │  Billing Period*   : [▼ Q2 Apr–Jun 2026 ▼]                            │ │
│  │                                                                         │ │
│  │  ━━━ IF ONE-TIME SERVICE ━━━━━━━━━━━━━━━━───────────────────━━━━━━━━━  │ │
│  │  Source Type*      : ( ) From Quotation/GMA    (•) Standalone (Indiv.)  │ │
│  │                                                                         │ │
│  │  IF FROM QUOTATION/GMA:                                                 │ │
│  │  Select Source*    : [🔍 Search Quotation / GMA by ID / Customer ▼]     │ │
│  │                                                                         │ │
│  │  IF STANDALONE:                                                         │ │
│  │  Select Customer*  : [🔍 Search Customer by Name / ID ▼]                │ │
│  │                                                                         │ │
│  │  ━━━ IF PRODUCT SALE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │ │
│  │  Source Type*      : Standalone (Fixed)                                 │ │
│  │  Select Customer*  : [🔍 Search Customer by Name / ID ▼]                │ │
│  │                                                                         │ │
│  │  SO Date*          : [📅 Date Picker] (Default: Today)                 │ │
│  │  Branch*           : [▼ Auto from source / Select ▼]                   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 2: DYNAMIC SCOPE & EXECUTION (SITE-WISE)                            │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  ━━━ FOR SERVICES (Contract / One-Time) ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │ │
│  │  (Auto-fetched from source OR manually added for standalone)            │ │
│  │                                                                         │ │
│  │  SITE 1: Head Office (SITE-00312)                                       │ │
│  │  ┌───────────────────────────────────────────────────────────────────┐  │ │
│  │  │ SERVICE 1: Cockroach Treatment                                    │  │ │
│  │  │ ┌────────┬──────────┬────────┬───────┬───────┬─────────┬────────┐ │  │ │
│  │  │ │Visits  │Unit Price│SQFT    │HSN    │Tax %  │Tax Amt  │Total   │ │  │ │
│  │  │ ├────────┼──────────┼────────┼───────┼───────┼─────────┼────────┤ │  │ │
│  │  │ │[ 12 ]  │₹ 3,250   │3,500   │996490 │18%    │₹ 7,020  │₹ 46,020│ │  │ │
│  │  │ └────────┴──────────┴────────┴───────┴───────┴─────────┴────────┘ │  │ │
│  │  │                                                                  │  │ │
│  │  │  ─── CONSUMABLE CHEMICALS (Read-only from Source) ─────────────  │  │ │
│  │  │  ┌────────────────────┬────────┬──────┬─────────┐              │  │ │
│  │  │  │ Chemical Name      │ HSN    │ UOM  │ Req Qty │              │  │ │
│  │  │  ├────────────────────┼────────┼──────┼─────────┤              │  │ │
│  │  │  │ Alpha Cyper.       │ 3808   │ ml   │ 120 ml  │              │  │ │
│  │  │  │ Fipronil Gel       │ 3808   │ tube │ 2 tubes │              │  │ │
│  │  │  └────────────────────┴────────┴──────┴─────────┘              │  │ │
│  │  │                                                                  │  │ │
│  │  │ [Remove Service]                                                 │  │ │
│  │  └───────────────────────────────────────────────────────────────────┘  │ │
│  │  [+ ADD SERVICE TO THIS SITE]                                           │ │
│  │                                                                         │ │
│  │  [+ ADD ANOTHER SITE]                                                    │ │
│  │                                                                         │ │
│  │  ━━━ FOR PRODUCTS (Product Sale) ━━━━━━━━━━━━━━━━───────────────────━  │ │
│  │  ┌───────────────────────────────────────────────────────────────────┐  │ │
│  │  │#│Product        │Qty │Unit Pr. (₹)     │HSN  │Tax% │Line Total│  │ │
│  │  │─┼───────────────┼────┼─────────────────┼─────┼─────┼──────────│  │ │
│  │  │1│Alpha Cyperm.  │[ 5]│[₹ 1,200] (Edit) │3808 │18%  │₹ 7,080   │  │ │
│  │  │2│Fipronil Gel   │[20]│[₹ 220]   (Edit) │3808 │18%  │₹ 5,192   │  │ │
│  │  └───────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                         │ │
│  │  ═══ ORDER TOTAL SUMMARY ═══════════════════════════════════════════  │ │
│  │  Sub-Total (₹)    : ₹ 61,300                                          │ │
│  │  Discount (₹)     : [ 0 ] [▼ Flat ▼]                                  │ │
│  │  Tax Total (₹)    : ₹ 11,016                                          │ │
│  │  ─────────────────────────────────────────────────────────────────     │ │
│  │  GRAND TOTAL (₹)  : ₹ 72,316                                          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 3: DELIVERY / EXECUTION TERMS                                       │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Execution Notes:                                                       │ │
│  │  [________________________________________________________]            │ │
│  │  [________________________________________________________]            │ │
│  │  (e.g., "Weekend service only", "Dispatch product to Site B")          │ │
│  │                                                                         │ │
│  │  Delivery Address  : [▼ Select Site Address / Custom ▼]                │ │
│  │                      (Auto from Customer's registered sites)           │ │
│  │                                                                         │ │
│  │  IF CUSTOM ADDRESS:                                                     │ │
│  │  Address Line 1*  : [________________________________]                 │ │
│  │  Address Line 2   : [________________________________]                 │ │
│  │  City*            : [________________]                                 │ │
│  │  State*           : [▼ Select State ▼]                                 │ │
│  │  Pincode          : [________]                                         │ │
│  │                                                                         │ │
│  │  Priority Level   : [▼ Normal / Urgent / Critical ▼]                   │ │
│  │  Expected Delivery: [📅 Date Picker]                                   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [SAVE AS DRAFT]      [SAVE & OPEN]      [CANCEL]                       │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Section 1: Order Source & Type Fields

| Field              | Type            | Required    | Options / Validation                                                                | Notes                                          |
| ------------------ | --------------- | ----------- | ----------------------------------------------------------------------------------- | ---------------------------------------------- |
| Order Type         | Radio           | Yes         | Service Contract / One-Time Service / Product Sale                                       | Determines which source + line-item fields show|
| Select Contract    | Search Dropdown | Conditional | Active contracts from Module 19                                                     | Required if Type = Service Contract                 |
| Billing Period     | Dropdown        | Conditional | Available billing periods from contract's payment schedule                          | Required if Type = Service Contract                 |
| Select Quotation/GMA | Search Dropdown | Conditional | Approved Quotations (Module 16) or Approved GMAs (Module 17)                     | Required if Type = One-Time Service            |
| Select Customer    | Search Dropdown | Conditional | Active customers from Module 18                                                     | Required if Type = Product Sale                |
| Contract ID        | Auto-filled     | System      | Read-only                                                                           | Populated from contract selection              |
| Customer Name      | Auto-filled     | System      | Read-only                                                                           | From source record                             |
| Customer ID        | Auto-filled     | System      | Read-only                                                                           | From source record                             |
| Service Type       | Auto-filled     | System      | Read-only (for service orders)                                                      | Pest / service category                        |
| Contract / Quoted Value | Auto-filled | System      | Read-only                                                                           | Reference value from source                    |
| SO Date            | Date            | Yes         | Default: Today; cannot be future > 30 days                                          | Sales Order creation date                      |
| Branch             | Dropdown        | Yes         | Auto from source; editable — active branches from Module 7                          | Servicing / dispatch branch                    |

> **Note:** For **Service Contract** orders, the system only shows contracts with unused billing periods. If all billing periods already have SOs, the contract will not appear in the search. For **One-Time Service**, only Quotations / GMAs that have not yet been converted to an SO are shown.

---

## Section 2: Line Items Fields

### Service Line Items (Site-Wise Configuration)

| Field           | Type            | Required | Validation / Notes                                                              |
| --------------- | --------------- | -------- | ------------------------------------------------------------------------------- |
| Site Name       | Search/Text     | Yes      | Auto-fetched from source OR manually entered for standalone                     |
| Service Name    | Search Dropdown | Yes      | Select from Module 12 (Services) if standalone; auto-fetched if linked          |
| Qty (Visits)    | Number          | Yes      | Number of visits for this SO; editable, must be > 0                             |
| Unit Price (₹)  | Currency        | Yes      | Price per visit; auto from contract/GMA; Read-only for linked; editable for standalone |
| HSN/SAC Code    | Auto-filled     | System   | Service Accounting Code for tax calculation (from Module 9)                     |
| Tax %           | Auto-filled     | System   | GST rate from HSN/SAC code (Module 9)                                           |
| Tax Amount (₹)  | Auto-calculated | System   | `Qty × Unit Price × Tax %`                                                      |
| Line Total (₹)  | Auto-calculated | System   | `(Qty × Unit Price) + Tax Amount`                                               |

### Chemical Line Items (Read-Only from Source)

| Field           | Type            | Required | Validation / Notes                                                              |
| --------------- | --------------- | -------- | ------------------------------------------------------------------------------- |
| Chemical Name   | Auto-filled     | System   | Fetched from Module 12 (Service Config) or Source Contract/GMA                  |
| HSN             | Auto-filled     | System   | From Product Master (Module 10)                                                 |
| UOM             | Auto-filled     | System   | Base unit (ml, Ltr, tube, etc.) from Module 10                                  |
| Req Qty         | Auto-filled     | System   | Quantity required per visit as per approved GMA/Contract                        |

### Product Line Items (Product Sale)

| Field              | Type            | Required | Validation / Notes                                                       |
| ------------------ | --------------- | -------- | ------------------------------------------------------------------------ |
| Product Name       | Search Dropdown | Yes      | Select from Module 10 Product Master (active products only)              |
| Product Code       | Auto-filled     | System   | Read-only; from Module 10                                                |
| UOM                | Auto-filled     | System   | Base unit of measure from Module 10 (ml / Ltr / gm / kg / Nos / Tube)   |
| Qty                | Number          | Yes      | Must be > 0                                                              |
| Unit Price (₹)     | Currency        | Yes      | Defaults to **Selling Price** from Module 10; editable for custom manual pricing |
| HSN Code           | Auto-filled     | System   | From Module 10 product master → Module 9 tax config                      |
| Tax %              | Auto-filled     | System   | GST rate from HSN code (Module 9)                                        |
| Tax Amount (₹)     | Auto-calculated | System   | `Qty × Unit Price × Tax%`                                                |
| Line Total (₹)     | Auto-calculated | System   | `(Qty × Unit Price) + Tax Amount`                                        |

### Order Total Summary (Auto-Calculated)

| Field          | Type            | Description                                         |
| -------------- | --------------- | --------------------------------------------------- |
| Sub-Total (₹)  | Auto-calculated | Sum of all line items before tax                    |
| Discount (₹)   | Currency        | Optional; flat or percentage discount               |
| Tax Total (₹)  | Auto-calculated | Sum of all line-item tax amounts                    |
| Grand Total (₹)| Auto-calculated | `Sub-Total – Discount + Tax Total`                  |

---

## 💡 Note: How `[+ Add Site]` and `[+ Add Service]` Works

- **`[+ Add Site]`**: A single Sales Order can cover multiple physical locations for the customer. Adding a new site creates a fresh site-block where specific services can be assigned. 
- **`[+ Add Service to this Site]`**: Within a single Site, a customer may require multiple different treatments (e.g., Cockroach + Termite). Adding a service inside a site spins up a fresh line item for that specific treatment at that specific location.
- **Linked Orders**: If the SO is linked to a Contract or GMA, the system auto-populates all Site/Service blocks based on the source record. The user can remove sites/services or adjust quantities but cannot add new service types not present in the source.
- **Standalone Orders**: The user has full freedom to add any number of sites and select any services from the Service Master (Module 12).
- **Consumable Chemicals**: These are automatically pulled from the source GMA or Contract and are displayed in read-only mode to ensure the service is executed exactly as scoped and approved.
- **Product Pricing**: For product-only sales, the system defaults to the **Selling Price** from the Product Master (Module 10). However, the price is editable to allow for ad-hoc custom quotes.

---

---

## Section 3: Delivery / Execution Terms Fields

| Field             | Type      | Required | Options / Validation                                         | Notes                                           |
| ----------------- | --------- | -------- | ------------------------------------------------------------ | ----------------------------------------------- |
| Execution Notes   | Text Area | No       | Max 500 characters                                           | Operational instructions for the dispatch team  |
| Delivery Address  | Dropdown  | No       | Customer's registered sites (Module 18) / Custom             | Default: primary site                           |
| Custom Address    | Text      | Cond.    | Required if Delivery Address = Custom; min 10 chars          | Full address for non-registered delivery        |
| City              | Text      | Cond.    | Required if Custom; min 3 chars                              | Delivery city                                   |
| State             | Dropdown  | Cond.    | Indian states list                                           | Delivery state                                  |
| Pincode           | Number    | No       | 6-digit numeric                                              | Optional                                        |
| Priority Level    | Dropdown  | No       | Normal / Urgent / Critical                                   | Determines dispatch priority in Operations      |
| Expected Delivery | Date      | No       | Must be ≥ SO Date                                            | Target date for service / product delivery      |

---

## Form Actions

| Button          | Description                                                                          |
| --------------- | ------------------------------------------------------------------------------------ |
| **Save as Draft** | Saves the SO without releasing. Status = Draft. Can be edited freely.              |
| **Save & Open** | Validates all sections, saves the SO with Status = Open, and makes it available for Operations dispatch. |
| **Cancel**      | Discards all entries and returns to the SO Master List.                               |

---

## Validation Rules

| Validation                        | Rule                                                                      |
| --------------------------------- | ------------------------------------------------------------------------- |
| Order Type Required               | Must select Service Contract, One-Time, or Product Sale                        |
| Source Required                   | Must select a valid contract / quotation / customer based on type         |
| At Least 1 Line Item              | Minimum one service or product line item required                         |
| Qty > 0                          | All line item quantities must be positive                                 |
| Unit Price > 0                   | All line item prices must be positive                                     |
| Grand Total > 0                  | Final order value must be positive after discounts                        |
| No Duplicate Billing Period       | For Contract, cannot create two SOs for the same billing period and contract   |
| SO Date Required                 | Must have a valid SO date                                                 |
| Branch Required                  | Must have an assigned branch                                              |

---

## System Behaviours on Save

| Trigger                         | System Action                                                                     |
| ------------------------------- | --------------------------------------------------------------------------------- |
| Contract selected (Contract)         | Auto-populates customer details, sites, services, and pricing from Module 19      |
| Quotation/GMA selected (One-Time)| Auto-populates service details and pricing from Module 16/17                      |
| Customer selected (Product)     | Auto-populates billing address from Module 18                                     |
| Product selected                | Fetches UOM, HSN Code, Sale Price from Module 10 Product Master                   |
| Save & Open clicked             | Status → Open; SO visible to Operations for dispatch                              |
| Contract SO created                  | Linked to contract billing log (Module 19 → Tab 2); fulfilment tracker updated   |
| One-Time SO created             | Quotation / GMA marked as "SO Generated" to prevent duplicate SOs                |
| Tax calculation                 | HSN/SAC codes auto-fetch tax rates; tax amounts computed in real-time             |

---

================================================================================

# 20.3 View Sales Order Details

**Description:**
A clear, read-only layout detailing exactly what needs to be billed and executed. Split into two tabs — SO Summary & Line Items, and Execution & Delivery Status.

---

## Screen Layout (Header)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to SO List]                              [Edit SO]  [Cancel SO]    │
│                                                                              │
│  SALES ORDER: SO-2026-0112                     Status: ✅ OPEN               │
│  Customer : ABC Corporation (CUST-00245)       │  Branch: Mumbai             │
│  Order Type: Service Contract                       │  Contract: CON-2026-0041    │
│  SO Date  : 01 Apr 2026                        │  Period: Q2 Apr–Jun 2026    │
│  Grand Total: ₹ 72,316                         │  Priority: Normal           │
│                                                                              │
│  [Tab 1: SO Summary & Line Items ●]  [Tab 2: Execution & Delivery Status]  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

================================================================================

# 20.3.1 Tab 1: SO Summary & Line Items

**Description:**
Displays the complete order breakdown — header details, source reference, and a detailed line-item grid with HSN/SAC codes, quantities, taxes, and final amounts.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Tab 1: SO Summary & Line Items ●]  [Tab 2: Execution & Delivery Status] │
│                                                                              │
│  ─── ORDER HEADER ────────────────────────────────────────────────────────  │
│  SO Number     : SO-2026-0112           SO Date      : 01 Apr 2026         │
│  Customer      : ABC Corporation        Customer ID  : CUST-00245          │
│  Order Type    : Service Contract            Contract Ref : CON-2026-0041       │
│  Billing Period: Q2 Apr–Jun 2026        Branch       : Mumbai              │
│  Priority      : Normal                 Expected Date: 30 Jun 2026         │
│                                                                              │
│  ─── LINE ITEMS (SITE-WISE) ────────────────────────────────────────────────  │
│  SITE 1: Head Office (SITE-00312)                                            │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │ SERVICE 1: Cockroach Treatment                                           ││
│  │ ┌────────┬──────────┬────────┬───────┬───────┬──────────┬──────────┐     ││
│  │ │ Qty    │ Unit Pr. │ SQFT   │ HSN   │ Tax%  │ Tax Amt  │ Total    │     ││
│  │ ├────────┼──────────┼────────┼───────┼───────┼──────────┼──────────┤     ││
│  │ │ 12     │ ₹ 3,250  │ 3,500  │ 996490│ 18%   │ ₹ 7,020  │ ₹ 46,020 │     ││
│  │ └────────┴──────────┴────────┴───────┴───────┴──────────┴──────────┘     ││
│  │                                                                          ││
│  │ ─── CONSUMABLE CHEMICALS (Read-only) ──────────────────────────────────  ││
│  │ ┌──────────────────────┬─────────┬────────┬───────────┐                  ││
│  │ │ Chemical Name        │ HSN     │ UOM    │ Req Qty   │                  ││
│  │ ├──────────────────────┼─────────┼────────┼───────────┤                  ││
│  │ │ Alpha Cypermethrin   │ 3808    │ ml     │ 120 ml    │                  ││
│  │ │ Fipronil Gel         │ 3808    │ tube   │ 2 tubes   │                  ││
│  │ └──────────────────────┴─────────┴────────┴───────────┘                  ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  SITE 2: Warehouse (SITE-00313) [▸ Click to View]                            │
│                                                                              │
│  ━━━ FOR PRODUCTS (Product Sale) ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━   │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │#│Product        │Qty │Unit Pr. │HSN  │Tax% │Tax Amt │Line Total│           ││
│  │─┼───────────────┼────┼─────────┼─────┼─────┼────────┼──────────│           ││
│  │1│Alpha Cyperm.  │ 5  │₹ 1,200  │3808 │18%  │₹ 1,080 │₹ 7,080   │           ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ─── ORDER TOTAL ─────────────────────────────────────────────────────────  │
│  Sub-Total   : ₹ 61,300                                                     │
│  Discount    : ₹ 0.00                                                       │
│  Tax Total   : ₹ 11,016                                                     │
│  ───────────────────────────────────────                                     │
│  GRAND TOTAL : ₹ 72,316                                                     │
│                                                                              │
│  ─── EXECUTION NOTES ─────────────────────────────────────────────────────  │
│  "Service visits to be scheduled on Wednesdays and Saturdays.               │
│   Warehouse site: Monthly service on 1st Monday only."                      │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Order Header Fields (Read-Only)

| Field          | Type     | Description                                              |
| -------------- | -------- | -------------------------------------------------------- |
| SO Number      | Display  | Unique SO identifier                                     |
| SO Date        | Date     | Date the SO was generated                                |
| Customer       | Display  | Customer name from Module 18                             |
| Customer ID    | Display  | Unique customer reference                                |
| Order Type     | Display  | Service Contract / One-Time Service / Product Sale            |
| Contract Ref   | Link     | Contract ID (Contract only); navigates to Module 19           |
| Billing Period | Display  | Period this SO covers (Contract only)                         |
| Branch         | Display  | Servicing / dispatch branch                              |
| Priority       | Display  | Normal / Urgent / Critical                               |
| Expected Date  | Date     | Target completion date                                   |

---

## Line Item Grid Fields (Read-Only)

| Field           | Type     | Description                                             |
| --------------- | -------- | ------------------------------------------------------- |
| #               | Number   | Line item sequence number                               |
| Description     | Display  | Service name or Product name                            |
| Site            | Display  | Site for service delivery (service orders only)         |
| Qty             | Number   | Visits (service) or units (product)                     |
| UOM             | Display  | Visits / Ltr / Nos / Tube / kg etc.                     |
| Unit Price (₹)  | Currency | Per-unit price                                          |
| HSN/SAC Code    | Display  | Tax classification code                                 |
| Tax %           | Display  | GST percentage                                          |
| Tax Amount (₹)  | Currency | Computed tax for this line                              |
| Line Total (₹)  | Currency | `(Qty × Unit Price) + Tax`                              |

---

## Order Total Fields (Read-Only)

| Field         | Type     | Description                          |
| ------------- | -------- | ------------------------------------ |
| Sub-Total     | Currency | Sum of all pre-tax line totals       |
| Discount      | Currency | Any applied discount                 |
| Tax Total     | Currency | Sum of all line-item taxes           |
| Grand Total   | Currency | Final payable amount                 |

---

================================================================================
[Note: It is depends on Module 21 or Task allocation module]
# 20.3.2 Tab 2: Execution & Delivery Status

**Description:**
Tracks the downstream release of the Sales Order. Shows whether Job Cards (for services) or Delivery Challans (for products) have been generated against this specific SO, providing an end-to-end visibility from order to execution.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Tab 1: SO Summary & Line Items]  [Tab 2: Execution & Delivery Status ●]  │
│                                                                              │
│  ─── SERVICE EXECUTION STATUS ────────────────────────────────────────────  │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Job Card ID  │Site      │Scheduled Date│Technician  │Status    │Actions   ││
│  │─────────────┼──────────┼──────────────┼────────────┼──────────┼──────────││
│  │JC-2026-0441 │Head Off. │05 Apr 2026   │Ravi S.     │✅ Done   │[View]    ││
│  │JC-2026-0442 │Showroom  │05 Apr 2026   │Anjali M.   │✅ Done   │[View]    ││
│  │JC-2026-0443 │Head Off. │12 Apr 2026   │Ravi S.     │🕐 Planned│[View]    ││
│  │JC-2026-0444 │Warehouse │01 May 2026   │Suresh K.   │⏳ Pending│[View]    ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ─── PRODUCT DELIVERY STATUS ─────────────────────────────────────────────  │
│  (Visible only for Product Sale or mixed orders)                             │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Challan ID   │Product       │Qty │Dispatch Date│Delivery Add │Status     ││
│  │─────────────┼──────────────┼────┼─────────────┼─────────────┼───────────││
│  │DC-2026-0101 │Alpha Cyper.  │ 5L │12 Apr 2026  │Koramangala  │🚚 In Transit│
│  │DC-2026-0102 │Fipronil Gel  │20T │12 Apr 2026  │Koramangala  │⏳ Pending ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ─── EXECUTION SUMMARY ──────────────────────────────────────────────────  │
│  Total Job Cards     : 4         Completed: 2    Pending: 2                  │
│  Total Challans      : 2         Dispatched: 1   Pending: 1                  │
│  Overall Progress    : ██████████░░░░░░░░░░  50%                            │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Service Execution Grid Fields (Read-Only)

| Field          | Type    | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| Job Card ID    | Link    | Unique job card reference; navigates to Operations module    |
| Site           | Display | Site where service is performed                              |
| Scheduled Date | Date    | Planned service date                                         |
| Technician     | Display | Assigned technician name                                     |
| Status         | Badge   | Planned / In Progress / Done / Cancelled                     |
| Actions        | Button  | [View] — opens the job card detail in Operations module      |

---

## Product Delivery Grid Fields (Read-Only)

| Field          | Type    | Description                                           |
| -------------- | ------- | ----------------------------------------------------- |
| Challan ID     | Link    | Delivery Challan reference; navigates to dispatch     |
| Product        | Display | Product name from the SO line item                    |
| Qty            | Number  | Quantity dispatched                                   |
| Dispatch Date  | Date    | Date of product dispatch                              |
| Delivery Addr  | Display | Destination address                                   |
| Status         | Badge   | Pending / In Transit / Delivered / Returned           |

---

## Execution Summary Fields (Auto-Calculated)

| Field            | Type     | Description                                     |
| ---------------- | -------- | ----------------------------------------------- |
| Total Job Cards  | Number   | Count of all generated job cards                |
| Completed        | Number   | Job cards with Status = Done                    |
| Total Challans   | Number   | Count of all delivery challans                  |
| Dispatched       | Number   | Challans with Status = In Transit or Delivered  |
| Overall Progress | Progress | Percentage of completed execution items          |

> **Note:** This tab is **read-only**. Job Cards and Delivery Challans are created and managed by the **Operations & Dispatch** module. This tab provides the commercial team visibility into whether the SO's execution has started, which gates the Edit and Cancel actions.

---

================================================================================

# 20.4 Edit Sales Order Form

**Description:**
Allows modification of a Sales Order before it is released to Operations or while still in Draft/Open status. Editing is fully blocked once execution has begun (Job Cards printed or Challans dispatched) or the SO status is Fulfilled/Billed.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back to SO Detail]         EDIT SALES ORDER   SO-2026-0112              │
│                                                                              │
│  SECTION 1: ORDER HEADER (Read-Only)                                         │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  SO Number     : SO-2026-0112      Status    : ✅ Open                  │ │
│  │  Customer      : ABC Corporation   Order Type: Service Contract              │ │
│  │  Contract Ref  : CON-2026-0041     SO Date   : 01 Apr 2026             │ │
│  │  Branch        : Mumbai                                                 │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 2: DYNAMIC SCOPE & LINE ITEMS                                       │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  SITE 1: Head Office (SITE-00312)                                       │ │
│  │  ┌───────────────────────────────────────────────────────────────────┐  │ │
│  │  │ SERVICE 1: Cockroach Treatment                                    │  │ │
│  │  │ ┌────────┬──────────┬────────┬───────┬───────┬─────────┬────────┐ │  │ │
│  │  │ │Visits  │Unit Price│SQFT    │HSN    │Tax %  │Tax Amt  │Total   │ │  │ │
│  │  │ ├────────┼──────────┼────────┼───────┼───────┼─────────┼────────┤ │  │ │
│  │  │ │[ 12 ]  │[₹ 3,250] │3,500   │996490 │18%    │₹ 7,020  │₹ 46,020│ │  │ │
│  │  │ └────────┴──────────┴────────┴───────┴───────┴─────────┴────────┘ │  │ │
│  │  │                                                                  │  │ │
│  │  │  ─── CONSUMABLE CHEMICALS (Read-only from Source) ─────────────  │  │ │
│  │  │  ┌────────────────────┬────────┬──────┬─────────┐              │  │ │
│  │  │  │ Chemical Name      │ HSN    │ UOM  │ Req Qty │              │  │ │
│  │  │  ├────────────────────┼────────┼──────┼─────────┤              │  │ │
│  │  │  │ Alpha Cyper.       │ 3808   │ ml   │ 120 ml  │              │  │ │
│  │  │  │ Fipronil Gel       │ 3808   │ tube │ 2 tubes │              │  │ │
│  │  │  └────────────────────┴────────┴──────┴─────────┘              │  │ │
│  │  │                                                                  │  │ │
│  │  │ [Remove Service]                                                 │  │ │
│  │  └───────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                         │ │
│  │  ━━━ FOR PRODUCTS (Product Sale) ━━━━━━━━━━━━━━━━───────────────────━  │ │
│  │  ┌───────────────────────────────────────────────────────────────────┐  │ │
│  │  │#│Product        │Qty │Unit Pr. (₹)     │HSN  │Tax% │Line Total│  │ │
│  │  │─┼───────────────┼────┼─────────────────┼─────┼─────┼──────────│  │ │
│  │  │1│Alpha Cyperm.  │[ 5]│[₹ 1,200] (Edit) │3808 │18%  │₹ 7,080   │  │ │
│  │  └───────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                         │ │
│  │  Editable Fields: Qty, Unit Price                                       │ │
│  │                                                                         │ │
│  │                                                                         │ │
│  │  Discount*         : [▼ None / Flat ₹ / Percentage % ▼]  [________]    │ │
│  │                                                                         │ │
│  │  Revised Sub-Total : ₹ 61,300                                          │ │
│  │  Revised Tax       : ₹ 11,016                                          │ │
│  │  Revised Grand Total: ₹ 72,316                                         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 3: EXECUTION NOTES (Editable)                                       │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Execution Notes: [Weekend service only__________________________]     │ │
│  │  Priority Level : [▼ Normal ▼]                                          │ │
│  │  Expected Date  : [📅 30 Jun 2026]                                      │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│       [SAVE CHANGES]                                    [CANCEL]             │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Editable vs. Locked Fields

| Field                   | Editable? | Notes                                                              |
| ----------------------- | --------- | ------------------------------------------------------------------ |
| SO Number               | ❌ Locked  | System-generated; immutable                                        |
| Customer / Contract Ref | ❌ Locked  | Source data cannot be changed                                      |
| Order Type              | ❌ Locked  | Cannot change the order type after creation                        |
| SO Date                 | ❌ Locked  | Historical record; cannot be modified                              |
| Branch                  | ❌ Locked  | Cannot change once created                                         |
| Line Item Qty           | ✅ Yes     | Can increase/decrease visit counts (if standalone) or product quantities |
| Line Item Unit Price    | ✅ Yes     | Only for standalone orders OR within approved variance for linked orders |
| Consumable Chemicals    | ❌ Locked  | Pulled from source GMA/Contract; cannot be edited in SO           |
| Chemical Substitution   | ❌ Locked  | Substitution must be handled at the source (Contract/GMA)           |
| Discount                | ✅ Yes     | Can add / modify flat or percentage discount                       |
| Execution Notes         | ✅ Yes     | Free-text operational instructions                                 |
| Priority Level          | ✅ Yes     | Can escalate or de-escalate                                        |
| Expected Delivery Date  | ✅ Yes     | Can adjust target completion date                                  |

---

## Validation Rules

| Validation                          | Rule                                                                    |
| ----------------------------------- | ----------------------------------------------------------------------- |
| Status must be Draft or Open        | Edit fails if Status = Fulfilled, Billed, or Cancelled                  |
| No Job Cards generated              | Edit is **completely blocked** if any Job Cards exist for this SO       |
| No Delivery Challans dispatched     | Edit is **completely blocked** if any Challans exist for this SO        |
| Qty > 0                            | All line item quantities must remain positive                           |
| Unit Price > 0                     | All prices must remain positive                                         |
| Grand Total > 0                    | Order value must remain positive after discounts                        |
| Chemical substitution      | ❌ Disallowed | Substitution must be handled at the Contract/GMA level, not in SO |

---

## Form Actions

| Button            | Description                                                                     |
| ----------------- | ------------------------------------------------------------------------------- |
| **Save Changes**  | Validates all edits, recalculates totals, saves the updated SO                  |
| **Cancel**        | Discards unsaved changes and returns to the SO Detail view                      |

---

================================================================================

# 20.5 Delete (Cancel) Sales Order

**Description:**
A mechanism to void a Sales Order. Cancellation is only possible before execution begins. The SO record is preserved with a "Cancelled" status for audit — hard deletion is not permitted.

---

## Screen Layout

```
┌───────────────────────────────────────────────────────┐
│           CANCEL SALES ORDER                           │
│  Cancellation Reason*:                                 │
│  [▼ Select Reason ▼]                                   │
│      • Customer Request                                │
│      • Duplicate Order                                 │
│      • Incorrect Line Items                            │
│      • Contract Terminated                             │
│      • Service No Longer Required                      │
│      • Other                                           │
│                                                        │
│  Additional Remarks:                                   │
│  [________________________________________]            │
│  [________________________________________]            │
│                                                        │
│  [CONFIRM CANCELLATION]        [GO BACK]               │
└───────────────────────────────────────────────────────┘
```

*When cancellation is blocked:*

```
┌───────────────────────────────────────────────────────┐
│           CANCEL SALES ORDER                           │
│                                                        │
│  SO Number  : SO-2026-0087                             │
│  Status     : ✅ Open                                   │
│                                                        │
│  ELIGIBILITY CHECK:                                    │
│  Job Cards Generated  : ❌  2 Job Cards exist          │
│  Pushed to Invoicing  : ❌  Invoice INV-2026-0019      │
│                                                        │
│  ──  Cancellation is BLOCKED.  ──                      │
│  Execution has already begun or this SO has been       │
│  pushed to invoicing. Please contact Operations        │
│  or Finance to resolve.                                │
│                                                        │
│              [OK — Go Back]                            │
└───────────────────────────────────────────────────────┘
```

---

## Cancellation Prerequisite Check for backend and frontend

| Check                   | Condition to Block                                          | Error Message                                               |
| ----------------------- | ----------------------------------------------------------- | ----------------------------------------------------------- |
| Job Cards Generated     | Any Job Card exists for this SO                             | "X Job Card(s) already generated. Cannot cancel."           |
| Challans Dispatched     | Any Delivery Challan exists for this SO                     | "Products have been dispatched. Cannot cancel."              |
| Pushed to Invoicing     | SO has been linked to an invoice                            | "SO is linked to Invoice. Contact Finance."                 |
| Status = Fulfilled/Billed| SO already completed                                       | "Fulfilled/Billed orders cannot be cancelled."              |

---

## Cancellation Form Fields

| Field                | Type     | Required    | Options / Validation                                                                 |
| -------------------- | -------- | ----------- | ------------------------------------------------------------------------------------ |
| Cancellation Reason  | Dropdown | Yes         | Customer Request / Duplicate Order / Incorrect Line Items / Contract Terminated / Service No Longer Required / Other |
| Additional Remarks   | Text Area| No          | Max 500 characters; required if Reason = Other                                       |

---

## Validation Rules

| Validation                    | Rule                                                            |
| ----------------------------- | --------------------------------------------------------------- |
| No Job Cards                  | Cancellation blocked if any Job Card exists for this SO         |
| No Challans                   | Cancellation blocked if any Delivery Challan dispatched         |
| Not in Invoicing              | Cancellation blocked if SO linked to an invoice                 |
| Status = Draft or Open        | Only Draft and Open SOs can be cancelled                        |
| Reason Required               | Must select a reason from the dropdown                          |
| Remarks (if Other)            | Mandatory free-text when "Other" is selected                    |

---

## System Behaviour on Cancellation

| Trigger                         | System Action                                                                |
| ------------------------------- | ---------------------------------------------------------------------------- |
| Confirm Cancellation clicked    | SO status set to **Cancelled**                                                |
| Svc Contract billing period freed   | For Service Contract SOs, the billing period becomes available for re-generation |
| Operations notified             | SO removed from dispatch queue; no future Job Cards generated                |
| Contract billing log updated    | Module 19 → Tab 2 reflects the cancelled SO with updated fulfilment tracker  |
| Customer history updated        | Module 18 → Tab 3 reflects the cancelled status                              |
| Audit log                       | Cancellation event logged with Reason, Timestamp, and user who acted         |

---

================================================================================

## RBAC – Role-Based Access Control

| Role              | List View (20.1)         | Generate SO (20.2) | View Details (20.3) | Edit SO (20.4)  | Cancel SO (20.5)  |
| ----------------- | ------------------------ | ------------------ | ------------------- | --------------- | ----------------- |
| Sales Person      | View (own branch)        | ✅                  | ✅                   | ✅ (own SOs)     | ❌                 |
| Sales Manager     | View (own branch / team) | ✅                  | ✅                   | ✅               | ✅                 |
| Branch Manager    | View (own branch)        | ✅                  | ✅                   | ✅               | ✅                 |
| Head Ops / Admin  | View (all branches)      | ✅                  | ✅                   | ✅               | ✅                 |
| Finance Auditor   | View (all – read-only)   | ❌                  | ✅                   | ❌               | ❌                 |
| System (Cron)     | —                        | ✅ (Auto Svc Contract)   | —                   | —               | —                 |

---

================================================================================

## Navigation Flow Summary

```
MODULE 20: SALES ORDER MANAGEMENT
│
├── 20.1 Sales Order Master List
│   ├── [+ Generate Sales Order] → 20.2 Add / Generate SO Form
│   │     ├── Service Contract → Select Active Contract (Module 19)
│   │     │     └── Auto-fetch customer, sites, services, pricing
│   │     ├── One-Time Service → Select Approved Quotation/GMA (Module 16/17)
│   │     │     └── Auto-fetch service details and pricing
│   │     ├── Product Sale → Select Customer (Module 18)
│   │     │     └── Add products from Module 10 Product Master
│   │     ├── [Save as Draft]   → SO created (Draft)
│   │     └── [Save & Open]     → ✅ SO created (Open)
│   │           ├── Available for Operations dispatch
│   │           └── Appears in Module 19 → Tab 2 (Billing Log)
│   │           └── Appears in Module 18 → Tab 3 (SO History)
│   │
│   ├── [View] → 20.3 View SO Details
│   │     ├── Tab 1: SO Summary & Line Items
│   │     │     └── Complete order breakdown with HSN/SAC, taxes, totals
│   │     └── Tab 2: Execution & Delivery Status
│   │           ├── Job Card tracker (services)
│   │           └── Delivery Challan tracker (products)
│   │
│   ├── [Edit] → 20.4 Edit SO Form
│   │     ├── Blocked if: Status = Fulfilled/Billed, Job Cards exist
│   │     ├── Modify: Qty, Unit Price, Chemical substitution, Discount
│   │     └── Recalculates totals on save
│   │
│   └── [Cancel] → 20.5 Cancellation Dialog
│         ├── Blocked if: Job Cards exist, Challans dispatched, Invoiced
│         ├── Reason Code + Remarks
│         └── SO status → Cancelled; billing period freed (Contract)
│
│  ═══ DOWNSTREAM HANDOFF ═══════════════════════════════════════════
│  Once SO reaches "Open" status, the baton is passed to:
│  → Operations & Dispatch (Job Cards, Technician Routing)
│  → Inventory (Chemical / Product Issuance from Module 11)
│  → Invoicing & Billing
```

---

================================================================================

## Module Dependency Map

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                                                                                  │
│   MODULE 19           MODULE 17         MODULE 16         MODULE 18              │
│   Contract Mgmt       GMA Mgmt          Quotation Mgmt   Customer Mgmt          │
│   ┌──────────┐       ┌──────────┐      ┌──────────┐     ┌──────────┐           │
│   │ Active   │       │ Approved │      │ Approved │     │ Customer │           │
│   │ Contract │       │ GMA      │      │ Quotation│     │ ID, Name,│           │
│   │ (Svc Contract SO │       │ (One-Time│      │ (One-Time│     │ Sites,   │           │
│   │  source) │       │  source) │      │  source) │     │ Billing  │           │
│   └─────┬────┘       └─────┬────┘      └─────┬────┘     └─────┬────┘           │
│         │                  │                  │               │                 │
│         └──────────┐   ┌───┘       ┌──────────┘    ┌──────────┘                 │
│                    │   │           │               │                            │
│                    ▼   ▼           ▼               ▼                            │
│             ╔═══════════════════════════════════════════════════╗               │
│             ║       MODULE 20: SALES ORDER MANAGEMENT          ║               │
│             ║                                                    ║               │
│             ║  • Svc Contract Source: Active Contract (M19)            ║               │
│             ║  • One-Time Source: Approved GMA/Quotation (M17/16)║               │
│             ║  • Product Source: Product Master (M10)            ║               │
│             ║  • Customer & Sites (M18)                         ║               │
│             ║  • Tax: HSN/SAC codes (M9)                        ║               │
│             ║  • Line Items, Taxes, Totals                      ║               │
│             ╚═══════════════╤═══════════════════════════════════╝               │
│                             │                                                   │
│   MODULE 10                 │                                                   │
│   Product Master            ▼                                                   │
│   ┌──────────┐   ╔═══════════════════════════════════╗                          │
│   │ Products,│   ║  DOWNSTREAM CONSUMERS              ║                          │
│   │ Prices,  │──►║  • Operations & Dispatch (Job Cards)║                          │
│   │ HSN, UOM │   ║  • Inventory (Module 11 – Issuance) ║                          │
│   └──────────┘   ║  • Invoicing & Billing              ║                          │
│                  ╚═══════════════════════════════════╝                          │
│   MODULE 9                                                                      │
│   Tax Management                                                                │
│   ┌──────────┐                                                                  │
│   │ HSN/SAC  │────► Tax rate lookup for line items                              │
│   │ Tax rates│                                                                  │
│   └──────────┘                                                                  │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## Data Flow Table

| Source Module  | Data Provided                                                 | Used In (SO Management)                                      |
| -------------- | ------------------------------------------------------------- | ------------------------------------------------------------ |
| **Module 19**  | Active Contract (customer, sites, services, payment schedule) | Section 1: Svc Contract source; auto-generates SOs per billing period |
| **Module 17**  | Approved GMA (services, costs, sites, GM%)                    | Section 1: One-Time Service source                           |
| **Module 16**  | Approved Quotation (services, pricing)                        | Section 1: One-Time Service source                           |
| **Module 18**  | Customer ID, Name, Sites, Billing Address                     | Section 1: Customer details; Section 3: Delivery address     |
| **Module 10**  | Product Name, Code, UOM, Sale Price, HSN Code                 | Section 2: Product line items for Product Sale orders        |
| **Module 9**   | HSN/SAC codes, GST rates                                      | Section 2: Tax calculation on all line items                 |
| **Module 7**   | Branch list (Active branches)                                 | Section 1: Branch assignment                                 |
| **Module 20**  | SO Number, Status, Line Items, Execution Status               | Operations (Job Cards), Invoicing, Module 18 Tab 3, Module 19 Tab 2 |

---
================================================================================

# 📊 COMPLETE DATA FLOW DIAGRAM — MODULE 1 TO MODULE 20

> This diagram maps the **entire data lifecycle** across all 20 modules, organized by functional layer. Arrows show data direction. Conditional branches are marked with `[IF condition]`.

---

## LAYER 1: PLATFORM & AUTHENTICATION (Module 1 – 4)

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                              LAYER 1: PLATFORM & ACCESS CONTROL                              │
│                                                                                              │
│   ┌──────────────────────────────┐                                                           │
│   │     MODULE 1: AUTHENTICATION │                                                           │
│   │  (Entry Point for ALL users) │                                                           │
│   └──────────┬───────────────────┘                                                           │
│              │                                                                               │
│     ┌────────┼────────────────────────┐                                                      │
│     ▼        ▼                        ▼                                                      │
│  ┌─────────────┐  ┌───────────────┐  ┌──────────────┐                                       │
│  │ Super Admin │  │ Company Admin │  │ IAM User     │                                       │
│  │ (Seravion)  │  │ (Client)      │  │ (Employee)   │                                       │
│  └──────┬──────┘  └───────┬───────┘  └──────┬───────┘                                       │
│         │                 │                  │                                                │
│         │    [IF New]─────┤                  │                                                │
│         │        ▼        │[IF Existing]     │                                                │
│         │  ┌──────────┐   │                  │                                                │
│         │  │M2: ONBOARD│  │                  │                                                │
│         │  │Company    │  │                  │                                                │
│         │  │Info+Docs  │  │                  │                                                │
│         │  └─────┬─────┘  │                  │                                                │
│         │        │        │                  │                                                │
│         │  [IF Docs Approved by Seravion]    │                                                │
│         │        │        │                  │                                                │
│         │        ▼        │                  │                                                │
│         │  ┌──────────┐   │                  │                                                │
│         │  │M2.8: Sub-│   │                  │                                                │
│         │  │scription │   │                  │                                                │
│         │  │Purchase  │◄──┼──── M4: Subscription Plans (Seravion-created)                    │
│         │  └─────┬─────┘  │                  │                                                │
│         │        │        │                  │                                                │
│         │  [IF Payment Success]              │                                                │
│         │        │        │                  │                                                │
│         │        ▼        ▼                  ▼                                                │
│         │     Company Dashboard ──────►  Role-Based Module Access                            │
│         │                                                                                    │
│         ▼                                                                                    │
│  ┌──────────────────┐                                                                        │
│  │M3: SUPER ADMIN   │                                                                        │
│  │• Company Approval│──► Approves/Rejects company docs (M2)                                  │
│  │• Plan Management │──► Creates plans used in M2.8                                          │
│  │• Role Templates  │──► Base templates for M5 (Client)                                      │
│  └──────────────────┘                                                                        │
│                                                                                              │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## LAYER 2: SYSTEM CONFIGURATION (Module 5 – 9)

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                         LAYER 2: SYSTEM CONFIGURATION                                        │
│                                                                                              │
│  ┌────────────────┐     ┌────────────────┐     ┌────────────────┐                            │
│  │  M5: ROLE MGMT │     │ M6: ROLE SALARY│     │ M7: BRANCH     │                            │
│  │  (Permissions) │     │ & LEAVE CONFIG │     │ MANAGEMENT     │                            │
│  └───────┬────────┘     └───────┬────────┘     └───────┬────────┘                            │
│          │                      │                      │                                     │
│          │  Seravion creates    │  Defines salary &    │  Defines physical                   │
│          │  role templates ──►  │  leave policy per    │  branches, locations                 │
│          │  Client customizes   │  role ──────────►    │  & contacts ──────►                  │
│          │         │            │         │            │         │                            │
│          └─────────┼────────────┘─────────┼────────────┘─────────┤                            │
│                    │                      │                      │                            │
│                    ▼                      ▼                      ▼                            │
│          ┌─────────────────────────────────────────────────────────────────┐                  │
│          │                  MODULE 8: EMPLOYEE MANAGEMENT                  │                  │
│          │                                                                 │                  │
│          │  ┌─ Step 1: Personal Details                                    │                  │
│          │  ├─ Step 2: Role Assignment ◄──── M5 (permissions auto-load)    │                  │
│          │  ├─ Step 3: Salary Config  ◄──── M6 (salary/leave auto-load)   │                  │
│          │  ├─ Step 4: Leave Config   ◄──── M6 (leave balance auto-load)  │                  │
│          │  ├─ Step 5: Documents                                           │                  │
│          │  └─ Step 6: Branch Assign  ◄──── M7 (branch list)              │                  │
│          │                                                                 │                  │
│          │  OUTPUT: Active employees with roles, branches, permissions     │                  │
│          └──────────────────────────┬──────────────────────────────────────┘                  │
│                                     │                                                        │
│                                     │  Employees used by:                                    │
│                                     ├──► M15 (Lead assignment)                               │
│                                     ├──► M17 (GMA prepared by)                               │
│                                     ├──► M19 (Technician assignment)                         │
│                                     └──► M20 (Sales person assignment)                       │
│                                                                                              │
│  ┌────────────────────────────────────────────────────────────────────────┐                   │
│  │                    MODULE 9: TAX MANAGEMENT                            │                   │
│  │                                                                        │                   │
│  │  Tax Types (CGST, SGST, IGST, CESS)                                    │                   │
│  │       │                                                                │                   │
│  │       ▼                                                                │                   │
│  │  HSN/SAC Codes (linked to tax rates)                                   │                   │
│  │       │                                                                │                   │
│  │       │  Tax rates auto-populate to ──►                                │                   │
│  │       ├──► M10 (Products — purchase/selling price tax calc)            │                   │
│  │       ├──► M14 (Purchase Orders — PO line item tax)                    │                   │
│  │       ├──► M16 (Quotations — quoted price tax)                         │                   │
│  │       ├──► M17 (GMA — chemical cost tax)                               │                   │
│  │       ├──► M19 (Contracts — contract value tax)                        │                   │
│  │       └──► M20 (Sales Orders — SO line item tax)                       │                   │
│  └────────────────────────────────────────────────────────────────────────┘                   │
│                                                                                              │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## LAYER 3: OPERATIONAL MASTERS (Module 10 – 14)

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                         LAYER 3: OPERATIONAL MASTERS                                         │
│                                                                                              │
│  ┌─────────────────────────────────────────────────────────────┐                              │
│  │               MODULE 10: PRODUCT MASTER                      │                              │
│  │  (Prerequisite: M9 Tax Management)                           │                              │
│  │                                                              │                              │
│  │  Product Name, Code, Category, Brand, UOM, HSN              │                              │
│  │  Purchase Price, Selling Price, Variants                    │                              │
│  │  Tax Config ◄──── auto-filled from M9 HSN codes            │                              │
│  └──────────────────────────┬──────────────────────────────────┘                              │
│                             │                                                                │
│            ┌────────────────┼────────────────┬──────────────────────┐                         │
│            ▼                ▼                ▼                      ▼                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     ┌──────────────┐                   │
│  │ M11: STOCK   │  │ M12: SERVICE │  │ M14: PURCHASE│     │ M20: SALES   │                   │
│  │ MANAGEMENT   │  │ MANAGEMENT   │  │ ORDER (PO)   │     │ ORDER (SO)   │                   │
│  │              │  │              │  │              │     │ (Product     │                   │
│  │ Assets,      │  │ Service      │  │ PO creates   │     │  Sale mode)  │                   │
│  │ Consumables, │  │ Templates,   │  │ stock on     │     │              │                   │
│  │ Resell stock │  │ Chemicals,   │  │ receipt      │     │ Uses Selling │                   │
│  │              │  │ Pricing      │  │              │     │ Price from   │                   │
│  │              │  │              │  │              │     │ Product Mstr │                   │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘     └──────────────┘                   │
│         │                 │                 │                                                 │
│         │                 │   ┌─────────────┘                                                │
│         │                 │   │                                                               │
│         │                 │   ▼                                                               │
│         │                 │  ┌──────────────┐                                                │
│         │                 │  │ M13: VENDOR  │                                                │
│         │                 │  │ MANAGEMENT   │                                                │
│         │                 │  │              │                                                │
│         │                 │  │ Vendor list  │                                                │
│         │                 │  │ used in PO   │                                                │
│         │                 │  │ creation     │                                                │
│         │                 │  └──────────────┘                                                │
│         │                 │                                                                  │
│   Stock issuance     Service config                                                          │
│   for SO execution   used in GMA, Quotation,                                                 │
│   (chemicals,        Contract, and SO                                                        │
│    products)                                                                                 │
│                                                                                              │
│  ═══ DATA FLOW: PROCUREMENT CHAIN ═══════════════════════════════════════                    │
│  M10 (Product) ──► M13 (Vendor maps to products) ──► M14 (PO created)                       │
│       ──► [IF PO Received] ──► M11 (Stock added to Central/Branch)                           │
│                                                                                              │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## LAYER 4: SALES PIPELINE (Module 15 – 20)

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                      LAYER 4: SALES PIPELINE (LEAD → SALES ORDER)                            │
│                                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────────────────────┐│
│  │                          MODULE 15: LEAD MANAGEMENT                                      ││
│  │                                                                                          ││
│  │  Sources: Website, Referral, Walk-in, Cold Call, Social Media, Exhibition, Partner       ││
│  │  Lead Status: New → Contacted → Qualified → Negotiation → Won / Lost                    ││
│  │                                                                                          ││
│  │  [IF Lead Status = QUALIFIED or WON]:                                                    ││
│  │       │                                                                                  ││
│  │       ├──► Route 1: Create Quotation    ──► MODULE 16                                    ││
│  │       ├──► Route 2: Create GMA Sheet    ──► MODULE 17                                    ││
│  │       └──► Route 3: Convert to Customer ──► MODULE 18                                    ││
│  │                                                                                          ││
│  └──────────────────────────────────────────────────────────────────────────────────────────┘│
│                    │                    │                    │                                │
│                    ▼                    ▼                    ▼                                │
│  ┌──────────────────────┐  ┌──────────────────────┐  ┌──────────────────────┐                │
│  │ M16: QUOTATION MGMT  │  │ M17: GMA (GROSS      │  │ M18: CUSTOMER MGMT  │                │
│  │                      │  │ MARGIN ANALYSIS)     │  │                      │                │
│  │ Source:              │  │                      │  │ Source:              │                │
│  │ • From Lead (M15)    │  │ Source:              │  │ • From Lead (M15)   │                │
│  │ • From Customer (M18)│  │ • From Lead (M15)    │  │ • Manual Add        │                │
│  │ • Add New Prospect   │  │ • From Customer(M18) │  │                      │                │
│  │                      │  │ • Add New            │  │ 360° view:          │                │
│  │ Uses:                │  │                      │  │ • Sites             │                │
│  │ • M12 Service Catalog│  │ Uses:                │  │ • Contracts (M19)   │                │
│  │ • M10 Product Master │  │ • M10 Chemicals      │  │ • SOs (M20)         │                │
│  │ • M9  Tax Config     │  │ • M12 Services       │  │ • History           │                │
│  │                      │  │ • M9  Tax Config     │  │                      │                │
│  │ Status Flow:         │  │                      │  │ Provides to:        │                │
│  │ Draft → Sent →       │  │ Approval Matrix:     │  │ • M17 (GMA source)  │                │
│  │ Viewed → Accepted    │  │ GM ≥ 40% → Auto ✅   │  │ • M19 (Contract     │                │
│  │ → Rejected/Expired   │  │ GM 20-39% → Mgr 🟡  │  │        prerequisite)│                │
│  │                      │  │ GM < 20% → CEO 🔴   │  │ • M20 (SO customer) │                │
│  └──────────┬───────────┘  └──────────┬───────────┘  └──────────┬───────────┘                │
│             │                         │                         │                            │
│  [IF Accepted]             [IF Approved]                        │                            │
│             │                         │                         │                            │
│             │   ┌─────────────────────┘                         │                            │
│             │   │                                               │                            │
│             ▼   ▼                                               │                            │
│  ╔══════════════════════════════════════════════════════════════╗│                            │
│  ║              MODULE 19: CONTRACT MANAGEMENT                  ║│                            │
│  ║                                                              ║│                            │
│  ║  [PREREQUISITE: Approved GMA (M17) + Active Customer (M18)] ║│                            │
│  ║                                                              ║│                            │
│  ║  Auto-inherits 80% data from GMA:                            ║│                            │
│  ║  • Customer & Sites (from M18 via M17)                       ║│                            │
│  ║  • Services & Pricing (from M17 GMA)                         ║│                            │
│  ║  • Chemicals & Quantities (from M17 GMA → M10/M12)          ║│                            │
│  ║  • Tax Config (from M9)                                      ║│                            │
│  ║                                                              ║│                            │
│  ║  Status: Draft → Active → Expiring Soon → Expired/Terminated ║│                            │
│  ║                                                              ║│                            │
│  ║  Generates Payment Schedule (Billing Periods)                ║│                            │
│  ║            │                                                 ║│                            │
│  ║  [IF Contract Status = ACTIVE]                               ║│                            │
│  ║            │                                                 ║│                            │
│  ║            ▼                                                 ║│                            │
│  ║  AUTO-TRIGGERS: Sales Order generation per billing period    ║│                            │
│  ╚════════════════════════════════════╦═════════════════════════╝│                            │
│                                       ║                          │                            │
│                                       ▼                          │                            │
│  ╔════════════════════════════════════════════════════════════════════════════╗                │
│  ║                    MODULE 20: SALES ORDER MANAGEMENT                      ║                │
│  ║                                                                           ║                │
│  ║  ORDER TYPE DETERMINES SOURCE & LINE ITEMS:                               ║                │
│  ║                                                                           ║                │
│  ║  ┌─────────────────────────────────────────────────────────────────────┐  ║                │
│  ║  │ [IF Order Type = SERVICE CONTRACT]                                  │  ║                │
│  ║  │   Source: Active Contract (M19) + Billing Period                    │  ║                │
│  ║  │   Auto-Generate: System cron creates SO per billing period          │  ║                │
│  ║  │   Line Items: Site-wise nested services (from Contract)             │  ║                │
│  ║  │   Chemicals: Read-only (auto-fetched from Contract/GMA)             │  ║                │
│  ║  │   Pricing: Locked to contract terms                                 │  ║                │
│  ║  └─────────────────────────────────────────────────────────────────────┘  ║                │
│  ║                                                                           ║                │
│  ║  ┌─────────────────────────────────────────────────────────────────────┐  ║                │
│  ║  │ [IF Order Type = ONE-TIME SERVICE]                                  │  ║                │
│  ║  │                                                                     │  ║                │
│  ║  │   [IF Source = Approved Quotation/GMA]                              │  ║                │
│  ║  │     Auto-populate sites & services from M16/M17                     │  ║                │
│  ║  │     Chemicals: Read-only (from approved source)                     │  ║                │
│  ║  │     Pricing: From approved source                                   │  ║                │
│  ║  │                                                                     │  ║                │
│  ║  │   [IF Source = Standalone (Customer only)]                          │  ║                │
│  ║  │     Manual: Add sites & services freely                             │  ║                │
│  ║  │     Chemicals: N/A (no source to pull from)                         │  ║                │
│  ║  │     Pricing: Manual entry by sales person                           │  ║                │
│  ║  └─────────────────────────────────────────────────────────────────────┘  ║                │
│  ║                                                                           ║                │
│  ║  ┌─────────────────────────────────────────────────────────────────────┐  ║                │
│  ║  │ [IF Order Type = PRODUCT SALE]                                      │  ║                │
│  ║  │   Source: Customer record (M18) — Standalone                        │  ║                │
│  ║  │   Line Items: Product selection from M10 (Product Master)           │  ║                │
│  ║  │   Pricing: Defaults to Selling Price from M10                       │  ║                │
│  ║  │            [IF User overrides] → Custom manual price accepted       │  ║                │
│  ║  │   Tax: Auto-calculated from HSN (M9)                                │  ║                │
│  ║  └─────────────────────────────────────────────────────────────────────┘  ║                │
│  ║                                                                           ║                │
│  ║  SO Status: Draft → Open → Fulfilled → Billed → Cancelled                ║                │
│  ║                                                                           ║                │
│  ║  [IF Status = OPEN] ──► Triggers Operations & Dispatch (Module 21+)       ║                │
│  ║  [IF Status = FULFILLED] ──► Triggers Invoicing & Billing                 ║                │
│  ║  [IF Status = CANCELLED] ──► Frees billing period (for Contract SOs)      ║                │
│  ╚═══════════════════════════════════════════════════════════════════════════╝                │
│                                                                                              │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## FULL SYSTEM — INTER-MODULE DATA DEPENDENCY MAP

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                         MODULE DEPENDENCY MAP (1 → 20)                                       │
│                                                                                              │
│  ┌─────┐                                                                                     │
│  │ M1  │─── Auth ───► All Modules (session, role, tenant context)                            │
│  └──┬──┘                                                                                     │
│     │                                                                                        │
│     ├──► ┌─────┐──► ┌─────┐──► ┌─────┐                                                      │
│     │    │ M2  │    │ M4  │    │ M3  │ (Seravion: Plans, Approvals, Role Templates)          │
│     │    └──┬──┘    └─────┘    └─────┘                                                      │
│     │       │                                                                                │
│     │       ▼                                                                                │
│     │    ┌─────┐    ┌─────┐    ┌─────┐                                                      │
│     │    │ M5  │──►│ M6  │    │ M7  │                                                      │
│     │    │Role │    │Sal/ │    │Brch │                                                      │
│     │    └──┬──┘    │Leav │    └──┬──┘                                                      │
│     │       │       └──┬──┘       │                                                          │
│     │       └─────┬────┘──────────┘                                                          │
│     │             ▼                                                                          │
│     │          ┌─────┐                                                                       │
│     │          │ M8  │ Employee Management                                                   │
│     │          └──┬──┘                                                                       │
│     │             │                                                                          │
│     │    ┌────────┼────────┬────────┬────────┐                                               │
│     │    ▼        ▼        ▼        ▼        ▼                                               │
│     │  ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐                                             │
│     │  │ M9  │ │ M15 │ │ M17 │ │ M19 │ │ M20 │                                             │
│     │  │Tax  │ │Lead │ │GMA  │ │Cont.│ │ SO  │                                             │
│     │  └──┬──┘ └──┬──┘ └─────┘ └─────┘ └─────┘                                             │
│     │     │       │                                                                          │
│     │     ▼       │                                                                          │
│     │  ┌─────┐    │                                                                          │
│     │  │ M10 │    │  ┌────────────────────────────────────────────────────────┐              │
│     │  │Prod │    │  │  SALES PIPELINE FLOW:                                  │              │
│     │  └──┬──┘    │  │                                                        │              │
│     │     │       │  │  M15 (Lead)                                            │              │
│     │     ▼       │  │    │                                                   │              │
│     │  ┌─────┐    │  │    ├─[IF Qualified]──► M16 (Quotation)                 │              │
│     │  │ M11 │    │  │    ├─[IF Qualified]──► M17 (GMA)                       │              │
│     │  │Stock│    │  │    └─[IF Won]────────► M18 (Customer)                  │              │
│     │  └──┬──┘    │  │                             │                          │              │
│     │     │       │  │  M16 ─[IF Accepted]──► M19 (Contract)                  │              │
│     │     │       │  │  M17 ─[IF Approved]──► M19 (Contract)  ◄── M18 (Cust) │              │
│     │     │       │  │                             │                          │              │
│     │     │       │  │  M19 ─[IF Active]────► M20 (Sales Order)               │              │
│     │     │       │  │  M16 ─[IF Approved]──► M20 (One-Time SO)               │              │
│     │     │       │  │  M17 ─[IF Approved]──► M20 (One-Time SO)               │              │
│     │     │       │  │  M18 ─[Direct]───────► M20 (Product Sale / Standalone) │              │
│     │     │       │  │  M10 ─[Products]─────► M20 (Product Line Items)        │              │
│     │     │       │  │                                                        │              │
│     │     │       │  └────────────────────────────────────────────────────────┘              │
│     │     │       │                                                                          │
│  ┌──┴─────┴───────┴──────────────────────────────────────────────────────────┐               │
│  │   PROCUREMENT CHAIN:                                                       │               │
│  │   M10 (Products) ──► M13 (Vendor Mapping) ──► M14 (Purchase Order)        │               │
│  │       ──► [IF PO Received] ──► M11 (Stock Receipt)                         │               │
│  │   M12 (Services) ──► Links chemicals from M10 to service templates        │               │
│  └────────────────────────────────────────────────────────────────────────────┘               │
│                                                                                              │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## CONDITIONAL DATA FLOW SUMMARY TABLE

| Condition                                  | Source Module | Data Sent                                     | Target Module | Action                                          |
| ------------------------------------------ | ------------ | --------------------------------------------- | ------------- | ----------------------------------------------- |
| Lead Status = Qualified                    | M15          | Lead details, contact info                    | M16, M17      | Create Quotation or GMA sheet                   |
| Lead Status = Won                          | M15          | Full lead record                              | M18           | Convert to Customer                             |
| Quotation Status = Accepted                | M16          | Quoted services, pricing, sites               | M19           | Trigger Contract creation                       |
| GMA Status = Approved (GM ≥ 40%)          | M17          | Full GMA sheet (auto-approved)                | M19           | Create Contract (80% data auto-inherited)       |
| GMA Status = Approved (GM < 40%)          | M17          | GMA sheet (manually approved by Mgr/CEO)      | M19           | Create Contract after approval                  |
| Contract Status = Active                   | M19          | Contract terms, billing periods, sites        | M20           | Auto-generate SO per billing period (cron)      |
| Contract Billing Period = Unbilled         | M19          | Billing period details                        | M20           | SO created for that specific period             |
| SO Order Type = Service Contract           | M20          | Uses Contract (M19) as source                 | Operations    | Site-wise service execution                     |
| SO Order Type = One-Time + Linked          | M20          | Uses Quotation (M16) or GMA (M17) as source   | Operations    | One-time service dispatch                       |
| SO Order Type = One-Time + Standalone      | M20          | Manual entry, Customer (M18) reference         | Operations    | Standalone service                              |
| SO Order Type = Product Sale               | M20          | Products from M10, Customer from M18           | Inventory     | Product dispatch from M11 stock                 |
| SO Status = Open                           | M20          | SO details, line items                        | M21+          | Job Card generation, dispatch                   |
| SO Status = Fulfilled                      | M20          | Completed SO                                  | Billing       | Invoice generation                              |
| SO Status = Cancelled                      | M20          | Cancellation record                           | M19           | Free billing period on Contract                 |
| PO Status = Received                       | M14          | Received items, quantities                    | M11           | Add to Central/Branch stock                     |
| Product Created/Updated                    | M10          | Product details, HSN, prices                  | M11,M14,M20   | Available for stock, PO, and SO                 |
| HSN Code Created/Updated                   | M9           | Tax rates, HSN code                           | M10,M16-M20   | Auto-populate tax on all transactions           |

---

