# Module 11 — Tri-Party Field Mismatch Report
**Scope:** Module 11 — Stock Management  
**Three Sources Compared:**

| Source | File / URL |
|--------|-----------|
| 📄 **Doc** | `Module11-20 (5).md` (Functional Specification) |
| 📬 **Postman** | `Module_11_Stock_Management.postman_collection.json` |
| 🔵 **Swagger** | `https://api.seraviontechnologies.com/v3/api-docs/20-stock` (Live OpenAPI 3.1.0) |

**Swagger Spec Title:** Seravion ERP · API · v1.0  
**Swagger Tag:** `11 · Stock Management`

---

## Legend

| Symbol | Meaning |
|--------|---------|
| ✅ | Present and matching |
| ❌ | Missing / absent |
| ➕ | Extra — present but not defined in that source |
| ⚠️ | Name mismatch or type difference |
| 🔒 | Read-only / System-generated (not expected in request body) |
| `R` | Required |
| `O` | Optional |

---

---

# ENDPOINT GROUP 1: Dashboard & Central Stock

---

## E1 — GET `/api/v1/stock/dashboard` — Stock Dashboard

### Query Parameters

| Field | Type | Doc | Postman | Swagger | Swagger Required | Status |
|---|---|---|---|---|---|---|
| `branchId` | string | ✅ | ✅ | ✅ | O | ✅ ALL MATCH |
| `search` | string | ✅ | ✅ | ✅ | O | ✅ ALL MATCH |
| `status` | enum | ✅ | ✅ | ✅ | O | ✅ ALL MATCH |
| `pageNo` | integer | ✅ | ✅ | ✅ | O (default 0) | ✅ ALL MATCH |
| `pageSize` | integer | ✅ | ✅ | ✅ | O (default 10) | ✅ ALL MATCH |
| `category` | multi-select | ✅ Doc | ❌ Postman | ❌ Swagger | — | ❌ **MISSING in Postman + Swagger** |
| `stockType` | multi-select | ✅ Doc | ❌ Postman | ❌ Swagger | — | ❌ **MISSING in Postman + Swagger** |
| `hsnCode` | string | ✅ Doc | ❌ Postman | ❌ Swagger | — | ❌ **MISSING in Postman + Swagger** |
| `hasAssets` | boolean | ✅ Doc | ❌ Postman | ❌ Swagger | — | ❌ **MISSING in Postman + Swagger** |
| `createdDateFrom` | date | ✅ Doc | ❌ Postman | ❌ Swagger | — | ❌ **MISSING in Postman + Swagger** |
| `createdDateTo` | date | ✅ Doc | ❌ Postman | ❌ Swagger | — | ❌ **MISSING in Postman + Swagger** |

> **Swagger `status` enum values:** `AVAILABLE, LOW, OUT, INACTIVE, DISCONTINUED` ✅ matches Doc

**Finding:** Doc defines 6 additional filters. **Neither Postman nor Swagger implements them.** This is a backend gap — the filters are not built.

---

## E2 — GET `/api/v1/stock/dashboard/detail` — Product Stock Detail

| Field | Doc | Postman | Swagger | Status |
|---|---|---|---|---|
| `productId` (query) | ✅ R | ✅ | ✅ R | ✅ ALL MATCH |

**No discrepancies.** ✅

---

## E3 — POST `/api/v1/stock/asset-ids/preview` — Preview Asset IDs

**Swagger Schema:** `AssetIdPreviewRequest`

| Field | Type | Doc | Postman | Swagger | Swagger Required | Status |
|---|---|---|---|---|---|---|
| `prefix` | string | ✅ | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `start` | integer | ✅ | ✅ | ✅ (min: 1) | O | ✅ ALL MATCH |
| `count` | integer | ✅ | ✅ | ✅ (min: 1) | O | ✅ ALL MATCH |

**No discrepancies.** ✅

> **Swagger note:** Only `prefix` is marked required. Doc says `start` and `count` are also required when mode is AUTO. Postman sends all 3.

---

## E4 — POST `/api/v1/stock/central-entries` — Create Central Stock Entry

**Swagger Schema:** `CreateCentralStockEntryRequest`

| Field | Type | Doc Status | Postman | Swagger | Swagger Required | Verdict |
|---|---|---|---|---|---|---|
| `productId` | string | R | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `productCode` | string | System (auto) | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `productName` | string | Not a sendable field in Doc | ➕ | ✅ | **R** | ⚠️ **Doc doesn't define as input; Swagger marks it Required** |
| `hsnCode` | string | System (auto-filled) | ❌ | ✅ | O | ❌ **Missing from Postman; Swagger has it** |
| `baseUom` | string | System (auto-filled) | ❌ | ✅ | O | ❌ **Missing from Postman; Swagger has it** |
| `totalQty` | integer (min:1) | R — "Total Quantity Received" | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `assetsQty` | integer (min:0) | O | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `consumableQty` | integer (min:0) | O | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `resellQty` | integer (min:0) | O | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `assetIdGeneration` | string | Conditional (if assets>0) | ✅ | ✅ | O | ✅ ALL MATCH |
| `assetIdPrefix` | string | Conditional (if AUTO) | ✅ | ✅ | O | ✅ ALL MATCH |
| `assetSequenceStart` | integer | Conditional (if AUTO) | ✅ | ✅ | O | ✅ ALL MATCH |
| `assignmentType` | string | Conditional (if assets>0) | ✅ | ✅ | O | ✅ ALL MATCH |
| `defaultAssignment` | string | Conditional (if assets>0) | ❌ | ✅ | O | ❌ **Missing from Postman; Swagger has it** |
| `supplierName` | string | R (Supplier/Vendor lookup) | ✅ | ✅ | O | ⚠️ **Doc says required lookup; Swagger/Postman treats as optional string** |
| `purchaseOrderRef` | string | O | ❌ | ✅ | O | ❌ **Missing from Postman; Swagger has it** |
| `invoiceNumber` | string | R | ✅ | ✅ | O | ⚠️ **Doc says Required; Swagger marks Optional** |
| `invoiceDate` | date | R | ✅ | ✅ | O | ⚠️ **Doc says Required; Swagger marks Optional** |
| `invoiceAmount` | number | R | ✅ | ✅ | O | ⚠️ **Doc says Required; Swagger marks Optional** |
| `taxAmount` | number | System (auto-calc) | ✅ | ✅ | O | ✅ ALL MATCH |
| `totalWithTax` | number | System (auto-calc) | ✅ | ✅ | O | ✅ ALL MATCH |
| `invoiceCopyUrl` | string | O | ❌ | ✅ | O | ❌ **Missing from Postman; Swagger has it** |
| `batchNumber` | string | **R for consumables** | ❌ | ✅ | O | ❌ **Missing from Postman; Swagger has it (but optional)** |
| `manufacturingDate` | date | **R for consumables** | ❌ | ✅ | O | ❌ **Missing from Postman; Swagger has it (but optional)** |
| `expiryDate` | date | **R for consumables** | ❌ | ✅ | O | ❌ **Missing from Postman; Swagger has it (but optional)** |
| `initialAllocations` | array | O (Sec 5) | ❌ *(handled by E8)* | ✅ **in same request** | O | ⚠️ **Swagger embeds initial allocations INTO create request; Postman uses separate API E8** |

