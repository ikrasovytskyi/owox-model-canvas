---
title: "Refunds"
description: |
  One row per refund paid back to a shopper — which order it reverses, when, how much, and why.
  A returned order already carries the status Returned in Orders; Refunds says how much money
  actually went back, which is what Finance subtracts to reach the revenue the store keeps.
tags: ["owox"]
type: "OWOX Data Mart"
timestamp: 2026-11-06T12:00:00.000Z
---

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `refund_id` | STRING | Refund ID | PK. Unique identifier of the refund |
| `order_id` | STRING | Order ID | The order this refund reverses, in full or in part FK to [Orders](./orders.md) |
| `refund_date` | DATE | Refund Date | The day the money was sent back |
| `reason` | STRING | Refund Reason | Why the shopper was refunded (e.g., Damaged, Wrong Item, Changed Mind, Late Delivery) |
| `amount` | FLOAT | Refund Amount | Amount paid back, in US dollars |

## Calculated fields

### `refunded_amount` · FLOAT · Metric

- **Alias:** Refunded Amount

Money paid back to shoppers in the selected rows.

```sql
SUM(amount)
```

### `refunded_orders` · INTEGER · Metric

- **Alias:** Refunded Orders

Distinct orders with at least one refund.

```sql
COUNT(DISTINCT order_id)
```

# Example Questions

- What share of revenue goes back as refunds, and which `reasons` drive it?
- Which `products` and `categories` are refunded most often?
- Does the refund rate differ by acquisition channel or `country`?

## Joins

- [Orders](./orders.md) — `order_id = order_id` [N:1] — The order this refund reverses.
  - [Customers](./customers.md) — The customer who was refunded.
  - [Purchases](./purchases.md) — The lines of the refunded order.
    - [Refunded Product](./products.md) — The products on those lines.
- [Calendar](./calendar.md) — `refund_date = date` [N:1] — The day the refund was sent.
