# Module 11 (11.1–11.4) — Stock Management APIs (Beginner-friendly frontend guide)

This guide maps each **Module 11 Stock screen** to a clear set of **backend API endpoints**, explains the **CEO / Head Office central stock entry** flow, and then covers the **branch request → approve → dispatch → receive** lifecycle.

It is written to stay aligned with the **Module 10 refactor** where **each variant is a SKU row**.

---

## Core alignment with Module 10 (Variants/SKUs)

### Product dropdown (used everywhere in Module 11)

- **API**: `GET /api/v1/inventory-products/dropdown`
- **Rule**: **each dropdown option = one SKU/variant**
- **Frontend must store**:
  - `inventoryProductId = option.id` (**primary key for stock**)
  - `productCode` (display only; do not use as DB key)
  - `productName`, `variantName`, `baseUom`, `hsnCode`
  - `groupKey` (optional: only for “view all variants” screens)

### Stock is always tracked at SKU level

All Module 11 stock / request / transfer records should reference:

- `inventoryProductId` (variant/SKU ID from Module 10)
- `branchId` (Central or specific branch)
- `stockType` (ASSET / CONSUMABLE / RESELL)

---

## Base path recommendation (Module 11)

Use one backend base path for stock operations:

- Base path: **`/api/v1/stock`**

If your backend already uses a different base path, keep the **screen-to-endpoint mapping** below but rename the base path consistently.

---

## Common headers

- **Auth**: JWT Bearer token
- **Tenant (if enabled)**: `X-Tenant-ID: <tenantSchemaOrTenantId>`
- **Content-Type**: `application/json`

---

## Glossary (beginner friendly)

- **Central**: Head Office / Central Warehouse stock location.
- **Branch**: Any non-central location (BLR / HYD / BOM etc.).
- **Stock types**:
  - **Assets**: individually tracked units (each has an `assetId`)
  - **Consumables**: bulk quantity
  - **Resell**: quantity meant for sale
- **Reserved**: approved quantity locked at source, not yet received.
- **In-Transit**: dispatched quantity moving between locations.

---

## CEO / Higher-level scenario (Central entry → allocation → transfers)

CEO / Head Ops typically does:

- Add new purchase into **Central** (creates a **Central Stock Entry**)
- Optionally allocate some quantity to branches immediately (creates transfers)
- Later, branches request stock; HO approves and dispatches; branch receives

High-level ASCII flow:

```text
PHASE 1: CENTRAL PROCUREMENT (Head Ops / CEO view)
┌─────────────────┐     ┌─────────────────────────┐
│ Product Master  │────▶│ Add to Central Stock    │
│ (Module 10 SKU) │     │ (creates entry CSTK-*)  │
└─────────────────┘     └──────────┬──────────────┘
                                   │
                                   ▼
                       ┌─────────────────────────┐
                       │ Split by stock type     │
                       │ Assets / Cons / Resell  │
                       └──────────┬──────────────┘
                                   │
                 ┌─────────────────┼─────────────────┐
                 ▼                 ▼                 ▼
          ┌────────────┐   ┌────────────┐    ┌────────────┐
          │ Asset rows  │   │ Cons qty    │    │ Resell qty  │
          │ (assetId)   │   │ (bulk)      │    │ (bulk)      │
          └──────┬──────┘   └──────┬──────┘    └──────┬──────┘
                 └─────────────────┼───────────────────┘
                                   ▼
                        ┌────────────────────────┐
                        │ Central stock updated  │
                        └────────────────────────┘
```

---

## 11.1 Tab 1 — Stock Dashboard (Table View)

### Screen needs

- Unified table of SKU-wise quantities across accessible branches
- Filters: branch view, category, stock type, status, date range, search
- Columns: Assets/Consumable/Resell + Total + In-Transit + Reserved

### API to call

**`GET /api/v1/stock/dashboard`**

#### Query params (all optional)

- `branchId` (Central or specific branch; omit for role-based default)
- `viewScope` (optional, if you implement: `COMPANY_WIDE | CENTRAL | MY_BRANCHES | BRANCH`)
- `categories` (multi) — values from Module 10 enum
- `stockTypes` (multi): `ASSET`, `CONSUMABLE`, `RESELL`
- `status` (multi): `AVAILABLE`, `LOW`, `OUT`, `INACTIVE`
- `fromDate`, `toDate` (ISO date)
- `search` (productCode / productName / assetId / hsnCode)
- `pageNo`, `pageSize`

