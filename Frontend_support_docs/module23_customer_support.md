# Module 23 – Customer Support

## Short Description

Module 23 provides **customer support ticket** lifecycle management for tenant back-office users: raise tickets against customers (optionally linked to sales orders and tasks), view SLA-aware dashboards, assign work, add notes/replies, pause/resume SLA clocks, move tickets through **OPEN → ASSIGNED → IN_PROGRESS → (PAUSED) → RESOLVED → CLOSED**, and attach file **paths** (upload happens elsewhere; this module stores paths only).

Frontend developers should know:

- **Base path:** `/api/v1/support/tickets` (all routes in this module).
- **Auth:** JWT on every call; tenant routing via **`X-Tenant-ID`** (same as the rest of the app). Data is resolved in the **current tenant schema**.
- **Response wrapper:** Successful JSON responses use `ResponseStructure` (`status`, `message`, `data`). Errors use `ValidationErrorResponse` (see **Exceptions**).
- **Permissions:** Read vs Add vs Edit are split; **`CEO`** role bypasses granular checks (see **Authorization**).
- **Ticket reference:** Path/query parameter `ticketRef` accepts either **internal ticket id** or **human-readable ticket number** (e.g. `TKT-2026-0001`).
- **Attachments:** `attachmentPaths` are **strings** (storage paths from your file-upload flow). This module does **not** expose multipart upload endpoints.
- **Integration:** Branch may come from the **customer** or **sales order** when SO is selected; **resolve** is blocked if any **linked task** (Module 21) for this ticket is not `COMPLETED`.

---

## Authorization

### Authentication type

- **JWT Bearer** required for all endpoints in this module.
- Header: `Authorization: Bearer <access_token>`

### Required roles / authorities / permissions

| Concern                                                   | Rule (controller)                                                          |
| --------------------------------------------------------- | -------------------------------------------------------------------------- |
| List ticket types, list tickets, ticket detail            | `hasAuthority('CUSTOMER_SUPPORT_MANAGEMENT_READ')` **or** `hasRole('CEO')` |
| Raise ticket                                              | `hasAuthority('CUSTOMER_SUPPORT_MANAGEMENT_ADD')` **or** `hasRole('CEO')`  |
| Assign, notes, pause, resume, in-progress, resolve, close | `hasAuthority('CUSTOMER_SUPPORT_MANAGEMENT_EDIT')` **or** `hasRole('CEO')` |

**Note:** Spring `hasRole('CEO')` matches the **`ROLE_CEO`** authority in the security model.

### Required headers

| Header          | Required          | Purpose                                                                                                                                       |
| --------------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `Authorization` | Yes               | JWT                                                                                                                                           |
| `X-Tenant-ID`   | Yes (recommended) | Tenant schema selection (`TenantResolverFilter` + JWT validation). Omitting may fall back to default tenant behavior depending on app config. |
| `Content-Type`  | For JSON bodies   | `application/json`                                                                                                                            |

### Tenant / schema behavior

- Requests run against the **tenant schema** selected by **`X-Tenant-ID`** (and aligned with JWT `tenantSchema` per `JwtAuthFilter`).
- All ticket entities, customers, branches, sales orders, and tasks are **tenant-scoped**.

### Access restrictions (business status)

| Operation        | Status / rule                                                                                           |
| ---------------- | ------------------------------------------------------------------------------------------------------- |
| Assign, add note | **Not** allowed when status is **`RESOLVED`** or **`CLOSED`** (`ensureMutable`).                        |
| Pause            | Only **`ASSIGNED`** or **`IN_PROGRESS`**; not already paused.                                           |
| Resume           | Only **`PAUSED`** with `pauseStartedAt` set.                                                            |
| Mark in progress | Only from **`OPEN`** or **`ASSIGNED`**.                                                                 |
| Resolve          | **Not** if already **`RESOLVED`** or **`CLOSED`**; must pass linked-task rule (see **POST …/resolve**). |
| Close            | Only when status is **`RESOLVED`**.                                                                     |

---

## Enums Used In This Module

### SupportTicketStatus

| Value         | Meaning                                            | Used In                                |
| ------------- | -------------------------------------------------- | -------------------------------------- |
| `OPEN`        | New ticket, not yet assigned/in progress.          | Create (initial), filters, list/detail |
| `ASSIGNED`    | Assignee set (from `OPEN` on first assign).        | List/detail, transitions               |
| `IN_PROGRESS` | Actively worked.                                   | List/detail, pause, resume target      |
| `PAUSED`      | Waiting; SLA pause window active in service logic. | List/detail, resume                    |
| `RESOLVED`    | Resolved; awaiting close.                          | List/detail, close                     |
| `CLOSED`      | Terminal state.                                    | List/detail                            |

**Frontend notes:** Edits (assign, notes) are blocked for `RESOLVED` / `CLOSED`. There is **no** REST “update ticket fields” for subject/description after create in this controller.

---

### SupportTicketPriority

| Value      | Meaning           | Used In                   |
| ---------- | ----------------- | ------------------------- |
| `NORMAL`   | Default priority. | Raise, filters, responses |
| `HIGH`     | Elevated.         |                           |
| `URGENT`   | Urgent.           |                           |
| `CRITICAL` | Highest.          |                           |

---

### SupportSlaHealth

Computed for **list** and **detail** responses (see `SupportTicketServiceImpl.computeHealth`). **Not** a persisted column.

| Value      | Meaning                                                                                                                         | Used In                 |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| `SAFE`     | Within SLA window (before risk instant), or paused (shown as SAFE for paused), or terminal closed/resolved with no breach flag. | List/detail `slaHealth` |
| `AT_RISK`  | At/after risk instant, before resolution deadline (active tickets).                                                             | List/detail             |
| `BREACHED` | Past resolution deadline for active tickets, or terminal states with `resolutionSlaBreached == true`.                           | List/detail             |

**Frontend notes:** For **`PAUSED`**, `computeHealth` returns **`SAFE`** in the service layer, while **dashboard filters** by `slaHealth` use JPA criteria that may still match paused tickets under `AT_RISK` / `BREACHED` if wall-clock time crosses thresholds. **Assumption:** Prefer displaying **`slaHealth`** from the API response for each row; treat filter + row display edge cases as possible inconsistency.

---

### SupportEscalationLevel

| Value  | Meaning                                                       | Used In                        |
| ------ | ------------------------------------------------------------- | ------------------------------ |
| `NONE` | No escalation.                                                | Default on create; list/detail |
| `L1`   | Level 1 (e.g. response SLA concern) — set by scheduler sweep. | List/detail                    |
| `L2`   | Level 2 (e.g. risk window).                                   | List/detail                    |
| `L3`   | Level 3 (e.g. past resolution deadline).                      | List/detail                    |

---

### SupportResolutionCode

