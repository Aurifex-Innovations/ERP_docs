# Seravion ERP V2 — Unified Plan  
**Super Admin onboarding · GMA & Contract decoupling · Multi-GST · Module tasks (1–32)**

> **Audience:** Product managers, Seravion ops, dev leads  
> **Product rules:** GMA is **always sold** with Contract. Contract may be created **from GMA** or **direct from Customer** — that is a **workflow default**, not a module on/off switch.  
> **Aligns with:** `SERAVION_ERP_V2_SCOPE_MODULES_1-32.md` v2.3

---

## 1. Executive summary

| Theme | Problem today | Target |
|-------|---------------|--------|
| **Onboarding** | Plans = price only; approval doesn’t persist trial limits; no tenant settings; CEO sees full ERP | Plan = **modules + limits + workflow default** → approval → **tenant settings** → CEO lands in **entitled** product |
| **GMA & Contract** | Contract **requires** GMA; Quote can’t become Contract; SOs stay DRAFT until manual release | **Multiple valid sales paths**; GMA stays; **auto-open SO** on service date |
| **Multi-GST** | One GST mindset; site/branch GST incomplete | **Tenant → Branch → Customer site** GST chain through Contract → SO → Invoice |
| **Module access** | RBAC shows all modules | Sidebar, roles, APIs gated by **plan module pack** |

**Before:** Seravion sells plans but the product doesn’t fully reflect them. Sales is locked to GMA → Contract. Multi-branch / multi-state GST is fragile.

**After:** Seravion configures **what the client bought and how they sell**; CEO onboards with clear status; **correct GST on every invoice**; ops aren’t blocked by manual SO release.

---

## 2. End-to-end journey (big picture)

```text
SERAVION (Super Admin)                    COMPANY (CEO / Admin)
──────────────────────                    ─────────────────────
A. Platform setup                         B. Signup & KYC
   • Plans (modules + limits + workflow)      • Register → docs → status stepper
   • Role templates (plan-scoped)             • See verification status
                                             C. Plan & pay (module preview)
C. Review & configure                     D. First admin setup
   • Approve / trial / reject                 • Sidebar = plan modules only
   • Persist trial limits                     • Branch (limit enforced)
   • Tenant settings (handoff)                • Roles (plan-scoped matrix)
   • Workflow default (GMA vs direct)         • Employees (limit enforced)
E. Ongoing oversight                          E. Day-one sales (both paths available)
   • Usage vs limits                          • GMA → Contract OR Customer → Contract
   • Plan change / support context
```

### Architecture — how config flows to modules

```text
                    ┌─────────────────────┐
                    │  Module 3 Super Admin│
                    │  Approve + Settings  │
                    └──────────┬──────────┘
                               │
         ┌─────────────────────┼─────────────────────┐
         ▼                     ▼                     ▼
  ┌─────────────┐      ┌──────────────┐      ┌──────────────┐
  │ Module 4    │      │ Workflow     │      │ Regional /   │
  │ Plan pack   │      │ Profile      │      │ Multi-GST    │
  │ + limits    │      │ (default     │      │ (tenant +    │
  │             │      │  sales path) │      │  branch)     │
  └──────┬──────┘      └──────┬───────┘      └──────┬───────┘
         │                    │                     │
         ▼                    ▼                     ▼
  Sidebar + RBAC         Modules 15–20          Modules 7, 9,
  (Mod 5)                sales paths            18, 28–29
         │                    │                     │
         └────────────────────┴─────────────────────┘
                               ▼
                    Contract → SO → Task → Invoice
                    (correct GST per site/branch)
```

---

## 3. Cross-cutting themes (not one module)

### 3.1 Tenant Workflow Profile — GMA & Contract decoupling

**GMA (Module 17)** = margin worksheet before contract. **Contract (Module 19)** = commercial agreement; activate → draft SOs.

| Product rule | Detail |
|--------------|--------|
| GMA in product | **Always included** in sales plans — no “disable GMA” toggle |
| Contract paths | **Path A:** GMA → Contract (pest-control default) · **Path B:** Customer → Contract (direct) · **Path C (Phase 2):** Quote → Contract |
| Where configured | **Module 4** plan default + **Module 3** tenant settings override |

