
================================================================================

# 🧪 Test Cases & Cross-Module Field Integration

## Fields Reused from Previous Modules

| Field / Data             | Source Module            | Used In (Module 25)                         |
| ------------------------ | ----------------------- | ------------------------------------------- |
| Emp ID                   | Module 8 (Step 1)       | Employee Table, Salary, Attendance, Leave   |
| Employee Name            | Module 8 (Step 1)       | All screens                                 |
| Email ID                 | Module 8 (Step 1)       | Employee Table                              |
| Contact Number           | Module 8 (Step 1)       | Employee Table                              |
| Designation              | Module 8 (Step 1)       | Employee Table                              |
| Department               | Module 8 (Step 1)       | Employee Table, Leave Requests              |
| Role                     | Module 8 (Step 1)       | Employee Table, RBAC                        |
| Branch                   | Module 8 / Module 7     | Employee Table, Filters, RBAC               |
| Reporting Manager        | Module 8 (Step 1)       | Employee Table                              |
| Status (Active/Inactive) | Module 8 (Step 1)       | Employee Table filter                       |
| Created Date             | Module 8 (Step 1)       | Employee Table                              |
| Salary Type              | Module 6 (Step 2)       | Salary View — Salary Info header            |
| Salary Eff. From/To      | Module 6 (Step 2)       | Salary View — Salary Info header            |
| Basic Salary             | Mod 6 S2 → Mod 8 S3    | Salary View — earnings                      |
| HRA                      | Mod 6 S2 → Mod 8 S3    | Salary View — earnings                      |
| Other Allowance          | Mod 6 S2 → Mod 8 S3    | Salary View — earnings                      |
| Incentive                | Mod 6 S2 → Mod 8 S3    | Salary View — earnings                      |
| Deductions (General)     | Mod 6 S2 → Mod 8 S3    | Salary View — deductions                    |
| PF Applicable            | Module 6 (Step 2)       | Salary View — statutory (conditional)       |
| ESI Applicable           | Module 6 (Step 2)       | Salary View — statutory (conditional)       |
| TDS Applicable           | Module 6 (Step 2)       | Salary View — statutory (conditional)       |
| Holiday Work Incentive   | Module 6 (Step 2)       | Salary View — incentive config              |
| Overtime Config          | Module 6 (Step 2)       | Salary View — incentive config              |
| Bank Name                | Module 8 (Step 3)       | Salary Slip generation                      |
| Account Number           | Module 8 (Step 3)       | Salary Slip generation                      |
| IFSC Code                | Module 8 (Step 3)       | Salary Slip generation                      |
| Casual Leave (CL)        | Mod 6 S3 → Mod 8 S4    | Leave Balance, Leave Form                   |
| Sick Leave (SL)          | Mod 6 S3 → Mod 8 S4    | Leave Balance, Leave Form                   |
| Paid Leave (PL)          | Mod 6 S3 → Mod 8 S4    | Leave Balance, Leave Form                   |
| Annual Leave Allocation  | Mod 6 S3 → Mod 8 S4    | Leave Balance, Leave Form                   |
| Carry Forward Allowed    | Module 6 (Step 3)       | Leave Balance display                       |
| Max Carry Forward Days   | Module 6 (Step 3)       | Leave Balance display                       |
| Leave Approval Authority | Module 6 (Step 3)       | Leave Request routing, Leave Popup          |
| Leave Reset Cycle        | Module 6 (Step 3)       | Leave balance reset                         |
| Weekly Off               | Module 8 (Step 5)       | Attendance Calendar — WO marking            |
| Task Started At          | Module 21 (21.6)        | Attendance — Punch In reference             |
| Task Completed At        | Module 21 (21.6)        | Attendance — Punch Out reference            |
| Tasks Assigned/Completed | Module 21 (21.2)        | Attendance Day Popup — task summary         |
| Branch List              | Module 7                | Filters                                     |
| Role List                | Module 5                | RBAC, Filters                               |

---

## HRM-Specific New Fields (Introduced in Module 25)