| Value                      | Meaning                                                   | Used In         |
| -------------------------- | --------------------------------------------------------- | --------------- |
| `SERVICE_RESOLVED_SUCCESS` | Service resolved successfully.                            | Resolve request |
| `FALSE_ALARM`              | False alarm / not found.                                  |                 |
| `DUPLICATE_TICKET`         | Duplicate ticket.                                         |                 |
| `UNRESOLVED_CLOSED`        | Closed unresolved (use when resolving with this outcome). |                 |

---

### SupportCloseReason

| Value                            | Meaning                            | Used In       |
| -------------------------------- | ---------------------------------- | ------------- |
| `RESOLVED_CUSTOMER_SATISFACTION` | Resolved to customer satisfaction. | Close request |
| `DUPLICATE_REQUEST`              | Duplicate request.                 |               |
| `CUSTOMER_NON_RESPONSIVE`        | Customer not responsive.           |               |
| `OUT_OF_SCOPE`                   | Out of scope.                      |               |

---

### SupportAttachmentPhase

| Value        | Meaning                                                                   | Used In         |
| ------------ | ------------------------------------------------------------------------- | --------------- |
| `RAISE`      | Attachment path stored at ticket creation.                                | Attachment list |
| `RESOLUTION` | Stored at resolve.                                                        |                 |
| `CLOSE`      | Stored at close.                                                          |                 |
| `NOTE`       | Reserved for future use (not set in current service paths for note-only). | Response schema |

---

## API List

| Method | Endpoint                                          | Purpose                                      | Authorization Required                      |
| ------ | ------------------------------------------------- | -------------------------------------------- | ------------------------------------------- |
| `GET`  | `/api/v1/support/tickets/types`                   | List active ticket types (dropdown).         | `CUSTOMER_SUPPORT_MANAGEMENT_READ` or `CEO` |
| `POST` | `/api/v1/support/tickets`                         | Raise a new ticket.                          | `CUSTOMER_SUPPORT_MANAGEMENT_ADD` or `CEO`  |
| `GET`  | `/api/v1/support/tickets`                         | Paginated ticket list with filters.          | `CUSTOMER_SUPPORT_MANAGEMENT_READ` or `CEO` |
| `GET`  | `/api/v1/support/tickets/by-id`                   | Ticket detail by id or number.               | `CUSTOMER_SUPPORT_MANAGEMENT_READ` or `CEO` |
| `POST` | `/api/v1/support/tickets/{ticketRef}/assign`      | Assign / reassign.                           | `CUSTOMER_SUPPORT_MANAGEMENT_EDIT` or `CEO` |
| `POST` | `/api/v1/support/tickets/{ticketRef}/notes`       | Add internal note or customer-visible reply. | `CUSTOMER_SUPPORT_MANAGEMENT_EDIT` or `CEO` |
| `POST` | `/api/v1/support/tickets/{ticketRef}/pause`       | Pause SLA (status → `PAUSED`).               | `CUSTOMER_SUPPORT_MANAGEMENT_EDIT` or `CEO` |
| `POST` | `/api/v1/support/tickets/{ticketRef}/resume`      | Resume from pause (extends deadlines).       | `CUSTOMER_SUPPORT_MANAGEMENT_EDIT` or `CEO` |
| `POST` | `/api/v1/support/tickets/{ticketRef}/in-progress` | Set status to `IN_PROGRESS`.                 | `CUSTOMER_SUPPORT_MANAGEMENT_EDIT` or `CEO` |
| `POST` | `/api/v1/support/tickets/{ticketRef}/resolve`     | Resolve ticket.                              | `CUSTOMER_SUPPORT_MANAGEMENT_EDIT` or `CEO` |
| `POST` | `/api/v1/support/tickets/{ticketRef}/close`       | Close ticket after resolve.                  | `CUSTOMER_SUPPORT_MANAGEMENT_EDIT` or `CEO` |

---

## API Details

### `GET` `/api/v1/support/tickets/types`

#### Purpose

Returns **active** ticket types ordered by `displayOrder` for raise-ticket and filter UIs. Always call this instead of hardcoding type ids.

#### Authorization

- Token: **required**
- Authority: **`CUSTOMER_SUPPORT_MANAGEMENT_READ`** or role **`CEO`**
- Tenant: **required** (recommended `X-Tenant-ID`)

#### Path parameters

None.

#### Query parameters

None.

#### Request body

Not applicable.

#### Response

- **HTTP:** `200 OK`
- **Body:** `ResponseStructure<List<SupportTicketTypeResponse>>`
- **Fields (`SupportTicketTypeResponse`):**

| Field   | Type   | Description                                |
| ------- | ------ | ------------------------------------------ |
| `id`    | string | Type id (use in `raise` as `ticketTypeId`) |
| `code`  | string | Stable code (e.g. `COMPLAINT_REEMERGENCE`) |
| `label` | string | Display label                              |

#### Full response JSON examples

**Success — multiple types**

```json
{
  "status": 200,
  "message": "Ticket types",
  "data": [
    {
      "id": "STT-REEM",
      "code": "COMPLAINT_REEMERGENCE",
      "label": "Complaint - Re-emergence"
    },
    {
      "id": "STT-STAFF",
      "code": "COMPLAINT_STAFF",
      "label": "Complaint - Staff"
    }
  ]
}
```

**Success — empty catalog (if none active)**

```json
{
  "status": 200,
  "message": "Ticket types",
  "data": []
}
```

#### Exceptions / error cases

| HTTP Status | Reason       | When It Happens     | Typical Message                               | Frontend Handling Note      |
| ----------- | ------------ | ------------------- | --------------------------------------------- | --------------------------- |
| 401         | Unauthorized | Missing/invalid JWT | `AuthenticationException` message             | Redirect to login           |
| 403         | Forbidden    | Missing authority   | `Access Denied: You don't have permission...` | Hide menu / show permission |
| 500         | Server error | Unexpected          | `RuntimeException` / generic                  | Retry, support              |

#### Error response JSON examples

**Unauthorized (401)**

```json
{
  "timestamp": "2026-04-13T10:15:30",
  "status": 401,
  "error": "Unauthorized",
  "message": "Full authentication is required to access this resource",
  "path": "/api/v1/support/tickets/types",
  "validationErrors": null
}
```

**Forbidden (403)**

```json
{
  "timestamp": "2026-04-13T10:15:30",
  "status": 403,
  "error": "Forbidden",
  "message": "Access Denied: You don't have permission to access this resource",
  "path": "/api/v1/support/tickets/types",
  "validationErrors": null
}
```

#### Frontend notes

- Cache types for the session; **refresh** after admin changes (no push notification in this API).
- Use **`id`** as `ticketTypeId` on raise.

#### cURL

```bash
curl -s -X GET "https://{host}/api/v1/support/tickets/types" \
  -H "Authorization: Bearer {access_token}" \
  -H "X-Tenant-ID: {tenant_schema}" \
  -H "Accept: application/json"
```

---

