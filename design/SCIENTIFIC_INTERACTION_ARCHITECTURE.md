# Scientific Interaction Architecture

## Purpose

This document defines a reusable interaction architecture for human-led scientific work assisted by heterogeneous LLMs. It deliberately separates scientific roles, project state, source/context access, notifications, and external-system connectors.

The target interaction style is a **conversational scientific CLI**: a human can resume a study, ask for status, request a bounded review or verification, and inspect the resulting scientific state without requiring fully autonomous LLM-to-LLM loops.

## Architectural principles

1. **Human authority remains explicit.** LLMs assist reasoning; they do not silently converge on a scientific conclusion.
2. **Canonical state is external to chat memory.** A resumed session must reconstruct state from project artifacts, not from remembered conversation.
3. **Roles are portable.** `Scientist` and `Reviewer` are contracts that any capable LLM can execute.
4. **Determinism applies to context loading and state transitions, not to natural-language wording.**
5. **Review must be actionable.** Objections should identify evidence, falsification conditions, or a bounded next action.
6. **No automatic agreement loop.** The architecture must not optimize Scientist and Reviewer toward mutual agreement.
7. **Connectors are not duplicated.** GitHub access remains the responsibility of a GitHub connector or GitHub App; the Context Fabric does not become a generic GitHub client.
8. **Context Fabric is epistemic infrastructure.** It exposes curated, provenance-aware sources and analyses to humans and LLMs.
9. **Notifications attract attention; they do not own scientific state.**
10. **Automation is progressive and bounded.** Observe and prepare before automating reasoning.

## Responsibility boundaries

| Component | Responsibility | Explicit non-responsibility |
|---|---|---|
| Human Researcher | Scope, prioritization, arbitration, publication authority | Hidden autonomous execution |
| Scientist role | Hypotheses, analysis, experiment design, interpretation, response to review | Reviewer approval |
| Reviewer role | Challenge, falsification, methodological/statistical review, evidence requests | Editing the scientific result under review |
| Scientific Research Harness | Interaction contracts, resume protocol, schemas, reusable scientific workflow | Provider-specific connector logic |
| GitHub / project repository | Canonical project state, code, PRs, commits, reviews, artifacts, evidence pointers | Scientific reasoning |
| GitHub connector / GitHub App | Technical access to GitHub APIs and events | Scientific context qualification |
| Context Fabric | Curated sources, analyses, tags, provenance, cross-project retrieval | Generic GitHub operations |
| Diderot or other UI | Human discovery and exploration of resources | Canonical scientific state ownership |
| Notification channel | Attention, status, optional queued intents | Scientific authority |

## Canonical planes

### State plane

Project repositories hold the canonical state required to resume work: study manifest, active investigation/PR, claims, reviews, experiment results, evidence references, and checkpoints.

### Context plane

The Context Fabric exposes a reusable catalogue of internal and external resources. A resource may be a paper, standard, web page, GitHub repository, repository sub-path, commit, notebook, dataset, PDF, generated artifact, toy, simulator, or other evidence-bearing object.

A **resource** is distinct from its **analysis/digestion**. One analysis may reference a whole repository and, where necessary, precise locations such as commit, directory, file, anchor, or line range.

### Interaction plane

The Harness defines portable intents such as:

- `START <study>`
- `STATUS`
- `REVIEW PR <n>`
- `CHECK R-<id>`
- `CHALLENGE C-<id>`
- `TRACE C-<id>`
- `EVIDENCE C-<id>`
- `CONTINUE`

These are conventions, not a rigid parser grammar. Natural language remains valid when intent is unambiguous.

### Event / attention plane

GitHub webhooks or Actions may derive attention states such as:

- `needs_scientist_attention`
- `needs_reviewer_attention`
- `experiment_completed`
- `review_stale`
- `evidence_changed`

These events do not automatically trigger scientific reasoning by default.

## Resume contract

When asked to start or continue a study, the LLM must:

1. Resolve the study manifest.
2. Resolve the canonical repository and current study scope.
3. Resolve active branch/PR/investigation where applicable.
4. Read current commit and last reviewed/checkpointed commit.
5. Load open and recently resolved review findings.
6. Load relevant experiment/evidence state.
7. Load referenced curated external/internal sources when needed.
8. Compute what changed since the last checkpoint.
9. Display a concise checkpoint.
10. **STOP and wait for the human's next instruction.**

The checkpoint should include, when applicable:

- study and objective;
- canonical repository;
- current branch/PR;
- current commit;
- last reviewed commit;
- open findings;
- resolved findings awaiting verification;
- latest experiment/evidence state;
- meaningful changes since last checkpoint;
- recommended next bounded action.

## Scientific role separation

### Scientist

The Scientist should not defend previous output merely because it authored it. Reviewer objections are treated as hypotheses to test. When warranted, the Scientist should revise or abandon a claim.

### Reviewer

The Reviewer should not rewrite the result under review. It should classify issues such as fatal flaw, major revision, minor limitation, or extension; identify unsupported inference; inspect experimental and statistical validity; propose the cheapest discriminating test when possible; and distinguish evidence from plausibility.

## Human-visible reasoning

The architecture does not depend on exposing private model chain-of-thought. Instead it requires inspectable scientific reasoning artifacts:

- assumptions;
- claims;
- evidence consulted;
- objections;
- alternative explanations;
- requested experiments;
- decision/status changes;
- revised conclusions.

These are reproducible and auditable across models.

## Source/reference model

A generic resource reference SHOULD support:

- stable id;
- resource type;
- provider;
- canonical URI;
- repository and ref/commit where relevant;
- optional path, anchor, or line range;
- tags/domains/projects;
- provenance;
- analysis/review status;
- relations to claims, experiments, or other resources.

GitHub is one provider, not a special epistemic category.

## Progressive implementation

The preferred implementation sequence is:

1. versioned contracts and diagrams;
2. study manifest + resume protocol;
3. persistent review objects;
4. cross-model pilot (Scientist and Reviewer);
5. generic Context Fabric resource model;
6. dynamic human library (e.g. Diderot);
7. webhook-derived attention state;
8. mobile/desktop notifications;
9. selective automation only where repeated use demonstrates value.

## Non-goals

- Fully autonomous LLM-to-LLM scientific negotiation.
- Reimplementing GitHub APIs inside the Context Fabric.
- Treating conversation memory as authoritative scientific state.
- Treating LLM agreement as evidence.
- Forcing every scientific project into a heavyweight workflow engine.
