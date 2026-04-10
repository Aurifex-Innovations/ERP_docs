# 📊 Module 20: Sales Order Management Dashboard

The Sales Order Dashboard provides complete visibility of order creation, revenue, execution, and delivery progress.

---

# 1. KPIs (4)

## 1. Total Sales Orders

### Meaning
Shows how many sales orders are created.

### Table
sales_order

### Calculation

```sql
SELECT COUNT(id) AS total_sales_orders
FROM sales_order
WHERE is_deleted = FALSE;
```

## 2. Total Sales Amount
**Meaning**

Shows total revenue generated from sales orders.

**Table**

sales_order

**Calculation**
```
SELECT SUM(grand_total) AS total_sales_amount
FROM sales_order
WHERE is_deleted = FALSE;
```

## 3. Open Sales Orders
**Meaning**

Shows sales orders that are still active.

**Table**

sales_order

**Calculation**
```
SELECT COUNT(id) AS open_sales_orders
FROM sales_order
WHERE status IN ('Draft','Open','In Progress')
AND is_deleted = FALSE;
```

## 4. Completed Sales Orders
**Meaning**

Shows completed or fulfilled sales orders.

**Table**

sales_order

**Calculation**
```
SELECT COUNT(id) AS completed_sales_orders
FROM sales_order
WHERE status IN ('Fulfilled','Billed','Completed')
AND is_deleted = FALSE;
```

# 2. Charts (3)
## 1. Sales Order Status Chart
**Meaning**

Shows how many orders are in each status.

**Tables**

sales_order

**SQL**
```
SELECT 
status,
COUNT(id) AS total_orders
FROM sales_order
WHERE is_deleted = FALSE
GROUP BY status;
```

**Chart Type**

Donut Chart


## 2. Monthly Sales Revenue Chart
**Meaning**

Shows monthly sales revenue.

**Tables**

sales_order

**SQL**
```
SELECT 
DATE_TRUNC('month', created_at) AS month,
SUM(grand_total) AS revenue
FROM sales_order
WHERE is_deleted = FALSE
GROUP BY month
ORDER BY month;
```
**Chart Type**

Line Chart


## 3. Branch Wise Sales Chart
**Meaning**

Shows sales amount by branch.

**Tables**

sales_order
branches

**SQL**
```
SELECT 
b.branch_name,
SUM(s.grand_total) AS total_sales
FROM sales_order s
JOIN branches b
ON s.branch_id = b.id
WHERE s.is_deleted = FALSE
GROUP BY b.branch_name
ORDER BY total_sales DESC;
```

**Chart Type**

Bar Chart


#3. Tables (2)
## Table 1: Recent Sales Orders
**Meaning**

Shows latest sales orders.

**Tables**

- sales_order
- customers
- branches

**SQL**
```
SELECT 
s.so_number,
c.customer_name,
s.order_type,
s.grand_total,
s.status,
b.branch_name,
s.created_at
FROM sales_order s
JOIN customers c
ON s.customer_id = c.id
JOIN branches b
ON s.branch_id = b.id
WHERE s.is_deleted = FALSE
ORDER BY s.created_at DESC
LIMIT 20;
```

**Table Header**
| SO Number | Customer Name | Order Type | Amount | Status | Branch | Created Date |


## Table 2: Sales Order Item List
**Meaning**

Shows products and services inside sales orders.

**Tables**

- sales_order_items
- sales_order
- inventory_products

**SQL**
```
SELECT 
s.so_number,
p.product_name,
i.quantity,
i.uom,
i.unit_price,
i.tax_amount,
i.line_total
FROM sales_order_items i
JOIN sales_order s
ON i.sales_order_id = s.id
JOIN inventory_products p
ON i.product_id = p.id
WHERE i.is_deleted = FALSE;
```

**Table Header**
| SO Number | Product Name | Quantity | UOM | Unit Price | Tax Amount | Line Total |


# 4. Alerts (2)
## 1. High Value Sales Order Alert
**Meaning**

Shows high-value sales orders.

**Tables**

sales_order

**SQL**
```
SELECT 
so_number,
customer_id,
grand_total,
status,
created_at
FROM sales_order
WHERE grand_total > 50000
AND status IN ('Draft','Open','In Progress')
AND is_deleted = FALSE;
```

**Purpose**
- Detect large orders
- Track important revenue


## 2. Pending Sales Order Alert
**Meaning**

Shows sales orders that are not completed for many days.

**Tables**

sales_order

**SQL**
```
SELECT 
so_number,
customer_id,
status,
created_at
FROM sales_order
WHERE status IN ('Draft','Open','In Progress')
AND created_at < CURRENT_DATE - INTERVAL '7 days'
AND is_deleted = FALSE;
```

**Purpose**
- Detect delayed orders
- Improve execution speed

# 🔐 Sales Order Dashboard Permission Note

## Module Dependency

The Sales Order Dashboard depends on:

- Contract Module
- GMA Module
- Quotation Module
- Customer Module
- Product Module
- Tax Module

Reason:

- Sales Orders can be created from Contracts
- Sales Orders can be created from GMA
- Sales Orders can be created from Quotations
- Customer is required in Sales Order
- Product and Service details come from Product Module
- Tax and GST calculation comes from Tax Module
- Dashboard shows complete sales order data based on these sources
- If user cannot access these modules, dashboard will not show proper data

---

# Permission Logic

## Case 1

User has:

- Sales Order Module Permission
- Contract Module Permission
- GMA Module Permission
- Quotation Module Permission
- Customer Module Permission
- Product Module Permission
- Tax Module Permission

### Result

Sales Order Dashboard will be **VISIBLE**

### Reason

- User can see sales order data
- User can see contract-based sales orders
- User can see GMA-based sales orders
- User can see quotation-based sales orders
- Customer and product data will load properly
- Tax and pricing calculations will be visible

---

## Case 2

User has:

- Sales Order Module Permission

BUT does not have:

- Contract Module Permission
- OR GMA Module Permission
- OR Quotation Module Permission
- OR Customer Module Permission
- OR Product Module Permission
- OR Tax Module Permission

### Result

Sales Order Dashboard will be **NOT VISIBLE**

### Reason

- Sales Orders depend on multiple source modules
- Contract, GMA, and Quotation provide order data
- Customer and Product provide essential information
- Tax module provides pricing calculation
- Without these modules, dashboard data will be incomplete
- System should restrict dashboard visibility

### Message

"You don't have permission to see the dashboard."

---

# Permission Matrix

| Sales Order Module | Contract | GMA | Quotation | Customer | Product | Tax | Dashboard |
|-------------------|---------|-----|----------|---------|--------|-----|----------|
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

IF sales_order_module_view = TRUE  
AND contract_module_view = TRUE  
AND gma_module_view = TRUE  
AND quotation_module_view = TRUE  
AND customer_module_view = TRUE  
AND product_module_view = TRUE  
AND tax_module_view = TRUE  

THEN  
    Show Dashboard  

ELSE  
    Hide Dashboard  
    Show Message: "You don't have permission to see the dashboard."

---

# Final Rule

Sales Order Dashboard will only be visible when user has:

- Sales Order Module Permission
- Contract Module Permission
- GMA Module Permission
- Quotation Module Permission
- Customer Module Permission
- Product Module Permission
- Tax Module Permission

Otherwise dashboard will be hidden.
