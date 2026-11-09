---
title: "Disputes"
description: |
  Every buyer dispute, inquiry and chargeback with its reason, stage, status, amount and response deadlines. Loaded by the window on `update_time`, so status changes are reloaded.
tags: ["owox", "connector", "paypal"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://developer.paypal.com/docs/api/transaction-search/v1/"
    title: "OWOX Data Marts — PayPal custom connector (PayPal, 7 nodes) on the PayPal REST APIs: Transaction Search v1, Disputes v1, Invoicing v2, Subscriptions v1, Catalog Products v1"
  - id: "vendor-reference"
    resource: "https://developer.paypal.com/docs/api/customer-disputes/v1/#disputes_list"
    title: "PayPal API reference — Disputes"
---

## Connector endpoint

- **Endpoint:** PayPal · `disputes`
- **Default destination table:** `paypal_disputes`
- **Unique keys:** `dispute_id`
- **Loads:** a dated snapshot per run window (`incremental: range`)

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `dispute_id` | STRING | Dispute ID | PK. FK to [Dispute details](./dispute-details.md). PayPal dispute id (PP-D-…). |
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

# Example Questions

- How many disputes are open, how much is at stake, and which responses are due this week?
- What is our dispute rate (disputes per hundred payments) by month and by reason?

## Joins

- [Dispute details](./dispute-details.md) — `dispute_id = dispute_id` [1:1] — Same dispute; details add the disputed transaction, outcome, offer and messages.
