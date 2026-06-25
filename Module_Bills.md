# Module: Bills (Money Going Out)

The Bills Dashboard gives business owners a simple view of all expenses they need to pay. It tracks upcoming bills, overdue payments, and helps manage outgoing cash flow without accounting jargon.

---

## 1. Executive Overview

- Centralizes all vendor bills and upcoming expenses
- Highlights overdue bills that need immediate payment
- Tracks monthly spending trends
- Helps avoid late fees by managing due dates effectively

---

## 2. Key Performance Indicators (KPIs)

| KPI Name | Business Meaning | Source Table | Aggregation Logic |
|----------|----------------|-------------|------------------|
| Total Unpaid Bills | Total money you owe to vendors | bills | SUM(amount) WHERE status = 'UNPAID' |
| Overdue Bills | Money owed past the due date | bills | SUM(amount) WHERE due_date < CURRENT_DATE AND status = 'UNPAID' |
| Bills Paid This Month | Total expenses paid in the current month | bills | SUM(amount) WHERE paid_date within THIS_MONTH |
| Upcoming Bills (Next 7 Days) | Money you need to pay very soon | bills | SUM(amount) WHERE due_date within NEXT_7_DAYS AND status = 'UNPAID' |

---

## 3. Strategic Visualizations (Charts)

### 1. Spending by Category (Donut Chart)
- X-axis: expense_category  
- Y-axis: SUM(amount)  
- Purpose: See where the money is going
- Source: bills

### 2. Upcoming vs Overdue Bills (Bar Chart)
- X-axis: status (Upcoming/Overdue)  
- Y-axis: SUM(amount)  
- Purpose: Track urgent payment needs vs planned payments
- Source: bills

### 3. Monthly Spending Trend (Line Chart)
- X-axis: month  
- Y-axis: SUM(amount)  
- Purpose: Monitor if expenses are increasing or decreasing
- Source: bills

---

## 4. Operational Data Tables

### 1. Urgent: Overdue Bills
- Columns:
  - id  
  - vendor_name  
  - amount_owed  
  - days_late  
- Source: bills  
- Purpose: Pay immediately to avoid late fees  

### 2. Bills to Pay Soon
- Columns:
  - id  
  - vendor_name  
  - amount  
  - due_date  
- Source: bills  
- Purpose: Plan cash for upcoming expenses  

---

## 5. Proactive Business Alerts

### 1. High Value Bill Overdue
-Purpose: Avoid service shutoffs or large late fees.
```sql
IF amount > 2000 AND due_date < CURRENT_DATE
THEN TRIGGER ALERT;
```

### 2. Unusual Spending Spike
-Purpose: Catch unexpected high expenses quickly.
```sql
IF bill_amount > (AVG(bill_amount) * 2)
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
