# Module 30 – Voucher (Receipts & Payments)

## Short Description

Module 30 implements **accounting vouchers** for **customer receipts** (cash/bank in) and **vendor payments** (cash/bank out). Creating a voucher **posts immediately** with status **`POSTED`**, optional **line allocations** against **sales invoices** or **purchase bills**, automatic **credit notes** or **debit notes** when **`SETTLE_CLOSE`** is used, **ledger entries** (bank/cash vs customer or vendor), **unallocated/advance** amounts, a **paginated register**, **summary** metrics, **allocation audit lines**, and **void** (status only).

**What frontend developers need to know**

- **Base path:** **`/api/v1/vouchers`**; create via **`POST /receipts`** and **`POST /payments`**.
- **Authentication:** JWT **`Authorization: Bearer`**. **Tenant:** **`X-Tenant-ID`** — **Assumption:** same multitenancy as other `/api/v1/**` modules (**Missing from backend context** if your app uses a different header).
- **Success JSON:** `ResponseStructure<T>` → `{ "status", "message", "data" }` for all JSON endpoints in this controller.
- **Split RBAC:** Receipts use **`RECEIPT_MANAGEMENT_*`**; payments use **`PAYMENT_MANAGEMENT_*`**. Shared read endpoints accept **either** read authority **or** **`CEO`**. Void accepts **either** edit authority **or** **`CEO`**.
- **No update endpoint** — vouchers are create + void only in this controller.
- **List query params** (`voucherType`, `partyId`, `paymentMode`, `search`) are **declared but not passed to the service** — only pagination applies (**Missing from backend context** / wire-up gap).
- **`bankLedgerId`** is optional in the DTO but **required for successful ledger posting** — sending `null` will fail at `findById` (**Assumption:** always send a valid bank/cash ledger id in production).
- **Void** sets status to **`VOID`** only; it does **not** reverse invoices, bills, or ledger entries (**documented limitation** — confirm with backend before using void for corrections).
- **`VoucherResponse`** does **not** expose **`bankLedgerId`** even though it is stored on the entity.

---

## Authorization

### Authentication type

- **Bearer JWT** (`Authorization: Bearer <access_token>`).

### Required token

- **Yes** for all endpoints.

### Required roles / authorities / permissions

| Operation                                                                  | Authority (alternatively `CEO`)                            |
| -------------------------------------------------------------------------- | ---------------------------------------------------------- |
| `POST /vouchers/receipts`                                                  | `RECEIPT_MANAGEMENT_ADD`                                   |
| `POST /vouchers/payments`                                                  | `PAYMENT_MANAGEMENT_ADD`                                   |
| `GET /vouchers/by-id`, `GET /vouchers`, `GET /summary`, `GET /allocations` | `PAYMENT_MANAGEMENT_READ` **or** `RECEIPT_MANAGEMENT_READ` |
| `POST /vouchers/void`                                                      | `PAYMENT_MANAGEMENT_EDIT` **or** `RECEIPT_MANAGEMENT_EDIT` |
| **CEO**                                                                    | `hasRole('CEO')` satisfies the same `@PreAuthorize` checks |

### Required headers

| Header          | Required            | Description        |
| --------------- | ------------------- | ------------------ |
| `Authorization` | Yes                 | `Bearer <JWT>`     |
| `X-Tenant-ID`   | **Assumption:** Yes | Tenant routing     |
| `Content-Type`  | Yes for POST bodies | `application/json` |

### Tenant / schema-specific behavior

- **Assumption:** Vouchers, invoices, bills, and ledger rows are tenant-scoped.

### Access restrictions by business status

| Condition    | Effect                                                                           |
| ------------ | -------------------------------------------------------------------------------- |
| Void         | Only when voucher `status === "POSTED"`                                          |
| Allocation   | `allocateAmount` ≤ document `pendingAmount` at time of posting                   |
| Cross-module | Receipt allocations need valid **invoice** ids; payments need valid **bill** ids |

---

## Enums Used In This Module

This module uses **string fields** on entities and DTOs rather than Java `enum` types for voucher classification. Below are the **values observed in code** and related modules.

### Voucher type (`voucherType`)

**Where used:** `Voucher.voucherType`, `VoucherResponse.voucherType`.

