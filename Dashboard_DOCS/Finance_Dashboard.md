# Final Financial Dashboard Design (Aligned with Seravion Backend)

This document contains the final dashboard specifications for all four core financial modules. Each module is designed with business-friendly terminology while mapping directly to current backend entities (`SalesInvoice`, `PurchaseBill`, `Voucher`, `Ledger`, etc.).

---

# Module 1: Invoices (Money Coming In)

The Invoices Dashboard tracks unpaid amounts, overdue payments, and helps predict cash flow.

## 1. Executive Overview

- Shows all money owed by customers in one place
- Highlights customers who are late on payments
- Tracks how fast money is being collected
- Maps directly to the `SalesInvoice` and `InvoicePaymentAllocation` backend entities

---

## 2. Key Performance Indicators (KPIs)

| KPI Name | Business Meaning | Source Table | Aggregation Logic |
|----------|----------------|-------------|------------------|
| Total Unpaid Money | Total money customers owe right now | `SalesInvoice` | SUM(amount) WHERE status != 'PAID' |
| Overdue Money | Money that is past its due date | `SalesInvoice` | SUM(amount) WHERE dueDate < CURRENT_DATE AND status != 'PAID' |
| Money Collected This Month | Cash received from customers this month | `InvoicePaymentAllocation` | SUM(allocatedAmount) WHERE allocationDate within THIS_MONTH |
| Average Days to Pay | How fast customers usually pay | `SalesInvoice` | AVG(allocationDate - invoiceDate) |
| Invoices Issued This Month | Total value of new bills sent | `SalesInvoice` | SUM(amount) WHERE invoiceDate within THIS_MONTH |
| Bad Debt Risk | Money overdue for over 90 days | `SalesInvoice` | SUM(amount) WHERE dueDate < (CURRENT_DATE - 90 days) |

---

## 3. Strategic Visualizations (Charts)

### 1. Aging Buckets (Column Chart)
- X-axis: Age Bucket (1-30 days, 31-60 days, 60-90 days, 90+ days)  
- Y-axis: SUM(amount)  
- Purpose: Show how much debt is aging into high-risk territory
- Source: `SalesInvoice` grouped by age of `dueDate`

### 2. Money Expected vs. Money Overdue (Stacked Bar Chart)
- X-axis: Month  
- Y-axis: SUM(amount)  
- Purpose: Compare what is owed in the future vs what is already late
- Source: `SalesInvoice`

### 3. Top 5 Customers Who Owe Most (Donut Chart)
- X-axis: Customer / `SalesInvoiceSite`  
- Y-axis: SUM(amount)  
- Purpose: Identify revenue concentration risk
- Source: `SalesInvoice`

### 4. Collection Trend (Line Chart)
- X-axis: Month  
- Y-axis: AVG(days_to_pay)  
- Purpose: Show if customers are paying faster or slower over time
- Source: `SalesInvoice` joined with `InvoicePaymentAllocation`

---

## 4. Operational Data Tables

### 1. Urgent: Severely Overdue Customers
- Columns:
  - id  
  - customerName  
  - amountOwed  
  - daysLate  
- Source: `SalesInvoice`  
- Purpose: Targeted hit-list for the collections team  

### 2. Coming Due This Week
- Columns:
  - id  
  - customerName  
  - amount  
  - dueDate  
- Source: `SalesInvoice`  
- Purpose: Track upcoming money expected for near-term cash planning  

### 3. Recently Paid
- Columns:
  - id  
  - customerName  
  - allocatedAmount  
  - paymentDate  
- Source: `InvoicePaymentAllocation`  
- Purpose: Verify customer payments have cleared  

---

## 5. Proactive Business Alerts

### 1. Invoice 60+ Days Late
- Purpose: Notify when a significant payment reaches critical delinquency.
```sql
IF amount > 5000 AND days_late > 60
THEN TRIGGER ALERT;
```

### 2. First Payment Bounced
- Purpose: Catch new customer payment issues early.
```sql
IF paymentStatus = 'BOUNCED' AND isNewCustomer = true
THEN TRIGGER ALERT;
```

