---
title: "Ad Account User"
description: |
  Named in the connector for the users assigned to an ad account. As the connector currently requests it, it returns the account's own `id` (`act_` plus the number) and `name`, one row per account.
tags: ["owox", "connector", "facebook-ads"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://github.com/OWOX/owox-data-marts/blob/a3766e5bc1053780a65b259d9515bedec3c9d782/packages/connectors/src/Sources/FacebookMarketing/MarketingAPIReference/FieldsSchema.js"
    title: "OWOX Data Marts Facebook Ads connector — Ad Account User endpoint (`ad-account-user`)"
  - id: "vendor-reference"
    resource: "https://developers.facebook.com/docs/marketing-api/reference/ad-account-user"
    title: "Meta Marketing API reference — ad-account-user"
---

## Connector endpoint

- **Endpoint:** Facebook Ads · `ad-account-user`
- **Default destination table:** `facebook_ads_ad_account_user`
- **Unique keys:** `id`
- **Time series:** no, one current row per key

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. As currently requested, the ad account's own ID in `act_<number>` form. FK to [Ad Account](./ad-account.md) |
| `name` | STRING | Name | As currently requested, the ad account's name. |

# Example Questions

- Which accounts does the connector read, by id and name?
- How many accounts are loaded?
- Do the names here match the names in Ad Account?

## Joins

- [Ad Account](./ad-account.md) — `id = id` [1:1] — As the connector requests it, this row is the account itself.
  - Description: As the connector requests it, this row is the account itself.
