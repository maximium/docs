# Architecture Decision Records

An ADR captures a single architecturally significant decision: the context that forced the choice, what was decided, and the consequences that follow. ADRs are append-only - when a decision is revisited, write a new ADR that supersedes the old one rather than editing history.

## When to write one

Write an ADR when a decision:

- Constrains future work (technology choice, integration boundary, data ownership).
- Reverses or supersedes an earlier decision.
- Was non-obvious enough that someone will ask "why did we do it this way?" six months later.

Do **not** write an ADR for routine implementation choices, bug fixes, or anything reversible within a single PR.

## Format

Each ADR has four sections:

- **Context** - the forces at play: constraints, requirements, alternatives considered.
- **Decision** - what was decided, stated plainly.
- **Consequences** - what becomes easier, what becomes harder, what is now off-limits.
- **Status** - one of `Proposed`, `Accepted`, `Superseded by NNN`, `Deprecated`.

See [000-template.md](000-template.md) for the exact layout.

## Numbering

Files are named `NNN-kebab-case-title.md`, where `NNN` is a zero-padded sequence number starting at `001`. The number is permanent - once assigned, it never changes, even if the ADR is superseded or deprecated.

Example: `007-postgres-as-single-source-of-truth.md`.

## Workflow

1. Copy `000-template.md` to the next available number.
2. Fill in Context, Decision, Consequences. Set Status to `Proposed`.
3. Open a PR. Discuss in review.
4. On merge, flip Status to `Accepted` (or leave as `Proposed` if discussion continues).
5. To revisit later: write a new ADR, set its Status to `Accepted`, and update the old one's Status to `Superseded by NNN`.