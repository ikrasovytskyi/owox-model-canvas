---
title: "Bill payments"
description: |
  Payments made against bills, by check or credit card, with the vendor and the accounts involved.
tags: ["owox", "connector", "quickbooks"]
type: "OWOX Data Mart"
sources:
  - id: "vendor-reference"
    resource: "https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/billpayment"
    title: "Intuit QuickBooks Online Accounting API reference — billpayment"
---

## Connector endpoint

- **Endpoint:** QuickBooks · `bill_payment`
- **Default destination table:** `quickbooks_bill_payment`
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
| `doc_number` | STRING | Document number shown on the form (for example the invoice or bill number); can be empty when the company does not use automatic numbering. |
| `txn_date` | DATE | Business date of the transaction (not the edit date). |
| `total_amt` | FLOAT | Total amount of the document including tax, in the document currency. |
| `currency_id` | STRING | Currency code of the document, for example USD; empty when multicurrency is off. |
| `currency_name` | STRING | Currency name that goes with currency_id, for example United States Dollar. |
| `exchange_rate` | FLOAT | Rate that converts the document currency to the company's home currency; 1 or empty when the document is in the home currency. |
| `private_note` | STRING | Internal memo entered on the document; not shown to the customer or vendor. |
| `pay_type` | STRING | Check or CreditCard. |
| `vendor_id` | STRING | Id of the vendor on the document; joins to vendor.id. |
| `vendor_name` | STRING | Name of the vendor that vendor_id points to. |
| `ap_account_id` | STRING | Id of the accounts payable account the document is posted to; joins to account.id. |
| `ap_account_name` | STRING | Name of the account that ap_account_id points to. |
| `check_bank_account_id` | STRING | Id of the bank account a check payment is drawn from; joins to account.id. |
| `check_bank_account_name` | STRING | Name of the bank account that check_bank_account_id points to. |
| `check_print_status` | STRING | Print state of the check: NotSet, NeedToPrint or PrintComplete. |
| `credit_card_account_id` | STRING | Id of the credit card account the payment was made with; joins to account.id. |
| `credit_card_account_name` | STRING | Name of the account that credit_card_account_id points to. |
| `line` | JSON | Line\[\] as JSON; LinkedTxn\[\] {TxnId, TxnType=Bill} names the bills settled. |

# Example Questions

- How much was paid to vendors per month, by check and by card?
- Which vendors received the largest payments, and from which accounts?

## Joins

- [Vendors](./vendors.md) — `realm_id = realm_id`, `vendor_id = id` [N:1] — The vendor the document is for.
  - Description: The vendor the document is for.
- [Accounts](./accounts.md) — `realm_id = realm_id`, `ap_account_id = id` [N:1] — The Accounts Payable account the payment is booked against. `check_bank_account_id` (the bank account of a check payment) and `credit_card_account_id` (the card account of a card payment) also hold account ids, each joined to Accounts the same way.
  - Description: The Accounts Payable account the payment is booked against. `check_bank_account_id` (the bank account of a check payment) and `credit_card_account_id` (the card account of a card payment) also hold account ids, each joined to Accounts the same way.
- [Company info](./company-info.md) — `realm_id = realm_id` [N:1] — The company the row belongs to.
  - Description: The company the row belongs to.
