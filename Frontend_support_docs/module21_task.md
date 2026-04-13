# Module 21 – Task Management

## Short Description

Module 21 manages **field service tasks** across two personas:

- **Web/Admin/Back-office**: create tasks, batch-create tasks, edit/reschedule/reassign technicians, list tasks with filters, export PDFs, and review audit logs.
- **Mobile Technician (APPLICATION_USER)**: view assigned tasks, navigate to a site, upload pre/post evidence photos, record service execution blocks + chemical usage, submit technician observations, submit customer feedback, and generate a consolidated **service report**.

Frontend developers should be aware of:

- **Auth & tenancy**: all endpoints are JWT-protected and tenant-scoped via `X-Tenant-ID`. Many mobile endpoints additionally require a **tenant user token** (userType `TENANT`) and `APPLICATION_USER` authority.
- **Status-driven behavior**: web edit is allowed only while task is `PENDING`. Mobile completion requires task to be `IN_PROGRESS` and technician must be assigned.
- **Evidence uploads**: mobile photo uploads are **Base64 in JSON**, validated to be images only and max 10MB each, with strict per-task limits (max 5 BEFORE and max 5 AFTER total; SELFIE replaces existing).
- **Integration touchpoints**: task completion increments execution count on Sales Orders (Module 20) when `soSiteServiceId` exists; technician tracking workflow can change task status and timestamps.
- **Response shapes**:
  - Success: `ResponseStructure<T>` wrapper.
  - Errors: `ValidationErrorResponse` from the global exception handler (field-level errors for Bean Validation; business-rule errors as `message`).
  - File exports: raw bytes (`application/pdf`) rather than `ResponseStructure`.

---

## Authorization

### Authentication type

- **JWT Bearer token** required for all endpoints in this module.
- Header: `Authorization: Bearer <access_token>`

### Tenant / schema behavior

- Header: `X-Tenant-ID: <tenant>` used by `TenantResolverFilter` to choose tenant schema.
- If omitted, backend may fall back to a default tenant.
- Several mobile APIs additionally require the token to represent a **tenant user** (`principal.userType == "TENANT"`), otherwise backend returns **403**.

### Roles / authorities / permissions

#### Web/Admin endpoints (`/api/v1/tasks`)

| Endpoint group                         | Authorization                                                                  |
| -------------------------------------- | ------------------------------------------------------------------------------ |
| Create / batch create                  | `TASK_MANAGEMENT_ADD` or `CEO`                                                 |
| Read (list/detail/pdf)                 | `TASK_MANAGEMENT_READ` or `CEO` (some endpoints also allow `APPLICATION_USER`) |
| Edit/reschedule/reassign/status update | `TASK_MANAGEMENT_EDIT` or `CEO` (complete also allows `APPLICATION_USER`)      |
| Delete/cancel                          | `TASK_MANAGEMENT_DELETE` or `CEO`                                              |

#### Mobile technician endpoints (`/api/v1/mobile/**`)

| Endpoint group                                                                                                    | Authorization                                                        |
| ----------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| Task list/detail/dashboard/calendar/navigation, uploads, execution blocks, observations, service report, feedback | `APPLICATION_USER` (and tenant-user token required by service layer) |

**Important:** Mobile controllers check `hasAuthority('APPLICATION_USER')`. The service layer additionally checks `userType == TENANT` and throws **403** `"This operation requires a tenant user token"` when violated.

### Required headers

| Header                           |               Required | Used for                             |
| -------------------------------- | ---------------------: | ------------------------------------ |
| `Authorization`                  |                    Yes | JWT auth                             |
| `X-Tenant-ID`                    |      Yes (recommended) | Tenant schema selection              |
| `Content-Type: application/json` | For JSON body requests | Create/update/complete/submit/upload |
| `Accept: application/json`       |            Recommended | JSON APIs                            |

---

## Enums Used In This Module

### TaskStatus

| Value            | Meaning                                                                                 | Used In                                                     |
| ---------------- | --------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `PENDING`        | Scheduled but not started; **only status that can be edited** from web update endpoint. | List filter, task detail/list responses, status update      |
| `TRAVEL_STARTED` | Travel workflow started (used in calendar and dashboard grouping).                      | Responses, scheduler logic                                  |
| `IN_PROGRESS`    | Technician on the way / on-site; required state for mobile completion.                  | Complete task validation; tracking workflow                 |
| `COMPLETED`      | Final successful completion.                                                            | Responses; final state restrictions                         |
| `OVERDUE`        | Derived/managed by scheduler; treated as active.                                        | Calendar/dashboard summaries                                |
| `CANCELLED`      | Cancelled task.                                                                         | Final state; conflict counting excludes cancelled/completed |

---

### TaskType

| Value     | Meaning                    | Used In                              |
| --------- | -------------------------- | ------------------------------------ |
| `NORMAL`  | Standard task.             | Create/list/detail, calendar filters |
| `RE_TASK` | Re-visit / follow-up task. | Same                                 |

---

### TaskSourceType

| Value             | Meaning                                                                 | Used In                 |
| ----------------- | ----------------------------------------------------------------------- | ----------------------- |
| `SALES_ORDER`     | Task linked to Sales Order context (`salesOrderId`, `soSiteServiceId`). | Create payload + detail |
| `CUSTOMER_TICKET` | Task linked to Support Ticket (`ticketId`).                             | Create + detail         |
| `MANUAL`          | Created manually without upstream link.                                 | Create + detail         |

---

### TaskPriority

| Value      | Meaning           | Used In                    |
| ---------- | ----------------- | -------------------------- |
| `NORMAL`   | Default priority. | Create/list/detail filters |
| `HIGH`     | Elevated.         |                            |
| `URGENT`   | High urgency.     |                            |
| `CRITICAL` | Highest urgency.  |                            |

---

### TaskPhotoType

| Value    | Meaning                                                   | Used In                               |
| -------- | --------------------------------------------------------- | ------------------------------------- |
| `SELFIE` | Pre-task selfie; **upload replaces any previous selfie**. | Mobile selfie upload + service report |
| `BEFORE` | Before-work evidence; max **5 total** per task.           | Mobile before upload + service report |
| `AFTER`  | After-work evidence; max **5 total** per task.            | Mobile after upload + service report  |

---

### InfestationLevel

| Value    | Meaning                    | Used In                  |
| -------- | -------------------------- | ------------------------ |
| `HIGH`   | High infestation severity. | Service execution blocks |
| `MEDIUM` | Medium severity.           |                          |
| `LOW`    | Low severity.              |                          |

---

### TechnicianObservationCategory

| Value                | Meaning                         | Used In                               |
| -------------------- | ------------------------------- | ------------------------------------- |
| `STRUCTURAL_GAPS`    | Structural gaps / entry points. | Observation sections + service report |
| `HYGIENE_SANITATION` | Hygiene/sanitation issues.      |                                       |
| `PEST_SIGHTING`      | Pest sightings.                 |                                       |

---

### TaskTravelSessionStatus (backend entity)

| Value        | Meaning              | Used In                               |
| ------------ | -------------------- | ------------------------------------- |
| `TRAVELLING` | Travel session open. | Travel session entity/repo (internal) |
| `ARRIVED`    | Arrived at site.     |                                       |
| `CANCELLED`  | Cancelled session.   |                                       |

**Frontend notes:** Mobile tracking APIs currently use `TechnicianStatus` (see below) for workflow states; Task travel session enum is internal and not exposed directly in API responses in the provided context.

---

### TechnicianStatus (Mobile tracking)

| Value        | Meaning                                | Used In                                                |
| ------------ | -------------------------------------- | ------------------------------------------------------ |
| `CLOCK_IN`   | Shift start.                           | `/api/v1/mobile/technician-tracking/check-in` response |
| `CLOCK_OUT`  | Shift end.                             | `/check-out`                                           |
| `IDLE`       | Not moving (within idle proximity).    | Periodic pings                                         |
| `TRAVELLING` | Moving (outside idle proximity).       | Periodic pings                                         |
| `ON_GOING`   | Traveling to a specific task.          | `/task/start-travel`                                   |
| `ARRIVED`    | Arrived to site.                       | `/task/arrived`                                        |
| `ON_SITE`    | On site, working.                      | `/task/on-site`                                        |
| `DEPARTED`   | Left site.                             | `/task/departed`                                       |
| `OFFLINE`    | Reserved/unused in current controller. | **Missing from backend context**: how/when set         |

