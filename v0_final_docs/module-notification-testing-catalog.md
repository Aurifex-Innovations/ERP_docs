# Module Notification Catalog — Web, Tech App & Customer App Testing

Complete reference for **where notifications are produced**, **what message is sent**, **who receives them**, and **how to verify** on each client.

---

## 1. Platform overview

### Delivery channels


| Channel         | Client                      | Transport                   | Recipient identity                    |
| --------------- | --------------------------- | --------------------------- | ------------------------------------- |
| `WEB`           | Office web (bell icon)      | REST feed + STOMP WebSocket | `users.id` (employee)                 |
| `TECH_PUSH`     | Technician / field app      | FCM + optional in-app feed  | `users.id` (`APPLICATION_USER`)       |
| `CUSTOMER_PUSH` | Customer / Site Contact app | FCM + REST feed             | `customers.id` or site-contact mobile |


- Employee events use `publishToEmployees` → `WEB` + `TECH_PUSH`
- Customer/site events use `publishToPortal` → `CUSTOMER_PUSH` only
- Legacy `publish(...)` also targets `WEB` + `TECH_PUSH` (same as employees)

Pipeline: `NotificationPublisher` → DB (`notifications` + recipients) → Redis queue → `NotificationQueueConsumer` → WebSocket / FCM.

Retention: **30 days** (`NotificationCleanupScheduler`).

### Read APIs (verify after action)


| App                     | Register device                                 | List feed                                   | Unread count                                             | Mark read                                              |
| ----------------------- | ----------------------------------------------- | ------------------------------------------- | -------------------------------------------------------- | ------------------------------------------------------ |
| **Web / employee**      | N/A (WebSocket)                                 | `GET /api/v1/notifications`                 | `GET /api/v1/notifications/unread-count`                 | `POST /api/v1/notifications/{id}/read`                 |
| **Tech app**            | `POST /api/v1/mobile/devices/register`          | Same employee APIs as web                   | Same                                                     | Same                                                   |
| **Customer / site app** | `POST /api/v1/mobile/customer/devices/register` | `GET /api/v1/mobile/customer/notifications` | `GET /api/v1/mobile/customer/notifications/unread-count` | `POST /api/v1/mobile/customer/notifications/{id}/read` |




### Realtime (web + tech)

- WebSocket endpoint: `/ws` (STOMP)
- Subscribe: `/topic/notifications/{userId}`
- See [frontend-integration-react.md](./frontend-integration-react.md)



### Customer / site mobile

- Full API guide: [customer-site-push-notifications-mobile-guide.md](../mobile/customer-site-push-notifications-mobile-guide.md)
- Auth: OTP via `/api/v1/mobile/auth/**`
- **Do not** use technician device register on customer app



### How recipients are resolved (use this to pick a test user)

Two mechanisms. The **Recipients** column in each module table tells you which one applies.

**A. Permission matrix** (most modules)

| Query | Who gets the bell | How to test |
| ----- | ----------------- | ----------- |
| `findUserIdsByPermissions(perms)` | Any **ACTIVE** user who has **any** listed permission on their user or role. **All CEOs** are always included. | Log in as a non-CEO with that permission, **or** as CEO. |
| `findUserIdsByBranchAndPermissions(branchId, perms)` | Same, but the user must also be **assigned to that branch**. CEO is included **only if that CEO is assigned to the branch**. | Assign the test user to the **same branch** as the record. A CEO with no branch on that record will **not** get it. |

Skip publish when the resolved list is empty.

**B. Role-name groups** (`NotificationRecipientResolver`) — **not used** for live catalog events anymore (kept in code for merge/helpers). Testers should use **permissions**, not role names.

Exception: the UI still lets the actor **pick** recipient roles/users for hiring submit, GMA submit, petty-cash submit, and Branch-Direct PO.


Portal recipients (`PortalNotificationHelper`):

- **CUSTOMER** → OTP login with the customer's registered phone (`customers.id`)
- **SITE_CONTACT** → OTP login with the site / task `siteContactMobile`



### CEO test helper

`POST /api/v1/notifications/internal/publish` (CEO role) — smoke-test bell/FCM without running full business flow.

---



## 2. How to test per client



### Web (office users)

1. Log in as a user from the event's **Recipients** column (permission **or** role). For **branch-scoped** events, that user must be assigned to the record's branch.
2. Perform the **trigger action** in the UI (or API). Prefer a **second** user to trigger so you are not only the actor.
3. Check bell: `GET /api/v1/notifications?unreadOnly=true`
4. Optional: WebSocket connected → notification appears without refresh.
5. Confirm `eventType`, `title`, `message`, `actionUrl` in feed item.



### Tech app (field staff / technicians)

1. Log in as `APPLICATION_USER` with FCM token registered (`POST /api/v1/mobile/devices/register`).
2. Perform action **or** get assigned to a task.
3. Verify:
  - Push notification on device (foreground + background)
  - Employee feed: `GET /api/v1/notifications` (same as web)
4. Task-specific: assigned tech gets `TASK_ASSIGNED`; branch users with `TASK_MANAGEMENT_ADD` / `EDIT` get completion / selfie.



### Customer / site app

1. OTP login (`CUSTOMER` or `SITE_CONTACT`).
2. Register FCM: `POST /api/v1/mobile/customer/devices/register`.
3. Trigger backend action (contract activate, visit schedule, invoice approve, payment receipt, etc.).
4. Verify:
  - Push on device
  - `GET /api/v1/mobile/customer/notifications`
5. Login as **customer phone** vs **site contact mobile** to test recipient split.

---



## 3. Module catalog

**Legend:** Channels = `W` (WEB), `T` (TECH_PUSH), `C` (CUSTOMER_PUSH)

**How to read Recipients (for testing)**

- **Branch `PERM`** — log in as an ACTIVE user **assigned to the record's branch** who has that permission. A CEO gets it only if assigned to that branch.
- **`PERM` (no Branch)** — log in as any ACTIVE user with that permission. **All CEOs** also receive it.
- **Named person** (assignee, requester, creator, reporting manager, preparedBy) — log in as that exact user.
- **Selected users / roles** — only where the UI lets the actor pick recipients (hiring, GMA submit, petty-cash submit, Branch-Direct PO).
- **Customer portal** — OTP login with the customer phone or site-contact mobile, then register FCM.

---



### Module 2 — Subscription (`moduleNo: 2`)


| Event                   | Trigger                                | Title                      | Message (pattern)                               | Recipients                                | Ch  | How to test                                        |
| ----------------------- | -------------------------------------- | -------------------------- | ----------------------------------------------- | ----------------------------------------- | --- | -------------------------------------------------- |
| `SUBSCRIPTION_EXPIRING` | Scheduler: subscription ends in 7 days | Subscription Expiring Soon | Your subscription ({id}) will expire on {date}. | Active CEO users (`findActiveCeoUserIds`) | W+T | Wait for scheduler or adjust subscription end date |
| `SUBSCRIPTION_EXPIRED`  | Scheduler: subscription past end       | Subscription Expired       | Your subscription ({id}) expired on {date}.     | Active CEO users                          | W+T | Same                                               |


Source: `CompanySubscriptionScheduler`

---



### Module 3 — Company onboarding (`moduleNo: 3`)


