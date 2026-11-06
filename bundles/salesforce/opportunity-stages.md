---
title: "Opportunity stages"
description: |
  The sales process: every stage with its order, default probability, forecast category and closed / won flags.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_opportunitystage.htm"
    title: "Salesforce API reference — Opportunity stages"
---

## Connector endpoint

- **Endpoint:** Salesforce · `opportunity_stage`
- **Default destination table:** `salesforce_opportunity_stage`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Stage definition id. Primary key. |
| `master_label` | STRING | Master Label | Stage name as it appears on deals (matches opportunity.stage_name). |
| `api_name` | STRING | Api Name | API name of the stage. |
| `sort_order` | INTEGER | Sort Order | Position of the stage in the sales process; sort funnels by it. |
| `is_active` | BOOLEAN | Is Active | false for retired stages that old deals may still reference. |
| `is_closed` | BOOLEAN | Is Closed | true for Closed Won and Closed Lost. |
| `is_won` | BOOLEAN | Is Won | true for Closed Won. |
| `default_probability` | FLOAT | Default Probability | Default win probability in percent that new deals in this stage receive. |
| `forecast_category` | STRING | Forecast Category | Forecast bucket API value for the stage. |
| `forecast_category_name` | STRING | Forecast Category Name | Forecast bucket label (Pipeline, Best Case, Commit, Omitted, Closed). |
| `description` | STRING | Description | Stage description. |
| `created_date` | TIMESTAMP | Created Date | When the stage was defined. |
| `last_modified_date` | TIMESTAMP | Last Modified Date | Last edit of the stage definition. |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change by anyone. |

# Example Questions

- What is the weighted pipeline using each stage's default probability instead of the rep's own?
- Which stages count as Commit in the forecast?
