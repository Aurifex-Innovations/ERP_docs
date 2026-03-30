# [Stock Management]

## Short Description

The Stock Management module provides a centralized system for tracking inventory across multiple branches. It handles central stock entries, initial allocations, stock requests between branches, and physical transfers (dispatch and receipt). Frontend developers use this module to build dashboards for stock visibility, request workflows for branch managers, and asset tracking for auditors. [Option B (Hybrid)] integration ensures historical data integrity by snapshotting product details from the Inventory module.

---

## Authorization

Authentication: **Bearer Token (JWT)**
Headers:

- `Authorization: Bearer <token>`
- `X-Tenant-ID: <tenant-id>` (Required for all multi-tenant operations)

### Role-Based Access Control (RBAC)

| Role / Authority  | Access Level                                                         |
| ----------------- | -------------------------------------------------------------------- |
| `CEO`, `ADMIN`    | Full access (Central entries, Approvals, Transfers, Dashboard)       |
| `BRANCH_MANAGER`  | Request Creation, Approval (Inbox), Transfer Management, View Assets |
| `USER`            | My Requests, Receive Stock, View Transfers                           |
| `FINANCE_AUDITOR` | Read-only access to all views and asset history                      |

---

## Enums Used In This Module

### AssetCondition

Used to track the physical state of individual asset units.
| Value | Meaning | Used In |
|-------|---------|---------|
| `NEW` | Brand new item | Asset Detail, Receipt |
| `GOOD` | Used but in good condition | Asset Detail, Receipt |
| `FAIR` | Showing wear and tear | Asset Detail |
| `DAMAGED` | Physical damage reported | Asset Detail, Issue Report |
| `NEEDS_REPAIR`| Non-functional, requires maintenance | Asset Detail |

### AssetStatus

Used to track the allocation state of an asset.
| Value | Meaning | Used In |
|-------|---------|---------|
| `AVAILABLE` | Ready for use in branch | Asset List |
| `ISSUED` | Currently assigned to a user/process | Asset List |
| `IN_TRANSIT`| Moving between branches | Transfer Flow |
| `MAINTENANCE`| Under repair | Asset List |
| `RETIRED` | No longer in use | Asset List |
| `QUARANTINE`| Held for inspection | Receipt Flow |

### RequestPriority

Used to define how fast a request needs fulfillment.
| Value | Meaning |
|-------|---------|
| `LOW` | Non-critical stock |
| `NORMAL`| Standard business pace |
| `HIGH` | Required for immediate project/task |
| `URGENT`| Critical shortage |

### RequestStatus

Used to track the lifecycle of a stock or transfer request.
| Value | Meaning | Used In |
|-------|---------|---------|
| `DRAFT` | Saved but not submitted | My Requests |
| `PENDING_APPROVAL` | Sent to approver | Inbox |
| `APPROVED` | Approved for full quantity | Approval |
| `PARTIALLY_APPROVED`| Approved for partial quantity | Approval |
| `REJECTED` | Request denied | Approval |
| `HOLD` | Verification required | Approval |
| `DISPATCH` | Items packed and ready | Transfer |
| `IN_TRANSIT` | Items on the way | Transfer |
| `PARTIALLY_RECEIVED`| Some items received | Receipt |
| `RECEIVED` | All items received | Receipt |
| `ISSUE_REPORTED` | Discrepancy during receipt | Receipt |
| `REVOKED` | Cancelled by requester | My Requests |

### StockAvailabilityStatus

Indicates product availability in the ledger.
| Value | Meaning |
|-------|---------|
| `AVAILABLE`| High stock level |
| `LOW` | Reorder threshold reached |
| `OUT` | Zero stock |
| `INACTIVE` | Not available for requests |
| `DISCONTINUED`| Item no longer supported |

### TransferType

| Value       | Meaning                   |
| ----------- | ------------------------- |
| `REGULAR`   | Standard branch refill    |
| `EMERGENCY` | Immediate requirement     |
| `SCHEDULED` | Planned periodic transfer |

---

## API List

### Dashboard & Assets (StockDashboardController)

