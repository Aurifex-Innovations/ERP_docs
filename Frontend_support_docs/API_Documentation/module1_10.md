# API Documentation

## Table of Contents

- [Module 1: Authentication](#module-1-authentication)
- [Module 2: User Onboarding](#module-2-user-onboarding)
- [Module 3: Super Admin Management](#module-3-super-admin-management)
- [Module 4: Subscription Plans](#module-4-subscription-plans)
- [Module 5: Role Management](#module-5-role-management)
- [Module 6: Role Salary & Leave Configuration](#module-6-role-salary--leave-configuration)
- [Module 7: Branch Management](#module-7-branch-management)
- [Module 8: Employee Management](#module-8-employee-management)
- [Module 9: Tax Management](#module-9-tax-management)
- [Module 10: Inventory & Services](#module-10-inventory--services)

---

## MODULE 1: AUTHENTICATION

### 1.1 Super Admin (Seravion) Authentication Flow

**Endpoint:** `POST /api/v1/auth/root/login`

**Description:** Authenticates the Super Admin (Seravion) user and provides access to the administrative dashboard.

---

### 1.2 Company Admin (Client) Sign Up Flow

**Endpoint:** `POST /api/v1/auth/signup`

**Description:** Registers a new Company Admin account. After submitting this request, a verification link is sent to the registered email address. The account remains disabled until the email is verified.

**Email Verification:**

- **Endpoint:** `GET /api/v1/auth/verify-email?token={{authorization-secret-uuid}}`
- **Description:** This link is provided in the verification email. Clicking it enables the account.

---

### 1.3 Login Option Screen

**Description:** UI/UX specific screen with no dedicated API endpoint.

---

### 1.4 Company Admin (Client) Login Flow

**Endpoint:** `POST /api/v1/auth/ceo/login`

**Description:** Authenticates the Company Admin (CEO) and grants access to the company management dashboard.

---

### 1.5 IAM User (Company User) Login Flow

**Endpoint:** `POST /api/v1/auth/login`

**Description:** Authenticates IAM users (employees) within the company and provides access based on their assigned roles.

---

### 1.6 Forgot Password (Root User + IAM User)

**Status:** `PENDING`

---

## MODULE 2: USER ONBOARDING

### 2.1 Company Information Fields

**Endpoint:** `POST /api/v1/company-details/submit`

**Description:** Submits company information during the onboarding process.

---

### 2.2 Upload Document Fields

**Endpoint:** `POST /api/v1/company/documents/upload`

**Description:** Uploads required company documents for verification purposes.

---

### 2.3 Document Uploaded Successfully Popup

**Description:** UI/UX specific confirmation popup.

---

### 2.4 Document Verification Pending (Client Facing)

**Description:** UI/UX specific screen indicating that document verification is in progress.

---

### 2.5 Document Rejected Screen (Client Facing)

**Endpoint:** `GET /api/v1/company-details`

**Description:** Retrieves company details including information about rejected documents. This response contains all company fields along with details of unverified documents.

---

### 2.6 Document Reupload (After Rejection)

**Endpoint:** `PUT /api/v1/company/documents/re-upload`

**Description:** Allows reupload of documents that were previously rejected during verification.

---

### 2.7 Document Verification Success Screen (Client Facing)

**Description:** UI/UX specific screen confirming successful document verification.

---

### 2.8 Subscription Screen (Client Side)

**Description:** Multi-step subscription purchase flow.

**Endpoints:**

- `GET /api/v1/root/subscription-plans/dropdown` - Retrieves available subscription plans for dropdown selection
- `GET /api/v1/root/subscription-plans` - According Selected Plan, you can get details using this api.
- `POST /api/v1/company/subscription/calculate` - Calculates the total subscription cost
- `POST /api/v1/company/subscription/create-order` - Creates a Razorpay payment order
- `POST /api/v1/company/subscription/verify-payment` - Verifies payment after transaction completion

---

### 2.9 Subscription Module (Company Admin Sidebar)

**Description:** After successfully purchasing a subscription, the user is redirected to the CEO main dashboard. This is a UI/UX specific screen.

---

### 2.10 Subscription Purchase List (Logs)

**Endpoint:** `POST /api/v1/company/subscription/list`

**Description:** Retrieves a list of all subscription purchase logs for the company.

---

### 2.11 View Subscription Details

**Endpoint:** `GET /api/v1/company/subscription/detail`

**Description:** Retrieves detailed information about the current subscription.

---

## MODULE 3: SUPER ADMIN (SERAVION) MANAGEMENT

### 3.1 Company Management (Seravion Side)

**Endpoint:** `GET /api/v1/super-admin/company-management/list`

**Description:** Retrieves a list of all companies registered on the platform for Super Admin review.

---

### 3.2 View Company Details (Seravion Side) - Before Approval

**Endpoint:** `GET /api/v1/super-admin/company-management/detail`

**Description:** Retrieves detailed information about a company before document approval.

---

### 3.3 View Company Details – After Document Approved

**Endpoint:** `GET /api/v1/super-admin/company-management/approved-detail`

**Description:** Retrieves detailed information about a company after documents have been approved.

**Note:** While there is no dedicated screen for approving company details, the approval can be performed using the following endpoint:

- **Endpoint:** `PUT /api/v1/super-admin/company-management/approval`

---

## MODULE 4: SUBSCRIPTION PLANS

### 4.1 Get All Plans – List View Screen

**Endpoint:** `GET /api/v1/root/subscription-plans/get-all`

**Description:** Retrieves all available subscription plans in a list view format.

---

### 4.2 Add Plan Screen

**Endpoint:** `POST /api/v1/root/subscription-plans`

**Description:** Creates a new subscription plan.

---

### 4.3 Edit Plan Screen

**Endpoint:** `PUT /api/v1/root/subscription-plans`

**Description:** Updates an existing subscription plan.

---

### 4.4 Delete Plan - Warning Popup

**Endpoint:** `DELETE /api/v1/root/subscription-plans/inactivate`

**Description:** Deactivates a subscription plan (soft delete with warning confirmation).

**Additional Endpoints:**

- `GET /api/v1/root/subscription-plans` - Retrieves a subscription plan by ID
- `GET /api/v1/root/subscription-plans/dropdown` - Retrieves subscription plans for dropdown selection

---

## MODULE 5: ROLE MANAGEMENT

### 5.0 Role Management Dashboard

**Endpoints:**

- `GET /api/v1/role/seravion/analytics` - Analytics dashboard for Seravion/Root users
- `GET /api/v1/role/analytics` - Analytics dashboard for CEO users

---

### 5.1 Create Role (Seravion/Super Admin Side)

**Endpoint:** `POST /api/v1/role/create`

**Description:** Creates a new role within the system.

**Additional Endpoints for CEO Role Creation:**
If creating a role from Seravion-provided roles, the following endpoints are required:

- `GET /api/v1/role/dropdown/public` - Retrieves available roles for selection
- `GET /api/v1/role/permissions/public` - Retrieves all permissions associated with the selected role

**Note:** The request payload should still be structured according to requirements.

---

### 5.2 Edit Role

**Endpoint:** `PUT /api/v1/role/update`

**Description:** Updates an existing role's information and permissions.

---

### 5.3 View Role Detail

**Endpoint:** `GET /api/v1/role`

**Description:** Retrieves detailed information about a specific role.

---

### 5.4 Delete Role (CEO Only)

**Endpoint:** `DELETE /api/v1/role/delete`

**Description:** Deletes a role. This action is restricted to CEO users only.

---

### 5.5 Role Management

**Endpoint:** `POST /api/v1/role/create`

**Description:** General role creation endpoint.

**Workflow:**

- If creating a role from Seravion-provided roles:
  - `GET /api/v1/role/dropdown/public` - Select from existing roles
  - `GET /api/v1/role/permissions/public` - Retrieve permissions for the selected role
- Otherwise, create a new custom role directly

**Note:** Request payload must be structured according to requirements regardless of the workflow chosen.

---

## MODULE 6: ROLE SALARY & LEAVE CONFIGURATION

### 6.1 Role Salary & Leave Configuration List (Client Side)

**Endpoint:** `GET /api/v1/role-compensations/get-all`

**Description:** Retrieves a list of all salary and leave configurations associated with roles.

---

### 6.2 Add Configuration Form (Multi-Step)

**Endpoint:** `POST /api/v1/role-compensations`

**Description:** Creates a new salary and leave configuration for a role through a multi-step form.

---

### 6.3 Edit Configuration Form

**Endpoint:** `PUT /api/v1/role-compensations/update`

**Description:** Updates an existing role salary and leave configuration.

---

### 6.4 View Configuration Details

**Endpoint:** `GET /api/v1/role-compensations`

**Description:** Retrieves detailed information about a specific role compensation configuration.

---

## MODULE 7: BRANCH MANAGEMENT

### 7.1 Branch List Screen

**Endpoint:** `GET /api/v1/company/branches`

**Description:** Retrieves a list of all branches for the company.

---

### 7.2 Add Branch Screen

**Endpoint:** `POST /api/v1/company/branches`

**Description:** Creates a new branch for the company.

---

### 7.3 Edit Branch Screen

**Endpoint:** `PUT /api/v1/company/branches/update`

**Description:** Updates information for an existing branch.

---

### 7.4 Delete Branch (Soft Delete)

**Endpoint:** `DELETE /api/v1/company/branches`

**Description:** Performs a soft delete on a branch, marking it as inactive without permanent removal.

---

### 7.5 View Particular Branch Details

#### 7.5.1 Branch Information (Tab 1)

**Endpoint:** `GET /api/v1/company/branches/by-id`

**Description:** Retrieves detailed information about a specific branch.

#### 7.5.2 Branch Users / Employees (Tab 2)

**Status:** `PENDING`

---

## MODULE 8: EMPLOYEE (USERS) MANAGEMENT

### 8.1 Tab 1: Employee (User) List

**Endpoint:** `GET /api/v1/users`

**Description:** Retrieves a list of all employees/users in the company.

---

### 8.2 Tab 2: My Hiring Requests (Table View)

**Endpoint:** `GET /api/v1/hiring-requests/my`

**Description:** Retrieves all hiring requests created by the current user.

#### 8.2.1 My Hiring Request View (Popup)

**Endpoint:** `GET /api/v1/hiring-requests`

**Description:** Displays detailed information about a specific hiring request in a popup view.

---

### 8.3 Tab 3: Received Requests to Add New Employee

**Endpoint:** `GET /api/v1/hiring-requests/received`

**Description:** Retrieves all hiring requests received by the current user for approval.

---

### 8.4 Request Form Fields

**Description:** Form for creating a new hiring request.

**Supporting Endpoints:**

- `GET /api/v1/company/branches/dropdown` - Retrieves branch options for dropdown
- `GET /api/v1/role/dropdown` - Retrieves role options for dropdown

#### 8.4.2 Select Request Recipients

**Endpoint:** `GET /api/v1/users/managers/dropdown`

**Description:** After filling out the hiring request form, this popup allows selection of managers to whom the request should be sent.

**Submit Hiring Request:**

- **Endpoint:** `POST /api/v1/hiring-requests`
- **Description:** Submits the completed hiring request to the selected recipients.

---

### 8.5 Request Detail View (Popup)

**Endpoint:** `GET /api/v1/hiring-requests`

**Description:** Displays comprehensive details of a hiring request in a popup view.

#### 8.5.2 Reject Request Popup (Triggered from Reject Button)

**Endpoint:** `PATCH /api/v1/hiring-requests/verify`

**Description:** Allows the recipient to reject a hiring request with appropriate feedback.

---

### 8.6 Add User Form (Multi-Step)

**Endpoint:** `POST /api/v1/users`

**Description:** Creates a new user/employee account through a multi-step form process.

---

### 8.7 Edit Employee Form

**Endpoint:** `PUT /api/v1/users`

**Description:** Updates information for an existing employee.

---

### 8.8 View Employee Screen

**Endpoint:** `GET /api/v1/users/by-id`

**Description:** Retrieves detailed information about a specific employee.

**Supporting Endpoints:**

- `GET /api/v1/users/documents/download` - Downloads employee documents

**Additional Actions:**

- `DELETE /api/v1/users` - Deactivates a user account

---

## MODULE 9: TAX MANAGEMENT

### 9.1 Tax Types Master – Table View

**Endpoint:** `GET /api/v1/tax-types`

**Description:** Retrieves a list of all tax types configured in the system.

---

### 9.2 Add Tax Type

**Endpoint:** `POST /api/v1/tax-types`

**Description:** Creates a new tax type in the system.

---

### 9.3 Edit Tax Type

**Endpoint:** `PUT /api/v1/tax-types/update`

**Description:** Updates an existing tax type configuration.

---

### 9.4 View Tax Type

**Endpoint:** `GET /api/v1/tax-types/by-id`

**Description:** Retrieves detailed information about a specific tax type.

---

### 9.5 Delete Tax Type

**Endpoint:** `DELETE /api/v1/tax-types`

**Description:** Removes a tax type from the system.

---

### 9.6 HSN Code Master – Table View

**Endpoint:** `GET /api/v1/tax/hsn-codes`

**Description:** Retrieves a list of all HSN (Harmonized System of Nomenclature) codes.

---

### 9.7 Add HSN Code

**Endpoint:** `POST /api/v1/tax/hsn-codes`

**Description:** Creates a new HSN code entry.

**Supporting Endpoints:**

- `GET /api/v1/tax-types/dropdown` - Retrieves tax types for dropdown selection

---

### 9.8 Edit HSN Code

**Endpoint:** `PUT /api/v1/tax/hsn-codes`

**Description:** Updates an existing HSN code.

**Supporting Endpoints:**

- `GET /api/v1/tax-types/dropdown` - Retrieves tax types for dropdown selection

---

### 9.9 View HSN Code

**Endpoint:** `GET /api/v1/tax/hsn-codes/by-id`

**Description:** Retrieves detailed information about a specific HSN code.

---

### 9.10 Delete HSN Code

**Endpoint:** `DELETE /api/v1/tax/hsn-codes`

**Description:** Removes an HSN code from the system.

---

## MODULE 10: INVENTORY & SERVICES

### 10.1 Product Management – Table View

**Endpoint:** `GET /api/v1/inventory-products`

**Description:** Retrieves a list of all products in the inventory.

---

### 10.2 Add Product

**Endpoint:** `POST /api/v1/inventory-products`

**Description:** Creates a new product in the inventory.

**Supporting Endpoints:**

- `GET /api/v1/tax/hsn-codes/dropdown` - Retrieves HSN codes for dropdown selection
- `GET /api/v1/tax/hsn-codes/products` - Retrieves tax details based on selected HSN code

---

### 10.3 Edit Product

**Endpoint:** `PUT /api/v1/inventory-products`

**Description:** Updates an existing product's information.

**Supporting Endpoints:**

- `GET /api/v1/tax/hsn-codes/dropdown` - Retrieves HSN codes for dropdown selection
- `GET /api/v1/tax/hsn-codes/products` - Retrieves tax details based on selected HSN code

---

### 10.4 View Product

**Endpoint:** `GET /api/v1/inventory-products/by-id`

**Description:** Retrieves detailed information about a specific product.

---

### 10.5 Delete Product

**Endpoint:** `DELETE /api/v1/inventory-products/delete`

**Description:** Removes a product from the inventory.

---

## Notes

- All endpoints require proper authentication unless otherwise specified
- Error responses follow standard HTTP status codes
- Date formats should follow ISO 8601 standard
- All timestamps are in UTC unless otherwise specified

---

**Document Version:** 1.0  
**Last Updated:** April 2026