### `POST` `/api/v1/support/tickets`

#### Purpose

Creates a ticket in **`OPEN`** status, computes SLA deadlines from settings, optionally records attachment paths (raise phase).

#### Authorization

- Token: **required**
- Authority: **`CUSTOMER_SUPPORT_MANAGEMENT_ADD`** or **`CEO`**
- Tenant: **required** (recommended)

#### Path parameters

None.

#### Query parameters

None.

#### Request body fields (`RaiseSupportTicketRequest`)

| Field                    | Type            | Required | Validation                              | Description                                    | Example                     | Allowed values          |
| ------------------------ | --------------- | -------- | --------------------------------------- | ---------------------------------------------- | --------------------------- | ----------------------- |
| `customerId`             | string          | Yes      | `@NotBlank`                             | Customer id                                    | `"CUST-0001"`               | —                       |
| `salesOrderId`           | string          | No       | —                                       | If set, must exist and belong to customer SO   | `"SO-UUID"`                 | —                       |
| `relatedTaskId`          | string          | No       | —                                       | If set, must exist and belong to same customer | `"TASK-UUID"`               | —                       |
| `reportedByName`         | string          | Yes      | `@NotBlank`, max 200                    | Reporter name                                  | `"Jane Doe"`                | —                       |
| `reportedByPhone`        | string          | Yes      | `@NotBlank`, max 20                     | Phone                                          | `"+919876543210"`           | —                       |
| `ticketTypeId`           | string          | Yes      | `@NotBlank`                             | From `GET /types`                              | `"STT-REEM"`                | Active type ids         |
| `priority`               | enum            | Yes      | `@NotNull`                              | Priority                                       | `"HIGH"`                    | `SupportTicketPriority` |
| `subject`                | string          | Yes      | `@NotBlank`, max 100                    | Subject                                        | `"Re-emergence at Site A"`  | —                       |
| `description`            | string          | Yes      | `@NotBlank`, max 1000                   | Description                                    | `"Pests observed again"`    | —                       |
| `expectedResolutionDate` | string (date)   | Yes      | `@NotNull`                              | Must be **today or future** (service)          | `"2026-04-20"`              | ISO date                |
| `expectedResolutionTime` | string (time)   | Yes      | `@NotNull`                              | Combined with date for SLA                     | `"17:00:00"`                | ISO-8601 local time     |
| `attachmentPaths`        | array of string | No       | each max 500; max **5** paths (service) | Storage paths post-upload                      | `["/uploads/tenant/x.png"]` | —                       |

**Branch resolution:** If `salesOrderId` is set, **branch** is taken from the sales order; otherwise from **customer**.

#### Full request JSON examples

**Minimal valid request**

```json
{
  "customerId": "CUST-0001",
  "reportedByName": "Jane Doe",
  "reportedByPhone": "+919876543210",
  "ticketTypeId": "STT-REEM",
  "priority": "NORMAL",
  "subject": "Issue at warehouse",
  "description": "Customer reported issue.",
  "expectedResolutionDate": "2026-04-20",
  "expectedResolutionTime": "17:00:00"
}
```

**Complete valid request (with SO, task, attachments)**

```json
{
  "customerId": "CUST-0001",
  "salesOrderId": "SO-2026-0042",
  "relatedTaskId": "TASK-2026-0088",
  "reportedByName": "Jane Doe",
  "reportedByPhone": "+919876543210",
  "ticketTypeId": "STT-REEM",
  "priority": "URGENT",
  "subject": "Re-emergence complaint",
  "description": "Customer observed re-infestation after last service visit.",
  "expectedResolutionDate": "2026-04-25",
  "expectedResolutionTime": "18:30:00",
  "attachmentPaths": [
    "/uploads/tenant/acme/tickets/photo1.jpg",
    "/uploads/tenant/acme/tickets/photo2.jpg"
  ]
}
```

**Each priority value**

```json
{
  "customerId": "CUST-0001",
  "reportedByName": "Jane Doe",
  "reportedByPhone": "+919876543210",
  "ticketTypeId": "STT-REEM",
  "priority": "CRITICAL",
  "subject": "Critical",
  "description": "Critical priority example.",
  "expectedResolutionDate": "2026-04-14",
  "expectedResolutionTime": "09:00:00"
}
```

#### Response

- **HTTP:** `201 Created`
- **Body:** `ResponseStructure<SupportTicketDetailResponse>` (same shape as detail endpoint — see **`GET`** `/by-id`).

#### Full response JSON example (abbreviated; full shape in detail section)

```json
{
  "status": 201,
  "message": "Ticket raised",
  "data": {
    "ticketId": "TKT-2026-0007",
    "ticketNumber": "TKT-2026-0007",
    "subject": "Issue at warehouse",
    "description": "Customer reported issue.",
    "customerId": "CUST-0001",
    "customerName": "Acme Corp",
    "branchId": "BR-01",
    "branchName": "Head Office",
    "salesOrderId": null,
    "soNumber": null,
    "relatedTaskId": null,
    "linkedTaskNumber": null,
    "ticketTypeId": "STT-REEM",
    "ticketTypeLabel": "Complaint - Re-emergence",
    "priority": "NORMAL",
    "status": "OPEN",
    "reportedByName": "Jane Doe",
    "reportedByPhone": "+919876543210",
    "expectedResolutionDate": "2026-04-20",
    "expectedResolutionTime": "17:00:00",
    "createdAt": "2026-04-13T08:00:00Z",
    "assigneeRoleCode": null,
    "assignedUserId": null,
    "assignedUserName": null,
    "responseSlaDeadlineAt": "2026-04-13T10:00:00Z",
    "firstResponseAt": null,
    "responseSlaMet": null,
    "resolutionExpectedAt": "2026-04-20T11:30:00Z",
    "slaHealth": "SAFE",
    "slaRemainingSeconds": 604800,
    "escalationLevel": "NONE",
    "paused": false,
    "resolutionCode": null,
    "resolutionNotes": null,
    "resolveCustomerRating": null,
    "resolveCustomerFeedback": null,
    "resolvedAt": null,
    "closeReason": null,
    "closureRemarks": null,
    "closedAt": null,
    "activities": [],
    "attachments": []
  }
}
```

**Note:** `createdAt` and SLA instants are **examples**; actual values depend on server clock and SLA settings.

#### Exceptions / error cases

