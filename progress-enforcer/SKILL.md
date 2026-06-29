# progress-enforcer — Aggressive Progress Reporting

## Description

Force the agent to provide frequent, specific, context-aware progress updates on ANY task that takes longer than 60 seconds. This skill is non-negotiable. Silent agents are unacceptable. Every tool call, every long operation, every multi-step process SHALL be accompanied by explicit progress communication.

## Activation Rule

**Auto-trigger on ALL tasks estimated to exceed 60 seconds.**
No exceptions. No opt-out. No "this is quick enough." If a task involves multiple tool calls, file reads, web searches, code generation, or any multi-step operation, this skill is ACTIVE.

## Mandatory Checkpoints — REQUIRED

The agent SHALL emit a progress update at each of the following checkpoints. Failure to comply is a violation of this skill.

### 1. START — Within 10 seconds of beginning the task

**Format:**
```
Starting [task name]. ETA ~X minutes.
Plan: [brief step-by-step outline].
```

**Example:**
```
Starting competitor website audit (5 sites). ETA ~4 minutes.
Plan: 1) Fetch site 1 → 2) Extract pricing data → 3) Compile comparison table.
```

**Rules:**
- MUST include task name and rough ETA.
- MUST include a brief plan (at least 2 bullets).
- MUST be the FIRST thing the agent outputs after accepting the task.

### 2. 2-MINUTE MARK — Every 2 minutes of elapsed time

**Format:**
```
Still working — [specific current step]. ~Y min remaining.
Status: [concrete detail about what was just completed and what is next].
```

**Example (good):**
```
Still working — reading site 3 of 5 (nutriburst.com). ~2 min remaining.
Status: Extracted pricing from 2 sites. Currently parsing product pages.
```

**Example (UNACCEPTABLE — WILL BE REJECTED):**
```
Still working...
```

**Rules:**
- MUST specify the exact step being executed.
- MUST quantify progress ("file 3 of 5", "batch 2 of 4", "40% complete").
- MUST NOT use vague placeholders like "still working" or "in progress" without specifics.
- SHALL count elapsed time from task start. If the agent loses track, it SHALL estimate and err on the side of over-communication.

### 3. COMPLETION — Immediately upon finishing

**Format:**
```
Done: [one-line summary of what was accomplished].
[Optional: key findings or next steps in ≤2 sentences].
```

**Example:**
```
Done: Audited 5 competitor sites — pricing ranges £24-£49/month, 3 offer subscriptions, 2 use sachet format.
Next: Recommend positioning based on gap analysis.
```

**Rules:**
- MUST be a single, informative sentence.
- MUST NOT be vague ("Done" or "Finished" without detail).
- MUST follow immediately after the last tool call / action.

### 4. STUCK — If any step takes >3x expected time or loops

**Format:**
```
Stuck on [step] — taking longer than expected.
What happened: [brief explanation].
Options: [A/B/C] or ask for direction.
```

**Example:**
```
Stuck on fetching site 4 — connection timeout after 3 retries.
What happened: The site blocks automated requests; no data available.
Options: A) Skip this site and note the gap, B) Try alternative source, C) Ask you for a different approach.
```

**Rules:**
- MUST NOT silently retry more than 3 times without declaring "Stuck."
- MUST admit the problem explicitly.
- MUST present options or ask the user for direction.
- MUST NOT loop indefinitely pretending progress is being made.

## Enforcement Mechanism — COMPLIANCE CHECK

Before EVERY response the agent generates during a long-running task, it SHALL run this internal compliance check:

```
PROGRESS COMPLIANCE CHECK:
1. Has it been >2 minutes since my last progress update? → If YES, emit update NOW.
2. Did I just complete a major step? → If YES, announce completion of that step.
3. Am I about to start a new step? → If YES, announce what it is.
4. Have I retried the same action >2 times? → If YES, declare STUCK and ask user.
5. Is my last message vague ("still working" without specifics)? → If YES, rewrite with concrete detail.
```

**This check is MANDATORY. It SHALL be run mentally before every response. The agent MUST NOT skip it.**

## Context-Aware Updates — SPECIFICITY REQUIRED

The agent SHALL describe WHAT it is doing, not merely THAT it is working.

| ❌ UNACCEPTABLE | ✅ REQUIRED |
|-----------------|-------------|
| "Still working..." | "Reading file 3 of 5 (SKILL.md)..." |
| "In progress..." | "Processing batch 2, 40% complete (8 of 20 items done)..." |
| "Just a moment..." | "Compiling results — 3 of 4 sections written, final section: references..." |
| "Almost done..." | "Finalising — writing output to disk, 2 files remaining..." |
| "Processing..." | "Searching web for supplier MOQs — query 2 of 3 executed, awaiting results..." |

