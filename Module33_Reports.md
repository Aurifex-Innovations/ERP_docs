# 🎯 MODULE 33: FINANCIAL REPORTS

## Overview

Financial Reports module generates all statutory and management accounting reports from the data accumulated across Module 28–32. Provides **Profit & Loss Statement**, **Balance Sheet**, **Cash Flow Statement**, **GST Reports (GSTR-1, GSTR-3B)**, **Ageing Analysis (Receivable & Payable)**, **Bank Reconciliation**, **TDS Reports**, and **Trial Balance**. All reports support date range filtering, branch filtering, comparative views (Year-on-Year), and export to PDF/Excel. Reports are generated real-time from Ledger data.

**Module Connections:**

- **Depends on:** Module 31 (Ledger — all transaction data), Module 32 (COA — report structure), Module 28 (Invoice data for GST reports), Module 29 (Bill data for ITC reports), Module 30 (Payment data for cash flow), Module 9 (Tax — GST rates for compliance)
- **Used by:** Company Admin Dashboard (Module 4.1 financial widgets), Management Decision-making
- **Prerequisites:** Modules 28–32 must have transactional data

---

The module contains the following screens:

- 33.1 Reports Dashboard (Landing)
- 33.2 Profit & Loss Statement (P&L)
- 33.3 Balance Sheet
- 33.4 Trial Balance
- 33.5 Cash Flow Statement
- 33.6 GST Reports (GSTR-1 & GSTR-3B)
- 33.7 Ageing Analysis (Receivable & Payable)
- 33.8 Bank Reconciliation
- 33.9 TDS Report

---

================================================================================

# 33.1 Reports Dashboard (Landing)

**Description:**
The landing screen for Module 33. Displays a **card-based layout** with quick access to all available reports. Each card shows the report name, icon, a brief description, and the last generated date. Also shows key financial health indicators as a summary strip.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       FINANCIAL REPORTS                                      │
│                                                                              │
│  FINANCIAL HEALTH (Current FY: Apr 2025 – Mar 2026)                          │
│  ┌──────────────┬──────────────┬──────────────┬──────────────┐               │
│  │ Revenue      │ Expenses     │ Net Profit   │ Cash Balance │               │
│  │ (This Month) │ (This Month) │ (This Month) │ (Current)    │               │
│  │ ₹ 18,50,000  │ ₹ 14,65,000  │ ₹ 3,85,000   │ ₹ 10,30,000  │               │
│  │ ▲ 12% vs LM  │ ▲ 5% vs LM   │ ▲ 45% vs LM  │              │               │
│  └──────────────┴──────────────┴──────────────┴──────────────┘               │
│                                                                              │
│  AVAILABLE REPORTS                                                           │
│  ┌──────────────────┬──────────────────┬──────────────────┐                  │
│  │ 📊 Profit & Loss │ 📋 Balance Sheet │ 📑 Trial Balance │                  │
│  │                  │                  │                  │                  │
│  │ Income vs        │ Assets, Liabilities│ Verify Debit = │                  │
│  │ Expenses summary │ & Capital snapshot│ Credit totals   │                  │
│  │                  │                  │                  │                  │
│  │ [Generate →]     │ [Generate →]     │ [Generate →]     │                  │
│  └──────────────────┴──────────────────┴──────────────────┘                  │
│                                                                              │
│  ┌──────────────────┬──────────────────┬──────────────────┐                  │
│  │ 💰 Cash Flow     │ 🧾 GST Reports   │ ⏰ Ageing Report │                  │
│  │                  │                  │                  │                  │
│  │ Money In vs      │ GSTR-1, GSTR-3B  │ Receivable &     │                  │
│  │ Money Out        │ compliance data  │ Payable aging    │                  │
│  │                  │                  │                  │                  │
│  │ [Generate →]     │ [Generate →]     │ [Generate →]     │                  │
│  └──────────────────┴──────────────────┴──────────────────┘                  │
│                                                                              │
│  ┌──────────────────┬──────────────────┐                                    │
│  │ 🏦 Bank          │ 📋 TDS Report    │                                    │
│  │ Reconciliation   │                  │                                    │
│  │                  │ TDS deducted &   │                                    │
│  │ Match bank       │ collected summary│                                    │
│  │ statement        │                  │                                    │
│  │ [Generate →]     │ [Generate →]     │                                    │
│  └──────────────────┴──────────────────┘                                    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Financial Health Fields

| Field         | Type   | Description                                              |
| ------------- | ------ | -------------------------------------------------------- |
| Revenue       | Number | Total income for the current month                       |
| Expenses      | Number | Total expenses for the current month                     |
| Net Profit    | Number | Revenue - Expenses                                       |
| Cash Balance  | Number | Sum of all Bank + Cash account balances                  |
| vs LM         | %      | Percentage change compared to Last Month                 |

