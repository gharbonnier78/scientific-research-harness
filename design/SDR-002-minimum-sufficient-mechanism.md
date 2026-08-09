# SDR-002 — Prefer the minimum sufficient mechanism

Status: **ACCEPTED DESIGN DIRECTION**  
Date: 2026-08-09

## Context

Research and engineering problems that look simple at the interface may depend on a partially observable state, imperfect models, uncertain priors, changing operating regimes, human behavior, and several distinct failure modes.

This often justifies hybrid solutions. A recommender may combine collaborative evidence, content, context, popularity, and business constraints. A decision-support system may combine an estimator, prior knowledge, telemetry, simulation, and human review.

However, "hybrid" must not become permission to accumulate models, agents, rules, metrics, or tools without a demonstrated need. Each additional mechanism increases at least some of:

- conceptual and cognitive load;
- implementation and integration cost;
- validation and evidence burden;
- operational failure surface;
- observability and debugging needs;
- maintenance and change risk.

The harness therefore needs an explicit simplicity and interpretability principle that is strong enough to challenge unnecessary complexity without rewarding scientifically inadequate simplification.

## Decision

The Scientific Research Harness should prefer the **minimum sufficient mechanism set**:

> Combine only the mechanisms needed to cover distinct, decision-relevant information sources, uncertainties, constraints, operating regimes, or failure modes that are not adequately covered by a simpler candidate.

The preferred candidate is the one with the lowest **total justified complexity** among candidates that satisfy the frozen scientific, decision, safety, and operational requirements.

This is a defeasible preference, not an absolute rule. A more complex candidate is admissible when evidence shows that a simpler candidate is insufficient, or when the simpler candidate leaves an unacceptable and explicitly identified residual risk.

## What simplicity means

Simplicity is not merely:

- the fewest components;
- the shortest code;
- the most familiar algorithm;
- the smallest model;
- or the easiest story to tell.

The harness should consider complexity across at least these dimensions:

1. **model complexity** — assumptions, parameters, interactions and learning capacity;
2. **system complexity** — components, interfaces, dependencies and state transitions;
3. **evidence complexity** — claims, datasets, experiments and gates needed for assurance;
4. **operational complexity** — deployment, monitoring, recovery and change management;
5. **cognitive complexity** — the effort required to understand, challenge and safely use the system.

A modular hybrid may therefore be simpler in the relevant engineering sense than a single opaque component whose behavior, failure modes, and evidence requirements are harder to control.

## What interpretability means

"Make it interpretable" must be scoped to the decision rather than asserted globally.

A study should state:

- **who** needs an explanation;
- **which decision or action** the explanation supports;
- **which level** must be interpretable: data, mechanism, model output, interaction, uncertainty, or final decision;
- **what fidelity** is required;
- **how interpretability will be evaluated**;
- **what remains opaque**.

Intrinsic interpretability is preferred when it is sufficient. An opaque model is not automatically forbidden, but its additional decision value must be demonstrated and its opacity addressed through appropriate tests, uncertainty reporting, observability, challenge paths, and human controls.

Interpretability is not itself evidence that a model is correct.

## Required challenge: the removal test

For every mechanism beyond the simplest credible baseline, ask:

> If this mechanism were removed, which distinct information source, uncertainty, constraint, operating regime, or failure mode would no longer be adequately covered?

The answer must name both:

1. the uncovered need or risk; and
2. the evidence showing that the remaining mechanisms do not cover it sufficiently.

If neither can be identified, the mechanism is presumed unnecessary until justified.

This test applies to models, rules, agents, data sources, metrics, fallback paths, human review steps, and infrastructure components.

## Evidence expected

Where technically and ethically feasible, the justification should include one or more of:

- comparison with a frozen simple baseline;
- ablation or removal study;
- error-slice analysis showing a distinct failure mode;
- uncertainty or calibration analysis;
- out-of-domain or regime-shift evaluation;
- operational evidence such as latency, reliability, cost, or recovery behavior;
- documented constraint or hazard analysis;
- evidence that two mechanisms are complementary rather than redundant.

The study must not add a mechanism merely because it improves an aggregate metric without checking the decision-relevant trade-off and the extra assurance burden.

## Proposed declaration contract

Future schemas and templates should support a declaration equivalent to:

```yaml
minimum_sufficient_mechanism:
  decision_supported: "..."
  frozen_requirements:
    - "..."
  simplest_credible_baseline: "..."
  mechanisms:
    - id: "..."
      role: "..."
      distinct_coverage:
        information_source: "..."
        uncertainty: "..."
        constraint_or_regime: "..."
        failure_mode: "..."
      evidence_refs:
        - "..."
      removal_effect: "..."
  interpretability:
    audience: "..."
    decision_or_action: "..."
    required_level: "..."
    evaluation: "..."
    known_opacity: "..."
  simpler_candidate_rejected_because: "..."
  residual_uncertainty: "..."
```

Fields not applicable to a mechanism may be explicit `null` values with a reason. The contract should not encourage invented coverage claims merely to fill a template.

## Candidate gate behavior

The future harness should be able to flag or block a claim or design decision when:

- no simplest credible baseline is named;
- an added mechanism has no distinct, decision-relevant role;
- a claimed contribution has no evidence reference or declared evidence gap;
- a simpler candidate is rejected only by assertion;
- interpretability is claimed without audience, purpose, level, and evaluation;
- complexity is added after outcome inspection without an amendment or appropriate epistemic-status record;
- aggregate improvement hides unacceptable degradation on a frozen constraint or critical failure mode.

The gate should permit `INDETERMINATE` when the required comparison has not yet been run. Missing evidence must not be converted into either approval or automatic rejection.

## Relationship to other harness objects

- **Claim / hypothesis** — states what additional value or coverage a mechanism is expected to provide.
- **Preregistration** — freezes the baseline, comparison, estimand, trade-offs, and decision rule before outcome inspection.
- **Evidence** — tests whether the claimed distinct contribution exists.
- **Chronicle** — records a complexity concern only when it changes an allowed research action or gate.
- **SDR** — records why the mechanism set was chosen.
- **Pedagogy** — explains why an apparently simple need required, or did not require, a hybrid solution.
- **CAL / gate** — determines whether the evidence is sufficient for the bounded claim or decision.

## Consequences

- Every extra mechanism carries a burden of justification and assurance.
- Simple baselines become mandatory reference points, not ceremonial comparisons.
- Hybrid systems remain legitimate when their components cover demonstrably distinct needs.
- Interpretability becomes a testable decision requirement rather than a vague virtue.
- Ablations and failure-slice evidence become preferred tools for detecting redundancy.
- The principle favors the simplest defensible solution, not the simplest-looking one.

## Challenge questions

The rule itself must remain falsifiable and revisable:

1. Does the removal test overlook mechanisms whose value emerges only through interaction?
2. Can the evidence burden become so expensive that it prevents useful exploration?
3. When is redundancy deliberate resilience rather than unjustified duplication?
4. Which forms of opacity can be compensated by system-level observability and which cannot?
5. How should total justified complexity be compared without collapsing it into a misleading single score?

Repeated counterexamples should lead to a revised SDR or a more precise artifact class, not to silent exceptions.