| Event                  | Trigger                                         | Title                | Message                                    | Recipients       | Ch  |
| ---------------------- | ----------------------------------------------- | -------------------- | ------------------------------------------ | ---------------- | --- |
| `ONBOARDING_COMPLETED` | Company APPROVED (trial) or subscription ACTIVE | Onboarding Completed | Company onboarding is complete for {name}. | Active CEO users | W+T |


Source: `CompanyManagementServiceImpl`, `CompanySubscriptionServiceImpl`

---



### Module 5 — Role config (`moduleNo: 5`)


| Event                      | Trigger                 | Title                    | Message                                          | Recipients                  | Ch  |
| -------------------------- | ----------------------- | ------------------------ | ------------------------------------------------ | --------------------------- | --- |
| `ROLE_PERMISSIONS_UPDATED` | Update role permissions | Role Permissions Updated | Permissions for role '{name}' have been updated. | Users assigned to that role | W+T |


**Test:** Web → Roles → edit permissions → log in as affected user → check bell.

Source: `RoleServiceImpl`

---



### Module 6 — Salary / leave config


| Event                      | Trigger                        | Title                     | Message                                 | Recipients                       | Ch  |
| -------------------------- | ------------------------------ | ------------------------- | --------------------------------------- | -------------------------------- | --- |
| `LEAVE_APPROVAL_ESCALATED` | Scheduler: leave pending > 48h | Leave Approval Escalation | Leave request {code} pending > 48 hours | `HRM_MANAGEMENT_APPROVE` (+ CEO) | W+T |


**No notification** on role compensation create/update (salary / leave policy config save).

Leave apply / approve / reject / low-balance also publish as **moduleNo 6** — see Module 25. `LEAVE_APPLIED` → `HRM_MANAGEMENT_APPROVE` / `EDIT`.

Source: `ApprovalEscalationScheduler`, `HrmLeaveServiceImpl`

---



### Module 7 — Branch (`moduleNo: 7`)


| Event          | Trigger       | Title | Message | Recipients | Ch  |
| -------------- | ------------- | ----- | ------- | ---------- | --- |
| `PETTY_CASH_*` | See Module 24 |       |         |            |     |


**No notification** on branch create, update-to-inactive, or soft-delete.

Source: `BranchServiceImpl` (branch lifecycle only; petty cash events in Module 24)

---



### Module 8 — Employee (`moduleNo: 8`)


| Event                  | Trigger                                        | Title                | Message                                | Recipients                            | Ch  |
| ---------------------- | ---------------------------------------------- | -------------------- | -------------------------------------- | ------------------------------------- | --- |
| `EMPLOYEE_DEACTIVATED` | Deactivate user (update status or soft-delete) | Employee Deactivated | {name} ({empId}) has been deactivated. | Reporting manager only (skip if none) | W+T |


**No notification** on employee create or document upload.

**Test:** Users → deactivate employee (edit status or delete); log in as **reporting manager** → check bell.

Source: `UserServiceImpl`

---



### Module 9 — Tax (`moduleNo: 9`)


| Event                      | Trigger                      | Title                        | Message                                                  | Recipients                                                                                                             | Ch  |
| -------------------------- | ---------------------------- | ---------------------------- | -------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- | --- |
| `TAX_RATE_MODIFIED`        | Update tax type default rate | Tax Rate Modified            | Tax rate for '{name}' changed from {old}% to {new}%.     | `TAX_MANAGEMENT_READ` / `EDIT`, `BILLS_MANAGEMENT_READ`, `LEDGER_MANAGEMENT_READ`, `CHART_OF_ACCOUNTS_MANAGEMENT_READ` | W+T |
| `HIRING_REQUEST_SUBMITTED` | Submit hiring request        | New Hiring Request Submitted | {n} {designation}(s) in {dept} requires review           | Selected `recipientUserIds` on the request                                                                             | W+T |
| `HIRING_REQUEST_APPROVED`  | Approve hiring request       | Hiring Request Approved      | Your hiring request for {designation} has been approved. | Requester                                                                                                              | W+T |
| `HIRING_REQUEST_REJECTED`  | Reject hiring request        | Hiring Request Rejected      | …rejected. Reason: {reason}                              | Requester                                                                                                              | W+T |


**Test:** Log in as a user with `TAX_MANAGEMENT_READ` (or CEO) → edit tax rate. For hiring: pick `recipientUserIds` on submit, then log in as that user; approve/reject → log in as requester.

Source: `TaxTypeServiceImpl`, `HiringRequestServiceImpl`

---



### Module 10 — Product / inventory (`moduleNo: 10`)

**No notifications** on product create, variant bulk create, or selling-price update.

Domain events (`InventoryProductCreatedEvent`, `InventoryProductUpdatedEvent`, etc.) still publish for stock/ledger sync.

Source: `InventoryProductServiceImpl`

---



### Module 11 — Stock (`moduleNo: 11`)


| Event                                    | Trigger                            | Title                                   | Message                                               | Recipients                                                                    | Ch  |
| ---------------------------------------- | ---------------------------------- | --------------------------------------- | ----------------------------------------------------- | ----------------------------------------------------------------------------- | --- |
| `STOCK_REQUEST_{STATUS}`                 | Stock request status change        | varies                                  | varies                                                | Selected stock-request recipient users + requester                            | W+T |
| `STOCK_LOW`                              | Stock falls below reorder          | Low Stock Alert                         | Product {name} running low ({qty}) in branch {branch} | `STOCK_MANAGEMENT_READ` / `EDIT` / `APPROVE`, `PURCHASE_ORDER_MANAGEMENT_ADD` | W+T |
| `STOCK_OUT`                              | Stock hits zero                    | Out of Stock Alert                      | Product {name} OUT OF STOCK in branch {branch}        | Same permission set                                                           | W+T |
| `STOCK_EXPIRY_WARN` / `STOCK_EXPIRED`    | Scheduler: batch expiry (30d/7d/0) | varies                                  | Product {name} (Batch…) in Central Stock …            | `STOCK_MANAGEMENT_READ` / `EDIT` / `APPROVE`                                  | W+T |
| `STOCK_REQUEST_SLA_BREACH`               | Scheduler: request pending > 48h   | Stock Request SLA Breach                | Request {id} pending > 48 hours                       | Selected stock-request recipient users + requester                            | W+T |
| `STOCK_TRANSFER_PENDING_APPROVAL`        | Submit branch transfer             | Branch transfer submitted               | Transfer {id} awaits approval at source branch        | Dest-branch `STOCK_MANAGEMENT_APPROVE`                                        | W+T |
| `STOCK_TRANSFER_APPROVED`                | Approve transfer                   | Branch transfer approved                | Transfer {id} approved — awaiting dispatch            | Source-branch `STOCK_MANAGEMENT_REQUEST`                                      | W+T |
| `STOCK_TRANSFER_DISPATCH` / `IN_TRANSIT` | Dispatch / in-transit              | Branch transfer dispatched / in transit | Transfer {id} → {status}                              | Dest `STOCK_MANAGEMENT_APPROVE` + source `STOCK_MANAGEMENT_REQUEST`           | W+T |


