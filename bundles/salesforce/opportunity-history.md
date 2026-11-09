---
title: "Opportunity history"
description: |
  One row per change of a deal's stage, amount, probability, forecast category or close date.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_opportunityhistory.htm"
    title: "Salesforce API reference — Opportunity history"
---

## Connector endpoint

- **Endpoint:** Salesforce · `opportunity_history`
- **Default destination table:** `salesforce_opportunity_history`
- **Unique keys:** `id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. History row id. Primary key. |
| `opportunity_id` | STRING | Opportunity ID | FK to [Opportunities](./opportunities.md). Deal the change belongs to. |
| `created_date` | TIMESTAMP | Created Date | When the change happened (UTC); order rows by this to rebuild the stage path. |
| `created_by_id` | STRING | Created By ID | FK to [Users](./users.md). User who made the change. |
| `stage_name` | STRING | Stage Name | FK to [Opportunity stages](./opportunity-stages.md). Stage after the change. |
| `amount` | FLOAT | Amount | Amount after the change. |
| `prev_amount` | FLOAT | Prev Amount | Amount before the change; the delta is the deal growing or shrinking. |
| `expected_revenue` | FLOAT | Expected Revenue | Expected revenue after the change. |
| `probability` | FLOAT | Probability | Probability after the change. |
| `close_date` | DATE | Close Date | Close date after the change. |
| `prev_close_date` | DATE | Prev Close Date | Close date before the change; later than prev = a slipped deal. |
| `forecast_category` | STRING | Forecast Category | Forecast category after the change. |
| `is_deleted` | BOOLEAN | Is Deleted | true when the parent deal was deleted. |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change to the history row (UTC); the daily window filters on it. |

# Example Questions

- How many deals passed each stage and what share moved to the next one (stage-to-stage conversion)?
- How many days do deals spend in each stage before they are won or lost?
- Which deals slipped their close date, and how often, in the last quarter?

## Joins

- [Opportunities](./opportunities.md) — `opportunity_id = id` [N:1] — Order by created_date to rebuild the stage path.
- [Opportunity stages](./opportunity-stages.md) — `stage_name = master_label` [N:1]
- [Users](./users.md) — `created_by_id = id` [N:1] — Who moved the deal.
