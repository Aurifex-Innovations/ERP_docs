# Module 20 – Sales Order Management

## Short Description

Module 20 manages **Sales Orders (SO)** for three business flows:

- **SERVICE_CONTRACT**: Sales orders generated against an **ACTIVE Contract (Module 19)** for a specific billing period (contract payment line).
- **ONE_TIME_SERVICE**: One-time service sales orders, optionally linked to an **APPROVED GMA sheet (Module 17)** and/or an **ACCEPTED Quotation (Module 16)**.
- **PRODUCT_SALE**: Product sales orders driven by **Inventory Products** and tax/HSN resolution.

For frontend developers, the critical points are:

- Strong **RBAC** (`SALES_ORDER_MANAGEMENT_*`) plus **CEO** role bypass.
- **Tenant routing** via `X-Tenant-ID` + JWT authentication.
- Create/update/cancel business rules: edit locks after execution starts (job cards / challans) or invoicing is linked; contract/GMA/quotation consumption rules; totals recalculated server-side (subTotal, taxes, discounts, grandTotal).
- Two response shapes:
  - **Success** uses `ResponseStructure<T>` (JSON wrapper).
  - **Errors** use `ValidationErrorResponse` (global handler), including field-level validation errors.
- Binary download endpoints return **raw bytes** (PDF) and must be handled as file downloads by the browser.

---

## Authorization

| Aspect                             | Details                                                                                                                                                                                           |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Authentication**                 | Stateless **JWT**. Header: `Authorization: Bearer <access_token>`.                                                                                                                                |
| **Tenant / schema**                | Header: `X-Tenant-ID: <tenant>` resolved by `TenantResolverFilter`. If omitted, defaults to the platform default tenant. **Assumption:** always send `X-Tenant-ID` for tenant users.              |
| **Roles / authorities**            | Every endpoint uses `@PreAuthorize("hasRole('CEO') or hasAuthority('...')")`. CEO role bypasses authority checks.                                                                                 |
| **Required headers**               | `Authorization`, `X-Tenant-ID`, `Accept: application/json`. For POST/PUT: `Content-Type: application/json`.                                                                                       |
| **Access restrictions (business)** | Create/update/cancel have state locks (status, execution counts, invoice linked). Some flows require upstream module status (Customer ACTIVE, Contract ACTIVE, Quotation ACCEPTED, GMA APPROVED). |

### Authority map

| Authority                         | Used for                                                    |
| --------------------------------- | ----------------------------------------------------------- |
| `SALES_ORDER_MANAGEMENT_READ`     | list, by-id, execution-status, eligibility lists, scaffolds |
| `SALES_ORDER_MANAGEMENT_ADD`      | create                                                      |
| `SALES_ORDER_MANAGEMENT_EDIT`     | update, cancel                                              |
| `SALES_ORDER_MANAGEMENT_DOWNLOAD` | export PDF                                                  |

---

## Enums Used In This Module

### SalesOrderType

| Value              | Meaning                                                            | Used In                                                                         |
| ------------------ | ------------------------------------------------------------------ | ------------------------------------------------------------------------------- |
| `SERVICE_CONTRACT` | SO against an ACTIVE contract billing period.                      | Create request (`orderType`), list filter (`orderTypes`), list/detail responses |
| `ONE_TIME_SERVICE` | One-time service SO; may be linked to quotation/GMA or standalone. | Same                                                                            |
| `PRODUCT_SALE`     | Product-only SO (no sites).                                        | Same                                                                            |

**Frontend notes:** Create validation differs by `orderType` (sites vs product lines, required source ids).

---

### SalesOrderStatus

| Value       | Meaning                                                                                     | Used In                                                       |
| ----------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| `DRAFT`     | Not released to operations; editable/cancellable.                                           | Create (saveAsDraft=true), list/detail responses              |
| `OPEN`      | Released to operations; editable/cancellable until execution starts & no invoice.           | Create (saveAsDraft=false), list/detail responses             |
| `FULFILLED` | Auto-set when executed visits reach required visits for all services (service orders only). | Detail response; internal update via `calculateFulfillment()` |
| `BILLED`    | Marked billed (invoicing integration).                                                      | Filter/detail; edits blocked when invoice linked              |
| `CANCELLED` | Cancelled SO; consumption reversed for GMA/Quotation; contract link marked CANCELLED.       | Cancel flow                                                   |

**Frontend notes:** Update/cancel allow only `DRAFT` and `OPEN`. Editing is blocked if job cards/challans exist or invoice linked.

---

### SoDiscountType

| Value        | Meaning                          | Used In                                             |
| ------------ | -------------------------------- | --------------------------------------------------- |
| `NONE`       | No discount.                     | Create/update request; detail response; totals calc |
| `FLAT`       | Flat amount discount.            | Same                                                |
| `PERCENTAGE` | Percentage of subtotal discount. | Same                                                |

**Frontend notes:** Server calculates `discountAmount` and enforces `grandTotal > 0`.

---

### SoPriority

| Value      | Meaning           | Used In                                |
| ---------- | ----------------- | -------------------------------------- |
| `NORMAL`   | Default priority. | Create/update request; detail response |
| `URGENT`   | Higher priority.  | Same                                   |
| `CRITICAL` | Highest priority. | Same                                   |

---

### DeliveryAddressType

| Value             | Meaning                                          | Used In                                    |
| ----------------- | ------------------------------------------------ | ------------------------------------------ |
| `REGISTERED_SITE` | Use an existing site reference (deliverySiteId). | Create/update request; detail response     |
| `CUSTOM`          | Provide full delivery address fields.            | Create request validation for OPEN release |

**Frontend notes:** On create when `saveAsDraft=false` (OPEN), `CUSTOM` requires addressLine1 + city + state + googleMapUrl (server checks “incomplete”).

---

### OneTimeSourceKind

| Value           | Meaning                                     | Used In       |
| --------------- | ------------------------------------------- | ------------- |
| `QUOTATION_GMA` | One-time SO linked to quotation and/or GMA. | Create/detail |
| `STANDALONE`    | One-time SO not linked to quotation/GMA.    | Create/detail |

**Frontend notes:** If `orderType=ONE_TIME_SERVICE`, `oneTimeSource` is required. If `oneTimeSource=QUOTATION_GMA`, at least one of `gmaSheetId` or `quotationId` is required.

---

### SalesOrderCancelReason

| Value                        | Meaning                         | Used In                     |
| ---------------------------- | ------------------------------- | --------------------------- |
| `CUSTOMER_REQUEST`           | Cancelled per customer request. | Cancel request              |
| `DUPLICATE_ORDER`            | Duplicate order.                | Cancel request              |
| `INCORRECT_LINE_ITEMS`       | Line items incorrect.           | Cancel request              |
| `CONTRACT_TERMINATED`        | Contract ended/terminated.      | Cancel request              |
| `SERVICE_NO_LONGER_REQUIRED` | No longer needed.               | Cancel request              |
| `OTHER`                      | Other reason; requires remarks. | Cancel request + validation |

---

### External enums (used indirectly)

These enums are not defined in Module 20, but the API enforces their states:

| Enum                                                                | Where                  | Rule                          |
| ------------------------------------------------------------------- | ---------------------- | ----------------------------- |
| `com.security.rbac.modules.customer.enums.Status`                   | Create                 | Customer must be `ACTIVE`.    |
| `com.security.rbac.modules.contractManagement.enums.ContractStatus` | Contract SO create     | Contract must be `ACTIVE`.    |
| `com.security.rbac.modules.gma.enums.GmaStatus`                     | One-time linked create | GMA must be `APPROVED`.       |
| `com.security.rbac.modules.quotation.enums.QuotationStatus`         | One-time linked create | Quotation must be `ACCEPTED`. |

