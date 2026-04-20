# Module 26 – Technician Performance

## Short Description

Module 26 exposes **read-only KPIs** for **field technicians** (`APPLICATION_USER` employees): organization **summary cards** (current month-to-date), a **sortable, paginated dashboard** table with period presets (week / month / quarter / custom) and filters, and an **individual lifetime profile** (joining date → today) with KPI cards, task/attendance/revenue/material/customer/re-task breakdowns, and **weighted performance score** breakdown.

Data is aggregated from **tasks** (Module 21), **attendance** (HRM), **leave**, **sales order line totals**, and **task materials**. No create/update/delete APIs exist in this module.

Frontend developers should know:

- **Base path:** `/api/v1/technician-performance`.
- **Auth:** JWT `Authorization: Bearer`; **`X-Tenant-ID`** for tenant routing.
- **Wrapper:** `ResponseStructure` → `{ "status", "message", "data" }`.
- **RBAC:** `TECHNICIAN_PERFORMANCE_READ` or **`CEO`** on every endpoint.
- **Branch scoping:** Resolved in the service from JWT (`ROOT` is **blocked**; `CEO` sees all branches or one selected; `TENANT` users may be **all branches** or **branch-scoped** for `BRANCH_MANAGER` / `TECHNICIAN_MANAGER`). Invalid branch → **403** / **400** as implemented.
- **Performance score:** `null` when **primary completed tasks** in the computed window is **less than** `app.technician-performance.min-completed-tasks-for-score` (default **5**). UI should show `insufficientDataForScore` / `INSUFFICIENT_DATA` grade.
- **Scoring weights** are configurable via `app.technician-performance.*` (defaults in **Assumptions**).

---

## Authorization

### Authentication

- **JWT Bearer** required.

### Headers

| Header          | Required    | Purpose       |
| --------------- | ----------- | ------------- |
| `Authorization` | Yes         | Bearer token  |
| `X-Tenant-ID`   | Recommended | Tenant schema |

### Permissions (controller)

| Rule                                                                  | Endpoints                |
| --------------------------------------------------------------------- | ------------------------ |
| `hasRole('CEO')` **or** `hasAuthority('TECHNICIAN_PERFORMANCE_READ')` | **All** Module 26 routes |

### Service-layer access rules (dashboard & detail)

| User type / role    | Behavior                                                                                                                   |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **`ROOT`**          | **403** — `"Not available for root user"`                                                                                  |
| **`CEO`**           | All branches, or single `branchId` if valid                                                                                |
| **`TENANT`**        | User loaded from JWT; unknown branch → **403** `"No access to this branch"`; empty branch list → empty summary / dashboard |
| Non-tenant, non-CEO | **403** — `"Unsupported user type"`                                                                                        |
| Missing user row    | **401** — `"User not found"`                                                                                               |

### Employee detail (`GET /employee`)

| Condition                                                | HTTP                                             |
| -------------------------------------------------------- | ------------------------------------------------ |
| No accessible branches                                   | **404** `"No accessible branches"`               |
| Invalid `userId`                                         | **404** `"Employee not found"`                   |
| User not application user / inactive / status not ACTIVE | **404** `"Not a field technician profile"`       |
| User’s branch not in caller’s scope                      | **403** `"Employee outside accessible branches"` |

---

## Enums Used In This Module

### TechnicianPerformancePeriod

Used for **`GET /dashboard`** query `period`.

| Value          | Meaning                 | Date range (inclusive `from`, inclusive `to`)          |
| -------------- | ----------------------- | ------------------------------------------------------ |
| `WEEK`         | Week-to-date (ISO week) | Monday of current week → **today**                     |
| `MONTH`        | Month-to-date           | 1st of month → **today**                               |
| `QUARTER`      | Quarter-to-date         | First day of current calendar quarter → **today**      |
| `CUSTOM_RANGE` | Custom                  | **Requires** query `from` and `to` (both `YYYY-MM-DD`) |

