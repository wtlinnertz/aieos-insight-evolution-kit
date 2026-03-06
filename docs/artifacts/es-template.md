# Evolution Signal

---

## 1. Document Control

| Field | Value |
|-------|-------|
| ES ID | ES-{SCOPE}-{NNN} |
| Service(s) | {service name(s) covered} |
| Coverage Period | {YYYY-MM-DD} through {YYYY-MM-DD} |
| Input Artifacts | {list each: RHR-ID (Frozen), SRP-ID (Frozen), VH-ID (Frozen) if applicable} |
| VH Reference | {VH-{PROJECT}-{NNN} — Frozen \| Not provided} |
| Author | {author} |
| Status | Draft / Validated / Freeze Pending / Frozen |
| Governance Model Version | 1.0 |
| Prompt Version | {prompt version} |

---

## 2. Coverage Summary

| RHR ID | Service | Coverage Period | SRP Version | SLO Compliance Summary |
|--------|---------|----------------|------------|------------------------|
| {RHR-{SERVICE}-{NNN}} | {service name} | {YYYY-MM-DD to YYYY-MM-DD} | {SRP version} | {e.g., Availability: Met (99.97%). Latency: Missed (97.2% vs 99.5% target).} |
| {RHR-{SERVICE}-{NNN}} | {service name} | {YYYY-MM-DD to YYYY-MM-DD} | {SRP version} | {SLO compliance summary} |

*Add additional rows for each input RHR. Minimum 2 rows required.*

**Coverage gaps:** {Note any periods not covered by the input RHRs, or "Coverage is contiguous across all input RHRs."}

---

## 3. VH Outcome Assessment

*Complete this section if a frozen VH is provided. If no VH is available, write exactly: "VH not provided; outcome assessment deferred." Blank is not accepted.*

**VH Reference:** {VH-{PROJECT}-{NNN} | Not provided}

{If VH provided:}

| SM-N | Success Metric (from VH) | Observed Production Data | Verdict |
|------|------------------------|------------------------|---------|
| SM-1 | {metric description and target from VH} | {observed value or proxy measure with data source} | Achieved / Missed / Insufficient Data |
| SM-2 | {metric description and target} | {observed value or proxy} | Achieved / Missed / Insufficient Data |

**Overall verdict:** {Validated / Invalidated / Partially Validated / Insufficient Data}

**Basis:** {1–2 sentences explaining the overall verdict, especially if Partially Validated or Insufficient Data.}

---

## 4. Reliability Trend Analysis

### SLO Trends

| SLO Name | Trend | Basis |
|----------|-------|-------|
| {SLO 1 name} | Improving / Stable / Degrading | {brief basis — e.g., "compliance improved from 99.2% to 99.7% over 2 periods"} |
| {SLO 2 name} | Improving / Stable / Degrading | {brief basis} |

### Error Budget Trajectory

| SLO Name | Trajectory | Notes |
|----------|-----------|-------|
| {SLO 1 name} | Healthy / At Risk / Exhausted | {budget remaining at end of coverage period; trend direction} |
| {SLO 2 name} | Healthy / At Risk / Exhausted | {budget remaining; trend direction} |

### Incident Pattern

**Frequency trend:** {Increasing / Stable / Decreasing} — {brief basis: e.g., "2 incidents in period 1, 1 incident in period 2"}

**Severity distribution:** {breakdown across SEV levels for the full coverage period}

**Repeat root cause classes:** {Describe any root cause categories that appeared in multiple incidents, or "No repeat root cause classes identified."}

---

## 5. Pattern Analysis

### Persisting Patterns

{Issues or behaviors observed across all covered periods. For each: describe the pattern, when it was first observed, and whether it is worsening, stable, or improving.}

*If none: "None identified: [brief explanation — e.g., all SLOs met across all periods, no recurring incidents]."*

### Resolved Patterns

{Issues present in earlier periods but absent in recent periods. For each: describe what was resolved and when.}

*If none: "None identified."*

### New Patterns

{Issues appearing for the first time in the most recent RHR period.}

*If none: "None identified."*

### Cross-Service Patterns

{If this ES covers multiple services: patterns that appear across more than one service. If single-service: "Not applicable — single-service ES."}

---

## 6. Re-Entry Signal

**Signal:** `maintain` / `watch` / `re-discover`

**Rationale:** {2–4 sentences explaining why this signal was chosen. If `re-discover`: include the specific discovery question this signal raises.}

{If re-discover:}
**Discovery question:** {The specific question a new Product Intelligence Kit engagement should investigate.}

---

## 7. Recommended Actions

| Action | Type | Rationale | Owner |
|--------|------|-----------|-------|
| {concrete action description} | Operational / Discovery / Engineering | {why this action is recommended, traceable to §4 or §5} | {name, role, or TBD} |
| {action} | {type} | {rationale} | {owner} |

*Minimum 1 action required. Owner may be TBD if not yet assigned.*

---

## 8. Freeze Declaration

This Evolution Signal has been generated from frozen input artifacts, reviewed for completeness and consistency with the input RHRs, and is ready for validation.

| Field | Value |
|-------|-------|
| Prepared By | {author} |
| Date | {YYYY-MM-DD} |
| Input Artifacts Confirmed Frozen | Yes |
