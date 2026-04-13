# Module — Mobile APIs (Profile, HRM, Tasks, Petty Cash, Tracking, Public OTP)

## Short Description

This document describes **mobile-facing HTTP APIs** used by field technicians and mobile apps. It is aligned with the Postman collection **[`postman/Mobile_API.postman_collection.json`](../../postman/Mobile_API.postman_collection.json)** and the Java controllers under **`com.security.rbac.modules.mobile`**, plus **related routes** the collection references: **petty cash** (`/api/v1/petty-cash/**`) and **public customer OTP** (`/api/v1/public/customer-otp/**`).

**What frontend developers need to know**

- **Base URL:** Configure Postman variable **`baseUrl`** (e.g. `http://localhost:8080` or your deployed host). All paths below are relative to **`{{baseUrl}}`**.
- **Authentication:** Most routes require **`Authorization: Bearer {{accessToken}}`**. The collection sets **collection-level Bearer auth**; the **Public — Customer OTP** folder overrides to **no auth**.
- **Authority:** Mobile routes under `/api/v1/mobile/**` use **`@PreAuthorize("hasAuthority('APPLICATION_USER')")`** — the JWT must include this authority (typically **application user / technician** role). **Assumption:** Same JWT works with **`X-Tenant-ID`** if your tenant filter expects it (**Missing from backend context** in this excerpt — verify security config).
- **Response wrapper:** JSON APIs return **`ResponseStructure<T>`** → `{ "status", "message", "data" }` unless noted.
- **Petty cash** routes live on **`PettyCashController`** — they are **not** under `/mobile` but are included in the Postman collection for the same mobile app; authorization is **JWT for `/api/**`** with **service-layer** visibility rules (not necessarily `APPLICATION_USER` only — **see Module 24** / service impl).
- **Public OTP** endpoints are **unauthenticated** — do not send Bearer token.

---

## Postman collection reference

| Variable         | Purpose                                                                                  |
| ---------------- | ---------------------------------------------------------------------------------------- |
| `baseUrl`        | API host root                                                                            |
| `accessToken`    | JWT for Bearer auth                                                                      |
| `userId`         | Used in **PUT** `/api/v1/mobile/profile?userId=` (must match server rules for the token) |
| `taskId`         | Task id for task and tracking calls                                                      |
| `pcRequestId`    | Petty cash request id after create                                                       |
| `verificationId` | From **send OTP** response (`data.verificationId`) for verify                            |

---

## Authorization

### Authentication type

- **Bearer JWT** for all **`/api/v1/mobile/**`**, **`/api/v1/petty-cash/**`** (authenticated), and other secured `/api/**` routes.
- **No auth** for **`/api/v1/public/customer-otp/**`\*\*.

### Required token

- **Yes** for mobile + petty cash (unless your deployment exposes exceptions — **Assumption:** standard Spring Security).

### Roles / authorities

| Area                | Authority (typical)                                                                                                                                                                              |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `/api/v1/mobile/**` | **`APPLICATION_USER`** (per controller)                                                                                                                                                          |
| Petty cash          | **Not** `APPLICATION_USER`-scoped in controller — uses general API auth + **service-layer** checks (**Missing from backend context:** exact role matrix per action — see `PettyCashServiceImpl`) |

### Headers

| Header                           | When                                    |
| -------------------------------- | --------------------------------------- |
| `Authorization: Bearer <token>`  | Mobile + petty cash                     |
| `Content-Type: application/json` | POST/PUT with body                      |
| `X-Tenant-ID`                    | **Assumption:** if multitenancy enabled |

---

## Enums Used In This Module

### TechnicianStatus

**Where used:** `TechnicianTrackingPingResponse.technicianStatus` (tracking APIs).

| Value        | Meaning                                        |
| ------------ | ---------------------------------------------- |
| `CLOCK_IN`   | Shift check-in                                 |
| `CLOCK_OUT`  | Shift check-out                                |
| `IDLE`       | No active travel                               |
| `TRAVELLING` | Moving (not tied to task or between tasks)     |
| `ON_GOING`   | Travelling to task                             |
| `ARRIVED`    | Arrived at site                                |
| `ON_SITE`    | Working on site                                |
| `DEPARTED`   | Left site after task                           |
| `OFFLINE`    | **Assumption:** terminal/offline state if used |

