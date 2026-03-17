## Add lead form
```md
┌────────────────────────────────────────────────────────────────────────────┐
│ ← Back                Add New Lead                         [ Save Lead ]    │
├────────────────────────────────────────────────────────────────────────────┤
│                                                                            │
│ 🔹 BASIC INFORMATION                                                       │
│ Full Name *        [________________________]   Phone Number * [+91 ______] │
│ Email              [________________________]   Company Name   [__________] │
│                                                                            │
│ 🔹 LEAD QUALIFICATION                                                      │
│ Lead Type *        [ Select ▼ ]          Source *        [ Select ▼ ]       │
│ Lead Status *      [ Select ▼ ]                                           │
│                                                                            │
│ 🔹 REQUIREMENT DETAILS                                                     │
│ Customer Need      [ Select ▼ ]          Urgency        [ Select ▼ ]        │
│ Requirement Desc   [______________________________________________]        │
│                    [______________________________________________]        │
│                                                                            │
│ 🔹 DEAL & VALUE (Auto show if needed)                                      │
│ Product/Service   [ Select ▼ ]        Estimated Value ₹ [__________]        │
│ Quantity          [____]               Expected Close Date [ 📅 ]            │
│                                                                            │
│ 🔹 FOLLOW-UP & ASSIGNMENT                                                  │
│ Next Follow-Up *  [ 📅 Select Date ]   Assigned To   [ Select ▼ ]           │
│ Preferred Time    [ Select ▼ ]                                           │
│                                                                            │
│ 🔹 NOTES & INTERNAL                                                        │
│ Notes             [______________________________________________]         │
│                   [______________________________________________]         │
│ Loss Reason       [ Select ▼ ]  (Only if Status = Lost)                     │
│                                                                            │
├────────────────────────────────────────────────────────────────────────────┤
│ [ Cancel ]                                             [ Save Lead ]       │
└────────────────────────────────────────────────────────────────────────────┘
```

## dropdown data master

| Dropdown Name         | Values                                                                                                                 |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Lead Type**         | Product, Service, Inquiry, Support, Partnership                                                                        |
| **Source**            | Website, Facebook Ads, Instagram Ads, Google Ads, WhatsApp, Referral, Walk-in, Cold Call, Email Campaign, Manual Entry |
| **Lead Status**       | New, Contacted, Qualified, Proposal Sent, Negotiation, Won, Lost                                                       |
| **Customer Need**     | Pest Control, Home Cleaning, Office Cleaning, Deep Cleaning, Termite Treatment, General Inquiry, Other                 |
| **Urgency**           | Immediate (Within 24 hrs), High (2–3 days), Medium (Within a week), Low (Just exploring)                               |
| **Product / Service** | Indoor Pest Control Spray, Outdoor Pest Control, Termite Treatment, Annual Maintenance Package, Custom Service         |
| **Assigned To**       | Suraj, Amit, Priya, Rahul, Sales Team 1 *(Dynamic from users table)*                                                   |
| **Preferred Time**    | Morning (9 AM – 12 PM), Afternoon (12 PM – 4 PM), Evening (4 PM – 8 PM), Anytime                                       |
| **Loss Reason**       | Price Too High, Not Interested, Competitor Chosen, No Response, Budget Issue, Timing Issue, Duplicate Lead, Other      |


