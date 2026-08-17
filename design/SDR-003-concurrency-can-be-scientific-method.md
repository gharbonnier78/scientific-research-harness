# SDR-003 — Concurrency can become part of the scientific method

Status: **ACCEPTED DESIGN DIRECTION**
Date: 2026-08-09

## Context

Concurrency is often presented as an implementation detail: split independent work,
run it faster, then combine the results. That separation fails when the execution
strategy can change assumptions required by the estimand or its uncertainty procedure.

Examples include concurrency that changes or obscures:

- the identity of Monte Carlo or resampling units;
- random-stream construction and independence assumptions;
- the mapping between logical datasets and random streams;
- aggregation order, floating-point reduction or tie handling;
- stopping rules and checkpoint selection;
- failure, retry, redraw or partial-result semantics;
- the ability to replay one unit independently;
- the result obtained when the worker count or scheduler changes.

In those cases, concurrency is not merely a performance mechanism. It is part of the
scientific instrument used to produce the evidence.

## Decision

The Scientific Research Harness should require a **concurrency-assurance contract**
before a concurrent execution path may produce outcome-bearing scientific evidence
whenever concurrency touches a scientific assumption.

The contract must identify:

1. the logical unit of parallel work;
2. the scientific assumptions that concurrency could affect;
3. task identities that do not depend on worker assignment or completion order;
4. the random-stream construction, when randomness is involved;
5. aggregation, checkpoint and stopping semantics;
6. failure, retry, redraw and partial-evidence behavior;
7. the equivalence relation required against a reviewed reference execution;
8. the replay metadata and evidence that demonstrate the relation;
9. the measured environment, cost and speedup, kept separate from scientific outcomes.

The harness must not prescribe one universal RNG or concurrency library. A hierarchical
`SeedSequence.spawn()` design is one defensible construction for NumPy simulations, but
other constructions may be admissible when their guarantees and replay properties are
explicitly justified.

## Required behavioral consequence

If the contract or its required evidence is missing, a named production step that would
emit outcome-bearing evidence must remain blocked or `INDETERMINATE`.

The blocker may be released only by a later, traceable decision referencing the required
evidence. A benchmark can establish feasibility; it cannot establish statistical validity.
An equivalence test can establish the bounded execution property it tests; it cannot by
itself validate the estimator's coverage or the substantive scientific claim.

This is an application of SDR-001: the concern belongs in a blocking chronicle only when
it changes a gate, permitted action or executable research step. The explanatory lesson
belongs in pedagogy; benchmark and replay outputs belong in evidence.

## Equivalence is claim-specific

"Equivalent" must not be left unqualified. Depending on the frozen scientific contract,
the required relation may be:

- byte-identical per-unit outcomes;
- identical random-lineage descriptors and output ordering;
- identical gate inputs and selected stopping checkpoint;
- numerically equivalent results under a preregistered tolerance;
- distributional equivalence under a separately justified test.

The strongest relation that is feasible and relevant should be preferred, but a bytewise
test must not be mistaken for a universal proof of statistical independence. Conversely,
a distributional similarity check must not replace exact replay when exact replay is a
declared requirement.

## Evidence expected

Where applicable, assurance should include:

- serial-versus-parallel comparison;
- worker-count and scheduling-order invariance;
- isolated replay of a selected logical unit;
- canonical ordering and digest comparison;
- explicit tests of retry and interruption behavior;
- proof that completed evidence is not silently discarded or regenerated differently;
- profiling on the actual workload before selecting threads, processes, accelerators or
  further decomposition;
- a clear boundary between runtime telemetry and partial scientific outcomes.

## Pedagogical correction rule

Explanations of concurrency must avoid three common overclaims:

1. the GIL does not imply that all NumPy workloads cannot benefit from threads; many
   NumPy operations release it, so the execution model must be selected from profiling;
2. `SeedSequence.spawn()` produces independent, very probably non-overlapping streams,
   not a universal mathematical proof of independence;
3. an implementation speedup measured at the same worker count is not a parallel-scaling
   speedup and must not be presented as one.

## Consequences

- Concurrency changes made before outcome inspection still require assurance when they
  touch scientific semantics.
- Arithmetic seed offsets, retries and dynamic scheduling are not accepted merely because
  the resulting numbers look plausible.
- Worker count becomes a tested invariance when the scientific contract requires it.
- Infrastructure failures remain distinct from complete scientific failures.
- Future decomposition or resume mechanisms inherit the same evidence burden.
- The minimum-sufficient-mechanism rule still applies: do not add a distributed execution
  layer when a simpler, adequately evidenced execution path is sufficient.

## Challenge questions

1. Which concurrency properties are actually required by the estimand, and which are only
   operational preferences?
2. When is numerical equivalence sufficient, and when is exact replay necessary?
3. Can an exact deterministic replay hide a poor probabilistic stream construction?
4. Does a resume mechanism preserve the original stopping and failure semantics?
5. Is the assurance burden proportionate to the scientific decision at stake?
