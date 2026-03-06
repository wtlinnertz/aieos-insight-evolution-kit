# Insight & Evolution Kit — Test Plan

This document contains the structural integrity checks and flow scenario tests for the Insight & Evolution Kit. These tests verify that the kit is complete, internally consistent, and capable of producing valid Evolution Signals.

---

## Structural Integrity Checks

Structural checks verify that the kit's files are present, properly named, and internally consistent. These checks do not require AI — they are verifiable by inspection.

---

### S-01: Four-File Completeness

**Check:** The governed artifact type (ES) has exactly four files: spec, template, prompt, validator.

| Artifact Type | Spec | Template | Prompt | Validator |
|---------------|------|----------|--------|-----------|
| ES | docs/specs/es-spec.md | docs/artifacts/es-template.md | docs/prompts/es-prompt.md | docs/validators/es-validator.md |

**Expected result:** All four files present.

---

### S-02: Hard Gate Count Alignment

**Check:** The spec's declared hard gate count matches the validator's gate checks.

| Artifact Type | Spec Hard Gates | Validator Gates |
|---------------|----------------|----------------|
| ES | 5 | 5 |

**Expected result:** Count matches.

---

### S-03: Hard Gate Name Alignment

**Check:** Gate names in the spec match gate names in the validator (exact string match for JSON output field names).

| Artifact | Spec Gate Names | Validator Gate Names |
|----------|----------------|---------------------|
| ES | coverage_adequacy, inputs_frozen, vh_assessment_explicit, re_entry_signal_valid, actions_present | coverage_adequacy, inputs_frozen, vh_assessment_explicit, re_entry_signal_valid, actions_present |

**Expected result:** All gate names match exactly.

---

### S-04: Prompt-to-Spec Reference Integrity

**Check:** The generation prompt references the correct spec and template. The prompt does not inline content rules.

| Prompt | References Spec | References Template | Inlines Rules? |
|--------|----------------|--------------------|----|
| es-prompt.md | docs/specs/es-spec.md | docs/artifacts/es-template.md | No |

**Expected result:** Prompt references correct spec and template; no inlined rules.

---

### S-05: Validator-to-Spec Reference Integrity

**Check:** The validator references its spec as the source of truth. The validator does not reference the prompt.

| Validator | References Spec | References Prompt? |
|-----------|-----------------|-------------------|
| es-validator.md | docs/specs/es-spec.md | No |

**Expected result:** Validator references correct spec; does not reference the prompt.

---

### S-06: Template Section Alignment

**Check:** The template's section headings match the required sections listed in the corresponding spec.

| Artifact | Spec Required Sections | Template Sections |
|----------|----------------------|-------------------|
| ES | Document Control, Coverage Summary, VH Outcome Assessment, Reliability Trend Analysis, Pattern Analysis, Re-Entry Signal, Recommended Actions, Freeze Declaration | §1–§8 (all present) |

**Expected result:** Template sections match spec required sections.

---

### S-07: Governance Model Sync

**Check:** `docs/governance-model.md` matches `aieos-spec/governance-model.md` exactly (byte-for-byte).

**Procedure:** Diff the two files. Any difference is a sync failure.

**Expected result:** Zero diff between kit copy and aieos-spec canonical.

---

### S-08: Example Artifact Coverage

**Check:** The worked examples cover both artifact types with corresponding validator outputs.

| Example File | Artifact Type | Validator Output |
|-------------|---------------|----------------|
| examples/basic-evolution/00-evolution-signal.md | ES | validator-outputs/es-pass.json |
| examples/portfolio-evolution/00-portfolio-evolution-signal.md | PES | validator-outputs/pes-pass.json |

**Expected result:** Both examples present; both validator outputs present with status: PASS.

---

### S-09: PES Four-File Completeness

**Check:** The PES artifact type has exactly four files: spec, template, prompt, validator.

| Artifact Type | Spec | Template | Prompt | Validator |
|---------------|------|----------|--------|-----------|
| PES | docs/specs/pes-spec.md | docs/artifacts/pes-template.md | docs/prompts/pes-prompt.md | docs/validators/pes-validator.md |

**Expected result:** All four files present.

---

## Flow Scenario Tests

Flow scenarios verify that the kit's artifacts, when produced in order with appropriate inputs, pass validation. These tests require AI execution.

---

### F-00: Normal Evolution Flow (maintain signal)