---

## Report Cards

| Report              | Description                                           | Screen   |
| ------------------- | ----------------------------------------------------- | -------- |
| Profit & Loss       | Income vs Expenses for a period                       | 33.2     |
| Balance Sheet       | Assets, Liabilities, Capital snapshot                 | 33.3     |
| Trial Balance       | All ledger balances (Dr = Cr verification)            | 33.4     |
| Cash Flow           | Money In vs Money Out                                 | 33.5     |
| GST Reports         | GSTR-1, GSTR-3B compliance data                      | 33.6     |
| Ageing Analysis     | Receivable and Payable aging buckets                  | 33.7     |
| Bank Reconciliation | Match bank statement with book records                | 33.8     |
| TDS Report          | TDS deducted and collected summary                    | 33.9     |

---

================================================================================

# 33.2 Profit & Loss Statement (P&L)

**Description:**
Shows the company's **Income vs Expenses** for a selected period, resulting in **Net Profit or Net Loss**. Structured as per Indian accounting format with Direct Income, Direct Expenses (Gross Profit), Indirect Income, Indirect Expenses (Net Profit). Supports monthly/quarterly/yearly views and year-on-year comparison.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     PROFIT & LOSS STATEMENT                                  │
│                     For the period: 01 Apr 2025 to 28 Mar 2026               │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Period  : [📅 01 Apr 2025] to [📅 28 Mar 2026]                       │  │
│  │ Branch  : [▼ All Branches ▼]                                         │  │
│  │ Compare : [☐ Previous Year]                    [GENERATE]            │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────┬────────────┐  │
│  │ PARTICULARS                                               │ AMOUNT (₹) │  │
│  │══════════════════════════════════════════════════════════════════════════│  │
│  │ INCOME                                                    │            │  │
│  │ ────────────────────────────────────────────────────────────────────── │  │
│  │ A. Direct Income                                          │            │  │
│  │    Service Income (Pest Control)                          │ 18,50,000  │  │
│  │    Product Sales (Resell)                                 │  2,20,000  │  │
│  │                                                           │            │  │
│  │    Total Direct Income                                    │ 20,70,000  │  │
│  │ ────────────────────────────────────────────────────────────────────── │  │
│  │ B. Direct Expenses                                        │            │  │
│  │    Chemical Cost                                          │  4,50,000  │  │
│  │    Equipment Cost                                         │  1,20,000  │  │
│  │    Fuel & Transportation                                  │    80,000  │  │
│  │                                                           │            │  │
│  │    Total Direct Expenses                                  │  6,50,000  │  │
│  │ ══════════════════════════════════════════════════════════════════════ │  │
│  │ GROSS PROFIT (A - B)                                      │ 14,20,000  │  │
│  │ ══════════════════════════════════════════════════════════════════════ │  │
│  │                                                           │            │  │
│  │ C. Indirect Income                                        │            │  │
│  │    Interest Received                                      │    15,000  │  │
│  │    Other Income                                           │     5,000  │  │
│  │                                                           │            │  │
│  │    Total Indirect Income                                  │    20,000  │  │
│  │ ────────────────────────────────────────────────────────────────────── │  │
│  │ D. Indirect Expenses                                      │            │  │
│  │    Salaries & Wages                                       │  6,00,000  │  │
│  │    Rent                                                   │  1,50,000  │  │
│  │    Electricity                                            │    25,000  │  │
│  │    Office Supplies                                        │    15,000  │  │
│  │    Professional Fees                                      │    50,000  │  │
│  │    Depreciation                                           │    45,000  │  │
│  │                                                           │            │  │
│  │    Total Indirect Expenses                                │  8,85,000  │  │
│  │ ══════════════════════════════════════════════════════════════════════ │  │
│  │ NET PROFIT (Gross Profit + C - D)                         │  5,55,000  │  │
│  │ ══════════════════════════════════════════════════════════════════════ │  │
│  └──────────────────────────────────────────────────────────┴────────────┘  │
│                                                                              │
│  [📥 DOWNLOAD PDF]  [📊 DOWNLOAD EXCEL]  [🖨 PRINT]  [🔙 BACK]            │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Filters

| Filter  | Type       | Options                                               |
| ------- | ---------- | ----------------------------------------------------- |
| Period  | Date Range | Custom / Monthly / Quarterly / Yearly                 |
| Branch  | Dropdown   | All Branches / Specific Branch                        |
| Compare | Checkbox   | Compare with Previous Year (side-by-side columns)     |

---

## Report Fields

