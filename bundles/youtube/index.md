---
title: "YouTube"
description: |
  What the OWOX Data Marts YouTube connectors load from one channel, and how the tables fit together.
  Every table carries `channel_id`, the channel as YouTube names it (UC…), so several channels share one dataset, and joins to
  [Channel](./channel.md) on it.
  
  Two kinds of table. The catalogue from the Data API, snapshots rewritten on every run: [Channel](./channel.md) with its lifetime
  counters, [Videos](./videos.md) with titles, tags, durations and lifetime views, [Playlists](./playlists.md) and
  [Playlist items](./playlist-items.md) as the topic dimension, and [Comment threads](./comment-threads.md) for what viewers ask.
  The series from the Analytics API, one row per day, loaded by date window with a 3-day lag: [Channel daily](./channel-daily.md)
  is the reference every breakdown is checked against; [Video daily](./video-daily.md), [Traffic sources daily](./traffic-sources-daily.md),
  [Geography daily](./geography-daily.md), [Devices daily](./devices-daily.md) and [Demographics daily](./demographics-daily.md) cut the
  same days by video, source, country, device and audience; [Revenue daily](./revenue-daily.md) exists for Partner Program channels.
  A lifetime `view_count` on a video and the sum of its `video_daily.views` differ on purpose: the first is YouTube's public counter
  as of the run, the second the settled daily series.
  
  In the business model the channel is a traffic source like paid media, only owned. `video_daily` joins
  [Sessions](../e-commerce/sessions.md) on the day and on the video id you carry in `utm_content` or the landing page, so a video's
  views stand next to the sessions and orders it produced; `channel_daily.subscribers_gained` sits beside
  [Visitors](../e-commerce/visitors.md) and leads by day as the audience the channel builds; and `channel_daily.estimated_minutes_watched`
  is the organic top-of-funnel input to put beside [Unified Ad Spend](../e-commerce/unified-ad-spend.md) by day. The search-side sibling
  of this bundle is [Google Search Console](../google-search-console/index.md).
  
  Definitions follow connector version 1 (2026-11-07) on the YouTube Data API v3 and the YouTube Analytics API v2.
tags: ["owox", "index", "connector", "youtube"]
type: "index"
sources:
  - id: "owox-connector"
    resource: "https://developers.google.com/youtube/analytics/reference/reports/query"
    title: "OWOX Data Marts — YouTube custom connectors (YouTube on the Data API v3, 5 nodes; YouTube Analytics on the Analytics API v2, 7 nodes)"
    last_modified: 2026-11-07
---

<!-- OWOX:GENERATED:START — regenerated on export, do not edit inside this block -->

| Data Mart | Fields | Description |
|-----------|--------|-------------|
| [Channel](./channel.md) | 15 | The channel itself, one row rewritten on every run: title, handle, country, creation date and the lifetime view, subscriber and video counts the channel page shows. |
| [Videos](./videos.md) | 25 | Every public upload with its metadata and lifetime counters as of the run: title, description, tags, category, duration, publish time, privacy, views, likes and comments. |
| [Playlists](./playlists.md) | 8 | The channel's playlists (series, courses, product lines) with item counts and privacy. |
| [Playlist items](./playlist-items.md) | 9 | Which video sits in which playlist at which position: the bridge between playlists and videos. |
| [Comment threads](./comment-threads.md) | 12 | Every top-level comment on the channel's videos with author, text, likes and reply count. |
| [Channel daily](./channel-daily.md) | 14 | One row per day for the whole channel: views, watch time, average view duration and percentage, likes, dislikes, comments, shares, subscribers gained and lost, playlist adds. |
| [Video daily](./video-daily.md) | 13 | Per video and day: views, watch time, average view duration and percentage, likes, dislikes, comments, shares, subscribers gained and lost. |
| [Traffic sources daily](./traffic-sources-daily.md) | 5 | Views and watch time per day and traffic source type: YouTube search, suggested videos, browse (home and subscriptions), external links, playlists, channel pages, notifications, Shorts feed, advertising. |
| [Geography daily](./geography-daily.md) | 13 | Views, watch time, retention and engagement per country and day. |
| [Demographics daily](./demographics-daily.md) | 5 | Share of the day's watch time per age group and gender, in percent. |
| [Devices daily](./devices-daily.md) | 6 | Views and watch time per day, device type and operating system. |
| [Revenue daily](./revenue-daily.md) | 10 | Estimated revenue, ad revenue, YouTube Premium revenue, gross revenue, CPM, playback-based CPM, ad impressions and monetized playbacks per day. |

# Example Questions

- Are views, watch time and subscribers up this month against last in `channel_daily`, and is the growth from new videos or the back catalogue (`video_daily` × `videos.published_at`)?
- Do people find the channel through YouTube search and suggestions or through our own links (`traffic_sources_daily`), and did the video we published turn into `Sessions` and leads on the site?
- Which videos hold attention (`video_daily.average_view_percentage`) rather than just getting clicks, and what do viewers ask under them in `comment_threads`?

# Explore this model

**[▶ Explore on canvas](https://model.owox.com/?okf=https://raw.githubusercontent.com/ikrasovytskyi/owox-model-canvas/main/bundles/youtube/index.md)**

One click opens this model in a free OWOX canvas you can poke around in — no account needed.

<!-- OWOX:GENERATED:END -->
