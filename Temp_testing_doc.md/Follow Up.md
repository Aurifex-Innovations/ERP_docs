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
