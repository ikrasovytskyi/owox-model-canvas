---
title: "Coupons"
description: |
  Every coupon set up in the store: code, type, value, minimum purchase, whether it is on, and how often it was used. Refreshed in full on every run.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/content/marketing/coupons/get-coupons"
    title: "BigCommerce API reference — Coupons"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `coupons`
- **Default destination table:** `bigcommerce_coupons`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. Coupon id. Join to order_coupons (coupon_id). |
| `name` | STRING | Name | Coupon name. |
| `code` | STRING | Code | Code shoppers enter. |
| `type` | STRING | Type | per_item_discount, percentage_discount, per_total_discount, shipping_discount, free_shipping or promotion. |
| `amount` | FLOAT | Amount | Discount value: an amount or a percentage, depending on type. |
| `min_purchase` | FLOAT | Min Purchase | Minimum order subtotal for the coupon to apply. |
| `enabled` | BOOLEAN | Enabled | true when the coupon can be used. |
| `num_uses` | INTEGER | Num Uses | Times the coupon has been used. |
| `max_uses` | INTEGER | Max Uses | Maximum uses; 0 for unlimited. |
| `max_uses_per_customer` | INTEGER | Max Uses Per Customer | Maximum uses per customer; 0 for unlimited. |
| `date_created` | DATETIME | Date Created | When the coupon was created (UTC); BigCommerce returns it in RFC 2822 (UTC). |
| `expires` | STRING | Expires | Expiry date as BigCommerce stores it (RFC 2822); empty when it never expires. |

# Example Questions

- Which coupons are enabled but never used?
- Which coupons are close to their maximum uses?

## Joins

- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
