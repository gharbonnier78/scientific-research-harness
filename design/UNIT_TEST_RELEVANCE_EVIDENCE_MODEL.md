# Unit-Test Relevance: Evidence Model and Ablation Programme

Status: **research-backed draft; not a normative score**

## Purpose

This note defines a falsifiable way to study whether a unit test, test-generation method, or test suite is useful. It deliberately avoids equating usefulness with test count, code coverage, requirements traceability, mutation score, or any other single proxy.

The central question is:

> Given a declared property, fault population and operating context, how much credible evidence does a test contribute that faults which matter would be detected, at what cost and with what uncertainty?

This note complements:

- `design/PROPERTY_TO_TEST_DERIVATION.md`;
- `pedagogy/HEURISTIC_FALSIFICATION_LAB.md`.

It is informed by established software-testing research and by assurance practices from aerospace, semiconductor manufacturing test, medical-device software assurance, and process functional safety. Cross-domain analogies are hypotheses to test, not proof that methods transfer unchanged.

---

## 1. What established practice already tells us

### 1.1 Aerospace software: local capability, off-nominal behaviour, repeatability, structural evidence

NASA's Software Engineering Handbook describes unit testing as confirming that a unit performs the capability allocated to it, interfaces correctly, and faithfully implements its design. It explicitly calls for maximum/minimum, invalid, empty and corrupt inputs, boundary/error situations, and, for safety-critical software, repeatable tests and stronger structural coverage such as MC/DC.

Important interpretation:

- boundary/off-nominal testing is not a fashionable add-on; it is an established assurance concern;
- repeatability is an evidence-quality property;
- MC/DC is structural evidence and must not be silently reinterpreted as a probability of fault detection;
- testing rigor should be related to criticality, not applied uniformly.

Primary sources:

- NASA SWE-062 Unit Test: https://swehb.nasa.gov/spaces/SWEHBVD/pages/102695446/SWE-062+-+Unit+Test
- NASA SWE-104 / Software Test Plan: https://swehb.nasa.gov/spaces/7150/pages/16449696/SWE-104+-+Software+Test+Plan
- NASA Off-Nominal Testing: https://swehb.nasa.gov/spaces/SWEHBVD/pages/102695701/8.01+-+Off+Nominal+Testing
- FAA AC 20-115D recognition of DO-178C and supplements: https://www.faa.gov/airports/resources/advisory_circulars/index.cfm/go/document.information/documentNumber/20-115D

### 1.2 Semiconductor test: explicit fault model, controllability, observability, fault accounting

Semiconductor manufacturing test provides a strong analogy because test quality is explicitly evaluated against declared fault models. IEEE 1804 standardizes fault accounting, classification and coverage reporting for the single stuck-at fault model. Industrial ATPG practice uses multiple models such as stuck-at, transition, bridging and cell-aware faults and generates patterns intended to sensitize and propagate a fault effect to an observable output.

Important interpretation for software testing:

- a detection percentage is meaningless without naming the fault model/population;
- testability depends on both controllability (can the relevant internal condition be stimulated?) and observability (can its effect reach an oracle?);
- one fault model is never the complete defect universe;
- multiple complementary fault models may be required;
- uniform accounting matters if scores are to be compared across time or tools.

Primary sources:

- IEEE 1804-2017, Fault Accounting and Coverage Reporting for Digital Modules: https://standards.ieee.org/ieee/1804/4604/
- Synopsys TestMAX ATPG product description and supported fault models: https://www.synopsys.com/implementation-and-signoff/test-automation/testmax-atpg.html

### 1.3 Medical device software: risk-based assurance rather than ritual evidence

FDA software-validation guidance states that software validation should establish documented evidence that software specifications conform to user needs and intended uses. FDA's 2026 Computer Software Assurance guidance explicitly promotes a risk-based approach and varying rigor for production and quality-system software.

Important interpretation:

- evidence strength should be proportional to risk and intended use;
- a test is not valuable merely because it exists or is automated;
- the relationship among user need, intended use, specification and evidence remains important even when local tests are generated automatically.

Primary sources:

- FDA General Principles of Software Validation: https://www.fda.gov/regulatory-information/search-fda-guidance-documents/general-principles-software-validation
- FDA Computer Software Assurance, February 2026: https://www.fda.gov/regulatory-information/search-fda-guidance-documents/computer-software-assurance-production-and-quality-management-system-software
- FDA recognition of ISO/IEC/IEEE 29119-1: https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfstandards/detail.cfm?standard__identification_no=44690

