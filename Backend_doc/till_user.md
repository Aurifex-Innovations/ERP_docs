# 📘 ERP System - Module Wise Workflow Documentation

---

# 🎯 MODULE 1: AUTHENTICATION

## 1.1 Overview

Authentication module handles access control for two distinct user types:

- **Super Admin (Seravion)**: Platform owner access
- **Company Admin (Client)**: Tenant/company access

---

## 1.2 Super Admin (Seravion) Authentication Flow

```
┌─────────────────────────────────────────────────────────────┐
│                    SERAVION LOGIN SCREEN                     │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │  Username   │  │  Password   │  │      SUBMIT         │ │
│  │   [Text]    │  │   [Mask]    │  │      [Button]       │ │
│  └─────────────┘  └─────────────┘  └─────────────────────┘ │
│                                                              │
│  ❌ NO Forgot Password Link                                  │
│  ❌ NO Signup Link                                           │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│              CREDENTIALS VALIDATION                          │
│  • Check against Seravion internal user database             │
│  • No public registration exists                             │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
        ┌─────────────┐                 ┌─────────────┐
        │   VALID     │                 │   INVALID   │
        └──────┬──────┘                 └──────┬──────┘
               │                               │
               ▼                               ▼
    ┌─────────────────────┐           ┌─────────────────────┐
    │  SUPER ADMIN        │           │  SHOW ERROR MESSAGE │
    │  DASHBOARD          │           │  "Invalid Credentials"│
    │  (Module 3)         │           │  Return to Login    │
    └─────────────────────┘           └─────────────────────┘
```

### Screen Fields: Super Admin Login

| Field    | Type     | Required | Validation                 |
| -------- | -------- | -------- | -------------------------- |
| Username | Text     | Yes      | Internal Seravion username |
| Password | Password | Yes      | Strong password policy     |
| Submit   | Button   | -        | Triggers authentication    |

### Business Rules:

- **No self-registration**: Super Admin accounts created internally by Seravion only
- **No password recovery**: Managed internally by Seravion IT team
- **Secure access**: Dedicated login portal, separate from client login

---

## 1.3 Company Admin (Client) Sign Up Flow

```
┌─────────────────────────────────────────────────────────────┐
│               COMPANY ADMIN SIGN UP SCREEN                   │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Company Name                    [____________]     │    │
│  │  Authorized Person Name          [____________]     │    │
│  │  Phone                           [____________]     │    │
│  │  Email                           [____________]     │    │
│  │  Password                        [____________]     │    │
│  │  Re-enter Password               [____________]     │    │
│  │                                                     │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │         GET STARTED BUTTON                  │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ❌ NO Google Signup Button                                  │
│  ❌ NO Social Login Options                                  │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│              VALIDATION CHECKS                               │
│  • Email uniqueness check                                    │
│  • Phone number format (10 digits)                           │
│  • Password strength (min 8 chars, complexity)               │
│  • Password match confirmation                               │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
        ┌─────────────┐                 ┌─────────────┐
        │   VALID     │                 │   INVALID   │
        └──────┬──────┘                 └──────┬──────┘
               │                               │
               ▼                               ▼
    ┌─────────────────────┐           ┌─────────────────────┐
    │  CREATE TENANT      │           │  SHOW FIELD ERRORS  │
    │  RECORD             │           │  Highlight issues   │
    │                     │           │  Allow correction   │
    │  Status: PENDING    │           │                     │
    │  Approval           │           │                     │
    └──────────┬──────────┘           └─────────────────────┘
               │
               ▼
    ┌─────────────────────┐
    │  REDIRECT TO        │
    │  MODULE 2:          │
    │  USER ONBOARDING    │
    │  (Company Info)     │
    └─────────────────────┘
```

### Screen Fields: Company Admin Sign Up

| Field                  | Type     | Required | Validation Rules                   |
| ---------------------- | -------- | -------- | ---------------------------------- |
| Company Name           | Text     | Yes      | Min 3 chars, alphanumeric          |
| Authorized Person Name | Text     | Yes      | Full name of primary contact       |
| Phone                  | Phone    | Yes      | 10 digits, Indian format           |
| Email                  | Email    | Yes      | Unique across platform             |
| Password               | Password | Yes      | Min 8 chars, 1 uppercase, 1 number |
| Re-enter Password      | Password | Yes      | Must match password field          |
| Get Started            | Button   | -        | Triggers registration              |

---

## 1.4 Company Admin (Client) Login Flow

```
┌─────────────────────────────────────────────────────────────┐
│                COMPANY ADMIN LOGIN SCREEN                    │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Email / Username                [____________]     │    │
│  │  Password                        [____________]     │    │
│  │                                                     │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │           SUBMIT BUTTON                     │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│              AUTHENTICATION CHECK                            │
│  • Verify email/username exists                              │
│  • Validate password hash                                    │
│  • Check tenant status                                       │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
        ┌─────────┐    ┌──────────┐    ┌──────────┐
        │  VALID  │    │ INVALID  │    │ PENDING  │
        │CREDENTIALS   │CREDENTIALS    │APPROVAL   │
        └────┬────┘    └────┬─────┘    └────┬─────┘
             │               │               │
             ▼               ▼               ▼
    ┌─────────────────┐ ┌──────────┐ ┌─────────────────┐
    │ CHECK DOCUMENT  │ │ SHOW     │ │ SHOW WAITING    │
    │ APPROVAL STATUS │ │ "Invalid │ │ SCREEN          │
    │                 │ │ Login"   │ │ (Module 2.1)    │
    │ (From Module 2) │ │          │ │                 │
    └────────┬────────┘ └──────────┘ └─────────────────┘
             │
    ┌────────┴────────┐
    ▼                 ▼
┌──────────┐    ┌──────────┐
│ APPROVED │    │ REJECTED │
│          │    │          │
│ Route to │    │ Route to │
│ Dashboard│    │ Rejection│
│ or       │    │ Screen   │
│ Subscription│  │ (Module  │
│ (Module 4)│   │ 2.1)     │
└──────────┘    └──────────┘
```

### Screen Fields: Company Admin Login

| Field            | Type     | Required | Notes                        |
| ---------------- | -------- | -------- | ---------------------------- |
| Email / Username | Text     | Yes      | Registered email or username |
| Password         | Password | Yes      | Case-sensitive               |
| Submit           | Button   | -        | Triggers login               |

---

# 🎯 MODULE 2: USER ONBOARDING

## 2.1 Overview

Post-authentication workflow for Company Admin to complete company profile and document verification before system access.

---

## 2.2 Complete Onboarding Flow

```
┌─────────────────────────────────────────────────────────────┐
│         TRIGGER: Company Admin Sign Up Complete              │
│                    OR                                        │
│         Login with Pending Document Status                   │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 1: COMPANY INFORMATION SCREEN                          │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Company Name                    [____________]     │    │
│  │  Industry Type                   [▼ Dropdown ]      │    │
│  │  Contact Person Name             [____________]     │    │
│  │  Contact Person Email            [____________]     │    │
│  │  Contact Person Phone            [____________]     │    │
│  │  GST Number                      [____________]     │    │
│  │  PAN Number                      [____________]     │    │
│  │  Address Line 1                  [____________]     │    │
│  │  Address Line 2                  [____________]     │    │
│  │  City                            [____________]     │    │
│  │  State                           [____________]     │    │
│  │  Pincode                         [____________]     │    │
│  │                                                     │    │
│  │  ⚠️ License Number is OPTIONAL                      │    │
│  │                                                     │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │         SAVE & CONTINUE                     │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 2: UPLOAD DOCUMENT SCREEN                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  GST Document                    [📎 Upload ]       │    │
│  │  PAN Document                    [📎 Upload ]       │    │
│  │  Business / Registration Doc     [📎 Upload ]       │    │
│  │                                                     │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │         SUBMIT FOR APPROVAL                 │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 3: DOCUMENT SUCCESS SCREEN                             │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                                                     │    │
│  │           ✅ DOCUMENTS SUBMITTED                   │    │
│  │                                                     │    │
│  │    "We will approve your access shortly."          │    │
│  │                                                     │    │
│  │    Your documents are under review by Seravion.    │    │
│  │    You will be notified once approved.             │    │
│  │                                                     │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  WAITING PERIOD (Background Process)                         │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  SUPER ADMIN REVIEW (Module 3.2)                    │    │
│  │  • Review uploaded documents                         │    │
│  │  • Verify GST/PAN authenticity                       │    │
│  │  • Check business registration                       │    │
│  │                                                     │    │
│  │  Decision: ───────────────────────►                 │    │
│  │            │  APPROVE  │  REJECT  │                 │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
              ┌───────────────┴───────────────┐
              ▼                               ▼
    ┌─────────────────────┐         ┌─────────────────────┐
    │   IF APPROVED       │         │   IF REJECTED       │
    │                     │         │                     │
    │  Redirect to:       │         │  Show:              │
    │  Subscription       │         │  DOCUMENT REJECTED  │
    │  Screen (Module 4)  │         │  SCREEN             │
    │                     │         │                     │
    │                     │         │  Display:           │
    │                     │         │  • Rejection reason │
    │                     │         │  • Re-upload option │
    │                     │         │                     │
    │                     │         │  Flow:              │
    │                     │         │  Re-upload →        │
    │                     │         │  Re-submit →        │
    │                     │         │  Back to Review     │
    └─────────────────────┘         └─────────────────────┘
```

