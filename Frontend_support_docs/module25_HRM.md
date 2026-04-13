# Module 25 – HRM (Human Resource Management)

## Short Description

Module 25 provides **tenant HRM** operations: **month-wise salary** view/edit, **mark paid** with PDF slip generation, **CSV salary import**; **attendance calendar** and **per-day manual correction** plus **CSV attendance import**; **leave** creation (HR on behalf), **balance**, **request list/detail**, and **approve/reject/pending** decisions.

Frontend developers should know:

- **Base paths:** `/api/v1/hrm/salary`, `/api/v1/hrm/attendance`, `/api/v1/hrm/leaves`.
- **Employee master list** (Tab 25.1 table) is **Module 8** — `GET /api/v1/users` with `EMPLOYEE_USER_MANAGEMENT_VIEW` (not `HRM_MANAGEMENT_*`). Module 25 APIs are opened from row actions (Salary / Attendance / Leave).
- **Auth:** JWT `Authorization: Bearer`; **`X-Tenant-ID`** for tenant schema (same as other modules).
- **Success wrapper:** `ResponseStructure` → `{ "status", "message", "data" }` except **salary slip download** → **raw `application/pdf`** (no JSON wrapper).
- **RBAC:** Controllers use `@PreAuthorize` with **`HRM_MANAGEMENT_READ` | `ADD` | `EDIT` | `APPROVE` | `DOWNLOAD`** and **`hasRole('CEO')`** bypass.
- **Presigned URLs:** Salary slip preview in `HrmSalaryMonthResponse.slipPresignedUrl` — **60 minutes** (same bucket pattern as other modules).

---

## Authorization

### Authentication

- **JWT Bearer** required for all HRM endpoints.

### Headers

| Header          | Required    | Purpose       |
| --------------- | ----------- | ------------- |
| `Authorization` | Yes         | Bearer token  |
| `X-Tenant-ID`   | Recommended | Tenant schema |

### Authority matrix (from controllers)

| Authority                 | Endpoints                                                                                                                                           |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `HRM_MANAGEMENT_READ`     | Salary **`GET /by-user`**; attendance **`GET /calendar`**, **`GET /day`**; leave **`GET /balance`**, **`GET /requests`**, **`GET /requests/by-id`** |
| `HRM_MANAGEMENT_ADD`      | Leave **`POST /leaves`**                                                                                                                            |
| `HRM_MANAGEMENT_EDIT`     | Salary **`PUT /update`**, **`POST /salary/upload`**; attendance **`PUT /day`**, **`POST /attendance/upload`**                                       |
| `HRM_MANAGEMENT_APPROVE`  | Salary **`PUT /mark-paid`**; leave **`PUT /requests/{id}/decision`**                                                                                |
| `HRM_MANAGEMENT_DOWNLOAD` | Salary **`GET /slip/download`**                                                                                                                     |
| `CEO` role                | All of the above via `hasRole('CEO')`                                                                                                               |

**Note:** `mark-paid` allows **`HRM_MANAGEMENT_APPROVE` OR `HRM_MANAGEMENT_EDIT`** OR CEO.

### Tenant / user-type notes

- Salary **mark-paid** sets `paidByUserId` only when JWT **`userType` is `TENANT`**.
- Leave balance / decisions use **`UserLeaveDetails`**; missing config → **404** _"User leave details not found"_.

---

## Enums Used In This Module

### SalaryPaymentStatus

| Value    | Meaning | Used In              |
| -------- | ------- | -------------------- |
| `PAID`   | Paid    | Month row, mark-paid |
| `UNPAID` | Unpaid  | Update/upload        |
| `DUE`    | Due     | Update/upload        |

**Frontend notes:** If status is **`UNPAID`** or **`DUE`**, backend requires **`reason`** with **min 10 characters** after update/recalc (`validatePaymentReason`). **`PAID`** does not require reason.

---

### AttendanceStatus

| Value      | Meaning           | Used In                              |
| ---------- | ----------------- | ------------------------------------ |
| `PRESENT`  | Present           | Calendar `DayCell`, save day default |
| `ABSENT`   | Absent            |                                      |
| `LEAVE`    | On approved leave | Derived calendar                     |
| `WEEK_OFF` | Weekly off        |                                      |
| `HOLIDAY`  | Public holiday    |                                      |
| `HALF_DAY` | Half day          |                                      |

