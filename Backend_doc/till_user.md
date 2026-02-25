# **PEST CONTROL ERP - BACKEND DEVELOPMENT GUIDE**

---

## **DOCUMENT STRUCTURE**

Each module contains:

- Workflow Diagram
- API Endpoints
- Request/Response Specifications
- Field Validation Rules
- Database Schema Notes

---

# **MODULE 1: AUTHENTICATION**

## **1.1 Super Admin Login (Seravion)**

### **Workflow**

```
[Super Admin enters credentials]
        ↓
[Validate against seravion_admin table]
        ↓
[Generate JWT with role: SUPER_ADMIN]
        ↓
[Redirect to Super Admin Dashboard]
```

### **API Endpoint**

| Method | Endpoint                         | Access |
| ------ | -------------------------------- | ------ |
| POST   | `/api/v1/auth/super-admin/login` | Public |

### **Request Body**

| Field    | Type   | Required | Validation  |
| -------- | ------ | -------- | ----------- |
| username | String | Yes      | Min 4 chars |
| password | String | Yes      | Min 8 chars |

```json
{
  "username": "admin@seravion.com",
  "password": "SecurePass123"
}
```

### **Response Success (200)**

```json
{
  "status": 200,
  "message": "Login successful",
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIs...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIs...",
    "expiresIn": 3600,
    "user": {
      "id": "uuid",
      "username": "admin@seravion.com",
      "role": "SUPER_ADMIN",
      "permissions": ["ALL"]
    }
  }
}
```

### **Response Error (401)**

```json
{
  "status": 401,
  "message": "Invalid credentials",
  "data": null
}
```

---

## **1.2 Company Admin Sign Up**

### **Workflow**

```
[Company enters registration details]
        ↓
[Validate email uniqueness across tenants]
        ↓
[Create tenant record (INACTIVE status)]
        ↓
[Create company_admin user]
        ↓
[Send verification email]
        ↓
[Redirect to Onboarding - Company Info]
```

### **API Endpoint**

| Method | Endpoint                      | Access |
| ------ | ----------------------------- | ------ |
| POST   | `/api/v1/auth/company/signup` | Public |

### **Request Body**

| Field                | Type   | Required | Validation                   |
| -------------------- | ------ | -------- | ---------------------------- |
| companyName          | String | Yes      | Max 200 chars                |
| authorizedPersonName | String | Yes      | Max 100 chars                |
| phone                | String | Yes      | 10 digits, Indian format     |
| email                | String | Yes      | Valid email, unique          |
| password             | String | Yes      | Min 8, 1 uppercase, 1 number |
| confirmPassword      | String | Yes      | Must match password          |

```json
{
  "companyName": "ABC Pest Control Pvt Ltd",
  "authorizedPersonName": "Rajesh Kumar",
  "phone": "9876543210",
  "email": "rajesh@abcpest.com",
  "password": "SecurePass123",
  "confirmPassword": "SecurePass123"
}
```

### **Response Success (201)**

```json
{
  "status": 201,
  "message": "Company registered successfully",
  "data": {
    "tenantId": "TENANT_2024_001234",
    "companyName": "ABC Pest Control Pvt Ltd",
    "status": "ONBOARDING_PENDING",
    "nextStep": "COMPANY_INFO"
  }
}
```

---

## **1.3 Company Admin Login**

### **API Endpoint**

| Method | Endpoint                     | Access |
| ------ | ---------------------------- | ------ |
| POST   | `/api/v1/auth/company/login` | Public |

### **Request Body**

| Field    | Type   | Required | Validation  |
| -------- | ------ | -------- | ----------- |
| email    | String | Yes      | Valid email |
| password | String | Yes      | Min 8 chars |

### **Response Success (200)**

```json
{
  "status": 200,
  "message": "Login successful",
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIs...",
    "tenantId": "TENANT_2024_001234",
    "onboardingStatus": "COMPANY_INFO_PENDING",
    "nextScreen": "/onboarding/company-info"
  }
}
```

**Note:** If onboarding incomplete, redirect to respective onboarding screen.

---

# **MODULE 2: ONBOARDING**

## **2.1 Company Information Screen**

### **Workflow**

```
[Company Admin submits company details]
        ↓
[Update tenant record with company info]
        ↓
[Mark onboarding status: DOCUMENT_UPLOAD_PENDING]
        ↓
[Redirect to Upload Document screen]
```

### **API Endpoint**

| Method | Endpoint                          | Access              |
| ------ | --------------------------------- | ------------------- |
| POST   | `/api/v1/onboarding/company-info` | Company Admin (JWT) |

### **Request Body**

| Field             | Type   | Required | Validation                                      |
| ----------------- | ------ | -------- | ----------------------------------------------- |
| companyName       | String | Yes      | Max 200 chars                                   |
| industryType      | String | Yes      | Dropdown: PEST_CONTROL, CLEANING, FACILITY_MGMT |
| contactPersonName | String | Yes      | Max 100 chars                                   |
| contactEmail      | String | Yes      | Valid email                                     |
| contactPhone      | String | Yes      | 10 digits                                       |
| gstNumber         | String | Yes      | 15 chars, GST format                            |
| panNumber         | String | Yes      | 10 chars, PAN format                            |
| addressLine1      | String | Yes      | Max 200 chars                                   |
| addressLine2      | String | No       | Max 200 chars                                   |
| city              | String | Yes      | Max 50 chars                                    |
| state             | String | Yes      | Indian state dropdown                           |
| pincode           | String | Yes      | 6 digits                                        |
| licenseNumber     | String | No       | Max 50 chars                                    |