## Lead table 
```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│  ← Back to Dashboard                                                                    │
│                                                                                         │
│  LEADS MANAGEMENT                                                    [+ Add New Lead]   │
│  Manage and track all your sales leads in one place                                     │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                         │
│  ┌─────────────────────────────────────────────────────────────────────────────────┐    │
│  │                                                                                 │    │
│  │   🔍 Search by Name, Phone, Email, Company, Lead ID...                     [🔍] │    │
│  │                                                                                 │    │
│  └─────────────────────────────────────────────────────────────────────────────────┘    │
│                                                                                         │
│  ┌─────────────────────────────────────────────────────────────────────────────────┐    │
│  │   FILTER BY:                                                                     │    │
│  │                                                                                  │    │
│  │   Lead Type: [All Types                              ▼]                          │    │
│  │   ├─ Product                                                                     │    │
│  │   ├─ Service                                                                     │    │
│  │   ├─ Inquiry                                                                     │    │
│  │   ├─ Support                                                                     │    │
│  │   └─ Partnership                                                                 │    │
│  │                                                                                  │    │
│  │   Status:    [All Status                            ▼]                           │    │
│  │   ├─ New                                                                         │    │
│  │   ├─ Contacted                                                                   │    │
│  │   ├─ Qualified                                                                   │    │
│  │   ├─ Proposal Sent                                                               │    │
│  │   ├─ Negotiation                                                                 │    │
│  │   ├─ Won                                                                         │    │
│  │   └─ Lost                                                                        │    │
│  │                                                                                  │    │
│  │   Source:    [All Sources                           ▼]                           │    │
│  │   ├─ Website                                                                     │    │
│  │   ├─ Facebook Ads                                                                │    │
│  │   ├─ Instagram Ads                                                               │    │
│  │   ├─ Google Ads                                                                  │    │
│  │   ├─ WhatsApp                                                                    │    │
│  │   ├─ Referral                                                                    │    │
│  │   ├─ Walk-in                                                                     │    │
│  │   ├─ Cold Call                                                                   │    │
│  │   ├─ Email Campaign                                                              │    │
│  │   └─ Manual Entry                                                                │    │
│  │                                                                                  │    │
│  │   Urgency:   [All Urgency Levels                    ▼]                           │    │
│  │   ├─ Immediate (Within 24 hrs)                                                   │    │
│  │   ├─ High (2–3 days)                                                             │    │
│  │   ├─ Medium (Within a week)                                                      │    │
│  │   └─ Low (Just exploring)                                                        │    │
│  │                                                                                  │    │
│  │   Assigned:  [All Team Members                      ▼]                           │    │
│  │   ├─ Suraj                                                                       │    │
│  │   ├─ Amit                                                                        │    │
│  │   ├─ Priya                                                                       │    │
│  │   ├─ Rahul                                                                       │    │
│  │   └─ Sales Team 1                                                                │    │
│  │                                                                                  │    │
│  │   Customer Need: [All Needs                         ▼]                           │    │
│  │   ├─ Pest Control                                                                │    │
│  │   ├─ Home Cleaning                                                               │    │
│  │   ├─ Office Cleaning                                                             │    │
│  │   ├─ Deep Cleaning                                                               │    │
│  │   ├─ Termite Treatment                                                           │    │
│  │   ├─ General Inquiry                                                             │    │
│  │   └─ Other                                                                       │    │
│  │                                                                                  │    │
│  │   Follow-Up: [All Dates                             ▼]   [Apply]  [Reset All]    │    │
│  │   ├─ Today                                                                       │    │
│  │   ├─ Tomorrow                                                                    │    │
│  │   ├─ This Week                                                                   │    │
│  │   ├─ Overdue                                                                     │    │
│  │   └─ Custom Range                                                                │    │
│  │                                                                                  │    │
│  └─────────────────────────────────────────────────────────────────────────────────┘    │
│                                                                                         │
│  ┌─────────────────────────────────────────────────────────────────────────────────┐    │
│  │   Active Filters:  [Lead Type: Product ✕]  [Status: New ✕]        [Clear All]   │    │
│  └─────────────────────────────────────────────────────────────────────────────────┘    │
│                                                                                         │
│  ┌─────────────────────────────────────────────────────────────────────────────────┐    │
│  │                                                                                  │    │
│  │   Total Leads: 156    Hot Leads: 42    Warm Leads: 89    Cold Leads: 25         │    │
│  │                                                                                  │    │
│  │   ┌────────┬────────────────┬────────────────┬─────────────┬─────────────┬─────────────┬─────────────┬─────────────┬─────────────┬─────────────┬────────┐ │
│  │   │ Lead ID│ Full Name      │ Phone Number   │ Email       │ Company     │ Lead Type   │ Source      │ Status      │ Next Follow │ Value  │ Actions│ │
│  │   ├────────┼────────────────┼────────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼────────┼────────┤ │
│  │   │LD-24001│ Rahul Shah     │ 9876543210     │rahul@gmail.c│ABC Corp     │ Product     │ Website     │ New         │ Today 2 PM  │₹45,000 │ View   │ │
│  │   ├────────┼────────────────┼────────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼────────┼────────┤ │
│  │   │LD-24002│ Priya Mehta    │ 9123456789     │priya@xyz.com│XYZ Pvt Ltd  │ Service     │ Facebook Ads│ Contacted   │ Tomorrow    │₹1,20,00│ Edit   │ │
│  │   ├────────┼────────────────┼────────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼────────┼────────┤ │
│  │   │LD-24003│ Vikram Rao     │ 9988776655     │vikram@abc.c │ABC Solutions│ Inquiry     │ Referral    │ Qualified   │ 18/03/2024  │₹75,000 │ Delete │ │
│  │   ├────────┼────────────────┼────────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼────────┼────────┤ │
│  │   │LD-24004│ Sunita Sharma  │ 9876512345     │sunita@def.c │DEF Infotech │ Partnership │ Google Ads  │ Proposal    │ 20/03/2024  │₹5,00,00│        │ │
│  │   ├────────┼────────────────┼────────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼────────┼────────┤ │
│  │   │LD-24005│ Amit Patel     │ 9998887776     │amit@ghi.com │GHI Services │ Support     │ WhatsApp    │ Negotiation │ 22/03/2024  │₹2,50,00│        │ │
│  │   ├────────┼────────────────┼────────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼────────┼────────┤ │
│  │   │LD-24006│ Neha Gupta     │ 9876541230     │neha@jkl.com │JKL Products │ Product     │ Walk-in     │ Won         │ Closed      │₹3,00,00│        │ │
│  │   ├────────┼────────────────┼────────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼────────┼────────┤ │
│  │   │LD-24007│ Rajesh Kumar   │ 9123459876     │rajesh@mno.c │MNO Company  │ Service     │ Cold Call   │ Lost        │ -           │₹0      │        │ │
│  │   ├────────┼────────────────┼────────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼────────┼────────┤ │
│  │   │LD-24008│ Anjali Desai   │ 9987654321     │anjali@pqr.c │PQR Limited  │ Inquiry     │ Email Campai│ New         │ Today 5 PM  │₹15,000 │        │ │
│  │   ├────────┼────────────────┼────────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼────────┼────────┤ │
│  │   │LD-24009│ Suresh Yadav   │ 9876123456     │suresh@stu.c │STU Group    │ Product     │ Manual Entry│ Contacted   │ 19/03/2024  │₹85,000 │        │ │
│  │   ├────────┼────────────────┼────────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼─────────────┼────────┼────────┤ │
│  │   │LD-24010│ Pooja Verma    │ 9997775553     │pooja@vwx.com│VWX Solutions│ Service     │ Instagram Ad│ Qualified   │ 21/03/2024  │₹55,000 │        │ │
│  │   └────────┴────────────────┴────────────────┴─────────────┴─────────────┴─────────────┴─────────────┴─────────────┴─────────────┴────────┴────────┘ │
│  │                                                                                  │    │
│  │   Showing 1-10 of 156 leads                                                      │    │
│  │                                                                                  │    │
│  │   [Rows per page: 10 ▼]   [< Previous]  [1]  [2]  [3]  [4]  [5]  ...  [16]  [Next >] │
│  │                                                                                  │    │
│  └─────────────────────────────────────────────────────────────────────────────────┘    │
│                                                                                         │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

### 1. TABLE COLUMNS SPECIFICATION
| Column ID       | Column Name    | Field Source      | Data Type | Width | Alignment | Format Example    | Sortable |
| --------------- | -------------- | ----------------- | --------- | ----- | --------- | ----------------- | -------- |
| `col_lead_id`   | Lead ID        | `lead_id`         | String    | 80px  | Left      | `LD-24001`        | Yes      |
| `col_full_name` | Full Name      | `full_name`       | String    | 140px | Left      | `Rahul Shah`      | Yes      |
| `col_phone`     | Phone Number   | `phone_number`    | String    | 120px | Left      | `9876543210`      | Yes      |
| `col_email`     | Email          | `email`           | String    | 140px | Left      | `rahul@gmail.com` | Yes      |
| `col_company`   | Company Name   | `company_name`    | String    | 140px | Left      | `ABC Corp`        | Yes      |
| `col_lead_type` | Lead Type      | `lead_type`       | Badge     | 100px | Center    | `Product`         | Yes      |
| `col_source`    | Source         | `source`          | String    | 130px | Left      | `Website`         | Yes      |
| `col_status`    | Status         | `lead_status`     | Badge     | 120px | Center    | `New`             | Yes      |
| `col_follow_up` | Next Follow-Up | `next_follow_up`  | DateTime  | 110px | Center    | `Today 2 PM`      | Yes      |
| `col_value`     | Value          | `estimated_value` | Currency  | 100px | Right     | `₹45,000`         | Yes      |
| `col_actions`   | Actions        | -                 | Buttons   | 100px | Center    | View Edit Delete  | No       |


### 2. FILTER DROPDOWN VALUES (Complete List)

| Filter Name        | Field ID               | All Options                                                                                                                         |
| ------------------ | ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| **Lead Type**      | `filter_lead_type`     | All Types, Product, Service, Inquiry, Support, Partnership                                                                          |
| **Lead Status**    | `filter_lead_status`   | All Status, New, Contacted, Qualified, Proposal Sent, Negotiation, Won, Lost                                                        |
| **Source**         | `filter_source`        | All Sources, Website, Facebook Ads, Instagram Ads, Google Ads, WhatsApp, Referral, Walk-in, Cold Call, Email Campaign, Manual Entry |
| **Urgency**        | `filter_urgency`       | All Urgency, Immediate (Within 24 hrs), High (2–3 days), Medium (Within a week), Low (Just exploring)                               |
| **Assigned To**    | `filter_assigned_to`   | All Team Members, Suraj, Amit, Priya, Rahul, Sales Team 1                                                                           |
| **Customer Need**  | `filter_customer_need` | All Needs, Pest Control, Home Cleaning, Office Cleaning, Deep Cleaning, Termite Treatment, General Inquiry, Other                   |
| **Follow-Up Date** | `filter_follow_up`     | All Dates, Today, Tomorrow, This Week, Overdue, Custom Range                                                                        |