---

### AttendanceSource

**Internal** (persisted on `HrmAttendanceDay`, not in `DayCell` JSON). Values: `TASK`, `AUTO`, `MANUAL`, `UPLOAD`, `MOBILE`. **Assumption:** Not returned on HRM calendar API response.

---

### LeaveType

| Value | Meaning      | Used In               |
| ----- | ------------ | --------------------- |
| `CL`  | Casual leave | Create, list, balance |
| `SL`  | Sick leave   |                       |
| `PL`  | Paid leave   |                       |

---

### LeaveStatus

| Value      | Meaning           | Used In                      |
| ---------- | ----------------- | ---------------------------- |
| `PENDING`  | Awaiting decision | Create, list                 |
| `APPROVED` | Approved          | Create (HR direct), decision |
| `REJECTED` | Rejected          | Create (HR), decision        |

---

### LeaveDecision

| Value     | Meaning            | Used In                |
| --------- | ------------------ | ---------------------- |
| `APPROVE` | Approve            | `PUT .../decision`     |
| `REJECT`  | Reject             |                        |
| `PENDING` | Re-open to pending | Clears reviewer fields |

**Frontend notes:** **`REJECT`** requires **`reason`** min **10** chars (service validation).

---

### SalaryType (read-only in salary response)

From `UserSalaryDetails`: `CTC` | `FIXED` | `HOURLY`.

---

### LeaveResetCycle (read-only in balance response)

`YEARLY` | `MONTHLY` | `CUSTOM`.

---

## API List

| Method | Endpoint                                    | Purpose                   | Authorization              |
| ------ | ------------------------------------------- | ------------------------- | -------------------------- |
| `GET`  | `/api/v1/hrm/salary/by-user`                | Month salary view         | `READ` or CEO              |
| `PUT`  | `/api/v1/hrm/salary/update`                 | Edit month                | `EDIT` or CEO              |
| `PUT`  | `/api/v1/hrm/salary/mark-paid`              | Mark paid + generate slip | `APPROVE` or `EDIT` or CEO |
| `GET`  | `/api/v1/hrm/salary/slip/download`          | Download PDF              | `DOWNLOAD` or CEO          |
| `POST` | `/api/v1/hrm/salary/upload`                 | Bulk CSV                  | `EDIT` or CEO              |
| `GET`  | `/api/v1/hrm/attendance/calendar`           | Month calendar            | `READ` or CEO              |
| `GET`  | `/api/v1/hrm/attendance/day`                | Single day cell           | `READ` or CEO              |
| `PUT`  | `/api/v1/hrm/attendance/day`                | Save/correct day          | `EDIT` or CEO              |
| `POST` | `/api/v1/hrm/attendance/upload`             | Bulk CSV                  | `EDIT` or CEO              |
| `POST` | `/api/v1/hrm/leaves`                        | HR create leave           | `ADD` or CEO               |
| `GET`  | `/api/v1/hrm/leaves/balance`                | Leave balance             | `READ` or CEO              |
| `GET`  | `/api/v1/hrm/leaves/requests`               | List requests (paged)     | `READ` or CEO              |
| `GET`  | `/api/v1/hrm/leaves/requests/by-id`         | Request detail            | `READ` or CEO              |
| `PUT`  | `/api/v1/hrm/leaves/requests/{id}/decision` | Approve/reject/pending    | `APPROVE` or CEO           |

---

## Shared response types

### `HrmSalaryMonthResponse` (record)

Includes: `userId`, `empId`, `employeeName`, `department`, `designation`, `roleId`, `roleName`, `month`, `year`, read-only salary config (`salaryType`, effective dates, PF/ESI/TDS flags, holiday/OT config), month amounts (`basicSalary`, `hra`, … `netSalary`), `paymentStatus`, `reason`, `paymentDate`, **`slipPresignedUrl`**.

### `HrmAttendanceCalendarResponse`

