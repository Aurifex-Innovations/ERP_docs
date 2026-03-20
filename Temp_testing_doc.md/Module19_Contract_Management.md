# 🎯 MODULE 19: CONTRACT MANAGEMENT

## Overview

A transactional module handling the legal and service-level agreements (SLA) for recurring (AMC) and one-time services. Uses the approved GMA sheet (Module 17) to lock in commercial terms — auto-inheriting 80% of the contract data — reducing manual entry and ensuring pricing integrity. Manages the full contract lifecycle from Draft through Active, Expiring, Expired, and Terminated statuses.

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
│  │  Service Type      : Cockroach Treatment (Read-only)                   │ │
│  │  Service Mode      : Contract (AMC) (Read-only)                        │ │
│  │  No. of Sites      : 3 (Read-only)                                     │ │
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
│  │  │SITE-00314│ Showroom    │ Bandra West      │  1,200   │ Commercial │ │ │
│  │  └──────────┴─────────────┴──────────────────┴──────────┴────────────┘ │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 2: CONTRACT TERMS                                                   │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Contract Duration*  : [▼ 6 Months / 1 Year / 2 Years / 3 Years / Custom ▼] │ │
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
│  SECTION 3: SERVICE SCHEDULE                                                 │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Service Frequency*  : [▼ Weekly / Fortnightly / Monthly / Quarterly / Custom ▼] │ │
│  │                        (Pre-filled from GMA Section 2)                 │ │
│  │                                                                         │ │
│  │  Preferred Service Days:                                                │ │
│  │  [☐ Mon] [☐ Tue] [☑ Wed] [☐ Thu] [☐ Fri] [☑ Sat] [☐ Sun]            │ │
│  │                                                                         │ │
│  │  Preferred Time Slot  : [▼ Morning (8–12) / Afternoon (12–5) /        │ │
│  │                           Evening (5–8) / Night (8–11) / Any ▼]       │ │
│  │                                                                         │ │
│  │  Assigned Technician Group*: [🔍 Search / Select Team ▼]              │ │
│  │                         (Teams from Module 8 – Employee Management)    │ │
│  │                                                                         │ │
│  │  SERVICE SITE SCHEDULE (Per Site):                                      │ │
│  │  ┌──────────┬─────────────┬───────────┬──────────────┬────────────────┐│ │
│  │  │ Site ID  │ Site Name   │ Frequency │ Service Days │ Time Slot      ││ │
│  │  │──────────┼─────────────┼───────────┼──────────────┼────────────────││ │
│  │  │SITE-00312│ Head Office │ Weekly    │ Wed, Sat     │ Morning (8–12) ││ │
│  │  │SITE-00313│ Warehouse   │ Monthly   │ 1st Monday   │ Afternoon      ││ │
│  │  │SITE-00314│ Showroom    │ Weekly    │ Wed          │ Evening (5–8)  ││ │
│  │  └──────────┴─────────────┴───────────┴──────────────┴────────────────┘│ │
│  │  (Each site can override the global frequency / days / time)           │ │
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
│  │                                                                         │ │
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
│  │  IF MILESTONE-BASED:                                                    │ │
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
│  │                            Annually / On Milestone ▼]                  │ │
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
| Service Type       | Auto-filled     | System   | Read-only; pest / service type from GMA                                  | e.g., Cockroach, Rodent, General Pest          |
| Service Mode       | Auto-filled     | System   | Read-only; AMC or One-Time from GMA                                      | Determines contract structure                  |
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

## Section 3: Service Schedule Fields

| Field                    | Type            | Required | Options / Validation                                                 | Notes                                            |
| ------------------------ | --------------- | -------- | -------------------------------------------------------------------- | ------------------------------------------------ |
| Service Frequency        | Dropdown        | Yes      | Weekly / Fortnightly / Monthly / Quarterly / Custom                  | Pre-filled from GMA; editable                    |
| Preferred Service Days   | Multi-checkbox  | No       | Mon / Tue / Wed / Thu / Fri / Sat / Sun                              | Default days for service visits                  |
| Preferred Time Slot      | Dropdown        | No       | Morning (8–12) / Afternoon (12–5) / Evening (5–8) / Night (8–11) / Any | Default time window for visits                  |
| Assigned Technician Group| Search Dropdown | Yes      | Active technician teams from Module 8                                | Team assigned for service execution              |

