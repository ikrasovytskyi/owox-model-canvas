---
title: "Cases"
description: |
  Support cases with status, priority, origin, reason, the contact and account, and when they closed.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm"
    title: "Salesforce API reference — Cases"
---

## Connector endpoint

- **Endpoint:** Salesforce · `case`
- **Default destination table:** `salesforce_case`
- **Unique keys:** `id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Salesforce record id of the case (18 characters, unique across the org). Primary key. |
| `created_date` | TIMESTAMP | Created Date | When the record was created (UTC). |
| `last_modified_date` | TIMESTAMP | Last Modified Date | Last edit by a user or an integration (UTC). |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change by anyone, including Salesforce itself (UTC). The field the daily window filters on; use it, not LastModifiedDate, to decide what to reload. |
| `is_deleted` | BOOLEAN | Is Deleted | true when the record sits in the Recycle Bin; the connector reads through queryAll so deletions reach the data warehouse instead of silently vanishing. |
| `case_number` | STRING | Case Number | Human-readable case number. |
| `contact_id` | STRING | Contact ID | FK to [Contacts](./contacts.md). Contact who raised the case. |
| `account_id` | STRING | Account ID | FK to [Accounts](./accounts.md). Account the case belongs to. |
| `parent_id` | STRING | Parent ID | Parent case, if any. |
| `case_type` | STRING | Case Type | Mechanical, Electrical, Electronic, Structural, Other. |
| `status` | STRING | Status | New, Working, Escalated, Closed (or your own values). |
| `reason` | STRING | Reason | Why the case was opened (Installation, Equipment Complexity, Performance, Breakdown, Equipment Design, Feedback, Other). |
| `origin` | STRING | Origin | Channel the case came in through: Phone, Email, Web. |
| `subject` | STRING | Subject | Case subject. |
| `priority` | STRING | Priority | High, Medium, Low. |
| `is_closed` | BOOLEAN | Is Closed | true when closed. |
| `closed_date` | TIMESTAMP | Closed Date | When the case was closed; time to resolve = closed_date − created_date. |
| `is_escalated` | BOOLEAN | Is Escalated | true when escalated. |
| `owner_id` | STRING | Owner ID | Case owner (User or Queue id). |
| `supplied_email` | STRING | Supplied Email | E-mail given by the requester on web-to-case. |
| `supplied_company` | STRING | Supplied Company | Company given by the requester on web-to-case. |
| `description` | STRING | Description | Case description. |

# Example Questions

- How many open `cases` sit on accounts with open pipeline or renewals?
- What is the time to close by priority and origin?

## Joins

- [Accounts](./accounts.md) — `account_id = id` [N:1]
- [Contacts](./contacts.md) — `contact_id = id` [N:1]
