---
title: "Invoices"
description: |
  Sales on credit: one row per invoice with customer, dates, total, open balance and the line items as JSON. The receivable side of the business.
tags: ["owox", "connector", "quickbooks"]
type: "OWOX Data Mart"
sources:
  - id: "vendor-reference"
    resource: "https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/invoice"
    title: "Intuit QuickBooks Online Accounting API reference — invoice"
---

## Connector endpoint

- **Endpoint:** QuickBooks · `invoice`
- **Default destination table:** `quickbooks_invoice`
- **Unique keys:** `realm_id`, `id`
- **Time series:** no, one current row per key

# Schema

| Column | Type | Description |
|--------|------|-------------|
| `id` | STRING | PK. QuickBooks entity Id (string). Primary key together with realm_id. |
| `realm_id` | STRING | PK. Company (realm) id, stamped from the RealmId parameter. |
| `sync_token` | STRING | Version counter; changes on every edit. |
| `create_time` | TIMESTAMP | When the record was created in QuickBooks (UTC timestamp). |
| `last_updated_time` | TIMESTAMP | Incremental cursor. |
| `doc_number` | STRING | Document number shown on the form (for example the invoice or bill number); can be empty when the company does not use automatic numbering. |
| `txn_date` | DATE | Business date of the transaction (not the edit date). |
| `total_amt` | FLOAT | Total amount of the document including tax, in the document currency. |
| `currency_id` | STRING | Currency code of the document, for example USD; empty when multicurrency is off. |
| `currency_name` | STRING | Currency name that goes with currency_id, for example United States Dollar. |
| `exchange_rate` | FLOAT | Rate that converts the document currency to the company's home currency; 1 or empty when the document is in the home currency. |
| `private_note` | STRING | Internal memo entered on the document; not shown to the customer or vendor. |
| `line` | JSON | Line array kept as JSON. Unnest in SQL: DetailType decides which \*LineDetail object is present. |
| `due_date` | DATE | Date the invoice is due for payment. |
| `balance` | FLOAT | Unpaid remainder; 0 when fully paid. |
| `customer_id` | STRING | Id of the customer on the document; joins to customer.id. |
| `customer_name` | STRING | Name of the customer that customer_id points to. |
| `customer_memo` | STRING | Message to the customer printed on the document. |
| `bill_email` | STRING | Email address the invoice is sent to. |
| `email_status` | STRING | Whether the document has been emailed: NotSet, NeedToSend or EmailSent. |
| `print_status` | STRING | Print state of the document: NotSet, NeedToPrint or PrintComplete. |
| `sales_term_id` | STRING | Id of the payment terms applied (for example Net 30); joins to the QuickBooks terms list. |
| `sales_term_name` | STRING | Name of the payment terms that sales_term_id points to. |
| `deposit_to_account_id` | STRING | Id of the account the money is deposited to; joins to account.id. |
| `deposit_to_account_name` | STRING | Name of the account that deposit_to_account_id points to. |
| `class_id` | STRING | Id of the class used to tag the document for class tracking; empty when classes are off. |
| `class_name` | STRING | Name of the class that class_id points to. |
| `department_id` | STRING | Id of the department (location) the document is tagged with; empty when locations are off. |
| `department_name` | STRING | Name of the department (location) that department_id points to. |
| `ship_date` | DATE | Date the goods were or will be shipped. |
| `tracking_num` | STRING | Shipping tracking number entered on the invoice. |
| `total_tax` | FLOAT | Total sales tax on the document, in the document currency. |
| `txn_tax_detail` | JSON | Tax detail of the document as JSON: total tax, tax code and the per-rate tax lines. |
| `linked_txn` | JSON | Header-level links (payments, estimates) as JSON: TxnId, TxnType. |
| `home_total_amt` | FLOAT | Total amount converted to the company's home currency. |
| `home_balance` | FLOAT | Open balance converted to the company's home currency. |

# Example Questions

- How much was invoiced per month, per customer and per sales term?
- Which invoices are overdue, by how many days, and what balance is open on each?
- How long do customers take to pay, from `txn_date` to the linked payment?

## Joins

- [Customers](./customers.md) — `realm_id = realm_id`, `customer_id = id` [N:1] — The customer the document is for.
  - Description: The customer the document is for.
- [Accounts](./accounts.md) — `realm_id = realm_id`, `deposit_to_account_id = id` [N:1] — Set only when the invoice carries a deposit.
  - Description: Set only when the invoice carries a deposit.
- [Company info](./company-info.md) — `realm_id = realm_id` [N:1] — The company the row belongs to.
  - Description: The company the row belongs to.
