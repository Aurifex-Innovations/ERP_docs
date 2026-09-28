# Seravion ERP — Accounting Module: Complete User Guide

> A from-scratch onboarding guide for new users. Covers system configuration, what the system auto-generates, how to interpret system-provided items, and how to achieve perfect accounts.

---

## Part 1 — First-Time Setup: Books Setup Wizard

When a new tenant (company) is onboarded, the **Books & Financial Year Setup** wizard walks you through 5 steps. You must complete all steps before the accounting engine activates.

---

### Step 1: Company & Financial Year

| Field | What to Enter | Why It Matters |
| :--- | :--- | :--- |
| **Legal Entity Name** | Your registered company name | Appears on all reports, invoices, and statutory filings |
| **GSTIN** | 15-digit GST registration number | Used for auto-generated tax vouchers and GST return data |
| **PAN** | 10-character PAN | Required for TDS calculations and compliance |
| **State / Jurisdiction** | Your state of registration | Determines SGST vs IGST applicability |
| **Base Currency** | INR (₹), USD ($), etc. | All ledger balances denominate in this currency. **Cannot be changed later.** |
| **Financial Year Cycle** | India — April to March **or** Calendar — January to December | Defines your reporting periods (12 months) and year-end close cycle |
| **Books Operational Starting Date** | The date from which you want the system to start tracking | Typically 1st April of the current FY, or company incorporation date |

> **What happens:** The system creates a Financial Year with 12 monthly periods and marks them as OPEN for transaction posting.

---

### Step 2: Asset Validation

This step ensures your essential **asset-side ledgers** exist in the Chart of Accounts (COA).

#### What the System Shows You:

| Item | What It Means |
| :--- | :--- |
| **Salary Payout Bank Ledger** | The default bank account used for salary disbursement. Select your primary operating bank account here. |
| **CASH_MAIN Ledger Status** | Whether a `CASH_MAIN` ledger exists. This is your primary cash-in-hand account for petty cash and counter sales. Shows **Active** ✅ or **Missing** ❌. |
| **Bank Ledgers Detected** | Count of bank-type ledgers found in your COA (e.g., "5 bank account(s) found"). Shows **Configured** or **Missing**. |

#### What Real Companies Should Create Here:

The system seeds a basic COA, but real companies typically carry many more asset accounts. You should create these in **Chart of Accounts → Ledger Management** before going live:

| Asset Type | Recommended Ledger(s) to Create | COA Group |
| :--- | :--- | :--- |
| **Bank Accounts** | One ledger per real bank account: `HDFC Current A/c 1234`, `ICICI Salary A/c 5678`, `SBI Operations A/c 9012` | Asset → Bank |
| **Cash in Hand** | `CASH_MAIN` (system-required), optionally `Petty Cash Fund — Branch A`, `Petty Cash Fund — Branch B` | Asset → Cash |
| **Fixed Assets** | `Furniture & Fixtures`, `Computer Equipment`, `Vehicles`, `Plant & Machinery`, `Office Equipment` | Asset |
| **Intangible Assets** | `Software Licenses`, `Goodwill`, `Trademarks & Patents` | Asset |
| **Deposits & Advances** | `Security Deposits`, `Rent Deposits`, `Electricity Deposits`, `Advance to Suppliers` | Asset |
| **Prepaid Expenses** | `Prepaid Insurance`, `Prepaid Rent`, `Prepaid Software Subscriptions` | Asset |
| **Investments** | `Fixed Deposits`, `Mutual Funds`, `Government Bonds` | Asset |
| **Loans & Receivables** | `Loans to Staff`, `Inter-Company Receivables` | Asset |

> **Tip — Bank Ledgers:** When you click **"Configured"** next to Bank Ledgers Detected, it currently shows a count. Each bank ledger you create in COA with type `BANK` automatically appears here and becomes available for selection in the **Payment Batches** module (for salary, vendor, and other disbursements).

> **Why this matters:** If you have 5 real bank accounts but only 1 ledger in the system, your bank reconciliation will be impossible. Create one ledger per real-world bank account.

---

### Step 3: Statutory & System Posting Keys

This is where you tell the system which ledger to use for each automated action. The system provides **30 Posting Keys** grouped by module:

#### Sales & Revenue Keys
| Posting Key | What It Does | Recommended Ledger |
| :--- | :--- | :--- |
| `INVOICE_APPROVE_INCOME` | Credits revenue when a Sales Invoice is approved | Sales Revenue / Sales Accounts |
| `INVOICE_CREDIT_NOTE_ADJUSTMENT` | Adjusts revenue when a Credit Note is issued | Sales Returns / Discounts Given |
| `CUSTOMER_ADVANCE` | Records advance payments from customers | Advances from Customers (Liability) |

#### Purchase & Expense Keys
| Posting Key | What It Does | Recommended Ledger |
| :--- | :--- | :--- |
| `BILL_CONFIRM_EXPENSE` | Debits expense when a Vendor Bill is confirmed | Purchase Accounts / COGS |
| `BILL_DEBIT_NOTE_ADJUSTMENT` | Adjusts expense when a Debit Note is raised | Purchase Returns |

#### GST Tax Keys (8 Keys)
| Posting Key | What It Does |
| :--- | :--- |
| `TAX_CGST_OUTPUT` / `TAX_SGST_OUTPUT` / `TAX_IGST_OUTPUT` / `TAX_CESS_OUTPUT` | Output tax collected on sales (Liability) |
| `TAX_CGST_INPUT` / `TAX_SGST_INPUT` / `TAX_IGST_INPUT` / `TAX_CESS_INPUT` | Input tax paid on purchases (Asset — claimable credit) |

#### TDS Keys
| Posting Key | What It Does | Recommended Ledger |
| :--- | :--- | :--- |
| `TDS_PAYABLE` | TDS deducted by you from vendor payments | TDS Payable (Liability) |
| `TDS_RECEIVABLE` | TDS deducted by your customer from your invoices | TDS Receivable (Asset) |
| `TDS_SALARY_PAYABLE` | TDS deducted from employee salary u/s 192 | TDS on Salaries Payable (Liability) |

#### HRMS & Payroll Keys
| Posting Key | What It Does | Recommended Ledger |
| :--- | :--- | :--- |
| `SALARY_WAGES` | Gross salary expense | Salary & Wages (Expense) |
| `SALARY_PAYABLE` | Accrued salary liability (before bank payout) | Salaries Payable (Liability) |
| `EMPLOYER_PF_EXPENSE` | Employer's EPF contribution | Employer PF Expense (Expense) |
| `EMPLOYER_ESI_EXPENSE` | Employer's ESIC contribution | Employer ESI Expense (Expense) |
| `PF_PAYABLE` | PF deducted, pending deposit to EPFO | PF Payable (Liability) |
| `ESI_PAYABLE` | ESI deducted, pending deposit | ESI Payable (Liability) |
| `PT_PAYABLE` | Professional Tax deducted | PT Payable (Liability) |
| `LEAVE_ENCASHMENT_EXPENSE` | Leave payout during F&F | Leave Encashment (Expense) |
| `GRATUITY_EXPENSE` | Gratuity during F&F | Gratuity (Expense) |
| `STAFF_ADVANCE` | Salary advance given to employee | Staff Advance (Asset) |
| `SALARY_BANK` | Default bank for salary/F&F payout | Your primary salary bank (Asset → Bank) |

#### Petty Cash & Reimbursement Keys
| Posting Key | What It Does | Recommended Ledger |
| :--- | :--- | :--- |
| `PETTY_CASH_EXPENSE` | Expense when petty claim is approved | Petty Cash / Field Expenses (Expense) |
| `STAFF_REIMB_PAYABLE` | Liability until reimbursement is actually paid out | Staff Reimbursement Payable (Liability) |

#### General Keys
| Posting Key | What It Does | Recommended Ledger |
| :--- | :--- | :--- |
| `ROUNDING_OFF` | Auto-adjusts rounding differences (₹0.50 etc.) | Rounding Off (Expense / Income) |
| `RETAINED_EARNINGS` | Year-end close transfers P&L balance here | Retained Earnings (Capital) |

---

### Step 4: Opening Trial Balance

If migrating from an older system, enter your opening balances here. If this is a brand-new company, tick **"Confirm Zero Openings"** to proceed.

### Step 5: Go Live

Review everything and click **Go Live**. The system locks your setup and activates automated GL posting.

---

## Part 2 — What the System Auto-Generates (Module by Module)

Once live, every module action creates automatic, balanced journal entries. Here's exactly what happens:

### A. Sales Module (Invoicing)

