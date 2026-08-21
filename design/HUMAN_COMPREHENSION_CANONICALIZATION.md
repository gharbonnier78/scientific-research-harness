# Human Comprehension and Canonicalization

Status: **draft reusable harness specification**

## Purpose

This specification governs the transition from a technically supported result to durable, canonical knowledge inside a harness-adopting project.

It exists because automated generation and automated verification can both scale faster than human understanding. The harness therefore distinguishes:

- **scientific validity** — whether the evidence supports a claim;
- **human comprehension** — whether an accountable human can reconstruct and explain the claim and its support;
- **canonical status** — whether the project is willing to promote that result into its durable reference knowledge.

These are related but not interchangeable states.

## Canonicalization axiom

> **A claim cannot reach canonical status solely because an automated verifier accepted it.**
> **At least one accountable human must be capable of explaining the result, its evidence, limitations, provenance and significance.**

This is a normative harness axiom.

## Scope and semantics

The axiom applies to **canonical promotion**, not to the truth value of a claim.

A result MAY be scientifically valid, reproducible and machine-verified while still being non-canonical because it has not yet passed accountable human comprehension.

Conversely, a clear human explanation MUST NOT rescue evidence that fails a scientific gate.

Therefore:

```text
scientific gate      -> controls whether the evidence supports the claim
understanding gate   -> controls pedagogical progression
canonicalization gate -> controls promotion into durable reference knowledge
```

No one of these gates substitutes for the others.

In this specification, `canonical` means canonical **for the declared project, repository, corpus or knowledge base**. It does not imply universal scientific consensus or field-wide acceptance.

## Human comprehension gate

Before a claim is promoted to canonical status, at least one accountable human MUST be able to do all of the following at an appropriate level for the claim:

1. state the result without relying on the generated wording;
2. identify the evidence that supports it and the checks that were actually performed;
3. distinguish measured results from assumptions, extrapolations and interpretations;
4. explain material limitations, failure modes and unresolved uncertainty;
5. identify the provenance of authoritative sources, data, code and automated assistance;
6. explain why the result matters and what decision, theory, design or future work it can legitimately support;
7. answer reasonable challenge questions or reconstruct the key reasoning path when asked.

The gate is about demonstrated intellectual possession, not ceremonial approval.

Suitable evidence MAY include a teach-back, expert presentation, review discussion, derivation, annotated walkthrough, oral examination, or written explanation that is sufficiently specific to the claim.

## Minimal comprehension record

A project claiming canonical promotion SHOULD preserve a compact record such as:

```yaml
claim_id: <stable claim identifier>
canonical_scope: <project/repository/knowledge-base scope>
accountable_human: <name or accountable role>
evidence_refs:
  - <artifact or evidence reference>
provenance_refs:
  - <source/data/code/tool reference>
explanation_artifact: <talk, note, review, derivation, walkthrough, etc.>
limitations:
  - <material limitation>
significance: <what this result legitimately changes or supports>
open_questions:
  - <remaining uncertainty>
human_comprehension_gate: pass | fail | pending
canonical_status: candidate | canonical | superseded
```

Projects MAY use stronger records. The harness does not require a specific identity scheme, but accountability must be recoverable.

## Agent and verifier boundary

An LLM, agent, proof assistant, test runner, static analyzer, CI pipeline, formal verifier or other automated system MAY:

- generate candidate artifacts;
- assemble provenance;
- execute checks;
- challenge a result;
- reproduce experiments;
- detect inconsistencies;
- propose explanations;
- prepare a canonicalization candidate.

It MUST NOT, by its own successful output alone, establish that the human-comprehension gate has passed.

An automated verifier's acceptance is therefore a potentially strong form of evidence, but it is **not self-authorizing canonical authority**.

An agent MAY record `candidate` or `pending` states and MAY ask the human to demonstrate or review understanding. It MUST NOT silently convert an automated success into `canonical`.

## Chronicle retention: preserve informative friction

When a result is being prepared for durable reuse, the Chronicle SHOULD preserve material parts of the path that explain where understanding changed, including when relevant:

- failed hypotheses;
- counterexamples;
- reviewer objections;
- falsified interpretations;
- ablations that changed the conclusion;
- corrections and errata;
- changes in assumptions or estimands;
- surprising observations;
- discarded approaches whose failure teaches a reusable lesson.

The purpose is not to retain every intermediate token or draft. It is to preserve **informative friction**: evidence of where the problem was genuinely difficult and where the mental model changed.

A polished final artifact SHOULD NOT erase the provenance of those material changes.

## Canonicalization pipeline

A project MAY model the downstream path as:

```text
candidate result
    -> scientific verification
    -> interpretation
    -> accountable human comprehension
    -> review / digestion
    -> canonicalization candidate
    -> canonical knowledge in declared scope
```

The stages need not be implemented as a rigid workflow. Their purpose is to prevent a single proxy — such as green CI, formal verification, benchmark success, publication, artifact count, or polished exposition — from being mistaken for the whole knowledge process.

## Goodhart guard

The harness MUST NOT define research closure or canonical quality solely through easy-to-optimize production metrics such as:

- number of generated claims;
- number of experiments or notebooks;
- test count or coverage alone;
- benchmark wins alone;
- number of repositories, documents or pages;
- verifier-pass count alone;
- publication count alone.

Such metrics MAY be useful operational signals, but they are not sufficient proxies for trustworthy, understood and reusable knowledge.

## Closure rule

A project MAY adopt the following compact closure maxim:

> **Closed when the accountable human can explain the whole chain.**

For a canonical claim, “the whole chain” means at minimum:

`question -> assumptions -> sources -> evidence -> reasoning -> failures/corrections -> limitations -> significance`

This maxim does not require that every research question be solved. A study can be correctly closed with a negative, partial or falsifying result if the evidence, uncertainty and significance are understood and made explicit.

## Application to LLM-assisted and agentic engineering

The axiom is domain-agnostic and SHOULD be reused for engineering governance when LLMs or agents help produce consequential artifacts such as:

- requirements and traceability;
- architecture and design decisions;
- infrastructure-as-code and deployment definitions;
- source code and generated configuration;
- test strategies, test cases and evidence;
- risk analyses and release recommendations;
- operational runbooks and change proposals.

A build, schema validation, formal check, unit-test suite, policy engine, or agent reviewer can establish important evidence. It cannot by itself make an engineering result canonical for the organization.

For consequential promotion, an accountable engineer or domain authority must remain able to explain what was produced, why it is supported, where it can fail, where it came from, and what decision it legitimately enables.

## Relationship to pedagogy and Diderot-like layers

Pedagogy is not an optional presentation layer after the science. The ability to reconstruct and teach the reasoning is evidence that the result has become intellectually usable by a human.

A separate knowledge layer — for example a project wiki, textbook, Diderot-like learning corpus, reference architecture, standard operating model or engineering handbook — can then act as a **canonicalization layer**. Such a layer SHOULD ingest only material whose evidence and comprehension status are explicit.

The harness governs the method and promotion criteria; the canonicalization layer owns the durable exposition and integration into its local knowledge graph.

## Provenance of this rule

The immediate design stimulus is Terence Tao, *Mathematics in the Age of AI* (2026), especially its analysis of verification, exposition, digestion, canonicalization, proof abundance, human review and the requirement that authors be able to explain their own results.

See `sources/tao-2026-mathematics-in-the-age-of-ai.md` for a source-bounded extraction and the explicit distinction between Tao's claims and the harness adaptations derived from them.