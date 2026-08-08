# Scientific Research Harness — Founding Design Reflection

**Status:** living design reflection / origin record  
**Purpose:** preserve why this project exists, what problem it is trying to solve, which ideas led to it, what it must not become, and which design directions are intentionally still open.

This file is deliberately **not** the normative harness specification. It is a design-memory artifact. Future `README`, `SKILL.md`, schemas, gates, reusable CI workflows and APIs should be traceable back to this reflection, but may simplify or restructure it. If the project later changes direction, this document should not be silently rewritten to make the final design look inevitable. Important changes should be recorded through the project chronicle or linked design-decision records.

---

## 1. Where the idea came from

The harness did not start as an abstract attempt to create a new research methodology.

It emerged while running and reviewing a real experimental programme around supervised biometric embedding compression. During that work, several things repeatedly proved valuable but were easy to lose if they remained only in chat, pull-request discussions or the researcher's memory:

- explicit claims and bounded wording;
- preregistered estimands and decision rules;
- negative results that remained visible rather than being polished away;
- reviewer objections that materially changed the work;
- errata linked to the original evidence rather than replacing it;
- exact data-source versions and hashes;
- immutable study snapshots and archived PDFs;
- replay manifests, seeds, environments and generated figures;
- hard CI gates that prevented prose, code and evidence from silently diverging;
- distinctions between scientific evidence, implementation assurance and engineering feasibility;
- doubts discovered during implementation, including computational cost, sparse-data structure and invalid bootstrap assumptions;
- decisions made **before** outcome-bearing evidence versus changes made **after** looking at results;
- explanations that helped the researcher understand *why* a method worked or failed, even when those explanations did not belong in the final paper.

The key realization was that much of the value of a rigorous research process lives **between** the formal protocol and the final result.

A conventional publication often compresses the trajectory into:

> question → method → result → conclusion

while the actual scientific learning process may have been closer to:

> intuition → prior → assumption → design → doubt → reviewer challenge → failed gate → implementation discovery → erratum → revised hypothesis → new experiment → negative result → belief update → next question

The harness exists to make more of that trajectory inspectable, reproducible and teachable without confusing reasoning provenance with empirical evidence.

---

## 2. The central idea

The project should become a **reusable scientific workflow harness** that helps humans and AI assistants conduct experimental work while preserving:

1. **scientific discipline** — falsifiable claims, preregistration, correct inferential units, uncertainty, negative results and explicit gates;
2. **reproducibility** — versioned inputs, environments, seeds, hashes, immutable evidence, replay and intermediate versions;
3. **reasoning provenance** — doubts, assumptions, alternatives, reviewer challenges, cost constraints, belief changes and unresolved questions;
4. **pedagogy** — progressive explanations, toys, visualizations, interactive replays and learning artifacts that remain bound to authoritative evidence;
5. **shareability** — a structure that scientists, engineers, students and reviewers can clone, challenge, adapt or reject in parts.

The harness is not intended to certify that research is scientifically valid merely because the process was followed. It should instead make it **harder to silently lose, rewrite or overstate the evidence and reasoning that led to a claim**.

---

## 3. Why pedagogy is first-class, not publication garnish

A major design requirement is that pedagogy must not be treated as a final communication step such as a LinkedIn post, carousel, paper figure or summary.

The researcher using the harness may also be an **apprentice scientist**. Conceptual understanding evolves during the study, and some of the most valuable learning events occur when:

- an assumption fails;
- a reviewer exposes a hidden dependency;
- an implementation reveals an unexpected computational cost;
- a statistical unit is discovered to be wrong;
- two apparently similar metrics are shown to answer different questions;
- a toy example suddenly makes a formal equation intuitive;
- a failed result changes the researcher's prior belief.

These moments deserve explicit pedagogical records.

A pedagogy entry may be created:

- on request from the learner/researcher;
- by a teacher, reviewer or collaborator;
- by an AI assistant suggesting that a difficult or important concept should be captured;
- at study closure when a full learning/replay pack is generated.

Pedagogical artifacts must remain **derived views of the authoritative research object**. They must not become an independent source of facts.

---

## 4. Diderot-style progressive exposition

