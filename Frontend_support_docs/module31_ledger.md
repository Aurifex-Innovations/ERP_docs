# Module 31 – Ledger (Chart of Accounts / Party Ledgers)

## Short Description

Module 31 manages **ledger master data** (codes, names, types, opening balances, links to customers/vendors) and **read-only financial views**: **paginated ledger list**, **ledger statement** (dated lines with running balance), **portfolio summary** (receivables, payables, cash/bank), **per-ledger balance list** (labelled “ageing” in code), and a **rebuild postings** hook (currently a **no-op**). **Posting of journal lines** is performed via **`LedgerService.postEntry`** from other modules (invoicing, bills, vouchers) — **there is no public REST endpoint** to post arbitrary entries from the frontend.

**What frontend developers need to know**

- **Base path:** **`/api/v1/ledgers`**.
- **Authentication:** JWT **`Authorization: Bearer`**. **Tenant:** **`X-Tenant-ID`** — **Assumption:** standard multitenancy for this app (**Missing from backend context** if your deployment differs).
- **Success JSON:** `ResponseStructure<T>` → `{ "status", "message", "data" }`.
- **RBAC:** `LEDGER_MANAGEMENT_*` per operation; **`CEO`** bypasses `@PreAuthorize` where present.
- **List filters** (`ledgerType`, `status`, `search`) are **declared on the controller** but **not passed to `LedgerService.list`** — only **`pageNo`** / **`pageSize`** apply (**Missing from backend context** / wire-up gap).
- **Create** sets **`status = ACTIVE`** always. **`LedgerCreateRequest`** does **not** include `status` — **no REST API in this controller** to set **`INACTIVE`** (**Missing from backend context** for deactivate/reactivate).
- **`POST /ledgers/rebuild-postings`** runs an **empty** method — **placeholder only**; safe to call but does nothing until implemented.
- **`GET /ledgers/summary`** sets **`overdue`** to **`0`** always in the current implementation.
- **Internal posting API:** `postEntry(ledgerId, voucherNo, entryDate, dr, cr, branchId, refType, refId, narration)` uses **`RefType.valueOf(refType)`** — callers must pass a string matching **`RefType`** enum names exactly.

---

## Authorization

### Authentication type

- **Bearer JWT** (`Authorization: Bearer <access_token>`).

### Required token

- **Yes** for all endpoints in `LedgerController`.

### Required roles / authorities / permissions

| Operation                                       | Authority (or `CEO`)                       |
| ----------------------------------------------- | ------------------------------------------ |
| `POST /ledgers`                                 | `LEDGER_MANAGEMENT_ADD`                    |
| `PUT /ledgers/update`                           | `LEDGER_MANAGEMENT_EDIT`                   |
| `GET` (list, by-id, statement, summary, ageing) | `LEDGER_MANAGEMENT_READ`                   |
| `POST /ledgers/rebuild-postings`                | `LEDGER_MANAGEMENT_EDIT`                   |
| **CEO**                                         | `hasRole('CEO')` satisfies the same checks |

### Required headers

| Header          | Required                | Description        |
| --------------- | ----------------------- | ------------------ |
| `Authorization` | Yes                     | Bearer token       |
| `X-Tenant-ID`   | **Assumption:** Yes     | Tenant schema      |
| `Content-Type`  | Yes for POST/PUT bodies | `application/json` |

### Tenant / schema-specific behavior

- **Assumption:** Ledgers and `ledger_entries` are tenant-scoped.

### Access restrictions by business status

| Condition                           | Effect                                              |
| ----------------------------------- | --------------------------------------------------- |
| **Internal `postEntry`** (not REST) | Fails with **400** if ledger **`status != ACTIVE`** |

REST layer does not expose `postEntry`; document for integration awareness.

---

## Enums Used In This Module

### LedgerType

**Where used:** `LedgerCreateRequest.ledgerType`, `LedgerResponse.ledgerType`, `Ledger.entity`, `LedgerServiceImpl.summary()` / `ageing()` (string compare on `name()`).

