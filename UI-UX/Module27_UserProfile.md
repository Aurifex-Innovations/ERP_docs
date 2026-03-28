# 🎯 MODULE 27: USER PROFILE

## Overview

The User Profile module provides a **360-degree, read-only view of any employee or technician** within the Pest Control ERP-CRM system. It consolidates personal details, organization hierarchy, salary configuration, banking details, uploaded documents, and leave summaries into a single, section-wise profile page.

All data displayed in this module is **sourced from existing modules** — no new data entry occurs here. The profile can be **edited only by users with appropriate RBAC permissions** through the underlying source modules (Employee Management, IAM, HRM).

**Module Connections:**

- **Depends on:** Module 1 (IAM — Account ID, password, login status), Module 7 (Branch Management — branch assignment), Module 8 (Employee Master — personal details, role, designation, documents), Module 6 (Configuration — leave types, salary structure, shift settings), Module 25 (HRM — attendance, leave, salary)
- **Used by:** All ERP users (self-view), HR Managers (employee review), Company Admins (full access)
- **Data Nature:** Read-only profile view. Editable only through source modules with RBAC control.

---

The module contains the following screens:

- 27.1 User Profile – View Mode (Default)
- 27.2 User Profile – Edit Mode (RBAC-controlled)

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

> **Note:** Employment Contract can only be uploaded by HR Manager or Admin — not by the employee.

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

---

================================================================================

# Access Control (RBAC)

| Role                   | View Own Profile | View Others' Profile | Edit Profile | Upload Documents | View Salary |
| ---------------------- | ---------------- | -------------------- | ------------ | ---------------- | ----------- |
| **Company Admin**      | ✅               | ✅ (All employees)   | ✅ (All)     | ✅                | ✅           |
| **HR Manager**         | ✅               | ✅ (All employees)   | ✅ (All)     | ✅                | ✅           |
| **Branch Manager**     | ✅               | ✅ (Own branch only) | ✅ (Branch)  | ✅ (Branch)       | ❌           |
| **Technician Manager** | ✅               | ✅ (Own branch only) | ❌           | ❌                | ❌           |
| **Operations Manager** | ✅               | ✅ (All employees)   | ❌           | ❌                | ❌           |
| **Technician**         | ✅ (Self only)   | ❌                   | ❌           | ✅ (Own docs)     | ✅ (Own)     |
| **Senior Technician**  | ✅ (Self only)   | ❌                   | ❌           | ✅ (Own docs)     | ✅ (Own)     |

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

---

> **Note:** This module acts as a consolidated view and editing interface for employee data that is primarily stored in Module 1 (IAM), Module 8 (Employee Master), Module 25 (HRM), and Module 6 (Configuration). Changes made through this profile are written back to the respective source modules.


