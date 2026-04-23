# 📊 Chart: Monthly Contract Revenue

---

## 🔹 Type  
**Bar Chart (Recommended)**  
*(Alternative: Line Chart for trend view)*

---

## 🎯 Purpose  

To track **monthly contract revenue**, helping:

- Monitor confirmed business revenue  
- Identify high/low revenue months  
- Support financial planning and forecasting  

---

## 🧾 Tables Used  

- contracts  

---

## 🧩 Fields Used (Exact from Schema)

- created_at  
- total_sale_value  
- status  

---

## 🔹 Logic  

- Group data by **month (created_at)**  
- Sum **total_sale_value**  
- Include only **ACTIVE contracts** (confirmed revenue)

---

## 🔹 Query (Base)

```sql
SELECT 
    TO_CHAR(created_at, 'YYYY-MM') AS month,
    SUM(total_sale_value) AS total_revenue

FROM contracts

WHERE 
    status = 'ACTIVE'

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

#### 🔹 Query (Month Selection)
```
SELECT 
    TO_CHAR(created_at, 'YYYY-MM') AS month,
    SUM(total_sale_value) AS total_revenue

FROM contracts

WHERE 
    status = 'ACTIVE'
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
    SUM(total_sale_value) AS total_revenue

FROM contracts

WHERE 
    status = 'ACTIVE'
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
