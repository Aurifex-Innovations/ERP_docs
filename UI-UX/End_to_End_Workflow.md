# Seravion ERP — End-to-End Core Workflow

This document traces the complete lifecycle of a business transaction within the Seravion ERP system, specifically tailored for pest control service operations. It maps the journey of data from the initial lead capture, through contract and order creation, down to the mobile execution by the field technician, and back up to web-side review, invoicing, and final payment collection.

> **Reading Guide:** Each Phase below tracks a real-world stage. **Bold module numbers** (e.g., **Module 21**) are clickable references to the detailed UI/UX specs in `Module11-20.md` and `Module21-onwards.md`. The **Interconnected Modules** callout at the end of each phase lists every module that reads from or writes to the current phase, creating a quick cross-reference map.

---

## Phase 1: Pre-Sales, Customer Onboarding & Order Generation

### 1.1 Lead Capture & Qualification (Module 15)

| Step | Action | System Behaviour | Data Created |
| :--- | :--- | :--- | :--- |
| 1 | New inquiry arrives (inbound call, web form, referral) | Lead record created in **Module 15** (Lead Management) with status = `New` | Lead ID, Contact, Source, Service Interest |
| 2 | Sales team qualifies the lead | Lead progresses through pipeline: `New` → `Contacted` → `Qualified` → `Won` | Status history, Follow-up notes |
| 3 | Lead conversion decision | Upon `Won` status, system enables **"Convert to Customer"** action | Conversion trigger |

> **Note:** Module 17 is **GMA (Gross Margin Analysis)** — not Lead Management. The Lead pipeline lives in **Module 15**.

### 1.2 Customer Onboarding (Module 18)

| Step | Action | System Behaviour | Data Created |
| :--- | :--- | :--- | :--- |
| 1 | Convert Lead to Customer | System creates a new Customer Master from the lead data (auto-populates contact, company details) | Customer ID (CUST-XXXXX), Customer Type |
| 2 | Site/Branch Registration | Operations team registers physical service sites under the customer (e.g., "XYZ Hotel — City Center Branch") | Site ID, GPS Coordinates, Address, POC |
| 3 | Billing Configuration | Finance team sets up billing address, GSTIN, PAN, credit period, and payment terms | Billing profile linked to Customer Master |

**Customer Types:** `Contract` (recurring AMC) · `One-time` (ad-hoc service) · `Product` (product-only sales)

### 1.3 Gross Margin Analysis (Module 17)

Before any contract or quotation is finalized, the sales team builds a **GMA Sheet** to validate profitability.

| Step | Action | System Behaviour |
| :--- | :--- | :--- |
| 1 | Sales person creates GMA sheet | Selects Customer, Sites, Services, adds chemicals from **Module 10** (Product Master), technician costs, revisit charges |
| 2 | System calculates GM% | Auto-computes: `(Revenue − Total Cost) / Revenue × 100` |
| 3 | Three-tier approval | Based on GM%: Auto-approve (high margin) → Manager approval (medium) → Director approval (low margin) |
| 4 | GMA approved | Status = `Approved` — prerequisite for Contract/Quotation creation |

### 1.4 Contract Formulation (Module 19) — *Recurring Services Only*

| Step | Action | System Behaviour | Data Created |
| :--- | :--- | :--- | :--- |
| 1 | Create Contract from approved GMA | **80% of data auto-inherited** from GMA: customer, sites, services, pricing | Contract ID (CON-YYYY-XXXX) |
| 2 | Define SLA & Frequency | Manager sets service frequency (Monthly/Quarterly/Annual), total contract value, payment schedule | SLA terms, Billing schedule |
| 3 | Contract Activation | Status: `Draft` → `Active` upon approval | Active flag triggers SO auto-generation |

**Status Lifecycle:** `Draft` → `Active` → `Expiring Soon` (auto-flag 30 days before end) → `Expired` / `Terminated`

### 1.5 Sales Order (SO) Creation (Module 20)

The Sales Order is the **official operational and financial execution mandate**.

| Source | Trigger | SO Generation Mode |
| :--- | :--- | :--- |
| **Active Contract** (Module 19) | System cron runs based on payment schedule | **Auto-generated** per billing period (e.g., quarterly SO for Q2 Apr–Jun) |
| **Approved Quotation/GMA** (Module 16/17) | User manually creates from approved source | **Manual** — for one-time services |
| **Standalone** (Module 18 Customer) | User creates directly | **Manual** — for ad-hoc or product-only orders |

**SO Status at Creation:** `Draft` → Upon manager approval → `Open`

**What the SO Contains:**
- Customer ID, Site(s), Branch
- Service/Product Line Items (with HSN/SAC, Qty, Rate, Discount, Tax%)
- Total Value (including GST — CGST+SGST or IGST based on inter-state logic from **Module 9**)
- Linked Contract ID (if recurring)

