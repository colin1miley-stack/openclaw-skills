# Build Orchestrator — Anti-Patterns

## 1. Skipping Phase 0

**What:** Spawning subagents without reading relevant skills first.

**Why it's bad:** Wrong model IDs, wrong skills, vague success criteria → wasted tokens, bad output.

**Fix:** Complete the 5-item checklist. Every time. No exceptions.

---

## 2. Guessing Timeouts

**What:** Using default timeouts for all tasks regardless of complexity.

**Why it's bad:** Short timeouts kill long tasks. Long timeouts waste money on stuck tasks.

**Fix:** Use the timeout guide:
- Quick check: 60–120s (git status, file read)
- Research: 180–300s (web research, competitor analysis)
- Multi-step build: 300–600s (site build, skill creation)
- Long-running: 900s+ (video processing, batch operations)

---

## 3. No Success Criteria

**What:** Spawning a subagent with "research this" and no definition of done.

**Why it's bad:** Subagent produces vague, unfocused output. You can't verify completion.

**Fix:** Define acceptance criteria before spawning:
- Format: "Deliver as markdown table"
- Length: "Max 500 words"
- Quality: "Every claim needs a source URL"
- Done: "Stop when you have 10 competitors with pricing"

---

## 4. Not Verifying Models

**What:** Using model IDs from memory without checking they still exist.

**Why it's bad:** Spawning fails silently or returns errors. Retries waste tokens.

**Fix:** Run `openclaw models list` or check provider docs. Update `models.json` when models change.

---

## 5. Long Silences

**What:** Letting a task run >90 seconds without giving the user an update.

**Why it's bad:** User thinks you're stuck or broken. Trust erodes.

**Fix:** If a task takes >90s, say something. "Still researching competitors — 3 of 8 done."
