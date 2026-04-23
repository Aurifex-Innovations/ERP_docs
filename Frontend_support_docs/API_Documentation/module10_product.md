# Module 10 — Product Master (Combined Flow Doc)

This is the **combined, flow-wise, screen-wise** documentation for Module 10 Product Master.

- It explains **Base-only** vs **Variants-wise** product creation
- It explains **Edit flow** (single SKU row edit)
- It maps **each UI screen** to **exact API endpoints**
- It explains **complex keys** (like `groupKey`, `variantSku`, `mediaFiles.operation`)
- It lists **ENUM tokens** and accepted values
- It includes **ASCII flow diagrams**
- If any line feels “hard / confusing”, I explain it in **Gujarati + easy English**

Backend base path: **`/api/v1/inventory-products`**

---

## 1) Mental model (most important)

### 1.1 What is “Base product” and what is “Variant SKU” in this backend?

- **Backend stores stock per SKU row**, not per “parent product”.
- In DB table `inventory_products`, **each row = one SKU** (sellable/trackable unit).
- The ID used by stock modules is always **`inventory_products.id`** (in API it’s `id`).

### 1.2 When variants are used

When you create one logical product with multiple variants, backend generates:

- **1 base row** (variantName = `BASE`)
- **N variant rows** (variantName = your variant, like `100 ML`, `250 ML`)
- all these rows share same **`groupKey`** (UUID string)

Gujarati (hard part explained):

- **`groupKey`** એ “Parent product ID” નથી.  
  **`groupKey` ફક્ત grouping માટે છે** (UI માં variants સાથે show કરવા / total stock sum કરવા).  
  Stock માટે **હંમેશા `id`** (productId) જ ઉપયોગ કરવો.

---

## 2) Key fields (meaning in simple language)

### 2.1 Product identifiers

- **`id`**: backend-generated primary key (example: `PRD-A9F3C`)
  - **Use this in stock/add-stock/stock-request lines**
- **`productCode`**: business code shown to users (must be unique per row)
- **`groupKey`**: groups base + variants created together (string UUID)

### 2.2 Variant fields (per SKU row)

- **`variantName`**: label of the SKU (e.g. `100 ML`, `1 LTR`, `DEFAULT`, `BASE`)
- **`variantSku`**: optional extra SKU code (also must be unique if provided)
- **`variantQuantity`**: numeric quantity of the variant (required)
- **`variantPackageType`**: package type for the variant (required)
- **`variantStatus`**: `ACTIVE` / `INACTIVE` for the variant

Gujarati:

- **`variantQuantity`** નો અર્થ “આ variant કેટલો size છે” (e.g. 100ml = `100`).  
  UOM (`baseUom`) root પર આવે છે (ML/LTR/KG...), તેથી size = quantity + uom.

### 2.3 Packaging (two levels)

There are **two** packaging concepts:

1. **Variant package fields** (required at variant-level):

- `variantPackageType`
- `variantQuantity`

2. **Product packaging fields** (optional at root, and can be overridden per variant):

- `packageType`
- `quantityPerPackage`
- `unitsPerPackage`

**Override rule (important):**

- In variants-wise create, for each variant line:
  - `packageType` / `quantityPerPackage` / `unitsPerPackage` are optional
  - If not provided, backend uses root values

Gujarati:

- જો **બધી variants** same packaging માં આવે છે (e.g. 12 bottles per box), તો root પર આપો.  
  જો variant પ્રમાણે packaging બદલાય (100ml vs 250ml box count), તો variant line માં override આપો.

### 2.4 Media files (`mediaFiles[]`)

Product images are sent in `mediaFiles[]` and backend supports operations:

Enum `DocumentOperation`:

- `KEEP`
- `ADD`
- `REPLACE`
- `DELETE`

Constraints (validated in service):

- **Max 5 images**
- **Max 2MB per image**
- Types allowed: `image/jpeg`, `image/jpg`, `image/png`

Important backend behavior:

- `KEEP` → does nothing (keeps as-is)
- `DELETE` → deletes existing media record (also deletes from storage)
- `REPLACE` → deletes old media (if `productId` set) then **falls through** into `ADD` and uploads new file
- `ADD` → uploads and creates new media record

Hard line clarification (Gujarati + English):

- In `ProductMediaDto`, field name **`productId` actually holds the MEDIA ID** when doing `DELETE/REPLACE`.  
  Gujarati: `productId` નામ misleading છે—DELETE/REPLACE માટે તેમાં media record નું `id` જ જાય છે.

Payload note:

- `fileData` can be full data-url (`data:image/png;base64,...`) or raw base64; backend strips prefix if present.

---

## 3) ENUMs (exact tokens to send)

Backend expects **UPPERCASE tokens** in requests/query params.

### 3.1 Category (`category`)

- `CHEMICAL`
- `SPRAYER`
- `ELECTRIC_PUMP`
- `MACHINE`
- `TRAP`
- `TOOL`
- `OTHER`

### 3.2 SubType (`subType`)

Currently same list as Category:

- `CHEMICAL`, `SPRAYER`, `ELECTRIC_PUMP`, `MACHINE`, `TRAP`, `TOOL`, `OTHER`

Gujarati:

- UI માં cascading dropdown બતાવશો તો પણ backend માં actual mapping નથી; currently same enum list છે.

### 3.3 Status (`status`, `variantStatus`)

- `ACTIVE`
- `INACTIVE`

### 3.4 Base UOM (`baseUom`)

- `LTR`
- `KG`
- `GRAM`
- `ML`
- `SET`
- `PKT`

### 3.5 Secondary UOM (`secondaryUom`)

- `LTR`
- `KG`
- `GRAM`
- `ML`
- `SET`
- `PKT`

### 3.6 Package Type (`packageType`, `variantPackageType`)

- `BOTTLE`
- `PACKET`
- `POUCH`
- `BOX`
- `BAG`
- `CAN`
- `SET`

---

## 4) Screen-wise flow + exact APIs

### 4.1 Screen: Product List / Table (10.1)

**Goal**: list rows, filter, search, paginate.

**API**

- `GET /api/v1/inventory-products`

**Common query params**

- `pageNo` (default `0`)
- `pageSize` (default `10`)
- `categories=CHEMICAL&categories=SPRAYER` (multi)
- `subTypes=CHEMICAL&subTypes=OTHER` (multi)
- `statuses=ACTIVE&statuses=INACTIVE` (multi)
- `brandIds=<brandId>&brandIds=<brandId>` (multi)
- `hsnCode=1234`
- `packageType=BOTTLE`
- `search=PRD001`
- `fromDate=2026-01-01` (ISO date)
- `toDate=2026-01-31`

Gujarati:

- Filter માં `brandIds` માં **brand master નું id** જ મોકલવું (name નહિ).

---

### 4.2 Screen: Add Product (10.2) — choose mode

UI typically supports two modes:

- **Mode A: Base-only (single SKU)**
- **Mode B: Variants-wise (multiple SKUs + grouping)**

#### Recommended single endpoint for UI

Use unified endpoint always:

- **`POST /api/v1/inventory-products/upsert`**

Backend decides:

- if `variants[]` is **present and non-empty** → behaves like variant-bulk
- else → creates single SKU

##### ASCII decision flow

```
User clicks "Save"
        |
        v
POST /inventory-products/upsert
        |
        +-- variants[] present & not empty? --- YES ---> createVariantBulk()
        |                                         |
        |                                         +-- creates BASE row + N variant rows
        |                                         +-- returns { groupKey, items[] }
        |
        '----------------------------- NO -----> create(single)
                                                  |
                                                  +-- returns InventoryProductResponse
```

---

### 4.3 Screen: Add Product (Base-only) — what to send

**API**

- `POST /api/v1/inventory-products/upsert`

**Rule**

- do **NOT** send `variants[]`
- do send single-variant fields (`variantName`, `variantQuantity`, `variantPackageType`, etc.)

