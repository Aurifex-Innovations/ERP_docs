# Module 18: Customer Management Dashboard

## 1. Key Performance Indicators (4)

| KPI Name | Meaning | Source Tables | Calculation |
|----------|--------|--------------|-------------|
| Total Customers | Total number of customers in system | customers | COUNT(id) WHERE is_deleted = FALSE |
| Active Customers | Customers currently active | customers | COUNT(id) WHERE status = 'Active' |
| Total Contract Customers | Customers with contract type | customers | COUNT(id) WHERE customer_type = 'Contract' |
| Total Customer Revenue | Total revenue generated from customers | sales_orders | SUM(total_value) |

---

# 2. Charts (3)

## 1. Customer Type Distribution

**Type:** Donut Chart

**Meaning:** Shows how many Contract, One-time, and Product customers exist.

### Source Table

customers

### SQL

```sql
SELECT 
customer_type,
COUNT(id) AS total_customers
FROM customers
WHERE is_deleted = FALSE
GROUP BY customer_type;
```

## 2. Branch Wise Customers

**Type:** Bar Chart

**Meaning:** Shows number of customers in each branch.

### Source Table

- customers
- branches

### SQL

```sql
SELECT 
b.branch_name,
COUNT(c.id) AS total_customers
FROM customers c
JOIN branches b
ON c.branch_id = b.id
WHERE c.is_deleted = FALSE
GROUP BY b.branch_name
ORDER BY total_customers DESC;
```

## 3. Monthly Customer Onboarding Trend

**Type:** Line Chart

**Meaning:** Shows how many customers are added every month.

### Source Table

customers

### SQL

```sql
SELECT 
DATE_TRUNC('month', created_at) AS month,
COUNT(id) AS total_customers
FROM customers
WHERE is_deleted = FALSE
GROUP BY month
ORDER BY month;
```

# 3. Tables (2)
## 1. Recent Customers
**Meaning**

Shows latest customers added in system.

**Tables Used**

- customers
- branches

**Table Headers**

| Customer ID | Customer Name | Customer Type | Phone | Branch | Status | Created Date |

**SQL Query**
```
SELECT 
c.customer_id AS "Customer ID",
c.name AS "Customer Name",
c.customer_type AS "Customer Type",
c.phone AS "Phone",
b.branch_name AS "Branch",
c.status AS "Status",
c.created_at AS "Created Date"
FROM customers c
JOIN branches b
ON c.branch_id = b.id
WHERE c.is_deleted = FALSE
ORDER BY c.created_at DESC
LIMIT 10;
```

## 2. Customers With Active Contracts
**Meaning**

Shows customers who currently have active contracts.
**Tables Used**

- customers
- contracts

**Table Headers**

| Customer ID | Customer Name | Contract ID | Contract Start Date | Contract End Date | Contract Value | Status |

**SQL Query**
```
SELECT 
c.customer_id AS "Customer ID",
c.name AS "Customer Name",
con.contract_id AS "Contract ID",
con.start_date AS "Contract Start Date",
con.end_date AS "Contract End Date",
con.contract_value AS "Contract Value",
con.status AS "Status"
FROM customers c
JOIN contracts con
ON c.id = con.customer_id
WHERE con.status = 'Active'
AND c.is_deleted = FALSE;
```

# 4. Alerts (2)
## 1. Inactive Customer Alert
**Meaning**

Shows customers who are inactive.

**Tables Used**

customers

**SQL Query**
```
SELECT 
customer_id,
name,
customer_type,
branch_id,
status
FROM customers
WHERE status = 'Inactive'
AND is_deleted = FALSE;
```

## 2. Customer Without Contract Alert
**Meaning**

Shows customers who do not have any contract.

**Tables Used**

customers
contracts

**SQL Query**
```
SELECT 
c.customer_id,
c.name,
c.customer_type,
c.created_at
FROM customers c
LEFT JOIN contracts con
ON c.id = con.customer_id
WHERE con.customer_id IS NULL
AND c.is_deleted = FALSE;
```

## 5. Global Filters for KPIs and Charts and tables 

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

# 🔐 Customer Dashboard Permission Note

## Module Dependency

The Customer Dashboard depends on:

- Branch Module
- Contract Module
- Sales Order Module

Reason:

- Customers are linked with Branch
- Dashboard shows branch-wise customers
- Customers with active contracts are displayed
- Contract data is used in tables and alerts
- Customer revenue comes from sales orders
- Sales order data is used in KPIs
- If user cannot access branch, contract, or sales order data, dashboard will not show correct information

---

# Permission Logic

## Case 1

User has:

- Customer Module Permission
- Branch Module Permission
- Contract Module Permission
- Sales Order Module Permission

### Result

Customer Dashboard will be **VISIBLE**

### Reason

- User can see customer data
- User can see branch data
- User can see contract data
- User can see sales order revenue
- All dashboard data will load properly

---

## Case 2

User has:

- Customer Module Permission
- BUT does not have Branch Module Permission
- OR does not have Contract Module Permission
- OR does not have Sales Order Module Permission

### Result

Customer Dashboard will be **NOT VISIBLE**

### Reason

- Customer dashboard uses branch data
- Contract data is required for contract customers
- Sales order data is required for revenue
- Missing module permission will block data
- Dashboard will not show proper information

### Message

"You don't have permission to see the dashboard."

---

# Permission Matrix

| Customer Module | Branch Module | Contract Module | Sales Order Module | Dashboard |
|---------------|--------------|----------------|-------------------|----------|
| ✅ | ✅ | ✅ | ✅ | ✅ Visible |
| ✅ | ❌ | ✅ | ✅ | ❌ Not Visible |
| ✅ | ✅ | ❌ | ✅ | ❌ Not Visible |
| ✅ | ✅ | ✅ | ❌ | ❌ Not Visible |
| ❌ | ❌ | ❌ | ❌ | ❌ Not Visible |

---

# Backend Logic

IF customer_module_view = TRUE  
AND branch_module_view = TRUE  
AND contract_module_view = TRUE  
AND sales_order_module_view = TRUE  

THEN  
    Show Dashboard  

ELSE  
    Hide Dashboard  
    Show Message: "You don't have permission to see the dashboard."

---

# Final Rule

Customer Dashboard will only be visible when user has:

- Customer Module Permission
- Branch Module Permission
- Contract Module Permission
- Sales Order Module Permission

Otherwise dashboard will be hidden.
