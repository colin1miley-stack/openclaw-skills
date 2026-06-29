---
name: customer-support-framework
description: Manage customer support tickets for any DTC or subscription brand. Use when handling inquiries, complaints, returns, shipping issues, or product questions. Triggers on support, ticket, help, inquiry, complaint, return, refund, shipping, or any mention of customer service.
author: colin
license: MIT
originally_built_for: [Your Brand]
---

> Generic framework for universal use across any DTC or subscription business.

# Customer Support Ticketing Framework

## Overview

Manage customer support for [Your Brand] — ticket creation, assignment, response templates, escalation rules, and satisfaction tracking. Ensures every customer gets a response within SLA.

## Authentication

Environment variables:
- `ZENDESK_SUBDOMAIN` — [your-domain].zendesk.com
- `ZENDESK_API_TOKEN` — API token
- `ZENDESK_EMAIL` — admin email for auth

Header for every request:
```
Authorization: Basic $(echo -n "$ZENDESK_EMAIL/token:$ZENDESK_API_TOKEN" | base64)
```

Base URL: `https://$ZENDESK_SUBDOMAIN.zendesk.com/api/v2`

## Ticket Management

### Create Ticket

```bash
curl -s -H "Content-Type: application/json" \
  -u "$ZENDESK_EMAIL/token:$ZENDESK_API_TOKEN" \
  -X POST "https://$ZENDESK_SUBDOMAIN.zendesk.com/api/v2/tickets.json" \
  -d '{
    "ticket": {
      "subject": "Order not received",
      "comment": {
        "body": "Customer reports not receiving order #1042 placed 5 days ago."
      },
      "requester": {
        "name": "Customer Name",
        "email": "customer@email.com"
      },
      "priority": "high",
      "tags": ["shipping", "order_issue"]
    }
  }'
```

### Get Ticket

```bash
curl -s -u "$ZENDESK_EMAIL/token:$ZENDESK_API_TOKEN" \
  "https://$ZENDESK_SUBDOMAIN.zendesk.com/api/v2/tickets/TICKET_ID.json" \
  | jq '.ticket | {id, subject, status, priority, created_at}'
```

### Update Ticket Status

```bash
curl -s -H "Content-Type: application/json" \
  -u "$ZENDESK_EMAIL/token:$ZENDESK_API_TOKEN" \
  -X PUT "https://$ZENDESK_SUBDOMAIN.zendesk.com/api/v2/tickets/TICKET_ID.json" \
  -d '{
    "ticket": {
      "status": "solved",
      "comment": {
        "body": "Issue resolved. Tracking number provided: RM123456789GB",
        "public": true
      }
    }
  }'
```

### List Open Tickets

```bash
curl -s -u "$ZENDESK_EMAIL/token:$ZENDESK_API_TOKEN" \
  "https://$ZENDESK_SUBDOMAIN.zendesk.com/api/v2/search.json?query=status:open+type:ticket&sort_by=created_at&sort_order=asc" \
  | jq '.results[] | {id, subject, status, created_at, requester}'
```

## Response Templates

### Shipping Delay

```
Hi {{ticket.requester.name}},

I'm sorry to hear your order hasn't arrived yet.

Your order #{{ticket.custom_fields.order_number}} was dispatched on {{ticket.custom_fields.dispatch_date}} via [Carrier Name]. Here's your tracking link:

{{ticket.custom_fields.tracking_url}}

Orders typically arrive within [X–Y] working days. If it hasn't arrived by {{expected_date}}, please let me know and I'll send a replacement immediately.

[Founder Name]
[Your Brand] Support
```

### Product Question

```
Hi {{ticket.requester.name}},

Great question about {{product_name}}.

{{answer}}

If you have any other questions, just reply to this email.

[Founder Name]
Founder, [Your Brand]
```

### Subscription Cancellation

```
Hi {{ticket.requester.name}},

I've cancelled your subscription effective immediately. You won't be charged again.

Your final shipment (if any) will arrive as scheduled.

I'm sorry to see you go. If you change your mind, you can reactivate anytime from your account page.

To help us improve: What made you cancel? (Reply optional)

[Founder Name]
Founder, [Your Brand]
```

### Refund Approved

```
Hi {{ticket.requester.name}},

Your refund of £{{amount}} has been processed and should appear in your account within 3–5 working days.

If you don't see it by {{expected_date}}, please let me know.

[Founder Name]
[Your Brand] Support
```

## SLA Rules

| Priority | Response Time | Resolution Time |
|----------|--------------|-----------------|
| Urgent | 1 hour | 4 hours |
| High | 4 hours | 24 hours |
| Normal | 8 hours | 48 hours |
| Low | 24 hours | 72 hours |

## Escalation Rules

1. **Auto-escalate** if ticket open > SLA time
2. **Flag for [Founder Name]** if: cancellation request, refund > £50, health/medical question, legal concern
3. **Escalate to** admin if: data breach concern, regulatory inquiry

## Ticket Categories

```
Billing — Payment issues, refunds, invoices
Shipping — Delivery, tracking, lost packages
Product — Questions about ingredients, usage, effects
Subscription — Changes, cancellations, skips
Returns — Return requests, exchanges
Account — Login, profile, data
Feedback — Reviews, suggestions, complaints
Compliance — Health claims, regulatory questions
```

## Automated Responses

### Auto-Reply (New Ticket)

```
Hi,

Thanks for contacting [Your Brand] support. We've received your message and will respond within {{sla_hours}} hours.

For urgent issues, please include your order number.

[Founder Name]
[Your Brand] Support
```

### Auto-Resolve (Common Questions)

```bash
# Check if ticket matches FAQ — if yes, auto-respond and close
# Track which FAQs are most common for improvement
```

## Rules

- Every ticket gets a human response (no fully automated closures)
- Use customer's first name
- Sign all responses "[Founder Name], [Your Brand] Support" or "[Founder Name], Founder"
- Never make medical claims in support responses
- Document all refund/return decisions
- Track ticket volume and resolution time weekly
- Flag recurring issues for product/process improvement
