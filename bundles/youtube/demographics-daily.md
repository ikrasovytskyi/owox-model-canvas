---
title: "Demographics daily"
description: |
  Share of the day's watch time per age group and gender, in percent. YouTube reports percentages, not counts; weight them by the day's views from Channel daily to aggregate over weeks or months.
tags: ["owox", "connector", "youtube"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.google.com/youtube/analytics/reference/reports/query"
    title: "OWOX Data Marts — YouTube custom connectors (YouTube on the Data API v3, 5 nodes; YouTube Analytics on the Analytics API v2, 7 nodes)"
  - id: "vendor-reference"
    resource: "https://developers.google.com/youtube/analytics/channel_reports#viewer-demographics"
    title: "YouTube API reference — Demographics daily"
---

## Connector endpoint

- **Endpoint:** YouTube · `demographics_daily`
- **Default destination table:** `youtube_demographics_daily`
- **Unique keys:** `channel_id`, `day`, `age_group`, `gender`
- **Loads:** by date window (`incremental: day-by-day`, 3-day lag)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `channel_id` | STRING | Channel ID | PK. FK to [Channel](./channel.md). The channel, stamped from the Channel ID parameter. Part of the key. |
| `day` | DATE | Day | PK. The day, stamped from the request window. Part of the key. |
| `age_group` | STRING | Age Group | PK. age13-17, age18-24, age25-34, age35-44, age45-54, age55-64, age65-. Part of the key. |
| `gender` | STRING | Gender | PK. female, male or user_specified. Part of the key. |
| `viewer_percentage` | FLOAT | Viewer Percentage | Percent of the day's watch time from that age group and gender; the day's rows sum to 100. |

# Example Questions

- What is the audience's age and gender mix, weighted by views, this quarter against last?

## Joins

- [Channel daily](./channel-daily.md) — `channel_id = channel_id, day = day` [N:1] — Percentages of the day's watch time; weight by the day's views to aggregate.
- [Channel](./channel.md) — `channel_id = channel_id` [N:1] — The channel the day belongs to.