| User Action | System Journal Entry |
| :--- | :--- |
| Approve Sales Invoice ₹1,18,000 | **Dr** Accounts Receivable (Customer) ₹1,18,000 <br> **Cr** Sales Revenue ₹1,00,000 <br> **Cr** CGST Output ₹9,000 <br> **Cr** SGST Output ₹9,000 |
| Issue Credit Note ₹5,000 | **Dr** Sales Returns ₹5,000 <br> **Cr** Accounts Receivable ₹5,000 |
| Receive Payment ₹1,18,000 | **Dr** Bank Account ₹1,18,000 <br> **Cr** Accounts Receivable ₹1,18,000 |

### B. Procurement Module (Bills / Purchases)

| User Action | System Journal Entry |
| :--- | :--- |
| Confirm Vendor Bill ₹59,000 | **Dr** Purchase Expense ₹50,000 <br> **Dr** CGST Input ₹4,500 <br> **Dr** SGST Input ₹4,500 <br> **Cr** Accounts Payable (Vendor) ₹59,000 |
| Issue Debit Note ₹3,000 | **Dr** Accounts Payable ₹3,000 <br> **Cr** Purchase Returns ₹3,000 |
| Make Payment ₹59,000 | **Dr** Accounts Payable ₹59,000 <br> **Cr** Bank Account ₹59,000 |

**What about internal office expense bills?**
Use the **Bills (Purchases)** module for ALL vendor-related expenses — whether it's a raw material supplier, an internet service provider, an office supplies vendor, or a landlord. Create a vendor entry for each party, raise a bill, and the system handles the rest. For example:
- **Office Rent:** Create vendor "ABC Properties", raise monthly bill under expense ledger "Rent Expense"
- **Internet/Telephone:** Create vendor "Airtel Business", raise bill under "Internet & Telephone Expense"
- **Software Subscriptions:** Create vendor "Microsoft", raise bill under "Software Expense"
- **Office Supplies:** Create vendor "Staples India", raise bill under "Office Supplies Expense"

> **Key Point:** Every expense that has an external party (vendor) and a formal bill/invoice should go through the Bills module. This ensures proper Accounts Payable tracking, TDS compliance, and GST input credit.

### C. HRMS Payroll (Two-Step Flow)

| User Action | System Journal Entry |
| :--- | :--- |
| Finalize Monthly Payroll | **Dr** Salary & Wages Expense ₹5,00,000 <br> **Dr** Employer PF Expense ₹60,000 <br> **Dr** Employer ESI Expense ₹16,250 <br> **Cr** Salary Payable ₹4,10,000 <br> **Cr** PF Payable ₹60,000 <br> **Cr** ESI Payable ₹16,250 <br> **Cr** PT Payable ₹12,500 <br> **Cr** TDS Payable ₹61,000 |
| Generate Payment Batch & Reconcile Bank Return | **Dr** Salary Payable ₹4,10,000 <br> **Cr** Bank Account (selected) ₹4,10,000 |

### D. Petty Cash & Reimbursements (Complete Workflow)

The Petty Cash module handles **all small, day-to-day expenses** that don't go through the formal Bills module. This is the proper channel for internal office expenses paid from pocket or petty cash fund.

#### Petty Cash Expense Categories Available:

| Category | Use For |
| :--- | :--- |
| `OFFICE_EXPENSES` | General office supplies, cleaning materials, minor repairs |
| `STATIONERY` | Paper, pens, printer cartridges, files |
| `FUEL` | Vehicle fuel for company operations |
| `LOCAL_CONVEYANCE` | Auto/cab fares for work-related travel |
| `TRAVEL_EXPENSES` | Inter-city travel, hotel stays for work |
| `INTERNET_AND_TELEPHONE` | Recharge, WiFi bills for field staff |
| `STAFF_WELFARE` | Tea, snacks, team meals, celebrations |
| `VEHICLE_MAINTENANCE` | Servicing, tyre replacement, insurance |
| `RENT` | Small rent payments, parking charges |
| `ASSET_PURCHASE` | Minor equipment, tools, devices |
| `CHEMICAL` | Cleaning agents, lab chemicals |
| `STATUTORY_AND_LICENSE` | Government fees, license renewals |
| `SALARY_ADVANCE` | Advance salary given to staff |
| `VENDOR_PAYMENT` | Small vendor payments not through Bills |
| `THIRD_PARTY_VENDOR` | External vendor linked from Vendor Master |
| `OFFICE_DEPOSIT` | Deposit payments for office utilities |
| `PROMOTER_INCENTIVE` | Promoter/sales incentive payouts |
| `OVERTIME` | Overtime pay |
| `TRANSPORTATION` | Goods transport, courier charges |
| `PETROCARD` | Fleet fuel card payments |

