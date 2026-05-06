# Project DB Schema
``` Aa File ne Download kari antigravity ma open karva par Preview nu option avshe tya view karva thi graph proper avshe```
Generated: 2026-05-06T10:07:07.020Z

Source migrations: 102 SQL files

Tables: 165

## Mermaid ER Diagrams (split)

Note: schema big. ER diagram split into smaller parts so Mermaid renderer can load.

### er_part_1_of_7

```mermaid
erDiagram
  actions {
    BIGSERIAL id PK
    VARCHAR name
    VARCHAR label
    VARCHAR description
  }
  asset_units {
    BIGSERIAL id PK
    VARCHAR asset_id
    VARCHAR product_id FK
    VARCHAR product_code
    VARCHAR product_name
    VARCHAR branch_id
    BIGINT assigned_user_id
    VARCHAR assigned_to_name
    VARCHAR assignment_mode
    VARCHAR condition
    VARCHAR status
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
  }
  bill_payment_allocations {
    VARCHAR id PK
    VARCHAR bill_id FK
    VARCHAR voucher_id FK
    NUMERIC allocated_amount
    VARCHAR allocation_type
    NUMERIC running_balance_after
    TIMESTAMPTZ created_at
  }
  branches {
    VARCHAR id PK
    VARCHAR branch_name
    VARCHAR branch_code
    VARCHAR email
    VARCHAR phone_number
    TEXT address_line1
    VARCHAR country
    VARCHAR state
    VARCHAR city
    VARCHAR pincode
    VARCHAR branch_type
    VARCHAR status
    VARCHAR created_by
    VARCHAR updated_by
    VARCHAR deleted_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
    TIMESTAMPTZ deleted_at
  }
  central_stock_entries {
    BIGSERIAL id PK
    VARCHAR entry_id
    VARCHAR product_id FK
    VARCHAR product_code
    VARCHAR product_name
    VARCHAR hsn_code
    VARCHAR base_uom
    INTEGER total_qty
    INTEGER assets_qty
    INTEGER consumable_qty
    INTEGER resell_qty
    VARCHAR asset_id_generation
    VARCHAR asset_id_prefix
    INTEGER asset_sequence_start
    VARCHAR assignment_type
    VARCHAR default_assignment
    VARCHAR supplier_name
    VARCHAR purchase_order_ref
    VARCHAR invoice_number
    DATE invoice_date
    NUMERIC invoice_amount
    NUMERIC tax_amount
    NUMERIC total_with_tax
    TEXT invoice_copy_url
    VARCHAR batch_number
    DATE manufacturing_date
    DATE expiry_date
    VARCHAR status
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
    VARCHAR deleted_by
    TIMESTAMPTZ deleted_at
  }
  central_stock_ledger {
    BIGSERIAL id PK
    VARCHAR product_id FK
    VARCHAR product_code
    VARCHAR product_name
    VARCHAR category
    VARCHAR brand
    VARCHAR hsn_code
    VARCHAR base_uom
    INTEGER assets_qty
    INTEGER consumable_qty
    INTEGER resell_qty
    INTEGER in_transit_qty
    INTEGER reserved_qty
    VARCHAR status
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
    VARCHAR deleted_by
    TIMESTAMPTZ deleted_at
  }
  coa_account_heads {
    VARCHAR id PK
    VARCHAR code
    VARCHAR name
    VARCHAR primary_group
    VARCHAR parent_head_id FK
    VARCHAR nature
    VARCHAR branch_scope
    VARCHAR branch_id
    BOOLEAN is_postable
    VARCHAR status
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  company_profile_extension {
    UUID id PK
    VARCHAR company_id
    TEXT logo_url
    VARCHAR tagline
    VARCHAR website
    INT founding_year
    TIMESTAMP created_at
    TIMESTAMP updated_at
  }
  contract_amendment_logs {
    VARCHAR id PK
    VARCHAR contract_id FK
    BIGINT amended_by_user_id
    VARCHAR amended_by_name
    TIMESTAMPTZ amended_at
    VARCHAR amendment_reason
    TEXT amendment_remarks
    BOOLEAN approval_required
    TEXT change_summary
  }
  contract_payment_lines {
    VARCHAR id PK
    VARCHAR contract_id FK
    VARCHAR period_label
    VARCHAR period_description
    NUMERIC amount
    DATE due_date
    BOOLEAN paid
    BOOLEAN locked
    INTEGER sort_order
  }
  contract_sales_order_links {
    VARCHAR id PK
    VARCHAR contract_id FK
    VARCHAR sales_order_number
    DATE sales_order_date
    VARCHAR period_label
    NUMERIC so_value
    VARCHAR so_status
    VARCHAR service_status
  }
  contract_site_services {
    VARCHAR id PK
    VARCHAR contract_site_id FK
    VARCHAR service_type_id
    VARCHAR service_type_name
    VARCHAR contract_mode
    VARCHAR frequency
    INTEGER annual_frequency
    VARCHAR preferred_days
    VARCHAR preferred_time_slot
    VARCHAR technician_team_id
    VARCHAR technician_team_name
    NUMERIC service_sale_value
    INTEGER display_order
  }
  contract_sites {
    VARCHAR id PK
    VARCHAR contract_id FK
    VARCHAR gma_site_id
    VARCHAR site_name
    TEXT address
    VARCHAR city
    VARCHAR state
    VARCHAR country
    TEXT google_map_url
    NUMERIC area_sqft
    VARCHAR category
    VARCHAR sub_category
    NUMERIC site_total_cost_year
    NUMERIC site_proposed_price_year
    NUMERIC site_gross_margin
    INTEGER display_order
  }
  contract_termination_logs {
    VARCHAR id PK
    VARCHAR contract_id FK
    BIGINT terminated_by_user_id
    VARCHAR terminated_by_name
    TIMESTAMPTZ terminated_at
    DATE effective_closure_date
    VARCHAR reason_code
    VARCHAR additional_remarks
    INT open_sales_orders_count
    VARCHAR open_sales_orders_resolution
  }
  contracts {
    VARCHAR id PK
    VARCHAR gma_sheet_id
    VARCHAR customer_id
    VARCHAR customer_name
    VARCHAR branch_id
    VARCHAR status
    VARCHAR duration_option
    DATE start_date
    DATE end_date
    NUMERIC total_sale_value
    NUMERIC gma_original_total_sale
    NUMERIC total_annual_cost_snapshot
    NUMERIC overall_gm_percent_snapshot
    VARCHAR contract_reference
    VARCHAR renewal_type
    TEXT legal_notes
    VARCHAR payment_schedule_type
    VARCHAR invoicing_frequency
    TEXT custom_payment_description
    DATE advance_payment_due_date
    TEXT legal_sla_remarks
    BOOLEAN variance_requires_approval
    DATE termination_effective_date
    VARCHAR termination_reason
    VARCHAR termination_remarks
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  credit_notes {
    VARCHAR id PK
    VARCHAR cn_number
    VARCHAR invoice_id FK
    DATE cn_date
    VARCHAR reason
    VARCHAR other_reason
    VARCHAR remarks
    NUMERIC credit_amount
    VARCHAR source
    VARCHAR status
    TIMESTAMPTZ created_at
    VARCHAR created_by
  }
  customer_audit_log {
    BIGSERIAL id PK
    VARCHAR customer_id
    VARCHAR field_name
    TEXT old_value
    TEXT new_value
    VARCHAR changed_by
    TIMESTAMP_WITH_TIME_ZONE changed_at
  }
  customers {
    VARCHAR id PK
    VARCHAR entry_mode
    VARCHAR lead_id
    VARCHAR customer_type
    VARCHAR full_name
    VARCHAR industry_type
    VARCHAR pan_number
    VARCHAR gst_number
    VARCHAR contact_person
    VARCHAR designation
    VARCHAR phone
    VARCHAR alternate_phone
    VARCHAR email
    VARCHAR branch_id
    TEXT billing_address_line1
    TEXT billing_address_line2
    VARCHAR city
    VARCHAR state
    VARCHAR pincode
    VARCHAR country
    TEXT google_map_url
    VARCHAR finance_contact_name
    VARCHAR finance_contact_phone
    VARCHAR finance_contact_email
    VARCHAR status
    TEXT reason_for_deactivation
    BOOLEAN is_deleted
    TIMESTAMP_WITH_TIME_ZONE created_at
    TIMESTAMP_WITH_TIME_ZONE updated_at
    VARCHAR created_by
    VARCHAR updated_by
  }
  debit_notes {
    VARCHAR id PK
    VARCHAR dn_number
    VARCHAR bill_id FK
    DATE dn_date
    VARCHAR reason
    VARCHAR other_reason
    VARCHAR remarks
    NUMERIC debit_amount
    VARCHAR source
    VARCHAR status
    TIMESTAMPTZ created_at
    VARCHAR created_by
  }
  follow_ups {
    VARCHAR id PK
    VARCHAR lead_id FK
    TEXT interaction_summary
    VARCHAR status_updated_to
    VARCHAR contact_mode
    TEXT lost_reason
    BOOLEAN next_action_scheduled
    DATE next_follow_up_date
    TIME next_follow_up_time
    TEXT reason_agenda
    VARCHAR created_by
    TIMESTAMP created_at
  }
  gma_audit_logs {
    VARCHAR id PK
    VARCHAR gma_sheet_id FK
    VARCHAR action
    VARCHAR user_id
    VARCHAR user_name
    TEXT remarks
    TIMESTAMPTZ action_at
  }
  gma_chemicals {
    VARCHAR id PK
    VARCHAR gma_service_id FK
    VARCHAR product_id FK
    VARCHAR product_code
    VARCHAR product_name
    VARCHAR uom
    NUMERIC coverage_sqft
    NUMERIC required_qty_per_visit
    NUMERIC price_per_uom
    NUMERIC cost_per_visit
    NUMERIC cost_per_month
    NUMERIC cost_per_year
    INTEGER display_order
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  gma_prospects {
    VARCHAR id PK
    VARCHAR full_name
    VARCHAR phone
    VARCHAR email
    VARCHAR company_name
    TEXT address
    VARCHAR city
    VARCHAR state
    VARCHAR pincode
    VARCHAR country
    TEXT google_map_url
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  gma_services {
    VARCHAR id PK
    VARCHAR gma_site_id FK
    VARCHAR service_type_id FK
    VARCHAR service_type_name
    VARCHAR service_mode
    VARCHAR frequency
    INTEGER annual_frequency
    NUMERIC visits_per_month
    NUMERIC rate_per_visit
    NUMERIC service_visit_cost_year
    NUMERIC service_visit_cost_month
    NUMERIC hours_per_visit
    NUMERIC rate_per_hour
    NUMERIC manpower_cost_year
    NUMERIC manpower_cost_month
    NUMERIC chemical_cost_year
    NUMERIC chemical_cost_month
    NUMERIC total_service_cost_year
    INTEGER display_order
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  gma_sheet_approver_roles {
    VARCHAR gma_sheet_id PK
    BIGINT role_id PK
  }
  coa_account_heads ||--o{ coa_account_heads : "fk_coa_parent"
  contracts ||--o{ contract_amendment_logs : "fk_cal_contract"
  contracts ||--o{ contract_payment_lines : "fk_cpl_contract"
  contracts ||--o{ contract_sales_order_links : "fk_csol_contract"
  contract_sites ||--o{ contract_site_services : "fk_css_site"
  contracts ||--o{ contract_sites : "fk_cs_contract"
  contracts ||--o{ contract_termination_logs : "fk_ctl_contract"
  gma_services ||--o{ gma_chemicals : "fk_gmachem_service"
```

### er_part_2_of_7

```mermaid
erDiagram
  gma_sheets {
    VARCHAR id PK
    VARCHAR source_type
    VARCHAR lead_id FK
    VARCHAR customer_id
    VARCHAR prospect_id FK
    VARCHAR contract_duration
    DATE proposed_start_date
    VARCHAR branch_id FK
    TEXT remarks
    NUMERIC total_annual_cost
    NUMERIC total_annual_price
    NUMERIC overall_gross_margin
    NUMERIC gm_without_doc
    NUMERIC gm_with_doc
    NUMERIC total_surcharge_cost
    NUMERIC total_visits_per_month
    VARCHAR status
    BIGINT approver_id
    TEXT approval_remarks
    TIMESTAMPTZ submitted_on
    TIMESTAMPTZ approved_on
    TIMESTAMPTZ deadline
    BOOLEAN is_deleted
    TIMESTAMPTZ deleted_at
    VARCHAR deleted_by
    BIGINT prepared_by_id
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  gma_sites {
    VARCHAR id PK
    VARCHAR gma_sheet_id FK
    VARCHAR site_name
    TEXT address
    VARCHAR city
    VARCHAR state
    VARCHAR country
    TEXT google_map_url
    VARCHAR category
    VARCHAR sub_category
    NUMERIC area_sqft
    BOOLEAN weekend_night_surcharge_applicable
    NUMERIC surcharge_cost
    BOOLEAN documentation_cost_applicable
    NUMERIC cost_per_document
    INTEGER docs_per_month
    NUMERIC documentation_cost_year
    NUMERIC site_total_cost_year
    NUMERIC site_proposed_price_year
    NUMERIC site_gross_margin
    INTEGER display_order
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  hiring_request_branches {
    VARCHAR hiring_request_id PK
    VARCHAR branch_id PK
  }
  hiring_request_recipients {
    VARCHAR hiring_request_id PK
    BIGINT recipient_user_id PK
  }
  hiring_requests {
    VARCHAR id PK
    BIGINT requested_by_user_id FK
    VARCHAR department
    VARCHAR designation
    BIGINT proposed_role_id FK
    VARCHAR employment_type
    DATE expected_date_of_joining
    INT number_of_positions
    TEXT hiring_reason
    TEXT job_description
    VARCHAR additional_remarks
    DECIMAL proposed_salary
    VARCHAR supporting_document_path
    VARCHAR status
    BIGINT reviewed_by_user_id FK
    DATE review_date
    TEXT rejection_reason
    BIGINT converted_user_id FK
    TIMESTAMPTZ submitted_at
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
  }
  hrm_attendance_day {
    BIGSERIAL id PK
    BIGINT user_id FK
    DATE attendance_date
    TIMESTAMPTZ punch_in_at
    TIMESTAMPTZ punch_out_at
    INT total_minutes
    VARCHAR status
    VARCHAR source
    VARCHAR notes
    INT tasks_assigned
    INT tasks_completed
    INT tasks_pending
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
  }
  hrm_holidays {
    BIGSERIAL id PK
    DATE holiday_date
    VARCHAR name
    VARCHAR branch_id
    VARCHAR created_by
    TIMESTAMPTZ created_at
  }
  hrm_leave_request {
    BIGSERIAL id PK
    VARCHAR leave_code
    BIGINT user_id FK
    VARCHAR leave_type
    DATE from_date
    DATE to_date
    INT working_days
    TEXT description
    VARCHAR status
    TEXT rejection_reason
    BIGINT reviewed_by_user_id FK
    TIMESTAMPTZ reviewed_at
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
  }
  hrm_salary_month {
    BIGSERIAL id PK
    BIGINT user_id FK
    INT salary_year
    INT salary_month
    NUMERIC basic_salary
    NUMERIC hra
    NUMERIC other_allowance
    NUMERIC incentive
    NUMERIC deductions
    NUMERIC other_deductions
    NUMERIC pf
    NUMERIC esi
    NUMERIC tds
    NUMERIC ot_hours
    INT holiday_days_worked
    NUMERIC ot_amount
    NUMERIC holiday_incentive_amt
    NUMERIC gross_salary
    NUMERIC total_deductions
    NUMERIC net_salary
    VARCHAR payment_status
    TEXT reason
    DATE payment_date
    BIGINT paid_by_user_id FK
    TIMESTAMPTZ paid_at
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
  }
  hrm_salary_slip {
    BIGSERIAL id PK
    BIGINT salary_month_id FK
    VARCHAR file_path
    VARCHAR generated_by
    TIMESTAMPTZ generated_at
  }
  hsn_code_tax_types {
    BIGINT hsn_code_id FK
    BIGINT tax_type_id FK
  }
  hsn_codes {
    BIGINT id PK
    VARCHAR hsn_code
    TEXT description
    VARCHAR chapter
    VARCHAR product_category
    VARCHAR product_subcategory
    DATE effective_from
    VARCHAR status
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
    VARCHAR deleted_by
    TIMESTAMPTZ deleted_at
  }
  incentive_overtime_details {
    VARCHAR config_id PK
    BOOLEAN holiday_work_applicable
    VARCHAR holiday_work_type
    NUMERIC holiday_work_amount
    BOOLEAN overtime_applicable
    VARCHAR overtime_type
    VARCHAR overtime_shift_type
    TIME custom_shift_from
    TIME custom_shift_to
    NUMERIC overtime_shift_incentive
    NUMERIC per_hour_incentive_pay
    INT max_ot_hours_per_month
  }
  inventory_brands {
    VARCHAR id PK
    VARCHAR name
    BOOLEAN is_active
    VARCHAR created_by
    TIMESTAMPTZ created_at
  }
  inventory_product_media {
    VARCHAR id PK
    VARCHAR product_id
    VARCHAR file_name
    VARCHAR content_type
    TEXT file_url
    TEXT file_data
    BOOLEAN is_primary
    VARCHAR created_by
    TIMESTAMP created_at
  }
  inventory_products {
    VARCHAR id PK
    VARCHAR product_name
    VARCHAR product_code
    VARCHAR category
    VARCHAR sub_type
    VARCHAR brand
    TEXT description
    VARCHAR status
    VARCHAR hsn_code
    VARCHAR base_uom
    VARCHAR unit_packaging_brand
    VARCHAR secondary_uom
    VARCHAR package_type
    DOUBLE_PRECISION quantity_per_package
    DOUBLE_PRECISION units_per_package
    VARCHAR variant_name
    VARCHAR variant_sku
    VARCHAR variant_package_type
    DOUBLE_PRECISION variant_quantity
    VARCHAR barcode
    VARCHAR variant_status
    DOUBLE_PRECISION purchase_price
    DOUBLE_PRECISION selling_price
    DOUBLE_PRECISION base_price
    DOUBLE_PRECISION tax_amount
    DOUBLE_PRECISION total_cost
    VARCHAR created_by
    VARCHAR updated_by
    VARCHAR deleted_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
    TIMESTAMPTZ deleted_at
  }
  invoice_payment_allocations {
    VARCHAR id PK
    VARCHAR invoice_id FK
    VARCHAR voucher_id FK
    NUMERIC allocated_amount
    VARCHAR allocation_type
    NUMERIC running_balance_after
    TIMESTAMPTZ created_at
  }
  lead_audit_logs {
    VARCHAR id PK
    VARCHAR lead_id FK
    VARCHAR field_changed
    TEXT old_value
    TEXT new_value
    VARCHAR changed_by
    TIMESTAMP changed_at
  }
  leads {
    VARCHAR id PK
    DATE lead_date
    VARCHAR source
    VARCHAR branch_id
    VARCHAR priority
    BIGINT assigned_to_id
    VARCHAR lead_name
    VARCHAR mobile_number
    VARCHAR alternate_number
    VARCHAR email_id
    VARCHAR lead_type
    VARCHAR service_type
    VARCHAR budget_range
    TEXT lead_description
    VARCHAR status
    VARCHAR gma_status
    TEXT lost_reason
    DATE next_follow_up_date
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMP created_at
    TIMESTAMP updated_at
  }
  leave_configuration {
    VARCHAR config_id PK
    INT casual_leave
    INT sick_leave
    INT paid_leave
    INT annual_leave_allocation
    BOOLEAN carry_forward_allowed
    INT max_carry_forward_days
    BIGINT leave_approval_role_id FK
    VARCHAR leave_reset_cycle
    DATE leave_reset_from
    DATE leave_reset_to
  }
  ledger_entries {
    VARCHAR id PK
    VARCHAR voucher_no
    DATE entry_date
    VARCHAR branch_id
    VARCHAR ledger_id FK
    NUMERIC dr_amount
    NUMERIC cr_amount
    VARCHAR ref_type
    VARCHAR ref_id
    VARCHAR narration
    VARCHAR posting_status
    TIMESTAMPTZ created_at
    VARCHAR created_by
  }
  ledgers {
    VARCHAR id PK
    VARCHAR ledger_code
    VARCHAR ledger_name
    VARCHAR account_head_id FK
    VARCHAR ledger_type
    VARCHAR linked_customer_id
    VARCHAR linked_vendor_id
    VARCHAR branch_id
    NUMERIC opening_balance
    VARCHAR opening_balance_type
    DATE opening_as_on
    NUMERIC credit_limit
    INTEGER credit_period_days
    BOOLEAN tds_applicable
    VARCHAR tds_section
    VARCHAR status
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  modules {
    BIGSERIAL id PK
    VARCHAR name
    VARCHAR label
    VARCHAR description
  }
  notification_recipients {
    BIGSERIAL id PK
    BIGINT notification_id FK
    BIGINT user_id FK
    TIMESTAMPTZ delivered_at
    TIMESTAMPTZ read_at
  }
  notifications {
    BIGSERIAL id PK
    INTEGER module_no
    VARCHAR event_type
    VARCHAR entity_id
    VARCHAR title
    TEXT message
    VARCHAR priority
    VARCHAR action_url
    VARCHAR created_by
    TIMESTAMPTZ created_at
  }
  leads ||--o{ gma_sheets : "fk_gma_lead"
  gma_sheets ||--o{ gma_sites : "fk_gmasite_sheet"
  hiring_requests ||--o{ hiring_request_branches : "fk_hrb_hiring_request"
  hiring_requests ||--o{ hiring_request_recipients : "fk_hrr_hiring_request"
  hrm_salary_month ||--o{ hrm_salary_slip : "fk_hrm_salary_slip_month"
  hsn_codes ||--o{ hsn_code_tax_types : "fk_hsn_code"
  inventory_brands ||--o{ inventory_products : "fk_inventory_products_brand"
  leads ||--o{ lead_audit_logs : "fk_audit_lead"
  ledgers ||--o{ ledger_entries : "fk_le_ledger"
  notifications ||--o{ notification_recipients : "fk_nr_notification"
```

### er_part_3_of_7

```mermaid
erDiagram
  observation_options_hygiene {
    VARCHAR id PK
    VARCHAR label
    INTEGER display_order
    BOOLEAN is_active
    TIMESTAMPTZ created_at
  }
  observation_options_pest_sighting {
    VARCHAR id PK
    VARCHAR label
    INTEGER display_order
    BOOLEAN is_active
    TIMESTAMPTZ created_at
  }
  observation_options_structural {
    VARCHAR id PK
    VARCHAR label
    INTEGER display_order
    BOOLEAN is_active
    TIMESTAMPTZ created_at
  }
  petty_cash_attachments {
    VARCHAR id PK
    VARCHAR request_id FK
    VARCHAR attachment_type
    VARCHAR file_key
    VARCHAR file_name
    VARCHAR content_type
    BIGINT file_size_bytes
    VARCHAR notes
    VARCHAR uploaded_by
    TIMESTAMPTZ uploaded_at
  }
  petty_cash_audit_logs {
    VARCHAR id PK
    VARCHAR request_id FK
    VARCHAR action
    BIGINT actor_user_id FK
    VARCHAR actor_name
    VARCHAR remarks
    TIMESTAMPTZ action_at
  }
  petty_cash_request_recipient_roles {
    VARCHAR request_id PK
    BIGINT recipient_role_id PK
  }
  petty_cash_request_recipients {
    VARCHAR id PK
    VARCHAR request_id FK
    BIGINT recipient_user_id FK
    BIGINT recipient_role_id FK
    TIMESTAMPTZ created_at
  }
  petty_cash_requests {
    VARCHAR id PK
    BIGINT requester_user_id FK
    VARCHAR requester_branch_id FK
    VARCHAR category
    DATE expense_date_from
    DATE expense_date_to
    NUMERIC amount_requested
    VARCHAR description
    VARCHAR related_task_ref
    VARCHAR related_so_ref
    VARCHAR justification_note
    VARCHAR payment_mode_requested
    VARCHAR bank_account_holder
    VARCHAR bank_name
    VARCHAR bank_account_number
    VARCHAR bank_ifsc
    VARCHAR upi_id
    BOOLEAN is_pre_approved
    BIGINT pre_approved_by_user_id FK
    VARCHAR approval_reference
    VARCHAR status
    TIMESTAMPTZ submitted_at
    VARCHAR submitted_to_label
    BIGINT reviewed_by_user_id FK
    TIMESTAMPTZ reviewed_at
    NUMERIC approved_amount
    VARCHAR reviewer_remarks
    VARCHAR rejection_reason
    VARCHAR correction_notes
    VARCHAR payment_mode_processed
    VARCHAR transaction_ref
    DATE payment_date
    VARCHAR finance_remarks
    BIGINT paid_by_user_id FK
    TIMESTAMPTZ paid_at
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  public_actions {
    BIGSERIAL id PK
    VARCHAR name
    VARCHAR label
    VARCHAR description
  }
  public_company_details {
    BIGSERIAL id PK
    VARCHAR company_code
    VARCHAR company_name
    VARCHAR industry_type
    VARCHAR contact_person_name
    VARCHAR contact_person_email
    VARCHAR contact_person_phone
    VARCHAR gst_number
    VARCHAR pan_number
    VARCHAR address_line_1
    VARCHAR address_line_2
    VARCHAR city
    VARCHAR state
    VARCHAR pincode
    VARCHAR license_number
    VARCHAR onboarding_status
    TEXT rejection_reason
    BOOLEAN is_active
    TIMESTAMPTZ submitted_at
    TIMESTAMPTZ verified_at
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
    BIGINT created_by
    BIGINT updated_by
    BIGINT verified_by
  }
  public_company_documents {
    BIGSERIAL id PK
    BIGINT company_id FK
    VARCHAR document_type
    VARCHAR document_name
    VARCHAR document_url
    VARCHAR file_content_type
    BIGINT file_size_bytes
    BOOLEAN verified
    TIMESTAMPTZ reviewed_at
    BIGINT reviewed_by
    TIMESTAMPTZ uploaded_at
    TIMESTAMPTZ updated_at
  }
  public_company_subscription {
    BIGSERIAL id PK
    VARCHAR subscription_id
    BIGINT company_id FK
    VARCHAR subscription_plan_id FK
    VARCHAR plan_type
    VARCHAR duration_type
    INTEGER branch_count
    INTEGER technician_count
    NUMERIC price_per_branch
    NUMERIC price_per_technician
    NUMERIC branch_cost
    NUMERIC technician_cost
    NUMERIC subtotal
    NUMERIC gst_percentage
    NUMERIC gst_amount
    NUMERIC final_total
    DATE start_date
    DATE end_date
    DATE purchase_date
    VARCHAR status
    VARCHAR razorpay_order_id
    VARCHAR razorpay_payment_id
    VARCHAR razorpay_signature
    VARCHAR payment_method
    BOOLEAN auto_renew
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  public_email_verification_tokens {
    BIGSERIAL id PK
    VARCHAR token
    BIGINT global_user_id FK
    TIMESTAMP_WITH_TIME_ZONE expiration_date
  }
  public_global_users {
    BIGSERIAL id PK
    VARCHAR email
    VARCHAR username
    VARCHAR password_hash
    VARCHAR full_name
    VARCHAR phone_number
    VARCHAR company_name
    VARCHAR target_schema
    BOOLEAN has_schema
    VARCHAR system_role
    BOOLEAN is_active
    TIMESTAMPTZ last_login_at
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  public_modules {
    BIGSERIAL id PK
    VARCHAR name
    VARCHAR label
    VARCHAR description
  }
  public_role_permissions {
    BIGSERIAL id PK
    BIGINT role_id FK
    BIGINT module_id FK
    BIGINT action_id FK
    BOOLEAN allowed
    BIGINT receiver_role_ids
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  public_roles {
    BIGSERIAL id PK
    VARCHAR name
    VARCHAR description
    BOOLEAN is_app_user
    VARCHAR status
    VARCHAR created_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  public_root_user {
    BIGSERIAL id PK
    VARCHAR username
    VARCHAR email
    VARCHAR password
    VARCHAR role
    BOOLEAN is_active
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  public_tenant_registry {
    BIGSERIAL id PK
    VARCHAR tenant_name
    VARCHAR schema_name
    VARCHAR display_name
    VARCHAR status
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  public_token_blacklist {
    BIGINT id PK
    VARCHAR token
    TIMESTAMP_WITH_TIME_ZONE expires_at
    TIMESTAMP_WITH_TIME_ZONE created_at
  }
  purchase_bill_attachments {
    VARCHAR id PK
    VARCHAR bill_id FK
    VARCHAR attachment_type
    VARCHAR file_key
    VARCHAR file_name
    VARCHAR content_type
    BIGINT file_size_bytes
    TIMESTAMPTZ uploaded_at
    VARCHAR uploaded_by
  }
  purchase_bill_audit_logs {
    VARCHAR id PK
    VARCHAR bill_id FK
    VARCHAR action
    VARCHAR remarks
    VARCHAR performed_by
    TIMESTAMPTZ performed_at
  }
  purchase_bill_lines {
    VARCHAR id PK
    VARCHAR bill_id FK
    INTEGER line_no
    VARCHAR item_type
    VARCHAR item_id
    VARCHAR description
    VARCHAR hsn_sac
    NUMERIC qty
    VARCHAR uom
    NUMERIC rate
    NUMERIC discount_pct
    NUMERIC tax_pct
    NUMERIC taxable_amount
    NUMERIC tax_amount
    NUMERIC line_total
  }
  purchase_bills {
    VARCHAR id PK
    VARCHAR bill_number
    VARCHAR vendor_bill_number
    VARCHAR bill_type
    VARCHAR status
    DATE bill_date
    INTEGER credit_period_days
    DATE due_date
    VARCHAR branch_id
    VARCHAR vendor_id
    VARCHAR purchase_order_id
    VARCHAR grn_reference
    VARCHAR vendor_name_snapshot
    VARCHAR vendor_gstin_snapshot
    VARCHAR vendor_state_snapshot
    VARCHAR tds_section_snapshot
    NUMERIC tds_rate_snapshot
    NUMERIC sub_total
    NUMERIC discount_total
    NUMERIC taxable_amount
    NUMERIC cgst_amount
    NUMERIC sgst_amount
    NUMERIC igst_amount
    NUMERIC tds_amount
    NUMERIC net_payable
    NUMERIC paid_amount
    NUMERIC pending_amount
    VARCHAR expense_category
    VARCHAR coa_expense_ledger_id
    TEXT internal_remarks
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  purchase_order {
    VARCHAR id PK
    VARCHAR po_number
    DATE po_date
    VARCHAR status
    VARCHAR gst_number
    VARCHAR vendor_id FK
    VARCHAR vendor_name
    TEXT vendor_address
    VARCHAR vendor_gst
    TEXT delivery_address
    VARCHAR contact_person
    VARCHAR contact_number
    VARCHAR authorized_person
    VARCHAR designation
    TEXT note
    DATE delivery_date
    INT items_count
    NUMERIC subtotal
    NUMERIC total_tax
    NUMERIC grand_total
    VARCHAR branch_name
    BOOLEAN is_deleted
    VARCHAR created_by
    VARCHAR updated_by
    VARCHAR deleted_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
    TIMESTAMPTZ deleted_at
    VARCHAR company_gst_number
    VARCHAR branch_id FK
  }
  petty_cash_requests ||--o{ petty_cash_attachments : "fk_pc_att_request"
  petty_cash_requests ||--o{ petty_cash_audit_logs : "fk_pc_aud_request"
  petty_cash_requests ||--o{ petty_cash_request_recipient_roles : "fk_pc_rr_request"
  petty_cash_requests ||--o{ petty_cash_request_recipients : "fk_pc_rec_request"
  public_company_details ||--o{ public_company_subscription : "fk_company_subscription_company"
  public_global_users ||--o{ public_email_verification_tokens : "fk_evt_global_user"
  public_roles ||--o{ public_role_permissions : "fk_rp_role"
  public_modules ||--o{ public_role_permissions : "fk_rp_module"
  public_actions ||--o{ public_role_permissions : "fk_rp_action"
  public_company_details ||--o{ public_tenant_registry : "fk_tenant_registry_company"
  purchase_bills ||--o{ purchase_bill_attachments : "fk_pba_bill"
  purchase_bills ||--o{ purchase_bill_audit_logs : "fk_pbal_bill"
  purchase_bills ||--o{ purchase_bill_lines : "fk_pbl_bill"
```

### er_part_4_of_7

