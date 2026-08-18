# SDR-004 — Separate scientific state, curated context, connectors, and attention

## Status

Proposed

## Context

The research harness is evolving toward a reusable conversational workflow where a human can ask heterogeneous LLMs to resume, review, challenge, or inspect scientific work using short commands.

Several concerns can easily become conflated:

- accessing GitHub technically;
- storing canonical scientific project state;
- curating internal/external references for LLM use;
- defining Scientist/Reviewer behavior;
- notifying a human about long-running work or stale review;
- exposing resources through a human-facing interface such as Diderot.

Combining these concerns into one MCP service or one autonomous orchestrator would create unnecessary coupling and obscure scientific authority.

## Decision

Adopt the following separation of responsibilities:

1. **Project repository / GitHub** stores canonical scientific state, code, evidence pointers, PRs, reviews, commits and generated artifacts.
2. **Provider connectors / GitHub Apps** provide technical access to provider APIs and events.
3. **Scientific Research Harness** defines portable interaction, Scientist and Reviewer contracts, study manifests, schemas and resume behavior.
4. **Context Fabric** provides a curated, provenance-aware resource and analysis layer. It may reference a GitHub repository or a precise repository location, but it does not reimplement a generic GitHub connector.
5. **Notification channels** expose derived attention state and optional queued intents but do not own scientific state or conclusions.
6. **Human-facing libraries such as Diderot** consume the curated resource layer dynamically and remain read-oriented by default.

The default scientific interaction is human-triggered and bounded. Webhooks may observe and notify, but do not start open-ended LLM-to-LLM reasoning loops by default.

## Consequences

### Positive

- Components have clear authority boundaries.
- Existing GitHub connectors can be reused rather than duplicated.
- The Context Fabric remains provider-agnostic and epistemically focused.
- Scientist and Reviewer roles remain portable across LLM vendors.
- A study can be resumed without trusting chat memory.
- Human oversight remains visible and meaningful.
- Notification channels can evolve independently.

### Negative / cost

- Cross-component identifiers and schemas must be kept coherent.
- The system does not provide full autonomous orchestration out of the box.
- Some state must be derived at session start instead of being held in a central orchestrator.

## Rejected alternatives

### Context Fabric as a generic GitHub connector

Rejected because it duplicates provider access logic and mixes technical retrieval with scientific curation.

### Fully autonomous Scientist ↔ Reviewer loop

Rejected as the default because repeated model agreement is not scientific evidence and may optimize for convergence rather than falsification.

### Conversation history as project state

Rejected because it is non-portable across models and sessions and is insufficiently reproducible.

## Validation

The first validation gate is cross-model session reconstruction: independent LLM sessions should execute `START <study>` and derive materially equivalent checkpoints from canonical project and context sources.