One useful design metaphor is **Diderot**: the same scientific object should be explainable progressively at different depths without creating contradictory versions of the truth.

For example, a single methodological object could expose:

1. a one-sentence intuition;
2. a visual diagram;
3. a toy example;
4. the mathematical definition;
5. the implementation contract;
6. the replay path;
7. the reviewer challenge;
8. the final decision and residual uncertainty.

Conceptually:

```text
                    AUTHORITATIVE OBJECT
                           |
          +----------------+----------------+
          |                |                |
          v                v                v
      intuition         mathematics        replay
          |                |                |
          v                v                v
       diagram          derivation       executable
          |                                 |
          +--------- reviewer challenge ----+
                            |
                            v
                         decision
```

The important principle is:

> These are not several versions of the science. They are several views of the same versioned scientific object.

---

## 5. Fabric-style composable learning artifacts

A complementary idea is to make pedagogical and reasoning artifacts composable rather than monolithic.

Possible reusable objects include:

- `ClaimCard`
- `HypothesisCard`
- `PriorCard`
- `MechanismCard`
- `EstimandCard`
- `AssumptionCard`
- `EvidenceCard`
- `FailureCard`
- `ReviewerChallengeCard`
- `ChronicleCard`
- `DecisionCard`
- `OpenQuestionCard`
- `ToyCard`
- `ReplayCard`

A paper, notebook, lesson, interactive page or social post could compose these same objects for different audiences.

For example:

```text
ClaimCard
   ↓
MechanismCard
   ↓
EstimandCard
   ↓
ToyCard
   ↓
ReviewerChallengeCard
   ↓
FailureCard
   ↓
ChronicleCard
   ↓
ReplayCard
   ↓
DecisionCard
   ↓
OpenQuestionCard
```

This reduces the risk that the paper tells one story, the notebook another, and the educational material invents a third simplified truth.

---

## 6. Three planes that should remain distinct

A core architectural idea is to separate at least three planes.

### 6.1 Scientific evidence plane

```text
claim / hypothesis
    ↓
preregistration
    ↓
experiment
    ↓
immutable evidence
    ↓
replay
    ↓
claim-admissibility gate
```

This is where empirical support lives.

### 6.2 Scientific chronicle plane

```text
priors
  ↓
doubts
  ↓
alternatives
  ↓
reviewer challenges
  ↓
decisions
  ↓
failures
  ↓
belief updates
  ↓
residual uncertainty
```

This preserves the evolution of reasoning and context.

### 6.3 Pedagogical plane

```text
intuition
  ↓
visualization
  ↓
toy
  ↓
mathematics
  ↓
implementation
  ↓
full replay
  ↓
reflection
```

This supports understanding and teaching.

These planes should cross-link strongly, but **none may substitute for another**.

A beautiful explanation does not create evidence.  
A complete chronicle does not repair a flawed experiment.  
A perfectly replayable run does not automatically make a scientific claim admissible.

---

## 7. Scientific chronicle

The harness should preserve reasoning events that ordinary changelogs do not capture well.

Examples of events worth chronicling:

- a new scientific or engineering doubt;
- an assumption added, weakened or falsified;
- a reviewer finding with material consequences;
- an infeasible runtime or memory plan;
- a data-source or representativity concern;
- a seriously considered alternative that was rejected;
- a methodological change after outcome inspection;
- a failed gate;
- a negative or surprising result;
- an unavailable piece of evidence;
- a change of belief;
- an important pedagogical clarification.

Every entry should distinguish whether **outcome-bearing evidence had already been seen**.

This distinction matters because an optimization or methodological change made before seeing outcomes has a different epistemic status from one made after observing the result.

The chronicle should be append-oriented: wrong entries may be superseded or corrected, but the original reasoning should remain reconstructible.

Git history improves traceability but should not be described as cryptographic non-repudiation of identity. Stronger provenance would require mechanisms such as signed commits/tags, independent archives or transparency logs.

---

## 8. Computational feasibility is part of the scientific story

One of the immediate triggers for this harness was a concrete implementation concern.

A preregistered coverage simulation was scientifically well specified but potentially computationally prohibitive. The reviewed threshold implementation contained a loop over unique impostor distances and repeatedly summed the full weighted array, producing approximately quadratic behavior in the number of distinct distances.