**Test:** Log in as a user with `STOCK_MANAGEMENT_READ` (company-wide, or CEO) for low/out/expiry. For requests: log in as a **selected recipient user** or the requester. For transfer: dest-branch `STOCK_MANAGEMENT_APPROVE` then source-branch `STOCK_MANAGEMENT_REQUEST`.

Source: `StockManagementServiceImpl`, `StockExpiryScheduler`, `ProcurementEscalationScheduler`

---



### Module 12 — Service master (`moduleNo: 12`)


| Event                   | Trigger                    | Title                           | Message                                 | Recipients                                                            | Ch  |
| ----------------------- | -------------------------- | ------------------------------- | --------------------------------------- | --------------------------------------------------------------------- | --- |
| `SERVICE_PRICE_UPDATED` | Update service (non-draft) | Service Pricing/Details Updated | Service Pricing/Details Updated: {name} | `SERVICE_MANAGEMENT_READ` / `EDIT` / `ADD` via user permission matrix | W+T |


**No notification** on service create (non-draft).

**Test:** Log in as a user with `SERVICE_MANAGEMENT_READ` (or CEO) → another user updates a non-draft service.

Source: `ServiceManagementServiceImpl`

---



### Module 13 — Vendor (`moduleNo: 13`)


| Event                | Trigger                                       | Title              | Message                    | Recipients                                                          | Ch  |
| -------------------- | --------------------------------------------- | ------------------ | -------------------------- | ------------------------------------------------------------------- | --- |
| `VENDOR_DEACTIVATED` | Soft-delete vendor (`DELETE /vendors/delete`) | Vendor Deactivated | Vendor Deactivated: {name} | `VENDOR_MANAGEMENT_ADD` / `EDIT` (+ CEO) via user permission matrix | W+T |


**No notification** on vendor create (active). Active vendor create still runs ledger auto-provision (`ensureVendorLedger`).

**Note:** Updating vendor status to inactive via `PUT /vendors/update` does **not** emit this event — only the delete API does.

**Test:** Log in as a user with `VENDOR_MANAGEMENT_ADD` or `EDIT` (or CEO) → delete vendor.

Source: `VendorServiceImpl`

---



### Module 14 — Purchase order (`moduleNo: 14`)


| Event                         | Trigger                                                          | Title                                   | Message                                                        | Recipients                                                                                          | Ch  |
| ----------------------------- | ---------------------------------------------------------------- | --------------------------------------- | -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | --- |
| `PO_SUBMITTED`                | Submit PO for approval (central or branch-direct over threshold) | Purchase Order Submitted                | PO {number} submitted / needs approval (over branch threshold) | Central: users with `PURCHASE_ORDER_MANAGEMENT_EDIT`; Branch-Direct: selected recipient users/roles | W+T |
| `PO_APPROVED` / `PO_REJECTED` | Approve/reject PO                                                | varies                                  | PO {number} approved/rejected                                  | Central: users with `PURCHASE_ORDER_MANAGEMENT_EDIT`; Branch-Direct: PO requester                   | W+T |
| `PO_AUTO_APPROVED`            | Branch-Direct: under threshold auto-approve                      | Purchase Order auto-approved            | PO auto-approved                                               | PO requester                                                                                        | W+T |
| `PO_RETURNED_FOR_CORRECTION`  | Branch-Direct: return for correction                             | Purchase Order returned for correction  | Returned for correction                                        | PO requester                                                                                        | W+T |
| `PO_GRN_COMPLETED`            | Full GRN on PO                                                   | GRN Completed                           | Goods for PO {number} fully received                           | Company-wide: `PURCHASE_ORDER_MANAGEMENT_READ` / `EDIT`, `BILLS_MANAGEMENT_ADD` / `READ`, `STOCK_MANAGEMENT_READ` | W+T |
| `PO_SLA_BREACH`               | Scheduler: approval SLA                                          | Warning / CRITICAL: PO Approval Overdue | PO pending {24h/48h}                                           | `PURCHASE_ORDER_MANAGEMENT_EDIT`                                                                    | W+T |
| `PO_DELIVERY_OVERDUE`         | Scheduler: past delivery date                                    | Purchase Order Delivery Overdue         | PO {number} past delivery date                                 | `PURCHASE_ORDER_MANAGEMENT_READ` / `EDIT`, `STOCK_MANAGEMENT_READ`                                  | W+T |


**Test:** Log in as a user with `PURCHASE_ORDER_MANAGEMENT_EDIT` (or CEO) for central submit/approve/SLA. Branch-Direct: log in as a **selected recipient user/role**, then as the **PO requester** for auto-approve / return. GRN: `PURCHASE_ORDER_MANAGEMENT_READ` or `BILLS_MANAGEMENT_READ` or `STOCK_MANAGEMENT_READ`.

Source: `PurchaseOrderServiceImpl`, `BranchDirectPoWorkflowService`, `ProcurementEscalationScheduler`

---



### Module 15 — Leads (`moduleNo: 15`)


| Event                 | Trigger                                                                      | Title                   | Message                                        | Recipients                                                                                                    | Ch  |
| --------------------- | ---------------------------------------------------------------------------- | ----------------------- | ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | --- |
| `LEAD_STATUS_CHANGED` | Update lead when status changes                                              | Lead Status Updated     | Lead {name} status → {status} (was {old})      | Same-branch users with `LEADS_MANAGEMENT_ADD` / `EDIT` (+ CEO)                                                | W+T |
| `LEAD_CONVERTED`      | Update lead status → `CONVERTED`                                             | Lead Converted          | Lead {name} has been converted.                | Same-branch users with `LEADS_MANAGEMENT_ADD` / `EDIT` or `CUSTOMER_CONTRACT_MANAGEMENT_ADD` / `EDIT` (+ CEO) | W+T |
| `LEAD_CONVERTED`      | Customer create: `IMPORT_FROM_LEAD` (non-draft)                              | Lead Converted          | Lead {name} has been converted.                | Same-branch `LEADS_MANAGEMENT_ADD` / `EDIT` or `CUSTOMER_CONTRACT_MANAGEMENT_ADD` / `EDIT` + assignee (if set) | W+T |
| `LEAD_REASSIGNED`     | Update lead when `assignedToId` changes to a new user                        | Lead Reassigned         | Lead {name} has been reassigned.               | New assignee only                                                                                             | W+T |
| `LEAD_FOLLOW_UP`      | Scheduler 08:00: `nextFollowUpDate` = today, status not `LOST` / `CONVERTED` | Lead Follow-up Reminder | Reminder to follow up with Lead: {name} today. | Assignee (`assignedToId`) only                                                                                | W+T |


**No notification** on lead create (non-draft).

**Notes:**

- Status change to `CONVERTED` from Leads emits both `LEAD_STATUS_CHANGED` and `LEAD_CONVERTED`.
- Unassigning a lead (`assignedToId` cleared) does **not** emit `LEAD_REASSIGNED`.
- Follow-up reminder is skipped / not sent if the lead has no assignee.

**Test:** Assign tester to the **lead's branch** with `LEADS_MANAGEMENT_EDIT` (status/convert). Reassign → log in as the **new assignee**. Import-from-lead → same-branch `LEADS_MANAGEMENT_EDIT` or `CUSTOMER_CONTRACT_MANAGEMENT_EDIT` (or the lead assignee). Follow-up: set `nextFollowUpDate` = today and log in as **assignee**.

