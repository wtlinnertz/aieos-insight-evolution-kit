# Portfolio Evolution Signal Spec

Version: v1.1

## Purpose

This spec defines the content rules, format requirements, and hard gates for the Portfolio Evolution Signal (PES). It is the single source of truth for what constitutes a valid PES. The PES prompt and PES validator reference this spec — they never inline their own rules.

---

## 1. Artifact Overview

The Portfolio Evolution Signal synthesizes multiple Engagement Records (and optionally their individual Evolution Signals) across two or more initiatives into:
- A cross-initiative VH pattern analysis (which categories of value bets validate, miss, or produce insufficient data)
- A cross-initiative reliability pattern analysis (which SLO types and architecture patterns correlate with degradation)
- An assumption pattern analysis (which assumption categories fail most often in discovery)
- Evidence-grounded improvement proposals for the governing prompt or spec files, or an explicit statement that no changes are warranted

The PES closes the cognitive loop: observed patterns → improvement proposals → governance amendment process → better prompts → better artifacts.

---

## What This Artifact Is Not

- **Not a strategic roadmap or organizational diagnosis.** The PES synthesizes observed patterns across initiatives into improvement proposals for governing prompts and specs — not organizational recommendations, headcount decisions, or product strategy.
- **Not a performance review.** The PES identifies patterns in value bets, reliability, and assumptions across the portfolio. It does not evaluate teams, individuals, or specific product decisions.
- **Not a substitute for individual ESes.** The PES provides cross-initiative pattern synthesis; it does not replace or summarize individual Evolution Signals. Both operate at different scopes.

---

## 2. Input Requirements

### Required Inputs

- **Minimum 2 frozen Engagement Records (ERs)** from separate initiatives. ERs must be in `Active` or `Deprecated` status — `Abandoned` ERs with no Layer 7 entry may be included for assumption pattern analysis but must not anchor VH or reliability findings.
- **All cited ERs must be Frozen** — meaning the ER has at least one frozen ES entry in §6, or the ER has reached a terminal state.

### Optional Inputs

- **Individual frozen Evolution Signals (ESes)** for any initiatives in scope — to provide richer §4 reliability data than the ER summaries alone contain.
- **ER §16 Impact Attribution data** across multiple ERs (if adopted) — for cross-initiative execution pattern analysis such as contribution concentration, role distribution across layers, and correlation between team composition patterns and initiative outcomes.

### What Does Not Belong Here

- Draft or non-frozen ERs
- Raw metrics, logs, or monitoring data (these belong in RHRs, not in PES)
- Strategic direction or organizational goals (the PES observes patterns; it does not prescribe strategy)

---

## 3. Section Rules

### §1 Document Control

Must contain:
- PES ID (format: `PES-{NNN}`)
- Initiative scope: list of all initiative names included in this analysis
- ER references: list of ER IDs with status (Frozen / Active / Deprecated)
- ES references: list of individual ES IDs included, if any (may be empty)
- Analysis period: the date range spanned by the oldest and most recent ER entries included
- Governance Model Version
- Prompt Version
- Status

All cited ERs must show Frozen, Active, or Deprecated status. Draft ERs are not acceptable inputs.

### §2 Engagement Coverage Summary

A table listing each included ER with: ER ID, initiative name, layers covered (e.g., "L2–L7"), time range, and final ES signal from §7 Initiative Outcome. The table must have at least 2 rows.

If an ER has no ES yet (initiative still active), note "No ES" in the final ES signal column.

### §3 Cross-Initiative VH Pattern Analysis

Analyze VH outcomes across all initiatives where a VH was produced.

Required subsections:
1. **VH verdict distribution** — Count and percentage of Validated / Invalidated / Partially Validated / Insufficient Data verdicts across all initiatives. Cite the ER §7 Final VH Verdict for each.
2. **SM category analysis** — Group SM-N metrics by category (user behavior, usage metrics, churn/retention, financial, operational). For each category: how many metrics Achieved vs Missed vs Insufficient Data across all initiatives.
3. **Pattern finding** — Identify at least one finding about what makes VH outcomes succeed or fail (or state "Insufficient data — fewer than 3 VH assessments available for pattern analysis").

If fewer than 2 initiatives produced a VH, state: "VH pattern analysis deferred — fewer than 2 initiatives have VH outcome data."

### §4 Cross-Initiative Reliability Patterns

Analyze reliability outcomes across all initiatives where RHRs and ESes were produced.

Required subsections:
1. **SLO miss pattern** — Which SLO types (Error Rate, Latency, Throughput, Delivery Rate, etc.) appear most frequently as misses across initiatives? Cite ER §5 or ES §4 data for each finding.
2. **First-period vulnerability** — Do SLO misses cluster in the first RHR period (within 60 days of release)? Cite the ES data.
3. **Architecture or operational correlations** — Are there patterns correlating architecture choices, team size, deployment practices, or service type with reliability degradation? Note explicitly if insufficient data. If ER §16 data is available across multiple initiatives, note any correlations between contributor patterns (e.g., single-Primary vs. distributed contribution) and reliability outcomes. This is optional and advisory — absence of §16 data does not affect this subsection.
4. **Root cause class frequencies** — Which incident root cause classes appear most often across incidents? Cite ER §5 IR data or ES §4.

