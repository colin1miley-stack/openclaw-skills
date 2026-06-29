---
name: email-automation-templates
description: |
  Reusable email automation framework: check inbox replies, update outreach trackers,
  send templated emails, and manage follow-up pipelines. Use when the user asks to
  "check emails", "check supplier replies", "track responses", "send follow-up",
  "email automation", "inbox check", or any task involving monitoring or managing
  email-based workflows. Also triggers on mentions of outreach tracking, supplier
  response status, or email pipeline management.
---

# Email Automation Templates

Reusable email automation framework for outreach workflows. Check replies, update trackers, send follow-ups, and manage pipelines for any brand.

## Prerequisites

The following must be configured:
- Email automation script in workspace root (e.g. `email_automation_oauth.py` or equivalent)
- OAuth token saved after authentication (`token.json`)
- `[Your Brand]-Supplier-Outreach.md` tracker file (or equivalent naming convention)

## Capabilities

### 1. Check Supplier Replies

Check the inbox for replies from known suppliers and update the tracker automatically.

```bash
cd workspace
python email_automation_oauth.py suppliers
```

**What it does:**
- Searches inbox for emails from supplier domains (last 14 days)
- Returns matches with subject, date, and preview
- Does NOT auto-update tracker (user reviews first)

**When to run:**
- User asks "check supplier replies"
- User asks "any responses from suppliers"
- Heartbeat check (if configured in HEARTBEAT.md)

### 2. Send Templated Email

Send an email using a template file.

```bash
python email_automation_oauth.py send <to> <subject> <body_file> [attachments...]
```

**When to use:**
- User wants to send a follow-up
- User wants to send a new outreach email
- Batch send (loop through a list)

### 3. Read Inbox

Read recent emails with optional search query.

```bash
python email_automation_oauth.py inbox [query]
```

**When to use:**
- General inbox check
- Search for specific topics
- Verify sent emails were delivered

## Tracker Update Protocol

When supplier replies are found, update `[Your Brand]-Supplier-Outreach.md`:

1. **Read the tracker** to find the supplier entry
2. **Update status:** `⬜ Not contacted` → `✅ Contacted` → `📨 Reply received` → `📞 Call scheduled` → `💰 Quote received` → `🤝 Negotiating` → `✅ Approved`
3. **Add notes** with key details from the reply (MOQ, pricing, lead time, certifications)
4. **Log the update** in `memory/YYYY-MM-DD.md`

### Status Flow

| Status | Meaning | Next Action |
|--------|---------|-------------|
| ⬜ Not contacted | Never emailed | Send initial outreach |
| ✅ Contacted | Email sent, awaiting reply | Wait 3-5 business days |
| 📨 Reply received | Supplier responded | Review reply, extract key data, schedule call if needed |
| 📞 Call scheduled | Meeting/call arranged | Prepare questions, attend call |
| 💰 Quote received | Pricing/MOQ/lead time provided | Compare with other suppliers, negotiate |
| 🤝 Negotiating | Terms being discussed | Finalize MOQ, pricing, payment terms |
| ❌ Rejected | Not a fit | Update tracker, note why, move to alternative |
| ✅ Approved | Selected for production | Update tracker, initiate next steps |

## Follow-Up Rules

### When to Follow Up

| Scenario | Timing | Template |
|----------|--------|----------|
| No reply to initial outreach | 5 business days | Follow-up #1 (gentle reminder) |
| No reply to follow-up #1 | 3 more business days | Follow-up #2 (final before moving on) |
| Reply received but no quote | After call + 3 days | Quote request reminder |
| Quote received, need clarification | Within 2 days | Clarification questions |
| Quote too high | Within 2 days | Negotiation email |

### Follow-Up Template #1 (Gentle)

```
Subject: Re: [Original Subject]

Hi [Name],

I wanted to follow up on my email from [date] regarding [topic]. 
I know things get busy — just wanted to make sure it didn't slip through.

I'd love to [schedule a call / receive a quote / discuss next steps] if this is still on your radar.

Best regards,
[Founder Name]
[Your Brand]
[Founder Email]
```

### Follow-Up Template #2 (Final)

```
Subject: Re: [Original Subject] — Last follow-up

Hi [Name],

This is my last follow-up on [topic]. I understand if timing isn't right — just wanted to give it one more shot before I move forward with other options.

If you're interested in reconnecting later, I'm always happy to pick up the conversation.

Best regards,
[Founder Name]
[Your Brand]
[Founder Email]
```

## Automation Rules

- **Always ask before sending** — Read the email body to the user before sending. Never auto-send without review.
- **Never auto-negotiate** — Pricing/MOQ negotiation requires the founder's input. Log the quote, present options, wait for approval.
- **Batch operations** — When checking multiple suppliers, run one check, present summary, then update tracker.
- **Log everything** — Every email sent, every reply received, every status change → log to `memory/YYYY-MM-DD.md` and `email_log.jsonl`

## Heartbeat Integration

If `HEARTBEAT.md` requests supplier checking:

```
- Check supplier replies (if last check > 24 hours)
- If replies found: alert user with summary
- If no replies after 5 days: suggest follow-up
```

## Example Usage

### User: "Check supplier replies"

1. Run `python email_automation_oauth.py suppliers`
2. Read output
3. If replies found:
   - Read `[Your Brand]-Supplier-Outreach.md`
   - Update statuses for matching suppliers
   - Extract key data (MOQ, pricing, lead time)
   - Present summary to user
   - Log to `memory/YYYY-MM-DD.md`
4. If no replies:
   - Check which suppliers are past 5 days
   - Suggest follow-up for those

### User: "Send follow-up to [Supplier Name]"

1. Read `[Your Brand]-Supplier-Outreach.md` to confirm status
2. Check if supplier replied
3. If no reply and > 5 days since contact:
   - Draft follow-up using template
   - Show user for review
   - On approval: send via `email_automation_oauth.py send ...`
   - Update tracker: `Status: ✅ Contacted (follow-up sent)`
   - Log action
4. If already followed up twice:
   - Suggest moving to alternative supplier

## Output Format

### Summary Report

```
=== EMAIL AUTOMATION REPORT ===
Date: [date]
Action: [check_replies / send_email / update_tracker]

Results:
- [Supplier]: [Status] — [Key detail]
- [Supplier]: [Status] — [Key detail]

Updates Made:
- [Your Brand]-Supplier-Outreach.md: [what changed]
- memory/2026-MM-DD.md: [logged]

Next Actions:
- [ ] Follow up with [Supplier] on [date]
- [ ] Schedule call with [Supplier]
- [ ] Compare quotes: [Supplier A] vs [Supplier B]
```
