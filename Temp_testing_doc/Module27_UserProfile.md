# 🎯 MODULE 27: USER PROFILE

## Overview

The User Profile module provides a **360-degree, read-only view of any employee or technician** within the Pest Control ERP-CRM system. It consolidates personal details, organization hierarchy, salary configuration, banking details, uploaded documents, and leave summaries into a single, section-wise profile page.

Additionally, for users with the **CEO** role, the profile includes a **Company Profile** section that displays and allows editing of the company details originally submitted during Onboarding (Module 2), along with additional branding fields such as Company Logo, Website, and Tagline.

All data displayed in this module is **sourced from existing modules** — no new data entry occurs here. The profile can be **edited only by users with appropriate RBAC permissions** through the underlying source modules (Employee Management, IAM, HRM).

**Module Connections:**

- **Depends on:** Module 1 (IAM — Account ID, password, login status), Module 2 (Company Onboarding — company details, documents), Module 7 (Branch Management — branch assignment), Module 8 (Employee Master — personal details, role, designation, documents), Module 6 (Configuration — leave types, salary structure, shift settings), Module 25 (HRM — attendance, leave, salary)
- **Used by:** All ERP users (self-view), HR Managers (employee review), Company Admins (full access), CEO (company profile management)
- **Data Nature:** Read-only profile view. Editable only through source modules with RBAC control. Company Profile section is editable only by CEO.

---

The module contains the following screens:

- 27.1 User Profile – View Mode (Default)
- 27.2 User Profile – Edit Mode (RBAC-controlled)
- 27.3 Company Profile – View & Edit (CEO Only)

---

================================================================================

# 27.1 User Profile – View Mode

