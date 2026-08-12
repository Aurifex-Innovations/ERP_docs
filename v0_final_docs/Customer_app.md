# Seravion Connect Customer ERP Application Analysis & API Listing

## 1. Executive Summary & System Overview

**Seravion Connect Customer** (`erp_app_customer`) is an enterprise-grade Flutter application engineered for customer portal interactions within facility management, commercial pest control, deep cleaning, and specialized maintenance services.

The application serves as a unified digital interface allowing enterprise clients to:
- Monitor active service contracts, SLA tiers, and multi-site service schedules.
- Access detailed service site directories with geolocation/Google Maps integrations.
- Track real-time service visit execution status, technician details, and timestamps.
- Download and inspect digital Service Reports (PDF) and Financial Invoices (PDF).
- Raise, track, and interact with Support Tickets with live SLA health monitoring.
- Receive cross-platform real-time push notifications via Firebase Cloud Messaging (FCM).

---

## 2. Technical Stack & Environment

| Technology / Component | Details / Version Specification |
| :--- | :--- |
| **Framework** | Flutter (SDK `^3.12.0`) |
| **Language** | Dart 3 |
| **Design System** | Material Design 3 (Custom Light & Dark Themes) |
| **Push Notifications** | `firebase_core` (^4.12.1), `firebase_messaging` (^16.4.3), `flutter_local_notifications` (^22.2.0) |
| **Device & Local Storage** | `shared_preferences` (^2.5.5), `device_info_plus` (^13.2.0), `path_provider` (^2.1.6) |
| **Permissions** | `permission_handler` (^12.0.3) |
| **Target Platforms** | Android, iOS, Web, macOS, Windows, Linux |
| **Production API Base URL** | `https://api.seravionconnect.com` |

---

## 3. Repository Directory Structure

```
erp_app_customer/
├── assets/
│   └── logo/
│       └── logo.png
├── docs/
│   ├── app_analysis_and_api_listing.md
│   └── module-system-docs.md
├── lib/
│   ├── main.dart
│   ├── firebase_options.dart
│   ├── core/
│   │   ├── api/
│   │   │   ├── api_client.dart          # HTTP client, token injection, status handling, mock fallback
│   │   │   ├── api_endpoints.dart       # Static endpoint paths & URI builders
│   │   │   └── api_exceptions.dart      # Custom network & HTTP exception classes
│   │   ├── constants/
│   │   │   ├── app_constants.dart       # Storage keys, app info, validation regexes
│   │   │   ├── app_colors.dart          # Hex color palette tokens
│   │   │   └── app_assets.dart          # Asset path definitions
│   │   ├── services/
│   │   │   └── notification_service.dart # FCM init, local notifications, device registration
│   │   ├── theme/
│   │   │   └── app_theme.dart           # Light and Dark M3 ThemeData specifications
│   │   ├── utils/
│   │   │   └── session_manager.dart     # Encapsulated SharedPreferences session management
│   │   └── widgets/
│   │       ├── global_header.dart       # Reusable top header widget
│   │       ├── global_footer.dart       # Reusable bottom navigation bar widget
│   │       └── fade_in_slide_up.dart    # Micro-animation transition wrapper
│   └── features/
│       ├── auth/                        # Mobile Login & OTP Verification screens
│       ├── dashboard/                   # Home KPI summary dashboard & policy screens
│       ├── contracts/                   # Active/Expired Contracts & detailed breakdowns
│       ├── sites/                       # Customer sites directory & site-level details
│       ├── visits/                      # Service visits schedule, site filter & PDF viewing
│       ├── tickets/                     # Support ticket creation, list & activity timelines
│       ├── notifications/              # Notifications feed & badge counter
│       └── feedback/                    # Customer feedback submission & history
└── test/                                # Widget & integration test suites
```

---

## 4. Feature Modules Deep Dive

### 4.1 Authentication Module (`lib/features/auth`)
- **[login_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/auth/presentation/screens/login_screen.dart)**: Entry screen for customer authentication. Accepts mobile phone input with regex validation (`phoneRegex`), requests a 6-digit OTP via the `/api/v1/mobile/auth/request-otp` API endpoint, and handles initial login state.
- **[verify_identity_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/auth/presentation/screens/verify_identity_screen.dart)**: Handles 6-digit OTP verification. Includes a 60-second countdown timer with resend capability. On successful verification against `/api/v1/mobile/auth/verify-otp`, persists the session auth token, account ID, and phone number via `SessionManager`, registers device FCM token, and navigates to `DashboardScreen`.

