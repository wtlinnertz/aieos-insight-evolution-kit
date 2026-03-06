# How to Adapt This Kit to Your Organization

This document explains how to adopt the Insight & Evolution Kit in your organization, including how to configure the feedback loop cadence, how to adapt the re-entry signal thresholds, and how to scope the kit to your environment.

---

## What You Configure vs. What You Keep

**You configure:**
- Feedback loop cadence — how often you run Evolution Signals (quarterly, after N RHRs, after significant events)
- Re-entry signal sensitivity — what SLO miss rate or pattern persistence triggers `re-discover` vs. `watch`
- Whether VH assessments are standard practice or optional
- Who reviews and approves the re-entry signal (product owner, engineering lead, both)
- How `re-discover` recommendations are actioned (who decides, what process kicks off)

**You keep (process architecture — not configurable):**
- The minimum of 2 frozen RHRs as input (hard gate — cannot be reduced)
- The three-signal model: maintain / watch / re-discover (replacing these with custom signals breaks cross-kit compatibility)
- The VH assessment explicit/deferred rule (§3 must not be blank)
- The separation of generation and validation sessions
- The freeze-before-promote rule (all inputs must be Frozen before ES generation)

The structural rules are in `docs/governance-model.md`. They exist because the kit's value depends on them.

---

## Configuring the Feedback Loop Cadence

The Evolution Signal is event-driven, not calendar-driven. The trigger is accumulating enough frozen RHRs to establish meaningful trends — not the passage of time alone.

**Typical patterns:**

| Organization Type | Typical ES Cadence |
|------------------|--------------------|
| High-velocity product teams (weekly deploys, active iteration) | After every 2–3 RHR cycles (quarterly to semi-annual) |
| Stable platform services (low change rate) | After 4–6 RHR cycles (semi-annual to annual) |
| Post-major-launch assessment | After the first 2 post-launch RHR periods |
| Specific concern flagged in RHR §5 | After the next 2 periods following the concern flag |

The minimum is always 2 RHRs. Running an ES with only 1 RHR is structurally invalid — you cannot detect trends from a single data point.

---

## Configuring Re-Entry Signal Thresholds

The ES spec defines the hard gate rules for what makes each signal valid. Within those rules, your organization defines its own sensitivity thresholds:

### `re-discover` trigger criteria examples

| Signal | Conservative (stable orgs) | Aggressive (fast-moving orgs) |
|--------|--------------------------|------------------------------|
| SLO Miss | 3+ consecutive periods | 2 consecutive periods |
| Error Budget | Exhausted + 2 freeze events | Exhausted once |
| VH Divergence | >25% miss on key metrics | >10% miss on key metrics |
| Incident frequency | Increasing 3 periods | Increasing 2 periods |

Document your thresholds in the `insight-evolution-principles.md` file. The ES prompt uses these principles as input context.

### `watch` trigger criteria examples

- Error budget At Risk (not yet exhausted) for 1+ periods
- New pattern appeared in most recent RHR with unknown root cause
- Incident severity distribution shifted (more SEV2, fewer SEV3)
- VH metric showing Insufficient Data for 2+ periods (data gap, not miss)

---

## Scoping: Single-Service vs. Portfolio

### Single-Service ES

The most common pattern: one ES covers one service across 2+ RHR periods. The ES ID is `ES-{SERVICE-NAME}-{NNN}`.

### Portfolio ES

A portfolio ES synthesizes signals across multiple services. Use `ES-PORTFOLIO-{NNN}` as the ES ID. The §2 Coverage Summary includes one row per RHR (which may span multiple services). §5 Cross-Service Patterns becomes the most important section.

Portfolio ESs are more complex and require more synthesis time. They are most valuable for:
- Platform services that share common infrastructure
- Product areas with multiple dependent services
- Pre-roadmap prioritization (which services most need investment?)

### When NOT to Use a Portfolio ES

Do not merge services with fundamentally different operational characteristics into a single ES just for convenience. If service A is a high-traffic user-facing API and service B is a low-frequency batch processor, they warrant separate ESs. Portfolio ESs should synthesize services with meaningful comparative value.

---

## Integrating with the Product Intelligence Kit

The feedback loop from Layer 7 to Layer 2 is advisory. If your organization uses PIK for discovery, the `re-discover` signal produces a discovery question that feeds the PIK Discovery Intake Form.

**What the ES provides:**
- A specific discovery question (§6)
- Discovery-type recommended actions (§7)
- The full historical context: trend data, pattern analysis, VH outcome assessment

**What the PIK does with it:**
- The product owner reviews the ES and decides whether to initiate a discovery engagement
- If yes: the discovery question becomes the frame for the PFD; the ES is cited as input context
- The new discovery engagement produces new artifact IDs — it does not modify the frozen ES

**If your organization does not use PIK:**
A `re-discover` signal means: a product owner should investigate whether a new initiative is warranted, using whatever discovery process your organization follows. The ES provides the evidence; the process is yours to define.

---

## Adapting the Principles File

The `docs/principles/insight-evolution-principles.md` file defines your organization's standards for:
- What counts as a meaningful signal
- How the feedback loop works
- What each re-entry signal means operationally

To adapt it:
1. Update the thresholds in §1 (Pattern Recognition Standards) to match your organization's risk tolerance.
2. Update §2 (Feedback Loop Governance) to reflect your re-entry decision-making process.
3. Bump the version field per the principle file versioning protocol (`aieos-governance-foundation/docs/principle-file-standard.md`).

Do not remove the enforcement mapping at the end of the principles file — it documents how the principles connect to the kit's hard gates.
