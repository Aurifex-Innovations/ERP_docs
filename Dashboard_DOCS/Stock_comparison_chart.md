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