### 4.2 Dashboard Module (`lib/features/dashboard`)
- **[dashboard_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/dashboard/presentation/screens/dashboard_screen.dart)**: Primary app hub displaying:
  - KPI summary metric cards (Active Contracts, Completed Services, In-Progress Services, Purchased Products).
  - Quick-action shortcuts to Contracts, Sites, Visits, and Tickets.
  - Recent activity feed timeline.
  - Navigation footer (`GlobalFooter`) with quick route switches.
- **[privacy_policy_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/dashboard/presentation/screens/privacy_policy_screen.dart)** & **[terms_of_service_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/dashboard/presentation/screens/terms_of_service_screen.dart)**: Compliance and legal documentation screens accessible from settings/footer.

### 4.3 Contracts Module (`lib/features/contracts`)
- **[contracts_list_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/contracts/presentation/screens/contracts_list_screen.dart)**: Displays active and expired contracts. Includes search filtering by contract ID or customer name and badge indicators for contract status (`ACTIVE`, `EXPIRED`, `PENDING`).
- **[contract_details_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/contracts/presentation/screens/contract_details_screen.dart)**: Comprehensive breakdown of a specific contract:
  - High-level financials: Total sale value, duration, start date, end date, invoicing frequency, payment schedule type.
  - Interactive site list: Collapsible cards listing covered customer sites.
  - Nested service details: Service type names, annual visit frequencies (e.g. `302 visits/year`), preferred days (`MON, TUE`), preferred time slots (`MORNING`, `AFTERNOON`), and search filter across services.

### 4.4 Sites Module (`lib/features/sites`)
- **[site_directory_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/sites/presentation/screens/site_directory_screen.dart)**: Shows all registered customer sites, featuring site code, contact person name, mobile number, physical address, service count, and Google Maps direct links.
- **[site_detail_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/sites/presentation/screens/site_detail_screen.dart)**: Displays individual site health stats, attached services, and active contract associations.
- **[site_user_mode_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/sites/presentation/screens/site_user_mode_screen.dart)** & **[active_contracts_selection_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/sites/presentation/screens/active_contracts_selection_screen.dart)**: Allows enterprise multi-site managers to switch between site administrative contexts.

### 4.5 Service Visits & Reports Module (`lib/features/visits`)
- **[site_selection_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/visits/presentation/screens/site_selection_screen.dart)**: Intermediary filter allowing users to search and select a specific site to inspect its assigned service visit tasks.
- **[visits_list_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/visits/presentation/screens/visits_list_screen.dart)**: Filterable visit feed (All, Upcoming, In Progress, Completed, Cancelled) featuring search query support, technician assignment, scheduled start/end times, and direct action triggers.
- **[visit_details_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/visits/presentation/screens/visit_details_screen.dart)**: Detailed visit summary showing assigned technician (photo, name, mobile), execution timestamps, site address, and embedded PDF document launchers for:
  - **Service Visit Report PDF**: via `/api/v1/mobile/tasks/my/service-report/pdf?taskId={taskId}`
  - **Invoice PDF**: via `/api/v1/invoices/pdf?id={invoiceId}`

### 4.6 Support Tickets Module (`lib/features/tickets`)
- **[tickets_list_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/tickets/presentation/screens/tickets_list_screen.dart)**: Support ticket list with status filters (`OPEN`, `IN_PROGRESS`, `RESOLVED`), priority indicators (`HIGH`, `NORMAL`, `LOW`), SLA countdown timers, and SLA health indicators (`HEALTHY`, `AT RISK`, `BREACHED`).
- **[ticket_details_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/tickets/presentation/screens/ticket_details_screen.dart)**: Displays ticket activity feed timeline (replies from customer & support agents), SLA metrics, and reply input text box for posting ticket notes via `/api/v1/support/tickets/{ticketRef}/notes`.
- **[create_ticket_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/tickets/presentation/screens/create_ticket_screen.dart)**: Multi-step ticket creation form allowing users to select ticket type (`/api/v1/support/tickets/types`), site (`/api/v1/mobile/sites`), service (`/api/v1/mobile/sites/{siteId}/services`), priority level, title, and description.

