# Charts

## 📈 1. Country Revenue Trend
### 📌 Description

Shows overall revenue growth over time across all branches using invoice data.

### 📊 Chart Type

Line Chart

### 📊 Axis Configuration
- X-Axis: invoice_date
- Y-Axis: SUM(grand_total)
- Legend: None

### 🗂 Data Table Name
sales_invoices

### 🧾 SQL Query
```
SELECT 
    invoice_date,
    SUM(grand_total) AS total_revenue
FROM sales_invoices
GROUP BY invoice_date
ORDER BY invoice_date;
```

## 📊 2. Branch Revenue Trend
### 📌 Description

Displays revenue comparison across branches over time.

### 📊 Chart Type

Line Chart 

### 📊 Axis Configuration
- X-Axis: invoice_date
- Y-Axis: SUM(grand_total)
- Legend: branch_id

### 🗂 Data Table Name
sales_invoices

### 🧾 SQL Query
```
SELECT 
    branch_id,
    invoice_date,
    SUM(grand_total) AS branch_revenue
FROM sales_invoices
GROUP BY branch_id, invoice_date
ORDER BY invoice_date;
```

## 📊 3. Revenue Breakup (Service vs Product)

### 📌 Description

Breaks revenue into Service and Product categories.

### 📊 Chart Type

Stacked Column Chart

### 📊 Axis Configuration
- X-Axis: item_type
- Y-Axis: SUM(line_total)
- Legend: item_type

### 🗂 Data Table Name
sales_invoice_lines

### 🧾 SQL Query
```
SELECT 
    item_type,
    SUM(line_total) AS revenue
FROM sales_invoice_lines
GROUP BY item_type;
```


## 📈 4. Technician Productivity Trend

### 📌 Description

Measures productivity based on tasks completed by technicians.

### 📊 Chart Type

Bar Chart

### Axis:
- X-Axis: primary_technician_id
- Y-Axis: productivity
 -Tooltip:
    - total_tasks_completed
    - total_revenue


### 🗂 Data Table Name
- tasks
- sales_invoices

🧾 SQL Query
```
SELECT 
    t.primary_technician_id,
    COUNT(t.id) AS total_tasks_completed,
    SUM(si.grand_total) AS total_revenue,
    CASE 
        WHEN COUNT(t.id) = 0 THEN 0
        ELSE SUM(si.grand_total) / COUNT(t.id)
    END AS productivity
FROM tasks t
LEFT JOIN sales_invoices si 
    ON si.sales_order_id = t.sales_order_id
WHERE t.status = 'COMPLETED'
GROUP BY t.primary_technician_id;
```


## 📊 5. Chemical Consumption Trend
### 📌 Description

Tracks consumption of materials/products used in services.

### 📊 Chart Type

Column Chart

### 📊 Axis Configuration
- X-Axis: product_code
- Y-Axis: SUM(consumable_qty)
- Legend: None

### 🗂 Data Table Name
stock_ledger

### 🧾 SQL Query
```
SELECT 
    product_code,
    SUM(consumable_qty) AS total_consumption
FROM stock_ledger
GROUP BY product_code;
```

## 📊 6. Collection vs Outstanding
### 📌 Description

Shows collected vs pending revenue.

### 📊 Chart Type

Clustered Bar Chart

### 📊 Axis Configuration
- X-Axis: Static (Collected vs Outstanding)
- Y-Axis: Amount
- Legend: Metric Type

### 🗂 Data Table Name
sales_invoices

### 🧾 SQL Query
```
SELECT 
    SUM(received_amount) AS collected,
    SUM(pending_amount) AS outstanding
FROM sales_invoices;
```

## 📊 7. Employee Growth (Onboarding)
### 📌 Description

Tracks employee onboarding over time.

### 📊 Chart Type

Line Chart

### 📊 Axis Configuration
- X-Axis: date_of_joining
- Y-Axis: COUNT(id)
- Legend: None

### 🗂 Data Table Name
users

### 🧾 SQL Query
```
SELECT 
    date_of_joining,
    COUNT(id) AS total_employees
FROM users
GROUP BY date_of_joining
ORDER BY date_of_joining;
```


## 📊 8. Invoice Status Breakdown
### 📌 Description

Shows distribution of invoice statuses (Paid, Pending, Overdue).

### 📊 Chart Type

Pie Chart

### 📊 Axis Configuration
- Category: status
- Legend: status

### 🗂 Data Table Name
sales_invoices

### 🧾 SQL Query
```
SELECT 
    status,
    COUNT(*) AS total
FROM sales_invoices
GROUP BY status;
```


# 📋 TABLE DOCUMENTATION
##📋 1. Revenue Summary Table
### 📌 Description

Displays total revenue per branch.

### 🧾 Table Headers
- branch_id
- total_revenue

### 🗂 Data Table Name
sales_invoices

### 🧾 SQL Query
```
SELECT 
    branch_id,
    SUM(grand_total) AS total_revenue
FROM sales_invoices
GROUP BY branch_id;
```


## 📋 2. Task Summary Table
### 📌 Description

Shows task status distribution.

### 🧾 Table Headers
- status
- total_tasks

### 🗂 Data Table Name
tasks

### 🧾 SQL Query
```
SELECT 
    status,
    COUNT(*) AS total_tasks
FROM tasks
GROUP BY status;
```

## 📋 3. Customer Revenue Table

### 📌 Description

Displays revenue generated per customer.

### 🧾 Table Headers
- customer_id
- total_revenue


### 🗂 Data Table Name
sales_invoices

### 🧾 SQL Query
```
SELECT 
    customer_id,
    SUM(grand_total) AS total_revenue
FROM sales_invoices
GROUP BY customer_id;
```

## 📋 4. Invoice Detail Table
### 📌 Description

Detailed invoice-level data.

### 🧾 Table Headers
- invoice_number
- customer_id
- invoice_date
- grand_total
- status


### 🗂 Data Table Name
sales_invoices

### 🧾 SQL Query
```
SELECT 
    invoice_number,
    customer_id,
    invoice_date,
    grand_total,
    status
FROM sales_invoices;
```

## 📋 5. Product Consumption Table
### 📌 Description

Shows consumption of products.

### 🧾 Table Headers
- product_code
- consumable_qty
  
### 🗂 Data Table Name
Stock tables

### 🧾 SQL Query
```
SELECT 
    product_code,
    consumable_qty
FROM stock;
```

## 📋 6. Vendor Outstanding Summary
### 📌 Description

#### Displays vendor-wise financial summary including:

- Total purchase value (Grand Total)
- Amount paid
- Pending (outstanding) amount

Used by finance team to track accounts payable.

### 🧾 Table Headers
- vendor_name
- paid_amount
- pending_amount
- grand_total
  
### 🗂 Data Table Name
-vendors
-purchase_order

### 🧾 SQL Query
```
SELECT 
    v.vendor_name,
    0 AS paid_amount,
    SUM(po.grand_total) AS pending_amount,
    SUM(po.grand_total) AS grand_total
FROM vendors v
LEFT JOIN purchase_order po 
    ON po.vendor_id = v.id
WHERE po.is_deleted = FALSE
GROUP BY v.vendor_name
ORDER BY v.vendor_name;
```

