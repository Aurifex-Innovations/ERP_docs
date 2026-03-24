# 📘 ERP System - Module Wise Workflow Documentation

---

# 🎯 MODULE 1: AUTHENTICATION

## Overview

Authentication module handles access control for three distinct user types:

- **Super Admin (Seravion)**: Platform owner access with secure internal login
- **Company Admin (Client)**: Tenant/company access with signup and login capabilities
- **Client Users (IAM Users)**: Branch staff, Operations Head, Technician Manager, etc., created internally by Company Admin

---

## 1.1 Super Admin (Seravion) Authentication Flow

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

### Validation Rules

| Field    | Rule                                     |
| -------- | ---------------------------------------- |
| Username | Must exist in Seravion internal database |
| Password | Must match stored hash, case-sensitive   |

### Business Rules:

- **No self-registration**: Super Admin accounts created internally by Seravion only
- **No password recovery**: Managed internally by Seravion IT team
- **Secure access**: Dedicated login portal, separate from client login

---

## 1.2 Company Admin (Client) Sign Up Flow

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

### Validation Rules

| Field                  | Rule                                                 |
| ---------------------- | ---------------------------------------------------- |
| Company Name           | Minimum 3 characters, alphanumeric only              |
| Authorized Person Name | Cannot be empty, no special characters               |
| Phone                  | Exactly 10 digits, numeric only, Indian format       |
| Email                  | Valid email format, must be unique across platform   |
| Password               | Minimum 8 characters, at least 1 uppercase, 1 number |
| Re-enter Password      | Must exactly match Password field                    |

---

## 1.3 Login Option Screen

```
┌─────────────────────────────────────────────────────────────┐
│               LOGIN OPTION SCREEN                            │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                                                     │    │
│  │         [  ROOT LOGIN  ]                            │    │
│  │                                                     │    │
│  │         [  USER LOGIN  ]                            │    │
│  │                                                     │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ❌ NO Google Login Button                                   │
│  ✅ UI Aligned with other screens                            │
└─────────────────────────────────────────────────────────────┘
```

**Navigation:**

- **Root Login** → Leads to Company Admin (Client) Login (Section 1.5)
- **User Login** → Leads to IAM User Login (Section 1.6)

---

## 1.4 Company Admin (Client) Login Flow

```
┌─────────────────────────────────────────────────────────────┐
│                COMPANY ADMIN LOGIN SCREEN                    │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Email                           [____________]     │    │
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
│  • Verify email exists                                       │
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
    │                 │ │ Login"   │ │ (Module 2.4)    │
    │ (From Module 2) │ │          │ │                 │
    └────────┬────────┘ └──────────┘ └─────────────────┘
             │
    ┌────────┴────────┐
    ▼                 ▼
┌──────────┐    ┌──────────┐
│ APPROVED │    │ REJECTED │
│          │    │          │
│ Route to │    │ Route to │
│ Module 2 │    │ Module 2 │
│ (Company │    │ (Reject  │
│ Info) or │    │ Screen)  │
│ Module 4 │    │          │
│ (If info │    │          │
│ complete)│    │          │
└──────────┘    └──────────┘
```

### Screen Fields: Company Admin Login

| Field    | Type     | Required | Notes                    |
| -------- | -------- | -------- | ------------------------ |
| Email    | Email    | Yes      | Registered email address |
| Password | Password | Yes      | Case-sensitive           |
| Submit   | Button   | -        | Triggers login           |

### Validation Rules

| Field    | Rule                                     |
| -------- | ---------------------------------------- |
| Email    | Valid email format, must exist in system |
| Password | Must match stored hash for the email     |

---

## 1.5 IAM User (Company User) Login Flow

```
┌─────────────────────────────────────────────────────────────┐
│                IAM USER LOGIN SCREEN                         │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
|  |  Account ID                      [_____________]    |    |
│  │  Email / Username                [____________]     │    │
│  │  Password                        [____________]     │    │
│  │                                                     │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │           SUBMIT BUTTON                     │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ❌ NO Sign Up redirect line                                 │
│  ✅ Forgot Password available                                │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│              AUTHENTICATION CHECK                            │
│  • Verify username/email exists                              │
│  • Validate password hash                                    │
│  • Check user status and role permissions                    │
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
    │  ROUTE TO           │           │  SHOW ERROR MESSAGE │
    │  ROLE-BASED         │           │  "Invalid Credentials"│
    │  DASHBOARD          │           │  Return to Login    │
    │  (Module 8)         │           │                     │
    └─────────────────────┘           └─────────────────────┘
```

### Screen Fields: IAM User Login

| Field            | Type     | Required | Notes                        |
| ---------------- | -------- | -------- | ---------------------------- |
| Account ID       | Text     | Yes      | Unique Auto generated Tenent |
| Email / Username | Text     | Yes      | Registered email or username |
| Password         | Password | Yes      | Case-sensitive               |
| Submit           | Button   | -        | Triggers login               |

### Validation Rules

| Field            | Rule                                   |
| ---------------- | -------------------------------------- |
| Email / Username | Must exist in system, case-insensitive |
| Password         | Must match stored hash                 |

---

# 1.6 Forgot Password (Root User + IAM User)

This feature allows **Root Users and IAM Users** to securely reset their password using **OTP verification via Email or Mobile Number**.

The password reset process follows a **4-step verification flow** to ensure account security.

---

## Forgot Password Flow

```
User clicks "Forgot Password"
        │
        ▼
STEP 1: Select Recovery Method
        │
        ├── Email
        └── Mobile Number
        │
        ▼
STEP 2: Enter Email / Mobile Number
        │
        ▼
System Sends OTP
        │
        ▼
STEP 3: OTP Verification
        │
        ▼
STEP 4: Set New Password
        │
        ▼
SUCCESS: Password Reset Complete
        │
        ▼
Redirect to Login Screen
```

---

# Screen Flow

## Step 1: Select Password Recovery Method

User selects how they want to receive the **OTP verification code**.

| Field Name      | Type   | Required | Description                           |
| --------------- | ------ | -------- | ------------------------------------- |
| Recovery Method | Radio  | Yes      | Select **Email** or **Mobile Number** |
| Continue        | Button | -        | Moves to next step based on selection |

---

## Step 2: Enter Email or Mobile Number

Based on the selected recovery method, the system displays the appropriate input field.

### If Email is Selected

| Field Name | Type   | Required | Description              |
| ---------- | ------ | -------- | ------------------------ |
| Email      | Email  | Yes      | Registered email address |
| Send OTP   | Button | -        | Sends OTP to email       |

### If Mobile Number is Selected

| Field Name    | Type   | Required | Description              |
| ------------- | ------ | -------- | ------------------------ |
| Mobile Number | Phone  | Yes      | Registered mobile number |
| Send OTP      | Button | -        | Sends OTP via SMS        |

---

## Step 3: OTP Verification

User enters the **One-Time Password (OTP)** received via Email or SMS.

| Field Name | Type   | Required | Description               |
| ---------- | ------ | -------- | ------------------------- |
| OTP        | Text   | Yes      | 6-digit verification code |
| Verify OTP | Button | -        | Validates OTP             |

---

## Step 4: Set New Password

User sets a new password after successful OTP verification.

| Field Name       | Type     | Required | Description              |
| ---------------- | -------- | -------- | ------------------------ |
| New Password     | Password | Yes      | Enter new password       |
| Confirm Password | Password | Yes      | Re-enter new password    |
| Reset Password   | Button   | -        | Updates account password |

---

# Validation Rules

## Step 2: Email / Mobile

| Field         | Rule                                                 |
| ------------- | ---------------------------------------------------- |
| Email         | Must be a valid email format and exist in the system |
| Mobile Number | Must be 10 digits and registered with the account    |

---

## Step 3: OTP Verification

| Field       | Rule                            |
| ----------- | ------------------------------- |
| OTP         | Must match system generated OTP |
| OTP Expiry  | OTP expires in **10 minutes**   |
| Retry Limit | Maximum **3 attempts** allowed  |

---

## Step 4: Password Rules

| Field            | Rule                                |
| ---------------- | ----------------------------------- |
| New Password     | Minimum **8 characters**            |
|                  | At least **1 uppercase letter**     |
|                  | At least **1 number**               |
|                  | At least **1 special character**    |
| Confirm Password | Must exactly match **New Password** |

---

# System Behavior

| Event                  | System Action                                   |
| ---------------------- | ----------------------------------------------- |
| OTP Sent               | System generates OTP and sends via Email or SMS |
| OTP Verified           | User is allowed to reset password               |
| OTP Failed             | Error message shown with retry option           |
| Password Reset Success | Redirect to Login Screen                        |
| OTP Expired            | User must request a new OTP                     |

---

# Success Message

```
Password reset successfully.
Please login with your new password.
```

Redirects user to **Login Screen**.

---

# 🎯 MODULE 2: USER ONBOARDING

## Overview

Post-authentication workflow for Company Admin to complete company profile and document verification before system access. This module bridges authentication and full system access, ensuring compliance through document verification by Seravion.

**Module Connections:**

- Receives from: Module 1 (Authentication)
- Sends to: Module 3 (Super Admin for approval) → Module 4 (Subscription)

---

## Complete Onboarding Flow

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
| Address Line 1       | Text     | Yes      | Street address              |
| Address Line 2       | Text     | No       | Additional address info     |
| City                 | Text     | Yes      | City name                   |
| State                | Text     | Yes      | State name                  |
| Pincode              | Text     | Yes      | 6-digit postal code         |
| License Number       | Text     | **NO**   | Optional field              |

### Validation Rules

| Field                | Rule                                                          |
| -------------------- | ------------------------------------------------------------- |
| Company Name         | Minimum 3 characters, alphanumeric and spaces allowed         |
| Industry Type        | Must select from predefined dropdown options                  |
| Contact Person Name  | Cannot be empty, no numbers or special characters             |
| Contact Person Email | Valid email format                                            |
| Contact Person Phone | Exactly 10 digits, numeric only                               |
| GST Number           | 15 characters, valid GSTIN format (2 digits + PAN + 3 digits) |
| PAN Number           | 10 characters, format: AAAAA9999A                             |
| Address Line 1       | Minimum 5 characters                                          |
| City                 | Minimum 2 characters, alphabets only                          |
| State                | Minimum 2 characters, alphabets only                          |
| Pincode              | Exactly 6 digits, numeric only                                |
| License Number       | Optional, alphanumeric if provided                            |

### Screen 2.2: Upload Document Fields

| Field                          | Type        | Required | Format                   |
| ------------------------------ | ----------- | -------- | ------------------------ |
| GST Document                   | File Upload | Yes      | PDF, JPG, PNG (Max 5MB)  |
| PAN Document                   | File Upload | Yes      | PDF, JPG, PNG (Max 5MB)  |
| Business/Registration Document | File Upload | Yes      | PDF, JPG, PNG (Max 10MB) |

### Validation Rules

| Field                          | Rule                                       |
| ------------------------------ | ------------------------------------------ |
| GST Document                   | Required, max 5MB, allowed: PDF, JPG, PNG  |
| PAN Document                   | Required, max 5MB, allowed: PDF, JPG, PNG  |
| Business/Registration Document | Required, max 10MB, allowed: PDF, JPG, PNG |

---

## 2.3 Document Uploaded Successfully Popup

```
┌─────────────────────────────────────────────────────────────┐
│              SUCCESS POPUP                                   │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                                                     │    │
│  │              ✅ UPLOAD SUCCESSFUL                  │    │
│  │                                                     │    │
│  │     Your documents have been uploaded to the       │    │
│  │     system successfully.                           │    │
│  │                                                     │    │
│  │     [OK]                                            │    │
│  │                                                     │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

## 2.4 Document Verification Pending (Client Facing)

```
┌─────────────────────────────────────────────────────────────┐
│              WAITING SCREEN                                  │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                                                     │    │
│  │              ⏳ VERIFICATION PENDING               │    │
│  │                                                     │    │
│  │     "We will approve your access shortly."         │    │
│  │                                                     │    │
│  │     Your documents are under review by Seravion.   │    │
│  │     You will be notified once approved.            │    │
│  │                                                     │    │
│  │     ⚠️ Dashboard access is restricted until       │    │
│  │        approval.                                    │    │
│  │                                                     │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

## 2.5 Document Rejected Screen (Client Facing)

```
┌─────────────────────────────────────────────────────────────┐
│              DOCUMENT REJECTED SCREEN                        │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                                                     │    │
│  │              ❌ DOCUMENTS REJECTED                 │    │
│  │                                                     │    │
│  │  Rejection Reason:                                  │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │ [Detailed reason provided by Seravion]      │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │                                                     │    │
│  │  Please correct the issues and re-upload:          │    │
│  │                                                     │    │
│  │  GST Document                    [📎 Re-upload ]    │    │
│  │  PAN Document                    [📎 Re-upload ]    │    │
│  │  Business / Registration Doc     [📎 Re-upload ]    │    │
│  │                                                     │    │
│  │  [RESUBMIT FOR APPROVAL]                            │    │
│  │                                                     │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Validation Rules

| Field                          | Rule                                |
| ------------------------------ | ----------------------------------- |
| GST Document                   | Required for resubmission, max 5MB  |
| PAN Document                   | Required for resubmission, max 5MB  |
| Business/Registration Document | Required for resubmission, max 10MB |

---

# 2.6 Document Reupload (After Rejection)

This feature allows the **User to reupload a document when the previously submitted document is rejected** during the verification process.

When a document is rejected in **2.5 Document Verification**, the system provides a **Reupload option**.
Clicking this option opens a **Reupload Document Popup**, allowing the user to submit a corrected file.

---

# Reupload Flow

```text
Document Rejected (2.5 Verification)
        │
        ▼
User clicks "Reupload Document"
        │
        ▼
Reupload Popup Opens
        │
        ▼
User uploads corrected document
        │
        ▼
User clicks Submit
        │
        ▼
Document resubmitted for verification
```

---

# Reupload Document Popup

```
┌─────────────────────────────────────────────┐
│            REUPLOAD DOCUMENT                │
│                                             │
│  Document Type : GST Certificate           │
│                                             │
│  Upload New File                            │
│  [ Choose File ]                            │
│                                             │
│  Supported Formats: PDF, JPG, PNG           │
│  Maximum Size: 5MB                          │
│                                             │
│  [ Cancel ]        [ Submit ]                │
└─────────────────────────────────────────────┘
```

---

# Screen Fields

| Field         | Type        | Required | Description                        |
| ------------- | ----------- | -------- | ---------------------------------- |
| Document Type | Label       | -        | Shows rejected document name       |
| Upload File   | File Upload | Yes      | Upload corrected document          |
| Cancel        | Button      | -        | Close popup                        |
| Submit        | Button      | -        | Resubmit document for verification |

---

# Validation Rules

| Field         | Rule                                      |
| ------------- | ----------------------------------------- |
| Upload File   | Only **PDF, JPG, PNG** formats allowed    |
| File Size     | Maximum **5MB**                           |
| File Required | User must upload a file before submitting |

---

# System Behavior

| Event                | System Action                            |
| -------------------- | ---------------------------------------- |
| Document Rejected    | System enables **Reupload option**       |
| User Clicks Reupload | Popup window opens                       |
| File Uploaded        | System validates file type and size      |
| Submit Clicked       | Document is resubmitted for verification |
| Cancel Clicked       | Popup closes without changes             |

---

# Success Message

```text
Document uploaded successfully.
Your document has been resubmitted for verification.
```

---

## 2.7 Document Verification Success Screen (Client Facing)

```
┌─────────────────────────────────────────────────────────────┐
│              APPROVAL SUCCESS SCREEN                         │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                                                     │    │
│  │              ✅ DOCUMENTS APPROVED                 │    │
│  │                                                     │    │
│  │     Congratulations! Your documents have been      │    │
│  │     verified successfully.                         │    │
│  │                                                     │    │
│  │     You can now proceed to subscription.           │    │
│  │                                                     │    │
│  │     [PROCEED TO SUBSCRIPTION]                       │    │
│  │                                                     │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

## 2.8 Subscription Screen (Client Side)

