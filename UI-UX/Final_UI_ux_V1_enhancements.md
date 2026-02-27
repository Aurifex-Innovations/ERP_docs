Figma Link -` https://www.figma.com/design/KRRyTkPnQ5Q77qyEznNF7U/ERP_v1_enhancement_Feb-26?node-id=1-2&p=f&t=eZnOH2G7iD20vkN7-0`

---

# Module 1: Authentication

## Overview

**Seravion (Super Admin)**

- Only Login with **Username + Password**.
- No Signup and No Forgot Password option.
- This is secure internal access.

**Client (Company Admin — e.g., Shrini Pest Control)**

- Sign Up + Login + Forgot Password functionality available.
- Can register their company and log in later using credentials.

**Client Users (Branch Staff, Operations Head, Technician Manager, etc.)**

- Only Login + Forgot Password.
- No public signup.
- Users are created by the Company Admin inside the ERP.

---

## 1.1 Super Admin (Seravion Login)

**Screen:** Super Admin (Seravion Login)

This screen is only for internal Seravion access.

### Fields

- Username
- Password
- Submit (Login)

### Required Changes

- Remove **Forgot Password**.
- Remove **Signup** line.
- No public registration allowed.

---

## 1.2 Company Admin (Client Sign Up)

**Screen:** Company Admin (Client Sign Up)

This screen allows new companies to register.

### Fields

- Company Name
- Authorized Person Name
- Phone
- Email
- Password
- Re-enter Password
- Get Started Button

### Required Changes

- Remove Google button.
- No social login integration.

---

## 1.3 Login Option Selection

**Screen:** Login Option Screen

User selects which login they want:

- Root Login
- User Login

### Required Changes

- Remove Google login button.
- Align UI with other authentication screens.

---

## 1.4 Company Admin (Client Login)

**Screen:** Company Admin (Client Login)

### Changes

- Replace **Root Username** with **Email**.

### Fields

- Email
- Password

---

## 1.5 Company User (IAM User Login)

**Screen:** Users / IAM Users Login

### Changes

- Heading should be **IAM User Login**.
- Remove Signup redirect line.

### Fields

- Tenant ID
- Username
- Password

---

## 1.6 Forgot Password

### Company Admin Flow

Email → OTP → Verification → Reset Password Screen

### Company IAM User Flow

Mobile → OTP → Verification → Reset Password Screen

---

# Module 2: User Onboarding

## Overview Flow

1. Company Admin completes Signup/Login.
2. Redirect to **Company Information Screen**.
3. Fill company details (License Number optional).
4. Click **Save & Continue**.
5. Redirect to **Upload Document Screen**.
6. Upload GST, PAN, and Business Registration documents.
7. Submit for verification.
8. Show Submission Success Screen.
9. Restrict Dashboard access.
10. Show message:
    **“We will approve your access shortly.”**

### Approval Outcomes

**If Approved**

- Redirect to Subscription Screen.

**If Rejected**

- Show Document Rejected Screen.
- Display rejection reason.
- Allow re-upload and resubmission.

---

## Subscription Flow

- Select Plan (Dropdown)
- View Plan Details (Short Description)
- View Per Branch Price
- View Per Technician Price
- Select Duration (Monthly / Quarterly / Yearly)
- Click Pay Now
- Complete Payment
- Subscription becomes Active

### Sidebar Addition

Add **Subscription Module** in Company Admin sidebar.

Inside Subscription Module:

- Subscription Purchase List (Logs)
- View Subscription Details
- Purchase New Plan
- Payment Status:
  - Paid
  - Partial Paid
  - Free
  - Non Paid

---

## 2.1 Company Information (Client Side)

**Screen:** Company Information

Captures official company details.

### Fields

- Company Name
- Industry Type (Dropdown)
- Contact Person Name
- Contact Person Email
- Contact Person Phone
- GST Number
- PAN Number
- Address Fields
- Save & Continue

### Change

- License Number should be optional.

---

## 2.2 Upload Document (Client Side)

**Screen:** Upload Document

### Upload Fields

- GST Document
- PAN Document
- Business / Registration Document

---

## 2.3 Document Uploaded Successfully

System popup confirming upload.

---

## 2.4 Document Verification Pending

Client-facing waiting screen.

---

## 2.5 Document Rejected Screen (Client Facing)

Displays rejection reason.

---

## 2.6 Re-upload Documents

Allows document resubmission after rejection.

---

## 2.7 Document Verification Success Screen

Client-facing approval confirmation.

---

## 2.8 Subscription Screen (Client Side)

Accessible only after document approval.

### Features

- Plan Selection Dropdown
- Plan Details (Short Description)
- Per Branch Price
- Per Technician Price
- Branch Count
- Technician Count

### Duration Options

- Yearly

- Monthly

- Custom Date Range

- Quarterly

- Calculation Popup

- Pay Now Button

---

## 2.9 Subscription Module (Company Admin Sidebar)

New sidebar module.

### Screens

- Subscription Purchased List (Logs)
- View Subscription Details
- Purchase New Plan

---

# Module 3: Super Admin (Seravion) Management Module

Create a **Super Admin Sidebar** visible only to Seravion users.

### Sidebar Modules

- Company Management
- Subscription Plans (Module 4)
- Reports (Subscription, Payment, Onboarding)
- Role Management

---

## 3.1 Company Management (Super Admin Side)

### Status Dropdown

