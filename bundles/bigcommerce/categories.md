---
title: "Categories"
description: |
  Every category in every category tree: name, parent and visibility, for grouping products and revenue.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/catalog/category-trees/categories/get-all-categories"
    title: "BigCommerce API reference — Categories"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `categories`
- **Default destination table:** `bigcommerce_categories`
- **Unique keys:** `id`, `tree_id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. Category id, unique in the store. |
| `tree_id` | INTEGER | Tree ID | PK. Category tree (one per storefront in a multi-storefront store). |
| `parent_id` | INTEGER | Parent ID | Parent category id; 0 for a top-level category. |
| `name` | STRING | Name | Category name. |
| `is_visible` | BOOLEAN | Is Visible | true when the category is shown on the storefront. |
| `sort_order` | INTEGER | Sort Order | Position among its siblings. |
| `category_url` | STRING | Category URL | Storefront path of the category page. |

# Example Questions

- Which categories bring the most revenue once order lines are joined through `products`?
- Which categories are hidden from the storefront but still sell?

## Joins

- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
