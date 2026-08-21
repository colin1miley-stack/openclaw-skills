# Audit Pipeline — Quality Review Rubric

## Purpose

Validate findings against objective criteria before proceeding to recommendations and proposal assembly.

## Rubric

| Criterion | Weight | Check |
|-----------|--------|-------|
| Evidence-backed | 30% | Every critical/high finding has ≥2 data points |
| Business impact quantified | 25% | ≥60% of findings have £ impact |
| Actionable | 25% | Every finding has a clear next step |
| No hallucinations | 20% | All claims traceable to source data |

## Scoring

- **Score ≥ 0.8:** Approve → proceed to Step 6
- **Score < 0.8:** Reject → return to Step 4 with specific feedback (max 1 revision)
- **Critical gap found:** Pause → ask Colin for clarification

## Scoring Worksheet

For each finding, rate 0-1 on each criterion, then compute weighted average:

```
Finding 1: Evidence [0.9] × 0.30 = 0.27
           Impact  [0.8] × 0.25 = 0.20
           Action  [1.0] × 0.25 = 0.25
           Source  [1.0] × 0.20 = 0.20
           ─────────────────────────────
           Weighted Score = 0.92
```

Aggregate across all findings for total review score.

## Feedback Format (on rejection)

When returning to Step 4, specify:
1. Which criterion failed
2. Which finding(s) are deficient
3. What evidence is missing
4. Suggested fix

Example:
```
REVIEW REJECTED — Score: 0.72

Failed criterion: Evidence-backed (weight 30%)
Deficient findings: #2 (Calendar Analysis), #4 (Email Review)
Issue: Finding #2 cites only 1 data point (needs ≥2)
Suggested fix: Add no-show rate + prep time data from calendar export
```