- `userId`, `month`, `year`
- `days[]` — `DayCell`: `date`, `status`, `punchInAt`, `punchOutAt`, `totalMinutes`, `tasksAssigned`, `tasksCompleted`, `tasksPending`, `notes`, lat/lng fields
- `summary` — `workingDays`, `presentDays`, `absentDays`, `leaveDays`, `holidayDays`, `weekOffDays`

### `HrmLeaveRequestResponse` (record)

`id`, `leaveCode`, `userId`, `empId`, `employeeName`, `department`, `branchId`, `leaveType`, `fromDate`, `toDate`, `workingDays`, `description`, `status`, `rejectionReason`, `reviewedByUserId`, `reviewedAt`, `createdAt`

### `HrmLeaveBalanceResponse` (record)

`userId`, `resetCycle`, `resetFrom`, `resetTo`, `clTotal`, `clUsed`, `clRemaining`, `slTotal`, `slUsed`, `slRemaining`, `plTotal`, `plUsed`, `plRemaining`

### `PaginationResponse<HrmLeaveRequestResponse>`

`count`, `next`, `prev`, `data[]` — links built from `app.base-url` + `/api/v1/hrm/leaves/requests?...`

---

## API Details — Salary

### `GET` `/api/v1/hrm/salary/by-user`

#### Purpose

Load (or seed) **month** salary for an employee. Defaults **month/year** to **current** if omitted.

#### Authorization

`HRM_MANAGEMENT_READ` or `CEO`

#### Query parameters

| Field    | Type | Required | Example    |
| -------- | ---- | -------- | ---------- |
| `userId` | long | Yes      | `42`       |
| `month`  | int  | No       | `3` (1–12) |
| `year`   | int  | No       | `2026`     |

#### Request Body

Not applicable.

#### Response

**200** — `ResponseStructure<HrmSalaryMonthResponse>`

#### Full response JSON example

```json
{
  "status": 200,
  "message": "Salary fetched",
  "data": {
    "userId": 42,
    "empId": "EMP001",
    "employeeName": "Ravi Sharma",
    "department": "Operations",
    "designation": "Technician",
    "roleId": 5,
    "roleName": "Service Technician",
    "month": 3,
    "year": 2026,
    "salaryType": "FIXED",
    "salaryEffectiveFrom": "2025-04-01",
    "salaryEffectiveTo": null,
    "pfApplicable": true,
    "esiApplicable": true,
    "tdsApplicable": true,
    "holidayWorkApplicable": true,
    "holidayWorkAmount": 500.0,
    "overtimeApplicable": true,
    "perHourIncentivePay": 200.0,
    "maxOtHoursPerMonth": 40,
    "basicSalary": 30000.0,
    "hra": 12000.0,
    "otherAllowance": 0,
    "incentive": 2000.0,
    "deductions": 0,
    "otherDeductions": 0,
    "pf": 3600.0,
    "esi": 0,
    "tds": 1500.0,
    "otHours": 8.5,
    "holidayDaysWorked": 1,
    "otAmount": 1700.0,
    "holidayIncentiveAmt": 500.0,
    "grossSalary": 46200.0,
    "totalDeductions": 5100.0,
    "netSalary": 41100.0,
    "paymentStatus": "UNPAID",
    "reason": "Awaiting finance clearance for March payroll.",
    "paymentDate": null,
    "slipPresignedUrl": null
  }
}
```

#### Exceptions

| HTTP | When           | Message (examples)                      |
| ---- | -------------- | --------------------------------------- |
| 404  | User missing   | `User not found: {id}`                  |
| 400  | Bad month/year | `month must be 1-12`, `year is invalid` |

#### cURL

```bash
curl -s -G "https://{host}/api/v1/hrm/salary/by-user" \
  --data-urlencode "userId=42" \
  --data-urlencode "month=3" \
  --data-urlencode "year=2026" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}"
```

---

### `PUT` `/api/v1/hrm/salary/update`

#### Purpose

Partial update of month fields; server **recalculates** gross/net; validates payment reason for UNPAID/DUE.

#### Authorization

`HRM_MANAGEMENT_EDIT` or `CEO`

#### Query parameters

