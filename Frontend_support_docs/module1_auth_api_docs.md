# Module 1: Authentication API Documentation

## Short Description
The Authentication module manages secure access control for the ERP system. It handles three primary user types: Super Admin (Root), Company Admin (CEO), and Client Users (IAM/Tenant Users). It includes functionality for public company registration, email-based account verification, multi-tenant login, secure session management via JWT (Access & Refresh tokens), and logout.

---

## APIs

### Root Admin Login
- **Method:** `POST`
- **Endpoint:** `/api/v1/auth/root/login`
- **Purpose:** Authenticate as a global platform administrator.
- **Use Case:** Global platform management, system-wide configuration, and tenant oversight.
- **Used In:** Super Admin Login Screen (Section 1.1).
- **Request Body:** [LoginRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/auth/dto/request/LoginRequest.java#5-10)
    - `identifier` (String): Root username or email.
    - `password` (String): Secure root password.
- **Response:** [AuthResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/auth/dto/response/AuthResponse.java#7-38)
    - Returns JWT tokens and user metadata with `userType: ROOT`.
- **Access / Security:** Publicly accessible but restricted to root credentials stored in the system.
- **Notes:** No tenant context is required for this endpoint.

### Company Admin (CEO) Signup
- **Method:** `POST`
- **Endpoint:** `/api/v1/auth/signup`
- **Purpose:** Public registration for a new company and its CEO user.
- **Use Case:** Initial onboarding of a new client company.
- **Used In:** Company Admin Sign Up Screen (Section 1.2).
- **Request Body:** [SignUpRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/globalUser/dto/request/SignUpRequest.java#5-37)
    - `companyName` (String): Min 3 chars, alphanumeric.
    - `fullName` (String): Name of the authorized person.
    - `email` (String): Must be unique.
    - `username` (String): Chosen login username.
    - `password` (String): Min 8 chars, 1 uppercase, 1 number.
    - `confirmPassword` (String): Must match password.
    - `phoneNumber` (String): 10 digits, Indian format.
- **Response:** `ResponseStructure<SignUpResponse>`
    - Includes `userId`, `username`, and confirmation message.
- **Access / Security:** Publicly accessible.
- **Notes:** Tenant workspace and database schema are provisioned only AFTER successful email verification.

### Company Admin (CEO) Login
- **Method:** `POST`
- **Endpoint:** `/api/v1/auth/ceo/login`
- **Purpose:** Authenticate as a company owner/CEO.
- **Use Case:** Access to Company Admin dashboard, onboarding steps, and subscription management.
- **Used In:** Root Login option -> Company Admin Login Screen (Section 1.4).
- **Request Body:** [LoginRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/auth/dto/request/LoginRequest.java#5-10)
- **Response:** [AuthResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/auth/dto/response/AuthResponse.java#7-38)
    - Returns JWT tokens and metadata with `userType: CEO`.
- **Access / Security:** Publicly accessible.
- **Notes:** Used by the primary account holder to manage the entire tenant.

### Tenant User (IAM User) Login
- **Method:** `POST`
- **Endpoint:** `/api/v1/auth/login`
- **Purpose:** Authenticate as a regular employee/staff member within a specific company.
- **Use Case:** Daily operations, task management, and role-based activities.
- **Used In:** User Login option -> IAM User Login Screen (Section 1.5).
- **Request Params / Query Params:**
    - `X-Tenant-ID` (Header, Mandatory): The unique Tenant/Account ID.
- **Request Body:** [LoginRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/auth/dto/request/LoginRequest.java#5-10)
- **Response:** [AuthResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/auth/dto/response/AuthResponse.java#7-38)
    - Returns JWT tokens and metadata with `userType: TENANT`.
- **Access / Security:** Publicly accessible but requires valid `X-Tenant-ID`.
- **Notes:** The `X-Tenant-ID` header is used for database schema routing.

### Email Verification
- **Method:** `GET`
- **Endpoint:** `/api/v1/auth/verify-email`
- **Purpose:** Verify the CEO's email address using a token sent during signup.
- **Use Case:** Completing the registration process and activating the tenant workspace.
- **Used In:** Link sent to the user's email inbox.
- **Request Params / Query Params:**
    - `token` (String, Query): The unique verification token.
- **Response:** `ResponseStructure<String>`
    - Success message indicating the account is active.
- **Access / Security:** Token-based public endpoint.
- **Notes:** Tokens typically expire after 24 hours.

### Refresh Access Token
- **Method:** `POST`
- **Endpoint:** `/api/v1/auth/refresh`
- **Purpose:** Generate a new Access Token using a valid Refresh Token.
- **Use Case:** Maintaining seamless user session without requiring re-login once the access token expires.
- **Used In:** Frontend background intercepters.
- **Request Body:** [RefreshTokenRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/auth/dto/request/RefreshTokenRequest.java#9-17)
    - [refreshToken](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/auth/controller/AuthController.java#77-85) (String): The long-lived refresh token.
- **Response:** [AuthResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/auth/dto/response/AuthResponse.java#7-38)
    - Returns a new access token and a new refresh token.
- **Access / Security:** Publicly accessible but requires a valid, non-expired refresh token.

### Universal Logout
- **Method:** `POST`
- **Endpoint:** `/api/v1/auth/logout`
- **Purpose:** Invalidate the current session.
- **Use Case:** Securely ending a user session.
- **Used In:** Sidebar/Header logout button.
- **Response:** `ResponseStructure<String>`
- **Access / Security:** Requires active `Bearer` authentication.
- **Notes:** The JWT is added to a server-side blacklist until its natural expiry.

### Get Current User Info
- **Method:** `GET`
- **Endpoint:** `/api/v1/auth/me`
- **Purpose:** Retrieve basic details of the currently authenticated user.
- **Use Case:** Hydrating frontend state, checking session validity.
- **Used In:** Application initialization (Profile/Header).
- **Response:** JSON Object
    - `name`: Username.
    - `authorities`: List of assigned permissions/roles.
- **Access / Security:** Requires active `Bearer` authentication.

---

## Reusable / Common APIs (Used in this Module)

### Lookup / Dropdown APIs
- **No module-specific dropdown APIs found in Module 1 implementation.**
- *Note: Onboarding dropdowns (e.g., Industry Type) are part of Module 2.*

---

## Module Enums

### User Type / Role (Conceptual)
- **Used In:** [AuthResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/auth/dto/response/AuthResponse.java#7-38), JWT Claims.
- **Values:**
  - `ROOT`: Global platform admin.
  - `CEO`: Company account owner.
  - `TENANT`: Regular IAM user.

---

## Notes for Frontend

- **Authentication Headers:** All protected APIs require the `Authorization: Bearer <token>` header.
- **Multi-Tenancy:** Regular user login (`/login`) **must** include the `X-Tenant-ID` header.
- **Token Management:**
    - Access Token: Should be stored in memory or a secure cookie.
    - Refresh Token: Should be used to call `/refresh` when the access token expires (401 response).
- **Signup Flow:** After `/signup`, the user is not automatically logged in. They must verify their email and then use `/ceo/login`.
- **Validation:** Ensure `confirmPassword` matches `password` on the frontend before calling signup.
- **Forgot Password Implementation Status:** 
    - > [!WARNING]
    - > The 4-step "Forgot Password" flow (OTP-based) described in the business requirements (Section 1.6) is not yet implemented in the current backend service layer.

---
