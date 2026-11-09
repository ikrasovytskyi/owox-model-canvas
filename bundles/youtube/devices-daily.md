---
title: "Devices daily"
description: |
  Views and watch time per day, device type and operating system. The table that tells you whether the audience watches on a phone or at a desk.
tags: ["owox", "connector", "youtube"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.google.com/youtube/analytics/reference/reports/query"
    title: "OWOX Data Marts — YouTube custom connectors (YouTube on the Data API v3, 5 nodes; YouTube Analytics on the Analytics API v2, 7 nodes)"
  - id: "vendor-reference"
    resource: "https://developers.google.com/youtube/analytics/channel_reports#device-type-and-operating-system"
    title: "YouTube API reference — Devices daily"
---

## Connector endpoint

- **Endpoint:** YouTube · `devices_daily`
- **Default destination table:** `youtube_devices_daily`
- **Unique keys:** `channel_id`, `day`, `device_type`, `operating_system`
- **Loads:** by date window (`incremental: range`, 3-day lag)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `channel_id` | STRING | Channel ID | PK. FK to [Channel](./channel.md). The channel, stamped from the Channel ID parameter. Part of the key. |
| `day` | DATE | Day | PK. Day. Part of the key. |
| `device_type` | STRING | Device Type | PK. DESKTOP, MOBILE, TABLET, TV, GAME_CONSOLE or UNKNOWN_PLATFORM. Part of the key. |
| `operating_system` | STRING | Operating System | PK. ANDROID, IOS, WINDOWS, MACINTOSH, LINUX, CHROMECAST, SMART_TV, PLAYSTATION, XBOX and others. Part of the key. |
| `views` | INTEGER | Views | Views that day on that device and OS. |
| `estimated_minutes_watched` | INTEGER | Estimated Minutes Watched | Watch time that day, minutes. |

# Example Questions

- Do viewers watch on a phone or at a desk, and does average watch time differ by device?

## Joins

- [Channel daily](./channel-daily.md) — `channel_id = channel_id, day = day` [N:1] — Sums to the day's views.
- [Channel](./channel.md) — `channel_id = channel_id` [N:1] — The channel the day belongs to.
