# Module 11 — Exhaustive Field Mismatch Report
**Doc Source:** `Module11-20 (5).md`  
**API Source:** `Module_11_Stock_Management.postman_collection.json`

---

## Legend

| Status | Meaning |
|--------|---------|
| ✅ MATCH | Field exists in both Doc and API with correct name |
| ❌ MISSING FROM API | Field is in the Doc but NOT sent in the API |
| ➕ EXTRA IN API | Field sent in API but NOT defined in the Doc |
| ⚠️ NAME MISMATCH | Field exists in both but named differently |
| 🔒 READ-ONLY / AUTO | Field is auto-filled / system-generated (not expected in request body) |

---

---

# GROUP 1: Dashboard & Central Stock APIs

---

## API 1 — Get Dashboard
**Method:** `GET`  
**API URL:** `/api/v1/stock/dashboard?pageNo=0&pageSize=10&branchId=&search=&status=`

### Query Parameters

| # | Field (Doc Name) | Required | API Has It? | Status |
|---|---|---|---|---|
| 1 | `pageNo` | No | ✅ Yes | ✅ MATCH |
| 2 | `pageSize` | No | ✅ Yes | ✅ MATCH |
| 3 | `branchId` | No | ✅ Yes | ✅ MATCH |
| 4 | `search` | No | ✅ Yes | ✅ MATCH |
| 5 | `status` | No | ✅ Yes | ✅ MATCH |
| 6 | `category` | No | ❌ No | ❌ MISSING FROM API |
| 7 | `stockType` | No | ❌ No | ❌ MISSING FROM API |
| 8 | `hsnCode` | No | ❌ No | ❌ MISSING FROM API |
| 9 | `hasAssets` | No | ❌ No | ❌ MISSING FROM API |
| 10 | `createdDateFrom` | No | ❌ No | ❌ MISSING FROM API |
| 11 | `createdDateTo` | No | ❌ No | ❌ MISSING FROM API |

**Summary:** 5 ✅ Matched · 6 ❌ Missing from API · 0 ➕ Extra

> **Doc reference:** Module 11.1 Filters section defines: Branch, Category (Multi-select: Chemical/Sprayer/Machine/Trap/Tool/Other), Stock Type (Multi-select: Assets/Consumable/Resell), Status, HSN Code, Has Assets, Created Date range.

---

## API 2 — Get Product Stock Detail
**Method:** `GET`  
**API URL:** `/api/v1/stock/dashboard/detail?productId={{productId}}`

### Query Parameters

| # | Field (Doc Name) | Required | API Has It? | Status |
|---|---|---|---|---|
| 1 | `productId` | Yes | ✅ Yes | ✅ MATCH |

**Summary:** 1 ✅ Matched · 0 Missing · 0 Extra — **Perfect Match**

---

## API 3 — Preview Asset IDs
**Method:** `POST`  
**API URL:** `/api/v1/stock/asset-ids/preview`

### Request Body Fields

| # | Field (Doc Name) | Required | API Has It? | API Field Name | Status |
|---|---|---|---|---|---|
| 1 | `prefix` | Yes | ✅ Yes | `prefix` | ✅ MATCH |
| 2 | `start` | Yes | ✅ Yes | `start` | ✅ MATCH |
| 3 | `count` | Yes | ✅ Yes | `count` | ✅ MATCH |

**Summary:** 3 ✅ Matched · 0 Missing · 0 Extra — **Perfect Match**

---

## API 4 — Create Central Entry (Add to Central Stock)
**Method:** `POST`  
**API URL:** `/api/v1/stock/central-entries`

### Request Body Fields

**Doc Sections Mapped:** Section 1 (Product Selection) + Section 2 (Stock Type) + Section 3 (Asset Details) + Section 4 (Purchase & Tax) + Section 5 (Initial Allocation)

| # | Field (Doc Name) | Doc Section | Required | API Has It? | API Field Name | Status |
|---|---|---|---|---|---|---|
| **SECTION 1 — Product Selection** |
| 1 | Product | Sec 1 | Yes | ✅ Yes | `productId` | ✅ MATCH |
| 2 | Product Code | Sec 1 | System | ✅ Yes | `productCode` | ✅ MATCH |
| 3 | HSN Code | Sec 1 | System | ❌ No | — | ❌ MISSING FROM API |
| 4 | Current Central Stock | Sec 1 | System | ❌ No | — | 🔒 READ-ONLY (display only) |
| 5 | Base UOM | Sec 1 | System | ❌ No | — | 🔒 READ-ONLY (display only) |
| **SECTION 2 — Stock Type Allocation** |
| 6 | Total Quantity Received | Sec 2 | Yes | ✅ Yes | `totalQty` | ⚠️ NAME MISMATCH — Doc: `Total Quantity Received`, API: `totalQty` |
| 7 | Assets Qty | Sec 2 | No (≥0) | ✅ Yes | `assetsQty` | ✅ MATCH |
| 8 | Consumable Qty | Sec 2 | No (≥0) | ✅ Yes | `consumableQty` | ✅ MATCH |
| 9 | Resell Qty | Sec 2 | No (≥0) | ✅ Yes | `resellQty` | ✅ MATCH |
| **SECTION 3 — Asset Details (Conditional: Assets Qty > 0)** |
| 10 | Asset ID Generation | Sec 3 | Yes (if assets>0) | ✅ Yes | `assetIdGeneration` | ✅ MATCH |
| 11 | Asset ID Prefix | Sec 3 | Yes (if Auto) | ✅ Yes | `assetIdPrefix` | ✅ MATCH |
| 12 | Starting Sequence | Sec 3 | Yes (if Auto) | ✅ Yes | `assetSequenceStart` | ⚠️ NAME MISMATCH — Doc: `Starting Sequence`, API: `assetSequenceStart` |
| 13 | Default Assignment | Sec 3 | Yes (if assets>0) | ❌ No | — | ❌ MISSING FROM API |
| 14 | Assignment Type | Sec 3 | Yes (if assets>0) | ✅ Yes | `assignmentType` | ✅ MATCH |
| 15 | Branch | Sec 3 | Conditional | ❌ No | — | ❌ MISSING FROM API |
| 16 | Role | Sec 3 | Conditional | ❌ No | — | ❌ MISSING FROM API |
| 17 | Person | Sec 3 | Conditional | ❌ No | — | ❌ MISSING FROM API |
| **SECTION 4 — Purchase & Tax Details** |
| 18 | Supplier / Vendor | Sec 4 | Yes | ✅ Yes | `supplierName` | ⚠️ NAME MISMATCH — Doc: `Supplier/Vendor` (lookup), API: `supplierName` (plain string) |
| 19 | Purchase Order Ref | Sec 4 | No | ❌ No | — | ❌ MISSING FROM API |
| 20 | Invoice Number | Sec 4 | Yes | ✅ Yes | `invoiceNumber` | ✅ MATCH |
| 21 | Invoice Date | Sec 4 | Yes | ✅ Yes | `invoiceDate` | ✅ MATCH |
| 22 | Invoice Amount (₹) | Sec 4 | Yes | ✅ Yes | `invoiceAmount` | ✅ MATCH |
| 23 | Tax Amount (₹) | Sec 4 | System | ✅ Yes | `taxAmount` | ✅ MATCH |
| 24 | Total with Tax (₹) | Sec 4 | System | ✅ Yes | `totalWithTax` | ✅ MATCH |
| 25 | Invoice Copy | Sec 4 | No | ❌ No | — | ❌ MISSING FROM API |
| 26 | Batch Number | Sec 4 | Conditional (consumables) | ❌ No | — | ❌ MISSING FROM API |
| 27 | Manufacturing Date | Sec 4 | Conditional (consumables) | ❌ No | — | ❌ MISSING FROM API |
| 28 | Expiry Date | Sec 4 | Conditional (consumables) | ❌ No | — | ❌ MISSING FROM API |
| **SECTION 5 — Initial Allocation** |
| 29 | Immediate Transfer to Branches (list) | Sec 5 | No | ❌ No | — | ❌ MISSING FROM API (handled by separate API 8) |
| 30 | Branch Qty (per branch) | Sec 5 | No | ❌ No | — | ❌ MISSING FROM API (handled by separate API 8) |
| 31 | Remain at Central | Sec 5 | System | 🔒 No | — | 🔒 READ-ONLY |
| **EXTRA FIELDS IN API (not in Doc)** |
| — | `productName` | — | — | ➕ Yes | `productName` | ➕ EXTRA IN API |