**Description:**
A comprehensive, section-wise profile page displaying all relevant information about an employee or technician. The page is rendered as a single scrollable view with collapsible sections. Accessible by the employee themselves (self-view) or by authorized managers/admins.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Back]              USER PROFILE                         [✏️ Edit]       │
│                                                                              │
│  ┌─ SECTION 1: BASIC USER INFORMATION ────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Profile Photo  : ┌─────┐                                              │  │
│  │                   │ 👤  │                                              │  │
│  │                   └─────┘                                              │  │
│  │                                                                        │  │
│  │  EMP ID         : EMP-00124                                            │  │
│  │  First Name     : Ravi                                                 │  │
│  │  Last Name      : Sharma                                               │  │
│  │  Full Name      : Ravi Sharma (auto-generated)                         │  │
│  │  Email          : ravi.s@company.com                                   │  │
│  │  Contact Number : 9876543210                                           │  │
│  │  Alternate Number: 9123456789                                          │  │
│  │  Account ID     : ravi.s                                               │  │
│  │  Password       : ●●●●●●●●                                            │  │
│  │  Status         : 🟢 Active                                            │  │
│  │  Date of Joining: 15 Jun 2024                                          │  │
│  │  Employment Type: Permanent                                            │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 2: ORGANIZATION INFORMATION ──────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Department    : Operations                                            │  │
│  │  Designation   : Senior Pest Control Technician                        │  │
│  │  Role          : Senior Technician                                     │  │
│  │  Branch        : Mumbai — Andheri                                      │  │
│  │  Reporting Mgr : Anil K. (Branch Manager)                              │  │
│  │  App User      : ✅ Yes (Mobile App Access Enabled)                    │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 3: ADDRESS INFORMATION ───────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  ── Current Address ──────────────────────────────                      │  │
│  │  Address Line 1     : 42, Shanti Nagar, Andheri West                   │  │
│  │  Address Line 2     : Near City Mall                                   │  │
│  │  City               : Mumbai                                           │  │
│  │  State              : Maharashtra                                      │  │
│  │  Country            : India                                            │  │
│  │  Pincode            : 400058                                           │  │
│  │                                                                        │  │
│  │  ☑ Same as Current Address                                             │  │
│  │                                                                        │  │
│  │  ── Permanent Address ────────────────────────────                      │  │
│  │  Address Line 1     : 42, Shanti Nagar, Andheri West  (auto-filled)    │  │
│  │  Address Line 2     : Near City Mall                  (auto-filled)    │  │
│  │  City               : Mumbai                         (auto-filled)    │  │
│  │  State              : Maharashtra                    (auto-filled)    │  │
│  │  Country            : India                          (auto-filled)    │  │
│  │  Pincode            : 400058                         (auto-filled)    │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 4: SALARY INFORMATION ────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Salary Type    : CTC                                                  │  │
│  │  Basic Salary   : ₹20,000                                              │  │
│  │  HRA            : ₹5,000                                               │  │
│  │  Other Allowance: ₹3,000                                               │  │
│  │  Incentive      : ₹2,000                                               │  │
│  │  Deductions     : ₹2,500                                               │  │
│  │                                                                        │  │
│  │  PF Applicable  : ✅ Yes                                                │  │
│  │  ESI Applicable : ✅ Yes                                                │  │
│  │  TDS Applicable : ❌ No                                                 │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 5: BANK INFORMATION ──────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Bank Name          : State Bank of India                              │  │
│  │  Account Number     : ●●●●●●●●4321  (masked)                          │  │
│  │  Account Holder Name: Ravi Sharma                                      │  │
│  │  IFSC Code          : SBIN0001234                                      │  │
│  │  UPI ID             : ravi.s@sbi                                       │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 6: DOCUMENTS ─────────────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Document Type          │ File Name           │ Status    │ Actions    │  │
│  │────────────────────────┼─────────────────────┼───────────┼────────────│  │
│  │  Government ID Proof    │ aadhaar_ravi.pdf    │ ✅ Uploaded│ [📥 ][👁] │  │
│  │  Address Proof          │ utility_bill.pdf    │ ✅ Uploaded│ [📥 ][👁] │  │
│  │  Employment Contract    │ contract_ravi.pdf   │ ✅ Uploaded│ [📥 ][👁] │  │
│  │  Education Certificates │ degree_cert.pdf     │ ✅ Uploaded│ [📥 ][👁] │  │
│  │  Other Documents        │ —                   │ ❌ Pending │     —      │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 7: LEAVE SUMMARY ─────────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  CL Balance     : 8 / 12                                               │  │
│  │  SL Balance     : 5 / 6                                                │  │
│  │  PL Balance     : 10 / 15                                              │  │
│  │  Total Leaves Taken : 4                                                │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 8: COMPANY PROFILE (CEO Only) ─────────────────────────────────┐ │
│  │  ⚠️ This section is ONLY visible when the logged-in user has CEO role   │ │
│  │                                                                        │  │
│  │  Company Logo   : ┌─────┐                                              │  │
│  │                   │ 🏢  │                                              │  │
│  │                   └─────┘                                              │  │
│  │                                                                        │  │
│  │  Company Name   : Acme Pest Solutions Pvt. Ltd.                        │  │
│  │  Tagline        : "Protecting Homes Since 2010"                        │  │
│  │  Industry Type  : Pest Control                                         │  │
│  │  Website        : www.acmepest.com                                     │  │
│  │  Founding Year  : 2010                                                 │  │
│  │                                                                        │  │
│  │  ── Contact Person ──────────────────────────────                      │  │
│  │  Name           : John Doe                                             │  │
│  │  Email          : john@acmepest.com                                    │  │
│  │  Phone          : 9988776655                                           │  │
│  │                                                                        │  │
│  │  ── Legal & Tax Information ─────────────────────                      │  │
│  │  GST Number     : 27AAAAA0000A1Z5                                      │  │
│  │  PAN Number     : ABCDE1234F                                           │  │
│  │  License Number : PCO-MH-2024-1234 (Optional)                          │  │
│  │                                                                        │  │
│  │  ── Registered Address ──────────────────────────                      │  │
│  │  Address Line 1 : Tower A, IT Park                                     │  │
│  │  Address Line 2 : —                                                    │  │
│  │  City           : Noida                                                │  │
│  │  State          : Uttar Pradesh                                        │  │
│  │  Pincode        : 201301                                               │  │
│  │                                                                        │  │
│  │  ── Company Documents ───────────────────────────                      │  │
│  │  GST Certificate         │ gst_cert.pdf     │ ✅ Uploaded │ [📥][👁]  │  │
│  │  PAN Card                │ pan_card.pdf     │ ✅ Uploaded │ [📥][👁]  │  │
│  │  Business Registration   │ biz_reg.pdf      │ ✅ Uploaded │ [📥][👁]  │  │
│  │                                                                        │  │
│  │  ── Onboarding Status ───────────────────────────                      │  │
│  │  Status         : ✅ Approved                                          │  │
│  │  Company Code   : ACME-PINE-456 (read-only)                            │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Section 1: Basic User Information (Read-Only)

| Field             | Type              | Required | Validation                    | Description                                                        | Source              |
| ----------------- | ----------------- | -------- | ----------------------------- | ------------------------------------------------------------------ | ------------------- |
| Profile Photo     | Display (Image)   | No       | JPG/PNG, Max 2MB              | Employee's profile photograph                                       | Module 8            |
| EMP ID            | Display           | Auto     | System-generated, unique      | Unique employee identifier (e.g., EMP-00124)                       | Module 8            |
| First Name        | Display           | Yes      | Min 2 chars, Max 50 chars     | Employee's first name                                               | Module 8            |
| Last Name         | Display           | Yes      | Min 1 char, Max 50 chars      | Employee's last name                                                | Module 8            |
| Full Name         | Display (Auto)    | Auto     | Auto-generated                | Concatenation of First Name + Last Name                             | Calculated          |
| Email             | Display           | Yes      | Valid email format, unique     | Official email used for login and communication                     | Module 8            |
| Contact Number    | Display           | Yes      | Valid 10-digit mobile number   | Primary phone number                                                | Module 8            |
| Alternate Number  | Display           | No       | Valid 10-digit mobile number   | Backup contact number                                               | Module 8            |
| Account ID        | Display           | Auto     | Unique, from IAM               | Login credential used for application authentication               | Module 1 (IAM)      |
| Password          | Display (Masked)  | —        | ●●●●●●●● (always masked)      | Login password — never shown in plain text                          | Module 1 (IAM)      |
| Status            | Display (Badge)   | Auto     | 🟢 Active / 🔴 Inactive       | Current employment status                                           | Module 8            |
| Date of Joining   | Display           | Yes      | Cannot be future date          | Date the employee joined the company                                | Module 8            |
| Employment Type   | Display (Badge)   | Yes      | Permanent / Contract / Intern  | Type of employment engagement                                       | Module 8            |

