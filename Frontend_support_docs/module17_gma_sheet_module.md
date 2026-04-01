# Module 17: Gross Margin Analysis (GMA) Sheet

The **Gross Margin Analysis (GMA) Sheet** module is a strategic pricing and feasibility tool designed to evaluate the profitability of service contracts before they are formalized. It calculates the Gross Margin (GM) by subtracting all service-related costs (manpower, chemicals, and visit costs) from the proposed annual sale price.

Frontend developers will use these APIs to build the three-tabbed management interface:

1.  **GMA Entries**: Master list of all analysis sheets.
2.  **My Requests**: Sheets created by the logged-in user, tracking submission/approval status.
3.  **Received Requests**: Workflow dashboard for managers to approve or reject sheets routed to them (triggered when GM < 40%).

---

## Authorization

| Type                     | Requirement                                                                                                                                                                                                                                                                                                    |
| :----------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Authentication**       | Bearer Token (JWT) required in `Authorization` header.                                                                                                                                                                                                                                                         |
| **Required Roles**       | `CEO` (Full Access)                                                                                                                                                                                                                                                                                            |
| **Required Permissions** | `GMA_SHEET_MANAGEMENT_READ` - Tab 1 access<br>`GMA_SHEET_MANAGEMENT_REQUEST` - Tab 2 access<br>`GMA_SHEET_MANAGEMENT_APPROVE` - Tab 3 access<br>`GMA_SHEET_MANAGEMENT_ADD` - Create/Edit sheets<br>`GMA_SHEET_MANAGEMENT_DELETE` - Revoke pending requests<br>`GMA_SHEET_MANAGEMENT_DOWNLOAD` - PDF generation |
| **Required Headers**     | `Authorization: Bearer <token>`<br>`X-Tenant-ID: <tenant_id>`                                                                                                                                                                                                                                                  |
| **Business Logic**       | 1. If GM **≥ 40%**, the sheet is auto-approved upon submission.<br>2. If GM **< 40%**, the status becomes `PENDING` and requires manager approval.                                                                                                                                                             |

---

## Enums Used In This Module

### GmaStatus

Possible lifecycle states of a GMA Sheet.
| Value | Meaning | Used In |
| :--- | :--- | :--- |
| `DRAFT` | Initial state; user is still editing. No logic applied. | List, Details, Create |
| `PENDING` | Submitted; waiting for manual approval (GM < 40%). | List, Details, Approval |
| `APPROVED` | Finalized; ready for quotation conversion. | List, Details, PDF |
| `REJECTED` | Declined by manager; requires correction or closure. | List, Details, Audit |

### GmaSourceType

Source of the prospect data.
| Value | Meaning |
| :--- | :--- |
| `FROM_LEAD` | Data pulled from Module 15 (Leads). |
| `FROM_CUSTOMER` | Data pulled from Module 3 (Customers). |
| `ADD_NEW` | Manual entry for a new prospect. |

### GmaSaveAction

Directive to the backend on how to process the submission.
| Value | Meaning |
| :--- | :--- |
| `DRAFT` | Save without status transition/validation. |
| `REQUEST_APPROVAL` | Save and force `PENDING` status regardless of GM%. |
| `SUBMIT` | Run auto-approval logic (Approved if GM ≥ 40%). |

### GmaFrequency

Service visit regularity (Annual Multiplier).
| Value | Annual Count | Frontend Note |
| :--- | :--- | :--- |
| `WEEKLY` | 52 | Auto-multiplier |
| `FORTNIGHTLY` | 26 | Auto-multiplier |
| `MONTHLY` | 12 | Auto-multiplier |
| `QUARTERLY` | 4 | Auto-multiplier |
| `CUSTOM` | User Defined | Must provide `annualFrequency` field |

### GmaServiceMode

| Value      | Meaning                      |
| :--------- | :--------------------------- |
| `CONTRACT` | Recurring service agreement. |
| `ONE_TIME` | Single transaction/service.  |

---

## API List