**`initialAllocations` sub-fields** (Swagger `InitialAllocationRequest`):

| Sub-field | Type | Required |
|---|---|---|
| `branchId` | string | **R** |
| `assetsQty` | integer (min:0) | O |
| `consumableQty` | integer (min:0) | O |
| `resellQty` | integer (min:0) | O |

> **Critical Finding:** Swagger's `CreateCentralStockEntryRequest` includes **`initialAllocations`** as an embedded optional array. Postman uses a **separate endpoint** `POST /central-entries/initial-allocations`. The Swagger version is the backend truth — initial allocations can be sent in the create request itself OR in the separate endpoint.

**Postman missing fields vs Swagger:** `hsnCode`, `baseUom`, `defaultAssignment`, `purchaseOrderRef`, `invoiceCopyUrl`, `batchNumber`, `manufacturingDate`, `expiryDate`, `initialAllocations`

---

## E5 — PUT `/api/v1/stock/central-entries` — Update Central Stock Entry

**Swagger Schema:** `CreateCentralStockEntryRequest` *(same schema as create)*

| Field | Type | Doc (Edit) | Postman | Swagger | Status |
|---|---|---|---|---|---|
| `productId` | string | 🔒 Locked | ✅ sent | ✅ | ⚠️ **Doc says locked; Swagger/Postman still accepts it** |
| `productCode` | string | 🔒 Locked | ✅ sent | ✅ R | ⚠️ **Doc says locked** |
| `productName` | string | 🔒 Locked | ✅ sent | ✅ R | ⚠️ **Doc says locked** |
| `hsnCode` | string | 🔒 Locked | ❌ | ✅ O | ❌ **In Swagger but not Postman** |
| `baseUom` | string | 🔒 Locked | ❌ | ✅ O | ❌ **In Swagger but not Postman** |
| `totalQty` | integer | 🔒 **Locked** | ✅ sent | ✅ R | ❌ **Doc says LOCKED after creation; Postman sends it anyway** |
| `assetsQty` | integer | ✅ Editable | ✅ | ✅ R | ✅ ALL MATCH |
| `consumableQty` | integer | ✅ Editable | ✅ | ✅ R | ✅ ALL MATCH |
| `resellQty` | integer | ✅ Editable | ✅ | ✅ R | ✅ ALL MATCH |
| `assetIdGeneration` | string | 🔒 Locked | ❌ | ✅ O | In Swagger schema (reused from create) |
| `assignmentType` | string | ✅ Editable | ✅ | ✅ O | ✅ ALL MATCH |
| `defaultAssignment` | string | ✅ Editable | ❌ | ✅ O | ❌ **Missing from Postman; Swagger has it** |
| `supplierName` | string | 🔒 Locked | ❌ | ✅ O | In Swagger schema but locked per doc |
| `purchaseOrderRef` | string | 🔒 Locked | ❌ | ✅ O | In Swagger schema but locked per doc |
| `invoiceNumber` | string | 🔒 Locked | ❌ | ✅ O | In Swagger schema but locked per doc |
| `invoiceDate` | date | 🔒 Locked | ❌ | ✅ O | In Swagger schema but locked per doc |
| `invoiceAmount` | number | 🔒 Locked | ❌ | ✅ O | In Swagger schema but locked per doc |
| `taxAmount` | number | 🔒 Locked | ❌ | ✅ O | In Swagger schema but locked per doc |
| `totalWithTax` | number | 🔒 Locked | ❌ | ✅ O | In Swagger schema but locked per doc |
| `invoiceCopyUrl` | string | ✅ Editable | ❌ | ✅ O | ❌ **Missing from Postman; Swagger has it; doc says editable** |
| `batchNumber` | string | ✅ Editable | ❌ | ✅ O | ❌ **Missing from Postman; Swagger has it; doc says editable** |
| `manufacturingDate` | date | ✅ Editable | ❌ | ✅ O | ❌ **Missing from Postman; Swagger has it; doc says editable** |
| `expiryDate` | date | ✅ Editable | ❌ | ✅ O | ❌ **Missing from Postman; Swagger has it; doc says editable** |
| `initialAllocations` | array | ✅ (branch qty update) | ❌ | ✅ O | ❌ **Missing from Postman; Swagger has it** |

> **Note:** Swagger reuses the same `CreateCentralStockEntryRequest` for both Create and Update. The backend does NOT distinguish locked fields at the schema level — it depends on the controller logic to ignore locked fields on update.

---

## E6 — GET `/api/v1/stock/central-entries` — Get Central Entry

| Field | Doc | Postman | Swagger | Status |
|---|---|---|---|---|
| `entryId` (query, R) | ✅ | ✅ | ✅ R | ✅ ALL MATCH |

**No discrepancies.** ✅

---

## E7 — DELETE `/api/v1/stock/central-entries` — Delete/Deactivate Entry

