---
title: "Playlist items"
description: |
  Which video sits in which playlist at which position: the bridge between playlists and videos. One row per placement, rewritten on every run.
tags: ["owox", "connector", "youtube"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.google.com/youtube/analytics/reference/reports/query"
    title: "OWOX Data Marts — YouTube custom connectors (YouTube on the Data API v3, 5 nodes; YouTube Analytics on the Analytics API v2, 7 nodes)"
  - id: "vendor-reference"
    resource: "https://developers.google.com/youtube/v3/docs/playlistItems/list"
    title: "YouTube API reference — Playlist items"
---

## Connector endpoint

- **Endpoint:** YouTube · `playlist_items`
- **Default destination table:** `youtube_playlist_items`
- **Unique keys:** `playlist_item_id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `playlist_item_id` | STRING | Playlist Item ID | PK. Placement id. Primary key. |
| `playlist_id` | STRING | Playlist ID | FK to [Playlists](./playlists.md). The playlist. Joins youtube_playlists. |
| `video_id` | STRING | Video ID | FK to [Videos](./videos.md). The video. Joins youtube_videos and video_daily. |
| `position` | INTEGER | Position | Zero-based position in the playlist. |
| `added_at` | TIMESTAMP | Added At | When the video was added to the playlist (UTC). |
| `video_published_at` | TIMESTAMP | Video Published At | When the video itself was published (UTC). |
| `title` | STRING | Title | Video title as of the run. |
| `video_owner_channel_id` | STRING | Video Owner Channel ID | Owner of the video; differs from channel_id when a playlist holds other channels' videos. |
| `privacy_status` | STRING | Privacy Status | Privacy of the placement. |

# Example Questions

- Which videos sit in the onboarding playlist, in what order, and which ones are in no playlist at all?

## Joins

- [Playlists](./playlists.md) — `playlist_id = playlist_id` [N:1] — The playlist the placement belongs to.
- [Videos](./videos.md) — `video_id = video_id` [N:1] — The video placed; a video can sit in several playlists.