**Summary:**
- ✅ MATCH: 13
- ❌ MISSING FROM API: 12 (`hsnCode`, `defaultAssignment`, `branch`, `role`, `person`, `purchaseOrderRef`, `invoiceCopy`, `batchNumber`, `manufacturingDate`, `expiryDate`, `initialAllocationBranches`, `branchQty`)
- ➕ EXTRA IN API: 1 (`productName`)
- ⚠️ NAME MISMATCH: 3 (`totalQty`, `assetSequenceStart`, `supplierName`)

---

## API 5 — Update Central Entry (Edit Central Stock)
**Method:** `PUT`  
**API URL:** `/api/v1/stock/central-entries?entryId={{entryId}}`

### Request Body Fields

| # | Field (Doc Name) | Doc Section | Required | API Has It? | API Field Name | Status |
|---|---|---|---|---|---|---|
| **SECTION 1 — Product Info (All READ-ONLY in Edit)** |
| 1 | Product | Sec 1 | No (locked) | ✅ Yes | `productId` | 🔒 Sent but doc says locked/read-only |
| 2 | Product Code | Sec 1 | No (locked) | ✅ Yes | `productCode` | 🔒 Sent but doc says locked/read-only |
| 3 | Product Name | Sec 1 | No (locked) | ✅ Yes | `productName` | ➕ EXTRA IN API (not a doc field at all) |
| **SECTION 2 — Stock Allocation (Editable)** |
| 4 | Total Quantity Received | Sec 2 | No (LOCKED) | ✅ Yes | `totalQty` | ❌ SHOULD NOT BE IN API — Doc says "Locked after creation" |
| 5 | Assets Qty | Sec 2 | Yes | ✅ Yes | `assetsQty` | ✅ MATCH |
| 6 | Consumable Qty | Sec 2 | Yes | ✅ Yes | `consumableQty` | ✅ MATCH |
| 7 | Resell Qty | Sec 2 | Yes | ✅ Yes | `resellQty` | ✅ MATCH |
| **SECTION 3 — Asset Details (Editable)** |
| 8 | Assignment Type | Sec 3 | Yes | ✅ Yes | `assignmentType` | ✅ MATCH |
| 9 | Default Assignment | Sec 3 | Conditional | ❌ No | — | ❌ MISSING FROM API |
| 10 | Branch | Sec 3 | Conditional | ❌ No | — | ❌ MISSING FROM API |
| 11 | Role | Sec 3 | Conditional | ❌ No | — | ❌ MISSING FROM API |
| 12 | Person | Sec 3 | Conditional | ❌ No | — | ❌ MISSING FROM API |
| **SECTION 4 — Purchase & Tax (Mostly LOCKED)** |
| 13 | Supplier | Sec 4 | No (locked) | ❌ No | — | 🔒 Correctly excluded (locked) |
| 14 | Purchase Order Ref | Sec 4 | No (locked) | ❌ No | — | 🔒 Correctly excluded (locked) |
| 15 | Invoice Number | Sec 4 | No (locked) | ❌ No | — | 🔒 Correctly excluded (locked) |
| 16 | Invoice Date | Sec 4 | No (locked) | ❌ No | — | 🔒 Correctly excluded (locked) |
| 17 | Invoice Amount | Sec 4 | No (locked) | ❌ No | — | 🔒 Correctly excluded (locked) |
| 18 | Tax Amount | Sec 4 | No (locked) | ❌ No | — | 🔒 Correctly excluded (locked) |
| 19 | Total with Tax | Sec 4 | No (locked) | ❌ No | — | 🔒 Correctly excluded (locked) |
| 20 | Invoice Copy (replacement) | Sec 4 | No | ❌ No | — | ❌ MISSING FROM API (editable in doc) |
| 21 | Batch Number | Sec 4 | Conditional | ❌ No | — | ❌ MISSING FROM API (editable for consumables) |
| 22 | Manufacturing Date | Sec 4 | Conditional | ❌ No | — | ❌ MISSING FROM API (editable for consumables) |
| 23 | Expiry Date | Sec 4 | Conditional | ❌ No | — | ❌ MISSING FROM API (editable for consumables) |
| **SECTION 5 — Branch Allocation (Editable)** |
| 24 | Branch Qty (per branch) | Sec 5 | Yes | ❌ No | — | ❌ MISSING FROM API |

