# Seravion ERP — Module Categorization & Developer Block Assignment

> **Purpose:** Categorize all 32 modules into logical development blocks, map dependencies, and assign to 3–4 developers based on similarity, coupling, and build priority.

---

## Master Module Index (1–32)

| # | Module Name | Type | Domain |
| :--- | :--- | :--- | :--- |
| 1 | Authentication | Platform | Auth & Onboarding |
| 2 | User Onboarding | Platform | Auth & Onboarding |
| 3 | Super Admin (Seravion) | Platform | Seravion Internal |
| 4 | Subscription Plans | Platform | Seravion Internal |
| 5 | Role Management (RBAC) | Config | Org Setup |
| 6 | Role Salary & Leave Config | Config | Org Setup |
| 7 | Branch Management | Config | Org Setup |
| 8 | Employee (Users) Mgmt | Config | Org Setup |
| 9 | Tax Management | Config | Master Data |
| 10 | Inventory & Services (Product Master) | Config | Master Data |
| 11 | Stock Management | Operations | Inventory & Supply |
| 12 | Service Management | Config | Master Data |
| 13 | Vendor Management | Operations | Inventory & Supply |
| 14 | Purchase Order | Operations | Inventory & Supply |
| 15 | Leads & Follow-up | Business | Sales Pipeline |
| 16 | Quotation Management | Business | Sales Pipeline |
| 17 | GMA (Gross Margin Analysis) | Business | Sales Pipeline |
| 18 | Customer Management | Business | Sales Pipeline |
| 19 | Contract Management | Business | Sales Pipeline |
| 20 | Sales Order Management | Business | Sales Pipeline |
| 21 | Task Management | Operations | Field Execution |
| 22 | Live Location & Travel | Operations | Field Execution |
| 23 | Customer Support (SLA) | Operations | Field Execution |
| 24 | Petty Cash | Finance | Expense Mgmt |
| 25 | HRM (Attendance/Leave/Payroll) | HR | People Mgmt |
| 26 | Technician Performance | Analytics | People Mgmt |
| 27 | User Profile (Self-Service) | Utility | People Mgmt |
| 28 | Invoicing (Sales) | Finance | Accounting |
| 29 | Bills (Purchases) | Finance | Accounting |
| 30 | Payments (Receipts & Vouchers) | Finance | Accounting |
| 31 | Ledger Management | Finance | Accounting |
| 32 | Chart of Accounts (COA) | Finance | Accounting |

---

## Dependency Graph (ASCII)