| Value     | Meaning                      | Used In                |
| --------- | ---------------------------- | ---------------------- |
| `RECEIPT` | Money received from customer | Set in `createReceipt` |
| `PAYMENT` | Money paid to vendor         | Set in `createPayment` |

**Frontend notes:** No `CONTRA` / `JOURNAL` in this controller.

---

### Voucher status (`status`)

**Where used:** `Voucher.status`, `VoucherResponse.status`.

| Value    | Meaning          | Used In            |
| -------- | ---------------- | ------------------ |
| `POSTED` | Voucher recorded | Default on create  |
| `VOID`   | Voided (soft)    | After `POST /void` |

**Frontend notes:** Summary and allocation listing logic only treat **`POSTED`** vouchers as active for totals (void excluded).

---

### Party type (`partyType`)

**Where used:** `Voucher.partyType`, `VoucherResponse.partyType`.

| Value      | Meaning         | Used In         |
| ---------- | --------------- | --------------- |
| `CUSTOMER` | Receipt voucher | `createReceipt` |
| `VENDOR`   | Payment voucher | `createPayment` |

---

### Allocation document type (`documentType` in allocation responses)

**Where used:** `VoucherAllocationResponse.documentType`.

| Value     | Meaning                          | Used In      |
| --------- | -------------------------------- | ------------ |
| `INVOICE` | Allocation against sales invoice | Receipt flow |
| `BILL`    | Allocation against purchase bill | Payment flow |

---

### Settlement action (`settlementAction`)

**Where used:** `AllocationItemRequest.settlementAction`, `VoucherAllocationResponse.settlementAction`.

| Value                          | Meaning             | Behavior when `allocateAmount` < previous pending                                              |
| ------------------------------ | ------------------- | ---------------------------------------------------------------------------------------------- |
| _(null or not `SETTLE_CLOSE`)_ | Treat as keep open  | Invoice/bill → **`PARTIAL`** if shortfall remains                                              |
| `SETTLE_CLOSE`                 | Write off shortfall | Issues **credit note** (invoice) or **debit note** (bill) for **shortfall** via Module 28 / 29 |

**Comment in code:** `KEEP_OPEN` / `SETTLE_CLOSE` — only **`SETTLE_CLOSE`** is compared explicitly; other values behave like keep-open.

---

### Invoice status (Module 28 — when allocations update invoices)

**Where used:** `SalesInvoice.status` after receipt allocation.

| Value     | Set when                                       |
| --------- | ---------------------------------------------- |
| `PAID`    | Pending becomes zero after allocation          |
| `PARTIAL` | Shortfall remains and not settling via CN path |

_(Full enum lives in `InvoiceStatus` — see Module 28 doc.)_

---

### Bill status (Module 29 — when allocations update bills)

**Where used:** `PurchaseBill.status` after payment allocation.

| Value     | Set when             |
| --------- | -------------------- |
| `PAID`    | Pending becomes zero |
| `PARTIAL` | Shortfall remains    |

_(Full enum lives in `BillStatus` — see Module 29 doc.)_

---

## API List

| Method | Endpoint                       | Purpose                                                        | Authorization Required                                        |
| ------ | ------------------------------ | -------------------------------------------------------------- | ------------------------------------------------------------- |
| `POST` | `/api/v1/vouchers/receipts`    | Create receipt voucher + optional invoice allocations + ledger | `RECEIPT_MANAGEMENT_ADD` or CEO                               |
| `POST` | `/api/v1/vouchers/payments`    | Create payment voucher + optional bill allocations + ledger    | `PAYMENT_MANAGEMENT_ADD` or CEO                               |
| `GET`  | `/api/v1/vouchers/by-id`       | Get voucher by id                                              | `PAYMENT_MANAGEMENT_READ` or `RECEIPT_MANAGEMENT_READ` or CEO |
| `GET`  | `/api/v1/vouchers`             | Paginated voucher list                                         | Same read                                                     |
| `GET`  | `/api/v1/vouchers/summary`     | Totals / net cash / unallocated advance                        | Same read                                                     |
| `GET`  | `/api/v1/vouchers/allocations` | Allocation rows for a voucher                                  | Same read                                                     |
| `POST` | `/api/v1/vouchers/void`        | Void voucher                                                   | `PAYMENT_MANAGEMENT_EDIT` or `RECEIPT_MANAGEMENT_EDIT` or CEO |