Source: `LeadServiceImpl`, `CustomerServiceImpl`, `LeadFollowUpScheduler`

---



### Module 16 — Quotation (`moduleNo: 16`)


| Event                                       | Trigger                  | Title                      | Message                                    | Recipients                                   | Ch    |
| ------------------------------------------- | ------------------------ | -------------------------- | ------------------------------------------ | -------------------------------------------- | ----- |
| `QUOTATION_SENT_TO_CUSTOMER`                | Send to client (portal)  | Quotation Sent             | Quotation {no} sent for your review        | customer (portal)                            | **C** |
| `QUOTATION_ACCEPTED` / `QUOTATION_REJECTED` | Client action            | varies                     | varies                                     | creator                                      | W+T   |
| `QUOTATION_VIEWED_BY_CLIENT`                | Public link opened       | Quotation Viewed           | Client viewed quotation {no}               | creator                                      | W+T   |
| `QUOTATION_EXPIRED`                         | Scheduler / expiry job   | Quotation expired          | Quotation {no} expired (valid till {date}) | creator                                      | W+T   |
| `QUOTATION_ESCALATION_L1`                   | Scheduler: pending > 7d  | Quotation Pending > 7 Days | Quotation {no} pending > 7 days            | `QUOTATION_MANAGEMENT_READ` / `EDIT` / `ADD` | W+T   |
| `QUOTATION_NOT_VIEWED_3D`                   | Scheduler: not viewed 3d | Quotation Not Viewed       | Quotation {no} not viewed 3+ days          | creator                                      | W+T   |


**No employee notification** on quotation create/save or send. Customer portal still receives `QUOTATION_SENT_TO_CUSTOMER` when the quotation is linked to an existing customer.

**Test (web):** Send quotation — no employee bell. Log in as the **quotation creator** after client accept/view/expiry. Escalation: user with `QUOTATION_MANAGEMENT_READ` (or CEO).  
**Test (customer):** OTP as the linked customer phone → `QUOTATION_SENT_TO_CUSTOMER`.

Source: `QuotationServiceImpl`, `QuotationExpiryService`, `QuotationEscalationScheduler`

---



### Module 17 — GMA (`moduleNo: 17`)


| Event               | Trigger                 | Title                                    | Message                               | Recipients                                              | Ch  |
| ------------------- | ----------------------- | ---------------------------------------- | ------------------------------------- | ------------------------------------------------------- | --- |
| `GMA_SUBMITTED`     | Submit sheet            | GMA Sheet Submitted                      | GMA Sheet {id} submitted for approval | Users in selected approver roles                        | W+T |
| `GMA_APPROVED`      | Approve / auto-approve  | GMA Sheet Approved                       | GMA Sheet {id} approved               | `preparedBy` user                                       | W+T |
| `GMA_REJECTED`      | Reject sheet            | GMA Sheet Rejected                       | GMA Sheet {id} rejected               | `preparedBy` user                                       | W+T |
| `GMA_RETURNED`      | Return for correction   | GMA Sheet Returned                       | GMA Sheet {id} needs corrections: …   | `preparedBy` user                                       | W+T |
| `GMA_ESCALATION_L1` | Scheduler: pending > 3d | GMA Pending Approval > 3 Days            | GMA Sheet {id} pending > 3 days       | `GMA_SHEET_MANAGEMENT_APPROVE`                          | W+T |
| `GMA_SLA_BREACH`    | Scheduler: hourly SLA   | Warning / CRITICAL: GMA Approval Overdue | GMA pending 24h / 48h                 | 24h: selected approver-role users; 48h: all active CEOs | W+T |


**No notification** on GMA revoke (hard delete).

**Test:** Submit with selected **approver roles** → log in as a user in those roles. Approve/reject/return → log in as **preparedBy**. 3-day: `GMA_SHEET_MANAGEMENT_APPROVE`. 48h SLA: **CEO**.

Source: `GmaSheetServiceImpl`, `GmaEscalationScheduler`, `ProcurementEscalationScheduler`

---



### Module 18 — Customer (`moduleNo: 18`)


| Event                  | Trigger             | Title                | Message                          | Recipients                                         | Ch  |
| ---------------------- | ------------------- | -------------------- | -------------------------------- | -------------------------------------------------- | --- |
| `CUSTOMER_DEACTIVATED` | Deactivate customer | Customer Deactivated | Customer Deactivated: {fullName} | Branch `CUSTOMER_CONTRACT_MANAGEMENT_ADD` / `EDIT` | W+T |


**No notification** on customer create (ACTIVE). Active customer create still runs ledger auto-provision (`ensureCustomerLedger`).

**Test:** Assign tester to the **customer's branch** with `CUSTOMER_CONTRACT_MANAGEMENT_EDIT` (or CEO on that branch) → deactivate.

Source: `CustomerServiceImpl`

---



### Module 19 — Contract (`moduleNo: 19`)


| Event                             | Trigger               | Title                           | Message                                   | Recipients                                                    | Ch    |
| --------------------------------- | --------------------- | ------------------------------- | ----------------------------------------- | ------------------------------------------------------------- | ----- |
| `CONTRACT_CREATED`                | Create contract       | New Contract Created            | New Contract Created: {customerName}      | `CONTRACT_MANAGEMENT_READ` / `ADD` / `EDIT` + all active CEOs | W+T   |
| `CONTRACT_ACTIVATED`              | Activate contract     | Contract Activated              | Contract Activated: {customerName}        | Same permission set + CEOs                                    | W+T   |
| `CONTRACT_ACTIVATED_FOR_CUSTOMER` | Activate contract     | Contract Activated              | Your service contract has been activated… | customer (portal)                                             | **C** |
| `CONTRACT_AMENDED`                | Amend contract        | Contract Amended                | Contract Amended: {customerName}          | Same permission set + CEOs                                    | W+T   |
| `CONTRACT_RENEWED`                | Renew contract        | Contract Renewed                | Contract {id} renewed to {endDate}        | Same permission set + CEOs                                    | W+T   |
| `CONTRACT_TERMINATED`             | Terminate             | Contract Terminated             | Contract Terminated: {customerName}       | Same permission set + CEOs                                    | W+T   |
| `CONTRACT_EXPIRY_WARN`            | Scheduler: 60/30 days | Contract Expiring in 60/30 Days | Contract {id} expires on {date}           | Same permission set + CEOs                                    | W+T   |
| `CONTRACT_EXPIRED`                | Scheduler: past end   | Contract Expired                | Contract {id} for {customer} expired      | Same permission set + CEOs                                    | W+T   |
| `CONTRACT_EXPIRING_FOR_CUSTOMER`  | Scheduler: 60/30 days | Contract Expiring Soon          | Your contract expires on {date}…          | customer (portal)                                             | **C** |


**Test (web):** Log in as a user with `CONTRACT_MANAGEMENT_READ` (or any CEO) → another user creates/activates.  
**Test (customer):** OTP as customer phone → activate contract → `CONTRACT_ACTIVATED_FOR_CUSTOMER`.

Source: `ContractServiceImpl`, `ContractRenewalScheduler`

---



### Module 20 — Sales order (`moduleNo: 20`)