> **🔗 Interconnected Modules (Phase 1):**
> - **Module 7** (Branch): Branch assignment for SO
> - **Module 9** (Tax): HSN/SAC codes, GST rate calculation
> - **Module 10** (Product): Chemical/product line items & pricing
> - **Module 12** (Services): Service definitions, pricing models (Fixed/Area-Based/Inspection/Custom)
> - **Module 15** (Lead): Source data for customer creation
> - **Module 16** (Quotation): One-time service SO source
> - **Module 17** (GMA): Profitability validation, data inheritance for contracts
> - **Module 18** (Customer): Customer master, sites, billing info
> - **Module 19** (Contract): Recurring SO auto-generation trigger

---

## Phase 2: Work Order / Task Scheduling (Module 21)

Once an SO reaches `Open` status, the Operations team takes over.

### 2.1 Task Generation

| Step | Action | System Behaviour |
| :--- | :--- | :--- |
| 1 | SO services broken into Tasks | Each service line of the SO becomes a schedulable **Task** (e.g., "Cockroach Control at Kitchen" → TASK-2026-001) |
| 2 | Dispatcher schedules | Uses the **Calendar Dashboard** (21.3) to pick date, time slot, and assign technician(s) |
| 3 | Conflict Check | System validates: same technician cannot have overlapping time slots (Role → Employee cascading filter from **Module 8**) |
| 4 | Primary Technician selection | From multi-selected employees, one is designated **Primary** |

### 2.2 Task Assignment Details

| Field | Source | Description |
| :--- | :--- | :--- |
| Task ID | Auto-generated | `TASK-YYYY-NNNN` (sequential per year) |
| Customer & Site | Module 18 | Customer name, site address, GPS coordinates, POC |
| Service(s) | Module 12 / SO line items | Service name, applicable areas |
| Scheduled Date/Time | Manual | Date + Start/End time window |
| Assigned Technicians | Module 8 (Employee) | Primary + Support technicians |
| Priority | Manual | Normal / Urgent / Critical |
| Special Instructions | SO / Dispatcher notes | Site-specific instructions |

**Task Status at Creation:** `Pending`
**SO Status:** Remains `Open`

### 2.3 Re-Task Flow (From Customer Support — Module 23)

If a customer raises a complaint after service completion:

1. Support team creates a **Ticket** (Module 23) linked to the original SO/Task
2. If re-service is needed, ticket is **"Converted to Task"** (23.5) → creates a new Task in Module 21 with `Task Type = Re-Task`
3. Re-Task maintains linkage to original SO and Ticket ID

> **🔗 Interconnected Modules (Phase 2):**
> - **Module 8** (Employee): Technician availability, role-based filtering
> - **Module 12** (Services): Service definitions mapped to task
> - **Module 18** (Customer): Site address, POC, GPS coordinates
> - **Module 20** (SO): Parent SO providing service line items
> - **Module 22** (Location): GPS coordinates for geo-fence validation
> - **Module 23** (Support): Re-Task creation from customer tickets

---

## Phase 3: Mobile Task Execution (Field Technician — 5-Step Flow)

The technician interacts directly with the assigned task via the **Mobile App** (`mobile_screen.md`), driving operational data back into the ERP in real-time.

### 3.1 Pre-Execution: Travel & Arrival

| Step | Technician Action (Mobile) | System Behaviour | Module Impact |
| :--- | :--- | :--- | :--- |
| **Travel Start** | Taps **"🚀 Start Travel"** (Screen 11) | GPS captured. Module 22 logs `Travelling` status. Button transforms to "Start Task" | Module 22: Travel log begins |
| **Navigation** | Views route on **Screen 12** (Navigation Map) | Real-time route, distance, ETA displayed. Option to open in Google/Apple Maps | Module 22: GPS polling active |
| **Arrival Check** | System evaluates **geo-fence** | Checks if device GPS is within **500m radius** of site coordinates (Module 18). Status indicator: 🔴 Outside / 🟢 Inside | Module 22: Geo-fence validation |
| **Confirm Arrival** | Taps **"☑️ Confirm Arrival"** (Screen 12) | Only enabled when inside geo-fence. Captures check-in timestamp. Task status → `In Progress` | Module 21: Status update |

### 3.2 Execution Flow (5 Steps)

#### Step 1: Before Task Photos (Screen 13)

| Action | Rules | Data Captured |
| :--- | :--- | :--- |
| Capture site condition photos | Max 5 images (Camera / Gallery) | Before-treatment photo evidence |
| Can be skipped | "Skip this step" link available | — |
| Proceed | Uploads images to storage, linked to Task ID | Image URIs attached to task profile |

#### Step 2: Service Execution & Material Log (Screen 14)

This is the **core data entry step** — critical integration with **Module 11** (Stock) and **Module 12** (Services).

