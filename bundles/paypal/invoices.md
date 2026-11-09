---
title: "Invoices"
description: |
  Every PayPal invoice in any status with its amounts, due date, recipient and what has been paid or refunded against it. The receivables side of the account.
tags: ["owox", "connector", "paypal"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.paypal.com/docs/api/transaction-search/v1/"
    title: "OWOX Data Marts — PayPal custom connector (PayPal, 7 nodes) on the PayPal REST APIs: Transaction Search v1, Disputes v1, Invoicing v2, Subscriptions v1, Catalog Products v1"
  - id: "vendor-reference"
    resource: "https://developer.paypal.com/docs/api/invoicing/v2/#invoices_list"
    title: "PayPal API reference — Invoices"
---

## Connector endpoint

- **Endpoint:** PayPal · `invoices`
- **Default destination table:** `paypal_invoices`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. FK to [Transactions](./transactions.md). PayPal invoice id (INV2-…). |
| `account_id` | STRING | Account ID | Your PayPal merchant account id as entered in the connector (Account settings → Business information); lets several PayPal accounts share one table. Empty when not set. |
| `status` | STRING | Status | DRAFT, SENT, SCHEDULED, PAID, MARKED_AS_PAID, CANCELLED, REFUNDED, PARTIALLY_PAID, PARTIALLY_REFUNDED, MARKED_AS_REFUNDED, UNPAID, PAYMENT_PENDING, AUTO_CANCELLED, PAID_EXTERNAL, REFUNDED_EXTERNAL or SHARED. |
| `invoice_number` | STRING | Invoice Number | The invoice number shown to the recipient. |
| `reference` | STRING | Reference | The recipient's reference, such as a purchase-order number. |
| `invoice_date` | DATE | Invoice Date | The invoice date. |
| `currency_code` | STRING | Currency Code | Three-letter ISO currency of every amount on the invoice. |
| `term_type` | STRING | Term Type | Payment term: DUE_ON_RECEIPT, DUE_ON_DATE_SPECIFIED, NET_10, NET_15, NET_30, NET_45, NET_60, NET_90 or NO_DUE_DATE. |
| `due_date` | DATE | Due Date | When payment is due. |
| `memo` | STRING | Memo | The merchant's private bookkeeping memo. |
| `note` | STRING | Note | The note to the recipient. |
| `create_time` | TIMESTAMP | Create Time | When the invoice was created (UTC). |
| `last_update_time` | TIMESTAMP | Last Update Time | When the invoice last changed (UTC). |
| `first_sent_time` | TIMESTAMP | First Sent Time | When the invoice was first sent to the recipient (UTC). |
| `last_sent_time` | TIMESTAMP | Last Sent Time | When the invoice was last sent or re-sent (UTC). |
| `cancel_time` | TIMESTAMP | Cancel Time | When the invoice was cancelled (UTC), if it was. |
| `recipient_email` | STRING | Recipient Email | The primary recipient's e-mail. The join to customers. |
| `recipient_business_name` | STRING | Recipient Business Name | The primary recipient's business name. |
| `recipient_name` | STRING | Recipient Name | The primary recipient's full name. |
| `recipient_country` | STRING | Recipient Country | The primary recipient's billing country code. |
| `total` | FLOAT | Total | Invoice total as a decimal: items minus discount plus tax, shipping and custom amounts. |
| `item_total` | FLOAT | Item Total | Sum of the line items before discount and tax. |
| `tax_total` | FLOAT | Tax Total | Total tax on the invoice. |
| `invoice_discount` | FLOAT | Invoice Discount | Invoice-level discount amount. |
| `item_discount` | FLOAT | Item Discount | Sum of the item-level discounts. |
| `shipping` | FLOAT | Shipping | Shipping charged on the invoice. |
| `custom_amount` | FLOAT | Custom Amount | Any custom amount added to the invoice. |
| `due_amount` | FLOAT | Due Amount | What is still owed: total minus payments. Zero on a paid invoice. |
| `gratuity` | FLOAT | Gratuity | Tip the payer added. |
| `paid_amount` | FLOAT | Paid Amount | Total paid against the invoice so far. |
| `refund_amount` | FLOAT | Refund Amount | Total refunded against the invoice. |
| `payment_transactions` | JSON | Payment Transactions | JSON list of the payments recorded on the invoice: type (PAYPAL or EXTERNAL), payment_id (the PayPal transaction id, join to transactions), payment_date, method, amount. |
| `refund_transactions` | JSON | Refund Transactions | JSON list of the refunds recorded on the invoice: type, refund_id, refund_date, method, amount. |
| `items` | JSON | Items | JSON list of the invoice lines: name, description, quantity, unit_amount, tax, discount, unit_of_measure. Unnest for invoiced revenue by item. |
| `invoicer_view_url` | STRING | Invoicer View URL | Merchant-side URL of the invoice on paypal.com. |

# Example Questions

- How much is invoiced, paid and still due this month?
- Which invoices are past their due date, to whom, and for how much?

## Joins

- [Transactions](./transactions.md) — `id = paypal_invoice_id` [1:N] — Payments made through PayPal against this invoice; payment_transactions also lists their payment_id.
