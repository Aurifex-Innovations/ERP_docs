# Module 15: Lead and Follow-up Management API Documentation

## Short Description
The Lead and Follow-up Management module is a comprehensive system designed to capture, track, and nurture potential customer inquiries. It manages the entire lead lifecycle from initial capture (Draft/New) through qualification and negotiation, eventually converting them into Quotations or Service Contracts.

---

## APIs

### Create a New Lead
- **Method:** `POST`
- **Endpoint:** `/api/v1/leads`
- **Purpose:** Registers a new customer inquiry in the system.
- **Use Case:** Used by sales representatives or administrators to log a new prospect's details.
- **Used In:** Add New Lead Form
- **Request Body:** [LeadRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/request/LeadRequest.java#9-56)
- **Response:** [LeadResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/response/LeadResponse.java#8-39) (201 Created)
- **Access / Security:** `LEAD_MANAGEMENT_ADD` or `ROLE_CEO`
- **Notes:** 
    - Initial status is typically `NEW`.
    - Can be saved as `DRAFT` (partial validation).
    - Mobile number must be unique and 10 digits.
    - `Next Follow-up Date` is mandatory and must be today or in the future.

### Update an Existing Lead
- **Method:** `PUT`
- **Endpoint:** `/api/v1/leads?id={id}`
- **Purpose:** Modifies details of an existing lead.
- **Use Case:** Correcting lead information or progressing the lead through the lifecycle.
- **Used In:** Edit Lead Form
- **Request Params:** `id` (Lead UUID)
- **Request Body:** [LeadRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/request/LeadRequest.java#9-56)
- **Response:** [LeadResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/response/LeadResponse.java#8-39)
- **Access / Security:** `LEAD_MANAGEMENT_EDIT` or `ROLE_CEO`
- **Notes:** 
    - `Mobile Number` and `Lead Source` are immutable after creation.
    - Editability of fields depends on the current `status` (e.g., restricted after `QUALIFIED`).
    - If status is changed to `LOST`, a `lostReason` must be provided.

### Get Lead by ID
- **Method:** `GET`
- **Endpoint:** `/api/v1/leads/by-id?id={id}`
- **Purpose:** Fetches complete details of a specific lead.
- **Use Case:** Viewing lead details in the management dashboard or before editing.
- **Used In:** Lead Detail View (Tab 1), Edit Lead Form
- **Request Params:** `id` (Lead UUID)
- **Response:** [LeadResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/response/LeadResponse.java#8-39)
- **Access / Security:** `LEAD_MANAGEMENT_VIEW` or `ROLE_CEO`

### Get All Leads (Lead Dashboard)
- **Method:** `GET`
- **Endpoint:** `/api/v1/leads`
- **Purpose:** Retrieves a paginated list of leads with advanced filtering.
- **Use Case:** Powering the Lead Management Dashboard table.
- **Used In:** Lead Management Dashboard
- **Query Params:**
    - `statuses`: List of `LeadStatus`
    - `sources`: List of `LeadSource`
    - `priorities`: List of `LeadPriority`
    - `leadTypes`: List of `LeadType`
    - `branchIds`: List of Strings
    - `startDate` / `endDate`: `LocalDate` (Created Date range)
    - `search`: String (Search by Lead ID, Name, Mobile, Email)
    - `pageNo` / `pageSize`: Integer (Pagination)
- **Response:** `PaginationResponse<LeadResponse>`
- **Access / Security:** `LEAD_MANAGEMENT_VIEW` or `ROLE_CEO`

### Add a New Follow-up
- **Method:** `POST`
- **Endpoint:** `/api/v1/follow-ups`
- **Purpose:** Logs an interaction with a lead and schedules the next action.
- **Use Case:** Recording a call, meeting, or site visit outcome.
- **Used In:** Add Follow-up Form / Quick Action
- **Request Body:** [FollowUpRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/request/FollowUpRequest.java#14-44)
- **Response:** [FollowUpResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/response/FollowUpResponse.java#11-31) (201 Created)
- **Access / Security:** `FOLLOW_UP_MANAGEMENT_ADD` or `ROLE_CEO`
- **Notes:** 
    - Automatically updates the parent Lead's `status` and `nextFollowUpDate`.
    - Captures `Interaction Summary` (Min 10 chars).
    - If `nextActionScheduled` is true, `nextFollowUpDate` and `reasonAgenda` are required.

### Get Follow-up History by Lead ID
- **Method:** `GET`
- **Endpoint:** `/api/v1/follow-ups/lead?leadId={leadId}`
- **Purpose:** Fetches all past follow-up records for a specific lead.
- **Use Case:** Reviewing chronological interaction history.
- **Used In:** Lead Detail View (Tab 2: Follow-up Log)
- **Request Params:** `leadId` (Lead UUID)
- **Response:** `List<FollowUpResponse>`
- **Access / Security:** `FOLLOW_UP_MANAGEMENT_VIEW` or `ROLE_CEO`

### Get Follow-up by ID
- **Method:** `GET`
- **Endpoint:** `/api/v1/follow-ups?id={id}`
- **Purpose:** Fetches details of a specific follow-up interaction.
- **Use Case:** Viewing detailed notes of a past interaction.
- **Used In:** View Follow-up Detail Screen
- **Request Params:** `id` (Follow-up UUID)
- **Response:** [FollowUpResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/response/FollowUpResponse.java#11-31)
- **Access / Security:** `FOLLOW_UP_MANAGEMENT_VIEW` or `ROLE_CEO`

---

## Reusable / Common APIs (Used in this Module)

### Get Active Branches
- **Method:** `GET`
- **Endpoint:** `/api/v1/company/branches`
- **Purpose:** Fetches list of branches for assignment.
- **Used In:** Lead Creation/Edit form (Branch Dropdown), Dashboard Filters.

---

## Module Enums

### LeadStatus
- **Used In:** [LeadRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/request/LeadRequest.java#9-56), [LeadResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/response/LeadResponse.java#8-39), [FollowUpRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/request/FollowUpRequest.java#14-44)
- **Values:** `DRAFT`, `NEW`, `QUALIFIED`, `QUOTATION_SENT`, `NEGOTIATION`, `LOST`, `CONVERTED`

### LeadSource
- **Used In:** [LeadRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/request/LeadRequest.java#9-56), [LeadResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/response/LeadResponse.java#8-39)
- **Values:** `WEBSITE`, `REFERRAL`, `WALK_IN`, `COLD_CALL`, `SOCIAL_MEDIA`, `EXHIBITION`, `PARTNER`

### LeadPriority
- **Used In:** [LeadRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/request/LeadRequest.java#9-56), [LeadResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/response/LeadResponse.java#8-39)
- **Values:** `LOW`, `NORMAL`, `HIGH`, `URGENT`

### LeadType
- **Used In:** [LeadRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/request/LeadRequest.java#9-56), [LeadResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/response/LeadResponse.java#8-39)
- **Values:** `PRODUCT`, `SERVICE`

### ServiceType
- **Used In:** [LeadRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/request/LeadRequest.java#9-56), [LeadResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/response/LeadResponse.java#8-39)
- **Values:** `CONTRACT`, `PRODUCT_PURCHASE`, `JOBBING`
- **Note:** Only applicable if `LeadType` is `SERVICE`.

### BudgetRange
- **Used In:** [LeadRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/request/LeadRequest.java#9-56), [LeadResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/response/LeadResponse.java#8-39)
- **Values:** `UNDER_5K`, `BETWEEN_5K_10K`, `BETWEEN_10K_25K`, `BETWEEN_25K_50K`, `OVER_50K`, `NOT_DISCUSSED`

### ContactMode
- **Used In:** [FollowUpRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/request/FollowUpRequest.java#14-44), [FollowUpResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/response/FollowUpResponse.java#11-31)
- **Values:** `CALL`, `MEETING`, `SITE_VISIT`, `EMAIL`, `WHATSAPP`, `OTHER`

### GmaStatus
- **Used In:** [LeadResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/lead/dto/response/LeadResponse.java#8-39)
- **Values:** `NOT_CREATED`, `DRAFT`, `UNDER_REVIEW`, `APPROVED`, `REJECTED`

---

## Notes for Frontend

- **Dropdown APIs:**
    - `Lead Source`, `Priority`, `Lead Type`, `Service Type`, `Budget Range`, `Status` are all driven by the enums listed above.
    - `Branch Name` should be fetched from the `Branch Management` module.
- **List APIs:**
    - The `GET /api/v1/leads` endpoint is the primary source for the dashboard table. Supports multi-select filtering for enums.
- **Detail APIs:**
    - `GET /api/v1/leads/by-id` provides the full model for "Tab 1".
    - `GET /api/v1/follow-ups/lead` provides the chronological list for "Tab 2".
- **Dynamic UI Logic:**
    - **Visibility:** Show `Service Type` dropdown only if `Lead Type == SERVICE`.
    - **Mandatory Fields:** If `Status == LOST` is selected in Add/Edit Lead or Add Follow-up, the `lostReason` text area becomes mandatory.
    - **Immutability:** `Mobile Number` and `Lead Source` should be disabled in the Edit form.
    - **Follow-up Planning:** In the `Add Follow-up` form, if `Schedule Next Action` is checked, the `Next Follow-up Date` and `Reason/Agenda` become mandatory.
