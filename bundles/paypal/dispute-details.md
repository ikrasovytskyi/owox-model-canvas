---
title: "Dispute details"
description: |
  One row per dispute with the disputed transaction id, its gross amount, the outcome and the refund PayPal made, any settlement offer and the message trail. The join from a chargeback to the payment and the order behind it.
tags: ["owox", "connector", "paypal"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.paypal.com/docs/api/transaction-search/v1/"
    title: "OWOX Data Marts — PayPal custom connector (PayPal, 7 nodes) on the PayPal REST APIs: Transaction Search v1, Disputes v1, Invoicing v2, Subscriptions v1, Catalog Products v1"
  - id: "vendor-reference"
    resource: "https://developer.paypal.com/docs/api/customer-disputes/v1/#disputes_get"
    title: "PayPal API reference — Dispute details"
---

## Connector endpoint

- **Endpoint:** PayPal · `dispute_details`
- **Default destination table:** `paypal_dispute_details`
- **Unique keys:** `dispute_id`

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `dispute_id` | STRING | Dispute ID | PK. PayPal dispute id (PP-D-…). |
| `account_id` | STRING | Account ID | Your PayPal merchant account id as entered in the connector (Account settings → Business information); lets several PayPal accounts share one table. Empty when not set. |
| `create_time` | TIMESTAMP | Create Time | When the dispute was opened (UTC). |
| `update_time` | TIMESTAMP | Update Time | When the dispute last changed (UTC). The field the window filters on, so a status change reloads the row. |
| `reason` | STRING | Reason | Why the buyer disputed: MERCHANDISE_OR_SERVICE_NOT_RECEIVED, MERCHANDISE_OR_SERVICE_NOT_AS_DESCRIBED, UNAUTHORISED, CREDIT_NOT_PROCESSED, DUPLICATE_TRANSACTION, INCORRECT_AMOUNT, PAYMENT_BY_OTHER_MEANS, CANCELED_RECURRING_BILLING, PROBLEM_WITH_REMITTANCE, OTHER. |
| `status` | STRING | Status | OPEN, WAITING_FOR_BUYER_RESPONSE, WAITING_FOR_SELLER_RESPONSE, UNDER_REVIEW, RESOLVED or OTHER. |
| `dispute_state` | STRING | Dispute State | The seller-side state: OPEN_INQUIRIES, REQUIRED_ACTION, REQUIRED_OTHER_PARTY_ACTION, UNDER_PAYPAL_REVIEW, APPEALABLE, RESOLVED. |
| `dispute_life_cycle_stage` | STRING | Dispute Life Cycle Stage | INQUIRY (a buyer question, no money moved), CHARGEBACK, PRE_ARBITRATION or ARBITRATION. |
| `dispute_channel` | STRING | Dispute Channel | INTERNAL (opened in PayPal), EXTERNAL (a card issuer chargeback) or ALERT. |
| `dispute_amount` | FLOAT | Dispute Amount | Amount in dispute as a decimal. |
| `dispute_currency` | STRING | Dispute Currency | Currency of dispute_amount. |
| `seller_response_due_date` | TIMESTAMP | Seller Response Due Date | Deadline for the seller's response or evidence (UTC). |
| `buyer_response_due_date` | TIMESTAMP | Buyer Response Due Date | Deadline for the buyer's response (UTC). |
| `seller_transaction_id` | STRING | Seller Transaction ID | FK to [Transactions](./transactions.md). The seller-side PayPal transaction id that is disputed. Join to transactions.transaction_id. |
| `buyer_transaction_id` | STRING | Buyer Transaction ID | The buyer-side id of the same payment. |
| `transaction_create_time` | TIMESTAMP | Transaction Create Time | When the disputed payment was made (UTC). |
| `transaction_status` | STRING | Transaction Status | Status of the disputed payment: COMPLETED, UNCLAIMED, DENIED, FAILED, HELD, PENDING, PARTIALLY_REFUNDED, REFUNDED, REVERSED. |
| `gross_amount` | FLOAT | Gross Amount | Gross amount of the disputed payment as a decimal. |
| `gross_currency` | STRING | Gross Currency | Currency of gross_amount. |
| `transaction_invoice_number` | STRING | Transaction Invoice Number | The merchant's invoice or order number on the disputed payment. |
| `transaction_custom` | STRING | Transaction Custom | The merchant's custom field on the disputed payment. |
| `buyer_name` | STRING | Buyer Name | The buyer's name on the disputed payment. |
| `disputed_items` | JSON | Disputed Items | JSON list of the items in dispute (item_id, item_quantity, dispute_amount, reason, notes). |
| `outcome_code` | STRING | Outcome Code | How it ended: RESOLVED_BUYER_FAVOUR, RESOLVED_SELLER_FAVOUR, RESOLVED_WITH_PAYOUT, CANCELED_BY_BUYER, ACCEPTED, DENIED, NONE. |
| `outcome_reason` | STRING | Outcome Reason | PayPal's reason for the outcome, when given. |
| `amount_refunded` | FLOAT | Amount Refunded | Amount refunded to the buyer as part of the outcome, as a decimal. |
| `external_reason_code` | STRING | External Reason Code | The card network's reason code for an external chargeback. |
| `offer_type` | STRING | Offer Type | Settlement offer type: REFUND, REFUND_WITH_RETURN, REFUND_WITH_REPLACEMENT, REPLACEMENT. |
| `buyer_requested_amount` | FLOAT | Buyer Requested Amount | What the buyer asked for, as a decimal. |
| `seller_offered_amount` | FLOAT | Seller Offered Amount | What the seller offered, as a decimal. |
| `messages` | JSON | Messages | JSON list of the messages exchanged on the dispute (posted_by, time_posted, content). |
| `evidences` | JSON | Evidences | JSON list of the evidence documents filed (evidence_type, date, notes). |
| `money_movements` | JSON | Money Movements | JSON list of the money PayPal moved for this dispute (affected_party, amount, initiated_time, type, reason). |

# Example Questions

- Which payment did each dispute concern, and how did it end: refunded, won, lost?
- How much did PayPal refund to buyers on our behalf this quarter?

## Joins

- [Transactions](./transactions.md) — `seller_transaction_id = transaction_id` [N:1] — The payment that was disputed.
