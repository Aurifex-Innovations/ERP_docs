# Module 8: User and Hiring Request Module API Documentation

## Short Description

The User and Hiring Request Module handles the complete employee lifecycle and hiring workflow. It encompasses employee listing, multi-branch attendance/assignment, and a detailed 5-step onboarding process (Personal, Permissions, Salary, Leave, and Documents). Additionally, it manages the internal hiring request system, allowing managers to request headcount and admins to approve or reject those requests with detailed justifications.

---

## We Provided Requests for APIs into Postman Take it from There and Take Enum Value From Here, Mentioned at last in Documentation.

## If you faced any issue then connect with aryan.

---

## APIs

### List Employees (Tab 1)

- **Method:** `GET`
- **Endpoint:** `/api/v1/users`
- **Purpose:** Fetch a paginated list of employees with advanced filtering.
- **Use Case:** Viewing organization-wide staff or specific branch/department teams.
- **Used In:** Employee List Screen (Tab 1).
- **Request Query Params:**
  - `branchIds` (List<String>): Filter by one or more branch IDs.
  - `department` (String): Filter by department name.
  - `designation` (String): Filter by job title.
  - `roleId` (Long): Filter by assigned RBAC role.
  - `status` (String): Filter by status (`ACTIVE`, `INACTIVE`, `DELETED`).
  - `createdFrom` / `createdTo` (Date): Filter by employee creation date.
  - `search` (String): Search by name, email, or employee ID.
  - `pageNo` / `pageSize`: Pagination control.
- **Response:** `PaginationResponse<UserListResponse>`
- **Access / Security:** `EMPLOYEE_USER_MANAGEMENT_VIEW` permission or `ROLE_CEO`.
- **Notes:** Supports soft-delete filtering (default excludes `DELETED` unless specified).

### Create Employee (Add User - 5 Steps)

- **Method:** `POST`
- **Endpoint:** `/api/v1/users`
- **Purpose:** Onboard a new employee with full profile configuration.
- **Use Case:** Adding a new staff member to the system.
- **Used In:** Add User Multi-step Form (Steps 1-5).
- **Request Body:** `CreateUserRequest`
  - **Step 1:** Personal & Employment Info (Emp ID, Role, Branch, Reporting Manager).
  - **Step 2:** Module-specific Permission Overrides.
  - **Step 3:** Salary & Bank Details (CTC/Fixed, PF/ESI, OT config).
  - **Step 4:** Leave Configuration (CL, SL, PL, Reset Cycle).
  - **Step 5:** Additional Data & Document Metadata.
- **Response:** `UserResponse` (Full employee profile).
- **Access / Security:** `EMPLOYEE_USER_MANAGEMENT_ADD` permission or `ROLE_CEO`.

### Update Employee (Edit User)

- **Method:** `PUT`
- **Endpoint:** `/api/v1/users`
- **Purpose:** Modify an existing employee's profile.
- **Use Case:** Updating designation, salary, or permissions after appraisal or transfer.
- **Used In:** Edit User Form.
- **Request Params:** `id` (Long) - Database ID of the user.
- **Request Body:** `UpdateUserRequest`
- **Response:** `UserResponse`
- **Access / Security:** `EMPLOYEE_USER_MANAGEMENT_EDIT` permission or `ROLE_CEO`.
- **Notes:** **EMP ID** is immutable and cannot be changed via this API.

### View Employee Details

- **Method:** `GET`
- **Endpoint:** `/api/v1/users/by-id`
- **Purpose:** Retrieve complete profile details for a specific employee.
- **Use Case:** Viewing the 5-step data in read-only mode.
- **Used In:** Employee Detail View Popup.
- **Request Params:** `id` (Long)
- **Response:** `UserResponse` (Contains nested objects for salary, leave, and permissions).
- **Access / Security:** `EMPLOYEE_USER_MANAGEMENT_VIEW` permission or `ROLE_CEO`.

### Soft-Delete Employee

- **Method:** `DELETE`
- **Endpoint:** `/api/v1/users`
- **Purpose:** Inactivate an employee and hide them from primary lists.
- **Use Case:** Offboarding an employee.
- **Used In:** Table Action (Delete).
- **Request Params:** `id` (Long)
- **Response:** Standard Success Structure.
- **Access / Security:** `EMPLOYEE_USER_MANAGEMENT_DELETE` permission or `ROLE_CEO`.

### Download Employee Document

- **Method:** `GET`
- **Endpoint:** `/api/v1/users/documents/download`
- **Purpose:** Stream internal documents (Aadhar, PAN, Experience Letters).
- **Use Case:** Compliance audit or HR verification.
- **Used In:** View Employee → Documents Tab.
- **Request Params:** `documentId` (Long)
- **Response:** Binary Stream (Resource).
- **Access / Security:** `EMPLOYEE_USER_MANAGEMENT_DOWNLOAD` permission or `ROLE_CEO`.

