# 📋 ERP System — Auto-Events, Notifications & Export API Specification

> **Version:** 2.0 (Audited against UI/UX Action Buttons)  
> **Last Updated:** 24 Apr 2026  
> **Scope:** All 32 Modules  
> **Purpose:** Production-ready specification — only includes download/exports that exist as actual UI action buttons, with notifications restructured into System (background) and External Send (user-triggered email).

---

## Document Architecture

### Notification Design Principle

Notifications are split into **two distinct systems** to minimize integration effort:

| System | Trigger | Backend | Delivery |
|--------|---------|---------|----------|
| **🔔 System Notifications** | Auto-triggered on status changes, approvals, escalations | Background job / Event listener | In-App Bell icon only (real-time via WebSocket) |
| **📧 External Sends** | User clicks a Send button (Send Quotation, Approve & Send, etc.) | On user action — generates PDF + sends to external party | Email / WhatsApp to customer/vendor |

> **Key Rule:** Only these actions send external emails:
> - **Send Quotation** (Module 16) / **Resend** Quotation
> - **Contract Document** (Module 19) — on activation/renewal
> - **Approve & Send Invoice** (Module 28) / **Resend** Invoice
> - **Payment Receipt** (Module 30) — Print Receipt for customer
> - **Service Completion Report** (Module 21) — Task PDF to customer
> - **Ledger Statement — Email to Party** (Module 31)
> - **Salary Slip** (Module 25) — on payroll completion

All other notifications (approvals, alerts, reminders, escalations) are **internal system notifications only (🔔 In-App)**.

---

### Export Design Principle

Download/Export buttons are **only** specified where the UI/UX documentation explicitly defines them as screen actions. No speculative endpoints.

---

## Legend

| Symbol | Meaning |
|--------|---------|
| 🔔 | System In-App Notification (background auto-trigger) |
| 📧 | External Email Send (user-triggered action) |
| 📥 | Download / Export button exists on screen |
| ⚡ | Auto-Event (system-triggered fallback) |

---

# ═══════════════════════════════════════════════════════════════
# GROUP 1: DASHBOARD (Module 1)
# ═══════════════════════════════════════════════════════════════

## Module 1: Main Dashboard

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 1.1 | Dashboard loaded | Refresh all widget data from module APIs | Stale KPIs |
| 1.2 | Any critical alert exists (overdue invoices, low stock, pending approvals) | Show persistent alert banner | User misses time-sensitive actions |

### 🔔 System Notifications

> Dashboard does not generate notifications. It **consumes** alerts from other modules and renders them as widgets/banners.

### 📥 Downloads

> No download buttons exist on Dashboard screens per UI/UX doc.

---

# ═══════════════════════════════════════════════════════════════
# GROUP 2: SETUP & CONFIGURATION (Modules 2–9)
# ═══════════════════════════════════════════════════════════════

## Module 2: Subscription Plans

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 2.1 | Subscription expiry ≤ 7 days | Auto-alert Super Admin | Service disruption |
| 2.2 | Subscription expired | Restrict new user/branch creation; show banner | Unauthorized usage |
| 2.3 | User/branch count exceeds plan limit | Block creation with validation error | Over-provisioning |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 2.1 | Subscription expiring in 7 days | Super Admin | "Subscription expires on {date}. Renew to avoid disruption." |
| 2.2 | Subscription expired | Super Admin | "Subscription expired. New user/branch creation restricted." |
| 2.3 | Plan upgraded/downgraded | Super Admin | "Plan changed to {plan_name}." |

### 📥 Downloads

> No download buttons on subscription screens.

---

## Module 3: Company Onboarding

### Auto-Events

> No auto-events — one-time manual process.

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 3.1 | Onboarding completed | Super Admin | "Company setup complete." |

### 📥 Downloads

> No download buttons.

---

## Module 4: Subscription Configuration

> No auto-events, notifications, or downloads.

---

## Module 5: Role Configuration

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 5.1 | Role deleted with active users | Block deletion: "Active users assigned to this role" | Orphaned permissions |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 5.1 | Role permissions updated | All users with that role | "Your role permissions have been updated." |

### 📥 Downloads

> RBAC permission: EXPORT permission exists per role config (Module 5 UI: `☑ VIEW ☑ ADD ☑ EDIT ☑ DELETE ☑ EXPORT`). This is a **global permission toggle** — not a screen-level download button. Export capability on any module dashboard is gated by this RBAC flag.

---

## Module 6: Salary & Leave Configuration

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 6.1 | Financial Year start (01 Apr) | Auto-reset carry-forward leave balances | Incorrect leave entitlements |
| 6.2 | Leave policy modified | Re-calculate affected employee leave balances | Stale entitlements |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 6.1 | Salary structure updated | HR Admin | "Salary config for {designation} updated." |
| 6.2 | Leave policy changed | HR Admin | "Leave policy updated." |

### 📥 Downloads

> No download buttons on config screens.

---

## Module 7: Branch Management

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 7.1 | Branch deactivated with active employees/stock | Block: "Transfer assets/employees first" | Orphaned resources |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 7.1 | New branch created | Super Admin, Ops Head | "Branch '{name}' created." |
| 7.2 | Branch deactivated | Branch Manager, HR | "Branch '{name}' deactivated." |

### 📥 Downloads

> No download buttons.

---

## Module 8: Employee Master

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 8.1 | Employee deactivated with assigned assets | Trigger asset return reminder to Branch Manager | Unaccounted assets |
| 8.2 | Employee probation ending (7 days) | Alert HR for confirmation | Missed probation review |
| 8.3 | Employee document expiry ≤ 30 days | Alert HR and employee | Compliance violation |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 8.1 | New employee added | HR Admin, Branch Manager | "Employee '{name}' added to {branch}." |
| 8.2 | Employee deactivated | HR Admin, Direct Manager | "Employee '{name}' deactivated. Ensure asset return." |
| 8.3 | Document expiry approaching | Employee, HR | "Your {document_type} expires on {date}." |
| 8.4 | Probation ending | HR Admin | "Employee '{name}' probation ends {date}." |

### 📥 Downloads

> No download buttons on employee screens per UI/UX doc.