---

## API List

### Web/Admin – Task Management

| Method   | Endpoint                                     | Purpose                             | Authorization Required                                |
| -------- | -------------------------------------------- | ----------------------------------- | ----------------------------------------------------- |
| `POST`   | `/api/v1/tasks`                              | Create one task                     | `TASK_MANAGEMENT_ADD` or `CEO`                        |
| `POST`   | `/api/v1/tasks/multiple`                     | Batch create tasks                  | `TASK_MANAGEMENT_ADD` or `CEO`                        |
| `PUT`    | `/api/v1/tasks?taskId=...`                   | Edit task (PENDING only)            | `TASK_MANAGEMENT_EDIT` or `CEO`                       |
| `GET`    | `/api/v1/tasks?taskId=...`                   | Task detail by id                   | `TASK_MANAGEMENT_READ` or `APPLICATION_USER` or `CEO` |
| `GET`    | `/api/v1/tasks`                              | List tasks (filters, pagination)    | `TASK_MANAGEMENT_READ` or `CEO`                       |
| `GET`    | `/api/v1/tasks/export-pdf`                   | Export task PDF                     | `TASK_MANAGEMENT_READ` or `APPLICATION_USER` or `CEO` |
| `GET`    | `/api/v1/tasks/calendar`                     | Calendar summary                    | `TASK_MANAGEMENT_READ` or `CEO`                       |
| `POST`   | `/api/v1/tasks/complete`                     | Complete task (web/admin or mobile) | `TASK_MANAGEMENT_EDIT` or `APPLICATION_USER` or `CEO` |
| `GET`    | `/api/v1/tasks/available-technician`         | Technicians available for a slot    | `TASK_MANAGEMENT_READ` or `CEO`                       |
| `POST`   | `/api/v1/tasks/reschedule`                   | Reschedule task                     | `TASK_MANAGEMENT_EDIT` or `CEO`                       |
| `POST`   | `/api/v1/tasks/reassign`                     | Reassign technicians                | `TASK_MANAGEMENT_EDIT` or `CEO`                       |
| `DELETE` | `/api/v1/tasks?taskId=...&reason=...`        | Cancel task                         | `TASK_MANAGEMENT_DELETE` or `CEO`                     |
| `PATCH`  | `/api/v1/tasks/status?taskId=...&status=...` | Update status only                  | `TASK_MANAGEMENT_EDIT` or `CEO`                       |
| `GET`    | `/api/v1/tasks/{taskId}/audit-logs`          | Audit log trail                     | `TASK_MANAGEMENT_READ` or `CEO`                       |

### Mobile – Assigned technician workflow (Module 21 field ops)

| Method | Endpoint                                          | Purpose                              | Authorization Required |
| ------ | ------------------------------------------------- | ------------------------------------ | ---------------------- |
| `GET`  | `/api/v1/mobile/tasks/dashboard`                  | Dashboard counts for my tasks        | `APPLICATION_USER`     |
| `GET`  | `/api/v1/mobile/tasks/my`                         | List my assigned tasks               | `APPLICATION_USER`     |
| `GET`  | `/api/v1/mobile/tasks/my/completed`               | List my completed tasks              | `APPLICATION_USER`     |
| `GET`  | `/api/v1/mobile/tasks/my/detail`                  | Task detail (assigned only)          | `APPLICATION_USER`     |
| `GET`  | `/api/v1/mobile/tasks/my/detail-screen`           | Sectioned detail for mobile UI       | `APPLICATION_USER`     |
| `GET`  | `/api/v1/mobile/tasks/my/navigation`              | Navigation card + distance           | `APPLICATION_USER`     |
| `GET`  | `/api/v1/mobile/tasks/my/calendar`                | Calendar summary for my tasks        | `APPLICATION_USER`     |
| `POST` | `/api/v1/mobile/tasks/my/photos/selfie`           | Upload selfie (Base64)               | `APPLICATION_USER`     |
| `POST` | `/api/v1/mobile/tasks/my/photos/before`           | Upload before photos batch           | `APPLICATION_USER`     |
| `POST` | `/api/v1/mobile/tasks/my/photos/after`            | Upload after photos batch            | `APPLICATION_USER`     |
| `PUT`  | `/api/v1/mobile/tasks/my/service-executions`      | Replace execution blocks + chemicals | `APPLICATION_USER`     |
| `POST` | `/api/v1/mobile/tasks/my/technician-observations` | Replace all observation sections     | `APPLICATION_USER`     |
| `GET`  | `/api/v1/mobile/tasks/my/service-report`          | Service report for task              | `APPLICATION_USER`     |
| `POST` | `/api/v1/mobile/tasks/my/customer-feedback`       | Submit customer feedback             | `APPLICATION_USER`     |

### Mobile – Observation options (dropdown data)

| Method | Endpoint                                           | Purpose                      | Authorization Required |
| ------ | -------------------------------------------------- | ---------------------------- | ---------------------- |
| `GET`  | `/api/v1/mobile/observation-options/structural`    | Active structural options    | `APPLICATION_USER`     |
| `GET`  | `/api/v1/mobile/observation-options/hygiene`       | Active hygiene options       | `APPLICATION_USER`     |
| `GET`  | `/api/v1/mobile/observation-options/pest-sighting` | Active pest sighting options | `APPLICATION_USER`     |
| `POST` | `/api/v1/mobile/observation-options/structural`    | Create structural option     | `APPLICATION_USER`     |
| `POST` | `/api/v1/mobile/observation-options/hygiene`       | Create hygiene option        | `APPLICATION_USER`     |
| `POST` | `/api/v1/mobile/observation-options/pest-sighting` | Create pest option           | `APPLICATION_USER`     |

### Mobile – Technician tracking (GPS & site workflow)

| Method | Endpoint                                               | Purpose                | Authorization Required |
| ------ | ------------------------------------------------------ | ---------------------- | ---------------------- |
| `POST` | `/api/v1/mobile/technician-tracking/check-in`          | Shift check-in         | `APPLICATION_USER`     |
| `POST` | `/api/v1/mobile/technician-tracking/ping`              | Periodic location ping | `APPLICATION_USER`     |
| `POST` | `/api/v1/mobile/technician-tracking/task/start-travel` | Start travel to a task | `APPLICATION_USER`     |
| `POST` | `/api/v1/mobile/technician-tracking/task/arrived`      | Arrived at site        | `APPLICATION_USER`     |
| `POST` | `/api/v1/mobile/technician-tracking/task/on-site`      | On-site status         | `APPLICATION_USER`     |
| `POST` | `/api/v1/mobile/technician-tracking/task/departed`     | Departed site          | `APPLICATION_USER`     |
| `POST` | `/api/v1/mobile/technician-tracking/check-out`         | Shift check-out        | `APPLICATION_USER`     |

---

## API Details

### Shared response conventions

#### Success wrapper (`ResponseStructure<T>`)

```json
{
  "status": 200,
  "message": "Some message",
  "data": {}
}
```

#### Error wrapper (`ValidationErrorResponse`)

```json
{
  "timestamp": "2026-04-13T10:15:30",
  "status": 400,
  "error": "Bad Request",
  "message": "Input validation failed",
  "path": "/api/v1/tasks",
  "validationErrors": {
    "fieldName": "Validation message"
  }
}
```

---

## Web/Admin APIs (`/api/v1/tasks`)

### `POST /api/v1/tasks`

**Purpose**  
Create a task with schedule, technicians, and customer/site/service snapshot fields. Backend checks technician schedule conflicts in the same time window (excluding cancelled/completed tasks).

**Authorization**

- Token: Required
- Authority: `TASK_MANAGEMENT_ADD` or `CEO`
- Tenant: `X-Tenant-ID`

**Path Parameters**: Not applicable

**Query Parameters**: Not applicable

**Request Body Fields** (`CreateTaskRequest`)

