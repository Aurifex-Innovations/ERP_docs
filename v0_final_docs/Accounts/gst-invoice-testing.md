# GST invoice testing — one script

> **Related:** [Invoicing](./invoicing.md) · [Testing scenarios](./testing-scenarios.md) · [Ledger management](./ledger-management.md)

Run this once after the backend has been restarted and the branch GST-scheme migration has applied. It checks which document the branch may save, how place of supply picks CGST/SGST or IGST, and which ledgers move.

Use one tenant. Do not mix these invoices with old proformas that were posted before this rule. Those old rows stay as they are.

---

## 1. Set up three branches and one customer

| Record | What to set | Why |
|--------|-------------|-----|
| Branch **BLR** | State Karnataka, GSTIN filled, GST scheme **Regular** | Can issue a Tax Invoice |
| Branch **COMP** | State Karnataka, GSTIN filled, GST scheme **Composition** | Can issue only Bill of Supply or Proforma |
| Branch **PUN** | State Maharashtra, GSTIN **blank** | Scheme is forced to Unregistered |
| Customer **Acme** | Active, so a customer ledger exists. No GSTIN on the customer master | B2C unless a GST card matches the site |
| GST card **KA** | On Acme, state Karnataka, a Karnataka GSTIN | Matches a Karnataka site only |
| GST card **MH** | On Acme, state Maharashtra, a Maharashtra GSTIN | Matches a Pune site only |
| Site **Pune** | On a sales order for Acme and branch BLR, state Maharashtra (or `MH`) | Place of supply for the site tests |
| Site **Bengaluru** | Same order, state Karnataka | Same-state site |
| HSN/SAC | Active, with CGST, SGST, and IGST tax types | Required on a Tax Invoice line |
| Ledgers | Customer, sales income, CGST output, SGST output, IGST output, `ROUNDING_OFF` | Note the balance of each **before** you start |

Write down the six ledger balances. Every expected journal below is the **change** from that starting point.

Invoice numbers look like `INV-BLR-2026-0001`, `PI-BLR-2026-0001`, and `BOS-PUN-2026-0001`. Old `INV-2026-0001` numbers are not renumbered.

---

## 2. Which document is legal

| # | Branch | Type you pick | Line tax | Save |
|---|--------|---------------|----------|------|
| D1 | BLR Regular | Tax Invoice | 18% | Allowed |
| D2 | BLR Regular | Proforma | any | Allowed. Tax is forced to 0 |
| D3 | BLR Regular | Bill of Supply | 0% on every line | Allowed |
| D4 | BLR Regular | Bill of Supply | 18% on any line | **Rejected.** Message: a regular GST branch must issue a Tax Invoice for a taxable supply |
| D5 | COMP Composition | Tax Invoice | 18% | **Rejected.** A composition branch cannot issue a Tax Invoice |
| D6 | COMP Composition | Bill of Supply | 18% shown on the form | Allowed. Stored tax becomes 0 |
| D7 | PUN, no GSTIN | Tax Invoice | 18% | **Rejected.** Branch has no GSTIN. Use Bill of Supply |
| D8 | PUN, no GSTIN | Bill of Supply | any | Allowed. Stored tax is 0 |

D4, D5, and D7 fail on **save**, not only on approve.

---

## 3. Place of supply and B2C

All of these use branch **BLR** (Karnataka, Regular) and type **Tax Invoice**. Line: rate ₹10,000, tax 18%, HSN filled. Grand total ₹11,800.

| # | Sites selected | GST card | Stored GSTIN | Tax | Books on approve |
|---|----------------|----------|--------------|-----|------------------|
| P1 | Pune (Maharashtra). Customer has **no** GST card | none | Blank | **IGST ₹1,800** | Dr customer 11,800 · Cr sales 10,000 · Cr IGST 1,800 |
| P2 | Bengaluru (Karnataka). No GST card | none | Blank | **CGST ₹900 + SGST ₹900** | Dr customer 11,800 · Cr sales 10,000 · Cr CGST 900 · Cr SGST 900 |
| P3 | Pune. Customer’s only card is **KA** | does not match the site | Blank. The Karnataka GSTIN is **not** copied | **IGST ₹1,800** | Same as P1 |
| P4 | Pune. Customer has card **MH** and you pick it | matches Maharashtra | That Maharashtra GSTIN | **IGST ₹1,800** (branch is Karnataka) | Same as P1, and the invoice stores the GSTIN |
| P5 | Two sites, one state `MH` and one state `Maharashtra` | — | — | Same state. Save is not rejected for mixed states | Intra-state if the branch is also Maharashtra |
| P6 | Pune **and** Bengaluru on one invoice | — | — | — | **Rejected.** One invoice cannot mix states |