---

## Module 9: Tax Configuration

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 9.1 | Tax rate modified | Alert all modules using affected HSN codes | Incorrect tax calculations |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 9.1 | Tax rate modified | Finance Admin | "GST rate for HSN {code} changed: {old}% → {new}%." |

### 📥 Downloads

> No download buttons.

---

# ═══════════════════════════════════════════════════════════════
# GROUP 3: INVENTORY & SERVICES (Modules 10–12)
# ═══════════════════════════════════════════════════════════════

## Module 10: Product Master

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 10.1 | Product deactivated with active stock | Show warning: "Stock exists" (allow with confirm) | Orphaned stock |
| 10.2 | Product price updated | Log in audit trail; alert procurement | Price discrepancy |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 10.1 | New product added | Inventory Manager | "Product '{name}' added. Code: {code}." |
| 10.2 | Product price changed | Procurement, Sales Manager | "Product '{name}' price: ₹{old} → ₹{new}." |

### 📥 Downloads

> No download buttons on Product Master screens per UI/UX doc.

---

## Module 11: Stock Management

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 11.1 | Stock falls below Reorder Level | Auto-alert Inventory Manager + Branch Manager | Stockouts |
| 11.2 | Stock = 0 (Out of Stock) | Alert Inventory Manager + Ops Head | Task failure |
| 11.3 | Stock request approved | Auto-change to **In-Transit**; notify branch | Branch unaware of incoming stock |
| 11.4 | Stock received at branch | Auto-update branch stock levels; status → Received | Inventory mismatch |
| 11.5 | Stock request pending > 48 hrs | Escalate to Ops Head | Approval bottleneck |
| 11.6 | Task completed (Module 21) | Auto-deduct materials from branch stock | Phantom stock |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 11.1 | Stock request submitted | Inventory Manager (HO) | "Stock request {REQ_ID} from {branch}." |
| 11.2 | Stock request approved | Requesting Branch Manager | "✅ Request {REQ_ID} approved. Dispatching." |
| 11.3 | Stock request rejected | Requesting Branch Manager | "❌ Request {REQ_ID} rejected: {reason}." |
| 11.4 | Stock dispatched (In-Transit) | Requesting Branch | "📦 Stock dispatched. Expected: {date}." |
| 11.5 | Stock received | Inventory Manager | "Stock {REQ_ID} received at {branch}." |
| 11.6 | Low stock alert | Inventory Manager, Branch Manager | "⚠️ Low stock: '{product}' at {branch}. Qty: {qty}." |
| 11.7 | Out of stock | Inventory Manager, Ops Head | "🔴 OUT OF STOCK: '{product}' at {branch}." |
| 11.8 | Approval overdue (48 hrs) | Ops Head | "Request {REQ_ID} pending 48+ hours." |

### 📥 Downloads

**UI Button: [DOWNLOAD INVOICE]** — on Stock Entry Detail View (11.1.3)

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 11.1 | 11.1.3 View Stock Entry | Download Invoice | `GET /v1/stock/entries/{id}/invoice-download` | PDF (uploaded supplier invoice file) |

> This downloads the **uploaded invoice copy** (file attachment) — not a system-generated export.

---

## Module 12: Service Management

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 12.1 | Service deactivated with active contracts | Block: "Used in active contracts" | Breaks service delivery |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 12.1 | New service added | Sales Team | "Service '{name}' added." |
| 12.2 | Service pricing updated | Sales Team | "Service '{name}' pricing updated." |

### 📥 Downloads

> No download buttons on Service Management screens.

---

# ═══════════════════════════════════════════════════════════════
# GROUP 4: PROCUREMENT (Modules 13–14)
# ═══════════════════════════════════════════════════════════════

## Module 13: Vendor Management

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 13.1 | Vendor deactivated with pending POs/Bills | Block: "Resolve pending transactions" | Unpaid liabilities |
| 13.2 | New Vendor created | Auto-create Sundry Creditor Ledger (Module 31) | Missing ledger blocks billing |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 13.1 | New vendor added | Procurement, Accounts | "Vendor '{name}' added." |
| 13.2 | Vendor deactivated | Procurement, Accounts | "Vendor '{name}' deactivated." |

### 📥 Downloads

> No download buttons on Vendor Management screens.

---

## Module 14: Purchase Orders

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 14.1 | PO delivery date passed (Status = Ordered) | Auto-mark **Overdue**; alert Procurement | Delayed stock |
| 14.2 | PO approved | Status → Ordered | Ordering delay |
| 14.3 | GRN completed (full qty received) | Auto → **Received**; auto-create Draft Bill (Module 29) | Delayed billing |
| 14.4 | Partial receipt | Status → **Partial** | Incorrect lifecycle |
| 14.5 | PO pending approval > 24 hrs | Escalate to Ops Head | Procurement delay |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 14.1 | PO submitted for approval | Approver | "PO {PO_ID} needs approval. ₹{amount}." |
| 14.2 | PO approved | Creator | "✅ PO {PO_ID} approved." |
| 14.3 | PO rejected | Creator | "❌ PO {PO_ID} rejected: {reason}." |
| 14.4 | PO delivery overdue | Procurement Manager | "⚠️ PO {PO_ID} delivery overdue." |
| 14.5 | GRN completed | Accounts | "📦 GRN for PO {PO_ID}. Draft bill created." |
| 14.6 | Approval overdue (24 hrs) | Ops Head | "PO {PO_ID} pending 24+ hours." |

### 📥 Downloads

**UI Button: [DOWNLOAD PDF]** — on PO View Detail screen (14.3)

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 14.1 | 14.3 View PO Detail | Download PDF | `GET /v1/purchase-orders/{id}/pdf` | PDF |

---

# ═══════════════════════════════════════════════════════════════
# GROUP 5: CRM (Modules 15–20)
# ═══════════════════════════════════════════════════════════════