```
                            ┌──────────────────────────────────────────────────────────────┐
 LAYER 0 (PLATFORM)         │  Mod 1 (Auth) → Mod 2 (Onboarding) → Mod 3 (Super Admin)   │
 Seravion-side only          │                                        ↓                     │
                            │                                   Mod 4 (Subscription)       │
                            │                                   Mod 5 (Role Mgmt)          │
                            └─────────────────────────────────────────┬────────────────────┘
                                                                      │
                            ┌─────────────────────────────────────────▼────────────────────┐
 LAYER 1 (ORG SETUP)        │  Mod 5 (RBAC) ──► Mod 6 (Salary/Leave Config)               │
 Client-side config          │       ↓                                                      │
                            │  Mod 7 (Branch) ──► Mod 8 (Employee / Users)                 │
                            │                         ↑ uses Mod 5, 6, 7                     │
                            └─────────────────────────────────────────┬────────────────────┘
                                                                      │
                            ┌─────────────────────────────────────────▼────────────────────┐
 LAYER 2 (MASTER DATA)      │  Mod 9 (Tax) ──► Mod 10 (Product Master)                    │
 Must exist before ops       │       ↓               ↓                                      │
                            │  Mod 12 (Service) ──► Mod 11 (Stock Mgmt)                    │
                            │  Mod 13 (Vendor) ──► Mod 14 (Purchase Order) ──► Mod 11      │
                            └─────────────────────────────────────────┬────────────────────┘
                                                                      │
                            ┌─────────────────────────────────────────▼────────────────────┐
 LAYER 3 (SALES PIPELINE)   │  Mod 15 (Lead) ──► Mod 18 (Customer)                        │
 Revenue flow                │       ↓                  ↓                                    │
                            │  Mod 16 (Quotation) ──► Mod 17 (GMA) ──► Mod 19 (Contract)  │
                            │                                              ↓                 │
                            │                                         Mod 20 (Sales Order)  │
                            └─────────────────────────────────────────┬────────────────────┘
                                                                      │
                            ┌─────────────────────────────────────────▼────────────────────┐
 LAYER 4 (FIELD OPS)        │  Mod 20 ──► Mod 21 (Task) ──► Mobile App (5-Step Exec)      │
 Execution + Mobile          │                  ↓                                            │
                            │             Mod 22 (Location / GPS)                           │
                            │             Mod 23 (Customer Support → Re-Task)               │
                            └─────────────────────────────────────────┬────────────────────┘
                                                                      │
                            ┌─────────────────────────────────────────▼────────────────────┐
 LAYER 5 (FINANCE)           │  Mod 32 (COA) ──► Mod 31 (Ledger)                           │
 Settlement + Accounting     │       ↓               ↑ posts from ↓                         │
                            │  Mod 28 (Invoice) ──► Mod 30 (Payments) ◄── Mod 29 (Bills)  │
                            │  Mod 24 (Petty Cash)                                          │
                            └──────────────────────────────────────────────────────────────┘
                                                                      │
                            ┌─────────────────────────────────────────▼────────────────────┐
 LAYER 6 (HR + ANALYTICS)   │  Mod 25 (HRM — Attendance / Leave / Payroll)                │
 People + Reporting          │  Mod 26 (Technician Performance)                             │
                            │  Mod 27 (User Profile — Self-Service)                        │
                            │  Mod 33 (Reports — P&L, BS, GST, Ageing, Cash Flow)          │
                            └──────────────────────────────────────────────────────────────┘
```

---

## Developer Block Assignment (4 Developers)

### 🔵 BLOCK A — Platform, Auth & Org Foundation

**Owner: Developer 1 (Full-Stack / Backend Lead)**

```
┌─────────────────────────────────────────────────────────────────┐
│  BLOCK A: PLATFORM + ORG SETUP (Modules 1–8)                    │
│                                                                  │
│  ┌─ SERAVION SIDE ─────────────────────────────────────────────┐│
│  │  Mod 1  Authentication (Super Admin + Client login)          ││
│  │  Mod 2  User Onboarding (Doc Upload → Approval)             ││
│  │  Mod 3  Super Admin Dashboard (Tenant Mgmt)                  ││
│  │  Mod 4  Subscription Plans (Plan CRUD)                       ││
│  └──────────────────────────────────────────────────────────────┘│
│                           │                                      │
│                    Flows into                                    │
│                           ▼                                      │
│  ┌─ CLIENT SIDE ──────────────────────────────────────────────┐ │
│  │  Mod 5  Role Management (RBAC — Templates + Client Custom)  ││
│  │  Mod 6  Role Salary & Leave Config                           ││
│  │  Mod 7  Branch Management                                    ││
│  │  Mod 8  Employee / User Management                           ││
│  └──────────────────────────────────────────────────────────────┘│
│                                                                  │
│  TOTAL SCREENS: ~45        PRIORITY: 🔴 P0 — Build FIRST        │
│  DEPENDENCY: None (root of everything)                           │
│  MOBILE: Mod 27 (User Profile) lives here logically             │
│                                                                  │
│  BUILD ORDER:                                                    │
│  1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 27                          │
└─────────────────────────────────────────────────────────────────┘
```

