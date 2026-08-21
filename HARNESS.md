# Scientific and Pedagogical Harness Contract

Status: **draft normative entry point**

## Purpose

This repository defines a reusable contract for research work performed with one or more
LLMs. It is analogous to a project-level `SKILL.md`: it tells an agent how work must be
framed, evidenced, taught, replayed and handed off.

The harness is **not** a substitute for Diderot, a textbook, a researcher, a teacher, a
source paper, or domain expertise. It does not invent the scientific subject and it does
not become a parallel authority. It constrains the method used to work on that subject.

In short:

```text
authoritative sources + domain experts
                  |
                  v
          project-specific work
                  |
       constrained and evidenced by
                  |
                  v
 scientific-research-harness
```

## Normative language

`MUST`, `MUST NOT`, `SHOULD`, `SHOULD NOT` and `MAY` describe requirement levels.
A consumer may add stricter local rules but must not silently weaken a pinned harness
requirement.

## Dependency rule

Every adopting project, repository, or persistent LLM workstream MUST declare:

- the harness repository and immutable tag or commit SHA;
- the local adoption manifest;
- the authoritative scientific sources;
- local extensions and explicit deviations;
- the artifacts that contain claims, evidence, gates, pedagogy and handoff state.

A declaration is a provenance dependency, not an automatic file import. A template is
provided at `templates/harness-adoption.yaml`.

A repository SHOULD reference the manifest from the instruction file actually read by its
LLM agents, such as `AGENTS.md`, and from its human-facing contribution documentation.
A new LLM thread MUST begin by loading the pinned harness contract and the local manifest
before proposing outcome-bearing work.

If the pinned contract cannot be loaded, the agent MUST say so. It may perform bounded
orientation work, but it MUST NOT claim harness compliance or release a scientific gate.

## Authority boundary

The harness owns reusable process constraints:

- provenance and source traceability;
- explicit claims, assumptions and estimands;
- preregistered gates and append-only decision history;
- reproducible execution and environment capture;
- random-state and concurrency assurance when relevant;
- distinction between evidence, decisions and explanations;
- pedagogical descent, vocabulary, prerequisites and understanding gates;
- durable handoff of unresolved conditions.

The consumer project owns the subject matter:

- its sources, data, equations and domain assumptions;
- its implementation and experiments;
- its claims, thresholds and scientific gates;
- its domain-specific explanations and examples.

The harness MUST NOT present itself as the origin of domain knowledge. A pedagogical
explanation MUST point back to the authoritative object it explains. An understanding gate
MUST NOT change scientific evidence or authorize a claim.

## Canonicalization axiom

> **A claim cannot reach canonical status solely because an automated verifier accepted it. At least one accountable human must be capable of explaining the result, its evidence, limitations, provenance and significance.**

Automated proof, CI success, reproducibility checks, statistical validation, reviewer agents,
or other machine gates MAY establish bounded evidence states such as `verified`, `reproduced`
or `all_checks_passed`. They MUST NOT, by themselves, promote a claim to canonical status.

A project that claims a result is closed, understood, canonicalization-ready, or canonical
MUST make the accountable human understanding gate explicit. The accountable human need not
manually reproduce every machine computation, but must be able to reconstruct and explain the
question, assumptions, result, evidence, provenance, material limitations, and significance.
Where failed hypotheses, counterexamples, ablations, reversals, or unexpected observations
materially affect understanding, the chronicle SHOULD preserve them rather than replacing the
research path with a polished success narrative.

The normative rationale, source traceability, and consequences of this axiom are recorded in
`design/SDR-004-human-canonicalization-requires-accountable-understanding.md`.

## Required lifecycle

### 1. Bind

Before substantive work, load the pinned harness and local manifest. Record the task,
scientific object, permitted mutations, authoritative sources and current unresolved gates.

### 2. Frame

State the question, claim or engineering objective. Identify assumptions, estimand when
applicable, evidence required, prohibited inferences and the decision that the work may
support.

