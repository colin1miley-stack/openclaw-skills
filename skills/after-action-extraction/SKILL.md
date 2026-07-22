---
name: after-action-extraction
description: |
  Extract transferable principles from any success, failure, or project outcome using military-grade 
  after-action review methodology. Use when the user wants to "learn from wins", "deconstruct success", 
  "extract patterns", "after-action review", "what made this work", "why did this succeed", 
  "transferable principles", or any analysis of a completed project, deal, launch, or campaign.
  
  Also triggers on: "post-mortem", "retrospective", "lessons learned", "debrief", 
  "what actually worked", "reverse-engineer success", or when celebrating a win without understanding it.
originally_built_for: [Your Brand]
---

# After-Action Extraction

Adopt the role of a pattern extraction specialist who previously worked in after-action review units for elite military teams, where the mandate after every mission — successful or failed — was to extract every transferable principle before institutional memory decayed.

**The Core Insight:** Most organizations are terrible at learning from their own wins. They celebrate success, attribute it to talent or luck, and move on without understanding the actual mechanics that produced the result. This skill fixes that.

## When to Use

- After a successful product launch, campaign, or deal
- After a failed project (extract what NOT to repeat)
- When onboarding new team members to a successful process
- Before scaling a tactic that worked once
- When documenting playbooks or SOPs from lived experience
- When a win feels like "luck" and you need to know if it was

## When NOT to Use

- For speculative future plans (no outcome to deconstruct)
- For ongoing projects (wait until completion)
- When the user wants forward-looking strategy rather than backward-looking analysis

## Required Input

Before beginning extraction, collect:

| Input | Question | Why It Matters |
|-------|----------|----------------|
| **Outcome** | "What specifically happened? What was the result?" | Defines the success to deconstruct |
| **Timeline** | "Walk me through the timeline from start to finish." | Enables chronological phase mapping |
| **Context** | "What was the situation? Constraints? Market conditions?" | Distinguishes situational luck from engineered success |
| **Team/Resources** | "Who was involved? What tools, budget, relationships?" | Identifies replicable structural factors |

> **Note:** The user may provide a full narrative, bullet points, or a document. Accept any format. If information is missing, flag it: *"[Detail] not provided — analysis assumes [assumption] or leaves gap."*

---

## Recursive Decomposition Methodology

### Phase 1: Recursion Down

Drill from outcome to mechanics. Each level answers a smaller, sharper question.

#### Level 1 — Chronological Phases

Break the success into **3-5 major phases** in chronological order.

For each phase, identify:
- **Phase name:** [Short label]
- **Timeframe:** [When this phase occurred]
- **Outcome produced:** [What this phase delivered]
- **Enabling function:** [How this outcome enabled the next phase]

**Template:**
```
Phase 1: [Name] → [Timeframe]
  Outcome: [What was produced]
  Enabled: [How this made Phase 2 possible]

Phase 2: [Name] → [Timeframe]
  Outcome: [What was produced]
  Enabled: [How this made Phase 3 possible]

[...continue through all phases]
```

**Rules:**
- Phases should be chunky but meaningful (not hourly tasks)
- If you can't articulate how Phase N enabled Phase N+1, you've misdrawn the boundary
- Maximum 5 phases. If there are more, consolidate adjacent micro-phases

---

#### Level 2 — Key Decisions Within Each Phase

Within **each phase**, identify the **2-4 key decisions or actions** that most directly contributed to that phase's outcome.

**Not everything that happened** — only the moves that mattered. If removing this decision would not materially change the outcome, it doesn't belong here.

**Template (per phase):**
```
Phase: [Name]

Key Decision 1: [What was done]
  → Directly produced: [Specific outcome]
  → Without it: [What would have been different]

Key Decision 2: [What was done]
  → Directly produced: [Specific outcome]
  → Without it: [What would have been different]

[...2-4 decisions per phase]
```

**Rules:**
- Be ruthless about filtering. Ten things happened; three mattered.
- Frame decisions as choices with alternatives, not as inevitable events
- If a decision was made by someone else (vendor, client, market), note it but don't extract it as a controllable principle

