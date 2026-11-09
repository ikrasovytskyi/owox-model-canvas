---
title: "Geography daily"
description: |
  Views, watch time, retention and engagement per country and day. One request per day; the table for where the audience is and whether it matches the markets you sell in.
tags: ["owox", "connector", "youtube"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.google.com/youtube/analytics/reference/reports/query"
    title: "OWOX Data Marts — YouTube custom connectors (YouTube on the Data API v3, 5 nodes; YouTube Analytics on the Analytics API v2, 7 nodes)"
  - id: "vendor-reference"
    resource: "https://developers.google.com/youtube/analytics/channel_reports#user-geography"
    title: "YouTube API reference — Geography daily"
---

## Connector endpoint

- **Endpoint:** YouTube · `geography_daily`
- **Default destination table:** `youtube_geography_daily`
- **Unique keys:** `channel_id`, `day`, `country`
- **Loads:** by date window (`incremental: day-by-day`, 3-day lag)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `channel_id` | STRING | Channel ID | PK. FK to [Channel](./channel.md). The channel, stamped from the Channel ID parameter. Part of the key. |
| `day` | DATE | Day | PK. The day, stamped from the request window. Part of the key. |
| `country` | STRING | Country | PK. Viewer country, ISO 3166-1 alpha-2; ZZ when unknown. Part of the key. |
| `views` | INTEGER | Views | Views on that day (YouTube's counted views, not impressions). |
| `estimated_minutes_watched` | INTEGER | Estimated Minutes Watched | Watch time in minutes. The metric YouTube ranks by; views × average duration. |
| `average_view_duration` | INTEGER | Average View Duration | Average seconds a view lasted. |
| `average_view_percentage` | FLOAT | Average View Percentage | Average share of the video watched, in percent: the retention proxy. |
| `likes` | INTEGER | Likes | Likes added that day. |
| `dislikes` | INTEGER | Dislikes | Dislikes added that day (visible to the owner only). |
| `comments` | INTEGER | Comments | Comments posted that day. |
| `shares` | INTEGER | Shares | Shares through the Share button that day. |
| `subscribers_gained` | INTEGER | Subscribers Gained | Subscribers gained from this content that day. |
| `subscribers_lost` | INTEGER | Subscribers Lost | Subscribers lost that day. |

# Example Questions

- Where is the audience, country by country, and does it match the markets we sell in?
- In which countries do viewers watch the largest share of a video?

## Joins

- [Channel daily](./channel-daily.md) — `channel_id = channel_id, day = day` [N:1] — Sums to the day's views; ZZ is the unknown country.
- [Channel](./channel.md) — `channel_id = channel_id` [N:1] — The channel the day belongs to.
