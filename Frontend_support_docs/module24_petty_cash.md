# Module 24 – Petty Cash

## Short Description

Module 24 manages **employee petty cash claims**: create/update **drafts** (and **returned** requests), attach **receipts** (Base64 JSON), **submit** for approval with **recipient routing**, **approve / reject / return** from approvers’ inbox, **revoke** while **pending**, and **record payment** with transaction reference and optional **payment proof** uploads.

Frontend developers should know:

- **Base path:** `/api/v1/petty-cash`.
- **JWT** required for all routes (same as other `/api/**`). Use **`X-Tenant-ID`** with the tenant JWT so `TenantContext` resolves the correct schema.
- **Success wrapper:** `ResponseStructure` → `{ "status", "message", "data" }`.
- **Lists:** `data` is `PaginationResponse` → `count`, `next`, `prev`, `data[]` (see below). **`next` / `prev`** are **full URLs** built from `app.base-url` + path + query.
- **Authorization:** There are **no** `@PreAuthorize` annotations on `PettyCashController`. **Business rules** live in `PettyCashServiceImpl`: **view detail**, **decide**, and **pay** enforce module actions **`PETTY_CASH_MANAGEMENT`** + **`READ` / `APPROVE` / `EDIT`**, recipient membership, and **requester** rules. **`ROOT`** and **`CEO`** JWT `userType` **bypass** these checks where implemented.
- **Save payload:** `PettyCashSaveRequest` requires `status` **`DRAFT`** or **`RETURNED`** only. **`paymentModeRequested`** must be **`BANK_TRANSFER`** or **`UPI`** (DB constraint); bank vs UPI fields follow `paymentModeRequested`.
- **Files:** Receipts and payment proofs use **`PettyCashFileDto`**: Base64 `fileData` (optional `data:...;base64,` prefix), PDF/JPEG/PNG, max **5 MB** per file, content-type allow-list. **Max 5 receipts** per request (cumulative across updates).
- **Submit-time validation** (server): at least one receipt, amount ≤ ₹50,000, description ≥ 10 chars, expense dates not in future, duplicate-claim check, max expense age (`petty-cash.max-expense-age-days`, default **30**).

---

## Authorization

### Authentication type

- **JWT Bearer** on every request: `Authorization: Bearer <access_token>`.

### Required token / tenant

- Valid JWT with tenant resolution. If the principal is not a direct `User`, the service maps **`username` / email** to a row in **`users`**; failure → **403** with a specific message.
- **`X-Tenant-ID`:** Send on requests so tenant schema matches JWT (`JwtAuthFilter` / `TenantResolverFilter`). If `tenantSchema` is missing for a non-tenant principal → **400** _"Petty cash requires tenant context (JWT tenantSchema and/or X-Tenant-ID header)"_.

### Roles / permissions (service-layer, not controller annotations)

| Action                          | Who can do it (unless ROOT/CEO bypass)                                                                                                                                                                                                                                                  |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **View** `GET /requests/by-id`  | **Draft:** requester only. **Other statuses:** requester **or** user with **`PETTY_CASH_MANAGEMENT` + `READ`** **or** user’s **role** is in the request’s **recipient roles**. Else **404** with generic _"Petty cash request not found"_ (no distinction for unauthorized vs missing). |
| **Decide** `PUT .../decision`   | **`PETTY_CASH_MANAGEMENT` + `APPROVE`** **and** caller’s **role** must be a **recipient** on the request.                                                                                                                                                                               |
| **Pay** `PUT .../pay`           | **`PETTY_CASH_MANAGEMENT` + `EDIT`** **or** **`APPROVE`**.                                                                                                                                                                                                                              |
| **Create/update/submit/revoke** | Requester must be the **current** resolved tenant user (ownership checks).                                                                                                                                                                                                              |

**ROOT / CEO:** `userType` **`ROOT`** or **`CEO`** skips `assertCanViewPettyCashRequest`, `assertCanDecidePettyCashRequest`, and `assertCanPayPettyCashRequest`.

### Required headers

| Header          | Required             | Purpose            |
| --------------- | -------------------- | ------------------ |
| `Authorization` | Yes                  | JWT                |
| `X-Tenant-ID`   | Strongly recommended | Tenant schema      |
| `Content-Type`  | For JSON bodies      | `application/json` |

### Access restrictions (business status)

| Status                        | Typical UI                                                  |
| ----------------------------- | ----------------------------------------------------------- |
| `DRAFT`, `RETURNED`           | Edit (save), submit                                         |
| `PENDING`                     | Requester: revoke. Approver (recipient + APPROVE): decision |
| `APPROVED`                    | Finance: pay                                                |
| `REJECTED`, `PAID`, `REVOKED` | Terminal for workflow (no edit/submit)                      |

---

## Enums Used In This Module

### PettyCashStatus

| Value      | Meaning                    | Used In                                          |
| ---------- | -------------------------- | ------------------------------------------------ |
| `DRAFT`    | Saved, not submitted       | Save request `status`, list filters, transitions |
| `PENDING`  | Awaiting approval          | After submit; revoke allowed                     |
| `APPROVED` | Approved, awaiting payment | Pay action                                       |
| `REJECTED` | Rejected                   | Detail list                                      |
| `RETURNED` | Sent back for correction   | Save `status`, resubmit                          |
| `PAID`     | Paid                       | Detail, segment `completedToday`                 |
| `REVOKED`  | Withdrawn by requester     | After revoke from `PENDING`                      |

**Frontend notes:** Save API only allows **`DRAFT`** or **`RETURNED`** in `PettyCashSaveRequest.status`.

---

### PettyCashCategory

