---
title: "Tasks"
description: |
  Logged calls, e-mails and to-dos against leads, contacts, accounts and deals.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_task.htm"
    title: "Salesforce API reference — Tasks"
---

## Connector endpoint

- **Endpoint:** Salesforce · `task`
- **Default destination table:** `salesforce_task`
- **Unique keys:** `id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Salesforce record id of the task (18 characters, unique across the org). Primary key. |
| `created_date` | TIMESTAMP | Created Date | When the record was created (UTC). |
| `last_modified_date` | TIMESTAMP | Last Modified Date | Last edit by a user or an integration (UTC). |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change by anyone, including Salesforce itself (UTC). The field the daily window filters on; use it, not LastModifiedDate, to decide what to reload. |
| `is_deleted` | BOOLEAN | Is Deleted | true when the record sits in the Recycle Bin; the connector reads through queryAll so deletions reach the data warehouse instead of silently vanishing. |
| `who_id` | STRING | Who ID | FK to [Leads](./leads.md). Lead or contact the task is about. |
| `what_id` | STRING | What ID | FK to [Opportunities](./opportunities.md). Account, opportunity, campaign or case the task is about (the id prefix tells which: 001 account, 006 opportunity, 701 campaign, 500 case). |
| `account_id` | STRING | Account ID | FK to [Accounts](./accounts.md). Account resolved from who_id / what_id. |
| `subject` | STRING | Subject | Call, Email, Send Letter, Send Quote, Other, or free text. |
| `task_subtype` | STRING | Task Subtype | Task, Email, ListEmail, Call: how the activity was logged. |
| `activity_type` | STRING | Activity Type | Activity type picklist (Call, Meeting, Other). |
| `status` | STRING | Status | Not Started, In Progress, Completed, Waiting on someone else, Deferred. |
| `priority` | STRING | Priority | High, Normal, Low. |
| `is_high_priority` | BOOLEAN | Is High Priority | true when priority is High. |
| `is_closed` | BOOLEAN | Is Closed | true when the status counts as closed. |
| `activity_date` | DATE | Activity Date | Due date. |
| `completed_date_time` | TIMESTAMP | Completed Date Time | When the task was completed (UTC). |
| `call_type` | STRING | Call Type | Inbound, Internal, Outbound for logged calls. |
| `call_duration_in_seconds` | INTEGER | Call Duration In Seconds | Logged call duration. |
| `owner_id` | STRING | Owner ID | Assigned user. |
| `description` | STRING | Description | Comments. |

# Example Questions

- How many calls and e-mails per rep per week, and how does that relate to won deals?
- Which open deals had no completed task in the last 14 days?

## Joins

- [Accounts](./accounts.md) — `account_id = id` [N:1]
- [Opportunities](./opportunities.md) — `what_id = id` [N:1] — When the task is logged on a deal (id prefix 006).
- [Contacts](./contacts.md) — `who_id = id` [N:1] — When the task is about a contact (id prefix 003).
- [Leads](./leads.md) — `who_id = id` [N:1] — When the task is about a lead (id prefix 00Q).