### Site-Level Schedule Override

Each site within the contract can have its own schedule that overrides the global settings:

| Field              | Type           | Required | Options / Validation                                 | Notes                                        |
| ------------------ | -------------- | -------- | ---------------------------------------------------- | -------------------------------------------- |
| Site ID            | Display        | System   | Read-only; inherited from GMA                        | Site reference                               |
| Site Name          | Display        | System   | Read-only                                            | Friendly site label                          |
| Frequency          | Dropdown       | No       | Same as global; defaults to global frequency         | Per-site override                            |
| Service Days       | Multi-checkbox | No       | Mon – Sun                                            | Specific days for this site                  |
| Time Slot          | Dropdown       | No       | Same options as global                               | Specific time window for this site           |

> **Note:** Technician teams are derived from **Module 8 (Employee Management)**. Only active teams assigned to the contract's branch are shown in the dropdown.

---

## Section 4: Payment & Commercial Terms Fields

| Field                  | Type     | Required | Options / Validation                                                              | Notes                                            |
| ---------------------- | -------- | -------- | --------------------------------------------------------------------------------- | ------------------------------------------------ |
| Payment Schedule Type  | Dropdown | Yes      | 100% Advance / Monthly Post-paid / Quarterly Post-paid / Half-Yearly / Milestone-based / Custom | Determines payment grid structure   |
| Payment Due Date       | Date     | Cond.    | Required if 100% Advance; must be ≤ Start Date                                    | Single payment date                              |
| Payment Grid           | Table    | Cond.    | Auto-generated for Quarterly/Monthly/Half-Yearly; manual for Milestone/Custom     | Must sum to Total Sale Value                     |
| Invoicing Frequency    | Dropdown | Yes      | Monthly / Quarterly / Half-Yearly / Annually / On Milestone                       | Determines invoice generation cadence            |
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
| Payment Sum = Total Value            | Sum of all payment amounts must equal the Total Sale Value             |
| Technician Group Required           | At least one active technician team must be assigned                  |
| Service Frequency Required          | Must have a valid service frequency                                   |
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
│  GMA Ref : GMA-00091                        │  Service: Cockroach Treatment  │
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
│  ─── CONTRACT TERMS ──────────────────────────────────────────────────────  │
│  Contract ID    : CON-2026-0041            Duration    : 1 Year             │
│  Start Date     : 01 Jan 2026             End Date    : 31 Dec 2026         │
│  Total Value    : ₹ 2,04,000              Renewal Type: Manual              │
│  Contract Ref   : REF-INT-2026-041        GM%         : 40.7%               │
│                                                                              │
│  ─── PAYMENT SCHEDULE ────────────────────────────────────────────────────  │
│  Payment Type: Quarterly Post-paid                                           │
│  Invoicing Frequency: Quarterly                                              │
│  ┌──────────┬──────────────┬──────────────┬──────────────┬──────────┐       │
│  │ Period   │ Duration     │ Amount (₹)   │ Due Date     │ Status   │       │
│  │──────────┼──────────────┼──────────────┼──────────────┼──────────│       │
│  │ Q1       │ Jan – Mar    │ ₹ 51,000     │ 31 Mar 2026  │ ✅ Paid  │       │
│  │ Q2       │ Apr – Jun    │ ₹ 51,000     │ 30 Jun 2026  │ 🟡 Due   │       │
│  │ Q3       │ Jul – Sep    │ ₹ 51,000     │ 30 Sep 2026  │ ⏳ Upcoming│     │
│  │ Q4       │ Oct – Dec    │ ₹ 51,000     │ 31 Dec 2026  │ ⏳ Upcoming│     │
│  └──────────┴──────────────┴──────────────┴──────────────┴──────────┘       │
│                                                                              │
│  Legal SLA Notes: "Response within 4 hours for emergency pest issues.       │
│   Re-treatment guaranteed within 48 hours if infestation recurrence."       │
│                                                                              │
│  ─── SITES & SERVICES GRID ──────────────────────────────────────────────  │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Site ID  │Site Name   │Address           │SQFT │Pest Type  │Frequency    ││
│  │─────────┼────────────┼──────────────────┼─────┼───────────┼─────────────││
│  │SITE-312 │Head Office │Andheri E., Mumbai│3,500│Cockroach  │Weekly       ││
│  │SITE-313 │Warehouse   │Bhiwandi, Thane   │8,000│Cockroach  │Monthly      ││
│  │SITE-314 │Showroom    │Bandra W., Mumbai │1,200│Cockroach  │Weekly       ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Service Days   │Time Slot       │Technician Group │Site Cost/Yr│Site GM% ││
│  │───────────────┼────────────────┼─────────────────┼────────────┼─────────││
│  │Wed, Sat       │Morning (8–12)  │Team Alpha       │₹ 92,992    │40.4%   ││
│  │1st Monday     │Afternoon       │Team Beta        │₹ 28,000    │41.7%   ││
│  │Wednesday      │Evening (5–8)   │Team Alpha       │₹ 15,200    │38.9%   ││
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Contract Terms Fields (Read-Only)

