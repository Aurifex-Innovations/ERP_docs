# 📊 Module 21: Task Management Dashboard

The Task Management Dashboard helps managers track technician workload, task progress, overdue services, and daily execution performance.

---

# 1. KPIs (4)

## 1. Total Tasks

### Meaning
Shows total number of tasks created.

### Table
tasks

### SQL

```sql
SELECT COUNT(id) AS total_tasks
FROM tasks
WHERE is_deleted = FALSE;
```

## 2. Completed Tasks
**Meaning**

Shows how many tasks are finished.

**Table**

tasks

**SQL**
```
SELECT COUNT(id) AS completed_tasks
FROM tasks
WHERE status = 'Completed'
AND is_deleted = FALSE;
```

## 3. Pending Tasks
**Meaning**

Shows tasks that are not started yet.

**Table**

tasks

**SQL**
```
SELECT COUNT(id) AS pending_tasks
FROM tasks
WHERE status = 'Pending'
AND is_deleted = FALSE;
```

## 4. Overdue Tasks
**Meaning**

Shows tasks whose scheduled date has passed and not completed.

**Table**

tasks

**SQL**
```
SELECT COUNT(id) AS overdue_tasks
FROM tasks
WHERE scheduled_date < CURRENT_DATE
AND status != 'Completed'
AND is_deleted = FALSE;
```

# 2. Charts (3)
## 1. Task Status Chart
**Meaning**

Shows tasks by status.

**Tables**

tasks

**SQL**
```
SELECT 
status,
COUNT(id) AS total_tasks
FROM tasks
WHERE is_deleted = FALSE
GROUP BY status;
```

**Chart Type**
DountChart


## 2. Monthly Task Trend
**Meaning**

Shows number of tasks created every month.

**Tables**

tasks

**Chart Type**

Line Chart

**SQL**
```
SELECT 
DATE_TRUNC('month', created_at) AS month,
COUNT(id) AS total_tasks
FROM tasks
WHERE is_deleted = FALSE
GROUP BY month
ORDER BY month;
```

## 3. Technician Workload Chart
**Meaning**

Shows number of tasks assigned to each technician.

**Tables**

- tasks
- employees

**Chart Type**
Bar Chart

**SQL**
```
SELECT 
e.employee_name,
COUNT(t.id) AS total_tasks
FROM tasks t
JOIN employees e
ON t.primary_technician_id = e.id
WHERE t.is_deleted = FALSE
GROUP BY e.employee_name
ORDER BY total_tasks DESC;
```

# 3. Tables (2)
## Table 1: Recent Tasks
**Meaning**

Shows latest created tasks.

**Tables**

- tasks
- customers
- sales_order

**SQL**
```
SELECT 
t.task_number,
c.customer_name,
s.so_number,
t.service_name,
t.scheduled_date,
t.start_time,
t.end_time,
t.status
FROM tasks t
JOIN customers c
ON t.customer_id = c.id
LEFT JOIN sales_order s
ON t.sales_order_id = s.id
WHERE t.is_deleted = FALSE
ORDER BY t.created_at DESC
LIMIT 20;
```

**Table Headers**
- | Task Number | Customer | SO Number | Service | Scheduled Date | Start Time | End Time | Status |


## Table 2: Task Material Usage
**Meaning**

Shows materials used in tasks.

**Tables**

- task_materials
- tasks
- inventory_products

**SQL**
```
SELECT 
t.task_number,
p.product_name,
m.uom,
m.required_qty,
m.used_qty,
t.scheduled_date
FROM task_materials m
JOIN tasks t
ON m.task_id = t.id
JOIN inventory_products p
ON m.product_id = p.id
WHERE t.is_deleted = FALSE;
```

**Table Headers**
- | Task Number | Product Name | UOM | Required Qty | Used Qty | Scheduled Date |


# 4. Alerts (2)
## 1. Overdue Task Alert
**Meaning**

Shows tasks that are overdue.

**Tables**

tasks

**SQL**
```
SELECT 
task_number,
customer_id,
service_name,
scheduled_date,
status
FROM tasks
WHERE scheduled_date < CURRENT_DATE
AND status != 'Completed'
AND is_deleted = FALSE;
```