| Value                    | Meaning                        | Used In             |
| ------------------------ | ------------------------------ | ------------------- |
| `ASSET_PURCHASE`         | Asset purchase                 | Save, filters, list |
| `CHEMICAL`               | Chemical                       |                     |
| `FUEL`                   | Fuel                           |                     |
| `INTERNET_AND_TELEPHONE` | Internet & telephone           |                     |
| `LOCAL_CONVEYANCE`       | Local conveyance               |                     |
| `OFFICE_EXPENSES`        | Office expenses                |                     |
| `SALARY_ADVANCE`         | Salary advance                 |                     |
| `STAFF_WELFARE`          | Staff welfare                  |                     |
| `STATIONERY`             | Stationery                     |                     |
| `STATUTORY_AND_LICENSE`  | Statutory & license            |                     |
| `TRAVEL_EXPENSES`        | Travel expenses                |                     |
| `VEHICLE_MAINTENANCE`    | Vehicle maintenance            |                     |
| `VENDOR_PAYMENT`         | Vendor payment                 |                     |
| `RENT`                   | Rent                           |                     |
| `OFFICE_DEPOSIT`         | Office deposit                 |                     |
| `PROMOTER_INCENTIVE`     | Promoter incentive             |                     |
| `OVERTIME`               | Overtime                       |                     |
| `TRANSPORTATION`         | Transportation                 |                     |
| `PETROCARD`              | Petrol/fuel card (label in UI) |                     |

---

### PettyCashPaymentMode

| Value           | Meaning       | Used In                                                                                                          |
| --------------- | ------------- | ---------------------------------------------------------------------------------------------------------------- |
| `BANK_TRANSFER` | Bank transfer | **Save:** `paymentModeRequested` (only this or UPI). **Pay:** `paymentMode` on pay API                           |
| `UPI`           | UPI           | Save + pay                                                                                                       |
| `CASH`          | Cash          | **Pay** API only (**Assumption:** not valid for `paymentModeRequested` on save due to `@AssertTrue` on save DTO) |
| `CHEQUE`        | Cheque        | **Pay** API only (**Assumption:** same as CASH for save)                                                         |

**Frontend notes:** On **create/update**, send only **`BANK_TRANSFER`** or **`UPI`**. If `BANK_TRANSFER`, send bank fields; if `UPI`, send `upiId`. On **pay**, any enum value may be accepted by deserialization; align UI with finance policy.

---

### PettyCashDecision

| Value     | Meaning               | Used In       |
| --------- | --------------------- | ------------- |
| `APPROVE` | Approve               | Decision body |
| `REJECT`  | Reject                |               |
| `RETURN`  | Return for correction |               |

---

### PettyCashRejectionReason

| Value                        | Meaning                    | Used In                             |
| ---------------------------- | -------------------------- | ----------------------------------- |
| `INSUFFICIENT_DOCUMENTATION` | Insufficient documentation | Required when `decision` = `REJECT` |
| `EXCEEDS_POLICY_LIMIT`       | Exceeds policy limit       |                                     |
| `NOT_AUTHORIZED`             | Not authorized             |                                     |
| `DUPLICATE_CLAIM`            | Duplicate claim            |                                     |
| `OTHER`                      | Other                      |                                     |

**Frontend notes:** Detail exposes `rejectionReason` as **string** (enum name), not nested object.

---

### PettyCashAttachmentType

Internal only (response does not expose type on `PettyCashAttachmentResponse`). Used server-side: **`RECEIPT`**, **`PAYMENT_PROOF`**.

---

## API List

| Method | Endpoint                                          | Purpose                              | Authorization (service)              |
| ------ | ------------------------------------------------- | ------------------------------------ | ------------------------------------ |
| `GET`  | `/api/v1/petty-cash/expenses`                     | Tab 1 — all expenses (query filters) | Authenticated; list data tenant-wide |
| `POST` | `/api/v1/petty-cash/expenses/list`                | Same as GET with JSON filters        | Same                                 |
| `GET`  | `/api/v1/petty-cash/requests/my`                  | Tab 2 — my requests (query / model)  | Scoped to JWT user                   |
| `GET`  | `/api/v1/petty-cash/requests/my/list`             | Tab 2 — explicit query params        | Scoped to JWT user                   |
| `POST` | `/api/v1/petty-cash/requests`                     | Create draft                         | Requester = current user             |
| `PUT`  | `/api/v1/petty-cash/requests/update`              | Update draft/returned                | Requester only; `?id=`               |
| `PUT`  | `/api/v1/petty-cash/requests/{id}/revoke`         | Revoke pending                       | Requester; `PENDING` only            |
| `GET`  | `/api/v1/petty-cash/requests/eligible-recipients` | Roles for routing dropdown           | Authenticated                        |
| `PUT`  | `/api/v1/petty-cash/requests/{id}/submit`         | Submit for approval                  | Requester; `DRAFT`/`RETURNED`        |
| `GET`  | `/api/v1/petty-cash/requests/received`            | Tab 3 — received inbox               | Inbox role filter in service         |
| `POST` | `/api/v1/petty-cash/requests/received/list`       | Same with JSON body                  | Same                                 |
| `GET`  | `/api/v1/petty-cash/requests/by-id`               | Detail                               | View rules (see Authorization)       |
| `PUT`  | `/api/v1/petty-cash/requests/{id}/decision`       | Approve/reject/return                | APPROVE + recipient                  |
| `PUT`  | `/api/v1/petty-cash/requests/{id}/pay`            | Record payment                       | EDIT or APPROVE                      |

---

## Shared: filters + pagination (`PettyCashListFilterRequest`)

Used by **GET** (`@ModelAttribute`) and **POST …/list** (JSON body). **`pageNo`** default **0**, **`pageSize`** default **10**, max **100**. Sort: **`createdAt` DESC**.

