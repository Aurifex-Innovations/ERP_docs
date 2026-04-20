# Service Management (Module 12)

## Short Description

The Service Management module is responsible for defining and managing the catalog of services offered by the business. It allows administrators to configure service details, pricing models (Fixed, Area-Based, Inspection Fee, or Custom Blocks), target pest types, treatment methods, and required chemical products. Frontend developers need to use this module to build the service creation wizard, the service dashboard, and to fetch master data for dropdowns used across the application (like in Sales or Operations).

## Authorization

- **Authentication Type**: Bearer Token (JWT)
- **Required Token**: Standard user token with appropriate permissions.
- **Required Roles / Authorities**:
  - `hasRole('CEO')` (Overriding access for all management APIs)
  - `SERVICE_MANAGEMENT_ADD`: Required to create services and master records.
  - `SERVICE_MANAGEMENT_EDIT`: Required to update existing services.
  - `SERVICE_MANAGEMENT_READ`: Required to view service details, lists, and audit logs.
- **Headers**:
  - `Authorization`: `Bearer <token>`
  - `X-Tenant-ID`: (Inferred from project context)
- **Access Restrictions**:
  - Creation/Update requires specific permissions.
  - Viewing services is restricted to `READ` authority or CEO role.

## Enums Used In This Module

### ServiceStatus

| Value      | Meaning                                             | Used In                      |
| :--------- | :-------------------------------------------------- | :--------------------------- |
| `ACTIVE`   | Service is available for new contracts/orders.      | All Detail/Summary Responses |
| `INACTIVE` | Service is deactivated and unavailable for new use. | All Detail/Summary Responses |

### ServicePriceType

| Value        | Meaning                                       | Used In                       |
| :----------- | :-------------------------------------------- | :---------------------------- |
| `FIXED`      | Single flat rate pricing.                     | Service Definition, Dashboard |
| `AREA_BASED` | Pricing calculated based on square footage.   | Service Definition, Dashboard |
| `INSPECTION` | Flat fee for an initial inspection or survey. | Service Definition, Dashboard |

### ServiceDurationUom

| Value     | Meaning                        | Used In                             |
| :-------- | :----------------------------- | :---------------------------------- |
| `MINUTES` | Duration specified in minutes. | Service Definition Request/Response |
| `HOURS`   | Duration specified in hours.   | Service Definition Request/Response |

### ServiceChangeType

| Value        | Meaning                                 | Used In    |
| :----------- | :-------------------------------------- | :--------- |
| `CREATE`     | Service was newly created.              | Audit Logs |
| `UPDATE`     | Service details were modified.          | Audit Logs |
| `DEACTIVATE` | Service status was changed to INACTIVE. | Audit Logs |

## API List

| Method | Endpoint                                | Purpose                                        | Authorization Required |
| :----- | :-------------------------------------- | :--------------------------------------------- | :--------------------- |
| `POST` | `/api/v1/services`                      | Create a new service definition                | Yes                    |
| `PUT`  | `/api/v1/services/update`               | Update an existing service                     | Yes                    |
| `GET`  | `/api/v1/services/by-id`                | Get full details of a specific service         | Yes                    |
| `GET`  | `/api/v1/services`                      | Paginated search and dashboard                 | Yes                    |
| `GET`  | `/api/v1/services/audit-logs`           | View modification history of a service         | Yes                    |
| `POST` | `/api/v1/service-categories`            | Create service category                        | Yes                    |
| `GET`  | `/api/v1/service-categories`            | List active categories for dropdowns           | No (Assumed)           |
| `GET`  | `/api/v1/service-sub-categories`        | List active sub-categories (Internal/External) | No (Assumed)           |
| `POST` | `/api/v1/service-pest-types`            | Create pest type                               | Yes                    |
| `GET`  | `/api/v1/service-pest-types`            | List active pest types                         | No (Assumed)           |
| `POST` | `/api/v1/service-treatments`            | Create treatment method                        | Yes                    |
| `GET`  | `/api/v1/service-treatments`            | List active treatments                         | No (Assumed)           |
| `POST` | `/api/v1/service-category-fixed`        | Create pricing tier (Fixed)                    | Yes                    |
| `GET`  | `/api/v1/service-category-fixed`        | List fixed pricing templates                   | No (Assumed)           |
| `POST` | `/api/v1/service-category-area`         | Create pricing template (Area)                 | Yes                    |
| `GET`  | `/api/v1/service-category-area`         | List area pricing templates                    | No (Assumed)           |
| `POST` | `/api/v1/service-category-inspection`   | Create inspection fee template                 | Yes                    |
| `GET`  | `/api/v1/service-category-inspection`   | List inspection fee templates                  | No (Assumed)           |
| `POST` | `/api/v1/service-custom-pricing-blocks` | Create custom pricing group                    | Yes                    |
| `GET`  | `/api/v1/service-custom-pricing-blocks` | List custom pricing blocks                     | No (Assumed)           |

