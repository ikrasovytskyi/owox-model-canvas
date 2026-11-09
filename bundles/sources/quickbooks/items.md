---
title: "Items"
description: |
  Products and services sold or bought, with price, cost, quantity on hand and the income and expense accounts each one posts to. Joining items to invoice lines turns revenue into margin. A sub-item names its parent item in `parent_id`; that self-reference is not drawn as a join.
tags: ["owox", "connector", "quickbooks"]
type: "OWOX Data Mart"
sources:
  - id: "vendor-reference"
    resource: "https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/item"
    title: "Intuit QuickBooks Online Accounting API reference — item"
---

## Connector endpoint

- **Endpoint:** QuickBooks · `item`
- **Default destination table:** `quickbooks_item`
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
| `name` | STRING | Name of the product or service as shown on sales forms. |
| `fully_qualified_name` | STRING | Full path of the item including its parents, separated by colons (Parent:Child). |
| `sku` | STRING | Stock keeping unit code of the item. |
| `type` | STRING | Inventory, NonInventory, Service, Category, Group… |
| `description` | STRING | Description of the item shown on sales forms. |
| `unit_price` | FLOAT | Default selling price per unit, in the home currency. |
| `purchase_cost` | FLOAT | Default cost per unit when buying the item, in the home currency. |
| `qty_on_hand` | FLOAT | Current stock quantity of an inventory item. |
| `taxable` | BOOLEAN | True when the record is subject to sales tax. |
| `track_qty_on_hand` | BOOLEAN | True when the item is an inventory item with tracked stock quantity. |
| `income_account_id` | STRING | Id of the income account that sales of the item are posted to; joins to account.id. |
| `income_account_name` | STRING | Name of the account that income_account_id points to. |
| `expense_account_id` | STRING | Id of the expense or cost of goods sold account that purchases of the item are posted to; joins to account.id. |
| `expense_account_name` | STRING | Name of the account that expense_account_id points to. |
| `asset_account_id` | STRING | Id of the inventory asset account that holds the item's stock value; joins to account.id. |
| `asset_account_name` | STRING | Name of the account that asset_account_id points to. |
| `sub_item` | BOOLEAN | True when the item is a sub-item of another item (see parent_id). |
| `parent_id` | STRING | Id of the parent record in the same table (the parent account, customer or item); empty for top-level records. |
| `parent_name` | STRING | Name of the parent record that parent_id points to. |
| `active` | BOOLEAN | True when the record is active; false when it has been made inactive (QuickBooks never hard-deletes lists). |

# Example Questions

- Which products and services sell most by quantity and by revenue?
- What is the margin per item once its income and expense accounts are joined?
- Which items that track quantity on hand have zero or negative `qty_on_hand`?

## Joins

- [Company info](./company-info.md) — `realm_id = realm_id` [N:1] — The company the row belongs to.
  - Description: The company the row belongs to.
- [Accounts](./accounts.md) — `realm_id = realm_id`, `income_account_id = id` [N:1] — The account where sales of the item are booked. `expense_account_id` holds the account where purchases of the item are booked (cost of goods for inventory), joined to Accounts the same way.
  - Description: The account where sales of the item are booked. `expense_account_id` holds the account where purchases of the item are booked (cost of goods for inventory), joined to Accounts the same way.