**Why this grouping:**
- These are **zero-dependency foundation** modules
- All downstream modules need Auth, Branch, Employee, and RBAC to function
- Single responsibility: "Identity & Organizational Structure"
- Seravion + Client flows are tightly coupled (signup → approval → onboarding)

| Module | Est. Complexity | Key Dependencies |
| :--- | :--- | :--- |
| Mod 1 (Auth) | Medium | None — root |
| Mod 2 (Onboarding) | Medium | Mod 1 |
| Mod 3 (Super Admin) | High | Mod 2 |
| Mod 4 (Subscription) | Low | Mod 3 |
| Mod 5 (RBAC) | High | Mod 3 (templates) |
| Mod 6 (Salary/Leave Config) | Medium | Mod 5 |
| Mod 7 (Branch) | Low | Mod 2 |
| Mod 8 (Employee) | High | Mod 5, 6, 7 |
| Mod 27 (User Profile) | Medium | Mod 1, 7, 8, 6, 25 |

---

### 🟢 BLOCK B — Sales Pipeline & CRM

**Owner: Developer 2 (Full-Stack / Business Logic)**

```
┌─────────────────────────────────────────────────────────────────┐
│  BLOCK B: SALES PIPELINE + CRM (Modules 9–10, 12, 15–20)       │
│                                                                  │
│  ┌─ MASTER DATA (Config pre-reqs) ─────────────────────────────┐│
│  │  Mod 9   Tax Management (GST, HSN/SAC)                       ││
│  │  Mod 10  Product Master (Items, Chemicals, UoM)              ││
│  │  Mod 12  Service Management (Services, Pricing Models)       ││
│  └──────────────────────────────────────────────────────────────┘│
│                           │                                      │
│                    Feeds into                                    │
│                           ▼                                      │
│  ┌─ SALES PIPELINE (Transaction flow) ─────────────────────────┐│
│  │  Mod 15  Leads & Follow-up                                   ││
│  │  Mod 16  Quotation Management                                ││
│  │  Mod 17  GMA (Gross Margin Analysis)                         ││
│  │  Mod 18  Customer Management (Master, Sites, Contacts)       ││
│  │  Mod 19  Contract Management (AMC / Recurring)               ││
│  │  Mod 20  Sales Order Management                              ││
│  └──────────────────────────────────────────────────────────────┘│
│                                                                  │
│  TOTAL SCREENS: ~60        PRIORITY: 🔴 P0 — Build in PARALLEL  │
│  DEPENDENCY: Needs Block A (Auth, Branch, Employee, RBAC)        │
│                                                                  │
│  BUILD ORDER:                                                    │
│  9 → 10 → 12 (master data first)                                │
│  then: 15 → 18 → 16 → 17 → 19 → 20                            │
└─────────────────────────────────────────────────────────────────┘
```

**Why this grouping:**
- **Mod 9, 10, 12** are master data that feed into everything downstream (invoicing, stock, SO, tasks)
- **Mod 15 → 20** is the entire pre-sales pipeline — one continuous data flow
- All share the same entities: Customer, Service, Product, Price, Tax
- Developer needs to understand the full Lead-to-SO conversion funnel

| Module | Est. Complexity | Key Dependencies |
| :--- | :--- | :--- |
| Mod 9 (Tax) | Medium | Block A |
| Mod 10 (Product Master) | High | Mod 9 |
| Mod 12 (Service Mgmt) | High | Mod 10, 9 |
| Mod 15 (Leads) | Medium | Mod 8 |
| Mod 16 (Quotation) | Medium | Mod 12, 10, 9, 15 |
| Mod 17 (GMA) | High | Mod 10, 12, 18, 8, 9 |
| Mod 18 (Customer) | High | Mod 15, 7, 9 |
| Mod 19 (Contract) | High | Mod 17, 18, 7, 8 |
| Mod 20 (Sales Order) | Very High | Mod 19, 17, 18, 16, 10, 9 |

---

### 🟠 BLOCK C — Field Operations, Mobile & Supply Chain

