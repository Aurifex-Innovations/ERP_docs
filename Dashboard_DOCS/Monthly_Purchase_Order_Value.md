# 📊 Chart: Monthly Purchase Order Value

---

## 🔹 Type  
**Bar Chart (Recommended)**  
*(Alternative: Line Chart for trend view)*

---

## 🎯 Purpose  

To track **total purchase spending per month**, helping:

- Monitor procurement cost  
- Identify high/low spending months  
- Support budgeting & forecasting  

---

## 🧾 Tables Used  

- purchase_order  

---

## 🧩 Fields Used (Exact from Schema)

- po_date  
- grand_total  
- is_deleted  

---

## 🔹 Logic  

- Group data by **month (po_date)**  
- Sum **grand_total**  
- Exclude deleted records  

---

## 🔹 Query (Base)

```sql
SELECT 
    TO_CHAR(po_date, 'YYYY-MM') AS month,
    SUM(grand_total) AS total_purchase_value

FROM purchase_order

WHERE 
    is_deleted = FALSE

GROUP BY 
    TO_CHAR(po_date, 'YYYY-MM')

ORDER BY 
    month;
```


## 🔽 Filters for This Chart

### 🔹 1. Month Picker (Multi-Select Dropdown)

#### 🧾 Request Format
```
{
  "months": ["2026-01", "2026-03", "2026-05"]
}
```

#### 🔹 Query (Month Selection)
```
SELECT 
    TO_CHAR(po_date, 'YYYY-MM') AS month,
    SUM(grand_total) AS total_purchase_value

FROM purchase_order

WHERE 
    is_deleted = FALSE
    AND TO_CHAR(po_date, 'YYYY-MM') IN (:months)

GROUP BY 
    TO_CHAR(po_date, 'YYYY-MM')

ORDER BY 
    month;
```

### 🔹 2. Quick Selection (Preset Filters)
#### 🧾 Request Format
```
{
  "preset": "LAST_6_MONTHS"
}
```

#### 🔹 Query (Preset)
```
SELECT 
    TO_CHAR(po_date, 'YYYY-MM') AS month,
    SUM(grand_total) AS total_purchase_value

FROM purchase_order

WHERE 
    is_deleted = FALSE
    AND po_date >= CURRENT_DATE - 
        CASE 
            WHEN :preset = 'LAST_3_MONTHS' THEN INTERVAL '3 months'
            WHEN :preset = 'LAST_6_MONTHS' THEN INTERVAL '6 months'
            WHEN :preset = 'LAST_12_MONTHS' THEN INTERVAL '12 months'
        END

GROUP BY 
    TO_CHAR(po_date, 'YYYY-MM')

ORDER BY 
    month;
```
