# Module 16: Quotation Management

## Table of Contents

- [Common Headers](#common-headers)
- [Status Lifecycle (Server-Enforced)](#status-lifecycle-server-enforced)
- [16.1 Quotation Dashboard – List](#161-quotation-dashboard--list)
- [16.2 Create Quotation – Save Draft or Send](#162-create-quotation--save-draft-or-send)
- [16.3 View Quotation Details](#163-view-quotation-details)
- [16.4 Delete (Soft-Delete) DRAFT Quotation](#164-delete-soft-delete-draft-quotation)
- [16.5 Update Status (Internal) – ACCEPTED/REJECTED](#165-update-status-internal--acceptedrejected)
- [16.6 Send Quotation (DRAFT → SENT)](#166-send-quotation-draft--sent)
- [16.7 Resend Quotation](#167-resend-quotation)
- [16.8 Download Quotation PDF](#168-download-quotation-pdf)
- [16.9 Upload Attachments (Base64)](#169-upload-attachments-base64)
- [16.10 Dropdowns (Create Form)](#1610-dropdowns-create-form)
  - [16.10.1 Qualified Leads](#16101-qualified-leads)
  - [16.10.2 Active Services](#16102-active-services)
  - [16.10.3 Active Products](#16103-active-products)
- [16.11 Public (Client) Endpoints](#1611-public-client-endpoints)
  - [16.11.1 Public View (SENT → VIEWED)](#16111-public-view-sent--viewed)
  - [16.11.2 Client Decision (ACCEPT/REJECT)](#16112-client-decision-acceptreject)
- [Reference – Enums](#reference--enums)

---

## Common Headers

### Authenticated APIs (most endpoints)

- `Authorization: Bearer {{token}}`
- `X-Tenant-ID: {{tenant}}`

### Public client APIs (no JWT)

- `X-Tenant-ID: {{tenant}}`

> `X-Tenant-ID` is required for tenant routing (multi-tenant schemas).

---

## Status Lifecycle (Server-Enforced)

**Primary flow**

`DRAFT → SENT → VIEWED → (ACCEPTED | REJECTED)`

**System transition**

`SENT/VIEWED → EXPIRED` when `validTill` has passed (nightly scheduler).

**Rules enforced**

- Only **DRAFT** can be deleted.
- Only **DRAFT** can be sent.
- Only **SENT** becomes **VIEWED** (on first public view).
- Only **SENT/VIEWED** can be **ACCEPTED/REJECTED** (public client decision or internal patch).

**Lead mapping (when `sourceType=FROM_LEAD`)**

- On **SENT**: Lead status → `QUOTATION_SENT`
- On **ACCEPTED**: Lead status → `CONVERTED`

---

## 16.1 Quotation Dashboard – List

### Endpoint

```
GET {{baseUrl}}/api/v1/quotations
```

### Query Parameters

| Parameter      | Type                | Example           | Description  |
| -------------- | ------------------- | ----------------- | ------------ |
| statuses       | `QuotationStatus[]` | `DRAFT,SENT`      | Optional     |
| quotationTypes | `QuotationType[]`   | `SERVICE,PRODUCT` | Optional     |
| sourceTypes    | `SourceType[]`      | `FROM_LEAD`       | Optional     |
| branchIds      | `string[]`          | `BR-001,BR-002`   | Optional     |
| createdBy      | `string`            | `John Doe`        | Optional     |
| createdFrom    | `date`              | `2026-05-01`      | Optional     |
| createdTo      | `date`              | `2026-05-31`      | Optional     |
| amountMin      | `number`            | `1000`            | Optional     |
| amountMax      | `number`            | `50000`           | Optional     |
| search         | `string`            | `QT-2026`         | Optional     |
| pageNo         | `number`            | `0`               | Default `0`  |
| pageSize       | `number`            | `10`              | Default `10` |

### Notes

- Response payload is wrapped in the project’s `ResponseStructure` with pagination in `PaginationResponse`.

---

## 16.2 Create Quotation – Save Draft or Send

### Endpoint

```
POST {{baseUrl}}/api/v1/quotations
```

### Description

- If `saveDraft=true`, quotation is saved as **DRAFT** (no email).
- If `saveDraft=false`, quotation is saved as **SENT**, generates `publicToken`, sends PDF via email, and publishes a notification.

### Request Body (example)

```json
{
  "sourceType": "NEW_PROSPECT",
  "leadId": null,
  "customerId": null,
  "prospect": {
    "fullName": "John Doe",
    "phone": "9876543210",
    "email": "john@example.com",
    "companyName": "Acme",
    "address": "123 Tech Park",
    "city": "Bengaluru",
    "state": "KA",
    "pincode": "560001",
    "country": "India",
    "googleMapUrl": null
  },
  "quotationType": "COMBINED",
  "serviceMode": "CONTRACT",
  "contractFrequency": "MONTHLY",
  "contractDuration": "ONE_YEAR",
  "contractProposedStart": "2026-05-10",
  "locations": [
    {
      "address": "Site A, Andheri",
      "city": "Mumbai",
      "state": "MH",
      "country": "India",
      "pincode": "400001",
      "googleMapUrl": null,
      "locationCategory": "COMMERCIAL",
      "locationSubCategory": "INTERNAL",
      "areaSqft": 1200,
      "branchId": "BRANCH-001",
      "displayOrder": 1,
      "serviceLines": [
        {
          "serviceId": "SVC-001",
          "priceType": "FIXED",
          "fixedTierName": "3 BHK",
          "basePrice": null,
          "pricePerSqft": null,
          "areaSqftUsed": null,
          "ratePerVisit": 5000,
          "visitFrequency": "MONTHLY",
          "totalVisits": 12,
          "displayOrder": 1
        }
      ]
    }
  ],
  "productLines": [
    {
      "productId": "PROD-001",
      "unitPrice": 1200,
      "quantity": 5,
      "displayOrder": 1
    }
  ],
  "discountType": "PERCENTAGE",
  "discountValue": 10,
  "validTill": "2026-05-30",
  "paymentTerms": "NET_30",
  "customPaymentTerms": null,
  "specialTerms": "Standard terms apply.",
  "internalNotes": "Internal note here.",
  "saveDraft": false,
  "attachments": [
    {
      "filename": "scope_of_work.pdf",
      "contenttype": "application/pdf",
      "filedata": "BASE64_HERE",
      "notes": "Scope of work"
    }
  ]
}
```

### Validation rules (high-signal)

- **Source**:
  - `FROM_LEAD` requires `leadId`
  - `FROM_CUSTOMER` requires `customerId`
  - `NEW_PROSPECT` requires `prospect`
- **When sending (`saveDraft=false`)**:
  - If `quotationType != PRODUCT`, `locations` must be non-empty
  - If `serviceMode=CONTRACT`, contract fields must be present
  - If `paymentTerms=CUSTOM`, `customPaymentTerms` must be present
- **Attachments**:
  - Max **5** per quotation
  - Allowed types: `application/pdf`, `image/jpeg`, `image/jpg`, `image/png`
  - Max **5 MB** each (estimated from base64 length)

---

## 16.3 View Quotation Details

### Endpoint

```
GET {{baseUrl}}/api/v1/quotations/by-id?id={{quotationId}}
```

### Description

Returns a full quotation detail view including header, locations, service lines, product lines, attachments, and audit logs.

---

## 16.4 Delete (Soft-Delete) DRAFT Quotation

### Endpoint

```
DELETE {{baseUrl}}/api/v1/quotations?id={{quotationId}}
```

### Request Body

```json
{
  "deletionReason": "DUPLICATE_QUOTATION",
  "deletionReasonDetail": "Optional, required only when deletionReason=OTHER."
}
```

### Notes

- Only `status=DRAFT` can be deleted.

---

## 16.5 Update Status (Internal) – ACCEPTED/REJECTED

### Endpoint

```
PATCH {{baseUrl}}/api/v1/quotations/{{quotationId}}/status?status=ACCEPTED
```

### Query Parameters

| Parameter | Type              | Example                 | Description            |
| --------- | ----------------- | ----------------------- | ---------------------- |
| status    | `QuotationStatus` | `ACCEPTED` / `REJECTED` | Only these are allowed |

### Notes

- Allowed current statuses: `SENT`, `VIEWED`
- Sets timestamps (`acceptedAt` / `rejectedAt`) and writes audit log.

---

## 16.6 Send Quotation (DRAFT → SENT)

### Endpoint

```
POST {{baseUrl}}/api/v1/quotations/{{quotationId}}/send
```

### Notes

- Generates `publicToken` if missing.
- Sends PDF email (Brevo).

---

## 16.7 Resend Quotation

### Endpoint

```
POST {{baseUrl}}/api/v1/quotations/{{quotationId}}/resend
```

### Notes

- Not allowed in statuses: `DRAFT`, `ACCEPTED`, `REJECTED`, `EXPIRED`

---

## 16.8 Download Quotation PDF

### Endpoint

```
GET {{baseUrl}}/api/v1/quotations/{{quotationId}}/pdf
```

### Description

Returns a binary PDF attachment generated via Thymeleaf template (`quotation-template.html`).

---

## 16.9 Upload Attachments (Base64)

### Endpoint

```
POST {{baseUrl}}/api/v1/quotations/{{quotationId}}/attachments
```

### Request Body

```json
[
  {
    "filename": "site_survey.pdf",
    "contenttype": "application/pdf",
    "filedata": "BASE64_HERE",
    "notes": "Optional notes"
  }
]
```

---

## 16.10 Dropdowns (Create Form)

Base path:

```
GET {{baseUrl}}/api/v1/quotations/dropdowns/*
```

### 16.10.1 Qualified Leads

```
GET {{baseUrl}}/api/v1/quotations/dropdowns/leads?search={{keyword}}
```

- Filters leads with statuses: `QUALIFIED`, `QUOTATION_SENT`, `NEGOTIATION`
- Limit: 20

### 16.10.2 Active Services

```
GET {{baseUrl}}/api/v1/quotations/dropdowns/services?search={{keyword}}
```

- Limit: 50

### 16.10.3 Active Products

```
GET {{baseUrl}}/api/v1/quotations/dropdowns/products?search={{keyword}}
```

- Limit: 50

---

## 16.11 Public (Client) Endpoints

> These endpoints are **unauthenticated** (no JWT), but still require `X-Tenant-ID`.

### 16.11.1 Public View (SENT → VIEWED)

#### Endpoint

```
GET {{baseUrl}}/api/v1/quotations/public/{{quotationId}}?token={{publicToken}}
```

#### Behavior

- Validates the `token` against `quotations.public_token`
- If current status is `SENT`, updates to `VIEWED` and sets `viewedAt`

---

### 16.11.2 Client Decision (ACCEPT/REJECT)

#### Endpoint

```
POST {{baseUrl}}/api/v1/quotations/public/{{quotationId}}/decision?token={{publicToken}}&decision=ACCEPT
```

#### Query Parameters

| Parameter | Type   | Example             | Description |
| --------- | ------ | ------------------- | ----------- |
| token     | string | `...`               | Required    |
| decision  | string | `ACCEPT` / `REJECT` | Required    |

#### Allowed current statuses

- `SENT`, `VIEWED`

#### Effects

- Sets `acceptedAt` / `rejectedAt`
- Writes audit log event: `ACCEPTED` / `REJECTED`
- If `sourceType=FROM_LEAD` and accepted: Lead status → `CONVERTED`

---

## Reference – Enums

### QuotationStatus

- `DRAFT`, `SENT`, `VIEWED`, `ACCEPTED`, `REJECTED`, `EXPIRED`, `REVISED`

### QuotationSourceType

- `FROM_LEAD`, `FROM_CUSTOMER`, `NEW_PROSPECT`

### QuotationTypeEnum

- `SERVICE`, `PRODUCT`, `COMBINED`

### PaymentTermsEnum

- `FULL_ADVANCE`, `FIFTY_ADVANCE_FIFTY_COMPLETION`, `NET_15`, `NET_30`, `CUSTOM`

### DeletionReasonEnum

- `CREATED_BY_MISTAKE`, `DUPLICATE_QUOTATION`, `CLIENT_WITHDREW_INTEREST`, `PRICING_ERROR`, `OTHER`

---

**End of Documentation**
