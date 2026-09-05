---
name: icm-core
description: Interpretable Context Methodology — the 5-layer context model for agent workflow orchestration. All multi-stage work follows this standard. Reference before any complex task decomposition.
---

# ICM Core — Interpretable Context Methodology

**Standard:** Jake Van Clief & David McDermott, arXiv:2603.16021 (March 2026)
**Complement:** Google Open Knowledge Format v0.2 (July 2026)
**Adopted:** 2026-09-05 by Colin Miley Revenue Systems

---

## The Five-Layer Context Model

```
Layer 0: SOUL.md / CLAUDE.md           ← Always loaded (identity, boundaries)
Layer 1: CONTEXT.md (root)             ← Routes request to stage
Layer 2: Stage CONTEXT.md              ← Task contract (inputs, process, outputs)
Layer 3: references/, _config/, skills/ ← Stable reference material
Layer 4: output/ from prior stages     ← Working artifacts
```

**Van Clief's terminology:** Layer 3 = "factory" (persistent constraints). Layer 4 = "product" (current run artifacts).

---

## When to Use ICM

**Use ICM when:**
- A task has **3+ sub-tasks** that can be executed independently
- Tasks involve coding, writing, or analysis that benefits from fresh context
- Quality gates (tests, checklists, review) can be automated
- Stage output feeds into subsequent stages (Layer 4 input)
- Human review is needed at stage boundaries

**Skip ICM when:**
- Single conversation thread is sufficient (simple tasks)
- Real-time human steering is needed at every step
- Quality gates require subjective human judgment (creative review, brand voice)

---

## File Structure (Standard)

```
workspace/
├── SOUL.md                          # Layer 0: Identity, boundaries
├── AGENTS.md                        # Layer 0: Security, conventions
├── CONTEXT.md                       # Layer 1: Stage routing
├── _config/                         # Layer 3: Persistent reference
│   ├── voice.md                     # Brand voice, tone rules
│   ├── conventions.md               # Coding/content conventions
│   ├── design-system.md             # UI/UX conventions
│   └── quality-gates.md             # Reusable checklists
├── skills/                          # Layer 3: Agent skills
│   └── icm-core/SKILL.md            # This file
├── stages/                          # Layer 2: Active work
│   └── NN-{name}/
│       ├── CONTEXT.md               # Stage contract
│       ├── stage-contract.json      # Machine-readable tasks
│       ├── quality-gate.md          # Per-stage checklist
│       ├── references/              # Stage-specific Layer 3
│       └── output/                  # Layer 4: Working artifacts
│           └── learnings.md         # Append-only log
├── memory/                          # Layer 4: Daily logs
├── clients/                         # Layer 4: Client work
└── .learnings/                      # Layer 4: Global patterns
```

---

## Layer 0: Identity (Always Loaded)

**Files:** `SOUL.md`, `AGENTS.md`, `USER.md`, `IDENTITY.md`

**Content:** Who the agent is, who it serves, security rules, conventions.

**Rule:** These are loaded at the start of EVERY session. Updates require explicit notification to the user.

---

## Layer 1: Routing (Root CONTEXT.md)

**File:** `CONTEXT.md` (workspace root)

**Purpose:** Routes user requests to the appropriate workflow or stage. Contains:
- List of active stages
- Stage descriptions and triggers
- Current priority

**Example:**
```markdown
---
type: workspace-router
updated: 2026-09-05
---

# Workspace Router

## Active Stages
| Stage | Status | Trigger |
|-------|--------|---------|
| 01-audit-deliverable | 🔴 Complete | Audit client: Acme Corp |
| 02-site-refresh | 🟡 Active | Brand update, pricing change |
| 03-signal-voice | 🟡 Active | ElevenLabs voice integration |

## Priority
1. Complete site-refresh (pricing page)
2. Begin Signal Phase 1 (text + image + audio)
3. Security audit with Strix

## How to Route
- "Audit" or "deliverable" → stages/01-audit-deliverable/
- "Site" or "pricing" → stages/02-site-refresh/
- "Signal" or "content" or "voice" → stages/03-signal-voice/
```

---

## Layer 2: Stage Contract

**File:** `stages/NN-{name}/CONTEXT.md`

**Purpose:** Defines the stage's inputs, process, and outputs.

**Required sections:**
```markdown
---
type: workflow-stage
stage: 02
title: Site Refresh
status: active
---

## Inputs
- File A from previous stage
- Reference file B from _config/

## Process
1. Step one
2. Step two
3. Step three

## Outputs
- Output file X written to output/
- Learning appended to output/learnings.md

## Quality Gate
- [ ] Check A
- [ ] Check B
```

---

## Stage Contract (Machine-Readable)

**File:** `stages/NN-{name}/stage-contract.json`

**Purpose:** Machine-readable task tracker for subagent orchestration.

```json
{
  "stageName": "site-refresh",
  "branchName": "feat/site-refresh",
  "qualityGate": "quality-gate.md",
  "subTasks": [
    {
      "id": "sr-001",
      "title": "Update hero copy",
      "agent": "frontend-developer",
      "acceptance": ["New headline renders", "CTA links correct"],
      "estimatedTokens": 8000,
      "passes": false,
      "attempts": 0,
      "notes": ""
    }
  ]
}
```

---

## Quality Gate Format