| HTTP Status | Reason         | When It Happens               | Typical Message                                                       | Frontend Handling Note          |
| ----------- | -------------- | ----------------------------- | --------------------------------------------------------------------- | ------------------------------- |
| 400         | Validation     | Bean validation               | `Input validation failed` + field map                                 | Show field errors               |
| 400         | Business       | Past expected resolution date | `Expected resolution date cannot be in the past`                      | Date picker min = today         |
| 400         | Business       | Too many attachments          | `At most 5 attachments are allowed`                                   | Limit UI to 5                   |
| 400         | Business       | Bad customer                  | `Customer not found`                                                  | Revalidate customer picker      |
| 400         | Business       | Bad type                      | `Invalid ticket type`                                                 | Refresh types                   |
| 400         | Business       | SO mismatch                   | `Sales order not found` / `does not belong to the selected customer`  | Validate SO belongs to customer |
| 400         | Business       | Task mismatch                 | `Related task not found` / `does not belong to the selected customer` | Validate task                   |
| 401         | Unauthorized   | Missing/invalid JWT           | —                                                                     | Login                           |
| 403         | Forbidden      | Missing ADD                   | —                                                                     | Hide raise                      |
| 400         | Malformed JSON | Bad JSON                      | `Malformed JSON request`                                              | Fix payload                     |
| 400         | Invalid enum   | Bad priority string           | `Invalid value '...' for field 'priority'`                            | Dropdown only                   |

#### Error response JSON examples

**Validation (Bean Validation)**

```json
{
  "timestamp": "2026-04-13T10:15:30",
  "status": 400,
  "error": "Bad Request",
  "message": "Input validation failed",
  "path": "/api/v1/support/tickets",
  "validationErrors": {
    "subject": "must not be blank"
  }
}
```

**Business rule (ApiBaseException)**

```json
{
  "timestamp": "2026-04-13T10:15:30",
  "status": 400,
  "error": "Bad Request",
  "message": "Expected resolution date cannot be in the past",
  "path": "/api/v1/support/tickets",
  "validationErrors": null
}
```

#### Frontend notes

- Implement **upload** using your **file upload API**; pass returned **paths** in `attachmentPaths`.
- Enforce **max 5** files client-side.
- `expectedResolutionDate` + `expectedResolutionTime` use **server default zone** when combined (`ZoneId.systemDefault()`).

#### cURL

```bash
curl -s -X POST "https://{host}/api/v1/support/tickets" \
  -H "Authorization: Bearer {access_token}" \
  -H "X-Tenant-ID: {tenant_schema}" \
  -H "Content-Type: application/json" \
  -d "{\"customerId\":\"CUST-0001\",\"reportedByName\":\"Jane Doe\",\"reportedByPhone\":\"+919876543210\",\"ticketTypeId\":\"STT-REEM\",\"priority\":\"NORMAL\",\"subject\":\"Issue\",\"description\":\"Details\",\"expectedResolutionDate\":\"2026-04-20\",\"expectedResolutionTime\":\"17:00:00\"}"
```

---

### `GET` `/api/v1/support/tickets`

#### Purpose

Paginated dashboard list with optional filters (branch, created date range, status, priority, SLA health, escalation, free-text search). Sort: **`createdAt` DESC** (fixed in controller).

#### Authorization

- **`CUSTOMER_SUPPORT_MANAGEMENT_READ`** or **`CEO`**

#### Path parameters

None.

#### Query parameters

| Field             | Type   | Required | Description                                                                                                           | Example       | Allowed values           |
| ----------------- | ------ | -------- | --------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------------ |
| `branchId`        | string | No       | Branch filter                                                                                                         | `"BR-01"`     | —                        |
| `createdFrom`     | date   | No       | Inclusive start (day start, server zone)                                                                              | `2026-04-01`  | `YYYY-MM-DD`             |
| `createdTo`       | date   | No       | Inclusive end (exclusive upper bound = start of next day)                                                             | `2026-04-30`  | `YYYY-MM-DD`             |
| `status`          | enum   | No       | Exact status                                                                                                          | `IN_PROGRESS` | `SupportTicketStatus`    |
| `priority`        | enum   | No       | Exact priority                                                                                                        | `HIGH`        | `SupportTicketPriority`  |
| `slaHealth`       | enum   | No       | SLA bucket (see spec)                                                                                                 | `AT_RISK`     | `SupportSlaHealth`       |
| `escalationLevel` | enum   | No       | Escalation                                                                                                            | `L2`          | `SupportEscalationLevel` |
| `search`          | string | No       | Case-insensitive contains on ticket number, id, customer name, SO number, related task id, task number (via subquery) | `TKT-2026`    | —                        |
| `pageNo`          | int    | No       | Page index                                                                                                            | `0`           | default `0`              |
| `pageSize`        | int    | No       | Page size                                                                                                             | `20`          | default `10`             |

#### Request body

Not applicable.

#### Response

- **HTTP:** `200 OK`
- **Body:** `ResponseStructure<PaginationResponse<SupportTicketListResponse>>`

**`PaginationResponse` fields:**

| Field   | Type           | Description                                                                              |
| ------- | -------------- | ---------------------------------------------------------------------------------------- |
| `count` | number         | Total matching rows                                                                      |
| `next`  | string \| null | Relative URL query for next page (includes `pageSize` and filters; `pageNo` incremented) |
| `prev`  | string \| null | Relative URL query for previous page                                                     |
| `data`  | array          | `SupportTicketListResponse` rows                                                         |

**`SupportTicketListResponse` fields:**

| Field                 | Type           | Description                                                              |
| --------------------- | -------------- | ------------------------------------------------------------------------ |
| `ticketId`            | string         | Same as id                                                               |
| `ticketNumber`        | string         | Display number                                                           |
| `customerName`        | string         | From customer                                                            |
| `branchId`            | string         | Branch id                                                                |
| `branchName`          | string         | Branch name                                                              |
| `soNumber`            | string \| null | From SO if linked                                                        |
| `linkedTaskNumber`    | string \| null | Latest task by `ticketId` or related task number                         |
| `priority`            | enum           | `SupportTicketPriority`                                                  |
| `status`              | enum           | `SupportTicketStatus`                                                    |
| `slaHealth`           | enum           | `SupportSlaHealth`                                                       |
| `slaRemainingSeconds` | number \| null | Seconds until resolution deadline; `null` for paused / resolved / closed |
| `escalationLevel`     | enum           | `SupportEscalationLevel`                                                 |

#### Full response JSON examples

**Paginated success**

```json
{
  "status": 200,
  "message": "Tickets",
  "data": {
    "count": 42,
    "next": "/api/v1/support/tickets?pageSize=10&branchId=BR-01&pageNo=1",
    "prev": null,
    "data": [
      {
        "ticketId": "TKT-2026-0007",
        "ticketNumber": "TKT-2026-0007",
        "customerName": "Acme Corp",
        "branchId": "BR-01",
        "branchName": "Head Office",
        "soNumber": "SO-2026-0042",
        "linkedTaskNumber": "TASK-2026-0088",
        "priority": "HIGH",
        "status": "IN_PROGRESS",
        "slaHealth": "AT_RISK",
        "slaRemainingSeconds": 3600,
        "escalationLevel": "L2"
      }
    ]
  }
}
```

**Empty page**

```json
{
  "status": 200,
  "message": "Tickets",
  "data": {
    "count": 0,
    "next": null,
    "prev": null,
    "data": []
  }
}
```