| Field                   | Type    | Required | Validation            | Description                                                                    | Example                      | Allowed Values                             |
| ----------------------- | ------- | -------: | --------------------- | ------------------------------------------------------------------------------ | ---------------------------- | ------------------------------------------ |
| `branchId`              | string  |      Yes | `@NotBlank`           | Branch id                                                                      | `BR-1`                       | —                                          |
| `taskType`              | enum    |      Yes | `@NotNull`            | Task type                                                                      | `NORMAL`                     | `NORMAL`, `RE_TASK`                        |
| `sourceType`            | enum    |      Yes | `@NotNull`            | Task source                                                                    | `SALES_ORDER`                | `SALES_ORDER`, `CUSTOMER_TICKET`, `MANUAL` |
| `salesOrderId`          | string  |       No | —                     | Source SO id                                                                   | `SO-7C1A2B3C`                | —                                          |
| `ticketId`              | string  |       No | —                     | Support ticket id                                                              | `TICKET-001`                 | —                                          |
| `customerId`            | string  |      Yes | `@NotBlank`           | Customer id                                                                    | `CUST-1`                     | —                                          |
| `customerName`          | string  |      Yes | `@NotBlank`           | Customer name snapshot                                                         | `Acme Industries Pvt Ltd`    | —                                          |
| `siteId`                | string  |       No | —                     | Site id snapshot                                                               | `SITE-01`                    | —                                          |
| `siteName`              | string  |      Yes | `@NotBlank`           | Site name                                                                      | `Andheri Plant`              | —                                          |
| `siteAddress`           | string  |       No | —                     | Address                                                                        | `Plot 12, MIDC`              | —                                          |
| `siteContactName`       | string  |       No | —                     | Site contact                                                                   | `Site Admin`                 | —                                          |
| `siteContactMobile`     | string  |       No | —                     | Contact mobile                                                                 | `9876543210`                 | —                                          |
| `scheduledDate`         | date    |      Yes | `@NotNull`            | Schedule date                                                                  | `2026-04-13`                 | `yyyy-MM-dd`                               |
| `startTime`             | time    |      Yes | `@NotNull`            | Start time                                                                     | `09:00:00`                   | ISO time                                   |
| `endTime`               | time    |      Yes | `@NotNull`            | End time                                                                       | `10:00:00`                   | ISO time                                   |
| `estimatedDurationMins` | int     |       No | —                     | Duration estimate                                                              | `60`                         | —                                          |
| `priority`              | enum    |      Yes | `@NotNull`            | Priority                                                                       | `URGENT`                     | `NORMAL`, `HIGH`, `URGENT`, `CRITICAL`     |
| `technicians`           | array   |      Yes | `@NotEmpty`, `@Valid` | Assigned technicians                                                           | —                            | —                                          |
| `materials`             | array   |       No | `@Valid`              | Planned materials list                                                         | —                            | —                                          |
| `soSiteServiceId`       | string  |       No | —                     | Sales order site service id (links to execution count increment on completion) | `SOSS-BBBB2222`              | —                                          |
| `serviceCategory`       | string  |       No | —                     | Service category snapshot                                                      | `Pest Control`               | —                                          |
| `serviceSubcategory`    | string  |       No | —                     |                                                                                | `Termite`                    | —                                          |
| `serviceTypeName`       | string  |       No | —                     |                                                                                | `Integrated Pest Management` | —                                          |
| `areaSqft`              | decimal |       No | —                     |                                                                                | `15000`                      | —                                          |

**Nested: `TechAssignmentRequest`**

| Field          | Type    | Required | Validation  | Description                                   | Example      |
| -------------- | ------- | -------: | ----------- | --------------------------------------------- | ------------ |
| `userId`       | number  |      Yes | `@NotNull`  | Technician user id                            | `101`        |
| `employeeName` | string  |      Yes | `@NotBlank` | Name snapshot                                 | `John Doe`   |
| `roleName`     | string  |       No | —           | Role name snapshot                            | `Technician` |
| `isPrimary`    | boolean |       No | —           | Primary tech flag (JSON field is `isPrimary`) | `true`       |

**Nested: `MaterialRequest`**

| Field         | Type    | Required | Validation              | Description          | Example         |
| ------------- | ------- | -------: | ----------------------- | -------------------- | --------------- |
| `productId`   | string  |      Yes | `@NotBlank`             | Inventory product id | `CHEM-01`       |
| `productName` | string  |      Yes | `@NotBlank`             | Snapshot name        | `Termiticide A` |
| `uom`         | string  |       No | —                       | UOM                  | `LITER`         |
| `hsnCode`     | string  |       No | —                       | HSN                  | `3808`          |
| `stdQty`      | decimal |       No | —                       | Standard qty         | `1.0`           |
| `requiredQty` | decimal |      Yes | `@NotNull`, `@Positive` | Planned qty          | `3.0`           |

#### Full Request JSON Examples

**Minimal Valid Request (MANUAL source, 1 technician)**

```json
{
  "branchId": "BR-1",
  "taskType": "NORMAL",
  "sourceType": "MANUAL",
  "customerId": "CUST-1",
  "customerName": "Acme Industries Pvt Ltd",
  "siteName": "Andheri Plant",
  "scheduledDate": "2026-04-13",
  "startTime": "09:00:00",
  "endTime": "10:00:00",
  "priority": "NORMAL",
  "technicians": [
    {
      "userId": 101,
      "employeeName": "John Doe",
      "roleName": "Technician",
      "isPrimary": true
    }
  ]
}
```

**Complete Valid Request (SALES_ORDER source + materials)**

```json
{
  "branchId": "BR-1",
  "taskType": "NORMAL",
  "sourceType": "SALES_ORDER",
  "salesOrderId": "SO-7C1A2B3C",
  "soSiteServiceId": "SOSS-BBBB2222",
  "customerId": "CUST-A1B2C",
  "customerName": "Acme Industries Pvt Ltd",
  "siteId": "SITE-01",
  "siteName": "Andheri Plant",
  "siteAddress": "Plot 12, MIDC",
  "siteContactName": "Site Admin",
  "siteContactMobile": "9876543210",
  "serviceCategory": "Pest Control",
  "serviceSubcategory": "Termite",
  "serviceTypeName": "Integrated Pest Management",
  "areaSqft": 15000,
  "scheduledDate": "2026-04-13",
  "startTime": "09:00:00",
  "endTime": "10:30:00",
  "estimatedDurationMins": 90,
  "priority": "URGENT",
  "technicians": [
    {
      "userId": 101,
      "employeeName": "John Doe",
      "roleName": "Technician",
      "isPrimary": true
    },
    {
      "userId": 102,
      "employeeName": "Priya Singh",
      "roleName": "Assistant",
      "isPrimary": false
    }
  ],
  "materials": [
    {
      "productId": "CHEM-01",
      "productName": "Termiticide A",
      "uom": "LITER",
      "hsnCode": "3808",
      "stdQty": 1.0,
      "requiredQty": 3.0
    }
  ]
}
```

**Update Request Example**: Not applicable (use `PUT /api/v1/tasks` and `POST /reschedule`, `/reassign`).

**Response**

- Success: `201 Created`
- `data`: `TaskDetailResponse`

#### Full Response JSON Example

```json
{
  "status": 201,
  "message": "Task created successfully",
  "data": {
    "id": "T-1A2B3C4D",
    "branchId": "BR-1",
    "taskNumber": "TSK-BR-1-26-00001",
    "taskType": "NORMAL",
    "sourceType": "SALES_ORDER",
    "salesOrderId": "SO-7C1A2B3C",
    "soSiteServiceId": "SOSS-BBBB2222",
    "ticketId": null,
    "customerId": "CUST-A1B2C",
    "customerName": "Acme Industries Pvt Ltd",
    "siteId": "SITE-01",
    "siteName": "Andheri Plant",
    "siteAddress": "Plot 12, MIDC",
    "siteContactName": "Site Admin",
    "siteContactMobile": "9876543210",
    "siteLatitude": null,
    "siteLongitude": null,
    "serviceCategory": "Pest Control",
    "serviceSubcategory": "Termite",
    "serviceTypeName": "Integrated Pest Management",
    "areaSqft": 15000,
    "scheduledDate": "2026-04-13",
    "startTime": "09:00:00",
    "endTime": "10:30:00",
    "estimatedDurationMins": 90,
    "status": "PENDING",
    "priority": "URGENT",
    "actualStartAt": null,
    "actualEndAt": null,
    "completionNotes": null,
    "customerRating": null,
    "customerFeedback": null,
    "feedbackAt": null,
    "technicians": [
      {
        "id": "tt-1",
        "userId": 101,
        "employeeName": "John Doe",
        "roleName": "Technician",
        "isPrimary": true
      }
    ],
    "materials": [
      {
        "id": "m-1",
        "productId": "CHEM-01",
        "productName": "Termiticide A",
        "uom": "LITER",
        "hsnCode": "3808",
        "stdQty": 1.0,
        "requiredQty": 3.0,
        "usedQty": null
      }
    ],
    "photos": [],
    "createdBy": "admin@tenant.com",
    "createdAt": "2026-04-13T09:00:00Z",
    "updatedBy": null,
    "updatedAt": "2026-04-13T09:00:00Z"
  }
}
```

