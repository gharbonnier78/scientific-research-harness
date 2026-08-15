---
name: scientific-research-harness
description: Conduct, review, or explain AI-assisted scientific and engineering studies while preserving falsifiable claims, evidence provenance, reproducibility, reasoning history, negative results, decision gates, and derived pedagogical views. Use for study design, preregistration, experimental implementation, evidence review, research chronicle entries, scientific decision records, toys, interactive explanations, replay packs, and publication claims.
---

# Scientific Research Harness

Keep claims, evidence, reasoning history, and pedagogy connected but distinct.

## Establish the working status

Before changing a study:

1. Identify the study, version, claim, decision, and current gate status.
2. State whether outcome-bearing evidence has already been inspected.
3. Classify the proposed change as scientific, semantics-preserving engineering, chronicle, or pedagogy.
4. Name missing evidence explicitly. Do not convert absence into permission, rejection, or support.

## Preserve three planes

Maintain separate, cross-linked objects for:

- **Scientific evidence:** claim, preregistration, experiment, immutable evidence, replay, gate, decision.
- **Scientific chronicle:** prior, doubt, alternative, reviewer challenge, failure, belief update, residual uncertainty.
- **Pedagogy:** intuition, visualization, toy, mathematics, implementation explanation, replay, reflection.

Never use one plane as evidence for another. A clear explanation is not empirical support. A complete chronicle does not repair a flawed experiment. A successful replay does not by itself make a claim admissible.

## Work from bounded claims

For every material claim:

1. Write falsifiable wording and prohibited overstatements.
2. Define the estimand, inferential unit, data source, uncertainty method, decision rule, and critical slices.
3. Freeze the simplest credible baseline.
4. Register amendments and whether they are pre-outcome or post-outcome.
5. Bind results to immutable evidence and replay metadata.
6. Preserve failed gates, negative results, and errata instead of rewriting them away.

Prefer the minimum sufficient mechanism set. For each additional mechanism, apply the removal test from [SDR-002](design/SDR-002-minimum-sufficient-mechanism.md): name the distinct need it covers and the evidence that the simpler candidate does not cover it adequately.

## Record only decision-relevant chronicle events

Create a chronicle entry when new information changes, blocks, permits, or reopens a research action, gate, interpretation, or belief. Do not create ceremonial logs for observations that have no behavioral effect. Apply [SDR-001](design/SDR-001-chronicle-requires-behavioral-effect.md).

Retain superseded reasoning and link corrections. Git history supports traceability but does not prove author identity or non-repudiation.

## Build pedagogical artifacts as derived views

For each important concept, prefer progressive views:

1. one-sentence intuition;
2. visual mechanism;
3. manipulable toy;
4. mathematical definition;
5. implementation contract;
6. faithful replay path;
7. reviewer challenge;
8. decision and residual uncertainty.

Label every toy with its simplifications and divergence from the full study. Bind figures and explanations to versioned authoritative data or calculations. If the pedagogical artifact disappears, the scientific runner and decision must remain unchanged.

### Make interactive labs purposeful

Use an interactive control only when changing it exposes a named mechanism, assumption, trade-off, failure mode, or decision boundary. Define:

- the parameter the learner manipulates;
- the mechanism expected to change;
- the observable response;
- the misleading interpretation to guard against;
- the authoritative calculation and replay source.

Keep the computation independent of the visualization library. Export versioned tabular or JSON data before rendering when practical.

Select the smallest sufficient rendering mechanism:

- Use **Matplotlib** for stable static figures, publication artifacts, and deterministic CI comparisons.
- Prefer **Bokeh** for Python or notebook labs where hover, linked selection, sliders, or browser-native exploration materially improve understanding.
- Use **Plotly** when rotatable 3D exploration is essential and a 2D view cannot expose the mechanism adequately.
- Use framework-native HTML/JavaScript only when it reduces integration cost or supports a Diderot page that cannot be served adequately by a notebook export.

Treat these as replaceable adapters, not scientific dependencies. Pin versions, provide a static fallback for important views, and keep dynamic Python callbacks behind a validated server-side runner with an allow-list, quotas, ephemeral workspaces, and no default Internet egress. Do not execute untrusted Python in the browser.

Apply the removal test to the visualization layer:

> If this interactive adapter is removed or replaced, do the authoritative calculation, evidence, gate, and scientific conclusion remain identical?

If not, the visualization has leaked into the scientific mechanism and must be redesigned or explicitly promoted into the scientific plane.

Follow [SDR-003](design/SDR-003-interactive-pedagogy-as-replaceable-view.md) for interactive pedagogy decisions.

## Challenge exploratory findings

Treat exploratory analysis as hypothesis generation unless its decision rule was preregistered. For apparent clusters, manifolds, anomalies, or regimes:

1. report preprocessing and feature scaling choices;
2. report variance or information retained;
3. compare with a simple baseline and at least one plausible alternative view;
4. test stability across seeds, samples, preprocessing, and dimensions;
5. quantify structure with an appropriate metric when possible;
6. inspect critical slices and domain meaning;
7. distinguish visual salience from validated structure;
8. record the next falsifiable question rather than promoting the pattern directly to a claim.

## Close with replay and residual uncertainty

At study closure, propose both:

- a simplified learning toy; and
- a faithful full replay.

Include claims, admissible wording, failed gates, negative results, errata, reviewer challenges, belief changes, unresolved questions, environment, seeds, input hashes, and generated-artifact provenance.