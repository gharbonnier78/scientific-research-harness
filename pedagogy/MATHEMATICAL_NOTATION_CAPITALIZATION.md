# Mathematical Notation Capitalization Contract

Status: **draft reusable harness specification**

## Purpose

Mathematical notation is part of the knowledge being learned, not presentation syntax to be
left implicit. This contract defines how a consuming project should capture, explain,
re-encounter, review and publish mathematical notation when the harness is used for
learning-oriented scientific or engineering work.

It complements `PEDAGOGICAL_CONCEPT_CONTRACT.md`. The concept contract explains a difficult
idea; this contract makes the mathematical language used to express that idea durable and
searchable.

The generic method belongs in the harness. Concrete notation entries and their learning
history belong in a consumer knowledge base such as Diderot.

## Trigger

When learning-oriented work introduces a non-trivial mathematical symbol, operator, map,
space, indexed object, decorated variable, relation or notation convention, the workstream
SHOULD check the consumer notation registry before treating the notation as understood.

Examples include `\forall`, `\exists!`, `\in`, `\subseteq`, `\times`, `\cap`, `f:E\to F`,
`x\mapsto f(x)`, `\Gamma_f`, `f_{\mid E'}`, tangent-space notation, probability notation,
losses, estimators and domain-specific operators.

Trivial repetitions do not require a new entry. A re-encounter that adds a new meaning,
domain, ambiguity, derivation or useful example SHOULD update the existing concept instead.

## Canonicalization rule: concept first, glyph second

An agent MUST NOT assume that one visual symbol denotes one mathematical concept.

- One concept may have several conventional notations.
- One symbol may be overloaded across domains or even within one source.
- A notation may depend on local conventions, indices, superscripts, typography or context.

Before creating a new entry, search by concept name, aliases, semantic meaning and known
notations. If an existing concept matches, append the new encounter or alias rather than
silently creating a duplicate.

When a glyph is overloaded, the registry MUST preserve the disambiguating context. For
example, `\times` may denote ordinary multiplication, a Cartesian product or another product
whose meaning is supplied by the surrounding objects.

## Minimum notation record

A non-trivial notation entry SHOULD make the following recoverable:

1. **Notation** — canonical LaTeX plus accepted aliases or variants.
2. **Read aloud** — how to say it in the learner's working language; add other languages when useful.
3. **Concept and type** — what mathematical object or operation it denotes.
4. **Formal meaning** — signature, assumptions or defining relation when applicable.
5. **Plain-language meaning** — one sentence that can be understood without decoding the symbols.
6. **Why it appears here** — the role it plays in the current derivation, model or argument.
7. **Minimal example** — a small valid instance that can be checked by hand.
8. **Misconception / non-example** — when a plausible reading is wrong or context-sensitive.
9. **Prerequisites** — concepts needed to understand the notation rather than merely pronounce it.
10. **Encounters** — append-only records of where the notation was met and what new role it acquired.
11. **Connections** — related concepts, notations and mathematical domains.
12. **Authority / provenance** — the source that authorizes the mathematical meaning and the separate provenance of any pedagogical explanation.
13. **Maturity** — how far the entry has progressed from capture to reviewed cross-domain understanding.

A compact machine-readable shape is recommended:

```yaml
id: function-restriction
status: draft | reviewed | stable
maturity: L0 | L1 | L2 | L3 | L4 | L5

notation:
  latex: "f_{\\mid E'}"
  aliases: []
  spoken:
    fr: "f restreinte à E prime"
    en: "f restricted to E prime"

concept:
  name: "restriction of a function"
  category: functions
  prerequisites:
    - function
    - subset
    - domain

formal:
  assumptions:
    - "f:E\\to F"
    - "E'\\subseteq E"
  meaning: "f_{\\mid E'}:E'\\to F and f_{\\mid E'}(x)=f(x) for x in E'"

plain_language:
  summary: "Same rule, smaller allowed input domain."
  why_here: "Study only the part of a function whose inputs lie in E'."

example:
  statement: "f(x)=x^2, E'=[-1,1]"
  explanation: "Keep x^2 unchanged, but only for -1 <= x <= 1."

misconceptions:
  - wrong: "Restriction approximates or changes the function on E'."
    correction: "Restriction changes the domain; the values on retained inputs are unchanged."

encounters:
  - source_ref: "<recoverable source>"
    context: "<section, exercise, project or derivation>"
    contribution: "<what this encounter added>"

connections:
  concepts:
    - function-graph
    - local-analysis
  domains:
    - analysis
    - topology

authority:
  mathematical_sources: []
  pedagogical_sources: []
  scientific_authority: false
```