---

### TaskStatus

**Where used:** Task list filters (`statuses` query param — repeatable).

| Value            |
| ---------------- |
| `PENDING`        |
| `TRAVEL_STARTED` |
| `IN_PROGRESS`    |
| `COMPLETED`      |
| `OVERDUE`        |
| `CANCELLED`      |

**Note:** **`GET /mobile/tasks/my/completed`** forces **`COMPLETED`** internally — extra `statuses` values may still be passed but logic overrides to completed list.

---

### TaskPriority

**Where used:** Optional query `taskPriority` on task list endpoints.

| Value      |
| ---------- |
| `NORMAL`   |
| `HIGH`     |
| `URGENT`   |
| `CRITICAL` |

---

### LeaveType

**Where used:** `HrmLeaveSelfApplyRequest.leaveType` (`POST /mobile/hrm/leaves/me`).

| Value | Meaning         |
| ----- | --------------- |
| `CL`  | Casual leave    |
| `SL`  | Sick leave      |
| `PL`  | Privilege leave |

---

### LeaveStatus

**Where used:** Optional query `status` on `GET /mobile/hrm/leaves/me/requests`.

| Value      |
| ---------- |
| `PENDING`  |
| `APPROVED` |
| `REJECTED` |

---

### Petty cash enums (summary)

**Where used:** Petty cash list filters and bodies — full lists are in **Module 24** (`PettyCashStatus`, `PettyCashCategory`, `PettyCashPaymentModeRequested`, etc.). Postman examples use values like **`DRAFT`**, **`RETURNED`**, **`FUEL`**, **`BANK_TRANSFER`**, **`LOCAL_CONVEYANCE`**.

---

## API List

Routes match **Postman** + **backend** naming.

| Method | Endpoint                                               | Purpose                                                 | Auth                     |
| ------ | ------------------------------------------------------ | ------------------------------------------------------- | ------------------------ |
| `GET`  | `/api/v1/mobile/profile`                               | Current user profile + leave balance                    | JWT + `APPLICATION_USER` |
| `PUT`  | `/api/v1/mobile/profile`                               | Update profile (query `userId`)                         | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/hrm/attendance/punch-in`               | Punch in (optional `lat`,`lng`)                         | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/hrm/attendance/punch-out`              | Punch out (optional `lat`,`lng`)                        | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/mobile/hrm/attendance/calendar`               | Attendance calendar (`month`,`year` optional)           | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/mobile/hrm/attendance/day`                    | Day detail: attendance + tasks (`date` required)        | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/hrm/leaves/me`                         | Submit leave                                            | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/mobile/hrm/leaves/me/balance`                 | Leave balance                                           | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/mobile/hrm/leaves/me/requests`                | Paginated my leave requests (`status` optional)         | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/mobile/tasks/dashboard`                       | Task dashboard counts for a date                        | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/mobile/tasks/my`                              | My tasks (filters + pagination)                         | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/mobile/tasks/my/completed`                    | My completed tasks                                      | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/mobile/tasks/my/detail`                       | Task detail (flat `TaskDetailResponse`)                 | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/mobile/tasks/my/detail-screen`                | Task detail (sectioned UI payload)                      | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/mobile/tasks/my/navigation`                   | Navigation + distance (`taskId`,`latitude`,`longitude`) | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/mobile/tasks/my/calendar`                     | Task calendar summary                                   | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/tasks/my/photos/selfie`                | Upload selfie (JSON Base64)                             | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/tasks/my/photos/before`                | Before photos batch                                     | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/tasks/my/photos/after`                 | After photos batch                                      | JWT + `APPLICATION_USER` |
| `PUT`  | `/api/v1/mobile/tasks/my/service-executions`           | Replace service execution blocks                        | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/tasks/my/technician-observations`      | Save observations                                       | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/mobile/tasks/my/service-report`               | Aggregated service report                               | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/tasks/my/customer-feedback`            | Submit customer feedback                                | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/technician-tracking/check-in`          | Shift check-in                                          | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/technician-tracking/ping`              | Periodic location ping                                  | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/technician-tracking/task/start-travel` | Start travel to task                                    | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/technician-tracking/task/arrived`      | Arrived at site                                         | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/technician-tracking/task/on-site`      | On site                                                 | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/technician-tracking/task/departed`     | Departed site                                           | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/technician-tracking/check-out`         | Shift check-out                                         | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/mobile/observation-options/structural`        | List structural options                                 | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/mobile/observation-options/hygiene`           | List hygiene options                                    | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/mobile/observation-options/pest-sighting`     | List pest sighting options                              | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/observation-options/structural`        | Create structural option                                | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/observation-options/hygiene`           | Create hygiene option                                   | JWT + `APPLICATION_USER` |
| `POST` | `/api/v1/mobile/observation-options/pest-sighting`     | Create pest sighting option                             | JWT + `APPLICATION_USER` |
| `GET`  | `/api/v1/petty-cash/requests/my/list`                  | My petty cash requests (query filters)                  | JWT                      |
| `POST` | `/api/v1/petty-cash/requests`                          | Create draft request                                    | JWT                      |
| `GET`  | `/api/v1/petty-cash/requests/by-id`                    | Request detail (`id` query)                             | JWT                      |
| `POST` | `/api/v1/public/customer-otp/send`                     | Send SMS OTP                                            | **No JWT**               |
| `POST` | `/api/v1/public/customer-otp/verify`                   | Verify OTP                                              | **No JWT**               |

