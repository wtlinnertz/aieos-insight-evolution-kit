# Insight & Evolution Kit — Documentation Index

This kit governs Layer 7 of the AIEOS system: synthesizing operational signals from production into actionable insights and closing the feedback loop to the Product Intelligence layer.

---

## Start Here

| Document | Purpose |
|----------|---------|
| `playbook.md` | **Complete process definition** — read this first |
| `how-to-use-with-ai.md` | Session setup for AI-assisted generation and validation |
| `how-to-adapt.md` | Organizational adoption and customization guidance |
| `governance-model.md` | AIEOS structural rules (reference) |

---

## Artifact Types

### Evolution Signal (ES)

Single-service synthesis. Inputs: ≥2 frozen RHRs + optional frozen VH. ID format: `ES-{SCOPE}-{NNN}`.

| File | Location | Purpose |
|------|----------|---------|
| Spec | `specs/es-spec.md` | Content rules and 5 hard gates |
| Template | `artifacts/es-template.md` | ES structure (8 sections) |
| Prompt | `prompts/es-prompt.md` | Generation instructions (9 steps) |
| Validator | `validators/es-validator.md` | Pass/fail evaluation (5 hard gates) |

### Portfolio Evolution Signal (PES)

Cross-initiative synthesis. Inputs: ≥2 frozen Engagement Records + optional individual ESes. Produces improvement proposals for governing prompt and spec files. ID format: `PES-{NNN}`.

| File | Location | Purpose |
|------|----------|---------|
| Spec | `specs/pes-spec.md` | Content rules and 5 hard gates |
| Template | `artifacts/pes-template.md` | PES structure (7 sections) |
| Prompt | `prompts/pes-prompt.md` | Generation instructions (8 steps) |
| Validator | `validators/pes-validator.md` | Pass/fail evaluation (5 hard gates) |

---

## Principles

| File | Scope |
|------|-------|
| `principles/insight-evolution-principles.md` | Pattern recognition standards, feedback loop governance, ES scope rules |

---

## Examples

| Path | What it shows |
|------|---------------|
| `examples/basic-evolution/` | Evolution Signal for TaskFlow notification-service after 2 RHR periods — PASS validator output included |
| `examples/portfolio-evolution/` | Portfolio Evolution Signal across 3 TaskFlow initiatives — cross-initiative patterns, improvement proposals, PASS validator output included |

---

## Tests

| File | Contents |
|------|----------|
| `tests/kit-test-plan.md` | Structural integrity checks (S-01 to S-09) + flow scenarios (F-00 to F-02) |
