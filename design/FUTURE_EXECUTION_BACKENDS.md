# Future execution backends for reproducible experiments

Status: **NON-NORMATIVE FUTURE DESIGN NOTE**

This note preserves a future industrialization idea without changing any current study protocol, runner, gate, claim, evidence, or execution decision.

## Idea

Keep scientific method and execution location separate.

A future harness should be able to validate the same frozen experiment in CI and execute heavy workloads on an interchangeable backend such as:

- local workstation;
- hosted notebook environment such as Colab;
- Colab Enterprise / managed notebook execution;
- HPC / cloud CPU or GPU environment;
- another controlled batch execution system.

The execution backend must not become a hidden part of the method.

## Desired split

```text
reviewed protocol + code + seeds + hashes
                |
                v
        CI / Research Assurance
  validate contracts, replay and smoke paths
                |
                v
       selected execution backend
 local / Colab / Enterprise / HPC / cloud
                |
                v
     immutable execution artifacts
 environment + config + lineage + timings
            + outputs + hashes
                |
                v
       replay / verification / CAL
```

## Colab-oriented path worth retaining

For experiments that genuinely require more CPU/GPU than CI should provide, a self-documenting notebook could act as an execution capsule rather than as a fork of the scientific code.

Possible structure:

1. explain what the notebook may and may not do;
2. clone or checkout an exact reviewed Git commit;
3. verify protocol/configuration hashes;
4. install a pinned environment;
5. show the frozen execution contract;
6. run a smoke/replay check before heavy execution;
7. select CPU/GPU resources only when the measured workload benefits from them;
8. execute a declared shard/checkpoint;
9. export environment, configuration, RNG lineage, timestamps and results;
10. hash the produced artifacts;
11. save/publish the evidence bundle through an authorized storage path.

A manually launched run by the researcher is acceptable if provenance and outputs are fully recorded. Automation is not itself a requirement for scientific reproducibility.

## Enterprise direction

If later useful, a managed/enterprise notebook execution path may replace interactive Drive-based workflows with service identities, IAM and controlled object storage. This is an industrialization option, not a current Study 0 dependency.

## GPU boundary

Do not move a workload to GPU merely because a GPU is available. Profile first. CPU-oriented NumPy/Python work may gain little without a semantics-preserving tensor/vectorized implementation. GPU-specific implementations require equivalence evidence against the reviewed reference path when they touch statistically sensitive execution.

## Harness principle to retain

> The same scientific object should remain reviewable and replayable even when the execution backend changes.

The backend may change throughput and operational convenience; it must not silently change estimands, sampling, RNG semantics, thresholds, stopping rules, failure behavior or evidence interpretation.

## Current-study boundary

This note does **not** alter Study 0. The current optimization/coverage work should continue on its existing path. Revisit this note only when execution cost, repeated use, or industrialization makes a separate heavy-compute backend materially useful.