#### Exceptions / error cases

| HTTP Status | Reason            | When                          | Message             | Frontend       |
| ----------- | ----------------- | ----------------------------- | ------------------- | -------------- |
| 400         | Bad enum in query | Invalid `status`/`priority`/… | `Invalid value ...` | Use enums only |
| 401         | Unauthorized      | Missing JWT                   | —                   | Login          |
| 403         | Forbidden         | No READ                       | —                   | —              |

#### Error response JSON example (invalid enum in query)

```json
{
  "timestamp": "2026-04-13T10:15:30",
  "status": 400,
  "error": "Bad Request",
  "message": "Invalid value 'FOO' for field 'status'",
  "path": "/api/v1/support/tickets",
  "validationErrors": null
}
```

#### Frontend notes

- Use **`next`** / **`prev`** as relative paths; **prepend** your API base URL.
- **`search`**: debounce (e.g. 300–500 ms); avoid empty spam.
- **Date range:** `createdTo` is inclusive end-of-day in server zone.

#### cURL

```bash
curl -s -G "https://{host}/api/v1/support/tickets" \
  --data-urlencode "branchId=BR-01" \
  --data-urlencode "status=IN_PROGRESS" \
  --data-urlencode "search=acme" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=10" \
  -H "Authorization: Bearer {access_token}" \
  -H "X-Tenant-ID: {tenant_schema}" \
  -H "Accept: application/json"
```

---

### `GET` `/api/v1/support/tickets/by-id`

#### Purpose

Full ticket detail (metadata, SLA, assignee, activities, attachments).

#### Authorization

- **`CUSTOMER_SUPPORT_MANAGEMENT_READ`** or **`CEO`**

#### Path parameters

None.

#### Query parameters

| Field       | Type   | Required | Description                     | Example         |
| ----------- | ------ | -------- | ------------------------------- | --------------- |
| `ticketRef` | string | **Yes**  | Ticket id **or** `ticketNumber` | `TKT-2026-0007` |

#### Request body

Not applicable.

#### Response

- **HTTP:** `200 OK`
- **Body:** `ResponseStructure<SupportTicketDetailResponse>`

**`SupportTicketDetailResponse` (all fields):**

| Field                     | Type            | Nullable / notes                      |
| ------------------------- | --------------- | ------------------------------------- |
| `ticketId`                | string          |                                       |
| `ticketNumber`            | string          |                                       |
| `subject`                 | string          |                                       |
| `description`             | string          |                                       |
| `customerId`              | string          |                                       |
| `customerName`            | string \| null  |                                       |
| `branchId`                | string          |                                       |
| `branchName`              | string \| null  |                                       |
| `salesOrderId`            | string \| null  |                                       |
| `soNumber`                | string \| null  |                                       |
| `relatedTaskId`           | string \| null  |                                       |
| `linkedTaskNumber`        | string \| null  | Latest task by ticket or related task |
| `ticketTypeId`            | string          |                                       |
| `ticketTypeLabel`         | string \| null  |                                       |
| `priority`                | enum            |                                       |
| `status`                  | enum            |                                       |
| `reportedByName`          | string          |                                       |
| `reportedByPhone`         | string          |                                       |
| `expectedResolutionDate`  | date            |                                       |
| `expectedResolutionTime`  | time            |                                       |
| `createdAt`               | instant         |                                       |
| `assigneeRoleCode`        | string \| null  | Free-text role code from assign       |
| `assignedUserId`          | number \| null  |                                       |
| `assignedUserName`        | string \| null  |                                       |
| `responseSlaDeadlineAt`   | instant         |                                       |
| `firstResponseAt`         | instant \| null |                                       |
| `responseSlaMet`          | boolean \| null | Set when first response / resolve     |
| `resolutionExpectedAt`    | instant         |                                       |
| `slaHealth`               | enum            | Computed                              |
| `slaRemainingSeconds`     | number \| null  |                                       |
| `escalationLevel`         | enum            |                                       |
| `paused`                  | boolean         | `true` if status `PAUSED`             |
| `resolutionCode`          | enum \| null    | After resolve                         |
| `resolutionNotes`         | string \| null  |                                       |
| `resolveCustomerRating`   | number \| null  | 1–5                                   |
| `resolveCustomerFeedback` | string \| null  |                                       |
| `resolvedAt`              | instant \| null |                                       |
| `closeReason`             | enum \| null    | After close                           |
| `closureRemarks`          | string \| null  |                                       |
| `closedAt`                | instant \| null |                                       |
| `activities`              | array           | `SupportTicketActivityResponse`       |
| `attachments`             | array           | `SupportTicketAttachmentResponse`     |

**`SupportTicketActivityResponse`:**

| Field              | Type           | Description                                                                        |
| ------------------ | -------------- | ---------------------------------------------------------------------------------- |
| `id`               | number         | Activity id                                                                        |
| `activityType`     | string         | e.g. `CREATED`, `ASSIGNED`, `NOTE`, `REPLY`, `PAUSE`, `RESOLVED`, `SLA_ESCALATION` |
| `summary`          | string         |                                                                                    |
| `detail`           | string \| null | Extra detail                                                                       |
| `internal`         | boolean        | Internal note flag                                                                 |
| `performedByLabel` | string \| null |                                                                                    |
| `performedAt`      | instant        | Sorted ascending in response                                                       |

**`SupportTicketAttachmentResponse`:**

| Field          | Type           | Description              |
| -------------- | -------------- | ------------------------ |
| `id`           | string         |                          |
| `phase`        | enum           | `SupportAttachmentPhase` |
| `filePath`     | string         | Storage path             |
| `originalName` | string \| null |                          |
| `uploadedAt`   | instant        |                          |

#### Full response JSON examples

**Success — in progress**