### Create Hiring Request (Tab 2)

- **Method:** `POST`
- **Endpoint:** `/api/v1/hiring-requests`
- **Purpose:** Create a formal request for new talent.
- **Use Case:** A Branch Manager requesting 2 new Sales Executives.
- **Used In:** New Hiring Request Form.
- **Request Body:** `CreateHiringRequest` (Includes JD, Reason, and Target Recipients).
- **Response:** `HiringRequestDetailResponse`
- **Access / Security:** `EMPLOYEE_USER_MANAGEMENT_REQUEST` permission or `ROLE_CEO`.

### List My Hiring Requests (Tab 2)

- **Method:** `GET`
- **Endpoint:** `/api/v1/hiring-requests/my`
- **Purpose:** List hiring requests submitted by the current user.
- **Use Case:** Checking if requested headcount is approved.
- **Used In:** Hiring Requests Dashboard (Tab 2).
- **Request Query Params:** Status, Department, Dates, Branch, Search.
- **Response:** `PaginationResponse<HiringRequestListResponse>`

### List Received Hiring Requests (Tab 3)

- **Method:** `GET`
- **Endpoint:** `/api/v1/hiring-requests/received`
- **Purpose:** List hiring requests requiring the user's review.
- **Use Case:** HR Manager reviewing requests from various branches.
- **Used In:** Received Requests Dashboard (Tab 3).
- **Request Query Params:** `status`, `department`, `branchId`, `requestedByUserId`, `employmentType`, Search.
- **Response:** `PaginationResponse<HiringRequestListResponse>`
- **Access / Security:** `EMPLOYEE_USER_MANAGEMENT_APPROVE` permission or `ROLE_CEO`.

### Get Hiring Request Detail

- **Method:** `GET`
- **Endpoint:** `/api/v1/hiring-requests`
- **Purpose:** Get full justification and JD of a hiring request.
- **Use Case:** Pre-approval review.
- **Used In:** Hiring Request Detail Popup.
- **Request Params:** `id` (String - HRQ ID)
- **Response:** `HiringRequestDetailResponse`

### Approve/Reject Hiring Request

- **Method:** `PATCH`
- **Endpoint:** `/api/v1/hiring-requests/verify`
- **Purpose:** Final decision on a pending request.
- **Use Case:** HR Head approving a request with a comment.
- **Used In:** Detail View Action Buttons.
- **Request Params:**
  - `id` (String): Request ID.
  - `status` (String): `APPROVED` or `REJECTED`.
  - `rejectionReason` (String): Mandatory if status is `REJECTED`.
- **Response:** `HiringRequestDetailResponse`
- **Access / Security:** `EMPLOYEE_USER_MANAGEMENT_APPROVE` permission or `ROLE_CEO`.

---

## Reusable / Common APIs (Used in this Module)

### Eligible Reporting Managers Dropdown

- **Method:** `GET`
- **Endpoint:** `/api/v1/users/managers/dropdown` (Used in Hiring Request Module Creation Time in Screen two.)
- **Purpose:** Fetches a list of active users filtered by role hierarchy to serve as managers.
- **Use Case:** Selecting a manager during Step 1 of User Creation.
- **Used In:** Add User Screen (Step 1).

---

## Module Enums

### SalaryType

- **Used In:** `CreateUserRequest` (SalaryDetails)
- **Values:**
  - `CTC`
  - `FIXED`
  - `HOURLY`

### LeaveResetCycle

- **Used In:** `CreateUserRequest` (LeaveDetails)
- **Values:**
  - `YEARLY`
  - `MONTHLY`
  - `CUSTOM`

### EmploymentType

- **Used In:** `CreateUserRequest`, `CreateHiringRequest`
- **Values:**
  - `PERMANENT`
  - `CONTRACT`
  - `INTERN`

### HiringRequestStatus

- **Used In:** Hiring Request APIs
- **Values:**
  - `PENDING`
  - `APPROVED`
  - `REJECTED`

---

## Notes for Frontend

- **Multi-Step Form Logic:** The `POST /api/v1/users` expects the entire multi-step payload in one go. You may choose to validate steps locally or send a partial draft if the backend supports it (currently full validation is applied).
- **Dropdown Dependency:** The "Reporting Manager" dropdown should be re-fetched if the "Branch" or "Role" selection changes to ensure hierarchy compliance.
- **List Filtering:** All list APIs support pagination (0-indexed `pageNo`) and standard ISO date formats for range filters.
- **File Uploads:** In Hiring Requests, the file data should be sent as Base64 in the `fileData` field of the request body along with `fileName` and `contentType`.
- **RBAC Enforcement:** Button visibility for "Add User" or "Approve Request" should be toggled based on the presence of `EMPLOYEE_USER_MANAGEMENT_ADD` and `EMPLOYEE_USER_MANAGEMENT_APPROVE` permissions respectively.
