# Modules 28 → 32 Finance — Unified Conditional Flow + Screen-wise API Guide (Easy English + Gujlish + Gujarati)

This document **unifies**:
- `docs/module-28-invoicing-frontend-screenwise.md`
- `docs/module-28-32-finance-frontend-screenwise.md`

And adds:
- **Conditional UI flow** (when to show/hide buttons)
- **Dependencies & prerequisites** (COA → Ledgers → postings)
- **Exact enums / allowed values** used by backend
- **Complex flows explained** in **Easy English + Gujlish + Gujarati**

---

## 0) Global integration rules

- **Base**: all finance APIs are under `/api/v1/*`
- **Auth**: `Authorization: Bearer <token>`
- **Tenant**: send your tenant header (commonly `X-Tenant-ID`) as per backend security config
- **Response wrapper**: JSON responses are `{ status, message, data }`
- **Pagination wrapper**: list APIs return `data = { count, next, prev, data: [] }`
- **Binary**: invoice/bill PDFs return bytes (not JSON):
  - `GET /api/v1/invoices/pdf?id=...`
  - `GET /api/v1/bills/pdf?id=...`

---

## 1) Full finance flow (ASCII, end-to-end)

```
(32) COA Heads  --->  (31) Ledgers  --->  (28) Invoices  ---> (30) Receipt Vouchers ---> (31) Ledger Reports
                             |                 |
                             |                 '-- Credit Note (manual / auto from SETTLE_CLOSE)
                             |
                             '-- (29) Bills  ---> (30) Payment Vouchers ---> (31) Ledger Reports
                                                |
                                                '-- Debit Note (manual / auto from SETTLE_CLOSE)
```

---

## 2) Dependencies & prerequisites (must be ready before real testing)

### 2.1 COA (Module 32) prerequisites

You need COA heads (postable) before creating ledgers.

APIs:
- `POST /api/v1/coa/heads`
- `GET /api/v1/coa/dropdowns/postable-heads`

### 2.2 Ledger (Module 31) prerequisites

Before you do **approve/confirm/receipt/payment**, ensure these exist in the tenant:

- **Customer ledger**: `ledgerType = CUSTOMER`, `linkedCustomerId = <customerId>`, `status = ACTIVE`
- **Vendor ledger**: `ledgerType = VENDOR`, `linkedVendorId = <vendorId>`, `status = ACTIVE`
- **Bank/Cash ledger**: used by vouchers via `bankLedgerId`
- **System ledgers by code** (hard dependency in services):
  - `SALES_INCOME` (invoice approve/send)
  - `SALES_ADJUSTMENT` (credit note posting)
  - `PURCHASE_EXPENSE` (bill confirm)
  - `PURCHASE_ADJUSTMENT` (debit note posting)

APIs:
- `POST /api/v1/ledgers`
- `GET /api/v1/ledgers?search=&ledgerType=&status=&pageNo=&pageSize=`
- `GET /api/v1/ledgers/by-id?id=...`

---

## 3) Enums / allowed values (backend source-of-truth)

### 3.1 Invoice status (Module 28)

`InvoiceStatus`:
- `DRAFT`, `SENT`, `PARTIAL`, `PAID`, `OVERDUE`, `CANCELLED`

**Important**:
- Backend currently has **no cancel API**; `CANCELLED` is reserved / future.

### 3.2 Bill status (Module 29)

`BillStatus`:
- `DRAFT`, `PENDING`, `PARTIAL`, `PAID`, `OVERDUE`, `CANCELLED`

**Important**:
- Backend currently has **no cancel API** for bills; `CANCELLED` is reserved / future.

### 3.3 Ledger enums (Module 31)

`LedgerType`:
- `CUSTOMER`, `VENDOR`, `BANK`, `CASH`, `INTERNAL`, `TAX`

`LedgerStatus`:
- `ACTIVE`, `INACTIVE`

### 3.4 Voucher values (Module 30)

Stored as strings on entity/DTO:
- `voucherType`: `RECEIPT` or `PAYMENT`
- `status`: `POSTED` or `VOID`
- `partyType`: `CUSTOMER` (receipt) or `VENDOR` (payment)

