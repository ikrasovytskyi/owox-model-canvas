---
title: "Accounts"
description: |
  Companies and organisations in the CRM: customers, prospects, partners, with industry, size, owner and the parent account for hierarchies.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_account.htm"
    title: "Salesforce API reference — Accounts"
---

## Connector endpoint

- **Endpoint:** Salesforce · `account`
- **Default destination table:** `salesforce_account`
- **Unique keys:** `id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Salesforce record id of the account (18 characters, unique across the org). Primary key. |
| `created_date` | TIMESTAMP | Created Date | When the record was created (UTC). |
| `last_modified_date` | TIMESTAMP | Last Modified Date | Last edit by a user or an integration (UTC). |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change by anyone, including Salesforce itself (UTC). The field the daily window filters on; use it, not LastModifiedDate, to decide what to reload. |
| `is_deleted` | BOOLEAN | Is Deleted | true when the record sits in the Recycle Bin; the connector reads through queryAll so deletions reach the data warehouse instead of silently vanishing. |
| `name` | STRING | Name | Account (company) name. |
| `account_type` | STRING | Account Type | Relationship type picklist: Customer - Direct, Customer - Channel, Prospect, Partner, Other. |
| `parent_id` | STRING | Parent ID | FK to [Accounts](./accounts.md). Parent account id for subsidiaries and hierarchies; null at the top. |
| `industry` | STRING | Industry | Industry picklist value. |
| `annual_revenue` | FLOAT | Annual Revenue | Annual revenue as entered by sales (firmographic, not from your books). |
| `number_of_employees` | INTEGER | Number Of Employees | Employee count as entered by sales. |
| `ownership` | STRING | Ownership | Public, Private, Subsidiary, Other. |
| `rating` | STRING | Rating | Hot, Warm, Cold. |
| `account_source` | STRING | Account Source | Where the account came from (Web, Phone Inquiry, Partner Referral, Trade Show…), copied from the converted lead. |
| `account_number` | STRING | Account Number | Your own account number if sales fills it. |
| `website` | STRING | Website | Company website; the join key to your web-analytics domain data. |
| `phone` | STRING | Phone | Main phone. |
| `billing_street` | STRING | Billing Street | Billing street address. |
| `billing_city` | STRING | Billing City | Billing city. |
| `billing_state` | STRING | Billing State | Billing state or province. |
| `billing_postal_code` | STRING | Billing Postal Code | Billing postal code. |
| `billing_country` | STRING | Billing Country | Billing country. |
| `owner_id` | STRING | Owner ID | FK to [Users](./users.md). Account owner (User id): the rep or team who owns the relationship. |
| `created_by_id` | STRING | Created By ID | User who created the record. |
| `last_activity_date` | DATE | Last Activity Date | Most recent of the last completed task and the last event on the account; empty means no logged activity. |
| `description` | STRING | Description | Free-text notes. |

# Example Questions

- Which `accounts` carry open pipeline right now, and who owns them?
- How many customer accounts did we add this quarter, by industry and by source?
- Which accounts have no logged activity for 60 days?

## Joins

- [Accounts](./accounts.md) — `parent_id = id` [N:1] — Subsidiary to its parent account.
- [Users](./users.md) — `owner_id = id` [N:1] — Account owner.