### Screen 2.1: Company Information Fields

| Field                | Type     | Required | Notes                       |
| -------------------- | -------- | -------- | --------------------------- |
| Company Name         | Text     | Yes      | Pre-filled from signup      |
| Industry Type        | Dropdown | Yes      | Select from predefined list |
| Contact Person Name  | Text     | Yes      | Pre-filled from signup      |
| Contact Person Email | Email    | Yes      | Pre-filled from signup      |
| Contact Person Phone | Phone    | Yes      | Pre-filled from signup      |
| GST Number           | Text     | Yes      | 15-character GSTIN format   |
| PAN Number           | Text     | Yes      | 10-character alphanumeric   |
| Address Fields       | Text     | Yes      | Complete address            |
| License Number       | Text     | **NO**   | Optional field              |

### Screen 2.2: Upload Document Fields

| Field                          | Type        | Required | Format                   |
| ------------------------------ | ----------- | -------- | ------------------------ |
| GST Document                   | File Upload | Yes      | PDF, JPG, PNG (Max 5MB)  |
| PAN Document                   | File Upload | Yes      | PDF, JPG, PNG (Max 5MB)  |
| Business/Registration Document | File Upload | Yes      | PDF, JPG, PNG (Max 10MB) |

---

# 🎯 MODULE 3: SUPER ADMIN (SERAVION) MANAGEMENT

## 3.1 Overview

Internal Seravion operations module for tenant management, subscription oversight, and platform administration.

---

## 3.2 Super Admin Sidebar Structure

```
┌─────────────────────────────────────────────────────────────┐
│              SUPER ADMIN SIDEBAR (New)                       │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  📊 Dashboard                                        │    │
│  │     └── Customer approval & payment tracking         │    │
│  │                                                      │    │
│  │  💳 Subscription Plan Creation / View / Edit         │    │
│  │     └── Manage pricing plans for clients             │    │
│  │                                                      │    │
│  │  📈 Reports                                          │    │
│  │     └── Platform analytics & tenant reports          │    │
│  │                                                      │    │
│  │  🔐 Role Management                                  │    │
│  │     └── Define system-wide roles (Module 5.2)        │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ⚠️ VISIBLE ONLY TO SUPER ADMIN ROLE                         │
└─────────────────────────────────────────────────────────────┘
```

---

## 3.3 Dashboard Screen Flow (Enhanced)

```
┌─────────────────────────────────────────────────────────────┐
│              SUPER ADMIN DASHBOARD                           │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  CUSTOMER LIST TABLE (Enhanced)                      │    │
│  │                                                      │    │
│  │  Columns:                                            │    │
│  │  • Company Name                                      │    │
│  │  • Email                                             │    │
│  │  • Phone                                             │    │
│  │  • 📋 Doc Status [Pending/Approved/Rejected]         │    │
│  │  • 💰 Pay Status [Paid/Partial/Free/Non-Paid] ⭐ NEW │    │
│  │  • Created Date                                      │    │
│  │  • Actions                                           │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                              │                               │
│                              ▼                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  CLICK ON CUSTOMER ROW                              │    │
│  │  Opens: PARTICULAR CUSTOMER POPUP                   │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│      PARTICULAR CUSTOMER POPUP (Enhanced)                    │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  TAB 1: DOCUMENT STATUS & APPROVAL                   │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │                                                      │    │
│  │  Status Dropdown:          [▼ Pending/Approved/     │    │
│  │                             Rejected]               │    │
│  │                                                      │    │
│  │  ☐ Enable Trial (Checkbox) ⭐ NEW                    │    │
│  │                                                      │    │
│  │  IF CHECKED:                                         │    │
│  │    • From Date:        [📅 Date Picker]             │    │
│  │    • To Date:          [📅 Date Picker]             │    │
│  │                                                      │    │
│  │  [SAVE CHANGES]                                      │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                              │                               │
│                              ▼                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  TAB 2: SUBSCRIPTION DETAILS ⭐ NEW SECTION          │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │                                                      │    │
│  │  Subscription Purchase Details:                      │    │
│  │  • Plan Name:          [____________]               │    │
│  │  • Duration:           [Monthly/Quarterly/Yearly]   │    │
│  │  • Branch Count:       [____]                       │    │
│  │  • Technician Count:   [____]                       │    │
│  │  • Payment Status:     [Paid/Partial/Free/Non-Paid] │    │
│  │  • Start Date:         [📅 ____________]            │    │
│  │  • End Date:           [📅 ____________]            │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Dashboard Field Changes

| Original Field | New Field            | Options                            |
| -------------- | -------------------- | ---------------------------------- |
| Status         | **Doc Status**       | Pending, Approved, Rejected        |
| -              | **Pay Status** (New) | Paid, Partial Paid, Free, Non Paid |

### Popup Fields

| Field            | Type     | Behavior                         |
| ---------------- | -------- | -------------------------------- |
| Status           | Dropdown | Controls document approval state |
| Enable Trial     | Checkbox | If checked, shows date fields    |
| From Date        | Date     | Trial start date                 |
| To Date          | Date     | Trial end date                   |
| Plan Name        | Text     | Selected subscription plan       |
| Duration         | Text     | Billing cycle                    |
| Branch Count     | Number   | Allowed branches                 |
| Technician Count | Number   | Allowed technicians              |
| Payment Status   | Dropdown | Current payment state            |
| Start Date       | Date     | Subscription start               |
| End Date         | Date     | Subscription end                 |

---

# 🎯 MODULE 4: SUBSCRIPTION

## 4.1 Overview

Handles plan selection, pricing configuration, and subscription lifecycle management for both client and Super Admin sides.

---

## 4.2 Client Side Subscription Flow

```
┌─────────────────────────────────────────────────────────────┐
│  TRIGGER: Document Approved (from Module 3)                  │
│  OR User clicks "Purchase New Plan" in sidebar               │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 1: SUBSCRIPTION SCREEN (Client Side)                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                                                      │    │
│  │  Plan Selection:         [▼ Select Plan    ▼]       │    │
│  │                                                      │    │
│  │  Plan Details:           [Short description         │    │
│  │                           displayed here]           │    │
│  │                                                      │    │
│  │  Pricing Structure:                                  │    │
│  │  • Per Branch Price:     ₹ ______ / unit            │    │
│  │  • Per Technician Price: ₹ ______ / unit            │    │
│  │                                                      │    │
│  │  Duration:                                           │    │
│  │  ○ Monthly       ○ Quarterly      ○ Yearly          │    │
│  │                                                      │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │            PAY NOW BUTTON                   │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ⚠️ ACCESSIBLE ONLY AFTER DOCUMENT APPROVAL                  │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 2: PAYMENT PROCESSING                                  │
│  • Calculate total based on:                                 │
│    - Number of branches                                      │
│    - Number of technicians                                   │
│    - Duration selected                                       │
│  • Process payment                                           │
│  • Update Pay Status                                         │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 3: SUBSCRIPTION ACTIVATED                              │
│  • Grant full system access                                  │
│  • Redirect to Company Admin Dashboard                       │
└─────────────────────────────────────────────────────────────┘
```

### Subscription Screen Fields

| Field                | Type                 | Required | Behavior                            |
| -------------------- | -------------------- | -------- | ----------------------------------- |
| Plan Selection       | Dropdown             | Yes      | Lists active plans from Super Admin |
| Plan Details         | Text (Read-only)     | Auto     | Description of selected plan        |
| Per Branch Price     | Currency (Read-only) | Auto     | From plan configuration             |
| Per Technician Price | Currency (Read-only) | Auto     | From plan configuration             |
| Duration             | Radio Button         | Yes      | Monthly/Quarterly/Yearly            |
| Pay Now              | Button               | -        | Triggers payment gateway            |

---

## 4.3 Company Admin Subscription Module (Sidebar)

```
┌─────────────────────────────────────────────────────────────┐
│     SUBSCRIPTION MODULE (New Sidebar Item)                   │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  SUBSCRIPTION PURCHASED LIST (Logs)                  │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │  Table:                                              │    │
│  │  • Plan Name                                         │    │
│  │  • Purchase Date                                     │    │
│  │  • Duration                                          │    │
│  │  • Amount Paid                                       │    │
│  │  • Payment Status [Paid/Partial/Free/Non-Paid]       │    │
│  │  • Actions: [View Details]                           │    │
│  │                                                      │    │
│  │  [PURCHASE NEW PLAN] → Redirects to 4.1              │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

