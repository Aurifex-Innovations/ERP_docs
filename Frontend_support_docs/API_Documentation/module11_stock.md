# Module 11 — Stock Management (Combined Flow-wise Guide)

This is the **combined** and **flow-wise** document for:

- `docs/module-11-stock-management-frontend-api.md` (full API workflow contract)
- `docs/module-11-stock-management-frontend-guide.md` (beginner-friendly screen mapping)

It explains, in simple English:

- How **Head Office / Central users (CEO and upper roles)** manage Central stock (add/edit + logs)
- How **Branch users** see branch stock, raise request, and receive after approval
- How **Transfers** work (direct + generated from approval with OTHER_BRANCH)
- Screen-wise: **which endpoint to call**, query params, payload notes
- **Enums and allowed values**
- **Conditional UI filters** (RBAC + CENTRAL visibility)

Where rules feel strict (“hard lines”), an easy Gujarati explanation is provided.

---

## 0) Quick glossary (simple)

- **Central**: Head Office warehouse branch (id usually `CENTRAL`)
- **Branch**: Any other branch id like `BR-001`, `BR-002`
- **SKU / Variant**: In Module 10 refactor, **each variant is a SKU row** and has its own id
- **Stock types**:
  - `ASSET`: unit-based, each item can have an `assetId`
  - `CONSUMABLE`: bulk quantity
  - `RESELL`: quantity for sale
- **Reserved**: quantity “locked” for a request (note: some parts are “recommended” and not fully automated in backend)
- **In-Transit**: dispatched and moving between branches

---

## 1) Must-follow: Product is always SKU-level (Module 10 alignment)

### Product dropdown (used everywhere)

- **Endpoint**: `GET /api/v1/inventory-products/dropdown`
- **Rule**: Each dropdown option is a **SKU/variant**
- **Frontend must store & send**:
  - `productId` / `inventoryProductId` (your backend uses both naming styles across docs; treat as **SKU id**)
  - `productCode` (display only)
  - `productName`, `variantName`, `baseUom`, `hsnCode`

**Hard line (why)**:

- Do **not** use `productCode` as database key.
- Always send the SKU id (`productId` / `inventoryProductId`) in stock APIs.

**Gujarati (easy)**:

- **ProductCode** matra display mate chhe. DB ma actual mapping **SKU id** par chalse.  
  એટલે request/stock ma hamesha **productId / inventoryProductId** j moklo.

---

## 2) RBAC + Filters (CENTRAL visibility)

### 2.1 Branch filter behavior (recommended UI rule)

In **Stock Dashboard** and **lists**, show filters based on permission:

- **If user has “Central Stock Add” permission** (example: **CEO role** and upper roles):
  - Show **extra dropdown**: `Scope Enum = CENTRAL | BRANCH`
  - When `CENTRAL` selected, default branch becomes `CENTRAL`
  - When `BRANCH` selected, show branch dropdown (non-central)

- **All other users**:
  - Do **not** show the CENTRAL scope enum
  - Show only **branch dropdown(s)** they can access (often “My Branch” default)

### 2.2 Suggested enums for UI filter (frontend-only enum)

This scope enum is UI-side unless you implement it in backend:

- `CENTRAL`
- `BRANCH`

If you implement backend param, a safe name is `viewScope`, but the contract already supports plain `branchId`.

---

## 3) Common enums (backend)

From the contract:

- **RequestStatus**:
  - `DRAFT`, `PENDING_APPROVAL`, `APPROVED`, `PARTIALLY_APPROVED`, `REJECTED`, `HOLD`,
  - `DISPATCH`, `IN_TRANSIT`, `PARTIALLY_RECEIVED`, `RECEIVED`,
  - `ISSUE_REPORTED`, `REVOKED`
- **RequestType**:
  - `STOCK_REQUEST`, `INTERNAL_TRANSFER`, `ASSET_ISSUE`
  - (UI docs also mention `TRANSFER_REQUEST` — keep FE mapping consistent with your backend actual enum)