Allocation request field:
- `settlementAction`: *(blank/anything)* = keep open, OR `"SETTLE_CLOSE"`

**Note (RBAC / Permission)**:
- Current controller requires `PAYMENT_MANAGEMENT_ADD` for both:
  - `POST /api/v1/vouchers/receipts`
  - `POST /api/v1/vouchers/payments`

---

## 4) Conditional UI rules (screen-wise)

### 4.1 Invoice buttons (Module 28)

**Invoice row actions should be enabled only when condition matches:**

- **Edit**: only if `status == DRAFT`
- **Delete**: only if `status == DRAFT`
- **Approve & Send**: only if `status == DRAFT`
- **Record Payment**: only if `status in {SENT, PARTIAL, OVERDUE}` AND `pendingAmount > 0`
- **Issue Credit Note**: only if `status in {SENT, PARTIAL, OVERDUE}` AND `pendingAmount > 0`
- **Download PDF**: only if `status in {SENT, PAID, PARTIAL, OVERDUE}` *(backend allows; UI may restrict to Sent/Paid per product spec)*

### 4.2 Bill buttons (Module 29)

- **Edit/Delete/Confirm**: only if `status == DRAFT`
- **Pay Bill**: only if `status in {PENDING, PARTIAL, OVERDUE}` AND `pendingAmount > 0`
- **Issue Debit Note**: only if `pendingAmount > 0` (and bill exists)

### 4.3 Voucher buttons (Module 30)

- **Void**: only if `status == POSTED`
- **Important limitation**: backend void is **status-only**; it does **not** reverse invoice/bill pending or ledger postings.

---

## 5) Screen-wise API mapping (what FE calls)

### 5.1 Module 28 — Invoices

**Dashboard**
- List: `GET /api/v1/invoices?...`
- Summary: `GET /api/v1/invoices/summary?branchId=`

**Create**
- Save draft: `POST /api/v1/invoices`
- Approve & send: `POST /api/v1/invoices/approve-send?id=...`

**View**
- Detail: `GET /api/v1/invoices/by-id?id=...`
- PDF: `GET /api/v1/invoices/pdf?id=...` *(placeholder bytes in controller)*
- Attachment: `GET /api/v1/invoices/attachment?id=...`

**Edit**
- Update draft: `PUT /api/v1/invoices/update?id=...`

**Credit note**
- Issue: `POST /api/v1/invoices/credit-notes?invoiceId=...`
- List: `GET /api/v1/invoices/credit-notes?invoiceId=...`
- By id: `GET /api/v1/invoices/credit-notes/by-id?id=...` OR `GET /api/v1/credit-notes/by-id?id=...`

### 5.2 Module 29 — Bills

**Dashboard**
- List: `GET /api/v1/bills?...`
- Summary: `GET /api/v1/bills/summary`

**Create/Edit/View**
- Create: `POST /api/v1/bills`
- Update: `PUT /api/v1/bills/update?id=...`
- By id: `GET /api/v1/bills/by-id?id=...`
- Delete (draft): `DELETE /api/v1/bills/delete?id=...`

**Confirm**
- `POST /api/v1/bills/confirm?id=...` (status -> `PENDING`, posts ledger)

**Debit note**
- Issue: `POST /api/v1/bills/debit-notes?billId=...`
- List: `GET /api/v1/bills/debit-notes?billId=...`
- By id: `GET /api/v1/bills/debit-notes/by-id?id=...`

### 5.3 Module 30 — Vouchers

- Receipt create: `POST /api/v1/vouchers/receipts`
- Payment create: `POST /api/v1/vouchers/payments`
- Register list: `GET /api/v1/vouchers?...`
- By id: `GET /api/v1/vouchers/by-id?id=...`
- Allocations: `GET /api/v1/vouchers/allocations?voucherId=...`
- Summary: `GET /api/v1/vouchers/summary`
- Void: `POST /api/v1/vouchers/void?id=...`

### 5.4 Module 31 — Ledger