```
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
│  │  Configuration:                                      │    │
│  │  • Branch Count:         [____]                     │    │
│  │  • Technician Count:     [____]                     │    │
│  │                                                      │    │
│  │  Duration:                                           │    │
│  │  ○ Monthly  ○ Quarterly  ○ Yearly  ○ Custom Date    │    │
│  │                                                      │    │
│  │  [CALCULATE TOTAL] → Shows calculation popup        │    │
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
| Exrta Technician Price | Currency (Read-only) | Auto     | From plan configuration             |
| Extra Branch Price     | Currency (Read-only) | Auto     | From plan configuration             |
| Branch Count         | Number               | Yes      | Number of branches to subscribe     |
| Technician Count     | Number               | Yes      | Number of technicians to subscribe  |
| Duration             | Radio Button         | Yes      | Monthly/Quarterly/Yearly/Custom     |
| Calculate Total      | Button               | -        | Shows calculation breakdown         |
| Pay Now              | Button               | -        | Triggers payment gateway            |

### Validation Rules

| Field             | Rule                                                 |
| ----------------- | ---------------------------------------------------- |
| Plan Selection    | Must select from available active plans              |
| Branch Count      | Required, minimum 1, maximum as per plan limit       |
| Technician Count  | Required, minimum 1, maximum as per plan limit       |
| Duration          | Must select one option                               |
| Custom Date Range | If Custom selected, From Date must be before To Date |

---

# 2.9 Subscription Module (Company Admin Sidebar)

The **Subscription Module** allows the **Company Admin** to manage and monitor the organization's subscription plan, including purchased resources, validity, and billing details.

This module is accessible from the **Company Admin Sidebar**.

---

## Sidebar Navigation Structure

```text
Home

Role Management
    ├─ Role Config
    └─ Salary & Leave

Branch Management

Employee Management

Stock Management
    ├─ Tax
    ├─ Stock Details
    ├─ Products
    └─ Services

Subscription Management
```

The **Subscription Management section** allows administrators to:

- View purchased subscription plans
- Track subscription validity
- Monitor resource usage (branches, technicians, users)
- Access subscription purchase logs
- View detailed billing and payment information

---

# 2.10 Subscription Purchase List (Logs)

This screen displays the **history of all purchased subscriptions** for the company.

Each record represents a **subscription purchase entry**.

---

## Table View

| Field                   | Description                              |
| ----------------------- | ---------------------------------------- |
| Subscription ID         | Unique identifier of the subscription    |
| Plan Type               | Type of subscription plan                |
| Duration                | Subscription duration (Monthly / Yearly) |
| Branches                | Number of branches included              |
| Extra Branches          | Number of extra branches included        |
| Technicians             | Number of technicians included           |
| Extra Technicians       | Number of extra technician included      |
| Subscription Start Date | Date when subscription becomes active    |
| Expiry Date             | Subscription end date                    |
| Status                  | Active / Expired / Cancelled             |
| Action                  | View icon to open subscription details   |

---

## Available Actions

| Action | Description                             |
| ------ | --------------------------------------- |
| View   | Opens detailed subscription information |

---


## **Filters & Search Configuration**

| Filter / Search Field | Type        | Options / Behavior                                         | Notes                  |
| --------------------- | ----------- | ---------------------------------------------------------- | ---------------------- |
| Search (Global)       | Text Search | Search by Subscription ID / Plan Type                      | Supports partial match |
| Status                | Dropdown    | Active / Expired / Cancelled                               | Single select          |
| Plan Type             | Dropdown    | Basic / Standard / Premium (configurable from plan master) | Dynamic values         |
| Date Range            | Date Picker | Filter by Subscription Start Date (From – To)              | Range selection        |
| Expiry Range          | Date Picker | Filter by Expiry Date (From – To)                          | Range selection        |

---

# 2.11 View Subscription Details

This screen provides **complete details of a selected subscription**, including plan information, resources, pricing, and payment details.

---

## Subscription Details Screen

```text
┌──────────────────────────────────────────────────────────────┐
│                     SUBSCRIPTION DETAILS                     │
└──────────────────────────────────────────────────────────────┘

Subscription ID
560096

Status : Active
-267 days remaining


---------------------------------------------------------------
PLAN INFORMATION
---------------------------------------------------------------

Plan Type          : Professional
Duration           : Yearly
Start Date         : 2024-03-01
End Date           : 2025-03-01
Purchase Date      : 2024-03-01
Days Remaining     : -267 days


---------------------------------------------------------------
INCLUDED RESOURCES
---------------------------------------------------------------

Number of Branches     : 3
Price per Branch       : ₹5,000

Number of Technicians  : 10
Price per Technician   : ₹2,000


---------------------------------------------------------------
PRICING DETAILS
---------------------------------------------------------------

Branch Cost (3 × ₹5,000)        : ₹15,000
Technician Cost (10 × ₹2,000)   : ₹20,000

Subtotal                        : ₹35,000
GST (18%)                       : ₹6,300

Final Total                     : ₹41,300


---------------------------------------------------------------
PAYMENT INFORMATION
---------------------------------------------------------------

Payment Method      : Credit Card
Transaction ID      : TXN-20240301-001
Auto-Renew          : Enabled
```

---

## System Behavior

| Event                 | System Action                            |
| --------------------- | ---------------------------------------- |
| User clicks View icon | Opens Subscription Details page          |
| Subscription Active   | Remaining days calculated automatically  |
| Subscription Expired  | Status displayed as Expired              |
| Auto-Renew Enabled    | System automatically renews subscription |

---

## Status Types

| Status    | Description                     |
| --------- | ------------------------------- |
| Active    | Subscription currently valid    |
| Expired   | Subscription validity ended     |
| Cancelled | Subscription manually cancelled |

---

# 🎯 MODULE 3: SUPER ADMIN (SERAVION) MANAGEMENT

## Overview

Internal Seravion operations module for tenant management, subscription oversight, and platform administration. This module provides complete control over client companies, subscription plans, and system-wide configurations.

**Module Connections:**

- Receives from: Module 2 (Document submissions for approval)
- Controls: Module 4 (Subscription Plans creation)
- Manages: Module 5 (Role Templates for clients)

---

## 3 Super Admin Sidebar Structure

```
┌─────────────────────────────────────────────────────────────┐
│              SUPER ADMIN SIDEBAR                             │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  🏢 Company Management                               │    │
│  │     └── Company list and approval management         │    │
│  │                                                      │    │
│  │  💳 Subscription Plans (Module 4)                    │    │
│  │     └── Manage pricing plans for clients             │    │
│  │                                                      │    │
│  │  📈 Reports                                          │    │
│  │     └── Platform analytics & tenant reports          │    │
│  │                                                      │    │
│  │  🔐 Role Management (Module 5)                       │    │
│  │     └── Define system-wide role templates            │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ⚠️ VISIBLE ONLY TO SUPER ADMIN ROLE                         │
└─────────────────────────────────────────────────────────────┘
```

---

## 3.1 Company Management (Seravion Side)

```
┌─────────────────────────────────────────────────────────────┐
│              COMPANY MANAGEMENT SCREEN                       │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Filters:                                            │    │
│  │  Status: [▼ All/Pending/Approved/Rejected ▼]        │    │
│  │  Search: [____________] (Company Name/Email)        │    │
│  │                                                      │    │
│  │  COMPANY LIST TABLE:                                 │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │ Company │ Email │ Phone │ Status │ Actions │    │    │
│  │  │─────────┼───────┼───────┼────────┼─────────│    │    │
│  │  │ABC Pest │a@...  │98...  │●Pending│ View    │    │    │
│  │  │XYZ Pest │x@...  │99...  │✓Approve│ View    │    │    │
│  │  │123 Pest │1@...  │97...  │✗Reject │ View    │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STATUS DROPDOWN OPTIONS:                                    │
│  • Approve                                                   │
│  • Pending                                                   │
│  • Rejected (with reason hover tooltip)                      │
└─────────────────────────────────────────────────────────────┘
```

---

### Table View Fields

| Field               | Description                                             |
| ------------------- | ------------------------------------------------------- |
| Company ID          | Unique identifier for the company                       |
| Company Name        | Registered company name                                 |
| Email,ContactPerson | Official company email ,person name                     |
| Created On          | Date when company account was created                   |
| Subscription Start  | Subscription activation date                            |
| Expiry Date         | Subscription expiration date                            |
| Location            | Company primary location                                |
| Branches            | Total number of branches registered                     |
| Technicians         | Total number of technicians under the company           |
| Status              | Current approval status (Pending / Approved / Rejected) |
| Action(Edit/view)   | Edit icon to modify company details                     |

---

### Company Management Fields

| Field         | Type     | Required | Notes                            |
| ------------- | -------- | -------- | -------------------------------- |
| Status Filter | Dropdown | No       | All, Pending, Approved, Rejected |
| Search        | Text     | No       | Company Name or Email            |

---

### Validation Rules

| Field         | Rule                                   |
| ------------- | -------------------------------------- |
| Status Filter | Must be valid status value             |
| Search        | Minimum 3 characters to trigger search |

---

## 3.2 View Company Details (Seravion Side) - Before Approval

```
┌─────────────────────────────────────────────────────────────┐
│      PARTICULAR COMPANY DETAILS (Pending Approval)           │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  COMPANY INFORMATION                                 │    │
│  │  • Company Name: [Value]                            │    │
│  │  • Industry Type: [Value]                           │    │
│  │  • Contact Person: [Value]                          │    │
│  │  • Email: [Value]                                   │    │
│  │  • Phone: [Value]                                   │    │
│  │  • Address: [Full Address]                          │    │
│  │                                                      │    │
│  │  DOCUMENTS UPLOADED:                                 │    │
│  │  • GST Document: [📎 View]                          │    │
│  │  • PAN Document: [📎 View]                          │    │
│  │  • Business Doc: [📎 View]                          │    │
│  │                                                      │    │
│  │  APPROVAL CONTROLS:                                  │    │
│  │  Status: [▼ Approve/Pending/Rejected ▼]             │    │
│  │                                                      │    │
│  │  IF REJECTED:                                        │    │
│  │  Rejection Reason: [____________________]           │    │
│  │                                                      │    │
│  │  ☐ Enable Trial (Checkbox)                          │    │
│  │                                                      │    │
│  │  IF CHECKED:                                         │    │
│  │    • From Date:        [📅 Date Picker]             │    │
│  │    • To Date:          [📅 Date Picker]             │    │
│  │                                                      │    │
│  │  [SAVE CHANGES]                                      │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Company Details Fields

| Field                   | Type            | Required    | Behavior                                   |
| ----------------------- | --------------- | ----------- | ------------------------------------------ |
| Company ID              | Text / Label    | No          | System generated unique company identifier |
| Company Name            | Text            | Yes         | Registered company name                    |
| Industry Type           | Dropdown        | Yes         | Select industry category                   |
| Contact Person Name     | Text            | Yes         | Primary contact person                     |
| Contact Email           | Email           | Yes         | Used for communication and login           |
| Contact Phone           | Phone           | Yes         | Registered contact number                  |
| Shop No                 | Text            | No          | Shop or office number                      |
| Address Line 1          | Text            | Yes         | Primary business address                   |
| Address Line 2          | Text            | No          | Additional address information             |
| Country                 | Dropdown        | Yes         | Country selection                          |
| State                   | Dropdown        | Yes         | State based on selected country            |
| City                    | Dropdown / Text | Yes         | City location                              |
| Pincode                 | Number          | Yes         | Postal code                                |
| GST Number              | Text            | Conditional | Required if GST registered                 |
| PAN Number              | Text            | Yes         | Business PAN number                        |
| License Number          | Text            | Conditional | Business license number                    |
| Business Certificate    | File Upload     | Conditional | Upload supporting certificate              |
| Aadhaar Document        | File Upload     | Conditional | Aadhaar verification document              |
| GST Document            | File Upload     | Conditional | GST certificate upload                     |
| Status                  | Dropdown        | Yes         | Controls company approval state            |
| Rejection Reason        | Textarea        | Conditional | Required if Status = Rejected              |
| Enable Trial            | Checkbox        | No          | Enables trial period configuration         |
| Trial From Date         | Date            | Conditional | Required if Enable Trial = true            |
| Trial To Date           | Date            | Conditional | Required if Enable Trial = true            |
| Number of Branches      | Number          | Yes         | Total allowed branches                     |
| Number of Technicians   | Number          | Yes         | Total allowed technicians                  |
| Admin Comment           | Textarea        | No          | Internal notes by admin                    |

---

### Validation Rules

| Field                 | Rule                                                      |
| --------------------- | --------------------------------------------------------- |
| Company Name          | Must not be empty                                         |
| Contact Email         | Must be valid email format                                |
| Contact Phone         | Must be valid 10-digit phone number                       |
| PAN Number            | Must follow valid PAN format                              |
| GST Number            | Must follow valid GST format                              |
| Status                | Must be one of: Approve, Pending, Rejected                |
| Rejection Reason      | Required if Status = Rejected, minimum 10 characters      |
| Trial From Date       | Required if Enable Trial checked, must be today or future |
| Trial To Date         | Required if Enable Trial checked, must be after From Date |
| Number of Branches    | Must be a positive number                                 |
| Number of Technicians | Must be a positive number                                 |

---

# 3.3 View Company Details – After Document Approved

This screen allows the **Seravion Admin** to **view complete company information after document verification**.
All fields are **read-only** and displayed only for **monitoring and administrative reference**.

---