```json
{
  "companyName": "ABC Pest Control Pvt Ltd",
  "industryType": "PEST_CONTROL",
  "contactPersonName": "Rajesh Kumar",
  "contactEmail": "rajesh@abcpest.com",
  "contactPhone": "9876543210",
  "gstNumber": "27AABCU9603R1ZX",
  "panNumber": "AABCU9603R",
  "addressLine1": "42, Industrial Estate",
  "addressLine2": "Near Sakinaka Metro",
  "city": "Mumbai",
  "state": "MAHARASHTRA",
  "pincode": "400072",
  "licenseNumber": ""
}
```

### **Response Success (200)**

```json
{
  "status": 200,
  "message": "Company information saved",
  "data": {
    "onboardingStatus": "DOCUMENT_UPLOAD_PENDING",
    "nextScreen": "/onboarding/upload-documents"
  }
}
```

---

## **2.2 Document Upload Screen**

### **Workflow**

```
[Admin uploads 3 documents]
        ↓
[Store files in S3/Azure Blob]
        ↓
[Save document URLs in tenant_documents table]
        ↓
[Update status: PENDING_VERIFICATION]
        ↓
[Show Document Success Screen]
        ↓
[Restrict access - show waiting popup]
```

### **API Endpoint**

| Method       | Endpoint                              | Access              |
| ------------ | ------------------------------------- | ------------------- |
| POST         | `/api/v1/onboarding/upload-documents` | Company Admin (JWT) |
| Content-Type | `multipart/form-data`                 |                     |

### **Request Body (Form Data)**

| Field                   | Type | Required | Validation             |
| ----------------------- | ---- | -------- | ---------------------- |
| gstDocument             | File | Yes      | PDF, JPG, PNG. Max 5MB |
| panDocument             | File | Yes      | PDF, JPG, PNG. Max 5MB |
| businessRegistrationDoc | File | Yes      | PDF, JPG, PNG. Max 5MB |

### **Response Success (200)**

```json
{
  "status": 200,
  "message": "Documents uploaded successfully",
  "data": {
    "documentStatus": "PENDING_VERIFICATION",
    "estimatedReviewTime": "24-48 hours",
    "nextScreen": "/onboarding/waiting-approval"
  }
}
```

---

## **2.3 Check Onboarding Status**

### **API Endpoint**

| Method | Endpoint                    | Access              |
| ------ | --------------------------- | ------------------- |
| GET    | `/api/v1/onboarding/status` | Company Admin (JWT) |

### **Response Scenarios**

**Scenario 1: Pending Verification**

```json
{
  "status": 200,
  "data": {
    "currentStage": "WAITING_APPROVAL",
    "message": "We will approve your access shortly",
    "canAccessDashboard": false
  }
}
```

**Scenario 2: Approved**

```json
{
  "status": 200,
  "data": {
    "currentStage": "APPROVED",
    "message": "Documents approved",
    "nextScreen": "/subscription/select-plan",
    "canAccessDashboard": false
  }
}
```

**Scenario 3: Rejected**

```json
{
  "status": 200,
  "data": {
    "currentStage": "REJECTED",
    "rejectionReason": "GST document unclear. Please re-upload.",
    "nextScreen": "/onboarding/reupload-documents",
    "canAccessDashboard": false
  }
}
```

---

## **2.4 Re-upload Documents (After Rejection)**

### **API Endpoint**

| Method       | Endpoint                                | Access              |
| ------------ | --------------------------------------- | ------------------- |
| POST         | `/api/v1/onboarding/reupload-documents` | Company Admin (JWT) |
| Content-Type | `multipart/form-data`                   |                     |

### **Request Body**

Same as 2.2, but only rejected documents required.

### **Response**

Same as 2.2, status returns to PENDING_VERIFICATION.

---

# **MODULE 3: SUPER ADMIN (SERAVION PANEL)**

## **3.1 Super Admin Sidebar Structure**

**Backend Implementation:** Role-based menu generation

```json
{
  "menuItems": [
    {
      "id": "dashboard",
      "label": "Dashboard",
      "icon": "dashboard",
      "route": "/super-admin/dashboard"
    },
    {
      "id": "subscription-plans",
      "label": "Subscription Plans",
      "icon": "plans",
      "route": "/super-admin/plans"
    },
    {
      "id": "reports",
      "label": "Reports",
      "icon": "reports",
      "route": "/super-admin/reports"
    },
    {
      "id": "role-management",
      "label": "Role Management",
      "icon": "roles",
      "route": "/super-admin/roles"
    }
  ]
}
```

---

## **3.2 Dashboard Screen - Customer List**

### **API Endpoint**

| Method | Endpoint                        | Access      |
| ------ | ------------------------------- | ----------- |
| GET    | `/api/v1/super-admin/customers` | Super Admin |

### **Query Parameters**

| Param     | Type    | Required | Description                        |
| --------- | ------- | -------- | ---------------------------------- |
| page      | Integer | No       | Default: 1                         |
| size      | Integer | No       | Default: 20                        |
| docStatus | String  | No       | PENDING, APPROVED, REJECTED        |
| payStatus | String  | No       | PAID, PARTIAL_PAID, FREE, NON_PAID |
| search    | String  | No       | Company name or email              |

### **Response Success (200)**

```json
{
  "status": 200,
  "data": {
    "totalRecords": 150,
    "page": 1,
    "size": 20,
    "customers": [
      {
        "tenantId": "TENANT_2024_001234",
        "companyName": "ABC Pest Control",
        "contactPerson": "Rajesh Kumar",
        "email": "rajesh@abcpest.com",
        "docStatus": "APPROVED",
        "payStatus": "PAID",
        "subscriptionPlan": "GROWTH",
        "createdAt": "2024-01-15T10:30:00"
      }
    ]
  }
}
```

---

## **3.3 Customer Detail Popup**

### **API Endpoint**

| Method | Endpoint                                   | Access      |
| ------ | ------------------------------------------ | ----------- |
| GET    | `/api/v1/super-admin/customers/{tenantId}` | Super Admin |

