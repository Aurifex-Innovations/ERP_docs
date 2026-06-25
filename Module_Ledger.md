# Module: Financial Health (Overall Business Health)

The Financial Health Dashboard provides a high-level summary of the entire business. It replaces complex "Chart of Accounts" and "Ledger" jargon with simple insights about profitability, assets, and liabilities.

---

## 1. Executive Overview

- Gives a simple snapshot of overall business profitability
- Shows what the business owns vs what it owes
- Tracks long-term financial growth and stability
- Flags big changes in revenue or expenses easily

---

## 2. Key Performance Indicators (KPIs)

| KPI Name | Business Meaning | Source Table | Aggregation Logic |
|----------|----------------|-------------|------------------|
| Total Profit | Total income minus total expenses | ledger | SUM(credit) - SUM(debit) WHERE account_type = 'INCOME/EXPENSE' |
| What We Own (Assets) | Total value of cash, inventory, and receivables | ledger | SUM(balance) WHERE account_type = 'ASSET' |
| What We Owe (Liabilities) | Total debt, loans, and payables | ledger | SUM(balance) WHERE account_type = 'LIABILITY' |
| Profit Margin % | Percentage of revenue that is profit | ledger | (Total Profit / Total Income) * 100 |

---

## 3. Strategic Visualizations (Charts)

### 1. Income vs Expenses (Bar Chart)
- X-axis: month  
- Y-axis: SUM(amount) by type  
- Purpose: Ensure the business is making money over time
- Source: ledger

### 2. What We Own vs What We Owe (Pie Chart)
- X-axis: category (Assets vs Liabilities)  
- Y-axis: SUM(balance)  
- Purpose: Check overall financial safety net
- Source: ledger

### 3. Profit Trend (Line Chart)
- X-axis: month  
- Y-axis: calculated_profit  
- Purpose: Track business growth journey
- Source: ledger

---

## 4. Operational Data Tables

### 1. Biggest Expense Categories
- Columns:
  - category_name  
  - total_spent  
  - percent_of_total_expenses  
- Source: ledger  
- Purpose: Identify where to cut costs  

### 2. Biggest Income Sources
- Columns:
  - category_name  
  - total_earned  
  - percent_of_total_income  
- Source: ledger  
- Purpose: Identify most profitable parts of the business  

---

## 5. Proactive Business Alerts

### 1. Dropping Profit Margin
-Purpose: Alert owner if business is becoming less profitable.
```sql
IF profit_margin < 10%
THEN TRIGGER ALERT;
```

### 2. High Debt Level
-Purpose: Warn if debts grow faster than assets.
```sql
IF total_liabilities > (total_assets * 0.8)
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
