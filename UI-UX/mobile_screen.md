# 📱 Field Technician / Employee Mobile Application — Screen-Wise Functional Document

**Version:** 1.0
**Date:** March 2026
**Audience:** Development Team, QA Team, UI/UX Designers, Product Managers
**Purpose:** End-user functional documentation for the mobile application used by Field Technicians and Employees of Pest Control companies onboarded to the Seravion ERP platform.

---

## Table of Contents

1. [Screen 1: Login](#screen-1-login)
2. [Screen 1.1: Forgot Password](#screen-11-forgot-password)
3. [Screen 2: Home Page (Dashboard)](#screen-2-home-page-dashboard)
4. [Screen 3: Bottom Navigation Bar](#screen-3-bottom-navigation-bar)
5. [Screen 4: Services (Tasks) Page](#screen-4-services-tasks-page)
6. [Screen 5: Calendar Page (Attendance View)](#screen-5-calendar-page-attendance-view)
7. [Screen 6: Leave Module](#screen-6-leave-module)
8. [Screen 6.1: Apply Leave Form](#screen-61-apply-leave-form)
9. [Screen 7: Profile Page](#screen-7-profile-page)
10. [Screen 8: Task Detail Page](#screen-8-task-detail-page)
11. [Screen 9: Navigation Map View](#screen-9-navigation-map-view)
12. [Screen 10: Start Task – Selfie Capture](#screen-10-start-task--selfie-capture)
13. [Screen 10.1: Safety Instructions Popup](#screen-101-safety-instructions-popup)
14. [Screen 11: Before Service Photo Upload](#screen-11-before-service-photo-upload)
15. [Screen 12: After Service Photo Upload](#screen-12-after-service-photo-upload)
16. [Screen 13: Service Execution Form](#screen-13-service-execution-form)
17. [Screen 14: Technician Observations](#screen-14-technician-observations)
18. [Screen 15: Additional Details & Notes](#screen-15-additional-details--notes)
19. [Screen 16: Customer Verification (OTP) & Service Submission](#screen-16-customer-verification-otp--service-submission)
20. [Screen 17: Service Report View](#screen-17-service-report-view)
21. [Screen 18: Notifications](#screen-18-notifications)
22. [Screen 19: Chatbot](#screen-19-chatbot)
23. [Screen 20: Petty Cash — My Requests](#screen-20-petty-cash--my-requests)
24. [Screen 20.1: Add Petty Cash Request](#screen-201-add-petty-cash-request)
25. [Screen 20.2: View My Petty Cash Request (Read-Only)](#screen-202-view-my-petty-cash-request-read-only)

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
| Profile & Salary | Module 25 – HRM (Employee, Salary), Module 6 (Salary & Leave Config), Module 8 (Employee Master) |
| Petty Cash | Module 24 – Petty Cash Management, Module 8 (Employee — bank/UPI details), Module 7 (Branch) |

---

# Screen 1: Login

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
│     App Version: v1.0.0                    │
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
| Forgot Password | Text Link | — | — | Navigates to Forgot Password screen (1.1) |

## Actions

| Action | Trigger | System Behaviour |
| --- | --- | --- |
| **Sign In** | Tap "SIGN IN" button | Validates Account ID + Password against IAM module. On success → redirects to Home Dashboard (Screen 2). On failure → shows inline error: "Invalid Account ID or Password." |
| **Forgot Password** | Tap link | Opens Forgot Password screen (Screen 1.1) |

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

# Screen 1.1: Forgot Password

**Source Reference:** Module 1 – IAM
**Purpose:** Allow the user to reset their password by receiving a reset link or OTP on their registered email/phone.

---

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
| **Send Reset OTP** | Tap button | Validates the input against IAM records. If match found → sends OTP to registered email AND phone. Shows success toast: "OTP sent to your registered email / phone." If no match → error: "No account found with the provided details." |
| **Back** | Tap ← icon | Returns to Login screen (Screen 1) |

## Error Messages

| Condition | Message |
| --- | --- |
| Empty field | "Please enter your Account ID, Email, or Phone Number." |
| No match found | "No account found with the provided details. Please check and try again." |
| Invalid phone format | "Please enter a valid 10-digit phone number." |
| OTP send failure | "Failed to send OTP. Please try again." |

---

# Screen 2: Home Page (Dashboard)

**Source Reference:** Module 21 – Task Management (21.2 Daily Task View), Module 25 – HRM (Attendance)
**Purpose:** Central hub for the field technician. Shows attendance controls, today's assigned tasks, and quick-access navigation.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [👤 Avatar] Good Morning, Ravi S.    [🔔 3] │
│  Branch: Mumbai | Role: Technician           │
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
│  │  ● Pending          [📍 View Map]    │    │
│  └──────────────────────────────────────┘    │
│  ┌──────────────────────────────────────┐    │
│  │  📋 TASK-2026-0202                   │    │
│  │  🏢 PQR Foods — Warehouse            │    │
│  │  🐀 Rodent Management                │    │
│  │  ⏰ 11:00 – 13:00 PM                │    │
│  │  ✅ Completed    [📄 Service Report]  │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  [💬 Chatbot]                                │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │ 🏠 Home │ 📋 Services │ 📅 Calendar │    │
│  │ 🏖️ Leave │ 👤 Profile                │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

## Screen Components

### 2.1 Header Section

| Element | Type | Description |
| --- | --- | --- |
| Avatar / Profile Picture | Image | User's profile photo (from Module 8 Employee record). Tap → opens Profile (Screen 7) |
| Greeting | Text | Dynamic greeting (Good Morning/Afternoon/Evening) + Employee Name |
| Notification Bell (🔔) | Icon + Badge | Shows unread notification count. Tap → opens Notifications screen (Screen 18) |
| Branch & Role | Text (Read-only) | Current branch and role assignment from Module 8 |

### 2.2 Attendance Section

| Field | Type | Behaviour | Source |
| --- | --- | --- | --- |
| Today's Date | Display | Auto-populated with current date | System |
| Status Badge | Badge | 🟢 Checked In / 🔴 Not Checked In / ⚪ Checked Out | Module 25 Attendance |
| Punch In | Slider | Swipe-to-confirm slider to record Check-In time. GPS location is captured automatically. Once punched in, this shows the recorded time as read-only | Module 25 – Attendance (25.3.1) |
| Punch Out | Slider | Swipe-to-confirm slider to record Check-Out time. GPS location is captured automatically. Available only after Punch In | Module 25 – Attendance (25.3.1) |
| Total Hours | Display (Auto) | Auto-calculated from Punch In to current time (running) or Punch Out (final) | System calculated |

**Attendance Business Rules (from Module 25):**

| Rule | Description |
| --- | --- |
| GPS Mandatory | Punch In/Out captures GPS coordinates automatically |
| One Punch-In Per Day | Only one Punch In allowed per day |
| Punch Out After Punch In | Punch Out slider only appears after Punch In is recorded |
| Late Marking | If Punch In is after the designated shift start time (from Module 6 config), the system marks the day as "Late" |
| Background Tracking | GPS tracking for Module 22 (Live Location) starts only after Punch In and stops after Punch Out |

### 2.3 Today's Tasks Section

| Element | Type | Description |
| --- | --- | --- |
| Section Header | Text | "TODAY'S TASKS" with count badge |
| View All Link | Text Link | Navigates to Services (Tasks) Page (Screen 4) showing all tasks |
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
| View Map | Button | Opens Navigation Map View (Screen 9) — visible only if status ≠ Completed | Module 22 |
| Service Report | Button | Opens Service Report View (Screen 17) — visible only if status = Completed | Module 21 (21.6) |

**Task Card Tap Action:** Tapping anywhere on a task card (except View Map / Service Report buttons) → opens Task Detail Page (Screen 8).

### 2.4 Chatbot Icon

| Element | Type | Description |
| --- | --- | --- |
| Chatbot FAB | Floating Action Button | Fixed at bottom-right corner. Tap → opens Chatbot screen (Screen 19) |

---

# Screen 3: Bottom Navigation Bar

**Purpose:** Persistent navigation across all screens of the app. Always visible at the bottom of the screen.

---

## Navigation Items

| Tab | Icon | Label | Destination | Badge |
| --- | --- | --- | --- | --- |
| Home | 🏠 | Home | Screen 2: Dashboard | — |
| Services | 📋 | Services | Screen 4: Services (Tasks) Page | Count of pending tasks |
| Calendar | 📅 | Calendar | Screen 5: Calendar Page | — |
| Leave | 🏖️ | Leave | Screen 6: Leave Module | Count of pending leave requests |
| Petty Cash | 💰 | Petty Cash | Screen 20: Petty Cash — My Requests | Count of Draft / Returned requests |
| Profile | 👤 | Profile | Screen 7: Profile Page | — |
* Profile not in bottom - it will set on header to open profile.
## Behaviour

| Rule | Description |
| --- | --- |
| Active State | The currently active tab is highlighted with the primary accent color |
| Persistent | Navigation bar is visible on all main screens (hidden during task execution flow: Screens 10–16) |
| Badge Counts | Real-time badge counts update from backend |

---

# Screen 4: Services (Tasks) Page

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
│  │  Priority: 🔴 Urgent    [📍 Map]     │    │
│  └──────────────────────────────────────┘    │
│  ┌──────────────────────────────────────┐    │
│  │  📋 TASK-2026-0203                   │    │
│  │  🏢 XYZ Hotel — Lobby Area           │    │
│  │  🐜 Ant Treatment                    │    │
│  │  ⏰ 14:00 – 16:00   ● Pending       │    │
│  │  Priority: 🟢 Normal    [📍 Map]     │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── 24 Mar 2026 (Tomorrow) ────────────     │
│  ┌──────────────────────────────────────┐    │
│  │  📋 TASK-2026-0210                   │    │
│  │  🏢 LMN Pvt — Factory               │    │
│  │  🐀 Rodent Control                   │    │
│  │  ⏰ 09:00 – 11:00   ● Pending       │    │
│  │  Priority: 🟡 High      [📍 Map]     │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  [Bottom Navigation Bar]                     │
└──────────────────────────────────────────────┘
```

## Screen Components

### 4.1 Header

| Element | Type | Description |
| --- | --- | --- |
| Title | Text | "SERVICES" |
| Search Icon (🔍) | Icon | Opens search bar for searching by Task ID, Customer Name, or Service Type |
| Filter Icon (🔽) | Icon | Opens filter panel with dropdown options |

### 4.2 Tabs

| Tab | Description | Content |
| --- | --- | --- |
| **Assigned** | Active / Pending tasks | Shows tasks with status: Pending, In Progress. Grouped by date. Count badge shows total |
| **Completed** | Finished tasks | Shows tasks with status: Completed. Each card shows "Service Report" button |

### 4.3 Filters

| Filter | Type | Options | Source |
| --- | --- | --- | --- |
| Date Range | Date Picker | Select From – To date range | — |
| Priority | Dropdown | All / Normal / High / Urgent / Critical | Module 21 |
| Service Category | Dropdown | All / General Pest / Specialized / Fumigation etc. | Module 12 |
| Status | Dropdown | All / Pending / In Progress / Overdue (in Assigned tab) | Module 21 |

### 4.4 Task Cards

Same card structure as described in [Screen 2 → Section 2.3](#23-todays-tasks-section), with the addition of:

| Extra Field | Type | Description |
| --- | --- | --- |
| Priority Badge | Badge | 🔴 Urgent / 🟡 High / 🟢 Normal — from Module 21 |
| Date Group Header | Section Header | Tasks grouped by scheduled date (e.g., "23 Mar 2026 (Today)", "24 Mar 2026 (Tomorrow)", "25 Mar 2026 (Tuesday)"). Displays full date with day label. For today and tomorrow, special text is appended in brackets. Past dates show the weekday name. Each group header separates tasks visually with a horizontal rule and bold date text. The grouped tasks can be collapsed/expanded by tapping on the header. If no tasks exist for a date within the filtered range, the group header is completely hidden from the view to save screen space |

**Card Tap Action:** Opens Task Detail Page (Screen 8).

---

# Screen 5: Calendar Page (Attendance View)

**Source Reference:** Module 25 – HRM (25.3 Attendance Calendar), Module 21 – Task Management (21.1 Calendar Dashboard)
**Purpose:** Visual calendar showing the technician's attendance records and task schedule overlaid on a monthly/weekly grid.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  CALENDAR                                    │
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

### 5.1 Header & Calendar Grid

| Element / Field | Type | Description |
| --- | --- | --- |
| Page Header | Header | "CALENDAR" title bar showing the current viewing month |
| Month Navigation | Arrows (◀ ▶) | Navigate to previous/next month |
| View Toggle | Dropdown | Monthly / Weekly view |
| Date Cells | Tap-able Grid | Each cell shows attendance status (color) and task count badge |

### 5.2 Calendar Cell Indicators

| Indicator | Color | Meaning | Source |
| --- | --- | --- | --- |
| 🟢 Present | Green | Employee was/is present (Punch In recorded) | Module 25 – Attendance |
| 🔴 Leave | Red | Approved leave on that date | Module 25 – Leave |
| 🟡 Late | Yellow | Employee punched in after shift start time | Module 25 – Attendance |
| ⬜ Week Off | Grey | Weekend / configured weekly off | Module 6 – Leave Config |
| ⚪ Absent | White/Empty | No attendance record (workday with no Punch In) | Module 25 – Attendance |
| Task Count (₃) | Number Badge | Number of tasks scheduled on that date | Module 21 |

### 5.3 Day Detail Section (Below Calendar)

Appears when a date cell is tapped:

| Field | Type | Description |
| --- | --- | --- |
| Date | Display | Selected date |
| Attendance Status | Badge | Present / Absent / Leave / Late / Week Off |
| Punch In Time | Display | Recorded check-in time (from Module 25) |
| Punch Out Time | Display | Recorded check-out time (from Module 25) |
| Task List | Scrollable Cards | Compact task cards for the selected day (Task ID, Customer, Time, Status) |

**Task Card Tap:** Opens Task Detail Page (Screen 8).

---

# Screen 6: Leave Module

**Source Reference:** Module 25 – HRM (25.4 Leave Application, 25.5 Leave Requests)
**Purpose:** View leave history and apply for new leave requests.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  LEAVE                           [🔍] [⚙️]   │
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
│  │  Reason: Family function             │    │
│  │  Status: ⏳ Pending                   │    │
│  │  Applied: 23 Mar 2026, 10:30 AM      │    │
│  └──────────────────────────────────────┘    │
│  ┌──────────────────────────────────────┐    │
│  │  🤒 Sick Leave                        │    │
│  │  24 Mar 2026 (1 day)                  │    │
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

### 6.1 Header & Top Controls

| Element / Field | Type | Description |
| --- | --- | --- |
| Page Header | Header | "LEAVE" title bar |
| Search Icon (🔍) | Icon | Tap to search leave records by reason or date |
| Filter Icon (⚙️) | Icon | Tap to open advanced filter options |

### 6.2 Leave Balance Summary

| Field | Type | Description | Source |
| --- | --- | --- | --- |
| Casual Leave (CL) | Display | Used / Total allocation | Module 6 → Module 8 → Module 25 |
| Sick Leave (SL) | Display | Used / Total allocation | Module 6 → Module 8 → Module 25 |
| Paid Leave (PL) | Display | Used / Total allocation | Module 6 → Module 8 → Module 25 |

### 6.3 Leave Filters (Tabs)

| Tab | Filter |
| --- | --- |
| All | Shows all leave records |
| Pending | Status = Pending (awaiting approval) |
| Approved | Status = Approved |
| Rejected | Status = Rejected |

### 6.3 Leave Card Fields

| Field | Type | Description |
| --- | --- | --- |
| Leave Type | Badge | CL / SL / PL with icon |
| Date Range | Display | From Date – To Date |
| Days Count | Display | Total leave days (excluding week offs & holidays) |
| Reason | Display | Employee's leave description |
| Status | Badge | ⏳ Pending / ✅ Approved / ❌ Rejected |
| Applied Date | Display | Submission timestamp |
| Rejection Reason | Display (conditional) | Shown only if Status = Rejected |

### 6.4 Apply Leave Button

| Action | Trigger | Destination |
| --- | --- | --- |
| Apply Leave | Tap "+ APPLY LEAVE" button | Opens Apply Leave Form (Screen 6.1) |

---

# Screen 6.1: Apply Leave Form

**Source Reference:** Module 25 – HRM (25.4 Leave Application — Employee Self-Service Flow 4)
**Purpose:** Form for the technician to submit a new leave request.

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

## Validation Rules

| Rule | Description |
| --- | --- |
| Balance Check | If requested days exceed remaining balance for selected leave type → Warning: "Insufficient leave balance. You have X days remaining." |
| Overlap Check | If dates overlap with already approved leave → Error: "Leave already exists for selected dates." |
| Past Date | From Date cannot be in the past |
| Holiday Check | Week offs and public holidays are excluded from Total Days calculation |

## Actions

| Action | System Behaviour |
| --- | --- |
| **Submit Request** | Validates form → Creates leave request with Status = Pending → Appears in Module 25 Tab 2 (Leave Requests) for HR/Manager approval → Shows success toast: "Leave request submitted successfully." → Returns to Leave Module (Screen 6) |
| **Cancel** | Discards form and returns to Leave Module (Screen 6) |

---

# Screen 7: Profile Page (View Mode)

**Source Reference:** Module 27 – User Profile
**Purpose:** View a comprehensive, read-only 360-degree profile of the logged-in employee (Self-View). Structured into 7 standard sections matching the web ERP (Module 27.1).

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  PROFILE                          [✏️ Edit]  │
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
│                                              │
│  [Bottom Navigation Bar]                     │
└──────────────────────────────────────────────┘
```

## Screen Components

| Element / Field | Type | Description |
| --- | --- | --- |
| Page Header | Header | "PROFILE" title with Edit button |
| 1. Basic Info | Section (Read-only) | Profile Photo, EMP ID, Full Name, Account ID, Email, Contact No, Alt No, Status, Date of Joining, Employment Type |
| 2. Org Info | Section (Read-only) | Department, Designation, Role, Branch, Reporting Manager |
| 3. Address Info | Section (Read-only) | Current and Permanent Address (Line 1/2, City, State, Country, Pincode) |
| 4. Salary Info | Section (Read-only) | Salary Type, Basic, HRA, Allowances, Incentives, Deductions, Net Salary. **Hidden** if user lacks salary view permissions |
| 5. Bank Info | Section (Read-only) | Bank Name, Masked Account Number, Account Holder, IFSC Code, UPI ID |
| 6. Documents | Section (Read-only) | List of uploaded documents (Gov ID, Address, Contract) with Download 📥 and View 👁 actions |
| 7. Leave Summary | Section (Read-only) | CL, SL, PL balances (Used/Total), Total Leaves Taken |

## Actions

| Action | Behaviour |
| --- | --- |
| **Edit** | Opens **Screen 7.1: Edit Profile** (Self-Edit mode) |
| **Download Salary Slip** | Downloads the latest month's PDF salary slip |
| **Logout** | Confirmation popup: "Are you sure you want to logout?" → On confirm: Clears session, stops GPS tracking, redirects to Login (Screen 1) |

---

# Screen 7.1: Edit Profile

**Source Reference:** Module 27 – User Profile (Edit Mode)
**Purpose:** Allows the logged-in user to update their own contact information, address, bank details, and upload missing documents (Self-Edit Rules).

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Cancel]       EDIT PROFILE     [💾 Save] │
│                                              │
│  ┌─ 1. BASIC USER INFORMATION ────────────┐  │
│  │  [👤 Photo] [📤 Change Photo]          │  │
│  │  First Name: Ravi (read-only)          │  │
│  │  Last Name : Sharma (read-only)        │  │
│  │  Email: ravi.s@company (read-only)     │  │
│  │  Contact No*: [9876543210________]     │  │
│  │  Alt No     : [9123456789________]     │  │
│  │  Account ID: ravi.s (read-only)        │  │
│  │  Status: 🟢 Active (read-only)         │  │
│  └────────────────────────────────────────┘  │
│  (Org, Salary, Leave sections read-only)     │
│                                              │
│  ┌─ 3. ADDRESS INFORMATION ───────────────┐  │
│  │  ── Current Address ──                 │  │
│  │  Address Line 1*:                      │  │
│  │  [42, Shanti Nagar, Andheri West___]   │  │
│  │  Address Line 2:                       │  │
│  │  [Near City Mall__________________]    │  │
│  │  City*:    [Mumbai___________]         │  │
│  │  State*:   [▼ Maharashtra ▼__]         │  │
│  │  Country*: [▼ India ▼________]         │  │
│  │  Pincode*: [400058___________]         │  │
│  │                                        │  │
│  │  [☑] Same as Current Address           │  │
│  │                                        │  │
│  │  ── Permanent Address ── (disabled)    │  │
│  │  (Auto-filled from Current Address)    │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌─ 5. BANK INFORMATION ──────────────────┐  │
│  │  Bank Name*  : [State Bank of India]   │  │
│  │  Account No* : [123456784321_______]   │  │
│  │  Holder Name*: [Ravi Sharma________]   │  │
│  │  IFSC Code*  : [SBIN0001234________]   │  │
│  │  UPI ID      : [ravi.s@sbi_________]   │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌─ 6. DOCUMENTS ─────────────────────────┐  │
│  │  Gov ID Proof       : ✅ Uploaded [👁]  │  │
│  │  Address Proof      : ✅ Uploaded [👁]  │  │
│  │  Education Certs    : ✅ Uploaded [👁]  │  │
│  │  Other Docs         : ❌ Pending        │  │
│  │  [📤 UPLOAD NEW DOCUMENT]               │  │
│  └────────────────────────────────────────┘  │
│  ⚠ Employment Contract cannot be uploaded   │
│    by self — managed by HR only.            │
└──────────────────────────────────────────────┘
```

## Form Fields (Self-Edit Permitted Fields)

> Matches **Module 27.2 Self-Edit Rules**. Only the following fields are editable by the employee. All other fields remain read-only.

| Section | Editable Fields | Read-Only Fields |
| --- | --- | --- |
| **1. Basic Info** | Profile Photo, Contact Number, Alternate Number | EMP ID, First Name, Last Name, Full Name, Email, Account ID, Password, Status, Date of Joining, Employment Type |
| **2. Org Info** | None (Read-only) | Department, Designation, Role, Branch, Reporting Manager, App User |
| **3. Address Info** | Current & Permanent Address (Line 1, Line 2, City, State, Country, Pincode), "Same as Current" checkbox | — |
| **4. Salary Info** | None (Read-only, view own salary only) | All salary fields |
| **5. Bank Info** | Bank Name, Account Number (unmasked in edit), Account Holder Name, IFSC Code, UPI ID | — |
| **6. Documents** | Upload: Gov ID Proof, Address Proof, Education Certs, Other Docs | Employment Contract (HR/Admin only — cannot be uploaded by self) |
| **7. Leave Summary** | None (Read-only) | All leave fields (managed via Module 25) |

---

## Validation Rules (Self-Edit Mode)

| Field | Validation |
| --- | --- |
| Profile Photo | JPG/PNG only, Max 2MB |
| Contact Number | Must be valid 10-digit Indian mobile number |
| Alternate Number | Must be valid 10-digit Indian mobile number (if provided) |
| Address Line 1 | Min 5 chars, Max 200 chars. Required |
| Address Line 2 | Max 200 chars. Optional |
| City | Min 2 chars, alphabets. Required |
| State | Must be from predefined list. Required |
| Country | Default: India. Required |
| Pincode | Must be 6-digit numeric. Required |
| Bank Name | Min 3 chars. Required |
| Account Number | Numeric. Required |
| Account Holder Name | Min 2 chars. Required |
| IFSC Code | Must be 11-character alphanumeric. Required |
| UPI ID | Must follow `name@bank` format (if provided). Optional |
| Documents | PDF/JPG/PNG only, Max 5MB per file |

## Actions

| Action | Behaviour |
| --- | --- |
| **Upload Document** | Opens standard mobile file picker / camera. Validates file size. Uploads file with status indicating success |
| **Save** | Validates all editable fields → Saves to database → Updates profile → Returns to Screen 7 with success toast: "Profile updated successfully." |
| **Cancel** | Discards changes → Returns to Screen 7 |

---

# Screen 8: Task Detail Page

**Source Reference:** Module 21 – Task Management (21.3 Task Detail View, 21.5 Edit Task)
**Purpose:** Full details of a selected task including customer information, service details, scheduling, and action buttons.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]   TASK-2026-0201      [📍 View Map]│
│                                              │
│  Status: ● Pending                           │
│  Priority: 🔴 Urgent    Type: Normal Task     │
│                                              │
│  ─── CUSTOMER DETAILS ───────────────────    │
│  ┌──────────────────────────────────────┐    │
│  │  Customer  : ABC Corporation         │    │
│  │  Site      : Head Office, Andheri    │    │
│  │  Contact   : Su**** (Site Contact)   │    │
│  │  Phone     : 98XXXX3210 (masked)     │    │
│  │  SO No     : SO-2026-00101           │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── SERVICE DETAILS ────────────────────    │
│  ┌──────────────────────────────────────┐    │
│  │  Category   : General Pest Control   │    │
│  │  Subcategory: Cockroach Management   │    │
│  │  Service    : Cockroach Treatment    │    │
│  │  Mode       : 📦 Contract Base       │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── SCHEDULE ───────────────────────────    │
│  ┌──────────────────────────────────────┐    │
│  │  Date      : 23 Mar 2026            │    │
│  │  Time Slot : 08:00 – 10:00 AM       │    │
│  │  Primary   : Ravi S. (You)          │    │
│  │  Support   : Anjali M.              │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── MATERIALS / CHEMICALS ──────────────    │
│  ┌──────────────────────────────────────┐    │
│  │  Chemical         │ UOM  │ Required  │    │
│  │───────────────────┼──────┼──────────│    │
│  │  Alpha Cypermethrin│ ml   │ 120 ml   │    │
│  │  Fipronil Gel     │ tube │ 2 tubes  │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── TASK NOTES ─────────────────────────    │
│  "Focus on server room and storage area.     │
│   Customer has reported repeated sightings." │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │         [▶ START TASK]               │    │
│  └──────────────────────────────────────┘    │
│                                              │
└──────────────────────────────────────────────┘
```

## Screen Components

### 8.1 Task Header

| Element / Field | Type | Description | Source |
| --- | --- | --- | --- |
| Page Header | Header | "TASK-YYYY-NNNN" title with Back and View Map buttons | — |
| Task ID | Display | Unique task identifier | Module 21 |
| Status | Badge | Pending / In Progress / Completed / Overdue | Module 21 |
| Priority | Badge | Normal / High / Urgent / Critical | Module 21 |
| Task Type | Badge | Normal / Re-Task | Module 21 |
| View Map | Button | Opens Navigation Map View (Screen 9) | Module 22 |

### 8.2 Customer Details (Partially Masked)

| Field | Type | Description | Source |
| --- | --- | --- | --- |
| Customer Name | Display | Full customer business name | Module 18 → Module 21 |
| Site Name & Address | Display | Service location | Module 21 |
| Site Contact | Display (Masked) | Contact person name — partially masked for privacy | Module 21 |
| Contact Phone | Display (Masked) | Phone number — partially masked (98XXXX3210) | Module 21 |
| SO Number | Display | Linked Sales Order reference | Module 20 → Module 21 |

### 8.3 Service Details

| Field | Type | Description | Source |
| --- | --- | --- | --- |
| Category | Display | Service Category | Module 12 → Module 21 |
| Subcategory | Display | Service Subcategory | Module 12 → Module 21 |
| Service | Display | Specific service name | Module 12 → Module 21 |
| Service Mode | Badge | Contract Base / One-Time | Module 12 |

### 8.4 Schedule & Assignment

| Field | Type | Description | Source |
| --- | --- | --- | --- |
| Scheduled Date | Display | Task execution date | Module 21 |
| Time Slot | Display | Start – End time | Module 21 |
| Primary Technician | Display | Main responsible technician (highlighted if it's the current user) | Module 21 |
| Support Technicians | Display | Additional technicians (if any) | Module 21 |

### 8.5 Materials / Chemicals (Read-only)

| Field | Type | Description | Source |
| --- | --- | --- | --- |
| Chemical Name | Display | Name of chemical/product required | Module 12 → Module 21 |
| UOM | Display | Unit of Measure (ml, tube, kg, etc.) | Module 10 |
| Required Quantity | Display | Pre-assigned quantity from task creation | Module 21 |

### 8.6 Task Notes

| Field | Type | Description |
| --- | --- | --- |
| Task Notes | Display (Read-only) | Instructions from Technician Manager. Max 500 chars |

## Actions

| Button | Condition | Behaviour |
| --- | --- | --- |
| **Start Task** | Visible only when Status = Pending and current time is within ±30 min of scheduled start | Initiates task execution flow → Opens Selfie Capture (Screen 10) |
| **View Map** | Visible when Status ≠ Completed | Opens Navigation Map (Screen 9) |
| **Service Report** | Visible only when Status = Completed | Opens Service Report View (Screen 17) |

## Business Rules

| Rule | Description |
| --- | --- |
| Start Window | "Start Task" button is only enabled within ±30 minutes of the scheduled start time |
| GPS Prerequisite | Technician must have active Punch In (attendance) before starting a task |
| Read-Only After Completion | Once Status = Completed, all fields are read-only and "Service Report" replaces "Start Task" |
| Re-Task Indicator | If Task Type = Re-Task, a banner shows: "⚠️ This is a Re-Task linked to Ticket TKT-YYYY-NNNN" |

---

# Screen 9: Navigation Map View

**Source Reference:** Module 22 – Live Location & Travel Tracking
**Purpose:** Provide turn-by-turn navigation to the task site, showing the technician's current GPS location and the destination pin.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]    NAVIGATE TO SITE                │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │                                      │    │
│  │          [ INTERACTIVE MAP ]         │    │
│  │                                      │    │
│  │     📍(You)                          │    │
│  │       \                              │    │
│  │        \  ← Route line               │    │
│  │         \                            │    │
│  │          🏢(Destination)             │    │
│  │                                      │    │
│  │  ETA: 25 min | Distance: 12.3 km    │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── DESTINATION DETAILS ────────────────    │
│  ┌──────────────────────────────────────┐    │
│  │  🏢 ABC Corp — Head Office           │    │
│  │  📍 Andheri West, Mumbai 400058      │    │
│  │  👤 Su**** (Contact Person)          │    │
│  │  📱 98XXXX3210                       │    │
│  │  [📞 Call Contact]  [📱 Open in Maps]│    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

## Screen Components

| Element / Field | Type | Description |
| --- | --- | --- |
| Page Header | Header | "NAVIGATE TO SITE" title with back button |
| Map | Interactive Map View | Shows real-time GPS position of technician and destination pin with route line |
| ETA | Display (Auto) | Estimated Time of Arrival (system-calculated) |
| Distance | Display (Auto) | Route distance in km |
| Site Name | Display | Destination site name from Module 21 |
| Address | Display | Full address of the site |
| Contact Person | Display (Masked) | Site contact name (partially masked) |
| Phone | Display (Masked) | Contact phone (partially masked) |
| Call Contact | Button | Opens phone dialer with the contact number |
| Open in Maps | Button | Opens the destination in the device's native maps app (Google Maps / Apple Maps) |

## Business Rules

| Rule | Description |
| --- | --- |
| GPS Tracking | Technician's location is continuously sent to backend (Module 22 polling rate) |
| Auto-Arrival | When GPS detects proximity to site coordinates (geofence), system logs "Arrived" event automatically |
| Background Mode | Map navigation continues even if app goes to background |

---

# Screen 10: Start Task – Selfie Capture

**Source Reference:** Module 21 – Task Execution Flow (Step 3 from mobile_screen requirements)
**Purpose:** Verify technician identity at the site by capturing a selfie before starting the service.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Cancel]     VERIFY IDENTITY              │
│                                              │
│  Please take a selfie to verify your         │
│  presence at the service location.           │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │                                      │    │
│  │                                      │    │
│  │         [ CAMERA VIEWFINDER ]        │    │
│  │         (Front Camera Active)        │    │
│  │                                      │    │
│  │                                      │    │
│  │  📍 Current Location: Andheri West   │    │
│  │  ⏰ Time: 08:02 AM, 23 Mar 2026     │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │           [📸 CAPTURE]               │    │
│  └──────────────────────────────────────┘    │
│                                              │
└──────────────────────────────────────────────┘
```

## Screen Fields

| Element / Field | Type | Description |
| --- | --- | --- |
| Page Header | Header | "VERIFY IDENTITY" title with Cancel button |
| Camera Viewfinder | Live Camera (Front) | Front-facing camera is activated automatically |
| GPS Location | Display (Auto) | Current GPS location shown as overlay on camera |
| Timestamp | Display (Auto) | Current date and time stamped on the photo |

## Actions

| Action | Trigger | System Behaviour |
| --- | --- | --- |
| **Capture** | Tap 📸 button | Takes selfie → Embeds GPS + Timestamp metadata → Shows preview with [Retake] and [Continue] options |
| **Retake** | Tap after capture | Clears photo and returns to camera viewfinder |
| **Continue** | Tap after capture | Saves selfie → Opens Safety Instructions Popup (Screen 10.1) |
| **Cancel** | Tap ← Cancel | Confirmation dialog: "Cancel task start?" → Returns to Task Detail (Screen 8) |

## Business Rules

| Rule | Description |
| --- | --- |
| Front Camera Only | Only front-facing (selfie) camera is allowed |
| GPS Mandatory | Selfie must have GPS coordinates embedded; if GPS is off → shows error: "Please enable location services to continue." |
| Single Photo | Only one selfie is captured (no multi-photo) |
| Anti-Spoofing | Photo must be a live capture (no gallery upload allowed) |

---

# Screen 10.1: Safety Instructions Popup

**Purpose:** Display mandatory safety reminders before the technician begins the pest control service.

---

## Popup Layout

```
┌──────────────────────────────────────────┐
│        ⚠️ SAFETY INSTRUCTIONS             │
│                                          │
│  Before you begin, ensure you have:      │
│                                          │
│  ✅ Worn protective gloves               │
│  ✅ Worn face mask / respirator          │
│  ✅ Worn safety goggles (if required)    │
│  ✅ Proper service kit ready             │
│  ✅ Checked chemical labels & expiry     │
│  ✅ Informed the site contact            │
│                                          │
│  ⏱ Auto-dismiss in: 10 seconds          │
│                                          │
│  ┌──────────────────────────────────┐    │
│  │     [✓ I UNDERSTAND, PROCEED]    │    │
│  └──────────────────────────────────┘    │
│  (Button enabled after 10 seconds)       │
└──────────────────────────────────────────┘
```

## Behaviour

| Rule | Description |
| --- | --- |
| Mandatory Display | Popup is non-dismissable for 10 seconds (countdown timer shown) |
| Proceed Button | "I Understand, Proceed" button is disabled for 10 seconds, then becomes active |
| On Proceed | Navigates to Before Service Photo Upload (Screen 11) |
| Task Status Update | On proceed, task status changes from "Pending" → "In Progress" in Module 21 |

---

# Screen 11: Before Service Photo Upload

**Source Reference:** Module 21 – Task Completion (21.6 – Before Photo requirement)
**Purpose:** Capture mandatory photographs of the site/area BEFORE the pest control service begins.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]      BEFORE SERVICE PHOTOS         │
│                                              │
│  Upload photos of the service area before    │
│  starting treatment. (Min 1, Max 5)          │
│                                              │
│  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐    │
│  │ 📸   │  │ 📸   │  │ 📸   │  │  +   │    │
│  │Photo1│  │Photo2│  │      │  │ Add  │    │
│  │  ✅  │  │  ✅  │  │      │  │      │    │
│  └──────┘  └──────┘  └──────┘  └──────┘    │
│                                              │
│  Photos uploaded: 2 / 5                      │
│                                              │
│  [🗑 Delete Photos]                           │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │         [CONTINUE →]                 │    │
│  └──────────────────────────────────────┘    │
│                                              │
└──────────────────────────────────────────────┘
```

## Screen Fields

| Element / Field | Type | Description |
| --- | --- | --- |
| Page Header | Header | "BEFORE SERVICE PHOTOS" with back button |
| Task ID | Display (Read-only) | Current Task ID being executed (if shown in header) |
| Photo Thumbnails | Image Grid | Preview thumbnails of captured photos |
| Add Button (+) | Camera Trigger | Opens camera to capture additional photos |
| Photo Counter | Display | "X / 5" counter showing uploaded vs max |
| Delete Photos Button | Button | Initiates the multi-step delete flow (see below) |

## Validation Rules

| Rule | Description |
| --- | --- |
| Minimum 1 Photo | At least 1 "Before" photo is mandatory (from Module 21.6) |
| Maximum 5 Photos | Cannot upload more than 5 photos |
| Camera Only | Photos must be captured live (no gallery upload) |
| File Format | JPG, PNG only (Max 5MB per photo) — from Module 21.6 |
| GPS Embedded | Each photo includes GPS metadata |

## Actions

| Action | Trigger | Behaviour |
| --- | --- | --- |
| **Capture Photo** | Tap + or empty slot | Opens camera → captures photo → adds to grid |
| **Delete Photos** | Tap 🗑 Delete Photos button | Enters selection mode (see Delete Flow below) |
| **Continue** | Tap button | Validates minimum 1 photo → Navigates to After Service Photos (Screen 12) |

## Delete Photo Flow (Multi-Step)

| Step | Screen State | User Action |
| --- | --- | --- |
| **Step 1: Tap Delete** | User taps the "🗑 Delete Photos" button | Photo grid switches to **selection mode** — each photo shows a checkbox overlay |
| **Step 2: Select Photos** | User taps on one or more photos to select them for deletion | Selected photos show a ☑ check mark and a highlighted border. A counter shows "X selected". [Cancel Selection] and [🗑 Delete Selected] buttons appear |
| **Step 3: Tap Delete Selected** | User taps "🗑 Delete Selected" button | A **confirmation popup** appears: "Are you sure you want to delete X selected photo(s)? This action cannot be undone." with [Delete] and [Cancel] buttons |
| **Step 4: Confirm Delete** | User taps "Delete" on the confirmation popup | Selected photos are permanently removed from the grid. Photo counter updates. Grid exits selection mode. Success toast: "X photo(s) deleted." |
| **Cancel at any step** | User taps "Cancel Selection" or "Cancel" on popup | Selection is cleared, grid returns to normal view. No photos are deleted |

---

# Screen 12: After Service Photo Upload

**Source Reference:** Module 21 – Task Completion (21.6 – After Photo requirement)
**Purpose:** Capture mandatory photographs of the site AFTER the pest control service is completed, along with optional treatment-area photos.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]     AFTER SERVICE PHOTOS           │
│  Task: TASK-2026-0201                        │
│                                              │
│  Upload photos of the treated area after     │
│  completing service. (Min 1, Max 5)          │
│                                              │
│  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐    │
│  │ 📸   │  │ 📸   │  │  +   │  │      │    │
│  │After1│  │After2│  │ Add  │  │      │    │
│  │  ✅  │  │  ✅  │  │      │  │      │    │
│  └──────┘  └──────┘  └──────┘  └──────┘    │
│  Photos uploaded: 2 / 5                      │
│                                              │
│  [🗑 Delete Photos]                           │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │         [CONTINUE →]                 │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

## Screen Fields

| Element / Field | Type | Description |
| --- | --- | --- |
| Page Header | Header | "AFTER SERVICE PHOTOS" title with back button |
| Task ID | Display (Read-only) | Current Task ID being executed |
| After Service Photos | Image Grid | Mandatory post-service photo grid (up to 5 photos) |
| Photo Counter | Display | "X / 5" counter showing uploaded vs max |
| Delete Photos Button | Button | Initiates the multi-step delete flow (see below) |

## Validation Rules

| Rule | Description |
| --- | --- |
| Minimum 1 Photo | At least 1 "After Service" photo is mandatory (from Module 21.6) |
| Maximum 5 Photos | Cannot upload more than 5 after-service photos |
| Camera Only | Photos must be captured live (no gallery upload) |
| File Format | JPG, PNG only (Max 5MB per photo) — from Module 21.6 |
| GPS Embedded | Each photo includes GPS metadata |

## Actions

| Action | Trigger | Behaviour |
| --- | --- | --- |
| **Capture Photo** | Tap + or empty slot | Opens camera → captures photo → adds to grid |
| **Delete Photos** | Tap 🗑 Delete Photos button | Enters selection mode (see Delete Flow below) |
| **Continue** | Tap button | Validates minimum 1 after-service photo → Navigates to Service Execution Form (Screen 13) |
| **Back** | Tap ← | Returns to Before Service Photos (Screen 11) — data saved in draft |

## Delete Photo Flow (Multi-Step)

| Step | Screen State | User Action |
| --- | --- | --- |
| **Step 1: Tap Delete** | User taps the "🗑 Delete Photos" button | Photo grid switches to **selection mode** — each photo shows a checkbox overlay |
| **Step 2: Select Photos** | User taps on one or more photos to select them for deletion | Selected photos show a ☑ check mark and a highlighted border. A counter shows "X selected". [Cancel Selection] and [🗑 Delete Selected] buttons appear |
| **Step 3: Tap Delete Selected** | User taps "🗑 Delete Selected" button | A **confirmation popup** appears: "Are you sure you want to delete X selected photo(s)? This action cannot be undone." with [Delete] and [Cancel] buttons |
| **Step 4: Confirm Delete** | User taps "Delete" on the confirmation popup | Selected photos are permanently removed from the grid. Photo counter updates. Grid exits selection mode. Success toast: "X photo(s) deleted." |
| **Cancel at any step** | User taps "Cancel Selection" or "Cancel" on popup | Selection is cleared, grid returns to normal view. No photos are deleted |

---

# Screen 13: Service Execution Form (Multi-Service)

**Source Reference:** Module 21 – Task Completion & Material Log (21.6), Module 12 – Services
**Purpose:** Core data entry form for recording the actual service performed. Supports **multiple services per task**, with per-service details including pest infestation level, location, treatment methods, and chemicals/products used.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]     SERVICE EXECUTION              │
│  Task: TASK-2026-0201                        │
│                                              │
│  ─── SERVICE 1 ──────────────── [🗑 Remove]  │
│  ┌──────────────────────────────────────┐    │
│  │  Service Name*:                      │    │
│  │  [▼ Cockroach Treatment ▼]           │    │
│  │                                      │    │
│  │  Level of Pest Infestation*:         │    │
│  │  [◉ High]  [○ Medium]  [○ Low]      │    │
│  │                                      │    │
│  │  Location / Area*:                   │    │
│  │  [Kitchen area, storage room, and   ]│    │
│  │  [server room behind reception desk ]│    │
│  │                                      │    │
│  │  Method of Treatment*:               │    │
│  │  [▼ Select Methods ▼]               │    │
│  │  ☑ Spraying                          │    │
│  │  ☑ Gel Application                   │    │
│  │  ☐ Baiting                           │    │
│  │  ☐ Fogging                           │    │
│  │  ☐ Fumigation                        │    │
│  │  ☐ Dusting                           │    │
│  │  ☐ Trapping                          │    │
│  │                                      │    │
│  │  ─── Chemicals & Products Used ───   │    │
│  │  ┌────────────────────────────────┐  │    │
│  │  │ Material │ Dil(R/U) │ Qty(R/U) │  │    │
│  │  │──────────┼──────────┼──────────│  │    │
│  │  │ Alpha C. │ 1:50/[__]│ 120/[__] │  │    │
│  │  │ Fipronil │ N/A /[__]│   2/[__] │  │    │
│  │  └────────────────────────────────┘  │    │
│  │  [+ Add Chemical]                    │    │
│  │                                      │    │
│  │  Pesticides & Monitoring Trap Codes: │    │
│  │  [PCT-001, MTC-004, MTC-012     ]   │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── SERVICE 2 ──────────────── [🗑 Remove]  │
│  ┌──────────────────────────────────────┐    │
│  │  Service Name*:                      │    │
│  │  [▼ Rodent Management ▼]             │    │
│  │  (Same fields as Service 1)          │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  [+ ADD ANOTHER SERVICE]                     │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │         [CONTINUE →]                 │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

## Form Fields

### 13.0 Header

| Element / Field | Type | Description |
| --- | --- | --- |
| Page Header | Header | "SERVICE EXECUTION" title with back button and Task ID |

### 13.1 Service Block (Repeatable — One per Service)

Each service block contains the following fields:

| Field | Type | Required | Validation | Source |
| --- | --- | --- | --- | --- |
| Service Name | Dropdown | Yes | Must select from available services linked to this task or from Module 12 service list | Module 12 → Module 21 |
| Level of Pest Infestation | Radio (Single-select) | Yes | Must select one: High / Medium / Low | Module 21.6 |
| Location / Area | Textarea | Yes | Min 5 chars, Max 500 chars | Module 21.6 |
| Method of Treatment | Multi-Checkbox Dropdown | Yes | Must select at least one method | Module 12 |
| Pesticides & Monitoring Trap Codes | Text Input | No | Comma-separated codes (e.g., PCT-001, MTC-004) | Module 21.6 |

**Method of Treatment Options:**

| Option | Description |
| --- | --- |
| Spraying | Chemical spray application |
| Gel Application | Gel-based pest treatment |
| Baiting | Bait station placement |
| Fogging | Area fogging / misting |
| Fumigation | Sealed area fumigation |
| Dusting | Powder-based application |
| Trapping | Physical trap placement |

### 13.2 Chemicals & Products Table (Per Service)

| Field | Type | Required | Validation | Source |
| --- | --- | --- | --- | --- |
| Chemical / Product Name | Dropdown | Yes | Select from assigned materials or Module 11 stock | Module 12 → Module 21 |
| HSN Code | Display (hidden) | — | Auto-fetched | Module 10 |
| UOM | Display (Read-only) | — | Unit of Measure | Module 10 |
| Required Dilution | Display (Read-only) | — | Pre-filled from task/service | Module 21 |
| Used Dilution | Text Input | No | Format: "1:XX" or "N/A" | User Input |
| Required Quantity | Display (Read-only) | — | Pre-filled from task/service | Module 21 |
| Used Quantity | Number Input | Yes | Must be ≥ 0; cannot exceed branch stock | User Input |

**Stock Deduction Logic (Module 11 integration):**

| Event | System Behaviour |
| --- | --- |
| Form submitted | "Quantity Used" values across all services are summed and auto-deducted from branch stock (Module 11) |
| Extra chemical added | System checks availability in branch stock before allowing |
| Qty exceeds stock | Warning: "Insufficient stock for [Chemical Name] at [Branch]" |
| Qty = 0 | No deduction; row kept for record purposes |

### 13.3 Multi-Service Controls

| Action | Behaviour |
| --- | --- |
| **+ Add Another Service** | Adds a new empty service block below the existing ones |
| **🗑 Remove** | Removes the service block (confirmation required). At least 1 service must remain |
| **+ Add Chemical** | Adds a new row to the Chemicals & Products table within that service |

## Actions

| Action | Behaviour |
| --- | --- |
| **Continue** | Validates all required fields across all service blocks → Navigates to Technician Observations (Screen 14) |
| **Back** | Returns to After Service Photos (Screen 12) — data is saved in draft |

---

# Screen 14: Technician Observations

**Source Reference:** Module 21 – Task Completion, Field Observation requirements
**Purpose:** Capture the technician's professional observations during service. Structured into **3 key observation points**, each with a checkbox toggle and a mandatory location/area text field.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]     TECHNICIAN OBSERVATIONS        │
│  Task: TASK-2026-0201                        │
│                                              │
│  ─── 1. STRUCTURAL GAPS / PEST ENTRY ────    │
│  ┌──────────────────────────────────────┐    │
│  │  Structural gaps or pest entry       │    │
│  │  points found?                       │    │
│  │  [◉ Yes]  [○ No]                    │    │
│  │                                      │    │
│  │  ☑ Gaps in door frames              │    │
│  │  ☐ Cracks in walls                  │    │
│  │  ☑ Open drainage / pipes            │    │
│  │  ☐ Broken window seals             │    │
│  │  ☐ Gaps in ceiling                  │    │
│  │  ☐ Cable entry points              │    │
│  │  ☐ Other: [__________]             │    │
│  │                                      │    │
│  │  Location / Area*:                   │    │
│  │  [Main entrance door frame and      ]│    │
│  │  [drainage pipe near kitchen area   ]│    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── 2. HYGIENE & SANITATION ─────────────   │
│  ┌──────────────────────────────────────┐    │
│  │  Hygiene or sanitation issues found? │    │
│  │  [◉ Yes]  [○ No]                    │    │
│  │                                      │    │
│  │  ☑ Food residue found               │    │
│  │  ☐ Stagnant water present           │    │
│  │  ☐ Improper waste management        │    │
│  │  ☐ General cleanliness issue        │    │
│  │  ☐ Garbage accumulation             │    │
│  │  ☐ Other: [__________]             │    │
│  │                                      │    │
│  │  Location / Area*:                   │    │
│  │  [Kitchen counter and storage room  ]│    │
│  │  [near the back exit               ]│    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── 3. PEST SIGHTING ────────────────────   │
│  ┌──────────────────────────────────────┐    │
│  │  Active pest sighting observed?      │    │
│  │  [◉ Yes]  [○ No]                    │    │
│  │                                      │    │
│  │  ☑ Live pests seen                  │    │
│  │  ☐ Droppings / excrement found      │    │
│  │  ☐ Egg casings / nests              │    │
│  │  ☐ Gnaw marks / damage             │    │
│  │  ☐ Dead pests found                 │    │
│  │  ☐ Other: [__________]             │    │
│  │                                      │    │
│  │  Location / Area*:                   │    │
│  │  [Near reception desk, behind the   ]│    │
│  │  [water cooler and storage cabinet  ]│    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │         [CONTINUE →]                 │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

## Screen Components / Form Fields

### 14.1 Header & Top Controls

| Element / Field | Type | Description |
| --- | --- | --- |
| Page Header | Header | "TECHNICIAN OBSERVATIONS" with back button and Task ID |

### 14.2 Structural Gaps / Pest Entry Points

| Field | Type | Required | Validation | Description |
| --- | --- | --- | --- | --- |
| Found? | Radio (Yes/No) | Yes | Must select one | Whether structural gaps or pest entry points were observed |
| Checkbox Options | Multi-select Checkboxes | Conditional | Required if "Yes" selected; at least one must be checked | Options: Gaps in door frames, Cracks in walls, Open drainage / pipes, Broken window seals, Gaps in ceiling, Cable entry points, Other |
| Other (specify) | Text Input | Conditional | Required if "Other" is checked | Free-text for unlisted observations |
| Location / Area | Textarea | Conditional | Required if "Yes" selected; Min 5, Max 500 chars | Specific location where gaps/entry points were found |

### 14.3 Hygiene & Sanitation

| Field | Type | Required | Validation | Description |
| --- | --- | --- | --- | --- |
| Found? | Radio (Yes/No) | Yes | Must select one | Whether hygiene or sanitation issues were observed |
| Checkbox Options | Multi-select Checkboxes | Conditional | Required if "Yes" selected; at least one must be checked | Options: Food residue found, Stagnant water present, Improper waste management, General cleanliness issue, Garbage accumulation, Other |
| Other (specify) | Text Input | Conditional | Required if "Other" is checked | Free-text for unlisted observations |
| Location / Area | Textarea | Conditional | Required if "Yes" selected; Min 5, Max 500 chars | Specific location where hygiene issues were found |

### 14.4 Pest Sighting

| Field | Type | Required | Validation | Description |
| --- | --- | --- | --- | --- |
| Observed? | Radio (Yes/No) | Yes | Must select one | Whether active pest sightings were observed during service |
| Checkbox Options | Multi-select Checkboxes | Conditional | Required if "Yes" selected; at least one must be checked | Options: Live pests seen, Droppings / excrement found, Egg casings / nests, Gnaw marks / damage, Dead pests found, Other |
| Other (specify) | Text Input | Conditional | Required if "Other" is checked | Free-text for unlisted sighting types |
| Location / Area | Textarea | Conditional | Required if "Yes" selected; Min 5, Max 500 chars | Specific location/area where pests were sighted |

## Actions

| Action | Behaviour |
| --- | --- |
| **Continue** | Validates all 3 observation sections → Navigates to Technician Info & Notes (Screen 15) |
| **Back** | Returns to Service Execution Form (Screen 13) — data is saved in draft |

---

# Screen 15: Technician Info & Notes

**Source Reference:** Module 21 – Task Completion (21.6)
**Purpose:** Capture the technician's own details, completion notes, and confirm technician assignment for the service report.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]     TECHNICIAN INFO & NOTES        │
│  Task: TASK-2026-0201                        │
│                                              │
│  ─── TECHNICIAN DETAILS ─────────────────    │
│  ┌──────────────────────────────────────┐    │
│  │  Primary Technician : Ravi S. (You)  │    │
│  │  EMP ID             : EMP-00124      │    │
│  │  Role               : Technician     │    │
│  │  Branch             : Mumbai Branch  │    │
│  │  Support Technicians: Anjali M.      │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── ACTUAL EXECUTION TIME ──────────────    │
│  ┌──────────────────────────────────────┐    │
│  │  Start Time* : [▼ 08:05 AM ▼]       │    │
│  │  End Time*   : [▼ 09:45 AM ▼]       │    │
│  │  Duration    : 1h 40m (auto)         │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── COMPLETION NOTES ───────────────────    │
│  ┌──────────────────────────────────────┐    │
│  │  Notes*:                             │    │
│  │  [Treated all areas including server ]│    │
│  │  [room and storage. Applied gel in   ]│    │
│  │  [kitchen cabinets and spray near    ]│    │
│  │  [drainage outlets.                  ]│    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │         [CONTINUE →]                 │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

## Form Fields

### 15.0 Header

| Element / Field | Type | Description |
| --- | --- | --- |
| Page Header | Header | "TECHNICIAN INFO & NOTES" title with back button and Task ID |

### 15.1 Technician Details (Read-only)

| Field | Type | Description | Source |
| --- | --- | --- | --- |
| Primary Technician | Display (Read-only) | Main responsible technician name (highlighted if current user) | Module 21 |
| EMP ID | Display (Read-only) | Employee ID of the primary technician | Module 8 |
| Role | Display (Read-only) | Current role assignment | Module 8 |
| Branch | Display (Read-only) | Assigned branch name | Module 7 → Module 8 |
| Support Technicians | Display (Read-only) | Additional technicians assigned (if any) | Module 21 |

### 15.2 Actual Execution Time

| Field | Type | Required | Validation | Source |
| --- | --- | --- | --- | --- |
| Actual Start Time | Time Picker | Yes | Must be ≤ Actual End Time | Module 21.6 |
| Actual End Time | Time Picker | Yes | Must be ≥ Actual Start Time | Module 21.6 |
| Actual Duration | Display (Auto) | Auto | End – Start auto-calculated | Module 21.6 |

### 15.3 Completion Notes

| Field | Type | Required | Validation | Source |
| --- | --- | --- | --- | --- |
| Completion Notes | Textarea | Yes | Min 10 chars, Max 1000 chars | Module 21.6 |

## Actions

| Action | Behaviour |
| --- | --- |
| **Continue** | Validates → Navigates to Customer Verification & Submit (Screen 16) |
| **Back** | Returns to Observations (Screen 14) — data saved in draft |

---

# Screen 16: Customer Verification (OTP) & Service Submission

**Purpose:** Final step — verify service completion with the customer through OTP verification, collect customer feedback (Voice of Customer), and submit the service report.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]     CUSTOMER VERIFICATION          │
│  Task: TASK-2026-0201                        │
│                                              │
│  ─── CUSTOMER DETAILS (For OTP) ─────────    │
│  ┌──────────────────────────────────────┐    │
│  │  Customer Name*:                     │    │
│  │  [Suresh Kumar________________]      │    │
│  │                                      │    │
│  │  Mobile Number*:                     │    │
│  │  [+91 9876543210_____________]       │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │       [📱 SEND OTP]                  │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── ENTER OTP ──────────────────────────    │
│  ┌──────────────────────────────────────┐    │
│  │     [  _  ] [  _  ] [  _  ] [  _  ]  │    │
│  │                                      │    │
│  │  OTP sent to 98XXXX3210              │    │
│  │  Expires in: 02:45                   │    │
│  │  [Resend OTP]  (available after 60s) │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── CUSTOMER FEEDBACK ──────────────────    │
│  ┌──────────────────────────────────────┐    │
│  │  Customer Rating*:                   │    │
│  │  [⭐] [⭐] [⭐] [⭐] [☆]  (4/5)     │    │
│  │                                      │    │
│  │  Client Feedback (optional):         │    │
│  │  [Good service, thorough treatment. ]│    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── SUBMISSION SUMMARY ─────────────────    │
│  ┌──────────────────────────────────────┐    │
│  │  ● Selfie Captured        ✅        │    │
│  │  ● Before Photos (2)      ✅        │    │
│  │  ● Service Execution      ✅        │    │
│  │  ● After Photos (2)       ✅        │    │
│  │  ● Observations           ✅        │    │
│  │  ● Tech notes & Time      ✅        │    │
│  │  ● Customer Verified      ✅        │    │
│  │  ● Customer Feedback      ✅        │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │   [📄 SUBMIT SERVICE REPORT]         │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

## Form Fields

### 16.0 Header

| Element / Field | Type | Description |
| --- | --- | --- |
| Page Header | Header | "CUSTOMER VERIFICATION" with back button and Task ID |

### 16.1 Customer Details & OTP

| Field | Type | Required | Validation | Description |
| --- | --- | --- | --- | --- |
| Customer Name | Text Input | Yes | Pre-filled from task, editable. Min 2 chars | The name of the person verifying the service on-site |
| Mobile Number | Phone Input | Yes | Valid 10-digit mobile number | The mobile number to receive the validation OTP |
| OTP Input | 4-digit OTP | Yes | Must match sent OTP | OTP entered by the technician after receiving it from the customer |

### 16.2 Customer Feedback (Voice of Customer)

| Field | Type | Required | Validation | Source |
| --- | --- | --- | --- | --- |
| Customer Rating | Star Rating (1-5) | Yes | Must select at least 1 star | Module 21.6 |
| Client Feedback | Textarea | No | Max 500 chars | Module 21.6 |

### 16.3 Submission Summary Checklist

Auto-generated checklist showing completion status of all task execution steps. All items must show ✅ before submission is enabled. Note that "Additional Notes" in the checklist was updated to "Tech notes & Time" to map to the new Screen 15.

## Actions

| Action | Trigger | System Behaviour |
| --- | --- | --- |
| **Send OTP** | Tap button | Sends 4-digit OTP via SMS to the entered Mobile Number. Starts 3-minute expiry timer |
| **Resend OTP** | Tap link (after 60s cooldown) | Resends a new OTP. Previous OTP is invalidated |
| **Submit Service Report** | Tap button | Validates OTP + all steps are complete → Confirms via dialog: "Submit service report and mark task as completed?" → On confirm: Task Status → Completed in Module 21; Stock deducted from Module 11; Service report generated; GPS tracking stops for this task. Returns to Home Dashboard (Screen 2) with success toast: "Service report submitted successfully!" |

## OTP Business Rules

| Rule | Description |
| --- | --- |
| OTP Expiry | OTP is valid for 3 minutes |
| Max Attempts | Maximum 3 OTP verification attempts. After 3 failures → must resend |
| Resend Cooldown | "Resend OTP" available only after 60 seconds |

## Post-Submission System Actions

| System Action | Description | Module |
| --- | --- | --- |
| Task Status Update | Status changes to "Completed" | Module 21 |
| Stock Deduction | "Actually Used" quantities deducted from branch stock | Module 11 |
| Service Report Generation | PDF/digital report generated with all captured data | Module 21.6 |
| Audit Trail | Completion event logged with timestamp, GPS, and technician ID | Module 21 |
| Manager Notification | Technician Manager receives notification of task completion | Module 21 |

---

# Screen 17: Service Report View

**Source Reference:** Module 21 – Task Completion (21.6)
**Purpose:** Read-only view of the completed service report. Accessible from task cards (Completed status) on Home, Services, and Calendar pages.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]   SERVICE REPORT      [📤 Share]   │
│  TASK-2026-0201                              │
│                                              │
│  ─── TASK SUMMARY ───────────────────────    │
│  Customer  : ABC Corporation                 │
│  Site      : Head Office, Andheri            │
│  Service   : Cockroach Treatment             │
│  Date      : 23 Mar 2026                    │
│  Time      : 08:05 AM – 09:45 AM            │
│  Duration  : 1h 40m                          │
│  Technician: Ravi S. + Anjali M.             │
│                                              │
│  ─── CHEMICALS USED ────────────────────     │
│  Alpha Cypermethrin : 110 ml                 │
│  Fipronil Gel       : 2 tubes                │
│                                              │
│  ─── TREATMENT ──────────────────────────    │
│  Method    : Spraying                        │
│  Pest Types: Cockroaches, Bed Bugs           │
│                                              │
│  ─── OBSERVATIONS ───────────────────────    │
│  Structural: Gaps in door frames,            │
│              Open drainage/pipes             │
│  Entry Points: Main entrance, Storage room   │
│  Hygiene   : Food residue found              │
│  Sightings : Near reception desk...          │
│                                              │
│  ─── PHOTOS ─────────────────────────────    │
│  [Selfie] [Before1] [Before2] [After1] [After2]│
│                                              │
│  ─── CUSTOMER FEEDBACK ──────────────────    │
│  Rating   : ⭐⭐⭐⭐ (4/5)                   │
│  Feedback : Good service, thorough treatment │
│  Verified : ✅ OTP Verified                  │
│                                              │
│  ─── NOTES ──────────────────────────────    │
│  Treated all areas including server room...  │
│                                              │
│  [📥 Download PDF]                           │
└──────────────────────────────────────────────┘
```

## Actions

| Action | Description |
| --- | --- |
| **Share** | Share service report via WhatsApp, Email, or other apps |
| **Download PDF** | Downloads the service report as a PDF document |

## Screen Components / Fields

| Element / Field | Type | Description |
| --- | --- | --- |
| Page Header | Header | "SERVICE REPORT" title with Task ID |
| Day-wise Grouping | Group Header | Groups reports by day (e.g., "Today", "23 Mar 2026") when viewed in history |
| Task Summary | Section | Customer, Site, Date, Time, Duration, and Technician details |
| Chemicals Used | Section | List of chemicals and quantities used |
| Treatment | Section | Treatment method and target pest types |
| Observations | Section | Structural gaps, hygiene issues, and pest sightings logged |
| Photos | Thumbnail Grid | Scrollable list of captured photos (Before/After/Treatment) |
| Customer Feedback | Section | Star rating, text feedback, and OTP verification status |
| Notes | Text | Additional technician notes |

## All Fields (Read-only)

All fields from Screens 12–16 are displayed in read-only format, consolidated into a single scrollable report view.

---

# Screen 18: Notifications

**Purpose:** Centralized notification center showing all system alerts, task updates, leave status changes, and announcements.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]       NOTIFICATIONS                │
│                                              │
│  [All] [Tasks] [Leave] [System]              │
│                                              │
│  ─── Today ──────────────────────────────    │
│  ┌──────────────────────────────────────┐    │
│  │  📋 New task assigned: TASK-2026-0210│    │
│  │  Rodent Control at LMN Factory       │    │
│  │  Tomorrow, 09:00 – 11:00             │    │
│  │  2 hours ago                         │    │
│  └──────────────────────────────────────┘    │
│  ┌──────────────────────────────────────┐    │
│  │  🏖️ Leave Approved                   │    │
│  │  Your leave for 25-26 Mar has been   │    │
│  │  approved by HR.                     │    │
│  │  4 hours ago                         │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── Yesterday ──────────────────────────    │
│  ┌──────────────────────────────────────┐    │
│  │  ⚠️ Task Rescheduled: TASK-2026-0199 │    │
│  │  Moved to 28 Mar. New time: 10:00 AM │    │
│  │  1 day ago                           │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  [Mark All as Read]                          │
└──────────────────────────────────────────────┘
```

## Screen Components

| Element / Field | Type | Description |
| --- | --- | --- |
| Page Header | Header | "NOTIFICATIONS" title with back button |

## Notification Types

| Type | Icon | Examples |
| --- | --- | --- |
| Task Assigned | 📋 | New task assignment, task reassignment |
| Task Update | ⚠️ | Task rescheduled, task priority changed |
| Leave | 🏖️ | Leave approved, leave rejected |
| Attendance | ⏰ | Reminder to punch in, late marking alert |
| Petty Cash | 💰 | Request approved, request rejected, request returned, payment processed |
| System | 🔔 | App updates, announcements, policy changes |

## Actions

| Action | Behaviour |
| --- | --- |
| **Tap Notification** | Navigates to the relevant screen (Task Detail, Leave Module, etc.) |
| **Mark All as Read** | Clears unread badge count |
| **Tab Filters** | Filter notifications by category |

---

# Screen 19: Chatbot

**Purpose:** AI-powered assistant to help technicians with common queries about services, chemicals, safety procedures, and app navigation.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]       ASSISTANT                    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │  👋 Hi Ravi! How can I help you?     │    │
│  │                                      │    │
│  │  Quick Actions:                      │    │
│  │  [📋 My Tasks] [💊 Chemical Info]    │    │
│  │  [🛡️ Safety Tips] [📞 Help Desk]     │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │  🧑 What chemicals do I use for     │    │
│  │     termite treatment?               │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │  🤖 For termite treatment, you       │    │
│  │  typically use:                       │    │
│  │  • Chlorpyrifos 20% EC              │    │
│  │  • Imidacloprid 30.5% SC            │    │
│  │  Refer to your task details for      │    │
│  │  assigned chemicals and dosage.      │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │  [Type your question...         ] [➤]│    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

## Screen Components & Features

| Element / Feature | Type | Description |
| --- | --- | --- |
| Page Header | Header | "ASSISTANT" title with back button |
| Quick Action Buttons | Button Row | Pre-defined shortcuts for common queries: [My Tasks], [Chemical Info], [Safety Tips], [Help Desk] |
| Chat Thread | Message List | Scrollable history of user questions and bot responses |
| Free Text Input | Text Box | Type any question related to services, chemicals, safety |
| Send Button (➤) | Icon Button | Submits the typed question to the chatbot |
| Context-Aware | Backend Logic | Can reference the technician's current tasks and assigned chemicals |
| Help Desk | Quick Link | Quick link to contact the branch manager or support team |

---

# Screen 20: Petty Cash — My Requests

**Source Reference:** Module 24 – Petty Cash Management (24.2 Tab 2: My Requests)
**Purpose:** Personal expense tracker for the logged-in employee. Shows all petty cash requests submitted by the current user. Users can create new requests, track approval status, and view details of past claims.
**Opens From:** Bottom Navigation Bar → **Petty Cash (💰)** tab.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  PETTY CASH                      [🔍] [🔽]   │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │ [All (18)] │ [Pending (2)]          │    │
│  │ [Approved (10)] │ [Rejected (3)]    │    │
│  │ [Returned (1)]  │ [Paid (2)]        │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │  💰 PC-2026-0045                     │    │
│  │  📂 Local Conveyance                │    │
│  │  💵 ₹ 1,250                          │    │
│  │  📅 23 Mar 2026                      │    │
│  │  Status: ⏳ Pending                   │    │
│  └──────────────────────────────────────┘    │
│  ┌──────────────────────────────────────┐    │
│  │  💰 PC-2026-0038                     │    │
│  │  📂 Chemical                         │    │
│  │  💵 ₹ 2,400                          │    │
│  │  📅 20 Mar 2026                      │    │
│  │  Status: ✅ Approved                  │    │
│  └──────────────────────────────────────┘    │
│  ┌──────────────────────────────────────┐    │
│  │  💰 PC-2026-0020                     │    │
│  │  📂 Vendor Payment                   │    │
│  │  💵 ₹ 4,500                          │    │
│  │  📅 12 Mar 2026                      │    │
│  │  Status: 🔄 Returned                 │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │       [+ ADD REQUEST]                │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  [Bottom Navigation Bar]                     │
└──────────────────────────────────────────────┘
```

## Screen Components

### 20.1 Header & Top Controls

| Element | Type | Description |
| --- | --- | --- |
| Page Header | Header | "PETTY CASH" title bar |
| Search Icon (🔍) | Icon | Tap to search by Request ID or Description |
| Filter Icon (🔽) | Icon | Tap to open filter options (Category, Date Range) |

### 20.2 Status Filter Tabs

| Tab | Filter |
| --- | --- |
| All | Shows all petty cash requests |
| Pending | Status = Pending (awaiting approval) |
| Approved | Status = Approved |
| Rejected | Status = Rejected |
| Returned | Status = Returned (needs correction & resubmission) |
| Paid | Status = Paid (reimbursement completed) |

### 20.3 Request Card Fields

| Field | Type | Description | Source |
| --- | --- | --- | --- |
| Request ID | Display | Unique ID `PC-YYYY-NNNN`. Tap card → opens View Detail (Screen 20.2) | Module 24 |
| Category | Badge | Expense category (e.g., Local Conveyance, Chemical) | Module 24 |
| Amount (₹) | Display | Total claimed amount | Module 24 |
| Date | Display | Expense date (From) or range | Module 24 |
| Status | Badge | ⏳ Pending / ✅ Approved / ❌ Rejected / 🔄 Returned / 💰 Paid / 📝 Draft | Module 24 |

**Card Tap Action:** Opens View My Petty Cash Request (Screen 20.2).

### 20.4 Filter Options

| Filter | Type | Options |
| --- | --- | --- |
| Category | Dropdown | All / Asset Purchase / Chemical / Fuel / Internet & Telephone / Local Conveyance / Office Expenses / Salary Advance / Staff Welfare / Stationery / Statutory & License / Travel Expenses / Vehicle Maintenance / Vendor Payment / Rent / Office Deposit / Promoter Incentive / Overtime / Transportation / Petrocard |
| Date Range | Date Picker | Custom From – To date range |

### 20.5 Add Request Button

| Action | Trigger | Destination |
| --- | --- | --- |
| Add Request | Tap "+ ADD REQUEST" button | Opens Add Petty Cash Request Form (Screen 20.1) |

---

# Screen 20.1: Add Petty Cash Request

**Source Reference:** Module 24 – Petty Cash Management (24.2.1 Add Petty Cash Request)
**Purpose:** Form for employees to submit a new petty cash expense claim. Captures expense details, supporting documents, employee bank/UPI information for reimbursement, and optional prior approval reference.
**Opens From:** Screen 20 (My Requests) → **[+ ADD REQUEST]** button.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]     ADD PETTY CASH REQUEST         │
│                                              │
│  Request ID: PC-2026-XXXX (Auto on Submit)   │
│                                              │
│  ─── EXPENSE DETAILS ──────────────────────  │
│  Category*                                   │
│  [▼ Select Category ▼]                       │
│    • Asset Purchase      • Chemical          │
│    • Fuel                • Internet & Tel.   │
│    • Local Conveyance    • Office Expenses   │
│    • Salary Advance      • Staff Welfare     │
│    • Stationery          • Statutory & Lic.  │
│    • Travel Expenses     • Vehicle Maint.    │
│    • Vendor Payment      • Rent              │
│    • Office Deposit      • Promoter Incent.  │
│    • Overtime            • Transportation    │
│    • Petrocard                               │
│                                              │
│  Expense Date (From)*                        │
│  [📅 20 Mar 2026]                            │
│                                              │
│  Expense Date (To)*                          │
│  [📅 23 Mar 2026]                            │
│                                              │
│  Amount (₹)*                                 │
│  [₹ 1,250______________]                     │
│                                              │
│  Description*                                │
│  [Purchased pest bait from local vendor ]    │
│  [during service at ABC Corp Head Office]    │
│                                              │
│  Related Task (Optional)                     │
│  [🔍 Search Task ID ▼]                       │
│                                              │
│  Related SO (Optional)                       │
│  [🔍 Search SO No. ▼]                        │
│                                              │
│  ─── SUPPORTING DOCUMENTS ─────────────────  │
│  Bill / Receipt*                             │
│  [📎 Upload File]  ✅ receipt_1.jpg           │
│  (PDF, JPG, PNG — Max 5MB per file)          │
│  [📎 Upload More]  (Up to 5 files)           │
│                                              │
│  Justification Note (Optional)               │
│  [________________________________]          │
│                                              │
│  ─── PRIOR APPROVAL ──────────────────────── │
│  Was this expense pre-approved?              │
│  [☑ Yes]  [☐ No]                             │
│                                              │
│  ── If Yes ──                                │
│  Approved By*                                │
│  [🔍 Search Manager / Supervisor ▼]          │
│  Approval Reference                          │
│  [Verbal approval on 22 Mar_______]          │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │  [SAVE DRAFT]    [SUBMIT REQUEST]   │    │
│  └──────────────────────────────────────┘    │
│  [Cancel]                                    │
└──────────────────────────────────────────────┘
```

---

## Section 1: Expense Details Fields

| Field | Type | Required | Validation | Description |
| --- | --- | --- | --- | --- |
| Category | Dropdown | Yes | Must select from 19 categories listed in layout | Type of expense |
| Expense Date From | Date Picker | Yes | Cannot be a future date (max = today) | Start date of the expense period |
| Expense Date To | Date Picker | Yes | Must be ≥ From date; cannot be a future date | End date of the expense period |
| Amount (₹) | Currency Input | Yes | Must be > 0; max ₹50,000 | Total expense amount |
| Description | Textarea | Yes | Min 10 chars, Max 500 chars | What, when, how — expense explanation |
| Related Task | Search Dropdown | No | Must exist in Module 21 (if provided) | Link to a specific service task |
| Related SO | Search Dropdown | No | Must exist in Module 20 (if provided) | Link to a specific Sales Order |

### Category Dropdown Values (19 categories)

| # | Category |
| --- | --- |
| 1 | Asset Purchase |
| 2 | Chemical |
| 3 | Fuel |
| 4 | Internet & Telephone |
| 5 | Local Conveyance |
| 6 | Office Expenses |
| 7 | Salary Advance |
| 8 | Staff Welfare |
| 9 | Stationery |
| 10 | Statutory & License |
| 11 | Travel Expenses |
| 12 | Vehicle Maintenance |
| 13 | Vendor Payment |
| 14 | Rent |
| 15 | Office Deposit |
| 16 | Promoter Incentive |
| 17 | Overtime |
| 18 | Transportation |
| 19 | Petrocard |

---

## Section 2: Supporting Documents Fields

| Field | Type | Required | Validation | Description |
| --- | --- | --- | --- | --- |
| Bill / Receipt | File Upload (Camera / Gallery) | Yes | Min 1 file; PDF, JPG, PNG; Max 5MB each | Proof of expense (up to 5 files) |
| Upload More | Button | — | Visible after 1st upload; max 5 files total | Add additional receipts |
| Justification Note | Textarea | No | Max 500 chars | Additional context for the approver |

---

> **Note:** Bank / Payment Details are NOT captured in this form. The employee's bank/UPI details are already available in their employee profile (Module 8). Reimbursement is processed by the Finance team / Manager through the web ERP (Module 24.3.2 — Payment Processing Form).

---

## Section 3: Prior Approval Fields

| Field | Type | Required | Validation | Description |
| --- | --- | --- | --- | --- |
| Pre-Approved? | Checkbox | Yes | Default: No | Whether expense was approved beforehand |
| Approved By | Search Dropdown | Cond. | Must be a manager/supervisor from Module 8. Required if Pre-Approved = Yes | Person who gave prior approval |
| Approval Reference | Text | No | Max 200 chars | Verbal/written approval reference (e.g., "Verbal approval on 22 Mar") |

---

## Validation Rules (Summary)

| Rule | Description |
| --- | --- |
| Category Required | Must select one of 19 categories |
| Date Validation | From Date ≤ To Date; neither can be a future date |
| Amount Range | Amount must be > ₹0 and ≤ ₹50,000 |
| Description Length | Minimum 10 characters, maximum 500 characters |
| Receipt Upload | At least 1 file required; each file max 5MB; accepted formats: PDF, JPG, PNG |
| Prior Approval | If "Yes" is checked, "Approved By" becomes mandatory |

---

## Form Actions

| Action | System Behaviour |
| --- | --- |
| **Save Draft** | Saves form without validation. Status = **Draft**. No notifications sent. Returns to My Requests (Screen 20) |
| **Submit Request** | Validates all fields → Opens Recipient Selection popup (matches web 24.2.1.1 behavior) → On confirm: Status = **Pending**, notification sent to selected approver(s) → Shows success toast: "Petty cash request submitted successfully." → Returns to My Requests (Screen 20) |
| **Cancel** | Discards form and returns to My Requests (Screen 20) |

### Recipient Selection Popup (On Submit)

```
┌─────────────────────────────────────────┐
│  SELECT RECIPIENTS                       │
│  ┌─────────────────────────────────┐    │
│  │  Send to:                       │    │
│  │  ☑ All (Default)               │    │
│  │  ☐ Priya D. (Branch Manager)   │    │
│  │  ☐ Kamal R. (Operations Head)  │    │
│  │  ☐ Neha S. (Finance Manager)   │    │
│  │                                 │    │
│  │  [CONFIRM SEND]  [CANCEL]       │    │
│  └─────────────────────────────────┘    │
└─────────────────────────────────────────┘
```

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| All | Checkbox | Default | Sends to all authorized approvers |
| Recipients | Multi-select | Cond. | Individual managers/supervisors (from employee roles in Module 8) |

---

## Business Rules

| Rule | Description |
| --- | --- |
| Live Capture | Receipt photos can be captured live from camera or selected from gallery |
| No Bank Input | Bank/UPI details are NOT collected in this form. Reimbursement is handled by Finance via ERP web (Module 24.3.2) |
| Offline Draft | Form supports offline draft saving. Auto-syncs when network is restored |
| Edit Draft / Returned | Only requests with Status = **Draft** or **Returned** can be edited. Editing opens this form pre-filled with existing data |

---

# Screen 20.2: View My Petty Cash Request (Read-Only)

**Source Reference:** Module 24 – Petty Cash Management (24.2.2 View My Request)
**Purpose:** Read-only detail screen showing the complete breakdown of a petty cash request, including expense info, supporting documents, prior approval, and approval status. **Reimbursement details (Payment Status, Transaction Ref, etc.) are visible only when Status = Paid.**
**Opens From:** Screen 20 (My Requests) → Tap on any **Request Card**.

---

## Screen Layout

```
┌──────────────────────────────────────────────┐
│  [← Back]          REQUEST DETAIL            │
│                                              │
│  PC-2026-0045            Status: ⏳ PENDING  │
│                                              │
│  ─── EXPENSE DETAILS ──────────────────────  │
│  ┌──────────────────────────────────────┐    │
│  │  Category      : Local Conveyance   │    │
│  │  Expense Date  : 20 – 23 Mar 2026   │    │
│  │  Amount (₹)    : ₹ 1,250            │    │
│  │  Description   : Purchased pest     │    │
│  │                  bait from local     │    │
│  │                  vendor during       │    │
│  │                  service at ABC Corp │    │
│  │  Related Task  : TASK-2026-0201     │    │
│  │  Related SO    : SO-2026-0112       │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── SUPPORTING DOCUMENTS ─────────────────  │
│  ┌──────────────────────────────────────┐    │
│  │  Bills/Receipts: [📄 receipt_1.jpg]  │    │
│  │                  [📄 receipt_2.pdf]  │    │
│  │  Justification : Urgent purchase    │    │
│  │                   during site visit │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── PRIOR APPROVAL ─────────────────────── │
│  ┌──────────────────────────────────────┐    │
│  │  Pre-Approved?  : No                │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── APPROVAL STATUS ──────────────────────  │
│  ┌──────────────────────────────────────┐    │
│  │  Approval Status : ⏳ Pending       │    │
│  │  Reviewed By     : —                │    │
│  │  Review Date     : —                │    │
│  │  Approved Amount : —                │    │
│  │  Reviewer Remarks: —                │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── REIMBURSEMENT DETAILS ────────────────  │
│  (⚠ This section visible ONLY when          │
│   Status = 💰 Paid)                          │
│  ┌──────────────────────────────────────┐    │
│  │  Payment Status  : 💰 Paid          │    │
│  │  Payment Mode    : Bank Transfer     │    │
│  │  Transaction Ref : UTR1234567890     │    │
│  │  Payment Date    : 28 Mar 2026       │    │
│  │  Paid Amount     : ₹ 1,250           │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ─── SUBMISSION INFO ──────────────────────  │
│  ┌──────────────────────────────────────┐    │
│  │  Submitted On  : 23 Mar 2026 10:30  │    │
│  │  Sent To       : All Managers       │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  [Bottom Navigation Bar]                     │
└──────────────────────────────────────────────┘
```

---

## View Fields

| Field | Type | Description |
| --- | --- | --- |
| Request ID | Display | `PC-YYYY-NNNN` |
| Status | Badge | Draft / Pending / Approved / Rejected / Returned / Paid |
| Category | Display | Expense category |
| Expense Date | Display | From – To date range |
| Amount (₹) | Display | Total claimed amount |
| Description | Display | Expense explanation |
| Related Task | Display (Link) | Task ID (if linked) |
| Related SO | Display (Link) | SO Number (if linked) |
| Bills / Receipts | Tap-to-View | Attached receipt files (tap to open full-screen preview) |
| Justification | Display | Additional note |

| Pre-Approved? | Display | Yes / No |
| Approved By | Display | Manager name (if pre-approved) |
| Approval Reference | Display | Free text (if provided) |
| Approval Status | Badge | Pending / Approved / Rejected |
| Reviewed By | Display | Manager who reviewed |
| Review Date | Display | Date of review |
| Approved Amount | Display | Amount approved (may differ from claimed) |
| Reviewer Remarks | Display | Manager's comments |
| **— REIMBURSEMENT (Visible only when Status = Paid) —** | | |
| Payment Status | Badge | Paid |
| Payment Mode | Display | Mode used for reimbursement (Bank Transfer / UPI / Cash) |
| Transaction Ref | Display | UTR / Cheque number |
| Payment Date | Display | Reimbursement date |
| Paid Amount | Display | Actual amount reimbursed |
| Submitted On | Display | Submission timestamp |
| Sent To | Display | Approver(s) who received the request |

---

## Actions (Conditional)

| Action | Available When | Description |
| --- | --- | --- |
| **Edit** | Status = Draft / Returned | Opens Add Petty Cash Request form (Screen 20.1) pre-filled with existing data |
| **Revoke** | Status = Pending | Cancels the submitted request. Confirmation popup: "Are you sure you want to revoke this request?" |

---

# 📊 Complete Task Execution Flow (End-to-End)

The following diagram summarizes the complete task execution flow from start to finish:

```
┌─────────────┐
│  Screen 2   │
│  Dashboard   │  ← Technician logs in, punches in, sees today's tasks
│  (Home)      │
└──────┬──────┘
       │ Tap Task Card
       ▼
┌─────────────┐
│  Screen 8   │
│  Task Detail │  ← View full task info, customer details, materials
└──────┬──────┘
       │ Tap "Start Task"
       ▼
┌─────────────┐
│  Screen 10  │
│  Selfie     │  ← Front camera selfie with GPS + timestamp
│  Capture    │
└──────┬──────┘
       │ Continue
       ▼
┌─────────────┐
│  Screen 10.1│
│  Safety     │  ← 10-second mandatory safety checklist popup
│  Popup      │   → Task status changes: Pending → In Progress
└──────┬──────┘
       │ I Understand
       ▼
┌─────────────┐
│  Screen 11  │
│  Before     │  ← Capture before-service photos (min 1, max 5)
│  Photos     │
└──────┬──────┘
       │ Continue
       ▼
┌─────────────┐
│  Screen 12  │
│  After      │  ← Capture after-service photos (min 1, max 5)
│  Photos     │    + optional treatment photos
└──────┬──────┘
       │ Continue
       ▼
┌─────────────┐
│  Screen 13  │
│  Service    │  ← Record time, chemicals used, treatment method,
│  Execution  │    pest types covered
└──────┬──────┘
       │ Continue
       ▼
┌─────────────┐
│  Screen 14  │
│  Technician │  ← Record structural gaps, pest entry points,
│  Observations│   hygiene issues, pest sighting locations
└──────┬──────┘
       │ Continue
       ▼
┌─────────────┐
│  Screen 15  │
│  Additional │  ← Completion notes, technician details
│  Details    │
└──────┬──────┘
       │ Continue
       ▼
┌─────────────┐
│  Screen 16  │
│  OTP +      │  ← OTP verification + customer rating & feedback
│  Review &   │   → Submit: Task → Completed, Stock deducted,
│  Submit     │     Report generated
└──────┬──────┘
       │ Submit
       ▼
┌─────────────┐
│  Screen 2   │
│  Dashboard   │  ← Back to home with success confirmation
│  (Home)      │   Task card now shows "Completed" + Service Report
└─────────────┘
```

---

# 🔐 Role-Based Access Control (RBAC) — Mobile App

**Source Reference:** Module 21 (RBAC), Module 25 (RBAC)

| Feature | Technician | Senior Technician |
| --- | --- | --- |
| Login to App | ✅ | ✅ |
| View Own Tasks | ✅ | ✅ |
| Start & Complete Tasks | ✅ | ✅ |
| Log Materials/Chemicals | ✅ | ✅ |
| View Map / Navigate | ✅ | ✅ |
| Punch In / Punch Out | ✅ | ✅ |
| View Own Attendance | ✅ | ✅ |
| Apply Leave | ✅ | ✅ |
| View Leave Status | ✅ | ✅ |
| View Own Salary | ✅ | ✅ |
| Download Salary Slip | ✅ (own only) | ✅ (own only) |
| Submit Petty Cash Request | ✅ | ✅ |
| View Own Petty Cash Requests | ✅ | ✅ |
| Edit Draft / Returned Requests | ✅ | ✅ |
| Revoke Pending Request | ✅ | ✅ |
| View Other's Tasks | ❌ | ❌ |
| Create/Edit Tasks | ❌ | ❌ |
| Approve/Reject Leave | ❌ | ❌ |
| Approve/Reject Petty Cash | ❌ | ❌ |
| Edit Attendance | ❌ | ❌ |

---

# 📋 Global Business Rules Summary

| Rule | Description | Source |
| --- | --- | --- |
| GPS Mandatory | GPS must be enabled for Punch In/Out, Selfie, Photo uploads, and navigation | Module 22, Module 25 |
| Network Handling | App supports offline draft saving for task execution forms. Auto-syncs when network is restored | System Design |
| Session Timeout | Auto-logout after 24 hours of inactivity | Module 1 |
| Photo Integrity | All photos must be live captures (no gallery), with GPS and timestamp metadata | Module 21.6 |
| Stock Deduction | Materials used are automatically deducted from branch stock on task submission | Module 11, Module 21.6 |
| Task Overdue | Tasks past scheduled date with status ≠ Completed are auto-marked as "Overdue" | Module 21 |
| Audit Trail | All actions (login, attendance, task start/complete, leave apply) are logged | All Modules |
| Background GPS | Active only between Punch In and Punch Out events | Module 22 |
| Data Privacy | Customer contact details are partially masked on technician's device | Module 21, Module 18 |
| Push Notifications | Real-time push notifications for task assignments, reschedules, leave approvals | System Design |

---

*End of Document*

