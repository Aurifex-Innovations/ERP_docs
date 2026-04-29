# 📱 Mobile App — Auto-Events, Notifications & Download Specification

> **Version:** 1.0  
> **Last Updated:** 24 Apr 2026  
> **Scope:** Field Technician / Employee Mobile Application (22 Screens)  
> **Source:** `mobile_screen.md` — UI/UX Documentation  
> **Purpose:** Defines all system events, push/in-app notifications, and download actions specific to the mobile app experience.

---

## Mobile App Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    MOBILE APP SCREENS                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  AUTH          │  Screen 1-5   :  Splash, Login, OTP, Reset │
│  DASHBOARD     │  Screen 6     :  Home (Attendance + Tasks) │
│  NAVIGATION    │  Screen 7     :  Bottom Nav Bar            │
│  TASKS         │  Screen 8     :  Services List             │
│                │  Screen 11    :  Task Detail                │
│                │  Screen 12    :  Navigation Map             │
│  EXECUTION     │  Screen 13-17 :  5-Step Task Flow          │
│  REPORT        │  Screen 18    :  Service Report + Share    │
│  CALENDAR      │  Screen 9     :  Calendar (Attendance)     │
│  LEAVE         │  Screen 9.1   :  Leave History + Apply     │
│  PROFILE       │  Screen 10    :  View Profile              │
│                │  Screen 10.1  :  Edit Profile              │
│  NOTIFICATIONS │  Screen 19    :  Notification Center       │
│  CHATBOT       │  Screen 20    :  Support Chatbot           │
│  PETTY CASH    │  Screen 21    :  My Requests               │
│                │  Screen 21.1  :  Add Request               │
│                │  Screen 21.2  :  View Request Detail       │
│  SETTINGS      │  Screen 22    :  App Preferences           │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Legend

| Symbol | Meaning |
|--------|---------|
| ⚡ | Auto-Event (system-triggered action) |
| 📲 | Push Notification (mobile-specific) |
| 🔔 | In-App Notification (bell icon, Screen 19) |
| 📥 | Download action (button exists on screen) |
| 📧 | External Send (email/WhatsApp to customer/vendor) |

---

# ═══════════════════════════════════════════════════════════════
# 1. PUNCH IN / PUNCH OUT — ATTENDANCE (Screen 6)
# ═══════════════════════════════════════════════════════════════

> **Source:** Screen 6 → Attendance Section | Module 25 – HRM Attendance

## Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| ATT.1 | Technician swipes **Punch In** slider | Capture GPS coordinates + timestamp; start background GPS tracking (Module 22); register **Clock In** status | Without GPS, attendance is unverifiable |
| ATT.2 | Punch In time is **after shift start** (from Module 6 config) | Auto-mark attendance as **"Late"** | Incorrect payroll deduction if not flagged |
| ATT.3 | Technician swipes **Punch Out** slider | Capture GPS + timestamp; calculate total hours; stop GPS tracking; register **Clock Out**; close daily travel log | Background tracking persists unnecessarily |
| ATT.4 | No Punch In recorded by **grace period end** | Auto-mark **Absent** (pending regularization request) | Employee shown as present without evidence |
| ATT.5 | Shift end time reached + no Punch Out | **Auto Punch Out** at shift end time; close travel log | GPS tracking runs indefinitely |
| ATT.6 | 3+ consecutive workdays **Absent** without approved leave | Auto-alert HR Manager: **"Unauthorized Absence"** | Non-compliance with attendance policy |

## Notifications (📲 Push + 🔔 In-App)