**Frontend notes:** If `period=CUSTOM_RANGE` and `from`/`to` missing → **400** _"Parameters 'from' and 'to' are required when period=CUSTOM_RANGE"_. If `to` before `from` → **400** _"Invalid date range"_.

---

### Score grades (string labels, not an enum)

Returned in `scoreGrade` / `overallGrade`:

| Label               | Condition       |
| ------------------- | --------------- |
| `INSUFFICIENT_DATA` | Score is `null` |
| `EXCELLENT`         | Score ≥ 90      |
| `GOOD`              | Score ≥ 75      |
| `AVERAGE`           | Score ≥ 60      |
| `BELOW_AVERAGE`     | Score below 60  |

---

### Revenue tier labels (`revenueTierLabel` on detail KPI)

| Label          | Percentile (approx.)  |
| -------------- | --------------------- |
| `Top 10%`      | ≥ 90th percentile     |
| `Top 25%`      | ≥ 75th                |
| `Above median` | ≥ 50th                |
| `Below median` | below 50th percentile |

---

## API List

| Method | Endpoint                                   | Purpose                                                    | Authorization                        |
| ------ | ------------------------------------------ | ---------------------------------------------------------- | ------------------------------------ |
| `GET`  | `/api/v1/technician-performance/summary`   | Org summary cards (month-to-date, all accessible branches) | `TECHNICIAN_PERFORMANCE_READ` or CEO |
| `GET`  | `/api/v1/technician-performance/dashboard` | Paginated ranked table + filters                           | Same                                 |
| `GET`  | `/api/v1/technician-performance/employee`  | Single technician lifetime detail                          | Same                                 |

---

## Shared response wrappers

### `ResponseStructure<T>`

```json
{
  "status": 200,
  "message": "<string>",
  "data": {}
}
```

### `PaginationResponse<T>` (dashboard only)

| Field   | Type                                                    |
| ------- | ------------------------------------------------------- |
| `count` | long — total rows before paging                         |
| `next`  | string \| null — full URL including `pageSize`, filters |
| `prev`  | string \| null                                          |
| `data`  | `TechnicianPerformanceRowDto[]`                         |

**Note:** When `branchScope` is empty, backend returns empty page; **`prev`** in edge cases may be a minimal string (implementation quirk). Prefer **`next`**/`prev` from API or rebuild query.

---

## API Details

### `GET` `/api/v1/technician-performance/summary`

#### Purpose

Returns **organization-wide** aggregates for **the current calendar month from the 1st through today**, across **all branches the caller can access**. Uses the same scoring pipeline as the dashboard (technicians in scope, metrics, optional scores).

#### Authorization

`TECHNICIAN_PERFORMANCE_READ` or `CEO`

#### Path parameters

None.

#### Query parameters

None.

#### Request Body

Not applicable.

#### Response

**200 OK** — `data`: `TechnicianPerformanceSummaryDto`

| Field                        | Type           | Description                              |
| ---------------------------- | -------------- | ---------------------------------------- |
| `totalTechnicians`           | int            | Count of technicians in scope with data  |
| `avgCompletionRatePercent`   | decimal        | Average completion % across rows         |
| `avgUtilizationRatePercent`  | decimal        | Average utilization %                    |
| `avgCustomerRating`          | decimal        | Average rating (users with ratings only) |
| `totalTasksCompletedPrimary` | long           | Sum of primary completed tasks           |
| `totalRevenue`               | decimal        | Sum of attributed revenue                |
| `avgReTaskRatePercent`       | decimal        | Average re-task rate %                   |
| `topPerformerName`           | string \| null | Display name                             |
| `topPerformerUserId`         | long \| null   | User id                                  |

#### Full response JSON example

