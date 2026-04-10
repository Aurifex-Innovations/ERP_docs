
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

### 1. Quotes by Status
- **Shows:** Distribution across Draft, Sent, Accepted, Rejected, Expired  
- **Why it matters:** Identifies bottlenecks in sales pipeline  
- **X-axis:** status  
- **Y-axis:** COUNT(id)
- **** COUNT(id)  
- **Chart Type:** Funnel Chart  

---

### 2. Monthly Sales Trend
- **Shows:** Total quotation value over 12 months  
- **Why it matters:** Identifies seasonal demand patterns  
- **X-axis:** Month(created_at)  
- **Y-axis:** SUM(grand_total)  
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
- **Chart Type:** Pie Chart  

---

## 4. Operational Data Tables

### 1. Recent High Value Quotes
- **Columns:**
  - quotation_number  
  - source_type  
  - grand_total  
  - status  
  - created_at  

- **Source:** quotations  

- **Purpose:**
  - Shows top 10 recent quotes with value > ₹25,000  
  - Helps prioritize high-value deals  

---

### 2. Quotes Expiring Soon
- **Columns:**
  - quotation_number  
  - valid_till  
  - grand_total  
  - users.first_name  

- **Source:**
  - quotations  
  - users  

- **Purpose:**
  - Identifies quotes expiring within 3 days  
  - Enables quick follow-up actions  

---

## 5. Proactive Alerts

| Alert Name | Purpose | Trigger Condition |
|------------|--------|------------------|
| Low Acceptance Alert | Detects poor pricing strategy | COUNT('ACCEPTED') / COUNT(*) < 0.20 (last 30 days) |
| High Value Pending | Flags high-value stalled deals | status = 'SENT' AND grand_total > ₹50,000 AND (now - sent_at) > 7 days |
| Critical Expiry Alert | Prevents quote expiration | status = 'SENT' AND valid_till = today + 1 day |

---

## Summary

Module 16 ensures:
- Financial discipline in quotation approvals  
- Visibility into sales pipeline performance  
- Proactive management through alerts and analytics  

---
