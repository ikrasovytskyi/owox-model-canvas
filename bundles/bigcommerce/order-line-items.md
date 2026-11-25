---
title: "Order line items"
description: |
  One row per product line in an order: product and variant, quantity, unit price, line totals before and after discounts, cost price, and how many units were refunded or shipped. Read order by order for every order modified in the daily window. The line-level table for revenue, units and margin by product.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management/orders/order-products/get-order-products"
    title: "BigCommerce API reference — Order line items"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `order_products`
- **Default destination table:** `bigcommerce_order_products`
- **Unique keys:** `id`
- **Loads:** by date window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. Order line id, unique in the store. Refund items of type PRODUCT point at it (item_id). |
| `order_id` | INTEGER | Order ID | FK to [Orders](./orders.md). Order this line belongs to. Join to orders (id). |
| `product_id` | INTEGER | Product ID | FK to [Products](./products.md). Catalog product id; 0 for a custom product typed into the order. Join to products (id). |
| `variant_id` | INTEGER | Variant ID | FK to [Variants](./variants.md). Catalog variant id (the SKU actually sold). Join to variants (id). |
| `order_address_id` | INTEGER | Order Address ID | FK to [Order shipping addresses](./order-shipping-addresses.md). Shipping address this line ships to. Join to order_shipping_addresses (id). |
| `name` | STRING | Name | Product name at the time of the order. |
| `sku` | STRING | SKU | SKU at the time of the order. |
| `brand` | STRING | Brand | Brand name at the time of the order. |
| `type` | STRING | Type | physical or digital. |
| `quantity` | INTEGER | Quantity | Units ordered. |
| `base_price` | FLOAT | Base Price | Unit price before tax and discounts, in the order's transactional currency. |
| `price_ex_tax` | FLOAT | Price Ex Tax | Unit price excluding tax, in the order's transactional currency. |
| `price_inc_tax` | FLOAT | Price Inc Tax | Unit price including tax, in the order's transactional currency. |
| `base_total` | FLOAT | Base Total | base_price × quantity, in the order's transactional currency. |
| `total_ex_tax` | FLOAT | Total Ex Tax | Line total excluding tax, before order-level discounts, in the order's transactional currency. |
| `total_inc_tax` | FLOAT | Total Inc Tax | Line total including tax, before order-level discounts, in the order's transactional currency. |
| `total_tax` | FLOAT | Total Tax | Tax on the line, in the order's transactional currency. |
| `discounted_total_inc_tax` | FLOAT | Discounted Total Inc Tax | Line total after every discount, including tax, in the order's transactional currency. BigCommerce's own field for accounting. |
| `base_cost_price` | FLOAT | Base Cost Price | Unit cost price from the catalog at order time, in the order's transactional currency; 0 when the store keeps no cost. |
| `cost_price_ex_tax` | FLOAT | Cost Price Ex Tax | Unit cost price excluding tax, in the order's transactional currency. |
| `quantity_refunded` | INTEGER | Quantity Refunded | Units of this line refunded so far. |
| `is_refunded` | BOOLEAN | Is Refunded | true when the line has been refunded in full. |
| `quantity_shipped` | INTEGER | Quantity Shipped | Units of this line shipped so far. |
| `is_bundled_product` | BOOLEAN | Is Bundled Product | true when the line is a product inside a bundle. |
| `parent_order_product_id` | INTEGER | Parent Order Product ID | For a bundled product, the line id of the bundle it belongs to. |
| `applied_discounts` | STRING | Applied Discounts | JSON list of the discounts on this line: id, amount, name, code (coupon code or automatic discount), target. |
| `product_options` | STRING | Product Options | JSON list of the options chosen (size, colour and the like): display_name, display_value. |

# Example Questions

- Which `products` and `variants` bring the most revenue after discounts, and what is their margin over cost price?
- Which SKUs are refunded most, by units refunded per unit sold?

## Joins

- [Orders](./orders.md) — `order_id = id` [N:1]
- [Products](./products.md) — `product_id = id` [N:1]
- [Variants](./variants.md) — `variant_id = id` [N:1] — The SKU sold; carries cost_price for margin.
- [Order shipping addresses](./order-shipping-addresses.md) — `order_address_id = id` [N:1]
- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