#### Exceptions / Error Cases

| HTTP Status | Reason              | When It Happens                        | Typical Message                                  | Frontend Handling Note       |
| ----------: | ------------------- | -------------------------------------- | ------------------------------------------------ | ---------------------------- |
|         400 | Bean validation     | Missing required fields                | `Input validation failed`                        | Render `validationErrors`    |
|         409 | Scheduling conflict | Technician has overlapping active task | `Technician <name> has a scheduling conflict...` | Pick another technician/time |
|         401 | Unauthorized        | Missing/invalid JWT                    | Auth error                                       | Re-auth                      |
|         403 | Forbidden           | No authority                           | Access denied                                    | Hide UI actions              |

#### Error Response JSON Examples

**Validation error**

```json
{
  "timestamp": "2026-04-13T10:00:00",
  "status": 400,
  "error": "Bad Request",
  "message": "Input validation failed",
  "path": "/api/v1/tasks",
  "validationErrors": {
    "branchId": "Branch ID is required",
    "technicians": "At least one technician must be assigned"
  }
}
```

**Conflict**

```json
{
  "timestamp": "2026-04-13T10:01:00",
  "status": 409,
  "error": "Conflict",
  "message": "Technician John Doe has a scheduling conflict for 2026-04-13 09:00-10:30",
  "path": "/api/v1/tasks",
  "validationErrors": null
}
```

#### Frontend Notes

- For technician selection UX, call `GET /api/v1/tasks/available-technician` and then create.
- When creating from SO or ticket, keep `sourceType` consistent with which ids you provide.

#### cURL

```bash
curl -sS -X POST "{{baseUrl}}/api/v1/tasks" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -H "Content-Type: application/json" \
  -d @task-create.json
```

---

### `POST /api/v1/tasks/multiple`

**Purpose**  
Batch create tasks by sending an array of `CreateTaskRequest`. Backend applies the same validation per item.

**Authorization**  
Token required; `TASK_MANAGEMENT_ADD` or `CEO`.

**Request Body Fields**  
Array of `CreateTaskRequest` objects (same schema as single create).

**Full Request JSON Examples**

**Minimal batch**

```json
[
  {
    "branchId": "BR-1",
    "taskType": "NORMAL",
    "sourceType": "MANUAL",
    "customerId": "CUST-1",
    "customerName": "Acme",
    "siteName": "Site A",
    "scheduledDate": "2026-04-13",
    "startTime": "09:00:00",
    "endTime": "10:00:00",
    "priority": "NORMAL",
    "technicians": [
      { "userId": 101, "employeeName": "John Doe", "isPrimary": true }
    ]
  },
  {
    "branchId": "BR-1",
    "taskType": "RE_TASK",
    "sourceType": "MANUAL",
    "customerId": "CUST-2",
    "customerName": "Zenith",
    "siteName": "Site B",
    "scheduledDate": "2026-04-13",
    "startTime": "11:00:00",
    "endTime": "12:00:00",
    "priority": "HIGH",
    "technicians": [
      { "userId": 102, "employeeName": "Priya Singh", "isPrimary": true }
    ]
  }
]
```

**Response**

- Success: `201 Created`
- `data`: `TaskDetailResponse[]`

**Error cases**: Same as single create; any failing item can fail the entire request (no partial success behavior indicated). **Missing from backend context**: explicit transactional semantics for batch.

---

### `PUT /api/v1/tasks?taskId=...`

**Purpose**  
Edit a task **only when status is `PENDING`**. The request body reuses `CreateTaskRequest`, but backend only updates schedule/contact/priority and technicians; source snapshot fields remain unchanged.

**Authorization**  
Token required; `TASK_MANAGEMENT_EDIT` or `CEO`.

**Query Parameters**

| Field    | Type   | Required | Description | Example      |
| -------- | ------ | -------: | ----------- | ------------ |
| `taskId` | string |      Yes | Task id     | `T-1A2B3C4D` |

**Request Body Fields**  
Same as `CreateTaskRequest`.

#### Full Request JSON Examples

**Update schedule + technicians**

```json
{
  "branchId": "BR-1",
  "taskType": "NORMAL",
  "sourceType": "SALES_ORDER",
  "customerId": "CUST-A1B2C",
  "customerName": "Acme Industries Pvt Ltd",
  "siteName": "Andheri Plant",
  "scheduledDate": "2026-04-14",
  "startTime": "10:00:00",
  "endTime": "11:30:00",
  "priority": "HIGH",
  "technicians": [
    {
      "userId": 103,
      "employeeName": "Rahul Tech",
      "roleName": "Technician",
      "isPrimary": true
    }
  ]
}
```

**Response**

- `200 OK` — `TaskDetailResponse`

#### Exceptions / Error Cases

| HTTP Status | Reason              | When             | Typical Message                     | Frontend note                 |
| ----------: | ------------------- | ---------------- | ----------------------------------- | ----------------------------- |
|         400 | Status restriction  | Task not PENDING | `Only pending tasks can be updated` | Disable edit when not PENDING |
|         404 | Not found           | Bad taskId       | `Task not found`                    | Show not found                |
|         409 | Scheduling conflict | Overlap          | conflict message                    | Choose slot                   |

---

### `GET /api/v1/tasks?taskId=...`

**Purpose**  
Fetch full task detail. If caller is `APPLICATION_USER`, backend restricts to tasks assigned to the current technician (otherwise returns not found).

**Authorization**  
Token required; `TASK_MANAGEMENT_READ` or `APPLICATION_USER` or `CEO`.

**Query Parameters**

| Field    | Type   | Required | Example      |
| -------- | ------ | -------: | ------------ |
| `taskId` | string |      Yes | `T-1A2B3C4D` |

**Request Body**: Not applicable

**Response**  
`200 OK` — `TaskDetailResponse`

**Full Response JSON Examples**

_(Same shape as create response; includes `photos` filtered to not-deleted and sorted.)_

---

### `GET /api/v1/tasks`

**Purpose**  
List tasks with filters: date, status, search (min 3 chars), time window, serviceId (`soSiteServiceId`), and task type. Returns `PaginationResponse<TaskListResponse>`.

**Authorization**  
Token required; `TASK_MANAGEMENT_READ` or `CEO`.

**Query Parameters**

| Field       | Type   | Required | Description                                                               | Example         | Allowed Values |
| ----------- | ------ | -------: | ------------------------------------------------------------------------- | --------------- | -------------- |
| `date`      | date   |       No | Filter by scheduled date                                                  | `2026-04-13`    | `yyyy-MM-dd`   |
| `status`    | enum   |       No | Filter by status                                                          | `PENDING`       | TaskStatus     |
| `search`    | string |       No | Matches taskNumber/customer/site/technician; applied only when length ≥ 3 | `acm`           | —              |
| `startTime` | time   |       No | Start time >= filter                                                      | `09:00:00`      | ISO time       |
| `endTime`   | time   |       No | End time <= filter                                                        | `18:00:00`      | ISO time       |
| `serviceId` | string |       No | Equals `soSiteServiceId`                                                  | `SOSS-BBBB2222` | —              |
| `taskType`  | enum   |       No | Task type                                                                 | `RE_TASK`       | TaskType       |
| `pageNo`    | int    |       No | Default 0                                                                 | `0`             | ≥0             |
| `pageSize`  | int    |       No | Default 10                                                                | `10`            | ≥1             |

**Request Body**: Not applicable

**Response**

`200 OK` — `PaginationResponse<TaskListResponse>`

#### Full Response JSON Examples

**Paginated list**