---

## API Details

---

### `POST` `/api/v1/vouchers/receipts`

#### Purpose

Creates a **receipt** voucher (`VCH-*`, number `RCP-{year}-{seq}`), optionally allocates to **invoices**, may trigger **auto credit note** on `SETTLE_CLOSE`, updates invoice balances, posts **bank debit** and **customer credit** in the ledger.

#### Authorization

- **Token:** Required.
- **Authority:** `RECEIPT_MANAGEMENT_ADD` **or** `CEO`.

#### Path Parameters

_Not applicable._

#### Query Parameters

_Not applicable._

#### Request Body Fields — `ReceiptCreateRequest`

| Field            | Type    | Required | Validation      | Description                    | Example               |
| ---------------- | ------- | -------- | --------------- | ------------------------------ | --------------------- |
| `voucherDate`    | date    | Yes      | `@NotNull`      | Voucher date                   | `"2026-04-13"`        |
| `branchId`       | string  | Yes      | `@NotBlank`     | Branch                         | `"BR-01"`             |
| `customerId`     | string  | Yes      | `@NotBlank`     | Customer                       | `"CUST-001"`          |
| `paymentMode`    | string  | Yes      | `@NotBlank`     | Mode label                     | `"NEFT"`              |
| `bankLedgerId`   | string  | No\*     | —               | Bank/cash ledger id            | `"LEDGER-BANK-01"`    |
| `referenceNo`    | string  | No       | —               | UTR / ref                      | `"UTR998877"`         |
| `amountReceived` | decimal | Yes      | `@NotNull`      | Total receipt                  | `100000.0`            |
| `tdsAmount`      | decimal | No       | —               | TDS; defaults **0** in service | `500.0`               |
| `notes`          | string  | No       | —               | Notes                          | `"Against inv batch"` |
| `allocations`    | array   | No       | `@Valid` nested | See `AllocationItemRequest`    | —                     |

#### `AllocationItemRequest` (each element)

| Field              | Type    | Required | Description                        | Example                |
| ------------------ | ------- | -------- | ---------------------------------- | ---------------------- |
| `documentId`       | string  | Yes      | **Sales invoice id**               | `"INV-A1B2C3D4"`       |
| `allocateAmount`   | decimal | Yes      | ≤ invoice pending                  | `25000.0`              |
| `settlementAction` | string  | No       | `SETTLE_CLOSE` or omit             | `"SETTLE_CLOSE"`       |
| `settlementReason` | string  | No       | Passed to CN `reason` if CN issued | `"PAYMENT_SETTLEMENT"` |

#### Full Request JSON Examples

**Minimal Valid Request (no allocations)**

```json
{
  "voucherDate": "2026-04-13",
  "branchId": "BR-01",
  "customerId": "CUST-001",
  "paymentMode": "NEFT",
  "bankLedgerId": "LEDGER-BANK-01",
  "referenceNo": "UTR112233",
  "amountReceived": 75000.0
}
```

**Complete Valid Request**

```json
{
  "voucherDate": "2026-04-13",
  "branchId": "BR-MUM-01",
  "customerId": "CUST-0192",
  "paymentMode": "UPI",
  "bankLedgerId": "LEDGER-BANK-01",
  "referenceNo": "UPI-884422",
  "amountReceived": 118000.0,
  "tdsAmount": 0,
  "notes": "Against invoice batch April",
  "allocations": [
    {
      "documentId": "INV-A1B2C3D4",
      "allocateAmount": 50000.0,
      "settlementAction": "KEEP_OPEN"
    }
  ]
}
```

**Allocation — full pay single invoice**

```json
{
  "voucherDate": "2026-04-13",
  "branchId": "BR-01",
  "customerId": "CUST-001",
  "paymentMode": "RTGS",
  "bankLedgerId": "LEDGER-BANK-01",
  "referenceNo": "RTGS-001",
  "amountReceived": 50000.0,
  "allocations": [
    {
      "documentId": "INV-A1B2C3D4",
      "allocateAmount": 50000.0
    }
  ]
}
```

**SETTLE_CLOSE (auto credit note for shortfall)**

