# Product Management API Documentation

## Short Description

The Product Management module (Module 10) allows frontend developers to manage the inventory of products, including their core details, variants, pricing, and media assets. It handles product creation, updates, deletions, and advanced filtering for list views.

## Authorization

- **Authentication Type:** JWT / Bearer Token
- **Required Token:** `Authorization: Bearer <token>`
- **Required Roles:** `CEO`
- **Required Authorities:**
  - `PRODUCT_MANAGEMENT_ADD` (for Create)
  - `PRODUCT_MANAGEMENT_EDIT` (for Update)
  - `PRODUCT_MANAGEMENT_READ` (for Get/Filter)
  - `PRODUCT_MANAGEMENT_DELETE` (for Delete)
- **Required Headers:**
  - `Authorization`: Bearer token
  - `X-Tenant-ID`: (Inferred as standard for this application architecture)
- **Access Restrictions:** Access is restricted based on the presence of the `CEO` role or the specific authorities mentioned above.

## Enums Used In This Module

### Category

Used in: Create/Update Request, Get/Filter Response
| Value | Meaning |
| :--- | :--- |
| CHEMICAL | Chemical products |
| SPRAYER | Spraying equipment |
| ELECTRIC_PUMP | Electric pumping units |
| MACHINE | General machinery |
| TRAP | Pest control traps |
| TOOL | Hardware tools |
| OTHER | Miscellaneous products |

### SubType

Used in: Create/Update Request, Get Response
| Value | Meaning |
| :--- | :--- |
| CHEMICAL | Chemical products |
| SPRAYER | Spraying equipment |
| ELECTRIC_PUMP | Electric pumping units |
| MACHINE | General machinery |
| TRAP | Pest control traps |
| TOOL | Hardware tools |
| OTHER | Miscellaneous products |

### Brand

Used in: Create/Update Request, Get/Filter Response
| Value | Meaning |
| :--- | :--- |
| PESTO | Pesto Brand products |

### Status

Used in: Create/Update Request, Get/Filter Response
| Value | Meaning |
| :--- | :--- |
| ACTIVE | Product is available and active |
| INACTIVE | Product is deactivated (Soft deleted) |

### BaseUOM / SecondaryUOM

Used in: Create/Update Request, Get Response
| Value | Meaning |
| :--- | :--- |
| LTR | Liters |
| KG | Kilograms |
| GRAM | Grams |
| ML | Milliliters |
| SET | Set |
| PKT | Packets |

### PackageType

Used in: Create/Update Request, Get Response
| Value | Meaning |
| :--- | :--- |
| BOTTLE | Bottle |
| PACKET | Packet |
| POUCH | Pouch |
| BOX | Box |
| BAG | Bag |
| CAN | Can |
| SET | Set |

### DocumentOperation

Used in: Create/Update Request (ProductMediaDto)
| Value | Meaning |
| :--- | :--- |
| KEEP | No change to existing media |
| ADD | Add new media |
| REPLACE | Replace existing media |
| DELETE | Delete existing media |

## API List

| Method | Endpoint                            | Purpose                                  | Authorization Required           |
| :----- | :---------------------------------- | :--------------------------------------- | :------------------------------- |
| POST   | `/api/v1/inventory-products`        | Create a new inventory product           | CEO or PRODUCT_MANAGEMENT_ADD    |
| PUT    | `/api/v1/inventory-products/update` | Update an existing product               | CEO or PRODUCT_MANAGEMENT_EDIT   |
| GET    | `/api/v1/inventory-products/by-id`  | Fetch product details by ID              | CEO or PRODUCT_MANAGEMENT_READ   |
| DELETE | `/api/v1/inventory-products/delete` | Soft delete a product                    | CEO or PRODUCT_MANAGEMENT_DELETE |
| GET    | `/api/v1/inventory-products`        | List and filter products with pagination | CEO or PRODUCT_MANAGEMENT_READ   |

## API Details

### POST `/api/v1/inventory-products`

**Purpose**
Creates a new inventory product with associated variant details and optional media files.

**Authorization**

- **Token:** Required
- **Role/Authority:** `CEO` or `PRODUCT_MANAGEMENT_ADD`
- **Headers:** `Authorization`, `X-Tenant-ID`