Ship-to on the form must show the **site** state in the place-of-supply field before you save. The banner and the saved tax must say the same thing. If you change ship-to to a different state, the form clears the GSTIN.

P1 is the B2C case: the customer has no GSTIN, and the branch still charges GST.

---

## 4. What approve posts

Use a line of ₹10,000. Where the screen rounds to the nearest rupee, check `ROUNDING_OFF` for the paise.

### T1 — Tax Invoice (P2 above)

Status becomes **Sent**. Pending = ₹11,800. Received = 0.

| Ledger | Debit | Credit |
|--------|------:|-------:|
| Acme (customer) | 11,800 | |
| Sales income | | 10,000 |
| CGST output | | 900 |
| SGST output | | 900 |

Sales is the **taxable** amount, not grand minus GST.

### T2 — Tax Invoice with round-off

If the unrounded total is ₹11,800.40 and the grand total stored is ₹11,800:

| Ledger | Debit | Credit |
|--------|------:|-------:|
| Customer | 11,800 | |
| Sales income | | taxable amount |
| GST output | | tax |
| Rounding off | 0.40 | |

A positive round-off (grand higher than taxable + tax) is a **credit** to Rounding off.

### T3 — Proforma

Same branch BLR. Type **Proforma**. The form shows no GST. Grand total = ₹10,000.

Approve.

| Check | Expected |
|-------|----------|
| Status | **Issued** |
| Pending | 0 |
| Received | 0 |
| Customer, sales, GST, rounding | **Unchanged** |
| Number | Starts with `PI-BLR-` |
| PDF title | PROFORMA INVOICE |
| Record Payment | Hidden / rejected |
| Credit note | Rejected. Only Sent, Partial, or Overdue |
| Overdue job | Does not change this row |
| A later Tax Invoice for the same job | Still allowed to post |

### T4 — Bill of Supply from PUN (no GSTIN)

Type **Bill of Supply**. Grand total = ₹10,000. GST on the form is 0.

Approve.

| Ledger | Debit | Credit |
|--------|------:|-------:|
| Acme (customer) | 10,000 | |
| Sales income | | 10,000 |

CGST, SGST, and IGST output do not move. Status **Sent**. Pending = ₹10,000. Number starts with `BOS-PUN-`. PDF title is BILL OF SUPPLY. HSN may be blank.

### T5 — Bill of Supply from COMP

Same journal as T4. A Tax Invoice on COMP never reaches approve, because save already rejected it (D5).

### T6 — Exempt lines on BLR

Regular branch, every line tax 0, type **Bill of Supply**. Same journal as T4. A Tax Invoice with taxable value and zero GST can be saved as a draft while tax is still being filled, but **approve** rejects it: add GST, or use Bill of Supply when the supply is exempt.

### T7 — Credit note

| Invoice | Credit note of the full amount |
|---------|--------------------------------|
| T1 Tax Invoice ₹11,800 | Dr sales adjustment 10,000 · Dr CGST 900 · Dr SGST 900 · Cr customer 11,800 |
| T4 Bill of Supply ₹10,000 | Dr sales adjustment 10,000 · Cr customer 10,000. GST ledgers unchanged |

---

## 5. Negative checks (do these after the positive ones)

| # | Action | Expected |
|---|--------|----------|
| N1 | Tax Invoice, place of supply blank, no site | Save rejected. Customer or site state is required |
| N2 | Tax Invoice line, HSN blank | Save rejected. HSN/SAC is required |
| N3 | Tax Invoice, both CGST and IGST amounts | Approve rejected. One document cannot have both |
| N4 | Receipt allocated to the Issued proforma from T3 | Rejected. Receipts are only for a sent tax invoice or bill of supply |
| N5 | Credit note on that Issued proforma | Rejected |
| N6 | Run overdue while that proforma is Issued and its due date is in the past | Status stays Issued |

---

## 6. Pass mark

The script passes when all of these are true:

1. D1–D3 and D6–D8 save. D4, D5, and D7 do not.
2. P1 and P3 store **no** GSTIN and post IGST. P2 posts CGST and SGST. P4 stores the Maharashtra GSTIN and posts IGST.
3. P5 treats `MH` and `Maharashtra` as one state. P6 is rejected.
4. T3 posts **no** ledger row. T1 and T4 post the journals in section 4, and sales equals the taxable amount.
5. T7 reverses GST only on the Tax Invoice.
6. N4, N5, and N6 leave the proforma out of receipts, credit notes, and overdue.

Out of this test: NIC e-invoice IRN, e-way bill, GSTR JSON upload, and the month-end GST input versus output set-off.