---

#### Level 3 — Decision Anatomy

For **each key decision**, answer three questions:

**Question 1: Information & Conditions**
> What information or conditions existed at the time that made this the right move?

- What did the decision-maker know that others might not have?
- What conditions in the environment made this decision viable?
- What data, research, or prior experience informed the choice?

**Question 2: Decision Mode**
> Was this decision made deliberately based on reasoning, or did it happen intuitively or accidentally?

- **Deliberate/Reasoned:** Structured analysis, framework, prior planning
- **Intuitive:** Gut feel, pattern recognition, experience-based snap judgment
- **Accidental:** Unplanned, emergent, a byproduct of something else
- **Mixed:** Some reasoning, some intuition, some luck

**Question 3: Counterfactual**
> What would have happened if the opposite decision had been made?

- What was the most likely alternative action?
- What outcome would that alternative have produced?
- How much worse (or better) would it have been?
- Was the "right" decision only right in hindsight?

**Template (per decision):**
```
Decision: [What was done]

Q1 — Information & Conditions:
  [What was known / What conditions existed]

Q2 — Decision Mode:
  [Deliberate / Intuitive / Accidental / Mixed — with evidence]

Q3 — Counterfactual:
  Alternative: [What would have been done instead]
  Likely outcome: [What would have happened]
  Delta: [How much this decision actually mattered]
```

---

### Phase 2: Recursion Up

Reassemble from mechanics to transferable principles. The goal: separate what was luck from what was engineering.

#### Level 1 — Situational vs. Engineered

From the Level 3 analysis, classify each key decision:

| Classification | Definition | Examples |
|----------------|-----------|----------|
| **Situational** | Unique to this specific situation; unlikely to repeat | One-time market window, specific relationship, unrepeatable timing, competitor mistake |
| **Engineered** | Conditions you can recreate or engineer in future projects | Process design, information systems, decision criteria, team structure, preparation |
| **Hybrid** | Situational opportunity exploited through engineered capability | Right place + right preparation, luck surface area |

**Template:**
```
Decision: [What was done]
Classification: [Situational / Engineered / Hybrid]
Rationale: [Why this classification]
Transferability: [High / Medium / Low / None]
```

**Critical Rule:** Be brutally honest about classification. The most dangerous bias in after-action review is upgrading situational luck to engineered skill. When in doubt, downgrade.

---

## Output Format

### 1. Executive Summary

**One paragraph:** What happened, why it mattered, and the single most important transferable principle.

> *"[Project/deal] succeeded because [core mechanism]. The key transferable insight is [principle]. [X]% of the outcome was situational; [Y]% was engineered."*

---

### 2. Phase Map

```
[Outcome]
  └── Phase 1: [Name] → [Outcome] → enabled Phase 2
        └── Key Decision A → [Anatomy]
        └── Key Decision B → [Anatomy]
  └── Phase 2: [Name] → [Outcome] → enabled Phase 3
        └── Key Decision C → [Anatomy]
        └── Key Decision D → [Anatomy]
  └── Phase 3: [Name] → [Final Outcome]
        └── Key Decision E → [Anatomy]
```

---

### 3. Decision Anatomy Table

| Phase | Decision | Info/Conditions | Mode | Counterfactual Delta | Classification |
|-------|----------|----------------|------|---------------------|----------------|
| 1 | [Decision A] | [What was known] | [Deliberate] | [High/Med/Low] | [Engineered] |
| 1 | [Decision B] | [What was known] | [Intuitive] | [High/Med/Low] | [Situational] |
| 2 | [Decision C] | [What was known] | [Mixed] | [High/Med/Low] | [Hybrid] |

---

### 4. Transferable Principles

**Engineered Decisions (Recreate These):**
1. **[Principle Name]:** [What to recreate] — From: [Decision reference]
2. **[Principle Name]:** [What to recreate] — From: [Decision reference]

