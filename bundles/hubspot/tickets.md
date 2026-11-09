---
title: "Tickets"
description: |
  Every support ticket with pipeline, status, priority, category, channel, owner and HubSpot's time-to-close and time-to-first-reply.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/crm/tickets"
    title: "HubSpot API reference — Tickets"
---

## Connector endpoint

- **Endpoint:** HubSpot · `tickets`
- **Default destination table:** `hubspot_tickets`
- **Unique keys:** `id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. FK to [Ticket associations](./ticket-associations.md). HubSpot object id of the ticket (hs_object_id), unique in the account. Primary key. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector; lets several HubSpot accounts share one table. |
| `subject` | STRING | Subject | Ticket subject. |
| `content` | STRING | Content | Ticket description. |
| `hs_pipeline` | STRING | Hs Pipeline | FK to [Ticket pipelines](./ticket-pipelines.md). Ticket pipeline id; joins hubspot_ticket_pipelines.id. |
| `hs_pipeline_stage` | STRING | Hs Pipeline Stage | FK to [Ticket pipeline stages](./ticket-pipeline-stages.md). Current ticket status (stage) id; joins hubspot_ticket_pipeline_stages.id. |
| `hs_ticket_priority` | STRING | Hs Ticket Priority | Priority: LOW, MEDIUM, HIGH, URGENT. |
| `hs_ticket_category` | STRING | Hs Ticket Category | Category: PRODUCT_ISSUE, BILLING_ISSUE, FEATURE_REQUEST, GENERAL_INQUIRY or custom. |
| `source_type` | STRING | Source Type | Channel the ticket came from: CHAT, EMAIL, FORM, PHONE. |
| `hubspot_owner_id` | STRING | Hubspot Owner ID | FK to [Owners](./owners.md). Agent who owns the ticket; joins hubspot_owners.id. |
| `createdate` | TIMESTAMP | Createdate | When the ticket was created. |
| `hs_lastmodifieddate` | TIMESTAMP | Hs Lastmodifieddate | When any ticket property last changed. The incremental filter for this table. |
| `closed_date` | TIMESTAMP | Closed Date | When the ticket was closed. |
| `time_to_close` | INTEGER | Time To Close | Milliseconds from creation to close, as HubSpot computes it. |
| `time_to_first_agent_reply` | INTEGER | Time To First Agent Reply | Milliseconds from creation to the first agent reply. |
| `hs_resolution` | STRING | Hs Resolution | Resolution picked at close. |
| `hs_num_times_contacted` | INTEGER | Hs Num Times Contacted | Number of times the customer was contacted on this ticket. |
| `hs_object_source` | STRING | Hs Object Source | How the ticket was created. |
| `created_at` | TIMESTAMP | Created At | When HubSpot created the record (UTC). |
| `updated_at` | TIMESTAMP | Updated At | When any property of the record last changed (UTC). The field the daily window filters on. |
| `archived` | BOOLEAN | Archived | true when the record was deleted (archived) in HubSpot; archived records stop appearing in loads. |

# Example Questions

- How many tickets are open per status, priority and agent, and how long do they wait for a first reply?
- What is median time to close by category and channel this quarter?

## Joins

- [Owners](./owners.md) — `hubspot_owner_id = id` [N:1]
- [Ticket pipelines](./ticket-pipelines.md) — `hs_pipeline = id` [N:1]
- [Ticket pipeline stages](./ticket-pipeline-stages.md) — `hs_pipeline_stage = id` [N:1] — Status label and whether it counts as closed.
- [Ticket associations](./ticket-associations.md) — `id = id` [1:1]