---

## API Details

### [POST] `/api/v1/services`

**Purpose**: Creates a new service definition. This API handles the complex service creation wizard including pricing logic and chemical requirements.

**Authorization**

- `Bearer token required`
- `hasRole('CEO')` or `SERVICE_MANAGEMENT_ADD`

**Request Body Fields**
| Field | Type | Required | Validation | Description | Example | Allowed Values |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `serviceName` | `String` | Yes | Not Blank, Unique | Name of the service. | "General Pest Control" | |
| `description` | `String` | Yes | Not Blank | Detailed description. | "Full property treatment..." | |
| `priceType` | `Enum` | Yes | Not Null | Primary pricing model. | `FIXED` | `FIXED`, `AREA_BASED`, `INSPECTION` |
| `durationValue` | `Double` | Yes | Not Null | Standard duration of visit. | 60 | |
| `durationUom` | `Enum` | Yes | Not Null | Time unit for duration. | `MINUTES` | `MINUTES`, `HOURS` |
| `status` | `Enum` | Yes | Not Null | Initial status. | `ACTIVE` | `ACTIVE`, `INACTIVE` |
| `draft` | `Boolean` | No | | If true, skips master ID validation. | `false` | |
| `visitsPerMonth` | `Double` | No | | Expected monthly visits. | 1.0 | |
| `warrantyMonths` | `Integer` | No | | Warranty period in months. | 6 | |
| `freeRevisitIncluded`| `Boolean` | No | | Are free revisits allowed? | `true` | |
| `freeRevisitQuantity`| `Integer` | No | | Number of free revisits. | 2 | |
| `displayOrder` | `Integer` | No | | Order in UI lists. | 1 | |
| `inactiveReason` | `String` | Cond. | Mandatory if status=INACTIVE | Why is it inactive? | "Market change" | |
| `categoryIds` | `List<String>`| Yes* | Not Empty if draft=false | List of Category UUIDs. | `["SCAT-1234"]` | |
| `subCategoryIds` | `List<String>`| Yes* | Not Empty if draft=false | List of Sub-Category UUIDs. | `["SSUB-5678"]` | |
| `pestTypeIds` | `List<String>`| Yes* | Not Empty if draft=false | Targeted Pest IDs. | `["SPT-9012"]` | |
| `treatmentIds` | `List<String>`| Yes* | Not Empty if draft=false | Treatment Method IDs. | `["STRT-3456"]` | |
| `categoryFixedIds` | `List<String>`| No | | Linked Fixed Pricing IDs. | `["SCF-7890"]` | |
| `categoryAreaIds` | `List<String>`| No | | Linked Area Pricing IDs. | `["SCA-1122"]` | |
| `categoryInspectionIds`| `List<String>`| No | | Linked Inspection Fee IDs. | `["SCI-3344"]` | |
| `customPricingBlockIds`| `List<String>`| No | | Linked Custom Block IDs. | `["SCPB-5566"]` | |
| `species` | `List<Object>`| No | | Nested species details. | (See Example) | |
| `species[].speciesName`| `String` | Yes | | Common name. | "Cockroach" | |
| `species[].scientificName`| `String` | No | | Latin name. | "Blattodea" | |
| `products` | `List<Object>`| Yes\* | Not Empty if draft=false | Chemicals/Inventory items. | (See Example) | |
| `products[].inventoryProductId`| `String` | Yes | | Product ID from Inventory. | "PROD-101" | |
| `products[].requiredQty`| `Double` | Yes | | Quantity per visit. | 250.0 | |
| `products[].pricePerUom`| `Double` | Yes | | Unit cost. | 2.5 | |

**Full Request JSON Examples**

_Minimal Valid Request (Draft)_

```json
{
  "serviceName": "Draft Pest Service",
  "description": "Draft setup",
  "priceType": "FIXED",
  "durationValue": 30,
  "durationUom": "MINUTES",
  "status": "ACTIVE",
  "draft": true
}
```

_Complete Valid Request_

