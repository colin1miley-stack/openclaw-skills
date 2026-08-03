---
name: audit-pipeline
version: "1.0"
description: "AI Revenue Systems Consulting Audit Pipeline. Transforms discovery call data into a priced, actionable proposal through a multi-node graph: intake → router → parallel data collection → synthesis → review → approval gate → delivery."
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

---

## Step 1: Intake — Parse Discovery Call

**Goal:** Convert raw notes into structured audit parameters.

**Required Input:**
- Client name
- Industry
- Team size (number of sales reps)
- Discovery call notes (transcript or bullet points)
- Pain points identified
- Current tools (CRM, calendar, email platform)
- Budget confirmed (yes/no)
- Timeline (urgent/standard/flexible)
- Contact email

**Optional Input:**
- CRM data export (CSV)
- Calendar export (ICS)
- Sample email sequences
- Process documentation (PDF/Notion/Confluence)
- Top 3 competitors

**Action:**
```
# Read discovery notes
read <path-to-notes-file>

# Structure into intake format
# (If notes are unstructured, use this skill's reasoning to extract fields)
```

**Output — Intake Document:**
```markdown
# Audit Intake: [Client Name]

## Client Profile
- **Name:** [Name]
- **Industry:** [Industry]
- **Team Size:** [N] sales reps
- **Timeline:** [urgent/standard/flexible]
- **Budget Confirmed:** [yes/no]
- **Contact:** [email]

## Current Stack
- CRM: [Salesforce/HubSpot/Pipedrive/none]
- Calendar: [Google/Outlook/Calendly]
- Email: [Gmail/Outlook/sequence tool]
- Other: [list]

## Pain Points (from discovery)
1. [Pain point 1]
2. [Pain point 2]
3. [Pain point 3]

## Data Access
- CRM export: [provided / not provided / API access]
- Calendar export: [provided / not provided]
- Email samples: [provided / not provided]
- Process docs: [provided / not provided]

## Competitors
1. [Competitor 1]
2. [Competitor 2]
3. [Competitor 3]

## Audit Type Recommendation
- **Detected:** [sales_process / content_engine / lead_gen / full_system]
- **Confidence:** [0-1]
- **Rationale:** [why this type]
```

Save to: `audits/[client-slug]/INTAKE.md`

---

## Step 2: Router — Classify Audit Type

**Goal:** Determine which audit module to run.

**Decision Logic:**
```
IF pain_points contains ("pipeline" OR "forecasting" OR "deal_stages" OR "quota")
   → sales_process
ELSE IF pain_points contains ("content" OR "LinkedIn" OR "newsletter" OR "blog")
   → content_engine
ELSE IF pain_points contains ("leads" OR "prospecting" OR "outreach" OR "qualification")
   → lead_gen
ELSE
   → full_system
```

**Confidence Threshold:** 0.7

**If confidence < 0.7:** Pause and ask Colin to confirm audit type.

**Output:** Write audit type to `audits/[client-slug]/ROUTING.md`

---

## Step 3: Parallel Data Collection (Fan-Out)

**Goal:** Gather data from 5 independent sources simultaneously.

### 3A: CRM Analysis

**Input:** CRM export (CSV) or API access

**What to extract:**
- Pipeline stages and counts
- Conversion rates (stage → stage)
- Rep performance (deals won, avg deal size, cycle time)
- Forecasting accuracy (if historical data available)
- Data quality issues (missing fields, duplicates)

**If no CRM export:** Document this gap and proceed with qualitative analysis from discovery notes.

**Output:** `audits/[client-slug]/data/CRM_ANALYSIS.md`

### 3B: Calendar Analysis

**Input:** Calendar export (ICS) or Google Calendar data

**What to extract:**
- Meetings per week (average)
- No-show rate
- Prep time per meeting
- Meeting type breakdown (discovery, demo, follow-up, internal)
- Time wasters (no agenda, too long, wrong attendees)

**If no calendar export:** Use discovery notes to estimate.

**Output:** `audits/[client-slug]/data/CALENDAR_ANALYSIS.md`

