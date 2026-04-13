# Module 28 – Invoice (Sales / Accounts Receivable)

## Short Description

Module 28 covers **customer sales invoices** for accounts receivable: create and edit **draft** invoices, **approve and send** (status moves to `SENT` and **ledger entries** are posted), **delete** drafts, **list** invoices with pagination, **summary** metrics for dashboards, **credit notes** linked to invoices (manual issue or system-triggered from other modules), **mark overdue** batch job style endpoint, **PDF download** (placeholder bytes), and **Tally export** (placeholder string). A **standalone** credit-note-by-id route exists under `/api/v1/credit-notes`.

**What frontend developers need to know**

- **Base paths:** `POST/GET/PUT/DELETE` under **`/api/v1/invoices`**; credit note read alias under **`/api/v1/credit-notes`**.
- **Authentication:** JWT **`Authorization: Bearer`**. **Tenant:** send **`X-Tenant-ID`** so the request hits the correct schema (standard pattern for this codebase).
- **Success JSON shape:** `ResponseStructure<T>` → `{ "status", "message", "data" }` for almost all endpoints. **Exception:** **`GET /invoices/pdf`** returns **raw PDF bytes** with `Content-Disposition`, **not** JSON.
- **RBAC:** granular authorities `INVOICE_MANAGEMENT_*` (see API list); **`CEO`** role bypasses checks where `@PreAuthorize` allows it.
- **Invoice payload is header-level:** `InvoiceCreateRequest` uses a single **`grandTotal`**; tax line splits on the entity are **not** driven from the request (service sets `subTotal`/`taxableAmount` equal to `grandTotal` — **Assumption:** simplified billing UI).
- **List filters** (`status`, `customerId`, `search`) are **declared on the controller** but **not wired into the service** — only **`pageNo`** / **`pageSize`** affect results (**Missing from backend context** / implementation gap). Implement client-side filtering or fix backend.
- **Approve/send** and **credit note** flows require **configured ledgers** (`SALES_INCOME`, `SALES_ADJUSTMENT`) and an **active customer ledger** — failures return **400** with explicit messages.
- **PDF** and **Tally export** are **stubs** in the current code (placeholder content).

---

## Authorization

### Authentication type

- **Bearer JWT** (`Authorization: Bearer <access_token>`).

### Required token

- **Yes** for all endpoints in this module (Spring Security).

### Required roles / authorities / permissions

| Symbol     | Meaning                       |
| ---------- | ----------------------------- |
| `ADD`      | `INVOICE_MANAGEMENT_ADD`      |
| `READ`     | `INVOICE_MANAGEMENT_READ`     |
| `EDIT`     | `INVOICE_MANAGEMENT_EDIT`     |
| `DELETE`   | `INVOICE_MANAGEMENT_DELETE`   |
| `APPROVE`  | `INVOICE_MANAGEMENT_APPROVE`  |
| `DOWNLOAD` | `INVOICE_MANAGEMENT_DOWNLOAD` |
| `EXPORT`   | `INVOICE_MANAGEMENT_EXPORT`   |
| `CEO`      | Role `CEO`                    |

Exact mapping is in the **API List** table per endpoint. **`CEO`** satisfies `hasRole('CEO')` alongside authority checks in `@PreAuthorize`.

### Required headers

| Header          | Required                                          | Description                                              |
| --------------- | ------------------------------------------------- | -------------------------------------------------------- |
| `Authorization` | Yes                                               | `Bearer <JWT>`                                           |
| `X-Tenant-ID`   | **Assumption:** required for multitenancy routing | Tenant identifier (align with other modules in this app) |
| `Content-Type`  | Yes for JSON bodies                               | `application/json`                                       |

### Tenant / schema-specific behavior

- **Assumption:** Requests are scoped to the tenant schema selected by **`X-Tenant-ID`** (or equivalent filter). Invoice and credit note rows are stored in that tenant’s database. **Missing from backend context:** exact header name if the app uses a different convention — verify against global security config.

### Access restrictions by role / business status

| User / state                   | Effect                                                         |
| ------------------------------ | -------------------------------------------------------------- |
| Without JWT                    | **401 Unauthorized**                                           |
| JWT without required authority | **403 Forbidden**                                              |
| Invoice `DRAFT`                | Update, delete, approve-send allowed (subject to authority)    |
| Invoice not `DRAFT`            | Update/delete/approve-send **rejected** with **400**           |
| Credit note                    | Allowed if invoice exists and `creditAmount` ≤ `pendingAmount` |

---

## Enums Used In This Module

### InvoiceStatus

**Where used:** `SalesInvoice.status`, `InvoiceResponse.status`, business rules in `InvoicingServiceImpl`.

| Value       | Meaning                                                  | Used In                                                                                                                     |
| ----------- | -------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `DRAFT`     | Invoice not yet approved; editable                       | Default on create; required for update/delete/approve                                                                       |
| `SENT`      | Approved and sent; AR recognized                         | After `approve-send`                                                                                                        |
| `PARTIAL`   | Outstanding balance after partial payment or credit note | Set when CN reduces pending but not to zero (from `SENT` or `OVERDUE`)                                                      |
| `PAID`      | No pending balance                                       | After credit note or payments module clears pending                                                                         |
| `OVERDUE`   | Past due date with open balance                          | After `mark-overdue` when conditions met                                                                                    |
| `CANCELLED` | Cancelled invoice                                        | **Assumption:** reserved — **not set** anywhere in `InvoicingServiceImpl` (**Missing from backend context** for cancel API) |