**Summary:**
- ✅ MATCH: 4
- ❌ MISSING FROM API: 8 (`defaultAssignment`, `branch`, `role`, `person`, `invoiceCopy`, `batchNumber`, `manufacturingDate`, `expiryDate`, `branchQty`)
- ➕ EXTRA IN API: 2 (`totalQty` sent but doc says locked; `productName` not a doc field)
- 🔒 Correctly excluded (locked fields): 7

---

## API 6 — Get Central Entry
**Method:** `GET`  
**API URL:** `/api/v1/stock/central-entries?entryId={{entryId}}`

| # | Field | Status |
|---|---|---|
| 1 | `entryId` | ✅ MATCH |

**No request body.** — **Perfect Match**

---

## API 7 — Delete Central Entry
**Method:** `DELETE`  
**API URL:** `/api/v1/stock/central-entries?entryId={{entryId}}`

| # | Field | Status |
|---|---|---|
| 1 | `entryId` | ✅ MATCH |

**No request body.** — **Perfect Match**

---

## API 8 — Create Initial Allocation
**Method:** `POST`  
**API URL:** `/api/v1/stock/central-entries/initial-allocations?entryId={{entryId}}`

### Request Body Fields (Array of objects)

| # | Field (Doc Name) | Required | API Has It? | API Field Name | Status |
|---|---|---|---|---|---|
| 1 | Branch | Yes | ✅ Yes | `branchId` | ⚠️ NAME MISMATCH — Doc: `Branch`, API: `branchId` |
| 2 | Assets Qty | No | ✅ Yes | `assetsQty` | ✅ MATCH |
| 3 | Consumable Qty | No | ✅ Yes | `consumableQty` | ✅ MATCH |
| 4 | Resell Qty | No | ✅ Yes | `resellQty` | ✅ MATCH |

**Summary:** 3 ✅ Matched · 1 ⚠️ Name Mismatch · 0 Missing · 0 Extra

---

## API 9 — Get Assets
**Method:** `GET`  
**API URL:** `/api/v1/stock/assets?pageNo=0&pageSize=10&search=&branchId={{branchFrom}}`

| # | Field | Required | Status |
|---|---|---|---|
| 1 | `pageNo` | No | ✅ MATCH |
| 2 | `pageSize` | No | ✅ MATCH |
| 3 | `search` | No | ✅ MATCH |
| 4 | `branchId` | No | ✅ MATCH |

**Summary:** 4 ✅ — **Perfect Match**

---

## API 10 — Get Asset History
**Method:** `GET`  
**API URL:** `/api/v1/stock/assets/history?assetId={{assetId}}`

| # | Field | Status |
|---|---|---|
| 1 | `assetId` | ✅ MATCH |

**Summary:** 1 ✅ — **Perfect Match**

---

---

# GROUP 2: Request Lifecycle APIs

---

## API 11 — Get My Requests
**Method:** `GET`  
**API URL:** `/api/v1/stock/requests/my?pageNo=0&pageSize=10&branchId={{branchFrom}}&status=&search=`

### Query Parameters

| # | Field (Doc Name) | Required | API Has It? | Status |
|---|---|---|---|---|
| 1 | `pageNo` | No | ✅ Yes | ✅ MATCH |
| 2 | `pageSize` | No | ✅ Yes | ✅ MATCH |
| 3 | `branchId` | No | ✅ Yes | ✅ MATCH |
| 4 | `status` | No | ✅ Yes | ✅ MATCH |
| 5 | `search` | No | ✅ Yes | ✅ MATCH |
| 6 | `requestType` | No | ❌ No | ❌ MISSING FROM API |
| 7 | `priority` | No | ❌ No | ❌ MISSING FROM API |
| 8 | `dateFrom` | No | ❌ No | ❌ MISSING FROM API |
| 9 | `dateTo` | No | ❌ No | ❌ MISSING FROM API |

> **Doc reference:** Module 11.2 Filters: Request Type (Stock Request/Transfer Request), Status, Branch, Date Range, Priority.

**Summary:** 5 ✅ Matched · 4 ❌ Missing · 0 Extra

---

## API 12 — Get Received Requests (Approval Tab)
**Method:** `GET`  
**API URL:** `/api/v1/stock/requests/received?pageNo=0&pageSize=10&segment=PENDING_APPROVAL&branchId=&status=&search=`

### Query Parameters

| # | Field (Doc Name) | Required | API Has It? | Status |
|---|---|---|---|---|
| 1 | `pageNo` | No | ✅ Yes | ✅ MATCH |
| 2 | `pageSize` | No | ✅ Yes | ✅ MATCH |
| 3 | `segment` | No | ✅ Yes | ✅ MATCH (aligns with Module 11.3 segmented control) |
| 4 | `branchId` | No | ✅ Yes | ✅ MATCH |
| 5 | `status` | No | ✅ Yes | ✅ MATCH |
| 6 | `search` | No | ✅ Yes | ✅ MATCH |

**Summary:** 6 ✅ — **Perfect Match**

---

## API 13 — Create Request (Stock Request Form)
**Method:** `POST`  
**API URL:** `/api/v1/stock/requests`

### Request Body Fields

**Doc Sections Mapped:** Section 1 (Header) + Items Table + Summary Section (11.2.1)

#### Header Fields

| # | Field (Doc Name) | Required | API Has It? | API Field Name | Status |
|---|---|---|---|---|---|
| 1 | Request ID | System | 🔒 No | — | 🔒 READ-ONLY (auto-generated) |
| 2 | Requesting Branch | System | ✅ Yes | `fromBranchId` | ⚠️ NAME MISMATCH — Doc: `Requesting Branch`, API: `fromBranchId` |
| 3 | Requested By | System | 🔒 No | — | 🔒 READ-ONLY |
| 4 | Request Date | System | 🔒 No | — | 🔒 READ-ONLY |
| 5 | Priority | Yes | ✅ Yes | `priority` | ✅ MATCH |
| 6 | Required By Date | Yes | ✅ Yes | `requiredByDate` | ✅ MATCH |
| 7 | Purpose | Yes | ✅ Yes | `purpose` | ✅ MATCH |
| 8 | Request Type | — | ✅ Yes | `requestType` | ✅ MATCH (API adds this for direction) |
| 9 | To Branch | — | ✅ Yes | `toBranchId` | ✅ MATCH |

#### Items Table Fields (per item)

