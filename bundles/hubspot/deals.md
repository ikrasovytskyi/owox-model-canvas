---
title: "Deals"
description: |
  Every deal with amount, pipeline, current stage, probability, close date, owner, type, forecast category, won and lost reasons and original source. Pipeline value, bookings, win rate, sales cycle and rep performance all start here.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/crm/deals"
    title: "HubSpot API reference — Deals"
---

## Connector endpoint

- **Endpoint:** HubSpot · `deals`
- **Default destination table:** `hubspot_deals`
- **Unique keys:** `id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. FK to [Deal associations](./deal-associations.md). HubSpot object id of the deal (hs_object_id), unique in the account. Primary key. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector; lets several HubSpot accounts share one table. |
| `dealname` | STRING | Dealname | Deal name. |
| `amount` | FLOAT | Amount | Deal amount in the deal's currency. Pipeline value is SUM(amount) over open deals; bookings is SUM(amount) over closed-won deals by closedate. |
| `amount_in_home_currency` | FLOAT | Amount In Home Currency | Deal amount converted to the account's home currency. |
| `deal_currency_code` | STRING | Deal Currency Code | ISO currency code of amount. |
| `pipeline` | STRING | Pipeline | FK to [Deal pipelines](./deal-pipelines.md). Pipeline id; joins hubspot_deal_pipelines.id. |
| `dealstage` | STRING | Dealstage | FK to [Deal pipeline stages](./deal-pipeline-stages.md). Current stage id; joins hubspot_deal_pipeline_stages.id for the label, order and win probability. |
| `hs_deal_stage_probability` | FLOAT | Hs Deal Stage Probability | Win probability of the current stage (0–1) as configured on the pipeline. |
| `hs_is_closed` | BOOLEAN | Hs Is Closed | true when the deal is in a closed stage (won or lost). |
| `hs_is_closed_won` | BOOLEAN | Hs Is Closed Won | true when the deal is closed won. |
| `hs_is_closed_lost` | BOOLEAN | Hs Is Closed Lost | true when the deal is closed lost. |
| `hs_closed_amount` | FLOAT | Hs Closed Amount | Amount counted as closed won (0 for open and lost deals). |
| `hs_forecast_amount` | FLOAT | Hs Forecast Amount | Weighted amount: amount × forecast probability. |
| `hs_projected_amount` | FLOAT | Hs Projected Amount | Amount × stage probability for open deals. |
| `hs_manual_forecast_category` | STRING | Hs Manual Forecast Category | Forecast category set by the rep: OMIT, PIPELINE, BEST_CASE, COMMIT, CLOSED. |
| `dealtype` | STRING | Dealtype | newbusiness or existingbusiness. |
| `closedate` | TIMESTAMP | Closedate | Expected close date for open deals, actual close date once closed. Bookings are dated by this field. |
| `createdate` | TIMESTAMP | Createdate | When the deal was created. Pipeline created and sales velocity start here. |
| `hs_lastmodifieddate` | TIMESTAMP | Hs Lastmodifieddate | When any deal property last changed. The incremental filter for this table. |
| `hs_v2_date_entered_current_stage` | TIMESTAMP | Hs V2 Date Entered Current Stage | When the deal entered its current stage; days in stage = now minus this. |
| `hs_v2_time_in_current_stage` | INTEGER | Hs V2 Time In Current Stage | Milliseconds the deal has spent in its current stage, as HubSpot computes it. |
| `hs_days_to_close` | INTEGER | Hs Days To Close | Days from createdate to closedate (sales cycle length) as HubSpot computes it. |
| `hubspot_owner_id` | STRING | Hubspot Owner ID | FK to [Owners](./owners.md). Deal owner (rep); joins hubspot_owners.id. Owner performance is deals and amounts grouped by this field. |
| `hs_created_by_user_id` | STRING | Hs Created By User ID | HubSpot user who created the deal. |
| `closed_won_reason` | STRING | Closed Won Reason | Free-text or picklist reason the deal was won. |
| `closed_lost_reason` | STRING | Closed Lost Reason | Free-text or picklist reason the deal was lost. |
| `hs_analytics_source` | STRING | Hs Analytics Source | Original traffic source of the deal's first associated contact: where the deal came from. |
| `hs_analytics_source_data_1` | STRING | Hs Analytics Source Data 1 | Original source drill-down 1. |
| `hs_analytics_source_data_2` | STRING | Hs Analytics Source Data 2 | Original source drill-down 2. |
| `hs_campaign` | STRING | Hs Campaign | Marketing campaign id associated with the deal. |
| `num_associated_contacts` | INTEGER | Num Associated Contacts | Number of contacts on the deal. |
| `hs_num_of_associated_line_items` | INTEGER | Hs Num Of Associated Line Items | Number of line items on the deal. |
| `num_notes` | INTEGER | Num Notes | Number of sales activities logged on the deal. |
| `notes_last_contacted` | TIMESTAMP | Notes Last Contacted | Last time a contact on this deal was contacted. |
| `hs_latest_meeting_activity` | TIMESTAMP | Hs Latest Meeting Activity | Latest meeting activity on the deal. |
| `hs_next_step` | STRING | Hs Next Step | Next step typed by the rep. |
| `hs_priority` | STRING | Hs Priority | Priority: low, medium, high. |
| `hs_deal_score` | FLOAT | Hs Deal Score | HubSpot's deal score (Sales Hub Enterprise). |
| `hs_mrr` | FLOAT | Hs Mrr | Monthly recurring revenue from the deal's recurring line items. |
| `hs_arr` | FLOAT | Hs Arr | Annual recurring revenue from the deal's recurring line items. |
| `hs_tcv` | FLOAT | Hs Tcv | Total contract value from line items. |
| `hs_acv` | FLOAT | Hs Acv | Annual contract value from line items. |
| `description` | STRING | Description | Deal description. |
| `created_at` | TIMESTAMP | Created At | When HubSpot created the record (UTC). |
| `updated_at` | TIMESTAMP | Updated At | When any property of the record last changed (UTC). The field the daily window filters on. |
| `archived` | BOOLEAN | Archived | true when the record was deleted (archived) in HubSpot; archived records stop appearing in loads. |

# Example Questions

- What is the open pipeline by stage, in count and amount, and how did it change since last week?
- What is our win rate and average sales cycle (hs_days_to_close) by owner, deal type and original source?
- How much did we book (closed won by closedate) this month against last month, and what is at risk in BEST_CASE and COMMIT?

## Joins

- [Owners](./owners.md) — `hubspot_owner_id = id` [N:1] — Owner performance: deals and amounts per rep.
- [Deal pipelines](./deal-pipelines.md) — `pipeline = id` [N:1]
- [Deal pipeline stages](./deal-pipeline-stages.md) — `dealstage = id` [N:1] — Stage label, order and win probability.
- [Deal associations](./deal-associations.md) — `id = id` [1:1] — The deal's contacts, companies and line items.
