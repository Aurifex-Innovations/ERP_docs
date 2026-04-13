# Module 29 – Bills (Vendor Purchase / Accounts Payable)

## Short Description

Module 29 manages **vendor purchase bills** for accounts payable: create and edit **draft** bills, **confirm** a bill (status becomes **`PENDING`** and **ledger entries** post to **`PURCHASE_EXPENSE`** and the **vendor** ledger), **delete** drafts, **paginated listing**, **AP summary** metrics, **debit notes** against bills (with ledger posts to **`PURCHASE_ADJUSTMENT`** and vendor), **mark overdue** batch-style, and **PDF download** (placeholder bytes). A **standalone** debit-note-by-id route exists under **`/api/v1/debit-notes`**.

**What frontend developers need to know**

- **Base paths:** **`/api/v1/bills`** (main); **`/api/v1/debit-notes`** (standalone DN by id).
- **Authentication:** JWT **`Authorization: Bearer`**. **Tenant:** **`X-Tenant-ID`** — **Assumption:** same multitenancy pattern as other `/api/v1/**` modules (**Missing from backend context** if your deployment uses a different header name).
- **Success JSON:** `ResponseStructure<T>` → `{ "status", "message", "data" }` for JSON APIs. **`GET /bills/pdf`** returns **raw PDF bytes** with `Content-Disposition`, **not** JSON.
- **RBAC:** authorities `BILLS_MANAGEMENT_*` per operation; **`CEO`** bypasses where `@PreAuthorize` allows.
- **Simplified totals:** `BillCreateRequest` carries **`netPayable`** as the main amount; `apply` sets **`subTotal`** equal to **`netPayable`** on the entity — line-level tax split is **not** in the request DTO.
- **List filters** (`status`, `vendorId`, `search`) are **declared on the controller** but **not passed to `BillsService.list`** — only **`pageNo`** / **`pageSize`** apply (**Missing from backend context** / wire-up gap).
- **Confirm** requires configured **`PURCHASE_EXPENSE`** ledger and an **active vendor ledger**. **Debit note** requires **`PURCHASE_ADJUSTMENT`** and vendor ledger.
- **PDF** is a **stub** (placeholder content, not a real document).

---

## Authorization

### Authentication type

- **Bearer JWT** (`Authorization: Bearer <access_token>`).

### Required token

- **Yes** for all endpoints in this module.

### Required roles / authorities / permissions

| Authority                   | Typical endpoints                                                |
| --------------------------- | ---------------------------------------------------------------- |
| `BILLS_MANAGEMENT_ADD`      | `POST /bills`, `POST /bills/debit-notes`                         |
| `BILLS_MANAGEMENT_EDIT`     | `PUT /bills/update`, `POST /bills/mark-overdue`                  |
| `BILLS_MANAGEMENT_READ`     | All `GET` JSON endpoints, `GET /debit-notes/by-id`               |
| `BILLS_MANAGEMENT_DELETE`   | `DELETE /bills/delete`                                           |
| `BILLS_MANAGEMENT_APPROVE`  | `POST /bills/confirm`                                            |
| `BILLS_MANAGEMENT_DOWNLOAD` | `GET /bills/pdf`                                                 |
| **CEO**                     | `hasRole('CEO')` satisfies the same checks alongside authorities |

Exact mapping is in the **API List** table.

### Required headers

| Header          | Required            | Description        |
| --------------- | ------------------- | ------------------ |
| `Authorization` | Yes                 | `Bearer <JWT>`     |
| `X-Tenant-ID`   | **Assumption:** Yes | Tenant routing     |
| `Content-Type`  | Yes for JSON bodies | `application/json` |

### Tenant / schema-specific behavior

- **Assumption:** Data is isolated per tenant schema. **Missing from backend context:** confirm header name and resolution in security config.

### Access restrictions by role / business status

| Condition        | Effect                                           |
| ---------------- | ------------------------------------------------ |
| Missing JWT      | **401**                                          |
| Wrong authority  | **403**                                          |
| Bill `DRAFT`     | Update, delete, confirm allowed (with authority) |
| Bill not `DRAFT` | Update/delete/confirm rejected (**400**)         |
| Debit note       | `debitAmount` ≤ `pendingAmount`; bill must exist |

---

## Enums Used In This Module

### BillStatus

**Where used:** `PurchaseBill.status`, `BillResponse.status`, rules in `BillsServiceImpl`.

