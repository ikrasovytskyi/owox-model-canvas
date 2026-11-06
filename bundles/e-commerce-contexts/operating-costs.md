---
title: "Operating Costs"
description: |
  The costs of running the store that no single order carries — shipping and fulfilment,
  platform and app fees, payroll, rent — one row per day and cost category. Together with
  cost of goods in Purchases, ad spend in Unified Ad Spend and processing fees in Payments,
  it lets Finance read profit after every cost, not just gross margin.
tags: ["owox"]
type: "OWOX Data Mart"
timestamp: 2026-11-06T12:00:00.000Z
---

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `date` | DATE | Cost Date | PK. The day the cost was booked |
| `cost_category` | STRING | Cost Category | PK. Shipping, Fulfilment, Platform Fees, Payroll, Rent or Other |
| `amount` | FLOAT | Cost Amount | Cost booked that day in that category, in US dollars |

## Calculated fields

### `total_operating_cost` · FLOAT · Metric

- **Alias:** Total Operating Cost

Operating costs in the selected rows.

```sql
SUM(amount)
```

# Example Questions

- What does it cost to run the store each month, before cost of goods and ad spend?
- Which cost category is growing faster than revenue?
- What is profit after cost of goods, ad spend, processing fees and operating costs?