| Value      | Meaning              | Frontend notes                                      |
| ---------- | -------------------- | --------------------------------------------------- |
| `CUSTOMER` | Customer (AR) ledger | Often paired with `linkedCustomerId`                |
| `VENDOR`   | Vendor (AP) ledger   | Often paired with `linkedVendorId`                  |
| `BANK`     | Bank account         | Included in **cashAndBank** summary when closing DR |
| `CASH`     | Cash                 | Same as bank for summary                            |
| `INTERNAL` | Internal / GL-style  |                                                     |
| `TAX`      | Tax                  |                                                     |

**JSON:** Serialize as string, e.g. `"ledgerType": "BANK"`.

---

### LedgerStatus

**Where used:** `Ledger.status`, `LedgerResponse.status`.

| Value      | Meaning                        | Used In                                                                                                         |
| ---------- | ------------------------------ | --------------------------------------------------------------------------------------------------------------- |
| `ACTIVE`   | Posting allowed                | Default on **create**                                                                                           |
| `INACTIVE` | Posting blocked by `postEntry` | **Not set** via `LedgerController` / `LedgerCreateRequest` (**Missing from backend context** for lifecycle API) |

---

### BalanceType

**Where used:** `LedgerCreateRequest.openingBalanceType`, `LedgerResponse.openingBalanceType`, opening balance interpretation in `statement` / `computeClosing`.

| Value | Meaning                                                                           |
| ----- | --------------------------------------------------------------------------------- |
| `DR`  | Debit-normal opening                                                              |
| `CR`  | Credit-normal opening (stored opening may be shown as positive with CR semantics) |

---

### RefType

**Where used:** `LedgerEntry.refType`, internal `postEntry` (string must match enum name). **Not** directly in REST DTOs; appears in **statement** as `refType` **string** (`.name()`).

| Value         | Meaning               | Typical source modules |
| ------------- | --------------------- | ---------------------- |
| `INVOICE`     | Sales invoice posting | Module 28              |
| `BILL`        | Purchase bill         | Module 29              |
| `RECEIPT`     | Receipt voucher       | Module 30              |
| `PAYMENT`     | Payment voucher       | Module 30              |
| `CREDIT_NOTE` | Credit note           | Module 28              |
| `DEBIT_NOTE`  | Debit note            | Module 29              |
| `JOURNAL`     | Journal (if used)     | —                      |
| `CONTRA`      | Contra (if used)      | —                      |

**Frontend notes:** Statement lines expose **`refType`** as uppercase enum name.

---

## API List

| Method | Endpoint                           | Purpose                                    | Authorization Required          |
| ------ | ---------------------------------- | ------------------------------------------ | ------------------------------- |
| `POST` | `/api/v1/ledgers`                  | Create ledger                              | `LEDGER_MANAGEMENT_ADD` or CEO  |
| `PUT`  | `/api/v1/ledgers/update`           | Update ledger                              | `LEDGER_MANAGEMENT_EDIT` or CEO |
| `GET`  | `/api/v1/ledgers/by-id`            | Ledger detail                              | `LEDGER_MANAGEMENT_READ` or CEO |
| `GET`  | `/api/v1/ledgers`                  | Paginated ledger list                      | `LEDGER_MANAGEMENT_READ` or CEO |
| `GET`  | `/api/v1/ledgers/statement`        | Ledger statement (lines + running balance) | `LEDGER_MANAGEMENT_READ` or CEO |
| `GET`  | `/api/v1/ledgers/summary`          | AR / AP / cash summary                     | `LEDGER_MANAGEMENT_READ` or CEO |
| `GET`  | `/api/v1/ledgers/ageing`           | Per-ledger balance list                    | `LEDGER_MANAGEMENT_READ` or CEO |
| `POST` | `/api/v1/ledgers/rebuild-postings` | Rebuild hook (no-op)                       | `LEDGER_MANAGEMENT_EDIT` or CEO |

---

## API Details

---

### `POST` `/api/v1/ledgers`

#### Purpose

Creates a ledger with a unique **`ledgerCode`** (case-sensitive storage; duplicate check uses trimmed code). Sets **`status = ACTIVE`**. Use for onboarding a new bank, customer AP/AR mirror, etc.

#### Authorization

- **Token:** Required.
- **Authority:** `LEDGER_MANAGEMENT_ADD` **or** `CEO`.

#### Path Parameters

_Not applicable._

#### Query Parameters

_Not applicable._

#### Request Body Fields — `LedgerCreateRequest`