- Create: `POST /api/v1/ledgers`
- Update: `PUT /api/v1/ledgers/update?id=...`
- List: `GET /api/v1/ledgers?...`
- Statement: `GET /api/v1/ledgers/statement?ledgerId=...&from=...&to=...`
- Summary: `GET /api/v1/ledgers/summary`
- Ageing: `GET /api/v1/ledgers/ageing`

### 5.5 Module 32 — COA

- Create head: `POST /api/v1/coa/heads`
- Update head: `PUT /api/v1/coa/heads/update?id=...`
- List heads: `GET /api/v1/coa/heads?...`
- Postable dropdown: `GET /api/v1/coa/dropdowns/postable-heads`
- Used-by ledgers: `GET /api/v1/coa/heads/used-by-ledgers?headId=...`

---

## 6) Complex flows explained (Easy English + Gujlish + Gujarati)

### 6.1 Invoice: Draft → Approve & Send (ledger posting)

**Easy English**
- Draft invoice means “saved but not posted”.
- When you click **Approve & Send**, invoice becomes `SENT` and ledger entries are posted:
  - Customer ledger **DR**
  - `SALES_INCOME` ledger **CR**

**Gujlish**
- Draft invoice etle “save thayu che pan accounting ma entry nathi padi”.
- **Approve & Send** dabavo to status `SENT` thai jase ane ledger ma entry padse:
  - Customer ledger DR
  - `SALES_INCOME` ledger CR

**Gujarati (શુદ્ધ ગુજરાતી)**
- ડ્રાફ્ટ ઇન્વૉઇસ એટલે “સેવ થઈ ગયું છે, પણ ખાતાવહીમાં નોંધાઈ નથી”.
- **Approve & Send** કરવાથી સ્થિતિ `SENT` થાય છે અને ખાતાવહીમાં નોંધ થાય છે:
  - ગ્રાહક લેજર (ડેબિટ)
  - `SALES_INCOME` લેજર (ક્રેડિટ)

### 6.2 Receipt voucher allocation: KEEP_OPEN vs SETTLE_CLOSE

**Easy English**
- When receiving money, you create a **receipt voucher** with allocations to invoice(s).
- If allocated amount < pending:
  - If `settlementAction` is empty (keep open) → invoice becomes `PARTIAL`, pending remains.
  - If `settlementAction = SETTLE_CLOSE` → backend auto-creates a **Credit Note** for the shortfall and invoice becomes paid (pending becomes 0).
  - Backend sets credit note reason as `PAYMENT_SETTLEMENT`.

**Gujlish**
- Receipt voucher banavo ane invoice par allocation karo.
- Pending karta ochu allocate thayu to:
  - `settlementAction` blank hoy to invoice `PARTIAL` rehse.
  - `SETTLE_CLOSE` hoy to baki amount mate system **Credit Note** banavi dese ane invoice close (pending 0).
  - CN reason backend ma `PAYMENT_SETTLEMENT` fix che.

**Gujarati (શુદ્ધ ગુજરાતી)**
- રસીદ વાઉચર બનાવીને ઇન્વૉઇસ સામે એલોકેશન કરો.
- જો એલોકેશન પેન્ડિંગ કરતાં ઓછું હોય તો:
  - `settlementAction` ખાલી હોય તો ઇન્વૉઇસ `PARTIAL` રહેશે.
  - `SETTLE_CLOSE` હોય તો બાકી રકમ માટે સિસ્ટમ **ક્રેડિટ નોટ** બનાવે છે અને ઇન્વૉઇસ બંધ થાય છે (પેન્ડિંગ 0).
  - ક્રેડિટ નોટનું કારણ બેકએન્ડમાં `PAYMENT_SETTLEMENT` રહે છે.

### 6.3 Payment voucher allocation (Bills): KEEP_OPEN vs SETTLE_CLOSE

**Easy English**
- Vendor payment voucher can allocate to bill(s).
- If allocated amount < bill pending:
  - keep open → bill `PARTIAL`
  - `SETTLE_CLOSE` → backend auto-creates **Debit Note** for shortfall and bill closes

**Gujlish**
- Vendor payment voucher thi bill allocate karo.
- Pending karta ochu allocate hoy to:
  - keep open → bill `PARTIAL`
  - `SETTLE_CLOSE` → system **Debit Note** banavi ne bill close kare