## Read-aloud rule

If a learner can manipulate an expression only by sight but cannot read it as a sentence,
the notation has not yet been fully externalized pedagogically.

For a new non-trivial notation, the consumer SHOULD record both:

- a near-literal reading, useful for parsing the expression; and
- a natural-language reading, useful for understanding the proposition.

Example:

```text
∀ x ∈ E, ∃! y ∈ F : (x,y) ∈ Γ

literal: "for every x belonging to E, there exists a unique y belonging to F such that (x,y) belongs to Gamma"
natural: "each input x in E is associated with exactly one output y in F"
```

Pronunciation is explanatory metadata. It does not determine formal meaning.

## Encounter history is append-only

A notation entry SHOULD preserve meaningful re-encounters rather than overwrite the first
learning context. A later encounter may add a new domain, reveal overloading, correct a
misconception or provide a better example.

The encounter history is not a raw chat transcript. Retain only the information needed to
reconstruct the learning step and its provenance.

## Maturity levels

A consumer MAY use the following maturity scale:

| Level | Meaning |
|---|---|
| L0 — Captured | notation, reading and first meaning recorded |
| L1 — Understood | intuition and a hand-checkable example added |
| L2 — Connected | prerequisites and related concepts linked |
| L3 — Re-encountered | multiple meaningful encounters retained |
| L4 — Cross-domain | distinct domain uses or overloads understood |
| L5 — Consolidated | provenance reviewed and the entry is suitable for stable publication/print |

Maturity is pedagogical metadata. It MUST NOT be reported as scientific evidence.

## Single-source publication rule

When a consumer publishes an interactive page, search index, knowledge graph, printable
poster, PDF appendix or other view of the notation atlas, those outputs SHOULD be derived
from one canonical notation registry rather than manually maintained as competing copies.

A printable representation MAY compress explanations, but it MUST preserve enough identity
to trace an item back to its canonical registry entry.

## Agent behavior

An LLM or agent MAY detect a new notation and propose a registry update automatically. It
MUST NOT silently promote an entry to reviewed/stable status merely because it generated the
explanation itself.

When the consumer uses an accountable-human canonicalization gate, the notation entry
inherits that gate before stable publication. A reviewer SHOULD check at least:

- formal correctness in the stated context;
- read-aloud fidelity;
- example validity;
- distinction between definition, convention, approximation and intuition;
- overloading/disambiguation;
- source provenance;
- absence of duplicate semantic entries.

## Authority boundary

The notation registry is a pedagogical index, not a new authority for mathematics. A
textbook, paper, standard or other authoritative source remains authoritative for the
meaning used in the workstream. Diderot or another consumer may explain and connect that
meaning, but it MUST distinguish its explanatory synthesis from source-derived claims.

## Consumer acceptance check

A consuming project claiming notation capitalization SHOULD be able to answer:

- Where is the canonical registry?
- Which agent-readable instruction triggers the check?
- How are new entries distinguished from re-encounters?
- Can a learner see how the notation is read aloud?
- Can every stable entry be traced to its source and at least one example?
- Are web/search/print views generated from the same registry?
- What review or human-comprehension step promotes an entry beyond draft?

The mechanism is successful when mathematical language becomes progressively easier to
read, connect and reuse without turning the notation atlas into an unreviewed dump of every
symbol ever displayed.