# Module 32 – COA (Chart of Accounts)

## Short Description

Module 32 manages **COA account heads** — the structural layer **above ledgers** (group, nature, parent, postable flag, branch scope). It supports **create/update**, **paginated list**, **postable heads dropdown** (for linking new ledgers in Module 31), **used-by ledgers** (which ledgers reference a head), **activate/inactivate**, and a **stub export** string. Account heads are identified by a unique **`code`** and are created with **`status = ACTIVE`** by default.

**What frontend developers need to know**

- **Base path:** **`/api/v1/coa`** — account head resources live under **`/heads`** and **`/dropdowns`**.
- **Authentication:** JWT **`Authorization: Bearer`**. **Tenant:** **`X-Tenant-ID`** — **Assumption:** same multitenancy as other finance APIs (**Missing from backend context** if your deployment differs).
- **Success JSON:** `ResponseStructure<T>` → `{ "status", "message", "data" }`.
- **RBAC:** `CHART_OF_ACCOUNTS_MANAGEMENT_*` per operation; **`CEO`** bypasses `@PreAuthorize`.
- **List filters** (`primaryGroup`, `status`, `search`) are **declared** on `GET /coa/heads` but **not passed to `CoaService.list`** — only **`pageNo`** / **`pageSize`** apply (**Missing from backend context** / wire-up gap). Sort order is **`code` ASC**.
- **`GET /coa/export`** returns a **placeholder** string, not a file download.
- **`branchScope`** defaults to **`"ALL"`** when omitted on create/update.
- **`isPostable`** on **update:** if the client sends **`null`**, the service **does not change** the existing postable flag (**Assumption:** always send explicit boolean when toggling).

---

## Authorization

### Authentication type

- **Bearer JWT** (`Authorization: Bearer <access_token>`).

### Required token

- **Yes** for all endpoints in `CoaController`.

### Required roles / authorities / permissions

| Operation                                                           | Authority (or `CEO`)                  |
| ------------------------------------------------------------------- | ------------------------------------- |
| `POST /coa/heads`                                                   | `CHART_OF_ACCOUNTS_MANAGEMENT_ADD`    |
| `PUT /coa/heads/update`, `POST .../activate`, `POST .../inactivate` | `CHART_OF_ACCOUNTS_MANAGEMENT_EDIT`   |
| `GET` (heads, by-id, list, dropdown, used-by)                       | `CHART_OF_ACCOUNTS_MANAGEMENT_READ`   |
| `GET /coa/export`                                                   | `CHART_OF_ACCOUNTS_MANAGEMENT_EXPORT` |

### Required headers

| Header          | Required                | Description        |
| --------------- | ----------------------- | ------------------ |
| `Authorization` | Yes                     | Bearer token       |
| `X-Tenant-ID`   | **Assumption:** Yes     | Tenant schema      |
| `Content-Type`  | Yes for POST/PUT bodies | `application/json` |

### Tenant / schema-specific behavior

- **Assumption:** `coa_account_heads` and related ledgers are tenant-scoped.

### Access restrictions

- **None** beyond RBAC — activate/inactivate do not check child ledgers in service (**Assumption:** business may want to block inactivation when ledgers exist; **Missing from backend context**).

---

## Enums Used In This Module

### CoaPrimaryGroup

**Where used:** `CoaAccountHeadRequest.primaryGroup`, `CoaAccountHeadResponse.primaryGroup`, entity.

| Value       | Meaning          | Frontend notes |
| ----------- | ---------------- | -------------- |
| `ASSET`     | Assets           | Balance sheet  |
| `LIABILITY` | Liabilities      | Balance sheet  |
| `INCOME`    | Income / revenue | P&amp;L        |
| `EXPENSE`   | Expenses         | P&amp;L        |
| `CAPITAL`   | Capital / equity | Balance sheet  |

---

### CoaNature

**Where used:** `CoaAccountHeadRequest.nature`, `CoaAccountHeadResponse.nature`, entity.

