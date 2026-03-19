# 🎯 MODULE 15: LEADS & FOLLOW-UP MANAGEMENT

## Overview

Comprehensive lead lifecycle management module that captures potential customer inquiries, tracks follow-up activities, and converts qualified leads into quotations or service contracts. Manages the complete journey from initial contact through qualification, nurturing, and conversion to GMA (General Measurement & Assessment) or direct quotation.

**Module Connections:**

- **Depends on:** Module 8 (Employee Management for lead assignment), Module 10 (Product/Service Master for quotation creation)
- **Used by:** Module 16 (GMA Management), Module 17 (Quotation Management), Module 18 (Contract Management)
- **Prerequisites:** Active employees with sales roles configured in Module 8

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
│  │ Lead Status:     [☑ New ☑ Contacted ☑ Qualified ☑ Proposal Sent        │  │
│  │                  ☑ Negotiation ☑ Won ☑ Lost ☑ On Hold]              │  │
│  │ Lead Source:     [☑ Website ☑ Referral ☑ Walk-in ☑ Cold Call         │  │
│  │                  ☑ Social Media ☑ Exhibition ☑ Partner]               │  │
│  │ Priority:        [☑ Low ☑ Normal ☑ High ☑ Urgent]                    │  │
│  │ Assigned To:     [▼ All Sales Reps ▼]                                 │  │
│  │ Property Type:   [☑ Residential ☑ Commercial ☑ Industrial]           │  │
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
│  │Lead ID│Lead Name│Contact Info│Property│Pest Type│Status│Priority│Next F/U│ │
│  │       │         │            │Type    │         │      │        │Date    │ │
│  │───────┼─────────┼────────────┼────────┼───────────┼──────┼────────┼────────│ │
│  │LD-001 │Rahul S. │98XXXX1234  │Res.    │Termite   │🟡    │High    │20-Mar  │ │
│  │       │         │rahul@...   │        │          │Cont. │        │        │ │
│  │LD-002 │Priya K. │99XXXX5678  │Comm.   │Cockroach │🟢    │Normal  │22-Mar  │ │
│  │       │         │priya@...   │        │          │Qual. │        │        │ │
│  │LD-003 │Amit V.  │97XXXX9012  │Ind.    │Rodent    │🔴    │Urgent  │Overdue │ │
│  │       │         │amit@...    │        │          │New   │        │18-Mar  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────┐│
│  │Source│Assigned To│Created Date│Last F/U Date│F/U Count│Actions         ││
│  │──────┼───────────┼────────────┼─────────────┼─────────┼────────────────││
│  │Website│Rajesh K.  │15-Mar-2026 │18-Mar-2026  │3        │[👁 View] [✏ Edit]│
│  │Refer. │Anita S.   │14-Mar-2026 │19-Mar-2026  │2        │[➕ Follow-up]    ││
│  │Walk-in│Rohit M.   │10-Mar-2026 │—            │0        │[👁 View] [✏ Edit]│
│  └──────────────────────────────────────────────────────────────────────────┘│
│                                                                              │
│  Pagination:  Previous   1   2   3   ...   10   Next                           │
│                                                                              │
│  Legend: 🟢 Qualified  🟡 Contacted  🔵 New  🟠 Proposal  ⚫ Won/Lost/On Hold  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table View Fields

| Field           | Type           | Required | Description                           |
| --------------- | -------------- | -------- | ------------------------------------- |
| Lead ID         | Text           | Auto     | Unique lead identifier (LD-YYYY-SEQ)  |
| Lead Name       | Text           | Yes      | Primary contact person name           |
| Contact Info    | Text           | Yes      | Mobile number and email (masked)      |
| Property Type   | Badge          | Yes      | Residential / Commercial / Industrial |
| Pest Type       | Multi Tag      | Yes      | Pest categories requiring service     |
| Status          | Status Badge   | Yes      | Current lead lifecycle stage          |
| Priority        | Priority Badge | Yes      | Low / Normal / High / Urgent          |
| Next Follow-up  | Date           | Auto     | Scheduled next follow-up date         |
| Lead Source     | Text           | Yes      | Origin of lead inquiry                |
| Assigned To     | Text           | Yes      | Sales representative responsible      |
| Created Date    | Date           | Auto     | Lead creation timestamp               |
| Last F/U Date   | Date           | Auto     | Most recent follow-up activity        |
| Follow-up Count | Number         | Auto     | Total follow-up attempts              |
| Actions         | Button Group   | —        | View / Edit / Add Follow-up           |

---

## Actions

| Action        | Icon | Description                                        | Available When      |
| ------------- | ---- | -------------------------------------------------- | ------------------- |
| View          | 👁   | Opens lead details in read-only mode (2 tabs)      | All statuses        |
| Edit          | ✏    | Opens lead edit form                               | All except Won/Lost |
| Add Follow-up | ➕   | Quick action to add follow-up without opening lead | All statuses        |

---

## Filters

| Filter        | Type         | Options                                                                        |
| ------------- | ------------ | ------------------------------------------------------------------------------ |
| Lead Status   | Multi-select | New / Contacted / Qualified / Proposal / Negotiation / Won / Lost / On Hold    |
| Lead Source   | Multi-select | Website / Referral / Walk-in / Cold Call / Social Media / Exhibition / Partner |
| Priority      | Multi-select | Low / Normal / High / Urgent                                                   |
| Assigned To   | Dropdown     | All active sales employees from Module 8                                       |
| Property Type | Multi-select | Residential / Commercial / Industrial                                          |
| Pest Type     | Multi-select | All pest types from Module 12                                                  |
| Date Range    | Date Range   | Created date filter                                                            |

---

## Search

Searchable by:

- Lead ID
- Lead Name
- Mobile Number
- Email Address
- Property Address (partial match)

---

# 15.2 Add Lead Form

**Description:**
Initial lead capture form for registering new customer inquiries. Captures essential contact information, service requirements, and initial qualification data to establish the lead record and trigger follow-up workflow.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           ADD NEW LEAD                                         │
│                                                                              │
│  [← Back to Lead List]                                            [Save Draft] │
│                                                                              │
│  SECTION 1: LEAD BASIC INFORMATION                                           │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Lead ID:              [AUTO GENERATED - Read Only]                    │ │
│  │  Lead Date:            [📅 Auto: Current Date]                          │ │
│  │  Lead Source*:         [▼ Website / Referral / Walk-in / Cold Call      │ │
│  │                         / Social Media / Exhibition / Partner ▼]       │ │
│  │  Priority*:            [▼ Low / Normal / High / Urgent ▼]              │ │
│  │  Assigned To*:         [▼ Sales Rep Dropdown ▼]                        │ │
│  │                         (Filtered: Active users with Sales role)       │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 2: CONTACT INFORMATION                                                │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Lead Name*:           [____________] (First + Last Name)               │ │
│  │  Mobile Number*:       [____________] (10 digits, unique check)         │ │
│  │  Alternate Number:     [____________] (Optional)                        │ │
│  │  Email ID:             [____________] (Valid email format)              │ │
│  │  WhatsApp Number:      [____________] (If different from mobile)       │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 3: PROPERTY & SERVICE DETAILS                                         │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Property Type*:       [▼ Residential / Commercial / Industrial ▼]    │ │
│  │                                                                              │
│  │  IF Residential:                                                        │ │
│  │  Property Sub-type:    [▼ 1BHK / 2BHK / 3BHK / 4BHK / Villa /          │ │
│  │                         Apartment / Independent House ▼]               │ │
│  │                                                                              │
│  │  IF Commercial/Industrial:                                            │ │
│  │  Property Sub-type:    [▼ Office / Retail / Warehouse / Factory /       │ │
│  │                         Restaurant / Hotel / Hospital / Other ▼]     │ │
│  │                                                                              │
│  │  Property Address*:    [____________________] (Full address)           │ │
│  │  Landmark:             [____________] (Nearby reference)              │ │
│  │  City*:                [____________]                                   │ │
│  │  State*:               [▼ State Dropdown ▼]                           │ │
│  │  Pincode*:             [____________] (6 digits)                      │ │
│  │                                                                              │
│  │  Pest Type Concern*:   [☑ Termite ☑ Cockroach ☑ Rodent ☑ Bed Bug     │ │
│  │                         ☑ Mosquito ☑ Ant ☑ Spider ☑ Fly ☑ Other]    │ │
│  │                                                                              │
│  │  Other Pest Specified: [____________] (Visible if Other checked)       │ │
│  │                                                                              │
│  │  Service Required*:    [▼ One-time / AMC (Annual) / Quarterly /       │ │
│  │                         Monthly / Inspection Only ▼]                   │ │
│  │                                                                              │
│  │  Preferred Date:       [📅 Date Picker] (Customer preferred service date)│ │
│  │  Preferred Time Slot:  [▼ Morning (9-12) / Afternoon (12-4) /          │ │
│  │                         Evening (4-7) / Anytime ▼]                     │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 4: ADDITIONAL INFORMATION                                           │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Budget Range:         [▼ <₹5K / ₹5K-10K / ₹10K-25K / ₹25K-50K /      │ │
│  │                         ₹50K+ / Not Discussed ▼]                       │ │
│  │                                                                              │
│  │  Current Provider:     [____________] (If any existing service)          │ │
│  │  Contract Expiry:      [📅 Date] (If known)                              │ │
│  │                                                                              │
│  │  Decision Maker:       [____________] (Name of authority)                │ │
│  │  Decision Maker Contact: [____________] (Phone/email if different)       │ │
│  │                                                                              │
│  │  Lead Description*:    [________________________________________]        │ │
│  │                         (Detailed requirement description)             │ │
│  │                         [Min 20 characters]                              │ │
│  │                                                                              │
│  │  Special Instructions: [________________________________________]        │ │
│  │                         (Access restrictions, pet concerns, etc.)       │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 5: REFERRAL INFORMATION (Visible if Lead Source = Referral)         │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Referred By:          [____________] (Person name)                     │ │
│  │  Referrer Contact:     [____________] (Phone/email)                   │ │
│  │  Referrer Type:        [▼ Existing Customer / Employee / Partner /      │ │
│  │                         Other ▼]                                       │ │
│  │  Referral Code:        [____________] (If applicable)                   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 6: SYSTEM FIELDS                                                      │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Created By:           [Auto: Current User]                             │ │
│  │  Created Date:         [Auto: System Timestamp]                         │ │
│  │  Status:               [Auto: NEW]                                       │ │
│  │  Next Follow-up Date:  [Auto: +2 days from creation]                    │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│                    [SAVE AS DRAFT]        [SUBMIT LEAD]        [CANCEL]       │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Section 1: Lead Basic Information Fields