```json
{
  "status": 200,
  "message": "Technician performance summary",
  "data": {
    "totalTechnicians": 12,
    "avgCompletionRatePercent": 87.5,
    "avgUtilizationRatePercent": 72.3,
    "avgCustomerRating": 4.2,
    "totalTasksCompletedPrimary": 340,
    "totalRevenue": 1250000.5,
    "avgReTaskRatePercent": 8.1,
    "topPerformerName": "Ravi Sharma",
    "topPerformerUserId": 42
  }
}
```

**Empty scope example**

```json
{
  "status": 200,
  "message": "Technician performance summary",
  "data": {
    "totalTechnicians": 0,
    "avgCompletionRatePercent": 0.0,
    "avgUtilizationRatePercent": 0.0,
    "avgCustomerRating": 0.0,
    "totalTasksCompletedPrimary": 0,
    "totalRevenue": 0.0,
    "avgReTaskRatePercent": 0.0,
    "topPerformerName": null,
    "topPerformerUserId": null
  }
}
```

#### Exceptions

| HTTP | When                      | Message                       |
| ---- | ------------------------- | ----------------------------- |
| 403  | ROOT user                 | `Not available for root user` |
| 403  | Unsupported JWT type      | `Unsupported user type`       |
| 401  | Tenant user missing in DB | `User not found`              |

#### Error JSON (403)

```json
{
  "status": 403,
  "error": "Forbidden",
  "message": "Not available for root user",
  "path": "/api/v1/technician-performance/summary"
}
```

_(Exact JSON may follow Spring ProblemDetails / `ValidationErrorResponse` depending on global handler configuration.)_

#### Frontend notes

- No period picker: always **month-to-date** for this endpoint.

#### cURL

```bash
curl -s "https://{host}/api/v1/technician-performance/summary" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}"
```

---

### `GET` `/api/v1/technician-performance/dashboard`

#### Purpose

Returns a **paginated**, **sorted** list of technicians with KPI columns and **rank** (1-based within the full filtered set). Date range is driven by **`period`** (default **`MONTH`**) and optional **`from`/`to`** for `CUSTOM_RANGE`.

#### Authorization

`TECHNICIAN_PERFORMANCE_READ` or `CEO`

#### Query parameters

| Field      | Type   | Required          | Default            | Description                                       |
| ---------- | ------ | ----------------- | ------------------ | ------------------------------------------------- |
| `period`   | enum   | No                | `MONTH`            | `WEEK`, `MONTH`, `QUARTER`, `CUSTOM_RANGE`        |
| `from`     | date   | If `CUSTOM_RANGE` | —                  | Inclusive start                                   |
| `to`       | date   | If `CUSTOM_RANGE` | —                  | Inclusive end                                     |
| `branchId` | string | No                | —                  | Filter to one branch (must be allowed)            |
| `roleId`   | long   | No                | —                  | Filter by user role id                            |
| `search`   | string | No                | —                  | `empId` or full name substring (case-insensitive) |
| `pageNo`   | int    | No                | `0`                | 0-based page index                                |
| `pageSize` | int    | No                | `20`               | Page size                                         |
| `sort`     | string | No                | `performanceScore` | See **Sort fields**                               |
| `dir`      | string | No                | `desc`             | `asc` or `desc`                                   |

**Sort fields** (`sort`) — aliases accepted:

| `sort` value                                      | Sorts by                                  |
| ------------------------------------------------- | ----------------------------------------- |
| `performanceScore` (default)                      | Weighted score; nulls last when ascending |
| `completionRate`, `completionRatePercent`         | Completion %                              |
| `utilizationRate`, `utilizationRatePercent`       | Utilization %                             |
| `revenue`, `revenueContributed`                   | Revenue                                   |
| `employeeName`                                    | Name                                      |
| `empId`                                           | Employee id                               |
| `tasksAssigned`                                   | Assigned count                            |
| `tasksCompleted`, `tasksCompletedPrimary`         | Primary completed                         |
| `materialEfficiency`, `materialEfficiencyPercent` | Material efficiency                       |
| `reTaskRate`, `reTaskRatePercent`                 | Re-task rate                              |

