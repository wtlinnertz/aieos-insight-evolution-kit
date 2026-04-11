# TaskFlow Notification Service: Evolution Signal Example

This example shows a complete Evolution Signal for the TaskFlow notification-service after two RHR periods.

---

## Scenario

The TaskFlow notification-service was released to production in January 2026 following the discovery and engineering engagement documented in:
- PIK: `VH-TASKFLOW-NOTIF-001` (Frozen) — 6 success metrics across 3 hypotheses
- EEK: `ORD-TASKFLOW-NOTIF-001` (Frozen)
- REK: `RR-TASKFLOW-001` (Frozen)
- RRK: `SRER-NOTIF-001`, `SRP-NOTIF-001 v1`, `IR-NOTIF-001`, `RHR-NOTIF-001`, `RHR-NOTIF-002` (all Frozen)

Two RHR periods are now available, making this the first ES for the service.

**RHR-NOTIF-001** — January 15 to February 13, 2026
- All 3 SLOs met (Error Rate 99.98%, p99 Latency 99.71%, Delivery Rate 99.10%)
- 1 SEV3 incident (queue backpressure from burst load — fixed worker pool issue)
- Watch items: autoscaling (TASK-4822, targeted 2026-02-14), rate limiting (TASK-4823, targeted 2026-02-28)

**RHR-NOTIF-002** — February 14 to March 15, 2026 (fictional)
- All 3 SLOs met (Error Rate 99.99%, p99 Latency 99.62%, Delivery Rate 99.44%)
- 0 incidents (autoscaling deployed on 2026-02-14; no burst saturation events)
- Rate limiting deployed on 2026-02-26
- VH product metrics: SM-1 improving (disable rate dropped from 12% to 7.8%), SM-2 positive, SM-3 and SM-4 positive; SM-5 and SM-6 insufficient data (too early for quarterly churn)

**Re-entry signal:** `maintain` — System is stable across both periods. All SLOs met. Structural burst issue resolved. VH metrics for UX outcomes (SM-1 to SM-4) are positive; churn metrics (SM-5, SM-6) are deferred to quarterly review.

---

## Files

| File | Contents |
|------|----------|
| `00-evolution-signal.md` | Complete frozen Evolution Signal (ES-NOTIFICATION-SVC-001) |
| `validator-outputs/es-pass.json` | Validator output — all 5 hard gates PASS |

---

## What This Example Shows

- How to synthesize two RHR periods into trend analysis
- How to assess VH success metrics against observed production data (Achieved / Insufficient Data)
- How to select `maintain` over `watch` when all SLOs are met and watch items are resolved
- How to write a coverage summary with contiguous periods
- What a complete §5 Pattern Analysis looks like (resolved: queue backpressure; no persisting or new patterns)
