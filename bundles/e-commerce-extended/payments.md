---
title: "Payments"
description: |
  One row per payment taken against an order — the card, wallet or bank transfer that paid for
  it, the amount captured, and the fee the payment provider kept. Payments are how Finance sees
  booked revenue turn into cash: an order can be paid in full, in part, or not at all, and the
  provider's fee comes off before the money reaches the store.
tags: ["owox"]
type: "OWOX Data Mart"
timestamp: 2026-11-06T12:00:00.000Z
---

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `payment_id` | STRING | Payment ID | PK. Unique identifier of the payment |
| `order_id` | STRING | Order ID | The order this payment pays for FK to [Orders](./orders.md) |
| `payment_date` | DATE | Payment Date | The day the provider captured the payment |
| `method` | STRING | Payment Method | How the shopper paid (e.g., Card, PayPal, Apple Pay, Bank Transfer) |
| `status` | STRING | Payment Status | Captured, Failed or Disputed |
| `amount` | FLOAT | Amount Paid | Amount captured, in US dollars; 0 when the payment failed |
| `processing_fee` | FLOAT | Processing Fee | Fee the payment provider kept on this payment, in US dollars |

## Calculated fields

### `net_cash` · FLOAT · Metric

- **Alias:** Net Cash Received

Cash that reached the store: captured amounts minus the provider's fees.

```sql
SUM(amount) - SUM(processing_fee)
```

### `fee_rate` · FLOAT · Metric

- **Alias:** Fee Rate

Processing fees as a share of captured amounts, as a fraction (0.029 means 2.9%).

```sql
SAFE_DIVIDE(SUM(processing_fee), SUM(amount))
```

### `failed_payment_rate` · FLOAT · Metric

- **Alias:** Failed Payment Rate

Share of payment attempts that failed.

```sql
SAFE_DIVIDE(COUNTIF(status = 'Failed'), COUNT(DISTINCT payment_id))
```

# Example Questions

- How much of last month's booked revenue arrived as cash, and how much did payment providers keep?
- Which payment methods fail most often, and in which `countries`?
- How many days pass between an `order` and the payment that settles it?

## Joins

- [Orders](./orders.md) — `order_id = order_id` [N:1] — The order this payment settles.
  - [Customers](./customers.md) — The customer who paid.
  - [Purchases](./purchases.md) — The lines the payment covers.
- [Calendar](./calendar.md) — `payment_date = date` [N:1] — The day the payment was captured.
