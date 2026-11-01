---
name: x402-skill-build
description: Build a new X402-metered scan skill on the canonical template — complete the spec first (detectors, COST-1 rubric, output contract, test plan), then build, wedge, cite, and verify. Use when the user says "build the skill", "complete the truncated spec", "X402 skill for X", "register row N", or approves a skill from a research review. Also triggers on "add a scanner", "new paid skill", or "the demand log says build this".
metadata:
  openclaw:
    emoji: ⚡
---

# X402 Skill Build

Ship a new paid, X402-metered scan skill the way the last ~15 were shipped:
spec first (approved before code), deterministic scoring (no LLM in the
scoring path), a free honest-basis wedge, canonical citations, and the full
verification battery before push.

Built from the Axiom scan family (EDI partner integration, Salesforce HPI
exposure, agent red team, brand drift, resilience score...). The order of
operations below is not cosmetic — the owner's standing rule is "complete the
spec first, then build".

## When to Use

**Triggers on:**
- "Build the skill" / "complete the truncated spec and build"
- "Register row N" (marketplace registry row for a new skill)
- "I approve the skill — conditionally" (the condition is: complete the spec)
- "The demand log / canary says build this"

**Does NOT trigger on:**
- Editing an existing skill's copy (routine edit)
- Non-metered features (route to the appropriate track)

## Step-by-Step

1. **Check overlap first.** Consult the existing ~15 X402 skills and the
   demand log before building — extend an existing skill rather than
   duplicating one.

2. **Complete the spec BEFORE writing code.** A truncated or implied spec is
   not buildable. The spec must state, concretely:
   - **Detectors:** each rule is deterministic (pattern/AST/structural match)
     — an LLM may polish prose downstream, never scores. Each detector maps
     to a rule family (AUTH-1, RES-2, COST-1, PII-4, BRAND-3, SEC-1/2/3,
     OUT-1...).
   - **COST-1 rubric:** every € figure carries its basis (actuarial band,
     in-band telemetry, or "not determinable — not scored", never a guess).
   - **Output contract:** the exact JSON/report shape a buyer receives.
   - **Test plan:** fixtures with known findings; fail-closed cases.
   - **Schema correctness:** Zod (or equivalent) schemas validated; input
     rejection paths tested.

3. **Get the spec GO.** Conditional approval means: finish the spec, restate
   it, then build. Unconditional approval means: proceed, citing the GO.

4. **Build on the canonical template.** Use the established scan template
   (scan-edi-partner-integration.ts / scan_salesforce_hpi pattern): pure
   scoring core (node-test safe) + thin route wrapper. Relative `.ts`
   imports so tests run under plain `node --test`.

5. **Free top-of-funnel wedge.** Attach the calculator / pre-flight variant
   (e.g., HPI Burn Calculator) with a deterministic basis. If no honest
   revenue model exists for the wedge, OMIT the revenue sentence — a missing
   sentence is honest; a fabricated one is a brand breach.

6. **Teaser + citations.** Teaser via `formatTeaserFinding`: institutional
   voice, no emoji, revenue sentence only when figure + basis both exist.
   OWASP/agentic citations imported from the canonical map
   (lib/standards/owasp-map.ts) — never hand-written. Add the Trust Pack
   matrix row for the new rule family.

7. **Verification battery (all must pass):**
   - `node --test test/<newSkill>.test.ts` — new tests green
   - `npm test` — full suite green
   - `npx tsc --noEmit` — clean outside known other-track WIP
   - `node scripts/anti-slop-guardrail.mts` — PASS (no new violations)
   - SQL validated read-only against prod if any schema is involved

8. **Ledger.** BUILD-RECORD entry + agent handoff entry; stage explicit
   paths only; never sweep other tracks' WIP into the commit. See `ship-task`.

## Output Format

```markdown
## Skill: [name] — SPEC (for approval)
- Detectors: [rule family → deterministic check]
- COST-1 basis: [figure + basis per severity]
- Output contract: [shape]
- Test plan: [fixtures + fail-closed cases]

## Build (post-GO)
- [files, wedge, citations, Trust Pack row]
- Verification: [suite counts, tsc, anti-slop]
```

## Error Handling

| Situation | Action |
|-----------|--------|
| Spec incomplete but owner says "build" | Complete the spec inline, show it, then build — state this |
| Detector needs judgment, not structure | Refuse the detector; propose an LLM-assisted *advisory* field clearly labelled unscored |
| Suite has other tracks' failures | Report them in the ledger as pre-existing; do not fix |
| Anti-slop regression from your file | Fix your file before committing |

## Rules

- No invented numbers anywhere: scores, € figures, or conversion rates.
- EUR pricing. Atomic-unit conversions only for known-decimal assets.
- One canonical mapping per rule. Honest empties (not_assessed) are
  labelled, never scored as failures.
- Spec → GO → build. Never the reverse.

## References

- `research-review` — the review that usually precedes a build
- `ship-task` — verification + ledger discipline
- `vertical-wedge-playbook` — how the skill becomes a motion