#### Tight couplings to audit and loosen

| ID | Coupling today | Loosen to | Modules | Priority |
|----|----------------|-----------|---------|----------|
| L-01 | Contract always needs GMA | Direct contract when workflow allows | 17, 19 | **P0** |
| L-02 | Quote cannot become Contract | Convert API + UI | 16, 19 | **P0** |
| L-03 | GMA dropdown branch-scoped; contract eligibility tenant-wide | **One scope rule** everywhere | 7, 17, 19 | **P0** |
| L-04 | Eligibility API error → Create enabled | Fail closed | 19 | **P0** |
| L-05 | Multi-GMA activate may not consume all sheets | Fix activate consumption | 19 | **P0** |
| L-11 | Import tool ≠ UI GMA rules | Align import | 17, 19 | P1 |
| L-13 | GMA auto-approve 40% hardcoded | Tenant-configurable threshold | 17 | P1 |
| L-14 | Contract create needs GMA_READ | Contract permission enough for direct path | 5, 17, 19 | P1 |
| L-20 | Contract SOs stay DRAFT until manual Release | **Auto-open on `soDate`** | 19, 20, 21 | **P0** |

#### Valid sales paths after V2

```text
Path 1 (default):  15 Lead → 16 Quote → 17 GMA → 19 Contract → 20 SO → 21 Task → 28 Invoice
Path 2:            15 Lead → 16 Quote → 19 Contract → 20 SO → …        (skip GMA)
Path 3:            18 Customer → 19 Contract → 20 SO → …                 (direct)
Path 4:            16 Quote or 17 GMA → 20 SO → 28 Invoice               (one-time, no contract)
Path 5:            18 Customer → 20 Product SO → 28 Invoice
```

---

### 3.2 Multi-GST — tenant, branch, customer site (critical ERP flow)

India AMC/field service needs **different GSTINs** at company, branch, and site level. Wrong GST = compliance failure.

| Level | What to store | Module |
|-------|---------------|--------|
| **Tenant / company** | Legal name, default GSTIN, tax regime | 2, 3, 27 |
| **Branch** | Branch GSTIN, state, place of supply | **7** |
| **Customer site** | Billing GSTIN, state, SEZ/export flags | **18** |
| **Tax master** | GST types, HSN/SAC, rates | **9** |
| **Transaction** | SO/Invoice picks tax from site + branch rules (IGST vs CGST/SGST) | **20, 28** |
| **Vendor** | Vendor GSTIN validation | **13, 29** |

#### Critical flows that must support multi-GST

| Flow | Gap | Target modules | Priority |
|------|-----|----------------|----------|
| CEO sets company GST at onboarding | Weak company-level default | 2, 27 | P1 |
| Branch create with GSTIN + state | Cap enforced; GST not always mandatory | **7** | **P0** |
| Customer site billing GST per site | Incomplete multi-GST | **18** | **P0** |
| Contract inherits site tax context | Per-site commercial lines | **19** | P0 |
| Invoice: IGST when branch ≠ site state | IGST gaps | **28** | P0 |
| Vendor bill tax matches vendor state | Light validation | **13, 29** | P1 |
| GST summary by branch/GSTIN | Scattered exports | **28, 31** | P2 |

---

### 3.3 Module packs & limits (onboarding backbone)

| Control | Set by | Enforced in |
|---------|--------|-------------|
| Module pack | Module 4 plan | Sidebar, API 403, Module 5 role matrix |
| Branch limit | Module 4 plan | Module 7 create |
| User / technician limit | Module 4 plan | Module 8 create |
| Workflow default | Module 4 + Module 3 tenant settings | Modules 15–20 CTAs |
| Trial limits | Module 3 approval | Modules 7, 8 |

---

## 4. Onboarding stages — features, flow changes, deliverables

### Stage A — Seravion platform setup (before any client)

#### A1 · Module 4 — Subscription plans

| | Today | Target |
|---|--------|--------|
| Purpose | Name, price, caps | + **module pack** + **workflow default** |
| Gap | CEO pays but system doesn’t know modules | Plan drives sidebar and APIs |

