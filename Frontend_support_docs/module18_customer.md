# Module 18 – Customer Module

## Short Description

The Customer module manages tenant-scoped customer master data: create and update profiles (manual entry or lead import), view a customer 360° detail screen, search and paginate the customer list, fetch a lightweight dropdown for selectors, and soft-delete customers when business rules allow. Frontend work centers on JWT + tenant headers, RBAC authorities (`CUSTOMER_MANAGEMENT_*`) or CEO role, strict request validation on `CustomerRequest`, enum serialization, and handling both the **wrapper success shape** (`ResponseStructure`) and **RFC-style error shape** (`ValidationErrorResponse`) returned by the global exception handler.

---

## Authorization

| Aspect                 | Details                                                                                                                                                                                                                                                                                                                   |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Authentication**     | Stateless **JWT** (`Authorization: Bearer <access_token>`). All `/api/v1/customer/**` routes require an authenticated user except what SecurityConfig marks `permitAll` (customer APIs are **not** public).                                                                                                               |
| **Method security**    | `@PreAuthorize` uses **`hasRole('CEO')`** **or** **`hasAuthority('...')`** per endpoint (see API List).                                                                                                                                                                                                                   |
| **Dropdown exception** | `GET /api/v1/customer/dropdown` has **no** `@PreAuthorize` — any **authenticated** user with a valid JWT can call it (still subject to tenant + JWT).                                                                                                                                                                     |
| **Tenant / schema**    | **`X-Tenant-ID`** (optional but required in practice for tenant DB routing) resolves the tenant schema (see `TenantResolverFilter`). If omitted, resolver falls back to **default/public** tenant (`TenantContext.DEFAULT_TENANT`). **Assumption:** production tenant users should always send the correct `X-Tenant-ID`. |
| **CEO bypass**         | Users with role **`CEO`** satisfy authorization without needing the listed authorities.                                                                                                                                                                                                                                   |
| **Headers**            | **`Authorization`**: Bearer JWT. **`X-Tenant-ID`**: tenant schema id (lowercased/trimmed server-side). **`Content-Type`**: `application/json` for bodies.                                                                                                                                                                 |

---

## Enums Used In This Module

### EntryMode

| Value              | Meaning                                                                                                                                 | Used In                                                                                                      |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `MANUAL_ENTRY`     | Customer created from form data (no lead conversion).                                                                                   | `CustomerRequest.entryMode`, persisted on `Customer`, echoed as string on `CustomerDetailResponse.entryMode` |
| `IMPORT_FROM_LEAD` | Create flow links a lead; server loads lead by `leadId`, sets `LeadStatus` to **CONVERTED** (lead module), copies `leadId` to customer. | Same as above                                                                                                |

**Frontend notes:** When `IMPORT_FROM_LEAD`, send a valid existing `leadId`. Backend throws **404** if lead missing (`CustomerServiceImpl`).

---

### CustomerType

| Value      | Meaning                 | Used In                                                |
| ---------- | ----------------------- | ------------------------------------------------------ |
| `CONTRACT` | Contract-type customer. | Filter query, `CustomerRequest`, list/detail responses |
| `ONE_TIME` | One-time customer.      | Same                                                   |
| `PRODUCT`  | Product-type customer.  | Same                                                   |

**Frontend notes:** **Cannot be changed after creation** — update with a different `customerType` returns **400** (`CustomerServiceImpl.update`).

---

### Status

| Value      | Meaning                                                        | Used In                                                                                     |
| ---------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `ACTIVE`   | Active customer.                                               | Filter, `CustomerRequest.status`, list/detail; dropdown lists only **ACTIVE** + not deleted |
| `INACTIVE` | Inactive (e.g. after deactivation).                            | Filter, responses                                                                           |
| `DRAFT`    | Draft record; mapper default if `status` omitted is **DRAFT**. | Create/update mapping                                                                       |

**Frontend notes:** Create path: if `status` is **not** `DRAFT`, service runs **full** `validate(req, true)`. If `status` **is** `DRAFT`, service runs relaxed `validate(req, false)` (mostly `entryMode`). **Important:** `CustomerRequest` still has **Jakarta `@NotNull` / `@NotBlank` / `@Pattern`** on many fields, so a “minimal draft” body **cannot** omit those fields unless backend adds validation groups — **effective behavior: almost all fields required on wire for create/update** (see Frontend Integration Notes).

Delete sets status to **`INACTIVE`** and `isDeleted` **true**.

---

### Other enum files in `customer/enums` (Country, State, Branch, City)

These types exist under the customer package but **`CustomerRequest` / responses use plain `String` for `country`, `state`, `city`, etc.** They are **not** exposed as enum values on the API surface in the reviewed code. **Missing from backend context:** any separate API that returns these as enums—none found in `CustomerController`.

---

## API List

| Method   | Endpoint                                                     | Purpose                                             | Authorization Required                                      |
| -------- | ------------------------------------------------------------ | --------------------------------------------------- | ----------------------------------------------------------- |
| `GET`    | `/api/v1/customer/dropdown`                                  | Active customers for selects                        | JWT only (no `CUSTOMER_MANAGEMENT_*` / CEO check on method) |
| `POST`   | `/api/v1/customer`                                           | Create customer                                     | `CEO` **or** `CUSTOMER_MANAGEMENT_ADD`                      |
| `PUT`    | `/api/v1/customer/update`                                    | Update customer                                     | `CEO` **or** `CUSTOMER_MANAGEMENT_EDIT`                     |
| `GET`    | `/api/v1/customer/by-id`                                     | Customer 360° detail                                | `CEO` **or** `CUSTOMER_MANAGEMENT_READ`                     |
| `GET`    | `/api/v1/customer/contract-logs`                             | 18.3.2 Tab 2: Contract Logs grid (paginated)        | `CEO` **or** `CUSTOMER_MANAGEMENT_READ`                     |
| `GET`    | `/api/v1/customer/sales-orders-service-history`              | 18.3.3 Tab 3: SO + Service History grid (paginated) | `CEO` **or** `CUSTOMER_MANAGEMENT_READ`                     |
| `GET`    | `/api/v1/customer/sales-orders-service-history/export-excel` | 18.3.3 Export: Download Service History `.xlsx`     | `CEO` **or** `CUSTOMER_MANAGEMENT_READ`                     |
| `GET`    | `/api/v1/customer`                                           | Paginated filter/search                             | `CEO` **or** `CUSTOMER_MANAGEMENT_READ`                     |
| `DELETE` | `/api/v1/customer/delete`                                    | Soft-delete / deactivate                            | `CEO` **or** `CUSTOMER_MANAGEMENT_DELETE`                   |

