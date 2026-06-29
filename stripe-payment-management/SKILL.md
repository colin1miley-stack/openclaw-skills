---
name: stripe-payment-management
description: Manage Stripe payments for subscription businesses. Use when processing payments, handling refunds, managing subscriptions, or setting up payment flows. Triggers on stripe, payment, refund, charge, billing, invoice, or any mention of payment processing.
---

# Stripe Payment Management

## Overview

Manage Stripe payments for a subscription business — one-time purchases, subscription billing, refunds, invoices, and payment method management. Used alongside Shopify Payments for custom checkout flows and B2B invoicing.

## Authentication

Environment variable:
- `STRIPE_SECRET_KEY` — Stripe secret key (sk_live_... or sk_test_...)
- `STRIPE_WEBHOOK_SECRET` — For webhook verification

Header for every request:
```
Authorization: Bearer $STRIPE_SECRET_KEY
```

Base URL: `https://api.stripe.com/v1`

## Payment Processing

### Create Payment Intent (One-Time)

```bash
curl -s -u "$STRIPE_SECRET_KEY:" \
  -d "amount=[Sample Amount]" \
  -d "currency=gbp" \
  -d "description=Sample Pack" \
  -d "metadata[customer_email]=customer@email.com" \
  -d "metadata[product]=sample-pack" \
  https://api.stripe.com/v1/payment_intents
```

### Confirm Payment Intent

```bash
curl -s -u "$STRIPE_SECRET_KEY:" \
  -X POST \
  https://api.stripe.com/v1/payment_intents/PAYMENT_INTENT_ID/confirm \
  -d "payment_method=pm_xxx"
```

## Subscription Billing

### Create Customer

```bash
curl -s -u "$STRIPE_SECRET_KEY:" \
  -d "email=customer@email.com" \
  -d "name=Customer Name" \
  -d "description=Subscriber" \
  https://api.stripe.com/v1/customers
```

### Create Subscription

```bash
curl -s -u "$STRIPE_SECRET_KEY:" \
  -d "customer=CUSTOMER_ID" \
  -d "items[0][price]=[Price ID]" \
  -d "items[1][price]=[Add-on Price ID]" \
  -d "billing_cycle_anchor=now" \
  -d "payment_behavior=default_incomplete" \
  https://api.stripe.com/v1/subscriptions
```

### Update Subscription (Add Add-On)

```bash
curl -s -u "$STRIPE_SECRET_KEY:" \
  -X POST \
  https://api.stripe.com/v1/subscription_items \
  -d "subscription=SUBSCRIPTION_ID" \
  -d "price=[New Price ID]" \
  -d "quantity=1"
```

### Cancel Subscription

```bash
curl -s -u "$STRIPE_SECRET_KEY:" \
  -X DELETE \
  https://api.stripe.com/v1/subscriptions/SUBSCRIPTION_ID
```

## Refunds

### Full Refund

```bash
curl -s -u "$STRIPE_SECRET_KEY:" \
  -X POST \
  https://api.stripe.com/v1/refunds \
  -d "payment_intent=PAYMENT_INTENT_ID" \
  -d "reason=requested_by_customer"
```

### Partial Refund

```bash
curl -s -u "$STRIPE_SECRET_KEY:" \
  -X POST \
  https://api.stripe.com/v1/refunds \
  -d "payment_intent=PAYMENT_INTENT_ID" \
  -d "amount=[Refund Amount]" \
  -d "reason=requested_by_customer"
```

## Invoicing

### Create Invoice

```bash
curl -s -u "$STRIPE_SECRET_KEY:" \
  -d "customer=CUSTOMER_ID" \
  -d "auto_advance=true" \
  -d "collection_method=send_invoice" \
  -d "days_until_due=14" \
  https://api.stripe.com/v1/invoices
```

### Add Line Item to Invoice

```bash
curl -s -u "$STRIPE_SECRET_KEY:" \
  -d "invoice=INVOICE_ID" \
  -d "amount=[Base Amount]" \
  -d "currency=gbp" \
  -d "description=Core Daily Supplement - Monthly" \
  https://api.stripe.com/v1/invoiceitems
```

### Finalize and Send Invoice

```bash
curl -s -u "$STRIPE_SECRET_KEY:" \
  -X POST \
  https://api.stripe.com/v1/invoices/INVOICE_ID/finalize
```

## Payment Method Management

### List Customer Payment Methods

```bash
curl -s -u "$STRIPE_SECRET_KEY:" \
  "https://api.stripe.com/v1/customers/CUSTOMER_ID/payment_methods?type=card" \
  | jq '.data[] | {id, card: {brand, last4, exp_month, exp_year}}'
```

### Update Default Payment Method

```bash
curl -s -u "$STRIPE_SECRET_KEY:" \
  -X POST \
  https://api.stripe.com/v1/customers/CUSTOMER_ID \
  -d "invoice_settings[default_payment_method]=pm_xxx"
```

## Webhook Handling

### Webhook Events to Listen For

```
payment_intent.succeeded — Payment completed
payment_intent.payment_failed — Payment failed
invoice.paid — Subscription invoice paid
invoice.payment_failed — Subscription payment failed
customer.subscription.created — New subscription
customer.subscription.updated — Subscription modified
customer.subscription.deleted — Subscription cancelled
charge.refunded — Refund processed
```

### Verify Webhook Signature

```python
import stripe

endpoint_secret = os.getenv("STRIPE_WEBHOOK_SECRET")

def verify_webhook(payload, sig_header):
    try:
        event = stripe.Webhook.construct_event(
            payload, sig_header, endpoint_secret
        )
        return event
    except ValueError:
        # Invalid payload
        return None
    except stripe.error.SignatureVerificationError:
        # Invalid signature
        return None
```

## Price IDs

```
Base Subscription: [Price ID] — [Base Price]/month
Add-Ons (each): [Add-on Price ID] — [Add-on Price]/month
Sample Pack: [Sample Price ID] — [Sample Price] one-time
Annual Subscription (10% off): [Annual Price ID] — [Annual Price]/year
```

## Rules

- Never store raw card numbers — use Stripe's vault
- Log all payment events for audit trail
- Send email confirmation for every charge
- Process refunds within 48 hours
- Retry failed payments 3 times before marking subscription at-risk
- Notify customer 3 days before charge
- Keep Stripe keys in environment variables only
- Use test keys for development, live keys for production