| Field       | Type            | Required | Options/Validation                                                             | Notes                                      |
| ----------- | --------------- | -------- | ------------------------------------------------------------------------------ | ------------------------------------------ |
| Lead ID     | Auto Generated  | System   | Format: LD-YYYY-XXXXX (e.g., LD-2026-00042)                                    | Read-only, unique sequential               |
| Lead Date   | Date            | System   | Current date, editable if needed                                               | Lead creation date                         |
| Lead Source | Dropdown        | Yes      | Website / Referral / Walk-in / Cold Call / Social Media / Exhibition / Partner | Track origin                               |
| Priority    | Dropdown        | Yes      | Low / Normal / High / Urgent                                                   | Determines follow-up SLA                   |
| Assigned To | Search Dropdown | Yes      | Active employees with Sales/BD role from Module 8                              | Auto-assign based on territory/round-robin |

---

## Section 2: Contact Information Fields

| Field            | Type  | Required | Validation                                | Notes                    |
| ---------------- | ----- | -------- | ----------------------------------------- | ------------------------ |
| Lead Name        | Text  | Yes      | Min 3 characters, alphabets and spaces    | Primary contact person   |
| Mobile Number    | Phone | Yes      | Exactly 10 digits, unique across leads    | Primary contact number   |
| Alternate Number | Phone | No       | Exactly 10 digits, different from primary | Secondary contact        |
| Email ID         | Email | No       | Valid email format, unique check          | For email communications |
| WhatsApp Number  | Phone | No       | 10 digits                                 | If different from mobile |

---

## Section 3: Property & Service Details Fields

| Field                | Type               | Required    | Options/Validation                                         | Notes                            |
| -------------------- | ------------------ | ----------- | ---------------------------------------------------------- | -------------------------------- |
| Property Type        | Dropdown           | Yes         | Residential / Commercial / Industrial                      | Determines sub-type options      |
| Property Sub-type    | Cascading Dropdown | Yes         | Based on Property Type selection                           | Specific property classification |
| Property Address     | Text Area          | Yes         | Min 10 characters                                          | Full service address             |
| Landmark             | Text               | No          | —                                                          | Nearby reference point           |
| City                 | Text               | Yes         | Min 2 characters                                           | Service city                     |
| State                | Dropdown           | Yes         | Indian states list                                         | Service state                    |
| Pincode              | Number             | Yes         | Exactly 6 digits, valid Indian pincode                     | Postal code                      |
| Pest Type Concern    | Multi Checkbox     | Yes         | All pest types from Module 12                              | Primary service requirements     |
| Other Pest Specified | Text               | Conditional | Required if "Other" pest type selected                     | Custom pest entry                |
| Service Required     | Dropdown           | Yes         | One-time / AMC (Annual) / Quarterly / Monthly / Inspection | Service frequency preference     |
| Preferred Date       | Date               | No          | Future date only                                           | Customer preferred date          |
| Preferred Time Slot  | Dropdown           | No          | Morning / Afternoon / Evening / Anytime                    | Preferred service timing         |

---

## Section 4: Additional Information Fields

| Field                  | Type      | Required | Options/Validation                                           | Notes                           |
| ---------------------- | --------- | -------- | ------------------------------------------------------------ | ------------------------------- |
| Budget Range           | Dropdown  | No       | <₹5K / ₹5K-10K / ₹10K-25K / ₹25K-50K / ₹50K+ / Not Discussed | Qualification data              |
| Current Provider       | Text      | No       | —                                                            | Competitor information          |
| Contract Expiry        | Date      | No       | Future date                                                  | If existing service known       |
| Decision Maker         | Text      | No       | Min 3 characters                                             | Authority for approval          |
| Decision Maker Contact | Text      | No       | Phone or email                                               | Decision maker's contact        |
| Lead Description       | Text Area | Yes      | Min 20 characters                                            | Detailed requirements           |
| Special Instructions   | Text Area | No       | Max 500 characters                                           | Access info, special conditions |

---

## Section 5: Referral Information Fields (Conditional)

| Field            | Type     | Required    | Options/Validation                             | Notes                         |
| ---------------- | -------- | ----------- | ---------------------------------------------- | ----------------------------- |
| Referred By      | Text     | Conditional | Min 3 characters                               | Required if Source = Referral |
| Referrer Contact | Text     | Conditional | Phone or email                                 | Referrer's contact details    |
| Referrer Type    | Dropdown | Conditional | Existing Customer / Employee / Partner / Other | Referrer category             |
| Referral Code    | Text     | No          | Alphanumeric                                   | Tracking code if applicable   |

---

## System Fields

| Field               | Type | Required | Notes                    |
| ------------------- | ---- | -------- | ------------------------ |
| Created By          | Auto | System   | Current logged-in user   |
| Created Date        | Auto | System   | System timestamp         |
| Status              | Auto | System   | Default: NEW             |
| Next Follow-up Date | Auto | System   | Default: +2 working days |

---

## Validation Rules

| Validation                | Rule                                                            |
| ------------------------- | --------------------------------------------------------------- |
| Mobile Uniqueness         | Mobile number must not exist in active leads (check duplicates) |
| Email Uniqueness          | Email must not exist in active leads (if provided)              |
| Property Sub-type Cascade | Sub-type options refresh based on Property Type selection       |
| Referral Fields           | Section 5 visible only when Lead Source = "Referral"            |
| Other Pest Field          | Visible and required only when "Other" pest type selected       |
| Preferred Date            | Must be today or future date, not past                          |
| Pincode Validation        | Must be valid 6-digit Indian pincode format                     |

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
│                           EDIT LEAD                                            │
│                                                                              │
│  [← Back to Lead List]     Lead ID: LD-2026-00142     Status: CONTACTED      │
│                                                                              │
│  SECTION 1: LEAD BASIC INFORMATION (Partially Editable)                      │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Lead ID:              [LD-2026-00142 - Read Only]                     │ │
│  │  Lead Date:            [15-Mar-2026 - Read Only]                      │ │
│  │  Lead Source:          [Website - Read Only]                          │ │
│  │  Priority:             [▼ High / Normal / Low / Urgent ▼] ✅          │ │
│  │  Assigned To:          [▼ Sales Rep Dropdown ▼] ✅                      │ │
│  │                         (Editable by Manager/Admin only)               │ │
│  │  Status:               [▼ Contacted ▼] (Controlled workflow)            │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 2: CONTACT INFORMATION (Editable)                                   │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Lead Name:            [Rahul Sharma] ✅                                 │ │
│  │  Mobile Number:        [9876543210 - Read Only]                         │ │
│  │                         (Locked - primary identifier)                    │ │
│  │  Alternate Number:     [9123456789] ✅                                   │ │
│  │  Email ID:             [rahul.s@email.com] ✅                            │ │
│  │  WhatsApp Number:      [9876543210] ✅                                   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 3: PROPERTY & SERVICE DETAILS (Partially Editable)                  │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Property Type:        [Residential - Read Only]                        │ │
│  │  Property Sub-type:    [3BHK - Read Only]                               │ │
│  │  Property Address:     [Flat 402, Sunshine Apartments...] ✅             │ │
│  │  Landmark:             [Near Metro Station] ✅                           │ │
│  │  City:                 [Bangalore - Read Only]                            │ │
│  │  State:                [Karnataka - Read Only]                          │ │
│  │  Pincode:              [560001] ✅                                        │ │
│  │                                                                              │
│  │  Pest Type Concern:    [☑ Termite ☑ Cockroach] ✅ (Editable)            │ │
│  │  Service Required:     [▼ AMC (Annual) ▼] ✅                             │ │
│  │  Preferred Date:       [25-Mar-2026] ✅                                 │ │
│  │  Preferred Time Slot:  [▼ Morning ▼] ✅                                  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 4: ADDITIONAL INFORMATION (Editable)                                  │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Budget Range:         [▼ ₹10K-25K ▼] ✅                                 │ │
│  │  Current Provider:     [PestGuard Solutions] ✅                          │ │
│  │  Contract Expiry:      [31-Mar-2026] ✅                                  │ │
│  │  Decision Maker:       [Rahul Sharma (Self)] ✅                          │ │
│  │  Decision Maker Contact: [—] ✅                                          │ │
│  │  Lead Description:   [Termite treatment required for entire...] ✅     │ │
│  │  Special Instructions: [Please call before visiting] ✅                  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 5: REFERRAL INFORMATION (Read Only)                                 │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Referred By:          [Anita Kumar - Read Only]                        │ │
│  │  Referrer Contact:     [anita.k@email.com - Read Only]                │ │
│  │  Referrer Type:        [Existing Customer - Read Only]                  │ │
│  │  Referral Code:        [REF-2026-089 - Read Only]                      │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 6: SYSTEM & AUDIT INFORMATION                                       │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Created By:           [Admin User]                                       │ │
│  │  Created Date:         [15-Mar-2026 10:30 AM]                            │ │
│  │  Last Updated By:      [Rajesh Kumar]                                     │ │
│  │  Last Updated Date:   [18-Mar-2026 03:45 PM]                            │ │
│  │  Next Follow-up Date:  [20-Mar-2026] ✅ (Auto-updated on follow-up)       │ │
│  │                                                                              │
│  │  CHANGE HISTORY:                                                         │ │
│  │  ┌─────────────┬─────────────┬─────────────────┬─────────────────────┐  │ │
│  │  │ Date        │ User        │ Field Changed   │ Change Summary      │  │ │
│  │  ├─────────────┼─────────────┼─────────────────┼─────────────────────┤  │ │
│  │  │ 18-Mar-2026 │ Rajesh K.   │ Status          │ NEW → CONTACTED     │  │ │
│  │  │ 18-Mar-2026 │ Rajesh K.   │ Priority        │ Normal → High       │  │ │
│  │  │ 17-Mar-2026 │ Rajesh K.   │ Budget Range    │ Not Discussed → ₹   │  │ │
│  │  └─────────────┴─────────────┴─────────────────┴─────────────────────┘  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│                    [UPDATE LEAD]        [CANCEL]        [VIEW FOLLOW-UPS]      │
│                                                                              │
│  ⚠️ NOTE: Mobile Number and Lead Source cannot be changed after creation.    │
│           Property Type/City/State require Manager approval to modify.       │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Field Editability Matrix

