# Role Permission Module API Documentation

## Short Description

The Role Permission module is the central RBAC (Role-Based Access Control) engine of the platform. It allows Super Admins to define system-wide role templates and Company Admins to manage organization-specific roles. The module supports fine-grained, module-wise permission assignment (VIEW, ADD, EDIT, DELETE, REQUEST, APPROVAL, EXPORT, CONFIGURE) and ensures secure user access across web and mobile applications.

---

## APIs

### Get All Roles

- **Method:** `GET`
- **Endpoint:** `/api/v1/role/get-all`
- **Purpose:** Retrieves a list of all roles available in the platform.
- **Use Case:** Displaying a list of all roles in a management table or for administrative oversight.
- **Used In:** Role Management Dashboard.
- **Response:** `List<RoleResponse>` containing role ID, name, description, and app user flag.
- **Access / Security:** `hasAnyRole('SERAVION', 'CEO')` or `hasAuthority('ROLE_MANAGEMENT_READ')`.

### Get Role by ID

- **Method:** `GET`
- **Endpoint:** `/api/v1/role`
- **Purpose:** Retrieves detailed information about a role by its ID, including mapped permissions.
- **Use Case:** Viewing or editing a specific role configuration.
- **Used In:** View Role Detail screen, Edit Role form.
- **Request Params:** `roleId` (Long)
- **Response:** `RoleResponse` (Enriched with module permissions).
- **Access / Security:** `hasAnyRole('SERAVION', 'CEO')` or `hasAuthority('ROLE_MANAGEMENT_READ')`.

### Create Role

- **Method:** `POST`
- **Endpoint:** `/api/v1/role/create`
- **Purpose:** Creates a new role along with its module permissions.
- **Use Case:** Defining a new job role (e.g., "Senior Technician") with specific access levels.
- **Used In:** Add Role Screen.
- **Request Body:** [RoleCreateRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/role/dto/request/RoleCreateRequest.java#7-16)
- **Response:** `RoleResponse`
- **Access / Security:** `hasAnyRole('SERAVION', 'CEO')` or `hasAuthority('ROLE_MANAGEMENT_ADD')`.
- **Notes:** If `isAppUser` is true, module permissions are ignored (mobile app only).

### Update Role

- **Method:** `PUT`
- **Endpoint:** `/api/v1/role/update`
- **Purpose:** Updates an existing role's metadata and refreshes its module permissions.
- **Use Case:** Modifying permissions for an existing role due to policy changes.
- **Used In:** Edit Role Screen.
- **Request Params:** `roleId` (Long)
- **Request Body:** `RoleUpdateRequest`
- **Response:** `RoleResponse`
- **Access / Security:** `hasAnyRole('SERAVION', 'CEO')` or `hasAuthority('ROLE_MANAGEMENT_EDIT')`.
- **Notes:** Overwrites existing permissions for the role.

### Delete Role

- **Method:** `DELETE`
- **Endpoint:** `/api/v1/role/delete`
- **Purpose:** Deletes a role and reassigns all linked users to a target role.
- **Use Case:** Removing a redundant role while ensuring existing users are moved to a valid role.
- **Used In:** Delete Role Warning/Migration Popup.
- **Request Params:** `roleId` (Long), `reassignRoleId` (Long)
- **Response:** Success Message.
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('ROLE_MANAGEMENT_DELETE')`.
- **Notes:** Hard delete from database. Requires a target role for migration if users are assigned.

### Role Dropdown

- **Method:** `GET`
- **Endpoint:** `/api/v1/role/dropdown`
- **Purpose:** Fetches a list of active role names and IDs for selection menus. (Use Whenever Role Dropdown Required.)
- **Use Case:** Selecting a role during User Creation or Role Migration.
- **Used In:** User Creation Step 1, Role Delete Popup.
- **Response:** `List<RoleDropdownItem>`

### Role Analytics (Seravion)

- **Method:** `GET`
- **Endpoint:** `/api/v1/role/seravion/analytics`
- **Purpose:** Retrieves platform-wide role statistics for Super Admins.
- **Use Case:** Monitoring total and active roles across the whole system.
- **Used In:** Super Admin Dashboard.
- **Access / Security:** `hasRole('SERAVION')`.

### Role Report (Company Admin)

- **Method:** `GET`
- **Endpoint:** `/api/v1/role/analytics`
- **Purpose:** Retrieves role statistics and usage for a specific company.
- **Use Case:** Viewing how many users are assigned to each role.
- **Used In:** Company Admin Role Management Dashboard.
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('ROLE_MANAGEMENT_READ')`.

---

## Reusable / Common APIs (Used in this Module)

### Get All Modules

- **Method:** `GET`
- **Endpoint:** `/api/v1/modules`
- **Purpose:** Fetch all global platform modules (e.g., BRANCH_MANAGEMENT, USER_MANAGEMENT).
- **Use Case:** Populating the module list in the Permission Grid.
- **Used In:** Add/Edit Role Screen (Permission Matrix).

### Get All Actions

- **Method:** `GET`
- **Endpoint:** `/api/v1/actions`
- **Purpose:** Fetch all global platform actions (e.g., READ, ADD, EDIT, DELETE, REQUEST, APPROVAL).
- **Use Case:** Defining the possible actions for each module in the Permission Grid.
- **Used In:** Add/Edit Role Screen (Permission Matrix).

---

## Module Enums

### RoleStatus

- **Used In:** Role Entity / Service logic
- **Values:**
  - `ACTIVE`
  - `INACTIVE`

---

## Notes for Frontend

- **Dropdown APIs:** Use `/api/v1/role/dropdown` for all role selection fields to ensure only active roles are assigned.
- **List API:** `/api/v1/role/get-all` provides list metadata, while `/api/v1/role/analytics` (or report) provides counts for the statistics cards.
- **Permission Grid:** When building the `modulePermissions` request body, use a Map for actions where key is the action name (lower case) and value is boolean.
- **App User Logic:** If the `isAppUser` checkbox is checked, the frontend should hide or disable the module permission grid.
- **Request Receivers:** For modules where "REQUEST" or "APPROVAL" actions are enabled, the `requestReceivers` list should be populated with Role IDs selected from a dropdown.
