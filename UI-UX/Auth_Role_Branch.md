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

<!--
# Module 1: Authentication & Onboarding

---

## 1.1 Super Admin Authentication (Seravion Login)

### Screen: Super Admin Login

**Fields:**

- Username
- Password
- Submit (Login)

**UI Requirements:**

- Remove "Forgot Password"
- Remove "Sign Up" option
- Dedicated login access for Seravion only

---

## 1.2 Company Admin Authentication (Client Side)

### A. Company Admin Sign Up

**Fields:**

- Company Name
- Authorized Person Name
- Phone
- Email
- Password
- Re-enter Password
- Get Started (CTA Button)

**UI Rules:**

- Remove Google login button
- No social authentication
- Clean onboarding-focused screen

---

### B. Company Admin Login

**Fields:**

- Email / Username
- Password
- Submit (Login)

---

# Module 2: Company Onboarding & Subscription Flow

(Triggered After Company Admin Sign Up / First Login)

---

## 2.1 Company Information Screen

**Fields:**

- Company Name
- Industry Type (Dropdown)
- Contact Person Name
- Contact Person Email
- Contact Person Phone
- GST Number
- PAN Number
- Address Fields (State, City, Pincode, Full Address)

**Rules:**

- License Number → Optional (Not Required)
- CTA: Save & Continue

---

## 2.2 Document Upload Screen

**Upload Fields:**

- GST Certificate
- PAN Card
- Business / Registration Certificate

File upload validation required.

---

## 2.3 Document Submission Status Flow

### After Submission:

- Show Document Success Screen
- Display message:
  > "Your documents are under review. Seravion team will approve your access shortly."

### Waiting Period Screen:

- Restrict dashboard access
- Show popup: “Approval Pending”

---

## 2.4 Document Review Outcome

### If Approved:

→ Redirect to Subscription Screen → Pay Now

### If Rejected:

→ Show Document Rejected Screen

- Show rejection reason
- Allow document re-upload
- Resubmit for review
- Upon approval → Redirect to Subscription Screen

---

## 2.5 Subscription Screen (Client Side)

**Elements:**

- Plan Selection Dropdown
- Plan Details (Short Description)
- Pricing Structure:
  - Per Branch Price
  - Per Technician Price
- Duration Options:
  - Monthly
  - Quarterly
  - Yearly
- Pay Now Button

---

## 2.6 Subscription Module (Sidebar - Company Admin)

**Features:**

- Subscription Purchased List (Logs)
- View Subscription Details
- Purchase New Plan
- Payment Status View:
  - Paid
  - Partial Paid
  - Free
  - Non Paid

---

# Module 3: Role-Based Access Control (RBAC)

---

## 3.1 Company Admin Role Management (Client Side)

**Sidebar Module: Role Management**

### Add Role

**Fields:**

- Select Base Role (Dropdown – Created by Seravion)
- Clone Permission from Existing Role (Optional)
- Role Name

### Permission Matrix (25+ Modules)

Each Module should have toggles for:

- Add
- View
- Edit
- Delete
- Export
- Configure
- Approval Authority

## Role Management – Permission Matrix Example

Module 1 – User Management
[ ] Add
[ ] View
[ ] Edit
[ ] Delete
[ ] Export
[ ] Configure
[ ] Approval Authority

Module 2 – Branch Management
[ ] Add
[ ] View
[ ] Edit
[ ] Delete
[ ] Export
[ ] Configure
[ ] Approval Authority

### Role Actions:

- Save Role
- Roles List
- View Particular Role Permissions
- Edit Role (With Warning)
- Delete Role (Warning if assigned to users)
- Migrate Users from One Role to Another

---

## 3.2 Super Admin Role Management (Seravion Side)

**Sidebar Module: Role Management**

### Add Role

**Fields:**

- Role Name
- Module-wise Permission Toggles:
  - Add
  - View
  - Edit
  - Delete
  - Export
  - Configure
  - Approval Authority

### Actions:

- Save Role
- Roles List
- View Role Permissions
- Edit (With Warning)
- Delete (With Migration Warning)

---

# Module 4: Super Admin (Seravion Panel)

---

## 4.1 Super Admin Sidebar Modules

- Dashboard
- Subscription Plan Management
- Reports
- Role Management

---

## 4.2 Dashboard Enhancements

**Changes Required:**