## 4.4 Super Admin Subscription Plan Management

```
┌─────────────────────────────────────────────────────────────┐
│     SUBSCRIPTION PLANS (Super Admin Sidebar)                 │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  [+ ADD NEW PLAN]                                    │    │
│  │                                                      │    │
│  │  EXISTING PLANS LIST:                                │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │ Plan Name │ Branch ₹ │ Tech ₹ │ Duration │ Actions│    │
│  │  │───────────┼──────────┼────────┼──────────┼────────│    │
│  │  │ Basic     │ 500      │ 100    │ Monthly  │ View   │    │
│  │  │           │          │        │          │ Edit   │    │
│  │  │           │          │        │          │ Delete │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                              │                               │
│                              ▼                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  ADD/EDIT PLAN FORM                                  │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │                                                      │    │
│  │  Plan Name:              [____________]             │    │
│  │                                                      │    │
│  │  Per Branch Pricing:                                 │    │
│  │  • Branch Count:         [____] units               │    │
│  │  • Price Per Branch:     ₹ [____]                   │    │
│  │                                                      │    │
│  │  Per Technician Pricing:                             │    │
│  │  • Technician Count:     [____] units               │    │
│  │  • Price Per Technician: ₹ [____]                   │    │
│  │                                                      │    │
│  │  Description:            [____________________]     │    │
│  │                          [Textarea]                 │    │
│  │                                                      │    │
│  │  Duration Options:                                   │    │
│  │  ☑ Monthly  ☑ Quarterly  ☑ Yearly                   │    │
│  │                                                      │    │
│  │  [SAVE]  [CANCEL]                                    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Super Admin Plan Fields

| Field                | Type         | Required | Notes                    |
| -------------------- | ------------ | -------- | ------------------------ |
| Plan Name            | Text         | Yes      | Unique identifier        |
| Branch Count         | Number       | Yes      | Base branch quantity     |
| Price Per Branch     | Currency     | Yes      | Unit price               |
| Technician Count     | Number       | Yes      | Base technician quantity |
| Price Per Technician | Currency     | Yes      | Unit price               |
| Description          | Textarea     | No       | Plan features            |
| Duration             | Multi-select | Yes      | Available billing cycles |

---

# 🎯 MODULE 5: ROLE MANAGEMENT

## 5.1 Overview

Central RBAC (Role-Based Access Control) module with three sub-modules:

- 5.1: Company Admin Role Management
- 5.2: Super Admin Role Management
- 5.3: Role Salary & Leave Configuration

---

## 5.2 Complete RBAC Architecture Flow

```
┌─────────────────────────────────────────────────────────────┐
│                    ROLE HIERARCHY                            │
│                                                              │
│  LEVEL 1: SUPER ADMIN (Seravion)                             │
│  └── Creates system-wide role templates (Module 5.2)         │
│       └── Example: "Sales Manager Template"                  │
│                                                              │
│  LEVEL 2: COMPANY ADMIN (Client)                             │
│  ├── Selects from Seravion templates                         │
│  ├── Customizes for their organization (Module 5.1)          │
│  └── Configures salary/leave structures (Module 5.3)         │
│                                                              │
│  LEVEL 3: EMPLOYEE                                           │
│  └── Assigned role → Inherits all configurations             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

I'll update the flow diagram to incorporate your new permission structure with the `REQUEST` action and role-based routing. Here's the revised version:

```
┌─────────────────────────────────────────────────────────────┐
│     ROLE MANAGEMENT (Company Admin Sidebar - New)            │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  [+ ADD ROLE]                                        │    │
│  │                                                      │    │
│  │  EXISTING ROLES LIST:                                │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │ Role Name │ Users │ Created │ Actions       │    │    │
│  │  │───────────┼───────┼─────────┼───────────────│    │    │
│  │  │ Sales Mgr │ 5     │ Jan 24  │ View Edit Del │    │    │
│  │  │ Acc Exec  │ 3     │ Jan 24  │ View Edit Del │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │                                                      │    │
│  │  [MIGRATE ROLE] - Move users between roles           │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                              │                               │
│                              ▼                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  ADD ROLE FORM                                       │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │                                                      │    │
│  │  Step 1: Basic Info                                  │    │
│  │  ─────────────────                                   │    │
│  │  Select Role from dropdown:    [▼ Seravion Templates│    │
│  │                                  ▼]                 │    │
│  │                                                      │    │
│  │  Clone permission from:        [▼ Existing Role ▼]  │    │
│  │  (Optional)                                         │    │
│  │                                                      │    │
│  │  Role Name:                    [____________]       │    │
│  │  (e.g., "Senior Sales Manager")                     │    │
│  │                                                      │    │
│  │  ☐ Is Application User ⭐ CRITICAL CHECKBOX         │    │
│  │                                                      │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │                                                      │    │
│  │  IF "Is Application User" = CHECKED:                │    │
│  │    → SKIP Step 2 (Module Permissions hidden)        │    │
│  │    → User gets mobile app access only               │    │
│  │    → No web dashboard access                        │    │
│  │                                                      │    │
│  │  IF "Is Application User" = UNCHECKED:              │    │
│  │    → SHOW Step 2 with full permission grid          │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼ (If Is Application User = false)
┌─────────────────────────────────────────────────────────────┐
│  Step 2: Module Permissions (25+ Modules)                    │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                                                      │    │
│  │  ┌─────────────────────────────────────────────────┐│    │
│  │  │ MODULE: User Management                         ││    │
│  │  │ ☑ VIEW  ☑ ADD  ☑ EDIT  ☑ DELETE  ☑ EXPORT      ││    │
│  │  │ ☑ CONFIGURE                                     ││    │
│  │  │                                                  ││    │
│  │  │ ☑ APPROVAL  →  Can approve requests for:        ││    │
│  │  │                [▼ Sales Person ▼]               ││    │
│  │  │                [▼ Account Executive ▼]          ││    │
│  │  │                [+ Add another role]             ││    │
│  │  │                                                  ││    │
│  │  │ ☑ REQUEST   →  Routes approval to:              ││    │
│  │  │                [▼ Sales Manager ▼]              ││    │
│  │  │                (Select multiple approver role)     ││    │
│  │  └─────────────────────────────────────────────────┘│    │
│  │                                                      │    │
│  │  ┌─────────────────────────────────────────────────┐│    │
│  │  │ MODULE: Lead Management                         ││    │
│  │  │ ☑ VIEW  ☑ ADD  ☐ EDIT  ☐ DELETE  ☑ EXPORT      ││    │
│  │  │ ☐ CONFIGURE                                     ││    │
│  │  │                                                  ││    │
│  │  │ ☐ APPROVAL                                      ││    │
│  │  │ ☑ REQUEST   →  Routes to: [▼ Sales Manager ▼]   ││    │
│  │  └─────────────────────────────────────────────────┘│    │
│  │                                                      │    │
│  │  [Repeat for 25+ modules...]                        │    │
│  │                                                      │    │
│  │  [PREVIOUS]  [SAVE ROLE]  [CANCEL]                  │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Updated Permission Toggle Behavior

| Permission    | Description                                                         | Dependencies                                    | Typical Roles                |
| ------------- | ------------------------------------------------------------------- | ----------------------------------------------- | ---------------------------- |
| **VIEW**      | Read/view records within the module                                 | Base permission                                 | All Roles                    |
| **ADD**       | Create new records                                                  | Usually paired with VIEW                        | Executives, Managers, Admins |
| **EDIT**      | Modify existing records                                             | Requires VIEW                                   | Executives, Managers, Admins |
| **DELETE**    | Remove or deactivate records — restricted by default                | Requires VIEW                                   | Managers, Admins only        |
| **APPROVAL**  | Review and approve workflow requests from team members              | Requires VIEW; Shows role multi-select dropdown | Managers, Admins             |
| **REQUEST**   | Initiate a request that routes to an approver                       | Requires VIEW; Shows single role dropdown       | Executives, Employees        |
| **EXPORT**    | Download module data as CSV, PDF, or Excel                          | Requires VIEW                                   | Managers, Auditors, Admins   |
| **CONFIGURE** | Master setting access — module configuration and system preferences | Admin-level                                     | Admins only                  |

### Key Workflow Logic

```
┌─────────────────────────────────────────────────────────┐
│              REQUEST → APPROVAL FLOW                     │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  1. User with REQUEST permission initiates action       │
│     └─ System checks: "Which role can approve this?"    │
│        └─ Looks up APPROVAL permission in same module   │
│                                                          │
│  2. Request routes to users with:                       │
│     • APPROVAL permission = CHECKED                     │
│     • Can approve for: [Requester's Role]               │
│                                                          │
│  3. Approver receives notification → Reviews → Acts     │
│                                                          │
│  Example:                                               │
│  ┌─────────────┐      REQUEST      ┌─────────────┐     │
│  │ Sales Person│ ─────────────────→│ Sales Mgr   │     │
│  │ (REQUEST→   │   "Discount >10%" │ (APPROVAL→  │     │
│  │  Sales Mgr) │                   │  Sales Person│    │
│  └─────────────┘                   └─────────────┘     │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

### Validation Rules

| Rule                         | Behavior                                                    |
| ---------------------------- | ----------------------------------------------------------- |
| **REQUEST without APPROVAL** | Warning: "No approver role configured for this module"      |
| **Circular Approval**        | Prevent: Role A approves Role B who approves Role A         |
| **Self-Approval**            | Option: Allow/Block users from approving their own requests |
| **Multi-level Approval**     | Chain: Request → Manager → Director (if configured)         |

## 5.4 Super Admin Role Management Flow

```
┌─────────────────────────────────────────────────────────────┐
│     ROLE MANAGEMENT (Super Admin Sidebar)                    │
│                                                              │
│  Similar structure to Company Admin, but:                    │
│  • Creates TEMPLATES, not specific roles                     │
│  • "Approval Authority" dropdown shows USERS, not roles      │
│  • Changes affect all tenants using template                 │
│                                                              │
│  ⚠️ EDIT WARNING:                                            │
│  "This is a system template. Changes may affect multiple     │
│   organizations. Proceed with caution."                      │
│                                                              │
│  ⚠️ DELETE WARNING:                                          │
│  "This template is used by X companies. Migrate users first" │                                                            │
└─────────────────────────────────────────────────────────────┘
```

---

## 5.5 Role Salary & Leave Configuration Flow

```
┌─────────────────────────────────────────────────────────────┐
│  ROLE SALARY & LEAVE CONFIG (Company Admin Sidebar - New)    │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  [+ ADD CONFIGURATION]                               │    │
│  │                                                      │    │
│  │  EXISTING CONFIGURATIONS:                            │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │ Role      │ Effective    │ Status │ Actions │    │    │
│  │  │           │ From         │        │         │    │    │
│  │  │───────────┼──────────────┼────────┼─────────│    │    │
│  │  │ Sales Mgr │ 01-Jan-2024  │ Active │ View... │    │    │
│  │  │ Tech Lead │ 01-Feb-2024  │ Active │ View... │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                              │                               │
│                              ▼                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  ADD/EDIT CONFIGURATION FORM                         │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │                                                      │    │
│  │  BASIC DETAILS                                       │    │
│  │  ─────────────                                       │    │
│  │  Select Role:              [▼ Dropdown ▼]           │    │
│  │  Effective From Date:      [📅 ________] *Required  │    │
│  │  Effective To Date:        [📅 ________] Optional   │    │
│  │  Status:                   [▼ Active/Inactive ▼]    │    │
│  │                                                      │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │  SALARY CONFIGURATION                                │    │
│  │  ───────────────────                                 │    │
│  │  Salary Type:              [▼ CTC/Fixed/Hourly ▼] * │    │
│  │  Default Basic Salary:     ₹ [________] *Required   │    │
│  │  Default HRA:              ₹ [________]             │    │
│  │  Default Other Allowance:  ₹ [________]             │    │
│  │  Default Incentive:        ₹ [________]             │    │
│  │  Default Deductions:       ₹ [________]             │    │
│  │                                                      │    │
│  │  Statutory:                                          │    │
│  │  ☐ PF Applicable    ☐ ESI Applicable                │    │
│  │  ☐ TDS Applicable                                   │    │
│  │                                                      │    │
│  │  HOLIDAY WORK CONFIGURATION                          │    │
│  │  ──────────────────────────                          │    │
│  │  ☐ Holiday Work Incentive Applicable                │    │
│  │    IF CHECKED:                                       │    │
│  │    • Type: [▼ Fixed/Per Day/Per Hour ▼]             │    │
│  │    • Default Amount: ₹ [________]                   │    │
│  │                                                      │    │
│  │  OVERTIME CONFIGURATION                              │    │
│  │  ──────────────────────                              │    │
│  │  ☐ Overtime Applicable                              │    │
│  │    IF CHECKED:                                       │    │
│  │    • Type: [▼ Per Hour/Per Shift ▼]                 │    │
│  │    • Shift Type: [________]                         │    │
│  │    • Shift Incentive: ₹ [________]                  │    │
│  │    • Per Hour Pay: ₹ [________]                     │    │
│  │    • Max OT Hours/Month: [____]                     │    │
│  │                                                      │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │  LEAVE CONFIGURATION                                 │    │
│  │  ──────────────────                                  │    │
│  │  Casual Leave (CL)/Year:   [____] days              │    │
│  │  Sick Leave (SL)/Year:     [____] days              │    │
│  │  Paid Leave (PL)/Year:     [____] days              │    │
│  │  Annual Leave Allocation:  [____] days              │    │
│  │                                                      │    │
│  │  ☐ Carry Forward Allowed                            │    │
│  │    IF CHECKED:                                       │    │
│  │    • Max Carry Forward Days: [____]                 │    │
│  │                                                      │    │
│  │  Leave Approval Authority: [▼ Select Role ▼] *Req   │    │
│  │  Leave Reset Cycle:        [▼ Yearly/Monthly ▼] *Req│    │
│  │                                                      │    │
│  │  [SAVE]  [CLONE TO ANOTHER ROLE]  [CANCEL]          │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Configuration Usage Flow

```
┌─────────────────────────────────────────────────────────────┐
│         HOW CONFIGURATION FLOWS TO USER CREATION             │
│                                                              │
│  1. Admin creates Role Configuration (Module 5.3)            │
│     └── Saves to Role_Compensation_Configuration table       │
│                                                              │
│  2. Admin creates User (Module 7.4)                          │
│     └── Selects Role in Step 1                               │
│                                                              │
│  3. System AUTO-FETCHES:                                     │
│     • Salary details → Pre-fills Step 3                      │
│     • Leave details → Pre-fills Step 4                       │
│                                                              │
│  4. Admin CAN:                                               │
│     • Accept defaults (no changes)                           │
│     • Override specific fields                               │
│                                                              │
│  5. Final values saved to Employee record                    │
│     (Role config remains unchanged)                          │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

# 🎯 MODULE 6: BRANCH MANAGEMENT

## 6.1 Overview

Manages organizational locations with hierarchical structure and employee associations.

---

## 6.2 Branch Management Flow

```
┌─────────────────────────────────────────────────────────────┐
│     BRANCH MANAGEMENT (Client Side)                          │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  [+ ADD BRANCH]                                      │    │
│  │                                                      │    │
│  │  BRANCH LIST TABLE (Modified):                       │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │ Name │ 3-Code │ Status │ Actions            │    │    │
│  │  │      │ Letter │        │                    │    │    │
│  │  │──────┼────────┼────────┼────────────────────│    │    │
│  │  │North │ NTH    │ Active │ View Edit Delete   │    │    │
│  │  │South │ STH    │ Active │ View Edit Delete   │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │                                                      │    │
│  │  ❌ REMOVED: Type column, City column, State column  │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                              │                               │
│                              ▼                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  ADD/EDIT BRANCH FORM                                │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │                                                      │    │
│  │  Branch Name:              [____________] *         │    │
│  │                                                      │    │
│  │  Address:                                              │    │
│  │  • Line 1:                 [____________]           │    │
│  │  • Line 2:                 [____________]           │    │
│  │  • City:                   [____________]           │    │
│  │  • State:                  [____________]           │    │
│  │  • Pincode:                [____________]           │    │
│  │                                                      │    │
│  │  3 Letter Code:            [___] *Required          │    │
│  │  (Unique identifier, e.g., NTH, STH, BOM)           │    │
│  │                                                      │    │
│  │  Branch Type:              [▼ Dropdown ▼]           │    │
│  │    • HEAD_OFFICE                                     │    │
│  │    • STATE_BRANCH                                    │    │
│  │    • CITY_BRANCH                                     │    │
│  │    • WAREHOUSE                                       │    │
│  │                                                      │    │
│  │  Created By (Add) /                                    │    │
│  │  Edited By (Edit):         [Auto-filled]            │    │
│  │                                                      │    │
│  │  [SAVE]  [CANCEL]                                    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                              │                               │
│                              ▼                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  VIEW BRANCH DETAILS (2 Tabs)                        │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │                                                      │    │
│  │  [TAB 1: Branch Info]  [TAB 2: Employees]           │    │
│  │                                                      │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │  TAB 1 CONTENT:                                      │    │
│  │  • Branch Name                                       │    │
│  │  • Address (Full)                                    │    │
│  │  • 3 Letter Code                                     │    │
│  │  • Status (Active/Inactive)                          │    │
│  │  • Created Date & By                                 │    │
│  │  • Edited Date & By                                  │    │
│  │                                                      │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │  TAB 2 CONTENT:                                      │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │ Emp ID │ Name │ Email │ Phone │ Designation │    │    │
│  │  │        │      │       │       │ Staff Status│    │    │
│  │  │────────┼──────┼───────┼───────┼─────────────│    │    │
│  │  │ E001   │ John │ j@... │ 98... │ Mgr-Active  │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Branch Fields

| Field          | Type     | Required | Notes                    |
| -------------- | -------- | -------- | ------------------------ |
| Branch Name    | Text     | Yes      | Display name             |
| Address Line 1 | Text     | Yes      | Street address           |
| Address Line 2 | Text     | No       | Additional info          |
| City           | Text     | Yes      | Part of address          |
| State          | Text     | Yes      | Part of address          |
| Pincode        | Text     | Yes      | Postal code              |
| 3 Letter Code  | Text     | Yes      | Unique short code        |
| Branch Type    | Dropdown | Yes      | Hierarchical type        |
| Created By     | Auto     | System   | Current user             |
| Edited By      | Auto     | System   | Current user (edit only) |

---

# 🎯 MODULE 7: USER MANAGEMENT

## 7.1 Overview

Complete employee lifecycle management with request-based hiring workflow, dynamic permission loading, and multi-step user creation.

---

## 7.2 Tab Visibility Control Flow

```
┌─────────────────────────────────────────────────────────────┐
│           TAB VISIBILITY DECISION TREE                       │
│                                                              │
│                    ┌─────────────────┐                       │
│                    │   USER LOGIN    │                       │
│                    └────────┬────────┘                       │
│                             │                                │
│                    ┌────────▼────────┐                       │
│                    │  CHECK USER ROLE │                       │
│                    └────────┬────────┘                       │
│                             │                                │
│         ┌───────────────────┼───────────────────┐            │
│         ▼                   ▼                   ▼            │
│    ┌─────────┐        ┌─────────┐        ┌─────────┐        │
│    │ LOWER   │        │ UPPER   │        │ SUPER   │        │
│    │ LEVEL   │        │ LEVEL   │        │ ADMIN   │        │
│    │(Manager)│        │(Admin)  │        │         │        │
│    └────┬────┘        └────┬────┘        └────┬────┘        │
│         │                   │                   │            │
│    ┌────▼────┐         ┌────▼────┐        ┌────▼────┐       │
│    │Tab 1:   │         │Tab 1:   │        │Tab 1:   │       │
│    │USER LIST│         │USER LIST│        │USER LIST│       │
│    │    ✓    │         │    ✓    │        │    ✓    │       │
│    ├─────────┤         ├─────────┤        ├─────────┤       │
│    │Tab 2:   │         │Tab 2:   │        │Tab 2:   │       │
│    │SEND     │         │SEND     │        │SEND     │       │
│    │REQUEST  │         │REQUEST  │        │REQUEST  │       │
│    │    ✓    │         │    ✗    │        │Optional │       │
│    ├─────────┤         ├─────────┤        ├─────────┤       │
│    │Tab 3:   │         │Tab 3:   │        │Tab 3:   │       │
│    │RECEIVED │         │RECEIVED │        │RECEIVED │       │
│    │REQUESTS │         │REQUESTS │        │REQUESTS │       │
│    │    ✗    │         │    ✓    │        │    ✓    │       │
│    └─────────┘         └─────────┘        └─────────┘       │
│                                                              │
│  LEGEND: ✓ = Visible  ✗ = Hidden  Optional = Configurable   │
└─────────────────────────────────────────────────────────────┘
```

---

## 7.3 Complete User Management Workflow

```
┌─────────────────────────────────────────────────────────────┐
│  SCENARIO A: LOWER-LEVEL USER (Manager/Team Lead)            │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  TAB 1: USER LIST                                    │    │
│  │  • View employees in scope (branch-based)            │    │
│  │  • Actions: View only (no Edit/Delete)               │    │
│  │                                                      │    │
│  │  [SWITCH TO TAB 2]                                   │    │
│  │                              │                       │    │
│  │                              ▼                       │    │
│  │  TAB 2: SEND REQUEST TO ADD NEW EMPLOYEE             │    │
│  │                                                      │    │
│  │  A) REQUEST FORM:                                    │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │ Requested By:        [Auto: Current User]   │    │    │
│  │  │ Department:          [____________] *       │    │    │
│  │  │ Designation:         [____________] *       │    │    │
│  │  │ Proposed Role:       [▼ Role ▼] *           │    │    │
│  │  │ Branch:              [☑ Multi-select] *     │    │    │
│  │  │ Employment Type:     [▼ Perm/Contract/Int ▼]*│    │    │
│  │  │ Expected DOJ:        [📅 ________] *        │    │    │
│  │  │ Number of Positions: [____] *               │    │    │
│  │  │ Hiring Reason:       [______________] *     │    │    │
│  │  │ Job Description:     [______________]       │    │    │
│  │  │ Additional Remarks:  [______________]       │    │    │
│  │  │ Supporting Doc:      [📎 Upload ]           │    │    │
│  │  │                                              │    │    │
│  │  │ [SUBMIT REQUEST]  [CANCEL]                  │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │                                                      │    │
│  │  B) MY HIRING REQUESTS (Table):                      │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │ Req ID │ Dept │ Role │ Pos │ Status │ Date │    │    │
│  │  │────────┼──────┼──────┼─────┼────────┼──────│    │    │
│  │  │ HRQ001 │Sales │SP    │ 2   │Pending │Jan24 │    │    │
│  │  │ HRQ002 │Ops   │Tech  │ 1   │Approved│Jan24 │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │                                                      │    │
│  │  Actions: View | View Rejection Reason (if rejected) │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ❌ TAB 3: NOT VISIBLE (No approval authority)               │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  SCENARIO B: UPPER-LEVEL USER (Admin/HR/Company Admin)       │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  TAB 1: USER LIST                                    │    │
│  │  • View all employees (within scope)                 │    │
│  │  • Actions: View, Edit, Delete                       │    │
│  │  • [+ ADD USER] button visible                       │    │
│  │                                                      │    │
│  │  [SWITCH TO TAB 2]                                   │    │
│  │                              │                       │    │
│  │                              ▼                       │    │
│  │  TAB 2: SEND REQUEST (Optional for upper-level)      │    │
│  │  • Can submit requests on behalf of others           │    │
│  │  • OR use direct user creation                       │    │
│  │                                                      │    │
│  │  [SWITCH TO TAB 3]                                   │    │
│  │                              │                       │    │
│  │                              ▼                       │    │
│  │  TAB 3: RECEIVED REQUESTS TO ADD NEW EMPLOYEE        │    │
│  │                                                      │    │
│  │  A) RECEIVED REQUESTS TABLE:                         │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │ Req ID │ By │ Dept │ Role │ Pos │ Status   │    │    │
│  │  │────────┼────┼──────┼──────┼─────┼──────────│    │    │
│  │  │ HRQ001 │Raj │Sales │SP    │ 2   │● Pending │    │    │
│  │  │ HRQ002 │Sam │Ops   │Tech  │ 1   │✓ Approved│    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │                                                      │    │
│  │  Filters: Status | Dept | Branch | Requested By      │    │
│  │           Date Range | Search                        │    │
│  │                                                      │    │
│  │  B) ACTIONS ON PENDING REQUESTS:                     │    │
│  │  • [VIEW] - Full details modal                       │    │
│  │  • [APPROVE] - Status → Approved                     │    │
│  │  • [REJECT] - Mandatory rejection reason             │    │
│  │                                                      │    │
│  │  C) AFTER APPROVAL:                                  │    │
│  │  Admin can convert to employee via "Add User" form   │    │
│  │  (Pre-filled with request data)                      │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

