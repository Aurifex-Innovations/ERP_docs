# Investment Declaration — Complete Reference
**Seravion Connect — HRM Module**
*For QA Team, HR Admins & Internal Reference*

---

## What Is the Investment Declaration Page?

The **Investment Declaration** page is the **digital equivalent of Income Tax Form 12BB**. It is where an employee (or HR on their behalf) declares their **tax-saving investments for a given financial year** so that the payroll engine can compute the correct **monthly TDS (Tax Deducted at Source)** throughout the year.

---

## Why This Form Exists

Every month, the payroll engine projects the employee's **annual taxable income** to compute how much TDS to deduct. Without knowing the employee's actual investments and exemptions, the engine would either:

- **Over-deduct TDS** → Employee receives less net salary all year and only gets a refund when filing their annual ITR.
- **Under-deduct TDS** → TDS balance piles up and gets recovered heavily in February / March.

This form allows employees to declare deductions **upfront at the start of the fiscal year**, making monthly TDS accurate and predictable.

---

> ## ⚠️ CRITICAL: Applies to OLD Tax Regime Only
>
> **ALL fields in this form — 80C investments (PPF, ELSS, LIC, NSC), Section 80D, NPS, and HRA Exemption — are ONLY used when the employee has selected the OLD Tax Regime.**
>
> **If the employee chooses the NEW Tax Regime, every single declared amount in this form is completely IGNORED by the TDS calculation engine. Only the Standard Deduction and Professional Tax deductions apply under the NEW Regime.**
>
> This is the single most important thing for QA to verify: **NEW Regime employees should never have their 80C / HRA / 80D / NPS amounts reflected in their TDS.**

---

## Form Fields — Detailed Breakdown

### Group 1 — Identity (Who & Which Year)

| Field | Description | Notes |
|---|---|---|
| **Employee** | Dropdown to select which employee this declaration is for | HR can declare on behalf of any employee |
| **Fiscal Year** | The FY this declaration covers (e.g., `2025-26`) | Only one declaration is allowed per employee per FY — the system upserts (create or update) |
| **Status** | Read-only display field | Shows `DRAFT`, `SUBMITTED`, or `LOCKED` |

---

### Group 2 — HRA / Rent (Section 10(13A) Exemption)

> **OLD Regime only.** Ignored entirely under NEW Regime.

| Field | Description |
|---|---|
| **Rent Paid (Annual)** | Total annual rent paid by the employee in rupees |
| **City Type** | `METRO` or `NON_METRO` — determines the HRA exemption cap percentage |

**HRA Exemption Calculation (used during TDS):**

```
HRA Exempt = min(
  HRA Received annually,
  Rent Paid - 10% of Annual Basic,
  50% of Annual Basic  [METRO]
  OR
  40% of Annual Basic  [NON_METRO]
)
```

The HRA Exempt amount is subtracted from the taxable income before applying tax slabs.

**Example:**
- Annual Basic = ₹7,20,000
- Annual HRA Received = ₹3,60,000
- Rent Paid = ₹2,40,000 (annually)
- City = METRO

```
10% of Basic          = ₹72,000
Rent - 10% Basic      = ₹2,40,000 - ₹72,000 = ₹1,68,000
50% of Basic (metro)  = ₹3,60,000
HRA Exempt            = min(₹3,60,000, ₹1,68,000, ₹3,60,000) = ₹1,68,000
```

---

### Group 3 — Section 80C Investments

> **OLD Regime only.** Ignored entirely under NEW Regime.  
> **Combined cap: ₹1,50,000 per financial year (all 4 fields added together).**

| Field | Full Name | What It Covers |
|---|---|---|
| **PPF** | Public Provident Fund | Annual deposits made to PPF account |
| **ELSS** | Equity Linked Saving Scheme | Tax-saving mutual fund investments |
| **LIC** | Life Insurance Premium | Life insurance premiums paid for self, spouse, or children |
| **NSC** | National Savings Certificate | Investments in NSC |

**How the cap works:**
```
Section80C Total = PPF + ELSS + LIC + NSC
Effective 80C Deduction = min(Section80C Total, ₹1,50,000)
```

The effective deduction is subtracted from taxable income.

---

### Group 4 — Other Deductions

> **OLD Regime only.** Ignored entirely under NEW Regime.

| Field | Section | Description | Limit |
|---|---|---|---|
| **Section 80D (Medical)** | Sec 80D | Health insurance premiums paid for self and family | Admin-configured cap |
| **NPS (80CCD)** | Sec 80CCD(1B) | National Pension Scheme contributions | Admin-configured cap |

---

### Group 5 — HR Fields

| Field | Who Uses It | Description |
|---|---|---|
| **HR Override Note** | HR Admin only | Free-text internal note — visible only when declaration is in an editable state or when locked and note exists |

---

## Declaration Status Lifecycle

```
[DRAFT] ──────── Employee saves form ──────────► [DRAFT]
   │
   │  Employee clicks "Submit"
   ▼
[SUBMITTED]  (submittedAt timestamp recorded)
   │
   │  HR locks the declaration
   ▼
[LOCKED]  ── No further edits allowed ──────────► Error if attempted
```

| Status | What It Means | Can Employee Edit? | Can HR Edit? |
|---|---|---|---|
| `DRAFT` | Saved but not finalized | Yes | Yes |
| `SUBMITTED` | Employee has submitted for HR review | Yes (can re-save as DRAFT) | Yes |
| `LOCKED` | HR has locked for payroll processing | No | No (shows "Contact HR" message) |

