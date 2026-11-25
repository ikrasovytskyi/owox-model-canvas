---
title: "Refunds"
description: |
  One row per refund: when it was made, the total and tax refunded, the reason, which items it covered (products, shipping, handling, the whole order) and how the money went back. Refunds are never edited, so the daily window by created is complete. Subtract total_amount by created date to get net sales.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management/order-operations/payment-actions/get-orders-refunds"
    title: "BigCommerce API reference — Refunds"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `refunds`
- **Default destination table:** `bigcommerce_refunds`
- **Unique keys:** `id`
- **Loads:** by date window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. Refund id, unique in the store. |
| `order_id` | INTEGER | Order ID | FK to [Orders](./orders.md). Order refunded. Join to orders (id). |
| `user_id` | INTEGER | User ID | Control-panel user who issued the refund; 0 for an API refund. |
| `created` | DATETIME | Created | When the refund was made (UTC, ISO 8601). The field the daily window filters on and the date to subtract refunds by. |
| `reason` | STRING | Reason | Reason typed for the refund. |
| `total_amount` | FLOAT | Total Amount | Total refunded, in the order's transactional currency. |
| `total_tax` | FLOAT | Total Tax | Tax part of the refund, in the order's transactional currency. |
| `uses_merchant_override_values` | BOOLEAN | Uses Merchant Override Values | true when the merchant typed the amounts instead of using BigCommerce's calculation. |
| `items` | STRING | Items | JSON list of the refunded items: item_type (PRODUCT, SHIPPING, HANDLING, GIFT_WRAPPING, ORDER, FEE), item_id (order line id for PRODUCT, shipping address id for SHIPPING/HANDLING, order id for ORDER), quantity, requested_amount, reason. |
| `payments` | STRING | Payments | JSON list of the refund payments: provider_id (payment gateway or store credit), amount, offline, is_declined, declined_message. |

# Example Questions

- How much was refunded each week, and on which `orders`?
- What share of refunds is shipping and handling rather than products?

## Joins

- [Orders](./orders.md) — `order_id = id` [N:1] — Refund items of type PRODUCT point at order_products.id through the items JSON.
- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
