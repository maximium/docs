# Keep README.md and AGENTS.md current

In a repository with a `README.md` (contributor-facing) and an `AGENTS.md`
(agent-facing map of the codebase, read by every coding agent), review both
files after any **significant change** and update them so they stay accurate.
State this expectation directly in the project's `AGENTS.md` so it travels with
the repo, and treat any thin tool-specific pointer (`CLAUDE.md` and friends, per
[agents-md-structure.md](agents-md-structure.md)) as part of the same review.

## Why

`README.md` and `AGENTS.md` are the entry points humans and agents read first.
When they drift from reality, they cost more than no docs: contributors run
commands that no longer exist, and agents plan against a map that is wrong. Docs
are part of the change, not a follow-up - update them in the same commit.

## When to update (what counts as "significant")

Treat a change as significant (and check both files) when it touches any of:

- **Commands or Make targets** - added, removed, renamed, or changed behavior
  (e.g. how `make test` / `make lint` run).
- **Test or lint workflow** - how tests run, where fixtures live, naming
  conventions, what the lint pipeline includes.
- **Directory structure** - new, moved, or removed top-level directories or
  modules; anything that invalidates a "key directories" table.
- **Dependencies / runtime** - new dependency, dependency removal, or a
  language/runtime version bump (e.g. Python 3.10 → 3.11).
- **Configuration** - new or changed environment variables, config profiles, or
  settings files.
- **Architecture** - a new pattern, data-flow change, or a decision that the
  existing docs describe differently.
- **Agent tooling** - a new skill, hook, permission, or MCP server, which
  belongs in the tool-specific file (`CLAUDE.md`), not in `AGENTS.md`.

Skip the doc check only for changes with no external surface: internal
refactors that keep the same commands, structure, and behavior; typo fixes;
comment-only edits.

## How to update

1. **Keep the two consistent but not identical.** `README.md` is for
   contributors (how to set up, run, test, contribute). `AGENTS.md` is the
   agent-facing map (tech stack, key directories, invariants, gotchas). The same
   fact may appear in both, phrased for its audience.
2. **Edit the fact where it lives, once.** Tool-neutral facts belong in
   `AGENTS.md`; a tool-specific file holds only the pointer plus its own
   surface. Updating a rule in `CLAUDE.md` that `AGENTS.md` owns creates the
   second copy the split exists to avoid.
3. **Verify before writing.** Run the command or inspect the file you are
   documenting; don't describe intended behavior, describe actual behavior.
4. **Update tables and indexes too.** Key-directories tables, command lists, and
   "additional documentation" link lists go stale silently - re-read them.
5. **Watch the budget.** `AGENTS.md` is loaded every session, so an update that
   pushes it well past ~150 lines is a signal to move a topic into `docs/` and
   index it, not to let the map grow.
6. **Prefer reasoning-first prose.** State the command/structure/decision, then
   why it is that way, so the next reader can tell whether a future change should
   revise it.

## Checklist

- [ ] Did this change add/remove/rename a command or Make target? → update both.
- [ ] Did the test or lint workflow change? → update both.
- [ ] Did directories or modules move? → update the key-directories table.
- [ ] Did dependencies or the runtime version change? → update tech-stack notes.
- [ ] Did env vars / config change? → update the configuration section.
- [ ] Did agent tooling (skills, hooks, MCP servers) change? → update the
      tool-specific file, not `AGENTS.md`.
- [ ] Are README.md and AGENTS.md still consistent with each other?
