---
title: "Companies"
description: |
  Every account with firmographics, lifecycle stage, owner, rolled-up deal facts and original source. Deals are won or lost at this level; `domain` is the join to billing accounts and e-mail domains.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/crm/companies"
    title: "HubSpot API reference — Companies"
---

## Connector endpoint

- **Endpoint:** HubSpot · `companies`
- **Default destination table:** `hubspot_companies`
- **Unique keys:** `id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. HubSpot object id of the companie (hs_object_id), unique in the account. Primary key. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector; lets several HubSpot accounts share one table. |
| `name` | STRING | Name | Company name. |
| `domain` | STRING | Domain | Website domain; HubSpot's deduplication key and the join to contact e-mail domains and to Stripe or billing accounts. |
| `industry` | STRING | Industry | Industry from HubSpot's standard list. |
| `company_type` | STRING | Company Type | Type of company: PROSPECT, PARTNER, RESELLER, VENDOR, OTHER. |
| `city` | STRING | City | City. |
| `state` | STRING | State | State or region. |
| `country` | STRING | Country | Country. |
| `numberofemployees` | INTEGER | Numberofemployees | Number of employees. |
| `annualrevenue` | FLOAT | Annualrevenue | Annual revenue as entered or enriched. |
| `lifecyclestage` | STRING | Lifecyclestage | Lifecycle stage of the company, same vocabulary as contacts. |
| `hs_lead_status` | STRING | Hs Lead Status | Lead status of the company. |
| `hubspot_owner_id` | STRING | Hubspot Owner ID | FK to [Owners](./owners.md). Owner of the company; joins hubspot_owners.id. |
| `createdate` | TIMESTAMP | Createdate | When the company was created in HubSpot. |
| `hs_lastmodifieddate` | TIMESTAMP | Hs Lastmodifieddate | When any company property last changed. The incremental filter for this table. |
| `closedate` | TIMESTAMP | Closedate | Close date of the first closed-won deal. |
| `first_deal_created_date` | TIMESTAMP | First Deal Created Date | When the first deal was created for the company. |
| `recent_deal_amount` | FLOAT | Recent Deal Amount | Amount of the most recently closed-won deal. |
| `recent_deal_close_date` | TIMESTAMP | Recent Deal Close Date | Close date of the most recently closed-won deal. |
| `total_revenue` | FLOAT | Total Revenue | Sum of closed-won deal amounts, as HubSpot rolls it up. |
| `num_associated_contacts` | INTEGER | Num Associated Contacts | Number of associated contacts. |
| `num_associated_deals` | INTEGER | Num Associated Deals | Number of associated deals. |
| `hs_num_open_deals` | INTEGER | Hs Num Open Deals | Number of open deals. |
| `hs_analytics_source` | STRING | Hs Analytics Source | Original traffic source of the company (from its first contact). |
| `hs_analytics_source_data_1` | STRING | Hs Analytics Source Data 1 | Original source drill-down 1. |
| `hs_analytics_source_data_2` | STRING | Hs Analytics Source Data 2 | Original source drill-down 2. |
| `hs_parent_company_id` | STRING | Hs Parent Company ID | Parent company id when the account is a subsidiary; joins hubspot_companies.id. |
| `website` | STRING | Website | Website URL. |
| `description` | STRING | Description | Company description. |
| `founded_year` | STRING | Founded Year | Year founded. |
| `is_public` | BOOLEAN | Is Public | true for publicly traded companies. |
| `hs_object_source` | STRING | Hs Object Source | How the record was created (FORM, IMPORT, API, CRM_UI, INTEGRATION, …). |
| `created_at` | TIMESTAMP | Created At | When HubSpot created the record (UTC). |
| `updated_at` | TIMESTAMP | Updated At | When any property of the record last changed (UTC). The field the daily window filters on. |
| `archived` | BOOLEAN | Archived | true when the record was deleted (archived) in HubSpot; archived records stop appearing in loads. |

# Example Questions

- How many accounts are in each lifecycle stage by industry and size band?
- Which accounts have open deals but no activity in 30 days?
- What is total closed-won revenue per account, and which accounts grew this quarter?

## Joins

- [Owners](./owners.md) — `hubspot_owner_id = id` [N:1] — The rep who owns the account.