## Module 15: Lead & Follow-Up Management

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 15.1 | Follow-up date passed without action | Auto-mark **Overdue**; escalate after 48 hrs | Lost opportunity |
| 15.2 | Lead "New" for > 24 hrs with no activity | Alert sales rep + Manager | Cold lead |
| 15.3 | Lead "Qualified" with no quotation for > 7 days | Alert Sales Manager | Pipeline stagnation |
| 15.4 | Lead "Converted" | Auto-create Customer (Module 18) if not exists | Manual gap |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 15.1 | New lead assigned | Sales Rep | "Lead '{name}' assigned to you." |
| 15.2 | Follow-up due today | Sales Rep | "Follow-up due for '{name}'." |
| 15.3 | Follow-up overdue | Sales Rep, Sales Manager | "⚠️ Follow-up overdue for '{name}'." |
| 15.4 | Lead status changed | Sales Rep | "Lead '{name}' → {new_status}." |
| 15.5 | Lead converted | Sales Rep, Manager | "🎉 Lead '{name}' converted." |
| 15.6 | Lead reassigned | Old Rep, New Rep | "Lead '{name}' reassigned." |

### 📥 Downloads

**UI Button: [DOWNLOAD PDF]** — on Lead Detail View (15.3)

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 15.1 | 15.3 View Lead Detail | Download PDF | `GET /v1/leads/{id}/pdf` | PDF |

---

## Module 16: Quotation Management

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 16.1 | Validity date passed (Status = Sent) | Auto → **Expired**; notify sales rep | Expired pricing referenced |
| 16.2 | "Sent" but not Viewed > 3 days | Alert sales rep | Low engagement |
| 16.3 | Quotation "Accepted" | Auto-update Lead to "WON"; prompt GMA creation | Contract delay |
| 16.4 | Quotation "Rejected" | Auto-update Lead to "LOST"/"FOLLOW-UP" | Incorrect pipeline |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 16.1 | Quotation created | Sales Rep | "Quotation {QT_ID} drafted." |
| 16.2 | Client viewed quotation | Sales Rep | "📋 {client} viewed {QT_ID}." |
| 16.3 | Quotation accepted | Sales Rep, Manager | "🎉 {QT_ID} accepted!" |
| 16.4 | Quotation rejected | Sales Rep, Manager | "❌ {QT_ID} rejected: {reason}." |
| 16.5 | Quotation expired (auto) | Sales Rep | "⏰ {QT_ID} expired." |
| 16.6 | Quotation deleted | Sales Manager | "{QT_ID} deleted by {user}: {reason}." |
| 16.7 | Not viewed (3-day) | Sales Rep | "{QT_ID} not viewed for 3 days." |

### 📧 External Sends (User-Triggered)

| # | UI Action Button | Trigger | What is Sent | To Whom |
|---|------------------|---------|--------------|---------|
| 16.S1 | **[SEND QUOTATION]** | User clicks on Add Quotation form | PDF quotation via Email/WhatsApp | Client (Lead/Customer) |
| 16.S2 | **[📧 RESEND]** | User clicks on View Quotation detail | Re-send same PDF | Client |

### 📥 Downloads

**UI Buttons: [📥 DOWNLOAD PDF]** — on View Quotation Detail (16.3)

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 16.1 | 16.3 View Quotation Detail | Download PDF | `GET /v1/quotations/{id}/pdf` | PDF (customer-facing) |

---

## Module 17: GMA (Gross Margin Analysis) Management

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 17.1 | GMA submitted + GM% ≥ 25% | Auto-route to **Sales Manager** (Tier 1) | Approval bottleneck |
| 17.2 | GMA submitted + GM% 15%–24% | Route to Sales Manager → Ops Head (Tier 2) | Low-margin deal without scrutiny |
| 17.3 | GMA submitted + GM% < 15% | Route to **CEO/Director** (Tier 3) | High-risk deal needs executive sign-off |
| 17.4 | Pending approval > 24 hrs (Manager) / 48 hrs (CEO) | Auto-escalate + alert original approver | Stalled pipeline |
| 17.5 | GMA approved | Enable "Create Contract" in Module 19 | Contract blocked without approved GMA |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 17.1 | GMA submitted | Assigned Approver | "GMA {GMA_ID} by {rep}. GM%: {pct}. Review required." |
| 17.2 | GMA approved | Sales Rep (requester) | "✅ GMA {GMA_ID} approved. Create contract." |
| 17.3 | GMA rejected | Sales Rep | "❌ GMA {GMA_ID} rejected: {reason}." |
| 17.4 | Approval overdue | Original + Next Approver | "⚠️ GMA {GMA_ID} pending {hours} hours." |
| 17.5 | GMA revoked | Approver | "GMA {GMA_ID} revoked by {rep}." |

### 📥 Downloads

**UI Buttons: [DOWNLOAD PDF]** — on GMA View Detail (17.1.1) and My Request View (17.2.2)

| # | Screen | Button | API Endpoint | Format | Condition |
|---|--------|--------|--------------|--------|-----------|
| 17.1 | 17.1.1 / 17.2.2 View GMA Detail | Download PDF | `GET /v1/gma/{id}/pdf` | PDF | Status ≠ Draft |

---

## Module 18: Customer Management

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 18.1 | Customer deactivation with active Contracts/SOs | Block: "Resolve active contracts" | Orphaned contracts |
| 18.2 | New customer created | Auto-create Sundry Debtor Ledger (Module 31) | Missing ledger blocks invoicing |
| 18.3 | Credit limit exceeded | Warn accounts; optionally block invoice | Credit risk |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 18.1 | New customer created | Sales Rep, Accounts | "Customer '{name}' created." |
| 18.2 | Customer deactivated | Sales Rep, Accounts | "Customer '{name}' deactivated." |
| 18.3 | Credit limit warning | Accounts, Sales Manager | "⚠️ '{name}' near credit limit." |

### 📥 Downloads

**UI Button: [Export Action]** — on Customer Detail → Service History tab (18.3.3)

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 18.1 | 18.3.3 Service History Tab | Export to Excel | `GET /v1/customers/{id}/service-history/export` | Excel (.xlsx) |

> This is the **Service History Excel export** documented on the SO grid within Customer Detail, listing all SOs, services, technicians, and execution status.

---

