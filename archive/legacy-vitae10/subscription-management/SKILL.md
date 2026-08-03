---
name: subscription-management
description: Manage Recharge subscriptions on Shopify. Use when setting up subscription products, managing recurring orders, processing subscription changes, cancellations, skips, or upsells. Triggers on subscription, recurring, recharge, monthly, billing, upsell, or any mention of subscription management.
---

# Subscription Management

## Overview

Manage all subscription operations via the Recharge API on Shopify. Handles subscription setup, recurring billing, modifications, skips, cancellations, and add-on upsells.

## Authentication

Environment variables:
- `RECHARGE_API_TOKEN` — API token from Recharge dashboard
- `RECHARGE_STORE_URL` — your-store.myshopify.com

Header for every request:
```
X-Recharge-Access-Token: $RECHARGE_API_TOKEN
```

Base URL: `https://api.rechargeapps.com`

## Subscription Product Setup

### Create Base Subscription Product

```bash
curl -s -H "X-Recharge-Access-Token: $RECHARGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -X POST "https://api.rechargeapps.com/products" \
  -d '{
    "product": {
      "title": "[Base Product]",
      "charge_interval_frequency": 30,
      "order_interval_frequency": 30,
      "order_interval_unit": "day",
      "price": [Base Price],
      "sku": "BASE-SUB-SKU",
      "charge_interval_unit": "day"
    }
  }'
```

### Create Add-On Subscription

```bash
curl -s -H "X-Recharge-Access-Token: $RECHARGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -X POST "https://api.rechargeapps.com/products" \
  -d '{
    "product": {
      "title": "[Add-on Product]",
      "charge_interval_frequency": 30,
      "order_interval_frequency": 30,
      "order_interval_unit": "day",
      "price": [Add-on Price],
      "sku": "ADDON-SUB-SKU"
    }
  }'
```

## Subscription Operations

### List Active Subscriptions

```bash
curl -s -H "X-Recharge-Access-Token: $RECHARGE_API_TOKEN" \
  "https://api.rechargeapps.com/subscriptions?status=active&limit=50" \
  | jq '.subscriptions[] | {id, status, next_charge_date, price, quantity}'
```

### Get Subscription by ID

```bash
curl -s -H "X-Recharge-Access-Token: $RECHARGE_API_TOKEN" \
  "https://api.rechargeapps.com/subscriptions/SUBSCRIPTION_ID" \
  | jq '.subscription'
```

### Update Subscription (Frequency, Quantity, Add-On)

```bash
curl -s -H "X-Recharge-Access-Token: $RECHARGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -X PUT "https://api.rechargeapps.com/subscriptions/SUBSCRIPTION_ID" \
  -d '{
    "subscription": {
      "charge_interval_frequency": 30,
      "order_interval_frequency": 30,
      "quantity": 1
    }
  }'
```

### Add Add-On to Subscription

```bash
curl -s -H "X-Recharge-Access-Token: $RECHARGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -X POST "https://api.rechargeapps.com/subscriptions/SUBSCRIPTION_ID/add_ons" \
  -d '{
    "add_on": {
      "shopify_variant_id": VARIANT_ID,
      "quantity": 1
    }
  }'
```

### Skip Next Charge

```bash
curl -s -H "X-Recharge-Access-Token: $RECHARGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -X POST "https://api.rechargeapps.com/subscriptions/SUBSCRIPTION_ID/skip" \
  -d '{
    "subscription": {
      "date": "NEXT_CHARGE_DATE"
    }
  }'
```

### Cancel Subscription

```bash
curl -s -H "X-Recharge-Access-Token: $RECHARGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -X PUT "https://api.rechargeapps.com/subscriptions/SUBSCRIPTION_ID/cancel" \
  -d '{
    "subscription": {
      "cancellation_reason": "Customer request"
    }
  }'
```

## Customer Management

### Get Customer Subscriptions

```bash
curl -s -H "X-Recharge-Access-Token: $RECHARGE_API_TOKEN" \
  "https://api.rechargeapps.com/customers/CUSTOMER_ID/subscriptions" \
  | jq '.subscriptions[] | {id, status, product_title, price, next_charge_date}'
```

### Update Customer Payment Method

```bash
curl -s -H "X-Recharge-Access-Token: $RECHARGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -X PUT "https://api.rechargeapps.com/customers/CUSTOMER_ID/payment_source" \
  -d '{
    "payment_source": {
      "stripe_token": "tok_xxx"
    }
  }'
```

## Upsell Flows

### Trigger Upsell After Onboarding

When a customer completes onboarding and their base subscription is active, trigger a targeted upsell:

```bash
curl -s -H "X-Recharge-Access-Token: $RECHARGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -X POST "https://api.rechargeapps.com/subscriptions/SUBSCRIPTION_ID/add_ons" \
  -d '{
    "add_on": {
      "shopify_variant_id": ADDON_VARIANT_ID,
      "quantity": 1
    }
  }'
```

## Billing & Charges

### List Upcoming Charges

```bash
curl -s -H "X-Recharge-Access-Token: $RECHARGE_API_TOKEN" \
  "https://api.rechargeapps.com/charges?scheduled_at_min=TODAY&status=queued&limit=50" \
  | jq '.charges[] | {id, customer_id, scheduled_at, total_price, status}'
```

### Process Immediate Charge

```bash
curl -s -H "X-Recharge-Access-Token: $RECHARGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -X POST "https://api.rechargeapps.com/charges/CHARGE_ID/process" \
  | jq '.charge'
```

## Subscription Structure Template

```
Base Subscription: [Base Price]/month
├── [Base Product]
├── [Delivery cadence — e.g., 30 units delivered monthly]
├── Pause/skip anytime
└── Cancel anytime

Add-Ons ([Add-on Price]/month each):
├── [Add-on 1]
├── [Add-on 2]
├── [Add-on 3]
└── [Add-on 4]

Sample/Trial Pack: [Trial Price] one-time
├── [Trial duration — e.g., 7-day trial]
├── [Auto-converts or opt-in behaviour]
└── [Cancel policy during trial]
```

## Rules

- Always confirm before cancelling or modifying subscriptions
- Show customer their current plan before suggesting changes
- Offer skip as alternative to cancel (retention)
- Never charge without clear notification
- Maintain audit trail of all subscription changes
- Process refunds within 48 hours for cancelled subscriptions
- Send charge reminder 3 days before billing
