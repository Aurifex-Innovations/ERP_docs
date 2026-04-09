# Frontend Sprint Plan — Apr 10 to Apr 14, 2026 (5 Days)

---

## Current State Snapshot

| Item | Status |
| :--- | :--- |
| **Module 1–20 Frontend** | Static UI exists — ~50% needs Figma alignment fixes |
| **Module 21–32 Frontend** | Completely pending (0% done) |
| **Backend Ready** | Module 1–21, 24–32 ✅ |
| **Backend ETA (Pending)** | Module 22, 23 ⏳ (**backend will be provided within 2 days**). Until then UI-only work continues; once backend is available, integration will start immediately. Any backend bugs/errors found during integration will be resolved on an immediate/priority basis. |

---

## Developer Roster

| Dev | Exp | Availability | Current Status | Assigned Track |
| :--- | :--- | :--- | :--- | :--- |
| **Araman** | 1 yr | 4 hrs + weekend extra | Working on Mod 1–20 fixes | 🔵 Track 1 (Fix 1–20) |
| **Ansh** | 6 mo | 4 hrs | Started Mod 6 UI, good UI skills | 🔵 Track 1 (Fix 1–20) |
| **Raj** | 3 yr | **Full-time (8 hrs)** | Available | 🟠 Track 2 (Build 21–32) |
| **Vipul** | 8 yr | 4 hrs | Started Mod 24 UI | 🟠 Track 2 (Build 21–32) |
| **Uday** | 3 yr | 4 hrs + weekend extra | Available | 🟠 Track 2 (Build 21–32) |

> **Note:** Nikhil is currently removed from the plan. If he joins, modules will be reassigned to him at that point.

---

## 🔵 TRACK 1 — Fix Module 1–20 (Araman + Ansh)

Figma alignment, missing screens, responsive fixes, and API integration where backend is ready.

### Module Split

**Araman** (more experienced — takes modules with heavier logic + integration):

| Module Group | Modules | Notes |
| :--- | :--- | :--- |
| Auth + Platform | 1, 2, 3, 4 | Login flows, onboarding, admin panel, subscription |
| Org Setup | 7, 8 | Branch (simpler), Employee (complex — tabs, hiring flow) |
| Inventory | 11, 13, 14 | Stock, Vendor, PO — table views + detail screens |
| Sales Pipeline | 15, 20 | Leads (pipeline view), SO (most complex — 3 source types) |

**Ansh** (Good UI + fresher — takes config/form-heavy modules):

| Module Group | Modules | Notes |
| :--- | :--- | :--- |
| RBAC + Config | 5, 6 | Role management (already started 6), salary/leave config |
| Master Data | 9, 10, 12 | Tax, Products, Services — config forms + table views |
| Sales Pipeline | 16, 17, 18, 19 | Quotation, GMA, Customer, Contract — forms + status flows |

---