| Field                  | Type   | Description                                          |
| ---------------------- | ------ | ---------------------------------------------------- |
| Direct Income          | Total  | Sum of all "Direct Income" group ledgers             |
| Direct Expenses        | Total  | Sum of all "Direct Expenses" group ledgers           |
| Gross Profit           | Calc   | Direct Income - Direct Expenses                      |
| Indirect Income        | Total  | Sum of all "Indirect Income" group ledgers           |
| Indirect Expenses      | Total  | Sum of all "Indirect Expenses" group ledgers         |
| Net Profit / Loss      | Calc   | Gross Profit + Indirect Income - Indirect Expenses   |

---

## Actions

| Action              | Type   | Description                                     |
| ------------------- | ------ | ----------------------------------------------- |
| **Download PDF**    | Button | Download P&L statement as PDF                   |
| **Download Excel**  | Button | Export data as Excel for further analysis        |
| **Print**           | Button | Print formatted P&L statement                   |
| **Back**            | Button | Returns to Reports Dashboard (33.1)             |

---

================================================================================

# 33.3 Balance Sheet

**Description:**
Shows the company's **financial position at a specific date** — what it owns (Assets), what it owes (Liabilities), and the owner's equity (Capital). Follows the equation: **Assets = Liabilities + Capital**. Structured per Indian accounting format.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        BALANCE SHEET                                         │
│                        As on: 28 Mar 2026                                    │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ As on Date : [📅 28 Mar 2026]                                        │  │
│  │ Branch     : [▼ All Branches ▼]                    [GENERATE]        │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌──────────────────────────────────┬──────────────────────────────────┐    │
│  │ LIABILITIES & CAPITAL            │ ASSETS                           │    │
│  │══════════════════════════════════│══════════════════════════════════│    │
│  │                                  │                                  │    │
│  │ Capital Account                  │ Fixed Assets                     │    │
│  │   Owner's Capital    25,00,000   │   Office Equipment   2,00,000   │    │
│  │   Net Profit (P&L)   5,55,000   │   Vehicles           5,00,000   │    │
│  │   ─────────────────────────────  │   PC Machines(M11)   1,80,000   │    │
│  │   Total Capital      30,55,000   │   ─────────────────────────────  │    │
│  │                                  │   Total Fixed        8,80,000   │    │
│  │ Long-term Liabilities           │                                  │    │
│  │   Term Loan (Bank)  15,00,000   │ Current Assets                   │    │
│  │                                  │   Bank Accounts     10,80,000   │    │
│  │ Current Liabilities             │   Cash-in-Hand          50,000   │    │
│  │   Sundry Creditors   1,55,300   │   Sundry Debtors     12,50,000   │    │
│  │   GST Payable           38,000   │   Stock-in-Hand       3,50,000   │    │
│  │   TDS Payable            5,500   │   GST Input Credit     12,000   │    │
│  │   Salary Payable     2,50,000   │   ─────────────────────────────  │    │
│  │   ─────────────────────────────  │   Total Current     27,42,000   │    │
│  │   Total Current      4,48,800   │                                  │    │
│  │                                  │ Investments                      │    │
│  │                                  │   (None)                 —       │    │
│  │                                  │                                  │    │
│  │══════════════════════════════════│══════════════════════════════════│    │
│  │ TOTAL                50,03,800   │ TOTAL                50,03,800   │    │
│  │ (Liabilities+Capital)           │ (Total Assets)                   │    │
│  │                       ✅ Balanced│                                  │    │
│  └──────────────────────────────────┴──────────────────────────────────┘    │
│                                                                              │
│  [📥 DOWNLOAD PDF]  [📊 DOWNLOAD EXCEL]  [🖨 PRINT]  [🔙 BACK]            │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Filters

| Filter  | Type       | Options                                           |
| ------- | ---------- | ------------------------------------------------- |
| As on   | Date       | Snapshot date (default: today)                    |
| Branch  | Dropdown   | All Branches / Specific Branch                    |

---

## Report Sections

| Section              | Description                                           |
| -------------------- | ----------------------------------------------------- |
| Capital Account      | Owner's equity + Retained earnings (Net Profit from P&L) |
| Long-term Liabilities| Loans, debentures with > 1 year maturity              |
| Current Liabilities  | Creditors, taxes payable, short-term obligations      |
| Fixed Assets         | Property, equipment, vehicles (from Module 11 Assets) |
| Current Assets       | Bank, Cash, Debtors, Stock, prepaid                   |
| Investments          | Long-term investments (if any)                        |

---

## Business Rules

| Rule                     | Description                                              |
| ------------------------ | -------------------------------------------------------- |
| Must balance             | Total Assets must equal Total Liabilities + Capital      |
| P&L auto-linked          | Net Profit from P&L automatically flows to Capital       |
| Stock from Module 11     | Current stock value fetched from Module 11               |
| Depreciation applied     | Fixed asset values shown net of accumulated depreciation |

