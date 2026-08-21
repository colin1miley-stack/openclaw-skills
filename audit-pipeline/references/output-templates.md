# Audit Pipeline — Output Templates

## INTAKE.md Template

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

---

## FINDINGS.md Template

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

## State Schema (state.json)

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

## Approval Gate Interface Template

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