**Request Body Fields**
| Field | Type | Required | Validation | Description | Example | Allowed Values |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| productName | String | Yes | @NotBlank | Name of the product | "Premium Sprayer 5L" | |
| productCode | String | No | Unique | Unique identifier code | "PRD-001" | |
| category | Enum | Yes | @NotNull | Product category | "SPRAYER" | [See Category Enum] |
| subType | Enum | Yes | @NotNull | Product sub-type | "SPRAYER" | [See SubType Enum] |
| brand | Enum | Yes | @NotNull | Primary brand | "PESTO" | [See Brand Enum] |
| description | String | No | | Detailed description | "Heavy duty sprayer" | |
| status | Enum | Yes | @NotNull | Initial status | "ACTIVE" | [See Status Enum] |
| hsnCode | String | Yes | @NotBlank | HSN code for taxation | "8424" | |
| baseUom | Enum | Yes | @NotNull | Base unit of measure | "ML" | [See BaseUOM Enum] |
| unitPackagingBrand | Enum | Yes | @NotNull | Packaging brand | "PESTO" | [See Brand Enum] |
| secondaryUom | Enum | Yes | @NotNull | Secondary unit of measure | "LTR" | [See SecondaryUOM Enum] |
| packageType | Enum | Yes | @NotNull | Type of packaging | "BOTTLE" | [See PackageType Enum] |
| quantityPerPackage| Double | Yes | @NotNull | Quantity in each package | 500.0 | |
| unitsPerPackage | Double | No | | Number of units in package | 1.0 | |
| variantName | String | Yes | @NotBlank | Name of the variant | "Standard Edition" | |
| variantSku | String | No | Unique | Unique SKU for variant | "SKU-991" | |
| variantQuantity | Double | Yes | @NotNull | Quantity for this variant | 100.0 | |
| variantPackageType| Enum | Yes | @NotNull | Package type for variant | "BOTTLE" | [See PackageType Enum] |
| barcode | String | No | | Barcode string | "123456789" | |
| variantStatus | Enum | Yes | @NotNull | Status of the variant | "ACTIVE" | [See Status Enum] |
| purchasePrice | Double | Yes | @NotNull | Cost of purchase | 1200.0 | |
| sellingPrice | Double | No | | Recommended selling price | 1500.0 | |
| basePrice | Double | No | | Price before tax | 1000.0 | |
| taxAmount | Double | No | | Applied tax amount | 200.0 | |
| totalCost | Double | No | | Total landing cost | 1400.0 | |
| mediaFiles | List | No | | List of media objects | | |
| mediaFiles[].fileName | String | Yes* | | Name of file | "image1.jpg" | |
| mediaFiles[].contentType | String | Yes* | | MIME type | "image/jpeg"| |
| mediaFiles[].fileData | String | Yes* | Base64 | Image data if adding | "data:image/jpeg;base64,..." | |
| mediaFiles[].isPrimary| Boolean| No | | Set as thumbnail | true | |
| mediaFiles[].operation| Enum | Yes* | | Media action | "ADD" | [See DocumentOperation] |

**Full Request JSON Examples**

**Minimal Valid Request**

```json
{
  "productName": "Basic Insecticide",
  "category": "CHEMICAL",
  "subType": "CHEMICAL",
  "brand": "PESTO",
  "status": "ACTIVE",
  "hsnCode": "3808",
  "baseUom": "ML",
  "unitPackagingBrand": "PESTO",
  "secondaryUom": "LTR",
  "packageType": "BOTTLE",
  "quantityPerPackage": 1000.0,
  "variantName": "1L Bottle",
  "variantQuantity": 50.0,
  "variantPackageType": "BOTTLE",
  "variantStatus": "ACTIVE",
  "purchasePrice": 450.0
}
```

**Complete Valid Request (With Media)**