**Base path:** `/api/v1/customer`

---

## API Details

---

### `GET /api/v1/customer/dropdown`

**Purpose**  
Returns a minimal list of customers where `isDeleted == false` and `status == ACTIVE` for dropdowns and type-ahead.

**Authorization**

- **Token:** Required.
- **Role/authority:** No method-level authority — authenticated user only.
- **Tenant:** Resolved via `X-Tenant-ID` (or default).

**Path parameters:** Not applicable.

**Query parameters:** Not applicable.

**Request body:** Not applicable.

**Response**

- **HTTP:** `200 OK`
- **Body:** `ResponseStructure<List<CustomerDropdownResponse>>`

| Field (data item) | Type   | Description                             |
| ----------------- | ------ | --------------------------------------- |
| `id`              | string | Logical customer id (e.g. `CUST-xxxxx`) |
| `customerName`    | string | Maps from `Customer.fullName`           |

#### Full Response JSON Examples

**Success – multiple rows**

```json
{
  "status": 200,
  "message": "Customer Dropdown Fetched Successfully",
  "data": [
    {
      "id": "CUST-A1B2C",
      "customerName": "ABC Innovations Pvt Ltd"
    },
    {
      "id": "CUST-X9Y8Z",
      "customerName": "Global Corp"
    }
  ]
}
```

**Success – empty list**

```json
{
  "status": 200,
  "message": "Customer Dropdown Fetched Successfully",
  "data": []
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason       | When It Happens                                              | Typical Message                | Frontend Handling Note            |
| ----------- | ------------ | ------------------------------------------------------------ | ------------------------------ | --------------------------------- |
| 401         | Unauthorized | Missing/invalid JWT                                          | From `AuthenticationException` | Redirect to login / refresh token |
| 403         | Forbidden    | Rare if no `@PreAuthorize` on method; possible in edge cases | "Access Denied…"               | Check roles if behavior changes   |
| 500         | Server error | Unhandled                                                    | Varies                         | Show generic error                |

#### Error Response JSON Examples

**Unauthorized (typical `ValidationErrorResponse`)**

```json
{
  "timestamp": "2026-04-13T10:15:30.123456789",
  "status": 401,
  "error": "Unauthorized",
  "message": "Full authentication is required to access this resource",
  "path": "/api/v1/customer/dropdown",
  "validationErrors": null
}
```

#### cURL

```bash
curl -sS -X GET "{{baseUrl}}/api/v1/customer/dropdown" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Accept: application/json"
```

---

### `POST /api/v1/customer`

**Purpose**  
Creates a customer with generated logical id `CUST-` + 5 hex chars. Optionally imports from a lead (`IMPORT_FROM_LEAD` + `leadId`). Enforces duplicate checks on **phone** and **GST** (if provided). Converts lead to **CONVERTED** when importing.

**Authorization**

- **Token:** Required.
- **Authority:** `CUSTOMER_MANAGEMENT_ADD` **or** role `CEO`.
- **Tenant:** `X-Tenant-ID` for correct schema.

**Path parameters:** Not applicable.

**Query parameters:** Not applicable.

**Request Body Fields (`CustomerRequest`)**

| Field                   | Type   | Required    | Validation                   | Description                             | Example                          | Allowed values                       |
| ----------------------- | ------ | ----------- | ---------------------------- | --------------------------------------- | -------------------------------- | ------------------------------------ |
| `entryMode`             | enum   | Yes         | `@NotNull`                   | Manual vs lead import                   | `MANUAL_ENTRY`                   | `MANUAL_ENTRY`, `IMPORT_FROM_LEAD`   |
| `leadId`                | string | Conditional | Max 50 (DB)                  | Required when importing from lead       | `LEAD-2024-001`                  | —                                    |
| `customerType`          | enum   | Yes         | `@NotNull`                   | Business segment                        | `CONTRACT`                       | `CONTRACT`, `ONE_TIME`, `PRODUCT`    |
| `fullName`              | string | Yes         | `@NotBlank`, `@Size(3..100)` | Legal/display name                      | `ABC Innovations Pvt Ltd`        | —                                    |
| `industryType`          | string | No          | —                            | Industry label                          | `IT`                             | —                                    |
| `panNumber`             | string | No          | PAN regex                    | Indian PAN                              | `ABCDE1234F`                     | Pattern `^[A-Z]{5}[0-9]{4}[A-Z]{1}$` |
| `gstNumber`             | string | No          | GST regex                    | Unique if provided                      | `27AAAAA0000A1Z5`                | 15-char pattern per annotation       |
| `contactPerson`         | string | Yes         | `@NotBlank`, `@Size(3..100)` | Primary contact                         | `John Doe`                       | —                                    |
| `designation`           | string | No          | —                            | Contact title                           | `Operations Manager`             | —                                    |
| `phone`                 | string | Yes         | 10-digit pattern             | Primary mobile                          | `9876543210`                     | `^[0-9]{10}$`                        |
| `alternatePhone`        | string | No          | 10-digit if set              | Secondary                               | `9123456780`                     | `^[0-9]{10}$`                        |
| `email`                 | string | Yes         | `@NotBlank`, `@Email`        | Contact email                           | `ops@abc.com`                    | —                                    |
| `branchId`              | string | Yes         | `@NotBlank`                  | Branch FK                               | `BR-MUM-01`                      | —                                    |
| `billingAddressLine1`   | string | Yes         | `@Size(10..200)`             | Billing line 1                          | `123, Andheri East, MIDC`        | —                                    |
| `billingAddressLine2`   | string | No          | —                            | Billing line 2                          | `Near Metro`                     | —                                    |
| `city`                  | string | Yes         | `@NotBlank`                  | City                                    | `Mumbai`                         | —                                    |
| `state`                 | string | Yes         | `@NotBlank`                  | State                                   | `Maharashtra`                    | —                                    |
| `pincode`               | string | Yes         | 6-digit pattern              | PIN                                     | `400093`                         | `^[0-9]{6}$`                         |
| `country`               | string | No          | Default in DTO builder       | Default `"India"`                       | `India`                          | —                                    |
| `googleMapUrl`          | string | Yes         | `@NotBlank`                  | Map link                                | `https://maps.google.com/?q=...` | —                                    |
| `financeContactName`    | string | Yes         | `@NotBlank`                  | Finance contact                         | `Priya Sharma`                   | —                                    |
| `financeContactPhone`   | string | Yes         | 10-digit pattern             | Finance phone                           | `9900112233`                     | —                                    |
| `financeContactEmail`   | string | No          | `@Email` if present          | Finance email                           | `finance@abc.com`                | —                                    |
| `status`                | enum   | No          | —                            | Defaults in mapper to **DRAFT** if null | `ACTIVE`                         | `ACTIVE`, `INACTIVE`, `DRAFT`        |
| `reasonForDeactivation` | string | No          | —                            | Reason text                             | `Merged with parent`             | —                                    |

