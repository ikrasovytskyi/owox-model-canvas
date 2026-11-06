---
title: "Deal pipeline stages"
description: |
  Every stage of every deal pipeline with label, order, win probability and the closed flag. Stage ids are unique in the account, so `deals.dealstage` joins `id` directly.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/crm/pipelines"
    title: "HubSpot API reference — Deal pipeline stages"
---

## Connector endpoint

- **Endpoint:** HubSpot · `deal_pipeline_stages`
- **Default destination table:** `hubspot_deal_pipeline_stages`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Stage id, unique in the account. Primary key; deals.dealstage joins it. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector. |
| `label` | STRING | Label | Stage name as shown in HubSpot. |
| `display_order` | INTEGER | Display Order | Position of the stage in its pipeline, 0 first. |
| `is_closed` | BOOLEAN | Is Closed | true for the closed stages (won and lost for deals, closed for tickets). |
| `probability` | FLOAT | Probability | Win probability of the stage, 0–1 (deal pipelines only; 1.0 = closed won, 0.0 = closed lost). |
| `ticket_state` | STRING | Ticket State | OPEN or CLOSED (ticket pipelines only). |
| `archived` | BOOLEAN | Archived | true when the stage was removed from the pipeline. |
| `created_at` | TIMESTAMP | Created At | When the stage was created. |
| `updated_at` | TIMESTAMP | Updated At | When the stage last changed. |

# Example Questions

- What is the stage order and configured win probability behind the weighted pipeline?
- Which stages count as closed, so `hs_is_closed` can be rebuilt from the stage table?
