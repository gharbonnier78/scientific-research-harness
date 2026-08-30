# Claude project bootstrap

Treat the versioned scientific-research-harness as project instructions, not as optional
background reading.

Before substantive work:

1. Read `<HARNESS_ADOPTION_MANIFEST>`.
2. Load the exact immutable harness commit/tag declared there, starting with `HARNESS.md`.
3. Load the companion specifications that apply to the current task.
4. Do not substitute a moving branch, remembered summary or previous conversation for the
   pinned contract.
5. For learning-oriented mathematical work, apply
   `pedagogy/MATHEMATICAL_NOTATION_CAPITALIZATION.md` when non-trivial notation is introduced
   or meaningfully re-encountered, and update the consumer notation registry when warranted.

If the harness is vendored locally, this file may import the pinned local entrypoint with a
Claude Code `@path` import. If it is not vendored, use the available repository/web tooling
to load the immutable URLs recorded by the local manifest.

If the pinned dependency cannot be loaded, say so and do not claim harness compliance or
release a scientific gate.

Keep this file as a short bootstrap into the system of record rather than copying the whole
harness into Claude memory instructions.