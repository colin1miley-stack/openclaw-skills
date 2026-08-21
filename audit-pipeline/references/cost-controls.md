# Audit Pipeline — Cost Controls

## Per-Step Budgets

| Step | Max Cost | Typical |
|------|----------|---------|
| Intake | £0.10 | £0.05 |
| Router | £0.05 | £0.02 |
| Data Collection (×5) | £0.50 | £0.25 |
| Synthesis | £0.25 | £0.15 |
| Review | £0.10 | £0.05 |
| Recommendations | £0.25 | £0.15 |
| Proposal | £0.10 | £0.05 |
| **Total** | **£50.00 hard cap** | **~£0.86** |

## Veto Triggers

- **Cost Veto:** If `cost_running > £50`, halt immediately and alert Colin
- **Failure Veto:** If 3+ steps fail, halt and alert Colin
- **Timeout Veto:** If any human gate times out, log and hold

## Cost Tracking

Each step must update `state.json`:

```json
{
  "cost_running": 0.43,
  "cost_by_step": {
    "intake": 0.05,
    "router": 0.02,
    "data_collection": 0.18,
    "synthesis": 0.12,
    "review": 0.06
  }
}
```

## Escalation Protocol

1. **Yellow alert:** Cost at 50% of cap (£25) → log warning, continue
2. **Red alert:** Cost at 80% of cap (£40) → pause, ask Colin to approve continuation
3. **Veto:** Cost exceeds cap (£50) → halt, do not proceed, alert Colin
