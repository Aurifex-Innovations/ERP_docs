# 📊 QA Dashboard Testing & Validation Guide

## 1. Document Information
| Field | Details |
| :--- | :--- |
| **Module** | ERP Dashboard Module |
| **Target Audience** | QA Engineers, Backend Developers, Frontend Developers |
| **Purpose** | To provide a comprehensive testing framework and data validation guidelines for the Dashboard Charts. |

## 2. Objective
This document outlines the testing strategy, test cases, and data validation requirements for the dashboard charts. It ensures that data representations are accurate, API payloads function correctly, and business logic aligns with the SQL queries defined in the schema.

---

## 3. Common Filter Validations (Applicable to All Charts)
All dashboard charts support two filtering mechanisms: **Month Picker** (Multi-Select) and **Quick Selection** (Presets). 

### 🟢 3.1. Month Picker (Multi-Select)
* **Payload Format:** `{ "months": ["YYYY-MM", "YYYY-MM"] }`
* **Test Scenarios:**
  * **TC_COM_001:** Select a single month. Verify the chart only displays data for that month.
  * **TC_COM_002:** Select non-consecutive months (e.g., `["2026-01", "2026-05"]`). Verify the chart successfully handles gaps and only displays the exact selected months.
  * **TC_COM_003:** Select a month with no data. Verify the chart displays an appropriate "No Data" or empty state without crashing.
  * **TC_COM_004 (Negative):** Pass an invalid date format (e.g., `["26-01"]` or `["Jan-2026"]`). Verify the API returns a `400 Bad Request` with a clear validation error.

### 🟢 3.2. Quick Selection (Presets)
* **Payload Format:** `{ "preset": "LAST_6_MONTHS" }`
* **Presets Available:** `LAST_3_MONTHS`, `LAST_6_MONTHS`, `LAST_12_MONTHS`
* **Test Scenarios:**
  * **TC_COM_005:** Select `LAST_3_MONTHS`. Verify data spans exactly from `CURRENT_DATE - 3 months` up to today.
  * **TC_COM_006:** Select `LAST_12_MONTHS`. Verify the X-axis handles 12 distinct data points without overlapping UI elements.
  * **TC_COM_007 (Negative):** Pass an unrecognized preset (e.g., `LAST_5_MONTHS`). Verify backend validation rejects the request.

---

## 4. Detailed Chart Test Cases & Data Validation

### 📈 Chart 1: Monthly Contract Revenue
* **Type:** Bar Chart / Line Chart
* **Purpose:** Monitor confirmed business revenue based on ACTIVE contracts.

#### Backend / Database Validation
| Table | Fields Used | Filter Conditions | Aggregation |
| :--- | :--- | :--- | :--- |
| `contracts` | `created_at`, `total_sale_value`, `status` | `status = 'ACTIVE'` | Sum of `total_sale_value` grouped by month (`created_at`) |

#### QA Test Cases
* **TC_CH1_001:** Create contracts with statuses like `DRAFT`, `PENDING`, `CANCELLED`. Verify their `total_sale_value` is **strictly excluded** from the total revenue sum.
* **TC_CH1_002:** Create multiple `ACTIVE` contracts in a single month. Verify the chart accurately sums their `total_sale_value`.
* **TC_CH1_003:** Create an `ACTIVE` contract with `total_sale_value = 0` or negative (if permissible). Ensure the system handles it without mathematical errors.

---

### 📊 Chart 2: Monthly GMA Value (Cost vs Price vs Margin)
* **Type:** Grouped Bar Chart
* **Purpose:** Compare financial performance of GMA sheets (Cost vs. Price vs. Margin).

#### Backend / Database Validation
| Table | Fields Used | Filter Conditions | Aggregation |
| :--- | :--- | :--- | :--- |
| `gma_sheets` | `created_at`, `total_annual_cost`, `total_annual_price`, `overall_gross_margin`, `is_deleted` | `is_deleted = FALSE` | Sum of Cost, Sum of Price, Average of Gross Margin grouped by month |

#### QA Test Cases
* **TC_CH2_001:** Soft-delete a GMA sheet (`is_deleted = TRUE`). Verify its values are completely removed from the aggregations.
* **TC_CH2_002:** Verify that the "Margin" bar reflects the **Average** (`AVG(overall_gross_margin)`), whereas Cost and Price bars reflect the **Sum**.
* **TC_CH2_003:** Verify UI handles differing scales (e.g., Cost/Price might be in thousands/millions, while Margin might be a smaller percentage or ratio). Tooltips should clearly differentiate them.

---

### 📉 Chart 3: Monthly Purchase Order Value
* **Type:** Bar Chart / Line Chart
* **Purpose:** Track total procurement spending per month.

#### Backend / Database Validation
| Table | Fields Used | Filter Conditions | Aggregation |
| :--- | :--- | :--- | :--- |
| `purchase_order`| `po_date`, `grand_total`, `is_deleted` | `is_deleted = FALSE` | Sum of `grand_total` grouped by month (`po_date`) |

#### QA Test Cases
* **TC_CH3_001 (Crucial):** Ensure data is grouped by `po_date` and NOT `created_at`. Create a backdated PO (e.g., generated today but `po_date` is 3 months ago) and verify it appears in the historical month.
* **TC_CH3_002:** Soft-delete a purchase order (`is_deleted = TRUE`). Verify it is excluded from the chart total.
* **TC_CH3_003:** Validate floating-point precision on `grand_total` sums to ensure no decimal data loss during aggregation.

---

### 📊 Chart 4: Monthly Stock Comparison
* **Type:** Grouped Bar Chart
* **Purpose:** Compare monthly movement of Assets vs. Consumables vs. Resell items.

#### Backend / Database Validation
| Table | Fields Used | Filter Conditions | Aggregation |
| :--- | :--- | :--- | :--- |
| `stock_movement_logs` | `created_at`, `quantity_delta`, `stock_type` | Split by `stock_type` (`ASSET`, `CONSUMABLE`, `RESELL`) | Sum of `quantity_delta` for each type grouped by month |

#### QA Test Cases
* **TC_CH4_001:** Create movements with negative `quantity_delta` (stock reductions). Verify how the chart renders negative values (bars dipping below the X-axis) or if absolute values are expected based on design.
* **TC_CH4_002:** Verify the conditional splitting (`CASE WHEN stock_type = ...`). Ensure only 'ASSET', 'CONSUMABLE', and 'RESELL' types are aggregated. If an unrecognized type exists (e.g., 'DEFECTIVE'), it should default to `0` and not corrupt the other columns.
* **TC_CH4_003:** Verify that each month on the X-axis strictly shows up to 3 bars grouped together, reflecting the respective categories.

---

## 5. UI/UX & Edge Case Testing

* **Tooltips:** Hovering over bars/points MUST display the exact numerical value, formatted according to currency or quantity standards.
* **Responsiveness:** Shrink the browser window to mobile/tablet size. Verify legends do not overlap and the X-axis labels (months) truncate or rotate elegantly without overlapping.
* **Empty States:** When a filter yields zero results, verify the chart shows a clean "No Data Available" graphic instead of a broken grid or console errors.
* **Loading States:** Verify a skeleton loader or spinner is shown while the API fetches data to prevent UI freezing.
* **Timezone Consistency:** Ensure that dates fetched are transformed properly so that a record created at `2026-01-31 23:59:59` isn't accidentally pushed to `2026-02` due to timezone offset discrepancies.
