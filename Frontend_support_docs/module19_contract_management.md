# Module 19 – Contract Management

## Short Description

Module 19 manages **contracts** tied to **GMA sheets** (Module 17): create draft or activate in one step, update while **DRAFT**, amend **ACTIVE** contracts, terminate **ACTIVE** contracts, list/filter with **display status** badges, check **create eligibility** (approved unconsumed GMA), inspect **sales order / billing schedule** (Module 20 links), and **export CSV**. Frontend must enforce **decimal totals** (payment lines sum = total sale value; service sale values sum = total sale value), **GMA approval/consumption** rules, **customer ACTIVE** (Module 18), JWT + **tenant** headers, and two response shapes: **`ResponseStructure`** for JSON APIs and **raw CSV bytes** for export.

---

## Authorization

| Aspect                    | Details                                                                                                                                                                                                      |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Authentication**        | Stateless **JWT** — `Authorization: Bearer <access_token>`. `/api/v1/contracts/**` is not in the public allowlist (`SecurityConfig`); all contract endpoints require authentication.                         |
| **Authorities (typical)** | `CONTRACT_MANAGEMENT_READ`, `CONTRACT_MANAGEMENT_ADD`, `CONTRACT_MANAGEMENT_EDIT`, `CONTRACT_MANAGEMENT_EXPORT` — **or** role **`CEO`** (each `@PreAuthorize` uses `hasRole('CEO') or hasAuthority('...')`). |
| **Per endpoint**          | See API List (READ vs ADD vs EDIT vs EXPORT).                                                                                                                                                                |
| **Tenant**                | **`X-Tenant-ID`** resolves tenant schema via `TenantResolverFilter`; if omitted, default tenant applies. **Assumption:** use correct tenant in production.                                                   |
| **Headers**               | **`Authorization`** (JWT), **`X-Tenant-ID`** (tenant), **`Content-Type: application/json`** for POST/PUT/terminate body. CSV export: response is **`text/csv`** (download).                                  |

---

## Enums Used In This Module

### ContractStatus (persisted)

| Value        | Meaning                                                                                                          | Used In                             |
| ------------ | ---------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| `DRAFT`      | Contract not yet activated; editable via `PUT /update`.                                                          | DB, `ContractDetailResponse.status` |
| `ACTIVE`     | Live contract.                                                                                                   | DB, detail                          |
| `EXPIRED`    | Stored expired state (also **ACTIVE** with `endDate` before today maps to display **EXPIRED** in UI derivation). | DB, derivation                      |
| `TERMINATED` | Formally terminated.                                                                                             | DB, detail                          |

**Frontend notes:** Listing/filtering uses **`ContractDisplayStatus`**, not raw `ContractStatus` alone, for badges (`EXPIRING_SOON` is derived for ACTIVE near end date).

---

### ContractDisplayStatus (UI / filters)

| Value           | Meaning                                                                        | Used In                                                          |
| --------------- | ------------------------------------------------------------------------------ | ---------------------------------------------------------------- |
| `DRAFT`         | Matches `ContractStatus.DRAFT`.                                                | `GET` list filter `displayStatuses`, list/detail `displayStatus` |
| `ACTIVE`        | ACTIVE and end date **after** today + more than 30 days until end.             | Filter, derived on responses                                     |
| `EXPIRING_SOON` | ACTIVE and end date within **30 days** from today (inclusive window per spec). | Filter, derived                                                  |
| `TERMINATED`    | Terminated contract.                                                           | Filter, derived                                                  |
| `EXPIRED`       | Expired (`EXPIRED` status, or ACTIVE with `endDate` before today).             | Filter, derived                                                  |

**Frontend notes:** Pass **multiple** `displayStatuses` query params (e.g. `displayStatuses=ACTIVE&displayStatuses=EXPIRING_SOON`). Server-generated `next` links join values with **comma** in a single param — both styles may appear; prefer following server `next`/`prev URLs` for pagination.

---

### ContractDurationOption

| Value         | Meaning                                       | Used In                          |
| ------------- | --------------------------------------------- | -------------------------------- |
| `SIX_MONTHS`  | Duration bucket.                              | `ContractPayloadRequest`, detail |
| `ONE_YEAR`    |                                               |                                  |
| `TWO_YEARS`   |                                               |                                  |
| `THREE_YEARS` |                                               |                                  |
| `CUSTOM`      | Custom; `startDate`/`endDate` still required. |                                  |

---

### ContractRenewalType

| Value           | Meaning | Used In          |
| --------------- | ------- | ---------------- |
| `AUTO_RENEW`    |         | Payload / detail |
| `MANUAL`        |         |                  |
| `NON_RENEWABLE` |         |                  |

---

### ContractPaymentScheduleType

| Value              | Meaning                                                                        | Used In             |
| ------------------ | ------------------------------------------------------------------------------ | ------------------- |
| `ADVANCE_100`      | 100% advance — requires **`advancePaymentDueDate`** on/before **`startDate`**. | Payload, validation |
| `MONTHLY_POST`     |                                                                                |                     |
| `QUARTERLY_POST`   |                                                                                |                     |
| `HALF_YEARLY_POST` |                                                                                |                     |
| `MILESTONE`        |                                                                                |                     |
| `CUSTOM`           |                                                                                |                     |

---

### ContractInvoicingFrequency

| Value                   | Meaning | Used In         |
| ----------------------- | ------- | --------------- |
| `MONTHLY`               |         | Payload, detail |
| `QUARTERLY`             |         |                 |
| `HALF_YEARLY`           |         |                 |
| `ANNUALLY`              |         |                 |
| `ON_MILESTONE`          |         |                 |
| `ON_SERVICE_COMPLETION` |         |                 |

---

### ContractPreferredTimeSlot

| Value       | Meaning                             | Used In                              |
| ----------- | ----------------------------------- | ------------------------------------ |
| `MORNING`   | Site service scheduling preference. | `ContractSiteServicePayload`, detail |
| `AFTERNOON` |                                     |                                      |
| `EVENING`   |                                     |                                      |

---

