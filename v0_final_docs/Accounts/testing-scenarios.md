# Testing Scenarios — Finance & Accounts Complete Guide

> **Related:** [Bill Management](./bill-management.md) · [Invoicing](./invoicing.md) · [Payments](./payments.md) · [GST Workflow](./gst-hsn-tax-workflow.md) · [Chart of Accounts](./chart-of-accounts.md) · [Ledger Management](./ledger-management.md)

This document provides **in-depth, end-to-end testing scenarios** for every Finance & Accounts module. Each scenario includes: pre-requisites, exact steps, expected ledger entries, and failure/edge cases.

---

## Part A: Chart of Accounts (COA)

### A-1: Create a Postable Asset Head

**Goal:** Verify a new postable head can be created and appears in the Ledger dropdown.

**Pre-requisites:** User has COA Add permission.

**Steps:**
1. Go to Finance & Accounts → Chart of Accounts → + Add Account Head.
2. Enter Name: "Advances to Staff", Primary Group: **Asset**, Nature: **Debit**, Code: `1900-ADV-STAFF`, Postable: ✅ Yes, Status: Active.
3. Click Save.

**Expected:** Row appears in COA list. When creating a Ledger, `1900-ADV-STAFF` appears in the Account Group dropdown.

**Failure case:** If Code already exists → system shows "Duplicate code" error.

---

### A-2: Inactivate a Head in Use

**Goal:** Verify inactivating a head with ledgers drops it from the new-ledger dropdown but does not affect existing ledgers.

**Steps:**
1. Edit `1000-SD` (Sundry Debtors) → set Status: Inactive → Save.
2. Try to create a new ledger → Account Group dropdown should NOT show `1000-SD`.
3. Go to an existing customer ledger (e.g., Acme Pvt Ltd) → confirm it still exists and its statement still loads.

**Expected:** Existing ledgers are unaffected. New-ledger dropdown hides the inactive head.

---

## Part B: Ledger Management

### B-1: Auto-Created Customer Ledger

**Goal:** Verify that activating a customer creates their ledger automatically.

**Steps:**
1. Go to Customers → Add Customer → fill fields → Save as **Active**.
2. Go to Finance & Accounts → Ledger Management.
3. Search for the new customer name.

**Expected:** A CUSTOMER-type ledger exists under Sundry Debtors, status Active, opening balance = ₹0.

---

### B-2: Opening Balance Sync

**Goal:** Verify that updating a ledger's opening balance syncs to LedgerEntry.

**Steps:**
1. Go to an existing ledger (e.g., "HDFC Current Account").
2. Click Edit → set Opening Balance: ₹50,000 (Debit).
3. Save.
4. Open the Ledger Statement for that ledger.

**Expected:** The statement shows an opening balance entry of ₹50,000 Dr. The books remain balanced (a contra entry is created internally to maintain the trial balance).

**Failure case:** If you enter ₹50,000 but select the wrong Dr/Cr side, the trial balance will show the opposite sign. Always verify after save.

---

### B-3: Ledger Statement Date Filter

**Steps:**
1. Open any customer ledger statement.
2. Set From: 1st April of current year, To: today.
3. Verify the opening balance for that range is shown.
4. Narrow to a single month.

**Expected:** Running balance updates correctly. Entries outside the date range are excluded but the opening balance reflects cumulative pre-range entries.

---

## Part C: Invoicing (Sales)

### C-1: B2B Tax Invoice — Same State (CGST + SGST)

**Pre-requisites:**
- Issuing branch has GSTIN (e.g., Karnataka).
- Customer has GSTIN, customer state = Karnataka.
- Active customer ledger exists.

**Steps:**
1. Invoicing → Create Invoice → Direct Invoice.
2. Branch: Karnataka branch, Customer: Acme Pvt Ltd (Karnataka).
3. Add line: Service "Pest Control", HSN 998591, Rate ₹10,000, Tax 18%.
4. Verify: CGST = ₹900, SGST = ₹900, Grand Total = ₹11,800.
5. Click Approve & Send.

**Expected Ledger Entries:**
| Account | DR | CR |
|---------|----|----|
| Acme Pvt Ltd (Customer) | ₹11,800 | |
| Sales Income | | ₹10,000 |
| GST Output CGST | | ₹900 |
| GST Output SGST | | ₹900 |

