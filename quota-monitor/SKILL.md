---
name: quota-monitor
description: |
  Automated usage monitoring and pattern learning for AI API costs. Runs periodically via
  cron or heartbeat to analyze spend, detect anomalies, predict quota exhaustion, and append
  insights to MEMORY.md. Alerts at 70% / 85% / 95% thresholds. Learns from historical patterns
  to improve predictions over time.
  
  Triggers on: cron schedule (every 2 hours), heartbeat check, or user asks "quota check",
  "usage status", "how much left", "am I near limit".
domain: operations
subdomain: monitoring
tags: [monitoring, quota, alerting, pattern-learning, cost-prediction]
version: "1.0"
author: vitae10
license: MIT
---

# Quota Monitor

**Purpose:** Watch usage patterns. Alert before limits hit. Learn from history. Surface insights to MEMORY.md.

**State file:** `~/.vitae10/state/quota-history.json`
**Log file:** `~/.vitae10/logs/quota-monitor.log`

---

## When To Run

| Trigger | Frequency |
|---|---|
| Cron job | Every 2 hours during active hours (09:00–21:00 GMT) |
| Heartbeat | Check once per heartbeat if last check >2h ago |
| User request | Immediate ("quota check", "usage status") |
| Pre-heavy-session | Before any session expected to use >50K tokens |

---

## Workflow

### Step 1: Collect Current Usage

Read from these sources:
- `BUDGET-TRACKER.md` — recent entries
- `~/.vitae10/state/rate-limit-state.json` — current limits (from rate-limit-guard)
- Session metadata (if available) — current session usage
- `memory/YYYY-MM-DD.md` — today's activity log

### Step 2: Calculate Metrics

```
Current metrics to compute:
- tokens_today (Kimi)
- claude_spend_today ($)
- perplexity_spend_today ($)
- total_spend_today ($)
- quota_percent (Kimi)
- budget_percent (monthly €70 target)
- calls_per_hour (Claude + Perplexity)
- heavy_sessions_today (count)
```

### Step 3: Check Thresholds

| Threshold | Level | Action |
|---|---|---|
| Kimi quota >70% | 🟡 Warning | Suggest batching, defer heavy tasks |
| Kimi quota >85% | 🟠 Caution | Force light sessions only, route to Claude for critical tasks |
| Kimi quota >95% | 🔴 Critical | Stop all non-essential Kimi work, use Claude/Perplexity only |
| Claude spend >$3/day | 🟡 Warning | Reduce Claude calls, cache more aggressively |
| Claude spend >$5/day | 🟠 Caution | Route synthesis to Kimi, Claude for code only |
| Monthly budget >70% (€49) | 🟡 Warning | Tighten daily targets |
| Monthly budget >90% (€63) | 🟠 Caution | Emergency mode — ask before every Claude call |

### Step 4: Predict Exhaustion

Use simple linear projection:

```
Kimi resets at: 2026-07-30T19:10:00Z
Current time:   2026-07-30T14:00:00Z
Time remaining: 5h 10m = 310 minutes
Current usage:  140,000 tokens (70%)
Burn rate:      140K / 690m (since reset) = ~203 tokens/minute
Projected total: 203 * 310 + 140K = ~203K tokens
Status: 🟢 Will stay under limit

BUT: If heavy session planned (400K), projection = 140K + 400K = 540K > 200K limit
Action: 🟠 Defer heavy session until after 19:10 reset
```

### Step 5: Learn Patterns

Read `~/.vitae10/state/quota-history.json` and update:

```json
{
  "patterns": {
    "day_of_week": {
      "monday": { "avg_kimi_tokens": 180000, "avg_claude_spend": 0.15, "peak_hour": "10:00" },
      "tuesday": { "avg_kimi_tokens": 120000, "avg_claude_spend": 0.08, "peak_hour": "14:00" }
    },
    "time_of_day": {
      "morning_9_12": { "avg_tokens": 150000, "typical_tasks": ["content", "research"] },
      "afternoon_12_18": { "avg_tokens": 80000, "typical_tasks": ["ops", "review"] }
    },
    "session_type": {
      "heavy": { "avg_tokens": 500000, "avg_duration_min": 150, "avg_claude_calls": 12 },
      "medium": { "avg_tokens": 150000, "avg_duration_min": 90, "avg_claude_calls": 5 },
      "light": { "avg_tokens": 30000, "avg_duration_min": 30, "avg_claude_calls": 1 }
    }
  },
  "anomalies": [
    {
      "date": "2026-07-08",
      "tokens": 2900000,
      "reason": "20 parallel subagents — exceptional",
      "lesson": "Parallel >5 subagents = quota exhaustion. Batch instead."
    }
  ],
  "recommendations": [
    "Tuesday mornings are consistently heavy — warn at 60% instead of 70%",
    "Friday afternoons are light — good time for heavy sessions",
    "Claude copy review rarely improves quality vs Kimi alone — skip for simple drafts"
  ]
}
```

**Learning rules:**
- After every session, append session data to history
- Weekly: compute averages per day-of-week, time-of-day
- Monthly: identify anomalies and extract lessons
- Recommendations auto-generate from pattern analysis

### Step 6: Alert + Log

**If any threshold crossed:**

```
🟡 QUOTA WARNING — Kimi at 73%
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Tokens used today: 146,000 / 200,000
Time until reset:  5h 10m (19:10 GMT)
Burn rate:         ~203 tokens/minute
Projection:        🟢 Under limit IF no heavy sessions

Recommendation:    You have ~54K tokens left today. A heavy session (400K) 
                   would exceed. Consider:
                   1. Split into two medium sessions
                   2. Defer until after 19:10 reset
                   3. Route synthesis to Claude ($0.011/task) instead of Kimi

Recent activity:   3 Claude calls ($0.033), 2 Perplexity searches ($0.08)
Today's total:     ~$0.45 (€0.41) — on track for €70 monthly budget
```