---

## Section 2: Organization Information (Read-Only)

| Field                      | Type            | Required | Validation                         | Description                                                                                   | Source           |
| -------------------------- | --------------- | -------- | ---------------------------------- | --------------------------------------------------------------------------------------------- | ---------------- |
| Department                 | Display         | Yes      | Must select from configured list   | Business unit (Operations, Sales, Admin, etc.)                                                | Module 8         |
| Designation                | Display         | Yes      | Must select from configured list   | Job title (e.g., Senior Pest Control Technician)                                              | Module 8         |
| Role                       | Display         | Yes      | Must select from system roles      | System role controlling RBAC permissions                                                       | Module 1 (IAM)   |
| Branch                     | Display         | Yes      | Must select from active branches   | Assigned branch for operations                                                                 | Module 7 → 8     |
| Reporting Manager          | Display         | Yes      | Must be an active employee         | Direct supervisor or manager                                                                   | Module 8         |
| Is Application User        | Display (Yes/No)| —        | Boolean (Yes/No)                   | If enabled, user accesses only the mobile app; ERP module permissions are disabled             | Module 1 (IAM)   |

---

## Section 3: Address Information (Read-Only)

| Field                       | Type       | Required    | Validation              | Description                                              | Source    |
| --------------------------- | ---------- | ----------- | ----------------------- | -------------------------------------------------------- | --------- |
| Current Address Line 1      | Display    | Yes         | Min 5 chars, Max 200    | Primary address line                                     | Module 8  |
| Current Address Line 2      | Display    | No          | Max 200 chars           | Additional address information (landmark, floor, etc.)   | Module 8  |
| City                        | Display    | Yes         | Min 2 chars             | City of residence                                        | Module 8  |
| State                       | Display    | Yes         | From predefined list    | State / Province                                         | Module 8  |
| Country                     | Display    | Yes         | Default: India          | Country of residence                                     | Module 8  |
| Pincode                     | Display    | Yes         | 6-digit numeric         | Postal code                                              | Module 8  |
| Same as Current (Checkbox)  | Display    | —           | Boolean                 | If checked, auto-copies current address to permanent     | Module 8  |
| Permanent Address Line 1    | Display    | Conditional | Required if checkbox off| Permanent residential address line 1                     | Module 8  |
| Permanent Address Line 2    | Display    | No          | Max 200 chars           | Additional permanent address details                     | Module 8  |
| Permanent City              | Display    | Conditional | Required if checkbox off| Permanent address city                                   | Module 8  |
| Permanent State             | Display    | Conditional | Required if checkbox off| Permanent address state                                  | Module 8  |
| Permanent Country           | Display    | Conditional | Default: India          | Permanent address country                                | Module 8  |
| Permanent Pincode           | Display    | Conditional | 6-digit numeric         | Permanent address postal code                            | Module 8  |

---

## Section 4: Salary Information (Read-Only)

| Field            | Type              | Required | Validation              | Description                                              | Source                |
| ---------------- | ----------------- | -------- | ----------------------- | -------------------------------------------------------- | --------------------- |
| Salary Type      | Display           | Yes      | CTC / Fixed / Hourly    | Payment model for this employee                          | Module 6 → Module 25  |
| Basic Salary     | Display (Currency)| Yes      | ≥ 0                     | Base monthly salary                                      | Module 25 (Salary)    |
| HRA              | Display (Currency)| No       | ≥ 0                     | House Rent Allowance                                     | Module 25 (Salary)    |
| Other Allowance  | Display (Currency)| No       | ≥ 0                     | Additional company allowances                            | Module 25 (Salary)    |
| Incentive        | Display (Currency)| No       | ≥ 0                     | Performance-based bonus                                  | Module 25 (Salary)    |
| Deductions       | Display (Currency)| No       | ≥ 0                     | Total deductions (tax, benefits, etc.)                   | Module 25 (Salary)    |
| PF Applicable    | Display (Yes/No)  | —        | ✅ Yes / ❌ No           | Whether Provident Fund deduction applies                 | Module 6 → Module 25  |
| ESI Applicable   | Display (Yes/No)  | —        | ✅ Yes / ❌ No           | Whether Employee State Insurance applies                 | Module 6 → Module 25  |
| TDS Applicable   | Display (Yes/No)  | —        | ✅ Yes / ❌ No           | Whether Tax Deduction at Source applies                  | Module 6 → Module 25  |

