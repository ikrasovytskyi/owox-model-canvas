---
title: "Marketing emails"
description: |
  Every marketing (bulk) e-mail with state, send date and lifetime statistics: sent, delivered, opens, clicks, bounces, unsubscribes and the ratios.
tags: ["owox", "connector", "hubspot"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
  - id: "vendor-reference"
    resource: "https://developers.hubspot.com/docs/api/marketing/emails/marketing-emails"
    title: "HubSpot API reference — Marketing emails"
---

## Connector endpoint

- **Endpoint:** HubSpot · `marketing_emails`
- **Default destination table:** `hubspot_marketing_emails`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Marketing e-mail id. Primary key. |
| `portal_id` | STRING | Portal ID | Your HubSpot account (portal) id as entered in the connector. |
| `name` | STRING | Name | Internal e-mail name. |
| `subject` | STRING | Subject | Subject line. |
| `state` | STRING | State | DRAFT, PUBLISHED, SCHEDULED, AUTOMATED, AUTOMATED_DRAFT and others. |
| `email_type` | STRING | Email Type | BATCH_EMAIL, AUTOMATED_EMAIL, AB_EMAIL, RSS_EMAIL and others. |
| `publish_date` | TIMESTAMP | Publish Date | When the e-mail was sent or scheduled. |
| `is_published` | BOOLEAN | Is Published | true once sent or live. |
| `campaign_id` | STRING | Campaign ID | Marketing campaign the e-mail belongs to. |
| `campaign_name` | STRING | Campaign Name | Campaign name. |
| `from_name` | STRING | From Name | Sender name. |
| `sent` | INTEGER | Sent | Recipients the send went to. |
| `delivered` | INTEGER | Delivered | Delivered recipients. |
| `opens` | INTEGER | Opens | Total opens. |
| `clicks` | INTEGER | Clicks | Total clicks. |
| `bounces` | INTEGER | Bounces | Bounced recipients. |
| `unsubscribes` | INTEGER | Unsubscribes | Unsubscribed recipients. |
| `spam_reports` | INTEGER | Spam Reports | Spam reports. |
| `replies` | INTEGER | Replies | Replies. |
| `open_rate` | FLOAT | Open Rate | Opens ÷ delivered, as HubSpot reports it. |
| `click_rate` | FLOAT | Click Rate | Clicks ÷ delivered. |
| `click_through_rate` | FLOAT | Click Through Rate | Clicks ÷ opens (CTOR). |
| `bounce_rate` | FLOAT | Bounce Rate | Bounces ÷ sent. |
| `unsubscribe_rate` | FLOAT | Unsubscribe Rate | Unsubscribes ÷ delivered. |
| `created_at` | TIMESTAMP | Created At | When the e-mail was created. |
| `updated_at` | TIMESTAMP | Updated At | When the e-mail last changed. |
| `archived` | BOOLEAN | Archived | true when archived. |

# Example Questions

- Which marketing e-mails had the best open and click rates this quarter?
- How did sends, opens and unsubscribes move month over month?
