# ERP System Design: Pest Control Service Management
## Core Workflow: Customer → Contracts → Sales Orders

---

# 🎯 MODULE 18: CUSTOMER MANAGEMENT

**Description:** Master module acting as the central repository for all onboarded clients. Converts approved leads into revenue-generating entities. Provides a "360-degree view" of a customer's lifetime relationship spanning contracts, active sites, and service history.

### 18.1 Customer Master List View
**Description:** A data table displaying all customers.
* **Filters:** Customer Type (Commercial / Residential), Status (Active / Inactive), Branch.
* **Columns:** Customer ID, Name, Primary Contact, Total Sites, Active Contracts, Status.
* **Actions:** [View], [Edit], [Deactivate / Delete].
* **Top Button:** `[+ Add Customer]`

### 18.2 Add Customer Form
**Description:** Form to register a new customer manually or convert from a Lead ID.
* **Section 1 (Source):** Import from Lead (Auto-fill) or Manual Entry.
* **Section 2 (Entity Info):** Company/Individual Name, Industry Type, PAN, GST Details.
* **Section 3 (Billing & Contact):** Primary billing address, Finance/Accounts point of contact.
* **Section 4 (Primary Site):** Captures the first physical service location (Address, SQFT, Category).

### 18.3 View Customer Details
**Description:** A unified, read-only dashboard for a specific customer, split into logical tabs.

* **18.3.1 Tab 1: Basic Details & Sites**
  * **Profile:** Core entity details, tax IDs, LTV (Lifetime Value).
  * **Sites Grid:** List of all physical locations attached to this customer (Site ID, Address, Area, Sub-Category).

* **18.3.2 Tab 2: Contract Logs**
  * **Contracts Grid:** List of all historical and active agreements.
  * **Columns:** Contract ID, Start Date, End Date, Value, Status (Active / Expired / Terminated).

* **18.3.3 Tab 3: Sales Orders & Service History**
  * **Order & Service Grid:** Recent execution mandates and their real-time service tracking.
  * **Columns:** SO Number, SO Date, Linked Contract, Total Value, SO Status, Service Status (Scheduled / In Progress / Completed / Pending).

### 18.4 Edit Customer Form
**Description:** Interface to update master data.
* **Fields:** Pre-filled fields from 18.2. Users can modify the billing address, contact persons, or add brand-new sites.
* **Audit Rule:** Changes here trigger a master data audit log.

### 18.5 Delete (Deactivate) Customer
**Description:** Soft-delete mechanism.
* **Rule:** A customer cannot be hard-deleted. They are marked as "Inactive".
* **Validation:** Fails if the customer has an "Active" Contract or "Open" Sales Order. Requires a mandatory "Reason for Deactivation" remark.

---

# 🎯 MODULE 19: CONTRACT MANAGEMENT

**Description:** Transactional module handling the legal and service-level agreements (SLA) for recurring (AMC) services. Uses the approved GMA (Module 17) to lock in commercial terms.

### 19.1 Contract Master List View
**Description:** A ledger of all contracts.
* **Filters:** Status (Draft, Active, Expiring Soon, Terminated, Expired), Date Range.
* **Columns:** Contract ID, Customer Name, GMA ID, Contract Value, Start/End Date, Status.
* **Actions:** [View], [Edit / Amend], [Terminate].
* **Top Button:** `[+ Create Contract]`

### 19.2 Add Contract Form
**Description:** Form to formalize an agreement, inheriting 80% of data directly from an approved GMA.
* **Section 1 (Source Selection):** Select Approved GMA ID (Auto-populates Customer, Sites, Services, Cost).
* **Section 2 (Contract Terms):** Start Date, End Date, Contract Duration (e.g., 1 Year), Total Sale Value.
* **Section 3 (Service Schedule):** Configuration of service delivery (Frequency, Specific Service Days, Assigned Technician Group).
* **Section 4 (Payment & Commercial Terms):** Dynamic Payment Schedule (e.g., 100% Advance, Quarterly Post-paid, Milestone-based), Invoicing Frequency, Legal SLA remarks.