**Purpose**
- Detect delayed tasks
- Help manager take action


## 2. Technician Overload Alert
**Meaning**

Shows technicians assigned too many tasks in one day.

**Tables**

- tasks
- employees

**SQL**
```
SELECT 
e.employee_name,
t.scheduled_date,
COUNT(t.id) AS total_tasks
FROM tasks t
JOIN employees e
ON t.primary_technician_id = e.id
WHERE t.is_deleted = FALSE
GROUP BY e.employee_name, t.scheduled_date
HAVING COUNT(t.id) > 5;
```

**Purpose**
- Avoid technician overload
- Balance workload

## 5. Global Filters for KPIs and Charts

To provide dynamic data views, the dashboard uses three global filters. If a user selects these filters, the API dynamically applies them. If the user clears the filters or makes no selection, the original base query runs.

### 1. Global Filters Definition

1. **Date Range Filter**: Custom date selection mapping to `start_date` and `end_date`.
2. **Time Period Filter**: A preset dropdown mapping to date ranges on the backend:
   - **Current**: `CURRENT_DATE`
   - **7 Days**: `CURRENT_DATE - INTERVAL '7 days'` to `CURRENT_DATE`
   - **1 Month**: `CURRENT_DATE - INTERVAL '1 month'` to `CURRENT_DATE`
   - **3 Months**: `CURRENT_DATE - INTERVAL '3 months'` to `CURRENT_DATE`
   - **6 Months**: `CURRENT_DATE - INTERVAL '6 months'` to `CURRENT_DATE`
   - **1 Year**: `CURRENT_DATE - INTERVAL '1 year'` to `CURRENT_DATE`


# 🔐 Task Management Dashboard Permission Note

## Module Dependency

The Task Management Dashboard depends on:

- Sales Order Module
- Contract Module
- Customer Module
- GMA Module
- Employee Module
- Stock Module

### Reason

- Tasks are created from Sales Orders and Contracts
- Customer details are required to show service location
- GMA provides service materials and chemicals
- Employee module provides technician assignment
- Stock module provides material usage and deduction
- Dashboard shows task data based on these modules
- If user cannot access these modules, dashboard will not show correct data

---

# Permission Logic

## Case 1

User has:

- Task Module Permission
- Sales Order Module Permission
- Contract Module Permission
- Customer Module Permission
- GMA Module Permission
- Employee Module Permission
- Stock Module Permission

### Result

Task Dashboard will be **VISIBLE**

### Reason

- User can see tasks
- User can see service source (SO/Contract)
- User can see customer data
- User can see technician
- User can see materials and stock usage
- Dashboard data will load properly

---

## Case 2

User has:

- Task Module Permission
- BUT missing any one of these:
  - Sales Order Module Permission
  - Contract Module Permission
  - Customer Module Permission
  - GMA Module Permission
  - Employee Module Permission
  - Stock Module Permission

### Result

Task Dashboard will be **NOT VISIBLE**

### Reason

- Task data depends on other modules
- User cannot access source data
- Dashboard will show incomplete information
- System will block dashboard

### Message

"You don't have permission to see the dashboard."

---

# Permission Matrix

| Task Module | Sales Order | Contract | Customer | GMA | Employee | Stock | Dashboard |
|------------|------------|---------|---------|-----|---------|------|----------|
| ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ Visible |
| ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ Not Visible |
| ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ Not Visible |
| ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ Not Visible |
| ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ Not Visible |
| ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ Not Visible |
| ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ Not Visible |
| ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ Not Visible |

---

# Backend Logic

IF task_module_view = TRUE  
AND sales_order_module_view = TRUE  
AND contract_module_view = TRUE  
AND customer_module_view = TRUE  
AND gma_module_view = TRUE  
AND employee_module_view = TRUE  
AND stock_module_view = TRUE  

THEN  
    Show Dashboard  

ELSE  
    Hide Dashboard  
    Show Message: "You don't have permission to see the dashboard."

---

# Final Rule

Task Management Dashboard will only be visible when user has:

- Task Module Permission
- Sales Order Module Permission
- Contract Module Permission
- Customer Module Permission
- GMA Module Permission
- Employee Module Permission
- Stock Module Permission

Otherwise dashboard will be hidden.