---

================================================================================

# 33.4 Trial Balance

**Description:**
Shows **all ledger balances** in a Debit and Credit column format. Used to verify that the books are balanced (Total Debits = Total Credits). Includes Opening Balance + Transactions = Closing Balance for each ledger.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          TRIAL BALANCE                                       │
│                          As on: 28 Mar 2026                                  │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Period : [📅 01 Apr 2025] to [📅 28 Mar 2026]                        │  │
│  │ Branch : [▼ All Branches ▼]                        [GENERATE]        │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌──────────────────────────┬──────────────────┬──────────────────────────┐  │
│  │ ACCOUNT NAME              │ DEBIT (Dr) ₹     │ CREDIT (Cr) ₹            │  │
│  │══════════════════════════╪══════════════════╪══════════════════════════│  │
│  │ HDFC Current A/C          │ 7,00,000         │ —                        │  │
│  │ SBI Savings A/C           │ 1,50,000         │ —                        │  │
│  │ Petty Cash                │ 50,000           │ —                        │  │
│  │ Sundry Debtors (Total)   │ 12,50,000        │ —                        │  │
│  │ Stock-in-Hand             │ 3,50,000         │ —                        │  │
│  │ Office Equipment          │ 2,00,000         │ —                        │  │
│  │ Vehicles                  │ 5,00,000         │ —                        │  │
│  │ PC Machines               │ 1,80,000         │ —                        │  │
│  │ Chemical Cost             │ 4,50,000         │ —                        │  │
│  │ Equipment Cost            │ 1,20,000         │ —                        │  │
│  │ Fuel & Transportation    │ 80,000           │ —                        │  │
│  │ Salaries & Wages          │ 6,00,000         │ —                        │  │
│  │ Rent                      │ 1,50,000         │ —                        │  │
│  │ Electricity               │ 25,000           │ —                        │  │
│  │ Office Supplies           │ 15,000           │ —                        │  │
│  │ Professional Fees         │ 50,000           │ —                        │  │
│  │ GST Input Credit          │ 12,000           │ —                        │  │
│  │ ─────────────────────────│──────────────────│──────────────────────── │  │
│  │ Sundry Creditors (Total)  │ —                │ 1,55,300                 │  │
│  │ GST Payable (CGST+SGST)  │ —                │ 38,000                   │  │
│  │ TDS Payable               │ —                │ 5,500                    │  │
│  │ Salary Payable            │ —                │ 2,50,000                 │  │
│  │ Term Loan                 │ —                │ 15,00,000                │  │
│  │ Service Income            │ —                │ 18,50,000                │  │
│  │ Product Sales             │ —                │ 2,20,000                 │  │
│  │ Interest Received         │ —                │ 15,000                   │  │
│  │ Other Income              │ —                │ 5,000                    │  │
│  │ Owner's Capital           │ —                │ 25,00,000                │  │
│  │══════════════════════════╪══════════════════╪══════════════════════════│  │
│  │ TOTAL                     │ 50,03,800        │ 50,03,800   ✅ Balanced │  │
│  └──────────────────────────┴──────────────────┴──────────────────────────┘  │
│                                                                              │
│  [📥 DOWNLOAD PDF]  [📊 DOWNLOAD EXCEL]  [🖨 PRINT]  [🔙 BACK]            │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table Fields

| Field        | Type   | Description                                              |
| ------------ | ------ | -------------------------------------------------------- |
| Account Name | Text   | Ledger name (grouped by account type)                    |
| Debit (Dr)   | Number | Closing debit balance of the ledger                      |
| Credit (Cr)  | Number | Closing credit balance of the ledger                     |

---

## Business Rules

| Rule                    | Description                                               |
| ----------------------- | --------------------------------------------------------- |
| Must balance            | Total Debit column must equal Total Credit column         |
| Imbalance warning       | If Dr ≠ Cr, system shows ❌ "Books are not balanced"      |
| Detailed / Condensed    | Toggle between individual ledgers and group totals        |

---

================================================================================

# 33.5 Cash Flow Statement