| Action | Rules | Data Captured |
| :--- | :--- | :--- |
| Select Performed Areas | Multi-select chips (from Module 12 service config) | Specific areas treated (e.g., Kitchen, Cafeteria) |
| Select Treatment Methods | Multi-select chips (from Module 12) | Methods used (e.g., Gel Baiting, Spraying, Fogging) |
| Log Chemicals/Materials Used | Fetches from technician's **Virtual Bin / Van Stock** (Module 11) | Chemical name, Qty used, UoM, Batch No |
| Real-time stock validation | If `Qty Used > Available Stock` → **Inline Error**: "Insufficient stock. Available: XXXg" | Prevents over-deduction |
| Mark Service as Completed | Checkbox per service (for multi-service tasks) | Service-level completion flag |

**Stock Deduction Logic (Module 11):**
- At this step: Stock is **temporarily/virtually deducted** (pending final OTP verification)
- At Step 5 (Submit): Stock deduction **commits permanently** to Module 11 ledger

#### Step 3: Technician Observations (Screen 15)

| Category | Input Type | Data Captured | Web-Side Usage |
| :--- | :--- | :--- | :--- |
| **Structural Issues** | Dropdown (pre-defined) + Area (free text) | e.g., "Gap in Door — Kitchen entrance" | Sales team uses for upselling repair services |
| **Hygiene Issues** | Dropdown (pre-defined) + Area (free text) | e.g., "Water accumulation — Behind fridge" | Customer report includes recommendations |
| **Pest Activity** | Multi-checkbox + Intensity (High/Med/Low) | e.g., "Cockroaches (High), Ants (Low)" | Trend analysis for recurring contracts |
| **General Remarks** | Textarea (max 1000 chars) | Free-text notes for internal or customer record | Appears in Service Report PDF |

#### Step 4: After Task Photos (Screen 16)

| Action | Rules | Data Captured |
| :--- | :--- | :--- |
| Capture post-treatment photos | Max 5 images | After-treatment evidence, structural/hygiene issue photos |
| Can be skipped | "Skip this step" link available | — |
| Proceed | Uploads images to storage, linked to Task ID | Image URIs attached to task profile |

#### Step 5: Customer Verification & Sign-off (Screen 17)

| Action | Rules | Data Captured |
| :--- | :--- | :--- |
| **OTP Verification** | 4-digit OTP sent to customer's registered mobile (from Module 18) | Customer acknowledgment of service completion |
| OTP Validation | Must match sent OTP. 60-second cooldown for resend | Verified timestamp |
| **Customer Rating** | 1–5 Star rating (mandatory) | Service quality score |
| **Customer Remarks** | Free text (optional, max 500 chars) | Direct feedback |

**On "Submit Task" (OTP Verified):**

| System Action | Detail |
| :--- | :--- |
| Task Status → `Completed` | Permanent status update in Module 21 |
| Stock Committed | Material usage permanently deducted from Module 11 branch stock |
| Service Report PDF | Auto-generated, merging all data from Steps 1–5 |
| Travel Log Closed | Module 22 logs departure timestamp, calculates on-site duration |
| Email Notification | Service Report emailed to mapped contacts (via Module 18 contact config) |

### 3.3 Post-Execution: Service Report (Screen 18)

| Action | Description |
| :--- | :--- |
| **PDF Preview** | Technician views generated Service Report on mobile |
| **Download** | Downloads PDF to device |
| **Share** | Selects contacts (SO Contact, Contract Contact, Billing Contact from Module 18) and sends email with: Current Service Report PDF, Previous Service Logs (Excel), Previous Service Details, Invoice (if applicable) |

> **🔗 Interconnected Modules (Phase 3):**
> - **Module 8** (Employee): Assigned technician identity
> - **Module 11** (Stock): Virtual bin check → permanent deduction on submit
> - **Module 12** (Services): Treatment methods, performed areas config
> - **Module 18** (Customer): Site GPS for geo-fence, customer OTP contact, email contacts for report sharing
> - **Module 21** (Task): Task status lifecycle management
> - **Module 22** (Location): GPS tracking, geo-fencing, travel logs, on-site duration
> - **Module 25** (Leave/Attendance): Technician availability cross-check

---

## Phase 4: Web-Side Data Visibility & Sales Order Impact

Once the technician presses "Submit Task" on the mobile device, multiple downstream changes occur **instantaneously** on the Web Portal.

### 4.1 Automated Service Report Generation

| Component | Content Source | Description |
| :--- | :--- | :--- |
| Header | Module 20 (SO) + Module 18 (Customer) | Customer name, site, SO number, service date |
| Service Details | Screen 14 input | Areas treated, methods used, chemicals consumed |
| Technician Observations | Screen 15 input | Structural, Hygiene, Pest Activity findings |
| Photo Evidence | Screens 13 & 16 | Before/After treatment photos |
| Customer Sign-off | Screen 17 input | OTP verification timestamp, rating, remarks |
| Timestamps | Module 22 (Location) | Travel start, arrival, departure, total on-site time |

