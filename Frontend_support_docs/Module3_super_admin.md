# Module 3: Super Admin (Company Management)

## Short Description

The Company Management module is designed for **Super Admins** (Seravion role) to oversee and manage all registered companies on the platform. It provides functionalities to list all companies, view their detailed profiles and uploaded documents, and process their onboarding through an approval or rejection workflow. This module is critical for the initial vetting and activation of new tenants.

## Authorization

- **Authentication Type:** Bearer Token (JWT)
- **Required Token:** X-Auth-Token (or standard Authorization header as per global config)
- **Required Roles / Authorities:** `ROLE_SERAVION`
- **Required Headers:**
  - `Authorization: Bearer <token>`
- **Tenant/Schema Behavior:** This is a global module; Super Admins operate across all schemas. No specific tenant header is typically required for these endpoints as they access the global registry.
- **Access Restrictions:** Restricted exclusively to platform owners (Seravion staff).

## Enums Used In This Module

### OnboardingStatus

| Value      | Meaning                                                 | Used In                        |
| :--------- | :------------------------------------------------------ | :----------------------------- |
| `PENDING`  | Company has submitted details and is waiting for review | List, Detail, Approval Request |
| `APPROVED` | Company has been vetted and activated                   | List, Detail, Approval Request |
| `REJECTED` | Company application has been declined                   | List, Detail, Approval Request |

### IndustryType

| Value          | Meaning                                  | Used In         |
| :------------- | :--------------------------------------- | :-------------- |
| `GROCERY`      | Retail or wholesale grocery business     | Detail Response |
| `PEST_CONTROL` | Service-based pest control business      | Detail Response |
| `CLOTHING`     | Apparel and fashion retail/manufacturing | Detail Response |

### CompanyDocumentType

| Value                   | Meaning                                      | Used In           |
| :---------------------- | :------------------------------------------- | :---------------- |
| `GST_CERTIFICATE`       | Goods and Services Tax registration document | Document Response |
| `PAN_CARD`              | Permanent Account Number card image          | Document Response |
| `BUSINESS_REGISTRATION` | Legal registration certificate               | Document Response |
| `OTHER`                 | Any other supporting identification          | Document Response |

## API List

| Method | Endpoint                                                 | Purpose                                   | Authorization Required |
| :----- | :------------------------------------------------------- | :---------------------------------------- | :--------------------- |
| `GET`  | `/api/v1/super-admin/company-management/list`            | List companies with filters               | `ROLE_SERAVION`        |
| `GET`  | `/api/v1/super-admin/company-management/detail`          | View full company profile and docs        | `ROLE_SERAVION`        |
| `PUT`  | `/api/v1/super-admin/company-management/approval`        | Approve or Reject a company               | `ROLE_SERAVION`        |
| `GET`  | `/api/v1/super-admin/company-management/approved-detail` | View approved company & subscription info | `ROLE_SERAVION`        |

## API Details

### [GET] `/api/v1/super-admin/company-management/list`

**Purpose**
Returns a paginated list of companies registered on the platform. Allows filtering by onboarding status and name-based search. Frontend should call this for the main Company Management dashboard.

**Authorization**

- **Token Required:** Yes
- **Role/Authority:** `ROLE_SERAVION`

**Query Parameters**
| Field | Type | Required | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| `pageNo` | `Integer` | No | Page number (default: 0) | `0` |
| `pageSize` | `Integer` | No | Items per page (default: 10) | `20` |
| `status` | `OnboardingStatus` | No | Filter by status | `PENDING` |
| `search` | `String` | No | Search by company name/email | `Alpha Corp` |

**Response Body Structure (VendorPaginationResponse)**
| Field | Type | Description |
| :--- | :--- | :--- |
| `count` | `Long` | Total records matching criteria |
| `next` | `String` | URL for the next page |
| `prev` | `String` | URL for the previous page |
| `data` | `List<CompanyManagementListItemResponse>` | List of company summaries |