Secondary tie-breakers: completion rate, avg rating, re-task rate, name (see `metricComparator`).

#### Request Body

Not applicable.

#### Response

**200 OK** — `data`: `PaginationResponse<TechnicianPerformanceRowDto>`

**`TechnicianPerformanceRowDto`**

| Field                       | Type           |
| --------------------------- | -------------- |
| `rank`                      | int            |
| `userId`                    | long           |
| `employeeName`              | string         |
| `empId`                     | string         |
| `branchId`                  | string \| null |
| `branchName`                | string \| null |
| `roleName`                  | string \| null |
| `tasksAssigned`             | int            |
| `tasksCompletedPrimary`     | int            |
| `tasksPending`              | int            |
| `tasksOverdue`              | int            |
| `completionRatePercent`     | decimal        |
| `utilizationRatePercent`    | decimal        |
| `tasksPerDay`               | decimal        |
| `revenueContributed`        | decimal        |
| `avgCustomerRating`         | decimal        |
| `reTaskRatePercent`         | decimal        |
| `materialEfficiencyPercent` | decimal        |
| `performanceScore`          | int \| null    |
| `insufficientDataForScore`  | boolean        |
| `scoreGrade`                | string         |

#### Full response JSON example

```json
{
  "status": 200,
  "message": "Technician performance dashboard",
  "data": {
    "count": 25,
    "next": "https://api.example.com/api/v1/technician-performance/dashboard?pageSize=20&period=MONTH&sort=performanceScore&dir=desc&pageNo=1",
    "prev": null,
    "data": [
      {
        "rank": 1,
        "userId": 42,
        "employeeName": "Ravi Sharma",
        "empId": "EMP001",
        "branchId": "BR-MUM-01",
        "branchName": "Mumbai Central",
        "roleName": "Service Technician",
        "tasksAssigned": 28,
        "tasksCompletedPrimary": 24,
        "tasksPending": 2,
        "tasksOverdue": 0,
        "completionRatePercent": 85.71,
        "utilizationRatePercent": 78.5,
        "tasksPerDay": 1.2,
        "revenueContributed": 145000.0,
        "avgCustomerRating": 4.5,
        "reTaskRatePercent": 8.33,
        "materialEfficiencyPercent": 96.2,
        "performanceScore": 82,
        "insufficientDataForScore": false,
        "scoreGrade": "GOOD"
      }
    ]
  }
}
```

#### Exceptions

| HTTP | When                                              |
| ---- | ------------------------------------------------- |
| 400  | Invalid date range                                |
| 400  | `CUSTOM_RANGE` without `from`/`to`                |
| 400  | CEO with unknown `branchId` — `"Unknown branch"`  |
| 403  | Branch not allowed — `"No access to this branch"` |
| 403  | ROOT / unsupported type                           |

#### Error JSON (400 custom range)

```json
{
  "status": 400,
  "error": "Bad Request",
  "message": "Parameters 'from' and 'to' are required when period=CUSTOM_RANGE",
  "path": "/api/v1/technician-performance/dashboard"
}
```

#### Frontend notes

- **`search`** is passed to `findTechniciansForPerformance`; use debounce for typeahead.
- **`next`** URL includes `period` and `from`/`to` only when `period=CUSTOM_RANGE` (see `buildDashboardApiPath`).

#### cURL

```bash
curl -s -G "https://{host}/api/v1/technician-performance/dashboard" \
  --data-urlencode "period=MONTH" \
  --data-urlencode "branchId=BR-MUM-01" \
  --data-urlencode "pageNo=0" \
  --data-urlencode "pageSize=20" \
  --data-urlencode "sort=performanceScore" \
  --data-urlencode "dir=desc" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}"
```

**Custom range**

```bash
curl -s -G "https://{host}/api/v1/technician-performance/dashboard" \
  --data-urlencode "period=CUSTOM_RANGE" \
  --data-urlencode "from=2026-01-01" \
  --data-urlencode "to=2026-03-31" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}"
```