### ContractAmendmentReason

| Value              | Meaning                                    | Used In                |
| ------------------ | ------------------------------------------ | ---------------------- |
| `SITE_ADDITION`    |                                            | `ContractAmendRequest` |
| `SERVICE_UPGRADE`  |                                            |                        |
| `VALUE_ADJUSTMENT` |                                            |                        |
| `SCHEDULE_CHANGE`  |                                            |                        |
| `SLA_MODIFICATION` |                                            |                        |
| `OTHER`            | Requires non-blank **`amendmentRemarks`**. |                        |

---

### ContractTerminationReason

| Value                     | Meaning                                     | Used In                    |
| ------------------------- | ------------------------------------------- | -------------------------- |
| `CUSTOMER_RELOCATED`      |                                             | `ContractTerminateRequest` |
| `PAYMENT_DEFAULT`         |                                             |                            |
| `SERVICE_DISSATISFACTION` |                                             |                            |
| `MUTUAL_AGREEMENT`        |                                             |                            |
| `BUSINESS_CLOSURE`        |                                             |                            |
| `OTHER`                   | Requires non-blank **`additionalRemarks`**. |                            |

---

### GmaServiceMode (Module 17 — used on contract site services)

| Value      | Meaning                                                                         | Used In                                  |
| ---------- | ------------------------------------------------------------------------------- | ---------------------------------------- |
| `CONTRACT` | If this mode, **`frequency`** is required (unless validation passes otherwise). | `ContractSiteServicePayload`, validation |
| `ONE_TIME` |                                                                                 |                                          |

---

### GmaFrequency (Module 17)

| Value         | Meaning                                                                       | Used In                     |
| ------------- | ----------------------------------------------------------------------------- | --------------------------- |
| `WEEKLY`      | Drives default **annual visit** count when not `CUSTOM`.                      | Site service payload/detail |
| `FORTNIGHTLY` |                                                                               |                             |
| `MONTHLY`     |                                                                               |                             |
| `QUARTERLY`   |                                                                               |                             |
| `CUSTOM`      | `defaultAnnualFrequency()` returns **0** — manual `annualFrequency` expected. |                             |

---

### GmaSiteCategory / GmaSiteSubCategory (Module 17 — manual sites)

**GmaSiteCategory:** `RESIDENTIAL`, `COMMERCIAL`, `INDUSTRIAL`  
**GmaSiteSubCategory:** `INTERNAL`, `EXTERNAL`

**Frontend notes:** Required for **manual** sites when `gmaSiteId` is null (see validation).

---

## API List

| Method | Endpoint                                 | Purpose                                   | Authorization Required                |
| ------ | ---------------------------------------- | ----------------------------------------- | ------------------------------------- |
| `GET`  | `/api/v1/contracts`                      | Paginated contract list + filters         | `CEO` or `CONTRACT_MANAGEMENT_READ`   |
| `GET`  | `/api/v1/contracts/by-id`                | Contract detail                           | `CEO` or `CONTRACT_MANAGEMENT_READ`   |
| `GET`  | `/api/v1/contracts/create-eligibility`   | Create button gating (approved GMA count) | `CEO` or `CONTRACT_MANAGEMENT_READ`   |
| `POST` | `/api/v1/contracts`                      | Create contract (draft or activate)       | `CEO` or `CONTRACT_MANAGEMENT_ADD`    |
| `PUT`  | `/api/v1/contracts/update`               | Update **DRAFT** only (save or activate)  | `CEO` or `CONTRACT_MANAGEMENT_EDIT`   |
| `PUT`  | `/api/v1/contracts/amend`                | Amend **ACTIVE**                          | `CEO` or `CONTRACT_MANAGEMENT_EDIT`   |
| `POST` | `/api/v1/contracts/terminate`            | Terminate **ACTIVE**                      | `CEO` or `CONTRACT_MANAGEMENT_EDIT`   |
| `GET`  | `/api/v1/contracts/sales-order-schedule` | SO/billing log + aggregates               | `CEO` or `CONTRACT_MANAGEMENT_READ`   |
| `GET`  | `/api/v1/contracts/export/csv`           | Download service/billing CSV              | `CEO` or `CONTRACT_MANAGEMENT_EXPORT` |

**Base URL prefix:** `/api/v1/contracts`

---

## API Details

---

### `GET /api/v1/contracts`

**Purpose**  
Paginated master list with optional **display status**, customer, branch, **date range** (overlap logic), and text **search** (id, customer name, GMA id).

**Authorization**  
Token required; `CONTRACT_MANAGEMENT_READ` or `CEO`.

**Path parameters:** Not applicable.

**Query parameters**

| Field             | Type   | Required | Description                                               | Example      | Allowed values                                                                                   |
| ----------------- | ------ | -------- | --------------------------------------------------------- | ------------ | ------------------------------------------------------------------------------------------------ |
| `pageNo`          | int    | No       | Page index (default **0**)                                | `0`          | ≥ 0                                                                                              |
| `pageSize`        | int    | No       | Page size (default **10**)                                | `20`         | ≥ 1                                                                                              |
| `displayStatuses` | list   | No       | Filter by derived UI statuses                             | —            | `DRAFT`, `ACTIVE`, `EXPIRING_SOON`, `TERMINATED`, `EXPIRED` (repeat param or compatible binding) |
| `customerId`      | string | No       | Exact customer id                                         | `CUST-ABC12` |                                                                                                  |
| `branchId`        | string | No       | Exact branch id                                           | `BR-MUM-01`  |                                                                                                  |
| `dateFrom`        | date   | No       | ISO date (`yyyy-MM-dd`)                                   | `2026-01-01` |                                                                                                  |
| `dateTo`          | date   | No       | ISO date                                                  | `2026-12-31` |                                                                                                  |
| `search`          | string | No       | Case-insensitive contains on id, customerName, gmaSheetId | `CON-2026`   |                                                                                                  |

**Request body:** Not applicable.

**Response**  
`200 OK` — `ResponseStructure< ContractPaginationResponse<ContractListItemResponse> >`