**If no thresholds crossed:**

```
🟢 QUOTA OK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Kimi:    45,000 / 200,000 tokens (23%) — healthy
Claude:  $0.15 today / ~$5 daily limit — healthy
Monthly: €23 / €70 (33%) — healthy

No action needed. Heavy session capacity available.
```

### Step 7: Append to MEMORY.md

Every 24 hours (or when significant pattern detected), append insights:

```markdown
## Quota Insights — 2026-07-30

### Pattern Detection
- Tuesday burn rate 2.3x Friday average
- Heavy sessions (>400K tokens) always follow competitor research
- Claude copy review on simple drafts: 0/5 showed improvement → skip in future

### Cost Optimization
- Batching Perplexity queries: saved ~$0.15 today (3 calls → 1 call)
- Context optimizer: reduced 5-file read to 1 grep → saved ~8K tokens

### Anomaly
- Spike at 14:00 — 180K tokens in 30 min (normal: 40K/30min)
- Cause: Spawned 6 parallel subagents for packaging copy
- Lesson: Limit parallel subagents to 3 max

### Recommendation for Tomorrow
- Morning: Light session (planning, email)
- Afternoon: Medium session (content batch)
- Evening: Defer heavy work until after 19:10 reset
```

---

## Cron Configuration

Set up via OpenClaw cron:

```json
{
  "name": "Quota Monitor — Every 2 Hours",
  "schedule": { "kind": "cron", "expr": "0 9,11,13,15,17,19,21 * * *", "tz": "Europe/Dublin" },
  "payload": { "kind": "agentTurn", "message": "Run quota-monitor skill. Check all thresholds, predict exhaustion, update patterns, alert if needed." },
  "sessionTarget": "isolated",
  "delivery": { "mode": "announce" }
}
```

Also set a pre-heavy-session check:

```json
{
  "name": "Quota Pre-Flight",
  "schedule": { "kind": "every", "everyMs": 1800000, "anchorMs": 0 },
  "payload": { "kind": "systemEvent", "text": "QUOTA PRE-FLIGHT: Before any session >50K tokens, run quota-monitor check." },
  "sessionTarget": "main"
}
```

---

## State File: quota-history.json

```json
{
  "version": "1.0",
  "created_at": "2026-07-30T12:00:00Z",
  "last_updated": "2026-07-30T14:00:00Z",
  "daily_records": [
    {
      "date": "2026-07-30",
      "kimi_tokens": 146000,
      "kimi_quota_percent": 73,
      "claude_calls": 3,
      "claude_spend_usd": 0.033,
      "perplexity_calls": 2,
      "perplexity_spend_usd": 0.08,
      "total_spend_eur": 0.41,
      "sessions": ["content-batch", "supplier-email"],
      "peak_hour": "10:00",
      "anomaly": false
    }
  ],
  "weekly_averages": {
    "monday": { "kimi_tokens": 180000, "claude_spend": 0.15 },
    "tuesday": { "kimi_tokens": 120000, "claude_spend": 0.08 }
  },
  "patterns": {
    "heavy_session_triggers": ["competitor research", "packaging design", "regulatory review"],
    "cheap_session_triggers": ["email drafting", "calendar check", "social post"],
    "claude_value_moments": ["code review", "complex synthesis", "council decisions"]
  },
  "lessons_learned": [
    { "date": "2026-07-08", "lesson": "20 parallel subagents = 2.9M tokens. Batch to 5 max." },
    { "date": "2026-07-28", "lesson": "Claude copy review rarely improves simple drafts. Reserve for complex regulatory/policy text." }
  ]
}
```

---

## Integration with rate-limit-guard

The quota-monitor reads `rate-limit-state.json` to get real-time remaining quotas. It writes predictions back that the rate-limit-guard uses for pre-flight checks.

```
rate-limit-guard ← reads quota predictions from quota-monitor
quota-monitor ← reads actual limits from rate-limit-guard state
```

This creates a feedback loop: guard enforces current limits, monitor learns patterns and predicts future limits, guard adjusts thresholds based on predictions.

---

## Pattern A + D Implementation

This skill implements the recommended architecture:

- **Pattern A (State File):** `quota-history.json` accumulates usage data. Each run reads history, updates patterns, writes back.
- **Pattern D (Cron-Driven Feedback):** Cron runs every 2 hours. It analyzes history, detects patterns, and appends insights to `MEMORY.md`. Monthly: human reviews accumulated insights and decides if any SKILL.md needs updating.

The skill itself doesn't change. The *system's memory* evolves.

---

## Logging Format

Append to `~/.vitae10/logs/quota-monitor.log`:

```
[2026-07-30T14:00:00Z] CHECK kim=73% claude=$0.15/5 status=WARNING
[2026-07-30T14:00:00Z] PREDICT reset_in=310m burn=203tok/min will_exceed=false
[2026-07-30T14:00:00Z] PATTERN tuesday_morning=heavy (1.8x avg)
[2026-07-30T14:00:00Z] ALERT 🟡 Kimi at 73% — suggest batching
[2026-07-30T14:00:00Z] MEMORY appended: quota-insights-2026-07-30
```

---

*Version: 1.0*
*For: Vitae10 multi-LLM orchestration*
*Complements: rate-limit-guard, budget-gate, token-usage-optimizer, automation-loop*