| Value | Meaning               |
| ----- | --------------------- |
| `DR`  | Debit-nature account  |
| `CR`  | Credit-nature account |

**Note:** Distinct from ledger **`BalanceType`** in Module 31 — same DR/CR labels, different domain (COA head vs ledger opening).

---

### CoaStatus

**Where used:** `CoaAccountHead.status`, `CoaAccountHeadResponse.status`; `postableHeads()` filters **`ACTIVE`**.

| Value      | Meaning       | Used In                           |
| ---------- | ------------- | --------------------------------- |
| `ACTIVE`   | Head usable   | Default on create; after activate |
| `INACTIVE` | Head inactive | After inactivate                  |

**Frontend notes:** **Postable dropdown** returns only **`ACTIVE`** and **`isPostable == true`**.

---

## API List

| Method | Endpoint                               | Purpose                      | Authorization Required |
| ------ | -------------------------------------- | ---------------------------- | ---------------------- |
| `POST` | `/api/v1/coa/heads`                    | Create account head          | `ADD` or CEO           |
| `PUT`  | `/api/v1/coa/heads/update`             | Update account head          | `EDIT` or CEO          |
| `GET`  | `/api/v1/coa/heads/by-id`              | Head detail                  | `READ` or CEO          |
| `GET`  | `/api/v1/coa/heads`                    | Paginated list               | `READ` or CEO          |
| `GET`  | `/api/v1/coa/dropdowns/postable-heads` | Postable heads for dropdowns | `READ` or CEO          |
| `GET`  | `/api/v1/coa/heads/used-by-ledgers`    | Ledgers using this head      | `READ` or CEO          |
| `POST` | `/api/v1/coa/heads/activate`           | Set status ACTIVE            | `EDIT` or CEO          |
| `POST` | `/api/v1/coa/heads/inactivate`         | Set status INACTIVE          | `EDIT` or CEO          |
| `GET`  | `/api/v1/coa/export`                   | Export placeholder           | `EXPORT` or CEO        |

---

## API Details

---

### `POST` `/api/v1/coa/heads`

#### Purpose

Creates an account head with server id `COA-{uuid}`, unique **`code`**, default **`branchScope = "ALL"`** if omitted, **`isPostable` default true** if null, **`status = ACTIVE`**.

#### Authorization

- **Token:** Required.
- **Authority:** `CHART_OF_ACCOUNTS_MANAGEMENT_ADD` **or** `CEO`.

#### Path Parameters

_Not applicable._

#### Query Parameters

_Not applicable._

#### Request Body Fields — `CoaAccountHeadRequest`

| Field          | Type    | Required | Validation  | Description                   | Example                  | Allowed values                     |
| -------------- | ------- | -------- | ----------- | ----------------------------- | ------------------------ | ---------------------------------- |
| `code`         | string  | Yes      | `@NotBlank` | Unique code                   | `"1000-SD"`              | Unique                             |
| `name`         | string  | Yes      | `@NotBlank` | Display name                  | `"Sundry Debtors"`       | —                                  |
| `primaryGroup` | enum    | Yes      | `@NotNull`  | Primary group                 | `"ASSET"`                | All **CoaPrimaryGroup**            |
| `parentHeadId` | string  | No       | —           | Parent COA id                 | `"COA-PARENT01"`         | —                                  |
| `nature`       | enum    | Yes      | `@NotNull`  | DR/CR nature                  | `"DR"`                   | `DR`, `CR`                         |
| `branchScope`  | string  | No       | —           | Defaults **`ALL`** in service | `"ALL"` or branch-scoped | **Assumption:** product vocabulary |
| `branchId`     | string  | No       | —           | If scoped to branch           | `"BR-01"`                | —                                  |
| `isPostable`   | boolean | No       | —           | Default **true** if null      | `true`                   | —                                  |

#### Full Request JSON Examples

**Minimal Valid Request**

```json
{
  "code": "4000-SALES",
  "name": "Sales Account",
  "primaryGroup": "INCOME",
  "nature": "CR"
}
```

