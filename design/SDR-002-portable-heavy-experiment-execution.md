# SDR-002 — Portable Heavy Experiment Execution

Status: DESIGN NOTE — NON-NORMATIVE / NON-BLOCKING

## Context

Some experiments eventually exceed the practical compute envelope of repository CI. That does not imply that scientific method, provenance, or replay rules should become coupled to a particular execution platform.

The desired separation is:

```text
reviewed protocol + code + replay contract
        |
        v
CI / research assurance validates behavior and evidence boundaries
        |
        v
heavy execution may run on an appropriate compute substrate
        |
        v
immutable, hashed artifacts return to the evidence chain
```

Possible execution substrates include a local workstation, a manually launched Colab notebook, managed cloud notebook execution, HPC, or another controlled compute environment.

## Design principle

**The execution venue is infrastructure, not hidden scientific method.**

A heavy experiment should remain scientifically the same experiment when moved between compatible execution substrates, provided that the frozen protocol, inputs, RNG semantics, software/environment requirements, failure semantics, and output/evidence contract remain unchanged.

CI should generally prove small-scale behavior, equivalence, replay, and contract compliance. It does not need to become a compute farm merely because a validated experiment is expensive.

## Candidate portable execution pack

A future harness may define a portable execution pack containing, where applicable:

- exact Git commit / source digest;
- frozen protocol and configuration references;
- environment or container lock;
- data/source manifests and hashes;
- RNG lineage / replay identifiers;
- execution entry point;
- resource declaration (CPU/GPU/memory when relevant);
- explicit non-outcome / outcome boundaries;
- start/end timestamps and execution-environment metadata;
- immutable result artifacts and SHA-256 manifest;
- replay/verification commands;
- human-readable execution summary.

A notebook may be one rendering of this pack rather than an independent scientific implementation.

## Colab-style interactive execution

A manually launched, self-documenting notebook can be a legitimate execution surface when the human explicitly starts the run and the notebook binds itself to the reviewed code/protocol rather than copying and silently modifying them.

Potential pattern:

```text
0. scope and evidence boundary
1. clone/fetch exact reviewed commit
2. verify commit + protocol hashes
3. install locked environment
4. print frozen configuration
5. run smoke/replay checks
6. configure resources
7. execute heavy experiment or shard
8. write immutable artifacts
9. hash outputs
10. export evidence bundle
11. print bounded decision summary
```

This can support personal/interactive compute while keeping authorization explicit. Automated managed execution may later use IAM/service identities and object storage instead of depending on a user's mounted personal drive.

## CPU / GPU boundary

Availability of a GPU is not by itself a reason to port an experiment to GPU. A GPU path is justified only when profiling shows that the workload is sufficiently tensorizable and the accelerator changes the operational feasibility materially.

Any CPU→GPU implementation change that touches numerical/statistical behavior requires equivalence evidence appropriate to the experiment. "Runs on GPU" is not evidence that the same scientific computation was preserved.

## Relationship to the harness

This note is intentionally not a Chronicle entry and has no blocking power. It records a future industrialization direction for experiments whose compute requirements exceed CI or local interactive limits.

The core harness separation remains:

- evidence supports;
- Chronicle constrains executable research behavior;
- replay reconstructs;
- CAL governs bounded claim admissibility;
- pedagogy explains;
- execution infrastructure supplies compute.

No execution platform may silently weaken the scientific contract.
