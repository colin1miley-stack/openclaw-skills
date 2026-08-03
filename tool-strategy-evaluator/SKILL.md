<!--
License: CC BY-NC-SA 4.0 (https://creativecommons.org/licenses/by-nc-sa/4.0/)
Copyright (c) 2026 Colin Miley � AI Revenue Systems
Commercial use requires permission: colin@colinmiley.com
-->
---
name: tool-strategy-evaluator
description: |
  Evaluate any new tool, skill, API, or technology against Colin's active strategies
  (Vitae10, AI Revenue Systems, content operations) and existing tool stack.
  Produces a clear verdict: ADOPT NOW / ROADMAP / REJECT / WATCH.
  Documents the decision in the right place (TOOLS.md, MEMORY.md, or skill notes).
  
  Triggers on: "evaluate this tool", "should we use", "new tool", "new skill",
  "worth adopting", "compare against", "add to stack", "tool evaluation",
  when user shares a URL to a product, or when research on a new tool is presented.
domain: operations
subdomain: strategy
tags: [tool-evaluation, stack-audit, strategy-alignment, adoption-decision, tech-review]
version: "1.0"
author: vitae10
license: MIT
---

# Tool Strategy Evaluator

**Purpose:** When a new tool, skill, API, or technology is surfaced, evaluate it against every active strategy and the existing stack. No blind adoption. No FOMO-driven decisions.

**Golden rule:** Every tool must earn its place. If it doesn't clearly solve a gap or outperform an incumbent, it gets rejected or roadmapped.

---

## When To Activate

Activate when:
- User shares a URL to a product, tool, or platform
- User asks "should we use X?" or "is X worth it?"
- Research on a new tool is presented (e.g., Fish Audio, new AI model, new SaaS)
- Someone recommends a tool in any context
- A new skill is discovered or shared
- A competitor or peer is using a tool you don't have

---

## Pre-Flight: Read Context (MANDATORY)

Before evaluating, you MUST read these files to understand current state:

1. **`TOOLS.md`** — Current tool stack, API keys, subscriptions, capabilities
2. **`ROUTING.md`** — LLM routing, guard skills, cost hierarchy
3. **`CLAUDE.md`** (first 200 lines) — Skills inventory, what's already installed
4. **`USER.md`** (first 200 lines) — Active strategies, goals, budgets
5. **`MEMORY.md`** (first 50 lines) — Recent decisions, lessons learned
6. **`skills/` directory** — List installed skills to check for overlap

**If any file is missing, note it and proceed with available context.**

---

## The 5-Framework Evaluation

Run ALL five frameworks. No skipping. Score 0–10 each.

---

### Framework 1: Strategic Fit

**Question:** Does this tool directly advance an active strategy?

| Strategy | Check |
|---|---|
| **Vitae10 Launch** | Does it help manufacturing, marketing, sales, or operations? |
| **AI Revenue Systems** | Does it help content creation, client work, or automation? |
| **Content Operations** | Does it help publishing, distribution, or creation at scale? |
| **Personal Productivity** | Does it save Colin meaningful time or reduce cognitive load? |

**Score 0–10:**
- 0–2: No clear strategic fit. Solves a problem we don't have.
- 3–5: Indirect fit. Nice to have, not mission-critical.
- 6–7: Direct fit for one active strategy.
- 8–10: Direct fit for multiple strategies or a critical blocker.

**Verdict threshold:** <5 = likely REJECT unless exceptional standalone value.

---

### Framework 2: Stack Overlap

**Question:** Does an existing tool already solve this problem?

**Check against:**
- TOOLS.md (every listed tool)
- ROUTING.md (LLM routing — is this a model/LLM tool?)
- CLAUDE.md skills inventory (every installed skill)
- Known free alternatives (e.g., Kimi vs Claude vs Perplexity)

**Overlap analysis:**
```
Existing tool: [name]
What it does: [description]
Gap vs new tool: [what's missing]
Switch cost: [migration effort, retraining, data loss risk]
```

**Score 0–10 (lower = more overlap = harder to justify):**
- 0–2: Exact duplicate of existing tool. No adoption unless significantly better.
- 3–5: Partial overlap. New tool has meaningful differentiator.
- 6–7: Complementary. Solves adjacent problem existing tools don't touch.
- 8–10: Green field. No existing tool comes close.

**Verdict threshold:** <3 with high switch cost = REJECT. 3–5 requires clear differentiator.

---

### Framework 3: Cost-Benefit

**Question:** Is the ROI clear and positive within 30 days?

**Cost components:**
- Monthly subscription: $____
- Setup time: ___ hours
- Learning curve: ___ hours
- Integration effort: ___ hours
- Opportunity cost of not using existing tool: $____

**Benefit components:**
- Time saved per week: ___ hours
- Revenue enabled: $____/month
- Quality improvement: [describe]
- Risk reduction: [describe]

**Score 0–10:**
- 0–2: Costs exceed benefits. Negative ROI.
- 3–5: Break-even or marginal. Need high conviction.
- 6–7: Clear positive ROI within 30–60 days.
- 8–10: Immediate, obvious ROI. Pays for itself in first week.

**Budget check:** Does this fit within the €70/month AI/tools budget? If not, what gets cut?

---

### Framework 4: Time to Value

**Question:** How fast can this tool produce meaningful results?

| Timeline | Score | Interpretation |
|---|---|---|
| Same day | 10 | Install → use → value in <4 hours |
| This week | 7–9 | Requires setup, config, learning |
| This month | 4–6 | Requires integration, workflow changes |
| 2–3 months | 2–3 | Requires training, team adoption, process change |
| 3+ months | 0–1 | Strategic bet, not tactical tool |

**Verdict threshold:** <4 with no strategic urgency = ROADMAP (not adopt now).

---

### Framework 5: Risk & Lock-in

**Question:** What's the downside if this tool fails, raises prices, or shuts down?

**Risk factors:**
- **Data portability:** Can you export your data? (YES/NO/UNKNOWN)
- **API dependency:** Is it embedded in workflows? (YES/NO)
- **Price volatility:** Has pricing changed recently? (YES/NO)
- **Vendor stability:** Startup? VC-backed? Bootstrapped? (assess)
- **Switch cost:** How hard to replace if needed? (HIGH/MEDIUM/LOW)
- **Free tier dependency:** Are you building on a free tier that could change? (YES/NO)

**Score 0–10 (higher = lower risk):**
- 0–2: High lock-in, unstable vendor, data trapped.
- 3–5: Moderate risk. Acceptable for tactical tools.
- 6–7: Low risk. Good data portability, stable vendor.
- 8–10: Minimal risk. Open source, self-hostable, or trivial to replace.

---

### Framework 6: Strategic Evolution ⭐ (NEW)

**Question:** Does this tool create a strategic opportunity that didn't exist before?

This is the most important framework. It asks not "does this fit our strategy?" but "does this tool *change* what's strategically possible?"

**How to evaluate:**

1. **Before this tool, what was impossible or impractical?**
   - Example: Before Fish Audio, multilingual voice content required separate voice clones per language = impractical.
   - Example: Before OpenClaw, orchestrating 100+ AI agents required custom code = impossible for a solo founder.

2. **What new strategic lever does this unlock?**
   - New market entry (e.g., EU expansion via multilingual content)
   - New revenue stream (e.g., AI consulting enabled by agent framework)
   - New competitive moat (e.g., content in languages competitors ignore)
   - New operating model (e.g., 24/7 automated workflows)

3. **Would this strategic opportunity exist without this tool?**
   - YES → Tool is table stakes, not differentiating. Score lower.
   - NO → Tool is a strategic enabler. Score higher.

**Score 0–10:**
- 0–2: No new strategic possibility. Tool is purely tactical (slightly better version of what we already do).
- 3–5: Minor strategic enhancement. Makes existing strategy slightly more efficient or effective.
- 6–7: Meaningful strategic expansion. Opens a new channel, market segment, or capability that was previously closed.
- 8–10: Transformative. Changes the fundamental geometry of what's possible. Creates a moat. Reframes the business model.

**Verdict threshold:** ≥7 on Strategic Evolution can override a mediocre score elsewhere. A tool that transforms strategy is worth adoption even if the tactical ROI is unclear.

**Documentation rule:** When a tool scores ≥7 on Strategic Evolution, the output MUST include:
- A "Strategic Implications" section describing the new opportunity
- A recommendation to update relevant strategy documents (USER.md, vitae10-positioning skill, etc.)
- A flag for the user: "This tool doesn't just fit your strategy — it expands it. Consider updating your plans."

---

## Scoring & Verdict

### Calculate Weighted Score

```
Weighted Score = (Strategic Fit × 0.25) + (Stack Overlap × 0.15) + (Cost-Benefit × 0.20) + (Time to Value × 0.10) + (Risk × 0.10) + (Strategic Evolution × 0.20)
```

### Verdict Matrix

| Weighted Score | Verdict | Action |
|---|---|---|
| 8.0–10.0 | **ADOPT NOW** | Add to stack immediately. Document in TOOLS.md. Set up within 48 hours. |
| 6.0–7.9 | **ROADMAP** | Add to "Tools to Evaluate" list. Set trigger condition for adoption. Revisit monthly. |
| 4.0–5.9 | **WATCH** | Bookmark. Monitor. No action unless something changes (price drop, feature addition, incumbent breaks). |
| 0–3.9 | **REJECT** | Document why in MEMORY.md. Do not revisit for 6 months unless circumstances change. |

### Override Rules

Override the score if any of these apply:

| Condition | Override |
|---|---|
| User explicitly wants it (regardless of score) | ADOPT NOW — but document reservations |
| Solves a P0 blocker for Vitae10 launch | ADOPT NOW — even if score is 6–7 |
| Replaces a tool that's actively broken or raised prices 50%+ | ADOPT NOW — urgent replacement |
| Free tier with no time limit and zero setup | Score +1.5 |
| Requires new subscription when budget is full | Score -2.0 unless something is cut |
| Open source + self-hostable | Score +1.0 (risk reduction) |

---

## Output Format

Deliver evaluation in this exact format:

```
# Tool Evaluation: [Tool Name]
**Evaluated:** [Date]
**Source:** [URL or how it was discovered]

## 🎯 Strategic Fit: [X]/10
[One paragraph explaining fit or lack thereof]

## 🔀 Stack Overlap: [X]/10
**Existing tools in this space:**
- [Tool A] — does [X], gap vs new tool: [Y]
- [Tool B] — does [X], gap vs new tool: [Y]
**Verdict:** [Complementary / Partial overlap / Duplicate]

## 💰 Cost-Benefit: [X]/10
**Cost:** $___/month + ___ hours setup
**Benefit:** [time saved, revenue enabled, quality gain]
**ROI estimate:** [payback period]

## ⚡ Time to Value: [X]/10
[Timeline to first meaningful result]

## 🛡️ Risk & Lock-in: [X]/10
**Data portability:** [YES/NO]
**Vendor stability:** [assessment]
**Switch cost:** [HIGH/MEDIUM/LOW]

## 📊 Final Score: [X.X]/10

## ✅ Verdict: [ADOPT NOW / ROADMAP / WATCH / REJECT]

### If ADOPT NOW:
**Immediate actions:**
1. [ ]
2. [ ]
3. [ ]
**Document in:** TOOLS.md / MEMORY.md / [skill file]
**Budget impact:** $___/month — [cut X or absorb into existing budget]

### If ROADMAP:
**Trigger condition:** [When to revisit]
**Revisit date:** [Specific date]
**File in:** [where to track]

### If WATCH:
**Monitor for:** [what would change the verdict]
**Check back:** [date]

### If REJECT:
**Why:** [one sentence]
**Revisit if:** [what would change circumstances]
**Logged in:** MEMORY.md
```

---

## Documentation Rules

### If ADOPT NOW:
1. Update `TOOLS.md` — add to relevant section with cost, status, usage
2. Update `ROUTING.md` if it affects LLM routing or guard skills
3. Append to `MEMORY.md` — brief note on adoption date and why
4. If it's a skill: install it, test it, document in `CLAUDE.md`

### If ROADMAP:
1. Create entry in `ROADMAP.md` (or `MEMORY.md` if no ROADMAP.md exists)
2. Set cron reminder for revisit date
3. Document trigger condition clearly

### If WATCH:
1. Add to `WATCHLIST.md` (or `MEMORY.md` watch section)
2. Set monthly check reminder

### If REJECT:
1. Append one-line note to `MEMORY.md` — tool name, score, why rejected
2. Do NOT add to TOOLS.md or any active tracking
3. Set 6-month re-evaluation reminder only if tool is in competitive space

---

## Example Evaluation (Fish Audio — Already Done)

```
# Tool Evaluation: Fish Audio
**Evaluated:** 2026-07-30
**Source:** Perplexity deep-dive research

## 🎯 Strategic Fit: 7/10
Direct fit for AI Revenue Systems (multilingual content) and Phase 1 Vitae10 EU expansion.
No fit for current Phase 0 UK launch (English-only).

## 🔀 Stack Overlap: 5/10
**Existing:** ElevenLabs (voice clone, TTS)
**Gap:** Fish Audio has 83 languages vs ElevenLabs 29. Zero-shot cross-lingual cloning.
ElevenLabs has slightly better pure realism (8.9 vs 8.7).
**Verdict:** Partial overlap — Fish Audio wins on multilingual, loses slightly on quality.

## 💰 Cost-Benefit: 8/10
**Cost:** ~$5.50/mo (Plus plan) vs ElevenLabs $6/mo
**Benefit:** 70% cheaper API, instant cloning, multilingual voice = EU content moat
**ROI:** Positive immediately if multilingual strategy activates

## ⚡ Time to Value: 8/10
Same-day setup. 10-second voice clone. API docs clear.

## 🛡️ Risk & Lock-in: 7/10
**Data portability:** YES (open source models)
**Vendor stability:** Startup, but open-source reduces lock-in
**Switch cost:** LOW — voice clones are portable, API is standard REST

## 📊 Final Score: 7.2/10

## ✅ Verdict: ROADMAP
**Trigger condition:** Activate when (1) ElevenLabs bill exceeds $20/mo, OR (2) Vitae10 enters Phase 1 multilingual content, OR (3) ElevenLabs voice clone needs re-training.
**Revisit date:** 2026-09-01
**Filed in:** TOOLS.md under "Fish Audio — Alternative Evaluated"
```

---

## Related Skills

| Skill | When to use |
|---|---|
| `budget-gate` | Before any expensive operation — run AFTER tool-evaluator if adoption is recommended |
| `rate-limit-guard` | Before any API-dependent tool adoption |
| `idea-validator` | When evaluating a business idea, not a tool |
| `leverage-stack-auditor` | When evaluating income streams and time allocation, not tools |
| `kimi-council` | When decision needs multi-perspective pressure-test |

---

*Version: 1.0*
*For: Colin's multi-strategy tool stack*
*Principle: No tool is adopted without evidence. No FOMO. Every tool earns its place.*