| Field                | Type    | Required | Validation  | Description                       | Example                | Allowed values                                                |
| -------------------- | ------- | -------- | ----------- | --------------------------------- | ---------------------- | ------------------------------------------------------------- |
| `ledgerCode`         | string  | Yes      | `@NotBlank` | Unique code                       | `"BANK-HDFC-01"`       | Unique per tenant                                             |
| `ledgerName`         | string  | Yes      | `@NotBlank` | Display name                      | `"HDFC Current — Ops"` | —                                                             |
| `accountHeadId`      | string  | Yes      | `@NotBlank` | COA head link                     | `"AH-1000"`            | **Missing from backend context:** head API not in this module |
| `ledgerType`         | enum    | Yes      | `@NotNull`  | See **LedgerType**                | `"BANK"`               | All `LedgerType` values                                       |
| `linkedCustomerId`   | string  | No       | —           | If type CUSTOMER                  | `"CUST-001"`           | —                                                             |
| `linkedVendorId`     | string  | No       | —           | If type VENDOR                    | `"VEND-002"`           | —                                                             |
| `branchId`           | string  | No       | —           | Branch scope                      | `"BR-01"`              | —                                                             |
| `openingBalance`     | decimal | No       | —           | Defaults **0** in service if null | `50000.0`              | —                                                             |
| `openingBalanceType` | enum    | Yes      | `@NotNull`  | DR/CR                             | `"DR"`                 | `DR`, `CR`                                                    |
| `openingAsOn`        | date    | Yes      | `@NotNull`  | Opening date                      | `"2026-04-01"`         | ISO date                                                      |
| `creditLimit`        | decimal | No       | —           | Defaults **0**                    | `100000.0`             | —                                                             |
| `creditPeriodDays`   | integer | No       | —           | Credit days                       | `30`                   | —                                                             |
| `tdsApplicable`      | boolean | No       | —           | Defaults false if null            | `true`                 | —                                                             |
| `tdsSection`         | string  | No       | —           | TDS section                       | `"194C"`               | —                                                             |

#### Full Request JSON Examples

**Minimal Valid Request**

```json
{
  "ledgerCode": "CUST-LED-001",
  "ledgerName": "Customer — Acme Pvt Ltd",
  "accountHeadId": "AH-SUNDRY-DEBTORS",
  "ledgerType": "CUSTOMER",
  "linkedCustomerId": "CUST-001",
  "openingBalanceType": "DR",
  "openingAsOn": "2026-04-01"
}
```

**Complete Valid Request (bank)**

```json
{
  "ledgerCode": "BANK-HDFC-OPS",
  "ledgerName": "HDFC Bank Current — Operations",
  "accountHeadId": "AH-BANK",
  "ledgerType": "BANK",
  "branchId": "BR-MUM-01",
  "openingBalance": 1250000.5,
  "openingBalanceType": "DR",
  "openingAsOn": "2026-04-01",
  "creditLimit": 0,
  "creditPeriodDays": null,
  "tdsApplicable": false,
  "tdsSection": null
}
```

**Vendor ledger**

```json
{
  "ledgerCode": "VEND-LED-044",
  "ledgerName": "Vendor — ABC Suppliers",
  "accountHeadId": "AH-SUNDRY-CREDITORS",
  "ledgerType": "VENDOR",
  "linkedVendorId": "VEND-044",
  "branchId": "BR-01",
  "openingBalance": 0,
  "openingBalanceType": "CR",
  "openingAsOn": "2026-04-01",
  "creditLimit": 500000.0,
  "creditPeriodDays": 45
}
```

**Each LedgerType variation (enum-driven)**

```json
{
  "ledgerCode": "CASH-MAIN",
  "ledgerName": "Main Cash",
  "accountHeadId": "AH-CASH",
  "ledgerType": "CASH",
  "openingBalanceType": "DR",
  "openingAsOn": "2026-04-01"
}
```

```json
{
  "ledgerCode": "GL-INTERNAL-ROUND",
  "ledgerName": "Rounding Difference",
  "accountHeadId": "AH-MISC",
  "ledgerType": "INTERNAL",
  "openingBalanceType": "DR",
  "openingAsOn": "2026-04-01"
}
```