- **RequestPriority**: `LOW`, `NORMAL`, `HIGH`, `URGENT`
- **TransferType** (contract): `REGULAR`, `EMERGENCY`, `RETURN`
  - (guide mentions `SCHEDULED` too; verify with backend, but document both as “spec vs contract”)
- **ReceiptCondition**: `GOOD`, `DAMAGED` (contract)
  - Transfer receive also uses: `MISSING_ITEMS` (documented in transfer receive)
- **StockAvailabilityStatus**: `AVAILABLE`, `OUT`
- **AssetCondition** (receive): `NEW`, `GOOD`, `FAIR`, `DAMAGED`, `NEEDS_REPAIR`

---

## 4) Screen 11.1 — Stock Dashboard (table)

### What user sees

- A single table with SKU-wise stock (assets/consumables/resell + totals)
- Filters: branch, search, status
- Optional columns: `inTransitQty`, `reservedQty`

### API

- **Endpoint**: `GET /api/v1/stock/dashboard?pageNo=0&pageSize=10&branchId=&search=&status=`
  - `branchId` optional
  - `status` optional: `AVAILABLE | OUT`
  - `search` optional

### Row actions (recommended)

- **View** → Product Stock Ledger (movement log)
- **Central Edit** → only when:
  - row belongs to Central entry context **and**
  - RBAC allows edit

---

## 5) Screen 11.1.A — Product Stock Ledger (movement log)

This is the **log-wise view** the user requested.

### What user sees

- “Stock movement” entries for one SKU: central entry receipt, requests, transfers

### APIs (guide)

- Summary: `GET /api/v1/stock/ledger/summary?inventoryProductId=<id>&branchId=<optional>`
- List: `GET /api/v1/stock/ledger`
  - Query: `inventoryProductId` required
  - Optional: `entryTypes=CENTRAL_ENTRY|STOCK_REQUEST|BRANCH_TRANSFER`, `direction=IN|OUT`, date range, paging

### Linking from ledger → detail screens

- `CENTRAL_ENTRY` → Central Stock Entry view
- `STOCK_REQUEST` → Stock Request view
- `BRANCH_TRANSFER` → Transfer view

---

## 6) Central flow (CEO / Head Office) — Add Central Stock, Edit, View, Logs

### 6.1 Flow diagram (Central procurement → stock updated)

```text
Central user (CEO / HO)                                System
┌────────────────────────────┐
│ 11.1 Add to Central Stock  │
│ pick SKU + qty split       │
└──────────────┬─────────────┘
               │ POST /central-entries
               ▼
┌────────────────────────────┐
│ Central Entry created       │
│ (CSTK-xxxx)                 │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ Dashboard qty updated       │
│ Ledger logs created         │
└────────────────────────────┘
```

### 6.2 Screen 11.1.1 — Add to Central Stock (form)

#### Key UX rule

When SKU selected, show **Current Central Stock** in read-only section.

#### APIs

1. Load current central stock (display-only):

- `GET /api/v1/stock/central-stock-level?productId=...`

2. (Optional) Preview asset IDs:

- `POST /api/v1/stock/asset-ids/preview`

3. Create entry:

- `POST /api/v1/stock/central-entries`

#### Create validation “hard line”

- `assetsQty + consumableQty + resellQty = totalQty`

**Gujarati**:

- Total qty ne 3 part ma split karo. 3 no sum barabar **totalQty** j hovo joiye.  
  Nahi to backend error aapse (balance mismatch).

#### Invoice copy note (hard line)

- Invoice is accepted as **Base64** (`invoiceCopy`) with max decoded size **5MB**
- Response does not return Base64 back; it returns metadata + download URL

**Gujarati**:

- Invoice upload ma base64 moklo. Response ma file data back nathi avtu;  
  tamne download endpoint thi file levi pade.

### 6.3 Screen 11.1.2 — Edit Central Stock Entry

#### API

- Load: `GET /api/v1/stock/central-entries?entryId=...`
- Update: `PUT /api/v1/stock/central-entries?entryId=...`