| # | Event | Delivery | Message | Deep Link |
|---|-------|----------|---------|-----------|
| ATT.N1 | Shift start time reached (no Punch In yet) | 📲 Push | "Good morning! Please punch in to start your day." | → Screen 6 (Dashboard) |
| ATT.N2 | Punch In recorded | 🔔 In-App | "✅ Checked in at {time}. Have a productive day!" | — |
| ATT.N3 | Late arrival flagged | 📲 Push + 🔔 | "⚠️ Late check-in: {time}. Shift started at {shift_start}." | → Screen 9 (Calendar) |
| ATT.N4 | Punch Out recorded | 🔔 In-App | "Checked out at {time}. Total hours: {hours}." | — |
| ATT.N5 | Auto Punch Out (shift end) | 📲 Push | "Auto punch-out recorded at {shift_end}. Please punch out manually next time." | → Screen 6 |
| ATT.N6 | Marked Absent (no attendance) | 📲 Push (next day) | "You were marked Absent on {date}. Apply regularization if needed." | → Screen 9.1 (Leaves) |
| ATT.N7 | 15 minutes before shift end (reminder) | 📲 Push | "Shift ending soon. Don't forget to punch out." | → Screen 6 |

## Downloads

> No download actions on the Attendance section of Screen 6.

---

# ═══════════════════════════════════════════════════════════════
# 2. TASK MANAGEMENT & EXECUTION (Screens 8, 11–18)
# ═══════════════════════════════════════════════════════════════

> **Source:** Screens 8, 11-18 | Module 21 – Task Management, Module 11 – Stock, Module 22 – Live Location

## Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| TSK.1 | New task assigned to technician | Push task data to device; add to Services list (Screen 8) + Dashboard (Screen 6) | Technician doesn't know about new task |
| TSK.2 | Task scheduled date = **Today** | Move to "Today's Tasks" section on Dashboard | Missed SLA |
| TSK.3 | "Start Travel" tapped (Screen 11) | Capture GPS; set location status → **On Going**; start task-specific travel leg timer | Travel time not tracked |
| TSK.4 | "Start Task" tapped (Screen 11) → Geo-fence check (Screen 12) | Validate GPS within 500m radius of site; if inside → set status **Arrived** + **In Progress**; begin execution flow | Fake task execution from remote location |
| TSK.5 | Geo-fence validation **fails** (outside 500m) | Disable "Confirm Arrival" button; show: "Cannot confirm arrival. You are outside the allowed radius." | — |
| TSK.6 | Service Execution Form saved (Screen 14) | **Real-time stock validation** — block if qty > available Van Stock; temporarily reserve stock | Over-usage of chemicals |
| TSK.7 | "Submit Task" tapped (Screen 17) with valid OTP | Finalize all data Steps 1-5; **permanently deduct** materials from branch stock (Module 11); update Task → **Completed**; update SO status (Module 20); generate **Service Report PDF** | Phantom stock; SO stuck open |
| TSK.8 | Task scheduled time **passed** (status still Pending) | Auto → **Overdue** on device; alert Task Manager (Web) | Missed SLA unknown to operations |
| TSK.9 | Task **rescheduled** by dispatcher (from web) | Update task card on mobile; adjust calendar | Technician goes to wrong time/date |

## Notifications (📲 Push + 🔔 In-App)

| # | Event | Delivery | Message | Deep Link |
|---|-------|----------|---------|-----------|
| TSK.N1 | **New task assigned** | 📲 Push + 🔔 | "📋 New task: {TASK_ID} at {customer}, {site}. Scheduled: {date} {time}." | → Screen 11 (Task Detail) |
| TSK.N2 | **Task reminder** (day before) | 📲 Push | "Tomorrow: {TASK_ID} at {customer}. {time}." | → Screen 11 |
| TSK.N3 | **Task reminder** (1 hour before) | 📲 Push | "⏰ Task {TASK_ID} starts in 1 hour. Start travel soon." | → Screen 11 |
| TSK.N4 | **Task overdue** | 📲 Push + 🔔 | "⚠️ Task {TASK_ID} is overdue! Please start immediately." | → Screen 11 |
| TSK.N5 | **Task rescheduled** | 📲 Push + 🔔 | "🔄 Task {TASK_ID} rescheduled to {new_date} {new_time}." | → Screen 11 |
| TSK.N6 | **Task reassigned** (to this tech) | 📲 Push + 🔔 | "📋 Task {TASK_ID} reassigned to you." | → Screen 11 |
| TSK.N7 | **Task reassigned** (from this tech) | 📲 Push + 🔔 | "Task {TASK_ID} has been reassigned to {new_tech}." | — |
| TSK.N8 | **Task completed successfully** | 🔔 In-App | "🎉 Task {TASK_ID} completed. Service report generated." | → Screen 18 |
| TSK.N9 | **Stock insufficient** during execution | 🔔 In-App (inline) | "⚠️ Insufficient stock: '{chemical}'. Available: {qty}." | — (inline on Screen 14) |