```json
{
  "serviceName": "Commercial Rodent Control",
  "description": "Monthly interior and exterior rodent monitoring",
  "priceType": "FIXED",
  "durationValue": 1.5,
  "durationUom": "HOURS",
  "status": "ACTIVE",
  "draft": false,
  "visitsPerMonth": 1,
  "warrantyMonths": 12,
  "freeRevisitIncluded": true,
  "freeRevisitQuantity": 1,
  "categoryIds": ["SCAT-ABCD"],
  "subCategoryIds": ["SSUB-EFGH"],
  "pestTypeIds": ["SPT-IJKL"],
  "treatmentIds": ["STRT-MNOP"],
  "categoryFixedIds": ["SCF-QRST"],
  "species": [{ "speciesName": "Brown Rat", "displayOrder": 0 }],
  "products": [
    {
      "inventoryProductId": "PROD-X",
      "dilution": "1:100",
      "coverageSqft": 1000,
      "requiredQty": 5,
      "pricePerUom": 150,
      "manualEntry": false
    }
  ]
}
```

**Response**

- **Success Status**: `201 Created`
- **Response Structure**: `ResponseStructure<ServiceDefinitionResponse>`
- **Important Fields**: `id` (same as serviceCode), `totalChemicalCostPerVisit`, `totalChemicalCostPerMonth`.

_Success Response Example_

```json
{
  "status": "CREATED",
  "message": "Service created",
  "data": {
    "id": "SRV-5A2C",
    "serviceCode": "SRV-5A2C",
    "serviceName": "Commercial Rodent Control",
    "totalChemicalCostPerVisit": 750.0,
    "totalChemicalCostPerMonth": 750.0,
    "status": "ACTIVE"
  }
}
```

**cURL**

```bash
curl -X POST "http://baseUrl/api/v1/services" \
     -H "Authorization: Bearer <TOKEN>" \
     -H "Content-Type: application/json" \
     -d '{
           "serviceName": "Termite Barrier",
           "description": "Soil treatment for termites",
           "priceType": "AREA_BASED",
           "durationValue": 4,
           "durationUom": "HOURS",
           "status": "ACTIVE",
           "draft": false,
           "categoryIds": ["SCAT-T1"],
           "subCategoryIds": ["SSUB-T1"],
           "pestTypeIds": ["SPT-T1"],
           "treatmentIds": ["STRT-T1"],
           "products": [
             { "inventoryProductId": "P1", "requiredQty": 50, "pricePerUom": 10, "coverageSqft": 500 }
           ]
         }'
```

---

### [PUT] `/api/v1/services/update`

**Purpose**: Updates an existing service definition. Replaces all associations (categories, species, products) with the new list provided.

**Authorization**

- `Bearer token required`
- `hasRole('CEO')` or `SERVICE_MANAGEMENT_EDIT`

**Path Parameters**: Not applicable.
**Query Parameters**
| Field | Type | Required | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| `id` | `String` | Yes | The Service ID (SRV-XXXX). | "SRV-5A2C" |

**Request Body**: Same as POST request.

**Business Rules**:

- If updating status from ACTIVE to INACTIVE, `inactiveReason` is mandatory.
- The `serviceCode` cannot be changed.

---

### [GET] `/api/v1/services/by-id`

**Purpose**: Fetches the full configuration of a service. Used for "View" or "Edit" page loading.

**Query Parameters**
| Field | Type | Required | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| `id` | `String` | Yes | Service ID. | "SRV-5A2C" |

**Full Response JSON Example**

```json
{
  "status": "OK",
  "data": {
    "id": "SRV-5A2C",
    "serviceName": "Commercial Rodent Control",
    "priceType": "FIXED",
    "categories": [{ "id": "SCAT-T1", "name": "General" }],
    "products": [
      {
        "productName": "Rodent Bait",
        "pricePerUom": 150,
        "costPerVisit": 750,
        "estCostPerMonth": 750
      }
    ],
    "auditLogs": [
      {
        "changeType": "CREATE",
        "notes": "Service created",
        "changedBy": "Admin User"
      }
    ]
  }
}
```

---

### [GET] `/api/v1/services`

**Purpose**: Service Dashboard. Returns a paginated list of services with summary labels (e.g., "3 Mo (1R)" for warranty).

**Query Parameters**
| Field | Type | Required | Description | Example | Allowed Values |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `pageNo` | `int` | No | Page index (0-based). | 0 | |
| `pageSize` | `int` | No | Items per page. | 10 | |
| `categoryIds`| `List<String>`| No | Filter by categories. | `SCAT-1,SCAT-2` | |
| `priceTypes` | `List<Enum>` | No | Filter by price type. | `FIXED` | `FIXED`, `AREA_BASED`, `INSPECTION` |
| `statuses` | `List<Enum>` | No | Filter by status. | `ACTIVE` | `ACTIVE`, `INACTIVE` |
| `search` | `String` | No | Text search on name/code. | "rodent" | |

