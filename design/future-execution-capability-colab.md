# Future capability — heavy experiment execution outside CI

Status: **non-blocking future industrialization note**

This note preserves an execution idea that emerged while working on Study 0. It is deliberately **out of scope for the current Study 0 path** and must not redirect the active research sequence.

## Idea to preserve

For experiments that eventually require substantial CPU/GPU resources, keep GitHub CI focused on validation, contracts, replay, equivalence tests and small benchmarks, while moving heavy execution to a reproducible external execution surface.

A promising intermediate form is a self-documenting Colab execution notebook that:

- checks out an exact reviewed Git commit;
- verifies protocol/configuration hashes;
- installs a pinned environment;
- records runtime/environment metadata;
- executes only the approved heavy workload;
- saves immutable result artifacts;
- computes hashes for exported artifacts;
- writes enough provenance for replay and later audit;
- can be manually launched by the researcher when interactive authorization boundaries (for example Drive access) make unattended automation inappropriate.

GPU use is not assumed to be useful by default. It should be introduced only when the workload is actually tensorizable/accelerator-friendly and only with equivalence evidence against the reviewed CPU/reference path where relevant.

A later enterprise form may use managed notebook execution, service accounts/IAM and cloud object storage instead of relying on personal Drive authorization. That is a future harness/industrialization concern, not a current Study 0 requirement.

## Harness implication

The long-term Scientific Research Harness may eventually distinguish:

1. **verification surface** — CI validates code, contracts, replay and boundaries;
2. **execution surface** — local/Colab/cloud/HPC executes heavy but already-reviewed workloads;
3. **evidence surface** — immutable artifacts, hashes, environment and execution provenance are returned to the evidence chain.

The execution location must not become a hidden methodological variable.

## Current boundary

Do not implement or operationalize this capability now merely because the idea exists. Continue the active Study 0 sequence and revisit this note only when execution cost becomes the actual blocker or when the harness is generalized for industrial experimentation.