## External Sends (📧 User-Triggered)

| # | UI Action | Screen | What is Sent | To Whom |
|---|-----------|--------|--------------|---------|
| TSK.S1 | **[SHARE]** button on Service Report | Screen 18 | Service Report PDF + Previous Service Logs (Excel) + Previous Details + Invoice (if applicable) | Selected customer contacts (SO Contact, Contract Contact, Billing Contact) via Email |

> This is the **primary customer-facing email** from the mobile app — sent on task completion.

## Downloads (📥)

| # | Screen | Button | API Endpoint | Format | Condition |
|---|--------|--------|--------------|--------|-----------|
| TSK.D1 | Screen 18: Service Report View | **📥 DOWNLOAD PDF** | `GET /v1/tasks/{id}/service-report/pdf` | PDF | After task completion only |
| TSK.D2 | Screen 11: Task Detail → Task History | **📄 View Previous Report** | `GET /v1/tasks/{previous_task_id}/service-report/pdf` | PDF | Previous task exists for this site |

---

# ═══════════════════════════════════════════════════════════════
# 3. SERVICE REPORT (Screen 18)
# ═══════════════════════════════════════════════════════════════

> **Source:** Screen 18 | Module 21 – Task Completion

### Service Report PDF Contents

The auto-generated PDF includes:

| Section | Content | Source |
|---------|---------|-------|
| Header | Company logo, Report #, Date, Task ID | System generated |
| Customer Info | Business name, Site name, Address, POC | Module 18 |
| Service Summary | Services performed, Areas treated, Methods used | Screen 14 data |
| Chemicals Used | Chemical name, HSN, Qty, Batch # | Screen 14 data |
| Before/After Photos | Embedded images | Screen 13 + 16 data |
| Observations | Structural issues, Hygiene recommendations, Pest activity | Screen 15 data |
| Customer Feedback | Star rating (1-5), Customer remarks | Screen 17 data |
| Technician Info | Name, ID, Signature (OTP-verified) | Module 8, Screen 17 |
| Verification | OTP verified timestamp, Customer name | Screen 17 |

### Share Options (Screen 18)

| Recipient | Source | Selectable |
|-----------|--------|------------|
| SO Contact | Service Order → Contact person | ☑️ Default checked |
| Contract Contact | Contract → Primary Contact | ☑️ Default checked |
| Billing Contact | Customer → Billing email | ☐ Unchecked by default |

### Attachments Included in Share Email

| Attachment | Description | Source |
|------------|-------------|-------|
| 📄 Service Report (Current) | Auto-generated PDF from this task | Screen 18 |
| 📊 Previous Service Logs | Service history Excel export | Module 18.3.3 (Customer Service History) |
| 📄 Previous Service Details | Last service report PDF | Module 21 → Previous task |
| 🧾 Invoice | Only if invoice already exists for this SO | Module 28 |

---

# ═══════════════════════════════════════════════════════════════
# 4. PETTY CASH (Screens 21, 21.1, 21.2)
# ═══════════════════════════════════════════════════════════════

> **Source:** Screens 21–21.2 | Module 24 – Petty Cash Management

## Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| PC.1 | "Submit for Approval" tapped (Screen 21.1) | Auto-route to **Level 1 approver** (Branch Manager) per Module 24 config | No accountability |
| PC.2 | Level 1 approved + amount **> threshold** | Auto-route to **Level 2** (Finance Head) | Large expenses without oversight |
| PC.3 | All levels approved | Enable "Mark Paid" for Finance on web dashboard | Delayed reimbursement |
| PC.4 | Request marked **Paid** (by Finance on web) | Update status on mobile; post expense to Ledger (Module 31) | Expense not in books |
| PC.5 | Request **Returned** (by approver) | Unlock for editing on mobile (via Screen 21.2 → Edit Request) | Employee can't correct |
| PC.6 | Approval pending > **48 hrs** | Auto-escalate to next approver | Employee frustration, stalled reimbursement |
| PC.7 | Expense date > **Admin-configured max past days** (e.g., 7 days) | Block submission: "Expense date is too old." | Backdating fraud |

## Notifications (📲 Push + 🔔 In-App)

| # | Event | Delivery | Message | Deep Link |
|---|-------|----------|---------|-----------|
| PC.N1 | Request **submitted** successfully | 🔔 In-App | "Petty cash request {PC_ID} submitted. ₹{amount}." | → Screen 21 |
| PC.N2 | Request **approved** (Level 1) | 📲 Push + 🔔 | "✅ PC {PC_ID} approved by {manager}." | → Screen 21.2 |
| PC.N3 | Request forwarded to **Level 2** | 🔔 In-App | "PC {PC_ID} forwarded to Finance for final approval." | → Screen 21.2 |
| PC.N4 | Request **approved** (final) | 📲 Push + 🔔 | "✅ PC {PC_ID} fully approved. ₹{amount}. Awaiting reimbursement." | → Screen 21.2 |
| PC.N5 | Request **rejected** | 📲 Push + 🔔 | "❌ PC {PC_ID} rejected: '{reason}'." | → Screen 21.2 |
| PC.N6 | Request **returned** for correction | 📲 Push + 🔔 | "↩️ PC {PC_ID} returned: '{comment}'. Please re-upload/correct." | → Screen 21.2 (Edit) |
| PC.N7 | Request **paid/reimbursed** | 📲 Push + 🔔 | "💰 PC {PC_ID} reimbursed! ₹{amount}." | → Screen 21.2 |
| PC.N8 | Approval **overdue** (48 hrs) — to employee | 🔔 In-App | "PC {PC_ID} pending approval for 48+ hours. Escalated." | → Screen 21 |
| PC.N9 | **Draft** reminder (3 days old) | 📲 Push | "You have a draft petty cash request. Submit or discard." | → Screen 21 |

## Downloads

> No download buttons on mobile Petty Cash screens per UI/UX doc.  
> Excel export of Petty Cash data is available **only on the Web dashboard** (Module 24 web).

---

# ═══════════════════════════════════════════════════════════════
# 5. SALARY SLIP DOWNLOAD (Screen 10 — Profile)
# ═══════════════════════════════════════════════════════════════

> **Source:** Screen 10 → Section 4 (Salary Information) | Module 25 – HRM Salary

## Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| SAL.1 | Payroll processed for current month (by HR on web) | Update salary display on Profile (Screen 10); enable Download Salary Slip | Employee doesn't know salary is processed |
| SAL.2 | Salary marked **"Paid"** (by HR on web) | Badge on Profile section → "✅ Paid"; enable download | Employee expects payslip |

## Notifications (📲 Push + 🔔 In-App)

| # | Event | Delivery | Message | Deep Link |
|---|-------|----------|---------|-----------|
| SAL.N1 | **Payroll processed** for this employee | 📲 Push + 🔔 | "📄 Your salary for {month} has been processed." | → Screen 10 (Profile → Salary) |
| SAL.N2 | **Salary paid** / credited | 📲 Push + 🔔 | "💰 Salary for {month} credited! ₹{net_amount}. Download payslip." | → Screen 10 (Profile → Salary) |

## Downloads (📥)