```json
{
  "productName": "Advanced Sprayer X1",
  "productCode": "SPR-X1-001",
  "category": "SPRAYER",
  "subType": "SPRAYER",
  "brand": "PESTO",
  "description": "High pressure electric sprayer for farm use.",
  "status": "ACTIVE",
  "hsnCode": "8424",
  "baseUom": "SET",
  "unitPackagingBrand": "PESTO",
  "secondaryUom": "SET",
  "packageType": "BOX",
  "quantityPerPackage": 1.0,
  "unitsPerPackage": 1.0,
  "variantName": "Electric Blue",
  "variantSku": "SKU-SPR-B1",
  "variantQuantity": 10.0,
  "variantPackageType": "BOX",
  "barcode": "890123456789",
  "variantStatus": "ACTIVE",
  "purchasePrice": 2500.0,
  "sellingPrice": 3200.0,
  "basePrice": 2100.0,
  "taxAmount": 400.0,
  "totalCost": 2500.0,
  "mediaFiles": [
    {
      "fileName": "sprayer_front.jpg",
      "contentType": "image/jpeg",
      "fileData": "base64_encoded_string_here",
      "isPrimary": true,
      "operation": "ADD"
    }
  ]
}
```

**Response**

- **Success Status Code:** `201 Created`
- **Response Structure:**

```json
{
  "status": 201,
  "message": "Product Created Successfully",
  "data": {
    "id": "PRD-A1234",
    "productName": "Advanced Sprayer X1",
    "productCode": "SPR-X1-001",
    "category": "SPRAYER",
    "brand": "PESTO",
    "baseUom": "SET",
    "packageType": "BOX",
    "hsnCode": "8424",
    "totalCost": 2500.0,
    "status": "ACTIVE",
    "createdBy": "Admin User",
    "createdAt": "2023-10-27T10:00:00Z",
    "imageUrls": ["https://presigned-url-to-image.com/..."]
  }
}
```

---

### PUT `/api/v1/inventory-products/update`

**Purpose**
Updates product details, variants, or manages media files.

**Authorization**

- **Token:** Required
- **Role/Authority:** `CEO` or `PRODUCT_MANAGEMENT_EDIT`
- **Headers:** `Authorization`, `X-Tenant-ID`

**Path Parameters**
Not applicable.

**Query Parameters**
| Field | Type | Required | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| id | String | Yes | ID of the product to update | "PRD-A1234" |

**Request Body Fields**
Same as Create Request. Additionally, for `mediaFiles`:

- `operation: DELETE` requires `productId` (which maps to media ID in service implementation).
- `operation: REPLACE` requires `productId` (id of media being replaced) and new `fileData`.
- `operation: KEEP` implies no change to that media item.

**Update Request Example (Change Price and Add Media)**

```json
{
  "productName": "Advanced Sprayer X1 (Updated)",
  "category": "SPRAYER",
  "subType": "SPRAYER",
  "brand": "PESTO",
  "status": "ACTIVE",
  "hsnCode": "8424",
  "baseUom": "SET",
  "unitPackagingBrand": "PESTO",
  "secondaryUom": "SET",
  "packageType": "BOX",
  "quantityPerPackage": 1.0,
  "variantName": "Electric Blue",
  "variantQuantity": 15.0,
  "variantPackageType": "BOX",
  "variantStatus": "ACTIVE",
  "purchasePrice": 2600.0,
  "mediaFiles": [
    {
      "productId": "MEDIA-ID-123",
      "operation": "DELETE"
    },
    {
      "fileName": "new_angle.jpg",
      "contentType": "image/jpeg",
      "fileData": "base64_data",
      "operation": "ADD",
      "isPrimary": false
    }
  ]
}
```

**Response**

- **Success Status Code:** `200 OK`
- **Response Structure:** Same as Create Response (InventoryProductResponse).

---

### GET `/api/v1/inventory-products/by-id`

**Purpose**
Fetches comprehensive details of a specific product including all variants, pricing, and full media details.

**Authorization**

- **Token:** Required
- **Role/Authority:** `CEO` or `PRODUCT_MANAGEMENT_READ`

**Query Parameters**
| Field | Type | Required | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| id | String | Yes | ID of the product | "PRD-A1234" |

**Full Response JSON Example**

