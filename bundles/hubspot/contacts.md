---
title: "Contacts"
description: |
  Every person in the CRM with identity, lifecycle stage, the dated funnel milestones (lead, MQL, SQL, opportunity, customer), original and latest traffic source, form conversions and owner. The marketing funnel is counted on this table; the e-mail is the join to billing, newsletter and ad-platform data.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/crm/contacts"
    title: "HubSpot API reference — Contacts"
---

## Connector endpoint

- **Endpoint:** HubSpot · `contacts`
- **Default destination table:** `hubspot_contacts`
- **Unique keys:** `id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. FK to [Contact associations](./contact-associations.md). HubSpot object id of the contact (hs_object_id), unique in the account. Primary key. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector; lets several HubSpot accounts share one table. |
| `email` | STRING | Email | Primary e-mail address. The join key to Stripe or QuickBooks customers, newsletter contacts and ad-platform conversions by e-mail. |
| `firstname` | STRING | Firstname | First name. |
| `lastname` | STRING | Lastname | Last name. |
| `phone` | STRING | Phone | Phone number as typed in HubSpot. |
| `company` | STRING | Company | Company name typed on the contact; the associated company record is in hubspot_contact_associations. |
| `jobtitle` | STRING | Jobtitle | Job title. |
| `lifecyclestage` | STRING | Lifecyclestage | Lifecycle stage id: subscriber, lead, marketingqualifiedlead, salesqualifiedlead, opportunity, customer, evangelist, other, or a custom stage id. |
| `hs_lead_status` | STRING | Hs Lead Status | Lead status set by sales (NEW, OPEN, IN_PROGRESS, OPEN_DEAL, UNQUALIFIED, ATTEMPTED_TO_CONTACT, CONNECTED, BAD_TIMING, or custom). |
| `hubspot_owner_id` | STRING | Hubspot Owner ID | FK to [Owners](./owners.md). Owner (sales rep) of the contact; joins hubspot_owners.id. |
| `hs_analytics_source` | STRING | Hs Analytics Source | Original traffic source of the contact's first visit: ORGANIC_SEARCH, PAID_SEARCH, EMAIL_MARKETING, SOCIAL_MEDIA, REFERRALS, OTHER_CAMPAIGNS, DIRECT_TRAFFIC, OFFLINE, PAID_SOCIAL. |
| `hs_analytics_source_data_1` | STRING | Hs Analytics Source Data 1 | Original source drill-down 1 (for example the search engine, the campaign or the import name). |
| `hs_analytics_source_data_2` | STRING | Hs Analytics Source Data 2 | Original source drill-down 2 (for example the keyword or the referring domain). |
| `hs_latest_source` | STRING | Hs Latest Source | Latest traffic source, same vocabulary as hs_analytics_source. |
| `hs_latest_source_timestamp` | TIMESTAMP | Hs Latest Source Timestamp | When the latest source was recorded. |
| `createdate` | TIMESTAMP | Createdate | When the contact was created in HubSpot. The date a lead enters the funnel. |
| `lastmodifieddate` | TIMESTAMP | Lastmodifieddate | When any contact property last changed. The incremental filter for this table. |
| `hs_lifecyclestage_lead_date` | TIMESTAMP | Hs Lifecyclestage Lead Date | When the contact became a lead. |
| `hs_lifecyclestage_marketingqualifiedlead_date` | TIMESTAMP | Hs Lifecyclestage Marketingqualifiedlead Date | When the contact became a marketing-qualified lead (MQL). |
| `hs_lifecyclestage_salesqualifiedlead_date` | TIMESTAMP | Hs Lifecyclestage Salesqualifiedlead Date | When the contact became a sales-qualified lead (SQL). |
| `hs_lifecyclestage_opportunity_date` | TIMESTAMP | Hs Lifecyclestage Opportunity Date | When the contact became an opportunity. |
| `hs_lifecyclestage_customer_date` | TIMESTAMP | Hs Lifecyclestage Customer Date | When the contact became a customer. MQL→SQL→customer conversion is these dates divided. |
| `closedate` | TIMESTAMP | Closedate | Close date copied from the first deal that closed won for this contact. |
| `first_conversion_event_name` | STRING | First Conversion Event Name | Name of the first form the contact submitted. |
| `first_conversion_date` | TIMESTAMP | First Conversion Date | When the contact first converted on a form. |
| `recent_conversion_event_name` | STRING | Recent Conversion Event Name | Name of the most recent form submission. |
| `recent_conversion_date` | TIMESTAMP | Recent Conversion Date | When the most recent form submission happened. |
| `num_conversion_events` | INTEGER | Num Conversion Events | Number of form submissions. |
| `hs_analytics_num_page_views` | INTEGER | Hs Analytics Num Page Views | Page views HubSpot tracked for this contact. |
| `hs_analytics_num_visits` | INTEGER | Hs Analytics Num Visits | Sessions (visits) HubSpot tracked for this contact. |
| `hs_analytics_first_visit_timestamp` | TIMESTAMP | Hs Analytics First Visit Timestamp | First tracked visit. |
| `hs_analytics_last_visit_timestamp` | TIMESTAMP | Hs Analytics Last Visit Timestamp | Most recent tracked visit. |
| `num_associated_deals` | INTEGER | Num Associated Deals | Number of deals associated with the contact. |
| `num_notes` | INTEGER | Num Notes | Number of sales activities (notes, calls, e-mails, meetings, tasks) logged on the contact. |
| `notes_last_contacted` | TIMESTAMP | Notes Last Contacted | Last time a rep contacted this person (call, e-mail or meeting). |
| `hs_sales_email_last_replied` | TIMESTAMP | Hs Sales Email Last Replied | Last time the contact replied to a sales e-mail. |
| `city` | STRING | City | City. |
| `state` | STRING | State | State or region. |
| `country` | STRING | Country | Country. |
| `website` | STRING | Website | Website URL typed on the contact. |
| `hs_email_domain` | STRING | Hs Email Domain | Domain part of the e-mail; joins companies.domain when no association exists. |
| `hs_object_source` | STRING | Hs Object Source | How the record was created: FORM, IMPORT, API, CRM_UI, INTEGRATION, WORKFLOWS, CONVERSATIONS, EMAIL and others. |
| `hs_object_source_label` | STRING | Hs Object Source Label | Human label of hs_object_source. |
| `created_at` | TIMESTAMP | Created At | When HubSpot created the record (UTC). |
| `updated_at` | TIMESTAMP | Updated At | When any property of the record last changed (UTC). The field the daily window filters on. |
| `archived` | BOOLEAN | Archived | true when the record was deleted (archived) in HubSpot; archived records stop appearing in loads. |

# Example Questions

- How many leads, MQLs, SQLs and customers did we add each month, and what is the MQL→SQL→customer conversion by original source?
- Which form brought the contacts that became customers?
- How many contacts has each rep touched in the last 30 days (notes_last_contacted)?

## Joins

- [Owners](./owners.md) — `hubspot_owner_id = id` [N:1] — The rep who owns the contact.
- [Contact associations](./contact-associations.md) — `id = id` [1:1] — The contact's companies.
