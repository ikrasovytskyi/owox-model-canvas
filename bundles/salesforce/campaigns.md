---
title: "Campaigns"
description: |
  Marketing campaigns as sales sees them: type, status, dates, cost and Salesforce's roll-ups of leads, responses, deals and won amount.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_campaign.htm"
    title: "Salesforce API reference — Campaigns"
---

## Connector endpoint

- **Endpoint:** Salesforce · `campaign`
- **Default destination table:** `salesforce_campaign`
- **Unique keys:** `id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Salesforce record id of the campaign (18 characters, unique across the org). Primary key. |
| `created_date` | TIMESTAMP | Created Date | When the record was created (UTC). |
| `last_modified_date` | TIMESTAMP | Last Modified Date | Last edit by a user or an integration (UTC). |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change by anyone, including Salesforce itself (UTC). The field the daily window filters on; use it, not LastModifiedDate, to decide what to reload. |
| `is_deleted` | BOOLEAN | Is Deleted | true when the record sits in the Recycle Bin; the connector reads through queryAll so deletions reach the data warehouse instead of silently vanishing. |
| `name` | STRING | Name | Campaign name; the join key to ad-platform campaign names or UTM values when they are kept in sync. |
| `parent_id` | STRING | Parent ID | FK to [Campaigns](./campaigns.md). Parent campaign for hierarchies. |
| `campaign_type` | STRING | Campaign Type | Conference, Webinar, Trade Show, Public Relations, Partners, Referral Program, Advertisement, Banner Ads, Direct Mail, Email, Telemarketing, Other. |
| `status` | STRING | Status | Planned, In Progress, Completed, Aborted. |
| `start_date` | DATE | Start Date | Campaign start. |
| `end_date` | DATE | End Date | Campaign end. |
| `is_active` | BOOLEAN | Is Active | false hides the campaign from pickers. |
| `budgeted_cost` | FLOAT | Budgeted Cost | Planned spend entered in Salesforce. |
| `actual_cost` | FLOAT | Actual Cost | Actual spend entered in Salesforce; compare with the ad-platform cost from your Ad Spend tables. |
| `expected_revenue` | FLOAT | Expected Revenue | Revenue the campaign is expected to produce, as entered. |
| `expected_response` | FLOAT | Expected Response | Expected response rate in percent. |
| `number_sent` | FLOAT | Number Sent | Number of people the campaign reached, as entered. |
| `number_of_leads` | INTEGER | Number Of Leads | Roll-up: leads that are campaign members. |
| `number_of_converted_leads` | INTEGER | Number Of Converted Leads | Roll-up: member leads that converted. |
| `number_of_contacts` | INTEGER | Number Of Contacts | Roll-up: contacts that are campaign members. |
| `number_of_responses` | INTEGER | Number Of Responses | Roll-up: members whose status counts as responded. |
| `number_of_opportunities` | INTEGER | Number Of Opportunities | Roll-up: deals with this campaign as primary source. |
| `number_of_won_opportunities` | INTEGER | Number Of Won Opportunities | Roll-up: those deals that were won. |
| `amount_all_opportunities` | FLOAT | Amount All Opportunities | Roll-up: total amount of deals sourced by the campaign. |
| `amount_won_opportunities` | FLOAT | Amount Won Opportunities | Roll-up: won amount of deals sourced by the campaign; the campaign ROI numerator Salesforce itself reports. |
| `owner_id` | STRING | Owner ID | Campaign owner (User id). |
| `last_activity_date` | DATE | Last Activity Date | Most recent activity logged on the campaign. |
| `description` | STRING | Description | Free-text description. |

# Example Questions

- Which `campaigns` sourced the most won amount, and what did they cost in Salesforce vs the ad platforms?
- How does the campaign hierarchy (program → channel) roll up leads, opportunities and revenue?

## Joins

- [Campaigns](./campaigns.md) — `parent_id = id` [N:1] — Campaign hierarchy.
