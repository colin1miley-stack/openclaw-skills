---
name: openreply-icm
description: Instagram comment-to-DM automation engine, ICM-converted from diwenne/openreply. Adaptable pattern for social platform messaging automation. Use when building keyword-triggered DM responses, lead capture via social comments, or automated conversation flows.
---

# OpenReply — ICM Converted

**Source:** `diwenne/openreply` (MIT License)  
**Converted:** 2026-09-05  
**Purpose:** Instagram comment-to-DM automation + cross-platform messaging patterns  

---

## ICM Layer Mapping

| Layer | File | Usage |
|-------|------|-------|
| **Layer 0** | `SOUL.md`, `AGENTS.md` | Security boundaries, skill vetting |
| **Layer 1** | Root `CONTEXT.md` | Route to Signal distribution stages |
| **Layer 2** | This SKILL.md | Task contract for OpenReply deployment/adaptation |
| **Layer 3** | `skills/openreply-icm/`, `references/` | Architecture patterns, webhook handling |
| **Layer 4** | `stages/NN-openreply/output/` | Deployment artifacts, learnings |

---

## What OpenReply Does

```
Instagram Post/Reel/Story
    ↓
User comments "LINK" (or any configured keyword)
    ↓
Meta webhook → OpenReply server
    ↓
Keyword match → Queue DM job
    ↓
BullMQ worker sends private reply (with optional follow-gate)
    ↓
Link delivered via DM button or inline text
    ↓
Tracked link click → Analytics
```