Nested **`data`**: `count`, `next`, `prev`, `data[]` (list items).

---

#### Full response JSON examples

**Success (with results)**

```json
{
  "status": 200,
  "message": "Contracts fetched successfully",
  "data": {
    "count": 2,
    "next": "https://api.example.com/api/v1/contracts?pageSize=10&displayStatuses=ACTIVE,EXPIRING_SOON&pageNo=1",
    "prev": null,
    "data": [
      {
        "id": "CON-2026-0001",
        "customerName": "Acme Industries Pvt Ltd",
        "gmaSheetId": "GMA-2026-0042",
        "totalSaleValue": 1250000.0,
        "startDate": "2026-04-01",
        "endDate": "2027-03-31",
        "displayStatus": "ACTIVE",
        "branchId": "BR-MUM-01",
        "branchName": "Mumbai HQ"
      }
    ]
  }
}
```

**Empty list**

```json
{
  "status": 200,
  "message": "Contracts fetched successfully",
  "data": {
    "count": 0,
    "next": null,
    "prev": null,
    "data": []
  }
}
```

---

#### Exceptions / error cases

| HTTP Status | Reason       | When                | Typical message | Frontend note   |
| ----------- | ------------ | ------------------- | --------------- | --------------- |
| 401         | Unauthorized | Missing/invalid JWT | Auth message    | Login / refresh |
| 403         | Forbidden    | No READ / not CEO   | Access denied   | Hide list       |

---

#### Error response JSON example

```json
{
  "timestamp": "2026-04-13T10:00:00",
  "status": 403,
  "error": "Forbidden",
  "message": "Access Denied: You don't have permission to access this resource",
  "path": "/api/v1/contracts",
  "validationErrors": null
}
```

---

#### cURL

```bash
curl -sS -G "{{baseUrl}}/api/v1/contracts" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=10" \
  --data-urlencode "displayStatuses=ACTIVE" \
  --data-urlencode "displayStatuses=EXPIRING_SOON" \
  --data-urlencode "customerId=CUST-001" \
  --data-urlencode "search=acme" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Accept: application/json"
```

---

### `GET /api/v1/contracts/by-id`

**Purpose**  
Full contract **detail**: header, financial snapshots, sites + services, payment lines with **paymentStatusDisplay**, termination fields.

**Authorization**  
`CONTRACT_MANAGEMENT_READ` or `CEO`.

**Query parameters**

| Field | Type   | Required | Description | Example         |
| ----- | ------ | -------- | ----------- | --------------- |
| `id`  | string | Yes      | Contract id | `CON-2026-0001` |

**Request body:** Not applicable.

**Response**  
`200 OK` — `ResponseStructure<ContractDetailResponse>`

Important fields: **`varianceRequiresApproval`** (total sale vs GMA original &gt; 10%), **`paymentLines[].paymentStatusDisplay`**: `PAID` \| `OVERDUE` \| `DUE` \| `UPCOMING`.

---

#### Full response JSON example

```json
{
  "status": 200,
  "message": "Contract fetched successfully",
  "data": {
    "id": "CON-2026-0001",
    "gmaSheetId": "GMA-2026-0042",
    "customerId": "CUST-A1B2C",
    "customerName": "Acme Industries Pvt Ltd",
    "branchId": "BR-MUM-01",
    "branchName": "Mumbai HQ",
    "status": "ACTIVE",
    "displayStatus": "ACTIVE",
    "durationOption": "ONE_YEAR",
    "startDate": "2026-04-01",
    "endDate": "2027-03-31",
    "totalSaleValue": 1250000.0,
    "gmaOriginalTotalSale": 1200000.0,
    "totalAnnualCostSnapshot": 480000.0,
    "overallGmPercentSnapshot": 42.5,
    "contractReference": "REF-ACME-2026",
    "renewalType": "MANUAL",
    "legalNotes": "Standard MSME terms.",
    "paymentScheduleType": "QUARTERLY_POST",
    "invoicingFrequency": "QUARTERLY",
    "customPaymentDescription": null,
    "advancePaymentDueDate": null,
    "legalSlaRemarks": "48h response SLA.",
    "varianceRequiresApproval": true,
    "terminationEffectiveDate": null,
    "terminationReason": null,
    "terminationRemarks": null,
    "sites": [
      {
        "id": "CS-A1B2C3D4",
        "gmaSiteId": "GSITE-01",
        "siteName": "Andheri Plant",
        "address": "Plot 12, MIDC",
        "city": "Mumbai",
        "state": "Maharashtra",
        "country": "India",
        "googleMapUrl": "https://maps.google.com/?q=...",
        "areaSqft": 15000.0,
        "category": "INDUSTRIAL",
        "subCategory": "INTERNAL",
        "siteTotalCostYear": 200000.0,
        "siteProposedPriceYear": 350000.0,
        "siteGrossMargin": 42.86,
        "siteServicesSaleTotal": 450000.0,
        "services": [
          {
            "id": "CSS-E5F6G7H8",
            "serviceTypeId": "ST-PEST-01",
            "serviceTypeName": "Integrated Pest Management",
            "contractMode": "CONTRACT",
            "frequency": "MONTHLY",
            "annualFrequency": 12,
            "preferredDays": "MON,WED",
            "preferredTimeSlot": "MORNING",
            "technicianTeamId": "TM-NORTH-1",
            "technicianTeamName": "North Team A",
            "serviceSaleValue": 450000.0
          }
        ]
      }
    ],
    "paymentLines": [
      {
        "id": "CPL-11AA22BB",
        "periodLabel": "Q1 FY26",
        "periodDescription": "Apr–Jun",
        "amount": 312500.0,
        "dueDate": "2026-04-10",
        "paid": false,
        "locked": false,
        "paymentStatusDisplay": "DUE"
      }
    ],
    "createdBy": "Jane Doe",
    "createdAt": "2026-03-15T08:00:00Z",
    "updatedBy": "Jane Doe",
    "updatedAt": "2026-04-10T12:30:00Z"
  }
}
```

---

#### Error cases

