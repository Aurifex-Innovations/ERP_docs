# 📊 Chart 6: Monthly Stock Comparison (Category-wise)

## Type

- Grouped Bar Chart

## Purpose

- To compare monthly stock movement across categories, helping:
```
Identify which category is moving more each month
Detect demand trends per category
Support procurement and planning decisions
```

## Tables Used
- stock_movement_logs
- stock_ledger

## Fields Used (Exact from DB)

### From stock_movement_logs:

- created_at
- quantity_delta
- product_id

### From stock_ledger:

- product_id
- category

## Logic
### Group by:
- Month (from created_at)
- Category (from stock_ledger)
### Aggregate:
- Total movement using quantity_delta

## Query
```
SELECT 
    TO_CHAR(sml.created_at, 'YYYY-MM') AS month,
    sl.category,
    SUM(sml.quantity_delta) AS total_movement

FROM stock_movement_logs sml
JOIN stock_ledger sl 
ON sml.product_id = sl.product_id

GROUP BY 
    TO_CHAR(sml.created_at, 'YYYY-MM'),
    sl.category

ORDER BY 
    month,
    sl.category;
```

## Chart Representation
- X-Axis → Month (YYYY-MM)
- Y-Axis → Quantity (total_movement)

## Bars (Grouped by Month):
- Each category = one bar in the group
