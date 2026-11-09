---
title: "Billing plans"
description: |
  Every subscription billing plan with its product, status, billing cycles (trial and regular price, interval, count), setup fee and tax settings. The subscription price list.
tags: ["owox", "connector", "paypal"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.paypal.com/docs/api/transaction-search/v1/"
    title: "OWOX Data Marts — PayPal custom connector (PayPal, 7 nodes) on the PayPal REST APIs: Transaction Search v1, Disputes v1, Invoicing v2, Subscriptions v1, Catalog Products v1"
  - id: "vendor-reference"
    resource: "https://developer.paypal.com/docs/api/subscriptions/v1/#plans_list"
    title: "PayPal API reference — Billing plans"
---

## Connector endpoint

- **Endpoint:** PayPal · `plans`
- **Default destination table:** `paypal_plans`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Plan id (P-…). |
| `account_id` | STRING | Account ID | Your PayPal merchant account id as entered in the connector (Account settings → Business information); lets several PayPal accounts share one table. Empty when not set. |
| `product_id` | STRING | Product ID | FK to [Catalog products](./catalog-products.md). The catalog product this plan sells. Join to products. |
| `name` | STRING | Name | Plan name. |
| `description` | STRING | Description | Plan description. |
| `status` | STRING | Status | CREATED, ACTIVE or INACTIVE. |
| `usage_type` | STRING | Usage Type | LICENSED (fixed price per cycle) or METERED. |
| `quantity_supported` | BOOLEAN | Quantity Supported | true when a subscriber can buy more than one unit of the plan. |
| `billing_cycles` | JSON | Billing Cycles | JSON list of the plan's cycles: tenure_type (TRIAL or REGULAR), sequence, total_cycles, frequency.interval_unit and interval_count, pricing_scheme.fixed_price.value and currency_code. Unnest and keep tenure_type = REGULAR for the recurring price. |
| `setup_fee` | FLOAT | Setup Fee | One-off setup fee charged on the first payment, as a decimal. |
| `setup_fee_currency` | STRING | Setup Fee Currency | Currency of setup_fee. |
| `auto_bill_outstanding` | BOOLEAN | Auto Bill Outstanding | true when an outstanding balance is billed with the next cycle. |
| `payment_failure_threshold` | INTEGER | Payment Failure Threshold | Failed payments allowed before the subscription is suspended. |
| `tax_percentage` | FLOAT | Tax Percentage | Tax percentage applied to the plan. |
| `tax_inclusive` | BOOLEAN | Tax Inclusive | true when the plan price already includes tax. |
| `create_time` | TIMESTAMP | Create Time | When the plan was created (UTC). |
| `update_time` | TIMESTAMP | Update Time | When the plan last changed (UTC). |

# Example Questions

- Which plans are active, at what regular price and interval?
- How many plans still bill a legacy price?

## Joins

- [Catalog products](./catalog-products.md) — `product_id = id` [N:1] — The product the plan sells.
