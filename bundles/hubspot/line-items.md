---
title: "Line items"
description: |
  Every line item on every deal: product, quantity, price, discount, amount and recurring values, with the deal it belongs to. Deal amount by product and MRR by product come from here.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/crm/line-items"
    title: "HubSpot API reference — Line items"
---

## Connector endpoint

- **Endpoint:** HubSpot · `line_items`
- **Default destination table:** `hubspot_line_items`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. HubSpot object id of the line items (hs_object_id), unique in the account. Primary key. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector; lets several HubSpot accounts share one table. |
| `name` | STRING | Name | Line item name (product name at the time of adding). |
| `hs_product_id` | STRING | Hs Product ID | FK to [Products](./products.md). Product this line was created from; joins hubspot_products.id. |
| `hs_sku` | STRING | Hs SKU | SKU copied from the product. |
| `quantity` | FLOAT | Quantity | Quantity. |
| `price` | FLOAT | Price | Unit price on the line. |
| `amount` | FLOAT | Amount | Net amount of the line (quantity × price minus discount). |
| `discount` | FLOAT | Discount | Absolute discount on the line. |
| `hs_discount_percentage` | FLOAT | Hs Discount Percentage | Percentage discount on the line. |
| `hs_total_discount` | FLOAT | Hs Total Discount | Total discount on the line. |
| `hs_line_item_currency_code` | STRING | Hs Line Item Currency Code | ISO currency code of the line. |
| `hs_recurring_billing_period` | STRING | Hs Recurring Billing Period | Billing period of a recurring line as an ISO 8601 duration. |
| `recurringbillingfrequency` | STRING | Recurringbillingfrequency | Billing frequency of a recurring line. |
| `hs_term_in_months` | INTEGER | Hs Term In Months | Contract term in months for recurring lines. |
| `hs_mrr` | FLOAT | Hs Mrr | Monthly recurring revenue of the line. |
| `hs_arr` | FLOAT | Hs Arr | Annual recurring revenue of the line. |
| `hs_tcv` | FLOAT | Hs Tcv | Total contract value of the line. |
| `hs_acv` | FLOAT | Hs Acv | Annual contract value of the line. |
| `hs_position_on_quote` | INTEGER | Hs Position On Quote | Order of the line on the deal or quote. |
| `createdate` | TIMESTAMP | Createdate | When the line was created. |
| `hs_lastmodifieddate` | TIMESTAMP | Hs Lastmodifieddate | When the line last changed. |
| `deal_id` | STRING | Deal ID | FK to [Deals](./deals.md). The deal this line item belongs to (first associated deal; a line item belongs to one deal). Joins hubspot_deals.id. |
| `created_at` | TIMESTAMP | Created At | When HubSpot created the record (UTC). |
| `updated_at` | TIMESTAMP | Updated At | When any property of the record last changed (UTC). The field the daily window filters on. |
| `archived` | BOOLEAN | Archived | true when the record was deleted (archived) in HubSpot; archived records stop appearing in loads. |

# Example Questions

- What is deal amount by product and by SKU this quarter?
- How much MRR and ARR sits in open deals, by product?
- Which deals carry the deepest discounts?

## Joins

- [Deals](./deals.md) — `deal_id = id` [N:1] — Deal amount by product; MRR by product.
- [Products](./products.md) — `hs_product_id = id` [N:1]
