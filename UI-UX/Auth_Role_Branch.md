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

# Module 3: Super Admin (Seravion) Management Module

This module is exclusively for Seravion internal operations.
It includes dashboard monitoring, customer approval workflow, subscription tracking, and role management.

All items mentioned below are either new modules or enhancements as specified.

---

## 3.1 Super Admin Sidebar (New – To Be Developed)

A completely new sidebar structure must be created for Super Admin (Seravion).

### Sidebar Modules:

- Dashboard
- Subscription Plan Creation / View / Edit
- Reports
- Role Management

This sidebar should be visible only to Super Admin users.

---

## 3.2 Dashboard Screen (Enhancement Required)

The existing Dashboard screen is functionally correct but requires modifications.

### Required Changes:

1. Rename "Status" field to:
   → **Doc Status**

2. Add a new field:
   → **Pay Status**

### Pay Status Options:

- Paid
- Partial Paid
- Free
- Non Paid

This will allow Super Admin to track both document approval and payment tracking separately.

---

## 3.3 Particular Customer Popup (Enhancement Required)

This popup appears when Super Admin clicks on a specific customer.

### Required Additions:

1. Status Dropdown:
   - Approve
   - Pending
   - Rejected

2. Trial Access Checkbox:
   - Checkbox: "Enable Trial"
   - If checked:
     - From Date
     - To Date

This allows Seravion to provide temporary trial access before subscription purchase.

---

## 3.4 Particular Customer Popup (Document Side) – New Section

This section is currently missing and must be developed.

### Add:

- Subscription Purchase Details
  - Plan Name
  - Duration
  - Branch Count
  - Technician Count
  - Payment Status
  - Start Date
  - End Date

This will help Super Admin see full customer subscription visibility inside the same popup.

---

=============================================================================================================

# Module 4: Subscription

This module handles plan selection, pricing, and subscription management.

---

## 4.1 Subscription Screen (Client Side)

