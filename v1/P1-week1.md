# Super Admin → Company Admin Onboarding  
**Features, flow changes & deliverables** (manager + dev friendly)

End-to-end journey: **Seravion sets up the platform → company signs up → Seravion approves & configures → CEO finishes onboarding → CEO runs the tenant as Admin.**

GMA stays part of the product. Contract can also be created without GMA — that is one workflow option, not the main theme of this doc.

---

## The full journey (big picture)

```text
SERAVION (Super Admin)          COMPANY (CEO / Admin)
─────────────────────          ─────────────────────
1. Create subscription plans   4. Sign up & company profile
2. Create role templates       5. Upload documents
3. Review & approve companies  6. Choose plan & pay (or trial)
   Configure tenant settings   7. Onboarding success → dashboard
4. Monitor usage & limits      8. First setup: branch, roles, team
```

**Today:** Steps 1–3 and 4–7 exist but are **disconnected** — plans don’t define what modules the client gets, approval doesn’t save trial limits properly, there’s no tenant settings screen, and the CEO doesn’t see a clear “what’s included in my plan.”

**Target:** One continuous pipeline where **what Seravion configures** flows directly into **what the CEO sees and can use** on day one.

---

## Stage A — Seravion platform setup (before any client)

### A1. Subscription plans

| | Today | Target |
|---|--------|--------|
| **Purpose** | Name, price, branch/user caps | Same + **what’s included in the software** |
| **Gap** | CEO buys a plan but system doesn’t know which modules they paid for | Plan defines **module pack** (CRM, Operations, Finance, etc.) |
| **Deliverable** | Plan create/edit shows **checklist of modules** included in that plan |
| **Deliverable** | Plan shows **limits**: branches, users, technicians — and system **enforces** them later |
| **Deliverable** | Plan shows **default sales workflow** (e.g. “usually start contract from GMA” vs “from customer”) — both paths still available; GMA always included in sales plans |
| **Deliverable** | CEO plan picker preview: *“Includes: Leads, Quotation, GMA, Contract, Customer, …”* |

**Manager outcome:** Sales sells “CRM Pro + Operations” and the product matches that package automatically.

---

### A2. Role templates (platform roles)

| | Today | Target |
|---|--------|--------|
| **Purpose** | Seravion defines reusable roles (CEO, Branch Manager, etc.) | Same, but **only for modules on the plan** |
| **Gap** | Permission matrix shows all modules; client may get permissions for things they didn’t buy | Matrix **filtered by plan** — no permissions for unsubscribed modules |
| **Deliverable** | Role template editor greys out or hides modules not on selected plan preview |
| **Deliverable** | Seravion can delete/edit platform roles without errors |
| **Deliverable** | Standard templates ready: CEO, Branch Manager, Sales, Technician, Accountant |

**Manager outcome:** New tenant copies a template that matches their subscription — no over-permissioning.

---

## Stage B — Company signup (CEO side)

### B1. Registration & company profile

| | Today | Target |
|---|--------|--------|
| **Flow** | Signup → company info → documents → wait | Same, with **clearer status** at each step |
| **Gap** | User unsure if waiting on docs, approval, or payment | Stepper with badges: *Pending docs / Under review / Approved / Select plan / Active* |
| **Deliverable** | Obvious “what to do next” on each onboarding screen |
| **Deliverable** | After submit, message: *“Seravion is reviewing your application”* |

---

### B2. Document upload & KYC

| | Today | Target |
|---|--------|--------|
| **Flow** | CEO uploads docs; Seravion verifies on company detail | Same + **verification status visible to CEO** (read-only) |
| **Deliverable** | CEO sees: uploaded / under review / verified / rejected with reason |
| **Deliverable** | Seravion can request re-upload without email-only loops |

---

### B3. Subscription selection & payment

| | Today | Target |
|---|--------|--------|
| **Flow** | After approval → CEO picks plan → Razorpay → success | Same + **plan shows module list before pay** |
| **Gap** | Payment succeeds but module access unchanged or unclear | Payment **activates plan modules** for that tenant (no full re-provision) |
| **Deliverable** | Plan cards: price, limits, **included modules** |
| **Deliverable** | Success page → onboarding complete → dashboard with only entitled modules in sidebar |

**Alternate path — Trial:** Seravion approves with trial → branch/user limits from approval **saved and enforced** (today UI sends limits but backend may not persist).

---

## Stage C — Seravion review & approval (gate between signup and go-live)

### C1. Company management list & detail

| | Today | Target |
|---|--------|--------|
| **Flow** | List companies → open detail → approve / reject / trial | Same + **configuration handoff** after approve |
| **Deliverable** | Company detail: status, docs, plan (assigned or pending), trial dates |
| **Deliverable** | Actions: Approve, Reject (with reason), Enable trial |
| **Deliverable** | On approve: **provision tenant** (database, CEO user) — as today, but reliable |

---

### C2. Approval decisions & trial

| | Today | Target |
|---|--------|--------|
| **Gap** | Trial branch/technician counts from UI not always stored | **Persist trial limits** on approval |
| **Deliverable** | Approve with trial: set end date, branch cap, user cap — stored and used when CEO creates branches/employees |
| **Deliverable** | Reject: CEO sees reason and can fix/resubmit if product allows |

---

