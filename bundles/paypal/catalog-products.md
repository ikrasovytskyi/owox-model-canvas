---
title: "Catalog products"
description: |
  Every catalog product that plans sell, with its type and category.
tags: ["owox", "connector", "paypal"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.paypal.com/docs/api/transaction-search/v1/"
    title: "OWOX Data Marts — PayPal custom connector (PayPal, 7 nodes) on the PayPal REST APIs: Transaction Search v1, Disputes v1, Invoicing v2, Subscriptions v1, Catalog Products v1"
  - id: "vendor-reference"
    resource: "https://developer.paypal.com/docs/api/catalog-products/v1/#products_get"
    title: "PayPal API reference — Catalog products"
---

## Connector endpoint

- **Endpoint:** PayPal · `products`
- **Default destination table:** `paypal_products`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Product id (PROD-…). |
| `account_id` | STRING | Account ID | Your PayPal merchant account id as entered in the connector (Account settings → Business information); lets several PayPal accounts share one table. Empty when not set. |
| `name` | STRING | Name | Product name. |
| `description` | STRING | Description | Product description. |
| `type` | STRING | Type | PHYSICAL, DIGITAL or SERVICE. |
| `category` | STRING | Category | PayPal's product category code, such as SOFTWARE or EDUCATIONAL_AND_TEXTBOOKS. |
| `image_url` | STRING | Image URL | Product image URL. |
| `home_url` | STRING | Home URL | Product page URL. |
| `create_time` | TIMESTAMP | Create Time | When the product was created (UTC). |
| `update_time` | TIMESTAMP | Update Time | When the product last changed (UTC). |

# Example Questions

- Which catalog products exist, and which ones have active plans?
