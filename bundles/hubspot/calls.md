---
title: "Calls"
description: |
  Every logged call with direction, duration, outcome and rep, plus the contact, deal and company it was logged on.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/crm/calls"
    title: "HubSpot API reference — Calls"
---

## Connector endpoint

- **Endpoint:** HubSpot · `calls`
- **Default destination table:** `hubspot_calls`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. HubSpot object id of the calls (hs_object_id), unique in the account. Primary key. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector; lets several HubSpot accounts share one table. |
| `hs_timestamp` | TIMESTAMP | Hs Timestamp | When the activity happened (the date to report activities by). |
| `hubspot_owner_id` | STRING | Hubspot Owner ID | FK to [Owners](./owners.md). Rep who logged or owns the activity; joins hubspot_owners.id. Activities per rep come from this field. |
| `hs_createdate` | TIMESTAMP | Hs Createdate | When the activity record was created. |
| `hs_lastmodifieddate` | TIMESTAMP | Hs Lastmodifieddate | When the activity record last changed. |
| `hs_activity_type` | STRING | Hs Activity Type | Activity type picked by the rep, when the account uses activity types. |
| `hs_call_title` | STRING | Hs Call Title | Call title. |
| `hs_call_direction` | STRING | Hs Call Direction | INBOUND or OUTBOUND. |
| `hs_call_duration` | INTEGER | Hs Call Duration | Call duration in milliseconds. |
| `hs_call_disposition` | STRING | Hs Call Disposition | Outcome id of the call (connected, left voicemail, no answer, busy, wrong number…), an id that maps to the account's call outcomes. |
| `hs_call_status` | STRING | Hs Call Status | Status: COMPLETED, BUSY, NO_ANSWER, FAILED, CANCELED, CONNECTING, IN_PROGRESS, QUEUED. |
| `hs_call_body` | STRING | Hs Call Body | Notes typed on the call. |
| `first_contact_id` | STRING | First Contact ID | FK to [Contacts](./contacts.md). The first contact the activity is logged on. Joins hubspot_contacts.id. |
| `contact_associations` | JSON | Contact Associations | All contacts the activity is associated with, as a JSON array of {id, type}. |
| `first_deal_id` | STRING | First Deal ID | FK to [Deals](./deals.md). The first deal the activity is logged on. Joins hubspot_deals.id; activities per deal and per stage come from here. |
| `deal_associations` | JSON | Deal Associations | All deals the activity is associated with, as a JSON array of {id, type}. |
| `first_company_id` | STRING | First Company ID | FK to [Companies](./companies.md). The first company the activity is logged on. Joins hubspot_companies.id. |
| `company_associations` | JSON | Company Associations | All companies the activity is associated with, as a JSON array of {id, type}. |
| `created_at` | TIMESTAMP | Created At | When HubSpot created the record (UTC). |
| `updated_at` | TIMESTAMP | Updated At | When any property of the record last changed (UTC). The field the daily window filters on. |
| `archived` | BOOLEAN | Archived | true when the record was deleted (archived) in HubSpot; archived records stop appearing in loads. |

# Example Questions

- How many calls per rep per week, and how many connected?
- How many calls did it take to win a deal, by stage?

## Joins

- [Owners](./owners.md) — `hubspot_owner_id = id` [N:1] — Calls per rep.
- [Contacts](./contacts.md) — `first_contact_id = id` [N:1]
- [Deals](./deals.md) — `first_deal_id = id` [N:1] — Touches per deal and per stage.
- [Companies](./companies.md) — `first_company_id = id` [N:1]