**Missing from backend context:** their full value lists are in those modules; Module 20 only depends on the states above.

---

## API List

| Method | Endpoint                                   | Purpose                                  | Authorization Required                     |
| ------ | ------------------------------------------ | ---------------------------------------- | ------------------------------------------ |
| `GET`  | `/api/v1/sales-orders`                     | Master list (filters, pagination)        | `CEO` or `SALES_ORDER_MANAGEMENT_READ`     |
| `GET`  | `/api/v1/sales-orders/by-id`               | SO detail (Tab 1)                        | `CEO` or `SALES_ORDER_MANAGEMENT_READ`     |
| `GET`  | `/api/v1/sales-orders/execution-status`    | Execution & delivery status (Tab 2)      | `CEO` or `SALES_ORDER_MANAGEMENT_READ`     |
| `POST` | `/api/v1/sales-orders`                     | Create SO (Draft or Open)                | `CEO` or `SALES_ORDER_MANAGEMENT_ADD`      |
| `PUT`  | `/api/v1/sales-orders/update`              | Edit SO (Draft/Open only; locks apply)   | `CEO` or `SALES_ORDER_MANAGEMENT_EDIT`     |
| `POST` | `/api/v1/sales-orders/cancel`              | Cancel SO (Draft/Open only; locks apply) | `CEO` or `SALES_ORDER_MANAGEMENT_EDIT`     |
| `GET`  | `/api/v1/sales-orders/export-pdf`          | Download SO PDF                          | `CEO` or `SALES_ORDER_MANAGEMENT_DOWNLOAD` |
| `GET`  | `/api/v1/sales-orders/eligible-contracts`  | Eligible contracts + free payment lines  | `CEO` or `SALES_ORDER_MANAGEMENT_READ`     |
| `GET`  | `/api/v1/sales-orders/eligible-gma`        | Eligible approved GMA sheets             | `CEO` or `SALES_ORDER_MANAGEMENT_READ`     |
| `GET`  | `/api/v1/sales-orders/eligible-quotations` | Eligible accepted quotations             | `CEO` or `SALES_ORDER_MANAGEMENT_READ`     |
| `GET`  | `/api/v1/sales-orders/scaffold/contract`   | Prefill from contract                    | `CEO` or `SALES_ORDER_MANAGEMENT_READ`     |
| `GET`  | `/api/v1/sales-orders/scaffold/gma`        | Prefill from GMA                         | `CEO` or `SALES_ORDER_MANAGEMENT_READ`     |
| `GET`  | `/api/v1/sales-orders/scaffold/quotation`  | Prefill from quotation                   | `CEO` or `SALES_ORDER_MANAGEMENT_READ`     |

---

## API Details

> **Response wrapper (success)**: every JSON success response uses:
>
> ```json
> { "status": 200, "message": "...", "data": <payload> }
> ```
>
> **Error shape**: errors use `ValidationErrorResponse`:
>
> ```json
> {
>   "timestamp": "2026-04-13T10:15:30.123",
>   "status": 400,
>   "error": "Bad Request",
>   "message": "Input validation failed",
>   "path": "/api/v1/sales-orders",
>   "validationErrors": { "field": "message" }
> }
> ```

---

### `GET /api/v1/sales-orders`

**Purpose**  
Master list with pagination and filters. Search matches `soNumber`, `customerName`, and `contractId`.

**Authorization**

- Token: Required
- Authority: `SALES_ORDER_MANAGEMENT_READ` or `CEO`
- Tenant: `X-Tenant-ID` required for tenant routing

**Path Parameters**: Not applicable

**Query Parameters**

| Field        | Type   | Required | Description              | Example            | Allowed Values                                         |
| ------------ | ------ | -------- | ------------------------ | ------------------ | ------------------------------------------------------ |
| `orderTypes` | array  | No       | Filter by SO type        | `SERVICE_CONTRACT` | `SERVICE_CONTRACT`, `ONE_TIME_SERVICE`, `PRODUCT_SALE` |
| `statuses`   | array  | No       | Filter by status         | `OPEN`             | `DRAFT`, `OPEN`, `FULFILLED`, `BILLED`, `CANCELLED`    |
| `branchId`   | string | No       | Branch filter            | `BR-1`             | —                                                      |
| `customerId` | string | No       | Customer filter          | `CUST-1`           | —                                                      |
| `dateFrom`   | date   | No       | SO date from (inclusive) | `2026-04-01`       | `yyyy-MM-dd`                                           |
| `dateTo`     | date   | No       | SO date to (inclusive)   | `2026-04-30`       | `yyyy-MM-dd`                                           |
| `search`     | string | No       | Search term              | `SO-2026`          | —                                                      |
| `pageNo`     | int    | No       | Default 0                | `0`                | ≥0                                                     |
| `pageSize`   | int    | No       | Default 10               | `10`               | ≥1                                                     |

**Request Body**: Not applicable

**Response**

- Success: `200 OK`
- `data`: `PaginationResponse<SalesOrderListItemResponse>`

**Full Response JSON Examples**

**Paginated list (with results)**

```json
{
  "status": 200,
  "message": "Sales orders fetched",
  "data": {
    "count": 25,
    "next": "https://api.example.com/api/v1/sales-orders?pageSize=10&statuses=OPEN,DRAFT&pageNo=1",
    "prev": null,
    "data": [
      {
        "id": "SO-7C1A2B3C",
        "soNumber": "SO-2026-0007",
        "customerName": "Acme Industries Pvt Ltd",
        "branchId": "BR-1",
        "branchName": "Mumbai HQ",
        "orderType": "SERVICE_CONTRACT",
        "siteCount": 2,
        "grandTotal": 312500.0,
        "soDate": "2026-04-01",
        "status": "OPEN"
      }
    ]
  }
}
```

**Empty state**

```json
{
  "status": 200,
  "message": "Sales orders fetched",
  "data": {
    "count": 0,
    "next": null,
    "prev": null,
    "data": []
  }
}
```

**Exceptions / Error Cases**

| HTTP Status | Reason       | When It Happens                     | Typical Message         | Frontend Handling Note             |
| ----------- | ------------ | ----------------------------------- | ----------------------- | ---------------------------------- |
| 401         | Unauthorized | Missing/invalid JWT                 | Auth exception message  | Redirect/login                     |
| 403         | Forbidden    | Missing READ authority              | Access denied message   | Hide menu/tab                      |
| 400         | Invalid enum | Bad `orderTypes`/`statuses` strings | Invalid value for field | Ensure dropdown values match enums |

**Error Response JSON Examples**

**Invalid enum**

```json
{
  "timestamp": "2026-04-13T10:00:00",
  "status": 400,
  "error": "Bad Request",
  "message": "Invalid value 'WRONG' for field 'statuses'",
  "path": "/api/v1/sales-orders",
  "validationErrors": null
}
```

**cURL**

```bash
curl -sS -G "{{baseUrl}}/api/v1/sales-orders" \
  --data-urlencode "statuses=OPEN" \
  --data-urlencode "orderTypes=SERVICE_CONTRACT" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=10" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Accept: application/json"
```

---

### `GET /api/v1/sales-orders/by-id`

**Purpose**  
Fetch full Sales Order detail for Tab 1, including totals, sites/services/chemicals, and product lines.

**Authorization**

- Token: Required
- Authority: `SALES_ORDER_MANAGEMENT_READ` or `CEO`