| HTTP Status | Reason     | Message (typical)         |
| ----------- | ---------- | ------------------------- |
| 404         | Unknown id | `Contract not found: ...` |

#### Error JSON

```json
{
  "timestamp": "2026-04-13T10:05:00",
  "status": 404,
  "error": "Not Found",
  "message": "Contract not found: CON-missing",
  "path": "/api/v1/contracts/by-id",
  "validationErrors": null
}
```

#### cURL

```bash
curl -sS -G "{{baseUrl}}/api/v1/contracts/by-id" \
  --data-urlencode "id=CON-2026-0001" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Accept: application/json"
```

---

### `GET /api/v1/contracts/create-eligibility`

**Purpose**  
Drive **Create Contract** button: enabled when at least one **APPROVED** GMA exists with **`contractConsumed == false`**.

**Authorization**  
`CONTRACT_MANAGEMENT_READ` or `CEO`.

**Path / query:** Not applicable.

**Request body:** Not applicable.

**Response**  
`200 OK` — `ResponseStructure<ContractEligibilityResponse>`

| Field                       | Type            | Description            |
| --------------------------- | --------------- | ---------------------- |
| `createEnabled`             | boolean         | True if can create     |
| `createDisabledTooltip`     | string nullable | Tooltip when disabled  |
| `approvedGmaAvailableCount` | long            | Count of eligible GMAs |

---

#### Full response JSON examples

**Enabled**

```json
{
  "status": 200,
  "message": "Eligibility resolved",
  "data": {
    "createEnabled": true,
    "createDisabledTooltip": null,
    "approvedGmaAvailableCount": 3
  }
}
```

**Disabled**

```json
{
  "status": 200,
  "message": "Eligibility resolved",
  "data": {
    "createEnabled": false,
    "createDisabledTooltip": "An approved GMA is required to create a contract.",
    "approvedGmaAvailableCount": 0
  }
}
```

#### cURL

```bash
curl -sS "{{baseUrl}}/api/v1/contracts/create-eligibility" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Accept: application/json"
```

---

### `POST /api/v1/contracts`

**Purpose**  
Create a new contract: loads GMA by **`gmaSheetId`**, copies customer/branch from GMA, validates business rules, persists sites and payment lines. **`activate: false`** → **DRAFT**; **`activate: true`** → **ACTIVE** and **consumes** GMA (sets `contractConsumed`, links contract id).

**Authorization**  
`CONTRACT_MANAGEMENT_ADD` or `CEO`.

**Request body** — `ContractPayloadRequest` plus nested payloads (see tables below).

**Root `ContractPayloadRequest`**

| Field                      | Type    | Required           | Validation                    | Description                                                                                 |
| -------------------------- | ------- | ------------------ | ----------------------------- | ------------------------------------------------------------------------------------------- |
| `gmaSheetId`               | string  | Yes (service)      | Non-blank                     | GMA id — **required on create** (server)                                                    |
| `durationOption`           | enum    | Yes                | `@NotNull`                    |                                                                                             |
| `startDate`                | date    | Yes                | `@NotNull`                    | Cannot be **before today** on create/update draft                                           |
| `endDate`                  | date    | Yes                | Must be **after** `startDate` |                                                                                             |
| `totalSaleValue`           | decimal | Yes                | `> 0`                         | Must equal sum of **payment lines** and sum of **service sale values** (2 dp scale compare) |
| `contractReference`        | string  | No                 |                               |                                                                                             |
| `renewalType`              | enum    | No                 |                               |                                                                                             |
| `legalNotes`               | string  | No                 |                               |                                                                                             |
| `sites`                    | array   | Yes                | `@NotEmpty`, `@Valid`         | At least one site                                                                           |
| `paymentScheduleType`      | enum    | Yes                | `@NotNull`                    | If `ADVANCE_100`, `advancePaymentDueDate` required, ≤ `startDate`                           |
| `invoicingFrequency`       | enum    | Yes                | `@NotNull`                    |                                                                                             |
| `customPaymentDescription` | string  | No                 |                               |                                                                                             |
| `advancePaymentDueDate`    | date    | Conditional        |                               |                                                                                             |
| `legalSlaRemarks`          | string  | No                 |                               |                                                                                             |
| `paymentLines`             | array   | Yes                | `@NotEmpty`, `@Valid`         | Sum of `amount` = `totalSaleValue`                                                          |
| `activate`                 | boolean | No (default false) |                               | `true` activates and consumes GMA                                                           |

**`ContractSitePayload`** (each item)

| Field                      | Type    | Required                | Notes                                                                            |
| -------------------------- | ------- | ----------------------- | -------------------------------------------------------------------------------- |
| `gmaSiteId`                | string  | No                      | If set, site fields pulled from GMA; must exist on GMA                           |
| `siteName` … `subCategory` | various | If **`gmaSiteId` null** | Manual site: **siteName, city, state, category, subCategory, areaSqft** required |
| `services`                 | array   | Yes                     | `@NotEmpty`                                                                      |

**`ContractSiteServicePayload`**

| Field                                    | Type             | Required                                          |
| ---------------------------------------- | ---------------- | ------------------------------------------------- |
| `serviceTypeId`, `serviceTypeName`       | string           | `@NotBlank`                                       |
| `contractMode`                           | `GmaServiceMode` | `@NotNull`; if **CONTRACT**, `frequency` required |
| `frequency`                              | `GmaFrequency`   | Required for CONTRACT mode (server)               |
| `annualFrequency`                        | int              | Sent; overwritten from frequency unless `CUSTOM`  |
| `preferredDays`                          | string           | Optional (e.g. `MON,WED`)                         |
| `preferredTimeSlot`                      | enum             | `@NotNull`                                        |
| `technicianTeamId`, `technicianTeamName` | string           | `@NotBlank`                                       |
| `serviceSaleValue`                       | decimal          | `@NotNull`                                        |

**`ContractPaymentLinePayload`**

