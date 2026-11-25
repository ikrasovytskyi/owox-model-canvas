---
title: "Channels"
description: |
  The store's sales channels: storefronts, marketplaces (Amazon, eBay, Walmart), point of sale and social shops. Orders and customers point at them.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management/channels/get-channels"
    title: "BigCommerce API reference — Channels"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `channels`
- **Default destination table:** `bigcommerce_channels`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. Channel id; 1 is the default storefront. Join to orders (channel_id) and customers (origin_channel_id). |
| `name` | STRING | Name | Channel name. |
| `type` | STRING | Type | storefront, marketplace, pos or marketing. |
| `platform` | STRING | Platform | Platform behind the channel, e.g. bigcommerce, amazon, ebay, facebook, google_shopping, square. |
| `status` | STRING | Status | active, inactive, prelaunch, connected, disconnected, archived, deleted or terminated. |
| `external_id` | STRING | External ID | The channel's id on the external platform. |
| `is_visible` | BOOLEAN | Is Visible | true when the channel shows in the control panel. |
| `date_created` | DATETIME | Date Created | When the channel was created (UTC, ISO 8601). |
| `date_modified` | DATETIME | Date Modified | When the channel last changed (UTC, ISO 8601). |

# Example Questions

- What share of orders comes from marketplaces against the storefront?
- Which channels are active, and when were they connected?

## Joins

- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