Example request (Base-only):

```json
{
  "productName": "P2",
  "productCode": "P2",
  "category": "CHEMICAL",
  "subType": "CHEMICAL",
  "brandId": "br-123",
  "description": "example",
  "status": "ACTIVE",
  "hsnCode": "3808",
  "baseUom": "ML",
  "unitPackagingBrand": "Company Printed",
  "secondaryUom": "ML",
  "packageType": "BOTTLE",
  "quantityPerPackage": 1,
  "unitsPerPackage": 12,

  "variantName": "DEFAULT",
  "variantSku": "P2-DEFAULT",
  "variantQuantity": 250,
  "variantPackageType": "BOTTLE",
  "barcode": "8901234567890",
  "variantStatus": "ACTIVE",

  "purchasePrice": 100,
  "sellingPrice": 120,
  "basePrice": 100,
  "taxAmount": 18,
  "totalCost": 118,

  "mediaFiles": [
    {
      "fileName": "p2.png",
      "contentType": "image/png",
      "fileData": "data:image/png;base64,iVBORw0KGgoAAA...",
      "isPrimary": true,
      "operation": "ADD"
    }
  ]
}
```

Important notes:

- `InventoryProductRequest` (single create) requires many fields as `@NotNull/@NotBlank`. Upsert fills some defaults but you should still send complete data.
- If you don’t send `variantName`, service defaults it to `DEFAULT`.

Gujarati:

- Base-only mode માં પણ backend internally “variant fields” mandatory રાખે છે, એટલે UI માં “variant” section hide કરશો તો પણ defaults fill કરવાના રહેશે.

---

### 4.4 Screen: Add Product (Variants-wise) — what to send

**API options**

- Preferred UI: `POST /api/v1/inventory-products/upsert`
- Direct bulk: `POST /api/v1/inventory-products/variant-bulk`

**Most important rules (validated)**

- Root `productCode` is required when variants exist (base code)
- Each variant line must have:
  - its own `productCode` (unique, cannot equal base code)
  - `variantName` (required)
  - `variantQuantity` (required)
  - `variantPackageType` (required)
  - `purchasePrice` (required)

##### ASCII creation result

```
Bulk save (P1 with variants V1,V2)
           |
           v
DB rows created:
  1) BASE row: productCode=P1, variantName=BASE, groupKey=G
  2) V1 row  : productCode=P1-V1, variantName=100 ML, groupKey=G
  3) V2 row  : productCode=P1-V2, variantName=250 ML, groupKey=G
```

Gujarati (hard part explained):

- Backend **BASE row પણ create કરે છે**. એટલે list/dropdown માં `P1(BASE)` જેવી entry પણ આવશે.  
  જો UI માં base row show ન કરવો હોય તો frontend filtering rule બનાવવો (example: hide `variantName == "BASE"`).

Example request (Variants-wise, using `upsert`):

```json
{
  "productName": "P1",
  "productCode": "P1",
  "category": "CHEMICAL",
  "subType": "CHEMICAL",
  "brandId": "br-123",
  "description": "example",
  "status": "ACTIVE",
  "hsnCode": "3808",
  "baseUom": "ML",
  "unitPackagingBrand": "Company Printed",
  "secondaryUom": "ML",
  "packageType": "BOTTLE",
  "quantityPerPackage": 1,
  "unitsPerPackage": 12,
  "mediaFiles": [],
  "variants": [
    {
      "productCode": "P1-100",
      "variantName": "100 ML",
      "variantSku": "SKU-P1-100",
      "variantQuantity": 100,
      "variantPackageType": "BOTTLE",
      "barcode": "111",
      "variantStatus": "ACTIVE",
      "purchasePrice": 50,
      "sellingPrice": 60,
      "basePrice": 50,
      "taxAmount": 9,
      "totalCost": 59
    },
    {
      "productCode": "P1-250",
      "variantName": "250 ML",
      "variantSku": "SKU-P1-250",
      "variantQuantity": 250,
      "variantPackageType": "BOTTLE",
      "barcode": "222",
      "variantStatus": "ACTIVE",
      "purchasePrice": 90,
      "sellingPrice": 110,
      "basePrice": 90,
      "taxAmount": 16.2,
      "totalCost": 106.2,

      "packageType": "BOX",
      "quantityPerPackage": 1,
      "unitsPerPackage": 24
    }
  ]
}
```

