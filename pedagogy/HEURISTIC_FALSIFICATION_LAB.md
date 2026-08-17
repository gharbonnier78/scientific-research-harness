# Heuristic Falsification Lab

Status: **draft reusable pedagogical method**

## Purpose

Engineering "best practices", rules of thumb and quality heuristics are treated here as **priors to be challenged**, not truths to be transmitted and not myths to be disproved by default.

The purpose is to let a learner reconstruct an intuition from evidence:

```text
common heuristic / prior
        |
        v
why it sounds reasonable
        |
        v
minimal experiment
        |
        +--> supporting case
        +--> counterexample
        +--> ablation / perturbation
        |
        v
observed evidence
        |
        v
revised and scoped belief
```

A useful lab may confirm the heuristic, weaken it, reject it for a bounded regime, or show that its value depends on context.

## Core rule

**Teach experiments, not conclusions.**

A lab SHOULD record:

- the heuristic or prior;
- its engineering rationale and origin when known;
- the property it is intended to protect;
- the metric commonly used as a proxy;
- at least one supporting example;
- at least one plausible counterexample;
- an executable falsification, perturbation or ablation;
- the observed evidence;
- the scope of any revised belief;
- what remains unresolved.

Do not replace one dogma with its inverse.

## Candidate heuristic families

Examples include:

- "80%, 90% or 100% code coverage means the code is well tested";
- "more automated tests means better quality";
- "more tests means more confidence";
- "the test pyramid is always the right test architecture";
- "one unit test should test one function";
- "one test means one assertion";
- "all external dependencies should be mocked";
- "unit tests must never touch a database";
- "all tests must be deterministic";
- "a flaky test is necessarily useless";
- "every requirement needs at least one test";
- "100% requirement coverage means the requirements are adequately tested";
- "every defect should produce a regression test";
- "shift-left means moving as many tests as possible earlier";
- "automate everything that can be automated";
- "a PASS necessarily increases confidence".

The lab must not assume these statements are false. It must make their conditions of usefulness observable.

## Unit-test diversity: examples and non-examples

A unit test is not defined by being tiny or by using a particular framework. It is best understood as a controlled check of a **coherent local property or behaviour** with a sufficiently precise oracle.

### Examples that can genuinely be unit tests

| Example | Property challenged | Why it can be unit-level |
|---|---|---|
| `threshold_at_fmr()` rejects a threshold selected from test data | data-use policy / local precondition | one bounded behaviour with dependencies controlled |
| a Kalman covariance update remains symmetric | mathematical invariant | local deterministic transformation |
| a PID anti-windup rule clamps the integral state under saturation | state-transition invariant | bounded local state machine behaviour |
| parser rejects an invalid checksum | input-domain/error contract | local observable behaviour |
| serializer then parser preserves a supported value | round-trip/metamorphic property | local transformation pair, no external service required |
| feature-normalization output has unit norm except declared zero-vector behaviour | numerical invariant and edge condition | local algorithmic contract |
| pricing rule applies the exact boundary at a tier transition | business rule allocated locally | bounded decision rule |
| retry policy does not retry declared non-retryable errors | local error-handling policy | controlled dependency doubles can expose calls |
| state machine forbids transition from `CLOSED` to `RUNNING` | transition legality | local behavioural contract |
| analytic gradient agrees with finite difference within tolerance | independent numerical oracle | local algorithmic property with a second derivation |

A single coherent property may require several assertions. "One test = one assertion" is therefore not a requirement of this harness.

### Build checks that are useful but are not normally unit tests

| Build/CI check | Usually unit test? | What it actually establishes |
|---|---:|---|
| compiler/type checker succeeds | no | syntactic/type/build consistency |
| linter passes | no | conformance to selected static rules |
| formatting check passes | no | formatting reproducibility/convention |
| package imports/starts | usually no | smoke/build viability |
| dependency vulnerability scan | no | known-vulnerability evidence for declared dependency inventory |
| license-policy scan | no | dependency/legal policy conformance |
| SAST rule passes | no | absence of selected statically detectable patterns |
| schema files validate | usually no | artifact/contract structural validity |
| OpenAPI producer and consumer contracts agree | usually contract test | interface compatibility at a declared contract surface |
| database migration applies to a real DB | integration test | compatibility with a database engine/state |
| service A calls service B successfully | integration test | interaction across components |
| application boots with production-like configuration | smoke/system-ish | assembled viability under that configuration |
| deterministic paper/firmware/package rebuild matches artifact | reproducibility/build test | artifact reproducibility |
| end-to-end user journey succeeds | no, system/E2E | cross-component user-visible behaviour |

The distinction matters because a green build is a bundle of heterogeneous evidence, not a large unit test.