**Scenario:** Receive 2 frozen RHRs from a stable service → provide optional frozen VH → generate Evolution Signal → validate → freeze. Signal is `maintain`.

**Setup:**
- Provide: RHR-NOTIF-001 and RHR-NOTIF-002 from the worked example (both Frozen)
- Provide: VH-TASKFLOW-NOTIF-001 (Frozen)
- Load es-prompt.md + es-spec.md + es-template.md
- Generate ES → validate

**Expected outcomes:**
- ES: all 5 hard gates PASS
- §2: 2 rows in coverage summary; contiguous coverage stated
- §3: SM-1 through SM-6 assessed; SM-5 and SM-6 are Insufficient Data; overall verdict is Partially Validated
- §6: signal is `maintain` (all SLOs met across both periods, structural issue resolved)
- §7: at least 1 recommended action with type, rationale, and owner

**Key gate to verify:** `vh_assessment_explicit` — confirm that SM-1 through SM-6 are individually addressed, not summarized. Insufficient Data verdicts must be present for SM-5 and SM-6.

---

### F-01: Re-Discover Signal Flow

**Scenario:** Receive 2 frozen RHRs showing consecutive SLO misses and a VH with missed outcome metrics → generate ES → signal is `re-discover`.

**Setup:**
- Provide: Two fictional RHRs showing Delivery Rate SLO Missed in both periods (actual compliance ~98.2% vs 99.0% target)
- Provide: A VH with SM-1 (notification disable rate) still at 11.5% after 90 days (target: <6%) — no improvement
- Load es-prompt.md + es-spec.md + es-template.md
- Generate ES → validate

**Expected outcomes:**
- ES: all 5 hard gates PASS
- §3: SM-1 verdict is Missed; overall verdict is Invalidated or Partially Validated
- §4: Delivery Rate trend is Degrading across both periods
- §5: Persisting pattern: Delivery Rate SLO miss pattern present in both periods
- §6: signal is `re-discover`; rationale is 2–4 sentences; rationale includes a specific discovery question
- §7: at least one Discovery-type action

**Key gate to verify:** `re_entry_signal_valid` — confirm the discovery question is specific and actionable, not vague. "Why are notifications failing?" is not sufficient. "Delivery rate SLO has been missed in two consecutive periods despite no incidents; the root cause may be in configuration, capacity, or routing — a targeted investigation is needed" is the kind of rationale expected.

---

### F-02: Portfolio Evolution Flow (evidence-grounded proposal)

**Scenario:** Receive 3 Engagement Records with completed ES entries → generate Portfolio Evolution Signal → validate. PES produces one improvement proposal grounded in a cross-initiative pattern.

**Setup:**
- Provide: ER-TASKFLOW-NOTIF-001, ER-TASKFLOW-ATTACH-001, ER-TASKFLOW-FEED-001 (all Active, all with ES §6 signals)
- Provide: ES-NOTIFICATION-SVC-001, ES-ATTACH-SVC-001, ES-FEED-SVC-001 (all Frozen)
- Load pes-prompt.md + pes-spec.md + pes-template.md
- Generate PES → validate

**Expected outcomes:**
- PES: all 5 hard gates PASS
- §2: 3 rows in engagement coverage summary
- §3: VH verdict distribution table present; SM category analysis table present; at least one pattern finding cites specific ER or ES IDs
- §4: SLO miss pattern table present; first-period vulnerability finding present with numeric basis
- §5: assumption category breakdown table present; under-validated categories identified with ER citations
- §6: at least one improvement proposal with all four fields populated; Affected File is a specific path; Observed Pattern cites a §3–§5 section and finding; Proposed Change is concrete and implementable

**Key gate to verify:** `patterns_evidence_based` — confirm that §3 Finding 1 cites specific ER IDs and a count ("all 3 churn/retention metrics"), not a vague generalization. `proposals_complete` — confirm the Affected File is `docs/prompts/es-prompt.md` (or equivalent specific path), not a generic reference.

---

## Notes

- All structural checks (S-01 through S-09) should be verified before running flow scenarios.
- Run flow scenarios in separate AI sessions for generation and validation — do not validate in the same session that generated the artifact.
- The check-structure.sh script in `aieos-spec/tests/` can validate structural checks S-01 through S-06 automatically.
- F-02 requires Engagement Records as input. Use the ER format from `aieos-spec/docs/engagement-record-spec.md` to construct test ERs if working ERs are not available.
