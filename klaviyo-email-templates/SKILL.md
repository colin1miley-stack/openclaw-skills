---
name: klaviyo-email-templates
description: |
  Reusable Klaviyo email marketing framework for any DTC or subscription brand.
  Use when creating email flows, campaigns, segments, templates, or analyzing email
  performance. Triggers on Klaviyo, email, flow, campaign, sequence, newsletter,
  abandon cart, welcome, or nurture keywords.
---

# Klaviyo Email Templates

Reusable Klaviyo email marketing framework — flows, campaigns, segments, templates, and analytics. All operations use the Klaviyo API.

## Authentication

Environment variable:
- `KLAVIYO_API_KEY` — Private API key (pk_...)

Header for every request:
```
Authorization: Bearer $KLAVIYO_API_KEY
```

Base URL: `https://a.klaviyo.com/api`

## Core Email Flows

### 1. Welcome Series (New Subscribers)

**Trigger:** List signup, quiz completion, sample pack order

**Flow:**
1. Email 1 (Immediate): Welcome + brand story
2. Email 2 (Day 2): Science behind [Product Name]
3. Email 3 (Day 4): Personalization quiz reminder
4. Email 4 (Day 7): Social proof + testimonials
5. Email 5 (Day 10): Subscription offer + discount

### 2. Abandoned Cart Recovery

**Trigger:** Added to cart but didn't checkout

**Flow:**
1. Email 1 (1 hour): Friendly reminder
2. Email 2 (24 hours): Incentive (e.g. 10% off)
3. Email 3 (72 hours): Urgency + social proof

### 3. Post-Purchase Nurture

**Trigger:** First order placed

**Flow:**
1. Email 1 (Day 1): Order confirmation + onboarding tips
2. Email 2 (Day 7): How to use + results timeline
3. Email 3 (Day 14): Check-in + upsell to add-ons
4. Email 4 (Day 21): Review request + referral offer
5. Email 5 (Day 25): Subscription reminder (before first month ends)

### 4. Re-engagement (Win-Back)

**Trigger:** No purchase in 60 days

**Flow:**
1. Email 1 (Day 60): "We miss you" + personalized offer
2. Email 2 (Day 67): Survey + feedback request
3. Email 3 (Day 75): Final offer + unsubscribe option

### 5. Subscription Management

**Trigger:** Subscription events

**Flow:**
- Upcoming charge reminder (3 days before)
- Delay/cancel win-back
- Upgrade prompts (add-on recommendations)

## API Operations

### Create a Flow

```bash
curl -s -H "Authorization: Bearer $KLAVIYO_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST "https://a.klaviyo.com/api/flows" \
  -d '{
    "data": {
      "type": "flow",
      "attributes": {
        "name": "[Your Brand] Welcome Series",
        "status": "draft"
      }
    }
  }'
```

### Create a Campaign

```bash
curl -s -H "Authorization: Bearer $KLAVIYO_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST "https://a.klaviyo.com/api/campaigns" \
  -d '{
    "data": {
      "type": "campaign",
      "attributes": {
        "name": "Monthly Newsletter - [Month]",
        "status": "draft",
        "channel": "email"
      }
    }
  }'
```

### Create a Segment

```bash
curl -s -H "Authorization: Bearer $KLAVIYO_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST "https://a.klaviyo.com/api/segments" \
  -d '{
    "data": {
      "type": "segment",
      "attributes": {
        "name": "Sample Pack Customers - No Subscription",
        "definition": {
          "and": [
            {"property": "ordered_sample_pack", "operator": "equals", "value": true},
            {"property": "has_subscription", "operator": "equals", "value": false}
          ]
        }
      }
    }
  }'
```

### Get Flow Metrics

```bash
curl -s -H "Authorization: Bearer $KLAVIYO_API_KEY" \
  "https://a.klaviyo.com/api/flow-actions/FLOW_ACTION_ID/metrics" \
  | jq '.data | {opens, clicks, conversions, revenue}'
```

### Get Campaign Performance

```bash
curl -s -H "Authorization: Bearer $KLAVIYO_API_KEY" \
  "https://a.klaviyo.com/api/campaigns/CAMPAIGN_ID" \
  | jq '.data.attributes | {name, status, send_time, scheduled_send_time}'
```

## Email Templates

### Welcome Email Template

```html
Subject: Welcome to [Your Brand] — Your journey starts now

Hi {{first_name|default:"there"}},

Welcome to [Your Brand]. I'm [Founder Name], founder.

Your body is about to get what it's been missing.

Here's what to expect:
• Week 1: Gut health improvements (less bloating, better digestion)
• Week 2-3: Energy levels rise
• Week 4: Full baseline established

Your personalized recommendation is ready. [Take the Quiz]

Questions? Just reply to this email.

[Founder Name]
[Your Brand]
```

### Abandoned Cart Template

```html
Subject: You left something behind...

Hi {{first_name|default:"there"}},

I noticed you were interested in [Product Name] but didn't complete your order.

Here's what you selected:
{{event.items}}

Complete your order in the next 24 hours and get [Discount]% off with code [CODE].

[Complete My Order]

[Founder Name]
```

## Rules

- Always confirm before sending campaigns or updating flows
- Preview emails before sending
- Test flows with a small segment first
- Respect unsubscribe requests immediately
- Never send to purchased lists
- Maintain 20%+ open rate, 3%+ click rate targets
- Use A/B testing for subject lines and CTAs
- Segment by behavior (quiz takers, sample buyers, subscribers)

## Recommended Segments

```
1. All Subscribers (list)
2. Quiz Completers (behavior)
3. Sample Pack Customers (purchase)
4. Active Subscribers (subscription)
5. Lapsed Subscribers (cancelled)
6. High Intent (added to cart, no purchase)
7. Engaged (opened 3+ emails in 30 days)
8. At-Risk (no purchase in 45 days)
```