#### Response (high level)

`StockDashboardPage`:

- `count`
- `data[]` rows:
  - `inventoryProductId` (**SKU id**)
  - `productCode`, `productName`, `variantName`, `baseUom`, `hsnCode`, `category`, `brand`, `imageUrls`
  - `assetsQty`, `consumableQty`, `resellQty`, `totalQty`
  - `inTransitQty`, `reservedQty`, `availableQty`
  - `status`

### Row actions

- **View** → 11.1.A Product Stock Ledger
- **Edit** → 11.1.2 Edit Central Stock Entry (when entry-type is central entry & user has permission)
- **Delete** → optional (usually soft-disable; permission gated)

---

## 11.1.A Product Stock Ledger (Stock Movement Log)

### Screen needs

- Movement log for a **single SKU** across entries (stock request receipts + transfers)

### APIs to call

1. **Header summary**

- `GET /api/v1/stock/ledger/summary?inventoryProductId=<id>&branchId=<optional>`

2. **Movement list**

- `GET /api/v1/stock/ledger`

#### Query params

- `inventoryProductId` (**required**)
- `entryTypes` (multi): `CENTRAL_ENTRY`, `STOCK_REQUEST`, `BRANCH_TRANSFER`
- `direction` (optional): `IN`, `OUT`
- `branchId` (optional)
- `fromDate`, `toDate`
- `search` (entryId / supplier / branch / createdBy)
- `pageNo`, `pageSize`

#### Response row (high level)

- `entryId` (CSTK-_ or SR-_ or TR-\*)
- `entryType`, `direction`, `date`
- `fromBranch`, `toBranch`
- `assetsDelta`, `consumableDelta`, `resellDelta`
- `createdBy`
- `referenceId` (link to details screen)

### Row navigation

- If `entryType=CENTRAL_ENTRY` → 11.1.3 View Central Stock Entry
- If `entryType=STOCK_REQUEST` → 11.2.2 View Stock Request
- If `entryType=BRANCH_TRANSFER` → 11.4 View Branch Transfer (or transfer detail endpoint)

---

## 11.1.1 Add Stock — Mode: Add to Central Stock (Head Ops)

### Screen needs

- Select SKU, enter total quantity, split into Assets/Consumable/Resell
- If Assets > 0: generate asset IDs (auto/manual) + default assignment
- Purchase/tax details (supplier, invoice, batch/expiry for consumables)
- Optional initial allocation to branches (creates transfers)

### APIs to call

1. **Create central stock entry**

- `POST /api/v1/stock/central-entries`

#### Body (high level)

- `inventoryProductId` (**required**)
- `totalQty` (**required**)
- `allocations`:
  - `assetsQty`, `consumableQty`, `resellQty` (sum = total)
- `assetGeneration` (only if assetsQty > 0):
  - `mode`: `AUTO | MANUAL`
  - `prefix` (if auto), `startSequence` (if auto)
  - `defaultAssignment`: `CENTRAL | BRANCH`
  - `defaultBranchId` (if defaultAssignment = BRANCH)
  - `assignmentType`: `BRANCH_POOL | DIRECT_TO_EMPLOYEE`
  - `employeeAssignments[]` (if direct assignment)
- `purchase`:
  - `supplierId`, `purchaseOrderRef`, `invoiceNumber`, `invoiceDate`
  - `invoiceAmount`, `taxAmount`, `totalWithTax`
  - `invoiceFileId` (if file upload handled separately)
  - `batchNumber`, `manufacturingDate`, `expiryDate` (for consumables if required)
- `initialBranchAllocations[]` (optional):
  - `toBranchId`, `assetsQty`, `consumableQty`, `resellQty`

2. **(Optional) Upload invoice file**

- `POST /api/v1/files` (or your existing file module)

### Response (high level)

- `centralEntryId` (e.g., `CSTK-000482`)
- created asset IDs (if generated)
- ledger impact summary

---

