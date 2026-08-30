# Pedagogical Concept Contract

Status: **draft reusable harness specification**

## Purpose

This contract defines how a difficult scientific or mathematical concept should be introduced when the harness is used for learning-oriented research.

It complements `STEP_STATE_SPEC.md`:

- a **step-state** preserves a meaningful change in understanding;
- a **concept contract** defines the minimum structure required to make one difficult concept understandable, traceable and executable.

It also complements `MATHEMATICAL_NOTATION_CAPITALIZATION.md`. This contract governs
understanding of the concept in the current learning step; the notation contract governs the
durable cross-context recording of non-trivial notation used to express that concept. Apply
both when both conditions are material, without duplicating the same pedagogical explanation.

The contract is domain-agnostic. It may be reused in biometrics, statistics, geometric machine learning, control, filtering, optimization, probabilistic modelling, or other research areas.

## Core pedagogical descent

A difficult concept SHOULD normally be introduced in this order:

1. **Plain-language intuition** — explain the object before introducing jargon.
2. **Concrete example** — show one small instance before generalizing.
3. **Mathematical descent** — derive the target expression step by step from already established objects.
4. **Immediate plain-language interpretation** — translate the mathematics back into ordinary language before moving on.
5. **Executable experiment** — demonstrate the concept numerically, visually, symbolically, or computationally.
6. **Misconception check** — identify at least one plausible but wrong interpretation when relevant.
7. **Understanding gate** — define what a learner must be able to explain, derive, predict, or verify before the next dependent concept is treated as understood.

This sequence is a default, not a ritual. A trivial concept may need fewer layers. A concept that remains opaque after one pass may require multiple examples or an `Autrement dit` view.

## Four mandatory questions for a new symbol or operator

For every non-trivial new symbol, operator, map, space, distribution, statistic, loss, invariant, or transformation, the pedagogical material SHOULD answer:

1. **What is it?**
2. **Why do we need it here?**
3. **Where does this expression come from mathematically?**
4. **What fails, becomes invalid, or becomes harder if we ignore its structure?**

Examples include symbols such as `Ad_g^*`, `g*`, a bootstrap multiplicity `m_i`, a covariance matrix, a likelihood, a Hamiltonian, a tangent space, or an operating threshold.

The answer may be compact for expert readers, but the questions should remain recoverable in the learning path.

These four questions are the local understanding check. When the symbol or operator is also a
non-trivial notation that should remain reusable across later contexts, the workstream SHOULD
also apply `MATHEMATICAL_NOTATION_CAPITALIZATION.md` to record its read-aloud form, formal and
plain-language meaning, provenance, encounters, ambiguity and cross-domain connections in the
consumer's canonical notation registry.

## Prerequisite graph

Concepts SHOULD declare their prerequisites explicitly when the dependency is non-obvious.

Example:

```yaml
concept: lie_algebra
prerequisites:
  - matrix_multiplication
  - derivative
  - tangent_space
```

The prerequisite relation forms a directed acyclic learning graph whenever possible:

```text
vectors / matrices
      |
      v
transformations
      |
      v
manifolds --> tangent spaces
      |          |
      +----------+
             |
             v
         Lie groups
             |
             v
        Lie algebra
             |
             v
      adjoint / dual
```

The graph is not a claim that every learner must follow one fixed curriculum. Its purpose is to make hidden expert priors explicit.

## Mathematical descent rule

Do not introduce a target equation only because it is standard in the literature.

Prefer:

```text
known object
  -> allowed operation
  -> intermediate identity
  -> target expression
  -> interpretation
```

For example, before writing a tangent-space constraint, start from the defining constraint of the manifold and differentiate it. Before introducing a coadjoint action, establish the group action, the adjoint action and the dual pairing it must preserve.

A mathematical descent SHOULD mark where a step uses:

- a definition;
- an algebraic identity;
- a theorem;
- an approximation;
- an assumption;
- a convention;
- a numerical method.

This prevents a derivation from looking logically stronger than it is.

## `En français dans le texte` rule

A sequence of equations SHOULD NOT be allowed to carry the explanatory burden alone when the target reader may lack the relevant priors.

