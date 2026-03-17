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