**Frontend notes:** Drive edit/delete buttons only when `status === "DRAFT"`. Show “Approve & send” only for `DRAFT`. After `SENT`/`PARTIAL`/`OVERDUE`, show credit note / payment flows instead of edit.

---

### InvoiceType

**Where used:** `InvoiceCreateRequest.invoiceType`, `InvoiceResponse.invoiceType`, `SalesInvoice.invoiceType`.

| Value      | Meaning     | Frontend notes                |
| ---------- | ----------- | ----------------------------- |
| `TAX`      | Tax invoice | Typical B2B taxable invoice   |
| `PROFORMA` | Proforma    | Non-final / proforma document |

**JSON:** Serialize as string enum, e.g. `"invoiceType": "TAX"`.

---

### Credit note `status` (string, not Java enum)

**Where used:** `CreditNote.status`, `CreditNoteResponse.status`.

| Value    | Meaning              | Used In                      |
| -------- | -------------------- | ---------------------------- |
| `ISSUED` | Credit note recorded | Default on create in service |

**Frontend notes:** Response today is typically `"ISSUED"`. Other values would require future backend changes.

---

### Credit note `source` (entity field; not in API response)

**Where used:** `CreditNote.source` (persisted).

| Value               | Meaning                                          |
| ------------------- | ------------------------------------------------ |
| `MANUAL`            | Issued via `POST /invoices/credit-notes` from UI |
| `AUTO_FROM_PAYMENT` | Issued from Module 30 (voucher settlement)       |

**Missing from backend context:** `CreditNoteResponse` does **not** include `source` — UI cannot distinguish manual vs auto from the REST response without backend extension.

---

## API List

| Method   | Endpoint                              | Purpose                        | Authorization Required |
| -------- | ------------------------------------- | ------------------------------ | ---------------------- |
| `POST`   | `/api/v1/invoices`                    | Create draft invoice           | `ADD` or `CEO`         |
| `PUT`    | `/api/v1/invoices/update`             | Update draft invoice           | `EDIT` or `CEO`        |
| `GET`    | `/api/v1/invoices/by-id`              | Get invoice by id              | `READ` or `CEO`        |
| `DELETE` | `/api/v1/invoices/delete`             | Delete draft invoice           | `DELETE` or `CEO`      |
| `POST`   | `/api/v1/invoices/approve-send`       | Approve and send; post ledger  | `APPROVE` or `CEO`     |
| `GET`    | `/api/v1/invoices`                    | Paginated invoice list         | `READ` or `CEO`        |
| `GET`    | `/api/v1/invoices/summary`            | Dashboard summary              | `READ` or `CEO`        |
| `POST`   | `/api/v1/invoices/credit-notes`       | Issue credit note              | `ADD` or `CEO`         |
| `GET`    | `/api/v1/invoices/credit-notes`       | List credit notes for invoice  | `READ` or `CEO`        |
| `GET`    | `/api/v1/invoices/credit-notes/by-id` | Credit note by id (nested)     | `READ` or `CEO`        |
| `POST`   | `/api/v1/invoices/mark-overdue`       | Mark overdue invoices          | `EDIT` or `CEO`        |
| `GET`    | `/api/v1/invoices/pdf`                | Download invoice PDF           | `DOWNLOAD` or `CEO`    |
| `GET`    | `/api/v1/invoices/export/tally`       | Tally export placeholder       | `EXPORT` or `CEO`      |
| `GET`    | `/api/v1/credit-notes/by-id`          | Credit note by id (standalone) | `READ` or `CEO`        |

---

## API Details

---

### `POST` `/api/v1/invoices`

#### Purpose

Creates a **new draft** invoice with server-generated `id` (`INV-` + UUID fragment) and `invoiceNumber` (`INV-{year}-{seq}`). Sets `receivedAmount = 0`, `pendingAmount = grandTotal`, `status = DRAFT`. Call when the user saves a new invoice in the UI.

#### Authorization

- **Token:** Required.
- **Authority:** `INVOICE_MANAGEMENT_ADD` **or** `CEO`.
- **Tenant:** Same as global rules.

#### Path Parameters

_Not applicable._

#### Query Parameters

_Not applicable._

#### Request Body Fields

