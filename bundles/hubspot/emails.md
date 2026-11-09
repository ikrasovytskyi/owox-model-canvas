---
title: "Emails"
description: |
  Every sales e-mail logged or sent from HubSpot with subject, direction, status and rep, plus its associations. Bulk marketing sends are in Marketing emails.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/crm/email"
    title: "HubSpot API reference — Emails"
---

## Connector endpoint

- **Endpoint:** HubSpot · `emails`
- **Default destination table:** `hubspot_emails`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. HubSpot object id of the emails (hs_object_id), unique in the account. Primary key. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector; lets several HubSpot accounts share one table. |
| `hs_timestamp` | TIMESTAMP | Hs Timestamp | When the activity happened (the date to report activities by). |
| `hubspot_owner_id` | STRING | Hubspot Owner ID | FK to [Owners](./owners.md). Rep who logged or owns the activity; joins hubspot_owners.id. Activities per rep come from this field. |
| `hs_createdate` | TIMESTAMP | Hs Createdate | When the activity record was created. |
| `hs_lastmodifieddate` | TIMESTAMP | Hs Lastmodifieddate | When the activity record last changed. |
| `hs_activity_type` | STRING | Hs Activity Type | Activity type picked by the rep, when the account uses activity types. |
| `hs_email_subject` | STRING | Hs Email Subject | E-mail subject. |
| `hs_email_direction` | STRING | Hs Email Direction | EMAIL (sent from HubSpot), INCOMING_EMAIL or FORWARDED_EMAIL. |
| `hs_email_status` | STRING | Hs Email Status | Status: SENT, BOUNCED, FAILED, SCHEDULED, SENDING. |
| `hs_email_to_email` | STRING | Hs Email To Email | Recipient address (joins contacts.email). |
| `hs_email_from_email` | STRING | Hs Email From Email | Sender address. |
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

- How many sales e-mails per rep per week, and what share got a reply?
- Which deals have had no e-mail in 14 days?

## Joins

- [Owners](./owners.md) — `hubspot_owner_id = id` [N:1]
- [Contacts](./contacts.md) — `first_contact_id = id` [N:1]
- [Deals](./deals.md) — `first_deal_id = id` [N:1]
- [Companies](./companies.md) — `first_company_id = id` [N:1]