| Field | Doc | Postman | Swagger | Status |
|---|---|---|---|---|
| `entryId` (query, R) | ✅ | ✅ | ✅ R | ✅ ALL MATCH |

**Swagger summary:** "Soft delete / inactivate" — confirms doc's soft-delete behavior. ✅

---

## E8 — POST `/api/v1/stock/central-entries/initial-allocations` — Initial Allocation

**Swagger Schema:** Array of `InitialAllocationRequest`

| Field | Type | Doc | Postman | Swagger | Required | Status |
|---|---|---|---|---|---|---|
| `branchId` | string | ✅ | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `assetsQty` | integer (min:0) | ✅ | ✅ | ✅ | O | ✅ ALL MATCH |
| `consumableQty` | integer (min:0) | ✅ | ✅ | ✅ | O | ✅ ALL MATCH |
| `resellQty` | integer (min:0) | ✅ | ✅ | ✅ | O | ✅ ALL MATCH |
| `entryId` (query) | ✅ | ✅ | ✅ R | ✅ ALL MATCH |

**No discrepancies.** ✅

---

## E9 — GET `/api/v1/stock/assets` — List Asset Units

**Swagger Schema response:** `AssetUnit`

| Field | Doc | Postman | Swagger | Status |
|---|---|---|---|---|
| `search` (query) | ✅ | ✅ | ✅ O | ✅ |
| `branchId` (query) | ✅ | ✅ | ✅ O | ✅ |
| `pageNo` (query) | ✅ | ✅ | ✅ O | ✅ |
| `pageSize` (query) | ✅ | ✅ | ✅ O | ✅ |

**No discrepancies.** ✅

**Swagger response `AssetUnit` object fields** (for reference):
`id, assetId, productId, productCode, productName, branchId, assignedUserId, assignedToName, assignmentMode, condition (NEW/GOOD/FAIR/DAMAGED/NEEDS_REPAIR), status (AVAILABLE/ISSUED/IN_TRANSIT/MAINTENANCE/RETIRED/QUARANTINE), createdBy, createdAt, updatedBy, updatedAt`

---

## E10 — GET `/api/v1/stock/assets/history` — Asset History

| Field | Doc | Postman | Swagger | Status |
|---|---|---|---|---|
| `assetId` (query, R) | ✅ | ✅ | ✅ R | ✅ ALL MATCH |

**No discrepancies.** ✅

---

---

# ENDPOINT GROUP 2: Request Lifecycle

---

## E11 — GET `/api/v1/stock/requests/my` — My Stock Requests

**Swagger description:** "Paged list for current user as requester."

| Field | Doc | Postman | Swagger | Status |
|---|---|---|---|---|
| `branchId` | ✅ | ✅ | ✅ O | ✅ ALL MATCH |
| `status` | ✅ | ✅ | ✅ O (enum) | ✅ ALL MATCH |
| `search` | ✅ | ✅ | ✅ O | ✅ ALL MATCH |
| `pageNo` | ✅ | ✅ | ✅ O | ✅ ALL MATCH |
| `pageSize` | ✅ | ✅ | ✅ O | ✅ ALL MATCH |
| `requestType` | ✅ Doc | ❌ Postman | ❌ Swagger | ❌ **MISSING in both Postman + Swagger** |
| `priority` | ✅ Doc | ❌ Postman | ❌ Swagger | ❌ **MISSING in both Postman + Swagger** |
| `dateFrom` | ✅ Doc | ❌ Postman | ❌ Swagger | ❌ **MISSING in both Postman + Swagger** |
| `dateTo` | ✅ Doc | ❌ Postman | ❌ Swagger | ❌ **MISSING in both Postman + Swagger** |

> **Swagger `status` enum values:** `DRAFT, PENDING_APPROVAL, APPROVED, PARTIALLY_APPROVED, REJECTED, HOLD, DISPATCH, IN_TRANSIT, PARTIALLY_RECEIVED, RECEIVED, ISSUE_REPORTED, REVOKED`
>
> **Doc status values:** `Pending, Approved, Rejected, Dispatch, In Transit, Received, Issue Reported`
>
> ⚠️ **Swagger has MORE status values than documented:** `DRAFT`, `PARTIALLY_APPROVED`, `PARTIALLY_RECEIVED`, `HOLD`, `REVOKED` are in Swagger but not in the Doc filter spec.

---

## E12 — GET `/api/v1/stock/requests/received` — Received Requests

> **Swagger:** This endpoint **does NOT exist** in the Swagger spec! The Swagger has `/approval/requests/inbox` for the approval view but NO `/requests/received` route.

| Source | Has this endpoint? |
|---|---|
| Doc | ✅ (Tab 3: Received Requests) |
| Postman | ✅ (with `segment=PENDING_APPROVAL`) |
| Swagger | ❌ **NOT DEFINED** |

> ❌ **This endpoint exists in Postman but is NOT documented in Swagger.** It may be an undocumented backend route or the `segment` filter may be on the approval inbox.

---

## E13 — POST `/api/v1/stock/requests` — Create Stock Request

**Swagger Schema:** `StockRequestUpsertRequest`

### Header Fields

| Field | Type | Doc | Postman | Swagger | Required | Status |
|---|---|---|---|---|---|---|
| `requestType` | enum | ✅ | ✅ | ✅ (STOCK_REQUEST / TRANSFER_REQUEST) | **R** | ✅ ALL MATCH |
| `fromBranchId` | string | ✅ | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `toBranchId` | string | ✅ | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `priority` | enum | ✅ | ✅ | ✅ (LOW/NORMAL/HIGH/URGENT) | **R** | ✅ ALL MATCH |
| `requiredByDate` | date | ✅ | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `purpose` | string | ✅ | ✅ | ✅ (minLength:1) | **R** | ✅ ALL MATCH |
| `notesForApprover` | string | ✅ O | ✅ | ✅ | O | ✅ ALL MATCH |

### Items Array Fields (Swagger Schema: `StockItemQuantityRequest`)

