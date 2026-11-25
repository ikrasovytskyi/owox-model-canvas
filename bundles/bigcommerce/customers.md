---
title: "Customers"
description: |
  One row per registered customer: e-mail, group, the channel they signed up on, and when the account was created and last changed. Guests are not customers; their orders have customer_id 0.
tags: ["owox", "connector", "bigcommerce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management"
    title: "OWOX Data Marts — BigCommerce custom connector (BigCommerce, 17 nodes) on the BigCommerce REST Management API v2 and v3"
  - id: "vendor-reference"
    resource: "https://docs.bigcommerce.com/developer/api-reference/rest/admin/management/customers/v3/get-customers"
    title: "BigCommerce API reference — Customers"
---

## Connector endpoint

- **Endpoint:** BigCommerce · `customers`
- **Default destination table:** `bigcommerce_customers`
- **Unique keys:** `id`
- **Loads:** by date window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `store_hash` | STRING | Store Hash | FK to [Store](./store.md). Your BigCommerce store hash (the id in the API path, e.g. abc123de), as entered in the connector; lets several stores share one table. |
| `id` | INTEGER | ID | PK. Customer id, unique in the store. Join to orders (customer_id). |
| `email` | STRING | Email | Customer e-mail; the join to other sources (CRM, e-mail marketing, analytics) by e-mail. |
| `company` | STRING | Company | Company name on the account. |
| `customer_group_id` | INTEGER | Customer Group ID | FK to [Customer groups](./customer-groups.md). Customer group (e.g. retail, wholesale); 0 for none. Join to customer_groups (id). |
| `date_created` | DATETIME | Date Created | When the account was created (UTC, ISO 8601). |
| `date_modified` | DATETIME | Date Modified | When the account last changed (UTC, ISO 8601). The field the daily window filters on. |
| `origin_channel_id` | INTEGER | Origin Channel ID | FK to [Channels](./channels.md). Channel the customer signed up on. Join to channels (id). |
| `channel_ids` | STRING | Channel Ids | JSON list of the channels the account can sign in to. |
| `accepts_product_review_abandoned_cart_emails` | BOOLEAN | Accepts Product Review Abandoned Cart Emails | true when the customer accepts review and abandoned-cart e-mails. |
| `tax_exempt_category` | STRING | Tax Exempt Category | Tax exemption code, empty for a taxable customer. |
| `address_count` | INTEGER | Address Count | Number of saved addresses. |
| `registration_ip_address` | STRING | Registration Ip Address | IP address the account was registered from. |

# Example Questions

- How many new customers registered each month, and how many placed a second order within 90 days?
- Which `customer groups` (retail, wholesale) bring the revenue?

## Joins

- [Customer groups](./customer-groups.md) — `customer_group_id = id` [N:1] — Retail, wholesale and other groups.
- [Channels](./channels.md) — `origin_channel_id = id` [N:1] — Where the customer signed up.
- [Store](./store.md) — `store_hash = id` [N:1] — The store the row belongs to; currency and time zone.