**Report Distribution:** Auto-emailed to Billing Contact, Contract Contact, and SO Contact (from Module 18 customer config). PDF is also available for download from the web portal.

### 4.2 Web-Side Visibility for Internal Teams

| Team | What They See | Source |
| :--- | :--- | :--- |
| **Operations/Dispatchers** | Task timeline (Travel Start → Arrive → Submit), live location replay | Module 21 dashboard + Module 22 travel logs |
| **Sales Team** | Structural & Hygiene observations — used to pitch add-on repair/deep-cleaning services | Module 21 "Technician Remarks" tab |
| **Finance Team** | Task completion flag → SO status update → Invoice readiness indicator | Module 20 SO status change |
| **Customer Support** | Task completion history, feedback score — context for future complaints | Module 23 can link to prior tasks |

### 4.3 Dynamic Sales Order (SO) Status Updates

The SO is intimately tied to the aggregate status of its underlying Tasks. Every task completion triggers a re-evaluation of the parent SO status.

| SO Status | Trigger Event | Condition |
| :--- | :--- | :--- |
| **`Draft`** | SO saved | Not yet approved by manager |
| **`Open`** | Manager approves SO | Ready for task generation and dispatch |
| **`In Progress`** | First task reaches `In Progress` | At least one technician has started a task |
| **`Partially Executed`** | First task reaches `Completed` | ≥1 task completed, but others still pending/in-progress |
| **`Fully Executed`** | All tasks reach `Completed` | Every task for the current SO billing cycle is done |
| **`Ready for Invoicing`** | Auto-set after `Fully Executed` | Flags Finance team to generate invoice |
| **`Invoiced`** | Invoice created in Module 28 | Invoice INV-XXXXX linked to this SO |
| **`Completed`** | Full payment received in Module 30 | Invoice marked `Paid`, SO ledger balance = zero |
| **`Cancelled`** | Manual abort | Admin/Manager cancels before execution begins |

> **Important — Module 20 Base Statuses vs. Extended Flow:**
> Module 20's UI filter shows: `Draft`, `Open`, `Fulfilled`, `Billed`, `Cancelled`. The extended statuses above (`In Progress`, `Partially Executed`, `Fully Executed`, `Ready for Invoicing`, `Invoiced`, `Completed`) represent the **granular operational tracking** driven by Module 21 task completion and Module 28/30 financial events. `Fulfilled` maps to `Fully Executed / Ready for Invoicing`, and `Billed` maps to `Invoiced`.

### 4.4 Key SO Status Transition Rules

1. **Task-to-SO Sync:** A single task completion immediately pushes an `Open` SO to `Partially Executed`. This provides real-time visibility to the web-side dispatcher.
2. **The "Invoiced" Barrier:** Once an SO is `Invoiced`, it remains in that state regardless of further service activity, until the **Payments Module (Mod 30)** confirms financial clearance.
3. **Financial Completion:** The SO only reaches its terminal `Completed` state when the Customer Ledger (Module 31) shows zero balance for the specific Invoice generated from it.
4. **Recurring Contracts:** For AMC contracts, each billing cycle produces a new SO. The Contract (Module 19) itself only becomes `Expired` when the contract end date passes — individual SO statuses are independent.

> **🔗 Interconnected Modules (Phase 4):**
> - **Module 18** (Customer): Email contacts for report distribution
> - **Module 20** (SO): Master transaction — status driven by task completion aggregate
> - **Module 21** (Task): Task completion triggers SO status re-evaluation
> - **Module 22** (Location): Travel logs and timestamps for service report
> - **Module 23** (Support): Task history feeds complaint context

---

## Phase 5: Financial Settlement (Invoicing, Bills & Payments)

### 5.1 Invoice Generation (Module 28)

Once an SO hits `Ready for Invoicing` (or based on predetermined billing schedules for AMCs):

| Step | Action | System Behaviour |
| :--- | :--- | :--- |
| 1 | Finance opens "Create Invoice" (28.2) | **From SO Mode:** Auto-populates customer, line items, pricing, taxes from the linked SO |
| 2 | Tax Calculation | System auto-determines: Intra-state → CGST+SGST, Inter-state → IGST (based on branch state vs. customer billing state from Module 9) |
| 3 | Review & Approve | Finance reviews line items, adjusts discount if needed |
| 4 | Save as Draft (optional) | NO ledger posting. Invoice editable |
| 5 | **Approve & Send** | **Triggers Ledger posting** (Module 31): `Dr Customer A/C (Sundry Debtor) → Cr Service Income` |

**Invoice Status Lifecycle:** `Draft` → `Sent` → `Partial` → `Paid` / `Overdue` / `Cancelled`

