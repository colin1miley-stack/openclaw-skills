---
name: subagent-team-execution
description: Builder/Validator subagent team pattern adapted from disler's Claude Code Hooks Mastery. Use for complex tasks requiring implementation + verification. Spawns fresh-context subagents with different tool permissions.
---

# Subagent Team Execution

**Adapted from:** `disler/claude-code-hooks-mastery` (Claude Code Hooks Mastery)
**Pattern:** Builder/Validator agent teams with quality gates
**For:** OpenClaw `sessions_spawn` runtime

---

## What This Is

A two-agent pattern where a **Builder** implements and a **Validator** verifies — increasing compute to increase trust. Each agent runs in a fresh context window with only the tools and references it needs.

```
Parent Session (Orchestrator)
    ├── Builder Subagent (all tools) → Implements
    └── Validator Subagent (read-only) → Verifies
```

---

## When to Use

**Use when:**
- Task has clear acceptance criteria
- Code quality matters (linting, type checking, tests)
- Output must match a spec or contract
- Risk of error is high (production code, client deliverables)

**Skip when:**
- Task is exploratory or creative (subjective judgment)
- Speed matters more than correctness (prototyping)
- Simple enough for single-agent execution

---

## Agent Definitions

### Builder Agent

```json
{
  "task": "Implement {feature} per stage-contract.json sub-task {id}. " +
          "Read: references/, _config/, output/learnings.md. " +
          "Write output to: stages/NN-{name}/output/. " +
          "Append learnings. Run tests if available.",
  "runtime": "subagent",
  "mode": "run",
  "timeoutSeconds": 600,
  "thinking": "high"
}
```

**Permissions:** All tools (Read, Write, Edit, exec, browser, etc.)
**Responsibility:** Implement the feature, write files, run commands

### Validator Agent

```json
{
  "task": "Validate builder's work against acceptance criteria. " +
          "Read only: output files, stage-contract.json, quality-gate.md. " +
          "DO NOT write or edit files. " +
          "Report: PASS / FAIL with specific issues.",
  "runtime": "subagent",
  "mode": "run",
  "timeoutSeconds": 300,
  "thinking": "medium"
}
```

**Permissions:** Read-only (no Write, no Edit, no destructive exec)
**Responsibility:** Check against acceptance criteria, report issues

---

## Execution Flow

```
1. Parent reads stage-contract.json
2. Parent spawns BUILDER with task + references
3. Builder writes output files
4. Parent spawns VALIDATOR with builder output + criteria
5. Validator reports PASS or FAIL with specifics
6. If PASS:
   - Parent commits: git commit -m "feat(stage): sub-task X"
   - Mark sub-task complete in stage-contract.json
7. If FAIL:
   - Append failure notes to output/learnings.md
   - Retry (max 3 attempts)
   - Escalate to parent on 3rd failure
```

---

## Quality Gate Integration

### Code Quality Validators (OpenClaw Adaptation)

| Validator | Trigger | Tool | Action |
|-----------|---------|------|--------|
| **Ruff** | `.py` file Write/Edit | `exec: ruff check {file}` | Blocks on lint errors |
| **Type checker** | `.py/.ts` file Write/Edit | `exec: ty check {file}` or `npx tsc --noEmit` | Blocks on type errors |
| **Tests** | Code change | `exec: npm test` or `pytest` | Blocks on test failures |
| **Link check** | Markdown output | `exec: markdown-link-check` | Blocks on broken links |

**OpenClaw implementation:** Use `exec` tool after builder completes, before spawning validator. Or spawn validator that runs these checks.

### Example: Python Code Validation

```javascript
// After builder writes .py file:
{
  "task": "Run ruff and type checker on {file}. " +
          "If any errors, report them without fixing. " +
          "Builder will handle fixes.",
  "runtime": "subagent",
  "mode": "run",
  "timeoutSeconds": 120
}
```

---

## Stage Contract with Team Orchestration

```json
{
  "stageName": "signal-phase-1",
  "branchName": "feat/signal-phase-1",
  "qualityGate": "quality-gate.md",
  "team": {
    "builder": {
      "tools": ["all"],
      "timeout": 600
    },
    "validator": {
      "tools": ["read", "exec"],
      "timeout": 300
    }
  },
  "subTasks": [
    {
      "id": "s1-001",
      "title": "Integrate ElevenLabs voice synthesis",
      "agent": "builder",
      "validator": "code-reviewer",
      "acceptance": [
        "Voice clone ID 7l4jPe6wAD9VL2TQqoN9 loads correctly",
        "MP3 output saves to output/audio/",
        "Error handling for API failures"
      ],
      "estimatedTokens": 15000,
      "passes": false,
      "attempts": 0,
      "notes": ""
    }
  ]
}
```

