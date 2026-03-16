# 🎯 MODULE 13: Vendor Management

## 13.1 Vendor Dashboard Table View



## Vendor Management Screen

  ### Screen Layout

```md id="z9lq1p"
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                     │
│  ┌──────────────────────────────────── FILTERS ──────────────────────────────────┐ │
│  │                                                                               │ │
│  │  Vendor Type:      [☑] Supplier   [☑] Service Provider   [☑] Both             │ │
│  │                                                                               │ │
│  │  Vendor Category:  [All Categories ▼]                                         │ │
│  │                                                                               │ │
│  │  Contract Type:    [☑] Annual   [☑] Project   [☑] One Time                    │ │
│  │                                                                               │ │
│  │  Billing Type:     [All ▼]                                                    │ │
│  │                                                                               │ │
│  │  Payment Terms:    [All ▼]                                                    │ │
│  │                                                                               │ │
│  │  Vendor Status:    [☑] Active   [☑] Inactive   [☑] Blocked                    │ │
│  │                                                                               │ │
│  │  Vendor Rating:    [⭐ 1+]   [⭐⭐ 2+]   [⭐⭐⭐ 3+]   [⭐⭐⭐⭐ 4+]   [⭐⭐⭐⭐⭐ 5]        │ │
│  │                                                                               │ │
│  │  Created Date:     [📅 From] - [📅 To]                                        │ │
│  │                                                                               │ │
│  │  ─────────────────────────────────────────────────────────────────────────   │ │
│  │                                                                               │ │
│  │  Search: [________________________________]                                  │ │
│  │          (ID / Name / Contact / Product)                                     │ │
│  │                                                                               │ │
│  │  [Reset Filters]                                      [+ ADD VENDOR]         │ │
│  └───────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│                                VENDOR OVERVIEW TABLE                                │
│                                                                                     │
│  ┌───────────────────────────────────────────────────────────────────────────────┐ │
│  │ Vdr ID │ Vendor Name       │ Type    │ Category  │ Contact Person │ Phone     │ │
│  ├────────┼───────────────────┼─────────┼───────────┼────────────────┼───────────┤ │
│  │ V-001  │ ABC Chemicals     │ Supp.   │ Chemical  │ Rajesh Kumar   │ 98765...  │ │
│  │ V-002  │ PestPro Services  │ Svc.    │ Fumigat.  │ Priya Sharma   │ 87654...  │ │
│  │ V-003  │ Global Equip      │ Both    │ Machine   │ Amit Singh     │ 76543...  │ │
│  └───────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌───────────────────────────────────────────────────────────────────────────────┐ │
│  │ Contract │ Billing   │ Payment │ Status │ Rating │ Actions                    │ │
│  ├──────────┼───────────┼─────────┼────────┼────────┼────────────────────────────┤ │
│  │ Annual   │ Per Svc   │ Net 30  │  🟢    │ ⭐⭐⭐⭐ │ [View] [Edit] [Delete]      │ │
│  │ Project  │ Monthly   │ Adv.    │  🟢    │ ⭐⭐⭐  │ [View] [Edit] [Delete]      │ │
│  │ One Time │ Project   │ Net 15  │  🔴    │ ⭐⭐   │ [View] [Edit] [Delete]      │ │
│  └───────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

#### Table View Fields
| Field                    | Type         | Required | Description                                 |
| ------------------------ | ------------ | -------- | ------------------------------------------- |
| Vendor ID                | Text         | Auto     | Unique auto-generated ID (V-001, V-002...)  |
| Vendor Name              | Text         | Yes      | Company/Organization full name              |
| Vendor Type              | Badge        | Yes      | Supplier 🟦 / Service Provider 🟪 / Both 🟧 |
| Vendor Category          | Text         | Yes      | Chemical / Equipment / Service / Other      |
| Service/Product Provided | Text         | Yes      | Primary service or product offering         |
| Contact Person           | Text         | Yes      | Primary contact name                        |
| Phone Number             | Link         | Yes      | Clickable to call, masked display           |
| Email ID                 | Link         | Yes      | Clickable mailto link                       |
| City                     | Text         | Yes      | Location city                               |
| State                    | Text         | Yes      | Location state                              |
| Contract Type            | Badge        | Yes      | Annual 🟢 / Project 🟡 / One Time 🔵        |
| Contract Start Date      | Date         | Yes      | Agreement start date (DD MMM YYYY)          |
| Contract End Date        | Date         | No       | Agreement end date (DD MMM YYYY)            |
| Billing Type             | Text         | Yes      | Per Service / Monthly / Project             |
| Payment Terms            | Text         | Yes      | Advance / Net 15 / Net 30 / Net 45          |
| Vendor Status            | Badge        | Yes      | Active 🟢 / Inactive 🟡 / Blocked 🔴        |
| Vendor Rating            | Star Rating  | No       | ⭐ 1-5 rating display                        |
| GST Number               | Link         | No       | Clickable GST verification link             |
| Created Date             | Date         | Auto     | System generated timestamp                  |
| Created By               | Text         | Auto     | Admin who created the vendor                |
| Actions                  | Button Group | —        | View / Edit / Delete                        |


#### Search Bar

| Search Field | Type       | Search Scope                                                                                         |
| ------------ | ---------- | ---------------------------------------------------------------------------------------------------- |
| Search       | Text Input | Vendor ID, Vendor Name, Contact Person, Phone Number, Email ID, GST Number, Service/Product Provided |


#### Filter Fields Layout
| Filter Name           | Type                   | Display Format                                                                 |
| --------------------- | ---------------------- | ------------------------------------------------------------------------------ |
| **Vendor Type**       | Multi-Select Checkbox  | `[☑] Supplier` `[☑] Service Provider` `[☑] Both`                               |
| **Vendor Category**   | Multi-Select Dropdown  | `[All Categories ▼]` `[Chemical]` `[Equipment]` `[Service]` `[Other]`          |
| **Contract Type**     | Multi-Select Checkbox  | `[☑] Annual` `[☑] Project` `[☑] One Time`                                      |
| **Billing Type**      | Single-Select Dropdown | `[All ▼]` `[Per Service]` `[Monthly]` `[Project]`                              |
| **Payment Terms**     | Single-Select Dropdown | `[All ▼]` `[Advance]` `[Net 15]` `[Net 30]` `[Net 45]`                         |
| **Vendor Status**     | Multi-Select Checkbox  | `[☑] Active 🟢` `[☑] Inactive 🟡` `[☑] Blocked 🔴`                             |
| **Vendor Rating**     | Rating Selector        | `[⭐ 1+]` `[⭐⭐ 2+]` `[⭐⭐⭐ 3+]` `[⭐⭐⭐⭐ 4+]` `[⭐⭐⭐⭐⭐ 5]` `[Any]`                  |
| **State**             | Multi-Select Dropdown  | `[All States ▼]` `[Maharashtra]` `[Karnataka]` `[Delhi]` `[Gujarat]` `[Other]` |
| **City**              | Text Search            | `[________________]`                                                           |
| **GST Available**     | Toggle Group           | `[All]` `[Yes]` `[No]`                                                         |
| **Contract End Date** | Date Range             | `[📅 From: __________]` `[📅 To: __________]`                                  |
| **Created Date**      | Date Range             | `[📅 From: __________]` `[📅 To: __________]`                                  |



## 13.2 Add Vendor


## Add Vendor Screen

  ### Screen Layout

```md
┌──────────────────────────────────────────────────────────────────────────────┐
│                                ADD NEW VENDOR                                │
└──────────────────────────────────────────────────────────────────────────────┘