```
┌─────────────────────────────────────────────────────────────────┐
│                    COMPANY DETAILS (Approved)                   │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────────┐ │
│ │ COMPANY DETAILS                                             │ │
│ │                                                             │ │
│ │ Company ID            : 560096                              │ │
│ │ Company Name          : ABC Pest Control                    │ │
│ │ Industry Type         : Pest Management                     │ │
│ │ Contact Person        : Rahul Sharma                        │ │
│ │ Contact Email         : abc@company.com                     │ │
│ │ Contact Phone         : 9876543210                          │ │
│ │                                                             │ │
│ │ Address                                                      │ │
│ │ Shop No              : 21A                                   │ │
│ │ Address Line 1       : MG Road                              │ │
│ │ Address Line 2       : Near Metro Station                   │ │
│ │ Country              : India                                │ │
│ │ State                : Karnataka                            │ │
│ │ City                 : Bangalore                            │ │
│ │ Pincode              : 560001                               │ │
│ └─────────────────────────────────────────────────────────────┘ │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────────┐ │
│ │ DOCUMENT DETAILS                                            │ │
│ │                                                             │ │
│ │ GST Number           : 29ABCDE1234F1Z5                      │ │
│ │ PAN Number           : ABCDE1234F                           │ │
│ │ License Number       : LIC-8899                             │ │
│ │                                                             │ │
│ │ GST Document         : [View] [Download]                    │ │
│ │ PAN Document         : [View] [Download]                    │ │
│ │ Business Certificate : [View] [Download]                    │ │
│ │                                                             │ │
│ │ Verification Status  : Approved                             │ │
│ │ Reject Reason        : -                                    │ │
│ │ Admin Comment        : Verified and approved                │ │
│ └─────────────────────────────────────────────────────────────┘ │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────────┐ │
│ │ SUBSCRIPTION DETAILS                                        │ │
│ │                                                             │ │
│ │ Subscription ID      : 560096                               │ │
│ │ Status               : Active                               │ │
│ │ Days Remaining       : -267 days                            │ │
│ │                                                             │ │
│ │ Plan Information                                            │ │
│ │ Plan Type            : Professional                         │ │
│ │ Duration             : Yearly                               │ │
│ │ Start Date           : 2024-03-01                           │ │
│ │ End Date             : 2025-03-01                           │ │
│ │ Purchase Date        : 2024-03-01                           │ │
│ │                                                             │ │
│ │ Included Resources                                          │ │
│ │ Number of Branches   : 3                                    │ │
│ │ Number of Technicians: 10                                   │ │
│ │                                                             │ │
│ │ Pricing Details                                             │ │
│ │ Price per Branch        : ₹5,000                            │ │
│ │ Branch Cost (3 × 5000)  : ₹15,000                           │ │
│ │ Price per Technician    : ₹2,000                            │ │
│ │ Technician Cost (10×2000): ₹20,000                          │ │
│ │ Subtotal                : ₹35,000                           │ │
│ │ GST (18%)               : ₹6,300                            │ │
│ │ Final Total             : ₹41,300                           │ │
│ │                                                             │ │
│ │ Payment Information                                         │ │
│ │ Payment Method       : Credit Card                          │ │
│ │ Transaction ID       : TXN-20240301-001                     │ │
│ │ Auto Renew           : Enabled                              │ │
│ └─────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

---

# View Fields

| Field                 | Description                               |
| --------------------- | ----------------------------------------- |
| Company ID            | Unique identifier assigned to the company |
| Company Name          | Registered company name                   |
| Industry Type         | Type of business                          |
| Contact Person        | Primary contact person                    |
| Contact Email         | Company communication email               |
| Contact Phone         | Company phone number                      |
| Address               | Business address details                  |
| GST Number            | GST registration number                   |
| PAN Number            | PAN number of the business                |
| License Number        | Business license                          |
| Documents             | GST, PAN, and Business Certificate files  |
| Verification Status   | Document approval status                  |
| Reject Reason         | Displayed if application was rejected     |
| Number of Branches    | Total allowed branches                    |
| Number of Technicians | Total allowed technicians                 |
| Plan Type             | Subscription plan name                    |
| Duration              | Billing cycle                             |
| Start Date            | Subscription start                        |
| End Date              | Subscription expiry                       |
| Days Remaining        | Remaining subscription days               |
| Pricing Details       | Cost breakdown of subscription            |
| Final Total           | Total amount paid                         |
| Payment Method        | Payment type used                         |
| Transaction ID        | Payment transaction reference             |
| Auto Renew            | Indicates if subscription auto-renews     |

---

# 🎯 MODULE 4: SUBSCRIPTION PLANS

## Overview

Handles plan creation, pricing configuration, and subscription lifecycle management from Super Admin side. These plans are then available for Company Admins to purchase in Module 2.

**Module Connections:**

- Created by: Module 3 (Super Admin)
- Consumed by: Module 2 (Client Subscription Screen)
- Referenced in: Module 3 (Company Details view)

---

# 4.1 Get All Plans – List View Screen

This screen allows the **Seravion Admin** to **view and manage all available subscription plans** in the system.

The screen displays **plan summary statistics** along with the **plan list table**.

---

```
┌────────────────────────────────────────────────────────────────────┐
│                 SUBSCRIPTION PLANS MANAGEMENT                      │
│                                                                    │
│  Plan Summary                                                      │
│  Total Plans : 12   |   Active Plans : 10   |   Inactive Plans : 2  │
│                                                                    │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │  [+ ADD NEW PLAN]                                            │  │
│  │                                                             │  │
│  │  PLANS LIST TABLE                                           │  │
│  │  ┌───────────────────────────────────────────────────────┐  │  │
│  │  │ Plan Name │ Branch Cnt │ Branch ₹ │ Tech Cnt │ Tech ₹ │ │  │
│  │  │ Description │ Created On │ Status │ Actions │        │ │  │
│  │  │──────────────────────────────────────────────────────│ │  │
│  │  │ Basic   │ 3 │ 500 │ 10 │ 100 │ Starter plan │Active │ │  │
│  │  │ Pro     │ 5 │ 450 │ 20 │ 90  │ Business plan│Active │ │  │
│  │  │ Enterprise│10│400 │50 │80 │ Enterprise plan │Inactive│ │ │
│  │  │                                                          │ │
│  │  │ Actions: [Edit] [Delete]                                 │ │
│  │  └───────────────────────────────────────────────────────┘  │  │
│  │                                                             │  │
│  └─────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│  FLOW:                                                       │
│  • Click Edit → Open Edit Plan screen                        │
│  • Click Delete → Open Delete confirmation popup             │
│    (Soft delete – plan marked as Inactive)                   │
└─────────────────────────────────────────────────────────────┘
```

---

# Plan Summary Fields

| Field          | Description                                |
| -------------- | ------------------------------------------ |
| Total Plans    | Total number of subscription plans created |
| Active Plans   | Number of currently active plans           |
| Inactive Plans | Number of disabled or inactive plans       |

---

# Plan List Fields

| Field            | Type     | Required | Notes                                  |
| ---------------- | -------- | -------- | -------------------------------------- |
| Plan Name        | Text     | Auto     | Name of the subscription plan          |
| Branch Count     | Number   | Auto     | Number of branches allowed in the plan |
| Branch Price     | Currency | Auto     | Cost per branch                        |
| Technician Count | Number   | Auto     | Number of technicians allowed          |
| Technician Price | Currency | Auto     | Cost per technician                    |
| Description      | Text     | Auto     | Short description of the plan          |
| Date of Creation | Date     | Auto     | Date when the plan was created         |
| Status           | Badge    | Auto     | Active / Inactive                      |
| Actions          | Buttons  | -        | Edit, Delete                           |

---

# System Behavior

| Event        | System Action                                   |
| ------------ | ----------------------------------------------- |
| Add New Plan | Opens Plan Creation Screen                      |
| Edit Plan    | Opens Edit Plan Screen                          |
| Delete Plan  | Soft deletes the plan (sets status to Inactive) |
| Plan Status  | Determines if plan is available for purchase    |

---

## 4.2 Add Plan Screen

```
┌─────────────────────────────────────────────────────────────┐
│              ADD SUBSCRIPTION PLAN                           │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Plan Name:              [____________] *           │    │
│  │                                                      │    │
│  │  Per Branch Pricing:                                 │    │
│  │  • Total Branch Count:   [____] units               │    │
│  │  • Price Per Branch:     ₹ [____]                   │    │
│  │                                                      │    │
│  │  Per Technician Pricing:                             │    │
│  │  • Total Technician Count: [____] units             │    │
│  │  • Price Per Technician: ₹ [____]                   │    │
│  │                                                      │    │
│  │  Description:            [____________________]     │    │
│  │                          [Textarea]                 │    │
│  │                                                      │    │
│  │  Duration Options:                                   │    │
│  │  ☑ Monthly  ☑ Quarterly  ☑ Yearly  Custom with date│    │
│  │                                                      │    │
│  │  [SAVE]  [CANCEL]                                    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

| Field                          | Type         | Required | Notes                                      |
| ------------------------------ | ------------ | -------- | ------------------------------------------ |
| Plan Name                      | Text         | Yes      | Unique identifier                          |
| Total Branch Count             | Number       | Yes      | Base branch quantity                       |
| Price Per Branch               | Currency     | Yes      | Unit price for base branches               |
| **Extra Price Per Branch**     | Currency     | Yes      | Price for additional branches beyond limit |
| Total Technician Count         | Number       | Yes      | Base technician quantity                   |
| Price Per Technician           | Currency     | Yes      | Unit price for base technicians            |
| **Extra Price Per Technician** | Currency     | Yes      | Price for additional technicians           |
| Description                    | Textarea     | No       | Plan features                              |
| Duration                       | Multi-select | Yes      | Available billing cycles                   |


### Validation Rules

| Field                  | Rule                                                |
| ---------------------- | --------------------------------------------------- |
| Plan Name              | Required, unique across all plans, min 3 characters |
| Total Branch Count     | Required, minimum 1, maximum 999                    |
| Price Per Branch       | Required, numeric, minimum 0, max 2 decimal places  |
| Total Technician Count | Required, minimum 1, maximum 999                    |
| Price Per Technician   | Required, numeric, minimum 0, max 2 decimal places  |
| Description            | Optional, max 500 characters if provided            |
| Duration               | Required, at least one option must be selected      |

---

## 4.3 Edit Plan Screen

```
┌─────────────────────────────────────────────────────────────┐
│              EDIT SUBSCRIPTION PLAN                          │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Same fields as Add Plan screen                      │    │
│  │  Pre-filled with existing data  
│  │    Status : Active
│  │                                                      │    │
│  │  ⚠️ Changes apply only to new subscriptions         │    │
│  │     (not already purchased ones)                     │    │
│  │                                                      │    │
│  │  [SAVE]  [CANCEL]                                    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Edit Plan Fields
| Field                          | Type         | Required | Notes                |
| ------------------------------ | ------------ | -------- | -------------------- |
| Plan Name                      | Text         | Yes      | Pre-filled, editable |
| Total Branch Count             | Number       | Yes      | Pre-filled, editable |
| Price Per Branch               | Currency     | Yes      | Pre-filled, editable |
| **Extra Price Per Branch**     | Currency     | Yes      | Pre-filled, editable |
| Total Technician Count         | Number       | Yes      | Pre-filled, editable |
| Price Per Technician           | Currency     | Yes      | Pre-filled, editable |
| **Extra Price Per Technician** | Currency     | Yes      | Pre-filled, editable |
| Description                    | Textarea     | No       | Pre-filled, editable |
| Status                         | Badge        |	Auto     |	Active / Inactive   |
| Duration                       | Multi-select | Yes      | Pre-filled, editable |


### Validation Rules

| Field                  | Rule                                             |
| ---------------------- | ------------------------------------------------ |
| Plan Name              | Required, uniqueness check excludes current plan |
| Total Branch Count     | Required, minimum 1                              |
| Price Per Branch       | Required, numeric, minimum 0                     |
| Total Technician Count | Required, minimum 1                              |
| Price Per Technician   | Required, numeric, minimum 0                     |
| Duration               | Required, at least one option must be selected   |

---

## 4.4 Delete Plan - Warning Popup

```
┌─────────────────────────────────────────────────────────────┐
│              DELETE CONFIRMATION                             │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                                                     │    │
│  │              ⚠️ CANNOT DELETE PLAN                 │    │
│  │                                                     │    │
│  │  "This plan is currently assigned to active        │    │
│  │   companies. It cannot be permanently deleted."    │    │
│  │                                                     │    │
│  │  Options:                                           │    │
│  │                                                     │    │
│  │  [CANCEL]          [MARK AS INACTIVE]               │    │
│  │                                                     │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  LOGIC:                                                      │
│  • If plan has active users → Do not allow hard delete       │
│  • Only allow status change to Inactive                      │
│  • Inactive plans not available for new purchases            │
│  • Remain visible in history                                 │
└─────────────────────────────────────────────────────────────┘
```

---

# 🎯 MODULE 5: ROLE MANAGEMENT (SERAVION SIDE)

## Overview

Central RBAC (Role-Based Access Control) module for Super Admin to create system-wide role templates. These templates propagate to all client companies for customization.

**Module Connections:**

- Creates templates for: Module 5.5 (Client Role Management)
- Referenced by: Module 8 (User Management for role assignment)
- Different from: Module 5.5 (Client creates company-specific roles)

---

# 5 Role Management Dashboard

This screen allows the **Company Admin** to manage system roles and monitor role usage across the organization.

The dashboard shows **role statistics cards** and a **table view of all roles with permissions and user assignments**.

---

```
┌─────────────────────────────────────────────────────────────────┐
│                     ROLE MANAGEMENT DASHBOARD                   │
│                                                                 │
│  STATISTICS                                                     │
│  ┌───────────────┐ ┌───────────────┐ ┌────────────────────┐     │
│  │ Total Roles   │ │ Active Roles  │ │ Total Users        │     │
│  │      8        │ │       6       │ │        42          │     │
│  └───────────────┘ └───────────────┘ └────────────────────┘     │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                     ROLE LIST TABLE                        │ │
│  │                                                           │ │
│  │ ┌──────────────────────────────────────────────────────┐  │ │
│  │ │ Role Name │ Status │ Users │ Permission Modules │Action│ │ │
│  │ │───────────┼────────┼───────┼───────────────────┼──────│ │ │
│  │ │ Admin     │Active  │ 5     │ 12 Modules        │Edit  │ │ │
│  │ │ Manager   │Active  │ 10    │ 8 Modules         │Edit  │ │ │
│  │ │ Technician│Active  │ 20    │ 5 Modules         │Edit  │ │ │
│  │ │ Viewer    │Inactive│ 7     │ 2 Modules         │Edit  │ │ │
│  │ │                                                     │ │ │
│  │ │ Actions: [View] [Edit] [Delete]                     │ │ │
│  │ └──────────────────────────────────────────────────────┘ │ │
│  └───────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

---

# Statistics Cards

| Field        | Description                                 |
| ------------ | ------------------------------------------- |
| Total Roles  | Total number of roles created in the system |
| Active Roles | Number of roles currently active            |
| Total Users  | Total users assigned across all roles       |

---

# Role List Table Fields

| Field                          | Type    | Required | Notes                                     |
| ------------------------------ | ------- | -------- | ----------------------------------------- |
| Role Name                      | Text    | Auto     | Name of the role                          |
| Status                         | Badge   | Auto     | Active / Inactive                         |
| Total User Count               | Number  | Auto     | Number of users assigned to the role      |
| Module Permission Matrix Count | Number  | Auto     | Number of modules this role has access to |
| Action                         | Buttons | -        | View, Edit, Delete                        |

---

# System Behavior

| Event         | System Action                                     |
| ------------- | ------------------------------------------------- |
| View Role     | Opens Role Permission Details                     |
| Edit Role     | Opens Role Configuration Screen                   |
| Delete Role   | Soft deletes role if not assigned to active users |
| Status Change | Role becomes Active or Inactive                   |

---

## 5.1 Create Role (Seravion/Super Admin Side)

```
┌─────────────────────────────────────────────────────────────┐
│     ROLE MANAGEMENT (Super Admin Sidebar)                    │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  [+ ADD ROLE]                                        │    │
│  │                                                      │    │
│  │  ROLE TEMPLATES LIST:                                │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │ Role Name │ App User │ Created │ Actions    │    │    │
│  │  │───────────┼──────────┼─────────┼────────────│    │    │
│  │  │ Sales Mgr │    No    │ Jan 24  │ View Edit  │    │    │
│  │  │ Tech App  │   Yes    │ Jan 24  │ View Edit  │    │    │
│  │  │           │          │         │ Del        │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                              │                               │
│                              ▼                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  ADD/EDIT ROLE FORM                                  │    │
│  │  ─────────────────────────────────────────────────   │    │
│  │                                                      │    │
│  │  Role Name:                    [____________] *      │    │
│  │  description   :               [____________] *      │    │
│  │                                                      │    │
│  │  ☐ Is Application User ⭐ CRITICAL CHECKBOX         │    │
│  │                                                      │    │
│  │  IF CHECKED:                                         │    │
│  │    → DISABLE all module permission toggles          │    │
│  │    → User gets mobile app access only               │    │
│  │                                                      │    │
│  │  IF UNCHECKED:                                       │    │
│  │    → SHOW Module-wise Permission Grid               │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Add Role Fields - Basic Info

| Field               | Type     | Required | Notes                         |
| ------------------- | -------- | -------- | ----------------------------- |
| Role Name           | Text     | Yes      | Unique template name          |
| description         | Text     | Yes      | description of the role       |
| Is Application User | Checkbox | No       | If true, disables permissions |

### Validation Rules

| Field               | Rule                                             |
| ------------------- | ------------------------------------------------ |
| Role Name           | Required, unique across system, min 3 characters |
| Is Application User | Boolean, defaults to false                       |

---

```
┌─────────────────────────────────────────────────────────────┐
│  Module-wise Permission Settings (25+ Modules)               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                                                      │    │
│  │  ┌─────────────────────────────────────────────────┐│    │
│  │  │ MODULE: User Management                         ││    │
│  │  │ ☑ VIEW  ☑ ADD  ☑ EDIT  ☑ DELETE  ☑ EXPORT      ││    │
│  │  │ ☑ CONFIGURE                                     ││    │
│  │  │                                                  ││    │
│  │  │ ☑ REQUEST  →  Show: [Multi-select Roles ▼]      ││    │
│  │  │               (Receivers of requests)           ││    │
│  │  │                                                  ││    │
│  │  │ ☑ APPROVAL →  Show: Info label                  ││    │
│  │  │               "Can approve incoming requests"   ││    │
│  │  └─────────────────────────────────────────────────┘│    │
│  │                                                      │    │
│  │  ┌─────────────────────────────────────────────────┐│    │
│  │  │ MODULE: Inventory                               ││    │
│  │  │ ☑ VIEW  ☑ ADD  ☐ EDIT  ☐ DELETE  ☑ EXPORT      ││    │
│  │  │ ☐ CONFIGURE  ☐ REQUEST  ☐ APPROVAL              ││    │
│  │  └─────────────────────────────────────────────────┘│    │
│  │                                                      │    │
│  │  [Repeat for 25+ modules...]                        │    │
│  │                                                      │    │
│  │  [SAVE]  [CANCEL]                                    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Permission Toggle Behavior

