# Module 23 — Customer Support Management (Frontend Integration Guide)

This document helps frontend developers integrate the **Customer Support** module with the backend APIs. It is written in simple language and is organized **screen by screen**, matching the product flows (dashboard, raise ticket, detail, modals).

---

## 1. How the API works (read this first)

### 1.1 Base URL

All support ticket endpoints share this prefix:

```text
/api/v1/support/tickets
```

Example full URL: `https://your-host/api/v1/support/tickets`

### 1.2 Authentication

Every request must include a **valid JWT** for a logged-in user (same as the rest of the app):

```http
Authorization: Bearer <access_token>
```

### 1.3 Standard response wrapper

Successful responses use this shape (see `ResponseStructure` in the backend):

| Field     | Type   | Meaning                                 |
| --------- | ------ | --------------------------------------- |
| `status`  | number | HTTP status (e.g. `200`, `201`)         |
| `message` | string | Short human-readable message            |
| `data`    | object | The actual payload (detail, list, etc.) |

Errors (validation, bad request, not found) follow the app’s global error format (RFC 7807-style / validation map). Field names in JSON usually match **camelCase** (e.g. `ticketNumber`, `customerId`).

### 1.4 Permissions (authorities)

| Authority                          | Typical use on UI                                         |
| ---------------------------------- | --------------------------------------------------------- |
| `CUSTOMER_SUPPORT_MANAGEMENT_READ` | View dashboard, types, detail, list                       |
| `CUSTOMER_SUPPORT_MANAGEMENT_ADD`  | Raise ticket                                              |
| `CUSTOMER_SUPPORT_MANAGEMENT_EDIT` | Assign, notes, pause, resume, in-progress, resolve, close |

Users with role **`CEO`** can access these endpoints as implemented today.

---

## 2. Global reference — enums (use these exact string values)

Send enum values in JSON **exactly as below** (uppercase, underscores).

### 2.1 `SupportTicketStatus` (ticket lifecycle)

| Value         | Meaning                                                |
| ------------- | ------------------------------------------------------ |
| `OPEN`        | New ticket, not yet fully worked                       |
| `ASSIGNED`    | A support user is assigned                             |
| `IN_PROGRESS` | Actively being worked on                               |
| `PAUSED`      | Waiting on customer (SLA handling paused in app logic) |
| `RESOLVED`    | Issue fixed; confirmation / close pending              |
| `CLOSED`      | Permanently closed                                     |

### 2.2 `SupportTicketPriority`

| Value      | Meaning           |
| ---------- | ----------------- |
| `NORMAL`   | Standard priority |
| `HIGH`     | High              |
| `URGENT`   | Urgent            |
| `CRITICAL` | Critical          |

### 2.3 `SupportSlaHealth` (dashboard / list — computed)

| Value      | Meaning (simplified)                           |
| ---------- | ---------------------------------------------- |
| `SAFE`     | Within SLA window (before “at risk” threshold) |
| `AT_RISK`  | Entered risk window (e.g. last 20% of time)    |
| `BREACHED` | Past resolution deadline (for active tickets)  |

### 2.4 `SupportEscalationLevel`

| Value  | Meaning                                 |
| ------ | --------------------------------------- |
| `NONE` | No escalation                           |
| `L1`   | Level 1 (e.g. response SLA concern)     |
| `L2`   | Level 2 (e.g. before deadline risk)     |
| `L3`   | Level 3 (e.g. past resolution deadline) |

### 2.5 `SupportResolutionCode` (resolve modal)

| Value                      | Meaning                       |
| -------------------------- | ----------------------------- |
| `SERVICE_RESOLVED_SUCCESS` | Service resolved successfully |
| `FALSE_ALARM`              | False alarm / not found       |
| `DUPLICATE_TICKET`         | Duplicate ticket              |
| `UNRESOLVED_CLOSED`        | Closed unresolved             |

### 2.6 `SupportCloseReason` (close modal)

| Value                            | Meaning                           |
| -------------------------------- | --------------------------------- |
| `RESOLVED_CUSTOMER_SATISFACTION` | Resolved to customer satisfaction |
| `DUPLICATE_REQUEST`              | Duplicate request                 |
| `CUSTOMER_NON_RESPONSIVE`        | Customer not responsive           |
| `OUT_OF_SCOPE`                   | Out of scope                      |

### 2.7 `SupportAttachmentPhase` (attachments in responses)