```json
{
  "status": 200,
  "message": "Tasks fetched successfully",
  "data": {
    "count": 1,
    "next": null,
    "prev": null,
    "data": [
      {
        "id": "T-1A2B3C4D",
        "taskId": "TSK-BR-1-26-00001",
        "timeSlot": "09:00 - 10:30",
        "customerName": "Acme Industries Pvt Ltd",
        "serviceName": "Integrated Pest Management",
        "siteName": "Andheri Plant",
        "primaryTechnicianName": "John Doe",
        "supportTechnicians": ["Priya Singh"],
        "serviceType": "NORMAL",
        "serviceStatus": "PENDING"
      }
    ]
  }
}
```

**Empty state**

```json
{
  "status": 200,
  "message": "Tasks fetched successfully",
  "data": {
    "count": 0,
    "next": null,
    "prev": null,
    "data": []
  }
}
```

---

### `GET /api/v1/tasks/export-pdf`

**Purpose**  
Download a printable task detail PDF (same information as task detail).

**Authorization**  
Token required; `TASK_MANAGEMENT_READ` or `APPLICATION_USER` or `CEO`.

**Query Parameters**

| Field    | Type   | Required | Example      |
| -------- | ------ | -------: | ------------ |
| `taskId` | string |      Yes | `T-1A2B3C4D` |

**Request Body**: Not applicable

**Response**

- `200 OK`
- Content-Type: `application/pdf`
- Content-Disposition: `attachment; filename="<taskNumber>.pdf"`
- Body: **raw PDF bytes**

**cURL**

```bash
curl -sS -G "{{baseUrl}}/api/v1/tasks/export-pdf" \
  --data-urlencode "taskId=T-1A2B3C4D" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: {{tenantId}}" \
  -o task.pdf
```

---

### `GET /api/v1/tasks/calendar`

**Purpose**  
Calendar dashboard summary for a month with breakdown by status, averages, and overdue alerts. Defaults month/year to current when omitted.

**Authorization**  
Token required; `TASK_MANAGEMENT_READ` or `CEO`.

**Query Parameters**

| Field            | Type   | Required | Description            | Example   | Allowed Values                         |
| ---------------- | ------ | -------: | ---------------------- | --------- | -------------------------------------- |
| `branchId`       | string |       No | Branch filter          | `BR-1`    | —                                      |
| `month`          | int    |       No | 1-12                   | `4`       | **Assumption:** no explicit validation |
| `year`           | int    |       No | Year                   | `2026`    | —                                      |
| `technicianName` | string |       No | Technician name filter | `John`    | —                                      |
| `types`          | array  |       No | Task types             | `NORMAL`  | TaskType                               |
| `statuses`       | array  |       No | Status filter          | `PENDING` | TaskStatus                             |
| `search`         | string |       No | Search term            | `acm`     | —                                      |

**Request Body**: Not applicable

**Response**

`200 OK` — `TaskCalendarSummaryResponse`

**Full Response JSON Example**

```json
{
  "status": 200,
  "message": "Calendar summary fetched",
  "data": {
    "totalTasksByDate": { "2026-04-13": 5 },
    "statusBreakdownByDate": { "2026-04-13": { "PENDING": 2, "COMPLETED": 3 } },
    "totalTasks": 120,
    "completedTasks": 90,
    "inProgressTasks": 10,
    "pendingTasks": 15,
    "overdueTasks": 5,
    "reTasks": 2,
    "avgTasksPerDay": 4.0,
    "overdueAlerts": [
      { "date": "2026-04-10", "count": 2, "taskIds": ["T-AAAA", "T-BBBB"] }
    ]
  }
}
```

---

### `POST /api/v1/tasks/complete`

**Purpose**  
Mark a task as completed and persist completion details. Also:

- Verifies customer sign-off OTP if the task has an OTP row (OTP request API is **Missing from backend context**).
- Syncs service execution chemical aggregates into task materials.
- Records stock consumption for used quantities.
- Saves photo references if provided.
- If `soSiteServiceId` is present, increments Sales Order execution count (Module 20) by 1.

**Authorization**

- Token: Required
- Authority: `TASK_MANAGEMENT_EDIT` or `APPLICATION_USER` or `CEO`
- Tenant restriction: when called as `APPLICATION_USER`, must be assigned to the task and task must be `IN_PROGRESS`.

**Query Parameters**

| Field    | Type   | Required | Example      |
| -------- | ------ | -------: | ------------ |
| `taskId` | string |      Yes | `T-1A2B3C4D` |

**Request Body Fields** (`TaskCompletionRequest`)

| Field                | Type       |    Required | Validation | Description                                        | Example                         | Allowed Values |
| -------------------- | ---------- | ----------: | ---------- | -------------------------------------------------- | ------------------------------- | -------------- |
| `actualStartAt`      | datetime   |         Yes | `@NotNull` | Actual start timestamp                             | `2026-04-13T09:05:00+05:30`     | ISO-8601       |
| `actualEndAt`        | datetime   |         Yes | `@NotNull` | Actual end timestamp                               | `2026-04-13T10:20:00+05:30`     | ISO-8601       |
| `completionNotes`    | string     |          No | —          | Notes                                              | `Completed with minor findings` | —              |
| `materialsUsed`      | object map |          No | —          | Map productId → usedQty                            | `{ "CHEM-01": 2.5 }`            | —              |
| `photos`             | array      |          No | —          | Photo references (not Base64)                      | —                               | —              |
| `customerRating`     | int        |          No | —          | Rating snapshot                                    | `5`                             | —              |
| `customerFeedback`   | string     |          No | —          | Feedback snapshot                                  | `Good service`                  | —              |
| `customerSignoffOtp` | string     | Conditional | —          | Required only when OTP was requested for this task | `123456`                        | —              |

**Nested: `PhotoUploadRequest`** (same as earlier)

#### Full Request JSON Examples

**Minimal completion**

```json
{
  "actualStartAt": "2026-04-13T09:05:00+05:30",
  "actualEndAt": "2026-04-13T10:20:00+05:30"
}
```

**Complete completion (materials + feedback + photos)**

```json
{
  "actualStartAt": "2026-04-13T09:05:00+05:30",
  "actualEndAt": "2026-04-13T10:20:00+05:30",
  "completionNotes": "Performed spraying and sealed entry points.",
  "materialsUsed": {
    "CHEM-01": 2.5,
    "CHEM-02": 1.0
  },
  "photos": [
    {
      "photoType": "AFTER",
      "filePath": "tasks/tenant/T-1A2B3C4D/after/uuid.jpg",
      "mimeType": "image/jpeg",
      "sortOrder": 0,
      "latitude": 19.076,
      "longitude": 72.8777
    }
  ],
  "customerRating": 5,
  "customerFeedback": "Very professional service."
}
```

**OTP completion (when OTP exists)** _(Assumption: OTP requested earlier)_

```json
{
  "actualStartAt": "2026-04-13T09:05:00+05:30",
  "actualEndAt": "2026-04-13T10:20:00+05:30",
  "customerSignoffOtp": "123456"
}
```

**Response**

`200 OK` — `TaskDetailResponse` (status becomes `COMPLETED`).

#### Exceptions / Error Cases

| HTTP Status | Reason                | When It Happens                                  | Typical Message                       | Frontend Handling Note                 |
| ----------: | --------------------- | ------------------------------------------------ | ------------------------------------- | -------------------------------------- |
|         400 | Final state           | Task already COMPLETED/CANCELLED                 | `Task is already in final state: ...` | Disable complete button                |
|         400 | Mobile restriction    | APPLICATION_USER completing when not IN_PROGRESS | `Task must be in progress...`         | Require arrival/start travel first     |
|         400 | OTP expired           | OTP row exists but expired                       | `Customer sign-off OTP expired...`    | Prompt “Request new OTP” (API missing) |
|         400 | OTP attempts exceeded | Too many attempts                                | `Too many invalid OTP attempts...`    | Prompt re-request                      |
|         400 | OTP invalid           | Wrong code                                       | `Invalid customer sign-off OTP`       | Show error                             |
|         403 | Not assigned          | APPLICATION_USER not on task                     | `You are not assigned to this task`   | Hide task                              |
|         404 | Not found             | Bad taskId                                       | `Task not found`                      | Not found                              |

---

### `GET /api/v1/tasks/available-technician`

**Purpose**  
Returns technicians available for the given slot. If manager user has branch assignments, results are limited to those branches; otherwise returns all active app-technicians.

**Authorization**  
Token required; `TASK_MANAGEMENT_READ` or `CEO`.