| Permission    | Description                                   | Dependencies                                     |
| ------------- | --------------------------------------------- | ------------------------------------------------ |
| **VIEW**      | Read/view records within the module           | Base permission                                  |
| **ADD**       | Create new records                            | Usually paired with VIEW                         |
| **EDIT**      | Modify existing records                       | Requires VIEW                                    |
| **DELETE**    | Remove or deactivate records                  | Requires VIEW                                    |
| **REQUEST**   | Initiate a request that routes to an approver | Requires VIEW; Shows multi-select roles dropdown |
| **APPROVAL**  | Review and approve workflow requests          | Requires VIEW; Shows info label only             |
| **EXPORT**    | Download module data as CSV, PDF, or Excel    | Requires VIEW                                    |
| **CONFIGURE** | Master setting access — module configuration  | Admin-level                                      |

### Validation Rules

| Permission      | Rule                                                            |
| --------------- | --------------------------------------------------------------- |
| REQUEST         | If enabled, at least one Request Receiver role must be selected |
| ADD/EDIT/DELETE | Requires VIEW to be enabled first                               |
| CONFIGURE       | Can be enabled independently (admin level)                      |

---

## 5.2 Edit Role (Seravion Side)

```
┌─────────────────────────────────────────────────────────────┐
│              EDIT ROLE SCREEN                                │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  ⚠️ WARNING:                                        │    │
│  │  "Changes will impact all users assigned to this    │    │
│  │   role across all client companies."                │    │
│  │                                                      │    │
│  │  [Same form as Add Role with pre-filled data]       │    │
│  │                                                      │    │
│  │  [SAVE]  [CANCEL]                                    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

## 5.3 View Role Detail (Seravion Side)

```
┌─────────────────────────────────────────────────────────────┐
│              VIEW ROLE DETAILS                               │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Role Name: [Value]                                  │    │
│  │  Is Application User: [Yes/No]                       │    │
│  │                                                      │    │
│  │  MODULE PERMISSIONS (Read-only):                     │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │ Module       │ View │ Add │ Edit │ Del │...│    │    │
│  │  │──────────────┼──────┼─────┼──────┼─────┼───│    │    │
│  │  │User Mgmt     │  ✓   │  ✓  │  ✓   │  ✓  │...│    │    │
│  │  │Inventory     │  ✓   │  ✓  │  ✗   │  ✗  │...│    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │                                                      │    │
│  │  Approval Authority Roles: [List if applicable]      │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

## 5.4 Delete Role (With Warning & Migration Handling)

```
┌─────────────────────────────────────────────────────────────┐
│              DELETE ROLE WARNING                             │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                                                     │    │
│  │              ⚠️ USERS ASSIGNED TO THIS ROLE        │    │
│  │                                                     │    │
│  │  Active Users Count: [XX]                           │    │
│  │                                                     │    │
│  │  Select New Role for Migration:                     │    │
│  │  [▼ Dropdown of active roles ▼]                     │    │
│  │                                                     │    │
│  │  All users will be reassigned to selected role.    │    │
│  │  Current role will be marked as Inactive.          │    │
│  │                                                     │    │
│  │  [CANCEL]              [MIGRATE & DEACTIVATE]       │    │
│  │                                                     │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  LOGIC:                                                      │
│  • Show count of users assigned                              │
│  • Require selection of New Role for Migration (Dropdown)    │
│  • Reassign all users to selected role                       │
│  • Mark current role as Inactive (No hard delete)            │
│  • Inactive roles not available for new user assignment      │
└─────────────────────────────────────────────────────────────┘
```

### Delete Role Fields

| Field                  | Type     | Required | Notes                   |
| ---------------------- | -------- | -------- | ----------------------- |
| New Role for Migration | Dropdown | Yes      | Must select active role |

### Validation Rules

| Field                  | Rule                                           |
| ---------------------- | ---------------------------------------------- |
| New Role for Migration | Required, cannot select the role being deleted |

---

## 5.5 Role Management (Client Side)

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
│  │  [Same permission grid as Seravion side]            │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Client Add Role Fields

| Field                | Type     | Required | Notes                           |
| -------------------- | -------- | -------- | ------------------------------- |
| Select Role Template | Dropdown | Yes      | From Seravion templates         |
| Description          | Text     | No       | Description of Role             |
| Role Name            | Text     | Yes      | Company-specific name           |
| Is Application User  | Checkbox | No       | Disables permissions if checked |

### Validation Rules

| Field                 | Rule                                                    |
| --------------------- | ------------------------------------------------------- |
| Select Role Template  | Required, must select from available Seravion templates |
| Clone Permission From | Optional, if selected must be valid existing role       |
| Role Name             | Required, unique within company, min 3 characters       |

---

# 🎯 MODULE 6: ROLE SALARY & LEAVE CONFIGURATION

## Overview

This module allows the Company Admin to define salary structure and leave policies role-wise. During User Creation (Module 8), when a role is selected, the system auto-fetches this configuration.

**Module Connections:**

- Sub-module of: Module 5 (Role Management)
- Consumed by: Module 8 (User Creation - Steps 3 & 4 auto-fetch)
- References: Module 5 (Roles list for selection)

---

# 6.1 Role Salary & Leave Configuration List (Client Side)

```
┌─────────────────────────────────────────────────────────────┐
│  ROLE SALARY & LEAVE CONFIGURATION                          │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  Filters:                                           │   │
│  │  Role: [▼ Dropdown ▼]  Status: [▼ Active/Inactive ▼]│   │
│  │  Salary Type: [▼ Dropdown ▼]   Search: [____]       │   │
│  │                                                     │   │
│  │  [+ ADD CONFIGURATION]                              │   │
│  │                                                     │   │
│  │  CONFIGURATIONS TABLE                               │   │
│  │  ┌────────────────────────────────────────────────┐ │   │
│  │  │Role │Status│Eff From│Eff To│Salary Type│Basic ₹│ │   │
│  │  │Salary Eff From│Salary Eff To│Leave Auth│Action │ │   │
│  │  │────────────────────────────────────────────────│ │   │
│  │  │Sales │Active │01-Jan-24│31-Dec-24│CTC │50000   │ │   │
│  │  │01-Jan-24│31-Dec-24│Manager │View/Edit │ │   │
│  │  │Tech  │Active │01-Feb-24│31-Dec-24│Fixed│30000  │ │   │
│  │  │01-Feb-24│31-Dec-24│Team Lead│View/Edit│ │   │
│  │  └────────────────────────────────────────────────┘ │   │
│  │                                                     │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

# Configuration Table Fields

| Field                 | Type     | Required | Notes                                |
| --------------------- | -------- | -------- | ------------------------------------ |
| Role Name             | Text     | Auto     | Name of the role                     |
| Status                | Badge    | Auto     | Active / Inactive configuration      |
| Effective From        | Date     | Auto     | Configuration start date             |
| Effective To          | Date     | Auto     | Configuration end date               |
| Salary Type           | Text     | Auto     | CTC / Fixed / Hourly                 |
| Basic Salary          | Currency | Auto     | Base salary defined for the role     |
| Salary Effective From | Date     | Auto     | Salary rule start date               |
| Salary Effective To   | Date     | Auto     | Salary rule end date                 |
| Leave Approval        | Text     | Auto     | Role responsible for approving leave |
| Action                | Buttons  | -        | View, Edit                           |

---

# Filters / Search Fields

| Field                | Type       | Required | Notes                                   |
| -------------------- | ---------- | -------- | --------------------------------------- |
| Status Filter        | Dropdown   | No       | Active / Inactive configurations        |
| Salary Type Filter   | Dropdown   | No       | CTC / Fixed / Hourly                    |
| Effective From Range | Date Range | No       | Filter by configuration effective dates |
| Search               | Text       | No       | Search by role name                     |

---

# System Behavior

| Event             | System Action                                |
| ----------------- | -------------------------------------------- |
| Add Configuration | Opens Role Salary & Leave Configuration Form |
| View              | Opens configuration details screen           |
| Edit              | Opens edit configuration screen              |
| Filters Applied   | Table refreshes with filtered records        |

---

## 6.2 Add Configuration Form (Multi-Step)

```
┌─────────────────────────────────────────────────────────────┐
│  ADD ROLE CONFIGURATION - STEP 1: BASIC DETAILS              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Select Role:              [▼ Multi-select ▼] *     │    │
│  │  Effective From Date:      [📅 ________] *          │    │
│  │  Effective To Date:        [📅 ________]            │    │
│  │  Status:                   [▼ Active/Inactive ▼] *  │    │
│  │                                                      │    │
│  │  [NEXT ►]  [CANCEL]                                  │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 2: SALARY DETAILS                                      │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Salary Type:            [▼ CTC/Fixed/Hourly ▼] *   │    │
│  │  Basic Salary:           ₹ [________] *             │    │
│  │  HRA:                    ₹ [________]               │    │
│  │  Other Allowance:        ₹ [________]               │    │
│  │  Incentive:              ₹ [________]               │    │
│  │  Deductions:             ₹ [________]               │    │
│  │                                                      │    │
│  │  Statutory:                                          │    │
│  │  ☐ PF Applicable    ☐ ESI Applicable                │    │
│  │  ☐ TDS Applicable                                   │    │
│  │                                                      │    │
│  │
│  │  INCENTIVE CONFIGURATION                             │    │
│  │  ☐ Holiday Work Incentive Applicable                │    │
│  │    IF CHECKED:                                       │    │
│  │    • Type: [▼ Fixed/Per Day/Per Hour ▼]             │    │
│  │    • Amount: ₹ [________]                           │    │
│  │                                                      │    │
│  │  ☐ Overtime Applicable                              │    │
│  │    IF CHECKED:                                       │    │
│  │    • Type: [▼ Per Hour/Per Shift ▼]                 │    │
│  │    • Shift Type: [________]                         │    │
│  │    • Shift Incentive: ₹ [________]                  │    │
│  │    • Per Hour Pay: ₹ [________]                     │    │
│  │    • Max OT Hours/Month: [____]                     │    │
│  │                                                      │    │
│  │  [◄ PREVIOUS]  [NEXT ►]  [SKIP]  [CANCEL]            │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 3: LEAVE DETAILS                                       │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Casual Leave (CL)/Year:   [____] days              │    │
│  │  Sick Leave (SL)/Year:     [____] days              │    │
│  │  Paid Leave (PL)/Year:     [____] days              │    │
│  │  Annual Leave Allocation:  [____] days              │    │
│  │                                                      │    │
│  │  ☐ Carry Forward Allowed                            │    │
│  │    IF CHECKED:                                       │    │
│  │    • Max Carry Forward Days: [____]                 │    │
│  │                                                      │    │
│  │  Leave Approval Authority: [▼ Select Role ▼] *      │    │
│  │  Leave Reset Cycle:        [▼ Yearly/Monthly/Custom ▼]*│   │
│  │                                                      │    │
│  │  [◄ PREVIOUS]  [SAVE]  [SKIP]  [CANCEL]              │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Step 1: Basic Details Fields

| Field               | Type         | Required | Notes                             |
| ------------------- | ------------ | -------- | --------------------------------- |
| Select Role         | Multi-select | Yes      | Can select multiple roles         |
| Effective From Date | Date         | Yes      | When configuration becomes active |
| Effective To Date   | Date         | No       | Optional end date                 |
| Status              | Dropdown     | Yes      | Active/Inactive                   |

### Step 1 Validation Rules

| Field               | Rule                                                    |
| ------------------- | ------------------------------------------------------- |
| Select Role         | Required, at least one role must be selected            |
| Effective From Date | Required, cannot be past date, must be today or future  |
| Effective To Date   | Optional, if provided must be after Effective From Date |
| Status              | Required, must be Active or Inactive                    |

### Step 2: Salary Details Fields

| Field                             | Type     | Required    | Notes                         |
| --------------------------------- | -------- | ----------- | ----------------------------- |
| Salary Type                       | Dropdown | Yes         | CTC/Fixed/Hourly              |
| Basic Salary                      | Currency | Yes         | Base salary amount            |
| HRA                               | Currency | No          | House Rent Allowance          |
| Other Allowance                   | Currency | No          | Additional allowances         |
| Incentive                         | Currency | No          | Performance incentive         |
| Deductions                        | Currency | No          | Standard deductions           |
| PF Applicable                     | Checkbox | No          | Provident Fund applicability  |
| ESI Applicable                    | Checkbox | No          | Employee State Insurance      |
| TDS Applicable                    | Checkbox | No          | Tax Deducted at Source        |
| Holiday Work Incentive Applicable | Checkbox | No          | Enable holiday work incentive |
| Holiday Work Incentive Type       | Dropdown | Conditional | Fixed/Per Day/Per Hour        |
| Holiday Work Incentive Amount     | Currency | Conditional | Amount if enabled             |
| Overtime Applicable               | Checkbox | No          | Enable overtime calculation   |
| Overtime Type                     | Dropdown | Conditional | Per Hour/Per Shift            |
| Overtime Shift Type               | Text     | Conditional | Shift description             |
| Overtime Shift Incentive Amount   | Currency | Conditional | Shift incentive amount        |
| Per Hour Incentive Pay Amount     | Currency | Conditional | Hourly overtime rate          |
| Maximum Overtime Hours Per Month  | Number   | Conditional | Monthly OT limit              |

### Step 2 Validation Rules

| Field                            | Rule                                                      |
| -------------------------------- | --------------------------------------------------------- |
| Salary Type                      | Required, must be CTC/Fixed/Hourly                        |
| Basic Salary                     | Required, numeric, minimum 0                              |
| HRA                              | Optional, numeric, minimum 0                              |
| Other Allowance                  | Optional, numeric, minimum 0                              |
| Incentive                        | Optional, numeric, minimum 0                              |
| Deductions                       | Optional, numeric, minimum 0                              |
| Bank Name                        | Required, minimum 3 characters                            |
| Account Number                   | Required, numeric, 9-18 digits                            |
| IFSC Code                        | Required, format: AAAA0###### (11 characters)             |
| Salary Effective From            | Required, valid date                                      |
| Salary Effective To              | Optional, must be after Salary Effective From             |
| Holiday Work Incentive Type      | Required if Holiday Work Incentive Applicable is checked  |
| Holiday Work Incentive Amount    | Required if Holiday Work Incentive Applicable is checked  |
| Overtime Type                    | Required if Overtime Applicable is checked                |
| Per Hour Incentive Pay Amount    | Required if Overtime Applicable is checked                |
| Maximum Overtime Hours Per Month | Required if Overtime Applicable is checked, max 720 hours |

### Step 3: Leave Details Fields

| Field                    | Type     | Required    | Notes                             |
| ------------------------ | -------- | ----------- | --------------------------------- |
| Casual Leave (CL)        | Number   | No          | Days per year                     |
| Sick Leave (SL)          | Number   | No          | Days per year                     |
| Paid Leave (PL)          | Number   | No          | Days per year                     |
| Annual Leave Allocation  | Number   | No          | Total annual allocation           |
| Carry Forward Allowed    | Checkbox | No          | Enable leave carry forward        |
| Max Carry Forward Days   | Number   | Conditional | Maximum days to carry forward     |
| Leave Approval Authority | Dropdown | Yes         | Role who can approve leaves       |
| Leave Reset Cycle        | Dropdown | Yes         | Yearly/Monthly/Custom(From to TO) |

### Step 3 Validation Rules

| Field                    | Rule                                                |
| ------------------------ | --------------------------------------------------- |
| Casual Leave             | Optional, numeric, minimum 0, max 365               |
| Sick Leave               | Optional, numeric, minimum 0, max 365               |
| Paid Leave               | Optional, numeric, minimum 0, max 365               |
| Annual Leave Allocation  | Optional, numeric, minimum 0, max 365               |
| Carry Forward Allowed    | Checkbox No Enable leave carry forward              |
| Max Carry Forward Days   | Required if Carry Forward Allowed checked, max 365  |
| Leave Approval Authority | Required, must select valid role                    |
| Leave Reset Cycle        | Required, must be Yearly/Monthly/Custom(From to TO) |

---

## 6.3 Edit Configuration Form