**Owner: Developer 3 (Full-Stack / Mobile-focused)**

```
┌─────────────────────────────────────────────────────────────────┐
│  BLOCK C: FIELD OPS + MOBILE + INVENTORY (Mod 11, 13, 14,       │
│                                           21, 22, 23 + Mobile)   │
│                                                                  │
│  ┌─ SUPPLY CHAIN (Vendor → Purchase → Stock) ──────────────────┐│
│  │  Mod 13  Vendor Management                                   ││
│  │  Mod 14  Purchase Order (PO)                                  ││
│  │  Mod 11  Stock Management (Central/Virtual Bin, GRN, Transfer)││
│  └──────────────────────────────────────────────────────────────┘│
│                           │                                      │
│                  Stock feeds into                                │
│                           ▼                                      │
│  ┌─ FIELD EXECUTION (Task → Mobile → Reports) ────────────────┐ │
│  │  Mod 21  Task Management (Calendar, Assignment, Dispatch)    ││
│  │  Mod 22  Live Location & Travel Tracking (GPS, Geo-fence)   ││
│  │  Mod 23  Customer Support (SLA Tickets → Re-Task)            ││
│  └──────────────────────────────────────────────────────────────┘│
│                           │                                      │
│                  Drives the                                      │
│                           ▼                                      │
│  ┌─ 📱 MOBILE APP (Technician Screens 1–18) ──────────────────┐ │
│  │  Screen 1–5:   Dashboard, Upcoming/Past Tasks, Notifications ││
│  │  Screen 6:     Attendance — Punch In/Out                     ││
│  │  Screen 7–10:  Leave, Upcoming, Week View, Travel Logs       ││
│  │  Screen 11–18: Task Execution (Start Travel → OTP → Report)  ││
│  └──────────────────────────────────────────────────────────────┘│
│                                                                  │
│  TOTAL SCREENS: ~55 (web) + 18 (mobile)                         │
│  PRIORITY: 🟡 P1 — Starts after Block A (needs Employee)         │
│                    + Block B (needs SO, Product, Customer)        │
│                                                                  │
│  BUILD ORDER:                                                    │
│  13 → 14 → 11 (supply chain)                                    │
│  then: 21 → 22 → 23 (field ops web)                            │
│  then: Mobile App (Screens 1–18)                                │
└─────────────────────────────────────────────────────────────────┘
```

**Why this grouping:**
- **Mod 11 + 13 + 14** form the supply chain (vendor → PO → stock) — tightly coupled
- **Mod 21 + 22 + 23** form the field operations trilogy — tasks drive location and support
- **Mobile App** is the execution arm of Mod 21 + 22 — same developer should own both
- Stock deduction (Mod 11) happens inside mobile task execution — critical coupling

| Module | Est. Complexity | Key Dependencies |
| :--- | :--- | :--- |
| Mod 13 (Vendor) | Medium | Block A |
| Mod 14 (Purchase Order) | High | Mod 13, 10, 9 |
| Mod 11 (Stock) | Very High | Mod 9, 10, 14 |
| Mod 21 (Task Mgmt) | Very High | Mod 20, 19, 18, 17, 11, 8 |
| Mod 22 (Location) | High | Mod 21, 8, 7 |
| Mod 23 (Support) | Medium | Mod 18, 20, 21, 8 |
| Mobile App | Very High | All of Block B + C |

---

### 🔴 BLOCK D — Finance, HR & Analytics

**Owner: Developer 4 (Full-Stack / Finance domain)**