| Field                              | Type    | Required   |
| ---------------------------------- | ------- | ---------- |
| `periodLabel`, `periodDescription` | string  | No         |
| `amount`                           | decimal | `@NotNull` |
| `dueDate`                          | date    | No         |
| `paid`, `locked`                   | boolean | No         |

---

#### Full request JSON examples

**Minimal valid draft (single site, one service, two payment lines)**

```json
{
  "gmaSheetId": "GMA-2026-0042",
  "durationOption": "ONE_YEAR",
  "startDate": "2026-05-01",
  "endDate": "2027-04-30",
  "totalSaleValue": 120000.0,
  "contractReference": "MIN-2026-001",
  "renewalType": "MANUAL",
  "paymentScheduleType": "MILESTONE",
  "invoicingFrequency": "ON_MILESTONE",
  "sites": [
    {
      "gmaSiteId": "GSITE-01",
      "services": [
        {
          "serviceTypeId": "ST-01",
          "serviceTypeName": "General Service",
          "contractMode": "ONE_TIME",
          "annualFrequency": 1,
          "preferredTimeSlot": "AFTERNOON",
          "technicianTeamId": "TM-1",
          "technicianTeamName": "Team One",
          "serviceSaleValue": 120000.0
        }
      ]
    }
  ],
  "paymentLines": [
    { "periodLabel": "M1", "amount": 60000.0, "dueDate": "2026-05-01" },
    { "periodLabel": "M2", "amount": 60000.0, "dueDate": "2026-11-01" }
  ],
  "activate": false
}
```

**Complete request (activate with CONTRACT service + quarterly payments)**

```json
{
  "gmaSheetId": "GMA-2026-0042",
  "durationOption": "CUSTOM",
  "startDate": "2026-06-01",
  "endDate": "2028-05-31",
  "totalSaleValue": 2400000.0,
  "contractReference": "REF-FULL-2026",
  "renewalType": "AUTO_RENEW",
  "legalNotes": "Includes addendum A.",
  "paymentScheduleType": "QUARTERLY_POST",
  "invoicingFrequency": "QUARTERLY",
  "customPaymentDescription": null,
  "advancePaymentDueDate": null,
  "legalSlaRemarks": "Critical SLA for pharma zone.",
  "sites": [
    {
      "gmaSiteId": "GSITE-01",
      "services": [
        {
          "serviceTypeId": "ST-PEST-01",
          "serviceTypeName": "IPM Contract",
          "contractMode": "CONTRACT",
          "frequency": "MONTHLY",
          "annualFrequency": 12,
          "preferredDays": "TUE,THU",
          "preferredTimeSlot": "MORNING",
          "technicianTeamId": "TM-N01",
          "technicianTeamName": "North 01",
          "serviceSaleValue": 2400000.0
        }
      ]
    }
  ],
  "paymentLines": [
    {
      "periodLabel": "Q1",
      "periodDescription": "Jun–Aug",
      "amount": 600000.0,
      "dueDate": "2026-06-05",
      "paid": false,
      "locked": false
    },
    { "periodLabel": "Q2", "amount": 600000.0, "dueDate": "2026-09-05" },
    { "periodLabel": "Q3", "amount": 600000.0, "dueDate": "2026-12-05" },
    { "periodLabel": "Q4", "amount": 600000.0, "dueDate": "2027-03-05" }
  ],
  "activate": true
}
```

**ADVANCE_100 payment schedule**

```json
{
  "gmaSheetId": "GMA-2026-0099",
  "durationOption": "SIX_MONTHS",
  "startDate": "2026-06-15",
  "endDate": "2026-12-14",
  "totalSaleValue": 500000.0,
  "paymentScheduleType": "ADVANCE_100",
  "invoicingFrequency": "ON_MILESTONE",
  "advancePaymentDueDate": "2026-06-10",
  "sites": [
    {
      "gmaSiteId": "GSITE-02",
      "services": [
        {
          "serviceTypeId": "ST-02",
          "serviceTypeName": "One-time sanitization",
          "contractMode": "ONE_TIME",
          "annualFrequency": 1,
          "preferredTimeSlot": "EVENING",
          "technicianTeamId": "TM-2",
          "technicianTeamName": "Team Two",
          "serviceSaleValue": 500000.0
        }
      ]
    }
  ],
  "paymentLines": [
    {
      "periodLabel": "100% Advance",
      "amount": 500000.0,
      "dueDate": "2026-06-10"
    }
  ],
  "activate": false
}
```

**Manual site (no gmaSiteId)**

```json
{
  "gmaSheetId": "GMA-2026-0042",
  "durationOption": "ONE_YEAR",
  "startDate": "2026-07-01",
  "endDate": "2027-06-30",
  "totalSaleValue": 90000.0,
  "paymentScheduleType": "MILESTONE",
  "invoicingFrequency": "ON_MILESTONE",
  "sites": [
    {
      "siteName": "New Warehouse",
      "address": "Plot 5",
      "city": "Pune",
      "state": "Maharashtra",
      "country": "India",
      "googleMapUrl": "https://maps.google.com/",
      "areaSqft": 8000.0,
      "category": "INDUSTRIAL",
      "subCategory": "EXTERNAL",
      "services": [
        {
          "serviceTypeId": "ST-03",
          "serviceTypeName": "Rodent control",
          "contractMode": "CONTRACT",
          "frequency": "QUARTERLY",
          "annualFrequency": 4,
          "preferredTimeSlot": "AFTERNOON",
          "technicianTeamId": "TM-3",
          "technicianTeamName": "Team Three",
          "serviceSaleValue": 90000.0
        }
      ]
    }
  ],
  "paymentLines": [
    { "periodLabel": "Full", "amount": 90000.0, "dueDate": "2026-07-01" }
  ],
  "activate": false
}
```

---

#### Response

`201 Created` — `ResponseStructure<ContractDetailResponse>` (same shape as **by-id** success).

---

#### Full response JSON example (abbreviated)

```json
{
  "status": 201,
  "message": "Contract created",
  "data": {
    "id": "CON-2026-0007",
    "gmaSheetId": "GMA-2026-0042",
    "status": "DRAFT",
    "displayStatus": "DRAFT",
    "totalSaleValue": 120000.0,
    "varianceRequiresApproval": false,
    "sites": [],
    "paymentLines": []
  }
}
```