| Field                     | Type                | Tab / note                                                                          |
| ------------------------- | ------------------- | ----------------------------------------------------------------------------------- |
| `statuses`                | `PettyCashStatus[]` | Repeat query `statuses=` for GET                                                    |
| `branchId`                | string              |                                                                                     |
| `category`                | `PettyCashCategory` |                                                                                     |
| `employeeUserId`          | long                | **Tab 1 only** (all expenses)                                                       |
| `segment`                 | string              | **Tab 3 only:** `pendingApproval`, `pendingPayment`, `completedToday`, `allHistory` |
| `from` / `to`             | `YYYY-MM-DD`        | Filter expense date range (see specification)                                       |
| `minAmount` / `maxAmount` | decimal             |                                                                                     |
| `q`                       | string              | Search: **min 2 chars** effective in spec; matches id, description, requester name  |
| `pageNo`, `pageSize`      | int                 |                                                                                     |

**Segment behavior (received):** maps to status subsets; `completedToday` also filters **`paymentDate` = today**.

---

## Standard wrappers

### `ResponseStructure<T>`

```json
{
  "status": 200,
  "message": "<string>",
  "data": {}
}
```

### `PaginationResponse<T>`

| Field   | Type           | Description                          |
| ------- | -------------- | ------------------------------------ |
| `count` | number         | Total elements                       |
| `next`  | string \| null | Full URL with `pageSize` and filters |
| `prev`  | string \| null | Full URL                             |
| `data`  | T[]            | Page rows (`PettyCashListResponse`)  |

---

## API Details

### `GET` `/api/v1/petty-cash/expenses`

#### Purpose

Paginated **all tenant expenses** with filters (including **`employeeUserId`**).

#### Authorization

JWT required. No extra controller-level permission.

#### Path parameters

None.

#### Query parameters

Same as **`PettyCashListFilterRequest`** (bound via `@ModelAttribute`). Repeat `statuses` for multiple values.

#### Request Body

Not applicable.

#### Response

- **200 OK** — `ResponseStructure<PaginationResponse<PettyCashListResponse>>`

#### `PettyCashListResponse` fields

| Field                              | Type            | Notes                            |
| ---------------------------------- | --------------- | -------------------------------- |
| `id`                               | string          | e.g. `PC-2026-0042`              |
| `employeeName`, `employeeRole`     | string          | From requester                   |
| `branchId`, `branchName`           | string          | Requester branch                 |
| `category`                         | enum            |                                  |
| `amountRequested`                  | number          |                                  |
| `expenseDateFrom`, `expenseDateTo` | date            |                                  |
| `status`                           | enum            |                                  |
| `preApproved`                      | boolean         |                                  |
| `submittedAt`                      | instant \| null |                                  |
| `submittedTo`                      | string \| null  | Label                            |
| `reviewedBy`                       | string \| null  | Reviewer display name            |
| `billsCount`                       | long            | Count of **RECEIPT** attachments |

#### Full response JSON examples

**Success (paginated)**

```json
{
  "status": 200,
  "message": "Expenses fetched",
  "data": {
    "count": 2,
    "next": "https://api.example.com/api/v1/petty-cash/expenses?pageSize=10&statuses=PENDING&pageNo=1",
    "prev": null,
    "data": [
      {
        "id": "PC-2026-0042",
        "employeeName": "Ravi Sharma",
        "employeeRole": "Service Technician",
        "branchId": "BR-MUM-01",
        "branchName": "Mumbai Central",
        "category": "LOCAL_CONVEYANCE",
        "amountRequested": 1250.0,
        "expenseDateFrom": "2026-03-20",
        "expenseDateTo": "2026-03-23",
        "status": "PENDING",
        "preApproved": false,
        "submittedAt": "2026-03-24T10:15:30Z",
        "submittedTo": "Branch Manager",
        "reviewedBy": null,
        "billsCount": 2
      }
    ]
  }
}
```

**Empty**

```json
{
  "status": 200,
  "message": "Expenses fetched",
  "data": {
    "count": 0,
    "next": null,
    "prev": null,
    "data": []
  }
}
```

#### Exceptions / error cases

| HTTP Status | Reason       | When                                   | Typical message                | Frontend note     |
| ----------- | ------------ | -------------------------------------- | ------------------------------ | ----------------- |
| 401         | Unauthorized | Invalid/missing JWT                    | Auth message                   | Login             |
| 400         | Bad request  | Tenant context missing for mapped user | Petty cash tenant message      | Set `X-Tenant-ID` |
| 403         | Forbidden    | Cannot map JWT to tenant user          | Long message about `users` row | Fix user linkage  |

#### Error JSON examples

**401**

```json
{
  "timestamp": "2026-04-13T10:00:00",
  "status": 401,
  "error": "Unauthorized",
  "message": "Full authentication is required to access this resource",
  "path": "/api/v1/petty-cash/expenses",
  "validationErrors": null
}
```

#### Frontend notes

- Use **`employeeUserId`** on Tab 1 only.
- Search **`q`**: backend applies text search only when trimmed length is at least **2** (`PettyCashSpecification`); enforce min 2 chars in UI.

#### cURL

```bash
curl -s -G "https://{host}/api/v1/petty-cash/expenses" \
  --data-urlencode "branchId=BR-MUM-01" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=10" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}"
```

---

### `POST` `/api/v1/petty-cash/expenses/list`

#### Purpose

Same data as **`GET /expenses`**, filters + pagination in **JSON body**.

#### Authorization

Same as GET.

#### Request body

`PettyCashListFilterRequest` (optional — empty body uses defaults).

#### Full request JSON examples

**Search with filters**

```json
{
  "statuses": ["PENDING", "APPROVED"],
  "branchId": "BR-MUM-01",
  "category": "LOCAL_CONVEYANCE",
  "employeeUserId": 42,
  "from": "2026-01-01",
  "to": "2026-12-31",
  "minAmount": 100,
  "maxAmount": 50000,
  "q": "vendor",
  "pageNo": 0,
  "pageSize": 20
}
```