| Method | Endpoint                                            | Purpose                                  | Authorization Required           |
| ------ | --------------------------------------------------- | ---------------------------------------- | -------------------------------- |
| GET    | `/api/v1/stock/dashboard`                           | Main ledger summary (paginated)          | `CEO`, `ADMIN`                   |
| GET    | `/api/v1/stock/dashboard/detail`                    | Detail of product across branches        | `CEO`, `ADMIN`, `BRANCH_MANAGER` |
| POST   | `/api/v1/stock/central-entries`                     | Create new procurement entry             | `CEO`, `ADMIN`                   |
| PUT    | `/api/v1/stock/central-entries`                     | Update central entry (before allocation) | `CEO`, `ADMIN`                   |
| GET    | `/api/v1/stock/central-entries`                     | Fetch specific central entry             | `ALL (Auditor view)`             |
| DELETE | `/api/v1/stock/central-entries`                     | Inactivate a central entry               | `CEO`, `ADMIN`                   |
| POST   | `/api/v1/stock/central-entries/initial-allocations` | Allocate stock to branches               | `CEO`, `ADMIN`                   |
| POST   | `/api/v1/stock/asset-ids/preview`                   | Preview auto-generated Asset IDs         | `CEO`, `ADMIN`                   |
| GET    | `/api/v1/stock/assets`                              | List of all physical asset units         | `CEO`, `ADMIN`, `BRANCH_MANAGER` |
| GET    | `/api/v1/stock/assets/history`                      | Movement history of a specific asset     | `ALL (Auditor view)`             |

### Approvals (StockApprovalController)

| Method | Endpoint                                        | Purpose                             | Authorization Required    |
| ------ | ----------------------------------------------- | ----------------------------------- | ------------------------- |
| GET    | `/api/v1/stock/approval/requests/inbox`         | List of requests waiting for action | `ADMIN`, `BRANCH_MANAGER` |
| GET    | `/api/v1/stock/approval/requests/approval-view` | Detail view for approver            | `Auditor`, `Approver`     |
| POST   | `/api/v1/stock/approval/requests/approve`       | Finalize quantities and approve     | `ADMIN`, `BRANCH_MANAGER` |
| POST   | `/api/v1/stock/approval/requests/reject`        | Deny request with reason            | `ADMIN`, `BRANCH_MANAGER` |
| POST   | `/api/v1/stock/approval/requests/hold`          | Place on hold for clarification     | `ADMIN`, `BRANCH_MANAGER` |
| PUT    | `/api/v1/stock/approval/requests/approval`      | Edit approval decision              | `ADMIN`, `BRANCH_MANAGER` |

### Requests (StockRequestController)

| Method | Endpoint                            | Purpose                                 | Authorization Required |
| ------ | ----------------------------------- | --------------------------------------- | ---------------------- |
| GET    | `/api/v1/stock/requests/my`         | Requests created by the current branch  | `ALL`                  |
| POST   | `/api/v1/stock/requests`            | Create/Save a stock request             | `ALL`                  |
| PUT    | `/api/v1/stock/requests`            | Update a draft request                  | `ALL`                  |
| POST   | `/api/v1/stock/requests/submit`     | Move DRAFT to PENDING                   | `ALL`                  |
| POST   | `/api/v1/stock/requests/recipients` | Save email/notif recipients for the req | `ALL`                  |
| POST   | `/api/v1/stock/requests/revoke`     | Withdraw a pending request              | `ALL`                  |
| GET    | `/api/v1/stock/requests`            | Fetch request details                   | `ALL`                  |
| POST   | `/api/v1/stock/requests/receive`    | Mark items as received at branch        | `ALL`                  |

### Transfers (StockTransferController)

| Method | Endpoint                                  | Purpose                               | Authorization Required    |
| ------ | ----------------------------------------- | ------------------------------------- | ------------------------- |
| POST   | `/api/v1/stock/transfers`                 | Initiate direct transfer (no request) | `ADMIN`, `BRANCH_MANAGER` |
| PUT    | `/api/v1/stock/transfers`                 | Update transfer metadata              | `ADMIN`, `BRANCH_MANAGER` |
| GET    | `/api/v1/stock/transfers`                 | Fetch transfer detail                 | `ALL`                     |
| POST   | `/api/v1/stock/transfers/dispatch`        | Confirm items are shipped             | `ADMIN`, `BRANCH_MANAGER` |
| POST   | `/api/v1/stock/transfers/mark-in-transit` | Automated state change to in-transit  | `ADMIN`, `BRANCH_MANAGER` |
| POST   | `/api/v1/stock/transfers/receive`         | Confirm receipt of transfer           | `ALL`                     |
| GET    | `/api/v1/stock/transfers`                 | List all transfers (list view)        | `ALL`                     |

---

## API Details

### [GET] /api/v1/stock/dashboard

#### Purpose

Fetches a paginated summary of stock across the organization.

#### Query Parameters

| Field    | Type   | Required | Description               | Example  |
| -------- | ------ | -------- | ------------------------- | -------- |
| branchId | String | NO       | Filter by branch          | `BR-001` |
| status   | Enum   | NO       | `StockAvailabilityStatus` | `LOW`    |

#### Full Response JSON Example

```json
{
  "status": 200,
  "message": "Stock dashboard fetched",
  "data": {
    "count": 1,
    "next": null,
    "prev": null,
    "data": [
      {
        "productId": "p1",
        "productCode": "LAP-001",
        "productName": "Laptop Dell",
        "assetsQty": 5,
        "consumableQty": 0,
        "resellQty": 0,
        "totalQty": 5,
        "updatedAt": "2026-03-30T12:00:00Z"
      }
    ]
  }
}
```

