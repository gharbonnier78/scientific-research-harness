# Independent review request template

Use this template when a pull request can affect scientific admissibility, execution semantics,
randomness/concurrency guarantees, artifact integrity, gate behavior, or the evidence chain.
The objective is not a cosmetic code review. The reviewer is asked to independently establish
whether the implementation really preserves the declared scientific and governance invariants.

Replace `<...>` placeholders with the consumer-project facts. Remove sections that are truly not
applicable, but do not silently weaken the review standard.

---

## Message to the independent reviewer

**Independent review request — `<project / study / PR>`**

Repository: `<owner/repository>`  
PR: `<number and title>`  
Base: `<base branch @ immutable SHA>`  
Head: `<head branch @ immutable SHA>`  
Pinned harness: `<harness repository @ immutable SHA>`

### Review objective

Please perform an **independent evidence-based review of the pull request itself**.
Do not treat the PR description, author statements, green checks, or previous reviewer conclusions
as proof. Reconstruct the important facts from the repository, executable evidence, logs and frozen
artifacts wherever possible.

The review question is:

> Does this change satisfy its bounded engineering objective while preserving every frozen
> scientific, reproducibility, provenance and governance invariant that is supposed to remain
> unchanged?

### Hard scientific / governance boundaries

The following state must remain unchanged unless this PR is explicitly authorized to mutate it:

- `<claim / estimand / protocol state>`
- `<scientific gate and current status>`
- `<chronicle item and current status>`
- `<historical-data access rule>`
- `<production/execution authorization state>`
- `<other frozen invariant>`

A technical merge **must not be interpreted as scientific execution authorization, gate release,
historical-data access permission, or a new scientific conclusion** unless that exact transition is
both in scope and independently evidenced.

### What must be verified independently

1. **Repository and provenance facts**
   - confirm base/head SHAs and ancestry;
   - confirm the pinned harness/source refs;
   - compare frozen protocol, Chronicle, errata/gate files against the base and verify that any
     claimed non-changes are structurally true;
   - verify cited artifact sizes/digests/event counts when these are part of the evidence chain.

2. **Executable guardrails**
   - run the relevant preflight/gate/validation commands rather than merely reading them;
   - confirm forbidden transitions actually fail under the current state;
   - distinguish a guard that exists in prose from a guard that is effective at runtime.

3. **Core implementation invariants**
   - identify the invariants that would invalidate the scientific interpretation if broken;
   - inspect the implementation path that enforces each invariant;
   - execute the smallest sufficient tests that exercise those paths;
   - prefer exact equality where the contract requires exact replay/equivalence; do not replace it
     with approximate equality without explicit authority.

4. **Randomness / concurrency / replay, when applicable**
   - verify seed lineage is attached to scientific identities rather than scheduling details;
   - verify worker count, chunking, retries and execution order cannot silently change outcomes;
   - verify decomposed/resumed execution reconstructs the same canonical result as the reference
     execution;
   - verify incomplete, duplicate, overlapping, reordered, mixed-config or corrupted artifacts are
     rejected rather than silently accepted.

5. **Scientific evidence versus runtime telemetry**
   - verify progress/logging artifacts cannot accidentally become admissible scientific evidence;
   - verify incomplete/cancelled runs are not reused as outcome evidence unless the protocol
     explicitly allows it;
   - verify intermediate stopping/precision decisions do not materialize or release a final gate
     earlier than permitted.

6. **Degeneracy and failure semantics, when applicable**
   - verify degenerate/failing cases are preserved and audited rather than silently redrawn,
     dropped, replaced or converted into success;
   - verify infrastructure failure is distinguishable from scientific failure.

7. **Authorization boundary**
   - verify what this PR can and cannot authorize after merge;
   - if production execution or a scientific gate remains blocked, execute the relevant preflight
     and confirm that it really blocks;
   - do not authorize a later scientific step merely because the infrastructure review passes.

### Required method

Please go beyond reading the diff when the repository makes execution possible. At minimum:

- inspect the relevant implementation and frozen contracts line-by-line at the critical paths;
- execute the project tests relevant to the bounded claim;
- execute at least one real guard/preflight/reconstruction/replay command where applicable;
- independently recompute hashes/counts/equality claims when they are load-bearing;
- clearly mark any item that could not be executed or independently verified as `NOT_VERIFIED`.

A green CI badge is supporting evidence, not a substitute for the independent checks above.

### Required review output

Do **not** return only “looks good”, “LGTM”, or a prose summary. Return the following structure.

#### A. Independent factual verification

Provide a compact table or equivalent with the important factual claims and one of:

- `CONFIRMED` — independently verified from code/execution/artifacts;
- `REFUTED` — independent evidence contradicts the claim;
- `NOT_VERIFIED` — evidence or access was insufficient.

For load-bearing claims, say **how** you verified them (diff, executed command, reconstructed digest,
exact equality test, etc.).

#### B. Formal review

**1. Verdict**

`APPROVE` or `REQUEST_CHANGES`

`APPROVE` requires no blocking finding and sufficient evidence for every load-bearing invariant.
Unverified critical invariants are blockers unless the governing contract explicitly permits the
uncertainty.

**2. Method actually executed**

State what was really run/read/reconstructed, including relevant test counts and commands. Do not
list intended work as completed work.

**3. Blocking findings**

For each blocker give:

- severity;
- file / code path / artifact;
- violated invariant;
- evidence;
- concrete correction required.

Write `(none)` when there are none.

**4. Non-blocking findings**

For each improvement give severity (`SHOULD_FIX` or similar), location, rationale and concrete
follow-up. A non-blocking finding must not be silently lost: recommend where it should be tracked
if it is intentionally deferred.

**5. Scientific-boundary assessment**

Explicitly confirm or reject every applicable boundary, for example:

- historical scientific scores/data were not read;
- production execution was not performed;
- no scientific conclusion or final gate was produced prematurely;
- incomplete/cancelled runtime evidence was not reused as scientific outcome evidence;
- replay/decomposition semantics remain exact where required;
- production/scientific execution remains unauthorized if it was unauthorized before this PR.

**6. Final recommendation**

State separately whether:

- the engineering/infrastructure PR may be merged;
- deferred non-blocking findings must be completed before production execution;
- a separate Chronicle/governance/authorization change may now be prepared;
- production execution itself is authorized or **not authorized**.

Never collapse “safe to merge infrastructure” into “safe to execute the scientific campaign”.

---

## Reviewer quality gate for the author / orchestrating agent

Before accepting the review as complete, verify that it contains all of the following:

- independent factual verification, not just repetition of the PR description;
- actual execution of relevant tests/guards when executable;
- explicit treatment of the highest-risk scientific invariants;
- `APPROVE` / `REQUEST_CHANGES`;
- blocking and non-blocking findings separated;
- explicit scientific-boundary assessment;
- explicit merge recommendation distinct from execution/gate authorization;
- `NOT_VERIFIED` for anything the reviewer could not independently establish.

If these elements are missing, ask the reviewer to continue rather than treating a partial review as
final approval.