### 🔵 Track 1 — Day-Wise Plan

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  TRACK 1: FIX MODULE 1–20 (Figma Align + Integration)                       │
├──────────┬──────────────────────────────┬───────────────────────────────────┤
│  Day     │  ARAMAN                      │  ANSH                            │
├──────────┼──────────────────────────────┼───────────────────────────────────┤
│          │                              │                                   │
│  Day 1   │  Mod 1, 2 — UI Fix          │  Mod 5, 6 — UI Fix               │
│  Apr 10  │  (Auth screens, Onboarding)  │  (RBAC, Salary/Leave Config)     │
│  Fri     │                              │  (Mod 6 already started)          │
│          │                              │                                   │
├──────────┼──────────────────────────────┼───────────────────────────────────┤
│          │                              │                                   │
│  Day 2   │  Mod 3, 4, 7 — UI Fix       │  Mod 9, 10 — UI Fix              │
│  Apr 11  │  (Super Admin, Subs, Branch) │  (Tax Mgmt, Product Master)      │
│  Sat ⬆️  │  + Integration: 1, 2        │  + Integration: 5, 6              │
│          │                              │                                   │
├──────────┼──────────────────────────────┼───────────────────────────────────┤
│          │                              │                                   │
│  Day 3   │  Mod 8 — UI Fix             │  Mod 12, 16 — UI Fix             │
│  Apr 12  │  (Employee — heavy, multi-   │  (Service Mgmt, Quotation Mgmt)  │
│  Sun ⬆️  │   tab, hiring flow)          │  + Integration: 9, 10            │
│          │  + Integration: 3, 4, 7      │                                   │
│          │                              │                                   │
├──────────┼──────────────────────────────┼───────────────────────────────────┤
│          │                              │                                   │
│  Day 4   │  Mod 11, 13 — UI Fix        │  Mod 17, 18 — UI Fix             │
│  Apr 13  │  (Stock, Vendor)             │  (GMA, Customer Mgmt)            │
│  Mon     │  + Integration: 8            │  + Integration: 12, 16           │
│          │                              │                                   │
├──────────┼──────────────────────────────┼───────────────────────────────────┤
│          │                              │                                   │
│  Day 5   │  Mod 14, 15, 20 — UI Fix    │  Mod 19 — UI Fix                 │
│  Apr 14  │  (PO, Leads, Sales Order)    │  (Contract Mgmt)                 │
│  Tue     │  + Integration: 11, 13       │  + Integration: 17, 18, 19       │
│          │                              │                                   │
│          │  ⚠️ Mod 20 integration may   │                                   │
│          │  spill — flag if behind       │                                   │
└──────────┴──────────────────────────────┴───────────────────────────────────┘