## Basic Information

Vendor ID* (Auto Generated)        Vendor Status*        [ Active ▼ ]

Vendor Name*                       [________________________________________]

Vendor Type*                       [ Supplier / Service Provider / Both ▼ ]

Vendor Category*                   [________________________________________]

Service / Product Provided*        [________________________________________]

Contact Person*                    [________________________________________]

Phone Number*                      [________________________________________]

Email ID*                          [________________________________________]


## Address Details

Address*                           [________________________________________]
                                   [________________________________________]

City*            [____________]      State*        [____________ ▼]

Pincode*         [____________]      Country*      [ India ▼ ]


## Tax & Compliance

GST Number                         [________________________________________]

PAN Number                         [________________________________________]

Vendor Registration Type           [ Registered / Unregistered ▼ ]


## Bank Details

Bank Name                          [________________________________________]

Account Holder Name                [________________________________________]

Account Number                     [________________________________________]

IFSC Code                          [________________________________________]


## Contract Details

Contract Type*                     [ Annual / Project / One Time ▼ ]

Contract Start Date*               [ 📅 Select Date ]

Contract End Date                  [ 📅 Select Date ]

SLA Agreement                      [ Yes / No ▼ ]

Contract Document Upload           [ Upload File ]


## Billing Terms

Billing Type*                      [ Per Service / Monthly / Project ▼ ]