---

## Section 5: Bank Information (Read-Only)

| Field              | Type             | Required | Validation               | Description                                  | Source    |
| ------------------ | ---------------- | -------- | ------------------------ | -------------------------------------------- | --------- |
| Bank Name          | Display          | Yes      | Min 3 chars              | Bank used for salary payments                | Module 8  |
| Account Number     | Display (Masked) | Yes      | Partially masked display | Employee's bank account number               | Module 8  |
| Account Holder Name| Display          | Yes      | Min 2 chars              | Name as per bank records                     | Module 8  |
| IFSC Code          | Display          | Yes      | 11-char alphanumeric     | Bank branch identification code              | Module 8  |
| UPI ID             | Display          | No       | Valid UPI format         | Digital payment identifier (optional)        | Module 8  |

---

## Section 6: Documents (Read-Only)

| Field                    | Type        | Required | Validation                   | Description                                      | Source    |
| ------------------------ | ----------- | -------- | ---------------------------- | ------------------------------------------------ | --------- |
| Government ID Proof      | Display (Link)| Yes      | PDF/JPG/PNG, Max 5MB         | Aadhaar / PAN / Voter ID or equivalent           | Module 8  |
| Address Proof            | Display (Link)| Yes      | PDF/JPG/PNG, Max 5MB         | Utility bill, rental agreement, etc.             | Module 8  |
| Employment Contract      | Display (Link)| Yes      | PDF only, Max 10MB           | Signed employment agreement                       | Module 8  |
| Education Certificates   | Display (Link)| No       | PDF/JPG/PNG, Max 5MB         | Qualification / degree certificates               | Module 8  |
| Other Documents          | Display (Link)| No       | PDF/JPG/PNG, Max 5MB each    | Any additional employee documentation             | Module 8  |

### Document Actions (View Mode)

| Action       | Icon | Description                                           |
| ------------ | ---- | ----------------------------------------------------- |
| **Download** | 📥   | Download the uploaded document to local device         |
| **View**     | 👁   | Open document in browser preview (PDF/Image viewer)   |

> **Note:** Upload and Delete actions are only available in **Edit Mode** (Screen 27.2).

---

## Section 7: Leave Summary (Read-Only)

| Field              | Type    | Description                                                        | Source              |
| ------------------ | ------- | ------------------------------------------------------------------ | ------------------- |
| CL Balance         | Display | Casual Leave: Used / Total allocation                              | Module 6 → Module 25|
| SL Balance         | Display | Sick Leave: Used / Total allocation                                | Module 6 → Module 25|
| PL Balance         | Display | Paid Leave: Used / Total allocation                                | Module 6 → Module 25|
| Total Leaves Taken | Number  | Total leave days consumed in current year                          | Module 25 (Leave)   |

---

## Section 8: Company Profile (CEO Only)

> **Visibility Rule:** This entire section is **only visible** when the logged-in user has the **CEO** role. For all other roles, this section is completely hidden.

### 8A. Company Identity

| Field              | Type              | Required | Validation                         | Description                                                        | Source              |
| ------------------ | ----------------- | -------- | ---------------------------------- | ------------------------------------------------------------------ | ------------------- |
| Company Logo       | Display (Image)   | No       | JPG/PNG, Max 2MB, Recommended 200×200px | Company logo image used across the ERP and invoices           | **New** (Module 27) |
| Company Name       | Display           | Yes      | Min 3 chars, Max 150 chars         | Registered company name                                            | Module 2            |
| Tagline            | Display           | No       | Max 200 chars                      | Company motto or short branding text                               | **New** (Module 27) |
| Industry Type      | Display (Badge)   | Yes      | Must be from predefined list       | Business sector (Grocery / Pest Control / Clothing)                | Module 2            |
| Website            | Display           | No       | Valid URL format                   | Company website URL                                                | **New** (Module 27) |
| Founding Year      | Display           | No       | 4-digit year, cannot be future     | Year the company was established                                   | **New** (Module 27) |

### 8B. Contact Person

| Field                | Type     | Required | Validation                    | Description                                                | Source    |
| -------------------- | -------- | -------- | ----------------------------- | ---------------------------------------------------------- | --------- |
| Contact Person Name  | Display  | Yes      | Min 2 chars, Max 120 chars    | Primary contact person for the company                     | Module 2  |
| Contact Person Email | Display  | Yes      | Valid email format            | Contact person's email address                             | Module 2  |
| Contact Person Phone | Display  | Yes      | Exactly 10 digits, numeric   | Contact person's phone number                              | Module 2  |

### 8C. Legal & Tax Information