### **Response - Tab 1: Company Info**

```json
{
  "status": 200,
  "data": {
    "tenantId": "TENANT_2024_001234",
    "companyName": "ABC Pest Control Pvt Ltd",
    "contactDetails": {
      "name": "Rajesh Kumar",
      "email": "rajesh@abcpest.com",
      "phone": "9876543210"
    },
    "gstNumber": "27AABCU9603R1ZX",
    "panNumber": "AABCU9603R",
    "address": {
      "line1": "42, Industrial Estate",
      "city": "Mumbai",
      "state": "MAHARASHTRA",
      "pincode": "400072"
    },
    "docStatus": "APPROVED",
    "payStatus": "PAID",
    "trialEnabled": true,
    "trialDates": {
      "from": "2024-01-15",
      "to": "2024-01-30"
    }
  }
}
```

### **Update Customer Status (POST)**

| Method | Endpoint                                          | Access      |
| ------ | ------------------------------------------------- | ----------- |
| POST   | `/api/v1/super-admin/customers/{tenantId}/status` | Super Admin |

### **Request Body**

| Field           | Type    | Required    | Description                    |
| --------------- | ------- | ----------- | ------------------------------ |
| docStatus       | String  | Yes         | APPROVED, PENDING, REJECTED    |
| rejectionReason | String  | Conditional | Required if docStatus=REJECTED |
| trialEnabled    | Boolean | No          | true/false                     |
| trialFrom       | Date    | Conditional | Required if trialEnabled=true  |
| trialTo         | Date    | Conditional | Required if trialEnabled=true  |

```json
{
  "docStatus": "APPROVED",
  "trialEnabled": true,
  "trialFrom": "2024-01-15",
  "trialTo": "2024-01-30"
}
```

---

## **3.4 Customer Documents & Subscription Tab**

### **API Endpoint**

| Method | Endpoint                                             | Access      |
| ------ | ---------------------------------------------------- | ----------- |
| GET    | `/api/v1/super-admin/customers/{tenantId}/documents` | Super Admin |

### **Response**

```json
{
  "status": 200,
  "data": {
    "documents": [
      {
        "type": "GST",
        "fileUrl": "https://s3.../gst_doc.pdf",
        "uploadedAt": "2024-01-15T10:30:00",
        "status": "VERIFIED"
      }
    ],
    "subscriptionHistory": [
      {
        "subscriptionId": "SUB_001",
        "planName": "GROWTH",
        "duration": "YEARLY",
        "branchCount": 3,
        "technicianCount": 30,
        "amountPaid": 155892,
        "paymentStatus": "PAID",
        "startDate": "2024-01-15",
        "endDate": "2025-01-14"
      }
    ]
  }
}
```

---

## **3.5 Subscription Plan Management**

### **3.5.1 Create Plan**

| Method | Endpoint                    | Access      |
| ------ | --------------------------- | ----------- |
| POST   | `/api/v1/super-admin/plans` | Super Admin |

### **Request Body**

| Field                                | Type    | Required | Description                |
| ------------------------------------ | ------- | -------- | -------------------------- |
| planName                             | String  | Yes      | Max 50 chars               |
| description                          | String  | Yes      | Max 500 chars              |
| branchPricing                        | Object  | Yes      | Pricing structure          |
| branchPricing.baseCount              | Integer | Yes      | Included branches          |
| branchPricing.pricePerBranch         | Decimal | Yes      | ₹ amount                   |
| technicianPricing                    | Object  | Yes      | Pricing structure          |
| technicianPricing.baseCount          | Integer | Yes      | Included technicians       |
| technicianPricing.pricePerTechnician | Decimal | Yes      | ₹ amount                   |
| durationOptions                      | Array   | Yes      | MONTHLY, QUARTERLY, YEARLY |
| isActive                             | Boolean | Yes      | true/false                 |

```json
{
  "planName": "GROWTH",
  "description": "Ideal for small multi-location operators",
  "branchPricing": {
    "baseCount": 3,
    "pricePerBranch": 2999
  },
  "technicianPricing": {
    "baseCount": 30,
    "pricePerTechnician": 199
  },
  "durationOptions": ["MONTHLY", "QUARTERLY", "YEARLY"],
  "isActive": true
}
```

### **3.5.2 List Plans**

| Method | Endpoint                    | Access      |
| ------ | --------------------------- | ----------- |
| GET    | `/api/v1/super-admin/plans` | Super Admin |

### **3.5.3 Update Plan**

| Method | Endpoint                             | Access      |
| ------ | ------------------------------------ | ----------- |
| PUT    | `/api/v1/super-admin/plans/{planId}` | Super Admin |

### **3.5.4 Delete Plan**

| Method | Endpoint                             | Access      |
| ------ | ------------------------------------ | ----------- |
| DELETE | `/api/v1/super-admin/plans/{planId}` | Super Admin |

**Rule:** Soft delete if any active subscriptions exist.

---

## **3.6 Role Management (Super Admin)**

### **3.6.1 Create Base Role**

| Method | Endpoint                    | Access      |
| ------ | --------------------------- | ----------- |
| POST   | `/api/v1/super-admin/roles` | Super Admin |

### **Request Body**

| Field       | Type   | Required | Description             |
| ----------- | ------ | -------- | ----------------------- |
| roleName    | String | Yes      | Max 100 chars           |
| description | String | No       | Max 500 chars           |
| permissions | Object | Yes      | Module-wise permissions |

