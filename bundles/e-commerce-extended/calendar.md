---
title: "Calendar"
description: |
  One row per day, 2024 to 2027 — the date every other mart is read by. Orders, sessions, ad
  spend, payments, refunds, operating costs and every plan land on the same day here, so actuals
  and plans can be set side by side by day, week, month, quarter or year without each mart
  rolling up its own dates.
tags: ["owox"]
type: "OWOX Data Mart"
timestamp: 2026-11-06T12:00:00.000Z
---

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `date` | DATE | Date | PK. The calendar day |
| `date_text` | STRING | Date (Text) | The same day as text in YYYY-MM-DD form; the key the plan marts join on, because their dates are text |
| `week_start` | DATE | Week Start | Monday of the ISO week the day falls in |
| `month` | STRING | Month | The month as YYYY-MM |
| `quarter` | STRING | Quarter | The quarter as YYYY-Qn |
| `year` | INTEGER | Year | The calendar year |

# Example Questions

- How do actual orders, sessions and ad spend compare with the plan, month by month?
- Which weeks fell furthest behind plan, and in which context — Marketing, Sales, Product or Finance?
- What was profit after every cost in each quarter?

## Joins

- [Orders](./orders.md) — `date = order_date` [1:N] — Orders placed that day.
- [Sessions](./sessions.md) — `date = date` [1:N] — Visits that day.
- [Unified Ad Spend](./unified-ad-spend.md) — `date = date` [1:N] — Ad spend that day.
- [Payments](./payments.md) — `date = payment_date` [1:N] — Payments captured that day.
- [Refunds](./refunds.md) — `date = refund_date` [1:N] — Refunds sent that day.
- [Operating Costs](./operating-costs.md) — `date = date` [1:N] — Operating costs booked that day.
- [[Plan] Marketing](./plan-marketing.md) — `date_text = date` [1:1] — The marketing plan for that day.
- [[Plan] Advertising](./plan-advertising.md) — `date_text = date` [1:1] — The advertising plan for that day.
- [[Plan] Product](./plan-product.md) — `date_text = date` [1:1] — The product plan for that day.
- [[Plan] Sales](./plan-sales.md) — `date_text = date` [1:1] — The sales plan for that day.