**Core Features:**
- Keyword-to-DM matching (whole-word or partial)
- Optional public reply on comment
- Opening DM with button tap reveal
- Follow-gating (require follow before sending link)
- Tracked links with click analytics
- Rate limiting (Meta's 750/hour cap)
- Multi-account support
- DM conversation inbox

---

## Architecture Patterns (Extractable)

### Pattern 1: Webhook → Event Parser → Queue → Worker

```
Webhook POST /api/webhooks/instagram
    ↓
Signature verification (HMAC-SHA256, timing-safe)
    ↓
Event parser (comment / message / postback / read)
    ↓
Queue job (BullMQ on Redis)
    ↓
Worker processes with retry + backoff
    ↓
API call to platform → Log result
```

**Applicable to:** Any platform with webhooks (Meta, Twitter/X, LinkedIn, TikTok)

### Pattern 2: Keyword Matcher with Unicode Support

```
Raw comment text
    ↓
Normalize Arabic script (Persian/Arabic folding)
    ↓
Strip emojis and special chars
    ↓
Fold Latin diacritics (PREÇO → preco)
    ↓
Case-insensitive match
    ↓
Whole-word or partial regex
```

**Applicable to:** Any platform with user-generated text input

### Pattern 3: Conversation State Machine

```
States: INITIAL → OPENING_DM_SENT → FOLLOW_CHECK → REVEAL_SENT → FOLLOW_UP
    ↓
Transitions triggered by: webhook events, button taps, time delays
    ↓
Deduplication: One private reply per comment (Meta rule)
    ↓
Idempotency: Retry-safe, no double-send
```

**Applicable to:** Any multi-step conversation flow

### Pattern 4: Rate Limiting with Queuing

```
Reserve DM slot (per-account counter in Redis)
    ↓
If allowed: Send immediately
    ↓
If rate-limited: Requeue with delay (5min → 15min → 45min backoff)
    ↓
If plan limit: Skip with logged reason
```

**Applicable to:** Any API with rate limits (all social platforms)

---

## Cross-Platform Messaging Application

### Instagram (OpenReply Native)

| Feature | OpenReply Support | Meta API |
|---------|-------------------|----------|
| Comment-to-DM | ✅ Native | Instagram Graph API |
| Story reply trigger | ✅ Native | `messages` webhook |
| Follow gate | ✅ Built-in | `is_user_follow_business` |
| Button templates | ✅ 3 buttons max | Messenger API |
| Rate limit | 750/hour | Hard limit |

### LinkedIn (Adaptation Needed)

| Feature | Feasibility | API |
|---------|-------------|-----|
| Comment-to-message | 🟡 Possible | LinkedIn Messaging API (restricted) |
| Post engagement trigger | ❌ No webhook | Polling only |
| Auto-DM on connection | ⚠️ Risky | Against ToS if automated |
| **Verdict:** | Not a good fit | LinkedIn doesn't support comment-to-DM |

### Twitter/X (Adaptation Needed)

| Feature | Feasibility | API |
|---------|-------------|-----|
| Mention-to-DM | 🟡 Possible | Twitter API v2 (paid tier $100/mo) |
| Reply-to-DM | ❌ No | No private reply from mention |
| Auto-DM on follow | ⚠️ Risky | Against ToS |
| **Verdict:** | Limited value | No native comment-to-DM |

### TikTok (Adaptation Needed)

| Feature | Feasibility | API |
|---------|-------------|-----|
| Comment-to-message | ❌ No API | TikTok for Business API (enterprise only) |
| **Verdict:** | Not possible | No public DM API |

### Facebook (Adaptation Needed)

| Feature | Feasibility | API |
|---------|-------------|-----|
| Comment-to-message | ✅ Direct adaptation | Messenger API (same as Instagram) |
| Post reaction trigger | 🟡 Possible | Webhooks for reactions |
| **Verdict:** | ✅ Good fit | Same Meta infrastructure |

### Pinterest (Adaptation Needed)

| Feature | Feasibility | API |
|---------|-------------|-----|
| Pin comment-to-message | ❌ No | Pinterest API has no messaging |
| **Verdict:** | Not possible | No messaging API |

---

## Where OpenReply Fits in Signal

```
Signal Content Engine
    ├── Generation Layer (text, image, video, voice)
    ├── Distribution Layer
    │   ├── LinkedIn → Manual / API post
    │   ├── Twitter/X → Manual / API post
    │   ├── Instagram → OpenReply comment-to-DM
    │   ├── Facebook → OpenReply adaptation
    │   └── TikTok → Manual upload
    └── Attribution Layer (Twenty CRM)
```

**OpenReply is a DISTRIBUTION ACCELERATOR, not a content generator.**

It captures leads who engage with Signal-generated content on Instagram/Facebook and routes them into the CRM funnel.

---

## ICM Stage Contract for OpenReply Deployment

```json
{
  "stageName": "openreply-deploy",
  "branchName": "feat/openreply-instagram",
  "qualityGate": "quality-gate.md",
  "subTasks": [
    {
      "id": "or-001",
      "title": "Create Meta Developer App",
      "agent": "builder",
      "acceptance": ["App ID created", "Instagram Basic Display + Graph API products added"],
      "estimatedTokens": 5000
    },
    {
      "id": "or-002",
      "title": "Deploy web app to Vercel",
      "agent": "builder",
      "acceptance": ["App builds without errors", "Domain configured", "HTTPS enforced"],
      "estimatedTokens": 8000
    },
    {
      "id": "or-003",
      "title": "Deploy worker + Postgres + Redis to Railway",
      "agent": "builder",
      "acceptance": ["Worker process running", "Database migrated", "Redis connected"],
      "estimatedTokens": 8000
    },
    {
      "id": "or-004",
      "title": "Configure Meta webhooks",
      "agent": "builder",
      "acceptance": ["Webhook verified", "Subscribed to comments + messages fields", "Domain whitelisted"],
      "estimatedTokens": 5000
    },
    {
      "id": "or-005",
      "title": "Create first campaign (Revenue Systems lead magnet)",
      "agent": "builder",
      "acceptance": ["Keyword: AUDIT", "DM message with tracked link", "Follow gate enabled"],
      "estimatedTokens": 3000
    }
  ]
}
```

---

## Quality Gate

- [ ] Meta app approved for Instagram Basic Display + Graph API
- [ ] Webhook signature verification working
- [ ] DM sends successfully (test with own comment)
- [ ] Tracked link redirects correctly
- [ ] Rate limiting under 750/hour
- [ ] No self-comment triggering
- [ ] Follow gate works (test with non-follower account)
- [ ] Campaign logs visible in dashboard

---

## Anti-Patterns to Avoid

| Anti-Pattern | Why It Fails |
|-------------|--------------|
| Personal Instagram account | Must be Business or Creator account |
| Skip webhook verification | Meta requires HMAC-SHA256 signature check |
| Send DMs without rate limiting | Account will be blocked by Meta |
| No follow gate on affiliate links | Against Meta ToS for commercial content |
| Hardcoded links without tracking | No attribution data |

---

## Cost Estimate

| Component | Cost/Month |
|-----------|-----------|
| Vercel (web app) | $0 (free tier) |
| Railway (worker + Postgres + Redis) | ~$15-25 |
| Meta API | Free (rate limited) |
| **Total** | **~$15-25/month** |

---

## Files to Review in Source

| File | Purpose |
|------|---------|
| `lib/meta/webhook.ts` | Webhook parsing + signature verification |
| `lib/queue/dm-worker.ts` | Job processing + DM sending |
| `lib/utils/keyword-matcher.ts` | Unicode-aware keyword matching |
| `lib/utils/rate-limiter.ts` | Rate limit enforcement |
| `lib/tracking/message.ts` | Tracked link generation |

---

*ICM-converted from diwenne/openreply. See original repo for full implementation.*