**Complete Valid Request**

```json
{
  "code": "1000-SD",
  "name": "Sundry Debtors",
  "primaryGroup": "ASSET",
  "parentHeadId": "COA-1000-CA",
  "nature": "DR",
  "branchScope": "ALL",
  "branchId": null,
  "isPostable": true
}
```

**Update Request Example** — _use `PUT /heads/update`; shown here as reference for full payload shape_

```json
{
  "code": "2000-SC",
  "name": "Sundry Creditors",
  "primaryGroup": "LIABILITY",
  "parentHeadId": null,
  "nature": "CR",
  "branchScope": "ALL",
  "isPostable": true
}
```

**Each CoaPrimaryGroup value (enum-driven)**

```json
{
  "code": "AST-CASH",
  "name": "Cash in Hand",
  "primaryGroup": "ASSET",
  "nature": "DR"
}
```

```json
{
  "code": "LIA-TDS",
  "name": "TDS Payable",
  "primaryGroup": "LIABILITY",
  "nature": "CR"
}
```

```json
{
  "code": "INC-SERV",
  "name": "Service Income",
  "primaryGroup": "INCOME",
  "nature": "CR"
}
```

```json
{
  "code": "EXP-RENT",
  "name": "Rent Expense",
  "primaryGroup": "EXPENSE",
  "nature": "DR"
}
```

```json
{
  "code": "CAP-EQ",
  "name": "Owner Equity",
  "primaryGroup": "CAPITAL",
  "nature": "CR"
}
```

**Non-postable group head (container)**

```json
{
  "code": "1000-CA",
  "name": "Current Assets (Group)",
  "primaryGroup": "ASSET",
  "nature": "DR",
  "isPostable": false
}
```

#### Response

- **201 Created**, `ResponseStructure<CoaAccountHeadResponse>`.

#### Full Response JSON Example

```json
{
  "status": 201,
  "message": "COA head created",
  "data": {
    "id": "COA-A1B2C3D4",
    "code": "1000-SD",
    "name": "Sundry Debtors",
    "primaryGroup": "ASSET",
    "parentHeadId": "COA-1000-CA",
    "nature": "DR",
    "branchScope": "ALL",
    "branchId": null,
    "isPostable": true,
    "status": "ACTIVE"
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason     | When It Happens | Typical Message           | Frontend Handling Note |
| ----------- | ---------- | --------------- | ------------------------- | ---------------------- |
| 400         | Validation | Bean validation | `Input validation failed` | Field errors           |
| 409         | Duplicate  | Code exists     | `COA code already exists` | Change code            |
| 401 / 403   | Auth       | —               | Standard                  | —                      |

#### Error Response JSON Examples

**Validation**

```json
{
  "timestamp": "2026-04-13T10:00:00.000",
  "status": 400,
  "error": "Bad Request",
  "message": "Input validation failed",
  "path": "/api/v1/coa/heads",
  "validationErrors": {
    "code": "must not be blank",
    "primaryGroup": "must not be null"
  }
}
```

**Conflict**

```json
{
  "timestamp": "2026-04-13T10:01:00.000",
  "status": 409,
  "error": "Conflict",
  "message": "COA code already exists",
  "path": "/api/v1/coa/heads",
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
  "path": "/api/v1/coa/heads",
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
  "path": "/api/v1/coa/heads",
  "validationErrors": null
}
```

#### Frontend Notes

- **`code`** is unique — validate uniqueness on blur + handle 409.
- **`parentHeadId`** must reference another head id if hierarchical UI is used (**Missing from backend context:** no tree endpoint).

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/coa/heads" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001" \
  -H "Content-Type: application/json" \
  -d '{
    "code": "1000-SD",
    "name": "Sundry Debtors",
    "primaryGroup": "ASSET",
    "nature": "DR",
    "isPostable": true
  }'
```

---

### `PUT` `/api/v1/coa/heads/update`

#### Purpose