**Nested / lists:** None on request.

#### Full Request JSON Examples

**Minimal valid request (Assumption: "minimal" still satisfies all `@NotBlank`/patterns — true minimal partial draft is blocked by validation)**

```json
{
  "entryMode": "MANUAL_ENTRY",
  "customerType": "CONTRACT",
  "fullName": "Minimal Corp Pvt Ltd",
  "contactPerson": "Test User",
  "phone": "9876543210",
  "email": "test@minimal.com",
  "branchId": "BR-01",
  "billingAddressLine1": "1234567890 Street Name Extended",
  "city": "Mumbai",
  "state": "Maharashtra",
  "pincode": "400001",
  "country": "India",
  "googleMapUrl": "https://maps.google.com/?q=19.0760,72.8777",
  "financeContactName": "Finance Head",
  "financeContactPhone": "9123456789"
}
```

**Complete valid request (ACTIVE)**

```json
{
  "entryMode": "MANUAL_ENTRY",
  "customerType": "PRODUCT",
  "fullName": "Zenith Retail India Pvt Ltd",
  "industryType": "Retail",
  "panNumber": "ABCDE1234F",
  "gstNumber": "27AAAAA0000A1Z5",
  "contactPerson": "Anita Rao",
  "designation": "Head of Procurement",
  "phone": "9988776655",
  "alternatePhone": "9123456780",
  "email": "anita.rao@zenithretail.in",
  "branchId": "BR-DEL-02",
  "billingAddressLine1": "Plot 45, Sector 18, Industrial Area, Long Address Line",
  "billingAddressLine2": "Near City Center Mall",
  "city": "New Delhi",
  "state": "Delhi",
  "pincode": "110001",
  "country": "India",
  "googleMapUrl": "https://www.google.com/maps/place/Connaught+Place",
  "financeContactName": "Ravi Mehta",
  "financeContactPhone": "9911223344",
  "financeContactEmail": "ravi.mehta@zenithretail.in",
  "status": "ACTIVE"
}
```

**Import from lead**

```json
{
  "entryMode": "IMPORT_FROM_LEAD",
  "leadId": "LEAD-2026-0142",
  "customerType": "CONTRACT",
  "fullName": "Converted From Lead LLC",
  "contactPerson": "Lead Owner",
  "phone": "9876501234",
  "email": "owner@converted.com",
  "branchId": "BR-BLR-01",
  "billingAddressLine1": "1234567890 Tech Park Road, Bellandur",
  "city": "Bengaluru",
  "state": "Karnataka",
  "pincode": "560103",
  "country": "India",
  "googleMapUrl": "https://maps.google.com/?q=Bellandur",
  "financeContactName": "CFO Name",
  "financeContactPhone": "9000011122",
  "financeContactEmail": "cfo@converted.com",
  "status": "DRAFT"
}
```

**DRAFT status (service relaxes internal validate, body still fully valid per Bean Validation)**

```json
{
  "entryMode": "MANUAL_ENTRY",
  "customerType": "ONE_TIME",
  "fullName": "Draft Customer Ltd",
  "contactPerson": "Draft User",
  "phone": "9812345678",
  "email": "draft@example.com",
  "branchId": "BR-01",
  "billingAddressLine1": "1234567890 Enough Length St, Area Name",
  "city": "Pune",
  "state": "Maharashtra",
  "pincode": "411001",
  "country": "India",
  "googleMapUrl": "https://maps.google.com/?q=Pune",
  "financeContactName": "Finance",
  "financeContactPhone": "9823456789",
  "status": "DRAFT"
}
```

**Enum-driven variation – CONTRACT**

```json
{
  "entryMode": "MANUAL_ENTRY",
  "customerType": "CONTRACT",
  "fullName": "Contract Customer Name Here",
  "contactPerson": "Contact Person",
  "phone": "9876543210",
  "email": "c@example.com",
  "branchId": "BR-01",
  "billingAddressLine1": "1234567890 Billing Street Extended Addr",
  "city": "Chennai",
  "state": "Tamil Nadu",
  "pincode": "600001",
  "country": "India",
  "googleMapUrl": "https://maps.google.com/?q=Chennai",
  "financeContactName": "Fin",
  "financeContactPhone": "9123456789",
  "status": "ACTIVE"
}
```

#### Response

- **Success status:** `201 Created`
- **Wrapper:** `ResponseStructure<CustomerResponse>`

| Field                        | Type                 | Notes                                            |
| ---------------------------- | -------------------- | ------------------------------------------------ |
| `id`                         | string               | Generated `CUST-xxxxx`                           |
| `fullName`, `phone`, `email` | string               |                                                  |
| `customerType`               | enum string          |                                                  |
| `status`                     | enum                 |                                                  |
| `branchId`, `branchName`     | string               | `branchName` enriched from branch table if found |
| `leadId`, `leadName`         | string               | Set when lead exists                             |
| `createdBy`, `createdAt`     | string / ISO instant |                                                  |
| `totalSites`                 | integer              | Default **0**                                    |

#### Full Response JSON Examples

**Success**

```json
{
  "status": 201,
  "message": "Customer Created Successfully",
  "data": {
    "id": "CUST-A3F2B",
    "fullName": "Zenith Retail India Pvt Ltd",
    "customerType": "PRODUCT",
    "phone": "9988776655",
    "email": "anita.rao@zenithretail.in",
    "branchId": "BR-DEL-02",
    "branchName": "Delhi Main Branch",
    "leadId": null,
    "leadName": null,
    "status": "ACTIVE",
    "createdBy": "Jane Doe",
    "createdAt": "2026-04-13T09:30:00Z",
    "totalSites": 0
  }
}
```

**With lead enrichment (after IMPORT_FROM_LEAD)**

