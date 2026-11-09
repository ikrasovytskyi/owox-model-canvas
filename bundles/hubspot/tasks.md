---
title: "Tasks"
description: |
  Every task with status, priority, type and completion date, plus rep and associations.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/crm/tasks"
    title: "HubSpot API reference — Tasks"
---

## Connector endpoint

- **Endpoint:** HubSpot · `tasks`
- **Default destination table:** `hubspot_tasks`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. HubSpot object id of the tasks (hs_object_id), unique in the account. Primary key. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector; lets several HubSpot accounts share one table. |
| `hs_timestamp` | TIMESTAMP | Hs Timestamp | When the activity happened (the date to report activities by). |
| `hubspot_owner_id` | STRING | Hubspot Owner ID | FK to [Owners](./owners.md). Rep who logged or owns the activity; joins hubspot_owners.id. Activities per rep come from this field. |
| `hs_createdate` | TIMESTAMP | Hs Createdate | When the activity record was created. |
| `hs_lastmodifieddate` | TIMESTAMP | Hs Lastmodifieddate | When the activity record last changed. |
| `hs_activity_type` | STRING | Hs Activity Type | Activity type picked by the rep, when the account uses activity types. |
| `hs_task_subject` | STRING | Hs Task Subject | Task title. |
| `hs_task_body` | STRING | Hs Task Body | Task notes. |
| `hs_task_status` | STRING | Hs Task Status | NOT_STARTED, IN_PROGRESS, WAITING, COMPLETED, DEFERRED. |
| `hs_task_priority` | STRING | Hs Task Priority | NONE, LOW, MEDIUM, HIGH. |
| `hs_task_type` | STRING | Hs Task Type | EMAIL, CALL, TODO, LINKED_IN_MESSAGE and others. |
| `hs_task_completion_date` | TIMESTAMP | Hs Task Completion Date | When the task was completed. |
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

- How many tasks are overdue per rep?
- What share of tasks is completed on time?

## Joins

- [Owners](./owners.md) — `hubspot_owner_id = id` [N:1]
- [Contacts](./contacts.md) — `first_contact_id = id` [N:1]
- [Deals](./deals.md) — `first_deal_id = id` [N:1]
- [Companies](./companies.md) — `first_company_id = id` [N:1]