**Query Parameters**

| Field | Type   | Required | Description | Example       |
| ----- | ------ | -------- | ----------- | ------------- |
| `id`  | string | Yes      | SO id       | `SO-7C1A2B3C` |

**Request Body**: Not applicable

**Response**

- Success: `200 OK`
- `data`: `SalesOrderDetailResponse`

**Full Response JSON Examples**

**Service Contract SO detail**

```json
{
  "status": 200,
  "message": "Sales order fetched",
  "data": {
    "id": "SO-7C1A2B3C",
    "soNumber": "SO-2026-0007",
    "orderType": "SERVICE_CONTRACT",
    "status": "OPEN",
    "customerId": "CUST-A1B2C",
    "customerName": "Acme Industries Pvt Ltd",
    "branchId": "BR-1",
    "branchName": "Mumbai HQ",
    "contractId": "CON-2026-0001",
    "contractPaymentLineId": "CPL-11AA22BB",
    "billingPeriodLabel": "Q1 FY26",
    "gmaSheetId": null,
    "quotationId": null,
    "oneTimeSource": null,
    "soDate": "2026-04-01",
    "subTotal": 300000.0,
    "discountType": "PERCENTAGE",
    "discountValue": 5.0,
    "discountAmount": 15000.0,
    "taxTotal": 13500.0,
    "grandTotal": 298500.0,
    "executionNotes": "Call site admin before visit.",
    "deliveryAddressType": "REGISTERED_SITE",
    "deliverySiteId": "SITE-01",
    "deliveryAddressLine1": null,
    "deliveryAddressLine2": null,
    "deliveryCity": null,
    "deliveryState": null,
    "deliveryPincode": null,
    "deliveryCountry": null,
    "deliveryGoogleMapUrl": null,
    "priority": "URGENT",
    "expectedDeliveryDate": "2026-04-10",
    "invoiceLinked": false,
    "jobCardsCount": 0,
    "challansCount": 0,
    "sites": [
      {
        "id": "SOS-AAAA1111",
        "contractSiteId": "CS-A1B2C3D4",
        "siteName": "Andheri Plant",
        "address": "Plot 12, MIDC",
        "city": "Mumbai",
        "state": "Maharashtra",
        "country": "India",
        "googleMapUrl": "https://maps.google.com/?q=...",
        "category": "INDUSTRIAL",
        "subCategory": "INTERNAL",
        "areaSqft": 15000.0,
        "contactPerson": "Site Admin",
        "contactMobile": "9876543210",
        "services": [
          {
            "id": "SOSS-BBBB2222",
            "serviceTypeId": "ST-PEST-01",
            "serviceTypeName": "Integrated Pest Management",
            "visits": 1,
            "executedVisits": 0,
            "unitPrice": 250000.0,
            "sqft": 15000.0,
            "hsnCode": "9985",
            "taxPercent": 18.0,
            "taxAmount": 45000.0,
            "lineTotal": 295000.0
          }
        ],
        "chemicals": [
          {
            "id": "SOSC-CCCC3333",
            "productId": "CHEM-01",
            "productName": "Termiticide A",
            "productCode": "T-A",
            "uom": "LITER",
            "coverageSqft": 5000.0,
            "requiredQty": "3L",
            "unitPrice": 500.0,
            "lineCost": 500.0,
            "hsnCode": "3808"
          }
        ]
      }
    ],
    "productLines": [],
    "createdBy": "Jane Doe",
    "createdAt": "2026-04-01T10:00:00Z",
    "updatedBy": "Jane Doe",
    "updatedAt": "2026-04-01T10:05:00Z"
  }
}
```

**Product Sale SO detail**

```json
{
  "status": 200,
  "message": "Sales order fetched",
  "data": {
    "id": "SO-PROD-0001",
    "soNumber": "SO-2026-0008",
    "orderType": "PRODUCT_SALE",
    "status": "DRAFT",
    "customerId": "CUST-P1",
    "customerName": "Retail Customer",
    "branchId": "BR-1",
    "branchName": "Mumbai HQ",
    "contractId": null,
    "contractPaymentLineId": null,
    "billingPeriodLabel": null,
    "gmaSheetId": null,
    "quotationId": null,
    "oneTimeSource": null,
    "soDate": "2026-04-06",
    "subTotal": 1000.0,
    "discountType": "NONE",
    "discountValue": 0,
    "discountAmount": 0,
    "taxTotal": 180.0,
    "grandTotal": 1180.0,
    "executionNotes": null,
    "deliveryAddressType": "CUSTOM",
    "deliverySiteId": null,
    "deliveryAddressLine1": "Shop 12, Main Road",
    "deliveryAddressLine2": "Near City Mall",
    "deliveryCity": "Mumbai",
    "deliveryState": "Maharashtra",
    "deliveryPincode": "400001",
    "deliveryCountry": "India",
    "deliveryGoogleMapUrl": "https://maps.google.com/?q=...",
    "priority": "NORMAL",
    "expectedDeliveryDate": "2026-04-08",
    "invoiceLinked": false,
    "jobCardsCount": 0,
    "challansCount": 0,
    "sites": [],
    "productLines": [
      {
        "id": "SOP-1111AAAA",
        "productId": "PROD-1",
        "productName": "Sprayer",
        "productCode": "SP-01",
        "uom": "PCS",
        "quantity": 1,
        "unitPrice": 1000.0,
        "hsnCode": "8424",
        "taxPercent": 18.0,
        "taxAmount": 180.0,
        "lineTotal": 1180.0
      }
    ],
    "createdBy": "Jane Doe",
    "createdAt": "2026-04-06T09:00:00Z",
    "updatedBy": "Jane Doe",
    "updatedAt": "2026-04-06T09:00:00Z"
  }
}
```

**Exceptions / Error Cases**

| HTTP Status | Reason                 | When It Happens          | Typical Message              | Frontend Handling Note |
| ----------- | ---------------------- | ------------------------ | ---------------------------- | ---------------------- |
| 404         | Not found              | Unknown SO id            | `Sales order not found: ...` | Show not-found page    |
| 401/403     | Unauthorized/Forbidden | Missing token/permission | Auth / access denied         | Gate route             |

**Error Response JSON Example**

```json
{
  "timestamp": "2026-04-13T10:05:00",
  "status": 404,
  "error": "Not Found",
  "message": "Sales order not found: SO-missing",
  "path": "/api/v1/sales-orders/by-id",
  "validationErrors": null
}
```

**cURL**

```bash
curl -sS -G "{{baseUrl}}/api/v1/sales-orders/by-id" \
  --data-urlencode "id=SO-7C1A2B3C" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}"
```

---

### `GET /api/v1/sales-orders/execution-status`

**Purpose**  
Tab 2: execution and delivery progress. Currently returns empty job cards/challans lists and derived counters; designed for Module 21+ integration.

**Authorization**  
Token required; `SALES_ORDER_MANAGEMENT_READ` or `CEO`.

**Query Parameters**

| Field | Type   | Required | Description | Example       |
| ----- | ------ | -------- | ----------- | ------------- |
| `id`  | string | Yes      | SO id       | `SO-7C1A2B3C` |

**Request Body**: Not applicable

**Response**

- `200 OK` — `SalesOrderExecutionResponse`

**Full Response JSON Examples**

```json
{
  "status": 200,
  "message": "Execution status fetched",
  "data": {
    "jobCards": [],
    "deliveryChallans": [],
    "totalJobCards": 0,
    "completedJobCards": 0,
    "pendingJobCards": 0,
    "totalChallans": 0,
    "dispatchedChallans": 0,
    "pendingChallans": 0,
    "overallProgressPercent": 0
  }
}
```

