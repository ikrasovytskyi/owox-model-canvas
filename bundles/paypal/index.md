---
title: "PayPal"
description: |
  What the OWOX Data Marts PayPal connector loads from one PayPal business account, and how the tables fit together.
  Every table carries `account_id`, your PayPal merchant account as entered in the connector, so several accounts can share one table.
  
  [Transactions](./transactions.md) is the spine: every balance-affecting movement with gross `transaction_amount`, PayPal's `fee_amount`
  and `ending_balance`, PayPal's own running balance after the row, classified by `transaction_event_code` (T00xx sales, T01xx fees,
  T02xx currency conversions, T04xx withdrawals to the bank, T11xx refunds, T12xx chargebacks, T15xx/T21xx holds). Net is gross plus fee.
  [Balances](./balances.md) is the daily snapshot the transactions reconcile to. [Disputes](./disputes.md) and [Dispute details](./dispute-details.md)
  are the chargebacks, with the disputed `seller_transaction_id` pointing back at the payment. [Invoices](./invoices.md) are the receivables;
  a payment that settles one carries its `paypal_invoice_id`. [Billing plans](./billing-plans.md) and [Catalog products](./catalog-products.md)
  are the subscription price list; recurring revenue itself is the `T0002` rows in Transactions, grouped by `paypal_reference_id`, the subscription id.
  
  Two amounts, one date: `transaction_amount` with `transaction_initiation_date` for what was sold, `transaction_amount + fee_amount` for what reached
  the balance, and the `T04xx` rows for what reached the bank. Amounts are decimals in `transaction_currency`.
  
  In the business model Transactions join Orders on `invoice_id` (the order number the store passes to PayPal) and Customers on `payer_email`;
  the fee rows sit beside Stripe's `balance_transactions.fee` as the payment-processing cost line; withdrawals join the bank or QuickBooks
  deposits on date and amount.
  
  Definitions follow connector version 1 (2026-11-07).
tags: ["owox", "index", "connector", "paypal"]
type: "index"
sources:
  - id: "owox-connector"
    resource: "https://developer.paypal.com/docs/api/transaction-search/v1/"
    title: "OWOX Data Marts — PayPal custom connector (PayPal, 7 nodes) on the PayPal REST APIs: Transaction Search v1, Disputes v1, Invoicing v2, Subscriptions v1, Catalog Products v1"
    last_modified: 2026-11-07
---

<!-- OWOX:GENERATED:START — regenerated on export, do not edit inside this block -->

| Data Mart | Fields | Description |
|-----------|--------|-------------|
| [Transactions](./transactions.md) | 45 | Every balance-affecting movement on the PayPal account: payments received, fees, refunds, chargebacks, holds, currency conversions and withdrawals, one row each, classified by `transaction_event_code`. |
| [Balances](./balances.md) | 7 | A daily snapshot of the PayPal balance per currency: total, available and withheld, as of 00:00 UTC on `as_of_date`. |
| [Disputes](./disputes.md) | 13 | Every buyer dispute, inquiry and chargeback with its reason, stage, status, amount and response deadlines. |
| [Dispute details](./dispute-details.md) | 33 | One row per dispute with the disputed transaction id, its gross amount, the outcome and the refund PayPal made, any settlement offer and the message trail. |
| [Invoices](./invoices.md) | 35 | Every PayPal invoice in any status with its amounts, due date, recipient and what has been paid or refunded against it. |
| [Billing plans](./billing-plans.md) | 17 | Every subscription billing plan with its product, status, billing cycles (trial and regular price, interval, count), setup fee and tax settings. |
| [Catalog products](./catalog-products.md) | 10 | Every catalog product that plans sell, with its type and category. |

# Example Questions

- How much did we net through PayPal last month after fees, refunds and chargebacks, and does the running balance in `transactions` match the morning `balances` snapshot?
- Which disputes are open, how much is at stake, and which payments and orders do they concern?
- How much recurring revenue came through PayPal subscriptions (`T0002` rows) by plan, and how much is still owed on open `invoices`?

# Explore this model

**[▶ Explore on canvas](https://model.owox.com/?okf=https://raw.githubusercontent.com/ikrasovytskyi/owox-model-canvas/main/bundles/paypal/index.md)**

One click opens this model in a free OWOX canvas you can poke around in — no account needed.

<!-- OWOX:GENERATED:END -->
