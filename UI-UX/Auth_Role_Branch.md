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

# =============================================================================================================

# Module 1: Authentication

This module handles login and registration access for both Super Admin (Seravion side) and Company Admin (Client side).

---

## 1.1 Super Admin (Seravion Login)

### Screen: Super Admin (Seravion Login)

This screen is only for internal Seravion access.

### Fields:

- Username
- Password
- Submit (Login)

### Required Changes:

- Remove “Forgot Password”.
- Remove “Signup” line.
- No public registration allowed.
- Dedicated secure login only.

This is an existing screen with UI modifications.

---

## 1.2 Company Admin (Client Sign Up)

### Screen: Company Admin (Client Sign Up)

This screen allows new companies to register.

### Fields:

- Company Name
- Authorized Person Name
- Phone
- Email
- Password
- Re-enter Password
- Get Started Button

### Required Changes:

- Remove Google button.
- No social login integration.

Existing screen with UI refinement.

---

## 1.3 Company Admin (Client Login)

### Screen: Company Admin (Client Login)

Standard login functionality.

### Fields:

- Email / Username
- Password
- Submit (Login)

No major structural changes required.

---

=============================================================================================================

# Module 2: User Onboarding

This module begins after Company Admin completes Signup/Login.

It covers company information submission and document verification.

---

## 2.1 Onboarding Screen

Triggered immediately after authentication.

---

### Company Information

### Screen: Company Information

This screen captures official company details.

### Fields:

- Company Name
- Industry Type (Dropdown)
- Contact Person Name
- Contact Person Email
- Contact Person Phone
- GST Number
- PAN Number
- Address Fields
- Save & Continue

### Changes:

- License Number should be optional (Non-required field).

Enhancement to onboarding flow.

---

### Upload Document

### Screen: Upload Document

This screen collects compliance documents.

### Upload Fields:

- GST Document
- PAN Document
- Business / Registration Document

---

### Document Success Screen

New screen to be developed.

Purpose:

- Show successful submission confirmation.
- Inform user that approval is pending from Seravion side.

---

### Missing Screen (Waiting Period)

New screen to be developed.

Purpose:

- Restrict dashboard access until approval.
- Show popup message:
  “We will approve your access shortly.”

---

### On Document Approve

Flow:

- Redirect user to Subscription Screen.
- Allow user to proceed with payment.

---

### On Document Reject

### Screen: Document Rejected Screen

New screen to be developed.

Flow:

- Show rejection reason.
- Allow document re-upload.
- Resubmit for review.
- Once approved → Redirect to Subscription Screen.

---

=============================================================================================================

# Module 3: Subscription

This module handles plan selection, pricing, and subscription management.

---

## 3.1 Subscription Screen (Client Side)

New screen to be developed.

### Features:

- Plan Selection Dropdown
- Plan Details (Short Description)
- Per Branch Price
- Per Technician Price
- Duration Options:
  - Yearly
  - Monthly
  - Quarterly
- Pay Now Button

Accessible only after document approval.

---

## 3.2 Subscription Module (Company Admin Sidebar)

New sidebar module to be created.

### Features:

- Subscription Purchased List (Logs)
- View Subscription Details
- Purchase New Plan
- Payment Status:
  - Paid
  - Partial Paid
  - Free
  - Non Paid

---

## 3.3 Subscription Plans (Super Admin Side)

New module to be developed.

### Add Plan

Fields:

- Plan Name
- Per Branch Pricing (branch count + price per branch)
- Per Technician Pricing (technician count + price per technician)
- Description
- Duration:
  - Monthly
  - Quarterly
  - Yearly

### Functionalities:

- View Plan List
- View Plan Detail
- Edit Plan
- Delete Plan

---

=============================================================================================================

# Module 4: Role Management

This module handles permission-based access control.

---

## 4.1 Role Management (Company Admin Side)

New sidebar module to be created.

### Add Role

Fields:

- Select Role from dropdown (Created by Seravion)
- Clone permission from existing role
- Role Name

### Permission Structure (25+ Modules)

Each module will have toggles:

- Add
- View
- Edit
- Delete
- Export
- Configure
- Approval Authority

### Functionalities:

- Save Role
- Roles List
- View Role Permissions
- Edit (With Warning)
- Delete (Warning if users assigned)
- Migrate role to another role

---

## 4.2 Role Management (Super Admin Side)

New module to be developed.

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

### Functionalities:

- Save Role
- Roles List
- View Role Permissions
- Edit with warning
- Delete with migration handling

---

=============================================================================================================

# Module 5: Branch Management (Client Side)

This module includes changes to the existing Branch Management system.

---

## 5.1 ADD Branch Screen

Enhancement to existing screen.

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

## 5.2 GET LIST Screen

Modification required.

### Remove:

- Type column
- City column
- State column

### Actions:

- View (Specific Branch Details Screen)
- Edit
- Delete (Soft delete – mark as Inactive)

---

## 5.3 Edit Screen

Same structure as Add Branch Screen.

Change:

- Replace “Created By” with “Edited By”.

---

## 5.4 View Details for Specific Branch

This screen should contain 2 Tabs:

### Tab 1: Branch Info.

- Branch Name
- Address
- 3 Letter Code
- Status
- Created / Edited Details

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