### 3. Top Customer Stopped Paying
- Purpose: Warns if a high-volume customer misses a major payment.
```sql
IF customerType = 'VIP' AND days_late > 15
THEN TRIGGER ALERT;
```

---
---

# Module 2: Bills (Money Going Out)

The Bills Dashboard centralizes all vendor expenses, highlights overdue payments, and tracks monthly spending trends.

## 1. Executive Overview

- Centralizes all vendor bills and upcoming expenses
- Helps avoid late fees by managing due dates
- Identifies unusual spending spikes quickly
- Maps directly to the `PurchaseBill` and `PurchaseBillLine` backend entities

---

## 2. Key Performance Indicators (KPIs)

| KPI Name | Business Meaning | Source Table | Aggregation Logic |
|----------|----------------|-------------|------------------|
| Total Unpaid Bills | Total money owed to vendors | `PurchaseBill` | SUM(amount) WHERE status != 'PAID' |
| Overdue Bills | Money owed past the due date | `PurchaseBill` | SUM(amount) WHERE dueDate < CURRENT_DATE AND status != 'PAID' |
| Upcoming Bills (Next 7 Days) | Cash needed to pay very soon | `PurchaseBill` | SUM(amount) WHERE dueDate within NEXT_7_DAYS AND status != 'PAID' |
| Total Expenses This Month | Total volume of money spent currently | `PurchaseBill` | SUM(amount) WHERE billDate within THIS_MONTH |
| Bills Paid This Month | Total expenses cleared current month | `PurchaseBill` | SUM(amount) WHERE status = 'PAID' AND paymentDate within THIS_MONTH |

---

## 3. Strategic Visualizations (Charts)

### 1. Spending by Category (Donut Chart)
- X-axis: expenseCategory  
- Y-axis: SUM(amount)  
- Purpose: Visually break down where money is going
- Source: `PurchaseBillLine`

### 2. Cash Need Forecast (Bar Chart)
- X-axis: Week  
- Y-axis: SUM(amount)  
- Purpose: Maps out required payments week-by-week
- Source: `PurchaseBill` grouped by `dueDate`

### 3. Top 5 Most Expensive Vendors (Column Chart)
- X-axis: Vendor  
- Y-axis: SUM(amount)  
- Purpose: Highlights suppliers taking the largest chunk of the budget
- Source: `PurchaseBill` grouped by vendorId

### 4. Monthly Spending Trend (Line Chart)
- X-axis: Month  
- Y-axis: SUM(amount)  
- Purpose: Show if the business is gradually spending more money over time
- Source: `PurchaseBill`

---

## 4. Operational Data Tables

### 1. Urgent: Bills to Pay Today
- Columns:
  - id  
  - vendorName  
  - amountOwed  
  - penaltyRisk  
- Source: `PurchaseBill`  
- Purpose: Pay immediately to avoid late fees or service cutoffs  

### 2. Bills Eligible for Discount
- Columns:
  - id  
  - vendorName  
  - amount  
  - discountDeadline  
- Source: `PurchaseBill`  
- Purpose: Optimize cash utilization by grabbing early payment savings  

### 3. Recurring Subscriptions
- Columns:
  - id  
  - vendorName  
  - averageMonthlyAmount  
- Source: `PurchaseBill`  
- Purpose: Spot and cancel unused software or services  

---

## 5. Proactive Business Alerts

### 1. Critical Service Bill Overdue
- Purpose: Avoid catastrophic shutoffs for essential services.
```sql
IF vendorType = 'CRITICAL' AND dueDate < CURRENT_DATE
THEN TRIGGER ALERT;
```

### 2. Unusual Spending Spike
- Purpose: Catch fraud or unauthorized employee spending.
```sql
IF billAmount > (AVG(billAmount) * 2)
THEN TRIGGER ALERT;
```

### 3. Duplicate Bill Detected
- Purpose: Prevent paying the same vendor invoice twice.
```sql
IF count(invoiceNumber) > 1 FOR SAME vendorId
THEN TRIGGER ALERT;
```

---
---

# Module 3: Payments (Cash Movement)

The Payments Dashboard tracks actual money moving in and out of the bank, stripping away paper invoices to show true liquidity.

## 1. Executive Overview

