# Module 13: Vendor Management

The Vendor Management Dashboard provides a complete view of the organization’s supply chain by combining vendor master data with operational supply metrics. It enables procurement transparency, performance tracking, and risk mitigation.

---

## 1. Executive Overview

- Centralizes supplier data and contract details  
- Tracks vendor performance and delivery efficiency  
- Helps in cost control and risk management  
- Supports financial forecasting using contract and billing data  

---

## 2. Key Performance Indicators (KPIs)

| KPI Name | Business Meaning | Source Table | Aggregation Logic |
|----------|----------------|-------------|------------------|
| Total Active Vendors | Current operational suppliers | vendors | COUNT(id) WHERE vendor_status = 'ACTIVE' |
| Average Vendor Rating | Supplier quality score | vendors | AVG(vendor_rating) |
| Expiring Vendor Contracts | Contracts ending soon | vendors | COUNT(id) WHERE contract_end_date within 30 days AND has_contract = true |
| Average Delivery Time | Supplier delivery efficiency | vendor_product_supplies | AVG(delivery_lead_time_days) |

---

## 3. Strategic Visualizations (Charts)

### 1. Vendors by Category (Donut Chart)
- X-axis: vendor_category  
- Y-axis: COUNT(id)  
- Purpose: Identify supplier distribution
- Source: vendors

### 2. Contract Status Split (Bar Chart)
- X-axis: has_contract (True/False)  
- Y-axis: COUNT(id)  
- Purpose: Track contract vs ad-hoc vendors
- Source: vendors

### 3. Rating Distribution (Column Chart)
- X-axis: vendor_rating (1–5)  
- Y-axis: COUNT(id)  
- Purpose: Identify low-performing vendors
- Source: vendors 

---

## 4. Operational Data Tables

### 1. Recent Vendor Additions
- Columns:
  - id  
  - vendor_name  
  - vendor_type  
  - created_at  
  - vendor_status  
- Source: vendors  
- Purpose: Audit new vendors  

### 2. Active Contract List
- Columns:
  - id  
  - vendor_name  
  - contract_type  
  - contract_end_date  
  - payment_terms  
- Source: vendors  
- Purpose: Manage renewals and payments  

---

## 5. Proactive Business Alerts

### 1. Low Vendor Rating
-Purpose: Notify procurement when a vendor's performance falls below acceptable levels.
```sql
IF vendor_rating < 2
THEN TRIGGER ALERT;
```

### 2. Contract Expiring Soon
-Purpose: Prevent the loss of negotiated rates or service coverage.
```sql
IF contract_end_date within next 30 days
THEN TRIGGER ALERT;
```


---
