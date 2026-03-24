# Subscription Plans API Documentation

## Short Description
The Subscription Plans module enables Seravion (Super Admin) to define and manage various subscription models for tenants. It allows for the creation, modification, and inactivation of plans with specific resource limits (branches, technicians) and pricing structures.

---

## APIs

### Create Subscription Plan
- **Method:** `POST`
- **Endpoint:** `/api/v1/root/subscription-plans`
- **Purpose:** Creates a new subscription plan with specific limits and pricing.
- **Use Case:** Seravion Admin creates a new "Standard" or "Premium" plan to be offered to clients.
- **Used In:** Seravion Admin Dashboard -> Plan Management -> Create Plan
- **Request Body:** [CreateSubscriptionPlanRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/subscriptionPlan/dto/request/CreateSubscriptionPlanRequest.java#10-45)
- **Response:** [SubscriptionPlanResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/subscriptionPlan/dto/response/SubscriptionPlanResponse.java#10-25)
- **Access / Security:** `hasRole('SERAVION')`
- **Notes:** Validates plan name uniqueness and positive resource counts/prices.

### Update Subscription Plan
- **Method:** `PUT`
- **Endpoint:** `/api/v1/root/subscription-plans`
- **Purpose:** Updates detail of an existing subscription plan.
- **Use Case:** Adjusting the price or resource limits of an existing plan.
- **Used In:** Seravion Admin Dashboard -> Plan Management -> Edit Plan
- **Request Params:** `id` (Query Param)
- **Request Body:** [CreateSubscriptionPlanRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/subscriptionPlan/dto/request/CreateSubscriptionPlanRequest.java#10-45)
- **Response:** [SubscriptionPlanResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/subscriptionPlan/dto/response/SubscriptionPlanResponse.java#10-25)
- **Access / Security:** `hasRole('SERAVION')`
- **Notes:** Updates are reflected for future purchases; existing subscriptions remain unaffected.

### Get Subscription Plan
- **Method:** `GET`
- **Endpoint:** `/api/v1/root/subscription-plans`
- **Purpose:** Retrieves details of a specific plan by its ID.
- **Use Case:** Viewing plan details before editing or for display in the CEO dashboard.
- **Used In:** Seravion Admin Dashboard, CEO Subscription Details
- **Request Params:** `id` (Query Param)
- **Response:** [SubscriptionPlanResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/subscriptionPlan/dto/response/SubscriptionPlanResponse.java#10-25)
- **Access / Security:** `hasAnyRole('SERAVION', 'CEO')`

### Inactivate Subscription Plan
- **Method:** `DELETE`
- **Endpoint:** `/api/v1/root/subscription-plans/inactivate`
- **Purpose:** Soft-deletes/Inactivates a plan so it cannot be selected for new subscriptions.
- **Use Case:** Discontinuing an old subscription plan.
- **Used In:** Seravion Admin Dashboard -> Plan List -> Inactivate Action
- **Request Params:** `id` (Query Param)
- **Response:** [SubscriptionPlanResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/subscriptionPlan/dto/response/SubscriptionPlanResponse.java#10-25)
- **Access / Security:** `hasRole('SERAVION')`
- **Notes:** Performs a soft delete by changing status to `INACTIVE`.

### Get All Subscription Plans
- **Method:** `GET`
- **Endpoint:** `/api/v1/root/subscription-plans/get-all`
- **Purpose:** Retrieves a comprehensive list of all plans with summary metadata.
- **Use Case:** Listing all plans for administrative oversight.
- **Used In:** Seravion Admin Dashboard -> Plan Management
- **Response:** [SubscriptionPlanListResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/subscriptionPlan/dto/response/SubscriptionPlanListResponse.java#7-20) (includes total, active, and inactive counts)
- **Access / Security:** `hasAnyRole('SERAVION', 'CEO')`

---

## Reusable / Common APIs (Used in this Module)

### Get Active Subscription Plans Dropdown
- **Method:** `GET`
- **Endpoint:** `/api/v1/root/subscription-plans/dropdown`
- **Purpose:** Provides a minimal list of active plans (ID and Name) for selection.
- **Use Case:** Selecting a plan during tenant onboarding or subscription renewal.
- **Used In:** CEO Dashboard -> Purchase Subscription, Seravion Admin -> Manual Plan Assignment
- **Access / Security:** `hasRole('CEO')`

---

## Module Enums

### SubscriptionStatus
- **Used In:** [CreateSubscriptionPlanRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/subscriptionPlan/dto/request/CreateSubscriptionPlanRequest.java#10-45), [SubscriptionPlanResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/subscriptionPlan/dto/response/SubscriptionPlanResponse.java#10-25), [SubscriptionPlan](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/subscriptionPlan/controller/SubscriptionPlanController.java#39-48) Entity
- **Values:**
  - `ACTIVE`: Plan is available for new subscriptions.
  - `INACTIVE`: Plan is discontinued.

### DurationType
- **Used In:** [CreateSubscriptionPlanRequest](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/subscriptionPlan/dto/request/CreateSubscriptionPlanRequest.java#10-45), [SubscriptionPlanResponse](file:///k:/JAVA%20FULL%20STACK%20DEVELOPER/RBAC/rbac/src/main/java/com/security/rbac/modules/subscriptionPlan/dto/response/SubscriptionPlanResponse.java#10-25)
- **Values:**
  - `MONTHLY`: 1-month duration.
  - `QUARTERLY`: 3-month duration.
  - `ANNUAL`: 12-month duration.
  - `CUSTOM`: Flexible duration defined by admin.

---

## Notes for Frontend

- **Dropdown APIs:** Use `/api/v1/root/subscription-plans/dropdown` for plan selection fields in both Admin and CEO flows.
- **List APIs:** `/api/v1/root/subscription-plans/get-all` provides the main table data along with filtered counts (Active/Inactive).
- **Detail APIs:** Use `/api/v1/root/subscription-plans?id={id}` for fetching full details in View/Edit modes.
- **Create/Update/Delete:** Standard CRUD follows REST conventions but note that `DELETE` is an inactivation (soft-delete).
- **Enum Usage:** 
  - Use `DurationType` for billing cycle selections.
  - Use `SubscriptionStatus` to toggle plan availability in the UI.