```json
{
  "voucherDate": "2026-04-13",
  "branchId": "BR-01",
  "customerId": "CUST-001",
  "paymentMode": "NEFT",
  "bankLedgerId": "LEDGER-BANK-01",
  "referenceNo": "UTR445566",
  "amountReceived": 40000.0,
  "allocations": [
    {
      "documentId": "INV-A1B2C3D4",
      "allocateAmount": 40000.0,
      "settlementAction": "SETTLE_CLOSE",
      "settlementReason": "WRITE_OFF_DIFF"
    }
  ]
}
```

**On-account receipt (no allocations — full unallocated)**

```json
{
  "voucherDate": "2026-04-13",
  "branchId": "BR-01",
  "customerId": "CUST-002",
  "paymentMode": "CASH",
  "bankLedgerId": "LEDGER-CASH-01",
  "amountReceived": 20000.0,
  "notes": "Advance on account",
  "allocations": null
}
```

#### Response

- **201 Created**, `ResponseStructure<VoucherResponse>`.

#### Full Response JSON Example

```json
{
  "status": 201,
  "message": "Receipt voucher created",
  "data": {
    "id": "VCH-A1B2C3D4",
    "voucherNumber": "RCP-2026-0001",
    "voucherType": "RECEIPT",
    "voucherDate": "2026-04-13",
    "branchId": "BR-01",
    "partyType": "CUSTOMER",
    "partyId": "CUST-001",
    "paymentMode": "NEFT",
    "referenceNo": "UTR112233",
    "grossAmount": 75000.0,
    "tdsAmount": 0,
    "allocatedAmount": 0,
    "unallocatedAmount": 75000.0,
    "status": "POSTED"
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason      | When It Happens    | Typical Message                                 | Frontend Handling Note |
| ----------- | ----------- | ------------------ | ----------------------------------------------- | ---------------------- |
| 400         | Validation  | Bean validation    | `Input validation failed`                       | Map `validationErrors` |
| 400         | Business    | Allocate > pending | `Allocate amount cannot exceed invoice pending` | Cap rows               |
| 404         | Invoice     | Bad `documentId`   | `Invoice not found: {id}`                       | Validate id            |
| 404         | Ledger      | Bad `bankLedgerId` | `Bank/Cash ledger not found`                    | Pick valid ledger      |
| 400         | Ledger      | No customer ledger | `Customer ledger not found/active`              | Fix customer setup     |
| 400         | CN/DN chain | Ledger / CN rules  | Messages from invoicing service                 | See Module 28          |

#### Error Response JSON Examples

**Validation**

```json
{
  "timestamp": "2026-04-13T10:00:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "Input validation failed",
  "path": "/api/v1/vouchers/receipts",
  "validationErrors": {
    "amountReceived": "must not be null",
    "customerId": "must not be blank"
  }
}
```

**Allocate exceeds pending**

```json
{
  "timestamp": "2026-04-13T10:01:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "Allocate amount cannot exceed invoice pending",
  "path": "/api/v1/vouchers/receipts",
  "validationErrors": null
}
```

**Unauthorized / Forbidden**

```json
{
  "timestamp": "2026-04-13T10:02:00.000",
  "status": 401,
  "error": "Unauthorized",
  "message": "Full authentication is required to access this resource",
  "path": "/api/v1/vouchers/receipts",
  "validationErrors": null
}
```

```json
{
  "timestamp": "2026-04-13T10:03:00.000",
  "status": 403,
  "error": "Forbidden",
  "message": "Access Denied: You don't have permission to access this resource",
  "path": "/api/v1/vouchers/receipts",
  "validationErrors": null
}
```

#### Frontend Notes

- Receipt screen needs **invoice picker** when allocating; **`documentId`** = invoice **primary key** from Module 28.
- **`bankLedgerId`**: treat as required in UI even if DTO omits `@NotBlank`.

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/vouchers/receipts" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001" \
  -H "Content-Type: application/json" \
  -d '{
    "voucherDate": "2026-04-13",
    "branchId": "BR-01",
    "customerId": "CUST-001",
    "paymentMode": "NEFT",
    "bankLedgerId": "LEDGER-BANK-01",
    "referenceNo": "UTR112233",
    "amountReceived": 75000.0
  }'
```

**With allocation**