| # | Screen | Button | API Endpoint | Format | Condition |
|---|--------|--------|--------------|--------|-----------|
| SAL.D1 | Screen 10: Profile → Salary Section | **📥 Download Latest Salary Slip** | `GET /v1/hrm/payslips/{employee_id}/latest/pdf` | PDF | Salary Status = Paid (current or latest month) |

### Salary Slip PDF Contents

| Section | Fields |
|---------|--------|
| Header | Company name, logo, "Salary Slip — {Month Year}" |
| Employee Info | Name, ID, Designation, Department, Branch, Bank A/C (masked) |
| Earnings | Basic Salary, HRA, Allowances, Incentive |
| Deductions | PF, ESI, TDS, Other deductions |
| Net Pay | Total Earnings − Total Deductions |
| Footer | Payment mode, Transaction ref, Generated date |

---

# ═══════════════════════════════════════════════════════════════
# 6. NOTIFICATION SYSTEM (Screen 19)
# ═══════════════════════════════════════════════════════════════

> **Source:** Screen 19 | All module event integrations

## Notification Center Architecture

```
┌─────────────────────────────────────────────────────────────┐
│              MOBILE NOTIFICATION SYSTEM                      │
│                                                              │
│  ┌──────────────────────┐    ┌──────────────────────┐       │
│  │   📲 PUSH (FCM/APNs) │    │  🔔 IN-APP (Bell)    │       │
│  │                      │    │                      │       │
│  │  • Delivered when    │    │  • Stored in local   │       │
│  │    app is in         │    │    notification DB   │       │
│  │    background/closed │    │  • Grouped by date   │       │
│  │  • Tap → deep link  │    │  • Read/Unread state │       │
│  │    to relevant screen│    │  • Badge count on 🔔 │       │
│  │  • Sound/vibrate     │    │  • Tap → deep link   │       │
│  └──────────────────────┘    └──────────────────────┘       │
│                                                              │
│  Backend: WebSocket (real-time) + REST fallback (polling)   │
└─────────────────────────────────────────────────────────────┘
```

### Screen 19 Layout Features

| Feature | Description |
|---------|-------------|
| **Grouped by Date** | Today, Yesterday, Earlier |
| **Read/Unread State** | Bold title + colored dot (🟢) for unread |
| **Mark All Read** | Header action [✓ Read] marks all as read |
| **Deep Link Routing** | Tapping any notification routes to the relevant screen |
| **Badge Count** | Bell icon (🔔) on Dashboard header shows unread count |
| **Retention** | Last 30 days of notifications retained |

### Complete Mobile Notification Matrix

Below is the **consolidated list of all notifications** that appear in the mobile notification center:

#### A. Attendance & Punch (ATT)

| # | Event | Push | In-App | Priority | Deep Link |
|---|-------|------|--------|----------|-----------|
| 1 | Shift start reminder (no punch-in) | ✅ | — | Normal | Screen 6 |
| 2 | Punch In confirmed | — | ✅ | Low | — |
| 3 | Late arrival flagged | ✅ | ✅ | High | Screen 9 |
| 4 | Punch Out confirmed | — | ✅ | Low | — |
| 5 | Auto Punch Out | ✅ | — | Normal | Screen 6 |
| 6 | Marked Absent | ✅ | — | High | Screen 9.1 |
| 7 | Punch Out reminder | ✅ | — | Normal | Screen 6 |

#### B. Tasks (TSK)

| # | Event | Push | In-App | Priority | Deep Link |
|---|-------|------|--------|----------|-----------|
| 8 | New task assigned | ✅ | ✅ | High | Screen 11 |
| 9 | Task reminder (day before) | ✅ | — | Normal | Screen 11 |
| 10 | Task reminder (1 hour before) | ✅ | — | Normal | Screen 11 |
| 11 | Task overdue | ✅ | ✅ | Critical | Screen 11 |
| 12 | Task rescheduled | ✅ | ✅ | High | Screen 11 |
| 13 | Task reassigned (to me) | ✅ | ✅ | High | Screen 11 |
| 14 | Task reassigned (from me) | ✅ | ✅ | Normal | — |
| 15 | Task completed | — | ✅ | Low | Screen 18 |