- Shows real-time cash balance based on actual movements
- Reconciles expected money with actual bank deposits
- Identifies failed or bounced payments
- Maps directly to `Voucher`, `VoucherAllocation`, and `InvoicePaymentAllocation`

---

## 2. Key Performance Indicators (KPIs)

| KPI Name | Business Meaning | Source Table | Aggregation Logic |
|----------|----------------|-------------|------------------|
| Total Cash In | Actual liquid cash hitting the bank | `Voucher` | SUM(amount) WHERE voucherType = 'RECEIPT' AND date within THIS_MONTH |
| Total Cash Out | Actual cash leaving the bank | `Voucher` | SUM(amount) WHERE voucherType = 'PAYMENT' AND date within THIS_MONTH |
| Net Cash Flow | Cash In minus Cash Out | `Voucher` | SUM(Cash In) - SUM(Cash Out) |
| Failed / Bounced Payments | Transactions rejected by banks | `Voucher` | COUNT(id) WHERE status = 'FAILED' |
| Unallocated Receipts | Money received but not matched to invoice | `VoucherAllocation` | SUM(unallocatedAmount) |
| Processing Fees Paid | Money lost to payment gateways | `Voucher` | SUM(feeAmount) WHERE voucherType = 'FEE' |

---

## 3. Strategic Visualizations (Charts)

### 1. Cash In vs Cash Out Trend (Double Line Chart)
- X-axis: Month  
- Y-axis: SUM(amount) by direction  
- Purpose: Track whether the business brings in more than it spends
- Source: `Voucher` totals grouped by month/direction

### 2. Payment Method Breakdown (Donut Chart)
- X-axis: paymentMode (Card, Bank, Check)  
- Y-axis: SUM(amount)  
- Purpose: Shows how customers prefer to pay
- Source: `Voucher`

### 3. Daily Cash Balance (Line Chart)
- X-axis: Date  
- Y-axis: Running Balance  
- Purpose: Ensure there is always enough cash on hand
- Source: `Voucher` rolling sum

### 4. Cash Flow Trajectory (Waterfall Chart)
- X-axis: Category (Starting Balance, Inflows, Outflows, Ending Balance)  
- Y-axis: Impact Amount  
- Purpose: Visually demonstrate how starting balance reached ending balance
- Source: `Voucher`

---

## 4. Operational Data Tables

### 1. Recent Transactions
- Columns:
  - id  
  - targetName  
  - amount  
  - direction  
  - date  
- Source: `Voucher`  
- Purpose: Audit log for all major spending and receiving  

### 2. Unreconciled / Unallocated Deposits
- Columns:
  - id  
  - targetName  
  - amount  
  - dateReceived  
- Source: `VoucherAllocation`  
- Purpose: Ensure accounting records perfectly match reality  

### 3. Failed Transactions
- Columns:
  - id  
  - targetName  
  - amount  
  - failureReason  
- Source: `Voucher`  
- Purpose: Follow up on blocked or bounced money immediately  

---

## 5. Proactive Business Alerts

### 1. Low Bank Balance Warning
- Purpose: Triggers when projected cash drops below a safety threshold.
```sql
IF runningBalance < 10000
THEN TRIGGER ALERT;
```

### 2. Payment Failed
- Purpose: Alert team to retry or contact customer when money doesn't move.
```sql
IF status = 'FAILED' AND direction = 'IN'
THEN TRIGGER ALERT;
```

### 3. Unexpected Large Withdrawal
- Purpose: Security alert protecting against embezzlement or hacking.
```sql
IF amount > 25000 AND direction = 'OUT' AND isUnplanned = true
THEN TRIGGER ALERT;
```

---
---

# Module 4: Financial Health (Overall Ledger)

The Financial Health Dashboard provides a high-level summary of profitability, assets, and liabilities without complex accounting jargon.

## 1. Executive Overview

- Gives a simple snapshot of overall business profitability
- Shows what the business owns vs what it owes
- Flags big changes in revenue or expenses easily
- Maps directly to `Ledger`, `LedgerEntry`, and `PostingLedgerBinding`

---

## 2. Key Performance Indicators (KPIs)