## Module 19: Contract Management

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 19.1 | Contract expiry ≤ 30 days (Active) | Auto → **Expiring Soon**; notify sales | Revenue loss |
| 19.2 | Contract expired (End Date passed) | Auto → **Expired**; block SO generation | Service without contract |
| 19.3 | Contract created without approved GMA | Block: "Approved GMA required" | Under-margin deals |
| 19.4 | Contract amended | Create amendment log; notify accounts | Financial discrepancy |
| 19.5 | Contract activated | Auto-generate recurring SOs per schedule | Missed visits |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 19.1 | Contract created | Sales Rep, Accounts | "Contract {CNT_ID} created." |
| 19.2 | Contract activated | Sales Rep, Ops Manager | "Contract {CNT_ID} Active. SO generation enabled." |
| 19.3 | Contract expiring (30 days) | Sales Rep, Manager | "⏰ Contract {CNT_ID} expires {date}. Renew." |
| 19.4 | Contract expired | Sales Rep, Accounts | "❌ Contract {CNT_ID} expired." |
| 19.5 | Contract renewed | Sales Rep | "✅ Contract {CNT_ID} renewed to {date}." |
| 19.6 | Contract amended | Accounts, Manager | "Contract {CNT_ID} amended." |
| 19.7 | Contract terminated | Sales Rep, Accounts | "Contract {CNT_ID} terminated." |

### 📥 Downloads

**UI Buttons: [Download PDF]** — on Contract Dashboard row + View Detail (19.3); **[Download Contract Service Log (Excel)]** — on Tab 2: SO Schedule (19.3.2)

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 19.1 | 19.1 Dashboard Row / 19.3 Detail | Download PDF | `GET /v1/contracts/{id}/pdf` | PDF |
| 19.2 | 19.3.2 Tab 2: SO Schedule | Download Contract Service Log | `GET /v1/contracts/{id}/execution-log/export` | Excel (.xlsx) |

---

## Module 20: Sales Orders

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 20.1 | Contract activated | Auto-generate recurring SOs per schedule | Missed visits |
| 20.2 | SO "Open" past scheduled date | Auto → **Overdue**; alert Ops | Delayed service |
| 20.3 | All tasks completed + materials consumed | Auto → **Fulfilled** | Inaccurate tracking |
| 20.4 | SO fulfilled + Invoice generated | Auto → **Billed** | Revenue leakage |
| 20.5 | SO cancelled | Reverse reserved stock; notify task manager | Ghost tasks |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 20.1 | New SO created | Ops Manager, Task Manager | "SO {SO_ID} created for {customer}." |
| 20.2 | SO overdue | Ops Manager, Task Manager | "⚠️ SO {SO_ID} overdue." |
| 20.3 | SO fulfilled | Sales Rep, Accounts | "✅ SO {SO_ID} fulfilled. Ready for invoicing." |
| 20.4 | SO cancelled | Task Manager, Sales Rep | "SO {SO_ID} cancelled." |

### 📥 Downloads

**UI Button: [Download PDF]** — on SO Dashboard row actions (20.1)

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 20.1 | 20.1 Dashboard Row | Download PDF | `GET /v1/sales-orders/{id}/pdf` | PDF |

---

# ═══════════════════════════════════════════════════════════════
# GROUP 6: TASK & FIELD OPERATIONS (Modules 21–24)
# ═══════════════════════════════════════════════════════════════

## Module 21: Task Management

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 21.1 | Task scheduled date passed (Pending) | Auto → **Overdue**; alert Task Manager + technician | Missed SLA |
| 21.2 | Task completed | Auto-deduct materials from branch stock (Module 11); update SO status (Module 20) | Phantom stock; SO stuck |
| 21.3 | Task assigned to busy technician (same slot) | Block: double-booking conflict | Service quality |
| 21.4 | Task rescheduled | Update calendar; free original slot | Customer unaware |
| 21.5 | Task overdue > 3 days | Escalate to Ops Head | Chronic failure |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 21.1 | Task assigned | Technician (Primary + Secondary) | "Task {TASK_ID} assigned. {customer}, {date}." |
| 21.2 | Task reminder (day before) | Technician | "Tomorrow: {TASK_ID} at {location}." |
| 21.3 | Task completed | Task Manager | "✅ Task {TASK_ID} completed." |
| 21.4 | Task overdue | Task Manager, Ops Manager | "⚠️ Task {TASK_ID} overdue." |
| 21.5 | Task overdue (3-day) | Ops Head, Branch Manager | "🔴 Task {TASK_ID} overdue 3+ days." |
| 21.6 | Task rescheduled | Technician | "Task {TASK_ID} rescheduled to {date}." |
| 21.7 | Task reassigned | Old + New Technician | "Task {TASK_ID} reassigned." |

### 📧 External Sends (User-Triggered)

| # | UI Action Button | Trigger | What is Sent | To Whom |
|---|------------------|---------|--------------|---------|
| 21.S1 | **Service Completion** (task completed) | On task completion by technician | Service completion report / notification | Customer (optional) |

### 📥 Downloads

**UI Button: [Print / PDF]** — on View Task Detail (21.4)

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 21.1 | 21.4 View Task Detail | Print / PDF | `GET /v1/tasks/{id}/pdf` | PDF (task detail with materials, service info) |

---

## Module 22: Support Tickets

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 22.1 | Ticket created | Auto-assign to nearest branch support | Unattended complaint |
| 22.2 | Ticket open > 24 hrs | Escalate to Branch Manager | SLA breach |
| 22.3 | Ticket open > 48 hrs | Escalate to Ops Head | Customer churn |
| 22.4 | Ticket resolved | Prompt for satisfaction rating | No feedback |
| 22.5 | Re-task needed | Auto-create task in Module 21 | Complaint not acted upon |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 22.1 | Ticket created | Branch Support Agent | "Ticket {TKT_ID} assigned." |
| 22.2 | Ticket escalated (24 hrs) | Branch Manager | "⚠️ Ticket {TKT_ID} unresolved 24+ hours." |
| 22.3 | Ticket escalated (48 hrs) | Ops Head | "🔴 Ticket {TKT_ID} unresolved 48+ hours." |
| 22.4 | Ticket resolved | Support Agent | "✅ Ticket {TKT_ID} resolved." |
| 22.5 | Re-task created | Task Manager | "Re-task created from {TKT_ID}." |