| Field           | Type     | Description                                              |
| --------------- | -------- | -------------------------------------------------------- |
| Contract ID     | Display  | Unique contract identifier                               |
| Start Date      | Date     | Contract commencement                                    |
| End Date        | Date     | Contract expiry                                          |
| Duration        | Display  | e.g., 1 Year, 6 Months                                  |
| Total Value (₹) | Currency | Total agreed sale value                                  |
| Renewal Type    | Display  | Auto-Renew / Manual / Non-Renewable                      |
| Contract Ref    | Display  | Optional internal reference number                       |
| GM%             | Display  | Gross margin from GMA                                    |

---

## Payment Schedule Fields (Read-Only)

| Field            | Type     | Description                                             |
| ---------------- | -------- | ------------------------------------------------------- |
| Payment Type     | Display  | 100% Advance / Quarterly / Monthly / Milestone / Custom |
| Invoicing Freq.  | Display  | Billing cadence                                         |
| Period / Label   | Display  | Quarter / Month / Milestone identifier                  |
| Duration         | Display  | Date range for the period                               |
| Amount (₹)       | Currency | Installment amount                                      |
| Due Date         | Date     | Payment due date                                        |
| Payment Status   | Badge    | Paid / Due / Overdue / Upcoming                          |

---

## Sites & Services Grid Fields (Read-Only)

| Field            | Type     | Description                                              |
| ---------------- | -------- | -------------------------------------------------------- |
| Site ID          | Display  | Unique site reference from Module 18                     |
| Site Name        | Display  | Friendly label                                           |
| Address          | Display  | Physical service address                                 |
| Area (SQFT)      | Number   | Service coverage area                                    |
| Pest Type        | Display  | Pest / service category from GMA                         |
| Frequency        | Display  | Service visit frequency (site-level)                     |
| Service Days     | Display  | Specific days of service                                 |
| Time Slot        | Display  | Preferred time window                                    |
| Technician Group | Display  | Assigned team from Module 8                              |
| Site Cost/Year   | Currency | Total annual cost for this site (from GMA)               |
| Site GM%         | Display  | Gross margin for this site                               |

> **Note:** All data in this tab is derived from the **GMA sheet (Module 17)** and the **contract creation form (19.2)**. Modifications can only be made via the **Edit / Amend** form (19.4).

---

================================================================================

