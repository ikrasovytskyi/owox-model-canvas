---
title: "Orders"
description: |
  One row per order: status, totals with and without tax, shipping, discounts, coupons, refunds, payment method, channel and currency. The daily window reads orders by date_modified, so a status change, a refund or an edit made weeks later reaches the table; id upserts the row. Revenue is read by date_created and filtered by status_id (see order_statuses): Incomplete (0), Cancelled (5) and Declined (6) are not sales.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management/orders/get-orders"
    title: "BigCommerce API reference — Orders"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `orders`
- **Default destination table:** `bigcommerce_orders`
- **Unique keys:** `id`
- **Loads:** by date window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. BigCommerce order id, unique in the store. Join key for order_products, order_coupons, order_transactions, refunds, order_shipping_addresses and order_shipments (order_id). |
| `customer_id` | INTEGER | Customer ID | FK to [Customers](./customers.md). Id of the customer who placed the order; 0 for a guest checkout. Join to customers (id). |
| `status_id` | INTEGER | Status ID | FK to [Order statuses](./order-statuses.md). Order status id, 0 to 14. Join to order_statuses (id). 0 Incomplete, 1 Pending, 2 Shipped, 3 Partially Shipped, 4 Refunded, 5 Cancelled, 6 Declined, 7 Awaiting Payment, 8 Awaiting Pickup, 9 Awaiting Shipment, 10 Completed, 11 Awaiting Fulfillment, 12 Manual Verification Required, 13 Disputed, 14 Partially Refunded. |
| `status` | STRING | Status | Status name as shown in the control panel (custom label if the store renamed it). |
| `date_created` | DATETIME | Date Created | When the order was placed (UTC); BigCommerce returns it in RFC 2822 (UTC). The date to count sales by. |
| `date_modified` | DATETIME | Date Modified | When the order last changed (UTC); BigCommerce returns it in RFC 2822 (UTC). The field the daily window filters on. |
| `date_shipped` | DATETIME | Date Shipped | When the order was fully shipped (UTC); empty until then. BigCommerce returns it in RFC 2822 (UTC). |
| `subtotal_ex_tax` | FLOAT | Subtotal Ex Tax | Sum of the line items before discounts, shipping and tax, in the order's transactional currency. Gross sales. |
| `subtotal_inc_tax` | FLOAT | Subtotal Inc Tax | Sum of the line items including tax, before discounts and shipping, in the order's transactional currency. |
| `subtotal_tax` | FLOAT | Subtotal Tax | Tax on the line items, in the order's transactional currency. |
| `total_ex_tax` | FLOAT | Total Ex Tax | Order total excluding tax, after discounts, with shipping, handling and wrapping, in the order's transactional currency. |
| `total_inc_tax` | FLOAT | Total Inc Tax | Order total the shopper paid, including tax, in the order's transactional currency. What BigCommerce shows as the order total. |
| `total_tax` | FLOAT | Total Tax | Total tax on the order, in the order's transactional currency. |
| `base_shipping_cost` | FLOAT | Base Shipping Cost | Shipping cost before tax and overrides, in the order's transactional currency. |
| `shipping_cost_ex_tax` | FLOAT | Shipping Cost Ex Tax | Shipping charged to the shopper excluding tax, in the order's transactional currency. |
| `shipping_cost_inc_tax` | FLOAT | Shipping Cost Inc Tax | Shipping charged to the shopper including tax, in the order's transactional currency. |
| `handling_cost_ex_tax` | FLOAT | Handling Cost Ex Tax | Handling charged excluding tax, in the order's transactional currency. |
| `wrapping_cost_ex_tax` | FLOAT | Wrapping Cost Ex Tax | Gift wrapping charged excluding tax, in the order's transactional currency. |
| `discount_amount` | FLOAT | Discount Amount | Manual and automatic discounts applied to the order (not coupons), in the order's transactional currency. |
| `coupon_discount` | FLOAT | Coupon Discount | Discount from coupon codes, in the order's transactional currency. Detail per code in order_coupons. |
| `store_credit_amount` | FLOAT | Store Credit Amount | Store credit the shopper redeemed on the order, in the order's transactional currency. |
| `gift_certificate_amount` | FLOAT | Gift Certificate Amount | Gift certificate value redeemed on the order, in the order's transactional currency. |
| `refunded_amount` | FLOAT | Refunded Amount | Amount refunded on the order so far, in the order's transactional currency. Grows after the order is placed; the refunds table holds each refund with its date. |
| `items_total` | INTEGER | Items Total | Number of items (units) in the order. |
| `items_shipped` | INTEGER | Items Shipped | Number of items shipped so far. |
| `payment_method` | STRING | Payment Method | Display name of the payment method, e.g. Credit Card, PayPal, Manual, Test Payment Gateway. |
| `payment_status` | STRING | Payment Status | Payment status of the order: authorized, captured, partially refunded, refunded, void, declined, pending and similar; empty for manual orders. |
| `currency_code` | STRING | Currency Code | Display currency the shopper saw on the storefront (ISO 4217). |
| `default_currency_code` | STRING | Default Currency Code | Transactional currency the shopper paid in (ISO 4217): the currency of every amount on this row. |
| `store_default_currency_code` | STRING | Store Default Currency Code | The store's default currency (ISO 4217). |
| `store_default_to_transactional_exchange_rate` | FLOAT | Store Default To Transactional Exchange Rate | Rate from the store's default currency to the transactional currency at order time; divide amounts by it to report in the store currency. |
| `channel_id` | INTEGER | Channel ID | FK to [Channels](./channels.md). Channel the order came from; 1 is the default storefront. Join to channels (id) for marketplaces, POS and other storefronts. |
| `order_source` | STRING | Order Source | Where the order was created: www (storefront), manual (control panel), external (Orders API), checkout_api, or a channel name. |
| `external_source` | STRING | External Source | System that created the order through the API (e.g. a marketplace connector or an ERP); empty for storefront orders. |
| `external_id` | STRING | External ID | The order's id in that external system, e.g. an Amazon order id. |
| `cart_id` | STRING | Cart ID | Id of the cart the order was placed from. |
| `geoip_country_iso2` | STRING | Geoip Country Iso2 | Country of the shopper's IP address, ISO 3166-1 alpha-2. |
| `billing_country_iso2` | STRING | Billing Country Iso2 | Billing address country, ISO 3166-1 alpha-2. |
| `billing_state` | STRING | Billing State | Billing address state or region. |
| `billing_city` | STRING | Billing City | Billing address city. |
| `billing_zip` | STRING | Billing Zip | Billing address postal code. |
| `billing_email` | STRING | Billing Email | E-mail on the billing address; the join to customers for guest orders and to other sources by e-mail. |
| `customer_locale` | STRING | Customer Locale | The shopper's locale, e.g. en or en-US. |
| `is_deleted` | BOOLEAN | Is Deleted | true when the order was archived (deleted) in the control panel. Exclude these from sales. |
| `is_tax_inclusive_pricing` | BOOLEAN | Is Tax Inclusive Pricing | true when the store's prices include tax. |
| `order_is_digital` | BOOLEAN | Order Is Digital | true when every item in the order is digital. |
| `is_email_opt_in` | BOOLEAN | Is Email Opt In | true when the shopper ticked the e-mail opt-in box at checkout. |

# Example Questions

- What were gross sales, discounts, refunds and net sales last month, counting only orders whose `order status` is a sale?
- How many orders and what average order value did each `channel` bring this month against last month?
- How many orders were placed by first-time `customers` versus returning ones?

## Joins

- [Customers](./customers.md) — `customer_id = id` [N:1] — 0 for a guest checkout.
- [Order statuses](./order-statuses.md) — `status_id = id` [N:1] — Which statuses count as a sale.
- [Channels](./channels.md) — `channel_id = id` [N:1] — Storefront, marketplace or POS the order came from.
- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
