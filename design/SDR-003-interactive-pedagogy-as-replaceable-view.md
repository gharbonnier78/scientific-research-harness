# SDR-003 — Treat interactive pedagogy as a replaceable derived view

Status: **ACCEPTED DESIGN DIRECTION**  
Date: 2026-08-15

## Context

The harness treats pedagogy as a first-class plane while keeping it distinct from scientific evidence. Diderot-style learning artifacts should let a learner manipulate parameters and observe consequences immediately, but a visualization tool must not become an undeclared part of the scientific method.

A PCA visualization lab reviewed on 2026-08-15 illustrates a useful pattern:

- Matplotlib provides static plots;
- Bokeh provides browser-native interactive exploration;
- Plotly provides rotatable 3D views;
- scikit-learn remains responsible for PCA calculations.

The pattern is valuable, but copying a library choice without an architectural boundary would create avoidable coupling. Interactive output can also become misleading when visual salience is treated as validation, preprocessing is hidden, randomness is not controlled, or a widget cannot be replayed.

Source observation:

- Bokeh project: https://bokeh.org/
- Course artifact observed: *PCA — An example on Exploratory Data Analysis*, C3 W2 optional visualization lab.
- The external course artifact is an inspiration and review target, not normative authority and is not republished here.

## Decision

Treat every interactive pedagogical artifact as a **replaceable derived view** over an authoritative, versioned calculation.

Use Bokeh as a preferred adapter for Python/notebook labs when hover, linked selection, sliders, or browser-native exploration exposes a named mechanism better than a static figure. Do not make Bokeh a mandatory dependency of the scientific runner.

Keep the following boundary:

```text
authoritative data + calculation + replay
                  |
        versioned rendering payload
                  |
       replaceable visualization adapter
        /             |              \
 Matplotlib         Bokeh          Plotly / web
```

The authoritative result must be obtainable and testable without the interactive adapter.

## Interaction contract

For each interactive view, declare:

1. **Learning objective** — what the learner should understand.
2. **Manipulated parameter** — what can be changed.
3. **Mechanism** — why the change should affect the result.
4. **Observable** — what visual or numeric response should change.
5. **Guardrail** — which tempting interpretation would be invalid.
6. **Authoritative source** — versioned data, calculation, configuration, and replay reference.
7. **Fallback** — static or tabular representation preserving the important information.
8. **Adapter status** — required for pedagogy, optional enhancement, or experimental.

An interaction that merely adds motion, decoration, or tool novelty is insufficient.

## Adapter-selection rule

Choose the minimum sufficient rendering mechanism:

| Need | Default adapter | Rationale |
|---|---|---|
| Stable publication or CI-comparable figure | Matplotlib | Small dependency and deterministic static output |
| Hover, linked brushing, selection, or sliders in Python/notebooks | Bokeh | Browser-native interaction close to the PyData stack |
| Essential 3D rotation | Plotly | Mature notebook 3D interaction |
| Integrated Diderot application behavior | Native HTML/JavaScript or approved app framework | Avoid unnecessary notebook/server coupling |

This is guidance, not exclusivity. A different adapter is admissible when it better satisfies the declared interaction contract.

## Scientific boundary and removal test

Apply this removal test:

> If the visualization adapter is removed or replaced, do the authoritative calculation, evidence, gate status, and scientific conclusion remain identical?

- If **yes**, keep the artifact in the pedagogical plane.
- If **no**, identify the leaked dependency. Redesign the boundary or explicitly promote the relevant interaction-generated data or procedure into the scientific protocol and evidence plane.

A pedagogical widget must never silently generate outcome-bearing evidence.

## Reproducibility and safety requirements

- Pin visualization and serialization versions.
- Separate calculation tests from rendering smoke tests.
- Version the rendering payload when practical.
- Record seeds for stochastic demonstrations.
- Provide a static fallback for important views.
- Sanitize data used in public examples.
- Keep dynamic Python callbacks behind a validated server-side runner with an allow-list, ephemeral workspace, quotas, rate limits, and no default Internet egress.
- Do not execute untrusted Python in the browser.
- Preserve accessibility: labels, numeric values, keyboard-compatible controls where supported, and a non-interactive explanation.

## Exploratory-analysis guardrails

Interactive exploration generates hypotheses; it does not validate them.

For apparent clusters, manifolds, anomalies, or regimes:

- disclose centering, scaling, missing-value handling, sampling, and feature selection;
- show the amount of variance or information retained;
- test sensitivity to preprocessing, random seed, sample, and dimensionality;
- compare against a simple baseline and plausible alternative representation;
- quantify stability or separation when possible;
- seek domain meaning and critical-slice behavior;
- record the next falsifiable question.

Do not infer a real system taxonomy merely because points look separated in a projection.

## Consequences

- Diderot-style labs can be interactive without coupling scientific validity to a UI library.
- Bokeh becomes reusable guidance rather than a mandatory platform choice.
- Static publication figures and rich exploratory widgets can share the same authoritative payload.
- Visualization changes carry a smaller review burden when the removal test passes.
- Interactive outputs require explicit guardrails against visual overinterpretation.
- The project must maintain at least lightweight rendering smoke tests and a fallback path.

## Challenge questions

1. Does a static fallback preserve the mechanism or only a screenshot of one parameter setting?
2. Can linked selection reveal a dependence that the exported payload does not record?
3. Does replacing the adapter change numerical transformations, filtering, or default sampling?
4. Are learners interpreting a visually dominant component as the most decision-relevant one?
5. Does 3D interaction add genuine information, or only make a weak pattern look compelling?
6. Which accessibility and offline constraints make another adapter preferable?
7. At what point does learner-generated exploration become outcome-bearing research data?

Repeated counterexamples should revise this SDR rather than create silent tool-specific exceptions.