**Minimal (defaults)**

```json
{}
```

#### Response

Same shape as **`GET /expenses`**.

#### cURL

```bash
curl -s -X POST "https://{host}/api/v1/petty-cash/expenses/list" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}" \
  -H "Content-Type: application/json" \
  -d "{\"statuses\":[\"PENDING\"],\"pageNo\":0,\"pageSize\":10}"
```

---

### `GET` `/api/v1/petty-cash/requests/my`

#### Purpose

**My requests** — requester fixed to **JWT user** (ignores `employeeUserId` in spec for this flow).

#### Query / model

`PettyCashListFilterRequest` via `@ModelAttribute`.

#### Request Body

Not applicable.

#### Response

`PaginationResponse<PettyCashListResponse>` — **`next`** links use `/api/v1/petty-cash/requests/my?...`

#### Full response JSON example

```json
{
  "status": 200,
  "message": "My requests fetched",
  "data": {
    "count": 1,
    "next": null,
    "prev": null,
    "data": [
      {
        "id": "PC-2026-0001",
        "employeeName": "Me User",
        "employeeRole": "Technician",
        "branchId": "BR-01",
        "branchName": "HQ",
        "category": "STATIONERY",
        "amountRequested": 500.0,
        "expenseDateFrom": "2026-04-01",
        "expenseDateTo": "2026-04-05",
        "status": "DRAFT",
        "preApproved": false,
        "submittedAt": null,
        "submittedTo": null,
        "reviewedBy": null,
        "billsCount": 0
      }
    ]
  }
}
```

#### cURL

```bash
curl -s -G "https://{host}/api/v1/petty-cash/requests/my" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=10" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}"
```

---

### `GET` `/api/v1/petty-cash/requests/my/list`

#### Purpose

Same as **`/requests/my`** but builds filter from **explicit** `@RequestParam` list (no `segment` / `employeeUserId` in builder).

#### Query parameters

| Field                    | Type    | Required |
| ------------------------ | ------- | -------- |
| `statuses`               | list    | No       |
| `branchId`               | string  | No       |
| `category`               | enum    | No       |
| `from`, `to`             | date    | No       |
| `minAmount`, `maxAmount` | decimal | No       |
| `q`                      | string  | No       |
| `pageNo`, `pageSize`     | int     | No       |

#### Request Body

Not applicable.

#### Response

Same as **`/requests/my`**.

#### cURL

```bash
curl -s -G "https://{host}/api/v1/petty-cash/requests/my/list" \
  --data-urlencode "statuses=DRAFT" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=10" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}"
```

---

### `POST` `/api/v1/petty-cash/requests`

#### Purpose

Create a new petty cash request as **draft** (typically `status: "DRAFT"`). Generates id `PC-{year}-{seq}`. Optionally uploads receipts Base64.

#### Authorization

Current user becomes requester.

#### Request body — `PettyCashSaveRequest`

| Field                                                            | Type                 | Required    | Validation                    | Description                                   |
| ---------------------------------------------------------------- | -------------------- | ----------- | ----------------------------- | --------------------------------------------- |
| `category`                                                       | enum                 | No\*        |                               | \*May be required by DB — send for real flows |
| `expenseDateFrom`                                                | date                 | Yes         | `@NotNull`                    |                                               |
| `expenseDateTo`                                                  | date                 | Yes         | `@NotNull`                    | Must be ≥ from (`@AssertTrue`)                |
| `amount`                                                         | decimal              | Yes         | `> 0`                         |                                               |
| `description`                                                    | string               | No          |                               | Submit-time min 10 chars                      |
| `relatedTaskRef`                                                 | string               | No          |                               |                                               |
| `relatedSoRef`                                                   | string               | No          |                               |                                               |
| `justificationNote`                                              | string               | No          |                               |                                               |
| `paymentModeRequested`                                           | enum                 | Yes         | `BANK_TRANSFER` or `UPI` only |                                               |
| `bankAccountHolder`, `bankName`, `bankAccountNumber`, `bankIfsc` | string               | Conditional |                               | If `BANK_TRANSFER`, service sets from request |
| `upiId`                                                          | string               | Conditional |                               | If `UPI`                                      |
| `preApproved`                                                    | boolean              | No          |                               |                                               |
| `preApprovedByUserId`                                            | long                 | Conditional |                               | If pre-approved                               |
| `approvalReference`                                              | string               | No          |                               |                                               |
| `receipts`                                                       | `PettyCashFileDto[]` | No          | max 5 items                   | Base64 files                                  |
| `requestRoles`                                                   | long[]               | No          | max 50 ids                    | Empty → all eligible role ids                 |
| `status`                                                         | enum                 | Yes         | Must be `DRAFT` or `RETURNED` | Use **`DRAFT`** on create                     |

**`PettyCashFileDto`**

| Field         | Type   | Required                                                        |
| ------------- | ------ | --------------------------------------------------------------- |
| `fileName`    | string | Yes                                                             |
| `contentType` | string | Yes — `application/pdf`, `image/jpeg`, `image/jpg`, `image/png` |
| `fileData`    | string | Yes — raw Base64 or `data:...;base64,...`                       |
| `notes`       | string | No                                                              |

#### Full request JSON examples

**Minimal valid draft**

```json
{
  "category": "STATIONERY",
  "expenseDateFrom": "2026-04-01",
  "expenseDateTo": "2026-04-05",
  "amount": 500.0,
  "description": "Draft description text",
  "paymentModeRequested": "UPI",
  "upiId": "requester@upi",
  "status": "DRAFT"
}
```

**Complete draft — bank transfer + receipt (abbreviated Base64)**

