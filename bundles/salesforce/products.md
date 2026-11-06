---
title: "Products"
description: |
  The product catalogue as sales sells it: name, code, family, SKU, active flag.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_product2.htm"
    title: "Salesforce API reference — Products"
---

## Connector endpoint

- **Endpoint:** Salesforce · `product`
- **Default destination table:** `salesforce_product`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Product id. Primary key. |
| `name` | STRING | Name | Product name. |
| `product_code` | STRING | Product Code | Internal product code; the join key to your ERP or e-commerce catalogue. |
| `family` | STRING | Family | Product family (category) picklist. |
| `stock_keeping_unit` | STRING | Stock Keeping Unit | SKU. |
| `quantity_unit_of_measure` | STRING | Quantity Unit Of Measure | Unit the quantity is sold in. |
| `external_id` | STRING | External ID | Id in an external system. |
| `display_url` | STRING | Display URL | Product page URL. |
| `is_active` | BOOLEAN | Is Active | false for products no longer sold. |
| `is_archived` | BOOLEAN | Is Archived | true for archived products. |
| `is_deleted` | BOOLEAN | Is Deleted | true when in the Recycle Bin. |
| `description` | STRING | Description | Product description. |
| `created_date` | TIMESTAMP | Created Date | When the product was created. |
| `last_modified_date` | TIMESTAMP | Last Modified Date | Last edit. |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change by anyone. |

# Example Questions

- Which products appear on open deals, and which have not been sold this year?
