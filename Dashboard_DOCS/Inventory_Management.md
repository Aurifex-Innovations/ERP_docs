# Inventory Dashboard

Based on Module 10 (Inventory Products) and Module 11 (Stock Management)

---

# 1. KPI Cards

## 1. Total Products

### Tables Used
inventory_products

### Fields
status,
deleted_at,
id

### Calculation

SELECT COUNT(id)
FROM inventory_products
WHERE deleted_at IS NULL;

### KPI Name
Total Products

---

## 2. Active Products

SELECT COUNT(id)
FROM inventory_products
WHERE status = 'ACTIVE'
AND deleted_at IS NULL;

---

## 3. Total Stock

### Tables Used
stock_ledger

### Fields
assets_qty
consumable_qty
resell_qty

### Calculation

SELECT 
SUM(assets_qty + consumable_qty + resell_qty) AS total_stock
FROM stock_ledger
WHERE deleted_at IS NULL;

---

## 4. Total Inventory Value

### Tables Used
stock_ledger
inventory_products

### Fields
assets_qty
consumable_qty
resell_qty
purchase_price

### Calculation
```
SELECT 
SUM(
(stock_ledger.assets_qty +
 stock_ledger.consumable_qty +
 stock_ledger.resell_qty)
 * inventory_products.purchase_price
) AS inventory_value
FROM stock_ledger
JOIN inventory_products
ON stock_ledger.product_id = inventory_products.id;
```
---

## 5. Total Assets

SELECT SUM(assets_qty)
FROM stock_ledger;

---

## 6. Total Consumables

SELECT SUM(consumable_qty)
FROM stock_ledger;

---

## 7. Total Resell Stock

SELECT SUM(resell_qty)
FROM stock_ledger;

---

## 8. In Transit Stock

SELECT SUM(in_transit_qty)
FROM stock_ledger;

---

## 9. Reserved Stock

SELECT SUM(reserved_qty)
FROM stock_ledger;

---

## 10. Low Stock Products

SELECT COUNT(*)
FROM stock_ledger
WHERE status = 'LOW';

---

## 11. Out of Stock

SELECT COUNT(*)
FROM stock_ledger
WHERE status = 'OUT';

---

# 2. Charts

---

## Chart 1: Stock by Category

### Type
Pie Chart

### Tables
stock_ledger

### Fields
category
assets_qty
consumable_qty
resell_qty

### Query

SELECT 
category,
SUM(assets_qty + consumable_qty + resell_qty) AS total_stock
FROM stock_ledger
GROUP BY category;

---

## Chart 2: Stock by Type

### Type
Donut Chart

### Query

SELECT 
SUM(assets_qty) AS assets,
SUM(consumable_qty) AS consumables,
SUM(resell_qty) AS resell
FROM stock_ledger;

---

## Chart 3: Branch-wise Stock

### Type
Bar Chart

### Fields
branch_id

### Query

SELECT 
branch_id,
SUM(assets_qty + consumable_qty + resell_qty) AS total_stock
FROM stock_ledger
GROUP BY branch_id;

---

## Chart 4: Stock Movement Trend

### Type
Line Chart

### Table
stock_movement_logs

### Fields
created_at
quantity_delta

### Query

SELECT 
DATE(created_at) AS date,
SUM(quantity_delta) AS movement
FROM stock_movement_logs
GROUP BY DATE(created_at)
ORDER BY date;

---

## Chart 5: Inventory Value by Category

### Tables
stock_ledger
inventory_products

### Query

SELECT 
sl.category,
SUM(
(sl.assets_qty + sl.consumable_qty + sl.resell_qty)
* ip.purchase_price
) AS value
FROM stock_ledger sl
JOIN inventory_products ip
ON sl.product_id = ip.id
GROUP BY sl.category;

---

# 3. Tables

---

## Table 1: Low Stock Products

### Tables
stock_ledger
inventory_products

### Fields

product_name
product_code
branch_id
category
assets_qty
consumable_qty
resell_qty
status

### Query

SELECT 
sl.product_name,
sl.product_code,
sl.branch_id,
sl.category,
sl.assets_qty,
sl.consumable_qty,
sl.resell_qty,
sl.status
FROM stock_ledger sl
WHERE sl.status = 'LOW';

---

## Table 2: Out of Stock Products

SELECT 
product_name,
product_code,
branch_id,
category
FROM stock_ledger
WHERE status = 'OUT';