---

### [POST] /api/v1/stock/central-entries

#### Purpose

Create a new central entry for freshly procured items.

#### Request Body Fields

| Field              | Type   | Required | Description                   | Example       |
| ------------------ | ------ | -------- | ----------------------------- | ------------- |
| productId          | String | YES      | Master Product ID             | `PROD-001`    |
| productCode        | String | YES      | Snapshot Prod Code            | `LAP-DELL`    |
| productName        | String | YES      | Snapshot Prod Name            | `Dell Laptop` |
| totalQty           | Int    | YES      | Total Procured                | `10`          |
| assetsQty          | Int    | YES      | Part of Assets                | `10`          |
| consumableQty      | Int    | YES      | Part of Consumable            | `0`           |
| resellQty          | Int    | YES      | Part of Resale                | `0`           |
| initialAllocations | List   | NO       | Direct allocation to branches | `[...]`       |

#### Full Request JSON Examples

##### Valid Complete Request

```json
{
  "productId": "p-001",
  "productCode": "LAP-001",
  "productName": "MacBook Pro M3",
  "totalQty": 10,
  "assetsQty": 10,
  "consumableQty": 0,
  "resellQty": 0,
  "assetIdGeneration": "AUTO",
  "assetIdPrefix": "MBP-",
  "assetSequenceStart": 1,
  "initialAllocations": [
    {
      "branchId": "branch-main-uuid",
      "assetsQty": 10
    }
  ]
}
```

---

### [POST] /api/v1/stock/approval/requests/approve

#### Purpose

Approvers use this to authorize a request and set the delivery details.

#### Request Body Fields

| Field                | Type   | Required | Description         | Example           |
| -------------------- | ------ | -------- | ------------------- | ----------------- |
| approvalType         | String | YES      | `FULL` or `PARTIAL` | `FULL`            |
| dispatchDate         | Date   | YES      | Planned dispatch    | `2026-04-05`      |
| expectedDeliveryDate | Date   | YES      | Expected at branch  | `2026-04-07`      |
| carrier              | String | YES      | Delivery service    | `FedEx`           |
| remarks              | String | YES      | Notes for requester | `Urgent dispatch` |
| approvedItems        | List   | NO       | Itemized qty list   | `[...]`           |

#### Full Request JSON Examples

##### Approve Request Example

```json
{
  "approvalType": "FULL",
  "dispatchDate": "2026-04-01",
  "expectedDeliveryDate": "2026-04-03",
  "carrier": "BlueDart",
  "lrNumber": "LR-123456",
  "remarks": "Shipment ready for pickup",
  "approvedItems": [
    {
      "productId": "p-001",
      "productCode": "LAP-001",
      "productName": "Laptop",
      "assetsQty": 5
    }
  ]
}
```

---

### [POST] /api/v1/stock/requests/receive

#### Purpose

Confirm the arrival of items at the requesting branch.

#### Request Body Fields

| Field            | Type    | Required | Description         | Example      |
| ---------------- | ------- | -------- | ------------------- | ------------ |
| receivedDate     | Date    | YES      | Actual arrival date | `2026-04-08` |
| packageCondition | Enum    | YES      | `GOOD`, `DAMAGED`   | `GOOD`       |
| confirmReceipt   | Boolean | NO       | Explicit flag       | `true`       |
| receivedItems    | List    | NO       | Actual qty received | `[...]`      |

#### Full Request JSON Examples

##### Success Receipt Example

```json
{
  "receivedDate": "2026-04-05",
  "packageCondition": "GOOD",
  "confirmReceipt": true,
  "remarks": "Received in perfect condition",
  "receivedItems": [
    {
      "productId": "p-001",
      "productCode": "LAP-001",
      "productName": "Laptop",
      "assetsQty": 5
    }
  ]
}
```

---

### [POST] /api/v1/stock/transfers/dispatch

#### Purpose

Finalize the dispatch of a transfer. Transfers can be standalone or linked to a request.

#### Request Body Fields

| Field                | Type   | Required | Description        | Example      |
| -------------------- | ------ | -------- | ------------------ | ------------ |
| dispatchDate         | Date   | YES      | Actual ship date   | `2026-04-10` |
| expectedDeliveryDate | Date   | YES      | Target arrival     | `2016-04-12` |
| carrier              | String | YES      | Logistics provider | `DHL`        |
| lrNumber             | String | NO       | Tracking number    | `TE-001122`  |

#### Full Request JSON Examples

##### Dispatch Transfer Example

