---
title: "Customers"
description: |
  Everyone the company sells to, including sub-customers and projects, with open balance, terms, currency and billing address. A sub-customer or project names its parent customer in `parent_id`; that self-reference is not drawn as a join.
tags: ["owox", "connector", "quickbooks"]
type: "OWOX Data Mart"
sources:
  - id: "vendor-reference"
    resource: "https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/customer"
    title: "Intuit QuickBooks Online Accounting API reference — customer"
---

## Connector endpoint

- **Endpoint:** QuickBooks · `customer`
- **Default destination table:** `quickbooks_customer`
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
| `company_name` | STRING | Business name of the customer. |
| `given_name` | STRING | First name of the customer contact. |
| `family_name` | STRING | Last name of the customer contact. |
| `primary_email` | STRING | Primary email address of the customer. |
| `primary_phone` | STRING | Primary phone number of the customer. |
| `balance` | FLOAT | Open A/R balance for the customer. |
| `balance_with_jobs` | FLOAT | Open balance of the customer including all of its sub-customers and jobs, in the home currency. |
| `currency_id` | STRING | Currency code of the document, for example USD; empty when multicurrency is off. |
| `currency_name` | STRING | Currency name that goes with currency_id, for example United States Dollar. |
| `taxable` | BOOLEAN | True when the record is subject to sales tax. |
| `job` | BOOLEAN | True for sub-customers (projects). |
| `parent_id` | STRING | Id of the parent record in the same table (the parent account, customer or item); empty for top-level records. |
| `parent_name` | STRING | Name of the parent record that parent_id points to. |
| `payment_method_id` | STRING | Id of the preferred payment method; joins to the QuickBooks payment method list. |
| `payment_method_name` | STRING | Name of the preferred payment method that payment_method_id points to, for example Cash or Check. |
| `sales_term_id` | STRING | Id of the payment terms applied (for example Net 30); joins to the QuickBooks terms list. |
| `sales_term_name` | STRING | Name of the payment terms that sales_term_id points to. |
| `bill_addr_city` | STRING | City of the billing address. |
| `bill_addr_country` | STRING | Country of the customer's billing address. |
| `bill_addr_postal_code` | STRING | Postal or ZIP code of the billing address. |
| `bill_addr` | JSON | Full billing address as JSON. |
| `active` | BOOLEAN | True when the record is active; false when it has been made inactive (QuickBooks never hard-deletes lists). |

# Example Questions

- Which `customers` owe money right now, and how much is on sub-customer projects?
- How many active customers were added each month, and from which parent accounts?
- Which customers are taxable and on which sales terms?

## Joins

- [Company info](./company-info.md) — `realm_id = realm_id` [N:1] — The company the row belongs to.
  - Description: The company the row belongs to.
