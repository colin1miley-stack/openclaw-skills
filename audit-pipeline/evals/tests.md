# Audit Pipeline — Self-Evaluation Tests

## Test 1: Intake Parsing

**Scenario:** Discovery notes are unstructured free text with no clear fields.

**Input:**
```
Had a call with Sarah from TechFlow yesterday. She's the VP of Sales. 
Team is 12 reps. They use Salesforce but it's a mess — nobody updates it. 
Main pain is forecasting is always wrong. They also struggle with LinkedIn 
outreach, reps spend hours manually connecting. Budget is confirmed, 
they've got £50K for sales ops this quarter. Timeline is flexible but 
want to start in September. sara@techflow.io
```

**Expected:**
- Extract: name=Sarah, company=TechFlow, role=VP of Sales, team_size=12
- Stack: CRM=Salesforce, Calendar=unknown, Email=unknown
- Pain points: forecasting accuracy, LinkedIn outreach efficiency
- Budget: confirmed, timeline: flexible, contact: sara@techflow.io
- Audit type detected: sales_process (confidence: high)

**Pass criteria:** All fields extracted correctly, audit type = sales_process

---

## Test 2: Router Classification

**Scenario:** Multiple pain points span multiple audit types.

**Input:**
```
Pain points:
1. Pipeline forecasting is inaccurate
2. No consistent LinkedIn content strategy
3. Lead qualification is ad-hoc
```

**Expected:**
- Audit type: full_system (pain spans sales_process, content_engine, lead_gen)
- Confidence: < 0.7
- Action: Ask Colin to confirm or default to full_system

**Pass criteria:** Router does NOT force a single type when pain is cross-cutting

---

## Test 3: Data Collection — Missing Export

**Scenario:** Client provides discovery notes but no CRM export, no calendar export, no email samples.

**Expected:**
- All 5 data collection nodes run
- Nodes without data document the gap and proceed with qualitative analysis
- FINDINGS.md includes "Data Quality Notes" section explaining gaps
- No node should error or stall

**Pass criteria:** Pipeline completes all steps, gaps documented, no errors

---

## Test 4: Quality Review — Borderline Score

**Scenario:** Findings have good evidence but poor quantification.

**Input:** 8 findings, 6 with evidence, 3 with £ impact, all actionable, all traceable.

**Expected:**
- Evidence-backed: 6/8 = 0.75 → weighted: 0.225
- Business impact: 3/8 = 0.375 → weighted: 0.094 (below 60% threshold)
- Actionable: 8/8 = 1.0 → weighted: 0.25
- No hallucinations: 8/8 = 1.0 → weighted: 0.20
- **Total: 0.769 → REJECT**

**Pass criteria:** Review correctly scores < 0.8 and rejects with specific feedback

---

## Test 5: Cost Veto

**Scenario:** Data collection step uses expensive reasoning (e.g., multiple kimi_search calls + sessions_spawn).

**Simulated state:**
```json
{
  "cost_running": 52.30,
  "cost_by_step": {
    "intake": 0.05,
    "router": 0.02,
    "data_collection": 51.98
  }
}
```

**Expected:**
- Cost veto triggers immediately
- Pipeline halts before synthesis
- Colin alerted with cost breakdown
- No further steps execute

**Pass criteria:** Pipeline halts, Colin alerted, state.json updated with veto flag
