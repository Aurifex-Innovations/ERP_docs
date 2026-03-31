# [Quotation Management - Module 16]

## Short Description
The Quotation Management module is a core business component designed to handle the creation, negotiation, and lifecycle tracking of customer service and product quotations. It supports a multi-step "Wizard" flow on the frontend, allowing sales teams to link quotes to existing Leads/Customers or create on-the-fly "Prospects." 

Frontend developers need to understand the **Draft vs. Send** logic, where a quotation can be saved incrementally as a draft and later validated for "completion" before being dispatched to the client via email (with automated PDF generation).

---

## Authorization

*   **Authentication Type:** JWT Bearer Token
*   **Required Token:** `Authorization: Bearer <token>`
*   **Required Headers:**
    *   `Content-Type: application/json`
    *   `X-Tenant-ID`: Required for tenant-specific data isolation and PDF branding.
*   **Required Roles/Permissions:**
    *   `SALES_EXECUTIVE` / `ADMIN` / `CEO`
    *   Specific authorities: `QUOTATION_READ`, `QUOTATION_WRITE`, `QUOTATION_DELETE`.
*   **Access Restrictions:** 
    *   **Edit/Delete:** Only allowed when status is `DRAFT`.
    *   **PDF/Resend:** Only available after status identifies as `SENT` or higher.
    *   **Lifecycle:** Status transitions are logic-gate protected (e.g., cannot go from `REJECTED` to `DRAFT`).

---

## Enums Used In This Module

### QuotationSourceType
Determines the client linkage for the quotation.
| Value | Meaning | Used In |
| :--- | :--- | :--- |
| `FROM_LEAD` | Linked to an existing CRM Lead | Create Request, List/Detail Response |
| `FROM_CUSTOMER` | Linked to a registered Company/Customer | Create Request, List/Detail Response |
| `NEW_PROSPECT` | For walk-in clients not yet in the DB | Create Request, List/Detail Response |

### QuotationStatus
The current state in the negotiation/fulfillment pipeline.
| Value | Meaning | Used In |
| :--- | :--- | :--- |
| `DRAFT` | Saved locally, not yet visible to client | List/Detail Response |
| `SENT` | Dispatched to client email, PDF generated | List/Detail Response |
| `VIEWED` | Client has opened the unique tracking link | List/Detail Response |
| `ACCEPTED` | Client approved via portal or manual update | List/Detail Response |
| `REJECTED` | Client declined the offer | List/Detail Response |
| `EXPIRED` | Quote validity date has passed | List/Detail Response |
| `REVISED` | Superseded by a newer version | List/Detail Response |

### QuotationTypeEnum
| Value | Meaning | Used In |
| :--- | :--- | :--- |
| `SERVICE` | Pure service quote (e.g., Pest Control visits) | Create Request, List/Detail Response |
| `PRODUCT` | Pure physical product sale (e.g., Sprayers) | Create Request, List/Detail Response |
| `COMBINED` | Both service visits and product sales | Create Request, List/Detail Response |

### ServiceModeEnum
| Value | Meaning | Used In |
| :--- | :--- | :--- |
| `ONE_TIME` | Single visit/job | Create Request, Detail Response |
| `CONTRACT` | AMC (Annual Maintenance Contract) with visits | Create Request, Detail Response |

### ContractFrequency
| Value | Meaning | Used In |
| :--- | :--- | :--- |
| `MONTHLY` | 12 Visits per year | Create Request, Detail Response |
| `QUARTERLY` | 4 Visits per year | Create Request, Detail Response |
| `HALF_YEARLY` | 2 Visits per year | Create Request, Detail Response |
| `YEARLY` | 1 Visit per year | Create Request, Detail Response |

---

## API List

