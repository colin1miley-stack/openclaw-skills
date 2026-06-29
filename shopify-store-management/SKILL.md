---
name: shopify-store-management
description: Manage Shopify store for supplement subscription businesses. Use when creating products, managing inventory, processing orders, setting up collections, configuring subscriptions, or handling checkout flows. Triggers on Shopify, store, product, order, inventory, collection, checkout, subscription, or cart keywords.
---

# Shopify Store Management

## Overview

Manage a Shopify store for a supplement subscription business — product listings, collections, subscriptions, inventory, orders, and checkout optimization. All operations use the Shopify Admin REST API.

## Authentication

Environment variables:
- `SHOPIFY_STORE_URL` — your-store.myshopify.com
- `SHOPIFY_ACCESS_TOKEN` — Custom app access token

Header for every request:
```
X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN
```

Base URL: `https://$SHOPIFY_STORE_URL/admin/api/2024-01`

## Product Operations

### Create a Product

```bash
curl -s -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -X POST "https://$SHOPIFY_STORE_URL/admin/api/2024-01/products.json" \
  -d '{
    "product": {
      "title": "Core Daily Supplement",
      "body_html": "<p>Your daily wellness drink. 30 sachets.</p>",
      "vendor": "Your Brand",
      "product_type": "Supplement",
      "tags": "daily, wellness, powder, subscription",
      "variants": [
        {
          "title": "30 Sachets / 1 Month",
          "price": "[Base Price]",
          "sku": "SKU-CORE-30",
          "inventory_quantity": 100,
          "requires_shipping": true
        }
      ]
    }
  }'
```

### Update Product

```bash
curl -s -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -X PUT "https://$SHOPIFY_STORE_URL/admin/api/2024-01/products/PRODUCT_ID.json" \
  -d '{
    "product": {
      "id": PRODUCT_ID,
      "title": "Updated Title"
    }
  }'
```

### List Products

```bash
curl -s -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  "https://$SHOPIFY_STORE_URL/admin/api/2024-01/products.json?limit=50" \
  | jq '.products[] | {id, title, status, variants: [.variants[] | {sku: .sku, price: .price, inventory: .inventory_quantity}]}'
```

## Collection Operations

### Create Collection

```bash
curl -s -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -X POST "https://$SHOPIFY_STORE_URL/admin/api/2024-01/custom_collections.json" \
  -d '{
    "custom_collection": {
      "title": "Daily Essentials",
      "body_html": "<p>Core daily supplements for optimal wellness.</p>",
      "published": true
    }
  }'
```

### Add Product to Collection

```bash
curl -s -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -X POST "https://$SHOPIFY_STORE_URL/admin/api/2024-01/collects.json" \
  -d '{
    "collect": {
      "product_id": PRODUCT_ID,
      "collection_id": COLLECTION_ID
    }
  }'
```

## Order Operations

### List Recent Orders

```bash
curl -s -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  "https://$SHOPIFY_STORE_URL/admin/api/2024-01/orders.json?status=any&limit=10" \
  | jq '.orders[] | {name, email: .email, total: .total_price, status: .fulfillment_status, created: .created_at}'
```

### Fulfill Order

```bash
curl -s -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -X POST "https://$SHOPIFY_STORE_URL/admin/api/2024-01/orders/ORDER_ID/fulfillments.json" \
  -d '{
    "fulfillment": {
      "location_id": LOCATION_ID,
      "tracking_number": "TRACKING_NUMBER",
      "tracking_company": "Royal Mail"
    }
  }'
```

## Inventory Management

### Check Inventory

```bash
curl -s -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  "https://$SHOPIFY_STORE_URL/admin/api/2024-01/products/PRODUCT_ID.json" \
  | jq '.product | {title, variants: [.variants[] | {sku: .sku, inventory: .inventory_quantity, price: .price}]}'
```

### Adjust Inventory

```bash
curl -s -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -X POST "https://$SHOPIFY_STORE_URL/admin/api/2024-01/inventory_levels/adjust.json" \
  -d '{
    "location_id": LOCATION_ID,
    "inventory_item_id": INVENTORY_ITEM_ID,
    "available_adjustment": 50
  }'
```

## Subscription Setup

For subscription products, use Shopify Subscriptions API or a subscription app like Recharge:

1. Create subscription-selling plans
2. Set up delivery intervals (monthly, quarterly)
3. Configure subscription discounts

## Rules

- Always confirm before creating or updating products/orders
- Show current state before making changes
- Respect rate limits: 40 requests per bucket, 2/second refill
- Never log access token in responses or memory
- Use pagination for large result sets
- Tag products consistently for collection automation

## Product Structure

```
Core Daily Supplement (Base) — [Base Price]/month
├── 30 sachets
├── Subscribe & Save option
└── One-time purchase option

Add-ons ([Add-on Price] each):
├── Sleep Support
├── Stress Relief
├── Recovery
├── Vitality
├── Hormone Balance
└── Men's Health

Sample Pack — One-time [Sample Price]
└── 7-day trial of Core supplement
```