| Task ID | Deliverable | Mod |
|---------|-------------|-----|
| T-04-01 | Plan create/edit: **checklist of modules 1–32** (by pack) | 4 |
| T-04-02 | Plan shows limits: branches, users, technicians | 4 |
| T-04-03 | Plan: **default sales workflow** (GMA-first / direct / user chooses) | 4 |
| T-04-04 | CEO plan picker: *“Includes: Leads, Quotation, GMA, Contract…”* | 4 |
| T-04-05 | Backend: `subscription_plan_modules` table + APIs | 4 |

#### A2 · Module 5 — Platform role templates

| Task ID | Deliverable | Mod |
|---------|-------------|-----|
| T-05-01 | Role template matrix **filtered by plan modules** (grey/hide unsubscribed) | 5 |
| T-05-02 | Seravion can edit/delete platform roles without errors | 5 |
| T-05-03 | Standard templates: CEO, Branch Manager, Sales, Technician, Accountant | 5 |

---

### Stage B — Company signup (CEO)

#### B1 · Module 2 — Registration & company profile

| Task ID | Deliverable | Mod |
|---------|-------------|-----|
| T-02-01 | Onboarding **stepper**: Pending docs / Under review / Approved / Select plan / Active | 2 |
| T-02-02 | Clear “what to do next” on each screen | 2 |
| T-02-03 | After submit: *“Seravion is reviewing your application”* | 2 |
| T-02-04 | Company profile: **default company GSTIN** (tenant level) | 2, 27 |

#### B2 · Module 2 — Document upload & KYC

| Task ID | Deliverable | Mod |
|---------|-------------|-----|
| T-02-05 | CEO sees doc status: uploaded / under review / verified / rejected + reason | 2 |
| T-02-06 | Seravion can request re-upload from company detail (not email-only) | 2, 3 |

#### B3 · Module 2 + 4 — Subscription & payment

| Task ID | Deliverable | Mod |
|---------|-------------|-----|
| T-02-07 | Plan cards: price, limits, **included modules** before pay | 2, 4 |
| T-02-08 | Payment success **activates plan modules** for tenant | 2, 4 |
| T-02-09 | Success → dashboard with **entitled modules only** in sidebar | 2, 5 |

**Trial path:** Seravion approves with trial → limits **persisted and enforced** (T-03-03).

---

### Stage C — Seravion review & approval (gate)

#### C1 · Module 3 — Company management

| Task ID | Deliverable | Mod |
|---------|-------------|-----|
| T-03-01 | Company detail: status, docs, plan, trial dates | 3 |
| T-03-02 | Actions: Approve, Reject (reason), Enable trial | 3 |
| T-03-03 | **Persist trial limits** (branch/user caps) on approve+trial | 3 |
| T-03-04 | Reliable tenant provision (schema + CEO user) on approve | 3 |
| T-03-05 | Optional **assign plan at approval** vs “CEO will subscribe” | 3 |
| T-03-06 | Audit trail: who approved, when, which plan | 3 |

#### C2 · Module 3 — Tenant settings (new — central handoff)

**Route:** `/seravionadmin/company-management/settings?companyId={id}`

| Panel | Deliverable | Task ID |
|-------|-------------|---------|
| **Subscription** | Active plan, dates, usage vs limits | T-03-07 |
| **Module pack** | Effective modules = plan ± overrides | T-03-08 |
| **Workflow** | Default contract entry: GMA / customer / user chooses — **GMA always on** | T-03-09 |
| **Regional** | Country, tax regime (Week 2+ for full US) | T-03-10 |
| **Handoff link** | After approve → “Configure tenant” → settings | T-03-11 |

---

### Stage D — CEO becomes Admin (first login)

#### D1 · Modules 1, 5 — Login & dashboard

| Task ID | Deliverable | Mod |
|---------|-------------|-----|
| T-01-01 | Sidebar + routes match **plan module pack** | 1, 5 |
| T-01-02 | Upgrade banner on locked module | 5 |
| T-01-03 | Welcome checklist: branch → roles → employees | 2 |
| T-01-04 | Employee-ID login (no Account ID) — Phase 1b | 1, 8 |

#### D2 · Module 7 — First branch (+ multi-GST)

