---
title: "Revenue daily"
description: |
  Estimated revenue, ad revenue, YouTube Premium revenue, gross revenue, CPM, playback-based CPM, ad impressions and monetized playbacks per day. Rows exist only for channels in the YouTube Partner Program with the monetary scope granted.
tags: ["owox", "connector", "youtube"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.google.com/youtube/analytics/reference/reports/query"
    title: "OWOX Data Marts — YouTube custom connectors (YouTube on the Data API v3, 5 nodes; YouTube Analytics on the Analytics API v2, 7 nodes)"
  - id: "vendor-reference"
    resource: "https://developers.google.com/youtube/analytics/channel_reports#ad-performance-reports"
    title: "YouTube API reference — Revenue daily"
---

## Connector endpoint

- **Endpoint:** YouTube · `revenue_daily`
- **Default destination table:** `youtube_revenue_daily`
- **Unique keys:** `channel_id`, `day`
- **Loads:** by date window (`incremental: range`, 3-day lag)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `channel_id` | STRING | Channel ID | PK. FK to [Channel](./channel.md). The channel, stamped from the Channel ID parameter. Part of the key. |
| `day` | DATE | Day | PK. Day. Part of the key. |
| `estimated_revenue` | FLOAT | Estimated Revenue | Total estimated net revenue from ads and YouTube Premium, USD. |
| `estimated_ad_revenue` | FLOAT | Estimated Ad Revenue | Estimated net revenue from AdSense and DoubleClick ads, USD. |
| `estimated_red_partner_revenue` | FLOAT | Estimated Red Partner Revenue | Estimated revenue from YouTube Premium subscribers' watch time, USD. |
| `gross_revenue` | FLOAT | Gross Revenue | Estimated gross revenue from Google-sold ads before YouTube's share, USD. |
| `cpm` | FLOAT | Cpm | Gross revenue per 1,000 ad impressions, USD. |
| `playback_based_cpm` | FLOAT | Playback Based Cpm | Gross revenue per 1,000 monetized playbacks, USD. |
| `ad_impressions` | INTEGER | Ad Impressions | Verified ad impressions served. |
| `monetized_playbacks` | INTEGER | Monetized Playbacks | Playbacks where at least one ad impression was served. |

# Example Questions

- What did the channel earn per day and per 1,000 views, and how did CPM move?

## Joins

- [Channel daily](./channel-daily.md) — `channel_id = channel_id, day = day` [1:1] — Earnings for the same day; rows exist only for Partner Program channels.
- [Channel](./channel.md) — `channel_id = channel_id` [N:1] — The channel the day belongs to.