```json
{
  "ledgerCode": "GST-OUTPUT-18",
  "ledgerName": "GST Output 18%",
  "accountHeadId": "AH-TAX",
  "ledgerType": "TAX",
  "openingBalanceType": "CR",
  "openingAsOn": "2026-04-01"
}
```

#### Response

- **201 Created**, `ResponseStructure<LedgerResponse>`.

#### Full Response JSON Example

```json
{
  "status": 201,
  "message": "Ledger created",
  "data": {
    "id": "LED-A1B2C3D4",
    "ledgerCode": "CUST-LED-001",
    "ledgerName": "Customer — Acme Pvt Ltd",
    "accountHeadId": "AH-SUNDRY-DEBTORS",
    "ledgerType": "CUSTOMER",
    "linkedCustomerId": "CUST-001",
    "linkedVendorId": null,
    "branchId": null,
    "openingBalance": 0,
    "openingBalanceType": "DR",
    "openingAsOn": "2026-04-01",
    "creditLimit": 0,
    "creditPeriodDays": null,
    "tdsApplicable": false,
    "tdsSection": null,
    "status": "ACTIVE"
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason         | When It Happens          | Typical Message              | Frontend Handling Note |
| ----------- | -------------- | ------------------------ | ---------------------------- | ---------------------- |
| 400         | Validation     | Bean validation          | `Input validation failed`    | Field map              |
| 409         | Duplicate code | Same `ledgerCode` exists | `Ledger code already exists` | Change code            |
| 401 / 403   | Auth           | Missing role             | Standard                     | —                      |

#### Error Response JSON Examples

**Validation**

```json
{
  "timestamp": "2026-04-13T10:00:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "Input validation failed",
  "path": "/api/v1/ledgers",
  "validationErrors": {
    "ledgerCode": "must not be blank",
    "openingAsOn": "must not be null"
  }
}
```

**Conflict (duplicate code)**

```json
{
  "timestamp": "2026-04-13T10:01:00.000",
  "status": 409,
  "error": "Conflict",
  "message": "Ledger code already exists",
  "path": "/api/v1/ledgers",
  "validationErrors": null
}
```

**Unauthorized**

```json
{
  "timestamp": "2026-04-13T10:02:00.000",
  "status": 401,
  "error": "Unauthorized",
  "message": "Full authentication is required to access this resource",
  "path": "/api/v1/ledgers",
  "validationErrors": null
}
```

**Forbidden**

```json
{
  "timestamp": "2026-04-13T10:03:00.000",
  "status": 403,
  "error": "Forbidden",
  "message": "Access Denied: You don't have permission to access this resource",
  "path": "/api/v1/ledgers",
  "validationErrors": null
}
```

#### Frontend Notes

- Enforce **unique `ledgerCode`** in UI with server validation on save.
- **`accountHeadId`** must reference a valid COA head (**Missing from backend context** in this repo excerpt).

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/ledgers" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001" \
  -H "Content-Type: application/json" \
  -d '{
    "ledgerCode": "CUST-LED-001",
    "ledgerName": "Customer — Acme Pvt Ltd",
    "accountHeadId": "AH-SUNDRY-DEBTORS",
    "ledgerType": "CUSTOMER",
    "linkedCustomerId": "CUST-001",
    "openingBalanceType": "DR",
    "openingAsOn": "2026-04-01"
  }'
```

---

### `PUT` `/api/v1/ledgers/update`

#### Purpose

Updates ledger fields. If **`ledgerCode`** changes, new code must not conflict. **`status`** is **not** updated from this request (preserves existing).

#### Authorization

- **Token:** Required.
- **Authority:** `LEDGER_MANAGEMENT_EDIT` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description | Example        |
| ----- | ------ | -------- | ----------- | -------------- |
| `id`  | string | Yes      | Ledger id   | `LED-A1B2C3D4` |

#### Request Body

Same as **`POST /ledgers`** (`LedgerCreateRequest`).

#### Full Request JSON Examples

**Update Request Example (rename + credit terms)**

```json
{
  "ledgerCode": "CUST-LED-001",
  "ledgerName": "Customer — Acme Pvt Ltd (HQ)",
  "accountHeadId": "AH-SUNDRY-DEBTORS",
  "ledgerType": "CUSTOMER",
  "linkedCustomerId": "CUST-001",
  "branchId": "BR-01",
  "openingBalance": 0,
  "openingBalanceType": "DR",
  "openingAsOn": "2026-04-01",
  "creditLimit": 200000.0,
  "creditPeriodDays": 60,
  "tdsApplicable": true,
  "tdsSection": "194C"
}
```

