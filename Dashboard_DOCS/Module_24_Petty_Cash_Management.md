# 📊 Module 24: Petty Cash Management Dashboard

The Petty Cash Management Dashboard provides complete visibility into petty cash requests, approvals, payments, and branch-wise expenses.

---

# 1. KPIs (4)

## KPI 1: Total Petty Cash Requested

### Meaning
Shows total amount requested by employees.

### Source Table
petty_cash_requests

### SQL
```
SELECT 
SUM(amount) AS total_requested_amount
FROM petty_cash_requests;
````
---

## KPI 2: Total Approved Amount

### Meaning
Shows total approved petty cash.

### Source Table
petty_cash_requests

### SQL
```
SELECT 
SUM(approved_amount) AS total_approved_amount
FROM petty_cash_requests
WHERE status = 'APPROVED';
```
---

## KPI 3: Pending Requests

### Meaning
Shows number of petty cash requests waiting for approval.

### Source Table
petty_cash_requests

### SQL
```
SELECT 
COUNT(*) AS pending_requests
FROM petty_cash_requests
WHERE status = 'PENDING';
```
---

## KPI 4: Paid Requests

### Meaning
Shows total number of paid petty cash requests.

### Source Table
petty_cash_requests

### SQL
```
SELECT 
COUNT(*) AS paid_requests
FROM petty_cash_requests
WHERE status = 'PAID';
```
---

# 2. Charts (3)

## Chart 1: Petty Cash Status Distribution

### Meaning
Shows how many requests are in each status.

### Source Table
petty_cash_requests

### Chart Type
Donut Chart

### SQL
```
SELECT 
status,
COUNT(*) AS total_requests
FROM petty_cash_requests
GROUP BY status;
```
### Axes

X-axis → status  
Y-axis → total_requests  

---

## Chart 2: Branch Wise Petty Cash Expenses

### Meaning
Shows which branch is spending more petty cash.

### Source Tables

petty_cash_requests  
branches  

### Chart Type
Bar Chart

### SQL
```
SELECT 
b.branch_name,
SUM(pc.approved_amount) AS total_expense
FROM petty_cash_requests pc
JOIN branches b
ON pc.requester_branch_id = b.id
GROUP BY b.branch_name
ORDER BY total_expense DESC;
```
### Axes

X-axis → branch_name  
Y-axis → total_expense  

---

## Chart 3: Monthly Petty Cash Requests

### Meaning
Shows petty cash request trend over time.

### Source Table
petty_cash_requests

### Chart Type
Line Chart

### SQL
```
SELECT 
DATE(submitted_at) AS request_date,
COUNT(*) AS total_requests
FROM petty_cash_requests
GROUP BY DATE(submitted_at)
ORDER BY request_date;
```
### Axes

X-axis → request_date  
Y-axis → total_requests  

---

# 3. Tables (2)

## Table 1: Recent Petty Cash Requests

### Meaning
Shows latest petty cash requests with employee and branch details.

### Tables Used

petty_cash_requests  
users  
branches  

### SQL
```
SELECT 
pc.id AS request_id,
u.first_name,
b.branch_name,
pc.category,
pc.amount,
pc.approved_amount,
pc.status,
pc.submitted_at
FROM petty_cash_requests pc
JOIN users u
ON pc.requester_user_id = u.id
JOIN branches b
ON pc.requester_branch_id = b.id
ORDER BY pc.submitted_at DESC
LIMIT 20;
```
### Table Headers

| Request ID | Employee Name | Branch | Category | Requested Amount | Approved Amount | Status | Submitted Date |

---

## Table 2: Approved Petty Cash Payments

### Meaning
Shows approved and paid petty cash with payment details.

### Tables Used

petty_cash_requests  
users  

### SQL
```
SELECT 
pc.id AS request_id,
u.first_name,
pc.category,
pc.approved_amount,
pc.payment_mode_processed,
pc.transaction_ref,
pc.payment_date,
pc.status
FROM petty_cash_requests pc
JOIN users u
ON pc.paid_by_user_id = u.id
WHERE pc.status = 'PAID'
ORDER BY pc.payment_date DESC;
```
### Table Headers

| Request ID | Paid By | Category | Approved Amount | Payment Mode | Transaction Reference | Payment Date | Status |

---

# 4. Alerts (2)

## Alert 1: High Amount Request Alert

### Meaning
Detects high-value petty cash requests.

### Source Table
petty_cash_requests

### SQL
```
SELECT 
id,
category,
amount,
status,
submitted_at
FROM petty_cash_requests
WHERE amount > 10000
ORDER BY amount DESC;
```
### Trigger Condition

amount > 10000

---

## Alert 2: Pending Approval Alert

### Meaning
Shows petty cash requests pending for approval for long time.

### Source Table
petty_cash_requests

### SQL
```
SELECT 
id,
category,
amount,
status,
submitted_at
FROM petty_cash_requests
WHERE status = 'PENDING'
AND submitted_at < CURRENT_DATE - INTERVAL '3 days'
ORDER BY submitted_at;
```
### Trigger Condition

status = PENDING  
request older than 3 days

---
## 5. Global Filters for KPIs and Charts and tables

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


# ✅ Summary

## Tables Used

- petty_cash_requests
- users
- branches

## KPIs

1. Total Petty Cash Requested  
2. Total Approved Amount  
3. Pending Requests  
4. Paid Requests  

## Charts

1. Status Distribution  
2. Branch Wise Expenses  
3. Monthly Requests  

## Tables

1. Recent Petty Cash Requests  
2. Approved Petty Cash Payments  

## Alerts

1. High Amount Request Alert  
2. Pending Approval Alert


# 🔐 Module 24: Petty Cash Dashboard Permission Visibility Note
## Module Dependency

## The Petty Cash Dashboard depends on:

- Branch Module
- Users Module
- Reason
- Petty cash transactions are recorded branch-wise
- Petty cash entries are created by users
- Dashboard shows branch-wise cash inflow and outflow
- Dashboard shows user-based transaction activity
- Without branch and user access, petty cash data cannot be displayed properly
- Permission Logic

## Case 1
```
User has:

Petty Cash Module Permission
Branch Module Permission
Users Module Permission
Result

Petty Cash Dashboard will be VISIBLE

Reason
User can see petty cash transactions
User can see branch-wise cash records
User can see user-based transaction data
Dashboard data will load correctly
```
## Case 2
```
User has:

Petty Cash Module Permission
BUT does not have Branch Module Permission
OR does not have Users Module Permission
Result

Petty Cash Dashboard will be NOT VISIBLE

Reason
Petty cash is managed branch-wise
Transactions are created by users
Dashboard requires branch and user data
Without dependency modules, dashboard data will be incomplete
Message

"You don't have permission to see the dashboard."
```

## Permission Matrix
| Petty Cash Module | Branch Module | Users Module | Dashboard     |
| ----------------- | ------------- | ------------ | ------------- |
| ✅                 | ✅             | ✅            | ✅ Visible     |
| ✅                 | ❌             | ✅            | ❌ Not Visible |
| ✅                 | ✅             | ❌            | ❌ Not Visible |
| ❌                 | ❌             | ❌            | ❌ Not Visible |