Updates head fields. If **`code`** changes, new code must be unique. **`status`** is **not** in the request — use activate/inactivate to change lifecycle.

#### Authorization

- **Token:** Required.
- **Authority:** `CHART_OF_ACCOUNTS_MANAGEMENT_EDIT` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description | Example        |
| ----- | ------ | -------- | ----------- | -------------- |
| `id`  | string | Yes      | COA head id | `COA-A1B2C3D4` |

#### Request Body

Same as **`CoaAccountHeadRequest`**.

#### Full Request JSON Example

```json
{
  "code": "1000-SD",
  "name": "Sundry Debtors — Trade",
  "primaryGroup": "ASSET",
  "parentHeadId": "COA-1000-CA",
  "nature": "DR",
  "branchScope": "ALL",
  "branchId": null,
  "isPostable": true
}
```

#### Response

- **200 OK**, `ResponseStructure<CoaAccountHeadResponse>`.

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "COA head updated",
  "data": {
    "id": "COA-A1B2C3D4",
    "code": "1000-SD",
    "name": "Sundry Debtors — Trade",
    "primaryGroup": "ASSET",
    "parentHeadId": "COA-1000-CA",
    "nature": "DR",
    "branchScope": "ALL",
    "branchId": null,
    "isPostable": true,
    "status": "ACTIVE"
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason       | Typical Message           |
| ----------- | ------------ | ------------------------- |
| 404         | Missing head | `COA head not found`      |
| 409         | Code taken   | `COA code already exists` |

#### Error Response JSON Example

```json
{
  "timestamp": "2026-04-13T10:05:00.000",
  "status": 404,
  "error": "Not Found",
  "message": "COA head not found",
  "path": "/api/v1/coa/heads/update",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X PUT "https://{baseUrl}/api/v1/coa/heads/update?id=COA-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001" \
  -H "Content-Type: application/json" \
  -d '{
    "code": "1000-SD",
    "name": "Sundry Debtors — Trade",
    "primaryGroup": "ASSET",
    "nature": "DR",
    "isPostable": true
  }'
```

---

### `GET` `/api/v1/coa/heads/by-id`

#### Purpose

Returns one account head for detail/edit.

#### Authorization

- **Token:** Required.
- **Authority:** `CHART_OF_ACCOUNTS_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description | Example        |
| ----- | ------ | -------- | ----------- | -------------- |
| `id`  | string | Yes      | Head id     | `COA-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "COA head fetched",
  "data": {
    "id": "COA-A1B2C3D4",
    "code": "1000-SD",
    "name": "Sundry Debtors",
    "primaryGroup": "ASSET",
    "parentHeadId": null,
    "nature": "DR",
    "branchScope": "ALL",
    "branchId": null,
    "isPostable": true,
    "status": "ACTIVE"
  }
}
```

**Inactive state example**

```json
{
  "status": 200,
  "message": "COA head fetched",
  "data": {
    "id": "COA-X1Y2Z3W4",
    "code": "9999-OLD",
    "name": "Legacy Head",
    "primaryGroup": "EXPENSE",
    "parentHeadId": null,
    "nature": "DR",
    "branchScope": "ALL",
    "branchId": null,
    "isPostable": false,
    "status": "INACTIVE"
  }
}
```

#### Error Response JSON Example

```json
{
  "timestamp": "2026-04-13T10:06:00.000",
  "status": 404,
  "error": "Not Found",
  "message": "COA head not found",
  "path": "/api/v1/coa/heads/by-id",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/coa/heads/by-id?id=COA-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/coa/heads`

#### Purpose

Paginated list of all heads, sorted **`code` ASC**. **Filters on the controller are ignored** by the service.

#### Authorization

- **Token:** Required.
- **Authority:** `CHART_OF_ACCOUNTS_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field          | Type   | Required          | Description | Example  | Notes                            |
| -------------- | ------ | ----------------- | ----------- | -------- | -------------------------------- |
| `pageNo`       | int    | No (default `0`)  | Page        | `0`      |                                  |
| `pageSize`     | int    | No (default `10`) | Size        | `25`     |                                  |
| `primaryGroup` | string | No                | **Ignored** | `ASSET`  | **Missing from backend context** |
| `status`       | string | No                | **Ignored** | `ACTIVE` | **Missing from backend context** |
| `search`       | string | No                | **Ignored** | `1000`   | **Missing from backend context** |

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "COA heads fetched",
  "data": {
    "count": 42,
    "next": "https://api.example.com/api/v1/coa/heads?pageSize=10&pageNo=1",
    "prev": null,
    "data": [
      {
        "id": "COA-A1B2C3D4",
        "code": "1000-SD",
        "name": "Sundry Debtors",
        "primaryGroup": "ASSET",
        "parentHeadId": null,
        "nature": "DR",
        "branchScope": "ALL",
        "branchId": null,
        "isPostable": true,
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
  "message": "COA heads fetched",
  "data": {
    "count": 0,
    "next": null,
    "prev": null,
    "data": []
  }
}
```

#### Search With Filters Example (no server-side filter)

```http
GET /api/v1/coa/heads?pageNo=0&pageSize=20&primaryGroup=ASSET&status=ACTIVE&search=1000
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/coa/heads?pageNo=0&pageSize=25" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/coa/dropdowns/postable-heads`

#### Purpose

Returns **all heads** where **`status == ACTIVE`** and **`isPostable == true`** — intended for **ledger create** (`accountHeadId` in Module 31).

#### Authorization

- **Token:** Required.
- **Authority:** `CHART_OF_ACCOUNTS_MANAGEMENT_READ` **or** `CEO`.

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Postable COA heads",
  "data": [
    {
      "id": "COA-A1B2C3D4",
      "code": "1000-SD",
      "name": "Sundry Debtors",
      "primaryGroup": "ASSET",
      "parentHeadId": null,
      "nature": "DR",
      "branchScope": "ALL",
      "branchId": null,
      "isPostable": true,
      "status": "ACTIVE"
    },
    {
      "id": "COA-B2C3D4E5",
      "code": "4000-SALES",
      "name": "Sales Account",
      "primaryGroup": "INCOME",
      "parentHeadId": null,
      "nature": "CR",
      "branchScope": "ALL",
      "branchId": null,
      "isPostable": true,
      "status": "ACTIVE"
    }
  ]
}
```

#### Empty (no postable active heads)

```json
{
  "status": 200,
  "message": "Postable COA heads",
  "data": []
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/coa/dropdowns/postable-heads" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/coa/heads/used-by-ledgers`

#### Purpose

Lists **ledgers** whose **`accountHeadId`** equals **`headId`** (Module 31 `LedgerResponse` array). Use before delete/inactivate warnings in UI (**Assumption** — service does not block inactivation).

#### Authorization

- **Token:** Required.
- **Authority:** `CHART_OF_ACCOUNTS_MANAGEMENT_READ` **or** `CEO`.

#### Query Parameters

| Field    | Type   | Required | Description | Example        |
| -------- | ------ | -------- | ----------- | -------------- |
| `headId` | string | Yes      | COA head id | `COA-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Used-by ledgers fetched",
  "data": [
    {
      "id": "LED-X1Y2Z3W4",
      "ledgerCode": "CUST-LED-001",
      "ledgerName": "Customer — Acme Pvt Ltd",
      "accountHeadId": "COA-A1B2C3D4",
      "ledgerType": "CUSTOMER",
      "linkedCustomerId": "CUST-001",
      "linkedVendorId": null,
      "branchId": "BR-01",
      "openingBalance": 0,
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
```

#### Empty (no ledgers)

```json
{
  "status": 200,
  "message": "Used-by ledgers fetched",
  "data": []
}
```

#### Error Response JSON Example

```json
{
  "timestamp": "2026-04-13T10:12:00.000",
  "status": 404,
  "error": "Not Found",
  "message": "COA head not found",
  "path": "/api/v1/coa/heads/used-by-ledgers",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/coa/heads/used-by-ledgers?headId=COA-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `POST` `/api/v1/coa/heads/activate`

#### Purpose

Sets **`status = ACTIVE`**.

#### Authorization

- **Token:** Required.
- **Authority:** `CHART_OF_ACCOUNTS_MANAGEMENT_EDIT` **or** `CEO`.

#### Query Parameters

| Field | Type   | Required | Description | Example        |
| ----- | ------ | -------- | ----------- | -------------- |
| `id`  | string | Yes      | Head id     | `COA-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "COA head activated",
  "data": null
}
```

#### Error Response JSON Example

```json
{
  "timestamp": "2026-04-13T10:13:00.000",
  "status": 404,
  "error": "Not Found",
  "message": "COA head not found",
  "path": "/api/v1/coa/heads/activate",
  "validationErrors": null
}
```

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/coa/heads/activate?id=COA-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `POST` `/api/v1/coa/heads/inactivate`

#### Purpose

Sets **`status = INACTIVE`**. **Approved Status Request** / **Rejected** labels do not apply — use activate/inactivate for lifecycle.

#### Authorization

- Same as activate.

#### Query Parameters

| Field | Type   | Required | Description | Example        |
| ----- | ------ | -------- | ----------- | -------------- |
| `id`  | string | Yes      | Head id     | `COA-A1B2C3D4` |

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "COA head inactivated",
  "data": null
}
```

#### cURL

```bash
curl -X POST "https://{baseUrl}/api/v1/coa/heads/inactivate?id=COA-A1B2C3D4" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

### `GET` `/api/v1/coa/export`

#### Purpose

Returns a **placeholder** export message — **not** a file download.

#### Authorization

- **Token:** Required.
- **Authority:** `CHART_OF_ACCOUNTS_MANAGEMENT_EXPORT` **or** `CEO`.

#### Request Body

**Not applicable.**

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "COA export prepared",
  "data": "COA export placeholder"
}
```

#### cURL

```bash
curl -X GET "https://{baseUrl}/api/v1/coa/export" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "X-Tenant-ID: tenant-001"
```

---

## Validation and Exception Summary

| Field / Scenario                         | Validation / Rule | Error Type | Frontend Impact |
| ---------------------------------------- | ----------------- | ---------- | --------------- |
| `code`, `name`, `primaryGroup`, `nature` | Bean validation   | 400        | Required fields |
| Duplicate `code`                         | Service           | 409        | Unique code     |
| Head not found                           | Service           | 404        | Not found       |
| List filters                             | **Ignored**       | —          | Client filter   |
| Invalid enum in JSON                     | Global handler    | 400        | Fix payload     |
| Auth                                     | Spring            | 401 / 403  | Login / role    |

---

## Frontend Integration Notes

- **Ledger create (Module 31):** Use **`GET /coa/dropdowns/postable-heads`** for **`accountHeadId`** options.
- **Hierarchy:** **`parentHeadId`** is a plain FK — load heads list or tree **client-side** (**Missing from backend context:** dedicated tree API).
- **Enum rendering:** **`CoaPrimaryGroup`**, **`CoaNature`**, **`CoaStatus`** as JSON strings.
- **Status:** Toggle via **activate/inactivate**; not part of create/update body.
- **Inactive heads:** Excluded from **postable** dropdown but still in full list.
- **Create vs update:** Same JSON schema; update needs **`id`** query param.
- **List/search:** Server-side filters **not implemented** — sort is **`code` ASC** only.
- **Used-by:** Call before inactivate to warn if ledgers reference head.
- **Export:** Stub — do not offer file download until backend implements.
- **Headers / token:** Always **`Authorization`** + tenant header per app.

---

_Generated from `CoaController`, `CoaServiceImpl`, COA DTOs/entity/enums, `LedgerResponse` mapping, and `GlobalExceptionHandler` / `ValidationErrorResponse` patterns._
