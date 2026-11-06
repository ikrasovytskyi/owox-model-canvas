---
title: "Contact associations"
description: |
  One row per contact with its primary company and every associated company as JSON. Rolls contact-level funnel dates up to the account.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/crm/associations"
    title: "HubSpot API reference — Contact associations"
---

## Connector endpoint

- **Endpoint:** HubSpot · `contact_associations`
- **Default destination table:** `hubspot_contact_associations`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. FK to [Contacts](./contacts.md). Contact id. Primary key; joins hubspot_contacts.id. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector. |
| `email` | STRING | Email | Contact e-mail, repeated here so the link table is readable on its own. |
| `first_company_id` | STRING | First Company ID | FK to [Companies](./companies.md). The first associated company (HubSpot lists the primary company first). Joins hubspot_companies.id. |
| `company_associations` | JSON | Company Associations | All associated companies as a JSON array of {id, type}; type contact_to_company is the primary link, contact_to_company_unlabeled the others. |

# Example Questions

- Which contacts belong to which company, so contact-level funnel dates roll up to the account?
- How many contacts without a company do we have?

## Joins

- [Contacts](./contacts.md) — `id = id` [1:1]
- [Companies](./companies.md) — `first_company_id = id` [N:1] — The primary company of the contact.