```json
{
  "status": 201,
  "message": "Customer Created Successfully",
  "data": {
    "id": "CUST-91ACD",
    "fullName": "Converted From Lead LLC",
    "customerType": "CONTRACT",
    "phone": "9876501234",
    "email": "owner@converted.com",
    "branchId": "BR-BLR-01",
    "branchName": "Bengaluru HQ",
    "leadId": "LEAD-2026-0142",
    "leadName": "Acme Industries Lead",
    "status": "DRAFT",
    "createdBy": "System User",
    "createdAt": "2026-04-13T10:00:00Z",
    "totalSites": 0
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason          | When It Happens                   | Typical Message                                    | Frontend Handling Note                   |
| ----------- | --------------- | --------------------------------- | -------------------------------------------------- | ---------------------------------------- |
| 400         | Validation      | Jakarta validation failures       | `"Input validation failed"` + field map            | Highlight fields from `validationErrors` |
| 400         | Malformed JSON  | Parser errors                     | `"Malformed JSON request"` or invalid enum message | Fix JSON / enum strings                  |
| 400         | Null request    | Service guard                     | `Request cannot be null`                           | N/A for normal clients                   |
| 401         | Unauthorized    | No/wrong JWT                      | Auth message                                       | Login / refresh                          |
| 403         | Forbidden       | Missing ADD authority and not CEO | Access denied                                      | Hide create action                       |
| 404         | Lead not found  | Bad `leadId` for import           | `Lead not found with ID: ...`                      | Verify lead id                           |
| 409         | Duplicate phone | Phone exists                      | `Phone number already registered...`               | Unique phone UX                          |
| 409         | Duplicate GST   | GST exists                        | `GST already registered...`                        | Unique GST UX                            |
| 409         | DB conflict     | Unique constraint (e.g. GST)      | Resolver message / generic                         | Same as above                            |

#### Error Response JSON Examples

**Validation (400)**

```json
{
  "timestamp": "2026-04-13T10:05:00.123456789",
  "status": 400,
  "error": "Bad Request",
  "message": "Input validation failed",
  "path": "/api/v1/customer",
  "validationErrors": {
    "panNumber": "Invalid PAN Number format",
    "billingAddressLine1": "Billing Address must be between 10 and 200 characters"
  }
}
```

**Invalid enum / wrong JSON shape (400)**

```json
{
  "timestamp": "2026-04-13T10:06:00.123456789",
  "status": 400,
  "error": "Bad Request",
  "message": "Invalid value 'WRONG' for field 'entryMode'",
  "path": "/api/v1/customer",
  "validationErrors": null
}
```

**Lead not found (404)**

```json
{
  "timestamp": "2026-04-13T10:07:00.123456789",
  "status": 404,
  "error": "Not Found",
  "message": "Lead not found with ID: LEAD-missing",
  "path": "/api/v1/customer",
  "validationErrors": null
}
```

**Duplicate phone (409)**

```json
{
  "timestamp": "2026-04-13T10:08:00.123456789",
  "status": 409,
  "error": "Conflict",
  "message": "Phone number already registered under another customer",
  "path": "/api/v1/customer",
  "validationErrors": null
}
```

#### cURL

```bash
curl -sS -X POST "{{baseUrl}}/api/v1/customer" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Content-Type: application/json" \
  -d '{
    "entryMode": "MANUAL_ENTRY",
    "customerType": "CONTRACT",
    "fullName": "API Test Customer",
    "contactPerson": "Contact Name",
    "phone": "9876543210",
    "email": "api@test.com",
    "branchId": "BR-01",
    "billingAddressLine1": "1234567890 Long Street Address Line Here",
    "city": "Mumbai",
    "state": "Maharashtra",
    "pincode": "400001",
    "country": "India",
    "googleMapUrl": "https://maps.google.com/",
    "financeContactName": "Finance",
    "financeContactPhone": "9123456789",
    "status": "ACTIVE"
  }'
```

---

### `PUT /api/v1/customer/update`

**Purpose**  
Updates an existing customer; writes **audit log** entries for changed fields; enforces immutability of **customerType** and **PAN**; duplicate phone/GST checks.

**Authorization**

- **Token:** Required.
- **Authority:** `CUSTOMER_MANAGEMENT_EDIT` **or** `CEO`.

**Path parameters:** Not applicable.

**Query parameters**

| Field | Type   | Required | Description         | Example      |
| ----- | ------ | -------- | ------------------- | ------------ |
| `id`  | string | Yes      | Customer logical id | `CUST-A3F2B` |

**Request body:** Same schema as **POST** (`CustomerRequest`).

#### Full Request JSON Examples

**Update contact and address**

```json
{
  "entryMode": "MANUAL_ENTRY",
  "customerType": "CONTRACT",
  "fullName": "Updated Legal Name Pvt Ltd",
  "industryType": "Logistics",
  "panNumber": "ABCDE1234F",
  "gstNumber": "27AAAAA0000A1Z5",
  "contactPerson": "New Contact",
  "designation": "VP Ops",
  "phone": "9876543210",
  "alternatePhone": "9000011122",
  "email": "newcontact@firm.com",
  "branchId": "BR-MUM-03",
  "billingAddressLine1": "1234567890 New Billing Address Extended Here",
  "billingAddressLine2": "Floor 7",
  "city": "Mumbai",
  "state": "Maharashtra",
  "pincode": "400051",
  "country": "India",
  "googleMapUrl": "https://maps.google.com/?q=19.1136,72.8697",
  "financeContactName": "Finance Contact",
  "financeContactPhone": "9811122233",
  "financeContactEmail": "finance@firm.com",
  "status": "ACTIVE"
}
```

**Attempt to change customerType (will fail at service if type differs from stored)**

```json
{
  "entryMode": "MANUAL_ENTRY",
  "customerType": "PRODUCT",
  "fullName": "Same Customer",
  "contactPerson": "Someone",
  "phone": "9876543210",
  "email": "a@b.com",
  "branchId": "BR-01",
  "billingAddressLine1": "1234567890 Street Address Long Enough",
  "city": "Pune",
  "state": "Maharashtra",
  "pincode": "411001",
  "country": "India",
  "googleMapUrl": "https://maps.google.com/",
  "financeContactName": "Fin",
  "financeContactPhone": "9123456789",
  "status": "ACTIVE"
}
```

#### Response

- **HTTP:** `200 OK`
- **Body:** `ResponseStructure<CustomerResponse>` (same shape as create).

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Customer Updated Successfully",
  "data": {
    "id": "CUST-A3F2B",
    "fullName": "Updated Legal Name Pvt Ltd",
    "customerType": "CONTRACT",
    "phone": "9876543210",
    "email": "newcontact@firm.com",
    "branchId": "BR-MUM-03",
    "branchName": "Mumbai Andheri",
    "leadId": null,
    "leadName": null,
    "status": "ACTIVE",
    "createdBy": "Jane Doe",
    "createdAt": "2026-04-01T08:00:00Z",
    "totalSites": 0
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason           | When                   | Message (typical)                                | Frontend note    |
| ----------- | ---------------- | ---------------------- | ------------------------------------------------ | ---------------- |
| 400         | Deleted customer | `isDeleted` true       | `Cannot update deleted customer`                 | Block edit UI    |
| 400         | Immutable type   | `customerType` changed | `Customer Type cannot be changed after creation` | Lock dropdown    |
| 400         | Immutable PAN    | PAN changed            | `PAN Number cannot be changed after creation`    | Read-only PAN    |
| 400         | Validation       | Bean validation        | Field errors                                     | Form errors      |
| 404         | Missing          | Bad id                 | `Customer Not Found`                             | Toast + redirect |
| 409         | Phone            | Duplicate              | Same as create                                   |                  |
| 409         | GST              | Duplicate              | Same as create                                   |                  |

#### Error Response JSON Examples

**Cannot update deleted customer**

```json
{
  "timestamp": "2026-04-13T11:00:00.123456789",
  "status": 400,
  "error": "Bad Request",
  "message": "Cannot update deleted customer",
  "path": "/api/v1/customer/update",
  "validationErrors": null
}
```

**Customer type change**

```json
{
  "timestamp": "2026-04-13T11:01:00.123456789",
  "status": 400,
  "error": "Bad Request",
  "message": "Customer Type cannot be changed after creation",
  "path": "/api/v1/customer/update",
  "validationErrors": null
}
```

#### cURL

```bash
curl -sS -X PUT "{{baseUrl}}/api/v1/customer/update?id=CUST-A3F2B" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Content-Type: application/json" \
  -d @customer-update.json