```
┌─────────────────────────────────────────────────────────────┐
│              EDIT CONFIGURATION                              │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Same as Add Form but:                               │    │
│  │  • Role Name field is DISABLED                       │    │
│  │  • All other fields editable                         │    │
│  │                                                      │    │
│  │  [◄ PREVIOUS]  [SAVE]  [CANCEL]                      │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Edit Configuration Fields

Same as Add Form with Role Name disabled.

### Validation Rules

Same as Add Form, plus:

- Role Name cannot be changed (disabled field)
- If active users exist using this config, show warning before deactivation

---

# 6.4 View Configuration Details

```
┌──────────────────────────────────────────────────────────────┐
│ ← Back                                                       │
│                                                              │
│  Sales Manager                                               │
│  Salary Type: CTC                                            │
│                                                              │
│  Basic Salary: ₹65,000                                       │
│  Config ID: RHC-00124                                        │
│  Effective: 02/01/2025 – 01/01/2026                          │
│  Created By&date: Admin -01/01/2026                          │
│  Last Modified By&date: Admin -01/01/2026                    │
│  Status: Active                                              │
│                                                              │
│  ───────────────── GENERAL INFORMATION ─────────────────     │
│                                                              │
│  COMPENSATION                                                │
│  ┌───────────────────────────────┬─────────────────────────┐ │
│  │ Basic Salary      : ₹65,000   │ Deductions : ₹5,200     │ │
│  │ HRA               : ₹10,000   │ Salary Effective From   │ │
│  │ Other Allowance   : ₹5,000    │ : 02/01/2025            │ │
│  │ Incentive         : ₹2,000    │ Salary Effective To     │ │
│  │                               │ : 01/01/2026            │ │
│  └───────────────────────────────┴─────────────────────────┘ │
│                                                              │
│  INCENTIVE CONFIGURATION                                     │
│  ┌───────────────────────────────┬─────────────────────────┐ │
│  │ Holiday Work Incentive: Yes   │ Overtime Type: Hourly   │ │
│  │ Incentive Type       : Fixed  │ Overtime Shift Type     │ │
│  │ Incentive Amount     : ₹500   │ : Night Shift           │ │
│  │ Overtime             : Yes    │ Per Hour Pay: ₹150      │ │
│  │                               │ Max OT Hours: 40        │ │
│  └───────────────────────────────┴─────────────────────────┘ │
│                                                              │
│  STATUTORY DEDUCTIONS                                        │
│  • PF Applicable : Yes                                       │
│  • ESIC Applicable : No                                      │
│  • TDS Applicable : Yes                                      │
│                                                              │
│  LEAVE CONFIGURATION                                         │
│  ┌───────────────────────────────┬─────────────────────────┐ │
│  │ Casual Leave (CL) : 12 Days   │ Max Carry Forward: 10   │ │
│  │ Sick Leave (SL)   : 10 Days   │ Leave Approval Role     │ │
│  │ Paid Leave (PL)   : 15 Days   │ : Director              │ │
│  │ Annual Leave      : 37 Days   │ Leave Reset Cycle       │ │
│  │                               │ : Monthly               │ │
│  │                               │ Reset From: 12/06/2024  │ │
│  │                               │ Reset To  : 12/06/2024  │ │
│  └───────────────────────────────┴─────────────────────────┘ │
│                                                              │
│  All fields displayed in READ-ONLY mode                      │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

# View Screen Behavior

| Element         | Behavior                      |
| --------------- | ----------------------------- |
| Back Button     | Returns to Configuration List |
| All Fields      | Read-only                     |
| Status Badge    | Shows Active / Inactive       |
| Currency Fields | Displayed with ₹ format       |
| Date Fields     | Displayed in DD/MM/YYYY       |

---

# Sections in View Screen

| Section                 | Description                       |
| ----------------------- | --------------------------------- |
| Header                  | Role name, salary type, status    |
| Configuration Info      | Config ID, Effective dates        |
| Compensation            | Salary structure                  |
| Incentive Configuration | Incentives and overtime           |
| Statutory Deductions    | PF / ESIC / TDS                   |
| Leave Configuration     | Leave entitlement and reset rules |

---

## Configuration Usage Flow

```
┌─────────────────────────────────────────────────────────────┐
│         HOW CONFIGURATION FLOWS TO USER CREATION             │
│                                                              │
│  1. Admin creates Role Configuration (Module 6)              │
│     └── Saves to Role_Compensation_Configuration table       │
│                                                              │
│  2. Admin creates User (Module 8.6)                          │
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

# 🎯 MODULE 7: BRANCH MANAGEMENT

## Overview

Manages organizational locations with hierarchical structure and employee associations. This module allows Company Admin to manage branches used for operations, employee allocation, service execution, and reporting.

**Module Connections:**

- Referenced by: Module 8 (User Management - Branch assignment)
- Used in: Module 2 (Subscription - Branch count configuration)
- Linked to: Module 8.5 (View Branch Employees)

---

# 7.1 Branch List Screen

```
┌─────────────────────────────────────────────────────────────┐
│     BRANCH MANAGEMENT (Client Side)                          │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Filters:                                            │    │
│  │  Search: [____________] (ID/Name/Code/City/State)   │    │
│  │  Status: [▼ All/Active/Inactive ▼]                  │    │
│  │  City: [▼ Dropdown ▼]  State: [▼ Dropdown ▼]        │    │
│  │  Branch Code: [___]                                 │    │
│  │                                                      │    │
│  │  [RESET FILTERS]                                     │    │
│  │                                                      │    │
│  │  [+ ADD BRANCH]                                      │    │
│  │                                                      │    │
│  │  BRANCH LIST TABLE:                                  │    │
│  │  ┌─────────────────────────────────────────────────────────────┐
│  │  │Branch ID│3-Code│Branch Name│Contact Info│Location│City│State│Status│Action│
│  │  │─────────┼──────┼───────────┼────────────┼────────┼────┼─────┼──────┼──────│
│  │  │B001     │NTH   │North Br.  │98xxxx /    │Street  │Delhi│DL  │Active│View  │
│  │  │         │      │           │mail@ex.com │Address │    │     │      │Edit  │
│  │  │B002     │STH   │South Br.  │99xxxx /    │Street  │Mumbai│MH│Active│Delete│
│  │  │         │      │           │mail@ex.com │Address │     │    │      │      │
│  │  └─────────────────────────────────────────────────────────────┘
│  │                                                      │
│  └─────────────────────────────────────────────────────┘
└─────────────────────────────────────────────────────────────┘
```

# Branch List Table Fields

| Field                   | Type    | Required | Notes                             |
| ----------------------- | ------- | -------- | --------------------------------- |
| Branch ID               | Text    | Auto     | Unique system generated branch ID |
| 3 Letter Code           | Text    | Yes      | Unique short code for branch      |
| Branch Name             | Text    | Yes      | Name of branch                    |
| Contact Info            | Text    | Yes      | Email and Mobile number           |
| Location (Full Address) | Text    | Yes      | Full branch address               |
| City                    | Text    | Yes      | Branch city                       |
| State                   | Text    | Yes      | Branch state                      |
| Status                  | Badge   | Auto     | Active / Inactive                 |
| Action                  | Buttons | -        | View / Edit / Delete              |

---

# Table Actions

| Action | Description                            |
| ------ | -------------------------------------- |
| View   | Opens branch details in read-only mode |
| Edit   | Opens edit branch screen               |
| Delete | Soft deletes branch after confirmation |

---

# Filters / Search Fields

| Field              | Type     | Required | Notes                                        |
| ------------------ | -------- | -------- | -------------------------------------------- |
| Search             | Text     | No       | Search by Branch ID, Name, Code              |
| Status Filter      | Dropdown | No       | All / Active / Inactive                      |
| City Filter        | Dropdown | No       | Populated from branch records                |
| State Filter       | Dropdown | No       | Populated from branch records                |

---

## 7.2 Add Branch Screen

```
┌─────────────────────────────────────────────────────────────┐
│              ADD BRANCH SCREEN                               │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Branch Name:              [____________] *         │    │
│  │  3 Letter Code:            [___] *                  │    │
│  │  Email:                    [____________] *         │    │
│  │  Phone Number:             [____________] *         │    │
│  │                                                      │    │
│  │  Address:                                              │    │
│  │  • Address Line 1:         [____________] *         │    │
│  │  • Country:                [____________] *         │    │
│  │  • State:                  [____________] *         │    │
│  │  • City:                   [____________] *         │    │
│  │  • Pincode:                [____________] *         │    │
│  │                                                      │    │
│  │  Branch Type:              [▼ Dropdown ▼] *         │    │
│  │    • HEAD_OFFICE                                     │    │
│  │    • STATE_BRANCH                                    │    │
│  │    • CITY_BRANCH                                     │    │
│  │    • WAREHOUSE                                       │    │
│  │                                                      │    │
│  │  Created By:               [Auto-filled]            │    │
│  │                                                      │    │
│  │  [SAVE]  [CANCEL]                                    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Add Branch Fields

| Field          | Type     | Required | Notes                                          |
| -------------- | -------- | -------- | ---------------------------------------------- |
| Branch Name    | Text     | Yes      | Display name                                   |
| 3 Letter Code  | Text     | Yes      | Unique short code                              |
| Email          | Email    | Yes      | Branch email address                           |
| Phone Number   | Phone    | Yes      | Branch contact number                          |
| Address Line 1 | Text     | Yes      | Street address                                 |
| Country        | Text     | Yes      | Country name                                   |
| State          | Text     | Yes      | State name                                     |
| City           | Text     | Yes      | City name                                      |
| Pincode        | Text     | Yes      | Postal code                                    |
| Branch Type    | Dropdown | Yes      | HEAD_OFFICE/STATE_BRANCH/CITY_BRANCH/WAREHOUSE |
| Created By     | Auto     | System   | Current user                                   |

### Validation Rules

| Field          | Rule                                                  |
| -------------- | ----------------------------------------------------- |
| Branch Name    | Required, minimum 3 characters, unique within company |
| 3 Letter Code  | Required, exactly 3 characters, alphanumeric, unique  |
| Email          | Required, valid email format, unique within company   |
| Phone Number   | Required, exactly 10 digits, numeric only             |
| Address Line 1 | Required, minimum 5 characters                        |
| Country        | Required, minimum 2 characters                        |
| State          | Required, minimum 2 characters                        |
| City           | Required, minimum 2 characters                        |
| Pincode        | Required, 6 digits for India, alphanumeric for others |
| Branch Type    | Required, must be valid option                        |

---

## 7.3 Edit Branch Screen