| Task ID | Deliverable | Mod |
|---------|-------------|-----|
| T-07-01 | Block branch create over plan cap + upgrade message | 7 |
| T-07-02 | Usage counter visible to CEO and Seravion | 7 |
| T-07-03 | Branch form: **GSTIN, state** mandatory where applicable | 7 |
| T-07-04 | Branch scope aligned with GMA/contract eligibility (L-03) | 7, 17, 19 |

#### D3 · Modules 5, 8 — Roles & employees

| Task ID | Deliverable | Mod |
|---------|-------------|-----|
| T-08-01 | Block employee create over user limit | 8 |
| T-08-02 | Role editor shows **only plan modules** | 5 |
| T-08-03 | Clone from Seravion platform template | 5, 8 |
| T-08-04 | Populate `iam_login_index` on employee create | 8 |

#### D4 · Modules 17–19 — Day-one sales (light touch)

| Task ID | Deliverable | Mod |
|---------|-------------|-----|
| T-19-01 | Create Contract: **From GMA** \| **From Customer** | 19 |
| T-19-02 | Default entry from tenant workflow settings | 3, 19 |
| T-18-01 | Customer screen: “Create Contract” when direct mode | 18, 19 |
| T-17-01 | GMA remains in nav for all sales plans (no disable) | 17 |

---

### Stage E — Ongoing Seravion oversight

| Task ID | Deliverable | Mod | Phase |
|---------|-------------|-----|-------|
| T-03-12 | Usage monitoring on tenant settings | 3 | 1 |
| T-03-13 | Plan upgrade/downgrade updates modules | 3, 4 | 2 |
| T-03-14 | Support context: plan, workflow, modules on company detail | 3 | 1 |
| T-03-15 | Platform analytics dashboard (replace mock) | 3 | 2 |
| T-03-16 | Linked systems: Razorpay, SMTP, e-invoice | 3 | 3 |

---

## 5. Master task register — all modules (1–32)

**Priority:** P0 = Phase 1 / Week 1 core · P1 = Phase 2 · P2 = Phase 3  
**Onboarding** tasks above are prefixed T-0x; below adds **module-specific V2** tasks.

---

### Layer 1 — Platform & Auth (Modules 1–4)

| Mod | Name | Key gaps | Tasks | Pri |
|-----|------|----------|-------|-----|
| **1** | Authentication | IAM needs Account ID | T-01-04 Employee-ID login API + UI; T-01-05 JWT permissions filtered by plan | P0 |
| **2** | User Onboarding | Unclear status; trial limits | T-02-01–09 (Stage B); T-02-04 company GST | P0 |
| **3** | Super Admin | No tenant settings; limits; reports stub | T-03-01–16 (Stage C/E); tenant settings screen | P0 |
| **4** | Subscription Plans | No module pack | T-04-01–05 (Stage A) | P0 |

---

### Layer 2 — Configuration (Modules 5–9)

| Mod | Name | Key gaps | Tasks | Pri |
|-----|------|----------|-------|-----|
| **5** | Role Management | Matrix shows all modules; EXPORT gaps | T-05-01–03; T-05-04 Remove DOWNLOAD from rbac.js; T-05-05 EXPORT only for subscribed modules (L-18) | P0 |
| **6** | Salary & Leave | OK; tied to hiring | T-06-01 Ensure limits align with Mod 8 hiring flow | P1 |
| **7** | Branch | No cap enforcement; **weak branch GST** | T-07-01–04; multi-GST foundation | **P0** |
| **8** | Employee | User cap not enforced | T-08-01–04; T-08-05 Document expiry alerts | P0 |
| **9** | Tax / HSN | India-only; HSN not gated on invoice | T-09-01 Tenant `tax_regime` at approval; T-09-02 Mandatory HSN before invoice save; T-09-03 IGST rule engine (branch vs site state) | P0/P1 |

---

### Layer 3 — Masters (Modules 10–14)

