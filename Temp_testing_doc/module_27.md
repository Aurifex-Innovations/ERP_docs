You are an **ERP Solution Architect** designing **MODULE 27 – USER PROFILE** for a Pest Control ERP-CRM system.

Users in the system are **employees and technicians created in Employee Management and IAM modules**. The profile module must display all important information about a user in a structured format.

The goal of this module is to **show a complete profile of a user**, including personal information, role, branch, salary, documents, permissions, and activity references.

The profile must be structured **section-wise** so that even a beginner user can easily understand the information.

Below are the sections and fields that must be included.

---

SECTION 1 — BASIC USER INFORMATION

Purpose
This section stores the main identity details of the user.

Fields

EMP ID
Purpose: Unique employee identifier used across the system.

First Name
Purpose: User's first name.

Last Name
Purpose: User's last name.

Full Name (Auto)
Purpose: Auto-generated from First Name + Last Name.

Email
Purpose: Official email address used for communication.

Contact Number
Purpose: Primary phone number of the employee.

Alternate Number
Purpose: Backup contact number.

Account ID
Purpose: Login ID used for application authentication.

Password (Masked)
Purpose: Credential used for login authentication.

Status
Purpose: Shows whether the employee is Active or Inactive.

Date of Joining
Purpose: The date the employee joined the company.

Employment Type
Purpose: Shows if the employee is Permanent, Contract, or Intern.

---

SECTION 2 — ORGANIZATION INFORMATION

Purpose
Shows how the user is positioned inside the company structure.

Fields

Department
Purpose: Business unit where the employee works (Operations, Sales, Admin etc).

Designation
Purpose: Job title of the employee.

Role
Purpose: System role which controls permissions.

Branch
Purpose: Branch where the employee is assigned.

Reporting Manager
Purpose: Manager or supervisor responsible for the employee.

Technician Group / Team
Purpose: Used to group technicians for task assignment.

Is Application User (Checkbox)
Purpose: Defines whether the user can access the mobile application. If enabled, module permissions are disabled and the user only uses the mobile app.

---

SECTION 3 — ADDRESS INFORMATION

Purpose
Stores location details of the employee.

Fields

Current Address Line 1
Purpose: Primary address line.

Current Address Line 2
Purpose: Additional address information.

City
Purpose: City where the employee resides.

State
Purpose: State location.

Country
Purpose: Country (default India).

Pincode
Purpose: Postal code for location identification.

Permanent Address
Purpose: Permanent residential address if different from current address.

Same as Current Address (Checkbox)
Purpose: Auto-copy address fields if both addresses are same.

---

SECTION 4 — ROLE & PERMISSION INFORMATION

Purpose
Shows access rights of the user inside the ERP.

Fields

Role Name
Purpose: System role assigned to the user.

Role Description
Purpose: Explains what the role represents.

Module Permissions
Purpose: List of modules where the user has View, Create, Edit, or Delete access.

Request Receiver Roles
Purpose: Defines which approval requests the user receives.

Is Application User
Purpose: If enabled, the user only accesses the mobile app and does not have full ERP permissions.

---

SECTION 5 — SALARY INFORMATION

Purpose
Stores employee compensation details.

Fields

Salary Type
Purpose: Defines payment model (CTC / Fixed / Hourly).

Basic Salary
Purpose: Base monthly salary.

HRA
Purpose: House Rent Allowance.

Other Allowance
Purpose: Additional company allowances.

Incentive
Purpose: Performance-based bonus.

Deductions
Purpose: Salary deductions such as tax or benefits.

PF Applicable
Purpose: Indicates if Provident Fund deduction applies.

ESI Applicable
Purpose: Indicates if Employee State Insurance applies.

TDS Applicable
Purpose: Indicates if tax deduction applies.

---

SECTION 6 — BANK INFORMATION

Purpose
Stores employee payment details.

Fields

Bank Name
Purpose: Bank used for salary payments.

Account Number
Purpose: Employee bank account number.

Account Holder Name
Purpose: Name on bank account.

IFSC Code
Purpose: Bank branch identifier.

UPI ID
Purpose: Digital payment identifier.

---

SECTION 7 — DOCUMENTS

Purpose
Stores uploaded documents related to the employee.

Fields

Government ID Proof
Purpose: Aadhaar / PAN or identity document.

Address Proof
Purpose: Residential verification document.

Employment Contract
Purpose: Signed employment agreement.

Education Certificates
Purpose: Qualification verification.

Other Documents
Purpose: Any additional employee documentation.

Each document must support file upload and download.

---

SECTION 8 — LEAVE & ATTENDANCE SUMMARY

Purpose
Shows HR related activity of the employee.

Fields

Total Leave Balance
Purpose: Remaining leave available.

Leaves Taken
Purpose: Number of leaves used.

Attendance Status
Purpose: Current day attendance status.

Punch In Time
Purpose: Employee login time.

Punch Out Time
Purpose: Employee logout time.

---

SECTION 9 — ACTIVITY & SYSTEM HISTORY

Purpose
Tracks actions performed by the user in the system.

Fields

Created By
Purpose: User who created the employee record.

Created Date
Purpose: Date when the profile was created.

Last Modified By
Purpose: User who last edited the profile.

Last Modified Date
Purpose: Timestamp of last modification.

Audit Trail
Purpose: List of system changes performed by or on the user record.

---

FINAL GOAL

The User Profile module must present a **complete 360-degree view of the employee or technician** including:

Personal details
Organization role
Branch assignment
Permissions
Salary configuration
Documents
Attendance information
Audit history

All fields must link to existing ERP modules such as Employee Management, IAM, HRM Attendance, Salary Configuration, and Branch Management.

Generate database structure, relationships, and validation rules accordingly.