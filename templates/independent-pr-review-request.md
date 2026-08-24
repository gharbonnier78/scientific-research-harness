# Independent PR review request template

Use this template when delegating a pull-request review to another LLM, agent, reviewer, or external tool.

## Required navigation block

A review request MUST provide direct, canonical URLs for the material the reviewer is expected to inspect. Do not assume that repository search, indexing, prior chat context, or connector discovery will find the target.

At minimum include:

- Repository URL: `https://github.com/<owner>/<repo>`
- Pull request URL: `https://github.com/<owner>/<repo>/pull/<number>`
- Base branch or base commit URL when it matters to the review
- Head branch or head commit URL when it matters to the review
- Pinned harness URL at the exact immutable commit/tag used by the consumer
- Direct URLs for any non-obvious normative artifacts that the reviewer must read and cannot reliably discover from the PR

Prefer immutable blob/commit URLs for normative references. A moving `main` URL may be supplied for navigation, but it MUST NOT replace the pinned immutable reference used to judge compliance.

## Reviewer instruction

The reviewer SHOULD begin from the direct PR URL, then verify the actual diff, repository instructions, pinned harness, referenced artifacts, and current CI/status evidence. It MUST NOT treat the review prompt's summary as evidence when repository evidence is available.

If any required URL cannot be opened, the reviewer MUST report the inaccessible object and MUST NOT silently substitute search snippets, memory, or prose summaries for the missing evidence.

## Minimal example

```text
Repository: https://github.com/acme/research-repo
PR: https://github.com/acme/research-repo/pull/42
Base: https://github.com/acme/research-repo/tree/main
Head commit: https://github.com/acme/research-repo/commit/<sha>
Pinned harness: https://github.com/gharbonnier78/scientific-research-harness/blob/<sha>/HARNESS.md
Local adoption manifest: https://github.com/acme/research-repo/blob/<head-sha>/harness-adoption.yaml
```

The navigation block is part of reproducible review handoff: a reviewer who cannot locate the evidence cannot independently verify the claim.
