---
title: "Ad Performance by Country"
description: |
  Daily performance of promoted tweets split by country: the same impressions, clicks, URL clicks, engagements, billed charge and social actions as Ad Performance, with `country` added to the key. One row per promoted tweet (`id`), `date`, `placement` and `country`.

  The connector loads it with the X Ads asynchronous stats jobs, one job per set of promoted tweets and placement, segmented by location. `country` holds a location ID, a hex string, not a name or a country code. Join `id` to Promoted Tweets for the promoted tweet. Join `country` to `targeting_value` in [Targeting Locations](./targeting-locations.md) for the name. The connector describes that table as the lookup for these IDs; the current X Ads docs no longer list the location segmentation as enabled, so check that the table has rows.
tags: ["owox", "connector", "x-ads"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://github.com/OWOX/owox-data-marts/blob/a3766e5bc1053780a65b259d9515bedec3c9d782/packages/connectors/src/Sources/XAds/XAdsAPIReference/XAdsFieldsSchema.js"
    title: "OWOX Data Marts X Ads connector — Ad Performance by Country endpoint (`stats_by_country`)"
  - id: "vendor-reference"
    resource: "https://developer.x.com/en/docs/x-ads-api/analytics/api-reference/asynchronous"
    title: "X Ads API reference — asynchronous"
---

## Connector endpoint

- **Endpoint:** X Ads · `stats_by_country`
- **Default destination table:** `x_ads_stats_by_country`
- **Unique keys:** `id`, `date`, `placement`, `country`
- **Time series:** yes, one row per key per `date`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. The unique identifier for the stats record. FK to [Promoted Tweets](./promoted-tweets.md) |
| `date` | STRING | Date | PK. The date for which the statistics were collected. |
| `placement` | STRING | Placement | PK. The placement type (ALL_ON_TWITTER or PUBLISHER_NETWORK). |
| `country` | STRING | Country | PK. Location ID from X Ads (hex string, e.g. 96683cc9126741d1). Join with the targeting_locations node to get the human-readable name and country code. FK to [Targeting Locations](./targeting-locations.md) |
| `impressions` | INTEGER | Impressions | Times the promoted tweet was shown; X's engagement, click and follow rates divide by it. |
| `tweets_send` | INTEGER | Tweets Send | Number of tweets sent. |
| `billed_charge_local_micro` | INTEGER | Billed Charge Local Micro | Billed amount in micros. |
| `qualified_impressions` | INTEGER | Qualified Impressions | Qualified impressions of the promoted tweet; X's qualified impression rate is this divided by `impressions`. |
| `follows` | INTEGER | Follows | Follows that came from the promoted tweet; X's cost per follow is `billed_charge_local_micro` divided by this. |
| `app_clicks` | INTEGER | App Clicks | App install or app open attempts from the promoted tweet. |
| `retweets` | INTEGER | Retweets | Reposts (retweets) of the promoted tweet. |
| `unfollows` | INTEGER | Unfollows | Unfollows; the current X Ads metrics table no longer lists it, and X's example responses return it as null. |
| `likes` | INTEGER | Likes | Likes on the promoted tweet. |
| `engagements` | INTEGER | Engagements | Engagements with the promoted tweet as X sums them (clicks, replies, reposts, follows and more); X's engagement rate is this divided by `impressions`. |
| `clicks` | INTEGER | Clicks | Clicks on the promoted tweet, including likes (favorites) and other engagements, per X; `url_clicks` counts only link clicks. |
| `card_engagements` | INTEGER | Card Engagements | Engagements with the card attached to the promoted tweet. |
| `poll_card_vote` | INTEGER | Poll Card Vote | Number of poll card votes. |
| `replies` | INTEGER | Replies | Replies to the promoted tweet. |
| `url_clicks` | INTEGER | URL Clicks | Clicks on the link or Website Card in the ad, including earned clicks. |
| `billed_engagements` | INTEGER | Billed Engagements | Engagements X billed, from its BILLING metric group, as opposed to all `engagements`. |
| `carousel_swipes` | INTEGER | Carousel Swipes | Swipes on carousel images or videos. |

# Example Questions

- Which countries bring the most clicks, and at what billed charge per click?
- Do promoted tweets get more engagements per impression in some countries than in others?
- Which promoted tweets reach more than one country?

## Joins

- [Promoted Tweets](./promoted-tweets.md) — `id = id` [N:1] — The promoted tweet the country row reports on.
  - Description: The promoted tweet the country row reports on.
- [Targeting Locations](./targeting-locations.md) — `country = targeting_value` [N:1] — The location the row is split by: country holds a location ID, not a name. The current X docs no longer list the LOCATIONS segmentation as enabled, so rows may be missing.
  - Description: The location the row is split by: country holds a location ID, not a name. The current X docs no longer list the LOCATIONS segmentation as enabled, so rows may be missing.
