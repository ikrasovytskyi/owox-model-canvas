---
title: "Variants"
description: |
  One row per SKU sold: the product it belongs to, its price, sale price, cost and stock. Refreshed in full on every run (the API has no date filter).
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/catalog/product-variants/variants-batch/get-variants"
    title: "BigCommerce API reference — Variants"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `variants`
- **Default destination table:** `bigcommerce_variants`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. Variant id, unique in the store. Join to order_products (variant_id). |
| `product_id` | INTEGER | Product ID | FK to [Products](./products.md). Product this variant belongs to. Join to products (id). |
| `sku` | STRING | SKU | Variant SKU. |
| `price` | FLOAT | Price | Variant price; empty when it inherits the product price. |
| `sale_price` | FLOAT | Sale Price | Variant sale price; empty when it inherits. |
| `retail_price` | FLOAT | Retail Price | Variant retail price; empty when it inherits. |
| `calculated_price` | FLOAT | Calculated Price | Price a shopper pays for this variant now. |
| `cost_price` | FLOAT | Cost Price | Cost of the variant to the store; empty when it inherits the product's cost. |
| `inventory_level` | INTEGER | Inventory Level | Units in stock for this variant. |
| `purchasing_disabled` | BOOLEAN | Purchasing Disabled | true when this variant cannot be bought. |
| `upc` | STRING | Upc | UPC barcode. |
| `option_values` | STRING | Option Values | JSON list of the options that define the variant: option_display_name, label. |

# Example Questions

- Which variants have no cost price, so their margin cannot be computed?
- What is the stock value at cost by product?

## Joins

- [Products](./products.md) — `product_id = id` [N:1]
- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
