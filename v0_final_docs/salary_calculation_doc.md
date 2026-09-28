# Salary Calculation — Complete Technical Reference
**Seravion Connect — HRM Payroll Engine**
*For QA Team: Test Scenarios, Formulas, Thresholds & Edge Cases*

---

## Table of Contents

1. [Salary Components Overview](#1-salary-components-overview)
2. [LOP (Loss of Pay) Calculation](#2-lop-loss-of-pay-calculation)
3. [Gross Salary Calculation](#3-gross-salary-calculation)
4. [PF (Provident Fund) Calculation](#4-pf-provident-fund-calculation)
5. [ESI (Employee State Insurance) Calculation](#5-esi-employee-state-insurance-calculation)
6. [PT (Professional Tax) Calculation](#6-pt-professional-tax-calculation)
7. [TDS (Tax Deducted at Source) Calculation](#7-tds-tax-deducted-at-source-calculation)
8. [Net Salary Calculation](#8-net-salary-calculation)
9. [Employer Cost Calculation](#9-employer-cost-calculation)
10. [Overtime & Holiday Pay](#10-overtime--holiday-pay)
11. [Salary Advance Recovery](#11-salary-advance-recovery)
12. [Sales Commission](#12-sales-commission)
13. [Rounding Policies](#13-rounding-policies)
14. [Negative Net Salary Policy](#14-negative-net-salary-policy)
15. [Salary Revision Mid-Month](#15-salary-revision-mid-month)
16. [Mid-Month Joining / Exit](#16-mid-month-joining--exit)
17. [Payroll Flow — End-to-End Diagram](#17-payroll-flow--end-to-end-diagram)
18. [Complete QA Test Scenarios](#18-complete-qa-test-scenarios)

---

## 1. Salary Components Overview

### Earnings (Included in Gross)

| Component | Source | Notes |
|---|---|---|
| **Basic Salary** | `UserSalaryDetails.basicSalary` | Core pay; LOP deducted here first |
| **HRA** (House Rent Allowance) | `UserSalaryDetails.hra` | Tax-exempt up to limit under Old Regime |
| **Other Allowance** | `UserSalaryDetails.otherAllowance` | Fully taxable |
| **Incentive** | `UserSalaryDetails.incentive` | Fixed incentive; fully taxable |
| **Sales Commission** | Derived: `targetAmount × commissionPercentage / 100` | Task/performance based |
| **Overtime Amount** | Derived from attendance + OT policy | See Section 10 |
| **Holiday Work Amount** | Derived from holiday OT policy | See Section 10 |

### Deductions (Subtracted from Gross)

| Component | Who Pays | Statutory? |
|---|---|---|
| PF Employee | Employee | Yes — 12% of Basic, capped at Rs.15,000 basis |
| ESI Employee | Employee | Yes — 0.75% of Gross, only if Gross <= Rs.21,000 |
| Professional Tax (PT) | Employee | Yes — State-defined slab |
| TDS | Employee | Yes — Per Income Tax Act |
| Other Deductions | Employee | Configurable |
| Advance Recovery | Employee | When salary advance is due |

---

## 2. LOP (Loss of Pay) Calculation

### 2.1 How Absence Days Are Counted

**Absence Days = Full Month Working Days − Payable Days**

```
Payable Days =
  + PRESENT days (1.0 each)
  + HALF_DAY days (0.5 each)
  + Approved PAID LEAVE days (1.0 each — leave type must be "paid")
  + HOLIDAY days (if policy: Holiday = Paid Day)
  + WEEK_OFF days (if policy: WeekOff = Paid Day)

NOT counted as payable: LOP leaves, unapproved absences, ABSENT without approval
```

### 2.2 LOP Denominator

Configured under **Company Payroll Policy → `lopDayBasis`**:

| `lopDayBasis` | Denominator Used |
|---|---|
| `WORKING_DAYS` *(default)* | Actual full-month working days |
| `CALENDAR_DAYS` | Actual calendar days in the month (28/29/30/31) |
| `FIXED_30` | Always 30 |

### 2.3 LOP Formula

```
Daily Rate  = (Basic + HRA + OtherAllowance + Incentive) / LOP Denominator
LOP Amount  = Absence Days x Daily Rate
LOP Amount is capped: cannot exceed the full package (min 0)
```

### 2.4 LOP for HOURLY Salary Type

```
Scheduled Hours = LOP Denominator x Standard Hours per Day
Unpaid Hours    = Scheduled Hours - Payable Hours
LOP Amount      = Unpaid Hours x Hourly Rate
```

---

### QA: LOP Test Cases

| # | Scenario | Input | Expected |
|---|---|---|---|
| L-1 | Full attendance | 26 working days, all PRESENT | Absence = 0, LOP = Rs.0 |
| L-2 | 2 ABSENT days (unapproved) | 2 absent | LOP = 2 x Daily Rate |
| L-3 | 1 HALF_DAY | 1 half-day | Payable += 0.5; LOP = 0.5 x Daily Rate |
| L-4 | Approved LOP Leave (2 days) | LeaveType = LOP, APPROVED | Not paid; LOP still applies |
| L-5 | Approved Paid Leave (2 days) | LeaveType = PL, APPROVED | isPaid = true; LOP = Rs.0 |
| L-6 | CALENDAR_DAYS, January | lopBasis = CALENDAR_DAYS | Denominator = 31 |
| L-7 | FIXED_30 | Any month | Denominator = 30 |
| L-8 | Full month absent | All days absent | LOP capped at full package |
| L-9 | HOURLY, 10 unpaid hours | hourlyRate = Rs.100 | LOP = Rs.1,000 |
| L-10 | Holiday = Paid Day (policy=true) | Absent on public holiday | Holiday in payable days → no LOP |
| L-11 | Holiday = NOT paid (policy=false) | Absent on public holiday | LOP applies |

---

## 3. Gross Salary Calculation

### 3.1 Step-by-Step

```
Step 1 — Adjusted Regular Earnings:
  AdjustedEarnings = (Basic + HRA + OtherAllowance + Incentive + SalesCommission) - LOP Amount
  (AdjustedEarnings is minimum 0)

Step 2 — Gross Earnings:
  GrossEarnings = AdjustedEarnings + OT Amount + Holiday Work Amount
```

> **Note:** ESI, PT, and TDS all use `GrossForStatutory` (same as GrossEarnings).  
> PF uses only `BasicPayable` (Basic after LOP reduction), NOT Gross.

---

## 4. PF (Provident Fund) Calculation

### 4.1 Default Rates

| Parameter | Default |
|---|---|
| Employee PF Rate | 12.00% |
| Employer PF Rate | 12.00% |
| PF Ceiling Enabled | Yes |
| PF Ceiling Wages | Rs.15,000 |

### 4.2 PF Formula

```
PF Wages = BasicPayable (after LOP)
           if ceiling enabled: PF Wages = min(BasicPayable, Rs.15,000)

Employee PF = PF Wages x 12%
Employer PF = PF Wages x 12%
```

> PF is computed on **Basic only** — NOT on Gross.  
> PF ceiling applies to the wage base, not to the rate.

---

### QA: PF Test Cases

| # | Scenario | Input | Expected |
|---|---|---|---|
| PF-1 | Normal with ceiling | Basic = Rs.18,000 | PF Wages = Rs.15,000; Emp PF = Rs.1,800 |
| PF-2 | Low basic (no ceiling trigger) | Basic = Rs.12,000 | PF = Rs.1,440 |
| PF-3 | PF not applicable | `pfApplicable = false` | PF = Rs.0 |
| PF-4 | Basic reduced by LOP | Basic = Rs.15,000; 3 days LOP | BasicPayable < Rs.15,000; PF on reduced amount |
| PF-5 | Ceiling disabled | Basic = Rs.25,000 | PF = Rs.3,000 |
| PF-6 | Custom rate (10%) | Rate = 10% | PF = BasicPayable x 10% |

---

## 5. ESI (Employee State Insurance) Calculation

### 5.1 Default Rates

| Parameter | Default |
|---|---|
| Employee ESI Rate | 0.75% |
| Employer ESI Rate | 3.25% |
| ESI Gross Threshold | Rs.21,000 |

### 5.2 ESI Formula

```
IF GrossForStatutory > Rs.21,000 AND esiPeriodCovered = false:
    Employee ESI = Rs.0
    Employer ESI = Rs.0
ELSE:
    Employee ESI = GrossForStatutory x 0.75%
    Employer ESI = GrossForStatutory x 3.25%
```

### 5.3 ESI Contribution Period Continuity Rule

> This is the most commonly missed compliance scenario.

```
ESI Contribution Periods:
  Apr-Sep: key = "APR-SEP-{YEAR}"
  Oct-Mar: key = "OCT-MAR-{YEAR}"

Rule:
  1. First month of a period: if Gross <= Rs.21,000 → employee is "covered" for this period
  2. Subsequent months in same period: even if Gross > Rs.21,000 → ESI still deducted
     (esiPeriodCovered = true)
  3. Next period starts fresh — re-evaluate based on new gross
```

---

### QA: ESI Test Cases

| # | Scenario | Input | Expected |
|---|---|---|---|
| ESI-1 | Gross below threshold | Gross = Rs.18,000 | Emp ESI = Rs.135; Er ESI = Rs.585 |
| ESI-2 | Gross exactly at threshold | Gross = Rs.21,000 | Emp ESI = Rs.157.50; Er ESI = Rs.682.50 |
| ESI-3 | Gross above threshold | Gross = Rs.22,000 | ESI = Rs.0 |
| ESI-4 | ESI not applicable | `esiApplicable = false` | ESI = Rs.0 |
| ESI-5 | Period covered, gross exceeds | Gross = Rs.23,000; `esiPeriodCovered = true` | ESI still applies |
| ESI-6 | LOP reduces gross below threshold | Pre-LOP Gross = Rs.22,000; post-LOP = Rs.19,000 | ESI applies on Rs.19,000 |
| ESI-7 | OT pushes gross above threshold | Base = Rs.19,000; OT = Rs.3,000 → Rs.22,000 | ESI = Rs.0 |
| ESI-8 | Not covered + gross above | Gross = Rs.22,000; `esiPeriodCovered = false` | ESI = Rs.0 |
| ESI-9 | Zero gross (full absent) | All LOP | ESI = Rs.0 |

---

## 6. PT (Professional Tax) Calculation

### 6.1 How PT Works

- PT is **state-specific**, configured as slabs via `ProfessionalTaxSlab`
- PT is a **flat monthly amount** (not a percentage) based on the gross slab
- Gender-specific slabs take priority over `ALL` gender slabs
- Only slabs within `effectiveFrom` / `effectiveTo` date range apply

### 6.2 PT Formula

```
1. Resolve employee's state code
2. Fetch all PT slabs for that state (ordered by minSalary ASC)
3. Match slab: minSalary <= GrossForStatutory <= maxSalary
4. Prefer gender-specific slab; fallback to ALL-gender slab
5. PT = matched slab's taxAmount (flat monthly rupee amount)
6. No match OR no state code → PT = Rs.0
```

### 6.3 Illustrative PT Slabs (Admin-Configured)

**Maharashtra (MH):**

| Gross Range (Monthly) | PT |
|---|---|
| Rs.0 – Rs.7,500 | Rs.0 |
| Rs.7,501 – Rs.10,000 | Rs.175 |
| Rs.10,001 and above | Rs.200 (Rs.300 in February) |

**Karnataka (KA):**

| Gross Range | PT |
|---|---|
| Rs.0 – Rs.14,999 | Rs.0 |
| Rs.15,000 and above | Rs.200 |

---

### QA: PT Test Cases

| # | Scenario | Input | Expected |
|---|---|---|---|
| PT-1 | Gross matches middle slab | MH, Gross = Rs.12,000 | PT = Rs.200 |
| PT-2 | Gross below any slab | MH, Gross = Rs.5,000 | PT = Rs.0 |
| PT-3 | No state code set | stateCode = null | PT = Rs.0 |
| PT-4 | State with no slabs configured | stateCode = "XY" | PT = Rs.0 |
| PT-5 | Gender-specific slab exists | Female employee, state has FEMALE slab | Use FEMALE slab |
| PT-6 | Slab expired | slab.effectiveTo = yesterday | Not applied → PT = Rs.0 |
| PT-7 | Slab not yet effective | slab.effectiveFrom = tomorrow | Not applied → PT = Rs.0 |
| PT-8 | Open-ended slab (maxSalary = null) | Gross = Rs.50,000 | Matches if Gross >= minSalary |

---

## 7. TDS (Tax Deducted at Source) Calculation

### 7.1 Key Concept

TDS is a **projected annual liability** calculation. Each month, the engine:
1. Estimates the full-year gross income
2. Computes the annual tax liability
3. Subtracts TDS already deducted in prior months
4. Spreads the balance across remaining FY months

### 7.2 Tax Regimes

| Regime | Deductions Allowed |
|---|---|
| **NEW** (default) | Standard Deduction + PT only |
| **OLD** | Std Deduction + PT + 80C + 80D + NPS + HRA Exemption |

### 7.3 TDS Step-by-Step

```
Step 1: Estimated Annual Gross
  EstimatedAnnual = YTD Gross (PAID months in this FY, excluding current)
                  + Current Month Gross
                  + Projected Future Gross (currentGross x months remaining after current)

Step 2: Deductions
  Standard Deduction = Rs.75,000 (NEW) or Rs.50,000 (OLD)  [admin-configured]
  Annual PT = ytdPT + currentPT + (currentPT x months remaining after current)

  OLD Regime only:
    Sec 80C  = min(PPF + ELSS + LIC + NSC, Rs.1,50,000)
    Sec 80D  = min(health insurance declared, cap)
    NPS      = min(NPS declared, cap)
    HRA Exemption = min(
                      HRA Received annually,
                      Rent Paid - 10% of Annual Basic,
                      50% of Annual Basic [metro] or 40% [non-metro]
                    )

Step 3: Taxable Income
  TaxableIncome = EstimatedAnnual - StdDeduction - AnnualPT - OtherDeductions
  Minimum: Rs.0

Step 4: Apply Progressive Tax Slabs (admin-configured per regime)

Step 5: Sec 87A Rebate
  If TaxableIncome <= RebateThreshold → TaxBeforeCess = Rs.0

Step 6: Health & Education Cess
  Cess = TaxBeforeCess x 4%
  AnnualTaxLiability = TaxBeforeCess + Cess

Step 7: Monthly TDS
  Balance = AnnualTaxLiability - TDS Already Deducted (PAID months only)
  MonthlyTDS = max(0, Balance / monthsLeftIncludingCurrent)
```

### 7.4 NEW Regime Tax Slabs (FY 2025-26)

| Income Slab | Rate |
|---|---|
| Rs.0 – Rs.4,00,000 | 0% |
| Rs.4,00,001 – Rs.8,00,000 | 5% |
| Rs.8,00,001 – Rs.12,00,000 | 10% |
| Rs.12,00,001 – Rs.16,00,000 | 15% |
| Rs.16,00,001 – Rs.20,00,000 | 20% |
| Rs.20,00,001 – Rs.24,00,000 | 25% |
| Above Rs.24,00,000 | 30% |

**Sec 87A Rebate (NEW):** Taxable Income <= Rs.12,00,000 → Tax = Rs.0

### 7.5 OLD Regime Tax Slabs

| Income Slab | Rate |
|---|---|
| Rs.0 – Rs.2,50,000 | 0% |
| Rs.2,50,001 – Rs.5,00,000 | 5% |
| Rs.5,00,001 – Rs.10,00,000 | 20% |
| Above Rs.10,00,000 | 30% |

**Sec 87A Rebate (OLD):** Taxable Income <= Rs.5,00,000 → Tax = Rs.0

---

### QA: TDS Test Cases

| # | Scenario | Annual Gross | Regime | Expected |
|---|---|---|---|---|
| TDS-1 | Below 87A rebate (New) | Rs.11,00,000 | NEW | Tax = Rs.0 (within Rs.12L limit) |
| TDS-2 | Just above 87A rebate (New) | Rs.12,10,000 | NEW | Tax on full slab amount (no rebate) |
| TDS-3 | Old regime, full 80C | Rs.8,00,000; 80C=Rs.1,50,000 | OLD | Taxable = 8L-50K-1.5L = Rs.6L; Tax = Rs.52,500 + cess |
| TDS-4 | New regime, mid-year joiner | Rs.7,50,000 (6 months left) | NEW | Spread over 6 months |
| TDS-5 | HRA exemption applied | Metro, rent > 10% basic | OLD | HRA portion exempt from taxable |
| TDS-6 | TDS not applicable | `tdsApplicable = false` | Any | TDS = Rs.0 |
| TDS-7 | TDS already partly deducted | Rs.40,000 deducted; annual = Rs.60,000 | Any | Monthly = Rs.20,000 / months left |
| TDS-8 | Overpaid TDS (balance negative) | Annual liability < already deducted | Any | Monthly TDS = Rs.0 (min 0) |
| TDS-9 | NPS + 80D old regime | NPS=Rs.50K; 80D=Rs.25K | OLD | Both deducted from taxable |
| TDS-10 | Annual gross = Rs.0 | New joiner, no salary yet | NEW | TDS = Rs.0 |
| TDS-11 | Only 1 month left in FY | March, last FY month | Any | Balance / 1 |

---

## 8. Net Salary Calculation

### 8.1 Formula

```
Total Employee Deductions =
    Employee PF
  + Employee ESI
  + Professional Tax
  + TDS
  + Other Deductions (custom)
  + Other Deductions (manual)
  + Advance Recovery

Net Salary = Gross Earnings - Total Employee Deductions
```

> **IMPORTANT:** Employer PF and Employer ESI are **never** subtracted from the employee's Net Salary.  
> They only appear in the Employer Cost figure.

---

### QA: Net Salary Test Cases

| # | Scenario | Input | Expected |
|---|---|---|---|
| N-1 | Standard full month | Gross=Rs.35,000; PF=Rs.1,800; PT=Rs.200; TDS=Rs.0; ESI=Rs.0 | Net = Rs.33,000 |
| N-2 | ESI below threshold | Gross=Rs.17,000; ESI=Rs.127.50; PF=Rs.1,200; PT=Rs.0; TDS=Rs.0 | Net = Rs.15,672.50 |
| N-3 | Negative net, BLOCK | Gross=Rs.5,000; advance=Rs.8,000 | Error thrown |
| N-4 | Negative net, ALLOW | Same above, policy=ALLOW | Net = -Rs.3,000 (saved) |
| N-5 | Advance partial recovery | Rs.5,000 advance; Rs.2,000 EMI due | Deduct Rs.2,000 this month |

---

## 9. Employer Cost Calculation

```
Employer Cost = Gross Earnings + Employer PF + Employer ESI
```

This represents the **total monthly CTC** from the company's perspective.

---

## 10. Overtime & Holiday Pay

### 10.1 OT Types

| `OvertimeType` | Calculation |
|---|---|
| `PER_HOUR` | OT Hours x Per Hour Rate |
| `SHIFT_INCENTIVE` | Flat daily incentive for qualifying shift |

### 10.2 PER_HOUR OT Calculation

```
Daily OT Hours = max(0, Actual Hours Worked - Standard Hours)
Monthly OT Amount = sum of daily OT amounts
Monthly Cap: if OT Amount > monthlyCap → cap it at monthlyCap
```

### 10.3 Holiday Work Pay

```
If employee works on a holiday AND holidayWorkApplicable = true:
  Holiday Amount calculated per configured OT formula
  (added to Gross as a separate line item)
```

### 10.4 Application User OT Gate (Field Technician Rule)

> If `isApplicationUser = true`:  
> OT is **suppressed** on any day where no field **Task** with ONGOING/COMPLETED status exists.  
> This prevents non-field time entries from inflating OT.

---

### QA: OT Test Cases

| # | Scenario | Input | Expected |
|---|---|---|---|
| OT-1 | 2 hrs OT x 5 days, rate Rs.100/hr | OT rate = Rs.100 | OT = Rs.1,000 |
| OT-2 | Monthly cap hit | Cap = Rs.2,000; computed OT = Rs.3,000 | OT = Rs.2,000 (capped) |
| OT-3 | App user, no task that day | `isApplicationUser=true`, no task | OT = Rs.0 (suppressed) |
| OT-4 | App user, task exists | Task in ONGOING state | OT computed normally |
| OT-5 | Holiday worked, policy enabled | 1 holiday day, fully worked | Holiday pay added to gross |
| OT-6 | Holiday worked, policy disabled | `holidayWorkApplicable=false` | Holiday amount = Rs.0 |

---

## 11. Salary Advance Recovery

```
Recovery this month = amount due per repayment schedule

AdvanceRecoveryType:
  FIXED_EMI     → same fixed amount each month
  FULL_RECOVERY → entire advance deducted in one go
```

Recovery is included in Total Deductions → reduces Net Salary.

---

### QA: Advance Test Cases

| # | Scenario | Input | Expected |
|---|---|---|---|
| ADV-1 | Fixed EMI | Gross=Rs.30,000; EMI=Rs.500 | Net reduced by Rs.500 |
| ADV-2 | Full recovery | Gross=Rs.30,000; advance=Rs.5,000 FULL | Rs.5,000 deducted |
| ADV-3 | Recovery exceeds net (BLOCK policy) | Net=Rs.2,000; recovery=Rs.5,000 | Error thrown |

---

## 12. Sales Commission

```
SalesCommission = TargetAmount x CommissionPercentage / 100
Only computed when: targetAmount > 0 AND commissionPercentage > 0
Commission IS included in Gross Earnings and GrossForStatutory
```

---

## 13. Rounding Policies

| Policy | Behavior |
|---|---|
| `ROUND_TO_2_DECIMAL` *(default)* | 2 decimal places, HALF_UP rounding |
| `ROUND_TO_NEAREST_RUPEE` | Round to whole rupee |
| `FLOOR_TO_RUPEE` | Always round down |
| `CEIL_TO_RUPEE` | Always round up |

---

## 14. Negative Net Salary Policy

| Policy | Behavior |
|---|---|
| `BLOCK` *(default)* | Throws exception: "Net salary cannot be negative" |
| `ALLOW` | Net saved as-is even if negative |
| `ALLOW_WITH_APPROVAL` | Negative allowed only when `allowNegativeApproved = true` passed |

---

## 15. Salary Revision Mid-Month

When a salary revision is approved with an effective date **within** the current month:

```
OldPackage = OldBasic + OldHRA + OldOtherAllowance + Incentive
NewPackage = NewBasic + NewHRA + NewOtherAllowance + Incentive

BlendedEarnings = (OldPackage x OldWorkingDays + NewPackage x NewWorkingDays)
                  / FullMonthWorkingDays

PF Base (blended):
  BasicForPF = (OldBasic x OldWorkingDays + NewBasic x NewWorkingDays)
               / FullMonthWorkingDays
```

---

### QA: Revision Test Cases

| # | Scenario | Input | Expected |
|---|---|---|---|
| REV-1 | Revision on day 16 (of 30 working days) | Old=Rs.20K; New=Rs.25K; 15+15 split | Blended = Rs.22,500 |
| REV-2 | Revision on day 1 | All days new | Full new package |
| REV-3 | Revision on last working day | 1 new day | Mostly old package |
| REV-4 | No revision this month | `monthRevision = null` | Use configured package as-is |

---

## 16. Mid-Month Joining / Exit

```
empFrom = max(Date of Joining, month start)
empTo   = min(Last Working Date or month end, month end)

Days before DOJ  → not employed → not in payableDays
Days before DOJ  → DO count in fullMonthWorkingDays (LOP denominator)
```

### Example: Joined on 11th, 26-working-day month

```
fullMonthWorkingDays = 26
employedWorkingDays = 16 (days 11-26)
LOP Denominator = 26 (WORKING_DAYS basis)
Daily Rate = Package / 26
Days 1-10 → not employed → counted as absence → LOP applied
```

---

### QA: Joining/Exit Test Cases

| # | Scenario | Input | Expected |
|---|---|---|---|
| J-1 | Joined mid-month on 15th | DOJ = 15th, all present from 15th | Days 1-14 are LOP (not employed) |
| J-2 | Exited on 20th (LWD) | All present 1-20 | Days 21+ not payable |
| J-3 | Joined and exited same month | DOJ=5th, LWD=25th | Only 5th-25th payable |
| J-4 | DOJ before month start | Already employed, no exit | Full month counted normally |
| J-5 | DOJ after month-end | Future employee | empFrom > empTo → zero salary |

---

## 17. Payroll Flow — End-to-End Diagram

```
Employee + Month/Year
        |
        v
Resolve: DOJ, LWD, Attendance records, Holidays, WeekOffs, Approved Leaves
        |
        v
Day-by-Day Loop:
  Classify each day: PRESENT / HALF_DAY / ABSENT / HOLIDAY / WEEK_OFF / LEAVE
  Count: fullMonthWorkingDays, employedDays, payableDays
  Compute: OT hours & amounts per day
        |
        v
LOP Calculation:
  absenceDays = fullMonthWorkingDays - payableDays
  dailyRate = earningBase / LOP_Denominator
  lopAmount = absenceDays x dailyRate  (capped at package)
        |
        v
Earnings Assembly:
  adjustedEarnings = package - lopAmount
  gross = adjustedEarnings + OT + holidayAmount + salesCommission
        |
        v
Statutory Computation:
  PF  -> on BasicPayable only (capped at Rs.15,000 wage basis)
  ESI -> on gross (only if <= Rs.21,000 OR esiPeriodCovered = true)
  PT  -> slab lookup by state, gross
  TDS -> annualized projection / spread over remaining FY months
        |
        v
Net Salary:
  net = gross - (empPF + empESI + PT + TDS + otherDed + advanceRecovery)
  Apply Negative Net Policy check
        |
        v
Employer Cost:
  employerCost = gross + employerPF + employerESI
        |
        v
Save HrmSalaryMonth record
```

---

## 18. Complete QA Test Scenarios — Worked Examples

### Scenario 1: Standard Full-Month Employee (New Regime)

**Input:**
- Basic = Rs.20,000 | HRA = Rs.8,000 | OA = Rs.5,000 | Incentive = Rs.2,000
- All 26 working days PRESENT | State: MH | PF ceiling enabled | ESI applicable
- TDS: New Regime, April (month 1 of FY)

**Calculation:**

```
Package     = Rs.35,000
LOP         = Rs.0
Gross       = Rs.35,000

PF Wages    = min(Rs.20,000, Rs.15,000) = Rs.15,000
Employee PF = Rs.1,800  |  Employer PF = Rs.1,800

ESI: Gross Rs.35,000 > Rs.21,000 → ESI = Rs.0

PT (MH): Gross > Rs.10,000 → PT = Rs.200

TDS (April, New Regime):
  EstimatedAnnual = Rs.35,000 x 12 = Rs.4,20,000
  StdDeduction    = Rs.75,000
  AnnualPT        = Rs.200 x 12 = Rs.2,400
  Taxable         = Rs.4,20,000 - Rs.75,000 - Rs.2,400 = Rs.3,42,600
  Tax (New slab)  = Rs.0 (below Rs.4,00,000 free slab)
  Monthly TDS     = Rs.0

Net = Rs.35,000 - Rs.1,800 - Rs.200 = Rs.33,000
Employer Cost = Rs.35,000 + Rs.1,800 = Rs.36,800
```

---

### Scenario 2: Employee with 3 Days LOP

**Input:** Same as Scenario 1, but 3 ABSENT days. LOP Basis = WORKING_DAYS (26 days).

```
Daily Rate   = Rs.35,000 / 26 = Rs.1,346.15
LOP Amount   = 3 x Rs.1,346.15 = Rs.4,038.46

Adjusted     = Rs.35,000 - Rs.4,038.46 = Rs.30,961.54
Gross        = Rs.30,961.54

BasicPayable = Rs.20,000 - (3 x Rs.20,000/26) = Rs.17,692.31
PF Wages     = min(Rs.17,692.31, Rs.15,000) = Rs.15,000
Employee PF  = Rs.1,800

ESI: Gross Rs.30,961 > Rs.21,000 → Rs.0
PT: Rs.200

Net = Rs.30,961.54 - Rs.1,800 - Rs.200 = Rs.28,961.54
```

---

### Scenario 3: Low-Salary Employee (ESI Applies)

**Input:**
- Basic=Rs.10,000 | HRA=Rs.4,000 | OA=Rs.3,000 | Full attendance | State: KA

```
Gross        = Rs.17,000

PF Wages     = Rs.10,000; Employee PF = Rs.1,200; Employer PF = Rs.1,200
ESI: Gross Rs.17,000 <= Rs.21,000
  Employee ESI = Rs.17,000 x 0.75% = Rs.127.50
  Employer ESI = Rs.17,000 x 3.25% = Rs.552.50

PT (KA): Gross < Rs.15,000 → PT = Rs.0
TDS: Annual = Rs.2,04,000 → below Rs.4L slab → TDS = Rs.0

Net = Rs.17,000 - Rs.1,200 - Rs.127.50 = Rs.15,672.50
Employer Cost = Rs.17,000 + Rs.1,200 + Rs.552.50 = Rs.18,752.50
```

---

### Scenario 4: High-Salary, Old Regime, with All Deductions

**Input:**
- Monthly Gross = Rs.1,25,000 (Annual = Rs.15,00,000)
- Old Regime | 80C = Rs.1,50,000 | 80D = Rs.25,000 | NPS = Rs.50,000
- Metro city | Rent paid = Rs.2,40,000/year | Monthly HRA = Rs.50,000

**Annual TDS Calculation:**

```
EstimatedAnnual = Rs.15,00,000
StdDeduction    = Rs.50,000
AnnualPT        = Rs.200 x 12 = Rs.2,400

Old Regime Deductions:
  80C         = Rs.1,50,000
  80D         = Rs.25,000
  NPS         = Rs.50,000
  Annual Basic (approx) = Rs.60,000 x 12 = Rs.7,20,000
  10% of Basic = Rs.72,000
  50% of Basic (metro) = Rs.3,60,000
  Rent - 10%Basic = Rs.2,40,000 - Rs.72,000 = Rs.1,68,000
  HRA Exempt  = min(Rs.6,00,000, Rs.1,68,000, Rs.3,60,000) = Rs.1,68,000

Taxable = Rs.15,00,000 - Rs.50,000 - Rs.2,400 - Rs.1,50,000 - Rs.25,000 - Rs.50,000 - Rs.1,68,000
        = Rs.11,54,600

Old Regime Tax:
  Rs.0-2.5L   = Rs.0
  Rs.2.5L-5L  = Rs.12,500
  Rs.5L-10L   = Rs.1,00,000
  Rs.10L-11.546L = Rs.46,380
  Total Tax   = Rs.1,58,880
  Cess (4%)   = Rs.6,355.20
  Annual Liability = Rs.1,65,235.20
  Monthly TDS = Rs.1,65,235.20 / 12 = Rs.13,769.60
```

---

### Scenario 5: ESI Contribution Period Continuity (Apr-Sep)

| Month | Gross | Covered? | ESI Deducted? |
|---|---|---|---|
| April | Rs.18,000 | Yes (enrolled) | Yes — Rs.135 emp / Rs.585 er |
| May | Rs.18,000 | Yes | Yes |
| June | Rs.18,000 | Yes | Yes |
| July | Rs.22,000 | Yes (period continues) | **Yes** (esiPeriodCovered = true) |
| August | Rs.22,000 | Yes (period continues) | **Yes** |
| September | Rs.22,000 | Yes (period continues) | **Yes** |
| October | Rs.22,000 | New period; not covered | **No** (gross > Rs.21,000) |

---

### Scenario 6: Salary Revision on 16th of 30-Working-Day Month

```
OldPackage = Rs.18,000 + Rs.7,000 + Rs.3,000 = Rs.28,000  (days 1-15)
NewPackage = Rs.22,000 + Rs.9,000 + Rs.4,000 = Rs.35,000  (days 16-30)

Blended = (Rs.28,000 x 15 + Rs.35,000 x 15) / 30 = Rs.31,500
LOP Daily Rate = Rs.31,500 / 30 = Rs.1,050

BasicForPF = (Rs.18,000 x 15 + Rs.22,000 x 15) / 30 = Rs.20,000
PF Wages   = min(Rs.20,000, Rs.15,000) = Rs.15,000 → PF = Rs.1,800
```

---

### Scenario 7: Negative Net — Advance Recovery Exceeds Net

```
Gross = Rs.15,000
Deductions:
  PF     = Rs.1,000
  ESI    = Rs.112.50
  PT     = Rs.175
  TDS    = Rs.0
  Advance = Rs.16,000

Total Deductions = Rs.17,287.50
Net = Rs.15,000 - Rs.17,287.50 = -Rs.2,287.50

Policy = BLOCK          → Error: "Net salary cannot be negative"
Policy = ALLOW          → Net = -Rs.2,287.50 saved
Policy = ALLOW_WITH_APPROVAL (approved) → Net = -Rs.2,287.50 saved
Policy = ALLOW_WITH_APPROVAL (not approved) → Error thrown
```

---

## Key Configuration Reference

| Configuration | DB/Code Path | Default |
|---|---|---|
| PF Employee Rate | `SalaryDetails.pfEmployeeRate` | 12.00% |
| PF Employer Rate | `SalaryDetails.pfEmployerRate` | 12.00% |
| PF Ceiling Amount | `HrmCompanyPayrollPolicy` | Rs.15,000 |
| ESI Employee Rate | `SalaryDetails.esiEmployeeRate` | 0.75% |
| ESI Employer Rate | `SalaryDetails.esiEmployerRate` | 3.25% |
| ESI Threshold | `HrmCompanyPayrollPolicy` | Rs.21,000 |
| LOP Day Basis | `HrmCompanyPayrollPolicy.lopDayBasis` | WORKING_DAYS |
| Payroll Rounding | `HrmCompanyPayrollPolicy.payrollRounding` | ROUND_TO_2_DECIMAL |
| Negative Net Policy | `HrmCompanyPayrollPolicy.negativeNetPolicy` | BLOCK |
| Std Deduction (New) | `HrmCompanyPayrollPolicy.stdDeductionNew` | Rs.75,000 |
| Std Deduction (Old) | `HrmCompanyPayrollPolicy.stdDeductionOld` | Rs.50,000 |
| 80C Cap | `IncomeTaxRuleBundle.section80cCap` | Rs.1,50,000 |
| Default Tax Regime | `HrmCompanyPayrollPolicy.defaultTaxRegime` | NEW |
| Sec 87A Rebate Threshold | `IncomeTaxRuleBundle.rebate87aThreshold` | Rs.12,00,000 (New) |

---

*Document generated: September 28, 2026*  
*Source code reference: `HrmPayrollComputeServiceImpl.java`, `HrmStatutoryComputeServiceImpl.java`, `IndianSalaryTdsCalculator.java`, `GrossNetCalculator.java`*