| Field              | Type    | Required | Validation  | Description                      | Example                 | Allowed values                            |
| ------------------ | ------- | -------- | ----------- | -------------------------------- | ----------------------- | ----------------------------------------- |
| `invoiceType`      | enum    | Yes      | `@NotNull`  | Tax vs proforma                  | `"TAX"`                 | `TAX`, `PROFORMA`                         |
| `creationMode`     | string  | Yes      | `@NotBlank` | Free-text mode (e.g. UI channel) | `"MANUAL"`              | Any non-blank                             |
| `branchId`         | string  | Yes      | `@NotBlank` | Branch                           | `"BR-MUM-01"`           | —                                         |
| `customerId`       | string  | Yes      | `@NotBlank` | Customer FK                      | `"CUST-0192A1"`         | —                                         |
| `customerName`     | string  | Yes      | `@NotBlank` | Snapshot name                    | `"Acme Pvt Ltd"`        | —                                         |
| `customerGstin`    | string  | No       | —           | GSTIN snapshot                   | `"27AAAAA0000A1Z5"`     | —                                         |
| `billingAddress`   | string  | No       | —           | Full address text                | `"Plot 12, MIDC..."`    | —                                         |
| `customerState`    | string  | No       | —           | State                            | `"Maharashtra"`         | —                                         |
| `contactPerson`    | string  | No       | —           | Contact                          | `"R. Sharma"`           | —                                         |
| `salesOrderId`     | string  | No       | —           | Optional SO link                 | `"SO-2026-0041"`        | —                                         |
| `contractId`       | string  | No       | —           | Optional contract                | `"CTR-88"`              | —                                         |
| `invoiceDate`      | date    | Yes      | `@NotNull`  | Invoice date                     | `"2026-04-13"`          | ISO date                                  |
| `creditPeriodDays` | integer | Yes      | `@NotNull`  | Days until due                   | `30`                    | ≥ 0 **Assumption** (not validated in DTO) |
| `grandTotal`       | decimal | Yes      | `@NotNull`  | Total payable                    | `118000.00`             | —                                         |
| `notes`            | string  | No       | —           | Customer-facing notes            | `"Thanks for business"` | —                                         |
| `internalRemarks`  | string  | No       | —           | Internal only                    | `"Approved by FM"`      | —                                         |

**Computed server-side:** `dueDate = invoiceDate + creditPeriodDays` (see response).

#### Full Request JSON Examples

**Minimal valid request**

```json
{
  "invoiceType": "TAX",
  "creationMode": "MANUAL",
  "branchId": "BR-01",
  "customerId": "CUST-001",
  "customerName": "Test Customer",
  "invoiceDate": "2026-04-13",
  "creditPeriodDays": 30,
  "grandTotal": 10000.0
}
```

**Complete valid request (all optional fields populated)**

```json
{
  "invoiceType": "TAX",
  "creationMode": "WEB_PORTAL",
  "branchId": "BR-MUM-01",
  "customerId": "CUST-0192A1",
  "customerName": "Acme Components Pvt Ltd",
  "customerGstin": "27AAAAA0000A1Z5",
  "billingAddress": "Unit 4, MIDC Industrial Area, Thane 400601",
  "customerState": "Maharashtra",
  "contactPerson": "Ravi Sharma",
  "salesOrderId": "SO-2026-0041",
  "contractId": "CTR-2026-88",
  "invoiceDate": "2026-04-13",
  "creditPeriodDays": 45,
  "grandTotal": 118000.0,
  "notes": "PO reference: PO-7788",
  "internalRemarks": "Margin approved by sales head"
}
```

**Proforma invoice**

```json
{
  "invoiceType": "PROFORMA",
  "creationMode": "MANUAL",
  "branchId": "BR-01",
  "customerId": "CUST-002",
  "customerName": "Beta Traders",
  "invoiceDate": "2026-04-13",
  "creditPeriodDays": 0,
  "grandTotal": 25000.0
}
```

**Zero credit period (due same day)**

```json
{
  "invoiceType": "TAX",
  "creationMode": "MANUAL",
  "branchId": "BR-01",
  "customerId": "CUST-003",
  "customerName": "Cash Customer",
  "invoiceDate": "2026-04-13",
  "creditPeriodDays": 0,
  "grandTotal": 5000.0
}
```

#### Response

- **Success:** **201 Created**
- **Body:** `ResponseStructure<InvoiceResponse>`
- **Important fields:** `status` is `DRAFT`; `pendingAmount` equals `grandTotal`; `receivedAmount` is `0`.

#### Full Response JSON Example

