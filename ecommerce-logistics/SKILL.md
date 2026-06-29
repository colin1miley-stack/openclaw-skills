---
name: ecommerce-logistics
description: Manage logistics, fulfillment, shipping, and returns for a DTC subscription brand. Use when processing orders, tracking shipments, managing inventory, handling returns, or coordinating with 3PL. Triggers on shipping, fulfillment, delivery, warehouse, 3PL, returns, inventory, stock, or any mention of logistics operations.
originally_built_for: Vitae10
---

> Originally developed for a supplement subscription business, now genericized for universal use.

# Logistics & Fulfillment — [Your Brand]

## Overview

Manage all logistics for [Your Brand] — order fulfillment, shipping, 3PL coordination, returns processing, and inventory tracking. Ensures every order ships within 24 hours and arrives within 3-5 working days.

## Authentication

Environment variables:
- `SHOPIFY_ACCESS_TOKEN` — Shopify Admin API token
- `SHOPIFY_STORE_URL` — your-store.myshopify.com
- `ROYAL_MAIL_API_KEY` — Royal Mail Click & Drop API
- `3PL_API_KEY` — Third-party logistics provider API

## Order Fulfillment

### Create Fulfillment

```bash
curl -s -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -X POST "https://$SHOPIFY_STORE_URL/admin/api/2024-01/orders/ORDER_ID/fulfillments.json" \
  -d '{
    "fulfillment": {
      "location_id": LOCATION_ID,
      "tracking_number": "RM123456789GB",
      "tracking_company": "Royal Mail",
      "line_items": [
        {"id": LINE_ITEM_ID, "quantity": 1}
      ]
    }
  }'
```

### Get Fulfillment Status

```bash
curl -s -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  "https://$SHOPIFY_STORE_URL/admin/api/2024-01/orders/ORDER_ID/fulfillments.json" \
  | jq '.fulfillments[] | {id, status, tracking_number, tracking_company, created_at}'
```

## Shipping

### Royal Mail Click & Drop

```bash
# Create shipping label via Royal Mail API
curl -s -H "Authorization: Bearer $ROYAL_MAIL_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST "https://api.royalmail.com/shipping/v2/orders" \
  -d '{
    "recipient": {
      "name": "Customer Name",
      "address": {
        "addressLine1": "123 High Street",
        "city": "London",
        "postcode": "SW1A 1AA",
        "country": "GB"
      }
    },
    "sender": {
      "name": "[Your Brand]",
      "address": {
        "addressLine1": "Warehouse Address",
        "city": "Birmingham",
        "postcode": "B1 1AA",
        "country": "GB"
      }
    },
    "items": [
      {
        "description": "[Your Brand] Core Daily Supplement",
        "value": 39.00,
        "weight": 500,
        "quantity": 1
      }
    ],
    "service": "RM_TRACKED_48"
  }'
```

### Shipping Rates

```
UK Standard (Royal Mail 48): £3.50 — 3-5 working days
UK Express (Royal Mail 24): £5.95 — 1-2 working days
UK Free Shipping: Orders over £50
EU Shipping: £8.50 — 5-10 working days
EU Free Shipping: Orders over £75
```

## 3PL Integration

### Send Order to 3PL

```bash
curl -s -H "Authorization: Bearer $3PL_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST "https://api.3pl.com/v1/orders" \
  -d '{
    "order_id": "ORDER_ID",
    "reference": "YB-1042",
    "recipient": {
      "name": "Customer Name",
      "address_line1": "123 High Street",
      "city": "London",
      "postcode": "SW1A 1AA",
      "country": "GB",
      "phone": "+44 7700 900000"
    },
    "items": [
      {
        "sku": "YB-CORE-30",
        "quantity": 1,
        "description": "[Your Brand] Core Daily + Gut Microbe"
      }
    ],
    "shipping_method": "royal_mail_48",
    "service_level": "standard"
  }'
```

### Check 3PL Inventory

