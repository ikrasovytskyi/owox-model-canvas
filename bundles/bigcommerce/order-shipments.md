---
title: "Order shipments"
description: |
  One row per shipment: when it was created, the carrier and tracking, and which order lines it carried. Read order by order for every order modified in the daily window. Time to ship is date_created here minus orders.date_created.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management/orders/order-shipments/get-order-shipments"
    title: "BigCommerce API reference — Order shipments"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `order_shipments`
- **Default destination table:** `bigcommerce_order_shipments`
- **Unique keys:** `id`
- **Loads:** by date window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. Shipment id, unique in the store. |
| `order_id` | INTEGER | Order ID | FK to [Orders](./orders.md). Order shipped. Join to orders (id). |
| `order_address_id` | INTEGER | Order Address ID | FK to [Order shipping addresses](./order-shipping-addresses.md). Shipping address the shipment went to. Join to order_shipping_addresses (id). |
| `customer_id` | INTEGER | Customer ID | Customer; 0 for a guest. |
| `date_created` | DATETIME | Date Created | When the shipment was created (UTC); BigCommerce returns it in RFC 2822 (UTC). |
| `shipping_method` | STRING | Shipping Method | Shipping method of the shipment. |
| `shipping_provider` | STRING | Shipping Provider | Shipping provider integration, e.g. ups, usps, fedex; empty for a custom carrier. |
| `tracking_carrier` | STRING | Tracking Carrier | Carrier code used for the tracking link. |
| `tracking_number` | STRING | Tracking Number | Tracking number. |
| `items` | STRING | Items | JSON list of the order lines in the shipment: order_product_id, product_id, quantity. |

# Example Questions

- How many hours pass between an order being placed and its first shipment?
- Which carriers carry most shipments?

## Joins

- [Orders](./orders.md) — `order_id = id` [N:1]
- [Order shipping addresses](./order-shipping-addresses.md) — `order_address_id = id` [N:1]
- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
