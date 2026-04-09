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
| **Nikhil** | 3 yr | **Joins Sunday (Apr 12)** — 4 hrs | Not available Day 1–2 | 🟠 Track 2 (Build 21–32) |

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

## 🟠 TRACK 2 — Build Module 21–32 from Scratch (Raj, Vipul, Uday, Nikhil)

### Module Complexity & Assignment Logic

| Module | Complexity | Screens | Backend Ready? | Assigned To | Reason |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 21 (Task Mgmt) | 🔴 Very High | ~12 | ✅ | **Raj** | Calendar + Assignment — needs full-time bandwidth |
| 22 (Location) | 🟡 Medium | ~4 | ❌ | **Nikhil** | UI only. Maps + tracking. Nikhil joins late, this is self-contained |
| 23 (Support) | 🟡 Medium | ~6 | ❌ | **Nikhil** | UI only. SLA tickets. Self-contained |
| 24 (Petty Cash) | 🟡 Medium | ~6 | ✅ | **Vipul** | Already started. Finish + integrate |
| 25 (HRM) | 🔴 Very High | ~15 | ✅ | **Uday** | Attendance + Leave + Payroll — 3 sub-modules |
| 26 (Performance) | 🟠 High | ~6 | ✅ | **Uday** | Dashboards. Reads from Mod 21/25 |
| 27 (User Profile) | 🟢 Low | ~4 | ✅ | **Nikhil** | Simple self-service. Quick build |
| 28 (Invoice) | 🔴 Very High | ~8 | ✅ | **Raj** | Complex forms + PDF + Credit Notes |
| 29 (Bills) | 🟠 High | ~7 | ✅ | **Vipul** | Mirror of 28 (purchase side). Senior dev can reuse patterns |
| 30 (Payments) | 🔴 Very High | ~8 | ✅ | **Raj** | Receipts + Vouchers + Allocation — most complex finance module |
| 31 (Ledger) | 🟡 Medium | ~4 | ✅ | **Vipul** | List + detail + filter views |
| 32 (COA) | 🟡 Medium | ~4 | ✅ | **Vipul** | Tree/list structure. Config module |

---

### 🟠 Track 2 — Day-Wise Plan

```
┌──────────────────────────────────────────────────────────────────────────────────────────────┐
│  TRACK 2: BUILD MODULE 21–32 (New UI + Integration)                                          │
├──────────┬──────────────────┬──────────────────┬──────────────────┬──────────────────────────┤
│  Day     │  RAJ (8hrs)      │  VIPUL (4hrs)    │  UDAY (4hrs)     │  NIKHIL (4hrs)           │
│          │  Full-time       │  Senior (8yr)    │  3yr exp         │  Joins Day 3             │
├──────────┼──────────────────┼──────────────────┼──────────────────┼──────────────────────────┤
│          │                  │                  │                  │                          │
│  Day 1   │  Mod 21 — UI     │  Mod 24 — UI     │  Mod 25 — UI     │                          │
│  Apr 10  │  (Task Calendar, │  (Finish + Start │  (Attendance     │      ❌ Not Available    │
│  Fri     │   Assignment,    │   Integration)   │   sub-module)    │                          │
│          │   Dispatch views)│                  │                  │                          │
│          │                  │                  │                  │                          │
├──────────┼──────────────────┼──────────────────┼──────────────────┼──────────────────────────┤
│          │                  │                  │                  │                          │
│  Day 2   │  Mod 21 — UI     │  Mod 24 — Integ  │  Mod 25 — UI     │                          │
│  Apr 11  │  (Task Detail,   │  + Mod 32 — UI   │  (Leave Apply/   │      ❌ Not Available    │
│  Sat ⬆️  │   Reschedule,   │  (COA — config   │   Approve sub-   │                          │
│          │   Re-assign) +   │   tree/list)     │   module)        │                          │
│          │  Integration     │                  │  ⬆️ extra hrs    │                          │
│          │  start           │                  │                  │                          │
│          │                  │                  │                  │                          │
├──────────┼──────────────────┼──────────────────┼──────────────────┼──────────────────────────┤
│          │                  │                  │                  │                          │
│  Day 3   │  Mod 28 — UI     │  Mod 32 — Integ  │  Mod 25 — UI     │  Mod 27 — UI + Integ    │
│  Apr 12  │  (Invoice Create,│  + Mod 31 — UI   │  (Payroll sub-   │  (User Profile — quick   │
│  Sun ⬆️  │   List, Detail,  │  (Ledger list,   │   module) +      │   build, self-service    │
│          │   Credit Note)   │   detail, filter)│  Integration     │   views)                 │
│          │                  │                  │  ⬆️ extra hrs    │                          │
│          │                  │                  │                  │                          │
├──────────┼──────────────────┼──────────────────┼──────────────────┼──────────────────────────┤
│          │                  │                  │                  │                          │
│  Day 4   │  Mod 28 — Integ  │  Mod 31 — Integ  │  Mod 26 — UI     │  Mod 22 — UI Only       │
│  Apr 13  │  + Mod 30 — UI   │  + Mod 29 — UI   │  (Technician     │  (Location maps,         │
│  Mon     │  (Payments —     │  (Bills — mirror  │   Performance    │   travel tracking,       │
│          │   Receipt,       │   of Invoice      │   dashboards)    │   geo-fence views)       │
│          │   Voucher forms) │   structure)     │                  │  ⚠️ No backend — UI only │
│          │                  │                  │                  │                          │
├──────────┼──────────────────┼──────────────────┼──────────────────┼──────────────────────────┤
│          │                  │                  │                  │                          │
│  Day 5   │  Mod 30 — UI     │  Mod 29 — Integ  │  Mod 26 — Integ  │  Mod 23 — UI Only       │
│  Apr 14  │  continued +     │  + Final Polish   │  + Help with     │  (Support tickets,       │
│  Tue     │  Integration     │  + Review Mod     │  Track 1 Integ   │   SLA dashboard)         │
│          │  (Allocation     │  24, 31, 32      │  if needed       │  ⚠️ No backend — UI only │
│          │   logic, advance,│                  │                  │                          │
│          │   shortfall)     │                  │                  │                          │
│          │                  │                  │                  │                          │
│          │  ⚠️ Mod 30 is   │                  │                  │                          │
│          │  the hardest —   │                  │                  │                          │
│          │  may need Day 5  │                  │                  │                          │
│          │  evening buffer  │                  │                  │                          │
└──────────┴──────────────────┴──────────────────┴──────────────────┴──────────────────────────┘

⬆️ = Extra hours available (weekend)
⚠️ = Risk flag
```

