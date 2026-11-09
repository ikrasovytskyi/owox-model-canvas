---
title: "Vendors"
description: |
  Everyone the company buys from, with open payable balance, terms and currency.
tags: ["owox", "connector", "quickbooks"]
type: "OWOX Data Mart"
sources:
  - id: "vendor-reference"
    resource: "https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/vendor"
    title: "Intuit QuickBooks Online Accounting API reference — vendor"
---

## Connector endpoint

- **Endpoint:** QuickBooks · `vendor`
- **Default destination table:** `quickbooks_vendor`
- **Unique keys:** `realm_id`, `id`
- **Time series:** no, one current row per key

# Schema

| Column | Type | Description |
|--------|------|-------------|
| `id` | STRING | PK. QuickBooks entity Id (string). Primary key together with realm_id. |
| `realm_id` | STRING | PK. Company (realm) id, stamped from the RealmId parameter. |
| `sync_token` | STRING | Version counter; changes on every edit. |
| `create_time` | TIMESTAMP | When the record was created in QuickBooks (UTC timestamp). |
| `last_updated_time` | TIMESTAMP | Incremental cursor. |
| `display_name` | STRING | Name shown on forms and lists, unique across customers, vendors and employees in the company. |
| `company_name` | STRING | Business name of the vendor. |
| `primary_email` | STRING | Primary email address of the vendor. |
| `primary_phone` | STRING | Primary phone number of the vendor. |
| `balance` | FLOAT | Open A/P balance owed to the vendor. |
| `currency_id` | STRING | Currency code of the document, for example USD; empty when multicurrency is off. |
| `currency_name` | STRING | Currency name that goes with currency_id, for example United States Dollar. |
| `vendor_1099` | BOOLEAN | True when the vendor is tracked for 1099 tax reporting. |
| `acct_num` | STRING | Account number the vendor uses for the company; optional and can be empty. |
| `term_id` | STRING | Id of the payment terms agreed with the vendor; joins to the QuickBooks terms list. |
| `term_name` | STRING | Name of the payment terms that term_id points to. |
| `bill_addr_country` | STRING | Country of the vendor's billing address. |
| `bill_addr` | JSON | Billing address as JSON: line1 to line5, city, country subdivision, postal code and country. |
| `active` | BOOLEAN | True when the record is active; false when it has been made inactive (QuickBooks never hard-deletes lists). |

# Example Questions

- Which `vendors` do we owe, and how much per vendor?
- How many vendors have been added or deactivated this year?

## Joins

- [Company info](./company-info.md) — `realm_id = realm_id` [N:1] — The company the row belongs to.
  - Description: The company the row belongs to.
