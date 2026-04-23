# Module 10 (10.1–10.5) — Product Master APIs (Beginner-friendly frontend guide)

This guide maps each **Module 10 Product Master screen** to the **exact backend API endpoints** and lists **what values each dropdown/enum accepts**.

Backend base path: **`/api/v1/inventory-products`**

## Authentication and headers

- **Auth**: Send JWT Bearer token.
- **Tenant (if enabled in your environment)**: send header `X-Tenant-ID: <tenantSchemaOrTenantId>`.
- **Content-Type**: `application/json` for all JSON requests.

---

## Dropdown / Enum values (what the API accepts)

Important: backend expects these exact **enum tokens** (uppercase). The UI can display friendly labels, but must send these values in API payload/query params.

### Category (`category`)

Accepted values (from `Category` enum):

- `CHEMICAL`
- `SPRAYER`
- `ELECTRIC_PUMP`
- `MACHINE`
- `TRAP`
- `TOOL`
- `OTHER`

### Sub-Type (`subType`)

Accepted values (from `SubType` enum):

- `CHEMICAL`
- `SPRAYER`
- `ELECTRIC_PUMP`
- `MACHINE`
- `TRAP`
- `TOOL`
- `OTHER`

Note: In the current backend, `subType` is **not truly cascading**; it’s the same enum list as `category`.

### Company / Brand (`brandId`)

This is now **dynamic** (not an enum). Products store `brandId` which is a FK to `inventory_brands.id`.

#### Brand dropdown API

- `GET /api/v1/inventory/brands` → returns `[{ id, name }]`

#### Create brand API (Add New Brand)

- `POST /api/v1/inventory/brands` with body `{ "name": "ABC Agro" }`

#### What to store in product payload

For product create/update, send `brandId` as the **brand primary key** (example: `"brandId": "br-..."`).

### Status (`status`, `variantStatus`)

Accepted values:

- `ACTIVE`
- `INACTIVE`

### Base UOM (`baseUom`)

Accepted values:

- `LTR`
- `KG`
- `GRAM`
- `ML`
- `SET`
- `PKT`

### Secondary UOM (`secondaryUom`)

Accepted values:

- `LTR`
- `KG`
- `GRAM`
- `ML`
- `SET`
- `PKT`

### Package Type (`packageType`, `variantPackageType`)

Accepted values:

- `BOTTLE`
- `PACKET`
- `POUCH`
- `BOX`
- `BAG`
- `CAN`
- `SET`

---

## Module 9 dependency (HSN dropdown + tax mapping)

Module 10 requires Module 9 for HSN selection.

- **HSN dropdown** (for Section 5 HSN Code search dropdown):  
  `GET /api/v1/tax/hsn-codes/dropdown`

- **HSN tax mapping** (if UI needs to auto-fill tax rates when an HSN is selected):  
  `GET /api/v1/tax/hsn-codes/products?id=<hsnId>`

Note: Product APIs currently store `hsnCode` as a string, but the auto-fill of tax rates should be handled by the frontend using Module 9 APIs.

---

## 10.1 Product Management — Table View (list + filters)

### Screen needs

- Table rows
- Filters: Category (multi), Sub-Type, Brand, HSN Code, Status (multi), Created date range
- Search: code/name/brand/HSN
- Pagination

### API to call

`GET /api/v1/inventory-products`

### Query params (all optional unless mentioned)

- **pagination**
  - `pageNo` (default `0`)
  - `pageSize` (default `10`)
- **filters**
  - `categories=CHEMICAL&categories=SPRAYER` (multi)
  - `subTypes=CHEMICAL&subTypes=OTHER` (multi)
  - `brandIds=br-...&brandIds=br-...` (multi; brand ids)
  - `hsnCode=1234`
  - `statuses=ACTIVE&statuses=INACTIVE` (multi)
  - `packageType=BOTTLE`
  - `fromDate=2026-01-01` (ISO date)
  - `toDate=2026-01-31` (ISO date)
- **search**
  - `search=PRD001`

### Response shape (high level)

`InventoryProductPaginationResponse<InventoryProductResponse>`:

- `count`: total items
- `next` / `prev`: pagination URLs (if present)
- `data[]`: rows; includes at least `id`, `productName`, `productCode`, `category`, `brand`, `baseUom`, `packageType`, `hsnCode`, `status`, `createdBy`, `createdAt`, `imageUrls`

---

## 10.2 Add Product — Create (single SKU OR variant-wise)

Your doc shows “Add Product” with variants. In backend, each variant is stored as its own SKU row.

### Recommended: ONE unified API (variants optional)

Use this single endpoint for the UI. The backend will automatically decide:\n

- if `variants[]` is **present and non-empty** → creates multiple SKUs (variant-bulk behavior)\n
- if `variants[]` is **missing or empty** → creates one SKU (single create behavior)\n

**API**: `POST /api/v1/inventory-products/upsert`\n

**Body**: `InventoryProductUnifiedUpsertRequest`\n

- Root fields: common product fields, images, packaging\n
- `variants[]` is optional\n

Notes:

- **Product Code**: in this variant-wise model, **each variant must have its own `productCode`** (ex: `P1-V1`, `P1-V2`). That `id` returned per variant is what stock uses later.
- **Images**: backend enforces **max 5 images**, **max 2MB each**.
- **Packaging fields (important)**: you can now pass `packageType`, `quantityPerPackage`, `unitsPerPackage` **inside each `variants[]` line** (recommended when bottle sizes differ, e.g. 100ml vs 250ml). If omitted, backend falls back to the common fields in the bulk request.

---

## 10.3 Edit Product

### Load current data

- `GET /api/v1/inventory-products/by-id?id=<productId>`

### Save changes (single SKU row)

- `PUT /api/v1/inventory-products/update?id=<productId>`

Note: In variant-wise design, editing “a product” typically means editing **one SKU row**. If the UI wants “edit all variants together”, it should update each SKU row individually (or a future bulk update API can be added).

---

## 10.4 View Product (read-only + audit)

### API

- `GET /api/v1/inventory-products/by-id?id=<productId>`

Includes:

- Product data
- Variant fields for this SKU
- Media files
- Audit: `createdBy/createdAt`, `updatedBy/updatedAt`, `deletedBy/deletedAt`

If you want to show all variants of the same logical product:

- Use the `groupKey` returned by variant-bulk (or returned on dropdown/list) and call:
  - `GET /api/v1/inventory-products/group-summary?groupKey=<groupKey>`

---

## 10.5 Delete Product (soft delete)

### API

- `DELETE /api/v1/inventory-products/delete?id=<productId>`

Behavior:

- Sets status to `INACTIVE` (soft delete)
- Existing stock records remain unchanged

---

## Product dropdown (used by other modules like Stock, Service chemicals)

### API

`GET /api/v1/inventory-products/dropdown`

### What it returns (variant-wise)

Each dropdown option corresponds to **one SKU row** (one variant).

Fields included:

- `id` (this is the **productId** to use in stock ledger / stock requests)
- `productName`
- `productCode`
- `groupKey` (nullable)
- `variantName`
- `hsnCode`
- `baseUom`

Filtering:

- Only returns rows where **`status == ACTIVE` AND `variantStatus == ACTIVE`**.
