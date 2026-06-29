# Keep README.md and CLAUDE.md current

Reusable instruction for AI agents (and humans) working in a repository that has
a `README.md` (contributor-facing) and a `CLAUDE.md` (agent-facing map of the
codebase). After any **significant change**, review both files and update them so
they stay accurate. State this expectation directly in the project's `CLAUDE.md`
so it travels with the repo.

## Why

`README.md` and `CLAUDE.md` are the entry points humans and agents read first.
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

Skip the doc check only for changes with no external surface: internal
refactors that keep the same commands, structure, and behavior; typo fixes;
comment-only edits.

## How to update

1. **Keep the two consistent but not identical.** `README.md` is for
   contributors (how to set up, run, test, contribute). `CLAUDE.md` is the
   agent-facing map (tech stack, key directories, invariants, gotchas). The same
   fact may appear in both, phrased for its audience.
2. **Verify before writing.** Run the command or inspect the file you are
   documenting; don't describe intended behavior, describe actual behavior.
3. **Update tables and indexes too.** Key-directories tables, command lists, and
   "additional documentation" link lists go stale silently - re-read them.
4. **Prefer reasoning-first prose.** State the command/structure/decision, then
   why it is that way, so the next reader can tell whether a future change should
   revise it.

## Checklist

- [ ] Did this change add/remove/rename a command or Make target? → update both.
- [ ] Did the test or lint workflow change? → update both.
- [ ] Did directories or modules move? → update the key-directories table.
- [ ] Did dependencies or the runtime version change? → update tech-stack notes.
- [ ] Did env vars / config change? → update the configuration section.
- [ ] Are README.md and CLAUDE.md still consistent with each other?