> When a declaration is `LOCKED`, any attempt to save it returns an error:  
> *"Investment declaration is locked for fiscal year YYYY-YY"*

---

## Database Storage

**Table:** `hrm_investment_declaration`

**Unique Constraint:** `(user_id, fiscal_year)` — one record per employee per fiscal year.

| Column | Type | Description |
|---|---|---|
| `id` | BIGINT PK | Auto-generated ID |
| `user_id` | FK → users | Which employee |
| `fiscal_year` | VARCHAR(9) | e.g., `"2025-26"` |
| `rent_paid_annual` | DECIMAL(12,2) | Annual rent paid |
| `city_type` | VARCHAR(20) | `METRO` / `NON_METRO` |
| `tax_regime` | VARCHAR(10) | `NEW` / `OLD` |
| `ppf_amount` | DECIMAL(12,2) | PPF investment |
| `elss_amount` | DECIMAL(12,2) | ELSS investment |
| `lic_amount` | DECIMAL(12,2) | LIC premium |
| `nsc_amount` | DECIMAL(12,2) | NSC investment |
| `section_80d_amount` | DECIMAL(12,2) | Health insurance premium |
| `nps_amount` | DECIMAL(12,2) | NPS contribution |
| `status` | VARCHAR(20) | `DRAFT` / `SUBMITTED` / `LOCKED` |
| `submitted_at` | TIMESTAMP | When employee submitted |
| `hr_override_note` | TEXT | HR's internal note |
| `created_by` | VARCHAR(100) | Username who created |
| `updated_by` | VARCHAR(100) | Username who last updated |
| `created_at` | TIMESTAMP | Auto-set on creation |
| `updated_at` | TIMESTAMP | Auto-updated on change |

---

## How This Data Is Used in Payroll / TDS Calculation

Every time payroll is computed for an employee (`HrmPayrollComputeServiceImpl.compute()`):

```
Step 1: Payroll engine fetches InvestmentDeclaration for this employee + fiscal year
Step 2: Passes it into IndianSalaryTdsCalculator.compute()
Step 3: Calculator checks the tax regime:

  IF tax_regime = OLD:
    → Apply Section 80C = min(PPF + ELSS + LIC + NSC, ₹1,50,000)
    → Apply Section 80D = min(declared amount, cap)
    → Apply NPS        = min(declared amount, cap)
    → Compute HRA Exemption using rentPaidAnnual + cityType + annualBasic + annualHra
    → All above are subtracted from EstimatedAnnual to get TaxableIncome

  IF tax_regime = NEW:
    → ALL of the above are IGNORED
    → Only Standard Deduction (₹75,000) and Annual PT deducted from EstimatedAnnual

Step 4: Progressive tax slabs applied on TaxableIncome
Step 5: Sec 87A Rebate applied (if eligible)
Step 6: Cess added (4%)
Step 7: Balance tax spread across remaining FY months → Monthly TDS
```

---

## Payroll Behavior When No Declaration Exists

If no `InvestmentDeclaration` record exists for an employee in a given FY:

- `declaration = null` is passed to the TDS calculator
- All 80C / 80D / NPS / HRA values default to **₹0**
- TDS is computed on the gross income minus only Standard Deduction + PT
- This typically results in **higher TDS** (conservative, no deductions assumed)

---

## QA Test Scenarios

| # | Scenario | Expected Behavior |
|---|---|---|
| ID-1 | Employee on NEW Regime, 80C declared ₹1,50,000 | TDS ignores 80C — uses only Std Deduction + PT |
| ID-2 | Employee on OLD Regime, 80C declared ₹1,50,000 | TDS reduces taxable by ₹1,50,000 |
| ID-3 | Employee on OLD Regime, 80C total = ₹2,00,000 | Cap applied → only ₹1,50,000 deducted |
| ID-4 | Metro employee, rent = ₹0 | HRA Exempt = ₹0 (no rent means no exemption) |
| ID-5 | Metro employee, rent > 10% of basic | HRA Exempt = correct formula result |
| ID-6 | Non-Metro, rent declared | HRA cap = 40% of basic instead of 50% |
| ID-7 | Declaration status = LOCKED | Cannot edit; save API returns error |
| ID-8 | Save as DRAFT | Status = DRAFT, submittedAt = null |
| ID-9 | Submit | Status = SUBMITTED, submittedAt = timestamp |
| ID-10 | Re-save after submit (edit) | submittedAt reset to null if moved back to DRAFT |
| ID-11 | No declaration for the FY | TDS computed with 0 deductions (conservative) |
| ID-12 | Two declarations for same FY | System upserts — no duplicate created |
| ID-13 | NEW Regime, HRA rent declared | HRA exemption = ₹0 (NEW Regime ignores HRA) |
| ID-14 | 80D declared on NEW Regime | Ignored in TDS — taxable income unchanged |
| ID-15 | NPS declared on NEW Regime | Ignored in TDS — taxable income unchanged |

---

## Summary — What This Form Affects

| What Changes | Who Is Affected |
|---|---|
| Monthly TDS amount | Employee's net take-home salary |
| Annual TDS projection | Spread calculation across remaining FY months |
| HRA Exemption | Only OLD Regime employees with rent paid |
| 80C / 80D / NPS relief | Only OLD Regime employees |
| Tax regime selection | Drives which entire branch of TDS logic executes |

---

*Document generated: September 28, 2026*  
*Source: `InvestmentDeclaration.java` (entity), `InvestmentDeclarationServiceImpl.java`, `IndianSalaryTdsCalculator.java`, `InvestmentDeclaration.jsx` (frontend)*