Billing Cycle                      [ Weekly / Monthly / Quarterly ▼ ]

Tax Applicable*                    [ Yes / No ▼ ]

Invoice Submission Method          [ Email / Portal / Physical ▼ ]


## Payment Details

Payment Terms*                     [ Advance / Net 15 / Net 30 / Net 45 ▼ ]

Credit Days                        [________]

Payment Method*                    [ Bank Transfer / UPI / Cheque ▼ ]

Currency*                          [ INR ▼ ]


## Performance & Notes

Vendor Rating                      [ 1 – 5 ]

Remarks / Notes                    [________________________________________]
                                   [________________________________________]


## System Information

Created Date (Auto)                Updated Date (Auto)

Created By (Auto)


┌──────────────────────────────────────────────────────────┐
│  [ Cancel ]                                   [ Save Vendor ] │
└──────────────────────────────────────────────────────────┘
```

## Vendor Data Fields Specification

| Field                          | Type                  | Required | Description                                                                     |
| ------------------------------ | --------------------- | -------- | ------------------------------------------------------------------------------- |
| **Vendor ID**                  | Text / Auto Generated | Auto     | Unique identifier generated by the system for each vendor                       |
| **Vendor Status**              | Dropdown              | Yes      | Indicates vendor availability (Active / Inactive / Blocked)                     |
| **Vendor Name**                | Text                  | Yes      | Official name of the vendor company or supplier                                 |
| **Vendor Type**                | Dropdown              | Yes      | Defines vendor role (Supplier / Service Provider / Both)                        |
| **Vendor Category**            | Dropdown              | Yes      | Category of vendor such as Chemical Supplier, Equipment Vendor, Service Partner |
| **Service / Product Provided** | Text / Dropdown       | Yes      | Type of service or product supplied by the vendor                               |
| **Contact Person**             | Text                  | Yes      | Primary contact person from vendor side                                         |
| **Phone Number**               | Phone                 | Yes      | Contact number of the vendor representative                                     |
| **Email ID**                   | Email                 | Yes      | Official communication email address                                            |
| **Address**                    | Textarea              | Yes      | Full business address of the vendor                                             |
| **City**                       | Text                  | Yes      | City where the vendor business is located                                       |
| **State**                      | Dropdown              | Yes      | State where the vendor operates                                                 |
| **Pincode**                    | Number                | Yes      | Postal code for vendor location                                                 |
| **Country**                    | Dropdown              | Yes      | Country of the vendor                                                           |
| **GST Number**                 | Text                  | No       | Vendor’s GST registration number used for tax compliance                        |
| **PAN Number**                 | Text                  | No       | Vendor PAN card number used for financial and legal verification                |
| **Vendor Registration Type**   | Dropdown              | No       | Indicates if vendor is Registered or Unregistered under tax regulations         |
| **Bank Name**                  | Text                  | No       | Name of vendor’s bank used for payment processing                               |
| **Account Holder Name**        | Text                  | No       | Name of the person or company holding the bank account                          |
| **Account Number**             | Number                | No       | Vendor bank account number for payment transfer                                 |
| **IFSC Code**                  | Text                  | No       | Bank IFSC code used for electronic payments                                     |
| **Contract Type**              | Dropdown              | Yes      | Type of agreement with vendor (Annual / Project / One Time)                     |
| **Contract Start Date**        | Date Picker           | Yes      | Date from which vendor contract becomes effective                               |
| **Contract End Date**          | Date Picker           | No       | Date when the vendor contract expires                                           |
| **SLA Agreement**              | Dropdown              | No       | Indicates whether a Service Level Agreement exists (Yes / No)                   |
| **Contract Document Upload**   | File Upload           | No       | Upload vendor contract or agreement document                                    |
| **Billing Type**               | Dropdown              | Yes      | Billing model used with vendor (Per Service / Monthly / Project)                |
| **Billing Cycle**              | Dropdown              | No       | Frequency of billing such as Weekly, Monthly, or Quarterly                      |
| **Tax Applicable**             | Dropdown              | Yes      | Specifies whether tax applies to vendor invoices                                |
| **Invoice Submission Method**  | Dropdown              | No       | How vendor submits invoices (Email / Portal / Physical)                         |
| **Payment Terms**              | Dropdown              | Yes      | Payment schedule such as Advance, Net 15, Net 30, Net 45                        |
| **Credit Days**                | Number                | No       | Number of days allowed before payment must be made                              |
| **Payment Method**             | Dropdown              | Yes      | Preferred payment mode (Bank Transfer / UPI / Cheque)                           |
| **Currency**                   | Dropdown              | Yes      | Currency used for vendor payments (e.g., INR)                                   |
| **Vendor Rating**              | Number                | No       | Performance rating assigned to vendor based on service quality                  |
| **Remarks / Notes**            | Textarea              | No       | Additional notes or internal comments about the vendor                          |
| **Created Date**               | DateTime              | Auto     | Timestamp when the vendor record was created                                    |
| **Updated Date**               | DateTime              | Auto     | Timestamp when the vendor record was last updated                               |
| **Created By**                 | User Reference        | Auto     | System user who created the vendor record                                       |


## 13.3 View vendor details



## View vendor data

  ### Screen Layout
```md
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                     │
│  [← Back to Vendors]                    VIEW VENDOR DETAILS                 [Edit] │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │  VENDOR HEADER                                                               │   │
│  │                                                                              │   │
│  │  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │  │  Vendor Name:     ABC CHEMICALS PRIVATE LIMITED                         │ │
│  │  │  Vendor ID:       V-001                                                  │ │
│  │  │  Status:          🟢 ACTIVE                                              │ │
│  │  │  Type:            🏭 Supplier                                            │ │
│  │  │  Rating:          ⭐⭐⭐⭐☆ (4/5)                                          │ │
│  │  └─────────────────────────────────────────────────────────────────────────┘ │
│  │                                                                              │
│  └─────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │  BASIC INFORMATION                                                           │   │
│  │                                                                              │   │
│  │  ┌────────────────────────────┬─────────────────────────────────────────┐   │
│  │  │  Vendor Type:              │  🏭 Supplier                            │   │
│  │  ├────────────────────────────┼─────────────────────────────────────────┤   │
│  │  │  Vendor Category:          │  Chemical                               │   │
│  │  ├────────────────────────────┼─────────────────────────────────────────┤   │
│  │  │  Service / Product:        │  Industrial Pest Control Chemicals      │   │
│  │  ├────────────────────────────┼─────────────────────────────────────────┤   │
│  │  │  Contact Person:           │  Rajesh Kumar Sharma                    │   │
│  │  ├────────────────────────────┼─────────────────────────────────────────┤   │
│  │  │  Phone Number:             │  📞 +91 9876543210                      │   │
│  │  ├────────────────────────────┼─────────────────────────────────────────┤   │
│  │  │  Email ID:                 │  ✉️ rajesh@abcchemicals.com             │   │
│  │  └────────────────────────────┴─────────────────────────────────────────┘   │
│  │                                                                              │
│  └─────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │  ADDRESS DETAILS                                                             │   │
│  │                                                                              │   │
│  │  ┌────────────────────────────┬─────────────────────────────────────────┐   │
│  │  │  Address:                  │  123, Industrial Estate, Phase 2        │   │
│  │  │                            │  Near Highway Road                      │   │
│  │  ├────────────────────────────┼─────────────────────────────────────────┤   │
│  │  │  City:                     │  Mumbai                                 │   │
│  │  ├────────────────────────────┼─────────────────────────────────────────┤   │
│  │  │  State:                    │  Maharashtra                            │   │
│  │  ├────────────────────────────┼─────────────────────────────────────────┤   │
│  │  │  Pincode:                  │  400001                                 │   │
│  │  ├────────────────────────────┼─────────────────────────────────────────┤   │
│  │  │  Country:                  │  🇮🇳 India                               │   │
│  │  └────────────────────────────┴─────────────────────────────────────────┘   │
│  │                                                                              │
│  └─────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │  TAX & COMPLIANCE                                                            │   │
│  │                                                                              │   │
│  │  ┌────────────────────────────┬─────────────────────────────────────────┐   │
│  │  │  GST Number:               │  27AAPFU0939F1ZV                        │   │
│  │  ├────────────────────────────┼─────────────────────────────────────────┤   │
│  │  │  PAN Number:               │  AAPFU0939F                             │   │
│  │  ├────────────────────────────┼─────────────────────────────────────────┤   │
│  │  │  Registration Type:        │  Registered                             │   │
│  │  └────────────────────────────┴─────────────────────────────────────────┘   │
│  │                                                                              │
│  └─────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │  BANK DETAILS                                                                │   │
│  │                                                                              │   │
│  │  ┌────────────────────────────┬─────────────────────────────────────────┐   │
│  │  │  Bank Name:                │  State Bank of India                    │   │
│  │  ├────────────────────────────┼─────────────────────────────────────────┤   │
│  │  │  Account Holder:           │  ABC Chemicals Pvt Ltd                  │   │
│  │  ├────────────────────────────┼─────────────────────────────────────────┤   │
│  │  │  Account Number:           │  **** **** **** 4521                    │   │
│  │  ├────────────────────────────┼─────────────────────────────────────────┤   │
│  │  │  IFSC Code:                │  SBIN0001234                            │   │
│  │  └────────────────────────────┴─────────────────────────────────────────┘   │
│  │                                                                              │
│  └─────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │  CONTRACT DETAILS                                                            │   │
│  │                                                                              │   │
│  │  │  Contract Type:   📅 Annual                                              │
│  │  │  Start Date:      01 Jan 2026                                            │
│  │  │  End Date:        31 Dec 2026                                            │
│  │  │  SLA Agreement:   ✅ Yes                                                 │
│  │  │  Contract File:   [📄 annual_contract_2026.pdf] [View]                   │
│  │                                                                              │
│  └─────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │  [← Back]  [Print]  [Duplicate]  [Delete]                      [Edit Vendor]│
│  └─────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