```json
{
  "category": "LOCAL_CONVEYANCE",
  "expenseDateFrom": "2026-04-01",
  "expenseDateTo": "2026-04-03",
  "amount": 1250.5,
  "description": "Local travel for site visit and fuel",
  "relatedTaskRef": "TASK-2026-0001",
  "relatedSoRef": "SO-2026-0042",
  "justificationNote": "Urgent client visit",
  "paymentModeRequested": "BANK_TRANSFER",
  "bankAccountHolder": "Ravi Sharma",
  "bankName": "HDFC Bank",
  "bankAccountNumber": "501234567890",
  "bankIfsc": "HDFC0001234",
  "preApproved": false,
  "status": "DRAFT",
  "requestRoles": [10, 11],
  "receipts": [
    {
      "fileName": "bill.pdf",
      "contentType": "application/pdf",
      "fileData": "JVBERi0xLjQKJeLjz9MK...",
      "notes": "Fuel bill"
    }
  ]
}
```

**Returned save (after reviewer return)**

```json
{
  "category": "STATIONERY",
  "expenseDateFrom": "2026-04-01",
  "expenseDateTo": "2026-04-05",
  "amount": 500.0,
  "description": "Updated after return with more detail",
  "paymentModeRequested": "UPI",
  "upiId": "user@upi",
  "status": "RETURNED"
}
```

#### Response

- **201 Created** — `ResponseStructure<PettyCashDetailResponse>`

#### Full detail response example

```json
{
  "status": 201,
  "message": "Petty cash request saved as draft",
  "data": {
    "id": "PC-2026-0042",
    "status": "DRAFT",
    "requesterName": "Ravi Sharma",
    "requesterRole": "Technician",
    "requesterBranchId": "BR-01",
    "requesterBranchName": "HQ",
    "submittedAt": null,
    "submittedTo": null,
    "category": "LOCAL_CONVEYANCE",
    "expenseDateFrom": "2026-04-01",
    "expenseDateTo": "2026-04-03",
    "amountRequested": 1250.5,
    "description": "Local travel for site visit and fuel",
    "relatedTaskRef": "TASK-2026-0001",
    "relatedSoRef": "SO-2026-0042",
    "justificationNote": "Urgent client visit",
    "paymentModeRequested": "BANK_TRANSFER",
    "bankAccountHolder": "Ravi Sharma",
    "bankName": "HDFC Bank",
    "bankAccountNumber": "501234567890",
    "bankIfsc": "HDFC0001234",
    "upiId": null,
    "preApproved": false,
    "preApprovedByName": null,
    "approvalReference": null,
    "reviewedByName": null,
    "reviewedAt": null,
    "approvedAmount": null,
    "reviewerRemarks": null,
    "rejectionReason": null,
    "correctionNotes": null,
    "paymentModeProcessed": null,
    "transactionRef": null,
    "paymentDate": null,
    "financeRemarks": null,
    "paidByName": null,
    "paidAt": null,
    "receipts": [
      {
        "id": "uuid-1",
        "fileName": "bill.pdf",
        "contentType": "application/pdf",
        "fileSizeBytes": 102400,
        "url": "https://storage.example.com/presigned...",
        "notes": "Fuel bill"
      }
    ],
    "paymentProof": [],
    "auditLogs": [
      {
        "action": "DRAFT_SAVED",
        "actorName": "Ravi Sharma",
        "remarks": null,
        "actionAt": "2026-04-13T09:00:00Z"
      }
    ]
  }
}
```

#### Exceptions

| HTTP | Reason                    | Message (examples)                    |
| ---- | ------------------------- | ------------------------------------- |
| 400  | Bean validation           | `Input validation failed` + field map |
| 400  | No branch                 | No branch in tenant…                  |
| 400  | Pre-approver missing user | `Pre-approved by user not found`      |
| 500  | Upload failure            | `Failed to upload file`               |
| 400  | Invalid file type         | `Invalid file type`                   |
| 400  | File too large            | `Max 5MB`                             |

#### Error JSON (validation)

```json
{
  "timestamp": "2026-04-13T10:00:00",
  "status": 400,
  "error": "Bad Request",
  "message": "Input validation failed",
  "path": "/api/v1/petty-cash/requests",
  "validationErrors": {
    "status": "status must be DRAFT or RETURNED when saving a draft",
    "paymentModeRequested": "paymentModeRequested must be BANK_TRANSFER or UPI for a request"
  }
}
```

#### cURL

```bash
curl -s -X POST "https://{host}/api/v1/petty-cash/requests" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}" \
  -H "Content-Type: application/json" \
  -d "{\"expenseDateFrom\":\"2026-04-01\",\"expenseDateTo\":\"2026-04-05\",\"amount\":500,\"description\":\"Short text\",\"paymentModeRequested\":\"UPI\",\"upiId\":\"a@upi\",\"status\":\"DRAFT\"}"
```

---

### `PUT` `/api/v1/petty-cash/requests/update`

#### Purpose

Update a **`DRAFT`** or **`RETURNED`** request. **Requester only.** New receipts are **appended** (existing not deleted); total receipts must not exceed **5**.

#### Authorization

Requester = current user; statuses **`DRAFT`** or **`RETURNED`** only.

#### Query parameters

| Field | Type   | Required | Description | Example        |
| ----- | ------ | -------- | ----------- | -------------- |
| `id`  | string | Yes      | Request id  | `PC-2026-0042` |

#### Request body

Same as **`PettyCashSaveRequest`** (create).

#### Full request JSON examples

**Update draft — add receipt**

```json
{
  "category": "STATIONERY",
  "expenseDateFrom": "2026-04-01",
  "expenseDateTo": "2026-04-05",
  "amount": 600.0,
  "description": "Stationery purchase updated with more detail",
  "paymentModeRequested": "UPI",
  "upiId": "user@upi",
  "status": "DRAFT",
  "receipts": [
    {
      "fileName": "receipt2.png",
      "contentType": "image/png",
      "fileData": "iVBORw0KGgoAAAANSUhEUg..."
    }
  ]
}
```

