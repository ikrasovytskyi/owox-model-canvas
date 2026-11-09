---
title: "Balances"
description: |
  A daily snapshot of the PayPal balance per currency: total, available and withheld, as of 00:00 UTC on `as_of_date`. The cash position the transactions add up to.
tags: ["owox", "connector", "paypal"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.paypal.com/docs/api/transaction-search/v1/"
    title: "OWOX Data Marts — PayPal custom connector (PayPal, 7 nodes) on the PayPal REST APIs: Transaction Search v1, Disputes v1, Invoicing v2, Subscriptions v1, Catalog Products v1"
  - id: "vendor-reference"
    resource: "https://developer.paypal.com/docs/api/transaction-search/v1/#balances_get"
    title: "PayPal API reference — Balances"
---

## Connector endpoint

- **Endpoint:** PayPal · `balances`
- **Default destination table:** `paypal_balances`
- **Unique keys:** `as_of_date`, `currency`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `as_of_date` | DATE | As Of Date | PK. The day of the snapshot; the balance is taken at 00:00 UTC on this date. |
| `currency` | STRING | Currency | PK. Three-letter ISO currency code of this balance. |
| `account_id` | STRING | Account ID | Your PayPal merchant account id as entered in the connector (Account settings → Business information); lets several PayPal accounts share one table. Empty when not set. |
| `primary` | BOOLEAN | Primary | true for the account's primary currency. |
| `total_balance` | FLOAT | Total Balance | Total balance held in this currency, including amounts on hold. |
| `available_balance` | FLOAT | Available Balance | Balance that can be withdrawn or spent now. |
| `withheld_balance` | FLOAT | Withheld Balance | Amount PayPal is holding (reserves, holds, pending disputes). |

# Example Questions

- How much is in the PayPal balance this morning, per currency, and how much of it is withheld?
- How did the available balance move over the month against the withdrawals we made?
