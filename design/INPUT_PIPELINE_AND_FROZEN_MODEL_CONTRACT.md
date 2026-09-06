# Input Pipeline and Frozen Model Contract

Status: **draft reusable contract**

## Purpose

A frozen model does not by itself define a frozen experiment. When scientific or engineering
results depend materially on how inputs are decoded, transformed, aligned, normalized,
selected, filtered or otherwise prepared before inference, the effective experimental object
is the **model plus its input pipeline**.

This contract generalizes a recurring failure mode without assuming that every consumer uses
images, CNNs, embeddings or even machine learning. It applies only when the local project
determines that input preparation can materially alter the measured outcome, estimand, gate or
scientific interpretation.

The consumer project owns the domain-specific input semantics. The harness owns the reusable
requirement to make those semantics explicit, proportionate and replayable when they matter.

## Applicability decision

Before outcome-bearing execution, a consumer MUST classify this contract as one of:

- `applicable`: preprocessing/input preparation can materially alter the result;
- `partially_applicable`: only named stages are material and must be frozen;
- `not_applicable`: justified because no material input-preparation stage exists or because the
  authoritative execution already fixes it immutably.

The justification MUST be recoverable. The harness MUST NOT force image-specific controls,
face alignment, color-space checks, augmentation rules or other domain details into projects
where they are irrelevant.

## Core rule

If this contract is applicable, the consumer MUST identify the effective execution chain from
source input to model-consumed tensor/object and from model output to the quantity used by the
scientific or engineering decision.

A suitable generic form is:

```text
source input
  -> decoding / canonicalization
  -> selection / detection / segmentation / alignment (if applicable)
  -> resizing / projection / feature construction (if applicable)
  -> representation convention
  -> normalization / scaling
  -> frozen model or algorithm
  -> deterministic postprocessing
  -> scientific quantity
```

Only stages that can influence the bounded claim need to be frozen. Irrelevant ceremony is not
required.

## Minimum input-contract fields when applicable

The local project SHOULD record, as relevant:

- exact source/dataset identity and version or immutable manifest;
- stable sample identifiers and provenance;
- decoding/orientation/canonicalization rules;
- dimensionality, shape, sampling rate or spatial/temporal resolution;
- representation convention, such as channel order, units, coordinate frame or encoding;
- normalization/scaling and numeric dtype;
- deterministic selection, detection, segmentation, alignment or interpolation rules;
- behavior for malformed, ambiguous, missing, multi-object or failed inputs;
- exclusion policy and counts/reasons;
- augmentation or test-time transformation policy;
- duplicate and near-duplicate policy where dependence or leakage can matter;
- subject/entity/template/group identifiers when observations are statistically dependent;
- software/library/version dependencies that can alter preprocessing behavior;
- exact model/checkpoint/artifact identity and immutable digest where applicable;
- deterministic postprocessing applied before the reported scientific quantity.

A project MAY store this information in one contract or distribute it across dataset manifests,
configuration files, code and provenance records, provided the effective chain is reviewable and
replayable.

## Hidden-preprocessing prohibition

When input preparation is scientifically material, the project MUST NOT silently introduce
transformations after inspecting outcome-bearing results. Examples include undocumented
resizing, denoising, sharpening, super-resolution, imputation, smoothing, color conversion,
normalization changes, data cleaning, sample removal or heuristic quality filtering.

A material transformation added after outcomes are opened is a protocol change and inherits the
scientific review burden of the affected claim.

## Failure and exclusion semantics

An input-processing failure is not automatically a product/model failure and is not automatically
ignorable. If failures can affect representativity or the measured endpoint, the project MUST:

- classify failure modes before interpretation where practical;
- retain stable sample identifiers;
- record exclusions and reasons;
- distinguish corrupt/missing input, preprocessing failure, model execution failure and valid
  model output;
- avoid silently changing the denominator of a metric.

The correct treatment is domain-specific and remains owned by the consumer protocol.

## Dependence and leakage

Where repeated subjects, entities, templates, sessions, sources, devices, sites or derivative
samples can create dependence, the project SHOULD preserve those identifiers through the input
pipeline. Random splitting of individually transformed samples MUST NOT be assumed independent
when the underlying scientific unit is shared.

Exact-duplicate and near-duplicate audits SHOULD be proportionate to the risk that overlap can
inflate apparent performance or contaminate screen/validation/test roles.

## Reproducibility controls

When applicable and proportionate, the project SHOULD establish one or more non-outcome-bearing
controls before opening protected scientific results:

### Preprocessing fingerprint

Run a small fixed set of non-protected fixtures through the material preprocessing chain and
record stable digests or canonical summaries of the resulting model inputs. This detects drift
in decoding, orientation, interpolation, representation convention, normalization and related
stages.

### Deterministic inference replay

For deterministic frozen inference, replay a fixed non-protected input through the complete
pipeline and verify that the model-consumed input and final representation/output are stable
under the declared environment. A digest of the serialized output MAY be used when byte-level
stability is part of the contract.

### Concurrency equivalence