**Minimal field update (no new files)**

```json
{
  "expenseDateFrom": "2026-04-01",
  "expenseDateTo": "2026-04-05",
  "amount": 600.0,
  "description": "Stationery purchase updated with more detail",
  "paymentModeRequested": "UPI",
  "upiId": "user@upi",
  "status": "DRAFT"
}
```

#### Response

**200 OK** — `PettyCashDetailResponse`

#### Exceptions

| HTTP | Message                                         |
| ---- | ----------------------------------------------- |
| 404  | `Petty cash request not found: {id}`            |
| 403  | `You are not allowed to edit this request`      |
| 400  | `Only DRAFT or RETURNED requests can be edited` |
| 400  | `Maximum 5 receipts allowed per request`        |

#### Error JSON

```json
{
  "timestamp": "2026-04-13T10:00:00",
  "status": 403,
  "error": "Forbidden",
  "message": "You are not allowed to edit this request",
  "path": "/api/v1/petty-cash/requests/update",
  "validationErrors": null
}
```

#### cURL

```bash
curl -s -X PUT "https://{host}/api/v1/petty-cash/requests/update?id=PC-2026-0042" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}" \
  -H "Content-Type: application/json" \
  -d "{\"expenseDateFrom\":\"2026-04-01\",\"expenseDateTo\":\"2026-04-05\",\"amount\":600,\"description\":\"Stationery purchase updated\",\"paymentModeRequested\":\"UPI\",\"upiId\":\"u@upi\",\"status\":\"DRAFT\"}"
```

---

### `PUT` `/api/v1/petty-cash/requests/{id}/revoke`

#### Purpose

Revoke a **`PENDING`** request → **`REVOKED`**. Requester only.

#### Path parameters

| Field | Type   | Required | Example        |
| ----- | ------ | -------- | -------------- |
| `id`  | string | Yes      | `PC-2026-0042` |

#### Request Body

Not applicable.

#### Response

**200 OK** — `PettyCashDetailResponse` with `status: "REVOKED"`

#### Exceptions

| HTTP | Message                                |
| ---- | -------------------------------------- |
| 404  | Not found                              |
| 403  | Forbidden (not requester)              |
| 400  | `Only PENDING requests can be revoked` |

#### Error JSON

```json
{
  "status": 400,
  "error": "Bad Request",
  "message": "Only PENDING requests can be revoked",
  "path": "/api/v1/petty-cash/requests/PC-2026-0042/revoke",
  "validationErrors": null
}
```

#### cURL

```bash
curl -s -X PUT "https://{host}/api/v1/petty-cash/requests/PC-2026-0042/revoke" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}"
```

---

### `GET` `/api/v1/petty-cash/requests/eligible-recipients`

#### Purpose

Returns **role** dropdown items for routing (from current user’s `PETTY_CASH_MANAGEMENT` + `REQUEST` permission `receiverRoleIds`).

#### Response

**200 OK** — `ResponseStructure<List<RoleDropdownItem>>`

**`RoleDropdownItem`:** `{ "id": 10, "name": "Branch Manager" }` (record JSON: `id`, `name`)

#### Full response JSON

```json
{
  "status": 200,
  "message": "Eligible recipients fetched",
  "data": [
    { "id": 10, "name": "Branch Manager" },
    { "id": 11, "name": "Finance" }
  ]
}
```

**Empty — no permission config**

```json
{
  "status": 200,
  "message": "Eligible recipients fetched",
  "data": []
}
```

#### Exceptions

| HTTP | When                                                                                                                                                                                                            |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 404  | Module/action misconfiguration (`ModuleNotFoundException` / `ActionNotFoundException`) — **Note:** code throws `ModuleNotFoundException` with message _"Module 'User' not found"_ (bug; treat as config error). |

#### cURL

```bash
curl -s "https://{host}/api/v1/petty-cash/requests/eligible-recipients" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}"
```

---

### `PUT` `/api/v1/petty-cash/requests/{id}/submit`

#### Purpose

Submit **`DRAFT`** or **`RETURNED`** → **`PENDING`**. Runs **validateSubmitState** (receipts, amounts, dates, duplicate claim, etc.).

#### Path parameters

`id` — required.

#### Request body — `PettyCashSubmitRecipientsRequest`

| Field              | Type    | Required | Description                                          |
| ------------------ | ------- | -------- | ---------------------------------------------------- |
| `sendToAll`        | boolean | Yes      | If true, route to all receiver roles from permission |
| `recipientRoleIds` | long[]  | No       | Used when `sendToAll` false                          |
| `recipientUserIds` | long[]  | No       | Legacy: users → roles                                |

#### Full request JSON examples

**Send to all**

```json
{
  "sendToAll": true
}
```

**Specific roles**

```json
{
  "sendToAll": false,
  "recipientRoleIds": [10, 11]
}
```

**Legacy users**

```json
{
  "sendToAll": false,
  "recipientUserIds": [101, 102]
}
```

#### Response

**200 OK** — `PettyCashDetailResponse`

#### Submit validation errors (400)

| Message                                               |
| ----------------------------------------------------- |
| `At least one receipt is required before submission`  |
| `Amount is required`                                  |
| `Max single expense is ₹50,000`                       |
| `Description must be at least 10 characters`          |
| `Expense date range is required`                      |
| `Expense date cannot be in the future`                |
| `Expense date (to) cannot be before (from)`           |
| `Approved By is required when pre-approved`           |
| Duplicate claim message                               |
| `Expense dates cannot be older than {N} days`         |
| `No approver roles configured for petty cash routing` |
| `Please select at least one valid role`               |
| `Selected users have no roles`                        |
| `Please select at least one recipient role`           |

