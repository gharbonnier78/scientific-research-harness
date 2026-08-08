# Case Study — Profile Before You Optimize

**Type:** pedagogical case study  
**Source:** Siamese Embedding Compression Lab, Study 0 v0.2.2 coverage preparation  
**Status:** derived learning artifact, not outcome evidence

## Learning objective

Show how a plausible engineering hypothesis can be corrected by measurement before it changes a scientific execution plan.

## The initial hypothesis

Code inspection identified `weighted_threshold_at_fmr` as a likely computational hotspot. Its worst-case structure loops over distinct impostor distances and repeatedly recomputes weighted cumulative sums, suggesting an unfavorable complexity profile.

That hypothesis was reasonable, but it was still only a hypothesis.

## The measurement

A non-outcome benchmark was run on the same approximate sparse geometry as the frozen coverage contract:

- 963 subjects;
- 1,000 observed pairs;
- 500 genuine edges;
- 500 impostor edges;
- frozen target FMR = 0.01;
- no historical Study 0 scores;
- no coverage outcome.

The benchmark showed that threshold selection was not the dominant measured cost at FMR = 0.01 because the implementation exits early once cumulative impostor weight exceeds the target.

The more important measured costs were subject multiplicity generation and Python-level `edge_weights` construction, repeated separately in representation and operational bootstrap paths.

## What changed

The optimization order changed from:

1. optimize threshold selection first

to:

1. deterministic dataset-level parallelism;
2. vectorize subject-edge weight construction;
3. consider reuse of identical subject draws/weights under equivalence proof;
4. optimize threshold selection only if re-profiling still justifies it.

The important scientific-engineering lesson is not that one function was slow and another was fast. It is that **the engineering decision changed because a measured result contradicted a plausible inspection-based prior**.

## A second hidden assumption: random streams

Parallelizing independent simulated datasets initially looked semantically low-risk. Review then exposed a deeper requirement: deterministic mapping from dataset index to a seed is not sufficient by itself as an explicit random-stream independence contract.

The execution plan was therefore strengthened to require a hierarchical NumPy `SeedSequence` design, for example:

```text
root seed
  -> scenario child stream
       -> dataset child stream
            -> data-generation child stream
            -> bootstrap child stream
```

Acceptance requires serial/parallel identity, worker-count invariance, scheduling-order invariance, replay of any dataset from recorded seed lineage, and no shared mutable RNG state.

This illustrates a second lesson: an optimization that appears orthogonal to statistics can still touch assumptions required by the inferential design.

## Diderot-style progressive views

### Intuition

Do not optimize the code that looks worst. Measure what actually dominates the execution path used by the experiment.

### Engineering view

Worst-case complexity does not determine practical runtime when control flow exits early under the frozen operating point.

### Statistical view

Parallelism is not only a scheduling problem. Independent Monte Carlo units require an explicit random-stream construction and replay contract.

### Reproducibility view

Record the operating point, benchmark geometry, environment, seed hierarchy, and extrapolation assumptions directly in the benchmark artifact rather than forcing future readers to reconstruct them from code.

### Reviewer challenge

Ask: "Which part of this conclusion comes from code inspection, which part comes from measurement, and which assumptions would change if the execution strategy changed?"

## Chronicle boundary

This case study is pedagogical. It does not itself block or release any research step.

The corresponding project chronicle entry is legitimate only because the computational-feasibility concern changes whether production coverage execution is permitted. If this learning artifact disappeared, the scientific runner should behave identically.

That separation is deliberate: **pedagogy explains the decision; the chronicle constrains the decision; evidence supports the decision.**
