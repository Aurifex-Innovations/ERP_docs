# 📱 Field Technician / Employee Mobile Application — Screen-Wise Functional Document

**Version:** 2.0
**Date:** March 2026
**Audience:** Development Team, QA Team, UI/UX Designers, Product Managers
**Purpose:** End-user functional documentation for the mobile application used by Field Technicians and Employees of Pest Control companies onboarded to the Seravion ERP platform.

---

## Table of Contents

1. [Screen 1: Splash Screen](#screen-1-splash-screen)
2. [Screen 2: Sign In](#screen-2-sign-in)
3. [Screen 3: Forgot Password](#screen-3-forgot-password)
4. [Screen 4: Enter OTP](#screen-4-enter-otp)
5. [Screen 5: Reset Password](#screen-5-reset-password)
6. [Screen 6: Home Page (Dashboard)](#screen-6-home-page-dashboard)
7. [Screen 7: Bottom Navigation Bar](#screen-7-bottom-navigation-bar)
8. [Screen 8: Services (Tasks) Page](#screen-8-services-tasks-page)
9. [Screen 9: Calendar Page](#screen-9-calendar-page)
10. [Screen 9.1: Leaves Tab](#screen-91-leaves-tab)
11. [Screen 9.1.1: Apply Leave Form](#screen-911-apply-leave-form)
12. [Screen 10: Profile Page (View Mode)](#screen-10-profile-page-view-mode)
13. [Screen 10.1: Edit Profile](#screen-101-edit-profile)
14. [Screen 11: Task Detail Page](#screen-11-task-detail-page)
15. [Screen 12: Navigation Map View](#screen-12-navigation-map-view)
16. [Screen 13: Before Task Photos](#screen-13-before-task-photos)
17. [Screen 14: Service Execution Form](#screen-14-service-execution-form)
18. [Screen 15: Technician Observations](#screen-15-technician-observations)
19. [Screen 16: After Task Photos](#screen-16-after-task-photos)
20. [Screen 17: Customer Verification (OTP) & Feedback](#screen-17-customer-verification-otp--feedback)
21. [Screen 18: Service Report View](#screen-18-service-report-view)
22. [Screen 19: Notifications](#screen-19-notifications)
23. [Screen 20: Chatbot](#screen-20-chatbot)
24. [Screen 21: Petty Cash — My Requests](#screen-21-petty-cash--my-requests)
25. [Screen 21.1: Add Petty Cash Request](#screen-211-add-petty-cash-request)
26. [Screen 21.2: View My Petty Cash Request](#screen-212-view-my-petty-cash-request)
27. [Screen 22: Settings](#screen-22-settings)
28. [Complete Task Execution Flow (End-to-End)](#-complete-task-execution-flow-end-to-end)
29. [Role-Based Access Control (RBAC)](#-role-based-access-control-rbac--mobile-app)
30. [Global Business Rules Summary](#-global-business-rules-summary)

---

## Module Dependencies

| Mobile Screen Area | ERP Module Reference |
| --- | --- |
| Authentication | Module 1 – IAM (Identity & Access Management) |
| Task Management | Module 21 – Task Management |
| Service Execution | Module 12 – Services, Module 21 – Task Completion & Material Log (21.6) |
| Stock / Material Usage | Module 11 – Stock Management |
| Location & Navigation | Module 22 – Live Location & Travel Tracking |
| Attendance | Module 25 – HRM (Attendance Section) |
| Leave | Module 25 – HRM (Leave Section) |
| Profile & Salary | Module 25 – HRM, Module 6 (Leave Config), Module 8 (Employee Master), Module 27 (User Profile) |
| Petty Cash | Module 24 – Petty Cash Management, Module 8 (Employee), Module 7 (Branch) |

---

# Screen 1: Splash Screen

**Purpose:** Initial loading screen and brand display while the app initializes and checks session status.

---

## Screen Layout

```
┌────────────────────────────────────────────┐
│                                            │
│                                            │
│                                            │
│                                            │
│            [Company Logo Area]             │
│                                            │
│               Seravion ERP                 │
│                                            │
│                                            │
│               [Loading Spinner]            │
│                                            │
│                                            │
│                                            │
│                                            │
│     App Version: v2.0.0                    │
└────────────────────────────────────────────┘
```

## Behaviour

| Rule | Description |
| --- | --- |
| Display Time | Displays for 2-3 seconds minimum while data loads in background |
| Auto-Redirect | If active session exists → redirects to Home Dashboard (Screen 6). If no active session → redirects to Sign In (Screen 2) |
| Background Checks | App fetches necessary config and checks authentication token validity while logo is displayed |

---

# Screen 2: Sign In

**Source Reference:** Module 1 – IAM (Identity & Access Management)
**Purpose:** Authenticate the field technician/employee into the mobile application using their company-issued credentials.

---

## Screen Layout

```
┌────────────────────────────────────────────┐
│                                            │
│            [Company Logo Area]             │
│                                            │
│     ─── SIGN IN ───────────────────────    │
│                                            │
│     Account ID*                            │
│     [________________________]             │
│                                            │
│     Username*                              │
│     [________________________]             │
│                                            │
│     Password*                              │
│     [________________________] [👁]        │
│                                            │
│     [Forgot Password?]                     │
│                                            │
│     ┌──────────────────────────────────┐   │
│     │           SIGN IN                │   │
│     └──────────────────────────────────┘   │
│                                            │
│     App Version: v2.0.0                    │
└────────────────────────────────────────────┘
```

## Screen Components / Fields

| Element / Field | Type | Required | Validation | Description |
| --- | --- | --- | --- | --- |
| Page Header | Header | — | — | Contains Company Logo Area |
| Account ID | Text Input | Yes | Must match existing IAM user record | Company-issued unique Account ID |
| Username | Text Input | Yes | Must match IAM user record | User's unique username |
| Password | Password Input | Yes | Minimum 8 characters | User's secret password, masked by default |
| Show/Hide Password | Toggle Icon (👁) | — | — | Toggles password visibility |
| Forgot Password | Text Link | — | — | Navigates to Forgot Password screen (Screen 3) |

## Actions

| Action | Trigger | System Behaviour |
| --- | --- | --- |
| **Sign In** | Tap "SIGN IN" button | Validates Account ID + Password against IAM module. On success → redirects to Home Dashboard (Screen 6). On failure → shows inline error: "Invalid Account ID or Password." |
| **Forgot Password** | Tap link | Opens Forgot Password screen (Screen 3) |

## Business Rules

| Rule | Description |
| --- | --- |
| IAM User Only | Only users created through Module 8 (Employee Management) with an active status can log in |
| Role Restriction | App login is restricted to roles: Technician, Senior Technician |
| Session Management | Session persists until manual logout or token expiry |
| Account Lock | After 5 consecutive failed attempts, account is locked for 30 minutes |

## Error Messages

| Condition | Message |
| --- | --- |
| Empty fields | "Please enter your Account ID and Password." |
| Invalid credentials | "Invalid Account ID or Password. Please try again." |
| Account locked | "Your account has been locked due to multiple failed attempts. Please try again after 30 minutes or contact your administrator." |
| Inactive account | "Your account is currently inactive. Please contact your administrator." |
| No network | "No internet connection. Please check your network and try again." |

---

# Screen 3: Forgot Password

**Source Reference:** Module 1 – IAM
**Purpose:** Allow the user to reset their password by receiving a reset link or OTP on their registered email/phone.

---

## Screen Layout

```
┌────────────────────────────────────────────┐
│  [← Back]       FORGOT PASSWORD            │
│                                            │
│     Enter your registered                  │
│     Email, or Phone Number to receive      │
│     a password reset OTP.                  │
│                                            │
│     Email / Phone*                         │
│     [________________________]             │
│                                            │
│     ┌──────────────────────────────────┐   │
│     │       SEND RESET OTP             │   │
│     └──────────────────────────────────┘   │
│                                            │
│     [← Back to Sign In]                   │
└────────────────────────────────────────────┘
```

## Screen Fields

| Field | Type | Required | Validation | Description |
| --- | --- | --- | --- | --- |
|Email / Phone | Text Input | Yes | Must match an existing IAM record (Account ID, registered email, or registered phone number) | User enters their Account ID, registered email, or registered 10-digit phone number |

## Actions

| Action | Trigger | System Behaviour |
| --- | --- | --- |
| **Send Reset OTP** | Tap button | Validates the input against IAM records. If match found → sends OTP to registered email AND phone. Shows success toast: "OTP sent to your registered email / phone." If no match → error: "No account found with the provided details." Navigates to Enter OTP screen (Screen 4). |
| **Back** | Tap ← icon | Returns to Login screen (Screen 2) |

## Error Messages

| Condition | Message |
| --- | --- |
| Empty field | "Please enter your Account ID, Email, or Phone Number." |
| No match found | "No account found with the provided details. Please check and try again." |
| Invalid phone format | "Please enter a valid 10-digit phone number." |
| OTP send failure | "Failed to send OTP. Please try again." |

---

# Screen 4: Enter OTP

**Source Reference:** Module 1 – IAM
**Purpose:** Verify the 4-digit OTP sent to the user for password reset.

---

## Screen Layout

```
┌────────────────────────────────────────────┐
│  [← Back]       VERIFY OTP                 │
│                                            │
│  Enter the 4-digit OTP sent to your        │
│  registered Email / Phone.                 │
│                                            │
│     [  _  ] [  _  ] [  _  ] [  _  ]        │
│                                            │
│  OTP sent to: 98XXXX3210 / r***@mail.com   │
│  Expires in: 02:45                          │
│                                            │
│  [Resend OTP]                               │
│                                            │
│  ┌──────────────────────────────────┐      │
│  │         VERIFY OTP               │      │
│  └──────────────────────────────────┘      │
└────────────────────────────────────────────┘
```

## Screen Fields / Components

| Field | Type | Required | Validation | Description |
| --- | --- | --- | --- | --- |
| OTP Input | 4-digit OTP | Yes | Must match sent OTP | User enters OTP |
| Resend OTP | Link | — | Cooldown 60s | Requests a new OTP |
| Timer | Display | Auto | Expiry 3 min | Shows time remaining |

## Actions & Popups

| Action | Behaviour |
| --- | --- |
| **Verify OTP** | If OTP matches → shows popup: **"Verification successful"** → navigates to Reset Password (Screen 5). If OTP mismatch → shows popup: **"Failed: Invalid OTP. Please try again."** |
| **Resend OTP** | Sends new OTP → toast: "OTP resent successfully." If failed → popup: "Failed to resend OTP. Please try again." |

## Business Rules

| Rule | Description |
| --- | --- |
| OTP Expiry | OTP is valid for 3 minutes |
| Max Attempts | Maximum 3 verification attempts. After 3 failures → force resend |
| Resend Cooldown | Resend available only after 60 seconds |

---

# Screen 5: Reset Password

**Source Reference:** Module 1 – IAM
**Purpose:** Set a new password after successful OTP verification.

---

## Screen Layout

```
┌────────────────────────────────────────────┐
│  [← Back]       RESET PASSWORD              │
│                                            │
│  New Password*                              │
│  [________________________]  [✓ ticks]      │
│                                            │
│  Confirm New Password*                      │
│  [________________________]  [✓ ticks]      │
│                                            │
│  Password Rules:                            │
│  ✓ Min 8 characters                         │
│  ✓ 1 uppercase, 1 lowercase                  │
│  ✓ 1 number, 1 special character             │
│                                            │
│  ┌──────────────────────────────────┐      │
│  │       RESET PASSWORD             │      │
│  └──────────────────────────────────┘      │
│                                            │
│  [← Back to Sign In]                        │
└────────────────────────────────────────────┘
```

## Validations

| Rule | Description |
| --- | --- |
| Strong Password | Must meet password policy |
| Match | New Password and Confirm must match |

## Actions

| Action | Behaviour |
| --- | --- |
| **Reset Password** | On success → toast: "Password reset successfully." → navigates to Login (Screen 2) |
| **Back to Sign In** | Navigates to Login (Screen 2) |

---

# Screen 6: Home Page (Dashboard)

**Source Reference:** Module 21 – Task Management (21.2 Daily Task View), Module 25 – HRM (Attendance)
**Purpose:** Central hub for the field technician. Shows attendance controls, today's assigned tasks, and quick-access navigation.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [👤 Profile] Welcome Ravi S.         [🔔 3] │
│  Branch: Mumbai | Role: Technician           │
│  ID: EMP-00124                               │
│                                              │
│  ─── ATTENDANCE ─────────────────────────    │
│  ┌──────────────────────────────────────┐    │
│  │  Today: 23 Mar 2026                  │    │
│  │  Status: 🟢 Checked In               │    │
│  │                                      │    │
│  │  Punch In : 08:00 AM                │    │
│  │  Punch Out: [  ──────●──── SLIDE →]  │    │
│  │                                      │    │
│  │  Total Hours: 4h 30m (running)       │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── TODAY'S TASKS ─────────── [View All →]  │
│  ┌──────────────────────────────────────┐    │
│  │  📋 TASK-2026-0201                   │    │
│  │  🏢 ABC Corp — Head Office           │    │
│  │  🐛 Cockroach Treatment              │    │
│  │  ⏰ 08:00 – 10:00 AM                │    │
│  │  ● Pending                           │    │
│  └──────────────────────────────────────┘    │
│  ┌──────────────────────────────────────┐    │
│  │  📋 TASK-2026-0202                   │    │
│  │  🏢 PQR Foods — Warehouse            │    │
│  │  🐀 Rodent Management                │    │
│  │  ⏰ 11:00 – 13:00 PM                │    │
│  │  ✅ Completed                        │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  [💬 Chatbot]                                │
│                                              │
│  [Bottom Navigation Bar]                     │
└──────────────────────────────────────────────┘
```

## Screen Components

### 6.1 Header Section

| Element | Type | Description |
| --- | --- | --- |
| Profile Icon / Avatar | Icon/Image | User's profile photo (from Module 8 Employee record). Tap → opens Profile (Screen 10) |
| Welcome Text | Text | Dynamic greeting "Welcome" + Employee Name |
| ID & Branch & Role | Text (Read-only) | Employee ID, Current branch and role assignment from Module 8 |
| Notification Bell (🔔) | Icon + Badge | Shows unread notification count. Tap → opens Notifications screen (Screen 19) |

### 6.2 Attendance Section

| Field | Type | Behaviour | Source |
| --- | --- | --- | --- |
| Today's Date | Display | Auto-populated with current date | System |
| Status Badge | Badge | 🟢 Checked In / 🔴 Not Checked In / ⚪ Checked Out | Module 25 Attendance |
| Punch In | Slider | Swipe-to-confirm slider to record Check-In time. GPS location is captured automatically | Module 25 – Attendance (25.3.1) |
| Punch Out | Slider | Swipe-to-confirm slider to record Check-Out time. GPS location is captured automatically. Available only after Punch In | Module 25 – Attendance |
| Total Hours | Display (Auto) | Auto-calculated from Punch In to current time (running) or Punch Out (final) | System calculated |

**Attendance Business Rules (from Module 25):**

| Rule | Description |
| --- | --- |
| GPS Mandatory | Punch In/Out captures GPS coordinates automatically |
| One Punch-In Per Day | Only one Punch In allowed per day |
| Punch Out After Punch In | Punch Out slider only appears after Punch In is recorded |
| Late Marking | If Punch In is after the designated shift start time (from Module 6 config), the system marks the day as "Late" |
| Background Tracking | GPS tracking for Module 22 (Live Location) starts only after Punch In and stops after Punch Out |

### 6.3 Today's Tasks Section

| Element | Type | Description |
| --- | --- | --- |
| Section Header | Text | "TODAY'S TASKS" with count badge |
| View All Link | Text Link | Navigates to Services Page (Screen 8) showing all tasks |
| Task Cards | Scrollable List | Shows tasks assigned to this technician for today (from Module 21 – 21.2 Daily Task View) |

**Task Card Fields:**

| Field | Type | Description | Source |
| --- | --- | --- | --- |
| Task ID | Display | Unique task identifier (e.g., TASK-2026-0201) | Module 21 |
| Customer Name | Display | Customer business name | Module 18 → Module 21 |
| Site Name | Display | Service location / site name | Module 21 |
| Service Type | Display | Specific pest control service (e.g., Cockroach Treatment) | Module 12 → Module 21 |
| Scheduled Time | Display | Start – End time slot | Module 21 |
| Status Badge | Badge | Pending / In Progress / Completed / Overdue | Module 21 |

**Task Card Tap Action:** Tapping anywhere on a task card → opens Task Detail Page (Screen 11). Map navigation and Service Report access have been moved inside the detail screen.

### 6.4 Chatbot Icon

| Element | Type | Description |
| --- | --- | --- |
| Chatbot FAB | Floating Action Button | Fixed at bottom-right corner. Tap → opens Chatbot screen (Screen 20) |
 

# Screen 7: Bottom Navigation Bar

**Purpose:** Persistent navigation across all screens of the app (except inside execution flow). Always visible at the bottom of the screen.

---

## Screen Layout

```
  ┌──────────────────────────────────────┐    
  │ 🏠 Home │ 📋 Services │ 📅 Calendar │    
  │ 💰 Petty Cash │ ⚙️ Settings          │    
  └──────────────────────────────────────┘    
```

## Navigation Items

| Tab | Icon | Label | Destination | Badge |
| --- | --- | --- | --- | --- |
| Home | 🏠 | Home | Screen 6: Dashboard | — |
| Services | 📋 | Services | Screen 8: Services (Tasks) Page | Count of pending tasks |
| Calendar | 📅 | Calendar | Screen 9: Calendar Page (Tabs: Calendar View \| Leaves) | — |
| Petty Cash | 💰 | Petty Cash | Screen 21: Petty Cash — My Requests | Count of Draft / Returned requests |
| Settings | ⚙️ | Settings | Screen 22: Settings | — |

**Note:**
- **Profile is accessible from the Settings tab or the Home Screen header** and navigates to Screen 10.
- **Leave module relies inside the Calendar tab.**

## Behaviour

| Rule | Description |
| --- | --- |
| Active State | The currently active tab is highlighted with the primary accent color |
| Persistent | Navigation bar is visible on all main screens (hidden during task execution flow: Screens 13–17) |
| Badge Counts | Real-time badge counts update from backend |

---

# Screen 8: Services (Tasks) Page

**Source Reference:** Module 21 – Task Management (21.1 Calendar Dashboard, 21.2 Daily Task View)
**Purpose:** Comprehensive view of all tasks assigned to the technician, with filtering and search capabilities.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  SERVICES                        [🔍] [🔽]   │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │ [Assigned (4)]  │  [Completed (12)]  │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  Filters: [📅 Date Range] [▼ Priority]       │
│                                              │
│  ─── 23 Mar 2026 (Today) ───────────────     │
│  ┌──────────────────────────────────────┐    │
│  │  📋 TASK-2026-0201                   │    │
│  │  🏢 ABC Corp — Head Office           │    │
│  │  🐛 Cockroach Treatment              │    │
│  │  ⏰ 08:00 – 10:00   ● Pending       │    │
│  │  Priority: 🔴 Urgent                 │    │
│  └──────────────────────────────────────┘    │
│  ┌──────────────────────────────────────┐    │
│  │  📋 TASK-2026-0203                   │    │
│  │  🏢 XYZ Hotel — Lobby Area           │    │
│  │  🐜 Ant Treatment                    │    │
│  │  ⏰ 14:00 – 16:00   ● Pending       │    │
│  │  Priority: 🟢 Normal                 │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── 24 Mar 2026 (Tomorrow) ────────────     │
│  ┌──────────────────────────────────────┐    │
│  │  📋 TASK-2026-0210                   │    │
│  │  🏢 LMN Pvt — Factory               │    │
│  │  🐀 Rodent Control                   │    │
│  │  ⏰ 09:00 – 11:00   ● Pending       │    │
│  │  Priority: 🟡 High                   │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  [Bottom Navigation Bar]                     │
└──────────────────────────────────────────────┘
```

## Screen Components

### 8.1 Header

| Element | Type | Description |
| --- | --- | --- |
| Title | Text | "SERVICES" |
| Search Icon (🔍) | Icon | Opens search bar for searching by Task ID, Customer Name, or Service Type |
| Filter Icon (🔽) | Icon | Opens filter panel with dropdown options |

### 8.2 Tabs

| Tab | Description | Content |
| --- | --- | --- |
| **Assigned** | Active / Pending tasks | Shows tasks with status: Pending, In Progress. Grouped by date. Count badge shows total |
| **Completed** | Finished tasks | Shows tasks with status: Completed. |

### 8.3 Filters

| Filter | Type | Options | Source |
| --- | --- | --- | --- |
| Date Range | Date Picker | Select From – To date range | — |
| Priority | Dropdown | All / Normal / High / Urgent / Critical | Module 21 |
| Service Category | Dropdown | All / General Pest / Specialized / Fumigation etc. | Module 12 |
| Status | Dropdown | All / Pending / In Progress / Overdue (in Assigned tab) | Module 21 |

### 8.4 Task Cards

Same card structure as described in [Screen 6 → Section 6.3](#63-todays-tasks-section), with the addition of:

| Extra Field | Type | Description |
| --- | --- | --- |
| Priority Badge | Badge | 🔴 Urgent / 🟡 High / 🟢 Normal — from Module 21 |
| Date Group Header | Section Header | Tasks grouped by scheduled date. displays full date with day label. |

**Card Tap Action:** Opens Task Detail Page (Screen 11).

---

# Screen 9: Calendar Page

**Source Reference:** Module 25 – HRM (25.3 Attendance Calendar), Module 21 – Task Management (21.1 Calendar Dashboard)
**Purpose:** Visual calendar showing the technician's attendance records and task schedule, with a second tab containing the full Leave module (history + apply).

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  CALENDAR                                    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │ [Calendar View]  |  [Leaves]         │    │
│  └──────────────────────────────────────┘    │
│  (Active tab underline highlight)            │
│                                              │
│  [◀ Feb]     MARCH 2026       [Apr ▶]       │
│  View: [Monthly ▼]                           │
│                                              │
│  ┌──┬──┬──┬──┬──┬──┬──┐                     │
│  │Mo│Tu│We│Th│Fr│Sa│Su│                     │
│  ├──┼──┼──┼──┼──┼──┼──┤                     │
│  │  │  │  │  │  │ 1│ 2│                     │
│  ├──┼──┼──┼──┼──┼──┼──┤                     │
│  │🟢│🟢│🟢│🟢│🟢│⬜│⬜│  ← 3-9             │
│  │₃ │₂ │₁ │₃ │₂ │  │  │  (task counts)     │
│  ├──┼──┼──┼──┼──┼──┼──┤                     │
│  │🟢│🟢│🔴│🟢│🟢│⬜│⬜│  ← 10-16           │
│  │₂ │₃ │LV│₁ │₂ │  │  │  (12 = Leave)      │
│  ├──┼──┼──┼──┼──┼──┼──┤                     │
│  │🟢│🟢│🟢│🟡│🟢│⬜│⬜│  ← 17-23           │
│  │₃ │₂ │₁ │₂ │₃ │  │  │  (20 = Late)       │
│  └──┴──┴──┴──┴──┴──┴──┘                     │
│                                              │
│  Legend: 🟢 Present  🔴 Leave  🟡 Late       │
│          ⬜ Week Off  ⚪ Absent  ₃ = 3 Tasks  │
│                                              │
│  ─── 23 Mar 2026 (Selected) ────────────     │
│  Attendance: 🟢 Present | In: 08:00 Out: —   │
│                                              │
│  Tasks (3):                                  │
│  ┌──────────────────────────────────────┐    │
│  │ 📋 TASK-0201 | ABC Corp | 08:00-10:00│    │
│  │ ● Completed                          │    │
│  ├──────────────────────────────────────┤    │
│  │ 📋 TASK-0202 | PQR Foods | 11:00-13:00│   │
│  │ ● Pending                            │    │
│  ├──────────────────────────────────────┤    │
│  │ 📋 TASK-0203 | XYZ Hotel | 14:00-16:00│   │
│  │ ● Pending                            │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  [Bottom Navigation Bar]                     │
└──────────────────────────────────────────────┘
```

## Screen Components

### 9.1 Header & Calendar Grid

| Element / Field | Type | Description |
| --- | --- | --- |
| Page Header | Header | "CALENDAR" title bar |
| Tab Switcher | Tabs | Highlighted **[Calendar View \| Leaves]**. Switching tabs applies filtering or opens Screen 9.1 |
| Month Navigation | Arrows (◀ ▶) | Navigate to previous/next month |
| View Toggle | Dropdown | Monthly / Weekly view |
| Date Cells | Tap-able Grid | Each cell shows attendance status (color) and task count badge |

### 9.2 Calendar Cell Indicators

| Indicator | Color | Meaning | Source |
| --- | --- | --- | --- |
| 🟢 Present | Green | Employee was/is present (Punch In recorded) | Module 25 – Attendance |
| 🔴 Leave | Red | Approved leave on that date | Module 25 – Leave |
| 🟡 Late | Yellow | Employee punched in after shift start time | Module 25 – Attendance |
| ⬜ Week Off | Grey | Weekend / configured weekly off | Module 6 – Leave Config |
| ⚪ Absent | White/Empty | No attendance record (workday with no Punch In) | Module 25 – Attendance |
| Task Count (₃) | Number Badge | Number of tasks scheduled on that date | Module 21 |

### 9.3 Day Detail Section (Below Calendar)

Appears when a date cell is tapped:

| Field | Type | Description |
| --- | --- | --- |
| Date | Display | Selected date |
| Attendance Status | Badge | Present / Absent / Leave / Late / Week Off |
| Punch In Time | Display | Recorded check-in time (from Module 25) |
| Punch Out Time | Display | Recorded check-out time (from Module 25) |
| Task List | Scrollable Cards | Compact task cards for the selected day (Task ID, Customer, Time, Status) |

**Task Card Tap:** Opens Task Detail Page (Screen 11).

---

# Screen 9.1: Leaves Tab

**Source Reference:** Module 25 – HRM (25.4 Leave Application, 25.5 Leave Requests)
**Purpose:** View leave history and apply for new leave requests. This is the content shown when the "Leaves" tab is selected in the Calendar screen.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  CALENDAR                                    │
│  ┌──────────────────────────────────────┐    │
│  │ [Calendar View]  |  [Leaves]         │    │
│  └──────────────────────────────────────┘    │
│  LEAVES                          [🔍] [⚙️]   │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │  Leave Balance                       │    │
│  │  CL: 8/12  |  SL: 5/6  |  PL: 10/15│    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │ [All (15)]  │ [Pending (2)] │        │    │
│  │ [Approved (10)] │ [Rejected (3)]     │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │  🏖️ Casual Leave                     │    │
│  │  25 Mar – 26 Mar 2026 (2 days)       │    │
│  │  Leave Period: 25 Mar 2026 → 26 Mar  │    │
│  │  Reason: Family function             │    │
│  │  Status: ⏳ Pending                   │    │
│  │  Applied: 23 Mar 2026, 10:30 AM      │    │
│  └──────────────────────────────────────┘    │
│  ┌──────────────────────────────────────┐    │
│  │  🤒 Sick Leave                        │    │
│  │  24 Mar 2026 (1 day)                  │    │
│  │  Leave Period: 24 Mar 2026 → 24 Mar  │    │
│  │  Reason: Fever and cold              │    │
│  │  Status: ✅ Approved                  │    │
│  │  Applied: 23 Mar 2026, 09:15 AM      │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │       [+ APPLY LEAVE]                │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  [Bottom Navigation Bar]                     │
└──────────────────────────────────────────────┘
```

## Screen Components

### 9.1.1 Header & Top Controls

| Element / Field | Type | Description |
| --- | --- | --- |
| Tab Header | Header | "LEAVES" title |
| Search Icon (🔍) | Icon | Tap to search leave records by reason or date |
| Filter Icon (⚙️) | Icon | Tap to open advanced filter options |

### 9.1.2 Leave Balance Summary

| Field | Type | Description | Source |
| --- | --- | --- | --- |
| Casual Leave (CL) | Display | Used / Total allocation | Module 6 → Module 8 → Module 25 |
| Sick Leave (SL) | Display | Used / Total allocation | Module 6 → Module 8 → Module 25 |
| Paid Leave (PL) | Display | Used / Total allocation | Module 6 → Module 8 → Module 25 |

### 9.1.3 Leave Filters (Tabs)

| Tab | Filter |
| --- | --- |
| All | Shows all leave records |
| Pending | Status = Pending (awaiting approval) |
| Approved | Status = Approved |
| Rejected | Status = Rejected |

### 9.1.4 Leave Card Fields

| Field | Type | Description |
| --- | --- | --- |
| Leave Type | Badge | CL / SL / PL with icon |
| Date Range | Display | Shows basic date (e.g., "24 Mar 2026 (1 day)") |
| Leave Period | Display | For approved leaves, shows actual period "**From Date → To Date**" explicitly mapped onto attendance dates. |
| Reason | Display | Employee's leave description |
| Status | Badge | ⏳ Pending / ✅ Approved / ❌ Rejected |
| Applied Date | Display | Submission timestamp |
| Rejection Reason | Display (conditional) | Shown only if Status = Rejected |

### 9.1.5 Apply Leave Button

| Action | Trigger | Destination |
| --- | --- | --- |
| Apply Leave | Tap "+ APPLY LEAVE" button | Opens Apply Leave Form (Screen 9.1.1) |

---

# Screen 9.1.1: Apply Leave Form

**Source Reference:** Module 25 – HRM (25.4 Leave Application — Employee Self-Service Flow)
**Purpose:** Form for the technician to submit a new leave request. Includes validation handling for exhausted balances.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]        APPLY LEAVE                 │
│                                              │
│  Leave Type*                                 │
│  [▼ Casual Leave ▼]                          │
│    • Casual Leave (CL)                       │
│    • Sick Leave (SL)                         │
│    • Paid Leave (PL)                         │
│                                              │
│  From Date*                                  │
│  [📅 25 Mar 2026]                            │
│                                              │
│  To Date*                                    │
│  [📅 26 Mar 2026]                            │
│                                              │
│  Total Days: 2 (auto-calculated)             │
│                                              │
│  Description / Reason*                       │
│  [Family function — need 2 days off    ]     │
│  [                                     ]     │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │           SUBMIT REQUEST             │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  [Cancel]                                    │
└──────────────────────────────────────────────┘
```

## Form Fields

| Field | Type | Required | Validation | Description |
| --- | --- | --- | --- | --- |
| Leave Type | Dropdown | Yes | Must select one | CL / SL / PL (from Module 6 Leave Config) |
| From Date | Date Picker | Yes | Cannot be past date | Leave start date |
| To Date | Date Picker | Yes | Must be ≥ From Date | Leave end date |
| Total Days | Display (Auto) | Auto | Auto-calculated (excludes week offs & holidays) | Computed leave day count |
| Description | Textarea | Yes | Min 10 chars, Max 500 chars | Reason for leave application |

## Validation & Error Rules

| Condition | Message | Behaviour |
| --- | --- | --- |
| Zero / Exhausted Balance | "You don't have leave balance for selected leave type." | Shows red error text directly below the Leave Type dropdown and disabled Submit button. Error must be resolved to proceed. |
| Insufficient Balance (Partial) | "Insufficient leave balance. You have X days remaining." | Shown if they select more days than balance available. Blocks submission. |
| Overlap Check | "Leave already exists for selected dates." | Blocks submission. |
| Past Date | "From Date cannot be in the past." | Blocks submission. |
| Holiday Exclusion | N/A | Week offs and public holidays are excluded from Total Days calculation automatically. |

## Actions

| Action | System Behaviour |
| --- | --- |
| **Submit Request** | Validates form → Creates leave request with Status = Pending → Appears in Module 25 for HR/Manager approval → Shows success toast: "Leave request submitted successfully." → Returns to Leaves tab (Screen 9.1) |
| **Cancel** | Discards form and returns to Leaves tab (Screen 9.1) |

---

# Screen 10: Profile Page (View Mode)

**Source Reference:** Module 27 – User Profile
**Purpose:** View a comprehensive, read-only profile of the logged-in employee. Structured explicitly indicating which fields are **Editable** vs **Non-Editable**. Accessible from Home header or Settings screen.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]        PROFILE          [✏️ Edit]  │
│                                              │
│  ┌─ 1. BASIC USER INFORMATION ────────────┐  │
│  │  [👤 Profile Photo]                    │  │
│  │  EMP-00124 | Ravi Sharma               │  │
│  │  Account ID: ravi.s                    │  │
│  │  ravi.s@company.com                    │  │
│  │  📱 9876543210  |  Alt: 9123456789     │  │
│  │  Status: 🟢 Active                      │  │
│  │  Joined: 15 Jun 2024 (Permanent)       │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌─ 2. ORGANIZATION INFORMATION ──────────┐  │
│  │  Dept        : Operations              │  │
│  │  Designation : Senior Pest Ctrl Tech   │  │
│  │  Role        : Senior Technician       │  │
│  │  Branch      : Mumbai — Andheri        │  │
│  │  Manager     : Anil K.                 │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌─ 3. ADDRESS INFORMATION ───────────────┐  │
│  │  Current Address:                      │  │
│  │  42, Shanti Nagar, Andheri West        │  │
│  │  Mumbai, Maharashtra, India - 400058   │  │
│  │                                        │  │
│  │  Permanent Address:                    │  │
│  │  (Same as Current)                     │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌─ 4. SALARY INFORMATION ────────────────┐  │
│  │  Salary Type : CTC                     │  │
│  │  Basic Salary: ₹20,000                 │  │
│  │  HRA         : ₹5,000                  │  │
│  │  Allowances  : ₹3,000                  │  │
│  │  Incentive   : ₹2,000                  │  │
│  │  Deductions  : ₹2,500                  │  │
│  │  PF: ✅ | ESI: ✅ | TDS: ❌              │  │
│  │  [📥 Download Latest Salary Slip]       │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌─ 5. BANK INFORMATION ──────────────────┐  │
│  │  Bank   : State Bank of India          │  │
│  │  Acct No: ●●●●●●●●4321                 │  │
│  │  Holder : Ravi Sharma                  │  │
│  │  IFSC   : SBIN0001234                  │  │
│  │  UPI    : ravi.s@sbi                   │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌─ 6. DOCUMENTS ─────────────────────────┐  │
│  │  Gov ID Proof       : ✅ [📥][👁]       │  │
│  │  Address Proof      : ✅ [📥][👁]       │  │
│  │  Employ. Contract   : ✅ [📥][👁]       │  │
│  │  Education Certs    : ✅ [📥][👁]       │  │
│  │  Other Docs         : ❌ Pending        │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌─ 7. LEAVE SUMMARY ─────────────────────┐  │
│  │  CL Balance: 8/12                      │  │
│  │  SL Balance: 5/6                       │  │
│  │  PL Balance: 10/15                     │  │
│  │  Total Leaves Taken: 4                 │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │           [🚪 LOGOUT]                  │  │
│  └────────────────────────────────────────┘  │
└──────────────────────────────────────────────┘
```

## Screen Components Details (Edit Permissions)

Fields are strictly mapped to Self-Edit capabilities corresponding to Module 27 rules. 

| Section | Field | Permissiom | Description |
| --- | --- | --- | --- |
| 1. Basic Info | Profile Photo | **(Editable)** | User can change photo in edit mode |
| 1. Basic Info | Contact No, Alt No | **(Editable)** | User can update phone numbers |
| 1. Basic Info | EMP ID, Name, Account ID, Email, Status, DOJ | **(Non-Editable)** | Fixed identity fields. Read-only. |
| 2. Org Info | Dept, Designation, Role, Branch, Manager | **(Non-Editable)** | Set by HR. Read-only. |
| 3. Address | Current & Permanent Address details | **(Editable)** | User can update address in edit mode |
| 4. Salary Info | All Salary components | **(Non-Editable)** | Managed by HR/Finance. Read-only. Hidden if permission lacks. |
| 5. Bank Info | Bank Name, Acct No, IFSC, UPI | **(Editable)** | User can update financial details for payouts |
| 6. Documents | Gov ID, Address Proof, Education, Others | **(Editable)** | User can upload missing documents |
| 6. Documents | Employment Contract | **(Non-Editable)** | Admin/HR upload only. |
| 7. Leave Summary| Balance and metrics | **(Non-Editable)** | Derived from Module 25. |

## Actions

| Action | Behaviour |
| --- | --- |
| **Edit** | Opens **Screen 10.1: Edit Profile** |
| **Download Salary Slip** | Downloads the latest month's PDF salary slip |
| **Logout** | Confirmation popup: "Are you sure you want to logout?" → Clears session, stops GPS tracking, redirects to Login (Screen 2) |

# Screen 10.1: Edit Profile

**Source Reference:** Module 27 – User Profile (27.2 Self-Edit Rules)
**Purpose:** Form to allow the user to modify explicitly editable fields and upload missing documents. Requires HR approval for critical changes per system rules.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Cancel]      EDIT PROFILE     [💾 Save]  │
│                                              │
│  [👤 Update Profile Photo 📷]                │
│                                              │
│  ─── CONTACT INFORMATION ────────────────    │
│  Primary Phone*                              │
│  [9876543210                            ]    │
│                                              │
│  Alternate Phone                             │
│  [9123456789                            ]    │
│                                              │
│  ─── ADDRESS INFORMATION ────────────────    │
│  Current Address                             │
│  [42, Shanti Nagar, Andheri West        ]    │
│  [Mumbai, Maharashtra                   ]    │
│                                              │
│  Pincode                                     │
│  [400058                                ]    │
│                                              │
│  [☑️] Permanent Address same as Current      │
│                                              │
│  ─── BANK INFORMATION ───────────────────    │
│  Bank Name                                   │
│  [State Bank of India                   ]    │
│                                              │
│  Account Number                              │
│  [xxxx xxxx 4321                        ]    │
│                                              │
│  IFSC Code                                   │
│  [SBIN0001234                           ]    │
│                                              │
│  UPI ID                                      │
│  [ravi.s@sbi                            ]    │
│                                              │
│  ─── UPLOAD DOCUMENTS ───────────────────    │
│  Gov ID Proof (Aadhar/PAN)                   │
│  [Filename.pdf (✓ Uploaded)]       [🖊️ Edit]  │
│                                              │
│  Address Proof                               │
│  [Filename.pdf (✓ Uploaded)]       [🖊️ Edit]  │
│                                              │
│  Other Documents (Optional)                  │
│  [+ Upload File]                             │
│                                              │
│  [Note: Changes to Bank/ID require admin     │
│   approval before taking effect]             │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │              SAVE CHANGES            │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

## Form Fields (Editable Only)

| Field | Type | Validation | Approval Required? |
| --- | --- | --- | --- |
| Profile Photo | Image Upload | JPG/PNG, Max 2MB | No |
| Primary Phone | Number | 10 digits | No |
| Alternate Phone | Number | 10 digits (Optional) | No |
| Current Address | Textarea | Max 250 chars | No |
| Pincode | Number | 6 digits | No |
| Same as Current | Checkbox | Auto-fills Permanent addr | No |
| Bank Name | Text | Match standard bank formats | Yes |
| Account Number | Number | Match bank digits | Yes |
| IFSC Code | Alphanumeric| Validate format | Yes |
| UPI ID | Text | Match format (xyz@abc) | Yes |
| Gov ID Proof | File Upload | PDF/JPG/PNG, Max 5MB | Yes |
| Address Proof | File Upload | PDF/JPG/PNG, Max 5MB | Yes |
| Other Docs | File Upload | PDF/JPG/PNG, Max 5MB | Yes |

## Actions

| Action | Behaviour |
| --- | --- |
| **Save Changes** | Validates form. Triggers Module 27 profile update flow. If structural changes (Bank/ID), triggers approval workflow for HR. Shows toast: "Profile updated successfully. Approval pending for restricted fields." Returns to View Profile (Screen 10) |
| **Cancel** | Discards changes and returns to View Profile |
| **Upload / Edit File** | Opens device file manager / camera to select a document |

---

# Screen 11: Task Detail Page

**Source Reference:** Module 21 – Task Management (21.3 Technician Screen Context), Module 18 – Customer & Asset Base, Module 22 – Live Location
**Purpose:** Detailed view of a specific assigned task, containing customer info, location, scheduled services, and call-to-actions to initiate the flow.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]      TASK DETAIL                   │
│                                              │
│  📋 TASK-2026-0201                           │
│  Status: ● Pending                           │
│                                              │
│  ─── CUSTOMER & SITE INFO ───────────────    │
│  🏢 ABC Corp — Head Office                   │
│  [📍 Open Map View]                          │
│                                              │
│  Address: Block C, Tech Park, Andheri,       │
│  Mumbai, Maharashtra 400053                  │
│                                              │
│  Contact: Amit K. (Facility Manager)         │
│  [📞 Call] [✉️ Email]                        │
│                                              │
│  ─── SCHEDULE & INSTRUCTIONS ────────────    │
│  📅 23 Mar 2026                              │
│  ⏰ 08:00 AM – 10:00 AM                      │
│                                              │
│  Special Instructions:                       │
│  "Please enter via Service Gate No 3 and     │
│   contact security at the reception."        │
│                                              │
│  ─── SERVICES TO BE PERFORMED ───────────    │
│  Service 1: Cockroach Treatment              │
│    • Kitchen Area, Cafeteria                 │
│  Service 2: Rodent Management                │
│    • Loading Bay, Basement Level 1           │
│                                              │
│  ─── TASK HISTORY (Site) ────────────────    │
│  Last Service: 15 Feb 2026 (John D.)         │
│  [📄 View Previous Report]                   │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │           🚀 START TRAVEL            │    │
│  └──────────────────────────────────────┘    │
│    (Changes to START TASK dynamically)       │
└──────────────────────────────────────────────┘
```

## Screen Components

### 11.1 Key Information Blocks

| Section | Field | Description | Source |
| --- | --- | --- | --- |
| Header | Task ID & Status | Auto-generated ID and real-time status. | Module 21 |
| Customer Info | Business & Site Name | Name of the client and specific site | Module 18 |
| Location | Address | Full physical address of the site | Module 18 |
| Map Link | [📍 Open Map View] | Opens Screen 12: Navigation Map View but purely for directions | — |
| Contact | Name & Role | Primary Point of Contact (POC) for the site | Module 18 |
| Quick Actions | Call / Email | Uses device's native dialer or email app | Device Native |
| Schedule | Date & Time | Scheduled start and end times | Module 21 |
| Instructions | Text Block | Specific notes from dispatcher/sales | Module 21/20 |
| Services | Service List | List of services (from Sales Order) mapped to specific areas (if applicable) | Module 12, 20 |
| History | Last Service | Information about previous service at this site | Module 21 |

### 11.2 Flow Control Actions

The execution flow begins here.

| Button State / Action | Trigger Condition | System Behaviour |
| --- | --- | --- |
| **🚀 START TRAVEL** | Default state when task is Pending | On tap, captures GPS location. Button disables/shows loader while verifying with Module 22 (Live Location). Sends "Travel Started" status to backend. Once confirmed, transforms into "**START TASK**". |
| **▶️ START TASK** | Appears after Start Travel is clicked and confirmed by backend | On tap, Opens **Screen 12: Navigation Map View** to confirm arrival at site geo-fence before actual task execution begins. |
| **View Previous Report** | Tap link | Opens the PDF of the last service report generated for this site. |

---

# Screen 12: Navigation Map View

**Source Reference:** Module 22 – Live Location & Travel Tracking
**Purpose:** Provide routing to the client site and verify the technician's physical presence via geo-fencing before allowing the task execution to start. **Opens when "Start Task" is tapped on Screen 11.**

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]      ROUTE TO SITE                 │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │                                      │    │
│  │                                      │    │
│  │                                      │    │
│  │        [Interactive Map View]        │    │
│  │                                      │    │
│  │              (Blue Dot)              │    │
│  │              Tech Loc                │    │
│  │                   \                  │    │
│  │                    \ (Route)         │    │
│  │                     \                │    │
│  │                   [📍 Site XYZ]      │    │
│  │                                      │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  Destination: ABC Corp — Head Office         │
│  Distance: 4.2 km  |  ETA: 15 mins           │
│                                              │
│  [↗️ Open in Google Maps]                    │
│                                              │
│  Geo-Fence Status:                           │
│  🔴 Outside 500m radius                      │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │     ☑️ CONFIRM ARRIVAL AT SITE       │    │
│  └──────────────────────────────────────┘    │
│    (Disabled if outside geo-fence)           │
└──────────────────────────────────────────────┘
```

## Screen Components

| Element | Type | Description |
| --- | --- | --- |
| Interactive Map | Map Canvas | Standard street map showing current location and destination pin |
| Tech Location | Blue Dot | Real-time GPS location of technician |
| Site Location | Red Pin (📍) | Coordinates registered for the Site in Module 18 |
| Route Overlay | Polyline | Visual route between current pos and site |
| Distance & ETA | Display | Real-time calculation |
| Open in External App | Button | Deep links to native mapping app (Google Maps / Apple Maps) passing destination coordinates |
| Geo-Fence Status | Dynamic Text | Re-evaluates distance every 10 seconds. 🔴 Outside / 🟢 Inside (e.g., `< 500m`) |

## Actions & Validation

| Action | Validation | System Behaviour |
| --- | --- | --- |
| **Confirm Arrival at Site** | **Strict Geo-Fencing validation:** User's device GPS must be within X meters (configurable in Admin, default 500m) of the Site's GPS coordinates. | **If Inside:** Captures check-in timestamp. Updates Task Status to "In Progress". Proceeds to **Screen 13: Before Task Photos** (Step 1 of Execution Flow).<br>**If Outside:** Button is disabled. If bypassed by error, shows toast: "Cannot confirm arrival. You are outside the allowed radius." |
| **Back** | — | Returns to Task Detail (Screen 11). Status remains Pending / Travel Started. |

# Screen 13: Before Task Photos

**Source Reference:** Module 21 – Task Management (Multi-Step Execution Flow)
**Purpose:** Step 1 of Execution Flow. Capture visual evidence of the site condition before beginning treatment.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]  EXECUTION: STEP 1 of 5            │
│                                              │
│  ─── BEFORE TASK PHOTOS ─────────────────    │
│                                              │
│  Capture photos of the areas before          │
│  starting the treatment.                     │
│                                              │
│  [ 📷 Add Photo (Max 5) ]                    │
│                                              │
│  Thumbnail Grid:                             │
│  ┌─────┐ ┌─────┐ ┌─────┐                     │
│  │Img 1│ │Img 2│ │Img 3│                     │
│  │ [x] │ │ [x] │ │ [x] │                     │
│  └─────┘ └─────┘ └─────┘                     │
│                                              │
│                                              │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │             PROCEED                  │    │
│  └──────────────────────────────────────┘    │
│            [Skip this step]                  │
└──────────────────────────────────────────────┘
```

## Screen Components

| Field | Type | Rules | Description |
| --- | --- | --- | --- |
| Add Photo | Button | Opens Camera / Gallery (depending on admin config) | Capture image |
| Thumbnail Grid | Image Array | Max 5 images. Each thumb has a delete [x] icon | Review captured images |
| Proceed | Button | Enabled if ≥ 1 image captured | Save to backend and go to next step |
| Skip this step | Text Link | Always enabled | Bypass photo capture and go to next step |

## Actions

| Action | Behaviour |
| --- | --- |
| **Proceed** | Uploads images associated with the Task ID to storage. Navigates to **Screen 14: Service Execution Form** (Step 2) |
| **Skip** | Bypasses uploads. Navigates to **Screen 14: Service Execution Form** (Step 2) |
| **Back** | Returns to Navigation/Detail view. Warning popup: "Are you sure? Progress will be saved." |

---

# Screen 14: Service Execution Form

**Source Reference:** Module 12 – Services, Module 11 – Stock Management (Live Deduction), Module 21 – Task Management (21.6 Material Log)
**Purpose:** Step 2 of Execution Flow. Core data entry where technicians log the specific services performed, methods used, and chemicals/materials consumed.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]  EXECUTION: STEP 2 of 5            │
│                                              │
│  ─── SERVICE DETAILS (Service 1 of 2) ───    │
│  Service Name: Cockroach Treatment           │
│                                              │
│  Performed Area*                             │
│  [☑️ Kitchen]  [☑️ Cafeteria] [ ] Stores     │
│                                              │
│  Method of Treatment*                        │
│  [☑️ Gel Baiting] [ ] Spraying [ ] Fogging   │
│                                              │
│  ─── CHEMICALS USED ────────────── [➕ Add]  │
│                                              │
│  ┌─ Chemical 1 ─────────────── [🗑️ Remove] │
│  │ Select Item*: [ Maxforce Gel 30g ▼]       │
│  │ Quantity Used*: [ 10 ]  [ Grams ▼]        │
│  │ Batch No (Auto): BT-2603-A (Qty: 500g)    │
│  └───────────────────────────────────────┘   │
│                                              │
│  ┌─ Chemical 2 ─────────────── [🗑️ Remove] │
│  │ Select Item*: [ Bifenthrin 10% ▼]         │
│  │ Quantity Used*: [ 50 ]  [ ML ▼]           │
│  │ Batch No (Auto): BT-2601-C (Qty: 2L)      │
│  └───────────────────────────────────────┘   │
│                                              │
│                                              │
│  ─── SERVICE STATUS ─────────────────────    │
│  [☑️] Mark this Service as Completed          │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │             PROCEED                  │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

## Screen Components

### 14.1 Service Definitions

| Field | Type | Validation | Source / Behaviour |
| --- | --- | --- | --- |
| Service Name | Display | Read-only | Fetch from Task data |
| Performed Area | Multi-select Chips | Yes (at least one) | List defined in Module 12 for this service |
| Method of Treatment | Multi-select Chips | Yes (at least one) | Methods mapped to the Service in Module 12 |

### 14.2 Chemicals / Material Usage Table

*Critical integration with Module 11 (Stock)*

| Field | Type | Validation | Behaviour / Source |
| --- | --- | --- | --- |
| Select Item | Dropdown | Required if chemical added | Fetches active inventory items mapped to the technician's Virtual Bin / Van Stock (Module 11) |
| Quantity Used | Numeric Input | > 0, ≤ Available stock | Entered amount |
| UoM | Dropdown | Match item UoM | Unit of measurement (Ltr, ML, Kg, Grams, Pcs) |
| Batch No & Stock Display | Display | Auto-filled | Shows FIFO / default batch number and currently available quantity in tech's bin |
| Action Buttons | Buttons | — | "[➕ Add]" adds a new row. "[🗑️ Remove]" deletes row |

**Real-Time Stock Validation:**
If Technician enters Qty Used (e.g., 600g) that exceeds their virtual bin stock for that batch (e.g., Qty: 500g) → **Immediate Inline Error: "Insufficient stock in your inventory. Available: 500g."**

### 14.3 Form Actions

| Action | Behaviour |
| --- | --- |
| **Mark as Completed** | Checkbox. Used if multiple services exist in one task mapping. |
| **Proceed** | Validates all required fields and stock availability. Temporarily saves the form state locally/draft state backend. Reduces *virtual* stock (pending final verification step). Navigates to **Screen 15: Technician Observations** (Step 3). |

---

# Screen 15: Technician Observations

**Source Reference:** Module 21 – Task Management (21.7 Observations & Notes)
**Purpose:** Step 3 of Execution Flow. Allowing the technician to log structural issues, hygiene recommendations, and pest activities identified during the site visit.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]  EXECUTION: STEP 3 of 5            │
│                                              │
│  ─── STRUCTURAL RECOMMENDATIONS ─────────    │
│  [➕ Add Structural Issue]                   │
│  ┌──────────────────────────────────────┐    │
│  │ Type*: [ Gap in Door ▼]              │    │
│  │ Area : [ Kitchen entrance    ]       │    │
│  │ [🗑️ Remove]                          │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── HYGIENE RECOMMENDATIONS ────────────    │
│  [➕ Add Hygiene Issue]                      │
│  ┌──────────────────────────────────────┐    │
│  │ Type*: [ Water accumulation ▼]       │    │
│  │ Area : [ Behind fridge       ]       │    │
│  │ [🗑️ Remove]                          │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── PEST ACTIVITY DETECTED ─────────────    │
│  [☑️] Cockroaches (High)                     │
│  [ ]  Rodents                                │
│  [ ]  Ants                                   │
│                                              │
│  ─── GENERAL REMARKS ────────────────────    │
│  [Customer advised to clear garbage daily]   │
│  [                                       ]   │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │             PROCEED                  │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

## Screen Components

### 15.1 Dynamic Observation Lists

| Category | Field | Type | Description | Source Mapping |
| --- | --- | --- | --- | --- |
| Structural | Issue Type | Dropdown | Pre-defined structural issues (e.g., crack in wall, broken pipe, missing mesh) | Module 21 Configuration |
| Structural | Area | Text Input | Specific location of the issue | Free text |
| Hygiene | Issue Type | Dropdown | Pre-defined hygiene issues (e.g., open bins, stagnant water, food debris) | Module 21 Configuration |
| Hygiene | Area | Text Input | Specific location | Free text |

*Technician can add multiple rows using the [➕ Add] button or remove them using [🗑️ Remove].*

### 15.2 Additional Logs

| Field | Type | Validation | Description |
| --- | --- | --- | --- |
| Pest Activity Detected | Multi-Checkbox + Intensity | Optional | Quick log of pests seen on site (High/Med/Low) |
| General Remarks | Textarea | Max 1000 chars | Free-text notes for internal record or customer report |

## Actions

| Action | Behaviour |
| --- | --- |
| **Proceed** | Saves observations to task draft. Navigates to **Screen 16: After Task Photos** (Step 4). |

---

# Screen 16: After Task Photos

**Source Reference:** Module 21 – Task Management (Multi-Step Execution Flow)
**Purpose:** Step 4 of Execution Flow. Capture visual evidence of the site condition after treatment is completed and/or photos of hygiene/structural issues.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]  EXECUTION: STEP 4 of 5            │
│                                              │
│  ─── AFTER TASK PHOTOS ──────────────────    │
│                                              │
│  Capture photos of the areas after           │
│  completing the treatment, or photos of      │
│  structural/hygiene issues identified.       │
│                                              │
│  [ 📷 Add Photo (Max 5) ]                    │
│                                              │
│  Thumbnail Grid:                             │
│  ┌─────┐ ┌─────┐                             │
│  │Img 1│ │Img 2│                             │
│  │ [x] │ │ [x] │                             │
│  └─────┘ └─────┘                             │
│                                              │
│                                              │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │             PROCEED                  │    │
│  └──────────────────────────────────────┘    │
│            [Skip this step]                  │
└──────────────────────────────────────────────┘
```

## Actions

| Action | Behaviour |
| --- | --- |
| **Proceed** | Uploads images associated with the Task ID to storage. Navigates to **Screen 17: Customer Verification OTP** (Step 5) |
| **Skip** | Bypasses uploads. Navigates to **Screen 17: Customer Verification OTP** (Step 5) |

---

# Screen 17: Customer Verification (OTP) & Feedback

**Source Reference:** Module 21 – Task Management (21.9 Customer Acknowledgment)
**Purpose:** Step 5 (Final Step) of Execution Flow. Capture customer sign-off using OTP sent to their registered contact, and collect rating/feedback.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]  EXECUTION: STEP 5 of 5            │
│                                              │
│  ─── CUSTOMER VERIFICATION ──────────────    │
│  An OTP has been sent to the registered      │
│  customer contact (Amit K. - 9876***210)     │
│                                              │
│  Enter 4-digit OTP*                          │
│  [ _ ] [ _ ] [ _ ] [ _ ]                     │
│                                              │
│  [Resend OTP]                                │
│                                              │
│  ─── CUSTOMER FEEDBACK ──────────────────    │
│  Service Rating*                             │
│  [★] [★] [★] [★] [☆] (4/5)                   │
│                                              │
│  Customer Remarks (Optional)                 │
│  [Good service, tech was polite.        ]    │
│  [                                      ]    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │             SUBMIT TASK              │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

## Form Fields

| Field | Type | Validation | Description |
| --- | --- | --- | --- |
| OTP Input | 4-digit | Required. Must match sent OTP | Customer provides OTP to technician |
| Resend OTP | Link | 60s cooldown | Triggers new OTP SMS/Email to customer |
| Rating | Star Rating | Required (1 to 5) | Customer satisfaction score |
| Customer Remarks | Textarea | Optional, Max 500 chars | Feedback written by customer |

## Form Actions

| Action | Behaviour |
| --- | --- |
| **Submit Task** | Validates OTP. Finalizes all data from Steps 1–5. Modifies actual Module 11 Stock records (permanent deduction). Updates Task Status to "Completed". Generates Service Report PDF. Navigates to **Screen 18: Service Report View**. |
| **Back** | Returns to Screen 16. |

---

# Screen 18: Service Report View

**Source Reference:** Module 21 – Task Management (21.10 End-of-Day/Task Completion), Module 20 – Sales Orders
**Purpose:** Final success screen. Allows technician to view the generated PDF report, download it, and share it immediately with relevant customer contacts.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Home]      TASK COMPLETED                │
│                                              │
│  🎉 Task TASK-2026-0201 has been           │
│     marked as Completed successfully!        │
│                                              │
│  ─── SERVICE REPORT ─────────────────────    │
│  ┌──────────────────────────────────────┐    │
│  │                                      │    │
│  │         [PDF Preview Area]           │    │
│  │                                      │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │          📥 DOWNLOAD PDF             │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── SHARE REPORT & DOCUMENTS ───────────    │
│  Share with:                                 │
│  [☑️] SO Contact: Amit K. (amit@abc.com)    │
│  [☑️] Contract Contact: HR (hr@abc.com)     │
│  [ ] Billing Contact: Accounts (acc@abc)     │
│                                              │
│  Attachments included:                       │
│  📄 Service Report (Current)                 │
│  📊 Previous Service Logs (Excel)             │
│  📄 Previous Service Details                 │
│  🧾 Invoice (if applicable)                  │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │               SHARE                  │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

## Actions

| Action | Source System | Behaviour |
| --- | --- | --- |
| **Download PDF** | Document Generation | Downloads the generated PDF to local device storage |
| **Share** | Module 21 / Notification Engine | Compiles selected emails and sends the standard closure email containing: Current Service Report, Previous Logs Excel (from Customer Module 18), Previous Details, and Invoice. Toast: "Documents shared successfully." |
| **Back/Home** | Navigation | Returns to Dashboard (Screen 6). |

---

# Screen 19: Notifications

**Purpose:** General alerting screen for the technician.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]      NOTIFICATIONS        [✓ Read] │
│                                              │
│  ┌─ Today ────────────────────────────────┐  │
│  │ [🟢] Task Assigned                     │  │
│  │      TASK-2026-0205 has been assigned. │  │
│  │      10 mins ago                       │  │
│  ├────────────────────────────────────────┤  │
│  │ [🔴] Leave Rejected                    │  │
│  │      CL request for 28 Mar rejected.   │  │
│  │      1 hour ago                        │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌─ Yesterday ────────────────────────────┐  │
│  │ [⚪] Task Reminder                     │  │
│  │      Remember to complete TASK-...     │  │
│  │      1 day ago                         │  │
│  └────────────────────────────────────────┘  │
└──────────────────────────────────────────────┘
```

## Behaviour

| Rule | Description |
| --- | --- |
| Read/Unread | Unread items have a bold title and colored dot. Tapping marks as read |
| Mark All Read | Action button in header |
| Click Action | Tapping notification routes to relevant screen (e.g., Task Assigned → Screen 11, Leave Update → Screen 9.1) |

---

# Screen 20: Chatbot

**Purpose:** General help and quick assistance for technicians.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]      SUPPORT CHATBOT               │
│                                              │
│  [Bot: Hello Ravi! How can I help you?]      │
│  [Bot: Here are some quick options:]         │
│                                              │
│  [ 🔘 My attendance for today  ]             │
│  [ 🔘 Show pending tasks       ]             │
│  [ 🔘 HR Policies              ]             │
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │ [User: Show pending tasks            ] │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  [Bot: You have 2 pending tasks today:    ]  │
│  [     1. TASK-0201 (08:00 AM)            ]  │
│  [     2. TASK-0202 (11:00 AM)            ]  │
│                                              │
│  [💬 Type your message here...      ] [➤]  │
└──────────────────────────────────────────────┘
```

---

# Screen 21: Petty Cash — My Requests

**Source Reference:** Module 24 – Petty Cash Management (24.1 Initiator Flow)
**Purpose:** Submitting and tracking reimbursement requests for on-field expenses (fuel, food, tolls).

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  PETTY CASH                                  │
│                                              │
│  Total Reimbursed (Mtd): ₹4,500              │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │ [All]  │  [Draft]  │  [Submitted]    │    │
│  │ [Approved]  │  [Rejected/Returned]   │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌── PC-2026-0042 ──────────────────────┐    │
│  │ Date: 22 Mar 2026                    │    │
│  │ Amount: ₹500                         │    │
│  │ Category: Fuel                       │    │
│  │ Status: ⏳ Submitted                  │    │
│  │ Current Approver: Anil K. (Manager)  │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌── PC-2026-0038 ──────────────────────┐    │
│  │ Date: 19 Mar 2026                    │    │
│  │ Amount: ₹250                         │    │
│  │ Category: Toll                       │    │
│  │ Status: ↩️ Returned (Need clear bill)│    │
│  │ Current Approver: Finance Dept       │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │         [+ ADD NEW REQUEST]          │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  [Bottom Navigation Bar]                     │
└──────────────────────────────────────────────┘
```

## Actions

| Action | Behaviour |
| --- | --- |
| **Add New Request** | Opens **Screen 21.1: Add Petty Cash Request** |
| **Tap Card** | Opens **Screen 21.2: View Request** for editing (if Draft/Returned) or viewing |

---

# Screen 21.1: Add Petty Cash Request

**Source Reference:** Module 24 – Petty Cash Management
**Purpose:** Multi-entry form to upload expense bills and submit for manager approval.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]      NEW PETTY CASH                │
│                                              │
│  ─── EXPENSE 1 ──────────────────────────    │
│  Date of Expense*                            │
│  [📅 22 Mar 2026]                            │
│                                              │
│  Category*                                   │
│  [▼ Fuel / Mileage ▼]                        │
│                                              │
│  Amount (₹)*                                 │
│  [ 500 ]                                     │
│                                              │
│  Description*                                │
│  [Travel to XYZ Site                  ]      │
│                                              │
│  Upload Bill / Receipt*                      │
│  [ 📷 Capture / Upload   ] ✓ file.jpg        │
│                                              │
│  [+ Add Another Expense]                     │
│                                              │
│  ─── SUMMARY ────────────────────────────    │
│  Total Amount: ₹500                          │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │            SAVE AS DRAFT             │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │          SUBMIT FOR APPROVAL         │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

## Validation & Rules

| Category | Description | Validation |
| --- | --- | --- |
| Editable Window | Max past days allowed | Must be ≤ Admin Config (e.g., 7 days) |
| Expense Categories | Dropdowns | Standardized in Module 24 (Fuel, Food, Tools, Hardware, etc.) |
| Attachment | Mandatory | The system mandates at least 1 image/PDF per expense entry |
| Flow | Approval | Submission routes request to Level 1 Approver (Branch Manager) based on Module 24 rules |

---

# Screen 21.2: View My Petty Cash Request

**Purpose:** Detailed view of an existing request, showing the current approval workflow state and comments.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]      REQUEST DETAILS               │
│                                              │
│  Ref: PC-2026-0038                           │
│  Status: ↩️ Returned                         │
│                                              │
│  ─── EXPENSE DETAILS ────────────────────    │
│  Date: 19 Mar 2026                           │
│  Category: Toll                              │
│  Amount: ₹250                                │
│  Description: Toll at Vashi bridge           │
│                                              │
│  Attachments:                                │
│  [📄 toll_receipt.jpg] (Tap to view)         │
│                                              │
│  ─── APPROVAL TRAIL ─────────────────────    │
│  Level 1 (Manager): ✅ Approved (Amit K.)    │
│  Level 2 (Finance): ↩️ Returned (Neha S.)    │
│  Comment: "Receipt image is too blurry to    │
│  read the amount. Please re-upload clear     │
│  picture."                                   │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │            EDIT REQUEST              │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

## Actions

| Status | Available Actions | Description |
| --- | --- | --- |
| Draft / Returned | **Edit Request** | Reopens Screen 21.1 populated with data, allowing modification and re-submission |
| Submitted / Approved| None (View Only) | Locked from edits while in approval transit |

---

# Screen 22: Settings

**Purpose:** Allow user to manage app configurations, access profile, and view help documents. Accessible from Bottom Navigation Bar.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  SETTINGS                                    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │ 👤 Profile & Account                 │    │
│  │    View and edit personal details    │    │
│  │    [ ❯ ]                             │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── APP PREFERENCES ────────────────────    │
│  ┌──────────────────────────────────────┐    │
│  │ 🌐 App Language                      │    │
│  │    Current: English                  │    │
│  │    [ ❯ ]                             │    │
│  └──────────────────────────────────────┘    │
│  ┌──────────────────────────────────────┐    │
│  │ 🔔 Notification Settings             │    │
│  │    Manage alerts and sounds          │    │
│  │    [ ❯ ]                             │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── SUPPORT ────────────────────────────    │
│  ┌──────────────────────────────────────┐    │
│  │ ❓ FAQ & Help Center                 │    │
│  │    [ ❯ ]                             │    │
│  └──────────────────────────────────────┘    │
│  ┌──────────────────────────────────────┐    │
│  │ 📄 Privacy Policy & Terms            │    │
│  │    [ ❯ ]                             │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  App Version 2.0.0                           │
│  [Bottom Navigation Bar]                     │
└──────────────────────────────────────────────┘
```

## Screen Components

| Element | Action / Destination |
| --- | --- |
| Profile & Account | Opens **Screen 10: Profile Page (View Mode)** |
| App Language | Opens modal/page to select Language (English, Hindi, regional) |
| Notification Settings | Opens OS level notification settings or internal toggles |
| FAQ & Help Center | Opens static text page with common QA or webview |
| Privacy Policy | Opens static text page with legal terms |

---

# 🔄 Complete Task Execution Flow (End-to-End)

Below is the updated standard sequence for executing a task from the mobile app.

1. **Dashboard (Screen 6)** → Technician taps a Task Card.
2. **Task Detail (Screen 11)** → Technician reviews Customer info, Address, and Instructions.
3. **Start Travel (Screen 11)** → Tech taps "Start Travel". GPS starts tracking. Backend acknowledges. Button becomes "Start Task".
4. **Start Task (Screen 11)** → Tech taps.
5. **Navigation Map (Screen 12)** → Validates 500m Geo-Fence. Tech taps "Confirm Arrival". Status = In Progress.
6. **Task Execution Step 1: Before Photos (Screen 13)** → Tech captures pre-service condition. Proceeds (or skips).
7. **Task Execution Step 2: Service Form (Screen 14)** → Tech selects service areas, methods, and chemicals used. Validates against Stock. Proceeds.
8. **Task Execution Step 3: Observations (Screen 15)** → Tech logs structural, hygiene issues, and pest activity. Proceeds.
9. **Task Execution Step 4: After Photos (Screen 16)** → Tech captures post-service condition. Proceeds (or skips).
10. **Task Execution Step 5: Verification (Screen 17)** → Customer provides OTP and Rating. Tech taps "Submit Task".
11. **Completion (Screen 18)** → Stock is permanently deducted. Report PDF generated. Tech shares report via email to configured contacts. Done.

---

# 🔐 Role-Based Access Control (RBAC) – Mobile App

| Feature / Screen | Role: Technician | Role: Senior Technician |
| --- | --- | --- |
| Login Access | Yes | Yes |
| Punch In / Out | Yes | Yes |
| Apply Leaves | Yes | Yes |
| View Assigned Tasks | Yes (Self only) | Yes (Self only) |
| Start / Execute Task | Yes | Yes |
| Add Petty Cash | Yes | Yes |
| View / Edit Profile | Yes (Self-View) | Yes (Self-View) |

---

# 🌍 Global Business Rules Summary

1. **Geo-Location Checks:** Module 25 (Attendance Check-In) and Module 22 (Task Arrival) heavily rely on device GPS. Operations are blocked if GPS is disabled or mocked.
2. **Offline Support:** If network drops during the Task Execution (Screens 13-17), data is stored locally. "Submit Task" on Screen 17 will cache the completion event and auto-sync when the network returns.
3. **Inventory Strictness:** Chemical usage in Screen 14 physically cannot exceed the technician's Virtual Bin volume at the time of entry (Module 11 integration).
4. **Leave Strictness:** Applying for leave on Screen 9.1.1 is strictly blocked if the selected leave balance is zero.
5. **Session Expiry:** App forces logout after X days (configured in Module 1) or immediately if the user is Deactivated in Module 8.

