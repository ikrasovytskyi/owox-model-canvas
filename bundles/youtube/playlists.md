---
title: "Playlists"
description: |
  The channel's playlists (series, courses, product lines) with item counts and privacy. One row per playlist, rewritten on every run; the topic dimension for video performance.
tags: ["owox", "connector", "youtube"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.google.com/youtube/analytics/reference/reports/query"
    title: "OWOX Data Marts — YouTube custom connectors (YouTube on the Data API v3, 5 nodes; YouTube Analytics on the Analytics API v2, 7 nodes)"
  - id: "vendor-reference"
    resource: "https://developers.google.com/youtube/v3/docs/playlists/list"
    title: "YouTube API reference — Playlists"
---

## Connector endpoint

- **Endpoint:** YouTube · `playlists`
- **Default destination table:** `youtube_playlists`
- **Unique keys:** `playlist_id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `playlist_id` | STRING | Playlist ID | PK. Playlist ID (PL…). Primary key. |
| `channel_id` | STRING | Channel ID | FK to [Channel](./channel.md). Owning channel. Joins youtube_channel. |
| `title` | STRING | Title | Playlist title. |
| `description` | STRING | Description | Playlist description. |
| `published_at` | TIMESTAMP | Published At | When the playlist was created (UTC). |
| `thumbnail_url` | STRING | Thumbnail URL | Thumbnail of the playlist's cover video. |
| `item_count` | INTEGER | Item Count | Number of videos in the playlist as of the run. |
| `privacy_status` | STRING | Privacy Status | public, unlisted or private. |

# Example Questions

- Which playlists (series, courses) does the channel run and how many videos does each hold?

## Joins

- [Channel](./channel.md) — `channel_id = channel_id` [N:1]