```bash
curl -X POST "https://{baseUrl}/api/v1/vouchers/receipts" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001" \
  -H "Content-Type: application/json" \
  -d '{
    "voucherDate": "2026-04-13",
    "branchId": "BR-01",
    "customerId": "CUST-001",
    "paymentMode": "NEFT",
    "bankLedgerId": "LEDGER-BANK-01",
    "amountReceived": 50000.0,
    "allocations": [
      {
        "documentId": "INV-A1B2C3D4",
        "allocateAmount": 50000.0
      }
    ]
  }'
```

---

### `POST` `/api/v1/vouchers/payments`

#### Purpose

Creates a **payment** voucher (`PAY-{year}-{seq}`), optionally allocates to **purchase bills**, may trigger **auto debit note** on `SETTLE_CLOSE`, updates bill balances, posts **vendor debit** and **bank credit**.

#### Authorization

- **Token:** Required.
- **Authority:** `PAYMENT_MANAGEMENT_ADD` **or** `CEO`.

#### Request Body Fields — `PaymentCreateRequest`

Same structure as receipt, except:

| Field        | Replaces         | Description    |
| ------------ | ---------------- | -------------- |
| `vendorId`   | `customerId`     | Vendor id      |
| `amountPaid` | `amountReceived` | Total paid out |

#### Full Request JSON Examples

**Minimal Valid Request**

```json
{
  "voucherDate": "2026-04-13",
  "branchId": "BR-01",
  "vendorId": "VEND-001",
  "paymentMode": "NEFT",
  "bankLedgerId": "LEDGER-BANK-01",
  "referenceNo": "UTR778899",
  "amountPaid": 60000.0
}
```

**Complete with bill allocation**

```json
{
  "voucherDate": "2026-04-13",
  "branchId": "BR-01",
  "vendorId": "VEND-001",
  "paymentMode": "NEFT",
  "bankLedgerId": "LEDGER-BANK-01",
  "referenceNo": "UTR778899",
  "amountPaid": 60000.0,
  "tdsAmount": 1200.0,
  "notes": "Against BILL-2026-0003",
  "allocations": [
    {
      "documentId": "BILL-A1B2C3D4",
      "allocateAmount": 60000.0
    }
  ]
}
```

**SETTLE_CLOSE (auto debit note)**

```json
{
  "voucherDate": "2026-04-13",
  "branchId": "BR-01",
  "vendorId": "VEND-001",
  "paymentMode": "NEFT",
  "bankLedgerId": "LEDGER-BANK-01",
  "amountPaid": 30000.0,
  "allocations": [
    {
      "documentId": "BILL-A1B2C3D4",
      "allocateAmount": 30000.0,
      "settlementAction": "SETTLE_CLOSE",
      "settlementReason": "PAYMENT_SETTLEMENT"
    }
  ]
}
```

#### Response

- **201 Created**, `ResponseStructure<VoucherResponse>`.

#### Full Response JSON Example

```json
{
  "status": 201,
  "message": "Payment voucher created",
  "data": {
    "id": "VCH-X9Y8Z7W6",
    "voucherNumber": "PAY-2026-0001",
    "voucherType": "PAYMENT",
    "voucherDate": "2026-04-13",
    "branchId": "BR-01",
    "partyType": "VENDOR",
    "partyId": "VEND-001",
    "paymentMode": "NEFT",
    "referenceNo": "UTR778899",
    "grossAmount": 60000.0,
    "tdsAmount": 1200.0,
    "allocatedAmount": 60000.0,
    "unallocatedAmount": 0,
    "status": "POSTED"
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason             | Typical Message                              |
| ----------- | ------------------ | -------------------------------------------- |
| 400         | Allocate > pending | `Allocate amount cannot exceed bill pending` |
| 404         | Bill               | `Bill not found: {id}`                       |
| 400         | Vendor ledger      | `Vendor ledger not found/active`             |
| 404         | Bank ledger        | `Bank/Cash ledger not found`                 |

#### Error Response JSON Example

```json
{
  "timestamp": "2026-04-13T10:10:00.000",
  "status": 404,
  "error": "Not Found",
  "message": "Bill not found: BILL-UNKNOWN",
  "path": "/api/v1/vouchers/payments",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/vouchers/payments" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001" \
  -H "Content-Type: application/json" \
  -d '{
    "voucherDate": "2026-04-13",
    "branchId": "BR-01",
    "vendorId": "VEND-001",
    "paymentMode": "NEFT",
    "bankLedgerId": "LEDGER-BANK-01",
    "amountPaid": 60000.0
  }'
