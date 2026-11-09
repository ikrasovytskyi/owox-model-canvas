---
title: "Comment threads"
description: |
  Every top-level comment on the channel's videos with author, text, likes and reply count. One row per thread, rewritten on every run because comments are edited and deleted; the table for what viewers ask and object to.
tags: ["owox", "connector", "youtube"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.google.com/youtube/analytics/reference/reports/query"
    title: "OWOX Data Marts — YouTube custom connectors (YouTube on the Data API v3, 5 nodes; YouTube Analytics on the Analytics API v2, 7 nodes)"
  - id: "vendor-reference"
    resource: "https://developers.google.com/youtube/v3/docs/commentThreads/list"
    title: "YouTube API reference — Comment threads"
---

## Connector endpoint

- **Endpoint:** YouTube · `comment_threads`
- **Default destination table:** `youtube_comment_threads`
- **Unique keys:** `comment_id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `comment_id` | STRING | Comment ID | PK. Top-level comment id. Primary key. |
| `video_id` | STRING | Video ID | FK to [Videos](./videos.md). The video commented on. Joins youtube_videos. |
| `channel_id` | STRING | Channel ID | FK to [Channel](./channel.md). The channel the video belongs to. Joins youtube_channel. |
| `text` | STRING | Text | The comment as written, plain text. |
| `author_display_name` | STRING | Author Display Name | Commenter's display name. |
| `author_channel_id` | STRING | Author Channel ID | Commenter's channel id; equals channel_id for the creator's own replies. |
| `like_count` | INTEGER | Like Count | Likes on the comment as of the run. |
| `published_at` | TIMESTAMP | Published At | When the comment was posted (UTC). |
| `updated_at` | TIMESTAMP | Updated At | Last edit time (UTC). |
| `total_reply_count` | INTEGER | Total Reply Count | Replies under the comment as of the run. |
| `can_reply` | BOOLEAN | Can Reply | Whether the viewer can reply. |
| `is_public` | BOOLEAN | Is Public | Whether the thread is visible to everyone. |

# Example Questions

- What do viewers ask in the comments this month, and which videos collect the questions?
- How fast does the channel reply (creator comments by day)?

## Joins

- [Videos](./videos.md) — `video_id = video_id` [N:1] — The video commented on.
- [Channel](./channel.md) — `channel_id = channel_id` [N:1]