#### Error JSON

```json
{
  "status": 400,
  "error": "Bad Request",
  "message": "At least one receipt is required before submission",
  "path": "/api/v1/petty-cash/requests/PC-2026-0042/submit",
  "validationErrors": null
}
```

#### cURL

```bash
curl -s -X PUT "https://{host}/api/v1/petty-cash/requests/PC-2026-0042/submit" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}" \
  -H "Content-Type: application/json" \
  -d "{\"sendToAll\":true}"
```

---

### `GET` `/api/v1/petty-cash/requests/received`

#### Purpose

Approver **inbox** — intersects search filter with request ids where current user’s **role** is a recipient.

#### Query

`PettyCashListFilterRequest` including **`segment`** for Tab 3.

#### Request Body

Not applicable.

#### Response

`PaginationResponse<PettyCashListResponse>` — **`next`** uses `/requests/received?...`

#### Special cases

- If **no inbox ids** for role → **empty list** without error (`count: 0`).
- If user **has no role** → **400** `Current user role not found`.

#### Error JSON

```json
{
  "status": 400,
  "error": "Bad Request",
  "message": "Current user role not found",
  "path": "/api/v1/petty-cash/requests/received",
  "validationErrors": null
}
```

#### cURL

```bash
curl -s -G "https://{host}/api/v1/petty-cash/requests/received" \
  --data-urlencode "segment=pendingApproval" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=10" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}"
```

---

### `POST` `/api/v1/petty-cash/requests/received/list`

#### Purpose

Same as GET received with **JSON** filters.

#### Full request JSON

```json
{
  "segment": "pendingPayment",
  "branchId": "BR-01",
  "from": "2026-04-01",
  "to": "2026-04-30",
  "pageNo": 0,
  "pageSize": 20
}
```

#### Response

Same as GET received.

#### cURL

```bash
curl -s -X POST "https://{host}/api/v1/petty-cash/requests/received/list" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}" \
  -H "Content-Type: application/json" \
  -d "{\"segment\":\"pendingApproval\",\"pageNo\":0,\"pageSize\":10}"
```

---

### `GET` `/api/v1/petty-cash/requests/by-id`

#### Purpose

Full detail for screen + audit + attachments (presigned URLs **60 minutes**).

#### Query parameters

| Field | Type   | Required |
| ----- | ------ | -------- |
| `id`  | string | Yes      |

#### Request Body

Not applicable.

#### Response

**200 OK** — `PettyCashDetailResponse`

#### Exceptions

| HTTP | Message                                                                             |
| ---- | ----------------------------------------------------------------------------------- |
| 404  | `Petty cash request not found: {id}` — also used when **not allowed** to view draft |

#### Error JSON

```json
{
  "status": 404,
  "error": "Not Found",
  "message": "Petty cash request not found: PC-2026-0001",
  "path": "/api/v1/petty-cash/requests/by-id",
  "validationErrors": null
}
```

#### cURL

```bash
curl -s -G "https://{host}/api/v1/petty-cash/requests/by-id" \
  --data-urlencode "id=PC-2026-0042" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}"
```

---

### `PUT` `/api/v1/petty-cash/requests/{id}/decision`

#### Purpose

**Approve**, **reject**, or **return** a **`PENDING`** request. Requires **`APPROVE`** permission + **recipient** role (unless ROOT/CEO).

#### Request body — `PettyCashDecisionRequest`

| Field             | Type    | Required   | Notes                                     |
| ----------------- | ------- | ---------- | ----------------------------------------- |
| `decision`        | enum    | Yes        | `APPROVE` / `REJECT` / `RETURN`           |
| `approvedAmount`  | decimal | If APPROVE | **Required**, &gt; 0, ≤ `amountRequested` |
| `rejectionReason` | enum    | If REJECT  | Required                                  |
| `remarks`         | string  | max 500    | Required non-blank for **REJECT**         |
| `correctionNotes` | string  | max 500    | **RETURN:** min **10** chars trimmed      |

#### Full request JSON examples

**Approve**

```json
{
  "decision": "APPROVE",
  "approvedAmount": 1200.0,
  "remarks": "Approved within policy"
}
```

**Reject**

```json
{
  "decision": "REJECT",
  "rejectionReason": "INSUFFICIENT_DOCUMENTATION",
  "remarks": "Please upload clear GST bill for all line items."
}
```

**Return**

```json
{
  "decision": "RETURN",
  "correctionNotes": "Please split fuel and toll into separate receipts and re-upload."
}
```

#### Response

**200 OK** — `PettyCashDetailResponse`

#### Exceptions

| HTTP | Message                                                   |
| ---- | --------------------------------------------------------- |
| 400  | `Only PENDING requests can be reviewed`                   |
| 403  | `Not authorized to approve or reject petty cash requests` |
| 403  | `You are not a recipient for this request`                |
| 400  | `Approved amount is required for approval`                |
| 400  | `Approved amount cannot exceed requested amount`          |
| 400  | `Rejection reason is required`                            |
| 400  | `Remarks are required when rejecting`                     |
| 400  | `Correction notes are required (min 10 chars)`            |

#### Error JSON (forbidden)

```json
{
  "status": 403,
  "error": "Forbidden",
  "message": "You are not a recipient for this request",
  "path": "/api/v1/petty-cash/requests/PC-2026-0042/decision",
  "validationErrors": null
}
```

#### cURL

```bash
curl -s -X PUT "https://{host}/api/v1/petty-cash/requests/PC-2026-0042/decision" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}" \
  -H "Content-Type: application/json" \
  -d "{\"decision\":\"APPROVE\",\"approvedAmount\":1200,\"remarks\":\"OK\"}"
```

---