**Gujarati (શુદ્ધ ગુજરાતી)**
- વેન્ડર પેમેન્ટ વાઉચરથી બિલ સામે એલોકેશન થાય છે.
- જો એલોકેશન પેન્ડિંગ કરતાં ઓછું હોય તો:
  - keep open → બિલ `PARTIAL`
  - `SETTLE_CLOSE` → બાકી રકમ માટે સિસ્ટમ **ડેબિટ નોટ** બનાવે છે અને બિલ બંધ થાય છે

### 6.4 Overdue marking (Invoices/Bills)

**Easy English**
- Overdue is calculated by backend job endpoints:
  - `POST /api/v1/invoices/mark-overdue`
  - `POST /api/v1/bills/mark-overdue`
- It marks `SENT/PARTIAL` invoices or `PENDING/PARTIAL` bills as `OVERDUE` if `dueDate < today` and pending > 0.

**Gujlish**
- Overdue automatic endpoint thi thai che (manual trigger pan kari sako).
- Due date pass thai gayu ane pending baki hoy to status `OVERDUE`.

**Gujarati (શુદ્ધ ગુજરાતી)**
- Due date પસાર થઈ જાય અને પેન્ડિંગ બાકી હોય તો સિસ્ટમ સ્થિતિ `OVERDUE` કરે છે.

### 6.5 Voucher void (WARNING)

**Easy English**
- Void only changes voucher status to `VOID`.
- It does **not** reverse ledger entries or undo invoice/bill updates in current implementation.

**Gujlish**
- Void etle khali status `VOID` thai jase.
- Invoice/bill/ledger ni entry reverse nahi thay (current code ma).

**Gujarati (શુદ્ધ ગુજરાતી)**
- વાઉચર void કરવાથી ફક્ત તેની સ્થિતિ `VOID` થાય છે.
- હાલના અમલીકરણમાં લેજર એન્ટ્રી રિવર્સ થતી નથી અને ઇન્વૉઇસ/બિલના બદલાવ પાછા ફેરવાતા નથી.

---

## 7) Known missing items (backend vs expected product buttons)

- **Invoice send/resend** (email/WhatsApp): not implemented.
- **Cancel invoice / cancel bill**: no endpoints implemented.
- **PDFs** are placeholders in controllers (not real PDFs yet).
- **Exports** (Tally/COA export) are placeholders.

---

## 8) One-by-one validation flow (start from Module 28 and validate till Module 32 screens)

This is the recommended **step sequence** for FE integration + QA so you can validate the complete finance chain.

### 8.1 Setup (Module 32 → Module 31) — do once per tenant

**Step A — COA Heads (Module 32 screens)**
- Create required COA heads:
  - Sales income head (for invoices)
  - Purchase/expense head (for bills)
  - Adjustment heads (sales adjustment, purchase adjustment)
  - Bank/Cash head
- API:
  - `POST /api/v1/coa/heads`
- Validation:
  - `GET /api/v1/coa/heads?pageNo=0&pageSize=10&search=...`
  - Ensure heads are `ACTIVE` and `isPostable = true` (where used).

**Step B — Ledgers (Module 31 screens)**
- Create ledgers mapped to those COA heads:
  - Customer ledger (`ledgerType=CUSTOMER`, `linkedCustomerId=...`, `status=ACTIVE`)
  - Vendor ledger (`ledgerType=VENDOR`, `linkedVendorId=...`, `status=ACTIVE`)
  - Bank/Cash ledger (`ledgerType=BANK` or `CASH`, `status=ACTIVE`)
  - Internal/system ledgers by code (must exist):
    - `SALES_INCOME`
    - `SALES_ADJUSTMENT`
    - `PURCHASE_EXPENSE`
    - `PURCHASE_ADJUSTMENT`
- API:
  - `POST /api/v1/ledgers`
- Validation:
  - `GET /api/v1/ledgers?search=SALES_INCOME&pageNo=0&pageSize=10`
  - Ensure `status=ACTIVE` (inactive ledgers will block postings).

### 8.2 Sales side validation (Module 28 → 30 → 31)