---

## API Details (by Postman folders)

### Profile

#### `GET` `/api/v1/mobile/profile`

**Purpose:** Returns **`ApplicationProfile`**: nested **`user`** (`UserResponse`) + **`balance`** (`MobileLeaveBalanceResponse`).

**Authorization:** JWT + **`APPLICATION_USER`**.

**Query / Path:** None.

**Request Body:** Not applicable.

**Response:** **200**, `ResponseStructure<ApplicationProfile>`.

**Full Response JSON Example**

```json
{
  "status": 200,
  "message": "Profile fetched",
  "data": {
    "user": {
      "id": 1,
      "email": "tech@example.com"
    },
    "balance": {
      "casualRemaining": 8,
      "sickRemaining": 6,
      "privilegeRemaining": 12
    }
  }
}
```

**Assumption:** Exact `UserResponse` / balance fields — see DTOs in codebase; above is illustrative.

**cURL**

```bash
curl -X GET "{{baseUrl}}/api/v1/mobile/profile" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "X-Tenant-ID: tenant-001"
```

---

#### `PUT` `/api/v1/mobile/profile?userId={userId}`

**Purpose:** Partial update — **omit or null** fields to leave unchanged (see `MobileProfileUpdateRequest` Javadoc). Profile image: Base64, optional data-URL prefix; max **5MB** after decode (**Assumption:** enforced in service).

**Authorization:** JWT + **`APPLICATION_USER`**.

**Query Parameters**

| Field    | Type | Required | Description                                   |
| -------- | ---- | -------- | --------------------------------------------- |
| `userId` | long | Yes      | Target user id (must align with token policy) |

**Request Body Fields** (`MobileProfileUpdateRequest`)

| Field                                            | Type    | Required | Notes                                 |
| ------------------------------------------------ | ------- | -------- | ------------------------------------- |
| `contactNumber`                                  | string  | No       |                                       |
| `alternateNumber`                                | string  | No       |                                       |
| `currentAddressLine1` … `currentPincode`         | strings | No       |                                       |
| `permanentAddressLine1` … `permanentPincode`     | strings | No       |                                       |
| `bankName`, `accountNumber`, `ifscCode`, `upiId` | string  | No       |                                       |
| `profileImageBase64`                             | string  | No       | Base64 or `data:image/...;base64,...` |
| `profileImageFileName`                           | string  | No       | e.g. `avatar.jpg`                     |

**Full Request JSON Examples**

**Minimal (contact only)**

```json
{
  "contactNumber": "9876543210"
}
```

**Complete (Postman-style skeleton)**

