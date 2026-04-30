# 🚀 Frontend Integration Guide: Notifications & Exports

This document provides a comprehensive guide for frontend developers to seamlessly integrate the **ERP Notification System (REST + WebSocket)** and the **Export/Download APIs** module by module.

---

## 1. 🔔 Notification System Integration

The notification system relies on a hybrid approach: **WebSocket (STOMP)** for real-time delivery and **REST APIs** for fetching historical feeds and managing read states.

### 1.1 WebSocket (Real-Time In-App Alerts)

To receive real-time notifications, the frontend must establish a WebSocket connection and subscribe to the user-specific topic.

- **Connection URL**: `wss://api.seraviontechnologies.com/ws`
- **Protocol**: STOMP over WebSocket
- **Authentication**: Pass the JWT token in the `Authorization` header during the STOMP `CONNECT` frame.

**Connection Steps (Example using `@stomp/stompjs`):**

```javascript
import { Client } from "@stomp/stompjs";

const client = new Client({
  brokerURL: "wss://api.seraviontechnologies.com/ws",
  connectHeaders: {
    Authorization: `Bearer ${yourJwtToken}`,
  },
  onConnect: () => {
    console.log("Connected to WebSocket");

    // Subscribe to the specific user's notification channel
    client.subscribe(`/topic/notifications/${userId}`, (message) => {
      const notification = JSON.parse(message.body);
      console.log("New Notification:", notification);
      // Trigger Toast / Update Badge Count
    });
  },
});
client.activate();
```

**Incoming Payload Structure:**

```json
{
  "event_type": "INVOICE_OVERDUE",
  "module": 28,
  "entity_id": "INV-2026-00142",
  "title": "Invoice Overdue",
  "message": "⚠️ Invoice INV-2026-00142 overdue. Outstanding: ₹28,037.",
  "priority": "HIGH",
  "timestamp": "2026-04-24T12:00:00Z",
  "action_url": "/invoices/INV-2026-00142"
}
```

### 1.2 REST APIs (Notification Feed & Management)

Use these endpoints for the Bell Icon drop-down and notification management.

| Action            | HTTP Method | Endpoint                                                      | Description                                                     |
| ----------------- | ----------- | ------------------------------------------------------------- | --------------------------------------------------------------- |
| **Get Feed**      | `GET`       | `/api/v1/notifications?unreadOnly=false&pageNo=0&pageSize=20` | Fetch paginated notifications. Set `unreadOnly=true` to filter. |
| **Get Latest**    | `GET`       | `/api/v1/notifications/latest`                                | Fetch the single most recent notification.                      |
| **Unread Count**  | `GET`       | `/api/v1/notifications/unread-count`                          | Fetch the integer count for the Bell Icon badge.                |
| **Mark Read**     | `POST`      | `/api/v1/notifications/{id}/read`                             | Mark a specific notification as read.                           |
| **Mark All Read** | `POST`      | `/api/v1/notifications/read-all`                              | Mark all unread notifications for the user as read.             |

> **Note**: For testing, there is an internal trigger endpoint `POST /api/v1/notifications/internal/publish` (Requires `CEO` role).

---

## 2. 📥 Module-Wise Export & Download APIs

The following are the specific REST endpoints for downloading PDFs and exporting Excel/CSV files, mapped by module. All endpoints require standard JWT Bearer authentication.

### Module 11: Stock Management

- **Download Supplier Invoice (File Attachment)**: `GET /api/v1/stock/central-entries/invoice-copy?entryId={id}`

### Module 14: Purchase Orders

- **Download PO PDF**: `GET /v1/purchase-orders/{id}/pdf`

### Module 15: Lead Management

- **Download Lead PDF**: `GET /v1/leads/{id}/pdf`

### Module 16: Quotation Management

- **Download Quotation PDF**: `GET /v1/quotations/{id}/pdf`
- **Action**: External Send (Email/WA) is triggered by `POST /v1/quotations/{id}/send` and `POST /v1/quotations/{id}/resend`.

### Module 17: Gross Margin Analysis (GMA)

- **Download GMA PDF**: `GET /v1/gma/{id}/pdf`

### Module 18: Customer Management

- **Export Service History (Excel)**: `GET /v1/customers/{id}/service-history/export`

### Module 19: Contract Management

- **Download Contract PDF**: `GET /v1/contracts/{id}/pdf`
- **Download Service Log (Excel)**: `GET /v1/contracts/{id}/execution-log/export`

### Module 20: Sales Orders

- **Download SO PDF**: `GET /v1/sales-orders/{id}/pdf`

### Module 21: Task Management

- **Download Task Print/PDF**: `GET /v1/tasks/{id}/pdf`

### Module 22: Support Tickets

- **Download Ticket Print/PDF**: `GET /v1/tickets/{id}/pdf`

### Module 24: Petty Cash

- **Export All Expenses (Excel)**: `GET /v1/petty-cash/export`
- **Export Received Requests (Excel)**: `GET /v1/petty-cash/received/export`

### Module 25: HRM (Salary & Attendance)

- **Download Salary Slip (Paid only)**: `GET /v1/hrm/payslips/{emp}/{month}/pdf`
- **Download Salary Template**: `GET /v1/hrm/salary/sample-template`
- **Download Attendance Template**: `GET /v1/hrm/attendance/sample-template`

### Module 26: Employee Self-Service

- **Download Uploaded Document**: `GET /v1/employees/{id}/documents/{doc}/download`

### Module 28: Invoicing (Sales)

- **Download Invoice PDF**: `GET /v1/invoices/{id}/pdf`
- **Download Payment Receipt (If Paid)**: `GET /v1/payments/receipts/{id}/pdf` _(routes to Module 30)_
- **Export PDF Batch (ZIP)**: `POST /v1/invoices/batch-pdf`
- **Export Tally Data**: `GET /v1/invoices/tally-export`
- **Action**: External Send (Approve & Send) via `POST /v1/invoices/{id}/approve-send`.

### Module 29: Bills (Purchases)

- **Download Bill PDF**: `GET /v1/bills/{id}/pdf`
- **Download Payment Voucher (If Paid)**: `GET /v1/payments/vouchers/{id}/pdf` _(routes to Module 30)_

### Module 30: Payments (Receipts & Vouchers)

- **Download Voucher/Receipt PDF**: `GET /v1/payments/{id}/pdf`

### Module 31: Ledger Management

- **Export All Ledgers**: `GET /v1/ledgers/export`
- **Download Ageing Report**: `GET /v1/ledgers/ageing-report`
- **Download Ledger Statement PDF**: `GET /v1/ledgers/{id}/statement/pdf`
- **Action**: Email Statement to Party via `POST /v1/ledgers/{id}/statement/email`.

### Module 32: Chart of Accounts (COA)

- **Export COA (CSV)**: `GET /v1/coa/export`

---

## 3. 📧 External Email Sends (User-Triggered)

For actions where the user explicitly sends an email/document to an external party (Customer/Vendor):

- **Send Quotation** (Mod 16): `POST /v1/quotations/{id}/send`
- **Approve & Send Invoice** (Mod 28): `POST /v1/invoices/{id}/approve-send`
- **Email Ledger Statement** (Mod 31): `POST /v1/ledgers/{id}/statement/email`
- **Email Payslip** (Mod 25): `POST /v1/hrm/payslips/{id}/email`
  _(These do not trigger In-App WebSocket notifications.)_
