---
title: "Events"
description: |
  Calendar events (meetings, demos) against leads, contacts, accounts and deals.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_event.htm"
    title: "Salesforce API reference — Events"
---

## Connector endpoint

- **Endpoint:** Salesforce · `event`
- **Default destination table:** `salesforce_event`
- **Unique keys:** `id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Salesforce record id of the event (18 characters, unique across the org). Primary key. |
| `created_date` | TIMESTAMP | Created Date | When the record was created (UTC). |
| `last_modified_date` | TIMESTAMP | Last Modified Date | Last edit by a user or an integration (UTC). |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change by anyone, including Salesforce itself (UTC). The field the daily window filters on; use it, not LastModifiedDate, to decide what to reload. |
| `is_deleted` | BOOLEAN | Is Deleted | true when the record sits in the Recycle Bin; the connector reads through queryAll so deletions reach the data warehouse instead of silently vanishing. |
| `who_id` | STRING | Who ID | FK to [Contacts](./contacts.md). Lead or contact the event is with. |
| `what_id` | STRING | What ID | FK to [Opportunities](./opportunities.md). Account, opportunity, campaign or case the event is about. |
| `account_id` | STRING | Account ID | FK to [Accounts](./accounts.md). Account resolved from who_id / what_id. |
| `subject` | STRING | Subject | Meeting, Call, Email, Other, or free text. |
| `event_subtype` | STRING | Event Subtype | Event (meeting) or other subtype. |
| `activity_type` | STRING | Activity Type | Event type picklist. |
| `location` | STRING | Location | Location or meeting link. |
| `is_all_day_event` | BOOLEAN | Is All Day Event | true for all-day events. |
| `activity_date` | DATE | Activity Date | Event day. |
| `activity_date_time` | TIMESTAMP | Activity Date Time | Event start (UTC). |
| `start_date_time` | TIMESTAMP | Start Date Time | Start (UTC). |
| `end_date_time` | TIMESTAMP | End Date Time | End (UTC). |
| `duration_in_minutes` | INTEGER | Duration In Minutes | Duration in minutes. |
| `show_as` | STRING | Show As | Busy, OutOfOffice, Free. |
| `is_child` | BOOLEAN | Is Child | true for invitee copies of a group event; filter them out when counting meetings. |
| `is_group_event` | BOOLEAN | Is Group Event | true when the event has invitees. |
| `owner_id` | STRING | Owner ID | Assigned user. |
| `description` | STRING | Description | Notes. |

# Example Questions

- How many meetings per won deal versus lost deal?
- Which reps hold the most meetings, and on which stages?

## Joins

- [Accounts](./accounts.md) — `account_id = id` [N:1]
- [Opportunities](./opportunities.md) — `what_id = id` [N:1] — When the meeting is on a deal (id prefix 006).
- [Contacts](./contacts.md) — `who_id = id` [N:1] — When the meeting is with a contact (id prefix 003).
