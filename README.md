# aieos-insight-evolution-kit

**Layer 7 of the AIEOS system** — Insight & Evolution.

This kit synthesizes reliability health signals from production into actionable insights and closes the feedback loop to the Product Intelligence layer. It answers the question: **What did we learn and what changes?**

---

## What This Kit Does

The Insight & Evolution Kit takes frozen Reliability Health Reports (from Layer 6) and an optional Value Hypothesis (from Layer 2), and produces an Evolution Signal that:

1. Assesses whether the original value hypothesis was realized in production
2. Identifies reliability trends and patterns across the reporting period
3. Determines whether the system should continue as-is, be watched, or trigger new discovery
4. Produces recommended actions with owners

The Evolution Signal closes the loop: Layer 7 → Layer 2 (Product Intelligence).

---

## Single Artifact Type

**Evolution Signal (ES)** — the only governed artifact in this kit.

| File | Purpose |
|------|---------|
| `docs/specs/es-spec.md` | Content rules and 5 hard gates |
| `docs/artifacts/es-template.md` | ES structure |
| `docs/prompts/es-prompt.md` | Generation instructions |
| `docs/validators/es-validator.md` | Pass/fail evaluation |

---

## Layer Position

```
Layer 6: Reliability & Resilience Kit
  Output: Frozen Reliability Health Report (RHR)
    ↓
Layer 7: Insight & Evolution Kit (this kit)
  Input: 2+ frozen RHRs
  Output: Frozen Evolution Signal (ES)
    ↓
Layer 2: Product Intelligence Kit (if re-discover signal)
  Input: PIK intake recommendation from ES §7
```

---

## When to Use This Kit

Run an Evolution Signal after accumulating at least 2 frozen RHRs for a service. Typical cadence: quarterly or after a significant reliability period.

The ES is not time-pressure-driven — it is value-driven. Run it when there is enough data to assess reliability trends and evaluate whether the system is behaving as the value hypothesis predicted.

---

## Worked Example

`examples/basic-evolution/` — TaskFlow notification-service evolution signal after 2 RHR periods.

---

## Links

- Playbook: `docs/playbook.md`
- Documentation index: `docs/index.md`
- AIEOS layer model: `aieos-spec/docs/layer-model.md`
- Governance model: `docs/governance-model.md`