| Value        | Meaning                         |
| ------------ | ------------------------------- |
| `RAISE`      | Uploaded when ticket was raised |
| `RESOLUTION` | Uploaded at resolve             |
| `CLOSE`      | Uploaded at close               |
| `NOTE`       | Linked to a note (if used)      |

---

## 3. Seeded ticket types (Raise ticket & filters)

After DB migration, these **ticket type IDs** exist (use `id` in `GET /types` and in `raise` `ticketTypeId`):

| `id`        | `code`                  | Example label            |
| ----------- | ----------------------- | ------------------------ |
| `STT-REEM`  | `COMPLAINT_REEMERGENCE` | Complaint - Re-emergence |
| `STT-STAFF` | `COMPLAINT_STAFF`       | Complaint - Staff        |
| `STT-BILL`  | `QUERY_BILLING`         | Query - Billing          |
| `STT-SVC`   | `QUERY_SERVICE`         | Query - Service          |

Always load the **live list** from **`GET /api/v1/support/tickets/types`** so the UI stays in sync if admin adds types later.

---

## 4. Screen-by-screen integration

---

### Screen 23.1 — Ticket Dashboard (SLA-driven list)

**Purpose:** Paginated table with filters (branch, dates, status, priority, SLA health, escalation, search).

#### APIs used

| Method | Endpoint                  | Permission                               |
| ------ | ------------------------- | ---------------------------------------- |
| `GET`  | `/api/v1/support/tickets` | `CUSTOMER_SUPPORT_MANAGEMENT_READ` / CEO |

Optional: `GET /api/v1/support/tickets/types` if you show a **Type** column (not filtered by type in API yet — see gaps).

#### Query parameters (all optional except paging defaults)

| Name              | Type              | Description                                                        |
| ----------------- | ----------------- | ------------------------------------------------------------------ |
| `branchId`        | string            | Filter by branch id (see Module 6)                                 |
| `createdFrom`     | date `YYYY-MM-DD` | Ticket created on/after this date                                  |
| `createdTo`       | date `YYYY-MM-DD` | Ticket created before end of this day                              |
| `status`          | enum              | See `SupportTicketStatus`                                          |
| `priority`        | enum              | See `SupportTicketPriority`                                        |
| `slaHealth`       | enum              | See `SupportSlaHealth`                                             |
| `escalationLevel` | enum              | See `SupportEscalationLevel`                                       |
| `search`          | string            | Free text: ticket id/number, customer name, SO number, task number |
| `pageNo`          | number            | Page index, default `0`                                            |
| `pageSize`        | number            | Page size, default `10`                                            |

**Request body:** none.

#### Response `data` shape

`data` is a **pagination object**:

| Field   | Type           | Description                                               |
| ------- | -------------- | --------------------------------------------------------- |
| `count` | number         | Total rows matching filters                               |
| `next`  | string \| null | Relative query string for next page (if backend provides) |
| `prev`  | string \| null | Relative query string for previous page                   |
| `data`  | array          | List of `SupportTicketListResponse`                       |

Each **row** (`SupportTicketListResponse`):

| Field                 | Type           | Description                                                          |
| --------------------- | -------------- | -------------------------------------------------------------------- |
| `ticketId`            | string         | Internal id (same as `ticketNumber` in current flow)                 |
| `ticketNumber`        | string         | Display id e.g. `TKT-2026-0001`                                      |
| `customerName`        | string         | Customer display name                                                |
| `branchId`            | string         | Branch id                                                            |
| `branchName`          | string         | Branch name                                                          |
| `soNumber`            | string \| null | Linked sales order number if any                                     |
| `linkedTaskNumber`    | string \| null | Latest linked task number from Module 21, or related task            |
| `priority`            | enum           | `SupportTicketPriority`                                              |
| `status`              | enum           | `SupportTicketStatus`                                                |
| `slaHealth`           | enum           | `SupportSlaHealth`                                                   |
| `slaRemainingSeconds` | number \| null | Seconds until resolution deadline; `null` for paused/closed/resolved |
| `escalationLevel`     | enum           | `SupportEscalationLevel`                                             |

#### UI validations & edge cases

- Empty filters = all tickets (subject to pagination).
- `search` can match multiple fields; keep **minimum length** sensible in UX (e.g. 2+ chars) to avoid huge scans.
- Format **SLA timer** from `slaRemainingSeconds` (e.g. `22h 10m` or negative if overdue — backend may return negative seconds when past deadline).
- **Row click → detail:** navigate with `ticketId` or `ticketNumber` and call **`GET /by-id`** (see Screen 23.3).