| Mod | Name | Key gaps | Tasks | Pri |
|-----|------|----------|-------|-----|
| **10** | Products | Grouping UX incomplete | T-10-01 Product grouping UI; variant ↔ stock | P1 |
| **11** | Stock | FE/BE filter gaps | T-11-01 Close `MODULE11` gaps; T-11-02 Task material → stock posting | P1 |
| **12** | Services | Pricing not shared across quote/SO/invoice | T-12-01 Shared pricing engine for 16, 17, 20, 28 | P1 |
| **13** | Vendor | GST validation light | T-13-01 Vendor GSTIN validation; T-13-02 Export RBAC gate | P1 |
| **14** | Purchase Order | Approval notifications missing | T-14-01 PO approve notifications (events platform) | P1 |

---

### Layer 4 — Sales pipeline (Modules 15–20) — decoupling hotspot

| Mod | Name | Key gaps | Tasks | Pri |
|-----|------|----------|-------|-----|
| **15** | Leads | Next step hardcoded to full pipeline | T-15-01 Next step from workflow default | P1 |
| **16** | Quotation | No convert to contract | T-16-01 Quote → Contract API + UI (L-02); T-16-02 Send quote email | P0/P1 |
| **17** | GMA | Mandatory for contract; 40% threshold; scope | T-17-01 GMA always in product; T-17-02 Optional path to contract only; T-17-03 Configurable auto-approve %; T-17-04 Branch scope alignment; T-17-05 Export RBAC | P0/P1 |
| **18** | Customer | **Multi-GST sites incomplete** | T-18-01 **Per-site billing GSTIN**; T-18-02 Create Contract CTA; T-18-03 Multi-GMA display on contract tab | **P0** |
| **19** | Contract | GMA-only; manual SO release; eligibility bugs | T-19-01–02; T-19-03 Direct contract (nullable gma); T-19-04 Quotation source; T-19-05 Multi-GMA consume fix; T-19-06 Fail-closed eligibility; T-19-07 **SO auto-open on `soDate`** (L-20); T-19-08 Remove legacy mock routes | **P0** |
| **20** | Sales Order | Manual release; source UX | T-20-01 Auto-open scheduler + immediate OPEN if date ≤ today; T-20-02 Wizard by SO type; T-20-03 Branch-scoped dropdowns consistent | **P0** |

---

### Layer 5 — Operations & HR (Modules 21–27)

| Mod | Name | Key gaps | Tasks | Pri |
|-----|------|----------|-------|-----|
| **21** | Task | Blocked until SO OPEN; stock posting | T-21-01 Depends on T-19-07 auto-open; T-21-02 Material → stock; T-21-03 Service report PDF + send; T-21-04 Assign notifications | P1 |
| **22** | Live Location | Integration depth | T-22-01 Stable map + task correlation | P1 |
| **23** | Support | SLA notifications | T-23-01 SLA breach notifications | P1 |
| **24** | Petty Cash | Not in sidebar | T-24-01 Add to nav; approval alerts | P1 |
| **25** | HRM | Attendance vs tasks | T-25-01 Task → attendance link; payroll posting | P2 |
| **26** | Technician KPI | Dashboard partial | T-26-01 Wire to real task/GPS data | P2 |
| **27** | User Profile | Company tab weak | T-27-01 Company GST defaults; sync with Mod 2 docs | P1 |

---

### Layer 6 — Finance (Modules 28–32)

| Mod | Name | Key gaps | Tasks | Pri |
|-----|------|----------|-------|-----|
| **28** | Invoicing | Broken routes; **IGST gaps**; no e-invoice | T-28-01 Fix navigation (L-10); T-28-02 **IGST from multi-GST** (site vs branch); T-28-03 Block invoice if site GST missing; T-28-04 Export RBAC; T-28-05 E-invoice IRN (Phase 3) | **P0**/P2 |
| **29** | Bills | Ledger drill-down | T-29-01 Bill transaction API; vendor GST on bills | P1 |
| **30** | Payments | Allocation N+1 | T-30-01 Batch allocation APIs; voucher PDF | P1 |
| **31** | Ledger | Missing voucherId | T-31-01 voucherId on ledger rows; GST summary by branch | P1 |
| **32** | COA | No onboarding wizard | T-32-01 COA wizard post-subscription; seed by country | P1 |

---

## 6. Week 1 sprint pack (onboarding + critical ERP)

Focused on **config handoff + CEO day one + sales unblock + GST minimum**.