---

## Combined 5-Day Sprint View (All 6 Devs)

```
═══════════════════════════════════════════════════════════════════════════════════════
 DAY 1 — Friday, Apr 10
═══════════════════════════════════════════════════════════════════════════════════════

 🔵 Araman    → Mod 1, 2 UI Fix
 🔵 Ansh      → Mod 5, 6 UI Fix
 🟠 Raj       → Mod 21 UI (Task Calendar, Assignment)
 🟠 Vipul     → Mod 24 UI Finish + Integration Start
 🟠 Uday      → Mod 25 UI (Attendance sub-module)
 ⬛ Nikhil    → ❌ Not available

═══════════════════════════════════════════════════════════════════════════════════════
 DAY 2 — Saturday, Apr 11  ⬆️ Weekend
═══════════════════════════════════════════════════════════════════════════════════════

 🔵 Araman ⬆️ → Mod 3, 4, 7 UI Fix + Integration (1, 2)
 🔵 Ansh      → Mod 9, 10 UI Fix + Integration (5, 6)
 🟠 Raj       → Mod 21 UI Finish + Integration Start
 🟠 Vipul     → Mod 24 Integration + Mod 32 UI (COA)
 🟠 Uday  ⬆️ → Mod 25 UI (Leave sub-module)
 ⬛ Nikhil    → ❌ Not available

═══════════════════════════════════════════════════════════════════════════════════════
 DAY 3 — Sunday, Apr 12  ⬆️ Weekend  ✅ Nikhil joins
═══════════════════════════════════════════════════════════════════════════════════════

 🔵 Araman ⬆️ → Mod 8 UI Fix (Employee — heavy) + Integration (3, 4, 7)
 🔵 Ansh      → Mod 12, 16 UI Fix + Integration (9, 10)
 🟠 Raj       → Mod 28 UI (Invoice — Create, List, Detail, CN)
 🟠 Vipul     → Mod 32 Integration + Mod 31 UI (Ledger)
 🟠 Uday  ⬆️ → Mod 25 UI Finish (Payroll) + Integration Start
 🟠 Nikhil ✅ → Mod 27 UI + Integration (User Profile — quick build)

═══════════════════════════════════════════════════════════════════════════════════════
 DAY 4 — Monday, Apr 13
═══════════════════════════════════════════════════════════════════════════════════════

 🔵 Araman    → Mod 11, 13 UI Fix + Integration (8)
 🔵 Ansh      → Mod 17, 18 UI Fix + Integration (12, 16)
 🟠 Raj       → Mod 28 Integration + Mod 30 UI Start (Payments)
 🟠 Vipul     → Mod 31 Integration + Mod 29 UI (Bills)
 🟠 Uday      → Mod 26 UI (Technician Performance)
 🟠 Nikhil    → Mod 22 UI Only ⚠️ (Location maps — no backend)

═══════════════════════════════════════════════════════════════════════════════════════
 DAY 5 — Tuesday, Apr 14  🏁 FINAL DAY
═══════════════════════════════════════════════════════════════════════════════════════

 🔵 Araman    → Mod 14, 15, 20 UI Fix + Integration (11, 13)
 🔵 Ansh      → Mod 19 UI Fix + Integration (17, 18, 19)
 🟠 Raj       → Mod 30 UI Finish + Integration (Allocation, Advance, Shortfall)
 🟠 Vipul     → Mod 29 Integration + Final Review (24, 31, 32)
 🟠 Uday      → Mod 26 Integration + Help Track 1 if needed
 🟠 Nikhil    → Mod 23 UI Only ⚠️ (Support tickets — no backend)

═══════════════════════════════════════════════════════════════════════════════════════
```