**Exceptions / Error Cases** (same as by-id for 404; auth/forbidden)

**cURL**

```bash
curl -sS -G "{{baseUrl}}/api/v1/sales-orders/execution-status" \
  --data-urlencode "id=SO-7C1A2B3C" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}"
```

---

### `POST /api/v1/sales-orders`

**Purpose**  
Create/generate a Sales Order as **DRAFT** (`saveAsDraft=true`) or **OPEN** (`saveAsDraft=false`). Server:

- Validates flow-specific rules (`orderType`).
- Ensures **Customer is ACTIVE**.
- Builds sites/services/chemicals or product lines.
- Recalculates totals and enforces `grandTotal > 0`.
- For `SERVICE_CONTRACT` on OPEN: marks contract-sales-order link.
- For OPEN (not draft): validates delivery custom address completeness and marks GMA/quotation as consumed (if linked).

**Authorization**

- Token: Required
- Authority: `SALES_ORDER_MANAGEMENT_ADD` or `CEO`

**Path Parameters**: Not applicable

**Query Parameters**: Not applicable

**Request Body Fields** (`SalesOrderCreateRequest`)

| Field                                          | Type    | Required          | Validation         | Description                                                        | Example             | Allowed Values                                           |
| ---------------------------------------------- | ------- | ----------------- | ------------------ | ------------------------------------------------------------------ | ------------------- | -------------------------------------------------------- |
| `orderType`                                    | enum    | Yes               | `@NotNull`         | SO type                                                            | `PRODUCT_SALE`      | `SERVICE_CONTRACT` / `ONE_TIME_SERVICE` / `PRODUCT_SALE` |
| `saveAsDraft`                                  | boolean | No (default true) | —                  | true=DRAFT, false=OPEN                                             | `false`             | —                                                        |
| `customerId`                                   | string  | Yes               | `@NotBlank`        | Customer id                                                        | `CUST-1`            | —                                                        |
| `branchId`                                     | string  | Yes               | `@NotBlank`        | Branch id                                                          | `BR-1`              | —                                                        |
| `soDate`                                       | date    | Yes               | `@NotNull`         | SO date                                                            | `2026-04-06`        | `yyyy-MM-dd`                                             |
| `contractId`                                   | string  | Conditional       | service validation | Required for `SERVICE_CONTRACT`                                    | `CON-2026-0001`     | —                                                        |
| `contractPaymentLineId`                        | string  | Conditional       | service validation | Required for `SERVICE_CONTRACT`                                    | `CPL-11AA22BB`      | —                                                        |
| `billingPeriodLabel`                           | string  | Conditional       | service validation | Required for `SERVICE_CONTRACT`                                    | `Q1 FY26`           | —                                                        |
| `oneTimeSource`                                | enum    | Conditional       | service validation | Required for `ONE_TIME_SERVICE`                                    | `QUOTATION_GMA`     | `QUOTATION_GMA`, `STANDALONE`                            |
| `gmaSheetId`                                   | string  | Conditional       | service validation | Required when `oneTimeSource=QUOTATION_GMA` and quotationId absent | `GMA-2026-0042`     | —                                                        |
| `quotationId`                                  | string  | Conditional       | service validation | Required when `oneTimeSource=QUOTATION_GMA` and gmaSheetId absent  | `QT-2026-0101`      | —                                                        |
| `discountType`                                 | enum    | Yes               | `@NotNull`         | Discount type                                                      | `NONE`              | `NONE` / `FLAT` / `PERCENTAGE`                           |
| `discountValue`                                | decimal | No                | —                  | Flat amount or percentage                                          | `5`                 | —                                                        |
| `executionNotes`                               | string  | No                | —                  | Ops instructions                                                   | `Call before visit` | —                                                        |
| `deliveryAddressType`                          | enum    | No                | —                  | Delivery address mode                                              | `CUSTOM`            | `REGISTERED_SITE`, `CUSTOM`                              |
| `deliverySiteId`                               | string  | No                | —                  | For REGISTERED_SITE                                                | `SITE-01`           | —                                                        |
| `deliveryAddressLine1`..`deliveryGoogleMapUrl` | strings | Conditional       | OPEN validation    | Required subset for `CUSTOM` on OPEN                               | —                   | —                                                        |
| `priority`                                     | enum    | No                | —                  | Priority                                                           | `URGENT`            | `NORMAL` / `URGENT` / `CRITICAL`                         |
| `expectedDeliveryDate`                         | date    | No                | —                  | Delivery date                                                      | `2026-04-10`        | `yyyy-MM-dd`                                             |
| `sites`                                        | array   | Conditional       | `@Valid`           | Required for service orders; must be empty for product sale        | —                   | —                                                        |
| `productLines`                                 | array   | Conditional       | `@Valid`           | Required for product sale; must be empty for service orders        | —                   | —                                                        |

#### Nested request structures

**`SalesOrderSiteRequest`**

| Field                        | Type    | Required | Validation            | Description               |
| ---------------------------- | ------- | -------- | --------------------- | ------------------------- |
| `siteName`                   | string  | Yes      | `@NotBlank`           | Site name                 |
| `city` / `state` / `country` | string  | Yes      | `@NotBlank`           | Location                  |
| `category` / `subCategory`   | string  | Yes      | `@NotBlank`           | Site category strings     |
| `areaSqft`                   | decimal | Yes      | `@NotNull`            | Area                      |
| `services`                   | array   | Yes      | `@NotNull` + `@Valid` | Service lines             |
| `chemicals`                  | array   | No       | `@Valid`              | Chemical consumption list |

**`SalesOrderSiteServiceRequest`**

| Field                              | Type    | Required | Validation  | Notes                                                      |
| ---------------------------------- | ------- | -------- | ----------- | ---------------------------------------------------------- |
| `serviceTypeId`, `serviceTypeName` | string  | Yes      | `@NotBlank` |                                                            |
| `visits`                           | decimal | Yes      | `@NotNull`  |                                                            |
| `unitPrice`                        | decimal | Yes      | `@NotNull`  |                                                            |
| `sqft`                             | decimal | Yes      | `@NotNull`  |                                                            |
| `hsnCode`                          | string  | No       | —           | If set and `taxPercent` null, tax resolved from HSN master |
| `taxPercent`                       | decimal | No       | —           | Optional override; must be >0 to apply override            |

**`SalesOrderProductLineRequest`**

| Field        | Type    | Required | Validation  | Notes                                                   |
| ------------ | ------- | -------- | ----------- | ------------------------------------------------------- |
| `productId`  | string  | Yes      | `@NotBlank` | Product must exist and be ACTIVE                        |
| `quantity`   | decimal | Yes      | `@NotNull`  |                                                         |
| `unitPrice`  | decimal | Yes      | `@NotNull`  |                                                         |
| `taxPercent` | decimal | No       | —           | Optional override; if absent, resolved from product HSN |

**Important server calculations**  
Server computes:

- For service line: `base = visits * unitPrice`, `taxAmount = base * taxPercent / 100`, `lineTotal = base + taxAmount`
- For product line: `base = quantity * unitPrice`, plus tax similarly
- `subTotal` is sum of bases; discount applied to subtotal; `taxTotal` is sum of taxAmount; `grandTotal = subTotal - discountAmount + taxTotal`

---

### Full Request JSON Examples

#### Minimal Valid Request (PRODUCT_SALE, Draft)