- Approve
- Pending
- Rejected (Reason hover)

### Fields

- Company ID
- Name
- Email / Contact Number
- Created On
- Subscription Start / End Date
- Location (City, State)
- Branch Count
- Technician Count
- Document Status
- Actions

---

## 3.2 View Company Details (Before Approval)

### Fields

- Company Name
- Company ID
- Contact Person
- Email
- Full Address
- Documents
- GST
- PAN
- License Number

### Required Additions

Status Dropdown:

- Approve
- Pending
- Rejected (Reason required)

Trial Access Checkbox:

- Enable Trial
- From Date / To Date

---

## 3.3 After Document Approval

Same as 3.2, but shows **Subscription Details**.

---

# Module 4: Subscription Plans (Super Admin Only)

Manage subscription plan lifecycle.

---

## 4.1 Plan List View

### Table Columns

- Plan Name
- Duration
- Per Branch Pricing
- Per Technician Pricing
- Status (Active / Inactive)
- Created Date
- Actions (View | Edit | Delete)

### Flow

- View → Plan Detail
- Edit → Edit Screen
- Delete → Confirmation Popup (Inactive only)

---

## 4.2 Add Plan Screen

### Fields

- Plan Name
- Per Branch Pricing
- Per Technician Pricing
- Description
- Duration
- Save Button

### Validation

- Plan Name required
- Pricing numeric & required
- Duration required

---

## 4.3 Edit Plan Screen

Same fields as Add Plan.

### Rule

Changes apply only to new subscriptions.

---

## 4.4 Delete Plan Warning Popup

Message:

> “This plan is currently assigned to active companies. It cannot be permanently deleted.”

### Options

- Cancel
- Mark as Inactive

Inactive plans:

- Hidden from new purchases
- Visible in history

---

# Module 5: Role Management (Seravion Side)

Controls module-wise ERP permissions.

---

## 5.1 Role Management (Super Admin)

### Add Role

**Field**

- Role Name (Required)

### UI Behavior

If **Is Application User** is checked:

- Disable all permission toggles.

If unchecked:

- Show module permission grid (25+ modules).

### Permission Toggles

- View
- Add
- Edit
- Delete
- Request (Multi-select dropdown)
- Approval
- Export
- Configure Access

### Conditional Behavior

**Request ON**

- Show Request Receivers (multi-select roles).

**Approval ON**

- Show info label:
  “Users with this role can approve/reject incoming requests.”

### Validation

- Role Name required.
- Request ON → Receiver required.

### Interaction Rules (For UIUX Implementation)

- Permissions displayed in table/grid layout:
  Rows → Modules
  Columns → Permission toggles
- Request Receivers field appears inline under module row when Request toggle is enabled.
- Multi-select supports search and multiple role selection.
- Hide Request Receivers when Request toggle is OFF.

---

## 5.2 Edit Role

Modify role name or permissions.

Show warning:

> “Changes will impact all users assigned to this role.”

---

## 5.3 View Role Detail

- Role Name
- Assigned permissions grid
- Application User type
- Approval authority roles

---

## 5.4 Delete Role (Migration Handling)

Popup:

> “Users are currently assigned to this role.”

System must:

- Show assigned user count
- Select new role for migration
- Reassign users
- Mark role as Inactive

Inactive roles cannot be assigned to new users.

---

## 5.5 Role Management (Client Side)

Add Role Management in client sidebar.

Same as Seravion role management with addition:

- Select existing role template
- Modify permissions instead of creating from scratch.

---

# Module 6: Role Salary & Leave Configuration (Company Admin)

Sub-module under Role Management.

Defines salary and leave policies role-wise.

During user creation:

- Configuration auto-fetches.
- Admin may override values.

---

## 6.1 Configuration List View

### Table Columns

**Basic**

- Role
- Effective From
- Effective To
- Status

**Salary Summary**

- Salary Type
- Basic Salary
- Bank Name
- Salary Effective Dates

**Leave Summary**

- Leave Approval Authority
- Leave Reset Cycle

### Actions

- Add Configuration
- View
- Edit
- Deactivate
- Clone

### Filters

- Role
- Status
- Salary Type
- Effective Date Range
- Leave Reset Cycle
- Search (Role Name)

---

## 6.2 Add Configuration (Multi-Step Form)

### Basic Details

- Select Role (Multi-select)
- Effective From (Required)
- Effective To (Optional)
- Status

---

### Step 1: Salary Details

- Salary Type (CTC / Fixed / Hourly)
- Basic Salary
- HRA
- Other Allowance
- Incentive
- Deductions
- PF / ESI / TDS (Optional)
- Bank Name
- Account Number
- IFSC Code
- Salary Effective Dates

#### Incentive Configuration

- Holiday Work Incentive
- Incentive Type & Amount
- Overtime Settings
- Per Hour Incentive
- Max Overtime Hours

---

### Step 2: Leave Details

(Auto-fetch if available)

- Casual Leave (CL)
- Sick Leave (SL)
- Paid Leave (PL)
- Annual Leave Allocation
- Carry Forward Allowed
- Max Carry Forward Days
- Leave Approval Authority (Required)
- Leave Reset Cycle (Yearly / Monthly/custom date)

---

## 6.3 Edit Configuration

Same as Add form, but **Role selection disabled**.

---

## 6.4 View Configuration

Read-only display of full salary and leave configuration.

---