```

---

### `GET` `/api/v1/vouchers/by-id`

#### Purpose

Returns a single voucher header for detail screens.

#### Authorization

- **Token:** Required.
- **Read:** `PAYMENT_MANAGEMENT_READ` **or** `RECEIPT_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description | Example        |
| ----- | ------ | -------- | ----------- | -------------- |
| `id`  | string | Yes      | Voucher id  | `VCH-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Voucher fetched",
  "data": {
    "id": "VCH-A1B2C3D4",
    "voucherNumber": "RCP-2026-0001",
    "voucherType": "RECEIPT",
    "voucherDate": "2026-04-13",
    "branchId": "BR-01",
    "partyType": "CUSTOMER",
    "partyId": "CUST-001",
    "paymentMode": "NEFT",
    "referenceNo": "UTR112233",
    "grossAmount": 75000.0,
    "tdsAmount": 0,
    "allocatedAmount": 50000.0,
    "unallocatedAmount": 25000.0,
    "status": "POSTED"
  }
}
```

#### Error Response JSON Example

```json
{
  "timestamp": "2026-04-13T10:11:00.000",
  "status": 404,
  "error": "Not Found",
  "message": "Voucher not found",
  "path": "/api/v1/vouchers/by-id",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/vouchers/by-id?id=VCH-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/vouchers`

#### Purpose

Paginated voucher register, sort **`voucherDate` DESC**, **`createdAt` DESC**. **Filters declared on controller are not used** in the service.

#### Authorization

- Same as read above.

#### Query Parameters

| Field         | Type   | Required          | Description | Example    | Notes                            |
| ------------- | ------ | ----------------- | ----------- | ---------- | -------------------------------- |
| `pageNo`      | int    | No (default `0`)  | Page        | `0`        |                                  |
| `pageSize`    | int    | No (default `10`) | Size        | `20`       |                                  |
| `voucherType` | string | No                | **Ignored** | `RECEIPT`  | **Missing from backend context** |
| `partyId`     | string | No                | **Ignored** | `CUST-001` | **Missing from backend context** |
| `paymentMode` | string | No                | **Ignored** | `NEFT`     | **Missing from backend context** |
| `search`      | string | No                | **Ignored** | `RCP`      | **Missing from backend context** |

#### Request Body

**Not applicable.**

#### Full Response JSON Example (with data)

```json
{
  "status": 200,
  "message": "Vouchers fetched",
  "data": {
    "count": 35,
    "next": "https://api.example.com/api/v1/vouchers?pageSize=10&pageNo=1",
    "prev": null,
    "data": [
      {
        "id": "VCH-A1B2C3D4",
        "voucherNumber": "RCP-2026-0001",
        "voucherType": "RECEIPT",
        "voucherDate": "2026-04-13",
        "branchId": "BR-01",
        "partyType": "CUSTOMER",
        "partyId": "CUST-001",
        "paymentMode": "NEFT",
        "referenceNo": "UTR112233",
        "grossAmount": 75000.0,
        "tdsAmount": 0,
        "allocatedAmount": 50000.0,
        "unallocatedAmount": 25000.0,
        "status": "POSTED"
      }
    ]
  }
}
```

#### Empty page

```json
{
  "status": 200,
  "message": "Vouchers fetched",
  "data": {
    "count": 0,
    "next": null,
    "prev": null,
    "data": []
  }
}
```

#### Search With Filters Example (params no-op server-side)

```http
GET /api/v1/vouchers?pageNo=0&pageSize=20&voucherType=RECEIPT&partyId=CUST-001&paymentMode=NEFT&search=RCP
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/vouchers?pageNo=0&pageSize=20" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/vouchers/summary`

#### Purpose

Aggregates **POSTED** vouchers: total receipts, total payments, net cash flow, unallocated advance (sum of **unallocatedAmount** on **receipt** vouchers only, where &gt; 0).

#### Authorization

- Same read.

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Voucher summary fetched",
  "data": {
    "totalReceipts": 2500000.0,
    "totalPayments": 1800000.0,
    "netCashFlow": 700000.0,
    "unallocatedAdvance": 45000.0
  }
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/vouchers/summary" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/vouchers/allocations`

#### Purpose

Returns allocation audit lines for a voucher (invoice or bill lines with amounts and settlement action).