- Rename “Status” → “Document Status”
- Add Payment Status Field:
  - Paid
  - Partial Paid
  - Free
  - Non Paid

---

## 4.3 Customer Detail Popup (Admin Side)

**Fields:**

- Status Dropdown:
  - Approved
  - Pending
  - Rejected
- Trial Access Checkbox:
  - Enable Trial
  - From Date – To Date

---

## 4.4 Customer Document Section (Popup)

**Add:**

- Subscription Purchase Details
- Payment History Summary

---

## 4.5 Subscription Plan Management (Super Admin)

### Add Plan

**Fields:**

- Plan Name
- Per Branch Pricing (Branch Count + Price per Branch)
- Per Technician Pricing (Technician Count + Price per Technician)
- Description
- Duration:
  - Monthly
  - Quarterly
  - Yearly

### Actions:

- View Plan List
- View Plan Detail
- Edit Plan
- Delete Plan

---

# Module 5: Branch Management (Client Side)

(Changes from Existing Module)

---

## 5.1 Add Branch Screen

**Branch Types (Enum):**

- HEAD_OFFICE
- STATE_BRANCH
- CITY_BRANCH
- WAREHOUSE

**Fields:**

- Branch Name
- Address Fields
- 3-Letter Branch Code
- Created By (Auto-filled)

**Required Fields:**

- Name
- Address
- 3-Letter Code

---

## 5.2 Branch List (GET List Screen)

**Changes:**

- Remove Branch Type Column
- Remove City and State Column

**Actions Column:**

- View (Specific Branch Detail Screen)
- Edit
- Delete (Soft Delete → Mark as Inactive)

---

## 5.3 Edit Branch Screen

Same as Add Screen.

Change:

- Replace “Created By” with “Edited By”

---

## 5.4 View Branch Details Screen

Two Tabs:

### Tab 1: Branch Information

- Branch Name
- Address
- Branch Code
- Status
- Created/Edited Details

### Tab 2: Branch Employees List

Columns:

- Employee ID
- Employee Name
- Email ID
- Contact Number
- Designation
- Staff Status
- Branch Name

--- -->

# UI/UX Documentation

> Note:
>
> - "Missing" mentioned in the raw input indicates **New Screen / New Module to be developed from scratch**.
> - All other items are enhancements or modifications to the existing system.

=========================================================================

# Module 1: Authentication & Onboarding

---

## 1.1 Super Admin Authentication (Seravion Login)

### Screen: Super Admin Login (Existing – UI Modification Required)

**Fields:**

- Username
- Password
- Submit (Login)

**Changes Required:**

- Remove "Forgot Password"
- Remove "Sign Up" option
- Dedicated login access for Seravion only

---

## 1.2 Company Admin Authentication (Client Side)

### A. Company Admin Sign Up (Existing – UI Modification Required)

**Fields:**

- Company Name
- Authorized Person Name
- Phone
- Email
- Password
- Re-enter Password
- Get Started (CTA Button)

**UI Changes:**

- Remove Google login button
- No social authentication

---

### B. Company Admin Login (Existing – Standard Login)

**Fields:**

- Email / Username
- Password
- Submit (Login)

---

# Module 2: Company Onboarding & Subscription Flow

(Triggered After Company Admin Sign Up / First Login)

---

## 2.1 Company Information Screen

**Type:** Enhancement to Existing Onboarding Screen

**Fields:**

- Company Name
- Industry Type (Dropdown)
- Contact Person Name
- Contact Person Email
- Contact Person Phone
- GST Number
- PAN Number
- Address Fields

**Changes:**

- License Number → Optional (Not Required)
- Save & Continue CTA

---

## 2.2 Document Upload Screen

**Type:** Existing Screen – Enhancement

**Upload Fields:**

- GST Certificate
- PAN Card
- Business / Registration Certificate

---

## 2.3 Document Success Screen

**Type:** Exisiting Screen

- Show submission success message
- Inform user that approval is pending from Seravion side

---

## 2.4 Approval Pending Waiting Screen

**Type:** New Screen (To Be Developed from Scratch)

- Restrict dashboard access
- Popup message:
  “Your access will be approved by Seravion shortly.”

---

## 2.5 Document Rejected Screen

**Type:** Exisiting Screen

- Show rejection reason
- Allow document re-upload
- Resubmit for review
- Flow: Reject → Reupload → Review → Approve → Subscription Screen

