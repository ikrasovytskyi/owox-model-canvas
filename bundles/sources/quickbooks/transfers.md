---
title: "Transfers"
description: |
  Money moved between the company's own accounts, bank to bank or bank to credit card.
tags: ["owox", "connector", "quickbooks"]
type: "OWOX Data Mart"
sources:
  - id: "vendor-reference"
    resource: "https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/transfer"
    title: "Intuit QuickBooks Online Accounting API reference — transfer"
---

## Connector endpoint

- **Endpoint:** QuickBooks · `transfer`
- **Default destination table:** `quickbooks_transfer`
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
| `txn_date` | DATE | Business date of the transaction (not the edit date). |
| `total_amt` | FLOAT | Total amount of the document including tax, in the document currency. |
| `currency_id` | STRING | Currency code of the document, for example USD; empty when multicurrency is off. |
| `currency_name` | STRING | Currency name that goes with currency_id, for example United States Dollar. |
| `exchange_rate` | FLOAT | Rate that converts the document currency to the company's home currency; 1 or empty when the document is in the home currency. |
| `private_note` | STRING | Internal memo entered on the document; not shown to the customer or vendor. |
| `amount` | FLOAT | Amount transferred between the two accounts, in the home currency. |
| `from_account_id` | STRING | Id of the account the money is moved from; joins to account.id. |
| `from_account_name` | STRING | Name of the account that from_account_id points to. |
| `to_account_id` | STRING | Id of the account the money is moved to; joins to account.id. |
| `to_account_name` | STRING | Name of the account that to_account_id points to. |

# Example Questions

- How much moved between bank and credit-card accounts each month?
- Which transfers fund credit-card payments?

## Joins

- [Accounts](./accounts.md) — `realm_id = realm_id`, `from_account_id = id` [N:1] — The account the money moved from. `to_account_id` holds the account it moved to, joined to Accounts the same way.
  - Description: The account the money moved from. `to_account_id` holds the account it moved to, joined to Accounts the same way.
- [Company info](./company-info.md) — `realm_id = realm_id` [N:1] — The company the row belongs to.
  - Description: The company the row belongs to.