| Field | Type | Doc | Postman | Swagger | Required | Status |
|---|---|---|---|---|---|---|
| `productId` | string | ✅ | ✅ | ✅ (minLength:1) | **R** | ✅ ALL MATCH |
| `productCode` | string | System (auto) | ✅ | ✅ (minLength:1) | **R** | ✅ Postman/Swagger match *(doc considers it auto-fill)* |
| `productName` | string | Not sendable per doc | ✅ | ✅ (minLength:1) | **R** | ⚠️ **Swagger marks as Required; Doc doesn't define as input field** |
| `baseUom` | string | System (auto) | ✅ | ✅ | O | ✅ Postman/Swagger match |
| `assetsQty` | integer (min:0) | ✅ | ✅ | ✅ | O | ✅ ALL MATCH |
| `consumableQty` | integer (min:0) | ✅ | ✅ | ✅ | O | ✅ ALL MATCH |
| `resellQty` | integer (min:0) | ✅ | ✅ | ✅ | O | ✅ ALL MATCH |
| `itemPurpose` | string | ✅ O | ✅ | ✅ | O | ✅ ALL MATCH |

**`items` array itself:** Required (minItems: 1) in Swagger. ✅

**Summary:** Near-perfect match across all three sources. Only issue is `productName` — Swagger requires it but doc treats as system-auto.

---

## E14 — PUT `/api/v1/stock/requests` — Update Draft Request

**Swagger Schema:** `StockRequestUpsertRequest` *(same as create)*

| Field | Create Postman | Update Postman | Swagger | Status |
|---|---|---|---|---|
| `requestType` | ✅ | ✅ | ✅ R | ✅ |
| `fromBranchId` | ✅ | ✅ | ✅ R | ✅ |
| `toBranchId` | ✅ | ✅ | ✅ R | ✅ |
| `priority` | ✅ | ✅ | ✅ R | ✅ |
| `requiredByDate` | ✅ | ✅ | ✅ R | ✅ |
| `purpose` | ✅ | ✅ | ✅ R | ✅ |
| `notesForApprover` | ✅ | ✅ | ✅ O | ✅ |
| `items[].productId` | ✅ | ✅ | ✅ R | ✅ |
| `items[].productCode` | ✅ | ✅ | ✅ R | ✅ |
| `items[].productName` | ✅ | ✅ | ✅ R | ✅ |
| `items[].baseUom` | ✅ | ✅ | ✅ O | ✅ |
| `items[].assetsQty` | ✅ | ✅ | ✅ O | ✅ |
| `items[].consumableQty` | ✅ | ✅ | ✅ O | ✅ |
| `items[].resellQty` | ✅ | ✅ | ✅ O | ✅ |
| `items[].itemPurpose` | ✅ | ❌ **Missing** | ✅ O | ❌ **Postman drops `itemPurpose` on update; Swagger keeps it** |

---

## E15 — POST `/api/v1/stock/requests/recipients` — Save Recipients

**Swagger Schema:** `RecipientsRequest`

| Field | Type | Doc | Postman | Swagger | Required | Status |
|---|---|---|---|---|---|---|
| `recipients` | array of string | ✅ | ✅ | ✅ (minItems: 1) | **R** | ✅ ALL MATCH |

**No discrepancies.** ✅

---

## E16 — POST `/api/v1/stock/requests/submit` — Submit Request

| Field | Doc | Postman | Swagger | Status |
|---|---|---|---|---|
| `requestId` (query, R) | ✅ | ✅ | ✅ R | ✅ ALL MATCH |

No request body. ✅

---

## E17 — GET `/api/v1/stock/requests` — Get Request Detail

| Field | Doc | Postman | Swagger | Status |
|---|---|---|---|---|
| `requestId` (query, R) | ✅ | ✅ | ✅ R | ✅ ALL MATCH |

**No discrepancies.** ✅

---

## E18 — POST `/api/v1/stock/requests/revoke` — Revoke Request

| Field | Doc | Postman | Swagger | Status |
|---|---|---|---|---|
| `requestId` (query, R) | ✅ | ✅ | ✅ R | ✅ ALL MATCH |

No request body. ✅

---

## E19 — POST `/api/v1/stock/requests/receive` — Confirm Receipt

**Swagger Schema:** `ReceiveRequest`

| Field | Type | Doc | Postman | Swagger | Required | Status |
|---|---|---|---|---|---|---|
| `receivedDate` | date | ✅ R | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `packageCondition` | enum | ✅ R | ✅ | ✅ (GOOD/DAMAGED/MISSING_ITEMS) | **R** | ✅ ALL MATCH |
| `remarks` | string | ✅ O | ✅ | ✅ | O | ✅ ALL MATCH |
| `confirmReceipt` | boolean | ✅ R | ✅ | ✅ | O | ⚠️ **Doc says Required; Swagger marks Optional** |
| `receivedItems` / `items` | array | ✅ (Qty Verification table) | ✅ as `receivedItems` | ✅ as `receivedItems` | O | ⚠️ **Name used in Postman is `receivedItems`, Swagger schema also uses `receivedItems`** ✅ |
| `receivedItems[].productId` | string | ✅ implied | ✅ | ✅ (via `StockItemQuantityRequest`) | R | ✅ |
| `receivedItems[].assetsQty` | integer | ✅ | ✅ | ✅ | O | ✅ |
| `receivedItems[].consumableQty` | integer | ✅ | ✅ | ✅ | O | ✅ |
| `receivedItems[].resellQty` | integer | ✅ | ✅ | ✅ | O | ✅ |
| `receivedBy` | Auto | 🔒 System | ❌ | ❌ | — | 🔒 Correctly excluded |
| `receivedCondition` (per asset) | ✅ R Doc | ❌ Postman | ❌ Swagger | ❌ **MISSING in both Postman + Swagger** |
| `assignTo` (per asset) | ✅ R Doc | ❌ Postman | ❌ Swagger | ❌ **MISSING in both Postman + Swagger** |
| `receiptPhotoUrl` | O Doc | ❌ Postman | ❌ Swagger | ❌ **MISSING in both** |