```
┌─────────────────────────────────────────────────────────────┐
│              EDIT BRANCH SCREEN                              │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Same fields as Add Branch                           │    │
│  │                                                      │    │
│  │  Change:                                             │    │
│  │  • Replace Created By with Edited By (Auto-filled)  │    │
│  │                                                      │    │
│  │  [SAVE]  [CANCEL]                                    │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Edit Branch Fields

Same as Add Branch with Edited By instead of Created By.

1. Status - required -active/inactive dropdown
2. Edited by instead created by of add screen.

### Validation Rules

Same as Add Branch, plus:

- 3 Letter Code cannot be changed if branch has active employees
- Branch Name uniqueness check excludes current branch

---

## 7.4 Delete Branch (Soft Delete)

```
┌─────────────────────────────────────────────────────────────┐
│              DELETE BRANCH CONFIRMATION                      │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                                                     │    │
│  │              ⚠️ DEACTIVATE BRANCH?                 │    │
│  │                                                     │    │
│  │  "This branch will be marked as inactive.          │    │
│  │   Existing records will remain unchanged."         │    │
│  │                                                     │    │
│  │  [CANCEL]              [DEACTIVATE]                 │    │
│  │                                                     │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  BEHAVIOR:                                                   │
│  • On Delete → Show confirmation popup                       │
│  • Branch status changes to Inactive                         │
│  • Branch remains visible in records/history                 │
│  • Cannot be used for new assignments                        │
│  • Existing employee assignments remain                      │
└─────────────────────────────────────────────────────────────┘
```

---

Based on your UI screenshots, it is better to **split section 7.5 into two separate subsections**:

- **7.5.1 Branch Information (Tab 1)**
- **7.5.2 Branch Users / Employees (Tab 2)**

This matches the **actual UI tabs** and keeps the documentation clean.

Below is the **correctly structured version based on your images.**

---

# 7.5 View Particular Branch Details

---

# 7.5.1 Branch Information (Tab 1)

```
┌─────────────────────────────────────────────────────────────┐
│ ← Back                                                      │
│                                                             │
│  PestGuard Solutions (HSR)                                  │
│  Branch Name: PestGuard Solutions (HSR)                     │
│  Branch ID: AD-560096                                       │
│  Email: suraj@gmail.com                                     │
│  Status: ● Active                                           │
│                                                             │
│  [TAB 1: Branch Information]   [TAB 2: Branch User's]        │
│                                                             │
│  ─────────────── BRANCH INFORMATION ─────────────────────   │
│                                                             │
│  3 Letter Code       : 214                                   │
│  Address Details     : Flat 301, Sunshine Apartments         │
│                        27th Main, HSR Layout, Sector 1       │
│  City                : Bangalore                             │
│  State               : Karnataka                             │
│  Contact Number      : +91 8876523456                        │
│  Branch Type         : Service                               │
│                                                             │
│  Created By          : Suraj Kumar on 26th Jan 2026          │
│  Edited By           : Suraj Kumar on 26th Jan 2026          │
│                                                             │
│  All fields displayed in READ-ONLY mode                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Branch Information Fields

| Field           | Type     | Notes                        |
| --------------- | -------- | ---------------------------- |
| Branch ID       | Text     | System generated unique ID   |
| Branch Name     | Text     | Name of the branch           |
| 3 Letter Code   | Text     | Unique short code            |
| Address Details | Text     | Full branch address          |
| City            | Text     | Branch city                  |
| State           | Text     | Branch state                 |
| Contact Number  | Phone    | Primary branch contact       |
| Branch Type     | Dropdown | Service / Warehouse / Office |
| Status          | Badge    | Active / Inactive            |
| Created By      | Text     | User who created branch      |
| Edited By       | Text     | Last modified user           |

---

Good catch 👍 — your **UI table contains more columns** than the documentation.
Below is the **corrected version of 7.5.2 with all missing fields added** based on your reference.

---

# 7.5.2 Branch Users / Employees (Tab 2)

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                          BRANCH USERS / EMPLOYEES                             │
│                                                                              │
│  Search: [________________________]                 [Filters]                 │
│                                                                              │
│ ┌──────────────────────────────────────────────────────────────────────────┐ │
│ │Emp ID │Employee Name│Email ID │Contact No│Designation│Department│Role │ │
│ │       │             │         │          │           │          │     │ │
│ │Branch │Reporting Manager│Status│Created Date│Action(View)                │ │
│ │───────┼─────────────┼─────────┼──────────┼───────────┼──────────┼──────│ │
│ │EMP001 │Suraj Sharma │suraj@.. │+91..     │Manager    │Operations│Admin │ │
│ │HSR    │Amit Verma   │Active   │12-Jan-26 │[View]                    │ │
│ │EMP002 │Anita Singh  │anita@.. │+91..     │Executive  │Sales     │User  │ │
│ │HSR    │Rohit Shah   │Inactive │11-Jan-26 │[View]                    │ │
│ │EMP003 │Rahul Patel  │rahul@.. │+91..     │Technician │Service   │Staff │ │
│ │HSR    │Suraj Sharma │Active   │10-Jan-26 │[View]                    │ │
│ └──────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
│ Pagination:  Previous   1   2   3   ...   10   Next                          │
│                                                                              │
│ • Click Employee Name → Opens Employee Detail Screen                         │
│ • Search supported across Name / Email / Contact / Employee ID               │
│ • Filters available for Status / Role / Department                           │
│ • Empty State: "No employees assigned to this branch."                       │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

# Branch Employees Table Fields

| Field             | Type   | Notes                         |
| ----------------- | ------ | ----------------------------- |
| Emp ID            | Text   | Unique employee ID            |
| Employee Name     | Text   | Full name of employee         |
| Email ID          | Email  | Employee email                |
| Contact Number    | Phone  | Mobile number                 |
| Designation       | Text   | Job title                     |
| Department        | Text   | Department name               |
| Role              | Text   | System role (RBAC role)       |
| Branch            | Text   | Assigned branch               |
| Reporting Manager | Text   | Manager name                  |
| Status            | Badge  | Active / Inactive             |
| Created Date      | Date   | Employee record creation date |
| Action            | Button | View employee details         |

---

# Filters

| Filter             | Type                | Required | Description                                |
| ------------------ | ------------------- | -------- | ------------------------------------------ |
| Branch             | Multi-select        | No       | Filter employees by assigned branch        |
| Department         | Dropdown            | No       | Filter by department                       |
| Designation        | Dropdown            | No       | Filter by designation                      |
| Role               | Dropdown            | No       | Filter by system role                      |
| Status             | Dropdown            | No       | Active / Inactive                          |
| Created Date Range | Date Range          | No       | Filter employees created within date range |

---

# Search

| Field         | Type | Description                                        |
| ------------- | ---- | -------------------------------------------------- |
| Global Search | Text | Search by Employee ID, Name, Email, Contact Number |

---

# 🎯 MODULE 8: EMPLOYEE (USERS) MANAGEMENT

## Overview

Manages the complete employee lifecycle including user visibility, hiring requests, employee creation, role-based permissions, salary configuration, leave setup, and employee records management. Supports a controlled hiring workflow where lower-level roles request new employees, while higher authorities review, approve, and convert requests into active system users.

**Module Connections:**

- Uses: Module 5 (Roles), Module 6 (Salary/Leave Config), Module 7 (Branches)
- Provides users to: Module 7.6 (Branch Employees), Module 5.6 (Role User Count)
- Role-based tabs: Different visibility based on user level

---

## 8 Tab Visibility Control Flow

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
│    │    ✓    │         │    ✓    │        │Optional │       │
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

# 8.1 Tab 1: Employee (User) List

```
┌────────────────────────────────────────────────────────────────────────────────────┐
│                           TAB 1: EMPLOYEE LIST                                      │
│                                                                                    │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                      │  │
│  │ Branch: [☑ Multi-select ▼]        Department: [▼ Dropdown ▼]                 │  │
│  │ Designation: [▼ Dropdown ▼]       Role: [▼ Dropdown ▼]                        │  │
│  │ Reporting Manager: [🔍 Searchable ▼]                                         │  │
│  │ Status: [▼ All / Active / Inactive ▼]                                        │  │
│  │ Created Date Range: [📅 From] - [📅 To]                                      │  │
│  │                                                                              │  │
│  │ Search: [_____________________________]                                      │  │
│  │ (Search by EMP ID / Name / Email / Contact Number)                           │  │
│  │                                                                              │  │
│  │ [RESET FILTERS]                     [+ ADD USER]                              │  │
│  │                                                                              │  │
│  │ EMPLOYEE TABLE                                                               │  │
│  │ ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ │Emp ID│Employee Name│Email ID│Contact No│Designation│Department│Role    │ │
│  │ │──────┼─────────────┼────────┼──────────┼───────────┼──────────┼────────│ │
│  │ │EMP01 │John Doe     │j@..    │9876....  │Manager    │Sales     │Admin   │ │
│  │ │EMP02 │Jane Smith   │js@..   │9987....  │Technician │Operations│Staff   │ │
│  │ │EMP03 │Rahul Patel  │rp@..   │9765....  │Executive  │Sales     │User    │ │
│  │ └─────────────────────────────────────────────────────────────────────────┘ │
│                                                                                │
│  ┌───────────────────────────────────────────────────────────────────────────┐ │
│  │Branch│Reporting Manager│Status│Created Date│Action                        │ │
│  │──────┼─────────────────┼──────┼────────────┼──────────────────────────────│ │
│  │HSR   │Suraj Sharma     │Active│24-Jan-2026 │[View] [Edit] [Delete]        │ │
│  │BTM   │Rohit Mehta      │Active│21-Jan-2026 │[View] [Edit] [Delete]        │ │
│  │Indira│Anil Kumar       │Inactive│20-Jan-26 │[View] [Edit] [Delete]        │ │
│  └───────────────────────────────────────────────────────────────────────────┘ │
│                                                                                │
│ Pagination:  Previous   1   2   3   ...   10   Next                            │
│                                                                                │
└────────────────────────────────────────────────────────────────────────────────┘
```

# Employee Table Fields

| Field             | Type    | Notes                      |
| ----------------- | ------- | -------------------------- |
| Emp ID            | Text    | Unique employee identifier |
| Employee Name     | Text    | Full name                  |
| Email ID          | Email   | Employee email             |
| Contact Number    | Phone   | Employee mobile            |
| Designation       | Text    | Job title                  |
| Department        | Text    | Employee department        |
| Role              | Text    | System role assigned       |
| Branch            | Text    | Assigned branch            |
| Reporting Manager | Text    | Direct reporting manager   |
| Status            | Badge   | Active / Inactive          |
| Created Date      | Date    | Employee creation date     |
| Action            | Buttons | View / Edit / Delete       |

---

# Table Actions

| Action | Description                              |
| ------ | ---------------------------------------- |
| View   | Opens Employee Detail Screen             |
| Edit   | Opens Employee Edit Screen               |
| Delete | Soft deletes employee after confirmation |

---

---

# Filters

| Filter             | Type                | Required | Description                                |
| ------------------ | ------------------- | -------- | ------------------------------------------ |
| Branch             | Multi-select        | No       | Filter employees by assigned branch        |
| Department         | Dropdown            | No       | Filter by department                       |
| Designation        | Dropdown            | No       | Filter by designation                      |
| Role               | Dropdown            | No       | Filter by system role                      |
| Status             | Dropdown            | No       | Active / Inactive                          |
| Created Date Range | Date Range          | No       | Filter employees created within date range |

---

# Search

| Field         | Type | Description                                        |
| ------------- | ---- | -------------------------------------------------- |
| Global Search | Text | Search by Employee ID, Name, Email, Contact Number |

---

# System Behaviour

| Event         | System Response                         |
| ------------- | --------------------------------------- |
| Apply Filters | Table refreshes with filtered results   |
| Search        | Filters records based on search keyword |
| Add User      | Opens Add Employee screen               |
| Delete        | Confirmation popup before removal       |

---

# 8.2 Tab 2: My Hiring Requests (Table View)

```
┌──────────────────────────────────────────────────────────────────────────────────┐
│                       TAB 2: MY HIRING REQUESTS                                   │
│             (For requesters to track their submitted requests)                   │
│                                                                                  │
│  ┌────────────────────────────────────────────────────────────────────────────┐  │
│  │ Filters                                                                    │  │
│  │ Status: [▼ All / Pending / Approved / Rejected / Converted ▼]             │  │
│  │ Department: [▼ Dropdown ▼]        Proposed Role: [▼ Dropdown ▼]            │  │
│  │ Branch: [☑ Multi-select ▼]                                                │  │
│  │ Expected Joining Date: [📅 From] - [📅 To]                                 │  │
│  │ Submitted Date: [📅 From] - [📅 To]                                        │  │
│  │ Last Updated Date: [📅 From] - [📅 To]                                     │  │
│  │                                                                            │  │
│  │ Search: [________________________]                                         │  │
│  │ (Search by Request ID / Department / Role / Hiring Reason)                 │  │
│  │                                                                            │  │
│  │ [RESET FILTERS]                 [+ NEW HIRING REQUEST]                     │  │
│  │                                                                            │  │
│  │ HIRING REQUESTS TABLE                                                      │  │
│  │ ┌────────────────────────────────────────────────────────────────────────┐ │
│  │ │Req ID│Department│Proposed Role│Branch│Positions│Expected Joining Date  │ │
│  │ │──────┼──────────┼─────────────┼──────┼─────────┼──────────────────────│ │
│  │ │HRQ001│Sales     │Sales Person │HSR   │2        │12-Feb-2026            │ │
│  │ │HRQ002│Operations│Technician   │BTM   │1        │05-Mar-2026            │ │
│  │ │HRQ003│HR        │HR Executive │Indira│1        │15-Feb-2026            │ │
│  │ └────────────────────────────────────────────────────────────────────────┘ │
│                                                                               │
│ ┌───────────────────────────────────────────────────────────────────────────┐ │
│ │Hiring Reason│Status│Sent To│Submitted Date│Last Updated Date│Action       │ │
│ │─────────────┼──────┼───────┼──────────────┼─────────────────┼────────────│ │
│ │Replacement  │Pending│HR Head│01-Jan-2026   │02-Jan-2026      │[View]      │ │
│ │New Project  │Approved│HR Team│05-Jan-2026  │07-Jan-2026      │[View]      │ │
│ │Expansion    │Rejected│Director│08-Jan-2026 │09-Jan-2026      │[View]      │ │
│ └───────────────────────────────────────────────────────────────────────────┘ │
│                                                                               │
│ Pagination:  Previous   1   2   3   ...   10   Next                           │
│                                                                               │
└────────────────────────────────────────────────────────────────────────────────┘
```

# Hiring Requests Table Fields

| Field                 | Type   | Notes                                     |
| --------------------- | ------ | ----------------------------------------- |
| Request ID            | Text   | Unique hiring request ID                  |
| Department            | Text   | Department requesting hiring              |
| Proposed Role         | Text   | Role requested                            |
| Branch                | Text   | Branch for which hiring is requested      |
| Positions             | Number | Number of positions requested             |
| Expected Joining Date | Date   | Expected start date                       |
| Hiring Reason         | Text   | Reason for hiring                         |
| Status                | Badge  | Pending / Approved / Rejected / Converted |
| Sent To               | Text   | Approver / HR reviewer                    |
| Submitted Date        | Date   | Request creation date                     |
| Last Updated Date     | Date   | Last modification date                    |
| Action                | Button | View hiring request                       |

---

# Table Actions

| Action             | Description                          |
| ------------------ | ------------------------------------ |
| View               | Opens detailed hiring request screen |
| New Hiring Request | Opens hiring request form            |

---

# Filters

| Filter                | Type         | Required | Description                               |
| --------------------- | ------------ | -------- | ----------------------------------------- |
| Status                | Dropdown     | No       | Pending / Approved / Rejected / Converted |
| Department            | Dropdown     | No       | Filter by department                      |
| Proposed Role         | Dropdown     | No       | Filter by role                            |
| Branch                | Multi-select | No       | Filter by requested branch                |
| Expected Joining Date | Date Range   | No       | Filter by expected joining date           |
| Submitted Date        | Date Range   | No       | Filter by request submission date         |

---

# Search

| Field         | Type | Description                                                    |
| ------------- | ---- | -------------------------------------------------------------- |
| Global Search | Text | Search by Request ID, Department, Proposed Role, Hiring Reason |

---

# System Behaviour

| Event              | System Response                        |
| ------------------ | -------------------------------------- |
| Apply Filters      | Table refreshes with filtered requests |
| Search             | Filters records based on keyword       |
| View               | Opens request details page             |
| New Hiring Request | Opens hiring request creation screen   |

### 8.2.1 My Hiring Request View (Popup)

```
┌───────────────────────────────────────────────────────────────┐
│                    MY HIRING REQUEST DETAILS                   │
│ Request ID: HRQ001                                  [X Close] │
│                                                               │
│ Status: ● Pending                                            │
│                                                               │
│ ───────────────── REQUEST INFORMATION ─────────────────       │
│ Requested By            : Raj Mehta                          │
│ Department              : Sales                              │
│ Designation             : Team Lead                          │
│ Proposed Role           : Sales Executive                    │
│ Branch Name             : Bangalore - HSR                    │
│ Employment Type         : Permanent                          │
│ Expected Date of Joining: 12-Feb-2026                        │
│ Number of Positions     : 2                                  │
│ Proposed Salary         : ₹50,000                            │
│ Hiring Reason           : Replacement for resigned employee  │
│                                                               │
│ ───────────────── REVIEW INFORMATION ─────────────────        │
│ Status                 : Pending                             │
│ Reviewed By            : —                                   │
│ Review Date            : —                                   │
│                                                               │
│ IF REJECTED:                                                 │
│ Rejection Reason       : —                                   │
│                                                               │
│ ───────────────── SUBMISSION INFORMATION ─────────────        │
│ Sent To                : HR Manager                          │
│ Submitted Date         : 02-Jan-2026                         │
│ Last Updated           : 03-Jan-2026                         │
│                                                               │
│                           [Close]                             │
└───────────────────────────────────────────────────────────────┘
```

---

# Request Details Fields (Table)

| Field                    | Type         | Description                           |
| ------------------------ | ------------ | ------------------------------------- |
| Request ID               | Text         | Unique hiring request identifier      |
| Status                   | Status Badge | Pending / Approved / Rejected         |
| Reviewed By              | Text         | Name of reviewer if approved/rejected |
| Review Date              | Date         | Date of review                        |
| Requested By             | Text         | Employee who created request          |
| Department               | Text         | Department requesting hiring          |
| Designation              | Text         | Requester designation                 |
| Proposed Role            | Text         | Role requested                        |
| Branch Name              | Text         | Branch where employee will work       |
| Employment Type          | Dropdown     | Permanent / Contract / Intern         |
| Expected Date of Joining | Date         | Expected joining date                 |
| Number of Positions      | Number       | Total required employees              |
| Proposed Salary          | Currency     | Proposed salary amount                |
| Hiring Reason            | Long Text    | Reason for hiring                     |
| Rejection Reason         | Long Text    | Reason if rejected                    |
| Sent To                  | Text         | Reviewer or HR responsible            |
| Submitted Date           | Date         | Date request was submitted            |
| Last Updated             | Date         | Last update timestamp                 |

---

# 8.3 Tab 3: Received Requests to Add New Employee

```
┌──────────────────────────────────────────────────────────────────────────────────┐
│                   TAB 3: RECEIVED REQUESTS                                        │
│           (For admins / HR to review hiring requests)                             │
│                                                                                   │
│  ┌────────────────────────────────────────────────────────────────────────────┐   │
│  │ Filters                                                                    │   │
│  │ Status: [▼ All / Pending / Approved / Rejected ▼]                          │   │
│  │ Department: [▼ Dropdown ▼]         Proposed Role: [▼ Dropdown ▼]            │   │
│  │ Branch: [▼ Dropdown ▼]             Requested By: [▼ Dropdown ▼]             │   │
│  │ Employment Type: [▼ Permanent / Contract / Intern ▼]                        │   │
│  │ Expected Joining Date: [📅 From] - [📅 To]                                   │   │
│  │ Submitted Date: [📅 From] - [📅 To]                                          │   │
│  │ Review Date: [📅 From] - [📅 To]                                             │   │
│  │                                                                              │   │
│  │ Search: [____________________________]                                       │   │
│  │ (Search by Request ID / Requested By / Department / Role)                   │   │
│  │                                                                              │   │
│  │ [RESET FILTERS]                                                             │   │
│  │                                                                              │   │
│  │ RECEIVED REQUESTS TABLE                                                     │   │
│  │ ┌────────────────────────────────────────────────────────────────────────┐ │
│  │ │Request│Requested│Department│Proposed│Branch│Positions│Employment Type  │ │
│  │ │ID     │By       │          │Role    │      │         │                 │ │
│  │ │───────┼─────────┼──────────┼────────┼──────┼─────────┼────────────────│ │
│  │ │HRQ001 │Raj Mehta│Sales     │Sales   │HSR   │2        │Permanent       │ │
│  │ │HRQ002 │Anita Rao│Operations│Technician│BTM│1        │Contract        │ │
│  │ │HRQ003 │Rohit Shah│HR       │HR Exec │Indira│1       │Permanent       │ │
│  │ └────────────────────────────────────────────────────────────────────────┘ │
│                                                                                │
│ ┌────────────────────────────────────────────────────────────────────────────┐ │
│ │Expected Joining│Status│Submitted Date│Reviewed By│Review Date│Action       │ │
│ │Date            │      │              │           │           │             │ │
│ │───────────────┼──────┼──────────────┼───────────┼───────────┼────────────│ │
│ │12-Feb-2026    │Pending│02-Jan-2026  │—          │—          │[View]      │ │
│ │05-Mar-2026    │Approved│04-Jan-2026 │HR Head    │06-Jan-26  │[View]      │ │
│ │20-Feb-2026    │Rejected│05-Jan-2026 │Director   │07-Jan-26  │[View]      │ │
│ └────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                │
│ Pagination:  Previous   1   2   3   ...   10   Next                            │
│                                                                                │
└────────────────────────────────────────────────────────────────────────────────┘
```

# Received Requests Table Fields

| Field                 | Type   | Notes                                |
| --------------------- | ------ | ------------------------------------ |
| Request ID            | Text   | Unique hiring request ID             |
| Requested By          | Text   | User who submitted the request       |
| Department            | Text   | Department requesting hiring         |
| Proposed Role         | Text   | Requested role                       |
| Branch                | Text   | Branch for which hiring is requested |
| Positions             | Number | Number of employees required         |
| Employment Type       | Text   | Permanent / Contract / Intern        |
| Expected Joining Date | Date   | Expected joining date                |
| Status                | Badge  | Pending / Approved / Rejected        |
| Submitted Date        | Date   | Request submission date              |
| Reviewed By           | Text   | Approver or reviewer                 |
| Review Date           | Date   | Date of review decision              |
| Action                | Button | View request details                 |

---

# Table Actions

| Action | Description                        |
| ------ | ---------------------------------- |
| View   | Opens Hiring Request Detail Screen |

---

# Filters

| Filter                | Type       | Required | Description                              |
| --------------------- | ---------- | -------- | ---------------------------------------- |
| Status                | Dropdown   | No       | Pending / Approved / Rejected            |
| Department            | Dropdown   | No       | Filter by department                     |
| Proposed Role         | Dropdown   | No       | Filter by requested role                 |
| Branch                | Dropdown   | No       | Filter by requested branch               |
| Requested By          | Dropdown   | No       | Filter by employee who submitted request |
| Submitted Date        | Date Range | No       | Filter by request submission date        |

---

# Search

| Field         | Type | Description                                          |
| ------------- | ---- | ---------------------------------------------------- |
| Global Search | Text | Search by Request ID, Requested By, Department, Role |

---

## 8.4 Request Form Fields(8.2 refer for open this)

```
┌─────────────────────────────────────────────────────────────┐
│  NEW HIRING REQUEST FORM                                     │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  BASIC INFORMATION                                   │    │
│  │  ─────────────────                                   │    │
│  │  Requested By:           [Auto: Current User]       │    │
│  │  Department:             [____________] *           │    │
│  │  Designation:            [____________] *           │    │
│  │  Proposed Role:          [▼ Role ▼] *               │    │
│  │  Branch:                 [☑ Multi-select] *         │    │
│  │  Employment Type:        [▼ Perm/Contract/Int ▼] *  │    │
│  │  Expected Date of Joining: [📅 ________] *          │    │
│  │  Number of Positions:    [____] *                   │    │
│  │                                                      │    │
│  │  ADDITIONAL INFORMATION                              │    │
│  │  ──────────────────────                              │    │
│  │  Hiring Reason:          [____________________] *   │    │
│  │                          [Textarea]                 │    │
│  │  Job Description:        [____________________]     │    │
│  │                          [Textarea]                 │    │
│  │  Additional Remarks:     [____________________]     │    │
│  │                          [Textarea]                 │    │
│  │  Supporting Document:    [📎 Upload ]               │    │
│  │                                                      │    │
│  │  [SUBMIT REQUEST]  [CANCEL]                          │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  POPUP: SELECT RECIPIENTS                                    │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Send to specific person(s):                         │    │
│  │  ☑ All (Default checked)                            │    │
│  │  ☐ Raj Kumar (Admin)                                │    │
│  │  ☐ Mike Smith (HR Manager)                          │    │
│  │  ☐ Sarah Jones (Operations Head)                    │    │
│  │                                                      │    │
│  │  [CONFIRM SEND]  [CANCEL]                            │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Request Form Fields

| Field               | Type         | Required | Notes                      |
| ------------------- | ------------ | -------- | -------------------------- |
| Requested By        | Auto         | System   | Logged-in user             |
| Department          | Text         | Yes      | Department name            |
| Designation         | Text         | Yes      | Job designation            |
| Proposed Role       | Dropdown     | Yes      | Select from existing roles |
| Branch              | Multi-select | Yes      | Select branch(es)          |
| Employment Type     | Dropdown     | Yes      | Permanent/Contract/Intern  |
| Expected DOJ        | Date         | Yes      | Expected joining date      |
| Number of Positions | Number       | Yes      | Positions to fill          |
| Hiring Reason       | Textarea     | Yes      | Why this hire is needed    |
| Job Description     | Textarea     | No       | Role description           |
| Additional Remarks  | Textarea     | No       | Any additional info        |
| Supporting Document | File Upload  | No       | JD, approval doc, etc.     |

### 8.4.2 after submit opens pop up whom to send this request (check above diagram )

### Validation Rules

| Field               | Rule                                           |
| ------------------- | ---------------------------------------------- |
| Department          | Required, minimum 2 characters                 |
| Designation         | Required, minimum 2 characters                 |
| Proposed Role       | Required, must select valid role               |
| Branch              | Required, at least one branch must be selected |
| Employment Type     | Required, must be Permanent/Contract/Intern    |
| Expected DOJ        | Required, must be future date                  |
| Number of Positions | Required, minimum 1, maximum 100               |
| Hiring Reason       | Required, minimum 20 characters                |
| Job Description     | Optional, max 1000 characters                  |
| Additional Remarks  | Optional, max 500 characters                   |
| Supporting Document | Optional, max 10MB, PDF/DOC/DOCX allowed       |

---

Based on the **reference UI in your image**, the popup structure is slightly different from your current draft. The screen is organized into **clear sections**:

1. **Header + Status**
2. **Basic Information (2-column layout)**
3. **Hiring Reason**
4. **Job Description + Document**
5. **Submission Information**
6. **Actions (Reject / Approve)**

I refactored your **8.5 Request Detail View** to match the UI structure in the screenshot.

---

# 8.5 Request Detail View (Popup)

_(Triggered from Tab 3 → Action: View)_

```
┌───────────────────────────────────────────────────────────────┐
│                     REQUEST DETAILS                            │
│                     Request ID: HRQ001                         │
│                                                [X Close]       │
│                                                               │
│  STATUS:  ● Pending                                           │
│                                                               │
│ ───────────────── BASIC INFORMATION ─────────────────         │
│                                                               │
│ Requested By        : [Employee Name]                         │
│ Department          : [Department Name]                       │
│ Designation         : [Requester Designation]                 │
│ Proposed Role       : [Requested Role]                        │
│ Branch              : [Branch Name(s)]                        │
│ Employment Type     : [Permanent / Contract / Intern]         │
│ Expected Date of Joining : [Date]                             │
│ Number of Positions : [Value]                                 │
│ Proposed Salary     : [Amount]                                │
│                                                               │
│ ───────────────── HIRING REASON ─────────────────              │
│ [Full reason text explaining why hiring is required]          │
│                                                               │
│ ───────────────── JOB DESCRIPTION ────────────────             │
│ [Detailed job description text]                                │
│                                                               │
│ Supporting Document                                           │
│ [📄 Document.pdf]  Uploaded on: [Date]                         │
│ [Download]                                                    │
│                                                               │
│ ───────────────── SUBMISSION INFORMATION ─────────             │
│ Sent To              : [Approver Role/User]                   │
│ Submitted Date       : [Date]                                 │
│ Last Updated         : [Date]                                 │
│                                                               │
│                                                               │
│ IF REJECTED:                                                 │
│ Rejection Reason     : [Full rejection text]                  │
│                                                               │
│                                                               │
│                          ACTIONS                              │
│             [ Reject ]            [ Approve ]                 │
│                                                               │
└───────────────────────────────────────────────────────────────┘
```

---

### 8.5.2 Reject Request Popup (Triggered from Reject Button)

```
┌───────────────────────────────────────────────┐
│                Reject Request                  │
│                                               │
│ Rejection Reason *                            │
│ ┌───────────────────────────────────────────┐ │
│ │ Please provide reason for rejection...    │ │
│ │                                           │ │
│ │                                           │ │
│ └───────────────────────────────────────────┘ │
│                                               │
│                 [Cancel]   [Reject Request]   │
└───────────────────────────────────────────────┘
```

---

# Request Detail Fields

| Field               | Type        | Description                           |
| ------------------- | ----------- | ------------------------------------- |
| Request ID          | Text        | Unique hiring request ID              |
| Requested By        | Text        | Employee who created request          |
| Department          | Text        | Department requesting hiring          |
| Designation         | Text        | Requester designation                 |
| Proposed Role       | Text        | Role requested                        |
| Branch              | Multi-value | Branch(es) where employee is required |
| Employment Type     | Dropdown    | Permanent / Contract / Intern         |
| Expected DOJ        | Date        | Expected joining date                 |
| Number of Positions | Number      | Total headcount required              |
| Proposed Salary     | Currency    | Suggested salary for role             |
| Hiring Reason       | Long Text   | Reason for request                    |
| Job Description     | Long Text   | Role responsibilities                 |
| Supporting Document | File        | JD or supporting file                 |
| Sent To             | Text        | Reviewer / approver                   |
| Submitted Date      | Date        | Request creation date                 |
| Last Updated        | Date        | Last modification                     |
| Reviewed By         | Text        | Approver name                         |
| Review Date         | Date        | Date of approval/rejection            |
| Rejection Reason    | Long Text   | Reason if rejected                    |

---

# Actions

| Action  | Behavior              |
| ------- | --------------------- |
| Approve | Status → **Approved** |
| Reject  | Opens rejection popup |
| Close   | Closes popup          |

---

# Validation Rules

| Action  | Rule                                                |
| ------- | --------------------------------------------------- |
| Reject  | Rejection Reason **mandatory (min 20 characters)**  |
| Approve | Reviewer Name **auto-captured from logged-in user** |
| Approve | Review Date **auto-recorded**                       |
| Reject  | Status updated to **Rejected**                      |

---

```
View Request
   ↓
Popup opens
   ↓
Admin reviews details
   ↓
Approve / Reject
   ↓
Status updated
```

## 8.6 Add User Form (Multi-Step)

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
│  │  Password:               [____________] *           │    │
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
│  │  • Address Line 1, Line 2, City, State, Country, Pin│    │
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
│  │  │ ☑ Request → [Multi-select Roles ▼]              ││    │
│  │  │ ☑ Approval Authority                            ││    │
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
│  │  PF/ESI/TDS:             [☑ Checkboxes]             │    │
│  │                                                      │    │
│  │  Bank Name:              [____________] *           │    │
│  │  Account Number:         [____________] *           │    │
│  │  IFSC Code:              [____________] *           │    │
│  │  Salary Effective From:  [📅 ________] *            │    │
│  │  Salary Effective To:    [📅 ________]              │    │
│  │                                                      │    │
│  │  INCENTIVE CONFIGURATION                             │    │
│  │  ☐ Holiday Work Incentive                           │    │
│  │  ☐ Overtime (with all sub-fields)                   │    │
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

## Step 1: Personal Details Fields

| Field             | Type          | Required | Notes                                         |
| ----------------- | ------------- | -------- | --------------------------------------------- |
| EMP ID            | Text          | Yes      | Unique employee ID                            |
| First Name        | Text          | Yes      | Employee first name                           |
| Last Name         | Text          | Yes      | Employee last name                            |
| Email             | Email         | No       | Optional email                                |
| Contact Number    | Phone         | Yes      | Primary contact, 10 digits                    |
| Alternate Number  | Phone         | No       | Secondary contact                             |
| Password          | Password      | Yes      | Auto-generated or manual                      |
| Re-Enter Password | Password      | Yes      | Match with password                           |
| Department        | Text          | Yes      | Department name                               |
| Designation       | Text          | Yes      | Job designation                               |
| Role              | Dropdown      | Yes      | ⭐ Triggers auto-load of config               |
| Branch            | Multi-select  | Yes      | Assign to branch(es)                          |
| Reporting Manager | Dropdown      | Yes      | Filtered by branch + role hierarchy           |
| Employment Type   | Dropdown      | Yes      | Permanent/Contract/Intern                     |
| Date of Joining   | Date          | Yes      | Joining date                                  |
| Status            | Dropdown      | Yes      | Active/Inactive                               |
| Current Address   | Address Block | Yes      | Line 1, Line 2, City, State, Country, Pincode |
| Permanent Address | Address Block | Yes      | Same as current checkbox OR manual            |

### Step 1 Validation Rules

| Field             | Rule                                                             |
| ----------------- | ---------------------------------------------------------------- |
| EMP ID            | Required, unique across company, alphanumeric, 3-20 characters   |
| First Name        | Required, alphabets only, 2-50 characters                        |
| Last Name         | Required, alphabets only, 2-50 characters                        |
| Email             | Optional, valid email format if provided                         |
| Contact Number    | Required, exactly 10 digits, numeric only, unique                |
| Alternate Number  | Optional, exactly 10 digits if provided, different from primary  |
| Password          | Required, min 8 chars, 1 uppercase, 1 number, 1 special char     |
| Department        | Required, min 2 characters                                       |
| Designation       | Required, min 2 characters                                       |
| Role              | Required, must select from active roles                          |
| Branch            | Required, at least one branch must be selected                   |
| Reporting Manager | Required, must be valid user in selected branch with higher role |
| Employment Type   | Required, must be Permanent/Contract/Intern                      |
| Date of Joining   | Required, valid date, not future date                            |
| Status            | Required, Active or Inactive                                     |
| Current Address   | All fields required, pincode 6 digits for India                  |
| Permanent Address | All fields required if not same as current                       |

---

### Step 2: Module Permissions Fields

| Field               | Type            | Required    | Notes                            |
| ------------------- | --------------- | ----------- | -------------------------------- |
| Is Application User | Checkbox        | No          | If checked, disables permissions |
| Module Permissions  | Permission Grid | Conditional | Required if not app user         |

### Step 2 Validation Rules

| Field               | Rule                                                             |
| ------------------- | ---------------------------------------------------------------- |
| Is Application User | Boolean, defaults to false                                       |
| Module Permissions  | If not app user, at least VIEW permission on one module required |
| REQUEST permission  | If enabled, at least one Request Receiver role must be selected  |

---

### Step 3: Salary Details Fields

| Field                             | Type     | Required    | Notes                     |
| --------------------------------- | -------- | ----------- | ------------------------- |
| Salary Type                       | Dropdown | Yes         | CTC/Fixed/Hourly          |
| Basic Salary                      | Currency | Yes         | Base salary amount        |
| HRA                               | Currency | No          | House Rent Allowance      |
| Other Allowance                   | Currency | No          | Additional allowances     |
| Incentive                         | Currency | No          | Performance incentive     |
| Deductions                        | Currency | No          | Standard deductions       |
| PF Applicable                     | Checkbox | No          | Provident Fund            |
| ESI Applicable                    | Checkbox | No          | Employee State Insurance  |
| TDS Applicable                    | Checkbox | No          | Tax Deducted at Source    |
| Bank Name                         | Text     | Yes         | Employee's bank           |
| Account Number                    | Text     | Yes         | Bank account number       |
| IFSC Code                         | Text     | Yes         | Bank IFSC code            |
| Salary Effective From             | Date     | Yes         | Salary validity start     |
| Salary Effective To               | Date     | No          | Salary validity end       |
| Holiday Work Incentive Applicable | Checkbox | No          | Enable holiday incentive  |
| Holiday Work Incentive Type       | Dropdown | Conditional | Fixed/Per Day/Per Hour    |
| Holiday Work Incentive Amount     | Currency | Conditional | Amount if enabled         |
| Overtime Applicable               | Checkbox | No          | Enable overtime           |
| Overtime Type                     | Dropdown | Conditional | Per Hour/Per Shift        |
| Overtime Shift Type               | Text     | Conditional | Shift description         |
| Overtime Shift Incentive Amount   | Currency | Conditional | Shift incentive           |
| Per Hour Incentive Pay Amount     | Currency | Conditional | Hourly OT rate            |
| Maximum Overtime Hours Per Month  | Number   | Conditional | Monthly OT limit, max 720 |

### Step 3 Validation Rules

| Field                         | Rule                                       |
| ----------------------------- | ------------------------------------------ |
| Salary Type                   | Required, must be CTC/Fixed/Hourly         |
| Basic Salary                  | Required, numeric, minimum 0               |
| HRA                           | Optional, numeric, minimum 0               |
| Other Allowance               | Optional, numeric, minimum 0               |
| Incentive                     | Optional, numeric, minimum 0               |
| Deductions                    | Optional, numeric, minimum 0               |
| Bank Name                     | Required, min 3 characters                 |
| Account Number                | Required, numeric, 9-18 digits             |
| IFSC Code                     | Required, format: AAAA0###### (11 chars)   |
| Salary Effective From         | Required, valid date                       |
| Salary Effective To           | Optional, must be after Effective From     |
| Holiday Work Incentive Type   | Required if Holiday Work Incentive checked |
| Holiday Work Incentive Amount | Required if Holiday Work Incentive checked |
| Overtime Type                 | Required if Overtime checked               |
| Per Hour Incentive Pay Amount | Required if Overtime checked               |
| Maximum Overtime Hours        | Required if Overtime checked, max 720      |

---

### Step 4: Leave Details Fields

| Field                    | Type     | Required    | Notes                       |
| ------------------------ | -------- | ----------- | --------------------------- |
| Casual Leave (CL)        | Number   | No          | Days per year               |
| Sick Leave (SL)          | Number   | No          | Days per year               |
| Paid Leave (PL)          | Number   | No          | Days per year               |
| Annual Leave Allocation  | Number   | No          | Total annual allocation     |
| Carry Forward Allowed    | Checkbox | No          | Enable leave carry forward  |
| Max Carry Forward Days   | Number   | Conditional | Max days to carry forward   |
| Leave Approval Authority | Dropdown | Yes         | Role who can approve leaves |
| Leave Reset Cycle        | Dropdown | Yes         | Yearly/Monthly/Custom       |

### Step 4 Validation Rules

| Field                    | Rule                                       |
| ------------------------ | ------------------------------------------ |
| Casual Leave             | Optional, numeric, 0-365                   |
| Sick Leave               | Optional, numeric, 0-365                   |
| Paid Leave               | Optional, numeric, 0-365                   |
| Annual Leave Allocation  | Optional, numeric, 0-365                   |
| Max Carry Forward Days   | Required if Carry Forward checked, max 365 |
| Leave Approval Authority | Required, must select valid role           |
| Leave Reset Cycle        | Required, must be Yearly/Monthly/Custom    |

---

### Step 5: Documents & Additional Data Fields

| Field                    | Type       | Required | Notes                    |
| ------------------------ | ---------- | -------- | ------------------------ |
| Aadhar Number            | Text       | No       | 12-digit Aadhar          |
| PAN Number               | Text       | No       | 10-character PAN         |
| UAN Number               | Text       | No       | Universal Account Number |
| Employee ID Card Number  | Text       | No       | Company ID card          |
| Aadhar Upload            | File       | No       | PDF/JPG/PNG, max 5MB     |
| PAN Upload               | File       | No       | PDF/JPG/PNG, max 5MB     |
| Address Proof Upload     | File       | No       | PDF/JPG/PNG, max 5MB     |
| Educational Certificates | File       | No       | PDF/JPG/PNG, max 10MB    |
| Experience Letter        | File       | No       | PDF/JPG/PNG, max 5MB     |
| Offer Letter             | File       | No       | PDF/JPG/PNG, max 5MB     |
| Appointment Letter       | File       | No       | PDF/JPG/PNG, max 5MB     |
| Other Documents          | File       | No       | PDF/JPG/PNG, max 10MB    |
| Grade / Level            | Text       | No       | Employee grade           |
| Shift Type               | Text       | No       | Work shift               |
| Weekly Off               | Text       | No       | Weekly off day(s)        |
| Target Amount            | Currency   | No       | Sales/target amount      |
| Commission Percentage    | Percentage | No       | Commission %             |
| Employee Photo           | Image      | No       | JPG/PNG, max 2MB         |

### Step 5 Validation Rules

| Field                   | Rule                                               |
| ----------------------- | -------------------------------------------------- |
| Aadhar Number           | Optional, exactly 12 digits if provided            |
| PAN Number              | Optional, format: AAAAA9999A if provided           |
| UAN Number              | Optional, numeric, max 12 digits                   |
| Employee ID Card Number | Optional, alphanumeric                             |
| All Document Uploads    | Optional, max 5MB each (10MB for Education/Others) |
| Employee Photo          | Optional, JPG/PNG, max 2MB                         |
| Commission Percentage   | Optional, 0-100 if provided                        |

---

## 8.7 Edit Employee Form

```
┌─────────────────────────────────────────────────────────────┐
│              EDIT EMPLOYEE                                   │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Same 5-step form as Add User                        │    │
│  │  Pre-filled with existing employee data              │    │
│  │                                                      │    │
│  │  Changes:                                            │    │
│  │  • EMP ID is DISABLED (cannot change)               │    │
│  │  • Password field shows [Keep empty for not change it] option     │    │
│  │  • All other fields editable based on permissions   │    │
│  │                                                      │    │
│  │  [◄ PREVIOUS]  [SAVE CHANGES]  [CANCEL]              │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Edit Employee Fields

Same as Add User with EMP ID disabled and Password as reset option.

### Validation Rules

Same as Add User, plus:

- EMP ID cannot be modified
- Email uniqueness check excludes current employee
- Contact Number uniqueness check excludes current employee

---

## 8.8 View Employee Screen

```
┌─────────────────────────────────────────────────────────────┐
│              VIEW EMPLOYEE DETAILS                           │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  All 5 sections displayed in READ-ONLY mode          │    │
│  │                                                      │    │
│  │  PERSONAL DETAILS                                    │    │
│  │  • All fields displayed as text values               │    │
│  │                                                      │    │
│  │  MODULE PERMISSIONS                                  │    │
│  │  • Grid showing all enabled permissions              │    │
│  │                                                      │    │
│  │  SALARY DETAILS                                      │    │
│  │  • All compensation fields displayed                 │    │
│  │                                                      │    │
│  │  LEAVE DETAILS                                       │    │
│  │  • All leave configuration displayed                 │    │
│  │                                                      │    │
│  │  DOCUMENTS                                           │    │
│  │  • List of uploaded documents with [Download] links  │    │
│  │                                                      │    │
│  │  AUDIT TRAIL                                         │    │
│  │  • Created By: [Name]  Created Date: [Date]         │    │
│  │  • Last Modified By: [Name]  Last Modified: [Date]  │    │
│  │                                                      │    │
│  │  [CLOSE]  [EDIT] (if has permission)                 │    │
│  │                                                      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

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
| package-Type    | Cascading Dropdown | Changes based on Category                                          |
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

# 📊 MASTER FLOWCHART: COMPLETE SYSTEM WORKFLOW

## Overall Module Connections

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         ENTRY POINTS                                     │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │ Super Admin │  │ Company     │  │ Company     │  │ Existing    │     │
│  │ Login       │  │ Admin Signup│  │ Admin Login │  │ User Login  │     │
│  │ (Module 1.2)│  │ (Module 1.3)│  │ (Module 1.5)│  │ (Module 1.6)│     │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘     │
│         │                │                │                │            │
│         ▼                ▼                ▼                ▼            │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │ Module 3    │  │ Module 2    │  │ Check Doc   │  │ Check Role  │     │
│  │ (Super Admin│  │ (Onboarding │  │ Status      │  │ & Route to  │     │
│  │  Dashboard) │  │  Flow)      │  │ (Module 2.4)│  │ Module 8    │     │
│  └─────────────┘  └──────┬──────┘  └──────┬──────┘  └─────────────┘     │
│                          │                │                              │
│                          ▼                ▼                              │
│                   ┌─────────────┐  ┌─────────────┐                       │
│                   │ Module 2.1  │  │ Module 2.5  │                       │
│                   │ (Company    │  │ (Waiting/   │                       │
│                   │  Info)      │  │  Rejected)  │                       │
│                   └──────┬──────┘  └──────┬──────┘                       │
│                          │                │                              │
│                          ▼                │                              │
│                   ┌─────────────┐         │                              │
│                   │ Module 2.2  │◄────────┘                              │
│                   │ (Document   │         (Re-upload loop)               │
│                   │  Upload)    │                                        │
│                   └──────┬──────┘                                        │
│                          │                                               │
│              ┌───────────┴───────────┐                                   │
│              ▼                       ▼                                   │
│       ┌─────────────┐         ┌─────────────┐                            │
│       │  APPROVED   │         │  REJECTED   │                            │
│       └──────┬──────┘         └─────────────┘                            │
│              │                                                            │
│              ▼                                                            │
│       ┌─────────────┐                                                     │
│       │ Module 2.8  │◄─────────────────────────────────────────────┐      │
│       │ (Subscription│                                                  │      │
│       │  Selection) │                                                  │      │
│       └──────┬──────┘                                                  │      │
│              │                                                          │      │
│              ▼                                                          │      │
│       ┌─────────────┐     ┌─────────────────────────────────────────┐   │      │
│       │ Module 2.9  │     │  MODULE 8: USER MANAGEMENT              │   │      │
│       │ (Payment &  │     │  (Role-based tab visibility)            │   │      │
│       │  Activation)│     │                                         │   │      │
│       └──────┬──────┘     │  ┌─────────┐  ┌─────────┐  ┌─────────┐  │   │      │
│              │            │  │ Tab 1   │  │ Tab 2   │  │ Tab 3   │  │   │      │
│              ▼            │  │ User    │  │ Send    │  │Received │  │   │      │
│       ┌─────────────┐     │  │ List    │  │ Request │  │Requests │  │   │      │
│       │ Company     │     │  └────┬────┘  └────┬────┘  └────┬────┘  │   │      │
│       │ Admin       │     │       │            │            │       │   │      │
│       │ Dashboard   │     │       ▼            ▼            ▼       │   │      │
│       └──────┬──────┘     │  ┌─────────┐  ┌─────────┐  ┌─────────┐  │   │      │
│              │            │  │ View/   │  │ Create  │  │ Review/ │  │   │      │
│              │            │  │ Edit    │  │ Hiring  │  │ Approve │  │   │      │
│              │            │  │ Users   │  │ Request │  │ Requests│  │   │      │
│              │            │  └─────────┘  └─────────┘  └─────────┘  │   │      │
│              │            │                                         │   │      │
│              │            │  Uses: Module 5 (Roles)                 │   │      │
│              │            │        Module 6 (Salary/Leave Config)   │   │      │
│              │            │        Module 7 (Branches)              │   │      │
│              │            └─────────────────────────────────────────┘   │      │
│              │                                                          │      │
│              ├──────────────────────────────────────────────────────────┘      │
│              │                                                                 │
│              ├──────────────────► Module 5 (Role Management) ◄──────────┐     │
│              │                    (Client Side - Custom Roles)           │     │
│              │                                                           │     │
│              ├──────────────────► Module 6 (Role Salary & Leave)         │     │
│              │                    (Configuration for Users)                │     │
│              │                                                           │     │
│              ├──────────────────► Module 7 (Branch Management)           │     │
│              │                    (User Branch Assignment)                 │     │
│              │                                                           │     │
│              └──────────────────► Module 9 (Tax Management) ────────────┐ │     │
│                                   (Prerequisite for Module 10)          │ │     │
│                                                                         │ │     │
│  ┌─────────────────────────────────────────────────────────────────────┐│ │     │
│  │  SUPER ADMIN (SERAVION) SIDE - MODULE 3                             ││ │     │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ ││ │     │
│  │  │ Company     │  │ Module 4    │  │ Reports     │  │ Module 5    │ ││ │     │
│  │  │ Management  │──│ (Subscription│  │ (Analytics) │  │ (Role       │ ││ │     │
│  │  │ (3.1-3.3)   │  │  Plans)     │  │             │  │  Templates) │ ││ │     │
│  │  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘ ││ │     │
│  │         ▲                    │                                       ││ │     │
│  │         │                    ▼                                       ││ │     │
│  │         └───────────── Module 4 creates plans ───────────────────────┘│ │     │
│  │                         used in Module 2.8                            │     │
│  │                                                                       │     │
│  │  Module 5 (Seravion) creates Role Templates ──────────────────────────┘     │
│  │  used in Module 5.5 (Client Role Management)                                │
│  └─────────────────────────────────────────────────────────────────────────────┘
│                                                                                │
│  ┌─────────────────────────────────────────────────────────────────────────────┐
│  │  MODULE 9 & 10 CONNECTION                                                    │
│  │                                                                              │
│  │  Module 9 (Tax Management) MUST be configured BEFORE Module 10              │
│  │                                                                              │
│  │  Module 9.3 (Add Tax Type) ──┐                                              │
│  │                              ├──► Module 9.8 (Add HSN Code) ──┐             │
│  │  Module 9.4 (Edit Tax Type) ─┘    (Links Tax to HSN)          │             │
│  │                                                               ▼             │
│  │  Module 10.3 (Add Product) ──────────────────────────────► Uses HSN Code   │
│  │                                                               │             │
│  │                                                               ▼             │
│  │  Auto-populates Tax Rates in Product from HSN Configuration                 │
│  │                                                                              │
│  └─────────────────────────────────────────────────────────────────────────────┘
```

---

## Data Flow Summary

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         CONFIGURATION FLOW                               │
│                                                                          │
│  SERAVION (Super Admin) creates:                                         │
│  ├── Subscription Plans (Module 4) ───────► Used by Clients in Module 2  │
│  └── Role Templates (Module 5.1-5.4) ─────► Used by Clients in Module 5.5│
│                                                                          │
│  COMPANY ADMIN configures:                                               │
│  ├── Company Info (Module 2.1) ───────────► Approved by Seravion         │
│  ├── Roles (Module 5.5) ─────────────────► Based on Seravion templates   │
│  ├── Role Salary/Leave (Module 6) ───────► Used in User Creation (8.6)   │
│  ├── Branches (Module 7) ────────────────► Used in User Assignment       │
│  ├── Tax Types (Module 9.3) ─────────────► Used in HSN Codes (9.8)       │
│  ├── HSN Codes (Module 9.8) ─────────────► Used in Products (10.3)       │
│  └── Products (Module 10.3) ─────────────► Uses HSN for tax calc         │
│                                                                          │
│  USERS are created in Module 8.6:                                        │
│  ├── Personal Details (Step 1)                                           │
│  ├── Role auto-loads permissions from Module 5.5                         │
│  ├── Salary auto-loads from Module 6 (Step 3)                            │
│  ├── Leave auto-loads from Module 6 (Step 4)                             │
│  └── Documents (Step 5)                                                  │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Module Dependencies Matrix

| Module              | Depends On          | Used By        | Description                |
| ------------------- | ------------------- | -------------- | -------------------------- |
| Module 1            | None                | Module 2, 3, 8 | Entry point for all users  |
| Module 2            | Module 1            | Module 3, 4    | Onboarding after auth      |
| Module 3            | Module 1            | Module 2, 4    | Super Admin management     |
| Module 4            | Module 3            | Module 2.8     | Subscription plan creation |
| Module 5 (Seravion) | Module 3            | Module 5.5     | Role template creation     |
| Module 5.5 (Client) | Module 5 (Seravion) | Module 6, 8    | Company role customization |
| Module 6            | Module 5.5          | Module 8.6     | Salary/leave configuration |
| Module 7            | Module 2 (Approved) | Module 8.6     | Branch management          |
| Module 8            | Module 5.5, 6, 7    | Module 7.6     | User management            |
| Module 9            | Module 2 (Approved) | Module 9.8, 10 | Tax configuration          |
| Module 10           | Module 9            | Inventory ops  | Product management         |

---

## Critical Path for New Company

```
Step 1: Module 1.3 (Sign Up)
    ↓
Step 2: Module 2.1 (Company Info)
    ↓
Step 3: Module 2.2 (Document Upload)
    ↓
Step 4: Module 3.2 (Seravion Approval)
    ↓
Step 5: Module 2.8 (Subscription Purchase)
    ↓
Step 6: Module 9 (Tax Setup) ──► Module 10 (Product Setup)
    ↓
Step 7: Module 7 (Branch Setup)
    ↓
Step 8: Module 5.5 (Role Setup) ──► Module 6 (Role Config)
    ↓
Step 9: Module 8.6 (User Creation)
    ↓
Step 10: Full System Access
```

---
