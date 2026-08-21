---
name: audit-pipeline
version: "2.0"
description: "AI Revenue Systems Consulting Audit Pipeline. Transforms discovery call data into a priced, actionable proposal through a multi-node graph."
argument-hint: 'audit-pipeline --client "Acme Corp" --notes "discovery_call_notes.txt" --type sales_process'
allowed-tools: Read, Write, Edit, Exec, kimi_search, sessions_spawn
homepage: https://github.com/colin1miley-stack/openclaw-skills
author: AI Revenue Systems
license: MIT
user-invocable: true
metadata:
  openclaw:
    emoji: "🔍"
    requires:
      env: []
      bins: []
    tags:
      - audit
      - consulting
      - pipeline
      - multi-agent
      - workflow
---

# AI Revenue Systems — Consulting Audit Pipeline

**Purpose:** Transform a sales team's raw operational data into a priced, actionable AI workflow audit proposal.

**Fee:** £1,850 per audit  
**AI Cost:** ~£0.86 typical, £50 hard cap  
**ROI:** 2,150x at standard fee

---

## When to Use

Activate this skill when:
- Colin has completed a discovery call with a qualified prospect
- The prospect has 5+ sales reps, uses a CRM, and has identified pain points
- Budget authority is confirmed
- Colin wants to generate a professional audit proposal

**Do NOT use when:**
- Prospect is unqualified (< 5 reps, no CRM, no clear pain)
- Discovery call was not completed
- Colin has not confirmed budget authority

---

## Workflow Overview

```
[Discovery Notes] → [Intake] → [Router] → [Data Collection ×5] → [Synthesis]
                                                        ↓
[Delivery] ← [Approval Gate] ← [Proposal] ← [Recommendations] ← [Review]
```

**Nodes:** 12 workers, 2 routers, 1 reviewer, 2 human gates, 1 veto  
**Topology:** Sequential → Router → Fan-Out → Fan-In → Loop → Gate → Terminal

**Steps (see references/workflow-steps.md for full detail):**
1. Intake — Parse discovery call notes
2. Router — Classify audit type
3. Parallel Data Collection (×5) — CRM, Calendar, Email, Competitors, Process
4. Synthesis — Merge findings
5. Quality Review — Validate against rubric
6. Recommendations + ROI — Prioritised actions
7. Proposal Assembly — Client-ready document
8. Colin Approval Gate — Human review
9. Delivery — Send to client

---

## State Management

Each audit lives in `audits/[client-slug]/` with the following outputs:

| File | Step | Description |
|------|------|-------------|
| `INTAKE.md` | 1 | Structured discovery parameters |
| `ROUTING.md` | 2 | Audit type classification |
| `data/CRM_ANALYSIS.md` | 3A | CRM pipeline analysis |
| `data/CALENDAR_ANALYSIS.md` | 3B | Meeting efficiency analysis |
| `data/EMAIL_REVIEW.md` | 3C | Sequence quality review |
| `data/COMPETITOR_INTEL.md` | 3D | Competitor research |
| `data/PROCESS_REVIEW.md` | 3E | SOP maturity assessment |
| `FINDINGS.md` | 4 | Combined findings document |
| `REVIEW.md` | 5 | Quality review score |
| `RECOMMENDATIONS.md` | 6 | Prioritised recommendations |
| `PROPOSAL.md` | 7 | Client-ready proposal |
| `DELIVERY.md` | 9 | Delivery log |
| `state.json` | — | Live state tracking |

See references/output-templates.md for full template specs and state schema.

---

## Quick Start

```bash
# 1. Create audit directory
mkdir -p audits/[client-slug]

# 2. Write intake (manually or from discovery notes)
# ... create INTAKE.md ...

# 3. Run the pipeline (this skill handles steps 2-9)
/audit-pipeline --client "Acme Corp" --intake audits/acme-corp/INTAKE.md

# 4. Approve at gate
# (Colin reviews PROPOSAL.md, gives approve/reject)

# 5. Deliver
# (Pipeline sends or Colin sends manually)
```

---

## Integration with Other Skills

| Skill | Integration Point |
|-------|-------------------|
| `rate-limit-guard` | Runs before every expensive step |
| `budget-gate` | Checks total cost before proceeding |
| `kimi-council` | Used in Step 6 for strategic recommendations |
| `last30days` | Used in Step 3D for competitor social pulse |
| `defuddle` | Used in Step 3E for doc content extraction |

---

## References

| Topic | File |
|-------|------|
| Full step-by-step workflow | references/workflow-steps.md |
| Output templates & state schema | references/output-templates.md |
| Quality review rubric | references/quality-rubric.md |
| Cost controls & vetoes | references/cost-controls.md |
| Anti-patterns | references/anti-patterns.md |

---

## Evaluations

See evals/tests.md for self-evaluation test cases.

---

*Version: 2.0 | Framework: Graph Engineering Design Framework v1.0*