| Method | Endpoint                               | Purpose                                        | Authorization Required |
| :----- | :------------------------------------- | :--------------------------------------------- | :--------------------- |
| `GET`  | `/api/v1/gma/sheets`                   | Tab 1: Master list of all GMA sheets           | `READ`                 |
| `GET`  | `/api/v1/gma/sheets/my-requests`       | Tab 2: User-specific created requests          | `REQUEST`              |
| `GET`  | `/api/v1/gma/sheets/received-requests` | Tab 3: Incoming approval workflow              | `APPROVE`              |
| `GET`  | `/api/v1/gma/sheets/by-id`             | Detailed view of a specific sheet              | None (Generic)         |
| `POST` | `/api/v1/gma/sheets`                   | Create a new sheet or save a draft             | `ADD`                  |
| `PUT`  | `/api/v1/gma/sheets/{id}/decision`     | Approve/Reject a PENDING sheet                 | `APPROVE`              |
| `PUT`  | `/api/v1/gma/sheets/{id}/revoke`       | Revert a PENDING sheet back to DRAFT           | `DELETE`               |
| `GET`  | `/api/v1/gma/sheets/eligible-managers` | Get roles allowed to approve low-margin sheets | None                   |
| `GET`  | `/api/v1/gma/sheets/{id}/pdf`          | Download the analysis report                   | `DOWNLOAD`             |

---

## API Details

### [POST] /api/v1/gma/sheets

**Purpose**: Creates or updates a GMA Sheet. Handles drafts, auto-approvals, and manual review triggers.

**Authorization**: `GMA_SHEET_MANAGEMENT_ADD` or `CEO`.

**Request Body Fields**:
| Field | Type | Required | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| `sourceType` | Enum | Yes | `FROM_LEAD`, `FROM_CUSTOMER`, `ADD_NEW` | `ADD_NEW` |
| `leadId` | String | No* | Required if `sourceType` = `FROM_LEAD` | `"L-101"` |
| `prospect` | Object | No* | Required if `sourceType` = `ADD_NEW` | See below |
| `branchId` | String | Yes | Branch handling this analysis | `"B-DELHI"` |
| `proposedStartDate` | Date | Yes | Anticipated start date | `"2026-05-01"` |
| `action` | Enum | Yes | `DRAFT`, `REQUEST_APPROVAL`, `SUBMIT` | `SUBMIT` |
| `approverRoleIds` | List | No | Required if GM < 40% | `[10, 15]` |
| `sites` | List | Yes | At least one site required | See nested structure |

**GmaSiteRequest (Nested)**

- `siteName`: String (Required)
- `category`: `RESIDENTIAL`\|`COMMERCIAL`\|`INDUSTRIAL`
- `areaSqft`: Decimal (Required)
- `siteProposedPriceYear`: Decimal (The sale price set by sales person)
- `services`: List of Service objects

**GmaServiceRequest (Nested)**

- `serviceTypeId`: String (From Master Module)
- `serviceMode`: `CONTRACT`\|`ONE_TIME`
- `frequency`: Enum (Weekly, etc.)
- `ratePerVisit`: Decimal
- `hoursPerVisit`: Decimal
- `ratePerHour`: Decimal
- `chemicals`: List of Chemical objects

**Request JSON Examples**

#### Valid Complete Submission (Triggers Manual Approval if GM < 40%)

```json
{
  "sourceType": "ADD_NEW",
  "prospect": {
    "fullName": "Aryan Sharma",
    "phone": "9876543210",
    "email": "aryan@example.com",
    "address": "Skyline Heights, Tower B",
    "city": "Mumbai",
    "state": "Maharashtra",
    "pincode": "400051"
  },
  "branchId": "MUM-01",
  "proposedStartDate": "2026-06-01",
  "action": "SUBMIT",
  "approverRoleIds": [5],
  "sites": [
    {
      "siteName": "Corporate Office",
      "city": "Mumbai",
      "category": "COMMERCIAL",
      "subCategory": "INTERNAL",
      "areaSqft": 5000,
      "siteProposedPriceYear": 150000.0,
      "services": [
        {
          "serviceTypeId": "SRV-001",
          "serviceMode": "CONTRACT",
          "frequency": "MONTHLY",
          "ratePerVisit": 2500,
          "hoursPerVisit": 3.5,
          "ratePerHour": 400,
          "chemicals": [
            {
              "productId": "CHM-01",
              "pricePerUom": 800,
              "coverageSqft": 1000,
              "requiredQtyPerVisit": 0.5
            }
          ]
        }
      ]
    }
  ]
}
```

