# Branch Management (Module 7) API Documentation

## Short Description

The Branch Management module handles the creation, organization, and maintenance of physical or operational locations within a company. It supports a hierarchical structure for branches (Head Office, State/City branches, Warehouses) and manages employee associations, operational tracking, and location-based reporting. This module is essential for resource allocation and multi-location operations.

---

## We Provided Requests for APIs into Postman Take it from There and Take Enum Value From Here, Mentioned at last in Documentation.

## If you faced any issue then connect with Veerat.

---

## APIs

### Create Branch

- **Method:** `POST`
- **Endpoint:** `/api/v1/company/branches`
- **Purpose:** Registers a new branch for the company.
- **Use Case:** Adding a new retail point, service center, or warehouse to the organizational network.
- **Used In:** Add Branch Screen.
- **Request Body:** `CreateBranchRequest`
- **Response:** `BranchResponse`
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('BRANCH_MANAGEMENT_ADD')`
- **Notes:** Validation ensures unique Branch Code and Email per company.

### Update Branch

- **Method:** `PUT`
- **Endpoint:** `/api/v1/company/branches/update`
- **Purpose:** Modifies existing branch details.
- **Use Case:** Updating contact information, address, or branch type for an existing location.
- **Used In:** Edit Branch Screen.
- **Request Params:** `id` (String - Branch ID)
- **Request Body:** `CreateBranchRequest`
- **Response:** `BranchResponse`
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('BRANCH_MANAGEMENT_EDIT')`
- **Notes:** Branch Code cannot be changed if the branch has active employee assignments.

### Get Branch Details

- **Method:** `GET`
- **Endpoint:** `/api/v1/company/branches/by-id`
- **Purpose:** Fetches comprehensive details of a specific branch.
- **Use Case:** Viewing the complete profile of a branch, including location and contact info.
- **Used In:** View Particular Branch Details (Tab 1: Branch Information).
- **Query Params:** `id` (String - Branch ID)
- **Response:** `BranchResponse`
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('BRANCH_MANAGEMENT_READ')`

### Deactivate Branch (Soft Delete)

- **Method:** `DELETE`
- **Endpoint:** `/api/v1/company/branches`
- **Purpose:** Marks a branch as `INACTIVE`.
- **Use Case:** Retiring a branch location while preserving historical records and existing employee associations.
- **Used In:** Branch List Table (Delete Action).
- **Request Params:** `id` (String - Branch ID)
- **Response:** `BranchResponse` (The deactivated branch object)
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('BRANCH_MANAGEMENT_DELETE')`
- **Notes:** Does not physically remove the record; updates status to `INACTIVE`.

### Get All Branches (Filter & Search)

- **Method:** `GET`
- **Endpoint:** `/api/v1/company/branches`
- **Purpose:** Retrieves a paginated list of branches with advanced filtering and search.
- **Use Case:** Browsing branches by city, state, or status; searching by name or code.
- **Used In:** Branch Management List Screen.
- **Query Params:**
  - `city` (String)
  - `state` (String)
  - `status` (Enum: `BranchStatus`)
  - `search` (String - searches name, code, email, phone)
  - `pageNo` (Integer, default 0)
  - `pageSize` (Integer, default 10)
- **Response:** `PaginationResponse<BranchResponse>`
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('BRANCH_MANAGEMENT_READ')`

---

## Reusable / Common APIs

### Branch Dropdown List

- **Method:** `GET`
- **Endpoint:** `/api/v1/company/branches/dropdown` (Note: Inherited or general utility often used in RBAC systems)
- **Purpose:** Provides a simplified list of active branches (ID and Name).
- **Use Case:** Selecting a branch during User Creation (Module 8) or Hiring Request submission. (Use whenever branch dropdwon required in forms)

---

## Module Enums

### BranchStatus

- **Used In:** `BranchResponse`, Filter Parameter.
- **Values:**
  - `ACTIVE`
  - `INACTIVE`

### BranchType

- **Used In:** `CreateBranchRequest`, `BranchResponse`.
- **Values:**
  - `RETAIL`
  - `SERVICE`
  - `STORAGE_UNIT`
  - `OTHER`
- **Note:** Requirements specification also mentions types like `HEAD_OFFICE`, `STATE_BRANCH`, and `WAREHOUSE`; ensure mapping matches current business needs.

---

## Validation Rules (Field-Level)

| Field              | Rule                                                  |
| :----------------- | :---------------------------------------------------- |
| **Branch Name**    | Required, min 3 characters, unique per company.       |
| **Branch Code**    | Required, exactly 3 characters, alphanumeric, unique. |
| **Email**          | Required, valid email format, unique per company.     |
| **Phone Number**   | Required, exactly 10 digits.                          |
| **Address Line 1** | Required, minimum 5 characters.                       |
| **Country**        | Required, minimum 2 characters.                       |
| **State**          | Required, minimum 2 characters.                       |
| **City**           | Required, minimum 2 characters.                       |
| **Pincode**        | Required, exactly 6 digits.                           |
| **Branch Type**    | Required, must be a valid Enum value.                 |

---

## Notes for Frontend

- **Tabbed View**: In the "View Branch" screen, the "Branch User's" tab should fetch data from the User Management module (e.g., `/api/v1/users` filtered by `branchId`).
- **Search Logic**: The backend search parameter is global across `branchName`, `branchCode`, `email`, and `phone`.
- **Soft Delete**: When "Delete" is clicked, trigger the confirmation popup and call the `DELETE` API to toggle the status to `INACTIVE`.
- **Branch Code Restriction**: Frontend should ideally disable the "Branch Code" field in the Edit screen if the branch has associated employees as per business rules.
