# Conditional ADEMP Simulation Study Contract

**Status:** proposed reusable companion contract  
**Scope:** planning, execution and reporting of simulation studies that may affect a scientific or engineering decision.

## 1. Purpose

Simulation can range from a harmless pedagogical toy to decision-bearing methodological evidence.

This contract prevents the harness from treating all simulation equally. It requires an explicit ADEMP structure only when the simulation itself is being used to evaluate a method, justify a design, release/block a gate, or materially alter a scientific/engineering decision.

ADEMP is taken from Morris, White & Crowther (2019):

```text
A — Aims
D — Data-generating mechanisms
E — Estimands and other targets
M — Methods
P — Performance measures
```

The source note is `sources/morris-white-crowther-2019-simulation-studies.md`.

## 2. Applicability classification

Each consumer simulation SHOULD classify this contract as one of:

- `applicable`;
- `partially_applicable`;
- `not_applicable`.

The classification and rationale MUST be recoverable when the simulation may affect a claim, gate or decision.

### `applicable`

Use when simulation is intended to do one or more of the following:

- evaluate or compare statistical/inferential methods;
- estimate or validate coverage, Type I error, power, bias, variance or related operating characteristics;
- design a sample size or information budget when simple closed-form assumptions are insufficient;
- test finite-sample behavior of an approximation;
- justify an uncertainty method;
- support promotion/rejection of a scientific method or qualification procedure;
- materially influence a release, qualification or engineering decision.

### `partially_applicable`

Use when only part of ADEMP is relevant, for example an engineering Monte Carlo used for robustness/sensitivity where there is a clear generated world and performance measure but no statistical estimand in the conventional sense.

The consumer MUST state which ADEMP fields are adapted or inapplicable rather than inventing an artificial estimand merely to fill a template.

### `not_applicable`

Typical examples:

- purely pedagogical animation/toy with no claim-bearing conclusion;
- deterministic numerical visualization that is not a Monte Carlo/simulation experiment;
- synthetic fixture generation used only for software testing and not promoted to scientific evidence;
- exploratory simulation explicitly classified as non-decision-bearing screening.

A `not_applicable` classification MUST NOT later be silently promoted to qualification evidence. Promotion requires a new prospective contract.

## 3. Required ADEMP block when applicable

Before outcome-bearing or decision-bearing execution, the consumer MUST record:

### A — Aims

- exact question the simulation is intended to answer;
- scientific/engineering decision that may be changed by the answer;
- explicit non-goals and prohibited inferences.

### D — Data-generating mechanisms

- generated variables and dependence structure;
- parameter/scenario grid;
- sample/information structure;
- how truth is defined and known;
- why each scenario is relevant;
- important plausible regimes intentionally not represented;
- relationship, if any, between the synthetic generator and real data.

The consumer MUST NOT claim real-world validity solely because a method performs well under the generator.

### E — Estimands and other targets

- the exact quantity, hypothesis, decision target or other object evaluated;
- its units and operating point when relevant;
- any threshold, margin or boundary that changes interpretation.

If the simulation evaluates a decision procedure rather than a scalar estimand, that decision target MUST be stated directly.

### M — Methods

- methods/estimators/tests/decision procedures being evaluated;
- parameters and fixed semantics;
- thresholding/selection/tie/failure rules where material;
- what remains identical across method comparisons;
- any oracle/diagnostic method that is intentionally non-admissible for the real study.

A method MUST NOT be tuned after observing its simulation result and then represented as prospectively validated under the same experiment.

### P — Performance measures

- measures used to judge method behavior;
- prospective success/failure criteria where decision-bearing;
- Monte Carlo uncertainty or precision target;
- degeneracy/convergence/failure semantics;
- primary versus diagnostic measures.

Examples include bias, empirical variance, RMSE, confidence-interval coverage, interval width, Type I error, power, decision error, failure rate and runtime.

## 4. Monte Carlo experiment quality

When results depend on Monte Carlo repetition, the consumer SHOULD justify the repetition count through a precision target or other proportionate rationale.

Decision-bearing frequencies SHOULD report Monte Carlo uncertainty, such as MCSE or an interval appropriate to the performance measure.

Increasing simulation repetitions reduces Monte Carlo error; it does **not** create new real information or repair an inadequate data-generating mechanism.

## 5. Freeze and anti-method-shopping rule

If a simulation can release/block a scientific or engineering gate, the ADEMP block and its primary success criteria MUST be frozen before seeing the relevant results.

After a method fails:

- the negative result MUST remain recoverable;
- thresholds, truth cells, methods or performance measures MUST NOT be changed silently to obtain PASS;
- a new method may be evaluated only under a separately justified prospective question/contract;
- exploratory diagnostics MUST remain labeled as diagnostics unless prospectively promoted in a new experiment.

## 6. Relationship to real outcome evidence

Synthetic known-truth evidence can establish statements such as:

> Under the declared generator and scenario grid, this method has the following operating characteristics.

It cannot by itself establish:

> The real-world data-generating process matches this generator.

or:

> The real candidate/model/system satisfies the target claim.

When real outcomes are protected behind a gate, simulation must not be used as a covert substitute for opening or inferring those outcomes.

## 7. Relationship to the wider harness

ADEMP structures the simulation experiment but does not replace harness requirements for:

- source/provenance authority;
- local authorization boundaries;
- preregistration/freeze semantics;
- data and artifact integrity;
- random-state/concurrency/replay assurance;
- engineering assurance for code;
- independent review where warranted;
- append-only Chronicle decisions;
- separation of infrastructure success from scientific success;
- pedagogical explanation and human understanding.

## 8. Minimum reusable template

```yaml
simulation_study:
  ademp_applicability: applicable | partially_applicable | not_applicable
  rationale: ...

  aims:
    question: ...
    decision_supported: ...
    non_goals: [...]

  data_generating_mechanisms:
    generator: ...
    scenarios: ...
    known_truth: ...
    dependence_structure: ...
    omitted_regimes: [...]
    real_world_correspondence_limit: ...

  estimands_or_targets:
    primary: ...
    operating_point: ...
    thresholds_or_margins: ...

  methods:
    candidates: [...]
    fixed_semantics: ...
    failure_rules: ...
    diagnostics_not_admissible_for_primary_decision: [...]

  performance_measures:
    primary: [...]
    diagnostic: [...]
    success_criteria: ...
    monte_carlo_precision: ...
    degeneracy_rule: ...
```

## 9. Review questions

A reviewer should be able to answer:

1. Does the DGM actually stress the assumptions that matter for the aim?
2. Is the target/estimand the same object the downstream decision cares about?
3. Are compared methods isolated cleanly enough to interpret differences?
4. Were success criteria fixed before the result?
5. Is Monte Carlo uncertainty small enough for the decision?
6. Are negative and degenerate outcomes retained rather than filtered away?
7. Is the conclusion properly conditional on the generator?
8. Is any real-world or product-level claim being inferred beyond what synthetic evidence supports?

## 10. Source/adaptation boundary

The ADEMP structure and the view of simulation studies as empirical/computer experiments are grounded in Morris, White & Crowther (2019).

The applicability classification, freeze rules, authorization boundary, Chronicle integration and wider evidence-governance requirements are **harness adaptations**. They MUST NOT be attributed to Morris et al. as if they were requirements of that paper.
