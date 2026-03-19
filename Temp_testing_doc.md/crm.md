# Quotation

## add quotation

```
┌────────────────────────────────────────────────────────────────────────────┐
│ ← Back              Create Quotation                    [ Save ] [ Send ]   │
├────────────────────────────────────────────────────────────────────────────┤

🔹 BASIC INFORMATION
──────────────────────────────────────────────────────────────────────────────
Quotation No *      [ Auto Generated ]     Quotation Date *   [ 📅 ]
Valid Till *        [ 📅 ]

Lead               [ 🔍 Search Lead ▼ ]

📌 LEAD DETAILS (Auto Fetched)
────────────────────────────────────────────────────────────
Lead Name        Rajesh Kumar
Company          ABC Pvt Ltd
Phone            +91 9876543210
Email            rajesh@email.com

Lead Type        Hot
Source           Website
Status           Qualified
_____________________________________________________________

Customer *         [ 🔍 Search Customer ▼ ]

📌 CUSTOMER DETAILS (Auto Fetched)
────────────────────────────────────────────────────────────
Customer Name     ABC Pvt Ltd
Contact Person    Rajesh Kumar
Phone             +91 9876543210
Email             rajesh@email.com
Address           Surat, Gujarat
GST No            24ABCDE1234F1Z5
_____________________________________________________________

Contact Person     [ Auto / Select ▼ ]
Phone Number       [ Auto Filled ]
Email              [ Auto Filled ]

──────────────────────────────────────────────────────────────────────────────

🔹 QUOTATION CONFIGURATION
──────────────────────────────────────────────────────────────────────────────
Quotation Type *   [ Product / Service ▼ ]
Source            [ Website / Referral / Direct ▼ ]

Status *          [ Draft / Sent / Accepted / Rejected ▼ ]
Send Via          [ WhatsApp / Email / Print ▼ ]

──────────────────────────────────────────────────────────────────────────────

🔹 ITEMS DETAILS
──────────────────────────────────────────────────────────────────────────────

[ + Add Item ]

┌────┬──────────────┬────────────┬────────┬────────┬────────┬────────┬────────┐
│ #  │ Item Name    │ Variant    │ Qty    │ Price  │ Tax %  │ Disc % │ Total  │
├────┼──────────────┼────────────┼────────┼────────┼────────┼────────┼────────┤
│ 1  │ 🔍 Select     │ Auto       │ [ 2 ]  │ ₹1000  │ 18%    │ [ 5 ]  │ ₹2360  │
└────┴──────────────┴────────────┴────────┴────────┴────────┴────────┴────────┘

📦 PRODUCT DETAILS (Auto Display on Selection)
────────────────────────────────────────────────────────────
Product Code    PRD001
Product Name    Chemical X
Category        Chemical
Company         ABC Agro
HSN Code        1234
Base UOM        Ltr
Package Type    Bottle
Status          Active

📦 VARIANTS (if available)
────────────────────────────────────────────────────────────
Variant Name │ SKU        │ Package │ Quantity │ Status
100 ml       │ PRD001-V1  │ Bottle  │ 0.1 Ltr  │ Active
250 ml       │ PRD001-V2  │ Bottle  │ 0.25 Ltr │ Active
1 Ltr        │ PRD001-V3  │ Bottle  │ 1 Ltr    │ Active

💰 PRICING DETAILS (Auto)
────────────────────────────────────────────────────────────
Base Price      ₹550
Selling Price   ₹650
Tax (%)         18% (Auto from HSN)

Quantity *          [____]
Estimated Value ₹   [ Auto = Qty × Selling Price ]

──────────────────────────────────────────────────────────────────────────────

🔹 PRICE SUMMARY
────────────────────────────────────────────────────────────
Sub Total           ₹ 5,000
Discount            [ ₹ / % ]   [____]
Tax Amount          ₹ 900
Shipping Charges    [____]
Adjustment          [ + / - ₹ ]

────────────────────────────────────────
Grand Total         ₹ 5,850

──────────────────────────────────────────────────────────────────────────────

🔹 TERMS & CONDITIONS
────────────────────────────────────────────────────────────
Payment Terms       [ Net 15 / Net 30 ▼ ]
Delivery Timeline   [__________]
Warranty            [__________]

Terms & Conditions
[__________________________________________________________]

──────────────────────────────────────────────────────────────────────────────

🔹 FOLLOW-UP & ASSIGNMENT
────────────────────────────────────────────────────────────
Next Follow-Up *   [ 📅 ]
Assigned To       [ Select ▼ ]
Preferred Time    [ Morning / Afternoon / Evening ▼ ]

──────────────────────────────────────────────────────────────────────────────

🔹 NOTES
────────────────────────────────────────────────────────────
Internal Notes
[__________________________________________________________]

Customer Notes (Visible in PDF)
[__________________________________________________________]

──────────────────────────────────────────────────────────────────────────────

🔹 ACTIONS
────────────────────────────────────────────────────────────
[ Cancel ]      [ Save Draft ]      [ Save & Send ]      [ Generate PDF ]

└────────────────────────────────────────────────────────────────────────────┘
```