#### Authorization

- Same read.

#### Query Parameters

| Field       | Type   | Required | Description | Example        |
| ----------- | ------ | -------- | ----------- | -------------- |
| `voucherId` | string | Yes      | Voucher id  | `VCH-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Voucher allocations fetched",
  "data": [
    {
      "documentType": "INVOICE",
      "documentId": "INV-A1B2C3D4",
      "pendingBefore": 60000.0,
      "allocatedAmount": 40000.0,
      "shortfallAmount": 20000.0,
      "settlementAction": "SETTLE_CLOSE",
      "statusAfter": "PAID"
    }
  ]
}
```

#### Empty

```json
{
  "status": 200,
  "message": "Voucher allocations fetched",
  "data": []
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/vouchers/allocations?voucherId=VCH-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `POST` `/api/v1/vouchers/void`

#### Purpose

Sets voucher **`status`** to **`VOID`** if currently **`POSTED`**. **Does not** reverse invoices, bills, or ledger postings (**limitation**).

#### Authorization

- **Token:** Required.
- **Authority:** `PAYMENT_MANAGEMENT_EDIT` **or** `RECEIPT_MANAGEMENT_EDIT` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description | Example        |
| ----- | ------ | -------- | ----------- | -------------- |
| `id`  | string | Yes      | Voucher id  | `VCH-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Voucher voided",
  "data": {
    "id": "VCH-A1B2C3D4",
    "voucherNumber": "RCP-2026-0001",
    "voucherType": "RECEIPT",
    "voucherDate": "2026-04-13",
    "branchId": "BR-01",
    "partyType": "CUSTOMER",
    "partyId": "CUST-001",
    "paymentMode": "NEFT",
    "referenceNo": "UTR112233",
    "grossAmount": 75000.0,
    "tdsAmount": 0,
    "allocatedAmount": 50000.0,
    "unallocatedAmount": 25000.0,
    "status": "VOID"
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason     | Typical Message                     |
| ----------- | ---------- | ----------------------------------- |
| 404         | Unknown id | `Voucher not found`                 |
| 400         | Not posted | `Only posted voucher can be voided` |

#### Error Response JSON Examples

```json
{
  "timestamp": "2026-04-13T10:20:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "Only posted voucher can be voided",
  "path": "/api/v1/vouchers/void",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/vouchers/void?id=VCH-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

## Validation and Exception Summary

| Field / Scenario                | Validation / Rule | Error Type | Frontend Impact       |
| ------------------------------- | ----------------- | ---------- | --------------------- |
| Receipt/payment required fields | Bean validation   | 400        | Inline errors         |
| Allocation > pending            | Service           | 400        | Cap allocation inputs |
| Missing invoice/bill id         | Service           | 404        | Validate references   |
| Bank/cash ledger                | Service           | 404        | Ledger picker         |
| Customer/vendor ledger          | Service           | 400        | Master data fix       |
| CN/DN from SETTLE_CLOSE         | Invoicing/Bills   | 400        | Show upstream message |
| Void non-POSTED                 | Service           | 400        | Disable void          |
| List filters                    | **Ignored**       | —          | Client-side filter    |
| Invalid JSON                    | Global handler    | 400        | Fix payload           |
| Auth                            | Spring            | 401 / 403  | Login / permissions   |

---

## Frontend Integration Notes

- **Dropdowns:** Customers, vendors, bank/cash ledgers, open invoices/bills — **other modules**; this API only accepts ids.
- **Dependent fields:** Allocation rows depend on **selected party** and **document list** from AR/AP modules.
- **Enum / string rendering:** Use `voucherType`, `status`, `partyType`, `documentType` exactly as returned.
- **Readonly:** No edit voucher — only **void** after posting (with limitation understood).
- **Create:** Separate screens for **receipt** vs **payment** (different URLs and **ADD** authorities).
- **List/search:** Server filters **not implemented** — debounce client-side filtering if you load pages.
- **Dates:** ISO **date-only** for `voucherDate`.
- **Files:** No multipart in this controller.
- **Token:** Always send **`Authorization`**; tenant header per deployment.

---

_Generated from `PaymentsController`, `PaymentsServiceImpl`, payment DTOs, `Voucher` entity, and `GlobalExceptionHandler` / `ValidationErrorResponse`._
