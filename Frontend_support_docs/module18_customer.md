# Customer Management (Module 18)

## Short Description

The Customer Management module is the central hub for managing client data within the ecosystem. It supports the entire lifecycle from converting a **Lead** into a **Customer** (via Lead Conversion) to manual customer creation and long-term data maintenance. Frontend developers should use this module to provide a 360-degree view of clients, including their contact details, billing information, financial contacts, and (in future phases) their associated contracts and sales orders.

## Authorization

- **Authentication Type**: Bearer Token (JWT).
- **Required Header**: `Authorization: Bearer <token>`
- **Required Authorities**:
  - `CUSTOMER_MANAGEMENT_READ`: Required for listing, viewing details, and dropdowns.
  - `CUSTOMER_MANAGEMENT_ADD`: Required for creating new customers.
  - `CUSTOMER_MANAGEMENT_UPDATE`: Required for modifying existing customer data.
  - `CUSTOMER_MANAGEMENT_DELETE`: Required for deactivating/soft-deleting customers.
- **Tenant Behavior**: Data is filtered by the user's tenant/organization context automatically by the backend.
- **Access Restrictions**: Role `CEO` has full access to all operations. Data is non-updatable if `isDeleted` is true.

## Enums Used In This Module

### Status

| Value      | Meaning                                                                                | Used In                       |
| ---------- | -------------------------------------------------------------------------------------- | ----------------------------- |
| `ACTIVE`   | The customer is operational and can be used in Contracts/Orders.                       | Create/Update/Filter/Response |
| `INACTIVE` | The customer is deactivated (soft-deleted).                                            | Delete Response/Filter        |
| `DRAFT`    | Initial state where full validation (Phone/Email/GST) is bypassed for saving progress. | Create Request                |

### CustomerType

| Value      | Meaning                                               | Used In       |
| ---------- | ----------------------------------------------------- | ------------- |
| `CONTRACT` | Long-term clients with recurring service agreements.  | Create/Update |
| `ONE_TIME` | Transactional clients for single-service engagements. | Create/Update |
| `PRODUCT`  | Clients primarily purchasing hardware or licenses.    | Create/Update |

### EntryMode

| Value              | Meaning                                                     | Used In        |
| ------------------ | ----------------------------------------------------------- | -------------- |
| `MANUAL_ENTRY`     | Customer data is entered from scratch by the user.          | Create Request |
| `IMPORT_FROM_LEAD` | Data is imported from an existing Lead (requires `leadId`). | Create Request |

## API List

| Method   | Endpoint                    | Purpose                                          | Authorization Required |
| -------- | --------------------------- | ------------------------------------------------ | ---------------------- |
| `GET`    | `/api/v1/customer/dropdown` | Fetch lightweight list for select components.    | `READ`                 |
| `POST`   | `/api/v1/customer`          | Create a new customer (Manual or Lead-based).    | `ADD`                  |
| `PUT`    | `/api/v1/customer/update`   | Modify customer details (with audit logging).    | `UPDATE`               |
| `GET`    | `/api/v1/customer/by-id`    | Retrieve full 360-degree view of a customer.     | `READ`                 |
| `GET`    | `/api/v1/customer`          | Paginated search and filtering of customers.     | `READ`                 |
| `DELETE` | `/api/v1/customer/delete`   | Deactivate/Soft-delete a customer with a reason. | `DELETE`               |

## API Details

### GET `/api/v1/customer/dropdown`

**Purpose**: Fetches a simplified list of active customers (ID and Name) for population Select/Autocomplete components.

**Authorization**:

- Token: Required
- Authority: `CUSTOMER_MANAGEMENT_READ`

**Query Parameters**: Not applicable

**Request Body**: Not applicable

**Full Response JSON Examples**:

#### Success Response

```json
{
  "status": 200,
  "message": "Customer Dropdown Fetched Successfully",
  "data": [
    {
      "id": "CUST-A1B2C",
      "customerName": "ABC Innovations Pvt Ltd"
    },
    {
      "id": "CUST-X9Y8Z",
      "customerName": "Global Corp"
    }
  ]
}
```

---

### POST `/api/v1/customer`

**Purpose**: Registers a new customer in the system. Use `IMPORT_FROM_LEAD` mode to automatically convert a Lead.

**Authorization**:

- Token: Required
- Authority: `CUSTOMER_MANAGEMENT_ADD`