⬆️ = Extra hours available (weekend)
```

> **Realistic Note:** Module 8 (Employee) and Module 20 (Sales Order) are the two heaviest modules in this track. If time runs short, deprioritize Module 14 (PO) and Module 3 (Super Admin) UI polish — these are lower-traffic screens.

---

## 🟠 TRACK 2 — Build Module 21–32 from Scratch (Raj, Vipul, Uday)

### Module Complexity & Assignment Logic

| Module | Complexity | Screens | Backend Ready? | Assigned To | Reason |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 21 (Task Mgmt) | 🔴 Very High | ~12 | ✅ | **Raj** | ✅ Already done |
| 22 (Location) | 🟡 Medium | ~4 | ❌ | **Raj** | Almost done. UI only (backend pending) |
| 23 (Support) | 🟡 Medium | ~6 | ❌ | **Raj** | Almost done. UI only (backend pending) |
| 24 (Petty Cash) | 🟡 Medium | ~6 | ✅ | **Vipul** | Already started. Finish + integrate |
| 25 (HRM) | 🔴 Very High | ~15 | ✅ | **Uday** | Attendance + Leave + Payroll — 3 sub-modules |
| 26 (Performance) | 🟠 High | ~6 | ✅ | **Uday** | Dashboards. Reads from Mod 21/25 |
| 27 (User Profile) | 🟢 Low | ~4 | ✅ | **Uday** | Simple self-service. Quick build |
| 28 (Invoice) | 🔴 Very High | ~8 | ✅ | **Raj** | Complex forms + PDF + Credit Notes |
| 29 (Bills) | 🟠 High | ~7 | ✅ | **Vipul** | Mirror of 28 (purchase side). Senior dev can reuse patterns |
| 30 (Payments) | 🔴 Very High | ~8 | ✅ | **Raj** | Receipts + Vouchers + Allocation — most complex finance module |
| 31 (Ledger) | 🟡 Medium | ~4 | ✅ | **Raj** | List + detail + filter views |
| 32 (COA) | 🟡 Medium | ~4 | ✅ | **Raj** | Tree/list structure. Config module — build before Ledger |

---

### 🟠 Track 2 — Day-Wise Plan

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  TRACK 2: BUILD MODULE 21–32 (New UI + Integration)                                  │
│  ⚡ Raj has already completed Mod 21 and nearly finished Mod 22, 23                  │
├──────────┬──────────────────┬──────────────────┬─────────────────────────────────────┤
│  Day     │  RAJ (8hrs)      │  VIPUL (4hrs)    │  UDAY (4hrs)                        │
│          │  Full-time       │  Senior (8yr)    │  3yr exp                            │
├──────────┼──────────────────┼──────────────────┼─────────────────────────────────────┤
│          │                  │                  │                                     │
│  Day 1   │  Mod 22, 23 —    │  Mod 24 — UI     │  Mod 25 — UI                        │
│  Apr 10  │  Finish remaining│  (Finish + Start │  (Attendance sub-module)             │
│  Fri     │  UI work         │   Integration)   │                                     │
│          │  + Mod 32 — UI   │                  │                                     │
│          │  (COA — config   │                  │                                     │
│          │   tree/list)     │                  │                                     │
│          │                  │                  │                                     │
├──────────┼──────────────────┼──────────────────┼─────────────────────────────────────┤
│          │                  │                  │                                     │
│  Day 2   │  Mod 32 — Integ  │  Mod 24 — Integ  │  Mod 25 — UI                        │
│  Apr 11  │  + Mod 31 — UI   │                  │  (Leave Apply/Approve sub-module)   │
│  Sat ⬆️  │  (Ledger list,   │                  │  ⬆️ extra hrs                       │
│          │   detail, filter)│                  │                                     │
│          │  + Mod 28 — UI   │                  │                                     │
│          │  Start (Invoice) │                  │                                     │
│          │                  │                  │                                     │
├──────────┼──────────────────┼──────────────────┼─────────────────────────────────────┤
│          │                  │                  │                                     │
│  Day 3   │  Mod 31 — Integ  │  Mod 29 — UI     │  Mod 25 — UI Finish (Payroll)       │
│  Apr 12  │  + Mod 28 — UI   │  (Bills — mirror  │  + Integration Start                │
│  Sun ⬆️  │  continued       │   of Invoice      │  + Mod 27 — UI + Integ              │
│          │  (Invoice Create,│   structure)     │  (User Profile — quick build)       │
│          │   List, Detail,  │                  │  ⬆️ extra hrs                       │
│          │   Credit Note)   │                  │                                     │
│          │                  │                  │                                     │
├──────────┼──────────────────┼──────────────────┼─────────────────────────────────────┤
│          │                  │                  │                                     │
│  Day 4   │  Mod 28 — Integ  │  Mod 29 — Integ  │  Mod 26 — UI                        │
│  Apr 13  │  + Mod 30 — UI   │                  │  (Technician Performance dashboards)│
│  Mon     │  Start (Payments │                  │  + Mod 25 Integration Finish        │
│          │   — Receipt,     │                  │                                     │
│          │   Voucher forms) │                  │                                     │
│          │                  │                  │                                     │
├──────────┼──────────────────┼──────────────────┼─────────────────────────────────────┤
│          │                  │                  │                                     │
│  Day 5   │  Mod 30 — UI     │  Final Polish     │  Mod 26 — Integration               │
│  Apr 14  │  continued +     │  + Review Mod     │  + Help with Track 1 Integ          │
│  Tue     │  Integration     │  24, 29          │  if needed                          │
│          │  (Allocation,    │                  │                                     │
│          │   Advance,       │                  │                                     │
│          │   Shortfall,     │                  │                                     │
│          │   CN/DN auto-gen)│                  │                                     │
│          │                  │                  │                                     │
└──────────┴──────────────────┴──────────────────┴─────────────────────────────────────┘

⬆️ = Extra hours available (weekend)
```

---

## Combined 5-Day Sprint View (All 5 Devs)

