# 📊 Module 25: HRM Dashboard

---

## 1. KPIs (6)

### KPI 1: Total Employees

Detects total number of employees in the company.

### Source Table

users

### SQL

```sql
SELECT COUNT(*) AS total_employees
FROM users;
```

---

### KPI 2: Active Employees

Detects employees who are currently active.

### Source Table

users

### SQL

```sql
SELECT COUNT(*) AS active_employees
FROM users
WHERE status = 'Active';
```

---

### KPI 3: Total Salary Paid This Month

Detects total salary paid in the current month.

### Source Table

employee_salary

### SQL

```sql
SELECT SUM(net_salary) AS total_salary_paid
FROM employee_salary
WHERE payment_status = 'Paid'
AND EXTRACT(MONTH FROM salary_month) = EXTRACT(MONTH FROM CURRENT_DATE)
AND EXTRACT(YEAR FROM salary_month) = EXTRACT(YEAR FROM CURRENT_DATE);
```

---

### KPI 4: Employees On Leave

Detects employees currently on leave.

### Source Table

employee_leaves

### SQL

```sql
SELECT COUNT(*) AS employees_on_leave
FROM employee_leaves
WHERE status = 'Approved'
AND CURRENT_DATE BETWEEN from_date AND to_date;
```

---

### KPI 5: Present Employees Today

Detects employees present today.

### Source Table

employee_attendance

### SQL

```sql
SELECT COUNT(*) AS present_today
FROM employee_attendance
WHERE attendance_date = CURRENT_DATE
AND status = 'Present';
```

---

### KPI 6: Pending Leave Requests

Detects pending leave requests.

### Source Table

employee_leaves

### SQL

```sql
SELECT COUNT(*) AS pending_leave_requests
FROM employee_leaves
WHERE status = 'Pending';
```

---

## 2. Charts (5)

### Chart 1: Employees by Branch

Shows employee count per branch.

### Source Tables

users, branches

### SQL

```sql
SELECT 
b.branch_name,
COUNT(u.id) AS employee_count
FROM users u
JOIN branches b ON u.branch_id = b.id
GROUP BY b.branch_name
ORDER BY employee_count DESC;
```

### Chart Type

Bar Chart

---

### Chart 2: Employees by Department

Shows department-wise distribution.

### Source Tables

users, departments

### SQL

```sql
SELECT 
d.department_name,
COUNT(u.id) AS total_employees
FROM users u
JOIN departments d ON u.department_id = d.id
GROUP BY d.department_name;
```

### Chart Type

Pie Chart

---

### Chart 3: Monthly Salary Payment Trend

Shows salary trend month-wise.

### Source Table

employee_salary

### SQL

```sql
SELECT 
salary_month,
SUM(net_salary) AS total_salary
FROM employee_salary
GROUP BY salary_month
ORDER BY salary_month;
```

### Chart Type

Line Chart

---

### Chart 4: Attendance Status Distribution

Shows present, absent, leave distribution.

### Source Table

employee_attendance

### SQL

```sql
SELECT 
status,
COUNT(*) AS total
FROM employee_attendance
GROUP BY status;
```

### Chart Type

Donut Chart

---

### Chart 5: Leave Type Distribution

Shows leave type usage.

### Source Table

employee_leaves

### SQL

```sql
SELECT 
leave_type,
COUNT(*) AS total_leaves
FROM employee_leaves
GROUP BY leave_type;
```

### Chart Type

Bar Chart

---

## 3. Tables (3)

### Table 1: Employee List

Shows employee details with branch and role.

### Source Tables

users, branches, roles

### SQL

```sql
SELECT 
u.id AS employee_id,
u.first_name,
u.email,
u.phone,
b.branch_name,
r.role_name,
u.status,
u.created_at
FROM users u
JOIN branches b ON u.branch_id = b.id
JOIN roles r ON u.role_id = r.id
ORDER BY u.created_at DESC
LIMIT 20;
```

---

### Table 2: Monthly Salary List

Shows salary details.

### Source Tables

employee_salary, users

### SQL

