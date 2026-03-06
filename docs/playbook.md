# Insight & Evolution Kit Playbook

This playbook defines the end-to-end process for producing a frozen Evolution Signal from frozen Reliability Health Reports. It is the non-negotiable process definition for this kit.

---

## Overview

The Insight & Evolution Kit closes the AIEOS feedback loop. It takes operational signals from production (frozen RHRs) and produces an Evolution Signal that determines whether a system should continue as-is, receive elevated attention, or trigger new discovery in the Product Intelligence layer.

**The only artifact this kit produces:** Evolution Signal (ES)

**Minimum inputs:** 2 frozen Reliability Health Reports (from the Reliability & Resilience Kit)

**Optional input:** A frozen Value Hypothesis (from the Product Intelligence Kit)

---

## Artifact Flow

```
Input: 2+ frozen RHRs (Layer 6)
Optional: frozen VH (Layer 2)
        ↓
Step 1: Gather inputs → confirm all are Frozen
        ↓
Step 2: Evolution Signal (ES) → ES Validator → freeze
        ↓
Step 3: Re-entry determination
  → maintain: no cross-kit action; monitor per §6 rationale
  → watch: no cross-kit action; elevate monitoring per §7 actions
  → re-discover: prepare PIK intake recommendation (§7) → human product owner decides
```

---

## Step 1: Gather and Confirm Inputs

Before starting the ES generation session:

1. **Collect frozen RHRs.** Locate the frozen RHRs for the service(s) being analyzed. Confirm each is in Frozen status. You need at least 2.

2. **Collect frozen SRPs.** The SRPs referenced by the RHRs should be available (they are cited in RHR §1 Document Control). The ES prompt does not require SRPs directly, but the validator may cross-reference them.

3. **Optionally collect the frozen VH.** If the original discovery engagement produced a Value Hypothesis that has not yet been assessed in a prior ES, include it. The ES §3 VH Outcome Assessment maps VH success metrics to observed production data.

4. **Confirm frozen status.** Every artifact cited in the ES §1 must be Frozen. Non-frozen artifacts are not valid inputs. If an RHR is not yet frozen, wait.

**When to skip a VH:** If no VH was produced (initiative entered EEK directly via Path B), if the VH was already assessed in a prior ES, or if the VH does not apply to the current services, omit it. §3 must then state "VH not provided; outcome assessment deferred." This is not a failure — it is an explicit acknowledgment.

---

## Step 2: Generate and Validate the ES

### Generation Session

1. Load the generation prompt: `docs/prompts/es-prompt.md`
2. Load the spec: `docs/specs/es-spec.md`
3. Load the template: `docs/artifacts/es-template.md`
4. Load all input artifacts: the frozen RHRs and optional VH
5. Begin generation per the prompt instructions

The prompt will:
- Confirm input frozen status before generating
- Build the coverage summary from RHR §1 and §2 data
- Map VH metrics to observed data (or explicitly defer if no VH)
- Synthesize trend analysis from SLO compliance data across periods
- Identify persisting, resolved, new, and cross-service patterns
- Select exactly one re-entry signal with rationale
- Produce recommended actions traceable to findings

**Do not start the generation session without frozen inputs.** The prompt is designed to stop if inputs are non-frozen — do not work around this.

### Human Review Before Validation

After generation, the product owner or designated reviewer should check:
- The coverage summary accurately reflects the provided RHRs
- The VH outcome assessment uses the correct SM-N identifiers
- The re-entry signal rationale is consistent with the trend data
- The recommended actions are concrete and traceable

### Validation Session

Start a new session. Load:
1. `docs/validators/es-validator.md`
2. `docs/specs/es-spec.md`
3. The draft ES

Ask the validator to evaluate all five hard gates and produce JSON output. A FAIL requires returning to the generation session.

### Hard Gates (5)

| Gate | What It Checks |
|------|---------------|
| `coverage_adequacy` | ≥2 frozen RHRs cited; §2 has ≥2 rows; coverage period stated; gaps noted |
| `inputs_frozen` | Every cited artifact is Frozen |
| `vh_assessment_explicit` | §3 complete OR explicit deferral phrase — blank not accepted |
| `re_entry_signal_valid` | Exactly one of maintain/watch/re-discover; 2–4 sentence rationale; discovery question if re-discover |
| `actions_present` | ≥1 action in §7 with type, rationale, and owner |

### Freeze

