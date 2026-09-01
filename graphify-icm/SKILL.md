# Graphify ICM Integration

**Query your codebase knowledge graph within ICM workflows.**

Teaches agents to use Graphify as a Layer 3 reference source — querying the knowledge graph before reading raw files, tracing connections during refactors, and using community detection to understand subsystem boundaries.

---

## What This Is

Graphify maps your codebase into a queryable knowledge graph. Instead of reading files one by one, agents ask the graph for relationships, paths, and explanations. This skill integrates Graphify queries into the ICM layer-loading model.

**Prerequisites:**
- Graphify CLI installed (`uv tool install graphifyy`)
- Graph built for target repo (`graphify .` or `graphify . --code-only`)
- Output in `graphify-out/` directory

---

## When to Use Graphify Queries

**Query the graph FIRST when:**
1. **Exploring unfamiliar code** — "What does this component do?"
2. **Planning refactors** — "What depends on this function?"
3. **Debugging cross-file issues** — "How does auth connect to payments?"
4. **Onboarding** — "What's the architecture of this system?"
5. **Code review** — "Does this change impact anything unexpected?"

**Skip the graph when:**
1. **File is already known** — You're editing a file you wrote yesterday
2. **Single-file change** — No cross-file impact expected
3. **Urgent hotfix** — Query graph later, fix now

---

## ICM Layer Mapping

```
Layer 0: SOUL.md / CLAUDE.md              ← Always loaded
Layer 1: CONTEXT.md                        ← Stage routing
Layer 2: Stage CONTEXT.md                  ← Task contract
Layer 3: references/, _config/, skills/    ← Includes graphify-out/
         ├─ graphify-out/GRAPH_REPORT.md   ← Architecture overview
         ├─ graphify-out/graph.json        ← Queryable graph
         └─ (queried on demand, not loaded)← Subgraph results
Layer 4: output/ from prior stages         ← Working artifacts
```

**Key principle:** Graphify lives in Layer 3 (reference) but is **queried on demand**, not loaded wholesale. The GRAPH_REPORT.md may be read at stage start, but `graph.json` is only accessed via targeted queries.

---

## Query Patterns

### Pattern 1: Understand a Component

```
graphify explain "ComponentName"
```

**Use when:** You need to understand what a specific component/function/class does.

**Example:**
```
graphify explain "AssessmentForm"
→ Returns: source file, line number, community, degree (connection count),
           and all connections with edge types
```

**ICM integration:** Agent reads explain output → identifies relevant files → reads only those files.

---

### Pattern 2: Trace Connections

```
graphify path "Source" "Target"
```

**Use when:** You need to know how two things connect, or if they connect at all.

**Example:**
```
graphify path "AuthMiddleware" "StripeWebhook"
→ Returns: shortest path with hops, showing intermediate nodes
→ Use case: "Will auth changes break payments?"
```

**ICM integration:** Path tracing for impact analysis before refactors.

---

### Pattern 3: Explore Dependencies

```
graphify query "what depends on {module}?"
graphify query "what uses {function}?"
```

**Use when:** You need to find all consumers of a module/function.

**Example:**
```
graphify query "what depends on LeadAPI?"
→ Returns: scoped subgraph showing all nodes connected to LeadAPI
```

**ICM integration:** Use in quality-gate.md for refactor stages. "Verify no orphaned consumers."

---

### Pattern 4: Architecture Overview

```
Read graphify-out/GRAPH_REPORT.md
```

**Use when:** Onboarding to a new codebase or planning major changes.

**GRAPH_REPORT.md contains:**
- **God nodes:** Most-connected concepts (everything flows through these)
- **Surprising connections:** Unexpected links between subsystems
- **The "why":** `# NOTE:` / `# WHY:` comments as first-class nodes
- **Suggested questions:** 4-5 questions the graph is positioned to answer
- **Confidence tags:** EXTRACTED vs INFERRED relationships

**ICM integration:** Read GRAPH_REPORT.md at start of any stage involving unfamiliar code.

---

### Pattern 5: Community Exploration

```
graphify query "show community {N}"
# Or read graph.html in browser and click community filter
```

**Use when:** Understanding subsystem boundaries and module clustering.

**ICM integration:** Use community labels to scope work. "This refactor affects Community 3 (auth subsystem)."

---

## Agent Workflow: Graph-First Code Exploration

### Standard Workflow (Without Graphify)
```
1. Read file A → Read file B → Read file C → Read file D
2. Eventually understand the system
3. Tokens wasted: ~15,000-30,000
```