**CompanyManagementListItemResponse Fields**
| Field | Type | Description |
| :--- | :--- | :--- |
| `companyId` | `Long` | Unique identifier |
| `companyName` | `String` | Corporate name |
| `email` | `String` | Primary contact email |
| `contactPerson` | `String` | Name of the primary contact |
| `phone` | `String` | Contact phone number |
| `createdOn` | `Instant` | Registration timestamp |
| `subscriptionStart`| `LocalDate` | Start date of active subscription |
| `expiryDate` | `LocalDate` | End date of active subscription |
| `location` | `String` | City/Location string |
| `branches` | `Integer` | Number of branches registered |
| `technicians` | `Integer` | Number of technicians registered |
| `status` | `OnboardingStatus` | Current onboarding status |

**Full Response JSON Examples**

#### Success Response (List)

```json
{
  "status": "OK",
  "message": "Company list fetched successfully",
  "data": {
    "count": 1,
    "next": null,
    "prev": null,
    "data": [
      {
        "companyId": 123,
        "companyName": "Tech Solutions Pvt Ltd",
        "email": "ceo@techsolutions.com",
        "contactPerson": "John Doe",
        "phone": "9876543210",
        "createdOn": "2023-10-01T10:00:00Z",
        "subscriptionStart": "2023-10-05",
        "expiryDate": "2024-10-05",
        "location": "Mumbai, Maharashtra",
        "branches": 5,
        "technicians": 10,
        "status": "APPROVED"
      }
    ]
  }
}
```

#### Empty State Response

```json
{
  "status": "OK",
  "message": "Company list fetched successfully",
  "data": {
    "count": 0,
    "next": null,
    "prev": null,
    "data": []
  }
}
```

**cURL Example**

```bash
curl --location 'http://localhost:8080/api/v1/super-admin/company-management/list?pageNo=0&pageSize=10&status=PENDING' \
--header 'Authorization: Bearer <your_token>'
```

---

### [GET] `/api/v1/super-admin/company-management/detail`

**Purpose**
Fetches full company details, including address, tax identifiers, and uploaded documents. This is used by the admin to review the company before granting approval.

**Authorization**

- **Token Required:** Yes
- **Role/Authority:** `ROLE_SERAVION`

**Query Parameters**
| Field | Type | Required | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| `companyId` | `Long` | Yes | ID of the company to view | `123` |

**Response Body (CompanyDetailViewResponse)**
| Field | Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `companyId` | `Long` | Unique identifier | `123` |
| `companyCode` | `String` | Internal system code | `COMP001` |
| `companyName` | `String` | Legal name | `Tech Solutions` |
| `industryType` | `IndustryType`| Sector of operation | `GROCERY` |
| `contactPersonName`| `String` | Primary contact | `John Doe` |
| `contactEmail` | `String` | Primary email | `ceo@tech.com` |
| `contactPhone` | `String` | Primary phone | `9876543210` |
| `officeShopNumber` | `String` | Address part 1 | `Flat 101` |
| `addressLine1` | `String` | Street address | `Main Road` |
| `addressLine2` | `String` | Area address | `Near Station` |
| `city` | `String` | City | `Mumbai` |
| `state` | `String` | State | `Maharashtra` |
| `pincode` | `String` | Postal code | `400001` |
| `gstNumber` | `String` | Tax identifier | `27AAAAA0000A1Z5` |
| `panNumber` | `String` | Tax identifier | `ABCDE1234F` |
| `licenseNumber` | `String` | Business license | `LIC12345` |
| `status` | `OnboardingStatus`| Current status | `PENDING` |
| `rejectionReason`| `String` | Reason if rejected | `Invalid documents` |
| `adminComment` | `String` | Internal notes | `Verified via call` |
| `enableTrial` | `Boolean` | Trial status | `true` |
| `trialFromDate` | `LocalDate` | Trial start | `2023-11-01` |
| `trialToDate` | `LocalDate` | Trial end | `2023-11-15` |
| `createdAt` | `Instant` | Registration date | `2023-10-01T...` |
| `submittedAt` | `Instant` | Submission date | `2023-10-02T...` |
| `verifiedDocuments`| `List` | Docs already marked valid | `[...]` |
| `unverifiedDocuments`| `List` | New/Pending documents | `[...]` |