### 📋 SECTION-BY-SECTION BREAKDOWN
#### 1. VENDOR HEADER
| Element          | Display Format                             |
| ---------------- | ------------------------------------------ |
| **Vendor Name**  | H1 heading, uppercase                      |
| **Vendor ID**    | Monospace font, gray color                 |
| **Status Badge** | 🟢 Active / 🟡 Inactive / 🔴 Blocked       |
| **Type Badge**   | 🏭 Supplier / 🔧 Service Provider / ⚡ Both |
| **Rating**       | ⭐⭐⭐⭐☆ (X out of 5)                         |


#### 2. BASIC INFORMATION
| Field               | Display Rule              |
| ------------------- | ------------------------- |
| **Vendor Type**     | Icon + Label              |
| **Vendor Category** | Text badge                |
| **Service/Product** | Full text, wrap if long   |
| **Contact Person**  | Bold text                 |
| **Phone Number**    | 📞 Clickable tel: link    |
| **Email ID**        | ✉️ Clickable mailto: link |


#### 3. ADDRESS DETAILS
| Field       | Display Format                  |
| ----------- | ------------------------------- |
| **Address** | Multi-line, preserve formatting |
| **City**    | Title case                      |
| **State**   | Full state name                 |
| **Pincode** | 6-digit number                  |
| **Country** | 🇮🇳 Flag + Full name           |


