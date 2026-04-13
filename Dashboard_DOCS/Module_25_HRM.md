# 📊 MODULE 25: HRM DASHBOARD

## 1. Key Performance Indicators (KPIs)

### 1. Total Employees

#### Tables Used: users
#### SQL
```
SELECT COUNT(*) AS total_employees
FROM users;
```

### 2. Active Employees

#### Tables Used: users
#### SQL
```
SELECT COUNT(*) AS active_employees
FROM users
WHERE status = 'Active';
```

### 3. Inactive Employees

#### Tables Used: users
#### SQL
```
SELECT COUNT(*) AS inactive_employees
FROM users
WHERE status = 'Inactive';
```


### 4. Total Salary Paid (This Month)

#### Tables Used: hrm_salary_month
#### SQL
```
SELECT SUM(total_salary) AS total_salary_paid
FROM hrm_salary_month
WHERE salary_month = DATE_FORMAT(CURRENT_DATE, '%Y-%m');
```

### 5. Total Leaves Taken

#### Tables Used: hrm_holidays
#### SQL
```
SELECT COUNT(*) AS total_leaves
FROM hrm_leaves;
```

### 6. Pending Salary Count

#### Tables Used: hrm_salary_slip
#### SQL
```
SELECT COUNT(*) AS pending_salary
FROM hrm_salary_slip
WHERE payment_status = 'Pending';
```


## 2. Charts (4)
### 1. Employees by Role (Bar Chart)

#### Tables Used: users, roles
#### SQL
```
SELECT 
r.role_name,
COUNT(u.id) AS total_employees
FROM users u
JOIN roles r ON u.role_id = r.id
GROUP BY r.role_name;
```

### 2. Salary Trend by Month (Line Chart)

#### Tables Used: hrm_salary_month
#### SQL
```
SELECT 
salary_month,
SUM(total_salary) AS total_salary
FROM hrm_salary_month
GROUP BY salary_month
ORDER BY salary_month;
```

### 4. Employees by Department & Employment Type (Bar Chart)
#### Tables Used: users, branches
#### SQL
```
SELECT 
    department,
    employment_type,
    COUNT(id) AS total_employees
FROM users
WHERE is_active = true
GROUP BY department, employment_type
ORDER BY department, total_employees DESC;
```

- X-axis: Department
- Y-axis: Total Employees
- Bars: Split by Employment Type (Grouped Bar Chart)

## 3. Tables (2)
### Table 1: Employee List

#### Meaning

Shows employee basic details

#### Tables Used

- users
- roles
- branches

#### Headers
- Employee Name
- Email
- Phone
- Role
- Branch
- Status
- Created Date

#### SQL
```
SELECT 
u.name AS employee_name,
u.email,
u.phone,
r.role_name,
b.branch_name,
u.status,
u.created_at
FROM users u
JOIN roles r ON u.role_id = r.id
JOIN branches b ON u.branch_id = b.id
ORDER BY u.created_at DESC;
```

### Table 2: Salary Slip List
#### Meaning

Shows generated salary slips

#### Tables Used

- hrm_salary_slip
- users

#### Headers
- Employee Name
- Salary Month
- Basic Salary
- Total Salary
- Paid Date
  
#### SQL
```
SELECT 
u.name AS employee_name,
s.salary_month,
s.basic_salary,
s.total_salary,
s.paid_date
FROM hrm_salary_slip s
JOIN users u ON s.user_id = u.id
ORDER BY s.salary_month DESC;
```
## 4. Alerts (2)
### 1. Unpaid Salary Alert
#### Meaning

Shows employees whose salary is pending

#### Tables Used

- hrm_salary_slip
- users

#### SQL
```
SELECT 
u.name,
s.salary_month,
s.total_salary
FROM hrm_salary_slip s
JOIN users u ON s.user_id = u.id
WHERE s.payment_status = 'Pending';
```

### 2. High Leave Alert
#### Meaning

Shows employees with high leave count

#### Tables Used

- hrm_holidays
- users

#### SQL
```
SELECT 
u.name,
COUNT(l.id) AS leave_count
FROM hrm_leaves l
JOIN users u ON l.user_id = u.id
GROUP BY u.name
HAVING COUNT(l.id) > 5;
```

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



# 🔐 HRM Dashboard Permission Visibility Note
## Module Dependency

### The HRM Dashboard depends on:
```
Users (Employee) Module
Branch Module
Roles Module
Salary Module (HRM Tables)
Leave Module (HRM Tables)
Task Module (for attendance/activity tracking)
```
### Reason
```
Employee data is stored in users
Employee structure (branch & role) comes from branches and roles
Salary data comes from hrm_salary_month and hrm_salary_slip
Leave data comes from hrm_leaves
Employee activity/attendance is derived from tasks
Dashboard combines all these data sources
```

If any of these modules are not accessible, HRM dashboard will not show complete and correct data.

## Permission Logic
### Case 1
```
User has:

HRM Module Permission
Users Module Permission
Branch Module Permission
Roles Module Permission
Salary Data Access
Leave Data Access
Task Module Permission
Result

HRM Dashboard will be VISIBLE

Reason
User can access employee data
User can access salary records
User can access leave records
User can access activity/task data
All HR metrics and reports will load correctly
```

Case 2

User has:

HRM Module Permission
BUT does not have Users Module Permission
OR does not have Salary Data Access
OR does not have Leave Data Access
OR does not have Task Module Permission
Result

HRM Dashboard will be NOT VISIBLE

Reason
HRM is fully dependent on employee data
Salary, leave, and activity data are required
Missing access will lead to incomplete or incorrect dashboard data
Message

"You don't have permission to see the dashboard."

Permission Matrix