### 4.7 Notifications Module (`lib/features/notifications`)
- **[notifications_screen.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/notifications/presentation/screens/notifications_screen.dart)**: Real-time notification feed screen. Allows filtering read/unread messages, marking single notifications as read (`/api/v1/mobile/customer/notifications/{id}/read`), or marking all as read (`/api/v1/mobile/customer/notifications/read-all`).
- **[notification_model.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/features/notifications/data/models/notification_model.dart)**: Strongly typed model representing push and feed notifications (`id`, `title`, `message`, `moduleNo`, `eventType`, `entityId`, `priority`, `actionUrl`, `createdAt`, `deliveredAt`, `readAt`).

---

## 5. Complete API Reference & Endpoint Listing

Base URL: `https://api.seravionconnect.com`

All authenticated requests require the standard HTTP header:
`Authorization: Bearer <AUTH_TOKEN>`

### 5.1 Authentication Endpoints

#### 1. Request OTP
- **Endpoint**: `/api/v1/mobile/auth/request-otp`
- **Method**: `POST`
- **Description**: Triggers a 6-digit verification code sent via SMS to the provided customer phone number.
- **Request Body**:
  ```json
  {
    "mobileNumber": "9875270347"
  }
  ```
- **Response `200 OK`**:
  ```json
  {
    "status": 200,
    "message": "OTP sent successfully",
    "data": {
      "otpSent": true,
      "expiresInSeconds": 60
    }
  }
  ```

#### 2. Verify OTP
- **Endpoint**: `/api/v1/mobile/auth/verify-otp`
- **Method**: `POST`
- **Description**: Validates the 6-digit OTP code and returns an API session token along with user account metadata.
- **Request Body**:
  ```json
  {
    "mobileNumber": "9875270347",
    "otp": "123456"
  }
  ```
- **Response `200 OK`**:
  ```json
  {
    "status": 200,
    "message": "Authentication successful",
    "data": {
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
      "refreshToken": "d8a1b2c3-4e5f-6a7b-8c9d-0e1f2a3b4c5d",
      "account": {
        "accountId": "CUST-897B8",
        "name": "Enterprise Core Care",
        "phoneNumber": "9875270347"
      }
    }
  }
  ```

#### 3. Refresh Token
- **Endpoint**: `/auth/refresh-token`
- **Method**: `POST`
- **Description**: Obtains a new JWT access token using a refresh token.
- **Request Body**: `{ "refreshToken": "..." }`

#### 4. Logout
- **Endpoint**: `/auth/logout`
- **Method**: `POST`
- **Description**: Invalidates the current session token on the server.

#### 5. User Profile
- **Endpoint**: `/auth/profile`
- **Method**: `GET`
- **Description**: Fetches authenticated customer profile details.

---

### 5.2 Dashboard Endpoints

#### 6. Mobile Home Dashboard Summary
- **Endpoint**: `/api/v1/mobile/home`
- **Method**: `GET`
- **Query Parameters**: `search` (optional), `page` (default `0`), `size` (default `10`)
- **Description**: Fetches primary KPI summary metrics, recent activities, and upcoming visits.
- **Response `200 OK`**:
  ```json
  {
    "status": 200,
    "message": "Home dashboard fetched successfully",
    "data": {
      "kpis": {
        "activeContracts": 1,
        "completedServices": 5,
        "inProgressServices": 0,
        "purchaseProductCount": 0
      },
      "recentActivities": [],
      "upcomingVisits": [],
      "success": true
    }
  }
  ```

#### 7. Dashboard Summary
- **Endpoint**: `/dashboard/summary`
- **Method**: `GET`

#### 8. Recent Activity
- **Endpoint**: `/dashboard/recent-activity`
- **Method**: `GET`

---

### 5.3 Contracts Endpoints

#### 9. Mobile Contracts List
- **Endpoint**: `/api/v1/mobile/contracts`
- **Method**: `GET`
- **Query Parameters**: `page` (int), `size` (int), `status` (optional: `ACTIVE` / `EXPIRED`)
- **Response `200 OK`**:
  ```json
  {
    "status": 200,
    "message": "Contracts fetched successfully",
    "data": {
      "count": 3,
      "next": null,
      "prev": null,
      "data": [
        {
          "contractId": "CON-2026-0001",
          "contractNumber": "CON-2026-0001",
          "customerName": "Enterprise Core Care",
          "startDate": "2026-01-01",
          "endDate": "2026-12-31",
          "status": "ACTIVE",
          "totalSaleValue": 450000.0,
          "noOfSites": 8
        }
      ]
    }
  }
  ```