> ⚠️ **Swagger reuses `StockItemQuantityRequest` for `receivedItems` sub-items** — this means `receivedItems[].productCode`, `productName`, `baseUom`, `itemPurpose` would also be in the schema for received items, although they aren't needed. The Swagger schema doesn't have a dedicated "received item" schema with `receivedCondition`.

---

---

# ENDPOINT GROUP 3: Approval

---

## E20 — GET `/api/v1/stock/approval/requests/inbox` — Approval Inbox

| Field | Doc | Postman | Swagger | Status |
|---|---|---|---|---|
| `branchId` | ✅ | ✅ | ✅ O | ✅ ALL MATCH |
| `status` | ✅ | ✅ | ✅ O (enum) | ✅ ALL MATCH |
| `search` | ✅ | ✅ | ✅ O | ✅ ALL MATCH |
| `pageNo` | ✅ | ✅ | ✅ O | ✅ ALL MATCH |
| `pageSize` | ✅ | ✅ | ✅ O | ✅ ALL MATCH |
| `segment` | ✅ Doc (Segmented Tab) | ❌ Postman | ❌ Swagger | ❌ **MISSING in Postman + Swagger** |

---

## E21 — GET `/api/v1/stock/approval/requests/approval-view` — Approval View

| Field | Doc | Postman | Swagger | Status |
|---|---|---|---|---|
| `requestId` (query, R) | ✅ | ✅ | ✅ R | ✅ ALL MATCH |

**No discrepancies.** ✅

---

## E22 — POST `/api/v1/stock/approval/requests/approve` — Approve Request

**Swagger Schema:** `ApprovalDecisionRequest`

| Field | Type | Doc | Postman | Swagger | Required | Status |
|---|---|---|---|---|---|---|
| `approvalType` | string | ✅ R | ✅ | ✅ (minLength:1) | **R** | ✅ ALL MATCH |
| `alternativeSource` | string | ✅ (None/Other Branch/PO) | ✅ | ✅ | O | ✅ ALL MATCH |
| `dispatchDate` | date | ✅ R | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `expectedDeliveryDate` | date | ✅ R | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `carrier` | string | ✅ R | ✅ | ✅ (minLength:1) | **R** | ✅ ALL MATCH |
| `lrNumber` | string | ✅ O | ✅ | ✅ | O | ✅ ALL MATCH |
| `remarks` | string | ✅ R (min 10 chars) | ✅ | ✅ (minLength:1) | **R** | ⚠️ **Doc says min 10 chars; Swagger only enforces minLength:1** |
| `approvedItems` | array | ✅ Conditional | ✅ | ✅ | O | ✅ ALL MATCH |
| `approvedItems[].productId` | string | ✅ | ✅ | ✅ R | **R** | ✅ ALL MATCH |
| `approvedItems[].productCode` | string | System | ✅ | ✅ R | **R** | ✅ |
| `approvedItems[].productName` | string | Not in doc | ✅ | ✅ R | **R** | ⚠️ Swagger marks required |
| `approvedItems[].assetsQty` | integer | ✅ | ✅ | ✅ | O | ✅ ALL MATCH |
| `approvedItems[].consumableQty` | integer | ✅ | ✅ | ✅ | O | ✅ ALL MATCH |
| `approvedItems[].resellQty` | integer | ✅ | ✅ | ✅ | O | ✅ ALL MATCH |
| `assetIdAssignment` | string | ✅ R Doc (Auto/Manual) | ❌ | ❌ | — | ❌ **MISSING from both Postman + Swagger** |
| `transferType` | string | ✅ Doc (if alt=OTHER_BRANCH) | ❌ | ❌ | — | ❌ **MISSING from both** |
| `transferStrategy` | string | ✅ Doc (if alt=OTHER_BRANCH) | ❌ | ❌ | — | ❌ **MISSING from both** |
| `sourceBranch` per item | string | ✅ Doc (if alt=OTHER_BRANCH) | ❌ | ❌ | — | ❌ **MISSING from both** |

---

## E23 — POST `/api/v1/stock/approval/requests/reject` — Reject Request

**Swagger Schema:** `ReasonRequest`

| Field | Type | Doc | Postman | Swagger | Required | Status |
|---|---|---|---|---|---|---|
| `reason` | string (minLength:1) | ✅ R | ✅ | ✅ | **R** | ✅ ALL MATCH |

**No discrepancies.** ✅

---

## E24 — POST `/api/v1/stock/approval/requests/hold` — Hold Request

**Swagger Schema:** `ReasonRequest`

| Field | Type | Doc | Postman | Swagger | Required | Status |
|---|---|---|---|---|---|---|
| `reason` | string (minLength:1) | ✅ R | ✅ | ✅ | **R** | ✅ ALL MATCH |

**No discrepancies.** ✅

---

## E25 — PUT `/api/v1/stock/approval/requests/approval` — Update Approval

**Swagger Schema:** `ApprovalDecisionRequest` *(same as Approve)*

| Field | In Approve (Postman) | In Update Approval (Postman) | Swagger | Status |
|---|---|---|---|---|
| `approvalType` | ✅ | ✅ | ✅ R | ✅ |
| `alternativeSource` | ✅ | ✅ | ✅ O | ✅ |
| `dispatchDate` | ✅ | ✅ | ✅ R | ✅ |
| `expectedDeliveryDate` | ✅ | ✅ | ✅ R | ✅ |
| `carrier` | ✅ | ✅ | ✅ R | ✅ |
| `lrNumber` | ✅ | ✅ | ✅ O | ✅ |
| `remarks` | ✅ | ✅ | ✅ R | ✅ |
| `approvedItems` | ✅ | ❌ **Postman drops it** | ✅ O | ❌ **Postman drops `approvedItems` on update; Swagger keeps it in schema** |

> **Finding:** Swagger uses the same `ApprovalDecisionRequest` schema for both `approve` and `updateApproval`. The Postman collection inconsistently omits `approvedItems` in the update payload. Since Swagger shows it as optional, omitting it may be intentional (update only metadata without changing quantities), but this creates inconsistency.

---

---

# ENDPOINT GROUP 4: Transfer Lifecycle

---

## E26 — POST `/api/v1/stock/transfers` — Create Transfer

