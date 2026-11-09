---
title: "Refund receipts"
description: |
  Money returned to a customer from a bank or credit-card account.
tags: ["owox", "connector", "quickbooks"]
type: "OWOX Data Mart"
sources:
  - id: "vendor-reference"
    resource: "https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/refundreceipt"
    title: "Intuit QuickBooks Online Accounting API reference — refundreceipt"
---

## Connector endpoint

- **Endpoint:** QuickBooks · `refund_receipt`
- **Default destination table:** `quickbooks_refund_receipt`
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
| `customer_id` | STRING | Id of the customer on the document; joins to customer.id. |
| `customer_name` | STRING | Name of the customer that customer_id points to. |
| `deposit_to_account_id` | STRING | Account the refund was paid from. |
| `deposit_to_account_name` | STRING | Name of the account that deposit_to_account_id points to. |
| `payment_method_id` | STRING | Id of the preferred payment method; joins to the QuickBooks payment method list. |
| `payment_method_name` | STRING | Name of the preferred payment method that payment_method_id points to, for example Cash or Check. |
| `payment_ref_num` | STRING | Payment reference number, for example a check number or card reference. |
| `class_id` | STRING | Id of the class used to tag the document for class tracking; empty when classes are off. |
| `class_name` | STRING | Name of the class that class_id points to. |
| `department_id` | STRING | Id of the department (location) the document is tagged with; empty when locations are off. |
| `department_name` | STRING | Name of the department (location) that department_id points to. |
| `total_tax` | FLOAT | Total sales tax on the document, in the document currency. |
| `home_total_amt` | FLOAT | Total amount converted to the company's home currency. |

# Example Questions

- How much was refunded per month and from which account?
- Which items or customers drive refunds?

## Joins

- [Customers](./customers.md) — `realm_id = realm_id`, `customer_id = id` [N:1] — The customer the document is for.
  - Description: The customer the document is for.
- [Accounts](./accounts.md) — `realm_id = realm_id`, `deposit_to_account_id = id` [N:1] — Account the refund was paid from.
  - Description: Account the refund was paid from.
- [Company info](./company-info.md) — `realm_id = realm_id` [N:1] — The company the row belongs to.
  - Description: The company the row belongs to.