**Description:**
Shows the **movement of money** — all receipts (inflows) and payments (outflows) grouped by Operating, Investing, and Financing activities. Sourced from Module 30 (Payment Register).

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       CASH FLOW STATEMENT                                    │
│                       01 Apr 2025 to 28 Mar 2026                             │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Period : [📅 From] to [📅 To]     Branch: [▼ All ▼]     [GENERATE]   │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────┬────────────┐  │
│  │ PARTICULARS                                               │ AMOUNT (₹) │  │
│  │══════════════════════════════════════════════════════════════════════════│  │
│  │ A. Cash Flow from Operating Activities                    │            │  │
│  │    Net Profit (from P&L)                                  │  5,55,000  │  │
│  │    (+) Depreciation                                       │    45,000  │  │
│  │    (+) Increase in Creditors                              │  1,55,300  │  │
│  │    (-) Increase in Debtors                                │ -12,50,000 │  │
│  │    (-) Increase in Stock                                  │  -3,50,000 │  │
│  │    ─────────────────────────────────────────────────────────────────── │  │
│  │    Net Cash from Operations                               │ -8,44,700  │  │
│  │                                                           │            │  │
│  │ B. Cash Flow from Investing Activities                    │            │  │
│  │    Purchase of Fixed Assets                               │  -8,80,000 │  │
│  │    ─────────────────────────────────────────────────────────────────── │  │
│  │    Net Cash from Investing                                │  -8,80,000 │  │
│  │                                                           │            │  │
│  │ C. Cash Flow from Financing Activities                    │            │  │
│  │    Owner's Capital Introduced                             │ 25,00,000  │  │
│  │    Term Loan Received                                     │ 15,00,000  │  │
│  │    ─────────────────────────────────────────────────────────────────── │  │
│  │    Net Cash from Financing                                │ 40,00,000  │  │
│  │                                                           │            │  │
│  │══════════════════════════════════════════════════════════════════════════│  │
│  │ NET INCREASE / (DECREASE) IN CASH (A + B + C)             │ 22,55,300  │  │
│  │ Opening Cash & Bank Balance                               │      —     │  │
│  │ Closing Cash & Bank Balance                               │ 22,55,300  │  │
│  │══════════════════════════════════════════════════════════════════════════│  │
│  └──────────────────────────────────────────────────────────┴────────────┘  │
│                                                                              │
│  [📥 DOWNLOAD PDF]  [📊 DOWNLOAD EXCEL]  [🖨 PRINT]  [🔙 BACK]            │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Report Sections

| Section              | Description                                              |
| -------------------- | -------------------------------------------------------- |
| Operating Activities | Day-to-day business cash flow (income, expenses, working capital) |
| Investing Activities | Purchase/sale of fixed assets and investments            |
| Financing Activities | Capital introduced, loans taken/repaid                   |

---

================================================================================

# 33.6 GST Reports (GSTR-1 & GSTR-3B)

**Description:**
Generates GST compliance data for filing GSTR-1 (Outward Supplies / Sales) and GSTR-3B (Summary Tax Return). Pulls data from Module 28 (Sales Invoices) and Module 29 (Purchase Bills) to compute output tax, input tax credit, and net GST payable.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          GST REPORTS                                         │
│                                                                              │
│  TABS: [ GSTR-1 (Sales) ]  [ GSTR-3B (Summary) ]                           │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Tax Period : [▼ March 2026 ▼]        Branch: [▼ All ▼]              │  │
│  │ GSTIN      : 27AAAPS1234F1Z5                     [GENERATE]         │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  GSTR-1: OUTWARD SUPPLIES SUMMARY                                           │
│  ┌──────────────────────────────────────────────────────────────────────┐   │
│  │ Category           │Count │Taxable Amt │CGST      │SGST     │IGST    │   │
│  │────────────────────┼──────┼────────────┼──────────┼─────────┼────────│   │
│  │ B2B (> ₹2.5L)      │  5   │₹ 8,50,000  │₹ 76,500  │₹ 76,500 │—       │   │
│  │ B2B (<= ₹2.5L)     │ 15   │₹ 4,20,000  │₹ 37,800  │₹ 37,800 │—       │   │
│  │ B2C Large (>₹2.5L) │  2   │₹ 3,00,000  │—         │—        │₹54,000 │   │
│  │ B2C Small           │ 25   │₹ 2,80,000  │₹ 25,200  │₹ 25,200 │—       │   │
│  │ Credit Notes        │  1   │-₹ 2,950    │-₹ 265    │-₹ 265   │—       │   │
│  │ ────────────────────┤──────┤────────────┤──────────┤─────────┤────────│   │
│  │ TOTAL               │ 48   │₹18,47,050  │₹1,39,235 │₹1,39,235│₹54,000│   │
│  └──────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  GSTR-3B: TAX LIABILITY SUMMARY                                             │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Output Tax (from Sales Invoices)                                      │  │
│  │   CGST   : ₹ 1,39,235     SGST  : ₹ 1,39,235     IGST : ₹ 54,000   │  │
│  │                                                                       │  │
│  │ Input Tax Credit (from Purchase Bills)                                │  │
│  │   CGST   : ₹ 45,000       SGST  : ₹ 45,000       IGST : ₹ 16,200   │  │
│  │                                                                       │  │
│  │ ──────────────────────────────────────────────────────────────────── │  │
│  │ NET TAX PAYABLE                                                       │  │
│  │   CGST   : ₹ 94,235       SGST  : ₹ 94,235       IGST : ₹ 37,800   │  │
│  │                                                                       │  │
│  │ TOTAL GST PAYABLE : ₹ 2,26,270                                       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [📥 DOWNLOAD JSON (GST Portal)]  [📊 DOWNLOAD EXCEL]  [🔙 BACK]          │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## GST Report Fields