If fewer than 2 initiatives produced RHRs, state: "Reliability pattern analysis deferred — fewer than 2 initiatives have reliability data."

### §5 Assumption Pattern Analysis

Analyze assumption outcomes across all initiatives where an AR and EL were produced.

Required subsections:
1. **Assumption category breakdown** — Group AR assumptions by category (user behavior, technical capacity, integration dependencies, market timing, organizational alignment). For each category: how many assumptions were validated vs invalidated vs not tested in EL?
2. **Under-validated categories** — Which assumption categories were most often carried forward to DPRD without experimental validation? Cite the ER §2 pivot decision data.
3. **Pivot correlation** — Did specific assumption categories correlate with pivot decisions? Cite PDR data from ER §2.

If fewer than 2 initiatives produced an AR, state: "Assumption pattern analysis deferred — fewer than 2 initiatives have assumption data."

### §6 Improvement Proposals

**If patterns in §3–§5 identify gaps in the governing files:**

A table with at least one row. Required columns: Proposal ID | Affected File | Observed Pattern | Proposed Change

- `Proposal ID`: Sequential within this PES (P-1, P-2, ...)
- `Affected File`: The specific prompt, spec, template, or validator file that should change (e.g., `docs/prompts/es-prompt.md`, `docs/specs/vh-spec.md`)
- `Observed Pattern`: A specific citation from §3, §4, or §5 — not a vague generalization. Include the section and finding number.
- `Proposed Change`: A concrete description of what should change in the affected file and why. Specific enough that a kit maintainer can implement it without re-reading the full PES.

Each proposal must reference the governance amendment process (governance-model.md §15) as the required next step before implementation.

**If no changes are warranted:**

Write exactly: "No improvement proposals — all observed patterns are consistent with current governing files." Follow with a 1–2 sentence rationale explaining what was examined and why no changes are needed.

**Blank is not accepted.** A PES with an empty §6 fails the `synthesis_present` hard gate.

### §7 Freeze Declaration

Standard freeze declaration confirming the PES is complete, inputs are Frozen, and the document is ready for validation.

---

## 4. Hard Gates

Five hard gates govern this artifact. All five must PASS for the validator to return PASS.

### Gate 1: `coverage_adequacy`

**Description:** The PES references at least 2 ERs as input. The §2 Coverage Summary table has at least 2 rows. The analysis period is stated in §1.

**Failure condition:** Fewer than 2 ERs cited; §2 table has fewer than 2 rows; analysis period absent from §1.

### Gate 2: `inputs_frozen`

**Description:** Every ER cited in §1 is in Frozen, Active, or Deprecated status (not Draft or Abandoned-only). If individual ESes are cited, they are in Frozen status.

**Failure condition:** Any cited ER is in Draft status; any cited ES is not Frozen.

### Gate 3: `patterns_evidence_based`

**Description:** §3, §4, and §5 pattern findings cite specific artifact IDs, ER section references, or quantified counts (e.g., "7 of 9 initiatives," "3 of 5 first-period RHRs"). General statements without citation or quantification do not satisfy this gate.

**Failure condition:** Any pattern finding in §3–§5 is a vague generalization without a specific artifact citation or numeric basis. Deferral statements ("analysis deferred — insufficient data") do not fail this gate if the deferral condition is met.

### Gate 4: `synthesis_present`

**Description:** §6 is not blank. It either contains at least one improvement proposal row, or contains exactly the no-proposals statement ("No improvement proposals — all observed patterns are consistent with current governing files") followed by a rationale.

**Failure condition:** §6 is blank; §6 contains only placeholder text ("TBD," "see findings"); §6 contains a no-proposals statement without a rationale sentence.

### Gate 5: `proposals_complete`

**Description:** If §6 contains improvement proposals, each proposal row has all four required fields populated: Proposal ID, Affected File (specific file path), Observed Pattern (§ citation), and Proposed Change (concrete description). If §6 contains only the no-proposals statement, this gate auto-passes.

**Failure condition:** Any proposal row is missing Proposal ID, Affected File, Observed Pattern citation, or Proposed Change description.

---

## 5. Quality Criteria (Non-Gate)

These criteria inform completeness score but do not affect pass/fail:

- **Proposal specificity**: §6 proposals are specific enough to implement without re-reading the full PES. Vague proposals ("improve the prompt") deduct points.
- **Pattern quantification**: §3–§5 findings include counts or percentages, not just narrative descriptions.
- **Cross-section coherence**: §6 proposals connect clearly to §3–§5 findings — the connection is explicit, not assumed.
- **Amendment process reference**: Each §6 proposal references governance-model.md §15 as the next step.
- **Cross-initiative execution patterns**: If ER §16 data is available across multiple initiatives, §4 notes correlations between contributor patterns and outcomes. If not available, this criterion does not apply.

---

## 6. Scope Rules

- The PES must not prescribe new strategic direction — it observes patterns and proposes prompt/spec improvements only.
- The PES may not modify governing files directly — it produces proposals. Approved proposals enter the §15 amendment process.
- The PES covers the time period spanned by its input ERs. It may not extrapolate beyond that period.
- The PES does not replace individual ESes — it synthesizes across them. Single-service reliability decisions belong in ESes, not in the PES.
