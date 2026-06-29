# {Project} - Docs

Engineering documentation for {Project}: {one-sentence description of what the project does}.

This repository holds reference docs, architecture decisions, and operational runbooks. Runnable code lives in sibling repositories.

## Contents

- [architecture.md](architecture.md) - system architecture, component boundaries, technology choices, and the reasoning behind them. Start here.
- [adr/](adr/) - Architecture Decision Records: one file per significant decision, with context and consequences.
- [runbooks/](runbooks/) - operational procedures: step-by-step instructions for tasks that need to be reproducible under pressure.

## Conventions

- All documents are Markdown.
- Prefer reasoning-first prose: state the decision or procedure, then explain why.
- Keep diagrams in Mermaid so they render directly in the Git host's web UI.