---
title: "Campaign members"
description: |
  Which leads and contacts touched which campaign, with status and first response date.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_campaignmember.htm"
    title: "Salesforce API reference — Campaign members"
---

## Connector endpoint

- **Endpoint:** Salesforce · `campaign_member`
- **Default destination table:** `salesforce_campaign_member`
- **Unique keys:** `id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Salesforce record id of the campaign member (18 characters, unique across the org). Primary key. |
| `created_date` | TIMESTAMP | Created Date | When the record was created (UTC). |
| `last_modified_date` | TIMESTAMP | Last Modified Date | Last edit by a user or an integration (UTC). |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change by anyone, including Salesforce itself (UTC). The field the daily window filters on; use it, not LastModifiedDate, to decide what to reload. |
| `is_deleted` | BOOLEAN | Is Deleted | true when the record sits in the Recycle Bin; the connector reads through queryAll so deletions reach the data warehouse instead of silently vanishing. |
| `campaign_id` | STRING | Campaign ID | FK to [Campaigns](./campaigns.md). Campaign. |
| `lead_id` | STRING | Lead ID | FK to [Leads](./leads.md). Lead member, when the member is a lead. |
| `contact_id` | STRING | Contact ID | FK to [Contacts](./contacts.md). Contact member, when the member is a contact. |
| `lead_or_contact_id` | STRING | Lead Or Contact ID | Whichever of the two is set; one join key for touches. |
| `lead_or_contact_owner_id` | STRING | Lead Or Contact Owner ID | Owner of the lead or contact. |
| `member_type` | STRING | Member Type | Lead or Contact. |
| `status` | STRING | Status | Member status (Sent, Responded, or the campaign's own values). |
| `has_responded` | BOOLEAN | Has Responded | true when the status counts as a response. |
| `first_responded_date` | DATE | First Responded Date | First day the member responded; the touch date for influence models. |
| `email` | STRING | Email | E-mail of the lead or contact. |
| `company_or_account` | STRING | Company Or Account | Lead company or contact account name. |
| `lead_source` | STRING | Lead Source | Lead source of the member. |

# Example Questions

- How many distinct campaigns touched each won deal's contacts before the close date (campaign influence)?
- Which campaigns got responses but no opportunities?

## Joins

- [Campaigns](./campaigns.md) — `campaign_id = id` [N:1]
- [Leads](./leads.md) — `lead_id = id` [N:1] — When the member is a lead.
- [Contacts](./contacts.md) — `contact_id = id` [N:1] — When the member is a contact.
