# SDR-004 — Canonical status requires accountable human understanding

Status: proposed
Date: 2026-08-21

## Decision

The harness adopts the following normative axiom:

> **A claim cannot reach canonical status solely because an automated verifier accepted it. At least one accountable human must be capable of explaining the result, its evidence, limitations, provenance and significance.**

Automated verification, reproducibility, statistical checks, formal proof, CI success, reviewer agents, and other machine gates may establish important evidence states. They do not, by themselves, establish canonical scientific or engineering knowledge.

A project may therefore distinguish at least these states:

1. `generated` — an artifact or candidate claim exists;
2. `verified` — bounded checks support the stated claim;
3. `understood` — an accountable human can reconstruct and explain the claim and its evidential basis;
4. `digested` — the result has been related to alternatives, prior work, failures, limitations, and neighboring knowledge;
5. `canonicalization_ready` — the result is suitable for durable teaching/reference material;
6. `canonical` — the adopting knowledge system has accepted it into its durable reference layer.

Projects MAY use different labels, but they MUST preserve the semantic distinction between automated acceptance and accountable human understanding whenever they claim canonical status.

## Motivation

The harness already separates evidence, chronicle, and pedagogy. This SDR makes explicit why pedagogy is not merely post-processing or communication polish: the ability to explain a result is part of research closure.

The immediate source is Terence Tao, *Mathematics in the Age of AI*, arXiv:2608.16753v1 (2026). Tao deliberately conditions on the hypothesis that AI systems will become capable of a substantial amount of research-level mathematics, and asks what mathematical research is actually trying to optimize. He decomposes problem solving from an apparently simple objective into a pipeline involving generation, verification, exposition, publication/acceptance, digestion, and canonicalization. He also argues that a formally verified proof that no accountable human can properly explain should still be viewed as incomplete.

The harness generalizes this insight beyond mathematics. Its domain is scientific and engineering work assisted by LLMs and agents, where artifact abundance may similarly move the bottleneck from production toward verification, interpretation, review, digestion, and durable knowledge formation.

## Source-derived observations retained by this harness

The following observations are retained as design inputs, not as domain-independent theorems:

- **Artifact abundance can create evidence indigestion.** AI can increase the rate of candidate proofs, experiments, analyses, code, reports, and claims faster than expert attention can verify and assimilate them.
- **Verification is necessary but not sufficient for knowledge.** Correctness or successful automated checks do not guarantee understanding, explanatory adequacy, attribution, significance, or integration with prior knowledge.
- **Canonicalization is a distinct and slower activity.** Durable knowledge requires restating a result at the right level of generality, connecting it to neighboring knowledge, and making it teachable and reusable.
- **Goodhart risk increases under automation.** Easily measurable proxies such as problem count, benchmark score, coverage, number of experiments, number of generated artifacts, or green checks can diverge from the underlying objective when aggressively optimized.
- **Natural friction can carry information.** Failed hypotheses, awkward steps, counterexamples, changes of notation, ablations, and other difficult parts of a reasoning path may identify where the intellectual content really lies. A polished final artifact must not erase these signals from the chronicle.
- **Human attention becomes a scarce validation resource.** Agentic workflows should help allocate expert review, not pretend to eliminate the need for accountable expert judgment in canonicalization.

## Harness consequences

### 1. Pedagogy is a closure obligation

A project MUST NOT treat pedagogical explanation as optional presentation work when the project claims that a result is closed, understood, canonicalization-ready, or canonical.

The accountable human should be able to explain, at a level appropriate to the claim:

- the question or engineering decision;
- assumptions and scope;
- the result itself;
- the evidence and verification path;
- provenance and source authority;
- important failed attempts, counterexamples, or ablations where they materially affect understanding;
- uncertainty and limitations;
- why the result matters and what it does not establish.

This is an understanding gate, not a mechanism for changing scientific evidence.

### 2. Automated gates cannot self-promote to canonical status

An agent, proof assistant, validator, CI workflow, statistical test, model evaluator, or automated reviewer MUST NOT promote a claim to canonical status solely from machine-readable gate success.

Automation MAY mark evidence states such as `verified`, `reproduced`, or `all_checks_passed`. Promotion beyond those states requires the adopting project's explicit human-accountability rule.

### 3. Chronicle preserves epistemically useful friction

The chronicle SHOULD retain material failed hypotheses, counterexamples, reversals, ablations, unexpected observations, and reasoning changes rather than replacing the whole path with a polished success narrative.

The purpose is not to archive every intermediate token or chain of thought. The purpose is to preserve externally inspectable events that materially changed what was believed, tested, rejected, or done next.

### 4. Canonicalization is downstream of the harness

The harness governs how work becomes eligible for durable knowledge. A Diderot-like encyclopedia, textbook, standard, reference repository, or other curated system may perform the final canonicalization step.

The harness MUST NOT equate `canonicalization_ready` with acceptance by such a downstream authority.

### 5. LLM- and agent-assisted engineering inherits the same rule

The axiom applies equally to engineering governance. An LLM-generated architecture, test strategy, IaC stack, risk assessment, qualification report, change proposal, or release recommendation cannot become an organizational reference merely because automated validators or reviewer agents accepted it.

At least one accountable human must be able to explain the output, the authoritative inputs used, its evidence, limitations, provenance, operational implications, and significance for the decision being governed.

## Non-goals

This SDR does not require a human to reproduce every machine computation manually. It does not reject proof assistants, formal verification, statistical automation, LLMs, or agents. It also does not require publication or community consensus for every local engineering result.

It requires that the boundary between **machine-verified artifact** and **accountably understood knowledge** remain explicit.

## Practical gate template

A project may implement the following gate:

```yaml
canonicalization_gate:
  machine_verification_complete: true
  accountable_human: <name-or-role>
  can_explain:
    result: true
    evidence: true
    limitations: true
    provenance: true
    significance: true
  material_failed_paths_recorded: true
  status: canonicalization_ready
```

The exact schema is local. The semantic requirement is normative.

## Reference

Terence Tao, *Mathematics in the Age of AI*, arXiv:2608.16753v1, 2026. In particular, the paper's discussion of goals and Goodhart effects, the iterative problem-solving pipeline, proof abundance, human comprehension, and canonicalization motivated this design record.
