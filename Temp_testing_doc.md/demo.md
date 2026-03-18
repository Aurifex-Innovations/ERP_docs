# 12.2 Add Service Form

**Description:**
The Add Service form allows administrators to create and configure pest control services. The form captures service information, pest categories, treatment methods, chemicals used, pricing configuration, and warranty policies. The service configuration integrates with the **Product Master (Module 10)** to automatically fetch chemicals, product codes, and units of measure (UOM).

---

# Screen Layout

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  [← Back to Services]                 ADD NEW SERVICE                        [Save] │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│  BASIC SERVICE INFORMATION                                                          │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ Service Name:        [____________________________________] *               │  │
│  │ Example: Rodocon Service, Roachcon Service                                  │  │
│  │                                                                              │  │
│  │ Service Category:                                                            │  │
│  │ [☑] Residential  [☑] Commercial  [☑] Industrial  [☑] Warehouse               │  │
│  │ [+ Add Custom Category]                                                      │  │
│  │                                                                              │  │
│  │ Service Code:        [Auto Generated]                                        │  │
│  │                                                                              │  │
│  │ Pest Type Covered:                                                           │  │
│  │ [☑] Rodent   [☑] Cockroach   [☑] Mosquito                                    │  │
│  │ [☑] Termite  [☑] Fly        [☑] Ant                                           │  │
│  │ [☑] Bed Bug  [☑] Spider     [☑] Lizard                                        │  │
│  │ [+ Add Custom Pest Type]                                                      │  │
│  │                                                                              │  │
│  │ Description:                                                                 │  │
│  │ [____________________________________________________________] *             │  │
│  │                                                                              │  │
│  │ Service Duration: [____] [Minutes / Hours ▼]                                 │  │
│  │                                                                              │  │
│  │ Service Status: (•) Active  ( ) Inactive                                     │  │
│  └──────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                     │
│  PEST SPECIES COVERED                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ Add Specific Pest Species                                                     │  │
│  │                                                                              │  │
│  │ Example:                                                                     │  │
│  │ • Roof Rat – Rattus                                                           │  │
│  │ • Norway Rat – Rattus Norvegicus                                             │  │
│  │ • German Cockroach – Blatella Germanica                                      │  │
│  │                                                                              │  │
│  │ [+ Add Species]                                                              │  │
│  └──────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                     │
│  METHOD OF CONTROL / TREATMENT                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ Available Treatment Methods                                                  │  │
│  │                                                                              │  │
│  │ [☑] Gel Baiting                                                              │  │
│  │ [☑] Rodent Baiting                                                           │  │
│  │ [☑] Residual Spray                                                           │  │
│  │ [☑] Dusting Cracks & Crevices                                                │  │
│  │ [☑] Trapping                                                                 │  │
│  │ [☑] Monitoring                                                               │  │
│  │ [☑] Fogging (Thermal / Cold)                                                 │  │
│  │ [☑] Foam Treatment                                                           │  │
│  │ [☑] Soil Treatment                                                           │  │
│  │ [☑] Sticky Pad Trapping                                                      │  │
│  │ [☑] Disinfection                                                             │  │
│  │ [+ Add Custom Treatment Method]                                              │  │
│  └──────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                     │
│  CHEMICALS / PRODUCTS USED                                                         │
│  (Integrated with Module 10 Product Master)                                       │
│                                                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ [Search Product from Product Master...]                                       │  │
│  │                                                                              │  │
│  │ Selected Chemicals                                                           │  │
│  │                                                                              │  │
│  │ ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │ │ Product Name │ Product Code │ UOM │ Standard Usage │ Required Qty │     │ │
│  │ ├──────────────┼──────────────┼─────┼────────────────┼──────────────┤     │ │
│  │ │ Alpha Cypermethrin │ P-001 │ ml │ 10 ml │ [____] ml │              │ │
│  │ │ Chlorpyriphos │ P-002 │ ml │ 20 ml │ [____] ml │                     │ │
│  │ └─────────────────────────────────────────────────────────────────────────┘ │
│  │                                                                              │
│  │ [+ Add Custom Chemical]                                                      │
│  └──────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  PRICING CONFIGURATION                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │ Price Type                                                                   │  │
│  │ (•) Fixed Price                                                              │  │
│  │ ( ) Area Based                                                               │  │
│  │ ( ) Inspection Based                                                         │  │
│  │                                                                              │  │
│  │ Residential Pricing                                                          │  │
│  │ 1BHK [₹____] 2BHK [₹____] 3BHK [₹____] 4BHK+ [₹____]                         │  │
│  │ [+ Add Custom Property Type]                                                 │  │
│  │                                                                              │  │
│  │ Commercial Pricing                                                           │  │
│  │ Small Office [₹____]                                                         │  │
│  │ Medium Office [₹____]                                                        │  │
│  │ Large Office [₹____]                                                         │  │
│  │ Warehouse [₹____]                                                            │  │
│  │ [+ Add Custom Commercial Type]                                               │  │
│  └──────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  WARRANTY / SERVICE GUARANTEE                                                      │
│  ┌──────────────────────────────────────────────────────────────────────────────┐
│  │ Warranty Period: [____] Months                                                │
│  │ Free Revisit Included: [☑] Yes  Qty: [____]                                   │
│  └──────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  SYSTEM FIELDS                                                                     │
│  ┌──────────────────────────────────────────────────────────────────────────────┐
│  │ Service ID: [Auto Generated]                                                  │
│  │ Created Date: [Auto]                                                          │
│  │ Created By: [Admin]                                                           │
│  │ Updated Date: [Auto]                                                          │
│  │ Updated By: [Auto]                                                            │
│  │ Display Order: [____]                                                         │
│  └──────────────────────────────────────────────────────────────────────────────┘
│                                                                                     │
│  [Cancel]                             [Save Draft]                         [Save] │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

