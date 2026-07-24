# Documenting architectural trade-offs

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | architects, leads, senior engineers |
| Level | intermediate |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

## Summary

Enterprise architecture quality often comes less from choosing a fashionable pattern and more from making trade-offs explicit, reviewable, and revisitable.

## Practice

1. Capture the **context** and **forces** (scale, compliance, team topology, time).
2. List real **options** (including “do nothing”).
3. Compare on shared criteria: complexity, risk, cost, operability, time-to-value.
4. Record the decision in an [ADR](../templates/architecture-decision-record.md).
5. Schedule a review date when assumptions may expire.

## Common enterprise forces

- Regulatory and audit requirements
- Multi-team ownership and platform boundaries
- Legacy integration constraints
- Hiring and skill availability
- Cost predictability vs flexibility

## Related

- [Architecture Decision Record](../templates/architecture-decision-record.md)
- [Evaluating frameworks for enterprise](../frameworks/evaluating-frameworks-for-enterprise.md)
