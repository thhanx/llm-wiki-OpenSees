# llm-wiki-OpenSees

This repository is an LLM-friendly collection of information and documentation about OpenSees software.

It follows an LLM-maintained wiki pattern:

- `raw/` stores immutable source material.
- `wiki/` stores generated, maintained Markdown knowledge pages.
- `AGENTS.md` defines the operating rules for Claude and other LLM agents.

Start with [wiki/index.md](wiki/index.md) for the content catalog and [wiki/log.md](wiki/log.md) for the chronological maintenance record.

Local Claude Code skills for this repository live under `.claude/skills/`:

- `llm-wiki-ingest`
- `llm-wiki-query`
- `llm-wiki-lint`

Unless otherwise noted, the contents of this repository are licensed under CC BY-NC 4.0.
Commercial use is not permitted.