---

## Risk Assessment

| Risk | Impact | Mitigation |
| :--- | :--- | :--- |
| **Mod 20 (SO) UI Fix may spill** | High — SO has 3 source types, complex forms | Araman prioritizes SO integration over Mod 14 polish |
| **Mod 30 (Payments) may not finish Day 5** | High — most complex finance module | Raj has full-time bandwidth. Vipul (senior) can support if Mod 29 finishes early |
| **Mod 25 (HRM) has 3 sub-modules** | Medium — Attendance + Leave + Payroll is a lot | Uday gets extra weekend hours (Day 2–3). Payroll can be UI-only if tight |
| **Mod 22, 23 cannot integrate** | Medium — backend not ready | Nikhil builds UI only. Integration planned for post-sprint when backend arrives |
| **Mod 21 (Task) is very heavy** | High — calendar, assignment, dispatch, re-task | Full 2 days allocated to Raj (full-time). This is the highest priority module |
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
│  Mod 6   │  ✅ Done           │  Salary Config — Ansh (already started)      │
│  Mod 7   │  ✅ Done           │  Branch — Araman                             │
│  Mod 8   │  ✅ Done           │  Employee — Araman (heavy but weekend day)   │
│  Mod 9   │  ✅ Done           │  Tax — Ansh                                  │
│  Mod 10  │  ✅ Done           │  Products — Ansh                             │
│  Mod 11  │  ✅ Done           │  Stock — Araman                              │
│  Mod 12  │  ✅ Done           │  Services — Ansh                             │
│  Mod 13  │  ✅ Done           │  Vendor — Araman                             │
│  Mod 14  │  🟡 UI Fix only   │  PO — Araman (integration if time permits)   │
│  Mod 15  │  ✅ Done           │  Leads — Araman                              │
│  Mod 16  │  ✅ Done           │  Quotation — Ansh                            │
│  Mod 17  │  ✅ Done           │  GMA — Ansh                                  │
│  Mod 18  │  ✅ Done           │  Customer — Ansh                             │
│  Mod 19  │  ✅ Done           │  Contract — Ansh                             │
│  Mod 20  │  🟡 At Risk       │  SO — Araman (complex, last day)             │
│  Mod 21  │  ✅ Done           │  Task Mgmt — Raj (2 full days)               │
│  Mod 22  │  🟡 UI Only       │  Location — Nikhil (backend NOT ready)       │
│  Mod 23  │  🟡 UI Only       │  Support — Nikhil (backend NOT ready)        │
│  Mod 24  │  ✅ Done           │  Petty Cash — Vipul (already started)        │
│  Mod 25  │  ✅ Done           │  HRM — Uday (3 days incl. weekend)           │
│  Mod 26  │  ✅ Done           │  Performance — Uday                          │
│  Mod 27  │  ✅ Done           │  User Profile — Nikhil                       │
│  Mod 28  │  ✅ Done           │  Invoice — Raj                               │
│  Mod 29  │  ✅ Done           │  Bills — Vipul                               │
│  Mod 30  │  🟡 At Risk       │  Payments — Raj (most complex, last day)     │
│  Mod 31  │  ✅ Done           │  Ledger — Vipul                              │
│  Mod 32  │  ✅ Done           │  COA — Vipul                                 │
├──────────┼────────────────────┼──────────────────────────────────────────────┤
│  TOTAL   │  28/32 ✅ Done     │  2 at risk (20, 30), 2 UI-only (22, 23)     │
└──────────┴────────────────────┴──────────────────────────────────────────────┘
```

---

## Dev Workload Summary (Modules Count)

| Developer | Modules Owned | Total | Track |
| :--- | :--- | :--- | :--- |
| **Araman** | 1, 2, 3, 4, 7, 8, 11, 13, 14, 15, 20 | 11 | 🔵 Fix (1–20) |
| **Ansh** | 5, 6, 9, 10, 12, 16, 17, 18, 19 | 9 | 🔵 Fix (1–20) |
| **Raj** | 21, 28, 30 | 3 (all Very High complexity) | 🟠 Build (21–32) |
| **Vipul** | 24, 29, 31, 32 | 4 | 🟠 Build (21–32) |
| **Uday** | 25, 26 | 2 (Mod 25 is Very High) | 🟠 Build (21–32) |
| **Nikhil** | 22, 23, 27 | 3 (joins Day 3 only) | 🟠 Build (21–32) |
