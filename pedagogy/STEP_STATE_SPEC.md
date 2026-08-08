# Step-State Pedagogy

Status: **design rule / draft**

## Purpose

Research evolves through intermediate states that are often more instructive than the final paper. When an intermediate state materially changes scientific understanding, engineering decisions, uncertainty, or the learner's mental model, the harness SHOULD preserve a **step-state explanation**.

A step-state is not a new source of scientific truth. It is a versioned pedagogical view over the authoritative research state at a specific moment.

## When a step-state is worth creating

Create one when at least one of the following is true:

- a scientific claim or its admissibility changes;
- a methodological assumption is corrected, weakened, or rejected;
- a reviewer challenge changes the next experiment or implementation decision;
- a failed gate or negative result changes the research path;
- an engineering constraint materially changes what can be executed;
- profiling, simulation, or measurement overturns a plausible prior belief;
- a concept remains difficult enough that an explanation would materially improve understanding;
- the project reaches a meaningful minor or major milestone worth preserving before further work changes the context.

Do **not** create a step-state mechanically after every commit. If nothing important changed in understanding, evidence, or decision, a changelog entry is enough.

## The three possible views

A valuable step-state SHOULD consider three views of the **same authoritative object**.

### 1. PhD / scientific view

For readers who already possess the relevant priors.

Include only what is scientifically material:

- claim or question;
- estimand / formal object;
- assumptions;
- evidence and uncertainty;
- inferential limits;
- methodological status;
- unresolved questions.

Do not dilute this view with explanations of concepts that are routine for the target expert.

### 2. Engineering view

Explain what the scientific object means for an engineer or industrial decision:

- why the work exists;
- what system constraint it addresses;
- what can and cannot be concluded operationally;
- implementation consequences;
- cost, scaling, observability, reproducibility, and deployment boundaries;
- what decision the evidence can support.

This is not a simplification of the science. It is the same object viewed through system consequences.

### 3. `Autrement dit` / 12-year-old view

This view is **optional and value-driven, never mandatory**.

Create it only when a non-specialist explanation genuinely removes a conceptual obstacle. It may use simple language, a toy graph, analogy, or tiny numerical example, but MUST preserve the direction of the scientific claim and explicitly mark any simplification that could affect interpretation.

A good `Autrement dit` section answers questions such as:

- What is the object we are manipulating?
- Why are we doing this at all?
- What could go wrong if we used the obvious method?
- What does the equation mean in a tiny concrete example?
- What did we learn that changed the next step?

It MUST NOT become a parallel scientific narrative.

## Progressive amplification, not duplication

The three views are not three independent documents that drift apart. Prefer a layered structure:

```text
Authoritative step state
  ├── Scientific / PhD view
  ├── Engineering consequences
  └── Autrement dit (only when useful)
```

For an expert reader, the first two views may be sufficient. The `Autrement dit` section can live as a collapsible subsection, linked companion, or explicit subchapter so that elementary explanation does not pollute an expert's reading path.

## Preservation rule

A step-state that later proves incomplete or wrong SHOULD remain available as an historical pedagogical object if it records a genuine prior belief or understanding. Later states link to it and explain what changed. Do not silently rewrite the old explanation to make the final solution look inevitable.

## Relationship to the three harness planes

- **Evidence supports the decision.**
- **Chronicle constrains the decision.**
- **Pedagogy explains the decision.**

Removing a pedagogical step-state MUST NOT change scientific runner behavior, gates, claims, or evidence.

## Suggested metadata

A machine-readable step-state may include:

- `id`
- `recorded_at_utc`
- `source_scope`
- `trigger`
- `importance`: `minor | major`
- `authoritative_refs`
- `scientific_view`
- `engineering_view`
- `autrement_dit`: optional
- `what_changed`
- `what_remains_open`
- `supersedes` / `superseded_by`
- `outcome_evidence_created: false`

## End-of-study expectation

At study closure, the harness SHOULD assemble selected step-states into the Learning & Replay Pack. The final learning path should show not only the polished result, but the sequence of meaningful changes in understanding that produced it.