_(Real responses include full sites/payment lines — mirrors `getById`.)_

---

#### Exceptions / error cases

| HTTP Status | Reason                               | Typical message                                                  |
| ----------- | ------------------------------------ | ---------------------------------------------------------------- |
| 400         | Missing gmaSheetId                   | `gmaSheetId is required`                                         |
| 400         | GMA not APPROVED / consumed          | `GMA sheet must be APPROVED...` / `already consumed`             |
| 400         | Customer not ACTIVE / missing        | `Customer must be ACTIVE in Module 18` / GMA has no customer     |
| 404         | Customer not found                   | `Customer not found: ...`                                        |
| 409         | Duplicate draft/active for GMA       | `A draft or active contract already exists for this GMA sheet`   |
| 409         | Activate but GMA consumed            | `GMA sheet is already consumed by another contract`              |
| 400         | Date / totals / payment / site rules | Various `ApiBaseException` messages from `validateBusinessRules` |
| 400         | Unknown GMA site                     | `Unknown GMA site id: ...`                                       |

#### Error JSON examples

**Conflict — existing contract for GMA**

```json
{
  "timestamp": "2026-04-13T11:00:00",
  "status": 409,
  "error": "Conflict",
  "message": "A draft or active contract already exists for this GMA sheet",
  "path": "/api/v1/contracts",
  "validationErrors": null
}
```

**Business rule — sums mismatch**

```json
{
  "timestamp": "2026-04-13T11:01:00",
  "status": 400,
  "error": "Bad Request",
  "message": "Sum of payment lines must equal total sale value",
  "path": "/api/v1/contracts",
  "validationErrors": null
}
```

**Validation (Bean Validation)**

```json
{
  "timestamp": "2026-04-13T11:02:00",
  "status": 400,
  "error": "Bad Request",
  "message": "Input validation failed",
  "path": "/api/v1/contracts",
  "validationErrors": {
    "sites": "must not be empty",
    "totalSaleValue": "must not be null"
  }
}
```

#### cURL

```bash
curl -sS -X POST "{{baseUrl}}/api/v1/contracts" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Content-Type: application/json" \
  -d @contract-create.json
```

---

### `PUT /api/v1/contracts/update`

**Purpose**  
Update only **`DRAFT`** contracts. Same payload shape as create; can **`activate: true`** once to go ACTIVE and consume GMA.

**Authorization**  
`CONTRACT_MANAGEMENT_EDIT` or `CEO`.

**Query parameters**

| Field | Type   | Required | Description | Example         |
| ----- | ------ | -------- | ----------- | --------------- |
| `id`  | string | Yes      | Contract id | `CON-2026-0007` |

**Request body:** `ContractPayloadRequest` — same structure as **POST**. **`gmaSheetId`** may still be sent but the contract is already bound to GMA in DB. Include **`activate: true`** to activate the draft (consumes GMA if not already consumed).

---

#### Full request JSON example (update draft — save)

```json
{
  "gmaSheetId": "GMA-2026-0042",
  "durationOption": "ONE_YEAR",
  "startDate": "2026-05-01",
  "endDate": "2027-04-30",
  "totalSaleValue": 120000.0,
  "paymentScheduleType": "MILESTONE",
  "invoicingFrequency": "ON_MILESTONE",
  "sites": [
    {
      "gmaSiteId": "GSITE-01",
      "services": [
        {
          "serviceTypeId": "ST-01",
          "serviceTypeName": "General Service",
          "contractMode": "ONE_TIME",
          "annualFrequency": 1,
          "preferredTimeSlot": "AFTERNOON",
          "technicianTeamId": "TM-1",
          "technicianTeamName": "Team One",
          "serviceSaleValue": 120000.0
        }
      ]
    }
  ],
  "paymentLines": [
    { "periodLabel": "M1", "amount": 60000.0, "dueDate": "2026-05-01" },
    { "periodLabel": "M2", "amount": 60000.0, "dueDate": "2026-11-01" }
  ],
  "activate": false
}
```

#### Activate from draft (same endpoint, `activate: true`)

Use same JSON structure with `"activate": true` and valid GMA consumption preconditions.

---

#### Response

`200 OK` — `ResponseStructure<ContractDetailResponse>`.

---

#### Error cases

| HTTP Status | Reason                   | Message                                                 |
| ----------- | ------------------------ | ------------------------------------------------------- |
| 400         | Not DRAFT                | `Only DRAFT contracts can be updated via this endpoint` |
| 404         | Bad id                   | Contract not found                                      |
| 409         | GMA consumed on activate | `GMA sheet is already consumed`                         |

#### Error JSON

```json
{
  "status": 400,
  "error": "Bad Request",
  "message": "Only DRAFT contracts can be updated via this endpoint",
  "path": "/api/v1/contracts/update",
  "validationErrors": null
}
```

#### cURL

```bash
curl -sS -X PUT "{{baseUrl}}/api/v1/contracts/update?id=CON-2026-0007" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Content-Type: application/json" \
  -d @contract-update.json
```

---

### `PUT /api/v1/contracts/amend`

**Purpose**  
Amend **ACTIVE** contracts. Requires **`amendmentReason`**; if **`OTHER`**, **`amendmentRemarks`** required. **`startDate`** in payload **must equal** existing contract start date. Uses same **`ContractPayloadRequest`** nested rules with **`enforceStartNotInPast = false`** for validation (dates in past allowed for amendment **Assumption**: only start unchanged). **Server does not process `activate`** flag for amend (not read in `amend()`).

**Authorization**  
`CONTRACT_MANAGEMENT_EDIT` or `CEO`.

**Query**

| Field | Type   | Required |
| ----- | ------ | -------- |
| `id`  | string | Yes      |

**Request body — `ContractAmendRequest`**