| Method | Endpoint | Purpose | Authorization Required |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/v1/quotations` | Fetch paginated dashboard list | Yes |
| `POST` | `/api/v1/quotations` | Create new quotation (Draft or Send) | Yes |
| `GET` | `/api/v1/quotations/by-id` | View full quotation detail | Yes |
| `DELETE` | `/api/v1/quotations` | Soft-delete a DRAFT quotation | Yes |
| `POST` | `/api/v1/quotations/{id}/send` | Send a DRAFT to client (Status -> SENT) | Yes |
| `POST` | `/api/v1/quotations/{id}/resend` | Re-send email/PDF to client | Yes |
| `GET` | `/api/v1/quotations/{id}/pdf` | Download current PDF version | Yes |
| `POST` | `/api/v1/quotations/{id}/attachments`| Upload supplemental files | Yes |

---

## API Details

### [POST] /api/v1/quotations
Purpose: The primary entry point for creating quotations. Use `saveDraft: true` for incremental saves and `saveDraft: false` for final submission/dispatch.

Authorization:
*   Token required: Yes
*   Roles: `SALES_EXECUTIVE`, `ADMIN`, `CEO`
*   Required Headers: `X-Tenant-ID`, `Authorization`

Request Body Fields:
| Field | Type | Required | Validation | Description |
| :--- | :--- | :--- | :--- | :--- |
| `sourceType` | Enum | Yes | `QuotationSourceType` | Lead, Customer, or Prospect |
| `leadId` | String | If Source=LEAD| UUID/String | ID of existing Lead |
| `customerId` | String | If Source=CUST| UUID/String | ID of existing Customer |
| `prospect` | Object | If Source=NEW | `@Valid` | New client data (see Prospect Object) |
| `quotationType` | Enum | Yes | `QuotationTypeEnum` | SERVICE, PRODUCT, or COMBINED |
| `serviceMode` | Enum | No | `ServiceModeEnum` | Req for SERVICE/COMBINED types |
| `contractDuration`| Enum | If CONTRACT | `SIX_MONTHS`, `ONE_YEAR`... | Duration of AMC |
| `locations` | List | If SERVICE | `@Valid` | List of sites and service lines |
| `productLines` | List | If PRODUCT | `@Valid` | List of inventory items |
| `discountType` | Enum | No | `PERCENTAGE` / `FLAT_AMOUNT` | Type of discount |
| `discountValue` | Decimal| No | 0 - 100 for % | Rate of discount |
| `validTill` | Date | Yes | `yyyy-MM-dd` | Expiry: Must be >= today |
| `paymentTerms` | Enum | Yes | `PaymentTermsEnum` | e.g. `NET_30`, `CUSTOM` |
| `saveDraft` | Boolean| Yes | Default: `true` | If `false`, triggers SEND logic |

**Nested: `prospect` Object**
| Field | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `fullName` | String | Yes | Client representative name |
| `phone` | String | Yes | 10-digit mobile number |
| `email` | String | No | Valid email for PDF delivery |
| `address` | String | Yes | Prospect's base location |

**Nested: `locations` List Item**
| Field | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `branchId` | String | Yes | Serving branch ID |
| `areaSqft` | Decimal| Yes | Site area for pricing |
| `serviceLines` | List | Yes | Specific services at this site |

Full Request JSON Examples:

#### Minimal Valid Request (Draft Only)
```json
{
  "sourceType": "FROM_LEAD",
  "leadId": "LD-2026-X1Y2",
  "quotationType": "PRODUCT",
  "validTill": "2026-05-30",
  "paymentTerms": "NET_30",
  "saveDraft": true
}
```

#### Complete Valid Request (Combined & Send)
```json
{
  "sourceType": "NEW_PROSPECT",
  "prospect": {
    "fullName": "Aryan Raval",
    "phone": "9876543210",
    "email": "aryan@example.com",
    "address": "Skyline Tower, Mumbai",
    "city": "Mumbai",
    "state": "Maharashtra"
  },
  "quotationType": "COMBINED",
  "serviceMode": "CONTRACT",
  "contractFrequency": "MONTHLY",
  "contractDuration": "ONE_YEAR",
  "contractProposedStart": "2026-04-01",
  "locations": [
    {
      "address": "Office A, 4th Floor",
      "city": "Mumbai",
      "state": "Maharashtra",
      "locationCategory": "COMMERCIAL",
      "locationSubCategory": "INTERNAL",
      "areaSqft": 2500,
      "branchId": "BR-MUM-01",
      "serviceLines": [
        {
          "serviceId": "SVC-PEST-01",
          "priceType": "FIXED",
          "ratePerVisit": 1200.00,
          "visitFrequency": "MONTHLY",
          "totalVisits": 12
        }
      ]
    }
  ],
  "productLines": [
    {
      "productId": "PRD-GEL-05",
      "unitPrice": 450.00,
      "quantity": 10
    }
  ],
  "discountType": "PERCENTAGE",
  "discountValue": 10.0,
  "validTill": "2026-06-15",
  "paymentTerms": "FIFTY_ADVANCE_FIFTY_COMPLETION",
  "saveDraft": false
}
```

Response:
*   Success Status: `201 Created` (Draft) / `200 OK` (Sent)
*   Structure: [QuotationDetailResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/quotation/dto/response/QuotationDetailResponse.java#14-123) containing calculated `grandTotal` and `taxTotal`.

Full Response JSON Examples:

#### Success Response (Creation)
```json
{
  "status": "success",
  "message": "Quotation created and sent successfully",
  "data": {
    "id": "QT-2026-B99C",
    "quotationNumber": "QT-2026-B99C",
    "status": "SENT",
    "clientName": "Aryan Raval",
    "grandTotal": 17582.00,
    "canEdit": false,
    "canSend": false
  }
}
```

---

### [GET] /api/v1/quotations
Purpose: Retrieves a paginated list for the main dashboard. Supports advanced filtering.

Query Parameters:
| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `pageNo` | Int | No | Default: 0 |
| `pageSize` | Int | No | Default: 10 |
| `statuses` | List | No | Filter by Status (SENT, DRAFT...) |
| `search` | String | No | Client name or Quote number |
| `amountMin/Max`| Decimal| No | Filter by Grand Total range |

Full Response JSON Examples:

#### Paginated List Response
```json
{
  "status": "success",
  "data": {
    "content": [
      {
        "id": "QT-2026-B99C",
        "clientName": "Aryan Raval",
        "grandTotal": 17582.00,
        "status": "SENT",
        "validTill": "2026-06-15",
        "canDelete": false
      }
    ],
    "pageNo": 0,
    "pageSize": 10,
    "totalElements": 1,
    "totalPages": 1,
    "last": true
  }
}
```

---

### Exceptions / Error Cases

| HTTP Status | Reason | When It Happens | Typical Message | Frontend Note |
| :--- | :--- | :--- | :--- | :--- |
| `400` | Validation | Missing `locations` in `SENT` mode | `Locations are required before sending` | Shake form / show red highlight |
| `400` | status Lock | Trying to DELETE a `SENT` quote | `Only DRAFT quotations can be deleted` | Hide delete icon in UI list |
| `400` | XOR Failure | Providing both `leadId` and `customerId` | `Exactly one source must be selected` | UI logic should reset one on select |
| `403` | Unauthorized | User without Sales/Admin role | `Access Denied` | Redirect to Login/Portal |
| `413` | Size Limit | Uploaded attachment > 5MB | `File size exceeds 5 MB limit` | Check size locally before upload |

Error Response JSON Examples:

#### Business Rule Failure
```json
{
  "status": "error",
  "message": "Contract frequency required for CONTRACT service mode",
  "code": 400
}
```

#### Validation Error (Postgres Constraint)
```json
{
  "status": "error",
  "message": "Exactly one source (Lead, Customer, or Prospect) must be selected.",
  "code": 400
}
```

---

## Frontend Notes

*   **Wizard Flow:** Split the creation form into: 1. Client Source, 2. Quote Configuration (Type/Mode), 3. Service Mapping (Locations/Services), 4. Product Lines, 5. Terms & Discounts, 6. Preview & Finalize.
*   **Draft Logic:** Encourage "Save Draft" frequently. Drafters do not need to fill in service lines or products immediately.
*   **Rounding:** Backend uses 2 decimal places. Frontend should format using `.toFixed(2)` but send raw numbers to API.
*   **File Uploads:** Use `POST /api/v1/quotations/{id}/attachments` for supplemental files after the initial creation. Backend accepts Base64 in `filedata`.

---

## cURL Representative Example

```bash
curl --location 'http://localhost:8080/api/v1/quotations' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <JWT_TOKEN>' \
--header 'X-Tenant-ID: <TENANT_ID>' \
--data '{
    "sourceType": "FROM_CUSTOMER",
    "customerId": "CUST-987",
    "quotationType": "SERVICE",
    "serviceMode": "ONE_TIME",
    "validTill": "2026-04-15",
    "paymentTerms": "FULL_ADVANCE",
    "saveDraft": true
}'
```

---

## Validation and Exception Summary

| Field / Scenario | Validation / Rule | Error Type | Frontend Impact |
| :--- | :--- | :--- | :--- |
| `validTill` | `@FutureOrPresent` | `400` | Disable past dates in Calendar |
| `attachments` | Max 5 files | `400` | Limit file input component to 5 |
| `customPaymentTerms`| Min 10 chars | `400` | Block 'Submit' if length < 10 |
| Deletion | Status must be `DRAFT` | `400` | Show 'Delete' only via `canDelete` flag |

---

## Frontend Integration Notes

*   **Dropdown APIs:** Use `/api/v1/quotations/dropdowns/leads` and `/api/v1/quotations/dropdowns/services` to populate selection lists.
*   **ReadOnly Fields:** Once status is `SENT`, the Entire form should be read-only to prevent tampering. Revisions should be created as whole new IDs.
*   **Calculation Note:** `grandTotal` and `taxTotal` provided in response are Source of Truth. Avoid local math for final invoice display.