| Field Category       | Field Name           | NEW | CONTACTED | QUALIFIED | PROPOSAL | WON/LOST | Notes                               |
| -------------------- | -------------------- | --- | --------- | --------- | -------- | -------- | ----------------------------------- |
| **Basic Info**       | Lead ID              | ❌  | ❌        | ❌        | ❌       | ❌       | System generated, never editable    |
|                      | Lead Date            | ❌  | ❌        | ❌        | ❌       | ❌       | Creation timestamp                  |
|                      | Lead Source          | ❌  | ❌        | ❌        | ❌       | ❌       | Origin tracking, locked             |
|                      | Priority             | ✅  | ✅        | ✅        | ⚠️       | ❌       | Editable until Proposal stage       |
|                      | Assigned To          | ⚠️  | ⚠️        | ⚠️        | ❌       | ❌       | Manager/Admin only                  |
|                      | Status               | ⚠️  | ⚠️        | ⚠️        | ⚠️       | ❌       | Workflow controlled                 |
| **Contact Info**     | Lead Name            | ✅  | ✅        | ⚠️        | ❌       | ❌       | Editable until Qualified            |
|                      | Mobile Number        | ❌  | ❌        | ❌        | ❌       | ❌       | Primary key, never editable         |
|                      | Alternate Number     | ✅  | ✅        | ✅        | ✅       | ❌       | Always editable                     |
|                      | Email ID             | ✅  | ✅        | ✅        | ⚠️       | ❌       | Editable until Proposal             |
|                      | WhatsApp Number      | ✅  | ✅        | ✅        | ✅       | ❌       | Always editable                     |
| **Property Details** | Property Type        | ⚠️  | ⚠️        | ❌        | ❌       | ❌       | Manager approval required           |
|                      | Property Sub-type    | ⚠️  | ⚠️        | ❌        | ❌       | ❌       | Linked to Property Type             |
|                      | Property Address     | ✅  | ✅        | ✅        | ⚠️       | ❌       | Editable until Proposal             |
|                      | Landmark             | ✅  | ✅        | ✅        | ✅       | ❌       | Always editable                     |
|                      | City/State/Pincode   | ⚠️  | ⚠️        | ❌        | ❌       | ❌       | Manager approval required           |
| **Service Details**  | Pest Type Concern    | ✅  | ✅        | ⚠️        | ❌       | ❌       | Editable until Qualified            |
|                      | Service Required     | ✅  | ✅        | ⚠️        | ❌       | ❌       | Editable until Qualified            |
|                      | Preferred Date/Time  | ✅  | ✅        | ✅        | ⚠️       | ❌       | Editable until Proposal             |
| **Additional Info**  | Budget Range         | ✅  | ✅        | ✅        | ⚠️       | ❌       | Editable until Proposal             |
|                      | Current Provider     | ✅  | ✅        | ✅        | ✅       | ❌       | Always editable                     |
|                      | Contract Expiry      | ✅  | ✅        | ✅        | ✅       | ❌       | Always editable                     |
|                      | Decision Maker       | ✅  | ✅        | ✅        | ⚠️       | ❌       | Editable until Proposal             |
|                      | Lead Description     | ✅  | ✅        | ✅        | ⚠️       | ❌       | Editable until Proposal             |
|                      | Special Instructions | ✅  | ✅        | ✅        | ✅       | ❌       | Always editable                     |
| **Referral Info**    | All Fields           | ❌  | ❌        | ❌        | ❌       | ❌       | Referral data locked after creation |

**Legend:** ✅ Fully Editable | ⚠️ Restricted/Approval Required | ❌ Not Editable

---

## Status Workflow Rules

| Current Status | Allowed Next Status             | Automatic Triggers                                  |
| -------------- | ------------------------------- | --------------------------------------------------- |
| NEW            | CONTACTED, LOST, ON HOLD        | First follow-up logged → auto-move to CONTACTED     |
| CONTACTED      | QUALIFIED, LOST, ON HOLD        | Budget + Timeline confirmed → QUALIFIED             |
| QUALIFIED      | PROPOSAL, LOST, ON HOLD         | Quotation created → auto-move to PROPOSAL           |
| PROPOSAL       | NEGOTIATION, WON, LOST, ON HOLD | Customer responds to quotation → NEGOTIATION        |
| NEGOTIATION    | WON, LOST, ON HOLD              | Contract signed → WON; No response 30 days → LOST   |
| WON            | —                               | Triggers Contract creation in Module 18             |
| LOST           | —                               | Requires Lost Reason; can be reactivated by Manager |
| ON HOLD        | Any previous status             | Follow-up paused; can resume to prior status        |

---

## Validation Rules (Edit Mode)

| Validation                  | Rule                                                              |
| --------------------------- | ----------------------------------------------------------------- |
| Status Change Authorization | Only assigned user or manager can change status                   |
| Mandatory Lost Reason       | Required when changing status to LOST                             |
| Reactivation Approval       | Manager approval required to move from LOST back to active status |
| Audit Trail                 | All field changes logged with user, timestamp, old/new values     |
| Duplicate Prevention        | Mobile/email uniqueness check maintained on edit (exclude self)   |

---

## Actions

| Action          | Behavior                                               |
| --------------- | ------------------------------------------------------ |
| Update Lead     | Saves changes, logs audit trail, updates timestamp     |
| Cancel          | Discards changes, returns to lead list                 |
| View Follow-ups | Redirects to Tab 2 (Follow-up Log) of View Lead screen |

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
│  SECTION 1: LEAD SUMMARY                                                     │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Lead Status:          🟡 CONTACTED                                     │ │
│  │  Priority:             🔴 HIGH                                          │ │
│  │  Lead Source:          Website                                          │ │
│  │  Assigned To:          Rajesh Kumar (Sales Executive)                   │ │
│  │  Created:              15-Mar-2026 by Admin                              │ │
│  │  Last Updated:         18-Mar-2026 by Rajesh Kumar                       │ │
│  │  Next Follow-up:       20-Mar-2026 (Due in 2 days)                        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 2: CONTACT INFORMATION                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Lead Name:            Rahul Sharma                                     │ │
│  │  Mobile Number:        +91 98765 43210                                  │ │
│  │  Alternate Number:     +91 91234 56789                                  │ │
│  │  Email ID:             rahul.sharma@email.com                           │ │
│  │  WhatsApp:             +91 98765 43210                                    │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 3: PROPERTY & SERVICE DETAILS                                         │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Property Type:        Residential                                      │ │
│  │  Property Sub-type:    3BHK Apartment                                   │ │
│  │  Property Address:     Flat 402, Sunshine Apartments, 27th Main,        │ │
│  │                        HSR Layout, Sector 1, Bangalore - 560001         │ │
│  │  Landmark:             Near HSR Metro Station                             │ │
│  │                                                                              │
│  │  Pest Type Concern:    • Termite (Primary)                              │ │
│  │                        • Cockroach (Secondary)                          │ │
│  │  Service Required:     AMC (Annual Maintenance Contract)                │ │
│  │  Preferred Date:       25-Mar-2026 (Morning slot)                       │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 4: ADDITIONAL INFORMATION                                           │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Budget Range:         ₹10,000 - ₹25,000                                  │ │
│  │  Current Provider:     PestGuard Solutions (Contract expires 31-Mar-26)  │ │
│  │  Decision Maker:       Rahul Sharma (Self)                                │ │
│  │                                                                              │
│  │  Lead Description:                                                        │ │
│  │  Customer reported termite infestation in wooden furniture and flooring.   │ │
│  │  Requires comprehensive termite treatment with annual maintenance.        │ │
│  │  Currently using competitor service but dissatisfied with results.        │ │
│  │                                                                              │ │
│  │  Special Instructions:                                                    │ │
│  │  • Call 30 minutes before site visit                                        │ │
│  │  • Customer has pet dog - ensure pet-safe chemicals                       │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 5: REFERRAL INFORMATION                                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Referred By:          Anita Kumar (Existing Customer: CU-2025-0042)    │ │
│  │  Referrer Contact:       anita.kumar@email.com                            │ │
│  │  Referrer Type:        Existing Customer                                  │ │
│  │  Referral Code:          REF-2026-089                                     │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 6: QUICK ACTIONS & CONVERSION                                         │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │                                                                              │ │
│  │  [📋 CREATE GMA SHEET]    [📄 CREATE QUOTATION]    [📞 LOG CALL]        │ │
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