**Pagination structure**
The response contains `count`, `next`, `prev`, and `data` (List of `ServiceSummaryResponse`).

---

### [GET] `/api/v1/services/audit-logs`

**Purpose**: Fetches the history of changes for a specific service.

**Query Parameters**
| Field | Type | Required | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| `serviceId` | `String` | Yes | Service ID. | "SRV-5A2C" |

---

## Master Data APIs (Summary)

These APIs manage the dropdown options and pricing templates used during service creation.

| [METHOD] | [ENDPOINT]                              | Purpose                                      |
| :------- | :-------------------------------------- | :------------------------------------------- |
| `GET`    | `/api/v1/service-categories`            | Get IDs and Names for Category dropdown.     |
| `GET`    | `/api/v1/service-sub-categories`        | Get IDs and Names for Sub-Category dropdown. |
| `GET`    | `/api/v1/service-pest-types`            | Get IDs and Names for Pest Type dropdown.    |
| `GET`    | `/api/v1/service-treatments`            | Get IDs and Names for Treatment dropdown.    |
| `GET`    | `/api/v1/service-category-fixed`        | Get predefined fixed pricing tiers.          |
| `GET`    | `/api/v1/service-category-area`         | Get predefined area pricing rules.           |
| `GET`    | `/api/v1/service-category-inspection`   | Get predefined inspection fees.              |
| `GET`    | `/api/v1/service-custom-pricing-blocks` | Get complex pricing block templates.         |

---

## Exceptions / Error Cases

| HTTP Status | Reason              | When It Happens                                             | Typical Message                     | Frontend Handling Note           |
| :---------- | :------------------ | :---------------------------------------------------------- | :---------------------------------- | :------------------------------- |
| `400`       | Validation Error    | Mandatory field missing or invalid `id` for master data.    | "At least one category is required" | Highlight missing fields in red. |
| `400`       | Inactive Constraint | `inactiveReason` missing when setting status to `INACTIVE`. | "Inactive reason is required..."    | Prompt user for reason.          |
| `409`       | Duplicate Name      | A service with the same name already exists.                | "Service name already exists"       | Ask user to vary name.           |
| `404`       | Not Found           | Requested `id` does not exist.                              | "Service not found: SRV-123"        | Redirect to dashboard.           |

**Error Response JSON Example (Validation)**

```json
{
  "status": "BAD_REQUEST",
  "message": "One or more invalid category ids",
  "data": null
}
```

---

## Frontend Notes

1. **Draft Mode**: When `draft=true`, the backend relaxes "At least one..." constraints. Use this for "Save as Draft" functionality.
2. **Conditional Revisit**: Disable `freeRevisitQuantity` input if `freeRevisitIncluded` is `false`.
3. **Price Type Dependency**:
   - If `priceType` is `FIXED`, show/filter `categoryFixedIds`.
   - If `priceType` is `AREA_BASED`, show/filter `categoryAreaIds`.
   - If `priceType` is `INSPECTION`, show/filter `categoryInspectionIds`.
4. **Readonly Fields**: `serviceCode` is generated by backend; do not allow user to edit it during update.
5. **Pagination**: Use `count` for total pages calculation. Use `next`/`prev` URLs if building a simple pager.
6. **Cost Calculation**: `totalChemicalCostPerVisit` is estimated by backend using `pricePerUom` and `requiredQty`. Use this to show "Estimated Profitability" in the UI.

## Validation and Exception Summary

| Field / Scenario  | Validation / Rule                           | Error Type        | Frontend Impact                        |
| :---------------- | :------------------------------------------ | :---------------- | :------------------------------------- |
| `serviceName`     | Must be unique (trimmed, case-insensitive). | `409 Conflict`    | Check uniqueness on blur.              |
| `inactiveReason`  | Mandatory if `status` == `INACTIVE`.        | `400 Bad Request` | Required field validator.              |
| Master Data IDs   | Must exist in respective master tables.     | `400 Bad Request` | Ensure dropdowns use latest data.      |
| Inventory Product | Must exist in inventory module.             | `400 Bad Request` | Handle sync issues if product deleted. |

## Frontend Integration Notes

- **Dropdown Dependencies**: All `list-` APIs in `ServiceMasterController` provide the data for the multisets and single selects in the Service Form.
- **Status Rendering**: Use "success" badge for `ACTIVE` and "gray/danger" for `INACTIVE`.
- **Search Debounce**: Recommend 300ms debounce on the `search` query parameter in the dashboard.
- **Audit View**: Render `auditLogs` as a vertical timeline with the most recent item first.
