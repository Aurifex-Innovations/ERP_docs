# Module: Invoices (Money Coming In)

The Invoices Dashboard gives business owners a clear view of money owed by customers. It tracks unpaid amounts, overdue payments, and helps predict cash flow.

---

## 1. Executive Overview

- Shows all money owed by customers in one place
- Highlights customers who are late on payments
- Tracks how fast money is being collected
- Helps predict future cash coming into the business

---

## 2. Key Performance Indicators (KPIs)

| KPI Name | Business Meaning | Source Table | Aggregation Logic |
|----------|----------------|-------------|------------------|
| Total Unpaid Money | Total money customers owe right now | invoices | SUM(amount) WHERE status = 'UNPAID' |
| Overdue Money | Money that is past its due date | invoices | SUM(amount) WHERE due_date < CURRENT_DATE AND status = 'UNPAID' |
| Money Collected This Month | Cash received from customers this month | invoices | SUM(amount_paid) WHERE paid_date within THIS_MONTH |
| Average Days to Pay | How fast customers usually pay | invoices | AVG(paid_date - issue_date) WHERE status = 'PAID' |

---

## 3. Strategic Visualizations (Charts)

### 1. Money Expected vs. Money Overdue (Bar Chart)
- X-axis: month  
- Y-axis: SUM(amount)  
- Purpose: Compare what is owed now vs what is late
- Source: invoices

### 2. Top 5 Customers Who Owe Money (Donut Chart)
- X-axis: customer_name  
- Y-axis: SUM(amount)  
- Purpose: Visual breakdown of biggest unpaid amounts
- Source: invoices

### 3. Collection Trend (Line Chart)
- X-axis: month  
- Y-axis: AVG(days_to_pay)  
- Purpose: Show if customers are paying faster or slower over time
- Source: invoices

---

## 4. Operational Data Tables

### 1. Urgent: Overdue Customers
- Columns:
  - id  
  - customer_name  
  - amount_owed  
  - days_late  
- Source: invoices  
- Purpose: Take action and send reminders  

### 2. Coming Due Soon
- Columns:
  - id  
  - customer_name  
  - amount  
  - due_date  
- Source: invoices  
- Purpose: Track upcoming money expected  

---

## 5. Proactive Business Alerts

### 1. Large Invoice Very Late
-Purpose: Notify when a big payment is missing for a long time.
```sql
IF amount > 5000 AND days_late > 30
THEN TRIGGER ALERT;
```

### 2. First Payment Bounced
-Purpose: Catch new customer payment issues early.
```sql
IF payment_status = 'BOUNCED' AND customer_type = 'NEW'
THEN TRIGGER ALERT;
```

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
