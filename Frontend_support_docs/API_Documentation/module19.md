# Module 19: Contract Module

## Table of Contents

- [19.1 Contract Master List View](#191-contract-master-list-view)
- [19.2 Add Contract Form](#192-add-contract-form)
- [19.3 View Contract Details](#193-view-contract-details)
  - [19.3.1 Tab 1: Contract Terms, Scope & Sites](#1931-tab-1-contract-terms-scope--sites)
  - [19.3.2 Tab 2: Sales Order Schedule (Billing Log)](#1932-tab-2-sales-order-schedule-billing-log)
- [19.4 Edit / Amend Contract Form](#194-edit--amend-contract-form)
- [19.5 Delete (Terminate) Contract](#195-delete-terminate-contract)
- [Extra APIs](#extra-apis)

---

## 19.1 Contract Master List View

### Endpoint

```
GET {{baseUrl}}/api/v1/contracts
```

### Query Parameters

| Parameter       | Type    | Example        | Description              |
| --------------- | ------- | -------------- | ------------------------ |
| pageNo          | integer | 0              | Page number              |
| pageSize        | integer | 10             | Number of items per page |
| displayStatuses | string  | DRAFT          | Contract status filter   |
| customerId      | string  | {{customerId}} | Customer ID filter       |
| branchId        | string  | {{branchId}}   | Branch ID filter         |
| dateFrom        | date    | 2026-01-01     | Start date filter        |
| dateTo          | date    | 2026-12-31     | End date filter          |
| search          | string  | -              | Search query             |

### Request Body

```
None
```

---

## 19.2 Add Contract Form

### Endpoint

```
POST /api/v1/contracts
```

### Request Body (Draft or Activate)

```json
{
  "gmaSheetId": "{{gmaSheetId}}",
  "durationOption": "ONE_YEAR",
  "startDate": "2026-06-01",
  "endDate": "2027-05-31",
  "totalSaleValue": 100000,
  "contractReference": "CTR-2026-001",
  "renewalType": "MANUAL",
  "legalNotes": "",
  "sites": [
    {
      "gmaSiteId": "{{gmaSiteId}}",
      "services": [
        {
          "serviceTypeId": "{{serviceTypeId}}",
          "serviceTypeName": "General Pest Control",
          "contractMode": "CONTRACT",
          "frequency": "MONTHLY",
          "annualFrequency": 12,
          "preferredDays": "MON,WED",
          "preferredTimeSlot": "MORNING",
          "technicianTeamId": "{{technicianTeamId}}",
          "technicianTeamName": "Team A",
          "serviceSaleValue": 100000
        }
      ]
    }
  ],
  "paymentScheduleType": "MONTHLY_POST",
  "invoicingFrequency": "MONTHLY",
  "customPaymentDescription": null,
  "advancePaymentDueDate": null,
  "legalSlaRemarks": "",
  "paymentLines": [
    {
      "periodLabel": "Period 1",
      "periodDescription": "Single payment line (example)",
      "amount": 100000,
      "dueDate": "2026-06-01",
      "paid": false,
      "locked": false
    }
  ],
  "activate": false
}
```

### Request Body (Create contract — manual site, no gmaSiteId)

```json
{
  "gmaSheetId": "{{gmaSheetId}}",
  "durationOption": "ONE_YEAR",
  "startDate": "2026-06-01",
  "endDate": "2027-05-31",
  "totalSaleValue": 500000,
  "contractReference": "CTR-MANUAL-SITE",
  "renewalType": "NON_RENEWABLE",
  "sites": [
    {
      "siteName": "Client HQ",
      "address": "Plot 10, Industrial Area",
      "city": "Pune",
      "state": "Maharashtra",
      "country": "India",
      "googleMapUrl": "https://maps.google.com/?q=18.5204,73.8567",
      "areaSqft": 8000,
      "category": "COMMERCIAL",
      "subCategory": "INTERNAL",
      "services": [
        {
          "serviceTypeId": "{{serviceTypeId}}",
          "serviceTypeName": "General Pest Control",
          "contractMode": "CONTRACT",
          "frequency": "MONTHLY",
          "annualFrequency": 12,
          "preferredDays": "TUE,THU",
          "preferredTimeSlot": "AFTERNOON",
          "technicianTeamId": "{{technicianTeamId}}",
          "technicianTeamName": "Team B",
          "serviceSaleValue": 500000
        }
      ]
    }
  ],
  "paymentScheduleType": "MONTHLY_POST",
  "invoicingFrequency": "MONTHLY",
  "paymentLines": [
    {
      "periodLabel": "100%",
      "amount": 500000,
      "dueDate": "2026-06-01"
    }
  ],
  "activate": false
}
```

### Required APIs List

**For GMA Selection:**

```
GET /api/v1/gma/sheets/dropdown
```

---

## 19.3 View Contract Details

### Endpoint

```
GET {{baseUrl}}/api/v1/contracts/by-id?id={{contractId}}
```

---

### 19.3.1 Tab 1: Contract Terms, Scope & Sites

#### Endpoint

```
GET {{baseUrl}}/api/v1/contracts/by-id?id={{contractId}}
```

---

### 19.3.2 Tab 2: Sales Order Schedule (Billing Log)

Similar to Module 20 table view.

#### Endpoint

```
GET {{baseUrl}}/api/v1/contracts/sales-order-schedule
```

#### Query Parameters

| Parameter  | Type    | Example        | Description              |
| ---------- | ------- | -------------- | ------------------------ |
| contractId | string  | {{contractId}} | Contract ID              |
| pageNo     | integer | 0              | Page number              |
| pageSize   | integer | 10             | Number of items per page |

---

## 19.4 Edit / Amend Contract Form

### Endpoint

```
PUT {{baseUrl}}/api/v1/contracts/amend?id={{contractId}}
```

### Request Body

```json
{
  "amendmentReason": "VALUE_ADJUSTMENT",
  "amendmentRemarks": "Price revision per customer agreement",
  "payload": {
    "durationOption": "ONE_YEAR",
    "startDate": "2026-06-01",
    "endDate": "2027-05-31",
    "totalSaleValue": 104166.67,
    "contractReference": "CTR-2026-001-A1",
    "renewalType": "MANUAL",
    "sites": [
      {
        "gmaSiteId": "{{gmaSiteId}}",
        "services": [
          {
            "serviceTypeId": "{{serviceTypeId}}",
            "serviceTypeName": "General Pest Control",
            "contractMode": "CONTRACT",
            "frequency": "MONTHLY",
            "annualFrequency": 12,
            "preferredTimeSlot": "MORNING",
            "technicianTeamId": "{{technicianTeamId}}",
            "technicianTeamName": "Team A",
            "serviceSaleValue": 104166.67
          }
        ]
      }
    ],
    "paymentScheduleType": "MONTHLY_POST",
    "invoicingFrequency": "MONTHLY",
    "paymentLines": [
      {
        "periodLabel": "Period 1",
        "amount": 104166.67,
        "dueDate": "2026-06-01",
        "paid": false,
        "locked": false
      }
    ],
    "activate": false
  }
}
```

---

## 19.5 Delete (Terminate) Contract

### Endpoint

```
POST {{baseUrl}}/api/v1/contracts/terminate?id={{contractId}}
```

### Request Body

```json
{
  "effectiveClosureDate": "2026-12-31",
  "reasonCode": "MUTUAL_AGREEMENT",
  "additionalRemarks": "Terminated via Postman",
  "cancelOpenSalesOrders": false,
  "acknowledgeOpenSalesOrders": true
}
```

---

## Extra APIs

### Download Service / Billing Log CSV (EXPORT)

#### Endpoint

```
GET {{baseUrl}}/api/v1/contracts/export/csv?id={{contractId}}
```

---

### Update Draft Contract

#### Endpoint

```
PUT {{baseUrl}}/api/v1/contracts/update?id={{contractId}}
```

#### Request Body

```json
{
  "gmaSheetId": "{{gmaSheetId}}",
  "durationOption": "ONE_YEAR",
  "startDate": "2026-06-01",
  "endDate": "2027-05-31",
  "totalSaleValue": 100000,
  "contractReference": "CTR-2026-001",
  "renewalType": "MANUAL",
  "legalNotes": "Updated notes",
  "sites": [
    {
      "gmaSiteId": "{{gmaSiteId}}",
      "services": [
        {
          "serviceTypeId": "{{serviceTypeId}}",
          "serviceTypeName": "General Pest Control",
          "contractMode": "CONTRACT",
          "frequency": "MONTHLY",
          "annualFrequency": 12,
          "preferredDays": "MON,WED",
          "preferredTimeSlot": "MORNING",
          "technicianTeamId": "{{technicianTeamId}}",
          "technicianTeamName": "Team A",
          "serviceSaleValue": 100000
        }
      ]
    }
  ],
  "paymentScheduleType": "MONTHLY_POST",
  "invoicingFrequency": "MONTHLY",
  "paymentLines": [
    {
      "periodLabel": "Period 1",
      "amount": 100000,
      "dueDate": "2026-06-01"
    }
  ],
  "activate": false
}
```

---

## Field Descriptions

### Common Fields

| Field             | Type   | Description                        |
| ----------------- | ------ | ---------------------------------- |
| gmaSheetId        | string | GMA Sheet identifier               |
| durationOption    | string | Contract duration (ONE_YEAR, etc.) |
| startDate         | date   | Contract start date (YYYY-MM-DD)   |
| endDate           | date   | Contract end date (YYYY-MM-DD)     |
| totalSaleValue    | number | Total contract value               |
| contractReference | string | Unique contract reference number   |
| renewalType       | string | MANUAL, AUTO, NON_RENEWABLE        |
| legalNotes        | string | Legal terms and conditions         |
| legalSlaRemarks   | string | SLA-related legal remarks          |

### Site Fields

| Field        | Type   | Description                                     |
| ------------ | ------ | ----------------------------------------------- |
| gmaSiteId    | string | GMA Site identifier (optional for manual sites) |
| siteName     | string | Site name (for manual sites)                    |
| address      | string | Site address                                    |
| city         | string | City name                                       |
| state        | string | State/Province                                  |
| country      | string | Country name                                    |
| googleMapUrl | string | Google Maps URL                                 |
| areaSqft     | number | Area in square feet                             |
| category     | string | COMMERCIAL, RESIDENTIAL, etc.                   |
| subCategory  | string | INTERNAL, EXTERNAL, etc.                        |

### Service Fields

| Field              | Type   | Description                        |
| ------------------ | ------ | ---------------------------------- |
| serviceTypeId      | string | Service type identifier            |
| serviceTypeName    | string | Service type display name          |
| contractMode       | string | CONTRACT mode                      |
| frequency          | string | MONTHLY, QUARTERLY, etc.           |
| annualFrequency    | number | Number of services per year        |
| preferredDays      | string | Comma-separated days (MON,TUE,WED) |
| preferredTimeSlot  | string | MORNING, AFTERNOON, EVENING        |
| technicianTeamId   | string | Assigned team identifier           |
| technicianTeamName | string | Team display name                  |
| serviceSaleValue   | number | Service value                      |

### Payment Fields

| Field                    | Type    | Description                      |
| ------------------------ | ------- | -------------------------------- |
| paymentScheduleType      | string  | MONTHLY_POST, ADVANCE, etc.      |
| invoicingFrequency       | string  | MONTHLY, QUARTERLY, etc.         |
| customPaymentDescription | string  | Custom payment terms description |
| advancePaymentDueDate    | date    | Due date for advance payment     |
| periodLabel              | string  | Payment period label             |
| periodDescription        | string  | Payment period description       |
| amount                   | number  | Payment amount                   |
| dueDate                  | date    | Payment due date                 |
| paid                     | boolean | Payment status                   |
| locked                   | boolean | Whether payment line is locked   |

### Amendment Fields

| Field            | Type   | Description                          |
| ---------------- | ------ | ------------------------------------ |
| amendmentReason  | string | VALUE_ADJUSTMENT, SCOPE_CHANGE, etc. |
| amendmentRemarks | string | Detailed remarks about amendment     |

### Termination Fields

| Field                      | Type    | Description                    |
| -------------------------- | ------- | ------------------------------ |
| effectiveClosureDate       | date    | Contract closure date          |
| reasonCode                 | string  | MUTUAL_AGREEMENT, BREACH, etc. |
| additionalRemarks          | string  | Termination notes              |
| cancelOpenSalesOrders      | boolean | Cancel pending sales orders    |
| acknowledgeOpenSalesOrders | boolean | Acknowledge open orders        |

---

**End of Documentation**
