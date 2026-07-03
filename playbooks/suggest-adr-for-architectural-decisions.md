# Suggest an ADR for architectural decisions

Reusable instruction for AI agents working in a repository that keeps
engineering docs in the `docs-template` spirit: a `docs/` directory inside the
project, or a standalone docs repository for a group of services, containing
any subset of `architecture.md`, `adr/`, `runbooks/`. The full trio is not
required.

When a change embodies an **architecturally significant decision** (one that
constrains future work, reverses an earlier decision, or is non-obvious enough
that someone will later ask "why did we do it this way?"), nudge the developer
to record an **ADR** in the `adr/` directory (`docs/adr/` in-project, top-level
`adr/` in a standalone docs repo).

## How

- **Trigger on the docs convention, not every repo.** Any subset of the
  `docs-template` layout counts: a `docs/` directory with at least one of
  `architecture.md`, `adr/`, `runbooks/`, or the current repo itself being a
  standalone docs repo. A missing `adr/` is no reason to stay silent: if the
  decision warrants it, the nudge is to create `adr/` with the first ADR.
  Otherwise, stay silent, since there is no docs convention to hook into.
- **Nudge, don't auto-write.** The ADR captures the developer's reasoning and is
  their call. Offer to draft it, but the decision and its framing stay theirs.
- **Skip the routine.** Bugfixes, refactors, dependency bumps, and reversible
  implementation choices are not ADR-worthy. (See `docs-template/adr/README.md`
  for the full "when to write one")
- **In-flow judgment, not monitoring.** Notice this while working on the change
  itself. Do not scan the codebase or re-read every doc to hunt for it.

The same idea applies to `architecture.md`: an architectural change may warrant
refreshing its "Key decisions" summary, but that stays a developer call, flagged
in-flow, not an automatic sync like `README.md` or `CLAUDE.md`.