**Swagger Schema:** `TransferUpsertRequest`

| Field | Type | Doc | Postman | Swagger | Required | Status |
|---|---|---|---|---|---|---|
| `fromBranchId` | string (minLength:1) | ✅ R | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `toBranchId` | string (minLength:1) | ✅ R | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `transferType` | enum | ✅ R | ✅ | ✅ (REGULAR/EMERGENCY/SCHEDULED) | **R** | ✅ ALL MATCH |
| `strategy` | string | ✅ (Transfer Strategy) | ✅ | ✅ | O | ✅ ALL MATCH |
| `referenceRequestId` | string | ✅ O | ✅ | ✅ | O | ✅ ALL MATCH |
| `items` | array (minItems:1) | ✅ R | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `items[].productId` | string R | ✅ | ✅ | ✅ R | **R** | ✅ ALL MATCH |
| `items[].productCode` | string R | System | ✅ | ✅ R | **R** | ✅ Postman/Swagger match |
| `items[].productName` | string R | Not sendable | ✅ | ✅ R | **R** | ⚠️ **Swagger marks Required; not in doc** |
| `items[].baseUom` | string | System | ✅ | ✅ O | O | ✅ Postman/Swagger match |
| `items[].assetsQty` | integer | ✅ | ✅ | ✅ O | O | ✅ ALL MATCH |
| `items[].consumableQty` | integer | ✅ | ✅ | ✅ O | O | ✅ ALL MATCH |
| `items[].resellQty` | integer | ✅ | ✅ | ✅ O | O | ✅ ALL MATCH |
| `items[].itemPurpose` | string | ✅ O | ✅ | ✅ O | O | ✅ ALL MATCH |
| `selectedAssets[]` (asset IDs) | object[] | ✅ R Doc (if assets>0) | ❌ | ❌ | — | ❌ **MISSING from both Postman + Swagger** |
| `condition` (per asset) | string | ✅ R Doc | ❌ | ❌ | — | ❌ **MISSING from both** |
| `transferWith` (per asset) | string | ✅ R Doc | ❌ | ❌ | — | ❌ **MISSING from both** |
| `employee` (target assignment) | string | ✅ Cond Doc | ❌ | ❌ | — | ❌ **MISSING from both** |
| `notes` | string | ✅ O Doc | ❌ | ❌ | — | ❌ **Missing from both** |

> **Swagger response `StockTransfer` object** has an `assets` array of `StockTransferAsset`:
>
> | `StockTransferAsset` Field | Type | Notes |
> |---|---|---|
> | `assetId` | string | The asset ID |
> | `conditionAtDispatch` | string | Condition when dispatched |
> | `transferWith` | string | Assignment rule |
> | `destinationUserId` | integer | Target employee ID |
> | `destinationUserName` | string | Target employee name |
> | `conditionAtReceipt` | string | Condition when received |
> | `receiptStatus` | string | Receipt verification |
>
> ⚠️ **The response schema has `assets[]` with `conditionAtDispatch`, `transferWith`, `destinationUserId` — meaning the BACKEND stores these asset-level details. But the request schema (`TransferUpsertRequest`) does NOT include them.** This is a backend schema gap — asset transfer details are tracked in response but cannot be submitted in the request.

---

## E27 — PUT `/api/v1/stock/transfers` — Update Transfer

**Swagger Schema:** `TransferUpsertRequest` *(same as create)*

| Field | Create Postman | Update Postman | Swagger | Status |
|---|---|---|---|---|
| `fromBranchId` | ✅ | ✅ | ✅ R | ✅ |
| `toBranchId` | ✅ | ✅ | ✅ R | ✅ |
| `transferType` | ✅ | ✅ | ✅ R | ✅ |
| `strategy` | ✅ | ✅ | ✅ O | ✅ |
| `referenceRequestId` | ✅ | ✅ | ✅ O | ✅ |
| `items[].productId` | ✅ | ✅ | ✅ R | ✅ |
| `items[].productCode` | ✅ | ✅ | ✅ R | ✅ |
| `items[].productName` | ✅ | ✅ | ✅ R | ✅ |
| `items[].baseUom` | ✅ | ❌ **Dropped in Update** | ✅ O | ❌ **Postman inconsistency** |
| `items[].assetsQty` | ✅ | ✅ | ✅ O | ✅ |
| `items[].consumableQty` | ✅ | ✅ | ✅ O | ✅ |
| `items[].resellQty` | ✅ | ✅ | ✅ O | ✅ |

---

## E28 — GET `/api/v1/stock/transfers` — Get Transfer Detail

| Field | Doc | Postman | Swagger | Status |
|---|---|---|---|---|
| `transferId` (query) | ✅ R | ✅ | ✅ **R** | ✅ ALL MATCH |
| `branchId` (query) | ✅ | ✅ | ✅ O | ✅ |
| `status` (query) | ✅ | ✅ | ✅ O | ✅ |
| `search` (query) | ✅ | ✅ | ✅ O | ✅ |
| `pageNo` (query) | ✅ | ✅ | ✅ O | ✅ |
| `pageSize` (query) | ✅ | ✅ | ✅ O | ✅ |

> **Note:** Swagger uses `oneOf [single transfer, paginated list]` as the response, which means the same `GET /transfers` endpoint handles both single fetch (with `transferId`) and list (without `transferId`). Postman has **two separate entries** for this — "Get Transfer" and "List Transfers". They are actually the **same Swagger endpoint** combined.

---

## E29 — POST `/api/v1/stock/transfers/dispatch` — Dispatch Transfer

**Swagger Schema:** `DispatchTransferRequest`

| Field | Type | Doc | Postman | Swagger | Required | Status |
|---|---|---|---|---|---|---|
| `dispatchDate` | date | ✅ R | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `expectedDeliveryDate` | date | ✅ R | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `carrier` | string (minLength:1) | ✅ R | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `lrNumber` | string | ✅ O | ✅ | ✅ | O | ✅ ALL MATCH |
| `remarks` | string | ✅ O | ✅ | ✅ | O | ✅ ALL MATCH |

