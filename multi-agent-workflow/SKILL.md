# Multi-Agent Workflow

**Version:** 1.0  
**Date:** 2026-08-03  
**Author:** Colin Miley / AI Revenue Systems  
**License:** CC BY-NC-SA 4.0  

---

## Purpose

A structured pipeline for producing public-facing content (LinkedIn posts, newsletters, website copy, proposals) that ensures accuracy, brand consistency, and zero errors — without requiring Colin to review every word.

**Trigger:** Any content that leaves the workspace (LinkedIn, newsletter, website, client proposals, GitHub)

---

## The Pipeline

```
┌─────────────────────────────────────────────────────────────────────┐
│  INPUT: Raw draft (Claude/Kimi/Colin voice memo)                    │
└─────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────┐
│  STEP 1: CONTENT-AUTHENTICITY-ENFORCER                              │
│  Agent: Fact-Checker                                                │
│  Checks:                                                            │
│    • Every number against COLIN-FACT-BASE.md                        │
│    • Every date against timeline                                    │
│    • Every name spelling verified                                   │
│    • Every story confirmed with Colin or fact base                  │
│    • No aspirational claims presented as facts                      │
│  Output: Flagged claims + corrected draft                           │
│  Blocker: 🔴 CRITICAL = stop pipeline, alert Colin                  │
└─────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────┐
│  STEP 2: BRAND-GUARDIAN                                             │
│  Agent: Brand Monitor                                               │
│  Checks:                                                            │
│    • Descriptor: "Forward Deployed Engineer for B2B Sales Teams"    │
│    • Business name: "AI Revenue Systems"                            │
│    • Currency: € (euro) — no £, no $                                │
│    • No Vitae10 mentions                                            │
│    • No supplement references                                       │
│    • No Vodafone/Eircom named in public content                     │
│    • Tone: sharp, practical, no corporate speak                     │
│    • CTA present and correct (calendly.com/colin1miley)             │
│  Output: Brand compliance report + fixes applied                    │
│  Blocker: 🟡 WARNING = fix and re-run                               │
└─────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────┐
│  STEP 3: CURRENCY-CHECKER                                           │
│  Agent: Financial Auditor                                           │
│  Checks:                                                            │
│    • All prices in €                                                │
│    • Audit price: €2,150                                            │
│    • Implementation range: €3,000–€8,000                            │
│    • Retainer: €1,500/mo                                            │
│    • No £ or $ unless explicitly contextual (e.g., "UK competitors  │
│      priced in £")                                                  │
│  Output: Currency audit report                                      │
│  Blocker: 🔴 CRITICAL = stop pipeline, fix immediately              │
└─────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────┐
│  STEP 4: COPY-EDITOR                                                │
│  Agent: Style Editor                                                │
│  Checks:                                                            │
│    • Active voice over passive                                      │
│    • No filler words ("great question", "I'd be happy to")          │
│    • Concise — cut 20% without losing meaning                       │
│    • LinkedIn: scannable, short paragraphs, punchy hooks            │
│    • Website: conversion-focused, clear CTAs                        │
│    • No em-dashes (use colons or periods)                           │
│  Output: Polished draft                                             │
│  Blocker: None — editorial, not factual                             │
└─────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────┐
│  STEP 5: COLIN (HUMAN)                                              │
│  Agent: Decision Maker                                              │
│  Action: Review flagged items, approve or reject                    │
│  Time budget: 5 minutes max for routine content                     │
│  Time budget: 15 minutes for major pieces (landing page, newsletter)│
│  Output: APPROVED / REJECTED / REVISE                               │
└─────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────┐
│  OUTPUT: Publish-ready content                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Agent Definitions

### 1. Content-Authenticity-Enforcer

**Role:** Pre-publish credibility guard  
**Input:** Draft content + COLIN-FACT-BASE.md  
**Output:** Verified draft + flagged issues  
**Critical rule:** If any claim cannot be verified, mark 🔴 and stop pipeline.

**Checklist:**
- [ ] All numbers verified against fact base
- [ ] All dates match timeline
- [ ] All names spelled correctly
- [ ] No aspirational claims presented as facts
- [ ] No invented stories or details
- [ ] "To my knowledge" added where appropriate

### 2. Brand-Guardian

**Role:** Brand consistency enforcer  
**Input:** Verified draft + brand guidelines  
**Output:** Brand-compliant draft  
**Critical rule:** Any deviation from brand = 🟡, must fix.

**Brand Guidelines (AI Revenue Systems):**
| Element | Correct | Incorrect |
|---------|---------|-----------|
| Descriptor | Forward Deployed Engineer for B2B Sales Teams | AI consultant, sales automation expert |
| Business name | AI Revenue Systems | Vitae10, anything else |
| Email | colin@colinmiley.com | colin@vitae10.com |
| Calendly | calendly.com/colin1miley | Any other link |
| LinkedIn | linkedin.com/in/colin-miley | Any other URL |
| Currency | €2,150 | £1,850, $2,000 |
| Tone | Sharp, practical, no corporate speak | "I'd be happy to", "Great question" |

### 3. Currency-Checker

**Role:** Financial accuracy  
**Input:** Brand-compliant draft  
**Output:** Currency-verified draft  
**Critical rule:** Any £ or $ in non-contextual use = 🔴.

**Price Reference:**
| Service | Price |
|---------|-------|
| AI Sales Process Audit | €2,150 |
| Done-With-You Implementation | €3,000–€8,000 |
| Ongoing FDE Retainer | €1,500/mo |
| First €10K month target | €10,000 |

### 4. Copy-Editor

**Role:** Style and clarity  
**Input:** Currency-verified draft  
**Output:** Polished draft  
**Critical rule:** None — this is editorial, not factual.

**Style Rules:**
- Active voice: "I built" not "A system was built"
- No filler: Delete "I think", "In my opinion", "To be honest"
- LinkedIn format: 1-sentence paragraphs, line breaks, emojis sparingly
- Website format: Clear H2s, bullet lists, single CTA per section
- No em-dashes: Use colons, periods, or parentheses

### 5. Colin (Human)

**Role:** Final approval  
**Input:** Polished draft + flagged issues summary  
**Output:** APPROVE / REJECT / REVISE  
**Time budget:** 5 min (routine), 15 min (major)

**Decision criteria:**
- Do I stand behind every claim?
- Would I say this to a prospect?
- Is the tone authentically me?
- Is the CTA clear and correct?

---

## Workflow Templates

### Template A: LinkedIn Post (Routine)

```
1. Colin voice memo → Kimi draft (2 min)
2. Content-Authenticity: Verify numbers/dates (1 min)
3. Brand-Guardian: Check descriptor, currency, tone (1 min)
4. Currency-Checker: All € (30 sec)
5. Copy-Editor: LinkedIn formatting (1 min)
6. Colin review (5 min)
7. Publish

