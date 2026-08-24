# Experiment Pedagogical Capitalization Contract

Status: **draft reusable harness specification**

## Purpose

A scientific study often generates reusable understanding that is broader than its final result: evaluation metrics, data-role discipline, model architecture, uncertainty methods, failure modes, provenance rules, or distinctions such as outcome-bearing versus non-outcome-bearing execution.

This contract defines how an adopting project should identify and export those reusable concepts without turning pedagogy into scientific authority.

It complements `PEDAGOGICAL_CONCEPT_CONTRACT.md`:

- the concept contract explains **how one difficult concept should be taught**;
- this capitalization contract explains **how a study should discover which concepts deserve durable pedagogical extraction**.

## Core rule

At each material study transition, the workstream SHOULD ask:

> What did we have to understand correctly in order to design, execute, interpret, falsify, or stop this study?

The answer should be captured separately from scientific evidence and separately from the Chronicle.

Pedagogical extraction MUST NOT:

- change a scientific claim or gate;
- retroactively redefine an estimand;
- convert a failed experiment into a success story;
- present a study-specific convention as a universal law;
- hide uncertainty, negative results or protocol limitations;
- substitute an encyclopedia page for the authoritative scientific artifact.

## Capitalization candidates

A study SHOULD consider pedagogical extraction when it introduces or materially clarifies one or more of the following:

1. **Data and dataset roles**
   - TRAIN / VALIDATION / SCREEN / TEST;
   - external-validity reference versus local replay data;
   - benchmark protocol versus dataset contents;
   - identity/image/template overlap and leakage.

2. **Models and algorithms**
   - model architecture and representation role;
   - control/baseline algorithms;
   - training versus frozen inference;
   - learned versus non-learned transformations;
   - algorithmic choices that change the scientific question.

3. **Evaluation protocol**
   - operating-point metrics;
   - threshold selection and freeze semantics;
   - point criteria versus descriptive curves;
   - non-inferiority or equivalence logic;
   - multiplicity and seed rules;
   - staged screening versus qualification.

4. **Statistical structure**
   - sampling unit;
   - dependence structure;
   - bootstrap/resampling unit;
   - confidence interval or UCB interpretation;
   - distinction between Monte-Carlo precision and sample-size information.

5. **Evidence governance**
   - outcome-bearing versus non-outcome-bearing execution;
   - scientific gate versus understanding gate;
   - infrastructure failure versus scientific failure;
   - preregistration amendment boundaries;
   - what a human GO does and does not authorize.

6. **Provenance and reproducibility**
   - checkpoint/model/data/code identities as distinct objects;
   - immutable commit and content hashes;
   - licence/usage-right separation;
   - environment/SBOM capture;
   - deterministic replay and concurrency equivalence.

7. **Historical mistakes that changed understanding**
   - a wrong independence assumption;
   - a leakage path;
   - a misleading proxy;
   - a silent failure mode;
   - a reviewer correction that exposes a general lesson.

## Study-to-pedagogy map

A consuming repository SHOULD maintain a lightweight study-to-pedagogy map when a study creates several reusable concepts.

Recommended structure:

```yaml
study_id: study_x
scientific_authority:
  protocol: path/to/protocol
  results: path/to/results

pedagogical_exports:
  - concept_id: outcome_bearing_execution
    reason: "Needed to separate engineering preflight from protected scientific evidence"
    local_source: path/to/authorization_or_protocol
    target: "Diderot or local pedagogy repository"
    status: proposed | drafted | reviewed | published
    scientific_authority: false
```

This map is not required to be machine-readable if the project is small, but its semantics should remain recoverable.

## Required content for an exported concept

An exported concept SHOULD follow `PEDAGOGICAL_CONCEPT_CONTRACT.md` and additionally make the following recoverable:

- **originating study or studies**;
- **authoritative local source** that motivated the explanation;
- **scope boundary**: what the concept does not establish;
- **generalizable core** separated from study-specific numbers or implementation details;
- **worked example** from the study when legally and scientifically appropriate;
- **misconception** exposed by the study, when one exists;
- **stable pedagogical destination** if the project uses an external living encyclopedia such as Diderot.

## Keep study-specific details local

The generic harness owns the extraction rule, not the domain content.

Therefore:

```text
scientific-research-harness
  -> capitalization rule
  -> concept-structure rule
  -> assurance that pedagogy is not scientific authority

consumer research repository
  -> exact model / dataset / metric / protocol / mistake
  -> study-specific worked examples
  -> local pedagogy map

living encyclopedia (optional)
  -> reviewed reusable explanation
  -> stable semantic links
```

For example, the harness may define that an operating-point metric should be explained if it is decision-critical. It should not define the scientific acceptance threshold of a particular biometric study.

## Relation to publication artifacts

A study transition SHOULD normally produce two distinct reader-facing layers when enough new method has accumulated:

1. **scientific/methods publication layer**
   - study question;
   - protocol;
   - data sources and roles;
   - models/algorithms;
   - metrics and gates;
   - results when authorized and available;
   - limitations and next admissible action.

2. **pedagogical capitalization layer**
   - concepts extracted from the study;
   - prerequisite-aware explanations;
   - worked examples;
   - misconceptions;
   - understanding gates;
   - stable links back to the authoritative study artifacts.

The two layers may reference each other but MUST NOT be merged semantically. A polished explanation cannot repair missing scientific evidence.

## Version-transition trigger

A new publication or major study version SHOULD trigger a capitalization review when it changes at least one of:

- primary scientific substrate or model family;
- benchmark hierarchy;
- estimand or operating point;
- statistical unit or uncertainty method;
- gate logic;
- execution-evidence boundary;
- scientific conclusion in a way that creates a reusable methodological lesson.

This rule is intentionally broader than “new result”. A pre-execution protocol can deserve pedagogical extraction if it introduces important reusable distinctions.

## Understanding-gate examples

A learner should be able to do at least one of the following for each material exported concept:

- explain why the concept matters without study-specific jargon;
- identify a realistic failure if it is ignored;
- classify a worked example correctly;
- distinguish two nearby but non-equivalent concepts;
- predict whether a proposed action would cross an outcome/evidence boundary;
- derive or verify a small special case when mathematical structure is involved.

Understanding gates remain pedagogical only.

## Example: face-embedding compression study

A face-embedding compression programme may export concepts such as:

- outcome-bearing execution;
- FMR/FNMR and TAR/FAR at fixed operating points;
- non-inferiority margin and one-sided decision logic;
- identity-dependence-aware resampling;
- backbone qualification before downstream compression;
- TRAIN/SCREEN/TEST separation;
- checkpoint hash versus code licence versus model/data rights.

The study-specific thresholds, datasets, checkpoint identifiers and conclusions remain in the consumer repository. The living pedagogical explanation may use them as examples, but it is not the source of scientific authority.

## Minimum handoff addition

When the adopting project uses this contract, a handoff SHOULD include:

- pedagogical concepts added or materially changed;
- concepts still worth extracting;
- their authoritative study sources;
- whether the destination is local or external;
- open review state;
- exact next pedagogical action.

This prevents useful methodological learning from disappearing when the scientific task moves on.