```json
{
  "roleName": "Standard Branch Manager",
  "description": "Manages branch operations with financial view",
  "permissions": {
    "DASHBOARD": {
      "view": true,
      "add": false,
      "edit": false,
      "delete": false,
      "export": true,
      "configure": false,
      "approve": false
    },
    "BRANCH_MGMT": {
      "view": true,
      "add": true,
      "edit": true,
      "delete": false,
      "export": true,
      "configure": false,
      "approve": false
    },
    "INVOICING": {
      "view": true,
      "add": true,
      "edit": true,
      "delete": false,
      "export": true,
      "configure": false,
      "approve": true
    }
  }
}
```

### **3.6.2 List Base Roles**

| Method | Endpoint                    | Access      |
| ------ | --------------------------- | ----------- |
| GET    | `/api/v1/super-admin/roles` | Super Admin |

### **3.6.3 Update Base Role**

| Method | Endpoint                             | Access      |
| ------ | ------------------------------------ | ----------- |
| PUT    | `/api/v1/super-admin/roles/{roleId}` | Super Admin |

**Warning:** Changes affect all tenants using this base role.

### **3.6.4 Delete Base Role**

| Method | Endpoint                             | Access      |
| ------ | ------------------------------------ | ----------- |
| DELETE | `/api/v1/super-admin/roles/{roleId}` | Super Admin |

**Validation:** Check if any tenant has active users with this role.

---

# **MODULE 4: SUBSCRIPTION (CLIENT SIDE)**

## **4.1 Select Subscription Plan**

### **Workflow**

```
[Company Admin views available plans]
        ↓
[System fetches plans from super_admin with pricing]
        ↓
[Admin selects plan, duration, calculates total]
        ↓
[Payment gateway integration]
        ↓
[On success: Activate subscription, send invoice]
```

### **API Endpoint**

| Method | Endpoint                     | Access        |
| ------ | ---------------------------- | ------------- |
| GET    | `/api/v1/subscription/plans` | Company Admin |

### **Response**

```json
{
  "status": 200,
  "data": {
    "availablePlans": [
      {
        "planId": "PLAN_GROWTH",
        "planName": "GROWTH",
        "description": "3 branches, 30 users",
        "basePrice": {
          "monthly": 12999,
          "quarterly": 37047,
          "yearly": 132588
        },
        "additionalPricing": {
          "perBranch": 2999,
          "perTechnician": 199
        }
      }
    ]
  }
}
```

### **Calculate Price (POST)**

| Method | Endpoint                         | Access        |
| ------ | -------------------------------- | ------------- |
| POST   | `/api/v1/subscription/calculate` | Company Admin |

### **Request Body**

| Field                 | Type    | Required |
| --------------------- | ------- | -------- | -------------------------- |
| planId                | String  | Yes      |
| duration              | String  | Yes      | MONTHLY, QUARTERLY, YEARLY |
| additionalBranches    | Integer | No       | Default: 0                 |
| additionalTechnicians | Integer | No       | Default: 0                 |

### **Response**

```json
{
  "status": 200,
  "data": {
    "baseAmount": 12999,
    "additionalBranchAmount": 0,
    "additionalTechnicianAmount": 0,
    "subtotal": 12999,
    "gstAmount": 2339.82,
    "totalAmount": 15338.82,
    "currency": "INR"
  }
}
```

### **Purchase Subscription (POST)**

| Method | Endpoint                        | Access        |
| ------ | ------------------------------- | ------------- |
| POST   | `/api/v1/subscription/purchase` | Company Admin |

### **Request Body**

| Field                 | Type    | Required |
| --------------------- | ------- | -------- | -------------------- |
| planId                | String  | Yes      |
| duration              | String  | Yes      |
| additionalBranches    | Integer | No       |
| additionalTechnicians | Integer | No       |
| paymentMethod         | String  | Yes      | RAZORPAY, etc.       |
| paymentId             | String  | Yes      | From payment gateway |

---

## **4.2 Subscription Module (Sidebar)**

### **4.2.1 List Subscriptions**

| Method | Endpoint                       | Access        |
| ------ | ------------------------------ | ------------- |
| GET    | `/api/v1/subscription/history` | Company Admin |

### **Response**

```json
{
  "status": 200,
  "data": {
    "currentSubscription": {
      "subscriptionId": "SUB_001",
      "planName": "GROWTH",
      "status": "ACTIVE",
      "startDate": "2024-01-15",
      "endDate": "2025-01-14",
      "paymentStatus": "PAID"
    },
    "history": [
      {
        "subscriptionId": "SUB_001",
        "planName": "STARTER",
        "status": "EXPIRED",
        "startDate": "2023-01-15",
        "endDate": "2024-01-14"
      }
    ]
  }
}
```

### **4.2.2 View Subscription Details**

| Method | Endpoint                                | Access        |
| ------ | --------------------------------------- | ------------- |
| GET    | `/api/v1/subscription/{subscriptionId}` | Company Admin |

---

# **MODULE 5: ROLE MANAGEMENT (CLIENT SIDE)**

## **5.1 Create Custom Role**

### **Workflow**

```
[Company Admin selects base role from Seravion list]
        ↓
[Optionally clones from existing custom role]
        ↓
[Modifies permissions per module]
        ↓
[System validates dependencies]
        ↓
[Saves role for tenant]
```

### **API Endpoint**

| Method | Endpoint        | Access        |
| ------ | --------------- | ------------- |
| POST   | `/api/v1/roles` | Company Admin |

### **Request Body**

| Field            | Type   | Required    | Description                      |
| ---------------- | ------ | ----------- | -------------------------------- |
| baseRoleId       | String | Yes         | From super admin base roles      |
| cloneFromRoleId  | String | No          | Existing custom role to clone    |
| roleName         | String | Yes         | Unique within tenant             |
| description      | String | No          | Max 500 chars                    |
| permissions      | Object | Yes         | 25+ modules                      |
| branchScope      | String | Yes         | ALL or SPECIFIC                  |
| allowedBranchIds | Array  | Conditional | Required if branchScope=SPECIFIC |