Once all 5 hard gates PASS and a human has reviewed:
1. Update the ES Status field to `Frozen`
2. Store the validator JSON output alongside the ES (e.g., `es-{id}-validation.json`)
3. Proceed to Step 3

---

## Step 3: Re-Entry Determination

The frozen ES produces one of three re-entry signals:

### `maintain`

No cross-kit action. The system is operating within expectations.

Retain the frozen ES for the service's reliability record. Schedule the next ES when the next 2 RHRs are available.

### `watch`

No immediate cross-kit action. Elevate operational monitoring.

Review the §7 recommended actions and assign owners to any Operational or Engineering actions that have not yet been addressed. Set a timeline for checking whether the concerning signals have resolved.

### `re-discover`

A new product discovery engagement is recommended. This is advisory — the product owner decides whether to act.

**If the product owner chooses to act:**
1. The ES §6 discovery question becomes the starting question for a new PIK engagement.
2. The ES §7 Discovery-type actions inform the PIK Discovery Intake Form.
3. The ES itself becomes a reference artifact for the new discovery engagement (attach it to the intake as context).
4. Begin a new PIK engagement under a new artifact series (new PFD, VH, AR, EL, DPRD IDs).

**If the product owner chooses not to act:**
Record the decision and rationale (e.g., in a brief note attached to the ES). The ES remains Frozen. The system continues under the current SRP.

---

## Re-Entry Protocol

When a frozen ES must change (rare — this is a terminal synthesis artifact):

1. Run impact analysis: what downstream work (PIK engagement, operational actions) has been initiated based on this ES?
2. Assess whether the change is material (affects the re-entry signal or §7 actions) or non-material (typos, formatting corrections).
3. If non-material: apply as an Amendment Log entry per governance model §6. No re-validation required.
4. If material: re-validate the entire ES. If a PIK engagement was initiated based on the original re-entry signal, assess whether it should continue, pause, or be restarted.

---

## Deprecation and Sunset

When a service is decommissioned, a frozen ES for that service transitions to `Deprecated`. If a discovery engagement was initiated based on an ES's `re-discover` signal and that initiative is subsequently cancelled, the ES itself may remain Frozen — the ES is evidence of the insight, not the initiative.

See `aieos-spec/docs/deprecation-protocol.md` for the full Deprecation Notice process.

---

## Maintaining the Engagement Record

The Engagement Record (ER) is a project-level artifact that lives in the consuming project at `docs/engagement/er-{initiative}.md`. It spans all AIEOS layers and is maintained by each kit's operators as work passes through. The ER spec and format are defined in `aieos-spec/docs/engagement-record-spec.md`.

**IEK maintains the Layer 7 section of the ER and sets the final Initiative Outcome.**

### What to Update When an ES is Frozen

| Trigger | ER update |
|---------|-----------|
| ES frozen | Add ES ID to §6 table with coverage period, re-entry signal, and VH verdict |
| PES produced that includes this initiative | Note the PES ID in §6 |

After adding the ES to §6, update §7 Initiative Outcome:
- Set `Final Re-Entry Signal` to the ES §6 signal
- Set `Final VH Verdict` to the ES §3 overall verdict
- If signal is `re-discover`, add a note in §7 with the discovery question from ES §6

### On Service End

When issuing a Deprecation Notice: update §1 Status to `Deprecated` and add the DN ID to §7 Initiative Outcome. If no ES has been produced before decommission, note "No ES produced" in §7 Final Re-Entry Signal.

---

## Principle File Revision

When the principle file in `docs/principles/` changes, use the change categories defined in `aieos-spec/docs/principle-file-standard.md`:

| Change Category | Version Bump | Re-Entry Impact |
|----------------|-------------|-----------------|
| **Minor** (clarification only) | `v_.x → v_.x+1` | No re-entry required; already-frozen artifacts remain valid |
| **Significant** (new requirement or tightened constraint) | `v1.x → v1.x+1` | Review artifacts generated after the change against updated principles; already-frozen artifacts are grandfathered |
| **Breaking** (removal or loosening) | `vN.x → vN+1.0` | Requires service owner authorization and documented business justification; re-entry may be warranted |

Every change to the principle file must bump the version field, even minor clarifications.

---

## Session Discipline

- **One artifact per generation session** — the ES is the only artifact in this kit; generate it in a single focused session
- **Separate generation and validation sessions** — prevents self-validation bias
- **Include full frozen upstream artifacts** — do not summarize or paraphrase RHR content
- **Include the spec, prompt, and template** for the generation session
- **Include the spec and validator** for the validation session
