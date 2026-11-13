---
title: "Advertiser Account"
description: |
  One row per advertiser account listed in the connector's Advertiser IDs: the advertiser ID, its name, company name and account currency. The connector reads it from the authorized ad accounts endpoint, whose documentation lists only the advertiser ID and name for each account, so `company_name` and `currency` are in the schema but are not documented for that endpoint.

  Every other table carries `advertiser_id`; this is the table to label them with the account name.
tags: ["owox", "connector", "tiktok-ads"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://github.com/OWOX/owox-data-marts/blob/a3766e5bc1053780a65b259d9515bedec3c9d782/packages/connectors/src/Sources/TikTokAds/TikTokAdsAPIReference/TikTokAdsFieldsSchema.js"
    title: "OWOX Data Marts TikTok Ads connector — Advertiser Account endpoint (`advertiser`)"
  - id: "vendor-reference"
    resource: "https://ads.tiktok.com/marketing_api/docs"
    title: "TikTok Business API reference — docs"
---

## Connector endpoint

- **Endpoint:** TikTok Ads · `advertiser`
- **Default destination table:** `tiktok_ads_advertiser`
- **Unique keys:** `advertiser_id`
- **Time series:** no, one current row per key

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `advertiser_id` | STRING | Advertiser ID | PK. ID of the advertiser account the row belongs to. |
| `advertiser_name` | STRING | Advertiser Name | Name of the advertiser account. |
| `company_name` | STRING | Company Name | Company name of the advertiser; TikTok's authorized ad accounts endpoint, which the connector reads, does not document this field. |
| `currency` | STRING | Currency | Account Currency |

# Example Questions

- Which advertiser accounts does this connector load, and what are their names?
- Which advertiser accounts belong to the same company?
- In which currency is each advertiser account set up?
