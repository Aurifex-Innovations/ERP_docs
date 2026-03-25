# Module 9: TaxType and HsnCode Module API Documentation

## Short Description

The Tax Management module is responsible for defining the organization's tax structure (e.g., GST, Cess) and mapping HSN/SAC codes to specific products and services. It serves as a foundational module for Inventory (Module 10) and Billing, ensuring that tax calculations are automated and compliant with regional regulations.

---

## APIs

### 1. Tax Type Management

#### Create Tax Type

- **Method:** `POST`
- **Endpoint:** `/api/v1/tax-types`
- **Purpose:** Configure a new tax type (e.g., CGST, SGST, IGST) with a default rate.
- **Use Case:** Adding a new tax category during initial setup.
- **Request Body:** `TaxTypeRequest`
- **Response:** `TaxTypeResponse`
- **Access / Security:** `TAX_MANAGEMENT_ADD` authority or `ROLE_CEO`.

#### Update Tax Type

- **Method:** `PUT`
- **Endpoint:** `/api/v1/tax-types/update`
- **Purpose:** Update rate, applicability, or status of an existing tax type.
- **Use Case:** Adjusting tax rates based on government updates.
- **Request Params:** `id` (Long)
- **Request Body:** `TaxTypeRequest`
- **Response:** `TaxTypeResponse`
- **Access / Security:** `TAX_MANAGEMENT_EDIT` authority or `ROLE_CEO`.
- **Notes:** Requires a `changeReason` if the tax rate is modified.

#### Get Tax Type by ID

- **Method:** `GET`
- **Endpoint:** `/api/v1/tax-types/by-id`
- **Purpose:** Retrieve full configuration details of a specific tax type.
- **Request Params:** `id` (Long)
- **Response:** `TaxTypeResponse`
- **Access / Security:** `TAX_MANAGEMENT_READ` authority or `ROLE_CEO`.

#### List Tax Types (Paginated)

- **Method:** `GET`
- **Endpoint:** `/api/v1/tax-types`
- **Purpose:** Fetch a filtered and paginated list of all tax types.
- **Used In:** Tax Types Master Table.
- **Request Query Params:**
  - `status` (Enum): `ACTIVE` / `INACTIVE`.
  - `applicability` (Enum): `GOODS` / `SERVICES` / `BOTH`.
  - `startDate` / `endDate`: Filter by creation date.
  - `searchField`: Search by Tax Name.
  - `pageNo` / `pageSize`: Pagination control.
- **Response:** `PaginationResponse<TaxTypeResponse>`
- **Access / Security:** `TAX_MANAGEMENT_READ` authority or `ROLE_CEO`.

#### Delete Tax Type

- **Method:** `DELETE`
- **Endpoint:** `/api/v1/tax-types`
- **Purpose:** Soft-delete a tax type.
- **Request Params:** `id` (Long)
- **Access / Security:** `TAX_MANAGEMENT_DELETE` authority or `ROLE_CEO`.
- **Notes:** Deletion is blocked if the tax type is currently mapped to any HSN codes.

#### Tax Type Dropdown

- **Method:** `GET`
- **Endpoint:** `/api/v1/tax-types/dropdown`
- **Purpose:** Fetch a simplified list of active tax types.
- **Used In:** HSN Code creation form (Tax Configuration section).
- **Response:** `List<TaxDropdown>`

---

### 2. HSN Code Management

#### Create HSN Code

- **Method:** `POST`
- **Endpoint:** `/api/v1/tax/hsn-codes`
- **Purpose:** Create an HSN code and map it to relevant tax types.
- **Use Case:** Adding a new product category HSN (e.g., 8424 for Mechanical Sprayers).
- **Request Body:** `HsnCodeCreateRequest`
- **Response:** `HsnCodeResponse`
- **Access / Security:** `TAX_MANAGEMENT_ADD` authority or `ROLE_CEO`.

#### Update HSN Code

- **Method:** `PUT`
- **Endpoint:** `/api/v1/tax/hsn-codes/update`
- **Purpose:** Modify HSN description or update its tax mapping.
- **Request Params:** `id` (Long)
- **Request Body:** `HsnCodeCreateRequest`
- **Response:** `HsnCodeResponse`
- **Access / Security:** `TAX_MANAGEMENT_EDIT` authority or `ROLE_CEO`.

#### Get HSN Code by ID

- **Method:** `GET`
- **Endpoint:** `/api/v1/tax/hsn-codes/by-id`
- **Purpose:** Fetch detailed mapping of an HSN code.
- **Request Params:** `id` (Long)
- **Response:** `HsnCodeResponse`
- **Access / Security:** `TAX_MANAGEMENT_READ` authority or `ROLE_CEO`.

#### List HSN Codes (Paginated)

- **Method:** `GET`
- **Endpoint:** `/api/v1/tax/hsn-codes`
- **Purpose:** Fetch a filtered and paginated list of HSN codes.
- **Used In:** HSN Code Master Table.
- **Request Query Params:**
  - `status` (Enum): `ACTIVE` / `INACTIVE`.
  - `productCategory` (Enum): `ASSET`, `CONSUMABLES`, `RESALE`.
  - `taxTypeId` (Long): Filter by a specific mapped tax.
  - `searchField`: Search by HSN Code or Description.
- **Response:** `PaginationResponse<HsnCodeResponse>`
- **Access / Security:** `TAX_MANAGEMENT_READ` authority or `ROLE_CEO`.

#### HSN Code Dropdown

- **Method:** `GET`
- **Endpoint:** `/api/v1/tax/hsn-codes/dropdown`
- **Purpose:** List active HSN codes for product association.
- **Used In:** Add Product Form (Step 5).
- **Response:** `List<HsnDropdown>`

#### Get Tax Mapping for Products

- **Method:** `GET`
- **Endpoint:** `/api/v1/tax/hsn-codes/products`
- **Purpose:** Get the live tax rates for a specific HSN code.
- **Use Case:** Auto-filling CGST/SGST/IGST rates when a user selects an HSN code while adding a product.
- **Request Params:** `id` (Long - HSN ID)
- **Response:** `HsnProductResponse` (Contains effective tax rates).

---

## Module Enums

### TaxCategory

- `CENTRAL` (e.g., CGST)
- `STATE` (e.g., SGST)
- `INTEGRATED` (e.g., IGST)
- `CESS`

### TaxApplicability

- `GOODS`
- `SERVICES`
- `BOTH`

### ProductCategory

- `ASSET`
- `CONSUMABLES`
- `RESALE`

### ProductSubcategory

- `CHEMICALS`
- `MACHINE`
- `SPRAYER`
- `POWDER`

### Status (Tax/HSN)

- `ACTIVE`
- `INACTIVE`

---

## Notes for Frontend

- **Prerequisite Check:** Ensure at least one Tax Type (CGST/SGST or IGST) is created before attempting to add an HSN Code.
- **Validation Rules:**
  - HSN Codes must be exactly 4, 6, or 8 numeric digits.
  - Tax rates should be restricted to 0-100 with max 2 decimal places.
- **Auto-fill Logic:** Trigger a call to `/api/v1/tax/hsn-codes/products` immediately after an HSN Code is selected in the Product Management module. These values should be displayed in the UI as read-only or informational tags.
- **Soft Delete:** Both modules use soft-delete. The UI should handle `INACTIVE` status by styling rows with a grayed-out effect or showing an "Inactive" badge.