| # | Task IDs | Deliverable | Modules |
|---|----------|-------------|---------|
| 1 | T-04-01–05 | Plan module checklist + limits + workflow default | 4 |
| 2 | T-03-03 | Persist trial limits on approval | 3 |
| 3 | T-03-07–11 | Tenant settings screen + handoff link | 3 |
| 4 | T-02-01–03 | CEO onboarding status stepper | 2 |
| 5 | T-02-07–09 | Plan shows modules before payment | 2, 4 |
| 6 | T-01-01–02 | Sidebar gated by plan | 1, 5 |
| 7 | T-07-01–03, T-08-01 | Enforce branch/user limits | 7, 8 |
| 8 | T-05-01, T-05-05 | Role matrix scoped to plan | 5 |
| 9 | T-19-01–03, T-18-01 | Contract: GMA path + direct path | 17–19 |
| 10 | T-07-03, T-18-01 | **Branch GST + site GST** (minimum multi-GST) | 7, 18 |
| 11 | T-19-07, T-20-01 | **SO auto-open on service date** | 19–21 |
| 12 | Ops doc | Written onboarding checklist for Seravion | — |

**Week 1 out of scope:** Platform analytics, full US regional, employee-ID login (can be Week 2), notifications, e-invoice, CEO reports hub.

---

## 7. Rollout waves (after Week 1)

| Wave | Focus | Main modules |
|------|-------|--------------|
| **Wave 1 — Foundation** | Onboarding, packs, limits, decoupling, multi-GST min, SO auto-open | 2–5, 7–9, 17–20, 28 (nav) |
| **Wave 2 — Sales & ops** | Quote→contract, notifications, stock/task, HSN gates, finance parity | 11, 12, 14–16, 21–24, 28–31 |
| **Wave 3 — Compliance & scale** | E-invoice, regional US, CEO reports, KPIs, linked systems | 1, 3, 9, 25–26, 28–32 |

---

## 8. Loophole register (quick reference)

| ID | Issue | Modules | Fix task |
|----|-------|---------|----------|
| L-01 | Contract requires GMA | 17, 19 | T-19-03 |
| L-02 | No Quote→Contract | 16, 19 | T-16-01 |
| L-03 | Branch vs tenant scope | 7, 17, 19 | T-07-04 |
| L-06 | No module toggles | 3, 5 | T-04-01, T-03-08 |
| L-07 | Limits not enforced | 3, 4, 7, 8 | T-07-01, T-08-01 |
| L-18 | EXPORT for unsubscribed modules | 4, 5 | T-05-05 |
| L-19 | IAM needs Account ID | 1, 8 | T-01-04 |
| L-20 | Manual SO release | 19, 20, 21 | T-19-07, T-20-01 |

---

## 9. QA scenarios (manager sign-off)

| # | Scenario | Proves |
|---|----------|--------|
| 1 | Seravion creates “CRM Pro” plan with modules 15,16,17,18,19,20 → CEO pays → sidebar shows only those | Module pack |
| 2 | Trial approve: 2 branches, 5 users → CEO blocked on 3rd branch / 6th user | Limits |
| 3 | Tenant settings: workflow = “direct contract” → Create Contract from Customer works without GMA | Decoupling |
| 4 | Tenant settings: workflow = “GMA first” → Contract from approved GMA still works | GMA path preserved |
| 5 | Contract activate → SO with past `soDate` is OPEN; future SO opens on date without manual Release | L-20 |
| 6 | Customer: Site A (MH GSTIN) + Site B (KA GSTIN) → invoices show correct IGST/CGST-SGST | Multi-GST |
| 7 | Branch GSTIN on invoice header matches issuing branch | Module 7 + 28 |

---

## 10. One-page leadership brief

Seravion will sell **plans that match the product**: modules, limits, and default sales workflow. After approval, **tenant settings** is the single place to configure each company. CEOs onboard with **clear status**, pay for a **visible package**, and land in a dashboard that **matches what they bought**. **GMA stays** in every sales plan; **Contract** can also start from **Customer** when configured. **Multi-GST** at branch and customer site is treated as **critical**, not optional, for Indian multi-location clients. **Contract activation** must **auto-open sales orders** so **tasks** can run on schedule without ops bottlenecks.

---
