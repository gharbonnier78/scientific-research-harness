# SDR-001 — Chronicle entries require a behavioral effect

Status: **ACCEPTED DESIGN DIRECTION**  
Date: 2026-08-08

## Context

The first live use of the scientific chronicle in the Siamese Embedding Compression study exposed an important risk: a chronicle can easily become a second changelog, a second audit log, or a narrative archive of everything that happened.

A reviewer proposed a simple anti-process-theater test:

> If this entry were removed, would some research behavior, decision, gate, or allowed next action change?

If the answer is no, the record probably does not belong in the blocking scientific chronicle.

This criterion was immediately useful. `CHRON-20260808-001`, which records the computational-feasibility risk of the production coverage design, changes behavior: it prevents the production coverage gate from running until the risk is resolved by a feasibility benchmark or a semantics-preserving optimization with equivalence evidence.

By contrast, a process incident about an accidental branch/write mistake was useful provenance but did not constrain a scientific action. It therefore belongs in process history/changelog, not in the blocking chronicle.

## Decision

The default Scientific Research Harness should treat the scientific chronicle as a **decision-constraining layer**, not a general diary.

A chronicle entry must identify at least one concrete consequence such as:

- a named execution step that is blocked while the entry is OPEN;
- a gate or decision that cannot advance;
- an allowed next action that changes;
- a previously blocked action that is released when the entry is RESOLVED;
- another machine-checkable research behavior whose state depends on the entry.

If no such consequence exists, route the record elsewhere.

## Routing rule

Use the smallest appropriate artifact:

- **Chronicle** — material uncertainty or reasoning that constrains/re-enables scientific behavior.
- **Changelog / process history** — what changed, operational incidents, implementation history with no scientific decision consequence.
- **Scientific Decision Record (SDR)** — durable rationale for an important choice when the record itself does not need to block execution.
- **Erratum** — a correction to a published/executed scientific statement or method.
- **CAL / scientific gate** — claim admissibility and evidence sufficiency.
- **MMALS replay / manifest** — executable and artifact provenance.
- **Pedagogy entry** — a learning object, misconception, toy, visualization, or progressive explanation.

## Why

This rule protects the harness from process theater. The objective is not to maximize documentation density. The objective is to preserve information that changes what a careful researcher is permitted or expected to do next.

It also keeps the three planes distinct:

1. empirical evidence;
2. decision/reasoning control;
3. pedagogy and explanation.

## Important nuance

"Behavioral effect" does not mean every useful reasoning artifact must be executable code. A Scientific Decision Record may be scientifically valuable without changing a runner. The stricter rule applies specifically to the **blocking chronicle** because that layer is wired into preflight and gates.

The harness may later support a separate non-blocking reasoning notebook or design-memory layer, but it must not be confused with the execution-constraining chronicle.

## Consequences for future design

- Chronicle schemas should require an explicit consequence field (`blocks`, `changes_decision`, or equivalent).
- CI should reject chronicle entries with no declared consequence.
- Resolved entries remain because their state transition releases a previously constrained behavior.
- Pedagogical suggestions may reference chronicle entries but should not create blockers unless a genuine research decision depends on them.
- The `SKILL.md` for AI assistants should ask the behavioral-effect question before creating a chronicle entry.

## Challenge question

This rule itself should remain challengeable:

> Are there reasoning events that are scientifically important enough to preserve centrally but do not change any immediate decision or runner behavior?

If repeated real studies produce convincing examples, the architecture should add a distinct artifact class rather than quietly weakening the chronicle criterion.