This observation did **not** invalidate the statistical design, but it created a real scientific-engineering question:

> Can the frozen experiment actually be executed at its preregistered scale without changing its semantics?

The correct response was not to quietly reduce bootstrap counts or scenarios.

Instead, the concern should be recorded **before production outcome inspection**, and resolution should be one of:

- benchmark the reviewed implementation and demonstrate feasibility; or
- optimize the implementation while proving semantic equivalence to the reviewed reference algorithm.

For example, a threshold algorithm may be optimized from repeated full sums to sorting plus cumulative weights while preserving:

- the same sampling unit;
- the same random-number semantics when frozen;
- the same whole-tie-block rule;
- the same sentinel rule;
- the same estimand;
- the same confidence interval;
- the same stopping rule;
- the same scenarios;
- the same gate thresholds.

This episode is a model for the harness: **implementation doubts, cost questions and alternatives can carry scientific value and should not disappear from the final history**.

---

## 9. Scientific Decision Records (SDRs)

The chronicle preserves a temporal stream, but some decisions deserve a compact crystallized record analogous to Architecture Decision Records.

A possible `SDR — Scientific Decision Record` could capture:

```yaml
decision_id: DEC-STAT-004

question:
  How should dependence be represented in uncertainty estimation?

alternatives:
  - pair bootstrap
  - complete identity-matrix bootstrap
  - sparse subject-slot weighted bootstrap

chosen:
  sparse subject-slot weighted bootstrap

why:
  - preserves repeated sampled identities
  - preserves the observed sparse graph
  - does not invent missing comparisons

rejected:
  pair_bootstrap:
    reason: wrong inferential unit
  complete_matrix:
    reason: synthesizes unobserved edges

chronicle_refs:
  - CHRON-...

review_refs:
  - PR-...
```

This could be especially useful for peer review and teaching because it separates **what was decided** from the chronological detail of how the discussion unfolded.

---

## 10. Belief updates as first-class objects

Another valuable layer is explicit epistemic change.

A study may begin with a prior expectation, encounter evidence, receive a challenge and end with a different belief even when no headline claim is proven.

A useful structure is:

```text
Prior
  ↓
Observation
  ↓
Challenge
  ↓
Posterior / revised belief
  ↓
Decision
  ↓
Residual uncertainty
```

A future harness could require any material belief update to reference the evidence that motivated it.

This is not intended to force subjective Bayesian formalism everywhere. The goal is simply to make significant changes in scientific expectation visible rather than retrospectively rewriting the original intuition.

---

## 11. Study-closure Learning & Replay Pack

At the end of a study, the harness should be able to generate or propose a complete **Learning & Replay Pack**.

Possible structure:

```text
release/
├── paper.pdf
├── evidence-manifest.json
├── replay.zip
├── chronicle-snapshot.yaml
├── claim-decision-summary.yaml
│
└── learning-pack/
    ├── README.md
    ├── 01_intuition.md
    ├── 02_visual_explanation.*
    ├── 03_mathematics.md
    ├── 04_toy_experiment.ipynb
    ├── 05_full_replay.ipynb
    ├── 06_reviewer_challenges.md
    ├── 07_failures_and_errata.md
    ├── 08_what_changed_my_mind.md
    └── learning_manifest.json
```

The **toy** and the **full replay** must be clearly distinguished.

- A toy is deliberately simplified for understanding.
- A full replay is intended to reproduce the actual experiment faithfully.

Any pedagogical simplification that could change interpretation should be declared explicitly.

---

## 12. What a reusable repository should provide

The long-term repository should be usable in several modes rather than as one monolithic framework.

### 12.1 Repository template

A scientist or student can start a new study with a ready structure for claims, protocols, evidence, chronicle, pedagogy and CI.

### 12.2 CLI / package

A local validator can check consistency, provenance and named execution blockers.

### 12.3 Reusable GitHub workflows

Projects can import research-assurance gates without copying all CI logic.

### 12.4 LLM skill / instruction layer

An AI assistant can be instructed on **how to behave scientifically inside the project**, including what it must never silently rewrite or overclaim.

### 12.5 Examples and counterexamples

The repository should include small worked examples such as:

- minimal reproducible study;
- negative result;
- statistical erratum;
- computational-feasibility blocker;
- post-outcome amendment;
- pedagogy toy versus full replay;
- intentionally bad example that the harness rejects.

---

## 13. Beyond `SKILL.md`: a discipline runtime for AI-assisted research

A `SKILL.md` can provide behavioral rules to an LLM, but the project should go beyond prompt instructions by making important constraints executable.

Example behavioral expectations:

- identify whether a change is pre-outcome or post-outcome;
- never overwrite failed evidence;
- preserve negative results and failed gates;
- record material doubts before silently resolving them;
- distinguish missing evidence from permission;
- distinguish scientific changes from semantics-preserving engineering changes;
- suggest a pedagogy entry when an important conceptual difficulty or belief update appears;
- never treat pedagogical output as independent scientific evidence;
- reconstruct claim → evidence → gate → decision before drafting a publication;
- at study closure, propose both a toy and a faithful replay learning artifact.

But these rules should also be reinforced by schemas, validators, CI and preflight gates so that rigor does not depend only on the model remembering instructions.

---

## 14. Reverse-engineered mechanisms worth preserving from the first real project

The original biometric research project already demonstrated several mechanisms that should inform this generic harness.

### Version and evidence discipline

- immutable executed-study identifiers;
- stable versioned paths for historical PDFs;
- archived old outputs rather than replacing them;
- linked errata instead of silent corrections;
- changelog as append-oriented research history;
- experiment ledger independent of prose narrative.

### Source and data provenance

- exact dataset version/handle;
- SHA-256 validation before accepting rematerialized sources;
- blocking mismatched mirrors instead of accepting semantic similarity;
- versioned pseudonymized subject maps while sensitive source identity files remain outside Git;
- explicit record of whether historical outcome-bearing scores were read.

### Replay and rendering

- run manifest;
- resolved configuration;
- seeds;
- environment lock;
- event/audit streams;
- complete evidence tables;
- figures regenerated from evidence rather than hand-edited;
- figure manifest binding outputs to source hashes;
- CI failing if regenerated artifacts differ.

### Scientific gates

- claims registry;
- permitted and forbidden wording;
- explicit gate statuses;
- `INDETERMINATE` or failure remaining visible when evidence is missing;
- smoke execution unable to become scientific evidence;
- unexecuted studies unable to contain result fields;
- Study 1 remaining blocked while Study 0 statistical correction is unresolved.

### Implementation assurance

- reference algorithm reviewed before optimization;
- tests for tie rules, sentinels and degenerate cases;
- structured audit for failed replicates;
- no silent redraw of degenerate replicates;
- preservation of completed work before a blocking failure;
- production execution distinct from smoke/benchmark modes.

### README usability

- quick links to current PDF, archived PDF, errata, results, protocol, experiment ledger and claims;
- CI / research-assurance badges;
- explicit current scientific status near the top rather than hidden in release notes;
- quick-start replay instructions;
- explanations of what a successful smoke replay does **not** prove.

These mechanisms should be generalized rather than copied blindly.

---

## 15. Candidate generic repository structure

A possible target structure is:

```text
scientific-research-harness/
│
├── README.md
├── SKILL.md
├── HARNESS_SPEC.md
├── DESIGN_HARNESS_REFLECTION.md
├── CHANGELOG.md
├── CITATION.cff
│
├── schemas/
│   ├── research_program.schema.json
│   ├── claim.schema.json
│   ├── chronicle_entry.schema.json
│   ├── pedagogy_entry.schema.json
│   ├── replay_manifest.schema.json
│   └── gate.schema.json
│
├── templates/
│   ├── research_program.yaml
│   ├── claims_registry.yaml
│   ├── scientific_chronicle.yaml
│   ├── experiment_ledger.yaml
│   ├── pedagogy_registry.yaml
│   ├── erratum.md
│   └── preregistration.md
│
├── harness/
│   ├── validate.py
│   ├── preflight.py
│   ├── provenance.py
│   └── pedagogy.py
│
├── gates/
│   ├── research_assurance.yaml
│   ├── chronicle.yaml
│   ├── replay.yaml
│   └── pedagogy.yaml
│
├── .github/workflows/
│   ├── reusable-research-assurance.yml
│   ├── reproducibility.yml
│   └── release-evidence.yml
│
├── pedagogy/
│   ├── PEDAGOGY_SPEC.md
│   ├── entry-template.yaml
│   ├── toy-template/
│   └── replay-template/
│
├── examples/
│   ├── minimal-study/
│   ├── negative-result/
│   ├── statistical-erratum/
│   ├── compute-feasibility/
│   └── reference-real-study/
│
└── docs/
    ├── CAL.md
    ├── MMALS_REPLAY.md
    ├── CHRONICLE.md
    ├── DIDEROT.md
    ├── SCIENTIFIC_DECISION_RECORDS.md
    └── ADOPTION_GUIDE.md
```