```json
{
  "contactNumber": "9876543210",
  "alternateNumber": null,
  "currentAddressLine1": "Line 1",
  "currentCity": "Mumbai",
  "currentState": "MH",
  "currentCountry": "IN",
  "currentPincode": "400001",
  "permanentAddressLine1": null,
  "permanentCity": null,
  "permanentState": null,
  "permanentCountry": null,
  "permanentPincode": null,
  "bankName": null,
  "accountNumber": null,
  "ifscCode": null,
  "upiId": null,
  "profileImageBase64": null,
  "profileImageFileName": null
}
```

**Response:** **200**, `ResponseStructure<ApplicationProfile>`.

**cURL**

```bash
curl -X PUT "{{baseUrl}}/api/v1/mobile/profile?userId=1" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "Content-Type: application/json" \
  -d '{"contactNumber":"9876543210"}'
```

---

### HRM — Attendance

#### `POST` `/api/v1/mobile/hrm/attendance/punch-in`

**Query (optional):** `lat`, `lng` — `BigDecimal`.

**Request Body:** Not applicable.

**Response:** **200**, `data: null`.

**cURL**

```bash
curl -X POST "{{baseUrl}}/api/v1/mobile/hrm/attendance/punch-in?lat=12.9716&lng=77.5946" \
  -H "Authorization: Bearer {{accessToken}}"
```

---

#### `POST` `/api/v1/mobile/hrm/attendance/punch-out`

Same as punch-in.

---

#### `GET` `/api/v1/mobile/hrm/attendance/calendar`

**Query (optional):** `month` (1–12), `year`.

**Response:** **200**, `ResponseStructure<MobileAttendanceCalendarResponse>`.

---

#### `GET` `/api/v1/mobile/hrm/attendance/day?date={yyyy-MM-dd}`

**Query:** `date` — **required**, ISO date.

**Response:** **200**, `ResponseStructure<MobileCalendarDayDetailResponse>` (attendance + my tasks for that day).

---

### HRM — Leaves

#### `POST` `/api/v1/mobile/hrm/leaves/me`

**Request Body** — `HrmLeaveSelfApplyRequest` (record)

| Field         | Type        | Validation                        |
| ------------- | ----------- | --------------------------------- |
| `leaveType`   | `LeaveType` | `@NotNull` — `CL` \| `SL` \| `PL` |
| `fromDate`    | date        | `@NotNull`                        |
| `toDate`      | date        | `@NotNull`                        |
| `totalDays`   | long        | `@NotNull`                        |
| `description` | string      | `@NotBlank`                       |

**Full Request JSON Example**

```json
{
  "leaveType": "CL",
  "fromDate": "2026-04-15",
  "toDate": "2026-04-16",
  "totalDays": 2,
  "description": "Personal work"
}
```

**Response:** **201 Created**, `data: null`, message e.g. `"Leave submitted"`.

**cURL**

```bash
curl -X POST "{{baseUrl}}/api/v1/mobile/hrm/leaves/me" \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "Content-Type: application/json" \
  -d '{"leaveType":"CL","fromDate":"2026-04-15","toDate":"2026-04-16","totalDays":2,"description":"Personal work"}'
```

---

#### `GET` `/api/v1/mobile/hrm/leaves/me/balance`

**Request Body:** Not applicable.

**Response:** **200**, `ResponseStructure<MobileLeaveBalanceResponse>`.

---

#### `GET` `/api/v1/mobile/hrm/leaves/me/requests`

**Query**

| Field      | Type          | Required        | Description                       |
| ---------- | ------------- | --------------- | --------------------------------- |
| `status`   | `LeaveStatus` | No              | `PENDING`, `APPROVED`, `REJECTED` |
| `pageNo`   | int           | No (default 0)  |                                   |
| `pageSize` | int           | No (default 20) |                                   |

**Response:** **200**, `ResponseStructure<PaginationResponse<HrmLeaveMyApplicationResponse>>`.

**Search With Filters Example**

```http
GET /api/v1/mobile/hrm/leaves/me/requests?status=PENDING&pageNo=0&pageSize=20
```

---

### Tasks (core)

#### `GET` `/api/v1/mobile/tasks/dashboard?date={optional}`

**Query:** `date` optional — defaults **today** in service if omitted (**Assumption** — verify `TaskService.getMyTaskDashboard`).

