---
title: "Brands"
description: |
  Brand names for grouping products and revenue.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/catalog/brands/get-brands"
    title: "BigCommerce API reference — Brands"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `brands`
- **Default destination table:** `bigcommerce_brands`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. Brand id. Join to products (brand_id). |
| `name` | STRING | Name | Brand name. |
| `page_title` | STRING | Page Title | Title of the brand page. |
| `brand_url` | STRING | Brand URL | Storefront path of the brand page. |

# Example Questions

- Which brands bring the most revenue and the highest refund rate?
- How many products does each brand have?

## Joins

- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
