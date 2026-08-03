---
name: rate-limit-guard
description: |
  Client-side throttling and rate-limit protection for Claude API, Perplexity API, and Kimi
  gateway calls. Maintains a local token-bucket state file to prevent 429s, enforce per-request
  budgets, and queue or degrade gracefully when limits approach.
  
  Triggers on: Before any Claude spawn, Perplexity search, or expensive external API call.
  Also triggers on: "rate limit", "throttling", "429", "too many requests", "slow down".
domain: operations
subdomain: api-management
tags: [rate-limiting, throttling, circuit-breaker, api-guard, cost-control]
version: "1.0"
author: vitae10
license: MIT
---

# Rate Limit Guard

**Purpose:** Prevent rate-limit failures before they happen. Enforce per-request budgets. Degrade gracefully when limits approach.

**State file:** `~/.vitae10/state/rate-limit-state.json`

---

## When To Activate

Activate BEFORE these operations:
- `sessions_spawn` with `runtime="acp"` (Claude API)
- `web_search` via Perplexity (Perplexity API)
- Any multi-agent fan-out (>2 subagents)
- Any operation expected to exceed $0.25
- User explicitly mentions rate limits or throttling

---

## Workflow

### Step 1: Read State File

```powershell
# Read current state
$state = Get-Content ~/.vitae10/state/rate-limit-state.json | ConvertFrom-Json
```

**If state file missing:** Create from template (see State File Schema below).

### Step 2: Check Limits

| Check | Action if Failing |
|---|---|
| `claude.tokens_remaining < estimated_tokens` | Queue request OR route to Kimi |
| `claude.requests_remaining < 1` | Wait until `resets_at` OR route to Kimi |
| `perplexity.requests_remaining < 1` | Route research to Kimi web_search |
| `kimi.quota_percent > 85` | Defer heavy session OR batch aggressively |
| `any.circuit_breaker == "OPEN"` | Force fallback model for 15 min |

### Step 3: Estimate Request Cost

Before proceeding, estimate:

```
Input tokens:  __________ (prompt length / 4)
Output tokens: __________ (expected response length / 4)
Model:         __________ (claude-sonnet-5 | perplexity-sonar | kimi-k2p6)
Est. cost:     __________ ($)
```

**Rule:** If estimated cost > per-request cap, shorten prompt or split into smaller requests.

| Model | Per-Request Cap |
|---|---|
| Claude Sonnet 5 | $0.25 |
| Perplexity Sonar | $0.10 |
| Kimi (any) | No cap (subscription) |

### Step 4: Deduct Budget (Pre-Flight)

If all checks pass, deduct estimated cost from state BEFORE making the call:

```json
{
  "claude": {
    "tokens_remaining": 49800,
    "requests_remaining": 49,
    "est_spend_remaining": 4.50,
    "resets_at": "2026-07-30T14:00:00Z",
    "last_updated": "2026-07-30T12:05:00Z"
  }
}
```

**Important:** This is optimistic accounting. Actual spend may differ. Reconcile after the call.

### Step 5: Execute Call + Reconcile

After the call completes:
1. Log actual tokens used (from response metadata if available, or estimate)
2. Adjust state: `tokens_remaining += (estimated - actual)`
3. Record in `BUDGET-TRACKER.md`
4. Update `last_updated`

### Step 6: Handle Failure

If the call returns 429 (rate limit) or 5xx:

```
[FAILURE] Source: claude
[FAILURE] Error: 429 Too Many Requests
[FAILURE] Retry-After: 60s
[FAILURE] Action: Exponential backoff + jitter
```

**Retry policy:**
- Attempt 1: Wait `base_delay * 2^0 + rand(0,1s)` = ~1s
- Attempt 2: Wait `base_delay * 2^1 + rand(0,1s)` = ~2s
- Attempt 3: Wait `base_delay * 2^2 + rand(0,1s)` = ~4s
- Attempt 4+: Log ERROR, escalate to user, OPEN circuit breaker

**Circuit breaker rule:**
- 3 failures in 10 minutes → OPEN circuit for 15 minutes
- During OPEN: All Claude calls route to Kimi or queue
- After 15 min: HALF-OPEN (test 1 call)
- If test succeeds: CLOSE circuit