| Field               | Screen            | Type     | Description                         |
| ------------------- | ----------------- | -------- | ----------------------------------- |
| Payment Status      | Salary View       | Dropdown | Paid / Unpaid / Due                 |
| Reason (Salary)     | Salary View       | Textarea | Reason for Unpaid/Due status        |
| Payment Date        | Salary View       | Date     | Date salary was paid                |
| Gross Salary        | Salary View       | Currency | Auto-calculated total earnings      |
| Total Deductions    | Salary View       | Currency | Auto-calculated total deductions    |
| Net Salary          | Salary View       | Currency | Auto-calculated net pay             |
| Other Deductions    | Salary View       | Currency | HRM-specific additional deductions  |
| OT Hours This Month | Salary View       | Number   | Actual OT hours for the month       |
| OT Amount           | Salary View       | Currency | Auto-calculated overtime pay        |
| Holiday Days Worked | Salary View       | Number   | Actual holiday days worked          |
| Holiday Incentive   | Salary View       | Currency | Auto-calculated holiday pay         |
| Punch In Time       | Attendance Popup  | Time     | Employee check-in time              |
| Punch Out Time      | Attendance Popup  | Time     | Employee check-out time             |
| Total Hours         | Attendance Popup  | Display  | Auto-calculated work hours          |
| Attendance Status   | Attendance Cal.   | Badge    | Present/Absent/Leave/WO/Holiday     |
| Leave ID            | Leave Form        | Text     | Unique leave request identifier     |
| Leave Type          | Leave Form        | Dropdown | CL / SL / PL                       |
| From Date (Leave)   | Leave Form        | Date     | Leave start date                    |
| To Date (Leave)     | Leave Form        | Date     | Leave end date                      |
| Total Days (Leave)  | Leave Form        | Number   | Auto-calculated leave days          |
| Description (Leave) | Leave Form        | Textarea | Reason for leave                    |
| Leave Status        | Leave Form/Tab 2  | Badge    | Pending / Approved / Rejected       |
| Reason (Rejection)  | Leave Popup       | Textarea | Mandatory reason for rejection      |
| Reviewed By         | Leave Popup       | Display  | Auto-captured reviewer name         |
| Review Date         | Leave Popup       | Display  | Auto-captured review timestamp      |

---

## Test Cases

### TC-25.1: Employee Management Table

| TC ID    | Test Case                                          | Expected Result                                    |
| -------- | -------------------------------------------------- | -------------------------------------------------- |
| TC-25.1.1| Load Employee Management Tab                       | Table displays all employees from Module 8         |
| TC-25.1.2| Apply Branch filter                                | Table shows employees of selected branch only      |
| TC-25.1.3| Search by Employee ID                              | Matching records displayed                         |
| TC-25.1.4| Search by partial name                             | Fuzzy/partial match results shown                  |
| TC-25.1.5| Click Salary button                                | Salary View (25.2) opens for that employee         |
| TC-25.1.6| Click Attendance button                            | Attendance Calendar (25.3) opens for that employee |
| TC-25.1.7| Click Leave button                                 | Leave Entry Form (25.4) opens for that employee    |
| TC-25.1.8| Filter by Status = Inactive                       | Only inactive employees shown                      |

### TC-25.2: Salary View