Invoice status → Sent. Customer pending = ₹11,800.

---

### C-2: B2B Tax Invoice — Different States (IGST)

**Pre-requisites:** Branch state = Karnataka, Customer state = Maharashtra.

**Steps:** Same as C-1 but customer is in Maharashtra.

**Expected Ledger Entries:**
| Account | DR | CR |
|---------|----|----|
| Customer Ledger | ₹11,800 | |
| Sales Income | | ₹10,000 |
| GST Output IGST | | ₹1,800 |

CGST and SGST must be ₹0.

---

### C-3: B2C Invoice — Unregistered Customer

**Pre-requisites:** Branch has GSTIN. Customer has NO GSTIN.

**Steps:**
1. Create Invoice → pick unregistered customer.
2. Add a delivery site with state = Tamil Nadu (different from branch Karnataka).
3. Add service line ₹10,000, 18% GST.

**Expected:** System uses the **Billing/Delivery Site state** (Tamil Nadu) as Place of Supply → determines inter-state → IGST = ₹1,800. Invoice can be approved without error (no GSTIN required for customer in B2C).

**Failure case (old behaviour):** System would throw "No active GST registration for customer in Karnataka." — this was a bug that was fixed by using site state for POS.

---

### C-4: Unregistered Branch — Must Use Proforma

**Pre-requisites:** Branch GSTIN is blank.

**Steps:**
1. Create Invoice → Invoice Type = **Tax Invoice**.
2. Click Approve & Send.

**Expected:** Error: `"Branch has no GSTIN. Use 'Proforma' or 'Bill of Supply' instead of Tax Invoice."`

**Fix:** Change Invoice Type to **Proforma** → Approve succeeds.

---

### C-5: Auto-Draft from Product Sales Order

**Steps:**
1. Create a Product Sale → SO type Product → Save as **Open** (not Draft).
2. Go to Invoicing list.

**Expected:** A Draft invoice with `FROM_SO` mode appears automatically. Finance can Approve & Send it directly.

---

### C-6: Duplicate SO + Month (Conflict)

**Steps:**
1. Create and Approve an invoice From SO for "Contract SO #1001" in October.
2. Create another invoice From SO for the same SO in October.

**Expected:** System warns "Same SO + same month already has an invoice." User must explicitly **acknowledge the duplicate** to save.

---

### C-7: Credit Note — Full Write-Off

**Steps:**
1. Find a Sent invoice for ₹11,800.
2. Issue Credit Note → Amount ₹11,800 → Reason: Full Cancellation.

**Expected Ledger Entries:**
| Account | DR | CR |
|---------|----|----|
| Sales Adjustment | ₹10,000 | |
| GST Output CGST | ₹900 | |
| GST Output SGST | ₹900 | |
| Customer Ledger | | ₹11,800 |

Invoice status → **Adjusted** (pending = 0, received = 0).

---

### C-8: Settle & Close Receipt

**Steps:**
1. Find a Sent invoice for ₹11,800.
2. Record Payment → Receipt ₹10,000 (shortfall ₹1,800).
3. Select **Settle & Close** → give reason.

**Expected:** Invoice → **Adjusted**. Auto credit note of ₹1,800 is issued. Customer ledger shows ₹10,000 cash received + ₹1,800 CN adjustment.

---

## Part D: Bill Management (Accounts Payable)

### D-1: Registered Vendor Bill — Intra-State

**Pre-requisites:**
- Vendor "ChemCo" is GST-registered, Karnataka.
- Branch is Karnataka.
- Vendor ledger exists and is Active.

**Steps:**
1. Finance & Accounts → Bills → Add Bill.
2. Branch: Karnataka, Vendor: ChemCo, Bill Date: today, Credit: 30 days.
3. Add line: Product "Chemicals", HSN 380891, Qty 10, Rate ₹1,000, Discount 0%, Tax 18%.
4. Verify: Taxable = ₹10,000, CGST = ₹900, SGST = ₹900, Net Payable = ₹11,800.
5. Save → DRAFT.
6. Click **Confirm**.

