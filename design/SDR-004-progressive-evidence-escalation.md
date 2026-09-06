# SDR-004 — Progressive evidence escalation: screen before qualification

Status: **PROPOSED**

## Decision

Research work SHOULD separate **exploratory screening** from **claim-bearing qualification** when the scientific question allows it.

The evidence burden should grow with the consequence of the decision being supported. A cheap screening stage may be used to decide whether a hypothesis, route, dataset, model family or engineering direction deserves a full qualification campaign. A screening result MUST NOT be silently promoted into a qualified scientific claim.

This is an escalation rule, not a relaxation rule.

## Motivation

A recent Study 0 recovery and reanalysis in the Siamese embedding-compression programme exposed a practical asymmetry:

- the broad scientific direction could have been triaged much earlier from low-cost exploratory evidence;
- the later full chain — frozen protocol, exact provenance recovery, known-truth coverage validation, immutable replay, independent materialization review and independent interpretation review — was still necessary because a methodological defect had been found in an already foundational result;
- the expensive correction did not reverse the bounded conclusion, but it materially widened uncertainty and established that the earlier pair-level bootstrap had been too confident.

The lesson is not that rigorous qualification was wasted. The lesson is that **qualification should be triggered deliberately**, rather than being the default cost paid for every early hypothesis.

## Two evidence modes

### Mode A — exploratory screening

Purpose: decide whether further investment is justified.

Typical admissible simplifications include:

- fewer predeclared seeds;
- reduced compute budget;
- reduced hyperparameter search;
- development or screening data only;
- descriptive uncertainty sufficient for triage;
- lightweight diagnostics and ablations.

Required safeguards:

- mark outputs `EXPLORATORY` or equivalent;
- keep qualification TEST data unopened;
- do not make confirmatory, regulatory, production, or publication-grade claims;
- record the screening question and the promotion/stop rule before reading the screening outcome when practical;
- preserve enough provenance to explain why the branch was continued, redirected or stopped.

A negative screening result may legitimately terminate the branch and should be retained as a negative result, not discarded.

### Mode B — qualification

Purpose: support a claim, gate, publication statement or consequential decision.

Qualification inherits the full evidence burden appropriate to the claim, including as applicable:

- preregistration or frozen decision rule;
- authoritative source and dataset provenance;
- immutable inputs and replay artifacts;
- declared randomness and concurrency semantics;
- validated uncertainty procedure;
- complete artifact hashes;
- independent review;
- append-only Chronicle updates;
- explicit claim wording and inferential limits.

## Promotion rule

Promotion from screening to qualification SHOULD require an explicit reason, for example:

- a plausible effect at the decision-relevant operating point;
- a result near a consequential decision boundary;
- a surprising contradiction worth resolving;
- a candidate that survives matched controls or ablations;
- an engineering value signal large enough to justify the qualification cost;
- a requirement to publish, standardize, certify or make a consequential operational decision.

The promotion decision is itself part of the research record.

## Stop rule

If screening shows no credible signal and there is no independent reason to continue, the preferred action may be to stop, archive the negative result and redirect effort. The harness SHOULD NOT encourage expensive qualification merely to complete a ritual sequence.

## Escalation after a defect

If a defect is discovered in evidence that has already become foundational, the burden changes. Even if the original headline result appears obvious, a full correction may be required to determine whether the defect could have changed the decision.

The correction MUST preserve the original evidence and MUST distinguish:

- `result unchanged after correction` from `original analysis was valid`;
- `larger uncertainty` from `opposite effect`;
- `failure to demonstrate` from `proof of inferiority`;
- infrastructure/provenance failure from scientific failure.

## Relation to the harness lifecycle

Progressive evidence escalation fits the existing lifecycle:

- **Bind / Frame**: declare whether the work is screening or qualification.
- **Execute / Verify**: apply the minimum sufficient evidence burden for that mode.
- **Explain**: state what the result can and cannot support.
- **Chronicle & handoff**: record promotion, stop, redirection or escalation decisions.

A consumer MAY implement additional modes, but any transition toward stronger claims must be explicit and evidence-preserving.

## Anti-patterns

- Running a full qualification campaign before establishing that the question is worth the cost.
- Treating exploratory screening as publishable confirmation.
- Reusing qualification TEST data for repeated screening.
- Selecting only favorable screening seeds for qualification.
- Hiding a negative screening branch because it was not promoted.
- Discovering a methodological defect and assuming the headline result is unchanged without reanalysis.
- Mistaking more process for more evidence when the process cannot change the decision.

## Case-study origin

Originating case: `gharbonnier78/siamese-embedding-compression-lab`, Study 0 / Study 0 v0.2.2 correction, August 2026.

The reusable lesson is process-level only. The harness does not inherit the biometric domain conclusion.