---

## Table 3: Branch Stock Table

SELECT 
branch_id,
product_name,
category,
assets_qty,
consumable_qty,
resell_qty,
in_transit_qty,
reserved_qty,
status
FROM stock_ledger;

---

## Table 4: Central Stock Entries

### Tables
central_stock_entries

SELECT 
entry_id,
product_name,
supplier_name,
invoice_number,
invoice_date,
total_qty,
assets_qty,
consumable_qty,
resell_qty,
total_with_tax,
created_at
FROM central_stock_entries;

---

## Table 5: Recent Stock Movements

SELECT 
reference_type,
reference_id,
product_id,
branch_id,
stock_type,
quantity_delta,
action,
created_by,
created_at
FROM stock_movement_logs
ORDER BY created_at DESC
LIMIT 20;

---

## Table 6: Stock Transfers

SELECT 
product_name,
assets_qty,
consumable_qty,
resell_qty,
source_branch_id
FROM stock_transfer_items;

---

# 4. Alerts

---

## Alert 1: Low Stock Alert

Condition

status = 'LOW'

Query

SELECT product_name, branch_id
FROM stock_ledger
WHERE status = 'LOW';

---

## Alert 2: Out of Stock

status = 'OUT'

SELECT product_name
FROM stock_ledger
WHERE status = 'OUT';

---

## Alert 3: High Reserved Stock

reserved_qty > 50

SELECT product_name, reserved_qty
FROM stock_ledger
WHERE reserved_qty > 50;

---

## Alert 4: High In Transit

in_transit_qty > 100

SELECT product_name, in_transit_qty
FROM stock_ledger
WHERE in_transit_qty > 100;

---

## Alert 5: Expired Consumables

Table

central_stock_entries

Fields

expiry_date

Query

SELECT product_name, expiry_date
FROM central_stock_entries
WHERE expiry_date < CURRENT_DATE;

---

# 5. Dashboard Layout

Row 1

Total Products
Total Stock
Inventory Value
Assets
Consumables
Resell

Row 2

Stock by Category
Stock by Type
Branch Stock

Row 3

Stock Movement
Inventory Value by Category

Row 4

Low Stock Table
Central Stock Entries
Recent Movements

Right Panel

Low Stock Alert
Out of Stock Alert
Reserved Stock Alert
In Transit Alert
Expiry Alert

---

# Data Relationship

inventory_products → stock_ledger → stock_movement_logs
inventory_products → central_stock_entries
stock_ledger → stock_transfer_items

# 🔐 Inventory Dashboard Permission Note

## Module Permission Rule

The Inventory Dashboard is primarily dependent on **Module 11 (Stock Management)**.

Although **Module 10 (Inventory Products)** contains product master data,  
Module 11 already stores product-related information such as:

- product_id
- product_name
- product_code
- category
- stock quantities
- branch stock
- stock movement

Therefore, dashboard visibility is controlled based on **Module 11 permissions**.

---

# Permission Logic

## Case 1

User has **VIEW permission for Module 11**  
User does **NOT have permission for Module 10**

### Result

Dashboard will be **VISIBLE**

### Reason

- Product data is already available in stock_ledger
- Stock movement and branch stock are part of Module 11
- Dashboard KPIs and charts are derived from Module 11 tables

### Conclusion

User can see the Inventory Dashboard.

---

## Case 2

User has **VIEW permission for Module 10**  
User does **NOT have permission for Module 11**

### Result

Dashboard will be **NOT VISIBLE**

### Reason

- Dashboard KPIs depend on stock_ledger
- Stock movement logs are part of Module 11
- Inventory value and branch stock come from Module 11
- Without Module 11, dashboard cannot be generated

### Message to Display

"You don't have permission to see the dashboards"

---

# Permission Matrix

| Module 10 Permission | Module 11 Permission | Dashboard Visibility |
|---------------------|---------------------|---------------------|
| ❌ No | ✅ Yes | ✅ Visible |
| ✅ Yes | ❌ No | ❌ Not Visible |
| ✅ Yes | ✅ Yes | ✅ Visible |
| ❌ No | ❌ No | ❌ Not Visible |

---

# Backend Permission Logic

## Condition

IF Module_11_View = TRUE  
    Dashboard = Visible

ELSE  
    Dashboard = Hidden  
    Show Message: "You don't have permission to see the dashboards"

---

# API / Backend Rule
