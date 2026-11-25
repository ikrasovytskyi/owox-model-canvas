---
title: "Order transactions"
description: |
  One row per payment event on an order: authorization, capture, purchase, refund, void, with the gateway, method, amount and outcome. Read order by order for every order modified in the daily window.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management/order-operations/get-order-transactions"
    title: "BigCommerce API reference — Order transactions"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `order_transactions`
- **Default destination table:** `bigcommerce_order_transactions`
- **Unique keys:** `id`
- **Loads:** by date window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. Transaction id, unique in the store. |
| `order_id` | INTEGER | Order ID | FK to [Orders](./orders.md). Order the payment belongs to. Join to orders (id). |
| `event` | STRING | Event | purchase, authorization, capture, refund, void, pending or settled. |
| `method` | STRING | Method | credit_card, electronic_wallet, gift_certificate, store_credit, apple_pay_card, bank_deposit, cash, check, money_order, nonce, offsite, custom and similar. |
| `amount` | FLOAT | Amount | Amount of the transaction in its currency. |
| `currency` | STRING | Currency | Currency of the amount (ISO 4217). |
| `gateway` | STRING | Gateway | Payment gateway, e.g. braintree, stripe, paypal, authorizenet, testgateway, custom. |
| `gateway_transaction_id` | STRING | Gateway Transaction ID | The transaction id at the gateway; the join to Stripe, PayPal or Braintree data. |
| `status` | STRING | Status | ok or error. |
| `test` | BOOLEAN | Test | true for a transaction made in the gateway's test mode. |
| `fraud_review` | BOOLEAN | Fraud Review | true when the gateway flagged the transaction for fraud review. |
| `reference_transaction_id` | INTEGER | Reference Transaction ID | Earlier transaction this one refers to (a capture refers to its authorization). |
| `date_created` | DATETIME | Date Created | When the transaction happened (UTC, ISO 8601). |

# Example Questions

- How much was captured, refunded and declined per payment gateway this month?
- Which orders have a capture but no matching settlement in the gateway's own data?

## Joins

- [Orders](./orders.md) — `order_id = id` [N:1]
- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
