# Module 24: Petty Cash Management

## Table of Contents

- [24.1 Tab 1: All Expenses](#241-tab-1-all-expenses)
- [24.2 Tab 2: My Requests](#242-tab-2-my-requests)
  - [24.2.1 Add Petty Cash Request](#2421-add-petty-cash-request)
    - [24.2.1.1 Select Recipients for Petty Cash Request (Popup)](#24211-select-recipients-for-petty-cash-request-popup)
  - [24.2.2 View My Request (Read-Only Detail)](#2422-view-my-request-read-only-detail)
- [24.3 Tab 3: Received Requests](#243-tab-3-received-requests)
  - [24.3.1 Request Review & Approval Form](#2431-request-review--approval-form)
  - [24.3.2 Payment Processing Form](#2432-payment-processing-form)
- [Extra APIs](#extra-apis)

---

## 24.1 Tab 1: All Expenses

### Endpoint

```
GET {{baseUrl}}/api/v1/petty-cash/expenses
```

### Query Parameters

| Parameter      | Type    | Example      | Description              |
| -------------- | ------- | ------------ | ------------------------ |
| statuses       | string  | PENDING      | Expense status filter    |
| branchId       | string  | {{branchId}} | Branch ID filter         |
| category       | string  | FUEL         | Expense category         |
| employeeUserId | string  | -            | Employee user ID filter  |
| from           | date    | 2026-01-01   | Start date filter        |
| to             | date    | 2026-12-31   | End date filter          |
| minAmount      | number  | -            | Minimum amount filter    |
| maxAmount      | number  | -            | Maximum amount filter    |
| q              | string  | -            | Search query             |
| pageNo         | integer | 0            | Page number              |
| pageSize       | integer | 10           | Number of items per page |

---

## 24.2 Tab 2: My Requests

### Endpoint

```
GET {{baseUrl}}/api/v1/petty-cash/requests/my/list
```

### Query Parameters

| Parameter | Type    | Example          | Description              |
| --------- | ------- | ---------------- | ------------------------ |
| statuses  | string  | DRAFT            | Request status filter    |
| branchId  | string  | {{branchId}}     | Branch ID filter         |
| category  | string  | LOCAL_CONVEYANCE | Expense category         |
| from      | date    | 2026-01-01       | Start date filter        |
| to        | date    | 2026-12-31       | End date filter          |
| minAmount | number  | -                | Minimum amount filter    |
| maxAmount | number  | -                | Maximum amount filter    |
| q         | string  | -                | Search query             |
| pageNo    | integer | 0                | Page number              |
| pageSize  | integer | 10               | Number of items per page |

---

### 24.2.1 Add Petty Cash Request

#### Endpoint

```
POST {{baseUrl}}/api/v1/petty-cash/requests
```

#### Request Body

```json
{
  "category": "FUEL",
  "expenseDateFrom": "2026-06-01",
  "expenseDateTo": "2026-06-01",
  "amount": 1500,
  "description": "Fuel for site visit",
  "relatedTaskRef": null,
  "relatedSoRef": null,
  "justificationNote": "Approved by manager verbally",
  "paymentModeRequested": "UPI",
  "bankAccountHolder": null,
  "bankName": null,
  "bankAccountNumber": null,
  "bankIfsc": null,
  "upiId": "employee@upi",
  "preApproved": false,
  "preApprovedByUserId": null,
  "approvalReference": null,
  "receipts": [],
  "requestRoles": [],
  "status": null
}
```

---

#### 24.2.1.1 Select Recipients for Petty Cash Request (Popup)

##### Endpoint

```
GET {{baseUrl}}/api/v1/petty-cash/requests/eligible-recipients
```

---

### 24.2.2 View My Request (Read-Only Detail)

#### Endpoint

```
GET {{baseUrl}}/api/v1/petty-cash/requests/by-id?id={{requestId}}
```

#### Query Parameters

| Parameter | Type   | Example       | Description        |
| --------- | ------ | ------------- | ------------------ |
| id        | string | {{requestId}} | Request identifier |

---

## 24.3 Tab 3: Received Requests

### Endpoint

```
GET {{baseUrl}}/api/v1/petty-cash/requests/received
```

### Query Parameters

| Parameter | Type    | Example         | Description              |
| --------- | ------- | --------------- | ------------------------ |
| segment   | string  | pendingApproval | Request segment filter   |
| statuses  | string  | PENDING         | Request status filter    |
| pageNo    | integer | 0               | Page number              |
| pageSize  | integer | 10              | Number of items per page |

---

### 24.3.1 Request Review & Approval Form

#### View Request Details

```
GET {{baseUrl}}/api/v1/petty-cash/requests/by-id?id={{requestId}}
```

#### Submit Decision

```
PUT {{baseUrl}}/api/v1/petty-cash/requests/{{requestId}}/decision
```

#### Request Body (For Approve)

```json
{
  "decision": "APPROVE",
  "approvedAmount": 1500,
  "remarks": "Approved as per policy"
}
```

#### Request Body (For Reject)

```json
{
  "decision": "REJECT",
  "rejectionReason": "INSUFFICIENT_DOCUMENTATION",
  "remarks": "Please upload clear receipt"
}
```

---

### 24.3.2 Payment Processing Form

#### Endpoint

```
PUT {{baseUrl}}/api/v1/petty-cash/requests/{{requestId}}/pay
```

#### Request Body

```json
{
  "paymentMode": "UPI",
  "transactionRef": "TXN98765432101234567890",
  "paymentDate": "2026-06-10",
  "financeRemarks": "Paid via company UPI",
  "paymentProof": []
}
```

---

## Extra APIs

### Download Attachment

#### Endpoint

```
GET {{baseUrl}}/api/v1/petty-cash/documents/download?attachmentId={{attachmentId}}
```

#### Query Parameters

| Parameter    | Type   | Example          | Description           |
| ------------ | ------ | ---------------- | --------------------- |
| attachmentId | string | {{attachmentId}} | Attachment identifier |

---

### Export Received Inbox (Excel)

#### Endpoint

```
GET {{baseUrl}}/api/v1/petty-cash/received/export?segment=pendingApproval
```

#### Query Parameters

| Parameter | Type   | Example         | Description            |
| --------- | ------ | --------------- | ---------------------- |
| segment   | string | pendingApproval | Request segment filter |

---

### Export All Expenses (Excel)

#### Endpoint

```
GET {{baseUrl}}/api/v1/petty-cash/export
```

#### Query Parameters

| Parameter | Type   | Example      | Description           |
| --------- | ------ | ------------ | --------------------- |
| statuses  | string | PENDING      | Expense status filter |
| branchId  | string | {{branchId}} | Branch ID filter      |
| from      | date   | 2026-01-01   | Start date filter     |
| to        | date   | 2026-12-31   | End date filter       |

---

### Revoke Request

#### Endpoint

```
PUT {{baseUrl}}/api/v1/petty-cash/requests/{{requestId}}/revoke
```

---

## Field Descriptions

### Request Creation Fields

| Field                | Type    | Description                                       |
| -------------------- | ------- | ------------------------------------------------- |
| category             | string  | Expense category (FUEL, LOCAL_CONVEYANCE, etc.)   |
| expenseDateFrom      | date    | Start date of expense period (YYYY-MM-DD)         |
| expenseDateTo        | date    | End date of expense period (YYYY-MM-DD)           |
| amount               | number  | Requested amount                                  |
| description          | string  | Description of the expense                        |
| relatedTaskRef       | string  | Related task reference (optional)                 |
| relatedSoRef         | string  | Related sales order reference (optional)          |
| justificationNote    | string  | Justification for the expense                     |
| paymentModeRequested | string  | Requested payment mode (UPI, BANK_TRANSFER, etc.) |
| bankAccountHolder    | string  | Bank account holder name (for bank transfer)      |
| bankName             | string  | Bank name (for bank transfer)                     |
| bankAccountNumber    | string  | Bank account number (for bank transfer)           |
| bankIfsc             | string  | Bank IFSC code (for bank transfer)                |
| upiId                | string  | UPI ID (for UPI payment)                          |
| preApproved          | boolean | Whether request is pre-approved                   |
| preApprovedByUserId  | string  | User ID of pre-approver (if applicable)           |
| approvalReference    | string  | Pre-approval reference number                     |
| receipts             | array   | Array of receipt attachments                      |
| requestRoles         | array   | Array of recipient roles                          |
| status               | string  | Request status (optional)                         |

### Approval Decision Fields

| Field           | Type   | Description                    |
| --------------- | ------ | ------------------------------ |
| decision        | string | APPROVE or REJECT              |
| approvedAmount  | number | Approved amount (for approval) |
| remarks         | string | Approval/rejection remarks     |
| rejectionReason | string | Reason code for rejection      |

### Payment Processing Fields

| Field          | Type   | Description                                          |
| -------------- | ------ | ---------------------------------------------------- |
| paymentMode    | string | Payment method used (UPI, BANK_TRANSFER, CASH, etc.) |
| transactionRef | string | Transaction reference number                         |
| paymentDate    | date   | Date of payment (YYYY-MM-DD)                         |
| financeRemarks | string | Finance team remarks                                 |
| paymentProof   | array  | Array of payment proof attachments                   |

---

## Expense Categories Reference

| Category          | Description                   |
| ----------------- | ----------------------------- |
| FUEL              | Fuel expenses for vehicles    |
| LOCAL_CONVEYANCE  | Local transportation costs    |
| FOOD_REFRESHMENTS | Food and refreshment expenses |
| OFFICE_SUPPLIES   | Office supply purchases       |
| COMMUNICATION     | Phone, internet charges       |
| TRAVEL            | Travel-related expenses       |
| MISCELLANEOUS     | Other expenses                |

---

## Request Status Flow

```
DRAFT → PENDING → APPROVED → PAID
           ↓
        REJECTED
           ↓
        REVOKED
```

### Status Descriptions

| Status   | Description                           |
| -------- | ------------------------------------- |
| DRAFT    | Request saved as draft, not submitted |
| PENDING  | Submitted, awaiting approval          |
| APPROVED | Approved by approver, pending payment |
| REJECTED | Rejected by approver                  |
| PAID     | Payment processed                     |
| REVOKED  | Request revoked by requester          |

---

## Payment Modes

| Mode          | Description          |
| ------------- | -------------------- |
| UPI           | UPI transfer         |
| BANK_TRANSFER | Direct bank transfer |
| CASH          | Cash payment         |
| CHEQUE        | Cheque payment       |

---

## Rejection Reasons Reference

| Reason Code                | Description                            |
| -------------------------- | -------------------------------------- |
| INSUFFICIENT_DOCUMENTATION | Missing or unclear receipts/documents  |
| EXCEEDS_POLICY_LIMIT       | Amount exceeds policy limits           |
| DUPLICATE_REQUEST          | Duplicate of existing request          |
| INVALID_CATEGORY           | Incorrect expense category             |
| NOT_ELIGIBLE               | Employee not eligible for this expense |
| INCOMPLETE_INFORMATION     | Missing required information           |

---

## Workflow Notes

### Request Lifecycle

1. **Create Request** (24.2.1) - Employee creates petty cash request
2. **Select Recipients** (24.2.1.1) - Choose approvers/finance team
3. **Submit** - Request moves to PENDING status
4. **Review & Approve** (24.3.1) - Approver reviews and approves/rejects
5. **Payment Processing** (24.3.2) - Finance processes payment
6. **Complete** - Request marked as PAID

### Alternative Flows

- **Revoke** - Employee can revoke request before approval
- **Reject** - Approver can reject with reason
- **Re-submit** - Employee can modify and re-submit rejected requests

### Segments

| Segment         | Description                            |
| --------------- | -------------------------------------- |
| pendingApproval | Requests waiting for approval decision |
| pendingPayment  | Approved requests waiting for payment  |
| completed       | Fully processed requests               |

---

## Access Control

### Tabs by Role

- **All Expenses (24.1)** - Finance/Admin view of all expenses
- **My Requests (24.2)** - Employee's own requests
- **Received Requests (24.3)** - Approver's inbox for review

### Permissions

- **Create Request** - All employees
- **Approve/Reject** - Designated approvers
- **Process Payment** - Finance team
- **View All Expenses** - Finance/Admin
- **Revoke Request** - Request creator (before approval)

---

## Attachment Guidelines

### Receipt Requirements

- Clear, readable images or PDFs
- Must show date, amount, vendor
- All receipts should be attached before submission
- Supported formats: JPG, PNG, PDF

### Payment Proof

- Transaction screenshot or receipt
- Bank statement entry
- UPI transaction confirmation
- Required for all payments

---

**End of Documentation**
