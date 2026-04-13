
# Module 16: Quotation Management


---

## 1. Key Performance Indicators (KPIs)

| KPI Name | Business Meaning | Source Table | Source Columns |
|----------|----------------|--------------|----------------|
| Total Quotes | Total number of proposals generated | quotations | COUNT(id) |
| Accepted Quote Rate | Ratio of accepted quotes to total quotes | quotations | COUNT('ACCEPTED') / COUNT(*) |
| Total Quote Value | Total revenue potential of all quotes | quotations | SUM(grand_total) |
| Average Quote Value | Average value per quotation | quotations | AVG(grand_total) |

---

## 3. Data Visualization & Charts

### 1. Quotetion by Status
- **Shows:** Distribution across Draft, Sent, Accepted, Rejected, Expired  
- **Why it matters:** Identifies bottlenecks in sales pipeline  
- **X-axis:** status  
- **Y-axis:** COUNT(id)
- **Source Tables:** quotations  
- **Chart Type:** Funnel Chart  

---

### 2. Monthly Sales Trend
- **Shows:** Total quotation value over 12 months  
- **Why it matters:** Identifies seasonal demand patterns  
- **X-axis:** Month(created_at)  
- **Y-axis:** SUM(grand_total)
- **Source Tables:** quotations
- **Chart Type:** Line Chart  

---

### 3. Quotes by Branch
- **Shows:** Performance across different branches  
- **Why it matters:** Highlights branch efficiency  
- **X-axis:** branch_name  
- **Y-axis:** SUM(grand_total)  
- **Source Tables:** quotations, quotation_locations, branches  
- **Chart Type:** Stacked Bar Chart  

---

### 4. Quote Source Mix
- **Shows:** Source of quotations (Leads, Customers, Prospects)  
- **Why it matters:** Helps optimize marketing strategy  
- **X-axis:** source_type  
- **Y-axis:** COUNT(id)
- **Source Tables:** quotations
- **Chart Type:** Pie Chart  

---

## 4. Operational Data Tables

## 1. Recent High Value Quotes

### Meaning
Shows top 10 recent quotations with high value (greater than ₹25,000).

### Tables Used

quotations

---

### Table Headers

| Column Header | Database Field | Table |
|--------------|---------------|------|
| Quotation Number | quotation_number | quotations |
| Source Type | source_type | quotations |
| Total Amount | grand_total | quotations |
| Status | status | quotations |
| Created Date | created_at | quotations |

---

### SQL Query

```sql
SELECT 
quotation_number AS "Quotation Number",
source_type AS "Source Type",
grand_total AS "Total Amount",
status AS "Status",
created_at AS "Created Date"
FROM quotations
WHERE grand_total > 25000
AND is_deleted = FALSE
ORDER BY created_at DESC
LIMIT 10;
```
---

## 2. Quotes Expiring Soon

### Meaning
Shows quotations that will expire within the next 3 days.
### Tables Used

quotations
users
---

### Table Headers

| Column Header    | Database Field   | Table      |
| ---------------- | ---------------- | ---------- |
| Quotation Number | quotation_number | quotations |
| Valid Till       | valid_till       | quotations |
| Total Amount     | grand_total      | quotations |
| Created By       | first_name       | users      |


---

### SQL Query

```sql
SELECT 
q.quotation_number AS "Quotation Number",
q.valid_till AS "Valid Till",
q.grand_total AS "Total Amount",
u.first_name AS "Created By"
FROM quotations q
JOIN users u
ON q.created_by = u.id
WHERE q.valid_till BETWEEN CURRENT_DATE AND CURRENT_DATE + INTERVAL '3 days'
AND q.status = 'SENT'
AND q.is_deleted = FALSE
ORDER BY q.valid_till ASC;
```
---

## 5. Proactive Alerts

| Alert Name | Purpose | Trigger Condition | Source Tables |
|------------|--------|------------------|--------------|
| Low Acceptance Alert | Detects poor pricing strategy | COUNT('ACCEPTED') / COUNT(*) < 0.20 (last 30 days) | quotations |
| High Value Pending | Flags high-value stalled deals | status = 'SENT' AND grand_total > 50000 AND (CURRENT_DATE - sent_at) > 7 days | quotations |
| Critical Expiry Alert | Prevents quote expiration | status = 'SENT' AND valid_till = CURRENT_DATE + 1 | quotations |

---

## 6. Global Filters for KPIs and Charts and tables

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

Module 16 ensures:
- Financial discipline in quotation approvals  
- Visibility into sales pipeline performance  
- Proactive management through alerts and analytics  

---


# 🔐 Quotation Dashboard Permission Note

## Module Dependency

The Quotation Dashboard depends on:

- Lead Module
- Customer Module

Reason:

- Quotations can be created from Leads
- Quotations can be created from Customers
- Source Type is used in quotation (Lead / Customer)
- Dashboard shows quotation data based on source
- If user cannot access Lead or Customer data, dashboard will not show correct information

---

# Permission Logic

## Case 1

User has:

- Quotation Module Permission
- Lead Module Permission
- Customer Module Permission

### Result

Quotation Dashboard will be **VISIBLE**

### Reason

- User can see quotation data
- User can see lead-based quotations
- User can see customer-based quotations
- Source type data will load properly

---

## Case 2

User has:

- Quotation Module Permission
- BUT does not have Lead Module Permission
- OR does not have Customer Module Permission

### Result

Quotation Dashboard will be **NOT VISIBLE**

### Reason

- Quotations are created from Leads and Customers
- Source Type is used in dashboard
- User cannot access source data
- Dashboard will not show proper information

### Message

"You don't have permission to see the dashboard."

---

# Permission Matrix

| Quotation Module | Lead Module | Customer Module | Dashboard |
|-----------------|------------|----------------|----------|
| ✅ | ✅ | ✅ | ✅ Visible |
| ✅ | ❌ | ✅ | ❌ Not Visible |
| ✅ | ✅ | ❌ | ❌ Not Visible |
| ❌ | ❌ | ❌ | ❌ Not Visible |

---

# Backend Logic

IF quotation_module_view = TRUE  
AND lead_module_view = TRUE  
AND customer_module_view = TRUE  

THEN  
    Show Dashboard  

ELSE  
    Hide Dashboard  
    Show Message: "You don't have permission to see the dashboard."

---

# Final Rule

Quotation Dashboard will only be visible when user has:

- Quotation Module Permission
- Lead Module Permission
- Customer Module Permission

Otherwise dashboard will be hidden.