| TC ID    | Test Case                                          | Expected Result                                    |
| -------- | -------------------------------------------------- | -------------------------------------------------- |
| TC-25.2.1| Open Salary View                                   | Pre-populated with Mod 6 S2 → Mod 8 S3 salary data|
| TC-25.2.2| Verify Salary Type display                         | Shows CTC/Fixed/Hourly from Module 6 config        |
| TC-25.2.3| Verify Salary Effective dates                      | Shows dates from Module 6 role config              |
| TC-25.2.4| Change month filter                                | Displays salary data for selected month            |
| TC-25.2.5| Edit Basic Salary to 0                             | Allowed (minimum 0)                                |
| TC-25.2.6| Edit salary and click Save                         | Changes saved, toast confirmation                  |
| TC-25.2.7| Mark as Paid                                       | Status = Paid, Payment Date auto-set               |
| TC-25.2.8| Download Salary Slip when Status = Paid            | PDF downloads successfully                         |
| TC-25.2.9| Download Salary Slip when Status = Unpaid          | Button disabled / error toast                      |
| TC-25.2.10| Set Status = Unpaid without Reason                | Validation error — Reason required                 |
| TC-25.2.11| Upload invalid file format (.txt)                 | Error: Unsupported format                          |
| TC-25.2.12| Upload valid Excel with invalid Emp IDs           | Error rows flagged, valid rows imported             |
| TC-25.2.13| Verify Net Salary calculation                     | Net = Gross + Incentives − All Deductions          |
| TC-25.2.14| PF row hidden when PF Applicable = No (Mod 6)    | PF deduction field not shown                       |
| TC-25.2.15| ESI row hidden when ESI Applicable = No (Mod 6)  | ESI deduction field not shown                      |
| TC-25.2.16| TDS row hidden when TDS Applicable = No (Mod 6)  | TDS deduction field not shown                      |
| TC-25.2.17| Holiday Incentive section shown (Mod 6 = Yes)     | Fields visible, amount auto-calculated             |
| TC-25.2.18| Holiday Incentive section hidden (Mod 6 = No)     | Section not displayed                              |
| TC-25.2.19| Overtime section shown (Mod 6 = Yes)              | OT fields visible, amount auto-calculated          |
| TC-25.2.20| Overtime section hidden (Mod 6 = No)              | Section not displayed                              |
| TC-25.2.21| OT hours exceed Max OT Hours/Month                | Warning: exceeds role config limit                 |

### TC-25.3: Attendance

| TC ID    | Test Case                                          | Expected Result                                    |
| -------- | -------------------------------------------------- | -------------------------------------------------- |
| TC-25.3.1| Open Attendance Calendar                           | Calendar shows current month with status per day   |
| TC-25.3.2| Click on a date                                    | Popup opens with Punch In/Out fields               |
| TC-25.3.3| Enter Punch In > Punch Out                         | Validation error: Punch In must be before Punch Out|
| TC-25.3.4| Enter valid Punch In/Out and Save                  | Attendance saved, calendar updates to 🟢 Present   |
| TC-25.3.5| Date with approved leave                           | Calendar shows 🔵 Leave (auto from leave records)  |
| TC-25.3.6| Date with tasks completed (Module 21)              | Popup shows task count from Module 21              |
| TC-25.3.7| Weekly off day                                     | Calendar shows 🟡 WO (from Module 8 config)       |
| TC-25.3.8| Upload attendance via Excel                        | Data imported, calendar refreshes                  |
| TC-25.3.9| Monthly summary calculation                        | Correct count of Present/Absent/Leave/WO/HD        |

### TC-25.4: Leave Entry

| TC ID    | Test Case                                          | Expected Result                                    |
| -------- | -------------------------------------------------- | -------------------------------------------------- |
| TC-25.4.1| Open Leave Form                                    | Shows balance from Mod 6 S3 → Mod 8 S4            |
| TC-25.4.2| Verify Annual Leave Allocation displayed           | Shows total annual allocation from Module 6        |
| TC-25.4.3| Verify Carry Forward display                       | Shows Allowed/Not + Max days from Module 6         |
| TC-25.4.4| Verify Leave Approval Authority displayed          | Shows approval role from Module 6 config           |
| TC-25.4.5| Verify Leave Reset Cycle displayed                 | Shows Yearly/Monthly/Custom from Module 6          |
| TC-25.4.6| Submit leave with From > To date                   | Validation error                                   |
| TC-25.4.7| Submit leave exceeding available balance            | Validation error — insufficient balance            |
| TC-25.4.8| Submit leave with Status = Approved          | Status = Approved, deducted from balance, calendar updated, does NOT appear in Tab 2  |
| TC-25.4.9 | Submit leave with Status = Pending           | Status = Pending, appears in Tab 2 for approval  |
| TC-25.4.10| Submit leave with < 10 char description            | Validation error                                   |
| TC-25.4.11| Submit overlapping leave dates                    | Validation error — dates overlap with existing     |
| TC-25.4.12| Total Days excludes weekends/holidays             | Auto-calculation correct                           |
| TC-25.4.13| View Leave History table                          | Past leave records displayed correctly             |

