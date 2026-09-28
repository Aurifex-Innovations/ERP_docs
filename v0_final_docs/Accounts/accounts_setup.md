# System Posting Keys & Ledger Mapping Guide

This guide breaks down all 30 System Posting Keys used by the ERP engine for automated double-entry accounting. When a specific transaction occurs (like raising a Sales Invoice or running HRMS Payroll), the system looks up these keys to determine exactly which Chart of Accounts (COA) ledger to debit or credit.

> [!TIP]
> **Golden Rule for Mapping**: When mapping these keys in the Books Setup Wizard, you must create standard ledgers in your Chart of Accounts under the 5 core groups (Assets, Liabilities, Income, Expenses, Capital) and then bind them to these keys.

---

## 1. Sales & Income (Accounts Receivable)
These keys handle revenue generation and customer adjustments.

| Posting Key | Purpose / Trigger | Recommended Ledger to Create & Bind | COA Type |
| :--- | :--- | :--- | :--- |
| **`INVOICE_APPROVE_INCOME`** | Triggered when a Sales Invoice is approved. Credits your revenue. | `Sales Revenue` or `Sales Accounts` | Income |
| **`INVOICE_CREDIT_NOTE_ADJUSTMENT`** | Triggered when a Credit Note is issued to a customer. | `Sales Returns` or `Discounts Given` | Income |
| **`CUSTOMER_ADVANCE`** | Triggered when a customer pays in advance before an invoice is raised. | `Advances from Customers` | Liability |

## 2. Purchases & Expenses (Accounts Payable)
These keys handle vendor bills and procurement adjustments.

| Posting Key | Purpose / Trigger | Recommended Ledger to Create & Bind | COA Type |
| :--- | :--- | :--- | :--- |
| **`BILL_CONFIRM_EXPENSE`** | Triggered when a Vendor Bill (Purchase) is confirmed. Debits your expense. | `Purchase Accounts` or `COGS` | Expense |
| **`BILL_DEBIT_NOTE_ADJUSTMENT`** | Triggered when you issue a Debit Note to a vendor (e.g., returning goods). | `Purchase Returns` or `Discounts Received`| Expense (or Income) |

## 3. GST (Goods & Services Tax)
The system strictly separates Input (Purchases) and Output (Sales) taxes for accurate GST return filing.

### Output GST (Liabilities - Tax collected from Customers)
| Posting Key | Purpose / Trigger | Recommended Ledger to Create & Bind | COA Type |
| :--- | :--- | :--- | :--- |
| **`TAX_CGST_OUTPUT`** | Central GST on Sales | `CGST Payable (Output)` | Liability |
| **`TAX_SGST_OUTPUT`** | State GST on Sales | `SGST Payable (Output)` | Liability |
| **`TAX_IGST_OUTPUT`** | Integrated GST on interstate Sales | `IGST Payable (Output)` | Liability |
| **`TAX_CESS_OUTPUT`** | GST Cess on Sales | `Cess Payable (Output)` | Liability |

### Input GST (Assets - Tax paid to Vendors / ITC)
| Posting Key | Purpose / Trigger | Recommended Ledger to Create & Bind | COA Type |
| :--- | :--- | :--- | :--- |
| **`TAX_CGST_INPUT`** | Central GST on Purchases | `CGST Receivable (Input/ITC)` | Asset |
| **`TAX_SGST_INPUT`** | State GST on Purchases | `SGST Receivable (Input/ITC)` | Asset |
| **`TAX_IGST_INPUT`** | Integrated GST on interstate Purchases | `IGST Receivable (Input/ITC)` | Asset |
| **`TAX_CESS_INPUT`** | GST Cess on Purchases | `Cess Receivable (Input/ITC)` | Asset |

## 4. TDS (Tax Deducted at Source)
Handles both withholding tax from vendors and tax withheld by customers.

| Posting Key | Purpose / Trigger | Recommended Ledger to Create & Bind | COA Type |
| :--- | :--- | :--- | :--- |
| **`TDS_PAYABLE`** | Tax you deduct from Vendor payments (to be paid to Govt). | `TDS Payable` | Liability |
| **`TDS_RECEIVABLE`** | Tax your Customers deduct from your invoices (claimed during filing).| `TDS Receivable` | Asset |

## 5. HRMS, Payroll & Admin
These keys automate the entire payroll journal entry process when salary batches are disbursed.

