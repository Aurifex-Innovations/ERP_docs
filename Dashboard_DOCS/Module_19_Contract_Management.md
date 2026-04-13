# Module 19: Contract Management Dashboard

## 1. Key Performance Indicators (4)

| KPI Name | Meaning | Source Table | Calculation |
|----------|--------|-------------|-------------|
| Total Contracts | Total number of contracts in system | contracts | COUNT(id) WHERE is_deleted = FALSE |
| Active Contracts | Currently running contracts | contracts | COUNT(id) WHERE status = 'Active' |
| Total Contract Value | Total revenue from contracts | contracts | SUM(total_sale_value) |
| Expiring Soon Contracts | Contracts ending soon | contracts | COUNT(id) WHERE end_date <= CURRENT_DATE + INTERVAL '30 days' AND status = 'Active' |

---

# 2. Charts (3)

## 1. Contract Status Distribution

**Type:** Donut Chart

**Meaning:** Shows Draft, Active, Expiring, Terminated and Expired contracts.

### Source Table

contracts

### SQL

```sql
SELECT 
status,
COUNT(id) AS total_contracts
FROM contracts
WHERE is_deleted = FALSE
GROUP BY status;
```

## 2. Branch Wise Contracts

**Type:** Bar Chart

**Meaning:** Shows number of contracts in each branch.

**Source Tables**

- contracts
- branches

**SQL**
```
SELECT 
b.branch_name,
COUNT(c.id) AS total_contracts
FROM contracts c
JOIN branches b
ON c.branch_id = b.id
WHERE c.is_deleted = FALSE
GROUP BY b.branch_name
ORDER BY total_contracts DESC;
```

## 3. Monthly Contract Value Trend

**Type:** Line Chart

**Meaning:** Shows contract value generated every month.

**Source Table**

contracts

**SQL**
```
SELECT 
DATE_TRUNC('month', start_date) AS month,
SUM(total_sale_value) AS total_value
FROM contracts
WHERE is_deleted = FALSE
GROUP BY month
ORDER BY month;
```

# 3. Tables (2)
## 1. Recent Contracts
**Meaning**

Shows latest created contracts.

**Tables Used**

- contracts
- customers
- gma_sheets
- branches

**Table Headers**

| Contract ID | Customer Name | GMA ID | Contract Value | Start Date | End Date | Status | Branch |

**SQL Query**
```
SELECT 
c.contract_id AS "Contract ID",
cu.name AS "Customer Name",
g.id AS "GMA ID",
c.total_sale_value AS "Contract Value",
c.start_date AS "Start Date",
c.end_date AS "End Date",
c.status AS "Status",
b.branch_name AS "Branch"
FROM contracts c
JOIN customers cu
ON c.customer_id = cu.id
JOIN gma_sheet g
ON c.id = g.id
JOIN branches b
ON c.branch_id = b.id
WHERE c.is_deleted = FALSE
ORDER BY c.created_at DESC
LIMIT 10;
```

## 2. Expiring Contracts List
**Meaning**

Shows contracts that will expire soon.

**Tables Used**

- contracts
- customers
- branches

**Table Headers**

| Contract ID | Customer Name | Contract Value | End Date | Status | Branch |

**SQL Query**
```
SELECT 
c.contract_id AS "Contract ID",
cu.name AS "Customer Name",
c.total_sale_value AS "Contract Value",
c.end_date AS "End Date",
c.status AS "Status",
b.branch_name AS "Branch"
FROM contracts c
JOIN customers cu
ON c.customer_id = cu.id
JOIN branches b
ON c.branch_id = b.id
WHERE c.end_date <= CURRENT_DATE + INTERVAL '30 days'
AND c.status = 'Active'
AND c.is_deleted = FALSE
ORDER BY c.end_date;
```

# 4. Alerts (2)
## 1. Contract Expiry Alert
**Meaning**

Shows contracts that are about to expire.

**Tables Used**

- contracts
- customers

**SQL Query**
```
SELECT 
c.contract_id,
cu.name AS customer_name,
c.end_date,
c.total_sale_value,
c.status
FROM contracts c
JOIN customers cu
ON c.customer_id = cu.id
WHERE c.end_date <= CURRENT_DATE + INTERVAL '7 days'
AND c.status = 'Active'
AND c.is_deleted = FALSE;
```

## 2. Contract Without Sales Order Alert
**Meaning**

Shows active contracts that do not have any sales orders generated.

**Tables Used**

- contracts
- sales_orders

**SQL Query**
```
SELECT 
c.contract_id,
c.total_sale_value,
c.start_date,
c.status
FROM contracts c
LEFT JOIN sales_orders so
ON c.id = so.contract_id
WHERE so.contract_id IS NULL
AND c.status = 'Active'
AND c.is_deleted = FALSE;
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

# 🔐 Contract Dashboard Permission Note

## Module Dependency

The Contract Dashboard depends on:

- GMA Module (Module 17)
- Customer Module (Module 18)
- Branch Module (Module 7)
- Technician Module (Module 8)
- Sales Order Module (Module 20)

Reason:

- Contracts are created from approved GMA
- Customer data is required for contract creation
- Branch is used for contract filtering and tracking
- Technician is used for service execution and assignment
- Sales orders are generated from contracts
- Dashboard uses data from all these modules
- If user cannot access any of these modules, dashboard will not show correct information

---

# Permission Logic

## Case 1

User has:

- Contract Module Permission
- GMA Module Permission
- Customer Module Permission
- Branch Module Permission
- Technician Module Permission
- Sales Order Module Permission

### Result

Contract Dashboard will be **VISIBLE**

### Reason

- User can see contract data
- User can see GMA data
- User can see customer data
- User can see branch data
- User can see technician data
- User can see sales order data
- All dashboard data will load properly

---

## Case 2

User has:

- Contract Module Permission
- BUT does not have any one of the following:

  - GMA Module Permission
  - Customer Module Permission
  - Branch Module Permission
  - Technician Module Permission
  - Sales Order Module Permission

### Result

Contract Dashboard will be **NOT VISIBLE**

### Reason

- Contract depends on GMA
- Contract depends on Customer
- Contract depends on Branch
- Contract depends on Technician
- Contract generates Sales Orders
- Missing any module permission will block data
- Dashboard will not show proper information

### Message

"You don't have permission to see the dashboard."

---

# Permission Matrix

| Contract Module | GMA Module | Customer Module | Branch Module | Technician Module | Sales Order Module | Dashboard |
|----------------|-----------|----------------|--------------|------------------|-------------------|----------|
| ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ Visible |
| ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ Not Visible |
| ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ Not Visible |
| ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ Not Visible |
| ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ Not Visible |
| ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ Not Visible |
| ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ Not Visible |

---

# Backend Logic

IF contract_module_view = TRUE  
AND gma_module_view = TRUE  
AND customer_module_view = TRUE  
AND branch_module_view = TRUE  
AND technician_module_view = TRUE  
AND sales_order_module_view = TRUE  

THEN  
    Show Dashboard  

ELSE  
    Hide Dashboard  
    Show Message: "You don't have permission to see the dashboard."

---

# Final Rule

Contract Dashboard will only be visible when user has:

- Contract Module Permission
- GMA Module Permission
- Customer Module Permission
- Branch Module Permission
- Technician Module Permission
- Sales Order Module Permission

Otherwise dashboard will be hidden.