---

### **Section 1: Basic Service Information**

| Field             | Type                  | Required | Validation                                          |
| ----------------- | --------------------- | -------- | --------------------------------------------------- |
| Service Name      | Text                  | Yes      | Minimum 3 characters, must be unique                |
| Service Category  | Multi Select Checkbox | Yes      | At least one category must be selected              |
| Custom Category   | Text                  | No       | Appears only if user clicks **Add Custom Category** |
| Service Code      | Auto Generated        | Yes      | System generated format: `SRV-XXXX`                 |
| Pest Type Covered | Multi Select Checkbox | Yes      | Minimum one pest must be selected                   |
| Custom Pest Type  | Text                  | No       | Enabled when **Add Custom Pest Type** selected      |
| Description       | Text Area             | Yes      | Minimum 10 characters                               |
| Service Duration  | Number + UOM          | Yes      | Value must be > 0                                   |
| Service Status    | Radio                 | Yes      | Active / Inactive                                   |

---

### **Section 2: Pest Species Covered**

| Field             | Type   | Required | Validation                         |
| ----------------- | ------ | -------- | ---------------------------------- |
| Pest Species Name | Text   | No       | Must allow multiple entries        |
| Scientific Name   | Text   | No       | Optional scientific classification |
| Add Species       | Button | No       | Adds row dynamically               |

---

### **Section 3: Method of Control / Treatment**

| Field            | Type     | Required | Validation                                     |
| ---------------- | -------- | -------- | ---------------------------------------------- |
| Treatment Method | Checkbox | Yes      | At least one treatment method required         |
| Custom Treatment | Text     | No       | Allowed when user clicks **Add Custom Method** |

---

### **Section 4: Chemicals / Products Used**

_(Integrated with Module 10 Product Master)_

| Field           | Type   | Required | Validation                       |
| --------------- | ------ | -------- | -------------------------------- |
| Product Name    | Lookup | Yes      | Must exist in Product Master     |
| Product Code    | Auto   | Yes      | Auto fetched                     |
| UOM             | Auto   | Yes      | From product master              |
| Standard Usage  | Auto   | Yes      | Based on product configuration   |
| Required Qty    | Number | Yes      | Must be ≥ 0                      |
| Custom Chemical | Text   | No       | Allowed if product not available |

**Business Rule**

- When a **product is selected**, the following fields auto populate
  - Product Code
  - UOM
  - Standard Usage

- Required Qty must follow **UOM measurement**.

Example

| Product    | UOM | Required Qty |
| ---------- | --- | ------------ |
| Chemical A | ml  | 50 ml        |
| Powder B   | gm  | 100 gm       |

---

### **Section 5: Pricing Configuration**

| Field                  | Type   | Required    | Validation                                |
| ---------------------- | ------ | ----------- | ----------------------------------------- |
| Price Type             | Radio  | Yes         | Fixed / Area Based / Inspection Based     |
| Residential Pricing    | Number | Conditional | Required if Residential category selected |
| Commercial Pricing     | Number | Conditional | Required if Commercial category selected  |
| Custom Property Type   | Text   | No          | Allowed via Add Custom                    |
| Custom Commercial Type | Text   | No          | Allowed via Add Custom                    |

---

### **Section 6: Warranty / Service Guarantee**

| Field                 | Type     | Required    | Validation                           |
| --------------------- | -------- | ----------- | ------------------------------------ |
| Warranty Period       | Number   | No          | Value must be ≥ 0                    |
| Free Revisit Included | Checkbox | No          | If checked quantity must be provided |
| Free Revisit Quantity | Number   | Conditional | Required when revisit enabled        |

---

### **Section 7: System Fields**

| Field         | Type   | Required | Validation                        |
| ------------- | ------ | -------- | --------------------------------- |
| Service ID    | Auto   | Yes      | System generated                  |
| Created Date  | Auto   | Yes      | System timestamp                  |
| Created By    | Auto   | Yes      | Logged user                       |
| Updated Date  | Auto   | Yes      | Updated automatically             |
| Updated By    | Auto   | Yes      | Logged user                       |
| Display Order | Number | No       | Used for service listing priority |
