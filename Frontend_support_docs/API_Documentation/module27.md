# Module 27: User Profile

## Table of Contents

- [27.1 User Profile – View Mode](#271-user-profile--view-mode)
- [27.2 User Profile – Edit Mode](#272-user-profile--edit-mode)
  - [Normal User (Except CEO Role)](#normal-user-except-ceo-role)
  - [CEO Role User](#ceo-role-user)

---

## 27.1 User Profile – View Mode

### Endpoint

```
GET {{baseUrl}}/api/v1/profile
```

### Description

Retrieves the current user's profile information including basic info, organization details, address, salary, and bank information.

---

## 27.2 User Profile – Edit Mode

### Normal User (Except CEO Role)

#### Endpoint

```
PUT {{baseUrl}}/api/v1/profile?employeeId={{employeeId}}
```

#### Query Parameters

| Parameter  | Type   | Example        | Description         |
| ---------- | ------ | -------------- | ------------------- |
| employeeId | string | {{employeeId}} | Employee identifier |

#### Request Body

```json
{
  "basicInfo": {
    "firstName": "Aryan",
    "lastName": "K",
    "email": "aryan@example.com",
    "contactNumber": "9876543210",
    "alternateNumber": null,
    "employmentType": "FULL_TIME",
    "profilePhotoUrl": null
  },
  "organizationInfo": {
    "department": "Operations",
    "designation": "Technician",
    "role": "APPLICATION_USER",
    "branch": "Bangalore",
    "reportingManagerId": null,
    "appUser": true
  },
  "addressInfo": {
    "current": {
      "line1": "Line 1",
      "line2": null,
      "city": "Bangalore",
      "state": "KA",
      "country": "IN",
      "pincode": "560001"
    },
    "permanent": {
      "line1": "Line 1",
      "line2": null,
      "city": "Bangalore",
      "state": "KA",
      "country": "IN",
      "pincode": "560001"
    },
    "sameAsCurrent": true
  },
  "salaryInfo": {
    "salaryType": "MONTHLY",
    "basicSalary": 25000,
    "hra": 5000,
    "otherAllowance": 2000,
    "incentive": 0,
    "deductions": 0,
    "pfApplicable": true,
    "esiApplicable": false,
    "tdsApplicable": false
  },
  "bankInfo": {
    "bankName": "HDFC",
    "accountNumber": "1234567890",
    "accountHolder": "Aryan K",
    "ifsc": "HDFC0000123",
    "upiId": "aryan@upi"
  }
}
```

---

### CEO Role User

#### Endpoint

```
PUT {{baseUrl}}/api/v1/profile/company?companyId={{companyId}}
```

#### Query Parameters

| Parameter | Type   | Example       | Description        |
| --------- | ------ | ------------- | ------------------ |
| companyId | string | {{companyId}} | Company identifier |

#### Request Body

```json
{
  "companyName": "RBAC Pvt Ltd",
  "tagline": "Security & Access Control",
  "website": "https://example.com",
  "foundingYear": 2020,
  "logoUrl": null,
  "industryType": "IT Services",
  "contactPersonName": "CEO Name",
  "contactPersonEmail": "ceo@example.com",
  "contactPersonPhone": "9876543210",
  "gst": "29ABCDE1234F1Z5",
  "pan": "ABCDE1234F",
  "licenseNumber": "LIC-001",
  "addressLine1": "Line 1",
  "addressLine2": null,
  "city": "Bangalore",
  "state": "KA",
  "pincode": "560001"
}
```

---

## Field Descriptions

### Basic Info Fields

| Field           | Type   | Description                               |
| --------------- | ------ | ----------------------------------------- |
| firstName       | string | Employee's first name                     |
| lastName        | string | Employee's last name                      |
| email           | string | Employee's email address                  |
| contactNumber   | string | Primary contact phone number              |
| alternateNumber | string | Secondary contact phone number (optional) |
| employmentType  | string | FULL_TIME, PART_TIME, CONTRACT, etc.      |
| profilePhotoUrl | string | URL to profile photo (optional)           |

### Organization Info Fields

| Field              | Type    | Description                                 |
| ------------------ | ------- | ------------------------------------------- |
| department         | string  | Department name (e.g., Operations, Sales)   |
| designation        | string  | Job title/designation                       |
| role               | string  | System role (APPLICATION_USER, ADMIN, etc.) |
| branch             | string  | Branch/office location                      |
| reportingManagerId | string  | Manager's user ID (optional)                |
| appUser            | boolean | Whether user has app access                 |

### Address Info Fields

#### Current Address

| Field   | Type   | Description               |
| ------- | ------ | ------------------------- |
| line1   | string | Address line 1            |
| line2   | string | Address line 2 (optional) |
| city    | string | City name                 |
| state   | string | State/province code       |
| country | string | Country code (e.g., IN)   |
| pincode | string | Postal/PIN code           |

#### Permanent Address

Same structure as Current Address

| Field         | Type    | Description                                |
| ------------- | ------- | ------------------------------------------ |
| sameAsCurrent | boolean | If true, permanent address same as current |

### Salary Info Fields

| Field          | Type    | Description                               |
| -------------- | ------- | ----------------------------------------- |
| salaryType     | string  | MONTHLY, DAILY, HOURLY                    |
| basicSalary    | number  | Basic salary amount                       |
| hra            | number  | House Rent Allowance                      |
| otherAllowance | number  | Other allowances                          |
| incentive      | number  | Performance incentive                     |
| deductions     | number  | Total deductions                          |
| pfApplicable   | boolean | Provident Fund applicable                 |
| esiApplicable  | boolean | ESI (Employee State Insurance) applicable |
| tdsApplicable  | boolean | TDS (Tax Deducted at Source) applicable   |

### Bank Info Fields

| Field         | Type   | Description                 |
| ------------- | ------ | --------------------------- |
| bankName      | string | Bank name                   |
| accountNumber | string | Bank account number         |
| accountHolder | string | Account holder name         |
| ifsc          | string | IFSC code                   |
| upiId         | string | UPI ID for digital payments |

### Company Info Fields (CEO Role)

| Field              | Type   | Description                       |
| ------------------ | ------ | --------------------------------- |
| companyName        | string | Company legal name                |
| tagline            | string | Company tagline/slogan            |
| website            | string | Company website URL               |
| foundingYear       | number | Year company was founded          |
| logoUrl            | string | Company logo URL (optional)       |
| industryType       | string | Industry/sector type              |
| contactPersonName  | string | Primary contact person name       |
| contactPersonEmail | string | Primary contact email             |
| contactPersonPhone | string | Primary contact phone             |
| gst                | string | GST registration number           |
| pan                | string | PAN number                        |
| licenseNumber      | string | Business license number           |
| addressLine1       | string | Company address line 1            |
| addressLine2       | string | Company address line 2 (optional) |
| city               | string | City name                         |
| state              | string | State/province code               |
| pincode            | string | Postal/PIN code                   |

---

## Employment Types Reference

| Type       | Description                  |
| ---------- | ---------------------------- |
| FULL_TIME  | Full-time permanent employee |
| PART_TIME  | Part-time employee           |
| CONTRACT   | Contract-based employment    |
| TEMPORARY  | Temporary employee           |
| INTERN     | Internship                   |
| CONSULTANT | Consultant/Freelancer        |

---

## Salary Types Reference

| Type    | Description          |
| ------- | -------------------- |
| MONTHLY | Monthly salary basis |
| DAILY   | Daily wage basis     |
| HOURLY  | Hourly rate basis    |
| WEEKLY  | Weekly payment basis |

---

## Role-Based Access

### Normal User Profile

- Can update own basic information
- Can update contact details
- Can update address information
- Can update bank details
- **Cannot update:** Organization info, salary info (typically restricted to HR/Admin)

### CEO Role Profile

- Has access to company profile settings
- Can update company information
- Can update company contact details
- Can update company registration details (GST, PAN)
- Can update company address

### Admin/HR Access

- Can update employee organization information
- Can update salary information
- Can update reporting structure
- Full access to all profile fields

---

## Validation Rules

### Email Validation

- Must be valid email format
- Unique across the organization
- Required field

### Phone Number Validation

- 10-digit format for Indian numbers
- Can include country code prefix
- Contact number is required

### Address Validation

- Line1, city, state, country, pincode are required
- Pincode format validated based on country

### Salary Validation

- All amounts must be non-negative
- Basic salary required for MONTHLY type
- At least one payment component required

### Bank Details Validation

- Account number format validated
- IFSC code format: XXXX0XXXXXX
- UPI ID format: username@provider

### Company Info Validation (CEO)

- GST format: 15 characters (e.g., 29ABCDE1234F1Z5)
- PAN format: 10 characters (e.g., ABCDE1234F)
- Website must be valid URL
- Founding year must be reasonable (e.g., 1900-current year)

---

## Use Cases

### Employee Self-Service

1. View own profile information
2. Update contact details
3. Update address (current/permanent)
4. Update bank details for salary transfer
5. Upload/update profile photo

### HR Management

1. Update employee organization details
2. Assign/change reporting manager
3. Update salary components
4. Update employment type
5. Grant/revoke app access

### Company Administration (CEO)

1. Update company branding (logo, tagline)
2. Update company contact information
3. Update registration details
4. Update company address

---

## Address Management

### Same as Current Address

When `sameAsCurrent` is set to `true`:

- Permanent address fields are ignored
- System automatically copies current address to permanent
- Reduces data entry effort

### Different Addresses

When `sameAsCurrent` is set to `false`:

- Both current and permanent addresses must be provided
- Each address is validated independently

---

## Security Considerations

### Personal Information

- Salary information should be restricted to employee and HR only
- Bank details should be encrypted in transit and at rest
- Access logs maintained for profile updates

### Company Information

- Company registration details restricted to CEO/Admin roles
- Changes to company info should be audited
- Critical fields may require approval workflow

---

## Integration Points

### Related Modules

- **Module 26** - Technician Performance (employee data)
- **Module 24** - Petty Cash (bank details for reimbursements)
- **Module 21** - Task Management (technician assignments)
- **Payroll System** - Salary information sync
- **Attendance System** - Employee details

---

**End of Documentation**
