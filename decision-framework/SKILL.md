<!--
License: CC BY-NC-SA 4.0 (https://creativecommons.org/licenses/by-nc-sa/4.0/)
Copyright (c) 2026 Colin Miley � AI Revenue Systems
Commercial use requires permission: colin@colinmiley.com
-->
---
name: structured-decision-framework
description: |
  Apply structured, transparent, confidence-weighted reasoning to complex decisions. 
  Use when facing ambiguous choices, high-stakes bets, or problems with no obvious answer.
  
  Triggers on: "help me decide", "what should I do", "evaluate options", "pros and cons", 
  "difficult choice", "strategic decision", "which path", "council review", "think through this".
originally_built_for: [Your Brand]
---

# Structured Decision Framework

A transparent, confidence-weighted methodology for navigating complex decisions. Inspired by military after-action review, medical diagnostic protocols, and Bayesian reasoning.

## When to Use

- Strategic pivots or directional changes
- High-stakes bets with significant downside
- Problems with no obvious answer
- Decisions requiring stakeholder alignment
- Situations where bias or blind spots are likely

## When NOT to Use

- Simple factual questions ("What's the weather?")
- Routine execution tasks ("Draft this email")
- Decisions with clear right answers
- Time-sensitive emergencies (act first, analyze later)

## The Framework

### Step 1: DECOMPOSE

Break the problem into 3-5 clear sub-problems or phases.

**Questions:**
- What are the component parts of this decision?
- What must be true for each part to work?
- What are the dependencies between parts?

**Output:**
```
Sub-problem 1: [Name]
  → Must be true: [Conditions]
  → Enables: [Next sub-problem]

Sub-problem 2: [Name]
  → Must be true: [Conditions]
  → Enables: [Next sub-problem]

[...]
```

---

### Step 2: SOLVE

Address each sub-problem. Provide a confidence score (0.0–1.0) for each solution.

**Questions:**
- What's the best approach to this sub-problem?
- What evidence supports this approach?
- What's my confidence that this will work?

**Confidence Scale:**
- **0.9–1.0:** Near-certain, strong evidence, proven pattern
- **0.7–0.89:** Likely, good evidence, some uncertainty
- **0.5–0.69:** Uncertain, mixed evidence, significant risk
- **0.3–0.49:** Unlikely, weak evidence, major gaps
- **0.0–0.29:** Very unlikely, contradicted by evidence

**Output:**
```
Sub-problem 1: [Approach]
  Confidence: [0.0-1.0]
  Evidence: [What supports this]
  Risks: [What could invalidate this]

Sub-problem 2: [Approach]
  Confidence: [0.0-1.0]
  Evidence: [What supports this]
  Risks: [What could invalidate this]

[...]
```

---

### Step 3: VERIFY

Check each solution for:

| Check | Question | Pass/Fail |
|-------|----------|-----------|
| **Logical consistency** | Does this solution contradict itself or other solutions? | |
| **Factual accuracy** | Are the facts correct? Are sources credible? | |
| **Completeness** | Have I missed any critical factors or alternatives? | |
| **Bias check** | Am I favoring a solution because of preference, not evidence? | |
| **Hidden assumptions** | What am I assuming that might not be true? | |

**Output:**
```
Verification Results:
✅ [Check passed]
⚠️ [Check flagged — explanation]
❌ [Check failed — must address]
```

---

### Step 4: SYNTHESIZE

Combine sub-solutions into a single recommended path, weighted by confidence.

**Calculation:**
- Overall confidence ≈ product of individual confidences (conservative)
- Or: weighted average if sub-problems have different importance

**Output:**
```
RECOMMENDED PATH:
[The integrated solution]

Overall Confidence: [0.0-1.0]
Calculation: [How derived]

Why this path:
- [Reason 1]
- [Reason 2]
- [Reason 3]

Alternative paths considered:
- [Option A] — rejected because [reason]
- [Option B] — rejected because [reason]
```

---

### Step 5: REFLECT

If overall confidence < 0.8:

1. **Identify weaknesses:** What are the main gaps or uncertainties?
2. **Decide:** Retry with modified approach OR ask one clarifying question
3. **Document:** What would increase confidence to 0.8+?

**Output:**
```
REFLECTION:

Overall Confidence: [0.0-1.0]

Weaknesses:
- [Gap 1] — Would need [X] to resolve
- [Gap 2] — Would need [Y] to resolve

Decision:
[Retry with modifications / Ask clarifying question / Proceed with risk acknowledgment]

To reach 0.8+ confidence, I need:
- [Missing information 1]
- [Missing information 2]
```

---

## Simplified Mode

For moderately complex decisions, use the **Lite Version:**

1. **Options:** List 2-4 viable paths
2. **Pros/Cons:** Bullet points for each
3. **Confidence:** High / Med / Low for each option
4. **Recommendation:** Best option + why
5. **Risk:** The one thing most likely to invalidate this

Skip full decomposition if the decision is straightforward.

---

## Example Invocation

> User: "Should we hire a developer now or wait until after launch?"

**DECOMPOSE:**
1. Current technical needs (what actually needs building?)
2. Budget impact (can we afford £3-5K/month?)
3. Timing (will a developer speed up launch or add complexity?)
4. Risk (what if we hire the wrong person?)

**SOLVE:**
1. Technical needs: Quiz logic, Shopify customizations, Klaviyo flows → Mostly template/no-code solutions exist → Confidence: 0.85
2. Budget: £2,000 first-run budget → Developer salary would consume it in 1 month → Confidence: 0.9
3. Timing: Launch in 3 months → Developer onboarding takes 4-6 weeks → Adds risk, minimal speed gain → Confidence: 0.6
4. Risk: Wrong hire = 2 months lost + £6K burned → High downside, hard to recover → Confidence: 0.7

**VERIFY:**
- ✅ Consistent: All sub-problems point to "wait"
- ✅ Accurate: Budget numbers from business plan
- ⚠️ Completeness: Haven't evaluated freelance vs. full-time
- ⚠️ Bias: I might be biased toward delaying spend

**SYNTHESIZE:**
Recommended path: Wait until post-launch (Month 4+) to hire developer. Use no-code tools and manual processes for v1. Revisit when MRR supports the salary.

Overall Confidence: 0.75 (product: 0.85 × 0.9 × 0.6 × 0.7 = 0.32; weighted average ≈ 0.75)

**REFLECT:**
Confidence: 0.75 (below 0.8 threshold)

Weaknesses:
- Haven't evaluated freelance/agency as middle ground
- Assuming no-code can handle all v1 needs

Clarifying question: "What's the one technical feature that absolutely cannot be done with no-code tools, and what's the fallback if we can't build it?"

---

## Rules

1. **Be honest about confidence.** Don't inflate to make the user feel better.
2. **Show your work.** The value is in the reasoning, not just the answer.
3. **Flag bias explicitly.** "I may be biased toward X because Y."
4. **Distinguish known from assumed.** "This is fact" vs. "This is assumed."
5. **It's okay to say "I don't know."** Better than false confidence.
6. **For time-sensitive decisions:** Use Lite Version. Speed beats perfection.

---

*"The goal isn't certainty — it's clarity. A well-reasoned decision with 70% confidence beats a gut feeling with 90% confidence, because you can improve the reasoning. You can't improve a gut feeling."*
