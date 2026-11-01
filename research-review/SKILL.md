---
name: research-review
description: Review a research brief or analysis document (usually attached as "Based on a rigorous...") and produce a verified, evidence-gated assessment with recommendations and novel ideas. Use when the user says "review this research", "review and advise", "same rigour", "additional novel ideas", "I want to accept/approve the recommendations", or attaches a research/analysis file for assessment. Also triggers on "assess this", "what do you think of this research", or "any learnings to take from this".
metadata:
  openclaw:
    emoji: 🔍
---

# Research Review

Review a research brief the way a sceptical partner would: verify before you
advise, map to what is actually locked, add what the research missed, and
never build anything until the owner says GO.

Built from the Axiom Revenue Labs operating pattern (15+ reviewed briefs,
Oct–Nov 2026). The owner almost always replies "yes with your amendments" —
the skill's job is to make the amendments sharp and the GO boundary explicit.

## When to Use

**Triggers on:**
- User attaches a research file and says "review", "advise", "recommend", "any novel ideas"
- User says "apply the same rigour to this research"
- User says "I accept the recommendations" (run the acceptance handling below)
- User asks whether a strategic decision (positioning, pricing, target) is supported

**Does NOT trigger on:**
- "Summarise this" — that is a reading task, not a review
- Building tasks — those start after the GO (see `ship-task`)

## Step-by-Step

1. **Read the entire attachment first.** Note its structure: target analysis,
   doctrine/module mapping, recommendations, novel ideas. Never advise from a
   partial read.

2. **Extract the load-bearing claims.** These are the claims a decision would
   rest on: funding rounds, partnership sizes, customer/member counts,
   named backers, competitor capabilities, pricing, market sizes. List them
   mentally before continuing.

3. **Verify independently.** Search the web for the load-bearing claims —
   never accept the research's own citations at face value. Classify each:
   **verified** (independent corroboration) / **unverified** (single-source)
   / **contradicted**. Unverified claims get an embargo: they may inform
   strategy but must never appear in copy, outreach, or grant applications.
   (Real case: a competitor's "$100M round" and "42.7M-member government
   partnership" were single-source — usable for framing, banned for citing.)

4. **Map to locked doctrine, not vibes.** Check the claims against the
   locked sources of truth (MasterBuild Documents, POSITIONING-SSOT).
   Keep the research's module mappings when correct; amend with reasons
   when wrong. One canonical mapping — if the same research would map two
   ways, pick one and say why.

5. **Assess with an explicit verdict:**
   - **ACCEPT** — sound, aligned, verified.
   - **ACCEPT-WITH-AMENDMENTS** (the usual outcome) — state each amendment
     precisely (wording changes, removed claims, added gates).
   - **REJECT** — say what would have to be true to accept it, and offer
     the smaller version worth keeping.

6. **Add novel ideas the research missed.** Recurring slots that have
   repeatedly moved the needle:
   - **Repeatable motion, not one-off** — encode the wedge into the rules
     engine / playbook instead of executing it once.
   - **Honest-empty positioning** — turn a refusal or a zero into a proof
     asset ("we refuse to fabricate this number" IS the brand).
   - **Doctrine content** — distil the decision into a founder's-doctrine
     piece for the content engine.
   - **Signal-source monitoring** — the target's future incidents/failures
     are your demand events; set a standing watch.
   - **Second revenue line** — data/licensing angles on assets you already
     compute (actuarial scores, preservation ledgers).

7. **Await GO.** End the review with the GO boundary explicit: what you
   recommend, what you advise against, and the single most probable next
   step as one bold question. When the owner says yes: route the work
   (ledger-flag cross-track items, hand off CE-owned surfaces — see
   `ship-task`), never start building inside the review itself.

## Output Format

```markdown
## Verification Status
- [Claim]: verified / unverified (embargo note) / contradicted

## Assessment
[ACCEPT / ACCEPT-WITH-AMENDMENTS / REJECT + reasoning in 1 paragraph]

## Recommendations
1. [Action] — [why] — owner: [amendment if any]

## Novel Ideas (beyond the research)
- [Idea] — [slot type] — [expected effect]

## Concerns
- [Risk + mitigation]

**[One bold next-step question]**
```

## Error Handling

| Situation | Action |
|-----------|--------|
| Attachment unreadable/truncated | Say so, ask for re-send — never review half a file |
| Claims unverifiable within reason | Mark "unverified", embargo from copy, proceed with strategy |
| Research contradicts locked doctrine | Doctrine wins; offer the amended version that fits |
| Owner approves everything wholesale | Still state your amendments + concerns once, then execute |

## Rules

- English only in output. Reply in the language of the owner's messages.
- Never fabricate a verification — "unverified" is a finding, not a failure.
- Never let an unverified claim leak into customer-facing or grant copy.
- The review is advice; the GO is the owner's. Keep that boundary explicit.

## References

- `POSITIONING-SSOT.md` (Axiom repo, website/docs/) — register + honesty gates
- `ship-task` — what happens after the GO
- `vertical-wedge-playbook` — when the research names a company to approach