## All Form Fields

| Section       | Field Name         | Data Type (DB) | Input Type (UI)                 | Description / Notes     |
| ------------- | ------------------ | -------------- | ------------------------------- | ----------------------- |
| Basic Info    | Quotation No       | VARCHAR(50)    | Auto Generated (Read-only Text) | Unique quotation number |
| Basic Info    | Quotation Date     | DATE           | Date Picker                     | Default = Today         |
| Basic Info    | Valid Till         | DATE           | Date Picker                     | Expiry date             |
| Basic Info    | Lead ID            | INT (FK)       | Searchable Dropdown             | लिंक to Leads table     |
| Basic Info    | Lead Name          | VARCHAR(100)   | Auto Display                    | From Lead (Panel)       |
| Basic Info    | Lead Company       | VARCHAR(150)   | Auto Display                    | From Lead               |
| Basic Info    | Lead Phone         | VARCHAR(15)    | Auto Display                    | From Lead               |
| Basic Info    | Lead Email         | VARCHAR(100)   | Auto Display                    | From Lead               |
| Basic Info    | Lead Type          | VARCHAR(50)    | Auto Display                    | Hot/Warm/Cold           |
| Basic Info    | Lead Source        | VARCHAR(50)    | Auto Display                    | From Lead               |
| Basic Info    | Lead Status        | VARCHAR(50)    | Auto Display                    | Qualified etc.          |
| Basic Info    | Customer ID        | INT (FK)       | Searchable Dropdown             | Required                |
| Basic Info    | Customer Name      | VARCHAR(150)   | Auto Display                    | From Customer           |
| Basic Info    | Contact Person     | VARCHAR(100)   | Dropdown / Text                 | Based on customer       |
| Basic Info    | Phone Number       | VARCHAR(15)    | Auto Fill Text                  | From Customer           |
| Basic Info    | Email              | VARCHAR(100)   | Auto Fill Text                  | From Customer           |
| Basic Info    | Address            | TEXT           | Auto Display                    | Customer details        |
| Basic Info    | GST No             | VARCHAR(20)    | Auto Display                    | Customer GST            |
| Config        | Quotation Type     | ENUM           | Dropdown                        | Product / Service       |
| Config        | Source             | VARCHAR(50)    | Dropdown                        | Lead source             |
| Config        | Status             | ENUM           | Dropdown                        | Draft, Sent etc.        |
| Config        | Send Via           | VARCHAR(50)    | Dropdown                        | Email, WhatsApp         |
| Items         | Product ID         | INT (FK)       | Searchable Dropdown             | Required                |
| Items         | Product Name       | VARCHAR(150)   | Auto Display                    | From Product            |
| Items         | Variant ID         | INT (FK)       | Dropdown                        | If available            |
| Items         | Variant Name       | VARCHAR(100)   | Auto Display                    |                         |
| Items         | Quantity           | INT / DECIMAL  | Number Input                    | Required                |
| Items         | Price              | DECIMAL(10,2)  | Auto Fill                       | Selling price           |
| Items         | Tax (%)            | DECIMAL(5,2)   | Auto Fill                       | From HSN                |
| Items         | Discount           | DECIMAL(10,2)  | Input (₹ / %)                   | Optional                |
| Items         | Total              | DECIMAL(10,2)  | Auto Calculated                 | Read-only               |
| Product Panel | Product Code       | VARCHAR(50)    | Auto Display                    |                         |
| Product Panel | Category           | VARCHAR(100)   | Auto Display                    |                         |
| Product Panel | Company            | VARCHAR(100)   | Auto Display                    |                         |
| Product Panel | HSN Code           | VARCHAR(20)    | Auto Display                    |                         |
| Product Panel | Base UOM           | VARCHAR(20)    | Auto Display                    |                         |
| Product Panel | Package Type       | VARCHAR(50)    | Auto Display                    |                         |
| Product Panel | Product Status     | VARCHAR(20)    | Auto Display                    | Active/Inactive         |
| Pricing       | Sub Total          | DECIMAL(10,2)  | Auto Calculated                 | Sum of items            |
| Pricing       | Discount (Overall) | DECIMAL(10,2)  | Input                           | ₹ / %                   |
| Pricing       | Tax Amount         | DECIMAL(10,2)  | Auto Calculated                 |                         |
| Pricing       | Shipping Charges   | DECIMAL(10,2)  | Input                           | Optional                |
| Pricing       | Adjustment         | DECIMAL(10,2)  | Input                           | + / -                   |
| Pricing       | Grand Total        | DECIMAL(10,2)  | Auto Calculated                 | Final amount            |
| Terms         | Payment Terms      | VARCHAR(50)    | Dropdown                        | Net 15 etc.             |
| Terms         | Delivery Timeline  | VARCHAR(100)   | Text Input                      |                         |
| Terms         | Warranty           | VARCHAR(100)   | Text Input                      |                         |
| Terms         | Terms & Conditions | TEXT           | Textarea                        |                         |
| Follow-up     | Next Follow-Up     | DATE           | Date Picker                     | Required                |
| Follow-up     | Assigned To        | INT (FK)       | Dropdown                        | Users table             |
| Follow-up     | Preferred Time     | VARCHAR(50)    | Dropdown                        | Morning etc.            |
| Notes         | Internal Notes     | TEXT           | Textarea                        | Internal use            |
| Notes         | Customer Notes     | TEXT           | Textarea                        | Visible in PDF          |
| System        | Created At         | DATETIME       | Auto                            | Timestamp               |
| System        | Updated At         | DATETIME       | Auto                            | Timestamp               |