| Field    | Type | Required |
| -------- | ---- | -------- |
| `userId` | long | Yes      |
| `month`  | int  | Yes      |
| `year`   | int  | Yes      |

#### Request body — `HrmSalaryMonthUpdateRequest`

All fields **optional** (partial patch):

| Field                                               | Type    | Notes                                               |
| --------------------------------------------------- | ------- | --------------------------------------------------- |
| `basicSalary`, `hra`, `otherAllowance`, `incentive` | decimal |                                                     |
| `deductions`, `otherDeductions`, `pf`, `esi`, `tds` | decimal |                                                     |
| `otHours`                                           | decimal |                                                     |
| `holidayDaysWorked`                                 | int     | `@Min(0)`                                           |
| `paymentStatus`                                     | enum    |                                                     |
| `reason`                                            | string  | Required length ≥10 if status UNPAID/DUE after save |

#### Full request JSON examples

**Minimal patch**

```json
{
  "incentive": 2500.0
}
```

**Set UNPAID with reason**

```json
{
  "paymentStatus": "UNPAID",
  "reason": "Payroll pending approval from finance department before release."
}
```

**Complete patch**

```json
{
  "basicSalary": 50000.0,
  "hra": 10000.0,
  "otherAllowance": 0,
  "incentive": 2000.0,
  "deductions": 5000.0,
  "otherDeductions": 0,
  "pf": 1800.0,
  "esi": 0,
  "tds": 2500.0,
  "otHours": 8.5,
  "holidayDaysWorked": 1,
  "paymentStatus": "DUE",
  "reason": "Payment deferred to next cycle due to cash flow review."
}
```

#### Response

**200** — updated `HrmSalaryMonthResponse`

#### Exceptions

| HTTP | Message                                                                 |
| ---- | ----------------------------------------------------------------------- |
| 400  | `Reason is required (min 10 chars) when paymentStatus is UNPAID or DUE` |
| 400  | `Net salary cannot be negative`                                         |
| 404  | User not found                                                          |

#### Error JSON

```json
{
  "status": 400,
  "error": "Bad Request",
  "message": "Reason is required (min 10 chars) when paymentStatus is UNPAID or DUE",
  "path": "/api/v1/hrm/salary/update",
  "validationErrors": null
}
```

#### cURL

```bash
curl -s -X PUT "https://{host}/api/v1/hrm/salary/update?userId=42&month=3&year=2026" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}" \
  -H "Content-Type: application/json" \
  -d "{\"incentive\":2000,\"paymentStatus\":\"UNPAID\",\"reason\":\"Pending bank mandate verification for this employee.\"}"
```

---

### `PUT` `/api/v1/hrm/salary/mark-paid`

#### Purpose

Sets **`PAID`**, **`paymentDate`**, clears **`reason`**, regenerates/uploads **PDF slip**, returns presigned URL.

#### Authorization

`HRM_MANAGEMENT_APPROVE` or `HRM_MANAGEMENT_EDIT` or `CEO`

#### Query parameters

`userId`, `month`, `year` — required.

#### Request body — `HrmSalaryMarkPaidRequest`

```json
{ "paymentDate": "2026-03-31" }
```

#### Response

**200** — `HrmSalaryMonthResponse` with `paymentStatus: "PAID"`, `slipPresignedUrl` set.

#### cURL

```bash
curl -s -X PUT "https://{host}/api/v1/hrm/salary/mark-paid?userId=42&month=3&year=2026" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}" \
  -H "Content-Type: application/json" \
  -d "{\"paymentDate\":\"2026-03-31\"}"
```

---

### `GET` `/api/v1/hrm/salary/slip/download`

#### Purpose

Download salary slip **PDF** (binary).

#### Authorization

`HRM_MANAGEMENT_DOWNLOAD` or `CEO`

#### Query parameters

`userId`, `month`, `year`

#### Request Body

Not applicable.

#### Response

- **200** — `Content-Type: application/pdf`
- **`Content-Disposition`:** `attachment; filename="salary-slip-{userId}-{year}-{mm}.pdf"`
- **Body:** PDF bytes (**not** `ResponseStructure`)

#### Exceptions

| HTTP | When                                |
| ---- | ----------------------------------- |
| 404  | Salary month or slip record missing |