| Field          | Type     | Required | Validation                              | Description                                    | Source    |
| -------------- | -------- | -------- | --------------------------------------- | ---------------------------------------------- | --------- |
| GST Number     | Display  | Yes      | 15-char valid GSTIN format              | Goods & Services Tax Identification Number     | Module 2  |
| PAN Number     | Display  | Yes      | 10-char alphanumeric (AAAAA9999A)       | Permanent Account Number                       | Module 2  |
| License Number | Display  | No       | Alphanumeric if provided                | Business or pest control license number        | Module 2  |

### 8D. Registered Address

| Field            | Type     | Required | Validation             | Description                                    | Source    |
| ---------------- | -------- | -------- | ---------------------- | ---------------------------------------------- | --------- |
| Address Line 1   | Display  | Yes      | Min 5 chars, Max 200   | Primary company address line                   | Module 2  |
| Address Line 2   | Display  | No       | Max 200 chars          | Additional address info (floor, landmark, etc.)| Module 2  |
| City             | Display  | Yes      | Min 2 chars, alphabets | City of registered office                      | Module 2  |
| State            | Display  | Yes      | Min 2 chars, alphabets | State of registered office                     | Module 2  |
| Pincode          | Display  | Yes      | 6-digit numeric        | Postal code of registered office               | Module 2  |

### 8E. Company Documents

| Field                          | Type          | Required | Validation                 | Description                                   | Source    |
| ------------------------------ | ------------- | -------- | -------------------------- | --------------------------------------------- | --------- |
| GST Certificate                | Display (Link)| Yes      | PDF/JPG/PNG, Max 5MB       | Uploaded GST certificate document             | Module 2  |
| PAN Card                       | Display (Link)| Yes      | PDF/JPG/PNG, Max 5MB       | Uploaded PAN card document                    | Module 2  |
| Business / Registration Doc    | Display (Link)| Yes      | PDF/JPG/PNG, Max 10MB      | Trade license or incorporation document       | Module 2  |

### 8F. Onboarding Status (Read-Only)

| Field              | Type            | Required | Description                                              | Source    |
| ------------------ | --------------- | -------- | -------------------------------------------------------- | --------- |
| Onboarding Status  | Display (Badge) | Auto     | 🟢 Approved / 🟡 Pending / 🔴 Rejected                  | Module 2  |
| Company Code       | Display         | Auto     | System-generated unique company identifier (read-only)   | Module 2  |

---

## Page-Level Actions

| Action   | Condition                              | Behaviour                                                     |
| -------- | -------------------------------------- | ------------------------------------------------------------- |
| **Back** | Always visible                         | Returns to previous page (Employee List or Dashboard)         |
| **Edit** | Visible only if user has Edit RBAC     | Opens Edit Mode (Screen 27.2) — toggles fields to editable   |

---

================================================================================

# 27.2 User Profile – Edit Mode

