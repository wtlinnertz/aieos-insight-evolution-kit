# CLAUDE.md — Insight & Evolution Kit

## What This Repository Is

This is the **Insight & Evolution Kit** — an AIEOS kit that synthesizes operational signals from production into actionable insights and feeds them back to the Product Intelligence layer. It is Layer 7 of the AIEOS system. It receives frozen Reliability Health Reports from the Reliability & Resilience Kit and produces Evolution Signals that close the feedback loop.

## Repository Structure

```
docs/
  principles/          # Organizational insight and evolution policy (input material)
  specs/               # Content rules and hard gates per artifact type
  artifacts/           # Templates
  prompts/             # AI generation prompt
  validators/          # Quality gate definition
  playbook.md          # End-to-end process definition
  index.md             # Documentation entry point
  how-to-adapt.md      # Organizational adoption guidance
  how-to-use-with-ai.md # AI tool usage guide
  governance-model.md  # AIEOS structural rules (reference)
examples/              # Worked example (TaskFlow basic evolution)
tests/                 # Structural integrity checks
```

## Artifact Types

This kit produces one governed artifact type:

1. **Evolution Signal (ES)** — Synthesizes reliability health data, assesses value hypothesis outcomes, and produces a re-entry signal for the Product Intelligence layer. Closes the Layer 6 → Layer 7 → Layer 2 feedback loop.

The ES has exactly four governing files: spec, template, prompt, validator.

No entry gate. The ES confirms its own inputs are frozen in §1 Document Control.

## Utility Prompts

None. The ES prompt handles all generation logic including input validation.

## Key Rules

- **Specs are the source of truth** — prompts and validators reference specs, never inline rules
- **Validators judge, they do not help** — no suggestions, no redesign
- **Freeze before promote** — all cited RHRs and SRPs must be Frozen before ES generation begins
- **Separate generation and validation** — different AI sessions to prevent self-validation bias
- **No scope expansion** — the ES must not produce recommendations beyond what the input RHRs support
- **No inferred information** — mark missing information explicitly, do not fill gaps
- **VH assessment is mandatory** — §3 must be explicitly addressed or explicitly deferred; blank is not accepted
- **Re-entry signal is advisory** — `re-discover` produces a PIK intake recommendation, not automatic re-entry
- **Governance model sync** — `docs/governance-model.md` is a synchronized copy of `aieos-spec/governance-model.md` (canonical authority). Do not edit kit copy directly; update `aieos-spec` first, then sync all kit copies to match exactly. See governance-model.md §15 for versioning and change protocol.

## Artifact Flow

```
Input: 2+ frozen RHRs (from Reliability & Resilience Kit, Layer 6)
Optional input: frozen Value Hypothesis (from Product Intelligence Kit, Layer 2)

Evolution Signal → validate → freeze
  → if re-discover: produces PIK intake recommendation → human product owner decides
  → if maintain or watch: no cross-kit action; monitor per signal
```

## Boundary Contracts

- **Upstream:** Receives frozen Reliability Health Reports from the Reliability & Resilience Kit (Layer 6). Minimum 2 frozen RHRs required. The RHR §5 Layer 7 Feed section is the primary upstream input. Additional optional input: frozen Value Hypothesis from PIK (Layer 2).
- **Downstream:** Produces a frozen Evolution Signal. If re-entry signal is `re-discover`, the ES §6 rationale and §7 recommended actions inform a Product Intelligence Kit Discovery Intake Form. The product owner decides whether to initiate a new discovery engagement.

## ES ID Format

`ES-{SCOPE}-{NNN}` where scope is the service name or `PORTFOLIO` for multi-service signals.

Examples: `ES-NOTIFICATION-SVC-001`, `ES-PORTFOLIO-001`

## File Naming

| Type | Pattern |
|------|---------|
| Spec | `{type}-spec.md` |
| Template | `{type}-template.md` |
| Prompt | `{type}-prompt.md` |
| Validator | `{type}-validator.md` |

## When Working on This Kit

- Read the playbook (`docs/playbook.md`) for the full process definition
- Read the governance model (`docs/governance-model.md`) for structural rules
- Check `docs/how-to-use-with-ai.md` for session setup instructions
- Reference `examples/basic-evolution/` for a complete worked example

## Building or Auditing AIEOS Kits

- `aieos-spec/docs/kit-structure-standard.md` — compliance checklist for building and auditing kits
- `aieos-spec/docs/philosophy.md` — design rationale for governance model decisions
- `aieos-spec/docs/layer-model.md` — seven-layer model and kit registry