**Rules:**
- Every update MUST include a countable metric (file N of M, X% complete, step Y of Z).
- Every update MUST name the current operation (reading, writing, searching, parsing, compiling, etc.).
- If the agent cannot quantify, it SHALL estimate and say so ("Approximately halfway through manual review...").

## Failure Mode — Loop Detection & Escalation

If the agent detects it may be stuck in a loop, the following protocol is MANDATORY:

### Loop Detection Criteria (any one triggers escalation):
- Same tool call made >3 times with identical parameters.
- Same error encountered >3 times without resolution.
- No forward progress for >5 minutes (same step, no new output).
- Repeated "Still working" messages with identical wording.

### Escalation Protocol — REQUIRED ACTIONS:
1. **ADMIT:** State explicitly: "I appear to be stuck in a loop on [step]."
2. **EXPLAIN:** Briefly describe what was attempted and why it failed.
3. **ASK:** Present 2-3 clear options or ask the user for direction.
4. **DO NOT:** Silently retry, change parameters randomly, or pretend progress is happening.

**Example:**
```
I appear to be stuck in a loop trying to fetch the pricing page.
Attempted: Direct fetch → textise.it → cached version. All returned 403.
Options: A) Skip pricing data and flag gap, B) Use manual summary from memory, C) Try a different URL if you have one.
What would you like me to do?
```

## Timing & Cadence

| Elapsed Time | Required Action |
|--------------|-----------------|
| 0-10s | Emit START message with plan and ETA. |
| 2:00 | Emit 2-MINUTE update with specific step and progress count. |
| 4:00 | Emit 4-MINUTE update with specific step and progress count. |
| 6:00+ | Emit updates every 2 minutes. If >6 min, also reassess ETA and notify user if significantly off. |
| At each step completion | Emit brief completion notice for that step. |
| At final completion | Emit DONE message with one-line summary. |
| If stuck >3 min on one step | Emit STUCK message with explanation and options. |

## Tone

Progress updates SHALL be:
- **Direct:** No fluff, no "Great question!", no filler.
- **Informative:** Concrete details, counts, names of files/sites/steps.
- **Honest:** If behind schedule, say so. If stuck, admit it. If ETA was wrong, correct it.
- **Respectful of user's time:** The user should never wonder "Is it still working?" or "Did it crash?"

## Examples — Full Task Flow

### Example A: Multi-File Read (3 min task)

```
[0:00] Starting: Audit 5 competitor SKUs. ETA ~3 minutes.
Plan: 1) Read site 1 → 2) Read site 2 → 3) Read site 3 → 4) Read site 4 → 5) Read site 5 → 6) Compile summary.

[0:45] Step 1/6 complete: Read site 1 (nutriburst.com) — 3 SKUs, £29-£39/month.

[1:30] Step 2/6 complete: Read site 2 (vitl.com) — 5 SKUs, £24-£49/month.

[2:00] Still working — reading site 3 of 5 (younique.com). ~1.5 min remaining.
Status: 2 sites parsed, 3 remaining. Currently extracting pricing tables.

[2:45] Step 3/6 complete: Read site 3 — 4 SKUs, £35-£45/month.

[3:30] Step 4/6 complete: Read site 4 — 2 SKUs, £30/month flat.

[4:00] Still working — reading final site (site 5 of 5). ~30 sec remaining.
Status: 4 sites done, 1 remaining. Then compiling summary.

[4:30] Step 5/6 complete: Read site 5 — 6 SKUs, £20-£55/month.

[4:45] Done: Audited 5 sites — 20 total SKUs, price range £20-£55/month, average £36/month.
```

### Example B: Web Search + Compilation (5 min task, gets stuck)

```
[0:00] Starting: Find 10 UK powder supplement manufacturers. ETA ~5 minutes.
Plan: 1) Search for manufacturers → 2) Visit top 10 results → 3) Extract contact info → 4) Compile table.

[2:00] Still working — visited 4 of 10 manufacturer sites. ~3 min remaining.
Status: Extracted contact info from 4 sites. Currently parsing site 5 (nutral.co.uk).

[4:00] Still working — visited 7 of 10 sites. ~2 min remaining.
Status: 7 contacts extracted. 3 sites remaining. Site 8 appears to be a distributor, not manufacturer — flagging.

[6:00] Still working — compiling final table from 9 valid manufacturers. ~1 min remaining.
Status: 9 of 10 targets confirmed. 1 was a distributor (excluded). Writing table now.

[7:00] Stuck on final site — connection timeout to manufacturer-10.co.uk after 3 attempts.
What happened: Site appears down or blocking requests. No cached version available.
Options: A) Publish table with 9 manufacturers and note the gap, B) Try a different search query for a 10th, C) Ask if you have an alternative source.
What would you like me to do?
```

## Final Rule

**If the user ever thinks "Is it still working?" or "Did it crash?" — this skill has FAILED.**
The agent SHALL be so verbose about progress that the user is NEVER left wondering. Over-communicate. Under-communicate NEVER.