#### C. Petty Cash (PC)

| # | Event | Push | In-App | Priority | Deep Link |
|---|-------|------|--------|----------|-----------|
| 16 | Request submitted | — | ✅ | Low | Screen 21 |
| 17 | Request approved (L1) | ✅ | ✅ | Normal | Screen 21.2 |
| 18 | Forwarded to L2 | — | ✅ | Low | Screen 21.2 |
| 19 | Request approved (final) | ✅ | ✅ | Normal | Screen 21.2 |
| 20 | Request rejected | ✅ | ✅ | High | Screen 21.2 |
| 21 | Request returned | ✅ | ✅ | High | Screen 21.2 (Edit) |
| 22 | Request reimbursed/paid | ✅ | ✅ | Normal | Screen 21.2 |
| 23 | Approval overdue (48h) | — | ✅ | Normal | Screen 21 |
| 24 | Draft reminder (3 days) | ✅ | — | Low | Screen 21 |

#### D. Leave (LV)

| # | Event | Push | In-App | Priority | Deep Link |
|---|-------|------|--------|----------|-----------|
| 25 | Leave request submitted | — | ✅ | Low | Screen 9.1 |
| 26 | Leave approved | ✅ | ✅ | Normal | Screen 9.1 |
| 27 | Leave rejected | ✅ | ✅ | High | Screen 9.1 |
| 28 | Leave balance low warning | ✅ | — | Normal | Screen 9.1 |
| 29 | Leave pending >48h (escalated) | — | ✅ | Normal | Screen 9.1 |

#### E. Salary (SAL)

| # | Event | Push | In-App | Priority | Deep Link |
|---|-------|------|--------|----------|-----------|
| 30 | Payroll processed | ✅ | ✅ | Normal | Screen 10 |
| 31 | Salary paid/credited | ✅ | ✅ | High | Screen 10 |

#### F. Profile (PRF)

| # | Event | Push | In-App | Priority | Deep Link |
|---|-------|------|--------|----------|-----------|
| 32 | Profile update approved by HR | ✅ | ✅ | Normal | Screen 10 |
| 33 | Profile update rejected by HR | ✅ | ✅ | Normal | Screen 10 |
| 34 | Document expiry approaching (30d) | ✅ | ✅ | High | Screen 10 → Documents |

#### G. System (SYS)

| # | Event | Push | In-App | Priority | Deep Link |
|---|-------|------|--------|----------|-----------|
| 35 | App update available | ✅ | ✅ | Normal | App Store/Play Store |
| 36 | Session expiring soon | ✅ | — | Normal | Screen 2 (Login) |
| 37 | Account deactivated | ✅ | — | Critical | Screen 2 (Login) |

### Notification Priority Levels

| Priority | Behavior | Sound | Vibrate | Persist |
|----------|----------|-------|---------|---------|
| **Critical** | Heads-up banner + sound + vibrate | ✅ | ✅ | Until dismissed |
| **High** | Banner + vibrate | ✅ | ✅ | Auto-dismiss 10s |
| **Normal** | Standard notification | Optional | ✅ | Auto-dismiss 5s |
| **Low** | Silent / In-App only | ❌ | ❌ | — |

### Settings (Screen 22 → Notification Settings)

| Setting | Options | Default |
|---------|---------|---------|
| Push Notifications | On / Off | On |
| Task Alerts | On / Off | On (cannot disable) |
| Leave Updates | On / Off | On |
| Petty Cash Updates | On / Off | On |
| Salary Notifications | On / Off | On |
| Sound | On / Off / Custom | On |
| Vibration | On / Off | On |
| Quiet Hours | Start – End time | Off |

> **Mandatory:** Task Assigned and Task Overdue notifications **cannot be disabled** — they are mandatory for SLA compliance.

---