**File:** `stages/NN-{name}/quality-gate.md`

**Purpose:** Checklist run after each sub-task and at stage completion.

```markdown
# Quality Gate: {Stage Name}

## Per-SubTask Checks
- [ ] Output file(s) exist in output/
- [ ] File compiles (if code) / renders (if content)
- [ ] No placeholder text or TODOs remain
- [ ] Follows conventions in _config/
- [ ] learnings.md appended with key findings

## Stage Completion Checks
- [ ] All sub-tasks marked passes: true
- [ ] All output files committed to git
- [ ] No console errors (if web)
- [ ] learnings.md has at least 3 entries
- [ ] References updated with new patterns
```

---

## Layer 3: Reference Material

**Directories:** `_config/`, `skills/`, `references/`

**Rule:** Loaded on demand, not wholesale. Reference files contain persistent constraints:
- Brand voice, tone rules
- Coding conventions
- Design system specs
- Domain policies

**Anti-pattern:** Loading full MEMORY.md into subagent context. Load only relevant sections.

---

## Layer 4: Working Artifacts

**Directories:** `stages/NN-{name}/output/`, `memory/`, `clients/`, `.learnings/`

**Key files:**
- `output/learnings.md` — Append-only, timestamped
- `output/*.tsx`, `output/*.md` — Stage deliverables
- `.learnings/{topic}.md` — Cross-stage pattern library

**Rule:** Never overwrite `learnings.md`. Always append.

---

## Subagent Spawning Pattern

Use `sessions_spawn` for each sub-task:

```json
{
  "task": "Implement feature X. " +
          "STEP 1: Read stage contract at stages/02-site-refresh/stage-contract.json " +
          "STEP 2: Read references: _config/voice.md, _config/design-system.md " +
          "STEP 3: Read prior learnings: stages/02-site-refresh/output/learnings.md " +
          "STEP 4: Implement sub-task sr-001 " +
          "STEP 5: Append learnings to output/learnings.md " +
          "STEP 6: Run quality gate checklist",
  "runtime": "subagent",
  "mode": "run",
  "timeoutSeconds": 300,
  "thinking": "medium"
}
```

**Critical rules for subagents:**
1. Load Layer 3 references first
2. Load Layer 4 prior output
3. Load learnings.md
4. Write to output/ only
5. Append to learnings.md (never overwrite)

---

## Git Conventions

Every passing sub-task gets a commit:

```
feat(site-refresh): update hero copy

- Implements sr-001 from stage-contract.json
- New headline: "Stop Guessing. Start Fixing."
- CTA links to /assessment

Quality gate: PASS
```

Stage completion gets a final commit:

```
feat(site-refresh): complete site refresh stage

- All 3 sub-tasks pass quality gate
- Hero, pricing, footer updated
- 5 learnings captured in output/learnings.md

Closes stage 02-site-refresh
```

---

## Converting Skills to ICM Format

When pulling a skill from external source (GitHub, skills.sh, etc.):

**Step 1:** Read SKILL.md fully — vet for safety
**Step 2:** Add ICM layer mapping section:
```markdown
## ICM Layer Mapping

| Layer | File | Usage |
|-------|------|-------|
| Layer 2 | This SKILL.md | Task contract when triggered |
| Layer 3 | `skills/{name}/` | Reference material |
| Layer 4 | `output/` from caller | Working artifacts |
```
**Step 3:** Add stage contract template if skill involves multi-step work
**Step 4:** Add quality gate checklist
**Step 5:** Install to `skills/` directory (not global)
**Step 6:** Register in `AGENTS.md` skill registry

---

## OKF Integration (Google Open Knowledge Format)

For knowledge bundles (prospects, playbooks, metrics):

```markdown
---
type: metric
title: Weekly active users
description: Count of distinct users active during a calendar week
tags:
  - product
  - engagement
generated:
  by: agent:enrichment-agent
  at: 2026-09-01
verified:
  - by: human:colin
    at: 2026-09-02
status: stable
---

# Weekly active users

[Content body]

## Related concepts
- [Events table](../tables/events.md)
- [Monthly active users](monthly-active-users.md)
```

**OKF fields:**
- `type`: concept category
- `generated`: creation provenance
- `verified`: human review trail
- `status`: draft | stable | deprecated
- `stale_after`: freshness threshold

---

## Anti-Patterns

| Anti-Pattern | Why It Fails | Fix |
|-------------|--------------|-----|
| One subagent for entire stage | Context window bloat | Decompose into sub-tasks |
| Overwriting learnings.md | Loses prior context | Append-only |
| Skipping quality gate | Compounding errors | Mandatory before commit |
| Subagent writes outside stage/ | Breaks isolation | Enforce output/ directory |
| No git commits between sub-tasks | Hard to debug | Commit every pass |
| Loading full MEMORY.md in subagent | Wastes tokens | Load only relevant layers |

---

## Metrics

| Metric | Target | How |
|--------|--------|-----|
| Context per stage | <8,000 tokens | Decomposition + selective loading |
| Git commits per stage | 1 per sub-task | Automated on quality gate pass |
| Learnings per stage | ≥3 entries | Mandatory in quality gate |
| Stage completion rate | 100% with passing gates | Retry logic (max 3) |

---

*Integrates with: `icm-ralph-execution`, `graphify-icm`, `subagent-team-execution`*