```json
{
  "orderType": "PRODUCT_SALE",
  "saveAsDraft": true,
  "customerId": "CUST-1",
  "branchId": "BR-1",
  "soDate": "2026-04-06",
  "discountType": "NONE",
  "productLines": [
    {
      "productId": "PROD-1",
      "quantity": 1,
      "unitPrice": 1000.0
    }
  ],
  "sites": []
}
```

#### Complete Valid Request (PRODUCT_SALE, Open with custom delivery + discount)

```json
{
  "orderType": "PRODUCT_SALE",
  "saveAsDraft": false,
  "customerId": "CUST-1",
  "branchId": "BR-1",
  "soDate": "2026-04-06",
  "discountType": "PERCENTAGE",
  "discountValue": 5,
  "executionNotes": "Pack fragile items separately.",
  "deliveryAddressType": "CUSTOM",
  "deliveryAddressLine1": "Shop 12, Main Road",
  "deliveryAddressLine2": "Near City Mall",
  "deliveryCity": "Mumbai",
  "deliveryState": "Maharashtra",
  "deliveryPincode": "400001",
  "deliveryCountry": "India",
  "deliveryGoogleMapUrl": "https://maps.google.com/?q=...",
  "priority": "NORMAL",
  "expectedDeliveryDate": "2026-04-08",
  "productLines": [
    {
      "productId": "PROD-1",
      "quantity": 2,
      "unitPrice": 1000.0,
      "taxPercent": 18
    },
    {
      "productId": "PROD-2",
      "quantity": 1,
      "unitPrice": 500.0
    }
  ],
  "sites": []
}
```

#### Service Contract SO (OPEN) – Billing Period Based

```json
{
  "orderType": "SERVICE_CONTRACT",
  "saveAsDraft": false,
  "customerId": "CUST-A1B2C",
  "branchId": "BR-1",
  "soDate": "2026-04-01",
  "contractId": "CON-2026-0001",
  "contractPaymentLineId": "CPL-11AA22BB",
  "billingPeriodLabel": "Q1 FY26",
  "discountType": "NONE",
  "deliveryAddressType": "REGISTERED_SITE",
  "deliverySiteId": "CS-A1B2C3D4",
  "priority": "URGENT",
  "sites": [
    {
      "contractSiteId": "CS-A1B2C3D4",
      "siteName": "Andheri Plant",
      "address": "Plot 12, MIDC",
      "city": "Mumbai",
      "state": "Maharashtra",
      "country": "India",
      "googleMapUrl": "https://maps.google.com/?q=...",
      "category": "INDUSTRIAL",
      "subCategory": "INTERNAL",
      "areaSqft": 15000,
      "contactPerson": "Site Admin",
      "contactMobile": "9876543210",
      "services": [
        {
          "serviceTypeId": "ST-PEST-01",
          "serviceTypeName": "Integrated Pest Management",
          "visits": 1,
          "unitPrice": 250000.0,
          "sqft": 15000,
          "hsnCode": "9985",
          "taxPercent": 18
        }
      ],
      "chemicals": []
    }
  ],
  "productLines": []
}
```

#### One-time Service (linked) – QUOTATION_GMA with quotation only

```json
{
  "orderType": "ONE_TIME_SERVICE",
  "saveAsDraft": false,
  "customerId": "CUST-A1B2C",
  "branchId": "BR-1",
  "soDate": "2026-04-12",
  "oneTimeSource": "QUOTATION_GMA",
  "quotationId": "QT-2026-0101",
  "discountType": "FLAT",
  "discountValue": 5000,
  "deliveryAddressType": "CUSTOM",
  "deliveryAddressLine1": "Customer Location Address",
  "deliveryCity": "Mumbai",
  "deliveryState": "Maharashtra",
  "deliveryGoogleMapUrl": "https://maps.google.com/?q=...",
  "sites": [
    {
      "siteName": "Customer Premise",
      "address": "Block A",
      "city": "Mumbai",
      "state": "Maharashtra",
      "country": "India",
      "googleMapUrl": "https://maps.google.com/?q=...",
      "category": "COMMERCIAL",
      "subCategory": "INTERNAL",
      "areaSqft": 5000,
      "services": [
        {
          "serviceTypeId": "ST-ONE-01",
          "serviceTypeName": "Deep Cleaning",
          "visits": 1,
          "unitPrice": 50000.0,
          "sqft": 5000,
          "hsnCode": "9985"
        }
      ],
      "chemicals": [
        {
          "productName": "Chemical X",
          "productCode": "CHX",
          "uom": "LITER",
          "coverageSqft": 2000,
          "requiredQty": "2L",
          "unitPrice": 300.0,
          "hsnCode": "3808"
        }
      ]
    }
  ],
  "productLines": []
}
```

#### One-time Service (standalone) – STANDALONE, Draft

```json
{
  "orderType": "ONE_TIME_SERVICE",
  "saveAsDraft": true,
  "customerId": "CUST-A1B2C",
  "branchId": "BR-1",
  "soDate": "2026-04-12",
  "oneTimeSource": "STANDALONE",
  "discountType": "NONE",
  "sites": [
    {
      "siteName": "Standalone Site",
      "city": "Pune",
      "state": "Maharashtra",
      "country": "India",
      "category": "COMMERCIAL",
      "subCategory": "EXTERNAL",
      "areaSqft": 3000,
      "services": [
        {
          "serviceTypeId": "ST-ONE-02",
          "serviceTypeName": "Sanitization",
          "visits": 1,
          "unitPrice": 15000.0,
          "sqft": 3000
        }
      ],
      "chemicals": []
    }
  ],
  "productLines": []
}
```

**Request Body: Not applicable** — for endpoints that do not use JSON bodies.

---

### Response

- Success: `201 Created`
- Body: `ResponseStructure<SalesOrderDetailResponse>`

**Full Response JSON Examples**

**Success (created draft)**

```json
{
  "status": 201,
  "message": "Sales order created",
  "data": {
    "id": "SO-7C1A2B3C",
    "soNumber": "SO-2026-0007",
    "orderType": "PRODUCT_SALE",
    "status": "DRAFT",
    "customerId": "CUST-1",
    "customerName": "ABC Corp",
    "branchId": "BR-1",
    "branchName": "Mumbai HQ",
    "contractId": null,
    "contractPaymentLineId": null,
    "billingPeriodLabel": null,
    "gmaSheetId": null,
    "quotationId": null,
    "oneTimeSource": null,
    "soDate": "2026-04-06",
    "subTotal": 1000.0,
    "discountType": "NONE",
    "discountValue": 0,
    "discountAmount": 0,
    "taxTotal": 180.0,
    "grandTotal": 1180.0,
    "executionNotes": null,
    "deliveryAddressType": null,
    "deliverySiteId": null,
    "deliveryAddressLine1": null,
    "deliveryAddressLine2": null,
    "deliveryCity": null,
    "deliveryState": null,
    "deliveryPincode": null,
    "deliveryCountry": null,
    "deliveryGoogleMapUrl": null,
    "priority": null,
    "expectedDeliveryDate": null,
    "invoiceLinked": false,
    "jobCardsCount": 0,
    "challansCount": 0,
    "sites": [],
    "productLines": [
      {
        "id": "SOP-1111AAAA",
        "productId": "PROD-1",
        "productName": "Sprayer",
        "productCode": "SP-01",
        "uom": "PCS",
        "quantity": 1,
        "unitPrice": 1000.0,
        "hsnCode": "8424",
        "taxPercent": 18.0,
        "taxAmount": 180.0,
        "lineTotal": 1180.0
      }
    ],
    "createdBy": "Jane Doe",
    "createdAt": "2026-04-06T09:00:00Z",
    "updatedBy": "Jane Doe",
    "updatedAt": "2026-04-06T09:00:00Z"
  }
}
```

