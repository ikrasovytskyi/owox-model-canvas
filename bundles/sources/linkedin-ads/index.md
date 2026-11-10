---
title: "LinkedIn Ads"
description: |
  What the OWOX Data Marts LinkedIn Ads connector loads, and how the pieces fit together. A LinkedIn ad account holds campaign groups, campaign groups hold campaigns, and campaigns hold creatives. The connector loads the settings of the account, its campaign groups, campaigns and creatives as current-state tables, one row per entity, and the daily performance as time-series tables: [Ad Analytics](./ad-analytics.md) by creative, campaign, campaign group and account, and [Ad Analytics by Country](./ad-analytics-by-country.md), [Ad Analytics by Region](./ad-analytics-by-region.md) and [Ad Analytics by County](./ad-analytics-by-county.md) by the location of the members who saw the ads.

  LinkedIn refers to its entities by URN, not by the plain ID in each table's own `id` column. [Campaigns](./campaigns.md) hold their account in `account` and their group in `campaignGroup`, and [Creatives](./creatives.md) hold their campaign in `campaign`, each as a URN such as `urn:li:sponsoredAccount:{id}`. [Ad Analytics](./ad-analytics.md) goes one step further: it has no campaign or creative column at all, only `pivotValues`, an array of the URNs the row is grouped by. Because a URN is not the same string as an `id`, this bundle draws no joins between the tables; to link them, extract the identifier from the URN first. The three location tables carry `account` and `campaign` as URNs too.

  Two caution notes apply. Budget and unit cost fields in Campaigns and Campaign Groups are JSON objects, and the amounts in them are strings. In Ad Analytics the `viral` fields count the same metric on viral impressions, those that come from users sharing sponsored content with their own network, and viral impressions are not counted as regular impressions.
tags: ["owox", "index", "connector", "linkedin-ads"]
type: "index"
sources:
  - id: "owox-connector"
    resource: "https://github.com/OWOX/owox-data-marts/tree/6b104bfa5980750dc37865eae15712577f42f307/packages/connectors/src/Sources/LinkedInAds"
    title: "OWOX Data Marts — LinkedIn Ads connector source"
---

8 tables, 475 fields, 0 joins.

| Data Mart | Fields | Description |
|-----------|--------|-------------|
| [Ad Accounts](./ad-accounts.md) | 14 | Your ad account settings, billing details, currency, and account status. |
| [Ad Analytics](./ad-analytics.md) | 104 | Daily performance metrics for your campaigns — impressions, clicks, spend, and conversions. |
| [Ad Analytics by Country](./ad-analytics-by-country.md) | 100 | Daily campaign performance by the country of the members who saw your ads — impressions, clicks, spend, and conversions per country. |
| [Ad Analytics by County](./ad-analytics-by-county.md) | 100 | Daily campaign performance by the county of the members who saw your ads — impressions, clicks, spend, and conversions per county. |
| [Ad Analytics by Region](./ad-analytics-by-region.md) | 100 | Daily campaign performance by the region of the members who saw your ads — impressions, clicks, spend, and conversions per region. |
| [Campaign Groups](./campaign-groups.md) | 14 | Top-level containers that group campaigns with shared objectives, budgets, and scheduling. |
| [Campaigns](./campaigns.md) | 29 | Individual campaigns defining your targeting, ad format, bid strategy, and budget. |
| [Creatives](./creatives.md) | 14 | The visual and copy elements of your ads shown to members across LinkedIn. |

# Example Questions

- Which campaigns are active, and which objective type does each pursue?
- How do impressions, clicks and cost in local currency move day by day across the account?
- Which creatives are meant to be active but are not serving, and why?

# Explore this model

**[▶ Explore on canvas](https://model.owox.com/?okf=https://github.com/ikrasovytskyi/owox-model-canvas/tree/main/bundles/sources/linkedin-ads)**

One click opens this model in a free OWOX canvas you can poke around in — no account needed.

## Model preview

![LinkedIn Ads model diagram](../../res/screens/sources/linkedin-ads.svg)
