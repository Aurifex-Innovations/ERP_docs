
# Module 15: Lead & Follow-up Performance

The Lead & Follow-up Dashboard transforms sales data into a structured system for tracking lead progression, optimizing conversions, and managing sales activities efficiently.

---

## 1. Executive Overview

- Tracks complete lead lifecycle from inquiry to conversion  
- Helps identify high-potential leads early  
- Optimizes sales funnel performance  
- Improves marketing ROI through lead source analysis  
- Prevents revenue loss due to poor follow-up  

---

## 2. Key Performance Indicators (KPIs)

| KPI Name | Business Meaning | Source Table | Aggregation Logic |
|----------|----------------|-------------|------------------|
| Total Active Leads | Total leads in pipeline | leads | COUNT(id) WHERE status NOT IN ('Lost','Converted') |
| Qualified Lead Count | High-quality leads ready for next stage | leads | COUNT(id) WHERE status = 'Qualified' |
| Pending Follow-ups | Scheduled future follow-ups | leads | COUNT(next_follow_up_date) WHERE date >= CURRENT_DATE |
| Lead Conversion Rate | Sales success rate | leads | Converted / (Converted + Lost) |

---

## 3. Strategic Visualizations (Charts)

### 1. Status Wise Leads (Funnel Chart)
- X-axis: status  
- Y-axis: COUNT(id)  
- Flow: New → Qualified → Quotation → Negotiation → Converted  
- Purpose: Identify drop-off stages
- Source Table: leads 

---

### 2. Leads by Source (Pie Chart)
- X-axis: source  
- Y-axis: COUNT(id)  
- Purpose: Analyze marketing channel effectiveness
- Source Table: leads

---

### 3. Daily Follow-up Activity (Line Chart)
- X-axis: DATE(created_at)  
- Y-axis: COUNT(id)  
- Source: follow_ups  
- Purpose: Track sales activity trends
- Source Table: follow_ups

---

### 4. Leads by Priority (Bar Chart)
- X-axis: priority (Urgent, High, Normal, Low)  
- Y-axis: COUNT(id)  
- Purpose: Ensure high-priority leads are handled
- Source Table: leads

---

## 4. Operational Data Tables

### 1. Recent Leads Table

#### Meaning
Shows latest leads for tracking and quick assignment.

### Tables Used

leads

---

### Table Headers (UI Display Names)

| Column Header | Database Field | Table |
|--------------|---------------|------|
| Lead ID | id | leads |
| Lead Name | lead_name | leads |
| Mobile Number | mobile_number | leads |
| Lead Type | lead_type | leads |
| Lead Source | source | leads |
| Priority | priority | leads |
| Lead Status | status | leads |
| Created Date | created_date | leads |


---
### 2. Upcoming Follow-ups Table

#### Meaning
Shows upcoming follow-up tasks for the sales team.

### Tables Used

leads  
follow_ups  

---
#### Table Headers
| Column Header  | Database Field      | Table      |
| -------------- | ------------------- | ---------- |
| Lead Name      | lead_name           | leads      |
| Follow-up Date | next_follow_up_date | follow_ups |
| Contact Mode   | contact_mode        | follow_ups |
| Priority       | priority            | leads      |


### Join Condition

```sql
follow_ups.lead_id = leads.id
```

## 5. Business Alerts

## 5. Critical Business Alerts

### 1. Overdue Follow-up Alert

#### Meaning
Shows leads whose follow-up date has passed and are not converted or lost.

### SQL Query

```sql
SELECT 
l.id,
l.lead_name,
f.next_follow_up_date,
l.status
FROM follow_ups f
JOIN leads l
ON f.lead_id = l.id
WHERE f.next_follow_up_date < CURRENT_DATE
AND l.status NOT IN ('Converted','Lost')
AND l.is_deleted = FALSE;
```
### Tables Used

leads  
follow_ups     

---

### 2. Urgent Lead Alert

#### Meaning
Shows urgent leads that are still new and need immediate action.

### SQL Query

```sql
SELECT 
id,
lead_name,
mobile_number,
priority,
status,
created_date
FROM leads
WHERE priority = 'Urgent'
AND status = 'New'
AND is_deleted = FALSE;
```
### Tables Used

leads  

---

### 3. Pending Lead Alert

#### Meaning
Shows leads stuck in negotiation for too long.

### SQL Query

```sql
SELECT 
id,
lead_name,
priority,
status,
updated_at
FROM leads
WHERE 
(
    priority = 'Urgent'
    AND status = 'Negotiation'
    AND CURRENT_DATE - updated_at > 2
)
OR
(
    priority = 'Normal'
    AND status = 'Negotiation'
    AND CURRENT_DATE - updated_at > 7
)
AND is_deleted = FALSE;
```

## 6. Global Filters for KPIs and Charts and tables

To provide dynamic data views, the dashboard uses three global filters. If a user selects these filters, the API dynamically applies them. If the user clears the filters or makes no selection, the original base query runs.

### 1. Global Filters Definition

1. **Date Range Filter**: Custom date selection mapping to `start_date` and `end_date`.
2. **Time Period Filter**: A preset dropdown mapping to date ranges on the backend:
   - **Current**: `CURRENT_DATE`
   - **7 Days**: `CURRENT_DATE - INTERVAL '7 days'` to `CURRENT_DATE`
   - **1 Month**: `CURRENT_DATE - INTERVAL '1 month'` to `CURRENT_DATE`
   - **3 Months**: `CURRENT_DATE - INTERVAL '3 months'` to `CURRENT_DATE`
   - **6 Months**: `CURRENT_DATE - INTERVAL '6 months'` to `CURRENT_DATE`
   - **1 Year**: `CURRENT_DATE - INTERVAL '1 year'` to `CURRENT_DATE`

### Tables Used

leads  

---
