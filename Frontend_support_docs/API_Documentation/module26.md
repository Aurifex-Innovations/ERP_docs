# Module 26: Technician Performance & Productivity

## Table of Contents

- [26.1 Performance Dashboard — Employee Table View](#261-performance-dashboard--employee-table-view)
  - [ORG Summary API](#org-summary-api)
  - [Dashboard API](#dashboard-api)
- [26.2 Individual Employee Performance View](#262-individual-employee-performance-view)

---

## 26.1 Performance Dashboard — Employee Table View

### ORG Summary API

#### Endpoint

```
GET {{baseUrl}}/api/v1/technician-performance/summary
```

#### Description

Provides organization-wide performance summary metrics.

---

### Dashboard API

#### Endpoint

```
GET {{baseUrl}}/api/v1/technician-performance/dashboard
```

#### Query Parameters

| Parameter | Type    | Example          | Description                                |
| --------- | ------- | ---------------- | ------------------------------------------ |
| period    | string  | MONTH            | Time period (MONTH, QUARTER, YEAR, CUSTOM) |
| from      | date    | 2026-01-01       | Start date filter (YYYY-MM-DD)             |
| to        | date    | 2026-06-30       | End date filter (YYYY-MM-DD)               |
| branchId  | string  | {{branchId}}     | Branch ID filter                           |
| roleId    | string  | {{roleId}}       | Role ID filter                             |
| search    | string  | {{search}}       | Search query for employee name/ID          |
| pageNo    | integer | 0                | Page number                                |
| pageSize  | integer | 20               | Number of items per page                   |
| sort      | string  | performanceScore | Sort field                                 |
| dir       | string  | desc             | Sort direction (asc, desc)                 |

---

## 26.2 Individual Employee Performance View

### Endpoint

```
GET {{baseUrl}}/api/v1/technician-performance/employee
```

### Query Parameters

| Parameter | Type   | Example    | Description              |
| --------- | ------ | ---------- | ------------------------ |
| userId    | string | {{userId}} | Employee/User identifier |

---

## Query Parameter Details

### Period Options

| Period  | Description                           |
| ------- | ------------------------------------- |
| MONTH   | Monthly performance data              |
| QUARTER | Quarterly performance data            |
| YEAR    | Yearly performance data               |
| CUSTOM  | Custom date range (use from/to dates) |

### Sort Fields

Common sortable fields may include:

| Field            | Description               |
| ---------------- | ------------------------- |
| performanceScore | Overall performance score |
| tasksCompleted   | Number of completed tasks |
| onTimeCompletion | On-time completion rate   |
| customerRating   | Average customer rating   |
| productivity     | Productivity metrics      |
| efficiency       | Efficiency metrics        |

### Sort Direction

| Direction | Description                          |
| --------- | ------------------------------------ |
| asc       | Ascending order (lowest to highest)  |
| desc      | Descending order (highest to lowest) |

---

## Response Data Overview

### ORG Summary Response

Typically includes:

- Total technicians count
- Average performance score
- Total tasks completed
- Overall productivity metrics
- Top performers summary
- Department/branch-wise breakdown

### Dashboard Response

Typically includes paginated list with:

- Employee ID and name
- Performance score
- Tasks assigned vs completed
- On-time completion rate
- Customer ratings
- Productivity metrics
- Efficiency indicators
- Recent activity summary

### Individual Employee Response

Typically includes detailed view with:

- Employee profile information
- Performance score breakdown
- Task completion statistics
- Time period comparisons
- Customer feedback summary
- Skill ratings
- Attendance and punctuality
- Quality metrics
- Historical trends
- Peer comparison

---

## Performance Metrics Guide

### Key Performance Indicators (KPIs)

#### Performance Score

Overall performance rating calculated from multiple factors:

- Task completion rate
- On-time delivery
- Customer satisfaction
- Quality of work
- Attendance

#### Productivity Metrics

- Tasks completed per day/week/month
- Average time per task
- Resource utilization
- Downtime analysis

#### Efficiency Metrics

- First-time fix rate
- Rework percentage
- Material usage efficiency
- Travel time optimization

#### Quality Metrics

- Customer ratings average
- Complaint rate
- Quality inspection scores
- Compliance adherence

---

## Use Cases

### Dashboard View (26.1)

**Purpose:** Monitor team performance across the organization

**Common Filters:**

- View by branch for regional performance
- Filter by role to compare technician levels
- Sort by performance score to identify top/bottom performers
- Custom date ranges for period-specific analysis

**Actions:**

- Export performance reports
- Drill down to individual employee details
- Identify training needs
- Recognize top performers

### Individual View (26.2)

**Purpose:** Deep dive into single employee performance

**Use Cases:**

- Performance reviews
- Training needs assessment
- Promotion decisions
- Issue investigation
- Goal setting and tracking

**Typical Data Points:**

- 360-degree performance view
- Trend analysis over time
- Comparison with team averages
- Detailed task breakdown
- Customer feedback details

---

## Filtering Best Practices

### Time Period Selection

- **MONTH** - For regular monthly reviews
- **QUARTER** - For quarterly performance assessments
- **YEAR** - For annual reviews and year-end evaluations
- **CUSTOM** - For specific campaign periods or project durations

### Branch/Role Filtering

- Use `branchId` to compare performance across locations
- Use `roleId` to benchmark within same role level
- Combine both for detailed segmentation

### Search Functionality

- Search by employee name for quick lookup
- Search by employee ID for precise matching
- Partial name matching supported

---

## Pagination Guidelines

### Recommended Page Sizes

| Use Case         | Recommended pageSize |
| ---------------- | -------------------- |
| Quick overview   | 10-20                |
| Detailed review  | 20-50                |
| Export/reporting | 100+                 |

### Performance Considerations

- Larger page sizes may impact load times
- Use pagination for better user experience
- Consider total record count when setting page size

---

## Integration Points

### Related Modules

- **Module 21** - Task Management (task completion data)
- **Module 23** - Customer Support (customer feedback)
- **Module 20** - Sales Orders (service delivery data)
- **Module 19** - Contracts (assigned contracts)

### Data Sources

Performance metrics are typically calculated from:

- Task completion records
- Customer ratings and feedback
- Attendance and time tracking
- Quality inspection results
- Sales order execution data
- Support ticket resolutions

---

**End of Documentation**