## 15.4.1 Tab 1: Basic Lead Information Fields

| Section                | Field Name           | Type           | Description                                   |
| ---------------------- | -------------------- | -------------- | --------------------------------------------- |
| **Lead Summary**       | Lead Status          | Status Badge   | Current lifecycle stage with visual indicator |
|                        | Priority             | Priority Badge | Urgency level with color coding               |
|                        | Lead Source          | Text           | Origin of inquiry                             |
|                        | Assigned To          | Text           | Sales owner with designation                  |
|                        | Created              | DateTime       | Creation timestamp with creator name          |
|                        | Last Updated         | DateTime       | Last modification timestamp                   |
|                        | Next Follow-up       | Date           | Scheduled next activity with countdown        |
| **Contact Info**       | Lead Name            | Text           | Primary contact person                        |
|                        | Mobile Number        | Phone          | Primary contact (click-to-call enabled)       |
|                        | Alternate Number     | Phone          | Secondary contact                             |
|                        | Email ID             | Email          | Electronic contact (click-to-mail enabled)    |
|                        | WhatsApp             | Phone          | WhatsApp contact                              |
| **Property & Service** | Property Type        | Text           | Residential/Commercial/Industrial             |
|                        | Property Sub-type    | Text           | Specific classification                       |
|                        | Property Address     | Text           | Full service location                         |
|                        | Landmark             | Text           | Navigation reference                          |
|                        | Pest Type Concern    | Bullet List    | Selected pest categories                      |
|                        | Service Required     | Text           | Service frequency preference                  |
|                        | Preferred Date/Time  | Text           | Customer availability                         |
| **Additional Info**    | Budget Range         | Currency Range | Customer budget indication                    |
|                        | Current Provider     | Text           | Competitor information                        |
|                        | Decision Maker       | Text           | Approval authority details                    |
|                        | Lead Description     | Text Area      | Detailed requirements                         |
|                        | Special Instructions | Text Area      | Operational notes                             |
| **Referral Info**      | Referred By          | Text           | Referrer name with customer link              |
|                        | Referrer Contact     | Text           | Referrer details                              |
|                        | Referrer Type        | Badge          | Category of referrer                          |
|                        | Referral Code        | Text           | Tracking identifier                           |

---

## 15.4.2 Tab 2: Follow-up Log Table

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
│  FOLLOW-UP SUMMARY CARDS                                                     │
│  ┌─────────────────┬─────────────────┬─────────────────┬─────────────────┐    │
│  │ Total Follow-ups│ Completed       │ Pending         │ Overdue         │    │
│  │       5         │       3         │       1         │       1         │    │
│  │                 │                 │ (Due 20-Mar)    │ (Due 18-Mar)    │    │
│  └─────────────────┴─────────────────┴─────────────────┴─────────────────┘    │
│                                                                              │
│  QUICK FILTERS: [All] [Calls] [Meetings] [Site Visits] [Emails] [WhatsApp]   │
│                                                                              │
│  FOLLOW-UP LOG TABLE                                                         │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │F/U #│Date & Time│Type│Contact│Outcome│Next Action│Status│Assigned│Action│ │
│  │     │           │    │Mode   │       │           │      │To      │      │ │
│  │─────┼───────────┼────┼───────┼───────┼───────────┼──────┼────────┼──────│ │
│  │#5   │18-Mar 15:30│Call│Mobile │Pos.   │Site visit │Comp. │Rajesh K│👁    │ │
│  │     │           │    │       │       │22-Mar     │      │        │      │ │
│  │#4   │17-Mar 11:00│WA  │WhatsApp│Info  │Share      │Comp. │Rajesh K│👁    │ │
│  │     │           │    │       │Req.   │brochure   │      │        │      │ │
│  │#3   │16-Mar 16:30│Call│Mobile │Not    │Callback   │Comp. │Rajesh K│👁    │ │
│  │     │           │    │       │Avail. │17-Mar     │      │        │      │ │
│  │#2   │15-Mar 14:00│Email│Email │Opened │Call to    │Comp. │Rajesh K│👁    │ │
│  │     │           │    │       │       │discuss    │      │        │      │ │
│  │#1   │15-Mar 10:30│Auto│System │Lead   │Qualify    │Comp. │System  │👁    │ │
│  │     │           │    │       │Created│req.       │      │        │      │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  PENDING FOLLOW-UPS                                                          │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │F/U #│Scheduled│Type│Purpose│Assigned To│Status│Overdue│Action          │ │
│  │─────┼─────────┼────┼───────┼───────────┼──────┼───────┼────────────────│ │
│  │#6   │20-Mar 10:00│Site│Initial│Rajesh K.  │Pend. │—      │[Complete] [Resch]│
│  │     │         │Visit│survey │           │      │       │                │ │
│  │#7   │18-Mar 16:00│Call│Follow-│Rajesh K.  │Pend. │⚠️ 2d  │[Complete] [Resch]│
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
│  │ Contract:     Not Applicable       (Requires Won status)                  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Follow-up Log Table Fields

| Field        | Type         | Description                                           |
| ------------ | ------------ | ----------------------------------------------------- |
| F/U #        | Number       | Sequential follow-up identifier                       |
| Date & Time  | DateTime     | When follow-up was conducted or scheduled             |
| Type         | Badge        | Call / Meeting / Site Visit / Email / WhatsApp / Auto |
| Contact Mode | Text         | Channel used for communication                        |
| Outcome      | Text         | Result of the follow-up interaction                   |
| Next Action  | Text         | Agreed next step from follow-up                       |
| Status       | Status Badge | Completed / Pending / Overdue / Cancelled             |
| Assigned To  | Text         | User responsible for this follow-up                   |
| Action       | Button       | View follow-up details                                |

---

## General Buttons (Tab 2)

| Button              | Action                                             | Redirects To                  | Condition                         |
| ------------------- | -------------------------------------------------- | ----------------------------- | --------------------------------- |
| ➕ New Follow-up    | Opens Add Follow-up Form                           | 15.5 Add Follow-up Form       | Always available                  |
| 📋 Create GMA Sheet | Opens GMA creation with lead data pre-filled       | Module 16: Add GMA Form       | Lead status = QUALIFIED or higher |
| 📄 Create Quotation | Opens Quotation creation with lead data pre-filled | Module 17: Add Quotation Form | Lead status = QUALIFIED or higher |

---

## Conversion Status Logic

| Status      | GMA Creation                  | Quotation Creation         | Contract Creation        |
| ----------- | ----------------------------- | -------------------------- | ------------------------ |
| NEW         | ❌ Not allowed                | ❌ Not allowed             | ❌ Not allowed           |
| CONTACTED   | ⚠️ Manager approval           | ❌ Not allowed             | ❌ Not allowed           |
| QUALIFIED   | ✅ Allowed                    | ✅ Allowed                 | ❌ Not allowed           |
| PROPOSAL    | ✅ Allowed                    | ✅ Allowed (edit existing) | ⚠️ Requires approval     |
| NEGOTIATION | ✅ Allowed (re-GMA if needed) | ✅ Allowed (revised quote) | ✅ Allowed               |
| WON         | ❌ Not applicable             | ❌ Not applicable          | ✅ Auto-trigger Contract |
| LOST        | ❌ Not applicable             | ❌ Not applicable          | ❌ Not applicable        |

---

# 15.5 Add Follow-up Form

