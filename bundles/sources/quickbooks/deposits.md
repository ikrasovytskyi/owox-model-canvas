---
title: "Deposits"
description: |
  Bank deposits, including the ones that group undeposited customer payments, with the account deposited to.
tags: ["owox", "connector", "quickbooks"]
type: "OWOX Data Mart"
sources:
  - id: "vendor-reference"
    resource: "https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/deposit"
    title: "Intuit QuickBooks Online Accounting API reference — deposit"
---

## Connector endpoint

- **Endpoint:** QuickBooks · `deposit`
- **Default destination table:** `quickbooks_deposit`
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
| `line` | JSON | Line\[\] as JSON; lines either link received Payments (LinkedTxn) or post directly (DepositLineDetail.AccountRef, Entity). |
| `deposit_to_account_id` | STRING | Id of the account the money is deposited to; joins to account.id. |
| `deposit_to_account_name` | STRING | Name of the account that deposit_to_account_id points to. |
| `cash_back_amount` | FLOAT | Amount of cash taken back from the deposit. |
| `cash_back_account_id` | STRING | Id of the account the cash back is posted to; joins to account.id. |
| `cash_back_account_name` | STRING | Name of the account that cash_back_account_id points to. |
| `department_id` | STRING | Id of the department (location) the document is tagged with; empty when locations are off. |
| `department_name` | STRING | Name of the department (location) that department_id points to. |
| `home_total_amt` | FLOAT | Total amount converted to the company's home currency. |

# Example Questions

- How much was deposited per bank account each month, and how much came from Undeposited Funds?
- Which deposits included cash back, and to which account?

## Joins

- [Accounts](./accounts.md) — `realm_id = realm_id`, `deposit_to_account_id = id` [N:1] — The bank account the deposit went into. `cash_back_account_id` holds a second account id (cash back taken), joined to Accounts the same way.
  - Description: The bank account the deposit went into. `cash_back_account_id` holds a second account id (cash back taken), joined to Accounts the same way.
- [Company info](./company-info.md) — `realm_id = realm_id` [N:1] — The company the row belongs to.
  - Description: The company the row belongs to.