Total: ~10 minutes
```

### Template B: Newsletter Issue (Major)

```
1. Colin voice memo → Kimi draft (20 min)
2. Content-Authenticity: Full fact-check (10 min)
3. Brand-Guardian: Brand compliance (5 min)
4. Currency-Checker: All € (2 min)
5. Copy-Editor: Newsletter formatting (10 min)
6. Colin review (15 min)
7. Schedule/publish

Total: ~60 minutes
```

### Template C: Website Copy (Critical)

```
1. Skill reading (mandatory: design-taste-frontend, build-orchestrator)
2. Claude API call for first draft (5 min)
3. Content-Authenticity: Verify every claim (15 min)
4. Brand-Guardian: Full brand audit (10 min)
5. Currency-Checker: All € (5 min)
6. Copy-Editor: Web formatting, CTAs (10 min)
7. Colin review (15 min)
8. BUILD command required
9. Deploy

Total: ~60 minutes + BUILD
```

---

## Integration with Build-Orkestrator

The multi-agent workflow is **Step 0.3** in the build-orchestrator protocol:

```
Step 0.1: Load context
Step 0.2: Read all relevant skills
Step 0.3: Activate multi-agent workflow ← THIS SKILL
Step 1-8: Build sequence
Step 9: Pre-flight check (multi-agent pipeline runs again)
```

---

## Error Log & Lessons

| Date | Error | Cause | Fix |
|------|-------|-------|-----|
| 2026-07-30 | LinkedIn post: "3 SDRs", "team management" | Fabricated details | Content-authenticity-enforcer created |
| 2026-08-02 | Site copy: "15 hours saved" (invented) | Aspirational truth | Fact-base verification added |
| 2026-08-02 | Site shows £ pricing | Currency mismatch | Currency-checker agent added |
| 2026-08-03 | GitHub profile: old branding | No brand audit step | Brand-guardian agent added |

---

## Usage

To run the multi-agent workflow on any content:

```bash
# The agent self-activates when:
# - Content mentions Colin's name, background, or results
# - Content includes pricing
# - Content will be published publicly
# - Content is part of a BUILD sequence

# Manual activation:
"Run multi-agent workflow on [file/content]"
```

---

*Skill created: 2026-08-03*  
*Source: Post-incident reviews, build-orchestrator integration, brand consistency failures*