```
┌─────────────────────────────────────────────────────────────────┐
│  BLOCK D: FINANCE + HR + ANALYTICS (Mod 24, 25, 26, 28–32)      │
│                                                                  │
│  ┌─ FINANCE ENGINE (Accounting Stack) ─────────────────────────┐│
│  │  Mod 32  Chart of Accounts (COA) — account head structure    ││
│  │  Mod 31  Ledger Management ("Bahi Khata" central book)       ││
│  │  Mod 28  Invoicing (Sales) — SO-linked, Direct, Credit Notes ││
│  │  Mod 29  Bills (Purchases) — PO-linked, Debit Notes          ││
│  │  Mod 30  Payments (Receipts & Vouchers, Allocation, Advance) ││
│  │  Mod 24  Petty Cash Management                               ││
│  └──────────────────────────────────────────────────────────────┘│
│                           │                                      │
│                    Feeds into                                    │
│                           ▼                                      │
│  ┌─ HR & PEOPLE (Payroll, Attendance, Performance) ────────────┐│
│  │  Mod 25  HRM (Attendance, Leave Apply/Approve, Payroll)      ││
│  │  Mod 26  Technician Performance & Productivity               ││
│  └──────────────────────────────────────────────────────────────┘│
│                           │                                      │
│                    Outputs to                                    │
│                           ▼                                      │
│  ┌─ REPORTING & ANALYTICS ─────────────────────────────────────┐│
│  │  Mod 33  Reports (P&L, Balance Sheet, GST Returns, Ageing,   ││
│  │          Cash Flow, Bank Reconciliation)                      ││
│  └──────────────────────────────────────────────────────────────┘│
│                                                                  │
│  TOTAL SCREENS: ~50        PRIORITY: 🟡 P1 (COA, Ledger) →      │
│                                      🟢 P2 (Invoice, Payment) → │
│                                      🔵 P3 (HRM, Reports)       │
│                                                                  │
│  BUILD ORDER:                                                    │
│  32 → 31 (foundation — COA then Ledger)                         │
│  then: 28 → 29 → 30 (transaction modules)                      │
│  then: 24 (petty cash — standalone)                             │
│  then: 25 → 26 (HR stack)                                       │
│  last: 33 (reports — needs all data)                            │
└─────────────────────────────────────────────────────────────────┘
```

**Why this grouping:**
- **Mod 28 + 29 + 30 + 31 + 32** are the core accounting stack — inseparable
- **Mod 30** (Payments) is the most complex module (receipts + vouchers + shortfall + advance adjustment + CN/DN auto-generation) — same dev who builds invoices must own payments
- **Mod 25** (HRM) touches Mod 6 (salary config) and Mod 21 (attendance via tasks) — but its primary output is payroll, which is financial
- **Mod 26** (Performance) reads from Mod 21, 25, 11 — aggregation module, grouped here for reporting synergy

| Module | Est. Complexity | Key Dependencies |
| :--- | :--- | :--- |
| Mod 32 (COA) | Medium | Mod 2, 7 |
| Mod 31 (Ledger) | High | Mod 18, 13, 32 |
| Mod 28 (Invoice) | Very High | Mod 18, 19, 20, 21, 9, 7 |
| Mod 29 (Bills) | High | Mod 11, 10, 9, 7 |
| Mod 30 (Payments) | Very High | Mod 28, 29, 18, 14, 31, 32 |
| Mod 24 (Petty Cash) | Medium | Mod 8, 7 |
| Mod 25 (HRM) | Very High | Mod 6, 8, 21, 7, 5 |
| Mod 26 (Performance) | High | Mod 21, 20, 25, 11, 8, 7 |
| Mod 33 (Reports) | High | Mod 31, 32, 28, 29, 30, 9 |

---

## Sprint-Level Build Priority (Gantt-style)

