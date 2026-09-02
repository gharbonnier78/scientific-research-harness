# Interactive Artifact Execution-Evidence Guidance

Status: **draft guidance proposal for independent review**

## Purpose

Some research, engineering and pedagogical artifacts make a user-facing claim that depends on actual interaction rather than static source inspection alone. Examples include a teaching lab whose controls change a rendered state, a dashboard whose filters drive a decision view, an evidence viewer whose navigation exposes provenance, or an application whose public workflow is itself part of what is being reviewed.

This guidance defines a technology-neutral way to collect **interaction execution evidence** for such bounded claims without turning UI execution into scientific evidence and without prescribing a particular browser, test framework or application stack.

The immediate consumer example was Diderot PR #14, which exercised learner-visible controls through Chromium/Playwright. That example motivates the vocabulary here; it does not make Playwright, browser automation or Diderot's exact journey normative.

## Applicability

A consumer SHOULD consider interaction execution evidence when all of the following are true:

- a material review, release or pedagogical claim depends on behavior exposed through an interactive user surface;
- source-level, unit-level or static checks alone would not establish that the intended user can actually exercise that behavior;
- the interaction can be exercised safely and proportionally to the selected engineering-care profile.

This guidance is normally **not applicable** when the artifact is non-interactive, when the user surface is incidental to the bounded claim, or when exercising the surface would add more risk or complexity than decision value. Non-applicability should be stated rather than inferred from a missing test.

## Core distinction

A workstream should distinguish:

- **public interaction contract** — the user-visible actions, navigation and observable outputs that matter to the bounded claim;
- **interaction execution evidence** — evidence that the declared public interaction path executed and produced the declared observable behavior;
- **implementation-level evidence** — unit, component or internal checks that may support the same feature without exercising the user surface;
- **scientific evidence** — evidence that bears on a scientific claim, estimand or hypothesis under its own scientific protocol.

Passing interaction execution evidence does not promote an implementation mechanism, screenshot, DOM assertion, UI trace or application workflow into scientific evidence.

## Public-surface principle

When the bounded claim is that an intended user can perform or observe a behavior through a public interaction surface, the verification SHOULD exercise that same public surface where practical rather than substitute a hidden helper, internal API or direct state mutation for the user path.

Internal assertions MAY be used after the public action to inspect resulting state, provided they do not replace the interaction whose accessibility or behavior is under review.

A consumer MAY use browser automation, native-app automation, accessibility APIs, command-line interaction, hardware-in-the-loop controls or another suitable mechanism. The reusable requirement is about the **surface being evidenced**, not the tool used to drive it.

## Minimum recoverable interaction contract

For a material interactive claim, a reviewer should be able to recover:

1. the bounded behavior being claimed;
2. the public actions or controls used to exercise it;
3. the expected observable outcome or state transition;
4. any important boundary or control condition that distinguishes a meaningful pass from a trivial happy path;
5. the code/configuration coordinate for the exercised artifact;
6. the execution/run coordinate and relevant environment when replay matters;
7. the verdict and enough evidence to distinguish product behavior from infrastructure or harness failure.

These items do not require a new canonical manifest schema in this phase. Consumers may record them in existing tests, CI metadata, evidence packs, chronicles or local contracts.

## Failure classification

A failed interaction run SHOULD be classified before drawing a conclusion. Useful classes include:

- **product/runtime failure** — the application or rendered runtime is broken;
- **interaction-contract failure** — the surface loads, but the declared user action does not produce the required observable behavior;
- **navigation/workflow failure** — the declared user path cannot be completed;
- **dependency/infrastructure failure** — runner, browser, device, network, test environment or dependency setup failed before the product claim could be evaluated;
- **evidence-capture failure** — the interaction may have run, but the evidence required for a reviewable verdict was not retained or correlated;
- **scientific disagreement** — **not inferable from interaction execution alone** unless the scientific protocol explicitly designates that interaction as part of the scientific measurement process.

A missing interaction signal caused by the execution harness must not be silently reported as a product-behavior failure.

## Boundary and control conditions

