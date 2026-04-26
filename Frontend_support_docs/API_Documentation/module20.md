# Module 20: Sales Order (SO) Management

## Table of Contents

- [20.1 Sales Order Master List View](#201-sales-order-master-list-view)
- [20.2 Add / Generate Sales Order Form](#202-add--generate-sales-order-form)
  - [Use Case 1: SERVICE_CONTRACT](#use-case-1-service_contract)
  - [Use Case 2: ONE_TIME_SERVICE (GMA-linked)](#use-case-2-one_time_service-gma-linked)
  - [Use Case 3: PRODUCT_SALE](#use-case-3-product_sale)
  - [Required APIs for POST](#required-apis-for-post)
  - [Prefill APIs](#prefill-apis)
- [20.3 View Sales Order Details](#203-view-sales-order-details)
  - [20.3.1 Tab 1: SO Summary & Line Items](#2031-tab-1-so-summary--line-items)
  - [20.3.2 Tab 2: Execution & Delivery Status](#2032-tab-2-execution--delivery-status)
- [20.4 Edit Sales Order Form](#204-edit-sales-order-form)
- [20.5 Delete (Cancel) Sales Order](#205-delete-cancel-sales-order)
- [Extra APIs](#extra-apis)

---

## 20.1 Sales Order Master List View

### Endpoint

```
GET {{baseUrl}}/api/v1/sales-orders
```

### Query Parameters

| Parameter  | Type    | Example          | Description              |
| ---------- | ------- | ---------------- | ------------------------ |
| orderTypes | string  | SERVICE_CONTRACT | Type of sales order      |
| statuses   | string  | OPEN             | Sales order status       |
| branchId   | string  | {{branchId}}     | Branch ID filter         |
| customerId | string  | {{customerId}}   | Customer ID filter       |
| dateFrom   | date    | 2026-01-01       | Start date filter        |
| dateTo     | date    | 2026-12-31       | End date filter          |
| search     | string  | -                | Search query             |
| pageNo     | integer | 0                | Page number              |
| pageSize   | integer | 10               | Number of items per page |

---

## 20.2 Add / Generate Sales Order Form

### Endpoint

```
POST {{baseUrl}}/api/v1/sales-orders
```

### Different Requests Based on Use Case

---

### Use Case 1: SERVICE_CONTRACT

Create / generate SO for Service Contract

#### Request Body

```json
{
  "orderType": "SERVICE_CONTRACT",
  "saveAsDraft": true,
  "customerId": "{{customerId}}",
  "branchId": "{{branchId}}",
  "soDate": "2026-06-01",
  "contractId": "{{contractId}}",
  "contractPaymentLineId": "{{contractPaymentLineId}}",
  "billingPeriodLabel": "Month 1",
  "discountType": "NONE",
  "discountValue": null,
  "executionNotes": "",
  "deliveryAddressType": "REGISTERED_SITE",
  "priority": "NORMAL",
  "expectedDeliveryDate": "2026-06-15",
  "sites": [
    {
      "contractSiteId": null,
      "siteName": "Client Site A",
      "address": "Plot 1",
      "city": "Mumbai",
      "state": "Maharashtra",
      "country": "India",
      "googleMapUrl": null,
      "category": "COMMERCIAL",
      "subCategory": "INTERNAL",
      "areaSqft": 5000,
      "contactPerson": "Site contact",
      "contactMobile": "9999999999",
      "services": [
        {
          "serviceTypeId": "{{serviceTypeId}}",
          "serviceTypeName": "General Pest Control",
          "visits": 1,
          "unitPrice": 50000,
          "sqft": 5000,
          "hsnCode": "998714",
          "taxPercent": 18
        }
      ],
      "chemicals": []
    }
  ],
  "productLines": []
}
```

---

### Use Case 2: ONE_TIME_SERVICE (GMA-linked)

Create / generate SO for One-Time Service

#### Request Body

```json
{
  "orderType": "ONE_TIME_SERVICE",
  "saveAsDraft": true,
  "customerId": "{{customerId}}",
  "branchId": "{{branchId}}",
  "soDate": "2026-06-01",
  "gmaSheetId": "{{gmaSheetId}}",
  "quotationId": "{{quotationId}}",
  "oneTimeSource": "QUOTATION_GMA",
  "discountType": "NONE",
  "executionNotes": "",
  "deliveryAddressType": "CUSTOM",
  "deliveryAddressLine1": "123, MG Road, full address line 10+ chars",
  "deliveryCity": "Pune",
  "deliveryState": "Maharashtra",
  "deliveryPincode": "411001",
  "deliveryCountry": "India",
  "priority": "NORMAL",
  "sites": [
    {
      "siteName": "One-time site",
      "city": "Pune",
      "state": "Maharashtra",
      "country": "India",
      "category": "RESIDENTIAL",
      "subCategory": "EXTERNAL",
      "areaSqft": 1200,
      "services": [
        {
          "serviceTypeId": "{{serviceTypeId}}",
          "serviceTypeName": "General Pest Control",
          "visits": 1,
          "unitPrice": 15000,
          "sqft": 1200,
          "taxPercent": 18
        }
      ],
      "chemicals": []
    }
  ],
  "productLines": []
}
```

---

### Use Case 3: PRODUCT_SALE

Create / generate SO for Product Sale

#### Request Body

```json
{
  "orderType": "PRODUCT_SALE",
  "saveAsDraft": true,
  "customerId": "{{customerId}}",
  "branchId": "{{branchId}}",
  "soDate": "2026-06-01",
  "discountType": "PERCENTAGE",
  "discountValue": 5,
  "executionNotes": "",
  "priority": "NORMAL",
  "sites": [],
  "productLines": [
    {
      "productId": "{{productId}}",
      "quantity": 2,
      "unitPrice": 50000,
      "taxPercent": 18
    }
  ]
}
```

---

### Required APIs for POST

#### Contract Dropdown

```
GET {{baseUrl}}/api/v1/sales-orders/eligible-contracts?pageNo=0&pageSize=10
```

#### Quotation Dropdown

```
GET {{baseUrl}}/api/v1/sales-orders/eligible-quotations?pageNo=0&pageSize=10&salesOrderNotConsumed=true&search=QUOT-001&limit=20
```

#### GMA Dropdown

```
GET {{baseUrl}}/api/v1/sales-orders/eligible-gma?pageNo=0&pageSize=10
```

#### Customer Dropdown

```
GET /api/v1/customer/dropdown
```

---

### Prefill APIs

These APIs help scaffold/prefill the sales order form from different sources:

#### Scaffold from Contract (Module 19)

```
GET {{baseUrl}}/api/v1/sales-orders/scaffold/contract?contractId={{contractId}}
```

#### Scaffold from GMA (Module 17)

```
GET {{baseUrl}}/api/v1/sales-orders/scaffold/gma?gmaSheetId={{gmaSheetId}}
```

#### Scaffold from Quotation (Module 16)

```
GET {{baseUrl}}/api/v1/sales-orders/scaffold/quotation?quotationId={{quotationId}}
```

---

## 20.3 View Sales Order Details

### Endpoint

```
GET {{baseUrl}}/api/v1/sales-orders/by-id?id={{salesOrderId}}
```

---

### 20.3.1 Tab 1: SO Summary & Line Items

#### Endpoint

```
GET {{baseUrl}}/api/v1/sales-orders/by-id?id={{salesOrderId}}
```

---

### 20.3.2 Tab 2: Execution & Delivery Status

#### Endpoint

```
GET {{baseUrl}}/api/v1/sales-orders/execution-status?id={{salesOrderId}}
```

---

## 20.4 Edit Sales Order Form

### Endpoint

```
PUT {{baseUrl}}/api/v1/sales-orders/update?id={{salesOrderId}}
```

### Request Body

Same structure as POST API (see section 20.2 for different use cases)

---

## 20.5 Delete (Cancel) Sales Order

### Endpoint

```
POST {{baseUrl}}/api/v1/sales-orders/cancel?id={{salesOrderId}}
```

---

## Extra APIs

### Download SO as PDF

#### Endpoint

```
GET {{baseUrl}}/api/v1/sales-orders/export-pdf?id={{salesOrderId}}
```

---

### Service Summary for Multiple Sales Orders

#### Endpoint

```
POST {{baseUrl}}/api/v1/sales-orders/service-summary
```

#### Request Body

```json
["{{salesOrderId}}"]
```

---

## Field Descriptions

### Common Fields

| Field                | Type    | Description                                      |
| -------------------- | ------- | ------------------------------------------------ |
| orderType            | string  | SERVICE_CONTRACT, ONE_TIME_SERVICE, PRODUCT_SALE |
| saveAsDraft          | boolean | Save as draft without finalizing                 |
| customerId           | string  | Customer identifier                              |
| branchId             | string  | Branch identifier                                |
| soDate               | date    | Sales order date (YYYY-MM-DD)                    |
| discountType         | string  | NONE, PERCENTAGE, FLAT                           |
| discountValue        | number  | Discount value (percentage or flat amount)       |
| executionNotes       | string  | Notes for execution team                         |
| priority             | string  | NORMAL, HIGH, URGENT                             |
| expectedDeliveryDate | date    | Expected delivery date                           |

### SERVICE_CONTRACT Specific Fields

| Field                 | Type   | Description                                 |
| --------------------- | ------ | ------------------------------------------- |
| contractId            | string | Associated contract identifier              |
| contractPaymentLineId | string | Payment line from contract                  |
| billingPeriodLabel    | string | Billing period identifier (e.g., "Month 1") |
| deliveryAddressType   | string | REGISTERED_SITE, CUSTOM                     |

### ONE_TIME_SERVICE Specific Fields

| Field                | Type   | Description                            |
| -------------------- | ------ | -------------------------------------- |
| gmaSheetId           | string | GMA Sheet identifier                   |
| quotationId          | string | Quotation identifier                   |
| oneTimeSource        | string | QUOTATION_GMA, etc.                    |
| deliveryAddressLine1 | string | Custom delivery address (min 10 chars) |
| deliveryCity         | string | Delivery city                          |
| deliveryState        | string | Delivery state/province                |
| deliveryPincode      | string | Delivery postal/PIN code               |
| deliveryCountry      | string | Delivery country                       |

### Site Fields

| Field          | Type   | Description                          |
| -------------- | ------ | ------------------------------------ |
| contractSiteId | string | Contract site identifier (if linked) |
| siteName       | string | Site name                            |
| address        | string | Site address                         |
| city           | string | City name                            |
| state          | string | State/Province                       |
| country        | string | Country name                         |
| googleMapUrl   | string | Google Maps URL                      |
| category       | string | COMMERCIAL, RESIDENTIAL, INDUSTRIAL  |
| subCategory    | string | INTERNAL, EXTERNAL                   |
| areaSqft       | number | Area in square feet                  |
| contactPerson  | string | On-site contact person name          |
| contactMobile  | string | Contact mobile number                |

### Service Fields

| Field           | Type   | Description                 |
| --------------- | ------ | --------------------------- |
| serviceTypeId   | string | Service type identifier     |
| serviceTypeName | string | Service type display name   |
| visits          | number | Number of service visits    |
| unitPrice       | number | Price per visit/unit        |
| sqft            | number | Area covered in square feet |
| hsnCode         | string | HSN code for taxation       |
| taxPercent      | number | Tax percentage              |

### Product Line Fields

| Field      | Type   | Description        |
| ---------- | ------ | ------------------ |
| productId  | string | Product identifier |
| quantity   | number | Quantity to sell   |
| unitPrice  | number | Price per unit     |
| taxPercent | number | Tax percentage     |

### Chemical Fields

| Field     | Type  | Description                                |
| --------- | ----- | ------------------------------------------ |
| chemicals | array | Array of chemical items (structure varies) |

---

## Order Type Reference

### SERVICE_CONTRACT

Sales orders generated from active contracts. Linked to contract payment schedules and billing periods.

**Key Features:**

- Requires contractId and contractPaymentLineId
- Typically recurring based on contract terms
- Uses registered site addresses from contract
- Includes billingPeriodLabel for tracking

### ONE_TIME_SERVICE

One-time service orders, often generated from quotations or GMA sheets.

**Key Features:**

- Can be linked to quotation or GMA
- Supports custom delivery addresses
- Typically non-recurring
- oneTimeSource field indicates origin

### PRODUCT_SALE

Sales orders exclusively for product sales (no service component).

**Key Features:**

- No sites array (empty)
- Uses productLines array instead
- Supports discount types
- Standard product catalog items

---

**End of Documentation**