#### Frontend notes

- Open in new tab or save blob; handle 403 if no `DOWNLOAD` permission.

#### cURL

```bash
curl -L -o slip.pdf "https://{host}/api/v1/hrm/salary/slip/download?userId=42&month=3&year=2026" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}"
```

---

### `POST` `/api/v1/hrm/salary/upload`

#### Purpose

Bulk import from **CSV** (`.csv` only). Row-level errors collected without failing whole file.

#### Authorization

`HRM_MANAGEMENT_EDIT` or `CEO`

#### Request

**`multipart/form-data`** — part name **`file`** (required).

#### Expected headers (case-insensitive mapping)

Emp ID, Month, Basic Salary, HRA, Other Allowance, Incentive, Deductions, PF, ESI, TDS, Other Deductions, OT Hours, Holiday Days Worked, Payment Status, Reason

#### Response

**200** — `ResponseStructure<Object>` where `data` is:

```json
{
  "imported": 2,
  "errors": [{ "row": "3", "error": "User not found for empId: EMP999" }]
}
```

#### Exceptions (whole request)

| Message                                                |
| ------------------------------------------------------ |
| `File is required`                                     |
| `Only .csv is supported right now`                     |
| `CSV must have header and at least one row`            |
| `CSV format mismatch. Missing required header(s): ...` |
| `CSV format mismatch. Unknown header(s): ...`          |

#### cURL

```bash
curl -s -X POST "https://{host}/api/v1/hrm/salary/upload" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}" \
  -F "file=@salary.csv;type=text/csv"
```

---

## API Details — Attendance

### `GET` `/api/v1/hrm/attendance/calendar`

#### Purpose

Full month grid with **derived** status (tasks, leave, holidays, weekly off, saved overrides).

#### Query parameters

| Field    | Type | Required             |
| -------- | ---- | -------------------- |
| `userId` | long | Yes                  |
| `month`  | int  | No (default current) |
| `year`   | int  | No                   |

#### Request Body

Not applicable.

#### Response

**200** — `ResponseStructure<HrmAttendanceCalendarResponse>`

#### cURL

```bash
curl -s -G "https://{host}/api/v1/hrm/attendance/calendar" \
  --data-urlencode "userId=42" \
  --data-urlencode "month=3" \
  --data-urlencode "year=2026" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}"
```

---

### `GET` `/api/v1/hrm/attendance/day`

#### Purpose

Single **`DayCell`** for a date (same shape as calendar item).

#### Query parameters

`userId`, `date` (`YYYY-MM-DD`)

#### Request Body

Not applicable.

#### Exceptions

404 if day cannot be resolved (e.g. invalid date handling → _"Day not found"_)

---

### `PUT` `/api/v1/hrm/attendance/day`

#### Purpose

Manual save: punches, optional **status**, notes. Source set to **MANUAL**.

#### Authorization

`HRM_MANAGEMENT_EDIT` or `CEO`

#### Request body — `HrmAttendanceDaySaveRequest`

| Field                     | Type            | Required                                         |
| ------------------------- | --------------- | ------------------------------------------------ |
| `userId`                  | long            | Yes                                              |
| `date`                    | date            | Yes                                              |
| `punchInAt`, `punchOutAt` | offset datetime | No                                               |
| `status`                  | enum            | No — default PRESENT if punch-in set else ABSENT |
| `notes`                   | string          | No                                               |

#### Full request JSON examples

**Present with punches**

```json
{
  "userId": 42,
  "date": "2026-03-10",
  "punchInAt": "2026-03-10T09:30:00+05:30",
  "punchOutAt": "2026-03-10T18:15:00+05:30",
  "status": "PRESENT",
  "notes": "Corrected from biometric mismatch"
}
```

**Absent**

```json
{
  "userId": 42,
  "date": "2026-03-12",
  "status": "ABSENT"
}
```

#### Exceptions

400 `Punch In must be before Punch Out`

#### cURL

