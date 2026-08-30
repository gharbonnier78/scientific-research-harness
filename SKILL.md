# Scientific Research Harness — portable skill entrypoint

This file is a compatibility shim for agent environments that discover a repository-level
`SKILL.md`.

The normative contract is **not duplicated here**. Load and obey:

1. `HARNESS.md` — canonical harness entrypoint.
2. The consumer project's pinned `harness-adoption.yaml` (or equivalent local manifest).
3. The companion specifications referenced by `HARNESS.md` that apply to the current task.
4. Any stricter consumer-local instructions that do not weaken the pinned harness.

For learning-oriented mathematics, also apply
`pedagogy/MATHEMATICAL_NOTATION_CAPITALIZATION.md` when non-trivial notation is introduced
or meaningfully re-encountered.

If the pinned harness or local manifest cannot be loaded, say so explicitly. Do not claim
harness compliance or release a scientific gate from memory, a moving branch, or a prose
summary.

This shim exists so tools can discover the harness; `HARNESS.md` remains the normative
source of process requirements.