```json
{
  "baseRoleId": "ROLE_BRANCH_MANAGER",
  "cloneFromRoleId": null,
  "roleName": "Senior Branch Manager",
  "description": "Can approve expenses up to 5000",
  "permissions": {
    "DASHBOARD": {
      "view": true,
      "add": false,
      "edit": false,
      "delete": false,
      "export": true,
      "configure": false,
      "approve": false
    },
    "BRANCH_MGMT": {
      "view": true,
      "add": true,
      "edit": true,
      "delete": false,
      "export": true,
      "configure": false,
      "approve": false
    },
    "EXPENSE_MGMT": {
      "view": true,
      "add": true,
      "edit": true,
      "delete": false,
      "export": true,
      "configure": false,
      "approve": true
    }
  },
  "branchScope": "SPECIFIC",
  "allowedBranchIds": ["branch-uuid-1", "branch-uuid-2"]
}
```

**Dependency Validation:** If EXPENSE_MGMT has edit=true, auto-enable view=true for same module.

---

## **5.2 List Custom Roles**

| Method | Endpoint        | Access        |
| ------ | --------------- | ------------- |
| GET    | `/api/v1/roles` | Company Admin |

---

## **5.3 View Role Details**

| Method | Endpoint                 | Access        |
| ------ | ------------------------ | ------------- |
| GET    | `/api/v1/roles/{roleId}` | Company Admin |

---

## **5.4 Update Role**

| Method | Endpoint                 | Access        |
| ------ | ------------------------ | ------------- |
| PUT    | `/api/v1/roles/{roleId}` | Company Admin |

**Warning Check:** System returns affected user count before confirm.

```json
{
  "warning": "This role is assigned to 5 users. Changes will apply immediately.",
  "affectedUsers": 5,
  "confirmUpdate": true
}
```

---

## **5.5 Delete Role with Migration**

| Method | Endpoint                 | Access        |
| ------ | ------------------------ | ------------- |
| DELETE | `/api/v1/roles/{roleId}` | Company Admin |

### **Request Body**

| Field           | Type   | Required    |
| --------------- | ------ | ----------- | ----------------------- |
| migrateToRoleId | String | Conditional | Required if users exist |

**Validation:**

- If users assigned: Must provide migrateToRoleId
- Cannot delete if no alternative role specified

---

## **5.6 Role Salary & Leave Configuration**

### **5.6.1 Create Role Configuration**

| Method | Endpoint                               | Access        |
| ------ | -------------------------------------- | ------------- |
| POST   | `/api/v1/roles/{roleId}/configuration` | Company Admin |

### **Request Body - Salary Section**

| Field                          | Type    | Required    |
| ------------------------------ | ------- | ----------- | ------------------------ |
| effectiveFrom                  | Date    | Yes         |
| effectiveTo                    | Date    | No          |
| status                         | String  | Yes         | ACTIVE, INACTIVE         |
| salaryType                     | String  | Yes         | CTC, FIXED, HOURLY       |
| defaultBasicSalary             | Decimal | Yes         |
| defaultHra                     | Decimal | No          |
| defaultOtherAllowance          | Decimal | No          |
| defaultIncentive               | Decimal | No          |
| defaultDeductions              | Decimal | No          |
| pfApplicable                   | Boolean | No          |
| esiApplicable                  | Boolean | No          |
| tdsApplicable                  | Boolean | No          |
| holidayWorkIncentiveApplicable | Boolean | No          |
| holidayWorkIncentiveType       | String  | Conditional | FIXED, PER_DAY, PER_HOUR |
| holidayWorkIncentiveAmount     | Decimal | Conditional |
| overtimeApplicable             | Boolean | No          |
| overtimeType                   | String  | Conditional | PER_HOUR, PER_SHIFT      |
| overtimeShiftType              | String  | Conditional |
| overtimeShiftIncentiveAmount   | Decimal | Conditional |
| perHourIncentiveAmount         | Decimal | Conditional |
| maxOvertimeHoursPerMonth       | Integer | Conditional |

### **Request Body - Leave Section**

| Field                        | Type    | Required    |
| ---------------------------- | ------- | ----------- | --------------- |
| casualLeavePerYear           | Integer | No          |
| sickLeavePerYear             | Integer | No          |
| paidLeavePerYear             | Integer | No          |
| annualLeaveAllocation        | Integer | No          |
| carryForwardAllowed          | Boolean | No          |
| maxCarryForwardDays          | Integer | Conditional |
| leaveApprovalAuthorityRoleId | String  | Yes         |
| leaveResetCycle              | String  | Yes         | YEARLY, MONTHLY |

---

# **MODULE 6: BRANCH MANAGEMENT**

## **6.1 Create Branch**

### **API Endpoint**

| Method | Endpoint           | Access                         |
| ------ | ------------------ | ------------------------------ |
| POST   | `/api/v1/branches` | Company Admin, Operations Head |

### **Request Body**

| Field           | Type   | Required | Validation                                        |
| --------------- | ------ | -------- | ------------------------------------------------- |
| branchName      | String | Yes      | Max 100 chars                                     |
| branchCode      | String | Yes      | Exactly 3 uppercase letters, unique               |
| branchType      | String | Yes      | HEAD_OFFICE, STATE_BRANCH, CITY_BRANCH, WAREHOUSE |
| addressLine1    | String | Yes      | Max 200 chars                                     |
| addressLine2    | String | No       | Max 200 chars                                     |
| city            | String | Yes      | Max 50 chars                                      |
| state           | String | Yes      | Indian state                                      |
| pincode         | String | Yes      | 6 digits                                          |
| phone           | String | Yes      | 10 digits                                         |
| email           | String | Yes      | Valid email                                       |
| branchManagerId | String | No       | Valid employee ID                                 |

