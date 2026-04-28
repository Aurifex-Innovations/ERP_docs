# Module 23: Customer Support Management (SLA-Driven)

## Table of Contents

- [23.1 Ticket Dashboard (SLA-Driven)](#231-ticket-dashboard-sla-driven)
- [23.2 Raise New Ticket](#232-raise-new-ticket)
- [23.3 Ticket Detail View](#233-ticket-detail-view)
- [23.4 Assign / Reassign Ticket (Modal)](#234-assign--reassign-ticket-modal)
- [23.5 Convert Ticket to Task (Modal / Flow)](#235-convert-ticket-to-task-modal--flow)
- [23.6 Ticket Resolution (Modal)](#236-ticket-resolution-modal)
- [23.7 Close Ticket (Modal)](#237-close-ticket-modal)
- [Extra APIs](#extra-apis)

---

## 23.1 Ticket Dashboard (SLA-Driven)

### Endpoint

```
GET {{baseUrl}}/api/v1/support/tickets
```

### Query Parameters

| Parameter       | Type    | Example      | Description                        |
| --------------- | ------- | ------------ | ---------------------------------- |
| branchId        | string  | {{branchId}} | Branch ID filter                   |
| createdFrom     | date    | 2026-01-01   | Start date filter                  |
| createdTo       | date    | 2026-12-31   | End date filter                    |
| status          | string  | OPEN         | Ticket status (OPEN, CLOSED, etc.) |
| priority        | string  | HIGH         | Priority level (HIGH, NORMAL, LOW) |
| slaHealth       | string  | SAFE         | SLA health status                  |
| escalationLevel | string  | L1           | Escalation level (L1, L2, L3)      |
| search          | string  | dummy        | Search query                       |
| pageNo          | integer | 0            | Page number                        |
| pageSize        | integer | 10           | Number of items per page           |

---

## 23.2 Raise New Ticket

### Endpoint

```
POST {{baseUrl}}/api/v1/support/tickets
```

### Request Body

```json
{
  "customerId": "{{customerId}}",
  "salesOrderId": null,
  "relatedTaskId": null,
  "reportedByName": "Suresh Kumar",
  "reportedByPhone": "+919876543210",
  "ticketTypeId": "STT-REEM",
  "priority": "NORMAL",
  "subject": "Postman dummy — re-emergence in lobby",
  "description": "Dummy description. Max 1000 chars in app validation. Replace customerId with a real tenant customer id.",
  "expectedResolutionDate": "2026-12-31",
  "expectedResolutionTime": "17:00:00",
  "attachments": [
    {
      "filename": "",
      "contentType": "",
      "fileData": ""
    }
  ]
}
```

---

## 23.3 Ticket Detail View

### Endpoint

```
GET {{baseUrl}}/api/v1/support/tickets/by-id?ticketRef={{ticketRef}}
```

### Query Parameters

| Parameter | Type   | Example       | Description             |
| --------- | ------ | ------------- | ----------------------- |
| ticketRef | string | {{ticketRef}} | Ticket reference number |

---

## 23.4 Assign / Reassign Ticket (Modal)

### Endpoint

```
POST {{baseUrl}}/api/v1/support/tickets/{{ticketRef}}/assign
```

### Request Body

```json
{
  "assigneeRoleCode": "SUPPORT_AGENT",
  "assignToUserId": "{{assignUserId}}",
  "assignmentNote": "Dummy handover from Postman — customer escalated."
}
```

---

## 23.5 Convert Ticket to Task (Modal / Flow)

For the complete Re-Task form layout, field validations, and technician assignment logic — see Module 21 → Screen 21.3 → Tab 2.

---

## 23.6 Ticket Resolution (Modal)

### Mark Resolved

#### Endpoint

```
POST {{baseUrl}}/api/v1/support/tickets/{{ticketRef}}/resolve
```

#### Request Body

```json
{
  "resolutionCode": "SERVICE_RESOLVED_SUCCESS",
  "resolutionNotes": "Technician completed re-visit; customer confirmed resolution.",
  "customerRating": 5,
  "customerFeedback": "Very satisfied with response time."
}
```

---

## 23.7 Close Ticket (Modal)

### Endpoint

```
POST {{baseUrl}}/api/v1/support/tickets/{{ticketRef}}/close
```

### Request Body

```json
{
  "closeReason": "RESOLVED_CUSTOMER_SATISFACTION",
  "closureRemarks": "Final QA closure — dummy Postman request."
}
```

---

## Extra APIs

### Add Note / Reply

#### Endpoint

```
POST {{baseUrl}}/api/v1/support/tickets/{{ticketRef}}/notes
```

#### Request Body

```json
{
  "note": "Called customer; follow-up scheduled for tomorrow.",
  "internal": false,
  "customerVisibleReply": true
}
```

---

### Convert to Task

#### Endpoint

```
POST {{baseUrl}}/api/v1/support/tickets/{{ticketRef}}/in-progress
```

---

### Pause Ticket

#### Endpoint

```
POST {{baseUrl}}/api/v1/support/tickets/{{ticketRef}}/pause
```

---

## Field Descriptions

### Ticket Creation Fields

| Field                  | Type   | Description                             |
| ---------------------- | ------ | --------------------------------------- |
| customerId             | string | Customer identifier (required)          |
| salesOrderId           | string | Related sales order ID (optional)       |
| relatedTaskId          | string | Related task ID (optional)              |
| reportedByName         | string | Name of person reporting the issue      |
| reportedByPhone        | string | Contact phone number                    |
| ticketTypeId           | string | Ticket type identifier (e.g., STT-REEM) |
| priority               | string | NORMAL, HIGH, LOW, URGENT               |
| subject                | string | Brief subject line for the ticket       |
| description            | string | Detailed description (max 1000 chars)   |
| expectedResolutionDate | date   | Expected resolution date (YYYY-MM-DD)   |
| expectedResolutionTime | time   | Expected resolution time (HH:mm:ss)     |

### Attachment Fields

| Field       | Type   | Description               |
| ----------- | ------ | ------------------------- |
| filename    | string | Name of the attached file |
| contentType | string | MIME type of the file     |
| fileData    | string | Base64 encoded file data  |

### Assignment Fields

| Field            | Type   | Description                         |
| ---------------- | ------ | ----------------------------------- |
| assigneeRoleCode | string | Role code (e.g., SUPPORT_AGENT)     |
| assignToUserId   | string | User ID to assign the ticket to     |
| assignmentNote   | string | Notes about the assignment/handover |

### Resolution Fields

| Field            | Type    | Description                        |
| ---------------- | ------- | ---------------------------------- |
| resolutionCode   | string  | Code indicating resolution type    |
| resolutionNotes  | string  | Detailed resolution notes          |
| customerRating   | integer | Customer satisfaction rating (1-5) |
| customerFeedback | string  | Customer feedback text             |

### Closure Fields

| Field          | Type   | Description                   |
| -------------- | ------ | ----------------------------- |
| closeReason    | string | Reason for closing the ticket |
| closureRemarks | string | Final closure remarks         |

### Note/Reply Fields

| Field                | Type    | Description                          |
| -------------------- | ------- | ------------------------------------ |
| note                 | string  | Note or reply content                |
| internal             | boolean | Whether note is internal only        |
| customerVisibleReply | boolean | Whether reply is visible to customer |

---

## SLA Health Status Reference

| Status   | Description                 |
| -------- | --------------------------- |
| SAFE     | Ticket within SLA timelines |
| WARNING  | Approaching SLA breach      |
| BREACHED | SLA timeline exceeded       |

---

## Priority Levels

| Priority | Description                               |
| -------- | ----------------------------------------- |
| LOW      | Low priority issue                        |
| NORMAL   | Standard priority                         |
| HIGH     | High priority requiring attention         |
| URGENT   | Critical issue requiring immediate action |

---

## Escalation Levels

| Level | Description                     |
| ----- | ------------------------------- |
| L1    | First level support             |
| L2    | Second level escalation         |
| L3    | Third level / senior escalation |

---

## Ticket Status Flow

```
NEW → ASSIGNED → IN_PROGRESS → RESOLVED → CLOSED
                       ↓
                    PAUSED
```

### Status Descriptions

| Status      | Description                            |
| ----------- | -------------------------------------- |
| NEW         | Ticket newly created, not yet assigned |
| ASSIGNED    | Ticket assigned to support agent       |
| IN_PROGRESS | Ticket being actively worked on        |
| PAUSED      | Ticket temporarily paused              |
| RESOLVED    | Issue resolved, pending closure        |
| CLOSED      | Ticket fully closed                    |

---

## Resolution Codes Reference

| Code                     | Description                      |
| ------------------------ | -------------------------------- |
| SERVICE_RESOLVED_SUCCESS | Service completed successfully   |
| CUSTOMER_SATISFIED       | Customer confirmed satisfaction  |
| WORKAROUND_PROVIDED      | Temporary workaround implemented |
| DUPLICATE_TICKET         | Duplicate of another ticket      |

---

## Close Reasons Reference

| Reason                         | Description                         |
| ------------------------------ | ----------------------------------- |
| RESOLVED_CUSTOMER_SATISFACTION | Resolved with customer satisfaction |
| DUPLICATE                      | Duplicate ticket                    |
| INVALID_REQUEST                | Invalid or inappropriate request    |
| ABANDONED                      | Customer did not respond/abandoned  |

---

## Workflow Notes

### Ticket Lifecycle

1. **Raise Ticket** (23.2) - Create new support ticket
2. **Assign** (23.4) - Assign to support agent or reassign
3. **Add Notes** (Extra API) - Track progress and communication
4. **Pause** (Extra API) - Temporarily pause if waiting for information
5. **Convert to Task** (23.5 / Extra API) - Create service task if needed
6. **Resolve** (23.6) - Mark ticket as resolved
7. **Close** (23.7) - Final closure after verification

### SLA Monitoring

- Tickets are tracked against SLA timelines
- `slaHealth` parameter indicates current SLA status
- Escalation levels help prioritize overdue tickets
- Real-time dashboard shows SLA compliance metrics

### Customer Communication

- Notes can be marked as customer-visible
- Internal notes for team communication only
- Resolution includes customer rating and feedback
- Expected resolution date/time communicated to customer

---

**End of Documentation**