| Event              | Trigger                                   | Title                     | Message                                    | Recipients                                                      | Ch  |
| ------------------ | ----------------------------------------- | ------------------------- | ------------------------------------------ | --------------------------------------------------------------- | --- |
| `SO_CREATED`       | Create SO                                 | New Sales Order Created   | New Sales Order Created: {soNumber}        | Branch `SALES_ORDER_MANAGEMENT_ADD` / `READ`                    | W+T |
| `SO_APPROVED`      | Create SO not as draft (open immediately) | Sales Order Approved      | Sales Order Approved: {soNumber}           | Branch `SALES_ORDER_MANAGEMENT_EDIT` + `TASK_MANAGEMENT_ADD`    | W+T |
| `SO_CANCELLED`     | Cancel SO                                 | Sales Order Cancelled     | Sales Order Cancelled: {soNumber}          | Branch `SALES_ORDER_MANAGEMENT_EDIT`                            | W+T |
| `SO_FULFILLED`     | Auto-fulfill when all tasks done          | Sales Order Fulfilled     | Sales Order Fulfilled: {soNumber}          | Branch `SALES_ORDER_MANAGEMENT_READ` + `INVOICE_MANAGEMENT_ADD` | W+T |
| `SO_FULFILLED`     | Manual fulfill                            | Sales Order Fulfilled     | Sales Order Fulfilled (Manual): {soNumber} | Branch `SALES_ORDER_MANAGEMENT_READ` + `INVOICE_MANAGEMENT_ADD` | W+T |
| `SO_BILLED`        | SO fully paid / billed                    | Sales Order Billed        | Sales order fully paid                     | Branch `INVOICE_MANAGEMENT_READ` + `PAYMENT_MANAGEMENT_READ`    | W+T |
| `SO_ESCALATION_L1` | Scheduler: open > 7d                      | Sales Order Open > 7 Days | SO {no} open 7 days without fulfillment    | Branch `SALES_ORDER_MANAGEMENT_READ` / `EDIT`                   | W+T |


**Test:** Assign tester to the **SO branch**. Create → `SALES_ORDER_MANAGEMENT_READ`. Non-draft create also notifies `SALES_ORDER_MANAGEMENT_EDIT` and `TASK_MANAGEMENT_ADD`. Manual fulfill → same as auto: `SALES_ORDER_MANAGEMENT_READ` or `INVOICE_MANAGEMENT_ADD`.

Source: `SalesOrderServiceImpl`, `SalesOrderEscalationScheduler`

---



### Module 21 — Task / visits (`moduleNo: 21`)


| Event                         | Trigger                             | Title                    | Message                                        | Recipients                                 | Ch    |
| ----------------------------- | ----------------------------------- | ------------------------ | ---------------------------------------------- | ------------------------------------------ | ----- |
| `TASK_ASSIGNED`               | Create / reassign task              | New Task Assigned        | Assigned to task {taskNo} at {site}            | assigned technician IDs                    | W+T   |
| `TASK_REASSIGNED`             | Reassign                            | Task Reassigned          | Reassigned to task {taskNo}                    | new techs                                  | W+T   |
| `TASK_REASSIGNED_FROM_ME`     | Removed from task                   | Removed from Task        | Removed from task {taskNo}                     | removed tech IDs                           | W+T   |
| `TASK_COMPLETED`              | Complete visit                      | Task Completed           | Task {taskNo} at {site} completed              | Branch `TASK_MANAGEMENT_ADD` / `EDIT`      | W+T   |
| `TASK_RESCHEDULED`            | Reschedule                          | Task Rescheduled         | Task {taskNo} rescheduled to {date} {time}     | assigned techs                             | W+T   |
| `TASK_SELFIE_COMPLETED`       | Pre-task selfie upload              | Task Selfie Completed    | Technician uploaded selfie for {taskNo}        | Branch `TASK_MANAGEMENT_ADD` / `EDIT`      | W+T   |
| `TASK_OVERDUE`                | Scheduler: past scheduled date      | Task overdue             | Task {taskNo} overdue                          | Assigned technicians                       | W+T   |
| `TASK_OVERDUE_CRITICAL`       | Scheduler: overdue by 3 days        | Task Critically Overdue  | Task {taskNo} overdue by 3 days                | Branch `TASK_MANAGEMENT_ADD` / `EDIT` | W+T   |
| `VISIT_SCHEDULED`             | Create task with schedule           | Visit Scheduled          | Visit scheduled on {date} at {time} for {site} | customer + site contact                    | **C** |
| `TASK_REMINDER_DAY_BEFORE`    | Scheduler: ~24h before start        | Task reminder (tomorrow) | Tomorrow: {taskNo} at {customer}…              | assigned techs                             | W+T   |
| `TASK_REMINDER_1_HOUR_BEFORE` | Scheduler: ~1h before start         | Task starts in 1 hour    | Task {taskNo} starts in 1 hour…                | assigned techs                             | W+T   |
| `VISIT_REMINDER`              | Same scheduler (portal counterpart) | Visit Reminder           | Upcoming visit reminder                        | customer + site contact                    | **C** |
| `TECH_ON_THE_WAY`             | Tech starts travel (GPS)            | Technician On The Way    | Technician started travel for {taskNo}         | customer + site contact                    | **C** |
| `TECH_CHECKED_IN`             | Tech check-in / selfie              | Technician Checked In    | Technician checked in for {taskNo}             | customer + site contact                    | **C** |
| `VISIT_COMPLETED`             | Complete task                       | Visit Completed          | Visit {taskNo} completed                       | customer + site contact                    | **C** |
| `REPORT_READY`                | Complete task                       | Service Report Ready     | Report for {taskNo} available                  | customer + site contact                    | **C** |
| `VISIT_RESCHEDULED`           | Reschedule                          | Visit Rescheduled        | Visit rescheduled to {date} {time}             | customer + site contact                    | **C** |
| `VISIT_CANCELLED`             | Cancel task                         | Visit Cancelled          | Visit cancelled (+ reason)                     | customer + site contact                    | **C** |
| `RE_TASK_FROM_TICKET`         | Task from support ticket            | Re-task from Ticket      | Re-task {taskNo} from ticket {ticketId}        | Branch `TASK_MANAGEMENT_ADD` / `EDIT` | W+T   |


**Test (web):** Assign tester to the **task branch** with `TASK_MANAGEMENT_EDIT` for complete/selfie, critical overdue, and re-task.  
**Test (tech):** Assigned technician user → `TASK_ASSIGNED`.  
**Test (customer):** OTP as customer phone **and** site-contact mobile (two logins).

Source: `TaskServiceImpl`, `TechnicianTrackingServiceImpl`, `TaskReminderScheduler`, `TaskStatusUpdateService`

---



### Module 22 — Support (`moduleNo: 22`)