**Exceptions / Error Cases**

| HTTP Status | Reason               | When It Happens                                                            | Typical Message                                                         | Frontend Handling Note                   |
| ----------- | -------------------- | -------------------------------------------------------------------------- | ----------------------------------------------------------------------- | ---------------------------------------- |
| 400         | Validation (bean)    | Missing required fields like `orderType`, `customerId`                     | `Input validation failed`                                               | Show field errors                        |
| 400         | Business rule        | Missing product lines for product sale                                     | `At least one product line is required`                                 | Require at least one line                |
| 400         | Business rule        | Product sale includes sites                                                | `Product sale orders must not include service sites`                    | Hide/disable sites UI                    |
| 400         | Business rule        | Service order missing sites                                                | `At least one site is required`                                         | Require at least one site                |
| 400         | Contract rule        | Missing contract fields                                                    | `contractId, contractPaymentLineId and billingPeriodLabel are required` | Enforce selection via eligible contracts |
| 409         | Conflict             | SO already exists for payment line                                         | `A sales order already exists for this billing period`                  | Disable used payment lines               |
| 400         | One-time rule        | Missing `oneTimeSource`                                                    | `oneTimeSource is required`                                             | Enforce selection                        |
| 400         | One-time linked rule | Missing both gma/quotation                                                 | `gmaSheetId or quotationId is required for linked one-time SO`          | Require at least one                     |
| 404         | Not found            | Customer/Contract/GMA/Quotation not found                                  | `... not found`                                                         | Show upstream missing                    |
| 400/409     | Upstream status      | Customer not ACTIVE / quotation not ACCEPTED / GMA not APPROVED / consumed | Various messages                                                        | Filter eligibility lists / show message  |
| 400         | Totals               | `Grand total must be positive`                                             | Discount or pricing invalid                                             | Prevent invalid discount                 |

**Error Response JSON Examples**

**Bean validation (missing required field)** _(example)_:

```json
{
  "timestamp": "2026-04-13T11:00:00",
  "status": 400,
  "error": "Bad Request",
  "message": "Input validation failed",
  "path": "/api/v1/sales-orders",
  "validationErrors": {
    "customerId": "must not be blank",
    "orderType": "must not be null"
  }
}
```

**Conflict (billing period already used)**:

```json
{
  "timestamp": "2026-04-13T11:01:00",
  "status": 409,
  "error": "Conflict",
  "message": "A sales order already exists for this billing period",
  "path": "/api/v1/sales-orders",
  "validationErrors": null
}
```

**cURL (Postman-friendly)**:

```bash
curl -sS -X POST "{{baseUrl}}/api/v1/sales-orders" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Content-Type: application/json" \
  -d @so-create.json
```

---

### `PUT /api/v1/sales-orders/update`

**Purpose**  
Partial update for **DRAFT/OPEN** SOs: can change discount, notes, delivery fields, priority, expected delivery, and replace sites/product lines. Server recalculates totals.

**Authorization**  
Token required; `SALES_ORDER_MANAGEMENT_EDIT` or `CEO`.

**Query Parameters**

| Field | Type   | Required | Description | Example       |
| ----- | ------ | -------- | ----------- | ------------- |
| `id`  | string | Yes      | SO id       | `SO-7C1A2B3C` |

**Request Body Fields** (`SalesOrderUpdateRequest`)

| Field                  | Type    | Required | Validation | Description                                   | Example            | Allowed Values                   |
| ---------------------- | ------- | -------- | ---------- | --------------------------------------------- | ------------------ | -------------------------------- |
| `discountType`         | enum    | No       | —          | Update discount type                          | `FLAT`             | `NONE` / `FLAT` / `PERCENTAGE`   |
| `discountValue`        | decimal | No       | —          | Discount value                                | `1000`             | —                                |
| `executionNotes`       | string  | No       | —          | Notes                                         | `Handle carefully` | —                                |
| `deliveryAddressType`  | enum    | No       | —          | Delivery mode                                 | `CUSTOM`           | `REGISTERED_SITE` / `CUSTOM`     |
| Delivery fields        | string  | No       | —          | Delivery address                              | —                  | —                                |
| `priority`             | enum    | No       | —          | Priority                                      | `URGENT`           | `NORMAL` / `URGENT` / `CRITICAL` |
| `expectedDeliveryDate` | date    | No       | —          | Expected delivery                             | `2026-04-10`       | `yyyy-MM-dd`                     |
| `sites`                | array   | No       | `@Valid`   | If non-empty, replaces existing sites         | —                  | —                                |
| `productLines`         | array   | No       | `@Valid`   | If non-empty, replaces existing product lines | —                  | —                                |

**Full Request JSON Examples**

**Update discount + delivery fields**

```json
{
  "discountType": "FLAT",
  "discountValue": 1500,
  "executionNotes": "Deliver between 10am-1pm",
  "deliveryAddressType": "CUSTOM",
  "deliveryAddressLine1": "Updated Address Line 1",
  "deliveryCity": "Mumbai",
  "deliveryState": "Maharashtra",
  "deliveryGoogleMapUrl": "https://maps.google.com/?q=...",
  "priority": "URGENT",
  "expectedDeliveryDate": "2026-04-09",
  "sites": [],
  "productLines": []
}
```

**Replace product lines (PRODUCT_SALE)**

```json
{
  "productLines": [
    { "productId": "PROD-1", "quantity": 3, "unitPrice": 950.0 },
    {
      "productId": "PROD-2",
      "quantity": 1,
      "unitPrice": 500.0,
      "taxPercent": 12
    }
  ]
}
```

**Replace sites (service order)**

```json
{
  "sites": [
    {
      "siteName": "Updated Site",
      "city": "Pune",
      "state": "Maharashtra",
      "country": "India",
      "category": "COMMERCIAL",
      "subCategory": "INTERNAL",
      "areaSqft": 2500,
      "services": [
        {
          "serviceTypeId": "ST-ONE-02",
          "serviceTypeName": "Sanitization",
          "visits": 2,
          "unitPrice": 8000.0,
          "sqft": 2500,
          "hsnCode": "9985"
        }
      ],
      "chemicals": []
    }
  ]
}
```

**Response**

- `200 OK` — `ResponseStructure<SalesOrderDetailResponse>`

**Exceptions / Error Cases**

| HTTP Status | Reason             | When It Happens                 | Typical Message                                       | Frontend Handling Note    |
| ----------- | ------------------ | ------------------------------- | ----------------------------------------------------- | ------------------------- |
| 400         | Status restriction | Not DRAFT/OPEN                  | `Only DRAFT or OPEN sales orders can be edited`       | Disable edit buttons      |
| 400         | Execution started  | jobCardsCount/challansCount > 0 | `Execution has started; cannot edit this sales order` | Read-only UI              |
| 400         | Invoice linked     | invoiceLinked true              | `SO is linked to invoicing; cannot edit`              | Read-only UI              |
| 404         | Not found          | Unknown id / product not found  | `Sales order not found...` / `Product not found: ...` | Show error                |
| 400         | Product inactive   | Product not ACTIVE              | `Product must be ACTIVE`                              | Block selection           |
| 400         | Totals invalid     | Grand total <= 0                | `Grand total must be positive`                        | Validate pricing/discount |

**Error Response JSON Example**

```json
{
  "timestamp": "2026-04-13T12:00:00",
  "status": 400,
  "error": "Bad Request",
  "message": "Execution has started; cannot edit this sales order",
  "path": "/api/v1/sales-orders/update",
  "validationErrors": null
}
```