```json
{
  "branchName": "Andheri West Service Center",
  "branchCode": "AND",
  "branchType": "CITY_BRANCH",
  "addressLine1": "42, Industrial Estate",
  "addressLine2": "Phase 2, Building 7",
  "city": "Mumbai",
  "state": "MAHARASHTRA",
  "pincode": "400072",
  "phone": "9876543210",
  "email": "andheri@company.com",
  "branchManagerId": "EMP001"
}
```

**Auto-populated:** createdBy (current user), createdAt (timestamp)

---

## **6.2 List Branches**

| Method | Endpoint           | Access                                       |
| ------ | ------------------ | -------------------------------------------- |
| GET    | `/api/v1/branches` | Authenticated User (filtered by permissions) |

### **Query Parameters**

| Param  | Type   | Description      |
| ------ | ------ | ---------------- |
| status | String | ACTIVE, INACTIVE |
| search | String | Name or code     |

### **Response**

```json
{
  "status": 200,
  "data": [
    {
      "branchId": "uuid",
      "branchCode": "AND",
      "branchName": "Andheri West Service Center",
      "city": "Mumbai",
      "state": "MAHARASHTRA",
      "employeeCount": 15,
      "status": "ACTIVE"
    }
  ]
}
```

**Note:** Type column removed from response. City and State shown explicitly.

---

## **6.3 Update Branch**

| Method | Endpoint                      | Access                         |
| ------ | ----------------------------- | ------------------------------ |
| PUT    | `/api/v1/branches/{branchId}` | Company Admin, Operations Head |

**Same fields as Create, except:**

- editedBy: Auto-populated current user
- editedAt: Auto-populated timestamp

---

## **6.4 View Branch Details**

| Method | Endpoint                      | Access             |
| ------ | ----------------------------- | ------------------ |
| GET    | `/api/v1/branches/{branchId}` | Authenticated User |

### **Response - Tab 1: Branch Info**

```json
{
  "status": 200,
  "data": {
    "branchId": "uuid",
    "branchCode": "AND",
    "branchName": "Andheri West Service Center",
    "branchType": "CITY_BRANCH",
    "address": {
      "line1": "42, Industrial Estate",
      "line2": "Phase 2, Building 7",
      "city": "Mumbai",
      "state": "MAHARASHTRA",
      "pincode": "400072"
    },
    "contact": {
      "phone": "9876543210",
      "email": "andheri@company.com"
    },
    "branchManager": {
      "employeeId": "EMP001",
      "name": "Amit Sharma"
    },
    "status": "ACTIVE",
    "createdBy": "Admin Raj",
    "createdAt": "2023-12-01T10:00:00",
    "editedBy": "Ops Head Priya",
    "editedAt": "2024-01-15T14:30:00"
  }
}
```

### **Response - Tab 2: Employees (Separate API)**

| Method | Endpoint                                | Access             |
| ------ | --------------------------------------- | ------------------ |
| GET    | `/api/v1/branches/{branchId}/employees` | Authenticated User |

### **Response**

```json
{
  "status": 200,
  "data": [
    {
      "employeeId": "EMP001",
      "employeeName": "Amit Sharma",
      "email": "amit@company.com",
      "contactNumber": "9876543211",
      "designation": "Branch Manager",
      "staffStatus": "ACTIVE",
      "branchName": "Andheri West Service Center"
    }
  ]
}
```

---

## **6.5 Delete Branch (Soft Delete)**

| Method | Endpoint                      | Access        |
| ------ | ----------------------------- | ------------- |
| DELETE | `/api/v1/branches/{branchId}` | Company Admin |

**Action:** Sets `status = INACTIVE`, does not hard delete.

---

# **MODULE 7: USER MANAGEMENT**

## **7.1 Tab Visibility Logic**

**Backend determines visible tabs based on role:**

```java
public List<String> getVisibleTabs(User user) {
    Role role = user.getPrimaryRole();

    if (role.isUpperLevel()) { // Admin, HR, Company Admin
        return Arrays.asList("USER_LIST", "SEND_REQUEST", "RECEIVED_REQUESTS");
    } else { // Manager, Team Lead
        return Arrays.asList("USER_LIST", "SEND_REQUEST");
    }
}
```

---

## **7.2 Tab 1: User List**

### **API Endpoint**

| Method | Endpoint        | Access                                         |
| ------ | --------------- | ---------------------------------------------- |
| GET    | `/api/v1/users` | Authenticated (filtered by branch permissions) |

### **Query Parameters**

| Param    | Type   | Description                |
| -------- | ------ | -------------------------- |
| branchId | String | Filter by branch           |
| status   | String | ACTIVE, INACTIVE, ON_LEAVE |
| roleId   | String | Filter by role             |
| search   | String | Name, email, or EMP ID     |

### **Response**

```json
{
  "status": 200,
  "data": {
    "totalRecords": 45,
    "users": [
      {
        "employeeId": "EMP001",
        "employeeName": "Amit Sharma",
        "email": "amit@company.com",
        "contactNumber": "9876543210",
        "designation": "Branch Manager",
        "department": "Operations",
        "role": "Branch Manager",
        "branchName": "Andheri, Bandra",
        "reportingManager": "Rajesh Kumar",
        "status": "ACTIVE",
        "createdDate": "2023-12-01"
      }
    ]
  }
}
```

---

## **7.3 Tab 2: Send Request (Hiring Request)**

### **7.3.1 Submit Hiring Request**

| Method | Endpoint                  | Access                           |
| ------ | ------------------------- | -------------------------------- |
| POST   | `/api/v1/hiring-requests` | Lower-Level Manager, Upper-Level |

### **Request Body**