### Expenses (Cost to Company)
| Posting Key | Purpose / Trigger | Recommended Ledger to Create & Bind | COA Type |
| :--- | :--- | :--- | :--- |
| **`SALARY_WAGES`** | Base salary and allowances paid to employees. | `Salaries & Wages Expense` | Expense |
| **`EMPLOYER_PF_EXPENSE`** | Employer's share of PF contribution. | `PF Employer Contribution Exp`| Expense |
| **`EMPLOYER_ESI_EXPENSE`** | Employer's share of ESI contribution. | `ESI Employer Contribution Exp`| Expense |
| **`LEAVE_ENCASHMENT_EXPENSE`** | Leave encashment during Full & Final Settlement. | `Leave Encashment Expense` | Expense |
| **`GRATUITY_EXPENSE`** | Gratuity paid during Full & Final Settlement. | `Gratuity Expense` | Expense |
| **`PETTY_CASH_EXPENSE`** | General admin and petty cash reimbursements. | `Office/Admin Expenses` | Expense |

### Statutory Payables & Liabilities (Deductions to pay Govt/Employees)
| Posting Key | Purpose / Trigger | Recommended Ledger to Create & Bind | COA Type |
| :--- | :--- | :--- | :--- |
| **`PF_PAYABLE`** | Total PF (Employee + Employer) to deposit to EPFO. | `EPF Payable` | Liability |
| **`ESI_PAYABLE`** | Total ESI (Employee + Employer) to deposit to ESIC. | `ESIC Payable` | Liability |
| **`PT_PAYABLE`** | Professional Tax deducted from employees. | `Professional Tax Payable` | Liability |
| **`TDS_SALARY_PAYABLE`** | Income Tax deducted from employees u/s 192. | `TDS on Salary Payable` | Liability |
| **`STAFF_REIMB_PAYABLE`** | Approved employee expense reimbursements waiting to be paid. | `Staff Reimbursements Payable` | Liability |

### Assets & Banks
| Posting Key | Purpose / Trigger | Recommended Ledger to Create & Bind | COA Type |
| :--- | :--- | :--- | :--- |
| **`STAFF_ADVANCE`** | Salary advances given to employees (recovered later). | `Staff Advances` | Asset |
| **`SALARY_BANK`** | The main bank account used for dispersing salaries. | `Primary Corporate Bank A/C` | Asset (Bank) |

## 6. Miscellaneous & System Adjustments
| Posting Key | Purpose / Trigger | Recommended Ledger to Create & Bind | COA Type |
| :--- | :--- | :--- | :--- |
| **`ROUNDING_OFF`** | Captures decimal rounding differences in invoices/bills. | `Rounding Off Adjustments` | Expense/Income |
| **`RETAINED_EARNINGS`** | Used during Year-End Close to carry forward net profit/loss. | `Retained Earnings (P&L)` | Capital |

---

## 7. Auto Ledger Mapping (Control Accounts)

While the System Posting Keys above map **automated transactions to Ledgers**, the system also automatically provisions **individual Ledgers** whenever you create a new Customer or Vendor.

To decide exactly which Chart of Accounts (COA) group these new ledgers belong to, the system uses **Finance Control Accounts**. These map core backend roles to your COA Head IDs.

| System Role | Purpose | Default COA Head Mapping |
| :--- | :--- | :--- |
| **`ROLE_SUNDRY_DEBTORS`** | Defines the parent COA group for all auto-generated Customer ledgers. | `Sundry Debtors` (Asset) |
| **`ROLE_SUNDRY_CREDITORS`** | Defines the parent COA group for all auto-generated Vendor ledgers. | `Sundry Creditors` (Liability) |

> [!WARNING]
> If a user decides to delete the default "Sundry Debtors" COA and creates a custom one (e.g., "Wholesale Customers"), they MUST update the `finance_control_accounts` mapping to point `ROLE_SUNDRY_DEBTORS` to their new COA ID. Otherwise, the auto-provisioning system will fail to generate new customer ledgers!

---

> [!IMPORTANT]  
> **How does Cash In/Out work?**
> You don't need a posting key for general Cash In/Out! When you record a manual Payment or Receipt voucher against a Vendor or Customer, you simply select the **Bank** or **Cash** ledger manually in the voucher UI. 
> The System Posting Keys above are strictly for **headless automation** (when a background module like HRMS or Invoicing needs to pass an entry without human intervention).
