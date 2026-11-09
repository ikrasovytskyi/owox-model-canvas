---
title: "Traffic sources daily"
description: |
  Views and watch time per day and traffic source type: YouTube search, suggested videos, browse (home and subscriptions), external links, playlists, channel pages, notifications, Shorts feed, advertising. Sums to the day's views; the table that says whether the audience came through YouTube or through you.
tags: ["owox", "connector", "youtube"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.google.com/youtube/analytics/reference/reports/query"
    title: "OWOX Data Marts — YouTube custom connectors (YouTube on the Data API v3, 5 nodes; YouTube Analytics on the Analytics API v2, 7 nodes)"
  - id: "vendor-reference"
    resource: "https://developers.google.com/youtube/analytics/channel_reports#traffic-source"
    title: "YouTube API reference — Traffic sources daily"
---

## Connector endpoint

- **Endpoint:** YouTube · `traffic_sources_daily`
- **Default destination table:** `youtube_traffic_sources_daily`
- **Unique keys:** `channel_id`, `day`, `traffic_source_type`
- **Loads:** by date window (`incremental: range`, 3-day lag)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `channel_id` | STRING | Channel ID | PK. FK to [Channel](./channel.md). The channel, stamped from the Channel ID parameter. Part of the key. |
| `day` | DATE | Day | PK. Day. Part of the key. |
| `traffic_source_type` | STRING | Traffic Source Type | PK. YouTube's traffic source type: YT_SEARCH, RELATED_VIDEO, SUBSCRIBER (browse: home and subscriptions), EXT_URL, PLAYLIST, YT_CHANNEL, NOTIFICATION, SHORTS, ADVERTISING, NO_LINK_OTHER, END_SCREEN, ANNOTATION, YT_OTHER_PAGE, PROMOTED, CAMPAIGN_CARD, PLAYLIST_PAGE, HASHTAGS, SOUND_PAGE, LIVE_REDIRECT, VIDEO_REMIXES, PRODUCT_PAGE. Part of the key. |
| `views` | INTEGER | Views | Views from that source that day. |
| `estimated_minutes_watched` | INTEGER | Estimated Minutes Watched | Watch time from that source that day, minutes. |

# Example Questions

- Do people find the channel through YouTube search and suggestions, or do we bring them ourselves (external links, newsletters, ads)?
- Did the newsletter day show up as an EXT_URL spike?

## Joins

- [Channel daily](./channel-daily.md) — `channel_id = channel_id, day = day` [N:1] — Sums to the day's views exactly.
- [Channel](./channel.md) — `channel_id = channel_id` [N:1] — The channel the day belongs to.