```mermaid
erDiagram
  purchase_order_item {
    VARCHAR id PK
    VARCHAR purchase_order_id FK
    VARCHAR product_id FK
    VARCHAR product_name
    NUMERIC quantity
    VARCHAR uom
    NUMERIC price
    NUMERIC gst_percent
    NUMERIC tax_amount
    NUMERIC total_amount
    VARCHAR created_by
    VARCHAR updated_by
    VARCHAR deleted_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
    TIMESTAMPTZ deleted_at
    INT line_number
    BOOLEAN is_deleted
  }
  quotation_attachments {
    VARCHAR id PK
    VARCHAR quotation_id FK
    TEXT file_key
    VARCHAR file_name
    VARCHAR content_type
    BIGINT file_size_bytes
    TEXT notes
    VARCHAR uploaded_by
    TIMESTAMPTZ uploaded_at
  }
  quotation_audit_logs {
    VARCHAR id PK
    VARCHAR quotation_id FK
    VARCHAR event_type
    VARCHAR field_changed
    TEXT old_value
    TEXT new_value
    TEXT notes
    VARCHAR changed_by
    TIMESTAMPTZ changed_at
  }
  quotation_locations {
    VARCHAR id PK
    VARCHAR quotation_id FK
    INTEGER display_order
    TEXT address
    VARCHAR city
    VARCHAR state
    VARCHAR country
    VARCHAR pincode
    TEXT google_map_url
    VARCHAR location_category
    VARCHAR location_sub_category
    NUMERIC area_sqft
    VARCHAR branch_id FK
    NUMERIC location_service_subtotal
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  quotation_product_lines {
    VARCHAR id PK
    VARCHAR quotation_id FK
    VARCHAR product_id FK
    VARCHAR product_code
    VARCHAR product_name
    VARCHAR uom
    VARCHAR hsn_code
    NUMERIC unit_price
    NUMERIC quantity
    NUMERIC line_subtotal
    VARCHAR tax_type
    NUMERIC cgst_rate
    NUMERIC sgst_rate
    NUMERIC igst_rate
    NUMERIC cgst_amount
    NUMERIC sgst_amount
    NUMERIC igst_amount
    NUMERIC tax_amount
    NUMERIC line_total
    INTEGER display_order
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  quotation_prospects {
    VARCHAR id PK
    VARCHAR full_name
    VARCHAR phone
    VARCHAR email
    VARCHAR company_name
    TEXT address
    VARCHAR city
    VARCHAR state
    VARCHAR pincode
    VARCHAR country
    TEXT google_map_url
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  quotation_service_lines {
    VARCHAR id PK
    VARCHAR quotation_id FK
    VARCHAR quotation_location_id FK
    VARCHAR service_id FK
    VARCHAR service_code
    VARCHAR service_name
    VARCHAR price_type
    VARCHAR fixed_tier_name
    NUMERIC base_price
    NUMERIC price_per_sqft
    NUMERIC area_sqft_used
    NUMERIC rate_per_visit
    VARCHAR visit_frequency
    INTEGER total_visits
    NUMERIC line_total
    INTEGER display_order
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  quotations {
    VARCHAR id PK
    VARCHAR quotation_number
    VARCHAR source_type
    VARCHAR lead_id FK
    VARCHAR customer_id
    VARCHAR prospect_id FK
    VARCHAR quotation_type
    VARCHAR service_mode
    VARCHAR contract_frequency
    VARCHAR contract_duration
    DATE contract_proposed_start
    NUMERIC services_subtotal
    NUMERIC products_subtotal
    NUMERIC subtotal_before_tax
    NUMERIC tax_total
    NUMERIC total_before_discount
    VARCHAR discount_type
    NUMERIC discount_value
    NUMERIC discount_amount
    NUMERIC grand_total
    DATE valid_till
    VARCHAR payment_terms
    TEXT custom_payment_terms
    TEXT special_terms
    TEXT internal_notes
    VARCHAR status
    TIMESTAMPTZ sent_at
    TIMESTAMPTZ viewed_at
    TIMESTAMPTZ accepted_at
    TIMESTAMPTZ rejected_at
    TIMESTAMPTZ expired_at
    BOOLEAN is_deleted
    TIMESTAMPTZ deleted_at
    VARCHAR deleted_by
    VARCHAR deletion_reason
    TEXT deletion_reason_detail
    VARCHAR revised_from_id FK
    VARCHAR contract_id
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  role_compensation_configuration {
    VARCHAR config_id PK
    BIGINT role_id FK
    DATE effective_from
    DATE effective_to
    VARCHAR status
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
  }
  role_permissions {
    BIGSERIAL id PK
    BIGINT role_id FK
    BIGINT module_id FK
    BIGINT action_id FK
    BOOLEAN allowed
    BIGINT receiver_role_ids
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  roles {
    BIGSERIAL id PK
    VARCHAR name
    VARCHAR description
    BOOLEAN is_app_user
    VARCHAR status
    VARCHAR created_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  salary_details {
    VARCHAR config_id PK
    VARCHAR salary_type
    NUMERIC basic_salary
    NUMERIC hra
    NUMERIC other_allowance
    NUMERIC incentive
    NUMERIC deductions
    BOOLEAN pf_applicable
    BOOLEAN esi_applicable
    BOOLEAN tds_applicable
    DATE salary_effective_from
    DATE salary_effective_to
  }
  sales_invoice_attachments {
    VARCHAR id PK
    VARCHAR invoice_id FK
    VARCHAR file_key
    VARCHAR file_name
    VARCHAR content_type
    BIGINT file_size_bytes
    TIMESTAMPTZ uploaded_at
    VARCHAR uploaded_by
  }
  sales_invoice_audit_logs {
    VARCHAR id PK
    VARCHAR invoice_id FK
    VARCHAR action
    VARCHAR remarks
    VARCHAR performed_by
    TIMESTAMPTZ performed_at
  }
  sales_invoice_lines {
    VARCHAR id PK
    VARCHAR invoice_id FK
    INTEGER line_no
    VARCHAR item_type
    VARCHAR item_id
    VARCHAR description
    VARCHAR hsn_sac
    NUMERIC qty
    VARCHAR uom
    NUMERIC rate
    NUMERIC discount_pct
    NUMERIC tax_pct
    NUMERIC taxable_amount
    NUMERIC tax_amount
    NUMERIC line_total
    VARCHAR price_type
    NUMERIC area_sqft
    TEXT pricing_config_json
  }
  sales_invoices {
    VARCHAR id PK
    VARCHAR invoice_number
    VARCHAR invoice_type
    VARCHAR creation_mode
    VARCHAR status
    DATE invoice_date
    INTEGER credit_period_days
    DATE due_date
    VARCHAR branch_id
    VARCHAR customer_id
    VARCHAR sales_order_id
    VARCHAR contract_id
    VARCHAR customer_name_snapshot
    VARCHAR customer_gstin_snapshot
    TEXT billing_address_snapshot
    VARCHAR customer_state_snapshot
    VARCHAR contact_person_snapshot
    NUMERIC sub_total
    NUMERIC discount_total
    NUMERIC taxable_amount
    NUMERIC cgst_amount
    NUMERIC sgst_amount
    NUMERIC igst_amount
    NUMERIC round_off_amount
    NUMERIC grand_total
    NUMERIC received_amount
    NUMERIC pending_amount
    BOOLEAN einvoice_required
    VARCHAR irn_number
    VARCHAR irn_status
    TEXT irn_payload_json
    TEXT notes
    TEXT internal_remarks
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  sales_order_cancellation_logs {
    VARCHAR id PK
    VARCHAR sales_order_id FK
    VARCHAR so_number
    VARCHAR cancel_reason
    VARCHAR remarks
    VARCHAR cancelled_by
    TIMESTAMPTZ cancelled_at
  }
  sales_order_product_lines {
    VARCHAR id PK
    VARCHAR sales_order_id FK
    VARCHAR product_id
    VARCHAR product_name
    VARCHAR product_code
    VARCHAR uom
    NUMERIC quantity
    NUMERIC unit_price
    VARCHAR hsn_code
    NUMERIC tax_percent
    NUMERIC tax_amount
    NUMERIC line_total
    INTEGER display_order
  }
  sales_order_site_chemicals {
    VARCHAR id PK
    VARCHAR sales_order_site_id FK
    VARCHAR product_id
    VARCHAR product_name
    VARCHAR product_code
    VARCHAR uom
    NUMERIC coverage_sqft
    VARCHAR required_qty
    NUMERIC unit_price
    NUMERIC line_cost
    VARCHAR hsn_code
    INTEGER display_order
  }
  sales_order_site_services {
    VARCHAR id PK
    VARCHAR sales_order_site_id FK
    VARCHAR service_type_id
    VARCHAR service_type_name
    NUMERIC visits
    NUMERIC unit_price
    NUMERIC sqft
    VARCHAR hsn_code
    NUMERIC tax_percent
    NUMERIC tax_amount
    NUMERIC line_total
    INTEGER display_order
  }
  sales_order_sites {
    VARCHAR id PK
    VARCHAR sales_order_id FK
    VARCHAR contract_site_id
    VARCHAR site_name
    TEXT address
    VARCHAR city
    VARCHAR state
    VARCHAR country
    TEXT google_map_url
    VARCHAR category
    VARCHAR sub_category
    NUMERIC area_sqft
    VARCHAR contact_person
    VARCHAR contact_mobile
    INTEGER display_order
  }
  sales_orders {
    VARCHAR id PK
    VARCHAR so_number
    VARCHAR order_type
    VARCHAR status
    VARCHAR customer_id
    VARCHAR customer_name
    VARCHAR branch_id
    VARCHAR contract_id
    VARCHAR contract_payment_line_id
    VARCHAR billing_period_label
    VARCHAR gma_sheet_id
    VARCHAR quotation_id
    VARCHAR one_time_source
    DATE so_date
    NUMERIC sub_total
    VARCHAR discount_type
    NUMERIC discount_value
    NUMERIC discount_amount
    NUMERIC tax_total
    NUMERIC grand_total
    TEXT execution_notes
    VARCHAR delivery_address_type
    VARCHAR delivery_site_id
    VARCHAR delivery_address_line1
    VARCHAR delivery_address_line2
    VARCHAR delivery_city
    VARCHAR delivery_state
    VARCHAR delivery_pincode
    VARCHAR delivery_country
    TEXT delivery_google_map_url
    VARCHAR priority
    DATE expected_delivery_date
    BOOLEAN invoice_linked
    INTEGER job_cards_count
    INTEGER challans_count
    VARCHAR cancel_reason
    VARCHAR cancel_remarks
    TIMESTAMPTZ cancelled_at
    VARCHAR cancelled_by
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  service_audit_logs {
    VARCHAR id PK
    VARCHAR service_id FK
    VARCHAR change_type
    TEXT notes
    VARCHAR changed_by
    TIMESTAMPTZ created_at
  }
  service_categories {
    VARCHAR id PK
    VARCHAR name
    BOOLEAN is_active
    INTEGER display_order
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  service_category_area {
    VARCHAR id PK
    VARCHAR service_category_id FK
    VARCHAR service_sub_category_id FK
    DOUBLE_PRECISION base_price
    DOUBLE_PRECISION price_per_sqft
    DOUBLE_PRECISION sqft_increment
    BOOLEAN is_active
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  quotations ||--o{ quotation_attachments : "fk_qa_quotation"
  quotations ||--o{ quotation_audit_logs : "fk_qal_quotation"
  quotations ||--o{ quotation_locations : "fk_ql_quotation"
  quotations ||--o{ quotation_product_lines : "fk_qpl_quotation"
  quotations ||--o{ quotation_service_lines : "fk_qsl_quotation"
  quotation_locations ||--o{ quotation_service_lines : "fk_qsl_location"
  quotation_prospects ||--o{ quotations : "fk_quot_prospect"
  quotations ||--o{ quotations : "fk_quot_revised_from"
  roles ||--o{ role_compensation_configuration : "fk_rcc_role"
  roles ||--o{ role_permissions : "fk_rp_role"
  role_compensation_configuration ||--o{ salary_details : "fk_sd_config"
  sales_invoices ||--o{ sales_invoice_attachments : "fk_sia_invoice"
  sales_invoices ||--o{ sales_invoice_audit_logs : "fk_sial_invoice"
  sales_invoices ||--o{ sales_invoice_lines : "fk_sil_invoice"
  sales_orders ||--o{ sales_order_cancellation_logs : "fk_socl_sales_order"
  sales_orders ||--o{ sales_order_product_lines : "fk_sopl_so"
  sales_order_sites ||--o{ sales_order_site_chemicals : "fk_sosc_site"
  sales_order_sites ||--o{ sales_order_site_services : "fk_soss_site"
  sales_orders ||--o{ sales_order_sites : "fk_sos_so"
  service_categories ||--o{ service_category_area : "fk_sca_category"
```

### er_part_5_of_7

```mermaid
erDiagram
  service_category_fixed {
    VARCHAR id PK
    VARCHAR service_category_id FK
    VARCHAR service_sub_category_id FK
    VARCHAR tier_name
    DOUBLE_PRECISION price_amount
    INTEGER display_order
    BOOLEAN is_active
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  service_category_inspection {
    VARCHAR id PK
    VARCHAR service_category_id FK
    DOUBLE_PRECISION inspection_fee
    BOOLEAN is_active
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  service_custom_pricing_blocks {
    VARCHAR id PK
    VARCHAR service_category_id FK
    VARCHAR service_sub_category_id FK
    VARCHAR label
    BOOLEAN is_active
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  service_custom_pricing_fields {
    VARCHAR id PK
    VARCHAR block_id FK
    VARCHAR field_name
    DOUBLE_PRECISION price_amount
    INTEGER display_order
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  service_execution_chemical_usages {
    VARCHAR id PK
    VARCHAR service_execution_id FK
    VARCHAR inventory_product_id FK
    VARCHAR service_product_id
    NUMERIC required_dilution_snapshot
    NUMERIC used_dilution
    NUMERIC required_qty
    NUMERIC used_qty
    VARCHAR product_name_snapshot
    VARCHAR hsn_snapshot
    VARCHAR uom_snapshot
    INTEGER sort_order
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  service_execution_treatments {
    VARCHAR service_execution_id PK
    VARCHAR service_treatment_id PK
  }
  service_executions {
    VARCHAR id PK
    VARCHAR task_id FK
    VARCHAR service_id FK
    VARCHAR service_name_snapshot
    VARCHAR infestation_level
    TEXT location_area
    TEXT trap_codes
    INTEGER sort_order
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  service_pest_types {
    VARCHAR id PK
    VARCHAR name
    BOOLEAN is_active
    INTEGER display_order
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  service_products {
    VARCHAR id PK
    VARCHAR service_id FK
    VARCHAR inventory_product_id FK
    VARCHAR dilution
    DOUBLE_PRECISION coverage_sqft
    DOUBLE_PRECISION required_qty
    DOUBLE_PRECISION price_per_uom
    DOUBLE_PRECISION cost_per_visit
    DOUBLE_PRECISION est_cost_per_month
    BOOLEAN is_manual_entry
    INTEGER display_order
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  service_species {
    VARCHAR id PK
    VARCHAR service_id FK
    VARCHAR species_name
    VARCHAR scientific_name
    INTEGER display_order
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  service_sub_categories {
    VARCHAR id PK
    VARCHAR code
    VARCHAR name
    BOOLEAN is_active
    INTEGER display_order
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  service_treatments {
    VARCHAR id PK
    VARCHAR name
    BOOLEAN is_active
    INTEGER display_order
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  services {
    VARCHAR id PK
    VARCHAR service_code
    VARCHAR service_name
    TEXT description
    VARCHAR price_type
    DOUBLE_PRECISION duration_value
    VARCHAR duration_uom
    VARCHAR status
    BOOLEAN is_draft
    DOUBLE_PRECISION visits_per_month
    INTEGER warranty_months
    BOOLEAN free_revisit_included
    INTEGER free_revisit_quantity
    TEXT inactive_reason
    TIMESTAMPTZ inactive_at
    VARCHAR inactivated_by
    INTEGER display_order
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  services_service_categories {
    VARCHAR service_id PK
    VARCHAR service_category_id PK
  }
  services_service_category_area {
    VARCHAR service_id PK
    VARCHAR service_category_area_id PK
  }
  services_service_category_fixed {
    VARCHAR service_id PK
    VARCHAR service_category_fixed_id PK
  }
  services_service_category_inspection {
    VARCHAR service_id PK
    VARCHAR service_category_inspection_id PK
  }
  services_service_custom_pricing_blocks {
    VARCHAR service_id PK
    VARCHAR service_custom_pricing_block_id PK
  }
  services_service_pest_types {
    VARCHAR service_id PK
    VARCHAR service_pest_type_id PK
  }
  services_service_sub_categories {
    VARCHAR service_id PK
    VARCHAR service_sub_category_id PK
  }
  services_service_treatments {
    VARCHAR service_id PK
    VARCHAR service_treatment_id PK
  }
  stock_approval_logs {
    BIGSERIAL id PK
    BIGINT request_id FK
    VARCHAR action
    VARCHAR previous_status
    VARCHAR new_status
    TEXT remarks
    VARCHAR created_by
    TIMESTAMPTZ created_at
  }
  stock_ledger {
    BIGINT_AUTO_INCREMENT id PK
    VARCHAR branch_id
    VARCHAR product_id FK
    VARCHAR product_code
    VARCHAR product_name
    VARCHAR category
    VARCHAR brand
    VARCHAR hsn_code
    VARCHAR base_uom
    INT assets_qty
    INT consumable_qty
    INT resell_qty
    INT in_transit_qty
    INT reserved_qty
    VARCHAR status
    VARCHAR created_by
    DATETIME created_at
    VARCHAR updated_by
    DATETIME updated_at
    VARCHAR deleted_by
    DATETIME deleted_at
  }
  stock_movement_logs {
    BIGSERIAL id PK
    VARCHAR reference_type
    VARCHAR reference_id
    VARCHAR branch_id
    VARCHAR product_id
    VARCHAR stock_type
    INTEGER quantity_delta
    VARCHAR action
    TEXT remarks
    VARCHAR created_by
    TIMESTAMPTZ created_at
  }
  stock_request_items {
    BIGSERIAL id PK
    BIGINT request_id FK
    VARCHAR product_id FK
    VARCHAR product_code
    VARCHAR product_name
    VARCHAR base_uom
    INTEGER assets_req_qty
    INTEGER consumable_req_qty
    INTEGER resell_req_qty
    INTEGER assets_appr_qty
    INTEGER consumable_appr_qty
    INTEGER resell_appr_qty
    NUMERIC estimated_cost
    NUMERIC tax_amount
    VARCHAR item_purpose
    VARCHAR alternative_source
  }
  service_sub_categories ||--o{ service_category_fixed : "fk_scf_sub_category"
  service_sub_categories ||--o{ service_custom_pricing_blocks : "fk_scpb_sub_category"
  service_custom_pricing_blocks ||--o{ service_custom_pricing_fields : "fk_scpf_block"
  service_executions ||--o{ service_execution_chemical_usages : "fk_sec_execution"
  service_executions ||--o{ service_execution_treatments : "fk_set_execution"
  service_treatments ||--o{ service_execution_treatments : "fk_set_treatment"
  services ||--o{ service_executions : "fk_se_service"
  services ||--o{ service_products : "fk_sp_service"
  services ||--o{ service_species : "fk_service_species_service"
  services ||--o{ services_service_categories : "fk_ssc_service"
  services ||--o{ services_service_category_area : "fk_ssca_service"
  services ||--o{ services_service_category_fixed : "fk_sscf_service"
  service_category_fixed ||--o{ services_service_category_fixed : "fk_sscf_fixed"
  services ||--o{ services_service_category_inspection : "fk_ssci_service"
  service_category_inspection ||--o{ services_service_category_inspection : "fk_ssci_inspection"
  services ||--o{ services_service_custom_pricing_blocks : "fk_sscpb_service"
  service_custom_pricing_blocks ||--o{ services_service_custom_pricing_blocks : "fk_sscpb_block"
  services ||--o{ services_service_pest_types : "fk_sspt_service"
  service_pest_types ||--o{ services_service_pest_types : "fk_sspt_pest"
  services ||--o{ services_service_sub_categories : "fk_sssc_service"
  service_sub_categories ||--o{ services_service_sub_categories : "fk_sssc_sub"
  services ||--o{ services_service_treatments : "fk_sst_service"
  service_treatments ||--o{ services_service_treatments : "fk_sst_treatment"
```

### er_part_6_of_7

```mermaid
erDiagram
  stock_request_recipients {
    BIGSERIAL id
    BIGINT request_id FK
    BIGINT recipient_user_id PK
    VARCHAR recipient_email
    TIMESTAMPTZ created_at
    VARCHAR stock_request_id PK
  }
  stock_requests {
    BIGSERIAL id PK
    VARCHAR request_id
    VARCHAR request_type
    VARCHAR direction
    VARCHAR from_branch_id
    VARCHAR to_branch_id
    BIGINT requested_by_user_id
    VARCHAR requested_by_name
    VARCHAR priority
    DATE required_by_date
    TEXT purpose
    TEXT notes_for_approver
    TEXT sent_to
    VARCHAR status
    VARCHAR approval_type
    VARCHAR alternative_source
    DATE dispatch_date
    DATE expected_delivery_date
    VARCHAR carrier
    VARCHAR lr_number
    TEXT remarks
    VARCHAR issue_type
    TEXT issue_description
    VARCHAR issue_resolution_status
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
    VARCHAR deleted_by
    TIMESTAMPTZ deleted_at
  }
  stock_transfer_assets {
    BIGSERIAL id PK
    BIGINT transfer_id FK
    VARCHAR asset_id
    VARCHAR condition_at_dispatch
    VARCHAR transfer_with
    BIGINT destination_user_id
    VARCHAR destination_user_name
    VARCHAR condition_at_receipt
    VARCHAR receipt_status
  }
  stock_transfer_items {
    BIGSERIAL id PK
    BIGINT transfer_id FK
    VARCHAR product_id FK
    VARCHAR product_code
    VARCHAR product_name
    INTEGER assets_qty
    INTEGER consumable_qty
    INTEGER resell_qty
    VARCHAR source_branch_id
  }
  stock_transfers {
    BIGSERIAL id PK
    VARCHAR transfer_id
    VARCHAR reference_request_id
    VARCHAR from_branch_id
    VARCHAR to_branch_id
    VARCHAR transfer_type
    VARCHAR strategy
    VARCHAR status
    DATE dispatch_date
    DATE expected_delivery_date
    VARCHAR carrier
    VARCHAR lr_number
    TEXT remarks
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
  }
  subscription_plans {
    VARCHAR id PK
    VARCHAR plan_name
    TEXT description
    INTEGER branch_count
    INTEGER technician_count
    NUMERIC price_per_branch
    NUMERIC price_per_technician
    VARCHAR duration_type
    DATE valid_from
    DATE valid_to
    VARCHAR status
    VARCHAR created_by
    VARCHAR updated_by
    VARCHAR deleted_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
    TIMESTAMPTZ deleted_at
  }
  support_sla_settings {
    SMALLINT id PK
    INTEGER response_sla_hours
    INTEGER resolution_risk_threshold_pct
    TIMESTAMPTZ updated_at
    VARCHAR updated_by
  }
  support_ticket_activities {
    BIGSERIAL id PK
    VARCHAR ticket_id FK
    VARCHAR activity_type
    TEXT summary
    TEXT detail
    BOOLEAN is_internal
    BIGINT performed_by_user_id FK
    VARCHAR performed_by_label
    TIMESTAMPTZ performed_at
    JSONB metadata_json
  }
  support_ticket_assignment_history {
    BIGSERIAL id PK
    VARCHAR ticket_id FK
    BIGINT from_user_id FK
    BIGINT to_user_id FK
    VARCHAR to_role_code
    TEXT assignment_note
    TIMESTAMPTZ assigned_at
    VARCHAR assigned_by
  }
  support_ticket_attachments {
    VARCHAR id PK
    VARCHAR ticket_id FK
    VARCHAR phase
    VARCHAR file_path
    VARCHAR original_name
    VARCHAR mime_type
    BIGINT size_bytes
    TIMESTAMPTZ uploaded_at
    VARCHAR uploaded_by
  }
  support_ticket_tasks {
    VARCHAR id PK
    VARCHAR ticket_id FK
    VARCHAR task_id FK
    TIMESTAMPTZ linked_at
    VARCHAR linked_by
  }
  support_ticket_types {
    VARCHAR id PK
    VARCHAR code
    VARCHAR label
    INTEGER display_order
    BOOLEAN active
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  support_tickets {
    VARCHAR id PK
    VARCHAR ticket_number
    VARCHAR customer_id FK
    VARCHAR branch_id FK
    VARCHAR sales_order_id FK
    VARCHAR related_task_id FK
    VARCHAR ticket_type_id FK
    VARCHAR priority
    VARCHAR subject
    TEXT description
    VARCHAR reported_by_name
    VARCHAR reported_by_phone
    DATE expected_resolution_date
    TIME expected_resolution_time
    VARCHAR status
    VARCHAR assignee_role_code
    BIGINT assigned_user_id FK
    TIMESTAMPTZ response_sla_deadline_at
    TIMESTAMPTZ first_response_at
    TIMESTAMPTZ resolution_expected_at
    BOOLEAN response_sla_met
    BOOLEAN response_sla_breached
    BOOLEAN resolution_sla_breached
    VARCHAR escalation_level
    TIMESTAMPTZ pause_started_at
    INTEGER total_paused_seconds
    VARCHAR resolution_code
    TEXT resolution_notes
    INTEGER resolve_customer_rating
    TEXT resolve_customer_feedback
    TIMESTAMPTZ resolved_at
    VARCHAR resolved_by
    VARCHAR close_reason
    TEXT closure_remarks
    TIMESTAMPTZ closed_at
    VARCHAR closed_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
    VARCHAR created_by
    VARCHAR updated_by
  }
  task_audit_logs {
    BIGSERIAL id PK
    VARCHAR task_id
    VARCHAR action
    TEXT details
    VARCHAR performed_by
    TIMESTAMPTZ performed_at
  }
  task_customer_feedback {
    VARCHAR id PK
    VARCHAR task_id FK
    BIGINT technician_id FK
    VARCHAR customer_name
    VARCHAR customer_phone
    TEXT customer_feedback
    INTEGER ratings
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
  }
  task_materials {
    VARCHAR id PK
    VARCHAR task_id FK
    VARCHAR product_id
    VARCHAR product_name
    VARCHAR uom
    VARCHAR hsn_code
    NUMERIC std_qty
    NUMERIC required_qty
    NUMERIC used_qty
  }
  task_photos {
    VARCHAR id PK
    VARCHAR task_id FK
    VARCHAR photo_type
    VARCHAR file_path
    TIMESTAMPTZ uploaded_at
  }
  task_technicians {
    VARCHAR id PK
    VARCHAR task_id FK
    BIGINT user_id FK
    VARCHAR employee_name
    VARCHAR role_name
    BOOLEAN is_primary
  }
  tasks {
    VARCHAR id PK
    VARCHAR branch_id
    VARCHAR task_number
    VARCHAR task_type
    VARCHAR source_type
    VARCHAR sales_order_id
    VARCHAR so_site_service_id
    VARCHAR ticket_id FK
    VARCHAR customer_id
    VARCHAR customer_name
    VARCHAR site_id
    VARCHAR site_name
    TEXT site_address
    VARCHAR site_contact_name
    VARCHAR site_contact_mobile
    VARCHAR service_category
    VARCHAR service_subcategory
    VARCHAR service_type_name
    NUMERIC area_sqft
    DATE scheduled_date
    TIME start_time
    TIME end_time
    INTEGER estimated_duration_mins
    VARCHAR status
    VARCHAR priority
    TIMESTAMPTZ actual_start_at
    TIMESTAMPTZ actual_end_at
    TEXT completion_notes
    INTEGER customer_rating
    TEXT customer_feedback
    TIMESTAMPTZ feedback_at
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
  }
  tax_types {
    BIGINT id PK
    VARCHAR tax_type_code
    VARCHAR tax_name
    VARCHAR tax_category
    NUMERIC default_rate
    VARCHAR applicability
    VARCHAR description
    DATE effective_from
    VARCHAR status
    VARCHAR change_reason
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
    VARCHAR deleted_by
    TIMESTAMPTZ deleted_at
  }
  technician_observation_hygiene_picks {
    VARCHAR section_id PK
    VARCHAR hygiene_option_id PK
  }
  technician_observation_pest_picks {
    VARCHAR section_id PK
    VARCHAR pest_option_id PK
  }
  technician_observation_sections {
    VARCHAR id PK
    VARCHAR task_id FK
    VARCHAR category
    BOOLEAN found
    TEXT other_notes
    TEXT location_area
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  technician_observation_structural_picks {
    VARCHAR section_id PK
    VARCHAR structural_option_id PK
  }
  technician_tracking {
    BIGSERIAL id PK
    BIGINT user_id FK
    VARCHAR task_id FK
    VARCHAR technician_status
    NUMERIC latitude
    NUMERIC longitude
    DATE local_date
    TIMESTAMPTZ recorded_at
  }
  stock_requests ||--o{ stock_request_recipients : "fk_stock_request_recipients_request"
  stock_requests ||--o{ stock_request_recipients : "fk_srr_stock_request"
  stock_transfers ||--o{ stock_transfer_assets : "fk_stock_transfer_asset_transfer"
  stock_transfers ||--o{ stock_transfer_items : "fk_stock_transfer_item_transfer"
  support_tickets ||--o{ support_ticket_activities : "fk_sta_ticket"
  support_tickets ||--o{ support_ticket_assignment_history : "fk_stah_ticket"
  support_tickets ||--o{ support_ticket_attachments : "fk_attach_ticket"
  support_tickets ||--o{ support_ticket_tasks : "fk_stt_ticket"
  tasks ||--o{ support_ticket_tasks : "fk_stt_task"
  tasks ||--o{ support_tickets : "fk_st_related_task"
  support_ticket_types ||--o{ support_tickets : "fk_st_ticket_type"
  tasks ||--o{ task_customer_feedback : "fk_tcf_task"
  tasks ||--o{ task_materials : "fk_tm_task"
  tasks ||--o{ task_photos : "fk_tp_task"
  tasks ||--o{ task_technicians : "fk_tt_task"
  support_tickets ||--o{ tasks : "fk_tasks_support_ticket"
  technician_observation_sections ||--o{ technician_observation_hygiene_picks : "fk_tohp_section"
  technician_observation_sections ||--o{ technician_observation_pest_picks : "fk_topp_section"
  tasks ||--o{ technician_observation_sections : "fk_tos_task"
  technician_observation_sections ||--o{ technician_observation_structural_picks : "fk_tosp_section"
  tasks ||--o{ technician_tracking : "fk_technician_tracking_task"
```

### er_part_7_of_7

```mermaid
erDiagram
  user_additional_data {
    BIGSERIAL id PK
    BIGINT user_id FK
    VARCHAR aadhar_number
    VARCHAR pan_number
    VARCHAR uan_number
    VARCHAR id_card_number
    VARCHAR grade_level
    VARCHAR shift_type
    VARCHAR weekly_off
    NUMERIC target_amount
    NUMERIC commission_percentage
    VARCHAR photo_url
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
  }
  user_branches {
    BIGINT user_id PK
    VARCHAR branch_id PK
  }
  user_documents {
    BIGSERIAL id PK
    BIGINT user_id FK
    VARCHAR document_type
    VARCHAR file_path
    VARCHAR original_file_name
    BIGINT file_size_bytes
    VARCHAR mime_type
    VARCHAR uploaded_by
    TIMESTAMPTZ uploaded_at
  }
  user_leave_details {
    BIGSERIAL id PK
    BIGINT user_id FK
    INT casual_leave
    INT sick_leave
    INT paid_leave
    INT annual_leave_allocation
    BOOLEAN carry_forward_allowed
    INT max_carry_forward_days
    BIGINT leave_approval_role_id FK
    VARCHAR leave_reset_cycle
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
  }
  user_permissions {
    BIGSERIAL id PK
    BIGINT user_id FK
    BIGINT module_id FK
    BIGINT action_id FK
    BOOLEAN allowed
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  user_profile_extension {
    UUID id PK
    VARCHAR employee_id
    TEXT profile_photo_url
    TIMESTAMP created_at
    TIMESTAMP updated_at
  }
  user_salary_details {
    BIGSERIAL id PK
    BIGINT user_id FK
    VARCHAR salary_type
    NUMERIC basic_salary
    NUMERIC hra
    NUMERIC other_allowance
    NUMERIC incentive
    NUMERIC deductions
    BOOLEAN pf_applicable
    BOOLEAN esi_applicable
    BOOLEAN tds_applicable
    VARCHAR bank_name
    VARCHAR account_number
    VARCHAR ifsc_code
    DATE salary_effective_from
    DATE salary_effective_to
    BOOLEAN holiday_work_applicable
    VARCHAR holiday_work_type
    NUMERIC holiday_work_amount
    BOOLEAN overtime_applicable
    VARCHAR overtime_type
    NUMERIC per_hour_incentive_pay
    INT max_ot_hours_per_month
    VARCHAR created_by
    TIMESTAMPTZ created_at
    VARCHAR updated_by
    TIMESTAMPTZ updated_at
  }
  users {
    BIGSERIAL id PK
    VARCHAR emp_id
    VARCHAR first_name
    VARCHAR last_name
    VARCHAR email
    VARCHAR username
    VARCHAR password_hash
    VARCHAR contact_number
    VARCHAR alternate_number
    VARCHAR department
    VARCHAR designation
    BIGINT role_id FK
    BIGINT reporting_manager_id FK
    VARCHAR employment_type
    DATE date_of_joining
    VARCHAR status
    BOOLEAN is_active
    VARCHAR current_address_line1
    VARCHAR current_address_line2
    VARCHAR current_city
    VARCHAR current_state
    VARCHAR current_country
    VARCHAR current_pincode
    VARCHAR permanent_address_line1
    VARCHAR permanent_address_line2
    VARCHAR permanent_city
    VARCHAR permanent_state
    VARCHAR permanent_country
    VARCHAR permanent_pincode
    TIMESTAMPTZ last_login_at
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  vendor_product_supplies {
    VARCHAR id PK
    VARCHAR vendor_id
    VARCHAR product_id
    VARCHAR product_category
    DOUBLE_PRECISION supply_quantity
    VARCHAR uom
    DOUBLE_PRECISION unit_supply_rate
    DOUBLE_PRECISION minimum_order_quantity
    VARCHAR delivery_frequency
    INTEGER delivery_lead_time_days
    BOOLEAN tax_applicable
    VARCHAR created_by
    VARCHAR updated_by
    VARCHAR deleted_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
    TIMESTAMPTZ deleted_at
  }
  vendors {
    VARCHAR id PK
    VARCHAR vendor_name
    VARCHAR vendor_type
    VARCHAR vendor_category
    VARCHAR product_supplied
    VARCHAR contact_person
    VARCHAR phone_number
    VARCHAR email_id
    VARCHAR vendor_status
    BOOLEAN has_contract
    TEXT address
    VARCHAR city
    VARCHAR state
    VARCHAR pincode
    VARCHAR country
    VARCHAR vendor_registration_type
    VARCHAR gst_number
    VARCHAR pan_number
    VARCHAR bank_name
    VARCHAR account_holder_name
    VARCHAR account_number
    VARCHAR ifsc_code
    VARCHAR contract_type
    DATE contract_start_date
    DATE contract_end_date
    BOOLEAN sla_agreement
    TEXT contract_document_url
    VARCHAR billing_type
    VARCHAR billing_cycle
    DATE custom_billing_start_date
    DATE custom_billing_end_date
    VARCHAR invoice_submission_method
    VARCHAR payment_terms
    DOUBLE_PRECISION advance_payment_percentage
    TEXT late_payment_penalty
    INTEGER vendor_rating
    TEXT remarks
    TEXT vendor_document_url
    VARCHAR vendor_document_name
    VARCHAR vendor_document_type
    VARCHAR created_by
    VARCHAR updated_by
    VARCHAR deleted_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
    TIMESTAMPTZ deleted_at
  }
  voucher_allocations {
    VARCHAR id PK
    VARCHAR voucher_id FK
    VARCHAR document_type
    VARCHAR document_id
    NUMERIC pending_before
    NUMERIC allocated_amount
    NUMERIC shortfall_amount
    VARCHAR settlement_action
    VARCHAR status_after
    TIMESTAMPTZ created_at
  }
  voucher_audit_logs {
    VARCHAR id PK
    VARCHAR voucher_id FK
    VARCHAR action
    VARCHAR remarks
    VARCHAR performed_by
    TIMESTAMPTZ performed_at
  }
  voucher_journal_lines {
    VARCHAR id PK
    VARCHAR voucher_id FK
    INTEGER line_no
    VARCHAR ledger_id
    NUMERIC dr_amount
    NUMERIC cr_amount
    VARCHAR line_narration
  }
  voucher_settlement_links {
    VARCHAR id PK
    VARCHAR voucher_id FK
    VARCHAR settlement_type
    VARCHAR settlement_id
    VARCHAR settlement_number
    NUMERIC settlement_amount
  }
  vouchers {
    VARCHAR id PK
    VARCHAR voucher_number
    VARCHAR voucher_type
    DATE voucher_date
    VARCHAR branch_id
    VARCHAR party_type
    VARCHAR party_id
    VARCHAR payment_mode
    VARCHAR bank_ledger_id
    VARCHAR from_ledger_id
    VARCHAR to_ledger_id
    VARCHAR reference_no
    DATE cheque_date
    NUMERIC gross_amount
    NUMERIC tds_amount
    NUMERIC advance_applied
    NUMERIC allocated_amount
    NUMERIC unallocated_amount
    TEXT notes
    VARCHAR status
    VARCHAR created_by
    VARCHAR updated_by
    TIMESTAMPTZ created_at
    TIMESTAMPTZ updated_at
  }
  users ||--o{ user_additional_data : "fk_user_additional_user"
  users ||--o{ user_branches : "fk_ub_user"
  users ||--o{ user_documents : "fk_user_doc_user"
  users ||--o{ user_leave_details : "fk_user_leave_user"
  users ||--o{ user_permissions : "fk_up_user"
  users ||--o{ user_salary_details : "fk_user_salary_user"
  users ||--o{ users : "fk_users_reporting_manager"
  vouchers ||--o{ voucher_allocations : "fk_va_voucher"
  vouchers ||--o{ voucher_audit_logs : "fk_val_voucher"
  vouchers ||--o{ voucher_journal_lines : "fk_vjl_voucher"
  vouchers ||--o{ voucher_settlement_links : "fk_vsl_voucher"
```