A purely happy-path click-through can be too weak to support a material interaction claim. Where proportionate, the public interaction contract SHOULD include at least one boundary, negative or control condition that could falsify an over-broad interpretation of the behavior.

Examples include:

- a threshold is crossed only after the relevant control moves far enough;
- an error or refusal state appears for an invalid input;
- a navigation route becomes unavailable when its prerequisite is not met;
- two apparently similar states remain distinguishable under a declared control;
- a reversible interaction returns the artifact to its prior state.

The boundary condition is part of engineering/test evidence. It is not automatically a scientific control condition.

## Reproducibility and evidence retention

When an interaction run is used in review or release reasoning, the workstream SHOULD retain enough provenance to replay or inspect it proportionally to impact. Useful coordinates include:

- immutable or bounded source/configuration revision;
- execution environment or environment lock when material;
- test/journey definition;
- CI or local run identifier;
- screenshots, recordings, logs, traces or structured assertions when they add review value;
- explicit expected-versus-actual verdict.

A screenshot alone is weak evidence for an interaction claim because it usually does not prove how the state was reached. Conversely, a fully automated journey without reviewable outputs may be hard for a human reviewer to audit. Consumers should retain the smallest combination that makes the bounded claim inspectable.

## Security and operational boundary

Automation through a public surface inherits the engineering and security posture of the artifact. If the interaction can mutate production data, access credentials, invoke external paid services, trigger safety-relevant behavior, expose personal data or perform privileged actions, the workstream MUST follow the applicable engineering-care and security constraints before using that path as routine evidence.

This guidance is not permission to automate a hazardous or privileged workflow merely because an interaction test would be convenient.

## Evidence boundary

A passing interaction execution check supports a bounded engineering statement such as:

> The declared user-accessible interaction path executed on the recorded artifact/environment and produced the declared observable behavior.

It does **not**, by itself, establish that:

- the underlying scientific explanation is true;
- the interactive toy validates a scientific hypothesis;
- all users, devices, browsers or environments behave equivalently;
- accessibility requirements are satisfied unless they are explicitly part of the interaction contract;
- the implementation is production-ready;
- hidden states or untested paths are correct.

Scientific qualification, usability research, accessibility conformance, security assurance and production acceptance each require their own appropriate evidence.

## Relationship to existing harness contracts

This guidance complements, but does not replace:

- proportional engineering care in `HARNESS.md` section 4b;
- the distinction between scientific evidence and runtime/test evidence in sections 4 and 4c;
- direct reviewer navigation in section 6b;
- consumer-specific local test or browser contracts.

It does not modify the telemetry-evidence contract and does not require OpenTelemetry for interaction evidence. Telemetry MAY support an interaction verdict when its expected-signal contract is separately declared.

## Consumer conformance questions

A consumer applying this guidance should be able to answer:

- What user-facing behavior is material to the bounded claim?
- Which public actions exercise it?
- What observable outcome constitutes pass or fail?
- Is there a useful boundary or control condition, or is the check merely a happy path?
- Does the automated journey actually use the user surface rather than bypass it?
- Can infrastructure failure be distinguished from product-behavior failure?
- What code/run/environment coordinate lets a reviewer inspect or replay the evidence?
- What exactly does a pass authorize us to say — and what does it not authorize?

## Origin and promotion boundary

The immediate evidence base is one independently reviewed consumer pattern: Diderot PR #14. Its browser journey demonstrated that a human-accessible pedagogical interaction path could be exercised through public controls while keeping browser/test evidence separate from scientific evidence.

One consumer is enough to motivate reusable terminology and guidance, but not enough to justify a mandatory framework-specific gate or a new required manifest schema. This document should therefore remain guidance/design material until another materially different consumer tests whether the abstraction transfers beyond the original browser-teaching context.

Before stronger promotion, independent review should challenge at least:

- whether the public-surface principle is proportionate across browser, native, CLI and hardware-mediated interaction;
- whether the failure taxonomy remains useful outside Diderot;
- whether the retained coordinates are sufficient without becoming a new bureaucracy;
- whether a second consumer reveals a need for stronger MUST/SHOULD language or a reusable handoff field.
