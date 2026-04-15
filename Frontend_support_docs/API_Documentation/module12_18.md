# 📋 API Documentation

> **Reference guide for all module endpoints, screen mappings, and API calls.**

---

## Table of Contents

- [Module 12 – Service Management](#module-12--service-management)
- [Module 13 – Vendor Management](#module-13--vendor-management)
- [Module 14 – Purchase Order](#module-14--purchase-order)
- [Module 15 – Leads & Follow-Up Management](#module-15--leads--follow-up-management)
- [Module 16 – Quotation Management](#module-16--quotation-management)
- [Module 17 – Gross Margin Analysis (GMA) Management](#module-17--gross-margin-analysis-gma-management)
- [Module 18 – Customer Management](#module-18--customer-management)

---

## Module 12 – Service Management

### 12.1 Service Dashboard – Table View

| Method | Endpoint           |
| ------ | ------------------ |
| `GET`  | `/api/v1/services` |

---

### 12.2 Add Service Form

| Method | Endpoint                                | Purpose                          |
| ------ | --------------------------------------- | -------------------------------- |
| `POST` | `/api/v1/services`                      | Create new service               |
| `POST` | `/api/v1/service-categories`            | Create service category          |
| `GET`  | `/api/v1/service-categories`            | Service category dropdown        |
| `GET`  | `/api/v1/service-sub-categories`        | Subcategory dropdown             |
| `POST` | `/api/v1/service-pest-types`            | Create pest type                 |
| `GET`  | `/api/v1/service-pest-types`            | Pest type dropdown               |
| `POST` | `/api/v1/service-treatments`            | Create treatment                 |
| `GET`  | `/api/v1/service-treatments`            | Treatment dropdown               |
| `POST` | `/api/v1/service-category-fixed`        | Create category fixed row        |
| `GET`  | `/api/v1/service-category-fixed`        | Category fixed row dropdown      |
| `POST` | `/api/v1/service-category-area`         | Create category area row         |
| `GET`  | `/api/v1/service-category-area`         | Category area row dropdown       |
| `POST` | `/api/v1/service-category-inspection`   | Create category inspection row   |
| `GET`  | `/api/v1/service-category-inspection`   | Category inspection row dropdown |
| `POST` | `/api/v1/service-custom-pricing-blocks` | Create custom pricing block      |
| `GET`  | `/api/v1/service-custom-pricing-blocks` | Custom pricing block dropdown    |

---

### 12.3 Edit Service

| Method | Endpoint                                | Purpose                          |
| ------ | --------------------------------------- | -------------------------------- |
| `PUT`  | `/api/v1/services/update`               | Update existing service          |
| `POST` | `/api/v1/service-categories`            | Create service category          |
| `GET`  | `/api/v1/service-categories`            | Service category dropdown        |
| `GET`  | `/api/v1/service-sub-categories`        | Subcategory dropdown             |
| `POST` | `/api/v1/service-pest-types`            | Create pest type                 |
| `GET`  | `/api/v1/service-pest-types`            | Pest type dropdown               |
| `POST` | `/api/v1/service-treatments`            | Create treatment                 |
| `GET`  | `/api/v1/service-treatments`            | Treatment dropdown               |
| `POST` | `/api/v1/service-category-fixed`        | Create category fixed row        |
| `GET`  | `/api/v1/service-category-fixed`        | Category fixed row dropdown      |
| `POST` | `/api/v1/service-category-area`         | Create category area row         |
| `GET`  | `/api/v1/service-category-area`         | Category area row dropdown       |
| `POST` | `/api/v1/service-category-inspection`   | Create category inspection row   |
| `GET`  | `/api/v1/service-category-inspection`   | Category inspection row dropdown |
| `POST` | `/api/v1/service-custom-pricing-blocks` | Create custom pricing block      |
| `GET`  | `/api/v1/service-custom-pricing-blocks` | Custom pricing block dropdown    |

---

### 12.4 View Service

| Method | Endpoint                 |
| ------ | ------------------------ |
| `GET`  | `/api/v1/services/by-id` |

---

## Module 13 – Vendor Management

### 13.1 Vendor Dashboard – Table View

| Method | Endpoint          |
| ------ | ----------------- |
| `GET`  | `/api/v1/vendors` |

---

### 13.2 Add Vendor – Vendor Registration Screen

| Method | Endpoint                              | Purpose                                  |
| ------ | ------------------------------------- | ---------------------------------------- |
| `POST` | `/api/v1/vendors`                     | Register new vendor                      |
| `GET`  | `/api/v1/inventory-products/dropdown` | Product dropdown _(if product required)_ |
| `GET`  | `/api/v1/inventory-products/by-id`    | Product details by selected product      |

---

### 13.3 Edit Vendor – Vendor Update Screen

| Method | Endpoint                              | Purpose                                  |
| ------ | ------------------------------------- | ---------------------------------------- |
| `PUT`  | `/api/v1/vendors/update`              | Update vendor details                    |
| `GET`  | `/api/v1/inventory-products/dropdown` | Product dropdown _(if product required)_ |
| `GET`  | `/api/v1/inventory-products/by-id`    | Product details by selected product      |

---

### 13.4 View Vendor – Vendor Detail Screen

| Method   | Endpoint                   | Purpose                  |
| -------- | -------------------------- | ------------------------ |
| `GET`    | `/api/v1/vendors/by-id`    | Fetch vendor details     |
| `GET`    | `/api/v1/vendors/download` | Download vendor document |
| `DELETE` | `/api/v1/vendors/delete`   | Deactivate vendor        |

---

## Module 14 – Purchase Order

### 14.1 Purchase Order – Table View

| Method | Endpoint                  |
| ------ | ------------------------- |
| `GET`  | `/api/v1/purchase-orders` |

---

### 14.2 Create Purchase Order

| Method | Endpoint                              | Purpose                                  |
| ------ | ------------------------------------- | ---------------------------------------- |
| `POST` | `/api/v1/purchase-orders`             | Create new purchase order                |
| `GET`  | `/api/v1/vendors/dropdown`            | Vendor dropdown                          |
| `GET`  | `/api/v1/vendors/by-id`               | Vendor details by selected vendor        |
| `GET`  | `/api/v1/inventory-products/dropdown` | Product dropdown _(if product required)_ |
| `GET`  | `/api/v1/inventory-products/by-id`    | Product details by selected product      |

---

### 14.3 Edit Purchase Order

| Method | Endpoint                              | Purpose                                  |
| ------ | ------------------------------------- | ---------------------------------------- |
| `POST` | `/api/v1/purchase-orders/update`      | Update purchase order                    |
| `GET`  | `/api/v1/vendors/dropdown`            | Vendor dropdown                          |
| `GET`  | `/api/v1/vendors/by-id`               | Vendor details by selected vendor        |
| `GET`  | `/api/v1/inventory-products/dropdown` | Product dropdown _(if product required)_ |
| `GET`  | `/api/v1/inventory-products/by-id`    | Product details by selected product      |

---

### 14.4 View Purchase Order

| Method | Endpoint                            |
| ------ | ----------------------------------- |
| `GET`  | `/api/v1/purchase-orders/get-by-id` |

---

### 14.5 Delete Purchase Order – Confirmation Popup (Permanent Delete)

| Method   | Endpoint                               | Purpose                           |
| -------- | -------------------------------------- | --------------------------------- |
| `DELETE` | `/api/v1/purchase-orders/delete`       | Permanently delete purchase order |
| `GET`    | `/api/v1/purchase-orders/download-pdf` | Download purchase order PDF       |

---

## Module 15 – Leads & Follow-Up Management

### 15.1 Lead Dashboard – Table View

| Method | Endpoint        |
| ------ | --------------- |
| `GET`  | `/api/v1/leads` |

---

### 15.2 Add Lead Form

| Method | Endpoint                            | Purpose         |
| ------ | ----------------------------------- | --------------- |
| `POST` | `/api/v1/leads`                     | Create new lead |
| `GET`  | `/api/v1/company/branches/dropdown` | Branch dropdown |

---

### 15.3 Edit Lead Form

| Method | Endpoint                            | Purpose             |
| ------ | ----------------------------------- | ------------------- |
| `PUT`  | `/api/v1/leads`                     | Update lead details |
| `GET`  | `/api/v1/company/branches/dropdown` | Branch dropdown     |

---

### 15.4 View Lead Information _(2 Tabs)_

#### Tab 1 – Basic Lead Information

| Method | Endpoint              |
| ------ | --------------------- |
| `GET`  | `/api/v1/leads/by-id` |

#### 15.4.1 Tab 2 – Follow-Up Log Table

| Method | Endpoint                  |
| ------ | ------------------------- |
| `GET`  | `/api/v1/follow-ups/lead` |

---

### 15.5 Add Follow-Up Form

| Method | Endpoint             |
| ------ | -------------------- |
| `POST` | `/api/v1/follow-ups` |

---

### 15.6 View Follow-Up Detail

| Method | Endpoint             |
| ------ | -------------------- |
| `GET`  | `/api/v1/follow-ups` |

---

## Module 16 – Quotation Management

### 16.1 Quotation Dashboard – Table View

| Method | Endpoint             |
| ------ | -------------------- |
| `GET`  | `/api/v1/quotations` |

---

### 16.2 Add Quotation Form

| Method | Endpoint                              | Purpose                                  |
| ------ | ------------------------------------- | ---------------------------------------- |
| `POST` | `/api/v1/quotations`                  | Create new quotation                     |
| `GET`  | `/api/v1/leads/dropdown`              | Lead dropdown                            |
| `GET`  | `/api/v1/leads/by-id`                 | Lead details by selected lead            |
| `GET`  | `/api/v1/customer/dropdown`           | Customer dropdown                        |
| `GET`  | `/api/v1/customer/by-id`              | Customer details by selected customer    |
| `GET`  | `/api/v1/company/branches/dropdown`   | Branch dropdown                          |
| `GET`  | `/api/v1/services/dropdown`           | Service dropdown                         |
| `GET`  | `/api/v1/services/by-id`              | Service details by selected service      |
| `GET`  | `/api/v1/inventory-products/dropdown` | Product dropdown _(if product required)_ |
| `GET`  | `/api/v1/inventory-products/by-id`    | Product details by selected product      |

---

### 16.3 View Quotation Details

| Method | Endpoint                   |
| ------ | -------------------------- |
| `GET`  | `/api/v1/quotations/by-id` |

---

### 16.4 Delete Quotation – Confirmation Popup

| Method   | Endpoint                                     | Purpose                    |
| -------- | -------------------------------------------- | -------------------------- |
| `DELETE` | `/api/v1/quotations`                         | Delete quotation           |
| `GET`    | `/api/v1/quotations/{{quotation_id}}/pdf`    | Download quotation PDF     |
| `POST`   | `/api/v1/quotations/{{quotation_id}}/send`   | Send quotation via email   |
| `POST`   | `/api/v1/quotations/{{quotation_id}}/resend` | Resend quotation via email |

---

## Module 17 – Gross Margin Analysis (GMA) Management

### 17.1 Tab 1 – GMA Dashboard – Table View

| Method | Endpoint             |
| ------ | -------------------- |
| `GET`  | `/api/v1/gma/sheets` |

---

### 17.2 Tab 2 – My Requests

| Method | Endpoint                         |
| ------ | -------------------------------- |
| `GET`  | `/api/v1/gma/sheets/my-requests` |

---

### 17.3 Tab 3 – Received Requests

| Method | Endpoint                               |
| ------ | -------------------------------------- |
| `GET`  | `/api/v1/gma/sheets/received-requests` |

---

### 17.1.1 Add GMA Sheet

| Method | Endpoint                              | Purpose                                  |
| ------ | ------------------------------------- | ---------------------------------------- |
| `POST` | `/api/v1/gma/sheets`                  | Create new GMA sheet                     |
| `GET`  | `/api/v1/leads/dropdown`              | Lead dropdown                            |
| `GET`  | `/api/v1/leads/by-id`                 | Lead details by selected lead            |
| `GET`  | `/api/v1/customer/dropdown`           | Customer dropdown                        |
| `GET`  | `/api/v1/customer/by-id`              | Customer details by selected customer    |
| `GET`  | `/api/v1/company/branches/dropdown`   | Branch dropdown                          |
| `GET`  | `/api/v1/services/dropdown`           | Service dropdown                         |
| `GET`  | `/api/v1/services/by-id`              | Service details by selected service      |
| `GET`  | `/api/v1/inventory-products/dropdown` | Product dropdown _(if product required)_ |
| `GET`  | `/api/v1/inventory-products/by-id`    | Product details by selected product      |

---

### 17.1.2 View GMA Sheet – Tab 1 (Read Only)

| Method | Endpoint                           | Purpose                 |
| ------ | ---------------------------------- | ----------------------- |
| `GET`  | `/api/v1/gma/sheets/by-id`         | Fetch GMA sheet details |
| `GET`  | `/api/v1/gma/sheets/{{gmaId}}/pdf` | Download GMA sheet PDF  |

---

### 17.2.1 View GMA Sheet – My Requests

| Method | Endpoint                         |
| ------ | -------------------------------- |
| `GET`  | `/api/v1/gma/sheets/my-requests` |

---

### 17.2.2 Delete (Revoke) GMA Sheet

| Method | Endpoint                              |
| ------ | ------------------------------------- |
| `PUT`  | `/api/v1/gma/sheets/{{gmaId}}/revoke` |

---

### 17.3.1 Received GMA Sheet – Approver View

| Method | Endpoint                               |
| ------ | -------------------------------------- |
| `GET`  | `/api/v1/gma/sheets/received-requests` |

---

### 17.3.2 Approve / Reject GMA Sheet

| Method | Endpoint                                |
| ------ | --------------------------------------- |
| `PUT`  | `/api/v1/gma/sheets/{{gmaId}}/decision` |

---

## Module 18 – Customer Management

### 18.1 Customer Master List View

| Method | Endpoint           |
| ------ | ------------------ |
| `GET`  | `/api/v1/customer` |

---

### 18.2 Add Customer Form

| Method | Endpoint                 | Purpose                       |
| ------ | ------------------------ | ----------------------------- |
| `POST` | `/api/v1/customer`       | Create new customer           |
| `GET`  | `/api/v1/leads/dropdown` | Lead dropdown                 |
| `GET`  | `/api/v1/leads/by-id`    | Lead details by selected lead |

---

### 18.3 View Customer Details _(3 Tabs)_

#### 18.3.1 Tab 1 – Basic Details

| Method | Endpoint                 |
| ------ | ------------------------ |
| `GET`  | `/api/v1/customer/by-id` |

#### 18.3.2 Tab 2 – Contract Logs

| Method | Endpoint                         |
| ------ | -------------------------------- |
| `GET`  | `/api/v1/customer/contract-logs` |

#### 18.3.3 Tab 3 – Sales Orders & Service History

| Method | Endpoint                                        |
| ------ | ----------------------------------------------- |
| `GET`  | `/api/v1/customer/sales-orders-service-history` |

---

### 18.4 Edit Customer Form

| Method | Endpoint                  |
| ------ | ------------------------- |
| `PUT`  | `/api/v1/customer/update` |

---

### 18.5 Delete (Deactivate) Customer

| Method   | Endpoint                  |
| -------- | ------------------------- |
| `DELETE` | `/api/v1/customer/delete` |

---

_Last updated: April 2026_