### 1.4 Process / chemical functional safety: risk, systematic faults, exact properties and diagnostic coverage

IEC 61511 applies functional-safety principles to safety instrumented systems in the process industry and ties safety functions to hazard/risk analysis and required safety integrity. IEC 61508 provides broader functional-safety foundations; IEC 61508-6 includes diagnostic-coverage calculations, and IEC TS 61508-3-2:2024 addresses mathematical and logical techniques for establishing exact software properties.

Important interpretation:

- assurance is connected to hazards and safety functions, not generic test counts;
- diagnostic/test coverage is meaningful only relative to defined failure modes and assumptions;
- some software properties are better established by formal or analytical techniques than by executable unit tests;
- the appropriate verification mechanism follows the property.

Primary sources:

- IEC TR 61511-0 overview: https://webstore.iec.ch/en/publication/60766
- IEC 61511-3 hazard/risk and SIL guidance: https://webstore.iec.ch/en/publication/25480
- IEC 61508-6 diagnostic coverage guidance: https://webstore.iec.ch/en/publication/5520
- IEC TS 61508-3-2:2024 exact software properties: https://webstore.iec.ch/en/publication/62902

---

## 2. Research evidence: what is known, and what remains a proxy

### 2.1 Code coverage is exposure evidence, not direct detection evidence

Coverage can reveal unexercised code and is useful for completeness/omission analysis, but it does not by itself establish that an exercised fault would be observed by the oracle. This distinction is compatible with aerospace structural-coverage practice and is supported empirically by software-testing research.

### 2.2 Mutation score is generally stronger, but it is still conditional on the mutant population

Just et al. (FSE 2014) found statistically significant correlation between mutant detection and real-fault detection, independently of code coverage, on 357 real faults from five open-source applications. This supports mutation as a useful proxy, not a universal ground truth.

Petrovic et al. (ICSE 2021), using a dataset of almost 15 million mutants, found evidence that mutation feedback can lead developers to improve test suites and that mutants can be coupled to historical real faults.

However:

- mutation effectiveness depends on the mutation operators and mutant selection;
- equivalent and subsumed mutants distort cost and interpretation;
- traditional mutants do not span the full space of real faults;
- recomputing changing mutant populations can make longitudinal scores incomparable.

Primary research:

- Just et al., *Are mutants a valid substitute for real faults in software testing?* https://homes.cs.washington.edu/~mernst/pubs/mutation-effectiveness-fse2014-abstract.html
- Petrovic et al., *Long Term Effects of Mutation Testing*: https://research.google/pubs/long-term-effects-of-mutation-testing/
- Papadakis et al., *Threats to the validity of mutation-based test assessment*: https://discovery.ucl.ac.uk/id/eprint/1508136
- Ojdanic et al., *Keeping Mutation Test Suites Consistent and Relevant with Long-Standing Mutants*: https://arxiv.org/abs/2212.11762
- Zhang et al., ASSENT framework for test-suite-effectiveness metrics: https://arxiv.org/abs/2204.09165

### 2.3 Automated test generation can achieve coverage without reliable fault detection

Shamshiri et al. evaluated Randoop, EvoSuite and Agitar on 357 Defects4J faults. Automatically generated tests detected some real faults, but high generation activity/coverage did not imply consistent real-fault detection. This makes real faults and held-out faults important external-validity checks for any future generator.

Primary research:

- Shamshiri et al., *Do Automatically Generated Unit Tests Find Real Faults?*: https://orbilu.uni.lu/handle/10993/21589

### 2.4 LLM + mutation is now an industrially demonstrated candidate, not yet an authority

Meta's Automated Compliance Hardening (ACH) uses LLMs to generate concern-specific mutants and mutation-guided unit tests at industrial scale. Intent-based mutation research similarly proposes mutating programming intent rather than only syntax. MUTGEN reports improved mutation score over EvoSuite and vanilla LLM prompting on its evaluated subjects.

These results justify an LLM mutant generator as an experimental arm. They do **not** justify assuming that LLM-generated mutants are more representative of real faults without held-out validation.

Primary / industrial sources:

- Foster et al., *Mutation-Guided LLM-based Test Generation at Meta*: https://arxiv.org/abs/2501.12862
- Meta Engineering description of ACH: https://engineering.fb.com/2025/02/05/security/revolutionizing-software-testing-llm-powered-bug-catchers-meta-ach/
- Hamidi et al., *Intent-Based Mutation Testing*: https://arxiv.org/abs/2607.05149
- Wang et al., *Mutation-Guided Unit Test Generation with a Large Language Model*: https://pure.ul.ie/en/publications/mutation-guided-unit-test-generation-with-a-large-language-model/

