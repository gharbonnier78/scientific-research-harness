# Telemetry Evidence Contract

Status: draft reusable engineering-evidence contract

## Purpose and boundary

Runtime telemetry is not automatically evidence merely because it exists. A project MAY promote runtime telemetry to **engineering/test evidence** when the signals are checked against a predeclared contract and the raw correlated signals remain recoverable.

This promotion does **not** make runtime telemetry scientific evidence. Scientific evidence, runtime telemetry, test evidence and explanatory material remain distinct provenance classes even when they are linked in one evidence pack.

## Authoritative specification

When OpenTelemetry is used as evidence, the project MUST identify the applicable OpenTelemetry specification and semantic-convention version or immutable reference used to define signal format and meaning. The authoritative upstream sources are the OpenTelemetry specification and semantic conventions, including:

- OTLP: https://opentelemetry.io/docs/specs/otlp/
- Trace API/data model: https://opentelemetry.io/docs/specs/otel/trace/
- Logs: https://opentelemetry.io/docs/specs/otel/logs/
- Metrics data model and exemplars: https://opentelemetry.io/docs/specs/otel/metrics/data-model/
- Semantic conventions: https://opentelemetry.io/docs/specs/semconv/

A local convention MUST NOT silently redefine a standardized OpenTelemetry attribute, span kind, signal field or unit. Project-specific attributes and signal names SHOULD use a bounded project namespace.

## Model-derived expected topology

When a behavioral or system model is authoritative for the tested path, the project SHOULD derive an **expected telemetry model** from that model before execution. UML activity models, sequence models, state machines or equivalent machine-readable process models are acceptable sources.

The mapping MUST be explicit rather than assuming that every model element is a span:

- an operation with meaningful duration MAY map to a span;
- nested operations SHOULD map to parent/child span topology where the causal structure is hierarchical;
- instantaneous state changes or decisions SHOULD normally map to span events or structured logs rather than zero-value spans;
- protocol operations SHOULD use the applicable OpenTelemetry semantic conventions;
- concurrent or non-hierarchical causality MAY require links rather than invented parentage;
- measurements intended for aggregation SHOULD use metrics;
- an individual metric observation correlated to the tested path SHOULD use an exemplar when the metric data model supports it.

The expected model SHOULD declare cardinality, parent/child or link relationships, required attributes, logs/events, metrics, values/ranges and prohibited signals needed for the bounded test claim.

## SpanId as an evidence entry handle

A project MAY use a `SpanId` as the primary evidence-query handle for a tested operation or scenario. The evidence service MUST resolve and preserve the associated `TraceId` and MUST be able to recover the selected span plus the required descendant/subspan topology.

For a subtree evidence query, the recoverable bundle SHOULD include:

- entry `SpanId` and associated `TraceId`;
- entry span and required descendants, including parent identifiers;
- correlated LogRecords using trace/span context;
- metric observations relevant to the bounded test, including correlated exemplars where required;
- the raw returned telemetry or an immutable reference to it;
- query/backend identity and time bounds when the backend requires them.

Whole-trace lookup by `TraceId` SHOULD remain available when global investigation is required.

## Expected-versus-actual verification

A telemetry-backed E2E test MUST preserve both the expected signal contract and the actual recovered evidence. Its verdict MUST be based on an explicit comparison, not on dashboard inspection.

At minimum, when applicable, verification SHOULD check:

- expected span presence and cardinality;
- parent/child or link topology;
- required semantic-convention and project attributes;
- expected structured logs/events and their trace/span correlation;
- absence or bounded count of prohibited/error signals;
- expected metrics, values/ranges and units;
- exemplar correlation for individual measurements used as path evidence;
- unexpected material spans or signals when the contract declares a closed topology.

The evidence pack SHOULD contain a human-readable expected-vs-actual view and a machine-readable verdict.

## Sampling, completeness and failure classification

If a test requires telemetry completeness, its qualification profile MUST guarantee collection of the required signals. A trace sampler, log filter, metric aggregation rule or retention policy MUST NOT silently remove signals that the test contract requires.

A missing required signal does not automatically prove product failure. The test system MUST distinguish at least:

- `PRODUCT_BEHAVIOR_FAILURE`: externally observable product behavior violates the test oracle;
- `TELEMETRY_CONTRACT_FAILURE`: the product ran but emitted wrong/missing required telemetry;
- `TELEMETRY_PIPELINE_FAILURE`: required emitted signals could not be collected, stored or queried reliably;
- `TEST_INFRASTRUCTURE_FAILURE`: the test harness itself could not establish a valid execution.

The classification and supporting raw evidence MUST remain recoverable.

## Evidence retention

For a passing or failing qualification test, the retained evidence SHOULD be sufficient for an accountable reviewer to answer:

1. What behavior was expected?
2. What telemetry topology and values were expected?
3. Which SpanId/TraceId identifies the observed path?
4. Which exact query recovered the evidence?
5. What spans, logs/events and metrics were actually returned?
6. Which comparisons passed or failed?
7. Was any signal missing because of sampling, collection or backend behavior?

A screenshot of an observability dashboard alone is not sufficient evidence for this contract.