**Expected Ledger Entries (on Confirm):**
| Account | DR | CR |
|---------|----|----|
| Purchase Expense | ₹10,000 | |
| GST Input CGST | ₹900 | |
| GST Input SGST | ₹900 | |
| ChemCo Vendor Ledger | | ₹11,800 |

Status → PENDING. Pending = ₹11,800.

---

### D-2: Unregistered Vendor (URD) Bill

**Pre-requisites:** Vendor "Local Supplier" is **Unregistered** in vendor master.

**Steps:**
1. Add Bill → Vendor: Local Supplier.
2. Notice: GST tax fields are **disabled** in the UI (greyed out).
3. Add line: Product, Rate ₹5,000, Tax shows **0%** (cannot be changed).
4. Net Payable = ₹5,000.
5. Confirm.

**Expected Ledger Entries:**
| Account | DR | CR |
|---------|----|----|
| Purchase Expense | ₹5,000 | |
| Local Supplier Vendor Ledger | | ₹5,000 |

**Failure case (what happens if you try to bypass via API):**
`POST /api/v1/bills` with CGST = ₹450 for an unregistered vendor → `400: "Unregistered vendor bills cannot include GST (no input tax credit). Set CGST/SGST/IGST to zero."`

---

### D-3: PO-Linked Bill

**Pre-requisites:** A Purchase Order for ChemCo exists with status "RECEIVED".

**Steps:**
1. Add Bill → Select Purchase Order from dropdown.
2. Verify: Lines are auto-populated from PO items.
3. Verify: Vendor is auto-filled and locked to PO vendor.
4. Confirm the bill.

**Expected:** `purchase-orders-in-use` API now includes this PO — it will be hidden from the PO picker for other bills.

**Failure case:** Try creating a second bill for the same PO → `409: "A bill already exists for this purchase order."`

---

### D-4: Bill with TDS (Section 194C)

**Steps:**
1. Add Bill → Vendor: Contractor, TDS Applicable: ✅ Yes.
2. TDS Section: 194C, TDS Rate: 1%.
3. Line: Rate ₹10,000, Tax 18%, Line Total ₹11,800.
4. TDS = ₹118 (1% of ₹11,800). Net Payable = ₹11,682.
5. Confirm.

**Expected Ledger Entries:**
| Account | DR | CR |
|---------|----|----|
| Purchase Expense | ₹10,000 | |
| GST Input CGST | ₹900 | |
| GST Input SGST | ₹900 | |
| Contractor Vendor Ledger | | ₹11,682 |
| TDS Payable | | ₹118 |

---

### D-5: Debit Note — Partial Return

**Steps:**
1. Find a PENDING bill for ₹11,800.
2. Issue Debit Note → Amount ₹1,000, Reason: Purchase Return.

**Expected:** Bill status → PARTIAL. Pending = ₹10,800.

**Ledger entries (prorated):**
| Account | DR | CR |
|---------|----|----|
| Vendor Ledger | ₹1,000 | |
| Purchase Adjustment | | ~₹847 |
| GST Input CGST | | ~₹76 |
| GST Input SGST | | ~₹76 |

---

### D-6: Mark Overdue

**Steps:**
1. Find a PENDING bill where Due Date < today.
2. Click **Mark Overdue**.

**Expected:** Bill status → OVERDUE. No ledger entries are created (status change only).

---

## Part E: Payments Module

### E-1: Full Receipt Against Invoice

**Pre-requisites:** Invoice C-1 is Sent (pending ₹11,800).

**Steps:**
1. Payments → + Add Receipt.
2. Customer: Acme, Bank Book: HDFC Current, Mode: Bank Transfer, UTR: UTR123, Amount: ₹11,800.
3. Tick invoice C-1 → Allocate ₹11,800.
4. Save.

**Expected Ledger Entries:**
| Account | DR | CR |
|---------|----|----|
| HDFC Current (Bank) | ₹11,800 | |
| Acme Pvt Ltd (Customer) | | ₹11,800 |

Invoice status → **PAID**. Pending = ₹0. Received = ₹11,800.

---

### E-2: Partial Receipt — Keep Open

**Steps:**
1. Invoice pending ₹11,800.
2. Receipt ₹5,000 → Allocate ₹5,000 to invoice → **Keep Open**.

