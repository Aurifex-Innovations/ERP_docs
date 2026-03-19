## Add Follow Up Form
```
┌────────────────────────────────────────────────────────────────────────────┐
│ ← Back              Add Follow-Up                         [ Save Follow-Up ]│
├────────────────────────────────────────────────────────────────────────────┤
│                                                                            │
│ 🔹 LEAD INFORMATION (Auto Filled / Read Only)                               │
│ Lead ID           [ LD-24001 ]        Lead Name        [ Rahul Shah ]       │
│ Phone Number      [ 9876543210 ]      Company          [ ABC Corp ]         │
│ Current Status    [ New ]             Assigned To      [ Suraj ]            │
│                                                                            │
│ 🔹 FOLLOW-UP DETAILS                                                        │
│ Follow-Up Date *  [ 📅 Select Date ]   Follow-Up Time * [ Select ▼ ]        │
│ Follow-Up Type *  [ Select ▼ ]                                          │
│ ├─ Call                                                                     │
│ ├─ WhatsApp                                                                 │
│ ├─ Email                                                                    │
│ ├─ Meeting                                                                  │
│ ├─ Site Visit                                                               │
│ └─ Other                                                                    │
│                                                                            │
│ 🔹 CONVERSATION SUMMARY                                                     │
│ Discussion Notes * [______________________________________________]         │
│                    [______________________________________________]         │
│                                                                            │
│ 🔹 STATUS UPDATE                                                            │
│ Update Lead Status  [ Select ▼ ]                                           │
│ ├─ No Change                                                                 │
│ ├─ Contacted                                                                 │
│ ├─ Qualified                                                                 │
│ ├─ Proposal Sent                                                             │
│ ├─ Negotiation                                                               │
│ ├─ Won                                                                       │
│ └─ Lost                                                                      │
│                                                                            │
│ 🔹 NEXT ACTION                                                              │
│ Next Follow-Up Date  [ 📅 Select Date ]   Preferred Time [ Select ▼ ]       │
│ Priority            [ Select ▼ ]                                           │
│ ├─ High                                                                     │
│ ├─ Medium                                                                   │
│ └─ Low                                                                      │
│                                                                            │
│ 🔹 DEAL UPDATE (Optional)                                                   │
│ Estimated Value ₹ [__________]      Probability % [____]                    │
│                                                                            │
│ 🔹 OUTCOME (Auto if Closed)                                                 │
│ Outcome            [ Select ▼ ]                                            │
│ ├─ Interested                                                                │
│ ├─ Not Interested                                                            │
│ ├─ Call Back Later                                                           │
│ ├─ Quotation Sent
│ ├─ Converted                                                                 │
│ └─ Lost                                                                      │
│                                                                            │
│ Reason (if Lost)   [_____________]                                         │
│                                                                            │
├────────────────────────────────────────────────────────────────────────────┤
│ [ Cancel ]                                        [ Save Follow-Up ]       │
└────────────────────────────────────────────────────────────────────────────┘
```

### Validations

| Section                     | Field Name             | Validation Rules                                                                 | Required |
|----------------------------|------------------------|----------------------------------------------------------------------------------|----------|
| Lead Information           | Lead ID                | Must exist in system, auto-filled, cannot be edited                              | Yes      |
|                            | Lead Name              | Auto-filled, text only                                                           | Yes      |
|                            | Phone Number           | Must be valid 10-digit number                                                    | Yes      |
|                            | Company                | Text (max 100 chars)                                                             | No       |
|                            | Current Status         | Must match existing lead status                                                  | Yes      |
|                            | Assigned To            | Must be valid user from system                                                   | Yes      |
|----------------------------|------------------------|----------------------------------------------------------------------------------|----------|
| Follow-Up Details          | Follow-Up Date         | Cannot be past date (except admin override)                                      | Yes      |
|                            | Follow-Up Time         | Required if date selected                                                        | Yes      |
|                            | Follow-Up Type         | Must be one of: Call, WhatsApp, Email, Meeting, Site Visit, Other                | Yes      |
|----------------------------|------------------------|----------------------------------------------------------------------------------|----------|
| Conversation Summary       | Discussion Notes       | Minimum 10 characters, max 1000 characters                                       | Yes      |
|----------------------------|------------------------|----------------------------------------------------------------------------------|----------|
| Status Update              | Update Lead Status     | Must be valid status from predefined list                                        | Yes      |
|----------------------------|------------------------|----------------------------------------------------------------------------------|----------|
| Next Action                | Next Follow-Up Date    | Must be future date if status ≠ Won/Lost                                         | Conditional |
|                            | Preferred Time         | Required if next follow-up date selected                                         | Conditional |
|                            | Priority               | Must be one of: High, Medium, Low                                                | No       |
|----------------------------|------------------------|----------------------------------------------------------------------------------|----------|
| Deal Update                | Estimated Value        | Must be numeric, ≥ 0                                                             | No       |
|                            | Probability %          | Must be between 0–100                                                            | No       |
|----------------------------|------------------------|----------------------------------------------------------------------------------|----------|
| Outcome                    | Outcome                | Required if status updated to Won or Lost                                        | Conditional |
|                            | Reason (if Lost)       | Required if status = Lost                                                        | Conditional |
|----------------------------|------------------------|----------------------------------------------------------------------------------|----------|
| General Rules              | Duplicate Entry        | Prevent duplicate follow-up for same lead at same date & time                    | Yes      |
|                            | Mandatory Fields       | All * marked fields must be filled                                               | Yes      |
|                            | Form Submission        | Disable submit until required fields are valid                                   | Yes      |
|                            | Auto Timestamp         | System should store created date & time automatically                            | Yes      |


