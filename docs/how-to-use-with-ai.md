# How to Use This Kit with AI

This document explains how to use AI tools effectively with the Insight & Evolution Kit. Following these practices prevents the most common failure mode: AI that generates plausible-looking Evolution Signals with non-frozen inputs, fabricated trend data, or ungrounded re-entry signals.

---

## The Core Rule: Generate and Validate in Separate Sessions

Never run the ES generation prompt and ES validator in the same AI conversation session.

**Why:** An AI session that generated content will defend that content. It will interpret ambiguous validator criteria in ways that favor the artifact it produced. This is not a bug. It is how language models work. separation prevents self-validation bias.

**How:**
1. Session A: Run `es-prompt.md` with frozen inputs → get a draft ES
2. End or pause Session A
3. Session B: Run `es-validator.md` with the draft ES → get a structured JSON verdict
4. Review the JSON verdict yourself
5. Return to Session A (or open Session C): provide blocking issues and regenerate affected sections

This separation is a structural requirement, not a suggestion.


## Session Setup

### Generation Session

Before running the generation prompt, load:
1. `docs/prompts/es-prompt.md`. the generation prompt
2. `docs/specs/es-spec.md`. the spec the prompt references
3. `docs/artifacts/es-template.md`. the template the prompt follows
4. All frozen RHRs being analyzed (minimum 2, full content. Do not summarize)
5. Optionally: the frozen Value Hypothesis (if applicable)

Start the session with:

> I need to generate an Evolution Signal. I'm providing the generation prompt, the spec, the template, and the frozen input RHRs [and optional VH]. Please generate the ES following the prompt instructions exactly. Confirm frozen status of each input artifact before generating.

### Validation Session

Before running the validator, load:
1. `docs/validators/es-validator.md`. the validator
2. `docs/specs/es-spec.md`. the spec the validator references
3. The draft ES (full content)

Start the session with:

> I need to validate this Evolution Signal. Evaluate it strictly against the spec and produce the JSON output exactly as specified. Do not suggest how to fix issues. Only identify what is missing or insufficient per each hard gate.


## ES Session Pattern

### Full Flow

1. **Collect frozen inputs**. Confirm rhr status before starting
2. **Generation session**. Provide the prompt + spec + template + frozen rhrs (+ optional vh)
3. **Human review**. Check coverage summary accuracy, vh metric identifiers, re-entry signal consistency with trend data
4. **Validation session**. Provide validator + spec + draft es → get json verdict
5. **Fix blocking issues** if FAIL. Return to generation session with specific blocking issue descriptions
6. **Human approval**. Product owner reviews the re-entry signal and §7 recommended actions
7. **Freeze**. Update status to frozen; store validator json output alongside the es


## What to Do When the Validator Finds Blocking Issues

1. **Read the blocking issue description carefully.** It identifies what is missing or insufficient, referenced to a section or gate name.

2. **Do not ask the generation session to "fix" the validator output.** Instead, provide the blocking issues to the generation session as specific inputs:

   > The validator found the following blocking issues. Please address each one and regenerate the affected sections only. [paste blocking issues JSON]

3. **Do not modify the validator to pass an artifact that should fail.** If the validator is identifying a genuine gap, fix the gap.

4. **Common blocking issue resolutions:**

| Gate | Common Failure | Resolution |
|------|---------------|------------|
| `coverage_adequacy` | §2 table has < 2 rows | Ensure at least 2 RHRs are provided and appear as rows in §2 |
| `inputs_frozen` | Non-frozen RHR cited | Wait for RHR to be frozen; do not proceed with non-frozen inputs |
| `vh_assessment_explicit` | §3 is blank | Add the deferral phrase ("VH not provided; outcome assessment deferred.") or provide the VH and complete the assessment |
| `re_entry_signal_valid` | re-discover without discovery question | Add a specific discovery question to the §6 rationale |
| `actions_present` | §7 has actions missing type or owner | Complete all four fields for each action row; owner may be TBD |


## Managing Input Volume

Evolution Signals can have large inputs (2+ full RHRs, each potentially long). Tips for managing this:

**Summarize what you can, not what you must not:**
- RHR §4 Incident Summary: provide the full text, incident details matter for pattern analysis
- RHR §5 Layer 7 Feed: provide the full text: this is the primary input for the ES
- RHR §2 SLO Compliance: provide the full table, exact figures are needed for trend analysis
- RHR §1 Document Control: provide the full section, frozen status and SRP references are checked

**What you can omit:**
- RHR §3 Error Budget State (provide if available; the ES will use it if present)
- Extended monitoring data or observability appendices not referenced in the RHR itself

**Do not summarize:**
- SLO compliance percentages: the trend analysis requires exact figures
- Incident records, pattern analysis requires root cause details
- VH success metrics, outcome assessment requires exact SM-N identifiers from the original VH


## What to Do When AI Struggles

### AI invents trend data
The AI may extrapolate or invent trend data if RHR content is incomplete. Prevention: provide the full RHR content, not summaries. Detection: cross-check every trend percentage in the ES against the RHR SLO compliance tables. Correction: in the generation session, point to the specific RHR sections and ask for re-synthesis.

### AI picks `re-discover` when `watch` is appropriate (or vice versa)
The prompt includes selection criteria for each signal, but judgment is involved. If the AI's signal selection doesn't match your assessment, provide feedback in the generation session:

> The signal in §6 is `re-discover`, but the data shows only a 1-period miss with no persisting pattern. Please reconsider and re-render §6 with a `watch` signal and appropriate rationale.

### AI cannot find VH metrics in the RHR data
This is correct behavior. The prompt marks vh metrics as "insufficient data" when production data is not available. review whether the rhrs actually contain the data needed (e.g., a latency vh metric requires latency slo data in the rhrs). if the data genuinely is not in the rhrs, insufficient data is the correct verdict.


## When Not to Use AI

**Use a human directly for:**
- The re-entry signal decision if data is ambiguous and judgment is required
- The decision to act on a `re-discover` recommendation (the product owner decides)
- Authorizing the freeze (the product owner or team lead reviews and approves)

**AI helps with:**
- Synthesizing trend direction across multiple RHR periods
- Mapping VH success metrics to observed production data
- Identifying cross-service patterns in multi-service ESs
- Structuring recommended actions from observations