| Field              | Type                   | Required                        |
| ------------------ | ---------------------- | ------------------------------- |
| `payload`          | ContractPayloadRequest | Yes (`@NotNull`, `@Valid`)      |
| `amendmentReason`  | enum                   | Yes                             |
| `amendmentRemarks` | string                 | Required if reason is **OTHER** |

---

#### Full request JSON example

```json
{
  "amendmentReason": "VALUE_ADJUSTMENT",
  "amendmentRemarks": null,
  "payload": {
    "durationOption": "CUSTOM",
    "startDate": "2026-04-01",
    "endDate": "2027-03-31",
    "totalSaleValue": 1300000.0,
    "paymentScheduleType": "QUARTERLY_POST",
    "invoicingFrequency": "QUARTERLY",
    "sites": [
      {
        "gmaSiteId": "GSITE-01",
        "services": [
          {
            "serviceTypeId": "ST-PEST-01",
            "serviceTypeName": "IPM",
            "contractMode": "CONTRACT",
            "frequency": "MONTHLY",
            "annualFrequency": 12,
            "preferredTimeSlot": "MORNING",
            "technicianTeamId": "TM-N01",
            "technicianTeamName": "North 01",
            "serviceSaleValue": 1300000.0
          }
        ]
      }
    ],
    "paymentLines": [
      { "periodLabel": "Q1", "amount": 325000.0, "dueDate": "2026-04-10" },
      { "periodLabel": "Q2", "amount": 325000.0, "dueDate": "2026-07-10" },
      { "periodLabel": "Q3", "amount": 325000.0, "dueDate": "2026-10-10" },
      { "periodLabel": "Q4", "amount": 325000.0, "dueDate": "2027-01-10" }
    ],
    "activate": false
  }
}
```

**OTHER reason**

```json
{
  "amendmentReason": "OTHER",
  "amendmentRemarks": "Customer negotiated scope reduction and price.",
  "payload": {
    "durationOption": "ONE_YEAR",
    "startDate": "2026-04-01",
    "endDate": "2027-03-31",
    "totalSaleValue": 1100000.0,
    "paymentScheduleType": "MILESTONE",
    "invoicingFrequency": "ON_MILESTONE",
    "sites": [],
    "paymentLines": []
  }
}
```

_(Example with empty sites/lines will fail `@NotEmpty` — real amend must send full valid payload; above illustrates OTHER reason only.)_

---

#### Response

`200 OK` — `ResponseStructure<ContractDetailResponse>`.

---

#### Error cases

| HTTP Status | Reason                   | Message                                                  |
| ----------- | ------------------------ | -------------------------------------------------------- |
| 400         | Not ACTIVE               | `Only ACTIVE contracts can be amended...`                |
| 400         | Missing reason / remarks | `amendmentReason is required` / remarks for OTHER        |
| 400         | Start date change        | `Start date cannot be changed after contract activation` |
| 404         | Unknown contract         | Contract not found                                       |

---

#### Error JSON

```json
{
  "status": 400,
  "message": "Start date cannot be changed after contract activation",
  "path": "/api/v1/contracts/amend"
}
```

#### cURL

```bash
curl -sS -X PUT "{{baseUrl}}/api/v1/contracts/amend?id=CON-2026-0001" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Content-Type: application/json" \
  -d @contract-amend.json
```

---

### `POST /api/v1/contracts/terminate`

**Purpose**  
Set contract to **TERMINATED** with effective date and reason.

**Authorization**  
`CONTRACT_MANAGEMENT_EDIT` or `CEO`.

**Query**

| Field | Type   | Required |
| ----- | ------ | -------- |
| `id`  | string | Yes      |

**Request body — `ContractTerminateRequest`**

| Field                  | Type   | Required     | Validation                        |
| ---------------------- | ------ | ------------ | --------------------------------- |
| `effectiveClosureDate` | date   | Yes          | ≥ **today**; ≤ contract `endDate` |
| `reasonCode`           | enum   | Yes          |                                   |
| `additionalRemarks`    | string | If **OTHER** | Non-blank                         |

**Request body examples**

**Standard termination**

```json
{
  "effectiveClosureDate": "2026-06-30",
  "reasonCode": "MUTUAL_AGREEMENT",
  "additionalRemarks": "Customer signed exit letter."
}
```

**OTHER reason**

```json
{
  "effectiveClosureDate": "2026-08-15",
  "reasonCode": "OTHER",
  "additionalRemarks": "Detailed narrative per legal."
}
```

---

#### Response

`200 OK` — `ResponseStructure<Void>` with **`data`: null**

```json
{
  "status": 200,
  "message": "Contract terminated",
  "data": null
}
```

---

#### Error cases

| HTTP Status | Reason                | Message                                                    |
| ----------- | --------------------- | ---------------------------------------------------------- |
| 400         | Not ACTIVE            | `Only ACTIVE contracts can be terminated`                  |
| 400         | Closure date in past  | `Effective closure date must be today or later`            |
| 400         | After end date        | `Effective closure date cannot be after contract end date` |
| 400         | OTHER without remarks | `additionalRemarks is required when reason is OTHER`       |
| 404         | Unknown id            | Contract not found                                         |

#### Error JSON

```json
{
  "status": 400,
  "message": "Effective closure date must be today or later",
  "path": "/api/v1/contracts/terminate"
}
```

#### cURL

```bash
curl -sS -X POST "{{baseUrl}}/api/v1/contracts/terminate?id=CON-2026-0001" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Content-Type: application/json" \
  -d '{"effectiveClosureDate":"2026-06-30","reasonCode":"MUTUAL_AGREEMENT"}'
```

---

### `GET /api/v1/contracts/sales-order-schedule`

**Purpose**  
Tab 2 schedule: paginated **sales order links** + **totals** (contract value, raised SO value, remaining, fulfillment %).

**Authorization**  
`CONTRACT_MANAGEMENT_READ` or `CEO`.

**Query**

| Field        | Type   | Required | Example         |
| ------------ | ------ | -------- | --------------- |
| `contractId` | string | Yes      | `CON-2026-0001` |
| `pageNo`     | int    | No (0)   | `0`             |
| `pageSize`   | int    | No (10)  | `10`            |