## 7.4 Add User Form - Multi-Step Flow

```
┌─────────────────────────────────────────────────────────────┐
│           ADD USER FORM (5 Steps)                            │
│     Triggered by: Direct creation OR Approved Request        │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 1: PERSONAL DETAILS                                    │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  BASIC INFORMATION                                   │    │
│  │  ─────────────────                                   │    │
│  │  EMP ID:                 [____________] *           │    │
│  │  First Name:             [____________] *           │    │
│  │  Last Name:              [____________] *           │    │
│  │  Email:                  [____________]             │    │
│  │  Contact Number:         [____________] *           │    │
│  │  Alternate Number:       [____________]             │    │
│  │  Password:               [____________] * (Create)  │    │
│  │  Department:             [____________] *           │    │
│  │  Designation:            [____________] *           │    │
│  │                                                      │    │
│  │  ⚠️ ROLE SELECTION (Triggers everything below)       │    │
│  │  Role:                   [▼ Role ▼] * ⭐ CRITICAL    │    │
│  │                                                      │    │
│  │  Branch:                 [☑ Multi-select] *         │    │
│  │                                                      │    │
│  │  Reporting Manager:      [▼ Dropdown ▼] *           │    │
│  │  └─► Filtered by: Selected Branch + Role Hierarchy   │    │
│  │                                                      │    │
│  │  Employment Type:        [▼ Perm/Contract/Int ▼] *  │    │
│  │  Date of Joining:        [📅 ________] *            │    │
│  │  Status:                 [▼ Active/Inactive ▼] *    │    │
│  │                                                      │    │
│  │  CURRENT ADDRESS (All Required)                      │    │
│  │  • Line 1, Line 2, City, State, Country, Pincode    │    │
│  │                                                      │    │
│  │  PERMANENT ADDRESS (All Required)                    │    │
│  │  • Same as Current [☑ Checkbox]                     │    │
│  │  • Or manual entry                                  │    │
│  │                                                      │    │
│  │  [NEXT ►]  [CANCEL]                                  │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 2: MODULE PERMISSIONS                                  │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                                                      │    │
│  │  ☐ Is Application User                               │    │
│  │                                                      │    │
│  │  IF CHECKED → SKIP TO STEP 3 (Hide permissions)      │    │
│  │  └─► User gets mobile app only, no web access        │    │
│  │                                                      │    │
│  │  IF UNCHECKED → SHOW PERMISSION GRID:                │    │
│  │                                                      │    │
│  │  ┌─────────────────────────────────────────────────┐│    │
│  │  │ AUTO-LOADED FROM ROLE CONFIGURATION             ││    │
│  │  │                                                 ││    │
│  │  │ For each of 25+ Modules:                        ││    │
│  │  │ ☑ Add  ☑ View  ☑ Edit  ☐ Delete  ☑ Export      ││    │
│  │  │ ☑ Configure                                     ││    │
│  │  │ ☑ Approval Authority [▼ Can approve: Role ▼]    ││    │
│  │  │                                                 ││    │
│  │  │ [Admin can override any toggle]                 ││    │
│  │  └─────────────────────────────────────────────────┘│    │
│  │                                                      │    │
│  │  [◄ PREVIOUS]  [NEXT ►]  [SKIP]  [CANCEL]            │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 3: SALARY DETAILS                                      │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  AUTO-FETCHED FROM ROLE CONFIGURATION (If exists)    │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │                                                      │    │
│  │  Salary Type:            [▼ CTC/Fixed/Hourly ▼] *   │    │
│  │  Basic Salary:           ₹ [________] *             │    │
│  │  HRA:                    ₹ [________]               │    │
│  │  Other Allowance:        ₹ [________]               │    │
│  │  Incentive:              ₹ [________]               │    │
│  │  Deductions:             ₹ [________]               │    │
│  │                                                      │    │
│  │  ☐ PF Applicable  ☐ ESI Applicable  ☐ TDS Applicable│    │
│  │                                                      │    │
│  │  BANKING DETAILS                                     │    │
│  │  • Bank Name:            [____________] *           │    │
│  │  • Account Number:       [____________] *           │    │
│  │  • IFSC Code:            [____________] *           │    │
│  │  • Effective From:       [📅 ________] *            │    │
│  │  • Effective To:         [📅 ________]              │    │
│  │                                                      │    │
│  │  INCENTIVE CONFIGURATION                             │    │
│  │  ☐ Holiday Work Incentive                           │    │
│  │    └─► Type, Amount (conditional)                   │    │
│  │  ☐ Overtime                                         │    │
│  │    └─► Type, Shift, Per Hour, Max Hours (conditional)│   │
│  │                                                      │    │
│  │  [◄ PREVIOUS]  [NEXT ►]  [SKIP]  [CANCEL]            │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 4: LEAVE DETAILS                                       │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  AUTO-FETCHED FROM ROLE CONFIGURATION (If exists)    │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │                                                      │    │
│  │  Casual Leave (CL):      [____] days/year           │    │
│  │  Sick Leave (SL):        [____] days/year           │    │
│  │  Paid Leave (PL):        [____] days/year           │    │
│  │  Annual Leave Allocation:[____] days                │    │
│  │                                                      │    │
│  │  ☐ Carry Forward Allowed                            │    │
│  │    └─► Max Carry Forward Days: [____] (conditional) │    │
│  │                                                      │    │
│  │  Leave Approval Authority: [▼ Role ▼] *Required     │    │
│  │  Leave Reset Cycle:        [▼ Yearly/Monthly ▼] *   │    │
│  │                                                      │    │
│  │  [◄ PREVIOUS]  [NEXT ►]  [SKIP]  [CANCEL]            │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 5: DOCUMENTS & ADDITIONAL DATA                         │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  IDENTITY & COMPLIANCE (All Optional)                │    │
│  │  • Aadhar Number, PAN Number, UAN, ID Card Number   │    │
│  │                                                      │    │
│  │  DOCUMENT UPLOADS (All Optional)                     │    │
│  │  • Aadhar, PAN, Address Proof, Education,           │    │
│  │    Experience, Offer Letter, Appointment, Others    │    │
│  │                                                      │    │
│  │  ADDITIONAL PROFESSIONAL DATA (All Optional)         │    │
│  │  • Grade/Level, Shift Type, Weekly Off              │    │
│  │  • Target Amount, Commission %                      │    │
│  │  • Employee Photo                                   │    │
│  │                                                      │    │
│  │  [◄ PREVIOUS]  [SAVE USER]  [CANCEL]                 │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                              │                               │
│                              ▼                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  SUCCESS: User created, credentials sent             │    │
│  │  If from Request: Request status → "Converted"       │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

## 7.5 Dynamic Permission Loading Flow

```
┌─────────────────────────────────────────────────────────────┐
│     HOW "ROLE SELECTION" TRIGGERS DYNAMIC LOADING            │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  EVENT: User selects "Role" in Step 1                        │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  PARALLEL DATA FETCHES (Async)                               │
│                                                              │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────┐ │
│  │ FETCH Module    │  │ FETCH Salary    │  │ FETCH Leave │ │
│  │ Permissions     │  │ Configuration   │  │ Configuration│ │
│  │                 │  │                 │  │             │ │
│  │ FROM:           │  │ FROM:           │  │ FROM:       │ │
│  │ Role_Module_    │  │ Role_Compensation│  │ Role_Leave_ │ │
│  │ Permission      │  │ _Configuration  │  │ Configuration│ │
│  │                 │  │                 │  │             │ │
│  │ WHERE:          │  │ WHERE:          │  │ WHERE:      │ │
│  │ role_id =       │  │ role_id =       │  │ role_id =   │ │
│  │ selected_role   │  │ selected_role   │  │ selected_role│ │
│  │ AND is_active   │  │ AND effective_  │  │ AND is_     │ │
│  │ = true          │  │ date <= today   │  │ active = true│ │
│  └────────┬────────┘  └────────┬────────┘  └──────┬──────┘ │
│           │                    │                   │        │
│           └────────────────────┼───────────────────┘        │
│                                ▼                            │
│                     ┌─────────────────────┐                 │
│                     │  PRE-FILL FORMS     │                 │
│                     │                     │                 │
│                     │  Step 2: Permissions│                 │
│                     │  Step 3: Salary     │                 │
│                     │  Step 4: Leave      │                 │
│                     │                     │                 │
│                     │  Show: "From Role   │                 │
│                     │  Config" badge on   │                 │
│                     │  pre-filled fields  │                 │
│                     └─────────────────────┘                 │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  ADMIN ACTIONS:                                              │
│  • Accept defaults (no changes) → Save as-is                │
│  • Override specific fields → Save with modifications       │
│  • Clear all → Start fresh (not recommended)                │
│                                                              │
│  NOTE: Changes here affect ONLY this user, not the role     │
│  template. Role configuration remains unchanged.            │
└─────────────────────────────────────────────────────────────┘
```

---

## 7.6 "Is Application User" Impact Flow

```
┌─────────────────────────────────────────────────────────────┐
│     "IS APPLICATION USER" CHECKBOX BEHAVIOR                  │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
    ┌─────────────────────┐         ┌─────────────────────┐
    │   CHECKED (true)    │         │  UNCHECKED (false)  │
    │                     │         │                     │
    │  MOBILE-ONLY USER   │         │  WEB DASHBOARD USER │
    │  (e.g., Technician) │         │  (e.g., Manager)    │
    │                     │         │                     │
    │  EFFECTS:           │         │  EFFECTS:           │
    │  ─────────          │         │  ─────────          │
    │  • Step 2 HIDDEN    │         │  • Step 2 VISIBLE   │
    │  • No module        │         │  • Full permission  │
    │    permissions      │         │    grid shown       │
    │  • Mobile app       │         │  • Web access       │
    │    credentials      │         │    granted          │
    │    generated        │         │  • Module access    │
    │  • GPS tracking     │         │    based on         │
    │    enabled          │         │    permissions      │
    │  • Task-based UI    │         │                     │
    │                     │         │                     │
    │  USE CASE:          │         │  USE CASE:          │
    │  Field staff who    │         │  Office staff who   │
    │  only execute tasks │         │  need dashboard     │
    │  via mobile app     │         │  access for mgmt    │
    │                     │         │                     │
    └─────────────────────┘         └─────────────────────┘