#### Locked fields (contract)

Backend rejects edits to:

- `totalQty`, product identity fields, many procurement fields, and asset ID config (`assetIdPrefix`, etc.)
  Editable: split fields + assignment + invoice replacement + batch/expiry + optional `initialAllocations`

**Hard line (why)**:
This preserves audit integrity for procurement and prevents changing the “receipt truth”.

**Gujarati**:

- Central entry ek “purchase receipt” jvi chhe. Ekvar create thay pachi  
  **totalQty/product/invoice details** badliye to audit kharab thai.  
  Tethi backend ae fields lock kare chhe.

#### Log-wise behavior (important for audit)

On successful update, backend writes a movement log row with an action like:

- `CENTRAL_STOCK_UPDATED` (remarks summarize changed fields)

### 6.4 Screen 11.1.3 — View Central Stock Entry (read-only)

- `GET /api/v1/stock/central-entries?entryId=...`
- Invoice download:
  - `GET /api/v1/stock/central-entries/invoice-copy?entryId=...`

### 6.5 (Optional) Initial allocation from Central → branches

Two options:

- Send `initialAllocations` inside `POST /central-entries`
- Or do it later:
  - `POST /api/v1/stock/central-entries/initial-allocations?entryId=...`

---

## 7) Branch flow — Branch stock view, request, approval, receive

### 7.1 What a normal branch user can do (scenario)

- Open Dashboard and see **their branch stock**
- Create a **Stock Request** to Central (or a transfer request if supported)
- Track status until approved/dispatched
- When goods arrive: **Receive** and confirm

### 7.2 Branch user views branch stock (Dashboard)

- `GET /api/v1/stock/dashboard?branchId=<theirBranchId>&...`

RBAC rule reminder:

- Normal user UI should not expose CENTRAL scope filter.

### 7.3 End-to-end scenario (Branch → Central → Branch receive)

This is the simplest “happy path” for QA and onboarding.

```text
Branch User                           HO Approver / Central                Branch User
┌───────────────┐                     ┌──────────────────────┐            ┌────────────────┐
│ Create SR     │── submit ──────────▶│ Approve + dispatch    │── goods ──▶│ Receive & close│
│ (draft/ready) │                     │ (or hold/reject)      │            │ (confirmReceipt)│
└───────────────┘                     └──────────────────────┘            └────────────────┘
```

**Exact API order (recommended)**

- Branch creates draft/ready:
  - `POST /api/v1/stock/requests?draft=true|false`
- Branch selects recipients + submits:
  - `GET /api/v1/stock/requests/recipient-candidates`
  - `POST /api/v1/stock/requests/submit?requestId=...` with `{ "notifyAll": true }` (or specific recipients)
- HO sees it in inbox:
  - `GET /api/v1/stock/requests/received?segment=PENDING_APPROVAL...`
- HO opens approval view:
  - `GET /api/v1/stock/approval/requests/approval-view?requestId=...`
- HO approves (or hold/reject):
  - `POST /api/v1/stock/approval/requests/approve?requestId=...`
- Branch tracks request:
  - `GET /api/v1/stock/requests?requestId=...`
- Branch receives:
  - `POST /api/v1/stock/requests/receive?requestId=...`

---

## 8) Screen 11.2 — My Requests (Branch user)

### Table/list

- Contract endpoint:
  - `GET /api/v1/stock/requests/my?pageNo=0&pageSize=10`
  - With filters like `statuses=...`, `branchAnyId`, `priority`, date range, search

### Create (11.2.1 Stock Request)

- `POST /api/v1/stock/requests?draft=false`
  - Use `draft=true` for “Save Draft”
  - Use `draft=false` for ready-to-submit validations

#### “Hard line” validations for submit-ready (contract)

- `purpose` trimmed length **≥ 20**
- `requiredByDate` **≥ tomorrow**
- at least one line with qty > 0

**Gujarati**:

