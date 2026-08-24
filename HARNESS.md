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

### 4b. Engineer code with proportional care

When a workstream produces executable code, it MUST make the implementation understandable
and reviewable by a competent human. At minimum, the repository MUST document the relevant
system context, software architecture or decomposition, and code structure/change rules.
Code that materially changes those boundaries MUST update the corresponding documentation.

Engineering assurance MUST be proportional to the artifact and its exposure. The label
`POC` or `MVP` is not a waiver for basic engineering care, but it is also not a reason to
build production ceremony before it is decision-relevant. The local adoption manifest MUST
select or justify an engineering-care profile:

| Profile | Minimum care |
|---|---|
| Spike | Disposable/local only; no secrets; reproducible run; syntax/build check; explicit limitations. |
| POC | Spike + small architecture note; focused unit tests; code/static checks; dependency/secret review; relevant security assumptions. |
| MVP | POC + deployable architecture and code docs; tests for critical logic and smoke path; automated code/security checks; least privilege; hardened defaults; explicit residual risks. |
| Production | Risk-based project standard stronger than MVP; threat model, operational controls, recovery/monitoring, supply-chain hardening and verification depth appropriate to impact. |

For web/API software, OWASP Top 10 MUST be treated as an awareness baseline, not as proof of
coverage. Projects SHOULD derive testable security requirements from the current OWASP ASVS
(or a stricter applicable standard) and use the current OWASP Top 10, SAMM/DSOMM and relevant
cheat sheets to prioritize risk. A project MUST NOT claim “OWASP compliant” merely because
a scanner or checklist ran.

For POC/MVP code exposed outside a local machine, the smallest sufficient automated set
SHOULD normally include: unit tests for critical pure logic, syntax/type/lint or equivalent
code checks, secret scanning, dependency/supply-chain review, static security analysis where
supported, and an executable smoke/health check. Containerized services SHOULD additionally
run as non-root where practical and scan the built image for material vulnerabilities.
Long-lived cloud credentials SHOULD NOT be stored when workload identity/OIDC is available.

Security findings MUST be fixed, shown not applicable, or explicitly accepted with bounded
scope and rationale. The workstream MUST distinguish a missing check from a passing check.
A POC/MVP MAY record a deliberate deferred control when implementing it now would add more
complexity than decision value, provided the risk, trigger for escalation and next action are
visible.

### 4c. Promote telemetry to engineering/test evidence only by contract

Runtime telemetry MAY become engineering or test evidence when it is evaluated against an
expected signal contract declared before the relevant execution. It does not thereby become
scientific evidence; the provenance classes MUST remain distinct.

When OpenTelemetry signals are used as evidence, the workstream MUST identify the applicable
OpenTelemetry specification and semantic-convention version or immutable reference. It MUST
NOT silently redefine standardized signal fields, semantic attributes, span kinds or units.
Project-specific conventions SHOULD use a bounded namespace.

For the detailed telemetry evidence contract, see `design/TELEMETRY_EVIDENCE_CONTRACT.md`.

### 5. Explain

When work introduces a difficult or hidden prerequisite, apply the pedagogical concept
contract: vocabulary, intuition, concrete example, mathematical descent, plain-language
interpretation, executable check, misconception and understanding gate. This layer explains
the authoritative work; it does not replace it.

### 6. Chronicle and hand off

Record decisions and blockers append-only when they change what may happen next. A handoff
MUST identify the pinned harness ref, frozen artifacts, evidence produced, commands needed
for replay, unresolved gates and the exact next admissible action.

### 6b. Make delegated reviews directly navigable

When a pull request, gate review, independent verification, replay request, or other evidence
review is delegated to another human, LLM, agent or external tool, the request MUST provide
direct canonical URLs for the objects the reviewer is expected to inspect. The workstream
MUST NOT assume that repository search, indexing, prior conversation context, or connector
discovery will locate the target reliably.

For a pull-request review, the navigation block MUST include at least:

- the repository URL;
- the direct pull-request URL;
- the pinned harness URL at the exact immutable commit or tag used to judge compliance;
- the head/base commit or branch URLs when they are material to the review;
- direct URLs for non-obvious normative artifacts that cannot be reliably discovered from
  the pull request itself.

Immutable blob or commit URLs SHOULD be preferred for normative references. A moving branch
URL MAY be included for convenience, but MUST NOT replace the immutable reference that
defines the review basis. If a required object is inaccessible, the reviewer MUST report the
missing evidence and MUST NOT silently replace it with search snippets, memory, or a prose
summary supplied by the author.

The reusable request template is `templates/independent-pr-review-request.md`.

## Minimum conformance evidence

A consumer claiming compliance MUST make the following recoverable:

| Obligation | Minimum evidence |
|---|---|
| Pinned dependency | repository plus immutable harness ref |
| Scope | task or claim identifier and permitted action |
| Sources | authoritative references with provenance |
| Scientific framing | assumptions, estimand/claim and inferential limits |
| Execution | code/config/environment and reproducible command |
| Architecture, if code is produced | system/software/code documentation proportional to the selected care profile |
| Code quality, if code is produced | focused automated checks and tests, with failures visible |
| Security, if code is exposed or deployed | applicable secure-design requirements, automated checks and explicit residual risks |
| Telemetry used as test evidence, if applicable | predeclared expected-signal contract, SpanId/TraceId correlation, recoverable actual signals/query and explicit expected-vs-actual verdict |
| Randomness, if used | root entropy policy, task-bound lineage and replay metadata |
| Concurrency, if used | serial/reference equivalence appropriate to the claim |
| Gates | named criteria and current status |
| Pedagogy | prerequisite-aware explanation for material difficult concepts |
| Chronicle | traceable decisions without retrospective rewriting |
| Handoff | unresolved conditions and exact next admissible action |
| Delegated PR/review navigation, if applicable | direct repository/PR URLs plus immutable harness and material review-basis URLs |
| Load-bearing evidence retention, if applicable | durable bundle or controlled locator, cryptographic digest, producing run/commit and retention boundary |

Not every project needs randomness, concurrency, durable evidence copies or advanced mathematics.
`not_applicable` is acceptable only when justified explicitly.

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
- `design/INPUT_PIPELINE_AND_FROZEN_MODEL_CONTRACT.md`
- `design/TELEMETRY_EVIDENCE_CONTRACT.md`
- `pedagogy/STEP_STATE_SPEC.md`
- `pedagogy/PEDAGOGICAL_CONCEPT_CONTRACT.md`
- `templates/independent-pr-review-request.md`
- applicable Scientific Design Records under `design/`
- reusable case studies under `pedagogy/case-studies/`

Until these drafts are merged and tagged, consumers should pin an exact commit rather than
claiming conformance to a moving branch.