#### Petty Cash Workflow (Step by Step):

```
DRAFT → PENDING → APPROVED → PAID
              ↘ RETURNED (needs correction)
              ↘ REJECTED
              ↘ REVOKED (cancelled after approval)
```

1. **Employee creates request:** Selects category, enters amount, attaches receipt photo, picks payment mode (Cash / Bank Transfer / UPI).
2. **Submits for approval:** Routes to the designated reviewer (role-based).
3. **Reviewer approves:** Sets approved amount (can be less than requested). 
   - **System auto-posts:** `Dr Petty Cash Expense` / `Cr Staff Reimbursement Payable`
4. **Finance pays out:** Selects payment mode, enters transaction reference.
   - **System auto-posts:** `Dr Staff Reimbursement Payable` / `Cr Cash or Bank` (depending on payment mode)

> **How this handles expenses wisely:**
> - Every petty expense is **categorized** — you can run reports by category to see where money is going.
> - Every expense has an **approval chain** — no unauthorized spending.
> - Every expense has **receipt attachments** — audit-ready documentation.
> - Every expense creates **automatic GL entries** — no manual bookkeeping.
> - **Branch-level tracking** — each request records which branch it came from.

---

## Part 3 — When to Use Which Module

| Expense Type | Which Module to Use | Why |
| :--- | :--- | :--- |
| Monthly office rent | **Bills (Purchases)** | Formal invoice from landlord, needs Accounts Payable tracking |
| Internet / phone bill | **Bills (Purchases)** | Vendor bill with GST, needs Input Tax Credit |
| Raw material purchase | **Bills (Purchases)** | Supply chain, inventory-linked |
| Employee cab fare ₹350 | **Petty Cash** | Small amount, paid from pocket, needs reimbursement |
| Office tea/snacks ₹500 | **Petty Cash** | Staff welfare, petty amount |
| Courier charges ₹200 | **Petty Cash** | Small operational expense |
| New laptop ₹85,000 | **Bills (Purchases)** | Significant asset purchase with vendor bill |
| Printer cartridge ₹1,200 | **Petty Cash** | Minor office supply |
| Monthly salary | **HRMS Payroll** | Automated payroll processing |
| Staff salary advance | **Petty Cash** (category: SALARY_ADVANCE) | Pre-payroll advance |

---

## Part 4 — Making Your Accounts Perfect

### ✅ Do This
1. **Create granular expense ledgers** — Don't dump everything into one "Miscellaneous" account. Create specific ledgers: Travel Expense, Office Supplies, Software Licenses, etc.
2. **One bank ledger per real bank account** — If you have 5 bank accounts, create 5 bank ledgers.
3. **Use the right module** — Vendor bills → Bills module. Small office expenses → Petty Cash. Salaries → HRMS.
4. **Reconcile banks weekly** — Match your system bank balance with actual bank statement.
5. **Close periods monthly** — After reconciliation, close the period to prevent backdated entries.

### ❌ Don't Do This
1. **Don't create manual journal entries for sales/purchases/payroll** — Use the modules; they handle the double-entry automatically.
2. **Don't skip the Petty Cash workflow** — Even ₹100 expenses should be logged. Over a year, untracked petty expenses can run into lakhs.
3. **Don't mix Input and Output GST** — The system strictly separates these. Mixing them will break your GST returns.
4. **Don't pay salaries without generating a Payment Batch** — Always generate the batch file, upload to bank, reconcile the return. This ensures the Bank ledger is debited correctly.

---

## Part 5 — Glossary of System Terms

| Term | Meaning |
| :--- | :--- |
| **Posting Key** | A fixed system label (like `SALARY_WAGES`) that tells the automation engine which ledger to debit or credit |
| **Binding** | The link between a Posting Key and your actual COA ledger |
| **COA (Chart of Accounts)** | Your complete list of ledger accounts organized under 5 groups: Assets, Liabilities, Income, Expenses, Capital |
| **GL (General Ledger)** | The master record of all financial transactions |
| **Trial Balance** | A report showing all ledger balances — must always net to zero |
| **Voucher** | A single journal entry with debit and credit lines (auto-generated by the system) |
| **Reconciliation** | Matching system records with external records (bank statement) |
| **Financial Period** | One month within a Financial Year. Can be OPEN (accepts entries) or CLOSED (locked) |
| **Go Live** | The moment the system starts tracking real transactions |
| **Outbox** | A queue of GL entries waiting to be posted (used when Books Setup is incomplete) |
