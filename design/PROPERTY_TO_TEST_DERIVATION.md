# Property-to-Test Derivation Model

Status: **draft reusable engineering design method**

## Purpose

This note defines a reusable way to derive locally verifiable properties and tests from system intent **without pretending that every business need maps one-to-one to a unit test**.

The method combines top-down requirement allocation with bottom-up evidence from design, hazards, interfaces, incidents and fault models.

## Why a simple requirement-to-test chain is insufficient

A useful top-down chain may look like:

```text
Need / RFP intent
    -> business requirement
    -> expected business value / outcome
    -> capability / system property
    -> feature / system behaviour
    -> verifiable criterion / system requirement
    -> verification requirement or verification objective
    -> use case / scenario
```

This is useful for preserving purpose and decision traceability, but stopping there leaves a gap between system-level intent and local implementation evidence.

The wrong response is to force every upstream object mechanically through:

```text
business requirement -> function -> unit test
```

That creates false traceability. Many business needs are emergent, cross-component or operational and cannot be demonstrated by a local test.

## Recommended descent

A more defensible engineering descent is:

```text
Need / RFP intent
        |
        v
Business requirement ----> business value / outcome
        |
        v
Capability / system property
        |
        +---------------------------+
        |                           |
        v                           v
Feature / system behaviour     hazards / failures that matter
        |                           |
        v                           v
System requirement /          safety/security/quality constraints
verifiable criterion               |
        |                           |
        +-------------+-------------+
                      |
                      v
           architecture and allocation
                      |
          +-----------+-----------+
          |                       |
          v                       v
 subsystem/component        interfaces / data / states
 responsibility                  contracts
          |                       |
          +-----------+-----------+
                      |
                      v
            local design obligations
                      |
        +-------------+-------------+----------------+
        |             |             |                |
        v             v             v                v
   invariants    pre/post       state rules      local budgets
                conditions      & errors        / tolerances
        |             |             |                |
        +-------------+-------------+----------------+
                      |
                      v
        coherent falsifiable local property
                      |
                      v
             verification mechanism
                      |
        +-------------+-------------------------+
        |             |                         |
        v             v                         v
     unit test    component/contract test   analysis/review/etc.
```

The test level is therefore selected **after** the property and its observation boundary are understood.

## Sources from which local properties emerge

A coherent local property may arise from more than one source.

### 1. Allocated requirement

Example:

```text
System: biometric decision threshold must be frozen before test evaluation
Allocated component responsibility: evaluator accepts threshold source = validation only
Local property: evaluator rejects threshold source = test
Verification: unit test
```

### 2. Interface or data contract

Example:

```text
System: downstream decision must be auditable
Interface contract: every decision event contains decision_id, model/version, threshold and evidence reference
Local property: serializer cannot emit a decision event missing a mandatory field
Verification: unit or schema/contract test depending on boundary
```

### 3. Algorithmic invariant

Example:

```text
No explicit business requirement says "covariance matrix shall remain symmetric".
Yet the estimator's mathematical validity requires it.
Local property: covariance update preserves symmetry within tolerance
Verification: unit/property test
```

### 4. Hazard or failure analysis

Example:

```text
Hazard: stale authorization remains usable after revocation
Control constraint: cache must not return authorization after expiry/revocation state
Local property: revocation transition invalidates cache entry before next authorization lookup
Verification: unit/component test depending on architecture
```

### 5. Incident or escaped defect

Example:

```text
Observed defect: score = NaN was silently interpreted as NO_MATCH
Revised obligation: non-finite scores must produce explicit invalid-result handling
Local property: NaN/Inf never enter ordinary threshold comparison
Verification: regression unit test plus broader incident replay if needed
```

### 6. Design assumption

Example:

```text
Assumption: sequence numbers are strictly increasing per source
Risk: wraparound or reordering violates downstream state logic
Local property: ordering function handles wraparound according to declared policy
Verification: unit/property test
```

### 7. Fault model / mutation survivor

Example:

```text
Surviving mutant changes >= to > at a safety boundary
Interpretation: existing tests are insensitive to exact-boundary behaviour
Local property: equality at the threshold has declared semantics
Verification: boundary unit test
```

### 8. Standard, policy or assurance constraint

A local rule may be introduced because a project has adopted a mandatory standard or internal policy. It still needs a bounded rationale and applicability statement rather than being treated as universal engineering truth.

