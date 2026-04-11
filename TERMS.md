# TERMS

This document defines common terms used throughout the **aieos-insight-evolution-kit** repository.
These definitions are intentionally **tool-agnostic**, **employer-neutral**, and **AI-friendly**.

---

## Core Concepts

### SDLC (Software Development Lifecycle)
The end-to-end process of delivering software, from intent and requirements through design, implementation, testing, deployment, and operation.

---

### AI-Assisted Delivery
The use of AI systems to **draft, decompose, validate, and analyze** SDLC artifacts.
AI may generate content, but **humans approve and decide**.

---

### Artifact
A persisted document produced at a specific stage of the SDLC.
Artifacts are **promoted**, not rewritten, as work progresses downstream.

---

### Artifact Promotion
The formal progression of an artifact to the next stage after passing validation.
Once promoted, an artifact is considered **frozen**.

---

### Freeze
A state indicating an artifact is approved and may not be reinterpreted, expanded, or redesigned by downstream artifacts without explicit re-entry.

---

## Insight & Evolution Artifacts

### Evolution Signal (ES)
A per-service synthesis artifact that assesses whether the original value hypothesis was realized in production, identifies reliability trends, and recommends whether a system should continue, be watched, or trigger new discovery.

ID format: `ES-{SCOPE}-{NNN}`

---

### Portfolio Evolution Signal (PES)
A cross-initiative synthesis artifact that synthesizes patterns across multiple initiatives from their Engagement Records. Produces improvement proposals for governing prompt and spec files.

ID format: `PES-{NNN}`

---

### Re-Entry Signal
The disposition declared in an Evolution Signal's §5. One of three values:
- `maintain` — system is performing as expected; no action required
- `watch` — reliability trends warrant closer monitoring before next action
- `re-discover` — a new discovery engagement is recommended for the Product Intelligence Kit

The re-entry signal is **advisory** — a human product owner decides whether to act.

---

### Value Hypothesis (VH)
An optional input to the Evolution Signal from the Product Intelligence Kit. Defines the testable value bet that the system was expected to realize. The ES assesses whether this hypothesis was confirmed, disconfirmed, or inconclusive.

---

### Reliability Health Report (RHR)
The primary input to the Evolution Signal, produced by the Reliability & Resilience Kit (Layer 6). Records SLO compliance, error budget state, and incident summary for a service over a reporting period. The ES requires a minimum of two frozen RHRs.

---

### Engagement Record (ER)
A project-level artifact that spans all kit layers. Maintains an index of every artifact ID, outcome, and key decision for one initiative. ERs are the primary input to Portfolio Evolution Signals.

---

## Validation & Governance

### Validator
A strict, non-prescriptive quality gate that evaluates whether an artifact is ready to be promoted.
Validators **do not redesign or suggest solutions**.

---

### Hard Gate
A binary pass/fail criterion that an artifact must satisfy to be promoted. Hard gates are non-negotiable.

---

### Evidence-Based Pattern
A pattern identified in a Portfolio Evolution Signal that is backed by specific artifact IDs, section references, and outcome data. Patterns without supporting evidence references are rejected by the PES validator.

---

## Key Principles (Terminology Anchors)

- **AI generates, validators decide**
- **Artifacts are promoted, not rewritten**
- **Signals are evidence-based, not opinion-based**
- **Ambiguity is a failure condition**

---

## Notes

- These terms are intentionally generic.
- No terminology implies a specific employer, vendor, or tool.
- Examples in this repository use placeholder names and neutral contexts (TaskFlow notification-service).