**Query Parameters**

| Field       | Type | Required | Example      |
| ----------- | ---- | -------: | ------------ |
| `date`      | date |      Yes | `2026-04-13` |
| `startTime` | time |      Yes | `09:00:00`   |
| `endTime`   | time |      Yes | `10:00:00`   |

**Request Body**: Not applicable

**Response**

`200 OK` — `TechnicianAvailabilityResponse[]`

**Full Response JSON Example**

```json
{
  "status": 200,
  "message": "Available technicians fetched successfully",
  "data": [
    { "userId": 101, "employeeName": "John Doe", "roleName": "Technician" }
  ]
}
```

---

### `POST /api/v1/tasks/reschedule`

**Purpose**  
Reschedules a task to a new date/time and checks conflicts for the currently assigned technicians.

**Authorization**  
Token required; `TASK_MANAGEMENT_EDIT` or `CEO`.

**Query Parameters**

| Field    | Type   | Required |
| -------- | ------ | -------: |
| `taskId` | string |      Yes |

**Request Body Fields** (`TaskRescheduleRequest`)

| Field           | Type   | Required | Validation | Example                     |
| --------------- | ------ | -------: | ---------- | --------------------------- |
| `scheduledDate` | date   |      Yes | `@NotNull` | `2026-04-14`                |
| `startTime`     | time   |      Yes | `@NotNull` | `10:00:00`                  |
| `endTime`       | time   |      Yes | `@NotNull` | `11:00:00`                  |
| `reason`        | string |       No | —          | `Customer requested change` |

**Full Request JSON Examples**

```json
{
  "scheduledDate": "2026-04-14",
  "startTime": "10:00:00",
  "endTime": "11:30:00",
  "reason": "Customer requested change"
}
```

**Response**  
`200 OK` — `TaskDetailResponse`

---

### `POST /api/v1/tasks/reassign`

**Purpose**  
Replace technician assignments for a task, with conflict checking.

**Authorization**  
Token required; `TASK_MANAGEMENT_EDIT` or `CEO`.

**Query Parameters**

| Field    | Type   | Required |
| -------- | ------ | -------: |
| `taskId` | string |      Yes |

**Request Body Fields**  
Array of `TechAssignmentRequest`.

**Full Request JSON Example**

```json
[
  {
    "userId": 101,
    "employeeName": "John Doe",
    "roleName": "Technician",
    "isPrimary": true
  },
  {
    "userId": 102,
    "employeeName": "Priya Singh",
    "roleName": "Assistant",
    "isPrimary": false
  }
]
```

**Response**  
`200 OK` — `TaskDetailResponse`

---

### `DELETE /api/v1/tasks?taskId=...&reason=...`

**Purpose**  
Cancels a task (sets status `CANCELLED`) and records reason into completionNotes.

**Authorization**  
Token required; `TASK_MANAGEMENT_DELETE` or `CEO`.

**Query Parameters**

| Field    | Type   | Required |
| -------- | ------ | -------: |
| `taskId` | string |      Yes |
| `reason` | string |      Yes |

**Request Body**: Not applicable

**Response**  
`200 OK` — `ResponseStructure<Void>` with `data: null`

---

### `PATCH /api/v1/tasks/status?taskId=...&status=...`

**Purpose**  
Status-only update.

**Authorization**  
Token required; `TASK_MANAGEMENT_EDIT` or `CEO`.

**Query Parameters**

| Field    | Type   | Required | Allowed Values |
| -------- | ------ | -------: | -------------- |
| `taskId` | string |      Yes | —              |
| `status` | enum   |      Yes | TaskStatus     |

**Request Body**: Not applicable

**Response**  
`200 OK` — `ResponseStructure<Void>` with `data: null`

---

### `GET /api/v1/tasks/{taskId}/audit-logs`

**Purpose**  
Returns audit log trail, sorted by performedAt desc.

**Authorization**  
Token required; `TASK_MANAGEMENT_READ` or `CEO`.

**Path Parameters**

| Field    | Type   | Required | Example      |
| -------- | ------ | -------: | ------------ |
| `taskId` | string |      Yes | `T-1A2B3C4D` |

**Response**  
`200 OK` — `TaskAuditLogResponse[]`

---

## Mobile technician APIs (`/api/v1/mobile/tasks`)

### `GET /api/v1/mobile/tasks/dashboard`

**Purpose**  
Dashboard counts for a date (defaults to today). Groups `PENDING` + `TRAVEL_STARTED` together as “pendingTravel”.

**Authorization**  
Token required; `APPLICATION_USER` and tenant-user token.

**Query Parameters**

| Field  | Type | Required | Example      |
| ------ | ---- | -------: | ------------ |
| `date` | date |       No | `2026-04-13` |

**Request Body**: Not applicable

**Response**  
`200 OK` — `MobileTaskDashboardResponse` (**Missing from backend context**: field list not documented here; provided by mobile module DTO).

---

### `GET /api/v1/mobile/tasks/my`

**Purpose**  
List tasks assigned to the current technician with optional filters.

**Authorization**  
`APPLICATION_USER` + tenant-user token.

**Query Parameters**

| Field             | Type   | Required | Description                  | Example        | Allowed Values            |
| ----------------- | ------ | -------: | ---------------------------- | -------------- | ------------------------- |
| `scheduledFrom`   | date   |       No | From date inclusive          | `2026-04-01`   | yyyy-MM-dd                |
| `scheduledTo`     | date   |       No | To date inclusive            | `2026-04-30`   | yyyy-MM-dd                |
| `statuses`        | array  |       No | Filter statuses              | `IN_PROGRESS`  | TaskStatus (repeat param) |
| `search`          | string |       No | Applied only when length ≥ 3 | `and`          | —                         |
| `taskPriority`    | enum   |       No | Priority filter              | `URGENT`       | TaskPriority              |
| `serviceCategory` | string |       No | Category filter              | `Pest Control` | —                         |
| `pageNo`          | int    |       No | Default 0                    | `0`            | —                         |
| `pageSize`        | int    |       No | Default 20                   | `20`           | —                         |

**Response**  
`200 OK` — `PaginationResponse<TaskListResponse>`

---

### `GET /api/v1/mobile/tasks/my/completed`

**Purpose**  
Same as `/my` but status is forced to `COMPLETED` by controller.

**Authorization**  
`APPLICATION_USER` + tenant-user token.

---

### `GET /api/v1/mobile/tasks/my/detail`

**Purpose**  
Task detail for assigned tasks only.

**Authorization**  
`APPLICATION_USER` + tenant-user token.

**Query Parameters**

| Field    | Type   | Required |
| -------- | ------ | -------: |
| `taskId` | string |      Yes |

**Response**  
`200 OK` — `TaskDetailResponse`

---

### `GET /api/v1/mobile/tasks/my/detail-screen`

**Purpose**  
Returns `MobileTaskDetailsScreenResponse`, a sectioned payload designed for the mobile UI.

**Authorization**  
`APPLICATION_USER` + tenant-user token.

**Query Parameters**: `taskId` (required)

**Response**  
`200 OK` — `MobileTaskDetailsScreenResponse` (**Missing from backend context**: DTO field list is in mobile module).

---

### `GET /api/v1/mobile/tasks/my/navigation`

**Purpose**  
Navigation card + straight-line distance from current coordinates to task site coordinates.

**Authorization**  
`APPLICATION_USER` + tenant-user token.

**Query Parameters**

| Field       | Type    | Required | Example      |
| ----------- | ------- | -------: | ------------ |
| `taskId`    | string  |      Yes | `T-1A2B3C4D` |
| `latitude`  | decimal |      Yes | `19.0760`    |
| `longitude` | decimal |      Yes | `72.8777`    |

**Exceptions**

- 400 if lat/lng missing or out of range.
- 404 if task not assigned/not found.

---

### `POST /api/v1/mobile/tasks/my/photos/selfie`

**Purpose**  
Upload a single **SELFIE** image (Base64). Replaces any previous selfie for the task.

**Authorization**  
`APPLICATION_USER` + tenant-user token; task must be assigned to current user.

**Query Parameters**: `taskId` (required)

**Request Body Fields** (`TaskSelfieUploadRequest`)

| Field  | Type   | Required | Validation           | Description     |
| ------ | ------ | -------: | -------------------- | --------------- |
| `file` | object |      Yes | `@NotNull`, `@Valid` | One Base64 file |