```
═══════════════════════════════════════════════════════════════════════════════════════
 DAY 1 — Friday, Apr 10
═══════════════════════════════════════════════════════════════════════════════════════

 🔵 Araman    → Mod 1, 2 UI Fix
 🔵 Ansh      → Mod 5, 6 UI Fix
 🟠 Raj       → Mod 22, 23 Finish + Mod 32 UI (COA)
 🟠 Vipul     → Mod 24 UI Finish + Integration Start
 🟠 Uday      → Mod 25 UI (Attendance sub-module)

═══════════════════════════════════════════════════════════════════════════════════════
 DAY 2 — Saturday, Apr 11  ⬆️ Weekend
═══════════════════════════════════════════════════════════════════════════════════════

 🔵 Araman ⬆️ → Mod 3, 4, 7 UI Fix + Integration (1, 2)
 🔵 Ansh      → Mod 9, 10 UI Fix + Integration (5, 6)
 🟠 Raj       → Mod 32 Integ + Mod 31 UI (Ledger) + Mod 28 UI Start
 🟠 Vipul     → Mod 24 Integration
 🟠 Uday  ⬆️ → Mod 25 UI (Leave sub-module)

═══════════════════════════════════════════════════════════════════════════════════════
 DAY 3 — Sunday, Apr 12  ⬆️ Weekend
═══════════════════════════════════════════════════════════════════════════════════════

 🔵 Araman ⬆️ → Mod 8 UI Fix (Employee — heavy) + Integration (3, 4, 7)
 🔵 Ansh      → Mod 12, 16 UI Fix + Integration (9, 10)
 🟠 Raj       → Mod 31 Integ + Mod 28 UI continued (Invoice)
 🟠 Vipul     → Mod 29 UI (Bills)
 🟠 Uday  ⬆️ → Mod 25 UI Finish (Payroll) + Integ Start + Mod 27 UI+Integ

═══════════════════════════════════════════════════════════════════════════════════════
 DAY 4 — Monday, Apr 13
═══════════════════════════════════════════════════════════════════════════════════════

 🔵 Araman    → Mod 11, 13 UI Fix + Integration (8)
 🔵 Ansh      → Mod 17, 18 UI Fix + Integration (12, 16)
 🟠 Raj       → Mod 28 Integ + Mod 30 UI Start (Payments)
 🟠 Vipul     → Mod 29 Integration
 🟠 Uday      → Mod 26 UI (Performance) + Mod 25 Integration Finish

═══════════════════════════════════════════════════════════════════════════════════════
 DAY 5 — Tuesday, Apr 14  🏁 FINAL DAY
═══════════════════════════════════════════════════════════════════════════════════════

 🔵 Araman    → Mod 14, 15, 20 UI Fix + Integration (11, 13)
 🔵 Ansh      → Mod 19 UI Fix + Integration (17, 18, 19)
 🟠 Raj       → Mod 30 UI + Integration (Allocation, Advance, Shortfall)
 🟠 Vipul     → Final Polish + Review (24, 29)
 🟠 Uday      → Mod 26 Integration + Help with Track 1 Integ if needed

═══════════════════════════════════════════════════════════════════════════════════════
```

---

## Risk Assessment

| Risk | Impact | Mitigation |
| :--- | :--- | :--- |
| **Mod 20 (SO) UI Fix may spill** | High — SO has 3 source types, complex forms | Araman prioritizes SO integration over Mod 14 polish |
| **Mod 30 (Payments) may not finish Day 5** | High — most complex finance module | Raj has full-time bandwidth + starts Day 3 (extra day gained since 21 is done). Vipul can support if Mod 29 finishes early |
| **Mod 25 (HRM) has 3 sub-modules** | Medium — Attendance + Leave + Payroll is a lot | Uday gets extra weekend hours (Day 2–3). Payroll can be UI-only if tight |
| **Mod 22, 23 cannot integrate** | Low — backend not ready | Raj finishes UI on Day 1. Integration planned for post-sprint when backend arrives |
| **Ansh is a fresher** | Low — Config/form modules are simpler | Paired with lower-risk modules (Tax, Products, Services, Quotation). Can ask Araman for help |

---

