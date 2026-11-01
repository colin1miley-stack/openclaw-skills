---
name: ship-task
description: Execute an owner-approved build end-to-end with the Axiom verification and ledger discipline — ownership claim, minimal diff, full test battery, explicit-path commit, dual ledgers, and a single next-step reply. Use when the owner says "Yes", "Go", "approved", or "build it" to a proposed task. Also triggers on "commit these changes", "prepare the deploy", or any request to ship work in the Axiom multi-agent repository.
metadata:
  openclaw:
    emoji: 🛡️
---

# Ship Task

Take a GO'd task from approval to deployed-and-ledgered without dropping a
step: claim before you touch, verify before you push, and leave every other
agent an accurate trail.

Built from the Axiom main-track operating loop (multi-agent repo, push =
production deploy, shared ledgers). Most failures in this environment come
from skipped steps, not hard problems — the checklist is the skill.

## When to Use

**Triggers on:**
- Owner replies "Yes" / "Go" / "approved" to a proposed build
- "Commit these changes on a branch and prepare the production deploy"
- "Ensure all work is tracked so there's no crossover"

**Does NOT trigger on:**
- Pure advice/asks ("advise", "recommend", "await go") — do not build
- Reviews (use `research-review` first; this skill runs after the GO)

## Step-by-Step

1. **Sync the repo first.** The checkout is shared by multiple agents and
   often falls behind: fetch, fast-forward (`git pull --rebase --autostash`),
   and check `git status` before planning your diff. Never plan on stale
   state.

2. **Claim ownership BEFORE touching anything.** Append an ownership-claim
   entry to the build record stating the exact paths you will touch, and
   explicitly what is excluded. If your task requires editing another
   track's files (CE-owned surfaces), ledger-flag the edit FIRST (scope,
   reason, owning GO), invite review, and keep the edit minimal. When the
   owner has directly assigned the GO to you, the flag states that — it does
   not ask permission twice.

3. **Build the minimal diff.** Only the paths claimed. No drive-by edits,
   no formatting sweeps.

4. **Run the verification battery (all must pass):**
   - New tests: `node --test test/<new>.test.ts`
   - Full suite: `npm test` (all green)
   - Types: `npx tsc --noEmit` (clean outside known other-track WIP)
   - Brand gate: `node scripts/anti-slop-guardrail.mts` (PASS, 0 regressions)
   - SQL involved: validate read-only against prod before shipping

5. **Stage explicit paths only.** Never `git add -A`. Never sweep other
   tracks' WIP (their failing uncommitted tests are reported in the ledger,
   not fixed by you). Commit message style:
   `feat(scope): summary (owner GO <ref>)`.

6. **Push (push = production deploy on the production branch).** If
   rejected, `git pull --rebase --autostash`, resolve ledger conflicts by
   keeping BOTH entries in chronological order, push again.

7. **Dual ledger, every task:**
   - Build record (website repo): outcome, root causes found, verification
     numbers, corrections to earlier flags — pushed.
   - Agent handoff (outer repo): what shipped, what's routed to whom,
     cross-track awareness items — local commit.

8. **Reply with the outcome, not the machinery.** What shipped, what was
   found (especially honest-empty findings — a measured zero beats a silent
   guess), verification numbers, and ONE bold next-step question.

## Error Handling

| Situation | Action |
|-----------|--------|
| Push rejected | Rebase with autostash; ledger conflict → keep both entries, chronological |
| Another track's tests fail in the full suite | Confirm zero coupling to your diff; report in ledger as pre-existing; do not fix |
| tsc fails in other tracks' WIP files | Exclude known-WIP paths; your files must be clean |
| Anti-slop regression from your file | Fix before committing — never pragma your way out without reason |
| Prod schema differs from code assumptions | Diff against information_schema before assuming; fix code to the live schema, not vice versa |
| Task grows beyond the claim | Stop, ledger the expansion, re-claim before proceeding |

## Rules

- English only, always. No exceptions.
- Honest empties everywhere: a zero with a label beats a fabricated number.
- Ledgers are the coordination protocol — a task isn't done until both
  entries land.
- Never leave the local checkout diverged behind remote when you finish.

## References

- `research-review` — what happens before the GO
- `x402-skill-build` — the build pipeline for scan skills
- Repo build record + handoff batch file (locations in the Axiom workspace)
