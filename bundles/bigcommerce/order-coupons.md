---
title: "Order coupons"
description: |
  One row per coupon code used on an order, with the discount it gave. Read order by order for every order modified in the daily window.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management/orders/order-coupons/get-order-coupons"
    title: "BigCommerce API reference — Order coupons"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `order_coupons`
- **Default destination table:** `bigcommerce_order_coupons`
- **Unique keys:** `id`
- **Loads:** by date window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. Order coupon line id. |
| `order_id` | INTEGER | Order ID | FK to [Orders](./orders.md). Order the coupon was used on. Join to orders (id). |
| `coupon_id` | INTEGER | Coupon ID | FK to [Coupons](./coupons.md). Coupon. Join to coupons (id). |
| `code` | STRING | Code | Coupon code the shopper entered. |
| `amount` | FLOAT | Amount | Coupon value as set up: a percentage or an amount, depending on type. |
| `type` | INTEGER | Type | Coupon type: 0 per item, 1 percentage, 2 per total, 3 shipping, 4 free shipping, 5 promotion. |
| `discount` | FLOAT | Discount | Discount the coupon gave on this order, in the order's transactional currency. |

# Example Questions

- Which coupon codes were used most this month, and how much discount did each give away?
- What is the average order value of orders with a coupon against orders without one?

## Joins

- [Orders](./orders.md) — `order_id = id` [N:1]
- [Coupons](./coupons.md) — `coupon_id = id` [N:1]
- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