---

## 2.6 Subscription Screen (Client Side)

**Type:** Existing Screen

**Elements:**

- Plan Selection Dropdown
- Plan Details (Short Description)
- Per Branch Price
- Per Technician Price
- Duration:
  - Monthly
  - Quarterly
  - Yearly
- Pay Now Button

---

## 2.7 Subscription Module (Sidebar – Company Admin)

**Type:** New Module (To Be Developed from Scratch)

**Features:**

- Subscription Purchased List (Logs)
- View Subscription Details
- Purchase New Plan
- Payment Status:
  - Paid
  - Partial Paid
  - Free
  - Non Paid

---

# Module 3: Role-Based Access Control (RBAC)

---

## 3.1 Company Admin – Role Management

**Type:** New Module (To Be Developed from Scratch)

**Sidebar Entry Required**

### Add Role

**Fields:**

- Select Base Role (Created by Seravion)
- Clone Permissions from Existing Role
- Role Name

### Permission Matrix (25+ Modules)

Each module will have:

- [ ] Add
- [ ] View
- [ ] Edit
- [ ] Delete
- [ ] Export
- [ ] Configure
- [ ] Approval Authority

### Example Permission Matrix

Module 1 – User Management  
[ ] Add  
[ ] View  
[ ] Edit  
[ ] Delete  
[ ] Export  
[ ] Configure  
[ ] Approval Authority

Module 2 – Branch Management  
[ ] Add  
[ ] View  
[ ] Edit  
[ ] Delete  
[ ] Export  
[ ] Configure  
[ ] Approval Authority

### Actions:

- Save Role
- Roles List
- View Role Permissions
- Edit (With Warning)
- Delete (Warning if users assigned)
- Migrate Role to Another Role

---

## 3.2 Super Admin – Role Management

**Type:** New Module (To Be Developed from Scratch)

Same structure as Company Admin Role Management.

---

# Module 4: Super Admin Panel (Seravion Side)

---

## 4.1 Sidebar (Missing – New Sidebar elements to Be Created)

**Type:** New Sidebar Structure for Seravion

Modules:

- Dashboard
- Subscription Plan Management
- Reports
- Role Management

---

## 4.2 Dashboard Enhancements

**Type:** Existing Screen – Modification Required

Changes:

- Rename “Status” → “Document Status”
- Add Payment Status:
  - Paid
  - Partial Paid
  - Free
  - Non Paid

---

## 4.3 Pariticular Customer Detail Popup

**Type:** Enhancement

Add:

- Status Dropdown:
  - Approved
  - Pending
  - Rejected
- Trial Checkbox
  - From Date – To Date

---

## 4.4 Pariticular Customer Document Popup Section

**Type:** Enhancement

Add:

- Subscription Purchase Details
- Payment History Summary

---

## 4.5 Subscription Plan Management

**Type:** New Module (To Be Developed from Scratch)

### Add Plan

**Fields:**

- Plan Name
- Per Branch Pricing
- Per Technician Pricing
- Description
- Duration:
  - Monthly
  - Quarterly
  - Yearly

### Actions:

- View Plan List
- View Plan Detail
- Edit
- Delete

---

# Module 5: Branch Management (Client Side)

(Changes Only – Existing Module Enhancement)

---

## 5.1 Add Branch Screen

**Type:** Enhancement

**Branch Types (Enum):**

- HEAD_OFFICE
- STATE_BRANCH
- CITY_BRANCH
- WAREHOUSE

**Fields:**

- Branch Name
- Address Fields
- 3-Letter Branch Code
- Created By

**Required Fields:**

- Name
- Address
- 3-Letter Code

---

## 5.2 Branch List Screen

**Type:** Modification

Changes:

- Remove Branch Type Column
- Remove City & State Columns

**Actions:**

- View
- Edit
- Delete (Soft Delete – Mark Inactive)

---

## 5.3 Edit Branch Screen

**Type:** Modification

- Same as Add
- Replace "Created By" with "Edited By"

---

## 5.4 Branch Detail View Screen

**Type:** Enhancement

### Tab 1 – Branch Information

- Name
- Address
- Branch Code
- Status
- Created/Edited Details

### Tab 2 – Branch Employees List

- Employee ID
- Employee Name
- Email
- Contact Number
- Designation
- Staff Status
- Branch Name

---