**Description:**
Same layout as View Mode, but applicable fields become editable. Accessible by the user themselves (self-edit for limited fields) or by authorized managers/admins (full edit). Certain fields remain read-only even in Edit Mode (EMP ID, Account ID, Status).

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [← Cancel]            EDIT PROFILE                         [💾 Save]       │
│                                                                              │
│  ┌─ SECTION 1: BASIC USER INFORMATION ────────────────────────────────────┐  │
│  │  ┌─────┐                                                               │  │
│  │  │ 👤  │  EMP ID : EMP-00124 (read-only)                              │  │
│  │  │Photo│                                                               │  │
│  │  │     │  First Name  : [Ravi_______________]                          │  │
│  │  └─────┘  Last Name   : [Sharma______________]                         │  │
│  │           Full Name   : Ravi Sharma (auto)                             │  │
│  │                                                                        │  │
│  │  Email          : [ravi.s@company.com_____]                            │  │
│  │  Contact Number : [9876543210_____________]                            │  │
│  │  Alt Number     : [9123456789_____________]                            │  │
│  │  Account ID     : ravi.s (read-only)                                   │  │
│  │  Password       : ●●●●●●●● (read-only)                                │  │
│  │  Status         : 🟢 Active (read-only)                                │  │
│  │  Date of Joining: [📅 15 Jun 2024________]                             │  │
│  │  Employment Type: [▼ Permanent ▼_________]                             │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 2: ORGANIZATION INFORMATION ──────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Department    : [▼ Operations ▼_________]                             │  │
│  │  Designation   : [▼ Senior Pest Control Technician ▼]                  │  │
│  │  Role          : [▼ Senior Technician ▼__]                             │  │
│  │  Branch        : [▼ Mumbai — Andheri ▼___]                             │  │
│  │  Reporting Mgr : [🔍 Anil K.____________]                              │  │
│  │  App User      : [☑] Yes                                               │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 3: ADDRESS INFORMATION ───────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  ── Current Address ──────────────────────────────                      │  │
│  │  Address Line 1 : [42, Shanti Nagar, Andheri West__]                   │  │
│  │  Address Line 2 : [_______________________________]                    │  │
│  │  City           : [Mumbai_____________________]                        │  │
│  │  State          : [▼ Maharashtra ▼____________]                        │  │
│  │  Country        : [▼ India ▼__________________]                        │  │
│  │  Pincode        : [400058_____________________]                        │  │
│  │                                                                        │  │
│  │  [☑] Same as Current Address                                           │  │
│  │                                                                        │  │
│  │  ── Permanent Address ────────────────────────────  (disabled)          │  │
│  │  (Auto-filled from Current Address)                                    │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 4: SALARY INFORMATION ────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Salary Type    : [▼ CTC ▼________________]                            │  │
│  │  Basic Salary   : [₹ 20,000_______________]                            │  │
│  │  HRA            : [₹ 5,000________________]                            │  │
│  │  Other Allowance: [₹ 3,000________________]                            │  │
│  │  Incentive      : [₹ 2,000________________]                            │  │
│  │  Deductions     : [₹ 2,500________________]                            │  │
│  │                                                                        │  │
│  │  PF Applicable  : [☑] Yes                                              │  │
│  │  ESI Applicable : [☑] Yes                                              │  │
│  │  TDS Applicable : [☐] No                                               │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 5: BANK INFORMATION ──────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Bank Name      : [State Bank of India____]                            │  │
│  │  Account Number : [123456784321___________]                            │  │
│  │  Account Holder : [Ravi Sharma____________]                            │  │
│  │  IFSC Code      : [SBIN0001234____________]                            │  │
│  │  UPI ID         : [ravi.s@sbi_____________]                            │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 6: DOCUMENTS ─────────────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Document Type          │ File Name           │ Status    │ Actions    │  │
│  │────────────────────────┼─────────────────────┼───────────┼────────────│  │
│  │  Government ID Proof    │ aadhaar_ravi.pdf    │ ✅ Uploaded│[📥][👁][🗑]│  │
│  │  Address Proof          │ utility_bill.pdf    │ ✅ Uploaded│[📥][👁][🗑]│  │
│  │  Employment Contract    │ contract_ravi.pdf   │ ✅ Uploaded│[📥][👁][🗑]│  │
│  │  Education Certificates │ degree_cert.pdf     │ ✅ Uploaded│[📥][👁][🗑]│  │
│  │  Other Documents        │ —                   │ ❌ Pending │ [📤 Upload]│  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 7: LEAVE SUMMARY ─────────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │  Leave Balance  : CL: 8/12  |  SL: 5/6  |  PL: 10/15  (read-only)    │  │
│  │  Leaves Taken   : 4                                     (read-only)    │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─ SECTION 8: COMPANY PROFILE (CEO Only) ─────────────────────────────────┐ │
│  │  ⚠️ Visible only to CEO role. Editable only by CEO.                     │ │
│  │                                                                        │  │
│  │  Company Logo   : ┌─────┐  [📤 Upload Logo]                            │  │
│  │                   │ 🏢  │                                              │  │
│  │                   └─────┘                                              │  │
│  │                                                                        │  │
│  │  Company Name   : [Acme Pest Solutions Pvt. Ltd.__]                    │  │
│  │  Tagline        : [Protecting Homes Since 2010____]                    │  │
│  │  Industry Type  : [▼ Pest Control ▼_______________]                    │  │
│  │  Website        : [www.acmepest.com_______________]                    │  │
│  │  Founding Year  : [2010___________________________]                    │  │
│  │                                                                        │  │
│  │  ── Contact Person ──────────────────────────────                      │  │
│  │  Name           : [John Doe_______________________]                    │  │
│  │  Email          : [john@acmepest.com______________]                    │  │
│  │  Phone          : [9988776655_____________________]                    │  │
│  │                                                                        │  │
│  │  ── Legal & Tax Information ─────────────────────                      │  │
│  │  GST Number     : [27AAAAA0000A1Z5________________]                    │  │
│  │  PAN Number     : [ABCDE1234F_____________________]                    │  │
│  │  License Number : [PCO-MH-2024-1234_______________]                    │  │
│  │                                                                        │  │
│  │  ── Registered Address ──────────────────────────                      │  │
│  │  Address Line 1 : [Tower A, IT Park_______________]                    │  │
│  │  Address Line 2 : [______________________________]                     │  │
│  │  City           : [Noida_________________________]                     │  │
│  │  State          : [Uttar Pradesh_________________]                     │  │
│  │  Pincode        : [201301________________________]                     │  │
│  │                                                                        │  │
│  │  ── Company Documents ────────────────────────── (read-only)           │  │
│  │  GST Certificate       │ gst_cert.pdf   │ ✅ Uploaded │  [📥][👁]    │  │
│  │  PAN Card              │ pan_card.pdf   │ ✅ Uploaded │  [📥][👁]    │  │
│  │  Business Registration │ biz_reg.pdf    │ ✅ Uploaded │  [📥][👁]    │  │
│  │                                                                        │  │
│  │  ── Onboarding Status ──────────────────────────── (read-only)         │  │
│  │  Status         : ✅ Approved                                          │  │
│  │  Company Code   : ACME-PINE-456                                        │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌────────────────────────────────────────────────────────────────────────┐  │
│  │                     [💾 Save]        [✖ Cancel]                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Editable vs Read-Only Rules (Admin / HR Manager Edit)