---

### `GET` `/api/v1/technician-performance/employee`

#### Purpose

**Lifetime** (or from **date of joining** if set) through **today** performance profile for one technician, including nested DTOs and **score factor breakdown** (weighted components).

#### Authorization

`TECHNICIAN_PERFORMANCE_READ` or `CEO`

#### Query parameters

| Field    | Type | Required |
| -------- | ---- | -------- |
| `userId` | long | Yes      |

#### Request Body

Not applicable.

#### Response

**200 OK** — `data`: `TechnicianPerformanceDetailDto`

Top-level fields:

| Field                              | Type                                     |
| ---------------------------------- | ---------------------------------------- |
| `from`                             | date — start of window (DOJ or fallback) |
| `to`                               | date — typically today                   |
| `employee`                         | `EmployeeInfoDto`                        |
| `kpi`                              | `KpiCardsDto`                            |
| `taskBreakdown`                    | `TaskBreakdownDto`                       |
| `attendance`                       | `AttendanceAnalysisDto`                  |
| `revenue`                          | `RevenueContributionDto`                 |
| `materialRows`                     | array of `MaterialEfficiencyRowDto`      |
| `overallMaterialEfficiencyPercent` | decimal                                  |
| `customerSatisfaction`             | `CustomerSatisfactionDto`                |
| `reTaskSection`                    | `ReTaskSectionDto`                       |
| `scoreBreakdown`                   | `ScoreFactorDto[]`                       |
| `totalPerformanceScore`            | int \| null                              |
| `scoreGrade`                       | string                                   |
| `insufficientDataForScore`         | boolean                                  |

**`ScoreFactorDto`** (from `PerformanceScoreCalculator.scoreFactors`): `factorName`, `weight` (e.g. `"30%"`), `rawValue`, `weightedScore`.

Factor names: **Task Completion Rate**, **Time Utilization Rate**, **Customer Satisfaction**, **Revenue Contribution**, **Material Efficiency**, **Re-Task Penalty (Inv.)**.

#### Full response JSON example (abbreviated)

```json
{
  "status": 200,
  "message": "Technician performance detail",
  "data": {
    "from": "2024-06-01",
    "to": "2026-04-13",
    "employee": {
      "userId": 42,
      "fullName": "Ravi Sharma",
      "empId": "EMP001",
      "roleName": "Service Technician",
      "branchId": "BR-MUM-01",
      "branchName": "Mumbai Central",
      "phone": "+919876543210",
      "email": "ravi@example.com",
      "dateOfJoining": "2024-06-01",
      "status": "ACTIVE",
      "profileImageUrl": null
    },
    "kpi": {
      "overallScore": 82,
      "overallGrade": "GOOD",
      "completionRatePercent": 85.71,
      "utilizationRatePercent": 78.5,
      "tasksPerDay": 1.2,
      "avgCustomerRating": 4.5,
      "revenueContribution": 145000.0,
      "revenueTierLabel": "Top 25%",
      "materialEfficiencyPercent": 96.2,
      "reTaskRatePercent": 8.33
    },
    "taskBreakdown": {
      "totalAssigned": 120,
      "completed": 95,
      "pending": 10,
      "overdue": 2,
      "normalTasks": 100,
      "reTasksAsPrimary": 20,
      "onTimeRatePercent": 88.0
    },
    "attendance": {
      "workingDays": 220,
      "presentDays": 200,
      "leaveDays": 10,
      "absentDays": 5,
      "weekOffDays": 0,
      "lateArrivals": 3,
      "attendanceRatePercent": 95.0,
      "totalWorkingHours": 1600.0,
      "taskExecutionHours": 1200.0,
      "avgHoursPerDay": 8.0,
      "avgTaskDurationHours": 2.5,
      "travelTimeEstimatedHours": 400.0,
      "utilizationRatePercent": 75.0,
      "metricsNotApplicable": false,
      "metricsNotApplicableReason": null
    },
    "revenue": {
      "totalRevenue": 145000.0,
      "asPrimary": 120000.0,
      "primaryTaskCount": 95,
      "asSupport": 25000.0,
      "supportTaskCount": 15,
      "avgRevenuePerTask": 1526.32
    },
    "materialRows": [],
    "overallMaterialEfficiencyPercent": 96.2,
    "customerSatisfaction": {
      "averageRating": 4.5,
      "totalFeedbackCount": 40,
      "star5": 25,
      "star4": 10,
      "star3": 3,
      "star2": 1,
      "star1": 1,
      "recentFeedback": []
    },
    "reTaskSection": {
      "reTasksAttributed": 2,
      "reTaskRatePercent": 2.1,
      "details": []
    },
    "scoreBreakdown": [
      {
        "factorName": "Task Completion Rate",
        "weight": "30%",
        "rawValue": "85.71%",
        "weightedScore": "25.7 / 30"
      }
    ],
    "totalPerformanceScore": 82,
    "scoreGrade": "GOOD",
    "insufficientDataForScore": false
  }
}
```

