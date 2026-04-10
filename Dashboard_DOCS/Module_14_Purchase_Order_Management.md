# Module 14: Purchase Order Dashboard

This dashboard helps users easily understand:

- How much money is spent on purchases
- How many purchase orders are pending
- What items are ordered
- Which orders are delayed

Main Tables Used

purchase_order
purchase_order_item
vendors
branches
inventory_products

--------------------------------------------------

# 1. KPIs (4)

## KPI 1: Total Purchase Money

### Meaning
Total money spent on all purchase orders.

### Table Used
purchase_order

### Field Used
grand_total

### Calculation
```
SELECT SUM(grand_total)
FROM purchase_order
WHERE is_deleted = FALSE;
```
--------------------------------------------------

## KPI 2: Pending Purchase Orders

### Meaning
Number of purchase orders that are still not completed.

### Table Used
purchase_order

### Field Used
status

### Calculation
```
SELECT COUNT(id)
FROM purchase_order
WHERE status IN
(
'Pending Approval',
'Approved',
'Ordered',
'Partially Received'
)
AND is_deleted = FALSE;
```
--------------------------------------------------

## KPI 3: Total Items Purchased

### Meaning
Total number of items ordered from vendors.

### Table Used
purchase_order_item

### Field Used
quantity

### Calculation
```
SELECT SUM(quantity)
FROM purchase_order_item
WHERE is_deleted = FALSE;
```
--------------------------------------------------

## KPI 4: Late Delivery Orders

### Meaning
Orders that should have arrived but are still not received.

### Table Used
purchase_order

### Fields Used
delivery_date
status

### Calculation
```
SELECT COUNT(id)
FROM purchase_order
WHERE delivery_date < CURRENT_DATE
AND status != 'Received'
AND is_deleted = FALSE;
```
--------------------------------------------------

# 2. Charts (3)

## Chart 1: Purchase Orders by Status

### Meaning
Shows how many orders are pending, approved, ordered, or received.

### Table
purchase_order

### Calculation
```
SELECT status, COUNT(id)
FROM purchase_order
WHERE is_deleted = FALSE
GROUP BY status;
```
### Chart Type
Donut Chart

--------------------------------------------------

## Chart 2: Vendor Purchase Spending

### Meaning
Shows which vendor we spend most money on.

### Tables

purchase_order
vendors

### Calculation

SELECT 
v.vendor_name,
SUM(p.grand_total)
FROM purchase_order p
JOIN vendors v
ON p.vendor_id = v.id
WHERE p.is_deleted = FALSE
GROUP BY v.vendor_name
ORDER BY SUM(p.grand_total) DESC
LIMIT 10;

### Chart Type
Bar Chart

--------------------------------------------------

## Chart 3: Daily Purchase Orders

### Meaning
Shows how many purchase orders are created each day.

### Table
purchase_order

### Calculation
```
SELECT 
po_date,
COUNT(id)
FROM purchase_order
WHERE is_deleted = FALSE
GROUP BY po_date
ORDER BY po_date;
```
### Chart Type
Line Chart

--------------------------------------------------

# 3. Tables (2)

# 3. Tables (2)

## Table 1: Recent Purchase Orders

### Meaning
Shows latest purchase orders created in the system.

### Tables Used

purchase_order  
vendors  
branches  

---

### Table Headers (UI Display Names)

| Column Header | Database Field | Table |
|--------------|---------------|------|
| PO Number | po_number | purchase_order |
| Vendor Name | vendor_name | vendors |
| Purchase Date | po_date | purchase_order |
| Delivery Date | delivery_date | purchase_order |
| Total Amount | grand_total | purchase_order |
| Order Status | status | purchase_order |
| Branch Name | branch_name | branches |

---

### SQL Query

```sql
SELECT 
p.po_number AS "PO Number",
v.vendor_name AS "Vendor Name",
p.po_date AS "Purchase Date",
p.delivery_date AS "Delivery Date",
p.grand_total AS "Total Amount",
p.status AS "Order Status",
b.branch_name AS "Branch Name"
FROM purchase_order p
JOIN vendors v
ON p.vendor_id = v.id
JOIN branches b
ON p.branch_id = b.id
WHERE p.is_deleted = FALSE
ORDER BY p.created_at DESC
LIMIT 20;
```
--------------------------------------------------

## Table 2: Vendor Purchase Summary

### Meaning
Shows how much we purchased from each vendor and how many purchase orders they have.

### Tables Used

purchase_order  
vendors  
branches  

---

### Table Headers (UI Display Names)

| Column Header | Database Field | Table |
|--------------|---------------|------|
| Vendor Name | vendor_name | vendors |
| Branch Name | branch_name | branches |
| Total Orders | COUNT(p.id) | purchase_order |
| Total Purchase Amount | SUM(grand_total) | purchase_order |
| Last Purchase Date | MAX(po_date) | purchase_order |

---

### SQL Query

```sql
SELECT 
v.vendor_name AS "Vendor Name",
b.branch_name AS "Branch Name",
COUNT(p.id) AS "Total Orders",
SUM(p.grand_total) AS "Total Purchase Amount",
MAX(p.po_date) AS "Last Purchase Date"
FROM purchase_order p
JOIN vendors v
ON p.vendor_id = v.id
JOIN branches b
ON p.branch_id = b.id
WHERE p.is_deleted = FALSE
GROUP BY 
v.vendor_name,
b.branch_name
ORDER BY "Total Purchase Amount" DESC;
```
--------------------------------------------------

# 4. Alerts (2)

## Alert 1: Late Delivery Warning

### Meaning
Order delivery date is passed but item not received.

### Table
purchase_order

### Query

SELECT 
po_number,
delivery_date
FROM purchase_order
WHERE delivery_date < CURRENT_DATE
AND status != 'Received'
AND is_deleted = FALSE;

### Alert Message

Order delivery is late

--------------------------------------------------

## Alert 2: High Purchase Amount

### Meaning
Purchase order with very high amount.

### Table
purchase_order

### Query

SELECT 
po_number,
grand_total
FROM purchase_order
WHERE grand_total > 50000
AND is_deleted = FALSE;

### Alert Message

High value purchase order

--------------------------------------------------

# 🔐 Purchase Order Dashboard Permission Note

## Module Dependency

The Purchase Order Dashboard (Module 14) depends on **Module 13 (Vendors)**.

Reason:

- Purchase Orders are linked with vendors
- Vendor data is required to show:
  - Vendor Name
  - Vendor Spending
  - Purchase Orders
  - Vendor Summary
- Without vendor access, dashboard data cannot be properly displayed

Therefore, **Module 13 permission is mandatory** for viewing Module 14 dashboard.

---

# Permission Logic

## Case 1

User has **Module 13 (Vendors) View Permission**

### Result

Dashboard will be **VISIBLE**

### Reason

- Vendor data is accessible
- Purchase orders can be displayed
- Charts and tables can load correctly

---

## Case 2

User does NOT have **Module 13 (Vendors) View Permission**

### Result

Dashboard will be **NOT VISIBLE**

### Message

"You haven't permission to see the dashboard."

---

# Permission Matrix

| Module 13 (Vendors) Permission | Module 14 Dashboard |
|-------------------------------|--------------------|
| ✅ Yes | ✅ Visible |
| ❌ No | ❌ Not Visible |

---

# Backend Logic

## Condition

IF Module_13_View = TRUE  
    Dashboard = Visible

ELSE  
    Dashboard = Hidden  
    Show Message: "You haven't permission to see the dashboard."

---