**Step 1 — Module 28.2 Create Invoice (Draft)**
- Create a draft invoice.
- API:
  - `POST /api/v1/invoices`
- Validate draft:
  - `GET /api/v1/invoices/by-id?id=INV...` → must be `status=DRAFT`

**Step 2 — Module 28.4 Edit Invoice (Optional)**
- Update draft fields/lines if required.
- API:
  - `PUT /api/v1/invoices/update?id=INV...`
- Validate:
  - `GET /api/v1/invoices/by-id?id=INV...` reflects updates

**Step 3 — Module 28 Approve & Send**
- Approve the draft invoice (posting happens here).
- API:
  - `POST /api/v1/invoices/approve-send?id=INV...`
- Validate:
  - Invoice becomes `status=SENT`
  - Ledger postings exist (see Step 6)

**Step 4 — Module 28.1 Dashboard validation**
- Verify invoice appears in list and summary cards move.
- APIs:
  - `GET /api/v1/invoices?...`
  - `GET /api/v1/invoices/summary`

**Step 5 — Module 30 Receipt Voucher (Record Payment for Invoice)**
- From invoice detail (28.3) click “Record Payment” → open receipt screen.
- API (choose one scenario):
  - **Exact / full receipt**:
    - `POST /api/v1/vouchers/receipts` with allocations where `allocateAmount == invoice.pendingAmount`
    - Expected: invoice status `PAID`
  - **Partial receipt (KEEP_OPEN)**:
    - `POST /api/v1/vouchers/receipts` with allocations where `allocateAmount < pending`, and `settlementAction` blank
    - Expected: invoice status `PARTIAL`, pending remains
  - **Partial receipt + SETTLE_CLOSE**
    - `POST /api/v1/vouchers/receipts` with `allocateAmount < pending` and `settlementAction="SETTLE_CLOSE"`
    - Expected: system creates **Credit Note** for shortfall, invoice becomes `PAID`
- Validate:
  - `GET /api/v1/vouchers/by-id?id=VCH...`
  - `GET /api/v1/vouchers/allocations?voucherId=VCH...`
  - `GET /api/v1/invoices/by-id?id=INV...` (pending/status updated)
  - If SETTLE_CLOSE used: `GET /api/v1/invoices/credit-notes?invoiceId=INV...`

**Step 6 — Module 31 Ledger verification (Sales path)**
- Open ledger screens and validate statements:
  - Customer ledger statement should show:
    - Invoice approve entry (INVOICE)
    - Receipt entry (RECEIPT)
    - Credit note entry (CREDIT_NOTE) if SETTLE_CLOSE / manual CN used
- APIs:
  - `GET /api/v1/ledgers/statement?ledgerId=...&from=YYYY-MM-DD&to=YYYY-MM-DD`
  - `GET /api/v1/ledgers/summary`

### 8.3 Purchase side validation (Module 29 → 30 → 31) (optional but recommended)

**Step 7 — Module 29 Create Bill (Draft)**
- API:
  - `POST /api/v1/bills`
- Validate: `GET /api/v1/bills/by-id?id=BILL...` → `status=DRAFT`

**Step 8 — Module 29 Confirm Bill**
- API:
  - `POST /api/v1/bills/confirm?id=BILL...`
- Validate:
  - Bill becomes `status=PENDING`
  - Ledger postings exist (expense + vendor)

**Step 9 — Module 30 Payment Voucher (Pay vendor bill)**
- API (choose scenario):
  - Full pay → bill `PAID`
  - Partial keep open → bill `PARTIAL`
  - Partial `SETTLE_CLOSE` → system creates **Debit Note**, bill closes
- Validate:
  - `GET /api/v1/vouchers/allocations?voucherId=...`
  - `GET /api/v1/bills/by-id?id=BILL...`
  - If SETTLE_CLOSE used: `GET /api/v1/bills/debit-notes?billId=BILL...`

**Step 10 — Module 31 Ledger verification (Purchase path)**
- Vendor ledger statement should show:
  - Bill confirmed (BILL)
  - Payment (PAYMENT)
  - Debit note (DEBIT_NOTE) if used


