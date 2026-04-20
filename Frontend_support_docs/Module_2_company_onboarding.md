# [Module Name: Company Onboarding]

## Short Description
The **Company Onboarding** module manages the critical transition from a new user registration to a fully verified and subscribed tenant. It encompasses company profile creation, mandatory legal document submission, administrative verification workflow, and the subscription purchase process (integrated with Razorpay). Frontend developers must handle the transition between "Pending Verification," "Rejected (Re-upload)," and "Subscribed" states.

---

## Authorization
*   **Authentication Type**: JWT (Bearer Token)
*   **Required Token**: Passed in the `Authorization` header as `Bearer <JWT_TOKEN>`.
*   **Required Roles / Authorities**:
    *   `ROLE_CEO`: Primary role for company owners.
    *   `ROLE_SERAVION`: Internal admin role for viewing all company profiles.
*   **Required Headers**:
    *   `Authorization`: `Bearer <token>`
*   **Tenant/Schema Behavior**: Backend resolves company context from the user's principal.
*   **Access Restrictions**: Most endpoints are restricted to `ROLE_CEO`.

---

## Enums Used In This Module

### IndustryType
| Value | Meaning | Used In |
|:---|:---|:---|
| `GROCERY` | Grocery or retail business | [CompanyDetailRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/company/dto/request/CompanyDetailRequest.java#7-70) |
| `PEST_CONTROL` | Pest control services | [CompanyDetailRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/company/dto/request/CompanyDetailRequest.java#7-70) |
| `CLOTHING` | Apparel and fashion retail | [CompanyDetailRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/company/dto/request/CompanyDetailRequest.java#7-70) |

### OnboardingStatus
| Value | Meaning | Used In |
|:---|:---|:---|
| `PENDING` | Documents submitted, awaiting review | [CompanyDetailResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/company/dto/response/CompanyDetailResponse.java#10-40) |
| `APPROVED` | Verified by Seravion, full access granted | [CompanyDetailResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/company/dto/response/CompanyDetailResponse.java#10-40) |
| `REJECTED` | Documents invalid, re-upload required | [CompanyDetailResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/company/dto/response/CompanyDetailResponse.java#10-40) |

### CompanyDocumentType
| Value | Meaning | Used In |
|:---|:---|:---|
| `GST_CERTIFICATE` | Goods and Services Tax certificate | [FileInfoDto](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/company/dto/request/FileInfoDto.java#11-29) |
| `PAN_CARD` | Permanent Account Number card | [FileInfoDto](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/company/dto/request/FileInfoDto.java#11-29) |
| `BUSINESS_REGISTRATION` | Trade license or incorporation doc | [FileInfoDto](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/company/dto/request/FileInfoDto.java#11-29) |

### SubscriptionPurchaseStatus
| Value | Meaning | Used In |
|:---|:---|:---|
| `PENDING_PAYMENT` | Order created, awaiting Razorpay checkout | [SubscriptionPurchaseDetailResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/subscriptionPurchase/dto/response/SubscriptionPurchaseDetailResponse.java#11-47) |
| `ACTIVE` | Payment verified, subscription live | [SubscriptionPurchaseDetailResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/subscriptionPurchase/dto/response/SubscriptionPurchaseDetailResponse.java#11-47) |
| `EXPIRED` | Subscription duration has ended | [SubscriptionPurchaseDetailResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/subscriptionPurchase/dto/response/SubscriptionPurchaseDetailResponse.java#11-47) |

---

## API List
| Method | Endpoint | Purpose | Authorization Required |
|:---|:---|:---|:---|
| `POST` | `/api/v1/company-details/submit` | Submit initial company profile fields | `CEO` |
| `GET` | `/api/v1/company-details` | Fetch own company profile and status | `CEO` |
| `POST` | `/api/v1/company/documents/upload` | Upload multiple docs (Base64) | `CEO` |
| `PUT` | `/api/v1/company/documents/re-upload` | Re-upload docs after rejection | `CEO` |
| `POST` | `/api/v1/company/subscription/calculate` | Calculate total cost | `CEO` |
| `POST` | `/api/v1/company/subscription/create-order` | Create Razorpay order | `CEO` |
| `POST` | `/api/v1/company/subscription/verify-payment` | Verify payment and activate | `CEO` |
| `POST` | `/api/v1/company/subscription/list` | List purchase history | `CEO` |
| `GET` | `/api/v1/company/subscription/detail` | View specific purchase details | `CEO` |

---

## API Details

### [POST] [/api/v1/company-details/submit]
**Purpose**: Stores company profile information during the initial onboarding step.

**Authorization**: JWT Required, `ROLE_CEO`.

**Request Body Fields**:
| Field | Type | Required | Validation | Description | Example |
|:---|:---|:---|:---|:---|:---|
| `companyName` | String | Yes | 3-150 chars | Name of the business entity | "Acme Corp" |
| `industryType` | Enum | Yes | Not Null | Sector category | "GROCERY" |
| `contactPersonName` | String | Yes | Max 120 chars | Primary contact person | "John Doe" |
| `gstNumber` | String | Yes | Valid GSTIN | Goods & Services Tax ID | "27AAAAA0000A1Z5" |

**Full Request JSON Examples**:
*Minimal Valid Request*:
```json
{
  "companyName": "Acme Solutions",
  "industryType": "PEST_CONTROL",
  "contactPersonName": "Jane Doe",
  "contactPersonEmail": "jane@acme.com",
  "contactPersonPhone": "9988776655",
  "gstNumber": "27AAAAA0000A1Z5",
  "panNumber": "ABCDE1234F",
  "addressLine1": "Tower A, IT Park",
  "city": "Noida",
  "state": "Uttar Pradesh",
  "pincode": "201301"
}
```

**Response**: `201 Created`
**Response JSON Example**:
```json
{
  "status": 201,
  "message": "Company details stored successfully",
  "data": {
    "id": 123,
    "companyCode": "ACME-PINE-456",
    "onboardingStatus": "PENDING"
  }
}
```

**Exceptions / Error Cases**:
| HTTP Status | Reason | Typical Message |
|:---|:---|:---|
| 409 | Conflict | "GST number already exists" |

**Error Response JSON Example**:
```json
{
  "status": 409,
  "message": "Company with same GST already exists",
  "error": "CONFLICT"
}
```

---

### [POST] [/api/v1/company/documents/upload]
**Purpose**: Upload mandatory legal documents as Base64.

**Authorization**: JWT Required, `ROLE_CEO`.

**Query Parameters**:
| Field | Type | Required | Description | Example |
|:---|:---|:---|:---|:---|
| `companyId` | Long | Yes | ID returned from /submit | 123 |

**Request Body Fields (Array)**:
| Field | Type | Required | Description | Example |
|:---|:---|:---|:---|:---|
| `fileName` | String | Yes | File name | "gst.pdf" |
| `contentType` | String | Yes | MIME type | "application/pdf" |
| `documentType` | Enum | Yes | Category | "GST_CERTIFICATE" |
| `fileData` | String | Yes | Base64 data | "JVBER..." |

**Full Request JSON Examples*:
*Complete Multi-Upload*:
```json
[
  {
    "fileName": "gst_cert.pdf",
    "contentType": "application/pdf",
    "documentType": "GST_CERTIFICATE",
    "fileData": "JVBERi0xLjQKJ..."
  }
]
```

**Response**: `200 OK`
**Response JSON Example**:
```json
{
  "status": 200,
  "message": "Document Uploaded Successfully !!",
  "data": "SUCCESS"
}
```

---

### [POST] [/api/v1/company/subscription/create-order]
**Purpose**: Creates a subscription record and a corresponding Razorpay order.

**Authorization**: `ROLE_CEO`.

**Request Body Fields**:
| Field | Type | Required | Description | Example |
|:---|:---|:---|:---|:---|
| `subscriptionPlanId` | String | Yes | Selected plan ID | "PLAN_PRO" |
| `branchCount` | Integer | Yes | Total branch licenses | 3 |
| `technicianCount` | Integer | Yes | Total tech licenses | 10 |
| `durationType` | Enum | Yes | "ANNUAL" or "MONTHLY" | "ANNUAL" |

**Full Request JSON Example**:
```json
{
  "subscriptionPlanId": "65e2b8f888c3a123",
  "branchCount": 2,
  "technicianCount": 5,
  "durationType": "ANNUAL",
  "autoRenew": true
}
```

**Response**: `201 Created`
**Response JSON Example**:
```json
{
  "status": 201,
  "message": "Order created. Complete payment via Razorpay checkout.",
  "data": {
    "razorpayOrderId": "order_NYxY12345ABC",
    "subscriptionId": "SUB_9901",
    "amount": 236000,
    "currency": "INR"
  }
}
```

---

### [POST] [/api/v1/company/subscription/verify-payment]
**Purpose**: Verifies Razorpay signature and finalizes subscription activation.

**Authorization**: `ROLE_CEO`.

**Request Body Fields**:
| Field | Type | Required | Description | Example |
|:---|:---|:---|:---|:---|
| `razorpayOrderId` | String | Yes | From checkout | "order_..." |
| `razorpayPaymentId` | String | Yes | From checkout | "pay_..." |
| `razorpaySignature` | String | Yes | From checkout | "abc..." |

**Full Request JSON Examples**:
*Standard Verification*:
```json
{
  "razorpayOrderId": "order_NYxY12345ABC",
  "razorpayPaymentId": "pay_NYxZ67890XYZ",
  "razorpaySignature": "d80b4d45abcdef..."
}
```

**Response JSON Example**:
```json
{
  "status": 200,
  "message": "Payment verified. Subscription activated.",
  "data": {
    "subscriptionId": "SUB_9901",
    "status": "ACTIVE",
    "endDate": "2025-03-30"
  }
}
```

---

## Validation and Exception Summary
| Scenario | Error Type | Message | Frontend Impact |
|:---|:---|:---|:---|
| Invalid GST | 400 | "Invalid GST format" | Fix UI input |
| Pending Approval | 403 / UI Block | "Verification Pending" | Block Dashboard |
| Payment Fail | 400 | "Payment verification failed" | Retry Checkout |

## Frontend Integration Summary
1. **Base64 Note**: Frontend must strip "data:xxx/yyy;base64," before sending `fileData`.
2. **Onboarding Status**: If `GET /api/v1/company-details` returns `REJECTED`, show `rejectionReason` and enable `re-upload`.
3. **Razorpay**: Call `create-order` -> Open Modal -> call `verify-payment`.

## cURL Example
```bash
curl -X POST "http://localhost:8080/api/v1/company/subscription/calculate" \
     -H "Authorization: Bearer <TOKEN>" \
     -H "Content-Type: application/json" \
     -d '{"subscriptionPlanId":"65e2b8f8", "branchCount":2, "technicianCount":5, "durationType":"ANNUAL"}'
```