| Value       | Meaning                         | Used In                                                                             |
| ----------- | ------------------------------- | ----------------------------------------------------------------------------------- |
| `DRAFT`     | Editable draft                  | Create default; update/delete/confirm                                               |
| `PENDING`   | Confirmed; liability recognized | After **confirm** (before payments)                                                 |
| `PARTIAL`   | Partly paid or adjusted         | After partial payment (Module 30) or DN leaving balance                             |
| `PAID`      | Settled                         | Pending zero                                                                        |
| `OVERDUE`   | Past due with balance           | After **mark-overdue** when eligible                                                |
| `CANCELLED` | Cancelled                       | **Not set** in `BillsServiceImpl` — **Missing from backend context** for cancel API |

**Frontend notes:** Enable edit/delete/confirm only when `status === "DRAFT"`. After confirm, show **Pay bill** / **Debit note** flows instead of edit.

---

### `billType` (request/response string — not a Java enum)

**Where used:** `BillCreateRequest.billType`, `BillResponse.billType`.

| Behavior       | Notes                                                                                |
| -------------- | ------------------------------------------------------------------------------------ |
| **Validation** | `@NotBlank` — any non-empty string                                                   |
| **Examples**   | `"PURCHASE"`, `"SERVICE"`, `"EXPENSE"` — **Assumption:** align with product taxonomy |

Document as **free-text**; no fixed enum in code.

---

### Debit note `status` (string on entity)

**Where used:** `DebitNote.status`, `DebitNoteResponse.status`.

| Value    | Meaning                         |
| -------- | ------------------------------- |
| `ISSUED` | Default when created in service |

**Frontend notes:** Response typically shows `"ISSUED"`.

---

### Debit note `source` (entity field; not in API response)

**Where used:** `DebitNote.source`.

| Value               | Meaning                           |
| ------------------- | --------------------------------- |
| `MANUAL`            | From `POST /bills/debit-notes`    |
| `AUTO_FROM_PAYMENT` | From Module 30 voucher settlement |

**Missing from backend context:** `DebitNoteResponse` does **not** include `source` — UI cannot distinguish without API extension.

---

## API List

| Method   | Endpoint                          | Purpose                    | Authorization Required             |
| -------- | --------------------------------- | -------------------------- | ---------------------------------- |
| `POST`   | `/api/v1/bills`                   | Create draft bill          | `BILLS_MANAGEMENT_ADD` or CEO      |
| `PUT`    | `/api/v1/bills/update`            | Update draft bill          | `BILLS_MANAGEMENT_EDIT` or CEO     |
| `GET`    | `/api/v1/bills/by-id`             | Bill detail                | `BILLS_MANAGEMENT_READ` or CEO     |
| `DELETE` | `/api/v1/bills/delete`            | Delete draft bill          | `BILLS_MANAGEMENT_DELETE` or CEO   |
| `POST`   | `/api/v1/bills/confirm`           | Confirm bill + post ledger | `BILLS_MANAGEMENT_APPROVE` or CEO  |
| `GET`    | `/api/v1/bills`                   | Paginated bill list        | `BILLS_MANAGEMENT_READ` or CEO     |
| `GET`    | `/api/v1/bills/summary`           | AP summary metrics         | `BILLS_MANAGEMENT_READ` or CEO     |
| `POST`   | `/api/v1/bills/debit-notes`       | Issue debit note           | `BILLS_MANAGEMENT_ADD` or CEO      |
| `GET`    | `/api/v1/bills/debit-notes`       | List DNs for bill          | `BILLS_MANAGEMENT_READ` or CEO     |
| `GET`    | `/api/v1/bills/debit-notes/by-id` | DN by id (nested path)     | `BILLS_MANAGEMENT_READ` or CEO     |
| `POST`   | `/api/v1/bills/mark-overdue`      | Mark overdue bills         | `BILLS_MANAGEMENT_EDIT` or CEO     |
| `GET`    | `/api/v1/bills/pdf`               | Download bill PDF (stub)   | `BILLS_MANAGEMENT_DOWNLOAD` or CEO |
| `GET`    | `/api/v1/debit-notes/by-id`       | DN by id (standalone)      | `BILLS_MANAGEMENT_READ` or CEO     |

---

## API Details

---

### `POST` `/api/v1/bills`

#### Purpose

Creates a **draft** purchase bill with server-generated **`id`** (`BILL-` + fragment) and **`billNumber`** (`BILL-{year}-{seq}`). Sets **`paidAmount = 0`**, **`pendingAmount = netPayable`**, **`status = DRAFT`**. Call when the user saves a new vendor bill.

#### Authorization

- **Token:** Required.
- **Authority:** `BILLS_MANAGEMENT_ADD` **or** `CEO`.

#### Path Parameters

