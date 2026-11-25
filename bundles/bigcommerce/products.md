---
title: "Products"
description: |
  One row per catalog product: name, SKU, brand, categories, prices, cost, stock and units sold. The daily window reads products by date_modified.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/catalog/products/get-products"
    title: "BigCommerce API reference — Products"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `products`
- **Default destination table:** `bigcommerce_products`
- **Unique keys:** `id`
- **Loads:** by date window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. Product id, unique in the store. Join to order_products (product_id) and variants (product_id). |
| `name` | STRING | Name | Product name. |
| `sku` | STRING | SKU | Product-level SKU; variants carry their own. |
| `type` | STRING | Type | physical or digital. |
| `brand_id` | INTEGER | Brand ID | FK to [Brands](./brands.md). Brand. Join to brands (id); 0 for none. |
| `categories` | STRING | Categories | JSON list of the category ids the product is in. Join to categories (id) after UNNEST. |
| `price` | FLOAT | Price | Default price in the store's default currency. |
| `sale_price` | FLOAT | Sale Price | Sale price, 0 when not on sale. |
| `retail_price` | FLOAT | Retail Price | Retail (MSRP) price shown for comparison. |
| `calculated_price` | FLOAT | Calculated Price | Price a shopper pays now, after sale price and catalog rules. |
| `cost_price` | FLOAT | Cost Price | Cost of the product to the store; the base for margin. |
| `inventory_level` | INTEGER | Inventory Level | Units in stock when inventory is tracked at product level. |
| `inventory_tracking` | STRING | Inventory Tracking | none, product or variant: where stock is counted. |
| `is_visible` | BOOLEAN | Is Visible | true when the product is visible on the storefront. |
| `availability` | STRING | Availability | available, disabled or preorder. |
| `total_sold` | INTEGER | Total Sold | Units sold over the product's lifetime, as BigCommerce counts them. |
| `base_variant_id` | INTEGER | Base Variant ID | Variant that holds the product's default SKU. |
| `upc` | STRING | Upc | UPC barcode. |
| `gtin` | STRING | Gtin | GTIN barcode. |
| `mpn` | STRING | Mpn | Manufacturer part number. |
| `date_created` | DATETIME | Date Created | When the product was created (UTC, ISO 8601). |
| `date_modified` | DATETIME | Date Modified | When the product last changed (UTC, ISO 8601). The field the daily window filters on. |
| `product_url` | STRING | Product URL | Storefront path of the product page. |

# Example Questions

- Which products are in stock but have not sold in 60 days?
- Which `brands` and categories carry the most products and the most units sold?

## Joins

- [Brands](./brands.md) — `brand_id = id` [N:1]
- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