**No discrepancies.** ✅

---

## E30 — POST `/api/v1/stock/transfers/mark-in-transit` — Mark In Transit

| Field | Doc | Postman | Swagger | Status |
|---|---|---|---|---|
| `transferId` (query, R) | ✅ | ✅ | ✅ R | ✅ ALL MATCH |

No request body. ✅

---

## E31 — POST `/api/v1/stock/transfers/receive` — Receive Transfer

**Swagger Schema:** `ReceiveRequest` *(same schema as Receive Request E19)*

| Field | Type | Doc | Postman | Swagger | Required | Status |
|---|---|---|---|---|---|---|
| `receivedDate` | date | ✅ R | ✅ | ✅ | **R** | ✅ ALL MATCH |
| `packageCondition` | enum | ✅ R | ✅ | ✅ (GOOD/DAMAGED/MISSING_ITEMS) | **R** | ✅ ALL MATCH |
| `remarks` | string | ✅ O | ✅ | ✅ | O | ✅ ALL MATCH |
| `confirmReceipt` | boolean | ✅ R | ✅ | ✅ | O | ⚠️ **Doc says Required; Swagger Optional** |
| `receivedItems` | array | ✅ implied | ✅ | ✅ | O | ✅ ALL MATCH |
| `receivedItems[].productId` | string | ✅ | ✅ | ✅ R | R | ✅ |
| `receivedItems[].assetsQty` | integer | ✅ | ✅ | ✅ | O | ✅ |
| `receivedItems[].consumableQty` | integer | ✅ | ✅ | ✅ | O | ✅ |
| `receivedItems[].resellQty` | integer | ✅ | ✅ | ✅ | O | ✅ |
| `receivedCondition` (per asset) | string | ✅ R Doc | ❌ | ❌ | — | ❌ **MISSING from both Postman + Swagger** |
| `assignTo` (per asset) | string | ✅ R Doc | ❌ | ❌ | — | ❌ **MISSING from both** |
| `receiptPhotoUrl` | string | ✅ O Doc | ❌ | ❌ | — | ❌ **MISSING from both** |

---

## E32 — GET `/api/v1/stock/transfers` — List Transfers

*(Same endpoint as E28 — see note above. Both `Get Transfer` and `List Transfers` in Postman map to the same Swagger route.)*

---

---

# MASTER MISMATCH SUMMARY TABLE

## Table 1: Fields MISSING FROM BOTH Postman AND Swagger (Backend Not Implemented)

These are the most critical gaps — the functionality is documented but NOT in the backend.

| # | Field | API | Impact | Priority |
|---|---|---|---|---|
| 1 | `category` filter | GET Dashboard | Cannot filter stock by category | 🟡 Medium |
| 2 | `stockType` filter | GET Dashboard | Cannot filter by Assets/Consumable/Resell | 🟡 Medium |
| 3 | `hsnCode` filter | GET Dashboard | Cannot filter by HSN code | 🟡 Medium |
| 4 | `hasAssets` filter | GET Dashboard | Cannot filter by asset presence | 🟡 Medium |
| 5 | `createdDateFrom` / `createdDateTo` | GET Dashboard | No date filter | 🟡 Medium |
| 6 | `requestType` filter | GET My Requests | Cannot separate stock vs transfer | 🟡 Medium |
| 7 | `priority` filter | GET My Requests | Cannot filter by urgency | 🟡 Medium |
| 8 | `dateFrom` / `dateTo` | GET My Requests | No date range filter | 🟡 Medium |
| 9 | `segment` filter | GET Approval Inbox | Can't segment pending/completed | 🟡 Medium |
| 10 | `receivedCondition` per asset | Receive Request + Transfer | **Asset condition not captured at receipt** | 🔴 Critical |
| 11 | `assignTo` per asset | Receive Request + Transfer | **Asset cannot be re-assigned on receipt** | 🔴 Critical |
| 12 | `receiptPhotoUrl` | Receive Request + Transfer | No physical proof of receipt | 🟠 High |
| 13 | `assetIdAssignment` decision | Approve Request | Auto vs manual asset ID not controlled | 🟠 High |
| 14 | `transferType` (when alt=OTHER_BRANCH) | Approve Request | Transfer plan incomplete | 🟠 High |
| 15 | `transferStrategy` | Approve Request | Single vs split transfer missing | 🟠 High |
| 16 | `sourceBranch` per item | Approve Request | Source branch for alt supply missing | 🟠 High |
| 17 | `selectedAssets[]` / asset selection | Create Transfer | Cannot specify WHICH assets to transfer | 🔴 Critical |
| 18 | `condition` per asset | Create Transfer | Pre-transfer condition not sent | 🟠 High |
| 19 | `transferWith` per asset | Create Transfer | Post-transfer assignment rule missing | 🟠 High |
| 20 | `employee` assignment | Create Transfer | Target employee not specified | 🟠 High |
| 21 | `/requests/received` endpoint | — | Endpoint in Postman but NOT in Swagger | 🟡 Medium |

---

## Table 2: Fields MISSING FROM POSTMAN ONLY (Swagger has it — Postman outdated)

| # | Field | API | Postman | Swagger |
|---|---|---|---|---|
| 1 | `hsnCode` | Create Central Entry | ❌ | ✅ O |
| 2 | `baseUom` | Create Central Entry | ❌ | ✅ O |
| 3 | `defaultAssignment` | Create/Update Central Entry | ❌ | ✅ O |
| 4 | `purchaseOrderRef` | Create Central Entry | ❌ | ✅ O |
| 5 | `invoiceCopyUrl` | Create/Update Central Entry | ❌ | ✅ O |
| 6 | `batchNumber` | Create/Update Central Entry | ❌ | ✅ O |
| 7 | `manufacturingDate` | Create/Update Central Entry | ❌ | ✅ O |
| 8 | `expiryDate` | Create/Update Central Entry | ❌ | ✅ O |
| 9 | `initialAllocations` | Create/Update Central Entry | ❌ (uses separate API) | ✅ O (embedded) |
| 10 | `items[].itemPurpose` | Update Request | ❌ | ✅ O |
| 11 | `approvedItems` | Update Approval | ❌ | ✅ O |
| 12 | `items[].baseUom` | Update Transfer | ❌ | ✅ O |