**Description:**
Captures interaction details with leads including contact mode, discussion outcomes, customer feedback, and next action planning. Automatically updates lead status based on follow-up results and schedules subsequent activities.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           ADD FOLLOW-UP                                        │
│                                                                              │
│  [← Back to Lead: LD-2026-00142]     Lead: Rahul Sharma | Status: CONTACTED   │
│                                                                              │
│  SECTION 1: FOLLOW-UP BASIC INFORMATION                                        │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Follow-up Number:     [Auto: #6]                                       │ │
│  │  Follow-up Date*:      [📅 19-Mar-2026] (Default: Today)                │ │
│  │  Follow-up Time*:      [🕐 14:30] (24-hour format)                       │ │
│  │  Follow-up Type*:      [▼ Call / Meeting / Site Visit / Email          │ │
│  │                         / WhatsApp / SMS / Video Call ▼]               │ │
│  │                                                                              │
│  │  Contact Mode*:        [▼ Mobile Call / WhatsApp Call / Email Reply     │ │
│  │                         / In-Person / Video Conference ▼]              │ │
│  │                         (Auto-suggested based on Follow-up Type)        │ │
│  │                                                                              │
│  │  Follow-up Status*:    [▼ Completed / No Response / Rescheduled         │ │
│  │                         / Wrong Number / Not Interested ▼]             │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 2: INTERACTION DETAILS                                                │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Contact Person*:        [Rahul Sharma] (Auto: Lead Name, editable)       │ │
│  │  Person Role:           [▼ Decision Maker / Influencer / End User     │ │
│  │                          / Facility Manager / Other ▼]                   │ │
│  │                                                                              │
│  │  Call Duration:         [____] minutes (For calls)                      │ │
│  │  Meeting Location:      [____________] (For meetings/site visits)        │ │
│  │                                                                              │
│  │  Discussion Summary*:    [________________________________________]       │ │
│  │                          (Key points discussed, customer responses)      │ │
│  │                          [Min 30 characters]                             │ │
│  │                                                                              │
│  │  Customer Feedback:      [▼ Very Interested / Interested / Neutral      │ │
│  │                          / Skeptical / Not Interested ▼]                 │ │
│  │                                                                              │
│  │  Objections Raised:      [☑ Price too high ☑ Timing not right           │ │
│  │                          ☑ Satisfied with current provider             │ │
│  │                          ☑ Need to check with family/partner           │ │
│  │                          ☑ Want to compare quotes                      │ │
│  │                          ☑ Other: [____________]]                        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 3: NEXT ACTION PLANNING                                               │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Next Action Required*:  [☑ Yes ☐ No]                                  │ │
│  │                                                                              │
│  │  IF YES:                                                                  │ │
│  │  Next Action Type*:      [▼ Call / Meeting / Site Visit / Send Quote   │ │
│  │                           / Send Brochure / GMA Visit / Contract       │ │
│  │                           / No Further Action ▼]                       │ │
│  │                                                                              │
│  │  Next Action Date*:      [📅 22-Mar-2026]                               │ │
│  │  Next Action Time:       [🕐 10:00]                                      │ │
│  │  Purpose/Agenda*:        [________________________________________]       │ │
│  │                          (Specific objective for next interaction)       │ │
│  │                                                                              │
│  │  Reminder Setting:       [☑ 1 hour before ☐ 1 day before               │ │
│  │                           ☐ Custom: [____] before]                     │ │
│  │                                                                              │
│  │  Assign To*:             [▼ Rajesh Kumar (Self) ▼]                     │ │
│  │                          (Sales team members)                            │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 4: STATUS UPDATE & QUALIFICATION                                      │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Update Lead Status:     [▼ CONTACTED → QUALIFIED ▼]                    │ │
│  │                          (Suggested based on customer feedback)          │ │
│  │                                                                              │
│  │  IF MOVING TO QUALIFIED:                                                  │ │
│  │  Budget Confirmed*:      [☑ Yes ☐ No]                                   │ │
│  │  Timeline Confirmed*:    [☑ Yes ☐ No]                                   │ │
│  │  Decision Maker Identified*: [☑ Yes ☐ No]                               │ │
│  │  Service Requirements Clear*: [☑ Yes ☐ No]                              │ │
│  │                                                                              │
│  │  Estimated Value (₹):    [________] (Projected deal size)               │ │
│  │  Probability of Closure: [▼ 10% / 25% / 50% / 75% / 90% ▼]                │ │
│  │                                                                              │
│  │  IF MARKING AS LOST:                                                      │ │
│  │  Lost Reason*:           [▼ Price / Competitor / No Budget / Not Ready   │ │
│  │                          / Wrong Contact / Duplicate / Other ▼]        │ │
│  │  Lost Reason Details:    [________________________________________]       │ │
│  │  Can Recontact:          [☑ Yes (After [____] months) ☐ No]             │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  SECTION 5: ATTACHMENTS & REFERENCES                                           │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Upload Recording:       [📎 Browse] (Call recording, max 10MB)           │ │
│  │  Site Visit Photos:      [📎 Multiple Upload] (Max 5 images, 2MB each)  │ │
│  │  Documents Shared:       [☑ Brochure ☑ Rate Card ☑ Case Study            │ │
│  │                           ☑ Technical Specs ☑ Other: [____]]            │ │
│  │  Reference Lead/Customer: [____________] (Similar case reference)         │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│                    [SAVE FOLLOW-UP]        [CANCEL]        [SAVE & NEW F/U]    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Section 1: Follow-up Basic Information Fields

| Field            | Type     | Required | Options/Validation                                                    | Notes                     |
| ---------------- | -------- | -------- | --------------------------------------------------------------------- | ------------------------- |
| Follow-up Number | Auto     | System   | Sequential (#1, #2, #3...)                                            | Read-only identifier      |
| Follow-up Date   | Date     | Yes      | Today or past date                                                    | When interaction occurred |
| Follow-up Time   | Time     | Yes      | 24-hour format                                                        | Exact time of contact     |
| Follow-up Type   | Dropdown | Yes      | Call / Meeting / Site Visit / Email / WhatsApp / SMS / Video Call     | Category of interaction   |
| Contact Mode     | Dropdown | Yes      | Auto-filtered based on Type                                           | Specific channel used     |
| Follow-up Status | Dropdown | Yes      | Completed / No Response / Rescheduled / Wrong Number / Not Interested | Outcome of attempt        |

---

## Section 2: Interaction Details Fields

| Field              | Type           | Required    | Options/Validation                                                  | Notes                   |
| ------------------ | -------------- | ----------- | ------------------------------------------------------------------- | ----------------------- |
| Contact Person     | Text           | Yes         | Auto-filled from Lead, editable                                     | Who was contacted       |
| Person Role        | Dropdown       | Yes         | Decision Maker / Influencer / End User / Facility Manager / Other   | Authority level         |
| Call Duration      | Number         | Conditional | Minutes, for Call/Video types                                       | Length of conversation  |
| Meeting Location   | Text           | Conditional | Required for Meeting/Site Visit                                     | Where meeting occurred  |
| Discussion Summary | Text Area      | Yes         | Min 30 characters                                                   | Key conversation points |
| Customer Feedback  | Dropdown       | Yes         | Very Interested / Interested / Neutral / Skeptical / Not Interested | Engagement level        |
| Objections Raised  | Multi Checkbox | No          | Common sales objections                                             | Barriers to close       |

---

## Section 3: Next Action Planning Fields

| Field                | Type           | Required    | Options/Validation                                                                                  | Notes                       |
| -------------------- | -------------- | ----------- | --------------------------------------------------------------------------------------------------- | --------------------------- |
| Next Action Required | Checkbox       | Yes         | Yes/No toggle                                                                                       | Whether follow-up continues |
| Next Action Type     | Dropdown       | Conditional | Call / Meeting / Site Visit / Send Quote / Send Brochure / GMA Visit / Contract / No Further Action | Planned activity            |
| Next Action Date     | Date           | Conditional | Future date                                                                                         | When to follow up           |
| Next Action Time     | Time           | No          | Optional scheduling                                                                                 | Preferred time slot         |
| Purpose/Agenda       | Text Area      | Conditional | Min 10 characters                                                                                   | Objective for next contact  |
| Reminder Setting     | Multi Checkbox | No          | 1 hour / 1 day / Custom before                                                                      | Notification preferences    |
| Assign To            | Dropdown       | Yes         | Active sales team members                                                                           | Owner of next action        |

---

## Section 4: Status Update & Qualification Fields

| Field                      | Type      | Required    | Options/Validation                                                             | Notes                       |
| -------------------------- | --------- | ----------- | ------------------------------------------------------------------------------ | --------------------------- |
| Update Lead Status         | Dropdown  | Yes         | Workflow-allowed status transitions                                            | Progress lead in pipeline   |
| Budget Confirmed           | Checkbox  | Conditional | Yes/No                                                                         | Required for QUALIFIED      |
| Timeline Confirmed         | Checkbox  | Conditional | Yes/No                                                                         | Required for QUALIFIED      |
| Decision Maker Identified  | Checkbox  | Conditional | Yes/No                                                                         | Required for QUALIFIED      |
| Service Requirements Clear | Checkbox  | Conditional | Yes/No                                                                         | Required for QUALIFIED      |
| Estimated Value            | Currency  | No          | Positive number                                                                | Projected revenue           |
| Probability of Closure     | Dropdown  | No          | 10% / 25% / 50% / 75% / 90%                                                    | Win likelihood              |
| Lost Reason                | Dropdown  | Conditional | Price / Competitor / No Budget / Not Ready / Wrong Contact / Duplicate / Other | Required for LOST           |
| Lost Reason Details        | Text Area | Conditional | Min 20 characters                                                              | Detailed explanation        |
| Can Recontact              | Checkbox  | Conditional | Yes/No with months                                                             | Future follow-up permission |

---

## Section 5: Attachments & References Fields

| Field                   | Type           | Required | Options/Validation                                          | Notes                |
| ----------------------- | -------------- | -------- | ----------------------------------------------------------- | -------------------- |
| Upload Recording        | File           | No       | Audio file, max 10MB                                        | Call recording       |
| Site Visit Photos       | Multi File     | No       | Images, max 5 files, 2MB each                               | Visual documentation |
| Documents Shared        | Multi Checkbox | No       | Brochure / Rate Card / Case Study / Technical Specs / Other | Materials provided   |
| Reference Lead/Customer | Search         | No       | Existing lead or customer ID                                | Similar case study   |

---

## Actions

| Action         | Behavior                                                                       |
| -------------- | ------------------------------------------------------------------------------ |
| Save Follow-up | Saves record, updates lead status, schedules next action, returns to lead view |
| Cancel         | Discards changes, returns to previous screen                                   |
| Save & New F/U | Saves current follow-up, immediately opens new follow-up form for same lead    |

---

## Automatic System Behaviors

| Trigger             | System Action                                    |
| ------------------- | ------------------------------------------------ |
| Status = QUALIFIED  | Enables GMA and Quotation creation buttons       |
| Status = PROPOSAL   | Tracks quotation reference                       |
| Status = WON        | Triggers Contract creation workflow in Module 18 |
| Status = LOST       | Archives lead, schedules recontact if allowed    |
| Next Action Created | Updates lead's "Next Follow-up Date" field       |
| Overdue Follow-up   | Sends escalation notification to manager         |

---

# 15.6 View Follow-up Detail

**Description:**
Read-only detailed view of a specific follow-up interaction showing complete conversation history, attachments, status changes, and linked next actions. Provides audit trail for sales activity review and coaching.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           VIEW FOLLOW-UP DETAIL                                │
│                                                                              │
│  [← Back to Lead: LD-2026-00142]     Follow-up #5 of Lead: Rahul Sharma      │
│                                                                              │
│  ─────────────────────────────────────────────────────────────────────────   │
│                                                                              │
│  FOLLOW-UP HEADER                                                            │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Follow-up Number:     #5                                               │ │
│  │  Date & Time:            18-Mar-2026, 15:30 IST                          │ │
│  │  Type:                   📞 Call (Mobile)                                  │ │
│  │  Duration:               12 minutes                                      │ │
│  │  Status:                 ✅ Completed                                     │ │
│  │  Conducted By:           Rajesh Kumar (Sales Executive)                  │ │
│  │  Contact Person:         Rahul Sharma (Decision Maker)                   │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  DISCUSSION DETAILS                                                          │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │                                                                              │ │
│  │  DISCUSSION SUMMARY:                                                      │ │
│  │  ─────────────────────────────────────────────────────────────────────    │ │
│  │  Called customer to discuss termite treatment requirements. Customer       │ │
│  │  confirmed infestation in 3 rooms and wooden furniture. Currently using    │ │
│  │  PestGuard but unhappy with results. Interested in our AMC package.        │ │
│  │  Asked for site visit to assess extent of problem.                         │ │
│  │                                                                              │ │
│  │  CUSTOMER FEEDBACK:        🟢 Very Interested                               │ │
│  │                                                                              │ │
│  │  OBJECTIONS RAISED:        • Want to compare quotes                        │ │
│  │                                                                              │ │
│  │  RESPONSE PROVIDED:        Shared that we offer free initial inspection    │ │
│  │                            and comparative analysis with existing provider.  │ │
│  │                            Customer agreed to site visit.                    │ │
│  │                                                                              │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  NEXT ACTION CREATED                                                         │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Next Action Type:       Site Visit (GMA)                                 │ │
│  │  Scheduled For:          22-Mar-2026, 10:00 AM                              │ │
│  │  Purpose:                Initial site survey and termite infestation      │ │
│  │                          assessment. Prepare GMA report and recommendations.│ │
│  │  Assigned To:            Rajesh Kumar                                     │ │
│  │  Reminder Set:           1 hour before                                     │ │
│  │  Status:                 ⏳ Pending (Due in 3 days)                          │ │
│  │                                                                              │ │
│  │  [View Next Follow-up #6 →]                                               │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  STATUS CHANGE IMPACT                                                        │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Lead Status Updated:    CONTACTED → QUALIFIED                            │ │
│  │  Update Reason:          Budget, timeline, and decision maker confirmed   │ │
│  │  Estimated Deal Value:     ₹18,000 (AMC Annual Package)                     │ │
│  │  Probability:              75%                                              │ │
│  │                                                                              │ │
│  │  QUALIFICATION CRITERIA MET:                                              │ │
│  │  ✅ Budget Confirmed        ✅ Timeline Confirmed                          │ │
│  │  ✅ Decision Maker ID'd     ✅ Requirements Clear                          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ATTACHMENTS & DOCUMENTATION                                                 │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Call Recording:         [🔊 Play Recording] (12:03 duration)              │ │
│  │  Documents Shared:       • Brochure (Termite Control)                      │ │
│  │                          • Rate Card (AMC Packages)                        │ │
│  │  Reference Case:         Similar: LD-2025-00842 (HSR Layout, 3BHK)         │ │
│  │                          [View Reference Case →]                          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  AUDIT TRAIL                                                                 │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  Created:              18-Mar-2026 15:45 by Rajesh Kumar                 │ │
│  │  Last Modified:        — (No edits)                                       │ │
│  │  System Updates:         • Next follow-up #6 auto-created                  │ │
│  │                          • Lead status auto-updated to QUALIFIED           │ │
│  │                          • GMA creation enabled                            │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│                              [CLOSE]    [PRINT]    [SHARE]                    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## View Follow-up Detail Fields

| Section           | Field                  | Type            | Description                             |
| ----------------- | ---------------------- | --------------- | --------------------------------------- |
| **Header**        | Follow-up Number       | Text            | Sequential identifier                   |
|                   | Date & Time            | DateTime        | When follow-up occurred                 |
|                   | Type                   | Badge with icon | Call / Meeting / Email etc. with visual |
|                   | Duration               | Number          | Length of interaction (if applicable)   |
|                   | Status                 | Status Badge    | Completed / Pending etc.                |
|                   | Conducted By           | Text            | Sales representative name               |
|                   | Contact Person         | Text            | Who was contacted                       |
| **Discussion**    | Discussion Summary     | Text Area       | Full conversation notes                 |
|                   | Customer Feedback      | Rating Badge    | Interest level with color               |
|                   | Objections Raised      | Bullet List     | Barriers identified                     |
|                   | Response Provided      | Text Area       | How objections were handled             |
| **Next Action**   | Next Action Type       | Text            | What was scheduled next                 |
|                   | Scheduled For          | DateTime        | When next action occurs                 |
|                   | Purpose                | Text            | Agenda for next interaction             |
|                   | Assigned To            | Text            | Owner of next action                    |
|                   | Reminder               | Text            | Notification settings                   |
|                   | Status                 | Status Badge    | Current state of next action            |
|                   | Link                   | Button          | Jump to next follow-up detail           |
| **Status Impact** | Lead Status Change     | Text            | Before → After status                   |
|                   | Update Reason          | Text            | Why status changed                      |
|                   | Estimated Value        | Currency        | Projected deal size                     |
|                   | Probability            | Percentage      | Win likelihood                          |
|                   | Qualification Criteria | Checklist       | Which criteria met                      |
| **Attachments**   | Call Recording         | Audio Player    | Playable recording                      |
|                   | Documents Shared       | List with icons | Materials provided                      |
|                   | Reference Case         | Link            | Similar successful case                 |
| **Audit**         | Created                | DateTime + User | Creation timestamp                      |
|                   | Last Modified          | DateTime + User | Edit timestamp                          |
|                   | System Updates         | Bullet List     | Automatic actions taken                 |

---

## Actions

| Action | Behavior                                          |
| ------ | ------------------------------------------------- |
| Close  | Returns to lead detail view (Tab 2)               |
| Print  | Generates printable PDF of follow-up record       |
| Share  | Sends follow-up summary via email to team/manager |

---

## Related Lead Information (Side Panel - Optional Collapsible)

```
┌────────────────────────────────────────┐
│  RELATED LEAD SUMMARY                  │
├────────────────────────────────────────┤
│  Lead ID:    LD-2026-00142             │
│  Name:       Rahul Sharma              │
│  Status:     🟢 QUALIFIED              │
│  Priority:   🔴 HIGH                   │
│  Assigned:   Rajesh Kumar              │
│                                        │
│  [View Full Lead →]                    │
│                                        │
│  QUICK STATS:                          │
│  • Total Follow-ups: 5                 │
│  • Days in Pipeline: 4                 │
│  • Next Action: 22-Mar (Site Visit)    │
│  • Est. Value: ₹18,000                 │
│                                        │
│  CONVERSION PROGRESS:                  │
│  [✓] Lead Captured                     │
│  [✓] Contacted                         │
│  [✓] Qualified                         │
│  [ ] GMA/Survey                        │
│  [ ] Quotation                         │
│  [ ] Negotiation                       │
│  [ ] Won                               │
└────────────────────────────────────────┘
```

---

# 15.7 Lead Status Workflow & Automation Rules

**Description:**
System-enforced business rules that govern lead lifecycle progression, automatic status updates, notifications, and escalations based on follow-up activities and time-based triggers.

---

## Status Transition Matrix

| From Status | To Status         | Trigger Condition                                             | Automatic Actions                                                                                                        |
| ----------- | ----------------- | ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| NEW         | CONTACTED         | First follow-up logged with "Completed" status                | • Send welcome SMS to customer<br>• Notify assigned sales rep<br>• Schedule 2-day follow-up reminder                     |
| CONTACTED   | QUALIFIED         | All 4 qualification criteria checked in follow-up             | • Enable GMA creation button<br>• Enable Quotation creation<br>• Notify sales manager<br>• Move to "Hot Leads" dashboard |
| CONTACTED   | LOST              | Follow-up status = "Not Interested" OR 3 no-response attempts | • Require lost reason<br>• Archive lead (soft delete)<br>• Schedule recontact if "Can Recontact" = Yes                   |
| QUALIFIED   | PROPOSAL          | Quotation created and sent                                    | • Link quotation reference<br>• Track quotation open/click<br>• Set 7-day follow-up reminder                             |
| QUALIFIED   | ON HOLD           | Customer requests delay with specific date                    | • Pause all automated reminders<br>• Resume on specified date<br>• Notify assigned rep                                   |
| PROPOSAL    | NEGOTIATION       | Customer responds to quotation (any feedback)                 | • Log negotiation start<br>• Enable revised quotation<br>• Manager notification for discount requests                    |
| PROPOSAL    | WON               | Customer accepts quotation                                    | • Trigger Contract creation (Module 18)<br>• Convert lead to customer<br>• Handover to operations team                   |
| PROPOSAL    | LOST              | Customer rejects quotation or no response in 30 days          | • Require lost reason with competitor name<br>• Archive lead<br>• Quarterly recontact reminder                           |
| NEGOTIATION | WON               | Contract terms finalized                                      | • Auto-generate contract draft<br>• Schedule signing appointment<br>• Notify finance for advance collection              |
| NEGOTIATION | LOST              | Unable to reach agreement                                     | • Document final negotiation points<br>• Manager review required<br>• 6-month cooling period before recontact            |
| ON HOLD     | [Previous Status] | Hold period expires or customer reactivates                   | • Resume standard workflow<br>• Update next follow-up date                                                               |
| WON         | —                 | Terminal status                                               | • Lead locked (read-only)<br>• Customer record created<br>• Service delivery initiated                                   |
| LOST        | NEW/CONTACTED     | Manager reactivation approval                                 | • Restore to pipeline<br>• Clear lost reason<br>• Require fresh follow-up                                                |

---

## Time-Based Automation Rules

| Trigger Time   | Condition                                          | System Action                                                                                                               |
| -------------- | -------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| +2 hours       | New lead created, no follow-up logged              | • Reminder notification to assigned rep<br>• Escalation to manager if after 4 hours                                         |
| +1 day         | Lead status = NEW                                  | • Urgent notification to sales head<br>• Suggest auto-reassignment                                                          |
| +2 days        | Last follow-up completed, no next action scheduled | • Create automatic follow-up task<br>• Default: "Call to check interest"                                                    |
| Due date       | Next follow-up overdue by 1 day                    | • Red flag on dashboard<br>• SMS to assigned rep<br>• CC manager                                                            |
| Due date       | Next follow-up overdue by 3 days                   | • Escalation to sales head<br>• Auto-reassignment suggestion<br>• Customer: "Your rep is unavailable, new contact assigned" |
| +7 days        | Status = PROPOSAL, no customer response            | • Automated gentle reminder email to customer<br>• Follow-up task: "Check quotation status"                                 |
| +15 days       | Status = PROPOSAL, no customer response            | • Manager involvement required<br>• Discount approval workflow triggered                                                    |
| +30 days       | Status = PROPOSAL, no customer response            | • Auto-move to LOST (pending manager approval)<br>• Final "Last chance" email to customer                                   |
| Recontact date | LOST lead with "Can Recontact" = Yes               | • Reactivate to NEW status<br>• Notify original assigned rep<br>• Suggest new approach strategy                             |

---

## Notification Matrix

| Event              | Recipient                  | Channel              | Content                                        |
| ------------------ | -------------------------- | -------------------- | ---------------------------------------------- |
| New Lead Assigned  | Assigned Sales Rep         | In-app + SMS + Email | Lead details, priority, suggested first action |
| Follow-up Overdue  | Assigned Rep + Manager     | In-app + SMS         | Overdue alert, customer may be lost            |
| Status → QUALIFIED | Sales Rep + Manager        | In-app + Email       | Qualified lead alert, GMA/Quote enabled        |
| Quotation Created  | Customer (auto) + Rep      | Email                | Quotation PDF with terms                       |
| Quotation Viewed   | Assigned Rep               | In-app               | Customer opened quotation (tracking pixel)     |
| Status → WON       | Rep + Manager + Operations | In-app + Email       | Celebration + handover initiation              |
| Status → LOST      | Rep + Manager              | In-app               | Loss analysis required, recontact scheduled    |
| Lead Reactivated   | New Assigned Rep           | In-app + SMS         | Reactivated lead with history summary          |

---

# 15.8 Lead Reports & Analytics (Dashboard Widgets)

**Description:**
Embedded analytics providing visibility into lead pipeline performance, conversion metrics, and sales activity effectiveness.

---

## Dashboard Widgets

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         LEAD MANAGEMENT DASHBOARD                               │
│                                                                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │
│  │   TOTAL     │  │    NEW      │  │  QUALIFIED  │  │   WON       │          │
│  │   LEADS     │  │   THIS WEEK │  │   THIS MONTH│  │  THIS MONTH │          │
│  │             │  │             │  │             │  │             │          │
│  │   1,247     │  │     42      │  │     38      │  │     12      │          │
│  │   ↑ 12%     │  │   ↑ 8%      │  │   ↑ 15%     │  │   ↑ 20%     │          │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘          │
│                                                                              │
│  ┌─────────────────────────┐  ┌─────────────────────────┐                      │
│  │   CONVERSION FUNNEL     │  │   LEADS BY SOURCE       │                      │
│  │                         │  │                         │                      │
│  │   NEW        100% ████ │  │   ┌─────────────────┐   │                      │
│  │   CONTACTED   78% ███  │  │   │ Website    35%  │   │                      │
│  │   QUALIFIED   45% ██   │  │   │ Referral   28%  │   │                      │
│  │   PROPOSAL    28% █    │  │   │ Walk-in    20%  │   │                      │
│  │   NEGOTIATION 15% ░    │  │   │ Cold Call  12%  │   │                      │
│  │   WON         12% ░    │  │   │ Other       5%  │   │                      │
│  │                         │  │   └─────────────────┘   │                      │
│  │   Avg. Cycle: 18 days   │  │                         │                      │
│  └─────────────────────────┘  └─────────────────────────┘                      │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │   TOP PERFORMERS THIS MONTH                                             │ │
│  │   ┌───────────┬──────────┬───────────┬───────────┬────────────┐        │ │
│  │   │ Sales Rep │ New Leads│ Qualified│   Won    │ Conv. Rate │        │ │
│  │   ├───────────┼──────────┼───────────┼───────────┼────────────┤        │ │
│  │   │ Rajesh K. │    15    │     8     │     4    │   26.7%    │        │ │
│  │   │ Anita S.  │    12    │     6     │     3    │   25.0%    │        │ │
│  │   │ Rohit M.  │    18    │     7     │     2    │   11.1%    │        │ │
│  │   └───────────┴──────────┴───────────┴───────────┴────────────┘        │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │   FOLLOW-UP ALERTS                                                      │ │
│  │   🔴 3 Overdue    🟡 8 Due Today    🟢 12 Scheduled Tomorrow            │ │
│  │   [View All →]                                                          │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Report Types Available

| Report Name             | Description                                      | Export Format |
| ----------------------- | ------------------------------------------------ | ------------- |
| Lead Pipeline Summary   | Status-wise lead count with aging                | PDF, Excel    |
| Sales Activity Report   | Follow-ups by rep, type, outcome                 | Excel, CSV    |
| Conversion Analysis     | Lead-to-customer conversion by source            | PDF, Excel    |
| Lost Lead Analysis      | Reasons for loss by competitor, price, timing    | PDF, Excel    |
| Lead Response Time      | Time from creation to first contact              | Excel         |
| Follow-up Effectiveness | Outcome analysis by follow-up type and frequency | Excel         |
| Revenue Forecast        | Pipeline value by probability-weighted stages    | Excel, PDF    |

---

# 15.9 Integration Points with Other Modules

| Module                     | Integration Type | Data Flow                                                           |
| -------------------------- | ---------------- | ------------------------------------------------------------------- |
| **Module 8 (Employee)**    | Uses             | Sales rep assignment, role-based access, performance tracking       |
| **Module 10 (Product)**    | Uses             | Service catalog for lead requirement matching, pricing reference    |
| **Module 12 (Service)**    | Uses             | Service configuration for accurate lead qualification               |
| **Module 13 (Vendor)**     | Uses             | Competitor information for lost lead analysis                       |
| **Module 16 (GMA)**        | Triggers         | Lead → GMA creation with auto-populated address, pest type, contact |
| **Module 17 (Quotation)**  | Triggers         | Qualified lead → Quotation with customer and service details        |
| **Module 18 (Contract)**   | Triggers         | Won lead → Contract initiation with terms negotiation               |
| **Module 19 (Scheduling)** | Triggers         | Follow-up dates → Technician visit scheduling                       |
| **Module 20 (Billing)**    | Receives         | Won lead value → Revenue recognition and invoicing                  |

---

# 15.10 Data Model Summary (Reference)

## Core Entities

| Entity                | Key Fields                                                             | Relationships                                                             |
| --------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| **Lead**              | Lead ID, Contact Info, Property Details, Status, Priority, Assigned To | Has many Follow-ups, Has one GMA (optional), Has one Quotation (optional) |
| **Follow-up**         | F/U ID, Lead ID, Date/Time, Type, Outcome, Next Action                 | Belongs to Lead, Created by User, May trigger Status Change               |
| **LeadStatusHistory** | Status, Changed Date, Changed By, Reason                               | Audit trail for lead lifecycle                                            |

---

## Complete Module 15 Structure Summary

| Section | Content Type         | Purpose                                          |
| ------- | -------------------- | ------------------------------------------------ |
| 15.1    | Table View           | Lead list with filters and quick actions         |
| 15.2    | Add Form             | New lead capture with full qualification         |
| 15.3    | Edit Form            | Lead modification with status-aware restrictions |
| 15.4    | View Screen (2 Tabs) | Complete lead information and follow-up history  |
| 15.4.1  | Tab 1                | Basic lead information display                   |
| 15.4.2  | Tab 2                | Follow-up log with conversion actions            |
| 15.5    | Add Form             | Follow-up logging with next action planning      |
| 15.6    | View Screen          | Detailed follow-up record with audit trail       |
| 15.7    | Workflow Rules       | Automation and status transition logic           |
| 15.8    | Analytics            | Dashboard widgets and reporting                  |
| 15.9    | Integration          | Cross-module data flow definitions               |
| 15.10   | Data Model           | Entity relationship reference                    |

===============================================================

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

| Field               | Type     | Required | Description                                                     |
| ------------------- | -------- | -------- | --------------------------------------------------------------- |
| Quotation ID        | Text     | Auto     | Unique identifier (QT-YYYY-SEQ format)                          |
| Source              | Badge    | Yes      | From Lead / From Customer / New Prospect                        |
| Client Name         | Text     | Yes      | Lead name, customer name, or new prospect name                  |
| Quotation Type      | Badge    | Yes      | Service / Product / Combined                                    |
| Branch              | Text     | Yes      | Assigned branch for quotation                                   |
| Total Amount (₹)    | Currency | Auto     | Grand total including tax                                       |
| Status              | Badge    | Auto     | Draft / Sent / Viewed / Accepted / Rejected / Expired / Revised |
| Valid Till          | Date     | Yes      | Quotation expiry date                                           |
| Created Date & Time | DateTime | Auto     | Timestamp of quotation creation                                 |
| Created By          | Text     | Auto     | User who created the quotation                                  |
| Actions             | Buttons  | —        | View / Delete (Revoke) / Download                               |

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
| Source         | Multi-select        | All          | From Lead / From Customer / New Prospect                        |
| Branch         | Searchable Dropdown | All          | All branches from Module 7                                      |
| Created By     | Searchable Dropdown | All          | Sales team members                                              |
| Created Date   | Date Range          | Last 30 Days | Custom range                                                    |
| Amount Range   | Number Range        | All          | Min – Max amount filter                                         |

---

# Search

Global search supports:

| Search Field | Type    |
| ------------ | ------- |
| Quotation ID | Text    |
| Client Name  | Text    |
| Phone Number | Numeric |
| Lead ID      | Text    |

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

| Field           | Type            | Required    | Options/Validation                                                                                        | Notes                              |
| --------------- | --------------- | ----------- | --------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| Source Type     | Radio           | Yes         | From Lead / From Customer / Add New                                                                       | Determines which sub-fields appear |
| Select Lead     | Search Dropdown | Conditional | Active leads from Module 15 (Status ≥ QUALIFIED)                                                          | Required if Source = From Lead     |
| Select Customer | Search Dropdown | Conditional | Active customers from Module 9                                                                            | Required if Source = From Customer |
| Full Name       | Text            | Conditional | Min 3 characters                                                                                          | Required if Source = Add New       |
| Phone           | Number          | Conditional | 10-digit Indian mobile                                                                                    | Required if Source = Add New       |
| Email           | Email           | No          | Valid email format                                                                                        | Optional for Add New               |
| Company Name    | Text            | No          | Max 100 characters                                                                                        | Optional for Add New               |
| Property Type   | Dropdown        | Conditional | 1BHK / 2BHK / 3BHK / 4BHK+ / Villa / Small Office / Medium Office / Large Office / Warehouse / Industrial | Required if Source = Add New       |
| Address         | Text            | Conditional | Min 10 characters                                                                                         | Required if Source = Add New       |
| City            | Text            | Conditional | Min 3 characters                                                                                          | Required if Source = Add New       |
| State           | Dropdown        | Conditional | Indian states list                                                                                        | Required if Source = Add New       |
| Pincode         | Number          | No          | 6-digit                                                                                                   | Optional                           |

---

## Section 2: Quotation Type Fields

| Field             | Type     | Required    | Options/Validation                         | Notes                                 |
| ----------------- | -------- | ----------- | ------------------------------------------ | ------------------------------------- |
| Quotation Type    | Radio    | Yes         | Service / Product / Combined               | Controls which sections are visible   |
| Service Mode      | Radio    | Conditional | One-Time / AMC                             | Visible if Type = Service or Combined |
| AMC Frequency     | Dropdown | Conditional | Monthly / Quarterly / Half-Yearly / Yearly | Required if Mode = AMC                |
| Contract Duration | Dropdown | Conditional | 6 Months / 1 Year / 2 Years / 3 Years      | Required if Mode = AMC                |
| Proposed Start    | Date     | Conditional | ≥ Today                                    | Required if Mode = AMC                |

---

## Section 3: Location & Branch Assignment Fields

| Field         | Type                | Required | Options/Validation                                         | Notes                         |
| ------------- | ------------------- | -------- | ---------------------------------------------------------- | ----------------------------- |
| Address       | Text                | Yes      | Min 10 characters                                          | Service delivery address      |
| City          | Text                | Yes      | Min 3 characters                                           | City name                     |
| State         | Dropdown            | Yes      | Indian states list                                         | State selection               |
| Property Type | Dropdown            | Yes      | Residential / Commercial types (same as Module 12 pricing) | Determines service pricing    |
| Area (sqft)   | Number              | Yes      | Must be > 0                                                | Used for area-based pricing   |
| Assign Branch | Searchable Dropdown | Yes      | Active branches from Module 7                              | Nearest branch auto-suggested |

**Business Rules:**

- First location is auto-populated from lead/customer address if available
- Additional locations can be added via "Add Location" button
- Each location gets independent service selection and pricing
- Branch assignment determines which branch handles that location

---

## Section 4: Service Selection Fields

_(Integrated with Module 12 – Service Management)_

| Field        | Type            | Required | Validation                                            | Notes                                   |
| ------------ | --------------- | -------- | ----------------------------------------------------- | --------------------------------------- |
| Service Name | Search Dropdown | Yes      | Active services from Module 12                        | Fetches service configuration           |
| Pest Type    | Auto-filled     | System   | From service master                                   | Read-only                               |
| Category     | Auto-filled     | System   | Residential / Commercial matched to location          | Read-only                               |
| Price Type   | Auto-filled     | System   | Fixed / Area Based / Inspection                       | From service master                     |
| Rate (₹)     | Auto-calculated | System   | Based on Property Type from Module 12 pricing         | E.g., 3BHK = ₹1,800 for Termite Control |
| Warranty     | Auto-filled     | System   | Period + revisit count from service config            | Read-only                               |
| Frequency    | Dropdown        | Yes      | One-Time / Monthly / Quarterly / Half-Yearly / Yearly | Overrides global if needed              |
| Qty / Visits | Auto-calculated | System   | Based on frequency × duration                         | E.g., Monthly × 1 Year = 12             |
| Total (₹)    | Auto-calculated | System   | Rate × Qty/Visits                                     | Per-service total                       |

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
| Service Selected          | Auto-fetch pricing from Module 12 based on property type           |
| Product Selected          | Auto-fetch pricing, UOM, HSN from Module 10; tax from Module 9     |
| Property Type Changed     | Recalculate service pricing for affected location                  |
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

| Section             | Field              | Type            | Description                                |
| ------------------- | ------------------ | --------------- | ------------------------------------------ |
| **Status Bar**      | Status Timeline    | Progress Bar    | Visual status progression                  |
|                     | Current Status     | Badge           | Active status highlighted                  |
|                     | Status Timestamps  | DateTime        | When each status was reached               |
| **Client Info**     | Source             | Badge + Link    | Lead/Customer/New with clickable reference |
|                     | Client Name        | Text            | Full name of client                        |
|                     | Phone              | Text            | Contact number                             |
|                     | Email              | Text            | Email address                              |
|                     | Property Type      | Text            | Type of property                           |
|                     | Address            | Text            | Full address                               |
| **Configuration**   | Quotation Type     | Text            | Service / Product / Combined               |
|                     | Service Mode       | Text            | One-Time / AMC                             |
|                     | Frequency          | Text            | AMC frequency if applicable                |
|                     | Contract Duration  | Text            | AMC duration if applicable                 |
|                     | Proposed Start     | Date            | AMC start date if applicable               |
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

| Field           | Type      | Required    | Options/Validation                                                                          | Notes                         |
| --------------- | --------- | ----------- | ------------------------------------------------------------------------------------------- | ----------------------------- |
| Quotation ID    | Display   | System      | Read-only                                                                                   | Identifies the quotation      |
| Client Name     | Display   | System      | Read-only                                                                                   | From quotation record         |
| Type            | Display   | System      | Read-only                                                                                   | Service / Product / Combined  |
| Amount          | Display   | System      | Read-only                                                                                   | Grand total from quotation    |
| Status          | Display   | System      | Read-only (always "Draft")                                                                  | Confirms deletion eligibility |
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
