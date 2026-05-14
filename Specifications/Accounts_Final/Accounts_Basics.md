# 📚 Accounts Module - Beginner's Guide (Bahi Khata)

## What is the Accounts Module?
Think of it as the **"Money Tracker"** of the ERP system. It records all money coming in (sales) and money going out (purchases), and keeps track of who owes whom. In the Indian context, this is your digital **"Bahi Khata"**.

---

## Module 28: Invoicing (बिक्री चालान) 🧾

### What it does?
Creates bills for customers when you sell services or products.

### Simple Workflow:
1. **Customer asks for pest control service**
2. **Create INVOICE (Bill to customer)**
3. **Add service details** (Cockroach treatment, Termite control, etc. from Module 21)
4. **System auto-calculates GST** (CGST + SGST or IGST based on Module 9)
5. **Send invoice to customer**
6. **Customer pays → Mark as PAID**

### Key Points:
- **Tax Invoice:** Official bill with GST.
- **Proforma Invoice:** Quotation/Estimate (not a legal bill).
- **Credit Note:** Issued when you refund or give a discount.
- **E-Invoice:** Digital invoice with QR code (required for B2B above ₹50,000).

### Status Flow:
`DRAFT → SENT → PAID or PARTIAL → CLOSED`
*(Also includes CANCELLED or OVERDUE status)*

---

## Module 29: Bills (खरीद बिल) 📥

### What it does?
Records bills you receive from suppliers/vendors when you buy items (Chemicals, Sprayers, etc.).

### Simple Workflow:
1. **You buy chemicals/equipment from supplier** (Module 11)
2. **Supplier sends BILL/Invoice**
3. **Create BILL in system** (match with Purchase Order if exists)
4. **Verify items received** (GRN - Goods Receipt Note from Module 11)
5. **Approve bill for payment**
6. **Pay the bill → Mark as PAID**

### Key Points:
- **Purchase Bill:** For stock items like chemicals.
- **Expense Bill:** For rent, electricity, office expenses.
- **3-Way Match:** PO (Order) + GRN (Received) + Bill (Charge) must match.
- **TDS:** Tax Deducted at Source (automatically calculated).

---

## Module 30: Payments (भुगतान रजिस्टर) 💰

### What it does?
Records all money movements - Receipts (IN) and Payments (OUT).

### Workflow - Receipt (Money IN):
- Customer pays (Cash/UPI/Bank) → Create **RECEIPT VOUCHER**.
- Adjust against pending **INVOICE** (Module 28).

### Workflow - Payment (Money OUT):
- You pay supplier → Create **PAYMENT VOUCHER**.
- Adjust against pending **BILL** (Module 29).

### Payment Modes:
- Cash 💵, Cheque 📝, Bank Transfer (NEFT/RTGS) 🏦, UPI 📱, Card 💳.

---

## Module 31: Ledger (खाata प्रबंधन) 📒

### What it does?
Master record of all parties: Customers, Vendors, Banks. Like a Phone Book + Bank Passbook combined!

| Type | Example | What it tracks |
|------|---------|--------------|
| **Sundry Debtors** | Customers | Money they owe YOU |
| **Sundry Creditors** | Suppliers | Money you owe THEM |
| **Duties & Taxes** | GST / TDS | Government tax accounts |
| **Income/Expenses** | Sales / Rent | Money earned or spent |

---

## Module 32: Charts of Accounts (लेखा चार्ट) 📊

### What it does?
Organizes all accounts in a standard "Folders and Sub-folders" structure (Assets, Liabilities, Income, Expenses).

---

## Module 33: Financial Reports 📈

1. **Balance Sheet:** Company's health (Assets = Liabilities + Capital).
2. **Profit & Loss:** Performance (Income - Expenses = Profit).
3. **GST Compliance:** Ready reports for GSTR-1 and GSTR-3B filing.
4. **Ageing Analysis:** Shows how old your pending payments are (0-30, 31-60 days).

---

## 🎯 Key Takeaways for Beginners

- **Invoice:** Bill you GIVE (Income).
- **Bill:** Bill you GET (Expense).
- **Receipt:** Money COMING IN.
- **Payment:** Money GOING OUT.
- **Ledger:** Party's history.
- **GST:** Tax on sales/purchases.