**Request Body Fields**:
| Field | Type | Required | Validation | Description |
|---|---|---|---|---|
| `entryMode` | Enum | Yes | Not null | `MANUAL_ENTRY` or `IMPORT_FROM_LEAD` |
| `leadId` | String | Condition | Max 50 | Required if `entryMode` is `IMPORT_FROM_LEAD`. |
| `customerType` | Enum | Yes | Not null | `CONTRACT`, `ONE_TIME`, `PRODUCT`. |
| `fullName` | String | Yes | 3-100 chars | Legal name of the customer entity. |
| `industryType` | String | No | Max 50 | Sector (e.g., IT, Manufacturing). |
| `panNumber` | String | No | `^[A-Z]{5}[0-9]{4}[A-Z]{1}$` | 10-digit Indian PAN format. |
| `gstNumber` | String | No | GST Regex | 15-digit GST format. |
| `contactPerson` | String | Yes | 3-100 chars | Primary point of contact name. |
| `designation` | String | No | Max 100 | Designation of contact person. |
| `phone` | String | Yes | 10 digits | Primary mobile number (unique). |
| `email` | String | Yes | Email format | Primary contact email. |
| `branchId` | String | Yes | Not blank | ID of the internal branch managing this client. |
| `billingAddressLine1` | String | Yes | 10-200 chars | Primary billing address line. |
| `billingAddressLine2` | String | No | Text | Optional address details. |
| `city` | String | Yes | Max 50 | Billing city. |
| `state` | String | Yes | Max 50 | Billing state. |
| `pincode` | String | Yes | 6 digits | Billing pincode. |
| `country` | String | No | Default: India | Billing country. |
| `googleMapUrl` | String | Yes | URL format | Link to office location. |
| `financeContactName` | String | Yes | Max 100 | Finance dept contact name. |
| `financeContactPhone` | String | Yes | 10 digits | Finance dept mobile number. |
| `financeContactEmail` | String | No | Email format | Finance dept email. |
| `status` | Enum | No | Default: DRAFT | `ACTIVE`, `DRAFT`, `INACTIVE`. |

**Full Request JSON Examples**:

#### Manual Entry (Minimal Valid)

```json
{
  "entryMode": "MANUAL_ENTRY",
  "customerType": "CONTRACT",
  "fullName": "Tech Solv",
  "contactPerson": "Amit Kumar",
  "phone": "9876543201",
  "email": "amit@techsolv.com",
  "branchId": "BR-01",
  "billingAddressLine1": "Tower A, High Tech City",
  "city": "Hyderabad",
  "state": "Telangana",
  "pincode": "500081",
  "googleMapUrl": "https://maps.google.com/ex1",
  "financeContactName": "Finance Manager",
  "financeContactPhone": "9988776655",
  "status": "ACTIVE"
}
```

#### Import From Lead

```json
{
  "entryMode": "IMPORT_FROM_LEAD",
  "leadId": "LEAD-4455",
  "customerType": "CONTRACT",
  "fullName": "Converted Client Inc",
  "phone": "9000011111",
  "email": "converted@client.com",
  "branchId": "BR-05",
  "status": "ACTIVE"
}
```

**Response**:

- Status: `201 Created`

```json
{
  "status": 201,
  "message": "Customer Created Successfully",
  "data": {
    "id": "CUST-XJ7Y2",
    "fullName": "Tech Solv",
    "branchName": "Hyderabad Main"
  }
}
```

---

### PUT `/api/v1/customer/update`

**Purpose**: Updates existing customer data. Note that `panNumber` and `customerType` cannot be changed after initial creation.

**Authorization**:

- Authority: `CUSTOMER_MANAGEMENT_UPDATE`

**Query Parameters**:
| Field | Type | Required | Description |
|---|---|---|---|
| `id` | String | Yes | Logical ID (e.g., CUST-AXUYS) |

**Full Request JSON Examples**:

#### Update Request Example

```json
{
  "fullName": "Tech Solv Global",
  "contactPerson": "Amit Varma",
  "industryType": "FinTech",
  "billingAddressLine1": "New Office Park, Sector 44",
  "city": "Cyberabad",
  "status": "ACTIVE"
}
```

---

### GET `/api/v1/customer/by-id`

**Purpose**: Retrieves the "360-degree view" of a customer, including lead origin details and branch info.

