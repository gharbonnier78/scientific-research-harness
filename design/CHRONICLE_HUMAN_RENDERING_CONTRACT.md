# Chronicle Human Rendering Contract

Status: **draft normative companion specification**

## Purpose

Research projects often preserve scientific decisions correctly in append-only, machine-readable records while remaining difficult for a human reviewer to read as a continuous laboratory history. This contract defines a reusable two-layer pattern:

```text
append-only machine-readable Chronicle
              |
              v
 deterministic renderer
       /              \
      v                v
Chronicle.md      Chronicle.tex/pdf
```

The machine-readable Chronicle remains the source of authority. Human-readable renderings are derived artifacts and MUST NOT become an alternate editable history.

## Normative requirements

An adopting project that maintains append-only Chronicle records SHOULD provide a deterministic human-readable rendering when the number or complexity of entries makes direct review materially difficult.

The source Chronicle MUST remain append-only and machine-readable. YAML, JSON or an equivalent structured format MAY be used. One event per file is RECOMMENDED when it provides immutable provenance and simple review diffs.

A renderer MUST:

- consume the authoritative Chronicle records without mutating them;
- impose a deterministic ordering and state the ordering rule;
- preserve stable event identifiers, dates, statuses and source paths;
- expose the scientific question or decision, evidence basis, result, authorization boundary and next admissible action when those fields exist;
- distinguish scientific failure, engineering/infrastructure failure, diagnostic analysis and authorization decisions;
- preserve links or identifiers for runs, artifacts, commits and immutable evidence when present;
- mark absent fields as absent rather than infer or fabricate them;
- be reproducible from a pinned repository state.

A generated Markdown, LaTeX or PDF Chronicle MUST carry an explicit banner such as:

> Generated view. The append-only structured Chronicle is authoritative.

The rendered artifact MUST NOT be hand-edited and then treated as evidence. If explanatory prose is added manually, it MUST be stored separately or clearly marked as non-authoritative commentary.

## Scientific semantics

Rendering MUST NOT change estimands, thresholds, multiplicity rules, confidence levels, power targets, seeds, data boundaries, gate statuses or authorization state.

A renderer MAY improve presentation by grouping events into phases, producing tables, timelines or cross-references, provided the underlying structured values remain recoverable and the grouping does not imply a scientific conclusion absent from the source records.

When a Chronicle records a failed preregistered gate followed by post-failure diagnostics or amendments, the rendered history MUST preserve that temporal order and MUST NOT present a later diagnostic or amended result as if it had been preregistered originally.

## Build and CI

Projects SHOULD provide a reproducible command such as:

```bash
python scripts/render_chronicle.py --source protocol/chronicle --out-dir artifacts/chronicle
```

CI SHOULD verify that rendering succeeds. Projects MAY publish `Chronicle.md`, `Chronicle.tex`, `Chronicle.pdf` or equivalent generated artifacts. Generated binary PDFs SHOULD normally be CI artifacts unless repository policy explicitly versions binaries.

A renderer test SHOULD include at least:

- deterministic ordering;
- stable output from the same source tree;
- no mutation of source Chronicle files;
- preservation of representative scientific values and evidence identifiers;
- correct handling of missing optional fields.

## Handoff requirement

When a generated human Chronicle exists, handoff documentation SHOULD identify both:

1. the authoritative structured Chronicle location; and
2. the command or CI artifact used to obtain the human-readable view.

Reviewers MUST be told that the generated view aids navigation but does not supersede the structured evidence.

## Relationship to the main harness

This specification refines the `Chronicle and hand off` lifecycle obligation in `HARNESS.md`. It does not require every small project to generate a PDF. The requirement is proportional: human rendering is valuable when it materially reduces review burden without creating a second source of truth.
