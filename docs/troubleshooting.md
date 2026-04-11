# Troubleshooting Guide: Insight Evolution Kit

## How to Use This Guide

When a validator returns FAIL, find the failing gate in the table below. The Remediation column describes the specific fix required. Reopen the artifact, apply the remediation, and rerun the validator in a new session.

Validators and generation are separate sessions. Don't embed fix attempts in your validation session.

---

## Evolution Signal (ES-{SCOPE}-{NNN})

| Gate | What Failure Looks Like | Typical Cause | Remediation |
|------|------------------------|---------------|-------------|
| coverage_adequacy | Only one RHR provided | Minimum input threshold not met | Provide at least 2 frozen RHRs before generating the ES; re-enter the IEK flow when the second RHR becomes available |
| inputs_frozen | RHRs referenced but not confirmed as frozen | Inputs assumed frozen without verification | Confirm each referenced RHR is in Frozen status; do not proceed with RHRs that are still in-progress or draft |
| vh_assessment_explicit | §3 VH assessment blank when a VH was provided as input | VH outcome assessment skipped during generation | Complete §3 with an explicit assessment of the VH outcome; if no VH was provided, state "VH not provided: outcome assessment not possible" |
| re_entry_signal_valid | Re-entry signal not from the enumerated values | Free-text signal used | Use exactly one of: maintain / watch / re-discover |
| actions_present | §7 Recommended Actions section empty | Actions omitted | Include at least one recommended action per re-entry signal; for a re-discover signal, include a specific discovery question that names the unknown to resolve |

---

## Portfolio Evolution Signal (PES-{NNN})

| Gate | What Failure Looks Like | Typical Cause | Remediation |
|------|------------------------|---------------|-------------|
| coverage_adequacy | Fewer than 2 Engagement Records provided | Insufficient portfolio breadth for synthesis | PES requires at least 2 frozen Engagement Records; generate individual ESes for each initiative first if needed |
| inputs_frozen | ERs referenced but not confirmed as frozen | Inputs assumed frozen without verification | Confirm each referenced ER is in Frozen status before generating the PES |
| patterns_evidence_based | Patterns stated without artifact citations | Generalizations drawn from memory or impression | Every pattern finding must cite specific artifact IDs and include quantified occurrence counts across those artifacts |
| synthesis_present | §4 Synthesis absent or only briefly addressed | Synthesis section omitted | Document cross-initiative synthesis: what the identified patterns mean collectively for the portfolio, not just individually |
| proposals_complete | §5 Improvement Proposals incomplete or absent | Proposals deferred or omitted | Include at least one specific improvement proposal with a named target artifact (spec, prompt, or template) and a concrete description of the proposed change |