#### 10. Mobile Contract Details
- **Endpoint**: `/api/v1/mobile/contracts/{contractId}`
- **Method**: `GET`
- **Path Parameter**: `contractId` (String, e.g. `CON-2026-0001`)
- **Description**: Fetches complete contract breakdown including site locations, service frequencies, and payment schedules.
- **Response `200 OK`**:
  ```json
  {
    "status": 200,
    "message": "Contract details fetched successfully",
    "data": {
      "contractId": "CON-2026-0001",
      "gmaId": "GMA-2026-0002",
      "customerName": "Enterprise Core Care",
      "branchName": "Chennai",
      "status": "ACTIVE",
      "duration": "1 Year",
      "startDate": "2026-01-01",
      "endDate": "2026-12-31",
      "totalSaleValue": 450000.0,
      "invoicingFrequency": "QUARTERLY",
      "paymentScheduleType": "MONTHLY_POST",
      "noOfSites": 2,
      "sites": [
        {
          "id": "CS-1EE10367",
          "siteName": "J P Nagar 7th phase",
          "address": "No.10/61/2A/2&3, Sarjapur Road Bengaluru-560035",
          "city": "Bangalore Rural",
          "state": "Karnataka",
          "contactPerson": "price",
          "contactMobile": "9875270347",
          "services": [
            {
              "id": "CSS-D26F15E9",
              "serviceTypeName": "01-General Pest Control Service",
              "frequency": "CUSTOM",
              "annualFrequency": 302,
              "visitsPerYearLabel": "302 visits/year",
              "preferredDays": "MON,TUE",
              "preferredTimeSlot": "AFTERNOON"
            }
          ]
        }
      ],
      "paymentLines": [
        {
          "periodLabel": "P1",
          "periodDescription": "May – Jun 2026",
          "amount": 37500.0,
          "dueDate": "2026-06-26",
          "paid": false,
          "paymentStatusDisplay": "DUE"
        }
      ]
    }
  }
  ```

---

### 5.4 Sites Endpoints

#### 11. Customer Sites Directory
- **Endpoint**: `/api/v1/mobile/sites`
- **Method**: `GET`
- **Query Parameters**: `page` (int), `size` (int), `search` (string), `sortBy` (string), `sortDirection` (`ASC` / `DESC`)
- **Response `200 OK`**:
  ```json
  {
    "status": 200,
    "message": "Sites fetched successfully",
    "data": {
      "count": 2,
      "data": [
        {
          "siteId": "CS-0EFC0B67",
          "siteCode": "CS-0EFC0B67",
          "siteName": "Makarpura phase 2",
          "sitePersonName": "devandh",
          "sitePersonContactNo": "9875270347",
          "siteAddress": "Sarjapur Road Bengaluru-560035",
          "googleMapUrl": "https://maps.app.goo.gl/86xJdu7Xiq2eZUes5",
          "totalServices": 2
        }
      ]
    }
  }
  ```

#### 12. Site Services List
- **Endpoint**: `/api/v1/mobile/sites/{siteId}/services`
- **Method**: `GET`
- **Path Parameter**: `siteId` (String)
- **Query Parameters**: `page` (int), `size` (int), `search` (string), `sortBy` (string), `sortDirection` (string)
- **Response `200 OK`**:
  ```json
  {
    "status": 200,
    "message": "Site services fetched successfully",
    "data": {
      "count": 2,
      "data": [
        {
          "taskId": "TASK-1029",
          "serviceName": "01-General Pest Control Service",
          "serviceDate": "2026-08-15",
          "technicianName": "Ramesh Kumar",
          "technicianPhoto": "https://example.com/photos/tech1.jpg",
          "technicianMobileNumber": "9876543210",
          "taskStartTime": "10:00 AM",
          "taskEndTime": "11:30 AM",
          "status": "UPCOMING"
        }
      ]
    }
  }
  ```

#### 13. General Sites List (`/sites`) & Details (`/sites/{siteId}`)
- **Methods**: `GET`