```json
{
  "status": 200,
  "message": "Ticket detail",
  "data": {
    "ticketId": "TKT-2026-0007",
    "ticketNumber": "TKT-2026-0007",
    "subject": "Re-emergence complaint",
    "description": "Customer observed re-infestation.",
    "customerId": "CUST-0001",
    "customerName": "Acme Corp",
    "branchId": "BR-01",
    "branchName": "Head Office",
    "salesOrderId": "SO-2026-0042",
    "soNumber": "SO-2026-0042",
    "relatedTaskId": "TASK-2026-0088",
    "linkedTaskNumber": "TASK-2026-0088",
    "ticketTypeId": "STT-REEM",
    "ticketTypeLabel": "Complaint - Re-emergence",
    "priority": "HIGH",
    "status": "IN_PROGRESS",
    "reportedByName": "Jane Doe",
    "reportedByPhone": "+919876543210",
    "expectedResolutionDate": "2026-04-25",
    "expectedResolutionTime": "18:30:00",
    "createdAt": "2026-04-13T08:00:00Z",
    "assigneeRoleCode": "L1_SUPPORT",
    "assignedUserId": 42,
    "assignedUserName": "Support Agent",
    "responseSlaDeadlineAt": "2026-04-13T10:00:00Z",
    "firstResponseAt": "2026-04-13T09:30:00Z",
    "responseSlaMet": true,
    "resolutionExpectedAt": "2026-04-25T13:00:00Z",
    "slaHealth": "SAFE",
    "slaRemainingSeconds": 950000,
    "escalationLevel": "NONE",
    "paused": false,
    "resolutionCode": null,
    "resolutionNotes": null,
    "resolveCustomerRating": null,
    "resolveCustomerFeedback": null,
    "resolvedAt": null,
    "closeReason": null,
    "closureRemarks": null,
    "closedAt": null,
    "activities": [
      {
        "id": 1,
        "activityType": "CREATED",
        "summary": "Ticket TKT-2026-0007 raised",
        "detail": null,
        "internal": false,
        "performedByLabel": "agent@example.com",
        "performedAt": "2026-04-13T08:00:00Z"
      }
    ],
    "attachments": [
      {
        "id": "att-0001",
        "phase": "RAISE",
        "filePath": "/uploads/tenant/acme/tickets/photo1.jpg",
        "originalName": null,
        "uploadedAt": "2026-04-13T08:00:05Z"
      }
    ]
  }
}
```

**Not found**

```json
{
  "timestamp": "2026-04-13T10:15:30",
  "status": 404,
  "error": "Not Found",
  "message": "Support ticket not found",
  "path": "/api/v1/support/tickets/by-id",
  "validationErrors": null
}
```

#### Frontend notes

- Poll or refresh detail after actions that change status.
- **Activities:** `activityType` `REPLY` vs `NOTE` is driven by `customerVisibleReply` on add-note API.

#### cURL

```bash
curl -s -G "https://{host}/api/v1/support/tickets/by-id" \
  --data-urlencode "ticketRef=TKT-2026-0007" \
  -H "Authorization: Bearer {access_token}" \
  -H "X-Tenant-ID: {tenant_schema}" \
  -H "Accept: application/json"
```

---

### `POST` `/api/v1/support/tickets/{ticketRef}/assign`

#### Purpose

Assigns a user, sets **assignee role code**, and records assignment history. If status is **`OPEN`**, sets to **`ASSIGNED`**. Triggers **first response** timestamp if not set.

#### Authorization

- **`CUSTOMER_SUPPORT_MANAGEMENT_EDIT`** or **`CEO`**

#### Path parameters

| Field       | Type   | Required | Description         | Example         |
| ----------- | ------ | -------- | ------------------- | --------------- |
| `ticketRef` | string | Yes      | Id or ticket number | `TKT-2026-0007` |

#### Query parameters

None.

#### Request body (`AssignSupportTicketRequest`)

| Field              | Type   | Required | Validation  | Description                           | Example                |
| ------------------ | ------ | -------- | ----------- | ------------------------------------- | ---------------------- |
| `assigneeRoleCode` | string | Yes      | `@NotBlank` | Role code for UI (not a backend enum) | `"L1_SUPPORT"`         |
| `assignToUserId`   | number | Yes      | `@NotNull`  | User id                               | `42`                   |
| `assignmentNote`   | string | No       | —           | Optional note                         | `"Handing over to L1"` |

#### Full request JSON examples

**Standard assign**

```json
{
  "assigneeRoleCode": "L1_SUPPORT",
  "assignToUserId": 42,
  "assignmentNote": "Please follow up today."
}
```

**Minimal (no note)**

```json
{
  "assigneeRoleCode": "L1_SUPPORT",
  "assignToUserId": 42
}
```

#### Response

- **HTTP:** `200 OK`
- **Body:** `ResponseStructure<SupportTicketDetailResponse>` (updated ticket)

#### Exceptions / error cases

| HTTP Status | Reason     | Message (examples)                                       |
| ----------- | ---------- | -------------------------------------------------------- |
| 400         | Validation | Bean validation on `assigneeRoleCode` / `assignToUserId` |
| 400         | Business   | `Assignee user not found`                                |
| 400         | Business   | `Ticket is read-only in this status` (resolved/closed)   |
| 404         | Not found  | `Support ticket not found`                               |

#### Error JSON example

```json
{
  "timestamp": "2026-04-13T10:15:30",
  "status": 400,
  "error": "Bad Request",
  "message": "Ticket is read-only in this status",
  "path": "/api/v1/support/tickets/TKT-2026-0007/assign",
  "validationErrors": null
}
```

#### cURL

```bash
curl -s -X POST "https://{host}/api/v1/support/tickets/TKT-2026-0007/assign" \
  -H "Authorization: Bearer {access_token}" \
  -H "X-Tenant-ID: {tenant_schema}" \
  -H "Content-Type: application/json" \
  -d "{\"assigneeRoleCode\":\"L1_SUPPORT\",\"assignToUserId\":42,\"assignmentNote\":\"Please follow up\"}"
```

---

### `POST` `/api/v1/support/tickets/{ticketRef}/notes`

#### Purpose

**Internal note** or **customer-visible reply** (activity type `NOTE` vs `REPLY`). Updates first response if needed.

#### Authorization

- **`CUSTOMER_SUPPORT_MANAGEMENT_EDIT`** or **`CEO`**

#### Path parameters

| Field       | Type   | Required | Description  | Example         |
| ----------- | ------ | -------- | ------------ | --------------- |
| `ticketRef` | string | Yes      | Id or number | `TKT-2026-0007` |

#### Request body (`AddSupportTicketNoteRequest`)

| Field                  | Type    | Required | Validation            | Description                                              |
| ---------------------- | ------- | -------- | --------------------- | -------------------------------------------------------- |
| `note`                 | string  | Yes      | `@NotBlank`, max 4000 | Note body                                                |
| `internal`             | boolean | No       | default `false`       | Internal flag on activity                                |
| `customerVisibleReply` | boolean | No       | default `false`       | If `true`, activity type is **`REPLY`**; else **`NOTE`** |

#### Full request JSON examples

**Internal note**

```json
{
  "note": "Called customer, left voicemail.",
  "internal": true,
  "customerVisibleReply": false
}
```

**Customer-visible reply**

```json
{
  "note": "We have scheduled a visit for tomorrow 10 AM.",
  "internal": false,
  "customerVisibleReply": true
}
```

#### Response

- **200 OK** + `SupportTicketDetailResponse`

#### Exceptions

| Status | When            | Message                              |
| ------ | --------------- | ------------------------------------ |
| 400    | Resolved/closed | `Ticket is read-only in this status` |
| 400    | Validation      | Field errors on `note`               |
| 404    | Not found       | `Support ticket not found`           |

#### cURL

