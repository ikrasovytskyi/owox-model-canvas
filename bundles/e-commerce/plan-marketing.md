---
title: "[Plan] Marketing"
description: |
  The paid-acquisition plan for every day of 2026 and 2027: the visits paid ads should bring,
  the settled revenue those visits should earn, the ad budget behind them and the return on ad
  spend expected on that day. Set it against Sessions, Purchases and Unified Ad Spend. Plan data
  is kept in the E-commerce Plan Data 2026–2027 spreadsheet, tab Marketing.
tags: ["owox"]
type: "OWOX Data Mart"
timestamp: 2026-10-26T16:15:05.000Z
---

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `date` | STRING | Date | PK. The day this plan row applies to, written as text in YYYY-MM-DD form (2026-01-01 to 2027-12-31). It is text, so range operators do not apply: filter a month with starts_with '2026-09', a year with starts_with '2026', and a quarter as its three months. |
| `paid_sessions` | INTEGER | Planned Paid Sessions | Storefront visits expected from paid ad clicks that day. |
| `paid_revenue` | FLOAT | Planned Paid Revenue | Settled revenue expected that day from orders placed in paid visits, in US dollars: planned ad spend times planned ROAS. |
| `roas` | FLOAT | Planned ROAS | Settled revenue expected for every $1 of ad spend on that day (4.0 means $4 back per $1). A rate fixed for that date, not a volume: never sum it. For a period, divide summed Planned Paid Revenue by summed Planned Ad Spend. |
| `ad_spend` | FLOAT | Planned Ad Spend | Ad budget planned for that day across all ad platforms, in US dollars; the same figure as in [Plan] Advertising, kept here so the return can be read from this mart alone. |
