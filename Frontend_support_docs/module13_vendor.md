# Vendor Management API Documentation

## Short Description
The Vendor Management module handles the lifecycle of suppliers and service providers. It includes features for vendor onboarding, profile management, contract tracking, and document handling. This module integrates with Purchase Orders (PO) for efficient procurement workflows.

---

## APIs

### Create Vendor
- **Method:** `POST`
- **Endpoint:** `/api/v1/vendors`
- **Purpose:** Onboard a new vendor into the system.
- **Use Case:** Registering a new chemical supplier or maintenance service provider.
- **Used In:** "Add Vendor" form.
- **Request Body:** `VendorRequest`
- **Response:** `VendorResponse`
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('VENDOR_MANAGEMENT_ADD')`
- **Notes:** Validates uniqueness of GST number, email, and vendor name.

### Update Vendor
- **Method:** `PUT`
- **Endpoint:** `/api/v1/vendors/update`
- **Purpose:** Update existing vendor details or contract information.
- **Use Case:** Extending a vendor's contract end date or updating contact person details.
- **Used In:** "Edit Vendor" screen.
- **Request Params:** `id` (String)
- **Request Body:** `VendorRequest`
- **Response:** `VendorResponse`
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('VENDOR_MANAGEMENT_EDIT')`
- **Notes:** Deletes old files during document updates to save cloud storage.

### Get Vendor by ID
- **Method:** `GET`
- **Endpoint:** `/api/v1/vendors/by-id`
- **Purpose:** Fetch detailed information about a specific vendor.
- **Use Case:** Viewing the complete profile of a vendor for auditing.
- **Used In:** Vendor details view / View entries.
- **Request Params:** `id` (String)
- **Response:** `VendorResponse`
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('VENDOR_MANAGEMENT_READ')`

### Delete Vendor
- **Method:** `DELETE`
- **Endpoint:** `/api/v1/vendors/delete`
- **Purpose:** Soft-delete a vendor by setting status to `INACTIVE`.
- **Use Case:** Deactivating a vendor who is no longer providing services.
- **Used In:** Vendor list actions.
- **Request Params:** `id` (String)
- **Response:** Success confirmation.
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('VENDOR_MANAGEMENT_DELETE')`

### Filter/List Vendors
- **Method:** `GET`
- **Endpoint:** `/api/v1/vendors`
- **Purpose:** Search and filter vendors with pagination support.
- **Use Case:** Viewing all `ACTIVE` chemical suppliers or vendors with expiring contracts.
- **Used In:** Vendor Dashboard / Table View.
- **Query Params:**
  - `pageNo` (default: 0)
  - `pageSize` (default: 10)
  - `types` (List of `VendorType`)
  - `category` (`VendorCategory`)
  - `statuses` (List of `VendorStatus`)
  - `search` (Generic search string)
  - `contractEndDateFrom`, `contractEndDateTo` (Date range)
- **Response:** `VendorPaginationResponse<VendorResponse>`
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('VENDOR_MANAGEMENT_READ')`

### Download Vendor Document
- **Method:** `GET`
- **Endpoint:** `/api/v1/vendors/download`
- **Purpose:** Download the profile/contract document uploaded for a vendor.
- **Use Case:** Retrieving the GST certificate or Service Agreement for verification.
- **Used In:** Download icon in table or details view.
- **Request Params:** `id` (String)
- **Response:** File resource (PDF/JPG/PNG).
- **Access / Security:** `hasRole('CEO')` or `hasAuthority('VENDOR_MANAGEMENT_DOWNLOAD')`

---

## Reusable / Common APIs (Used in this Module)

### Vendor Dropdown List
- **Method:** `GET`
- **Endpoint:** `/api/v1/vendors/dropdown`
- **Purpose:** Get a lightweight list of active vendors for selection.
- **Use Case:** Selecting a vendor while creating a Purchase Order (PO).
- **Used In:** Dropdowns across other modules.

---

## Module Enums

### VendorStatus
- **Used In:** `VendorRequest`, `VendorResponse`, Filter
- **Values:**
  - `ACTIVE`
  - `INACTIVE`
  - `BLOCKED`

### VendorType
- **Used In:** `VendorRequest`, `VendorResponse`, Filter
- **Values:**
  - `SUPPLIER`
  - `SERVICE_PROVIDER`
  - `BOTH`

### VendorCategory
- **Used In:** `VendorRequest`, `VendorResponse`, Filter
- **Values:**
  - `CHEMICAL_SUPPLIER`
  - `EQUIPMENT_VENDOR`
  - `LOGISTICS_VENDOR`
  - `MAINTENANCE_VENDOR`
  - `OTHER`

### VendorRegistrationType
- **Used In:** `VendorRequest`, `VendorResponse`
- **Values:**
  - `REGISTERED`
  - `UNREGISTERED`

### BillingCycle
- **Used In:** `VendorRequest`, `VendorResponse`
- **Values:**
  - `WEEKLY`
  - `MONTHLY`
  - `QUARTERLY`
  - `CUSTOM`

### BillingType
- **Used In:** `VendorRequest`, `VendorResponse`
- **Values:**
  - `PER_SERVICE`
  - `MONTHLY`
  - `PROJECT`

### ContractType
- **Used In:** `VendorRequest`, `VendorResponse`
- **Values:**
  - `ANNUAL`
  - `PROJECT`
  - `ONE_TIME`

### DeliveryFrequency
- **Used In:** `VendorRequest`, `VendorResponse`
- **Values:**
  - `WEEKLY`
  - `MONTHLY`
  - `QUARTERLY`
  - `CUSTOM`

### InvoiceSubmissionMethod
- **Used In:** `VendorRequest`, `VendorResponse`
- **Values:**
  - `EMAIL`
  - `PORTAL`
  - `PHYSICAL`

### PaymentTerms
- **Used In:** `VendorRequest`, `VendorResponse`
- **Values:**
  - `ADVANCE`
  - `NET15`
  - `NET30`
  - `NET45`

---

## Notes for Frontend

- **Dropdown API**: Use `/api/v1/vendors/dropdown` for select inputs. It only returns `ACTIVE` vendors with Minimal data (`id`, `vendorName`).
- **File Validation**:
  - Max Size: **5 MB**.
  - Allowed Formats: **PDF, JPG, JPEG, PNG**.
- **Mandatory Fields**:
    - Vendor Name, Type, Category, Product Supplied, Contact Person, Phone (10 digits).
    - Email, Address, City, State, Pincode.
    - Vendor Registration Type.
- **Conditional Fields**:
    - If `billingCycle` is `CUSTOM`, `customBillingStartDate` and `customBillingEndDate` are required.
    - Contract details are mandatory only if `hasContract` is `true`.
- **Soft Delete**: To "delete" a vendor, the `vendorStatus` is set to `INACTIVE`. Avoid hard deletes for data auditability.