**Rename ledger code (must stay unique)**

```json
{
  "ledgerCode": "CUST-LED-001-REV",
  "ledgerName": "Customer — Acme Pvt Ltd",
  "accountHeadId": "AH-SUNDRY-DEBTORS",
  "ledgerType": "CUSTOMER",
  "linkedCustomerId": "CUST-001",
  "openingBalanceType": "DR",
  "openingAsOn": "2026-04-01"
}
```

#### Response

- **200 OK**, `ResponseStructure<LedgerResponse>`.

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Ledger updated",
  "data": {
    "id": "LED-A1B2C3D4",
    "ledgerCode": "CUST-LED-001",
    "ledgerName": "Customer — Acme Pvt Ltd (HQ)",
    "accountHeadId": "AH-SUNDRY-DEBTORS",
    "ledgerType": "CUSTOMER",
    "linkedCustomerId": "CUST-001",
    "linkedVendorId": null,
    "branchId": "BR-01",
    "openingBalance": 0,
    "openingBalanceType": "DR",
    "openingAsOn": "2026-04-01",
    "creditLimit": 200000.0,
    "creditPeriodDays": 60,
    "tdsApplicable": true,
    "tdsSection": "194C",
    "status": "ACTIVE"
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason     | Typical Message              |
| ----------- | ---------- | ---------------------------- |
| 404         | Unknown id | `Ledger not found`           |
| 409         | Code taken | `Ledger code already exists` |
| 400         | Validation | `Input validation failed`    |

#### Error Response JSON Example (not found)

```json
{
  "timestamp": "2026-04-13T10:05:00.000",
  "status": 404,
  "error": "Not Found",
  "message": "Ledger not found",
  "path": "/api/v1/ledgers/update",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X PUT "https://{baseUrl}/api/v1/ledgers/update?id=LED-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001" \
  -H "Content-Type: application/json" \
  -d '{
    "ledgerCode": "CUST-LED-001",
    "ledgerName": "Customer — Acme Pvt Ltd (HQ)",
    "accountHeadId": "AH-SUNDRY-DEBTORS",
    "ledgerType": "CUSTOMER",
    "linkedCustomerId": "CUST-001",
    "openingBalanceType": "DR",
    "openingAsOn": "2026-04-01"
  }'
```

---

### `GET` `/api/v1/ledgers/by-id`

#### Purpose

Returns ledger master for detail/edit forms.

#### Authorization

- **Token:** Required.
- **Authority:** `LEDGER_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description | Example        |
| ----- | ------ | -------- | ----------- | -------------- |
| `id`  | string | Yes      | Ledger id   | `LED-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Ledger fetched",
  "data": {
    "id": "LED-A1B2C3D4",
    "ledgerCode": "BANK-HDFC-OPS",
    "ledgerName": "HDFC Bank Current — Operations",
    "accountHeadId": "AH-BANK",
    "ledgerType": "BANK",
    "linkedCustomerId": null,
    "linkedVendorId": null,
    "branchId": "BR-MUM-01",
    "openingBalance": 1250000.5,
    "openingBalanceType": "DR",
    "openingAsOn": "2026-04-01",
    "creditLimit": 0,
    "creditPeriodDays": null,
    "tdsApplicable": false,
    "tdsSection": null,
    "status": "ACTIVE"
  }
}
```

#### Error Response JSON Example

```json
{
  "timestamp": "2026-04-13T10:06:00.000",
  "status": 404,
  "error": "Not Found",
  "message": "Ledger not found",
  "path": "/api/v1/ledgers/by-id",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/ledgers/by-id?id=LED-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/ledgers`

#### Purpose

Paginated list of ledgers, sorted **`updatedAt` DESC**, **`createdAt` DESC**. **Query filters are not applied** in the service.

#### Authorization

- **Token:** Required.
- **Authority:** `LEDGER_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field        | Type   | Required          | Description | Example  | Notes                            |
| ------------ | ------ | ----------------- | ----------- | -------- | -------------------------------- |
| `pageNo`     | int    | No (default `0`)  | Page        | `0`      |                                  |
| `pageSize`   | int    | No (default `10`) | Size        | `25`     |                                  |
| `ledgerType` | string | No                | **Ignored** | `BANK`   | **Missing from backend context** |
| `status`     | string | No                | **Ignored** | `ACTIVE` | **Missing from backend context** |
| `search`     | string | No                | **Ignored** | `HDFC`   | **Missing from backend context** |

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Ledgers fetched",
  "data": {
    "count": 48,
    "next": "https://api.example.com/api/v1/ledgers?pageSize=10&pageNo=1",
    "prev": null,
    "data": [
      {
        "id": "LED-A1B2C3D4",
        "ledgerCode": "BANK-HDFC-OPS",
        "ledgerName": "HDFC Bank Current — Operations",
        "accountHeadId": "AH-BANK",
        "ledgerType": "BANK",
        "linkedCustomerId": null,
        "linkedVendorId": null,
        "branchId": "BR-MUM-01",
        "openingBalance": 1250000.5,
        "openingBalanceType": "DR",
        "openingAsOn": "2026-04-01",
        "creditLimit": 0,
        "creditPeriodDays": null,
        "tdsApplicable": false,
        "tdsSection": null,
        "status": "ACTIVE"
      }
    ]
  }
}
```

#### Empty list

```json
{
  "status": 200,
  "message": "Ledgers fetched",
  "data": {
    "count": 0,
    "next": null,
    "prev": null,
    "data": []
  }
}
```

#### Search With Filters Example (no server filter)

```http
GET /api/v1/ledgers?pageNo=0&pageSize=20&ledgerType=BANK&status=ACTIVE&search=HDFC
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/ledgers?pageNo=0&pageSize=25" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/ledgers/statement`