_Not applicable._

#### Query Parameters

_Not applicable._

#### Request Body Fields

| Field              | Type    | Required | Validation  | Description          | Example                     |
| ------------------ | ------- | -------- | ----------- | -------------------- | --------------------------- |
| `vendorBillNumber` | string  | Yes      | `@NotBlank` | Vendor’s invoice ref | `"VINV-2026-0092"`          |
| `billType`         | string  | Yes      | `@NotBlank` | Category label       | `"PURCHASE"`                |
| `branchId`         | string  | Yes      | `@NotBlank` | Branch               | `"BR-01"`                   |
| `vendorId`         | string  | Yes      | `@NotBlank` | Vendor FK            | `"VEND-0044"`               |
| `vendorName`       | string  | Yes      | `@NotBlank` | Name snapshot        | `"ABC Suppliers LLP"`       |
| `vendorGstin`      | string  | No       | —           | GSTIN                | `"27AAAAA0000A1Z5"`         |
| `vendorState`      | string  | No       | —           | State                | `"Maharashtra"`             |
| `purchaseOrderId`  | string  | No       | —           | PO link              | `"PO-2026-0100"`            |
| `billDate`         | date    | Yes      | `@NotNull`  | Bill date            | `"2026-04-13"`              |
| `creditPeriodDays` | integer | Yes      | `@NotNull`  | Days to due          | `30`                        |
| `netPayable`       | decimal | Yes      | `@NotNull`  | Amount due           | `88500.00`                  |
| `expenseCategory`  | string  | No       | —           | GL/category hint     | `"IT_HARDWARE"`             |
| `internalRemarks`  | string  | No       | —           | Internal notes       | `"Approved by procurement"` |

**Server-derived:** `dueDate = billDate + creditPeriodDays` (see response).

#### Full Request JSON Examples

**Minimal Valid Request**

```json
{
  "vendorBillNumber": "VINV-0092",
  "billType": "PURCHASE",
  "branchId": "BR-01",
  "vendorId": "VEND-001",
  "vendorName": "ABC Suppliers",
  "billDate": "2026-04-13",
  "creditPeriodDays": 30,
  "netPayable": 50000.0
}
```

**Complete Valid Request**

```json
{
  "vendorBillNumber": "VINV-2026-0092",
  "billType": "SERVICE",
  "branchId": "BR-MUM-01",
  "vendorId": "VEND-0044",
  "vendorName": "ABC Suppliers LLP",
  "vendorGstin": "27AAAAA0000A1Z5",
  "vendorState": "Maharashtra",
  "purchaseOrderId": "PO-2026-0100",
  "billDate": "2026-04-13",
  "creditPeriodDays": 45,
  "netPayable": 118250.5,
  "expenseCategory": "PROFESSIONAL_SERVICES",
  "internalRemarks": "Linked to annual maintenance contract"
}
```

**Zero Credit Period (due same day)**

```json
{
  "vendorBillNumber": "VINV-COD-01",
  "billType": "PURCHASE",
  "branchId": "BR-01",
  "vendorId": "VEND-002",
  "vendorName": "Cash Vendor",
  "billDate": "2026-04-13",
  "creditPeriodDays": 0,
  "netPayable": 1200.0
}
```

**Different billType values (illustrative — strings are free-form)**

```json
{
  "vendorBillNumber": "VINV-100",
  "billType": "EXPENSE",
  "branchId": "BR-01",
  "vendorId": "VEND-003",
  "vendorName": "Travel Agency",
  "billDate": "2026-04-13",
  "creditPeriodDays": 15,
  "netPayable": 8500.0
}
```

#### Response

- **201 Created**, `ResponseStructure<BillResponse>`.

#### Full Response JSON Example

