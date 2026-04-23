# 📊 Chart: Monthly GMA Value (Cost vs Price vs Margin)

---

## 🔹 Type  
**Grouped Bar Chart**

---

## 🎯 Purpose  

To compare **monthly financial performance of GMA sheets**, including:

- Total Annual Cost  
- Total Annual Price  
- Gross Margin  

This helps in:

- Tracking profitability trends  
- Identifying high-margin months  
- Supporting pricing and business decisions  

---

## 🧾 Tables Used  

- gma_sheets  

---

## 🧩 Fields Used (Exact from Schema)

- created_at  
- total_annual_cost  
- total_annual_price  
- overall_gross_margin  
- is_deleted  

---

## 🔹 Logic  

- Group data by **month (created_at)**  
- Aggregate:
  - SUM(total_annual_cost)  
  - SUM(total_annual_price)  
- Calculate average margin:
  - AVG(overall_gross_margin)  
- Exclude deleted records  

---

## 🔹 Query (Base)

```sql
SELECT 
    TO_CHAR(created_at, 'YYYY-MM') AS month,

    SUM(total_annual_cost) AS total_cost,
    SUM(total_annual_price) AS total_price,
    AVG(overall_gross_margin) AS avg_margin

FROM gma_sheets

WHERE 
    is_deleted = FALSE

GROUP BY 
    TO_CHAR(created_at, 'YYYY-MM')

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

####🔹 Query (Month Selection)
```
SELECT 
    TO_CHAR(created_at, 'YYYY-MM') AS month,

    SUM(total_annual_cost) AS total_cost,
    SUM(total_annual_price) AS total_price,
    AVG(overall_gross_margin) AS avg_margin

FROM gma_sheets

WHERE 
    is_deleted = FALSE
    AND TO_CHAR(created_at, 'YYYY-MM') IN (:months)

GROUP BY 
    TO_CHAR(created_at, 'YYYY-MM')

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
    TO_CHAR(created_at, 'YYYY-MM') AS month,

    SUM(total_annual_cost) AS total_cost,
    SUM(total_annual_price) AS total_price,
    AVG(overall_gross_margin) AS avg_margin

FROM gma_sheets

WHERE 
    is_deleted = FALSE
    AND created_at >= CURRENT_DATE - 
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
