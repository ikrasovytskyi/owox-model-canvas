---
title: "Leads"
description: |
  Prospects before they become contacts and accounts: status, source, company, owner and what they converted into.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_lead.htm"
    title: "Salesforce API reference — Leads"
---

## Connector endpoint

- **Endpoint:** Salesforce · `lead`
- **Default destination table:** `salesforce_lead`
- **Unique keys:** `id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Salesforce record id of the lead (18 characters, unique across the org). Primary key. |
| `created_date` | TIMESTAMP | Created Date | When the record was created (UTC). |
| `last_modified_date` | TIMESTAMP | Last Modified Date | Last edit by a user or an integration (UTC). |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change by anyone, including Salesforce itself (UTC). The field the daily window filters on; use it, not LastModifiedDate, to decide what to reload. |
| `is_deleted` | BOOLEAN | Is Deleted | true when the record sits in the Recycle Bin; the connector reads through queryAll so deletions reach the data warehouse instead of silently vanishing. |
| `first_name` | STRING | First Name | First name. |
| `last_name` | STRING | Last Name | Last name. |
| `name` | STRING | Name | Full name. |
| `company` | STRING | Company | Company the lead works for, as entered on the form. |
| `title` | STRING | Title | Job title. |
| `email` | STRING | Email | E-mail address; the join key to form fills, web sessions and marketing-automation contacts. |
| `phone` | STRING | Phone | Phone. |
| `website` | STRING | Website | Company website. |
| `city` | STRING | City | City. |
| `state` | STRING | State | State or province. |
| `postal_code` | STRING | Postal Code | Postal code. |
| `country` | STRING | Country | Country. |
| `status` | STRING | Status | Lead status picklist (Open - Not Contacted, Working - Contacted, Closed - Converted, Closed - Not Converted); your qualification funnel. |
| `lead_source` | STRING | Lead Source | Where the lead came from (Web, Phone Inquiry, Partner Referral, Purchased List, Other); the field marketing attribution lives or dies on. |
| `industry` | STRING | Industry | Industry picklist value. |
| `rating` | STRING | Rating | Hot, Warm, Cold. |
| `annual_revenue` | FLOAT | Annual Revenue | Annual revenue of the company as entered. |
| `number_of_employees` | INTEGER | Number Of Employees | Employee count as entered. |
| `owner_id` | STRING | Owner ID | FK to [Users](./users.md). Lead owner (User id or Queue id). |
| `is_converted` | BOOLEAN | Is Converted | true once the lead was converted into an account, contact and (optionally) opportunity. |
| `converted_date` | DATE | Converted Date | Day of conversion; lead-to-opportunity speed is this minus created_date. |
| `converted_account_id` | STRING | Converted Account ID | FK to [Accounts](./accounts.md). Account created or chosen at conversion. |
| `converted_contact_id` | STRING | Converted Contact ID | FK to [Contacts](./contacts.md). Contact created at conversion. |
| `converted_opportunity_id` | STRING | Converted Opportunity ID | FK to [Opportunities](./opportunities.md). Opportunity created at conversion, if any; the link from a lead to pipeline and revenue. |
| `is_unread_by_owner` | BOOLEAN | Is Unread By Owner | true while the owner has not opened the lead: the speed-to-lead gap. |
| `has_opted_out_of_email` | BOOLEAN | Has Opted Out Of Email | true when the lead opted out of e-mail. |
| `last_activity_date` | DATE | Last Activity Date | Most recent completed task or event logged on the lead. |
| `description` | STRING | Description | Free-text notes. |

# Example Questions

- How many `leads` did each source bring this month, and how many of them converted into deals?
- How long from lead creation to conversion, by owner?
- Which leads are still unread by their owner after 24 hours?

## Joins

- [Users](./users.md) — `owner_id = id` [N:1]
- [Accounts](./accounts.md) — `converted_account_id = id` [N:1] — Set after conversion.
- [Contacts](./contacts.md) — `converted_contact_id = id` [N:1] — Set after conversion.
- [Opportunities](./opportunities.md) — `converted_opportunity_id = id` [N:1] — Set when conversion created a deal; the lead-to-revenue link.