**cURL**

```bash
curl -sS -X PUT "{{baseUrl}}/api/v1/sales-orders/update?id=SO-7C1A2B3C" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Content-Type: application/json" \
  -d @so-update.json
```

---

### `POST /api/v1/sales-orders/cancel`

**Purpose**  
Cancel a DRAFT/OPEN SO. Reverses consumption flags for linked GMA/Quotation and marks contract link status to CANCELLED (if contract SO).

**Authorization**  
Token required; `SALES_ORDER_MANAGEMENT_EDIT` or `CEO`.

**Query Parameters**

| Field | Type   | Required | Description | Example       |
| ----- | ------ | -------- | ----------- | ------------- |
| `id`  | string | Yes      | SO id       | `SO-7C1A2B3C` |

**Request Body Fields** (`SalesOrderCancelRequest`)

| Field     | Type   | Required    | Validation | Description                   | Example                    | Allowed Values               |
| --------- | ------ | ----------- | ---------- | ----------------------------- | -------------------------- | ---------------------------- |
| `reason`  | enum   | Yes         | `@NotNull` | Cancel reason                 | `CUSTOMER_REQUEST`         | See `SalesOrderCancelReason` |
| `remarks` | string | Conditional | —          | Required when reason is OTHER | `Customer merged accounts` | —                            |

**Full Request JSON Examples**

**Standard cancel**

```json
{
  "reason": "CUSTOMER_REQUEST",
  "remarks": "Customer requested cancellation via email."
}
```

**OTHER cancel (remarks required)**

```json
{
  "reason": "OTHER",
  "remarks": "Duplicate created by mistake after refresh."
}
```

**Response**

- `200 OK` — `ResponseStructure<Void>` with `data: null`

```json
{
  "status": 200,
  "message": "Sales order cancelled",
  "data": null
}
```

**Exceptions / Error Cases**

| HTTP Status | Reason                | When It Happens             | Typical Message                                    | Frontend Handling Note |
| ----------- | --------------------- | --------------------------- | -------------------------------------------------- | ---------------------- |
| 400         | Status restriction    | Not DRAFT/OPEN              | `Only DRAFT or OPEN sales orders can be cancelled` | Disable cancel         |
| 400         | Job cards exist       | jobCardsCount > 0           | `Job Cards already generated. Cannot cancel.`      | Hide cancel            |
| 400         | Challans exist        | challansCount > 0           | `Products have been dispatched. Cannot cancel.`    | Hide cancel            |
| 400         | Invoice linked        | invoiceLinked true          | `SO is linked to Invoice...`                       | Route to finance       |
| 400         | OTHER missing remarks | reason OTHER, remarks blank | `remarks required when reason is OTHER`            | Require textarea       |
| 404         | Not found             | Unknown id                  | `Sales order not found: ...`                       | Not-found UI           |

**Error Response JSON Example**

```json
{
  "timestamp": "2026-04-13T12:30:00",
  "status": 400,
  "error": "Bad Request",
  "message": "Job Cards already generated. Cannot cancel.",
  "path": "/api/v1/sales-orders/cancel",
  "validationErrors": null
}
```

**cURL**

```bash
curl -sS -X POST "{{baseUrl}}/api/v1/sales-orders/cancel?id=SO-7C1A2B3C" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Content-Type: application/json" \
  -d '{"reason":"CUSTOMER_REQUEST","remarks":"Customer requested cancellation"}'
```

---

### `GET /api/v1/sales-orders/export-pdf`

**Purpose**  
Download the sales order PDF.

**Authorization**  
Token required; `SALES_ORDER_MANAGEMENT_DOWNLOAD` or `CEO`.

**Query Parameters**

| Field | Type   | Required | Description | Example       |
| ----- | ------ | -------- | ----------- | ------------- |
| `id`  | string | Yes      | SO id       | `SO-7C1A2B3C` |

**Request Body**: Not applicable

**Response**

- `200 OK`
- `Content-Type: application/pdf`
- `Content-Disposition: attachment; filename="sales-order.pdf"`
- Body: **binary PDF bytes**

**Exceptions / Error Cases**: 404 if SO not found; 401/403.

**cURL**

```bash
curl -sS -G "{{baseUrl}}/api/v1/sales-orders/export-pdf" \
  --data-urlencode "id=SO-7C1A2B3C" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -o sales-order.pdf
```

---

### `GET /api/v1/sales-orders/eligible-contracts`

**Purpose**  
Returns ACTIVE contracts that have **at least one payment line** not yet used by a non-cancelled SO. Used to drive Contract SO create wizard.

**Authorization**  
Token required; `SALES_ORDER_MANAGEMENT_READ` or `CEO`.

**Query Parameters**

| Field      | Type   | Required | Description                          | Example | Allowed Values |
| ---------- | ------ | -------- | ------------------------------------ | ------- | -------------- |
| `search`   | string | No       | Matches contract id or customer name | `acme`  | —              |
| `pageNo`   | int    | No       | Default 0                            | `0`     | ≥0             |
| `pageSize` | int    | No       | Default 10                           | `10`    | ≥1             |

**Request Body**: Not applicable

**Response**  
`200 OK` — `PaginationResponse<EligibleContractSummaryResponse>`

**Full Response JSON Examples**

```json
{
  "status": 200,
  "message": "Eligible contracts",
  "data": {
    "count": 1,
    "next": null,
    "prev": null,
    "data": [
      {
        "contractId": "CON-2026-0001",
        "customerName": "Acme Industries Pvt Ltd",
        "branchId": "BR-1",
        "startDate": "2026-04-01",
        "endDate": "2027-03-31",
        "totalSaleValue": 1250000.0,
        "availablePaymentLines": [
          {
            "paymentLineId": "CPL-11AA22BB",
            "periodLabel": "Q1 FY26",
            "amount": 312500.0,
            "dueDate": "2026-04-10"
          }
        ]
      }
    ]
  }
}
```

**Frontend Notes**

- Use `availablePaymentLines[*].paymentLineId` for `contractPaymentLineId`.
- Hide/disable payment lines already used (server filters by existence of non-cancelled SO).

**cURL**

```bash
curl -sS -G "{{baseUrl}}/api/v1/sales-orders/eligible-contracts" \
  --data-urlencode "search=acme" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=10" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}"
```

---

### `GET /api/v1/sales-orders/eligible-gma`

**Purpose**  
Returns APPROVED GMA sheets that are not deleted, not contract-consumed, and not already sales-order-consumed; used to create one-time linked SO.

**Authorization**  
Token required; `SALES_ORDER_MANAGEMENT_READ` or `CEO`.

**Query Parameters**

| Field      | Type   | Required | Description                                                                                    | Example | Allowed Values |
| ---------- | ------ | -------- | ---------------------------------------------------------------------------------------------- | ------- | -------------- |
| `search`   | string | No       | **Missing from backend context:** search is accepted but not applied in service implementation | `acme`  | —              |
| `pageNo`   | int    | No       | Default 0                                                                                      | `0`     | ≥0             |
| `pageSize` | int    | No       | Default 10                                                                                     | `10`    | ≥1             |

**Request Body**: Not applicable

**Response**  
`200 OK` — `PaginationResponse<SalesOrderEligibleGmaResponse>`

**Full Response JSON Example**

