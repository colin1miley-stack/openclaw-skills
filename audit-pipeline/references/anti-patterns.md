# Audit Pipeline — Anti-Patterns

## 1. God Node

**What:** Trying to do intake + analysis + writing in one pass.

**Why it's bad:** Violates the graph topology. Each step is a separate node for a reason — isolating failure domains, enabling parallel execution, and keeping context windows manageable.

**Fix:** Follow the 9-step sequence. No step should exceed 150 lines of reasoning.

---

## 2. Hidden Router

**What:** Letting the synthesis node change audit type mid-flight.

**Why it's bad:** The router (Step 2) is the single source of truth for audit type. If synthesis discovers the router was wrong, that's a signal to flag, not to override.

**Fix:** Synthesis writes a "Router Accuracy Note" if findings contradict the audit type. Colin decides whether to re-route.

---

## 3. Infinite Loop

**What:** Review fails → revise → review fails → revise → ...

**Why it's bad:** Wastes tokens and time. If findings are fundamentally flawed, more revision won't help.

**Fix:** Max 1 revision on review. If still failing (score < 0.8 after revision), escalate to Colin.

---

## 4. State Dump

**What:** Passing the entire audit directory to every node.

**Why it's bad:** Bloated context, irrelevant information, higher token costs.

**Fix:** Each step only reads what it needs:
- Router reads INTAKE.md only
- Data collection nodes read INTAKE.md + ROUTING.md
- Synthesis reads all 5 data files
- Review reads FINDINGS.md only
- Recommendations reads FINDINGS.md + REVIEW.md

---

## 5. Missing Default

**What:** Router can't classify and has no fallback.

**Why it's bad:** Pipeline stalls. Uncertainty should not block progress.

**Fix:** Router must always have a fallback. If confidence < 0.7 for any specific type, default to `full_system` and flag for Colin review.