---

## State File Schema

```json
{
  "version": "1.0",
  "last_updated": "2026-07-30T12:00:00Z",
  "providers": {
    "claude": {
      "tier": "low",
      "requests_per_minute": 50,
      "tokens_per_minute": 50000,
      "requests_remaining": 50,
      "tokens_remaining": 50000,
      "est_spend_remaining": 5.00,
      "resets_at": "2026-07-30T13:00:00Z",
      "circuit_breaker": "CLOSED",
      "circuit_opened_at": null,
      "consecutive_failures": 0,
      "last_failure_at": null
    },
    "perplexity": {
      "requests_per_day": 100,
      "requests_remaining": 100,
      "resets_at": "2026-07-31T00:00:00Z",
      "circuit_breaker": "CLOSED",
      "consecutive_failures": 0,
      "last_failure_at": null
    },
    "kimi": {
      "daily_quota_tokens": 200000,
      "quota_used_today": 0,
      "quota_percent": 0,
      "resets_at": "2026-07-30T19:10:00Z",
      "circuit_breaker": "CLOSED"
    }
  },
  "session": {
    "session_id": "main-2026-07-30",
    "started_at": "2026-07-30T09:00:00Z",
    "claude_calls_this_session": 0,
    "perplexity_calls_this_session": 0,
    "kimi_calls_this_session": 0
  }
}
```

**Reset rules:**
- Claude: Hourly (or per Anthropic's actual reset schedule)
- Perplexity: Daily at 00:00 UTC
- Kimi: Daily at 19:10 GMT (as established)

---

## Per-Request Budget Enforcement

Before any expensive operation, fill this checklist:

```
□ Task: ________________________________
□ Model: _______________________________
□ Est. input tokens: ___________________
□ Est. output tokens: __________________
□ Est. cost: $ _________________________
□ Per-request cap: $ ___________________
□ Within cap? YES / NO
□ If NO: Shorten prompt? Split request? Use cheaper model?
□ Route to: Kimi / Perplexity / Claude
```

**Hard rule:** Never exceed per-request cap without explicit user approval.

---

## Graceful Degradation

When limits approach, degrade in this order:

1. **Reduce context:** Shorter prompts, fewer examples
2. **Reduce quality:** Smaller model (Claude Haiku vs Sonnet)
3. **Defer:** Queue for after reset
4. **Fallback:** Route to Kimi entirely
5. **Skip:** Ask user if task is worth the cost

---

## Integration with Other Skills

| Skill | Integration |
|---|---|
| `budget-gate` | Rate-limit-gate runs AFTER budget-gate. Budget asks "should we spend?" Rate-limit asks "can we spend?" |
| `token-usage-optimizer` | Reads rate-limit state to inform batching decisions |
| `automation-loop` | Every loop gets rate-limit wrapper; loop kills if circuit opens |
| `quota-monitor` | Quota-monitor writes usage history; rate-limit-guard reads it for predictions |

---

## Logging

Append to `~/.vitae10/logs/rate-limit.log`:

```
[2026-07-30T12:05:00Z] PRE-FLIGHT claude est=4500tokens actual=4100tokens status=OK
[2026-07-30T12:08:00Z] FAILURE claude 429 retry_after=60s attempt=1
[2026-07-30T12:08:02Z] RETRY claude attempt=2 delay=2.3s
[2026-07-30T12:08:05Z] SUCCESS claude attempt=2
[2026-07-30T12:15:00Z] CIRCUIT-OPEN claude 3 failures in 10min
```

---

## Quick Reference

```
Before Claude call:
  1. Read state file
  2. Check tokens_remaining > est_input + est_output
  3. Check circuit_breaker == "CLOSED"
  4. Check est_cost < per-request cap ($0.25)
  5. Deduct estimated budget
  6. Execute call
  7. Reconcile actual spend
  8. On failure: retry with backoff, track consecutive failures
```

---

*Version: 1.0*
*For: Vitae10 multi-LLM orchestration*
*Complements: budget-gate, token-usage-optimizer, automation-loop*