```bash
curl -s -X PUT "https://{host}/api/v1/hrm/attendance/day" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}" \
  -H "Content-Type: application/json" \
  -d "{\"userId\":42,\"date\":\"2026-03-10\",\"punchInAt\":\"2026-03-10T04:00:00Z\",\"punchOutAt\":\"2026-03-10T12:45:00Z\",\"status\":\"PRESENT\"}"
```

---

### `POST` `/api/v1/hrm/attendance/upload`

#### Purpose

CSV bulk upload — same semantics as salary: `.csv`, row errors in `errors[]`.

#### Authorization

`HRM_MANAGEMENT_EDIT` or `CEO`

#### Multipart

Part name **`file`**.

**Assumption:** Headers validated in `validateAttendanceCsvHeaders` — align with `docs/hrm/sample-attendance-upload.csv` in repo.

#### Response

Same pattern: `{ "imported": n, "errors": [...] }` inside `data`.

#### cURL

```bash
curl -s -X POST "https://{host}/api/v1/hrm/attendance/upload" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}" \
  -F "file=@attendance.csv;type=text/csv"
```

---

## API Details — Leave

### `POST` `/api/v1/hrm/leaves`

#### Purpose

HR creates leave for an employee. Can set **`PENDING`**, **`APPROVED`**, or **`REJECTED`** at creation (`REJECTED` sets synthetic rejection reason; **`APPROVED`** checks balance).

#### Authorization

`HRM_MANAGEMENT_ADD` or `CEO`

#### Request body — `HrmLeaveCreateRequest`

| Field         | Type   | Required | Validation                      |
| ------------- | ------ | -------- | ------------------------------- |
| `userId`      | long   | Yes      |                                 |
| `leaveType`   | enum   | Yes      |                                 |
| `fromDate`    | date   | Yes      | ≤ `toDate`                      |
| `toDate`      | date   | Yes      |                                 |
| `description` | string | Yes      | **Min 10** chars (service trim) |
| `status`      | enum   | Yes      |                                 |

#### Full request JSON examples

**Pending application**

```json
{
  "userId": 42,
  "leaveType": "CL",
  "fromDate": "2026-04-10",
  "toDate": "2026-04-12",
  "description": "Family function out of town; need three working days off.",
  "status": "PENDING"
}
```

**Direct approve (HR)**

```json
{
  "userId": 42,
  "leaveType": "SL",
  "fromDate": "2026-04-05",
  "toDate": "2026-04-05",
  "description": "Medical leave for scheduled procedure on this date.",
  "status": "APPROVED"
}
```

**Direct reject**

```json
{
  "userId": 42,
  "leaveType": "PL",
  "fromDate": "2026-05-01",
  "toDate": "2026-05-01",
  "description": "Rejected at intake due to blackout period policy.",
  "status": "REJECTED"
}
```

#### Response

**201** — `HrmLeaveRequestResponse`

#### Business errors (400 / IllegalArgumentException)

| Message                                       |
| --------------------------------------------- |
| `fromDate must be <= toDate`                  |
| `description must be at least 10 characters`  |
| `Leave overlaps with existing approved leave` |
| `No working days in selected range`           |
| `Insufficient leave balance` (when approving) |

#### cURL

```bash
curl -s -X POST "https://{host}/api/v1/hrm/leaves" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}" \
  -H "Content-Type: application/json" \
  -d "{\"userId\":42,\"leaveType\":\"CL\",\"fromDate\":\"2026-04-10\",\"toDate\":\"2026-04-12\",\"description\":\"Family function requiring leave.\",\"status\":\"PENDING\"}"
```

---

### `GET` `/api/v1/hrm/leaves/balance`

#### Query

`userId` (required)

#### Response

**200** — `HrmLeaveBalanceResponse`

#### Exceptions

404 `User leave details not found for user: {id}`

---

### `GET` `/api/v1/hrm/leaves/requests`

#### Query parameters

| Field        | Type   | Required   |
| ------------ | ------ | ---------- |
| `branchId`   | string | No         |
| `department` | string | No         |
| `status`     | enum   | No         |
| `leaveType`  | enum   | No         |
| `from`       | date   | No         |
| `to`         | date   | No         |
| `search`     | string | No         |
| `pageNo`     | int    | Default 0  |
| `pageSize`   | int    | Default 10 |

#### Request Body