#### 14. Site Health (`/sites/{siteId}/health`)
- **Method**: `GET`

---

### 5.5 Service Visits & PDF Report Endpoints

#### 15. Visits Feed (`/visits` & `/visits/{visitId}`)
- **Methods**: `GET`

#### 16. Service Report PDF Download
- **Endpoint**: `/api/v1/mobile/tasks/my/service-report/pdf`
- **Method**: `GET`
- **Query Parameters**: `taskId` (String, e.g. `TASK-1029`)
- **Headers**: `Authorization: Bearer <token>`
- **Response**: Binary PDF file payload (`application/pdf`).

#### 17. Invoice PDF Download
- **Endpoint**: `/api/v1/invoices/pdf`
- **Method**: `GET`
- **Query Parameters**: `id` (String, e.g. `INV-2026-004`)
- **Headers**: `Authorization: Bearer <token>`
- **Response**: Binary PDF file payload (`application/pdf`).

---

### 5.6 Support Tickets Endpoints

#### 18. Support Tickets List
- **Endpoint**: `/api/v1/support/tickets`
- **Method**: `GET`
- **Query Parameters**: `page` (int), `size` (int), `status` (optional)
- **Response `200 OK`**:
  ```json
  {
    "status": 200,
    "message": "Tickets fetched successfully",
    "data": {
      "count": 4,
      "data": [
        {
          "ticketNumber": "TKT-0001",
          "priority": "HIGH",
          "customerName": "Network Outage in Northern Facility",
          "branchName": "HQ",
          "escalationLevel": "L1",
          "slaHealth": "AT RISK",
          "slaRemainingSeconds": 1800,
          "status": "OPEN",
          "createdAt": "2026-06-28T10:00:00.000Z"
        }
      ]
    }
  }
  ```

#### 19. Create Support Ticket
- **Endpoint**: `/api/v1/support/tickets`
- **Method**: `POST`
- **Request Body**:
  ```json
  {
    "ticketTypeId": "TYPE-01",
    "siteId": "CS-0EFC0B67",
    "serviceId": "CSS-D26F15E9",
    "priority": "HIGH",
    "title": "Pest recurrence in kitchen area",
    "description": "Observed pest activity in the main cafeteria section."
  }
  ```

#### 20. Ticket Types Catalog
- **Endpoint**: `/api/v1/support/tickets/types`
- **Method**: `GET`

#### 21. Ticket Details & Timeline
- **Endpoint**: `/api/v1/support/tickets/by-id`
- **Method**: `GET`
- **Query Parameters**: `ticketRef` (String, e.g. `TKT-0001`)
- **Response `200 OK`**:
  ```json
  {
    "status": 200,
    "message": "Ticket details fetched successfully",
    "data": {
      "ticketNumber": "TKT-0001",
      "priority": "HIGH",
      "status": "OPEN",
      "createdAt": "2026-06-28T10:00:00.000Z",
      "firstResponseAt": "2026-06-28T10:15:00.000Z",
      "slaHealth": "AT RISK",
      "slaRemainingSeconds": 1800,
      "activities": [
        {
          "activityType": "REPLY",
          "performedByLabel": "Support Agent",
          "performedAt": "2026-06-28T10:20:00.000Z",
          "summary": "We are dispatching a technician to inspect.",
          "internal": false
        }
      ]
    }
  }
  ```

#### 22. Add Ticket Reply Note
- **Endpoint**: `/api/v1/support/tickets/{ticketRef}/notes`
- **Method**: `POST`
- **Path Parameter**: `ticketRef` (String)
- **Request Body**:
  ```json
  {
    "summary": "Please update on the estimated arrival time.",
    "internal": false
  }
  ```

---

### 5.7 Notifications Endpoints

#### 23. Register FCM Device Token
- **Endpoint**: `/api/v1/mobile/customer/devices/register`
- **Method**: `POST`
- **Request Body**:
  ```json
  {
    "fcmToken": "f7d9a8c7b6a5...",
    "deviceType": "ANDROID",
    "deviceModel": "Google Pixel 8"
  }
  ```

#### 24. Notifications Feed
- **Endpoint**: `/api/v1/mobile/customer/notifications`
- **Method**: `GET`
- **Query Parameters**: `unreadOnly` (boolean), `pageNo` (int), `pageSize` (int)