### C3. Tenant settings (new — central Seravion config per company)

| | Today | Target |
|---|--------|--------|
| **Gap** | No single screen after approval to tune the tenant | **Tenant settings** linked from company detail |
| **Deliverable** | **Subscription panel:** active plan, dates, usage vs limits (branches, users, technicians) |
| **Deliverable** | **Module panel:** effective modules = plan ± optional overrides (upgrade/downgrade messaging) |
| **Deliverable** | **Workflow panel:** default how users start a contract (GMA / customer / user chooses) — GMA always on, no “disable GMA” toggle |
| **Deliverable** | **Regional panel (later):** country, tax regime — can be Week 2+ |
| **Deliverable** | Save → CEO environment updates without re-approval |

**Manager outcome:** Ops configures Acme Corp in one place after approval; CEO logs in to a ready tenant.

---

### C4. Assign or confirm plan at approval

| | Today | Target |
|---|--------|--------|
| **Flow** | CEO often selects plan themselves after approval | Seravion can **assign plan** at approval OR confirm CEO will purchase |
| **Deliverable** | Approval screen: optional “Assign plan now” vs “CEO will subscribe” |
| **Deliverable** | Audit trail: who approved, when, which plan |

---

## Stage D — CEO becomes Admin (first login & setup)

### D1. Login & first dashboard

| | Today | Target |
|---|--------|--------|
| **Flow** | CEO logs in → dashboard | Dashboard shows **only subscribed modules** in sidebar |
| **Gap** | Full ERP menu even if plan is CRM-only | Sidebar + routes **match plan**; upgrade banner if user hits locked module |
| **Deliverable** | Welcome / checklist: *Create first branch → Create roles → Add employees* |
| **Deliverable** | Permissions from CEO role respect plan modules |

---

### D2. First branch & structure

| | Today | Target |
|---|--------|--------|
| **Flow** | CEO creates branches in Module 7 | Block creation when **over plan branch limit** |
| **Deliverable** | Clear error: *“Plan allows 3 branches — upgrade to add more”* |
| **Deliverable** | Usage counter visible to CEO (and Seravion on tenant settings) |

---

### D3. Roles & employees (CEO as Admin)

| | Today | Target |
|---|--------|--------|
| **Flow** | CEO copies Seravion template or builds role → creates employees | Role editor shows **only modules on their plan** |
| **Deliverable** | Clone from platform template |
| **Deliverable** | Employee create blocked at **user limit** with upgrade message |
| **Deliverable** | Employee login improvement (later phase): login with Employee ID without tenant code |

---

### D4. Day-one sales workflow (light touch)

| | Today | Target |
|---|--------|--------|
| **Flow** | Contract always tied to GMA | GMA + Contract both available; contract **from GMA or from customer** per workflow default |
| **Deliverable** | Create contract: two entry paths; default from tenant settings |
| **Not in onboarding core** | Hiding GMA — GMA remains in product |

---

## Stage E — Ongoing Seravion oversight (post go-live)

| Feature | Deliverable |
|---------|-------------|
| **Usage monitoring** | Seravion sees branches/users vs plan on tenant settings |
| **Plan change** | Upgrade/downgrade updates modules (policy: hide vs read-only) |
| **Support context** | Company detail shows plan, workflow default, module list |
| **Platform dashboard** | Later phase: active tenants, trials expiring, revenue — today often stub/mock |
| **Reports** | Later phase: platform analytics for Seravion leadership |

---

## Week 1 deliverables (Super Admin → Admin onboarding focus)

Prioritized for **config + handoff + CEO first day** — not full V2.

| # | Area | Deliverable | Who benefits |
|---|------|-------------|--------------|
| 1 | **Subscription plans** | Module checklist on plan (what’s included) + limits on plan | Seravion sales/ops |
| 2 | **Company approval** | Persist trial limits when approving with trial | Seravion + CEO |
| 3 | **Tenant settings screen** | New page per company: plan, usage, workflow default | Seravion ops |
| 4 | **Approval handoff** | After approve → “Configure tenant” link to settings | Seravion ops |
| 5 | **CEO onboarding status** | Clear stepper/status on signup screens | CEO |
| 6 | **CEO plan page** | Show included modules before payment | CEO |
| 7 | **Sidebar after login** | Hide modules not on plan (basic version) | CEO + employees |
| 8 | **Limit enforcement** | Block branch/employee over plan cap with message | Revenue + ops |
| 9 | **Role templates** | Matrix preview scoped to plan (basic) | Seravion + CEO |
| 10 | **Support one-pager** | Written onboarding checklist for ops | Manager |

**Week 1 out of scope (later weeks):** Platform analytics dashboard, regional/country profiles, linked systems (SMTP, Razorpay admin), employee-ID login, notifications, full report packs.

---

## Before vs after (one paragraph for leadership)

**Before:** Seravion approves companies and sells plans, but the product doesn’t fully reflect plan contents — limits aren’t enforced, modules aren’t gated, and there’s no tenant settings screen. CEOs complete signup but may see the whole ERP or hit confusing blocks.

**After:** Seravion defines **plans with modules and limits**, **approves and configures each tenant** in one settings place, and CEOs **onboard with clear status**, **pay for a visible package**, and **land in a dashboard that matches what they bought** — then set up branch, roles, and team as Admin.

---