```json
{
  "status": 200,
  "message": "Product Fetched",
  "data": {
    "id": "PRD-A1234",
    "productName": "Advanced Sprayer X1",
    "productCode": "SPR-X1-001",
    "category": "SPRAYER",
    "subType": "SPRAYER",
    "brand": "PESTO",
    "description": "High pressure electric sprayer for farm use.",
    "status": "ACTIVE",
    "hsnCode": "8424",
    "baseUom": "SET",
    "unitPackagingBrand": "PESTO",
    "secondaryUom": "SET",
    "packageType": "BOX",
    "quantityPerPackage": 1.0,
    "unitsPerPackage": 1.0,
    "variantName": "Electric Blue",
    "variantSku": "SKU-SPR-B1",
    "variantPackageType": "BOX",
    "variantQuantity": 15.0,
    "barcode": "890123456789",
    "variantStatus": "ACTIVE",
    "purchasePrice": 2600.0,
    "sellingPrice": 3200.0,
    "basePrice": 2100.0,
    "taxAmount": 400.0,
    "totalCost": 2600.0,
    "mediaFiles": [
      {
        "id": "MEDIA-001",
        "productId": "PRD-A1234",
        "fileName": "sprayer_front.jpg",
        "contentType": "image/jpeg",
        "fileUrl": "https://presigned-url.com/...",
        "isPrimary": true,
        "createdBy": "Admin",
        "createdAt": "2023-10-27T10:00:00Z"
      }
    ],
    "createdBy": "Admin",
    "createdAt": "2023-10-27T10:00:00Z",
    "updatedBy": "Admin",
    "updatedAt": "2023-10-28T10:00:00Z"
  }
}
```

---

### DELETE `/api/v1/inventory-products/delete`

**Purpose**
Performs a soft delete by setting the product status to `INACTIVE`.

**Authorization**

- **Token:** Required
- **Role/Authority:** `CEO` or `PRODUCT_MANAGEMENT_DELETE`

**Query Parameters**
| Field | Type | Required | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| id | String | Yes | ID of the product to delete | "PRD-A1234" |

**Response**

- **Success Status Code:** `200 OK`
- **Response Structure:** InventoryProductResponse with status `INACTIVE`.

---

### GET `/api/v1/inventory-products`

**Purpose**
Retrieves a paginated and filtered list of products.

**Authorization**

- **Token:** Required
- **Role/Authority:** `CEO` or `PRODUCT_MANAGEMENT_READ`

**Query Parameters**
| Field | Type | Required | Description | Example | Allowed Values |
| :--- | :--- | :--- | :--- | :--- | :--- |
| pageNo | Integer| No | Page number (0-indexed)| 0 | |
| pageSize| Integer| No | Items per page (default 10)| 10 | |
| categories| List | No | Filter by categories | "SPRAYER,CHEMICAL"| [See Category Enum] |
| statuses| List | No | Filter by statuses | "ACTIVE" | [See Status Enum] |
| packageType| Enum | No | Filter by package type | "BOX" | [See PackageType Enum] |
| search | String | No | Search in name/code/variant | "blue sprayer" | |
| fromDate| Date | No | Filter by creation date | "2023-10-01" | YYYY-MM-DD |
| toDate | Date | No | Filter by creation date | "2023-10-31" | YYYY-MM-DD |

**Full Response JSON Examples**

**Paginated List Response**

```json
{
  "status": 200,
  "message": "Products fetched successfully",
  "data": {
    "count": 45,
    "next": "http://api.base/api/v1/inventory-products?pageSize=10&pageNo=1",
    "prev": null,
    "data": [
      {
        "id": "PRD-A1234",
        "productName": "Advanced Sprayer X1",
        "productCode": "SPR-X1-001",
        "category": "SPRAYER",
        "brand": "PESTO",
        "baseUom": "SET",
        "packageType": "BOX",
        "hsnCode": "8424",
        "totalCost": 2600.0,
        "status": "ACTIVE",
        "createdBy": "Admin",
        "createdAt": "2023-10-27T10:00:00Z",
        "imageUrls": ["https://presigned-url.com/..."]
      }
    ]
  }
}
```

**Empty State Response**

```json
{
  "status": 200,
  "message": "Products fetched successfully",
  "data": {
    "count": 0,
    "next": null,
    "prev": null,
    "data": []
  }
}
```

## Exceptions / Error Cases