# ═══════════════════════════════════════════════════════════════
# 7. LEAVE MANAGEMENT (Screens 9.1, 9.1.1)
# ═══════════════════════════════════════════════════════════════

> **Source:** Screen 9.1 + 9.1.1 | Module 25 – HRM Leave

## Auto-Events

| # | Trigger Condition | System Action | Business Impact |
|---|-------------------|---------------|-----------------|
| LV.1 | "Submit Request" tapped (Screen 9.1.1) | Auto-route to **Reporting Manager** | Leave without approval |
| LV.2 | Leave approved (by Manager on web/mobile) | Deduct from employee balance; update Calendar (Screen 9) colors | Incorrect balance |
| LV.3 | Leave rejected | Restore tentative deduction; notification to employee | Employee unaware |
| LV.4 | Leave balance = **0** for selected type | **Block** submission: "You don't have leave balance for selected leave type." | Negative balance |
| LV.5 | Leave dates **overlap** with existing approved leave | **Block** submission: "Leave already exists for selected dates." | Double booking |
| LV.6 | From Date is **past date** | **Block** submission: "From Date cannot be in the past." | Backdating |
| LV.7 | Leave pending approval > **48 hrs** | Escalate to HR Manager | Employee in limbo |

## Notifications

(Covered in Section 6 → Leave group, items #25-29)

## Downloads

> No download actions on Leave screens.

---

# ═══════════════════════════════════════════════════════════════
# 8. DOCUMENT DOWNLOAD (Screen 10 — Profile Documents)
# ═══════════════════════════════════════════════════════════════

> **Source:** Screen 10 → Section 6 (Documents) | Module 27 – User Profile

## Downloads (📥)

| # | Screen | Button | API Endpoint | Format | Condition |
|---|--------|--------|--------------|--------|-----------|
| DOC.D1 | Screen 10: Profile → Documents | **📥** (per document row) | `GET /v1/employees/{id}/documents/{doc_id}/download` | Original format (PDF/JPG/PNG) | Document exists (✅ uploaded) |
| DOC.D2 | Screen 10: Profile → Documents | **👁** (per document row) | `GET /v1/employees/{id}/documents/{doc_id}/preview` | In-app viewer | Document exists |

### Available Documents

| Document | Uploadable by Employee | Download | View |
|----------|------------------------|----------|------|
| Gov ID Proof (Aadhar/PAN) | ✅ Editable | 📥 | 👁 |
| Address Proof | ✅ Editable | 📥 | 👁 |
| Employment Contract | ❌ HR only | 📥 | 👁 |
| Education Certificates | ✅ Editable | 📥 | 👁 |
| Other Documents | ✅ Editable | 📥 | 👁 |

---

# ═══════════════════════════════════════════════════════════════
# SUMMARY TABLES
# ═══════════════════════════════════════════════════════════════

## 📥 All Mobile Download Buttons (Complete List)

| # | Screen | Button Label | Format | Endpoint | Condition |
|---|--------|--------------|--------|----------|-----------|
| 1 | Screen 18: Service Report | 📥 DOWNLOAD PDF | PDF | `/v1/tasks/{id}/service-report/pdf` | Task completed |
| 2 | Screen 11: Task History | 📄 View Previous Report | PDF | `/v1/tasks/{prev_id}/service-report/pdf` | Previous task exists |
| 3 | Screen 10: Profile → Salary | 📥 Download Latest Salary Slip | PDF | `/v1/hrm/payslips/{emp}/latest/pdf` | Status = Paid |
| 4 | Screen 10: Profile → Documents | 📥 Download (per doc) | Original | `/v1/employees/{id}/documents/{doc_id}/download` | Document uploaded |

> **Total: 4 download actions** on mobile (vs 31 on web). Mobile is intentionally lean.

---

## 📧 All Mobile External Sends (Complete List)

| # | Screen | Action | What is Sent | To Whom |
|---|--------|--------|--------------|---------|
| 1 | Screen 18: Service Report → Share | [SHARE] button | Service Report PDF + Service Logs + Previous Report + Invoice (if any) | Selected customer contacts via Email |

> **Total: 1 external send** from mobile. All other notifications are internal system-level.

---

## 📲 Push Notification Trigger Summary

| Category | Push Count | Critical | High | Normal | Low |
|----------|------------|----------|------|--------|-----|
| Attendance | 5 | — | 1 | 4 | — |
| Tasks | 7 | 1 | 3 | 2 | 1 |
| Petty Cash | 5 | — | 2 | 2 | 1 |
| Leave | 3 | — | 1 | 2 | — |
| Salary | 2 | — | 1 | 1 | — |
| Profile | 2 | — | 1 | 1 | — |
| System | 2 | 1 | — | 1 | — |
| **Total** | **26** | **2** | **9** | **13** | **2** |

---

## 🔗 Deep Link Mapping

| Deep Link Target | Screen | Used By (Notification IDs) |
|------------------|--------|----------------------------|
| Dashboard (Attendance) | Screen 6 | ATT.N1, ATT.N5, ATT.N7 |
| Calendar | Screen 9 | ATT.N3 |
| Leaves Tab | Screen 9.1 | ATT.N6, LV.N25-29 |
| Task Detail | Screen 11 | TSK.N1-6, N8-14 |
| Service Report | Screen 18 | TSK.N15 |
| Profile | Screen 10 | SAL.N30-31, PRF.N32-34 |
| Petty Cash List | Screen 21 | PC.N16, N23, N24 |
| Petty Cash Detail | Screen 21.2 | PC.N17-22 |
| Login | Screen 2 | SYS.N36, N37 |

---

## Backend Integration Points

```
Mobile App → Backend APIs Required:

ATTENDANCE
  POST  /v1/attendance/punch-in         (GPS coords, timestamp)
  POST  /v1/attendance/punch-out        (GPS coords, timestamp)
  GET   /v1/attendance/status/today     (current punch status)

TASKS
  GET   /v1/tasks/assigned/today        (dashboard tasks)
  GET   /v1/tasks/assigned              (all assigned tasks)
  GET   /v1/tasks/{id}                  (task detail)
  POST  /v1/tasks/{id}/start-travel     (GPS, timestamp)
  POST  /v1/tasks/{id}/confirm-arrival  (GPS, geo-fence validation)
  POST  /v1/tasks/{id}/execution        (steps 1-5 data payload)
  POST  /v1/tasks/{id}/complete         (OTP, rating, finalize)
  GET   /v1/tasks/{id}/service-report/pdf  (📥 download)

PETTY CASH
  GET   /v1/petty-cash/my-requests      (list with filters)
  POST  /v1/petty-cash                  (create new request)
  PUT   /v1/petty-cash/{id}             (edit draft/returned)
  POST  /v1/petty-cash/{id}/submit      (submit for approval)

SALARY
  GET   /v1/hrm/payslips/{emp}/latest/pdf  (📥 download)

PROFILE
  GET   /v1/employees/{id}/profile      (view profile)
  PUT   /v1/employees/{id}/profile      (update editable fields)
  GET   /v1/employees/{id}/documents/{doc}/download  (📥 download)

LEAVE
  GET   /v1/leaves/my-requests          (history with filters)
  POST  /v1/leaves                      (submit leave request)
  GET   /v1/leaves/balance              (leave balances)

NOTIFICATIONS
  GET   /v1/notifications               (paginated list)
  PUT   /v1/notifications/{id}/read     (mark read)
  PUT   /v1/notifications/read-all      (mark all read)
  WS    /v1/notifications/stream        (WebSocket for real-time)

LOCATION
  POST  /v1/location/ping               (periodic GPS update)
  GET   /v1/location/geo-fence/{site}   (site radius check)
```

---

> **End of Document**  
> Total Auto-Events: 20  
> Total Push Notifications: 26 triggers  
> Total In-App Notifications: 37 types  
> Total Download Actions: 4  
> Total External Sends: 1 (Service Report Share)
