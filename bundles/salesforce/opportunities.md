---
title: "Opportunities"
description: |
  Deals in the pipeline and closed: amount, stage, probability, forecast category, close date, owner, account and primary campaign.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_opportunity.htm"
    title: "Salesforce API reference — Opportunities"
---

## Connector endpoint

- **Endpoint:** Salesforce · `opportunity`
- **Default destination table:** `salesforce_opportunity`
- **Unique keys:** `id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Salesforce record id of the opportunity (18 characters, unique across the org). Primary key. |
| `created_date` | TIMESTAMP | Created Date | When the record was created (UTC). |
| `last_modified_date` | TIMESTAMP | Last Modified Date | Last edit by a user or an integration (UTC). |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change by anyone, including Salesforce itself (UTC). The field the daily window filters on; use it, not LastModifiedDate, to decide what to reload. |
| `is_deleted` | BOOLEAN | Is Deleted | true when the record sits in the Recycle Bin; the connector reads through queryAll so deletions reach the data warehouse instead of silently vanishing. |
| `account_id` | STRING | Account ID | FK to [Accounts](./accounts.md). Account the deal is with. |
| `name` | STRING | Name | Deal name. |
| `stage_name` | STRING | Stage Name | FK to [Opportunity stages](./opportunity-stages.md). Current stage (Prospecting, Qualification, Needs Analysis, Value Proposition, Id. Decision Makers, Perception Analysis, Proposal/Price Quote, Negotiation/Review, Closed Won, Closed Lost); joins Opportunity stages for order, probability and forecast category. |
| `amount` | FLOAT | Amount | Deal amount in the org currency (or the record currency when multi-currency is on). Changes over the life of the deal; see Opportunity history for the trail. |
| `probability` | FLOAT | Probability | Win probability in percent, defaulted from the stage and editable by the rep. |
| `expected_revenue` | FLOAT | Expected Revenue | amount × probability / 100, as Salesforce computes it. |
| `total_opportunity_quantity` | FLOAT | Total Opportunity Quantity | Total quantity across line items when products are used. |
| `close_date` | DATE | Close Date | Expected or actual close date; the date revenue forecasts and cycle length are measured on. |
| `opportunity_type` | STRING | Opportunity Type | Existing Customer - Upgrade, Existing Customer - Replacement, Existing Customer - Downgrade, New Customer. |
| `next_step` | STRING | Next Step | Rep's next step note. |
| `lead_source` | STRING | Lead Source | Source copied from the lead or chosen by the rep; first-touch attribution as sales sees it. |
| `is_closed` | BOOLEAN | Is Closed | true in a closed stage (won or lost). |
| `is_won` | BOOLEAN | Is Won | true in a Closed Won stage. Win rate = won / closed. |
| `forecast_category` | STRING | Forecast Category | Forecast bucket API value: Pipeline, BestCase, Forecast (Commit), Omitted, Closed. |
| `forecast_category_name` | STRING | Forecast Category Name | Forecast bucket label: Pipeline, Best Case, Commit, Omitted, Closed. |
| `campaign_id` | STRING | Campaign ID | FK to [Campaigns](./campaigns.md). Primary campaign source: the one campaign Salesforce credits the deal to. |
| `has_opportunity_line_item` | BOOLEAN | Has Opportunity Line Item | true when products are attached; then Opportunity line items hold the detail. |
| `pricebook_2_id` | STRING | Pricebook 2 ID | FK to [Price books](./price-books.md). Price book used for the line items. |
| `owner_id` | STRING | Owner ID | FK to [Users](./users.md). Deal owner (User id): the rep whose pipeline this is. |
| `created_by_id` | STRING | Created By ID | User who created the deal. |
| `last_stage_change_date` | TIMESTAMP | Last Stage Change Date | When the stage last changed; days in current stage = today minus this. |
| `last_activity_date` | DATE | Last Activity Date | Most recent completed task or event logged on the deal; a stale deal has none for weeks. |
| `fiscal_year` | INTEGER | Fiscal Year | Fiscal year of the close date. |
| `fiscal_quarter` | INTEGER | Fiscal Quarter | Fiscal quarter of the close date (1–4). |
| `description` | STRING | Description | Free-text notes. |

# Example Questions

- What is the open pipeline by stage and by owner, and how much of it closes this quarter?
- What is our win rate (won / closed) and median cycle length (close_date − created_date) for won deals, by type and by lead source?
- Which open deals have no activity for 30 days or a close date in the past?

## Joins

- [Accounts](./accounts.md) — `account_id = id` [N:1]
- [Users](./users.md) — `owner_id = id` [N:1] — Deal owner: pipeline by rep.
- [Opportunity stages](./opportunity-stages.md) — `stage_name = master_label` [N:1] — Stage order, default probability and forecast category.
- [Campaigns](./campaigns.md) — `campaign_id = id` [N:1] — Primary campaign source.
- [Price books](./price-books.md) — `pricebook_2_id = id` [N:1]