| Field            | Type   | Description                                             |
| ---------------- | ------ | ------------------------------------------------------- |
| Category         | Text   | B2B Large / B2B Small / B2C Large / B2C Small / CN / DN |
| Count            | Number | Number of invoices in the category                      |
| Taxable Amount   | Number | Sum of taxable amounts                                  |
| CGST / SGST      | Number | Intra-state tax components                              |
| IGST             | Number | Inter-state tax component                               |
| Output Tax       | Number | Total tax collected from customers                      |
| Input Tax Credit | Number | Total tax paid to vendors (claimable)                   |
| Net Tax Payable  | Number | Output Tax - Input Tax Credit                           |

---

## Business Rules

| Rule                    | Description                                               |
| ----------------------- | --------------------------------------------------------- |
| B2B threshold           | ₹2.5 Lakh determines Large vs Small categorization       |
| E-Invoice data          | Where E-Invoice generated (Module 28), data auto-pulled  |
| Credit Note adjustment  | Credit Notes reduce the output tax                        |
| ITC conditions          | Only approved vendor bills with valid GSTIN are eligible  |
| JSON export for portal  | Data exported in GST portal-compatible JSON format        |

---

================================================================================

# 33.7 Ageing Analysis (Receivable & Payable)

**Description:**
Shows outstanding amounts grouped by **age buckets** (0–30 days, 31–60 days, 61–90 days, 90+ days) for both Receivable (customers owe us) and Payable (we owe vendors). Helps identify overdue accounts and prioritize collections/payments.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      AGEING ANALYSIS                                        │
│                                                                              │
│  TABS: [ RECEIVABLE (Customers) ]  [ PAYABLE (Vendors) ]                    │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ As on Date : [📅 28 Mar 2026]    Branch: [▼ All ▼]    [GENERATE]    │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  RECEIVABLE AGEING (Customers Owe Us)                                        │
│  ┌──────────────────┬──────────┬──────────┬──────────┬──────────┬─────────┐  │
│  │ Customer          │ 0-30 days│31-60 days│61-90 days│ 90+ days │ TOTAL   │  │
│  │──────────────────┼──────────┼──────────┼──────────┼──────────┼─────────│  │
│  │ ABC Corp Ltd      │₹ 15,000  │₹ 20,000  │—         │—         │₹ 35,000│  │
│  │ XYZ Hotels        │₹ 22,000  │—         │—         │—         │₹ 22,000│  │
│  │ Global Biz        │—         │—         │₹ 10,000  │₹ 5,000   │₹ 15,000│  │
│  │ ─────────────────┤──────────┤──────────┤──────────┤──────────┤─────────│  │
│  │ TOTAL             │₹ 37,000  │₹ 20,000  │₹ 10,000  │₹ 5,000   │₹ 72,000│  │
│  │ % of Total        │ 51.4%    │ 27.8%    │ 13.9%    │ 6.9%     │ 100%   │  │
│  └──────────────────┴──────────┴──────────┴──────────┴──────────┴─────────┘  │
│                                                                              │
│  AGEING CHART (Visual Bar)                                                   │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ 0-30d:  ████████████████████████████████████████████████ 51%          │  │
│  │ 31-60d: █████████████████████████████ 28%                             │  │
│  │ 61-90d: ████████████████ 14%                                          │  │
│  │ 90+d:   ████████ 7%                                                   │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  [📥 DOWNLOAD PDF]  [📊 DOWNLOAD EXCEL]  [📧 EMAIL TO OVERDUE PARTIES]     │
│  [🔙 BACK]                                                                  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Table Fields

| Field       | Type   | Description                                              |
| ----------- | ------ | -------------------------------------------------------- |
| Customer/Vendor| Text| Party name (clickable → opens Ledger Statement)         |
| 0-30 Days   | Number | Amount outstanding for 0-30 days                         |
| 31-60 Days  | Number | Amount outstanding for 31-60 days                        |
| 61-90 Days  | Number | Amount outstanding for 61-90 days                        |
| 90+ Days    | Number | Amount outstanding for more than 90 days                 |
| Total       | Number | Sum of all age buckets for that party                    |