### TC-25.5: Leave Requests (Tab 2)

| TC ID    | Test Case                                          | Expected Result                                    |
| -------- | -------------------------------------------------- | -------------------------------------------------- |
| TC-25.5.1| Load Leave Requests tab                            | Displays all pending/processed requests from **Mobile/Self-Service** |
| TC-25.5.2| Filter by Status = Pending                         | Only pending requests shown                        |
| TC-25.5.3| Click View on a request                            | Popup opens with full leave details                |
| TC-25.5.4| Approve a pending request                          | Status → Approved, leave balance deducted          |
| TC-25.5.5| Reject without reason                              | Validation error — reason required                 |
| TC-25.5.6| Reject with reason (≥10 chars)                     | Status → Rejected, reason saved                    |
| TC-25.5.7| Mark Pending on approved request                   | Status reverts to Pending, balance restored         |
| TC-25.5.8| Employee (App User) leave appears here             | Leave submitted via app shows in Tab 2             |
| TC-25.5.9| Reviewed By auto-captured                          | Current logged-in user name recorded               |
| TC-25.5.10| Review Date auto-captured                         | Current date/time recorded                         |

### TC-25.6: Cross-Module Integration

| TC ID    | Test Case                                          | Expected Result                                    |
| -------- | -------------------------------------------------- | -------------------------------------------------- |
| TC-25.6.1| New employee added in Module 8                     | Appears in Module 25 Employee Table                |
| TC-25.6.2| Employee deactivated in Module 8                   | Status badge = Inactive in Module 25               |
| TC-25.6.3| Role config created in Module 6                    | New role's salary/leave defaults available          |
| TC-25.6.4| Module 6 Salary Type changed for a role            | Salary View shows updated type for employees       |
| TC-25.6.5| Module 6 PF Applicable toggled Off                 | PF row hidden in Salary View for that role         |
| TC-25.6.6| Module 6 Holiday Incentive enabled for role        | Incentive section appears in Salary View           |
| TC-25.6.7| Module 6 Overtime enabled for role                 | Overtime section appears in Salary View            |
| TC-25.6.8| Module 6 Leave config updated (CL/SL/PL days)     | New balance reflected in Leave Form                |
| TC-25.6.9| Module 6 Carry Forward toggled On                  | Carry Forward info displayed in Leave Balance      |
| TC-25.6.10| Module 6 Leave Approval Authority changed         | Updated authority shown in Leave Form/Popup        |
| TC-25.6.11| Module 6 Leave Reset Cycle changed                | Reset cycle info updated in Leave Balance          |
| TC-25.6.12| Salary config overridden in Module 8 Step 3       | Module 25 uses employee-level override             |
| TC-25.6.13| Task completed in Module 21                       | Attendance calendar reflects task log data          |
| TC-25.6.14| Branch deleted in Module 7                        | Employees of that branch handled gracefully        |
| TC-25.6.15| Role permissions checked                          | Admin/HR see all; Manager sees own branch/team     |
| TC-25.6.16| App User submits leave                            | Request visible in Tab 2 for Admin/HR              |

### TC-25.7: RBAC & Security

| TC ID    | Test Case                                          | Expected Result                                    |
| -------- | -------------------------------------------------- | -------------------------------------------------- |
| TC-25.7.1| Employee (App User) tries to access Tab 1          | Access denied / not visible                        |
| TC-25.7.2| Manager tries to edit salary                       | Access denied                                      |
| TC-25.7.3| Manager views attendance of own branch             | Allowed                                            |
| TC-25.7.4| Manager views attendance of other branch            | Access denied                                      |
| TC-25.7.5| HR approves leave                                  | Allowed                                            |
| TC-25.7.6| Employee downloads own salary slip                 | Allowed (when Paid)                                |
| TC-25.7.7| Employee downloads other's salary slip              | Access denied                                      |

---

> **Document Version:** 2.0 (Updated with Module 6 dynamic configuration integration)
> **Last Updated:** 25-Mar-2026
> **Module Owner:** Product Management / HR Operations
> **Audience:** Developers, UI/UX Designers, QA Team, Stakeholders