### 3C: Email/Sequence Review

**Input:** Sample emails or sequence templates

**What to assess:**
- Number of sequences reviewed
- Tone consistency
- Personalisation score (0-1)
- Open/reply rates (if provided)
- Improvement opportunities

**If no samples:** Document gap, use industry benchmarks.

**Output:** `audits/[client-slug]/data/EMAIL_REVIEW.md`

### 3D: Competitor Intelligence

**Input:** Client's top 3 competitors

**Action:** Use `kimi_search` to research each competitor:
```
kimi_search: "[competitor name] sales process messaging pricing 2026"
```

**What to extract:**
- Positioning/messaging
- Pricing (if public)
- Tech stack (inferred from job posts, case studies)
- Recent moves (funding, product launches)

**Output:** `audits/[client-slug]/data/COMPETITOR_INTEL.md`

### 3E: Process Documentation Review

**Input:** Any SOPs, playbooks, onboarding docs

**What to assess:**
- Process maturity score (0-1)
- Gaps identified ("No onboarding playbook", "No objection handler")
- Outdated materials
- Missing documentation

**If no docs:** Flag as critical finding.

**Output:** `audits/[client-slug]/data/PROCESS_REVIEW.md`

---

## Step 4: Synthesis — Combine Findings

**Goal:** Merge all 5 data sources into a coherent findings document.

**Merge Strategy:**
1. Categorise findings: Pipeline, Forecasting, Content, Lead Gen, Process
2. Assign severity: critical / high / medium / low
3. Quantify business impact (£) where possible
4. Identify cross-cutting themes

**Output:** `audits/[client-slug]/FINDINGS.md`

```markdown
# Findings: [Client Name]

## Executive Summary
[3-4 sentences on the state of their sales operation]

## Critical Findings
### 1. [Finding Title]
- **Severity:** Critical
- **Evidence:** [Data points from CRM/calendar/email]
- **Business Impact:** £[X] per [month/quarter/year]
- **Root Cause:** [Why this is happening]

## High Findings
[Same structure]

## Medium/Low Findings
[Same structure]

## Cross-Cutting Themes
1. [Theme 1 — e.g., "Lack of automation across the funnel"]
2. [Theme 2 — e.g., "No consistent qualification framework"]

## Data Quality Notes
- [What we couldn't access and why]
```

---

## Step 5: Quality Review

**Goal:** Validate findings against rubric before proceeding.

**Rubric:**
| Criterion | Weight | Check |
|-----------|--------|-------|
| Evidence-backed | 30% | Every critical/high finding has ≥2 data points |
| Business impact quantified | 25% | ≥60% of findings have £ impact |
| Actionable | 25% | Every finding has a clear next step |
| No hallucinations | 20% | All claims traceable to source data |

**Scoring:**
- Score ≥ 0.8: Approve → proceed to Step 6
- Score < 0.8: Reject → return to Step 4 with specific feedback (max 1 revision)
- Critical gap found: Pause → ask Colin for clarification

**Output:** `audits/[client-slug]/REVIEW.md`

---

## Step 6: Recommendations + ROI

**Goal:** Write specific, prioritised recommendations with projected ROI.

**For each recommendation:**
- Priority (1 = do first)
- Title
- Description
- Expected impact ("Increase pipeline velocity by 20%")
- Estimated ROI ("£50K in Year 1")
- Implementation effort (low/medium/high)
- Tools suggested
- Quick win? (can be done in <1 week)

**Implementation Roadmap:**
- Week 1-2: [Quick wins]
- Month 1: [Phase 1]
- Quarter 1: [Phase 2]

**Output:** `audits/[client-slug]/RECOMMENDATIONS.md`

---

## Step 7: Proposal Assembly

**Goal:** Format everything into a client-ready document.

**Structure:**
1. Cover page
2. Executive Summary
3. Methodology (how the audit was conducted)
4. Findings (critical → high → medium)
5. Recommendations (prioritised)
6. Implementation Roadmap
7. Pricing
   - Audit fee: £1,850 (fixed)
   - Implementation estimate: [variable, based on recommendations]
   - Projected ROI: [from recommendations]
