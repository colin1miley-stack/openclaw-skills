# build-orchestrator

**Use when:** Building any deliverable — website, content, audit, tool, campaign.

**Mandatory:** Read this SKILL.md in full before spawning any subagent.

---

## Phase 0: Pre-Flight Checklist (MANDATORY)

Before spawning ANY subagent, you MUST complete this checklist. No exceptions.

- [ ] **Have I read all relevant SKILL.md files?**  
  Search `skills/` directory. Read any skill matching your task type.
- [ ] **Have I verified model IDs exist?**  
  Run `openclaw models list` or check the provider's API before using non-default models.
- [ ] **Have I defined success criteria?**  
  What does "done" look like? What format? What length? What quality bar?
- [ ] **Is the timeout appropriate?**  
  Quick tasks: 60–120s. Research: 180–300s. Multi-step: 300–600s.
- [ ] **Have I checked for stale data?**  
  Memory older than 7 days may be outdated. Verify key facts.

**If you skip any checkbox, stop. Read the relevant skill. Then proceed.**

---

## Phase 1: Context Loading (1–2 min)

1. Load SOUL.md, USER.md, MEMORY.md
2. Load any skill-specific context files
3. Check `memory/YYYY-MM-DD.md` for today's and yesterday's context
4. Load MEMORY.md for long-term memory

---

## Phase 2: Skill Selection

1. List all skills in `skills/` directory
2. Read any skill matching your task type
3. Note dependencies between skills

---

## Phase 3: Task Definition

1. Define the goal clearly
2. Define scope (what's in, what's out)
3. Define deliverables (format, location, naming)
4. Define acceptance criteria
5. Define time constraints

---

## Phase 4: Graph Definition

Map the dependency graph of sub-tasks. See references/graph-patterns.md for examples.

Rules:
- Mark which tasks can run in parallel
- Mark which tasks require human review
- Mark which tasks have external dependencies

---

## Phase 5: Execution

### Parallel Execution

Spawn independent tasks simultaneously:
```
Agent 1 (Research) ──┐
                     ├──→ [Synthesis]
Agent 2 (Analysis) ──┘
```

### Sequential Execution

Chain dependent tasks:
```
Agent 1 (Research) → Agent 2 (Writing) → Agent 3 (Review)
```

### Communication Protocol

Use `sessions_send` to pass context between agents:
```
sessions_send(sessionKey, "Context from Agent 1: ...")
```

---

## Phase 6: Quality Gates

Before marking a task complete, verify:

- [ ] Output matches acceptance criteria
- [ ] No placeholders or TODOs remain
- [ ] Format matches specification
- [ ] All files are in correct locations
- [ ] Git committed with descriptive message

---

## Phase 7: Delivery

1. Summarize what was built
2. List all files created/modified
3. Note any technical debt or follow-up items
4. Update MEMORY.md with key decisions

---

## Quick Reference

| When you need... | Read this skill... |
|-----------------|-------------------|
| Build a website | `website-structure` |
| Write content | `article-content`, `copywriting` |
| Set up tracking | `analytics-tracking` |
| Run A/B tests | `ab-testing` |
| Research competitors | `competitor-profiling` |
| Create ad copy | `ad-creative` |
| Build email flows | `email-marketing` |
| SEO optimization | `seo-audit`, `content-optimization` |
| Multi-agent workflow | This file |

---

## References

| Topic | File |
|-------|------|
| Common dependency graph patterns | references/graph-patterns.md |
| Anti-patterns to avoid | references/anti-patterns.md |
| Timeout guidelines by task type | references/timeout-guide.md |

---

## Evaluations

See evals/tests.md for self-evaluation test cases.