## Validations 

| Section    | Field Name              | Required    | Validation Rules                   | Condition / Notes         |
| ---------- | ----------------------- | ----------- | ---------------------------------- | ------------------------- |
| Basic Info | Quotation No            | Yes (Auto)  | Unique, Auto-generated             | Read-only                 |
| Basic Info | Quotation Date          | Yes         | Valid date, ≤ Today                | Format: YYYY-MM-DD        |
| Basic Info | Valid Till              | Yes         | Must be > Quotation Date           |                           |
| Basic Info | Lead                    | No          | Must exist in DB                   | Optional                  |
| Basic Info | Lead Details Panel      | Auto        | Must fetch correct lead data       | Show if Lead selected     |
| Basic Info | Customer                | Yes         | Must exist in DB                   | Mandatory                 |
| Basic Info | Customer Details Panel  | Auto        | Must fetch correct customer data   | Show if Customer selected |
| Basic Info | Lead → Customer Mapping | Yes         | Customer must match Lead           | If Lead selected          |
| Basic Info | Contact Person          | No          | Must belong to selected Customer   | Auto-fill or manual       |
| Basic Info | Phone Number            | Yes (Auto)  | `^[6-9]\d{9}$`                     | 10 digit Indian number    |
| Basic Info | Email                   | No          | Valid email format                 | If filled                 |
| Config     | Quotation Type          | Yes         | Must be selected                   |                           |
| Config     | Source                  | No          | Must be valid option               |                           |
| Config     | Status                  | Yes         | Must be selected                   | Default = Draft           |
| Config     | Send Via                | Conditional | Required if Status = Sent          |                           |
| Items      | Items Count             | Yes         | At least 1 item required           | Cannot save empty         |
| Items      | Item Name               | Yes         | Must be selected from product list |                           |
| Items      | Variant                 | Conditional | Required if product has variants   |                           |
| Items      | Quantity                | Yes         | Numeric, > 0                       | Max optional              |
| Items      | Price                   | Yes (Auto)  | ≥ 0                                | Read-only                 |
| Items      | Tax %                   | Yes (Auto)  | 0–100                              | From HSN                  |
| Items      | Discount                | No          | 0–100 (%) OR ≥ 0 (₹)               |                           |
| Items      | Total                   | Yes (Auto)  | Auto-calculated                    | Read-only                 |
| Items      | Duplicate Item          | Yes         | Same product + variant not allowed | Prevent duplicates        |
| Product    | Product Status          | Yes         | Must be Active                     | Auto check                |
| Product    | Variant Status          | Yes         | Must be Active                     | Auto check                |
| Pricing    | Discount                | No          | ≤ Subtotal                         | If ₹                      |
| Pricing    | Shipping Charges        | No          | ≥ 0                                |                           |
| Pricing    | Adjustment              | No          | Can be + or -                      |                           |
| Pricing    | Grand Total             | Yes         | ≥ 0                                | Auto-calculated           |
| Terms      | Payment Terms           | No          | Must be valid option               |                           |
| Terms      | Delivery Timeline       | No          | Max 100 chars                      |                           |
| Terms      | Warranty                | No          | Max 100 chars                      |                           |
| Terms      | Terms & Conditions      | No          | Max 1000 chars                     |                           |
| Follow-up  | Next Follow-Up          | Yes         | Must be future date                |                           |
| Follow-up  | Assigned To             | No          | Must exist in users table          |                           |
| Follow-up  | Preferred Time          | No          | Must be valid option               |                           |
| Notes      | Internal Notes          | No          | Max 1000 chars                     |                           |
| Notes      | Customer Notes          | No          | Max 1000 chars                     |                           |
| System     | Expiry Check            | Yes         | Auto mark expired                  | If Valid Till < Today     |
| System     | Backend Calculation     | Yes         | Recalculate totals                 | Security purpose          |


## Dropdown values
| Field Name     | Dropdown Values                                                            |
| -------------- | -------------------------------------------------------------------------- |
| Quotation Type | Product, Service                                                           |
| Source         | Website, Referral, Walk-in, Call, Email, Social Media, Other               |
| Status         | Draft, Sent, Viewed, Accepted, Rejected, Expired, Converted to Invoice     |
| Send Via       | WhatsApp, Email, Print, Download PDF                                       |
| Payment Terms  | Immediate, Net 7, Net 15, Net 30, Net 45, Net 60, Custom                   |
| Preferred Time | Morning (9 AM–12 PM), Afternoon (12 PM–4 PM), Evening (4 PM–7 PM), Anytime |
| Assigned To    | Dynamic (Users Table)                                                      |
| Customer       | Dynamic (Customer Table - Searchable)                                      |
| Lead           | Dynamic (Leads Table - Searchable)                                         |
| Product        | Dynamic (Product Table - Searchable)                                       |
| Variant        | Dynamic (Product Variants Table)                                           |
| Contact Person | Dynamic (Based on Customer)                                                |