**Response:** **200**, `ResponseStructure<MobileTaskDashboardResponse>`.

---

#### `GET` `/api/v1/mobile/tasks/my`

**Query:** `scheduledFrom`, `scheduledTo` (ISO date), **`statuses`** (repeatable — Spring `List<TaskStatus>`), `search`, `taskPriority`, `serviceCategory`, `pageNo`, `pageSize`.

**Response:** **200**, `PaginationResponse<TaskListResponse>`.

**Full Response JSON Example (shape)**

```json
{
  "status": 200,
  "message": "Tasks fetched",
  "data": {
    "count": 15,
    "next": null,
    "prev": null,
    "data": []
  }
}
```

**cURL (with filters)**

```bash
curl -G "{{baseUrl}}/api/v1/mobile/tasks/my" \
  -H "Authorization: Bearer {{accessToken}}" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=20" \
  --data-urlencode "statuses=PENDING" \
  --data-urlencode "statuses=IN_PROGRESS"
```

---

#### `GET` `/api/v1/mobile/tasks/my/completed`

Same filters as `/my` but backend passes **`COMPLETED`** only.

---

#### `GET` `/api/v1/mobile/tasks/my/detail?taskId=`

**Response:** **200**, `ResponseStructure<TaskDetailResponse>` — **flat** detail (not sectioned).

---

#### `GET` `/api/v1/mobile/tasks/my/detail-screen?taskId=`

**Response:** **200**, `ResponseStructure<MobileTaskDetailsScreenResponse>` — **sectioned** for mobile UI.

---

#### `GET` `/api/v1/mobile/tasks/my/navigation?taskId=&latitude=&longitude=`

**Response:** **200**, `ResponseStructure<MobileTaskNavigationResponse>`.

---

#### `GET` `/api/v1/mobile/tasks/my/calendar?month=&year=`

**Response:** **200**, `ResponseStructure<TaskCalendarSummaryResponse>`.

---

### Tasks — Field execution (photos, execution, observations, report, feedback)

#### `POST` `/api/v1/mobile/tasks/my/photos/selfie?taskId=`

**Body:** `TaskSelfieUploadRequest` — nested `file` with `fileName`, `fileType`, `fileData` (Base64). Matches Postman.

**Minimal JSON**

```json
{
  "file": {
    "fileName": "selfie.jpg",
    "fileType": "image/jpeg",
    "fileData": "REPLACE_WITH_BASE64_JPEG"
  }
}
```

**Response:** **201**, `ResponseStructure<PhotoResponse>`.

---

#### `POST` `/api/v1/mobile/tasks/my/photos/before?taskId=` / **`.../after`**

**Body:** `TaskPhotoBatchUploadRequest` — `files[]` same shape as selfie file object.

```json
{
  "files": [
    {
      "fileName": "before1.jpg",
      "fileType": "image/jpeg",
      "fileData": "REPLACE_WITH_BASE64_JPEG"
    }
  ]
}
```

**Response:** **201**, `ResponseStructure<List<PhotoResponse>>`.

---

#### `PUT` `/api/v1/mobile/tasks/my/service-executions?taskId=`

**Body:** `ServiceExecutionReplaceRequest` — `blocks[]` with `serviceName`, `infestationLevel`, `locationArea`, `treatmentIds`, `chemicals` (see Module 21 / task management DTOs).

**Example (Postman)**

```json
{
  "blocks": [
    {
      "serviceName": "General spray",
      "infestationLevel": "LOW",
      "locationArea": "Kitchen",
      "treatmentIds": [],
      "chemicals": []
    }
  ]
}
```

**Response:** **200**, `ResponseStructure<List<ServiceExecutionBlockResponse>>`.

---

#### `POST` `/api/v1/mobile/tasks/my/technician-observations?taskId=`

**Body:** `TechnicianObservationSaveRequest` — `sections[]` with `category` (`STRUCTURAL_GAPS`, `HYGIENE_SANITATION`, `PEST_SIGHTING`), `found`, `optionIds`, `locationArea`.

**Response:** **200**, `ResponseStructure<List<TechnicianObservationSectionResponse>>`.

---

#### `GET` `/api/v1/mobile/tasks/my/service-report?taskId=`

