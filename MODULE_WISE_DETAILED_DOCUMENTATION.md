# Module-wise Detailed Documentation

## MODULE 1: AUTHENTICATION

### 4.1 Module Overview

**Purpose:**
The Authentication module verifies the identity of users (Admin, Sub-users) before allowing access to the application. It manages login, logout, token generation, and session management.

**Business Importance:**
Ensures secure access to the system, prevents unauthorized usage, and maintains user session integrity across the platform.

### 4.2 Features List

- Admin login
- Sub-user login (with schema validation)
- JWT token generation (access token and refresh token)
- Token expiration management
- Logout functionality
- Role-based authentication

### 4.3 User Flow

**Admin Login Flow:**

1. Admin navigates to login page
2. Enters email and password
3. System validates credentials
4. System generates access token and refresh token
5. System returns user details, roles, and tokens
6. Admin is redirected to dashboard

**Sub-User Login Flow:**

1. User navigates to login page
2. User enters schema ID, email, and password
3. System validates credentials against specified company schema
4. System generates tokens
5. System returns user details, roles, branch ID, and module permissions
6. User is redirected to role-appropriate dashboard

**Logout Flow:**

1. User clicks logout button
2. System invalidates current session tokens
3. User is redirected to login page

### 4.4 Screens Involved

- Login Screen (for Admin)
- Login Screen with Schema Selection (for Sub-users)
- Dashboard (post-login redirect)

### 4.5 Field-Level Details

**Admin Login Screen**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| email | String (Email) | Yes | Registered email address of admin |
| password | String (Password) | Yes | Admin account password |

**Sub-User Login Screen**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| schema | String | Yes | Unique company schema identifier (e.g., tenant_1_shini90_gmail_com) |
| email | String (Email) | Yes | Registered email address of user |
| password | String (Password) | Yes | User account password |

### 4.6 Validations & Business Rules

**Login Validations:**

- Email format must be valid
- Password must not be empty
- For sub-users, schema ID must be valid and exist in the system
- User account must be active (isActive = true)
- Credentials must match registered user records

**Token Management Rules:**

- Access token expires after 1 hour (3,600,000 ms)
- Refresh token expires after 60 days (5,184,000,000 ms)
- Tokens are signed using HS256 algorithm
- Each token contains: user_id, user_email, roles, schema_name, is_active

**Session Rules:**

- Only one active session per user (Implied behavior)
- Logout invalidates all tokens for that session

### 4.7 API Interactions

**API 1: Admin Login**

- **Endpoint:** POST /api/v1/login
- **Purpose:** Authenticate admin user and generate session tokens
- **Request Body:**

```json
{
  "email": "shini90@gmail.com",
  "password": "Shini@123"
}
```

- **Success Response (200):**

```json
{
  "status": 200,
  "message": "Login successful",
  "data": {
    "id": 1,
    "email": "shini90@gmail.com",
    "isActive": true,
    "schemaName": "tenant_1_shini90_gmail_com",
    "accessExpiration": 1768297178234,
    "refreshExpiration": 1773394778234,
    "roles": ["ROLE_ADMIN"],
    "token": "<JWT_ACCESS_TOKEN>",
    "refreshToken": "<JWT_REFRESH_TOKEN>",
    "branchId": null,
    "modules": null
  }
}
```

- **Error Handling:**
  - 401: Invalid credentials
  - 403: Account inactive
  - 400: Invalid request format

**API 2: Sub-User Login**

- **Endpoint:** POST /api/v1/login
- **Purpose:** Authenticate sub-user with company schema validation
- **Request Body:**

```json
{
  "schema": "tenant_1_shini90_gmail_com",
  "email": "aarugaming2005gmail.com",
  "password": "Aaru@123"
}
```

- **Success Response (200):**

```json
{
  "status": 200,
  "message": "Login successful",
  "data": {
    "id": 1,
    "email": "aarugaming2005gmail.com",
    "isActive": true,
    "schemaName": "tenant_1_shini90_gmail_com",
    "accessExpiration": 1768297306827,
    "refreshExpiration": 1773394906827,
    "roles": ["ROLE_BRANCHADMIN", "ROLE_EMPLOYEE"],
    "token": "<JWT_ACCESS_TOKEN>",
    "refreshToken": "<JWT_REFRESH_TOKEN>",
    "branchId": 1,
    "modules": {
      "User Management": 1,
      "Branch Management": 2,
      "Product": 3,
      "Tax": 4,
      "Services Management": 5,
      "Leads": 6,
      "Follow Ups": 7,
      "Customers": 8,
      "Quotations": 9,
      "Sales Orders": 10,
      "AMC": 11,
      "Invoices": 12,
      "Payments": 13,
      "Receipts": 14,
      "Task": 15,
      "Live Tracking": 16,
      "Customer Support": 17,
      "Performance Management": 18,
      "Check In Check Out": 19,
      "Attendance": 20,
      "Salary": 21,
      "Leave": 22,
      "Profile": 23
    }
  }
}
```

- **Error Handling:**
  - 401: Invalid credentials
  - 400: Invalid or missing schema
  - 403: Account inactive
  - 404: User not found in specified schema

**API 3: Logout**

- **Endpoint:** POST /api/v1/root/logout
- **Purpose:** Terminate user session and invalidate tokens
- **Request:** No request body required (uses authentication token from header)
- **Success Response (200):**

```json
{
  "status": 200,
  "message": "Logout successful",
  "data": null
}
```

- **Error Handling:**
  - 401: Unauthorized (invalid or expired token)

### 4.8 Error & Success Scenarios

**Success Scenarios:**

- Valid admin credentials → Successful login with admin role and full access
- Valid sub-user credentials with correct schema → Successful login with assigned roles and module access
- Valid logout request → Session terminated successfully

**Failure Scenarios:**

- Incorrect email or password → Authentication fails, error message displayed
- Invalid schema ID → Schema not found error
- Inactive user account → Access denied even with correct credentials
- Expired token on logout → Logout still processes (graceful handling)
- Missing required fields → Bad request error with field validation messages

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Login using email and password
  - Logout from system
- **Restrictions:** None
- **UI Behavior:** Full authentication access with admin privileges granted upon login

**Branch Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Login using schema, email, and password
  - Logout from system
  - Receive branch-specific permissions upon login
- **Restrictions:** Must provide valid schema ID
- **UI Behavior:** Authentication screen requires schema selection dropdown before login

**Technician Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Login using schema, email, and password
  - Logout from system
- **Restrictions:** Must provide valid schema ID
- **UI Behavior:** Standard sub-user authentication with schema requirement

**Sales Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Login using schema, email, and password
  - Logout from system
- **Restrictions:** Must provide valid schema ID
- **UI Behavior:** Standard sub-user authentication with schema requirement

**Account Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Login using schema, email, and password
  - Logout from system
- **Restrictions:** Must provide valid schema ID
- **UI Behavior:** Standard sub-user authentication with schema requirement

---

## MODULE 2: USER MANAGEMENT

### 4.1 Module Overview

**Purpose:**
The User Management module handles the creation, updating, and deletion of all system users. It enforces role-based user creation rules and manages user profiles, permissions, and organizational hierarchy.

**Business Importance:**
Establishes organizational structure, maintains reporting hierarchy, enforces access control, and ensures proper user lifecycle management across the system.

### 4.2 Features List

- Role-based user creation
- User profile management
- User profile updates (including profile image upload)
- Retrieve user details by ID or email
- Branch-wise user listing
- Manager and technician dropdown lists
- User filtering by designation
- Module assignment retrieval
- Soft delete (deactivation)
- Hard delete (permanent removal)
- Reporting hierarchy management

### 4.3 User Flow

**User Creation Flow:**

1. Authorized user navigates to User Management screen
2. System displays "Create User" button (visible only to authorized roles)
3. User clicks "Create User"
4. System opens user creation form
5. User selects branch from branch dropdown
6. User selects reporter (manager) from manager dropdown
7. System populates role dropdown based on logged-in user's role
8. User fills in user details (name, email, phone, password, designation)
9. User selects role(s) from filtered dropdown
10. User submits form
11. System validates data and role creation permissions
12. System creates user account and generates schema name
13. System displays success message
14. New user appears in user list

**User Profile Update Flow:**

1. User navigates to Profile screen
2. System displays current profile information
3. User clicks "Edit Profile"
4. User modifies editable fields (name, phone, profile image)
5. User uploads new profile image (optional)
6. User clicks "Save"
7. System validates and updates profile
8. System uploads image to S3 (if provided)
9. System displays success message with updated information

**User Deletion Flow (Soft Delete):**

1. Authorized user views user list
2. User clicks "Delete" button on specific user row
3. System shows confirmation dialog
4. User confirms deletion
5. System marks user as inactive (active = false)
6. User remains in database but cannot login
7. System displays success message

### 4.4 Screens Involved

- User List Screen (with filters and search)
- User Creation Form Screen
- User Profile Screen
- User Edit Form Screen
- User Details View Screen

### 4.5 Field-Level Details

**User Creation Form**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| firstName | String | Yes | User's first name |
| lastName | String | Yes | User's last name |
| email | String (Email) | Yes | Unique email address for login |
| phoneNo | Number | Yes | Contact phone number (10 digits) |
| password | String (Password) | Yes | Initial password (must meet security criteria) |
| branchId | Number (Dropdown) | Yes | Branch assignment |
| reporterId | Number (Dropdown) | Yes | Reporting manager ID |
| designation | String (Dropdown) | Yes | User designation (MANAGER, TECHNICIAN, EMPLOYEE, etc.) |
| roles | Array of Objects | Yes | Role assignment(s) |
| branchAdmin | Boolean | Conditional | Set to true for Branch Admin creation |
| manager | Boolean | Conditional | Set to true for Manager creation |

**Role Object Structure:**
```json
{
  "roleName": "BRANCHADMIN" // or TECHNICIANMANAGER, SALESADMIN, ACCOUNTMANAGER, TECHNICIAN, EMPLOYEE
}
```

**User Profile Update Form**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| firstName | String | Yes | Updated first name |
| lastName | String | Yes | Updated last name |
| phoneNo | Number | Yes | Updated phone number |
| files | Array of Objects | No | Profile image upload |

**File Object Structure:**
```json
{
  "fileName": "profile.jpg",
  "fileType": "image/jpeg",
  "fileData": "<base64_encoded_string>"
}
```

### 4.6 Validations & Business Rules

**User Creation Validations:**

- Email must be unique across the system
- Email format must be valid
- Phone number must be exactly 10 digits
- Password must meet complexity requirements (minimum 8 characters, must contain uppercase, lowercase, number, and special character - pattern: `^(?=.*[a-z])(?=.*[A-Z])(?=.*\\d)(?=.*[@$!%*?&])[A-Za-z\\d@$!%*?&]{8,}$`)
- First name and last name must not be empty
- Branch ID must exist in the system
- Reporter ID must exist and belong to the same branch
- Designation must match the role being assigned
- Role selection must follow creation hierarchy rules

**Role-Based Creation Rules:**

- **Company Admin can create:**
  - Branch Admin only
  - Must set branchAdmin: true
  - Role dropdown shows only "BRANCHADMIN"

- **Branch Admin can create:**
  - Managers only (Technician Manager, Sales Manager, Account Manager)
  - Must set manager: true
  - Role dropdown shows only manager roles

- **Technician Manager can create:**
  - Technicians only
  - Designation must be "TECHNICIAN"
  - Role dropdown shows only "TECHNICIAN"

- **Sales Manager can create:**
  - Sales Employees only
  - Designation must be "EMPLOYEE"
  - Role dropdown shows only "EMPLOYEE"

- **Account Manager can create:**
  - Account Employees only
  - Designation must be "EMPLOYEE"
  - Role dropdown shows only "EMPLOYEE"

**Profile Update Validations:**

