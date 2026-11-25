---
title: "Customer groups"
description: |
  Customer groups such as retail and wholesale, with their discount rules: the field that separates B2B from B2C orders.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management/customers/v2/customer-groups/get-customer-groups"
    title: "BigCommerce API reference — Customer groups"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `customer_groups`
- **Default destination table:** `bigcommerce_customer_groups`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. Customer group id. Join to customers (customer_group_id). |
| `name` | STRING | Name | Group name, e.g. Wholesale. |
| `is_default` | BOOLEAN | Is Default | true for the group new customers join automatically. |
| `is_group_for_guests` | BOOLEAN | Is Group For Guests | true for the group guest shoppers are treated as. |
| `discount_rules` | STRING | Discount Rules | JSON list of the group's discount rules: type (all, category, product), method (percent, fixed, price), amount. |

# Example Questions

- How much do wholesale customers buy compared with retail ones?
- Which group discounts are set up, and how many customers are in each group?

## Joins

- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