Existing screen enhancement(screen name-https://www.figma.com/design/mHNlJGxWHuUlzFx35YJcpG/ERP--Aurifex-?node-id=5925-75976&t=ThWj8nSZUO8qVZql-4)

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

## 4.2 Subscription Module (Company Admin Sidebar add module)

New sidebar module to be created.

### Features:

- Subscription Purchased List (Logs)
- View Subscription Details
- Purchase New Plan (redirect to 3.1)
- Payment Status:
  - Paid
  - Partial Paid
  - Free
  - Non Paid

---

## 4.3 Subscription Plans (Super Admin Sidebar add module)

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

# Module 5: Role Management

This module handles permission-based access control.

---

## 5.1 Role Management (Company Admin Side)

New sidebar module to be created.

### Add Role

Fields:

- Select Role from dropdown (Created by Seravion)
- Clone permission from existing role
- Role Name

### Permission Structure (25+ Modules)

Application User: checkbox set for application user only permission
Note:  
If "Is Application User" is checked, module permission structure will not be applicable for this role.

Each module will have toggles:

- Add
- View
- Edit
- Delete
- Export
- Configure
- Approval Authority [Selected then show dropdown to select role]

### Functionalities:

- Save Role
- Roles List
- View Role Permissions
- Edit (With Warning)
- Delete (Warning if users assigned)
- Migrate role to another role

---

## 5.2 Role Management (Super Admin Side)

New module to be developed.

### Add Role

Application User: checkbox set for application user only permission
Note:  
If "Is Application User" is checked, module permission structure will not be applicable for this role.

Fields:

- Role Name
- 25+ module-wise permission toggles:
  - Add
  - View
  - Edit
  - Delete
  - Export
  - Configure
  - Approval Authority [Selected then show dropdown to select user]

### Functionalities:

- Save Role
- Roles List
- View Role Permissions
- Edit with warning
- Delete with migration handling

---

## 5.3 Role Salary & Leave Configuration (Company Admin Side)

New sidebar module to be created.

### Purpose

Company Admin can define Salary Structure and Leave Structure role-wise.  
During User Creation, based on selected Role, the system auto-fetches this configuration.  
Admin can modify if required, otherwise default values will be used.

---

## Add Role Compensation Configuration

### Basic Details

- Select Role – Required (Dropdown)
- Effective From Date – Required
- Effective To Date – Optional
- Status (Active / Inactive) – Required

---

## Salary Configuration Section (Role Based)

- Salary Type (CTC / Fixed / Hourly) – Required
- Default Basic Salary – Required
- Default HRA – Optional
- Default Other Allowance – Optional
- Default Incentive – Optional
- Default Deductions – Optional

- PF Applicable – Optional
- ESI Applicable – Optional
- TDS Applicable – Optional

### Holiday Work Configuration

- Holiday Work Incentive Applicable – Optional
- Holiday Work Incentive Type (Fixed / Per Day / Per Hour) – Optional
- Holiday Work Incentive Default Amount – Optional

### Overtime Configuration

- Overtime Applicable – Optional
- Overtime Type (Per Hour / Per Shift) – Optional
- Overtime Shift Type – Optional
- Overtime Shift Incentive Default Amount – Optional

- Per Hour Incentive Default Pay Amount – Optional
- Maximum Overtime Hours Per Month – Optional

---

## Leave Configuration Section (Role Based)

- Casual Leave (CL) Per Year – Optional
- Sick Leave (SL) Per Year – Optional
- Paid Leave (PL) Per Year – Optional
- Annual Leave Allocation – Optional

- Carry Forward Allowed – Optional
- Max Carry Forward Days – Optional

- Leave Approval Authority Role – Required
- Leave Reset Cycle (Yearly / Monthly) – Required

---

## Functionalities

- Save Configuration
- Configuration List (Role Wise)
- View Configuration
- Edit (With Effective Date Handling)
- Deactivate Configuration
- Clone Configuration to Another Role

---

=============================================================================================================

# Module 6: Branch Management (Client Side)

This module includes changes to the existing Branch Management system.

---

## 6.1 ADD Branch Screen

Enhancement to existing screen.

### Fields:

- Branch Name
- Address Fields
- 3 Letter Code
- Created By
- Branch Types dropdown[HEAD_OFFICE, STATE_BRANCH, CITY_BRANCH, WAREHOUSE]

### Required Fields:

- Name
- Address
- 3 Letter Code

---

## 6.2 GET LIST Screen

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

## 6.3 Edit Screen

Same structure as Add Branch Screen.

Change:

- Replace “Created By” with “Edited By”.

---

## 6.4 View Details for Specific Branch

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

============================================================================================================

# Module 7 – User Management

---

## 7.1 Overview with Tab Logic

User Management contains 3 Tabs:

- Tab 1: User List
- Tab 2: Send Request to Add New Employee
- Tab 3: Received Requests to Add New Employee

Tab visibility depends on user role level.

---

### Role-Based Tab Visibility

#### Lower-Level Roles (Manager / Team Lead)

Can See:

- Tab 1: User List
- Tab 2: Send Request to Add New Employee

Cannot See:

- Tab 3: Received Requests

##### Why?

- They can view existing users (as per permission).
- They cannot directly create employees.
- They can only raise hiring requests.
- They cannot approve or review other requests.

---

#### Upper-Level Roles (Admin / HR / Company Admin)

Can See:

- Tab 1: User List
- Tab 2: Send Request to Add New Employee
- Tab 3: Received Requests

##### Why?

- They manage employee creation.
- They review and approve hiring requests.
- They can convert approved requests into actual employees.

---

### Tab Combination Matrix

| Role Level  | Tab 1 | Tab 2    | Tab 3 |
| ----------- | ----- | -------- | ----- |
| Lower-Level | Yes   | Yes      | No    |
| Upper-Level | Yes   | No       | Yes   |
| Super Admin | Yes   | Optional | Yes   |

---

## 7.2 Get All User List

### Tab 1: User List (View Permissions)

Exisiting screen have in figma.

Purpose:  
Display all existing users with quick actions.

#### Table Columns

- EMP ID
- Employee Name
- Email ID
- Contact Number
- Designation
- Department
- Role
- Branch Name (If multiple, show comma separated or count + view option)
- Reporting Manager
- Status (Active / Inactive)
- Created Date

#### Actions

- View
- Edit
- Delete (With confirmation)

---

## 7.3 Get Tabs Wise

---

### Tab 2: Send Request to Add New Employee

(For Lower-Level Manager to request hiring)

New form module in User Management.

Purpose:  
Managers cannot directly create users but can raise hiring request to Admin.

---

#### A) Request Form Fields

##### Basic Information

- Requested By – Auto-filled (Logged-in User)
- Department – Required (Textfield)
- Designation – Required (Textfield)
- Proposed Role – Required (Dropdown)
- Branch – Required (Multiple Selection)
- Employment Type (Permanent / Contract / Intern) – Required
- Expected Date of Joining – Required
- Number of Positions – Required
- Hiring Reason – Required (Textarea)
- Job Description – Optional (Textarea)
- Additional Remarks – Optional (Textarea)
- Supporting Document – Optional (File Upload)

##### Actions

- Submit Request
- Cancel

---

#### B) My Hiring Requests (Table View)

Purpose:  
Requester can see all requests submitted by them.  
New form module in User Management.

##### Table Columns

- Request ID
- Department
- Proposed Role
- Branch (can be multiple)
- Number of Positions
- Expected Joining Date
- Hiring Reason (Short Preview)
- Status (Pending / Approved / Rejected / Converted)
- Submitted Date
- Last Updated Date

##### Actions

- View (Full Details)
- View Rejection Reason (If Rejected)

---

### Tab 3: Received Requests to Add New Employee

(Admin / Higher Authority View)

New form module in User Management.

Purpose:  
Admin or authorized role can review, approve, reject, or convert hiring requests into actual employees.

---

#### A) Received Requests – Table View

##### Table Columns

- Request ID
- Requested By (Employee Name)
- Department
- Proposed Role
- Branch
- Employment Type
- Number of Positions
- Expected Joining Date
- Proposed Salary (if applicable)
- Status (Pending / Approved / Rejected)
- Submitted Date
- Reviewed By
- Reviewed Date

---

#### B) Filters & Search

- Filter by Status
- Filter by Department
- Filter by Branch
- Filter by Requested By
- Date Range Filter
- Search (Request ID / Employee Name / Role)

---

#### C) Actions

##### For Pending Requests

- View (Full Details)
- Approve
- Reject (Rejection Reason Mandatory)

##### For Rejected Requests

- View Rejection Reason

---

#### D) Request Detail View (On Clicking View)

Display Full Information:

- Requested By
- Department
- Designation
- Proposed Role
- Branch
- Employment Type
- Expected Date of Joining
- Number of Positions
- Hiring Reason
- Job Description
- Additional Remarks
- Supporting Document (Download Option)

---

#### Status Flow

- Pending → Approved
- Pending → Rejected

---

#### Validation Rules

- Reject → Rejection Reason Mandatory
- Approve → Reviewer Name Auto-Captured

---

## 7.4 Add User Form

Exisiting screen have in figma. but we have to change

Add User Form for After hiring manual work done just add emp in system

---

# USER ADD FORM

-Multi step form with skip and previous next button

## Step 1: Personal Details

### Basic Information

- EMP ID – Required
- First Name – Required
- Last Name – Required
- Email – Optional
- Contact Number – Required
- Alternate Number – Optional
- Password – Required (Only at creation)
- Department – Required
- Designation – Required
- Role – Required
- Branch (Multiple Selection) – Required
- Reporting Manager – Required (Filtered by Branch & Role)
- Employment Type (Permanent / Contract / Intern) – Required
- Date of Joining – Required
- Status (Active / Inactive) – Required

---

### Address Details

#### Current Address

- Address Line 1 – Required
- Address Line 2 – Optional
- City – Required
- State – Required
- Country – Required
- Pincode – Required

#### Permanent Address

- Permanent Address Line 1 – Required
- Permanent Address Line 2 – Optional
- Permanent City – Required
- Permanent State – Required
- Permanent Country – Required
- Permanent Pincode – Required

---

## Step 2: Module Permissions

(Load based on selected Role)

- Is Application User – Optional (Checkbox)

If "Is Application User" is checked:

- Disable entire module permission section.

If unchecked:

- Load 25+ modules with permission toggles.

For each module:

- Add – Optional
- View – Optional
- Edit – Optional
- Delete – Optional
- Export – Optional
- Approval Authority – Optional
  - If enabled → Show dropdown to select Role

---

## Step 3: Salary Details

(Auto-fetch from Role Configuration if available)

- Salary Type (CTC / Fixed / Hourly) – Required
- Basic Salary – Required
- HRA – Optional
- Other Allowance – Optional
- Incentive – Optional
- Deductions – Optional
- PF Applicable – Optional
- ESI Applicable – Optional
- TDS Applicable – Optional
- Bank Name – Required
- Account Number – Required
- IFSC Code – Required
- Salary Effective From – Required
- Salary Effective To – Optional

### Incentive Configuration

- Holiday Work Incentive Applicable – Optional
- Holiday Work Incentive Type – Optional
- Holiday Work Incentive Amount – Optional
- Overtime Applicable – Optional
- Overtime Type – Optional
- Overtime Shift Type – Optional
- Overtime Shift Incentive Amount – Optional
- Per Hour Incentive Pay Amount – Optional
- Maximum Overtime Hours Per Month – Optional

---

## Step 4: Leave Details

(Auto-fetch from Role Configuration if available)

- Casual Leave (CL) – Optional
- Sick Leave (SL) – Optional
- Paid Leave (PL) – Optional
- Annual Leave Allocation – Optional
- Carry Forward Allowed – Optional
- Max Carry Forward Days – Optional
- Leave Approval Authority – Required
- Leave Reset Cycle (Yearly / Monthly) – Required

---

## Step 5: Documents & Additional Data

### Identity & Compliance

- Aadhar Number – Optional
- PAN Number – Optional
- UAN Number – Optional
- Employee ID Card Number – Optional

### Upload Documents

- Aadhar Upload – Optional
- PAN Upload – Optional
- Address Proof Upload – Optional
- Educational Certificates – Optional
- Experience Letter – Optional
- Offer Letter – Optional
- Appointment Letter – Optional
- Other Documents – Optional

### Additional Professional Data

- Grade / Level – Optional
- Shift Type – Optional
- Weekly Off – Optional
- Target Amount – Optional
- Commission Percentage – Optional
- Employee Photo – Optional

## Edit User Form

-Edit User Form – Same as Add User Form (All steps and fields remain same as Add User).

---