| Field                 | Type    | Required |
| --------------------- | ------- | -------- | --------------------------- |
| department            | String  | Yes      |
| designation           | String  | Yes      |
| proposedRoleId        | String  | Yes      |
| branchIds             | Array   | Yes      |
| employmentType        | String  | Yes      | PERMANENT, CONTRACT, INTERN |
| expectedDateOfJoining | Date    | Yes      |
| numberOfPositions     | Integer | Yes      |
| hiringReason          | String  | Yes      |
| jobDescription        | String  | No       |
| additionalRemarks     | String  | No       |
| supportingDocument    | File    | No       |

**Auto-populated:** requestedBy (current user), requestedAt (timestamp)

---

### **7.3.2 My Hiring Requests (List)**

| Method | Endpoint                     | Access                           |
| ------ | ---------------------------- | -------------------------------- |
| GET    | `/api/v1/hiring-requests/my` | Lower-Level Manager, Upper-Level |

### **Response**

```json
{
  "status": 200,
  "data": [
    {
      "requestId": "REQ_001",
      "department": "Operations",
      "proposedRole": "Technician",
      "branches": ["Andheri", "Bandra"],
      "numberOfPositions": 2,
      "expectedJoiningDate": "2024-02-01",
      "hiringReason": "Business expansion",
      "status": "PENDING",
      "submittedDate": "2024-01-15",
      "lastUpdatedDate": "2024-01-15"
    }
  ]
}
```

---

## **7.4 Tab 3: Received Requests (Admin Only)**

### **7.4.1 List Received Requests**

| Method | Endpoint                           | Access                 |
| ------ | ---------------------------------- | ---------------------- |
| GET    | `/api/v1/hiring-requests/received` | Upper-Level, Admin, HR |

### **Query Parameters**

| Param      | Type   | Description                 |
| ---------- | ------ | --------------------------- |
| status     | String | PENDING, APPROVED, REJECTED |
| department | String | Filter                      |
| branchId   | String | Filter                      |
| fromDate   | Date   | Date range                  |
| toDate     | Date   | Date range                  |

---

### **7.4.2 Approve/Reject Request**

| Method | Endpoint                                     | Access                 |
| ------ | -------------------------------------------- | ---------------------- |
| POST   | `/api/v1/hiring-requests/{requestId}/action` | Upper-Level, Admin, HR |

### **Request Body**

| Field               | Type   | Required    |
| ------------------- | ------ | ----------- | ------------------ |
| action              | String | Yes         | APPROVE, REJECT    |
| rejectionReason     | String | Conditional | Required if REJECT |
| approvedSalaryRange | Object | No          | If APPROVE         |

```json
{
  "action": "APPROVE",
  "approvedSalaryRange": {
    "min": 25000,
    "max": 35000
  }
}
```

**OR**

```json
{
  "action": "REJECT",
  "rejectionReason": "Budget constraints for Q1"
}
```

---

## **7.5 Add User (After Hiring Approval)**

### **Multi-Step Form API Structure**

### **Step 1: Personal Details**

| Method | Endpoint        | Access                 |
| ------ | --------------- | ---------------------- |
| POST   | `/api/v1/users` | Upper-Level, Admin, HR |

### **Request Body - Step 1**

| Field              | Type   | Required |
| ------------------ | ------ | -------- | ----------------------- |
| employeeId         | String | Yes      | Unique                  |
| firstName          | String | Yes      |
| lastName           | String | Yes      |
| email              | String | No       | Valid email             |
| contactNumber      | String | Yes      | 10 digits               |
| alternateNumber    | String | No       | 10 digits               |
| password           | String | Yes      | Auto-generated if empty |
| department         | String | Yes      |
| designation        | String | Yes      |
| roleId             | String | Yes      |
| branchIds          | Array  | Yes      |
| reportingManagerId | String | Yes      |
| employmentType     | String | Yes      |
| dateOfJoining      | Date   | Yes      |
| status             | String | Yes      | ACTIVE, INACTIVE        |
| currentAddress     | Object | Yes      |
| permanentAddress   | Object | Yes      |

```json
{
  "employeeId": "EMP005",
  "firstName": "Rahul",
  "lastName": "Kumar",
  "email": "rahul@company.com",
  "contactNumber": "9876543212",
  "password": "TempPass123",
  "department": "Operations",
  "designation": "Senior Technician",
  "roleId": "role-uuid-senior-tech",
  "branchIds": ["branch-uuid-1"],
  "reportingManagerId": "EMP001",
  "employmentType": "PERMANENT",
  "dateOfJoining": "2024-02-01",
  "status": "ACTIVE",
  "currentAddress": {
    "line1": "123, Staff Quarters",
    "city": "Mumbai",
    "state": "MAHARASHTRA",
    "country": "India",
    "pincode": "400072"
  },
  "permanentAddress": {
    "line1": "456, Home Town",
    "city": "Pune",
    "state": "MAHARASHTRA",
    "country": "India",
    "pincode": "411001"
  }
}
```

---

### **Step 2: Module Permissions**

| Method | Endpoint                                 | Access                 |
| ------ | ---------------------------------------- | ---------------------- |
| PUT    | `/api/v1/users/{employeeId}/permissions` | Upper-Level, Admin, HR |

### **Request Body**

| Field             | Type    | Required    |
| ----------------- | ------- | ----------- | -------------------------- |
| isApplicationUser | Boolean | Yes         |
| permissions       | Object  | Conditional | If isApplicationUser=false |

```json
{
  "isApplicationUser": false,
  "permissions": {
    "TASK_MGMT": {
      "view": true,
      "add": true,
      "edit": true,
      "delete": false,
      "export": false,
      "configure": false,
      "approve": false
    },
    "INVENTORY": {
      "view": true,
      "add": true,
      "edit": false,
      "delete": false,
      "export": false,
      "configure": false,
      "approve": false
    }
  }
}
```

