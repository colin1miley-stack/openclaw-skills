# Build Orchestrator — Self-Evaluation Tests

## Test 1: Pre-Flight Compliance

**Scenario:** User asks "Build me a landing page."

**Check:** Did the agent complete Phase 0 before spawning any subagent?

**Expected:**
- [ ] Read `website-structure` skill
- [ ] Verified model IDs (if non-default)
- [ ] Defined success criteria (format, length, quality)
- [ ] Set timeout (300–600s for site build)
- [ ] Checked for stale data (memory >7 days old)

**Pass criteria:** All 5 checklist items completed before any subagent spawn

---

## Test 2: Skill Discovery

**Scenario:** User asks "Write me an email sequence."

**Check:** Did the agent discover and read relevant skills?

**Expected:**
- Read `email-marketing` or `email-sequence` skill
- Read `copywriting` skill
- Checked for dependencies (e.g., `klaviyo-email-marketing` if using Klaviyo)

**Pass criteria:** At least 2 relevant skills read before task definition

---

## Test 3: Graph Definition

**Scenario:** Multi-step build requiring research + writing + review.

**Check:** Did the agent define the dependency graph?

**Expected:**
- Parallel tasks identified (e.g., research + data collection)
- Sequential tasks identified (e.g., writing depends on research)
- Human review gates marked
- External dependencies noted

**Pass criteria:** Graph defined with clear parallel/sequential/human markers

---

## Test 4: Timeout Appropriateness

**Scenario:** Different task types spawned.

**Check:** Are timeouts matched to task complexity?

**Expected:**
- Git status: 60s
- Competitor research: 180s
- Full site build: 600s
- Video processing: 900s

**Pass criteria:** No task uses default timeout when specific guidance exists

---

## Test 5: Quality Gate Enforcement

**Scenario:** Subagent returns output.

**Check:** Did the agent verify before declaring complete?

**Expected:**
- Output matches acceptance criteria
- No placeholders or TODOs
- Format matches specification
- Files in correct locations
- Git committed

**Pass criteria:** All 5 quality checks passed before delivery
