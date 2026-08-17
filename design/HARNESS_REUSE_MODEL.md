# Harness Reuse Model

Status: **draft design rule / non-normative until merged**

## Goal

The Scientific Research Harness is intended to be **externalized and reused**, not copied into each research project as an independent framework variant.

A consumer repository should be able to adopt the harness while keeping its scientific domain, experiments and pedagogical material local.

## Separation of responsibilities

### `scientific-research-harness`

Owns reusable mechanisms such as:

- research-assurance patterns;
- claim / evidence / gate conventions;
- chronicle and replay concepts;
- pedagogical step-state rules;
- pedagogical concept contracts;
- generic schemas and templates;
- execution-portability rules;
- reusable CI / validation logic when it becomes stable enough to share.

### Consumer research repository

Owns domain-specific objects such as:

- source papers and source provenance;
- hypotheses and claims for that study;
- mathematical derivations specific to the domain;
- datasets and manifests;
- code and notebooks;
- experiments and results;
- domain-specific scientific gates;
- concept instances and local understanding gates;
- handoff / learning traces for the study.

## Preferred consumption pattern

A consumer SHOULD declare which harness state it relies on.

During harness development, this may be a PR branch or commit SHA. After stabilization, prefer a tag or immutable commit SHA.

Example:

```yaml
harness:
  repository: gharbonnier78/scientific-research-harness
  ref: <tag-or-commit>
  imported_concepts:
    - pedagogy/STEP_STATE_SPEC.md
    - pedagogy/PEDAGOGICAL_CONCEPT_CONTRACT.md
  local_extensions:
    - pedagogy/concepts/
    - experiments/
```

The declaration is a provenance pointer. It does not imply that GitHub automatically imports files.

## Avoid silent forks

A project MAY temporarily copy a harness template for execution convenience, but it SHOULD record:

- the upstream repository;
- the exact upstream ref;
- whether the file is vendored or derived;
- any local changes;
- whether local changes should be proposed upstream.

Reusable improvements discovered in a consumer project SHOULD be promoted back to the harness rather than remaining as an undocumented local fork.

## Promotion rule

Ask of every new rule discovered in a consumer:

> Is this rule specific to the scientific domain, or would another rigorous study benefit from it unchanged?

If domain-specific, keep it local.

If reusable across domains, propose it upstream in `scientific-research-harness`.

Examples:

**Local:**
- the Lie-Poisson equation used in a geometric dynamics experiment;
- the LFW sparse-pair mapping used in a biometric study;
- a specific non-inferiority margin.

**Reusable:**
- prerequisite graphs;
- distinction between scientific and understanding gates;
- requirement to identify hidden expert priors;
- executable examples for difficult concepts;
- preservation of meaningful intermediate learning states;
- provenance rules for portable heavy execution.

## Compatibility principle

Harness evolution MUST NOT silently change a frozen study's scientific meaning.

A study may upgrade its harness reference only through an explicit change that records whether the upgrade affects:

- scientific claims;
- estimands;
- thresholds;
- evidence interpretation;
- execution semantics;
- only documentation / pedagogy.

Pure pedagogical improvements SHOULD normally be non-blocking and scientifically non-authoritative.

## First intended reuse path

`geometric-latent-dynamics` is an intended early consumer of the generalized pedagogical harness.

The project should keep its LLPNN-specific derivations, `SE(2)` experiments and source paper locally, while referencing the reusable pedagogical contracts from this repository.

The flow is therefore:

```text
Siamese biometrics study
        |
        | exposed reusable assurance + pedagogy needs
        v
scientific-research-harness
        |
        | generalized contract
        v
geometric-latent-dynamics
        |
        | discovers further reusable needs
        +-------------------------------> upstream proposals
```

This feedback loop is intentional: concrete studies should improve the harness, but the harness should not become coupled to any one study.