**CompanyDocumentResponse structure**
| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | `Long` | Doc entry ID |
| `documentType`| `CompanyDocumentType` | Type of doc |
| `documentName`| `String` | Original filename |
| `documentUrl` | `String` | Link to view file |
| `verified` | `Boolean` | Verification bit |

**Full Response JSON Example**

```json
{
  "status": "OK",
  "message": "Company detail fetched successfully",
  "data": {
    "companyId": 123,
    "companyCode": "TECH001",
    "companyName": "Tech Solutions",
    "industryType": "GROCERY",
    "contactPersonName": "John Doe",
    "contactEmail": "ceo@tech.com",
    "contactPhone": "9876543210",
    "officeShopNumber": "101",
    "addressLine1": "Main St",
    "city": "Mumbai",
    "state": "Maharashtra",
    "pincode": "400001",
    "gstNumber": "27AAAAA0000A1Z5",
    "panNumber": "ABCDE1234F",
    "status": "PENDING",
    "enableTrial": false,
    "verifiedDocuments": [],
    "unverifiedDocuments": [
      {
        "id": 1,
        "documentType": "GST_CERTIFICATE",
        "documentName": "gst_cert.pdf",
        "documentUrl": "https://s3.aws.com/bucket/doc1",
        "verified": false
      }
    ]
  }
}
```

**cURL Example**

```bash
curl --location 'http://localhost:8080/api/v1/super-admin/company-management/detail?companyId=123' \
--header 'Authorization: Bearer <your_token>'
```

---

### [PUT] `/api/v1/super-admin/company-management/approval`

**Purpose**
Updates the onboarding status of a company. Allows the admin to Approve or Reject. If approving, the admin can also enable a trial period.

**Authorization**

- **Token Required:** Yes
- **Role/Authority:** `ROLE_SERAVION`

**Request Body (CompanyApprovalRequest)**
| Field | Type | Required | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| `companyId` | `Long` | Yes | ID of company | `123` |
| `status` | `OnboardingStatus`| Yes | Target status | `APPROVED` |
| `rejectionReason`| `String` | No | Required if rejecting | `Documents unclear` |
| `enableTrial` | `Boolean` | No | Grant trial access | `true` |
| `trialFromDate` | `LocalDate` | No | Trial start date | `2023-11-01` |
| `trialToDate` | `LocalDate` | No | Trial end date | `2023-11-15` |
| `adminComment` | `String` | No | Internal remark | `Approved after review` |

**Full Request JSON Examples**

#### Approve With Trial

```json
{
  "companyId": 123,
  "status": "APPROVED",
  "enableTrial": true,
  "trialFromDate": "2023-11-01",
  "trialToDate": "2023-11-15",
  "adminComment": "Verified documents. Granting 15 days trial."
}
```

#### Reject Request

```json
{
  "companyId": 123,
  "status": "REJECTED",
  "rejectionReason": "PAN card photo is blurry. Please re-upload.",
  "adminComment": "Flagged for document quality check."
}
```

**Response**

- **Success Code:** `200 OK`
- **Body:** `CompanyDetailViewResponse` (Updated state)

**cURL Example**

```bash
curl --location --request PUT 'http://localhost:8080/api/v1/super-admin/company-management/approval' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <your_token>' \
--data '{
    "companyId": 123,
    "status": "APPROVED",
    "enableTrial": false,
    "adminComment": "Verified"
}'
```

---

### [GET] `/api/v1/super-admin/company-management/approved-detail`

**Purpose**
Used to view detailed information about an **already approved** company, including their active subscription, billing totals, and branch/technician quotas.

**Authorization**

- **Token Required:** Yes
- **Role/Authority:** `ROLE_SERAVION`

**Query Parameters**
| Field | Type | Required | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| `companyId` | `Long` | Yes | ID of the company | `123` |