**Response**

- **Status**: `201 Created`
- **Body**: Full `GmaSheetDetailResponse` including calculated `overallGrossMargin`.

---

### [PUT] /api/v1/gma/sheets/{id}/decision

**Purpose**: Manager action to Approve or Reject a pending request.

**Authorization**: `GMA_SHEET_MANAGEMENT_APPROVE`.

**Request Body**:

```json
{
  "decision": "APPROVE",
  "remarks": "Margins acceptable given the client profile."
}
```

---

### [GET] /api/v1/gma/sheets/by-id

**Purpose**: Fetch single sheet detail with all cost breakdowns.

**Response Fields**:

- `totalAnnualCost`: Sum of all site/service costs.
- `totalAnnualPrice`: Sum of all site proposed prices.
- `overallGrossMargin`: Calculated profit percentage.
- `gmWithoutDoc`: GM excluding documentation costs.
- `auditLogs`: List of actions taken (SUBMIT, APPROVE, etc.).

---

## Exceptions / Error Cases

| HTTP Status | Reason       | Typical Message                           | Frontend Note                    |
| :---------- | :----------- | :---------------------------------------- | :------------------------------- |
| `400`       | Validation   | "Proposed start date is required"         | Highlight missing fields in Red. |
| `403`       | Unauthorized | "Access Denied"                           | Hide "Delete/Approve" buttons.   |
| `404`       | Not Found    | "GMA sheet not found"                     | Redirect to list view.           |
| `422`       | Logic Error  | "Cannot submit without at least one site" | Prevent submission at UI Level.  |

---

## Frontend Integration Notes

1.  **Status Badges**:
    - `DRAFT`: Gray
    - `PENDING`: Orange
    - `APPROVED`: Green
    - `REJECTED`: Red
2.  **Calculation Logic**:
    - `Total Site Cost` = (Visit Cost + Manpower Cost + Chemical Cost + Surcharges) \* Annual Frequency.
    - `Site GM%` = `((Price - Cost) / Price) * 100`.
3.  **UI Interlock**:
    - When `action` is `SUBMIT` and `overallGrossMargin` is calculated to be **< 40%**, the frontend **MUST** force the user to select at least one `approverRole`.
4.  **Date Formatting**:
    - Requests: `YYYY-MM-DD`.
    - Responses (Audit): `ISO-8601` (Instant).
5.  **Multi-Step Form**:
    - Break down the creation into: 1. Client Info -> 2. Sites -> 3. Services -> 4. Review & Submit.

---

## Validation and Exception Summary

| Field / Scenario     | Rule             | Error Type | Impact                          |
| :------------------- | :--------------- | :--------- | :------------------------------ |
| `areaSqft`           | Must be > 0      | Validation | Prevents site addition.         |
| `proposedStartDate`  | Future or Today  | Validation | Prevents creation.              |
| `overallGrossMargin` | < 40%            | Workflow   | Requires Approval Roles.        |
| `revocation`         | Only for PENDING | Logic      | Revoke button hidden otherwise. |

---

## cURL Example

```bash
curl -X POST "https://api.yourdomain.com/api/v1/gma/sheets" \
     -H "Authorization: Bearer <JWT_TOKEN>" \
     -H "Content-Type: application/json" \
     -d '{
           "sourceType": "FROM_LEAD",
           "leadId": "LD-SH-992",
           "branchId": "BR-MUM",
           "proposedStartDate": "2026-05-20",
           "action": "DRAFT",
           "sites": []
         }'
```
