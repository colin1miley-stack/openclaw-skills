# Audit Pipeline — Full Workflow Steps

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

**Output:** See output-templates.md for INTAKE.md template.

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

See output-templates.md for FINDINGS.md template.

---

## Step 5: Quality Review

**Goal:** Validate findings against rubric before proceeding.

See references/quality-rubric.md for full rubric and scoring logic.

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