## Local property record

A consuming project SHOULD be able to represent the following fields, formally or informally:

```yaml
property_id: LP-...
statement: "..."
source_refs:
  - requirement / hazard / interface / incident / invariant / assumption / fault model
allocated_to: component_or_artifact
scope: "..."
preconditions:
  - "..."
inputs_or_stimuli:
  - "..."
expected_observable:
  - "..."
forbidden_observable:
  - "..."
boundaries:
  - "..."
failure_modes_challenged:
  - "..."
verification_mechanism: unit_test | property_test | contract_test | integration_test | analysis | review | other
oracle_basis: independent_formula | invariant | specification | reference_model | state_contract | metamorphic_relation | other
claim_limit: "What PASS does not establish"
```

The record is deliberately property-centred rather than test-centred.

## Property coherence rule

A useful local property SHOULD be semantically coherent and falsifiable.

Good examples:

- "a non-finite biometric score is rejected as invalid";
- "the state machine cannot transition from CLOSED directly to RUNNING";
- "serialization followed by parsing preserves every value in the supported domain";
- "a normalized non-zero embedding has norm 1 within tolerance";
- "a retryable dependency failure cannot cause more than N attempts";
- "a threshold chosen on validation data cannot later be silently replaced from test data".

Weak examples:

- "test function X";
- "cover line 217";
- "execute method Y";
- "achieve 90% coverage".

The weak statements describe activity or exposure, not a property whose violation would matter.

## Selecting the verification level

The same upstream requirement can generate evidence at several levels. Choose the lowest level that can observe the property **without mocking away the phenomenon of interest**.

### Unit test is appropriate when

- the property is local to a bounded computational/stateful unit;
- dependencies can be controlled without removing the relevant behaviour;
- the oracle can be made precise;
- the failure can be reproduced cheaply and diagnostically.

### Unit test is not sufficient when

- the property emerges only from interaction among components;
- protocol compatibility is itself the object of verification;
- real persistence/database semantics matter;
- timing/concurrency across components matters;
- deployment/configuration is material;
- hardware/network behaviour is part of the property;
- human/operator/system interaction is essential;
- performance, resilience, safety or security depends on system context.

## A property can require multiple tests

Do not force one-to-one cardinality.

```text
one need -> many system properties
one system property -> many allocated obligations
one local property -> several test stimuli / methods
one test -> sometimes evidence for several trace links
```

Examples for one property such as "threshold semantics are correct" may include:

```text
nominal below threshold
exact equality
nominal above threshold
minimum/maximum representable value
NaN/Inf invalid-input behaviour
mutation of >= to >
```

These may be separate tests for diagnosis while remaining one semantic property family.

## Detection adequacy: nominal correctness is not enough

A local property test should be challenged not only with "does the nominal example pass?" but also:

```text
Which plausible violations of this property would this test detect?
Which would escape?
Which inputs/states make the oracle informative?
Could the implementation be wrong while the assertion still passes?
```

Useful techniques include:

- equivalence classes and boundaries;
- decision tables;
- state-transition coverage;
- property-based generation;
- metamorphic testing;
- mutation testing;
- fault injection;
- historical defect replay;
- independent reference calculations;
- robustness/perturbation sweeps.

This is where code coverage becomes a supporting exposure measure rather than the definition of adequacy.

## Bidirectional traceability

Traceability should support two questions:

### Downward

> Why does this local property/test exist?

A path should reach a meaningful source such as a requirement, hazard, interface contract, invariant, assumption, incident or fault model.

### Upward

> What evidence do we have for this business/system intent, and where are the gaps?

A path should aggregate evidence without implying that a local PASS proves an emergent system property.

## Non-goals

This model does NOT require:

- a unit test for every business requirement;
- a requirement identifier on every trivial implementation test;
- 100% traceability as a quality proxy;
- replacing architecture or detailed design with test specifications;
- choosing unit tests when a higher-level phenomenon is the actual object of interest;
- treating tests as the only form of verification.

## Relation to the heuristic falsification lab

`pedagogy/HEURISTIC_FALSIFICATION_LAB.md` provides the learning method for challenging common test doctrines.

This design note provides the complementary production method: derive meaningful properties before selecting a test mechanism, and make explicit what each resulting test can and cannot demonstrate.
