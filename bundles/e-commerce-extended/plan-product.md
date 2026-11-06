---
title: "[Plan] Product"
description: |
  The storefront conversion plan for every day of 2026 and 2027: the visits the store expects,
  the orders they should produce and the share of visits expected to end in an order on that
  day. Set it against Sessions. Plan data is kept in the E-commerce Plan Data 2026–2027
  spreadsheet, tab Product.
tags: ["owox"]
type: "OWOX Data Mart"
timestamp: 2026-10-24T09:54:57.000Z
---

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `date` | STRING | Date | PK. The day this plan row applies to, written as text in YYYY-MM-DD form (2026-01-01 to 2027-12-31). It is text, so range operators do not apply: filter a month with starts_with '2026-09', a year with starts_with '2026', and a quarter as its three months. |
| `sessions` | INTEGER | Planned Sessions | Storefront visits expected that day, from every channel. |
| `orders` | INTEGER | Planned Orders | Orders the day's visits should produce: planned sessions times planned conversion rate. |
| `conversion_rate` | FLOAT | Planned Conversion Rate | Share of visits expected to end in an order on that day, on a 0–1 scale (0.028 means 2.8%). A rate fixed for that date, not a volume: never sum it. For a period, divide summed Planned Orders by summed Planned Sessions. |

## Joins

- [Calendar](./calendar.md) — `date = date_text` [1:1] — The day this plan row is for; set it against the actuals that land on the same day.