**Situational Factors (Don't Count On These):**
1. **[Factor]:** [What happened] — [Why it won't repeat]
2. **[Factor]:** [What happened] — [Why it won't repeat]

**Hybrid Insights (Engineered Preparedness + Situational Opportunity):**
1. **[Insight]:** [How preparation met opportunity] — [How to increase surface area]

---

### 5. Anti-Patterns (What Almost Killed It)

From the counterfactual analysis, identify:
- **Near-misses:** Decisions that nearly went wrong
- **Single points of failure:** Things that only worked because of luck
- **Unforced errors that somehow didn't matter:** Bad decisions that didn't sink the project

---

### 6. Actionable Playbook

**If we had to repeat this outcome tomorrow:**
```
1. [Phase 1 action] — [Specific playbook step]
2. [Phase 2 action] — [Specific playbook step]
3. [Phase 3 action] — [Specific playbook step]
```

**Preconditions that must exist:**
- [Resource/condition 1]
- [Resource/condition 2]

**Where to invest for repeatability:**
- [Area 1]: [Why this matters for future projects]
- [Area 2]: [Why this matters for future projects]

---

## Rules

1. **Celebrate Nothing Until Deconstructed**
   - Never attribute success to "talent" or "luck" as a final answer
   - Every win has mechanics. Find them.

2. **Ruthless Filtering**
   - If a decision didn't materially change the outcome, exclude it
   - Ten things happened; three mattered. Find the three.

3. **Honest Classification**
   - When in doubt, downgrade situational to luck
   - The most expensive mistake is building a playbook around a one-time market window

4. **No Hindsight Bias**
   - Evaluate decisions based on information available AT THE TIME
   - If a decision was lucky, call it lucky — even if it worked

5. **Complete the Counterfactual**
   - Never skip Question 3 (What if the opposite?)
   - This is where the real learning lives

6. **Quantify Where Possible**
   - "High impact" → try "~40% of the outcome"
   - "Fast" → try "2 weeks vs. typical 6 weeks"
   - Specificity builds better playbooks

7. **Flag Missing Information**
   - If a critical detail is unknown, state the assumption
   - Don't fabricate reasoning to make the narrative clean

---

## Example Invocation

> User: "We just closed our biggest enterprise deal. I want to understand what actually worked so we can repeat it."

**Extraction begins:**

**Phase Map:**
1. **Outbound Targeting** (Week 1) — Identified the account and stakeholder map → enabled warm intro
2. **Discovery & Pain Mapping** (Weeks 2-3) — Uncovered a compliance deadline no one else knew about → enabled urgency
3. **Proof of Concept** (Week 4) — Built a live demo with their data → enabled internal champion
4. **Executive Alignment** (Week 5) — Secured CFO buy-in → enabled budget approval
5. **Contract & Close** (Week 6) — Negotiated terms → deal signed

**Key Decisions (sample):**
- Phase 2: Prioritized compliance deadline over feature checklist (Deliberate, based on LinkedIn research of their recent audit)
- Phase 3: Built PoC with real data instead of generic demo (Accidental — they offered it, we said yes)
- Phase 4: Scheduled CFO meeting through champion instead of cold outreach (Intuitive — champion seemed eager)

**Classification:**
- Compliance deadline research → **Engineered** (process of researching publicly available signals)
- Real-data PoC → **Hybrid** (situational opportunity: they offered data + engineered capability: we could integrate fast)
- CFO meeting routing → **Situational** (specific champion's internal relationships; not replicable)

**Playbook:** Build a pre-call research checklist that surfaces compliance deadlines, regulatory events, and board meeting dates. This is recreatable. The specific champion relationship is not.

---

## Notes

- **Works for failures too.** The same methodology extracts why things broke. Counterfactual becomes: "What if we HAD done X?"
- **Works for small wins.** Don't reserve this for major launches. A single successful LinkedIn post contains extractable principles.
- **Works for others' wins.** You can after-action-review a competitor's successful campaign using public information.
- **Time investment:** 15-30 minutes of structured thinking saves months of repeating the wrong playbook.

---

*"The difference between a one-hit wonder and a repeatable machine is the quality of their after-action extraction. Most people celebrate. The best people deconstruct."*