```json
{
  "status": 200,
  "message": "Eligible GMA sheets",
  "data": {
    "count": 2,
    "next": null,
    "prev": null,
    "data": [
      {
        "id": "GMA-2026-0042",
        "clientName": "Acme Industries Pvt Ltd",
        "branchId": "BR-1",
        "totalAnnualPrice": 1200000.0,
        "status": "APPROVED"
      }
    ]
  }
}
```

**cURL**

```bash
curl -sS -G "{{baseUrl}}/api/v1/sales-orders/eligible-gma" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=10" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}"
```

---

### `GET /api/v1/sales-orders/eligible-quotations`

**Purpose**  
Returns ACCEPTED quotations that are not deleted and not already consumed by a sales order; used to create one-time linked SO.

**Authorization**  
Token required; `SALES_ORDER_MANAGEMENT_READ` or `CEO`.

**Query Parameters**

| Field      | Type   | Required | Description                                                                                    | Example   | Allowed Values |
| ---------- | ------ | -------- | ---------------------------------------------------------------------------------------------- | --------- | -------------- |
| `search`   | string | No       | **Missing from backend context:** search is accepted but not applied in service implementation | `QT-2026` | —              |
| `pageNo`   | int    | No       | Default 0                                                                                      | `0`       | ≥0             |
| `pageSize` | int    | No       | Default 10                                                                                     | `10`      | ≥1             |

**Request Body**: Not applicable

**Response**  
`200 OK` — `PaginationResponse<SalesOrderEligibleQuotationResponse>`

**Full Response JSON Example**

```json
{
  "status": 200,
  "message": "Eligible quotations",
  "data": {
    "count": 1,
    "next": null,
    "prev": null,
    "data": [
      {
        "id": "QT-2026-0101",
        "quotationNumber": "QTN-2026-0010",
        "clientName": "Acme Industries Pvt Ltd",
        "grandTotal": 75000.0,
        "status": "ACCEPTED"
      }
    ]
  }
}
```

**cURL**

```bash
curl -sS -G "{{baseUrl}}/api/v1/sales-orders/eligible-quotations" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=10" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}"
```

---

### `GET /api/v1/sales-orders/scaffold/contract`

**Purpose**  
Fetches a contract payload directly from Module 19 to prefill the SO create screen.

**Authorization**  
Token required; `SALES_ORDER_MANAGEMENT_READ` or `CEO`.

**Query Parameters**

| Field        | Type   | Required | Description | Example         |
| ------------ | ------ | -------- | ----------- | --------------- |
| `contractId` | string | Yes      | Contract id | `CON-2026-0001` |

**Request Body**: Not applicable

**Response**  
`200 OK` — `ResponseStructure<ContractDetailResponse>`

**Missing from backend context:** full `ContractDetailResponse` fields are defined/maintained in Module 19; see Module 19 docs.

**cURL**

```bash
curl -sS -G "{{baseUrl}}/api/v1/sales-orders/scaffold/contract" \
  --data-urlencode "contractId=CON-2026-0001" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}"
```

---

### `GET /api/v1/sales-orders/scaffold/gma`

**Purpose**  
Fetches GMA detail from Module 17 to prefill one-time SO creation.

**Authorization**  
Token required; `SALES_ORDER_MANAGEMENT_READ` or `CEO`.

**Query Parameters**

| Field        | Type   | Required | Description  | Example         |
| ------------ | ------ | -------- | ------------ | --------------- |
| `gmaSheetId` | string | Yes      | GMA sheet id | `GMA-2026-0042` |

**Request Body**: Not applicable

**Response**  
`200 OK` — `ResponseStructure<GmaSheetDetailResponse>`

**Missing from backend context:** full `GmaSheetDetailResponse` schema is defined in Module 17.

---

### `GET /api/v1/sales-orders/scaffold/quotation`

**Purpose**  
Fetches quotation detail from Module 16 to prefill one-time SO creation.

**Authorization**  
Token required; `SALES_ORDER_MANAGEMENT_READ` or `CEO`.

**Query Parameters**

| Field         | Type   | Required | Description  | Example        |
| ------------- | ------ | -------- | ------------ | -------------- |
| `quotationId` | string | Yes      | Quotation id | `QT-2026-0101` |

**Request Body**: Not applicable

**Response**  
`200 OK` — `ResponseStructure<QuotationDetailResponse>`

**Missing from backend context:** full `QuotationDetailResponse` schema is defined in Module 16.

---

## Validation and Exception Summary

| Field / Scenario                                | Validation / Rule                                                                 | Error Type     | Frontend Impact                  |
| ----------------------------------------------- | --------------------------------------------------------------------------------- | -------------- | -------------------------------- |
| `orderType`, `customerId`, `branchId`, `soDate` | Required (`@NotNull`/`@NotBlank`)                                                 | 400 validation | Required form fields             |
| PRODUCT_SALE requires `productLines`            | Must have ≥1 line; must not include sites                                         | 400 business   | UI: product-only mode            |
| Service orders require `sites`                  | Must have ≥1 site                                                                 | 400 business   | UI: require site section         |
| SERVICE_CONTRACT refs                           | `contractId`, `contractPaymentLineId`, `billingPeriodLabel` required              | 400 business   | Use eligible-contracts selector  |
| Billing period uniqueness                       | No non-cancelled SO for same contract payment line                                | 409 conflict   | Disable used billing periods     |
| ONE_TIME_SERVICE source                         | `oneTimeSource` required; linked requires gma or quotation                        | 400 business   | Conditional UI                   |
| Upstream status rules                           | Customer ACTIVE; Contract ACTIVE; Quotation ACCEPTED; GMA APPROVED and unconsumed | 400/409        | Filter eligibility, show message |
| OPEN release + CUSTOM delivery                  | Must have required custom fields                                                  | 400 business   | Validate before submit           |
| Edit lock                                       | Only DRAFT/OPEN; blocked if execution started or invoice linked                   | 400            | Read-only UI when locked         |
| Cancel lock                                     | Only DRAFT/OPEN; blocked if job cards/challans/invoice linked                     | 400            | Hide cancel when locked          |
| Totals                                          | Grand total must be positive                                                      | 400            | Prevent discount over-subtotal   |
| Enum parse                                      | Invalid enum strings                                                              | 400 malformed  | Use strict enum values           |

---

## Frontend Integration Notes

- **Flow gating**:
  - **Contract SO**: use `/eligible-contracts` → select `contractId` + `paymentLineId` + label.
  - **One-time linked**: use `/eligible-gma` and `/eligible-quotations` to pick allowed sources.
  - **Scaffold** endpoints return upstream module payloads for prefilling; render with their module schemas.
- **Enum rendering**:
  - Use the enum lists above for dropdown options. All enums are serialized as their **name()** strings.
- **Totals**:
  - Backend recalculates totals; frontend can pre-calc for UX but must rely on server response for final numbers.
  - Tax resolution uses HSN master if `taxPercent` absent (service lines) or product HSN (product lines) — show `taxPercent` from response in UI.
- **Status-based UI locks**:
  - Disable edit/cancel when server reports or errors indicate execution started or invoicing linked.
- **Pagination**:
  - `PaginationResponse.next/prev` are absolute URLs built with `app.base-url`. Frontend may either follow them directly or rebuild query state.
- **Downloads**:
  - PDF export returns bytes; handle via `blob` download in browser.

---

_Document generated from backend: `SalesOrderController`, `SalesOrderServiceImpl`, DTOs, enums, `SalesOrderSpecification`, exception `SalesOrderNotFoundException`, and shared utilities (`ResponseStructure`, `PaginationResponse`, `GlobalExceptionHandler`). Items marked **Missing from backend context** or **Assumption** indicate gaps not fully specified in the provided backend sources._
