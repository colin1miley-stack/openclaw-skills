---
name: leverage-stack-auditor
description: Audit income streams and work activities through Naval Ravikant's four-layer leverage framework (labor, capital, code, media). Use when someone wants to diagnose over-dependence on low-leverage activities, redesign work around near-zero marginal cost leverage, or identify where their time is leaking relative to output. Triggers on phrases like "leverage audit", "where am I leaking time", "low leverage activities", "Naval leverage framework", "income stream audit", "time vs output", "convert labor to code", "scalable income", or when someone lists their income sources and hours and wants to know which to keep, kill, or convert.
---

# Leverage Stack Auditor

A Naval Ravikant-inspired diagnostic that maps every income stream and work activity across four leverage layers, scores them, and redesigns the lowest-leverage activities into code or media assets with near-zero marginal cost.

## The Four Layers

| Layer | Definition | Examples |
|-------|-----------|----------|
| **Labor** | Time traded directly for money | Consulting, freelancing, hourly billing, 1:1 services |
| **Capital** | Money deployed to make money | Investments, product stock, equipment, lending |
| **Code** | Software that works while you sleep | Apps, tools, automations, SaaS, digital products |
| **Media** | Content that compounds over time | Books, newsletters, courses, videos, podcasts |

**Core rule:** Labor has the lowest leverage (marginal cost ≈ time). Code and Media have the highest (marginal cost ≈ zero).

## Audit Process

### Step 1: Inventory

Extract from the user:
- Every income source
- Hours per week on each
- Monthly revenue from each

If not provided, ask for it.

### Step 2: Map & Score

For each activity, assign:
- **Primary Leverage Type** — Labor, Capital, Code, or Media
- **Leverage Score (0–10)** — Based on how decoupled output is from time input
  - Hourly consulting or billed time = **0** regardless of rate
  - Retainer with defined scope = 2–3
  - Productized service = 4–5
  - Digital product with organic discovery = 7–8
  - Software/asset with recurring revenue and no time input = 9–10
- **Hours/Week**
- **Monthly Revenue**
- **Leverage Risk Flag** — Mark `YES` if the income stream disappears if the user stops working for 60 days

### Step 3: Calculate Leverage Index

**Formula:** `Leverage Index = (Σ Activity Score × Weekly Hours) / Σ Weekly Hours`

Scale: 0 = pure labor, 10 = pure code/media at scale. Most people sit between 1–3.

### Step 4: Identify the Biggest Leverage Leak

Find the activity where:
- Hours are highest
- Leverage score is lowest
- Revenue is capped by time

Describe why it's a trap and what it costs annually (opportunity cost of time not spent on high-leverage assets).

### Step 5: Propose 3 Concrete Upgrades

For each upgrade:
- **Current State** — What the activity is now
- **Convert To** — Code or Media leverage type
- **Specific Change** — Exact asset to build (not conceptual; e.g., "turn into newsletter" is banned)
- **Timeline** — X days, typically 30

Rules for upgrades:
- Must define the **exact deliverable** (e.g., "A 10-email welcome sequence in Kit that sells the [product] on autopilot")
- Must be buildable in 30 days or less
- Must shift primary leverage type from Labor to Code or Media
- Must have a defined output metric (e.g., "automates 5 hours/week of client onboarding")

### Step 6: 30-Day First Move

Pick the highest-impact upgrade and break it into the **exact action to take this week**. No strategy. Just the first task.

## Output Format

```
## Leverage Audit

| Activity | Leverage Type | Hours/Week | Score | Revenue | Risk Flag |
|----------|---------------|------------|-------|---------|-----------|
| [Name] | Labor/Capital/Code/Media | X | 0–10 | £X | YES/NO |
| ... | ... | ... | ... | ... | ... |

## Your Leverage Index

**[X]/10** — [One-sentence interpretation]

## Biggest Leverage Leak

**[Activity Name]**

- **Why it's a trap:** [Explanation]
- **What it costs you:** [Annual opportunity cost calculation]

## 3 Upgrade Moves

| # | Current | Convert To | Specific Change | Timeline |
|---|---------|------------|-----------------|----------|
| 1 | [Activity] | Code/Media | [Exact asset description] | X days |
| 2 | [Activity] | Code/Media | [Exact asset description] | X days |
| 3 | [Activity] | Code/Media | [Exact asset description] | X days |

## 30-Day First Move

**This week:** [Exact action — one sentence, no ambiguity]
```

## Scoring Reference

| Score | Description |
|-------|-------------|
| 0 | Pure hourly labor — stops when you stop |
| 1–2 | Retainer/day-rate — still time-bound |
| 3–4 | Productized service — some systemization |
| 5–6 | Small digital product — one-time sale, some discovery |
| 7–8 | Content/asset with organic reach — compounds |
| 9–10 | Software/recurring asset — works without you |

## Guardrails

- **Never inflate labor scores.** A £500/hour consultant still scores 0 on leverage.
- **Flag every time-dependent stream.** If it dies in 60 days without input, mark it.
- **Upgrades must be assets, not ideas.** "Build a course" is banned. "A 6-module Notion course with Stripe checkout and Kit email automation" is acceptable.
- **Opportunity cost must be calculated.** Show the annual cost of the leak in real numbers.
- **The first move must be this week.** Not next month. Not "plan." The actual first task.
