# ICM Ralph Execution Engine

**Autonomous stage execution within Interpretable Context Methodology workflows.**

Run multi-step build/creative/analytical stages autonomously using fresh-context subagents, append-only learning logs, and mandatory quality gates.

---

## What This Is

Ralph is an autonomous execution loop where each task iteration spawns a **fresh AI instance** with clean context. Memory persists through the filesystem — git history, append-only `learnings.md`, and stage contracts. No context window bloat. No "remember what I said 50 messages ago."

This skill integrates Ralph's execution model into ICM's layered context structure.

**Source inspiration:** [snarktank/ralph](https://github.com/snarktank/ralph) (Geoffrey Huntley's pattern)

---

## When to Use

Use ICM Ralph Execution when:
- A stage has **3+ sub-tasks** that can be executed independently
- Tasks involve **coding, writing, or analysis** that benefits from fresh context
- Quality gates (tests, checklists, review) can be automated
- The stage output feeds into subsequent stages (Layer 4 input)

Do NOT use when:
- The task requires **real-time human steering** at every step
- A single conversation thread is sufficient (simple tasks)
- Quality gates require subjective human judgment (creative review, brand voice)

---

## Prerequisites

- Stage directory exists: `stages/{nn}-{name}/`
- Stage `CONTEXT.md` defines the contract (inputs, process, outputs)
- `_config/` has reference materials (brand, voice, conventions)
- Git repository initialized in workspace

---

## Execution Loop

```
┌─────────────────────────────────────────────────────────────┐
│  STAGE START                                                │
│  Read: CONTEXT.md (Layer 2) + references/ (Layer 3)         │
└─────────────────────────────────────────────────────────────┘
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  1. DECOMPOSE                                               │
│  Break stage into sub-tasks fitting in ONE context window   │
│  Save: stage-contract.json                                  │
└─────────────────────────────────────────────────────────────┘
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  2. ITERATE (for each uncompleted sub-task)                 │
│                                                             │
│  a. SPAWN — Fresh subagent via sessions_spawn               │
│     ├─ Load: SOUL.md + relevant Layer 3 files              │
│     ├─ Load: output/ from prior sub-tasks (Layer 4)        │
│     └─ Load: learnings.md (append-only context)            │
│                                                             │
│  b. EXECUTE — Subagent performs the task                   │
│     ├─ Write output to stage output/ directory             │
│     └─ Append findings to learnings.md                     │
│                                                             │
│  c. QUALITY GATE — Run automated checks                    │
│     ├─ If coding: typecheck, tests, lint                   │
│     ├─ If content: checklist, word count, link check       │
│     └─ If analysis: data validation, source check          │
│                                                             │
│  d. COMMIT — Only if quality gate passes                   │
│     ├─ git commit with conventional commit format          │
│     └─ Mark sub-task complete in stage-contract.json       │
│                                                             │
│  e. RETRY — If quality gate fails                          │
│     ├─ Append failure notes to learnings.md                │
│     └─ Re-spawn with error context (max 3 retries)         │
└─────────────────────────────────────────────────────────────┘
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  3. FINALIZE                                                │
│  ├─ Run stage audit checklist                              │
│  ├─ Update references/ with discovered patterns            │
│  ├─ Append final learnings to global .learnings/           │
│  └─ Mark stage complete                                    │
└─────────────────────────────────────────────────────────────┘
```

---

## File Structure

```
stages/
  03-build-landing-page/
    CONTEXT.md                 # Layer 2: stage contract
    stage-contract.json        # Machine-readable task list
    quality-gate.md            # Automated checklist
    references/                # Layer 3: stage-specific rules
      component-patterns.md
      color-usage.md
    output/                    # Layer 4: working artifacts
      hero-component.tsx
      cta-section.tsx
      learnings.md             # Append-only learning log
```

---

## Stage Contract Format

`stage-contract.json` — machine-readable task tracker:

```json
{
  "stageName": "build-landing-page",
  "branchName": "feat/landing-page",
  "qualityGate": "quality-gate.md",
  "subTasks": [
    {
      "id": "lp-001",
      "title": "Build Hero section with CTA",
      "agent": "frontend-developer",
      "acceptance": [
        "Hero renders above fold on desktop and mobile",
        "CTA button links to /assessment",
        "Uses brand colors from _config/brand.md"
      ],
      "estimatedTokens": 15000,
      "passes": false,
      "attempts": 0,
      "notes": ""
    },
    {
      "id": "lp-002",
      "title": "Build social proof testimonials section",
      "agent": "frontend-developer",
      "acceptance": [
        "3 testimonial cards in responsive grid",
        "Each card has quote, avatar, name, title",
        "Uses testimonial data from output/testimonials.json"
      ],
      "estimatedTokens": 12000,
      "passes": false,
      "attempts": 0,
      "notes": ""
    }
  ]
}
```

---

## Quality Gate Format

`quality-gate.md` — checklist run after each sub-task and at stage completion:

```markdown
# Quality Gate: Build Landing Page

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
- [ ] Visual inspection passed (screenshot if UI)
- [ ] learnings.md has at least 3 entries
- [ ] References updated with new patterns
```

---

## Learnings Format

`output/learnings.md` — append-only, timestamped:

```markdown
# Learnings: Build Landing Page Stage

## 2026-09-01 10:23 — Hero CTA Pattern
The primary CTA should use the `Button` component from `ui/button.tsx`, not raw `<button>`.
Variant: `default` for primary, `outline` for secondary.
Always wrap in `Link` from next/link for client-side nav.

## 2026-09-01 11:45 — Testimonial Grid Responsive
3-column grid (`grid-cols-1 md:grid-cols-3`) works for testimonials.
Card height varies with content — use `items-stretch` on grid.
Avatar images: 64x64px, rounded-full, object-cover.

## 2026-09-01 12:10 — Color Token Issue
Discovered that `bg-brand` tailwind class not generating.
Fix: Added `brand: '#1a1a2e'` to tailwind.config.js colors.
Must restart dev server after config changes.
```

---

## Subagent Spawning Pattern

Use `sessions_spawn` for each sub-task:

```javascript
// Example: Spawn a frontend developer subagent
{
  "task": "Build Hero section per stage-contract.json sub-task lp-001. " +
          "Read: stages/03-build-landing-page/references/component-patterns.md, " +
          "stages/03-build-landing-page/output/learnings.md, " +
          "_config/brand.md. " +
          "Write output to: stages/03-build-landing-page/output/hero-component.tsx. " +
          "Append any learnings to: stages/03-build-landing-page/output/learnings.md.",
  "runtime": "subagent",
  "mode": "run",
  "timeoutSeconds": 300,
  "thinking": "medium"
}
```

**Critical rules for subagents:**
1. **Load Layer 3 references** — Every subagent MUST read relevant reference files first
2. **Load Layer 4 prior output** — Subagent sees output from completed sub-tasks
3. **Load learnings.md** — Subagent sees what previous iterations discovered
4. **Write to output/** — Never write outside the stage directory
5. **Append to learnings.md** — Never overwrite; always append

---

## Agent Selection

Each sub-task declares which agent persona executes it. Use `revenue-systems-agency` skill for agent definitions.

| Task Type | Agent | Why |
|-----------|-------|-----|
| React/UI component | Frontend Developer | Component implementation, performance |
| API endpoint | Backend Architect | API design, database, scalability |
| LinkedIn post | LinkedIn Content Creator | Personal brand, B2B voice |
| SEO audit | SEO Specialist | Technical SEO, content optimization |
| Outreach sequence | Outbound Strategist | Signal-based prospecting |
| Data analysis | Analytics Reporter | KPI tracking, insights |
| Code review | Code Reviewer | Security, maintainability |
| Research | Trend Researcher | Market intel, competitive analysis |

---

## Retry Logic

```
if quality_gate fails:
  attempts += 1
  if attempts >= 3:
    mark "needs-human-review"
    append to learnings.md: "FAILED after 3 attempts: {reason}"
    break — escalate to parent session
  else:
    append to learnings.md: "ATTEMPT {N} FAILED: {reason}"
    respawn with error context
```

**Never silently fail.** Failed quality gates are learning opportunities.

---

## Git Conventions

Every passing sub-task gets a commit:

```
feat(landing-page): add Hero section with CTA

- Implements lp-001 from stage-contract.json
- Uses Button component with next/link
- Responsive: mobile-first, md breakpoint

Quality gate: PASS
```

Stage completion gets a final commit:

```
feat(landing-page): complete landing page build stage

- All 5 sub-tasks pass quality gate
- Responsive hero, social proof, features, CTA, footer
- 7 learnings captured in output/learnings.md

Closes stage 03-build-landing-page
```

---

## Integration with ICM Layers

| Ralph Element | ICM Layer | File |
|--------------|-----------|------|
| prd.json | Layer 2 | `stage-contract.json` |
| progress.txt | Layer 4 | `output/learnings.md` |
| AGENTS.md updates | Layer 3 | `references/` updates |
| Quality checks | Layer 2 | `quality-gate.md` |
| Git history | Layer 4 | `.git/` (implicit) |
| Fresh context | Runtime | `sessions_spawn` |

---

## Example: Full Stage Execution

### Scenario: Build pricing page

**Stage directory:** `stages/04-build-pricing-page/`

**Step 1 — Parent reads contract:**
```
Read stages/04-build-pricing-page/CONTEXT.md
Read stages/04-build-pricing-page/stage-contract.json
```

**Step 2 — Decompose:** (Already done in contract)
- Sub-task pp-001: Pricing tier cards (3 tiers)
- Sub-task pp-002: FAQ accordion section
- Sub-task pp-003: CTA banner

**Step 3 — Iterate:**

**Iteration 1 — pp-001: Pricing tier cards**
```
sessions_spawn: {
  task: "Build pricing tier cards per pp-001. Read references/pricing-patterns.md, _config/brand.md, output/learnings.md. Write to output/pricing-cards.tsx. Append learnings.",
  runtime: "subagent",
  timeout: 300
}
```
Subagent returns → output/pricing-cards.tsx written.
Quality gate: Check file exists, compiles, uses brand colors, no placeholders.
→ PASS → git commit → mark pp-001 passes: true

**Iteration 2 — pp-002: FAQ accordion**
```
sessions_spawn: {
  task: "Build FAQ accordion per pp-002. Read references/component-patterns.md, _config/brand.md, output/learnings.md (has pp-001 learnings). Write to output/faq-section.tsx."
}
```
Quality gate: Check accordion opens/closes, responsive, accessible.
→ PASS → git commit → mark pp-002 passes: true

**Iteration 3 — pp-003: CTA banner**
Similar pattern. Uses learnings from pp-001 and pp-002.

**Step 4 — Finalize:**
- Run stage quality gate (all sub-tasks pass, visual inspection, no errors)
- Update references/pricing-patterns.md with discovered patterns
- Append final learnings to global `.learnings/pricing-pages.md`
- Final commit
- Mark stage complete

---

## Anti-Patterns to Avoid

| Anti-Pattern | Why It Fails | Fix |
|-------------|--------------|-----|
| One subagent for entire stage | Context window bloat, poor output | Decompose into sub-tasks |
| Overwriting learnings.md | Loses prior context | Append-only |
| Skipping quality gate | Compounding errors | Mandatory before commit |
| Subagent writes outside stage/ | Breaks isolation | Enforce output/ directory |
| No git commits between sub-tasks | Hard to debug | Commit every pass |
| Loading full MEMORY.md in subagent | Wastes tokens | Load only relevant layers |
| Human review inside loop | Defeats autonomy | Human reviews between stages, not sub-tasks |

---

## Comparison: Ralph vs ICM Ralph

| Aspect | Original Ralph | ICM Ralph |
|--------|---------------|-----------|
| Trigger | PRD → prd.json | Stage CONTEXT.md → stage-contract.json |
| Context persistence | progress.txt | learnings.md + full ICM layers |
| Execution | Bash script | OpenClaw sessions_spawn |
| AI tool | Amp / Claude Code | Kimi (kimi-coding/k2p6) |
| Scope | Code only | Code, content, analysis, research |
| Quality gates | typecheck, tests | Custom per-stage checklists |
| Memory | Git + progress.txt | Git + learnings.md + references/ |
| Multi-client | No | Yes (via `clients/{client}/` isolation) |

---

*Integrates with: `icm-core`, `revenue-systems-agency`, `pre-deployment-checklist`*