**Query Parameters**:
| Field | Type | Required | Description |
|---|---|---|---|
| `id` | String | Yes | Customer ID |

**Response**:

```json
{
  "status": 200,
  "message": "Success",
  "data": {
    "id": "CUST-XJ7Y2",
    "fullName": "Tech Solv",
    "industryType": "IT",
    "gstNumber": "27AAAAA0000A1Z5",
    "branchName": "Hyderabad Main",
    "leadName": "John's Software Lead",
    "leadEmail": "john@ex.com",
    "activeSites": [],
    "contracts": [],
    "salesOrders": [],
    "ltv": 0.0
  }
}
```

---

### GET `/api/v1/customer`

**Purpose**: Paginated list retrieval. Supports search and multiple filters.

**Query Parameters**:
| Field | Type | Required | Description | Allowed Values |
|---|---|---|---|---|
| `pageNo` | Integer | No | Default: 0 | |
| `pageSize` | Integer | No | Default: 10 | |
| `search` | String | No | Search by Name, Email, or Phone | |
| `status` | Enum | No | Filter by status | ACTIVE, INACTIVE, DRAFT |
| `customerType`| Enum | No | Filter by type | CONTRACT, ONE_TIME, PRODUCT |
| `branchId` | String | No | Filter by managing branch | |

**Full Response JSON Examples**:

```json
{
    "status": 200,
    "data": {
        "count": 12,
        "data": [...],
        "next": "http://localhost:8080/api/v1/customer?pageSize=10&pageNo=1",
        "prev": null
    }
}
```

---

### DELETE `/api/v1/customer/delete`

**Purpose**: Deactivates a customer. This is a soft-delete (sets `isDeleted = true`).

**Query Parameters**:
| Field | Type | Required | Description |
|---|---|---|---|
| `id` | String | Yes | Customer ID |
| `reason` | String | Yes | Reason for deactivation |

**cURL Example**:

```bash
curl -X DELETE "http://localhost:8080/api/v1/customer/delete?id=CUST-123&reason=Business%20Closed" \
     -H "Authorization: Bearer <token>"
```

## Exceptions / Error Cases

| HTTP Status | Reason                 | When It Happens                                                         | Frontend Handling Note                                                  |
| ----------- | ---------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| `400`       | Validation Failure     | Invalid PAN/GST format or missing mandatory fields for `ACTIVE` status. | Show red borders on specific fields using the `errors` map in response. |
| `400`       | Immutability Violation | Attempting to change `panNumber` or `customerType` during update.       | Alert user that these fields are read-only after creation.              |
| `404`       | Not Found              | Providing a non-existent `id` or `leadId`.                              | Redirect to list page with an error toast.                              |
| `409`       | Conflict               | Phone or GST number already registered by another customer.             | Suggest searching for the existing customer record.                     |

#### Validation Error Example

```json
{
  "status": 400,
  "message": "Validation Failed",
  "errors": {
    "gstNumber": "Invalid GST Number format",
    "phone": "Phone must be a 10-digit mobile number"
  }
}
```

## Validation and Exception Summary

| Field / Scenario    | Validation / Rule            | Error Type        | Frontend Impact                       |
| ------------------- | ---------------------------- | ----------------- | ------------------------------------- |
| **PAN Number**      | `^[A-Z]{5}[0-9]{4}[A-Z]{1}$` | `400 Bad Request` | Inline validation on blur.            |
| **GST Number**      | 15-digit Regex               | `400 Bad Request` | Masked input field recommended.       |
| **Customer Type**   | Immutable                    | `400 Bad Request` | Disable field in Update Form.         |
| **Duplicate Phone** | Unique constraint            | `409 Conflict`    | Check uniqueness on blur if possible. |

## Frontend Integration Notes

1. **Dropdown Dependencies**: The `branchId` must be fetched from the Branch module's dropdown API before rendering the Customer Create form.
2. **Read-only Fields**: In the **Update Form**, `panNumber`, `customerType`, and `entryMode` should be rendered as read-only labels/disabled fields.
3. **Draft Mode**: If the user wants to "Save for later", set `status = DRAFT`. The backend will skip strict validations for Phone/Address in this state.
4. **Audit Logs**: The system automatically tracks every field change on `update`. Ensure the user knows that "New Name" vs "Old Name" will be visible in the history logs.
5. **Search Debounce**: Recommend a 300ms-500ms debounce on the `search` query parameter for the list view.