```

---

### `GET /api/v1/customer/by-id`

**Purpose**  
Full customer profile + branch name + lead snapshot + **360°** sections: contracts (from contract service), sales orders (top 50), placeholders for sites/service history/LTV.

**Authorization**

- **Token:** Required.
- **Authority:** `CUSTOMER_MANAGEMENT_READ` **or** `CEO`.

**Query parameters**

| Field | Type   | Required | Description | Example      |
| ----- | ------ | -------- | ----------- | ------------ |
| `id`  | string | Yes      | Customer id | `CUST-A3F2B` |

**Request body:** Not applicable.

**Response fields (summary)**

| Field                    | Type                          | Notes                                                         |
| ------------------------ | ----------------------------- | ------------------------------------------------------------- |
| `status`                 | string                        | Enum **name** as string (not nested enum object)              |
| `entryMode`              | string                        | Enum name                                                     |
| `leadType`, `leadStatus` | string                        | From lead entity `.name()`                                    |
| `contracts`              | `ContractSummaryResponse[]`   | id, contractNumber, status, startDate, endDate, contractValue |
| `salesOrders`            | `SalesOrderSummaryResponse[]` | id, orderNumber, status, orderDate, totalAmount               |
| `activeSites`            | `SiteSummaryResponse[]`       | Currently **always empty list** in service                    |
| `serviceHistory`         | `ServiceHistoryResponse[]`    | **Always empty** in service                                   |
| `ltv`                    | number                        | **Always 0.0** in service                                     |
| Timestamps               | `OffsetDateTime`              | Created/updated/deleted in UTC offset                         |

**Missing from backend context:** Future population of `activeSites`, `serviceHistory`, real `ltv` — code sets stubs.

#### Full Response JSON Examples

**Success – rich detail**

```json
{
  "status": 200,
  "message": "Customer Fetched Successfully",
  "data": {
    "id": "CUST-A3F2B",
    "status": "ACTIVE",
    "reasonForDeactivation": null,
    "entryMode": "IMPORT_FROM_LEAD",
    "leadId": "LEAD-2026-0142",
    "leadName": "Acme Prospect",
    "leadPhone": "9811122233",
    "leadEmail": "prospect@acme.com",
    "leadType": "ENTERPRISE",
    "leadStatus": "CONVERTED",
    "customerType": "CONTRACT",
    "fullName": "Acme Industries Pvt Ltd",
    "industryType": "Manufacturing",
    "panNumber": "ABCDE1234F",
    "gstNumber": "27AAAAA0000A1Z5",
    "contactPerson": "Vikram Singh",
    "designation": "Director",
    "phone": "9876543210",
    "alternatePhone": null,
    "email": "vikram@acme.com",
    "branchId": "BR-MUM-01",
    "branchName": "Mumbai HQ",
    "billingAddressLine1": "1234567890 Industrial Estate Road, Andheri",
    "billingAddressLine2": "Unit 3B",
    "city": "Mumbai",
    "state": "Maharashtra",
    "pincode": "400093",
    "country": "India",
    "googleMapUrl": "https://maps.google.com/?q=Andheri",
    "financeContactName": "CFO Name",
    "financeContactPhone": "9000011223",
    "financeContactEmail": "cfo@acme.com",
    "contracts": [
      {
        "id": "CTR-001",
        "contractNumber": "CTR-001",
        "status": "ACTIVE",
        "startDate": "2025-04-01",
        "endDate": "2028-03-31",
        "contractValue": 1250000.5
      }
    ],
    "salesOrders": [
      {
        "id": "SO-1001",
        "orderNumber": "SO-2026-0042",
        "status": "OPEN",
        "orderDate": "2026-04-01",
        "totalAmount": 45000.0
      }
    ],
    "activeSites": [],
    "serviceHistory": [],
    "ltv": 0.0,
    "createdAt": "2025-12-01T08:30:00Z",
    "updatedAt": "2026-04-10T14:20:00Z",
    "deletedAt": null,
    "createdBy": "Jane Doe",
    "updatedBy": "Admin User",
    "deletedBy": null
  }
}
```

**Lead fields null (manual entry customer)**

```json
{
  "status": 200,
  "message": "Customer Fetched Successfully",
  "data": {
    "id": "CUST-MANUAL",
    "status": "DRAFT",
    "reasonForDeactivation": null,
    "entryMode": "MANUAL_ENTRY",
    "leadId": null,
    "leadName": null,
    "leadPhone": null,
    "leadEmail": null,
    "leadType": null,
    "leadStatus": null,
    "customerType": "ONE_TIME",
    "fullName": "Walk In Customer",
    "contracts": [],
    "salesOrders": [],
    "activeSites": [],
    "serviceHistory": [],
    "ltv": 0.0
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason    | When               | Message              | Frontend                 |
| ----------- | --------- | ------------------ | -------------------- | ------------------------ |
| 404         | Not found | Invalid id         | `Customer Not Found` | Empty state / error page |
| 401/403     | Auth      | Missing permission | Standard             | Hide route               |

#### Error Response JSON Example

**Not found**

```json
{
  "timestamp": "2026-04-13T12:00:00.123456789",
  "status": 404,
  "error": "Not Found",
  "message": "Customer Not Found",
  "path": "/api/v1/customer/by-id",
  "validationErrors": null
}
```

#### cURL

```bash
curl -sS -G "{{baseUrl}}/api/v1/customer/by-id" \
  --data-urlencode "id=CUST-A3F2B" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Accept: application/json"
```

---

### `GET /api/v1/customer`

**Purpose**  
Paginated list with optional search and filters. Search matches **case-insensitive** `fullName`, or **phone** / **id** contains. **Assumption:** Specification does **not** filter `isDeleted` — deleted rows **may** appear unless filtered elsewhere (not in code reviewed).

**Authorization**

- **Token:** Required.
- **Authority:** `CUSTOMER_MANAGEMENT_READ` **or** `CEO`.

**Query parameters**

| Field          | Type    | Required            | Description                                           | Example                     | Allowed values                     |
| -------------- | ------- | ------------------- | ----------------------------------------------------- | --------------------------- | ---------------------------------- |
| `pageNo`       | integer | No (default **0**)  | Zero-based page                                       | `0`                         | ≥ 0                                |
| `pageSize`     | integer | No (default **10**) | Page size                                             | `20`                        | ≥ 1                                |
| `search`       | string  | No                  | Name/phone/id partial                                 | `ABC`                       | —                                  |
| `status`       | enum    | No                  | Filter by status                                      | `ACTIVE`                    | `ACTIVE`, `INACTIVE`, `DRAFT`      |
| `customerType` | enum    | No                  | Filter                                                | `CONTRACT`                  | `CONTRACT`, `ONE_TIME`, `PRODUCT`  |
| `branchId`     | string  | No                  | Exact branch                                          | `BR-MUM-01`                 | —                                  |
| `fromDate`     | string  | No\*                | Start of created range (ISO-8601 **offset datetime**) | `2026-01-01T00:00:00+05:30` | Parsed with `OffsetDateTime.parse` |
| `toDate`       | string  | No\*                | End of range                                          | `2026-04-13T23:59:59+05:30` | Same                               |

\* **Both** `fromDate` and `toDate` must be non-null for the date filter to apply (`CustomerSpecification`). If only one is sent, **that filter is skipped** (no error).

**Request body:** Not applicable.

**Full request examples (query-string only)**

- Minimal: `GET /api/v1/customer` → `pageNo=0`, `pageSize=10`.
- Search: `GET /api/v1/customer?pageNo=0&pageSize=10&search=Zenith`
- Filtered: `GET /api/v1/customer?pageNo=0&pageSize=20&search=retail&status=ACTIVE&customerType=PRODUCT&branchId=BR-DEL-02`
- Date range: `GET /api/v1/customer?pageNo=0&pageSize=10&fromDate=2026-04-01T00:00:00Z&toDate=2026-04-30T23:59:59Z`

#### Response

- **HTTP:** `200 OK`
- **Body:** `ResponseStructure<CustomerProductPaginationResponse<CustomerResponse>>`

| Field   | Type           | Description                                                                         |
| ------- | -------------- | ----------------------------------------------------------------------------------- |
| `count` | long           | Total elements                                                                      |
| `next`  | string \| null | Absolute URL for next page (built from `app.base-url`, may omit `branchId` / dates) |
| `prev`  | string \| null | Previous page URL                                                                   |
| `data`  | array          | `CustomerResponse` items                                                            |

**Known limitation:** `next` / `prev` URLs **only** append `pageSize`, `search`, `status`, `customerType` — **not** `branchId`, `fromDate`, or `toDate`. Frontend should **reapply** full filter set when paging or build URLs client-side.

#### Full Response JSON Examples

**Success – page with results**

```json
{
  "status": 200,
  "message": "Customer Fetched Successfully",
  "data": {
    "count": 48,
    "next": "https://api.example.com/api/v1/customer?pageSize=10&search=Zenith&status=ACTIVE&customerType=CONTRACT&pageNo=1",
    "prev": null,
    "data": [
      {
        "id": "CUST-A3F2B",
        "fullName": "Zenith Retail India Pvt Ltd",
        "customerType": "PRODUCT",
        "phone": "9988776655",
        "email": "anita.rao@zenithretail.in",
        "branchId": "BR-DEL-02",
        "branchName": "Delhi Main Branch",
        "leadId": null,
        "leadName": null,
        "status": "ACTIVE",
        "createdBy": "Jane Doe",
        "createdAt": "2026-04-01T08:00:00Z",
        "totalSites": 0
      }
    ]
  }
}
```

**Empty page**

```json
{
  "status": 200,
  "message": "Customer Fetched Successfully",
  "data": {
    "count": 0,
    "next": null,
    "prev": null,
    "data": []
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason           | When                        | Message                                                                                                                 | Frontend                         |
| ----------- | ---------------- | --------------------------- | ----------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| 400         | Date parse error | Invalid `fromDate`/`toDate` | **Assumption:** may surface as 500 via `RuntimeException` handler — **Missing from backend context:** dedicated handler | Prefer strict ISO offset strings |
| 401/403     | Auth             | Missing READ                | Standard                                                                                                                | —                                |

#### Error Response JSON Example

**Forbidden**

```json
{
  "timestamp": "2026-04-13T13:00:00.123456789",
  "status": 403,
  "error": "Forbidden",
  "message": "Access Denied: You don't have permission to access this resource",
  "path": "/api/v1/customer",
  "validationErrors": null
}
```

#### cURL

```bash
curl -sS -G "{{baseUrl}}/api/v1/customer" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=10" \
  --data-urlencode "search=Zenith" \
  --data-urlencode "status=ACTIVE" \
  --data-urlencode "customerType=CONTRACT" \
  --data-urlencode "branchId=BR-DEL-02" \
  --data-urlencode "fromDate=2026-01-01T00:00:00Z" \
  --data-urlencode "toDate=2026-12-31T23:59:59Z" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Accept: application/json"
```

---

### `GET /api/v1/customer/contract-logs` (18.3.2 Tab 2: Contract Logs)

**Purpose**  
Read-only, paginated **Contract Logs** grid for a customer (Module 19 agreements, including historical).

**Authorization**

- **Token:** Required.
- **Authority:** `CUSTOMER_MANAGEMENT_READ` **or** `CEO`.
- **Tenant:** `X-Tenant-ID` required in practice.

**Query parameters**

| Field        | Type   | Required | Default | Description                     |
| ------------ | ------ | -------- | ------- | ------------------------------- |
| `customerId` | string | Yes      | —       | Customer id (e.g. `CUST-A3F2B`) |
| `pageNo`     | int    | No       | `0`     | 0-based page index              |
| `pageSize`   | int    | No       | `10`    | Page size                       |

#### Response

- **HTTP:** `200 OK`
- **Body:** `ResponseStructure<CustomerProductPaginationResponse<CustomerContractLogRowResponse>>`

`CustomerContractLogRowResponse` fields:

| Field           | Type   | Description                                                                                 |
| --------------- | ------ | ------------------------------------------------------------------------------------------- |
| `contractId`    | string | Contract system id (e.g. `CON-2026-0041`)                                                   |
| `startDate`     | date   | Contract start date (`YYYY-MM-DD`)                                                          |
| `endDate`       | date   | Contract end date (`YYYY-MM-DD`)                                                            |
| `contractValue` | number | Total contract value                                                                        |
| `gmaId`         | string | Source GMA id (Module 17)                                                                   |
| `status`        | string | Display status from Module 19 (`ACTIVE`, `EXPIRING_SOON`, `EXPIRED`, `TERMINATED`, `DRAFT`) |

#### cURL

```bash
curl -sS -G "{{baseUrl}}/api/v1/customer/contract-logs" \
  --data-urlencode "customerId=CUST-A3F2B" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=10" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Accept: application/json"
```

---

### `GET /api/v1/customer/sales-orders-service-history` (18.3.3 Tab 3: Sales Orders & Service History)

**Purpose**  
Read-only, paginated grid of Sales Orders (Module 20) for a customer, with a **service status** snapshot.

**Service status source (current backend behavior)**  
`serviceStatus` is taken from `contract_sales_order_links.service_status` when a link exists for the SO id; otherwise it defaults to `PENDING`.

**Authorization**

- **Token:** Required.
- **Authority:** `CUSTOMER_MANAGEMENT_READ` **or** `CEO`.
- **Tenant:** `X-Tenant-ID` required in practice.

**Query parameters**

| Field        | Type   | Required | Default | Description                     |
| ------------ | ------ | -------- | ------- | ------------------------------- |
| `customerId` | string | Yes      | —       | Customer id (e.g. `CUST-A3F2B`) |
| `pageNo`     | int    | No       | `0`     | 0-based page index              |
| `pageSize`   | int    | No       | `10`    | Page size                       |

#### Response

- **HTTP:** `200 OK`
- **Body:** `ResponseStructure<CustomerProductPaginationResponse<CustomerSalesOrderServiceHistoryRowResponse>>`

`CustomerSalesOrderServiceHistoryRowResponse` fields:

| Field              | Type   | Description                                                                               |
| ------------------ | ------ | ----------------------------------------------------------------------------------------- |
| `salesOrderId`     | string | Sales order id                                                                            |
| `soNumber`         | string | SO number (e.g. `SO-2026-0112`)                                                           |
| `soDate`           | date   | SO date (`YYYY-MM-DD`)                                                                    |
| `linkedContractId` | string | Contract id if contract-based; empty otherwise                                            |
| `orderType`        | string | `Contract` / `One-Time Service` / `Product Sale`                                          |
| `totalValue`       | number | SO grand total                                                                            |
| `soStatus`         | string | `DRAFT` / `OPEN` / `FULFILLED` / `BILLED` / `CANCELLED`                                   |
| `serviceStatus`    | string | `PENDING` by default; otherwise link value (e.g. `SCHEDULED`, `IN_PROGRESS`, `COMPLETED`) |

#### cURL

```bash
curl -sS -G "{{baseUrl}}/api/v1/customer/sales-orders-service-history" \
  --data-urlencode "customerId=CUST-A3F2B" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=10" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Accept: application/json"
```

---

### `GET /api/v1/customer/sales-orders-service-history/export-excel` (18.3.3 Export)

**Purpose**  
Downloads Sales Orders & Service History for a customer as an Excel `.xlsx` file.

**Authorization**

- **Token:** Required.
- **Authority:** `CUSTOMER_MANAGEMENT_READ` **or** `CEO`.
- **Tenant:** `X-Tenant-ID` required in practice.

**Query parameters**

| Field        | Type   | Required | Description                     |
| ------------ | ------ | -------- | ------------------------------- |
| `customerId` | string | Yes      | Customer id (e.g. `CUST-A3F2B`) |

#### Response

- **HTTP:** `200 OK`
- **Content-Type:** `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`
- **Content-Disposition:** `attachment; filename="service-history.xlsx"`
- **Body:** raw bytes

**Excel columns (current implementation)**  
`SO Number`, `SO Date`, `Linked Contract`, `Order Type`, `Total Value`, `SO Status`, `Service Status`

#### cURL (download)

```bash
curl -L -X GET "{{baseUrl}}/api/v1/customer/sales-orders-service-history/export-excel?customerId=CUST-A3F2B" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -o service-history.xlsx
```

---

### `DELETE /api/v1/customer/delete`

**Purpose**  
Soft-delete: sets `isDeleted=true`, `status=INACTIVE`, audit deletion fields, stores `reasonForDeactivation`. **Blocked** if customer has sales orders in **`DRAFT`** or **`OPEN`** (`SalesOrderStatus`).

**Authorization**

- **Token:** Required.
- **Authority:** `CUSTOMER_MANAGEMENT_DELETE` **or** `CEO`.

**Query parameters**

| Field    | Type   | Required | Description           | Example                          |
| -------- | ------ | -------- | --------------------- | -------------------------------- |
| `id`     | string | Yes      | Customer id           | `CUST-A3F2B`                     |
| `reason` | string | Yes      | Passed as query param | `Contract ended, account closed` |

**Request body:** Not applicable.

**Full request example (query-only)**

`DELETE /api/v1/customer/delete?id=CUST-A3F2B&reason=No%20longer%20doing%20business`

#### Response

- **HTTP:** `200 OK`
- **Body:** `ResponseStructure<CustomerResponse>` reflecting **INACTIVE** state and enriched fields.

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Customer Deleted Successfully",
  "data": {
    "id": "CUST-A3F2B",
    "fullName": "Zenith Retail India Pvt Ltd",
    "customerType": "PRODUCT",
    "phone": "9988776655",
    "email": "anita.rao@zenithretail.in",
    "branchId": "BR-DEL-02",
    "branchName": "Delhi Main Branch",
    "leadId": null,
    "leadName": null,
    "status": "INACTIVE",
    "createdBy": "Jane Doe",
    "createdAt": "2026-04-01T08:00:00Z",
    "totalSites": 0
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason        | When                | Message                                                  | Frontend                      |
| ----------- | ------------- | ------------------- | -------------------------------------------------------- | ----------------------------- |
| 404         | Not found     | Bad id              | `Customer Not Found`                                     | —                             |
| 400         | Business rule | Open/draft SO exist | `Open Sales Orders must be fulfilled or cancelled first` | Guide user to SO module first |

#### Error Response JSON Examples

**Blocked by sales orders**

```json
{
  "timestamp": "2026-04-13T14:00:00.123456789",
  "status": 400,
  "error": "Bad Request",
  "message": "Open Sales Orders must be fulfilled or cancelled first",
  "path": "/api/v1/customer/delete",
  "validationErrors": null
}
```

**Not found**

```json
{
  "timestamp": "2026-04-13T14:01:00.123456789",
  "status": 404,
  "error": "Not Found",
  "message": "Customer Not Found",
  "path": "/api/v1/customer/delete",
  "validationErrors": null
}
```

#### cURL

```bash
curl -sS -X DELETE "{{baseUrl}}/api/v1/customer/delete" \
  -G \
  --data-urlencode "id=CUST-A3F2B" \
  --data-urlencode "reason=Customer requested account closure" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Accept: application/json"
```

---

### Frontend Notes (per-endpoint highlights)

| Endpoint                     | Notes                                                                                                                     |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Dropdown                     | Use for dependent fields (e.g. SO, contract) — no extra authority; still send tenant.                                     |
| Create                       | Duplicate UX on 409; lead import updates lead to CONVERTED.                                                               |
| Update                       | Lock **customerType** and **PAN** in UI; block if backend says deleted.                                                   |
| By id                        | `status` is **string**; contracts/SOs are real lists; sites/history/LTV are placeholders.                                 |
| Contract Logs (Tab 2)        | Use for 360° Contract Logs grid. Display `status` may include `EXPIRING_SOON` (derived from Module 19).                   |
| SO + Service History (Tab 3) | `serviceStatus` is read from contract schedule link if present; otherwise defaults to `PENDING`. Export is `.xlsx` bytes. |
| List                         | Reapply filters when using `next`/`prev`; confirm date format; soft-delete visibility **not** excluded in spec.           |
| Delete                       | Requires `reason` query param; check 400 for open SO.                                                                     |

---

## Validation and Exception Summary

| Field / Scenario                             | Validation / Rule               | Error Type                    | Frontend Impact                             |
| -------------------------------------------- | ------------------------------- | ----------------------------- | ------------------------------------------- |
| All `CustomerRequest` annotated fields       | Jakarta patterns/size/email     | 400 + `validationErrors` map  | Inline field errors                         |
| `entryMode` / `customerType` / `status` JSON | Valid enum strings              | 400 invalid format message    | Dropdown-only values                        |
| Duplicate `phone`                            | Unique per tenant DB            | 409                           | Prevent submit / search existing            |
| Duplicate `GST`                              | Unique when present             | 409                           | GST uniqueness                              |
| Lead import                                  | Lead must exist                 | 404                           | Validate lead id first                      |
| Update                                       | `customerType` change           | 400                           | Disable type control                        |
| Update                                       | `panNumber` change              | 400                           | Read-only PAN                               |
| Update                                       | Deleted customer                | 400                           | Hide edit                                   |
| Delete                                       | Open/ Draft SO exist            | 400                           | Workflow to close SO first                  |
| Auth missing                                 | JWT                             | 401                           | Login                                       |
| Authority missing                            | `@PreAuthorize`                 | 403                           | Hide actions                                |
| Success wrapper                              | Business success                | 2xx + `ResponseStructure`     | Read `data`                                 |
| API errors                                   | `ApiBaseException` / validation | 4xx `ValidationErrorResponse` | Use `message` + optional `validationErrors` |

---

## Frontend Integration Notes

- **Dropdown vs list:** Use **`/dropdown`** for quick selects (ACTIVE only); use **`GET /api/v1/customer`** for admin grids with filters.
- **Dependent fields:** `IMPORT_FROM_LEAD` requires valid **`leadId`**; branch display uses **`branchId`** + resolved **`branchName`** on responses.
- **Enum rendering:** Send/receive **STRING** enum names matching Java enums (`ENTRY_MODE`, `CUSTOMER_TYPE`, `STATUS`).
- **Status UI:** After delete, status is **INACTIVE**; update forbidden when deleted.
- **Create vs update:** Same JSON schema; update forbids changing **customerType** and **PAN**.
- **Search/filter:** `search` OR-matches name/phone/id; combine with `status`, `customerType`, `branchId`; date range needs **both** dates in ISO offset form.
- **Headers:** `Authorization`, `X-Tenant-ID` (production tenant routing), `Content-Type: application/json` for POST/PUT.
- **Token handling:** Stateless JWT; refresh via app auth flow (`/api/v1/auth/refresh` — not part of this module).
- **Pagination:** Prefer client-managed query rebuild for **next/prev** because server URLs may omit some filters.
- **Date display:** Detail response uses **OffsetDateTime**; list uses **Instant** on `CustomerResponse.createdAt` — format consistently in UI (UTC).
- **Two error shapes:** Success uses `ResponseStructure`; failures often use **`ValidationErrorResponse`** (not the same as success wrapper).
- **360 tabs split:** For large customers, load Tab 2 and Tab 3 via dedicated APIs (`/contract-logs`, `/sales-orders-service-history`) for proper pagination instead of relying on the top-50 summaries returned by `GET /by-id`.
- **Excel export:** `/sales-orders-service-history/export-excel` returns binary `.xlsx` bytes. In browser clients set `responseType: 'blob'` and download using filename `service-history.xlsx`.

---

_Document generated from backend: `CustomerController`, `CustomerServiceImpl`, DTOs, enums, `CustomerSpecification`, `SecurityConfig`, `TenantResolverFilter`, `GlobalExceptionHandler`. Items marked **Assumption** or **Missing from backend context** are not fully specified in those sources._
