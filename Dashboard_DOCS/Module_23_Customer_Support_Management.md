# 📊 Module 23: Customer Support Management Dashboard

The Customer Support Dashboard provides complete visibility into support operations, ticket handling, and service performance.

---

# 1. Key Performance Indicators (4)

## KPI 1: Total Support Tickets

### Meaning
Shows total number of support tickets created.

### Source Table
support_tickets

### Calculation

```sql
SELECT COUNT(id) AS total_support_tickets
FROM support_tickets;
```

## KPI 2: Open Support Tickets
**Meaning**

Shows number of tickets that are still open.

**Source Table**

support_tickets

**Calculation**
```
SELECT COUNT(id) AS open_support_tickets
FROM support_tickets
WHERE status = 'Open';
```

## KPI 3: Closed Support Tickets
**Meaning**

Shows number of resolved support tickets.

**Source Table**

support_tickets

**Calculation**
```
SELECT COUNT(id) AS closed_support_tickets
FROM support_tickets
WHERE status = 'Closed';
```

## KPI 4: High Priority Tickets
**Meaning**

Shows urgent support tickets.

**Source Table**

support_tickets

**Calculation**
```
SELECT COUNT(id) AS high_priority_tickets
FROM support_tickets
WHERE priority = 'High';
```

# 2. Charts (3)
## Chart 1: Ticket Status Distribution
**Meaning**

Shows ticket status distribution.

**Chart Type**

Donut Chart

**Tables**

support_tickets

**SQL**
```
SELECT 
status,
COUNT(id) AS total_tickets
FROM support_tickets
GROUP BY status;
```

## Chart 2: Tickets Created Daily
**Meaning**

Shows daily support ticket creation.

**Chart Type**

Line Chart

**Tables**

support_tickets

**SQL**
```
SELECT 
DATE(created_at) AS ticket_date,
COUNT(id) AS total_tickets
FROM support_tickets
GROUP BY DATE(created_at)
ORDER BY ticket_date;
```

## Chart 3: Priority Wise Tickets
**Meaning**

Shows priority distribution.

**Chart Type**

Bar Chart

**Tables**

support_tickets

**SQL**
```
SELECT 
priority,
COUNT(id) AS total_tickets
FROM support_tickets
GROUP BY priority;
```

# 3. Tables (2)
## Table 1: Recent Support Tickets
**Meaning**

Shows latest support tickets.

**Tables**

- support_tickets
- customers
- branches

**SQL**
```
SELECT 
st.ticket_number,
c.customer_name,
st.issue_type,
st.priority,
st.status,
st.created_at,
b.branch_name
FROM support_tickets st
JOIN customers c
ON st.customer_id = c.id
JOIN branches b
ON st.branch_id = b.id
ORDER BY st.created_at DESC
LIMIT 20;
```

**Table Headers**

- | Ticket Number | Customer Name | Issue Type | Priority | Status | Created Date | Branch Name |


## Table 2: Open High Priority Tickets
**Meaning**

Shows urgent open support tickets.

**Tables**

- support_tickets
- customers

**SQL**
```
SELECT 
st.ticket_number,
c.customer_name,
st.issue_type,
st.priority,
st.status,
st.created_at
FROM support_tickets st
JOIN customers c
ON st.customer_id = c.id
WHERE st.priority = 'High'
AND st.status = 'Open'
ORDER BY st.created_at DESC;
```

**Table Headers**

- | Ticket Number | Customer Name | Issue Type | Priority | Status | Created Date |


# 4. Alerts (2)
## Alert 1: High Priority Open Ticket Alert
**Meaning**

Shows urgent open tickets.

**Tables**

support_tickets

**SQL**
```
SELECT 
ticket_number,
issue_type,
priority,
status,
created_at
FROM support_tickets
WHERE priority = 'High'
AND status = 'Open';
```

## Alert 2: Old Open Ticket Alert
**Meaning**

Shows tickets open for more than 3 days.

**Tables**

support_tickets

**SQL**
```
SELECT 
ticket_number,
priority,
status,
created_at
FROM support_tickets
WHERE status = 'Open'
AND created_at < CURRENT_DATE - INTERVAL '3 days';
```
## 5. Global Filters for KPIs and Charts

To provide dynamic data views, the dashboard uses three global filters. If a user selects these filters, the API dynamically applies them. If the user clears the filters or makes no selection, the original base query runs.

### 1. Global Filters Definition

1. **Branch Filter**: Filter specific data by selecting a `branch_id`.
2. **Date Range Filter**: Custom date selection mapping to `start_date` and `end_date`.
3. **Time Period Filter**: A preset dropdown mapping to date ranges on the backend:
   - **Current**: `CURRENT_DATE`
   - **7 Days**: `CURRENT_DATE - INTERVAL '7 days'` to `CURRENT_DATE`
   - **1 Month**: `CURRENT_DATE - INTERVAL '1 month'` to `CURRENT_DATE`
   - **3 Months**: `CURRENT_DATE - INTERVAL '3 months'` to `CURRENT_DATE`
   - **6 Months**: `CURRENT_DATE - INTERVAL '6 months'` to `CURRENT_DATE`
   - **1 Year**: `CURRENT_DATE - INTERVAL '1 year'` to `CURRENT_DATE`

# 🔐 Customer Support Dashboard Permission Note

## Module Dependency

The Customer Support Dashboard depends on:

- Customer Support Module
- Customer Module
- HRM (Users)
- Branch Module

Reason:

- Support tickets are created for customers
- Customer information is shown in dashboard
- Users handle support tickets
- Branch information is used in reporting
- Without customer or user data, dashboard cannot show correct information

---

# Permission Logic

## Case 1

User has:

- Customer Support Module Permission
- Customer Module Permission
- HRM (Users) Permission
- Branch Permission

### Result

Customer Support Dashboard will be **VISIBLE**

### Reason

- User can see support tickets
- User can see customer information
- User can see employee handling tickets
- Dashboard data loads properly

---

## Case 2

User has:

- Customer Support Module Permission
- BUT does not have Customer Module Permission
- OR does not have HRM (Users) Permission
- OR does not have Branch Permission

### Result

Customer Support Dashboard will be **NOT VISIBLE**

### Reason

- Support tickets depend on customer and user data
- Dashboard cannot show proper data
- Source data will be incomplete

### Message

"You don't have permission to see the dashboard."

---

# Permission Matrix

| Support Module | Customer Module | HRM (Users) | Branch | Dashboard |
|---------------|----------------|------------|-------|----------|
| ✅ | ✅ | ✅ | ✅ | ✅ Visible |
| ✅ | ❌ | ✅ | ✅ | ❌ Not Visible |
| ✅ | ✅ | ❌ | ✅ | ❌ Not Visible |
| ✅ | ✅ | ✅ | ❌ | ❌ Not Visible |
| ❌ | ❌ | ❌ | ❌ | ❌ Not Visible |

---

# Backend Logic

IF support_module_view = TRUE  
AND customer_module_view = TRUE  
AND users_module_view = TRUE  
AND branch_module_view = TRUE  

THEN  
    Show Dashboard  

ELSE  
    Hide Dashboard  
    Show Message: "You don't have permission to see the dashboard."

---

# Final Rule

Customer Support Dashboard will only be visible when user has:

- Customer Support Module Permission
- Customer Module Permission
- HRM (Users) Permission
- Branch Permission

Otherwise dashboard will be hidden.