Response (high-level):

- returns `InventoryProductVariantBulkCreateResponse`:
  - `groupKey`: the grouping key
  - `items[]`: base row + variant rows (each has its own `id`)

---

### 4.5 Screen: Edit Product (10.3)

Important: backend update is **per SKU row** (`id`).

#### Load (for edit form)

- `GET /api/v1/inventory-products/by-id?id=<productId>`

#### Save (single SKU update)

- `PUT /api/v1/inventory-products/update?id=<productId>`
- Body: `InventoryProductRequest`

Gujarati (important):

- Variants-wise “Edit all variants together” backend API નથી.  
  UI માં group edit કરવો હોય તો:
  - first get groupKey from any SKU
  - then update each SKU separately (multiple PUT calls)

Media edit:

- send `mediaFiles[]` with operations (`ADD/DELETE/REPLACE/KEEP`)
- backend does **not** automatically wipe old media unless you DELETE/REPLACE them

---

### 4.6 Screen: View Product (10.4)

- `GET /api/v1/inventory-products/by-id?id=<productId>`

If you want “view all variants + total stock” for the same product group:

- `GET /api/v1/inventory-products/group-summary?groupKey=<groupKey>`
- Optional: `&branchId=<branchId>` to show one-branch stock only

---

### 4.7 Screen: Delete Product (10.5) (soft delete)

- `DELETE /api/v1/inventory-products/delete?id=<productId>`

Backend behavior:

- sets `status = INACTIVE`
- sets `deletedAt`, `deletedBy`
- stock ledger remains

Gujarati:

- Delete કર્યા પછી dropdown માં દેખાવું બંધ થશે કારણ કે dropdown only `status=ACTIVE` AND `variantStatus=ACTIVE` rows આપે છે.

---

## 5) Supporting dropdown APIs (screen dependencies)

### 5.1 Brand dropdown (Company/Brand search)

Not under `/inventory-products`:

- `GET /api/v1/inventory/brands` → `[{ id, name }]`
- Create brand: `POST /api/v1/inventory/brands` body `{ "name": "ABC Agro" }`

### 5.2 Product dropdown (used by Stock module etc.)

- `GET /api/v1/inventory-products/dropdown`

Important output behavior:

- returns one option per SKU row (`id`)
- only ACTIVE + variantStatus ACTIVE
- `productName` is formatted as:  
  `productName + "(" + variantName + ")"`  
  Example: `P1 (100 ML)` might appear as `P1 (100 ML)(100 ML)` if base name already includes variant text.  
  Gujarati: dropdown formatting currently appends variant in string; UI should prefer showing separate fields if you want clean labels.

### 5.3 HSN dropdown (Module 9 dependency)

- `GET /api/v1/tax/hsn-codes/dropdown`
- optional auto-fill: `GET /api/v1/tax/hsn-codes/products?id=<hsnId>`

---

## 6) Quick “Which API for which screen” summary

- **List/table**: `GET /api/v1/inventory-products`
- **Add (base-only or variants)**: `POST /api/v1/inventory-products/upsert`
- **Add variants direct**: `POST /api/v1/inventory-products/variant-bulk`
- **View/edit load**: `GET /api/v1/inventory-products/by-id?id=...`
- **Edit save**: `PUT /api/v1/inventory-products/update?id=...`
- **Delete**: `DELETE /api/v1/inventory-products/delete?id=...`
- **Dropdown**: `GET /api/v1/inventory-products/dropdown`
- **Group totals + variants**: `GET /api/v1/inventory-products/group-summary?groupKey=...(&branchId=...)`