```json
{
  "status": 201,
  "message": "Bill created",
  "data": {
    "id": "BILL-A1B2C3D4",
    "billNumber": "BILL-2026-0001",
    "vendorBillNumber": "VINV-0092",
    "billType": "PURCHASE",
    "status": "DRAFT",
    "billDate": "2026-04-13",
    "dueDate": "2026-05-13",
    "branchId": "BR-01",
    "vendorId": "VEND-001",
    "vendorName": "ABC Suppliers",
    "netPayable": 50000.0,
    "paidAmount": 0,
    "pendingAmount": 50000.0
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason         | When It Happens   | Typical Message                          | Frontend Handling Note |
| ----------- | -------------- | ----------------- | ---------------------------------------- | ---------------------- |
| 400         | Validation     | Bean validation   | `Input validation failed`                | Map `validationErrors` |
| 400         | Malformed JSON | Bad body          | `Malformed JSON request` / invalid field | Fix payload            |
| 401         | Unauthorized   | No/invalid JWT    | Spring message                           | Login                  |
| 403         | Forbidden      | Missing authority | Access denied message                    | Hide action            |

#### Error Response JSON Examples

**Validation error**

```json
{
  "timestamp": "2026-04-13T10:00:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "Input validation failed",
  "path": "/api/v1/bills",
  "validationErrors": {
    "vendorName": "must not be blank",
    "netPayable": "must not be null"
  }
}
```

**Unauthorized**

```json
{
  "timestamp": "2026-04-13T10:01:00.000",
  "status": 401,
  "error": "Unauthorized",
  "message": "Full authentication is required to access this resource",
  "path": "/api/v1/bills",
  "validationErrors": null
}
```

**Forbidden**

```json
{
  "timestamp": "2026-04-13T10:02:00.000",
  "status": 403,
  "error": "Forbidden",
  "message": "Access Denied: You don't have permission to access this resource",
  "path": "/api/v1/bills",
  "validationErrors": null
}
```

#### Frontend Notes

- Persist returned **`id`** for update/confirm/delete.
- Show **`dueDate`** next to **`billDate`**.

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/bills" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001" \
  -H "Content-Type: application/json" \
  -d '{
    "vendorBillNumber": "VINV-0092",
    "billType": "PURCHASE",
    "branchId": "BR-01",
    "vendorId": "VEND-001",
    "vendorName": "ABC Suppliers",
    "billDate": "2026-04-13",
    "creditPeriodDays": 30,
    "netPayable": 50000.0
  }'
```

---

### `PUT` `/api/v1/bills/update`

#### Purpose

Updates a bill **only while `status` is `DRAFT`**. Reapplies header fields and sets **`pendingAmount = netPayable - paidAmount`** (keeps recorded payments). Use when editing a draft.

#### Authorization

- **Token:** Required.
- **Authority:** `BILLS_MANAGEMENT_EDIT` **or** `CEO`.

#### Path Parameters

_Not applicable._

#### Query Parameters

| Field | Type   | Required | Description | Example         |
| ----- | ------ | -------- | ----------- | --------------- |
| `id`  | string | Yes      | Bill id     | `BILL-A1B2C3D4` |

#### Request Body Fields

Same as **`POST /api/v1/bills`**.

#### Full Request JSON Examples

**Update Request Example**

```json
{
  "vendorBillNumber": "VINV-0092-REV1",
  "billType": "PURCHASE",
  "branchId": "BR-01",
  "vendorId": "VEND-001",
  "vendorName": "ABC Suppliers",
  "billDate": "2026-04-13",
  "creditPeriodDays": 30,
  "netPayable": 52000.0,
  "expenseCategory": "PURCHASE",
  "internalRemarks": "Revised per vendor credit memo discussion"
}
```

**Update after partial payment (illustrative)**

If `paidAmount` were `10000` and new `netPayable` is `52000`, service sets `pendingAmount = 42000`.

```json
{
  "vendorBillNumber": "VINV-0092",
  "billType": "PURCHASE",
  "branchId": "BR-01",
  "vendorId": "VEND-001",
  "vendorName": "ABC Suppliers",
  "billDate": "2026-04-13",
  "creditPeriodDays": 30,
  "netPayable": 52000.0
}
```

#### Response