| # | Field (Doc Name) | Required | API Has It? | API Field Name | Status |
|---|---|---|---|---|---|
| 1 | Product | Yes | ✅ Yes | `productId` | ✅ MATCH |
| 2 | Product Code | System | ✅ Yes | `productCode` | ✅ MATCH |
| 3 | Current Branch Stock | System | 🔒 No | — | 🔒 READ-ONLY |
| 4 | Assets Qty | No (≥0) | ✅ Yes | `assetsQty` | ✅ MATCH |
| 5 | Consumable Qty | No (≥0) | ✅ Yes | `consumableQty` | ✅ MATCH |
| 6 | Resell Qty | No (≥0) | ✅ Yes | `resellQty` | ✅ MATCH |
| 7 | Total Qty | System | 🔒 No | — | 🔒 READ-ONLY (auto-sum) |
| 8 | UOM | System | ✅ Yes | `baseUom` | ⚠️ NAME MISMATCH — Doc: `UOM`, API: `baseUom` |
| 9 | Estimated Cost (₹) | System | 🔒 No | — | 🔒 READ-ONLY (auto-calculated) |
| 10 | Tax (₹) | System | 🔒 No | — | 🔒 READ-ONLY (auto-calculated) |
| 11 | Purpose per Item | No | ✅ Yes | `itemPurpose` | ✅ MATCH |

#### Summary Section Fields

| # | Field (Doc Name) | Required | API Has It? | Status |
|---|---|---|---|---|
| 1 | Total Products | System | 🔒 No | 🔒 READ-ONLY |
| 2 | Total Assets Requested | System | 🔒 No | 🔒 READ-ONLY |
| 3 | Total Consumables Requested | System | 🔒 No | 🔒 READ-ONLY |
| 4 | Total Resell Requested | System | 🔒 No | 🔒 READ-ONLY |
| 5 | Total Estimated Value (₹) | System | 🔒 No | 🔒 READ-ONLY |
| 6 | Notes for Approver | No | ✅ Yes | `notesForApprover` | ✅ MATCH |

**Extra fields in API (not in Doc):**
- `productName` — ➕ EXTRA IN API

**Summary:**
- ✅ MATCH: 11
- ❌ MISSING FROM API: 0
- ➕ EXTRA IN API: 1 (`productName`)
- ⚠️ NAME MISMATCH: 3 (`fromBranchId`, `baseUom`, `itemPurpose` vs Doc names)

---

## API 14 — Update Request
**Method:** `PUT`  
**API URL:** `/api/v1/stock/requests?requestId={{requestId}}`

### Request Body Fields (compared to Create Request fields)

| # | Field | In Create API | In Update API | Status |
|---|---|---|---|---|
| 1 | `requestType` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 2 | `fromBranchId` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 3 | `toBranchId` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 4 | `priority` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 5 | `requiredByDate` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 6 | `purpose` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 7 | `notesForApprover` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 8 | `items[].productId` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 9 | `items[].productCode` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 10 | `items[].productName` | ✅ Yes | ✅ Yes | ✅ MATCH (extra in both) |
| 11 | `items[].baseUom` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 12 | `items[].assetsQty` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 13 | `items[].consumableQty` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 14 | `items[].resellQty` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 15 | `items[].itemPurpose` | ✅ Yes | ❌ No | ❌ MISSING FROM UPDATE API |

**Summary:** 14 ✅ Matched · 1 ❌ Missing (`itemPurpose` dropped in update vs create)

---

## API 15 — Save Recipients
**Method:** `POST`  
**API URL:** `/api/v1/stock/requests/recipients?requestId={{requestId}}`

### Request Body Fields

| # | Field (Doc Name) | Required | API Has It? | API Field Name | Status |
|---|---|---|---|---|---|
| 1 | Recipients (email list) | Yes | ✅ Yes | `recipients` | ✅ MATCH |

**Summary:** 1 ✅ — **Perfect Match**

---

## API 16 — Submit Request
**Method:** `POST`  
**API URL:** `/api/v1/stock/requests/submit?requestId={{requestId}}`

No request body. Query param `requestId` present. — **Perfect Match**

---

## API 17 — Get Request Detail
**Method:** `GET`  
**API URL:** `/api/v1/stock/requests?requestId={{requestId}}`

No request body. Query param `requestId` present. — **Perfect Match**

---

## API 18 — Revoke Request
**Method:** `POST`  
**API URL:** `/api/v1/stock/requests/revoke?requestId={{requestId}}`

No request body. Query param `requestId` present. — **Perfect Match**

---

## API 19 — Receive Request (Confirm Receipt)
**Method:** `POST`  
**API URL:** `/api/v1/stock/requests/receive?requestId={{requestId}}`

### Request Body Fields

**Doc Reference:** Module 11.2.3 — Receive Stock / Asset Allocation

| # | Field (Doc Name) | Required | API Has It? | API Field Name | Status |
|---|---|---|---|---|---|
| **RECEIPT DETAILS SECTION** |
| 1 | Received Date | Yes | ✅ Yes | `receivedDate` | ✅ MATCH |
| 2 | Received By | System | 🔒 No | — | 🔒 READ-ONLY (auto-filled) |
| 3 | Package Condition | Yes | ✅ Yes | `packageCondition` | ✅ MATCH |
| **ASSET VERIFICATION SECTION** |
| 4 | Asset ID | System | 🔒 No | — | 🔒 READ-ONLY |
| 5 | Product | System | 🔒 No | — | 🔒 READ-ONLY |
| 6 | Expected Condition | System | 🔒 No | — | 🔒 READ-ONLY |
| 7 | Received Condition (per asset) | Yes | ❌ No | — | ❌ MISSING FROM API |
| 8 | Status (per asset) | System | 🔒 No | — | 🔒 READ-ONLY |
| **ASSET ASSIGNMENT SECTION** |
| 9 | Assign To (per asset) | Yes | ❌ No | — | ❌ MISSING FROM API |
| **QUANTITY VERIFICATION SECTION** |
| 10 | Expected Qty | System | 🔒 No | — | 🔒 READ-ONLY |
| 11 | Received Qty (per product) | Yes | ✅ Yes | `receivedItems[].consumableQty` etc. | ✅ MATCH (via `receivedItems` array) |
| **RECEIPT CONFIRMATION SECTION** |
| 12 | Upload Receipt Photo | No | ❌ No | — | ❌ MISSING FROM API |
| 13 | Remarks | No | ✅ Yes | `remarks` | ✅ MATCH |
| 14 | Confirm Receipt (checkbox) | Yes | ✅ Yes | `confirmReceipt` | ✅ MATCH |
| **RECEIVED ITEMS ARRAY (Additional in API)** |
| — | `receivedItems[].productId` | — | ➕ Yes | `productId` | ➕ EXTRA IN API (not in doc field table, but aligns with Qty Verification table) |
| — | `receivedItems[].assetsQty` | — | ➕ Yes | `assetsQty` | ➕ EXTRA IN API |
| — | `receivedItems[].consumableQty` | — | ➕ Yes | `consumableQty` | ➕ EXTRA IN API |
| — | `receivedItems[].resellQty` | — | ➕ Yes | `resellQty` | ➕ EXTRA IN API |

