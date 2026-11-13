---
title: "Facebook Ads"
description: |
  What the OWOX Data Marts Facebook Ads connector loads from Meta, and how the pieces fit
  together. Every table belongs to one ad account. Under it, ads sit in ad sets and campaigns
  and each ad runs one creative from the account's library; the performance tables report
  each ad day by day, either whole or split by audience, place, device, product or link.

  The account is the anchor. The entity tables [Ads](./ads.md) and
  [Ad Creatives](./ad-creatives.md) and the three unsplit performance tables
  ([Ad Insights](./ad-insights.md), [Ad Insights by Ad Set](./ad-insights-by-ad-set.md) and
  [Ad Insights by Campaign](./ad-insights-by-campaign.md)) carry `account_id`, the bare
  account number, and join to [Ad Account](./ad-account.md) on it. The breakdown tables reach
  the account through Ad Insights, [Ad](./ad.md) through Ads, and
  [Ad Account User](./ad-account-user.md) joins it on `id`, the `act_` form it carries.
  Performance reaches an ad's name, status and creative through [Ads](./ads.md) on `ad_id`.
  Campaigns and ad sets have no table of their own in this connector — their IDs and names
  travel on every performance row, and their deduplicated reach comes from the two
  level-specific tables.

  Two cautions matter more than any single field. **Reach and frequency do not add up**:
  summing them across ads, days or breakdown values counts the same person several times;
  take them from [Ad Insights by Ad Set](./ad-insights-by-ad-set.md) or
  [Ad Insights by Campaign](./ad-insights-by-campaign.md). And **the ad tables are snapshots of what
  exists now**: Meta omits deleted and archived ads, so older performance rows can point at
  an ad that is no longer listed.
tags: ["owox", "index", "connector", "facebook-ads"]
type: "index"
sources:
  - id: "owox-connector"
    resource: "https://github.com/OWOX/owox-data-marts/tree/a3766e5bc1053780a65b259d9515bedec3c9d782/packages/connectors/src/Sources/FacebookMarketing"
    title: "OWOX Data Marts — Facebook Ads connector source"
---

15 tables, 1174 fields, 18 joins.

| Data Mart | Fields | Description |
|-----------|--------|-------------|
| [Ad](./ad.md) | 32 | Individual ads with their creative, status, and links to their campaign and ad set. |
| [Ad Account](./ad-account.md) | 61 | Your ad account settings, status, currency, and billing details. |
| [Ad Account User](./ad-account-user.md) | 2 | Users with access to an ad account and their assigned roles. |
| [Ad Creatives](./ad-creatives.md) | 65 | The images, videos, text, and other visual elements used in your ads. |
| [Ad Insights](./ad-insights.md) | 100 | Daily performance metrics for each ad — impressions, clicks, spend, and conversions. |
| [Ad Insights by Ad Set](./ad-insights-by-ad-set.md) | 92 | Daily ad set performance with reach deduplicated to match Meta Ads Manager. |
| [Ad Insights by Age and Gender](./ad-insights-by-age-and-gender.md) | 102 | Daily ad performance broken down by audience age range and gender. |
| [Ad Insights by Campaign](./ad-insights-by-campaign.md) | 89 | Daily campaign performance with reach deduplicated to match Meta Ads Manager. |
| [Ad Insights by Country](./ad-insights-by-country.md) | 101 | Daily ad performance broken down by the country where your audience is located. |
| [Ad Insights by Device Platform](./ad-insights-by-device-platform.md) | 101 | Daily ad performance broken down by the device type your audience used. |
| [Ad Insights by Link URL Asset](./ad-insights-by-link-url-asset.md) | 101 | Daily ad performance broken down by the link URL shown in your ads. |
| [Ad Insights by Product ID](./ad-insights-by-product-id.md) | 101 | Daily ad performance broken down by individual product from your catalog. |
| [Ad Insights by Publisher Platform and Position](./ad-insights-by-publisher-platform-and-position.md) | 102 | Daily ad performance broken down by where your ads appeared — platform and placement position. |
| [Ad Insights by Region](./ad-insights-by-region.md) | 101 | Daily ad performance broken down by the region where your audience is located. |
| [Ads](./ads.md) | 24 | Individual ads with their status, creative, and campaign hierarchy. |

# Example Questions

- Which campaigns spend the most for the fewest results, once spend is divided by results?
- Which creatives drive the cheapest conversions, once performance is joined to the creative each ad runs?
- How many people did each campaign reach on a given day, and how often did they see the ads?

# Explore this model

**[▶ Explore on canvas](https://model.owox.com/?okf=https://github.com/ikrasovytskyi/owox-model-canvas/tree/main/bundles/sources/facebook-ads)**

One click opens this model in a free OWOX canvas you can poke around in — no account needed.

## Model preview

![Facebook Ads model diagram](../../res/screens/sources/facebook-ads.svg)
