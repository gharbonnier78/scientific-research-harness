# Agent bootstrap templates

These templates make the harness discoverable by agent runtimes without copying the whole
contract into every prompt.

- `AGENTS.md` is the preferred short repository map for OpenAI/Codex-style repository agents.
- `CLAUDE.md` is the corresponding Claude Code project-memory bootstrap.
- A consumer may add other vendor-specific thin wrappers, but all wrappers should point to
  the same local adoption manifest and immutable harness ref.

The pattern is deliberately:

```text
automatically discovered small instruction file
                 |
                 v
       local harness-adoption manifest
                 |
                 v
 immutable scientific-research-harness ref
                 |
                 +--> task-relevant companion contracts
                 |
                 +--> consumer-local extensions
```

Do not turn the bootstrap into a giant duplicated instruction file. The harness is a
versioned system of record and should remain inspectable, pinnable and reviewable.

Automatic discovery is runtime-specific. Repository files cannot force every generic chat
surface to read them. For environments without repository-instruction discovery, configure
the project/workspace/agent itself to load the same bootstrap at startup.