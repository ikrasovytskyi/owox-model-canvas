---
title: "Deal pipelines"
description: |
  The deal pipelines of the account: id, label and order.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/crm/pipelines"
    title: "HubSpot API reference — Deal pipelines"
---

## Connector endpoint

- **Endpoint:** HubSpot · `deal_pipelines`
- **Default destination table:** `hubspot_deal_pipelines`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Pipeline id. Primary key. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector. |
| `label` | STRING | Label | Pipeline name. |
| `display_order` | INTEGER | Display Order | Position of the pipeline in HubSpot's list. |
| `stage_count` | INTEGER | Stage Count | Number of stages (counted from the stages array). |
| `archived` | BOOLEAN | Archived | true when the pipeline was deleted. |
| `created_at` | TIMESTAMP | Created At | When the pipeline was created. |
| `updated_at` | TIMESTAMP | Updated At | When the pipeline last changed. |

# Example Questions

- Which pipelines exist and how many stages does each have?
