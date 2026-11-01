---
name: vertical-wedge-playbook
description: Turn a named company or vertical into an Axiom opportunity brief — persona alignment, MBD-mapped pain table, three novel wedge ideas, and outreach framing. Use when the user names a company and asks "advise", "approach them", "tomorrow's call", "how do I differentiate", "outreach to", or "strategic alignment". Also triggers on "same analysis for [company]" or when the user mentions a contact at a target company.
metadata:
  openclaw:
    emoji: 🎯
---

# Vertical Wedge Playbook

Convert one company name into a complete opportunity brief: who the economic
buyer really is, which locked modules answer their pains, three wedge ideas
in fixed slots, and outreach copy that positions Axiom as a complement —
never a rival.

Built from the repeated pattern behind the Hyva, Standard Economics, and
Salesforce HPI briefs: the same six moves produced an approved brief every
time. The structure below IS the deliverable.

## When to Use

**Triggers on:**
- User names a company + "review", "advise", "approach", "call with"
- "How do I differentiate Axiom from [parallel product]?"
- "A member of staff reached out / I connected with the owner on LinkedIn"
- "Produce the explainer/message I can send"

**Does NOT trigger on:**
- Pure research review with no target company (use `research-review`)
- Direct competitors of Axiom's own product (handle as positioning, not outreach)

## Step-by-Step

1. **Persona alignment.** Identify the economic buyer's systemic concerns:
   reliability, data integrity, security/compliance, technical debt,
   provability to leadership. They do not buy "AI magic" — they buy fewer
   silent failures and receipts. State the persona in one line before
   anything else.

2. **Reality-check the company.** Verify size, recent news, funding, and
   stack signals. Unverified claims get flagged and embargoed from any
   copy that leaves the building (see `research-review` §3).

3. **Pain → module map.** Build a 3–6 row table:
   `their pain | locked module | how Axiom prevents/solves it`.
   Use canonical module numbers. One canonical mapping — never two.

4. **Three fixed novel-idea slots** (the research almost always underfills
   these — this is where you add value):
   - **Slot A — Narrative wedge:** how to frame Axiom FOR THIS BUYER.
     Category-defining target? Use the pairing posture: *they allocate,
     Axiom preserves* — complement, not rival. Institutional target?
     "Deterministic governance layer", never "marketing tool".
   - **Slot B — Trust accelerator:** a signed, deliverable asset their side
     can hand onward — CISO Trust Pack / compliance matrix variant matched
     to their procurement reality (government, enterprise, or partner bank).
   - **Slot C — Ecosystem trojan horse:** a free, honest-basis scan their
     PARTNERS or dealers can run against middleware connected to the target —
     funnel to the paid Deep Structural Audit (€7,500). Encode it as a
     repeatable motion, not a one-off.

5. **Differentiation note.** If a parallel product exists (observability,
   profilers, error tracking), position Axiom as the deterministic
   structural layer — prevention and governance with receipts, not
   monitoring and dashboards. Never claim feature parity you don't have.

6. **Outreach framing.** When asked for the explainer/message: one page,
   readable boxes (no crammed tables), positioned so the recipient can
   forward it internally, one-sentence ask, and the free wedge offered
   with its honest basis stated. Founder voice for the intro; institutional
   voice for the body.

7. **GO boundary.** The brief is advice. Wait for explicit approval before
   building skills, scanners, or sending anything. On approval, route via
   `ship-task` / `x402-skill-build` as appropriate.

## Output Format

```markdown
## [Company] — Opportunity Brief
**Persona:** [one line]
**Posture:** complement / rival — [why]

| Their pain | Module | How Axiom answers |
|---|---|---|
| ... | ... | ... |

## Wedge Ideas
- A (narrative): ...
- B (trust accelerator): ...
- C (ecosystem trojan horse): ...

## Differentiation
[vs named parallel products, honestly]

## Concerns
- [risk + mitigation]

**[One bold next-step question]**
```

## Error Handling

| Situation | Action |
|-----------|--------|
| Company too small/noise-level | Say so; offer the vertical-level brief instead of the company brief |
| Target is category-defining | Pairing posture mandatory — never attack by name in buyer copy |
| User already has a warm contact | Lead with the relationship; the brief becomes the call prep, not cold outreach |
| Claims about target unverifiable | Embargo; brief strategy on verified facts only |

## Rules

- All prices in EUR. Free wedges carry an honest basis or no revenue sentence.
- Complement-not-rival is the default posture; rivalry lives in category
  content only, never in buyer-facing copy.
- A brief is not an approach. Await GO.

## References

- `research-review` — verification discipline
- `x402-skill-build` — when slot C becomes a real scanner
- Axiom POSITIONING-SSOT — pairing doctrine + register rules