```

---

# 📊 MASTER FLOWCHART: COMPLETE USER JOURNEY

```
┌─────────────────────────────────────────────────────────────────────┐
│                         ENTRY POINTS                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │
│  │ Super Admin │  │ Company     │  │ Company     │  │ Existing    │ │
│  │ Login       │  │ Admin Signup│  │ Admin Login │  │ User Login  │ │
│  │ (1.1)       │  │ (1.2)       │  │ (1.3)       │  │ (Module 7)  │ │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘ │
│         │                │                │                │        │
│         ▼                ▼                ▼                ▼        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │
│  │ Super Admin │  │ Company     │  │ Check Doc   │  │ Check Role  │ │
│  │ Dashboard   │  │ Information │  │ Status      │  │ & Route to  │ │
│  │ (Module 3)  │  │ (Module 2.1)│  │ (Module 2.1)│  │ Dashboard   │ │
│  └─────────────┘  └──────┬──────┘  └──────┬──────┘  └─────────────┘ │
│                          │                │                          │
│                          ▼                ▼                          │
│                   ┌─────────────┐  ┌─────────────┐                   │
│                   │ Document    │  │ Waiting/    │                   │
│                   │ Upload      │  │ Rejected    │                   │
│                   │ (Module 2.1)│  │ (Module 2.1)│                   │
│                   └──────┬──────┘  └──────┬──────┘                   │
│                          │                │                          │
│                          ▼                │                          │
│                   ┌─────────────┐         │                          │
│                   │ Seravion    │◄────────┘                          │
│                   │ Review      │         (Re-upload loop)           │
│                   │ (Module 3.2)│                                      │
│                   └──────┬──────┘                                      │
│                          │                                            │
│              ┌───────────┴───────────┐                                │
│              ▼                       ▼                                │
│       ┌─────────────┐         ┌─────────────┐                         │
│       │  APPROVED   │         │  REJECTED   │                         │
│       └──────┬──────┘         └─────────────┘                         │
│              │                                                        │
│              ▼                                                        │
│       ┌─────────────┐                                                 │
│       │ Subscription│                                                 │
│       │ Selection   │                                                 │
│       │ (Module 4)  │                                                 │
│       └──────┬──────┘                                                 │
│              │                                                        │
│              ▼                                                        │
│       ┌─────────────┐                                                 │
│       │ Payment     │                                                 │
│       │ Processing  │                                                 │
│       └──────┬──────┘                                                 │
│              │                                                        │
│              ▼                                                        │
│       ┌─────────────┐     ┌─────────────────────────────────────────┐ │
│       │ Company     │────►│  MODULE 7: USER MANAGEMENT              │ │
│       │ Admin       │     │  (Role-based tab visibility)            │ │
│       │ Dashboard   │     │                                         │ │
│       └─────────────┘     │  ┌─────────┐  ┌─────────┐  ┌─────────┐  │ │
│                           │  │ Tab 1   │  │ Tab 2   │  │ Tab 3   │  │ │
│                           │  │ User    │  │ Send    │  │Received │  │ │
│                           │  │ List    │  │ Request │  │Requests │  │ │
│                           │  └────┬────┘  └────┬────┘  └────┬────┘  │ │
│                           │       │            │            │       │ │
│                           │       ▼            ▼            ▼       │ │
│                           │  ┌─────────┐  ┌─────────┐  ┌─────────┐  │ │
│                           │  │ View/   │  │ Create  │  │ Review/ │  │ │
│                           │  │ Edit    │  │ Hiring  │  │ Approve │  │ │
│                           │  │ Users   │  │ Request │  │ Requests│  │ │
│                           │  └─────────┘  └─────────┘  └─────────┘  │ │
│                           │                                         │ │
│                           │  ┌─────────────────────────────────┐    │ │
│                           │  │     ADD USER FORM (5 Steps)     │    │ │
│                           │  │  ┌─────┐┌─────┐┌─────┐┌─────┐┌────┐ │ │
│                           │  │  │Step1││Step2││Step3││Step4││Step5│ │ │
│                           │  │  │Personal│Module│Salary│Leave│Docs │ │ │
│                           │  │  │Details │Perm  │     │     │     │ │ │
│                           │  │  └─────┘└─────┘└─────┘└─────┘└────┘ │ │
│                           │  │       ▲                             │ │
│                           │  │       │ Role selection triggers      │ │
│                           │  │       │ dynamic config loading       │ │
│                           │  └─────────────────────────────────────┘ │
│                           └─────────────────────────────────────────┘
└─────────────────────────────────────────────────────────────────────┘
```

---

# 📋 QUICK REFERENCE: FIELD REQUIREMENTS SUMMARY

## Module 1: Authentication

| Screen            | Required Fields                                                      |
| ----------------- | -------------------------------------------------------------------- |
| Super Admin Login | Username, Password                                                   |
| Company Sign Up   | Company Name, Auth Person, Phone, Email, Password, Re-enter Password |
| Company Login     | Email/Username, Password                                             |

## Module 2: Onboarding

| Screen              | Required Fields                |
| ------------------- | ------------------------------ |
| Company Information | All except License Number      |
| Upload Document     | GST Doc, PAN Doc, Business Doc |

## Module 3: Super Admin

| Screen         | Required Fields                                 |
| -------------- | ----------------------------------------------- |
| Dashboard      | Doc Status, Pay Status (system)                 |
| Customer Popup | Status, Enable Trial (if checked: From/To Date) |

## Module 4: Subscription

| Screen              | Required Fields                               |
| ------------------- | --------------------------------------------- |
| Client Subscription | Plan Selection, Duration                      |
| Super Admin Plan    | Plan Name, Branch Price, Tech Price, Duration |

## Module 5: Role Management

| Screen      | Required Fields                                                           |
| ----------- | ------------------------------------------------------------------------- |
| Add Role    | Role Name, Is Application User (checkbox)                                 |
| Role Config | Role, Effective From, Salary Type, Basic Salary, Leave Approval Authority |

## Module 6: Branch Management

| Screen          | Required Fields                     |
| --------------- | ----------------------------------- |
| Add/Edit Branch | Branch Name, Address, 3 Letter Code |

## Module 7: User Management

### Required Fields Per Screen

| Screen          | Required Fields                                                                                                                                    |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hiring Request  | Department, Designation, Role, Branch, Employment Type, DOJ, Positions, Reason                                                                     |
| Add User Step 1 | EMP ID, First Name, Last Name, Contact, Department, Designation, Role, Branch, Reporting Manager, Employment Type, DOJ, Status, All Address fields |
| Add User Step 2 | Is Application User (if false: at least one module permission)                                                                                     |
| Add User Step 3 | Salary Type, Basic Salary, Bank Name, Account Number, IFSC, Effective From                                                                         |
| Add User Step 4 | Leave Approval Authority, Leave Reset Cycle                                                                                                        |
| Add User Step 5 | All optional                                                                                                                                       |

---

# 🎯 ROLE MANAGEMENT - OVERVIEW

## Architecture: Two-Level Role System

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         LEVEL 1: SERAVION SIDE                           │
│                    (Platform Owner - Super Admin)                        │
│                                                                          │
│  Purpose: Create system-wide role templates that clients can use         │
│                                                                          │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │  SUPER ADMIN CREATES ROLE TEMPLATES                             │    │
│  │                                                                 │    │
│  │  Examples:                                                      │    │
│  │  • "Sales Manager Template"                                     │    │
│  │  • "Account Executive Template"                                 │    │
│  │  • "Branch Admin Template"                                      │    │
│  │  • "Operations Head Template"                                   │    │
│  │                                                                 │    │
│  │  For Each Template, Defines:                                    │    │
│  │  ┌─────────────────────────────────────────────────────────┐   │    │
│  │  │ • Role Name                                             │   │    │
│  │  │ • Is Application User (checkbox)                        │   │    │
│  │  │ • 25+ Module Permissions (Add/View/Edit/Delete/Export/  │   │    │
│  │  │   Configure/Approval Authority)                         │   │    │
│  │  │                                                         │   │    │
│  │  │ Note: "Approval Authority" dropdown shows USERS here    │   │    │
│  │  │       (since Seravion manages platform-level access)    │   │    │
│  │  └─────────────────────────────────────────────────────────┘   │    │
│  │                                                                 │    │
│  │  These templates become available to ALL client companies       │    │
│  │                                                                 │    │
│  └─────────────────────────────────────────────────────────────────┘    │
│                                                                          │
│  ⚠️ Impact: Changes to templates affect future role creations,          │
│             not existing assigned roles                                  │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
                           ┌────────────────┐
                           │  TEMPLATES     │
                           │  AVAILABLE TO  │
                           │  ALL CLIENTS   │
                           └───────┬────────┘
                                   │
                                   ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      LEVEL 2: CLIENT SIDE                                │
│                 (Company Admin - Individual Tenant)                      │
│                                                                          │
│  Purpose: Customize Seravion templates for their organization            │
│                                                                          │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │  STEP 1: SELECT FROM SERAVION TEMPLATES                         │    │
│  │                                                                 │    │
│  │  Company Admin picks: "Sales Manager Template"                  │    │
│  │                    OR "Account Executive Template"              │    │
│  │                                                                 │    │
│  └─────────────────────────────────────────────────────────────────┘    │
│                                    │                                     │
│                                    ▼                                     │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │  STEP 2: CUSTOMIZE FOR THEIR COMPANY                            │    │
│  │                                                                 │    │
│  │  • Clone permissions from existing role (optional)              │    │
│  │  • Rename if needed (e.g., "Senior Sales Manager")              │    │
│  │  • Adjust module permissions (override template defaults)       │    │
│  │                                                                 │    │
│  │  Note: "Approval Authority" dropdown shows ROLES here           │    │
│  │        (e.g., Sales Manager can approve for Sales Person)       │    │
│  │                                                                 │    │
│  └─────────────────────────────────────────────────────────────────┘    │
│                                    │                                     │
│                                    ▼                                     │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │  STEP 3: CONFIGURE SALARY & LEAVE STRUCTURE (Module 5.3)        │    │
│  │                                                                 │    │
│  │  For each role, define:                                         │    │
│  │  • Salary components (Basic, HRA, Allowances, Incentives)       │    │
│  │  • Statutory deductions (PF, ESI, TDS applicability)            │    │
│  │  • Overtime and holiday work rules                              │    │
│  │  • Leave entitlements (CL, SL, PL, carry forward)               │    │
│  │  • Leave approval hierarchy                                     │    │
│  │                                                                 │    │
│  │  Effective dates control when configuration applies             │    │
│  │                                                                 │    │
│  └─────────────────────────────────────────────────────────────────┘    │
│                                    │                                     │
│                                    ▼                                     │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │  STEP 4: ASSIGN TO EMPLOYEES (Module 7)                         │    │
│  │                                                                 │    │
│  │  When creating/editing user:                                    │    │
│  │  • Select Role → Auto-loads permissions + salary + leave        │    │
│  │  • Admin can override at user level if needed                   │    │
│  │  • Employee inherits all role configurations                    │    │
│  │                                                                 │    │
│  └─────────────────────────────────────────────────────────────────┘    │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Key Differences: Seravion vs Client Side

| Aspect                          | Seravion (Super Admin)        | Client (Company Admin)                  |
| ------------------------------- | ----------------------------- | --------------------------------------- |
| **What they create**            | Role Templates                | Company-specific Roles                  |
| **Scope**                       | Platform-wide                 | Tenant-only                             |
| **Base for creation**           | From scratch                  | From Seravion templates                 |
| **Approval Authority dropdown** | Lists **Users**               | Lists **Roles**                         |
| **Salary/Leave config**         | Not applicable                | Configured per role                     |
| **Edit impact**                 | Affects future template usage | Affects only their company              |
| **Delete restrictions**         | Warn if used by clients       | Warn if users assigned, allow migration |

---

## Data Flow Summary

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   SERAVION  │────►│   TEMPLATE  │────►│    CLIENT   │────►│   EMPLOYEE  │
│   CREATES   │     │   STORED IN │     │  CUSTOMIZES │     │   ASSIGNED  │
│   TEMPLATE  │     │   SYSTEM    │     │   & USES    │     │   ROLE      │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
      │                                          │                  │
      │                                          │                  │
      └──────────────────────────────────────────┘                  │
                    Templates propagate to                          │
                    all tenants                                     │
                                                                     │
      ┌─────────────────────────────────────────────────────────────┘
      │
      ▼
┌─────────────┐
│  EMPLOYEE   │
│  INHERITS:  │
│  • Permissions from Role          │
│  • Salary structure from Role Config │
│  • Leave structure from Role Config  │
│  (All can be overridden at user level)│
└─────────────┘
```

---

## "Is Application User" Behavior (Both Sides)

| Checkbox State | Result                                                                                                             |
| -------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Checked**    | Role is for mobile app users only (e.g., Technicians). No web dashboard access. Module permissions section hidden. |
| **Unchecked**  | Role gets full web dashboard access with module permissions as configured.                                         |

This setting is defined at template level (Seravion) and inherited by clients, but clients can modify it when creating their role.
