# Source Note — Terence Tao, *Mathematics in the Age of AI* (2026)

Status: **reviewed source capture for harness design**

## Bibliographic record

- **Author:** Terence Tao
- **Title:** *Mathematics in the Age of AI*
- **Identifier:** arXiv:2608.16753v1 `[math.HO]`
- **arXiv posting shown in the supplied paper:** 17 August 2026
- **Manuscript date shown in the paper:** 18 August 2026
- **Origin:** essay based on a public lecture delivered at the 2026 International Congress of Mathematicians
- **Supplied artifact:** `2608.16753v1.pdf`
- **SHA-256 of supplied artifact:** `ea1d16a81ec79f362914f85b832996e3eb8c3b81b1c983358fc6d76ac7420ee0`

This note preserves the source separately from the harness rules derived from it. The observations below are paraphrases of the paper, not quotations.

## Source-derived observations

### 1. Condition on strong AI capability, then inspect goals and values

The paper deliberately does not try to settle how capable research-level mathematical AI will become. It conditions on a reasonably strong AI-capability hypothesis and asks the orthogonal question: what goals, values, practices and forms of contribution the mathematical community actually wants to preserve or optimize.

Relevant sections: 3–5.

### 2. Proxy goals can diverge under optimization

Tao argues that mathematical goals historically correlated well enough that a small number of visible outputs could act as usable proxies for broader progress. Under strong optimization, especially with generative AI and benchmark incentives, Goodhart effects can make these proxies diverge from the underlying values.

Harness-relevant examples include treating counts of solved problems, artifacts, experiments, tests, coverage, or benchmark successes as if they were equivalent to understanding or durable knowledge.

Relevant section: 5.

### 3. Problem solving expands into a multi-stage knowledge pipeline

The paper iteratively refines the apparently simple goal “solve unsolved problems” into a pipeline that includes:

`generation -> verification -> exposition -> publication / acceptance -> digestion -> canonicalization`

The key observation is that correctness alone is not the end state. A result must also be communicated, absorbed, connected to neighboring knowledge and, at the slowest stage, incorporated into the field’s durable toolkit.

Relevant sections: 6 and Figure 4 in section 6.

### 4. AI can move the bottleneck downstream

Under the working hypothesis, proof production can become abundant. The limiting resource then shifts toward verification, exposition, expert review, digestion and canonicalization. Tao describes this as a transition from proof scarcity to proof abundance and warns of “indigestion” when downstream human processes cannot absorb generated material fast enough.

Relevant section: 7.

### 5. Automated filtering is useful but is not community acceptance

The paper allows a useful role for AI evaluation and triage, for example detecting inadequate verification, missing attribution or incoherent exposition. It nevertheless distinguishes such automated filtering from human refereeing and community acceptance, which remain separate processes.

Relevant section: 6, especially the discussion following Goal 6.4.

### 6. Human explainability is part of completion

Tao proposes a rule of thumb that authors should be able to give a clear, expert-level presentation of their own result, including correctness and attribution. He argues that a formally verified proof that no human can properly explain should still be considered incomplete.

Relevant section: 8.

### 7. “Natural friction” can carry tacit knowledge

The paper notes that human mathematical writing often preserves traces of where the real difficulty was: unusually careful lemmas, changes of notation, rewritten passages, false starts and other forms of intellectual friction. Excessive AI polishing can erase these signals and make a text easy to read but harder to learn from.

For the harness this supports preserving material reasoning turns, failed hypotheses, corrections, counterexamples and decision changes in the Chronicle instead of retaining only a polished final result.

Relevant section: 6.

### 8. Tool use, attribution and review burden must remain visible

The paper endorses disclosure of automated tool use, accurate attribution, and practices that make expert review easier rather than merely increasing submission throughput.

Relevant section: 8 and the Leiden Declaration recommendations discussed there.

## Harness interpretation and adaptation

The following are **harness design inferences**, not claims that Tao formulates in this exact language.

1. **Automated verification is evidence, not canonical authority.**
2. **Scientific validity and canonical status are distinct.** A result may be valid yet not ready to become durable reference knowledge.
3. **Canonical promotion requires accountable human comprehension.** At least one accountable human must be able to explain the result, evidence, limitations, provenance and significance.
4. **Chronicle is epistemic infrastructure.** Material failed paths and reasoning turns can preserve information needed for later understanding.
5. **The optimization target is not artifact throughput.** The harness should increase the rate at which trustworthy, understood and reusable knowledge is produced.
6. **Agentic production shifts the bottleneck downstream.** As generation becomes cheap, review, digestion, prioritization and canonicalization become scarce resources that must be explicitly governed.
7. **The same pattern applies beyond mathematics.** In LLM-assisted science and engineering, generated code, architectures, test plans, requirements, infrastructure, analyses or decisions should not become canonical merely because automated checks pass.

The normative companion rule is defined in `design/HUMAN_COMPREHENSION_CANONICALIZATION.md` and surfaced in `HARNESS.md`.

## Source boundary

This paper is a conceptual and methodological source, not empirical evidence that any particular harness implementation improves research quality. Its value here is to motivate and challenge the design of downstream verification, explanation, digestion and canonicalization processes.