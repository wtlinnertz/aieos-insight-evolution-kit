# Evolution Signal Spec

## Purpose

This spec defines the content rules, format requirements, and hard gates for the Evolution Signal (ES). It is the single source of truth for what constitutes a valid ES. The ES prompt and ES validator reference this spec — they never inline their own rules.

---

## 1. Artifact Overview

The Evolution Signal synthesizes frozen Reliability Health Reports (and optionally a frozen Value Hypothesis) into:
- A VH outcome assessment (was the original value bet realized in production?)
- A reliability trend analysis (is the system improving, stable, or degrading?)
- A pattern analysis (what systemic patterns emerged?)
- A re-entry signal (should this system continue, be watched, or trigger new discovery?)
- Recommended actions with owners

The ES closes the Layer 6 → Layer 7 → Layer 2 feedback loop.

---

## 2. Input Requirements

### Required Inputs

- **Minimum 2 frozen RHRs** for the same service or set of related services. The coverage period must be stated and any gaps must be explicitly noted.
- **All cited artifacts must be in Frozen status** — Draft, Validated, or Freeze Pending artifacts are not acceptable inputs.

### Optional Inputs

- **A frozen Value Hypothesis (VH)** from the Product Intelligence Kit — for assessing whether original product bets were validated by production behavior. If not provided, §3 VH Outcome Assessment must explicitly state "VH not provided; outcome assessment deferred."

### Inputs That Do Not Belong Here

- Raw metrics, logs, or monitoring data (these belong in the RHR, not the ES)
- Unfrozen or draft artifacts
- Strategic direction documents (the ES feeds Layer 2, not Layer 1)

---

## 3. Section Rules

### §1 Document Control

Must contain:
- ES ID (format: `ES-{SCOPE}-{NNN}` where scope = service name or `PORTFOLIO`)
- Service(s) covered
- Coverage period (must match the RHR coverage period(s))
- List of input artifacts (each with ID and status: Frozen)
- VH reference (VH artifact ID, or "Not provided")
- Governance Model Version
- Prompt Version
- Status

All cited input artifacts must show Frozen status. Draft or non-frozen inputs are a hard gate failure.

### §2 Coverage Summary

A table listing each RHR with: RHR ID, service, coverage period, SRP version in effect, and a brief SLO compliance summary. The table must have at least 2 rows (minimum 2 frozen RHRs required).

Gaps in coverage must be explicitly noted in this section. Contiguous coverage is preferred but not required — gaps must be explained.

### §3 VH Outcome Assessment

**If a VH is provided:**
For each SM-N success metric from the VH, provide: the metric, the observed production data or proxy, and an outcome verdict (Achieved / Missed / Insufficient Data). Conclude with an overall verdict: Validated / Invalidated / Partially Validated / Insufficient Data.

**If no VH is provided:**
Write exactly: "VH not provided; outcome assessment deferred."

**Blank is not accepted.** An ES with an empty §3 fails the `vh_assessment_explicit` hard gate.

### §4 Reliability Trend Analysis

For each SLO tracked in the covered RHRs:
- Trend direction: improving / stable / degrading (with basis)
- Error budget trajectory: healthy / at risk / exhausted (with basis)

Incident pattern analysis:
- Frequency trend (increasing / stable / decreasing)
- Severity distribution
- Repeat root cause classes (if any)

If this is the first ES for this service (only 2 RHRs available), note that trend data is limited and only initial patterns can be established.

### §5 Pattern Analysis

Four subsections required:
1. **Persisting patterns** — issues or behaviors present across all covered periods
2. **Resolved patterns** — issues present in earlier periods but absent in recent periods
3. **New patterns** — issues first appearing in the most recent period
4. **Cross-service patterns** — if multi-service: patterns that appear across multiple services

Each subsection must be addressed. Use "None identified" with a brief explanation if a category is empty — blank subsections are not accepted.

### §6 Re-Entry Signal

Exactly one of three values, followed by a 2–4 sentence rationale:

- **`maintain`** — System is operating within expectations; no discovery action recommended. The rationale should explain what "expectations" means here (e.g., SLO targets met, error budgets healthy, incident rate stable).
- **`watch`** — System shows concerning signals that do not yet warrant discovery but require elevated monitoring or targeted engineering attention. The rationale should specify what is concerning and what to watch for.
- **`re-discover`** — System behavior or value delivery is sufficiently different from the original hypothesis to warrant a new discovery engagement. The rationale must include a discovery question: the specific question a new PIK engagement should investigate.

Multiple signals (e.g., `maintain` for one SLO and `re-discover` for another) are not permitted. Synthesize into a single signal. If signals conflict, escalate to `re-discover`.

### §7 Recommended Actions

A table with at least one row. Columns: Action, Type (Operational / Discovery / Engineering), Rationale, Owner.

- `Operational` — monitoring, alerting, runbook, or reliability practice changes
- `Discovery` — new product investigation (correlates with `re-discover` signal)
- `Engineering` — technical debt, architecture, or implementation changes

Owner may be "TBD" if not yet assigned.

### §8 Freeze Declaration

Standard freeze declaration confirming the ES is complete, inputs are frozen, and the document is ready for validation.

---

## 4. Hard Gates

Five hard gates govern this artifact. All five must PASS for the validator to return PASS.

### Gate 1: `coverage_adequacy`

**Description:** The ES references at least 2 frozen RHRs as input. The §2 Coverage Summary table has at least 2 rows. The coverage period is stated. If there are gaps in coverage, they are explicitly noted.

**Failure condition:** Fewer than 2 RHRs cited; §2 table has fewer than 2 rows; coverage period is not stated; gaps exist but are not noted.

### Gate 2: `inputs_frozen`

**Description:** Every artifact cited in §1 Document Control (RHRs, SRPs, and any cited VH) is confirmed in Frozen status.

**Failure condition:** Any cited artifact shows status other than Frozen. Draft, Validated, or Freeze Pending artifacts are not acceptable inputs.

### Gate 3: `vh_assessment_explicit`

**Description:** §3 is not blank. It either contains a complete VH outcome assessment (with SM-N metrics mapped to observed data and an overall verdict) or contains exactly the phrase "VH not provided; outcome assessment deferred."

**Failure condition:** §3 is blank, contains a placeholder like "TBD" or "See VH," or contains partial content that neither completes the assessment nor explicitly defers it.

### Gate 4: `re_entry_signal_valid`

**Description:** §6 contains exactly one of the three valid signals: `maintain`, `watch`, or `re-discover`. The signal is followed by a rationale of 2–4 sentences. If the signal is `re-discover`, the rationale includes a discovery question.

**Failure condition:** §6 contains no signal, an invalid signal, multiple signals, a signal without rationale, or a `re-discover` signal without a discovery question.

### Gate 5: `actions_present`

**Description:** §7 contains at least one recommended action with: an action description, a type (Operational / Discovery / Engineering), a rationale, and an owner (which may be "TBD").

**Failure condition:** §7 is empty or contains only placeholder text; an action row is missing its type, rationale, or owner column.

---

## 5. Quality Criteria (Non-Gate)

These criteria inform completeness score but do not affect pass/fail:

- **Trend analysis depth**: §4 addresses all SLOs tracked across the covered RHRs, not just the ones that had issues.
- **Pattern specificity**: §5 patterns are named and described specifically, not vaguely (e.g., "database connection exhaustion during peak load" not "database issues").
- **Action specificity**: §7 actions are concrete and actionable, not generic (e.g., "add alerting for connection pool at 80% capacity" not "improve monitoring").
- **VH traceability**: §3 references specific SM-N identifiers from the VH, not paraphrased summaries.

---

## 6. Scope Rules

- The ES must not introduce new requirements, goals, or scope for the system it covers. It observes and synthesizes — it does not redesign.
- The ES may recommend that a new PIK discovery engagement investigate a specific question, but it may not prescribe the answer to that question.
- The ES covers the time period spanned by its input RHRs. It may not extrapolate beyond that period.
