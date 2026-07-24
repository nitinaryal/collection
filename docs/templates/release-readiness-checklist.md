# Release readiness checklist

| Field | Value |
|-------|-------|
| Status | template |
| Audience | engineers, leads, release managers |
| Level | intermediate |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

Adapt this checklist to your branching, change, and compliance process.

## Product and scope

- [ ] Release scope agreed and documented
- [ ] Breaking changes identified and communicated
- [ ] Feature flags / kill switches reviewed (if used)

## Quality

- [ ] Automated tests green on release candidate
- [ ] Critical manual / exploratory checks complete
- [ ] Performance and capacity checks done for high-risk changes
- [ ] Accessibility and UX checks for user-facing changes

## Security and compliance

- [ ] Dependency and vulnerability scan reviewed
- [ ] Secrets not present in artifacts or logs
- [ ] Security-relevant changes peer reviewed
- [ ] Required approvals obtained

## Operability

- [ ] Runbook / rollback steps updated
- [ ] Monitoring and alerts verified
- [ ] On-call aware of release window
- [ ] Data migration plan tested (if applicable)

## Communication

- [ ] Release notes drafted
- [ ] Stakeholders notified
- [ ] Support / customer-facing teams briefed when needed

## Go / no-go

| Field | Value |
|-------|-------|
| Release ID / version | |
| Decision | go / no-go |
| Approver | |
| Date / time | |
