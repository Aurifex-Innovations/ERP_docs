# Module 8: Employee & Hiring Management Dashboard

The Employee & Hiring Management Dashboard monitors the complete employee lifecycle, from hiring requests to active workforce and payroll. It helps optimize workforce planning, hiring efficiency, and salary expenditure.

---

## 3.1 Key Performance Indicators (KPIs)

| KPI Name | Business Meaning | Source Table | Source Columns | Aggregation Logic |
|----------|----------------|-------------|---------------|------------------|
| Total Active Workforce | Current headcount for production capacity | users | is_active | COUNT(id) WHERE is_active = true |
| Open Hiring Requisitions | Talent acquisition requirements | hiring_requests | status | COUNT(id) WHERE status = 'PENDING' |
| Departmental Staff Count | Functional staff distribution | users | department | COUNT(id) GROUP BY department |
| Monthly Payroll Liability | Total salary obligation | user_salary_details | basic_salary | SUM(basic_salary) WHERE user_id IN (SELECT id FROM users WHERE is_active = true) |

---

## 3.2 Strategic Visualizations (Charts)

### 1. Hiring Request Pipeline (Funnel Chart)
- What it shows: Flow from PENDING → APPROVED → CONVERTED  
- Why it matters: Identifies hiring bottlenecks and conversion efficiency  
- Source: hiring_requests.status  

### 2. Employment Type Composition (Pie Chart)
- What it shows: Distribution of Permanent, Contract, and Intern employees  
- Why it matters: Evaluates workforce stability  
- Source: users.employment_type  

### 3. Onboarding Trend (Area Chart)
- What it shows: New hires over time  
- Why it matters: Tracks growth and onboarding load  
- X-axis: date_of_joining  
- Y-axis: COUNT(id)  
- Source: users.date_of_joining  

---

## 3.3 Operational Data Tables

### 1. Critical Hiring Requisitions
- Columns:
  - id  
  - department  
  - designation  
  - number_of_positions  
  - expected_date_of_joining  
- Source Table: hiring_requests  
- Filter: number_of_positions > 5  
- Purpose: Prioritize high-volume hiring needs  

### 2. Employee Compensation Audit
- Columns:
  - emp_id  
  - first_name  
  - department  
  - salary_type  
  - basic_salary  
  - hra  
  - deductions  
- Source Tables:
```sql
users u
JOIN user_salary_details s ON u.id = s.user_id
```
- Purpose: Payroll verification and budget reconciliation  

---

## 3.4 Proactive Business Alerts

### 1. Delayed Joining Alert
```sql
IF expected_date_of_joining < CURRENT_DATE
AND status != 'CONVERTED'
THEN TRIGGER ALERT;
```

### 2. Low Leave Balance Warning
```sql
IF casual_leave <= 1
OR sick_leave <= 1
THEN TRIGGER ALERT;
```

---

## Summary

This module ensures:
- Efficient workforce management  
- Visibility into hiring pipeline  
- Control over payroll expenses  
- Early detection of HR risks through alerts  