**Key Invoice Fields:**
| Field | Source |
| :--- | :--- |
| Invoice No | Auto-generated (INV-XXXXX, sequential per branch) |
| Customer & GSTIN | Module 18 (Customer Master) |
| Line Items | Module 20 SO line items (Service from Module 12, Product from Module 10) |
| HSN/SAC + Tax% | Module 9 (Tax Config) |
| Due Date | Invoice Date + Credit Period (from Contract/Customer terms) |
| SO Reference | Linked SO-YYYY-NNNN |

**System Behaviour on Invoice Events:**

| Event | System Action |
| :--- | :--- |
| Approve & Send | Posts to Customer Ledger (Module 31). Status → `Sent`. SO Status → `Invoiced` |
| Due Date passes (unpaid) | Status → `Overdue`. Notification sent to Finance team |
| Credit Note issued (28.5) | Original invoice amount reduced. Ledger adjusted |

### 5.2 Bills (Module 29) — *Vendor/Purchase Side*

While invoices handle the **revenue side** (money owed to the company), Bills handle the **cost side** (money owed by the company to vendors/suppliers).

| Trigger | Example | System Behaviour |
| :--- | :--- | :--- |
| Chemical purchase from vendor | Purchase Order for Maxforce Gel | Bill created, linked to vendor (Module 14) |
| Confirm Bill | Finance approves | Posts to Vendor Ledger (Module 31): `Dr Purchase A/C → Cr Vendor A/C (Sundry Creditor)` |

**Bill Status Lifecycle:** `Draft` → `Confirmed` → `Partial` → `Paid` / `Overdue`

### 5.3 Payment Processing (Module 30)

#### 5.3.1 Receipt Entry (Money IN — from Customer)

When a customer pays against an invoice:

| Step | Action | System Behaviour |
| :--- | :--- | :--- |
| 1 | Select Customer | Fetches all pending invoices (Pending/Partial/Overdue) and current balance from Customer Ledger (Module 31) |
| 2 | Enter Payment Info | Mode (Cash/Bank/UPI/Cheque/Card), Amount, Reference/UTR number |
| 3 | Advance Check | If customer has existing advance ("On Account" balance), offers **Advance Adjustment** — auto-applies to oldest invoice (FIFO) |
| 4 | Invoice Allocation | Manually or auto-allocate received amount against specific invoices (FIFO default) |
| 5 | Save | Creates voucher RCP-XXXX. Posts to Ledger: `Dr Bank/Cash A/C → Cr Customer A/C` |

**Deep-link:** When opened via `[+ Record Payment]` from Module 28.3 (Invoice Detail), the Customer and Invoice are **pre-selected** and locked.

#### 5.3.2 Conditional Allocation Outcomes

```
                    Amount Received
                         │
          ┌──────────────┼──────────────┐
          │              │              │
          ▼              ▼              ▼
    Amount == Pending   Amount < Pending   Amount > Pending
          │              │                     │
          ▼              ▼                     ▼
    Invoice → PAID    Settlement Choice     ADVANCE
                      │            │       (On Account)
                      ▼            ▼
                Keep Open     Settle & Close
                Invoice →     Auto-generate
                PARTIAL       Credit Note (CN)
                              in Module 28.5
                              Invoice → PAID
```

| Condition | Action | Invoice Impact | SO Impact |
| :--- | :--- | :--- | :--- |
| **Full Payment** (Amount = Invoice Total) | Direct allocation | Invoice → `Paid` | SO → `Completed` |
| **Partial Payment** (Amount < Invoice Total) + **"Keep Open"** | Allocate what's received | Invoice → `Partial`, Pending reduced | SO remains `Invoiced` |
| **Partial Payment** + **"Settle & Close"** | Auto-generate Credit Note (CN-XXXXX) for shortfall | Invoice → `Paid` (via CN adjustment) | SO → `Completed` |
| **Overpayment** (Amount > Total Pending) | Excess saved as "On Account" in Customer Ledger | All checked invoices → `Paid` | SO → `Completed` (for fully allocated invoices) |
| **Advance Payment** (No invoice exists) | Entire amount → "On Account" in Customer Ledger | No invoice affected yet | No SO impact yet |

#### 5.3.3 Payment Entry (Money OUT — to Vendor)

Mirror of Receipt Entry but for vendor bills:

| Condition | Action |
| :--- | :--- |
| Full Payment to Vendor | Bill → `Paid` |
| Partial Payment + Keep Open | Bill → `Partial` |
| Partial Payment + Settle & Close | Auto-generate Debit Note (DN-XXXXX) in Module 29.5 |
| Overpayment | Excess → Vendor Advance in Vendor Ledger |

#### 5.3.4 Ledger Posting Summary (Module 31)

Every financial event from Modules 28, 29, and 30 posts to the Ledger automatically:

| Source Event | Ledger Entry |
| :--- | :--- |
| Invoice Approved & Sent (28) | `Dr Customer (Sundry Debtor) → Cr Income A/C` |
| Bill Confirmed (29) | `Dr Purchase/Expense A/C → Cr Vendor (Sundry Creditor)` |
| Receipt Saved (30.2) | `Dr Bank/Cash → Cr Customer` |
| Payment Saved (30.3) | `Dr Vendor → Cr Bank/Cash` |
| Credit Note Auto-generated (28.5) | `Dr Income/Discount → Cr Customer` (reduces receivable) |
| Debit Note Auto-generated (29.5) | `Dr Vendor → Cr Purchase/Expense` (reduces payable) |

> **Posting Rule:** `Draft` documents **never** post to the Ledger. Only finalized/approved documents trigger ledger entries. If a party ledger does not exist, the system raises a validation error — it does **not** create the ledger from a document context.

> **🔗 Interconnected Modules (Phase 5):**
> - **Module 9** (Tax): GST calculation (CGST+SGST vs IGST), HSN/SAC validation
> - **Module 10** (Product): Product line items for product sale invoices
> - **Module 12** (Services): Service line items, pricing model for invoice lines
> - **Module 14** (Vendor): Vendor details for bills, TDS config
> - **Module 18** (Customer): Billing address, GSTIN, contact for invoice
> - **Module 20** (SO): SO → Invoice data binding, SO status update on payment
> - **Module 28** (Invoice): Creates receivable, links to SO/Task
> - **Module 29** (Bills): Creates payable, links to vendor/PO
> - **Module 30** (Payments): Allocates cash against invoices/bills
> - **Module 31** (Ledger): Central posting engine — all financial events feed here
> - **Module 32** (COA): Account classification (Debtors/Creditors/Bank/Income/Expense)
> - **Module 33** (Reports): P&L, Balance Sheet, GST Returns, Ageing, Cash Flow

---

## Phase 6: Customer Perspective Lifecycle

From the moment a Sales Order is generated, the customer follows this digital journey:

| Stage | Trigger | Customer Experience |
| :--- | :--- | :--- |
| **1. Schedule Alert** | Task Scheduled in Module 21 | Receives Email/SMS: "Your service for [Site] is scheduled on [Date] at [Time]." |
| **2. Active Tracking** | Tech taps "Start Travel" | Receives Notification: "Technician [Name] is on the way." (Optional: Live Location tracking link) |
| **3. Arrival** | Tech confirms arrival (geo-fence passed) | Receives SMS: "Technician has arrived at your site." |
| **4. On-Site Interaction** | Execution Phase (Steps 1–4) | Customer observes service. Discusses structural/hygiene issues with technician |
| **5. Digital Sign-off** | Verification Phase (Step 5) | Customer receives **4-digit OTP** on their phone. Provides OTP + Rating (1–5 Stars) to technician |
| **6. Instant Report** | Tech taps "Submit" | Receives Email: "Service Report for [Task ID] attached." Includes Pre/Post photos and Observation notes |
| **7. Final Billing** | Invoice Generated (Module 28) | Receives Email: "Invoice [ID] for your recent service is ready. [Pay Now Link]." |
| **8. Payment Confirmation** | Payment Settled (Module 30) | Receives Payment Receipt: "Payment of ₹[Amount] received. Thank you." |

---

## Phase 7: Reverse Data Flow — Mobile Action ➔ SO Status (Conditional Logic)

This section details how specific technician actions on the field conditionally trigger status updates back to the Sales Order (SO), enabling real-time operational visibility.

| Technician Action (Mobile) | Primary Condition Checked | Resultant SO Status Update |
| :--- | :--- | :--- |
| **Start Travel** | Is this the first task activity for the SO? | **No Change** (Remains `Open` / `Scheduled`) |
| **Start Task** (Confirm Arrival) | Is this the **FIRST** task being started for the SO? | `Open` → **`In Progress`** |
| **Start Task** (Confirm Arrival) | Are other tasks for this SO already `In Progress`? | **No Change** (Remains `In Progress`) |
| **Submit Task** (OTP Verified) | Is this the **FIRST** task completed for the SO? | `In Progress` → **`Partially Executed`** |
| **Submit Task** (OTP Verified) | Are there **PENDING** tasks still remaining for this SO? | **Remains `Partially Executed`** |
| **Submit Task** (OTP Verified) | Is this the **LAST** pending task for the SO cycle? | `Partially Executed` → **`Fully Executed`** |
| **All Tasks Complete** (Auto) | Has the SO cycle reached execution completion? | **`Ready for Invoicing`** (automated flag set by system) |
| **Finance: Create Invoice** | Invoice generated and sent (Module 28) | **`Invoiced`** |
| **Finance: Record Full Payment** | Full payment allocated in Module 30 | **`Completed`** (terminal state) |

---

## Phase 8: Complete Master Data Flow Diagram