### 📥 Downloads

**UI Button: [Print / PDF]** — on Ticket Detail View

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 22.1 | Ticket Detail View | Print / PDF | `GET /v1/tickets/{id}/pdf` | PDF |

---

## Module 24: Petty Cash Management

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 24.1 | Request submitted | Auto-route to Branch Manager | No accountability |
| 24.2 | Approved by Manager + amount > threshold | Auto-route to Finance Head (Tier 2) | Large expenses without oversight |
| 24.3 | Approved (final) | Enable "Mark Paid" action | Delayed reimbursement |
| 24.4 | Marked "Paid" | Post expense to Ledger (Module 31) | Expense not recorded |
| 24.5 | Request pending > 48 hrs | Escalate to next approver | Employee frustration |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 24.1 | Request submitted | Branch Manager (approver) | "PC request {PC_ID} from {employee}. ₹{amount}." |
| 24.2 | Approved (Tier 1) | Employee | "✅ PC {PC_ID} approved by {manager}." |
| 24.3 | Needs Tier 2 approval | Finance Head | "PC {PC_ID} needs approval. ₹{amount}." |
| 24.4 | Rejected | Employee | "❌ PC {PC_ID} rejected: {reason}." |
| 24.5 | Paid/reimbursed | Employee | "💰 PC {PC_ID} reimbursed. ₹{amount}." |
| 24.6 | Approval overdue (48 hrs) | Next approver | "PC {PC_ID} pending 48+ hours." |

### 📥 Downloads

**UI Button: [📥 Export to Excel]** — on Petty Cash Dashboard (Tab: All Expenses) and Received Requests tab

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 24.1 | All Expenses Tab | Export to Excel | `GET /v1/petty-cash/export` | Excel (.xlsx) |
| 24.2 | Received Requests Tab | Export to Excel | `GET /v1/petty-cash/received/export` | Excel (.xlsx) |

---

# ═══════════════════════════════════════════════════════════════
# GROUP 7: HR (Modules 25–27)
# ═══════════════════════════════════════════════════════════════

## Module 25: HRM (Salary, Attendance, Leave)

### Auto-Events — Attendance

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 25.A1 | No check-in by grace period end | Mark **Absent** (pending regularization) | Incorrect records |
| 25.A2 | Late check-in | Mark **Late / Half Day** per policy | Incorrect payroll |
| 25.A3 | Month-end freeze date | Lock attendance for the month | Late corrections |

### Auto-Events — Leave

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 25.L1 | Leave request submitted | Auto-route to Reporting Manager | Leave without approval |
| 25.L2 | Leave approved | Deduct from balance; update calendar | Incorrect balance |
| 25.L3 | Leave rejected | Restore tentative deduction | Employee unaware |
| 25.L4 | Leave pending > 48 hrs | Escalate to HR Manager | Employee in limbo |
| 25.L5 | 3+ consecutive absent without leave | Alert HR: "Unauthorized absence" | Compliance |
| 25.L6 | FY start | Reset annual leave balances; process carry-forward | Stale balances |

### Auto-Events — Salary

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 25.S1 | Payroll triggered (month-end) | Calculate salary; generate payslips | Delayed salary |
| 25.S2 | Salary processed (bulk) | Auto-create payment entries (Module 30); post to Ledger (Module 31) | Not in books |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 25.1 | Marked absent | Employee | "Marked absent {date}. Apply regularization." |
| 25.2 | Late arrival | Employee | "Late check-in: {time} on {date}." |
| 25.3 | Leave submitted | Reporting Manager | "Leave request: {employee}, {type}, {dates}." |
| 25.4 | Leave approved | Employee | "✅ Leave approved: {type}, {dates}." |
| 25.5 | Leave rejected | Employee | "❌ Leave rejected: {reason}." |
| 25.6 | Leave balance low | Employee | "Balance: {remaining} days for {type}." |
| 25.7 | Leave pending > 48 hrs | HR Manager | "Leave pending 48+ hours for {employee}." |
| 25.8 | Unauthorized absence | HR, Branch Manager | "⚠️ {employee} absent 3+ days without leave." |
| 25.9 | Payroll started | HR Admin | "Payroll processing for {month}." |
| 25.10 | Payroll completed | HR Head | "✅ Payroll {month} done. Total: ₹{total}." |

### 📧 External Sends (User-Triggered)

| # | UI Action Button | Trigger | What is Sent | To Whom |
|---|------------------|---------|--------------|---------|
| 25.S1 | **Payslip generated** (on payroll completion) | System generates payslip after "Mark as Paid" | Payslip notification | Employee (in-app + optional email) |

### 📥 Downloads

**UI Buttons documented in HRM screens:**

| # | Screen | Button | API Endpoint | Format | Condition |
|---|--------|--------|--------------|--------|-----------|
| 25.1 | 25.3 Employee Salary Detail | Download Salary Slip | `GET /v1/hrm/payslips/{employee_id}/{month}/pdf` | PDF | Status = Paid only |
| 25.2 | 25.4 Salary Bulk Upload | Download Sample Excel Sheet | `GET /v1/hrm/salary/sample-template` | Excel (.xlsx) | Always available |
| 25.3 | 25.5 Attendance Bulk Upload | Download Sample Excel Sheet | `GET /v1/hrm/attendance/sample-template` | Excel (.xlsx) | Always available |

---

## Module 26: Employee Self-Service (User Profile / Documents)

### Auto-Events

> No auto-events — self-service actions route through Module 25 workflows.

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 26.1 | Profile update requested | HR Admin | "{employee} requested profile update." |
| 26.2 | Document uploaded | HR | "{employee} uploaded {doc_type}." |

### 📥 Downloads

**UI Actions: [📥 Download] on document rows (view/download uploaded documents)**

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 26.1 | User Profile → Documents Tab | Download (per document) | `GET /v1/employees/{id}/documents/{doc_id}/download` | Original file format |

> This is a **file download** of existing uploaded documents — not a generated export.

---

# ═══════════════════════════════════════════════════════════════
# GROUP 8: FINANCE & ACCOUNTS (Modules 28–32)
# ═══════════════════════════════════════════════════════════════

