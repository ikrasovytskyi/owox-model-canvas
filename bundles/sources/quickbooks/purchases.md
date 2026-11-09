---
title: "Purchases"
description: |
  Expenses paid directly, without a bill: cash, check or card purchases with the account paid from and the payee. `entity_id` names the payee, and `entity_type` says whether it is a vendor, customer or employee; because ids are only unique per type, it is not drawn as a join to Vendors.
tags: ["owox", "connector", "quickbooks"]
type: "OWOX Data Mart"
sources:
  - id: "vendor-reference"
    resource: "https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/purchase"
    title: "Intuit QuickBooks Online Accounting API reference — purchase"
---

## Connector endpoint

- **Endpoint:** QuickBooks · `purchase`
- **Default destination table:** `quickbooks_purchase`
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
| `payment_type` | STRING | Cash, Check or CreditCard. |
| `account_id` | STRING | Bank or credit-card account the money left. |
| `account_name` | STRING | Name of the bank or credit card account the purchase was paid from. |
| `entity_id` | STRING | Vendor, Customer or Employee paid. |
| `entity_name` | STRING | Name of the payee (vendor, customer or employee) on the purchase. |
| `entity_type` | STRING | Type of the payee: Vendor, Customer or Employee. |
| `payment_method_id` | STRING | Id of the preferred payment method; joins to the QuickBooks payment method list. |
| `payment_method_name` | STRING | Name of the preferred payment method that payment_method_id points to, for example Cash or Check. |
| `credit` | BOOLEAN | True for a credit-card refund. |
| `print_status` | STRING | Print state of the check: NotSet, NeedToPrint or PrintComplete. |
| `department_id` | STRING | Id of the department (location) the document is tagged with; empty when locations are off. |
| `department_name` | STRING | Name of the department (location) that department_id points to. |
| `total_tax` | FLOAT | Total sales tax on the document, in the document currency. |

# Example Questions

- What was spent directly, without a bill, by account and payee each month?
- Which vendors are paid mostly by card versus check?

## Joins

- [Accounts](./accounts.md) — `realm_id = realm_id`, `account_id = id` [N:1] — Bank, credit card or cash account paid from.
  - Description: Bank, credit card or cash account paid from.
- [Company info](./company-info.md) — `realm_id = realm_id` [N:1] — The company the row belongs to.
  - Description: The company the row belongs to.
