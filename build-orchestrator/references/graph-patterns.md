# Build Orchestrator — Common Dependency Graph Patterns

## Pattern A: Research → Analysis → Writing

```
[Research] ──┐
             ├──→ [Analysis] ──→ [Writing] ──→ [Review] ──→ [Delivery]
[Data] ──────┘
```

**Use when:** Building content, audits, proposals
**Parallel:** Research + Data collection
**Sequential:** Analysis → Writing → Review → Delivery

---

## Pattern B: Fan-Out → Fan-In

```
         ┌──→ [Agent 1: CRM Analysis]
[Input] ──┼──→ [Agent 2: Calendar Analysis]
         ├──→ [Agent 3: Email Review]
         ├──→ [Agent 4: Competitor Intel]
         └──→ [Agent 5: Process Review]
                  ↓
            [Synthesis Agent]
                  ↓
            [Review] → [Delivery]
```

**Use when:** Audit pipelines, multi-source analysis
**Parallel:** All 5 data collection agents
**Sequential:** Synthesis → Review → Delivery

---

## Pattern C: Iterative Loop

```
[Draft] ──→ [Review] ──→ [Revise] ──→ [Review] ──→ [Approve] ──→ [Delivery]
              ↓ reject              ↓ reject
           (max 2 loops)
```

**Use when:** Content that needs human approval
**Loop limit:** 2 iterations max, then escalate to Colin

---

## Pattern D: Parallel Independent

```
[Task A] ──→ [Done]
[Task B] ──→ [Done]
[Task C] ──→ [Done]
```

**Use when:** Multiple unrelated deliverables
**No dependencies:** Spawn all simultaneously

---

## Pattern E: Conditional Branch

```
[Input] ──→ [Router] ──→ [Type A Workflow]
              │
              └──→ [Type B Workflow]
```

**Use when:** Different inputs need different processing paths
**Router logic:** Must have explicit decision criteria + default fallback
