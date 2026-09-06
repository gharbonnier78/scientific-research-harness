# Conversational Scientific Session Protocol

## Goal

Provide a portable, model-independent contract for resuming and steering scientific work through short human commands while keeping canonical state outside the chat.

## Command intents

The following forms are RECOMMENDED shortcuts. They are not a strict grammar.

- `START <study>` — resolve and display current study checkpoint.
- `CONTINUE` — resume the current study from canonical state.
- `STATUS` — display state without performing scientific reasoning.
- `REVIEW PR <n>` — perform a bounded review of pull request `<n>`.
- `CHECK R-<id>` — verify whether a specific review finding is resolved.
- `CHALLENGE C-<id>` — challenge a claim and identify discriminating evidence.
- `TRACE C-<id>` — trace a claim to evidence, assumptions, experiments and reviews.
- `EVIDENCE C-<id>` — summarize current evidence for and against a claim.

Natural-language equivalents remain valid.

## START / CONTINUE behavior

On `START` or `CONTINUE`, the agent MUST reconstruct state from canonical project artifacts and MUST NOT rely on conversational memory when a canonical source exists.

Required sequence:

1. Find the study manifest.
2. Resolve canonical repository and study scope.
3. Determine active branch, pull request or investigation if defined.
4. Determine current commit and most recent review/checkpoint commit.
5. Read open review findings.
6. Read recently resolved findings still awaiting verification.
7. Read latest experiments, results and evidence references.
8. Resolve curated source references relevant to current open work.
9. Compute meaningful change since the last checkpoint.
10. Render the checkpoint.
11. STOP.

No scientific review or modification is automatically performed after the checkpoint unless the user's command explicitly requests it.

## Checkpoint format

A checkpoint SHOULD be concise and include only fields that exist:

```text
Study: ...
Objective: ...
Repository: ...
Active branch/PR: ...
Current commit: ...
Last reviewed commit: ...
Open findings: ...
Resolved awaiting verification: ...
Latest experiments/evidence: ...
Meaningful changes: ...
Recommended next bounded action: ...
```

## Bounded-action rule

Commands such as `REVIEW`, `CHECK`, `CHALLENGE`, `TRACE`, and `EVIDENCE` perform one bounded action and return control to the human.

The agent MUST NOT implicitly continue into another review cycle merely because it can infer a next action.

## Review result

A review action SHOULD distinguish:

- observation;
- evidence;
- inference;
- objection;
- severity;
- falsification or resolution condition;
- recommended next action.

## State persistence

Important review findings and scientific state changes SHOULD be persisted in repository artifacts when the project adopts the persistent-review phase of the harness.

Conversation text alone is not sufficient canonical state.

## Cross-model portability test

A project passes the basic portability test when two independent LLM sessions, without access to prior conversation history, can execute `START <study>` and reconstruct materially equivalent checkpoints from canonical sources.