**Response:** **200**, `ResponseStructure<ServiceReportResponse>`.

---

#### `POST` `/api/v1/mobile/tasks/my/customer-feedback`

**Body:** `TaskCustomerFeedbackSubmitRequest` — includes **`taskId`** in body (not query).

```json
{
  "taskId": "TASK-001",
  "customerName": "Test Customer",
  "customerPhone": "9876543210",
  "feedback": "Good service.",
  "ratings": 5
}
```

**Response:** **201**, `ResponseStructure<List<TaskCustomerFeedbackResponse>>`.

---

### Petty Cash (collection paths)

#### `GET` `/api/v1/petty-cash/requests/my/list`

**Query:** Repeat **`statuses`** for multiple; optional `branchId`, `category`, `from`, `to`, `minAmount`, `maxAmount`, `q`, `pageNo`, `pageSize`.

**Example (Postman)**

```http
GET /api/v1/petty-cash/requests/my/list?statuses=DRAFT&statuses=RETURNED&pageNo=0&pageSize=15
```

**Response:** **200**, `PaginationResponse<PettyCashListResponse>`.

---

#### `POST` `/api/v1/petty-cash/requests`

**Body:** `PettyCashSaveRequest` — Postman example:

```json
{
  "paymentModeRequested": "BANK_TRANSFER",
  "category": "LOCAL_CONVEYANCE",
  "expenseDateFrom": "2026-03-20",
  "expenseDateTo": "2026-03-22",
  "amount": 1250,
  "description": "Local conveyance for client visits during March.",
  "preApproved": false
}
```

**Response:** **201**, `ResponseStructure<PettyCashDetailResponse>` — save **`data.id`** as **`pcRequestId`**.

---

#### `GET` `/api/v1/petty-cash/requests/by-id?id=`

**Response:** **200**, `ResponseStructure<PettyCashDetailResponse>`.

---

### Technician tracking

All **`POST`**, JSON body, **`TechnicianTrackingPingResponse`** in **`data`** (fields: `id`, `userId`, `taskId`, `technicianStatus`, `latitude`, `longitude`, `localDate`, `recordedAt`).

| Endpoint             | Body DTO                               | Notes                                                   |
| -------------------- | -------------------------------------- | ------------------------------------------------------- |
| `/check-in`          | `TechnicianTrackingLatLngRequest`      | `latitude`, `longitude`                                 |
| `/ping`              | `TechnicianTrackingLatLngRequest`      |                                                         |
| `/task/start-travel` | `TechnicianTrackingStartTravelRequest` | `taskId`, `latitude`, `longitude`, `startTravelForTask` |
| `/task/arrived`      | `TechnicianTrackingArrivedRequest`     | `taskId` optional, `flag` for arrival                   |
| `/task/on-site`      | `TechnicianTrackingTaskLatLngRequest`  |                                                         |
| `/task/departed`     | `TechnicianTrackingTaskLatLngRequest`  |                                                         |
| `/check-out`         | `TechnicianTrackingLatLngRequest`      |                                                         |

**Example: check-in**

```json
{
  "latitude": 12.9716,
  "longitude": 77.5946
}
```

**Example: start travel**

```json
{
  "taskId": "TASK-001",
  "latitude": 12.9716,
  "longitude": 77.5946,
  "startTravelForTask": true
}
```

**Response example**

```json
{
  "status": 201,
  "message": "Checked in",
  "data": {
    "id": 1001,
    "userId": 5,
    "taskId": null,
    "technicianStatus": "CLOCK_IN",
    "latitude": 12.9716,
    "longitude": 77.5946,
    "localDate": "2026-04-13",
    "recordedAt": "2026-04-13T09:00:00Z"
  }
}
```

---

### Observation options

**GET** lists return `List<ObservationOptionItemResponse>`. **POST** create uses `ObservationOptionCreateRequest`:

```json
{
  "label": "New gap type",
  "displayOrder": 0,
  "active": true
}
```

**Response:** **201**, single `ObservationOptionItemResponse`.

---

### Public — Customer OTP (no auth)

#### `POST` `/api/v1/public/customer-otp/send`

**Request Body:** `CustomerOtpSendRequest`