```
                                    ┌─────────────────────────┐
                                    │    LEAD (Module 15)      │
                                    │    Status: Won           │
                                    └────────────┬────────────┘
                                                 │ Convert
                                                 ▼
                                    ┌─────────────────────────┐
                                    │  CUSTOMER (Module 18)    │
                                    │  + Sites, Billing, GPS   │
                                    └────────────┬────────────┘
                                                 │
                          ┌──────────────────────┼──────────────────────┐
                          │                      │                      │
                          ▼                      ▼                      ▼
                ┌──────────────────┐   ┌──────────────────┐   ┌──────────────────┐
                │ GMA (Mod 17)     │   │ Quotation (Mod16)│   │ Direct / Standalone│
                │ GM% Calculation  │   │ One-time pricing │   │ Ad-hoc order      │
                └────────┬─────────┘   └────────┬─────────┘   └────────┬─────────┘
                         │                      │                      │
                         ▼                      │                      │
                ┌──────────────────┐            │                      │
                │ CONTRACT (Mod 19)│            │                      │
                │ Recurring / AMC  │            │                      │
                └────────┬─────────┘            │                      │
                         │ Auto-gen             │ Manual               │ Manual
                         └──────────────────────┼──────────────────────┘
                                                │
                                                ▼
                                    ┌─────────────────────────┐
                                    │  SALES ORDER (Module 20) │
                                    │  Status: Draft → Open    │
                                    └────────────┬────────────┘
                                                 │
                                                 ▼
                                    ┌─────────────────────────┐
                                    │  TASK (Module 21)         │
                                    │  Status: Pending          │
                                    └────────────┬────────────┘
                                                 │
                        ┌────────────────────────┼────────────────────────┐
                        │                        │                        │
                        ▼                        ▼                        ▼
               ┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
               │ MOBILE APP       │     │ WEB PORTAL       │     │ LOCATION         │
               │ Technician       │     │ Dispatchers      │     │ (Module 22)      │
               │ Execution Flow   │     │ Live Monitoring  │     │ GPS + Geo-Fence  │
               └────────┬────────┘     └─────────────────┘     └─────────────────┘
                        │
     ┌──────────────────┼──────────────────┐
     │                  │                  │
     ▼                  ▼                  ▼
┌──────────┐     ┌──────────┐      ┌──────────┐
│ Start     │     │ Execute  │      │ Submit   │
│ Travel    │     │ Service  │      │ Task     │
│           │     │ Log Mat  │      │ OTP ✓    │
│ Mod 22    │     │ Mod 11   │      │          │
│ GPS Log   │     │ Stock    │      │ Status:  │
└─────┬─────┘     └────┬─────┘      │ Completed│
      │                │            └─────┬────┘
      │                │                  │
      │                │     ┌────────────┼─────────────────┐
      │                │     │            │                 │
      │                │     ▼            ▼                 ▼
      │                │  ┌────────┐  ┌────────────┐  ┌──────────────┐
      │                │  │Service │  │Stock       │  │ SO STATUS    │
      │                │  │Report  │  │Deduction   │  │ UPDATE       │
      │                │  │PDF Gen │  │Permanent   │  │ (Conditional)│
      │                │  │Email   │  │Mod 11      │  └──────┬───────┘
      │                │  └────────┘  └────────────┘         │
      │                │                                     │
      │                │          ┌──────────────────────────┘
      │                │          │
      │                │          ▼
      │                │  ┌─────────────────────────────────────────┐
      │                │  │  SO Ready for Invoicing?                 │
      │                │  │  (All tasks completed for this cycle)    │
      │                │  └──────────────────┬──────────────────────┘
      │                │                     │ YES
      │                │                     ▼
      │                │  ┌─────────────────────────────────────────┐
      │                │  │  INVOICE (Module 28)                     │
      │                │  │  Finance creates → Approve & Send        │
      │                │  │  Ledger: Dr Customer / Cr Income         │
      │                │  └──────────────────┬──────────────────────┘
      │                │                     │
      │                │                     ▼
      │                │  ┌─────────────────────────────────────────┐
      │                │  │  PAYMENT (Module 30)                     │
      │                │  │  Customer pays → Receipt allocated       │
      │                │  │  Ledger: Dr Bank / Cr Customer           │
      │                │  └──────────────────┬──────────────────────┘
      │                │                     │
      │                │          ┌──────────┼──────────┐
      │                │          │          │          │
      │                │          ▼          ▼          ▼
      │                │     Full Pay    Partial    Overpay
      │                │     Inv=Paid    Inv=Part   Advance
      │                │     SO=Done     SO=Inv'd   (On Acct)
      │                │                     │
      │                │                     ▼
      │                │              ┌─────────────┐
      │                │              │ LEDGER       │
      │                │              │ (Module 31)  │
      │                │              │ Zero Balance │
      │                │              │ = SO Complete│
      │                │              └─────────────┘
      │                │
      └────────────────┘
```

---

## Phase 9: Module Interconnection Summary (Happy Flow Cross-Reference)

The table below maps **every module touched** in the happy-flow lifecycle, in the order they are activated:

| # | Module | Role in Happy Flow | Writes To | Reads From |
| :--- | :--- | :--- | :--- | :--- |
| 1 | **Module 15** (Lead) | Captures initial inquiry | Module 18 (Customer conversion) | — |
| 2 | **Module 18** (Customer) | Central customer & site repository | Module 17, 19, 20, 21, 28, 30, 31 | Module 15 (lead data) |
| 3 | **Module 17** (GMA) | Validates profitability of deal | Module 19 (data inheritance) | Module 10 (products), Module 12 (services), Module 18 (customer/sites) |
| 4 | **Module 19** (Contract) | Defines recurring service terms | Module 20 (auto-generates SOs per billing cycle) | Module 17 (approved GMA), Module 18 (customer) |
| 5 | **Module 20** (Sales Order) | Execution mandate + financial boundary | Module 21 (task generation), Module 28 (invoice source) | Module 19 (contract), Module 17/16 (one-time source), Module 18 (customer) |
| 6 | **Module 21** (Task) | Granular work unit for field execution | Module 22 (location tracking), Module 11 (stock deduction), Module 20 (SO status) | Module 20 (SO line items), Module 18 (site/POC), Module 8 (technician) |
| 7 | **Module 22** (Location) | GPS tracking, geo-fencing, travel logs | Module 21 (arrival validation) | Module 18 (site coordinates), Module 8 (technician device) |
| 8 | **Module 11** (Stock) | Chemical/material inventory management | Module 21 (stock availability for service), Ledger | Module 10 (product master), Module 21 (usage log) |
| 9 | **Module 12** (Services) | Service definitions, pricing, treatment methods | Module 20 (SO line items), Module 21 (task service config) | Module 9 (tax/HSN) |
| 10 | **Module 28** (Invoice) | Accounts Receivable — customer billing | Module 31 (ledger posting on approve/send), Module 20 (SO status → Invoiced) | Module 20 (SO data), Module 18 (customer billing), Module 9 (tax) |
| 11 | **Module 30** (Payments) | Cash/Bank transaction recording & allocation | Module 31 (ledger posting), Module 28 (invoice status → Paid), Module 20 (SO → Completed) | Module 28 (pending invoices), Module 31 (balance), Module 18 (customer) |
| 12 | **Module 31** (Ledger) | Central account book ("Bahi Khata") | Module 33 (Reports input) | Module 28, 29, 30 (postings) |
| 13 | **Module 33** (Reports) | Financial & operational reporting | — (output only) | Module 31, 32, 28, 29, 30, 9 |

### Supporting Modules (Always Active)

| Module | Role |
| :--- | :--- |
| **Module 7** (Branch) | Branch context for all transactions |
| **Module 8** (Employee) | Technician data, role-based assignment |
| **Module 9** (Tax) | GST rates, HSN/SAC codes |
| **Module 10** (Product) | Chemical/product master data |
| **Module 14** (Vendor) | Vendor data for bills & purchases |
| **Module 23** (Support) | Customer complaints → Re-task generation |
| **Module 25** (Leave/Attendance) | Technician availability for scheduling |
| **Module 27** (User Profile) | Employee self-service (mobile profile) |
| **Module 32** (COA) | Account classification for ledger postings |

---

## Phase 10: Edge Cases & Exception Flows

| Scenario | System Handling |
| :--- | :--- |
| **Technician outside geo-fence** | "Confirm Arrival" button disabled. Toast: "Cannot confirm arrival. You are outside the allowed radius." |
| **Insufficient stock during service** | Inline error on Screen 14: "Insufficient stock in your inventory. Available: XXXg." Blocks proceed |
| **Wrong OTP entered** | Validation fails. Tech must retry. OTP resend available after 60s cooldown |
| **Task reschedule needed** | Dispatcher uses 21.7 (Reschedule/Reassign). Reason required. Audit trail maintained |
| **Customer complaint after service** | Module 23 ticket created → Optional "Convert to Task" creates a **Re-Task** linked to original SO |
| **Partial payment → Keep Open** | Invoice stays `Partial`. SO stays `Invoiced`. Balance reminder sent to customer |
| **Partial payment → Settle & Close** | Credit Note auto-generated (Module 28.5). Invoice → `Paid`. SO → `Completed` |
| **Advance payment (no invoice)** | Amount sits as "On Account" in Customer Ledger. Auto-adjusted on next invoice creation |
| **Invoice Draft deleted** | Only `Draft` invoices can be deleted. `Sent` invoices require Credit Note for reversal |
| **Party ledger missing** | System raises validation error on posting attempt — does NOT auto-create ledger from document context |
| **SO cancelled** | Only if `Draft`/`Open` status AND no tasks have started execution. Reason + remarks required |
| **Contract expiration** | Contract auto-flags `Expiring Soon` 30 days before end date. Expired contracts stop auto-generating SOs |