#### Dependencies (other modules)

| Need                 | Module                  | API hint                                                           |
| -------------------- | ----------------------- | ------------------------------------------------------------------ |
| Branch dropdown      | `Branch`                | `GET /api/v1/company/branches` (list branches for filter)          |
| Customer name search | `Customer`              | `GET /api/v1/customer` (filter) or `GET /api/v1/customer/dropdown` |
| SO / task search     | `Sales Order` / `Tasks` | `/api/v1/sales-orders` and `/api/v1/tasks` for cross-links only    |

---

### Screen 23.2 — Raise New Ticket

**Purpose:** Create a ticket with customer, optional SO, optional related task, type, priority, subject, description, expected resolution date/time, optional attachment paths.

#### APIs used

| Method | Endpoint                        | Permission                              |
| ------ | ------------------------------- | --------------------------------------- |
| `GET`  | `/api/v1/support/tickets/types` | READ                                    |
| `POST` | `/api/v1/support/tickets`       | `CUSTOMER_SUPPORT_MANAGEMENT_ADD` / CEO |

**Dependencies for dropdowns / cascading:**

| Field                              | Module    | API                                                                                                        |
| ---------------------------------- | --------- | ---------------------------------------------------------------------------------------------------------- |
| Customer                           | Module 18 | **`GET /api/v1/customer/dropdown`** (quick dropdown) or **`GET /api/v1/customer`** with filters for search |
| Sales Order (after customer)       | Module 20 | Filter SOs by customer — e.g. sales order APIs under `/api/v1/sales-orders`                                |
| Related Task (after SO / customer) | Module 21 | `GET /api/v1/tasks` with filters (date, search, etc.) — show tasks for that customer/SO                    |

#### Request body — `RaiseSupportTicketRequest`

| Field                    | Required | Type     | Max / rules                          | Description                                                                          |
| ------------------------ | -------- | -------- | ------------------------------------ | ------------------------------------------------------------------------------------ |
| `customerId`             | Yes      | string   | —                                    | Customer id from Module 18                                                           |
| `salesOrderId`           | No       | string   | —                                    | Must belong to same customer if set                                                  |
| `relatedTaskId`          | No       | string   | —                                    | Task id from Module 21; must belong to same customer                                 |
| `reportedByName`         | Yes      | string   | 200                                  | Reporter name                                                                        |
| `reportedByPhone`        | Yes      | string   | 20                                   | Phone (no extra format check in API yet)                                             |
| `ticketTypeId`           | Yes      | string   | —                                    | From `GET /types` (`id`)                                                             |
| `priority`               | Yes      | enum     | —                                    | `SupportTicketPriority`                                                              |
| `subject`                | Yes      | string   | 100                                  | Short title                                                                          |
| `description`            | Yes      | string   | 1000                                 | Long description                                                                     |
| `expectedResolutionDate` | Yes      | date     | —                                    | `YYYY-MM-DD`, **cannot be in the past**                                              |
| `expectedResolutionTime` | Yes      | time     | —                                    | `HH:mm:ss` or `HH:mm` (ISO local time)                                               |
| `attachmentPaths`        | No       | string[] | max 5 paths, each path max 500 chars | **Storage paths** after upload via your file service (same pattern as other modules) |

#### Response `data`

`SupportTicketDetailResponse` — full ticket (same structure as detail screen; see section 6).

**HTTP status:** `201 Created` on success.

#### Backend validations & errors (user-facing messages)

| Scenario                   | Error                                                   |
| -------------------------- | ------------------------------------------------------- |
| Past expected date         | `Expected resolution date cannot be in the past`        |
| More than 5 attachments    | `At most 5 attachments are allowed`                     |
| Invalid `customerId`       | `Customer not found`                                    |
| Invalid `ticketTypeId`     | `Invalid ticket type`                                   |
| SO not found               | `Sales order not found`                                 |
| SO not for this customer   | `Sales order does not belong to the selected customer`  |
| Task not found             | `Related task not found`                                |
| Task not for this customer | `Related task does not belong to the selected customer` |
| Missing/invalid fields     | `400` with field-level validation errors                |

#### Step-by-step (frontend)

