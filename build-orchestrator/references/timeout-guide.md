# Build Orchestrator — Timeout Guidelines

| Task Type | Timeout | Example |
|-----------|---------|---------|
| Quick check | 60–120s | Git status, file read, simple search |
| Research | 180–300s | Web research, competitor analysis, data gathering |
| Multi-step build | 300–600s | Site build, skill creation, doc generation |
| Long-running | 900s+ | Video processing, batch operations, large dataset analysis |

## Subagent Timeout Rule

When in doubt, set longer. Subagents fail silently on timeout.

## Polling Interval

- Active tasks: 30–60s
- Background tasks: 120–300s
- Use `process(poll, 60000)` for active tasks

## Timeout Overrides

Some skills specify their own timeouts. Respect them. If a skill says "use 300s for research", use 300s.