## 11.1.2 Edit Central Stock Entry (Head Ops)

### Screen needs

- Edit type split, some allocation fields, replace invoice copy
- Must preserve audit integrity (lock product, lock totalQty, protect issued/transferred assets)

### APIs to call

1. **Load entry**

- `GET /api/v1/stock/central-entries/<centralEntryId>`

2. **Update entry**

- `PUT /api/v1/stock/central-entries/<centralEntryId>`

### Important backend rule

Reject edits if any of the entry’s stock has been:

- issued to employees, transferred, sold, consumed

---

## 11.1.3 View Central Stock Entry (Head Ops) — read-only

### Screen needs

- Full entry detail: procurement + split + asset list + branch allocations

### API to call

- `GET /api/v1/stock/central-entries/<centralEntryId>`

### Variant-level display rule (important)

Always show:

- `productCode` of the **SKU** (e.g., `BSP3-001`)
- `productName + (variantName)`
  Never show a shortened base code (ambiguous when variants exist).

---

## 11.2 Tab 2 — My Requests (list)

### Screen needs

- List of stock requests and transfer requests created by user / branch
- Filter by status, type, from-to branch, date range, priority

### API to call

- `GET /api/v1/stock/requests`

#### Query params

- `type`: `STOCK_REQUEST | TRANSFER_REQUEST`
- `status` (multi): `DRAFT | PENDING | APPROVED | REJECTED | DISPATCH | IN_TRANSIT | PARTIALLY_RECEIVED | RECEIVED | ISSUE_REPORTED | REVOKED | ON_HOLD`
- `fromBranchId`, `toBranchId`
- `fromDate`, `toDate`
- `priority`: `LOW | NORMAL | HIGH | URGENT`
- `pageNo`, `pageSize`

---

## 11.2.1 Stock Request (Branch → Central)

### Screen needs

- Create request with branch, required-by date, purpose
- Add items (each item is a **SKU**), with type-wise quantities
- Show “current branch stock” as reference

### APIs to call

1. **Create draft / submit**

- `POST /api/v1/stock/requests`

#### Body (high level)

- `type`: `STOCK_REQUEST`
- `requestingBranchId`
- `requestToBranchId` = Central branch id
- `priority`, `requiredByDate`, `purpose`
- `items[]`:
  - `inventoryProductId` (**required**)
  - `assetsQty`, `consumableQty`, `resellQty`
  - `itemPurpose` (optional)
- `status`: `DRAFT` or `PENDING`

2. **Fetch current stock (reference column)**

- `GET /api/v1/stock/availability?branchId=<branch>&inventoryProductId=<id>`

---

## 11.2.1.1 Submit Stock Request — Select Recipients (Popup)

### Screen needs

- Choose recipients at HO for notifications

### APIs to call

1. **Load recipient options**

- `GET /api/v1/stock/requests/<requestId>/recipients`

2. **Submit with recipients**

- `POST /api/v1/stock/requests/<requestId>/submit`

Body:

- `recipientUserIds[]`

---

## 11.2.2 View Stock Request (read-only lifecycle)

### APIs to call

- `GET /api/v1/stock/requests/<requestId>`
- `GET /api/v1/stock/requests/<requestId>/timeline`

---

## 11.3 Tab 3 — Received Requests (approval + receipt queue)

### Screen needs

- Queue for approvers to approve/reject/hold and for receivers to receive

### API to call

- `GET /api/v1/stock/inbox`

Query params:

- `mode`: `PENDING_APPROVAL | PENDING_RECEIPT | COMPLETED_TODAY | ALL_HISTORY`
- same filtering options as 11.2 list

---

## 11.3.1 Request Approval Form (Approve / Reject / Hold)

### Screen needs

- Validate central availability per item (A/C/R)
- Approve quantities (full/partial)
- Enter dispatch details (carrier, LR, expected delivery)
- If insufficient stock: alternative source = other branch transfer or purchase order

### APIs to call

1. **Get approval view**

- `GET /api/v1/stock/requests/<requestId>/approval-view`

2. **Approve / reject / hold**

- `POST /api/v1/stock/requests/<requestId>/decision`

Body (high level):

