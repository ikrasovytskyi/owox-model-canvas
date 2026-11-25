---
title: "Order shipping addresses"
description: |
  One row per shipping address on an order (an order can ship to several): where it went, the shipping method and zone, and what shipping cost. Read order by order for every order modified in the daily window. Street, names and phone numbers are left out.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management/orders/order-shipping-addresses/get-order-shipping-addresses"
    title: "BigCommerce API reference — Order shipping addresses"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `order_shipping_addresses`
- **Default destination table:** `bigcommerce_order_shipping_addresses`
- **Unique keys:** `id`
- **Loads:** by date window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. Shipping address id, unique in the store. Join to order_products (order_address_id) and order_shipments (order_address_id). |
| `order_id` | INTEGER | Order ID | FK to [Orders](./orders.md). Order. Join to orders (id). |
| `country_iso2` | STRING | Country Iso2 | Destination country, ISO 3166-1 alpha-2. |
| `state` | STRING | State | Destination state or region. |
| `city` | STRING | City | Destination city. |
| `zip` | STRING | Zip | Destination postal code. |
| `items_total` | INTEGER | Items Total | Units shipping to this address. |
| `items_shipped` | INTEGER | Items Shipped | Units shipped to this address so far. |
| `shipping_method` | STRING | Shipping Method | Shipping method chosen, e.g. Free Shipping, Flat Rate, UPS Ground. |
| `shipping_zone_id` | INTEGER | Shipping Zone ID | Shipping zone id. |
| `shipping_zone_name` | STRING | Shipping Zone Name | Shipping zone name. |
| `base_cost` | FLOAT | Base Cost | Shipping cost before tax and overrides, in the order's transactional currency. |
| `cost_ex_tax` | FLOAT | Cost Ex Tax | Shipping charged for this address excluding tax, in the order's transactional currency. |
| `cost_inc_tax` | FLOAT | Cost Inc Tax | Shipping charged for this address including tax, in the order's transactional currency. |

# Example Questions

- Which countries and regions do we ship to most, and what does shipping cost there?
- How many orders ship to more than one address?

## Joins

- [Orders](./orders.md) — `order_id = id` [N:1]
- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