**Expected:** Invoice → PARTIAL. Pending = ₹6,800. Received = ₹5,000.

---

### E-3: Vendor Payment — Full

**Steps:**
1. Bill D-1 PENDING (₹11,800).
2. Payments → + Payment → Vendor: ChemCo, Amount ₹11,800 → allocate to bill.

**Expected Ledger Entries:**
| Account | DR | CR |
|---------|----|----|
| ChemCo Vendor Ledger | ₹11,800 | |
| HDFC Current (Bank) | | ₹11,800 |

Bill → PAID.

---

### E-4: Vendor Payment with TDS

**Steps:**
1. Bill D-4 PENDING (₹11,682).
2. Payment → TDS Yes, Section 194C, Rate 1%, TDS = ₹118, Paid: ₹11,682.

**Expected Ledger Entries:**
| Account | DR | CR |
|---------|----|----|
| Contractor Vendor Ledger | ₹11,800 | |
| Bank | | ₹11,682 |
| TDS Payable | | ₹118 |

---

### E-5: Contra — Cash to Bank

**Steps:**
1. Payments → + Contra → From: Cash in Hand, To: HDFC Current, Amount ₹10,000.

**Expected:**
| Account | DR | CR |
|---------|----|----|
| HDFC Current | ₹10,000 | |
| Cash in Hand | | ₹10,000 |

---

### E-6: Journal Correction

**Steps:**
1. Payments → + Journal → Narration: "Correct rent head".
2. Debit: Rent Expense ledger ₹5,000, Credit: Telephone Expense ₹5,000.

**Expected:** Both ledgers updated. No cash moves. Books remain balanced.

---

### E-7: Void a Voucher

**Steps:**
1. Find a Posted Receipt → click Void → confirm.

**Expected:** Status = Void. **Books are NOT reversed.** Invoice pending does NOT increase back. Treat as a label only — reverse manually via Journal if needed.

---

## Part F: GST Scenarios

### F-1: HSN Rate Mismatch Error

**Steps:**
1. Create HSN 998591 with CGST 9%, SGST 9% but IGST = 15% (wrong — should be 18%).
2. Try to Save as Active.

**Expected:** Error: "CGST + SGST must equal IGST."

---

### F-2: URD Purchase — Attempted API Bypass

**Steps:**
1. Use API directly: `POST /api/v1/bills` with vendorId of an UNREGISTERED vendor, cgstAmount = ₹500.

**Expected:** `400: "Unregistered vendor bills cannot include GST (no input tax credit). Set CGST/SGST/IGST to zero."`

---

## Part G: Internal Accounts

### G-1: Opening Balance Update Sync

**Steps:**
1. Ledger Management → Edit an Asset ledger.
2. Set Opening Balance: ₹1,00,000 (Debit).
3. Save.
4. View the Ledger Statement.

**Expected:** Opening entry of ₹1,00,000 Dr appears. Trial balance remains balanced (system posts matching contra entry automatically).

---

### G-2: New Seeded Internal Heads Visible in COA

**Steps:**
1. Go to Chart of Accounts.
2. Search for "Imprest" or "Petty Cash" or "Salary Payable".

**Expected:** New seeded heads from the Indian GL migration script appear with correct Primary Group (Liability for Salary Payable, Asset for Imprest Fund, Expense for Petty Cash Expenses).

---

## Summary of Key Failure Cases

| Scenario | Error | What to check |
|----------|-------|---------------|
| Approve invoice — no customer ledger | 400: Ledger not found | Create customer ledger or activate customer |
| Approve invoice — unregistered branch | 400: Branch has no GSTIN | Use Proforma type or set branch GSTIN |
| Confirm bill — URD vendor has GST | 400: Cannot include GST | Clear tax on all lines |
| Confirm bill — no vendor ledger | 400: Vendor ledger not found | Activate vendor (auto-creates ledger) |
| Create bill — PO already billed | 409: Bill exists for PO | Cancel existing bill first |
| Receipt — UTR missing for Bank Transfer | Validation error | Add UTR/reference |
| Contra — same from/to ledger | 400 | Choose different ledgers |
| Credit note > invoice pending | 400: Exceeds pending | Reduce credit amount |
| Debit note > bill pending | 400: Exceeds pending | Reduce debit amount |
