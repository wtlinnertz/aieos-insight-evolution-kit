# Entry from Reliability & Resilience Kit (RRK)

**You are here because:** At least 2 Reliability Health Reports (RHRs) are frozen, covering sufficient operational history to synthesize patterns.

**What you must bring:**

| Artifact | Status Required | Where to Find It |
|----------|----------------|-----------------|
| Reliability Health Report #1 (RHR-{SERVICE}-{NNN}) | Frozen | RRK `docs/artifacts/` in the consuming project |
| Reliability Health Report #2 (RHR-{SERVICE}-{NNN}) | Frozen | RRK `docs/artifacts/` — must cover a distinct review period from #1 |
| Value Hypothesis (VH-{PROJECT}-{NNN}) | Frozen (optional) | PIK `docs/artifacts/` — enables outcome assessment in ES §3 |
| Engagement Record (ER-{INITIATIVE}-{NNN}) | Frozen (optional) | `docs/engagement/` in the consuming project |

**First artifact to generate in this kit:** Evolution Signal (ES-{SCOPE}-{NNN})

**Where to start:** `docs/session-setup.md` → "Evolution Signal (ES)"

**What changes at this boundary:**

- You shift from operational measurement to learning synthesis. The question changes from "is the service healthy?" to "what did we learn and what should change?"
- The VH assessment is a new responsibility: IEK asks whether the value hypothesis that drove the initiative was correct. This requires consulting the original PIK artifacts.
- The re-entry signal (`maintain` / `watch` / `re-discover`) is advisory — it informs, not commands. A human product owner decides whether to act on a `re-discover` signal.

**Common mistakes at this transition:**

| Mistake | How to Avoid |
|---------|--------------|
| Generating ES with only 1 frozen RHR | The minimum is 2 frozen RHRs. Wait for the second RHR before generating the ES. |
| Leaving §3 VH assessment blank when a VH exists | Even if the VH outcome is inconclusive, the assessment must be written explicitly. Blank is not a valid response. |
| Treating `re-discover` as automatic PIK re-entry | The ES re-entry signal does not automatically trigger a new PIK engagement. It informs the product owner, who decides. |

**If you arrived here without a complete upstream artifact:**

Stop. IEK requires at least 2 frozen RHRs to have sufficient signal for pattern analysis. Continue operating in RRK until the second RHR is frozen, then return to IEK. A single RHR does not provide the cross-period pattern visibility that ES synthesis requires.

---

*For the full entry flow, see `docs/playbook.md`.*
