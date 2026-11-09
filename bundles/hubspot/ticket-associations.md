---
title: "Ticket associations"
description: |
  One row per ticket with its primary contact, company and deal and every link as JSON.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/crm/associations"
    title: "HubSpot API reference — Ticket associations"
---

## Connector endpoint

- **Endpoint:** HubSpot · `ticket_associations`
- **Default destination table:** `hubspot_ticket_associations`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. FK to [Tickets](./tickets.md). Ticket id. Primary key; joins hubspot_tickets.id. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector. |
| `subject` | STRING | Subject | Ticket subject, repeated so the link table is readable on its own. |
| `first_contact_id` | STRING | First Contact ID | FK to [Contacts](./contacts.md). The first associated contact. Joins hubspot_contacts.id. |
| `contact_associations` | JSON | Contact Associations | All associated contacts as a JSON array of {id, type}. |
| `first_company_id` | STRING | First Company ID | FK to [Companies](./companies.md). The first associated company. Joins hubspot_companies.id. |
| `company_associations` | JSON | Company Associations | All associated companies as a JSON array of {id, type}. |
| `first_deal_id` | STRING | First Deal ID | FK to [Deals](./deals.md). The first associated deal. Joins hubspot_deals.id. |
| `deal_associations` | JSON | Deal Associations | All associated deals as a JSON array of {id, type}. |

# Example Questions

- Which deals and companies generate the most tickets after closing?

## Joins

- [Tickets](./tickets.md) — `id = id` [1:1]
- [Contacts](./contacts.md) — `first_contact_id = id` [N:1]
- [Companies](./companies.md) — `first_company_id = id` [N:1]
- [Deals](./deals.md) — `first_deal_id = id` [N:1]