| HTTP Status | Reason       | When It Happens                                 | Typical Message                    | Frontend Handling Note                           |
| :---------- | :----------- | :---------------------------------------------- | :--------------------------------- | :----------------------------------------------- |
| 409         | Conflict     | Product Code or Variant SKU already exists      | "Product code already exists"      | Prompt user to change the code/SKU               |
| 404         | Not Found    | Requested product ID or media ID does not exist | "Product not found"                | Redirect to listing page or show "Missing" state |
| 400         | Bad Request  | File too large                                  | "File too large (Max 5MB)"         | Validate file size before sending base64         |
| 400         | Bad Request  | Invalid file type                               | "Invalid file type"                | Restrict file input to .jpg, .jpeg, .png         |
| 400         | Bad Request  | Missing file data for ADD                       | "File data required"               | Ensure base64 string is generated correctly      |
| 400         | Bad Request  | Media ownership mismatch                        | "Media does not belong to product" | Should not happen if UI is consistent            |
| 403         | Forbidden    | Insufficient Permission                         | "Access Denied"                    | Show "Permission Required" overlay               |
| 401         | Unauthorized | Missing/Expired Token                           | "Full authentication is required"  | Redirect to login page                           |

**Error Response JSON Examples**

**Validation Error (Conflict)**

```json
{
  "status": 409,
  "message": "Product code already exists",
  "data": null
}
```

**Bad Request (File Size)**

```json
{
  "status": 400,
  "message": "File too large (Max 5MB)",
  "data": null
}
```

## Frontend Notes

- **Unique Fields:** `productCode` and `variantSku` are validated for uniqueness on the server.
- **Media Upload:** Files must be sent as Base64 strings within the `mediaFiles` array. Maximum size is 5MB per file. Only JPG, JPEG, and PNG are allowed.
- **Soft Delete:** The DELETE API doesn't wipe data; it flips `status` to `INACTIVE`.
- **Pagination:** Zero-based `pageNo`. Use `count` for UI pagination controls.
- **Date Search:** Use `YYYY-MM-DD` format for `fromDate` and `toDate`.
- **Dropdowns:** Use the Enums provided to populate dropdowns for Category, Brand, UOM, and Package Type.
- **Image URLs:** The API returns temporary S3 presigned URLs (valid for 60 seconds). Frontend should not cache these URLs long-term.

## Validation and Exception Summary

| Field / Scenario | Validation / Rule | Error Type      | Frontend Impact                 |
| :--------------- | :---------------- | :-------------- | :------------------------------ |
| Product Code     | Must be unique    | 409 Conflict    | Block submit; ask for new code  |
| Variant SKU      | Must be unique    | 409 Conflict    | Block submit; ask for new SKU   |
| mediaFiles       | Max 5MB per file  | 400 Bad Request | Show client-side toast for size |
| mediaFiles       | JPG/PNG only      | 400 Bad Request | Restrict file picker extensions |
| productName      | Required          | 400 Bad Request | Mark field as required          |
| baseUom          | Required          | 400 Bad Request | Dropdown mandatory selection    |

## Frontend Integration Notes

- **Create vs Update:** Use the same form for both. Ensure the `id` is passed as a query param for the Update PUT request.
- **Thumbnail:** The first image in the `imageUrls` list is either the primary image or the first available image.
- **Primary Image:** When adding media, setting `isPrimary: true` will mark it as the default display image.

---

**cURL Example (Create Product)**

```bash
curl --location 'http://localhost:8080/api/v1/inventory-products' \
--header 'Authorization: Bearer <TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
    "productName": "Alpha Sprayer",
    "category": "SPRAYER",
    "subType": "SPRAYER",
    "brand": "PESTO",
    "status": "ACTIVE",
    "hsnCode": "8424",
    "baseUom": "ML",
    "unitPackagingBrand": "PESTO",
    "secondaryUom": "LTR",
    "packageType": "BOTTLE",
    "quantityPerPackage": 500,
    "variantName": "v1",
    "variantQuantity": 10,
    "variantPackageType": "BOTTLE",
    "variantStatus": "ACTIVE",
    "purchasePrice": 1000
}'
```

**cURL Example (Filter Products)**

```bash
curl --location 'http://localhost:8080/api/v1/inventory-products?pageNo=0&pageSize=10&categories=SPRAYER&search=Alpha' \
--header 'Authorization: Bearer <TOKEN>'
```