**Request body:** Not applicable.

**Response**  
`200 OK` — `ResponseStructure<ContractSalesOrderScheduleResponse>`

- **`page`**: `ContractPaginationResponse<ContractSalesOrderRowResponse>`
- **`totalContractValue`**, **`totalSoValueRaised`**, **`remainingToInvoice`**, **`fulfillmentProgressPercent`**

| Row field                                                      | Description               |
| -------------------------------------------------------------- | ------------------------- |
| `salesOrderId`                                                 | Deep-link to Module 20    |
| `salesOrderNumber`, `salesOrderDate`, `periodLabel`, `soValue` |                           |
| `soStatus`, `serviceStatus`                                    | Stored strings from links |

---

#### Full response JSON example

```json
{
  "status": 200,
  "message": "Schedule fetched",
  "data": {
    "page": {
      "count": 5,
      "next": null,
      "prev": null,
      "data": [
        {
          "salesOrderId": "SO-1001",
          "salesOrderNumber": "SO-2026-0042",
          "salesOrderDate": "2026-04-01",
          "periodLabel": "Q1 FY26",
          "soValue": 312500.0,
          "soStatus": "OPEN",
          "serviceStatus": "IN_PROGRESS"
        }
      ]
    },
    "totalContractValue": 1250000.0,
    "totalSoValueRaised": 312500.0,
    "remainingToInvoice": 937500.0,
    "fulfillmentProgressPercent": 0.0
  }
}
```

**Empty page**

```json
{
  "status": 200,
  "message": "Schedule fetched",
  "data": {
    "page": {
      "count": 0,
      "next": null,
      "prev": null,
      "data": []
    },
    "totalContractValue": 500000.0,
    "totalSoValueRaised": 0,
    "remainingToInvoice": 500000.0,
    "fulfillmentProgressPercent": 0.0
  }
}
```

#### cURL

```bash
curl -sS -G "{{baseUrl}}/api/v1/contracts/sales-order-schedule" \
  --data-urlencode "contractId=CON-2026-0001" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=10" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Accept: application/json"
```

---

### `GET /api/v1/contracts/export/csv`

**Purpose**  
Download CSV (UTF-8 with BOM): contract summary row, then site/services, then payment lines.

**Authorization**  
`CONTRACT_MANAGEMENT_EXPORT` or `CEO`.

**Query**

| Field | Type   | Required          |
| ----- | ------ | ----------------- |
| `id`  | string | Yes — contract id |

**Request body:** Not applicable.

**Response**  
`200 OK` — **raw bytes** (not `ResponseStructure`)

- **`Content-Type`:** `text/csv;charset=UTF-8`
- **`Content-Disposition`:** `attachment; filename="contract-service-log.csv"`

**Frontend:** trigger file download; no JSON body.

#### cURL

```bash
curl -sS -G "{{baseUrl}}/api/v1/contracts/export/csv" \
  --data-urlencode "id=CON-2026-0001" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -o contract-service-log.csv
```

---

## Frontend Notes (cross-cutting)

- **Totals:** Client-side calculators should match server: **sum(paymentLines.amount) == totalSaleValue** and **sum(service.serviceSaleValue per contract) == totalSaleValue** (scale 2).
- **Variance:** Show approval UX when **`varianceRequiresApproval`** is true (&gt;10% vs GMA original).
- **GMA:** Create needs **approved, unconsumed** GMA; activating consumes it.
- **Customer:** Must be **ACTIVE** (Module 18).
- **Draft vs activate:** `activate` only meaningful on **create** and **update** (draft).
- **Amend:** Lock **startDate** in UI to current value.
- **Terminate:** Validate closure date vs today and contract end.
- **List filters:** `displayStatuses` supports multi-select; date filter semantics support overlap and open ranges (`ContractSpecification`).
- **Pagination:** Prefer server **`next`**/ **`prev`**, but verify **`displayStatuses`** encoding if customizing.
- **SO tab:** Use `salesOrderId` for navigation to sales order detail.
- **CSV:** Separate content-type handling from JSON APIs.

---

## Validation and Exception Summary

| Field / scenario       | Rule                                                 | Error   | Frontend impact      |
| ---------------------- | ---------------------------------------------------- | ------- | -------------------- |
| `gmaSheetId` on create | Required, unique draft/active per GMA                | 400/409 | Pick eligible GMA    |
| GMA / customer         | APPROVED, not consumed; customer ACTIVE              | 400/404 | Pre-check            |
| Dates                  | end &gt; start; start not past (create/draft update) | 400     | Date pickers         |
| Totals                 | Payments and services sum = total                    | 400     | Real-time validation |
| ADVANCE_100            | advance due date, ≤ start                            | 400     | Conditional fields   |
| CONTRACT service       | frequency required                                   | 400     | Show frequency       |
| Manual site            | required fields if no gmaSiteId                      | 400     | Full manual form     |
| Amend                  | ACTIVE only; start unchanged; OTHER remarks          | 400     |                      |
| Terminate              | ACTIVE; date rules; OTHER remarks                    | 400     |                      |
| Not found              | Contract id                                          | 404     |                      |
| Auth                   | JWT                                                  | 401     |                      |
| Permission             | Authority                                            | 403     | Feature gating       |

---

## Frontend Integration Notes

- **Eligibility API** drives create CTA + tooltip.
- **GMA & branch & customer** come from upstream modules — keep ids consistent.
- **Enums** in JSON as **STRING** names matching Java.
- **READ** for list/detail/schedule/eligibility; **ADD** create; **EDIT** update/amend/terminate; **EXPORT** CSV only.
- **Headers:** always **`Authorization`**, **`X-Tenant-ID`** for tenant DB.
- **Token refresh:** use global auth endpoints (outside this module).

---

_Document sourced from `ContractController`, `ContractServiceImpl`, DTOs, enums, `ContractSpecification`, `ContractDisplayStatusUtils`, `ContractNotFoundException`, and cross-module enums (`gma`). **Assumption** lines are called out in Authorization and amend sections._