| Event                        | Trigger                       | Title                 | Message                           | Recipients                                                                            | Ch    |
| ---------------------------- | ----------------------------- | --------------------- | --------------------------------- | ------------------------------------------------------------------------------------- | ----- |
| `TICKET_CREATED`             | Raise ticket                  | New Support Ticket    | varies (includes customer/branch) | Branch `CUSTOMER_SUPPORT_MANAGEMENT_EDIT` / `READ` (+ CEO) via user permission matrix | W+T   |
| `TICKET_ASSIGNED`            | Assign ticket                 | Ticket Assigned       | Ticket {no} assigned to you       | Assignee                                                                              | W+T   |
| `TICKET_RESOLVED`            | Resolve ticket                | Ticket Resolved       | Ticket {no} resolved              | Branch `CUSTOMER_SUPPORT_MANAGEMENT_EDIT` / `READ` (+ CEO) via user permission matrix | W+T   |
| `TICKET_UPDATE_FOR_CUSTOMER` | In-progress / resolve / close | Support Ticket Update | status message                    | Customer (portal)                                                                     | **C** |
| `TICKET_SLA_ESCALATION_L1`   | SLA sweep: response deadline  | Ticket SLA escalation | Ticket {no} escalated to L1       | Branch `CUSTOMER_SUPPORT_MANAGEMENT_EDIT` (+ CEO on that branch)                      | W+T   |
| `TICKET_SLA_ESCALATION_L2`   | SLA sweep: resolution at risk | Ticket SLA escalation | Ticket {no} escalated to L2       | Branch `CUSTOMER_SUPPORT_MANAGEMENT_EDIT` + all active CEOs                           | W+T   |
| `TICKET_SLA_ESCALATION_L3`   | SLA sweep: resolution overdue | Ticket SLA escalation | Ticket {no} escalated to L3       | All active CEOs                                                                       | W+T   |


SLA escalation fires only when the computed level **changes**. L3 is Critical; L1/L2 are High.

**Test (web):** Assign tester to the **ticket branch** with `CUSTOMER_SUPPORT_MANAGEMENT_READ` (create/resolve) or `EDIT` (L1). L2/L3: also log in as **CEO**. Assign → log in as **assignee**.  
**Test (customer):** OTP as ticket customer phone.

Source: `SupportTicketServiceImpl`

---



### Module 24 — Petty cash (`moduleNo: 24` / also module 7 in some schedulers)


| Event                   | Trigger                  | Title                        | Message                     | Recipients                        | Ch  |
| ----------------------- | ------------------------ | ---------------------------- | --------------------------- | --------------------------------- | --- |
| `PETTY_CASH_SUBMITTED`  | Submit request           | Petty Cash request submitted | Amount ₹{amt} needs review  | Users in selected recipient roles | W+T |
| `PETTY_CASH_APPROVED`   | Approve                  | Petty Cash Approved          | Request for ₹{amt} approved | requester                         | W+T |
| `PETTY_CASH_REJECTED`   | Reject                   | Petty Cash Rejected          | Rejected: {reason}          | requester                         | W+T |
| `PETTY_CASH_RETURNED`   | Return for correction    | Petty Cash Returned          | Needs corrections: {notes}  | requester                         | W+T |
| `PETTY_CASH_PAID`       | Mark paid                | Petty Cash Paid              | Disbursed ₹{amt}            | requester                         | W+T |
| `PETTY_CASH_SLA_BREACH` | Scheduler: pending > 48h | Petty Cash SLA Breach        | Pending > 48 hours          | Branch `PETTY_CASH_MANAGEMENT_APPROVE` | W+T |
| `PC_DRAFT_REMINDER`     | Scheduler: draft > 3d    | Petty Cash Draft Reminder    | Draft unsubmitted 3+ days   | requester                         | W+T |


Publisher `moduleNo` is **7** (branch/petty-cash), not 24.

**Test:** Submit with selected **recipient roles** → log in as a user in those roles. Approve/pay → log in as **requester**. SLA: same-branch `PETTY_CASH_MANAGEMENT_APPROVE`.

Source: `PettyCashServiceImpl`, `ApprovalEscalationScheduler`

---



### Module 25 — HRM (`moduleNo: 25`)


| Event                                   | Trigger                        | Title                         | Message                                 | Recipients                                                       | Ch  |
| --------------------------------------- | ------------------------------ | ----------------------------- | --------------------------------------- | ---------------------------------------------------------------- | --- |
| `LEAVE_APPLIED`                         | Apply leave                    | Leave Application             | {name} applied for {n} day(s) of {type} | `HRM_MANAGEMENT_APPROVE` / `EDIT`                                | W+T |
| `LEAVE_APPROVED`                        | Approve leave                  | Leave Approved                | Your {type} for {n} day(s) approved     | employee                                                         | W+T |
| `LEAVE_REJECTED`                        | Reject leave                   | Leave Rejected                | Rejected: {reason}                      | employee                                                         | W+T |
| `LEAVE_BALANCE_LOW`                     | After approval, low balance    | Low Leave Balance             | {type} balance low ({remaining} days)   | employee                                                         | W+T |
| `PUNCH_IN_SUCCESS`                      | Mobile/web punch in            | Checked In                    | Checked in at {time}                    | employee                                                         | W+T |
| `PUNCH_OUT_SUCCESS`                     | Punch out                      | Checked Out                   | Checked out at {time}, total minutes    | employee                                                         | W+T |
| `LATE_CHECK_IN`                         | Late punch in                  | Late Check-In                 | Late check-in at {time}                 | employee                                                         | W+T |
| `MARKED_ABSENT`                         | HR marks absent                | Marked Absent                 | Marked absent on {date}                 | employee                                                         | W+T |
| `SHIFT_START_REMINDER`                  | Scheduler: 30 min before shift | Shift Start Reminder          | Shift starts in 30 minutes              | employee                                                         | W+T |
| `SHIFT_END_REMINDER`                    | Scheduler: 15 min before end   | Shift End Reminder            | Attendance day ends soon — punch out    | open-session users                                               | W+T |
| `AUTO_PUNCH_OUT`                        | Scheduler: auto punch out      | Auto Punch-Out                | Auto punched out (no logout)            | employee                                                         | W+T |
| `ATTENDANCE_MISSING`                    | Scheduler: no check-in today   | Missing Attendance            | No check-in for {employee} today        | `HRM_MANAGEMENT_EDIT` / `ADD`                                    | W+T |
| `UNAUTHORIZED_ABSENCE`                  | Scheduler: 3+ days absent      | Unauthorized Absence Alert    | Absent 3+ days without leave            | `HRM_MANAGEMENT_EDIT` + reporting manager (if set)               | W+T |
| `PAYROLL_PROCESSED`                     | Process salary month           | Payroll Processed             | Salary for {m}/{y} processed            | employee                                                         | W+T |
| `SALARY_PAID`                           | Mark salary paid               | Salary Paid                   | Salary for {m}/{y} has been paid        | employee                                                         | W+T |
| `PAYROLL_STARTED` / `PAYROLL_COMPLETED` | Bulk salary upload             | Payroll Run Started/Completed | Bulk upload status                      | `HRM_MANAGEMENT_EDIT`                                            | W+T |


Leave apply/approve/reject/low-balance use publisher **moduleNo 6**.

**Test (web):** Leave apply → log in as a user with `HRM_MANAGEMENT_APPROVE` or `EDIT`. Leave approve → log in as the **employee**. Missing attendance → `HRM_MANAGEMENT_EDIT` / `ADD`. Unauthorized absence → `HRM_MANAGEMENT_EDIT` and the employee's reporting manager. Payroll bulk → `HRM_MANAGEMENT_EDIT`.  
**Test (tech):** Punch in/out as the **same employee** → `PUNCH_IN_SUCCESS` / `PUNCH_OUT_SUCCESS`.