---

## Business Rules

| Rule                         | Description                                            |
| ---------------------------- | ------------------------------------------------------ |
| Age calculated from Due Date | Not from Invoice Date — age = Today - Due Date         |
| Color coding                 | 0-30: Green, 31-60: Yellow, 61-90: Orange, 90+: Red   |
| Email reminder               | Bulk email to all 90+ overdue parties with statement   |
| Drill-down                   | Click party name → opens their Ledger Statement (31.3) |

---

================================================================================

# 33.8 Bank Reconciliation

**Description:**
Matches the company's **book balance** (from ERP Ledger) with the **bank statement balance** to identify discrepancies. Users can upload the bank statement (CSV/Excel) and the system auto-matches entries. Unmatched entries are highlighted for manual resolution.

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       BANK RECONCILIATION                                    │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Bank Account  : [▼ HDFC Current A/C - 1234 ▼]                       │  │
│  │ Period        : [📅 01 Mar 2026] to [📅 31 Mar 2026]                 │  │
│  │ Upload Statement: [📎 Upload CSV / Excel]            [RECONCILE]     │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  RECONCILIATION SUMMARY                                                      │
│  ┌──────────────────────────────────────────────────────────────────────┐   │
│  │ Balance as per Books (ERP)          : ₹ 7,00,000                     │   │
│  │ Balance as per Bank Statement       : ₹ 6,85,000                     │   │
│  │ Difference                          : ₹ 15,000                       │   │
│  │                                                                      │   │
│  │ Unmatched in Books (Not in Bank)    : 2 entries (₹ 15,000)           │   │
│  │ Unmatched in Bank (Not in Books)    : 1 entry (₹ 0)                  │   │
│  └──────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  MATCHED ENTRIES ✅                                                         │
│  ┌──────────┬──────────┬────────────────┬──────────┬──────────────────┐     │
│  │Date      │Ref #     │Description     │Book Amt  │Bank Stmt Amt     │     │
│  │──────────┼──────────┼────────────────┼──────────┼──────────────────│     │
│  │05 Mar 26 │UTR123456 │Receipt-ABC Corp│₹ 15,000  │₹ 15,000   ✅     │     │
│  │10 Mar 26 │NEFT78901 │Payment-Vendor X│₹ 85,000  │₹ 85,000   ✅     │     │
│  └──────────┴──────────┴────────────────┴──────────┴──────────────────┘     │
│                                                                              │
│  UNMATCHED ENTRIES ⚠️                                                       │
│  ┌──────────┬──────────┬────────────────┬──────────┬──────────────────────┐  │
│  │Date      │Ref #     │Description     │Amount    │Source   │Action       │  │
│  │──────────┼──────────┼────────────────┼──────────┼────────┼─────────────│  │
│  │25 Mar 26 │CHQ-22345 │Cheque to Vendor│₹ 10,000  │Books   │[Match] [Adj]│  │
│  │28 Mar 26 │UTR99999  │Cash Deposit    │₹ 5,000   │Books   │[Match] [Adj]│  │
│  └──────────┴──────────┴────────────────┴──────────┴────────┴─────────────┘  │
│                                                                              │
│  [📥 DOWNLOAD REPORT]  [✅ MARK ALL AS RECONCILED]  [🔙 BACK]              │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Screen Fields

| Field           | Type        | Required | Description                                     |
| --------------- | ----------- | -------- | ----------------------------------------------- |
| Bank Account    | Dropdown    | Yes      | Select bank account from Module 32              |
| Period          | Date Range  | Yes      | Reconciliation period                           |
| Upload Statement| File Upload | Yes      | Bank statement in CSV/Excel format              |

---

## Reconciliation Fields

| Field           | Type   | Description                                              |
| --------------- | ------ | -------------------------------------------------------- |
| Book Balance    | Number | Balance as per ERP Ledger for the account                |
| Bank Balance    | Number | Balance as per uploaded bank statement                   |
| Difference      | Number | Book Balance - Bank Balance                              |
| Matched Entries | Table  | Entries found in both books and bank                     |
| Unmatched       | Table  | Entries in books but not in bank (or vice versa)         |

---

## Actions

| Action           | Type   | Description                                             |
| ---------------- | ------ | ------------------------------------------------------- |
| **Match**        | Button | Manually match an entry with a bank statement entry     |
| **Adjust**       | Button | Create adjustment journal entry                         |
| **Reconcile All**| Button | Mark all matched entries as reconciled                  |

---

## System Behavior