If preprocessing or inference is parallelized, sharded or distributed, verify the equivalence
relation required by the local claim. Worker count or scheduling MUST NOT silently alter sample
identity, ordering, transformations, outputs or random-state lineage.

### Restart/resume equivalence

If execution is restartable, resumed work MUST preserve provenance and scientific identity.
Partial or corrupt outputs MUST NOT be mistaken for complete valid artifacts.

### Representation sentinels

Where multiple conventions are plausible and materially different (for example unit systems,
coordinate frames, byte order, channel order or normalization), the project SHOULD include a
bounded sentinel that fails closed on the wrong convention.

These controls are engineering/provenance evidence unless the local protocol explicitly defines
them otherwise. They do not themselves satisfy a scientific gate.

## Durable evidence retention

When a control, replay, qualification run, independent review or scientific execution produces
an artifact that may later be needed to justify a decision, reproduce a claim, diagnose a defect
or hand off the study, the project MUST decide whether the platform-generated artifact is durable
enough for the expected evidence lifetime.

A temporary CI artifact, transient object-store URL, notebook runtime file or expiring attachment
MUST NOT be treated as the sole durable evidence source when loss of that object would make a
material decision unreconstructable.

When durable retention is required, the project SHOULD archive the smallest sufficient evidence
bundle in a repository or other controlled durable store, subject to size, confidentiality,
licence and data-governance constraints. The retained bundle SHOULD include, as applicable:

- the exact report/result files needed to reconstruct the verdict;
- a cryptographic digest of the bundle and important contained artifacts;
- run/job/artifact identifiers and the exact code/config commit that produced them;
- relevant environment/protocol/model/dataset identifiers;
- the evidence class and authority boundary (for example engineering/provenance vs scientific);
- retention limitations, omitted sensitive bytes and the reason for omission;
- a README or machine-readable manifest explaining replay/retrieval.

Large, sensitive, licensed or regulated artifacts SHOULD NOT be copied into Git merely for
convenience. In those cases the repository SHOULD retain a content-addressed manifest and a
stable controlled-store locator/version/retention record sufficient to retrieve and verify the
artifact under the applicable access policy.

GitHub Actions artifacts or equivalent CI attachments MAY serve as transport and short-term
review evidence, but their expiry policy MUST be recorded when they are material. A project
SHOULD NOT discover at handoff time that the only copy of a load-bearing evidence ZIP expired.

The retention decision is conditional on the real project situation: trivial diagnostics may be
left ephemeral, while load-bearing evidence required for a gate, review, audit, publication or
future replay should be retained proportionately.

## Artifact identity and preprocessing identity

A model/checkpoint name is not sufficient scientific identity when multiple artifacts or
preprocessing conventions can exist under the same label. The consumer SHOULD bind together:

- artifact locator(s);
- immutable digest(s);
- architecture/implementation identity when relevant;
- material preprocessing contract;
- environment information needed for replay.

A source URL is a locator, not an artifact identity. Functional similarity alone MUST NOT be
silently treated as exact artifact equivalence when exact identity is required by the local
protocol.

## Change control

Before outcome-bearing execution, material input-pipeline choices SHOULD be frozen to the level
required by the claim. After outcomes are opened:

- a semantics-preserving implementation change requires evidence appropriate to the declared
  equivalence relation;
- a material scientific change requires a versioned protocol amendment and applicable review;
- a change made only for runtime convenience MUST NOT silently alter exclusions, ordering,
  randomization, sampling units or reported denominators.

## Chronicle and handoff

A project SHOULD add an append-only Chronicle entry when an input-pipeline discovery changes what
may happen next, for example:

- a previously implicit preprocessing convention becomes scientifically material;
- two nominally identical artifacts require different input conventions;
- a dataset failure/exclusion rule changes admissibility;
- preprocessing drift invalidates replay;
- an equivalence check closes or reopens a blocker;
- a temporary evidence artifact is promoted to durable retention because it becomes load-bearing.

The handoff SHOULD identify the frozen input contract, unresolved pipeline risks, replay command
or check, durable evidence location/digest where material, and the exact next admissible action.

## Pedagogical rule

When a hidden preprocessing dependency is non-obvious and reusable, the consumer SHOULD explain
it pedagogically. A useful summary is:

> A frozen model is not necessarily a frozen experiment; the material preprocessing path is part
> of the model as executed.

This explanation must not replace the executable input contract or scientific evidence.

## Non-goals

This contract does not require:

- image-specific preprocessing for non-image projects;
- a universal alignment, normalization or augmentation policy;
- byte-identical outputs when the local claim only requires a weaker reviewed equivalence;
- duplicate detection when overlap cannot affect the bounded claim;
- committing large/sensitive/licensed evidence bytes to Git when a controlled durable store is
  more appropriate;
- production-grade data governance for a bounded research spike where it adds no decision value.

The governing principle is proportionality: freeze and verify the input stages that can actually
change the scientific or engineering conclusion, and retain evidence only to the degree needed to
make material decisions reconstructable.