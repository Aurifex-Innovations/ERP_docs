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
| Status              | Auto           | System      | Default: NEW                                                                   | New / Qualified / Quotation send / Negotiation / Lost / Converted |
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
| Lead Type            | Dropdown  | Auto        | Service, Product, AMC                                        |                                        |
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
│  │  PestGuard but unhappy with results. Interested in our AMC package.     ││
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
│  │  │ Category*:       [▼ Residential ▼]                               │  │ │
│  │  │ Sub-Category*:   [▼ Internal ▼]                                  │  │ │
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
│  │  │ Category*:       [▼ Select Category ▼]                           │  │ │
│  │  │ Sub-Category*:   [▼ Select Sub-Category ▼]                       │  │ │
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
│  │  ── LOCATION 1: HSR Layout, Bengaluru ──                               │ │
│  │  [🔍 Search Service from Service Master...]                             │ │
│  │                                                                         │ │
│  │  ┌────────────┬──────────┬──────────┬──────────┬─────────┬────────────┐│ │
│  │  │Service Name│Pest Type │Price Type│Rate (₹)   │Qty      │Total (₹)   ││ │
│  │  │────────────┼──────────┼──────────┼──────────┼─────────┼────────────││ │
│  │  │Termite Ctrl│Termite   │Fixed     │₹1,800    │ 1       │₹1,800      ││ │
│  │  │  (SVC-001) │          │          │(Fetched) │         │            ││ │
│  │  └────────────┴──────────┴──────────┴──────────┴─────────┴────────────┘│ │
│  │                                                                         │ │
│  │  DYNAMIC PRICING CONDITIONS (FETCHED FROM MODULE 12)                    │ │
│  │  (Displays only one block based on Service Price Type)                  │ │
│  │                                                                         │ │
│  │  [OPTION 1: FIXED PRICE]                                                │ │
│  │  ┌─────────────────────────────────────────────────────────────┐        │ │
│  │  │ SERVICE: Termite Control (SVC-001)                          │        │ │
│  │  │ LOCATION: HSR Layout, BLR | Category: Residential           │        │ │
│  │  ├─────────────────────────────────────────────────────────────┤        │ │
│  │  │ SELECT PROPERTY SIZE / TYPE:                                │        │ │
│  │  │                                                             │        │ │
│  │  │ IF RESIDENTIAL:           IF COMMERCIAL:                    │        │ │
│  │  │ ( ) 1 BHK    (₹ 1,200)    ( ) Small Office  (₹ 2,500)       │        │ │
│  │  │ ( ) 2 BHK    (₹ 1,500)    ( ) Medium Office (₹ 4,500)       │        │ │
│  │  │ (●) 3 BHK    (₹ 1,800)    ( ) Large Office  (₹ 7,000)       │        │ │
│  │  │ ( ) 4 BHK+   (₹ 2,200)    ( ) Warehouse     (₹ 12,000)      │        │ │
│  │  │ ( ) Villa    (₹ 3,500)                                      │        │ │
│  │  │                                                             │        │ │
│  │  │ *Selection fetches rate automatically from Module 12.       │        │ │
│  │  └─────────────────────────────────────────────────────────────┘        │ │
│  │                                                                         │ │
│  │  [OPTION 2: AREA BASED]                                                 │ │
│  │  ┌─────────────────────────────────────────────────────────────┐        │ │
│  │  │ SERVICE: Cockroach Gel (SVC-002)                             │        │ │
│  │  ├─────────────────────────────────────────────────────────────┤        │ │
│  │  │ PRICING LOGIC: Base Price + (Rate per SQFT × Total Area)     │        │ │
│  │  ├─────────────────────────────────────────────────────────────┤        │ │
│  │  │ - Base Price: ₹500                                          │        │ │
│  │  │ - Rate/SQFT:  ₹2.00                                         │        │ │
│  │  │ - Total Area: 1200 sqft (from Location details)             │        │ │
│  │  ├─────────────────────────────────────────────────────────────┤        │ │
│  │  │ CALCULATION: 500 + (2.00 × 1200)                            │        │ │
│  │  │ RESULT:      ₹2,900                                         │        │ │
│  │  └─────────────────────────────────────────────────────────────┘        │ │
│  │                                                                         │ │
│  │  [OPTION 3: INSPECTION BASED]                                           │ │
│  │  ┌─────────────────────────────────────────────────────────────┐        │ │
│  │  │ SERVICE: Rodent Baiting (SVC-003)                            │        │ │
│  │  ├─────────────────────────────────────────────────────────────┤        │ │
│  │  │ - Inspection Fee: ₹300                                      │        │ │
│  │  │ - Note: The final service quotation will be generated and   │        │ │
│  │  │   shared with the client ONLY after the on-site physical    │        │ │
│  │  │   inspection visit is completed.                            │        │ │
│  │  └─────────────────────────────────────────────────────────────┘        │ │
│  │                                                                         │ │
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
│  │  ── LOCATION 2: Whitefield, Bengaluru ──                                │ │
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
| Address       | Text                | Yes      | Min 10 characters                                                                                                        | Service delivery address      |
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
| Service Name        | Search Dropdown     | Yes      | Active services from Module 12                                                                                           | Fetches service configuration           |
| Pest Type           | Auto-filled         | System   | From service master                                                                                                      | Read-only                               |
| Dynamic Pricing     | ASCII Block         | Auto     | Displays options based on Price Type (Fixed / Area / Inspection)                                                         | Per-service selection/calc              |
| Rate (₹)            | Auto-calculated     | System   | Based on Dynamic Selection (Fixed) or Area (Area-Based)                                                                 | Dynamic fetch from Module 12            |
| Warranty            | Auto-filled         | System   | Period + revisit count from service config                                                                               | Read-only                               |
| Frequency           | Dropdown            | Yes      | Single / Monthly / Quarterly / AMC                                                                                       | Determines revisit cycle                |
| Qty/Visits          | Number              | Yes      | Number of visits required                                                                                                | Default 1                               |
| Total (₹)           | Number              | Auto     | `Rate × Qty`                                                                                                             | Auto-calculated                         |
| Action              | Button              | —        | Add / Remove Service Row                                                                                                 | —                                       |

**Business Rules:**

- When service is selected, pricing auto-populates based on the **Selection at Service level or Area calculation**:
  - **Fixed Price**: Matches BHK/Office size pricing from Module 12 based on selection in the Dynamic Pricing block.
  - **Area Based**: Calculates `Base Price + (Rate per SQFT × Total SQFT)` from location details.
  - **Inspection Based**: Sets initial price to `Inspection Fee`. Final quotation updated post-visit.
- Warranty/Revisits: Inherited from Module 12 config for the selected service.
- If AMC is selected: Total = Rate × Number of visits in contract duration.
- Multiple services can be added per location, each following its own pricing model.

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
│  │  Service Mode:        AMC (Annual Maintenance Contract)                 │ │
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