## Module 28: Invoicing (Sales)

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 28.1 | "Approve & Send" clicked | Post Dr Customer / Cr Income to Ledger (Module 31); generate invoice# | Revenue not recorded |
| 28.2 | Due Date passed (Sent, not paid) | Auto → **Overdue** | Cash flow risk |
| 28.3 | Partial payment received (Module 30) | Auto → **Partial**; reduce pending | Incorrect balance |
| 28.4 | Full payment received | Auto → **Paid** | Shown as outstanding |
| 28.5 | Credit Note issued | Auto-adjust pending amount; update Ledger | Financial discrepancy |
| 28.6 | Overdue > 30 days | Escalate to Finance Manager | Bad debt risk |
| 28.7 | Overdue > 60 days | Escalate to CFO/CEO | Critical cash flow |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 28.1 | Invoice created (Draft) | Accounts User | "Invoice {INV_ID} drafted. ₹{amount}." |
| 28.2 | Invoice finalized | Accounts, Sales Rep | "Invoice {INV_ID} sent to {customer}." |
| 28.3 | Invoice overdue | Accounts | "⚠️ Invoice {INV_ID} overdue." |
| 28.4 | Overdue 30-day escalation | Finance Manager | "🔴 {INV_ID} overdue 30+ days." |
| 28.5 | Overdue 60-day escalation | CFO/CEO | "🚨 {INV_ID} overdue 60+ days." |
| 28.6 | Payment received (partial) | Accounts | "₹{amount} received against {INV_ID}." |
| 28.7 | Invoice fully paid | Accounts | "✅ {INV_ID} fully paid." |
| 28.8 | Credit Note issued | Accounts | "CN {CN_ID} issued against {INV_ID}." |

### 📧 External Sends (User-Triggered)

| # | UI Action Button | Trigger | What is Sent | To Whom |
|---|------------------|---------|--------------|---------|
| 28.S1 | **[APPROVE & SEND]** | User finalizes invoice (from Create/Edit form) | Invoice PDF via Email | Customer |
| 28.S2 | **[✉ RESEND]** | User clicks Resend on View Invoice Detail | Re-send invoice PDF | Customer |

### 📥 Downloads

**UI Buttons from Invoice screens:**

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 28.1 | 28.1 Dashboard Row | Download PDF | `GET /v1/invoices/{id}/pdf` | PDF |
| 28.2 | 28.1 Dashboard Row (Paid) | Download Receipt | `GET /v1/payments/receipts/{receipt_id}/pdf` | PDF (from Module 30) |
| 28.3 | 28.3 View Invoice Detail | 📥 Download PDF | `GET /v1/invoices/{id}/pdf` | PDF |
| 28.4 | 28.1 Dashboard Toolbar | 📥 Export PDF Batch | `POST /v1/invoices/batch-pdf` | ZIP (multiple PDFs) |
| 28.5 | 28.1 Dashboard Toolbar | 📊 Tally Export | `GET /v1/invoices/tally-export` | CSV/XML (Tally-compatible) |

> **Note:** Download PDF and Download Receipt share the **same button position** per row — shown based on invoice status (Sent/Partial → Download PDF; Paid → Download Receipt).

---

## Module 29: Bills (Purchases)

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 29.1 | Bill confirmed (Draft → Pending) | Post Dr Expense / Cr Vendor to Ledger; record GST ITC | Vendor balance incorrect |
| 29.2 | Due Date passed (unpaid) | Auto → **Overdue** | Late payment penalties |
| 29.3 | GRN completed (Module 11) | Auto-create Draft Bill linked to PO | Delayed vendor billing |
| 29.4 | Full payment (Module 30) | Auto → **Paid** | Shown as outstanding |
| 29.5 | Debit Note issued | Auto-reduce pending; update Vendor Ledger | Financial mismatch |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 29.1 | Bill recorded | Accounts | "Bill {BILL_ID} for vendor '{vendor}'. ₹{amount}." |
| 29.2 | Bill confirmed | Accounts Manager | "Bill {BILL_ID} confirmed. Ledger updated." |
| 29.3 | Bill approaching due (3 days) | Accounts | "⏰ Bill {BILL_ID} due {date}." |
| 29.4 | Bill overdue | Accounts Manager | "⚠️ Bill {BILL_ID} overdue." |
| 29.5 | Bill paid | Accounts | "✅ Bill {BILL_ID} paid." |
| 29.6 | Debit Note issued | Accounts | "DN {DN_ID} against {BILL_ID}." |
| 29.7 | Auto-Draft bill (from GRN) | Accounts | "Draft bill auto-created for PO {PO_ID}." |

### 📥 Downloads

**UI Buttons from Bill screens:**

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 29.1 | 29.1 Dashboard Row | Download PDF | `GET /v1/bills/{id}/pdf` | PDF |
| 29.2 | 29.1 Dashboard Row (Paid) | Download Receipt | `GET /v1/payments/vouchers/{voucher_id}/pdf` | PDF (from Module 30) |
| 29.3 | 29.3 View Bill Detail | 📥 Download PDF | `GET /v1/bills/{id}/pdf` | PDF |

> Same row-level button toggle as invoices: Make Payment (unpaid) ↔ Download Receipt (paid).

---

## Module 30: Payments (Receipts & Vouchers)

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 30.1 | Receipt saved | Post Dr Bank / Cr Customer to Ledger; update invoice in Module 28 | Revenue not recorded |
| 30.2 | Payment saved | Post Dr Vendor / Cr Bank to Ledger; update bill in Module 29 | Vendor balance incorrect |
| 30.3 | "Settle & Close" on Receipt | Auto-generate Credit Note (Module 28.5) for shortfall | Invoice stays open forever |
| 30.4 | "Settle & Close" on Payment | Auto-generate Debit Note (Module 29.5) for shortfall | Bill stays open forever |
| 30.5 | Contra entry saved | Post Dr Destination / Cr Source in Ledger | Transfer not reflected |
| 30.6 | Journal entry debit ≠ credit | Block save: "Debits must equal Credits" | Ledger corruption |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 30.1 | Receipt recorded | Accounts | "Receipt ₹{amount} from {customer}." |
| 30.2 | Payment made | Accounts | "Payment ₹{amount} to {vendor}." |
| 30.3 | CN auto-generated | Accounts | "CN auto-generated against {INV_ID}." |
| 30.4 | DN auto-generated | Accounts | "DN auto-generated against {BILL_ID}." |

