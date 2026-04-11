# Portfolio Evolution Signal

---

## 1. Document Control

| Field | Value |
|-------|-------|
| PES ID | PES-{NNN} |
| Initiative Scope | {list all initiative names included} |
| ER References | {ER-XXX (Active), ER-XXX (Deprecated), ...} |
| ES References | {ES-XXX (Frozen), ... or None} |
| Analysis Period | {YYYY-MM-DD through YYYY-MM-DD} |
| Author | AI-generated, human-reviewed |
| Status | Draft |
| Governance Model Version | 1.0 |
| Prompt Version | {prompt version} |
| Spec Version | {spec version} |
| Principles Version | {principles file versions} |

---

## 2. Engagement Coverage Summary

| ER ID | Initiative | Layers Covered | Time Range | Final ES Signal |
|-------|-----------|---------------|-----------|----------------|
| ER-XXX | {initiative name} | L2–L{N} | YYYY-MM-DD to YYYY-MM-DD | maintain / watch / re-discover / No ES |
| ER-XXX | {initiative name} | L2–L{N} | YYYY-MM-DD to YYYY-MM-DD | maintain / watch / re-discover / No ES |

---

## 3. Cross-Initiative VH Pattern Analysis

### VH Verdict Distribution

| Initiative | ER ID | Final VH Verdict |
|-----------|-------|-----------------|
| {initiative} | ER-XXX | Validated / Invalidated / Partially Validated / Insufficient Data / N/A |

**Summary:** {X} of {N} initiatives with VH data: {count} Validated, {count} Invalidated, {count} Partially Validated, {count} Insufficient Data.

### SM Category Analysis

| SM Category | Total Metrics | Achieved | Missed | Insufficient Data |
|------------|--------------|---------|--------|-----------------|
| User Behavior | | | | |
| Usage / Adoption | | | | |
| Churn / Retention | | | | |
| Financial | | | | |
| Operational | | | | |

**Source:** ER §7 Final VH Verdict and individual ES §3 SM-N tables for initiatives with available data.

### Pattern Finding

{State at least one finding about what makes VH outcomes succeed or fail, citing specific ER IDs or ES §3 data. Or state: "Insufficient data — fewer than 3 VH assessments available for pattern analysis."}

---

## 4. Cross-Initiative Reliability Patterns

### SLO Miss Pattern

{Which SLO types appear most frequently as misses across initiatives? Cite ER §5 or ES §4 data.}

| SLO Type | Miss Count | Initiatives Affected | Notes |
|----------|-----------|---------------------|-------|
| | | | |

### First-Period Vulnerability

{Do SLO misses cluster in the first RHR period? Cite ES data.}

### Architecture and Operational Correlations

{Patterns correlating architecture choices, team size, deployment practices, or service type with reliability degradation. Or: "Insufficient data to establish architecture correlations."}

### Root Cause Class Frequencies

| Root Cause Class | Incident Count | Initiatives Affected |
|-----------------|---------------|---------------------|
| | | |

{Source citations from ER §5 IR data or ES §4.}

---

## 5. Assumption Pattern Analysis

### Assumption Category Breakdown

| Category | Total Assumptions | Validated in EL | Invalidated in EL | Not Tested |
|---------|------------------|----------------|------------------|-----------|
| User Behavior | | | | |
| Technical Capacity | | | | |
| Integration Dependencies | | | | |
| Market Timing | | | | |
| Organizational Alignment | | | | |

**Source:** ER §2 artifact tables and pivot decision records.

### Under-Validated Categories

{Which assumption categories were most often carried forward to DPRD without experimental validation? Cite ER §2 data.}

### Pivot Correlation

{Did specific assumption categories correlate with pivot decisions? Cite PDR data from ER §2.}

---

## 6. Improvement Proposals

{Either a table of proposals or the no-proposals statement. Do not leave blank.}

| Proposal ID | Affected File | Observed Pattern | Proposed Change |
|-------------|--------------|-----------------|----------------|
| P-1 | {docs/prompts/xxx-prompt.md} | {§N finding — specific citation} | {Concrete change description. Next step: governance-model.md §15 amendment process.} |

*Or:* No improvement proposals — all observed patterns are consistent with current governing files. {1–2 sentence rationale.}

---

## 7. Freeze Declaration

This Portfolio Evolution Signal has been generated from Engagement Records with confirmed Frozen or terminal status, reviewed for completeness and consistency with the input data, and is ready for validation.

| Field | Value |
|-------|-------|
| Prepared By | AI-generated, human-reviewed |
| Date | {YYYY-MM-DD} |
| Input ERs Confirmed Active/Frozen/Deprecated | Yes |