#### 25. Unread Notification Count
- **Endpoint**: `/api/v1/mobile/customer/notifications/unread-count`
- **Method**: `GET`
- **Response `200 OK`**:
  ```json
  {
    "status": 200,
    "message": "Unread count fetched",
    "data": 3
  }
  ```

#### 26. Mark Single Notification Read
- **Endpoint**: `/api/v1/mobile/customer/notifications/{id}/read`
- **Method**: `POST`

#### 27. Mark All Notifications Read
- **Endpoint**: `/api/v1/mobile/customer/notifications/read-all`
- **Method**: `POST`

---

### 5.8 Feedback Endpoints

#### 28. Submit Feedback (`/feedback`) & History (`/feedback/history`)
- **Methods**: `POST` / `GET`

---

## 6. Infrastructure Core Utilities & Services

### 6.1 `ApiClient` & Central Error Handling
`ApiClient` ([api_client.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/core/api/api_client.dart)) provides:
1. **Dynamic Authorization Injection**: Automatically attaches `Bearer <AUTH_TOKEN>` on all requests.
2. **Automatic 401 Unauthorized Interceptor**: If the backend returns HTTP 401 (token expired/revoked), `ApiClient.handleUnauthorized()`:
   - Clears local storage via `SessionManager.logout()`.
   - Triggers an alert popup informing the user.
   - Pops all routes and redirects to `LoginScreen`.
3. **Automated Test Fallback Mocking**: When running under test environments (`FLUTTER_TEST`), automatically returns structured mock responses for `/api/v1/mobile/home`, `/api/v1/mobile/sites`, `/api/v1/mobile/contracts`, `/api/v1/support/tickets`, etc., allowing complete UI widget testing without network dependencies.

### 6.2 Session Management (`SessionManager`)
`SessionManager` ([session_manager.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/core/utils/session_manager.dart)) manages user session data in `SharedPreferences`:
- `PREF_KEY_AUTH_TOKEN`: JWT authorization token.
- `PREF_KEY_REFRESH_TOKEN`: Refresh token string.
- `PREF_KEY_USER_PROFILE`: Serialized user account profile JSON.

### 6.3 Firebase Cloud Messaging & Local Notifications (`NotificationService`)
`NotificationService` ([notification_service.dart](file:///Users/cms/AndroidStudioProjects/erp_app_customer/lib/core/services/notification_service.dart)):
- Handlers: Foreground messages (`onMessage`), background/terminated tap (`onMessageOpenedApp`), background message handler (`@pragma('vm:entry-point') _firebaseMessagingBackgroundHandler`).
- Android Channel: `customer_notifications` (Importance Max).
- Unread badge sync: Exposes `ValueNotifier<int> unreadCountNotifier` updated on every incoming notification or token registration.

---

## 7. Design System & Theming

- **Colors (`AppColors`)**: Premium corporate palette featuring Primary Deep Navy, Secondary Teal, Accent Gold, Clean Dark Card backgrounds, and Status Colors (Green for Active/Healthy, Amber for At Risk/Pending, Red for Expired/Breached).
- **Themes (`AppTheme`)**: Material 3 configuration supporting both Light Theme and Dark Theme (`AppTheme.lightTheme` & `AppTheme.darkTheme`).
- **Custom UI Components**:
  - `GlobalHeader`: Dynamic header displaying page title and unread notification badge counter.
  - `GlobalFooter`: Tabbed bottom navigation bar with fluid transitions.
  - `FadeInSlideUp`: Micro-animation wrapper providing sleek entry animations for list items and cards.

---

## 8. Test Suite Summary

The app contains comprehensive automated Flutter widget test files under `test/`:
- `test/contracts_test.dart`: Verifies Dashboard navigation, Contracts list rendering, contract details expansion, nested site services collapsing/expanding, and real-time service searching.
- `test/visits_search_test.dart`: Validates site selection, service visit listing, and search filters.
- `test/sites_test.dart`: Tests customer site directory rendering and detail views.
- `test/support_tickets_test.dart`: Tests support ticket listing, SLA badges, creation forms, and reply posting.
- `test/visit_details_test.dart`: Verifies visit details rendering and PDF download button actions.
- `test/notifications_test.dart`: Validates notification feed rendering and read state toggles.
- `test/back_navigation_test.dart`: Verifies back button routing integrity across screens.