---

## 3. Do not define one universal `test_relevance_score`

A single scalar invites Goodhart effects and hides trade-offs. Store a vector of evidence dimensions first. A scalar utility may later be derived for a declared decision context.

For test `T_i`, property `P`, fault class `F_k`, and context `C`:

```text
R(T_i) = {
  property_authority,
  fault_scope,
  detection_estimate,
  detection_uncertainty,
  false_alarm_rate,
  marginal_detection,
  boundary_or_interaction_reach,
  oracle_evidence,
  repeatability,
  execution_cost,
  maintenance_cost,
  real_fault_external_validity
}
```

Each item has a different epistemic meaning.

---

## 4. Core measurable quantities

### 4.1 Property authority / legitimacy

Question:

> Why is this property supposed to be true?

Possible provenance:

- requirement / verifiable criterion;
- architecture or interface contract;
- mathematical invariant;
- safety/security constraint;
- standard;
- historical incident;
- accepted design decision;
- inferred candidate invariant.

This is not a performance metric. It is provenance and scope. An inferred property must remain `candidate` until accepted by an appropriate authority.

### 4.2 Conditional detection probability / empirical sensitivity

For a declared fault population `F_k` and context `C`:

\[
p_{i,k,C} = P(T_i\;\text{fails}\mid F_k, C).
\]

If the test and mutant are deterministic, an individual execution is only a hit/miss observation. A probability requires repeated sampling over a declared population of faults and/or contexts.

Estimation options include:

- binomial proportion with Wilson or exact interval;
- beta-binomial Bayesian posterior for a homogeneous declared population;
- logistic regression when detection depends on covariates;
- hierarchical logistic models when faults/tests/projects form nested or heterogeneous groups;
- bootstrap when the resampling unit is defensible and dependence is preserved.

Do not call this `POD` without naming the conditioning population.

Recommended terminology until validated:

> **Conditional Fault Detection Probability (CFDP)**

or

> **Empirical Detection Rate for fault population X**.

### 4.3 False-alarm / instability probability

The dual question is:

\[
q_i = P(T_i\;\text{fails}\mid \text{target property is not violated}).
\]

This captures flakiness, environmental noise, oversensitive tolerances and non-target failures.

Sensitivity without specificity is not sufficient test quality.

### 4.4 Detection surface instead of a single POD curve

NASA NDE POD commonly models detection as a function of physical flaw size and requires explicit confidence bounds. Software has no universal scalar equivalent of flaw size.

Use a conditional detection surface instead:

\[
P(D=1 \mid
\text{fault class},
\text{activation distance},
\text{boundary distance},
\text{configuration},
\text{state},
\text{severity},
\text{test strategy})
\]

where covariates are defined only when they have semantic meaning.

NASA POD is therefore a methodological inspiration for uncertainty-aware detection studies, not a formula to copy blindly.

Primary NASA POD sources:

- NASA NDE POD Study Guidebook: https://ntrs.nasa.gov/citations/20220013822
- NASA POD methodologies (ROC, binomial, logistic, Bayesian): https://ntrs.nasa.gov/citations/20140005337
- NASA binomial POD and 90/95 demonstration discussion: https://ntrs.nasa.gov/archive/nasa/casi.ntrs.nasa.gov/20110015149.pdf

### 4.5 Marginal detection contribution

A test can have high raw detection but add no information to an existing suite.

For binary fault detection and fault weights `w_k`:

\[
\Delta U(T_i\mid S)
=
U(S\cup\{T_i\})-U(S).
\]

A simple non-probabilistic form is weighted unique kills:

\[
M_i = \sum_k w_k\;I(T_i\text{ detects }F_k \land S\text{ does not}).
\]

This supports redundancy analysis and test-suite minimization.

### 4.6 Time/cost-sensitive detection

APFD measures the rate at which faults are detected in an ordered test suite. Cost-aware variants such as APFDc incorporate test cost and fault severity. These are relevant when CI budget or feedback latency matters, but they measure prioritization performance rather than intrinsic truth of a test.

Primary research:

- Elbaum, Malishevsky, Rothermel, *Test Case Prioritization: A Family of Empirical Studies*: https://digitalcommons.unl.edu/csearticles/8/