## Module Completion Forecast (End of Day 5)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  MODULE STATUS FORECAST — End of Apr 14                                      │
├──────────┬────────────────────┬──────────────────────────────────────────────┤
│  Module  │  UI + Integration  │  Notes                                       │
├──────────┼────────────────────┼──────────────────────────────────────────────┤
│  Mod 1   │  ✅ Done           │  Auth — Araman                               │
│  Mod 2   │  ✅ Done           │  Onboarding — Araman                         │
│  Mod 3   │  ✅ Done           │  Super Admin — Araman                        │
│  Mod 4   │  ✅ Done           │  Subscription — Araman                       │
│  Mod 5   │  ✅ Done           │  RBAC — Ansh                                 │
│  Mod 6   │  ✅ Done           │  Salary Config — Ansh                        │
│  Mod 7   │  ✅ Done           │  Branch — Araman                             │
│  Mod 8   │  ✅ Done           │  Employee — Araman                           │
│  Mod 9   │  ✅ Done           │  Tax — Ansh                                  │
│  Mod 10  │  ✅ Done           │  Products — Ansh                             │
│  Mod 11  │  ✅ Done           │  Stock — Araman                              │
│  Mod 12  │  ✅ Done           │  Services — Ansh                             │
│  Mod 13  │  ✅ Done           │  Vendor — Araman                             │
│  Mod 14  │  ✅ Done           │  PO — Araman                                 │
│  Mod 15  │  ✅ Done           │  Leads — Araman                              │
│  Mod 16  │  ✅ Done           │  Quotation — Ansh                            │
│  Mod 17  │  ✅ Done           │  GMA — Ansh                                  │
│  Mod 18  │  ✅ Done           │  Customer — Ansh                             │
│  Mod 19  │  ✅ Done           │  Contract — Ansh                             │
│  Mod 20  │  ✅ Done           │  SO — Araman                                 │
│  Mod 21  │  ✅ Done           │  Task Mgmt — Raj                             │
│  Mod 22  │  ✅ Done           │  Location — Raj                              │
│  Mod 23  │  ✅ Done           │  Support — Raj                               │
│  Mod 24  │  ✅ Done           │  Petty Cash — Vipul                          │
│  Mod 25  │  ✅ Done           │  HRM — Uday                                  │
│  Mod 26  │  ✅ Done           │  Performance — Uday                          │
│  Mod 27  │  ✅ Done           │  User Profile — Uday                         │
│  Mod 28  │  ✅ Done           │  Invoice — Raj                               │
│  Mod 29  │  ✅ Done           │  Bills — Vipul                               │
│  Mod 30  │  ✅ Done           │  Payments — Raj                              │
│  Mod 31  │  ✅ Done           │  Ledger — Raj                                │
│  Mod 32  │  ✅ Done           │  COA — Raj                                   │
├──────────┼────────────────────┼──────────────────────────────────────────────┤
│  TOTAL   │  32/32 ✅ Done     │  All modules completed                       │
└──────────┴────────────────────┴──────────────────────────────────────────────┘
```

---

## Dev Workload Summary (Modules Count)

| Developer | Modules Owned | Total | Track |
| :--- | :--- | :--- | :--- |
| **Araman** | 1, 2, 3, 4, 7, 8, 11, 13, 14, 15, 20 | 11 | 🔵 Fix (1–20) |
| **Ansh** | 5, 6, 9, 10, 12, 16, 17, 18, 19 | 9 | 🔵 Fix (1–20) |
| **Raj** | 21, 22, 23, 28, 30, 31, 32 | 7 (21 done; 22, 23 almost done) | 🟠 Build (21–32) |
| **Vipul** | 24, 29 | 2 | 🟠 Build (21–32) |
| **Uday** | 25, 26, 27 | 3 | 🟠 Build (21–32) |

---

## If Nikhil Joins (Reassignment Plan)

> When Nikhil becomes available, immediately reassign the following from Uday:

| Give to Nikhil | Take from Uday | Reason |
| :--- | :--- | :--- |
| **Mod 22** (Location) | UI Only — self-contained | Frees Uday to focus on Mod 25 integration + Mod 26 |
| **Mod 23** (Support) | UI Only — self-contained | SLA tickets, independent module |
| **Mod 27** (User Profile) | Low complexity | Quick build, gives Nikhil a warm-up module |

This returns Uday's load to **Mod 25, 26 only** (the original plan), keeping the sprint balanced.