### `PUT` `/api/v1/petty-cash/requests/{id}/pay`

#### Purpose

Record payment for **`APPROVED`** → **`PAID`**. **`transactionRef`** must be **unique** tenant-wide (excluding this request).

#### Request body — `PettyCashPayRequest`

| Field            | Type                 | Required | Validation                                          |
| ---------------- | -------------------- | -------- | --------------------------------------------------- |
| `paymentMode`    | enum                 | Yes      | `PettyCashPaymentMode` (typically how finance paid) |
| `transactionRef` | string               | Yes      | `@NotNull` `@Size(min=5,max=120)`; trim non-empty   |
| `paymentDate`    | date                 | Yes      | Not in future                                       |
| `financeRemarks` | string               | No       | max 500                                             |
| `paymentProof`   | `PettyCashFileDto[]` | No       | Same file rules                                     |

#### Full request JSON examples

**Pay — UPI ref only**

```json
{
  "paymentMode": "UPI",
  "transactionRef": "TXN-20260413-ABCD123456",
  "paymentDate": "2026-04-13",
  "financeRemarks": "Paid via company UPI"
}
```

**Pay with proof**

```json
{
  "paymentMode": "BANK_TRANSFER",
  "transactionRef": "NEFT-REF-998877665544",
  "paymentDate": "2026-04-13",
  "financeRemarks": "Settlement batch 42",
  "paymentProof": [
    {
      "fileName": "payment-screenshot.png",
      "contentType": "image/png",
      "fileData": "iVBORw0KGgoAAAANSUhEUg..."
    }
  ]
}
```

**Cash — enum variation**

```json
{
  "paymentMode": "CASH",
  "transactionRef": "CASH-VOUCHER-001",
  "paymentDate": "2026-04-13"
}
```

#### Response

**200 OK** — `PettyCashDetailResponse`

#### Exceptions

| HTTP | Message                                         |
| ---- | ----------------------------------------------- |
| 400  | `Only APPROVED requests can be paid`            |
| 403  | `Not authorized to process petty cash payments` |
| 400  | `Payment date cannot be in the future`          |
| 400  | `Transaction reference is required`             |
| 409  | `Transaction reference is already used`         |

#### Error JSON (conflict)

```json
{
  "status": 409,
  "error": "Conflict",
  "message": "Transaction reference is already used",
  "path": "/api/v1/petty-cash/requests/PC-2026-0042/pay",
  "validationErrors": null
}
```

#### cURL

```bash
curl -s -X PUT "https://{host}/api/v1/petty-cash/requests/PC-2026-0042/pay" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}" \
  -H "Content-Type: application/json" \
  -d "{\"paymentMode\":\"UPI\",\"transactionRef\":\"TXN-20260413-ABCD123456\",\"paymentDate\":\"2026-04-13\"}"
```

---

## Validation and Exception Summary

| Field / scenario            | Rule                         | Error          | Frontend                   |
| --------------------------- | ---------------------------- | -------------- | -------------------------- |
| Save `status`               | `DRAFT` or `RETURNED`        | 400 validation | Dropdown lock              |
| Save `paymentModeRequested` | `BANK_TRANSFER` or `UPI`     | 400            | Hide CASH/CHEQUE on create |
| Receipts total              | ≤ 5                          | 400            | Counter                    |
| File                        | type allow-list, 5MB         | 400            | Client validation          |
| Submit                      | ≥1 receipt                   | 400            | Block submit               |
| Submit amount               | ≤ 50000                      | 400            |                            |
| Submit description          | ≥10 chars                    | 400            |                            |
| Submit expense dates        | not future; within max age   | 400            |                            |
| Duplicate claim             | same user/amount/range       | 400            |                            |
| Approve                     | `approvedAmount` ≤ requested | 400            |                            |
| Reject                      | reason + remarks             | 400            |                            |
| Return                      | correction ≥10 chars         | 400            |                            |
| Pay                         | unique `transactionRef`      | **409**        | Show duplicate             |
| View draft                  | non-owner                    | **404**        | Do not leak existence      |

---

## Frontend Integration Notes

- **Tabs:** Map **Tab 1** → `/expenses`, **Tab 2** → `/requests/my` or `/requests/my/list`, **Tab 3** → `/received` with **`segment`**.
- **Detail:** `GET /by-id?id=` — handle **404** as “not found or no access”.
- **Create vs update:** Create `POST /requests`; update `PUT /requests/update?id=...`. Both use **`PettyCashSaveRequest`**.
- **Recipients:** `GET /eligible-recipients` for picker; submit with `sendToAll` or `recipientRoleIds`.
- **Presigned URLs:** `receipts[].url` / `paymentProof[].url` expire (**60 min**); refresh detail if needed.
- **Pagination:** Use `next`/`prev` full URLs from API, or rebuild with same query params.
- **Search `q`:** Minimum **2** characters for DB filter (spec).
- **Dates:** ISO `YYYY-MM-DD`; instants ISO-8601 in JSON.
- **CEO/ROOT:** Full bypass for view/decide/pay rules — still test as normal tenant user for real UX.

---

## Assumptions / missing from backend context

- **Category** on create: not `@NotNull` in DTO; DB may still require it — confirm with migration.
- **`PettyCashPayRequest.transactionRef`:** `@Size(min=5)` — service also trims; very short refs fail validation before service.
- **Eligible recipients** error message _"Module 'User' not found"_ is **incorrect** string in code (`PETTY_CASH_MANAGEMENT` lookup) — treat as misconfiguration.
- **Payment proof** count: no explicit max in service (unlike receipts); **Assumption:** keep UI to 1–3 files.

---

_Generated from: `PettyCashController`, `PettyCashServiceImpl`, DTOs, enums, `PettyCashSpecification`, `GlobalExceptionHandler`, `ValidationErrorResponse`._
