---
title: "Owners"
description: |
  The users who own records: name, e-mail and teams. The lookup behind `hubspot_owner_id` everywhere; rep and team roll-ups go through it.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/crm/owners"
    title: "HubSpot API reference — Owners"
---

## Connector endpoint

- **Endpoint:** HubSpot · `owners`
- **Default destination table:** `hubspot_owners`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Owner id. Primary key; hubspot_owner_id on every object joins it. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector. |
| `email` | STRING | Email | Owner's e-mail (the HubSpot user login). |
| `first_name` | STRING | First Name | First name. |
| `last_name` | STRING | Last Name | Last name. |
| `user_id` | INTEGER | User ID | HubSpot user id behind the owner. |
| `owner_type` | STRING | Owner Type | PERSON or QUEUE. |
| `teams` | JSON | Teams | Teams the owner belongs to, as a JSON array of {id, name, primary}. |
| `created_at` | TIMESTAMP | Created At | When HubSpot created the record (UTC). |
| `updated_at` | TIMESTAMP | Updated At | When any property of the record last changed (UTC). The field the daily window filters on. |
| `archived` | BOOLEAN | Archived | true when the record was deleted (archived) in HubSpot; archived records stop appearing in loads. |

# Example Questions

- Which reps own the most pipeline, and how does their win rate compare?
- Which team does each owner belong to, for roll-ups by team?
