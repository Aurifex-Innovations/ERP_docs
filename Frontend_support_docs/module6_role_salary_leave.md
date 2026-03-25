# Role Salary & Leave Configuration (Role Compensation) API Documentation

## Short Description

The Role Salary & Leave Configuration module (Role Compensation) manages the financial and leave-related policies for different organizational roles. It allows administrators to define salary structures (CTC, Fixed, or Hourly), specify incentive and overtime rules, and configure leave entitlements, including accrual and carry-forward policies. These configurations are then automatically applied during employee onboarding (Module 8).

---

## We Provided Requests for APIs into Postman Take it from There and Take Enum Value From Here, Mentioned at last in Documentation.

## If you faced any issue then connect with aryan.

---

## APIs

### Create Configurations

- **Method:** `POST`
- **Endpoint:** `/api/v1/role-compensations`
- **Purpose:** Creates role compensation configurations for one or more selected roles.
- **Use Case:** Initial setup of salary and leave rules for new or existing roles in the system.
- **Used In:** Add Role Configuration Form (Step 1-3).
- **Request Body:** `RoleCompensationRequestDto`
- **Response:** `List<RoleCompensationResponseDto>`
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('RSL_MANAGEMENT_ADD')`
- **Notes:** Supports multi-role selection for batch configuration.

### Update Configuration

- **Method:** `PUT`
- **Endpoint:** `/api/v1/role-compensations/update`
- **Purpose:** Updates an existing role compensation configuration.
- **Use Case:** Adjusting salary components, leave days, or incentive rules for a role.
- **Used In:** Edit Configuration Screen.
- **Request Params:** `configId` (String)
- **Request Body:** `RoleCompensationRequestDto`
- **Response:** `RoleCompensationResponseDto`
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('RSL_MANAGEMENT_EDIT')`
- **Notes:** Role selection is typically disabled during update.

### Get Configuration Details

- **Method:** `GET`
- **Endpoint:** `/api/v1/role-compensations`
- **Purpose:** Retrieves read-only details of a specific configuration
- **Use Case:** Viewing the complete breakdown of salary and leave settings for a specific configuration ID .
- **Used In:** View Configuration Details Screen.
- **Query Params:** `configId` (String)
- **Response:** `RoleCompensationResponseDto`
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('RSL_MANAGEMENT_READ')`

### Get Active Configuration by Role ID

- **Method:** `GET`
- **Endpoint:** `/api/v1/role-compensations/role/active`
- **Purpose:** Fetches the currently active configuration mapped to a specific role. (Used in User Module for getting role base compensation Details on Creation Time.)
- **Use Case:** Automatically fetching default salary and leave settings when creating a new employee for a specific role.
- **Used In:** User Management (Module 8) - Step 3 & 4 of employee creation.
- **Query Params:** `roleId` (Long)
- **Response:** `RoleCompensationResponseDto`
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('RSL_MANAGEMENT_READ')`

### Get All Configurations (List API)

- **Method:** `GET`
- **Endpoint:** `/api/v1/role-compensations/get-all`
- **Purpose:** Fetches all defined compensation records with pagination and filtering support.
- **Use Case:** Monitoring all configured roles, filtering by status or salary type, and searching for specific roles.
- **Used In:** Role Salary & Leave Configuration List Screen.
- **Query Params:**
  - `status` (Enum: CompensationStatus)
  - `salaryType` (Enum: SalaryType)
  - `startDate` (LocalDate)
  - `endDate` (LocalDate)
  - `search` (String - search by role name)
  - `pageNo` (Integer, default 0)
  - `pageSize` (Integer, default 10)
- **Response:** `PaginationResponse<RoleCompensationResponseDto>`
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('RSL_MANAGEMENT_READ')`

---

## Reusable / Common APIs (Used in this Module)

### Get Role Dropdown List

- **Method:** `GET`
- **Endpoint:** `/api/v1/role/dropdown`
- **Purpose:** Fetches active roles for dropdown selection.
- **Use Case:** Populating the "Select Role" field in the configuration form and the "Leave Approval Authority" selection.
- **Used In:** Add/Edit Configuration Form (Step 1 and Step 3).

---

## Module Enums

### CompensationStatus

- **Used In:** RoleCompensationRequestDto, RoleCompensationResponseDto, List Filtering
- **Values:**
  - `ACTIVE`
  - `INACTIVE`

### SalaryType

- **Used In:** SalaryDetailsDto, List Filtering
- **Values:**
  - `CTC`
  - `FIXED`
  - `HOURLY`

### HolidayWorkType

- **Used In:** IncentiveDetailsDto
- **Values:**
  - `FIXED`
  - `PER_DAY`
  - `PER_HOUR`

### OvertimeType

- **Used In:** IncentiveDetailsDto
- **Values:**
  - `PER_HOUR`
  - `PER_DAY`

### OvertimeShiftType

- **Used In:** IncentiveDetailsDto
- **Values:**
  - `NIGHT`
  - `NORMAL`
  - `CUSTOM`

### LeaveResetCycle

- **Used In:** LeaveDetailsDto
- **Values:**
  - `YEARLY`
  - `MONTHLY`
  - `CUSTOM`

---

## Notes for Frontend

- **Dropdown APIs:**
  - Use `/api/v1/role/dropdown` for selecting roles during configuration.
  - Use metadata from Enums for Salary Type, Status, Holiday Work Type, etc.
- **List APIs:**
  - `/api/v1/role-compensations/get-all` provides paginated data for the main table.
- **Detail APIs:**
  - `/api/v1/role-compensations?configId=...` should be used for the read-only view.
  - `/api/v1/role-compensations/role/active?roleId=...` is critical for Module 8 (Employee Creation) to auto-fill forms.
- **Create/Update:**
  - Configuration is split into 3 steps in the UI but saved as a single object (Request DTO).
  - Multi-select in Step 1 allows applying the same rules to multiple roles at once.
- **Enum usage in forms:** All Enums listed above should be rendered as dropdowns or radio selections in the multi-step form.
