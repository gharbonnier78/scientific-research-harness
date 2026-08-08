# Future industrial experiment execution — Colab / external compute

Status: **IDEA / PARKED — NOT ON THE CURRENT STUDY 0 CRITICAL PATH**

This note preserves a future infrastructure direction discussed during Study 0 without changing the current scientific method, execution contract, or next research action.

## Core idea

When a reviewed experiment becomes genuinely compute-heavy, GitHub CI should remain primarily the place for validation, equivalence, replay checks, contracts and small benchmarks rather than being stretched into a general compute platform.

A heavier execution path may use an auto-documented Colab notebook or equivalent external Python execution environment that:

- checks out an exact reviewed Git commit;
- verifies protocol/configuration hashes before execution;
- records environment, runtime and accelerator information;
- runs the same reviewed scientific code rather than maintaining a notebook-local fork;
- records seed lineage and execution provenance;
- writes immutable result artifacts and SHA-256 hashes;
- separates interactive human-triggered execution from unattended execution permissions;
- can later evolve toward managed/enterprise execution with IAM/service accounts and object storage if warranted.

## CPU/GPU boundary

Do not introduce GPU execution merely because a GPU is available. The current subject-bootstrap coverage workload is predominantly NumPy/Python CPU work. GPU acceleration should be considered only when profiling and implementation structure show a workload that can genuinely benefit from vectorized/tensor execution, with equivalence evidence against the reviewed CPU path.

Embedding extraction, training, projection learning or other tensor-heavy workloads are more natural accelerator candidates than the current bootstrap orchestration.

## Possible progressive execution architecture

```text
GitHub reviewed commit
        |
        +--> CI / Research Assurance
        |      - contracts
        |      - tests
        |      - replay
        |      - equivalence
        |      - small benchmarks
        |
        +--> reviewed execution notebook / external Python
               - exact commit checkout
               - environment capture
               - CPU/GPU chosen from measured need
               - deterministic configuration
               - immutable artifacts + hashes
               - human-triggered execution initially

Future, only if useful:

GitHub reviewed commit
        -> managed notebook/API execution
        -> IAM/service account
        -> object storage
        -> artifact verification / evidence ingestion
```

## Harness implication

A future generic Scientific Research Harness may define an **execution portability contract**: the scientific method and authoritative code remain the same while execution can occur in CI, locally, in Colab, or in managed cloud compute. The execution location must be explicit provenance, not a hidden methodological variable.

Automation is not itself a requirement for reproducibility. A human explicitly triggering a reviewed notebook can be scientifically acceptable when the exact commit, configuration, environment, seed lineage and output hashes are preserved.

## Current decision

Park this idea. Do not let it divert Study 0 from the current critical path:

1. obtain the corrected parallel-scaling evidence;
2. decide whether execution feasibility is sufficient;
3. if not, continue the preregistered engineering optimization sequence (`edge_weights` next);
4. revisit external/Colab execution only when compute requirements justify it.