| Section                  | Editable Fields                                                          | Always Read-Only Fields                                   |
| ------------------------ | ------------------------------------------------------------------------ | --------------------------------------------------------- |
| 1. Basic Info            | First Name, Last Name, Email, Contact, Alt Number, Employment Type       | EMP ID, Full Name (auto), Account ID, Password, Status ,Date of joining    |
| 2. Organization Info     | Department, Designation, Role, Branch, Reporting Mgr, App User           | —                                                         |
| 3. Address               | All address fields                                                       | —                                                         |
| 4. Salary                | All salary fields                                                        | —                                                         |
| 5. Bank Info             | All bank fields                                                          | —                                                         |
| 6. Documents             | Upload / Delete documents                                                | —                                                         |
| 7. Leave Summary         | —                                                                        | All fields (managed via Module 25 HRM)                    |
| 8. Company Profile       | Company Logo, Name, Tagline, Industry Type, Website, Founding Year, Contact Person (Name/Email/Phone), GST Number, PAN Number, License Number, Address fields | Onboarding Status, Company Code, Company Documents (always read-only once uploaded) |

---

## Self-Edit Rules (Employee editing their own profile)

> When a user opens their own profile and taps **Edit**, only the following fields are editable. All other fields remain read-only.

| Section                  | Self-Editable Fields                                                          |
| ------------------------ | ----------------------------------------------------------------------------- |
| 1. Basic Info            | Contact Number, Alternate Number                                              |
| 2. Organization Info     | — (Read-Only)                                                                 |
| 3. Address               | All current & permanent address fields                                        |
| 4. Salary                | — (Read-Only, view own salary only)                                           |
| 5. Bank Info             | Bank Name, Account Number, Account Holder Name, IFSC Code, UPI ID            |
| 6. Documents             | Upload own documents (Gov ID, Address Proof, Education Certs, Other Docs)    |
| 7. Leave Summary         | — (Read-Only)                                                                 |
| 8. Company Profile       | All fields (CEO role only — see Section 8 details)                           |

> **Note:** Employment Contract can only be uploaded by HR Manager or Admin — not by the employee.
> **Note:** Section 8 (Company Profile) is only visible and editable for users with the CEO role.

### Document Actions (Edit Mode)

| Action       | Icon | Condition                              | Description                                           |
| ------------ | ---- | -------------------------------------- | ----------------------------------------------------- |
| **Upload**   | 📤   | Edit Mode only                         | Upload a new document (opens file picker)             |
| **Download** | 📥   | Always (also in View Mode)             | Download the uploaded document to local device         |
| **View**     | 👁   | Always (also in View Mode)             | Open document in browser preview (PDF/Image viewer)   |
| **Delete**   | 🗑   | Edit Mode only (Admin/HR only)         | Remove uploaded document (confirmation required)       |

---

## Edit Mode Actions

| Action     | Trigger    | System Behaviour                                                                                      |
| ---------- | ---------- | ----------------------------------------------------------------------------------------------------- |
| **Save**   | Tap button | Validates all modified fields → Saves changes to respective source modules → Logs changes in audit trail → Shows success toast: "Profile updated successfully." → Returns to View Mode |
| **Cancel** | Tap button | Discards all unsaved changes → Returns to View Mode. Confirmation dialog: "Discard unsaved changes?" |

---

## Validation Rules (Edit Mode)

| Field            | Validation                                                             |
| ---------------- | ---------------------------------------------------------------------- |
| Email            | Must be valid email format. Must be unique across all employees.       |
| Contact Number   | Must be valid 10-digit Indian mobile number.                           |
| Pincode          | Must be 6-digit numeric.                                               |
| IFSC Code        | Must be 11-character alphanumeric (standard bank format).              |
| UPI ID           | Must follow `name@bank` format if provided.                            |
| Basic Salary     | Must be ≥ 0. Cannot be blank.                                          |
| Documents        | File size limits enforced. Only PDF/JPG/PNG accepted.                  |
| Reporting Manager| Must be an active employee. Cannot be self.                            |

### Section 8 — Company Profile Validation (CEO Only)

| Field                     | Validation                                                                  |
| ------------------------- | --------------------------------------------------------------------------- |
| Company Logo              | JPG/PNG only. Max 2MB. Recommended 200×200px. Optional.                     |
| Company Name              | Min 3, Max 150 chars. Alphanumeric and spaces allowed.                      |
| Tagline                   | Max 200 chars. Optional.                                                    |
| Industry Type             | Must select from predefined dropdown (Grocery / Pest Control / Clothing).   |
| Website                   | Must be a valid URL format if provided. Optional.                           |
| Founding Year             | Must be a 4-digit year. Cannot be in the future. Optional.                  |
| Contact Person Name       | Min 2, Max 120 chars. No numbers or special characters.                     |
| Contact Person Email      | Must be valid email format.                                                 |
| Contact Person Phone      | Exactly 10 digits, numeric only.                                            |
| GST Number                | 15-character valid GSTIN format (2 digits + PAN + 3 chars).                 |
| PAN Number                | 10 characters, format: AAAAA9999A.                                          |
| License Number            | Alphanumeric if provided. Optional.                                         |
| Company Address Line 1    | Min 5 chars, Max 200 chars.                                                 |
| Company City              | Min 2 chars, alphabets only.                                                |
| Company State             | Min 2 chars, alphabets only.                                                |
| Company Pincode           | 6-digit numeric.                                                            |

