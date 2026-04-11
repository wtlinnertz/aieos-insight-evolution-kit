# Session Setup — Insight & Evolution Kit

Use this file to set up an AI session for each IEK artifact. Find the section for the artifact you are generating or validating. Follow the checklist before starting.

**Rule:** Generate and validate in separate sessions. Do not self-validate.

**IEK trigger:** IEK is entered when ≥2 frozen RHRs are available from RRK. There is no formal entry gate artifact — the ES prompt's Step 1 confirms frozen input status.

---

## Evolution Signal (ES-{SCOPE}-{NNN})

**What you're creating:** A synthesis of operational signals from ≥2 RHR periods — assessing reliability trends, VH outcomes (if a VH was provided), and recommending a re-entry signal (maintain / watch / re-discover) with specific actions.

**Required Inputs (confirm before starting):**
- [ ] At least 2 frozen RHRs — Frozen? (minimum; more provides better signal quality)
- [ ] Optional: Frozen VH (VH-{PROJECT}-{NNN}) from PIK — Available? (enables §3 outcome assessment)
- [ ] Optional: Frozen Engagement Record (ER) — Available? (context supplement)

**Pre-Flight Gate Check (verify before generating):**
- [ ] `coverage_adequacy`: ≥2 frozen RHRs confirmed, each covering a distinct review period
- [ ] `inputs_frozen`: All RHRs are in Frozen status (not In Progress or Validated)
- [ ] `vh_assessment_explicit`: If VH is available, §3 assessment will be written explicitly; if not available, §3 will state "VH not provided — outcome assessment not possible"
- [ ] `re_entry_signal_valid`: Will use exactly one signal value: maintain / watch / re-discover
- [ ] `actions_present`: At least one recommended action per signal type ready to articulate

**Session Setup:**
1. Load: `docs/prompts/es-prompt.md`
2. Provide: All frozen RHR content (by ID, in chronological order)
3. Provide: Frozen VH if available — note explicitly if not
4. Provide: `docs/specs/es-spec.md` (or confirm it is in context)
5. Validate in a separate session: `docs/validators/es-validator.md`

**Re-entry signal guidance:**
- `maintain` — Service is stable; continue current operational practices; no discovery trigger
- `watch` — Trends warrant closer monitoring; define specific watch conditions and duration
- `re-discover` — Patterns indicate the value hypothesis or problem framing needs revisiting; §6 must contain a specific discovery question

**Common Failure to Avoid:**
§3 VH assessment left blank when a VH was provided — even an inconclusive outcome requires explicit written assessment. Blank §3 fails the `vh_assessment_explicit` gate.

---

## Portfolio Evolution Signal (PES-{NNN})

**What you're creating:** A cross-initiative synthesis of patterns and insights from ≥2 Engagement Records — identifying systemic trends and generating improvement proposals for governing artifacts (specs, prompts, or templates).

**Required Inputs (confirm before starting):**
- [ ] At least 2 frozen Engagement Records (ERs) — Frozen? (in `docs/engagement/` of each initiative project)
- [ ] Optional: Individual frozen Evolution Signals (ESes) — Available? (supplements ER data)

**Pre-Flight Gate Check (verify before generating):**
- [ ] `coverage_adequacy`: ≥2 frozen ERs confirmed, covering distinct initiatives
- [ ] `inputs_frozen`: All ERs are in Frozen status
- [ ] `patterns_evidence_based`: Every pattern finding will cite specific artifact IDs and counts (not generalizations)
- [ ] `synthesis_present`: Ready to synthesize what patterns mean collectively for the portfolio
- [ ] `proposals_complete`: At least one specific improvement proposal ready, with target artifact identified

**Session Setup:**
1. Load: `docs/prompts/pes-prompt.md`
2. Provide: All frozen ER content (by ID, in chronological order by initiative)
3. Provide: Any frozen ESes available as supplemental context
4. Provide: `docs/specs/pes-spec.md` (or confirm it is in context)
5. Validate in a separate session: `docs/validators/pes-validator.md`

**PES improvement proposals:** Proposals should identify the specific governing artifact to improve (e.g., `assumption-register-prompt.md`, `experiment-log-spec.md`) and describe the change in enough detail for a human to act on it. Vague proposals ("improve the AR process") fail the `proposals_complete` gate.

**Common Failure to Avoid:**
Pattern findings stated vaguely without artifact citations — every pattern claim must be backed by specific artifact IDs and quantified counts (e.g., "AR gate failure: source_traceability — 3 of 4 ERs show this failure: AR-TF-001, AR-COMMS-001, AR-PAY-001").