```json
{
  "dispatchDate": "2026-04-10",
  "expectedDeliveryDate": "2026-04-12",
  "carrier": "Internal Courier",
  "lrNumber": "TRACK-7788",
  "remarks": "Sealed with secure tape"
}
```

---

### Response Structures

#### Success Pagination Response

Most list APIs return this structure:

```json
{
  "status": 200,
  "message": "...",
  "data": {
    "count": 100,
    "next": "url-to-next-page",
    "prev": "url-to-prev-page",
    "data": [ ... objects ... ]
  }
}
```

#### Request Detail Response

```json
{
  "requestId": "uuid",
  "status": "APPROVED",
  "priority": "HIGH",
  "fromBranchId": "BR-01",
  "toBranchId": "BR-MAIN",
  "items": [
    {
      "productId": "p1",
      "productCode": "CODE",
      "productName": "Name",
      "assetsQty": 10,
      "consumableQty": 0,
      "resellQty": 0,
      "totalQty": 10
    }
  ]
}
```

---

### Exceptions / Error Cases

| HTTP Status | Reason         | When It Happens                  | Typical Message             | Frontend Handling Note         |
| ----------- | -------------- | -------------------------------- | --------------------------- | ------------------------------ |
| 400         | Validation     | `assets + consumable != total`   | `Quantity split is invalid` | Highlight Qty fields in red    |
| 403         | Forbidden      | Non-admin creating Central Entry | `Access Denied`             | Hide menu for restricted roles |
| 404         | Not Found      | Linked product ID doesn't exist  | `Product not found`         | Refresh master list            |
| 409         | State Conflict | Revoking an already approved req | `Request cannot be revoked` | Disable Revoke button          |

#### Error Response JSON Example (Conflict)

```json
{
  "status": 409,
  "error": "Conflict",
  "message": "Request is already in IN_TRANSIT state and cannot be modified",
  "path": "/api/v1/stock/requests/revoke"
}
```

---

## Frontend Notes

- **Hybrid Data Integrity**: Remember that `productName` and `productCode` are SNAPSHOTTED in stock requests. Do not rely solely on the linked Product Entity for display in historical views; use the Snapshot fields from the DTO.
- **Form UX**: When creating a Stock Request, the `items` array must be validated locally to ensure no negative quantities are entered before submission.
- **Asset ID Management**: Use the `/assets` endpoint to filter by `branchId` to show available local assets for assignment/issuance.
- **Status Progression**:
  - `DRAFT` -> `PENDING_APPROVAL` (Submit)
  - `PENDING_APPROVAL` -> `APPROVED` (Approve)
  - `APPROVED` -> `DISPATCH` -> `IN_TRANSIT` (Dispatch)
  - `IN_TRANSIT` -> `RECEIVED` (Receive)

---

## Postman Friendly cURL

### Create Central Entry

```bash
curl --location 'https://api.yourdomain.com/api/v1/stock/central-entries' \
--header 'Content-Type: application/json' \
--header 'X-Tenant-ID: your-tenant-id' \
--header 'Authorization: Bearer {{token}}' \
--data '{
    "productId": "p-001",
    "productCode": "LAP-001",
    "productName": "MacBook Pro M3",
    "totalQty": 10,
    "assetsQty": 10,
    "consumableQty": 0,
    "resellQty": 0,
    "assetIdGeneration": "AUTO",
    "assetIdPrefix": "MBP-",
    "assetSequenceStart": 1,
    "initialAllocations": [
        {
            "branchId": "branch-main-uuid",
            "assetsQty": 10
        }
    ]
}'
```

### Approve Request

```bash
curl --location 'https://api.yourdomain.com/api/v1/stock/approval/requests/approve?requestId=req-123' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{token}}' \
--data '{
    "approvalType": "FULL",
    "dispatchDate": "2026-04-01",
    "expectedDeliveryDate": "2026-04-03",
    "carrier": "UPS",
    "remarks": "Urgent fulfillment"
}'
```

---

## Validation and Exception Summary

| Field / Scenario | Validation / Rule                 | Error Type                 | Frontend Impact              |
| ---------------- | --------------------------------- | -------------------------- | ---------------------------- |
| Qty split        | `assets + cons + resell == total` | `StockValidationException` | UI Modal Block               |
| Dates            | `expectedDate >= dispatchDate`    | `Validation`               | Disable past dates in picker |
| Revoke           | Must be in `DRAFT` or `PENDING`   | `Concurrency/Conflict`     | Conditional Rendering        |

---

## Frontend Integration Notes

1. **Dropdown Source**:
   - For `branchId`: `/api/v1/tenant/branches`
   - For `productId`: `/api/v1/inventory/products`
2. **Read-only**: Fields like `requestId` and `fromBranchId` should be disabled in the "Update" view.
3. **Empty States**: If `/dashboard` returns empty `data` array, show "No Stock Ledger Entries Found".
