---
title: "[Plan] Sales"
description: |
  The revenue plan for every day of 2026 and 2027: the orders the store expects, the settled
  value of an average order and the settled revenue that follows. Set it against Purchases by
  Orders · Order Date. Plan data is kept in the E-commerce Plan Data 2026–2027 spreadsheet, tab
  Sales.
tags: ["owox"]
type: "OWOX Data Mart"
timestamp: 2026-10-26T16:14:57.000Z
---

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `date` | STRING | Date | PK. The day this plan row applies to, written as text in YYYY-MM-DD form (2026-01-01 to 2027-12-31). It is text, so range operators do not apply: filter a month with starts_with '2026-09', a year with starts_with '2026', and a quarter as its three months. |
| `orders` | INTEGER | Planned Orders | Orders expected that day; the same figure as Planned Orders in [Plan] Product. |
| `average_order_value` | FLOAT | Planned Average Order Value | Settled revenue expected per order, in US dollars; set per month, so every day of a month carries the same value. For a period, divide summed Planned Settled Revenue by summed Planned Orders. |
| `settled_revenue` | FLOAT | Planned Settled Revenue | Revenue the store expects to keep from that day's orders after cancellations and returns, in US dollars: planned orders times planned average order value. |