- Phone number format validation
- Image file type must be image/* (JPEG, PNG, JPG)
- Image file size limit (not specified in documentation)
- First name and last name cannot be empty

**Deletion Business Rules:**

- Soft delete: Sets active: false, user cannot login but data is retained
- Hard delete: Permanently removes user from database (admin/super-admin only)
- Deletion should check for dependencies (tasks assigned, reports created, etc. - implied)

**Designation-Role Mapping:**

- TECHNICIAN designation → TECHNICIAN role
- MANAGER designation → Manager roles (TECHNICIANMANAGER, SALESADMIN, ACCOUNTMANAGER)
- EMPLOYEE designation → EMPLOYEE role
- BRANCHADMIN designation → BRANCHADMIN role

### 4.7 API Interactions

**API 1: Create User**

- **Endpoint:** POST /api/v1/users
- **Purpose:** Create a new user with role and branch assignment
- **Request Body:**

```json
{
  "firstName": "Aryan",
  "lastName": "Raval",
  "email": "aarugaming2005gmail.com",
  "phoneNo": 7874591247,
  "password": "Aaru@123",
  "branchAdmin": true,
  "manager": false,
  "branchId": 1,
  "reporterId": 2,
  "designation": "MANAGER",
  "roles": [
    {
      "roleName": "BRANCHADMIN"
    }
  ]
}
```

- **Success Response (201):**

```json
{
  "status": 201,
  "message": "User created successfully !!",
  "data": {
    "id": 9,
    "firstName": "Aryan",
    "lastName": "Raval",
    "email": "aarugaming2005gmail.com",
    "phoneNo": 7874591247,
    "schemaName": "tenant_1_aryanraval299_gmail_com",
    "createdAt": "2026-01-12",
    "lastModifiedAt": "2026-01-12",
    "roleNames": ["EMPLOYEE", "BRANCHADMIN"],
    "branchId": 1,
    "reporterId": 2,
    "branchName": "GreenTechTech ServiceService Point (Koramangala)",
    "moduleName": null,
    "reportingTo": null,
    "fullName": "Aryan Raval",
    "profileUrl": null,
    "active": true
  }
}
```

- **Error Handling:**
  - 400: Validation error (duplicate email, invalid phone, weak password)
  - 403: Unauthorized role creation attempt
  - 404: Branch or reporter not found

**API 2: Update User Profile**

- **Endpoint:** PUT /api/v1/user/profile/update
- **Purpose:** Update current logged-in user's profile details
- **Request Body:**

```json
{
  "firstName": "XYZ",
  "lastName": "ABC",
  "phoneNo": 1234567890,
  "files": [
    {
      "fileName": "profile.jpg",
      "fileType": "image/jpeg",
      "fileData": "<base64_string>"
    }
  ]
}
```

- **Success Response (200):**

```json
{
  "status": 200,
  "message": "Profile updated successfully",
  "data": {
    "id": 1,
    "firstName": "XYZ",
    "lastName": "ABC",
    "email": "user@example.com",
    "phoneNo": 1234567890,
    "profileUrl": "https://s3.amazonaws.com/.../profile.jpg",
    "updatedAt": "2026-01-12T10:30:00"
  }
}
```

- **Error Handling:**
  - 400: Validation error
  - 401: Unauthorized
  - 500: File upload error

**API 3: Get User Details by ID**

- **Endpoint:** GET /api/v1/users/{id}
- **Purpose:** Retrieve user details by user ID
- **Request:** Path parameter (id)
- **Success Response (200):** User details object
- **Error Handling:**
  - 404: User not found
  - 403: Unauthorized access

**API 4: Get User by Email**

- **Endpoint:** GET /api/v1/users/byEmail
- **Purpose:** Retrieve user details by email address
- **Request:** Query parameter (email)
- **Success Response (200):** User details object
- **Error Handling:**
  - 404: User not found

**API 5: Get All Users (Branch-wise)**

- **Endpoint:** GET /api/v1/users/getAll/branchWise
- **Purpose:** Retrieve all users within current user's branch
- **Request:** Not required (uses token to identify branch)
- **Success Response (200):** Array of user objects
- **Error Handling:**
  - 401: Unauthorized

**API 6: Get Manager Dropdown**

- **Endpoint:** GET /api/v1/users/managers/dropdown
- **Purpose:** Retrieve list of managers for dropdown selection
- **Request:** Not required
- **Success Response (200):** Array of {id, value} objects
- **Error Handling:**
  - 401: Unauthorized

**API 7: Get Technician Dropdown**

- **Endpoint:** GET /api/v1/users/technicians/dropdown
- **Purpose:** Retrieve list of technicians for dropdown selection
- **Request:** Not required
- **Success Response (200):** Array of {id, value} objects
- **Error Handling:**
  - 401: Unauthorized

**API 8: Delete User**

- **Endpoint:** DELETE /api/v1/users/{id}
- **Purpose:** Soft delete user (set active = false)
- **Request:** Path parameter (id)
- **Success Response (200):** Success message
- **Error Handling:**
  - 404: User not found
  - 403: Unauthorized deletion attempt

### 4.8 Error & Success Scenarios

**Success Scenarios:**

- Valid user creation data → User created successfully with assigned roles
- Valid profile update → Profile updated with new image URL
- Valid user ID → User details returned
- Valid deletion request → User marked as inactive

**Failure Scenarios:**

- Duplicate email → Error: "Email already exists"
- Invalid phone format → Validation error
- Weak password → Error: "Password does not meet requirements"
- Unauthorized role creation → 403 Forbidden
- Invalid branch ID → Error: "Branch not found"
- Invalid reporter ID → Error: "Reporter not found"
- User not found → 404 Not Found
- Missing required fields → Validation error with field names

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create Branch Admin users
  - View all users across all branches
  - Update any user profile
  - Delete any user
- **Restrictions:** Can only create Branch Admin role
- **UI Behavior:** Full access to user management with ability to create Branch Admins only

**Branch Admin**

- **Access Level:** Branch-Only Access
- **Allowed Actions:**
  - Create Manager users (Technician Manager, Sales Manager, Account Manager)
  - View users within their branch
  - Update users within their branch
  - Delete users within their branch
- **Restrictions:** 
  - Can only create Manager roles
  - Can only manage users within assigned branch
- **UI Behavior:** Branch-scoped user management with manager creation capability

**Technician Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View users (read-only)
- **Restrictions:** Cannot create, update, or delete users
- **UI Behavior:** User list visible but all action buttons (Create, Edit, Delete) are disabled/hidden

**Sales Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Cannot access User Management module
- **UI Behavior:** User Management module is not visible in navigation

**Account Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Cannot access User Management module
- **UI Behavior:** User Management module is not visible in navigation

---

## MODULE 3: BRANCH MANAGEMENT

### 4.1 Module Overview

**Purpose:**
The Branch Management module handles the creation, updating, and management of company branches. It ensures proper branch setup, location tracking, and branch-specific configuration.

**Business Importance:**
Enables multi-branch operations, supports branch-level data isolation, and provides foundation for branch-scoped access control and reporting.

### 4.2 Features List

- Create branch
- Update branch details
- View all branches
- Get branch dropdown list
- Branch status management (ACTIVE/INACTIVE)
- Branch type management (SERVICE, SALES, etc.)

### 4.3 User Flow

**Branch Creation Flow:**

1. Company Admin navigates to Branch Management
2. Clicks "Create Branch" button
3. System opens branch creation form
4. Admin fills in branch details (name, contact, location, type, status)
5. Admin submits form
6. System validates data
7. System creates branch record
8. System displays success message
9. New branch appears in branch list

**Branch Update Flow:**

1. Admin navigates to Branch Management
2. Clicks "Edit" on specific branch
3. System opens branch edit form with current data
4. Admin modifies branch details
5. Admin submits form
6. System validates and updates branch
7. System displays success message

**Branch List View Flow:**

1. Admin navigates to Branch Management
2. System displays all branches in table/list
3. Admin can view branch details
4. Admin can filter/search branches

### 4.4 Screens Involved

- Branch List Screen
- Branch Creation Form Screen
- Branch Edit Form Screen
- Branch Details View Screen

### 4.5 Field-Level Details

**Branch Creation Form**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| branchName | String | Yes | Name of the branch |
| contactInfo | String (Email) | Yes | Branch contact email |
| phoneNumber | String | Yes | Branch contact phone number |
| branchStatus | String (Enum) | Yes | Branch status (ACTIVE, INACTIVE) |
| branchType | String (Enum) | Yes | Branch type (SERVICE, SALES, etc.) |
| pincode | String | Yes | Postal code |
| city | String | Yes | City name |
| state | String | Yes | State name |
| location | String | Yes | Detailed location/address |

**Branch Response:**

| Field Name | Type | Description |
|------------|------|-------------|
| branchId | Integer | Unique branch identifier |
| branchName | String | Branch name |
| contactInfo | String | Contact email |
| phoneNumber | String | Contact phone |
| branchStatus | String | Status (ACTIVE/INACTIVE) |
| branchType | String | Type of branch |
| pincode | String | Postal code |
| city | String | City |
| state | String | State |
| location | String | Detailed address |
| editedBy | String | Last editor name |
| createdAt | DateTime | Creation timestamp |

### 4.6 Validations & Business Rules

**Branch Creation Validations:**

- Branch name must be unique
- Contact email must be valid format
- Phone number must be valid format
- Branch status must be valid enum value (ACTIVE, INACTIVE)
- Branch type must be valid enum value
- Location fields (pincode, city, state) must not be empty
- Only Company Admin can create branches

**Branch Update Validations:**

- Branch ID must exist
- Same validation rules as creation
- Cannot change branch ID

**Business Rules:**

- Only Company Admin can create, update, or delete branches
- Branch Admin has view-only access to their assigned branch
- Other roles have no access to branch management
- Branch status determines if branch is operational
- Branch type determines branch functionality scope

### 4.7 API Interactions

**API 1: Create Branch**

- **Endpoint:** POST /branch
- **Purpose:** Create a new branch
- **Access:** Company Admin only
- **Request Body:**

```json
{
  "branchName": "East Zone Service Center",
  "contactInfo": "opseast@zencorp.com",
  "phoneNumber": "+91 93345 88990",
  "branchStatus": "ACTIVE",
  "branchType": "SERVICE",
  "pincode": "700091",
  "city": "Kolkata",
  "state": "West Bengal",
  "location": "Salt Lake Sector V"
}
```

- **Success Response (201):**

```json
{
  "status": 201,
  "message": "Branch Created",
  "data": {
    "branchId": 4,
    "branchName": "East Zone Service Center",
    "contactInfo": "opseast@zencorp.com",
    "phoneNumber": "+91 93345 88990",
    "branchStatus": "ACTIVE",
    "branchType": "SERVICE",
    "editedBy": "Palak Dave",
    "pincode": "700091",
    "city": "Kolkata",
    "state": "West Bengal",
    "location": "Salt Lake Sector V",
    "createdAt": "2026-01-12T16:30:13.3233182"
  }
}
```

- **Error Handling:**
  - 400: Validation error
  - 403: Unauthorized (not admin)
  - 409: Duplicate branch name

**API 2: Update Branch**

- **Endpoint:** PUT /branch/update
- **Purpose:** Update existing branch details
- **Access:** Company Admin only
- **Request Body:**

```json
{
  "id": 4,
  "branchName": "Updated Branch Name",
  "contactInfo": "updated@example.com",
  "phoneNumber": "+91 98765 43210",
  "branchStatus": "ACTIVE",
  "branchType": "SERVICE",
  "pincode": "110089",
  "city": "New Delhi",
  "state": "Delhi",
  "location": "Sector 8, Rohini Depot"
}
```

- **Success Response (200):** Updated branch data
- **Error Handling:**
  - 400: Validation error
  - 403: Unauthorized
  - 404: Branch not found

**API 3: Get All Branches**

- **Endpoint:** GET /branch/all
- **Purpose:** Retrieve all branches
- **Access:** Company Admin only
- **Request:** Not required
- **Success Response (200):**

```json
{
  "status": 200,
  "message": "Branches Retrieved",
  "data": [
    {
      "branchId": 1,
      "branchName": "Branch 1",
      ...
    }
  ]
}
```

- **Error Handling:**
  - 401: Unauthorized

**API 4: Get Branch Dropdown**

- **Endpoint:** GET /branch/dropdown
- **Purpose:** Get simplified branch list for dropdowns
- **Access:** Company Admin only
- **Request:** Not required
- **Success Response (200):**

```json
{
  "status": 200,
  "message": "Branch Drop Down List",
  "data": {
    "count": 2,
    "results": [
      {
        "id": 2,
        "value": "Jindagi"
      },
      {
        "id": 3,
        "value": "North Zone Operations"
      }
    ]
  }
}
```

- **Error Handling:**
  - 401: Unauthorized

### 4.8 Error & Success Scenarios

**Success Scenarios:**

- Valid branch data → Branch created successfully
- Valid update data → Branch updated successfully
- Valid request → Branch list returned
- Valid branch ID → Branch details returned

**Failure Scenarios:**

- Duplicate branch name → Error: "Branch name already exists"
- Invalid email format → Validation error
- Missing required fields → Validation error with field names
- Unauthorized access → 403 Forbidden
- Branch not found → 404 Not Found
- Invalid branch status/type → Validation error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create branches
  - Update branches
  - View all branches
  - Delete branches (if supported)
- **Restrictions:** None
- **UI Behavior:** Full access to branch management with all CRUD operations enabled

**Branch Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View their assigned branch details
- **Restrictions:** Cannot create, update, or delete branches
- **UI Behavior:** Branch details visible but all action buttons are disabled/hidden

**Technician Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Cannot access Branch Management module
- **UI Behavior:** Branch Management module is not visible in navigation

**Sales Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Cannot access Branch Management module
- **UI Behavior:** Branch Management module is not visible in navigation

**Account Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Cannot access Branch Management module
- **UI Behavior:** Branch Management module is not visible in navigation

---

## MODULE 4: PRODUCT

### 4.1 Module Overview

**Purpose:**
The Product module manages inventory products including product details, categories, pricing, stock levels, and product status. It supports both regular products and rental products.

**Business Importance:**
Central to inventory management, enables product catalog management, supports sales operations, and provides foundation for quotations and sales orders.

### 4.2 Features List

- Create product
- Update product details
- View all products
- Get product dropdown
- Product filtering and search
- Product category management
- Product status management (ACTIVE, INACTIVE)
- Rental product management
- Product stock tracking

### 4.3 User Flow

**Product Creation Flow:**

1. Authorized user navigates to Product Management
2. Clicks "Create Product" button
3. System opens product creation form
4. User selects branch from dropdown
5. User fills in product details (name, category, price, tax, status)
6. User selects product type (regular or rental)
7. If rental, user enters rental quantity and status
8. User submits form
9. System validates data
10. System creates product record
11. System displays success message
12. New product appears in product list

**Product Update Flow:**

1. User navigates to Product Management
2. Clicks "Edit" on specific product
3. System opens product edit form with current data
4. User modifies product details
5. User submits form
6. System validates and updates product
7. System displays success message

**Product List View Flow:**

1. User navigates to Product Management
2. System displays products (branch-wise for non-admin users)
3. User can filter by category, status, price range
4. User can search by product name
5. User can view product details

### 4.4 Screens Involved

- Product List Screen (with filters and search)
- Product Creation Form Screen
- Product Edit Form Screen
- Product Details View Screen

### 4.5 Field-Level Details

**Product Creation Form**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| productName | String | Yes | Name of the product |
| productCategories | String | Yes | Product category |
| productPrice | Decimal | Yes | Product price |
| taxId | Integer | Yes | Tax ID reference |
| productStatus | String (Enum) | Yes | Product status (ACTIVE, INACTIVE) |
| branchId | Integer | Yes | Branch assignment |
| rentalProductQuantity | Integer | Conditional | Quantity available for rental (if rental product) |
| rentalProductStatus | String (Enum) | Conditional | Rental status (AVAILABLE, RENTED) (if rental product) |

**Product Response:**

| Field Name | Type | Description |
|------------|------|-------------|
| productId | Integer | Unique product identifier |
| productName | String | Product name |
| productCategories | String | Product category |
| productPrice | Decimal | Product price |
| taxId | Integer | Associated tax ID |
| taxRate | Decimal | Tax rate percentage |
| productStatus | String | Status (ACTIVE/INACTIVE) |
| branchId | Integer | Branch ID |
| branchName | String | Branch name |
| rentalProductQuantity | Integer | Rental quantity (if applicable) |
| rentalProductStatus | String | Rental status (if applicable) |
| createdAt | DateTime | Creation timestamp |
| updatedAt | DateTime | Last update timestamp |

### 4.6 Validations & Business Rules

**Product Creation Validations:**

- Product name must not be empty
- Product price must be positive number
- Tax ID must exist in system
- Product status must be valid enum value
- Branch ID must exist
- For rental products, rental quantity must be non-negative
- Product category must be valid

**Product Update Validations:**

- Product ID must exist
- Same validation rules as creation
- Cannot change product ID

**Business Rules:**

- Company Admin and Branch Admin can create, update, and delete products
- Sales Manager has full access to products
- Technician Manager and Account Manager have view-only access
- Products are branch-scoped (non-admin users see only their branch products)
- Product status determines if product is available for sale
- Rental products have separate quantity tracking

### 4.7 API Interactions

**API 1: Create Product**

- **Endpoint:** POST /api/v2/inventory/product
- **Purpose:** Create a new product
- **Access:** Company Admin, Branch Admin
- **Request Body:**

```json
{
  "productName": "Product Name",
  "productCategories": "Category",
  "productPrice": 1000.00,
  "taxId": 1,
  "productStatus": "ACTIVE",
  "branchId": 1,
  "rentalProductQuantity": 4,
  "rentalProductStatus": "AVAILABLE"
}
```

- **Success Response (201):** Created product data
- **Error Handling:**
  - 400: Validation error
  - 403: Unauthorized
  - 404: Branch or tax not found

**API 2: Update Product**

- **Endpoint:** PUT /api/v2/inventory/product/update
- **Purpose:** Update existing product
- **Access:** Company Admin, Branch Admin
- **Request Body:** Product update data with ID
- **Success Response (200):** Updated product data
- **Error Handling:**
  - 400: Validation error
  - 403: Unauthorized
  - 404: Product not found

**API 3: Get All Products**

- **Endpoint:** GET /api/v2/inventory/product/all
- **Purpose:** Retrieve all products (admin) or branch-wise products
- **Access:** Based on role
- **Request:** Not required
- **Success Response (200):** Array of product objects
- **Error Handling:**
  - 401: Unauthorized

**API 4: Get Product Dropdown**

- **Endpoint:** GET /api/v2/inventory/dropdown
- **Purpose:** Get simplified product list for dropdowns
- **Access:** Based on role
- **Request:** Not required
- **Success Response (200):** Array of {id, value} objects
- **Error Handling:**
  - 401: Unauthorized

**API 5: Get Product by ID**

- **Endpoint:** GET /api/v2/inventory/product/byId
- **Purpose:** Retrieve specific product details
- **Access:** Based on role
- **Request:** Query parameter (id)
- **Success Response (200):** Product details object
- **Error Handling:**
  - 404: Product not found

**API 6: Delete Product**

- **Endpoint:** DELETE /api/v2/inventory/product/delete
- **Purpose:** Delete product
- **Access:** Company Admin, Branch Admin
- **Request:** Product ID in request body
- **Success Response (200):** Success message
- **Error Handling:**
  - 404: Product not found
  - 403: Unauthorized

### 4.8 Error & Success Scenarios

**Success Scenarios:**

- Valid product data → Product created successfully
- Valid update data → Product updated successfully
- Valid request → Product list returned
- Valid product ID → Product details returned

**Failure Scenarios:**

- Missing required fields → Validation error
- Invalid tax ID → Error: "Tax not found"
- Invalid branch ID → Error: "Branch not found"
- Invalid price (negative) → Validation error
- Unauthorized access → 403 Forbidden
- Product not found → 404 Not Found

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create products
  - Update products
  - View all products across branches
  - Delete products
- **Restrictions:** None
- **UI Behavior:** Full access to product management with all CRUD operations enabled

**Branch Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create products
  - Update products
  - View products within their branch
  - Delete products within their branch
- **Restrictions:** Can only manage products within assigned branch
- **UI Behavior:** Branch-scoped product management with full CRUD operations

**Technician Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View products (read-only)
- **Restrictions:** Cannot create, update, or delete products
- **UI Behavior:** Product list visible but all action buttons are disabled/hidden

**Sales Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create products
  - Update products
  - View products
  - Delete products
- **Restrictions:** Branch-scoped access
- **UI Behavior:** Full access to product management within branch scope

**Account Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View products (read-only)
- **Restrictions:** Cannot create, update, or delete products
- **UI Behavior:** Product list visible but all action buttons are disabled/hidden

---

## MODULE 5: TAX

### 4.1 Module Overview

**Purpose:**
The Tax module manages tax configurations including tax types, tax rates, and tax names. It provides tax settings that are used across products, services, and financial transactions.

**Business Importance:**
Ensures accurate tax calculations in quotations, invoices, and sales orders. Supports various tax types (CGST, SGST, IGST, etc.) and tax calculation methods (percentage or fixed amount).

### 4.2 Features List

- Create tax
- Update tax details
- View all taxes
- Get tax by ID
- Delete tax
- Filter taxes by type and name
- Tax type management (PERCENTAGE, FIXED_AMOUNT)
- Tax name management (CGST, SGST, IGST, UTGST, LUXURY_TAX, OTHER)

### 4.3 User Flow

**Tax Creation Flow:**

1. Company Admin navigates to Tax Management
2. Clicks "Create Tax" button
3. System opens tax creation form
4. Admin fills in tax details (name, type, rate)
5. Admin submits form
6. System validates data
7. System creates tax record
8. System displays success message
9. New tax appears in tax list

**Tax Update Flow:**

1. Admin navigates to Tax Management
2. Clicks "Edit" on specific tax
3. System opens tax edit form with current data
4. Admin modifies tax details
5. Admin submits form
6. System validates and updates tax
7. System displays success message

**Tax List View Flow:**

1. Admin navigates to Tax Management
2. System displays all taxes
3. Admin can filter by tax type or tax name
4. Admin can view tax details

### 4.4 Screens Involved

- Tax List Screen
- Tax Creation Form Screen
- Tax Edit Form Screen
- Tax Details View Screen

### 4.5 Field-Level Details

**Tax Creation Form**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| taxName | String (Enum) | Yes | Tax name (CGST, SGST, IGST, UTGST, LUXURY_TAX, OTHER) |
| taxType | String (Enum) | Yes | Tax type (PERCENTAGE, FIXED_AMOUNT) |
| taxRate | Decimal | Yes | Tax rate (percentage or fixed amount based on type) |

**Tax Response:**

| Field Name | Type | Description |
|------------|------|-------------|
| taxId | Integer | Unique tax identifier |
| taxName | String | Tax name |
| taxType | String | Tax type (PERCENTAGE/FIXED_AMOUNT) |
| taxRate | Decimal | Tax rate value |
| createdAt | DateTime | Creation timestamp |
| updatedAt | DateTime | Last update timestamp |

### 4.6 Validations & Business Rules

**Tax Creation Validations:**

- Tax name must be valid enum value
- Tax type must be valid enum value (PERCENTAGE, FIXED_AMOUNT)
- Tax rate must be positive number
- For PERCENTAGE type, rate should typically be between 0-100
- Only Company Admin can create taxes

**Tax Update Validations:**

- Tax ID must exist
- Same validation rules as creation
- Cannot change tax ID

**Business Rules:**

- Only Company Admin can create, update, or delete taxes
- All other roles have view-only access
- Tax names are predefined: CGST, SGST, IGST, UTGST, LUXURY_TAX, OTHER
- Tax type determines calculation method (percentage or fixed amount)
- Taxes are used across products, services, and financial transactions

### 4.7 API Interactions

**API 1: Create Tax**

- **Endpoint:** POST /tax
- **Purpose:** Create a new tax configuration
- **Access:** Company Admin only
- **Request Body:**

```json
{
  "taxName": "IGST",
  "taxType": "PERCENTAGE",
  "taxRate": 18.0
}
```

- **Success Response (201):** Created tax data
- **Error Handling:**
  - 400: Validation error
  - 403: Unauthorized (not admin)

**API 2: Update Tax**

- **Endpoint:** PUT /tax/update
- **Purpose:** Update existing tax
- **Access:** Company Admin only
- **Request Body:** Tax update data with ID
- **Success Response (200):** Updated tax data
- **Error Handling:**
  - 400: Validation error
  - 403: Unauthorized
  - 404: Tax not found

**API 3: Get Tax by ID**

- **Endpoint:** POST /tax/byid
- **Purpose:** Retrieve specific tax details
- **Access:** Company Admin only
- **Request Body:** { "id": 1 }
- **Success Response (200):** Tax details object
- **Error Handling:**
  - 404: Tax not found

**API 4: Get All Taxes**

- **Endpoint:** GET /tax/all
- **Purpose:** Retrieve all taxes
- **Access:** Company Admin only
- **Request:** Not required
- **Success Response (200):** Array of tax objects
- **Error Handling:**
  - 401: Unauthorized

**API 5: Filter Taxes**

- **Endpoint:** GET /tax/find
- **Purpose:** Filter taxes by type and name
- **Access:** Company Admin only
- **Request:** Query parameters or request body with filters
- **Success Response (200):** Filtered tax list
- **Error Handling:**
  - 400: Invalid filter parameters

**API 6: Delete Tax**

- **Endpoint:** DELETE /tax/delete
- **Purpose:** Delete tax
- **Access:** Company Admin only
- **Request Body:** { "id": 1 }
- **Success Response (200):** Success message
- **Error Handling:**
  - 404: Tax not found
  - 403: Unauthorized
  - 400: Tax in use (cannot delete)

### 4.8 Error & Success Scenarios

**Success Scenarios:**

- Valid tax data → Tax created successfully
- Valid update data → Tax updated successfully
- Valid request → Tax list returned
- Valid tax ID → Tax details returned

**Failure Scenarios:**

- Invalid tax name → Validation error
- Invalid tax type → Validation error
- Invalid tax rate → Validation error
- Unauthorized access → 403 Forbidden
- Tax not found → 404 Not Found
- Tax in use → Cannot delete error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create taxes
  - Update taxes
  - View all taxes
  - Delete taxes
- **Restrictions:** None
- **UI Behavior:** Full access to tax management with all CRUD operations enabled

**Branch Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View taxes (read-only)
- **Restrictions:** Cannot create, update, or delete taxes
- **UI Behavior:** Tax list visible but all action buttons are disabled/hidden

**Technician Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View taxes (read-only)
- **Restrictions:** Cannot create, update, or delete taxes
- **UI Behavior:** Tax list visible but all action buttons are disabled/hidden

**Sales Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View taxes (read-only)
- **Restrictions:** Cannot create, update, or delete taxes
- **UI Behavior:** Tax list visible but all action buttons are disabled/hidden

**Account Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View taxes (read-only)
- **Restrictions:** Cannot create, update, or delete taxes
- **UI Behavior:** Tax list visible but all action buttons are disabled/hidden

---

## MODULE 6: SERVICES MANAGEMENT

### 4.1 Module Overview

**Purpose:**
The Services Management module handles service catalog management including service creation, pricing, categorization, and branch assignment. Services are used in quotations, sales orders, and task assignments.

**Business Importance:**
Enables service-based business operations, supports service quotations, and provides foundation for service delivery and AMC contracts.

### 4.2 Features List

- Create service
- Update service details
- View all services
- Get service by ID
- Delete service
- Filter services (by status, category, price range, date range)
- Search services by name
- Get service dropdown (branch-wise)
- Service status management (ACTIVE, INACTIVE)
- Service category management (COMMERCIAL, RESIDENTIAL)

### 4.3 User Flow

**Service Creation Flow:**

1. Authorized user navigates to Services Management
2. Clicks "Create Service" button
3. System opens service creation form
4. User selects branch from dropdown
5. User fills in service details (name, category, price, status)
6. User selects associated products/inventories (optional)
7. User submits form
8. System validates data
9. System creates service record
10. System displays success message
11. New service appears in service list

**Service Update Flow:**

1. User navigates to Services Management
2. Clicks "Edit" on specific service
3. System opens service edit form with current data
4. User modifies service details
5. User updates associated products/inventories (if needed)
6. User submits form
7. System validates and updates service
8. System displays success message

**Service List View Flow:**

1. User navigates to Services Management
2. System displays services (branch-wise for non-admin users)
3. User can filter by status, category, price range, date range
4. User can search by service name
5. User can view service details

### 4.4 Screens Involved

- Service List Screen (with filters and search)
- Service Creation Form Screen
- Service Edit Form Screen
- Service Details View Screen

### 4.5 Field-Level Details

**Service Creation Form**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| serviceName | String | Yes | Name of the service |
| serviceCategory | String (Enum) | Yes | Service category (COMMERCIAL, RESIDENTIAL) |
| servicePrice | Decimal | Yes | Service price |
| serviceStatus | String (Enum) | Yes | Service status (ACTIVE, INACTIVE) |
| branchId | Integer | Yes | Branch assignment |
| inventoryIds | Array<Integer> | No | Associated product/inventory IDs |

**Service Response:**

| Field Name | Type | Description |
|------------|------|-------------|
| serviceId | Integer | Unique service identifier |
| serviceName | String | Service name |
| serviceCategory | String | Service category |
| servicePrice | Decimal | Service price |
| serviceStatus | String | Status (ACTIVE/INACTIVE) |
| branchId | Integer | Branch ID |
| branchName | String | Branch name |
| inventories | Array | Associated products/inventories |
| createdAt | DateTime | Creation timestamp |
| updatedAt | DateTime | Last update timestamp |

### 4.6 Validations & Business Rules

**Service Creation Validations:**

- Service name must not be empty
- Service price must be positive number
- Service category must be valid enum value (COMMERCIAL, RESIDENTIAL)
- Service status must be valid enum value (ACTIVE, INACTIVE)
- Branch ID must exist
- Inventory IDs must exist (if provided)

**Service Update Validations:**

- Service ID must exist
- Same validation rules as creation
- Cannot change branch ID
- Existing inventories are removed and new ones are updated

**Business Rules:**

- Company Admin and Branch Admin can create, update, and delete services
- Technician Manager, Sales Manager, and Account Manager have view-only access
- Services are branch-scoped (non-admin users see only their branch services)
- Service status determines if service is available for quotation/order
- Services can be associated with multiple products/inventories
- Branch cannot be changed after service creation

### 4.7 API Interactions

**API 1: Create Service**

- **Endpoint:** POST /service
- **Purpose:** Create a new service
- **Access:** Company Admin, Branch Admin
- **Request Body:**

```json
{
  "serviceName": "AC Maintenance Service",
  "serviceCategory": "COMMERCIAL",
  "servicePrice": 5000.00,
  "serviceStatus": "ACTIVE",
  "branchId": 1,
  "inventoryIds": [1, 2, 3]
}
```

- **Success Response (201):** Created service data
- **Error Handling:**
  - 400: Validation error
  - 403: Unauthorized
  - 404: Branch or inventory not found

**API 2: Update Service**

- **Endpoint:** PUT /service/update
- **Purpose:** Update existing service
- **Access:** Company Admin, Branch Admin
- **Request Body:** Service update data with ID
- **Success Response (200):** Updated service data
- **Error Handling:**
  - 400: Validation error
  - 403: Unauthorized
  - 404: Service not found

**API 3: Get All Services**

- **Endpoint:** GET /service/all
- **Purpose:** Retrieve all services (admin) or branch-wise services
- **Access:** Company Admin, Branch Admin
- **Request:** Not required
- **Success Response (200):** Array of service objects
- **Error Handling:**
  - 401: Unauthorized

**API 4: Get Services Branch-Wise**

- **Endpoint:** GET /service/branchWise
- **Purpose:** Get services for specific branch
- **Access:** Branch Admin (primary), others when required
- **Request:** Not required (uses token to identify branch)
- **Success Response (200):** Array of service objects for branch
- **Error Handling:**
  - 401: Unauthorized

**API 5: Get Service Dropdown Branch-Wise**

- **Endpoint:** GET /service/dropdown
- **Purpose:** Get simplified service list for dropdowns (branch-wise)
- **Access:** Branch Admin (primary), others when required
- **Request:** Not required (uses token to identify branch)
- **Success Response (200):** Array of {id, value} objects
- **Error Handling:**
  - 401: Unauthorized

**API 6: Get Service by ID**

- **Endpoint:** GET /service/byid?id=100
- **Purpose:** Retrieve specific service details
- **Access:** Company Admin, Branch Admin
- **Request:** Query parameter (id)
- **Success Response (200):** Service details object
- **Error Handling:**
  - 404: Service not found

**API 7: Filter Services**

- **Endpoint:** POST /service/filter
- **Purpose:** Filter and search services
- **Access:** Company Admin, Branch Admin
- **Request Body:**

```json
{
  "paginationRequest": {
    "pageNumber": 0,
    "pageSize": 10
  },
  "filterColumns": {
    "serviceStatus": "ACTIVE",
    "serviceCategory": "COMMERCIAL",
    "startDate": "2025-10-29T11:53:00",
    "endDate": "2025-10-30T00:00:00",
    "startPrice": 100.0,
    "endPrice": 300.0
  },
  "searchColumns": {
    "serviceName": "om"
  },
  "orderByColumns": {
    "createdAt": "asc",
    "servicePrice": "desc"
  }
}
```

- **Success Response (200):** Paginated filtered service list
- **Error Handling:**
  - 400: Invalid filter parameters

**API 8: Delete Service**

- **Endpoint:** DELETE /service/delete
- **Purpose:** Delete service
- **Access:** Company Admin, Branch Admin
- **Request:** Service ID in request
- **Success Response (200):** Success message
- **Error Handling:**
  - 404: Service not found
  - 403: Unauthorized

### 4.8 Error & Success Scenarios

**Success Scenarios:**

- Valid service data → Service created successfully
- Valid update data → Service updated successfully
- Valid request → Service list returned
- Valid service ID → Service details returned
- Valid filters → Filtered service list returned

**Failure Scenarios:**

- Missing required fields → Validation error
- Invalid branch ID → Error: "Branch not found"
- Invalid inventory IDs → Error: "Inventory not found"
- Invalid price (negative) → Validation error
- Unauthorized access → 403 Forbidden
- Service not found → 404 Not Found

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create services
  - Update services
  - View all services across branches
  - Delete services
- **Restrictions:** None
- **UI Behavior:** Full access to service management with all CRUD operations enabled

**Branch Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create services
  - Update services
  - View services within their branch
  - Delete services within their branch
- **Restrictions:** Can only manage services within assigned branch
- **UI Behavior:** Branch-scoped service management with full CRUD operations

**Technician Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View services (read-only)
- **Restrictions:** Cannot create, update, or delete services
- **UI Behavior:** Service list visible but all action buttons are disabled/hidden

**Sales Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View services (read-only)
- **Restrictions:** Cannot create, update, or delete services
- **UI Behavior:** Service list visible but all action buttons are disabled/hidden

**Account Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View services (read-only)
- **Restrictions:** Cannot create, update, or delete services
- **UI Behavior:** Service list visible but all action buttons are disabled/hidden

---

## MODULE 7: LEADS

### 4.1 Module Overview

**Purpose:** The Leads module manages potential customer inquiries and opportunities. It tracks lead information, status, and conversion to customers. Leads can be created independently or converted from other sources.

**Business Importance:** Central to the CRM sales pipeline, enabling sales teams to track prospects, manage follow-ups, and convert leads into customers. Supports both product and service-based lead management.

### 4.2 Features List

- Create lead (Product or Service type)
- Update lead status and details
- Review/approve leads
- Convert lead to customer
- Get all leads (company-wide)
- Get branch-wise leads
- Get lead by ID
- Delete lead
- Lead dropdown for follow-ups
- Lead status management (Active, Lost, Converted)
- Lost reason tracking

### 4.3 User Flow

**Lead Creation Flow:**
1. User navigates to Leads module
2. User clicks "Create Lead" button
3. System displays lead creation form
4. User selects lead type (Product or Service)
5. User selects branch from dropdown
6. If Product type: User selects products from inventory dropdown
7. If Service type: User selects services from service dropdown
8. User fills in lead details (customer name, contact, address, etc.)
9. User submits form
10. System creates lead with unique ID
11. System displays success message
12. New lead appears in leads list

**Lead Update/Review Flow:**
1. User views lead list
2. User clicks on specific lead
3. System displays lead details
4. User updates lead status (e.g., to "Lost" or "Converted")
5. If status is "Lost", user must provide lost reason
6. User submits update
7. System updates lead status
8. If converted, system triggers customer creation workflow

### 4.4 Screens Involved

- Lead List Screen (with filters and search)
- Lead Creation Form Screen
- Lead Details View Screen
- Lead Update/Review Screen
- Lead Conversion Screen

### 4.5 Field-Level Details

**Create Lead Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| leadType | String (Enum) | Yes | Type of lead: "PRODUCT" or "SERVICE" |
| customerName | String | Yes | Name of potential customer |
| customerEmail | String (Email) | No | Email address of lead |
| customerPhone | Number | Yes | Contact phone number (10 digits) |
| customerAddress | String | No | Physical address |
| branchId | Number | Yes | Branch assignment |
| productList | Array of Objects | Conditional | Required if leadType is "PRODUCT" |
| serviceList | Array of Numbers | Conditional | Required if leadType is "SERVICE" |
| remarks | String | No | Additional notes about the lead |
| status | String | No | Initial status (default: Active) |

**Product Object Structure:**
```json
{
  "productId": 1,
  "quantity": 2
}
```

**Update Lead Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| id | Number | Yes | Lead ID to update |
| status | String | Yes | New status (Active, Lost, Converted) |
| lostReason | String | Conditional | Required if status is "Lost" |
| customerName | String | No | Updated customer name |
| customerPhone | Number | No | Updated phone number |
| remarks | String | No | Updated remarks |

**Lead Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique lead identifier |
| leadNumber | String | Auto-generated lead number |
| customerName | String | Customer name |
| customerEmail | String | Customer email |
| customerPhone | Number | Customer phone |
| customerAddress | String | Customer address |
| leadType | String | PRODUCT or SERVICE |
| status | String | Current status |
| branchId | Number | Branch ID |
| branchName | String | Branch name |
| products | Array | Product details (if Product type) |
| services | Array | Service details (if Service type) |
| lostReason | String | Reason if status is Lost |
| remarks | String | Additional notes |
| createdAt | Date | Creation timestamp |
| lastModifiedAt | Date | Last update timestamp |

### 4.6 Validations & Business Rules

**Lead Creation Validations:**
- Lead type must be either "PRODUCT" or "SERVICE"
- If leadType is "PRODUCT", productList must be provided and not empty
- If leadType is "SERVICE", serviceList must be provided and not empty
- Customer phone must be exactly 10 digits
- Customer email format must be valid (if provided)
- Branch ID must exist in the system
- Customer name must not be empty

**Lead Update Validations:**
- Lead ID must exist
- If status is changed to "Lost", lostReason must be provided
- Status must be a valid enum value (Active, Lost, Converted)
- Phone number format validation (if updated)

**Business Rules:**
- Lead status workflow: Active → Lost/Converted
- Once converted, lead cannot be edited (read-only)
- Lost leads can be reactivated by changing status back to Active
- Product leads require product selection from inventory
- Service leads require service selection from service catalog
- Branch assignment determines visibility scope

### 4.7 API Interactions

**API 1: Create Lead**

- **Endpoint:** `POST /api/v1/leads/add`
- **Purpose:** Create a new lead (Product or Service type)
- **Request Body:**
```json
{
  "leadType": "PRODUCT",
  "customerName": "John Doe",
  "customerEmail": "john@example.com",
  "customerPhone": 9876543210,
  "customerAddress": "123 Main St",
  "branchId": 1,
  "productList": [
    {
      "productId": 1,
      "quantity": 2
    }
  ],
  "remarks": "Interested in product demo"
}
```
- **Success Response (201):**
```json
{
  "status": 201,
  "message": "Lead created successfully",
  "data": {
    "id": 8,
    "leadNumber": "LD001",
    "customerName": "John Doe",
    "customerEmail": "john@example.com",
    "customerPhone": 9876543210,
    "leadType": "PRODUCT",
    "status": "Active",
    "branchId": 1,
    "branchName": "Main Branch",
    "createdAt": "2026-01-12",
    "lastModifiedAt": "2026-01-12"
  }
}
```
- **Error Handling:**
  - 400: Validation error (missing fields, invalid type, etc.)
  - 404: Branch not found
  - 403: Unauthorized access

**API 2: Update/Review Lead**

- **Endpoint:** `PUT /api/v1/leads/review`
- **Purpose:** Update lead status and details
- **Request Body:**
```json
{
  "id": 8,
  "status": "Lost",
  "lostReason": "Customer found alternative solution",
  "remarks": "Follow up in 3 months"
}
```
- **Success Response (200):**
```json
{
  "status": 200,
  "message": "Lead updated successfully",
  "data": {
    "id": 8,
    "status": "Lost",
    "lostReason": "Customer found alternative solution",
    "lastModifiedAt": "2026-01-12"
  }
}
```
- **Error Handling:**
  - 400: Validation error (missing lostReason for Lost status)
  - 404: Lead not found
  - 403: Unauthorized access

**API 3: Get All Leads**

- **Endpoint:** `GET /api/v1/leads`
- **Purpose:** Retrieve all leads (company-wide, Admin access)
- **Success Response (200):** Array of lead objects
- **Error Handling:**
  - 401: Unauthorized
  - 403: Access denied (non-admin users)

**API 4: Get Branch-wise Leads**

- **Endpoint:** `GET /api/v1/leads/branchWise`
- **Purpose:** Retrieve leads for current user's branch
- **Success Response (200):** Array of lead objects filtered by branch
- **Error Handling:**
  - 401: Unauthorized
  - 404: Branch not found

**API 5: Get Lead by ID**

- **Endpoint:** `GET /api/v1/leads/byId?id=8`
- **Purpose:** Retrieve detailed lead information
- **Request Parameter:** `id` (required)
- **Success Response (200):** Detailed lead object
- **Error Handling:**
  - 404: Lead not found
  - 401: Unauthorized

**API 6: Delete Lead**

- **Endpoint:** `DELETE /api/v1/leads?id=8`
- **Purpose:** Delete a lead record
- **Request Parameter:** `id` (required)
- **Success Response (200):** Success message
- **Error Handling:**
  - 404: Lead not found
  - 403: Unauthorized (cannot delete converted leads)
  - 400: Lead has dependencies (cannot delete)

**API 7: Get Lead Dropdown**

- **Endpoint:** `GET /api/v1/leads/dropdown`
- **Purpose:** Get leads for dropdown selection (used in Follow-ups)
- **Success Response (200):** Array of lead objects with minimal fields
- **Error Handling:**
  - 401: Unauthorized

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid lead creation → Lead created with unique number
- Valid status update → Lead status updated successfully
- Valid conversion → Customer created from lead
- Valid deletion → Lead removed from system

**Failure Scenarios:**
- Missing leadType → Validation error
- Product lead without productList → Validation error
- Service lead without serviceList → Validation error
- Invalid branch ID → Branch not found error
- Status "Lost" without lostReason → Validation error
- Attempting to delete converted lead → Access denied
- Invalid phone format → Validation error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View all leads across branches
- **Restrictions:** Cannot create, update, or delete leads
- **UI Behavior:** Leads visible but all action buttons are disabled/hidden

**Branch Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View leads related to their branch
- **Restrictions:** Cannot create, update, or delete leads
- **UI Behavior:** Leads visible but all action buttons are disabled/hidden

**Technician Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Leads module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

**Sales Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create leads
  - Update leads
  - Review/approve leads
  - Convert leads to customers
  - Delete leads
  - View all branch leads
- **Restrictions:** None
- **UI Behavior:** Full access with all CRUD operations enabled

**Account Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Leads module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

---


## MODULE 8: FOLLOW UPS

### 4.1 Module Overview

**Purpose:** The Follow Ups module manages customer follow-up activities related to leads and quotations. It tracks follow-up schedules, status, remarks, and outcomes to ensure proper customer engagement throughout the sales process.

**Business Importance:** Ensures timely customer communication, tracks sales pipeline progress, and maintains relationship history. Critical for converting leads and quotations into sales orders.

### 4.2 Features List

- Create follow-up for lead or quotation
- Update follow-up status and details
- Get all follow-ups (company-wide)
- Get branch-wise follow-ups
- Get follow-up by ID
- Delete follow-up
- Filter follow-ups by status, date, lead/quotation
- Track follow-up remarks and outcomes
- Lost reason tracking (when status is Lost)

### 4.3 User Flow

**Follow-Up Creation Flow:**
1. User navigates to Follow Ups module
2. User clicks "Create Follow-Up" button
3. System displays follow-up creation form
4. User selects lead from lead dropdown (or quotation from quotation dropdown)
5. User selects branch from branch dropdown
6. User enters follow-up date and time
7. User enters remarks/notes
8. User selects status (if applicable)
9. User submits form
10. System creates follow-up record
11. System displays success message
12. New follow-up appears in follow-ups list

**Follow-Up Update Flow:**
1. User views follow-up list
2. User clicks on specific follow-up
3. System displays follow-up details
4. User updates follow-up information (date, remarks, status)
5. If status changes to "Lost", user must provide lost reason
6. User submits update
7. System updates follow-up record
8. System displays success message

### 4.4 Screens Involved

- Follow-Up List Screen (with filters)
- Follow-Up Creation Form Screen
- Follow-Up Details View Screen
- Follow-Up Update Screen
- Follow-Up Calendar View (optional)

### 4.5 Field-Level Details

**Create Follow-Up Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| leadId | Number | Conditional | Lead ID (required if quotationId not provided) |
| quotationId | Number | Conditional | Quotation ID (required if leadId not provided) |
| branchId | Number | Yes | Branch assignment |
| followUpDate | Date | Yes | Scheduled follow-up date |
| followUpTime | Time | No | Scheduled follow-up time |
| remarks | String | No | Follow-up notes and observations |
| status | String | No | Initial status (default: Pending) |

**Update Follow-Up Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| id | Number | Yes | Follow-up ID to update |
| leadId | Number | No | Updated lead ID |
| quotationId | Number | No | Updated quotation ID |
| followUpDate | Date | No | Updated follow-up date |
| followUpTime | Time | No | Updated follow-up time |
| status | String | No | Updated status |
| lostReason | String | Conditional | Required if status is "Lost" |
| remarks | String | No | Updated remarks |

**Follow-Up Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique follow-up identifier |
| leadId | Number | Associated lead ID (if applicable) |
| leadDetails | Object | Lead information (if linked) |
| quotationId | Number | Associated quotation ID (if applicable) |
| quotationDetails | Object | Quotation information (if linked) |
| branchId | Number | Branch ID |
| branchName | String | Branch name |
| followUpDate | Date | Scheduled follow-up date |
| followUpTime | Time | Scheduled follow-up time |
| status | String | Current status (Pending, Completed, Lost, Converted) |
| lostReason | String | Reason if status is Lost |
| remarks | String | Follow-up notes |
| createdAt | Date | Creation timestamp |
| lastModifiedAt | Date | Last update timestamp |

### 4.6 Validations & Business Rules

**Follow-Up Creation Validations:**
- Either leadId or quotationId must be provided (not both, not neither)
- Branch ID must exist in the system
- Follow-up date must be a valid date
- Lead or quotation must exist in the system

**Follow-Up Update Validations:**
- Follow-up ID must exist
- If status is changed to "Lost", lostReason must be provided
- Status must be a valid enum value
- Date format validation

**Business Rules:**
- Follow-up status workflow: Pending → Completed/Lost/Converted
- Lost follow-ups require lost reason
- Follow-ups can be linked to either lead OR quotation (not both)
- Branch assignment determines visibility scope
- Completed follow-ups can have outcome remarks

### 4.7 API Interactions

**API 1: Create Follow-Up**

- **Endpoint:** `POST /api/v1/followup/add`
- **Purpose:** Create a new follow-up for lead or quotation
- **Request Body:**
```json
{
  "leadId": 8,
  "branchId": 1,
  "followUpDate": "2026-01-15",
  "followUpTime": "10:00:00",
  "remarks": "Customer interested, needs pricing details",
  "status": "Pending"
}
```
- **Success Response (201):** Follow-up object
- **Error Handling:**
  - 400: Validation error (missing leadId/quotationId, invalid date, etc.)
  - 404: Lead/quotation or branch not found
  - 403: Unauthorized access

**API 2: Update Follow-Up**

- **Endpoint:** `PUT /api/v1/followup`
- **Purpose:** Update follow-up details and status
- **Request Body:**
```json
{
  "id": 1,
  "status": "Lost",
  "lostReason": "Customer chose competitor",
  "remarks": "Follow-up completed, customer not interested"
}
```
- **Success Response (200):** Updated follow-up object
- **Error Handling:**
  - 400: Validation error (missing lostReason for Lost status)
  - 404: Follow-up not found
  - 403: Unauthorized access

**API 3: Get All Follow-Ups**

- **Endpoint:** `GET /api/v1/followup`
- **Purpose:** Retrieve all follow-ups (company-wide, Admin access)
- **Success Response (200):** Array of follow-up objects
- **Error Handling:**
  - 401: Unauthorized
  - 403: Access denied (non-admin users)

**API 4: Get Branch-wise Follow-Ups**

- **Endpoint:** `GET /api/v1/followup/branchWise`
- **Purpose:** Retrieve follow-ups for current user's branch
- **Success Response (200):** Array of follow-up objects filtered by branch
- **Error Handling:**
  - 401: Unauthorized
  - 404: Branch not found

**API 5: Get Follow-Up by ID**

- **Endpoint:** `GET /api/v1/followup/byId?id=1`
- **Purpose:** Retrieve detailed follow-up information
- **Request Parameter:** `id` (required)
- **Success Response (200):** Detailed follow-up object
- **Error Handling:**
  - 404: Follow-up not found
  - 401: Unauthorized

**API 6: Delete Follow-Up**

- **Endpoint:** `DELETE /api/v1/followup?id=1`
- **Purpose:** Delete a follow-up record
- **Request Parameter:** `id` (required)
- **Success Response (200):** Success message
- **Error Handling:**
  - 404: Follow-up not found
  - 403: Unauthorized access

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid follow-up creation → Follow-up created successfully
- Valid status update → Follow-up status updated
- Valid deletion → Follow-up removed from system
- Branch-wise filtering → Only relevant follow-ups returned

**Failure Scenarios:**
- Missing both leadId and quotationId → Validation error
- Invalid branch ID → Branch not found error
- Status "Lost" without lostReason → Validation error
- Invalid date format → Validation error
- Follow-up not found → 404 error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access (Module Disabled)
- **Allowed Actions:** None
- **Restrictions:** Follow-up module is disabled for Company Admin
- **UI Behavior:** Module not visible or disabled in navigation

**Branch Admin**

- **Access Level:** View-Only Access (Module Disabled)
- **Allowed Actions:** None
- **Restrictions:** Follow-up module is disabled for Branch Admin
- **UI Behavior:** Module not visible or disabled in navigation

**Technician Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Follow-up module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

**Sales Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create follow-ups
  - Update follow-ups
  - View all branch follow-ups
  - Delete follow-ups
  - Filter follow-ups
- **Restrictions:** None
- **UI Behavior:** Full access with all CRUD operations enabled

**Account Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Follow-up module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

---
## MODULE 9: CUSTOMERS

### 4.1 Module Overview

**Purpose:** The Customers module manages customer information and relationships. Customers can be created independently or converted from leads. It tracks customer details, product/service preferences, and transaction history.

**Business Importance:** Central to CRM and sales operations. Maintains customer database, tracks preferences, and enables relationship management. Essential for quotations, sales orders, and invoicing.

### 4.2 Features List

- Create customer (Product or Service type)
- Update customer details
- Get all customers (company-wide)
- Get branch-wise customers
- Get customer by ID
- Delete customer
- Customer dropdown for quotations/sales orders
- Customer history tracking
- Convert lead to customer

### 4.3 User Flow

**Customer Creation Flow:**
1. User navigates to Customers module
2. User clicks "Create Customer" button
3. System displays customer creation form
4. User selects customer type (Product or Service)
5. User selects branch from dropdown
6. If Product type: User selects products from inventory dropdown
7. If Service type: User selects services from service dropdown
8. User fills in customer details (name, contact, address, etc.)
9. User submits form
10. System creates customer record
11. System displays success message
12. New customer appears in customers list

**Customer Update Flow:**
1. User views customer list
2. User clicks on specific customer
3. System displays customer details
4. User updates customer information
5. Note: Customer type cannot be changed after creation
6. User submits update
7. System updates customer record
8. System displays success message

### 4.4 Screens Involved

- Customer List Screen (with filters and search)
- Customer Creation Form Screen
- Customer Details View Screen
- Customer Update Screen
- Customer History Screen

### 4.5 Field-Level Details

**Create Customer Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| customerType | String (Enum) | Yes | Type of customer: "PRODUCT" or "SERVICE" |
| customerName | String | Yes | Customer name |
| customerEmail | String (Email) | No | Email address |
| customerPhone | Number | Yes | Contact phone number (10 digits) |
| customerAddress | String | No | Physical address |
| branchId | Number | Yes | Branch assignment |
| productList | Array of Objects | Conditional | Required if customerType is "PRODUCT" |
| serviceList | Array of Numbers | Conditional | Required if customerType is "SERVICE" |
| leadId | Number | No | Lead ID if converted from lead |

**Update Customer Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| customerId | Number | Yes | Customer ID to update |
| customerName | String | No | Updated customer name |
| customerEmail | String | No | Updated email |
| customerPhone | Number | No | Updated phone number |
| customerAddress | String | No | Updated address |
| productList | Array | Conditional | Can update if customerType is PRODUCT |
| serviceList | Array | Conditional | Can update if customerType is SERVICE |

**Customer Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique customer identifier |
| customerName | String | Customer name |
| customerEmail | String | Customer email |
| customerPhone | Number | Customer phone |
| customerAddress | String | Customer address |
| customerType | String | PRODUCT or SERVICE |
| branchId | Number | Branch ID |
| branchName | String | Branch name |
| products | Array | Product details (if Product type) |
| services | Array | Service details (if Service type) |
| totalInvoices | Number | Total invoice count |
| createdAt | Date | Creation timestamp |
| lastModifiedAt | Date | Last update timestamp |

### 4.6 Validations & Business Rules

**Customer Creation Validations:**
- Customer type must be either "PRODUCT" or "SERVICE"
- If customerType is "PRODUCT", productList must be provided
- If customerType is "SERVICE", serviceList must be provided
- Customer phone must be exactly 10 digits
- Customer email format must be valid (if provided)
- Branch ID must exist in the system
- Customer name must not be empty

**Customer Update Validations:**
- Customer ID must exist
- Customer type cannot be changed after creation
- If customerType is PRODUCT, can only update productList
- If customerType is SERVICE, can only update serviceList
- Phone number format validation (if updated)

**Business Rules:**
- Customer type is immutable after creation
- Product customers can only have products assigned
- Service customers can only have services assigned
- Branch assignment determines visibility scope
- Customer history tracks all transactions

### 4.7 API Interactions

**API 1: Create Customer**

- **Endpoint:** `POST /api/v1/customers/add`
- **Purpose:** Create a new customer (Product or Service type)
- **Request Body:**
```json
{
  "customerType": "PRODUCT",
  "customerName": "ABC Corporation",
  "customerEmail": "contact@abc.com",
  "customerPhone": 9876543210,
  "customerAddress": "123 Business St",
  "branchId": 1,
  "productList": [
    {
      "productId": 1,
      "quantity": 5
    }
  ]
}
```
- **Success Response (201):** Created customer object
- **Error Handling:**
  - 400: Validation error
  - 404: Branch not found
  - 403: Unauthorized access

**API 2: Update Customer**

- **Endpoint:** `PUT /api/v1/customers/byId/update?customerId=8`
- **Purpose:** Update customer details
- **Request Body:** Customer update data
- **Success Response (200):** Updated customer object
- **Error Handling:**
  - 400: Validation error
  - 404: Customer not found
  - 403: Unauthorized access

**API 3: Get All Customers**

- **Endpoint:** `GET /api/v1/customers`
- **Purpose:** Retrieve all customers (company-wide, Admin access)
- **Success Response (200):** Array of customer objects
- **Error Handling:**
  - 401: Unauthorized
  - 403: Access denied (non-admin users)

**API 4: Get Branch-wise Customers**

- **Endpoint:** `GET /api/v1/customers/branchWise`
- **Purpose:** Retrieve customers for current user's branch
- **Success Response (200):** Array of customer objects filtered by branch
- **Error Handling:**
  - 401: Unauthorized
  - 404: Branch not found

**API 5: Get Customer by ID**

- **Endpoint:** `GET /api/v1/customers/byId?id=17`
- **Purpose:** Retrieve detailed customer information
- **Request Parameter:** `id` (required)
- **Success Response (200):** Detailed customer object
- **Error Handling:**
  - 404: Customer not found
  - 401: Unauthorized

**API 6: Delete Customer**

- **Endpoint:** `DELETE /api/v1/customers?id=1`
- **Purpose:** Delete a customer record
- **Request Parameter:** `id` (required)
- **Success Response (200):** Success message
- **Error Handling:**
  - 404: Customer not found
  - 403: Unauthorized access
  - 400: Customer has dependencies (cannot delete)

**API 7: Get Customer Dropdown**

- **Endpoint:** `GET /api/v1/customers/dropdown`
- **Purpose:** Get customers for dropdown selection
- **Success Response (200):** Array of customer objects with minimal fields
- **Error Handling:**
  - 401: Unauthorized

**API 8: Get Customer History**

- **Endpoint:** `POST /api/v1/customers/byId/track`
- **Purpose:** Get complete customer transaction history
- **Request Body:** Customer ID
- **Success Response (200):** Customer history object with all transactions
- **Error Handling:**
  - 404: Customer not found
  - 401: Unauthorized

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid customer creation → Customer created successfully
- Valid customer update → Customer details updated
- Valid deletion → Customer removed from system
- Branch-wise filtering → Only relevant customers returned

**Failure Scenarios:**
- Missing customerType → Validation error
- Product customer without productList → Validation error
- Service customer without serviceList → Validation error
- Invalid branch ID → Branch not found error
- Attempting to change customer type → Validation error
- Customer with transactions → Cannot delete error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View all customers across branches
- **Restrictions:** Cannot create, update, or delete customers
- **UI Behavior:** Customers visible but all action buttons are disabled/hidden

**Branch Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create customers
  - Update customers
  - View branch customers
  - Delete customers
- **Restrictions:** None
- **UI Behavior:** Full access with all CRUD operations enabled

**Technician Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Customers module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

**Sales Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create customers
  - Update customers
  - View all branch customers
  - Delete customers
  - View customer history
- **Restrictions:** None
- **UI Behavior:** Full access with all CRUD operations enabled

**Account Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View customers (read-only)
- **Restrictions:** Cannot create, update, or delete customers
- **UI Behavior:** Customer list visible but all action buttons are disabled/hidden

---

## MODULE 10: QUOTATIONS

### 4.1 Module Overview

**Purpose:** The Quotations module manages price quotations for products and services. Quotations can be created from leads or customers and can be converted to sales orders when accepted. Supports both one-time and recurring service quotations.

**Business Importance:** Critical for sales process, enabling sales teams to provide pricing to prospects. Acts as a bridge between leads/customers and sales orders. Supports recurring service contracts (AMC).

### 4.2 Features List

- Create quotation (Product or Service type)
- Update quotation status
- Get all quotations (company-wide)
- Get branch-wise quotations
- Get quotation by ID
- Delete quotation
- Convert quotation to sales order
- Support for recurring quotations
- Quotation number auto-generation

### 4.3 User Flow

**Quotation Creation Flow:**
1. User navigates to Quotations module
2. User clicks "Create Quotation" button
3. System displays quotation creation form
4. User selects lead OR customer (one must be provided)
5. User selects quotation type (Product or Service)
6. If Product type: User selects products with quantities
7. If Service type: User selects services and enters sqft
8. User enters financial details (subtotal, tax, grand total)
9. If recurring service: User enters start date, end date, frequency
10. User submits form
11. System generates unique quotation number
12. System creates quotation with DRAFT status
13. System displays success message
14. New quotation appears in quotations list

**Quotation Status Update Flow:**
1. User views quotation list
2. User clicks on specific quotation
3. System displays quotation details
4. User updates quotation status (e.g., to "ACCEPTED")
5. If status is ACCEPTED and linked to lead, system triggers lead conversion
6. User submits update
7. System updates quotation status
8. System may trigger sales order creation workflow
9. System displays success message

### 4.4 Screens Involved

- Quotation List Screen (with filters)
- Quotation Creation Form Screen
- Quotation Details View Screen
- Quotation Update Screen
- Quotation to Sales Order Conversion Screen

### 4.5 Field-Level Details

**Create Quotation Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| leadId | Number | Conditional | Lead ID (required if customerId not provided) |
| customerId | Number | Conditional | Customer ID (required if leadId not provided) |
| quotationType | String (Enum) | Yes | Type: "PRODUCT" or "SERVICE" |
| products | Array of Objects | Conditional | Required if quotationType is "PRODUCT" |
| services | Array of Numbers | Conditional | Required if quotationType is "SERVICE" |
| sqft | Decimal | Conditional | Required for Service quotations |
| financials | Object | Yes | Subtotal, taxAmount, grandTotal |
| recurring | Object | Conditional | Required for recurring services (isRecurring, startDate, endDate) |
| branchId | Number | Yes | Branch assignment |

**Product Object Structure:**
```json
{
  "productId": 1,
  "quantity": 2
}
```

**Financials Object Structure:**
```json
{
  "subtotal": 10000.00,
  "taxAmount": 1800.00,
  "grandTotal": 11800.00
}
```

**Recurring Object Structure:**
```json
{
  "isRecurring": true,
  "startDate": "2026-01-01",
  "endDate": "2026-12-31",
  "frequency": "MONTHLY"
}
```

**Update Quotation Status Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| id | Number | Yes | Quotation ID |
| status | String | Yes | New status (e.g., "ACCEPTED", "REJECTED") |

**Quotation Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique quotation identifier |
| quotationNumber | String | Auto-generated quotation number |
| leadId | Number | Associated lead ID (if applicable) |
| customerId | Number | Associated customer ID (if applicable) |
| quotationType | String | PRODUCT or SERVICE |
| status | String | Current status (DRAFT, ACCEPTED, REJECTED, etc.) |
| products | Array | Product details (if Product type) |
| services | Array | Service details (if Service type) |
| sqft | Decimal | Area coverage (for Service) |
| financials | Object | Financial details |
| recurring | Object | Recurring service details (if applicable) |
| branchId | Number | Branch ID |
| branchName | String | Branch name |
| createdAt | Date | Creation timestamp |
| lastModifiedAt | Date | Last update timestamp |

### 4.6 Validations & Business Rules

**Quotation Creation Validations:**
- Either leadId OR customerId must be provided (not both, not neither)
- Quotation type must be either "PRODUCT" or "SERVICE"
- If quotationType is "PRODUCT", products array must be provided and not empty
- If quotationType is "SERVICE", services array must be provided and not empty
- If quotationType is "SERVICE", sqft must be provided
- Financials object must contain valid subtotal, taxAmount, and grandTotal
- Branch ID must exist in the system
- Lead or customer must exist in the system

**Quotation Update Validations:**
- Quotation ID must exist
- Status must be a valid enum value
- Cannot change quotation type after creation

**Business Rules:**
- Quotation number is auto-generated and unique
- Quotation status workflow: DRAFT → ACCEPTED/REJECTED
- When status changes to ACCEPTED and linked to lead, triggers lead conversion event
- Accepted quotations can be converted to sales orders
- Recurring quotations can be converted to AMC contracts
- Product quotations require product selection with quantities
- Service quotations require service selection with sqft area
- Branch assignment determines visibility scope

### 4.7 API Interactions

**API 1: Create Quotation**

- **Endpoint:** `POST /api/v1/quotation/add`
- **Purpose:** Create a new quotation
- **Request Body:**
```json
{
  "customerId": 17,
  "quotationType": "SERVICE",
  "services": [1, 2, 3],
  "sqft": 1500.00,
  "financials": {
    "subtotal": 50000.00,
    "taxAmount": 9000.00,
    "grandTotal": 59000.00
  },
  "recurring": {
    "isRecurring": true,
    "startDate": "2026-01-01",
    "endDate": "2026-12-31",
    "frequency": "MONTHLY"
  },
  "branchId": 1
}
```
- **Success Response (201):**
```json
{
  "status": 201,
  "message": "Quotation created successfully",
  "data": {
    "id": 1,
    "quotationNumber": "QT001",
    "customerId": 17,
    "quotationType": "SERVICE",
    "status": "DRAFT",
    "financials": {...},
    "createdAt": "2026-01-12",
    "lastModifiedAt": "2026-01-12"
  }
}
```
- **Error Handling:**
  - 400: Validation error (missing leadId/customerId, invalid type, etc.)
  - 404: Lead/customer or branch not found
  - 403: Unauthorized access

**API 2: Update Quotation Status**

- **Endpoint:** `PATCH /api/v1/quotation/update-status`
- **Purpose:** Update quotation status and trigger workflows
- **Request Body:**
```json
{
  "id": 1,
  "status": "ACCEPTED"
}
```
- **Success Response (200):** Updated quotation object
- **Business Logic:**
  - Updates quotation status
  - If status is ACCEPTED and leadId is present, publishes QuotationAcceptedEvent
  - Event triggers lead conversion workflow
- **Error Handling:**
  - 400: Validation error
  - 404: Quotation not found
  - 403: Unauthorized access

**API 3: Get Quotation by ID**

- **Endpoint:** `GET /api/v1/quotation/getById`
- **Purpose:** Retrieve detailed quotation information
- **Request Parameter:** `id` (required)
- **Success Response (200):** Detailed quotation object
- **Error Handling:**
  - 404: Quotation not found
  - 401: Unauthorized

**API 4: Get All Quotations**

- **Endpoint:** `GET /api/v1/quotation/getAll`
- **Purpose:** Retrieve all quotations (company-wide, Admin access)
- **Success Response (200):** Array of quotation objects
- **Error Handling:**
  - 401: Unauthorized
  - 403: Access denied (non-admin users)

**API 5: Get Branch-wise Quotations**

- **Endpoint:** `GET /api/v1/quotation/getByBranch`
- **Purpose:** Retrieve quotations for specific branch
- **Success Response (200):** Array of quotation objects filtered by branch
- **Error Handling:**
  - 401: Unauthorized
  - 404: Branch not found

**API 6: Delete Quotation**

- **Endpoint:** `DELETE /api/v1/quotation/{id}`
- **Purpose:** Delete a quotation record
- **Request Parameter:** `id` (path parameter)
- **Success Response (200):** Success message
- **Error Handling:**
  - 404: Quotation not found
  - 403: Unauthorized access
  - 400: Quotation has dependencies (cannot delete)

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid quotation creation → Quotation created with unique number
- Valid status update → Quotation status updated, workflows triggered
- Valid conversion → Sales order created from quotation
- Branch-wise filtering → Only relevant quotations returned

**Failure Scenarios:**
- Missing both leadId and customerId → Validation error
- Product quotation without products → Validation error
- Service quotation without services → Validation error
- Service quotation without sqft → Validation error
- Invalid branch ID → Branch not found error
- Invalid financial calculations → Validation error
- Quotation with sales order → Cannot delete error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View all quotations across branches
- **Restrictions:** Cannot create, update, or delete quotations
- **UI Behavior:** Quotations visible but all action buttons are disabled/hidden

**Branch Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create quotations
  - Update quotations
  - View branch quotations
  - Delete quotations
  - Convert quotations to sales orders
- **Restrictions:** None
- **UI Behavior:** Full access with all CRUD operations enabled

**Technician Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Quotations module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

**Sales Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create quotations
  - Update quotations
  - View all branch quotations
  - Delete quotations
  - Convert quotations to sales orders
- **Restrictions:** None
- **UI Behavior:** Full access with all CRUD operations enabled

**Account Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Quotations module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

---


## MODULE 11: SALES ORDERS

### 4.1 Module Overview

**Purpose:** The Sales Orders module manages confirmed sales transactions. Sales orders can be created from quotations or directly. When confirmed, they automatically trigger invoice creation and can create AMC contracts for recurring services.

**Business Importance:** Core sales transaction management. Bridges quotations to invoicing. Handles both one-time sales and recurring service contracts (AMC). Automatically generates invoices upon confirmation.

### 4.2 Features List

- Create sales order (Product or Service type)
- Update sales order status
- Get all sales orders (company-wide)
- Get branch-wise sales orders
- Get sales order by ID
- Delete sales order
- Convert quotation to sales order
- Automatic invoice generation on confirmation
- Automatic AMC contract creation (for recurring services)
- Support for AMC flag and details

### 4.3 User Flow

**Sales Order Creation Flow:**
1. User navigates to Sales Orders module
2. User clicks "Create Sales Order" button
3. System displays sales order creation form
4. User selects customer (mandatory)
5. User optionally selects quotation (if converting from quotation)
6. User selects sales order type (Product or Service)
7. If Product type: User selects products with quantities
8. If Service type: User selects services and enters sqft
9. User enters financial details (subtotal, tax, grand total)
10. If recurring service: User sets isAmc flag and enters AMC details
11. User submits form
12. System creates sales order with DRAFT status
13. System displays success message
14. New sales order appears in sales orders list

**Sales Order Confirmation Flow:**
1. User views sales order list
2. User clicks on specific sales order
3. System displays sales order details
4. User updates status to "CONFIRMED"
5. If isAmc is true, user provides AMC details (startDate, frequency)
6. User submits update
7. System updates sales order status to CONFIRMED
8. System automatically creates invoice (DRAFT status)
9. If isAmc is true, system creates AMC contract
10. System displays success message

### 4.4 Screens Involved

- Sales Order List Screen (with filters)
- Sales Order Creation Form Screen
- Sales Order Details View Screen
- Sales Order Update/Confirmation Screen
- Sales Order to Invoice View Screen

### 4.5 Field-Level Details

**Create Sales Order Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| customerId | Number | Yes | Customer ID |
| quotationId | Number | No | Quotation ID (if converting from quotation) |
| soType | String (Enum) | Yes | Type: "PRODUCT" or "SERVICE" |
| salesOrderRequest | Array | Yes | List of items (products or services) |
| financials | Object | Yes | Subtotal, taxAmount, grandTotal |
| sqft | Decimal | Conditional | Required for Service orders |
| isAmc | Boolean | No | Flag for AMC contract creation |
| amcDetails | Object | Conditional | Required if isAmc is true (startDate, frequency) |
| branchId | Number | Yes | Branch assignment |

**Sales Order Item Structure (Product):**
```json
{
  "productId": 1,
  "quantity": 2
}
```

**Sales Order Item Structure (Service):**
```json
{
  "serviceId": 1
}
```

**AMC Details Object Structure:**
```json
{
  "startDate": "2026-01-01",
  "frequency": "MONTHLY",
  "endDate": "2026-12-31"
}
```

**Update Sales Order Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| id | Number | Yes | Sales order ID |
| status | String | Yes | New status (e.g., "CONFIRMED") |
| isAmc | Boolean | No | AMC flag |
| amcDetails | Object | Conditional | Required if isAmc is true |

**Sales Order Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique sales order identifier |
| salesOrderNumber | Number | Auto-generated sales order number |
| customerId | Number | Customer ID |
| customerDetails | Object | Customer information |
| quotationId | Number | Associated quotation ID (if applicable) |
| soType | String | PRODUCT or SERVICE |
| status | String | Current status (DRAFT, CONFIRMED, etc.) |
| products | Array | Product details (if Product type) |
| services | Array | Service details (if Service type) |
| sqft | Decimal | Area coverage (for Service) |
| financials | Object | Financial details |
| isAmc | Boolean | AMC flag |
| amcDetails | Object | AMC contract details (if applicable) |
| branchId | Number | Branch ID |
| branchName | String | Branch name |
| invoiceId | Number | Linked invoice ID (if created) |
| amcId | Number | Linked AMC ID (if created) |
| createdAt | Date | Creation timestamp |
| lastModifiedAt | Date | Last update timestamp |

### 4.6 Validations & Business Rules

**Sales Order Creation Validations:**
- Customer ID must exist in the system
- Sales order type must be either "PRODUCT" or "SERVICE"
- If soType is "PRODUCT", products must be provided
- If soType is "SERVICE", services must be provided and sqft must be provided
- Financials object must contain valid subtotal, taxAmount, and grandTotal
- Branch ID must exist in the system
- If quotationId is provided, quotation must exist

**Sales Order Update Validations:**
- Sales order ID must exist
- Status must be a valid enum value
- If isAmc is true, amcDetails must be provided
- If status is CONFIRMED, cannot change back to DRAFT

**Business Rules:**
- Sales order number is auto-generated and unique
- Sales order status workflow: DRAFT → CONFIRMED
- When status changes to CONFIRMED:
  - System automatically creates invoice (DRAFT status)
  - If isAmc is true, system creates AMC contract
  - Invoice financials are mapped from sales order
- Product sales orders require product selection with quantities
- Service sales orders require service selection with sqft area
- AMC sales orders create recurring service contracts
- Branch assignment determines visibility scope
- Cannot delete sales orders with linked invoices

### 4.7 API Interactions

**API 1: Create Sales Order**

- **Endpoint:** `POST /api/v1/sales-orders`
- **Purpose:** Create a new sales order
- **Request Body:**
```json
{
  "customerId": 17,
  "quotationId": 1,
  "soType": "SERVICE",
  "salesOrderRequest": [
    {
      "serviceId": 1
    },
    {
      "serviceId": 2
    }
  ],
  "sqft": 1500.00,
  "financials": {
    "subtotal": 50000.00,
    "taxAmount": 9000.00,
    "grandTotal": 59000.00
  },
  "isAmc": true,
  "amcDetails": {
    "startDate": "2026-01-01",
    "frequency": "MONTHLY",
    "endDate": "2026-12-31"
  },
  "branchId": 1
}
```
- **Success Response (201):** Created sales order object
- **Error Handling:**
  - 400: Validation error
  - 404: Customer, quotation, or branch not found
  - 403: Unauthorized access

**API 2: Update Sales Order (Confirmation)**

- **Endpoint:** `PUT /api/v1/sales-orders/{id}`
- **Purpose:** Update sales order and trigger confirmation workflows
- **Request Body:**
```json
{
  "status": "CONFIRMED",
  "isAmc": true,
  "amcDetails": {
    "startDate": "2026-01-01",
    "frequency": "MONTHLY"
  }
}
```
- **Success Response (200):** Updated sales order object
- **Business Logic:**
  - Updates sales order status
  - If status is CONFIRMED:
    - Creates invoice (DRAFT status) via InvoiceService
    - If isAmc is true, creates AMC contract
    - Maps financials from sales order to invoice
- **Error Handling:**
  - 400: Validation error
  - 404: Sales order not found
  - 403: Unauthorized access

**API 3: Get Sales Order by ID**

- **Endpoint:** `GET /api/v1/sales-orders/{id}`
- **Purpose:** Retrieve detailed sales order information
- **Request Parameter:** `id` (path parameter)
- **Success Response (200):** Detailed sales order object
- **Error Handling:**
  - 404: Sales order not found
  - 401: Unauthorized

**API 4: Get All Sales Orders**

- **Endpoint:** `GET /api/v1/sales-orders`
- **Purpose:** Retrieve all sales orders (company-wide, Admin access)
- **Success Response (200):** Array of sales order objects
- **Error Handling:**
  - 401: Unauthorized
  - 403: Access denied (non-admin users)

**API 5: Get Branch-wise Sales Orders**

- **Endpoint:** `GET /api/v1/sales-orders/branch/{branchId}`
- **Purpose:** Retrieve sales orders for specific branch
- **Request Parameter:** `branchId` (path parameter)
- **Success Response (200):** Array of sales order objects filtered by branch
- **Error Handling:**
  - 401: Unauthorized
  - 404: Branch not found

**API 6: Delete Sales Order**

- **Endpoint:** `DELETE /api/v1/sales-orders/{id}`
- **Purpose:** Delete a sales order record
- **Request Parameter:** `id` (path parameter)
- **Success Response (200):** Success message
- **Error Handling:**
  - 404: Sales order not found
  - 403: Unauthorized access
  - 400: Sales order has dependencies (cannot delete)

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid sales order creation → Sales order created successfully
- Valid confirmation → Sales order confirmed, invoice and AMC created
- Valid deletion → Sales order removed from system
- Branch-wise filtering → Only relevant sales orders returned

**Failure Scenarios:**
- Missing customer ID → Validation error
- Product order without products → Validation error
- Service order without services → Validation error
- Service order without sqft → Validation error
- Invalid branch ID → Branch not found error
- AMC flag true without amcDetails → Validation error
- Sales order with invoice → Cannot delete error
- Invalid financial calculations → Validation error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View all sales orders across branches
- **Restrictions:** Cannot create, update, or delete sales orders
- **UI Behavior:** Sales orders visible but all action buttons are disabled/hidden

**Branch Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create sales orders
  - Update sales orders
  - Confirm sales orders
  - View branch sales orders
  - Delete sales orders
- **Restrictions:** None
- **UI Behavior:** Full access with all CRUD operations enabled

**Technician Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Sales Orders module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

**Sales Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create sales orders
  - Update sales orders
  - Confirm sales orders
  - View all branch sales orders
  - Delete sales orders
- **Restrictions:** None
- **UI Behavior:** Full access with all CRUD operations enabled

**Account Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View sales orders (read-only)
- **Restrictions:** Cannot create, update, or delete sales orders
- **UI Behavior:** Sales order list visible but all action buttons are disabled/hidden

---

## MODULE 12: AMC (Annual Maintenance Contract)

### 4.1 Module Overview

**Purpose:** The AMC module manages Annual Maintenance Contracts for recurring services. AMC contracts are automatically created from confirmed sales orders with isAmc flag. A scheduler runs daily to generate invoices for due AMC services.

**Business Importance:** Enables recurring revenue management through automated service invoicing. Tracks contract lifecycle, service cycles, and payment history. Critical for service-based businesses with subscription models.

### 4.2 Features List

- Automatic AMC creation from sales orders
- Update AMC status (Pause, Terminate, Resume)
- Get all AMCs (company-wide)
- Get branch-wise AMCs
- Get AMC by ID
- Get AMC tracking history
- Automatic invoice generation via scheduler
- AMC status management (ACTIVE, PAUSED, TERMINATED, DRAFT)
- Track service cycles and payments

### 4.3 User Flow

**AMC Creation Flow (Automatic):**
1. Sales order is created with isAmc = true
2. Sales order status is updated to CONFIRMED
3. System automatically creates AMC contract
4. System generates unique AMC number (e.g., AMC001)
5. System sets AMC status to DRAFT initially
6. System calculates total cycles and contract value
7. AMC appears in AMC list

**AMC Status Update Flow:**
1. User navigates to AMC module
2. User views AMC list
3. User clicks on specific AMC
4. System displays AMC details
5. User updates AMC status (e.g., to PAUSED or TERMINATED)
6. If PAUSED, user provides pause reason
7. If TERMINATED, user provides termination reason
8. User submits update
9. System updates AMC status
10. PAUSED AMCs are skipped by scheduler
11. System displays success message

**AMC Scheduler Flow (Automatic - Daily at 01:00 AM):**
1. Scheduler runs daily
2. System identifies AMCs where next_invoice_date == today
3. System filters for ACTIVE or initial DRAFT status
4. System creates invoice for each due AMC
5. System updates AmcTracking with invoice details
6. System calculates next invoice date based on frequency
7. System updates AMC tracking record

### 4.4 Screens Involved

- AMC List Screen (with filters)
- AMC Details View Screen
- AMC Status Update Screen
- AMC Tracking History Screen
- AMC Scheduler Status Screen (Admin)

### 4.5 Field-Level Details

**Update AMC Status Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| amcId | Number | Yes | AMC ID to update |
| amcStatus | String | Yes | New status (ACTIVE, PAUSED, TERMINATED) |
| pauseReason | String | Conditional | Required if status is PAUSED |
| terminationReason | String | Conditional | Required if status is TERMINATED |

**AMC Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique AMC identifier |
| amcNumber | String | Auto-generated AMC number |
| customerId | Number | Customer ID |
| customerDetails | Object | Customer information |
| salesOrderId | Number | Linked sales order ID |
| status | String | Current status (DRAFT, ACTIVE, PAUSED, TERMINATED) |
| startDate | Date | Contract start date |
| endDate | Date | Contract end date |
| frequency | String | Service frequency (MONTHLY, QUARTERLY, etc.) |
| totalCycle | Number | Total number of service cycles |
| contractTotalValue | Decimal | Total contract value |
| totalCompleteAmount | Decimal | Total amount collected |
| totalRemainAmount | Decimal | Remaining amount to be collected |
| nextInvoiceDate | Date | Next scheduled invoice date |
| pauseReason | String | Reason if status is PAUSED |
| terminationReason | String | Reason if status is TERMINATED |
| branchId | Number | Branch ID |
| branchName | String | Branch name |
| createdAt | Date | Creation timestamp |
| lastModifiedAt | Date | Last update timestamp |

**AMC Tracking Response**

| Field Name | Type | Description |
|------------|------|-------------|
| amcTrackingId | Number | Unique tracking identifier |
| amcId | Number | Associated AMC ID |
| invoiceId | Number | Generated invoice ID |
| invoiceNumber | String | Invoice number |
| invoiceCreateDate | Date | Invoice creation date |
| paymentPaidDate | Date | Payment received date |
| receiptId | Number | Receipt ID |
| cycleNumber | Number | Service cycle number |
| amount | Decimal | Invoice amount |

### 4.6 Validations & Business Rules

**AMC Creation Validations:**
- Sales order must have isAmc = true
- Sales order status must be CONFIRMED
- AMC details (startDate, frequency) must be provided
- Customer must exist in the system
- Branch must exist in the system

**AMC Update Validations:**
- AMC ID must exist
- If status is PAUSED, pauseReason must be provided
- If status is TERMINATED, terminationReason must be provided
- Status must be a valid enum value

**Business Rules:**
- AMC number is auto-generated and unique
- AMC status workflow: DRAFT → ACTIVE → PAUSED/TERMINATED
- PAUSED AMCs are skipped by scheduler (no invoices generated)
- TERMINATED AMCs are permanently stopped
- Scheduler runs daily at 01:00 AM
- Scheduler only processes ACTIVE or initial DRAFT AMCs
- Next invoice date is calculated based on frequency
- Total cycles are calculated based on start date, end date, and frequency
- Contract total value is calculated from sales order
- AmcTracking records all invoice generations and payments
- Cannot delete AMCs with tracking history

### 4.7 API Interactions

**API 1: Update AMC Status**

- **Endpoint:** `PUT /api/v1/amc/update`
- **Purpose:** Pause, terminate, or resume an AMC
- **Request Body:**
```json
{
  "amcId": 1,
  "amcStatus": "PAUSED",
  "pauseReason": "Customer requested temporary hold"
}
```
- **Success Response (200):** Updated AMC object
- **Business Logic:**
  - Updates AMC status
  - PAUSED records are skipped by scheduler
  - TERMINATED records are permanently stopped
- **Error Handling:**
  - 400: Validation error (missing pauseReason/terminationReason)
  - 404: AMC not found
  - 403: Unauthorized access

**API 2: Get All AMCs**

- **Endpoint:** `GET /api/v1/amcs`
- **Purpose:** Fetch all AMC records (Admin view)
- **Success Response (200):** Array of AMC objects
- **Error Handling:**
  - 401: Unauthorized
  - 403: Access denied (non-admin users)

**API 3: Get AMC by ID**

- **Endpoint:** `GET /api/v1/amc?amcId=1`
- **Purpose:** Fetch complete AMC details
- **Request Parameter:** `amcId` (required)
- **Success Response (200):** Detailed AMC object
- **Error Handling:**
  - 404: AMC not found
  - 401: Unauthorized

**API 4: Get Branch-wise AMCs**

- **Endpoint:** `GET /api/v1/amcs/getAll/branchWise`
- **Purpose:** Fetch AMCs scoped to current user's branch
- **Success Response (200):** Array of AMC objects filtered by branch
- **Error Handling:**
  - 401: Unauthorized
  - 404: Branch not found

**API 5: Get AMC Tracking History**

- **Endpoint:** `GET /api/v1/amc/{amcId}/tracking`
- **Purpose:** Fetch full execution timeline for an AMC
- **Request Parameter:** `amcId` (path parameter)
- **Success Response (200):**
```json
{
  "status": 200,
  "message": "AMC tracking retrieved successfully",
  "data": [
    {
      "amcTrackingId": 1,
      "amcId": 1,
      "invoiceId": 10,
      "invoiceNumber": "INV001",
      "invoiceCreateDate": "2026-01-01",
      "paymentPaidDate": "2026-01-05",
      "receiptId": 5,
      "cycleNumber": 1,
      "amount": 5000.00
    }
  ]
}
```
- **Business Logic:**
  - Queries amc_tracking table by amcId
  - Ordered by invoiceCreateDate (ASC)
  - Shows complete service cycle history
- **Error Handling:**
  - 404: AMC not found
  - 401: Unauthorized

**API 6: Manual Cron Trigger (Dev Only)**

- **Endpoint:** `POST /api/v1/dev/cron/run-amc`
- **Purpose:** Manually trigger AMC scheduler for testing
- **Access:** Development/Admin only
- **Success Response (200):** Scheduler execution result
- **Error Handling:**
  - 403: Unauthorized (non-dev access)

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid AMC creation → AMC created from sales order
- Valid status update → AMC status updated, scheduler behavior changed
- Valid scheduler run → Invoices generated for due AMCs
- Valid tracking retrieval → Complete AMC history returned

**Failure Scenarios:**
- Sales order without isAmc flag → AMC not created
- Status PAUSED without pauseReason → Validation error
- Status TERMINATED without terminationReason → Validation error
- AMC not found → 404 error
- Scheduler failure → Error logged, retry on next run

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View all AMCs across branches
  - View AMC tracking history
- **Restrictions:** Cannot create, update, or delete AMCs
- **UI Behavior:** AMCs visible but all action buttons are disabled/hidden

**Branch Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - View branch AMCs
  - Update AMC status
  - View AMC tracking history
- **Restrictions:** Cannot manually create AMCs (auto-created from sales orders)
- **UI Behavior:** Full access with status update enabled

**Technician Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View AMC details
  - View AMC tracking history
- **Restrictions:** Cannot create, update, or delete AMCs
- **UI Behavior:** AMCs visible but all action buttons are disabled/hidden

**Sales Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - View all branch AMCs
  - Update AMC status
  - View AMC tracking history
- **Restrictions:** Cannot manually create AMCs (auto-created from sales orders)
- **UI Behavior:** Full access with status update enabled

**Account Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** AMC module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

---

## MODULE 13: INVOICES

### 4.1 Module Overview

**Purpose:** The Invoices module manages billing and invoicing. Invoices can be created manually, automatically from confirmed sales orders, or via AMC scheduler. When confirmed, invoices automatically create payment records.

**Business Importance:** Core financial document for billing customers. Tracks payment status, amounts due, and integrates with payments and receipts. Essential for accounts receivable management.

### 4.2 Features List

- Create invoice manually
- Automatic invoice creation from sales orders
- Automatic invoice generation via AMC scheduler
- Update invoice status (DRAFT → CONFIRM)
- Get all invoices (company-wide)
- Get branch-wise invoices
- Get invoice by ID
- Delete invoice
- Automatic payment record creation on confirmation
- Invoice number auto-generation

### 4.3 User Flow

**Manual Invoice Creation Flow:**
1. User navigates to Invoices module
2. User clicks "Create Invoice" button
3. System displays invoice creation form
4. User selects customer
5. User selects branch
6. User enters invoice details (products/services, amounts)
7. User enters financial details (subtotal, tax, grand total)
8. User submits form
9. System creates invoice with DRAFT status
10. System generates unique invoice number
11. System displays success message
12. New invoice appears in invoices list

**Invoice Confirmation Flow:**
1. User views invoice list
2. User clicks on specific invoice
3. System displays invoice details
4. User updates status to "CONFIRM"
5. User submits update
6. System updates invoice status to CONFIRM
7. System automatically creates Payment record (PENDING status)
8. If AMC-linked, system updates AmcTracking
9. System displays success message

**Automatic Invoice Creation (Sales Order):**
1. Sales order status changes to CONFIRMED
2. System automatically creates invoice (DRAFT status)
3. Invoice financials are mapped from sales order
4. Invoice appears in invoices list

**Automatic Invoice Creation (AMC Scheduler):**
1. AMC scheduler runs daily at 01:00 AM
2. System identifies AMCs due for service (next_invoice_date == today)
3. System creates invoice for each due AMC
4. System updates AmcTracking with invoice details
5. System calculates next invoice date

### 4.4 Screens Involved

- Invoice List Screen (with filters)
- Invoice Creation Form Screen
- Invoice Details View Screen
- Invoice Update/Confirmation Screen
- Invoice Print/PDF Screen

### 4.5 Field-Level Details

**Create Invoice Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| customerId | Number | Yes | Customer ID |
| salesOrderId | Number | No | Sales order ID (if from sales order) |
| branchId | Number | Yes | Branch assignment |
| invoiceIsFor | String (Enum) | Yes | Type: "PRODUCT" or "SERVICE" |
| serviceCategory | String | No | Service category (e.g., "AMC", "General") |
| financials | Object | Yes | Subtotal, taxAmount, grandTotal, discountAmount |
| sqft | Decimal | Conditional | Required for Service invoices |
| status | String | No | Initial status (default: DRAFT) |

**Update Invoice Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| id | Number | Yes | Invoice ID |
| status | String | Yes | New status (CONFIRM, CANCELLED) |
| financials | Object | No | Updated financial details |

**Invoice Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique invoice identifier |
| invoiceNumber | String | Auto-generated invoice number |
| customerId | Number | Customer ID |
| customerDetails | Object | Customer information |
| salesOrderId | Number | Linked sales order ID (if applicable) |
| status | String | Current status (DRAFT, CONFIRM, CANCELLED) |
| paymentStatus | String | Payment status (UNPAID, PAID, PARTIAL) |
| invoiceIsFor | String | PRODUCT or SERVICE |
| serviceCategory | String | Service category |
| grandTotal | Decimal | Final billing amount |
| amountPaid | Decimal | Total amount received |
| balanceAmount | Decimal | Remaining amount to be collected |
| sqft | Decimal | Area coverage (for Service) |
| branchId | Number | Branch ID |
| branchName | String | Branch name |
| paymentId | Number | Linked payment ID (if created) |
| createdAt | Date | Creation timestamp |
| lastModifiedAt | Date | Last update timestamp |

### 4.6 Validations & Business Rules

**Invoice Creation Validations:**
- Customer ID must exist in the system
- Branch ID must exist in the system
- Invoice type must be either "PRODUCT" or "SERVICE"
- If SERVICE type, sqft must be provided
- Financials object must contain valid amounts
- Grand total must be positive

**Invoice Update Validations:**
- Invoice ID must exist
- Status must be a valid enum value
- Cannot change status from CONFIRM back to DRAFT
- Cannot cancel confirmed invoices with payments

**Business Rules:**
- Invoice number is auto-generated and unique
- Invoice status workflow: DRAFT → CONFIRM → CANCELLED
- When status changes to CONFIRM:
  - System automatically creates Payment record (PENDING status)
  - If AMC-linked, system updates AmcTracking
  - System updates CustomerDetails (total invoice count)
- Payment status is calculated: UNPAID → PARTIAL → PAID
- Balance amount = grandTotal - amountPaid
- Branch assignment determines visibility scope
- Cannot delete confirmed invoices with payments

### 4.7 API Interactions

**API 1: Create Invoice**

- **Endpoint:** `POST /api/v1/invoices/add`
- **Purpose:** Create a new invoice manually
- **Request Body:**
```json
{
  "customerId": 17,
  "branchId": 1,
  "invoiceIsFor": "SERVICE",
  "serviceCategory": "General",
  "sqft": 1500.00,
  "financials": {
    "subtotal": 50000.00,
    "taxAmount": 9000.00,
    "discountAmount": 0.00,
    "grandTotal": 59000.00
  },
  "status": "DRAFT"
}
```
- **Success Response (201):** Created invoice object
- **Error Handling:**
  - 400: Validation error
  - 404: Customer or branch not found
  - 403: Unauthorized access

**API 2: Update/Confirm Invoice**

- **Endpoint:** `POST /api/v1/invoices/update`
- **Purpose:** Update invoice details or change status to CONFIRM
- **Request Body:**
```json
{
  "id": 1,
  "status": "CONFIRM"
}
```
- **Success Response (200):** Updated invoice object
- **Business Logic:**
  - Updates invoice status
  - If status changes to CONFIRM:
    - Publishes InvoiceConfirmedEvent
    - Automatically creates Payment record (PENDING status)
    - Updates AmcTracking if AMC-linked
- **Error Handling:**
  - 400: Validation error
  - 404: Invoice not found
  - 403: Unauthorized access

**API 3: Get All Invoices (Admin)**

- **Endpoint:** `GET /api/v1/invoices/all`
- **Purpose:** Fetch all invoices (company-wide, Admin access)
- **Success Response (200):** Array of invoice objects
- **Error Handling:**
  - 401: Unauthorized
  - 403: Access denied (non-admin users)

**API 4: Get Invoice by ID**

- **Endpoint:** `GET /api/v1/invoices/{id}`
- **Purpose:** Fetch specific invoice details
- **Request Parameter:** `id` (path parameter)
- **Success Response (200):** Detailed invoice object
- **Error Handling:**
  - 404: Invoice not found
  - 401: Unauthorized

**API 5: Get Branch-wise Invoices**

- **Endpoint:** `GET /api/v1/invoices/getAll/branchWise`
- **Purpose:** Fetch invoices for current user's branch
- **Success Response (200):** Array of invoice objects filtered by branch
- **Error Handling:**
  - 401: Unauthorized
  - 404: Branch not found

**API 6: Delete Invoice**

- **Endpoint:** `DELETE /api/v1/invoices/{id}`
- **Purpose:** Remove an invoice record
- **Request Parameter:** `id` (path parameter)
- **Success Response (200):** Success message
- **Error Handling:**
  - 404: Invoice not found
  - 403: Unauthorized access
  - 400: Invoice has dependencies (cannot delete)

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid invoice creation → Invoice created with unique number
- Valid confirmation → Invoice confirmed, payment record created
- Valid deletion → Invoice removed from system
- Branch-wise filtering → Only relevant invoices returned

**Failure Scenarios:**
- Missing customer ID → Validation error
- Service invoice without sqft → Validation error
- Invalid financial calculations → Validation error
- Invalid branch ID → Branch not found error
- Confirmed invoice with payments → Cannot delete error
- Invalid status transition → Validation error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View all invoices across branches
- **Restrictions:** Cannot create, update, or delete invoices
- **UI Behavior:** Invoices visible but all action buttons are disabled/hidden

**Branch Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create invoices
  - Update invoices
  - Confirm invoices
  - View branch invoices
  - Delete invoices
- **Restrictions:** None
- **UI Behavior:** Full access with all CRUD operations enabled

**Technician Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View invoices (read-only)
- **Restrictions:** Cannot create, update, or delete invoices
- **UI Behavior:** Invoice list visible but all action buttons are disabled/hidden

**Sales Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create invoices
  - Update invoices
  - Confirm invoices
  - View all branch invoices
  - Delete invoices
- **Restrictions:** None
- **UI Behavior:** Full access with all CRUD operations enabled

**Account Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create invoices
  - Update invoices
  - Confirm invoices
  - View all branch invoices
  - Delete invoices
- **Restrictions:** None
- **UI Behavior:** Full access with all CRUD operations enabled

---

## MODULE 14: PAYMENTS

### 4.1 Module Overview

**Purpose:** The Payments module manages payment records for invoices. Payment records are automatically created when invoices are confirmed. When payment status is updated to PAID, it automatically creates receipts and updates invoice payment status.

**Business Importance:** Critical for accounts receivable management. Tracks payment collection, updates invoice status, and triggers receipt generation. Integrates with AMC tracking for recurring service payments.

### 4.2 Features List

- Automatic payment record creation from confirmed invoices
- Update payment status (PENDING → PAID)
- Get all payments (company-wide)
- Get branch-wise payments
- Get payment by ID
- Get payment history for invoice
- Delete payment (admin only)
- Automatic receipt generation on payment
- Automatic invoice status update
- AMC amount tracking updates

### 4.3 User Flow

**Payment Record Creation Flow (Automatic):**
1. Invoice status is updated to CONFIRM
2. System automatically creates Payment record
3. Payment status is set to PENDING
4. Payment is linked to invoice
5. Payment appears in payments list

**Payment Settlement Flow:**
1. User navigates to Payments module
2. User views payment list
3. User clicks on specific payment (PENDING status)
4. System displays payment details
5. User enters payment details (payment method, paid date, amount)
6. User updates payment status to PAID
7. User submits update
8. System updates payment status to PAID
9. System updates linked Invoice status to PAID
10. System automatically creates Receipt record
11. If AMC-linked, system updates AmcTracking and AMC amounts
12. System creates ledger entry
13. System displays success message

### 4.4 Screens Involved

- Payment List Screen (with filters)
- Payment Details View Screen
- Payment Settlement Screen
- Payment History Screen (for invoice)

### 4.5 Field-Level Details

**Update Payment Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| id | Number | Yes | Payment ID |
| status | String | Yes | New status (PAID) |
| paymentMethod | String | Yes | Payment method (CASH, CHEQUE, ONLINE, etc.) |
| paymentPaidDate | Date | Yes | Date payment was received |
| amountPaid | Decimal | Yes | Amount received |
| paymentDelayDays | Number | No | Days delayed from invoice date |

**Payment Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique payment identifier |
| invoiceId | Number | Linked invoice ID |
| invoiceDetails | Object | Invoice information |
| customerId | Number | Customer ID |
| customerDetails | Object | Customer information |
| status | String | Current status (PENDING, PAID) |
| paymentMethod | String | Payment method |
| paymentPaidDate | Date | Date payment was received |
| amountPaid | Decimal | Amount received |
| paymentDelayDays | Number | Days delayed from invoice date |
| branchId | Number | Branch ID |
| branchName | String | Branch name |
| receiptId | Number | Linked receipt ID (if created) |
| createdAt | Date | Creation timestamp |
| lastModifiedAt | Date | Last update timestamp |

### 4.6 Validations & Business Rules

**Payment Creation Validations:**
- Invoice ID must exist
- Invoice must be in CONFIRM status
- Payment record is auto-created (no manual creation)

**Payment Update Validations:**
- Payment ID must exist
- Status must be PAID (cannot change from PAID back to PENDING)
- Payment method must be provided
- Payment paid date must be valid
- Amount paid must be positive

**Business Rules:**
- Payment records are automatically created when invoice is confirmed
- Payment status workflow: PENDING → PAID
- When payment status changes to PAID:
  - System updates linked Invoice status to PAID
  - System automatically creates Receipt record
  - System creates ledger entry
  - If AMC-linked, system updates AmcTracking (paymentPaidDate, receiptId)
  - If AMC-linked, system updates AMC amounts (totalCompleteAmount, totalRemainAmount)
- Payment delay days are calculated from invoice date
- Cannot delete payments with receipts (audit trail)
- Branch assignment determines visibility scope

### 4.7 API Interactions

**API 1: Update Payment (Settle Transaction)**

- **Endpoint:** `PUT /api/v1/payments/{id}`
- **Purpose:** Record receipt of funds and settle the invoice
- **Request Body:**
```json
{
  "status": "PAID",
  "paymentMethod": "ONLINE",
  "paymentPaidDate": "2026-01-15",
  "amountPaid": 59000.00,
  "paymentDelayDays": 5
}
```
- **Success Response (200):** Updated payment object
- **Business Logic:**
  - Updates payment status to PAID
  - Updates linked Invoice status to PAID
  - Calls ledgerEntryService.createLedgerBasedOnInvoicePaid(invoice)
  - Triggers addReceipt(savedPayment) to generate receipt
  - If AMC-linked, triggers updateAmcAmounts
- **Error Handling:**
  - 400: Validation error
  - 404: Payment not found
  - 403: Unauthorized access

**API 2: Get Payment by ID**

- **Endpoint:** `GET /api/v1/payments/{id}`
- **Purpose:** Fetch specific payment details
- **Request Parameter:** `id` (path parameter)
- **Success Response (200):** Detailed payment object
- **Error Handling:**
  - 404: Payment not found
  - 401: Unauthorized

**API 3: Get Payment History for Invoice**

- **Endpoint:** `GET /api/v1/payments/invoice/{invoiceId}`
- **Purpose:** Fetch all payment attempts/history for a specific invoice
- **Request Parameter:** `invoiceId` (path parameter)
- **Success Response (200):** Array of payment objects
- **Error Handling:**
  - 404: Invoice not found
  - 401: Unauthorized

**API 4: Get Branch-wise Payments**

- **Endpoint:** `GET /api/v1/payments/getAll/branchWise`
- **Purpose:** Fetch payments for current user's branch
- **Success Response (200):** Array of payment objects filtered by branch
- **Error Handling:**
  - 401: Unauthorized
  - 404: Branch not found

**API 5: Delete Payment**

- **Endpoint:** `DELETE /api/v1/payments/{id}`
- **Purpose:** Remove a payment record (admin only)
- **Request Parameter:** `id` (path parameter)
- **Success Response (200):** Success message
- **Note:** Generally restricted to Admin roles to preserve financial audit trails
- **Error Handling:**
  - 404: Payment not found
  - 403: Unauthorized access
  - 400: Payment has dependencies (cannot delete)

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid payment settlement → Payment updated, invoice and receipt created
- Valid payment history retrieval → All payment records returned
- Branch-wise filtering → Only relevant payments returned

**Failure Scenarios:**
- Payment not found → 404 error
- Invalid payment method → Validation error
- Invalid payment date → Validation error
- Payment with receipt → Cannot delete error
- Unauthorized deletion attempt → 403 error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View all payments across branches
- **Restrictions:** Cannot update or delete payments
- **UI Behavior:** Payments visible but all action buttons are disabled/hidden

**Branch Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - View branch payments
  - Update payment status
  - Delete payments (with restrictions)
- **Restrictions:** Cannot manually create payments (auto-created)
- **UI Behavior:** Full access with payment settlement enabled

**Technician Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View payments (read-only)
- **Restrictions:** Cannot update or delete payments
- **UI Behavior:** Payment list visible but all action buttons are disabled/hidden

**Sales Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View payments (read-only)
- **Restrictions:** Cannot update or delete payments
- **UI Behavior:** Payment list visible but all action buttons are disabled/hidden

**Account Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - View all branch payments
  - Update payment status
  - Delete payments (with restrictions)
- **Restrictions:** Cannot manually create payments (auto-created)
- **UI Behavior:** Full access with payment settlement enabled

---

## MODULE 15: RECEIPTS

### 4.1 Module Overview

**Purpose:** The Receipts module manages payment receipts. Receipts are automatically generated when payments are settled (status changes to PAID). Provides proof of payment to customers and maintains audit trail.

**Business Importance:** Essential for financial documentation and customer records. Provides proof of payment, maintains audit trail, and integrates with AMC tracking for service cycle documentation.

### 4.2 Features List

- Automatic receipt generation from paid payments
- Manual receipt creation (if auto-trigger fails)
- Update receipt details
- Get all receipts (company-wide)
- Get branch-wise receipts
- Get receipt by ID
- Delete receipt
- Receipt number auto-generation
- AMC tracking integration

### 4.3 User Flow

**Automatic Receipt Generation Flow:**
1. Payment status is updated to PAID
2. System automatically creates Receipt record
3. System generates unique receipt number (RCT format)
4. System pulls amountPaid and paymentMethod from Payment record
5. System links receipt to payment and invoice
6. If AMC-linked, system updates AmcTracking with receiptId and paymentPaidDate
7. Receipt appears in receipts list

**Manual Receipt Creation Flow:**
1. User navigates to Receipts module
2. User clicks "Create Receipt" button
3. System displays receipt creation form
4. User selects payment ID
5. User enters receipt details
6. User submits form
7. System creates receipt record
8. System generates unique receipt number
9. System displays success message

### 4.4 Screens Involved

- Receipt List Screen (with filters)
- Receipt Creation Form Screen (manual)
- Receipt Details View Screen
- Receipt Update Screen
- Receipt Print/PDF Screen

### 4.5 Field-Level Details

**Create Receipt Request (Manual)**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| paymentId | Number | Yes | Payment ID |
| invoiceId | Number | Yes | Invoice ID |
| customerId | Number | Yes | Customer ID |
| amountReceived | Decimal | Yes | Amount received (must be > 0) |
| paymentMethod | String | No | Payment method (from payment if not provided) |

**Update Receipt Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| id | Number | Yes | Receipt ID |
| notes | String | No | Updated notes or corrections |

**Receipt Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique receipt identifier |
| receiptNumber | String | Auto-generated receipt number (e.g., RCT001) |
| paymentId | Number | Linked payment ID |
| paymentDetails | Object | Payment information |
| invoiceId | Number | Linked invoice ID |
| invoiceDetails | Object | Invoice information |
| customerId | Number | Customer ID |
| customerDetails | Object | Customer information |
| amountReceived | Decimal | Amount received |
| paymentMethod | String | Payment method |
| receiptDate | Date | Receipt date |
| branchId | Number | Branch ID |
| branchName | String | Branch name |
| notes | String | Additional notes |
| createdAt | Date | Creation timestamp |
| lastModifiedAt | Date | Last update timestamp |

### 4.6 Validations & Business Rules

**Receipt Creation Validations:**
- Payment ID must exist and be valid
- Invoice ID must exist and be valid
- Customer ID must exist and be valid
- Amount received must be greater than 0
- Payment must be in PAID status (for auto-generation)

**Receipt Update Validations:**
- Receipt ID must exist
- Only notes can be updated (amount and payment method are locked)

**Business Rules:**
- Receipt number is auto-generated and unique (RCT format)
- Receipts are automatically created when payment status changes to PAID
- Receipt amount and payment method are pulled from Payment record
- Receipt date is set to payment paid date
- If AMC-linked, receiptId and paymentPaidDate are updated in AmcTracking
- Receipts should ideally not be editable or deletable once sent to customer (audit lock)
- Branch assignment determines visibility scope
- Receipt validation ensures no receipt is created without positive amount

### 4.7 API Interactions

**API 1: Create Receipt (Manual)**

- **Endpoint:** `POST /api/v1/receipts`
- **Purpose:** Manual generation of a receipt (used if auto-trigger fails)
- **Request Body:**
```json
{
  "paymentId": 1,
  "invoiceId": 10,
  "customerId": 17,
  "amountReceived": 59000.00,
  "paymentMethod": "ONLINE"
}
```
- **Success Response (201):** Created receipt object
- **Error Handling:**
  - 400: Validation error (invalid IDs, amount <= 0)
  - 404: Payment, invoice, or customer not found
  - 403: Unauthorized access

**API 2: Update Receipt**

- **Endpoint:** `PUT /api/v1/receipts/{id}`
- **Purpose:** Update notes or correction in details
- **Request Body:**
```json
{
  "notes": "Payment received via bank transfer"
}
```
- **Success Response (200):** Updated receipt object
- **Error Handling:**
  - 404: Receipt not found
  - 403: Unauthorized access

**API 3: Get All Receipts**

- **Endpoint:** `GET /api/v1/receipts`
- **Purpose:** Fetch all receipts (company-wide, Admin access)
- **Success Response (200):** Array of receipt objects
- **Error Handling:**
  - 401: Unauthorized
  - 403: Access denied (non-admin users)

**API 4: Get Receipt by ID**

- **Endpoint:** `GET /api/v1/receipts/{id}`
- **Purpose:** Detailed view of a single receipt
- **Request Parameter:** `id` (path parameter)
- **Success Response (200):** Detailed receipt object
- **Error Handling:**
  - 404: Receipt not found
  - 401: Unauthorized

**API 5: Get Branch-wise Receipts**

- **Endpoint:** `GET /api/v1/receipts/getAll/branchWise`
- **Purpose:** Fetch receipts for current user's branch
- **Success Response (200):** Array of receipt objects filtered by branch
- **Error Handling:**
  - 401: Unauthorized
  - 404: Branch not found

**API 6: Delete Receipt**

- **Endpoint:** `DELETE /api/v1/receipts/{id}`
- **Purpose:** Remove a receipt record
- **Request Parameter:** `id` (path parameter)
- **Success Response (200):** Success message
- **Note:** Generally restricted for audit purposes
- **Error Handling:**
  - 404: Receipt not found
  - 403: Unauthorized access

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid payment settlement → Receipt automatically generated
- Valid manual receipt creation → Receipt created with unique number
- Valid receipt update → Receipt notes updated
- Branch-wise filtering → Only relevant receipts returned

**Failure Scenarios:**
- Invalid payment ID → Validation error
- Invalid invoice ID → Validation error
- Amount <= 0 → Validation error
- Receipt not found → 404 error
- Unauthorized access → 403 error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View all receipts across branches
- **Restrictions:** Cannot create, update, or delete receipts
- **UI Behavior:** Receipts visible but all action buttons are disabled/hidden

**Branch Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - View branch receipts
  - Create receipts (manual)
  - Update receipts
  - Delete receipts (with restrictions)
- **Restrictions:** None
- **UI Behavior:** Full access with all CRUD operations enabled

**Technician Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View receipts (read-only)
- **Restrictions:** Cannot create, update, or delete receipts
- **UI Behavior:** Receipt list visible but all action buttons are disabled/hidden

**Sales Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View receipts (read-only)
- **Restrictions:** Cannot create, update, or delete receipts
- **UI Behavior:** Receipt list visible but all action buttons are disabled/hidden

**Account Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - View all branch receipts
  - Create receipts (manual)
  - Update receipts
  - Delete receipts (with restrictions)
- **Restrictions:** None
- **UI Behavior:** Full access with all CRUD operations enabled

---

## MODULE 16: TASK

### 4.1 Module Overview

**Purpose:** The Task module manages service tasks assigned to technicians. Tasks are created from sales orders or service requests and assigned to technicians for execution. Tracks task status, materials, images, and completion details.

**Business Importance:** Core operational module for field service management. Enables task assignment, tracking, and completion verification. Essential for service delivery and technician management.

### 4.2 Features List

- Create task
- Assign task to technician
- Get all tasks (branch-wise)
- Get task by ID
- Get task material list
- Get unassigned tasks
- Update task status
- Task completion workflow
- Material tracking
- Image uploads (before/after)

### 4.3 User Flow

**Task Creation and Assignment Flow:**
1. User navigates to Task module
2. User clicks "Create Task" button
3. System displays task creation form
4. User selects customer and service location
5. User selects service/product
6. User selects technician from dropdown
7. User enters task details (date, time, description)
8. User assigns materials (if required)
9. User submits form
10. System creates task with ASSIGNED status
11. System displays success message
12. Task appears in task list

**Task Completion Flow (Technician APK):**
1. Technician views assigned tasks
2. Technician clicks on specific task
3. Technician starts task (uploads selfie)
4. Task status changes to IN_PROGRESS
5. Technician performs service
6. Technician uploads before/after images
7. Technician updates materials used
8. System sends OTP to customer mobile
9. Technician enters OTP for verification
10. Technician submits feedback and rating
11. Task status changes to COMPLETED
12. System displays success message

### 4.4 Screens Involved

- Task List Screen (with filters)
- Task Creation Form Screen
- Task Assignment Screen
- Task Details View Screen
- Task Material List Screen
- Unassigned Tasks Screen

### 4.5 Field-Level Details

**Create Task Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| customerId | Number | Yes | Customer ID |
| serviceLocation | String | Yes | Service location address |
| taskName | String | Yes | Task name/description |
| assignedDate | Date | Yes | Task assignment date |
| assignedTime | Time | Yes | Task assignment time |
| technicianId | Number | Yes | Assigned technician ID |
| materialList | Array of Objects | No | Materials assigned to task |
| branchId | Number | Yes | Branch assignment |

**Task Material Object Structure:**
```json
{
  "materialId": 1,
  "quantity": 5
}
```

**Task Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique task identifier |
| taskName | String | Task name/description |
| customerId | Number | Customer ID |
| customerDetails | Object | Customer information |
| serviceLocation | String | Service location address |
| technicianId | Number | Assigned technician ID |
| technicianDetails | Object | Technician information |
| status | String | Current status (ASSIGNED, IN_PROGRESS, COMPLETED, CANCELLED) |
| assignedDate | Date | Task assignment date |
| assignedTime | Time | Task assignment time |
| startDate | Date | Task start date (when technician starts) |
| completedDate | Date | Task completion date |
| materials | Array | Material details |
| images | Array | Task images (selfie, before/after) |
| feedback | Object | Task completion feedback |
| branchId | Number | Branch ID |
| branchName | String | Branch name |
| createdAt | Date | Creation timestamp |
| lastModifiedAt | Date | Last update timestamp |

### 4.6 Validations & Business Rules

**Task Creation Validations:**
- Customer ID must exist
- Technician ID must exist
- Service location must not be empty
- Task name must not be empty
- Assigned date and time must be valid
- Branch ID must exist

**Task Assignment Validations:**
- Technician must be active
- Technician must have TECHNICIAN role
- Task cannot be assigned to multiple technicians

**Business Rules:**
- Task status workflow: ASSIGNED → IN_PROGRESS → COMPLETED/CANCELLED
- Tasks can only be started by assigned technician
- Task completion requires OTP verification
- Materials can be assigned during task creation
- Images (selfie, before/after) are required for completion
- Branch assignment determines visibility scope
- Unassigned tasks are visible to managers for assignment

### 4.7 API Interactions

**API 1: Get All Tasks (Branch-wise)**

- **Endpoint:** `GET /api/v1/task/all`
- **Purpose:** Retrieve all task details for current user's branch
- **Success Response (200):** Array of task objects with full details
- **Error Handling:**
  - 401: Unauthorized
  - 404: Branch not found

**API 2: Get Task by ID**

- **Endpoint:** `GET /api/v1/task/byId?taskId=3`
- **Purpose:** Retrieve detailed task information
- **Request Parameter:** `taskId` (required)
- **Success Response (200):** Detailed task object with technician, material, and image details
- **Error Handling:**
  - 404: Task not found
  - 401: Unauthorized

**API 3: Get Task Material List**

- **Endpoint:** `GET /api/v1/task/material?taskId=12`
- **Purpose:** Get material list for specific task
- **Request Parameter:** `taskId` (required)
- **Success Response (200):** Array of material objects
- **Error Handling:**
  - 404: Task not found
  - 401: Unauthorized

**API 4: Get Unassigned Tasks**

- **Endpoint:** `GET /api/v1/task/assign`
- **Purpose:** Get tasks that are not yet assigned to technicians
- **Success Response (200):** Array of unassigned task objects
- **Error Handling:**
  - 401: Unauthorized

**API 5: Search Tasks**

- **Endpoint:** `POST /api/v1/task/search`
- **Purpose:** Search and filter tasks with pagination
- **Request Body:** Filter criteria (technicianId, status, date range, etc.)
- **Success Response (200):** Paginated task results
- **Error Handling:**
  - 400: Validation error
  - 401: Unauthorized

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid task creation → Task created and assigned
- Valid task assignment → Task assigned to technician
- Valid task completion → Task completed with verification
- Valid search → Filtered task list returned

**Failure Scenarios:**
- Invalid customer ID → Validation error
- Invalid technician ID → Validation error
- Task not found → 404 error
- Unauthorized access → 403 error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View all tasks across branches
- **Restrictions:** Cannot create, update, or delete tasks
- **UI Behavior:** Tasks visible but all action buttons are disabled/hidden

**Branch Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create tasks
  - Assign tasks to technicians
  - View branch tasks
  - Update task status
  - Delete tasks
- **Restrictions:** None
- **UI Behavior:** Full access with all CRUD operations enabled

**Technician Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create tasks
  - Assign tasks to technicians
  - View all branch tasks
  - Update task status
  - Delete tasks
- **Restrictions:** None
- **UI Behavior:** Full access with all CRUD operations enabled

**Sales Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View tasks (read-only)
- **Restrictions:** Cannot create, update, or delete tasks
- **UI Behavior:** Task list visible but all action buttons are disabled/hidden

**Account Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View tasks (read-only)
- **Restrictions:** Cannot create, update, or delete tasks
- **UI Behavior:** Task list visible but all action buttons are disabled/hidden

---

## MODULE 17: LIVE TRACKING

### 4.1 Module Overview

**Purpose:** The Live Tracking module provides real-time location tracking of technicians in the field. Technicians update their location via mobile app, and managers can view their current location and movement history.

**Business Importance:** Enables real-time field monitoring, route optimization, and emergency response. Critical for field service management and technician safety.

### 4.2 Features List

- Update technician location
- View live technician locations
- View technician location history
- Track technician movement
- Filter by technician, date, branch

### 4.3 User Flow

**Location Update Flow (Technician APK):**
1. Technician opens mobile app
2. App automatically tracks location (with permission)
3. Technician performs service at location
4. App sends location update to server
5. System stores location with timestamp
6. Location appears on live tracking map

**Live Tracking View Flow (Manager):**
1. Manager navigates to Live Tracking module
2. System displays map with technician locations
3. Manager selects technician from filter
4. System shows selected technician's current location
5. Manager views location history
6. System displays movement path on map

### 4.4 Screens Involved

- Live Tracking Map Screen
- Technician Location List Screen
- Location History Screen
- Technician Filter Screen

### 4.5 Field-Level Details

**Update Location Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| technicianId | Number | Yes | Technician ID |
| latitude | Decimal | Yes | GPS latitude |
| longitude | Decimal | Yes | GPS longitude |
| timestamp | Date | Yes | Location timestamp |

**Location Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique location identifier |
| technicianId | Number | Technician ID |
| technicianDetails | Object | Technician information |
| latitude | Decimal | GPS latitude |
| longitude | Decimal | GPS longitude |
| timestamp | Date | Location timestamp |
| address | String | Resolved address (if available) |
| branchId | Number | Branch ID |
| branchName | String | Branch name |

### 4.6 Validations & Business Rules

**Location Update Validations:**
- Technician ID must exist
- Latitude must be valid (-90 to 90)
- Longitude must be valid (-180 to 180)
- Timestamp must be valid

**Business Rules:**
- Location updates are real-time
- Location history is maintained for tracking
- Branch assignment determines visibility scope
- Only active technicians are tracked
- Location data is used for route optimization

### 4.7 API Interactions

**API 1: Update Location**

- **Endpoint:** `POST /api/v1/track/update`
- **Purpose:** Update technician location
- **Request Body:**
```json
{
  "technicianId": 5,
  "latitude": 12.9716,
  "longitude": 77.5946,
  "timestamp": "2026-01-12T10:30:00"
}
```
- **Success Response (200):** Location update confirmation
- **Error Handling:**
  - 400: Validation error
  - 404: Technician not found
  - 401: Unauthorized

**API 2: Get Live Locations**

- **Endpoint:** `GET /api/v1/track/live`
- **Purpose:** Get current locations of all active technicians
- **Success Response (200):** Array of location objects
- **Error Handling:**
  - 401: Unauthorized

**API 3: Get Location History**

- **Endpoint:** `GET /api/v1/track/history?technicianId=5&date=2026-01-12`
- **Purpose:** Get location history for specific technician and date
- **Request Parameters:** `technicianId` (required), `date` (optional)
- **Success Response (200):** Array of location objects
- **Error Handling:**
  - 404: Technician not found
  - 401: Unauthorized

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid location update → Location recorded successfully
- Valid live tracking → Current locations returned
- Valid history retrieval → Location history returned

**Failure Scenarios:**
- Invalid coordinates → Validation error
- Technician not found → 404 error
- Unauthorized access → 403 error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View live technician locations across branches
- **Restrictions:** Cannot update locations
- **UI Behavior:** Live tracking map visible but update controls disabled

**Branch Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - View live technician locations for branch
  - View location history
- **Restrictions:** Cannot manually update locations (auto-updated by app)
- **UI Behavior:** Full access to live tracking features

**Technician Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - View live technician locations for branch
  - View location history
- **Restrictions:** Cannot manually update locations (auto-updated by app)
- **UI Behavior:** Full access to live tracking features

**Sales Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Live Tracking module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

**Account Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Live Tracking module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

---

## MODULE 18: CUSTOMER SUPPORT

### 4.1 Module Overview

**Purpose:** The Customer Support module manages support tickets raised against tasks. Technicians or managers can create tickets when issues arise during task execution. Enables issue tracking, assignment, and resolution.

**Business Importance:** Critical for customer service and issue resolution. Tracks problems, enables reassignment, and maintains support history. Essential for service quality management.

### 4.2 Features List

- Create ticket against task
- Get all tickets
- Get tickets with full details (dashboard)
- Filter/search tickets
- Delete ticket
- Update ticket status
- Assign ticket to technician
- Update task schedule from ticket

### 4.3 User Flow

**Ticket Creation Flow:**
1. User navigates to Customer Support module
2. User views task list or specific task
3. User clicks "Create Ticket" button
4. System displays ticket creation form
5. User selects task with issue
6. User enters issue description
7. User optionally assigns new technician
8. User optionally updates task date/time
9. User submits form
10. System creates ticket with OPEN status
11. System updates task assignment/schedule if provided
12. System displays success message
13. Ticket appears in tickets list

**Ticket Management Flow:**
1. User views ticket list
2. User clicks on specific ticket
3. System displays ticket details
4. User updates ticket status (e.g., to RESOLVED)
5. User submits update
6. System updates ticket status
7. System displays success message

### 4.4 Screens Involved

- Ticket List Screen (with filters)
- Ticket Creation Form Screen
- Ticket Details View Screen
- Ticket Update Screen
- Ticket Dashboard Screen

### 4.5 Field-Level Details

**Create Ticket Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| taskId | Number | Yes | Task ID with issue |
| issueDescription | String | Yes | Description of the issue |
| technicianId | Number | No | New technician assignment (if reassigning) |
| taskDate | Date | No | Updated task date (if rescheduling) |
| taskTime | Time | No | Updated task time (if rescheduling) |

**Ticket Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique ticket identifier |
| taskId | Number | Associated task ID |
| taskDetails | Object | Task information |
| issueDescription | String | Issue description |
| status | String | Current status (OPEN, IN_PROGRESS, RESOLVED, CLOSED) |
| technicianId | Number | Assigned technician ID |
| technicianDetails | Object | Technician information |
| createdBy | Number | User ID who created ticket |
| createdAt | Date | Creation timestamp |
| lastModifiedAt | Date | Last update timestamp |

### 4.6 Validations & Business Rules

**Ticket Creation Validations:**
- Task ID must exist
- Task must have an issue (issueDescription required)
- Technician ID must exist (if provided)
- Date and time must be valid (if provided)

**Ticket Update Validations:**
- Ticket ID must exist
- Status must be a valid enum value

**Business Rules:**
- Ticket status workflow: OPEN → IN_PROGRESS → RESOLVED → CLOSED
- If new technician assigned, task assignment is updated
- If new date/time provided, task schedule is updated
- Only TECHNICIAN MANAGER can create and delete tickets
- Branch assignment determines visibility scope
- Tickets are linked to tasks for issue tracking

### 4.7 API Interactions

**API 1: Create Ticket**

- **Endpoint:** `POST /api/tickets/Create`
- **Purpose:** Raise ticket against specific task
- **Request Body:**
```json
{
  "taskId": 10,
  "issueDescription": "Customer not available at scheduled time",
  "technicianId": 5,
  "taskDate": "2026-01-15",
  "taskTime": "14:00:00"
}
```
- **Success Response (201):** Created ticket object
- **Access:** TECHNICIAN MANAGER only
- **Error Handling:**
  - 400: Validation error
  - 404: Task or technician not found
  - 403: Unauthorized access

**API 2: Get All Tickets**

- **Endpoint:** `GET /api/tickets`
- **Purpose:** Retrieve all tickets with basic details
- **Success Response (200):** Array of ticket objects
- **Error Handling:**
  - 401: Unauthorized

**API 3: Get All Tickets (Dashboard)**

- **Endpoint:** `GET /api/tickets/dash`
- **Purpose:** Retrieve tickets with technician and other details for dashboard
- **Success Response (200):** Array of ticket objects with full details
- **Error Handling:**
  - 401: Unauthorized

**API 4: Filter Tickets**

- **Endpoint:** `POST /api/tickets/search`
- **Purpose:** Filter and search tickets
- **Request Body:** Filter criteria with pagination
- **Success Response (200):** Paginated ticket results
- **Error Handling:**
  - 400: Validation error
  - 401: Unauthorized

**API 5: Delete Ticket**

- **Endpoint:** `DELETE /api/tickets?ticketId=1`
- **Purpose:** Delete ticket by ID
- **Request Parameter:** `ticketId` (required)
- **Success Response (200):** Success message
- **Access:** TECHNICIAN MANAGER only
- **Error Handling:**
  - 404: Ticket not found
  - 403: Unauthorized access

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid ticket creation → Ticket created, task updated if needed
- Valid filters → Filtered ticket list returned
- Valid deletion → Ticket deleted successfully

**Failure Scenarios:**
- Invalid task ID → Error response
- Missing issue description → Validation error
- Unauthorized role → 403 error
- Invalid ticket ID for deletion → Error response

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View all support tickets across branches
- **Restrictions:** Cannot create, update, or delete tickets
- **UI Behavior:** Tickets visible but action buttons (Create, Delete) are disabled/hidden

**Branch Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View support tickets related to their branch
- **Restrictions:** Cannot create or manage tickets
- **UI Behavior:** Tickets visible but action buttons are disabled/hidden

**Technician Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Create tickets
  - View all branch tickets
  - Update ticket status
  - Assign tickets to technicians
  - Delete tickets
- **Restrictions:** None
- **UI Behavior:** Full access with all action buttons enabled

**Sales Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Customer Support module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

**Account Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Customer Support module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

---

## MODULE 19: CHECK IN CHECK OUT

### 4.1 Module Overview

**Purpose:** The Check In Check Out module manages employee attendance through punch IN/OUT functionality. Technicians use the mobile app to record their attendance. Tracks daily attendance, work hours, and location.

**Business Importance:** Essential for attendance management and payroll calculation. Ensures accurate time tracking for field employees. Critical for HR and payroll operations.

### 4.2 Features List

- Punch IN for attendance
- Punch OUT for attendance
- View attendance records
- Get daily attendance status
- Location-based punch (optional)
- Attendance history

### 4.3 User Flow

**Punch IN Flow (Technician APK):**
1. Technician opens mobile app
2. Technician navigates to Attendance section
3. Technician clicks "Punch IN" button
4. System records current time and location
5. System creates attendance record
6. System displays success message
7. Technician can now view assigned tasks

**Punch OUT Flow (Technician APK):**
1. Technician completes work for the day
2. Technician navigates to Attendance section
3. Technician clicks "Punch OUT" button
4. System records current time
5. System calculates work hours
6. System updates attendance record
7. System displays success message

### 4.4 Screens Involved

- Attendance Punch Screen (Mobile App)
- Attendance History Screen
- Daily Attendance Status Screen

### 4.5 Field-Level Details

**Punch Status Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| status | String (Enum) | Yes | "PUNCH_IN" or "PUNCH_OUT" |

**Attendance Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique attendance identifier |
| userId | Number | User/Technician ID |
| punchInTime | Date | Punch IN timestamp |
| punchOutTime | Date | Punch OUT timestamp |
| workHours | Decimal | Calculated work hours |
| date | Date | Attendance date |
| status | String | Attendance status |
| location | Object | Punch location (if available) |

### 4.6 Validations & Business Rules

**Punch Validations:**
- User must be logged in
- Status must be PUNCH_IN or PUNCH_OUT
- Cannot punch IN twice on same day
- Cannot punch OUT without punching IN
- Punch OUT time must be after punch IN time

**Business Rules:**
- One punch IN per day per user
- One punch OUT per day per user
- Work hours = punchOutTime - punchInTime
- Attendance records are date-specific
- Branch assignment determines visibility scope

### 4.7 API Interactions

**API 1: Punch IN/OUT**

- **Endpoint:** `PATCH /api/v1/user-attendance/punch-status?status={PUNCH_IN|PUNCH_OUT}`
- **Purpose:** Record attendance punch
- **Request Parameter:** `status` (required) - "PUNCH_IN" or "PUNCH_OUT"
- **Success Response (200):** Attendance record
- **Error Handling:**
  - 400: Validation error (already punched IN, no punch IN for OUT, etc.)
  - 401: Unauthorized
  - 403: Access denied

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid punch IN → Attendance recorded, punch IN time saved
- Valid punch OUT → Attendance updated, work hours calculated

**Failure Scenarios:**
- Already punched IN → Validation error
- Punch OUT without punch IN → Validation error
- Invalid status → Validation error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View attendance records across branches
- **Restrictions:** Cannot punch IN/OUT
- **UI Behavior:** Attendance records visible but punch controls disabled

**Branch Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View attendance records for branch
- **Restrictions:** Cannot punch IN/OUT
- **UI Behavior:** Attendance records visible but punch controls disabled

**Technician Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View attendance records for branch
- **Restrictions:** Cannot punch IN/OUT
- **UI Behavior:** Attendance records visible but punch controls disabled

**Sales Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Check In Check Out module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

**Account Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Check In Check Out module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

**Technician (Mobile App)**

- **Access Level:** Full Access
- **Allowed Actions:**
  - Punch IN
  - Punch OUT
  - View own attendance history
- **Restrictions:** None
- **UI Behavior:** Full access via mobile app

---

## MODULE 20: ATTENDANCE

### 4.1 Module Overview

**Purpose:** The Attendance module provides comprehensive attendance management and reporting. Tracks employee attendance, calculates work hours, and generates attendance reports for payroll and HR purposes.

**Business Importance:** Essential for HR operations, payroll calculation, and employee management. Provides attendance analytics and reporting capabilities.

### 4.2 Features List

- View attendance records
- Get attendance by user
- Get attendance by date range
- Get branch-wise attendance
- Calculate work hours
- Attendance reports
- Attendance analytics

### 4.3 User Flow

**Attendance View Flow:**
1. User navigates to Attendance module
2. System displays attendance list
3. User filters by date range, user, or branch
4. System displays filtered attendance records
5. User views attendance details
6. System shows work hours and status

### 4.4 Screens Involved

- Attendance List Screen (with filters)
- Attendance Details View Screen
- Attendance Report Screen
- Attendance Analytics Dashboard

### 4.5 Field-Level Details

**Attendance Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique attendance identifier |
| userId | Number | User ID |
| userDetails | Object | User information |
| date | Date | Attendance date |
| punchInTime | Date | Punch IN timestamp |
| punchOutTime | Date | Punch OUT timestamp |
| workHours | Decimal | Calculated work hours |
| status | String | Attendance status (PRESENT, ABSENT, HALF_DAY) |
| branchId | Number | Branch ID |
| branchName | String | Branch name |

### 4.6 Validations & Business Rules

**Attendance Validations:**
- User ID must exist
- Date must be valid
- Work hours calculation requires both punch IN and OUT

**Business Rules:**
- Attendance records are created from punch IN/OUT
- Work hours = punchOutTime - punchInTime
- Attendance status is determined by punch records
- Branch assignment determines visibility scope
- Attendance reports are generated for payroll

### 4.7 API Interactions

**API 1: Get Attendance Records**

- **Endpoint:** `GET /api/v1/attendance`
- **Purpose:** Retrieve attendance records with filters
- **Request Parameters:** `userId`, `startDate`, `endDate`, `branchId` (all optional)
- **Success Response (200):** Array of attendance objects
- **Error Handling:**
  - 401: Unauthorized

**API 2: Get Attendance by User**

- **Endpoint:** `GET /api/v1/attendance/user/{userId}`
- **Purpose:** Get attendance records for specific user
- **Request Parameter:** `userId` (path parameter)
- **Success Response (200):** Array of attendance objects
- **Error Handling:**
  - 404: User not found
  - 401: Unauthorized

**API 3: Get Branch-wise Attendance**

- **Endpoint:** `GET /api/v1/attendance/branchWise`
- **Purpose:** Get attendance records for current user's branch
- **Success Response (200):** Array of attendance objects filtered by branch
- **Error Handling:**
  - 401: Unauthorized
  - 404: Branch not found

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid attendance retrieval → Attendance records returned
- Valid filtering → Filtered attendance list returned

**Failure Scenarios:**
- Invalid user ID → 404 error
- Invalid date range → Validation error
- Unauthorized access → 403 error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View all attendance records across branches
- **Restrictions:** Cannot create, update, or delete attendance
- **UI Behavior:** Attendance records visible but all action buttons are disabled/hidden

**Branch Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View attendance records for branch
- **Restrictions:** Cannot create, update, or delete attendance
- **UI Behavior:** Attendance records visible but all action buttons are disabled/hidden

**Technician Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View attendance records for branch
- **Restrictions:** Cannot create, update, or delete attendance
- **UI Behavior:** Attendance records visible but all action buttons are disabled/hidden

**Sales Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Attendance module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

**Account Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View attendance records (read-only)
- **Restrictions:** Cannot create, update, or delete attendance
- **UI Behavior:** Attendance records visible but all action buttons are disabled/hidden

---

## MODULE 21: SALARY

### 4.1 Module Overview

**Purpose:** The Salary module manages employee salary information, calculations, and payroll processing. Tracks salary details, deductions, and generates salary reports.

**Business Importance:** Critical for HR and finance operations. Enables payroll management, salary calculations, and financial reporting.

### 4.2 Features List

- View salary information
- Calculate salary based on attendance
- Get salary by user
- Get salary reports
- Salary history

### 4.3 User Flow

**Salary View Flow:**
1. User navigates to Salary module
2. System displays salary list or user selection
3. User selects employee or date range
4. System displays salary information
5. User views salary details and calculations

### 4.4 Screens Involved

- Salary List Screen
- Salary Details View Screen
- Salary Report Screen
- Salary Calculation Screen

### 4.5 Field-Level Details

**Salary Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique salary identifier |
| userId | Number | User ID |
| userDetails | Object | User information |
| month | String | Salary month |
| year | Number | Salary year |
| baseSalary | Decimal | Base salary amount |
| allowances | Decimal | Allowances |
| deductions | Decimal | Deductions |
| netSalary | Decimal | Net salary amount |
| attendanceDays | Number | Number of days present |
| branchId | Number | Branch ID |
| branchName | String | Branch name |

### 4.6 Validations & Business Rules

**Salary Validations:**
- User ID must exist
- Month and year must be valid
- Salary amounts must be positive

**Business Rules:**
- Salary is calculated based on attendance
- Net salary = baseSalary + allowances - deductions
- Salary records are monthly
- Branch assignment determines visibility scope

### 4.7 API Interactions

**API 1: Get Salary Information**

- **Endpoint:** `GET /api/v1/salary`
- **Purpose:** Retrieve salary information with filters
- **Request Parameters:** `userId`, `month`, `year`, `branchId` (all optional)
- **Success Response (200):** Array of salary objects
- **Error Handling:**
  - 401: Unauthorized

**API 2: Get Salary by User**

- **Endpoint:** `GET /api/v1/salary/user/{userId}`
- **Purpose:** Get salary records for specific user
- **Request Parameter:** `userId` (path parameter)
- **Success Response (200):** Array of salary objects
- **Error Handling:**
  - 404: User not found
  - 401: Unauthorized

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid salary retrieval → Salary information returned
- Valid filtering → Filtered salary list returned

**Failure Scenarios:**
- Invalid user ID → 404 error
- Invalid month/year → Validation error
- Unauthorized access → 403 error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View all salary information across branches
- **Restrictions:** Cannot create, update, or delete salary records
- **UI Behavior:** Salary information visible but all action buttons are disabled/hidden

**Branch Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View salary information for branch
- **Restrictions:** Cannot create, update, or delete salary records
- **UI Behavior:** Salary information visible but all action buttons are disabled/hidden

**Technician Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View salary information for branch
- **Restrictions:** Cannot create, update, or delete salary records
- **UI Behavior:** Salary information visible but all action buttons are disabled/hidden

**Sales Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Salary module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

**Account Manager**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View salary information (read-only)
- **Restrictions:** Cannot create, update, or delete salary records
- **UI Behavior:** Salary information visible but all action buttons are disabled/hidden

---

## MODULE 22: LEAVE

### 4.1 Module Overview

**Purpose:** The Leave module manages employee leave applications and approvals. Employees can apply for leave, and managers can approve or reject leave requests. Tracks leave balance and history.

**Business Importance:** Essential for HR operations and employee management. Enables leave tracking, balance management, and approval workflows.

### 4.2 Features List

- Apply for leave
- View leave applications
- Approve/reject leave
- Get leave balance
- Leave history
- Leave reports

### 4.3 User Flow

**Leave Application Flow (Technician APK):**
1. Technician opens mobile app
2. Technician navigates to Leave section
3. Technician clicks "Apply Leave" button
4. System displays leave application form
5. Technician selects leave type
6. Technician enters start date and end date
7. Technician enters reason
8. Technician submits application
9. System creates leave application with PENDING status
10. System displays success message

**Leave Approval Flow (Manager):**
1. Manager navigates to Leave module
2. System displays pending leave applications
3. Manager clicks on specific application
4. System displays leave details
5. Manager approves or rejects leave
6. System updates leave status
7. System updates leave balance if approved
8. System displays success message

### 4.4 Screens Involved

- Leave Application Form Screen (Mobile App)
- Leave List Screen
- Leave Approval Screen
- Leave Balance Screen
- Leave History Screen

### 4.5 Field-Level Details

**Apply Leave Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| leaveType | String | Yes | Type of leave (SICK, CASUAL, EARNED, etc.) |
| startDate | Date | Yes | Leave start date |
| endDate | Date | Yes | Leave end date |
| reason | String | Yes | Reason for leave |
| userId | Number | Yes | User ID (from token) |

**Leave Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | Unique leave identifier |
| userId | Number | User ID |
| userDetails | Object | User information |
| leaveType | String | Type of leave |
| startDate | Date | Leave start date |
| endDate | Date | Leave end date |
| numberOfDays | Number | Calculated number of days |
| reason | String | Reason for leave |
| status | String | Current status (PENDING, APPROVED, REJECTED) |
| approvedBy | Number | User ID who approved/rejected |
| approvedDate | Date | Approval/rejection date |
| branchId | Number | Branch ID |
| branchName | String | Branch name |

### 4.6 Validations & Business Rules

**Leave Application Validations:**
- Leave type must be valid
- Start date must be valid
- End date must be valid and after start date
- Reason must not be empty
- User must have sufficient leave balance

**Leave Approval Validations:**
- Leave application ID must exist
- Status must be APPROVED or REJECTED
- Only managers can approve/reject

**Business Rules:**
- Leave status workflow: PENDING → APPROVED/REJECTED
- Number of days = endDate - startDate + 1
- Leave balance is updated when approved
- Cannot apply for past dates
- Branch assignment determines visibility scope

### 4.7 API Interactions

**API 1: Apply Leave**

- **Endpoint:** `POST /leave/apply`
- **Purpose:** Submit leave application
- **Request Body:**
```json
{
  "leaveType": "CASUAL",
  "startDate": "2026-01-15",
  "endDate": "2026-01-17",
  "reason": "Personal work"
}
```
- **Success Response (201):** Created leave application object
- **Error Handling:**
  - 400: Validation error
  - 401: Unauthorized
  - 403: Insufficient leave balance

**API 2: Get Leave Applications**

- **Endpoint:** `GET /api/v1/leave`
- **Purpose:** Retrieve leave applications with filters
- **Request Parameters:** `userId`, `status`, `branchId` (all optional)
- **Success Response (200):** Array of leave objects
- **Error Handling:**
  - 401: Unauthorized

**API 3: Approve/Reject Leave**

- **Endpoint:** `PUT /api/v1/leave/{id}/approve`
- **Purpose:** Approve or reject leave application
- **Request Body:**
```json
{
  "status": "APPROVED",
  "remarks": "Approved"
}
```
- **Success Response (200):** Updated leave object
- **Access:** Managers only
- **Error Handling:**
  - 404: Leave application not found
  - 403: Unauthorized access

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid leave application → Leave application created
- Valid approval → Leave approved, balance updated
- Valid rejection → Leave rejected

**Failure Scenarios:**
- Invalid date range → Validation error
- Insufficient leave balance → Validation error
- Past dates → Validation error
- Unauthorized approval → 403 error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** View-Only Access
- **Allowed Actions:**
  - View all leave applications across branches
- **Restrictions:** Cannot apply or approve leave
- **UI Behavior:** Leave applications visible but action buttons are disabled/hidden

**Branch Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - View leave applications for branch
  - Approve/reject leave applications
- **Restrictions:** Cannot apply for own leave (use profile module)
- **UI Behavior:** Full access with approval controls enabled

**Technician Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - View leave applications for branch
  - Approve/reject leave applications
- **Restrictions:** Cannot apply for own leave (use profile module)
- **UI Behavior:** Full access with approval controls enabled

**Sales Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Leave module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

**Account Manager**

- **Access Level:** No Access
- **Allowed Actions:** None
- **Restrictions:** Leave module should be hidden/disabled
- **UI Behavior:** Module not visible in navigation

**Technician (Mobile App)**

- **Access Level:** Full Access (Application Only)
- **Allowed Actions:**
  - Apply for leave
  - View own leave applications
  - View leave balance
- **Restrictions:** Cannot approve/reject leave
- **UI Behavior:** Full access to leave application features via mobile app

---

## MODULE 23: PROFILE

### 4.1 Module Overview

**Purpose:** The Profile module allows users to view and update their personal information. Users can update their name, phone number, and profile image. Provides access to user's own account details.

**Business Importance:** Essential for user account management. Enables users to maintain their profile information and access their account details.

### 4.2 Features List

- View own profile
- Update profile information
- Update profile image
- View account details
- View assigned modules and permissions

### 4.3 User Flow

**Profile View Flow:**
1. User navigates to Profile module
2. System displays current profile information
3. User views account details, roles, and permissions
4. User views assigned modules

**Profile Update Flow:**
1. User navigates to Profile module
2. User clicks "Edit Profile" button
3. System displays profile edit form
4. User updates name, phone number
5. User optionally uploads new profile image
6. User submits form
7. System validates and updates profile
8. System uploads image to S3 (if provided)
9. System displays success message
10. Updated information is reflected

### 4.4 Screens Involved

- Profile View Screen
- Profile Edit Screen
- Profile Image Upload Screen

### 4.5 Field-Level Details

**Update Profile Request**

| Field Name | Type | Mandatory | Description |
|------------|------|-----------|-------------|
| firstName | String | Yes | Updated first name |
| lastName | String | Yes | Updated last name |
| phoneNo | Number | Yes | Updated phone number |
| files | Array of Objects | No | Profile image upload |

**File Object Structure:**
```json
{
  "fileName": "profile.jpg",
  "fileType": "image/jpeg",
  "fileData": "<base64_encoded_string>"
}
```

**Profile Response**

| Field Name | Type | Description |
|------------|------|-------------|
| id | Number | User ID |
| firstName | String | First name |
| lastName | String | Last name |
| email | String | Email address (immutable) |
| phoneNo | Number | Phone number |
| profileUrl | String | Profile image URL |
| roles | Array | User roles |
| branchId | Number | Branch ID |
| branchName | String | Branch name |
| modules | Object | Assigned modules and permissions |
| createdAt | Date | Account creation timestamp |
| lastModifiedAt | Date | Last update timestamp |

### 4.6 Validations & Business Rules

**Profile Update Validations:**
- First name and last name must not be empty
- Phone number must be exactly 10 digits
- Image file type must be image/* (JPEG, PNG, JPG)
- Image file size limit (not specified in documentation)

**Business Rules:**
- Email cannot be changed (immutable)
- Users can only update their own profile
- Profile image is uploaded to S3
- Profile URL is updated after image upload
- Branch and role information are read-only
- Module permissions are read-only

### 4.7 API Interactions

**API 1: Update User Profile**

- **Endpoint:** `PUT /api/v1/user/profile/update`
- **Purpose:** Update current logged-in user's profile details
- **Request Body:**
```json
{
  "firstName": "XYZ",
  "lastName": "ABC",
  "phoneNo": 1234567890,
  "files": [
    {
      "fileName": "profile.jpg",
      "fileType": "image/jpeg",
      "fileData": "<base64_string>"
    }
  ]
}
```
- **Success Response (200):**
```json
{
  "status": 200,
  "message": "Profile updated successfully",
  "data": {
    "id": 1,
    "firstName": "XYZ",
    "lastName": "ABC",
    "email": "user@example.com",
    "phoneNo": 1234567890,
    "profileUrl": "https://s3.amazonaws.com/.../profile.jpg",
    "updatedAt": "2026-01-12T10:30:00"
  }
}
```
- **Error Handling:**
  - 400: Validation error
  - 401: Unauthorized
  - 500: File upload error

**API 2: Get User Details**

- **Endpoint:** `GET /api/v1/users/{id}` or `GET /api/v1/users/byEmail`
- **Purpose:** Retrieve user details (for profile view)
- **Success Response (200):** User details object with roles and modules
- **Error Handling:**
  - 404: User not found
  - 401: Unauthorized

### 4.8 Error & Success Scenarios

**Success Scenarios:**
- Valid profile update → Profile updated with new image URL
- Valid image upload → Image uploaded to S3, URL updated

**Failure Scenarios:**
- Invalid phone format → Validation error
- Invalid image format → Validation error
- File upload failure → 500 error
- Unauthorized access → 403 error

### 4.9 ⭐ ROLE-WISE MODULE ACCESS & BEHAVIOR

**Company Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - View own profile
  - Update profile information
  - Update profile image
- **Restrictions:** Cannot change email, roles, or branch
- **UI Behavior:** Full access to profile management

**Branch Admin**

- **Access Level:** Full Access
- **Allowed Actions:**
  - View own profile
  - Update profile information
  - Update profile image
- **Restrictions:** Cannot change email, roles, or branch
- **UI Behavior:** Full access to profile management

**Technician Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - View own profile
  - Update profile information
  - Update profile image
- **Restrictions:** Cannot change email, roles, or branch
- **UI Behavior:** Full access to profile management

**Sales Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - View own profile
  - Update profile information
  - Update profile image
- **Restrictions:** Cannot change email, roles, or branch
- **UI Behavior:** Full access to profile management

**Account Manager**

- **Access Level:** Full Access
- **Allowed Actions:**
  - View own profile
  - Update profile information
  - Update profile image
- **Restrictions:** Cannot change email, roles, or branch
- **UI Behavior:** Full access to profile management

**All Users**

- **Access Level:** Full Access (Own Profile Only)
- **Allowed Actions:**
  - View own profile
  - Update own profile information
  - Update own profile image
- **Restrictions:** Cannot view or update other users' profiles
- **UI Behavior:** Full access to own profile management

---

## END OF MODULE-WISE DETAILED DOCUMENTATION

This completes the comprehensive module-wise documentation for all 23 modules of the ERP system. Each module includes:

- Module Overview (Purpose and Business Importance)
- Features List
- User Flow
- Screens Involved
- Field-Level Details (with request/response field tables)
- Validations & Business Rules
- API Interactions (with endpoints, request/response examples, and error handling)
- Error & Success Scenarios
- Role-wise Module Access & Behavior

All modules follow the same structured format for consistency and ease of reference.

---