# 19.3.2 Tab 2: Sales Order Schedule (Billing Log)

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
│  │  Service Type   : Cockroach Treatment                                   │ │
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
│  SECTION 3: AMEND SERVICE SCHEDULE (Editable)                                │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Service Frequency   : [▼ Weekly ▼] (Editable)                         │ │
│  │  Preferred Days      : [☐ Mon][☐ Tue][☑ Wed][☐ Thu][☐ Fri][☑ Sat]     │ │
│  │  Preferred Time Slot : [▼ Morning ▼]                                    │ │
│  │  Technician Group    : [🔍 Team Alpha ▼]                               │ │
│  │                                                                         │ │
│  │  Site Schedule (site-level overrides editable):                          │ │
│  │  ┌──────────┬─────────────┬───────────┬──────────────┬──────────┐      │ │
│  │  │ Site ID  │ Site Name   │ Frequency │ Service Days │ Time     │      │ │
│  │  │──────────┼─────────────┼───────────┼──────────────┼──────────│      │ │
│  │  │SITE-00312│ Head Office │ [Weekly ▼]│ [Wed, Sat]   │ [Morning]│      │ │
│  │  │SITE-00313│ Warehouse   │ [Monthly▼]│ [1st Monday] │ [Afternoon]│    │ │
│  │  │SITE-00314│ Showroom    │ [Weekly ▼]│ [Wed]        │ [Evening]│      │ │
│  │  └──────────┴─────────────┴───────────┴──────────────┴──────────┘      │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 4: ADD NEW SITE TO CONTRACT (Optional)                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  [+ Add Site from Customer Sites]                                       │ │
│  │                                                                         │ │
│  │  IF ADDING A SITE:                                                      │ │
│  │  Select Site*  : [🔍 Search Customer Sites (Module 18) ▼]             │ │
│  │  Pest Type*    : [▼ Cockroach / Rodent / General Pest ▼]              │ │
│  │  Frequency*    : [▼ Weekly / Monthly ▼]                                │ │
│  │  Additional Cost/Year (₹)*: [________]                                 │ │
│  │  Additional Sale Price/Year (₹)*: [________]                           │ │
│  │                                                                         │ │
│  │  ⚠️ Adding a site increases the contract value and may require          │ │
│  │  re-approval if total change exceeds 10%.                               │ │
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

## Editable vs. Locked Fields

| Field                   | Editable? | Notes                                                            |
| ----------------------- | --------- | ---------------------------------------------------------------- |
| Contract ID             | ❌ Locked  | System-generated; immutable                                      |
| Customer / GMA Ref      | ❌ Locked  | Source data cannot be changed                                    |
| Service Type            | ❌ Locked  | To change service type, a new GMA/Contract must be created       |
| Original Value          | ❌ Locked  | Displayed as reference; cannot be changed                        |
| Start Date              | ❌ Locked  | Cannot modify once contract is Active                            |
| End Date                | ✅ Yes     | Can be extended (not shortened below today)                      |
| Revised Sale Value      | ✅ Yes     | Triggers re-approval if variance > 10%                           |
| Renewal Type            | ✅ Yes     | Can change renewal behavior                                      |
| Legal Notes / SLA       | ✅ Yes     | Free-text update                                                 |
| Service Frequency       | ✅ Yes     | Global or site-level frequency change                            |
| Service Days / Time     | ✅ Yes     | Schedule adjustment                                              |
| Technician Group        | ✅ Yes     | Can reassign technician team                                     |
| Add New Site            | ✅ Yes     | Can add from customer's existing sites (Module 18)               |
| Amendment Reason        | ✅ Required| Mandatory reason and optional remarks                            |

---

## Validation Rules

| Validation                         | Rule                                                              |
| ---------------------------------- | ----------------------------------------------------------------- |
| End Date > Today                   | Cannot set End Date to past or today                              |
| Revised Value > 0                  | Must remain a positive amount                                     |
| Value Variance Check               | If |Revised – Original| > 10% of Original, triggers re-approval  |
| Site must exist in Module 18       | New sites must be registered in the customer's record             |
| Additional Cost/Price > 0          | If adding a site, costs must be positive                          |
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