| Field          | Validation                                           |
| -------------- | ---------------------------------------------------- |
| `customerName` | `@NotBlank`, max 200                                 |
| `mobileNumber` | 10-digit India `^[6-9]\d{9}$`                        |
| `countryCode`  | optional, max 4 digits; default from config if blank |

```json
{
  "customerName": "Test User",
  "mobileNumber": "9876543210",
  "countryCode": "91"
}
```

**Response:** **200**, `ResponseStructure<CustomerOtpSendResponse>` — includes **`verificationId`** for next step.

---

#### `POST` `/api/v1/public/customer-otp/verify`

**Request Body:** `CustomerOtpVerifyRequest`

| Field            | Notes                        |
| ---------------- | ---------------------------- |
| `verificationId` | **Long**, from send response |
| `otp`            | 4–8 digits                   |
| `mobileNumber`   | 6–15 digits national number  |
| `countryCode`    | optional                     |

```json
{
  "verificationId": 1,
  "otp": "123456",
  "mobileNumber": "9876543210",
  "countryCode": "91"
}
```

**Response:** **200**, `ResponseStructure<CustomerOtpVerifyResponse>`.

**cURL (no Authorization header)**

```bash
curl -X POST "{{baseUrl}}/api/v1/public/customer-otp/send" \
  -H "Content-Type: application/json" \
  -d '{"customerName":"Test User","mobileNumber":"9876543210","countryCode":"91"}'
```

---

## Exceptions / Error Cases (typical)

| HTTP      | Reason                                         | Frontend note                   |
| --------- | ---------------------------------------------- | ------------------------------- |
| 400       | Validation (`MethodArgumentNotValidException`) | `validationErrors` map          |
| 401       | Missing/invalid JWT                            | Login / refresh                 |
| 403       | Missing `APPLICATION_USER` or petty-cash rule  | Hide action                     |
| 404       | Resource not found                             | Task / COA / petty id           |
| 409       | Conflict                                       | Rare on mobile flows            |
| 400 / 409 | Tracking workflow violations                   | Message from `ApiBaseException` |

**Error JSON** follows **`ValidationErrorResponse`** (`timestamp`, `status`, `error`, `message`, `path`, `validationErrors`).

**Validation error example**

```json
{
  "timestamp": "2026-04-13T10:00:00",
  "status": 400,
  "error": "Bad Request",
  "message": "Input validation failed",
  "path": "/api/v1/mobile/hrm/leaves/me",
  "validationErrors": {
    "description": "must not be blank"
  }
}
```

---

## Validation and Exception Summary

| Scenario                         | Typical error                                   |
| -------------------------------- | ----------------------------------------------- |
| Invalid leave dates / type       | 400                                             |
| Task not assigned to user        | 403/404 (**Assumption** — verify `TaskService`) |
| Tracking state machine violation | 400 with message                                |
| OTP wrong / expired              | 400 in verify                                   |
| Petty cash visibility            | 403/404 from service                            |

---

## Frontend Integration Notes

- **Postman:** Import **`postman/Mobile_API.postman_collection.json`**; set **`baseUrl`**, **`accessToken`**, **`taskId`**, **`userId`** for profile PUT.
- **Repeat query params:** Spring accepts repeated keys for **`statuses`** (tasks) and **`statuses`** (petty cash lists).
- **Detail vs detail-screen:** Use **`detail-screen`** for rich UI; **`detail`** for flat DTO integrations.
- **Photos:** JSON Base64 — not `multipart/form-data` for these mobile task endpoints.
- **OTP folder:** **Disable** inherited Bearer auth or override **noauth** per request (already in collection).
- **Deeper DTO fields:** Task execution, petty cash receipts, and full list row shapes are documented in **Module 21 (tasks)** and **Module 24 (petty cash)**.
- **Debouncing:** Throttle **`/technician-tracking/ping`** (~10 min as per Swagger) to match product rules.

---

_Generated from `postman/Mobile_API.postman_collection.json`, `MobileController`, `MobileHrmController`, `MobileTaskController`, `MobileTechnicianTrackingController`, `MobileObservationOptionsController`, `PettyCashController`, `CustomerOtpController`, and related DTOs._