#### Exceptions

| HTTP | Message                                |
| ---- | -------------------------------------- |
| 404  | `No accessible branches`               |
| 404  | `Employee not found`                   |
| 404  | `Not a field technician profile`       |
| 404  | `Employee not in cohort`               |
| 403  | `Employee outside accessible branches` |

#### cURL

```bash
curl -s -G "https://{host}/api/v1/technician-performance/employee" \
  --data-urlencode "userId=42" \
  -H "Authorization: Bearer {token}" \
  -H "X-Tenant-ID: {tenant}"
```

---

## Validation and Exception Summary

| Scenario                            | HTTP | Frontend                         |
| ----------------------------------- | ---- | -------------------------------- |
| `period=CUSTOM_RANGE` without dates | 400  | Require both date pickers        |
| `from` after `to`                   | 400  | Validate range                   |
| ROOT user                           | 403  | Hide module or message           |
| Unknown branch (CEO)                | 400  | Branch dropdown from tenant list |
| Branch not in scope                 | 403  | Disable branch                   |
| Not a technician profile            | 404  | Only pick app users              |
| Insufficient tasks for score        | 200  | Show `insufficientDataForScore`  |

---

## Frontend Integration Notes

- **Summary** = fixed **MTD**; **Dashboard** = selectable period.
- **Rank** is computed on the **full** filtered/sorted list, then sliced for the page.
- **Revenue** uses configurable primary/support shares (`primaryRevenueShare` / `supportRevenueShare`).
- **Attendance block** may set `metricsNotApplicable: true` with reason _"No attendance or on leave entire period"_ when applicable.
- **Re-task details** list re-tasks where the original primary on the ticket matches this user.
- Configure **defaults** via `app.technician-performance` in `application.yml` (weights, min tasks, on-time buffer, re-task penalty).

---

## Assumptions / configuration (`app.technician-performance`)

Default values from `TechnicianPerformanceProperties`:

| Property                        | Default |
| ------------------------------- | ------- |
| `primary-revenue-share`         | 0.6     |
| `support-revenue-share`         | 0.4     |
| `min-completed-tasks-for-score` | 5       |
| `re-task-full-penalty-percent`  | 20      |
| `on-time-buffer-minutes`        | 30      |
| `weight-completion`             | 30      |
| `weight-satisfaction`           | 25      |
| `weight-utilization`            | 15      |
| `weight-revenue`                | 10      |
| `weight-material`               | 10      |
| `weight-retask`                 | 10      |

Mobile shift (`app.mobile.attendance.shift-start`, `late-grace-minutes`) affects late-arrival counts in metrics.

---

_Generated from `TechnicianPerformanceController`, `TechnicianPerformanceServiceImpl`, `TechnicianPerformancePeriodResolver`, DTOs, `PerformanceScoreCalculator`, `TechnicianPerformanceProperties`._
