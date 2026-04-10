# Module 17: GMA Dashboard

## 1. Key Performance Indicators

| KPI Name | Meaning | Source Table | Calculation |
|----------|--------|-------------|-------------|
| Total GMA Sheets | Total number of GMA created | gma_sheets | COUNT(id) WHERE is_deleted = FALSE |
| Approved GMA Sheets | Total approved GMA sheets | gma_sheets | COUNT(id) WHERE status = 'APPROVED' |
| Pending GMA Sheets | GMA waiting for approval | gma_sheets | COUNT(id) WHERE status = 'PENDING' |
| Average Gross Margin | Average overall gross margin | gma_sheets | AVG(overall_gross_margin) |

---

## 2. Charts

### 1. GMA Status Distribution

**Type:** Donut Chart

**Meaning:** Shows how many GMA are Draft, Pending, Approved, or Rejected.

**Source Table:**

gma_sheets

**SQL**

```sql
SELECT 
status,
COUNT(id) AS total_gma
FROM gma_sheets
WHERE is_deleted = FALSE
GROUP BY status;
```

### 2. Branch Wise GMA Count

**Type:** Bar Chart

**Meaning:** Shows number of GMA created per branch.

**Source Table:**

gma_sheets
branches

**SQL**

```sql
SELECT 
b.branch_name,
COUNT(g.id) AS total_gma
FROM gma_sheets g
JOIN branches b
ON g.branch_id = b.id
WHERE g.is_deleted = FALSE
GROUP BY b.branch_name
ORDER BY total_gma DESC;
```


### 3. Monthly GMA Creation Trend

**Type:** Line Chart

**Meaning:** Shows GMA created every month.

**Source Table:**

gma_sheets

**SQL**

```sql
SELECT 
DATE_TRUNC('month', created_at) AS month,
COUNT(id) AS total_gma
FROM gma_sheets
WHERE is_deleted = FALSE
GROUP BY month
ORDER BY month;
```
## 3. Tables
### 1. Recent GMA Sheets

**Meaning:** Shows latest GMA sheets created in the system.

**Source Table:**

gma_sheets
branches

**Table Headers**
| GMA ID | Source Type | Contract Duration | Start Date | Branch Name | Total Annual Price | Status | Created Date |

**SQL**

```sql
SELECT 
g.id AS "GMA ID",
g.source_type AS "Source Type",
g.contract_duration AS "Contract Duration",
g.proposed_start_date AS "Start Date",
b.branch_name AS "Branch Name",
g.total_annual_price AS "Total Annual Price",
g.status AS "Status",
g.created_at AS "Created Date"
FROM gma_sheets g
JOIN branches b
ON g.branch_id = b.id
WHERE g.is_deleted = FALSE
ORDER BY g.created_at DESC
LIMIT 10;
```

### 2. Approved GMA Financial Summary

**Meaning:** Shows approved GMA with financial details.

**Source Table:**

gma_sheets

**Table Headers**
| GMA ID | Total Annual Cost | Total Annual Price | Gross Margin | GM With DOC | Total Visits Per Month | Approved Date |

**SQL**

```sql
SELECT 
id AS "GMA ID",
total_annual_cost AS "Total Annual Cost",
total_annual_price AS "Total Annual Price",
overall_gross_margin AS "Gross Margin",
gm_with_doc AS "GM With DOC",
total_visits_per_month AS "Total Visits Per Month",
approved_on AS "Approved Date"
FROM gma_sheets
WHERE status = 'APPROVED'
AND is_deleted = FALSE
ORDER BY approved_on DESC;  
```

### 4. Alerts (2)
#### 1. Pending Approval Alert
**Meaning**

Shows GMA sheets waiting for approval beyond deadline.

**Tables Used**

gma_sheets

**SQL**
```
SELECT 
id,
source_type,
status,
deadline,
created_at
FROM gma_sheets
WHERE status = 'PENDING'
AND deadline < CURRENT_TIMESTAMP
AND is_deleted = FALSE;
```

#### 2. Low Gross Margin Alert
**Meaning**

Shows GMA sheets with low profit margin.

**Tables Used**

gma_sheets

**SQL**
```
SELECT 
id,
total_annual_price,
total_annual_cost,
overall_gross_margin,
status
FROM gma_sheets
WHERE overall_gross_margin < 10
AND is_deleted = FALSE;
```
# 🔐 GMA Dashboard Permission Note

## Module Dependency

The GMA Dashboard depends on:

- Lead Module
- Customer Module

Reason:

- GMA can be created from Leads
- GMA can be created from Customers
- Source Type is used in GMA (Lead / Customer)
- Dashboard shows GMA data based on source
- If user cannot access Lead or Customer data, dashboard will not show correct information

---

# Permission Logic

## Case 1

User has:

- GMA Module Permission
- Lead Module Permission
- Customer Module Permission

### Result

GMA Dashboard will be **VISIBLE**

### Reason

- User can see GMA data
- User can see lead-based GMA
- User can see customer-based GMA
- Source type data will load properly

---

## Case 2

User has:

- GMA Module Permission
- BUT does not have Lead Module Permission
- OR does not have Customer Module Permission

### Result

GMA Dashboard will be **NOT VISIBLE**

### Reason

- GMA is created from Leads and Customers
- Source Type is used in dashboard
- User cannot access source data
- Dashboard will not show proper information

### Message

"You don't have permission to see the dashboard."

---

# Permission Matrix

| GMA Module | Lead Module | Customer Module | Dashboard |
|-----------|------------|----------------|----------|
| ✅ | ✅ | ✅ | ✅ Visible |
| ✅ | ❌ | ✅ | ❌ Not Visible |
| ✅ | ✅ | ❌ | ❌ Not Visible |
| ❌ | ❌ | ❌ | ❌ Not Visible |

---

# Backend Logic

IF gma_module_view = TRUE  
AND lead_module_view = TRUE  
AND customer_module_view = TRUE  

THEN  
    Show Dashboard  

ELSE  
    Hide Dashboard  
    Show Message: "You don't have permission to see the dashboard."

---

# Final Rule

GMA Dashboard will only be visible when user has:

- GMA Module Permission
- Lead Module Permission
- Customer Module Permission

Otherwise dashboard will be hidden.
