# 📌 UI/UX Enhancement Documentation Notice

This document is prepared for UI/UX enhancement purposes.

Before starting work on any module or screen, please follow the below order strictly:

---

## 🔎 Reference Order (Mandatory)

1. **First**, refer to the provided Figma design:  
   👉 https://www.figma.com/design/a85AHKJ6mYHcksgbnJSIb8/New-enhanced?node-id=235-28040&m=draw

2. **If any required screen is not available there**, then refer to **Jay’s ERP Figma file**
   👉https://www.figma.com/design/mHNlJGxWHuUlzFx35YJcpG/ERP--Aurifex-?t=eHxRtMiPZJqywVMQ-0
   .
3. **If the screen is not found in both Figma files**, only then create a new screen as per the requirement.

---

## 📂 Documentation Structure

- All enhancements must be completed **module by module**.
- Each module must include:
  - Proper **Headings**
  - Structured **Screen-wise Documentation**
  - Clear **UI/UX improvement notes**

---

⚠️ **Important:**  
No new screen should be created without verifying both Figma references first.

## ==========================================================================

# UI/UX Documentation

> Note:
>
> - "Missing" mentioned in the raw input indicates **New Screen / New Module to be developed from scratch**.
> - # All other items are enhancements or modifications to the existing system.

# 1. Authentication Module (Sign Up / Login Screen)

This module covers authentication flow for both Super Admin (Seravion) and Company Admin (Client Side), including onboarding and subscription activation.

---

## Super Admin (Seravion Login)

### Screen: Super Admin (Seravion Login)

This screen is dedicated only for Seravion internal access.

### Fields:

- Username
- Password
- Submit (Login)

### Required Changes:

- Remove "Forgot Password" option.
- Remove "Sign Up" line.
- No external registration should be allowed.
- Strict login-only access for Seravion users.

This screen is an existing login screen but requires UI modification.

---

## Company Admin (Client Sign Up)

### Screen: Company Admin (Client Sign Up)

This screen allows new companies to register into the system.

### Fields:

- Company Name
- Authorized Person Name
- Phone
- Email
- Password
- Re-enter Password
- Get Started Button

### Required Changes:

- Remove Google login button.
- No third-party authentication.
- Keep a clean onboarding-focused registration form.

This is an existing screen but requires UI adjustment.

---

## Company Admin (Client Login)

### Screen: Company Admin (Client Login)

Standard login screen for registered company admins.

### Fields:

- Email / Username
- Password
- Submit (Login)

No major structural changes required.

---

# Onboarding Screen

This flow starts immediately after Signup/Login for Company Admin.

---

## Company Information

### Screen: Company Information

This screen collects essential company details.

### Fields:

- Company Name
- Industry Type (Dropdown)
- Contact Person Name
- Contact Person Email
- Contact Person Phone
- GST Number
- PAN Number
- Address Fields
- Save & Continue Button

### Change Required:

- License Number should be optional (Non-mandatory field).

This is an enhancement to the existing onboarding flow.

---

## Upload Document

### Screen: Upload Document

This screen allows company to upload required compliance documents.

### File Upload Fields:

- GST Certificate
- PAN Card
- Business / Registration Document

File validation and secure storage must be implemented.

---

## Document Success Screen

This is a new screen to be developed from scratch.

After successful document upload:

- Show confirmation message.
- Inform the user that approval is pending from Seravion side.
- User should not get full system access yet.

---

## Missing Screen (Waiting Period)

This is a new screen to be developed from scratch.

Until document approval:

- Show popup or restricted dashboard.
- Display message like:
  “We will approve your access shortly.”
- Prevent access to subscription and operational modules.

---

## On Document Approve

Once Seravion approves the document:

- Redirect user to Subscription Screen.
- Allow user to proceed with Pay Now.

---

## On Document Reject

### Document Rejected Screen

This is a new screen to be developed from scratch.

Flow:

- Show rejection reason.
- Allow document re-upload.
- Send again for review.
- Upon approval → Redirect to Subscription Screen.

---

## Subscription Screen

This is a new screen to be developed from scratch.

### Fields:

- Plan Selection Dropdown
- Plan Details (Short Description)
- Per Branch Price
- Per Technician Price
- Time Duration:
  - Yearly
  - Monthly
  - Quarterly
- Pay Now Button

This screen becomes accessible only after document approval.

---

## Missing Module in Sidebar (Subscription Module for Company Admin)

This is a new module to be developed from scratch.

### Features:

- Subscription Purchased List (Logs)
- View Subscription Details
- Purchase New Subscription
- View Payment Status

This module must be added in Company Admin sidebar.

---

## Missing Module for Company Admin – ROLE Management

This is a new module to be developed from scratch.

Must be added in sidebar.

### Add Role

Fields:

- Select Role from dropdown (created by Seravion)
- Clone permission from existing role (optional)
- Role Name

### Permission Structure:

25+ module-wise permission toggles:

- Add
- View
- Edit
- Delete
- Export
- Configure
- Approval Authority

### Role Management Features:

- Save Role
- Roles List
- View particular Role permissions
- Delete (with warning if role is assigned to users)
- Migrate role to another role
- Edit with warning

---

# Super Admin (Seravion Side)

---

## Side Bar Missing for Super Admin

This is a new sidebar structure to be created.

Modules to include:

- Dashboard
- Subscription Plan Creation / View / Edit
- Reports
- Role Management

---

## Dashboard Screen

Existing screen but requires modification.

### Required Changes:

- Change Status name to “Doc Status”.
- Add Pay Status:
  - Paid
  - Partial Paid
  - Free
  - Non Paid

---

## Particular Customer Popup

Enhancement required.

### Add:

- Status dropdown:
  - Approve
  - Pending
  - Rejected
- One checkbox for Trial
  - Select From Date – To Date

---

## Particular Customer Popup (Document Side)

Missing section to be developed.

Add:

- Subscription Purchase Details

---

## Subscription Plans (Missing Module)

This is a new module to be developed from scratch.

### Add Plan

Fields:

- Plan Name
- Per Branch Pricing (Branch count, price per branch)
- Per Technician Pricing (Branch count, price per technician)
- Description
- Duration:
  - Monthly
  - Yearly
  - Quarterly

### Features:

- View List of Plans
- View Plan Detail
- Edit Plan
- Delete Plan

---

## Role Management (Missing Module – Super Admin)

This is a new module to be developed from scratch.

### Add Role

Fields:

- Role Name
- 25+ module-wise permission toggles:
  - Add
  - View
  - Edit
  - Delete
  - Export
  - Configure
  - Approval Authority

### Features:

- Save Role
- Roles List
- View particular role permissions
- Delete (with migration warning)
- Migrate role
- Edit with warning

---

# Client Side – Branch Management

(Only changes from existing module)

---

## ADD Branch Screen

Enhancement required.

### Branch Types:

- HEAD_OFFICE
- STATE_BRANCH
- CITY_BRANCH
- WAREHOUSE

### Fields:

- Branch Name
- Address Fields
- 3 Letter Code
- Created By

### Required Fields:

- Name
- Address
- 3 Letter Code

---

## GET LIST Screen

Modify existing branch list.

### Remove:

- Type column
- City column
- State column

### Actions:

- View (Specific Branch Details Screen)
- Edit
- Delete (Soft delete – mark as Inactive)

---

## Edit Screen

Same as Add Branch Screen.

Change:

- Replace “Created By” with “Edited By”.

---

## View Details for Specific Branch

This screen must contain 2 Tabs:

### Tab 1: Branch Info.

- Branch Name
- Address
- 3 Letter Code
- Status
- Created/Edited details

### Tab 2: Branch Employees List

Columns:

- Emp ID
- Emp Name
- Email ID
- Contact Number
- Designation
- Staff Status
- Branch Name

---