- Submit mate purpose ne minimum 20 character rakho.  
  requiredByDate “aaj” na hovu joiye; **aavti kale** thi start.  
  Ane least 1 item ma qty > 0 hovi joiye.

### Recipient picker + submit (11.2.1.1)

- Candidates:
  - `GET /api/v1/stock/requests/recipient-candidates`
- Submit:
  - `POST /api/v1/stock/requests/submit?requestId=...`
  - Body optional:
    - `{ "notifyAll": true }` OR `{ "notifyAll": false, "recipients": ["a@x.com"] }`

### View request details (read-only lifecycle)

- `GET /api/v1/stock/requests?requestId=...`

---

## 9) Screen 11.3 — Received Requests (Central/Approver inbox)

This is the “receive request flow and approval” part.

### 9.1 Inbox list

Two patterns exist in docs; use the contract ones for consistency:

- `GET /api/v1/stock/requests/received?pageNo=0&pageSize=10`
  - `segment=PENDING_APPROVAL | PENDING_RECEIPT | COMPLETED_TODAY | ALL_HISTORY`
  - `workflows=APPROVAL | RECEIPT` (unions statuses; precedence over segment)

**Hard line (visibility)**

- Only requests where `sentTo` contains current user email are visible.
- Draft is never shown to approvers.

**Gujarati**:

- Approver inbox ma request tabhi j avse jyare user ae submit kari ne  
  recipients set karye hoy. Draft ma request “invisible” rahe chhe.

### 9.2 Approval view (open the detail for approve screen)

- `GET /api/v1/stock/approval/requests/approval-view?requestId=...`
  - (Contract also notes `GET /requests?requestId=...` has same JSON shape for read-only)

### 9.3 Approve / Reject / Hold

- Approve:
  - `POST /api/v1/stock/approval/requests/approve?requestId=...`
- Reject:
  - `POST /api/v1/stock/approval/requests/reject?requestId=...` (reason min 10 chars)
- Hold:
  - `POST /api/v1/stock/approval/requests/hold?requestId=...` (reason min 10 chars)

**Hard line (remarks min length)**

- Approve remarks min 10 chars; hold/reject reason min 10 chars.

**Gujarati**:

- Backend empty/short remark allow nathi karto.  
  Minimum 10 character no reason/remarks jaruri chhe (audit clarity mate).

### 9.4 Approval edit (after approval)

- `PUT /api/v1/stock/approval/requests/approval?requestId=...`
  - Draft mode: `"saveAsDraft": true` (remarks ≥ 3)
  - Finalize: `dispatchDate`, `expectedDeliveryDate`, `carrier` required (remarks ≥ 10)

---

## 10) Receive flow (Destination branch) — after dispatch/in-transit

There are two receive APIs in docs: **request receive** and **transfer receive**.

### 10.1 Receive a Stock Request (simpler receive)

- `POST /api/v1/stock/requests/receive?requestId=...`
  Body:
- `receivedDate` required
- `packageCondition`: `GOOD | DAMAGED | MISSING_ITEMS`
- `confirmReceipt` must be `true`

Status outcome:

- `GOOD` → `RECEIVED`
- `DAMAGED` / `MISSING_ITEMS` → `ISSUE_REPORTED`

### 10.2 Receive a Transfer (full receive with asset assignment)

- `POST /api/v1/stock/transfers/receive?transferId=...`

Supports:

- receipt photo (Base64, 5MB)
- optional `receivedItems[]` match check
- `assetReceipts[]` with:
  - `receivedCondition`: `NEW|GOOD|FAIR|DAMAGED|NEEDS_REPAIR`
  - `assignmentType`: `EMPLOYEE|BRANCH_POOL|QUARANTINE`

**Hard line (confirmReceipt must be true)**
Backend requires a positive confirmation to close the receive step.

**Gujarati**:

- Receive finalize karva mate `confirmReceipt=true` jaruri chhe.  
  Nahi to backend “confirm nathi” em samji ne reject kare chhe.

---

## 11) Branch Transfer flow (11.4) — direct transfer + approval-generated transfer

