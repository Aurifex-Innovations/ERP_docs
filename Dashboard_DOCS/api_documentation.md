# Unified Dashboard API Documentation

This documentation covers the newly refactored Orchestrator dashboard API. The single API dynamically handles all 16 modules, avoiding payload bloat by utilizing exclusive fetching.

## 🧭 Base Endpoint

```http
GET /api/dashboard/{module_name}
```

**Headers Required**:
* `Authorization: Bearer <token>` (Handled automatically by `get_current_tenant`)

---

## 🏗️ 1. Default Request (KPIs & Charts Only)

By default, hitting a module endpoint fetches its heavily cached KPIs and Charts immediately.

**Request:**
```http
GET /api/dashboard/branch_management
```

**Response (Status: 200 OK):**
```json
{
  "dashboard": "branch_management",
  "kpis": {
    "active_branches": 15,
    "employee_branch_ratio": 4.5
  },
  "charts": {
    "branch_growth_trend": [
      { "month": "Jan", "count": 2 }
    ]
  }
}
```

---

## 📊 2. Table Request (Isolated Pagination)

If you need specific data tables for a dashboard, you pass the `?table=` query parameter. **This skips KPIs and Charts entirely to provide an instant response.**

**Request:**
```http
GET /api/dashboard/branch_management?table=recent_activations&recent_activations_page=1&recent_activations_size=10
```

**Response (Status: 200 OK):**
```json
{
  "table": {
    "id": "recent_activations",
    "data": [
      {
        "branch_name": "Downtown Hub",
        "branch_code": "DT-001",
        "created_at": "2026-04-14 10:00:00"
      }
    ],
    "pagination": {
      "page": 1,
      "size": 10,
      "total": 45,
      "total_pages": 5
    }
  }
}
```

> [!TIP]
> **Multiple Tables:** You can request multiple tables at once by comma separating them: 
> `?table=table_1,table_2`. The response will wrap them in a `"tables": [...]` array.
>
> **Global Pagination Fallback:** If you pass `?page=2&size=20`, it applies to all requested tables that don't have a specific `{table_id}_page` defined.

---

## 🗂️ Module Reference Guide

Below are the acceptable `{module_name}` path parameters and the specific `?table=` IDs available to query underneath them.

| `{module_name}` | Available `?table=` parameters |
| :--- | :--- |
| **`inventory`** | `low_stock_products`, `out_of_stock_products`, `branch_stock`, `central_stock_entries`, `recent_stock_movements`, `stock_transfers` |
| **`branch_management`** | `branch_directory`, `recent_branch_activations` |
| **`sales_order`** | `recent_orders` |
| **`vendor_management`**| `recent_vendor_additions`, `active_contract_list` |
| **`purchase`** | `recent_po`, `vendor_summary` |
| **`lead_followup`** | `recent_leads`, `upcoming_followups` |
| **`quotation`** | `high_value_quotes`, `expiring_quotes` |
| **`customer_management`** | `recent_customers`, `active_contracts` |
| **`contract_management`** | `recent_contracts`, `expiring_contracts` |
| **`customer_support`** | `recent_tickets`, `open_high_priority` |
| **`gma`** | `recent_gma`, `approved_summary` |
| **`task_management`** | `recent_tasks`, `material_usage` |
| **`employee_management`**| `critical_hiring`, `compensation_audit` |
| **`hrm`** | `employee_list`, `salary_slips` |
| **`petty_cash`** | `recent_requests`, `approved_payments` |
| **`financial`** | `revenue_summary`, `task_summary`, `customer_revenue`, `invoice_details`, `product_consumption`, `vendor_outstanding` |

---

## ⚠️ Error Responses

**404 Not Found (Invalid Module)**
```json
{
  "detail": "Dashboard 'invalid_module' is not registered."
}
```

**404 Not Found (Invalid Table requested on valid module)**
```json
{
  "detail": "None of the requested tables were found."
}
```

**401 Unauthorized** (Missing or expired Bearer Token)
```json
{
  "detail": "Could not validate credentials"
}
```