### Graph-First Workflow (With Graphify)
```
1. graphify explain "TargetComponent"
   → Returns: 5 connected files, 2 communities, key relationships
2. Read only the 5 relevant files
3. Tokens used: ~5,000-8,000
4. Understanding: Faster and more complete
```

---

## Integration with Ralph Execution

When using `icm-ralph-execution`, add graph queries to the subagent task:

```json
{
  "task": "Implement feature X. " +
          "STEP 1: Query graph for relevant components: " +
          "`graphify explain 'AssessmentForm'` and `graphify query 'what depends on auth?'` " +
          "STEP 2: Read only files identified by graph query. " +
          "STEP 3: Implement feature. " +
          "STEP 4: Run quality gate.",
  "runtime": "subagent",
  "timeoutSeconds": 300
}
```

---

## Quality Gate: Graph Verification

Add to `quality-gate.md` for coding stages:

```markdown
## Graph Verification (if graphify available)
- [ ] Ran `graphify explain` on modified components
- [ ] Verified no orphaned consumers via `graphify query`
- [ ] Confirmed community boundaries respected
- [ ] GRAPH_REPORT.md updated if architecture changed
```

---

## Multi-Repo Graphs

Our workspace has multiple repos. Query the right graph:

| Repo | Graph Location | Command |
|------|---------------|---------|
| Workspace root | `graphify-out/` | `graphify query "..."` |
| ai-revenue-site | `ai-revenue-site/graphify-out/` | `cd ai-revenue-site && graphify query "..."` |
| app | `app/graphify-out/` | `cd app && graphify query "..."` |
| Client project | `clients/{client}/graphify-out/` | `cd clients/{client} && graphify query "..."` |

**Global graph** (cross-repo):
```
graphify global path "ComponentA" "ComponentB"
```

---

## Keeping Graphs Current

### Manual Update
```bash
graphify update .        # Re-extract changed files only
graphify extract .       # Full rebuild
```

### Git Hook Auto-Update
```bash
graphify hook install    # One-time setup
# Now auto-rebuilds on every commit and branch switch
```

### Post-Pull Sync
```bash
# Add to git config:
git config --global alias.gpull "!git pull && graphify update ."
```

---

## Command Reference

```bash
# Build graph
graphify .                          # Current directory
graphify . --code-only              # AST only, no LLM (fast, no API cost)
graphify . --no-viz                 # Skip HTML, keep JSON + report
graphify . --update                 # Incremental: only changed files

# Query graph
graphify query "question"           # Natural language query
graphify query "..." --graph path   # Use specific graph.json
graphify path "A" "B"               # Shortest path between nodes
graphify explain "Node"             # Node detail with connections

# Analysis
graphify export callflow-html       # Architecture diagram
graphify cluster-only .             # Re-run community detection
graphify label .                    # Re-name communities

# Maintenance
graphify hook install               # Auto-rebuild hooks
graphify hook status                # Check hook status
graphify update .                   # Sync after git pull
```

---

## .graphifyignore Template

```
# .graphifyignore — exclude from knowledge graph
node_modules/
dist/
.next/
.vercel/
build/
*.generated.ts
*.generated.js
*.min.js
*.min.css
*.map
coverage/
.cache/
*.log
.env*
!.env.example
*.zip
*.tar.gz
graphify-out/cost.json
graphify-out/cache/
```

---

## Anti-Patterns

| Anti-Pattern | Why It Fails | Fix |
|-------------|--------------|-----|
| Query graph for every file read | Wastes time on simple edits | Use graph for unfamiliar code only |
| Never update graph after commits | Stale relationships | Install git hooks |
| Ignore EXTRACTED vs INFERRED tags | May act on guessed relationships | Verify INFERRED edges manually |
| Load full graph.json into context | Massive token waste | Query for scoped subgraph only |
| Skip GRAPH_REPORT.md on onboarding | Misses architecture overview | Read report first, query second |

---

## Metrics

| Metric | Target | How |
|--------|--------|-----|
| Graph currency | <5 min from commit | Git hooks |
| File reads saved | 50%+ vs file-by-file | Compare token usage |
| Refactor safety | 100% impact detection | Path tracing pre-refactor |
| Onboarding speed | <30 min to productive | GRAPH_REPORT.md + queries |

---

*Integrates with: `icm-core`, `icm-ralph-execution`, `revenue-systems-agency`*