**Summary:**
- ✅ MATCH: 5
- ❌ MISSING FROM API: 3 (`receivedCondition` per asset, `assignTo` per asset, `receiptPhotoUrl`)
- ➕ EXTRA IN API: 4 (the `receivedItems[]` array - though justified by doc's Qty Verification table)
- 🔒 READ-ONLY: 6

---

---

# GROUP 3: Approval APIs

---

## API 20 — Get Approval Inbox
**Method:** `GET`  
**API URL:** `/api/v1/stock/approval/requests/inbox?pageNo=0&pageSize=10&branchId=&status=&search=`

### Query Parameters

| # | Field (Doc Name) | Required | API Has It? | Status |
|---|---|---|---|---|
| 1 | `pageNo` | No | ✅ Yes | ✅ MATCH |
| 2 | `pageSize` | No | ✅ Yes | ✅ MATCH |
| 3 | `branchId` | No | ✅ Yes | ✅ MATCH |
| 4 | `status` | No | ✅ Yes | ✅ MATCH |
| 5 | `search` | No | ✅ Yes | ✅ MATCH |
| 6 | `segment` | No | ❌ No | ❌ MISSING FROM API (present in Received Requests API but not inbox) |

**Summary:** 5 ✅ Matched · 1 ❌ Missing

---

## API 21 — Get Approval View
**Method:** `GET`  
**API URL:** `/api/v1/stock/approval/requests/approval-view?requestId={{requestId}}`

| # | Field | Status |
|---|---|---|
| 1 | `requestId` | ✅ MATCH |

**No request body.** — **Perfect Match**

---

## API 22 — Approve Request
**Method:** `POST`  
**API URL:** `/api/v1/stock/approval/requests/approve?requestId={{requestId}}`

### Request Body Fields

**Doc Reference:** Module 11.3.1 — Section 2 (Approved Quantities) + Section 3 (Approval Decision) + Section 4 (Branch Transfer Planning)

| # | Field (Doc Name) | Doc Section | Required | API Has It? | API Field Name | Status |
|---|---|---|---|---|---|---|
| **SECTION 1 — Request Details (All READ-ONLY)** |
| 1 | Request ID | Sec 1 | System | 🔒 No | — | 🔒 READ-ONLY |
| 2 | Request Type | Sec 1 | System | 🔒 No | — | 🔒 READ-ONLY |
| 3 | From Branch | Sec 1 | System | 🔒 No | — | 🔒 READ-ONLY |
| 4 | Requested By | Sec 1 | System | 🔒 No | — | 🔒 READ-ONLY |
| 5 | Requested Date | Sec 1 | System | 🔒 No | — | 🔒 READ-ONLY |
| 6 | Priority | Sec 1 | System | 🔒 No | — | 🔒 READ-ONLY |
| 7 | Required By | Sec 1 | System | 🔒 No | — | 🔒 READ-ONLY |
| 8 | Purpose | Sec 1 | System | 🔒 No | — | 🔒 READ-ONLY |
| **SECTION 2 — Approved Quantities (per product)** |
| 9 | Appr. Assets (per product) | Sec 2 | Conditional | ✅ Yes | `approvedItems[].assetsQty` | ✅ MATCH |
| 10 | Appr. Consumables (per product) | Sec 2 | Conditional | ✅ Yes | `approvedItems[].consumableQty` | ✅ MATCH |
| 11 | Appr. Resell (per product) | Sec 2 | Conditional | ✅ Yes | `approvedItems[].resellQty` | ✅ MATCH |
| 12 | Alternative Source (per product) | Sec 2 | Conditional | ✅ Yes | `alternativeSource` | ✅ MATCH (global field in API) |
| **SECTION 3 — Approval Decision** |
| 13 | Approval Type | Sec 3 | Yes | ✅ Yes | `approvalType` | ✅ MATCH |
| 14 | Alternative Source | Sec 3 | Conditional | ✅ Yes | `alternativeSource` | ✅ MATCH |
| 15 | Dispatch Date | Sec 3 | Yes | ✅ Yes | `dispatchDate` | ✅ MATCH |
| 16 | Expected Delivery | Sec 3 | Yes | ✅ Yes | `expectedDeliveryDate` | ✅ MATCH |
| 17 | Carrier | Sec 3 | Yes | ✅ Yes | `carrier` | ✅ MATCH |
| 18 | LR Number | Sec 3 | No | ✅ Yes | `lrNumber` | ✅ MATCH |
| 19 | Asset ID Assignment | Sec 3 | Conditional | ❌ No | — | ❌ MISSING FROM API |
| 20 | Remarks | Sec 3 | Yes | ✅ Yes | `remarks` | ✅ MATCH |
| **SECTION 4 — Branch Transfer Planning (Conditional: alternativeSource = OTHER_BRANCH)** |
| 21 | Transfer Type | Sec 4 | Yes | ❌ No | — | ❌ MISSING FROM API |
| 22 | Transfer ID | Sec 4 | System | 🔒 No | — | 🔒 READ-ONLY |
| 23 | From Branch | Sec 4 | System | 🔒 No | — | 🔒 READ-ONLY |
| 24 | To Branch | Sec 4 | System | 🔒 No | — | 🔒 READ-ONLY |
| 25 | Reference Request | Sec 4 | System | 🔒 No | — | 🔒 READ-ONLY |
| 26 | Source Branch (per transfer product) | Sec 4 | Yes | ❌ No | — | ❌ MISSING FROM API |
| 27 | Assets Qty (per transfer product) | Sec 4 | Conditional | ❌ No | — | ❌ MISSING FROM API |
| 28 | Consumable Qty (per transfer product) | Sec 4 | Conditional | ❌ No | — | ❌ MISSING FROM API |
| 29 | Resell Qty (per transfer product) | Sec 4 | Conditional | ❌ No | — | ❌ MISSING FROM API |
| 30 | Transfer Strategy | Sec 4 | Yes | ❌ No | — | ❌ MISSING FROM API |

**Summary:**
- ✅ MATCH: 12
- ❌ MISSING FROM API: 8 (`assetIdAssignment`, `transferType`, `sourceBranch`, `transferAssetsQty`, `transferConsumableQty`, `transferResellQty`, `transferStrategy`, and per-product `alternativeSource` detail)
- 🔒 READ-ONLY: 13

---

## API 23 — Reject Request
**Method:** `POST`  
**API URL:** `/api/v1/stock/approval/requests/reject?requestId={{requestId}}`

### Request Body Fields

| # | Field (Doc Name) | Required | API Has It? | API Field Name | Status |
|---|---|---|---|---|---|
| 1 | Reason | Yes | ✅ Yes | `reason` | ✅ MATCH |

**Summary:** 1 ✅ — **Perfect Match**

---

## API 24 — Hold Request
**Method:** `POST`  
**API URL:** `/api/v1/stock/approval/requests/hold?requestId={{requestId}}`

### Request Body Fields

| # | Field (Doc Name) | Required | API Has It? | API Field Name | Status |
|---|---|---|---|---|---|
| 1 | Reason | Yes | ✅ Yes | `reason` | ✅ MATCH |

**Summary:** 1 ✅ — **Perfect Match**

---

## API 25 — Update Approval
**Method:** `PUT`  
**API URL:** `/api/v1/stock/approval/requests/approval?requestId={{requestId}}`

### Request Body Fields (compared to Approve Request)

| # | Field | In Approve API | In Update Approval API | Status |
|---|---|---|---|---|
| 1 | `approvalType` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 2 | `alternativeSource` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 3 | `dispatchDate` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 4 | `expectedDeliveryDate` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 5 | `carrier` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 6 | `lrNumber` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 7 | `remarks` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 8 | `approvedItems` | ✅ Yes | ❌ No | ❌ MISSING FROM UPDATE APPROVAL API |

**Summary:** 7 ✅ Matched · 1 ❌ Missing (`approvedItems` array removed in Update vs Approve)

---

---

# GROUP 4: Transfer Lifecycle APIs

---

## API 26 — Create Transfer
**Method:** `POST`  
**API URL:** `/api/v1/stock/transfers`

### Request Body Fields

**Doc Reference:** Module 11.4 — Section 1 (Transfer Details) + Section 2 (Product Selection) + Section 3 (Transfer Quantity) + Section 4 (Asset Selection)

| # | Field (Doc Name) | Doc Section | Required | API Has It? | API Field Name | Status |
|---|---|---|---|---|---|---|
| **SECTION 1 — Transfer Details** |
| 1 | Transfer ID | Sec 1 | System | 🔒 No | — | 🔒 READ-ONLY |
| 2 | From Branch | Sec 1 | Yes | ✅ Yes | `fromBranchId` | ✅ MATCH |
| 3 | To Branch | Sec 1 | Yes | ✅ Yes | `toBranchId` | ✅ MATCH |
| 4 | Transfer Type | Sec 1 | Yes | ✅ Yes | `transferType` | ✅ MATCH |
| 5 | Reference Request | Sec 1 | No | ✅ Yes | `referenceRequestId` | ✅ MATCH |
| 6 | Transfer Date | Sec 1 | System | 🔒 No | — | 🔒 READ-ONLY |
| 7 | Created By | Sec 1 | System | 🔒 No | — | 🔒 READ-ONLY |
| 8 | Transfer Strategy | Sec 1 / Workflow | Yes | ✅ Yes | `strategy` | ⚠️ NAME MISMATCH — Doc: `Transfer Strategy`, API: `strategy` |
| **SECTION 2 — Product Selection (per item)** |
| 9 | Product | Sec 2 | Yes | ✅ Yes | `items[].productId` | ✅ MATCH |
| 10 | Product Code | Sec 2 | System | ✅ Yes | `items[].productCode` | ✅ MATCH (extra context) |
| 11 | From Branch Stock | Sec 2 | System | 🔒 No | — | 🔒 READ-ONLY |
| 12 | Assets Available | Sec 2 | System | 🔒 No | — | 🔒 READ-ONLY |
| 13 | Consumables Available | Sec 2 | System | 🔒 No | — | 🔒 READ-ONLY |
| 14 | Resell Available | Sec 2 | System | 🔒 No | — | 🔒 READ-ONLY |
| **SECTION 3 — Transfer Quantity Allocation (per item)** |
| 15 | Transfer Assets Qty | Sec 3 | Conditional | ✅ Yes | `items[].assetsQty` | ✅ MATCH |
| 16 | Transfer Consumable Qty | Sec 3 | Conditional | ✅ Yes | `items[].consumableQty` | ✅ MATCH |
| 17 | Transfer Resell Qty | Sec 3 | Conditional | ✅ Yes | `items[].resellQty` | ✅ MATCH |
| 18 | Total Transfer Qty | Sec 3 | System | 🔒 No | — | 🔒 READ-ONLY |
| **SECTION 4 — Asset Selection (Conditional: Assets > 0)** |
| 19 | Select Asset (checkbox) | Sec 4 | Yes | ❌ No | — | ❌ MISSING FROM API |
| 20 | Asset ID | Sec 4 | System | 🔒 No | — | 🔒 READ-ONLY |
| 21 | Current Assignment | Sec 4 | System | 🔒 No | — | 🔒 READ-ONLY |
| 22 | Condition | Sec 4 | Yes | ❌ No | — | ❌ MISSING FROM API |
| 23 | Transfer With | Sec 4 | Yes | ❌ No | — | ❌ MISSING FROM API |
| **SECTION 5 — Destination Employee Assignment (Conditional: Transfer With = Assign to Employee)** |
| 24 | Employee | Sec 5 | Yes | ❌ No | — | ❌ MISSING FROM API |
| 25 | Department | Sec 5 | System | 🔒 No | — | 🔒 READ-ONLY |
| 26 | Notes | Sec 5 | No | ❌ No | — | ❌ MISSING FROM API |
| **EXTRA IN API** |
| — | `productName` | — | — | ➕ Yes | `productName` | ➕ EXTRA IN API |
| — | `baseUom` | — | — | ➕ Yes | `baseUom` | ➕ EXTRA IN API |

**Summary:**
- ✅ MATCH: 9
- ❌ MISSING FROM API: 5 (`selectedAssets[]`, `condition`, `transferWith`, `employee`, `notes`)
- ➕ EXTRA IN API: 2 (`productName`, `baseUom`)
- ⚠️ NAME MISMATCH: 1 (`strategy`)
- 🔒 READ-ONLY: 9

---

## API 27 — Update Transfer
**Method:** `PUT`  
**API URL:** `/api/v1/stock/transfers?transferId={{transferId}}`

### Request Body Fields (compared to Create Transfer)

| # | Field | In Create API | In Update API | Status |
|---|---|---|---|---|
| 1 | `fromBranchId` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 2 | `toBranchId` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 3 | `transferType` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 4 | `strategy` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 5 | `referenceRequestId` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 6 | `items[].productId` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 7 | `items[].productCode` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 8 | `items[].productName` | ✅ Yes | ✅ Yes | ✅ MATCH (extra in both) |
| 9 | `items[].baseUom` | ✅ Yes | ❌ No | ❌ MISSING FROM UPDATE (present in Create) |
| 10 | `items[].assetsQty` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 11 | `items[].consumableQty` | ✅ Yes | ✅ Yes | ✅ MATCH |
| 12 | `items[].resellQty` | ✅ Yes | ✅ Yes | ✅ MATCH |

**Summary:** 11 ✅ Matched · 1 ❌ Missing (`baseUom` inconsistently dropped in Update)

---

## API 28 — Get Transfer
**Method:** `GET`  
**API URL:** `/api/v1/stock/transfers?transferId={{transferId}}`

| # | Field | Status |
|---|---|---|
| 1 | `transferId` | ✅ MATCH |

**No request body.** — **Perfect Match**

---

## API 29 — Dispatch Transfer
**Method:** `POST`  
**API URL:** `/api/v1/stock/transfers/dispatch?transferId={{transferId}}`

### Request Body Fields

| # | Field (Doc Name) | Required | API Has It? | API Field Name | Status |
|---|---|---|---|---|---|
| 1 | Dispatch Date | Yes | ✅ Yes | `dispatchDate` | ✅ MATCH |
| 2 | Expected Delivery Date | Yes | ✅ Yes | `expectedDeliveryDate` | ✅ MATCH |
| 3 | Carrier | Yes | ✅ Yes | `carrier` | ✅ MATCH |
| 4 | LR Number | No | ✅ Yes | `lrNumber` | ✅ MATCH |
| 5 | Remarks | No | ✅ Yes | `remarks` | ✅ MATCH |

**Summary:** 5 ✅ — **Perfect Match**

---

## API 30 — Mark In Transit
**Method:** `POST`  
**API URL:** `/api/v1/stock/transfers/mark-in-transit?transferId={{transferId}}`

No request body. Query param `transferId` present. — **Perfect Match**

---

## API 31 — Receive Transfer
**Method:** `POST`  
**API URL:** `/api/v1/stock/transfers/receive?transferId={{transferId}}`

### Request Body Fields

**Doc Reference:** Module 11.2.3 (same receive screen applies to both request and transfer receipt)

| # | Field (Doc Name) | Required | API Has It? | API Field Name | Status |
|---|---|---|---|---|---|
| 1 | Received Date | Yes | ✅ Yes | `receivedDate` | ✅ MATCH |
| 2 | Received By | System | 🔒 No | — | 🔒 READ-ONLY |
| 3 | Package Condition | Yes | ✅ Yes | `packageCondition` | ✅ MATCH |
| 4 | Received Condition (per asset) | Yes | ❌ No | — | ❌ MISSING FROM API |
| 5 | Assign To (per asset) | Yes | ❌ No | — | ❌ MISSING FROM API |
| 6 | Received Qty (per product) | Yes | ✅ Yes | `receivedItems[].consumableQty` etc. | ✅ MATCH |
| 7 | Upload Receipt Photo | No | ❌ No | — | ❌ MISSING FROM API |
| 8 | Remarks | No | ✅ Yes | `remarks` | ✅ MATCH |
| 9 | Confirm Receipt | Yes | ✅ Yes | `confirmReceipt` | ✅ MATCH |

**Summary:**
- ✅ MATCH: 6
- ❌ MISSING FROM API: 3 (`receivedCondition` per asset, `assignTo` per asset, `receiptPhotoUrl`)
- 🔒 READ-ONLY: 1

---

## API 32 — List Transfers
**Method:** `GET`  
**API URL:** `/api/v1/stock/transfers?pageNo=0&pageSize=10&branchId=&status=&search=`

| # | Field | Required | Status |
|---|---|---|---|
| 1 | `pageNo` | No | ✅ MATCH |
| 2 | `pageSize` | No | ✅ MATCH |
| 3 | `branchId` | No | ✅ MATCH |
| 4 | `status` | No | ✅ MATCH |
| 5 | `search` | No | ✅ MATCH |

**Summary:** 5 ✅ — **Perfect Match**

---

---

# CONSOLIDATED MISMATCH SUMMARY

## All Missing Fields (❌ MISSING FROM API) — Sorted by API

| API | Field Missing from API | Required? | Impact |
|---|---|---|---|
| Get Dashboard | `category` filter | No | Can't filter by product category |
| Get Dashboard | `stockType` filter | No | Can't filter by Assets/Consumable/Resell |
| Get Dashboard | `hsnCode` filter | No | Can't filter by HSN code |
| Get Dashboard | `hasAssets` filter | No | Can't filter by asset presence |
| Get Dashboard | `createdDateFrom` filter | No | Date range filter missing |
| Get Dashboard | `createdDateTo` filter | No | Date range filter missing |
| Create Central Entry | `hsnCode` | System | Tax calculation reference missing |
| Create Central Entry | `defaultAssignment` | Yes | Asset assignment destination missing |
| Create Central Entry | `branch` | Conditional | Employee direct assignment missing |
| Create Central Entry | `role` | Conditional | Employee direct assignment missing |
| Create Central Entry | `person` | Conditional | Employee direct assignment missing |
| Create Central Entry | `purchaseOrderRef` | No | PO reference tracking missing |
| Create Central Entry | `invoiceCopy` | No | Invoice file upload missing |
| Create Central Entry | `batchNumber` | **MANDATORY for consumables** | Consumable batch tracking broken |
| Create Central Entry | `manufacturingDate` | **MANDATORY for consumables** | Consumable lifecycle data missing |
| Create Central Entry | `expiryDate` | **MANDATORY for consumables** | Expiry tracking broken |
| Create Central Entry | `initialAllocationBranches` | No | Handled separately in API 8 ✅ |
| Update Central Entry | `defaultAssignment` | Conditional | Assignment update incomplete |
| Update Central Entry | `branch` | Conditional | Assignment update incomplete |
| Update Central Entry | `role` | Conditional | Assignment update incomplete |
| Update Central Entry | `person` | Conditional | Assignment update incomplete |
| Update Central Entry | `invoiceCopy` | No | Can't replace invoice file |
| Update Central Entry | `batchNumber` | Conditional | Consumable batch update missing |
| Update Central Entry | `manufacturingDate` | Conditional | Date update missing |
| Update Central Entry | `expiryDate` | Conditional | Expiry update missing |
| Update Central Entry | `branchQty` | Yes | Branch quota update missing |
| Get My Requests | `requestType` filter | No | Can't separate stock vs transfer |
| Get My Requests | `priority` filter | No | Can't filter by urgency |
| Get My Requests | `dateFrom` filter | No | Date range filter missing |
| Get My Requests | `dateTo` filter | No | Date range filter missing |
| Update Request | `items[].itemPurpose` | No | Purpose per item lost on update |
| Receive Request | `receivedCondition` (per asset) | **YES** | Asset condition at receipt not captured |
| Receive Request | `assignTo` (per asset) | **YES** | Asset assignment at receipt missing |
| Receive Request | `receiptPhotoUrl` | No | Physical evidence upload missing |
| Get Approval Inbox | `segment` filter | No | Can't segment pending/completed |
| Approve Request | `assetIdAssignment` | Conditional | Auto vs manual asset ID control missing |
| Approve Request | `transferType` | Yes (if alt=OTHER_BRANCH) | Branch transfer type not specified |
| Approve Request | `sourceBranch` (per transfer product) | Yes | Source branch for alternate supply missing |
| Approve Request | `transferAssetsQty` | Conditional | Transfer quantity breakdown missing |
| Approve Request | `transferConsumableQty` | Conditional | Transfer quantity breakdown missing |
| Approve Request | `transferResellQty` | Conditional | Transfer quantity breakdown missing |
| Approve Request | `transferStrategy` | Yes | Single vs Split strategy missing |
| Update Approval | `approvedItems` | Yes | Item quantities lost on approval update |
| Create Transfer | `selectedAssets[]` | Yes (if assets>0) | Which assets to transfer not specified |
| Create Transfer | `condition` (per asset) | Yes | Asset pre-transfer condition missing |
| Create Transfer | `transferWith` | Yes | Post-transfer assignment rule missing |
| Create Transfer | `employee` | Conditional | Employee assignment target missing |
| Create Transfer | `notes` | No | Transfer notes missing |
| Update Transfer | `baseUom` | No | Inconsistent with Create Transfer |
| Receive Transfer | `receivedCondition` (per asset) | **YES** | Asset condition at receipt not captured |
| Receive Transfer | `assignTo` (per asset) | **YES** | Asset assignment at receipt missing |
| Receive Transfer | `receiptPhotoUrl` | No | Physical evidence upload missing |

---

## All Extra Fields (➕ EXTRA IN API — Not in Doc)

| API | Extra Field | Notes |
|---|---|---|
| Create Central Entry | `productName` | Redundant if `productId` exists; doc doesn't define this as a sendable field |
| Update Central Entry | `productName` | Same as above |
| Update Central Entry | `totalQty` | Doc explicitly says "Locked after creation" — should NOT be sent |
| Update Central Entry | `productId`, `productCode` | Doc says Section 1 is read-only in edit mode |
| Create Request | `productName` | Not a sendable request field in doc — redundant with `productId` |
| Create Transfer | `productName` | Not in doc's transfer item fields |
| Create Transfer | `baseUom` | Not in doc's transfer item fields |

---

## All Name Mismatches (⚠️ NAME MISMATCH)

| API | Doc Field Name | API Field Name | Severity |
|---|---|---|---|
| Create Central Entry | Total Quantity Received | `totalQty` | Low — logical abbreviation |
| Create Central Entry | Starting Sequence | `assetSequenceStart` | Low — logical alternative |
| Create Central Entry | Supplier / Vendor (Search Dropdown) | `supplierName` (plain string) | **Medium — Doc expects a supplier ID from master, API sends a raw string name** |
| Create Initial Allocation | Branch | `branchId` | Low — logical |
| Create Request | Requesting Branch | `fromBranchId` | Low — logical |
| Create Request | UOM | `baseUom` | Low — logical |
| Create Request | Purpose per Item | `itemPurpose` | Low — logical |
| Create Transfer | Transfer Strategy | `strategy` | Low — logical abbreviation |

---

## Overall Score

| Category | Count |
|---|---|
| ✅ Perfect Match APIs | 13 |
| ⚠️ APIs with Field Issues | 19 |
| ❌ Missing Fields (total) | 51 |
| ➕ Extra Fields in API | 7 |
| ⚠️ Name Mismatches | 8 |
| Total APIs Validated | 32 |

### Priority Fix List

| Priority | Field | API | Reason |
|---|---|---|---|
| 🔴 Critical | `batchNumber` | Create Central Entry | Required for consumables per doc |
| 🔴 Critical | `manufacturingDate` | Create Central Entry | Required for consumables per doc |
| 🔴 Critical | `expiryDate` | Create Central Entry | Required for consumables per doc |
| 🔴 Critical | `receivedCondition` per asset | Receive Request, Receive Transfer | Core receipt verification broken |
| 🔴 Critical | `assignTo` per asset | Receive Request, Receive Transfer | Asset cannot be allocated on receipt |
| 🔴 Critical | `approvedItems` | Update Approval | Approved quantities lost on edit |
| 🟠 High | `defaultAssignment` | Create/Update Central Entry | Asset assignment destination missing |
| 🟠 High | `assetIdAssignment` | Approve Request | Auto/manual ID generation not controlled |
| 🟠 High | `transferType`, `transferStrategy` | Approve Request | Transfer plan incomplete when alt source = OTHER_BRANCH |
| 🟠 High | `selectedAssets[]`, `condition`, `transferWith` | Create Transfer | Asset-level transfer control completely absent |
| 🟡 Medium | `category`, `stockType`, `hsnCode`, `hasAssets`, date range | Get Dashboard | Dashboard filters severely limited |
| 🟡 Medium | `requestType`, `priority`, date range | Get My Requests | Request list filters incomplete |
| 🟡 Medium | `purchaseOrderRef`, `invoiceCopy` | Create Central Entry | Procurement traceability incomplete |
| 🟡 Medium | `receiptPhotoUrl` | Receive Request/Transfer | Physical proof of receipt missing |
| 🟢 Low | `itemPurpose` missing in Update Request | Update Request | Minor inconsistency with Create |
| 🟢 Low | `baseUom` missing in Update Transfer | Update Transfer | Minor inconsistency with Create |
| 🟢 Low | All name mismatches | Multiple | Cosmetic — functionally equivalent |
