---
title: "Users"
description: |
  Reps, managers and integration users: name, role, profile, manager, active flag.
tags: ["owox", "connector", "salesforce"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm"
    title: "OWOX Data Marts — Salesforce custom connector (Salesforce, 16 nodes) on the Salesforce REST API v62.0, SOQL through queryAll"
  - id: "vendor-reference"
    resource: "https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_user.htm"
    title: "Salesforce API reference — Users"
---

## Connector endpoint

- **Endpoint:** Salesforce · `user`
- **Default destination table:** `salesforce_user`
- **Unique keys:** `id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `id` | STRING | ID | PK. User id. Primary key; the target of every owner_id and created_by_id. |
| `username` | STRING | Username | Login name (unique across all Salesforce orgs). |
| `name` | STRING | Name | Full name. |
| `first_name` | STRING | First Name | First name. |
| `last_name` | STRING | Last Name | Last name. |
| `email` | STRING | Email | E-mail. |
| `alias` | STRING | Alias | Short alias shown in list views. |
| `is_active` | BOOLEAN | Is Active | false for deactivated users whose records still carry their id. |
| `title` | STRING | Title | Job title. |
| `department` | STRING | Department | Department. |
| `division` | STRING | Division | Division. |
| `company_name` | STRING | Company Name | Company name on the user record. |
| `manager_id` | STRING | Manager ID | FK to [Users](./users.md). Manager (User id); build team roll-ups by walking it. |
| `user_role_id` | STRING | User Role ID | Role id in the role hierarchy. |
| `profile_id` | STRING | Profile ID | Profile id (permissions). |
| `user_type` | STRING | User Type | Standard, PowerPartner, CspLitePortal and other licence types; filter to Standard for rep reporting. |
| `time_zone_sid_key` | STRING | Time Zone Sid Key | User time zone, e.g. America/Los_Angeles. |
| `country` | STRING | Country | Country on the user record. |
| `state` | STRING | State | State on the user record. |
| `city` | STRING | City | City on the user record. |
| `last_login_date` | TIMESTAMP | Last Login Date | Last login (UTC); licence hygiene. |
| `created_date` | TIMESTAMP | Created Date | When the user was created. |
| `last_modified_date` | TIMESTAMP | Last Modified Date | Last edit of the user record. |
| `system_modstamp` | TIMESTAMP | System Modstamp | Last change by anyone. |

# Example Questions

- Pipeline, won amount and activities per rep and per manager's team.
- Which users are inactive but still own open deals?

## Joins

- [Users](./users.md) — `manager_id = id` [N:1] — Manager; walk it for team roll-ups.