| Event                    | System Action                                            |
| ------------------------ | -------------------------------------------------------- |
| Statement uploaded       | System auto-matches by UTR/Ref # and amount              |
| Auto-match found         | Entry marked as ✅ Matched                               |
| No match found           | Entry highlighted as ⚠️ Unmatched                        |
| Manual match clicked     | User selects bank entry to match with book entry         |
| Adjust clicked           | Opens Journal Entry form (Module 30.5) pre-filled        |

---

================================================================================

# 33.9 TDS Report

**Description:**
Shows all **TDS deducted and collected** during the selected period. Used for quarterly TDS filing (Form 26Q) and annual certificate generation (Form 16A).

---

## Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          TDS REPORT                                          │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │ Quarter : [▼ Q4 (Jan-Mar 2026) ▼]     Branch: [▼ All ▼]            │  │
│  │ Section : [▼ All / 194C / 194J / 194H / etc ▼]    [GENERATE]       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  TDS DEDUCTED (WE DEDUCTED FROM VENDORS)                                    │
│  ┌────────────────┬────────────┬──────────────┬──────────┬──────────────┐   │
│  │ Vendor          │ PAN        │ Section       │ Taxable  │ TDS Amount   │   │
│  │────────────────┼────────────┼──────────────┼──────────┼──────────────│   │
│  │Industrial Chem  │AABCI1234F  │194C (1%)     │₹85,000   │₹ 850         │   │
│  │Legal Advisors   │AADLA5678G  │194J (10%)    │₹50,000   │₹ 5,000       │   │
│  │──────────────────────────────────────────────────────────────────────│   │
│  │ TOTAL DEDUCTED                                          │₹ 5,850       │   │
│  └────────────────┴────────────┴──────────────┴──────────┴──────────────┘   │
│                                                                              │
│  TDS COLLECTED (CUSTOMERS DEDUCTED FROM US)                                  │
│  ┌────────────────┬────────────┬──────────────┬──────────┬──────────────┐   │
│  │ Customer        │ TAN        │ Section       │ Amount   │ TDS Deducted │   │
│  │────────────────┼────────────┼──────────────┼──────────┼──────────────│   │
│  │ABC Corp Ltd     │MUMA12345B  │194C (2%)     │₹25,000   │₹ 500         │   │
│  │──────────────────────────────────────────────────────────────────────│   │
│  │ TOTAL COLLECTED                                         │₹ 500         │   │
│  └────────────────┴────────────┴──────────────┴──────────┴──────────────┘   │
│                                                                              │
│  [📥 DOWNLOAD FORM 26Q]  [📊 DOWNLOAD EXCEL]  [📄 GENERATE FORM 16A]       │
│  [🔙 BACK]                                                                  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Report Fields

| Field          | Type   | Description                                              |
| -------------- | ------ | -------------------------------------------------------- |
| Vendor/Customer| Text   | Party name                                               |
| PAN / TAN      | Text   | Tax identification number                                |
| Section        | Text   | TDS section and rate                                     |
| Taxable Amount | Number | Amount on which TDS was deducted                         |
| TDS Amount     | Number | Actual TDS deducted                                      |

---

## Actions

| Action                | Description                                           |
| --------------------- | ----------------------------------------------------- |
| **Download Form 26Q** | Download TDS return in NSDL-compatible format         |
| **Download Excel**    | Export as Excel for analysis                          |
| **Generate Form 16A** | Generate TDS certificates for each deductee           |

---

## Business Rules

| Rule                       | Description                                           |
| -------------------------- | ----------------------------------------------------- |
| Quarterly reporting        | TDS must be filed quarterly (Q1-Q4)                   |
| PAN mandatory              | TDS cannot be deducted without valid PAN              |
| Higher rate without PAN    | If vendor PAN missing, TDS deducted at 20%            |
| Certificate generation     | Form 16A must be issued within 15 days of filing      |

---

## Cross-Module Integration Summary (Modules 28–33)

```
MODULE 18: CUSTOMER ─────────► MODULE 28: INVOICING ──────► MODULE 31: LEDGER
(Customer Master)              (Sales Invoices)              (Customer Balance)
                                      │                           │
MODULE 20: SALES ORDER ───────────────┘                           │
                                                                  │
MODULE 11: STOCK/VENDOR ─────► MODULE 29: BILLS ─────────► MODULE 31: LEDGER
(Vendor Master + PO + GRN)    (Purchase Bills)              (Vendor Balance)
                                      │                           │
                                      └──────────── MODULE 30: PAYMENTS
                                                   (Receipts & Vouchers)
                                                          │
                                                          ├──► MODULE 31: LEDGER
                                                          │    (Balance Update)
                                                          │
MODULE 32: CHART OF ACCOUNTS ◄────────────────────────────┘
(Account Structure)                   │
                                      ▼
                               MODULE 33: REPORTS
                               (P&L, BS, GST, Ageing, BRS, TDS)
```

---
