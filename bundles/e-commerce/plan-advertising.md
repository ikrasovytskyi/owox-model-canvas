---
title: "[Plan] Advertising"
description: |
  The ad-buying plan for every day of 2026 and 2027: the budget to spend across all ad
  platforms, the paid clicks it should buy and the cost per click the team expects on that day.
  Set it against Unified Ad Spend, which records what was actually spent and clicked. Plan data
  is kept in the E-commerce Plan Data 2026–2027 spreadsheet, tab Advertising.
tags: ["owox"]
type: "OWOX Data Mart"
timestamp: 2026-10-26T16:15:11.000Z
---

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `date` | STRING | Date | PK. The day this plan row applies to, written as text in YYYY-MM-DD form (2026-01-01 to 2027-12-31). It is text, so range operators do not apply: filter a month with starts_with '2026-09', a year with starts_with '2026', and a quarter as its three months. |
| `ad_spend` | FLOAT | Planned Ad Spend | Ad budget planned for that day across all ad platforms, in US dollars. Sum the days for a period's plan. |
| `clicks` | INTEGER | Planned Clicks | Paid clicks the day's budget should buy: planned ad spend divided by planned cost per click. |
| `cost_per_click` | FLOAT | Planned Cost per Click | Cost per click expected on that day, in US dollars. A rate fixed for that date, not a volume: never sum it. For a period, divide summed Planned Ad Spend by summed Planned Clicks. |