This is a design direction, not a frozen architecture.

---

## 16. Example pedagogy-entry contract

A pedagogy entry could eventually look like:

```yaml
id: PED-20260808-003
source_scope: study_0_subject_bootstrap_v0_2_2

trigger:
  type: learner_question
  origin: apprentice_scientist

learning_goal:
  understand why subject bootstrap differs from pair bootstrap

authoritative_refs:
  - protocol/studies/study_0_subject_bootstrap_spec.md
  - CHRON-20260808-001
  - tests/test_subject_bootstrap.py

levels:
  intuition: true
  visual: true
  mathematical: true
  implementation: true
  reviewer_view: true

misconceptions:
  - resampling pairs is equivalent to resampling identities
  - repeated subject slots should be deduplicated

toy:
  status: PLANNED
  simplifications_must_be_declared: true

replay:
  status: PLANNED

outcome_evidence_created: false
```

The exact schema should emerge from several real uses rather than being over-designed immediately.

---

## 17. Scientific principles the harness should defend

At minimum, future implementations should preserve these principles:

- falsifiable, bounded claims rather than success-oriented narratives;
- negative results and failed gates remain visible;
- sampling unit matches the inferential unit;
- train/validation/test boundaries are explicit where applicable;
- data/source versions and hashes are recorded;
- degenerate observations are not silently redrawn;
- post-outcome selection or methodological changes are disclosed;
- uncertainty procedures used for decisions are fixed before outcome inspection when possible;
- external validity is not inferred from benchmark validity;
- engineering feasibility and scientific evidence remain distinct evidence planes;
- reviewer criticism is not erased from the final history;
- reproducibility includes both executable evidence and decision context;
- intermediary published versions, PDFs and manifests are preserved when they matter;
- process compliance can never substitute for empirical evidence.

---

## 18. The major risk: process theatre

The harness itself needs to be falsifiable and challengeable.

A serious danger is creating an elegant bureaucracy that produces many YAML files but little epistemic value.

The project should therefore continuously ask:

- Which artifact actually helps reconstruct or challenge a scientific decision?
- Which artifact merely duplicates information?
- Which gates prevent real errors?
- Which gates only create ceremony?
- Does the chronicle preserve useful uncertainty, or encourage retrospective storytelling?
- Are students learning science, or learning how to fill templates?
- Does the LLM become more rigorous, or merely more verbose?
- Can another researcher reproduce the claim/evidence chain without the original conversation?
- Can a skeptical reviewer identify exactly what the harness does **not** guarantee?

Anything that repeatedly adds no epistemic, reproducibility or pedagogical value should be removed.

---

## 19. `CHALLENGE_THE_HARNESS.md` should exist

The project should explicitly invite criticism rather than presenting itself as a finished methodology.

Possible challenge questions:

- What is genuinely reproducible here?
- Which artifact creates knowledge and which creates bureaucracy?
- Can a chronicle itself introduce retrospective narrative bias?
- Which reasoning events are important enough to record?
- When does a gate protect scientific integrity, and when does it block useful exploration?
- How can a semantics-preserving optimization be distinguished from a methodological change?
- Does AI assistance improve or weaken provenance?
- Which guarantees are not provided by Git?
- Can an independent student reproduce not just a result but the logic behind the major decisions?
- Can an external scientist challenge the framework without adopting its terminology first?

The harness should be presented as an experiment that is itself open to failure and revision.

---

## 20. Positioning for external sharing

