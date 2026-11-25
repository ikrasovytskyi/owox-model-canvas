---
title: "Store"
description: |
  One row with the store's settings: name, domain, default currency, time zone, plan and default channel. Read it to report in the store's currency and to shift UTC timestamps into the store's day.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management/store-information/v2/get-store-information"
    title: "BigCommerce API reference — Store"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `store`
- **Default destination table:** `bigcommerce_store`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | STRING | ID | PK. Store hash; the same value as store_hash. |
| `store_id` | INTEGER | Store ID | Numeric store id. |
| `name` | STRING | Name | Store name. |
| `domain` | STRING | Domain | Primary domain. |
| `secure_url` | STRING | Secure URL | HTTPS address of the storefront. |
| `currency` | STRING | Currency | Default currency (ISO 4217). |
| `decimal_places` | INTEGER | Decimal Places | Decimal places the store shows prices with. |
| `timezone_name` | STRING | Timezone Name | Store time zone, e.g. America/Chicago. |
| `timezone_offset` | INTEGER | Timezone Offset | Time zone offset from UTC in seconds. |
| `language` | STRING | Language | Storefront language. |
| `plan_name` | STRING | Plan Name | BigCommerce plan name. |
| `plan_level` | STRING | Plan Level | BigCommerce plan level. |
| `plan_is_trial` | BOOLEAN | Plan Is Trial | true while the store is on a free trial. |
| `default_channel_id` | INTEGER | Default Channel ID | Id of the default storefront channel. |
| `is_price_entered_with_tax` | BOOLEAN | Is Price Entered With Tax | true when catalog prices are entered including tax. |
| `weight_units` | STRING | Weight Units | Weight unit of the catalog. |

# Example Questions

- What is the store's default currency and time zone, so daily totals line up with BigCommerce's own reports?
- Which plan is the store on?