### 4.7 Combinatorial / interaction reach

NIST ACTS and t-way testing provide a principled way to cover combinations of parameter values under constraints. This is especially useful for generating contexts in which a fault is activated.

It should remain a coverage statement:

> all declared t-way combinations were exercised

not:

> all relevant failures were tested.

Primary sources:

- NIST combinatorial testing programme: https://csrc.nist.gov/Projects/Automated-Combinatorial-Testing-for-Software/
- NIST SP 800-142: https://csrc.nist.gov/pubs/sp/800/142/final

### 4.8 Oracle / observability evidence

A fault can be activated and propagate without causing a test failure. Distinguish where possible:

1. **reachability** — test executes the affected code/state;
2. **infection** — faulty state differs internally from correct state;
3. **propagation** — difference reaches an observable boundary;
4. **revelation** — oracle rejects the observed result.

Mutation killing observes the end-to-end result of this chain but normally does not reveal which stage failed. Instrumented experiments can help diagnose weak stimulation versus weak oracle versus poor observability.

The semiconductor DFT analogy is useful here: controllability and observability are explicit design concerns.

### 4.9 Repeatability / stability

Measure:

- rerun disagreement rate;
- environment sensitivity;
- seed sensitivity where randomness is legitimate;
- semantics-preserving refactor survival when appropriate.

A test that detects faults but cannot be reproduced is weak operational evidence.

### 4.10 External validity against real faults

When historical real bugs are available, hold them out from generation/tuning and use them as a stronger evaluation population.

Do not:

- derive mutants from a bug and then claim success because the resulting test detects that same bug;
- tune on one mutation operator set and evaluate only on the same set;
- use generated mutants as both training signal and sole ground truth.

---

## 5. Proposed evidence hierarchy

No level is universally mandatory; the hierarchy describes increasing external validity.

```text
L0  structural exposure
    line / branch / MC/DC / path evidence

L1  declared property examples
    nominal + boundary + invalid/error cases

L2  generated property exploration
    property-based / metamorphic / combinatorial

L3  classical mutation adequacy
    with explicit operators and equivalent-mutant policy

L4  complementary fault populations
    semantic/LLM, historical, risk/hazard, configuration, concurrency, numerical

L5  held-out fault-model detection
    unseen operators/classes/contexts

L6  historical real-fault detection
    faults not used to create or tune tests

L7  prospective escapes
    future defects/incidents observed after deployment of the method
```

A higher level does not erase lower-level obligations. It changes the strength and scope of the claim.

---

## 6. Ablation programme for test-authoring methods

### 6.1 Research question

Which method, or combination of methods, increases credible fault detection and decision value per unit cost?

### 6.2 Candidate arms

Keep the same code subjects, properties, budgets and evaluation faults wherever possible.

```text
A0  Existing/manual baseline
A1  Requirement/property-derived examples
A2  + property-based/metamorphic generation
A3  + classical mutation-guided improvement
A4  + LLM semantic/intent mutant generation
A5  + historical/risk/hazard-derived faults
A6  + combinatorial context generation
A7  + adaptive prioritization / marginal-utility selection
```

Do not assume monotonic improvement from A0 to A7. Full-system complexity may reduce value.

### 6.3 Primary outcomes

Prefer a small number of predeclared primary outcomes.

Candidate primary outcome:

\[
\text{weighted held-out fault detection with lower confidence bound}
\]

where weights come from a predeclared severity/risk model rather than post-hoc preference.

Strong secondary outcome when available:

\[
\text{held-out historical real-fault detection rate}.
\]

### 6.4 Secondary outcomes

- conditional detection by fault class;
- lower confidence bounds;
- unique / marginal fault detection;
- APFD/APFDc or time-to-first-detection under a CI budget;
- test execution cost;
- generation cost, including LLM tokens/compute if used;
- human review time;
- false-alarm/flaky rate;
- equivalent-mutant burden;
- test churn after semantics-preserving refactors;
- readability/maintainability assessed separately from fault-detection performance;
- semantic relevance adjudication by blinded domain reviewers where feasible.

### 6.5 Required ablations

For a candidate `Full` system, at minimum compare:

```text
Full
Full - LLM mutants
Full - classical mutants
Full - property derivation
Full - combinatorial generation
Full - risk weights
Full - adaptive selection
```

If removing a sophisticated component does not materially reduce the predeclared primary outcome, the component has not demonstrated sufficient marginal value.

### 6.6 Hold-out discipline

Separate:

```text
faults used to guide generation
faults used for local validation
faults used for final evaluation
```

Where possible also hold out complete fault families/operators, not merely individual instances, to test generalization.

Historical bugs used as final evaluation must not be included in prompts, mutation templates or training examples for that run.

### 6.7 Statistical design

Prefer paired comparisons: the same subject/change should be tested by competing methods.

Use:

- paired binary analysis for fault-detection differences;
- bootstrap at the correct independent unit (often project/change/fault family rather than individual mutant);
- hierarchical models when observations are clustered by project, file, developer, fault class or test suite;
- preregistered effect sizes and confidence/credible intervals, not only p-values;
- sensitivity analysis to alternative fault weights.

Report heterogeneity. A method that is excellent for boundary faults and poor for concurrency faults should not be collapsed into one unexplained mean.

---

## 7. Mutant/fault generator comparison

Every fault candidate should record provenance:

```yaml
fault_id: ...
generator:
  type: classical_operator | llm_semantic | incident_derived | hazard_derived | human | fuzz | other
  version: ...
source_property: ...
fault_class: ...
rationale: ...
expected_effect: ...
context_constraints: ...
equivalence_status: unknown | suspected | equivalent | non_equivalent
real_fault_analogue: ...
held_out: true | false
```

Compare generators on:

1. **validity** — does it compile/run and express a genuine behavioural variation?
2. **non-equivalence rate**;
3. **novelty/non-subsumption** relative to other generators;
4. **historical-real-fault coupling**;
5. **ability to discriminate weak and strong test suites**;
6. **cost per useful mutant**;
7. **human/domain relevance**;
8. **stability over software evolution**.

The LLM is one generator in this registry, never the oracle of its own relevance.

---

## 8. What should be challenged experimentally

### Hypothesis H1 — Mutation score is a better proxy than branch coverage

Prior evidence supports H1 on multiple datasets, but test it locally against held-out faults.

### H2 — LLM semantic mutants add fault classes not represented by classical operators

Recent intent-based mutation results make this plausible. Test non-subsumption and real-fault coupling rather than counting generated mutants.

### H3 — Requirements/property derivation improves semantic relevance

Measure whether generated tests detect weighted held-out faults linked to declared properties, not only whether reviewers like the test text.

### H4 — Boundary/off-nominal generation improves fault detection

NASA practice provides strong rationale. Quantify the incremental effect over nominal examples.

### H5 — Combinatorial contexts expose interaction faults efficiently

NIST research supports t-way methods, but local interaction strength and constraints must be measured.

### H6 — Adaptive test selection improves detection per CI minute

Compare against simple greedy marginal-coverage/mutation baselines before bandits/RL/POMDP.

### H7 — A unified scalar relevance score predicts real-fault detection

Treat this hypothesis skeptically. Start with the evidence vector; learn/validate any scalar only against held-out outcomes.

---

## 9. A tentative decision-oriented utility, not a standard

Only after the dimensions above are measured may a local decision function be considered.

For suite `S` and declared fault population `F`:

\[
U(S)=
\sum_k w_k P(D_k\mid S,C)
-\lambda C_{exec}(S)
-\mu C_{maint}(S)
-\nu FP(S).
\]

This expression is deliberately incomplete:

- `w_k` is contextual and contestable;
- detection events may be dependent;
- fault prevalence is generally unknown;
- maintenance cost is difficult to estimate;
- mutation populations can be biased;
- one aggregate can conceal catastrophic blind spots.

Therefore store component measures and confidence bounds even if a local utility is computed.

---

## 10. Research rule

The harness SHOULD apply the following rule to proposed test-generation sophistication:

> **A technique is not promoted because it generates more tests, more coverage, more mutants, or a higher internal score. It is promoted only when a controlled comparison shows material incremental value on a predeclared relevance criterion, with uncertainty and scope stated.**

Ablation is therefore part of the method, not an optional academic exercise.

---

## 11. Immediate practical minimum

A first implementation does not need ML, RL or POMDP. It can begin with:

1. explicit property references;
2. nominal/boundary/off-nominal partitions;
3. classical mutants with declared operators;
4. one optional LLM semantic-mutant generator;
5. held-out mutants/fault families;
6. a real-bug corpus when available;
7. hit/miss detection matrices;
8. Wilson/Beta-binomial detection intervals;
9. unique-kill/marginal contribution;
10. execution cost and flakiness;
11. paired ablations.

Only after this baseline exists should more adaptive techniques be justified.