## Mermaid FK Flowcharts (split)

Note: schema big. Graph split into smaller parts so Mermaid renderer can load.

### flowchart_part_1_of_1

```mermaid
flowchart LR
  actions["actions"]
  asset_units["asset_units"]
  bill_payment_allocations["bill_payment_allocations"]
  branches["branches"]
  central_stock_entries["central_stock_entries"]
  central_stock_ledger["central_stock_ledger"]
  coa_account_heads["coa_account_heads"]
  company_details["company_details"]
  contract_amendment_logs["contract_amendment_logs"]
  contract_payment_lines["contract_payment_lines"]
  contract_sales_order_links["contract_sales_order_links"]
  contract_site_services["contract_site_services"]
  contract_sites["contract_sites"]
  contract_termination_logs["contract_termination_logs"]
  contracts["contracts"]
  credit_notes["credit_notes"]
  customers["customers"]
  debit_notes["debit_notes"]
  follow_ups["follow_ups"]
  gma_audit_logs["gma_audit_logs"]
  gma_chemicals["gma_chemicals"]
  gma_prospects["gma_prospects"]
  gma_services["gma_services"]
  gma_sheet_approver_roles["gma_sheet_approver_roles"]
  gma_sheets["gma_sheets"]
  gma_sites["gma_sites"]
  hiring_request_branches["hiring_request_branches"]
  hiring_request_recipients["hiring_request_recipients"]
  hiring_requests["hiring_requests"]
  hrm_attendance_day["hrm_attendance_day"]
  hrm_leave_request["hrm_leave_request"]
  hrm_salary_month["hrm_salary_month"]
  hrm_salary_slip["hrm_salary_slip"]
  hsn_code_tax_types["hsn_code_tax_types"]
  hsn_codes["hsn_codes"]
  incentive_overtime_details["incentive_overtime_details"]
  inventory_brands["inventory_brands"]
  inventory_products["inventory_products"]
  invoice_payment_allocations["invoice_payment_allocations"]
  lead_audit_logs["lead_audit_logs"]
  leads["leads"]
  leave_configuration["leave_configuration"]
  ledger_entries["ledger_entries"]
  ledgers["ledgers"]
  modules["modules"]
  notification_recipients["notification_recipients"]
  notifications["notifications"]
  observation_options_hygiene["observation_options_hygiene"]
  observation_options_pest_sighting["observation_options_pest_sighting"]
  observation_options_structural["observation_options_structural"]
  petty_cash_attachments["petty_cash_attachments"]
  petty_cash_audit_logs["petty_cash_audit_logs"]
  petty_cash_request_recipient_roles["petty_cash_request_recipient_roles"]
  petty_cash_request_recipients["petty_cash_request_recipients"]
  petty_cash_requests["petty_cash_requests"]
  public_actions["public.actions"]
  public_company_details["public.company_details"]
  public_company_documents["public.company_documents"]
  public_company_subscription["public.company_subscription"]
  public_email_verification_tokens["public.email_verification_tokens"]
  public_global_users["public.global_users"]
  public_modules["public.modules"]
  public_role_permissions["public.role_permissions"]
  public_roles["public.roles"]
  public.subscription_plans["public.subscription_plans"]
  public_tenant_registry["public.tenant_registry"]
  purchase_bill_attachments["purchase_bill_attachments"]
  purchase_bill_audit_logs["purchase_bill_audit_logs"]
  purchase_bill_lines["purchase_bill_lines"]
  purchase_bills["purchase_bills"]
  purchase_order["purchase_order"]
  purchase_order_item["purchase_order_item"]
  quotation_attachments["quotation_attachments"]
  quotation_audit_logs["quotation_audit_logs"]
  quotation_locations["quotation_locations"]
  quotation_product_lines["quotation_product_lines"]
  quotation_prospects["quotation_prospects"]
  quotation_service_lines["quotation_service_lines"]
  quotations["quotations"]
  role_compensation_configuration["role_compensation_configuration"]
  role_permissions["role_permissions"]
  roles["roles"]
  salary_details["salary_details"]
  sales_invoice_attachments["sales_invoice_attachments"]
  sales_invoice_audit_logs["sales_invoice_audit_logs"]
  sales_invoice_lines["sales_invoice_lines"]
  sales_invoices["sales_invoices"]
  sales_order_cancellation_logs["sales_order_cancellation_logs"]
  sales_order_product_lines["sales_order_product_lines"]
  sales_order_site_chemicals["sales_order_site_chemicals"]
  sales_order_site_services["sales_order_site_services"]
  sales_order_sites["sales_order_sites"]
  sales_orders["sales_orders"]
  service_audit_logs["service_audit_logs"]
  service_categories["service_categories"]
  service_category_area["service_category_area"]
  service_category_fixed["service_category_fixed"]
  service_category_inspection["service_category_inspection"]
  service_custom_pricing_blocks["service_custom_pricing_blocks"]
  service_custom_pricing_fields["service_custom_pricing_fields"]
  service_execution_chemical_usages["service_execution_chemical_usages"]
  service_execution_treatments["service_execution_treatments"]
  service_executions["service_executions"]
  service_pest_types["service_pest_types"]
  service_products["service_products"]
  service_species["service_species"]
  service_sub_categories["service_sub_categories"]
  service_treatments["service_treatments"]
  services["services"]
  services_service_categories["services_service_categories"]
  services_service_category_area["services_service_category_area"]
  services_service_category_fixed["services_service_category_fixed"]
  services_service_category_inspection["services_service_category_inspection"]
  services_service_custom_pricing_blocks["services_service_custom_pricing_blocks"]
  services_service_pest_types["services_service_pest_types"]
  services_service_sub_categories["services_service_sub_categories"]
  services_service_treatments["services_service_treatments"]
  stock_approval_logs["stock_approval_logs"]
  stock_ledger["stock_ledger"]
  stock_request_items["stock_request_items"]
  stock_request_recipients["stock_request_recipients"]
  stock_requests["stock_requests"]
  stock_transfer_assets["stock_transfer_assets"]
  stock_transfer_items["stock_transfer_items"]
  stock_transfers["stock_transfers"]
  support_ticket_activities["support_ticket_activities"]
  support_ticket_assignment_history["support_ticket_assignment_history"]
  support_ticket_attachments["support_ticket_attachments"]
  support_ticket_tasks["support_ticket_tasks"]
  support_ticket_types["support_ticket_types"]
  support_tickets["support_tickets"]
  task_customer_feedback["task_customer_feedback"]
  task_materials["task_materials"]
  task_photos["task_photos"]
  task_technicians["task_technicians"]
  tasks["tasks"]
  tax_types["tax_types"]
  technician_observation_hygiene_picks["technician_observation_hygiene_picks"]
  technician_observation_pest_picks["technician_observation_pest_picks"]
  technician_observation_sections["technician_observation_sections"]
  technician_observation_structural_picks["technician_observation_structural_picks"]
  technician_tracking["technician_tracking"]
  user_additional_data["user_additional_data"]
  user_branches["user_branches"]
  user_documents["user_documents"]
  user_leave_details["user_leave_details"]
  user_permissions["user_permissions"]
  user_salary_details["user_salary_details"]
  users["users"]
  vendors["vendors"]
  voucher_allocations["voucher_allocations"]
  voucher_audit_logs["voucher_audit_logs"]
  voucher_journal_lines["voucher_journal_lines"]
  voucher_settlement_links["voucher_settlement_links"]
  vouchers["vouchers"]
  asset_units --> inventory_products
  bill_payment_allocations --> purchase_bills
  bill_payment_allocations --> vouchers
  central_stock_entries --> users
  central_stock_entries --> inventory_products
  central_stock_entries --> vendors
  central_stock_ledger --> inventory_products
  coa_account_heads --> coa_account_heads
  contract_amendment_logs --> contracts
  contract_payment_lines --> contracts
  contract_sales_order_links --> contracts
  contract_sales_order_links --> sales_orders
  contract_site_services --> contract_sites
  contract_sites --> contracts
  contract_termination_logs --> contracts
  credit_notes --> sales_invoices
  debit_notes --> purchase_bills
  follow_ups --> leads
  gma_audit_logs --> gma_sheets
  gma_chemicals --> inventory_products
  gma_chemicals --> gma_services
  gma_services --> gma_sites
  gma_services --> services
  gma_sheet_approver_roles --> roles
  gma_sheet_approver_roles --> gma_sheets
  gma_sheets --> branches
  gma_sheets --> leads
  gma_sheets --> gma_prospects
  gma_sites --> gma_sheets
  hiring_request_branches --> hiring_requests
  hiring_request_recipients --> hiring_requests
  hiring_request_recipients --> users
  hiring_requests --> users
  hiring_requests --> roles
  hiring_requests --> users
  hiring_requests --> users
  hrm_attendance_day --> users
  hrm_leave_request --> users
  hrm_leave_request --> users
  hrm_salary_month --> users
  hrm_salary_month --> users
  hrm_salary_slip --> hrm_salary_month
  hsn_code_tax_types --> hsn_codes
  hsn_code_tax_types --> tax_types
  incentive_overtime_details --> role_compensation_configuration
  inventory_products --> inventory_brands
  invoice_payment_allocations --> sales_invoices
  invoice_payment_allocations --> vouchers
  lead_audit_logs --> leads
  leave_configuration --> role_compensation_configuration
  leave_configuration --> roles
  ledger_entries --> ledgers
  ledgers --> coa_account_heads
  notification_recipients --> notifications
  notification_recipients --> users
  petty_cash_attachments --> petty_cash_requests
  petty_cash_audit_logs --> users
  petty_cash_audit_logs --> petty_cash_requests
  petty_cash_request_recipient_roles --> petty_cash_requests
  petty_cash_request_recipient_roles --> roles
  petty_cash_request_recipients --> petty_cash_requests
  petty_cash_request_recipients --> roles
  petty_cash_request_recipients --> users
  petty_cash_requests --> branches
  petty_cash_requests --> users
  petty_cash_requests --> users
  petty_cash_requests --> users
  petty_cash_requests --> users
  public_company_details --> public.subscription_plans
  public_company_documents --> company_details
  public_company_subscription --> public_company_details
  public_company_subscription --> public.subscription_plans
  public_email_verification_tokens --> public_global_users
  public_role_permissions --> public_actions
  public_role_permissions --> public_modules
  public_role_permissions --> public_roles
  public_tenant_registry --> public_company_details
  purchase_bill_attachments --> purchase_bills
  purchase_bill_audit_logs --> purchase_bills
  purchase_bill_lines --> purchase_bills
  purchase_order_item --> inventory_products
  purchase_order_item --> purchase_order
  purchase_order --> branches
  purchase_order --> vendors
  quotation_attachments --> quotations
  quotation_audit_logs --> quotations
  quotation_locations --> branches
  quotation_locations --> quotations
  quotation_product_lines --> inventory_products
  quotation_product_lines --> quotations
  quotation_service_lines --> quotation_locations
  quotation_service_lines --> quotations
  quotation_service_lines --> services
  quotations --> leads
  quotations --> quotation_prospects
  quotations --> quotations
  role_compensation_configuration --> roles
  role_permissions --> actions
  role_permissions --> modules
  role_permissions --> roles
  salary_details --> role_compensation_configuration
  sales_invoice_attachments --> sales_invoices
  sales_invoice_audit_logs --> sales_invoices
  sales_invoice_lines --> sales_invoices
  sales_order_cancellation_logs --> sales_orders
  sales_order_product_lines --> sales_orders
  sales_order_site_chemicals --> sales_order_sites
  sales_order_site_services --> sales_order_sites
  sales_order_sites --> sales_orders
  service_audit_logs --> services
  service_category_area --> service_categories
  service_category_area --> service_sub_categories
  service_category_fixed --> service_categories
  service_category_fixed --> service_sub_categories
  service_category_inspection --> service_categories
  service_custom_pricing_blocks --> service_categories
  service_custom_pricing_blocks --> service_sub_categories
  service_custom_pricing_fields --> service_custom_pricing_blocks
  service_execution_chemical_usages --> service_executions
  service_execution_chemical_usages --> inventory_products
  service_execution_treatments --> service_executions
  service_execution_treatments --> service_treatments
  service_executions --> services
  service_executions --> tasks
  service_products --> inventory_products
  service_products --> services
  service_species --> services
  services_service_categories --> service_categories
  services_service_categories --> services
  services_service_category_area --> service_category_area
  services_service_category_area --> services
  services_service_category_fixed --> service_category_fixed
  services_service_category_fixed --> services
  services_service_category_inspection --> service_category_inspection
  services_service_category_inspection --> services
  services_service_custom_pricing_blocks --> service_custom_pricing_blocks
  services_service_custom_pricing_blocks --> services
  services_service_pest_types --> service_pest_types
  services_service_pest_types --> services
  services_service_sub_categories --> services
  services_service_sub_categories --> service_sub_categories
  services_service_treatments --> services
  services_service_treatments --> service_treatments
  stock_approval_logs --> stock_requests
  stock_ledger --> inventory_products
  stock_ledger --> inventory_products
  stock_request_items --> inventory_products
  stock_request_items --> stock_requests
  stock_request_recipients --> stock_requests
  stock_request_recipients --> users
  stock_request_recipients --> stock_requests
  stock_transfer_assets --> stock_transfers
  stock_transfer_items --> inventory_products
  stock_transfer_items --> stock_transfers
  support_ticket_activities --> support_tickets
  support_ticket_activities --> users
  support_ticket_assignment_history --> users
  support_ticket_assignment_history --> support_tickets
  support_ticket_assignment_history --> users
  support_ticket_attachments --> support_tickets
  support_ticket_tasks --> tasks
  support_ticket_tasks --> support_tickets
  support_tickets --> users
  support_tickets --> branches
  support_tickets --> customers
  support_tickets --> tasks
  support_tickets --> sales_orders
  support_tickets --> support_ticket_types
  task_customer_feedback --> tasks
  task_customer_feedback --> users
  task_materials --> tasks
  task_photos --> tasks
  task_technicians --> tasks
  task_technicians --> users
  tasks --> support_tickets
  technician_observation_hygiene_picks --> observation_options_hygiene
  technician_observation_hygiene_picks --> technician_observation_sections
  technician_observation_pest_picks --> observation_options_pest_sighting
  technician_observation_pest_picks --> technician_observation_sections
  technician_observation_sections --> tasks
  technician_observation_structural_picks --> observation_options_structural
  technician_observation_structural_picks --> technician_observation_sections
  technician_tracking --> tasks
  technician_tracking --> users
  user_additional_data --> users
  user_branches --> users
  user_documents --> users
  user_leave_details --> roles
  user_leave_details --> users
  user_permissions --> actions
  user_permissions --> modules
  user_permissions --> users
  user_salary_details --> users
  users --> users
  users --> roles
  voucher_allocations --> vouchers
  voucher_audit_logs --> vouchers
  voucher_journal_lines --> vouchers
  voucher_settlement_links --> vouchers
```

## Relationships (FKs)

- Total: 199

```text
asset_units(product_id) -> inventory_products(id) [fk_asset_units_product] ON DELETE RESTRICT
bill_payment_allocations(bill_id) -> purchase_bills(id) [fk_bpa_bill] ON DELETE CASCADE
bill_payment_allocations(voucher_id) -> vouchers(id) [fk_bpa_voucher] ON DELETE CASCADE
central_stock_entries(assignee_user_id) -> users(id) [fk_central_stock_entries_assignee_user] ON DELETE SET NULL
central_stock_entries(product_id) -> inventory_products(id) [fk_central_stock_entries_product] ON DELETE RESTRICT
central_stock_entries(supplier_id) -> vendors(id) [fk_central_stock_entries_supplier] ON DELETE SET NULL
central_stock_ledger(product_id) -> inventory_products(id) [fk_central_stock_ledger_product] ON DELETE RESTRICT
coa_account_heads(parent_head_id) -> coa_account_heads(id) [fk_coa_parent] ON DELETE RESTRICT
contract_amendment_logs(contract_id) -> contracts(id) [fk_cal_contract] ON DELETE CASCADE
contract_payment_lines(contract_id) -> contracts(id) [fk_cpl_contract] ON DELETE CASCADE
contract_sales_order_links(contract_id) -> contracts(id) [fk_csol_contract] ON DELETE CASCADE
contract_sales_order_links(sales_order_id) -> sales_orders(id) [fk_csol_sales_order] ON DELETE SET NULL
contract_site_services(contract_site_id) -> contract_sites(id) [fk_css_site] ON DELETE CASCADE
contract_sites(contract_id) -> contracts(id) [fk_cs_contract] ON DELETE CASCADE
contract_termination_logs(contract_id) -> contracts(id) [fk_ctl_contract] ON DELETE CASCADE
credit_notes(invoice_id) -> sales_invoices(id) [fk_cn_invoice] ON DELETE RESTRICT
debit_notes(bill_id) -> purchase_bills(id) [fk_dn_bill] ON DELETE RESTRICT
follow_ups(lead_id) -> leads(id) [fk_follow_ups_lead]
gma_audit_logs(gma_sheet_id) -> gma_sheets(id) [fk_gmaaud_sheet] ON DELETE CASCADE
gma_chemicals(product_id) -> inventory_products(id) [fk_gmachem_product] ON DELETE RESTRICT
gma_chemicals(gma_service_id) -> gma_services(id) [fk_gmachem_service] ON DELETE CASCADE
gma_services(gma_site_id) -> gma_sites(id) [fk_gmasvc_site] ON DELETE CASCADE
gma_services(service_type_id) -> services(id) [fk_gmasvc_type] ON DELETE RESTRICT
gma_sheet_approver_roles(role_id) -> roles(id) [fk_gsar_role] ON DELETE CASCADE
gma_sheet_approver_roles(gma_sheet_id) -> gma_sheets(id) [fk_gsar_sheet] ON DELETE CASCADE
gma_sheets(branch_id) -> branches(id) [fk_gma_branch] ON DELETE RESTRICT
gma_sheets(lead_id) -> leads(id) [fk_gma_lead] ON DELETE RESTRICT
gma_sheets(prospect_id) -> gma_prospects(id) [fk_gma_prospect] ON DELETE RESTRICT
gma_sites(gma_sheet_id) -> gma_sheets(id) [fk_gmasite_sheet] ON DELETE CASCADE
hiring_request_branches(hiring_request_id) -> hiring_requests(id) [fk_hrb_hiring_request] ON DELETE CASCADE
hiring_request_recipients(hiring_request_id) -> hiring_requests(id) [fk_hrr_hiring_request] ON DELETE CASCADE
hiring_request_recipients(recipient_user_id) -> users(id) [fk_hrr_user] ON DELETE CASCADE
hiring_requests(converted_user_id) -> users(id) [fk_hiring_converted_user] ON DELETE SET NULL
hiring_requests(proposed_role_id) -> roles(id) [fk_hiring_proposed_role] ON DELETE RESTRICT
hiring_requests(requested_by_user_id) -> users(id) [fk_hiring_requested_by] ON DELETE RESTRICT
hiring_requests(reviewed_by_user_id) -> users(id) [fk_hiring_reviewed_by] ON DELETE SET NULL
hrm_attendance_day(user_id) -> users(id) [fk_hrm_attendance_user] ON DELETE CASCADE
hrm_leave_request(reviewed_by_user_id) -> users(id) [fk_hrm_leave_reviewed_by] ON DELETE SET NULL
hrm_leave_request(user_id) -> users(id) [fk_hrm_leave_user] ON DELETE CASCADE
hrm_salary_month(paid_by_user_id) -> users(id) [fk_hrm_salary_paid_by] ON DELETE SET NULL
hrm_salary_month(user_id) -> users(id) [fk_hrm_salary_user] ON DELETE CASCADE
hrm_salary_slip(salary_month_id) -> hrm_salary_month(id) [fk_hrm_salary_slip_month] ON DELETE CASCADE
hsn_code_tax_types(hsn_code_id) -> hsn_codes(id) [fk_hsn_code]
hsn_code_tax_types(tax_type_id) -> tax_types(id) [fk_tax_type]
incentive_overtime_details(config_id) -> role_compensation_configuration(config_id) [fk_iod_config] ON DELETE CASCADE
inventory_products(brand_id) -> inventory_brands(id) [fk_inventory_products_brand] ON DELETE RESTRICT
invoice_payment_allocations(invoice_id) -> sales_invoices(id) [fk_ipa_invoice] ON DELETE CASCADE
invoice_payment_allocations(voucher_id) -> vouchers(id) [fk_ipa_voucher] ON DELETE CASCADE
lead_audit_logs(lead_id) -> leads(id) [fk_audit_lead]
leave_configuration(config_id) -> role_compensation_configuration(config_id) [fk_lc_config] ON DELETE CASCADE
leave_configuration(leave_approval_role_id) -> roles(id) [fk_lc_role] ON DELETE RESTRICT
ledger_entries(ledger_id) -> ledgers(id) [fk_le_ledger] ON DELETE RESTRICT
ledgers(account_head_id) -> coa_account_heads(id) [fk_ledgers_account_head] ON DELETE RESTRICT
notification_recipients(notification_id) -> notifications(id) [fk_nr_notification] ON DELETE CASCADE
notification_recipients(user_id) -> users(id) [fk_nr_user] ON DELETE CASCADE
petty_cash_attachments(request_id) -> petty_cash_requests(id) [fk_pc_att_request] ON DELETE CASCADE
petty_cash_audit_logs(actor_user_id) -> users(id) [fk_pc_aud_actor] ON DELETE SET NULL
petty_cash_audit_logs(request_id) -> petty_cash_requests(id) [fk_pc_aud_request] ON DELETE CASCADE
petty_cash_request_recipient_roles(request_id) -> petty_cash_requests(id) [fk_pc_rr_request] ON DELETE CASCADE
petty_cash_request_recipient_roles(recipient_role_id) -> roles(id) [fk_pc_rr_role] ON DELETE CASCADE
petty_cash_request_recipients(request_id) -> petty_cash_requests(id) [fk_pc_rec_request] ON DELETE CASCADE
petty_cash_request_recipients(recipient_role_id) -> roles(id) [fk_pc_rec_role] ON DELETE CASCADE
petty_cash_request_recipients(recipient_user_id) -> users(id) [fk_pc_rec_user] ON DELETE CASCADE
petty_cash_requests(requester_branch_id) -> branches(id) [fk_pc_branch] ON DELETE RESTRICT
petty_cash_requests(paid_by_user_id) -> users(id) [fk_pc_paid_by] ON DELETE SET NULL
petty_cash_requests(pre_approved_by_user_id) -> users(id) [fk_pc_pre_approved_by] ON DELETE SET NULL
petty_cash_requests(requester_user_id) -> users(id) [fk_pc_requester_user] ON DELETE RESTRICT
petty_cash_requests(reviewed_by_user_id) -> users(id) [fk_pc_reviewed_by] ON DELETE SET NULL
public.company_details(subscription_plan_id) -> public.subscription_plans(id) [fk_company_details_subscription_plan] ON DELETE SET NULL
public.company_documents(company_id) -> company_details(id) [fk_company_documents_company] ON DELETE CASCADE
public.company_subscription(company_id) -> public.company_details(id) [fk_company_subscription_company] ON DELETE CASCADE
public.company_subscription(subscription_plan_id) -> public.subscription_plans(id) [fk_company_subscription_plan] ON DELETE RESTRICT
public.email_verification_tokens(global_user_id) -> public.global_users(id) [fk_evt_global_user] ON DELETE CASCADE
public.role_permissions(action_id) -> public.actions(id) [fk_rp_action] ON DELETE CASCADE
public.role_permissions(module_id) -> public.modules(id) [fk_rp_module] ON DELETE CASCADE
public.role_permissions(role_id) -> public.roles(id) [fk_rp_role] ON DELETE CASCADE
public.tenant_registry(company_id) -> public.company_details(id) [fk_tenant_registry_company]
purchase_bill_attachments(bill_id) -> purchase_bills(id) [fk_pba_bill] ON DELETE CASCADE
purchase_bill_audit_logs(bill_id) -> purchase_bills(id) [fk_pbal_bill] ON DELETE CASCADE
purchase_bill_lines(bill_id) -> purchase_bills(id) [fk_pbl_bill] ON DELETE CASCADE
purchase_order_item(product_id) -> inventory_products(id) [fk_poi_product]
purchase_order_item(purchase_order_id) -> purchase_order(id) [fk_poi_purchase_order] ON DELETE CASCADE
purchase_order(branch_id) -> branches(id) [fk_po_branch]
purchase_order(vendor_id) -> vendors(id) [fk_po_vendor]
quotation_attachments(quotation_id) -> quotations(id) [fk_qa_quotation] ON DELETE CASCADE
quotation_audit_logs(quotation_id) -> quotations(id) [fk_qal_quotation] ON DELETE CASCADE
quotation_locations(branch_id) -> branches(id) [fk_ql_branch] ON DELETE RESTRICT
quotation_locations(quotation_id) -> quotations(id) [fk_ql_quotation] ON DELETE CASCADE
quotation_product_lines(product_id) -> inventory_products(id) [fk_qpl_product] ON DELETE RESTRICT
quotation_product_lines(quotation_id) -> quotations(id) [fk_qpl_quotation] ON DELETE CASCADE
quotation_service_lines(quotation_location_id) -> quotation_locations(id) [fk_qsl_location] ON DELETE CASCADE
quotation_service_lines(quotation_id) -> quotations(id) [fk_qsl_quotation] ON DELETE CASCADE
quotation_service_lines(service_id) -> services(id) [fk_qsl_service] ON DELETE RESTRICT
quotations(lead_id) -> leads(id) [fk_quot_lead] ON DELETE RESTRICT
quotations(prospect_id) -> quotation_prospects(id) [fk_quot_prospect] ON DELETE RESTRICT
quotations(revised_from_id) -> quotations(id) [fk_quot_revised_from] ON DELETE SET NULL
role_compensation_configuration(role_id) -> roles(id) [fk_rcc_role] ON DELETE CASCADE
role_permissions(action_id) -> actions(id) [fk_rp_action] ON DELETE CASCADE
role_permissions(module_id) -> modules(id) [fk_rp_module] ON DELETE CASCADE
role_permissions(role_id) -> roles(id) [fk_rp_role] ON DELETE CASCADE
salary_details(config_id) -> role_compensation_configuration(config_id) [fk_sd_config] ON DELETE CASCADE
sales_invoice_attachments(invoice_id) -> sales_invoices(id) [fk_sia_invoice] ON DELETE CASCADE
sales_invoice_audit_logs(invoice_id) -> sales_invoices(id) [fk_sial_invoice] ON DELETE CASCADE
sales_invoice_lines(invoice_id) -> sales_invoices(id) [fk_sil_invoice] ON DELETE CASCADE
sales_order_cancellation_logs(sales_order_id) -> sales_orders(id) [fk_socl_sales_order] ON DELETE CASCADE
sales_order_product_lines(sales_order_id) -> sales_orders(id) [fk_sopl_so] ON DELETE CASCADE
sales_order_site_chemicals(sales_order_site_id) -> sales_order_sites(id) [fk_sosc_site] ON DELETE CASCADE
sales_order_site_services(sales_order_site_id) -> sales_order_sites(id) [fk_soss_site] ON DELETE CASCADE
sales_order_sites(sales_order_id) -> sales_orders(id) [fk_sos_so] ON DELETE CASCADE
service_audit_logs(service_id) -> services(id) [fk_sal_service] ON DELETE CASCADE
service_category_area(service_category_id) -> service_categories(id) [fk_sca_category] ON DELETE CASCADE
service_category_area(service_sub_category_id) -> service_sub_categories(id) [fk_sca_sub_category] ON DELETE SET NULL
service_category_fixed(service_category_id) -> service_categories(id) [fk_scf_category] ON DELETE CASCADE
service_category_fixed(service_sub_category_id) -> service_sub_categories(id) [fk_scf_sub_category] ON DELETE SET NULL
service_category_inspection(service_category_id) -> service_categories(id) [fk_sci_category] ON DELETE CASCADE
service_custom_pricing_blocks(service_category_id) -> service_categories(id) [fk_scpb_category] ON DELETE CASCADE
service_custom_pricing_blocks(service_sub_category_id) -> service_sub_categories(id) [fk_scpb_sub_category] ON DELETE CASCADE
service_custom_pricing_fields(block_id) -> service_custom_pricing_blocks(id) [fk_scpf_block] ON DELETE CASCADE
service_execution_chemical_usages(service_execution_id) -> service_executions(id) [fk_sec_execution] ON DELETE CASCADE
service_execution_chemical_usages(inventory_product_id) -> inventory_products(id) [fk_sec_product] ON DELETE RESTRICT
service_execution_treatments(service_execution_id) -> service_executions(id) [fk_set_execution] ON DELETE CASCADE
service_execution_treatments(service_treatment_id) -> service_treatments(id) [fk_set_treatment] ON DELETE RESTRICT
service_executions(service_id) -> services(id) [fk_se_service] ON DELETE SET NULL
service_executions(task_id) -> tasks(id) [fk_se_task] ON DELETE CASCADE
service_products(inventory_product_id) -> inventory_products(id) [fk_sp_product] ON DELETE RESTRICT
service_products(service_id) -> services(id) [fk_sp_service] ON DELETE CASCADE
service_species(service_id) -> services(id) [fk_service_species_service] ON DELETE CASCADE
services_service_categories(service_category_id) -> service_categories(id) [fk_ssc_category] ON DELETE CASCADE
services_service_categories(service_id) -> services(id) [fk_ssc_service] ON DELETE CASCADE
services_service_category_area(service_category_area_id) -> service_category_area(id) [fk_ssca_area] ON DELETE CASCADE
services_service_category_area(service_id) -> services(id) [fk_ssca_service] ON DELETE CASCADE
services_service_category_fixed(service_category_fixed_id) -> service_category_fixed(id) [fk_sscf_fixed] ON DELETE CASCADE
services_service_category_fixed(service_id) -> services(id) [fk_sscf_service] ON DELETE CASCADE
services_service_category_inspection(service_category_inspection_id) -> service_category_inspection(id) [fk_ssci_inspection] ON DELETE CASCADE
services_service_category_inspection(service_id) -> services(id) [fk_ssci_service] ON DELETE CASCADE
services_service_custom_pricing_blocks(service_custom_pricing_block_id) -> service_custom_pricing_blocks(id) [fk_sscpb_block] ON DELETE CASCADE
services_service_custom_pricing_blocks(service_id) -> services(id) [fk_sscpb_service] ON DELETE CASCADE
services_service_pest_types(service_pest_type_id) -> service_pest_types(id) [fk_sspt_pest] ON DELETE CASCADE
services_service_pest_types(service_id) -> services(id) [fk_sspt_service] ON DELETE CASCADE
services_service_sub_categories(service_id) -> services(id) [fk_sssc_service] ON DELETE CASCADE
services_service_sub_categories(service_sub_category_id) -> service_sub_categories(id) [fk_sssc_sub] ON DELETE CASCADE
services_service_treatments(service_id) -> services(id) [fk_sst_service] ON DELETE CASCADE
services_service_treatments(service_treatment_id) -> service_treatments(id) [fk_sst_treatment] ON DELETE CASCADE
stock_approval_logs(request_id) -> stock_requests(id) [fk_stock_approval_log_request] ON DELETE CASCADE
stock_ledger(product_id) -> inventory_products(id) [fk_stock_ledger_product] ON DELETE RESTRICT
stock_ledger(product_id) -> inventory_products(id) [fk_stock_ledger_product] ON UPDATE CASCADE ON DELETE RESTRICT ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS central_stock_entries ( id BIGINT AUTO_INCREMENT PRIMARY KEY, entry_id VARCHAR(40) NOT NULL UNIQUE, product_id VARCHAR(50) NOT NULL, product_code VARCHAR(50) NOT NULL, product_name VARCHAR(255) NOT NULL, hsn_code VARCHAR(20), base_uom VARCHAR(30), total_qty INT NOT NULL, assets_qty INT NOT NULL DEFAULT 0, consumable_qty INT NOT NULL DEFAULT 0, resell_qty INT NOT NULL DEFAULT 0, asset_id_generation VARCHAR(15), asset_id_prefix VARCHAR(30), asset_sequence_start INT, assignment_type VARCHAR(30), default_assignment VARCHAR(50), supplier_name VARCHAR(200), purchase_order_ref VARCHAR(80), invoice_number VARCHAR(80) UNIQUE, invoice_date DATE, invoice_amount DECIMAL(14,2), tax_amount DECIMAL(14,2), total_with_tax DECIMAL(14,2), invoice_copy_data LONGBLOB, invoice_copy_file_name VARCHAR(255), invoice_copy_content_type VARCHAR(100), batch_number VARCHAR(80), manufacturing_date DATE, expiry_date DATE, status VARCHAR(30) NOT NULL DEFAULT 'ACTIVE', created_by VARCHAR(80), created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP, updated_by VARCHAR(80), updated_at DATETIME NULL ON UPDATE CURRENT_TIMESTAMP, deleted_by VARCHAR(80), deleted_at DATETIME, CONSTRAINT chk_central_qty_split CHECK (assets_qty + consumable_qty + resell_qty = total_qty), CONSTRAINT fk_central_entry_product FOREIGN KEY (product_id) REFERENCES inventory_products(id) ON UPDATE CASCADE ON DELETE RESTRICT ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS asset_units ( id BIGINT AUTO_INCREMENT PRIMARY KEY, asset_id VARCHAR(60) NOT NULL UNIQUE, product_id VARCHAR(50) NOT NULL, product_code VARCHAR(50) NOT NULL, product_name VARCHAR(255) NOT NULL, branch_id VARCHAR(30) NOT NULL, assigned_user_id BIGINT, assigned_to_name VARCHAR(160), assignment_mode VARCHAR(30), `condition` VARCHAR(30) NOT NULL DEFAULT 'GOOD', status VARCHAR(30) NOT NULL DEFAULT 'AVAILABLE', created_by VARCHAR(80), created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP, updated_by VARCHAR(80), updated_at DATETIME NULL ON UPDATE CURRENT_TIMESTAMP, CONSTRAINT fk_asset_unit_product FOREIGN KEY (product_id) REFERENCES inventory_products(id) ON UPDATE CASCADE ON DELETE RESTRICT ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS stock_requests ( id BIGINT AUTO_INCREMENT PRIMARY KEY, request_id VARCHAR(40) NOT NULL UNIQUE, request_type VARCHAR(30) NOT NULL, direction VARCHAR(15) NOT NULL, from_branch_id VARCHAR(30) NOT NULL, to_branch_id VARCHAR(30) NOT NULL, requested_by_user_id BIGINT, requested_by_name VARCHAR(160), priority VARCHAR(15) NOT NULL DEFAULT 'NORMAL', required_by_date DATE NOT NULL, purpose TEXT NOT NULL, notes_for_approver TEXT, sent_to TEXT, status VARCHAR(40) NOT NULL DEFAULT 'DRAFT', approval_type VARCHAR(30), alternative_source VARCHAR(30), dispatch_date DATE, expected_delivery_date DATE, carrier VARCHAR(120), lr_number VARCHAR(80), remarks TEXT, issue_type VARCHAR(40), issue_description TEXT, issue_resolution_status VARCHAR(40), created_by VARCHAR(80), created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP, updated_by VARCHAR(80), updated_at DATETIME NULL ON UPDATE CURRENT_TIMESTAMP, deleted_by VARCHAR(80), deleted_at DATETIME ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS stock_request_items ( id BIGINT AUTO_INCREMENT PRIMARY KEY, request_id BIGINT NOT NULL, product_id VARCHAR(50) NOT NULL, product_code VARCHAR(50) NOT NULL, product_name VARCHAR(255) NOT NULL, base_uom VARCHAR(30), assets_req_qty INT NOT NULL DEFAULT 0, consumable_req_qty INT NOT NULL DEFAULT 0, resell_req_qty INT NOT NULL DEFAULT 0, assets_appr_qty INT NOT NULL DEFAULT 0, consumable_appr_qty INT NOT NULL DEFAULT 0, resell_appr_qty INT NOT NULL DEFAULT 0, estimated_cost DECIMAL(14,2), tax_amount DECIMAL(14,2), item_purpose VARCHAR(250), alternative_source VARCHAR(30), CONSTRAINT fk_request_items_request FOREIGN KEY (request_id) REFERENCES stock_requests(id) ON DELETE CASCADE, CONSTRAINT fk_request_items_product FOREIGN KEY (product_id) REFERENCES inventory_products(id) ON UPDATE CASCADE ON DELETE RESTRICT ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS stock_transfers ( id BIGINT AUTO_INCREMENT PRIMARY KEY, transfer_id VARCHAR(40) NOT NULL UNIQUE, reference_request_id VARCHAR(40), from_branch_id VARCHAR(30) NOT NULL, to_branch_id VARCHAR(30) NOT NULL, transfer_type VARCHAR(20) NOT NULL, strategy VARCHAR(30), status VARCHAR(40) NOT NULL DEFAULT 'DRAFT', dispatch_date DATE, expected_delivery_date DATE, carrier VARCHAR(120), lr_number VARCHAR(80), remarks TEXT, created_by VARCHAR(80), created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP, updated_by VARCHAR(80), updated_at DATETIME NULL ON UPDATE CURRENT_TIMESTAMP ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS stock_transfer_items ( id BIGINT AUTO_INCREMENT PRIMARY KEY, transfer_id BIGINT NOT NULL, product_id VARCHAR(50) NOT NULL, product_code VARCHAR(50) NOT NULL, product_name VARCHAR(255) NOT NULL, assets_qty INT NOT NULL DEFAULT 0, consumable_qty INT NOT NULL DEFAULT 0, resell_qty INT NOT NULL DEFAULT 0, source_branch_id VARCHAR(30), CONSTRAINT fk_transfer_items_transfer FOREIGN KEY (transfer_id) REFERENCES stock_transfers(id) ON DELETE CASCADE, CONSTRAINT fk_transfer_items_product FOREIGN KEY (product_id) REFERENCES inventory_products(id) ON UPDATE CASCADE ON DELETE RESTRICT ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS stock_transfer_assets ( id BIGINT AUTO_INCREMENT PRIMARY KEY, transfer_id BIGINT NOT NULL, asset_id VARCHAR(60) NOT NULL, condition_at_dispatch VARCHAR(30), transfer_with VARCHAR(40), destination_user_id BIGINT, destination_user_name VARCHAR(160), condition_at_receipt VARCHAR(30), receipt_status VARCHAR(30), CONSTRAINT fk_transfer_assets_transfer FOREIGN KEY (transfer_id) REFERENCES stock_transfers(id) ON DELETE CASCADE ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS stock_movement_logs ( id BIGINT AUTO_INCREMENT PRIMARY KEY, reference_type VARCHAR(30) NOT NULL, reference_id VARCHAR(40) NOT NULL, branch_id VARCHAR(30), product_id VARCHAR(50), stock_type VARCHAR(20), quantity_delta INT NOT NULL DEFAULT 0, action VARCHAR(50) NOT NULL, remarks TEXT, created_by VARCHAR(80), created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS stock_approval_logs ( id BIGINT AUTO_INCREMENT PRIMARY KEY, request_id BIGINT NOT NULL, action VARCHAR(40) NOT NULL, previous_status VARCHAR(40), new_status VARCHAR(40), remarks TEXT, created_by VARCHAR(80), created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP, CONSTRAINT fk_approval_logs_request FOREIGN KEY (request_id) REFERENCES stock_requests(id) ON DELETE CASCADE ) ENGINE=InnoDB; CREATE INDEX idx_stock_ledger_product ON stock_ledger(product_id
stock_request_items(product_id) -> inventory_products(id) [fk_stock_request_item_product] ON DELETE RESTRICT
stock_request_items(request_id) -> stock_requests(id) [fk_stock_request_item_request] ON DELETE CASCADE
stock_request_recipients(stock_request_id) -> stock_requests(request_id) [fk_srr_stock_request] ON DELETE CASCADE
stock_request_recipients(recipient_user_id) -> users(id) [fk_srr_user] ON DELETE CASCADE
stock_request_recipients(request_id) -> stock_requests(id) [fk_stock_request_recipients_request] ON DELETE CASCADE
stock_transfer_assets(transfer_id) -> stock_transfers(id) [fk_stock_transfer_asset_transfer] ON DELETE CASCADE
stock_transfer_items(product_id) -> inventory_products(id) [fk_stock_transfer_item_product] ON DELETE RESTRICT
stock_transfer_items(transfer_id) -> stock_transfers(id) [fk_stock_transfer_item_transfer] ON DELETE CASCADE
support_ticket_activities(ticket_id) -> support_tickets(id) [fk_sta_ticket] ON DELETE CASCADE
support_ticket_activities(performed_by_user_id) -> users(id) [fk_sta_user] ON DELETE SET NULL
support_ticket_assignment_history(from_user_id) -> users(id) [fk_stah_from] ON DELETE SET NULL
support_ticket_assignment_history(ticket_id) -> support_tickets(id) [fk_stah_ticket] ON DELETE CASCADE
support_ticket_assignment_history(to_user_id) -> users(id) [fk_stah_to] ON DELETE RESTRICT
support_ticket_attachments(ticket_id) -> support_tickets(id) [fk_attach_ticket] ON DELETE CASCADE
support_ticket_tasks(task_id) -> tasks(id) [fk_stt_task] ON DELETE CASCADE
support_ticket_tasks(ticket_id) -> support_tickets(id) [fk_stt_ticket] ON DELETE CASCADE
support_tickets(assigned_user_id) -> users(id) [fk_st_assignee_user] ON DELETE SET NULL
support_tickets(branch_id) -> branches(id) [fk_st_branch] ON DELETE RESTRICT
support_tickets(customer_id) -> customers(id) [fk_st_customer] ON DELETE RESTRICT
support_tickets(related_task_id) -> tasks(id) [fk_st_related_task] ON DELETE SET NULL
support_tickets(sales_order_id) -> sales_orders(id) [fk_st_so] ON DELETE SET NULL
support_tickets(ticket_type_id) -> support_ticket_types(id) [fk_st_ticket_type] ON DELETE RESTRICT
task_customer_feedback(task_id) -> tasks(id) [fk_tcf_task] ON DELETE CASCADE
task_customer_feedback(technician_id) -> users(id) [fk_tcf_technician]
task_materials(task_id) -> tasks(id) [fk_tm_task] ON DELETE CASCADE
task_photos(task_id) -> tasks(id) [fk_tp_task] ON DELETE CASCADE
task_technicians(task_id) -> tasks(id) [fk_tt_task] ON DELETE CASCADE
task_technicians(user_id) -> users(id) [fk_tt_user]
tasks(ticket_id) -> support_tickets(id) [fk_tasks_support_ticket] ON DELETE SET NULL
technician_observation_hygiene_picks(hygiene_option_id) -> observation_options_hygiene(id) [fk_tohp_option] ON DELETE RESTRICT
technician_observation_hygiene_picks(section_id) -> technician_observation_sections(id) [fk_tohp_section] ON DELETE CASCADE
technician_observation_pest_picks(pest_option_id) -> observation_options_pest_sighting(id) [fk_topp_option] ON DELETE RESTRICT
technician_observation_pest_picks(section_id) -> technician_observation_sections(id) [fk_topp_section] ON DELETE CASCADE
technician_observation_sections(task_id) -> tasks(id) [fk_tos_task] ON DELETE CASCADE
technician_observation_structural_picks(structural_option_id) -> observation_options_structural(id) [fk_tosp_option] ON DELETE RESTRICT
technician_observation_structural_picks(section_id) -> technician_observation_sections(id) [fk_tosp_section] ON DELETE CASCADE
technician_tracking(task_id) -> tasks(id) [fk_technician_tracking_task] ON DELETE SET NULL
technician_tracking(user_id) -> users(id) [fk_technician_tracking_user]
user_additional_data(user_id) -> users(id) [fk_user_additional_user] ON DELETE CASCADE
user_branches(user_id) -> users(id) [fk_ub_user] ON DELETE CASCADE
user_documents(user_id) -> users(id) [fk_user_doc_user] ON DELETE CASCADE
user_leave_details(leave_approval_role_id) -> roles(id) [fk_user_leave_approval_role] ON DELETE SET NULL
user_leave_details(user_id) -> users(id) [fk_user_leave_user] ON DELETE CASCADE
user_permissions(action_id) -> actions(id) [fk_up_action] ON DELETE CASCADE
user_permissions(module_id) -> modules(id) [fk_up_module] ON DELETE CASCADE
user_permissions(user_id) -> users(id) [fk_up_user] ON DELETE CASCADE
user_salary_details(user_id) -> users(id) [fk_user_salary_user] ON DELETE CASCADE
users(reporting_manager_id) -> users(id) [fk_users_reporting_manager] ON DELETE SET NULL
users(role_id) -> roles(id) [fk_users_role] ON DELETE RESTRICT
voucher_allocations(voucher_id) -> vouchers(id) [fk_va_voucher] ON DELETE CASCADE
voucher_audit_logs(voucher_id) -> vouchers(id) [fk_val_voucher] ON DELETE CASCADE
voucher_journal_lines(voucher_id) -> vouchers(id) [fk_vjl_voucher] ON DELETE CASCADE
voucher_settlement_links(voucher_id) -> vouchers(id) [fk_vsl_voucher] ON DELETE CASCADE
```

