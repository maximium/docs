# Suggest an ADR for architectural decisions

Reusable instruction for AI agents working in a repository that keeps
engineering docs in a `docs/` directory (the `docs-template` layout:
`architecture.md`, `adr/`, `runbooks/`).

When a change embodies an **architecturally significant decision** (one that
constrains future work, reverses an earlier decision, or is non-obvious enough
that someone will later ask "why did we do it this way?"), nudge the developer
to record an **ADR** under `docs/adr/`.

## How

- **Trigger on the docs convention, not every repo.** Only when the project has
  a `docs/` directory; otherwise stay silent, since there is no ADR convention
  there.
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