The project should **not** initially be presented as a new scientific method or a certification framework.

A more defensible description is:

> **An experimental harness for making research reasoning, evidence, failures, decisions and learning artifacts inspectable and replayable.**

The invitation should be explicit:

> Challenge the assumptions, identify process theatre, remove what adds no epistemic value, and propose counterexamples.

This positioning makes the repository suitable for discussion with:

- scientists;
- research engineers;
- QA/test and evidence-governance specialists;
- students and teachers;
- reproducibility practitioners;
- AI-assisted research communities.

---

## 21. Two complementary identities

The project may ultimately serve two related purposes.

### Research harness

Help make experimental work more falsifiable, traceable, replayable and explicit about its evidence boundaries.

### Apprentice-scientist harness

Expose how real scientific reasoning evolves through uncertainty, mistakes, critique, correction and learning.

The second purpose may be unusually valuable pedagogically.

Most educational material shows a cleaned-up route from question to conclusion. This harness could preserve the much more instructive path:

```text
intuition
  ↓
prior
  ↓
doubt
  ↓
design
  ↓
critique
  ↓
error
  ↓
erratum
  ↓
new hypothesis
  ↓
decision
  ↓
evidence
  ↓
negative result
  ↓
revision
  ↓
next question
```

The process itself becomes a learning artifact without turning the learner's reflections into scientific evidence.

---

## 22. Development strategy: extract from real studies, do not over-framework early

The generic harness should evolve from real consumers.

The first biometric compression programme can serve as **reference consumer #1**. Rather than immediately abstracting every detail, the recommended path is:

1. identify mechanisms that proved useful in the real study;
2. extract only those that can be generalized cleanly;
3. create a minimal generic implementation;
4. apply it to a second, substantially different study;
5. identify what was accidentally biometric-specific;
6. simplify aggressively;
7. only then stabilize schemas and reusable CI interfaces.

This is intended to prevent the repository from becoming a sophisticated framework that has only ever validated itself against the project that created it.

---

## 23. Open design questions

The following questions are intentionally unresolved:

- What is the minimum viable chronicle entry?
- Should Scientific Decision Records be mandatory or optional?
- Which pedagogy entries should be manually requested versus automatically suggested?
- How should Diderot-style progressive exposition be rendered: Markdown, notebooks, static site, interactive app, or several adapters?
- What exactly should “Fabric” mean here: composable files, typed cards, UI components, LLM patterns, or all of these?
- Should the harness define CAL/MMALS interfaces directly or only integration contracts?
- Which provenance guarantees should be core versus optional hardened profiles?
- How should signed releases and external archival be supported?
- Can the harness remain useful outside software-heavy computational science?
- How should qualitative research or hardware experiments adapt the same principles?
- What is the right balance between machine-readable structure and researcher freedom?
- How can the harness detect process theatre rather than creating it?
- How should an AI assistant decide when a doubt is material enough to chronicle?
- Should a study-closure learning pack be recommended, gated or entirely optional?
- How can pedagogical toys be automatically checked against authoritative facts?

These are research questions about the harness itself.

---

## 24. Initial success criteria for the harness project

The first meaningful success should **not** be the number of templates or stars on GitHub.

More useful criteria would be:

- a second researcher can understand why a major decision was made without reading private chat history;
- a reviewer can reconstruct claim → protocol → evidence → gate → decision;
- a failed experiment remains as easy to inspect as a successful one;
- a student can move from intuitive explanation to mathematical definition to toy to full replay;
- an AI assistant can contribute without silently rewriting evidence or history;
- an implementation optimization can be demonstrated to preserve frozen scientific semantics;
- intermediate versions remain recoverable through stable artifacts and hashes;
- external users can identify unnecessary process and successfully remove it;
- at least one substantially different research project can adopt the harness without forcing its science into the original project's shape.

---

## 25. Founding intent

The project's founding intent can be summarized as follows:

> Preserve not only the final scientific result, but enough of the evidence chain, reasoning history, failures, decisions and learning path that another person can inspect what happened, challenge it, replay what is replayable, understand what changed, and learn from the process — without confusing documentation, pedagogy or AI assistance with scientific evidence.

This intent should remain more important than any particular file structure, acronym or implementation technology.
