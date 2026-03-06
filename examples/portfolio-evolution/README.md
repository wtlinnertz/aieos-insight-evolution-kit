# Portfolio Evolution Signal — Worked Example

## Scenario

Three TaskFlow platform initiatives have completed at least one Evolution Signal cycle. This PES synthesizes their Engagement Records to find cross-initiative patterns and propose one targeted improvement to the ES prompt.

## Initiatives in Scope

| Initiative | ER ID | Service | Final ES Signal | Final VH Verdict |
|-----------|-------|---------|----------------|-----------------|
| TaskFlow Notifications | ER-TASKFLOW-NOTIF-001 | notification-service | maintain | Partially Validated |
| TaskFlow File Attachments | ER-TASKFLOW-ATTACH-001 | attachment-service | watch | Partially Validated |
| TaskFlow Activity Feed | ER-TASKFLOW-FEED-001 | activity-feed-service | re-discover | Invalidated |

## What This Example Shows

- **§3 VH pattern finding**: All three initiatives show Insufficient Data for churn/retention metrics at the first ES — user behavior metrics with quarterly measurement windows cannot be assessed in a 60-day observation period. The PES identifies this as a structural pattern and proposes an es-prompt.md addition.
- **§4 Reliability finding**: First-period SLO misses appear in 2 of 3 services (Notifications had a burst incident, Activity Feed had a throughput miss). This is identified as a first-period vulnerability pattern.
- **§5 Assumption finding**: User behavior assumptions (how users will change habits in response to new features) were the most common assumption category carried to DPRD without experimental validation across all three initiatives.
- **§6 Improvement proposal**: One specific proposal to es-prompt.md Step 7 to add an explicit check for SM-N metrics with quarterly measurement windows, requiring the re-entry signal section to address whether an early ES signal may be premature.

## Files

| File | Contents |
|------|----------|
| `00-portfolio-evolution-signal.md` | Frozen PES-001 — cross-initiative analysis with one improvement proposal |
| `validator-outputs/pes-pass.json` | Validator output confirming all 5 hard gates PASS |