## Follow up dashboard (Table view)
```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│  ← Back to Leads                                                                    │
│                                                                                         │
│  FOLLOW-UP MANAGEMENT                                            [+ Add Follow-Up]      │
│  Track and manage all follow-ups for your leads                                       │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                         │
│  ┌─────────────────────────────────────────────────────────────────────────────────┐    │
│  │                                                                                 │    │
│  │   🔍 Search by Lead Name, Phone, Lead ID, Company...                      [🔍] │    │
│  │                                                                                 │    │
│  └─────────────────────────────────────────────────────────────────────────────────┘    │
│                                                                                         │
│  ┌─────────────────────────────────────────────────────────────────────────────────┐    │
│  │   FILTER BY:                                                                     │    │
│  │                                                                                  │    │
│  │   Follow-Up Type: [All Types                         ▼]                         │    │
│  │   ├─ Call                                                                       │    │
│  │   ├─ WhatsApp                                                                   │    │
│  │   ├─ Email                                                                      │    │
│  │   ├─ Meeting                                                                    │    │
│  │   ├─ Site Visit                                                                 │    │
│  │   └─ Other                                                                      │    │
│  │                                                                                  │    │
│  │   Status:         [All Status                        ▼]                         │    │
│  │   ├─ New                                                                        │    │
│  │   ├─ Contacted                                                                  │    │
│  │   ├─ Qualified                                                                  │    │
│  │   ├─ Proposal Sent                                                              │    │
│  │   ├─ Negotiation                                                                │    │
│  │   ├─ Won                                                                        │    │
│  │   └─ Lost                                                                       │    │
│  │                                                                                  │    │
│  │   Outcome:        [All Outcomes                      ▼]                         │    │
│  │   ├─ Interested                                                                 │    │
│  │   ├─ Not Interested                                                             │    │
│  │   ├─ Call Back Later                                                            │    │
│  │   ├─ Converted                                                                  │    │
│  │   └─ Lost                                                                       │    │
│  │                                                                                  │    │
│  │   Assigned To:   [All Team Members                 ▼]                         │    │
│  │                                                                                  │    │
│  │   Priority:      [All Priority Levels             ▼]                         │    │
│  │   ├─ High                                                                      │    │
│  │   ├─ Medium                                                                    │    │
│  │   └─ Low                                                                       │    │
│  │                                                                                  │    │
│  │   Follow-Up Date: [All Dates ▼]   [Apply]   [Reset All]                         │    │
│  │   ├─ Today                                                                     │    │
│  │   ├─ Tomorrow                                                                  │    │
│  │   ├─ This Week                                                                 │    │
│  │   ├─ Overdue                                                                   │    │
│  │   └─ Custom Range                                                              │    │
│  │                                                                                  │    │
│  └─────────────────────────────────────────────────────────────────────────────────┘    │
│                                                                                         │
│  ┌─────────────────────────────────────────────────────────────────────────────────┐    │
│  │   Active Filters: [Type: Call ✕] [Status: Contacted ✕] [Clear All]             │    │
│  └─────────────────────────────────────────────────────────────────────────────────┘    │
│                                                                                         │
│  ┌─────────────────────────────────────────────────────────────────────────────────┐    │
│  │                                                                                  │    │
│  │   Total Follow-Ups: 320   Today: 45   Upcoming: 180   Overdue: 95               │    │
│  │                                                                                  │    │
│  │   ┌────────┬──────────────┬──────────────┬─────────────┬─────────────┬────────────┬────────────┬────────────┬────────────┬────────┐ │
│  │   │ FU ID  │ Lead Name    │ Phone        │ Type        │ Status      │ Outcome    │ Date       │ Time       │ Assigned   │ Actions│ │
│  │   ├────────┼──────────────┼──────────────┼─────────────┼─────────────┼────────────┼────────────┼────────────┼────────────┼────────┤ │
│  │   │FU-001  │ Rahul Shah   │ 9876543210   │ Call        │ Contacted   │ Interested │ 18/03/2026 │ 02:00 PM   │ Suraj     │ View   │ │
│  │   ├────────┼──────────────┼──────────────┼─────────────┼─────────────┼────────────┼────────────┼────────────┼────────────┼────────┤ │
│  │   │FU-002  │ Priya Mehta  │ 9123456789   │ Email       │ Qualified   │ Call Back  │ 19/03/2026 │ 11:00 AM   │ Amit      │ Edit   │ │
│  │   ├────────┼──────────────┼──────────────┼─────────────┼─────────────┼────────────┼────────────┼────────────┼────────────┼────────┤ │
│  │   │FU-003  │ Vikram Rao   │ 9988776655   │ Meeting     │ Proposal    │ Interested │ 17/03/2026 │ 04:30 PM   │ Priya     │ Delete │ │
│  │   ├────────┼──────────────┼──────────────┼─────────────┼─────────────┼────────────┼────────────┼────────────┼────────────┼────────┤ │
│  │   │FU-004  │ Sunita Sharma│ 9876512345   │ Site Visit  │ Negotiation │ Converted  │ 20/03/2026 │ 01:00 PM   │ Rahul     │        │ │
│  │   ├────────┼──────────────┼──────────────┼─────────────┼─────────────┼────────────┼────────────┼────────────┼────────────┼────────┤ │
│  │   │FU-005  │ Amit Patel   │ 9998887776   │ WhatsApp    │ Contacted   │ Not Inter. │ 16/03/2026 │ 06:00 PM   │ Suraj     │        │ │
│  │   └────────┴──────────────┴──────────────┴─────────────┴─────────────┴────────────┴────────────┴────────────┴────────────┴────────┘ │
│  │                                                                                  │    │
│  │   Showing 1–5 of 320 follow-ups                                                  │    │
│  │                                                                                  │    │
│  │   [Rows per page: 10 ▼]   [< Previous]  [1]  [2]  [3] ... [32]  [Next >]         │    │
│  │                                                                                  │    │
│  └─────────────────────────────────────────────────────────────────────────────────┘    │
│                                                                                         │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

## TABLE COLUMNS SPECIFICATION
| Column Name    | Display Name | Data Type     | Description                                  | Example Value | Required | Sortable | Searchable |
|----------------|-------------|---------------|----------------------------------------------|--------------|----------|----------|------------|
| followup_id    | FU ID       | VARCHAR(20)   | Unique follow-up identifier                  | FU-001       | Yes      | Yes      | Yes        |
| lead_name      | Lead Name   | VARCHAR(100)  | Name of the lead/customer                    | Rahul Shah   | Yes      | Yes      | Yes        |
| phone_number   | Phone       | VARCHAR(15)   | Contact number                              | 9876543210   | Yes      | No       | Yes        |
| followup_type  | Type        | ENUM          | Type of follow-up (Call, Email, etc.)       | Call         | Yes      | Yes      | Yes        |
| lead_status    | Status      | ENUM          | Current lead status                         | Contacted    | Yes      | Yes      | Yes        |
| outcome        | Outcome     | ENUM          | Result of follow-up                         | Interested   | No       | Yes      | Yes        |
| followup_date  | Date        | DATE          | Follow-up scheduled date                    | 18/03/2026   | Yes      | Yes      | Yes        |
| followup_time  | Time        | TIME          | Follow-up scheduled time                    | 02:00 PM     | Yes      | Yes      | No         |
| assigned_to    | Assigned    | VARCHAR(50)   | Person responsible for follow-up            | Suraj        | Yes      | Yes      | Yes        |
| actions        | Actions     | UI (Buttons)  | Actions like View, Edit, Delete             | View         | Yes      | No       | No         |


## FILTER DROPDOWN VALUES (COMPLETE LIST)
| Filter Name        | Dropdown Values                                                                 |
|--------------------|----------------------------------------------------------------------------------|
| Follow-Up Type     | All Types, Call, WhatsApp, Email, Meeting, Site Visit, Other                    |
| Status             | All Status, New, Contacted, Qualified, Proposal Sent, Negotiation, Won, Lost   |
| Outcome            | All Outcomes, Interested, Not Interested, Call Back Later, Converted, Lost     |
| Assigned To        | All Team Members, Suraj, Amit, Priya, Rahul, Sales Team 1                      |
| Priority           | All Priority Levels, High, Medium, Low                                         |
| Follow-Up Date     | All Dates, Today, Tomorrow, This Week, Overdue, Custom Range                   |

## SEARCHABLE FIELDS (for Search Bar)
- Follow-Up ID
- Lead ID
- Lead Name
- Phone Number
- Company Name
- Assigned To
- Notes (optional, if needed)