## Tables

### actions

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V1__init_tenant.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| description | VARCHAR(255) | YES | — |
| id | BIGSERIAL | NO | — |
| label | VARCHAR(255) | YES | — |
| name | VARCHAR(100) | NO | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | name |

---

### asset_units

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V12__stock_management_core.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| asset_id | VARCHAR(60) | NO | — |
| assigned_to_name | VARCHAR(160) | YES | — |
| assigned_user_id | BIGINT | YES | — |
| assignment_mode | VARCHAR(30) | YES | — |
| branch_id | VARCHAR(30) | NO | — |
| condition | VARCHAR(30) | NO | 'GOOD' |
| created_at | TIMESTAMPTZ | NO | NOW() |
| created_by | VARCHAR(80) | YES | — |
| id | BIGSERIAL | NO | — |
| product_code | VARCHAR(50) | NO | — |
| product_id | VARCHAR(50) | NO | — |
| product_name | VARCHAR(255) | NO | — |
| status | VARCHAR(30) | NO | 'AVAILABLE' |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(80) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_asset_units_product | product_id | inventory_products(id) | ON DELETE RESTRICT |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | asset_id |

#### Check Constraints

```text
chk_asset_condition: condition IN ('NEW', 'GOOD', 'FAIR', 'DAMAGED', 'NEEDS_REPAIR')
chk_asset_status: status IN ('AVAILABLE', 'ISSUED', 'IN_TRANSIT', 'MAINTENANCE', 'RETIRED', 'QUARANTINE')
```

---

### bill_payment_allocations

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V38__payments_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| allocated_amount | NUMERIC(14,2) | NO | 0 |
| allocation_type | VARCHAR(30) | NO | — |
| bill_id | VARCHAR(50) | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| id | VARCHAR(50) | NO | — |
| running_balance_after | NUMERIC(14,2) | NO | 0 |
| voucher_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_bpa_bill | bill_id | purchase_bills(id) | ON DELETE CASCADE |
| fk_bpa_voucher | voucher_id | vouchers(id) | ON DELETE CASCADE |

#### Check Constraints

```text
allocation_type IN ('PAYMENT','ADVANCE_ADJUSTMENT','DEBIT_NOTE')
```

---

### branches

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V6__branch_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| address_line1 | TEXT | NO | — |
| branch_code | VARCHAR(3) | NO | — |
| branch_name | VARCHAR(100) | NO | — |
| branch_type | VARCHAR(20) | NO | — |
| city | VARCHAR(50) | NO | — |
| country | VARCHAR(50) | NO | — |
| created_at | TIMESTAMPTZ | YES | now() |
| created_by | VARCHAR(30) | YES | — |
| deleted_at | TIMESTAMPTZ | YES | — |
| deleted_by | VARCHAR(30) | YES | — |
| email | VARCHAR(100) | NO | — |
| id | VARCHAR(30) | NO | — |
| phone_number | VARCHAR(10) | NO | — |
| pincode | VARCHAR(10) | NO | — |
| state | VARCHAR(50) | NO | — |
| status | VARCHAR(20) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(30) | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | email |
| uq_branch_code | branch_code |

#### Check Constraints

```text
chk_branch_type: branch_type IN ('RETAIL','SERVICE','STORAGE_UNIT','OTHER')
chk_branch_status: status IN ('ACTIVE','INACTIVE')
```

#### Indexes

| Name | Unique | Columns |
|---|---:|---|
| idx_branch_status | NO | status |
| idx_branch_city | NO | city |
| idx_branch_state | NO | state |

---

### central_stock_entries

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V12__stock_management_core.sql, seravion-connect-backend/src/main/resources/db/migration/tenant/V55__central_stock_entry_supplier_assignee.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| asset_id_generation | VARCHAR(15) | YES | — |
| asset_id_prefix | VARCHAR(30) | YES | — |
| asset_sequence_start | INTEGER | YES | — |
| assets_qty | INTEGER | NO | 0 CHECK (assets_qty >= 0) |
| assignment_type | VARCHAR(30) | YES | — |
| base_uom | VARCHAR(30) | YES | — |
| batch_number | VARCHAR(80) | YES | — |
| consumable_qty | INTEGER | NO | 0 CHECK (consumable_qty >= 0) |
| created_at | TIMESTAMPTZ | NO | NOW() |
| created_by | VARCHAR(80) | YES | — |
| default_assignment | VARCHAR(50) | YES | — |
| deleted_at | TIMESTAMPTZ | YES | — |
| deleted_by | VARCHAR(80) | YES | — |
| entry_id | VARCHAR(40) | NO | — |
| expiry_date | DATE | YES | — |
| hsn_code | VARCHAR(20) | YES | — |
| id | BIGSERIAL | NO | — |
| invoice_amount | NUMERIC(14,2) | YES | — |
| invoice_copy_url | TEXT | YES | — |
| invoice_date | DATE | YES | — |
| invoice_number | VARCHAR(80) | YES | — |
| manufacturing_date | DATE | YES | — |
| product_code | VARCHAR(50) | NO | — |
| product_id | VARCHAR(50) | NO | — |
| product_name | VARCHAR(255) | NO | — |
| purchase_order_ref | VARCHAR(80) | YES | — |
| resell_qty | INTEGER | NO | 0 CHECK (resell_qty >= 0) |
| status | VARCHAR(30) | NO | 'ACTIVE' |
| supplier_name | VARCHAR(200) | YES | — |
| tax_amount | NUMERIC(14,2) | YES | — |
| total_qty | INTEGER | NO | — |
| total_with_tax | NUMERIC(14,2) | YES | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(80) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_central_stock_entries_product | product_id | inventory_products(id) | ON DELETE RESTRICT |
| fk_central_stock_entries_supplier | supplier_id | vendors(id) | ON DELETE SET NULL |
| fk_central_stock_entries_assignee_user | assignee_user_id | users(id) | ON DELETE SET NULL |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | entry_id |
| — | invoice_number |

#### Check Constraints

```text
total_qty > 0
assets_qty >= 0
consumable_qty >= 0
resell_qty >= 0
chk_central_stock_entry_qty_split: assets_qty + consumable_qty + resell_qty = total_qty
```

#### Indexes

| Name | Unique | Columns |
|---|---:|---|
| idx_central_stock_entries_supplier_id | NO | supplier_id |
| idx_central_stock_entries_assignee_user_id | NO | assignee_user_id |

---

### central_stock_ledger

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V64__central_stock_ledger_isolation.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| assets_qty | INTEGER | NO | 0 CHECK (assets_qty >= 0) |
| base_uom | VARCHAR(30) | YES | — |
| brand | VARCHAR(120) | YES | — |
| category | VARCHAR(50) | YES | — |
| consumable_qty | INTEGER | NO | 0 CHECK (consumable_qty >= 0) |
| created_at | TIMESTAMPTZ | NO | NOW() |
| created_by | VARCHAR(80) | YES | — |
| deleted_at | TIMESTAMPTZ | YES | — |
| deleted_by | VARCHAR(80) | YES | — |
| hsn_code | VARCHAR(20) | YES | — |
| id | BIGSERIAL | NO | — |
| in_transit_qty | INTEGER | NO | 0 CHECK (in_transit_qty >= 0) |
| product_code | VARCHAR(50) | NO | — |
| product_id | VARCHAR(50) | NO | — |
| product_name | VARCHAR(255) | NO | — |
| resell_qty | INTEGER | NO | 0 CHECK (resell_qty >= 0) |
| reserved_qty | INTEGER | NO | 0 CHECK (reserved_qty >= 0) |
| status | VARCHAR(30) | NO | 'AVAILABLE' |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(80) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_central_stock_ledger_product | product_id | inventory_products(id) | ON DELETE RESTRICT |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | product_id |

#### Check Constraints

```text
assets_qty >= 0
consumable_qty >= 0
resell_qty >= 0
in_transit_qty >= 0
reserved_qty >= 0
```

---

