# Purchase Order Management (Module 14)

## Short Description
The Purchase Order (PO) Management module enables businesses to manage the procurement of physical goods from vendors. It tracks the entire lifecycle of a purchase order—from initial drafting and approval to ordering and eventual receipt of goods. 

Frontend developers use this module to build the "Create Purchase Order" wizard, the "PO History Dashboard," and the "PO Detail" view with status-based action buttons (e.g., Approve, Receive, Cancel).

---

## Authorization

*   **Authentication Type:** JWT Bearer Token
*   **Required Token:** `Authorization: Bearer <token>`
*   **Required Headers:**
    *   `Content-Type: application/json`
    *   `X-Tenant-ID`: Required for tenant data isolation.
*   **Access Restrictions:** 
    *   Requires role `CEO` or specific authorities:
        *   `PURCHASE_ORDER_MANAGEMENT_ADD` (Create)
        *   `PURCHASE_ORDER_MANAGEMENT_EDIT` (Update)
        *   `PURCHASE_ORDER_MANAGEMENT_READ` (View)
        *   `PURCHASE_ORDER_MANAGEMENT_DELETE` (Delete)
*   **Business Logic Restrictions:**
    *   **Editing:** Full edits are only allowed when the PO is in `DRAFT` status. Once `APPROVED`, only the status can be updated.
    *   **Deletion:** Only `DRAFT` purchase orders can be soft-deleted.

---

## Enums Used In This Module

### PurchaseOrderStatus
Tracks the current state of the procurement process.
| Value | Meaning | Used In |
| :--- | :--- | :--- |
| `DRAFT` | Initial creation, fully editable | Create/Update, List/Detail Response |
| `PENDING_APPROVAL` | Submitted for internal review | Update, List/Detail Response |
| `APPROVED` | Approved, ready to be sent to vendor | Update, List/Detail Response |
| `ORDERED` | Order officially placed with vendor | Update, List/Detail Response |
| `PARTIALLY_RECEIVED` | Some items have been delivered | Update, List/Detail Response |
| `RECEIVED` | Full delivery received | Update, List/Detail Response |
| `CANCELLED` | Order retracted | Update, List/Detail Response |

---

## API List
| Method | Endpoint | Purpose | Authorization Required |
| :--- | :--- | :--- | :--- |
| `POST` | `/purchase-orders` | Create a new Purchase Order | Yes |
| `PUT` | `/purchase-orders/update` | Update PO details or status | Yes |
| `GET` | `/purchase-orders/get-by-id` | Get full PO details by ID | Yes |
| `GET` | `/purchase-orders` | Filtered paginated list of POs | Yes |
| `DELETE` | `/purchase-orders/delete` | Soft delete a DRAFT PO | Yes |

---

## API Details

### [POST] /purchase-orders
Purpose: Creates a new Purchase Order. The backend automatically handles PO number generation and financial calculations (Subtotal, Tax, Grand Total).

Authorization:
*   Token required: Yes
*   Roles/Authorities: `CEO`, `PURCHASE_ORDER_MANAGEMENT_ADD`

Request Body Fields:
| Field | Type | Required | Validation | Description | Example |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `poDate` | String | Yes | `yyyy-MM-dd` | Date of PO creation | `2026-03-30` |
| `status` | Enum | No | `PurchaseOrderStatus`| Default is `DRAFT` | `DRAFT` |
| `vendorName` | String | Yes | Must match DB | Name of the existing vendor | `Global Chemical Supplies` |
| `deliveryAddress` | String | Yes | | Final destination for items | `Warehouse A, Bangalore` |
| `contactPerson` | String | Yes | | Person at delivery site | `John Doe` |
| `contactNumber` | String | Yes | | Phone of site contact | `9876543210` |
| `deliveryDate` | String | Yes | `yyyy-MM-dd` | Expected delivery date | `2026-04-15` |
| `designation` | String | No | | Designation of site contact | `Warehouse Manager` |
| `note` | String | No | | Internal/Vendor notes | `Handle with care.` |
| `items` | List | Yes | At least one item | List of products to order | `[...]` |

**Items Object Structure (`items`):**
| Field | Type | Required | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| `productName` | String | Yes | Must match existing product | `Pest Spray 500ml` |
| `quantity` | Decimal| Yes | Amount to order | `50.00` |
| `uom` | String | Yes | Unit of Measure | `Units`, `Liters` |
| `price` | Decimal| Yes | Unit price before tax | `450.00` |
| `gstPercent` | Decimal| Yes | Tax percentage | `18.00` |

Full Request JSON Examples:

#### Minimal Valid Request (Draft)
```json
{
    "poDate": "2026-03-30",
    "vendorName": "Global Chemical Supplies",
    "deliveryAddress": "Sector 5, Warehouse B",
    "contactPerson": "Alice Smith",
    "contactNumber": "9888877777",
    "deliveryDate": "2026-04-10",
    "items": [
        {
            "productName": "Hand Gloves",
            "quantity": 100,
            "uom": "Pairs",
            "price": 25.00,
            "gstPercent": 12.00
        }
    ]
}
```