Not applicable.

#### Response

**200** — `ResponseStructure<PaginationResponse<HrmLeaveRequestResponse>>`

#### Full example

```json
{
  "status": 200,
  "message": "Leave requests fetched",
  "data": {
    "count": 1,
    "next": null,
    "prev": null,
    "data": [
      {
        "id": 1001,
        "leaveCode": "LV-A1B2C3D4",
        "userId": 42,
        "empId": "EMP001",
        "employeeName": "Ravi Sharma",
        "department": "Operations",
        "branchId": "BR-01",
        "leaveType": "CL",
        "fromDate": "2026-04-10",
        "toDate": "2026-04-12",
        "workingDays": 3,
        "description": "Family function out of town",
        "status": "PENDING",
        "rejectionReason": null,
        "reviewedByUserId": null,
        "reviewedAt": null,
        "createdAt": "2026-04-08T10:00:00Z"
      }
    ]
  }
}
```

---

### `GET` `/api/v1/hrm/leaves/requests/by-id`

#### Query

`id` (long) — required.

#### Response

**200** — single `HrmLeaveRequestResponse`

#### Exceptions

404 `Leave request not found: {id}`

---

### `PUT` `/api/v1/hrm/leaves/requests/{id}/decision`

#### Authorization

`HRM_MANAGEMENT_APPROVE` or `CEO`

#### Request body — `HrmLeaveDecisionRequest`

| Field      | Type   | Required                               |
| ---------- | ------ | -------------------------------------- |
| `decision` | enum   | Yes                                    |
| `reason`   | string | Required for **REJECT** (min 10 chars) |

#### Full request JSON examples

**Approve**

```json
{
  "decision": "APPROVE"
}
```

**Reject**

```json
{
  "decision": "REJECT",
  "reason": "Insufficient documentation attached; please upload medical certificate."
}
```

**Mark pending again**

```json
{
  "decision": "PENDING"
}
```

#### Exceptions

400 `Reason is required (min 10 chars) when rejecting`  
400 `Insufficient leave balance` on approve

---

## Validation and Exception Summary

| Scenario              | Rule                     | HTTP | Frontend             |
| --------------------- | ------------------------ | ---- | -------------------- |
| Salary UNPAID/DUE     | reason ≥ 10 chars        | 400  | Show reason field    |
| Salary net            | must be ≥ 0              | 400  | Adjust inputs        |
| CSV headers           | exact set                | 400  | Use template         |
| Attendance punches    | in before out            | 400  | Validate times       |
| Leave create          | description ≥ 10         | 400  |                      |
| Leave overlap         | no overlap with approved | 400  |                      |
| Leave decision reject | reason ≥ 10              | 400  |                      |
| Leave balance         | UserLeaveDetails exists  | 404  | Configure Module 6/8 |
| Entity not found      | user/salary/day          | 404  |                      |
| Auth                  | missing permission       | 403  | Hide action          |

---

## Frontend Integration Notes

- **Employee picker:** Load from **`GET /api/v1/users`** (Module 8), then pass **`userId`** into HRM APIs.
- **Salary:** After **mark-paid**, refresh detail; **download** uses separate **DOWNLOAD** permission.
- **Attendance:** Calendar cells may be **derived** without saved row — saving creates **`MANUAL`** row.
- **Leave:** **`workingDays`** is server-computed (excludes holidays/weekly off). **`leaveCode`** is generated (e.g. `LV-` + hex).
- **Pagination (leave list):** Use **`next`** URL from API or increment **`pageNo`** with same filters.
- **Errors:** `IllegalArgumentException` → **400** with `message`; `EntityNotFoundException` → **404**.

---

## Assumptions / outside this module

- **Mobile** self-service leave (`submitMyLeave`) and **mobile attendance** punch APIs live under **`/api/v1/mobile/...`** — not listed in the controllers above.
- **`AttendanceSource`** is not exposed on `DayCell`.
- CSV column names for attendance: confirm against **`validateAttendanceCsvHeaders`** and sample CSV in **`docs/hrm/`**.

---

_Generated from `HrmSalaryController`, `HrmAttendanceController`, `HrmLeaveController`, and service implementations._