#### Purpose

Returns **ledger entries** between **`from`** and **`inclusive`** **`to`** for **`ledgerId`**, ordered by date, with **running balance** per line. Opening position starts from ledger’s **opening balance** (CR treated as negative in the running calculation; **absolute** value and **DR/CR** label on each line).

#### Authorization

- **Token:** Required.
- **Authority:** `LEDGER_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field      | Type   | Required | Description            | Example        |
| ---------- | ------ | -------- | ---------------------- | -------------- |
| `ledgerId` | string | Yes      | Ledger id              | `LED-A1B2C3D4` |
| `from`     | date   | Yes      | Start date (inclusive) | `2026-04-01`   |
| `to`       | date   | Yes      | End date (inclusive)   | `2026-04-30`   |

#### Request Body

**Not applicable.**

#### Response

- **200 OK**, `ResponseStructure<List<LedgerStatementLineResponse>>`
- Lines may be **empty** if no entries in range (running balance loop still starts from opening).

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Statement fetched",
  "data": [
    {
      "entryDate": "2026-04-05",
      "voucherNo": "INV-2026-0001",
      "refType": "INVOICE",
      "refId": "INV-A1B2C3D4",
      "drAmount": 100000.0,
      "crAmount": 0,
      "runningBalance": 100000.0,
      "runningBalanceType": "DR",
      "narration": "Invoice approved"
    },
    {
      "entryDate": "2026-04-10",
      "voucherNo": "RCP-2026-0002",
      "refType": "RECEIPT",
      "refId": "VCH-X1Y2Z3W4",
      "drAmount": 0,
      "crAmount": 40000.0,
      "runningBalance": 60000.0,
      "runningBalanceType": "DR",
      "narration": "Receipt entry"
    }
  ]
}
```

#### Empty statement (no lines in range)

```json
{
  "status": 200,
  "message": "Statement fetched",
  "data": []
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason | Typical Message    |
| ----------- | ------ | ------------------ |
| 404         | Ledger | `Ledger not found` |

#### Error Response JSON Example

```json
{
  "timestamp": "2026-04-13T10:10:00.000",
  "status": 404,
  "error": "Not Found",
  "message": "Ledger not found",
  "path": "/api/v1/ledgers/statement",
  "validationErrors": null
}
```

#### Frontend Notes

- **`from` > `to`** is **not** validated in code — **Assumption:** enforce in UI.
- **`refType`** values match **`RefType`** enum names.

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/ledgers/statement?ledgerId=LED-A1B2C3D4&from=2026-04-01&to=2026-04-30" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/ledgers/summary`

