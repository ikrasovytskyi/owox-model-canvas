---
title: "Order statuses"
description: |
  The 15 order statuses with their names and the store's own labels. The lookup that says which orders count as sales.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management/orders/order-status/get-order-statuses"
    title: "BigCommerce API reference — Order statuses"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `order_statuses`
- **Default destination table:** `bigcommerce_order_statuses`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. Status id, 0 to 14, as in orders.status_id. |
| `name` | STRING | Name | Status name shown in the control panel. |
| `system_label` | STRING | System Label | BigCommerce's own name for the status. |
| `custom_label` | STRING | Custom Label | The label the store gave the status, if renamed. |
| `system_description` | STRING | System Description | BigCommerce's description of what the status means. |

# Example Questions

- Which order statuses does our sales number include, and how many orders sit in each one?
- How many orders are Incomplete, Cancelled or Declined this month?

## Joins

- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
