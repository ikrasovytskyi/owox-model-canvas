---
title: "Accounts"
description: |
  The chart of accounts: every bank, receivable, payable, income, expense and equity account with its type, classification, parent and current balance. The spine every transaction posts to. A sub-account names its parent account in `parent_id`; that self-reference is not drawn as a join.
tags: ["owox", "connector", "quickbooks"]
type: "OWOX Data Mart"
sources:
  - id: "vendor-reference"
    resource: "https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/account"
    title: "Intuit QuickBooks Online Accounting API reference — account"
---

## Connector endpoint

- **Endpoint:** QuickBooks · `account`
- **Default destination table:** `quickbooks_account`
- **Unique keys:** `realm_id`, `id`
- **Time series:** no, one current row per key

# Schema

| Column | Type | Description |
|--------|------|-------------|
| `id` | STRING | PK. QuickBooks entity Id (string). Primary key together with realm_id. |
| `realm_id` | STRING | PK. Company (realm) id, stamped from the RealmId parameter. |
| `sync_token` | STRING | Version counter; changes on every edit. |
| `create_time` | TIMESTAMP | When the record was created in QuickBooks (UTC timestamp). |
| `last_updated_time` | TIMESTAMP | Incremental cursor. |
| `name` | STRING | Name of the account in the chart of accounts. |
| `fully_qualified_name` | STRING | Parent:Child path for sub-accounts. |
| `account_type` | STRING | Bank, Accounts Receivable, Expense, Income, Credit Card… |
| `account_sub_type` | STRING | QuickBooks account sub-type that refines the account type, for example Checking or Savings for bank accounts. |
| `classification` | STRING | Asset, Liability, Equity, Revenue, Expense. |
| `acct_num` | STRING | Account number in the chart of accounts; optional and can be empty. |
| `current_balance` | FLOAT | Current balance of the account in the company's home currency. |
| `current_balance_with_sub_accounts` | FLOAT | Current balance of the account plus all of its sub-accounts, in the home currency. |
| `currency_id` | STRING | Currency code of the document, for example USD; empty when multicurrency is off. |
| `currency_name` | STRING | Currency name that goes with currency_id, for example United States Dollar. |
| `sub_account` | BOOLEAN | True when the account is a sub-account of another account (see parent_id). |
| `parent_id` | STRING | Id of the parent record in the same table (the parent account, customer or item); empty for top-level records. |
| `parent_name` | STRING | Name of the parent record that parent_id points to. |
| `description` | STRING | Description of the account. |
| `active` | BOOLEAN | True when the record is active; false when it has been made inactive (QuickBooks never hard-deletes lists). |

# Example Questions

- How is the chart of accounts structured, and which accounts are sub-accounts of which?
- What is the current balance of every bank and credit-card account, and how much sits in Accounts Receivable and Payable?
- Which accounts are inactive but still hold a balance?

## Joins

- [Company info](./company-info.md) — `realm_id = realm_id` [N:1] — Every table carries the company id.
  - Description: Every table carries the company id.
