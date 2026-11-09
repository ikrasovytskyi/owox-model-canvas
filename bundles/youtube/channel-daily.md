---
title: "Channel daily"
description: |
  One row per day for the whole channel: views, watch time, average view duration and percentage, likes, dislikes, comments, shares, subscribers gained and lost, playlist adds. The reference series every breakdown sums to, and the table to put beside Sessions and Unified Ad Spend by day. Loaded by date window with a 3-day lag so only settled days land.
tags: ["owox", "connector", "youtube"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.google.com/youtube/analytics/reference/reports/query"
    title: "OWOX Data Marts — YouTube custom connectors (YouTube on the Data API v3, 5 nodes; YouTube Analytics on the Analytics API v2, 7 nodes)"
  - id: "vendor-reference"
    resource: "https://developers.google.com/youtube/analytics/channel_reports#basic-stats"
    title: "YouTube API reference — Channel daily"
---

## Connector endpoint

- **Endpoint:** YouTube · `channel_daily`
- **Default destination table:** `youtube_channel_daily`
- **Unique keys:** `channel_id`, `day`
- **Loads:** by date window (`incremental: range`, 3-day lag)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `channel_id` | STRING | Channel ID | PK. FK to [Channel](./channel.md). The channel, stamped from the Channel ID parameter. Part of the key. |
| `day` | DATE | Day | PK. Day in the channel's time zone as YouTube Analytics reports it. Part of the key. |
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
| `videos_added_to_playlists` | INTEGER | Videos Added To Playlists | Times viewers added the channel's videos to playlists that day. |
| `videos_removed_from_playlists` | INTEGER | Videos Removed From Playlists | Times viewers removed them from playlists that day. |

# Example Questions

- Are views, watch time and subscribers up this month against last, week by week?
- What is the channel's average view percentage trend: are people watching longer or dropping earlier?
- How does watch time compare with ad spend as a top-of-funnel input, month by month?

## Joins

- [Channel](./channel.md) — `channel_id = channel_id` [N:1] — One row per channel and day; the channel row holds the lifetime counters, this table the day.
