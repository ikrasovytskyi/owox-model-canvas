---
title: "Channel"
description: |
  The channel itself, one row rewritten on every run: title, handle, country, creation date and the lifetime view, subscriber and video counts the channel page shows. The anchor every other table joins to on channel_id; its counters are snapshots, the daily numbers live in Channel daily.
tags: ["owox", "connector", "youtube"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.google.com/youtube/analytics/reference/reports/query"
    title: "OWOX Data Marts — YouTube custom connectors (YouTube on the Data API v3, 5 nodes; YouTube Analytics on the Analytics API v2, 7 nodes)"
  - id: "vendor-reference"
    resource: "https://developers.google.com/youtube/v3/docs/channels/list"
    title: "YouTube API reference — Channel"
---

## Connector endpoint

- **Endpoint:** YouTube · `channels`
- **Default destination table:** `youtube_channel`
- **Unique keys:** `channel_id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `channel_id` | STRING | Channel ID | PK. The channel ID (UC…). Primary key, and the join key to every other YouTube table. |
| `title` | STRING | Title | Channel name as shown on YouTube. |
| `description` | STRING | Description | The channel's About text. |
| `custom_url` | STRING | Custom URL | The channel handle, e.g. @owox_bi. |
| `published_at` | TIMESTAMP | Published At | When the channel was created (UTC). |
| `country` | STRING | Country | Country set in the channel settings (ISO 3166-1 alpha-2), empty when not set. |
| `thumbnail_url` | STRING | Thumbnail URL | URL of the channel avatar, 800 px. |
| `view_count` | INTEGER | View Count | Lifetime views of all public videos, as of the run. A snapshot, not a daily number: daily views live in channel_daily. |
| `subscriber_count` | INTEGER | Subscriber Count | Subscribers as of the run, rounded by YouTube to three significant figures for channels above 1,000. |
| `hidden_subscriber_count` | BOOLEAN | Hidden Subscriber Count | True when the channel hides its subscriber count; subscriber_count is then 0. |
| `video_count` | INTEGER | Video Count | Public videos on the channel as of the run. |
| `uploads_playlist_id` | STRING | Uploads Playlist ID | The uploads playlist (UU…); copy it into the Uploads playlist ID parameter. |
| `keywords` | STRING | Keywords | Channel keywords from Studio → Customization, space-separated, quoted when multi-word. |
| `privacy_status` | STRING | Privacy Status | public, unlisted or private. |
| `made_for_kids` | BOOLEAN | Made For Kids | Channel-level made-for-kids setting. |

# Example Questions

- Which channel do these tables describe, when was it created, and what are its lifetime views, subscribers and video count as of today?
- What is the uploads playlist id the videos table is built from?