- `decision`: `APPROVE | REJECT | HOLD`
- `approvalType`: `FULL | PARTIAL` (when decision=APPROVE)
- `approvedItems[]`:
  - `inventoryProductId`
  - `approvedAssetsQty`, `approvedConsumableQty`, `approvedResellQty`
  - `alternativeSource`: `NONE | OTHER_BRANCH | PURCHASE_ORDER`
  - `sourceBranchId` (if OTHER_BRANCH)
- `dispatch` (when dispatch starts here):
  - `dispatchDate`, `expectedDeliveryDate`, `carrierId`, `lrNumber`, `trackingId`
  - `assetIdAssignmentMode`: `AUTO | MANUAL` (if relevant)
- `remarks`

### Status behavior (recommended)

- On approval + dispatch: status moves to `IN_TRANSIT`
- Reserved quantities lock at source until received

---

## 11.2.3 Receive Stock / Asset Allocation (Destination Branch)

### Screen needs

- Confirm receipt, verify quantities, verify asset conditions
- Assign assets to Employee / Branch Pool / Quarantine
- Upload receipt photo and confirm

### APIs to call

1. **Load receipt form**

- `GET /api/v1/stock/transfers/<transferId>/receive-view`

2. **Confirm receipt**

- `POST /api/v1/stock/transfers/<transferId>/receive`

Body (high level):

- `receivedDate`, `packageCondition`, `remarks`
- `receivedItems[]`:
  - `inventoryProductId`
  - `receivedAssetsQty`, `receivedConsumableQty`, `receivedResellQty`
- `assetReceipts[]` (for assets):
  - `assetId`
  - `receivedCondition` (`NEW|GOOD|FAIR|DAMAGED|NEEDS_REPAIR`)
  - `assignTo` (`EMPLOYEE|BRANCH_POOL|QUARANTINE`)
  - `employeeId` (if EMPLOYEE)
- `receiptPhotoFileId` (optional)
- `confirmReceipt` (boolean)

3. **Report issue (if needed)**

- `POST /api/v1/stock/transfers/<transferId>/issues`

---

## 11.4 Branch Transfer (Direct transfer or generated from approval)

### Screen needs

- Create TR with from/to branch, type, reference request (optional)
- Add SKU products + type-wise quantities
- Select asset IDs when assets > 0
- Next: dispatch details

### APIs to call

1. **Create transfer draft**

- `POST /api/v1/stock/transfers`

Body (high level):

- `fromBranchId`, `toBranchId`
- `transferType`: `EMERGENCY | REGULAR | SCHEDULED`
- `referenceRequestId` (optional)
- `items[]`:
  - `inventoryProductId`
  - `assetsQty`, `consumableQty`, `resellQty`
  - `assetIds[]` (required when assetsQty > 0)
  - `assetTransferWith` (`REASSIGN_AT_DESTINATION | ASSIGN_TO_EMPLOYEE | BRANCH_POOL`)
  - `destinationEmployeeId` (if ASSIGN_TO_EMPLOYEE)

2. **Dispatch transfer**

- `POST /api/v1/stock/transfers/<transferId>/dispatch`

Body:

- `dispatchDate`, `carrierId`, `lrNumber`, `trackingId`, `expectedDeliveryDate`

3. **View transfer**

- `GET /api/v1/stock/transfers/<transferId>`

4. **Receive transfer**

- Use 11.2.3 receive endpoints (same flow)

---

## End-to-end lifecycle example (from Module 11 spec)

This ASCII timeline is a practical reference for beginner devs to test the UI end-to-end:

```text
15-Feb 09:00  Branch creates request → DRAFT
15-Feb 11:15  Submit → PENDING APPROVAL (notify HO)
15-Feb 15:00  HO approves → APPROVED (reserve qty at source)
16-Feb 16:00  Dispatch → IN TRANSIT (carrier + LR recorded)
18-Feb 11:50  Branch receives + assigns assets → RECEIVED
```

---

## Frontend “must do” checklist (to stay variant-aligned)

- Always store and send **`inventoryProductId`** from Module 10 dropdown.
- Show label as: `productName (variantName) - productCode` for clarity.
- Never aggregate by `productName` unless you intentionally build a **groupKey** report.
- Central entry view must show full SKU code (avoid ambiguous base codes).
