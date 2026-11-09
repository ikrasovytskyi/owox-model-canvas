---
title: "Opportunity line items"
description: |
  Products on deals: product, quantity, unit and total price, and the price-book entry they were priced from.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_opportunitylineitem.htm"
    title: "Salesforce API reference — Opportunity line items"
---

## Connector endpoint

- **Endpoint:** Salesforce · `opportunity_line_item`
- **Default destination table:** `salesforce_opportunity_line_item`
- **Unique keys:** `id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Salesforce record id of the line item (18 characters, unique across the org). Primary key. |
| `created_date` | TIMESTAMP | Created Date | When the record was created (UTC). |
| `last_modified_date` | TIMESTAMP | Last Modified Date | Last edit by a user or an integration (UTC). |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change by anyone, including Salesforce itself (UTC). The field the daily window filters on; use it, not LastModifiedDate, to decide what to reload. |
| `is_deleted` | BOOLEAN | Is Deleted | true when the record sits in the Recycle Bin; the connector reads through queryAll so deletions reach the data warehouse instead of silently vanishing. |
| `opportunity_id` | STRING | Opportunity ID | FK to [Opportunities](./opportunities.md). Deal the line belongs to. |
| `product_2_id` | STRING | Product 2 ID | FK to [Products](./products.md). Product sold. |
| `pricebook_entry_id` | STRING | Pricebook Entry ID | FK to [Price book entries](./price-book-entries.md). Price-book entry the line was priced from. |
| `name` | STRING | Name | Line name (deal name + product name). |
| `product_code` | STRING | Product Code | Product code copied from the product. |
| `quantity` | FLOAT | Quantity | Units sold. |
| `unit_price` | FLOAT | Unit Price | Sales price per unit after any discount. |
| `total_price` | FLOAT | Total Price | quantity × unit_price; sums to the deal amount when products are used. |
| `list_price` | FLOAT | List Price | List price per unit from the price book; discount = 1 − unit_price / list_price. |
| `service_date` | DATE | Service Date | Service or delivery date, if used. |
| `sort_order` | INTEGER | Sort Order | Line order on the deal. |
| `description` | STRING | Description | Line description. |

# Example Questions

- Which `products` drive won revenue, and at what discount against list price?
- How many products per won deal, and does the mix differ by segment?

## Joins

- [Opportunities](./opportunities.md) — `opportunity_id = id` [N:1]
- [Products](./products.md) — `product_2_id = id` [N:1]
- [Price book entries](./price-book-entries.md) — `pricebook_entry_id = id` [N:1] — List price the line was priced from.