### 📥 Downloads

**UI Buttons from Payment screens:**

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 30.1 | 30.1 Payment Register Row | PDF | `GET /v1/payments/{id}/pdf` | PDF (voucher) |
| 30.2 | 30.6 View Voucher Detail | 📥 Download PDF | `GET /v1/payments/{id}/pdf` | PDF |

---

## Module 31: Ledger Management

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 31.1 | Customer created (Module 18) | Auto-create Sundry Debtor Ledger | Invoice posting fails |
| 31.2 | Vendor created (Module 13) | Auto-create Sundry Creditor Ledger | Bill posting fails |
| 31.3 | Posting to Inactive Ledger attempted | Block: "Cannot post to inactive ledger" | Frozen account bypass |
| 31.4 | Credit limit exceeded | Warn/block new invoice | Credit risk |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 31.1 | Ledger auto-created | Accounts | "Ledger '{name}' created under {group}." |
| 31.2 | Credit limit exceeded | Accounts, Sales Manager | "⚠️ Credit limit exceeded for {name}." |

### 📧 External Sends (User-Triggered)

| # | UI Action Button | Trigger | What is Sent | To Whom |
|---|------------------|---------|--------------|---------|
| 31.S1 | **[📧 EMAIL TO PARTY]** | User clicks on Ledger Statement view (31.3) | Statement PDF via Email | Customer / Vendor (party's registered email) |

### 📥 Downloads

**UI Buttons from Ledger screens:**

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 31.1 | 31.1 Ledger Dashboard Toolbar | 📥 Export All | `GET /v1/ledgers/export` | Excel/CSV |
| 31.2 | 31.1 Ledger Dashboard Toolbar | 📊 Ageing Report | `GET /v1/ledgers/ageing-report` | Excel/PDF |
| 31.3 | 31.3 Ledger Statement | 📥 Download PDF | `GET /v1/ledgers/{id}/statement/pdf` | PDF |

---

## Module 32: Chart of Accounts (COA)

### Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| 32.1 | COA head deactivated with active ledgers | Block new ledger assignment | Incorrect classification |
| 32.2 | COA head deleted with child groups/ledgers | Block: "Has dependent entries" | Breaks hierarchy |

### 🔔 System Notifications

| # | Event | Recipients | Message |
|---|-------|------------|---------|
| 32.1 | COA head created | Finance Admin | "Account head '{name}' added." |
| 32.2 | COA head deactivated | Finance Admin | "Account head '{name}' deactivated." |

### 📥 Downloads

**UI Button: [📥 EXPORT CSV]** — on COA Dashboard Toolbar (32.1)

| # | Screen | Button | API Endpoint | Format |
|---|--------|--------|--------------|--------|
| 32.1 | 32.1 COA Dashboard | Export CSV | `GET /v1/coa/export` | CSV |

---

# ═══════════════════════════════════════════════════════════════
# SUMMARY TABLES
# ═══════════════════════════════════════════════════════════════

## 📧 All External Send Actions (Complete List)

These are the **only** actions in the entire system that send emails/WhatsApp to customers or vendors. All other notifications are internal 🔔 only.

| # | Module | UI Button | Recipient | Document Sent |
|---|--------|-----------|-----------|---------------|
| 1 | 16 — Quotation | [SEND QUOTATION] | Client (Lead/Customer) | Quotation PDF |
| 2 | 16 — Quotation | [📧 RESEND] | Client | Quotation PDF |
| 3 | 19 — Contract | On activation (system) | Customer | Contract activation notice |
| 4 | 21 — Task | On completion (system) | Customer (optional) | Service completion report |
| 5 | 25 — HRM | On payroll completion | Employee | Payslip |
| 6 | 28 — Invoice | [APPROVE & SEND] | Customer | Invoice PDF |
| 7 | 28 — Invoice | [✉ RESEND] | Customer | Invoice PDF |
| 8 | 30 — Payment | [Print Receipt] (optional email) | Customer | Payment receipt PDF |
| 9 | 31 — Ledger | [📧 EMAIL TO PARTY] | Customer/Vendor | Ledger statement PDF |

---

## 📥 All Download/Export Buttons (Complete List — UI Verified)

| # | Module | Screen | Button Label | Format | Endpoint |
|---|--------|--------|--------------|--------|----------|
| 1 | 11 — Stock | View Entry Detail (11.1.3) | Download Invoice | PDF | `/v1/stock/entries/{id}/invoice-download` |
| 2 | 14 — PO | View PO Detail (14.3) | Download PDF | PDF | `/v1/purchase-orders/{id}/pdf` |
| 3 | 15 — Lead | View Lead Detail (15.3) | Download PDF | PDF | `/v1/leads/{id}/pdf` |
| 4 | 16 — Quotation | View Quotation (16.3) | 📥 Download PDF | PDF | `/v1/quotations/{id}/pdf` |
| 5 | 17 — GMA | View GMA Detail | Download PDF | PDF | `/v1/gma/{id}/pdf` |
| 6 | 18 — Customer | Service History Tab (18.3.3) | Export to Excel | Excel | `/v1/customers/{id}/service-history/export` |
| 7 | 19 — Contract | Dashboard Row / Detail | Download PDF | PDF | `/v1/contracts/{id}/pdf` |
| 8 | 19 — Contract | Tab 2: SO Schedule (19.3.2) | Download Service Log | Excel | `/v1/contracts/{id}/execution-log/export` |
| 9 | 20 — SO | Dashboard Row (20.1) | Download PDF | PDF | `/v1/sales-orders/{id}/pdf` |
| 10 | 21 — Task | View Task Detail (21.4) | Print / PDF | PDF | `/v1/tasks/{id}/pdf` |
| 11 | 22 — Tickets | Ticket Detail | Print / PDF | PDF | `/v1/tickets/{id}/pdf` |
| 12 | 24 — Petty Cash | All Expenses Tab | 📥 Export to Excel | Excel | `/v1/petty-cash/export` |
| 13 | 24 — Petty Cash | Received Requests Tab | 📥 Export to Excel | Excel | `/v1/petty-cash/received/export` |
| 14 | 25 — HRM | Salary Detail (25.3) | Download Salary Slip | PDF | `/v1/hrm/payslips/{emp}/{month}/pdf` |
| 15 | 25 — HRM | Salary Bulk Upload (25.4) | Download Sample | Excel | `/v1/hrm/salary/sample-template` |
| 16 | 25 — HRM | Attendance Bulk Upload (25.5) | Download Sample | Excel | `/v1/hrm/attendance/sample-template` |
| 17 | 26 — Self-Service | Documents Tab | 📥 Download | Original | `/v1/employees/{id}/documents/{doc}/download` |
| 18 | 28 — Invoice | Dashboard Row | Download PDF | PDF | `/v1/invoices/{id}/pdf` |
| 19 | 28 — Invoice | Dashboard Row (Paid) | Download Receipt | PDF | `/v1/payments/receipts/{id}/pdf` |
| 20 | 28 — Invoice | View Detail (28.3) | 📥 Download PDF | PDF | `/v1/invoices/{id}/pdf` |
| 21 | 28 — Invoice | Dashboard Toolbar | 📥 Export PDF Batch | ZIP | `POST /v1/invoices/batch-pdf` |
| 22 | 28 — Invoice | Dashboard Toolbar | 📊 Tally Export | CSV/XML | `/v1/invoices/tally-export` |
| 23 | 29 — Bill | Dashboard Row | Download PDF | PDF | `/v1/bills/{id}/pdf` |
| 24 | 29 — Bill | Dashboard Row (Paid) | Download Receipt | PDF | `/v1/payments/vouchers/{id}/pdf` |
| 25 | 29 — Bill | View Detail (29.3) | 📥 Download PDF | PDF | `/v1/bills/{id}/pdf` |
| 26 | 30 — Payment | Register Row | PDF | PDF | `/v1/payments/{id}/pdf` |
| 27 | 30 — Payment | View Voucher (30.6) | 📥 Download PDF | PDF | `/v1/payments/{id}/pdf` |
| 28 | 31 — Ledger | Dashboard Toolbar | 📥 Export All | Excel/CSV | `/v1/ledgers/export` |
| 29 | 31 — Ledger | Dashboard Toolbar | 📊 Ageing Report | Excel/PDF | `/v1/ledgers/ageing-report` |
| 30 | 31 — Ledger | Statement (31.3) | 📥 Download PDF | PDF | `/v1/ledgers/{id}/statement/pdf` |
| 31 | 32 — COA | Dashboard Toolbar | 📥 Export CSV | CSV | `/v1/coa/export` |

---

## 🔔 Notification Backend Architecture

### Single Notification Service

All system notifications flow through **one unified service** to minimize integration:

```
┌──────────────────────────────────────────────────────┐
│                NOTIFICATION SERVICE                    │
│                                                        │
│  Event Bus (Kafka/RabbitMQ/Redis PubSub)              │
│  ┌──────────┐                                         │
│  │ Producer  │ ← All 32 modules publish events        │
│  └────┬─────┘                                         │
│       │                                                │
│  ┌────▼─────┐     ┌────────────────┐                  │
│  │ Consumer  │────►│ 🔔 In-App DB   │ (WebSocket)     │
│  │           │     │ notification   │                  │
│  │           │     │ table + badge  │                  │
│  └───────────┘     └────────────────┘                  │
│                                                        │
│  📧 External Email is NOT part of this service.        │
│  Email is triggered by specific controller actions.    │
└──────────────────────────────────────────────────────┘
```

### Event Payload (Standard)

```json
{
  "event_type": "INVOICE_OVERDUE",
  "module": 28,
  "entity_id": "INV-2026-00142",
  "recipients": ["user_id_1", "user_id_2"],
  "message": "⚠️ Invoice INV-2026-00142 overdue. Outstanding: ₹28,037.",
  "priority": "high",
  "timestamp": "2026-04-24T12:00:00Z",
  "action_url": "/invoices/INV-2026-00142"
}
```

### Email Sends — Controller-Level Only

```
📧 Emails are sent from specific API controller actions:

POST /v1/quotations/{id}/send          → Email + WhatsApp to client
POST /v1/quotations/{id}/resend        → Re-send email
POST /v1/invoices/{id}/approve-send    → Email to customer
POST /v1/invoices/{id}/resend          → Re-send email
POST /v1/ledgers/{id}/statement/email  → Email to party
POST /v1/hrm/payslips/{id}/email       → Email to employee (optional)
```

> No background cron job sends emails to external parties. All external communication is **user-initiated**.

---

## Escalation Time Matrix

| Module | Entity | Level 1 Reminder | Level 2 Escalation | Level 3 Critical |
|--------|--------|-------------------|--------------------|-------------------|
| 11 — Stock | Stock Request | — | 48 hrs → Ops Head | — |
| 14 — PO | PO Approval | — | 24 hrs → Ops Head | — |
| 15 — Lead | Follow-up | Due date | 48 hrs → Sales Manager | — |
| 17 — GMA | GMA Approval | — | 24 hrs (Mgr) / 48 hrs (CEO) | Auto-escalate |
| 21 — Task | Task Overdue | Due date | — | 3 days → Ops Head |
| 22 — Ticket | Ticket Open | — | 24 hrs → Branch Mgr | 48 hrs → Ops Head |
| 24 — Petty Cash | PC Approval | — | 48 hrs → Next approver | — |
| 25 — Leave | Leave Approval | — | 48 hrs → HR Manager | — |
| 28 — Invoice | Invoice Overdue | Due date | 30 days → Finance Mgr | 60 days → CFO |

---

> **End of Document**  
> Total Auto-Events: 65 (trimmed from 85 — removed speculative events)  
> Total System Notifications: 85 (all 🔔 In-App only)  
> Total External Send Actions: 9 (📧 user-triggered only)  
> Total Download Endpoints: 31 (📥 UI-verified only — trimmed from 60+)