#### 4. TAX & COMPLIANCE
| Field                 | Display Format                          |
| --------------------- | --------------------------------------- |
| **GST Number**        | Clickable link to GST portal            |
| **PAN Number**        | Masked: AAPFU\*\*\*\*F (last 4 visible) |
| **Registration Type** | Registered / Unregistered badge         |


#### 5. BANK DETAILS
| Field              | Display Format                          |
| ------------------ | --------------------------------------- |
| **Bank Name**      | Title case                              |
| **Account Holder** | Full name as per bank                   |
| **Account Number** | Masked: \*\*\*\* \*\*\*\* \*\*\*\* XXXX |
| **IFSC Code**      | Uppercase, copy button                  |


#### 6. CONTRACT DETAILS
| Field                 | Display Format                      |
| --------------------- | ----------------------------------- |
| **Contract Type**     | 📅 Annual / 📋 Project / ⚡ One Time |
| **Start Date**        | DD MMM YYYY                         |
| **End Date**          | DD MMM YYYY (or "Not specified")    |
| **SLA Agreement**     | ✅ Yes / ❌ No                        |
| **Contract Document** | \[📄 filename] \[View] \[Download]  |


#### 7. BILLING TERMS

| Field              | Display Format                  |
| ------------------ | ------------------------------- |
| **Billing Type**   | Per Service / Monthly / Project |
| **Billing Cycle**  | Weekly / Monthly / Quarterly    |
| **Tax Applicable** | ✅ Yes (GST XX%) / ❌ No          |
| **Invoice Method** | Email / Portal / Physical       |


