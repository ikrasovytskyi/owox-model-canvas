---
title: "HubSpot"
description: |
  What the OWOX Data Marts HubSpot connector loads from one HubSpot account, and how the tables fit together.
  Every table carries `id`, HubSpot's object id (unique per object type in the account), and `portal_id`, your HubSpot account, so several accounts can share one table.
  
  [Deals](./deals.md) is the spine of the sales side: amount, pipeline, current stage, close date and owner, with the stage's label, order and win probability in
  [Deal pipeline stages](./deal-pipeline-stages.md) and the rep in [Owners](./owners.md). Pipeline by stage is open deals grouped by `dealstage`; bookings are closed-won deals by `closedate`;
  win rate is closed won over closed; sales cycle is `hs_days_to_close`. [Line items](./line-items.md) with [Products](./products.md) break a deal into what was sold.
  [Contacts](./contacts.md) carry the marketing funnel: original source and the dated lifecycle milestones (lead, MQL, SQL, opportunity, customer). [Companies](./companies.md) are the accounts deals are won or lost at.
  HubSpot keeps the links between objects apart from the objects, so the three association tables ([Deal associations](./deal-associations.md), [Contact associations](./contact-associations.md),
  [Ticket associations](./ticket-associations.md)) carry the primary contact and company of each record plus every link as a JSON array. The five activity tables ([Calls](./calls.md), [Emails](./emails.md), [Meetings](./meetings.md),
  [Notes](./notes.md), [Tasks](./tasks.md)) are the touches, each with its rep and the records it was logged on. [Tickets](./tickets.md) is the service side, [Marketing emails](./marketing-emails.md) the bulk-send statistics.
  
  Three dates: `createdate` for when a deal or contact entered the funnel, `closedate` for when revenue was won, `hs_lastmodifieddate` for the incremental load. Amounts are in the deal currency; `amount_in_home_currency` is the account's base currency.
  
  In the business model Contacts are the [Lead](../marketing-leadgen/lead.md) and Companies the [Account](../marketing-leadgen/account.md) of the Marketing Leadgen model; Deals are its [Opportunities](../marketing-leadgen/opportunities.md),
  the activity tables its [Touchpoints](../marketing-leadgen/touchpoints.md), and `hs_analytics_source` on contacts and deals is the join to [Web Sessions](../marketing-leadgen/web-sessions.md) and [Ad Spend](../marketing-leadgen/ad-spend.md) by channel and campaign.
  For an online store Contacts join [Customers](../e-commerce/customers.md) on e-mail and closed-won Deals sit next to [Orders](../e-commerce/orders.md) by date; for a subscription business Companies join the SaaS [Account](../saas/account.md) on domain.
  
  Definitions follow connector version 1 (2026-11-07) on the HubSpot CRM v3 API.
tags: ["owox", "index", "connector", "hubspot"]
type: "index"
sources:
  - id: "owox-connector"
    resource: "https://developers.hubspot.com/docs/api/crm/understanding-the-crm"
    title: "OWOX Data Marts — HubSpot custom connector (HubSpot, 20 nodes) on the HubSpot CRM v3 API"
    last_modified: 2026-11-07
---

<!-- OWOX:GENERATED:START — regenerated on export, do not edit inside this block -->

| Data Mart | Fields | Description |
|-----------|--------|-------------|
| [Contacts](./contacts.md) | 47 | Every person in the CRM with identity, lifecycle stage, the dated funnel milestones (lead, MQL, SQL, opportunity, customer), original and latest traffic source, form conversions and owner. |
| [Companies](./companies.md) | 36 | Every account with firmographics, lifecycle stage, owner, rolled-up deal facts and original source. |
| [Deals](./deals.md) | 47 | Every deal with amount, pipeline, current stage, probability, close date, owner, type, forecast category, won and lost reasons and original source. |
| [Tickets](./tickets.md) | 21 | Every support ticket with pipeline, status, priority, category, channel, owner and HubSpot's time-to-close and time-to-first-reply. |
| [Owners](./owners.md) | 11 | The users who own records: name, e-mail and teams. |
| [Deal pipelines](./deal-pipelines.md) | 8 | The deal pipelines of the account: id, label and order. |
| [Deal pipeline stages](./deal-pipeline-stages.md) | 10 | Every stage of every deal pipeline with label, order, win probability and the closed flag. |
| [Ticket pipelines](./ticket-pipelines.md) | 8 | The ticket pipelines of the account. |
| [Ticket pipeline stages](./ticket-pipeline-stages.md) | 10 | Every ticket status with label, order and OPEN/CLOSED state. |
| [Products](./products.md) | 16 | The product library: price, SKU, cost and billing period of every product. |
| [Line items](./line-items.md) | 26 | Every line item on every deal: product, quantity, price, discount, amount and recurring values, with the deal it belongs to. |
| [Contact associations](./contact-associations.md) | 5 | One row per contact with its primary company and every associated company as JSON. |
| [Deal associations](./deal-associations.md) | 8 | One row per deal with its primary contact and company and every associated contact, company and line item as JSON. |
| [Ticket associations](./ticket-associations.md) | 9 | One row per ticket with its primary contact, company and deal and every link as JSON. |
| [Calls](./calls.md) | 22 | Every logged call with direction, duration, outcome and rep, plus the contact, deal and company it was logged on. |
| [Emails](./emails.md) | 21 | Every sales e-mail logged or sent from HubSpot with subject, direction, status and rep, plus its associations. |
| [Meetings](./meetings.md) | 22 | Every meeting with start, end, outcome and booking source, plus rep and associations. |
| [Notes](./notes.md) | 17 | Every note a rep left on a record, with rep and associations. |
| [Tasks](./tasks.md) | 22 | Every task with status, priority, type and completion date, plus rep and associations. |
| [Marketing emails](./marketing-emails.md) | 27 | Every marketing (bulk) e-mail with state, send date and lifetime statistics: sent, delivered, opens, clicks, bounces, unsubscribes and the ratios. |

# Example Questions

- What is the open pipeline by `stage` today, in count and amount, and how much of it is weighted by the stage's win probability?
- What is the win rate and sales cycle by `owner`, deal type and original source, and how many `calls`, `emails` and `meetings` did a won deal take?
- Which original source and which form brought the `contacts` that became customers, and what did each source cost in ad spend?

# Explore this model

**[▶ Explore on canvas](https://model.owox.com/?okf=https://raw.githubusercontent.com/ikrasovytskyi/owox-model-canvas/main/bundles/hubspot/index.md)**

One click opens this model in a free OWOX canvas you can poke around in — no account needed.

<!-- OWOX:GENERATED:END -->
