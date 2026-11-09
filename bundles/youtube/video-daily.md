---
title: "Video daily"
description: |
  Per video and day: views, watch time, average view duration and percentage, likes, dislikes, comments, shares, subscribers gained and lost. One request per day returns the day's 200 most-viewed videos, so every video with views that day is present. The table for which video drove this week and for a video's first-week curve.
tags: ["owox", "connector", "youtube"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.google.com/youtube/analytics/reference/reports/query"
    title: "OWOX Data Marts — YouTube custom connectors (YouTube on the Data API v3, 5 nodes; YouTube Analytics on the Analytics API v2, 7 nodes)"
  - id: "vendor-reference"
    resource: "https://developers.google.com/youtube/analytics/channel_reports#top-videos-reports"
    title: "YouTube API reference — Video daily"
---

## Connector endpoint

- **Endpoint:** YouTube · `video_daily`
- **Default destination table:** `youtube_video_daily`
- **Unique keys:** `channel_id`, `video_id`, `day`
- **Loads:** by date window (`incremental: day-by-day`, 3-day lag)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `channel_id` | STRING | Channel ID | PK. FK to [Channel](./channel.md). The channel, stamped from the Channel ID parameter. Part of the key. |
| `day` | DATE | Day | PK. The day, stamped from the request window. Part of the key. |
| `video_id` | STRING | Video ID | PK. FK to [Videos](./videos.md). The video. Part of the key; joins youtube_videos. |
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

- Which videos drove this week's views and watch time, and which ones hold attention (average view percentage) rather than just getting clicks?
- What does a video's first 14 days look like, and did the one we published last week beat the channel's typical curve?
- Which videos gained subscribers per 1,000 views?

## Joins

- [Videos](./videos.md) — `video_id = video_id` [N:1] — Title, publish date and duration of the video; days since publish = day minus published_at.
- [Channel daily](./channel-daily.md) — `channel_id = channel_id, day = day` [N:1] — The day's channel total the video rows add up to (top 200 videos per day).
- [Channel](./channel.md) — `channel_id = channel_id` [N:1] — The channel the day belongs to.