```bash
curl -s -X POST "https://{host}/api/v1/support/tickets/TKT-2026-0007/notes" \
  -H "Authorization: Bearer {access_token}" \
  -H "X-Tenant-ID: {tenant_schema}" \
  -H "Content-Type: application/json" \
  -d "{\"note\":\"Internal note\",\"internal\":true,\"customerVisibleReply\":false}"
```

---

### `POST` `/api/v1/support/tickets/{ticketRef}/pause`

#### Purpose

Sets status to **`PAUSED`** and records `pauseStartedAt` (SLA extension logic on resume).

#### Authorization

- **`CUSTOMER_SUPPORT_MANAGEMENT_EDIT`** or **`CEO`**

#### Path parameters

`ticketRef` — required.

#### Query / body

None.

#### Request body

Not applicable.

#### Response

- **200 OK** + detail (`paused: true`)

#### Exceptions

| Status | Message                                              |
| ------ | ---------------------------------------------------- |
| 400    | `Only assigned or in-progress tickets can be paused` |
| 400    | `Ticket is already paused`                           |
| 404    | `Support ticket not found`                           |

#### Error JSON

```json
{
  "timestamp": "2026-04-13T10:15:30",
  "status": 400,
  "error": "Bad Request",
  "message": "Only assigned or in-progress tickets can be paused",
  "path": "/api/v1/support/tickets/TKT-2026-0007/pause",
  "validationErrors": null
}
```

#### cURL

```bash
curl -s -X POST "https://{host}/api/v1/support/tickets/TKT-2026-0007/pause" \
  -H "Authorization: Bearer {access_token}" \
  -H "X-Tenant-ID: {tenant_schema}" \
  -H "Accept: application/json"
```

---

### `POST` `/api/v1/support/tickets/{ticketRef}/resume`

#### Purpose

Clears pause, adds elapsed pause duration to **total paused seconds**, **extends** `responseSlaDeadlineAt` and `resolutionExpectedAt` by that duration, sets status to **`IN_PROGRESS`**.

#### Authorization

- **`CUSTOMER_SUPPORT_MANAGEMENT_EDIT`** or **`CEO`**

#### Request body

Not applicable.

#### Response

- **200 OK** + detail

#### Exceptions

| Status | Message                    |
| ------ | -------------------------- |
| 400    | `Ticket is not paused`     |
| 404    | `Support ticket not found` |

#### cURL

```bash
curl -s -X POST "https://{host}/api/v1/support/tickets/TKT-2026-0007/resume" \
  -H "Authorization: Bearer {access_token}" \
  -H "X-Tenant-ID: {tenant_schema}" \
  -H "Accept: application/json"
```

---

### `POST` `/api/v1/support/tickets/{ticketRef}/in-progress`

#### Purpose

Sets status to **`IN_PROGRESS`** from **`OPEN`** or **`ASSIGNED`**.

#### Authorization

- **`CUSTOMER_SUPPORT_MANAGEMENT_EDIT`** or **`CEO`**

#### Request body

Not applicable.

#### Response

- **200 OK** + detail

#### Exceptions

| Status | Message                                    |
| ------ | ------------------------------------------ |
| 400    | `Invalid status transition to in progress` |
| 404    | `Support ticket not found`                 |

#### cURL

```bash
curl -s -X POST "https://{host}/api/v1/support/tickets/TKT-2026-0007/in-progress" \
  -H "Authorization: Bearer {access_token}" \
  -H "X-Tenant-ID: {tenant_schema}" \
  -H "Accept: application/json"
```

---

### `POST` `/api/v1/support/tickets/{ticketRef}/resolve`

#### Purpose

Marks ticket **`RESOLVED`**, stores resolution + customer feedback, optional resolution attachments. **Blocks** if any task linked to this ticket id has status **not** `COMPLETED`.

#### Authorization

- **`CUSTOMER_SUPPORT_MANAGEMENT_EDIT`** or **`CEO`**

#### Request body (`ResolveSupportTicketRequest`)

| Field              | Type            | Required | Validation                      | Description               |
| ------------------ | --------------- | -------- | ------------------------------- | ------------------------- |
| `resolutionCode`   | enum            | Yes      | `@NotNull`                      | `SupportResolutionCode`   |
| `resolutionNotes`  | string          | Yes      | `@NotBlank`, max 4000           | Internal resolution notes |
| `customerRating`   | number          | Yes      | `@NotNull`, `@Min(1)` `@Max(5)` | Rating                    |
| `customerFeedback` | string          | Yes      | `@NotBlank`, max 2000           | Customer feedback text    |
| `attachmentPaths`  | array of string | No       | each max 500; max **5** total   | Resolution phase paths    |

#### Full request JSON examples

**Minimal valid**

```json
{
  "resolutionCode": "SERVICE_RESOLVED_SUCCESS",
  "resolutionNotes": "Technician completed service; customer confirmed.",
  "customerRating": 5,
  "customerFeedback": "Issue resolved to satisfaction."
}
```

**Each resolution code**

```json
{
  "resolutionCode": "FALSE_ALARM",
  "resolutionNotes": "No pest activity found on inspection.",
  "customerRating": 3,
  "customerFeedback": "Customer agrees it was a false alarm."
}
```

```json
{
  "resolutionCode": "DUPLICATE_TICKET",
  "resolutionNotes": "Duplicate of TKT-2026-0003.",
  "customerRating": 2,
  "customerFeedback": "Explained duplicate handling."
}
```

```json
{
  "resolutionCode": "UNRESOLVED_CLOSED",
  "resolutionNotes": "Could not resolve; customer unreachable.",
  "customerRating": 1,
  "customerFeedback": "We tried multiple times."
}
```

**With attachments**

```json
{
  "resolutionCode": "SERVICE_RESOLVED_SUCCESS",
  "resolutionNotes": "Service completed with photos.",
  "customerRating": 5,
  "customerFeedback": "Satisfied.",
  "attachmentPaths": ["/uploads/tenant/acme/tickets/resolution/after1.jpg"]
}
```

#### Response

- **200 OK** + detail (`status: RESOLVED`, `resolvedAt` set)

#### Exceptions

| Status | Message                                                             |
| ------ | ------------------------------------------------------------------- |
| 400    | `Ticket is already closed or resolved`                              |
| 400    | `Cannot resolve ticket: one or more linked tasks are not completed` |
| 400    | `At most 5 attachments are allowed`                                 |
| 400    | Validation (rating 1–5, blank fields)                               |
| 404    | `Support ticket not found`                                          |

#### Error JSON (linked tasks)

```json
{
  "timestamp": "2026-04-13T10:15:30",
  "status": 400,
  "error": "Bad Request",
  "message": "Cannot resolve ticket: one or more linked tasks are not completed",
  "path": "/api/v1/support/tickets/TKT-2026-0007/resolve",
  "validationErrors": null
}
```

#### Frontend notes

- Ensure **Module 21 tasks** linked to this ticket are **`COMPLETED`** before enabling submit.
- Rating **1–5** enforced server-side.

#### cURL