---

## OpenClaw-Specific Adaptations

### What's Different from Claude Code

| Feature | Claude Code | OpenClaw |
|---------|-------------|----------|
| Hooks | 13 lifecycle hooks (UserPromptSubmit, PreToolUse, etc.) | ❌ Not available |
| Subagent spawning | `Task` tool | ✅ `sessions_spawn` |
| Permission control | Hook-level blocking | ⚠️ Tool policy profiles |
| TTS announcements | Built-in hook | ✅ `tts` tool after spawn |
| Status lines | Terminal UI | ❌ Not available |
| Output styles | `/output-style` command | ⚠️ System prompt modification |

### What We Keep

- **Builder/Validator split** — Core pattern, fully transferable
- **Quality gates** — Checklists + automated validators
- **Append-only learnings** — `learnings.md` pattern
- **Git per sub-task** — Conventional commits
- **Stage contracts** — `stage-contract.json` tracking
- **Fresh context** — `sessions_spawn` with clean windows

### What We Adapt

- **No hooks** — Use explicit `exec` validation steps instead
- **Tool restriction** — Use task description + trust (no true sandbox)
- **TTS** — Call `tts` tool after subagent completes
- **Status** — Log to `output/learnings.md` instead of terminal UI

---

## Example: Signal Phase 1 with Team Execution

### Scenario: Integrate ElevenLabs voice

**Stage:** `stages/03-signal-voice/`

**Step 1 — Parent reads contract:**
```
Read stages/03-signal-voice/stage-contract.json
→ Sub-task s1-001: Integrate ElevenLabs
```

**Step 2 — Spawn Builder:**
```javascript
sessions_spawn({
  task: "Integrate ElevenLabs API for voice synthesis. " +
        "Read: stages/03-signal-voice/references/elevenlabs-api.md, " +
        "_config/voice.md (ElevenLabs Voice ID: 7l4jPe6wAD9VL2TQqoN9). " +
        "Write: stages/03-signal-voice/output/elevenlabs-integration.mjs " +
        "and stages/03-signal-voice/output/test-voice.mp3. " +
        "Append learnings.",
  runtime: "subagent",
  mode": "run",
  timeoutSeconds: 600
})
```

**Step 3 — Builder returns:**
- Files written: `elevenlabs-integration.mjs`, `test-voice.mp3`
- Learnings appended

**Step 4 — Run automated validators:**
```javascript
exec({ command: "node stages/03-signal-voice/output/elevenlabs-integration.mjs --test" })
```

**Step 5 — Spawn Validator:**
```javascript
sessions_spawn({
  task: "Validate ElevenLabs integration. " +
        "Read: stages/03-signal-voice/output/elevenlabs-integration.mjs " +
        "and acceptance criteria from stage-contract.json. " +
        "Check: API key not hardcoded, error handling present, " +
        "voice ID correct, output path correct. " +
        "Report PASS or FAIL with specifics.",
  runtime: "subagent",
  mode: "run",
  timeoutSeconds: 300
})
```

**Step 6 — Validator reports:** PASS

**Step 7 — Parent commits:**
```bash
git add stages/03-signal-voice/output/
git commit -m "feat(signal): integrate ElevenLabs voice synthesis

- Implements s1-001 from stage-contract.json
- Voice clone ID: 7l4jPe6wAD9VL2TQqoN9
- Test MP3 generated successfully
- Error handling for API failures

Quality gate: PASS"
```

---

## Anti-Patterns

| Anti-Pattern | Why It Fails | Fix |
|-------------|--------------|-----|
| Validator edits files | Breaks separation of concerns | Validator reports only, builder fixes |
| No automated checks | Manual validation is slow/error-prone | Add exec validators before human review |
| Builder skips learnings | Lost context for next iteration | Mandatory in quality gate |
| One agent does everything | Context bloat, no verification | Split builder/validator |
| No timeout on subagents | Hanging processes | Always set timeoutSeconds |

---

## Metrics

| Metric | Target |
|--------|--------|
| Builder success rate | >90% (first attempt) |
| Validator catch rate | >95% of real issues |
| Retry rate | <20% |
| Escalation rate | <5% (3 failures) |
| Time per sub-task | <10 min (builder + validator) |

---

*Integrates with: `icm-core`, `icm-ralph-execution`*