Transfers are used when:

- HO decides alternate source: `OTHER_BRANCH`, or
- You do a direct branch-to-branch movement.

### 11.1 Flow diagram (approval → linked transfer → dispatch → receive)

```text
Branch SR submitted
      │
      ▼
Central approves with alternativeSource=OTHER_BRANCH
      │ (system creates/updates transfer draft)
      ▼
Transfer DRAFT (TR-xxxx)  ──► operator completes asset lines (if needed)
      │
      ▼
Dispatch transfer ──► In transit ──► Receive at destination (assign assets)
```

### 11.2 Transfer APIs (contract)

- Create: `POST /api/v1/stock/transfers`
- Update: `PUT /api/v1/stock/transfers?transferId=...`
- View: `GET /api/v1/stock/transfers?transferId=...`
- Dispatch: `POST /api/v1/stock/transfers/dispatch?transferId=...`
- Mark in transit: `POST /api/v1/stock/transfers/mark-in-transit?transferId=...`
- Receive: `POST /api/v1/stock/transfers/receive?transferId=...`
- List: `GET /api/v1/stock/transfers?pageNo=0&pageSize=10&branchId=&status=&search=`

### 11.3 Transfer “hard lines”

- `fromBranchId` ≠ `toBranchId`
- Each line qty sum > 0
- Source stock must be sufficient
- If assets are transferred, `assetLines` count must match assets qty before dispatch

**Gujarati**:

- Transfer ma from/to same nathi hovu joiye.  
  Qty zero hoy to line invalid.  
  Asset transfer ma actual assetId select karva pade; count match na thay to dispatch block thase.

---

## 12) Screen-wise summary (quick mapping)

### Dashboard

- `GET /stock/dashboard`

### Product stock detail (dashboard row drilldown)

- `GET /stock/dashboard/detail?productId=...`

### Central Stock Entry

- Current central stock: `GET /stock/central-stock-level?productId=...`
- Create: `POST /stock/central-entries`
- View: `GET /stock/central-entries?entryId=...`
- Edit: `PUT /stock/central-entries?entryId=...`
- Delete/inactivate: `DELETE /stock/central-entries?entryId=...`
- Invoice download: `GET /stock/central-entries/invoice-copy?entryId=...`
- Initial allocations: `POST /stock/central-entries/initial-allocations?entryId=...`

### Requests

- My list: `GET /stock/requests/my`
- Create: `POST /stock/requests?draft=...`
- Update: `PUT /stock/requests?requestId=...&draft=...`
- Submit: `POST /stock/requests/submit?requestId=...`
- Recipient candidates: `GET /stock/requests/recipient-candidates`
- Save recipients: `POST /stock/requests/recipients?requestId=...`
- Detail: `GET /stock/requests?requestId=...`
- Revoke: `POST /stock/requests/revoke?requestId=...`
- Receive (simple): `POST /stock/requests/receive?requestId=...`

### Approval (HO inbox)

- Received list: `GET /stock/requests/received`
- Approval view: `GET /stock/approval/requests/approval-view?requestId=...`
- Approve: `POST /stock/approval/requests/approve?requestId=...`
- Reject: `POST /stock/approval/requests/reject?requestId=...`
- Hold: `POST /stock/approval/requests/hold?requestId=...`
- Update approval: `PUT /stock/approval/requests/approval?requestId=...`

### Transfers

- Create/update/get/list/dispatch/in-transit/receive: see section 11.2

---

## 13) Notes on naming mismatches (to avoid frontend bugs)

Across docs you will see both:

- `inventoryProductId` (guide)
- `productId` (contract)

Treat both as the **same idea**: “SKU/variant primary id from Module 10”.

**Recommendation for frontend state**

- Keep one internal key: `skuId`
- Map it to payload key expected by endpoint you call (`productId` vs `inventoryProductId`)

**Gujarati**

- Naming alag chhe pan concept same chhe: **SKU id**.  
  FE ma `skuId` rakhine endpoint pramane key map karo.