### coa_account_heads

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V40__chart_of_accounts_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| branch_id | VARCHAR(30) | YES | — |
| branch_scope | VARCHAR(10) | NO | 'ALL' CHECK (branch_scope IN ('ALL' |
| code | VARCHAR(30) | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| id | VARCHAR(50) | NO | — |
| is_postable | BOOLEAN | NO | TRUE |
| name | VARCHAR(150) | NO | — |
| nature | VARCHAR(2) | NO | — |
| parent_head_id | VARCHAR(50) | YES | — |
| primary_group | VARCHAR(20) | NO | — |
| status | VARCHAR(20) | NO | 'ACTIVE' CHECK (status IN ('ACTIVE' |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_coa_parent | parent_head_id | coa_account_heads(id) | ON DELETE RESTRICT |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | code |

#### Check Constraints

```text
primary_group IN ('ASSET','LIABILITY','INCOME','EXPENSE','CAPITAL')
nature IN ('DR','CR')
branch_scope IN ('ALL','BRANCH')
status IN ('ACTIVE','INACTIVE')
```

---

### company_profile_extension

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V50__view_profile.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| company_id | VARCHAR(50) | NO | — |
| created_at | TIMESTAMP | YES | CURRENT_TIMESTAMP |
| founding_year | INT | YES | — |
| id | UUID | NO | gen_random_uuid() |
| logo_url | TEXT | YES | — |
| tagline | VARCHAR(200) | YES | — |
| updated_at | TIMESTAMP | YES | — |
| website | VARCHAR(255) | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | company_id |

---

### contract_amendment_logs

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V61__contract_amendment_audit_log.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| amended_at | TIMESTAMPTZ | NO | now() |
| amended_by_name | VARCHAR(200) | YES | — |
| amended_by_user_id | BIGINT | YES | — |
| amendment_reason | VARCHAR(40) | NO | — |
| amendment_remarks | TEXT | YES | — |
| approval_required | BOOLEAN | NO | FALSE |
| change_summary | TEXT | YES | — |
| contract_id | VARCHAR(50) | NO | — |
| id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_cal_contract | contract_id | contracts(id) | ON DELETE CASCADE |

---

### contract_payment_lines

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V19__contract_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| amount | NUMERIC(14,2) | NO | — |
| contract_id | VARCHAR(50) | NO | — |
| due_date | DATE | YES | — |
| id | VARCHAR(50) | NO | — |
| locked | BOOLEAN | NO | FALSE |
| paid | BOOLEAN | NO | FALSE |
| period_description | VARCHAR(500) | YES | — |
| period_label | VARCHAR(50) | YES | — |
| sort_order | INTEGER | NO | 1 |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_cpl_contract | contract_id | contracts(id) | ON DELETE CASCADE |

---

### contract_sales_order_links

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V19__contract_management_module.sql, seravion-connect-backend/src/main/resources/db/migration/tenant/V20__sales_order_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| contract_id | VARCHAR(50) | NO | — |
| id | VARCHAR(50) | NO | — |
| period_label | VARCHAR(100) | YES | — |
| sales_order_date | DATE | YES | — |
| sales_order_number | VARCHAR(50) | NO | — |
| service_status | VARCHAR(30) | NO | 'PENDING' |
| so_status | VARCHAR(30) | NO | 'DRAFT' |
| so_value | NUMERIC(14,2) | NO | 0 |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_csol_contract | contract_id | contracts(id) | ON DELETE CASCADE |
| fk_csol_sales_order | sales_order_id | sales_orders(id) | ON DELETE SET NULL |

---

### contract_site_services

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V19__contract_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| annual_frequency | INTEGER | NO | 0 |
| contract_mode | VARCHAR(20) | NO | — |
| contract_site_id | VARCHAR(50) | NO | — |
| display_order | INTEGER | NO | 1 |
| frequency | VARCHAR(20) | YES | — |
| id | VARCHAR(50) | NO | — |
| preferred_days | VARCHAR(200) | YES | — |
| preferred_time_slot | VARCHAR(30) | NO | — |
| service_sale_value | NUMERIC(14,2) | NO | 0 |
| service_type_id | VARCHAR(50) | NO | — |
| service_type_name | VARCHAR(200) | NO | — |
| technician_team_id | VARCHAR(50) | NO | — |
| technician_team_name | VARCHAR(200) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_css_site | contract_site_id | contract_sites(id) | ON DELETE CASCADE |

#### Check Constraints

```text
contract_mode IN ('CONTRACT', 'ONE_TIME')
frequency IS NULL OR frequency IN ('WEEKLY', 'FORTNIGHTLY', 'MONTHLY', 'QUARTERLY', 'CUSTOM')
```

---

### contract_sites

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V19__contract_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| address | TEXT | YES | — |
| area_sqft | NUMERIC(12,2) | NO | 0 |
| category | VARCHAR(30) | NO | — |
| city | VARCHAR(100) | NO | — |
| contract_id | VARCHAR(50) | NO | — |
| country | VARCHAR(100) | NO | 'India' |
| display_order | INTEGER | NO | 1 |
| gma_site_id | VARCHAR(50) | YES | — |
| google_map_url | TEXT | YES | — |
| id | VARCHAR(50) | NO | — |
| site_gross_margin | NUMERIC(5,2) | NO | 0 |
| site_name | VARCHAR(200) | NO | — |
| site_proposed_price_year | NUMERIC(14,2) | NO | 0 |
| site_total_cost_year | NUMERIC(14,2) | NO | 0 |
| state | VARCHAR(100) | NO | — |
| sub_category | VARCHAR(20) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_cs_contract | contract_id | contracts(id) | ON DELETE CASCADE |

---

### contract_termination_logs

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V62__contract_termination_audit_log.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| additional_remarks | VARCHAR(500) | YES | — |
| contract_id | VARCHAR(50) | NO | — |
| effective_closure_date | DATE | NO | — |
| id | VARCHAR(50) | NO | — |
| open_sales_orders_count | INT | NO | 0 |
| open_sales_orders_resolution | VARCHAR(40) | NO | — |
| reason_code | VARCHAR(40) | NO | — |
| terminated_at | TIMESTAMPTZ | NO | now() |
| terminated_by_name | VARCHAR(200) | YES | — |
| terminated_by_user_id | BIGINT | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_ctl_contract | contract_id | contracts(id) | ON DELETE CASCADE |

---

### contracts

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V19__contract_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| advance_payment_due_date | DATE | YES | — |
| branch_id | VARCHAR(30) | NO | — |
| contract_reference | VARCHAR(50) | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| custom_payment_description | TEXT | YES | — |
| customer_id | VARCHAR(50) | NO | — |
| customer_name | VARCHAR(255) | NO | — |
| duration_option | VARCHAR(30) | NO | — |
| end_date | DATE | NO | — |
| gma_original_total_sale | NUMERIC(14,2) | NO | 0 |
| gma_sheet_id | VARCHAR(50) | NO | — |
| id | VARCHAR(50) | NO | — |
| invoicing_frequency | VARCHAR(40) | NO | — |
| legal_notes | TEXT | YES | — |
| legal_sla_remarks | TEXT | YES | — |
| overall_gm_percent_snapshot | NUMERIC(5,2) | NO | 0 |
| payment_schedule_type | VARCHAR(40) | NO | — |
| renewal_type | VARCHAR(30) | YES | — |
| start_date | DATE | NO | — |
| status | VARCHAR(30) | NO | — |
| termination_effective_date | DATE | YES | — |
| termination_reason | VARCHAR(50) | YES | — |
| termination_remarks | VARCHAR(500) | YES | — |
| total_annual_cost_snapshot | NUMERIC(14,2) | NO | 0 |
| total_sale_value | NUMERIC(14,2) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |
| variance_requires_approval | BOOLEAN | NO | FALSE |

#### Check Constraints

```text
status IN ('DRAFT', 'ACTIVE', 'EXPIRED', 'TERMINATED')
duration_option IN ('SIX_MONTHS', 'ONE_YEAR', 'TWO_YEARS', 'THREE_YEARS', 'CUSTOM')
renewal_type IS NULL OR renewal_type IN ('AUTO_RENEW', 'MANUAL', 'NON_RENEWABLE')
```

---

### credit_notes

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V36__invoicing_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| cn_date | DATE | NO | — |
| cn_number | VARCHAR(50) | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(120) | YES | — |
| credit_amount | NUMERIC(14,2) | NO | — |
| id | VARCHAR(50) | NO | — |
| invoice_id | VARCHAR(50) | NO | — |
| other_reason | VARCHAR(500) | YES | — |
| reason | VARCHAR(40) | NO | — |
| remarks | VARCHAR(500) | YES | — |
| source | VARCHAR(30) | NO | — |
| status | VARCHAR(20) | NO | 'ISSUED' CHECK (status IN ('ISSUED' |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_cn_invoice | invoice_id | sales_invoices(id) | ON DELETE RESTRICT |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | cn_number |

#### Check Constraints

```text
reason IN ('PAYMENT_SETTLEMENT','PRICING_ERROR','SERVICE_ISSUE','FULL_CANCELLATION','OTHER')
credit_amount > 0
source IN ('MANUAL','AUTO_FROM_PAYMENT')
status IN ('ISSUED','CANCELLED')
```

---

### customer_audit_log

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V18__customer_management.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| changed_at | TIMESTAMP WITH TIME ZONE | YES | CURRENT_TIMESTAMP |
| changed_by | VARCHAR(100) | YES | — |
| customer_id | VARCHAR(50) | YES | — |
| field_name | VARCHAR(50) | NO | — |
| id | BIGSERIAL | NO | — |
| new_value | TEXT | YES | — |
| old_value | TEXT | YES | — |

---

### customers

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V18__customer_management.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| alternate_phone | VARCHAR(15) | YES | — |
| billing_address_line1 | TEXT | NO | — |
| billing_address_line2 | TEXT | YES | — |
| branch_id | VARCHAR(30) | NO | — |
| city | VARCHAR(50) | NO | — |
| contact_person | VARCHAR(100) | NO | — |
| country | VARCHAR(50) | YES | 'India' |
| created_at | TIMESTAMP WITH TIME ZONE | YES | CURRENT_TIMESTAMP |
| created_by | VARCHAR(100) | YES | — |
| customer_type | VARCHAR(20) | NO | — |
| designation | VARCHAR(100) | YES | — |
| email | VARCHAR(100) | NO | — |
| entry_mode | VARCHAR(20) | NO | — |
| finance_contact_email | VARCHAR(100) | YES | — |
| finance_contact_name | VARCHAR(100) | NO | — |
| finance_contact_phone | VARCHAR(15) | NO | — |
| full_name | VARCHAR(100) | NO | — |
| google_map_url | TEXT | YES | — |
| gst_number | VARCHAR(15) | YES | — |
| id | VARCHAR(50) | NO | — |
| industry_type | VARCHAR(50) | YES | — |
| is_deleted | BOOLEAN | YES | FALSE |
| lead_id | VARCHAR(50) | YES | — |
| pan_number | VARCHAR(10) | YES | — |
| phone | VARCHAR(15) | NO | — |
| pincode | VARCHAR(6) | NO | — |
| reason_for_deactivation | TEXT | YES | — |
| state | VARCHAR(50) | NO | — |
| status | VARCHAR(20) | YES | 'DRAFT' |
| updated_at | TIMESTAMP WITH TIME ZONE | YES | CURRENT_TIMESTAMP |
| updated_by | VARCHAR(100) | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | gst_number |
| — | phone |

---

### debit_notes

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V37__bills_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| bill_id | VARCHAR(50) | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(120) | YES | — |
| debit_amount | NUMERIC(14,2) | NO | — |
| dn_date | DATE | NO | — |
| dn_number | VARCHAR(50) | NO | — |
| id | VARCHAR(50) | NO | — |
| other_reason | VARCHAR(500) | YES | — |
| reason | VARCHAR(40) | NO | — |
| remarks | VARCHAR(500) | YES | — |
| source | VARCHAR(30) | NO | — |
| status | VARCHAR(20) | NO | 'ISSUED' CHECK (status IN ('ISSUED' |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_dn_bill | bill_id | purchase_bills(id) | ON DELETE RESTRICT |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | dn_number |

#### Check Constraints

```text
reason IN ('PAYMENT_SETTLEMENT','PURCHASE_RETURN','DISCOUNT','ERROR','OTHER')
debit_amount > 0
source IN ('MANUAL','AUTO_FROM_PAYMENT')
status IN ('ISSUED','CANCELLED')
```

---

### follow_ups

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V10__lead_followup_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| contact_mode | VARCHAR(30) | NO | — |
| created_at | TIMESTAMP | NO | — |
| created_by | VARCHAR(100) | YES | — |
| id | VARCHAR(50) | NO | — |
| interaction_summary | TEXT | NO | — |
| lead_id | VARCHAR(50) | NO | — |
| lost_reason | TEXT | YES | — |
| next_action_scheduled | BOOLEAN | YES | FALSE |
| next_follow_up_date | DATE | YES | — |
| next_follow_up_time | TIME | YES | — |
| reason_agenda | TEXT | YES | — |
| status_updated_to | VARCHAR(20) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_follow_ups_lead | lead_id | leads(id) | — |

---

### gma_audit_logs

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V17__gma_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| action | VARCHAR(50) | NO | — |
| action_at | TIMESTAMPTZ | NO | now() |
| gma_sheet_id | VARCHAR(50) | NO | — |
| id | VARCHAR(50) | NO | — |
| remarks | TEXT | YES | — |
| user_id | VARCHAR(50) | YES | — |
| user_name | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_gmaaud_sheet | gma_sheet_id | gma_sheets(id) | ON DELETE CASCADE |

#### Check Constraints

```text
action IN ( 'DRAFT_CREATED', 'SUBMITTED', 'APPROVED_AUTO', 'APPROVED_MANUAL', 'REJECTED', 'REVOKED' )
```

---

### gma_chemicals

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V17__gma_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| cost_per_month | NUMERIC(14,2) | NO | — |
| cost_per_visit | NUMERIC(14,2) | NO | — |
| cost_per_year | NUMERIC(14,2) | NO | — |
| coverage_sqft | NUMERIC(10,2) | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| display_order | INTEGER | NO | 1 |
| gma_service_id | VARCHAR(50) | NO | — |
| id | VARCHAR(50) | NO | — |
| price_per_uom | NUMERIC(14,4) | NO | — |
| product_code | VARCHAR(100) | NO | — |
| product_id | VARCHAR(50) | NO | — |
| product_name | VARCHAR(200) | NO | — |
| required_qty_per_visit | NUMERIC(10,3) | NO | — |
| uom | VARCHAR(50) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_gmachem_service | gma_service_id | gma_services(id) | ON DELETE CASCADE |
| fk_gmachem_product | product_id | inventory_products(id) | ON DELETE RESTRICT |

#### Check Constraints

```text
coverage_sqft > 0
required_qty_per_visit > 0
```

---

### gma_prospects

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V17__gma_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| address | TEXT | NO | — |
| city | VARCHAR(100) | NO | — |
| company_name | VARCHAR(100) | YES | — |
| country | VARCHAR(100) | NO | 'India' |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| email | VARCHAR(255) | YES | — |
| full_name | VARCHAR(200) | NO | — |
| google_map_url | TEXT | YES | — |
| id | VARCHAR(50) | NO | — |
| phone | VARCHAR(15) | NO | — |
| pincode | VARCHAR(10) | YES | — |
| state | VARCHAR(100) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |

---

### gma_services

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V17__gma_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| annual_frequency | INTEGER | NO | — |
| chemical_cost_month | NUMERIC(14,2) | NO | 0 |
| chemical_cost_year | NUMERIC(14,2) | NO | 0 |
| created_at | TIMESTAMPTZ | NO | now() |
| display_order | INTEGER | NO | 1 |
| frequency | VARCHAR(20) | YES | — |
| gma_site_id | VARCHAR(50) | NO | — |
| hours_per_visit | NUMERIC(5,2) | NO | 0 |
| id | VARCHAR(50) | NO | — |
| manpower_cost_month | NUMERIC(14,2) | NO | 0 |
| manpower_cost_year | NUMERIC(14,2) | NO | 0 |
| rate_per_hour | NUMERIC(14,2) | NO | 0 |
| rate_per_visit | NUMERIC(14,2) | NO | 0 |
| service_mode | VARCHAR(20) | NO | — |
| service_type_id | VARCHAR(50) | NO | — |
| service_type_name | VARCHAR(200) | NO | — |
| service_visit_cost_month | NUMERIC(14,2) | NO | 0 |
| service_visit_cost_year | NUMERIC(14,2) | NO | 0 |
| total_service_cost_year | NUMERIC(14,2) | NO | 0 |
| updated_at | TIMESTAMPTZ | YES | — |
| visits_per_month | NUMERIC(5,2) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_gmasvc_site | gma_site_id | gma_sites(id) | ON DELETE CASCADE |
| fk_gmasvc_type | service_type_id | services(id) | ON DELETE RESTRICT |

#### Check Constraints

```text
service_mode IN ('CONTRACT', 'ONE_TIME')
frequency IN ('WEEKLY', 'FORTNIGHTLY', 'MONTHLY', 'QUARTERLY', 'CUSTOM')
annual_frequency > 0
```

---

### gma_sheet_approver_roles

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V17__gma_management_module.sql
- Primary key: gma_sheet_id, role_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| gma_sheet_id | VARCHAR(50) | NO | — |
| role_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_gsar_sheet | gma_sheet_id | gma_sheets(id) | ON DELETE CASCADE |
| fk_gsar_role | role_id | roles(id) | ON DELETE CASCADE |

---

### gma_sheets

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V17__gma_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| approval_remarks | TEXT | YES | — |
| approved_on | TIMESTAMPTZ | YES | — |
| approver_id | BIGINT | YES | — |
| branch_id | VARCHAR(30) | NO | — |
| contract_duration | VARCHAR(20) | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| customer_id | VARCHAR(50) | YES | — |
| deadline | TIMESTAMPTZ | YES | — |
| deleted_at | TIMESTAMPTZ | YES | — |
| deleted_by | VARCHAR(100) | YES | — |
| gm_with_doc | NUMERIC(5,2) | NO | 0 |
| gm_without_doc | NUMERIC(5,2) | NO | 0 |
| id | VARCHAR(50) | NO | — |
| is_deleted | BOOLEAN | NO | FALSE |
| lead_id | VARCHAR(50) | YES | — |
| overall_gross_margin | NUMERIC(5,2) | NO | 0 |
| prepared_by_id | BIGINT | NO | — |
| proposed_start_date | DATE | NO | — |
| prospect_id | VARCHAR(50) | YES | — |
| remarks | TEXT | YES | — |
| source_type | VARCHAR(20) | NO | — |
| status | VARCHAR(20) | NO | 'DRAFT' CHECK (status IN ('DRAFT' |
| submitted_on | TIMESTAMPTZ | YES | — |
| total_annual_cost | NUMERIC(14,2) | NO | 0 |
| total_annual_price | NUMERIC(14,2) | NO | 0 |
| total_surcharge_cost | NUMERIC(14,2) | NO | 0 |
| total_visits_per_month | NUMERIC(10,2) | NO | 0 |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_gma_lead | lead_id | leads(id) | ON DELETE RESTRICT |
| fk_gma_prospect | prospect_id | gma_prospects(id) | ON DELETE RESTRICT |
| fk_gma_branch | branch_id | branches(id) | ON DELETE RESTRICT |

#### Check Constraints

```text
source_type IN ('FROM_LEAD', 'FROM_CUSTOMER', 'ADD_NEW')
contract_duration IN ('SIX_MONTHS', 'ONE_YEAR', 'TWO_YEARS', 'THREE_YEARS', 'CUSTOM')
status IN ('DRAFT', 'PENDING', 'APPROVED', 'REJECTED')
chk_gma_source_xor: ( (CASE WHEN lead_id IS NOT NULL THEN 1 ELSE 0 END) + (CASE WHEN customer_id IS NOT NULL THEN 1 ELSE 0 END) + (CASE WHEN prospect_id IS NOT NULL THEN 1 ELSE 0 END) ) = 1
```

---

### gma_sites

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V17__gma_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| address | TEXT | YES | — |
| area_sqft | NUMERIC(10,2) | NO | — |
| category | VARCHAR(30) | NO | — |
| city | VARCHAR(100) | NO | — |
| cost_per_document | NUMERIC(10,2) | NO | 0 |
| country | VARCHAR(100) | NO | 'India' |
| created_at | TIMESTAMPTZ | NO | now() |
| display_order | INTEGER | NO | 1 |
| docs_per_month | INTEGER | NO | 0 |
| documentation_cost_applicable | BOOLEAN | NO | FALSE |
| documentation_cost_year | NUMERIC(14,2) | NO | 0 |
| gma_sheet_id | VARCHAR(50) | NO | — |
| google_map_url | TEXT | YES | — |
| id | VARCHAR(50) | NO | — |
| site_gross_margin | NUMERIC(5,2) | NO | 0 |
| site_name | VARCHAR(200) | NO | — |
| site_proposed_price_year | NUMERIC(14,2) | NO | 0 |
| site_total_cost_year | NUMERIC(14,2) | NO | 0 |
| state | VARCHAR(100) | NO | — |
| sub_category | VARCHAR(20) | NO | — |
| surcharge_cost | NUMERIC(14,2) | NO | 0 |
| updated_at | TIMESTAMPTZ | YES | — |
| weekend_night_surcharge_applicable | BOOLEAN | NO | FALSE |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_gmasite_sheet | gma_sheet_id | gma_sheets(id) | ON DELETE CASCADE |

#### Check Constraints

```text
category IN ('RESIDENTIAL', 'COMMERCIAL', 'INDUSTRIAL')
sub_category IN ('INTERNAL', 'EXTERNAL')
area_sqft > 0
```

---

### hiring_request_branches

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V9__hiring_request_module.sql
- Primary key: hiring_request_id, branch_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| branch_id | VARCHAR(30) | NO | — |
| hiring_request_id | VARCHAR(30) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_hrb_hiring_request | hiring_request_id | hiring_requests(id) | ON DELETE CASCADE |

---

### hiring_request_recipients

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V9__hiring_request_module.sql
- Primary key: hiring_request_id, recipient_user_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| hiring_request_id | VARCHAR(30) | NO | — |
| recipient_user_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_hrr_hiring_request | hiring_request_id | hiring_requests(id) | ON DELETE CASCADE |
| fk_hrr_user | recipient_user_id | users(id) | ON DELETE CASCADE |

---

### hiring_requests

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V9__hiring_request_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| additional_remarks | VARCHAR(500) | YES | — |
| converted_user_id | BIGINT | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| department | VARCHAR(150) | NO | — |
| designation | VARCHAR(150) | NO | — |
| employment_type | VARCHAR(20) | NO | — |
| expected_date_of_joining | DATE | NO | — |
| hiring_reason | TEXT | NO | — |
| id | VARCHAR(30) | NO | — |
| job_description | TEXT | YES | — |
| number_of_positions | INT | NO | — |
| proposed_role_id | BIGINT | NO | — |
| proposed_salary | DECIMAL(15,2) | YES | — |
| rejection_reason | TEXT | YES | — |
| requested_by_user_id | BIGINT | NO | — |
| review_date | DATE | YES | — |
| reviewed_by_user_id | BIGINT | YES | — |
| status | VARCHAR(20) | NO | 'PENDING' CHECK (status IN ('PENDING' |
| submitted_at | TIMESTAMPTZ | YES | — |
| supporting_document_path | VARCHAR(500) | YES | — |
| updated_at | TIMESTAMPTZ | NO | now() |
| updated_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_hiring_requested_by | requested_by_user_id | users(id) | ON DELETE RESTRICT |
| fk_hiring_proposed_role | proposed_role_id | roles(id) | ON DELETE RESTRICT |
| fk_hiring_reviewed_by | reviewed_by_user_id | users(id) | ON DELETE SET NULL |
| fk_hiring_converted_user | converted_user_id | users(id) | ON DELETE SET NULL |

#### Check Constraints

```text
employment_type IN ('PERMANENT', 'CONTRACT', 'INTERN')
number_of_positions BETWEEN 1 AND 100
status IN ('PENDING', 'APPROVED', 'REJECTED', 'CONVERTED')
```

---

### hrm_attendance_day

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V28__hrm_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| attendance_date | DATE | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| id | BIGSERIAL | NO | — |
| notes | VARCHAR(1000) | YES | — |
| punch_in_at | TIMESTAMPTZ | YES | — |
| punch_out_at | TIMESTAMPTZ | YES | — |
| source | VARCHAR(20) | NO | 'MANUAL' CHECK (source IN ('TASK' |
| status | VARCHAR(20) | NO | — |
| tasks_assigned | INT | NO | 0 |
| tasks_completed | INT | NO | 0 |
| tasks_pending | INT | NO | 0 |
| total_minutes | INT | YES | — |
| updated_at | TIMESTAMPTZ | NO | now() |
| updated_by | VARCHAR(100) | YES | — |
| user_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_hrm_attendance_user | user_id | users(id) | ON DELETE CASCADE |

#### Unique Constraints

| Name | Columns |
|---|---|
| uk_hrm_attendance_user_date | user_id, attendance_date |

#### Check Constraints

```text
status IN ('PRESENT', 'ABSENT', 'LEAVE', 'WEEK_OFF', 'HOLIDAY', 'HALF_DAY')
source IN ('TASK', 'AUTO', 'MANUAL', 'UPLOAD')
```

---

### hrm_holidays

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V28__hrm_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| branch_id | VARCHAR(30) | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| holiday_date | DATE | NO | — |
| id | BIGSERIAL | NO | — |
| name | VARCHAR(200) | NO | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| uk_hrm_holiday | holiday_date, branch_id |

---

### hrm_leave_request

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V28__hrm_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| description | TEXT | NO | — |
| from_date | DATE | NO | — |
| id | BIGSERIAL | NO | — |
| leave_code | VARCHAR(30) | NO | — |
| leave_type | VARCHAR(10) | NO | — |
| rejection_reason | TEXT | YES | — |
| reviewed_at | TIMESTAMPTZ | YES | — |
| reviewed_by_user_id | BIGINT | YES | — |
| status | VARCHAR(20) | NO | — |
| to_date | DATE | NO | — |
| updated_at | TIMESTAMPTZ | NO | now() |
| updated_by | VARCHAR(100) | YES | — |
| user_id | BIGINT | NO | — |
| working_days | INT | NO | 0 |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_hrm_leave_user | user_id | users(id) | ON DELETE CASCADE |
| fk_hrm_leave_reviewed_by | reviewed_by_user_id | users(id) | ON DELETE SET NULL |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | leave_code |

#### Check Constraints

```text
leave_type IN ('CL', 'SL', 'PL')
status IN ('PENDING', 'APPROVED', 'REJECTED')
chk_hrm_leave_dates: from_date <= to_date
```

---

### hrm_salary_month

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V28__hrm_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| basic_salary | NUMERIC(12,2) | NO | 0.00 |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| deductions | NUMERIC(12,2) | NO | 0.00 |
| esi | NUMERIC(12,2) | NO | 0.00 |
| gross_salary | NUMERIC(12,2) | NO | 0.00 |
| holiday_days_worked | INT | NO | 0 |
| holiday_incentive_amt | NUMERIC(12,2) | NO | 0.00 |
| hra | NUMERIC(12,2) | NO | 0.00 |
| id | BIGSERIAL | NO | — |
| incentive | NUMERIC(12,2) | NO | 0.00 |
| net_salary | NUMERIC(12,2) | NO | 0.00 |
| ot_amount | NUMERIC(12,2) | NO | 0.00 |
| ot_hours | NUMERIC(8,2) | NO | 0.00 |
| other_allowance | NUMERIC(12,2) | NO | 0.00 |
| other_deductions | NUMERIC(12,2) | NO | 0.00 |
| paid_at | TIMESTAMPTZ | YES | — |
| paid_by_user_id | BIGINT | YES | — |
| payment_date | DATE | YES | — |
| payment_status | VARCHAR(20) | NO | 'UNPAID' CHECK (payment_status IN ('PAID' |
| pf | NUMERIC(12,2) | NO | 0.00 |
| reason | TEXT | YES | — |
| salary_month | INT | NO | — |
| salary_year | INT | NO | — |
| tds | NUMERIC(12,2) | NO | 0.00 |
| total_deductions | NUMERIC(12,2) | NO | 0.00 |
| updated_at | TIMESTAMPTZ | NO | now() |
| updated_by | VARCHAR(100) | YES | — |
| user_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_hrm_salary_user | user_id | users(id) | ON DELETE CASCADE |
| fk_hrm_salary_paid_by | paid_by_user_id | users(id) | ON DELETE SET NULL |

#### Unique Constraints

| Name | Columns |
|---|---|
| uk_hrm_salary_user_month | user_id, salary_year, salary_month |

#### Check Constraints

```text
salary_month BETWEEN 1 AND 12
payment_status IN ('PAID', 'UNPAID', 'DUE')
```

---

### hrm_salary_slip

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V28__hrm_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| file_path | VARCHAR(500) | NO | — |
| generated_at | TIMESTAMPTZ | NO | now() |
| generated_by | VARCHAR(100) | YES | — |
| id | BIGSERIAL | NO | — |
| salary_month_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_hrm_salary_slip_month | salary_month_id | hrm_salary_month(id) | ON DELETE CASCADE |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | salary_month_id |

---

### hsn_code_tax_types

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V5__tax_hsn_module.sql
- Primary key: —

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| hsn_code_id | BIGINT | NO | — |
| tax_type_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_hsn_code | hsn_code_id | hsn_codes(id) | — |
| fk_tax_type | tax_type_id | tax_types(id) | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| uq_hsn_tax | hsn_code_id, tax_type_id |

---

### hsn_codes

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V5__tax_hsn_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| chapter | VARCHAR(20) | YES | — |
| created_at | TIMESTAMPTZ | NO | NOW() |
| created_by | VARCHAR(50) | NO | — |
| deleted_at | TIMESTAMPTZ | YES | — |
| deleted_by | VARCHAR(50) | YES | — |
| description | TEXT | NO | — |
| effective_from | DATE | NO | — |
| hsn_code | VARCHAR(8) | NO | — |
| id | BIGINT | NO | — |
| product_category | VARCHAR(20) | NO | — |
| product_subcategory | VARCHAR(20) | YES | — |
| status | VARCHAR(20) | NO | 'ACTIVE' |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(50) | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| uq_hsn_code | hsn_code |

#### Check Constraints

```text
chk_hsn_code_format: hsn_code ~ '^[0-9]{4}$|^[0-9]{6}$|^[0-9]{8}$'
chk_hsn_status: status IN ('ACTIVE', 'INACTIVE')
chk_product_category: product_category IN ('ASSET', 'CONSUMABLES', 'RESALE')
chk_product_subcategory: product_subcategory IN ('CHEMICALS', 'MACHINE', 'SPRAYER', 'POWDER')
```

---

### incentive_overtime_details

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V7__role_salary_leave_config.sql
- Primary key: config_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| config_id | VARCHAR(50) | NO | — |
| custom_shift_from | TIME | YES | — |
| custom_shift_to | TIME | YES | — |
| holiday_work_amount | NUMERIC(12,2) | YES | — |
| holiday_work_applicable | BOOLEAN | YES | FALSE |
| holiday_work_type | VARCHAR(20) | YES | — |
| max_ot_hours_per_month | INT | YES | — |
| overtime_applicable | BOOLEAN | YES | FALSE |
| overtime_shift_incentive | NUMERIC(12,2) | YES | — |
| overtime_shift_type | VARCHAR(50) | YES | — |
| overtime_type | VARCHAR(20) | YES | — |
| per_hour_incentive_pay | NUMERIC(12,2) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_iod_config | config_id | role_compensation_configuration(config_id) | ON DELETE CASCADE |

#### Check Constraints

```text
holiday_work_type IN ('FIXED', 'PER_DAY', 'PER_HOUR')
overtime_type IN ('PER_HOUR', 'PER_DAY')
overtime_shift_type IN ('NIGHT', 'NORMAL', 'CUSTOM')
```

---

### inventory_brands

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V71__inventory_brand_master.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| id | VARCHAR(50) | NO | — |
| is_active | BOOLEAN | NO | TRUE |
| name | VARCHAR(200) | NO | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | name |

---

### inventory_product_media

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V11__inventory_product.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| content_type | VARCHAR(100) | YES | — |
| created_at | TIMESTAMP | YES | CURRENT_TIMESTAMP |
| created_by | VARCHAR(255) | YES | — |
| file_data | TEXT | YES | — |
| file_name | VARCHAR(100) | YES | — |
| file_url | TEXT | YES | — |
| id | VARCHAR(50) | NO | — |
| is_primary | BOOLEAN | YES | FALSE |
| product_id | VARCHAR(100) | YES | — |

---

### inventory_products

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V11__inventory_product.sql, seravion-connect-backend/src/main/resources/db/migration/tenant/V72__inventory_products_brand_fk.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| barcode | VARCHAR(100) | YES | — |
| base_price | DOUBLE PRECISION | YES | — |
| base_uom | VARCHAR(100) | YES | — |
| brand | VARCHAR(100) | YES | — |
| category | VARCHAR(100) | YES | — |
| created_at | TIMESTAMPTZ | YES | now() |
| created_by | VARCHAR(100) | YES | — |
| deleted_at | TIMESTAMPTZ | YES | — |
| deleted_by | VARCHAR(100) | YES | — |
| description | TEXT | YES | — |
| hsn_code | VARCHAR(100) | YES | — |
| id | VARCHAR(50) | NO | — |
| package_type | VARCHAR(100) | YES | — |
| product_code | VARCHAR(100) | YES | — |
| product_name | VARCHAR(100) | NO | — |
| purchase_price | DOUBLE PRECISION | YES | — |
| quantity_per_package | DOUBLE PRECISION | YES | — |
| secondary_uom | VARCHAR(100) | YES | — |
| selling_price | DOUBLE PRECISION | YES | — |
| status | VARCHAR(100) | YES | 'ACTIVE' |
| sub_type | VARCHAR(100) | YES | — |
| tax_amount | DOUBLE PRECISION | YES | — |
| total_cost | DOUBLE PRECISION | YES | — |
| unit_packaging_brand | VARCHAR(100) | YES | — |
| units_per_package | DOUBLE PRECISION | YES | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |
| variant_name | VARCHAR(100) | NO | — |
| variant_package_type | VARCHAR(100) | YES | — |
| variant_quantity | DOUBLE PRECISION | YES | — |
| variant_sku | VARCHAR(100) | YES | — |
| variant_status | VARCHAR(100) | YES | 'ACTIVE' |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_inventory_products_brand | brand_id | inventory_brands(id) | ON DELETE RESTRICT |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | product_code |
| — | variant_sku |

---

### invoice_payment_allocations

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V38__payments_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| allocated_amount | NUMERIC(14,2) | NO | 0 |
| allocation_type | VARCHAR(30) | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| id | VARCHAR(50) | NO | — |
| invoice_id | VARCHAR(50) | NO | — |
| running_balance_after | NUMERIC(14,2) | NO | 0 |
| voucher_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_ipa_invoice | invoice_id | sales_invoices(id) | ON DELETE CASCADE |
| fk_ipa_voucher | voucher_id | vouchers(id) | ON DELETE CASCADE |

#### Check Constraints

```text
allocation_type IN ('PAYMENT','ADVANCE_ADJUSTMENT','CREDIT_NOTE')
```

---

### lead_audit_logs

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V10__lead_followup_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| changed_at | TIMESTAMP | NO | — |
| changed_by | VARCHAR(100) | YES | — |
| field_changed | VARCHAR(100) | NO | — |
| id | VARCHAR(50) | NO | — |
| lead_id | VARCHAR(50) | NO | — |
| new_value | TEXT | YES | — |
| old_value | TEXT | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_audit_lead | lead_id | leads(id) | — |

---

### leads

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V10__lead_followup_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| alternate_number | VARCHAR(15) | YES | — |
| assigned_to_id | BIGINT | YES | — |
| branch_id | VARCHAR(30) | NO | — |
| budget_range | VARCHAR(50) | YES | — |
| created_at | TIMESTAMP | NO | — |
| created_by | VARCHAR(100) | YES | — |
| email_id | VARCHAR(255) | YES | — |
| gma_status | VARCHAR(20) | NO | — |
| id | VARCHAR(50) | NO | — |
| lead_date | DATE | NO | — |
| lead_description | TEXT | NO | — |
| lead_name | VARCHAR(200) | NO | — |
| lead_type | VARCHAR(20) | NO | — |
| lost_reason | TEXT | YES | — |
| mobile_number | VARCHAR(15) | NO | — |
| next_follow_up_date | DATE | YES | — |
| priority | VARCHAR(20) | NO | — |
| service_type | VARCHAR(50) | YES | — |
| source | VARCHAR(50) | NO | — |
| status | VARCHAR(20) | NO | — |
| updated_at | TIMESTAMP | NO | — |
| updated_by | VARCHAR(100) | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| uk_leads_mobile | mobile_number |
| uk_leads_email | email_id |

---

### leave_configuration

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V7__role_salary_leave_config.sql
- Primary key: config_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| annual_leave_allocation | INT | YES | 0 |
| carry_forward_allowed | BOOLEAN | YES | FALSE |
| casual_leave | INT | YES | 0 |
| config_id | VARCHAR(50) | NO | — |
| leave_approval_role_id | BIGINT | NO | — |
| leave_reset_cycle | VARCHAR(20) | NO | — |
| leave_reset_from | DATE | YES | — |
| leave_reset_to | DATE | YES | — |
| max_carry_forward_days | INT | YES | — |
| paid_leave | INT | YES | 0 |
| sick_leave | INT | YES | 0 |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_lc_config | config_id | role_compensation_configuration(config_id) | ON DELETE CASCADE |
| fk_lc_role | leave_approval_role_id | roles(id) | ON DELETE RESTRICT |

#### Check Constraints

```text
leave_reset_cycle IN ('YEARLY', 'MONTHLY', 'CUSTOM')
```

---

### ledger_entries

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V39__ledger_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| branch_id | VARCHAR(30) | YES | — |
| cr_amount | NUMERIC(14,2) | NO | 0 |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(120) | YES | — |
| dr_amount | NUMERIC(14,2) | NO | 0 |
| entry_date | DATE | NO | — |
| id | VARCHAR(50) | NO | — |
| ledger_id | VARCHAR(50) | NO | — |
| narration | VARCHAR(500) | YES | — |
| posting_status | VARCHAR(20) | NO | 'POSTED' CHECK (posting_status IN ('POSTED' |
| ref_id | VARCHAR(50) | NO | — |
| ref_type | VARCHAR(30) | NO | — |
| voucher_no | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_le_ledger | ledger_id | ledgers(id) | ON DELETE RESTRICT |

#### Check Constraints

```text
ref_type IN ('INVOICE','BILL','RECEIPT','PAYMENT','CREDIT_NOTE','DEBIT_NOTE','JOURNAL','CONTRA')
posting_status IN ('POSTED','REVERSED')
chk_le_one_side: (CASE WHEN dr_amount > 0 THEN 1 ELSE 0 END) + (CASE WHEN cr_amount > 0 THEN 1 ELSE 0 END) = 1
```

---

### ledgers

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V39__ledger_management_module.sql, seravion-connect-backend/src/main/resources/db/migration/tenant/V40__chart_of_accounts_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| account_head_id | VARCHAR(50) | NO | — |
| branch_id | VARCHAR(30) | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| credit_limit | NUMERIC(14,2) | NO | 0 |
| credit_period_days | INTEGER | YES | — |
| id | VARCHAR(50) | NO | — |
| ledger_code | VARCHAR(50) | NO | — |
| ledger_name | VARCHAR(255) | NO | — |
| ledger_type | VARCHAR(20) | NO | — |
| linked_customer_id | VARCHAR(50) | YES | — |
| linked_vendor_id | VARCHAR(50) | YES | — |
| opening_as_on | DATE | NO | — |
| opening_balance | NUMERIC(14,2) | NO | 0 |
| opening_balance_type | VARCHAR(2) | NO | — |
| status | VARCHAR(20) | NO | 'ACTIVE' CHECK (status IN ('ACTIVE' |
| tds_applicable | BOOLEAN | NO | FALSE |
| tds_section | VARCHAR(20) | YES | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_ledgers_account_head | account_head_id | coa_account_heads(id) | ON DELETE RESTRICT |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | ledger_code |

#### Check Constraints

```text
ledger_type IN ('CUSTOMER','VENDOR','BANK','CASH','INTERNAL','TAX')
opening_balance_type IN ('DR','CR')
status IN ('ACTIVE','INACTIVE')
```

---

### modules

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V1__init_tenant.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| description | VARCHAR(255) | YES | — |
| id | BIGSERIAL | NO | — |
| label | VARCHAR(255) | YES | — |
| name | VARCHAR(100) | NO | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | name |

---

### notification_recipients

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V75__notifications_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| delivered_at | TIMESTAMPTZ | NO | now() |
| id | BIGSERIAL | NO | — |
| notification_id | BIGINT | NO | — |
| read_at | TIMESTAMPTZ | YES | — |
| user_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_nr_notification | notification_id | notifications(id) | ON DELETE CASCADE |
| fk_nr_user | user_id | users(id) | ON DELETE CASCADE |

#### Unique Constraints

| Name | Columns |
|---|---|
| uk_nr_notification_user | notification_id, user_id |

---

### notifications

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V75__notifications_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| action_url | VARCHAR(500) | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(120) | YES | — |
| entity_id | VARCHAR(80) | YES | — |
| event_type | VARCHAR(80) | NO | — |
| id | BIGSERIAL | NO | — |
| message | TEXT | NO | — |
| module_no | INTEGER | YES | — |
| priority | VARCHAR(20) | NO | 'NORMAL' CHECK (priority IN ('LOW' |
| title | VARCHAR(200) | YES | — |

#### Check Constraints

```text
priority IN ('LOW', 'NORMAL', 'HIGH', 'CRITICAL')
```

---

### observation_options_hygiene

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V35__task_photos_evidence_columns.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| display_order | INTEGER | NO | 0 |
| id | VARCHAR(50) | NO | — |
| is_active | BOOLEAN | NO | TRUE |
| label | VARCHAR(255) | NO | — |

---

### observation_options_pest_sighting

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V35__task_photos_evidence_columns.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| display_order | INTEGER | NO | 0 |
| id | VARCHAR(50) | NO | — |
| is_active | BOOLEAN | NO | TRUE |
| label | VARCHAR(255) | NO | — |

---

### observation_options_structural

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V35__task_photos_evidence_columns.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| display_order | INTEGER | NO | 0 |
| id | VARCHAR(50) | NO | — |
| is_active | BOOLEAN | NO | TRUE |
| label | VARCHAR(255) | NO | — |

---

### petty_cash_attachments

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V21__petty_cash_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| attachment_type | VARCHAR(30) | NO | — |
| content_type | VARCHAR(100) | YES | — |
| file_key | VARCHAR(600) | NO | — |
| file_name | VARCHAR(255) | YES | — |
| file_size_bytes | BIGINT | YES | — |
| id | VARCHAR(50) | NO | — |
| notes | VARCHAR(500) | YES | — |
| request_id | VARCHAR(50) | NO | — |
| uploaded_at | TIMESTAMPTZ | NO | now() |
| uploaded_by | VARCHAR(120) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_pc_att_request | request_id | petty_cash_requests(id) | ON DELETE CASCADE |

#### Check Constraints

```text
attachment_type IN ('RECEIPT','PAYMENT_PROOF')
```

---

### petty_cash_audit_logs

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V21__petty_cash_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| action | VARCHAR(50) | NO | — |
| action_at | TIMESTAMPTZ | NO | now() |
| actor_name | VARCHAR(150) | YES | — |
| actor_user_id | BIGINT | YES | — |
| id | VARCHAR(50) | NO | — |
| remarks | VARCHAR(500) | YES | — |
| request_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_pc_aud_request | request_id | petty_cash_requests(id) | ON DELETE CASCADE |
| fk_pc_aud_actor | actor_user_id | users(id) | ON DELETE SET NULL |

#### Check Constraints

```text
action IN ( 'DRAFT_SAVED', 'SUBMITTED', 'RECIPIENTS_SELECTED', 'REVOKED', 'APPROVED', 'REJECTED', 'RETURNED', 'PAID' )
```

---

### petty_cash_request_recipient_roles

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V32__petty_cash_request_recipient_roles_mapping.sql
- Primary key: request_id, recipient_role_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| recipient_role_id | BIGINT | NO | — |
| request_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_pc_rr_request | request_id | petty_cash_requests(id) | ON DELETE CASCADE |
| fk_pc_rr_role | recipient_role_id | roles(id) | ON DELETE CASCADE |

---

### petty_cash_request_recipients

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V21__petty_cash_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| id | VARCHAR(50) | NO | — |
| recipient_role_id | BIGINT | YES | — |
| recipient_user_id | BIGINT | YES | — |
| request_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_pc_rec_request | request_id | petty_cash_requests(id) | ON DELETE CASCADE |
| fk_pc_rec_user | recipient_user_id | users(id) | ON DELETE CASCADE |
| fk_pc_rec_role | recipient_role_id | roles(id) | ON DELETE CASCADE |

#### Check Constraints

```text
chk_pc_rec_one_target: ( (CASE WHEN recipient_user_id IS NOT NULL THEN 1 ELSE 0 END) + (CASE WHEN recipient_role_id IS NOT NULL THEN 1 ELSE 0 END) ) = 1
```

---

### petty_cash_requests

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V21__petty_cash_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| amount_requested | NUMERIC(14,2) | YES | — |
| approval_reference | VARCHAR(200) | YES | — |
| approved_amount | NUMERIC(14,2) | YES | — |
| bank_account_holder | VARCHAR(150) | YES | — |
| bank_account_number | VARCHAR(50) | YES | — |
| bank_ifsc | VARCHAR(20) | YES | — |
| bank_name | VARCHAR(150) | YES | — |
| category | VARCHAR(80) | YES | — |
| correction_notes | VARCHAR(500) | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| description | VARCHAR(500) | YES | — |
| expense_date_from | DATE | YES | — |
| expense_date_to | DATE | YES | — |
| finance_remarks | VARCHAR(500) | YES | — |
| id | VARCHAR(50) | NO | — |
| is_pre_approved | BOOLEAN | NO | FALSE |
| justification_note | VARCHAR(500) | YES | — |
| paid_at | TIMESTAMPTZ | YES | — |
| paid_by_user_id | BIGINT | YES | — |
| payment_date | DATE | YES | — |
| payment_mode_processed | VARCHAR(20) | YES | — |
| payment_mode_requested | VARCHAR(20) | YES | — |
| pre_approved_by_user_id | BIGINT | YES | — |
| rejection_reason | VARCHAR(120) | YES | — |
| related_so_ref | VARCHAR(60) | YES | — |
| related_task_ref | VARCHAR(60) | YES | — |
| requester_branch_id | VARCHAR(30) | YES | — |
| requester_user_id | BIGINT | NO | — |
| reviewed_at | TIMESTAMPTZ | YES | — |
| reviewed_by_user_id | BIGINT | YES | — |
| reviewer_remarks | VARCHAR(500) | YES | — |
| status | VARCHAR(20) | NO | 'DRAFT' CHECK (status IN ('DRAFT' |
| submitted_at | TIMESTAMPTZ | YES | — |
| submitted_to_label | VARCHAR(200) | YES | — |
| transaction_ref | VARCHAR(120) | YES | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |
| upi_id | VARCHAR(120) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_pc_requester_user | requester_user_id | users(id) | ON DELETE RESTRICT |
| fk_pc_branch | requester_branch_id | branches(id) | ON DELETE RESTRICT |
| fk_pc_pre_approved_by | pre_approved_by_user_id | users(id) | ON DELETE SET NULL |
| fk_pc_reviewed_by | reviewed_by_user_id | users(id) | ON DELETE SET NULL |
| fk_pc_paid_by | paid_by_user_id | users(id) | ON DELETE SET NULL |

#### Check Constraints

```text
amount_requested > 0
payment_mode_requested IN ('BANK_TRANSFER','UPI')
status IN ('DRAFT','PENDING','APPROVED','REJECTED','RETURNED','PAID','REVOKED')
approved_amount >= 0
payment_mode_processed IN ('BANK_TRANSFER','UPI','CASH','CHEQUE')
chk_pc_expense_date_range: expense_date_to >= expense_date_from
```

---

### public.actions

- Sources: seravion-connect-backend/src/main/resources/db/migration/public/V1__init_public.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| description | VARCHAR(255) | YES | — |
| id | BIGSERIAL | NO | — |
| label | VARCHAR(255) | YES | — |
| name | VARCHAR(100) | NO | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | name |

---

### public.company_details

- Sources: seravion-connect-backend/src/main/resources/db/migration/public/V8__company_details.sql, seravion-connect-backend/src/main/resources/db/migration/public/V9__company_subscription.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| address_line_1 | VARCHAR(255) | NO | — |
| address_line_2 | VARCHAR(255) | YES | — |
| city | VARCHAR(100) | NO | — |
| company_code | VARCHAR(30) | NO | — |
| company_name | VARCHAR(150) | NO | — |
| contact_person_email | VARCHAR(150) | NO | — |
| contact_person_name | VARCHAR(120) | NO | — |
| contact_person_phone | VARCHAR(10) | NO | — |
| created_at | TIMESTAMPTZ | NO | NOW() |
| created_by | BIGINT | YES | — |
| gst_number | VARCHAR(15) | NO | — |
| id | BIGSERIAL | NO | — |
| industry_type | VARCHAR(100) | NO | — |
| is_active | BOOLEAN | NO | TRUE |
| license_number | VARCHAR(100) | YES | — |
| onboarding_status | VARCHAR(30) | NO | 'PENDING_COMPANY_DETAILS' |
| pan_number | VARCHAR(10) | NO | — |
| pincode | VARCHAR(6) | NO | — |
| rejection_reason | TEXT | YES | — |
| state | VARCHAR(100) | NO | — |
| submitted_at | TIMESTAMPTZ | YES | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | BIGINT | YES | — |
| verified_at | TIMESTAMPTZ | YES | — |
| verified_by | BIGINT | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_company_details_subscription_plan | subscription_plan_id | public.subscription_plans(id) | ON DELETE SET NULL |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | company_code |
| — | gst_number |
| — | pan_number |

---

### public.company_documents

- Sources: seravion-connect-backend/src/main/resources/db/migration/public/V8__company_details.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| company_id | BIGINT | NO | — |
| document_name | VARCHAR(255) | NO | — |
| document_type | VARCHAR(50) | NO | — |
| document_url | VARCHAR(500) | NO | — |
| file_content_type | VARCHAR(50) | YES | — |
| file_size_bytes | BIGINT | YES | — |
| id | BIGSERIAL | NO | — |
| reviewed_at | TIMESTAMPTZ | YES | — |
| reviewed_by | BIGINT | YES | — |
| updated_at | TIMESTAMPTZ | YES | — |
| uploaded_at | TIMESTAMPTZ | NO | NOW() |
| verified | BOOLEAN | NO | FALSE |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_company_documents_company | company_id | company_details(id) | ON DELETE CASCADE |

---

### public.company_subscription

- Sources: seravion-connect-backend/src/main/resources/db/migration/public/V9__company_subscription.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| auto_renew | BOOLEAN | NO | FALSE |
| branch_cost | NUMERIC(12,2) | NO | — |
| branch_count | INTEGER | NO | — |
| company_id | BIGINT | NO | — |
| created_at | TIMESTAMPTZ | NO | NOW() |
| duration_type | VARCHAR(20) | NO | — |
| end_date | DATE | NO | — |
| final_total | NUMERIC(12,2) | NO | — |
| gst_amount | NUMERIC(12,2) | NO | — |
| gst_percentage | NUMERIC(5,2) | NO | 18 |
| id | BIGSERIAL | NO | — |
| payment_method | VARCHAR(50) | YES | — |
| plan_type | VARCHAR(100) | NO | — |
| price_per_branch | NUMERIC(12,2) | NO | — |
| price_per_technician | NUMERIC(12,2) | NO | — |
| purchase_date | DATE | NO | — |
| razorpay_order_id | VARCHAR(255) | YES | — |
| razorpay_payment_id | VARCHAR(255) | YES | — |
| razorpay_signature | VARCHAR(512) | YES | — |
| start_date | DATE | NO | — |
| status | VARCHAR(20) | NO | 'ACTIVE' |
| subscription_id | VARCHAR(30) | NO | — |
| subscription_plan_id | VARCHAR(30) | NO | — |
| subtotal | NUMERIC(12,2) | NO | — |
| technician_cost | NUMERIC(12,2) | NO | — |
| technician_count | INTEGER | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_company_subscription_company | company_id | public.company_details(id) | ON DELETE CASCADE |
| fk_company_subscription_plan | subscription_plan_id | public.subscription_plans(id) | ON DELETE RESTRICT |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | subscription_id |

#### Check Constraints

```text
branch_count >= 1
technician_count >= 1
chk_company_subscription_duration: duration_type IN ('MONTHLY', 'QUARTERLY', 'ANNUAL', 'CUSTOM')
chk_company_subscription_status: status IN ('PENDING_PAYMENT', 'ACTIVE', 'EXPIRED', 'CANCELLED')
```

---

### public.email_verification_tokens

- Sources: seravion-connect-backend/src/main/resources/db/migration/public/V6__email_verification_token.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| expiration_date | TIMESTAMP WITH TIME ZONE | NO | — |
| global_user_id | BIGINT | NO | — |
| id | BIGSERIAL | NO | — |
| token | VARCHAR(255) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_evt_global_user | global_user_id | public.global_users(id) | ON DELETE CASCADE |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | token |

---

### public.global_users

- Sources: seravion-connect-backend/src/main/resources/db/migration/public/V4__global_users.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| company_name | VARCHAR(63) | YES | — |
| created_at | TIMESTAMPTZ | YES | now() |
| email | VARCHAR(255) | NO | — |
| full_name | VARCHAR(100) | YES | — |
| has_schema | BOOLEAN | NO | FALSE |
| id | BIGSERIAL | NO | — |
| is_active | BOOLEAN | YES | TRUE |
| last_login_at | TIMESTAMPTZ | YES | — |
| password_hash | VARCHAR(512) | NO | — |
| phone_number | VARCHAR(50) | YES | — |
| system_role | VARCHAR(50) | YES | 'CEO' |
| target_schema | VARCHAR(63) | YES | — |
| updated_at | TIMESTAMPTZ | YES | now() |
| username | VARCHAR(255) | NO | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | email |
| — | username |

---

### public.modules

- Sources: seravion-connect-backend/src/main/resources/db/migration/public/V1__init_public.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| description | VARCHAR(255) | YES | — |
| id | BIGSERIAL | NO | — |
| label | VARCHAR(255) | YES | — |
| name | VARCHAR(100) | NO | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | name |

---

### public.role_permissions

- Sources: seravion-connect-backend/src/main/resources/db/migration/public/V3__role_action_module_permission_table.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| action_id | BIGINT | NO | — |
| allowed | BOOLEAN | NO | TRUE |
| created_at | TIMESTAMPTZ | NO | now() |
| id | BIGSERIAL | NO | — |
| module_id | BIGINT | NO | — |
| receiver_role_ids | BIGINT[] | YES | NULL |
| role_id | BIGINT | NO | — |
| updated_at | TIMESTAMPTZ | NO | now() |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_rp_role | role_id | public.roles(id) | ON DELETE CASCADE |
| fk_rp_module | module_id | public.modules(id) | ON DELETE CASCADE |
| fk_rp_action | action_id | public.actions(id) | ON DELETE CASCADE |

#### Unique Constraints

| Name | Columns |
|---|---|
| uk_rp_role_module_action | role_id, module_id, action_id |

---

### public.roles

- Sources: seravion-connect-backend/src/main/resources/db/migration/public/V1__init_public.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(20) | NO | 'SERAVION' CHECK (created_by IN ('SERAVION')) |
| description | VARCHAR(255) | YES | — |
| id | BIGSERIAL | NO | — |
| is_app_user | BOOLEAN | NO | FALSE |
| name | VARCHAR(100) | NO | — |
| status | VARCHAR(20) | NO | 'ACTIVE' CHECK (status IN ('INACTIVE' |
| updated_at | TIMESTAMPTZ | NO | now() |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | name |

#### Check Constraints

```text
status IN ('INACTIVE', 'ACTIVE')
created_by IN ('SERAVION')
```

---

### public.root_user

- Sources: seravion-connect-backend/src/main/resources/db/migration/public/V1__init_public.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| email | VARCHAR(255) | NO | — |
| id | BIGSERIAL | NO | — |
| is_active | BOOLEAN | NO | TRUE |
| password | VARCHAR(255) | NO | — |
| role | VARCHAR(255) | NO | — |
| updated_at | TIMESTAMPTZ | NO | now() |
| username | VARCHAR(150) | NO | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | username |
| — | email |

---

### public.tenant_registry

- Sources: seravion-connect-backend/src/main/resources/db/migration/public/V1__init_public.sql, seravion-connect-backend/src/main/resources/db/migration/public/V21__add_company_id_to_tenant_registry.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| display_name | VARCHAR(255) | YES | — |
| id | BIGSERIAL | NO | — |
| schema_name | VARCHAR(100) | NO | — |
| status | VARCHAR(20) | NO | 'ACTIVE' |
| tenant_name | VARCHAR(100) | NO | — |
| updated_at | TIMESTAMPTZ | NO | now() |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_tenant_registry_company | company_id | public.company_details(id) | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | schema_name |

---

### public.token_blacklist

- Sources: seravion-connect-backend/src/main/resources/db/migration/public/V5__token_blacklist.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMP WITH TIME ZONE | YES | CURRENT_TIMESTAMP |
| expires_at | TIMESTAMP WITH TIME ZONE | NO | — |
| id | BIGINT | NO | — |
| token | VARCHAR(1000) | NO | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | token |

---

### purchase_bill_attachments

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V37__bills_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| attachment_type | VARCHAR(30) | NO | — |
| bill_id | VARCHAR(50) | NO | — |
| content_type | VARCHAR(100) | YES | — |
| file_key | VARCHAR(600) | NO | — |
| file_name | VARCHAR(255) | YES | — |
| file_size_bytes | BIGINT | YES | — |
| id | VARCHAR(50) | NO | — |
| uploaded_at | TIMESTAMPTZ | NO | now() |
| uploaded_by | VARCHAR(120) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_pba_bill | bill_id | purchase_bills(id) | ON DELETE CASCADE |

#### Check Constraints

```text
attachment_type IN ('VENDOR_BILL','GRN','SUPPORTING_DOC')
```

---

### purchase_bill_audit_logs

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V37__bills_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| action | VARCHAR(80) | NO | — |
| bill_id | VARCHAR(50) | NO | — |
| id | VARCHAR(50) | NO | — |
| performed_at | TIMESTAMPTZ | NO | now() |
| performed_by | VARCHAR(150) | YES | — |
| remarks | VARCHAR(500) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_pbal_bill | bill_id | purchase_bills(id) | ON DELETE CASCADE |

---

### purchase_bill_lines

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V37__bills_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| bill_id | VARCHAR(50) | NO | — |
| description | VARCHAR(500) | NO | — |
| discount_pct | NUMERIC(6,3) | NO | 0 |
| hsn_sac | VARCHAR(20) | YES | — |
| id | VARCHAR(50) | NO | — |
| item_id | VARCHAR(50) | YES | — |
| item_type | VARCHAR(20) | NO | — |
| line_no | INTEGER | NO | — |
| line_total | NUMERIC(14,2) | NO | 0 |
| qty | NUMERIC(14,3) | NO | 0 |
| rate | NUMERIC(14,2) | NO | 0 |
| tax_amount | NUMERIC(14,2) | NO | 0 |
| tax_pct | NUMERIC(6,3) | NO | 0 |
| taxable_amount | NUMERIC(14,2) | NO | 0 |
| uom | VARCHAR(30) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_pbl_bill | bill_id | purchase_bills(id) | ON DELETE CASCADE |

#### Check Constraints

```text
item_type IN ('PRODUCT','SERVICE','EXPENSE')
```

---

### purchase_bills

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V37__bills_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| bill_date | DATE | NO | — |
| bill_number | VARCHAR(50) | NO | — |
| bill_type | VARCHAR(20) | NO | — |
| branch_id | VARCHAR(30) | NO | — |
| cgst_amount | NUMERIC(14,2) | NO | 0 |
| coa_expense_ledger_id | VARCHAR(50) | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| credit_period_days | INTEGER | NO | — |
| discount_total | NUMERIC(14,2) | NO | 0 |
| due_date | DATE | NO | — |
| expense_category | VARCHAR(120) | YES | — |
| grn_reference | VARCHAR(80) | YES | — |
| id | VARCHAR(50) | NO | — |
| igst_amount | NUMERIC(14,2) | NO | 0 |
| internal_remarks | TEXT | YES | — |
| net_payable | NUMERIC(14,2) | NO | 0 |
| paid_amount | NUMERIC(14,2) | NO | 0 |
| pending_amount | NUMERIC(14,2) | NO | 0 |
| purchase_order_id | VARCHAR(50) | YES | — |
| sgst_amount | NUMERIC(14,2) | NO | 0 |
| status | VARCHAR(20) | NO | — |
| sub_total | NUMERIC(14,2) | NO | 0 |
| taxable_amount | NUMERIC(14,2) | NO | 0 |
| tds_amount | NUMERIC(14,2) | NO | 0 |
| tds_rate_snapshot | NUMERIC(6,3) | YES | — |
| tds_section_snapshot | VARCHAR(20) | YES | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |
| vendor_bill_number | VARCHAR(120) | NO | — |
| vendor_gstin_snapshot | VARCHAR(20) | YES | — |
| vendor_id | VARCHAR(50) | NO | — |
| vendor_name_snapshot | VARCHAR(255) | NO | — |
| vendor_state_snapshot | VARCHAR(100) | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | bill_number |

#### Check Constraints

```text
bill_type IN ('PURCHASE','EXPENSE')
status IN ('DRAFT','PENDING','PARTIAL','PAID','OVERDUE','CANCELLED')
credit_period_days BETWEEN 1 AND 365
```

---

### purchase_order

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V15__purchase_order.sql, seravion-connect-backend/src/main/resources/db/migration/tenant/V23__purchase_order_rename_supplier_gst.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| authorized_person | VARCHAR(100) | YES | — |
| branch_id | VARCHAR(30) | NO | — |
| branch_name | VARCHAR(50) | YES | — |
| company_gst_number | VARCHAR(15) | NO | — |
| contact_number | VARCHAR(15) | NO | — |
| contact_person | VARCHAR(100) | NO | — |
| created_at | TIMESTAMPTZ | YES | now() |
| created_by | VARCHAR(100) | YES | — |
| deleted_at | TIMESTAMPTZ | YES | — |
| deleted_by | VARCHAR(100) | YES | — |
| delivery_address | TEXT | NO | — |
| delivery_date | DATE | YES | — |
| designation | VARCHAR(100) | YES | — |
| grand_total | NUMERIC(15,2) | YES | — |
| gst_number | VARCHAR(20) | NO | — |
| id | VARCHAR(50) | NO | — |
| is_deleted | BOOLEAN | YES | FALSE |
| items_count | INT | YES | — |
| note | TEXT | YES | — |
| po_date | DATE | NO | — |
| po_number | VARCHAR(50) | NO | — |
| status | VARCHAR(30) | NO | — |
| subtotal | NUMERIC(15,2) | YES | — |
| total_tax | NUMERIC(15,2) | YES | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |
| vendor_address | TEXT | YES | — |
| vendor_gst | VARCHAR(20) | YES | — |
| vendor_id | VARCHAR(50) | NO | — |
| vendor_name | VARCHAR(255) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_po_vendor | vendor_id | vendors(id) | — |
| fk_po_branch | branch_id | branches(id) | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | po_number |

---

### purchase_order_item

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V15__purchase_order.sql, seravion-connect-backend/src/main/resources/db/migration/tenant/V23__purchase_order_rename_supplier_gst.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | YES | now() |
| created_by | VARCHAR(100) | YES | — |
| deleted_at | TIMESTAMPTZ | YES | — |
| deleted_by | VARCHAR(100) | YES | — |
| gst_percent | NUMERIC(5,2) | YES | — |
| id | VARCHAR(50) | NO | — |
| is_deleted | BOOLEAN | YES | FALSE |
| line_number | INT | NO | 1 |
| price | NUMERIC(15,2) | YES | — |
| product_id | VARCHAR(50) | NO | — |
| product_name | VARCHAR(255) | YES | — |
| purchase_order_id | VARCHAR(50) | NO | — |
| quantity | NUMERIC(10,2) | NO | — |
| tax_amount | NUMERIC(15,2) | YES | — |
| total_amount | NUMERIC(15,2) | YES | — |
| uom | VARCHAR(100) | YES | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_poi_purchase_order | purchase_order_id | purchase_order(id) | ON DELETE CASCADE |
| fk_poi_product | product_id | inventory_products(id) | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| uq_poi_po_line | purchase_order_id, line_number |

---

### quotation_attachments

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V16__quotation_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| content_type | VARCHAR(100) | NO | — |
| file_key | TEXT | NO | — |
| file_name | VARCHAR(255) | NO | — |
| file_size_bytes | BIGINT | YES | — |
| id | VARCHAR(50) | NO | — |
| notes | TEXT | YES | — |
| quotation_id | VARCHAR(50) | NO | — |
| uploaded_at | TIMESTAMPTZ | NO | now() |
| uploaded_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_qa_quotation | quotation_id | quotations(id) | ON DELETE CASCADE |

---

### quotation_audit_logs

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V16__quotation_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| changed_at | TIMESTAMPTZ | NO | now() |
| changed_by | VARCHAR(100) | YES | — |
| event_type | VARCHAR(50) | NO | — |
| field_changed | VARCHAR(100) | YES | — |
| id | VARCHAR(50) | NO | — |
| new_value | TEXT | YES | — |
| notes | TEXT | YES | — |
| old_value | TEXT | YES | — |
| quotation_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_qal_quotation | quotation_id | quotations(id) | ON DELETE CASCADE |

#### Check Constraints

```text
event_type IN ( 'CREATED', 'UPDATED', 'SENT', 'VIEWED', 'ACCEPTED', 'REJECTED', 'EXPIRED', 'REVISED', 'DELETED', 'CONVERTED_TO_CONTRACT', 'RESENT' )
```

---

### quotation_locations

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V16__quotation_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| address | TEXT | NO | — |
| area_sqft | NUMERIC(10,2) | NO | — |
| branch_id | VARCHAR(30) | NO | — |
| city | VARCHAR(100) | NO | — |
| country | VARCHAR(100) | NO | 'India' |
| created_at | TIMESTAMPTZ | NO | now() |
| display_order | INTEGER | NO | 1 |
| google_map_url | TEXT | YES | — |
| id | VARCHAR(50) | NO | — |
| location_category | VARCHAR(30) | NO | — |
| location_service_subtotal | NUMERIC(14,2) | NO | 0 |
| location_sub_category | VARCHAR(20) | NO | — |
| pincode | VARCHAR(10) | YES | — |
| quotation_id | VARCHAR(50) | NO | — |
| state | VARCHAR(100) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_ql_quotation | quotation_id | quotations(id) | ON DELETE CASCADE |
| fk_ql_branch | branch_id | branches(id) | ON DELETE RESTRICT |

#### Check Constraints

```text
location_category IN ('RESIDENTIAL', 'COMMERCIAL', 'INDUSTRIAL', 'WAREHOUSE')
location_sub_category IN ('INTERNAL', 'EXTERNAL')
area_sqft > 0
```

---

### quotation_product_lines

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V16__quotation_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| cgst_amount | NUMERIC(14,2) | NO | 0 |
| cgst_rate | NUMERIC(5,2) | NO | 0 |
| created_at | TIMESTAMPTZ | NO | now() |
| display_order | INTEGER | NO | 1 |
| hsn_code | VARCHAR(20) | YES | — |
| id | VARCHAR(50) | NO | — |
| igst_amount | NUMERIC(14,2) | NO | 0 |
| igst_rate | NUMERIC(5,2) | NO | 0 |
| line_subtotal | NUMERIC(14,2) | NO | — |
| line_total | NUMERIC(14,2) | NO | — |
| product_code | VARCHAR(100) | NO | — |
| product_id | VARCHAR(50) | NO | — |
| product_name | VARCHAR(200) | NO | — |
| quantity | NUMERIC(10,3) | NO | — |
| quotation_id | VARCHAR(50) | NO | — |
| sgst_amount | NUMERIC(14,2) | NO | 0 |
| sgst_rate | NUMERIC(5,2) | NO | 0 |
| tax_amount | NUMERIC(14,2) | NO | 0 |
| tax_type | VARCHAR(10) | NO | 'INTRA' CHECK (tax_type IN ('INTRA' |
| unit_price | NUMERIC(14,2) | NO | — |
| uom | VARCHAR(50) | YES | — |
| updated_at | TIMESTAMPTZ | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_qpl_quotation | quotation_id | quotations(id) | ON DELETE CASCADE |
| fk_qpl_product | product_id | inventory_products(id) | ON DELETE RESTRICT |

#### Check Constraints

```text
quantity > 0
tax_type IN ('INTRA', 'INTER')
```

---

### quotation_prospects

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V16__quotation_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| address | TEXT | NO | — |
| city | VARCHAR(100) | NO | — |
| company_name | VARCHAR(100) | YES | — |
| country | VARCHAR(100) | NO | 'India' |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| email | VARCHAR(255) | YES | — |
| full_name | VARCHAR(200) | NO | — |
| google_map_url | TEXT | YES | — |
| id | VARCHAR(50) | NO | — |
| phone | VARCHAR(15) | NO | — |
| pincode | VARCHAR(10) | YES | — |
| state | VARCHAR(100) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |

---

### quotation_service_lines

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V16__quotation_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| area_sqft_used | NUMERIC(10,2) | YES | — |
| base_price | NUMERIC(14,2) | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| display_order | INTEGER | NO | 1 |
| fixed_tier_name | VARCHAR(150) | YES | — |
| id | VARCHAR(50) | NO | — |
| line_total | NUMERIC(14,2) | NO | 0 |
| price_per_sqft | NUMERIC(10,4) | YES | — |
| price_type | VARCHAR(20) | NO | — |
| quotation_id | VARCHAR(50) | NO | — |
| quotation_location_id | VARCHAR(50) | NO | — |
| rate_per_visit | NUMERIC(14,2) | NO | — |
| service_code | VARCHAR(50) | NO | — |
| service_id | VARCHAR(50) | NO | — |
| service_name | VARCHAR(200) | NO | — |
| total_visits | INTEGER | NO | 1 |
| updated_at | TIMESTAMPTZ | YES | — |
| visit_frequency | VARCHAR(20) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_qsl_quotation | quotation_id | quotations(id) | ON DELETE CASCADE |
| fk_qsl_location | quotation_location_id | quotation_locations(id) | ON DELETE CASCADE |
| fk_qsl_service | service_id | services(id) | ON DELETE RESTRICT |

#### Check Constraints

```text
price_type IN ('FIXED', 'AREA_BASED', 'INSPECTION')
visit_frequency IN ('ONE_TIME', 'MONTHLY', 'QUARTERLY', 'HALF_YEARLY', 'YEARLY')
```

---

### quotations

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V16__quotation_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| accepted_at | TIMESTAMPTZ | YES | — |
| contract_duration | VARCHAR(20) | YES | — |
| contract_frequency | VARCHAR(20) | YES | — |
| contract_id | VARCHAR(50) | YES | — |
| contract_proposed_start | DATE | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| custom_payment_terms | TEXT | YES | — |
| customer_id | VARCHAR(50) | YES | — |
| deleted_at | TIMESTAMPTZ | YES | — |
| deleted_by | VARCHAR(100) | YES | — |
| deletion_reason | VARCHAR(100) | YES | — |
| deletion_reason_detail | TEXT | YES | — |
| discount_amount | NUMERIC(14,2) | NO | 0 |
| discount_type | VARCHAR(20) | YES | — |
| discount_value | NUMERIC(10,2) | YES | 0 |
| expired_at | TIMESTAMPTZ | YES | — |
| grand_total | NUMERIC(14,2) | NO | 0 |
| id | VARCHAR(50) | NO | — |
| internal_notes | TEXT | YES | — |
| is_deleted | BOOLEAN | NO | FALSE |
| lead_id | VARCHAR(50) | YES | — |
| payment_terms | VARCHAR(50) | NO | — |
| products_subtotal | NUMERIC(14,2) | NO | 0 |
| prospect_id | VARCHAR(50) | YES | — |
| quotation_number | VARCHAR(30) | NO | — |
| quotation_type | VARCHAR(20) | NO | — |
| rejected_at | TIMESTAMPTZ | YES | — |
| revised_from_id | VARCHAR(50) | YES | — |
| sent_at | TIMESTAMPTZ | YES | — |
| service_mode | VARCHAR(20) | YES | — |
| services_subtotal | NUMERIC(14,2) | NO | 0 |
| source_type | VARCHAR(20) | NO | — |
| special_terms | TEXT | YES | — |
| status | VARCHAR(20) | NO | 'DRAFT' CHECK (status IN ('DRAFT' |
| subtotal_before_tax | NUMERIC(14,2) | NO | 0 |
| tax_total | NUMERIC(14,2) | NO | 0 |
| total_before_discount | NUMERIC(14,2) | NO | 0 |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |
| valid_till | DATE | NO | — |
| viewed_at | TIMESTAMPTZ | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_quot_lead | lead_id | leads(id) | ON DELETE RESTRICT |
| fk_quot_prospect | prospect_id | quotation_prospects(id) | ON DELETE RESTRICT |
| fk_quot_revised_from | revised_from_id | quotations(id) | ON DELETE SET NULL |

#### Check Constraints

```text
source_type IN ('FROM_LEAD', 'FROM_CUSTOMER', 'NEW_PROSPECT')
quotation_type IN ('SERVICE', 'PRODUCT', 'COMBINED')
service_mode IN ('ONE_TIME', 'CONTRACT')
contract_frequency IN ('MONTHLY', 'QUARTERLY', 'HALF_YEARLY', 'YEARLY')
contract_duration IN ('SIX_MONTHS', 'ONE_YEAR', 'TWO_YEARS', 'THREE_YEARS')
discount_type IN ('PERCENTAGE', 'FLAT_AMOUNT')
payment_terms IN ( 'FULL_ADVANCE', 'FIFTY_ADVANCE_FIFTY_COMPLETION', 'NET_15', 'NET_30', 'CUSTOM' )
status IN ('DRAFT', 'SENT', 'VIEWED', 'ACCEPTED', 'REJECTED', 'EXPIRED', 'REVISED')
deletion_reason IN ( 'CREATED_BY_MISTAKE', 'DUPLICATE_QUOTATION', 'CLIENT_WITHDREW_INTEREST', 'PRICING_ERROR', 'OTHER' )
chk_quot_source_xor: ( (CASE WHEN lead_id IS NOT NULL THEN 1 ELSE 0 END) + (CASE WHEN customer_id IS NOT NULL THEN 1 ELSE 0 END) + (CASE WHEN prospect_id IS NOT NULL THEN 1 ELSE 0 END) ) = 1
chk_quot_contract_fields: service_mode IS NULL OR service_mode = 'ONE_TIME' OR ( service_mode = 'CONTRACT' AND contract_frequency IS NOT NULL AND contract_duration IS NOT NULL AND contract_proposed_start IS NOT NULL )
chk_quot_custom_payment: payment_terms <> 'CUSTOM' OR custom_payment_terms IS NOT NULL
chk_quot_deletion_detail: deletion_reason IS NULL OR deletion_reason <> 'OTHER' OR deletion_reason_detail IS NOT NULL
```

---

### role_compensation_configuration

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V7__role_salary_leave_config.sql
- Primary key: config_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| config_id | VARCHAR(50) | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(20) | YES | — |
| effective_from | DATE | NO | — |
| effective_to | DATE | YES | — |
| role_id | BIGINT | NO | — |
| status | VARCHAR(20) | NO | 'ACTIVE' CHECK (status IN ('INACTIVE' |
| updated_at | TIMESTAMPTZ | NO | now() |
| updated_by | VARCHAR(20) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_rcc_role | role_id | roles(id) | ON DELETE CASCADE |

#### Check Constraints

```text
status IN ('INACTIVE', 'ACTIVE')
```

---

### role_permissions

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V1__init_tenant.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| action_id | BIGINT | NO | — |
| allowed | BOOLEAN | NO | TRUE |
| created_at | TIMESTAMPTZ | NO | now() |
| id | BIGSERIAL | NO | — |
| module_id | BIGINT | NO | — |
| receiver_role_ids | BIGINT[] | YES | NULL |
| role_id | BIGINT | NO | — |
| updated_at | TIMESTAMPTZ | NO | now() |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_rp_role | role_id | roles(id) | ON DELETE CASCADE |
| fk_rp_module | module_id | modules(id) | ON DELETE CASCADE |
| fk_rp_action | action_id | actions(id) | ON DELETE CASCADE |

#### Unique Constraints

| Name | Columns |
|---|---|
| uk_rp_role_module_action | role_id, module_id, action_id |

---

### roles

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V1__init_tenant.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(20) | NO | 'COMPANY' CHECK (created_by IN ('COMPANY')) |
| description | VARCHAR(255) | YES | — |
| id | BIGSERIAL | NO | — |
| is_app_user | BOOLEAN | NO | FALSE |
| name | VARCHAR(100) | NO | — |
| status | VARCHAR(20) | NO | 'ACTIVE' CHECK (status IN ('INACTIVE' |
| updated_at | TIMESTAMPTZ | NO | now() |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | name |

#### Check Constraints

```text
status IN ('INACTIVE', 'ACTIVE')
created_by IN ('COMPANY')
```

---

### salary_details

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V7__role_salary_leave_config.sql
- Primary key: config_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| basic_salary | NUMERIC(12,2) | NO | — |
| config_id | VARCHAR(50) | NO | — |
| deductions | NUMERIC(12,2) | YES | 0.00 |
| esi_applicable | BOOLEAN | YES | FALSE |
| hra | NUMERIC(12,2) | YES | 0.00 |
| incentive | NUMERIC(12,2) | YES | 0.00 |
| other_allowance | NUMERIC(12,2) | YES | 0.00 |
| pf_applicable | BOOLEAN | YES | FALSE |
| salary_effective_from | DATE | YES | — |
| salary_effective_to | DATE | YES | — |
| salary_type | VARCHAR(20) | NO | — |
| tds_applicable | BOOLEAN | YES | FALSE |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sd_config | config_id | role_compensation_configuration(config_id) | ON DELETE CASCADE |

#### Check Constraints

```text
salary_type IN ('CTC', 'FIXED', 'HOURLY')
```

---

### sales_invoice_attachments

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V36__invoicing_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| content_type | VARCHAR(100) | YES | — |
| file_key | VARCHAR(600) | NO | — |
| file_name | VARCHAR(255) | YES | — |
| file_size_bytes | BIGINT | YES | — |
| id | VARCHAR(50) | NO | — |
| invoice_id | VARCHAR(50) | NO | — |
| uploaded_at | TIMESTAMPTZ | NO | now() |
| uploaded_by | VARCHAR(120) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sia_invoice | invoice_id | sales_invoices(id) | ON DELETE CASCADE |

---

### sales_invoice_audit_logs

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V36__invoicing_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| action | VARCHAR(80) | NO | — |
| id | VARCHAR(50) | NO | — |
| invoice_id | VARCHAR(50) | NO | — |
| performed_at | TIMESTAMPTZ | NO | now() |
| performed_by | VARCHAR(150) | YES | — |
| remarks | VARCHAR(500) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sial_invoice | invoice_id | sales_invoices(id) | ON DELETE CASCADE |

---

### sales_invoice_lines

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V36__invoicing_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| area_sqft | NUMERIC(12,2) | YES | — |
| description | VARCHAR(500) | NO | — |
| discount_pct | NUMERIC(6,3) | NO | 0 |
| hsn_sac | VARCHAR(20) | YES | — |
| id | VARCHAR(50) | NO | — |
| invoice_id | VARCHAR(50) | NO | — |
| item_id | VARCHAR(50) | YES | — |
| item_type | VARCHAR(20) | NO | — |
| line_no | INTEGER | NO | — |
| line_total | NUMERIC(14,2) | NO | 0 |
| price_type | VARCHAR(30) | YES | — |
| pricing_config_json | TEXT | YES | — |
| qty | NUMERIC(14,3) | NO | 0 |
| rate | NUMERIC(14,2) | NO | 0 |
| tax_amount | NUMERIC(14,2) | NO | 0 |
| tax_pct | NUMERIC(6,3) | NO | 0 |
| taxable_amount | NUMERIC(14,2) | NO | 0 |
| uom | VARCHAR(30) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sil_invoice | invoice_id | sales_invoices(id) | ON DELETE CASCADE |

#### Check Constraints

```text
item_type IN ('SERVICE','PRODUCT')
```

---

### sales_invoices

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V36__invoicing_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| billing_address_snapshot | TEXT | YES | — |
| branch_id | VARCHAR(30) | NO | — |
| cgst_amount | NUMERIC(14,2) | NO | 0 |
| contact_person_snapshot | VARCHAR(200) | YES | — |
| contract_id | VARCHAR(50) | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| creation_mode | VARCHAR(20) | NO | — |
| credit_period_days | INTEGER | NO | — |
| customer_gstin_snapshot | VARCHAR(20) | YES | — |
| customer_id | VARCHAR(50) | NO | — |
| customer_name_snapshot | VARCHAR(255) | NO | — |
| customer_state_snapshot | VARCHAR(100) | YES | — |
| discount_total | NUMERIC(14,2) | NO | 0 |
| due_date | DATE | NO | — |
| einvoice_required | BOOLEAN | NO | FALSE |
| grand_total | NUMERIC(14,2) | NO | 0 |
| id | VARCHAR(50) | NO | — |
| igst_amount | NUMERIC(14,2) | NO | 0 |
| internal_remarks | TEXT | YES | — |
| invoice_date | DATE | NO | — |
| invoice_number | VARCHAR(50) | NO | — |
| invoice_type | VARCHAR(20) | NO | — |
| irn_number | VARCHAR(100) | YES | — |
| irn_payload_json | TEXT | YES | — |
| irn_status | VARCHAR(20) | YES | — |
| notes | TEXT | YES | — |
| pending_amount | NUMERIC(14,2) | NO | 0 |
| received_amount | NUMERIC(14,2) | NO | 0 |
| round_off_amount | NUMERIC(14,2) | NO | 0 |
| sales_order_id | VARCHAR(50) | YES | — |
| sgst_amount | NUMERIC(14,2) | NO | 0 |
| status | VARCHAR(20) | NO | — |
| sub_total | NUMERIC(14,2) | NO | 0 |
| taxable_amount | NUMERIC(14,2) | NO | 0 |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | invoice_number |

#### Check Constraints

```text
invoice_type IN ('TAX','PROFORMA')
creation_mode IN ('FROM_SO','DIRECT')
status IN ('DRAFT','SENT','PARTIAL','PAID','OVERDUE','CANCELLED')
credit_period_days BETWEEN 1 AND 365
```

---

### sales_order_cancellation_logs

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V63__sales_order_cancellation_audit_log.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| cancel_reason | VARCHAR(50) | NO | — |
| cancelled_at | TIMESTAMPTZ | NO | now() |
| cancelled_by | VARCHAR(100) | YES | — |
| id | VARCHAR(50) | NO | — |
| remarks | VARCHAR(500) | YES | — |
| sales_order_id | VARCHAR(50) | NO | — |
| so_number | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_socl_sales_order | sales_order_id | sales_orders(id) | ON DELETE CASCADE |

---

### sales_order_product_lines

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V20__sales_order_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| display_order | INTEGER | NO | 1 |
| hsn_code | VARCHAR(20) | YES | — |
| id | VARCHAR(50) | NO | — |
| line_total | NUMERIC(14,2) | NO | 0 |
| product_code | VARCHAR(50) | YES | — |
| product_id | VARCHAR(50) | NO | — |
| product_name | VARCHAR(200) | NO | — |
| quantity | NUMERIC(14,3) | NO | 0 |
| sales_order_id | VARCHAR(50) | NO | — |
| tax_amount | NUMERIC(14,2) | NO | 0 |
| tax_percent | NUMERIC(6,3) | NO | 0 |
| unit_price | NUMERIC(14,2) | NO | 0 |
| uom | VARCHAR(30) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sopl_so | sales_order_id | sales_orders(id) | ON DELETE CASCADE |

---

### sales_order_site_chemicals

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V20__sales_order_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| coverage_sqft | NUMERIC(12,2) | YES | — |
| display_order | INTEGER | NO | 1 |
| hsn_code | VARCHAR(20) | YES | — |
| id | VARCHAR(50) | NO | — |
| line_cost | NUMERIC(14,2) | NO | 0 |
| product_code | VARCHAR(50) | YES | — |
| product_id | VARCHAR(50) | YES | — |
| product_name | VARCHAR(200) | NO | — |
| required_qty | VARCHAR(50) | YES | — |
| sales_order_site_id | VARCHAR(50) | NO | — |
| unit_price | NUMERIC(14,2) | NO | 0 |
| uom | VARCHAR(30) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sosc_site | sales_order_site_id | sales_order_sites(id) | ON DELETE CASCADE |

---

### sales_order_site_services

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V20__sales_order_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| display_order | INTEGER | NO | 1 |
| hsn_code | VARCHAR(20) | YES | — |
| id | VARCHAR(50) | NO | — |
| line_total | NUMERIC(14,2) | NO | 0 |
| sales_order_site_id | VARCHAR(50) | NO | — |
| service_type_id | VARCHAR(50) | NO | — |
| service_type_name | VARCHAR(200) | NO | — |
| sqft | NUMERIC(12,2) | NO | 0 |
| tax_amount | NUMERIC(14,2) | NO | 0 |
| tax_percent | NUMERIC(6,3) | NO | 0 |
| unit_price | NUMERIC(14,2) | NO | 0 |
| visits | NUMERIC(12,2) | NO | 1 |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_soss_site | sales_order_site_id | sales_order_sites(id) | ON DELETE CASCADE |

---

### sales_order_sites

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V20__sales_order_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| address | TEXT | YES | — |
| area_sqft | NUMERIC(12,2) | NO | 0 |
| category | VARCHAR(30) | NO | — |
| city | VARCHAR(100) | NO | — |
| contact_mobile | VARCHAR(20) | YES | — |
| contact_person | VARCHAR(200) | YES | — |
| contract_site_id | VARCHAR(50) | YES | — |
| country | VARCHAR(100) | NO | 'India' |
| display_order | INTEGER | NO | 1 |
| google_map_url | TEXT | YES | — |
| id | VARCHAR(50) | NO | — |
| sales_order_id | VARCHAR(50) | NO | — |
| site_name | VARCHAR(200) | NO | — |
| state | VARCHAR(100) | NO | — |
| sub_category | VARCHAR(20) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sos_so | sales_order_id | sales_orders(id) | ON DELETE CASCADE |

---

### sales_orders

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V20__sales_order_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| billing_period_label | VARCHAR(100) | YES | — |
| branch_id | VARCHAR(30) | NO | — |
| cancel_reason | VARCHAR(50) | YES | — |
| cancel_remarks | VARCHAR(500) | YES | — |
| cancelled_at | TIMESTAMPTZ | YES | — |
| cancelled_by | VARCHAR(100) | YES | — |
| challans_count | INTEGER | NO | 0 |
| contract_id | VARCHAR(50) | YES | — |
| contract_payment_line_id | VARCHAR(50) | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| customer_id | VARCHAR(50) | NO | — |
| customer_name | VARCHAR(255) | NO | — |
| delivery_address_line1 | VARCHAR(500) | YES | — |
| delivery_address_line2 | VARCHAR(500) | YES | — |
| delivery_address_type | VARCHAR(30) | YES | — |
| delivery_city | VARCHAR(100) | YES | — |
| delivery_country | VARCHAR(100) | YES | — |
| delivery_google_map_url | TEXT | YES | — |
| delivery_pincode | VARCHAR(10) | YES | — |
| delivery_site_id | VARCHAR(50) | YES | — |
| delivery_state | VARCHAR(100) | YES | — |
| discount_amount | NUMERIC(14,2) | NO | 0 |
| discount_type | VARCHAR(20) | YES | — |
| discount_value | NUMERIC(14,2) | NO | 0 |
| execution_notes | TEXT | YES | — |
| expected_delivery_date | DATE | YES | — |
| gma_sheet_id | VARCHAR(50) | YES | — |
| grand_total | NUMERIC(14,2) | NO | 0 |
| id | VARCHAR(50) | NO | — |
| invoice_linked | BOOLEAN | NO | FALSE |
| job_cards_count | INTEGER | NO | 0 |
| one_time_source | VARCHAR(30) | YES | — |
| order_type | VARCHAR(30) | NO | — |
| priority | VARCHAR(20) | YES | — |
| quotation_id | VARCHAR(50) | YES | — |
| so_date | DATE | NO | — |
| so_number | VARCHAR(50) | NO | — |
| status | VARCHAR(30) | NO | — |
| sub_total | NUMERIC(14,2) | NO | 0 |
| tax_total | NUMERIC(14,2) | NO | 0 |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | so_number |

#### Check Constraints

```text
order_type IN ('SERVICE_CONTRACT', 'ONE_TIME_SERVICE', 'PRODUCT_SALE')
status IN ('DRAFT', 'OPEN', 'FULFILLED', 'BILLED', 'CANCELLED')
one_time_source IS NULL OR one_time_source IN ('QUOTATION_GMA', 'STANDALONE')
discount_type IS NULL OR discount_type IN ('NONE', 'FLAT', 'PERCENTAGE')
delivery_address_type IS NULL OR delivery_address_type IN ('REGISTERED_SITE', 'CUSTOM')
priority IS NULL OR priority IN ('NORMAL', 'URGENT', 'CRITICAL')
```

---

### service_audit_logs

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| change_type | VARCHAR(50) | NO | — |
| changed_by | VARCHAR(100) | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| id | VARCHAR(50) | NO | — |
| notes | TEXT | YES | — |
| service_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sal_service | service_id | services(id) | ON DELETE CASCADE |

#### Check Constraints

```text
change_type IN ('CREATE', 'UPDATE', 'DEACTIVATE')
```

#### Indexes

| Name | Unique | Columns |
|---|---:|---|
| idx_service_audit_logs_service | NO | service_id |

---

### service_categories

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| display_order | INTEGER | YES | — |
| id | VARCHAR(50) | NO | — |
| is_active | BOOLEAN | NO | TRUE |
| name | VARCHAR(150) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| uk_service_categories_name | name |

---

### service_category_area

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| base_price | DOUBLE PRECISION | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| id | VARCHAR(50) | NO | — |
| is_active | BOOLEAN | NO | TRUE |
| price_per_sqft | DOUBLE PRECISION | NO | — |
| service_category_id | VARCHAR(50) | NO | — |
| service_sub_category_id | VARCHAR(50) | YES | — |
| sqft_increment | DOUBLE PRECISION | NO | 100 |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sca_category | service_category_id | service_categories(id) | ON DELETE CASCADE |
| fk_sca_sub_category | service_sub_category_id | service_sub_categories(id) | ON DELETE SET NULL |

#### Indexes

| Name | Unique | Columns |
|---|---:|---|
| idx_service_category_area_cat | NO | service_category_id |

---

### service_category_fixed

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| display_order | INTEGER | YES | — |
| id | VARCHAR(50) | NO | — |
| is_active | BOOLEAN | NO | TRUE |
| price_amount | DOUBLE PRECISION | NO | — |
| service_category_id | VARCHAR(50) | NO | — |
| service_sub_category_id | VARCHAR(50) | YES | — |
| tier_name | VARCHAR(150) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_scf_category | service_category_id | service_categories(id) | ON DELETE CASCADE |
| fk_scf_sub_category | service_sub_category_id | service_sub_categories(id) | ON DELETE SET NULL |

#### Indexes

| Name | Unique | Columns |
|---|---:|---|
| idx_service_category_fixed_cat | NO | service_category_id |

---

### service_category_inspection

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| id | VARCHAR(50) | NO | — |
| inspection_fee | DOUBLE PRECISION | NO | — |
| is_active | BOOLEAN | NO | TRUE |
| service_category_id | VARCHAR(50) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sci_category | service_category_id | service_categories(id) | ON DELETE CASCADE |

#### Indexes

| Name | Unique | Columns |
|---|---:|---|
| idx_service_category_inspection_cat | NO | service_category_id |

---

### service_custom_pricing_blocks

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| id | VARCHAR(50) | NO | — |
| is_active | BOOLEAN | NO | TRUE |
| label | VARCHAR(200) | YES | — |
| service_category_id | VARCHAR(50) | NO | — |
| service_sub_category_id | VARCHAR(50) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_scpb_category | service_category_id | service_categories(id) | ON DELETE CASCADE |
| fk_scpb_sub_category | service_sub_category_id | service_sub_categories(id) | ON DELETE CASCADE |

---

### service_custom_pricing_fields

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| block_id | VARCHAR(50) | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| display_order | INTEGER | YES | — |
| field_name | VARCHAR(200) | NO | — |
| id | VARCHAR(50) | NO | — |
| price_amount | DOUBLE PRECISION | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_scpf_block | block_id | service_custom_pricing_blocks(id) | ON DELETE CASCADE |

#### Indexes

| Name | Unique | Columns |
|---|---:|---|
| idx_service_custom_pricing_fields_block | NO | block_id |

---

### service_execution_chemical_usages

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V35__task_photos_evidence_columns.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| hsn_snapshot | VARCHAR(50) | YES | — |
| id | VARCHAR(50) | NO | — |
| inventory_product_id | VARCHAR(50) | NO | — |
| product_name_snapshot | VARCHAR(255) | NO | — |
| required_dilution_snapshot | NUMERIC(14, 3) | YES | — |
| required_qty | NUMERIC(14, 3) | NO | — |
| service_execution_id | VARCHAR(50) | NO | — |
| service_product_id | VARCHAR(50) | YES | — |
| sort_order | INTEGER | NO | 0 |
| uom_snapshot | VARCHAR(50) | YES | — |
| updated_at | TIMESTAMPTZ | NO | now() |
| used_dilution | NUMERIC(14, 3) | NO | — |
| used_qty | NUMERIC(14, 3) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sec_execution | service_execution_id | service_executions(id) | ON DELETE CASCADE |
| fk_sec_product | inventory_product_id | inventory_products(id) | ON DELETE RESTRICT |

---

### service_execution_treatments

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V35__task_photos_evidence_columns.sql
- Primary key: service_execution_id, service_treatment_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| service_execution_id | VARCHAR(50) | NO | — |
| service_treatment_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_set_execution | service_execution_id | service_executions(id) | ON DELETE CASCADE |
| fk_set_treatment | service_treatment_id | service_treatments(id) | ON DELETE RESTRICT |

---

### service_executions

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V35__task_photos_evidence_columns.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| id | VARCHAR(50) | NO | — |
| infestation_level | VARCHAR(20) | NO | — |
| location_area | TEXT | NO | — |
| service_id | VARCHAR(50) | YES | — |
| service_name_snapshot | VARCHAR(200) | NO | — |
| sort_order | INTEGER | NO | 0 |
| task_id | VARCHAR(50) | NO | — |
| trap_codes | TEXT | YES | — |
| updated_at | TIMESTAMPTZ | NO | now() |
| updated_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_se_task | task_id | tasks(id) | ON DELETE CASCADE |
| fk_se_service | service_id | services(id) | ON DELETE SET NULL |

#### Check Constraints

```text
infestation_level IN ('HIGH', 'MEDIUM', 'LOW')
```

---

### service_pest_types

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| display_order | INTEGER | YES | — |
| id | VARCHAR(50) | NO | — |
| is_active | BOOLEAN | NO | TRUE |
| name | VARCHAR(150) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| uk_service_pest_types_name | name |

---

### service_products

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| cost_per_visit | DOUBLE PRECISION | YES | — |
| coverage_sqft | DOUBLE PRECISION | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| dilution | VARCHAR(100) | YES | — |
| display_order | INTEGER | YES | — |
| est_cost_per_month | DOUBLE PRECISION | YES | — |
| id | VARCHAR(50) | NO | — |
| inventory_product_id | VARCHAR(50) | NO | — |
| is_manual_entry | BOOLEAN | NO | FALSE |
| price_per_uom | DOUBLE PRECISION | NO | — |
| required_qty | DOUBLE PRECISION | NO | — |
| service_id | VARCHAR(50) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sp_service | service_id | services(id) | ON DELETE CASCADE |
| fk_sp_product | inventory_product_id | inventory_products(id) | ON DELETE RESTRICT |

#### Indexes

| Name | Unique | Columns |
|---|---:|---|
| idx_service_products_service | NO | service_id |

---

### service_species

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| display_order | INTEGER | YES | — |
| id | VARCHAR(50) | NO | — |
| scientific_name | VARCHAR(300) | YES | — |
| service_id | VARCHAR(50) | NO | — |
| species_name | VARCHAR(200) | YES | — |
| updated_at | TIMESTAMPTZ | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_service_species_service | service_id | services(id) | ON DELETE CASCADE |

#### Indexes

| Name | Unique | Columns |
|---|---:|---|
| idx_service_species_service | NO | service_id |

---

### service_sub_categories

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| code | VARCHAR(50) | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| display_order | INTEGER | YES | — |
| id | VARCHAR(50) | NO | — |
| is_active | BOOLEAN | NO | TRUE |
| name | VARCHAR(100) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| uk_service_sub_categories_code | code |

---

### service_treatments

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| display_order | INTEGER | YES | — |
| id | VARCHAR(50) | NO | — |
| is_active | BOOLEAN | NO | TRUE |
| name | VARCHAR(200) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| uk_service_treatments_name | name |

---

### services

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| description | TEXT | NO | — |
| display_order | INTEGER | YES | — |
| duration_uom | VARCHAR(20) | NO | — |
| duration_value | DOUBLE PRECISION | NO | — |
| free_revisit_included | BOOLEAN | NO | FALSE |
| free_revisit_quantity | INTEGER | YES | — |
| id | VARCHAR(50) | NO | — |
| inactivated_by | VARCHAR(100) | YES | — |
| inactive_at | TIMESTAMPTZ | YES | — |
| inactive_reason | TEXT | YES | — |
| is_draft | BOOLEAN | NO | FALSE |
| price_type | VARCHAR(50) | NO | — |
| service_code | VARCHAR(50) | NO | — |
| service_name | VARCHAR(200) | NO | — |
| status | VARCHAR(20) | NO | 'ACTIVE' CHECK (status IN ('ACTIVE' |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |
| visits_per_month | DOUBLE PRECISION | YES | — |
| warranty_months | INTEGER | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| uk_services_code | service_code |
| uk_services_name | service_name |

#### Check Constraints

```text
price_type IN ('FIXED', 'AREA_BASED', 'INSPECTION')
duration_uom IN ('MINUTES', 'HOURS')
status IN ('ACTIVE', 'INACTIVE')
```

#### Indexes

| Name | Unique | Columns |
|---|---:|---|
| idx_services_status | NO | status |
| idx_services_price_type | NO | price_type |
| idx_services_created_at | NO | created_at |

---

### services_service_categories

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: service_id, service_category_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| service_category_id | VARCHAR(50) | NO | — |
| service_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_ssc_service | service_id | services(id) | ON DELETE CASCADE |
| fk_ssc_category | service_category_id | service_categories(id) | ON DELETE CASCADE |

---

### services_service_category_area

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: service_id, service_category_area_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| service_category_area_id | VARCHAR(50) | NO | — |
| service_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_ssca_service | service_id | services(id) | ON DELETE CASCADE |
| fk_ssca_area | service_category_area_id | service_category_area(id) | ON DELETE CASCADE |

---

### services_service_category_fixed

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: service_id, service_category_fixed_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| service_category_fixed_id | VARCHAR(50) | NO | — |
| service_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sscf_service | service_id | services(id) | ON DELETE CASCADE |
| fk_sscf_fixed | service_category_fixed_id | service_category_fixed(id) | ON DELETE CASCADE |

---

### services_service_category_inspection

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: service_id, service_category_inspection_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| service_category_inspection_id | VARCHAR(50) | NO | — |
| service_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_ssci_service | service_id | services(id) | ON DELETE CASCADE |
| fk_ssci_inspection | service_category_inspection_id | service_category_inspection(id) | ON DELETE CASCADE |

---

### services_service_custom_pricing_blocks

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: service_id, service_custom_pricing_block_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| service_custom_pricing_block_id | VARCHAR(50) | NO | — |
| service_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sscpb_service | service_id | services(id) | ON DELETE CASCADE |
| fk_sscpb_block | service_custom_pricing_block_id | service_custom_pricing_blocks(id) | ON DELETE CASCADE |

---

### services_service_pest_types

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: service_id, service_pest_type_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| service_id | VARCHAR(50) | NO | — |
| service_pest_type_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sspt_service | service_id | services(id) | ON DELETE CASCADE |
| fk_sspt_pest | service_pest_type_id | service_pest_types(id) | ON DELETE CASCADE |

---

### services_service_sub_categories

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: service_id, service_sub_category_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| service_id | VARCHAR(50) | NO | — |
| service_sub_category_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sssc_service | service_id | services(id) | ON DELETE CASCADE |
| fk_sssc_sub | service_sub_category_id | service_sub_categories(id) | ON DELETE CASCADE |

---

### services_service_treatments

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V14__service_management_module.sql
- Primary key: service_id, service_treatment_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| service_id | VARCHAR(50) | NO | — |
| service_treatment_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sst_service | service_id | services(id) | ON DELETE CASCADE |
| fk_sst_treatment | service_treatment_id | service_treatments(id) | ON DELETE CASCADE |

---

### stock_approval_logs

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V12__stock_management_core.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| action | VARCHAR(40) | NO | — |
| created_at | TIMESTAMPTZ | NO | NOW() |
| created_by | VARCHAR(80) | YES | — |
| id | BIGSERIAL | NO | — |
| new_status | VARCHAR(40) | YES | — |
| previous_status | VARCHAR(40) | YES | — |
| remarks | TEXT | YES | — |
| request_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_stock_approval_log_request | request_id | stock_requests(id) | ON DELETE CASCADE |

---

### stock_ledger

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V12__stock_management_core.sql, seravion-connect-backend/src/main/resources/db/mysql/module11_stock_management_hybrid_mysql.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| assets_qty | INT | NO | 0 |
| base_uom | VARCHAR(30) | YES | — |
| branch_id | VARCHAR(30) | NO | — |
| brand | VARCHAR(120) | YES | — |
| category | VARCHAR(50) | YES | — |
| consumable_qty | INT | NO | 0 |
| created_at | DATETIME | NO | CURRENT_TIMESTAMP |
| created_by | VARCHAR(80) | YES | — |
| deleted_at | DATETIME | YES | — |
| deleted_by | VARCHAR(80) | YES | — |
| hsn_code | VARCHAR(20) | YES | — |
| id | BIGINT AUTO_INCREMENT | NO | — |
| in_transit_qty | INT | NO | 0 |
| product_code | VARCHAR(50) | NO | — |
| product_id | VARCHAR(50) | NO | — |
| product_name | VARCHAR(255) | NO | — |
| resell_qty | INT | NO | 0 |
| reserved_qty | INT | NO | 0 |
| status | VARCHAR(30) | NO | 'AVAILABLE' |
| updated_at | DATETIME | YES | — |
| updated_by | VARCHAR(80) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_stock_ledger_product | product_id | inventory_products(id) | ON DELETE RESTRICT |
| fk_stock_ledger_product | product_id | inventory_products(id) | ON UPDATE CASCADE ON DELETE RESTRICT ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS central_stock_entries ( id BIGINT AUTO_INCREMENT PRIMARY KEY, entry_id VARCHAR(40) NOT NULL UNIQUE, product_id VARCHAR(50) NOT NULL, product_code VARCHAR(50) NOT NULL, product_name VARCHAR(255) NOT NULL, hsn_code VARCHAR(20), base_uom VARCHAR(30), total_qty INT NOT NULL, assets_qty INT NOT NULL DEFAULT 0, consumable_qty INT NOT NULL DEFAULT 0, resell_qty INT NOT NULL DEFAULT 0, asset_id_generation VARCHAR(15), asset_id_prefix VARCHAR(30), asset_sequence_start INT, assignment_type VARCHAR(30), default_assignment VARCHAR(50), supplier_name VARCHAR(200), purchase_order_ref VARCHAR(80), invoice_number VARCHAR(80) UNIQUE, invoice_date DATE, invoice_amount DECIMAL(14,2), tax_amount DECIMAL(14,2), total_with_tax DECIMAL(14,2), invoice_copy_data LONGBLOB, invoice_copy_file_name VARCHAR(255), invoice_copy_content_type VARCHAR(100), batch_number VARCHAR(80), manufacturing_date DATE, expiry_date DATE, status VARCHAR(30) NOT NULL DEFAULT 'ACTIVE', created_by VARCHAR(80), created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP, updated_by VARCHAR(80), updated_at DATETIME NULL ON UPDATE CURRENT_TIMESTAMP, deleted_by VARCHAR(80), deleted_at DATETIME, CONSTRAINT chk_central_qty_split CHECK (assets_qty + consumable_qty + resell_qty = total_qty), CONSTRAINT fk_central_entry_product FOREIGN KEY (product_id) REFERENCES inventory_products(id) ON UPDATE CASCADE ON DELETE RESTRICT ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS asset_units ( id BIGINT AUTO_INCREMENT PRIMARY KEY, asset_id VARCHAR(60) NOT NULL UNIQUE, product_id VARCHAR(50) NOT NULL, product_code VARCHAR(50) NOT NULL, product_name VARCHAR(255) NOT NULL, branch_id VARCHAR(30) NOT NULL, assigned_user_id BIGINT, assigned_to_name VARCHAR(160), assignment_mode VARCHAR(30), `condition` VARCHAR(30) NOT NULL DEFAULT 'GOOD', status VARCHAR(30) NOT NULL DEFAULT 'AVAILABLE', created_by VARCHAR(80), created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP, updated_by VARCHAR(80), updated_at DATETIME NULL ON UPDATE CURRENT_TIMESTAMP, CONSTRAINT fk_asset_unit_product FOREIGN KEY (product_id) REFERENCES inventory_products(id) ON UPDATE CASCADE ON DELETE RESTRICT ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS stock_requests ( id BIGINT AUTO_INCREMENT PRIMARY KEY, request_id VARCHAR(40) NOT NULL UNIQUE, request_type VARCHAR(30) NOT NULL, direction VARCHAR(15) NOT NULL, from_branch_id VARCHAR(30) NOT NULL, to_branch_id VARCHAR(30) NOT NULL, requested_by_user_id BIGINT, requested_by_name VARCHAR(160), priority VARCHAR(15) NOT NULL DEFAULT 'NORMAL', required_by_date DATE NOT NULL, purpose TEXT NOT NULL, notes_for_approver TEXT, sent_to TEXT, status VARCHAR(40) NOT NULL DEFAULT 'DRAFT', approval_type VARCHAR(30), alternative_source VARCHAR(30), dispatch_date DATE, expected_delivery_date DATE, carrier VARCHAR(120), lr_number VARCHAR(80), remarks TEXT, issue_type VARCHAR(40), issue_description TEXT, issue_resolution_status VARCHAR(40), created_by VARCHAR(80), created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP, updated_by VARCHAR(80), updated_at DATETIME NULL ON UPDATE CURRENT_TIMESTAMP, deleted_by VARCHAR(80), deleted_at DATETIME ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS stock_request_items ( id BIGINT AUTO_INCREMENT PRIMARY KEY, request_id BIGINT NOT NULL, product_id VARCHAR(50) NOT NULL, product_code VARCHAR(50) NOT NULL, product_name VARCHAR(255) NOT NULL, base_uom VARCHAR(30), assets_req_qty INT NOT NULL DEFAULT 0, consumable_req_qty INT NOT NULL DEFAULT 0, resell_req_qty INT NOT NULL DEFAULT 0, assets_appr_qty INT NOT NULL DEFAULT 0, consumable_appr_qty INT NOT NULL DEFAULT 0, resell_appr_qty INT NOT NULL DEFAULT 0, estimated_cost DECIMAL(14,2), tax_amount DECIMAL(14,2), item_purpose VARCHAR(250), alternative_source VARCHAR(30), CONSTRAINT fk_request_items_request FOREIGN KEY (request_id) REFERENCES stock_requests(id) ON DELETE CASCADE, CONSTRAINT fk_request_items_product FOREIGN KEY (product_id) REFERENCES inventory_products(id) ON UPDATE CASCADE ON DELETE RESTRICT ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS stock_transfers ( id BIGINT AUTO_INCREMENT PRIMARY KEY, transfer_id VARCHAR(40) NOT NULL UNIQUE, reference_request_id VARCHAR(40), from_branch_id VARCHAR(30) NOT NULL, to_branch_id VARCHAR(30) NOT NULL, transfer_type VARCHAR(20) NOT NULL, strategy VARCHAR(30), status VARCHAR(40) NOT NULL DEFAULT 'DRAFT', dispatch_date DATE, expected_delivery_date DATE, carrier VARCHAR(120), lr_number VARCHAR(80), remarks TEXT, created_by VARCHAR(80), created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP, updated_by VARCHAR(80), updated_at DATETIME NULL ON UPDATE CURRENT_TIMESTAMP ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS stock_transfer_items ( id BIGINT AUTO_INCREMENT PRIMARY KEY, transfer_id BIGINT NOT NULL, product_id VARCHAR(50) NOT NULL, product_code VARCHAR(50) NOT NULL, product_name VARCHAR(255) NOT NULL, assets_qty INT NOT NULL DEFAULT 0, consumable_qty INT NOT NULL DEFAULT 0, resell_qty INT NOT NULL DEFAULT 0, source_branch_id VARCHAR(30), CONSTRAINT fk_transfer_items_transfer FOREIGN KEY (transfer_id) REFERENCES stock_transfers(id) ON DELETE CASCADE, CONSTRAINT fk_transfer_items_product FOREIGN KEY (product_id) REFERENCES inventory_products(id) ON UPDATE CASCADE ON DELETE RESTRICT ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS stock_transfer_assets ( id BIGINT AUTO_INCREMENT PRIMARY KEY, transfer_id BIGINT NOT NULL, asset_id VARCHAR(60) NOT NULL, condition_at_dispatch VARCHAR(30), transfer_with VARCHAR(40), destination_user_id BIGINT, destination_user_name VARCHAR(160), condition_at_receipt VARCHAR(30), receipt_status VARCHAR(30), CONSTRAINT fk_transfer_assets_transfer FOREIGN KEY (transfer_id) REFERENCES stock_transfers(id) ON DELETE CASCADE ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS stock_movement_logs ( id BIGINT AUTO_INCREMENT PRIMARY KEY, reference_type VARCHAR(30) NOT NULL, reference_id VARCHAR(40) NOT NULL, branch_id VARCHAR(30), product_id VARCHAR(50), stock_type VARCHAR(20), quantity_delta INT NOT NULL DEFAULT 0, action VARCHAR(50) NOT NULL, remarks TEXT, created_by VARCHAR(80), created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ) ENGINE=InnoDB; CREATE TABLE IF NOT EXISTS stock_approval_logs ( id BIGINT AUTO_INCREMENT PRIMARY KEY, request_id BIGINT NOT NULL, action VARCHAR(40) NOT NULL, previous_status VARCHAR(40), new_status VARCHAR(40), remarks TEXT, created_by VARCHAR(80), created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP, CONSTRAINT fk_approval_logs_request FOREIGN KEY (request_id) REFERENCES stock_requests(id) ON DELETE CASCADE ) ENGINE=InnoDB; CREATE INDEX idx_stock_ledger_product ON stock_ledger(product_id |

#### Unique Constraints

| Name | Columns |
|---|---|
| uq_stock_ledger_branch_product | branch_id, product_id |

#### Check Constraints

```text
assets_qty >= 0
consumable_qty >= 0
resell_qty >= 0
in_transit_qty >= 0
reserved_qty >= 0
chk_stock_ledger_qty: assets_qty >= 0 AND consumable_qty >= 0 AND resell_qty >= 0 AND in_transit_qty >= 0 AND reserved_qty >= 0
```

#### Indexes

| Name | Unique | Columns |
|---|---:|---|
| idx_stock_ledger_product | NO | product_id |

---

### stock_movement_logs

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V12__stock_management_core.sql, seravion-connect-backend/src/main/resources/db/mysql/module11_stock_management_hybrid_mysql.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| action | VARCHAR(50) | NO | — |
| branch_id | VARCHAR(30) | YES | — |
| created_at | TIMESTAMPTZ | NO | NOW() |
| created_by | VARCHAR(80) | YES | — |
| id | BIGSERIAL | NO | — |
| product_id | VARCHAR(50) | YES | — |
| quantity_delta | INTEGER | NO | 0 |
| reference_id | VARCHAR(40) | NO | — |
| reference_type | VARCHAR(30) | NO | — |
| remarks | TEXT | YES | — |
| stock_type | VARCHAR(20) | YES | — |

#### Indexes

| Name | Unique | Columns |
|---|---:|---|
| idx_stock_movement_ref | NO | reference_type, reference_id |

---

### stock_request_items

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V12__stock_management_core.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| alternative_source | VARCHAR(30) | YES | — |
| assets_appr_qty | INTEGER | NO | 0 CHECK (assets_appr_qty >= 0) |
| assets_req_qty | INTEGER | NO | 0 CHECK (assets_req_qty >= 0) |
| base_uom | VARCHAR(30) | YES | — |
| consumable_appr_qty | INTEGER | NO | 0 CHECK (consumable_appr_qty >= 0) |
| consumable_req_qty | INTEGER | NO | 0 CHECK (consumable_req_qty >= 0) |
| estimated_cost | NUMERIC(14,2) | YES | — |
| id | BIGSERIAL | NO | — |
| item_purpose | VARCHAR(250) | YES | — |
| product_code | VARCHAR(50) | NO | — |
| product_id | VARCHAR(50) | NO | — |
| product_name | VARCHAR(255) | NO | — |
| request_id | BIGINT | NO | — |
| resell_appr_qty | INTEGER | NO | 0 CHECK (resell_appr_qty >= 0) |
| resell_req_qty | INTEGER | NO | 0 CHECK (resell_req_qty >= 0) |
| tax_amount | NUMERIC(14,2) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_stock_request_item_request | request_id | stock_requests(id) | ON DELETE CASCADE |
| fk_stock_request_item_product | product_id | inventory_products(id) | ON DELETE RESTRICT |

#### Check Constraints

```text
assets_req_qty >= 0
consumable_req_qty >= 0
resell_req_qty >= 0
assets_appr_qty >= 0
consumable_appr_qty >= 0
resell_appr_qty >= 0
```

---

### stock_request_recipients

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V67__stock_request_recipients.sql, seravion-connect-backend/src/main/resources/db/migration/tenant/V68__update_receipts_users_stock.sql
- Primary key: stock_request_id, recipient_user_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | NOW() |
| id | BIGSERIAL | NO | — |
| recipient_email | VARCHAR(255) | NO | — |
| recipient_user_id | BIGINT | NO | — |
| request_id | BIGINT | NO | — |
| stock_request_id | VARCHAR(40) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_stock_request_recipients_request | request_id | stock_requests(id) | ON DELETE CASCADE |
| fk_srr_stock_request | stock_request_id | stock_requests(request_id) | ON DELETE CASCADE |
| fk_srr_user | recipient_user_id | users(id) | ON DELETE CASCADE |

#### Unique Constraints

| Name | Columns |
|---|---|
| uq_stock_request_recipients_request_email | request_id, recipient_email |

---

### stock_requests

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V12__stock_management_core.sql, seravion-connect-backend/src/main/resources/db/mysql/module11_stock_management_hybrid_mysql.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| alternative_source | VARCHAR(30) | YES | — |
| approval_type | VARCHAR(30) | YES | — |
| carrier | VARCHAR(120) | YES | — |
| created_at | TIMESTAMPTZ | NO | NOW() |
| created_by | VARCHAR(80) | YES | — |
| deleted_at | TIMESTAMPTZ | YES | — |
| deleted_by | VARCHAR(80) | YES | — |
| direction | VARCHAR(15) | NO | — |
| dispatch_date | DATE | YES | — |
| expected_delivery_date | DATE | YES | — |
| from_branch_id | VARCHAR(30) | NO | — |
| id | BIGSERIAL | NO | — |
| issue_description | TEXT | YES | — |
| issue_resolution_status | VARCHAR(40) | YES | — |
| issue_type | VARCHAR(40) | YES | — |
| lr_number | VARCHAR(80) | YES | — |
| notes_for_approver | TEXT | YES | — |
| priority | VARCHAR(15) | NO | 'NORMAL' |
| purpose | TEXT | NO | — |
| remarks | TEXT | YES | — |
| request_id | VARCHAR(40) | NO | — |
| request_type | VARCHAR(30) | NO | — |
| requested_by_name | VARCHAR(160) | YES | — |
| requested_by_user_id | BIGINT | YES | — |
| required_by_date | DATE | NO | — |
| sent_to | TEXT | YES | — |
| status | VARCHAR(40) | NO | 'DRAFT' |
| to_branch_id | VARCHAR(30) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(80) | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | request_id |

#### Indexes

| Name | Unique | Columns |
|---|---:|---|
| idx_stock_requests_status | NO | status |

---

### stock_transfer_assets

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V12__stock_management_core.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| asset_id | VARCHAR(60) | NO | — |
| condition_at_dispatch | VARCHAR(30) | YES | — |
| condition_at_receipt | VARCHAR(30) | YES | — |
| destination_user_id | BIGINT | YES | — |
| destination_user_name | VARCHAR(160) | YES | — |
| id | BIGSERIAL | NO | — |
| receipt_status | VARCHAR(30) | YES | — |
| transfer_id | BIGINT | NO | — |
| transfer_with | VARCHAR(40) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_stock_transfer_asset_transfer | transfer_id | stock_transfers(id) | ON DELETE CASCADE |

---

### stock_transfer_items

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V12__stock_management_core.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| assets_qty | INTEGER | NO | 0 CHECK (assets_qty >= 0) |
| consumable_qty | INTEGER | NO | 0 CHECK (consumable_qty >= 0) |
| id | BIGSERIAL | NO | — |
| product_code | VARCHAR(50) | NO | — |
| product_id | VARCHAR(50) | NO | — |
| product_name | VARCHAR(255) | NO | — |
| resell_qty | INTEGER | NO | 0 CHECK (resell_qty >= 0) |
| source_branch_id | VARCHAR(30) | YES | — |
| transfer_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_stock_transfer_item_transfer | transfer_id | stock_transfers(id) | ON DELETE CASCADE |
| fk_stock_transfer_item_product | product_id | inventory_products(id) | ON DELETE RESTRICT |

#### Check Constraints

```text
assets_qty >= 0
consumable_qty >= 0
resell_qty >= 0
```

---

### stock_transfers

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V12__stock_management_core.sql, seravion-connect-backend/src/main/resources/db/mysql/module11_stock_management_hybrid_mysql.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| carrier | VARCHAR(120) | YES | — |
| created_at | TIMESTAMPTZ | NO | NOW() |
| created_by | VARCHAR(80) | YES | — |
| dispatch_date | DATE | YES | — |
| expected_delivery_date | DATE | YES | — |
| from_branch_id | VARCHAR(30) | NO | — |
| id | BIGSERIAL | NO | — |
| lr_number | VARCHAR(80) | YES | — |
| reference_request_id | VARCHAR(40) | YES | — |
| remarks | TEXT | YES | — |
| status | VARCHAR(40) | NO | 'DRAFT' |
| strategy | VARCHAR(30) | YES | — |
| to_branch_id | VARCHAR(30) | NO | — |
| transfer_id | VARCHAR(40) | NO | — |
| transfer_type | VARCHAR(20) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(80) | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | transfer_id |

#### Indexes

| Name | Unique | Columns |
|---|---:|---|
| idx_stock_transfers_status | NO | status |

---

### subscription_plans

- Sources: seravion-connect-backend/src/main/resources/db/migration/public/V7__subscription_plans.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| branch_count | INTEGER | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(30) | YES | — |
| deleted_at | TIMESTAMPTZ | YES | — |
| deleted_by | VARCHAR(30) | YES | — |
| description | TEXT | YES | — |
| duration_type | VARCHAR(20) | NO | — |
| id | VARCHAR(30) | NO | — |
| plan_name | VARCHAR(100) | NO | — |
| price_per_branch | NUMERIC(12,2) | NO | — |
| price_per_technician | NUMERIC(12,2) | NO | — |
| status | VARCHAR(20) | NO | 'ACTIVE' |
| technician_count | INTEGER | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(30) | YES | — |
| valid_from | DATE | YES | — |
| valid_to | DATE | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | plan_name |

#### Check Constraints

```text
branch_count > 0
technician_count > 0
price_per_branch > 0
price_per_technician > 0
chk_subscription_plan_duration_type: duration_type IN ('MONTHLY', 'QUARTERLY', 'ANNUAL', 'CUSTOM')
chk_subscription_plan_status: status IN ('ACTIVE', 'INACTIVE')
chk_subscription_plan_valid_dates: valid_to IS NULL OR valid_from IS NULL OR valid_to >= valid_from
```

---

### support_sla_settings

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V27__customer_support_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| id | SMALLINT | NO | 1 CHECK (id = 1) |
| resolution_risk_threshold_pct | INTEGER | NO | 80 CHECK (resolution_risk_threshold_pct > 0 AND resolution_risk_threshold_pct < 100) |
| response_sla_hours | INTEGER | NO | 2 |
| updated_at | TIMESTAMPTZ | NO | now() |
| updated_by | VARCHAR(100) | YES | — |

#### Check Constraints

```text
id = 1
resolution_risk_threshold_pct > 0 AND resolution_risk_threshold_pct < 100
```

---

### support_ticket_activities

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V27__customer_support_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| activity_type | VARCHAR(50) | NO | — |
| detail | TEXT | YES | — |
| id | BIGSERIAL | NO | — |
| is_internal | BOOLEAN | NO | FALSE |
| metadata_json | JSONB | YES | — |
| performed_at | TIMESTAMPTZ | NO | now() |
| performed_by_label | VARCHAR(200) | YES | — |
| performed_by_user_id | BIGINT | YES | — |
| summary | TEXT | NO | — |
| ticket_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_sta_ticket | ticket_id | support_tickets(id) | ON DELETE CASCADE |
| fk_sta_user | performed_by_user_id | users(id) | ON DELETE SET NULL |

---

### support_ticket_assignment_history

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V27__customer_support_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| assigned_at | TIMESTAMPTZ | NO | now() |
| assigned_by | VARCHAR(100) | YES | — |
| assignment_note | TEXT | YES | — |
| from_user_id | BIGINT | YES | — |
| id | BIGSERIAL | NO | — |
| ticket_id | VARCHAR(50) | NO | — |
| to_role_code | VARCHAR(50) | NO | — |
| to_user_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_stah_ticket | ticket_id | support_tickets(id) | ON DELETE CASCADE |
| fk_stah_from | from_user_id | users(id) | ON DELETE SET NULL |
| fk_stah_to | to_user_id | users(id) | ON DELETE RESTRICT |

---

### support_ticket_attachments

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V27__customer_support_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| file_path | VARCHAR(500) | NO | — |
| id | VARCHAR(50) | NO | — |
| mime_type | VARCHAR(100) | YES | — |
| original_name | VARCHAR(255) | YES | — |
| phase | VARCHAR(20) | NO | — |
| size_bytes | BIGINT | YES | — |
| ticket_id | VARCHAR(50) | NO | — |
| uploaded_at | TIMESTAMPTZ | NO | now() |
| uploaded_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_attach_ticket | ticket_id | support_tickets(id) | ON DELETE CASCADE |

#### Check Constraints

```text
phase IN ('RAISE', 'RESOLUTION', 'CLOSE', 'NOTE')
```

---

### support_ticket_tasks

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V27__customer_support_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| id | VARCHAR(50) | NO | — |
| linked_at | TIMESTAMPTZ | NO | now() |
| linked_by | VARCHAR(100) | YES | — |
| task_id | VARCHAR(50) | NO | — |
| ticket_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_stt_ticket | ticket_id | support_tickets(id) | ON DELETE CASCADE |
| fk_stt_task | task_id | tasks(id) | ON DELETE CASCADE |

#### Unique Constraints

| Name | Columns |
|---|---|
| uq_stt_ticket_task | ticket_id, task_id |

---

### support_ticket_types

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V27__customer_support_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| active | BOOLEAN | NO | TRUE |
| code | VARCHAR(80) | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| display_order | INTEGER | NO | 0 |
| id | VARCHAR(50) | NO | — |
| label | VARCHAR(200) | NO | — |
| updated_at | TIMESTAMPTZ | NO | now() |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | code |

---

### support_tickets

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V27__customer_support_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| assigned_user_id | BIGINT | YES | — |
| assignee_role_code | VARCHAR(50) | YES | — |
| branch_id | VARCHAR(30) | NO | — |
| close_reason | VARCHAR(80) | YES | — |
| closed_at | TIMESTAMPTZ | YES | — |
| closed_by | VARCHAR(100) | YES | — |
| closure_remarks | TEXT | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| customer_id | VARCHAR(50) | NO | — |
| description | TEXT | NO | — |
| escalation_level | VARCHAR(20) | NO | 'NONE' CHECK (escalation_level IN ('NONE' |
| expected_resolution_date | DATE | NO | — |
| expected_resolution_time | TIME | NO | — |
| first_response_at | TIMESTAMPTZ | YES | — |
| id | VARCHAR(50) | NO | — |
| pause_started_at | TIMESTAMPTZ | YES | — |
| priority | VARCHAR(20) | NO | — |
| related_task_id | VARCHAR(50) | YES | — |
| reported_by_name | VARCHAR(200) | NO | — |
| reported_by_phone | VARCHAR(20) | NO | — |
| resolution_code | VARCHAR(50) | YES | — |
| resolution_expected_at | TIMESTAMPTZ | NO | — |
| resolution_notes | TEXT | YES | — |
| resolution_sla_breached | BOOLEAN | NO | FALSE |
| resolve_customer_feedback | TEXT | YES | — |
| resolve_customer_rating | INTEGER | YES | — |
| resolved_at | TIMESTAMPTZ | YES | — |
| resolved_by | VARCHAR(100) | YES | — |
| response_sla_breached | BOOLEAN | NO | FALSE |
| response_sla_deadline_at | TIMESTAMPTZ | NO | — |
| response_sla_met | BOOLEAN | YES | — |
| sales_order_id | VARCHAR(50) | YES | — |
| status | VARCHAR(30) | NO | 'OPEN' CHECK (status IN ( 'OPEN' |
| subject | VARCHAR(100) | NO | — |
| ticket_number | VARCHAR(50) | NO | — |
| ticket_type_id | VARCHAR(50) | NO | — |
| total_paused_seconds | INTEGER | NO | 0 |
| updated_at | TIMESTAMPTZ | NO | now() |
| updated_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_st_customer | customer_id | customers(id) | ON DELETE RESTRICT |
| fk_st_branch | branch_id | branches(id) | ON DELETE RESTRICT |
| fk_st_so | sales_order_id | sales_orders(id) | ON DELETE SET NULL |
| fk_st_related_task | related_task_id | tasks(id) | ON DELETE SET NULL |
| fk_st_ticket_type | ticket_type_id | support_ticket_types(id) | ON DELETE RESTRICT |
| fk_st_assignee_user | assigned_user_id | users(id) | ON DELETE SET NULL |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | ticket_number |

#### Check Constraints

```text
priority IN ('NORMAL', 'HIGH', 'URGENT', 'CRITICAL')
status IN ( 'OPEN', 'ASSIGNED', 'IN_PROGRESS', 'PAUSED', 'RESOLVED', 'CLOSED' )
escalation_level IN ('NONE', 'L1', 'L2', 'L3')
resolution_code IS NULL OR resolution_code IN ( 'SERVICE_RESOLVED_SUCCESS', 'FALSE_ALARM', 'DUPLICATE_TICKET', 'UNRESOLVED_CLOSED' )
resolve_customer_rating IS NULL OR (resolve_customer_rating >= 1 AND resolve_customer_rating <= 5)
close_reason IS NULL OR close_reason IN ( 'RESOLVED_CUSTOMER_SATISFACTION', 'DUPLICATE_REQUEST', 'CUSTOMER_NON_RESPONSIVE', 'OUT_OF_SCOPE' )
```

---

### task_audit_logs

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V25__task_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| action | VARCHAR(100) | NO | — |
| details | TEXT | YES | — |
| id | BIGSERIAL | NO | — |
| performed_at | TIMESTAMPTZ | NO | now() |
| performed_by | VARCHAR(100) | YES | — |
| task_id | VARCHAR(50) | NO | — |

---

### task_customer_feedback

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V35__task_photos_evidence_columns.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| customer_feedback | TEXT | YES | — |
| customer_name | VARCHAR(255) | NO | — |
| customer_phone | VARCHAR(30) | NO | — |
| id | VARCHAR(50) | NO | — |
| ratings | INTEGER | NO | — |
| task_id | VARCHAR(50) | NO | — |
| technician_id | BIGINT | NO | — |
| updated_at | TIMESTAMPTZ | NO | now() |
| updated_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_tcf_task | task_id | tasks(id) | ON DELETE CASCADE |
| fk_tcf_technician | technician_id | users(id) | — |

#### Check Constraints

```text
ratings BETWEEN 1 AND 5
```

---

### task_materials

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V25__task_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| hsn_code | VARCHAR(20) | YES | — |
| id | VARCHAR(50) | NO | — |
| product_id | VARCHAR(50) | NO | — |
| product_name | VARCHAR(255) | NO | — |
| required_qty | NUMERIC(14,3) | NO | — |
| std_qty | NUMERIC(14,3) | YES | — |
| task_id | VARCHAR(50) | NO | — |
| uom | VARCHAR(30) | YES | — |
| used_qty | NUMERIC(14,3) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_tm_task | task_id | tasks(id) | ON DELETE CASCADE |

---

### task_photos

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V25__task_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| file_path | VARCHAR(500) | NO | — |
| id | VARCHAR(50) | NO | — |
| photo_type | VARCHAR(30) | NO | — |
| task_id | VARCHAR(50) | NO | — |
| uploaded_at | TIMESTAMPTZ | NO | now() |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_tp_task | task_id | tasks(id) | ON DELETE CASCADE |

#### Check Constraints

```text
photo_type IN ('BEFORE', 'AFTER', 'TREATMENT')
```

---

### task_technicians

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V25__task_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| employee_name | VARCHAR(200) | NO | — |
| id | VARCHAR(50) | NO | — |
| is_primary | BOOLEAN | NO | FALSE |
| role_name | VARCHAR(100) | YES | — |
| task_id | VARCHAR(50) | NO | — |
| user_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_tt_task | task_id | tasks(id) | ON DELETE CASCADE |
| fk_tt_user | user_id | users(id) | — |

---

### tasks

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V25__task_management_module.sql, seravion-connect-backend/src/main/resources/db/migration/tenant/V27__customer_support_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| actual_end_at | TIMESTAMPTZ | YES | — |
| actual_start_at | TIMESTAMPTZ | YES | — |
| area_sqft | NUMERIC(12,2) | YES | — |
| branch_id | VARCHAR(30) | NO | — |
| completion_notes | TEXT | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| customer_feedback | TEXT | YES | — |
| customer_id | VARCHAR(50) | NO | — |
| customer_name | VARCHAR(255) | NO | — |
| customer_rating | INTEGER | YES | — |
| end_time | TIME | NO | — |
| estimated_duration_mins | INTEGER | YES | — |
| feedback_at | TIMESTAMPTZ | YES | — |
| id | VARCHAR(50) | NO | — |
| priority | VARCHAR(20) | NO | 'NORMAL' CHECK (priority IN ('NORMAL' |
| sales_order_id | VARCHAR(50) | YES | — |
| scheduled_date | DATE | NO | — |
| service_category | VARCHAR(100) | YES | — |
| service_subcategory | VARCHAR(100) | YES | — |
| service_type_name | VARCHAR(200) | YES | — |
| site_address | TEXT | YES | — |
| site_contact_mobile | VARCHAR(20) | YES | — |
| site_contact_name | VARCHAR(200) | YES | — |
| site_id | VARCHAR(50) | YES | — |
| site_name | VARCHAR(255) | NO | — |
| so_site_service_id | VARCHAR(50) | YES | — |
| source_type | VARCHAR(30) | NO | — |
| start_time | TIME | NO | — |
| status | VARCHAR(30) | NO | — |
| task_number | VARCHAR(50) | NO | — |
| task_type | VARCHAR(20) | NO | — |
| ticket_id | VARCHAR(50) | YES | — |
| updated_at | TIMESTAMPTZ | NO | now() |
| updated_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_tasks_support_ticket | ticket_id | support_tickets(id) | ON DELETE SET NULL |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | task_number |

#### Check Constraints

```text
task_type IN ('NORMAL', 'RE_TASK')
source_type IN ('SALES_ORDER', 'CUSTOMER_TICKET', 'MANUAL')
status IN ('PENDING', 'IN_PROGRESS', 'COMPLETED', 'OVERDUE', 'CANCELLED')
priority IN ('NORMAL', 'URGENT', 'CRITICAL')
customer_rating BETWEEN 1 AND 5
```

---

### tax_types

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V5__tax_hsn_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| applicability | VARCHAR(20) | NO | — |
| change_reason | VARCHAR(255) | YES | — |
| created_at | TIMESTAMPTZ | NO | NOW() |
| created_by | VARCHAR(50) | NO | — |
| default_rate | NUMERIC(5,2) | NO | — |
| deleted_at | TIMESTAMPTZ | YES | — |
| deleted_by | VARCHAR(50) | YES | — |
| description | VARCHAR(500) | YES | — |
| effective_from | DATE | NO | — |
| id | BIGINT | NO | — |
| status | VARCHAR(20) | NO | 'ACTIVE' |
| tax_category | VARCHAR(20) | NO | — |
| tax_name | VARCHAR(100) | NO | — |
| tax_type_code | VARCHAR(30) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(50) | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | tax_type_code |
| uq_tax_name | tax_name |

#### Check Constraints

```text
default_rate >= 0 AND default_rate <= 100
chk_tax_category: tax_category IN ('CENTRAL', 'STATE', 'INTEGRATED', 'CESS')
chk_applicability: applicability IN ('GOODS', 'SERVICES', 'BOTH')
chk_status: status IN ('ACTIVE', 'INACTIVE')
```

---

### technician_observation_hygiene_picks

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V35__task_photos_evidence_columns.sql
- Primary key: section_id, hygiene_option_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| hygiene_option_id | VARCHAR(50) | NO | — |
| section_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_tohp_section | section_id | technician_observation_sections(id) | ON DELETE CASCADE |
| fk_tohp_option | hygiene_option_id | observation_options_hygiene(id) | ON DELETE RESTRICT |

---

### technician_observation_pest_picks

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V35__task_photos_evidence_columns.sql
- Primary key: section_id, pest_option_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| pest_option_id | VARCHAR(50) | NO | — |
| section_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_topp_section | section_id | technician_observation_sections(id) | ON DELETE CASCADE |
| fk_topp_option | pest_option_id | observation_options_pest_sighting(id) | ON DELETE RESTRICT |

---

### technician_observation_sections

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V35__task_photos_evidence_columns.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| category | VARCHAR(40) | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| found | BOOLEAN | NO | — |
| id | VARCHAR(50) | NO | — |
| location_area | TEXT | NO | — |
| other_notes | TEXT | YES | — |
| task_id | VARCHAR(50) | NO | — |
| updated_at | TIMESTAMPTZ | NO | now() |
| updated_by | VARCHAR(100) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_tos_task | task_id | tasks(id) | ON DELETE CASCADE |

#### Unique Constraints

| Name | Columns |
|---|---|
| uq_tos_task_category | task_id, category |

#### Check Constraints

```text
category IN ('STRUCTURAL_GAPS', 'HYGIENE_SANITATION', 'PEST_SIGHTING')
```

---

### technician_observation_structural_picks

- Junction table: yes
- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V35__task_photos_evidence_columns.sql
- Primary key: section_id, structural_option_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| section_id | VARCHAR(50) | NO | — |
| structural_option_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_tosp_section | section_id | technician_observation_sections(id) | ON DELETE CASCADE |
| fk_tosp_option | structural_option_id | observation_options_structural(id) | ON DELETE RESTRICT |

---

### technician_tracking

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V34__technician_tracking.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| id | BIGSERIAL | NO | — |
| latitude | NUMERIC(10, 7) | NO | — |
| local_date | DATE | NO | — |
| longitude | NUMERIC(10, 7) | NO | — |
| recorded_at | TIMESTAMPTZ | NO | now() |
| task_id | VARCHAR(50) | YES | — |
| technician_status | VARCHAR(50) | NO | — |
| user_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_technician_tracking_user | user_id | users(id) | — |
| fk_technician_tracking_task | task_id | tasks(id) | ON DELETE SET NULL |

---

### user_additional_data

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V8__employee_management.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| aadhar_number | VARCHAR(12) | YES | — |
| commission_percentage | NUMERIC(5,2) | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| grade_level | VARCHAR(50) | YES | — |
| id | BIGSERIAL | NO | — |
| id_card_number | VARCHAR(50) | YES | — |
| pan_number | VARCHAR(10) | YES | — |
| photo_url | VARCHAR(500) | YES | — |
| shift_type | VARCHAR(50) | YES | — |
| target_amount | NUMERIC(14,2) | YES | — |
| uan_number | VARCHAR(12) | YES | — |
| updated_at | TIMESTAMPTZ | NO | now() |
| updated_by | VARCHAR(100) | YES | — |
| user_id | BIGINT | NO | — |
| weekly_off | VARCHAR(50) | YES | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_user_additional_user | user_id | users(id) | ON DELETE CASCADE |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | user_id |

---

### user_branches

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V8__employee_management.sql
- Primary key: user_id, branch_id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| branch_id | VARCHAR(30) | NO | — |
| user_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_ub_user | user_id | users(id) | ON DELETE CASCADE |

---

### user_documents

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V8__employee_management.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| document_type | VARCHAR(60) | NO | — |
| file_path | VARCHAR(500) | NO | — |
| file_size_bytes | BIGINT | YES | — |
| id | BIGSERIAL | NO | — |
| mime_type | VARCHAR(100) | YES | — |
| original_file_name | VARCHAR(255) | YES | — |
| uploaded_at | TIMESTAMPTZ | NO | now() |
| uploaded_by | VARCHAR(100) | YES | — |
| user_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_user_doc_user | user_id | users(id) | ON DELETE CASCADE |

---

### user_leave_details

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V8__employee_management.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| annual_leave_allocation | INT | NO | 0 |
| carry_forward_allowed | BOOLEAN | NO | FALSE |
| casual_leave | INT | NO | 0 |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| id | BIGSERIAL | NO | — |
| leave_approval_role_id | BIGINT | YES | — |
| leave_reset_cycle | VARCHAR(20) | NO | — |
| max_carry_forward_days | INT | YES | — |
| paid_leave | INT | NO | 0 |
| sick_leave | INT | NO | 0 |
| updated_at | TIMESTAMPTZ | NO | now() |
| updated_by | VARCHAR(100) | YES | — |
| user_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_user_leave_user | user_id | users(id) | ON DELETE CASCADE |
| fk_user_leave_approval_role | leave_approval_role_id | roles(id) | ON DELETE SET NULL |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | user_id |

#### Check Constraints

```text
leave_reset_cycle IN ('YEARLY', 'MONTHLY', 'CUSTOM')
```

---

### user_permissions

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V4__user_permission.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| action_id | BIGINT | NO | — |
| allowed | BOOLEAN | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| id | BIGSERIAL | NO | — |
| module_id | BIGINT | NO | — |
| updated_at | TIMESTAMPTZ | NO | now() |
| user_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_up_user | user_id | users(id) | ON DELETE CASCADE |
| fk_up_module | module_id | modules(id) | ON DELETE CASCADE |
| fk_up_action | action_id | actions(id) | ON DELETE CASCADE |

#### Unique Constraints

| Name | Columns |
|---|---|
| uk_up_user_module_action | user_id, module_id, action_id |

---

### user_profile_extension

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V50__view_profile.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMP | YES | CURRENT_TIMESTAMP |
| employee_id | VARCHAR(50) | NO | — |
| id | UUID | NO | gen_random_uuid() |
| profile_photo_url | TEXT | YES | — |
| updated_at | TIMESTAMP | YES | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | employee_id |

---

### user_salary_details

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V8__employee_management.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| account_number | VARCHAR(20) | NO | — |
| bank_name | VARCHAR(150) | NO | — |
| basic_salary | NUMERIC(12,2) | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| deductions | NUMERIC(12,2) | NO | 0.00 |
| esi_applicable | BOOLEAN | NO | FALSE |
| holiday_work_amount | NUMERIC(12,2) | YES | — |
| holiday_work_applicable | BOOLEAN | NO | FALSE |
| holiday_work_type | VARCHAR(20) | YES | — |
| hra | NUMERIC(12,2) | NO | 0.00 |
| id | BIGSERIAL | NO | — |
| ifsc_code | VARCHAR(11) | NO | — |
| incentive | NUMERIC(12,2) | NO | 0.00 |
| max_ot_hours_per_month | INT | YES | — |
| other_allowance | NUMERIC(12,2) | NO | 0.00 |
| overtime_applicable | BOOLEAN | NO | FALSE |
| overtime_type | VARCHAR(20) | YES | — |
| per_hour_incentive_pay | NUMERIC(12,2) | YES | — |
| pf_applicable | BOOLEAN | NO | FALSE |
| salary_effective_from | DATE | NO | — |
| salary_effective_to | DATE | YES | — |
| salary_type | VARCHAR(20) | NO | — |
| tds_applicable | BOOLEAN | NO | FALSE |
| updated_at | TIMESTAMPTZ | NO | now() |
| updated_by | VARCHAR(100) | YES | — |
| user_id | BIGINT | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_user_salary_user | user_id | users(id) | ON DELETE CASCADE |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | user_id |

#### Check Constraints

```text
salary_type IN ('CTC', 'FIXED', 'HOURLY')
holiday_work_type IN ('FIXED', 'PER_DAY', 'PER_HOUR')
overtime_type IN ('PER_HOUR', 'PER_DAY')
```

---

### users

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V3__users_table.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| alternate_number | VARCHAR(15) | YES | — |
| contact_number | VARCHAR(15) | NO | — |
| created_at | TIMESTAMPTZ | NO | now() |
| current_address_line1 | VARCHAR(255) | NO | — |
| current_address_line2 | VARCHAR(255) | YES | — |
| current_city | VARCHAR(100) | NO | — |
| current_country | VARCHAR(100) | NO | — |
| current_pincode | VARCHAR(20) | NO | — |
| current_state | VARCHAR(100) | NO | — |
| date_of_joining | DATE | NO | — |
| department | VARCHAR(150) | NO | — |
| designation | VARCHAR(150) | NO | — |
| email | VARCHAR(255) | YES | — |
| emp_id | VARCHAR(50) | NO | — |
| employment_type | VARCHAR(50) | NO | — |
| first_name | VARCHAR(100) | NO | — |
| id | BIGSERIAL | NO | — |
| is_active | BOOLEAN | NO | TRUE |
| last_login_at | TIMESTAMPTZ | YES | — |
| last_name | VARCHAR(100) | NO | — |
| password_hash | VARCHAR(512) | NO | — |
| permanent_address_line1 | VARCHAR(255) | NO | — |
| permanent_address_line2 | VARCHAR(255) | YES | — |
| permanent_city | VARCHAR(100) | NO | — |
| permanent_country | VARCHAR(100) | NO | — |
| permanent_pincode | VARCHAR(20) | NO | — |
| permanent_state | VARCHAR(100) | NO | — |
| reporting_manager_id | BIGINT | YES | — |
| role_id | BIGINT | NO | — |
| status | VARCHAR(20) | NO | 'ACTIVE' |
| updated_at | TIMESTAMPTZ | NO | now() |
| username | VARCHAR(255) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_users_role | role_id | roles(id) | ON DELETE RESTRICT |
| fk_users_reporting_manager | reporting_manager_id | users(id) | ON DELETE SET NULL |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | emp_id |
| — | email |
| — | username |

---

### vendor_product_supplies

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V13__vendor_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| created_at | TIMESTAMPTZ | YES | now() |
| created_by | VARCHAR(100) | YES | — |
| deleted_at | TIMESTAMPTZ | YES | — |
| deleted_by | VARCHAR(100) | YES | — |
| delivery_frequency | VARCHAR(50) | NO | — |
| delivery_lead_time_days | INTEGER | YES | — |
| id | VARCHAR(50) | NO | — |
| minimum_order_quantity | DOUBLE PRECISION | YES | — |
| product_category | VARCHAR(100) | YES | — |
| product_id | VARCHAR(50) | NO | — |
| supply_quantity | DOUBLE PRECISION | NO | — |
| tax_applicable | BOOLEAN | NO | FALSE |
| unit_supply_rate | DOUBLE PRECISION | NO | — |
| uom | VARCHAR(50) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |
| vendor_id | VARCHAR(50) | NO | — |

---

### vendors

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V13__vendor_management_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| account_holder_name | VARCHAR(100) | YES | — |
| account_number | VARCHAR(50) | YES | — |
| address | TEXT | NO | — |
| advance_payment_percentage | DOUBLE PRECISION | YES | — |
| bank_name | VARCHAR(100) | YES | — |
| billing_cycle | VARCHAR(50) | YES | — |
| billing_type | VARCHAR(50) | YES | — |
| city | VARCHAR(100) | NO | — |
| contact_person | VARCHAR(100) | NO | — |
| contract_document_url | TEXT | YES | — |
| contract_end_date | DATE | YES | — |
| contract_start_date | DATE | YES | — |
| contract_type | VARCHAR(50) | YES | — |
| country | VARCHAR(100) | NO | 'India' |
| created_at | TIMESTAMPTZ | YES | now() |
| created_by | VARCHAR(100) | YES | — |
| custom_billing_end_date | DATE | YES | — |
| custom_billing_start_date | DATE | YES | — |
| deleted_at | TIMESTAMPTZ | YES | — |
| deleted_by | VARCHAR(100) | YES | — |
| email_id | VARCHAR(100) | NO | — |
| gst_number | VARCHAR(50) | YES | — |
| has_contract | BOOLEAN | NO | FALSE |
| id | VARCHAR(50) | NO | — |
| ifsc_code | VARCHAR(20) | YES | — |
| invoice_submission_method | VARCHAR(50) | YES | 'Email' |
| late_payment_penalty | TEXT | YES | — |
| pan_number | VARCHAR(50) | YES | — |
| payment_terms | VARCHAR(50) | YES | — |
| phone_number | VARCHAR(20) | NO | — |
| pincode | VARCHAR(20) | NO | — |
| product_supplied | VARCHAR(255) | NO | — |
| remarks | TEXT | YES | — |
| sla_agreement | BOOLEAN | YES | — |
| state | VARCHAR(100) | NO | — |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |
| vendor_category | VARCHAR(100) | NO | — |
| vendor_document_name | VARCHAR(255) | YES | — |
| vendor_document_type | VARCHAR(100) | YES | — |
| vendor_document_url | TEXT | YES | — |
| vendor_name | VARCHAR(100) | NO | — |
| vendor_rating | INTEGER | YES | — |
| vendor_registration_type | VARCHAR(50) | NO | — |
| vendor_status | VARCHAR(50) | NO | 'ACTIVE' |
| vendor_type | VARCHAR(50) | NO | — |

---

### voucher_allocations

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V38__payments_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| allocated_amount | NUMERIC(14,2) | NO | 0 |
| created_at | TIMESTAMPTZ | NO | now() |
| document_id | VARCHAR(50) | NO | — |
| document_type | VARCHAR(20) | NO | — |
| id | VARCHAR(50) | NO | — |
| pending_before | NUMERIC(14,2) | NO | 0 |
| settlement_action | VARCHAR(20) | YES | — |
| shortfall_amount | NUMERIC(14,2) | NO | 0 |
| status_after | VARCHAR(20) | YES | — |
| voucher_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_va_voucher | voucher_id | vouchers(id) | ON DELETE CASCADE |

#### Check Constraints

```text
document_type IN ('INVOICE','BILL')
settlement_action IN ('KEEP_OPEN','SETTLE_CLOSE')
status_after IN ('PARTIAL','PAID')
```

---

### voucher_audit_logs

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V38__payments_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| action | VARCHAR(80) | NO | — |
| id | VARCHAR(50) | NO | — |
| performed_at | TIMESTAMPTZ | NO | now() |
| performed_by | VARCHAR(150) | YES | — |
| remarks | VARCHAR(500) | YES | — |
| voucher_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_val_voucher | voucher_id | vouchers(id) | ON DELETE CASCADE |

---

### voucher_journal_lines

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V38__payments_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| cr_amount | NUMERIC(14,2) | NO | 0 |
| dr_amount | NUMERIC(14,2) | NO | 0 |
| id | VARCHAR(50) | NO | — |
| ledger_id | VARCHAR(50) | NO | — |
| line_narration | VARCHAR(500) | YES | — |
| line_no | INTEGER | NO | — |
| voucher_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_vjl_voucher | voucher_id | vouchers(id) | ON DELETE CASCADE |

#### Check Constraints

```text
chk_vjl_one_side: (CASE WHEN dr_amount > 0 THEN 1 ELSE 0 END) + (CASE WHEN cr_amount > 0 THEN 1 ELSE 0 END) = 1
```

---

### voucher_settlement_links

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V38__payments_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| id | VARCHAR(50) | NO | — |
| settlement_amount | NUMERIC(14,2) | NO | 0 |
| settlement_id | VARCHAR(50) | NO | — |
| settlement_number | VARCHAR(50) | YES | — |
| settlement_type | VARCHAR(20) | NO | — |
| voucher_id | VARCHAR(50) | NO | — |

#### Foreign Keys

| Name | Columns | References | Actions |
|---|---|---|---|
| fk_vsl_voucher | voucher_id | vouchers(id) | ON DELETE CASCADE |

#### Check Constraints

```text
settlement_type IN ('CREDIT_NOTE','DEBIT_NOTE')
```

---

### vouchers

- Sources: seravion-connect-backend/src/main/resources/db/migration/tenant/V38__payments_module.sql
- Primary key: id

#### Columns

| Column | Type | Nullable | Default |
|---|---|---:|---|
| advance_applied | NUMERIC(14,2) | NO | 0 |
| allocated_amount | NUMERIC(14,2) | NO | 0 |
| bank_ledger_id | VARCHAR(50) | YES | — |
| branch_id | VARCHAR(30) | NO | — |
| cheque_date | DATE | YES | — |
| created_at | TIMESTAMPTZ | NO | now() |
| created_by | VARCHAR(100) | YES | — |
| from_ledger_id | VARCHAR(50) | YES | — |
| gross_amount | NUMERIC(14,2) | NO | — |
| id | VARCHAR(50) | NO | — |
| notes | TEXT | YES | — |
| party_id | VARCHAR(50) | YES | — |
| party_type | VARCHAR(20) | NO | — |
| payment_mode | VARCHAR(20) | NO | — |
| reference_no | VARCHAR(120) | YES | — |
| status | VARCHAR(20) | NO | 'POSTED' CHECK (status IN ('POSTED' |
| tds_amount | NUMERIC(14,2) | NO | 0 |
| to_ledger_id | VARCHAR(50) | YES | — |
| unallocated_amount | NUMERIC(14,2) | NO | 0 |
| updated_at | TIMESTAMPTZ | YES | — |
| updated_by | VARCHAR(100) | YES | — |
| voucher_date | DATE | NO | — |
| voucher_number | VARCHAR(50) | NO | — |
| voucher_type | VARCHAR(20) | NO | — |

#### Unique Constraints

| Name | Columns |
|---|---|
| — | voucher_number |

#### Check Constraints

```text
voucher_type IN ('RECEIPT','PAYMENT','CONTRA','JOURNAL')
party_type IN ('CUSTOMER','VENDOR','NONE')
payment_mode IN ('CASH','BANK','UPI','CHEQUE','CARD','ADJUSTMENT')
gross_amount > 0
status IN ('POSTED','VOID')
```

---

