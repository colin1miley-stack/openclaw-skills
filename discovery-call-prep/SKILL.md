---
name: discovery-call-prep
description: B2B sales discovery call preparation and execution. Adapted from mattpocock/grilling. Stress-tests prospect qualification, pain point mapping, and solution fit before the call. Use when preparing for a Revenue Systems sales call or qualifying a lead.
---

# Discovery Call Prep — Adapted from Grilling

**Source:** `mattpocock/skills/productivity/grilling` (MIT License)  
**Adapted:** 2026-09-05  
**Purpose:** Revenue Systems B2B sales discovery — qualify prospects, map pain to solutions  

---

## ICM Layer Mapping

| Layer | File | Usage |
|-------|------|-------|
| **Layer 0** | `SOUL.md`, `AGENTS.md` | Sales ethics, no manipulation |
| **Layer 1** | Root `CONTEXT.md` | Route to sales stages |
| **Layer 2** | This SKILL.md | Task contract for discovery prep |
| **Layer 3** | `skills/discovery-call-prep/` | Question frameworks, objection handlers |
| **Layer 4** | `stages/08-sales-prep/output/` | Call scripts, prospect profiles, learnings |

---

## What Discovery Call Prep Does

```
Prospect data (LinkedIn, website, grant matcher, CRM)
    ↓
Structured qualification (BANT/MEDDIC hybrid)
    ↓
Pain point mapping to Revenue Systems offerings
    ↓
Objection anticipation and response prep
    ↓
Call script with branching logic
    ↓
Post-call scoring and next-action recommendation
```

---

## The Discovery Framework

### Qualification: BANT-MEDDIC Hybrid

| Dimension | Questions | Revenue Systems Angle |
|-----------|-----------|----------------------|
| **Budget** | What budget is allocated for revenue optimization? | Audit: €7,500; Build: €12,500+ |
| **Authority** | Who decides on revenue system investments? | Map stakeholders |
| **Need** | What revenue leaks have you identified? | Signal detection → evidence |
| **Timeline** | When do you need to see results? | Sprint: 2 weeks; Build: 6-8 weeks |
| **Metrics** | How do you measure sales effectiveness now? | Baseline for improvement |
| **Economic Buyer** | Who owns the P&L? | C-suite engagement required |
| **Decision Process** | What approvals are needed? | Contract, procurement |
| **Champion** | Who internally advocates for this? | Reference for future calls |

---

## Call Structure (Rounds)

### Round 1: Context & Current State

```
❓ **Q1** - **Revenue Overview**: What's your current annual revenue, and what growth rate are you targeting?
➡️ Listen for: Specific numbers, growth urgency

❓ **Q2** - **Team Structure**: How many people are in your sales/revenue organization?
➡️ Listen for: Team size, roles, gaps

❓ **Q3** - **Tech Stack**: What tools do you use for CRM, outreach, analytics?
➡️ Listen for: Integration opportunities, data quality issues
```

### Round 2: Pain Point Detection

```
❓ **Q4** - **Lead Follow-up**: What percentage of inbound leads get a response within 1 hour?
➡️ Listen for: "I don't know" = red flag; <50% = massive opportunity

❓ **Q5** - **Pipeline Visibility**: How do you track deals from first contact to close?
➡️ Listen for: Spreadsheet answers = CRM gap

❓ **Q6** - **Conversion Rates**: What are your current conversion rates at each stage?
➡️ Listen for: Vague answers = measurement gap

❓ **Q7** - **Time Spent**: How much time do reps spend on admin vs. selling?
➡️ Listen for: >30% admin = automation opportunity
```

### Round 3: Solution Mapping

```
❓ **Q8** - **Past Attempts**: What have you tried to fix these issues?
➡️ Listen for: Tool purchases without implementation = implementation gap

❓ **Q9** - **Success Definition**: What would a successful engagement look like in 90 days?
➡️ Listen for: Specific, measurable outcomes

❓ **Q10** - **Budget Reality**: For the right solution, what's the budget range?
➡️ Listen for: €7,500+ = audit viable; €12,500+ = build viable
```

### Round 4: Next Steps

```
❓ **Q11** - **Decision Process**: Who else needs to be involved in this decision?
➡️ Action: Schedule follow-up with economic buyer

❓ **Q12** - **Timeline**: What's driving the urgency?
➡️ Action: Align proposal to their timeline

❓ **Q13** - **Assessment**: Would you be open to a free Revenue Leak Assessment?
➡️ Action: Send assessment link or book diagnostic call
```

---

## Objection Handling

| Objection | Response |
|-----------|----------|
| "We don't have budget" | "The assessment is free. Let's find the leaks first, then budget becomes obvious." |
| "We're evaluating options" | "Fair. What criteria matter most? I can show you exactly how we compare." |
| "We need to see ROI first" | "That's exactly what the audit delivers — evidence, not projections." |
| "Our team is too busy" | "The audit takes 2 hours of your time. The fixes save 8+ hours/week." |
| "We've tried consultants before" | "We're not consultants — we build systems that run without us." |
| "We need to talk internally" | "Of course. What questions can I answer for that conversation?" |

---

## Post-Call Scoring

| Score | Meaning | Next Action |
|-------|---------|-------------|
| **A (80-100)** | Clear pain, budget, authority, timeline | Send proposal within 24h |
| **B (60-79)** | Pain clear, one dimension missing | Schedule follow-up to resolve gap |
| **C (40-59)** | Interest but unclear fit | Send assessment, nurture sequence |
| **D (0-39)** | No clear need or budget | Archive, check back in 6 months |

---

## ICM Stage Contract

```json
{
  "stageName": "sales-prep",
  "branchName": "feat/discovery-call-prep",
  "qualityGate": "quality-gate.md",
  "subTasks": [
    {
      "id": "sp-001",
      "title": "Research prospect (LinkedIn, website, CRM)",
      "agent": "builder",
      "acceptance": ["Company profile", "Key contacts", "Recent news"],
      "estimatedTokens": 5000
    },
    {
      "id": "sp-002",
      "title": "Build qualification questionnaire",
      "agent": "builder",
      "acceptance": ["BANT-MEDDIC covered", "Revenue Systems mapped"],
      "estimatedTokens": 5000
    },
    {
      "id": "sp-003",
      "title": "Prepare call script with branching",
      "agent": "builder",
      "acceptance": ["Round-based structure", "Objection handlers", "Next-action triggers"],
      "estimatedTokens": 8000
    },
    {
      "id": "sp-004",
      "title": "Post-call scoring and action",
      "agent": "builder",
      "acceptance": ["Score calculated", "Next step defined", "CRM updated"],
      "estimatedTokens": 3000
    }
  ]
}
```

---

## Quality Gate

- [ ] Prospect research complete (LinkedIn + website + CRM)
- [ ] Qualification questions map to Revenue Systems offerings
- [ ] Objection handlers prepared for top 6 objections
- [ ] Call script has branching logic (if-then)
- [ ] Next-action triggers defined for each round
- [ ] Scoring rubric documented
- [ ] Post-call CRM update template ready

---

*Adapted from mattpocock/skills/productivity/grilling. See original for design tree methodology.*
