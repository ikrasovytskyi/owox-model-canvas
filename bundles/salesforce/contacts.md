---
title: "Contacts"
description: |
  People at accounts: name, title, e-mail, phone, lead source and the account they belong to.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_contact.htm"
    title: "Salesforce API reference — Contacts"
---

## Connector endpoint

- **Endpoint:** Salesforce · `contact`
- **Default destination table:** `salesforce_contact`
- **Unique keys:** `id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Salesforce record id of the contact (18 characters, unique across the org). Primary key. |
| `created_date` | TIMESTAMP | Created Date | When the record was created (UTC). |
| `last_modified_date` | TIMESTAMP | Last Modified Date | Last edit by a user or an integration (UTC). |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change by anyone, including Salesforce itself (UTC). The field the daily window filters on; use it, not LastModifiedDate, to decide what to reload. |
| `is_deleted` | BOOLEAN | Is Deleted | true when the record sits in the Recycle Bin; the connector reads through queryAll so deletions reach the data warehouse instead of silently vanishing. |
| `account_id` | STRING | Account ID | FK to [Accounts](./accounts.md). Account the contact belongs to. |
| `first_name` | STRING | First Name | First name. |
| `last_name` | STRING | Last Name | Last name. |
| `name` | STRING | Name | Full name. |
| `title` | STRING | Title | Job title. |
| `department` | STRING | Department | Department. |
| `email` | STRING | Email | E-mail address; the join key to web sessions, e-mail platform and support tickets. |
| `phone` | STRING | Phone | Business phone. |
| `mobile_phone` | STRING | Mobile Phone | Mobile phone. |
| `mailing_city` | STRING | Mailing City | Mailing city. |
| `mailing_state` | STRING | Mailing State | Mailing state or province. |
| `mailing_postal_code` | STRING | Mailing Postal Code | Mailing postal code. |
| `mailing_country` | STRING | Mailing Country | Mailing country. |
| `lead_source` | STRING | Lead Source | Where the contact came from (Web, Phone Inquiry, Partner Referral…). |
| `reports_to_id` | STRING | Reports To ID | FK to [Contacts](./contacts.md). Contact this person reports to, for org charts and buying committees. |
| `owner_id` | STRING | Owner ID | Contact owner (User id). |
| `has_opted_out_of_email` | BOOLEAN | Has Opted Out Of Email | true when the contact opted out of e-mail; respect it in any outreach list you build. |
| `do_not_call` | BOOLEAN | Do Not Call | true when the contact asked not to be called. |
| `email_bounced_date` | TIMESTAMP | Email Bounced Date | When the last e-mail to this address bounced; empty if none. |
| `last_activity_date` | DATE | Last Activity Date | Most recent completed task or event logged on the contact. |
| `description` | STRING | Description | Free-text notes. |

# Example Questions

- How many `contacts` sit on each open deal's account, and which of them responded to a campaign?
- Which contacts share a domain with web sessions on our site this month?

## Joins

- [Accounts](./accounts.md) — `account_id = id` [N:1]
- [Contacts](./contacts.md) — `reports_to_id = id` [N:1] — Reporting line inside the buying committee.
