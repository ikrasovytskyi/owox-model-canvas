---
title: "Price books"
description: |
  Price books: the standard one and any regional or partner books.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_pricebook2.htm"
    title: "Salesforce API reference — Price books"
---

## Connector endpoint

- **Endpoint:** Salesforce · `pricebook`
- **Default destination table:** `salesforce_pricebook`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. Price book id. Primary key. |
| `name` | STRING | Name | Price book name. |
| `is_standard` | BOOLEAN | Is Standard | true for the one Standard Price Book that holds list prices. |
| `is_active` | BOOLEAN | Is Active | false for inactive books. |
| `is_archived` | BOOLEAN | Is Archived | true for archived books. |
| `is_deleted` | BOOLEAN | Is Deleted | true when in the Recycle Bin. |
| `description` | STRING | Description | Description. |
| `created_date` | TIMESTAMP | Created Date | When the book was created. |
| `last_modified_date` | TIMESTAMP | Last Modified Date | Last edit. |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change by anyone. |

# Example Questions

- Which price books are in use on open deals?