**Response Body (CompanyApprovedDetailResponse)**
| Field | Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `companyId` | `Long` | Unique identifier | `123` |
| `companyName` | `String` | Legal name | `Alpha Services` |
| `verificationStatus`| `OnboardingStatus`| Should be `APPROVED` | `APPROVED` |
| `subscriptionId` | `String` | Active sub ID | `SUB-9988` |
| `subscriptionStatus`| `String` | `ACTIVE`, `EXPIRED`, etc. | `ACTIVE` |
| `daysRemaining` | `Long` | Days until expiry | `45` |
| `planType` | `String` | Name of plan | `Premium Annual` |
| `startDate` | `LocalDate` | Sub start | `2023-01-01` |
| `endDate` | `LocalDate` | Sub end | `2023-12-31` |
| `finalTotal` | `BigDecimal` | Total paid | `12500.00` |
| `numberOfBranches` | `Integer` | Branch quota | `5` |
| `numberOfTechnicians`| `Integer`| Tech quota | `10` |

**Full Response JSON Example**

```json
{
  "status": "OK",
  "message": "Approved company detail fetched successfully",
  "data": {
    "companyId": 123,
    "companyName": "Alpha Services",
    "industryType": "PEST_CONTROL",
    "verificationStatus": "APPROVED",
    "subscriptionId": "SUB-P-1001",
    "subscriptionStatus": "ACTIVE",
    "daysRemaining": 320,
    "planType": "Advanced Growth",
    "duration": "ANNUAL",
    "startDate": "2023-05-01",
    "endDate": "2024-05-01",
    "finalTotal": 45000.0,
    "paymentMethod": "RAZORPAY",
    "autoRenew": true
  }
}
```

---

## Exceptions / Error Cases

| HTTP Status | Reason       | When It Happens            | Typical Message                  | Frontend Handling Note           |
| :---------- | :----------- | :------------------------- | :------------------------------- | :------------------------------- |
| `401`       | Unauthorized | Missing or expired token   | `JWT expired`                    | Redirect to Login                |
| `403`       | Forbidden    | User is not `SERAVION`     | `Access Denied`                  | Show "Access Restricted" screen  |
| `404`       | Not Found    | `companyId` does not exist | `Company not found with id: 123` | Redirect back to list            |
| `400`       | Bad Request  | Missing required fields    | `Status is required`             | Show validation toast            |
| `400`       | Invalid Enum | Wrong status value sent    | `JSON parse error`               | Ensure only enum values are sent |

**Error Response JSON Example (Validation)**

```json
{
  "status": "BAD_REQUEST",
  "message": "Validation failed",
  "errors": {
    "companyId": "Company ID is required",
    "status": "Status is required"
  }
}
```

## Frontend Notes

- **Status Badges:** Use different colors for `PENDING` (Orange), `APPROVED` (Green), and `REJECTED` (Red).
- **Trial Fields:** In the Approval modal, show `trialFromDate` and `trialToDate` inputs only if `enableTrial` checkbox is checked.
- **Document Viewing:** `documentUrl` provides a direct link. If it's a PDF/Image, try to open in a new tab or use a lightweight viewer.
- **Rejection Reason:** When a company status is `REJECTED`, the frontend should clearly display the `rejectionReason` in the detail view so the admin knows why it was declined.
- **ReadOnly:** Most fields in the Detail view for Super Admin are read-only except for the Approval actions.

## Validation and Exception Summary

| Field / Scenario | Validation / Rule                | Error Type    | Frontend Impact                       |
| :--------------- | :------------------------------- | :------------ | :------------------------------------ |
| Approval Request | `companyId` & `status` mandatory | `@NotNull`    | Prevent button click if empty         |
| Trial Dates      | Should be in future              | Business Rule | Validate date picker range            |
| Search Filter    | Debounce input                   | Performance   | Avoid spamming API on every keystroke |

## Frontend Integration Notes

- **Initial Load:** Fetch the list with `pageNo=0` and `status=PENDING` to show new applications first.
- **Navigation:** Deep link to `/detail?companyId={id}` from the list row.
- **Approval Flow:** Upon successful `PUT /approval`, refresh the list or show a success checkmark and navigate back.
- **Role Check:** Guard these routes with a Seravion-only middleware/check.