**Nested: `TaskPhotoUploadFileRequest`**

| Field      | Type   | Required | Validation  | Example                      |
| ---------- | ------ | -------: | ----------- | ---------------------------- |
| `fileName` | string |       No | max 255     | `selfie.jpg`                 |
| `fileType` | string |      Yes | `@NotBlank` | `image/jpeg`                 |
| `fileData` | string |      Yes | `@NotBlank` | `data:image/jpeg;base64,...` |

**Rules**

- Allowed image MIME types enforced server-side.
- Max size: **10MB**.

**Full Request JSON Example**

```json
{
  "file": {
    "fileName": "selfie.jpg",
    "fileType": "image/jpeg",
    "fileData": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD..."
  }
}
```

**Response**

`201 Created` — `PhotoResponse`

---

### `POST /api/v1/mobile/tasks/my/photos/before` and `POST /api/v1/mobile/tasks/my/photos/after`

**Purpose**  
Upload 1–5 photos per request; max **5 total** before photos and **5 total** after photos per task.

**Authorization**  
`APPLICATION_USER` + tenant-user token; task assigned.

**Query Parameters**: `taskId` required

**Request Body Fields** (`TaskPhotoBatchUploadRequest`)

| Field   | Type  | Required | Validation           | Description      |
| ------- | ----- | -------: | -------------------- | ---------------- |
| `files` | array |      Yes | `@Size(min=1,max=5)` | Base64 file list |

**Error cases**

- 400 if total would exceed 5: message includes existing count.
- 400 if any file >10MB or invalid MIME.

---

### `PUT /api/v1/mobile/tasks/my/service-executions`

**Purpose**  
Replace all service execution blocks and chemical usage for the task. Also aggregates chemical usage into task materials.

**Authorization**  
`APPLICATION_USER` + tenant-user token; task assigned.

**Query Parameters**: `taskId` required

**Request Body Fields** (`ServiceExecutionReplaceRequest`)

| Field    | Type  | Required | Validation            |
| -------- | ----- | -------: | --------------------- |
| `blocks` | array |      Yes | `@NotEmpty`, `@Valid` |

**Block: `ServiceExecutionBlockRequest`**

| Field              | Type     | Required | Validation           | Notes                                         |
| ------------------ | -------- | -------: | -------------------- | --------------------------------------------- |
| `serviceId`        | string   |       No | max 50               | If provided, must exist in service master     |
| `serviceName`      | string   |      Yes | `@NotBlank`, max 200 | Snapshot name                                 |
| `infestationLevel` | enum     |      Yes | `@NotNull`           | InfestationLevel                              |
| `locationArea`     | string   |      Yes | `@NotBlank`          |                                               |
| `trapCodes`        | string   |       No | —                    |                                               |
| `treatmentIds`     | string[] |       No | —                    | Each id must exist (service treatment master) |
| `chemicals`        | array    |       No | `@Valid`             | Chemical usage lines                          |
| `sortOrder`        | int      |       No | —                    | Defaults to block index                       |

**Chemical line: `ServiceExecutionChemicalLineRequest`**

| Field                | Type    | Required | Validation          | Notes                                            |
| -------------------- | ------- | -------: | ------------------- | ------------------------------------------------ |
| `inventoryProductId` | string  |      Yes | `@NotBlank`, max 50 | Must exist                                       |
| `serviceProductId`   | string  |       No | max 50              | If provided, must match inventoryProduct         |
| `requiredDilution`   | decimal |       No | —                   | If absent, derived from service product dilution |
| `usedDilution`       | decimal |      Yes | `@NotNull`, `>= 0`  |                                                  |
| `requiredQty`        | decimal |      Yes | `@NotNull`, `>= 0`  |                                                  |
| `usedQty`            | decimal |      Yes | `@NotNull`, `>= 0`  |                                                  |
| `sortOrder`          | int     |       No | —                   |                                                  |

**Full Request JSON Example**

```json
{
  "blocks": [
    {
      "serviceId": "SVC-01",
      "serviceName": "IPM Contract",
      "infestationLevel": "MEDIUM",
      "locationArea": "Kitchen",
      "trapCodes": "TR-01,TR-02",
      "treatmentIds": ["TRT-01", "TRT-02"],
      "chemicals": [
        {
          "inventoryProductId": "CHEM-01",
          "serviceProductId": "SP-01",
          "usedDilution": 10,
          "requiredQty": 1.0,
          "usedQty": 0.8,
          "sortOrder": 0
        }
      ],
      "sortOrder": 0
    }
  ]
}
```

**Response**  
`200 OK` — `ServiceExecutionBlockResponse[]`

---

### `POST /api/v1/mobile/tasks/my/technician-observations`

**Purpose**  
Replace all observation categories for this task. Rejects duplicate categories in payload.

**Authorization**  
`APPLICATION_USER` + tenant-user token; task assigned.

**Query Parameters**: `taskId` required

**Request Body Fields** (`TechnicianObservationSaveRequest`)

| Field      | Type  | Required | Validation            |
| ---------- | ----- | -------: | --------------------- |
| `sections` | array |      Yes | `@NotEmpty`, `@Valid` |

**Section: `TechnicianObservationSectionPayload`**

| Field          | Type     | Required | Validation  | Notes                                              |
| -------------- | -------- | -------: | ----------- | -------------------------------------------------- |
| `category`     | enum     |      Yes | `@NotNull`  | TechnicianObservationCategory                      |
| `found`        | boolean  |      Yes | `@NotNull`  |                                                    |
| `optionIds`    | string[] |       No | —           | Used when found=true; inactive options are ignored |
| `otherNotes`   | string   |       No | —           |                                                    |
| `locationArea` | string   |      Yes | `@NotBlank` |                                                    |

**Full Request JSON Example**

```json
{
  "sections": [
    {
      "category": "STRUCTURAL_GAPS",
      "found": true,
      "optionIds": ["OBS-ST-abc"],
      "otherNotes": "Gap under sink",
      "locationArea": "Kitchen"
    },
    {
      "category": "HYGIENE_SANITATION",
      "found": false,
      "optionIds": [],
      "otherNotes": null,
      "locationArea": "Pantry"
    },
    {
      "category": "PEST_SIGHTING",
      "found": true,
      "optionIds": ["OBS-PS-def"],
      "otherNotes": "2 cockroaches",
      "locationArea": "Store room"
    }
  ]
}
```

**Response**  
`200 OK` — `TechnicianObservationSectionResponse[]`

---

### `GET /api/v1/mobile/tasks/my/service-report`

**Purpose**  
Returns the aggregated service report for a task: summary, chemicals used, treatment methods, formatted observations, presigned photo URLs, PDF link, and customer feedback.

**Authorization**  
`APPLICATION_USER` + tenant-user token; task assigned.

**Query Parameters**: `taskId` required

**Request Body**: Not applicable

**Response**  
`200 OK` — `ServiceReportResponse`

**Full Response JSON Example**

```json
{
  "status": 200,
  "message": "Service report loaded",
  "data": {
    "taskId": "T-1A2B3C4D",
    "taskNumber": "TSK-BR-1-26-00001",
    "summary": {
      "customerName": "Acme Industries Pvt Ltd",
      "siteLabel": "Andheri Plant — Plot 12, MIDC",
      "scheduledDate": "2026-04-13",
      "scheduledDateDisplay": "13 Apr 2026",
      "timeRangeDisplay": "9:00 AM – 10:30 AM",
      "durationDisplay": "1h 15m",
      "techniciansDisplay": "John Doe, Priya Singh"
    },
    "chemicalsUsed": [
      { "chemicalName": "Termiticide A", "quantityDisplay": "0.8 LITER" }
    ],
    "treatment": {
      "methods": ["Spraying", "Gel baiting"],
      "pestTypesDisplay": "Integrated Pest Management, Termite, Pest Control"
    },
    "observations": {
      "structural": "Gap under sink. Kitchen",
      "entryPoints": "Kitchen",
      "hygiene": null,
      "sightings": "Cockroach. Store room"
    },
    "photos": [
      {
        "id": "photo-1",
        "photoType": "BEFORE",
        "viewUrl": "https://presigned.example.com/...",
        "downloadUrl": "https://presigned.example.com/..."
      }
    ],
    "serviceLogPdf": {
      "label": "Service report (PDF)",
      "relativePath": "/api/v1/tasks/export-pdf?taskId=T-1A2B3C4D",
      "absoluteUrl": "https://api.example.com/api/v1/tasks/export-pdf?taskId=T-1A2B3C4D"
    },
    "customerFeedback": {
      "averageRating": 4.5,
      "taskCustomerRating": 5,
      "taskCustomerFeedback": "Very professional service.",
      "feedbackVerified": true,
      "perTechnician": [
        {
          "technicianUserId": 101,
          "technicianName": "John Doe",
          "customerName": "Acme Industries Pvt Ltd",
          "customerPhone": "9876543210",
          "ratings": 5,
          "customerFeedback": "Very professional service."
        }
      ]
    },
    "notes": "Performed spraying and sealed entry points."
  }
}
```

