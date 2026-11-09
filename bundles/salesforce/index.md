---
title: "Salesforce"
description: |
  What the OWOX Data Marts Salesforce connector loads from one Salesforce org, and how the tables fit together.
  Every table carries `id`, the 18-character record id that is unique across the org, and `system_modstamp`, the field the daily window filters on.
  
  [Accounts](./accounts.md) is the anchor: every company the business sells to or supports. [Contacts](./contacts.md) are the people at those accounts,
  [Leads](./leads.md) the prospects before conversion, with the account, contact and deal a converted lead became. [Opportunities](./opportunities.md) are the deals,
  with stage, amount, probability, forecast category and close date; [Opportunity history](./opportunity-history.md) is every change to those five fields,
  which is where stage-to-stage conversion, time in stage and slipped close dates come from, and [Opportunity stages](./opportunity-stages.md) is the sales process
  that orders them. [Opportunity line items](./opportunity-line-items.md) with [Products](./products.md), [Price books](./price-books.md) and
  [Price book entries](./price-book-entries.md) are the product side: revenue by product and discount against list. [Campaigns](./campaigns.md) and
  [Campaign members](./campaign-members.md) are the marketing side: the one primary campaign on a deal, and the many campaigns that touched its contacts.
  [Cases](./cases.md) are support load on the same accounts; [Tasks](./tasks.md) and [Events](./events.md) are the activity log; [Users](./users.md) resolve every owner.
  
  Three flags, three dates: `is_closed` and `is_won` with `close_date` for win rate and cycle length, `stage_name` with `last_stage_change_date` for
  time in stage, `is_converted` with `converted_date` for lead speed. Deleted and merged records arrive with `is_deleted = true` because the connector reads through queryAll.
  
  In the business model Leads and Contacts join [Lead](../marketing-leadgen/lead.md) and [Web Sessions](../marketing-leadgen/web-sessions.md) on e-mail and on the
  UTM values your forms write into lead source; Opportunities join [Opportunities](../marketing-leadgen/opportunities.md), and the won amount joins [Orders](../e-commerce/orders.md)
  and invoiced revenue on the account or the order id kept on the deal; Campaigns join [Ad Spend](../marketing-leadgen/ad-spend.md) and [Unified Ad Spend](../e-commerce/unified-ad-spend.md)
  on campaign name or UTM campaign, so cost per won deal is spend from the ad platforms against won amount from here.
  
  Definitions follow connector version 1 (2026-11-07) on Salesforce REST API v62.0.
tags: ["owox", "index", "connector", "salesforce"]
type: "index"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
    last_modified: 2026-11-07
---

<!-- OWOX:GENERATED:START — regenerated on export, do not edit inside this block -->

| Data Mart | Fields | Description |
|-----------|--------|-------------|
| [Accounts](./accounts.md) | 26 | Companies and organisations in the CRM: customers, prospects, partners, with industry, size, owner and the parent account for hierarchies. |
| [Contacts](./contacts.md) | 26 | People at accounts: name, title, e-mail, phone, lead source and the account they belong to. |
| [Leads](./leads.md) | 33 | Prospects before they become contacts and accounts: status, source, company, owner and what they converted into. |
| [Opportunities](./opportunities.md) | 30 | Deals in the pipeline and closed: amount, stage, probability, forecast category, close date, owner, account and primary campaign. |
| [Opportunity line items](./opportunity-line-items.md) | 17 | Products on deals: product, quantity, unit and total price, and the price-book entry they were priced from. |
| [Opportunity history](./opportunity-history.md) | 14 | One row per change of a deal's stage, amount, probability, forecast category or close date. |
| [Opportunity stages](./opportunity-stages.md) | 14 | The sales process: every stage with its order, default probability, forecast category and closed / won flags. |
| [Campaigns](./campaigns.md) | 28 | Marketing campaigns as sales sees them: type, status, dates, cost and Salesforce's roll-ups of leads, responses, deals and won amount. |
| [Campaign members](./campaign-members.md) | 17 | Which leads and contacts touched which campaign, with status and first response date. |
| [Cases](./cases.md) | 22 | Support cases with status, priority, origin, reason, the contact and account, and when they closed. |
| [Users](./users.md) | 24 | Reps, managers and integration users: name, role, profile, manager, active flag. |
| [Tasks](./tasks.md) | 21 | Logged calls, e-mails and to-dos against leads, contacts, accounts and deals. |
| [Events](./events.md) | 23 | Calendar events (meetings, demos) against leads, contacts, accounts and deals. |
| [Products](./products.md) | 15 | The product catalogue as sales sells it: name, code, family, SKU, active flag. |
| [Price books](./price-books.md) | 10 | Price books: the standard one and any regional or partner books. |
| [Price book entries](./price-book-entries.md) | 13 | A product's list price in a given price book. |

# Example Questions

- What is the open pipeline by stage and by `owner`, how much of it closes this quarter, and what is the win rate and cycle length of the deals that closed?
- How many deals passed each stage, what share moved on, and how many days did they spend there (`opportunity history`)?
- Which `campaigns` touched the contacts of won deals before the close date, and what did those campaigns cost in the ad platforms?

# Explore this model

**[▶ Explore on canvas](https://model.owox.com/?okf=https://raw.githubusercontent.com/ikrasovytskyi/owox-model-canvas/main/bundles/salesforce/index.md)**

One click opens this model in a free OWOX canvas you can poke around in — no account needed.

<!-- OWOX:GENERATED:END -->

## Model preview

![Salesforce model diagram](../res/screens/salesforce.svg)
