---
title: "Deal associations"
description: |
  One row per deal with its primary contact and company and every associated contact, company and line item as JSON. The path from a deal to the account and to the contact's traffic source.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/crm/associations"
    title: "HubSpot API reference — Deal associations"
---

## Connector endpoint

- **Endpoint:** HubSpot · `deal_associations`
- **Default destination table:** `hubspot_deal_associations`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. FK to [Deals](./deals.md). Deal id. Primary key; joins hubspot_deals.id. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector. |
| `dealname` | STRING | Dealname | Deal name, repeated so the link table is readable on its own. |
| `first_contact_id` | STRING | First Contact ID | FK to [Contacts](./contacts.md). The first associated contact. Joins hubspot_contacts.id. |
| `contact_associations` | JSON | Contact Associations | All associated contacts as a JSON array of {id, type}. |
| `first_company_id` | STRING | First Company ID | FK to [Companies](./companies.md). The first associated company (the primary company). Joins hubspot_companies.id. |
| `company_associations` | JSON | Company Associations | All associated companies as a JSON array of {id, type}. |
| `line_item_associations` | JSON | Line Item Associations | All line items of the deal as a JSON array of {id, type}; joins hubspot_line_items.id. |

# Example Questions

- Which company and which contact does each deal belong to, so deals can be joined to the account's traffic source and ad spend?
- How many deals have no contact at all?

## Joins

- [Deals](./deals.md) — `id = id` [1:1]
- [Contacts](./contacts.md) — `first_contact_id = id` [N:1] — The primary contact; the path from a deal to its traffic source and form.
- [Companies](./companies.md) — `first_company_id = id` [N:1] — The account the deal was won or lost at.