> **Note:** Company Documents (GST Certificate, PAN Card, Business Registration) are read-only in the User Profile and are not editable or re-uploadable. They can only be managed through Module 2 (Onboarding).

---

================================================================================

# Access Control (RBAC)

| Role                   | View Own Profile | View Others' Profile | Edit Profile | Upload Documents | View Salary | Company Profile (Sec 8) |
| ---------------------- | ---------------- | -------------------- | ------------ | ---------------- | ----------- | ----------------------- |
| **CEO**                | ✅               | ✅ (All employees)   | ✅ (All)     | ✅                | ✅           | ✅ View & Edit           |
| **Company Admin**      | ✅               | ✅ (All employees)   | ✅ (All)     | ✅                | ✅           | ❌ Hidden                |
| **HR Manager**         | ✅               | ✅ (All employees)   | ✅ (All)     | ✅                | ✅           | ❌ Hidden                |
| **Branch Manager**     | ✅               | ✅ (Own branch only) | ✅ (Branch)  | ✅ (Branch)       | ❌           | ❌ Hidden                |
| **Technician Manager** | ✅               | ✅ (Own branch only) | ❌           | ❌                | ❌           | ❌ Hidden                |
| **Operations Manager** | ✅               | ✅ (All employees)   | ❌           | ❌                | ❌           | ❌ Hidden                |
| **Technician**         | ✅ (Self only)   | ❌                   | ❌           | ✅ (Own docs)     | ✅ (Own)     | ❌ Hidden                |
| **Senior Technician**  | ✅ (Self only)   | ❌                   | ❌           | ✅ (Own docs)     | ✅ (Own)     | ❌ Hidden                |

---

================================================================================

# Business Rules

| Rule                        | Description                                                                                  |
| --------------------------- | -------------------------------------------------------------------------------------------- |
| Self-View Default           | When a user opens Profile without specifying an employee, their own profile is displayed.     |
| Password Never Exposed      | Password is always displayed as masked (●●●●●●●●). Cannot be viewed or copied from Profile. |
| Bank Account Masking         | Account Number is partially masked (e.g., ●●●●●●●●4321) in View Mode. Full number visible only in Edit Mode for authorized users. |
| App User Logic              | If "Is Application User" = Yes, the user only accesses the mobile app; ERP module-level permissions are managed via IAM. |
| Address Auto-Copy           | If "Same as Current Address" checkbox is enabled, permanent address fields are auto-populated and disabled. |
| Document Requirements       | Government ID Proof, Address Proof, and Employment Contract are mandatory for profile completion. Missing documents show ❌ Pending status. |
| Status Change Restriction    | Employee Status (Active/Inactive) can only be changed by HR Manager or Company Admin. Deactivating an employee also deactivates their IAM account. |
| Salary Visibility           | Salary section is hidden for users without salary view permission. Technicians and Senior Technicians can only see their own salary. |
| Role Change Cascade         | Changing an employee's Role automatically updates their Module Permissions in Module 1 (IAM). |
| Profile Completeness        | A profile completion indicator (e.g., "85% Complete") can be shown based on how many required fields and documents are filled. |
| Company Profile Visibility  | Section 8 (Company Profile) is **only rendered** when the logged-in user's role = CEO. For all other roles, the section DOM element is not rendered at all. |
| Company Profile Source      | Fields in Section 8 are populated from the Module 2 (Onboarding) company details API (`GET /api/v1/company-details`). New fields (Logo, Tagline, Website, Founding Year) are stored as extensions to the company profile. |
| Company Logo Usage          | The uploaded Company Logo is used on invoices (Module 28), quotations, and other customer-facing documents generated by the ERP. |
| Company Doc Read-Only     | Once company documents (GST Certificate, PAN Card, Business Registration) are uploaded during onboarding (Module 2), they become **permanently read-only** in the User Profile. The CEO can only Download (📥) and View (👁) them — no re-upload or delete is allowed. Any document changes must go through Module 2 onboarding flow. |
| Onboarding Status Read-Only | The Onboarding Status and Company Code fields are always read-only and cannot be modified by any user, including the CEO. They reflect the current verification state from Module 2 → Module 3.

---

> **Note:** This module acts as a consolidated view and editing interface for employee data that is primarily stored in Module 1 (IAM), Module 8 (Employee Master), Module 25 (HRM), and Module 6 (Configuration). Changes made through this profile are written back to the respective source modules.