**Note:** If `isApplicationUser=true`, permissions section is disabled/ignored.

---

### **Step 3: Salary Details**

| Method | Endpoint                            | Access                 |
| ------ | ----------------------------------- | ---------------------- |
| PUT    | `/api/v1/users/{employeeId}/salary` | Upper-Level, Admin, HR |

### **Request Body**

| Field                          | Type    | Required    | Source                      |
| ------------------------------ | ------- | ----------- | --------------------------- |
| salaryType                     | String  | Yes         | Auto-fetch from role config |
| basicSalary                    | Decimal | Yes         | Auto-fetch, editable        |
| hra                            | Decimal | No          | Auto-fetch, editable        |
| otherAllowance                 | Decimal | No          | Auto-fetch, editable        |
| incentive                      | Decimal | No          | Auto-fetch, editable        |
| deductions                     | Decimal | No          | Auto-fetch, editable        |
| pfApplicable                   | Boolean | No          | Auto-fetch, editable        |
| esiApplicable                  | Boolean | No          | Auto-fetch, editable        |
| tdsApplicable                  | Boolean | No          | Auto-fetch, editable        |
| bankName                       | String  | Yes         |
| accountNumber                  | String  | Yes         |
| ifscCode                       | String  | Yes         |
| salaryEffectiveFrom            | Date    | Yes         |
| salaryEffectiveTo              | Date    | No          |
| holidayWorkIncentiveApplicable | Boolean | No          | Auto-fetch                  |
| holidayWorkIncentiveType       | String  | Conditional | Auto-fetch                  |
| holidayWorkIncentiveAmount     | Decimal | Conditional | Auto-fetch                  |
| overtimeApplicable             | Boolean | No          | Auto-fetch                  |
| overtimeType                   | String  | Conditional | Auto-fetch                  |
| overtimeShiftType              | String  | Conditional | Auto-fetch                  |
| overtimeShiftIncentiveAmount   | Decimal | Conditional | Auto-fetch                  |
| perHourIncentiveAmount         | Decimal | Conditional | Auto-fetch                  |
| maxOvertimeHoursPerMonth       | Integer | Conditional | Auto-fetch                  |

---

### **Step 4: Leave Details**

| Method | Endpoint                                  | Access                 |
| ------ | ----------------------------------------- | ---------------------- |
| PUT    | `/api/v1/users/{employeeId}/leave-config` | Upper-Level, Admin, HR |

### **Request Body**

| Field                        | Type    | Required    | Source               |
| ---------------------------- | ------- | ----------- | -------------------- |
| casualLeave                  | Integer | No          | Auto-fetch from role |
| sickLeave                    | Integer | No          | Auto-fetch from role |
| paidLeave                    | Integer | No          | Auto-fetch from role |
| annualLeaveAllocation        | Integer | No          | Auto-fetch from role |
| carryForwardAllowed          | Boolean | No          | Auto-fetch           |
| maxCarryForwardDays          | Integer | Conditional | Auto-fetch           |
| leaveApprovalAuthorityRoleId | String  | Yes         | Auto-fetch           |
| leaveResetCycle              | String  | Yes         | Auto-fetch           |

---

### **Step 5: Documents & Additional Data**

| Method       | Endpoint                               | Access                 |
| ------------ | -------------------------------------- | ---------------------- |
| PUT          | `/api/v1/users/{employeeId}/documents` | Upper-Level, Admin, HR |
| Content-Type | `multipart/form-data`                  |                        |

### **Request Body**

| Field                   | Type    | Required |
| ----------------------- | ------- | -------- | --------- |
| aadharNumber            | String  | No       | 12 digits |
| panNumber               | String  | No       | 10 chars  |
| uanNumber               | String  | No       |
| employeeIdCardNumber    | String  | No       |
| aadharDocument          | File    | No       |
| panDocument             | File    | No       |
| addressProof            | File    | No       |
| educationalCertificates | File    | No       |
| experienceLetter        | File    | No       |
| offerLetter             | File    | No       |
| appointmentLetter       | File    | No       |
| otherDocuments          | File    | No       |
| grade                   | String  | No       |
| shiftType               | String  | No       |
| weeklyOff               | String  | No       |
| targetAmount            | Decimal | No       |
| commissionPercentage    | Decimal | No       |
| employeePhoto           | File    | No       |

---

## **7.6 Edit User**

### **API Endpoint**

| Method | Endpoint                     | Access                 |
| ------ | ---------------------------- | ---------------------- |
| PUT    | `/api/v1/users/{employeeId}` | Upper-Level, Admin, HR |

**Same fields and structure as Add User (all 5 steps).**

**Difference:** Pre-populate all existing data, allow modification.

---

## **DATABASE RELATIONSHIP SUMMARY**

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  SUPER_ADMIN    │     │    TENANTS      │     │ SUBSCRIPTION_   │
│  (Seravion)     │────▶│  (Companies)    │◄────│ PLANS           │
└─────────────────┘     └────────┬────────┘     └─────────────────┘
                                 │
                    ┌────────────┼────────────┐
                    ▼            ▼            ▼
            ┌───────────┐  ┌───────────┐  ┌───────────┐
            │  BRANCHES │  │   ROLES   │  │   USERS   │
            │           │  │(Custom +  │  │           │
            │           │  │ Base from │  │           │
            │           │  │ Seravion) │  │           │
            └───────────┘  └─────┬─────┘  └─────┬─────┘
                                 │              │
                                 └──────────────┘
                                        │
                              ┌─────────┴─────────┐
                              ▼                   ▼
                        ┌───────────┐       ┌───────────┐
                        │ HIRING_   │       │  USER_    │
                        │ REQUESTS  │       │  ROLE_    │
                        │           │       │  MAPPING  │
                        └───────────┘       └───────────┘
```

---
