# Derived Artifact Provenance Contract

Status: **draft proposal for independent review**

## Purpose

Research, engineering and pedagogical repositories often publish several views of the same underlying state: web pages, generated indexes, reports, release footers, dashboards, printable artifacts, evidence packs or static snapshots. When those views repeat provenance or release metadata manually, they can silently diverge from the object that is supposed to be authoritative.

This contract defines a reusable method for keeping derived publication artifacts traceable to a **single declared authority** without forcing any particular build system, static-site generator, framework or storage technology.

It generalizes a lesson observed in the Diderot consumer while remaining independent of Diderot's implementation.

## Core distinction

A workstream SHOULD distinguish:

- **canonical authority** — the object that owns the current meaning of a piece of metadata or structured state;
- **derived artifact** — a view produced from that authority for publication, navigation, review or execution;
- **frozen snapshot** — an intentionally immutable historical copy retained for reproducibility;
- **independent copy** — a manually maintained duplicate that can drift and is therefore not automatically authoritative.

A derived artifact may be important and reviewable without becoming a second source of truth.

## Single-authority rule

When a workstream declares one canonical authority for release, provenance, identity, notation, configuration or other structured metadata, it **SHOULD** generate, inject or validate derived views from that authority rather than maintain competing manual copies.

A consumer **MUST NOT** silently treat two independently editable copies as co-authoritative when disagreement between them could change what a reviewer, learner, operator or agent believes about the artifact under review.

If several sources are intentionally authoritative for different fields, that partition MUST be explicit.

## Derived publication rule

For a derived artifact, the workstream SHOULD make recoverable:

1. the canonical source or sources from which it was produced;
2. the immutable or otherwise bounded source coordinate when reproducibility matters;
3. the transformation/build mechanism or command when the output is not trivially reconstructible;
4. the generated artifact or a stable reference to it;
5. the check that detects material divergence when a derived view can become stale.

The derived artifact MAY compress or rearrange information for usability. It MUST NOT silently change the authoritative meaning of the metadata it republishes.

## Frozen snapshot exception

An intentionally frozen publication snapshot MAY be retained even after the canonical source evolves.

A frozen snapshot is acceptable when:

- its frozen status is explicit;
- its source coordinate is recorded;
- it is not presented as the current canonical state;
- later consumers can distinguish historical reproducibility from current authority.

A frozen snapshot is not a failure of single authority. An unlabeled stale copy is.

## Divergence handling

Validation SHOULD fail or surface an explicit discrepancy when:

- a derived artifact contains a value that contradicts its declared authority;
- a required canonical field is missing from a derived artifact that claims to publish it;
- a build step cannot determine which authority owns a repeated field;
- a historical snapshot is presented without enough provenance to distinguish it from current state.

A build MAY repair missing derived metadata deterministically from the canonical source when that transformation is itself part of the declared publication mechanism.

A build MUST NOT silently choose between contradictory authoritative values. Contradiction is a provenance defect that requires resolution.

## Evidence boundary

Successful generation or consistency checking is **engineering/provenance evidence**. It demonstrates that the declared publication relationship is being enforced for the checked artifact.

It does not, by itself:

- validate a scientific claim;
- upgrade a pedagogical explanation into scientific evidence;
- prove that the canonical source is scientifically correct;
- prove completeness outside the declared fields and transformation.

The source of authority and the truth of the scientific claim remain separate questions.

## Relation to existing harness contracts

This contract is broader than the notation-specific single-source publication rule in `pedagogy/MATHEMATICAL_NOTATION_CAPITALIZATION.md`.

The notation contract remains the authority for notation identity, meaning, provenance and encounter history. This document only generalizes the publication/provenance pattern: several views may exist, but their relationship to the canonical authority must remain explicit.

The same pattern can apply to release metadata, generated concept indexes, configuration summaries, evidence manifests, report headers or other derived publication state.

## Example pattern

A repository may declare:

```text
site.config.json
      |
      | canonical release metadata
      v
build / validation step
      |
      +--> HTML release footer
      +--> generated index
      +--> reviewer artifact metadata
```

The HTML pages are publishable artifacts. They do not become independent release authorities merely because a human can read them.

If a page already contains the canonical signature, a deterministic build may preserve it. If required metadata is missing, the build may inject it from the authority. If a page contains a contradictory release value, the safer behavior is to surface the contradiction rather than choose silently.

This example illustrates the method; it does not prescribe a specific implementation.

## Anti-patterns

Avoid:

- copying a version, review status or provenance string into many files and updating them manually;
- treating a generated report as the new authority without explicitly changing ownership;
- using a moving branch as the only provenance coordinate for an artifact intended to support replay;
- accepting a stale derived view because its presentation looks plausible;
- fixing contradictory copies by whichever value appears newest without an authority rule.

## Consumer conformance questions

A consumer applying this contract SHOULD be able to answer:

- What object is authoritative for each repeated field or structured state?
- Which published artifacts are derived views rather than authorities?
- How can a reviewer trace a derived artifact back to its source coordinate?
- What transformation or build step creates the view?
- What check detects stale or contradictory derived state?
- Which snapshots are intentionally frozen, and how are they labeled?
- Could two editable copies disagree without a gate noticing?

## Origin and promotion boundary

The immediate trigger for this proposal was an independently reviewed Diderot change in which CI exposed stale/missing release metadata and the consumer preserved a single release authority while repairing derived publication behavior.

The Diderot implementation exercised deterministic repair/injection of missing release metadata; it did not yet exercise detection of present-but-contradictory values. Its evidence therefore motivates, but does not validate, the stronger contradiction rule proposed here.

Consumer evidence motivates this reusable method; it does not make Diderot's implementation normative for other repositories.

Before this document is treated as a canonical companion specification, independent review should decide:

- whether the SHOULD/MUST levels are proportionate;
- whether the rule belongs directly in `HARNESS.md`, remains a design companion, or both;
- whether more than one consumer example is needed before stronger conformance requirements are added.