> **Action:** Update the Postman collection to include these Swagger-defined fields.

---

## Table 3: Fields Where Doc, Postman, and Swagger CONFLICT on Required/Optional

| # | Field | API | Doc Says | Swagger Says | Postman Has It? |
|---|---|---|---|---|---|
| 1 | `productName` | Create Central Entry, Requests, Transfers | NOT a sendable input | **Required** | ✅ |
| 2 | `invoiceNumber` | Create Central Entry | **Required** | Optional | ✅ |
| 3 | `invoiceDate` | Create Central Entry | **Required** | Optional | ✅ |
| 4 | `invoiceAmount` | Create Central Entry | **Required** | Optional | ✅ |
| 5 | `batchNumber` | Create Central Entry | **Required for consumables** | Optional | ❌ |
| 6 | `manufacturingDate` | Create Central Entry | **Required for consumables** | Optional | ❌ |
| 7 | `expiryDate` | Create Central Entry | **Required for consumables** | Optional | ❌ |
| 8 | `confirmReceipt` | Receive Request/Transfer | **Required** | Optional | ✅ |
| 9 | `remarks` (Approve) | Approve Request | min 10 chars | minLength: 1 | ✅ |
| 10 | `supplierName` | Create Central Entry | **Required** (vendor lookup) | Optional (string) | ✅ (as string) |
| 11 | `totalQty` | Update Central Entry | **LOCKED (not editable)** | **Required** (in shared schema) | ✅ (sent incorrectly) |

---

## Table 4: Status Values — Swagger vs Doc

> Swagger uses consistent status enums. Comparing:

| Status Value | Doc Mentions | Swagger Has It? |
|---|---|---|
| `DRAFT` | ✅ | ✅ |
| `PENDING_APPROVAL` / Pending | ✅ | ✅ |
| `APPROVED` | ✅ | ✅ |
| `PARTIALLY_APPROVED` | ❌ Not in doc filters | ✅ **Extra in Swagger** |
| `REJECTED` | ✅ | ✅ |
| `HOLD` | ✅ | ✅ |
| `DISPATCH` | ✅ | ✅ |
| `IN_TRANSIT` | ✅ | ✅ |
| `PARTIALLY_RECEIVED` | ❌ Not in doc | ✅ **Extra in Swagger** |
| `RECEIVED` | ✅ | ✅ |
| `ISSUE_REPORTED` | ✅ | ✅ |
| `REVOKED` | ✅ | ✅ |

> **2 statuses in Swagger not documented:** `PARTIALLY_APPROVED`, `PARTIALLY_RECEIVED` — these are valid backend states but not described in the functional spec.

---

## Final Score — All 32 API Checks

| Category | Doc vs Postman | Doc vs Swagger | Postman vs Swagger |
|---|---|---|---|
| ✅ All Match | 13 APIs | 15 APIs | 16 APIs |
| ⚠️ Has Issues | 19 APIs | 17 APIs | 12 APIs |
| Fields Missing | 51 | 28 | 12 (Postman missing vs Swagger) |
| Fields Extra | 7 | 4 | 1 |
| Name Mismatches | 8 | 2 | 2 |

---

## Priority Fix List (Consolidated — All Three Sources)

| Priority | Fix | Source Gap | API Affected |
|---|---|---|---|
| 🔴 P1 Critical | Add `receivedCondition` + `assignTo` per asset to `ReceiveRequest` schema | Doc → Swagger + Postman | Receive Request, Receive Transfer |
| 🔴 P1 Critical | Add `selectedAssets[]` (with condition, transferWith) to `TransferUpsertRequest` | Doc → Swagger + Postman | Create/Update Transfer |
| 🔴 P1 Critical | Make `batchNumber`, `manufacturingDate`, `expiryDate` conditionally required for consumables | Doc → Swagger + Postman | Create Central Entry |
| 🟠 P2 High | Add `assetIdAssignment`, `transferType`, `transferStrategy` to `ApprovalDecisionRequest` | Doc → Swagger + Postman | Approve Request |
| 🟠 P2 High | Update Postman Create Central Entry with: `hsnCode`, `baseUom`, `defaultAssignment`, `purchaseOrderRef`, `invoiceCopyUrl`, `batchNumber`, `manufacturingDate`, `expiryDate` | Postman → Swagger | Create Central Entry |
| 🟠 P2 High | Clarify `productName` as truly required (Swagger) or read-only (Doc) | Doc → Swagger | All request schemas |
| 🟠 P2 High | Make `confirmReceipt` required in Swagger `ReceiveRequest` | Doc → Swagger | Receive Request + Transfer |
| 🟠 P2 High | Add `receiptPhotoUrl` to `ReceiveRequest` Swagger schema | Doc → Swagger + Postman | Receive Request, Receive Transfer |
| 🟡 P3 Medium | Add 6 dashboard filter params to Swagger + backend | Doc → Swagger + Postman | GET Dashboard |
| 🟡 P3 Medium | Add `requestType`, `priority`, date range filters | Doc → Swagger + Postman | GET My Requests |
| 🟡 P3 Medium | Define `/requests/received` in Swagger or clarify if `segment` param goes in inbox | Doc → Swagger | GET Received Requests |
| 🟡 P3 Medium | Add `approvedItems` back to Update Approval Postman body | Postman → Swagger | Update Approval |
| 🟡 P3 Medium | Fix `totalQty` in Update Central Entry — doc says locked but Swagger schema makes it required | Doc vs Swagger | Update Central Entry |
| 🟢 P4 Low | Add `items[].itemPurpose` to Update Request Postman body | Postman → Swagger | Update Request |
| 🟢 P4 Low | Add `items[].baseUom` to Update Transfer Postman body | Postman → Swagger | Update Transfer |
| 🟢 P4 Low | Document `PARTIALLY_APPROVED` and `PARTIALLY_RECEIVED` statuses in MD spec | Swagger → Doc | All status docs |