```json
{
  "status": 201,
  "message": "Invoice created",
  "data": {
    "id": "INV-A1B2C3D4",
    "invoiceNumber": "INV-2026-0001",
    "invoiceType": "TAX",
    "creationMode": "MANUAL",
    "status": "DRAFT",
    "invoiceDate": "2026-04-13",
    "dueDate": "2026-05-13",
    "branchId": "BR-01",
    "customerId": "CUST-001",
    "customerName": "Test Customer",
    "salesOrderId": null,
    "grandTotal": 10000.0,
    "receivedAmount": 0,
    "pendingAmount": 10000.0
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason         | When It Happens            | Typical Message                                    | Frontend Handling Note                   |
| ----------- | -------------- | -------------------------- | -------------------------------------------------- | ---------------------------------------- |
| 400         | Validation     | Bean validation on body    | `"Input validation failed"` + field map            | Highlight fields from `validationErrors` |
| 400         | Malformed JSON | Invalid JSON / wrong types | `"Malformed JSON request"` or invalid enum message | Fix payload shape                        |
| 401         | Unauthorized   | Missing/invalid JWT        | Spring message                                     | Redirect to login                        |
| 403         | Forbidden      | Missing authority          | Access denied message                              | Hide actions / request role              |
| 500         | Server         | Uncaught errors            | Runtime message                                    | Generic error UI                         |

#### Error Response JSON Examples

**Validation error (multiple fields)**

```json
{
  "timestamp": "2026-04-13T10:15:30.123",
  "status": 400,
  "error": "Bad Request",
  "message": "Input validation failed",
  "path": "/api/v1/invoices",
  "validationErrors": {
    "customerName": "must not be blank",
    "grandTotal": "must not be null"
  }
}
```

**Invalid enum value for `invoiceType`**

```json
{
  "timestamp": "2026-04-13T10:16:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "Invalid value 'RETAIL' for field 'invoiceType'",
  "path": "/api/v1/invoices",
  "validationErrors": null
}
```

**Unauthorized**

```json
{
  "timestamp": "2026-04-13T10:17:00.000",
  "status": 401,
  "error": "Unauthorized",
  "message": "Full authentication is required to access this resource",
  "path": "/api/v1/invoices",
  "validationErrors": null
}
```

**Forbidden**

```json
{
  "timestamp": "2026-04-13T10:18:00.000",
  "status": 403,
  "error": "Forbidden",
  "message": "Access Denied: You don't have permission to access this resource",
  "path": "/api/v1/invoices",
  "validationErrors": null
}
```

#### Frontend Notes

- After success, store **`id`** for subsequent update/approve/delete.
- **`dueDate`** is returned — display next to invoice date.
- **`creationMode`** is opaque string — use for analytics if needed.

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/invoices" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001" \
  -H "Content-Type: application/json" \
  -d '{
    "invoiceType": "TAX",
    "creationMode": "MANUAL",
    "branchId": "BR-01",
    "customerId": "CUST-001",
    "customerName": "Test Customer",
    "invoiceDate": "2026-04-13",
    "creditPeriodDays": 30,
    "grandTotal": 10000.0
  }'
```

---

### `PUT` `/api/v1/invoices/update`

#### Purpose

Updates an invoice **only while `status` is `DRAFT`**. Reapplies header fields and sets `pendingAmount = grandTotal - receivedAmount` (preserves payments already recorded). Use when the user edits a draft.

#### Authorization

- **Token:** Required.
- **Authority:** `INVOICE_MANAGEMENT_EDIT` **or** `CEO`.

#### Path Parameters

_Not applicable._

#### Query Parameters

| Field | Type   | Required | Description | Example        |
| ----- | ------ | -------- | ----------- | -------------- |
| `id`  | string | Yes      | Invoice id  | `INV-A1B2C3D4` |

#### Request Body Fields

Same as **`POST /api/v1/invoices`** (see table above).

#### Full Request JSON Examples

**Minimal valid update**

```json
{
  "invoiceType": "TAX",
  "creationMode": "MANUAL",
  "branchId": "BR-01",
  "customerId": "CUST-001",
  "customerName": "Test Customer Updated",
  "invoiceDate": "2026-04-13",
  "creditPeriodDays": 30,
  "grandTotal": 12000.0
}
```

**Update after partial receipt (illustrative — pending recalculated)**

If `receivedAmount` was 2000 and new `grandTotal` is 12000, service sets `pendingAmount = 12000 - 2000 = 10000`.

```json
{
  "invoiceType": "TAX",
  "creationMode": "MANUAL",
  "branchId": "BR-01",
  "customerId": "CUST-001",
  "customerName": "Test Customer",
  "invoiceDate": "2026-04-13",
  "creditPeriodDays": 45,
  "grandTotal": 12000.0,
  "notes": "Revised value post negotiation"
}
```

#### Response

