# Module: Payments (Cash Movement)

The Payments Dashboard tracks actual money moving in and out of the business bank accounts. It helps owners see their real cash position without confusing ledger terms.

---

## 1. Executive Overview

- Shows real-time cash balance based on actual movements
- Tracks total money received vs money spent
- Identifies failed or bounced payments quickly
- Reconciles expected money with actual bank deposits

---

## 2. Key Performance Indicators (KPIs)

| KPI Name | Business Meaning | Source Table | Aggregation Logic |
|----------|----------------|-------------|------------------|
| Total Cash In | All money successfully received | payments | SUM(amount) WHERE direction = 'IN' AND status = 'COMPLETED' |
| Total Cash Out | All money successfully spent | payments | SUM(amount) WHERE direction = 'OUT' AND status = 'COMPLETED' |
| Net Cash Flow | Cash In minus Cash Out | payments | (SUM(Cash In) - SUM(Cash Out)) |
| Failed Payments | Number of payments that didn't go through | payments | COUNT(id) WHERE status = 'FAILED' |

---

## 3. Strategic Visualizations (Charts)

### 1. Money In vs Money Out (Bar Chart)
- X-axis: month  
- Y-axis: SUM(amount) by direction  
- Purpose: See if making more than spending
- Source: payments

### 2. Payment Methods (Donut Chart)
- X-axis: payment_method (Credit Card, Bank Transfer, etc.)  
- Y-axis: SUM(amount)  
- Purpose: Understand how customers prefer to pay
- Source: payments

### 3. Daily Cash Balance (Line Chart)
- X-axis: date  
- Y-axis: running_balance  
- Purpose: Ensure there is always enough cash on hand
- Source: payments

---

## 4. Operational Data Tables

### 1. Recent Cash Received
- Columns:
  - id  
  - from_who  
  - amount  
  - date_received  
  - method  
- Source: payments  
- Purpose: Verify customer payments arrived  

### 2. Failed Transactions
- Columns:
  - id  
  - target_name  
  - amount  
  - direction  
  - failure_reason  
- Source: payments  
- Purpose: Follow up on blocked or bounced money  

---

## 5. Proactive Business Alerts

### 1. Payment Failed
-Purpose: Quickly retry or contact the person when money doesn't move.
```sql
IF status = 'FAILED'
THEN TRIGGER ALERT;
```

### 2. Negative Cash Flow Warning
-Purpose: Alert if spending outpaces earning for the week.
```sql
IF total_cash_out > total_cash_in for last 7 days
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
