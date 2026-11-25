---
title: "BigCommerce"
description: |
  What the OWOX Data Marts BigCommerce connector loads from one or more BigCommerce stores, and how the tables fit together.
  Every table carries `store_hash`, the store's id in the API, so several stores share one table, and [Store](./store.md) holds each store's currency and time zone.
  
  [Orders](./orders.md) is the spine: one row per order with its status, totals, discounts, coupons, shipping, refunds and channel,
  loaded by `date_modified` so later status changes and refunds arrive. [Order line items](./order-line-items.md) are the order lines
  (product, variant, quantity, price, discounts, cost, units refunded); [Refunds](./refunds.md) hold each refund with its date and items;
  [Order statuses](./order-statuses.md) say which orders count as sales. Gross sales minus discounts minus refunds is net sales.
  
  [Customers](./customers.md) with [Customer groups](./customer-groups.md) separate first-time from returning buyers and retail from wholesale.
  [Products](./products.md), [Variants](./variants.md), [Categories](./categories.md) and [Brands](./brands.md) are the catalog: margin is order lines × variant cost price.
  [Order coupons](./order-coupons.md) and [Coupons](./coupons.md) give the discount by code; [Order transactions](./order-transactions.md) the payments per gateway;
  [Order shipping addresses](./order-shipping-addresses.md) and [Order shipments](./order-shipments.md) where orders went and how fast; [Channels](./channels.md) the storefront, marketplaces and POS.
  
  In the E-Commerce model, Orders are the Orders, Order line items the Purchases, Customers the Customers, Products with Variants the Products and Categories the Product Category;
  channels and dates join them to Sessions, Traffic Sources and Unified Ad Spend.
tags: ["owox", "index", "connector", "bigcommerce"]
type: "index"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
    last_modified: 2026-11-26
---

<!-- OWOX:GENERATED:START — regenerated on export, do not edit inside this block -->

| Data Mart | Fields | Description |
|-----------|--------|-------------|
| [Orders](./orders.md) | 48 | One row per order: status, totals with and without tax, shipping, discounts, coupons, refunds, payment method, channel and currency. |
| [Order line items](./order-line-items.md) | 28 | One row per product line in an order: product and variant, quantity, unit price, line totals before and after discounts, cost price, and how many units were refunded or shipped. |
| [Refunds](./refunds.md) | 11 | One row per refund: when it was made, the total and tax refunded, the reason, which items it covered (products, shipping, handling, the whole order) and how the money went back. |
| [Order statuses](./order-statuses.md) | 6 | The 15 order statuses with their names and the store's own labels. |
| [Customers](./customers.md) | 13 | One row per registered customer: e-mail, group, the channel they signed up on, and when the account was created and last changed. |
| [Customer groups](./customer-groups.md) | 6 | Customer groups such as retail and wholesale, with their discount rules: the field that separates B2B from B2C orders. |
| [Products](./products.md) | 24 | One row per catalog product: name, SKU, brand, categories, prices, cost, stock and units sold. |
| [Variants](./variants.md) | 13 | One row per SKU sold: the product it belongs to, its price, sale price, cost and stock. |
| [Categories](./categories.md) | 8 | Every category in every category tree: name, parent and visibility, for grouping products and revenue. |
| [Brands](./brands.md) | 5 | Brand names for grouping products and revenue. |
| [Order coupons](./order-coupons.md) | 8 | One row per coupon code used on an order, with the discount it gave. |
| [Coupons](./coupons.md) | 13 | Every coupon set up in the store: code, type, value, minimum purchase, whether it is on, and how often it was used. |
| [Channels](./channels.md) | 10 | The store's sales channels: storefronts, marketplaces (Amazon, eBay, Walmart), point of sale and social shops. |
| [Order transactions](./order-transactions.md) | 14 | One row per payment event on an order: authorization, capture, purchase, refund, void, with the gateway, method, amount and outcome. |
| [Order shipping addresses](./order-shipping-addresses.md) | 15 | One row per shipping address on an order (an order can ship to several): where it went, the shipping method and zone, and what shipping cost. |
| [Order shipments](./order-shipments.md) | 11 | One row per shipment: when it was created, the carrier and tracking, and which order lines it carried. |
| [Store](./store.md) | 17 | One row with the store's settings: name, domain, default currency, time zone, plan and default channel. |

# Example Questions


# Explore this model

**[▶ Explore on canvas](https://model.owox.com/?okf=https://raw.githubusercontent.com/ikrasovytskyi/owox-model-canvas/main/bundles/bigcommerce/index.md)**

One click opens this model in a free OWOX canvas you can poke around in — no account needed.

<!-- OWOX:GENERATED:END -->