---

### `POST /api/v1/mobile/tasks/my/customer-feedback`

**Purpose**  
Persists the same customer feedback once per technician assigned to the task (creates multiple rows).

**Authorization**  
`APPLICATION_USER` + tenant-user token; task assigned.

**Request Body Fields** (`TaskCustomerFeedbackSubmitRequest`)

| Field           | Type   | Required | Validation           | Example                   |
| --------------- | ------ | -------: | -------------------- | ------------------------- |
| `taskId`        | string |      Yes | `@NotBlank`, max 50  | `T-1A2B3C4D`              |
| `customerName`  | string |      Yes | `@NotBlank`, max 255 | `Acme Industries Pvt Ltd` |
| `customerPhone` | string |      Yes | `@NotBlank`, max 30  | `9876543210`              |
| `feedback`      | string |       No | max 8000             | `Great service`           |
| `ratings`       | int    |      Yes | `@Min(1) @Max(5)`    | `5`                       |

**Full Request JSON Example**

```json
{
  "taskId": "T-1A2B3C4D",
  "customerName": "Acme Industries Pvt Ltd",
  "customerPhone": "9876543210",
  "feedback": "Great service, very polite.",
  "ratings": 5
}
```

**Response**

`201 Created` — `TaskCustomerFeedbackResponse[]`

**Error cases**

- 400 if task has no technicians.
- 404 if task not assigned/not found.

---

## Mobile observation options (`/api/v1/mobile/observation-options`)

### `GET /structural`, `GET /hygiene`, `GET /pest-sighting`

**Purpose**  
Provide dropdown source data for the multi-selects in technician observations.

**Authorization**  
`APPLICATION_USER` + tenant-user token.

**Response**  
`200 OK` — `ObservationOptionItemResponse[]`

**Full Response JSON Example**

```json
{
  "status": 200,
  "message": "Options loaded",
  "data": [{ "id": "OBS-ST-abc", "label": "Gap under sink", "displayOrder": 1 }]
}
```

### `POST /structural`, `POST /hygiene`, `POST /pest-sighting`

**Purpose**  
Create a new option.

**Authorization**  
`APPLICATION_USER` + tenant-user token.

**Request body** (`ObservationOptionCreateRequest`)

| Field          | Type    | Required | Validation           | Notes               |
| -------------- | ------- | -------: | -------------------- | ------------------- |
| `label`        | string  |      Yes | `@NotBlank`, max 255 | trimmed server-side |
| `displayOrder` | int     |       No | —                    | defaults to 0       |
| `active`       | boolean |       No | —                    | defaults to true    |

**Full Request JSON Example**

```json
{ "label": "Open drain near pantry", "displayOrder": 5, "active": true }
```

---

## Mobile technician tracking (`/api/v1/mobile/technician-tracking`)

These endpoints manage the technician’s GPS pings and a simplified task travel/on-site workflow.

### `POST /check-in` and `POST /check-out`

**Purpose**  
Shift boundaries. Creates tracking rows with statuses `CLOCK_IN` and `CLOCK_OUT`.

**Authorization**  
`APPLICATION_USER`

**Request body** (`TechnicianTrackingLatLngRequest`)

| Field       | Type    | Required | Validation  |
| ----------- | ------- | -------: | ----------- |
| `latitude`  | decimal |      Yes | [-90, 90]   |
| `longitude` | decimal |      Yes | [-180, 180] |

**Response**  
`201 Created` — `TechnicianTrackingPingResponse`

---

### `POST /ping`

**Purpose**  
Periodic ping. When last status is `CLOCK_IN/IDLE/TRAVELLING/DEPARTED`, backend infers `IDLE` vs `TRAVELLING` using proximity threshold (default 100m). When last status is `ON_GOING/ARRIVED/ON_SITE`, status is repeated and taskId must exist.

**Common errors**  
400 if not checked in; 409 if previous workflow row missing taskId.

---

### `POST /task/start-travel`

**Purpose**  
Begin travel toward a task (status `ON_GOING`). Also updates the task:

- sets task `status = IN_PROGRESS`
- sets `startTime = now`
- sets `actualStartAt = now`

**Request body** (`TechnicianTrackingStartTravelRequest`) requires `startTravelForTask=true`.

---

### `POST /task/arrived`, `POST /task/on-site`, `POST /task/departed`

**Purpose**  
Site workflow steps. `arrived` requires previous status `ON_GOING`; `on-site` requires previous `ARRIVED`; `departed` requires previous `ON_SITE`.

**Task effects**

- `departed` updates task:
  - sets `status = COMPLETED`
  - sets `endTime = now`
  - sets `actualEndAt = now`

**Important frontend note:** This can complete the task **without** calling `/api/v1/tasks/complete`. If your app uses completion notes, materials, photos, OTP, or stock consumption, you should ensure the correct completion flow is used. **Assumption / Missing from backend context:** intended precedence between tracking “departed completion” and `/tasks/complete`.

---

## Validation and Exception Summary

| Field / Scenario             | Validation / Rule                             | Error Type     | Frontend Impact                 |
| ---------------------------- | --------------------------------------------- | -------------- | ------------------------------- |
| Create/edit: required fields | Bean validation on `CreateTaskRequest`        | 400 validation | Must fill form                  |
| Technician assignment        | At least 1 technician                         | 400 validation | Require primary tech            |
| Technician conflicts         | Overlap check excluding CANCELLED/COMPLETED   | 409 conflict   | Slot picker UX                  |
| Web update allowed           | Only when status is PENDING                   | 400            | Disable edit based on status    |
| Mobile detail                | Must be assigned                              | 404 not found  | Hide unauthorized tasks         |
| Mobile completion            | Must be IN_PROGRESS when APPLICATION_USER     | 400            | Require travel/on-site workflow |
| OTP completion               | OTP can expire / attempts capped / must match | 400            | OTP UI + retries                |
| Photo upload                 | Image-only, ≤10MB, before/after max 5         | 400            | Enforce constraints client-side |
| Execution blocks             | Invalid service/treatment/product ids         | 400            | Validate dropdown selections    |
| Observations                 | Duplicate category rejected                   | 400            | Ensure unique categories        |
| Tracking workflow            | Must check-in; state transitions enforced     | 400/409        | Step-based UI guards            |

---

## Frontend Integration Notes

- **Web vs mobile endpoints**:
  - Use `/api/v1/tasks` for admin screens.
  - Use `/api/v1/mobile/tasks/**` for technician-only flows.
- **Search minimum length**:
  - Task list endpoints apply search only when `search.length >= 3`.
- **Evidence uploads**:
  - Selfie/before/after uploads accept Base64; store returns object key in `filePath`. Service report returns presigned URLs for viewing/downloading.
- **Completion flows**:
  - `POST /api/v1/tasks/complete` is the “full” completion path (stock, photos, SO execution increment).
  - Tracking `/task/departed` also sets task completed without those extras; align your UI to the intended business process.
- **Service report**:
  - Build report UI from `/my/service-report`; prefer `serviceLogPdf.relativePath` for in-app download.
- **Headers**:
  - Always send `Authorization` and `X-Tenant-ID` for consistent tenant routing.

---

_Document generated from backend context for Module 21 Task Management: `TaskController`, `TaskServiceImpl`, task DTOs/enums/specs/exceptions; mobile controllers `MobileTaskController`, `MobileObservationOptionsController`, `MobileTechnicianTrackingController` and their related services/DTOs. Items marked **Missing from backend context** or **Assumption** indicate incomplete specification in the reviewed sources._
