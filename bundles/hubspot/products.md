---
title: "Products"
description: |
  The product library: price, SKU, cost and billing period of every product.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/crm/products"
    title: "HubSpot API reference — Products"
---

## Connector endpoint

- **Endpoint:** HubSpot · `products`
- **Default destination table:** `hubspot_products`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. HubSpot object id of the products (hs_object_id), unique in the account. Primary key. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector; lets several HubSpot accounts share one table. |
| `name` | STRING | Name | Product name. |
| `description` | STRING | Description | Product description. |
| `price` | FLOAT | Price | Unit list price. |
| `hs_sku` | STRING | Hs SKU | SKU; the join to the product catalogue in Shopify, Stripe or QuickBooks. |
| `hs_cost_of_goods_sold` | FLOAT | Hs Cost Of Goods Sold | Unit cost of goods sold, when entered. |
| `hs_product_type` | STRING | Hs Product Type | inventory, non-inventory or service. |
| `hs_recurring_billing_period` | STRING | Hs Recurring Billing Period | Billing period for recurring products as an ISO 8601 duration (P1M, P12M). |
| `recurringbillingfrequency` | STRING | Recurringbillingfrequency | Billing frequency: monthly, quarterly, annually and others. |
| `createdate` | TIMESTAMP | Createdate | When the product was created. |
| `hs_lastmodifieddate` | TIMESTAMP | Hs Lastmodifieddate | When any product property last changed. |
| `hs_url` | STRING | Hs URL | Product URL. |
| `created_at` | TIMESTAMP | Created At | When HubSpot created the record (UTC). |
| `updated_at` | TIMESTAMP | Updated At | When any property of the record last changed (UTC). The field the daily window filters on. |
| `archived` | BOOLEAN | Archived | true when the record was deleted (archived) in HubSpot; archived records stop appearing in loads. |

# Example Questions

- Which products are sold most often (through line items), and at what average discount?
- What is the list price versus the realised price per SKU?
