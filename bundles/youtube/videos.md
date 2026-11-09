---
title: "Videos"
description: |
  Every public upload with its metadata and lifetime counters as of the run: title, description, tags, category, duration, publish time, privacy, views, likes and comments. Built from the uploads playlist, one row per video, rewritten daily; the dimension table for Video daily.
tags: ["owox", "connector", "youtube"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.google.com/youtube/analytics/reference/reports/query"
    title: "OWOX Data Marts — YouTube custom connectors (YouTube on the Data API v3, 5 nodes; YouTube Analytics on the Analytics API v2, 7 nodes)"
  - id: "vendor-reference"
    resource: "https://developers.google.com/youtube/v3/docs/videos/list"
    title: "YouTube API reference — Videos"
---

## Connector endpoint

- **Endpoint:** YouTube · `videos`
- **Default destination table:** `youtube_videos`
- **Unique keys:** `video_id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `video_id` | STRING | Video ID | PK. The 11-character video ID (youtube.com/watch?v=…). Primary key; the join key to video_daily and to the video id you put in UTM content or landing pages. |
| `channel_id` | STRING | Channel ID | FK to [Channel](./channel.md). The channel that owns the video. Joins youtube_channel. |
| `title` | STRING | Title | Video title as of the run. |
| `description` | STRING | Description | Full video description, including the links in it. |
| `published_at` | TIMESTAMP | Published At | When the video was published (UTC). Age of a video = run date minus this. |
| `tags` | JSON | Tags | Tags set in Studio, as a JSON array string. |
| `category_id` | STRING | Category ID | YouTube category id (28 = Science & Technology, 27 = Education, 22 = People & Blogs). |
| `default_language` | STRING | Default Language | Language of the title and description (BCP-47), when set. |
| `default_audio_language` | STRING | Default Audio Language | Language of the audio track, when set. |
| `live_broadcast_content` | STRING | Live Broadcast Content | none for uploads, live or upcoming for streams. |
| `thumbnail_url` | STRING | Thumbnail URL | URL of the 480×360 thumbnail. |
| `duration` | STRING | Duration | Length as an ISO 8601 duration (PT12M34S). Under PT1M is usually a Short; parse in SQL with REGEXP_EXTRACT. |
| `definition` | STRING | Definition | hd or sd. |
| `caption` | BOOLEAN | Caption | True when the video has captions. |
| `licensed_content` | BOOLEAN | Licensed Content | True when the video is claimed by a content partner. |
| `privacy_status` | STRING | Privacy Status | public, unlisted or private. |
| `upload_status` | STRING | Upload Status | processed, uploaded, failed, rejected or deleted. |
| `license` | STRING | License | youtube (standard) or creativeCommon. |
| `embeddable` | BOOLEAN | Embeddable | Whether the video can be embedded on other sites. |
| `made_for_kids` | BOOLEAN | Made For Kids | Made-for-kids flag; such videos get no personalised ads and no comments. |
| `view_count` | INTEGER | View Count | Lifetime views as of the run. |
| `like_count` | INTEGER | Like Count | Lifetime likes as of the run (empty when the creator hides it). |
| `comment_count` | INTEGER | Comment Count | Lifetime comments as of the run (empty when comments are off). |
| `favorite_count` | INTEGER | Favorite Count | Always 0 since YouTube retired favourites; kept for schema stability. |
| `topic_categories` | JSON | Topic Categories | Wikipedia topic URLs YouTube assigned to the video, as a JSON array string. |

# Example Questions

- Which videos were published this quarter, how long are they, and which are Shorts (under a minute)?
- Which titles and tags do our best lifetime performers share?

## Joins

- [Channel](./channel.md) — `channel_id = channel_id` [N:1] — Every video belongs to the channel.