## Coherent property rule

A unit test SHOULD be explainable as a sentence about one coherent property or behaviour, for example:

- "below the declared threshold the decision is NO_MATCH";
- "invalid input is rejected without mutating persistent state";
- "the transformation preserves norm within tolerance";
- "the state machine cannot enter an illegal state";
- "the calculation is monotone over the declared domain".

A test that cannot state what property it is capable of falsifying should be challenged.

This does **not** mean the test must contain one input, one branch, one function call or one assertion. The unit of meaning is semantic, not syntactic.

## Beyond the nominal case: detection profile

A passing nominal example is weak evidence if the test is insensitive to plausible faults.

For a test `T` and a bounded fault class `F`, a useful conceptual quantity is:

```text
Detection power(T, F) = P(T fails | a fault from F is present)
```

For a deterministic test against one concrete implementation this is not a statistical estimate; it is simply pass/fail. A probability becomes meaningful when the experiment samples or enumerates a fault/input population, for example with:

- mutation operators;
- fault injection;
- boundary perturbations;
- parameter sweeps;
- property-based generation;
- metamorphic transformations;
- historical defect replay;
- adversarial or malformed inputs.

The complementary question also matters:

```text
Noise / false-alarm tendency = P(T fails | the relevant property is actually preserved)
```

The harness therefore prefers **detection profile** or **fault-detection power** over casually borrowing the word "sensitivity" without defining its population.

### Dimensions of a useful detection profile

A test or suite can be challenged along several independent dimensions:

1. **Oracle strength** — would the assertions notice a wrong result, or merely that code executed?
2. **Stimulus/domain reach** — which nominal, boundary, invalid and exceptional inputs are exercised?
3. **Fault-model relevance** — are the challenged faults plausible and important?
4. **Observability** — could the fault occur without reaching an asserted output/state?
5. **State-transition reach** — are only happy-path states observed?
6. **Robustness reach** — what happens just outside the nominal operating envelope?
7. **Independence of oracle** — is expected output derived independently, or by reproducing the implementation logic?
8. **Diagnostic locality** — does failure identify a bounded property rather than only "something broke"?
9. **Repeatability/noise** — can the evidence be trusted across replays?

No single scalar is required to summarize all these dimensions.

## Example lab: code coverage versus detection

Start with the prior:

> Increasing code coverage should increase confidence in defect detection.

Construct at least three suites over the same implementation:

- A: high statement/branch coverage with weak assertions;
- B: lower coverage but boundary, invariant and failure-oriented assertions;
- C: a suite selected or augmented using surviving mutants.

Measure both exposure and detection evidence:

```text
statement / branch coverage
mutation score by operator and fault family
historical defect detection, if available
execution cost
false failures / flakiness
```

Possible outcomes include:

- coverage and mutation detection rise together;
- coverage rises while detection plateaus;
- a lower-coverage suite detects more relevant faults;
- the conclusion reverses for another fault family.

The intended lesson is not "coverage is useless". It is that coverage measures exposure to executed structure and is not, by itself, a sufficient estimate of test adequacy.

## Example lab: number of automated tests versus value

Prior:

> More automated tests should reduce escaped defects.

Compare, for a fixed or reported budget:

- many shallow nominal tests;
- fewer risk-targeted tests;
- mutation-guided additions;
- property-based/boundary exploration.

Possible outcome variables include:

```text
fault detection probability by fault family
escaped relevant faults
time-to-detection
diagnosis cost
maintenance cost
runtime
```

The learner should be allowed to discover that the relationship can be monotone, flat, inverted or regime-dependent.

## Prior, evidence and revised belief

The lab SHOULD distinguish:

- **belief**: the current state of confidence in a proposition;
- **prior**: the belief state before the evidence being considered in this update;
- **posterior/revised belief**: the state after that evidence.

The posterior of one experiment may become the prior of the next. Priors must not be rewritten retrospectively to look prescient.

## Relation to scientific and engineering gates

A heuristic lab is pedagogical evidence unless a consuming project explicitly promotes an experiment into its scientific or engineering assurance process.

A CI gate should not be created merely because a heuristic is popular. Before promotion to a gate, state:

- which property or failure class the gate protects against;
- why that property matters;
- what evidence established the check;
- what a PASS permits and does not permit one to conclude;
- applicability and exceptions;
- whether failure means inadmissible evidence, a blocked build, or merely a warning.

## Relation to Diderot and consumer projects

The generic lab method belongs in `scientific-research-harness`. Concrete executable labs belong in the consuming teaching or research repository, for example Diderot or `Coverage Isn't Enough` work.

Consumer labs should preserve surprising or negative results. Their job is to update intuition, not to validate the author of the prior.