| KPI Name | Business Meaning | Source Table | Aggregation Logic |
|----------|----------------|-------------|------------------|
| Net Profit | Bottom line money made after all expenses | `LedgerEntry` | SUM(Income) - SUM(Expense) |
| Gross Profit Margin % | Percentage of revenue left after direct costs | `LedgerEntry` | (Gross Profit / Total Revenue) * 100 |
| Total Assets (What We Own) | Value of cash, inventory, receivables | `LedgerEntry` | SUM(balance) WHERE accountType = 'ASSET' |
| Total Liabilities (What We Owe) | Debt, loans, vendor payables | `LedgerEntry` | SUM(balance) WHERE accountType = 'LIABILITY' |
| Debt-to-Equity Ratio | How much of business is funded by debt | `LedgerEntry` | Total Liabilities / Total Equity |
| Monthly Operating Costs | Fixed costs required to keep lights on | `LedgerEntry` | SUM(balance) WHERE accountGroup = 'OPEX' |

---

## 3. Strategic Visualizations (Charts)

### 1. Profit vs Revenue Trend (Line Chart)
- X-axis: Month  
- Y-axis: Amount  
- Purpose: Prevents trap of growing revenue but shrinking profit
- Source: `LedgerEntry` aggregated by month for Income/Expense

### 2. Asset vs Liability Balance (Stacked Column Chart)
- X-axis: Category (Assets vs Liabilities)  
- Y-axis: SUM(balance)  
- Purpose: Shows if business owns significantly more than it owes
- Source: `LedgerEntry` totals

### 3. Top Expenses vs Top Income Sources (Tornado Chart)
- X-axis: Amount  
- Y-axis: Category Name  
- Purpose: Compares biggest revenue drivers directly against biggest cost centers
- Source: `LedgerEntry`

### 4. Income vs Expenses (Bar Chart)
- X-axis: Month  
- Y-axis: SUM(amount) by type  
- Purpose: Ensure the business is making money over time
- Source: `LedgerEntry`

---

## 4. Operational Data Tables

### 1. Highest Cost Centers
- Columns:
  - categoryName  
  - totalSpent  
  - percentOfTotalExpenses  
- Source: `LedgerEntry`  
- Purpose: Focus cost-cutting measures where they have most impact  

### 2. Most Profitable Segments
- Columns:
  - segmentName  
  - totalEarned  
  - netProfitContribution  
- Source: `LedgerEntry`  
- Purpose: Guides owner on where to focus expansion efforts  

### 3. Monthly Income Statement Summary
- Columns:
  - category  
  - monthValue  
  - yearToDateValue  
- Source: `LedgerEntry`  
- Purpose: Simplified P&L checkup without deep accounting reports  

---

## 5. Proactive Business Alerts

### 1. Dropping Profit Margin
- Purpose: Warns if the cost of doing business is creeping up faster than prices.
```sql
IF profitMargin < targetMargin
THEN TRIGGER ALERT;
```

### 2. Liability Exceeds Asset Warning
- Purpose: Critical red-flag indicating the business is technically insolvent.
```sql
IF totalLiabilities > totalAssets
THEN TRIGGER ALERT;
```

### 3. Tax Reserve Too Low
- Purpose: Warns if cash set aside is less than estimated quarterly tax bill.
```sql
IF taxReserveBalance < estimatedTaxLiability
THEN TRIGGER ALERT;
```

---

## 6. Global Filters for KPIs, Charts, and Tables

To provide dynamic data views, the dashboard uses global filters mapped to the backend API.

1. **Date Range Filter**: Custom date selection mapping to `start_date` and `end_date`.
2. **Time Period Filter**: A preset dropdown mapping to date ranges on the backend:
   - **Current**: `CURRENT_DATE`
   - **7 Days**: `CURRENT_DATE - INTERVAL '7 days'` to `CURRENT_DATE`
   - **1 Month**: `CURRENT_DATE - INTERVAL '1 month'` to `CURRENT_DATE`
   - **3 Months**: `CURRENT_DATE - INTERVAL '3 months'` to `CURRENT_DATE`
   - **6 Months**: `CURRENT_DATE - INTERVAL '6 months'` to `CURRENT_DATE`
   - **1 Year**: `CURRENT_DATE - INTERVAL '1 year'` to `CURRENT_DATE`
