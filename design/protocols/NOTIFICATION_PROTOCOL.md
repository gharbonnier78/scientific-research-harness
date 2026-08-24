# Notification and Attention Protocol

## Purpose

Define how external events attract human attention without becoming scientific authority or silently triggering autonomous reasoning.

## Principle

Notifications are an **attention interface**, not a scientific state store.

Canonical state remains in project repositories and evidence artifacts.

## Candidate attention states

- `needs_scientist_attention`
- `needs_reviewer_attention`
- `experiment_completed`
- `review_stale`
- `evidence_changed`
- `human_arbitration_required`

## Event sources

Examples include:

- GitHub Actions workflow completion;
- pull request update;
- review submission;
- new experiment artifact;
- claim/evidence file change;
- explicit human request.

## Default behavior

An event MAY:

1. update derived attention state;
2. generate a human notification;
3. prepare context for a future scientific session;
4. queue a requested bounded action.

An event MUST NOT, by default:

- start an open-ended Scientist/Reviewer loop;
- close a scientific finding;
- change a scientific conclusion;
- publish or merge scientific output.

## Human response model

A notification channel MAY accept short intents such as:

- `status`
- `show changes`
- `review 27`
- `check R-014`
- `pause`
- `ignore`

These responses may either execute deterministic read-only operations or queue an intent for the next Scientist/Reviewer session.

## Idempotency

Webhook/event processing SHOULD record an event identifier and derived state transition so duplicate deliveries do not create duplicate actions or notifications.

## Portability

The notification protocol is channel-independent. Slack, Discord, email, mobile push, or another channel may implement the same attention semantics.