After a mathematically meaningful step, state what it means operationally or conceptually.

Example pattern:

```text
Equation / derivation

En français dans le texte:
This constraint means that an allowed infinitesimal displacement must remain tangent to the admissible state space; a naive Euclidean update may leave it.
```

The exact language may be French, English, Spanish, or another project language. The rule concerns explanatory function, not a specific language.

## Executable example rule

When a concept affects computation, geometry, inference, uncertainty or a decision boundary, prefer an executable check over a purely declarative explanation.

A useful executable example can answer one of these questions:

- Does the claimed invariant remain constant?
- Does a naive implementation violate the constraint?
- Do two apparently identical observations lead to different futures?
- Does an approximation converge as expected?
- Does a resampling rule preserve the intended dependency structure?
- Does a numerical update stay on the manifold / group / admissible domain?

The example SHOULD be small enough to inspect manually when practical.

## Understanding gates vs scientific gates

These two gate classes MUST remain distinct.

### Scientific gate

Controls whether a claim, experiment, result, or next study is scientifically admissible.

Examples:

- coverage must pass;
- a confidence bound must remain within a preregistered margin;
- evidence must be reproducible;
- a protocol amendment must occur before outcome inspection.

### Understanding gate

Controls only the pedagogical progression.

Examples:

- explain the concept without notation;
- derive a simple special case;
- predict what will fail under a naive update;
- reproduce a toy calculation;
- identify the role of each term in an equation.

An understanding gate MUST NOT alter scientific evidence, claims, thresholds, or runner behavior.

A learner may fail an understanding gate while the experiment remains scientifically valid. Conversely, an experiment may fail a scientific gate even if the concept is perfectly understood.

## Misconception check

For concepts with a known semantic trap, record at least one misconception.

Example structure:

```yaml
misconceptions:
  - wrong: "observable variable = complete dynamical state"
    correction: "an observable can omit latent coordinates that still govern its future evolution"
```

Misconceptions may come from:

- an earlier project mistake;
- a reviewer correction;
- a common textbook shortcut;
- an ambiguous notation;
- a plausible engineering analogy that breaks under the formal model.

Historical mistakes can be especially valuable because they show where a reasonable mental model failed.

## Recommended machine-readable concept record

```yaml
id: concept_id
title: Human-readable title
status: draft | reviewed | stable
prerequisites:
  - prerequisite_1

intuition:
  summary: "..."

concrete_example:
  description: "..."

mathematical_descent:
  entry_objects:
    - "..."
  target_expression: "..."
  derivation_ref: "..."

plain_language:
  summary: "..."

executable_example:
  artifact: "path/to/notebook_or_test"
  expected_observation: "..."

misconceptions:
  - wrong: "..."
    correction: "..."

understanding_gate:
  - "explain_without_formula"
  - "derive_special_case"
  - "verify_executable_property"

authoritative_refs:
  - "paper / protocol / source section"

scientific_authority: false
```

## Reuse boundary

A consuming research repository SHOULD keep domain-specific concept instances, notebooks, experiments and explanations locally.

The generic harness repository SHOULD keep only reusable rules, schemas, examples and assurance patterns.

Therefore:

```text
scientific-research-harness
  -> generic pedagogical contract
  -> generic schemas / assurance rules

consumer research repository
  -> domain-specific concepts
  -> equations and derivations
  -> notebooks / experiments
  -> local understanding gates
```

Do not duplicate and fork the generic contract into every research repository unless a pinned copy is required for archival reproducibility. Prefer a declared harness reference and explicit local extensions.

## Origin and first new consumer

This specification generalizes lessons first developed while externalizing the pedagogical path of a Siamese biometrics study and then sharpened in the `geometric-latent-dynamics` project while planning a first-principles route toward Latent Lie-Poisson Neural Networks.

The LLPNN use case made one missing requirement especially clear: advanced notation must not silently assume differential geometry or geometric mechanics priors. The harness therefore needs a reusable mechanism for prerequisite graphs, mathematical descent, plain-language translation and understanding gates.

This origin is historical context, not a domain restriction.