#### Complete Valid Request
```json
{
    "poDate": "2026-03-30",
    "status": "APPROVED",
    "vendorName": "Global Chemical Supplies",
    "deliveryAddress": "Plot 12, Industrial Area, Noida",
    "contactPerson": "Bob Johnson",
    "contactNumber": "9988776655",
    "deliveryDate": "2026-04-20",
    "designation": "Operations Head",
    "note": "Include HSN codes in invoice.",
    "items": [
        {
            "productName": "Pest Spray 500ml",
            "quantity": 50,
            "uom": "Units",
            "price": 450.00,
            "gstPercent": 18.00
        }
    ]
}
```

Response:
*   Success Status: `201 Created`
*   Response Structure: [PurchaseOrderResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/PurchaseOrder/dto/response/PurchaseOrderResponse.java#9-30)
*   Notes: `grandTotal` and `poNumber` are returned by the server.

Full Response JSON Example:
```json
{
    "status": "success",
    "message": "Purchase Order Created Successfully",
    "data": {
        "id": "PO-AB12",
        "vendorName": "Global Chemical Supplies",
        "poDate": "2026-03-30",
        "deliveryDate": "2026-04-20",
        "itemsCount": 1,
        "grandTotal": 26550.00,
        "status": "APPROVED"
    }
}
```

---

### [PUT] /purchase-orders/update
Purpose: Updates an existing Purchase Order. 

Authorization:
*   Token required: Yes
*   Roles/Authorities: `CEO`, `PURCHASE_ORDER_MANAGEMENT_EDIT`

Path Parameters: Not applicable.
Query Parameters:
| Field | Type | Required | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| [id](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/quotation/service/QuotationServiceImpl.java#519-539) | String | Yes | The internal ID of the PO | `PO-AB12` |

Request Body Fields:
Same structure as [POST]. 

**Important Status Transition Rules:**
*   If current status is `DRAFT`: You can update all fields including items.
*   If current status is `APPROVED`: Only the `status` field can be updated (e.g., to `ORDERED`). Any attempts to change items or vendor info will be rejected by the backend.
*   Other statuses: No updates allowed.

Full Request JSON Examples:

#### Update Status Only (From Approved to Ordered)
```json
{
    "status": "ORDERED"
}
```

#### Update Full Draft
```json
{
    "poDate": "2026-03-31",
    "vendorName": "Alt Vendor Pvt Ltd",
    "deliveryAddress": "New Address Lane 2",
    "contactPerson": "Charlie",
    "contactNumber": "9111122222",
    "deliveryDate": "2026-04-15",
    "items": [
        {
            "productName": "Sanitizer 5L",
            "quantity": 10,
            "uom": "Cans",
            "price": 800.00,
            "gstPercent": 18.00
        }
    ]
}
```

Response:
*   Success Status: `200 OK`
*   Response Structure: [PurchaseOrderResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/PurchaseOrder/dto/response/PurchaseOrderResponse.java#9-30)

---

### [GET] /purchase-orders/get-by-id
Purpose: Fetches complete details of a Purchase Order, including vendor info, line items with tax breakdowns, and audit metadata.

Query Parameters:
| Field | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| [id](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/quotation/service/QuotationServiceImpl.java#519-539) | String | Yes | The internal ID of the PO |

Full Response JSON Example:
```json
{
    "status": "success",
    "message": "Purchase Order Fetched Successfully",
    "data": {
        "id": "PO-AB12",
        "poNumber": "PO-1711812345678",
        "poDate": "2026-03-30",
        "deliveryDate": "2026-04-20",
        "status": "APPROVED",
        "gstNumber": "27AAAAA0000A1Z5",
        "vendorId": "VND-101",
        "vendorName": "Global Chemical Supplies",
        "vendorAddress": "123 Supply Lane, Pune",
        "vendorGst": "27AAAAA1111A1Z2",
        "deliveryAddress": "Plot 12, Noida",
        "contactPerson": "Bob Johnson",
        "contactNumber": "9988776655",
        "authorizedPerson": "Aryan Raval",
        "designation": "Operations Head",
        "note": "Include HSN codes.",
        "itemsCount": 1,
        "subtotal": 22500.00,
        "totalTax": 4050.00,
        "grandTotal": 26550.00,
        "branchName": "Main Branch",
        "createdAt": "2026-03-30T10:00:00Z",
        "items": [
            {
                "id": "item-uuid-1",
                "productId": "PRD-500",
                "productName": "Pest Spray 500ml",
                "quantity": 50,
                "uom": "Units",
                "price": 450.00,
                "gstPercent": 18.00,
                "taxAmount": 4050.00,
                "totalAmount": 26550.00
            }
        ]
    }
}
```

---

### [GET] /purchase-orders (Filter/Search)
Purpose: Retrieves a paginated list of purchase orders based on filters.

Query Parameters:
| Field | Type | Required | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| `pageNo` | Int | No | Page index (Default: 0) | `0` |
| `pageSize` | Int | No | Rows per page (Default: 10) | `10` |
| `search` | String | No | Search by PO Number | `PO-1711` |
| `vendorName` | String | No | Exact vendor name match | `Global Chemical`|
| `status` | Enum | No | `PurchaseOrderStatus` | `DRAFT` |
| `branchName` | String | No | Branch filter | `Main Branch` |
| `fromDate` | Date | No | Filter by poDate (Start) | `2026-03-01` |
| `toDate` | Date | No | Filter by poDate (End) | `2026-03-31` |

Response JSON Example (Paginated):
```json
{
    "status": "success",
    "message": "Purchase Orders Fetched Successfully",
    "data": {
        "count": 15,
        "data": [
            {
                "id": "PO-AB12",
                "vendorName": "Global Chemical Supplies",
                "poDate": "2026-03-30",
                "deliveryDate": "2026-04-20",
                "itemsCount": 1,
                "grandTotal": 26550.00,
                "status": "APPROVED"
            }
        ],
        "next": "http://localhost:8080/api/v1/purchase-orders?pageSize=10&pageNo=1",
        "prev": null
    }
}
```

---

## Exceptions / Error Cases

| HTTP Status | Reason | When It Happens | Typical Message | Frontend Handling Note |
| :--- | :--- | :--- | :--- | :--- |
| `400` | Invalid Status Change| Trying to edit items of an `APPROVED` PO | `Cannot edit PO in status: APPROVED` | Disable Item Table if status != DRAFT |
| `400` | Deletion Restricted | Trying to delete a non-DRAFT PO | `Only Draft can be deleted` | Hide Delete button for non-Draft |
| `400` | Partial Update Error | Missing status field in APPROVED update | `Only status update allowed` | Show status dropdown only for Approved |
| `404` | Resource Not Found | vendorName or productName not in DB | `Vendor not found` or `Product not found: X` | Ensure user selects from dropdowns |
| `404` | Record Not Found | Invalid PO ID | `PO Not Found` | Redirect to list view |

Error Response JSON Example (Business Rule):
```json
{
    "status": "error",
    "message": "Cannot edit PO in status: ORDERED",
    "code": 400
}
```

---

## Frontend Notes

1.  **Selection-Centric Flow:** 
    *   `vendorName` and `productName` are lookups. Do not let users type arbitrary names; use autocomplete/dropdown components.
    *   Vendor dropdown should be fetched from `/api/v1/vendors/dropdown`.
    *   Product dropdown should be fetched from the Inventory module.
2.  **Status-Driven UI:**
    *   **Draft State:** All fields editable. Action: "Submit for Approval" (sets status to PENDING_APPROVAL).
    *   **Approved State:** UI should be read-only except for a "Mark as Ordered" or "Cancel" button.
    *   **Ordered State:** UI should enable "Receive Items" workflow.
3.  **Date Formatting:** All dates in the request are strings in `yyyy-MM-dd` format. Use a date picker that outputs this ISO string.
4.  **Calculations:** While the frontend can show a preview sum, always trust the `grandTotal` returned by the server response after creation/update.

---

## cURL Example

```bash
curl --location --request POST 'http://localhost:8080/purchase-orders' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{jwt_token}}' \
--header 'X-Tenant-ID: {{tenant_id}}' \
--data '{
    "poDate": "2026-03-31",
    "vendorName": "Global Chemical Supplies",
    "deliveryAddress": "Plot 12, Noida",
    "contactPerson": "Bob Johnson",
    "contactNumber": "9988776655",
    "deliveryDate": "2026-04-20",
    "items": [
        {
            "productName": "Pest Spray 500ml",
            "quantity": 10,
            "uom": "Units",
            "price": 400.0,
            "gstPercent": 18.0
        }
    ]
}'
```

---

## Validation and Exception Summary
| Field / Scenario | Validation / Rule | Error Type | Frontend Impact |
| :--- | :--- | :--- | :--- |
| `vendorName` | Must exist in Vendors module | `404 Not Found` | Validate via Dropdown selection |
| `productName` | Must exist in Inventory module | `404 Not Found` | Validate via Dropdown selection |
| `poDate` / `deliveryDate` | Format `yyyy-MM-dd` | `400 Bad Request`| Use DatePicker with ISO format |
| Status Transition | Items immutable once not in `DRAFT` | `400 Bad Request`| Lock form fields in UI |
| Deletion | Status must be `DRAFT` | `400 Bad Request`| Hide delete button for Sent/Received POs |

---

## Frontend Integration Notes
*   **Dependent Fields:** Selecting a `vendorName` should ideally auto-fill `vendorAddress` and `vendorGst` for the user's review (fetched from vendor details).
*   **Inconsistent Prefix:** Note that this module currently uses `/purchase-orders` (without `/api/v1`) in the controller, although internal pagination links may contain `/api/v1`. Verify the final production gateway prefix.
*   **Search/List:** The filter API returns a flattened [PurchaseOrderResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/PurchaseOrder/dto/response/PurchaseOrderResponse.java#9-30). For full line-items, you must call the `get-by-id` endpoint.
