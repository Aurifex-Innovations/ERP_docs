# 🎯 MODULE 14: Purchase Order

## 14.1 Purchase Order Dashboard Table View



## Purchase Order Screen

  ### Screen Layout


```md
┌─────────────────────────────────────────────────────────────────────────────┐
│  PURCHASE ORDERS                                          [+ New PO] [⚙️]  │
├─────────────────────────────────────────────────────────────────────────────┤
│  🔍 Search PO, Vendor, Product, HSN... [                              ]    │
├─────────────────────────────────────────────────────────────────────────────┤
│  FILTERS:                                                                    │
│  [Status: All ▼] [PO Date: All ▼] [Vendor: All ▼] [Tax Type: All ▼]         │
│  [Amount: ₹0 - ₹Max ▼] [State: All ▼] [Product: All ▼] [Reset]             │
├─────────────────────────────────────────────────────────────────────────────┤
│  ┌──────────┬──────────┬─────────────┬──────────┬─────────┬────────┬─────┐ │
│  │ PO No    │ PO Date  │ Vendor      │ Items    │ Status  │ Total  │ Act │ │
│  ├──────────┼──────────┼─────────────┼──────────┼─────────┼────────┼─────┤ │
│  │PO-24-001 │15/03/2024│ABC Supplies │ 5 items  │●Approved│₹45,230 │👁✏🗑│ │
│  │PO-24-002 │14/03/2024│XYZ Corp     │ 3 items  │⏳Pending │₹12,500 │👁✏🗑│ │
│  └──────────┴──────────┴─────────────┴──────────┴─────────┴────────┴─────┘ │
│                                                                              │
│  Showing 1-10 of 156 records          [< Prev] [1][2][3]...[16] [Next >]    │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Filter Brakdown
| Filter Name           | Field ID                   | Source Field        | Data Type      | Format Example            | Options/Values                                                  |
| --------------------- | -------------------------- | ------------------- | -------------- | ------------------------- | --------------------------------------------------------------- |
| **Status**            | `filter_status`            | `status`            | Enum (Multi)   | `Approved`                | Draft, Pending Approval, Approved, Ordered, Received, Cancelled |
| **PO Date Range**     | `filter_po_date`           | `po_date`           | Date Range     | `01/03/2024 - 31/03/2024` | Today, This Week, This Month, Custom Range                      |
| **Vendor Name**       | `filter_vendor_name`       | `vendor_name`       | Search String  | `ABC Supplies`            | Vendor Master Search                                            |
| **Tax Type**          | `filter_tax_type`          | `tax_type`          | Enum           | `Intra State`             | Intra State, Inter State                                        |
| **Amount Range**      | `filter_amount_range`      | `grand_total`       | Currency Range | `₹0 - ₹1,00,000`          | Min-Max Slider                                                  |
| **Delivery State**    | `filter_delivery_state`    | `state`             | String         | `Maharashtra`             | State Master                                                    |
| **Product Name**      | `filter_product_name`      | `product_name`      | Search String  | `Insecticide`             | Product Master Search                                           |
| **Brand/Company**     | `filter_brand`             | `brand_company`     | String         | `Bayer`                   | Auto Fetch List                                                 |
| **HSN Code**          | `filter_hsn_code`          | `hsn_code`          | String         | `3808`                    | Module 9 Linked                                                 |
| **Category**          | `filter_category`          | `category`          | String         | `Pesticides`              | Auto Fetch List                                                 |
| **Authorized Person** | `filter_authorized_person` | `authorized_person` | String         | `John Doe`                | User Master                                                     |


### Table field brakdown
| Column          | Field ID      | Source          | Type     | Format Example         | Width | Alignment |
| --------------- | ------------- | --------------- | -------- | ---------------------- | ----- | --------- |
| **PO Number**   | `po_number`   | Auto Generated  | String   | `PO-2024-0001`         | 110px | Left      |
| **PO Date**     | `po_date`     | Date Picker     | Date     | `15/03/2024`           | 95px  | Center    |
| **Vendor Name** | `vendor_name` | Vendor Master   | String   | `ABC Supplies Pvt Ltd` | 180px | Left      |
| **Item Count**  | `item_count`  | Calculated      | Integer  | `5 items`              | 80px  | Center    |
| **Status**      | `status`      | Dropdown        | Badge    | `● Approved`           | 120px | Center    |
| **Grand Total** | `grand_total` | Auto Calculated | Currency | `₹45,230.00`           | 120px | Right     |
| **Actions**     | `actions`     | -               | Icons    | `👁 ✏ 🗑`              | 90px  | Center    |