- **200 OK**, `ResponseStructure<BillResponse>`.

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Bill updated",
  "data": {
    "id": "BILL-A1B2C3D4",
    "billNumber": "BILL-2026-0001",
    "vendorBillNumber": "VINV-0092-REV1",
    "billType": "PURCHASE",
    "status": "DRAFT",
    "billDate": "2026-04-13",
    "dueDate": "2026-05-13",
    "branchId": "BR-01",
    "vendorId": "VEND-001",
    "vendorName": "ABC Suppliers",
    "netPayable": 52000.0,
    "paidAmount": 0,
    "pendingAmount": 52000.0
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason       | Typical Message                  |
| ----------- | ------------ | -------------------------------- |
| 400         | Not draft    | `Only draft bill can be updated` |
| 404         | Missing bill | `Bill not found`                 |

#### Error Response JSON Examples

```json
{
  "timestamp": "2026-04-13T10:05:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "Only draft bill can be updated",
  "path": "/api/v1/bills/update",
  "validationErrors": null
}
```

```json
{
  "timestamp": "2026-04-13T10:06:00.000",
  "status": 404,
  "error": "Not Found",
  "message": "Bill not found",
  "path": "/api/v1/bills/update",
  "validationErrors": null
}
```

#### Frontend Notes

- Block edit unless **`DRAFT`**.

#### cURL

```bash
curl -X PUT "https://{baseUrl}/api/v1/bills/update?id=BILL-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001" \
  -H "Content-Type: application/json" \
  -d '{
    "vendorBillNumber": "VINV-0092-REV1",
    "billType": "PURCHASE",
    "branchId": "BR-01",
    "vendorId": "VEND-001",
    "vendorName": "ABC Suppliers",
    "billDate": "2026-04-13",
    "creditPeriodDays": 30,
    "netPayable": 52000.0
  }'
```

---

### `GET` `/api/v1/bills/by-id`

#### Purpose

Returns one bill for detail screens.

#### Authorization

- **Token:** Required.
- **Authority:** `BILLS_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description | Example         |
| ----- | ------ | -------- | ----------- | --------------- |
| `id`  | string | Yes      | Bill id     | `BILL-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Response

- **200 OK**, `ResponseStructure<BillResponse>`.

#### Full Response JSON Example (confirmed bill)

```json
{
  "status": 200,
  "message": "Bill fetched",
  "data": {
    "id": "BILL-A1B2C3D4",
    "billNumber": "BILL-2026-0001",
    "vendorBillNumber": "VINV-0092",
    "billType": "PURCHASE",
    "status": "PENDING",
    "billDate": "2026-04-13",
    "dueDate": "2026-05-13",
    "branchId": "BR-01",
    "vendorId": "VEND-001",
    "vendorName": "ABC Suppliers",
    "netPayable": 50000.0,
    "paidAmount": 0,
    "pendingAmount": 50000.0
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason     | Typical Message  |
| ----------- | ---------- | ---------------- |
| 404         | Unknown id | `Bill not found` |

#### Error Response JSON Example

```json
{
  "timestamp": "2026-04-13T10:07:00.000",
  "status": 404,
  "error": "Not Found",
  "message": "Bill not found",
  "path": "/api/v1/bills/by-id",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/bills/by-id?id=BILL-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `DELETE` `/api/v1/bills/delete`

#### Purpose

Deletes a bill **only in `DRAFT`**.

#### Authorization

- **Token:** Required.
- **Authority:** `BILLS_MANAGEMENT_DELETE` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description | Example         |
| ----- | ------ | -------- | ----------- | --------------- |
| `id`  | string | Yes      | Bill id     | `BILL-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Response

- **200 OK**, `data`: `null`.

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Bill deleted",
  "data": null
}
```

#### Error Response JSON Example

```json
{
  "timestamp": "2026-04-13T10:08:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "Only draft bill can be deleted",
  "path": "/api/v1/bills/delete",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X DELETE "https://{baseUrl}/api/v1/bills/delete?id=BILL-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `POST` `/api/v1/bills/confirm`

#### Purpose

Moves bill **`DRAFT` → `PENDING`** and posts ledger: **debit `PURCHASE_EXPENSE`**, **credit vendor AP**. Call when finance accepts the vendor bill.

#### Authorization

- **Token:** Required.
- **Authority:** `BILLS_MANAGEMENT_APPROVE` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description | Example         |
| ----- | ------ | -------- | ----------- | --------------- |
| `id`  | string | Yes      | Bill id     | `BILL-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Response

- **200 OK**, `ResponseStructure<BillResponse>` with `status: "PENDING"`.

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Bill confirmed",
  "data": {
    "id": "BILL-A1B2C3D4",
    "billNumber": "BILL-2026-0001",
    "vendorBillNumber": "VINV-0092",
    "billType": "PURCHASE",
    "status": "PENDING",
    "billDate": "2026-04-13",
    "dueDate": "2026-05-13",
    "branchId": "BR-01",
    "vendorId": "VEND-001",
    "vendorName": "ABC Suppliers",
    "netPayable": 50000.0,
    "paidAmount": 0,
    "pendingAmount": 50000.0
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason       | Typical Message                          |
| ----------- | ------------ | ---------------------------------------- |
| 400         | Not draft    | `Only draft bill can be confirmed`       |
| 404         | Missing bill | `Bill not found`                         |
| 400         | Ledger       | `PURCHASE_EXPENSE ledger not configured` |
| 400         | Ledger       | `Vendor ledger not found/active`         |

#### Error Response JSON Examples

```json
{
  "timestamp": "2026-04-13T10:09:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "PURCHASE_EXPENSE ledger not configured",
  "path": "/api/v1/bills/confirm",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/bills/confirm?id=BILL-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/bills`

#### Purpose

Returns a **paginated** list of bills, sorted **`updatedAt` DESC**, **`createdAt` DESC**. **Controller query filters are not applied** in the service.

#### Authorization

- **Token:** Required.
- **Authority:** `BILLS_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field      | Type   | Required          | Description            | Example     | Allowed values |
| ---------- | ------ | ----------------- | ---------------------- | ----------- | -------------- |
| `pageNo`   | int    | No (default `0`)  | Page index             | `0`         | ≥ 0            |
| `pageSize` | int    | No (default `10`) | Page size              | `20`        | ≥ 1            |
| `status`   | string | No                | **Ignored by service** | `PENDING`   | —              |
| `vendorId` | string | No                | **Ignored by service** | `VEND-001`  | —              |
| `search`   | string | No                | **Ignored by service** | `BILL-2026` | —              |

#### Request Body

**Not applicable.**

#### Response

- **200 OK**, `ResponseStructure<PaginationResponse<BillResponse>>`.

#### Full Response JSON Example (with rows)

```json
{
  "status": 200,
  "message": "Bills fetched",
  "data": {
    "count": 15,
    "next": "https://api.example.com/api/v1/bills?pageSize=10&pageNo=1",
    "prev": null,
    "data": [
      {
        "id": "BILL-A1B2C3D4",
        "billNumber": "BILL-2026-0001",
        "vendorBillNumber": "VINV-0092",
        "billType": "PURCHASE",
        "status": "PENDING",
        "billDate": "2026-04-13",
        "dueDate": "2026-05-13",
        "branchId": "BR-01",
        "vendorId": "VEND-001",
        "vendorName": "ABC Suppliers",
        "netPayable": 50000.0,
        "paidAmount": 0,
        "pendingAmount": 50000.0
      }
    ]
  }
}
```

#### Full Response JSON Example (empty list)

```json
{
  "status": 200,
  "message": "Bills fetched",
  "data": {
    "count": 0,
    "next": null,
    "prev": null,
    "data": []
  }
}
```

#### Frontend Notes

- **Search With Filters Example** (request works but **does not filter** server-side):

```http
GET /api/v1/bills?pageNo=0&pageSize=20&status=PENDING&vendorId=VEND-001&search=BILL
```

Implement **client-side** filtering or fix backend.

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/bills?pageNo=0&pageSize=20" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

**Filters (currently no-op server-side)**

```bash
curl -X GET "https://{baseUrl}/api/v1/bills?pageNo=0&pageSize=20&status=PENDING&vendorId=VEND-001" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/bills/summary`

#### Purpose

Dashboard aggregates over **all bills** in tenant.

#### Authorization

- **Token:** Required.
- **Authority:** `BILLS_MANAGEMENT_READ` **or** `CEO`.

#### Request Body

**Not applicable.**

#### Response fields (from service)

| Field           | Logic                                                                  |
| --------------- | ---------------------------------------------------------------------- |
| `totalPayable`  | Sum `pendingAmount` where status is `PENDING`, `PARTIAL`, or `OVERDUE` |
| `overdueAmount` | Sum `pendingAmount` where status is `OVERDUE`                          |
| `paidAmount`    | Sum `netPayable` where status is `PAID`                                |
| `draftCount`    | Count where status is `DRAFT`                                          |

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Bill summary fetched",
  "data": {
    "totalPayable": 890000.25,
    "overdueAmount": 120000.0,
    "paidAmount": 450000.0,
    "draftCount": 2
  }
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/bills/summary" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `POST` `/api/v1/bills/debit-notes`

#### Purpose

Creates a **debit note** against a bill, reduces **`pendingAmount`**, may set **`PAID`** or **`PARTIAL`**, posts **`PURCHASE_ADJUSTMENT`** and **vendor** ledger. `source` stored as **`MANUAL`**.

#### Authorization

- **Token:** Required.
- **Authority:** `BILLS_MANAGEMENT_ADD` **or** `CEO`.

#### Query Parameters

| Field    | Type   | Required | Description | Example         |
| -------- | ------ | -------- | ----------- | --------------- |
| `billId` | string | Yes      | Bill id     | `BILL-A1B2C3D4` |

#### Request Body Fields

| Field         | Type    | Required | Validation  | Description       | Example                   |
| ------------- | ------- | -------- | ----------- | ----------------- | ------------------------- |
| `dnDate`      | date    | Yes      | `@NotNull`  | DN date           | `"2026-04-13"`            |
| `reason`      | string  | Yes      | `@NotBlank` | Reason            | `"RATE_DIFFERENCE"`       |
| `otherReason` | string  | No       | —           | Extra detail      | `"Per vendor agreement"`  |
| `remarks`     | string  | No       | —           | Notes             | `"DN for short shipment"` |
| `debitAmount` | decimal | Yes      | `@NotNull`  | ≤ `pendingAmount` | `5000.0`                  |

#### Full Request JSON Examples

**Minimal Valid Request**

```json
{
  "dnDate": "2026-04-13",
  "reason": "GOODS_RETURN",
  "debitAmount": 5000.0
}
```

**Complete Valid Request**

```json
{
  "dnDate": "2026-04-13",
  "reason": "OTHER",
  "otherReason": "Post-audit price correction",
  "remarks": "Ticket AP-441",
  "debitAmount": 12500.5
}
```

**Full pending settlement**

```json
{
  "dnDate": "2026-04-13",
  "reason": "FULL_SETTLEMENT",
  "debitAmount": 50000.0
}
```

**Partial debit (leaves balance — bill may become PARTIAL)**

```json
{
  "dnDate": "2026-04-13",
  "reason": "PARTIAL_REBATE",
  "debitAmount": 10000.0
}
```

#### Response

- **201 Created**, `ResponseStructure<DebitNoteResponse>`.

#### Full Response JSON Example

```json
{
  "status": 201,
  "message": "Debit note issued",
  "data": {
    "id": "DN-E1F2G3H4",
    "dnNumber": "DN-2026-0001",
    "billId": "BILL-A1B2C3D4",
    "dnDate": "2026-04-13",
    "reason": "GOODS_RETURN",
    "debitAmount": 5000.0,
    "status": "ISSUED"
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason | Typical Message                             |
| ----------- | ------ | ------------------------------------------- |
| 404         | Bill   | `Bill not found`                            |
| 400         | Amount | `Debit amount cannot exceed pending`        |
| 400         | Ledger | `Vendor ledger not found/active`            |
| 400         | Ledger | `PURCHASE_ADJUSTMENT ledger not configured` |

#### Error Response JSON Examples

```json
{
  "timestamp": "2026-04-13T10:12:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "Debit amount cannot exceed pending",
  "path": "/api/v1/bills/debit-notes",
  "validationErrors": null
}
```

```json
{
  "timestamp": "2026-04-13T10:13:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "PURCHASE_ADJUSTMENT ledger not configured",
  "path": "/api/v1/bills/debit-notes",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/bills/debit-notes?billId=BILL-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001" \
  -H "Content-Type: application/json" \
  -d '{
    "dnDate": "2026-04-13",
    "reason": "GOODS_RETURN",
    "debitAmount": 5000.0
  }'
```

---

### `GET` `/api/v1/bills/debit-notes`

#### Purpose

Lists debit notes for a **`billId`**, newest first.

#### Authorization

- **Token:** Required.
- **Authority:** `BILLS_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field    | Type   | Required | Description | Example         |
| -------- | ------ | -------- | ----------- | --------------- |
| `billId` | string | Yes      | Bill id     | `BILL-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Debit notes fetched",
  "data": [
    {
      "id": "DN-E1F2G3H4",
      "dnNumber": "DN-2026-0002",
      "billId": "BILL-A1B2C3D4",
      "dnDate": "2026-04-15",
      "reason": "RATE_DIFFERENCE",
      "debitAmount": 2000.0,
      "status": "ISSUED"
    },
    {
      "id": "DN-E1F2G3H5",
      "dnNumber": "DN-2026-0001",
      "billId": "BILL-A1B2C3D4",
      "dnDate": "2026-04-13",
      "reason": "GOODS_RETURN",
      "debitAmount": 5000.0,
      "status": "ISSUED"
    }
  ]
}
```

#### Empty state

```json
{
  "status": 200,
  "message": "Debit notes fetched",
  "data": []
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/bills/debit-notes?billId=BILL-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/bills/debit-notes/by-id`

#### Purpose

Fetches one debit note by **`id`** (nested under bills path).

#### Authorization

- **Token:** Required.
- **Authority:** `BILLS_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description   | Example       |
| ----- | ------ | -------- | ------------- | ------------- |
| `id`  | string | Yes      | Debit note id | `DN-E1F2G3H4` |

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Debit note fetched",
  "data": {
    "id": "DN-E1F2G3H4",
    "dnNumber": "DN-2026-0001",
    "billId": "BILL-A1B2C3D4",
    "dnDate": "2026-04-13",
    "reason": "GOODS_RETURN",
    "debitAmount": 5000.0,
    "status": "ISSUED"
  }
}
```

#### Error Response JSON Example

```json
{
  "timestamp": "2026-04-13T10:14:00.000",
  "status": 404,
  "error": "Not Found",
  "message": "Debit note not found",
  "path": "/api/v1/bills/debit-notes/by-id",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/bills/debit-notes/by-id?id=DN-E1F2G3H4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `POST` `/api/v1/bills/mark-overdue`

#### Purpose

For each bill with status **`PENDING`** or **`PARTIAL`**, positive **`pendingAmount`**, **`dueDate` < today**, sets status to **`OVERDUE`**. Returns count of bills updated.

#### Authorization

- **Token:** Required.
- **Authority:** `BILLS_MANAGEMENT_EDIT` **or** `CEO`.

#### Request Body

**Not applicable.**

#### Full Response JSON Examples

```json
{
  "status": 200,
  "message": "Overdue marking completed",
  "data": 3
}
```

```json
{
  "status": 200,
  "message": "Overdue marking completed",
  "data": 0
}
```

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/bills/mark-overdue" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/bills/pdf`

#### Purpose

Returns **placeholder PDF bytes** (not a real template). **Assumption:** may not validate bill existence before returning stub.

#### Authorization

- **Token:** Required.
- **Authority:** `BILLS_MANAGEMENT_DOWNLOAD` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description | Example         |
| ----- | ------ | -------- | ----------- | --------------- |
| `id`  | string | Yes      | Bill id     | `BILL-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Response

- **200 OK**, `Content-Type: application/pdf`, `Content-Disposition: attachment; filename="bill-{id}.pdf"`, body = bytes (placeholder).

**Not** wrapped in `ResponseStructure`.

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/bills/pdf?id=BILL-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001" \
  -o bill.pdf
```

---

### `GET` `/api/v1/debit-notes/by-id`

#### Purpose

Same payload as **`GET /api/v1/bills/debit-notes/by-id`** — for deep links when only DN id is known.

#### Authorization

- **Token:** Required.
- **Authority:** `BILLS_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description   | Example       |
| ----- | ------ | -------- | ------------- | ------------- |
| `id`  | string | Yes      | Debit note id | `DN-E1F2G3H4` |

#### Request Body

**Not applicable.**

#### Response

Same `DebitNoteResponse` structure as nested route.

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/debit-notes/by-id?id=DN-E1F2G3H4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

## Validation and Exception Summary

| Field / Scenario                               | Validation / Rule | Error Type | Frontend Impact           |
| ---------------------------------------------- | ----------------- | ---------- | ------------------------- |
| Required bill fields                           | Bean validation   | 400        | Show field errors         |
| Update/delete/confirm non-draft                | Service           | 400        | Disable actions by status |
| Bill not found                                 | Service           | 404        | Not found UI              |
| DN amount > pending                            | Service           | 400        | Cap at `pendingAmount`    |
| Missing `PURCHASE_EXPENSE` / vendor on confirm | Service           | 400        | Admin configures COA      |
| Missing `PURCHASE_ADJUSTMENT` / vendor on DN   | Service           | 400        | Admin configures COA      |
| List filters                                   | **Ignored**       | —          | Client filter or API fix  |
| Invalid JSON / enum parse                      | Global handler    | 400        | Fix payload               |
| No JWT / wrong role                            | Spring security   | 401 / 403  | Login / hide UI           |

---

## Frontend Integration Notes

- **Dropdowns:** Vendor and branch master data come from **other modules** — not listed here.
- **Dependent fields:** **`dueDate`** = **`billDate` + `creditPeriodDays`** — show preview on the form.
- **BillStatus rendering:** Use enum strings exactly as returned; **`CANCELLED`** unused in service.
- **Readonly:** After **`PENDING`** / **`PARTIAL`** / **`OVERDUE`** / **`PAID`**, edit/delete/confirm are blocked — align buttons with API.
- **Create vs update:** Same JSON schema; update needs **`id`** query param.
- **Debit note:** Max **`debitAmount`** = current **`pendingAmount`** from bill detail.
- **Pagination:** Use `next`/`prev` from `PaginationResponse` or increment `pageNo`.
- **Search/debounce:** Until server filters work, debounce client-side search if you load full pages.
- **Dates:** ISO **date-only** (`LocalDate`) in JSON.
- **Files / multipart:** **Not used** in this module’s controllers.
- **Token:** Send **`Authorization`** on every request; tenant header per app standard.

---

_Generated from `BillsController`, `DebitNoteController`, `BillsServiceImpl`, DTOs, `BillStatus`, and `GlobalExceptionHandler` / `ValidationErrorResponse`._