```sql
SELECT 
u.first_name,
es.salary_month,
es.basic_salary,
es.net_salary,
es.payment_status
FROM employee_salary es
JOIN users u ON es.employee_id = u.id
ORDER BY es.salary_month DESC;
```

---

### Table 3: Leave Requests List

Shows leave requests.

### Source Tables

employee_leaves, users

### SQL

```sql
SELECT 
u.first_name,
el.leave_type,
el.from_date,
el.to_date,
el.status,
el.created_at
FROM employee_leaves el
JOIN users u ON el.employee_id = u.id
ORDER BY el.created_at DESC;
```

---

## 4. Alerts (3)

### Alert 1: High Pending Leave Requests

Detects excessive pending requests.

### Source Table

employee_leaves

### SQL

```sql
SELECT 
COUNT(*) AS pending_leaves
FROM employee_leaves
WHERE status = 'Pending'
HAVING COUNT(*) > 10;
```

---

### Alert 2: Salary Not Paid

Detects unpaid salaries.

### Source Tables

employee_salary, users

### SQL

```sql
SELECT 
u.first_name,
es.salary_month,
es.net_salary
FROM employee_salary es
JOIN users u ON es.employee_id = u.id
WHERE es.payment_status = 'Unpaid';
```

---

### Alert 3: Absent Employees Today

Detects employees absent today.

### Source Tables

employee_attendance, users

### SQL

```sql
SELECT 
u.first_name,
ea.attendance_date,
ea.status
FROM employee_attendance ea
JOIN users u ON ea.employee_id = u.id
WHERE ea.attendance_date = CURRENT_DATE
AND ea.status = 'Absent';
```

## 🔐 HRM Dashboard Permission Visibility Note
### Module Dependency

#### The HRM Dashboard depends on:

- Users (Employee) Module
- Branch Module
- Roles Module
- Salary Module
- Attendance Module
- Leave Module


#### Reason
- HRM manages employee data from Users module
- Employee structure is defined using Branch and Roles
- Salary is generated using salary module
- Attendance is tracked using attendance module
- Leave records are managed using leave module
- Dashboard shows employee, salary, attendance, and leave information

#### If user cannot access these modules, HRM dashboard will not show proper data.

### Permission Logic
#### Case 1
```
User has:

HRM Module Permission
Users Module Permission
Branch Module Permission
Roles Module Permission
Salary Module Permission
Attendance Module Permission
Leave Module Permission
Result

HRM Dashboard will be VISIBLE

Reason
User can see employee data
User can see salary records
User can see attendance records
User can see leave records
Dashboard will load complete HR information
```

#### Case 2
```
User has:

HRM Module Permission
BUT does not have Users Module Permission
OR does not have Salary Module Permission
OR does not have Attendance Module Permission
OR does not have Leave Module Permission
Result

HRM Dashboard will be NOT VISIBLE

Reason
HRM depends on employee data
Salary and attendance require employee records
Leave data depends on employees
Without dependent modules, dashboard will show incomplete data
Message

"You don't have permission to see the dashboard."
```

### Permission Matrix
| HRM Module | Users | Branch | Roles | Salary | Attendance | Leave | Dashboard     |
| ---------- | ----- | ------ | ----- | ------ | ---------- | ----- | ------------- |
| ✅          | ✅     | ✅      | ✅     | ✅      | ✅          | ✅     | ✅ Visible     |
| ✅          | ❌     | ✅      | ✅     | ✅      | ✅          | ✅     | ❌ Not Visible |
| ✅          | ✅     | ❌      | ✅     | ✅      | ✅          | ✅     | ❌ Not Visible |
| ✅          | ✅     | ✅      | ❌     | ✅      | ✅          | ✅     | ❌ Not Visible |
| ✅          | ✅     | ✅      | ✅     | ❌      | ✅          | ✅     | ❌ Not Visible |
| ✅          | ✅     | ✅      | ✅     | ✅      | ❌          | ✅     | ❌ Not Visible |
| ✅          | ✅     | ✅      | ✅     | ✅      | ✅          | ❌     | ❌ Not Visible |