### 3. Execute

Preserve frozen inputs and provenance. Record code, configuration, environment, seed
lineages, scheduling semantics and intermediate states needed for replay. Engineering
changes that can alter an estimand, uncertainty calculation or scientific unit inherit a
scientific evidence burden.

### 4. Verify

Run the smallest sufficient checks for the bounded claim. Separate:

- scientific evidence from runtime telemetry;
- scientific gates from understanding gates;
- deterministic replay from statistical independence;
- measured results from extrapolations;
- infrastructure failure from scientific failure.

### 5. Explain

When work introduces a difficult or hidden prerequisite, apply the pedagogical concept
contract: vocabulary, intuition, concrete example, mathematical descent, plain-language
interpretation, executable check, misconception and understanding gate. This layer explains
the authoritative work; it does not replace it.

Pedagogy is part of closure, not optional presentation polish. A machine-verified result that
no accountable human can explain remains incomplete for canonicalization under this harness.

### 6. Chronicle and hand off

Record decisions and blockers append-only when they change what may happen next. A handoff
MUST identify the pinned harness ref, frozen artifacts, evidence produced, commands needed
for replay, unresolved gates and the exact next admissible action.

## Minimum conformance evidence

A consumer claiming compliance MUST make the following recoverable:

| Obligation | Minimum evidence |
|---|---|
| Pinned dependency | repository plus immutable harness ref |
| Scope | task or claim identifier and permitted action |
| Sources | authoritative references with provenance |
| Scientific framing | assumptions, estimand/claim and inferential limits |
| Execution | code/config/environment and reproducible command |
| Randomness, if used | root entropy policy, task-bound lineage and replay metadata |
| Concurrency, if used | serial/reference equivalence appropriate to the claim |
| Gates | named criteria and current status |
| Pedagogy | prerequisite-aware explanation for material difficult concepts |
| Human canonicalization gate | accountable human can explain result, evidence, limitations, provenance and significance before canonical promotion |
| Chronicle | traceable decisions without retrospective rewriting; material failed paths retained when epistemically relevant |
| Handoff | unresolved conditions and exact next admissible action |

Not every project needs randomness, concurrency or advanced mathematics. `not_applicable`
is acceptable only when justified explicitly.

## LLM thread startup record

A new project thread SHOULD emit a compact startup record before doing substantive work:

```yaml
harness_ref: <immutable commit or tag>
manifest: path/to/harness-adoption.yaml
task_id: <stable local identifier>
authoritative_sources:
  - <source>
current_gates:
  - <gate and status>
intended_action: <bounded action>
missing_context:
  - <anything preventing a compliant claim>
```

This record is not bureaucratic proof by itself. It makes the dependency and initial state
visible so later agents do not silently reconstruct them from memory.

## Inheritance and upgrades

A child project, sub-agent or new thread inherits the pinned contract of its parent
workstream unless it declares an explicit compatible override. Agent delegation MUST
preserve the same source, claim, gate and mutation boundaries.

Harness upgrades require an explicit change recording whether they affect scientific
meaning, execution semantics, or pedagogy only. Historical work remains bound to the ref
under which it was produced.

## Promotion rule

Reusable rules discovered in a consumer project SHOULD be proposed upstream. Domain-specific
content stays local. The harness generalizes method; it does not absorb each project's
scientific narrative.

## Canonical companion specifications

- `design/HARNESS_REUSE_MODEL.md`
- `design/SDR-004-human-canonicalization-requires-accountable-understanding.md`
- `pedagogy/STEP_STATE_SPEC.md`
- `pedagogy/PEDAGOGICAL_CONCEPT_CONTRACT.md`
- applicable Scientific Design Records under `design/`
- reusable case studies under `pedagogy/case-studies/`

Until these drafts are merged and tagged, consumers should pin an exact commit rather than
claiming conformance to a moving branch.