1. Load **ticket types**: `GET /types` → populate dropdown.
2. User selects **customer** (from customer module) → set `customerId`.
3. Optionally load **sales orders** for that customer → set `salesOrderId`.
4. Optionally load **tasks** for that customer/SO → set `relatedTaskId`.
5. User fills reporter, phone, subject, description, expected date/time, priority.
6. Upload files elsewhere → collect **paths** → `attachmentPaths` (max 5).
7. `POST /api/v1/support/tickets` → redirect to detail with returned `ticketId` / `ticketNumber`.

---

### Screen 23.3 — Ticket Detail View

**Purpose:** Full ticket, SLA summary, assignee, timeline, actions (assign, note, pause, resume, in-progress, resolve, close). **Convert to task** is a **navigation to Module 21** (see 23.5).

#### APIs used

| Method | Endpoint                                                 | Permission |
| ------ | -------------------------------------------------------- | ---------- |
| `GET`  | `/api/v1/support/tickets/by-id?ticketRef=<id-or-number>` | READ       |

#### Query parameters

| Name        | Required | Description                                               |
| ----------- | -------- | --------------------------------------------------------- |
| `ticketRef` | Yes      | Ticket **id** or **ticket number** (e.g. `TKT-2026-0001`) |

No request body.

#### Response `data` — `SupportTicketDetailResponse`

| Field                     | Type             | Description                                      |
| ------------------------- | ---------------- | ------------------------------------------------ |
| `ticketId`                | string           | Primary id                                       |
| `ticketNumber`            | string           | Display number                                   |
| `subject`                 | string           | Title                                            |
| `description`             | string           | Full description                                 |
| `customerId`              | string           | Customer id                                      |
| `customerName`            | string           | Name                                             |
| `branchId`                | string           | Branch id                                        |
| `branchName`              | string           | Branch name                                      |
| `salesOrderId`            | string \| null   | Linked SO id                                     |
| `soNumber`                | string \| null   | SO display number                                |
| `relatedTaskId`           | string \| null   | Optional task selected at raise                  |
| `linkedTaskNumber`        | string \| null   | Latest task from Module 21 linked to this ticket |
| `ticketTypeId`            | string           | Type id                                          |
| `ticketTypeLabel`         | string           | Type label                                       |
| `priority`                | enum             | `SupportTicketPriority`                          |
| `status`                  | enum             | `SupportTicketStatus`                            |
| `reportedByName`          | string           | Reporter                                         |
| `reportedByPhone`         | string           | Phone                                            |
| `expectedResolutionDate`  | date             | Expected resolution date                         |
| `expectedResolutionTime`  | time             | Expected resolution time                         |
| `createdAt`               | ISO-8601 instant | Created timestamp                                |
| `assigneeRoleCode`        | string \| null   | Role code assigned in UI                         |
| `assignedUserId`          | number \| null   | User id (Module 8)                               |
| `assignedUserName`        | string \| null   | Display name                                     |
| `responseSlaDeadlineAt`   | instant          | Response SLA deadline                            |
| `firstResponseAt`         | instant \| null  | When first response happened                     |
| `responseSlaMet`          | boolean \| null  | Whether response SLA was met                     |
| `resolutionExpectedAt`    | instant          | Resolution deadline                              |
| `slaHealth`               | enum             | Current SLA health                               |
| `slaRemainingSeconds`     | number \| null   | Countdown helper                                 |
| `escalationLevel`         | enum             | Escalation                                       |
| `paused`                  | boolean          | True if status is paused                         |
| `resolutionCode`          | enum \| null     | Set after resolve                                |
| `resolutionNotes`         | string \| null   | Resolve notes                                    |
| `resolveCustomerRating`   | number \| null   | 1–5                                              |
| `resolveCustomerFeedback` | string \| null   | Text                                             |
| `resolvedAt`              | instant \| null  | When resolved                                    |
| `closeReason`             | enum \| null     | After close                                      |
| `closureRemarks`          | string \| null   | Close remarks                                    |
| `closedAt`                | instant \| null  | When closed                                      |
| `activities`              | array            | Timeline (see below)                             |
| `attachments`             | array            | Files (see below)                                |

**`activities[]` — `SupportTicketActivityResponse`**

| Field              | Type           | Description                                                                                            |
| ------------------ | -------------- | ------------------------------------------------------------------------------------------------------ |
| `id`               | number         | Activity id                                                                                            |
| `activityType`     | string         | e.g. `CREATED`, `ASSIGNED`, `NOTE`, `REPLY`, `PAUSE`, `RESUME`, `RESOLVED`, `CLOSED`, `SLA_ESCALATION` |
| `summary`          | string         | Short line                                                                                             |
| `detail`           | string \| null | Extra text                                                                                             |
| `internal`         | boolean        | Internal note                                                                                          |
| `performedByLabel` | string         | Who performed                                                                                          |
| `performedAt`      | instant        | When                                                                                                   |