#### Purpose

Roll-up: **totalReceivable** (CUSTOMER ledgers with positive closing), **totalPayable** (VENDOR with negative closing → absolute), **cashAndBank** (BANK/CASH with positive closing), **`overdue`** is **always `0`** in current code.

#### Authorization

- **Token:** Required.
- **Authority:** `LEDGER_MANAGEMENT_READ` **or** `CEO`.

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Ledger summary fetched",
  "data": {
    "totalReceivable": 450000.0,
    "totalPayable": 320000.0,
    "cashAndBank": 2100000.0,
    "overdue": 0
  }
}
```

**Assumption:** Do not build an “overdue” KPI from this field until backend populates it.

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/ledgers/summary" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/ledgers/ageing`

#### Purpose

Returns **one row per ledger** with **absolute** balance and **DR/CR** indicator (not bucketed ageing). Naming is **historical** — treat as **balance list** unless backend adds buckets later.

#### Authorization

- **Token:** Required.
- **Authority:** `LEDGER_MANAGEMENT_READ` **or** `CEO`.

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Ledger ageing fetched",
  "data": [
    {
      "ledgerId": "LED-A1B2C3D4",
      "ledgerName": "Customer — Acme Pvt Ltd",
      "ledgerType": "CUSTOMER",
      "balance": 60000.0,
      "balanceType": "DR"
    },
    {
      "ledgerId": "LED-B2C3D4E5",
      "ledgerName": "Vendor — ABC Suppliers",
      "ledgerType": "VENDOR",
      "balance": 25000.0,
      "balanceType": "CR"
    }
  ]
}
```

#### Empty tenant (no ledgers)

```json
{
  "status": 200,
  "message": "Ledger ageing fetched",
  "data": []
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/ledgers/ageing" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `POST` `/api/v1/ledgers/rebuild-postings`

#### Purpose

Reserved for rebuilding derived data. **Current implementation is empty** — returns **200** with **`data: null`** and message success.

#### Authorization

- **Token:** Required.
- **Authority:** `LEDGER_MANAGEMENT_EDIT` **or** `CEO`.

#### Path / Query

_Not applicable._

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Posting rebuild completed",
  "data": null
}
```

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/ledgers/rebuild-postings" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

## Validation and Exception Summary

| Field / Scenario                                      | Validation / Rule             | Error Type | Frontend Impact |
| ----------------------------------------------------- | ----------------------------- | ---------- | --------------- |
| `ledgerCode` / `ledgerName` / `accountHeadId` / enums | Bean validation               | 400        | Inline errors   |
| Duplicate `ledgerCode` on create/update               | Service                       | 409        | Show conflict   |
| Ledger not found                                      | Service                       | 404        | Not found       |
| List filters                                          | **Ignored**                   | —          | Client filter   |
| Statement invalid ledger                              | Service                       | 404        | —               |
| **`postEntry` (internal)**                            | Inactive ledger / no DR or CR | 400        | N/A to REST     |

---

## Frontend Integration Notes

- **Account heads:** `accountHeadId` must exist in your COA — **dropdown API not in this module** (**Missing from backend context**).
- **Customer/vendor link:** Set `linkedCustomerId` / `linkedVendorId` when type is CUSTOMER/VENDOR for AR/AP and voucher matching (see Modules 28–30).
- **Enum rendering:** Use **`LedgerType`**, **`LedgerStatus`**, **`BalanceType`** as returned (JSON strings).
- **Readonly / hidden:** **`status`** is not editable via update payload — **INACTIVE** cannot be set from this API (**Missing from backend context**).
- **Create vs update:** Same body schema; update requires **`id`** query param.
- **Statement:** Pick **`from`/`to`**; optionally disable inverted ranges client-side.
- **Summary:** **`overdue`** is always zero — use Module 28/29 overdue if needed.
- **Ageing:** Display as balance grid, not aged buckets unless product redefines.
- **Posting entries:** No REST — flows go through invoices, bills, vouchers.
- **Headers / token:** Send **`Authorization`** and tenant header on every call.

---

_Generated from `LedgerController`, `LedgerServiceImpl`, ledger DTOs/entities/enums, and `GlobalExceptionHandler` / `ValidationErrorResponse` patterns._