```
                   Sprint 1       Sprint 2       Sprint 3       Sprint 4       Sprint 5       Sprint 6
                   (Wk 1-2)      (Wk 3-4)      (Wk 5-6)      (Wk 7-8)      (Wk 9-10)     (Wk 11-12)

 Dev 1 (Block A)   ████████████   ████████████
                    Mod 1,2,3,4    Mod 5,6,7,8                  Mod 27
                    Auth+Onboard   RBAC+Branch                  UserProfile
                                   +Employee

 Dev 2 (Block B)   ████████████   ████████████   ████████████   ████████████
                    Mod 9, 10      Mod 12, 15     Mod 16, 17     Mod 18, 19, 20
                    Tax, Products  Service, Lead  Quote, GMA     Customer→Contract→SO

 Dev 3 (Block C)                  ████████████   ████████████   ████████████   ████████████   ████████████
                                   Mod 13, 14     Mod 11         Mod 21, 22     Mod 23 +       Mobile
                                   Vendor, PO     Stock Mgmt     Task, Location  Support        App Build

 Dev 4 (Block D)                  ████████████   ████████████   ████████████   ████████████   ████████████
                                   Mod 32, 31     Mod 28, 29     Mod 30, 24     Mod 25, 26      Mod 33
                                   COA, Ledger    Invoice, Bill   Payment, Petty  HRM, Perf      Reports
```

### Critical Path

```
Auth (1) → RBAC (5) → Employee (8) → Customer (18) → SO (20) → Task (21) → Invoice (28) → Payment (30)
    │                                                                │
    └── MUST complete before ANY downstream module can start          └── Mobile App depends on this
```

---

## Cross-Block Dependency Map

| From Block | To Block | Critical Handoff Points |
| :--- | :--- | :--- |
| **A → B** | Auth, Branch, Employee must exist for Lead/Customer creation | Mod 8 (Employee) → Mod 15 (Lead Assignment) |
| **A → C** | Employee/Branch needed for technician assignment | Mod 8 → Mod 21 (Task Assignment) |
| **A → D** | Employee needed for HRM, Branch for COA | Mod 7, 8 → Mod 25, 32 |
| **B → C** | SO drives Task generation, Products feed Stock | Mod 20 → Mod 21, Mod 10 → Mod 11 |
| **B → D** | SO and Customer drive Invoice creation | Mod 18, 20 → Mod 28 |
| **C → D** | Task completion triggers Invoice readiness, PO drives Bills | Mod 21 → Mod 28, Mod 14 → Mod 29 |
| **D → B** | Payment settlement updates SO status to "Completed" | Mod 30 → Mod 20 (status update) |

---

## Summary: Quick Reference Card

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                              │
│    🔵 BLOCK A (Dev 1)        🟢 BLOCK B (Dev 2)                             │
│    Platform + Org Setup      Sales Pipeline + CRM                           │
│    Mod 1-8, 27               Mod 9, 10, 12, 15-20                          │
│    9 modules                 9 modules                                      │
│    Priority: P0              Priority: P0                                   │
│                                                                              │
│    🟠 BLOCK C (Dev 3)        🔴 BLOCK D (Dev 4)                             │
│    Field Ops + Mobile        Finance + HR + Analytics                       │
│    Mod 11, 13, 14,           Mod 24, 25, 26,                               │
│    21, 22, 23 + Mobile       28, 29, 30, 31, 32, 33                        │
│    6 modules + Mobile App    9 modules                                      │
│    Priority: P1              Priority: P1–P3                                │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

| Block | Modules | Total | Priority | Starts After |
| :--- | :--- | :--- | :--- | :--- |
| **A** | 1, 2, 3, 4, 5, 6, 7, 8, 27 | 9 | P0 | — (Root) |
| **B** | 9, 10, 12, 15, 16, 17, 18, 19, 20 | 9 | P0 | Block A Sprint 1 |
| **C** | 11, 13, 14, 21, 22, 23 + Mobile | 6 + App | P1 | Block A + partial B |
| **D** | 24, 25, 26, 28, 29, 30, 31, 32, 33 | 9 | P1–P3 | Block A + partial B |

> **If only 3 developers:** Merge Block A + Block D → one senior developer handles Platform Foundation + Finance (builds Auth first, then switches to Finance stack in Sprint 3 when Block A is done). This works because Block D doesn't start until Sprint 2 anyway.