- **200 OK**, `ResponseStructure<InvoiceResponse>`.

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Invoice updated",
  "data": {
    "id": "INV-A1B2C3D4",
    "invoiceNumber": "INV-2026-0001",
    "invoiceType": "TAX",
    "creationMode": "MANUAL",
    "status": "DRAFT",
    "invoiceDate": "2026-04-13",
    "dueDate": "2026-05-28",
    "branchId": "BR-01",
    "customerId": "CUST-001",
    "customerName": "Test Customer Updated",
    "salesOrderId": null,
    "grandTotal": 12000.0,
    "receivedAmount": 0,
    "pendingAmount": 12000.0
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason        | When It Happens | Typical Message                     | Frontend Handling Note        |
| ----------- | ------------- | --------------- | ----------------------------------- | ----------------------------- |
| 400         | Business rule | Not draft       | `Only draft invoice can be updated` | Disable edit when not `DRAFT` |
| 404         | Not found     | Bad id          | `Invoice not found`                 | Show 404 page                 |
| 400         | Validation    | Invalid body    | `Input validation failed`           | Same as create                |

#### Error Response JSON Examples

**Not draft**

```json
{
  "timestamp": "2026-04-13T10:20:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "Only draft invoice can be updated",
  "path": "/api/v1/invoices/update",
  "validationErrors": null
}
```

**Invoice not found**

```json
{
  "timestamp": "2026-04-13T10:21:00.000",
  "status": 404,
  "error": "Not Found",
  "message": "Invoice not found",
  "path": "/api/v1/invoices/update",
  "validationErrors": null
}
```

#### Frontend Notes

- Block navigation to edit screen unless status is **`DRAFT`**.
- Changing **`creditPeriodDays`** updates **`dueDate`** on save.

#### cURL

```bash
curl -X PUT "https://{baseUrl}/api/v1/invoices/update?id=INV-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001" \
  -H "Content-Type: application/json" \
  -d '{
    "invoiceType": "TAX",
    "creationMode": "MANUAL",
    "branchId": "BR-01",
    "customerId": "CUST-001",
    "customerName": "Test Customer Updated",
    "invoiceDate": "2026-04-13",
    "creditPeriodDays": 30,
    "grandTotal": 12000.0
  }'
```

---

### `GET` `/api/v1/invoices/by-id`

#### Purpose

Returns a single invoice for detail screens.

#### Authorization

- **Token:** Required.
- **Authority:** `INVOICE_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description | Example        |
| ----- | ------ | -------- | ----------- | -------------- |
| `id`  | string | Yes      | Invoice id  | `INV-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Response

- **200 OK**, `ResponseStructure<InvoiceResponse>`.

#### Full Response JSON Example (sent invoice)

```json
{
  "status": 200,
  "message": "Invoice fetched",
  "data": {
    "id": "INV-A1B2C3D4",
    "invoiceNumber": "INV-2026-0001",
    "invoiceType": "TAX",
    "creationMode": "MANUAL",
    "status": "SENT",
    "invoiceDate": "2026-04-13",
    "dueDate": "2026-05-13",
    "branchId": "BR-01",
    "customerId": "CUST-001",
    "customerName": "Test Customer",
    "salesOrderId": "SO-2026-0041",
    "grandTotal": 100000.0,
    "receivedAmount": 40000.0,
    "pendingAmount": 60000.0
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason     | Typical Message     |
| ----------- | ---------- | ------------------- |
| 404         | Unknown id | `Invoice not found` |

#### Error Response JSON Example

```json
{
  "timestamp": "2026-04-13T10:22:00.000",
  "status": 404,
  "error": "Not Found",
  "message": "Invoice not found",
  "path": "/api/v1/invoices/by-id",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/invoices/by-id?id=INV-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `DELETE` `/api/v1/invoices/delete`

#### Purpose

Deletes an invoice **only in `DRAFT`**. Use for discard draft.

#### Authorization

- **Token:** Required.
- **Authority:** `INVOICE_MANAGEMENT_DELETE` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description | Example        |
| ----- | ------ | -------- | ----------- | -------------- |
| `id`  | string | Yes      | Invoice id  | `INV-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Response

- **200 OK**
- **`data`:** `null` (void)

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Invoice deleted",
  "data": null
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason          | Typical Message                     |
| ----------- | --------------- | ----------------------------------- |
| 400         | Not draft       | `Only draft invoice can be deleted` |
| 404         | Missing invoice | `Invoice not found`                 |

#### Error Response JSON Examples

**Not draft**

```json
{
  "timestamp": "2026-04-13T10:23:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "Only draft invoice can be deleted",
  "path": "/api/v1/invoices/delete",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X DELETE "https://{baseUrl}/api/v1/invoices/delete?id=INV-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `POST` `/api/v1/invoices/approve-send`

#### Purpose

Moves invoice from **`DRAFT` → `SENT`** and posts **double-entry** to **customer ledger** (debit AR) and **`SALES_INCOME`** (credit revenue). Call when finance approves the invoice.

#### Authorization

- **Token:** Required.
- **Authority:** `INVOICE_MANAGEMENT_APPROVE` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description | Example        |
| ----- | ------ | -------- | ----------- | -------------- |
| `id`  | string | Yes      | Invoice id  | `INV-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Response

- **200 OK**, `ResponseStructure<InvoiceResponse>` with `status: "SENT"`.

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Invoice approved and sent",
  "data": {
    "id": "INV-A1B2C3D4",
    "invoiceNumber": "INV-2026-0001",
    "invoiceType": "TAX",
    "creationMode": "MANUAL",
    "status": "SENT",
    "invoiceDate": "2026-04-13",
    "dueDate": "2026-05-13",
    "branchId": "BR-01",
    "customerId": "CUST-001",
    "customerName": "Test Customer",
    "salesOrderId": null,
    "grandTotal": 10000.0,
    "receivedAmount": 0,
    "pendingAmount": 10000.0
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason          | Typical Message                      | Frontend Handling Note             |
| ----------- | --------------- | ------------------------------------ | ---------------------------------- |
| 400         | Not draft       | `Only draft invoice can be approved` | Show only for drafts               |
| 404         | Invoice missing | `Invoice not found`                  | —                                  |
| 400         | Ledger          | `Customer ledger not found/active`   | Fix customer master / ledger setup |
| 400         | Ledger          | `SALES_INCOME ledger not configured` | Admin configures chart of accounts |

#### Error Response JSON Examples

**Ledger configuration**

```json
{
  "timestamp": "2026-04-13T10:24:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "SALES_INCOME ledger not configured",
  "path": "/api/v1/invoices/approve-send",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/invoices/approve-send?id=INV-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/invoices`

#### Purpose

Returns a **paginated** list of invoices (all statuses), sorted by **`updatedAt` DESC**, **`createdAt` DESC`. **Assumption:** Use for main grid; **filters on the controller are ignored** by the service (see **Frontend Notes\*\*).

#### Authorization

- **Token:** Required.
- **Authority:** `INVOICE_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field        | Type   | Required          | Description              | Example    | Allowed values |
| ------------ | ------ | ----------------- | ------------------------ | ---------- | -------------- |
| `pageNo`     | int    | No (default `0`)  | Zero-based page          | `0`        | ≥ 0            |
| `pageSize`   | int    | No (default `10`) | Page size                | `20`       | ≥ 1            |
| `status`     | string | No                | **Declared but ignored** | `SENT`     | —              |
| `customerId` | string | No                | **Declared but ignored** | `CUST-001` | —              |
| `search`     | string | No                | **Declared but ignored** | `INV-2026` | —              |

#### Request Body

**Not applicable.**

#### Response

- **200 OK**, `ResponseStructure<PaginationResponse<InvoiceResponse>>`
- **`count`:** total elements (Long)
- **`next` / `prev`:** absolute URLs built from `app.base-url`
- **`data`:** array of `InvoiceResponse`

#### Full Response JSON Example (page with data)

```json
{
  "status": 200,
  "message": "Invoices fetched",
  "data": {
    "count": 42,
    "next": "https://api.example.com/api/v1/invoices?pageSize=10&pageNo=1",
    "prev": null,
    "data": [
      {
        "id": "INV-A1B2C3D4",
        "invoiceNumber": "INV-2026-0001",
        "invoiceType": "TAX",
        "creationMode": "MANUAL",
        "status": "SENT",
        "invoiceDate": "2026-04-13",
        "dueDate": "2026-05-13",
        "branchId": "BR-01",
        "customerId": "CUST-001",
        "customerName": "Test Customer",
        "salesOrderId": null,
        "grandTotal": 10000.0,
        "receivedAmount": 0,
        "pendingAmount": 10000.0
      }
    ]
  }
}
```

#### Full Response JSON Example (empty page)

```json
{
  "status": 200,
  "message": "Invoices fetched",
  "data": {
    "count": 0,
    "next": null,
    "prev": null,
    "data": []
  }
}
```

#### Exceptions / Error Cases

Standard 401/403. No business errors specific to list.

#### Frontend Notes

- **Search with filters:** Sending `status` / `customerId` / `search` **does not filter** until backend is fixed — apply filters on the client or request API change.
- **Debounce** search if you filter client-side on large lists.
- **Pagination:** Use `next`/`prev` URLs or build from `pageNo`/`pageSize`.

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/invoices?pageNo=0&pageSize=20" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

**Illustrative filtered request (params currently ignored by backend)**

```bash
curl -X GET "https://{baseUrl}/api/v1/invoices?pageNo=0&pageSize=20&status=SENT&customerId=CUST-001&search=INV" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/invoices/summary`

#### Purpose

Aggregates **all invoices** in tenant for dashboard cards: total receivable (pending on open AR), overdue bucket, paid total (sum of `grandTotal` for `PAID`), draft count.

#### Authorization

- **Token:** Required.
- **Authority:** `INVOICE_MANAGEMENT_READ` **or** `CEO`.

#### Request Body

**Not applicable.**

#### Response

- **200 OK**, `ResponseStructure<InvoiceSummaryResponse>`

| Field             | Meaning (from service logic)                                           |
| ----------------- | ---------------------------------------------------------------------- |
| `totalReceivable` | Sum of `pendingAmount` where status is `SENT`, `PARTIAL`, or `OVERDUE` |
| `overdueAmount`   | Sum of `pendingAmount` where status is `OVERDUE`                       |
| `paidAmount`      | Sum of `grandTotal` where status is `PAID`                             |
| `draftCount`      | Count where status is `DRAFT`                                          |

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Invoice summary fetched",
  "data": {
    "totalReceivable": 1250000.5,
    "overdueAmount": 180000.0,
    "paidAmount": 3400000.0,
    "draftCount": 3
  }
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/invoices/summary" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `POST` `/api/v1/invoices/credit-notes`

#### Purpose

Issues a **credit note** against an invoice: reduces `pendingAmount`, may set status to `PAID` or `PARTIAL`, posts ledger entries to **`SALES_ADJUSTMENT`** and **customer ledger**. `source` stored as **`MANUAL`** for this route.

#### Authorization

- **Token:** Required.
- **Authority:** `INVOICE_MANAGEMENT_ADD` **or** `CEO`.

#### Query Parameters

| Field       | Type   | Required | Description    | Example        |
| ----------- | ------ | -------- | -------------- | -------------- |
| `invoiceId` | string | Yes      | Target invoice | `INV-A1B2C3D4` |

#### Request Body Fields

| Field          | Type    | Required | Validation  | Description                       | Example                    |
| -------------- | ------- | -------- | ----------- | --------------------------------- | -------------------------- |
| `cnDate`       | date    | Yes      | `@NotNull`  | Credit note date                  | `"2026-04-13"`             |
| `reason`       | string  | Yes      | `@NotBlank` | Reason code / text                | `"RATE_DIFFERENCE"`        |
| `otherReason`  | string  | No       | —           | Extra reason                      | `"Approved by VP Sales"`   |
| `remarks`      | string  | No       | —           | Notes                             | `"CN against dispute #44"` |
| `creditAmount` | decimal | Yes      | `@NotNull`  | Must be ≤ invoice `pendingAmount` | `5000.0`                   |

#### Full Request JSON Examples

**Minimal valid**

```json
{
  "cnDate": "2026-04-13",
  "reason": "GOODS_RETURN",
  "creditAmount": 5000.0
}
```

**Complete**

```json
{
  "cnDate": "2026-04-13",
  "reason": "OTHER",
  "otherReason": "Post-audit price correction Q1",
  "remarks": "Linked to ticket FD-9921",
  "creditAmount": 12500.5
}
```

**Full pending write-off (sets invoice to PAID if pending matched)**

```json
{
  "cnDate": "2026-04-13",
  "reason": "FULL_WRITEOFF",
  "creditAmount": 60000.0
}
```

**Partial CN leaving balance (invoice becomes PARTIAL if was SENT/OVERDUE)**

```json
{
  "cnDate": "2026-04-13",
  "reason": "PARTIAL_REBATE",
  "creditAmount": 10000.0
}
```

#### Response

- **201 Created**, `ResponseStructure<CreditNoteResponse>`.

#### Full Response JSON Example

```json
{
  "status": 201,
  "message": "Credit note issued",
  "data": {
    "id": "CN-E1F2G3H4",
    "cnNumber": "CN-2026-0001",
    "invoiceId": "INV-A1B2C3D4",
    "cnDate": "2026-04-13",
    "reason": "GOODS_RETURN",
    "creditAmount": 5000.0,
    "status": "ISSUED"
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason  | Typical Message                          |
| ----------- | ------- | ---------------------------------------- |
| 404         | Invoice | `Invoice not found`                      |
| 400         | Amount  | `Credit amount cannot exceed pending`    |
| 400         | Ledger  | `Customer ledger not found/active`       |
| 400         | Ledger  | `SALES_ADJUSTMENT ledger not configured` |

#### Error Response JSON Examples

**Exceeds pending**

```json
{
  "timestamp": "2026-04-13T10:30:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "Credit amount cannot exceed pending",
  "path": "/api/v1/invoices/credit-notes",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/invoices/credit-notes?invoiceId=INV-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001" \
  -H "Content-Type: application/json" \
  -d '{
    "cnDate": "2026-04-13",
    "reason": "GOODS_RETURN",
    "creditAmount": 5000.0
  }'
```

---

### `GET` `/api/v1/invoices/credit-notes`

#### Purpose

Lists credit notes for a given **`invoiceId`**, newest first.

#### Authorization

- **Token:** Required.
- **Authority:** `INVOICE_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field       | Type   | Required | Description | Example        |
| ----------- | ------ | -------- | ----------- | -------------- |
| `invoiceId` | string | Yes      | Invoice id  | `INV-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Response

- **200 OK**, `ResponseStructure<List<CreditNoteResponse>>`

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Credit notes fetched",
  "data": [
    {
      "id": "CN-E1F2G3H4",
      "cnNumber": "CN-2026-0002",
      "invoiceId": "INV-A1B2C3D4",
      "cnDate": "2026-04-15",
      "reason": "RATE_DIFFERENCE",
      "creditAmount": 2000.0,
      "status": "ISSUED"
    },
    {
      "id": "CN-E1F2G3H5",
      "cnNumber": "CN-2026-0001",
      "invoiceId": "INV-A1B2C3D4",
      "cnDate": "2026-04-13",
      "reason": "GOODS_RETURN",
      "creditAmount": 5000.0,
      "status": "ISSUED"
    }
  ]
}
```

#### Empty list example

```json
{
  "status": 200,
  "message": "Credit notes fetched",
  "data": []
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/invoices/credit-notes?invoiceId=INV-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/invoices/credit-notes/by-id`

#### Purpose

Fetches **one** credit note by its **`id`** (nested under invoices path). Payload matches standalone `/api/v1/credit-notes/by-id`.

#### Authorization

- **Token:** Required.
- **Authority:** `INVOICE_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description    | Example       |
| ----- | ------ | -------- | -------------- | ------------- |
| `id`  | string | Yes      | Credit note id | `CN-E1F2G3H4` |

#### Request Body

**Not applicable.**

#### Response

- **200 OK**, `ResponseStructure<CreditNoteResponse>`

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Credit note fetched",
  "data": {
    "id": "CN-E1F2G3H4",
    "cnNumber": "CN-2026-0001",
    "invoiceId": "INV-A1B2C3D4",
    "cnDate": "2026-04-13",
    "reason": "GOODS_RETURN",
    "creditAmount": 5000.0,
    "status": "ISSUED"
  }
}
```

#### Error Response JSON Example

```json
{
  "timestamp": "2026-04-13T10:35:00.000",
  "status": 404,
  "error": "Not Found",
  "message": "Credit note not found",
  "path": "/api/v1/invoices/credit-notes/by-id",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/invoices/credit-notes/by-id?id=CN-E1F2G3H4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `POST` `/api/v1/invoices/mark-overdue`

#### Purpose

Scans **all invoices**: for each with status **`SENT`** or **`PARTIAL`**, positive **`pendingAmount`**, non-null **`dueDate`** before **today**, sets status to **`OVERDUE`**. Returns **count** of rows changed. Use from a scheduled job or admin action.

#### Authorization

- **Token:** Required.
- **Authority:** `INVOICE_MANAGEMENT_EDIT` **or** `CEO`.

#### Request Body

**Not applicable.**

#### Response

- **200 OK**, `ResponseStructure<Integer>` — number of invoices marked overdue.

#### Full Response JSON Examples

**Some invoices updated**

```json
{
  "status": 200,
  "message": "Overdue marking completed",
  "data": 5
}
```

**None eligible**

```json
{
  "status": 200,
  "message": "Overdue marking completed",
  "data": 0
}
```

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/invoices/mark-overdue" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/invoices/pdf`

#### Purpose

Returns a **placeholder PDF** (not a real document). Frontend should still handle **`application/pdf`** download.

#### Authorization

- **Token:** Required.
- **Authority:** `INVOICE_MANAGEMENT_DOWNLOAD` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description | Example        |
| ----- | ------ | -------- | ----------- | -------------- |
| `id`  | string | Yes      | Invoice id  | `INV-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Response

- **200 OK**
- **Content-Type:** `application/pdf`
- **Content-Disposition:** `attachment; filename="invoice-{id}.pdf"`
- **Body:** bytes (placeholder text in current implementation)

**Not** wrapped in `ResponseStructure`.

#### Exceptions

Standard 401/403/404 if extended — **Assumption:** current code does not validate invoice existence before returning placeholder.

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/invoices/pdf?id=INV-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001" \
  -o invoice.pdf
```

---

### `GET` `/api/v1/invoices/export/tally`

#### Purpose

Returns a **placeholder** Tally export message. No file download.

#### Authorization

- **Token:** Required.
- **Authority:** `INVOICE_MANAGEMENT_EXPORT` **or** `CEO`.

#### Request Body

**Not applicable.**

#### Response

- **200 OK**, `ResponseStructure<String>` — `data` is literal placeholder.

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Tally export prepared",
  "data": "Tally export placeholder"
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/invoices/export/tally" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/credit-notes/by-id`

#### Purpose

Same as **`GET /api/v1/invoices/credit-notes/by-id`** — standalone route for deep links to a credit note without invoice context in the path.

#### Authorization

- **Token:** Required.
- **Authority:** `INVOICE_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description    | Example       |
| ----- | ------ | -------- | -------------- | ------------- |
| `id`  | string | Yes      | Credit note id | `CN-E1F2G3H4` |

#### Request Body

**Not applicable.**

#### Response

Identical to nested credit note by-id.

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/credit-notes/by-id?id=CN-E1F2G3H4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

## Validation and Exception Summary

| Field / Scenario        | Validation / Rule        | Error Type         | Frontend Impact          |
| ----------------------- | ------------------------ | ------------------ | ------------------------ |
| `invoiceType` null      | Bean validation          | 400 + field errors | Required dropdown        |
| `grandTotal` null       | Bean validation          | 400                | Required amount          |
| `customerName` blank    | Bean validation          | 400                | Required text            |
| Update non-draft        | Service                  | 400                | Disable edit             |
| Delete non-draft        | Service                  | 400                | Hide delete              |
| Approve non-draft       | Service                  | 400                | Hide approve             |
| Approve missing ledgers | Service                  | 400                | Show admin message       |
| CN amount > pending     | Service                  | 400                | Cap input at pending     |
| CN missing ledgers      | Service                  | 400                | Admin fix                |
| Unknown invoice/CN id   | Service                  | 404                | Not found UI             |
| Invalid JSON / enum     | `HttpMessageNotReadable` | 400                | Fix payload              |
| No JWT                  | Spring                   | 401                | Login                    |
| Missing authority       | Spring                   | 403                | No access                |
| List filters            | **Ignored**              | —                  | Client filter or API fix |

---

## Frontend Integration Notes

- **Dropdowns:** Customer, branch, and ledger data come from **other modules** — not exposed in this controller.
- **Dependent fields:** **`dueDate`** is derived from **`invoiceDate` + `creditPeriodDays`** on save — show preview in form.
- **Enum rendering:** Use **`InvoiceStatus`** and **`InvoiceType`** string values exactly as listed.
- **Readonly fields:** After **`SENT`**, header editing is blocked by API — show read-only detail.
- **Create vs update:** Same JSON schema; update requires **`id`** query param.
- **List/search:** Server-side filters **not implemented** — document as gap; use client-side or backend ticket.
- **Credit note form:** Max **`creditAmount`** = current **`pendingAmount`** from invoice detail.
- **PDF:** Treat as stub until real template — do not promise pixel-perfect PDF.
- **Token:** Attach **`Authorization`** on every call; align tenant header with app standard.

---

_Documentation generated from `InvoicingController`, `CreditNoteController`, `InvoicingServiceImpl`, DTOs, entities, enums, and `GlobalExceptionHandler`._