**`attachments[]` — `SupportTicketAttachmentResponse`**

| Field          | Type           | Description               |
| -------------- | -------------- | ------------------------- |
| `id`           | string         | Attachment id             |
| `phase`        | enum           | `SupportAttachmentPhase`  |
| `filePath`     | string         | Path for download/preview |
| `originalName` | string \| null | Original name             |
| `uploadedAt`   | instant        | When uploaded             |

#### Not found

`404` if `ticketRef` does not match any ticket.

#### Dependencies

- **Print/PDF:** not implemented as API; build client-side or add a future export endpoint.
- **Assignee user list:** Module 8 — `GET /api/v1/users` (or your filtered list) for `assignToUserId`.

---

### Screen 23.4 — Assign / Reassign (modal)

#### API

| Method | Endpoint                                     | Permission |
| ------ | -------------------------------------------- | ---------- |
| `POST` | `/api/v1/support/tickets/{ticketRef}/assign` | EDIT       |

`{ticketRef}` = same as ticket id or number (URL-encoded).

#### Request body — `AssignSupportTicketRequest`

| Field              | Required | Type   | Description                                                                             |
| ------------------ | -------- | ------ | --------------------------------------------------------------------------------------- |
| `assigneeRoleCode` | Yes      | string | Free text code (e.g. `SUPPORT_AGENT`, `OPERATIONS_MANAGER`) — align with your UI labels |
| `assignToUserId`   | Yes      | number | **Tenant user id** (`users.id`)                                                         |
| `assignmentNote`   | No       | string | Handover note                                                                           |

#### Response

`SupportTicketDetailResponse` (updated ticket).

#### Validations

| Case                   | Message                              |
| ---------------------- | ------------------------------------ |
| User id invalid        | `Assignee user not found`            |
| Ticket closed/resolved | `Ticket is read-only in this status` |

---

### Screen 23.5 — Convert to Task (Module 21)

**There is no separate “convert” endpoint in Module 23.** The flow is:

1. Open **Module 21 — Add Re-Task / Task** screen with **source = customer ticket**.
2. Call **`POST /api/v1/tasks`** with:
   - `sourceType`: `CUSTOMER_TICKET`
   - `ticketId`: current ticket id **or** number (same as `ticketRef`)
   - Other fields per Module 21 (customer, site, schedule, technicians, etc.)

After a task is created, the backend may record a link in `support_ticket_tasks` when the ticket exists.

**Dependency:** Module 21 Task API — `/api/v1/tasks`.

---

### Screen 23.6 — Resolve Ticket (modal)

#### API

| Method | Endpoint                                      | Permission |
| ------ | --------------------------------------------- | ---------- |
| `POST` | `/api/v1/support/tickets/{ticketRef}/resolve` | EDIT       |

#### Request body — `ResolveSupportTicketRequest`

| Field              | Required | Type     | Rules                                          |
| ------------------ | -------- | -------- | ---------------------------------------------- |
| `resolutionCode`   | Yes      | enum     | `SupportResolutionCode`                        |
| `resolutionNotes`  | Yes      | string   | max 4000                                       |
| `customerRating`   | Yes      | number   | integer **1–5**                                |
| `customerFeedback` | Yes      | string   | max 2000                                       |
| `attachmentPaths`  | No       | string[] | max **5** paths total, each path max 500 chars |

#### Response

`SupportTicketDetailResponse` with `status` = `RESOLVED`.

#### Validations

| Case                           | Message                                                             |
| ------------------------------ | ------------------------------------------------------------------- |
| Already resolved/closed        | `Ticket is already closed or resolved`                              |
| Linked tasks not all completed | `Cannot resolve ticket: one or more linked tasks are not completed` |
| Too many attachments           | `At most 5 attachments are allowed`                                 |

**Important:** If any **task** linked to this ticket (`tasks.ticket_id` = this ticket) has status **not** `COMPLETED`, resolve **fails**. Frontend should check task status or show the error message.

---

### Screen 23.7 — Close Ticket (modal)

#### API

| Method | Endpoint                                    | Permission |
| ------ | ------------------------------------------- | ---------- |
| `POST` | `/api/v1/support/tickets/{ticketRef}/close` | EDIT       |