Source: `HrmLeaveServiceImpl`, `HrmAttendanceServiceImpl`, `HrmSalaryServiceImpl`, `AttendanceScheduler`, `SiteComplianceScheduler`

---



### Module 26 — Self-service (`moduleNo: 26`)

**No notifications** on mobile profile update (self-service).

Source: `MobileServiceImpl`

---



### Module 28 — Invoicing (`moduleNo: 28`)


| Event                      | Trigger                  | Title                   | Message                                   | Recipients                                                                 | Ch    |
| -------------------------- | ------------------------ | ----------------------- | ----------------------------------------- | -------------------------------------------------------------------------- | ----- |
| `INVOICE_GENERATED`        | Approve/send invoice     | New Invoice Generated   | Invoice {no} approved and sent            | Branch `INVOICE_MANAGEMENT_APPROVE` / `READ` / `EDIT`                      | W+T   |
| `INVOICE_AVAILABLE`        | Approve/send invoice     | Invoice Available       | Invoice {no} for ₹{total}, due {date}     | Customer portal (customer phone)                                           | **C** |
| `INVOICE_PARTIAL_PAID`     | Payment allocated        | Invoice Partially Paid  | Partially paid, pending ₹{amt}            | `RECEIPT_MANAGEMENT_ADD`, `PAYMENT_MANAGEMENT_ADD` / `READ`, `INVOICE_MANAGEMENT_READ` | W+T   |
| `INVOICE_FULLY_PAID`       | Full payment             | Invoice Fully Paid      | Invoice {no} fully paid                   | `RECEIPT_MANAGEMENT_ADD`, `PAYMENT_MANAGEMENT_ADD`, `INVOICE_MANAGEMENT_READ`, `SALES_ORDER_MANAGEMENT_READ` | W+T   |
| `CREDIT_NOTE_ISSUED`       | Issue credit note        | Credit Note Issued      | CN {no} for invoice {no}                  | `INVOICE_MANAGEMENT_ADD` / `EDIT` / `READ` / `EXPORT`                      | W+T   |
| `INVOICE_DUE_SOON`         | Scheduler: due in 3 days | Invoice Due Soon        | Invoice {no} due on {date}                | Customer portal                                                            | **C** |
| `INVOICE_OVERDUE_CUSTOMER` | Scheduler: just overdue  | Invoice Overdue         | Invoice {no} overdue, pay soon            | Customer portal                                                            | **C** |
| `INVOICE_ESCALATION_L1`    | Scheduler: 30d overdue   | Invoice 30 Days Overdue | Invoice {no} is 30 days overdue           | `INVOICE_MANAGEMENT_READ` / `EDIT` / `EXPORT`, `PAYMENT_MANAGEMENT_ADD`    | W+T   |
| `INVOICE_ESCALATION_L2`    | Scheduler: 60d overdue   | Invoice 60 Days Overdue | CRITICAL: Invoice {no} is 60 days overdue | All active CEOs                                                            | W+T   |


**Test (web):** Assign tester to the **invoice branch** with `INVOICE_MANAGEMENT_READ` for generated. Partial/full paid: `PAYMENT_MANAGEMENT_ADD` or `INVOICE_MANAGEMENT_READ`. L1: `INVOICE_MANAGEMENT_EDIT`. L2: **CEO**.  
**Test (customer):** OTP as invoice customer phone → `INVOICE_AVAILABLE`.

Source: `InvoicingServiceImpl`, `PaymentsServiceImpl`, `InvoiceEscalationScheduler`

---



### Module 29 — Bills (`moduleNo: 29`)


| Event                 | Trigger                                   | Title                   | Message                             | Recipients                                                                 | Ch  |
| --------------------- | ----------------------------------------- | ----------------------- | ----------------------------------- | -------------------------------------------------------------------------- | --- |
| `BILL_RECORDED`       | Record purchase bill (draft)              | New Bill Recorded       | Bill {no} recorded as DRAFT         | `BILLS_MANAGEMENT_ADD` / `READ` / `EDIT` / `APPROVE`                       | W+T |
| `DRAFT_BILL_FROM_GRN` | Draft bill create with `grnReference` set | Draft Bill from GRN     | Draft bill {no} linked to GRN {ref} | Same as `BILL_RECORDED`                                                    | W+T |
| `BILL_CONFIRMED`      | Confirm bill                              | Purchase Bill Confirmed | Bill {no} confirmed, ledger posted  | `BILLS_MANAGEMENT_APPROVE` / `READ`, `PAYMENT_MANAGEMENT_ADD`              | W+T |
| `DEBIT_NOTE_ISSUED`   | Issue debit note                          | Debit Note Issued       | DN {no} against bill {no}           | `BILLS_MANAGEMENT_ADD` / `EDIT` / `READ`                                   | W+T |
| `BILL_PAID`           | Fully settle bill                         | Bill Fully Paid         | Bill {no} fully settled             | `BILLS_MANAGEMENT_READ` / `APPROVE`, `PAYMENT_MANAGEMENT_ADD`              | W+T |
| `BILL_DUE_SOON`       | Scheduler: due in 3 days                  | Purchase Bill Due Soon  | Bill {no} due in 3 days             | `BILLS_MANAGEMENT_READ` / `APPROVE`, `PAYMENT_MANAGEMENT_ADD`              | W+T |
| `BILL_OVERDUE`        | Scheduler: overdue                        | Purchase Bill Overdue   | Bill {no} overdue                   | Same as `BILL_DUE_SOON`                                                    | W+T |


**Test:** Log in as a user with `BILLS_MANAGEMENT_READ` (or CEO) → record / confirm / pay.

Source: `BillsServiceImpl`, `BillEscalationScheduler`

---



### Module 30 — Payments (`moduleNo: 30`)


| Event                  | Trigger                 | Title              | Message                          | Recipients                                                                 | Ch    |
| ---------------------- | ----------------------- | ------------------ | -------------------------------- | -------------------------------------------------------------------------- | ----- |
| `PAYMENT_RECEIVED`     | Record customer receipt | Payment Received   | Receipt {voucherNo} for ₹{amt}   | `PAYMENT_MANAGEMENT_ADD` / `READ`, `INVOICE_MANAGEMENT_READ`               | W+T   |
| `PAYMENT_ACKNOWLEDGED` | Record customer receipt | Payment Received   | We received your payment ₹{amt}… | Customer portal (customer phone)                                           | **C** |
| `PAYMENT_DISPATCHED`   | Vendor payment out      | Payment Dispatched | Voucher {no} processed ₹{amt}    | `PAYMENT_MANAGEMENT_ADD` / `READ`, `BILLS_MANAGEMENT_READ`                 | W+T   |


**Test (web):** Log in as a user with `PAYMENT_MANAGEMENT_READ` (or CEO) → record receipt.  
**Test (customer):** OTP as customer phone → `PAYMENT_ACKNOWLEDGED`.

Source: `PaymentsServiceImpl`

---



### Module 31 — Ledger (`moduleNo: 31`)