### 19.3 View Contract Details
**Description:** Read-only breakdown of the active agreement's terms.

* **19.3.1 Tab 1: Contract Terms, Scope & Sites**
  * Header: Contract ID, Customer Name, Validity Timeline, Status Badge.
  * Commercials: Total Value, Dynamic Payment Schedule, SLA notes.
  * **Sites & Services Grid:** Displays physical locations covered, respective pest types, and scheduled frequency.

* **19.3.2 Tab 2: Sales Order Schedule (Billing Log)**
  * **Grid:** Shows auto-generated and manual SOs linked to this contract to track fulfillment vs. agreement.

### 19.4 Edit / Amend Contract Form
**Description:** Interface to handle mid-cycle changes.
* **Fields:** Allows adding a new site to an active contract, upgrading the pest service, or adjusting value (which may trigger a re-approval workflow).

### 19.5 Delete (Terminate) Contract
**Description:** Early closure mechanism.
* **Fields:** Prompts for "Effective Closure Date" and "Reason Code" (e.g., Customer Relocated, Payment Default). Changes status to "Terminated" and halts future SO generation.

---

# 🎯 MODULE 20: SALES ORDER (SO) MANAGEMENT

**Description:** The official financial and operational document that confirms revenue boundaries and acts as a trigger to the Operations team to dispatch technicians for a job.

### 20.1 Sales Order Master List View
**Description:** Dashboard of all execution mandates.
* **Filters:** Order Type (One-Time Service / AMC Contract / Product Sale), Status (Draft, Open, Fulfilled, Billed, Cancelled), Branch.
* **Columns:** SO Number, Customer, Order Type, Total Value, SO Date, Status.
* **Actions:** [View], [Edit], [Cancel].
* **Top Button:** `[+ Generate Sales Order]`

### 20.2 Add / Generate Sales Order Form
**Description:** Interface to generate an SO. Can be done automatically via cron (for AMC) or manually formatted here.
* **Section 1 (Order Source & Type):** Select Type (Service AMC / One-Time Service / Product Sale). 
  * "From Active Contract" (AMC), "From Approved Quotation/GMA" (One-Time), or Standalone Product Sale. Customer details auto-fetch.
* **Section 2 (Line Items):** 
  * *For Services:* Auto-fetched services (e.g., Cockroach Treatment - Site A). Displays SQFT, Quantity (Visits), Unit Price, Taxes.
  * *For Products:* Select Products from Product Master (Module 10). Displays Qty, UOM, Unit Price, Taxes.
* **Section 3 (Delivery / Execution Terms):** Operational notes (e.g., "Weekend service only" or "Dispatch Product to Site B").

### 20.3 View Sales Order Details
**Description:** Clear, read-only layout detailing exactly what needs to be billed and executed.

* **20.3.1 Tab 1: SO Summary & Line Items**
  * Header: SO Number, Date, Customer info, Order Type.
  * Line Item Grid: Services/Products breakdown with HSN/SAC codes, Qty, Tax, and Final Amount.

* **20.3.2 Tab 2: Execution & Delivery Status**
  * **Grid:** Details the downstream release. Shows if Job Cards (for services) or Delivery Challans (for products) have been generated against this specific SO yet.

### 20.4 Edit Sales Order Form
**Description:** Allows modification of an SO before it is released to Operations.
* **Validation:** Fails entirely if the Status is "Fulfilled" or if Job Cards have already been printed. 
* **Fields:** Can modify line item quantities, substitute chemicals if allowed, or apply distinct discounts.

### 20.5 Delete (Cancel) Sales Order
**Description:** Mechanism to void the order.
* **Fields:** Select SO → Input Cancellation Reason (Dropdown + Free text).
* **Validation:** System blocks cancellation if execution has already begun or if the SO has been pushed to Invoicing. 

---


### End of Workflow Context
Once a **Sales Order (Module 20)** reaches "Open" status, the baton is passed from the Commercial team to the **Operations & Dispatch team**. The next steps in the ERP lifecycle involve consuming this SO to generate job cards, routing technicians, and issuing chemicals from inventory.
