---
title: "Transactions"
description: |
  Every balance-affecting movement on the PayPal account: payments received, fees, refunds, chargebacks, holds, currency conversions and withdrawals, one row each, classified by `transaction_event_code`. Gross is `transaction_amount`, PayPal's cut is `fee_amount`, their sum is net, and `ending_balance` is PayPal's running balance after the row, so the table reconciles to itself.
tags: ["owox", "connector", "paypal"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.paypal.com/docs/api/transaction-search/v1/"
    title: "OWOX Data Marts — PayPal custom connector (PayPal, 7 nodes) on the PayPal REST APIs: Transaction Search v1, Disputes v1, Invoicing v2, Subscriptions v1, Catalog Products v1"
  - id: "vendor-reference"
    resource: "https://developer.paypal.com/docs/api/transaction-search/v1/#transactions_get"
    title: "PayPal API reference — Transactions"
---

## Connector endpoint

- **Endpoint:** PayPal · `transactions`
- **Default destination table:** `paypal_transactions`
- **Unique keys:** `transaction_id`, `transaction_event_code`, `transaction_currency`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `transaction_id` | STRING | Transaction ID | PK. FK to [Dispute details](./dispute-details.md). PayPal transaction id, 17 characters. Not unique on its own in the reporting system (a currency conversion writes two rows with one id), so the table key is transaction_id + transaction_event_code + transaction_currency. |
| `account_id` | STRING | Account ID | Your PayPal merchant account id as entered in the connector (Account settings → Business information); lets several PayPal accounts share one table. Empty when not set. |
| `transaction_event_code` | STRING | Transaction Event Code | PK. Five-character event code that classifies the money movement: T00xx payments received (T0006 Express Checkout, T0007 website payment, T0002 subscription payment, T0001 mass payment), T01xx fees, T02xx currency conversions, T03xx bank deposits, T04xx withdrawals to the bank, T11xx refunds and reversals (T1107 refund), T12xx adjustments and chargebacks (T1201 chargeback), T15xx and T21xx holds and releases. |
| `transaction_status` | STRING | Transaction Status | S completed, P pending, D denied, V reversed, F partially refunded. |
| `transaction_initiation_date` | TIMESTAMP | Transaction Initiation Date | When the transaction was initiated (UTC). The business date: bucket revenue by this field. |
| `transaction_updated_date` | TIMESTAMP | Transaction Updated Date | When PayPal last updated the transaction, for example when a pending payment completed. A row whose status changes reappears in a later window and is updated in place by its key. |
| `transaction_amount` | FLOAT | Transaction Amount | Gross amount as a decimal in the row's currency (PayPal returns amounts as decimal strings); positive for money in, negative for money out (refunds, withdrawals, chargebacks). |
| `transaction_currency` | STRING | Transaction Currency | PK. FK to [Balances](./balances.md). Three-letter ISO currency code of transaction_amount, fee_amount and the balances on this row. |
| `fee_amount` | FLOAT | Fee Amount | PayPal fee on this row as a decimal in the row's currency (PayPal returns amounts as decimal strings); negative when PayPal took a fee, positive when a fee was returned with a refund. Net = transaction_amount + fee_amount. |
| `fee_currency` | STRING | Fee Currency | Currency of fee_amount. |
| `ending_balance` | FLOAT | Ending Balance | The PayPal balance in this currency after this row. Reconciliation check: the running sum of transaction_amount + fee_amount ordered by transaction_initiation_date equals this value. |
| `available_balance` | FLOAT | Available Balance | The balance available for withdrawal after this row; differs from ending_balance by the amounts on hold. |
| `paypal_reference_id` | STRING | Paypal Reference ID | FK to [Billing plans](./billing-plans.md). Id of the related PayPal object: the order (ODR), the original transaction (TXN, for a refund), the subscription (SUB) or the pre-approved payment (PAP). For a T0002 row this is the subscription id, which is how recurring revenue is grouped. |
| `paypal_reference_id_type` | STRING | Paypal Reference ID Type | ODR, TXN, SUB or PAP: what paypal_reference_id points at. |
| `paypal_account_id` | STRING | Paypal Account ID | PayPal account id of the counterparty (the payer for money in, the recipient for money out). |
| `invoice_id` | STRING | Invoice ID | The invoice or order id the merchant sent with the payment (the store's order number for most checkouts). The join key to orders. |
| `custom_field` | STRING | Custom Field | Merchant-provided custom text passed with the payment; stores often put the cart or customer id here. |
| `transaction_subject` | STRING | Transaction Subject | Subject of the payment as the payer saw it. |
| `transaction_note` | STRING | Transaction Note | Note the payer attached to the payment. |
| `protection_eligibility` | STRING | Protection Eligibility | Seller protection: 01 eligible, 02 not eligible, 03 partially eligible. |
| `instrument_type` | STRING | Instrument Type | Funding instrument family: PayPal, credit card, debit card, bank, Pay Later. |
| `instrument_sub_type` | STRING | Instrument Sub Type | Finer instrument: PayPal wallet, Visa, American Express, ... |
| `payment_method_type` | STRING | Payment Method Type | PUI (pay upon invoice), installment or mEFT when one of those PayPal methods was used; empty otherwise. |
| `sales_tax_amount` | FLOAT | Sales Tax Amount | Sales tax included in the gross as a decimal in the row's currency (PayPal returns amounts as decimal strings). |
| `shipping_amount` | FLOAT | Shipping Amount | Shipping included in the gross as a decimal in the row's currency (PayPal returns amounts as decimal strings). |
| `discount_amount` | FLOAT | Discount Amount | Discount applied as a decimal in the row's currency (PayPal returns amounts as decimal strings). |
| `tip_amount` | FLOAT | Tip Amount | Tip included as a decimal in the row's currency (PayPal returns amounts as decimal strings). |
| `insurance_amount` | FLOAT | Insurance Amount | Insurance included as a decimal in the row's currency (PayPal returns amounts as decimal strings). |
| `payer_account_id` | STRING | Payer Account ID | The payer's PayPal customer account id. The stable key for one buyer across payments. |
| `payer_email` | STRING | Payer Email | The payer's e-mail address. The join to customers, CRM and ad-platform conversions. |
| `payer_name` | STRING | Payer Name | The payer's full name as PayPal has it. |
| `payer_given_name` | STRING | Payer Given Name | The payer's given name. |
| `payer_surname` | STRING | Payer Surname | The payer's surname. |
| `payer_country` | STRING | Payer Country | Two-letter country of the payer's PayPal account. |
| `payer_status` | STRING | Payer Status | Y when the payer's PayPal account is verified, N otherwise. |
| `payer_address_status` | STRING | Payer Address Status | Y when PayPal confirmed the payer's address, N otherwise. |
| `shipping_name` | STRING | Shipping Name | Ship-to name on the transaction. |
| `shipping_country` | STRING | Shipping Country | Ship-to country code. |
| `shipping_city` | STRING | Shipping City | Ship-to city. |
| `shipping_postal_code` | STRING | Shipping Postal Code | Ship-to postal code. |
| `cart_items` | JSON | Cart Items | JSON list of the cart lines PayPal recorded with the payment: item_code, item_name, item_quantity, item_unit_price, item_amount, discount_amount, tax_amounts. Unnest in SQL for revenue by product. |
| `paypal_invoice_id` | STRING | Paypal Invoice ID | FK to [Invoices](./invoices.md). Id of the PayPal invoice this payment paid, when the payment came through PayPal Invoicing. Join to invoices. |
| `tax_inclusive` | BOOLEAN | Tax Inclusive | true when the item amounts already include tax. |
| `store_id` | STRING | Store ID | Store id for in-person or multi-store setups. |
| `terminal_id` | STRING | Terminal ID | Terminal id for in-person payments. |

# Example Questions

- How much did we receive through PayPal last month: gross, PayPal fees, refunds, chargebacks and net?
- Which rows are sales (T00xx), which are fees (T01xx), refunds (T11xx), chargebacks (T12xx) and withdrawals to the bank (T04xx)?
- Does the running sum of transaction_amount + fee_amount match ending_balance for every currency, and where does it first drift?
- How much recurring revenue came in as subscription payments (T0002), by paypal_reference_id?
- Which store orders (invoice_id) were paid with PayPal, and which PayPal payments have no order behind them?

## Joins

- [Balances](./balances.md) — `transaction_currency = currency` [N:1] — The day's rows reconcile to the balance snapshot of the next morning: running sum of transaction_amount + fee_amount per currency equals ending_balance, and the last ending_balance of a day equals the next as_of_date's total_balance.
- [Dispute details](./dispute-details.md) — `transaction_id = seller_transaction_id` [1:1] — The disputed payment; a T1201 chargeback row and a T1110 hold row point back at it through paypal_reference_id.
- [Invoices](./invoices.md) — `paypal_invoice_id = id` [N:1] — Set when the payment paid a PayPal invoice.
- [Billing plans](./billing-plans.md) — `paypal_reference_id = id` [N:1] — Indirect: a T0002 row's paypal_reference_id is the subscription id; the subscription resource names the plan. Until a subscriptions list exists, group recurring revenue by paypal_reference_id and look the plan up once.
