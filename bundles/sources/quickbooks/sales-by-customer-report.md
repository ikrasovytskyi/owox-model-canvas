---
title: "Sales by Customer report"
description: |
  Sales per customer for the period, as QuickBooks reports it.
tags: ["owox", "connector", "quickbooks"]
type: "OWOX Data Mart"
sources:
  - id: "vendor-reference"
    resource: "https://developer.intuit.com/app/developer/qbo/docs/api/accounting/report-entities/customersales"
    title: "Intuit QuickBooks Online Accounting API reference — customersales"
---

## Connector endpoint

- **Endpoint:** QuickBooks · `customer_sales`
- **Default destination table:** `quickbooks_report_customer_sales`
- **Unique keys:** `realm_id`, `report_name`, `start_period`, `end_period`, `accounting_method_param`
- **Time series:** yes, loaded by date range

# Schema

| Column | Type | Description |
|--------|------|-------------|
| `realm_id` | STRING | PK. Company (realm) id, stamped from the RealmId parameter. |
| `report_name` | STRING | PK. Name of the QuickBooks report, for example ProfitAndLoss. |
| `report_time` | TIMESTAMP | When QuickBooks produced the snapshot. |
| `report_basis` | STRING | Cash or Accrual (absent on aging reports). |
| `start_period` | DATE | PK. First day of the reporting period. |
| `end_period` | DATE | PK. Last day of the reporting period. |
| `summarize_columns_by` | STRING | How the report columns are split, for example Total, Month or Customers. |
| `currency` | STRING | Currency code the report amounts are in. |
| `header_options` | JSON | Name/Value pairs; NoReportData=true means an empty report. |
| `accounting_method_param` | STRING | PK. AccountingMethod parameter used for the request (ReportBasis echoes it). |
| `report_rows` | JSON | Nested Section/Data rows as JSON. Flatten in SQL; ColData.id is the Account/Customer/Vendor Id. |
| `report_columns` | JSON | Column metadata (ColTitle, ColType) as JSON; index-aligned with every ColData array. |

# Example Questions

- Which customers generated the most sales in the period?
- How concentrated is revenue among the top customers?

## Joins

- [Company info](./company-info.md) — `realm_id = realm_id` [N:1] — The company the report snapshot belongs to. One snapshot row per report period; the figures are inside report_rows.
  - Description: The company the report snapshot belongs to. One snapshot row per report period; the figures are inside report_rows.
