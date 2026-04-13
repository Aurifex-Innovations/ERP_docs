# Module 7: Branch Management Dashboard

The Branch Management Dashboard provides real-time visibility into the organization’s physical network and operational capacity. It helps monitor geographic distribution, infrastructure growth, and staffing efficiency.

It also acts as a primary dimensional filter across the ERP system.

---

## 2.1 Key Performance Indicators (KPIs)

| KPI Name | Business Meaning | Source Table | Source Columns | Aggregation Logic |
|----------|----------------|-------------|---------------|------------------|
| Total Active Branches | Current operational footprint | branches | status | COUNT(id) WHERE status = 'ACTIVE' |
| Branch Type Distribution | Distribution of branch types | branches | branch_type | COUNT(id) GROUP BY branch_type |
| Regional Branch Density | Branch concentration by state | branches | state | COUNT(id) GROUP BY state |
| Employee-to-Branch Ratio | Staffing efficiency per branch | users, branches | id | COUNT(users.id) / COUNT(branches.id) |

---

## 2.2 Strategic Visualizations (Charts)

### 1. Branch Growth Trend (Line Chart)
- What it shows: Monthly new branch creation  
- Why it matters: Tracks expansion speed and infrastructure scaling  
- X-axis: DATE_TRUNC('month', created_at)  
- Y-axis: Cumulative count  
- Source: branches.created_at  

### 2. Geographic Distribution by State (Bar Chart)
- What it shows: Number of branches per state  
- Why it matters: Identifies underserved regions and market gaps  
- X-axis: state  
- Y-axis: COUNT(id)  
- Source: branches.state  

### 3. Operational Status Breakdown (Pie Chart)
- What it shows: Active vs Inactive branches  
- Why it matters: Helps monitor network availability and downtime  
- Source: branches.status  

---

## 2.3 Operational Data Tables

### 1. Branch Contact & Leadership Directory
- Columns:
  - branch_name  
  - branch_code  
  - city  
  - email  
  - phone_number  
- Source Table: branches  
- Purpose: Enables direct communication and leadership lookup  

### 2. Recent Branch Activations (Audit Table)
- Columns:
  - branch_name  
  - branch_code  
  - branch_type  
  - created_at  
  - created_by  
- Source Table: branches  
- Purpose: Tracks accountability for new branch creation  

---

## 2.4 Proactive Business Alerts

### 1. Inactive Branch Warning
```sql
IF status = 'INACTIVE'
THEN TRIGGER ALERT;
```

### 2. Branch Code Integrity Alert
```sql
IF LENGTH(branch_code) != 3
OR branch_code !~ '^[A-Z0-9]{3}$'
THEN TRIGGER ALERT;
```

---
---

## 2.5 Global Filters for KPIs and Charts and tables

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

---

## Summary

This module ensures:
- Visibility into branch performance  
- Monitoring of expansion and distribution  
- Maintenance of data integrity  
- Quick response through real-time alerts  