#### 8. PAYMENT DETAILS
| Field              | Display Format                     |
| ------------------ | ---------------------------------- |
| **Payment Terms**  | Advance / Net 15 / Net 30 / Net 45 |
| **Credit Days**    | X Days (or "Not applicable")       |
| **Payment Method** | Bank Transfer / UPI / Cheque       |
| **Currency**       | ₹ INR / \$ USD / € EUR             |


#### 9. PERFORMANCE & NOTES
| Field             | Display Format                    |
| ----------------- | --------------------------------- |
| **Vendor Rating** | ⭐⭐⭐⭐☆ (X out of 5) + Progress bar |
| **Remarks**       | Full text, preserve line breaks   |


#### 10. SYSTEM INFORMATION
| Field     | Format                   |
| --------- | ------------------------ |
| **Dates** | DD MMM YYYY, HH:MM AM/PM |
| **Users** | Name + Email tooltip     |


## 13.4 Edit vendor details



## Edit vendor data

  ### Screen Layout
```md
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                     │
│  [← Back]                     EDIT VENDOR                                   [Update]│
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│  ┌──────────────────────────── 🔴 REQUIRED FIELDS (MANDATORY) ───────────────────┐ │
│  │                                                                              │ │
│  │ ⚠️ [Same as Add Vendor Screen - See Below for Structure]                     │ │
│  │                                                                              │ │
│  └──────────────────────────────────────────────────────────────────────────────┘ │
 │
│                                                                                     │
│  ┌──────────────────────────── 🟣 AUDIT LOG ────────────────────────────────────┐ │
│  │                                                                              │ │
│  │  ┌────────────────────────────┬───────────────────────────────────────────┐ │ │
│  │  │ Edited By:                 │ [Auto-detect: Current User]               │ │ │
│  │  │                            │ Admin User (admin@pestcontrol.com)        │ │ │
│  │  ├────────────────────────────┼───────────────────────────────────────────┤ │ │
│  │  │ Edited Date:               │ [Auto-detect: Current Date & Time]        │ │ │
│  │  │                            │ 16 Mar 2026, 03:45 PM                     │ │ │
│  │  └────────────────────────────┴───────────────────────────────────────────┘ │ │
│  │                                                                              │ │
│  │  ⚠️ These fields will automatically update when "Update Service" is clicked │ │
│  │      and all the fields are filled automatically                             │ │
│  └──────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────────┐ │
│  │  [Cancel]      [Reset Changes]                           [Update vendor]   │ │
│  └──────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
└─────────────────────────────────────────────────────────────────────────────────────┘
```
#### 🎨 AUDIT LOG FIELD SPECIFICATIONS
| Field               | Behavior                                     | Display Format            |
| ------------------- | -------------------------------------------- | ------------------------- |
| **Edited By**       | Auto-populate from logged-in user session    | Name + Email + Role badge |
| **Edited Date**     | Auto-capture system datetime on Update click | Date + Time (24hr format) |
| **Change Tracking** | Compare old vs new values                    | Highlight modified fields |


## 12.4 Edit Service



## Delete Vendor Screen

  ### Screen Layout

```md
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│                          ⚠️ CONFIRMATION                         │
│                                                                 │
│                                                                 │
│           Are you sure you want to mark this vendor             │
│                        as inactive?                             │
│                                                                 │
│                                                                 │
│              [Cancel]              [Mark as Inactive]           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```
