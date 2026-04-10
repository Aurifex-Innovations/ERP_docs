# Tenant database schema (consolidated)
This document reflects the **final** tenant schema after Flyway migrations **V1** through **V28** under `src/main/resources/db/migration/tenant/`.
Historical `ALTER TABLE` statements are **folded into** the `CREATE TABLE` definitions below (no separate alter list).
Indexes, sequences, views, and seed `INSERT`s remain in the migration files; this document focuses on **table** definitions.

Regenerate from sources: `python3 scripts/generate_tenant_schema_consolidated.py` (writes this file).

**Note on `tasks` ↔ `support_tickets`:** there is a circular FK (`tasks.ticket_id` → `support_tickets`, `support_tickets.related_task_id` → `tasks`). Migrations create tables first, then attach `fk_tasks_support_ticket`. For a greenfield script run, create both tables without those two FKs, then `ALTER TABLE` to add them, or use `DEFERRABLE` constraints.

## Table of contents

1. [Core RBAC & tenancy](#1--core-rbac--tenancy-v1)
2. [Users & directory](#2--users--directory-v3-v4-v8)
3. [Tax & HSN](#3--tax--hsn-v5)
4. [Branches](#4--branches-v6-v26)
5. [Role compensation & leave config](#5--role-compensation--leave-config-v7)
6. [Hiring](#6--hiring-v9)
7. [Leads & follow-up](#7--leads--follow-up-v10)
8. [Inventory products](#8--inventory-products-v11)
9. [Stock](#9--stock-v12)
10. [Vendors](#10--vendors-v13)
11. [Service management](#11--service-management-v14)
12. [Purchase orders](#12--purchase-orders-v23-supersedes-v15)
13. [Quotations](#13--quotations-v16-v20)
14. [GMA](#14--gma-v17-v19-v20)
15. [Customers](#15--customers-v18)
16. [Contracts](#16--contracts-v19)
17. [Sales orders](#17--sales-orders-v20-v25)
18. [Petty cash](#18--petty-cash-v21)
19. [Task management](#19--task-management-v25-v27)
20. [Customer support](#20--customer-support-v27)
21. [HRM](#21--hrm-v28)

---

## 1 — Core RBAC & tenancy (V1)

### `roles`

```sql
CREATE TABLE IF NOT EXISTS roles (
    id           BIGSERIAL    PRIMARY KEY,
    name         VARCHAR(100) NOT NULL UNIQUE,
    description  VARCHAR(255),
    is_app_user  BOOLEAN      NOT NULL DEFAULT FALSE,
    status       VARCHAR(20)  NOT NULL DEFAULT 'ACTIVE'
                     CHECK (status IN ('INACTIVE', 'ACTIVE')),
    created_by   VARCHAR(20)  DEFAULT 'COMPANY',
    created_at   TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at   TIMESTAMPTZ  NOT NULL DEFAULT now()
);
```

### `modules`

```sql
CREATE TABLE IF NOT EXISTS modules (
    id          BIGSERIAL    PRIMARY KEY,
    name        VARCHAR(100) NOT NULL UNIQUE,
    label       VARCHAR(255),
    description VARCHAR(255)
);
```

### `actions`

```sql
CREATE TABLE IF NOT EXISTS actions (
    id          BIGSERIAL    PRIMARY KEY,
    name        VARCHAR(100) NOT NULL UNIQUE,   -- READ, WRITE, DELETE, EXECUTE …
    label       VARCHAR(255),
    description VARCHAR(255)
);
```

### `role_permissions`

```sql
CREATE TABLE IF NOT EXISTS role_permissions (
    id                BIGSERIAL   PRIMARY KEY,

    role_id           BIGINT      NOT NULL,
    module_id         BIGINT      NOT NULL,
    action_id         BIGINT      NOT NULL,

    allowed           BOOLEAN     NOT NULL DEFAULT TRUE,
    receiver_role_ids BIGINT[]    DEFAULT NULL,

    created_at        TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at        TIMESTAMPTZ NOT NULL DEFAULT now(),

    CONSTRAINT fk_rp_role
        FOREIGN KEY (role_id)   REFERENCES roles(id)   ON DELETE CASCADE,
    CONSTRAINT fk_rp_module
        FOREIGN KEY (module_id) REFERENCES modules(id) ON DELETE CASCADE,
    CONSTRAINT fk_rp_action
        FOREIGN KEY (action_id) REFERENCES actions(id) ON DELETE CASCADE,

    CONSTRAINT uk_rp_role_module_action
        UNIQUE (role_id, module_id, action_id)
);
```


## 2 — Users & directory (V3, V4, V8)

### `users`

```sql
CREATE TABLE IF NOT EXISTS users (
    id                      BIGSERIAL PRIMARY KEY,

    -- Employee Identity
    emp_id                   VARCHAR(50) NOT NULL UNIQUE,
    first_name               VARCHAR(100) NOT NULL,
    last_name                VARCHAR(100) NOT NULL,

    -- Authentication
    email                    VARCHAR(255) UNIQUE,
    username                 VARCHAR(255) NOT NULL UNIQUE,
    password_hash            VARCHAR(512) NOT NULL,

    is_application_user      BOOLEAN NOT NULL DEFAULT FALSE,

    -- Contact
    contact_number           VARCHAR(15) NOT NULL,
    alternate_number         VARCHAR(15),

    -- Organization
    department               VARCHAR(150) NOT NULL,
    designation              VARCHAR(150) NOT NULL,
    role_id                  BIGINT NOT NULL,

    -- Branch / Hierarchy
    reporting_manager_id     BIGINT,

    -- Employment
    employment_type          VARCHAR(50) NOT NULL,
    date_of_joining          DATE NOT NULL,

    -- Status
    status                   VARCHAR(20) NOT NULL DEFAULT 'ACTIVE',
    is_active                BOOLEAN NOT NULL DEFAULT TRUE,

    -- Current Address
    current_address_line1    VARCHAR(255) NOT NULL,
    current_address_line2    VARCHAR(255),
    current_city             VARCHAR(100) NOT NULL,
    current_state            VARCHAR(100) NOT NULL,
    current_country          VARCHAR(100) NOT NULL,
    current_pincode          VARCHAR(20) NOT NULL,

    -- Permanent Address
    permanent_address_line1  VARCHAR(255) NOT NULL,
    permanent_address_line2  VARCHAR(255),
    permanent_city           VARCHAR(100) NOT NULL,
    permanent_state          VARCHAR(100) NOT NULL,
    permanent_country        VARCHAR(100) NOT NULL,
    permanent_pincode        VARCHAR(20) NOT NULL,

    -- Audit
    created_by               VARCHAR(100),
    updated_by               VARCHAR(100),

    last_login_at            TIMESTAMPTZ,
    created_at               TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at               TIMESTAMPTZ NOT NULL DEFAULT now(),

    -- Foreign Keys
    CONSTRAINT fk_users_role
        FOREIGN KEY (role_id)
        REFERENCES roles(id)
        ON DELETE RESTRICT,

    CONSTRAINT fk_users_reporting_manager
        FOREIGN KEY (reporting_manager_id)
        REFERENCES users(id)
        ON DELETE SET NULL
);
```

### `user_permissions`

```sql
CREATE TABLE IF NOT EXISTS user_permissions (
    id          BIGSERIAL PRIMARY KEY,

    user_id     BIGINT NOT NULL,
    module_id   BIGINT NOT NULL,
    action_id   BIGINT NOT NULL,

    allowed     BOOLEAN NOT NULL,

    receiver_role_ids BIGINT[]    DEFAULT NULL,

    created_at  TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at  TIMESTAMPTZ NOT NULL DEFAULT now(),

    CONSTRAINT fk_up_user
        FOREIGN KEY (user_id) REFERENCES users(id)
        ON DELETE CASCADE,

    CONSTRAINT fk_up_module
        FOREIGN KEY (module_id) REFERENCES modules(id)
        ON DELETE CASCADE,

    CONSTRAINT fk_up_action
        FOREIGN KEY (action_id) REFERENCES actions(id)
        ON DELETE CASCADE,

    CONSTRAINT uk_up_user_module_action
        UNIQUE (user_id, module_id, action_id)
);
```

### `user_branches`

```sql
CREATE TABLE IF NOT EXISTS user_branches (
    user_id   BIGINT       NOT NULL,
    branch_id VARCHAR(30)  NOT NULL,

    CONSTRAINT pk_user_branches
        PRIMARY KEY (user_id, branch_id),

    CONSTRAINT fk_ub_user
        FOREIGN KEY (user_id)
        REFERENCES users(id)
        ON DELETE CASCADE
);
```

### `user_salary_details`

```sql
CREATE TABLE IF NOT EXISTS user_salary_details (
    id                       BIGSERIAL PRIMARY KEY,
    user_id                  BIGINT        NOT NULL UNIQUE,

    -- Salary
    salary_type              VARCHAR(20)   NOT NULL
                             CHECK (salary_type IN ('CTC', 'FIXED', 'HOURLY')),
    basic_salary             NUMERIC(12,2) NOT NULL,
    hra                      NUMERIC(12,2) NOT NULL DEFAULT 0.00,
    other_allowance          NUMERIC(12,2) NOT NULL DEFAULT 0.00,
    incentive                NUMERIC(12,2) NOT NULL DEFAULT 0.00,
    deductions               NUMERIC(12,2) NOT NULL DEFAULT 0.00,

    -- Statutory
    pf_applicable            BOOLEAN       NOT NULL DEFAULT FALSE,
    esi_applicable           BOOLEAN       NOT NULL DEFAULT FALSE,
    tds_applicable           BOOLEAN       NOT NULL DEFAULT FALSE,

    -- Banking
    bank_name                VARCHAR(150)  NOT NULL,
    account_number           VARCHAR(20)   NOT NULL,
    ifsc_code                VARCHAR(11)   NOT NULL,

    -- Effective dates
    salary_effective_from    DATE          NOT NULL,
    salary_effective_to      DATE,

    -- Incentive / Overtime
    holiday_work_applicable  BOOLEAN       NOT NULL DEFAULT FALSE,
    holiday_work_type        VARCHAR(20)   CHECK (holiday_work_type IN ('FIXED', 'PER_DAY', 'PER_HOUR')),
    holiday_work_amount      NUMERIC(12,2),
    overtime_applicable      BOOLEAN       NOT NULL DEFAULT FALSE,
    overtime_type            VARCHAR(20)   CHECK (overtime_type IN ('PER_HOUR', 'PER_DAY')),
    per_hour_incentive_pay   NUMERIC(12,2),
    max_ot_hours_per_month   INT,

    overtime_shift_type      VARCHAR(50)
        CHECK (overtime_shift_type IS NULL OR overtime_shift_type IN ('NIGHT', 'NORMAL', 'CUSTOM')),
    custom_shift_from        TIME,
    custom_shift_to          TIME,
    overtime_shift_incentive NUMERIC(12, 2),

    -- Audit
    created_by               VARCHAR(100),
    created_at               TIMESTAMPTZ   NOT NULL DEFAULT now(),
    updated_by               VARCHAR(100),
    updated_at               TIMESTAMPTZ   NOT NULL DEFAULT now(),

    CONSTRAINT fk_user_salary_user
        FOREIGN KEY (user_id)
        REFERENCES users(id)
        ON DELETE CASCADE
);
```

### `user_leave_details`

```sql
CREATE TABLE IF NOT EXISTS user_leave_details (
    id                       BIGSERIAL PRIMARY KEY,
    user_id                  BIGINT      NOT NULL UNIQUE,

    casual_leave             INT         NOT NULL DEFAULT 0,
    sick_leave               INT         NOT NULL DEFAULT 0,
    paid_leave               INT         NOT NULL DEFAULT 0,
    annual_leave_allocation  INT         NOT NULL DEFAULT 0,
    carry_forward_allowed    BOOLEAN     NOT NULL DEFAULT FALSE,
    max_carry_forward_days   INT,

    leave_approval_role_id   BIGINT      ,
    leave_reset_cycle        VARCHAR(20) NOT NULL
                             CHECK (leave_reset_cycle IN ('YEARLY', 'MONTHLY', 'CUSTOM')),

    leave_reset_from         DATE,
    leave_reset_to           DATE,

    -- Audit
    created_by               VARCHAR(100),
    created_at               TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_by               VARCHAR(100),
    updated_at               TIMESTAMPTZ NOT NULL DEFAULT now(),

    CONSTRAINT fk_user_leave_user
        FOREIGN KEY (user_id)
        REFERENCES users(id)
        ON DELETE CASCADE,

    CONSTRAINT fk_user_leave_approval_role
        FOREIGN KEY (leave_approval_role_id)
        REFERENCES roles(id)
        ON DELETE SET NULL
);
```

### `user_documents`

```sql
CREATE TABLE IF NOT EXISTS user_documents (
    id                   BIGSERIAL    PRIMARY KEY,
    user_id              BIGINT       NOT NULL,

    document_type        VARCHAR(60)  NOT NULL,   -- AADHAR | PAN | ADDRESS_PROOF | EDUCATION | …
    file_path            VARCHAR(500) NOT NULL,
    original_file_name   VARCHAR(255),
    file_size_bytes      BIGINT,
    mime_type            VARCHAR(100),
    uploaded_by          VARCHAR(100),
    uploaded_at          TIMESTAMPTZ  NOT NULL DEFAULT now(),

    CONSTRAINT fk_user_doc_user
        FOREIGN KEY (user_id)
        REFERENCES users(id)
        ON DELETE CASCADE
);
```

### `user_additional_data`

```sql
CREATE TABLE IF NOT EXISTS user_additional_data (
    id                    BIGSERIAL     PRIMARY KEY,
    user_id               BIGINT        NOT NULL UNIQUE,

    -- Identity
    aadhar_number         VARCHAR(12),
    pan_number            VARCHAR(10),
    uan_number            VARCHAR(12),
    id_card_number        VARCHAR(50),

    -- Professional
    grade_level           VARCHAR(50),
    shift_type            VARCHAR(50),
    weekly_off            VARCHAR(50),
    target_amount         NUMERIC(14,2),
    commission_percentage NUMERIC(5,2),
    photo_url             VARCHAR(500),

    -- Audit
    created_by            VARCHAR(100),
    created_at            TIMESTAMPTZ   NOT NULL DEFAULT now(),
    updated_by            VARCHAR(100),
    updated_at            TIMESTAMPTZ   NOT NULL DEFAULT now(),

    CONSTRAINT fk_user_additional_user
        FOREIGN KEY (user_id)
        REFERENCES users(id)
        ON DELETE CASCADE
);
```


## 3 — Tax & HSN (V5)

### `tax_types`

```sql
CREATE TABLE IF NOT EXISTS tax_types (

    -- Primary Key
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    -- Business Identifier
    tax_type_code       VARCHAR(30) NOT NULL UNIQUE,  -- e.g. AD-56096

    -- Core Fields
    tax_name            VARCHAR(100) NOT NULL,
    tax_category        VARCHAR(20) NOT NULL,
    default_rate        NUMERIC(5,2) NOT NULL CHECK (default_rate >= 0 AND default_rate <= 100),
    applicability       VARCHAR(20) NOT NULL,
    description         VARCHAR(500),
    effective_from      DATE NOT NULL,
    status              VARCHAR(20) NOT NULL DEFAULT 'ACTIVE',

    -- Change Tracking
    change_reason       VARCHAR(255),

    -- Audit Fields
    created_by          VARCHAR(50) NOT NULL,
    created_at          TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_by          VARCHAR(50),
    updated_at          TIMESTAMPTZ,
    deleted_by          VARCHAR(50),
    deleted_at          TIMESTAMPTZ,

    -- ============================
    -- CHECK Constraints
    -- ============================

    CONSTRAINT chk_tax_category
        CHECK (tax_category IN ('CENTRAL', 'STATE', 'INTEGRATED', 'CESS')),

    CONSTRAINT chk_applicability
        CHECK (applicability IN ('GOODS', 'SERVICES', 'BOTH')),

    CONSTRAINT chk_status
        CHECK (status IN ('ACTIVE', 'INACTIVE')),

    CONSTRAINT uq_tax_name UNIQUE (tax_name)
);
```

### `hsn_codes`

```sql
CREATE TABLE IF NOT EXISTS hsn_codes (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    hsn_code VARCHAR(8) NOT NULL ,
    description TEXT NOT NULL,
    chapter VARCHAR(20),
    product_category VARCHAR(20) NOT NULL,
    product_subcategory VARCHAR(20),
    effective_from DATE NOT NULL,
    status VARCHAR(20) NOT NULL DEFAULT 'ACTIVE',

    created_by VARCHAR(50) NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_by VARCHAR(50),
    updated_at TIMESTAMPTZ,
    deleted_by VARCHAR(50),
    deleted_at TIMESTAMPTZ,

    CONSTRAINT uq_hsn_code UNIQUE (hsn_code),

    CONSTRAINT chk_hsn_code_format
        CHECK (hsn_code ~ '^[0-9]{4}$|^[0-9]{6}$|^[0-9]{8}$'),

    CONSTRAINT chk_hsn_status
        CHECK (status IN ('ACTIVE', 'INACTIVE')),

    CONSTRAINT chk_product_category
            CHECK (product_category IN ('ASSET', 'CONSUMABLES', 'RESALE')),

    CONSTRAINT chk_product_subcategory
            CHECK (product_subcategory IN ('CHEMICALS', 'MACHINE', 'SPRAYER', 'POWDER'))
);
```

### `hsn_code_tax_types`

```sql
CREATE TABLE IF NOT EXISTS hsn_code_tax_types (
    hsn_code_id BIGINT NOT NULL,
    tax_type_id BIGINT NOT NULL,

    CONSTRAINT fk_hsn_code
        FOREIGN KEY (hsn_code_id) REFERENCES hsn_codes(id),

    CONSTRAINT fk_tax_type
        FOREIGN KEY (tax_type_id) REFERENCES tax_types(id),

    CONSTRAINT uq_hsn_tax UNIQUE (hsn_code_id, tax_type_id)
);
```


## 4 — Branches (V6, V26)

### `branches`

```sql
CREATE TABLE IF NOT EXISTS branches (
    id VARCHAR(30) PRIMARY KEY,

    branch_name VARCHAR(100) NOT NULL,
    branch_code VARCHAR(3) NOT NULL,

    email VARCHAR(100) NOT NULL UNIQUE,
    phone_number VARCHAR(10) NOT NULL,

    address_line1 TEXT NOT NULL,
    country VARCHAR(50) NOT NULL,
    state VARCHAR(50) NOT NULL,
    city VARCHAR(50) NOT NULL,
    pincode VARCHAR(10) NOT NULL,

    branch_type VARCHAR(20) NOT NULL,
    status VARCHAR(20) NOT NULL,

    created_by VARCHAR(30),
    updated_by VARCHAR(30),
    deleted_by VARCHAR(30),

    created_at TIMESTAMPTZ DEFAULT now(),
    updated_at TIMESTAMPTZ,
    deleted_at TIMESTAMPTZ,

    CONSTRAINT uq_branch_code UNIQUE (branch_code),

    CONSTRAINT chk_branch_type CHECK (
        branch_type IN ('HEAD_OFFICE', 'STATE_BRANCH', 'CITY_BRANCH', 'WAREHOUSE')
    ),

    CONSTRAINT chk_branch_status CHECK (
        status IN ('ACTIVE','INACTIVE')
    )
);
```


## 5 — Role compensation & leave config (V7)

### `role_compensation_configuration`

```sql
CREATE TABLE IF NOT EXISTS role_compensation_configuration (
    config_id          VARCHAR(50) PRIMARY KEY,
    role_id            BIGINT      NOT NULL,
    effective_from     DATE        NOT NULL,
    effective_to       DATE,
    status             VARCHAR(20) NOT NULL DEFAULT 'ACTIVE' 
                       CHECK (status IN ('INACTIVE', 'ACTIVE')),
    created_by         VARCHAR(20),
    created_at         TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_by         VARCHAR(20),
    updated_at         TIMESTAMPTZ NOT NULL DEFAULT now(),
    
    CONSTRAINT fk_rcc_role FOREIGN KEY (role_id) REFERENCES roles(id) ON DELETE CASCADE
);
```

### `salary_details`

```sql
CREATE TABLE IF NOT EXISTS salary_details (
    config_id          VARCHAR(50) PRIMARY KEY,
    salary_type        VARCHAR(20) NOT NULL 
                       CHECK (salary_type IN ('CTC', 'FIXED', 'HOURLY')),
    basic_salary       NUMERIC(12,2) NOT NULL,
    hra                NUMERIC(12,2) DEFAULT 0.00,
    other_allowance    NUMERIC(12,2) DEFAULT 0.00,
    incentive          NUMERIC(12,2) DEFAULT 0.00,
    deductions         NUMERIC(12,2) DEFAULT 0.00,
    pf_applicable      BOOLEAN DEFAULT FALSE,
    esi_applicable     BOOLEAN DEFAULT FALSE,
    tds_applicable     BOOLEAN DEFAULT FALSE,
    salary_effective_from DATE,
    salary_effective_to   DATE,
    
    CONSTRAINT fk_sd_config FOREIGN KEY (config_id) REFERENCES role_compensation_configuration(config_id) ON DELETE CASCADE
);
```

### `incentive_overtime_details`

```sql
CREATE TABLE IF NOT EXISTS incentive_overtime_details (
    config_id                VARCHAR(50) PRIMARY KEY,
    holiday_work_applicable  BOOLEAN DEFAULT FALSE,
    holiday_work_type        VARCHAR(20) 
                             CHECK (holiday_work_type IN ('FIXED', 'PER_DAY', 'PER_HOUR')),
    holiday_work_amount      NUMERIC(12,2),
    overtime_applicable      BOOLEAN DEFAULT FALSE,
    overtime_type            VARCHAR(20) 
                             CHECK (overtime_type IN ('PER_HOUR', 'PER_DAY')),
    overtime_shift_type      VARCHAR(50) CHECK (overtime_shift_type IN ('NIGHT', 'NORMAL', 'CUSTOM')),
    custom_shift_from        TIME,
    custom_shift_to          TIME,
    overtime_shift_incentive NUMERIC(12,2),
    per_hour_incentive_pay   NUMERIC(12,2),
    max_ot_hours_per_month   INT,
    
    CONSTRAINT fk_iod_config FOREIGN KEY (config_id) REFERENCES role_compensation_configuration(config_id) ON DELETE CASCADE
);
```

### `leave_configuration`

```sql
CREATE TABLE IF NOT EXISTS leave_configuration (
    config_id                VARCHAR(50) PRIMARY KEY,
    casual_leave             INT DEFAULT 0,
    sick_leave               INT DEFAULT 0,
    paid_leave               INT DEFAULT 0,
    annual_leave_allocation  INT DEFAULT 0,
    carry_forward_allowed    BOOLEAN DEFAULT FALSE,
    max_carry_forward_days   INT,
    leave_approval_role_id   BIGINT NOT NULL,
    leave_reset_cycle        VARCHAR(20) NOT NULL 
                             CHECK (leave_reset_cycle IN ('YEARLY', 'MONTHLY', 'CUSTOM')),
    leave_reset_from         DATE,
    leave_reset_to           DATE,
    
    CONSTRAINT fk_lc_config FOREIGN KEY (config_id) REFERENCES role_compensation_configuration(config_id) ON DELETE CASCADE,
    CONSTRAINT fk_lc_role   FOREIGN KEY (leave_approval_role_id) REFERENCES roles(id) ON DELETE RESTRICT
);
```


## 6 — Hiring (V9)

### `hiring_requests`

```sql
CREATE TABLE IF NOT EXISTS hiring_requests (
    id                        VARCHAR(30)  PRIMARY KEY,

    -- Submitter
    requested_by_user_id      BIGINT       NOT NULL,

    -- Details
    department                VARCHAR(150) NOT NULL,
    designation               VARCHAR(150) NOT NULL,
    proposed_role_id          BIGINT       NOT NULL,
    employment_type           VARCHAR(20)  NOT NULL
                              CHECK (employment_type IN ('PERMANENT', 'CONTRACT', 'INTERN')),
    expected_date_of_joining  DATE         NOT NULL,
    number_of_positions       INT          NOT NULL
                              CHECK (number_of_positions BETWEEN 1 AND 100),

    hiring_reason             TEXT         NOT NULL,
    job_description           TEXT,
    additional_remarks        VARCHAR(500),
    proposed_salary           DECIMAL(15,2),
    supporting_document_path  VARCHAR(500),

    -- Status
    status                    VARCHAR(20)  NOT NULL DEFAULT 'PENDING'
                              CHECK (status IN ('PENDING', 'APPROVED', 'REJECTED', 'CONVERTED')),

    -- Review
    reviewed_by_user_id       BIGINT,
    review_date               DATE,
    rejection_reason          TEXT,

    -- Conversion link
    converted_user_id         BIGINT,

    -- Audit
    submitted_at              TIMESTAMPTZ,
    created_by                VARCHAR(100),
    created_at                TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_by                VARCHAR(100),
    updated_at                TIMESTAMPTZ  NOT NULL DEFAULT now(),

    CONSTRAINT fk_hiring_requested_by
        FOREIGN KEY (requested_by_user_id)
        REFERENCES users(id)
        ON DELETE RESTRICT,

    CONSTRAINT fk_hiring_proposed_role
        FOREIGN KEY (proposed_role_id)
        REFERENCES roles(id)
        ON DELETE RESTRICT,

    CONSTRAINT fk_hiring_reviewed_by
        FOREIGN KEY (reviewed_by_user_id)
        REFERENCES users(id)
        ON DELETE SET NULL,

    CONSTRAINT fk_hiring_converted_user
        FOREIGN KEY (converted_user_id)
        REFERENCES users(id)
        ON DELETE SET NULL
);
```

### `hiring_request_branches`

```sql
CREATE TABLE IF NOT EXISTS hiring_request_branches (
    hiring_request_id  VARCHAR(30) NOT NULL,
    branch_id          VARCHAR(30) NOT NULL,

    CONSTRAINT pk_hiring_request_branches
        PRIMARY KEY (hiring_request_id, branch_id),

    CONSTRAINT fk_hrb_hiring_request
        FOREIGN KEY (hiring_request_id)
        REFERENCES hiring_requests(id)
        ON DELETE CASCADE
);
```

### `hiring_request_recipients`

```sql
CREATE TABLE IF NOT EXISTS hiring_request_recipients (
    hiring_request_id  VARCHAR(30) NOT NULL,
    recipient_user_id  BIGINT      NOT NULL,

    CONSTRAINT pk_hiring_request_recipients
        PRIMARY KEY (hiring_request_id, recipient_user_id),

    CONSTRAINT fk_hrr_hiring_request
        FOREIGN KEY (hiring_request_id)
        REFERENCES hiring_requests(id)
        ON DELETE CASCADE,

    CONSTRAINT fk_hrr_user
        FOREIGN KEY (recipient_user_id)
        REFERENCES users(id)
        ON DELETE CASCADE
);
```


## 7 — Leads & follow-up (V10)

### `leads`

```sql
CREATE TABLE IF NOT EXISTS leads (
    id VARCHAR(50) PRIMARY KEY,
    lead_date DATE NOT NULL,
    source VARCHAR(50) NOT NULL,
    branch_id VARCHAR(30) NOT NULL,
    priority VARCHAR(20) NOT NULL,
    assigned_to_id BIGINT,
    lead_name VARCHAR(200) NOT NULL,
    mobile_number VARCHAR(15) NOT NULL,
    alternate_number VARCHAR(15),
    email_id VARCHAR(255),
    lead_type VARCHAR(20) NOT NULL,
    service_type VARCHAR(50),
    budget_range VARCHAR(50),
    lead_description TEXT NOT NULL,
    status VARCHAR(20) NOT NULL,
    gma_status VARCHAR(20) NOT NULL,
    lost_reason TEXT,
    next_follow_up_date DATE,
    created_by VARCHAR(100),
    updated_by VARCHAR(100),
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP NOT NULL,
    CONSTRAINT uk_leads_mobile UNIQUE (mobile_number),
    CONSTRAINT uk_leads_email UNIQUE (email_id)
);
```

### `follow_ups`

```sql
CREATE TABLE IF NOT EXISTS follow_ups (
    id VARCHAR(50) PRIMARY KEY,
    lead_id VARCHAR(50) NOT NULL,
    interaction_summary TEXT NOT NULL,
    status_updated_to VARCHAR(20) NOT NULL,
    contact_mode VARCHAR(30) NOT NULL,
    lost_reason TEXT,
    next_action_scheduled BOOLEAN DEFAULT FALSE,
    next_follow_up_date DATE,
    next_follow_up_time TIME,
    reason_agenda TEXT,
    created_by VARCHAR(100),
    created_at TIMESTAMP NOT NULL,
    CONSTRAINT fk_follow_ups_lead FOREIGN KEY (lead_id) REFERENCES leads(id)
);
```

### `lead_audit_logs`

```sql
CREATE TABLE IF NOT EXISTS lead_audit_logs (
    id VARCHAR(50) PRIMARY KEY,
    lead_id VARCHAR(50) NOT NULL,
    field_changed VARCHAR(100) NOT NULL,
    old_value TEXT,
    new_value TEXT,
    changed_by VARCHAR(100),
    changed_at TIMESTAMP NOT NULL,
    CONSTRAINT fk_audit_lead FOREIGN KEY (lead_id) REFERENCES leads(id)
);
```


## 8 — Inventory products (V11)

### `inventory_products`

```sql
CREATE TABLE IF NOT EXISTS inventory_products (
    id VARCHAR(50) PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    product_code VARCHAR(100) UNIQUE,
    category VARCHAR(100),
    sub_type VARCHAR(100),
    brand VARCHAR(100),
    description TEXT,
    status VARCHAR(100) DEFAULT 'ACTIVE',

    hsn_code VARCHAR(100),
    base_uom VARCHAR(100),

    unit_packaging_brand VARCHAR(100),
    secondary_uom VARCHAR(100),
    package_type VARCHAR(100),
    quantity_per_package DOUBLE PRECISION,
    units_per_package DOUBLE PRECISION,

    variant_name VARCHAR(100) NOT NULL,
    variant_sku VARCHAR(100) UNIQUE,
    variant_package_type VARCHAR(100),
    variant_quantity DOUBLE PRECISION,
    barcode VARCHAR(100),
    variant_status VARCHAR(100) DEFAULT 'ACTIVE',

    purchase_price DOUBLE PRECISION,
    selling_price DOUBLE PRECISION,
    base_price DOUBLE PRECISION,
    tax_amount DOUBLE PRECISION,
    total_cost DOUBLE PRECISION,

    created_by VARCHAR(100),
    updated_by VARCHAR(100),
    deleted_by VARCHAR(100),

    created_at TIMESTAMPTZ DEFAULT now(),
    updated_at TIMESTAMPTZ,
    deleted_at TIMESTAMPTZ
);
```

### `inventory_product_media`

```sql
CREATE TABLE IF NOT EXISTS inventory_product_media (
    id VARCHAR(50) PRIMARY KEY,
    product_id VARCHAR(100),

    file_name VARCHAR(100),
    content_type VARCHAR(100),
    file_url TEXT,
    file_data TEXT,
    is_primary BOOLEAN DEFAULT FALSE,

    created_by VARCHAR(255),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```


## 9 — Stock (V12)

### `stock_ledger`

```sql
CREATE TABLE IF NOT EXISTS stock_ledger (
    id BIGSERIAL PRIMARY KEY,
    branch_id VARCHAR(30) NOT NULL,
    product_id VARCHAR(50) NOT NULL,
    product_code VARCHAR(50) NOT NULL,
    product_name VARCHAR(255) NOT NULL,
    category VARCHAR(50),
    brand VARCHAR(120),
    hsn_code VARCHAR(20),
    base_uom VARCHAR(30),
    assets_qty INTEGER NOT NULL DEFAULT 0 CHECK (assets_qty >= 0),
    consumable_qty INTEGER NOT NULL DEFAULT 0 CHECK (consumable_qty >= 0),
    resell_qty INTEGER NOT NULL DEFAULT 0 CHECK (resell_qty >= 0),
    in_transit_qty INTEGER NOT NULL DEFAULT 0 CHECK (in_transit_qty >= 0),
    reserved_qty INTEGER NOT NULL DEFAULT 0 CHECK (reserved_qty >= 0),
    status VARCHAR(30) NOT NULL DEFAULT 'AVAILABLE',
    created_by VARCHAR(80),
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_by VARCHAR(80),
    updated_at TIMESTAMPTZ,
    deleted_by VARCHAR(80),
    deleted_at TIMESTAMPTZ,
    CONSTRAINT uq_stock_ledger_branch_product UNIQUE (branch_id, product_id),
    CONSTRAINT fk_stock_ledger_product FOREIGN KEY (product_id) REFERENCES inventory_products(id) ON DELETE RESTRICT
);
```

### `central_stock_entries`

```sql
CREATE TABLE IF NOT EXISTS central_stock_entries (
    id BIGSERIAL PRIMARY KEY,
    entry_id VARCHAR(40) NOT NULL UNIQUE,
    product_id VARCHAR(50) NOT NULL,
    product_code VARCHAR(50) NOT NULL,
    product_name VARCHAR(255) NOT NULL,
    hsn_code VARCHAR(20),
    base_uom VARCHAR(30),
    total_qty INTEGER NOT NULL CHECK (total_qty > 0),
    assets_qty INTEGER NOT NULL DEFAULT 0 CHECK (assets_qty >= 0),
    consumable_qty INTEGER NOT NULL DEFAULT 0 CHECK (consumable_qty >= 0),
    resell_qty INTEGER NOT NULL DEFAULT 0 CHECK (resell_qty >= 0),
    asset_id_generation VARCHAR(15),
    asset_id_prefix VARCHAR(30),
    asset_sequence_start INTEGER,
    assignment_type VARCHAR(30),
    default_assignment VARCHAR(50),
    supplier_name VARCHAR(200),
    purchase_order_ref VARCHAR(80),
    invoice_number VARCHAR(80) UNIQUE,
    invoice_date DATE,
    invoice_amount NUMERIC(14,2),
    tax_amount NUMERIC(14,2),
    total_with_tax NUMERIC(14,2),
    invoice_copy_url TEXT,
    batch_number VARCHAR(80),
    manufacturing_date DATE,
    expiry_date DATE,
    status VARCHAR(30) NOT NULL DEFAULT 'ACTIVE',
    created_by VARCHAR(80),
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_by VARCHAR(80),
    updated_at TIMESTAMPTZ,
    deleted_by VARCHAR(80),
    deleted_at TIMESTAMPTZ,
    CONSTRAINT chk_central_stock_entry_qty_split CHECK (assets_qty + consumable_qty + resell_qty = total_qty),
    CONSTRAINT fk_central_stock_entries_product FOREIGN KEY (product_id) REFERENCES inventory_products(id) ON DELETE RESTRICT
);
```

### `asset_units`

```sql
CREATE TABLE IF NOT EXISTS asset_units (
    id BIGSERIAL PRIMARY KEY,
    asset_id VARCHAR(60) NOT NULL UNIQUE,
    product_id VARCHAR(50) NOT NULL,
    product_code VARCHAR(50) NOT NULL,
    product_name VARCHAR(255) NOT NULL,
    branch_id VARCHAR(30) NOT NULL,
    assigned_user_id BIGINT,
    assigned_to_name VARCHAR(160),
    assignment_mode VARCHAR(30),
    condition VARCHAR(30) NOT NULL DEFAULT 'GOOD',
    status VARCHAR(30) NOT NULL DEFAULT 'AVAILABLE',
    created_by VARCHAR(80),
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_by VARCHAR(80),
    updated_at TIMESTAMPTZ,
    CONSTRAINT chk_asset_condition CHECK (condition IN ('NEW', 'GOOD', 'FAIR', 'DAMAGED', 'NEEDS_REPAIR')),
    CONSTRAINT chk_asset_status CHECK (status IN ('AVAILABLE', 'ISSUED', 'IN_TRANSIT', 'MAINTENANCE', 'RETIRED', 'QUARANTINE')),
    CONSTRAINT fk_asset_units_product FOREIGN KEY (product_id) REFERENCES inventory_products(id) ON DELETE RESTRICT
);
```

### `stock_requests`

```sql
CREATE TABLE IF NOT EXISTS stock_requests (
    id BIGSERIAL PRIMARY KEY,
    request_id VARCHAR(40) NOT NULL UNIQUE,
    request_type VARCHAR(30) NOT NULL,
    direction VARCHAR(15) NOT NULL,
    from_branch_id VARCHAR(30) NOT NULL,
    to_branch_id VARCHAR(30) NOT NULL,
    requested_by_user_id BIGINT,
    requested_by_name VARCHAR(160),
    priority VARCHAR(15) NOT NULL DEFAULT 'NORMAL',
    required_by_date DATE NOT NULL,
    purpose TEXT NOT NULL,
    notes_for_approver TEXT,
    sent_to TEXT,
    status VARCHAR(40) NOT NULL DEFAULT 'DRAFT',
    approval_type VARCHAR(30),
    alternative_source VARCHAR(30),
    dispatch_date DATE,
    expected_delivery_date DATE,
    carrier VARCHAR(120),
    lr_number VARCHAR(80),
    remarks TEXT,
    issue_type VARCHAR(40),
    issue_description TEXT,
    issue_resolution_status VARCHAR(40),
    created_by VARCHAR(80),
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_by VARCHAR(80),
    updated_at TIMESTAMPTZ,
    deleted_by VARCHAR(80),
    deleted_at TIMESTAMPTZ
);
```

### `stock_request_items`

```sql
CREATE TABLE IF NOT EXISTS stock_request_items (
    id BIGSERIAL PRIMARY KEY,
    request_id BIGINT NOT NULL,
    product_id VARCHAR(50) NOT NULL,
    product_code VARCHAR(50) NOT NULL,
    product_name VARCHAR(255) NOT NULL,
    base_uom VARCHAR(30),
    assets_req_qty INTEGER NOT NULL DEFAULT 0 CHECK (assets_req_qty >= 0),
    consumable_req_qty INTEGER NOT NULL DEFAULT 0 CHECK (consumable_req_qty >= 0),
    resell_req_qty INTEGER NOT NULL DEFAULT 0 CHECK (resell_req_qty >= 0),
    assets_appr_qty INTEGER NOT NULL DEFAULT 0 CHECK (assets_appr_qty >= 0),
    consumable_appr_qty INTEGER NOT NULL DEFAULT 0 CHECK (consumable_appr_qty >= 0),
    resell_appr_qty INTEGER NOT NULL DEFAULT 0 CHECK (resell_appr_qty >= 0),
    estimated_cost NUMERIC(14,2),
    tax_amount NUMERIC(14,2),
    item_purpose VARCHAR(250),
    alternative_source VARCHAR(30),
    CONSTRAINT fk_stock_request_item_request FOREIGN KEY (request_id) REFERENCES stock_requests(id) ON DELETE CASCADE,
    CONSTRAINT fk_stock_request_item_product FOREIGN KEY (product_id) REFERENCES inventory_products(id) ON DELETE RESTRICT
);
```

### `stock_transfers`

```sql
CREATE TABLE IF NOT EXISTS stock_transfers (
    id BIGSERIAL PRIMARY KEY,
    transfer_id VARCHAR(40) NOT NULL UNIQUE,
    reference_request_id VARCHAR(40),
    from_branch_id VARCHAR(30) NOT NULL,
    to_branch_id VARCHAR(30) NOT NULL,
    transfer_type VARCHAR(20) NOT NULL,
    strategy VARCHAR(30),
    status VARCHAR(40) NOT NULL DEFAULT 'DRAFT',
    dispatch_date DATE,
    expected_delivery_date DATE,
    carrier VARCHAR(120),
    lr_number VARCHAR(80),
    remarks TEXT,
    created_by VARCHAR(80),
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_by VARCHAR(80),
    updated_at TIMESTAMPTZ
);
```

### `stock_transfer_items`

```sql
CREATE TABLE IF NOT EXISTS stock_transfer_items (
    id BIGSERIAL PRIMARY KEY,
    transfer_id BIGINT NOT NULL,
    product_id VARCHAR(50) NOT NULL,
    product_code VARCHAR(50) NOT NULL,
    product_name VARCHAR(255) NOT NULL,
    assets_qty INTEGER NOT NULL DEFAULT 0 CHECK (assets_qty >= 0),
    consumable_qty INTEGER NOT NULL DEFAULT 0 CHECK (consumable_qty >= 0),
    resell_qty INTEGER NOT NULL DEFAULT 0 CHECK (resell_qty >= 0),
    source_branch_id VARCHAR(30),
    CONSTRAINT fk_stock_transfer_item_transfer FOREIGN KEY (transfer_id) REFERENCES stock_transfers(id) ON DELETE CASCADE,
    CONSTRAINT fk_stock_transfer_item_product FOREIGN KEY (product_id) REFERENCES inventory_products(id) ON DELETE RESTRICT
);
```

### `stock_transfer_assets`

```sql
CREATE TABLE IF NOT EXISTS stock_transfer_assets (
    id BIGSERIAL PRIMARY KEY,
    transfer_id BIGINT NOT NULL,
    asset_id VARCHAR(60) NOT NULL,
    condition_at_dispatch VARCHAR(30),
    transfer_with VARCHAR(40),
    destination_user_id BIGINT,
    destination_user_name VARCHAR(160),
    condition_at_receipt VARCHAR(30),
    receipt_status VARCHAR(30),
    CONSTRAINT fk_stock_transfer_asset_transfer FOREIGN KEY (transfer_id) REFERENCES stock_transfers(id) ON DELETE CASCADE
);
```

### `stock_movement_logs`

```sql
CREATE TABLE IF NOT EXISTS stock_movement_logs (
    id BIGSERIAL PRIMARY KEY,
    reference_type VARCHAR(30) NOT NULL,
    reference_id VARCHAR(40) NOT NULL,
    branch_id VARCHAR(30),
    product_id VARCHAR(50),
    stock_type VARCHAR(20),
    quantity_delta INTEGER NOT NULL DEFAULT 0,
    action VARCHAR(50) NOT NULL,
    remarks TEXT,
    created_by VARCHAR(80),
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);
```

### `stock_approval_logs`

```sql
CREATE TABLE IF NOT EXISTS stock_approval_logs (
    id BIGSERIAL PRIMARY KEY,
    request_id BIGINT NOT NULL,
    action VARCHAR(40) NOT NULL,
    previous_status VARCHAR(40),
    new_status VARCHAR(40),
    remarks TEXT,
    created_by VARCHAR(80),
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    CONSTRAINT fk_stock_approval_log_request FOREIGN KEY (request_id) REFERENCES stock_requests(id) ON DELETE CASCADE
);
```


## 10 — Vendors (V13)

### `vendors`

```sql
CREATE TABLE IF NOT EXISTS vendors (
    id VARCHAR(50) PRIMARY KEY,
    vendor_name VARCHAR(100) NOT NULL,
    vendor_type VARCHAR(50) NOT NULL, -- Supplier / Service Provider / Both
    vendor_category VARCHAR(100) NOT NULL, 
    product_supplied VARCHAR(255) NOT NULL,
    contact_person VARCHAR(100) NOT NULL,
    phone_number VARCHAR(20) NOT NULL,
    email_id VARCHAR(100) NOT NULL,
    vendor_status VARCHAR(50) NOT NULL DEFAULT 'ACTIVE', -- Active / Inactive / Blocked
    has_contract BOOLEAN NOT NULL DEFAULT FALSE,

    -- Address Details
    address TEXT NOT NULL,
    city VARCHAR(100) NOT NULL,
    state VARCHAR(100) NOT NULL,
    pincode VARCHAR(20) NOT NULL,
    country VARCHAR(100) NOT NULL DEFAULT 'India',

    -- Tax & Compliance
    vendor_registration_type VARCHAR(50) NOT NULL, -- Registered / Unregistered
    gst_number VARCHAR(50),
    pan_number VARCHAR(50),

    -- Bank Details
    bank_name VARCHAR(100),
    account_holder_name VARCHAR(100),
    account_number VARCHAR(50),
    ifsc_code VARCHAR(20),

    -- Contract Details (if has_contract is TRUE)
    contract_type VARCHAR(50), -- Annual / Project / One Time
    contract_start_date DATE,
    contract_end_date DATE,
    sla_agreement BOOLEAN,
    contract_document_url TEXT,

    -- Billing Configuration
    billing_type VARCHAR(50), -- Per Service / Monthly / Project
    billing_cycle VARCHAR(50), -- Weekly / Monthly / Quarterly / Custom
    custom_billing_start_date DATE,
    custom_billing_end_date DATE,
    invoice_submission_method VARCHAR(50) DEFAULT 'Email', -- Email / Portal / Physical

    -- Payment Terms
    payment_terms VARCHAR(50), -- Advance / Net15 / Net30 / Net45
    advance_payment_percentage DOUBLE PRECISION,
    late_payment_penalty TEXT,

    -- Performance Tracking
    vendor_rating INTEGER,
    remarks TEXT,

    vendor_document_url TEXT,
    vendor_document_name VARCHAR(255),
    vendor_document_type VARCHAR(100),

    -- Audit Columns
    created_by VARCHAR(100),
    updated_by VARCHAR(100),
    deleted_by VARCHAR(100),
    created_at TIMESTAMPTZ DEFAULT now(),
    updated_at TIMESTAMPTZ,
    deleted_at TIMESTAMPTZ
);
```

### `vendor_product_supplies`

```sql
CREATE TABLE IF NOT EXISTS vendor_product_supplies (
    id VARCHAR(50) PRIMARY KEY,
    vendor_id VARCHAR(50) NOT NULL REFERENCES vendors(id) ON DELETE CASCADE,
    product_id VARCHAR(50) NOT NULL,
    product_category VARCHAR(100),
    supply_quantity DOUBLE PRECISION NOT NULL,
    uom VARCHAR(50) NOT NULL,
    unit_supply_rate DOUBLE PRECISION NOT NULL,
    minimum_order_quantity DOUBLE PRECISION,
    delivery_frequency VARCHAR(50) NOT NULL, -- Weekly / Monthly / Quarterly / Custom
    delivery_lead_time_days INTEGER,
    tax_applicable BOOLEAN NOT NULL DEFAULT FALSE,

    -- Audit Columns
    created_by VARCHAR(100),
    updated_by VARCHAR(100),
    deleted_by VARCHAR(100),
    created_at TIMESTAMPTZ DEFAULT now(),
    updated_at TIMESTAMPTZ,
    deleted_at TIMESTAMPTZ
);
```


## 11 — Service management (V14)

### `service_categories`

```sql
CREATE TABLE IF NOT EXISTS service_categories (
    id              VARCHAR(50) PRIMARY KEY,
    name            VARCHAR(150) NOT NULL,
    is_active       BOOLEAN      NOT NULL DEFAULT TRUE,
    display_order   INTEGER,
    created_by      VARCHAR(100),
    updated_by      VARCHAR(100),
    created_at      TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at      TIMESTAMPTZ,
    CONSTRAINT uk_service_categories_name UNIQUE (name)
);
```

### `service_sub_categories`

```sql
CREATE TABLE IF NOT EXISTS service_sub_categories (
    id              VARCHAR(50) PRIMARY KEY,
    code            VARCHAR(50)  NOT NULL,
    name            VARCHAR(100) NOT NULL,
    is_active       BOOLEAN      NOT NULL DEFAULT TRUE,
    display_order   INTEGER,
    created_at      TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at      TIMESTAMPTZ,
    CONSTRAINT uk_service_sub_categories_code UNIQUE (code)
);
```

### `service_pest_types`

```sql
CREATE TABLE IF NOT EXISTS service_pest_types (
    id              VARCHAR(50) PRIMARY KEY,
    name            VARCHAR(150) NOT NULL,
    is_active       BOOLEAN      NOT NULL DEFAULT TRUE,
    display_order   INTEGER,
    created_by      VARCHAR(100),
    updated_by      VARCHAR(100),
    created_at      TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at      TIMESTAMPTZ,
    CONSTRAINT uk_service_pest_types_name UNIQUE (name)
);
```

### `service_treatments`

```sql
CREATE TABLE IF NOT EXISTS service_treatments (
    id              VARCHAR(50) PRIMARY KEY,
    name            VARCHAR(200) NOT NULL,
    is_active       BOOLEAN      NOT NULL DEFAULT TRUE,
    display_order   INTEGER,
    created_by      VARCHAR(100),
    updated_by      VARCHAR(100),
    created_at      TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at      TIMESTAMPTZ,
    CONSTRAINT uk_service_treatments_name UNIQUE (name)
);
```

### `service_category_fixed`

```sql
CREATE TABLE IF NOT EXISTS service_category_fixed (
    id                      VARCHAR(50) PRIMARY KEY,
    service_category_id     VARCHAR(50) NOT NULL,
    service_sub_category_id VARCHAR(50),
    tier_name               VARCHAR(150) NOT NULL,
    price_amount            DOUBLE PRECISION NOT NULL,
    display_order           INTEGER,
    is_active               BOOLEAN      NOT NULL DEFAULT TRUE,
    created_by              VARCHAR(100),
    updated_by              VARCHAR(100),
    created_at              TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at              TIMESTAMPTZ,
    CONSTRAINT fk_scf_category
        FOREIGN KEY (service_category_id) REFERENCES service_categories(id) ON DELETE CASCADE,
    CONSTRAINT fk_scf_sub_category
        FOREIGN KEY (service_sub_category_id) REFERENCES service_sub_categories(id) ON DELETE SET NULL
);
```

### `service_category_area`

```sql
CREATE TABLE IF NOT EXISTS service_category_area (
    id                      VARCHAR(50) PRIMARY KEY,
    service_category_id     VARCHAR(50) NOT NULL,
    service_sub_category_id VARCHAR(50),
    base_price              DOUBLE PRECISION NOT NULL,
    price_per_sqft          DOUBLE PRECISION NOT NULL,
    sqft_increment          DOUBLE PRECISION NOT NULL DEFAULT 100,
    is_active               BOOLEAN      NOT NULL DEFAULT TRUE,
    created_by              VARCHAR(100),
    updated_by              VARCHAR(100),
    created_at              TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at              TIMESTAMPTZ,
    CONSTRAINT fk_sca_category
        FOREIGN KEY (service_category_id) REFERENCES service_categories(id) ON DELETE CASCADE,
    CONSTRAINT fk_sca_sub_category
        FOREIGN KEY (service_sub_category_id) REFERENCES service_sub_categories(id) ON DELETE SET NULL
);
```

### `service_category_inspection`

```sql
CREATE TABLE IF NOT EXISTS service_category_inspection (
    id                  VARCHAR(50) PRIMARY KEY,
    service_category_id VARCHAR(50) NOT NULL,
    inspection_fee      DOUBLE PRECISION NOT NULL,
    is_active           BOOLEAN      NOT NULL DEFAULT TRUE,
    created_by          VARCHAR(100),
    updated_by          VARCHAR(100),
    created_at          TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at          TIMESTAMPTZ,
    CONSTRAINT fk_sci_category
        FOREIGN KEY (service_category_id) REFERENCES service_categories(id) ON DELETE CASCADE
);
```

### `service_custom_pricing_blocks`

```sql
CREATE TABLE IF NOT EXISTS service_custom_pricing_blocks (
    id                      VARCHAR(50) PRIMARY KEY,
    service_category_id     VARCHAR(50) NOT NULL,
    service_sub_category_id VARCHAR(50) NOT NULL,
    label                   VARCHAR(200),
    is_active               BOOLEAN      NOT NULL DEFAULT TRUE,
    created_by              VARCHAR(100),
    updated_by              VARCHAR(100),
    created_at              TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at              TIMESTAMPTZ,
    CONSTRAINT fk_scpb_category
        FOREIGN KEY (service_category_id) REFERENCES service_categories(id) ON DELETE CASCADE,
    CONSTRAINT fk_scpb_sub_category
        FOREIGN KEY (service_sub_category_id) REFERENCES service_sub_categories(id) ON DELETE CASCADE
);
```

### `service_custom_pricing_fields`

```sql
CREATE TABLE IF NOT EXISTS service_custom_pricing_fields (
    id              VARCHAR(50) PRIMARY KEY,
    block_id        VARCHAR(50) NOT NULL,
    field_name      VARCHAR(200) NOT NULL,
    price_amount    DOUBLE PRECISION NOT NULL,
    display_order   INTEGER,
    created_at      TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at      TIMESTAMPTZ,
    CONSTRAINT fk_scpf_block
        FOREIGN KEY (block_id) REFERENCES service_custom_pricing_blocks(id) ON DELETE CASCADE
);
```

### `services`

```sql
CREATE TABLE IF NOT EXISTS services (
    id                      VARCHAR(50) PRIMARY KEY,
    service_code            VARCHAR(50)  NOT NULL,
    service_name            VARCHAR(200) NOT NULL,
    description             TEXT         NOT NULL,

    price_type              VARCHAR(50)  NOT NULL
        CHECK (price_type IN ('FIXED', 'AREA_BASED', 'INSPECTION')),
    duration_value          DOUBLE PRECISION NOT NULL,
    duration_uom              VARCHAR(20)  NOT NULL
        CHECK (duration_uom IN ('MINUTES', 'HOURS')),

    status                  VARCHAR(20)  NOT NULL DEFAULT 'ACTIVE'
        CHECK (status IN ('ACTIVE', 'INACTIVE')),
    is_draft                BOOLEAN      NOT NULL DEFAULT FALSE,

    visits_per_month        DOUBLE PRECISION,

    warranty_months         INTEGER,
    free_revisit_included   BOOLEAN      NOT NULL DEFAULT FALSE,
    free_revisit_quantity   INTEGER,

    inactive_reason         TEXT,
    inactive_at             TIMESTAMPTZ,
    inactivated_by          VARCHAR(100),

    display_order           INTEGER,

    created_by              VARCHAR(100),
    updated_by              VARCHAR(100),
    created_at              TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at              TIMESTAMPTZ,

    CONSTRAINT uk_services_code UNIQUE (service_code),
    CONSTRAINT uk_services_name UNIQUE (service_name)
);
```

### `services_service_categories`

```sql
CREATE TABLE IF NOT EXISTS services_service_categories (
    service_id          VARCHAR(50) NOT NULL,
    service_category_id VARCHAR(50) NOT NULL,
    PRIMARY KEY (service_id, service_category_id),
    CONSTRAINT fk_ssc_service
        FOREIGN KEY (service_id) REFERENCES services(id) ON DELETE CASCADE,
    CONSTRAINT fk_ssc_category
        FOREIGN KEY (service_category_id) REFERENCES service_categories(id) ON DELETE CASCADE
);
```

### `services_service_sub_categories`

```sql
CREATE TABLE IF NOT EXISTS services_service_sub_categories (
    service_id              VARCHAR(50) NOT NULL,
    service_sub_category_id VARCHAR(50) NOT NULL,
    PRIMARY KEY (service_id, service_sub_category_id),
    CONSTRAINT fk_sssc_service
        FOREIGN KEY (service_id) REFERENCES services(id) ON DELETE CASCADE,
    CONSTRAINT fk_sssc_sub
        FOREIGN KEY (service_sub_category_id) REFERENCES service_sub_categories(id) ON DELETE CASCADE
);
```

### `services_service_pest_types`

```sql
CREATE TABLE IF NOT EXISTS services_service_pest_types (
    service_id          VARCHAR(50) NOT NULL,
    service_pest_type_id VARCHAR(50) NOT NULL,
    PRIMARY KEY (service_id, service_pest_type_id),
    CONSTRAINT fk_sspt_service
        FOREIGN KEY (service_id) REFERENCES services(id) ON DELETE CASCADE,
    CONSTRAINT fk_sspt_pest
        FOREIGN KEY (service_pest_type_id) REFERENCES service_pest_types(id) ON DELETE CASCADE
);
```

### `services_service_treatments`

```sql
CREATE TABLE IF NOT EXISTS services_service_treatments (
    service_id           VARCHAR(50) NOT NULL,
    service_treatment_id VARCHAR(50) NOT NULL,
    PRIMARY KEY (service_id, service_treatment_id),
    CONSTRAINT fk_sst_service
        FOREIGN KEY (service_id) REFERENCES services(id) ON DELETE CASCADE,
    CONSTRAINT fk_sst_treatment
        FOREIGN KEY (service_treatment_id) REFERENCES service_treatments(id) ON DELETE CASCADE
);
```

### `services_service_category_fixed`

```sql
CREATE TABLE IF NOT EXISTS services_service_category_fixed (
    service_id                VARCHAR(50) NOT NULL,
    service_category_fixed_id VARCHAR(50) NOT NULL,
    PRIMARY KEY (service_id, service_category_fixed_id),
    CONSTRAINT fk_sscf_service
        FOREIGN KEY (service_id) REFERENCES services(id) ON DELETE CASCADE,
    CONSTRAINT fk_sscf_fixed
        FOREIGN KEY (service_category_fixed_id) REFERENCES service_category_fixed(id) ON DELETE CASCADE
);
```

### `services_service_category_area`

```sql
CREATE TABLE IF NOT EXISTS services_service_category_area (
    service_id               VARCHAR(50) NOT NULL,
    service_category_area_id VARCHAR(50) NOT NULL,
    PRIMARY KEY (service_id, service_category_area_id),
    CONSTRAINT fk_ssca_service
        FOREIGN KEY (service_id) REFERENCES services(id) ON DELETE CASCADE,
    CONSTRAINT fk_ssca_area
        FOREIGN KEY (service_category_area_id) REFERENCES service_category_area(id) ON DELETE CASCADE
);
```

### `services_service_category_inspection`

```sql
CREATE TABLE IF NOT EXISTS services_service_category_inspection (
    service_id                    VARCHAR(50) NOT NULL,
    service_category_inspection_id VARCHAR(50) NOT NULL,
    PRIMARY KEY (service_id, service_category_inspection_id),
    CONSTRAINT fk_ssci_service
        FOREIGN KEY (service_id) REFERENCES services(id) ON DELETE CASCADE,
    CONSTRAINT fk_ssci_inspection
        FOREIGN KEY (service_category_inspection_id) REFERENCES service_category_inspection(id) ON DELETE CASCADE
);
```

### `services_service_custom_pricing_blocks`

```sql
CREATE TABLE IF NOT EXISTS services_service_custom_pricing_blocks (
    service_id                  VARCHAR(50) NOT NULL,
    service_custom_pricing_block_id VARCHAR(50) NOT NULL,
    PRIMARY KEY (service_id, service_custom_pricing_block_id),
    CONSTRAINT fk_sscpb_service
        FOREIGN KEY (service_id) REFERENCES services(id) ON DELETE CASCADE,
    CONSTRAINT fk_sscpb_block
        FOREIGN KEY (service_custom_pricing_block_id) REFERENCES service_custom_pricing_blocks(id) ON DELETE CASCADE
);
```

### `service_species`

```sql
CREATE TABLE IF NOT EXISTS service_species (
    id                  VARCHAR(50) PRIMARY KEY,
    service_id          VARCHAR(50) NOT NULL,
    species_name        VARCHAR(200),
    scientific_name     VARCHAR(300),
    display_order       INTEGER,
    created_at          TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at          TIMESTAMPTZ,
    CONSTRAINT fk_service_species_service
        FOREIGN KEY (service_id) REFERENCES services(id) ON DELETE CASCADE
);
```

### `service_products`

```sql
CREATE TABLE IF NOT EXISTS service_products (
    id                  VARCHAR(50) PRIMARY KEY,
    service_id          VARCHAR(50) NOT NULL,
    inventory_product_id VARCHAR(50) NOT NULL,
    dilution            VARCHAR(100),
    coverage_sqft       DOUBLE PRECISION NOT NULL,
    required_qty        DOUBLE PRECISION NOT NULL,
    price_per_uom       DOUBLE PRECISION NOT NULL,
    cost_per_visit      DOUBLE PRECISION,
    est_cost_per_month  DOUBLE PRECISION,
    is_manual_entry     BOOLEAN      NOT NULL DEFAULT FALSE,
    display_order       INTEGER,
    created_at          TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at          TIMESTAMPTZ,
    CONSTRAINT fk_sp_service
        FOREIGN KEY (service_id) REFERENCES services(id) ON DELETE CASCADE,
    CONSTRAINT fk_sp_product
        FOREIGN KEY (inventory_product_id) REFERENCES inventory_products(id) ON DELETE RESTRICT
);
```

### `service_audit_logs`

```sql
CREATE TABLE IF NOT EXISTS service_audit_logs (
    id              VARCHAR(50) PRIMARY KEY,
    service_id      VARCHAR(50) NOT NULL,
    change_type     VARCHAR(50) NOT NULL
        CHECK (change_type IN ('CREATE', 'UPDATE', 'DEACTIVATE')),
    notes           TEXT,
    changed_by      VARCHAR(100),
    created_at      TIMESTAMPTZ NOT NULL DEFAULT now(),
    CONSTRAINT fk_sal_service
        FOREIGN KEY (service_id) REFERENCES services(id) ON DELETE CASCADE
);
```


## 12 — Purchase orders (V23; supersedes V15)

### `purchase_order`

```sql
CREATE TABLE IF NOT EXISTS purchase_order (
    id VARCHAR(50) PRIMARY KEY,
    po_number VARCHAR(50) UNIQUE NOT NULL,
    po_date DATE NOT NULL,
    status VARCHAR(30) NOT NULL,

    supplier_gst_number VARCHAR(15) NOT NULL,

    vendor_id VARCHAR(50) NOT NULL,
    branch_id VARCHAR(30) NOT NULL,

    delivery_date DATE,
    items_count INT,

    subtotal NUMERIC(15,2),
    total_tax NUMERIC(15,2),
    grand_total NUMERIC(15,2),

    delivery_address TEXT NOT NULL,
    contact_person VARCHAR(100) NOT NULL,
    contact_number VARCHAR(15) NOT NULL,

    authorized_person VARCHAR(100),
    designation VARCHAR(100),
    note TEXT,

    is_deleted BOOLEAN DEFAULT FALSE,

    created_by VARCHAR(100),
    updated_by VARCHAR(100),
    deleted_by VARCHAR(100),
    created_at TIMESTAMPTZ DEFAULT now(),
    updated_at TIMESTAMPTZ,
    deleted_at TIMESTAMPTZ,

    CONSTRAINT fk_po_vendor FOREIGN KEY (vendor_id) REFERENCES vendors (id),
    CONSTRAINT fk_po_branch FOREIGN KEY (branch_id) REFERENCES branches (id)
);
```

### `purchase_order_item`

```sql
CREATE TABLE IF NOT EXISTS purchase_order_item (
    id VARCHAR(50) PRIMARY KEY,
    purchase_order_id VARCHAR(50) NOT NULL,
    line_number INT NOT NULL DEFAULT 1,

    product_id VARCHAR(50) NOT NULL,

    quantity NUMERIC(10,2) NOT NULL,
    uom VARCHAR(100),

    price NUMERIC(15,2),
    gst_percent NUMERIC(5,2),

    tax_amount NUMERIC(15,2),
    total_amount NUMERIC(15,2),

    is_deleted BOOLEAN DEFAULT FALSE,

    created_by VARCHAR(100),
    updated_by VARCHAR(100),
    deleted_by VARCHAR(100),
    created_at TIMESTAMPTZ DEFAULT now(),
    updated_at TIMESTAMPTZ,
    deleted_at TIMESTAMPTZ,

    CONSTRAINT fk_poi_purchase_order FOREIGN KEY (purchase_order_id) REFERENCES purchase_order (id) ON DELETE CASCADE,
    CONSTRAINT fk_poi_product FOREIGN KEY (product_id) REFERENCES inventory_products (id),
    CONSTRAINT uq_poi_po_line UNIQUE (purchase_order_id, line_number)
);
```


## 13 — Quotations (V16, V20)

### `quotation_prospects`

```sql
CREATE TABLE IF NOT EXISTS quotation_prospects (
    id              VARCHAR(50)  PRIMARY KEY,

    full_name       VARCHAR(200) NOT NULL,
    phone           VARCHAR(15)  NOT NULL,
    email           VARCHAR(255),
    company_name    VARCHAR(100),

    address         TEXT         NOT NULL,
    city            VARCHAR(100) NOT NULL,
    state           VARCHAR(100) NOT NULL,
    pincode         VARCHAR(10),
    country         VARCHAR(100) NOT NULL DEFAULT 'India',
    google_map_url  TEXT,

    created_by      VARCHAR(100),
    updated_by      VARCHAR(100),
    created_at      TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at      TIMESTAMPTZ
);
```

### `quotations`

```sql
CREATE TABLE IF NOT EXISTS quotations (
    id                      VARCHAR(50)  PRIMARY KEY,
    quotation_number        VARCHAR(30)  NOT NULL,

    source_type             VARCHAR(20)  NOT NULL
        CHECK (source_type IN ('FROM_LEAD', 'FROM_CUSTOMER', 'NEW_PROSPECT')),

    lead_id                 VARCHAR(50),
    customer_id             VARCHAR(50),
    prospect_id             VARCHAR(50),

    quotation_type          VARCHAR(20)  NOT NULL
        CHECK (quotation_type IN ('SERVICE', 'PRODUCT', 'COMBINED')),

    service_mode            VARCHAR(20)
        CHECK (service_mode IN ('ONE_TIME', 'CONTRACT')),

    contract_frequency      VARCHAR(20)
        CHECK (contract_frequency IN ('MONTHLY', 'QUARTERLY', 'HALF_YEARLY', 'YEARLY')),

    contract_duration       VARCHAR(20)
        CHECK (contract_duration IN ('SIX_MONTHS', 'ONE_YEAR', 'TWO_YEARS', 'THREE_YEARS')),

    contract_proposed_start DATE,

    services_subtotal       NUMERIC(14,2) NOT NULL DEFAULT 0,
    products_subtotal       NUMERIC(14,2) NOT NULL DEFAULT 0,
    subtotal_before_tax     NUMERIC(14,2) NOT NULL DEFAULT 0,
    tax_total               NUMERIC(14,2) NOT NULL DEFAULT 0,
    total_before_discount   NUMERIC(14,2) NOT NULL DEFAULT 0,

    discount_type           VARCHAR(20)
        CHECK (discount_type IN ('PERCENTAGE', 'FLAT_AMOUNT')),
    discount_value          NUMERIC(10,2) DEFAULT 0,
    discount_amount         NUMERIC(14,2) NOT NULL DEFAULT 0,
    grand_total             NUMERIC(14,2) NOT NULL DEFAULT 0,

    valid_till              DATE         NOT NULL,
    payment_terms           VARCHAR(50)  NOT NULL
        CHECK (payment_terms IN (
            'FULL_ADVANCE',
            'FIFTY_ADVANCE_FIFTY_COMPLETION',
            'NET_15',
            'NET_30',
            'CUSTOM'
        )),
    custom_payment_terms    TEXT,
    special_terms           TEXT,

    internal_notes          TEXT,

    status                  VARCHAR(20)  NOT NULL DEFAULT 'DRAFT'
        CHECK (status IN ('DRAFT', 'SENT', 'VIEWED', 'ACCEPTED', 'REJECTED', 'EXPIRED', 'REVISED')),

    sent_at                 TIMESTAMPTZ,
    viewed_at               TIMESTAMPTZ,
    accepted_at             TIMESTAMPTZ,
    rejected_at             TIMESTAMPTZ,
    expired_at              TIMESTAMPTZ,

    is_deleted              BOOLEAN      NOT NULL DEFAULT FALSE,
    deleted_at              TIMESTAMPTZ,
    deleted_by              VARCHAR(100),
    deletion_reason         VARCHAR(100)
        CHECK (deletion_reason IN (
            'CREATED_BY_MISTAKE',
            'DUPLICATE_QUOTATION',
            'CLIENT_WITHDREW_INTEREST',
            'PRICING_ERROR',
            'OTHER'
        )),
    deletion_reason_detail  TEXT,

    revised_from_id         VARCHAR(50),
    contract_id             VARCHAR(50),

    sales_order_consumed    BOOLEAN      NOT NULL DEFAULT FALSE,

    created_by              VARCHAR(100),
    updated_by              VARCHAR(100),
    created_at              TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at              TIMESTAMPTZ,

    CONSTRAINT fk_quot_lead
        FOREIGN KEY (lead_id) REFERENCES leads(id) ON DELETE RESTRICT,

    CONSTRAINT fk_quot_prospect
        FOREIGN KEY (prospect_id) REFERENCES quotation_prospects(id) ON DELETE RESTRICT,

    CONSTRAINT fk_quot_revised_from
        FOREIGN KEY (revised_from_id) REFERENCES quotations(id) ON DELETE SET NULL,

    CONSTRAINT chk_quot_source_xor CHECK (
        (
            (CASE WHEN lead_id IS NOT NULL THEN 1 ELSE 0 END) +
            (CASE WHEN customer_id IS NOT NULL THEN 1 ELSE 0 END) +
            (CASE WHEN prospect_id IS NOT NULL THEN 1 ELSE 0 END)
        ) = 1
    ),

    CONSTRAINT chk_quot_contract_fields CHECK (
        service_mode IS NULL
        OR service_mode = 'ONE_TIME'
        OR (
            service_mode = 'CONTRACT'
            AND contract_frequency IS NOT NULL
            AND contract_duration IS NOT NULL
            AND contract_proposed_start IS NOT NULL
        )
    ),

    CONSTRAINT chk_quot_custom_payment CHECK (
        payment_terms <> 'CUSTOM' OR custom_payment_terms IS NOT NULL
    ),

    CONSTRAINT chk_quot_deletion_detail CHECK (
        deletion_reason IS NULL
        OR deletion_reason <> 'OTHER'
        OR deletion_reason_detail IS NOT NULL
    )
);
```

### `quotation_locations`

```sql
CREATE TABLE IF NOT EXISTS quotation_locations (
    id                      VARCHAR(50)  PRIMARY KEY,
    quotation_id            VARCHAR(50)  NOT NULL,
    display_order           INTEGER      NOT NULL DEFAULT 1,

    address                 TEXT         NOT NULL,
    city                    VARCHAR(100) NOT NULL,
    state                   VARCHAR(100) NOT NULL,
    country                 VARCHAR(100) NOT NULL DEFAULT 'India',
    pincode                 VARCHAR(10),
    google_map_url          TEXT,

    location_category       VARCHAR(30)  NOT NULL
        CHECK (location_category IN ('RESIDENTIAL', 'COMMERCIAL', 'INDUSTRIAL', 'WAREHOUSE')),

    location_sub_category   VARCHAR(20)  NOT NULL
        CHECK (location_sub_category IN ('INTERNAL', 'EXTERNAL')),

    area_sqft               NUMERIC(10,2) NOT NULL CHECK (area_sqft > 0),
    branch_id               VARCHAR(30)   NOT NULL,
    location_service_subtotal NUMERIC(14,2) NOT NULL DEFAULT 0,

    created_at              TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at              TIMESTAMPTZ,

    CONSTRAINT fk_ql_quotation
        FOREIGN KEY (quotation_id) REFERENCES quotations(id) ON DELETE CASCADE,

    CONSTRAINT fk_ql_branch
        FOREIGN KEY (branch_id) REFERENCES branches(id) ON DELETE RESTRICT
);
```

### `quotation_service_lines`

```sql
CREATE TABLE IF NOT EXISTS quotation_service_lines (
    id                      VARCHAR(50)  PRIMARY KEY,
    quotation_id            VARCHAR(50)  NOT NULL,
    quotation_location_id   VARCHAR(50)  NOT NULL,

    service_id              VARCHAR(50)  NOT NULL,
    service_code            VARCHAR(50)  NOT NULL,
    service_name            VARCHAR(200) NOT NULL,

    price_type              VARCHAR(20)  NOT NULL
        CHECK (price_type IN ('FIXED', 'AREA_BASED', 'INSPECTION')),

    fixed_tier_name         VARCHAR(150),
    base_price              NUMERIC(14,2),
    price_per_sqft          NUMERIC(10,4),
    area_sqft_used          NUMERIC(10,2),

    rate_per_visit          NUMERIC(14,2) NOT NULL,

    visit_frequency         VARCHAR(20) NOT NULL
        CHECK (visit_frequency IN ('ONE_TIME', 'MONTHLY', 'QUARTERLY', 'HALF_YEARLY', 'YEARLY')),

    total_visits            INTEGER       NOT NULL DEFAULT 1,
    line_total              NUMERIC(14,2) NOT NULL DEFAULT 0,
    display_order           INTEGER       NOT NULL DEFAULT 1,

    created_at              TIMESTAMPTZ   NOT NULL DEFAULT now(),
    updated_at              TIMESTAMPTZ,

    CONSTRAINT fk_qsl_quotation
        FOREIGN KEY (quotation_id) REFERENCES quotations(id) ON DELETE CASCADE,

    CONSTRAINT fk_qsl_location
        FOREIGN KEY (quotation_location_id) REFERENCES quotation_locations(id) ON DELETE CASCADE,

    CONSTRAINT fk_qsl_service
        FOREIGN KEY (service_id) REFERENCES services(id) ON DELETE RESTRICT
);
```

### `quotation_product_lines`

```sql
CREATE TABLE IF NOT EXISTS quotation_product_lines (
    id                  VARCHAR(50)  PRIMARY KEY,
    quotation_id        VARCHAR(50)  NOT NULL,

    product_id          VARCHAR(50)  NOT NULL,
    product_code        VARCHAR(100) NOT NULL,
    product_name        VARCHAR(200) NOT NULL,
    uom                 VARCHAR(50),
    hsn_code            VARCHAR(20),

    unit_price          NUMERIC(14,2) NOT NULL,
    quantity            NUMERIC(10,3) NOT NULL CHECK (quantity > 0),
    line_subtotal       NUMERIC(14,2) NOT NULL,

    tax_type            VARCHAR(10) NOT NULL DEFAULT 'INTRA'
        CHECK (tax_type IN ('INTRA', 'INTER')),

    cgst_rate           NUMERIC(5,2)  NOT NULL DEFAULT 0,
    sgst_rate           NUMERIC(5,2)  NOT NULL DEFAULT 0,
    igst_rate           NUMERIC(5,2)  NOT NULL DEFAULT 0,
    cgst_amount         NUMERIC(14,2) NOT NULL DEFAULT 0,
    sgst_amount         NUMERIC(14,2) NOT NULL DEFAULT 0,
    igst_amount         NUMERIC(14,2) NOT NULL DEFAULT 0,
    tax_amount          NUMERIC(14,2) NOT NULL DEFAULT 0,

    line_total          NUMERIC(14,2) NOT NULL,
    display_order       INTEGER       NOT NULL DEFAULT 1,

    created_at          TIMESTAMPTZ   NOT NULL DEFAULT now(),
    updated_at          TIMESTAMPTZ,

    CONSTRAINT fk_qpl_quotation
        FOREIGN KEY (quotation_id) REFERENCES quotations(id) ON DELETE CASCADE,

    CONSTRAINT fk_qpl_product
        FOREIGN KEY (product_id) REFERENCES inventory_products(id) ON DELETE RESTRICT
);
```

### `quotation_attachments`

```sql
CREATE TABLE IF NOT EXISTS quotation_attachments (
    id              VARCHAR(50)  PRIMARY KEY,
    quotation_id    VARCHAR(50)  NOT NULL,

    file_key        TEXT         NOT NULL,
    file_name       VARCHAR(255) NOT NULL,
    content_type    VARCHAR(100) NOT NULL,
    file_size_bytes BIGINT,
    notes           TEXT,

    uploaded_by     VARCHAR(100),
    uploaded_at     TIMESTAMPTZ  NOT NULL DEFAULT now(),

    CONSTRAINT fk_qa_quotation
        FOREIGN KEY (quotation_id) REFERENCES quotations(id) ON DELETE CASCADE
);
```

### `quotation_audit_logs`

```sql
CREATE TABLE IF NOT EXISTS quotation_audit_logs (
    id              VARCHAR(50)  PRIMARY KEY,
    quotation_id    VARCHAR(50)  NOT NULL,

    event_type      VARCHAR(50)  NOT NULL
        CHECK (event_type IN (
            'CREATED',
            'UPDATED',
            'SENT',
            'VIEWED',
            'ACCEPTED',
            'REJECTED',
            'EXPIRED',
            'REVISED',
            'DELETED',
            'CONVERTED_TO_CONTRACT',
            'RESENT'
        )),

    field_changed   VARCHAR(100),
    old_value       TEXT,
    new_value       TEXT,
    notes           TEXT,

    changed_by      VARCHAR(100),
    changed_at      TIMESTAMPTZ  NOT NULL DEFAULT now(),

    CONSTRAINT fk_qal_quotation
        FOREIGN KEY (quotation_id) REFERENCES quotations(id) ON DELETE CASCADE
);
```


## 14 — GMA (V17, V19, V20)

### `gma_prospects`

```sql
CREATE TABLE IF NOT EXISTS gma_prospects (
    id              VARCHAR(50)  PRIMARY KEY,

    full_name       VARCHAR(200) NOT NULL,
    phone           VARCHAR(15)  NOT NULL,
    email           VARCHAR(255),
    company_name    VARCHAR(100),

    address         TEXT         NOT NULL,
    city            VARCHAR(100) NOT NULL,
    state           VARCHAR(100) NOT NULL,
    pincode         VARCHAR(10),
    country         VARCHAR(100) NOT NULL DEFAULT 'India',
    google_map_url  TEXT,

    created_by      VARCHAR(100),
    updated_by      VARCHAR(100),
    created_at      TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at      TIMESTAMPTZ
);
```

### `gma_sheets`

```sql
CREATE TABLE IF NOT EXISTS gma_sheets (
    id                      VARCHAR(50)  PRIMARY KEY,

    -- Source selection
    source_type             VARCHAR(20)  NOT NULL
        CHECK (source_type IN ('FROM_LEAD', 'FROM_CUSTOMER', 'ADD_NEW')),

    lead_id                 VARCHAR(50),    -- FK → leads.id (Module 15)
    customer_id             VARCHAR(50),    -- logically FK → customers (Module 4) – plain column
    prospect_id             VARCHAR(50),    -- FK → gma_prospects.id

    -- General config (Section 2)
    contract_duration       VARCHAR(20)
        CHECK (contract_duration IN ('SIX_MONTHS', 'ONE_YEAR', 'TWO_YEARS', 'THREE_YEARS', 'CUSTOM')),
    proposed_start_date     DATE         NOT NULL,
    branch_id               VARCHAR(30)  NOT NULL,  -- FK → branches.id (Module 7)
    remarks                 TEXT,

    -- Overall financial summary (Section 4 – calculated on save)
    total_annual_cost       NUMERIC(14,2) NOT NULL DEFAULT 0,
    total_annual_price      NUMERIC(14,2) NOT NULL DEFAULT 0,
    overall_gross_margin    NUMERIC(5,2)  NOT NULL DEFAULT 0,
    gm_without_doc          NUMERIC(5,2)  NOT NULL DEFAULT 0,
    gm_with_doc             NUMERIC(5,2)  NOT NULL DEFAULT 0,
    total_surcharge_cost    NUMERIC(14,2) NOT NULL DEFAULT 0,
    total_visits_per_month  NUMERIC(10,2) NOT NULL DEFAULT 0,

    contract_consumed         BOOLEAN      NOT NULL DEFAULT FALSE,
    consumed_by_contract_id   VARCHAR(50),
    sales_order_consumed      BOOLEAN      NOT NULL DEFAULT FALSE,

    -- Approval workflow
    status                  VARCHAR(20)  NOT NULL DEFAULT 'DRAFT'
        CHECK (status IN ('DRAFT', 'PENDING', 'APPROVED', 'REJECTED')),

    approver_id             BIGINT,             -- user ID routed to
    approval_remarks        TEXT,

    submitted_on            TIMESTAMPTZ,
    approved_on             TIMESTAMPTZ,
    deadline                TIMESTAMPTZ,        -- 24 h (Sales Mgr) or 48 h (CEO)

    -- Soft delete (Draft only)
    is_deleted              BOOLEAN      NOT NULL DEFAULT FALSE,
    deleted_at              TIMESTAMPTZ,
    deleted_by              VARCHAR(100),

    -- Audit
    prepared_by_id          BIGINT NOT NULL,    -- logged-in user (FK to users)
    created_by              VARCHAR(100),
    updated_by              VARCHAR(100),
    created_at              TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at              TIMESTAMPTZ,

    CONSTRAINT fk_gma_lead
        FOREIGN KEY (lead_id) REFERENCES leads(id) ON DELETE RESTRICT,

    CONSTRAINT fk_gma_prospect
        FOREIGN KEY (prospect_id) REFERENCES gma_prospects(id) ON DELETE RESTRICT,

    CONSTRAINT fk_gma_branch
        FOREIGN KEY (branch_id) REFERENCES branches(id) ON DELETE RESTRICT,

    -- Exactly one source must be set
    CONSTRAINT chk_gma_source_xor CHECK (
        (
            (CASE WHEN lead_id     IS NOT NULL THEN 1 ELSE 0 END) +
            (CASE WHEN customer_id IS NOT NULL THEN 1 ELSE 0 END) +
            (CASE WHEN prospect_id IS NOT NULL THEN 1 ELSE 0 END)
        ) = 1
    )
);
```

### `gma_sheet_approver_roles`

```sql
CREATE TABLE IF NOT EXISTS gma_sheet_approver_roles (
    gma_sheet_id    VARCHAR(50) NOT NULL,
    role_id         BIGINT      NOT NULL,

    PRIMARY KEY (gma_sheet_id, role_id),
    CONSTRAINT fk_gsar_sheet FOREIGN KEY (gma_sheet_id) REFERENCES gma_sheets(id) ON DELETE CASCADE,
    CONSTRAINT fk_gsar_role  FOREIGN KEY (role_id)      REFERENCES roles(id)      ON DELETE CASCADE
);
```

### `gma_sites`

```sql
CREATE TABLE IF NOT EXISTS gma_sites (
    id                              VARCHAR(50)  PRIMARY KEY,
    gma_sheet_id                    VARCHAR(50)  NOT NULL,
    site_name                       VARCHAR(200) NOT NULL,

    -- Location
    address                         TEXT,
    city                            VARCHAR(100) NOT NULL,
    state                           VARCHAR(100) NOT NULL,
    country                         VARCHAR(100) NOT NULL DEFAULT 'India',
    google_map_url                  TEXT,

    -- Site classification (Section 3 – Site Info)
    category                        VARCHAR(30)  NOT NULL
        CHECK (category IN ('RESIDENTIAL', 'COMMERCIAL', 'INDUSTRIAL')),
    sub_category                    VARCHAR(20)  NOT NULL
        CHECK (sub_category IN ('INTERNAL', 'EXTERNAL')),
    area_sqft                       NUMERIC(10,2) NOT NULL CHECK (area_sqft > 0),

    -- 3D: Weekend / Night Surcharge (site-level)
    weekend_night_surcharge_applicable  BOOLEAN      NOT NULL DEFAULT FALSE,
    surcharge_cost                  NUMERIC(14,2) NOT NULL DEFAULT 0,

    -- 3E: Documentation Cost (site-level)
    documentation_cost_applicable   BOOLEAN      NOT NULL DEFAULT FALSE,
    cost_per_document               NUMERIC(10,2) NOT NULL DEFAULT 0,
    docs_per_month                  INTEGER       NOT NULL DEFAULT 0,
    documentation_cost_year         NUMERIC(14,2) NOT NULL DEFAULT 0,

    -- Site financial summary (auto-calculated on save)
    site_total_cost_year            NUMERIC(14,2) NOT NULL DEFAULT 0,
    site_proposed_price_year        NUMERIC(14,2) NOT NULL DEFAULT 0,
    site_gross_margin               NUMERIC(5,2)  NOT NULL DEFAULT 0,

    display_order                   INTEGER       NOT NULL DEFAULT 1,

    created_at                      TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at                      TIMESTAMPTZ,

    CONSTRAINT fk_gmasite_sheet
        FOREIGN KEY (gma_sheet_id) REFERENCES gma_sheets(id) ON DELETE CASCADE
);
```

### `gma_services`

```sql
CREATE TABLE IF NOT EXISTS gma_services (
    id                          VARCHAR(50)  PRIMARY KEY,
    gma_site_id                 VARCHAR(50)  NOT NULL,

    -- Service identity
    service_type_id             VARCHAR(50)  NOT NULL,   -- FK → services.id (Module 12)
    service_type_name           VARCHAR(200) NOT NULL,   -- denormalised for display

    -- Service configuration
    service_mode                VARCHAR(20)  NOT NULL
        CHECK (service_mode IN ('CONTRACT', 'ONE_TIME')),

    frequency                   VARCHAR(20)
        CHECK (frequency IN ('WEEKLY', 'FORTNIGHTLY', 'MONTHLY', 'QUARTERLY', 'CUSTOM')),

    annual_frequency            INTEGER      NOT NULL CHECK (annual_frequency > 0),
    visits_per_month            NUMERIC(5,2) NOT NULL,

    -- 3A: Service Visit Cost
    rate_per_visit              NUMERIC(14,2) NOT NULL DEFAULT 0,
    service_visit_cost_year     NUMERIC(14,2) NOT NULL DEFAULT 0,   -- A = rate × annual_freq
    service_visit_cost_month    NUMERIC(14,2) NOT NULL DEFAULT 0,   -- A ÷ 12

    -- 3B: Manpower / Labor Cost
    hours_per_visit             NUMERIC(5,2)  NOT NULL DEFAULT 0,
    rate_per_hour               NUMERIC(14,2) NOT NULL DEFAULT 0,
    manpower_cost_year          NUMERIC(14,2) NOT NULL DEFAULT 0,   -- B = hours × annual_freq × rate/hr
    manpower_cost_month         NUMERIC(14,2) NOT NULL DEFAULT 0,   -- B ÷ 12

    -- 3C: Chemical Cost (sum of gma_chemicals for this service)
    chemical_cost_year          NUMERIC(14,2) NOT NULL DEFAULT 0,   -- C
    chemical_cost_month         NUMERIC(14,2) NOT NULL DEFAULT 0,

    -- Total for this service block (A + B + C)
    total_service_cost_year     NUMERIC(14,2) NOT NULL DEFAULT 0,

    display_order               INTEGER       NOT NULL DEFAULT 1,

    created_at                  TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at                  TIMESTAMPTZ,

    CONSTRAINT fk_gmasvc_site
        FOREIGN KEY (gma_site_id) REFERENCES gma_sites(id) ON DELETE CASCADE,

    CONSTRAINT fk_gmasvc_type
        FOREIGN KEY (service_type_id) REFERENCES services(id) ON DELETE RESTRICT
);
```

### `gma_chemicals`

```sql
CREATE TABLE IF NOT EXISTS gma_chemicals (
    id                      VARCHAR(50)  PRIMARY KEY,
    gma_service_id          VARCHAR(50)  NOT NULL,

    -- Product details (auto-filled from Module 10 via Module 12 mapping)
    product_id              VARCHAR(50)  NOT NULL,       -- FK → inventory_products.id (Module 10)
    product_code            VARCHAR(100) NOT NULL,
    product_name            VARCHAR(200) NOT NULL,
    uom                     VARCHAR(50)  NOT NULL,       -- ml / Ltr / gm / kg / Nos / tube

    -- Usage inputs (user-entered on form)
    coverage_sqft           NUMERIC(10,2) NOT NULL CHECK (coverage_sqft > 0),
    required_qty_per_visit  NUMERIC(10,3) NOT NULL CHECK (required_qty_per_visit > 0),

    -- Price (auto-filled from Product Master; user may override)
    price_per_uom           NUMERIC(14,4) NOT NULL,     -- 4 decimal places to handle fraction prices

    -- Auto-calculated costs
    cost_per_visit          NUMERIC(14,2) NOT NULL,     -- required_qty_per_visit × price_per_uom
    cost_per_month          NUMERIC(14,2) NOT NULL,     -- cost_per_visit × visits_per_month
    cost_per_year           NUMERIC(14,2) NOT NULL,     -- cost_per_month × 12

    display_order           INTEGER       NOT NULL DEFAULT 1,

    created_at              TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at              TIMESTAMPTZ,

    CONSTRAINT fk_gmachem_service
        FOREIGN KEY (gma_service_id) REFERENCES gma_services(id) ON DELETE CASCADE,

    CONSTRAINT fk_gmachem_product
        FOREIGN KEY (product_id) REFERENCES inventory_products(id) ON DELETE RESTRICT
);
```

### `gma_audit_logs`

```sql
CREATE TABLE IF NOT EXISTS gma_audit_logs (
    id              VARCHAR(50)  PRIMARY KEY,

    -- Plain FK column (survives soft-delete)
    gma_sheet_id    VARCHAR(50)  NOT NULL,

    action          VARCHAR(50)  NOT NULL
        CHECK (action IN (
            'DRAFT_CREATED',
            'SUBMITTED',
            'APPROVED_AUTO',
            'APPROVED_MANUAL',
            'REJECTED',
            'REVOKED'
        )),

    user_id         VARCHAR(50),
    user_name       VARCHAR(100),
    remarks         TEXT,

    action_at       TIMESTAMPTZ  NOT NULL DEFAULT now(),

    CONSTRAINT fk_gmaaud_sheet
        FOREIGN KEY (gma_sheet_id) REFERENCES gma_sheets(id) ON DELETE CASCADE
);
```


## 15 — Customers (V18)

### `customers`

```sql
CREATE TABLE IF NOT EXISTS customers (
    id VARCHAR(50) PRIMARY KEY, -- -- Logical ID (e.g., CUST-AXUYS) (UUID 5 character)
    entry_mode VARCHAR(20) NOT NULL, -- MANUAL, IMPORT_FROM_LEAD
    
    -- Lead Link (Module 15 Integration)
    lead_id VARCHAR(50) REFERENCES leads(id),
    
    -- Entity Information
    customer_type VARCHAR(20) NOT NULL, -- CONTRACT, ONE_TIME, PRODUCT
    full_name VARCHAR(100) NOT NULL,
    industry_type VARCHAR(50),
    pan_number VARCHAR(10),
    gst_number VARCHAR(15) UNIQUE,
    contact_person VARCHAR(100) NOT NULL,
    designation VARCHAR(100),
    phone VARCHAR(15) UNIQUE NOT NULL,
    alternate_phone VARCHAR(15),
    email VARCHAR(100) NOT NULL,
    
    -- Branch Link (Module 7 Integration)
    branch_id VARCHAR(30) NOT NULL REFERENCES branches(id),
    
    -- Billing Details
    billing_address_line1 TEXT NOT NULL,
    billing_address_line2 TEXT,
    city VARCHAR(50) NOT NULL,
    state VARCHAR(50) NOT NULL,
    pincode VARCHAR(6) NOT NULL,
    country VARCHAR(50) DEFAULT 'India',
    google_map_url TEXT,
    
    -- Finance Contact
    finance_contact_name VARCHAR(100) NOT NULL,
    finance_contact_phone VARCHAR(15) NOT NULL,
    finance_contact_email VARCHAR(100),
    
    -- Status and Audit
    status VARCHAR(20) DEFAULT 'DRAFT', -- ACTIVE, INACTIVE, DRAFT
    reason_for_deactivation TEXT,
    is_deleted BOOLEAN DEFAULT FALSE,
    
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    created_by VARCHAR(100),
    updated_by VARCHAR(100)
);
```

### `customer_audit_log`

```sql
CREATE TABLE IF NOT EXISTS customer_audit_log (
    id BIGSERIAL PRIMARY KEY,
    customer_id VARCHAR(50) REFERENCES customers(id),
    field_name VARCHAR(50) NOT NULL,
    old_value TEXT,
    new_value TEXT,
    changed_by VARCHAR(100),
    changed_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
```


## 16 — Contracts (V19)

### `contracts`

```sql
CREATE TABLE IF NOT EXISTS contracts (
    id                          VARCHAR(50)   PRIMARY KEY,

    gma_sheet_id                VARCHAR(50)   NOT NULL,
    customer_id                 VARCHAR(50)   NOT NULL,
    customer_name               VARCHAR(255)  NOT NULL,
    branch_id                   VARCHAR(30)   NOT NULL,

    status                      VARCHAR(30)   NOT NULL
        CHECK (status IN ('DRAFT', 'ACTIVE', 'EXPIRED', 'TERMINATED')),

    duration_option             VARCHAR(30)   NOT NULL
        CHECK (duration_option IN ('SIX_MONTHS', 'ONE_YEAR', 'TWO_YEARS', 'THREE_YEARS', 'CUSTOM')),

    start_date                  DATE          NOT NULL,
    end_date                    DATE          NOT NULL,

    total_sale_value            NUMERIC(14,2) NOT NULL,
    gma_original_total_sale     NUMERIC(14,2) NOT NULL DEFAULT 0,
    total_annual_cost_snapshot  NUMERIC(14,2) NOT NULL DEFAULT 0,
    overall_gm_percent_snapshot NUMERIC(5,2)  NOT NULL DEFAULT 0,

    contract_reference          VARCHAR(50),
    renewal_type                VARCHAR(30)
        CHECK (renewal_type IS NULL OR renewal_type IN ('AUTO_RENEW', 'MANUAL', 'NON_RENEWABLE')),
    legal_notes                 TEXT,

    payment_schedule_type       VARCHAR(40)   NOT NULL,
    invoicing_frequency         VARCHAR(40)   NOT NULL,
    custom_payment_description  TEXT,
    advance_payment_due_date    DATE,
    legal_sla_remarks           TEXT,

    variance_requires_approval  BOOLEAN       NOT NULL DEFAULT FALSE,

    termination_effective_date  DATE,
    termination_reason          VARCHAR(50),
    termination_remarks         VARCHAR(500),

    created_by                  VARCHAR(100),
    updated_by                  VARCHAR(100),
    created_at                  TIMESTAMPTZ   NOT NULL DEFAULT now(),
    updated_at                  TIMESTAMPTZ
);
```

### `contract_sites`

```sql
CREATE TABLE IF NOT EXISTS contract_sites (
    id                      VARCHAR(50)   PRIMARY KEY,
    contract_id             VARCHAR(50)   NOT NULL,
    gma_site_id             VARCHAR(50),

    site_name               VARCHAR(200)  NOT NULL,
    address                 TEXT,
    city                    VARCHAR(100)  NOT NULL,
    state                   VARCHAR(100)  NOT NULL,
    country                 VARCHAR(100)  NOT NULL DEFAULT 'India',
    google_map_url          TEXT,
    area_sqft               NUMERIC(12,2) NOT NULL DEFAULT 0,
    category                VARCHAR(30)   NOT NULL,
    sub_category            VARCHAR(20)   NOT NULL,

    site_total_cost_year    NUMERIC(14,2) NOT NULL DEFAULT 0,
    site_proposed_price_year NUMERIC(14,2) NOT NULL DEFAULT 0,
    site_gross_margin       NUMERIC(5,2)  NOT NULL DEFAULT 0,

    display_order           INTEGER       NOT NULL DEFAULT 1,

    CONSTRAINT fk_cs_contract FOREIGN KEY (contract_id)
        REFERENCES contracts(id) ON DELETE CASCADE
);
```

### `contract_site_services`

```sql
CREATE TABLE IF NOT EXISTS contract_site_services (
    id                      VARCHAR(50)   PRIMARY KEY,
    contract_site_id        VARCHAR(50)   NOT NULL,

    service_type_id         VARCHAR(50)   NOT NULL,
    service_type_name       VARCHAR(200)  NOT NULL,

    contract_mode           VARCHAR(20)   NOT NULL
        CHECK (contract_mode IN ('CONTRACT', 'ONE_TIME')),
    frequency               VARCHAR(20)
        CHECK (frequency IS NULL OR frequency IN ('WEEKLY', 'FORTNIGHTLY', 'MONTHLY', 'QUARTERLY', 'CUSTOM')),
    annual_frequency        INTEGER       NOT NULL DEFAULT 0,

    preferred_days          VARCHAR(200),
    preferred_time_slot     VARCHAR(30)   NOT NULL,

    technician_team_id      VARCHAR(50)   NOT NULL,
    technician_team_name    VARCHAR(200)  NOT NULL,

    service_sale_value      NUMERIC(14,2) NOT NULL DEFAULT 0,
    display_order           INTEGER       NOT NULL DEFAULT 1,

    CONSTRAINT fk_css_site FOREIGN KEY (contract_site_id)
        REFERENCES contract_sites(id) ON DELETE CASCADE
);
```

### `contract_payment_lines`

```sql
CREATE TABLE IF NOT EXISTS contract_payment_lines (
    id                  VARCHAR(50)   PRIMARY KEY,
    contract_id         VARCHAR(50)   NOT NULL,

    period_label        VARCHAR(50),
    period_description  VARCHAR(500),
    amount              NUMERIC(14,2) NOT NULL,
    due_date            DATE,
    paid                BOOLEAN       NOT NULL DEFAULT FALSE,
    locked              BOOLEAN       NOT NULL DEFAULT FALSE,
    sort_order          INTEGER       NOT NULL DEFAULT 1,

    CONSTRAINT fk_cpl_contract FOREIGN KEY (contract_id)
        REFERENCES contracts(id) ON DELETE CASCADE
);
```

### `contract_sales_order_links`

```sql
CREATE TABLE IF NOT EXISTS contract_sales_order_links (
    id                  VARCHAR(50)   PRIMARY KEY,
    contract_id         VARCHAR(50)   NOT NULL,

    sales_order_id      VARCHAR(50),

    sales_order_number  VARCHAR(50)   NOT NULL,
    sales_order_date    DATE,
    period_label        VARCHAR(100),
    so_value            NUMERIC(14,2) NOT NULL DEFAULT 0,
    so_status           VARCHAR(30)   NOT NULL DEFAULT 'DRAFT',
    service_status      VARCHAR(30)   NOT NULL DEFAULT 'PENDING',

    CONSTRAINT fk_csol_contract FOREIGN KEY (contract_id)
        REFERENCES contracts(id) ON DELETE CASCADE,
    CONSTRAINT fk_csol_sales_order FOREIGN KEY (sales_order_id)
        REFERENCES sales_orders(id) ON DELETE SET NULL
);
```


## 17 — Sales orders (V20, V25)

### `sales_orders`

```sql
CREATE TABLE IF NOT EXISTS sales_orders (
    id                          VARCHAR(50)   PRIMARY KEY,

    so_number                   VARCHAR(50)   NOT NULL UNIQUE,
    order_type                  VARCHAR(30)   NOT NULL
        CHECK (order_type IN ('SERVICE_CONTRACT', 'ONE_TIME_SERVICE', 'PRODUCT_SALE')),
    status                      VARCHAR(30)   NOT NULL
        CHECK (status IN ('DRAFT', 'OPEN', 'FULFILLED', 'BILLED', 'CANCELLED')),

    customer_id                 VARCHAR(50)   NOT NULL,
    customer_name               VARCHAR(255)  NOT NULL,
    branch_id                   VARCHAR(30)   NOT NULL,

    contract_id                 VARCHAR(50),
    contract_payment_line_id    VARCHAR(50),
    billing_period_label        VARCHAR(100),

    gma_sheet_id                VARCHAR(50),
    quotation_id                VARCHAR(50),

    one_time_source             VARCHAR(30)
        CHECK (one_time_source IS NULL OR one_time_source IN ('QUOTATION_GMA', 'STANDALONE')),

    so_date                     DATE          NOT NULL,

    sub_total                   NUMERIC(14,2) NOT NULL DEFAULT 0,
    discount_type               VARCHAR(20)
        CHECK (discount_type IS NULL OR discount_type IN ('NONE', 'FLAT', 'PERCENTAGE')),
    discount_value              NUMERIC(14,2) NOT NULL DEFAULT 0,
    discount_amount             NUMERIC(14,2) NOT NULL DEFAULT 0,
    tax_total                   NUMERIC(14,2) NOT NULL DEFAULT 0,
    grand_total                 NUMERIC(14,2) NOT NULL DEFAULT 0,

    execution_notes             TEXT,
    delivery_address_type       VARCHAR(30)
        CHECK (delivery_address_type IS NULL OR delivery_address_type IN ('REGISTERED_SITE', 'CUSTOM')),
    delivery_site_id            VARCHAR(50),
    delivery_address_line1      VARCHAR(500),
    delivery_address_line2      VARCHAR(500),
    delivery_city               VARCHAR(100),
    delivery_state              VARCHAR(100),
    delivery_pincode              VARCHAR(10),
    delivery_country            VARCHAR(100),
    delivery_google_map_url     TEXT,

    priority                    VARCHAR(20)
        CHECK (priority IS NULL OR priority IN ('NORMAL', 'URGENT', 'CRITICAL')),
    expected_delivery_date      DATE,

    invoice_linked              BOOLEAN       NOT NULL DEFAULT FALSE,
    job_cards_count             INTEGER       NOT NULL DEFAULT 0,
    challans_count              INTEGER       NOT NULL DEFAULT 0,

    cancel_reason               VARCHAR(50),
    cancel_remarks              VARCHAR(500),
    cancelled_at                TIMESTAMPTZ,
    cancelled_by                VARCHAR(100),

    created_by                  VARCHAR(100),
    updated_by                  VARCHAR(100),
    created_at                  TIMESTAMPTZ   NOT NULL DEFAULT now(),
    updated_at                  TIMESTAMPTZ
);
```

### `sales_order_sites`

```sql
CREATE TABLE IF NOT EXISTS sales_order_sites (
    id                      VARCHAR(50)   PRIMARY KEY,
    sales_order_id          VARCHAR(50)   NOT NULL,
    contract_site_id        VARCHAR(50),

    site_name               VARCHAR(200)  NOT NULL,
    address                 TEXT,
    city                    VARCHAR(100)  NOT NULL,
    state                   VARCHAR(100)  NOT NULL,
    country                 VARCHAR(100)  NOT NULL DEFAULT 'India',
    google_map_url          TEXT,
    category                VARCHAR(30)   NOT NULL,
    sub_category            VARCHAR(20)   NOT NULL,
    area_sqft               NUMERIC(12,2) NOT NULL DEFAULT 0,

    contact_person          VARCHAR(200),
    contact_mobile          VARCHAR(20),

    display_order           INTEGER       NOT NULL DEFAULT 1,

    CONSTRAINT fk_sos_so FOREIGN KEY (sales_order_id)
        REFERENCES sales_orders(id) ON DELETE CASCADE
);
```

### `sales_order_site_services`

```sql
CREATE TABLE IF NOT EXISTS sales_order_site_services (
    id                      VARCHAR(50)   PRIMARY KEY,
    sales_order_site_id     VARCHAR(50)   NOT NULL,

    service_type_id         VARCHAR(50)   NOT NULL,
    service_type_name       VARCHAR(200)  NOT NULL,

    visits                  NUMERIC(12,2) NOT NULL DEFAULT 1,
    unit_price              NUMERIC(14,2) NOT NULL DEFAULT 0,
    sqft                    NUMERIC(12,2) NOT NULL DEFAULT 0,
    hsn_code                VARCHAR(20),
    tax_percent             NUMERIC(6,3)  NOT NULL DEFAULT 0,
    tax_amount              NUMERIC(14,2) NOT NULL DEFAULT 0,
    line_total              NUMERIC(14,2) NOT NULL DEFAULT 0,

    executed_visits           NUMERIC(12,2) NOT NULL DEFAULT 0,

    display_order           INTEGER       NOT NULL DEFAULT 1,

    CONSTRAINT fk_soss_site FOREIGN KEY (sales_order_site_id)
        REFERENCES sales_order_sites(id) ON DELETE CASCADE
);
```

### `sales_order_site_chemicals`

```sql
CREATE TABLE IF NOT EXISTS sales_order_site_chemicals (
    id                      VARCHAR(50)   PRIMARY KEY,
    sales_order_site_id     VARCHAR(50)   NOT NULL,

    product_id              VARCHAR(50),
    product_name            VARCHAR(200)  NOT NULL,
    product_code            VARCHAR(50),
    uom                     VARCHAR(30),
    coverage_sqft           NUMERIC(12,2),
    required_qty            VARCHAR(50),
    unit_price              NUMERIC(14,2) NOT NULL DEFAULT 0,
    line_cost               NUMERIC(14,2) NOT NULL DEFAULT 0,
    hsn_code                VARCHAR(20),

    display_order           INTEGER       NOT NULL DEFAULT 1,

    CONSTRAINT fk_sosc_site FOREIGN KEY (sales_order_site_id)
        REFERENCES sales_order_sites(id) ON DELETE CASCADE
);
```

### `sales_order_product_lines`

```sql
CREATE TABLE IF NOT EXISTS sales_order_product_lines (
    id                      VARCHAR(50)   PRIMARY KEY,
    sales_order_id          VARCHAR(50)   NOT NULL,

    product_id              VARCHAR(50)   NOT NULL,
    product_name            VARCHAR(200)  NOT NULL,
    product_code            VARCHAR(50),
    uom                     VARCHAR(30),

    quantity                NUMERIC(14,3) NOT NULL DEFAULT 0,
    unit_price              NUMERIC(14,2) NOT NULL DEFAULT 0,
    hsn_code                VARCHAR(20),
    tax_percent             NUMERIC(6,3)  NOT NULL DEFAULT 0,
    tax_amount              NUMERIC(14,2) NOT NULL DEFAULT 0,
    line_total              NUMERIC(14,2) NOT NULL DEFAULT 0,

    display_order           INTEGER       NOT NULL DEFAULT 1,

    CONSTRAINT fk_sopl_so FOREIGN KEY (sales_order_id)
        REFERENCES sales_orders(id) ON DELETE CASCADE
);
```


## 18 — Petty cash (V21)

### `petty_cash_requests`

```sql
CREATE TABLE IF NOT EXISTS petty_cash_requests (
    id                      VARCHAR(50) PRIMARY KEY,  -- PC-YYYY-NNNN

    requester_user_id       BIGINT      NOT NULL,
    -- For CEO/global flows branch may be unavailable; keep nullable.
    requester_branch_id     VARCHAR(30),

    -- Draft can be saved without validation, so these become required at submit-time (service-level validation).
    category                VARCHAR(80),
    expense_date_from       DATE,
    expense_date_to         DATE,
    amount_requested        NUMERIC(14,2) CHECK (amount_requested > 0),
    description             VARCHAR(500),

    -- Generic placeholders for future module mapping (Task / Sales Order)
    related_task_ref        VARCHAR(60),
    related_so_ref          VARCHAR(60),

    -- Supporting documents meta
    justification_note      VARCHAR(500),

    -- Requested payment details (snapshot from employee profile; editable per request)
    payment_mode_requested  VARCHAR(20)
        CHECK (payment_mode_requested IN ('BANK_TRANSFER','UPI')),
    bank_account_holder     VARCHAR(150),
    bank_name               VARCHAR(150),
    bank_account_number     VARCHAR(50),
    bank_ifsc               VARCHAR(20),
    upi_id                  VARCHAR(120),

    -- Prior approval section
    is_pre_approved         BOOLEAN NOT NULL DEFAULT FALSE,
    pre_approved_by_user_id BIGINT,
    approval_reference      VARCHAR(200),

    -- Workflow
    status                  VARCHAR(20) NOT NULL DEFAULT 'DRAFT'
        CHECK (status IN ('DRAFT','PENDING','APPROVED','REJECTED','RETURNED','PAID','REVOKED')),

    submitted_at            TIMESTAMPTZ,
    submitted_to_label      VARCHAR(200),

    reviewed_by_user_id     BIGINT,
    reviewed_at             TIMESTAMPTZ,
    approved_amount         NUMERIC(14,2) CHECK (approved_amount >= 0),
    reviewer_remarks        VARCHAR(500),
    rejection_reason        VARCHAR(120),
    correction_notes        VARCHAR(500),

    -- Payment processing (Finance)
    payment_mode_processed  VARCHAR(20)
        CHECK (payment_mode_processed IN ('BANK_TRANSFER','UPI','CASH','CHEQUE')),
    transaction_ref         VARCHAR(120),
    payment_date            DATE,
    finance_remarks         VARCHAR(500),
    paid_by_user_id         BIGINT,
    paid_at                 TIMESTAMPTZ,

    -- Audit columns (repo convention)
    created_by              VARCHAR(100),
    updated_by              VARCHAR(100),
    created_at              TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at              TIMESTAMPTZ,

    CONSTRAINT fk_pc_requester_user
        FOREIGN KEY (requester_user_id) REFERENCES users(id) ON DELETE RESTRICT,

    CONSTRAINT fk_pc_branch
        FOREIGN KEY (requester_branch_id) REFERENCES branches(id) ON DELETE RESTRICT,

    CONSTRAINT fk_pc_pre_approved_by
        FOREIGN KEY (pre_approved_by_user_id) REFERENCES users(id) ON DELETE SET NULL,

    CONSTRAINT fk_pc_reviewed_by
        FOREIGN KEY (reviewed_by_user_id) REFERENCES users(id) ON DELETE SET NULL,

    CONSTRAINT fk_pc_paid_by
        FOREIGN KEY (paid_by_user_id) REFERENCES users(id) ON DELETE SET NULL,

    CONSTRAINT chk_pc_expense_date_range CHECK (expense_date_to >= expense_date_from)
);
```

### `petty_cash_request_recipients`

```sql
CREATE TABLE IF NOT EXISTS petty_cash_request_recipients (
    id                  VARCHAR(50) PRIMARY KEY,
    request_id           VARCHAR(50) NOT NULL,
    recipient_user_id    BIGINT,
    recipient_role_id    BIGINT,
    created_at           TIMESTAMPTZ NOT NULL DEFAULT now(),

    CONSTRAINT fk_pc_rec_request
        FOREIGN KEY (request_id) REFERENCES petty_cash_requests(id) ON DELETE CASCADE,

    CONSTRAINT fk_pc_rec_user
        FOREIGN KEY (recipient_user_id) REFERENCES users(id) ON DELETE CASCADE,

    CONSTRAINT fk_pc_rec_role
        FOREIGN KEY (recipient_role_id) REFERENCES roles(id) ON DELETE CASCADE,

    CONSTRAINT chk_pc_rec_one_target CHECK (
        (
            (CASE WHEN recipient_user_id IS NOT NULL THEN 1 ELSE 0 END) +
            (CASE WHEN recipient_role_id IS NOT NULL THEN 1 ELSE 0 END)
        ) = 1
    )
);
```

### `petty_cash_attachments`

```sql
CREATE TABLE IF NOT EXISTS petty_cash_attachments (
    id              VARCHAR(50) PRIMARY KEY,
    request_id       VARCHAR(50) NOT NULL,

    attachment_type  VARCHAR(30) NOT NULL
        CHECK (attachment_type IN ('RECEIPT','PAYMENT_PROOF')),

    file_key         VARCHAR(600) NOT NULL,
    file_name        VARCHAR(255),
    content_type     VARCHAR(100),
    file_size_bytes  BIGINT,
    notes            VARCHAR(500),

    uploaded_by      VARCHAR(120),
    uploaded_at      TIMESTAMPTZ NOT NULL DEFAULT now(),

    CONSTRAINT fk_pc_att_request
        FOREIGN KEY (request_id) REFERENCES petty_cash_requests(id) ON DELETE CASCADE
);
```

### `petty_cash_audit_logs`

```sql
CREATE TABLE IF NOT EXISTS petty_cash_audit_logs (
    id              VARCHAR(50) PRIMARY KEY,
    request_id       VARCHAR(50) NOT NULL,

    action           VARCHAR(50) NOT NULL
        CHECK (action IN (
            'DRAFT_SAVED',
            'SUBMITTED',
            'RECIPIENTS_SELECTED',
            'REVOKED',
            'APPROVED',
            'REJECTED',
            'RETURNED',
            'PAID'
        )),

    actor_user_id    BIGINT,
    actor_name       VARCHAR(150),
    remarks          VARCHAR(500),
    action_at        TIMESTAMPTZ NOT NULL DEFAULT now(),

    CONSTRAINT fk_pc_aud_request
        FOREIGN KEY (request_id) REFERENCES petty_cash_requests(id) ON DELETE CASCADE,

    CONSTRAINT fk_pc_aud_actor
        FOREIGN KEY (actor_user_id) REFERENCES users(id) ON DELETE SET NULL
);
```


## 19 — Task management (V25, V27)

### `tasks`

```sql
CREATE TABLE IF NOT EXISTS tasks (
    id                      VARCHAR(50)   PRIMARY KEY,
    branch_id               VARCHAR(30)   NOT NULL,
    task_number             VARCHAR(50)   NOT NULL UNIQUE,
    task_type               VARCHAR(20)   NOT NULL CHECK (task_type IN ('NORMAL', 'RE_TASK')),
    source_type             VARCHAR(30)   NOT NULL CHECK (source_type IN ('SALES_ORDER', 'CUSTOMER_TICKET', 'MANUAL')),
    
    -- Source Links
    sales_order_id          VARCHAR(50),
    so_site_service_id      VARCHAR(50),
    ticket_id               VARCHAR(50),
    
    -- Customer & Site Info (Snapshotted for performance and history)
    customer_id             VARCHAR(50)   NOT NULL,
    customer_name           VARCHAR(255)  NOT NULL,
    site_id                 VARCHAR(50),
    site_name               VARCHAR(255)  NOT NULL,
    site_address            TEXT,
    site_contact_name       VARCHAR(200),
    site_contact_mobile     VARCHAR(20),
    
    -- Service Info Details
    service_category        VARCHAR(100),
    service_subcategory     VARCHAR(100),
    service_type_name       VARCHAR(200),
    area_sqft               NUMERIC(12,2),
    
    -- Scheduling
    scheduled_date          DATE          NOT NULL,
    start_time              TIME          NOT NULL,
    end_time                TIME          NOT NULL,
    estimated_duration_mins INTEGER,
    
    -- Status & Priority
    status                  VARCHAR(30)   NOT NULL CHECK (status IN ('PENDING', 'IN_PROGRESS', 'COMPLETED', 'OVERDUE', 'CANCELLED')),
    priority                VARCHAR(20)   NOT NULL DEFAULT 'NORMAL' CHECK (priority IN ('NORMAL', 'HIGH', 'URGENT', 'CRITICAL')),
    
    -- Execution Tracking
    actual_start_at         TIMESTAMPTZ,
    actual_end_at           TIMESTAMPTZ,
    completion_notes        TEXT,
    
    -- Feedback Loop
    customer_rating         INTEGER       CHECK (customer_rating BETWEEN 1 AND 5),
    customer_feedback       TEXT,
    feedback_at             TIMESTAMPTZ,
    
    -- System Audit
    created_by              VARCHAR(100),
    created_at              TIMESTAMPTZ   NOT NULL DEFAULT now(),
    updated_by              VARCHAR(100),
    updated_at              TIMESTAMPTZ   NOT NULL DEFAULT now(),

    CONSTRAINT fk_tasks_support_ticket FOREIGN KEY (ticket_id)
        REFERENCES support_tickets(id) ON DELETE SET NULL
);
```

### `task_technicians`

```sql
CREATE TABLE IF NOT EXISTS task_technicians (
    id                      VARCHAR(50)   PRIMARY KEY,
    task_id                 VARCHAR(50)   NOT NULL,
    user_id                 BIGINT        NOT NULL, -- Emp ID from users table
    employee_name           VARCHAR(200)  NOT NULL,
    role_name               VARCHAR(100),
    is_primary              BOOLEAN       NOT NULL DEFAULT FALSE,
    
    CONSTRAINT fk_tt_task FOREIGN KEY (task_id) REFERENCES tasks(id) ON DELETE CASCADE,
    CONSTRAINT fk_tt_user FOREIGN KEY (user_id) REFERENCES users(id)
);
```

### `task_materials`

```sql
CREATE TABLE IF NOT EXISTS task_materials (
    id                      VARCHAR(50)   PRIMARY KEY,
    task_id                 VARCHAR(50)   NOT NULL,
    product_id              VARCHAR(50)   NOT NULL,
    product_name            VARCHAR(255)  NOT NULL,
    uom                     VARCHAR(30),
    hsn_code                VARCHAR(20),
    std_qty                 NUMERIC(14,3),
    required_qty            NUMERIC(14,3) NOT NULL,
    used_qty                NUMERIC(14,3),
    
    CONSTRAINT fk_tm_task FOREIGN KEY (task_id) REFERENCES tasks(id) ON DELETE CASCADE
);
```

### `task_photos`

```sql
CREATE TABLE IF NOT EXISTS task_photos (
    id                      VARCHAR(50)   PRIMARY KEY,
    task_id                 VARCHAR(50)   NOT NULL,
    photo_type              VARCHAR(30)   NOT NULL CHECK (photo_type IN ('BEFORE', 'AFTER', 'TREATMENT')),
    file_path               VARCHAR(500)  NOT NULL,
    uploaded_at             TIMESTAMPTZ   NOT NULL DEFAULT now(),
    
    CONSTRAINT fk_tp_task FOREIGN KEY (task_id) REFERENCES tasks(id) ON DELETE CASCADE
);
```

### `task_audit_logs`

```sql
CREATE TABLE IF NOT EXISTS task_audit_logs (
    id                      BIGSERIAL     PRIMARY KEY,
    task_id                 VARCHAR(50)   NOT NULL,
    action                  VARCHAR(100)  NOT NULL,
    details                 TEXT,
    performed_by            VARCHAR(100),
    performed_at            TIMESTAMPTZ   NOT NULL DEFAULT now()
);
```


## 20 — Customer support (V27)

### `support_ticket_types`

```sql
CREATE TABLE IF NOT EXISTS support_ticket_types (
    id           VARCHAR(50) PRIMARY KEY,
    code         VARCHAR(80)  NOT NULL UNIQUE,
    label        VARCHAR(200) NOT NULL,
    display_order INTEGER      NOT NULL DEFAULT 0,
    active       BOOLEAN      NOT NULL DEFAULT TRUE,
    created_at   TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at   TIMESTAMPTZ  NOT NULL DEFAULT now()
);
```

### `support_sla_settings`

```sql
CREATE TABLE IF NOT EXISTS support_sla_settings (
    id                           SMALLINT PRIMARY KEY DEFAULT 1 CHECK (id = 1),
    response_sla_hours           INTEGER      NOT NULL DEFAULT 2,
    resolution_risk_threshold_pct INTEGER     NOT NULL DEFAULT 80
        CHECK (resolution_risk_threshold_pct > 0 AND resolution_risk_threshold_pct < 100),
    updated_at                   TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_by                   VARCHAR(100)
);
```

### `support_tickets`

```sql
CREATE TABLE IF NOT EXISTS support_tickets (
    id                          VARCHAR(50)   PRIMARY KEY,
    ticket_number               VARCHAR(50)   NOT NULL UNIQUE,

    customer_id                 VARCHAR(50)   NOT NULL,
    branch_id                   VARCHAR(30)   NOT NULL,
    sales_order_id              VARCHAR(50),
    related_task_id             VARCHAR(50),

    ticket_type_id              VARCHAR(50)   NOT NULL,
    priority                    VARCHAR(20)   NOT NULL
        CHECK (priority IN ('NORMAL', 'HIGH', 'URGENT', 'CRITICAL')),

    subject                     VARCHAR(100)  NOT NULL,
    description                 TEXT          NOT NULL,

    reported_by_name            VARCHAR(200)  NOT NULL,
    reported_by_phone           VARCHAR(20)   NOT NULL,

    expected_resolution_date    DATE          NOT NULL,
    expected_resolution_time    TIME          NOT NULL,

    status                      VARCHAR(30)   NOT NULL DEFAULT 'OPEN'
        CHECK (status IN (
            'OPEN', 'ASSIGNED', 'IN_PROGRESS', 'PAUSED', 'RESOLVED', 'CLOSED'
        )),

    assignee_role_code          VARCHAR(50),
    assigned_user_id            BIGINT,

    -- Dual SLA tracking (resolution_expected_at = expected date + time in app TZ)
    response_sla_deadline_at    TIMESTAMPTZ   NOT NULL,
    first_response_at           TIMESTAMPTZ,
    resolution_expected_at      TIMESTAMPTZ   NOT NULL,

    response_sla_met            BOOLEAN,
    response_sla_breached       BOOLEAN       NOT NULL DEFAULT FALSE,
    resolution_sla_breached     BOOLEAN       NOT NULL DEFAULT FALSE,

    sla_risk_at                 TIMESTAMPTZ,

    escalation_level            VARCHAR(20)   NOT NULL DEFAULT 'NONE'
        CHECK (escalation_level IN ('NONE', 'L1', 'L2', 'L3')),

    pause_started_at            TIMESTAMPTZ,
    total_paused_seconds        INTEGER       NOT NULL DEFAULT 0,

    -- Resolution (Screen 23.6)
    resolution_code             VARCHAR(50)
        CHECK (resolution_code IS NULL OR resolution_code IN (
            'SERVICE_RESOLVED_SUCCESS',
            'FALSE_ALARM',
            'DUPLICATE_TICKET',
            'UNRESOLVED_CLOSED'
        )),
    resolution_notes            TEXT,
    resolve_customer_rating     INTEGER
        CHECK (resolve_customer_rating IS NULL OR (resolve_customer_rating >= 1 AND resolve_customer_rating <= 5)),
    resolve_customer_feedback   TEXT,
    resolved_at                 TIMESTAMPTZ,
    resolved_by                 VARCHAR(100),

    -- Close (Screen 23.7)
    close_reason                VARCHAR(80)
        CHECK (close_reason IS NULL OR close_reason IN (
            'RESOLVED_CUSTOMER_SATISFACTION',
            'DUPLICATE_REQUEST',
            'CUSTOMER_NON_RESPONSIVE',
            'OUT_OF_SCOPE'
        )),
    closure_remarks             TEXT,
    closed_at                   TIMESTAMPTZ,
    closed_by                   VARCHAR(100),

    created_at                  TIMESTAMPTZ   NOT NULL DEFAULT now(),
    updated_at                  TIMESTAMPTZ   NOT NULL DEFAULT now(),
    created_by                  VARCHAR(100),
    updated_by                  VARCHAR(100),

    CONSTRAINT fk_st_customer FOREIGN KEY (customer_id)
        REFERENCES customers(id) ON DELETE RESTRICT,
    CONSTRAINT fk_st_branch FOREIGN KEY (branch_id)
        REFERENCES branches(id) ON DELETE RESTRICT,
    CONSTRAINT fk_st_so FOREIGN KEY (sales_order_id)
        REFERENCES sales_orders(id) ON DELETE SET NULL,
    CONSTRAINT fk_st_related_task FOREIGN KEY (related_task_id)
        REFERENCES tasks(id) ON DELETE SET NULL,
    CONSTRAINT fk_st_ticket_type FOREIGN KEY (ticket_type_id)
        REFERENCES support_ticket_types(id) ON DELETE RESTRICT,
    CONSTRAINT fk_st_assignee_user FOREIGN KEY (assigned_user_id)
        REFERENCES users(id) ON DELETE SET NULL
);
```

### `support_ticket_tasks`

```sql
CREATE TABLE IF NOT EXISTS support_ticket_tasks (
    id         VARCHAR(50) PRIMARY KEY,
    ticket_id  VARCHAR(50) NOT NULL,
    task_id    VARCHAR(50) NOT NULL,
    linked_at  TIMESTAMPTZ NOT NULL DEFAULT now(),
    linked_by  VARCHAR(100),

    CONSTRAINT fk_stt_ticket FOREIGN KEY (ticket_id)
        REFERENCES support_tickets(id) ON DELETE CASCADE,
    CONSTRAINT fk_stt_task FOREIGN KEY (task_id)
        REFERENCES tasks(id) ON DELETE CASCADE,
    CONSTRAINT uq_stt_ticket_task UNIQUE (ticket_id, task_id)
);
```

### `support_ticket_activities`

```sql
CREATE TABLE IF NOT EXISTS support_ticket_activities (
    id             BIGSERIAL PRIMARY KEY,
    ticket_id      VARCHAR(50)   NOT NULL,
    activity_type  VARCHAR(50)   NOT NULL,
    summary        TEXT          NOT NULL,
    detail         TEXT,
    is_internal    BOOLEAN       NOT NULL DEFAULT FALSE,
    performed_by_user_id BIGINT,
    performed_by_label     VARCHAR(200),
    performed_at   TIMESTAMPTZ   NOT NULL DEFAULT now(),
    metadata_json  JSONB,

    CONSTRAINT fk_sta_ticket FOREIGN KEY (ticket_id)
        REFERENCES support_tickets(id) ON DELETE CASCADE,
    CONSTRAINT fk_sta_user FOREIGN KEY (performed_by_user_id)
        REFERENCES users(id) ON DELETE SET NULL
);
```

### `support_ticket_attachments`

```sql
CREATE TABLE IF NOT EXISTS support_ticket_attachments (
    id            VARCHAR(50) PRIMARY KEY,
    ticket_id     VARCHAR(50)  NOT NULL,
    phase         VARCHAR(20)  NOT NULL
        CHECK (phase IN ('RAISE', 'RESOLUTION', 'CLOSE', 'NOTE')),
    file_path     VARCHAR(500) NOT NULL,
    original_name VARCHAR(255),
    mime_type     VARCHAR(100),
    size_bytes    BIGINT,
    uploaded_at   TIMESTAMPTZ  NOT NULL DEFAULT now(),
    uploaded_by   VARCHAR(100),

    CONSTRAINT fk_attach_ticket FOREIGN KEY (ticket_id)
        REFERENCES support_tickets(id) ON DELETE CASCADE
);
```

### `support_ticket_assignment_history`

```sql
CREATE TABLE IF NOT EXISTS support_ticket_assignment_history (
    id                BIGSERIAL PRIMARY KEY,
    ticket_id         VARCHAR(50)  NOT NULL,
    from_user_id      BIGINT,
    to_user_id        BIGINT       NOT NULL,
    to_role_code      VARCHAR(50)  NOT NULL,
    assignment_note   TEXT,
    assigned_at       TIMESTAMPTZ  NOT NULL DEFAULT now(),
    assigned_by       VARCHAR(100),

    CONSTRAINT fk_stah_ticket FOREIGN KEY (ticket_id)
        REFERENCES support_tickets(id) ON DELETE CASCADE,
    CONSTRAINT fk_stah_from FOREIGN KEY (from_user_id)
        REFERENCES users(id) ON DELETE SET NULL,
    CONSTRAINT fk_stah_to FOREIGN KEY (to_user_id)
        REFERENCES users(id) ON DELETE RESTRICT
);
```


## 21 — HRM (V28)

### `hrm_salary_month`

```sql
CREATE TABLE IF NOT EXISTS hrm_salary_month (
    id                    BIGSERIAL PRIMARY KEY,
    user_id               BIGINT      NOT NULL,
    salary_year           INT         NOT NULL,
    salary_month          INT         NOT NULL CHECK (salary_month BETWEEN 1 AND 12),

    -- Monthly editable components (seeded from user_salary_details initially)
    basic_salary          NUMERIC(12,2) NOT NULL DEFAULT 0.00,
    hra                   NUMERIC(12,2) NOT NULL DEFAULT 0.00,
    other_allowance       NUMERIC(12,2) NOT NULL DEFAULT 0.00,
    incentive             NUMERIC(12,2) NOT NULL DEFAULT 0.00,
    deductions            NUMERIC(12,2) NOT NULL DEFAULT 0.00,
    other_deductions      NUMERIC(12,2) NOT NULL DEFAULT 0.00,

    -- Statutory deduction amounts for the month
    pf                    NUMERIC(12,2) NOT NULL DEFAULT 0.00,
    esi                   NUMERIC(12,2) NOT NULL DEFAULT 0.00,
    tds                   NUMERIC(12,2) NOT NULL DEFAULT 0.00,

    -- OT/Holiday actuals for the month (entered by HR)
    ot_hours              NUMERIC(8,2)  NOT NULL DEFAULT 0.00,
    holiday_days_worked   INT          NOT NULL DEFAULT 0,
    ot_amount             NUMERIC(12,2) NOT NULL DEFAULT 0.00,
    holiday_incentive_amt NUMERIC(12,2) NOT NULL DEFAULT 0.00,

    -- Totals
    gross_salary          NUMERIC(12,2) NOT NULL DEFAULT 0.00,
    total_deductions      NUMERIC(12,2) NOT NULL DEFAULT 0.00,
    net_salary            NUMERIC(12,2) NOT NULL DEFAULT 0.00,

    -- Payment tracking
    payment_status        VARCHAR(20)  NOT NULL DEFAULT 'UNPAID'
                          CHECK (payment_status IN ('PAID', 'UNPAID', 'DUE')),
    reason                TEXT,
    payment_date          DATE,
    paid_by_user_id       BIGINT,
    paid_at               TIMESTAMPTZ,

    created_by            VARCHAR(100),
    created_at            TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_by            VARCHAR(100),
    updated_at            TIMESTAMPTZ  NOT NULL DEFAULT now(),

    CONSTRAINT fk_hrm_salary_user FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    CONSTRAINT fk_hrm_salary_paid_by FOREIGN KEY (paid_by_user_id) REFERENCES users(id) ON DELETE SET NULL,
    CONSTRAINT uk_hrm_salary_user_month UNIQUE (user_id, salary_year, salary_month)
);
```

### `hrm_salary_slip`

```sql
CREATE TABLE IF NOT EXISTS hrm_salary_slip (
    id               BIGSERIAL PRIMARY KEY,
    salary_month_id  BIGINT      NOT NULL UNIQUE,
    file_path        VARCHAR(500) NOT NULL,
    generated_by     VARCHAR(100),
    generated_at     TIMESTAMPTZ NOT NULL DEFAULT now(),

    CONSTRAINT fk_hrm_salary_slip_month FOREIGN KEY (salary_month_id) REFERENCES hrm_salary_month(id) ON DELETE CASCADE
);
```

### `hrm_holidays`

```sql
CREATE TABLE IF NOT EXISTS hrm_holidays (
    id            BIGSERIAL PRIMARY KEY,
    holiday_date  DATE         NOT NULL,
    name          VARCHAR(200) NOT NULL,
    branch_id     VARCHAR(30),

    created_by    VARCHAR(100),
    created_at    TIMESTAMPTZ  NOT NULL DEFAULT now(),

    CONSTRAINT uk_hrm_holiday UNIQUE (holiday_date, branch_id)
);
```

### `hrm_attendance_day`

```sql
CREATE TABLE IF NOT EXISTS hrm_attendance_day (
    id              BIGSERIAL PRIMARY KEY,
    user_id         BIGINT      NOT NULL,
    attendance_date DATE        NOT NULL,

    punch_in_at     TIMESTAMPTZ,
    punch_out_at    TIMESTAMPTZ,
    total_minutes   INT,

    status          VARCHAR(20) NOT NULL
                    CHECK (status IN ('PRESENT', 'ABSENT', 'LEAVE', 'WEEK_OFF', 'HOLIDAY', 'HALF_DAY')),
    source          VARCHAR(20) NOT NULL DEFAULT 'MANUAL'
                    CHECK (source IN ('TASK', 'AUTO', 'MANUAL', 'UPLOAD')),
    notes           VARCHAR(1000),

    tasks_assigned  INT         NOT NULL DEFAULT 0,
    tasks_completed INT         NOT NULL DEFAULT 0,
    tasks_pending   INT         NOT NULL DEFAULT 0,

    created_by      VARCHAR(100),
    created_at      TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_by      VARCHAR(100),
    updated_at      TIMESTAMPTZ NOT NULL DEFAULT now(),

    CONSTRAINT fk_hrm_attendance_user FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    CONSTRAINT uk_hrm_attendance_user_date UNIQUE (user_id, attendance_date)
);
```

### `hrm_leave_request`

```sql
CREATE TABLE IF NOT EXISTS hrm_leave_request (
    id                 BIGSERIAL PRIMARY KEY,
    leave_code         VARCHAR(30)  NOT NULL UNIQUE, -- LV-xxxx
    user_id            BIGINT       NOT NULL,
    leave_type         VARCHAR(10)  NOT NULL CHECK (leave_type IN ('CL', 'SL', 'PL')),
    from_date          DATE         NOT NULL,
    to_date            DATE         NOT NULL,
    working_days       INT          NOT NULL DEFAULT 0,
    description        TEXT         NOT NULL,

    status             VARCHAR(20)  NOT NULL
                       CHECK (status IN ('PENDING', 'APPROVED', 'REJECTED')),
    rejection_reason   TEXT,
    reviewed_by_user_id BIGINT,
    reviewed_at        TIMESTAMPTZ,

    created_by         VARCHAR(100),
    created_at         TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_by         VARCHAR(100),
    updated_at         TIMESTAMPTZ  NOT NULL DEFAULT now(),

    CONSTRAINT fk_hrm_leave_user FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    CONSTRAINT fk_hrm_leave_reviewed_by FOREIGN KEY (reviewed_by_user_id) REFERENCES users(id) ON DELETE SET NULL,
    CONSTRAINT chk_hrm_leave_dates CHECK (from_date <= to_date)
);
```