```bash
curl -s -H "Authorization: Bearer $3PL_API_KEY" \
  "https://api.3pl.com/v1/inventory?sku=YB-CORE-30" \
  | jq '.inventory[] | {sku, available, reserved, on_hand}'
```

### Get 3PL Order Status

```bash
curl -s -H "Authorization: Bearer $3PL_API_KEY" \
  "https://api.3pl.com/v1/orders/ORDER_ID" \
  | jq '.order | {id, status, tracking_number, shipped_at}'
```

## Returns Processing

### Initiate Return

```bash
curl -s -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -X POST "https://$SHOPIFY_STORE_URL/admin/api/2024-01/orders/ORDER_ID/returns.json" \
  -d '{
    "return": {
      "line_items": [
        {
          "line_item_id": LINE_ITEM_ID,
          "quantity": 1,
          "return_reason": "customer_changed_mind"
        }
      ]
    }
  }'
```

### Generate Return Label

```bash
# Via Royal Mail
# Generate prepaid return label and email to customer
curl -s -H "Authorization: Bearer $ROYAL_MAIL_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST "https://api.royalmail.com/shipping/v2/returns" \
  -d '{
    "recipient": {
      "name": "[Your Brand] Returns",
      "address": {
        "addressLine1": "Returns Department",
        "city": "Birmingham",
        "postcode": "B1 1AA",
        "country": "GB"
      }
    },
    "service": "RM_RETURNS"
  }'
```

## Inventory Management

### Check Stock Levels

```bash
curl -s -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  "https://$SHOPIFY_STORE_URL/admin/api/2024-01/inventory_levels.json?inventory_item_ids=ITEM_ID" \
  | jq '.inventory_levels[] | {inventory_item_id, available, location_id}'
```

### Set Reorder Alerts

```bash
# Check all products with low stock
curl -s -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  "https://$SHOPIFY_STORE_URL/admin/api/2024-01/products.json?limit=250" \
  | jq '.products[] | select(.variants[0].inventory_quantity < 50) | {title, sku: .variants[0].sku, stock: .variants[0].inventory_quantity}'
```

### Adjust Inventory

```bash
curl -s -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -X POST "https://$SHOPIFY_STORE_URL/admin/api/2024-01/inventory_levels/adjust.json" \
  -d '{
    "location_id": LOCATION_ID,
    "inventory_item_id": INVENTORY_ITEM_ID,
    "available_adjustment": 500
  }'
```

## Warehouse Operations

### Receiving Stock

```
1. Count received items against PO
2. Update Shopify inventory
3. Update 3PL system (if using 3PL)
4. Quality check samples
5. Store in designated location
6. Update stock location records
```

### Pick & Pack

```
1. Receive order from Shopify
2. Generate pick list
3. Pick items from shelf
4. Quality check
5. Pack in branded box
6. Apply shipping label
7. Mark as fulfilled in Shopify
8. Hand to courier
```

## Packaging Standards

### Subscription Box

```
Branded [Your Brand] box
├── 30 sachets (arranged neatly)
├── Welcome card with QR code
├── Usage instructions
├── QR code to join community
└── Referral card (£10 off for friends)
```

### Trial Pack

```
Branded [Your Brand] envelope
├── 7 sachets
├── Quick start guide
├── Subscription offer card
└── Feedback request card
```

## SLA Targets

| Metric | Target |
|--------|--------|
| Order to dispatch | < 24 hours |
| UK delivery | 3-5 working days |
| EU delivery | 5-10 working days |
| Returns processing | < 48 hours |
| Stock accuracy | > 99% |
| Order accuracy | > 99.5% |

## Rules

- Ship all orders within 24 hours (weekdays)
- Use branded packaging for all shipments
- Include welcome card in first order
- Track all shipments and pro-actively notify delays
- Process returns within 48 hours of receipt
- Maintain minimum 30-day safety stock
- Reorder when stock drops below 100 units
- Weekly inventory reconciliation
- Monthly 3PL performance review