#### Request body — `CloseSupportTicketRequest`

| Field             | Required | Type     | Rules                           |
| ----------------- | -------- | -------- | ------------------------------- |
| `closeReason`     | Yes      | enum     | `SupportCloseReason`            |
| `closureRemarks`  | Yes      | string   | max 4000                        |
| `attachmentPaths` | No       | string[] | max 5 paths, each max 500 chars |

#### Response

`SupportTicketDetailResponse` with `status` = `CLOSED`.

#### Validations

| Case                     | Message                               |
| ------------------------ | ------------------------------------- |
| Status is not `RESOLVED` | `Only resolved tickets can be closed` |

---

## 5. Other actions (same detail screen)

### 5.1 Add note / reply

| Method | Endpoint                                    |
| ------ | ------------------------------------------- |
| `POST` | `/api/v1/support/tickets/{ticketRef}/notes` |

**Body — `AddSupportTicketNoteRequest`**

| Field                  | Required | Description                                         |
| ---------------------- | -------- | --------------------------------------------------- |
| `note`                 | Yes      | max 4000                                            |
| `internal`             | No       | boolean — internal note                             |
| `customerVisibleReply` | No       | boolean — treat as customer-visible reply when true |

### 5.2 Pause

| Method | Endpoint                                    |
| ------ | ------------------------------------------- |
| `POST` | `/api/v1/support/tickets/{ticketRef}/pause` |

**Valid:** `ASSIGNED` or `IN_PROGRESS`. Errors: `Only assigned or in-progress tickets can be paused`, `Ticket is already paused`.

### 5.3 Resume

| Method | Endpoint                                     |
| ------ | -------------------------------------------- |
| `POST` | `/api/v1/support/tickets/{ticketRef}/resume` |

**Valid:** `PAUSED`. Error: `Ticket is not paused`.

### 5.4 Mark in progress

| Method | Endpoint                                          |
| ------ | ------------------------------------------------- |
| `POST` | `/api/v1/support/tickets/{ticketRef}/in-progress` |

**Valid:** `OPEN` or `ASSIGNED`. Error: `Invalid status transition to in progress`.

---

## 6. Suggested frontend state machine (high level)

```
OPEN → ASSIGNED (assign)
OPEN → IN_PROGRESS (in-progress)
ASSIGNED → IN_PROGRESS (in-progress)
IN_PROGRESS → PAUSED (pause) → IN_PROGRESS (resume)
OPEN / ASSIGNED / IN_PROGRESS → RESOLVED (resolve)
RESOLVED → CLOSED (close)
```

Disable actions when `status` is `RESOLVED` or `CLOSED` unless you only show read-only history.

---

## 7. Quick reference — all Module 23 endpoints

| #   | Method | Path                                              | Body         |
| --- | ------ | ------------------------------------------------- | ------------ |
| 1   | GET    | `/api/v1/support/tickets/types`                   | —            |
| 2   | POST   | `/api/v1/support/tickets`                         | Raise JSON   |
| 3   | GET    | `/api/v1/support/tickets`                         | Query params |
| 4   | GET    | `/api/v1/support/tickets/by-id?ticketRef=`        | —            |
| 5   | POST   | `/api/v1/support/tickets/{ticketRef}/assign`      | Assign JSON  |
| 6   | POST   | `/api/v1/support/tickets/{ticketRef}/notes`       | Note JSON    |
| 7   | POST   | `/api/v1/support/tickets/{ticketRef}/pause`       | —            |
| 8   | POST   | `/api/v1/support/tickets/{ticketRef}/resume`      | —            |
| 9   | POST   | `/api/v1/support/tickets/{ticketRef}/in-progress` | —            |
| 10  | POST   | `/api/v1/support/tickets/{ticketRef}/resolve`     | Resolve JSON |
| 11  | POST   | `/api/v1/support/tickets/{ticketRef}/close`       | Close JSON   |

---

## 8. Known limitations (for planning)

- **Email / notifications** for escalations are not exposed via these APIs.
- **Ticket type filter** on dashboard list may need a follow-up API if product requires it.
- **File upload** is assumed to be **upload first, then send paths**; there is no multipart upload in this module’s controller.
- **Timezone:** `expectedResolutionDate` + `expectedResolutionTime` are combined with the **server default zone** when stored.

---

_Generated from backend: `SupportTicketController`, DTOs, enums, and `SupportTicketServiceImpl`. Update this doc if APIs change._