8. Next Steps
9. Terms & Conditions

**Output:** `audits/[client-slug]/PROPOSAL.md`

---

## Step 8: Colin Approval Gate

**Goal:** Colin reviews and approves before delivery.

**Interface:**
```
🔍 Proposal Ready for Review

Client: [Name]
Audit Type: [Type]
Findings: [N critical, N high, N medium]
Recommendations: [N total, N quick wins]
Projected ROI: [£X in Year 1]

Options:
[APPROVE & SEND] [EDIT] [REJECT & REVISE]

Timeout: 24 hours → Reminder at 12h
```

**If approved:** Proceed to Step 9
**If rejected:** Return to Step 6 with feedback (max 1 revision)
**If timeout:** Hold proposal, send reminder

---

## Step 9: Delivery

**Goal:** Send proposal to client via agreed channel.

**Actions:**
1. Convert PROPOSAL.md to PDF (if required)
2. Send via email (or Slack/portal as agreed)
3. Log delivery in state
4. Set follow-up reminder (48h)

**Output:** `audits/[client-slug]/DELIVERY.md`

---

## State Management

### Directory Structure

```
audits/
└── [client-slug]/
    ├── INTAKE.md              # Step 1 output
    ├── ROUTING.md             # Step 2 output
    ├── data/
    │   ├── CRM_ANALYSIS.md    # Step 3A
    │   ├── CALENDAR_ANALYSIS.md    # Step 3B
    │   ├── EMAIL_REVIEW.md    # Step 3C
    │   ├── COMPETITOR_INTEL.md    # Step 3D
    │   └── PROCESS_REVIEW.md  # Step 3E
    ├── FINDINGS.md            # Step 4 output
    ├── REVIEW.md              # Step 5 output
    ├── RECOMMENDATIONS.md     # Step 6 output
    ├── PROPOSAL.md            # Step 7 output
    ├── DELIVERY.md            # Step 9 output
    └── state.json             # Live state tracking
```

### State Schema (state.json)

```json
{
  "run_id": "uuid",
  "client_slug": "acme-corp",
  "status": "running|paused|completed|failed",
  "current_step": "intake|router|data_collection|synthesis|review|recommendations|proposal|approval|delivery",
  "started_at": "2026-08-02T11:00:00Z",
  "steps_completed": ["intake", "router"],
  "cost_running": 0.0,
  "human_gates": {
    "approval_gate": {
      "status": "pending|approved|rejected|timed_out",
      "decided_at": null
    }
  },
  "vetoes": []
}
```

---

## Cost Controls

### Per-Step Budgets

| Step | Max Cost | Typical |
|------|----------|---------|
| Intake | £0.10 | £0.05 |
| Router | £0.05 | £0.02 |
| Data Collection (×5) | £0.50 | £0.25 |
| Synthesis | £0.25 | £0.15 |
| Review | £0.10 | £0.05 |
| Recommendations | £0.25 | £0.15 |
| Proposal | £0.10 | £0.05 |
| **Total** | **£50.00 hard cap** | **~£0.86** |

### Veto Triggers

- **Cost Veto:** If `cost_running > £50`, halt immediately and alert Colin
- **Failure Veto:** If 3+ steps fail, halt and alert Colin
- **Timeout Veto:** If any human gate times out, log and hold

---

## Anti-Patterns to Avoid

1. **God Node:** Don't try to do intake + analysis + writing in one pass. Each step is a separate node.
2. **Hidden Router:** Don't let the synthesis node change audit type. Router decides, everything else follows.
3. **Infinite Loop:** Max 1 revision on review. If still failing, escalate to Colin.
4. **State Dump:** Each step only reads what it needs. Don't pass the entire audit directory to every node.
5. **Missing Default:** Router must always have a fallback (`full_system` if unclear).

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

*Version: 1.0*  
*Framework: Graph Engineering Design Framework v1.0*