| Event                          | Trigger               | Title                 | Message                             | Recipients                                                                 | Ch  |
| ------------------------------ | --------------------- | --------------------- | ----------------------------------- | -------------------------------------------------------------------------- | --- |
| `LEDGER_CREATED`               | Create ledger         | New Ledger Created    | Ledger {name} ({code}) created      | `LEDGER_MANAGEMENT_ADD` / `READ` (+ all CEOs)                              | W+T |
| `LEDGER_CREDIT_LIMIT_EXCEEDED` | Posting exceeds limit | Credit Limit Exceeded | Ledger {name} exceeded credit limit | `LEDGER_MANAGEMENT_EDIT`, `INVOICE_MANAGEMENT_READ` (+ all CEOs)           | W+T |


**No notification** when ledger credit utilization crosses 80% (warning threshold removed).

**Test:** Log in as a user with `LEDGER_MANAGEMENT_READ` (or CEO) → create ledger; post enough to exceed credit limit (no 80% warning).

Source: `LedgerServiceImpl`

---



### Module 32 — Chart of accounts (`moduleNo: 32`)


| Event                  | Trigger         | Title                   | Message                         | Recipients                                                                 | Ch  |
| ---------------------- | --------------- | ----------------------- | ------------------------------- | -------------------------------------------------------------------------- | --- |
| `NEW_COA_ACCOUNT`      | Create COA head | New COA Account Created | New COA Account: {name}         | `CHART_OF_ACCOUNTS_MANAGEMENT_ADD` / `READ` (+ all CEOs)                   | W+T |
| `COA_HEAD_DEACTIVATED` | Deactivate head | COA Account Deactivated | COA Account Deactivated: {name} | `CHART_OF_ACCOUNTS_MANAGEMENT_EDIT` / `READ` (+ all CEOs)                  | W+T |


**Test:** Log in as a user with `CHART_OF_ACCOUNTS_MANAGEMENT_READ` (or CEO) → create / deactivate COA head.

Source: `CoaServiceImpl`

---



## 4. Customer / site events (quick matrix)

All use `CUSTOMER_PUSH` only. Login and device register required.


| `eventType`                       | Typical trigger              | Who receives            |
| --------------------------------- | ---------------------------- | ----------------------- |
| `QUOTATION_SENT_TO_CUSTOMER`      | Send quotation to client     | Customer                |
| `CONTRACT_ACTIVATED_FOR_CUSTOMER` | Activate contract            | Customer                |
| `CONTRACT_EXPIRING_FOR_CUSTOMER`  | Contract renewal scheduler   | Customer                |
| `VISIT_SCHEDULED`                 | Create scheduled task        | Customer + site contact |
| `VISIT_REMINDER`                  | Task reminder scheduler      | Customer + site contact |
| `TECH_ON_THE_WAY`                 | Tech starts travel           | Customer + site contact |
| `TECH_CHECKED_IN`                 | Tech check-in                | Customer + site contact |
| `VISIT_COMPLETED`                 | Complete task                | Customer + site contact |
| `REPORT_READY`                    | Complete task                | Customer + site contact |
| `VISIT_RESCHEDULED`               | Reschedule task              | Customer + site contact |
| `VISIT_CANCELLED`                 | Cancel task                  | Customer + site contact |
| `INVOICE_AVAILABLE`               | Approve invoice              | Customer                |
| `INVOICE_DUE_SOON`                | Invoice scheduler            | Customer                |
| `INVOICE_OVERDUE_CUSTOMER`        | Invoice overdue scheduler    | Customer                |
| `PAYMENT_ACKNOWLEDGED`            | Record payment receipt       | Customer                |
| `TICKET_UPDATE_FOR_CUSTOMER`      | Support ticket status change | Customer / site         |


---



## 5. Suggested end-to-end test scenarios



### Scenario A — Visit lifecycle (all 3 apps)

1. **Web:** Create sales order → create task with `customerId`, `siteContactMobile`, schedule, assign tech.
2. **Customer app:** `VISIT_SCHEDULED` push + feed.
3. **Tech app:** `TASK_ASSIGNED` push.
4. **Tech app:** Start travel → customer gets `TECH_ON_THE_WAY`.
5. **Tech app:** Check in → customer gets `TECH_CHECKED_IN`.
6. **Tech app:** Complete task → customer gets `VISIT_COMPLETED` + `REPORT_READY`; web gets `TASK_COMPLETED`.



### Scenario B — Invoice & payment (web + customer)

1. **Web:** Approve invoice → customer `INVOICE_AVAILABLE`.
2. **Web:** Record payment receipt → customer `PAYMENT_ACKNOWLEDGED`; accounts `PAYMENT_RECEIVED` + `INVOICE_FULLY_PAID`.



### Scenario C — Contract (web + customer)

1. **Web:** Create and activate contract.
2. **Customer app:** `CONTRACT_ACTIVATED_FOR_CUSTOMER`.
3. *(Optional)* Adjust end date for scheduler → `CONTRACT_EXPIRING_FOR_CUSTOMER`.



### Scenario D — Employee-only (web + tech)

1. **Web:** Create lead → no bell; change status → `LEAD_STATUS_CHANGED` (branch Leads ADD/EDIT); reassign → new assignee `LEAD_REASSIGNED`.
2. **Web:** Submit PO → users with PO edit permission get `PO_SUBMITTED` (Branch-Direct may use recipient users/roles).
3. **Web (Branch-Direct):** Auto-approve under threshold → requester `PO_AUTO_APPROVED`; return → `PO_RETURNED_FOR_CORRECTION`.
4. **Tech:** Punch in/out → `PUNCH_IN_SUCCESS` / `PUNCH_OUT_SUCCESS` on technician device.

---



## 6. Not wired yet (blocked)

No employee-module events are currently blocked. See [pending-notifications-implementation-plan.md](./pending-notifications-implementation-plan.md) for backlog history.

---



## 7. Related docs


| Doc                                                                                                            | Purpose                            |
| -------------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| [notification-platform.md](./notification-platform.md)                                                         | Architecture & write/read paths    |
| [pending-notifications-implementation-plan.md](./pending-notifications-implementation-plan.md)                 | Live vs pending vs blocked backlog |
| [frontend-integration-react.md](./frontend-integration-react.md)                                               | Web bell + WebSocket               |
| [customer-site-push-notifications-mobile-guide.md](../mobile/customer-site-push-notifications-mobile-guide.md) | Customer app APIs & FCM            |
| [api-index.md](./api-index.md)                                                                                 | Notification REST index            |


---



## 8. Source code index


| Area              | Primary classes                                                    |
| ----------------- | ------------------------------------------------------------------ |
| Publish API       | `NotificationPublisher`, `NotificationPublisherImpl`               |
| Employee feed     | `NotificationController`, `NotificationFeedService`                |
| Customer feed     | `CustomerNotificationController`, `PortalNotificationFeedService`  |
| Recipients        | `NotificationRecipientResolver`, `PortalNotificationHelper`        |
| Constants         | `NotificationEventTypes`, `NotificationModuleNos`                  |
| Schedulers        | `src/main/java/com/security/rbac/modules/notification/scheduler/*` |
| Branch-Direct PO  | `BranchDirectPoWorkflowService`                                    |
| Tech FCM register | `MobileController` → `POST /api/v1/mobile/devices/register`        |


