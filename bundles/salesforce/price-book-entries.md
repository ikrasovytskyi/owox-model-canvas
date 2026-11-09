---
title: "Price book entries"
description: |
  A product's list price in a given price book.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_pricebookentry.htm"
    title: "Salesforce API reference — Price book entries"
---

## Connector endpoint

- **Endpoint:** Salesforce · `pricebook_entry`
- **Default destination table:** `salesforce_pricebook_entry`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Price-book entry id. Primary key. |
| `name` | STRING | Name | Product name as shown in the book. |
| `pricebook_2_id` | STRING | Pricebook 2 ID | FK to [Price books](./price-books.md). Price book. |
| `product_2_id` | STRING | Product 2 ID | FK to [Products](./products.md). Product. |
| `product_code` | STRING | Product Code | Product code. |
| `unit_price` | FLOAT | Unit Price | List price per unit in this book. |
| `use_standard_price` | BOOLEAN | Use Standard Price | true when the entry inherits the standard book price. |
| `is_active` | BOOLEAN | Is Active | false for entries no longer offered. |
| `is_archived` | BOOLEAN | Is Archived | true for archived entries. |
| `is_deleted` | BOOLEAN | Is Deleted | true when in the Recycle Bin. |
| `created_date` | TIMESTAMP | Created Date | When the entry was created. |
| `last_modified_date` | TIMESTAMP | Last Modified Date | Last edit. |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change by anyone. |

# Example Questions

- What is the list price of each product in each book, and how far below it do we sell?

## Joins

- [Price books](./price-books.md) — `pricebook_2_id = id` [N:1]
- [Products](./products.md) — `product_2_id = id` [N:1]
