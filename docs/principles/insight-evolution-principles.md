# Insight & Evolution Principles

Version: v1.0

These principles define how this organization collects, synthesizes, and acts on operational signals from production systems. They govern what counts as a meaningful pattern, how feedback loops are closed, and what triggers a return to discovery.

These are **organizational policy documents** — input material for ES generation, not governed artifacts. They answer: "What standards does this organization hold?"

---

## 1. Pattern Recognition Standards

### Signals must be grounded in data

An insight claim is only as credible as the data behind it. Observations about system behavior must trace to concrete evidence: SLO compliance percentages, incident records, error budget figures. Qualitative impressions ("the system feels faster") are not signals. Quantitative observations with measurement context ("availability improved from 99.1% to 99.7% over 60 days") are signals.

### Two periods are the minimum for trend detection

A single RHR period provides a baseline, not a trend. Trend analysis requires at least two comparable periods. The Evolution Signal requires a minimum of two frozen RHRs specifically because one data point cannot establish direction.

### Patterns require recurrence or persistence

A single anomalous event is not a pattern. A root cause class that appears in two or more incidents is a pattern. An SLO that has missed its target in two or more consecutive periods is a pattern. When in doubt about whether something is a pattern or a one-time event, treat it as a watch item, not a confirmed pattern.

### Absence of evidence is not evidence of absence

If the available data does not address a question (e.g., whether a VH metric was achieved), this must be stated explicitly as "Insufficient Data." Omitting a metric from the assessment implies it was checked and found satisfactory. Omission is not silence — it is a misleading signal.

### Cross-service patterns require careful scope management

When analyzing multiple services, clearly distinguish between per-service patterns and cross-service patterns. A pattern observed in one service is not automatically a cross-service pattern even if other services share the same infrastructure. Cross-service claims require evidence from each service's own RHR data.

---

## 2. Feedback Loop Governance

### The feedback loop is advisory, not automatic

The Evolution Signal produces a re-entry recommendation, not a mandate. A `re-discover` signal means: "based on available data, a new discovery engagement appears warranted." The product owner decides whether to act. The ES does not trigger automatic work. It provides information for human decision-making.

### Re-entry thresholds must be explicit

A `re-discover` signal must come with a discovery question — the specific aspect of the system's behavior or value delivery that warrants new discovery. "Things seem off" is not a discovery question. "Average delivery latency has been above the VH target by 15% for two consecutive periods, and we don't know if this is a technical constraint or a need to recalibrate the expectation" is a discovery question.

### `watch` is not a substitute for `re-discover`

The `watch` signal means monitoring is warranted; engineering attention may be appropriate. It does not mean "re-discover later." If a signal clearly warrants discovery, `re-discover` is the correct choice. Using `watch` to avoid recommending `re-discover` is a signal quality failure.

### `maintain` does not mean "stop paying attention"

A `maintain` signal means the current operational posture is appropriate. It does not mean the system is perfect or that no improvements are possible. It means the available evidence does not point to a need for discovery or elevated operational concern. Operational hygiene continues regardless.

### Escalation paths are separate from re-entry

An IR or RHR that triggers an AIEOS escalation (via the escalation assessment utility prompt) follows the escalation path, not the evolution path. Escalation involves human intervention in the immediate operational posture. Evolution Signals look across periods to identify trends. They are complementary, not competing.

---

## 3. Evolution Signal Scope

### The ES observes; it does not prescribe

An Evolution Signal identifies what happened and what it implies. It does not redesign the system, rewrite the VH, or prescribe the answer to the discovery question it raises. It creates the input for the next layer of decision-making.

### VH outcome assessment is forward-looking, not backward blame

When a value hypothesis is assessed as Invalidated or Partially Validated, this is a learning outcome — not a failure of the original product bet. VH outcomes inform future discovery. The ES must not frame a VH assessment as a retrospective criticism of the decision that launched the initiative.

### Actions must be traceable to observations

Every recommended action in the ES must trace to a specific finding in the trend analysis (§4) or pattern analysis (§5). An action that cannot be traced to a finding is speculation, not recommendation. Delete it or add the supporting finding first.

### The ES covers the period of its inputs

The Evolution Signal covers the time period spanned by its input RHRs. It may not extrapolate beyond that period, make predictions about future performance, or reference events that happened after the most recent RHR's coverage period ended.

---

## Enforcement Mapping

| Principle | Enforced In |
|-----------|------------|
| Signals grounded in data | ES spec §3 quality criteria; ES validator §5 warnings |
| Two periods minimum | ES spec hard gate 1 (`coverage_adequacy`) |
| Absence = Insufficient Data (not omission) | ES spec §3 section rules; ES prompt Step 4 |
| Re-entry is advisory | CLAUDE.md key rules; ES prompt behavioral rules |
| Re-discover requires discovery question | ES spec hard gate 4 (`re_entry_signal_valid`) |
| VH assessment explicit or explicitly deferred | ES spec hard gate 3 (`vh_assessment_explicit`) |
| Actions traceable to findings | ES spec §5 quality criteria; ES validator completeness scoring |
