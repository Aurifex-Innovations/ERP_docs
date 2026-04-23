# 📊 Chart 6: Monthly Stock Comparison (Assets vs Consumables vs Resell)

## Type

- Grouped Bar Chart

## Purpose

**Compare monthly movement of each stock type:**

- Assets 📦
- Consumables 🧪
- Resell 🛒

## Tables Used
- stock_movement_logs

## Fields Used (Exact from DB)
- created_at
- quantity_delta
- stock_type


## Assumption (VERY IMPORTANT — based on your schema)

**stock_type contains values like:**

- 'ASSET'
- 'CONSUMABLE'
- 'RESELL'

- (If naming differs, adjust CASE conditions accordingly)

## Logic
```
Group by month
Split data using stock_type
Aggregate movement separately
```
Query
```
SELECT 
    TO_CHAR(created_at, 'YYYY-MM') AS month,

    SUM(CASE 
        WHEN stock_type = 'ASSET' 
        THEN quantity_delta 
        ELSE 0 
    END) AS assets,

    SUM(CASE 
        WHEN stock_type = 'CONSUMABLE' 
        THEN quantity_delta 
        ELSE 0 
    END) AS consumables,

    SUM(CASE 
        WHEN stock_type = 'RESELL' 
        THEN quantity_delta 
        ELSE 0 
    END) AS resell

FROM stock_movement_logs

GROUP BY 
    TO_CHAR(created_at, 'YYYY-MM')

ORDER BY 
    month;
```

## Chart Representation
- X-Axis → Month (YYYY-MM)
- Y-Axis → Quantity


## Bars (per month):
- Assets
- Consumables
- Resell




# 🔽 Filters Documentation

## This chart supports two types of filters:

## 🔹 1. Month Picker (Multi-Select Dropdown)
### 🎯 Purpose
 
### Allow users to select specific months manually for comparison.

### 🧾 UI Input

#### 📅 Dropdown (Multi Select)

**Example:**

- Jan 2026
- Feb 2026
- Mar 2026

### 🔹 Backend Request Format
```
{
  "months": ["2026-01", "2026-03", "2026-05"]
}
```

- 👉 Format must be: YYYY-MM

### 🔹 Query (Multi-Month Selection)
```
SELECT 
    TO_CHAR(created_at, 'YYYY-MM') AS month,

    SUM(CASE 
        WHEN stock_type = 'ASSET' 
        THEN quantity_delta 
        ELSE 0 
    END) AS assets,

    SUM(CASE 
        WHEN stock_type = 'CONSUMABLE' 
        THEN quantity_delta 
        ELSE 0 
    END) AS consumables,

    SUM(CASE 
        WHEN stock_type = 'RESELL' 
        THEN quantity_delta 
        ELSE 0 
    END) AS resell

FROM stock_movement_logs

WHERE 
    TO_CHAR(created_at, 'YYYY-MM') IN (:months)

GROUP BY 
    TO_CHAR(created_at, 'YYYY-MM')

ORDER BY 
    month;
```

### ⚠️ Notes
- Supports non-continuous comparison (e.g., Jan vs March)
- Best for custom analysis


## 🔹 2. Quick Selection (Preset Filters)

### 🎯 Purpose

- Allow users to quickly select recent time windows

### 🧾 UI Options
- Last 3 Months
- Last 6 Months
- Last 12 Months

### 🔹 Backend Request Format
```
{
  "preset": "LAST_6_MONTHS"
}
```

### 🔹 Backend Mapping Logic
```
Preset	Logic
LAST_3_MONTHS	CURRENT_DATE - INTERVAL '3 months'
LAST_6_MONTHS	CURRENT_DATE - INTERVAL '6 months'
LAST_12_MONTHS	CURRENT_DATE - INTERVAL '12 months'
```

### 🔹 Query (Preset Filter)
```
SELECT 
    TO_CHAR(created_at, 'YYYY-MM') AS month,

    SUM(CASE 
        WHEN stock_type = 'ASSET' 
        THEN quantity_delta 
        ELSE 0 
    END) AS assets,

    SUM(CASE 
        WHEN stock_type = 'CONSUMABLE' 
        THEN quantity_delta 
        ELSE 0 
    END) AS consumables,

    SUM(CASE 
        WHEN stock_type = 'RESELL' 
        THEN quantity_delta 
        ELSE 0 
    END) AS resell

FROM stock_movement_logs

WHERE 
    created_at >= CURRENT_DATE - 
    CASE 
        WHEN :preset = 'LAST_3_MONTHS' THEN INTERVAL '3 months'
        WHEN :preset = 'LAST_6_MONTHS' THEN INTERVAL '6 months'
        WHEN :preset = 'LAST_12_MONTHS' THEN INTERVAL '12 months'
    END

GROUP BY 
    TO_CHAR(created_at, 'YYYY-MM')

ORDER BY 
    month;
```