```bash
curl -s -X POST "https://{host}/api/v1/support/tickets/TKT-2026-0007/resolve" \
  -H "Authorization: Bearer {access_token}" \
  -H "X-Tenant-ID: {tenant_schema}" \
  -H "Content-Type: application/json" \
  -d "{\"resolutionCode\":\"SERVICE_RESOLVED_SUCCESS\",\"resolutionNotes\":\"Done\",\"customerRating\":5,\"customerFeedback\":\"Good\"}"
```

---

### `POST` `/api/v1/support/tickets/{ticketRef}/close`

#### Purpose

Permanently closes ticket from **`RESOLVED`** only.

#### Authorization

- **`CUSTOMER_SUPPORT_MANAGEMENT_EDIT`** or **`CEO`**

#### Request body (`CloseSupportTicketRequest`)

| Field             | Type            | Required | Validation            | Description          |
| ----------------- | --------------- | -------- | --------------------- | -------------------- |
| `closeReason`     | enum            | Yes      | `@NotNull`            | `SupportCloseReason` |
| `closureRemarks`  | string          | Yes      | `@NotBlank`, max 4000 | Remarks              |
| `attachmentPaths` | array of string | No       | each max 500          | Close phase          |

#### Full request JSON examples

**Standard close**

```json
{
  "closeReason": "RESOLVED_CUSTOMER_SATISFACTION",
  "closureRemarks": "Customer confirmed closure on call."
}
```

**Other reasons**

```json
{
  "closeReason": "DUPLICATE_REQUEST",
  "closureRemarks": "Merged with primary ticket."
}
```

```json
{
  "closeReason": "CUSTOMER_NON_RESPONSIVE",
  "closureRemarks": "No response after 3 attempts."
}
```

```json
{
  "closeReason": "OUT_OF_SCOPE",
  "closureRemarks": "Request outside contract scope."
}
```

#### Response

- **200 OK** + detail (`status: CLOSED`)

#### Exceptions

| Status | Message                               |
| ------ | ------------------------------------- |
| 400    | `Only resolved tickets can be closed` |
| 404    | `Support ticket not found`            |

#### Error JSON

```json
{
  "timestamp": "2026-04-13T10:15:30",
  "status": 400,
  "error": "Bad Request",
  "message": "Only resolved tickets can be closed",
  "path": "/api/v1/support/tickets/TKT-2026-0007/close",
  "validationErrors": null
}
```

#### cURL

```bash
curl -s -X POST "https://{host}/api/v1/support/tickets/TKT-2026-0007/close" \
  -H "Authorization: Bearer {access_token}" \
  -H "X-Tenant-ID: {tenant_schema}" \
  -H "Content-Type: application/json" \
  -d "{\"closeReason\":\"RESOLVED_CUSTOMER_SATISFACTION\",\"closureRemarks\":\"Closure confirmed\"}"
```

---

## Validation and Exception Summary

| Field / scenario               | Validation / rule                                | Error type                     | Frontend impact        |
| ------------------------------ | ------------------------------------------------ | ------------------------------ | ---------------------- |
| Raise `expectedResolutionDate` | must be ≥ today                                  | 400 message                    | Min date today         |
| Raise `attachmentPaths`        | Max 5 items                                      | 400 message                    | Cap uploads            |
| Raise `customerId`             | Must exist                                       | 400 message                    | Valid picker           |
| Raise `salesOrderId`           | Must exist + match customer                      | 400 message                    | SO–customer coupling   |
| Raise `relatedTaskId`          | Must exist + match customer                      | 400 message                    | Task–customer coupling |
| `priority` / enums in JSON     | Valid enum strings                               | 400 malformed / invalid format | Strict dropdowns       |
| List query enums               | Valid `SupportTicketStatus` etc.                 | 400 invalid format             | Encode query safely    |
| `resolve` `customerRating`     | 1–5                                              | 400                            | Star rating            |
| `resolve` `attachmentPaths`    | Max 5                                            | 400                            | Cap                    |
| Resolve when tasks incomplete  | `existsByTicketIdAndStatusIsNot(..., COMPLETED)` | 400                            | Block until tasks done |
| Assign / note on closed        | `ensureMutable`                                  | 400                            | Disable actions        |
| Pause                          | Status ASSIGNED or IN_PROGRESS                   | 400                            | Button enable rules    |
| Resume                         | Status PAUSED                                    | 400                            | —                      |
| In progress                    | from OPEN or ASSIGNED                            | 400                            | —                      |
| Close                          | status RESOLVED                                  | 400                            | Linear flow            |
| Unknown ticket                 | `ticketRef`                                      | 404                            | Not found page         |
| Missing JWT                    | —                                                | 401                            | Login                  |
| Missing permission             | `@PreAuthorize`                                  | 403                            | Hide features          |

---

## Frontend Integration Notes

- **Dropdowns:** Load ticket types from **`GET /types`**; load users/branches/customers/SOs from their respective modules (not in this controller).
- **Ticket reference:** Use **`ticketId`** or **`ticketNumber`** interchangeably in URLs for `ticketRef`.
- **Status UI:**
  - **Assign / notes:** hide when `RESOLVED` or `CLOSED`.
  - **Pause:** only `ASSIGNED` or `IN_PROGRESS`.
  - **Resume:** only `PAUSED`.
  - **In progress:** from `OPEN` or `ASSIGNED`.
  - **Resolve:** not `RESOLVED`/`CLOSED`; check **tasks**.
  - **Close:** only `RESOLVED`.
- **Enums:** Send **exact** uppercase enum strings as in tables.
- **Dates/times:** `LocalDate` `YYYY-MM-DD`; `LocalTime` ISO; instants in responses as ISO-8601 UTC.
- **Pagination:** `pageNo` 0-based; `pageSize` default 10; use `next`/`prev` with base URL.
- **Search:** Debounce; align with backend `LIKE` behavior.
- **Files:** Upload elsewhere → pass **paths** only; no multipart in this module.
- **Headers:** Always send **`Authorization`**; send **`X-Tenant-ID`** to match tenant JWT.
- **CEO:** Users with **`CEO`** role can call all endpoints per `@PreAuthorize` (same as granular permissions).

---

## Assumptions / missing from backend context

- **File upload API** and URL format for `attachmentPaths` are **not** defined in this module (paths are opaque strings).
- **Assignee role codes** (`assigneeRoleCode`) are **not** validated against a fixed enum in this module.
- **ROOT** user bypass is **not** declared on this controller; only **`CEO`** is named alongside granular authorities.
- **`SupportAttachmentPhase.NOTE`** is not used when saving attachments in the current service implementation.

---

_Generated from backend sources: `SupportTicketController`, `SupportTicketServiceImpl`, request/response DTOs, enums, `SupportTicketSpecification`, `GlobalExceptionHandler`, `ValidationErrorResponse`, `PaginationResponse`, `ResponseStructure`._
