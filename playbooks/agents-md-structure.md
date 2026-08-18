# Write a project's AGENTS.md

Setup recipe for creating a project's `AGENTS.md`: the agent-facing map any
coding agent reads before anything else. The goal is a short, verified
orientation file that is tool-neutral, with deeper notes kept in the repo's
own docs directory and indexed, and with each tool's own file reduced to a
pointer at `AGENTS.md`. It is not a second README and not a style guide.

## When to use this

- A repository has no agent instructions at all, or has a `CLAUDE.md` (or
  another tool-specific file) that grew into a dump of everything anyone
  ever wanted the agent to know.
- You are restructuring existing agent instructions to fit the budget
  below, or consolidating several tool-specific files into one.

In Claude Code this recipe is wrapped as the `/agents-md` skill. In an agent
that cannot read this repository, hand it this file (a copy, or its raw URL)
and tell it to follow the recipe; do not paraphrase the rules into a prompt,
because the paraphrase is a second copy that drifts.

Several tools ship a built-in "init" that writes their own file (Claude
Code has `/init`). This recipe differs on purpose: one tool-neutral file
as the source of truth, a size budget, deeper material pushed into
`docs/` instead of inlined, references by filename and symbol instead of
code snippets, and no formatting rules at all (linters own those).
Keeping an existing file accurate afterwards is a different job, covered
by [keep-readme-and-agentsmd-current.md](keep-readme-and-agentsmd-current.md).

## Philosophy

`AGENTS.md` is loaded into every session, so every line is paid for on
every request, whether or not it turns out to be relevant. That single
fact drives the whole shape:

- **A map, not a manual.** It answers what the project is, what it is
  built from, where things live, and how to build/test/lint it. Anything a
  session needs only sometimes is a link, not a paragraph.
- **One file, every agent.** `AGENTS.md` is the format the coding agents
  converge on (see agents.md), so a rule written once is read by all of
  them. Do not fork the content per tool; fork only the pointer.
- **About 150 lines is the budget**, and it is a soft one: a file that
  lands at 160 lines is fine, one at 300 has stopped being a map. Treat
  it as the number that triggers a look for what to cut or move, not as a
  limit to enforce. It is also not a target to fill: a small repo with a
  20-line file is a good outcome.
- **The budget is advice to the author, not project content.** Do not
  write the line count, this recipe, or any rule about how to maintain
  the file into `AGENTS.md` itself. The file holds facts about the
  project; meta-rules about the file spend context on every request and
  earn nothing.
- **Linters own style.** Naming, formatting, import order, line width:
  none of it belongs here. If the project cannot enforce a rule
  mechanically, that is an argument for adding the linter, not for
  spending context on the rule.
- **Verified, not intended.** Every command listed must have been run, and
  every path must exist. A wrong map is worse than no map, because the
  agent plans against it confidently.

## Structure

Six sections, in this order, all of them in `AGENTS.md`. Drop any that the
repo does not earn.

1. **Project overview** - two to four sentences: what the project does,
   who or what consumes it, and why it exists. The "why" is the part an
   agent cannot infer from the code.
2. **Tech stack** - languages, frameworks, runtime versions, the test and
   lint tooling, and where each is configured.
3. **Key directories** - one line per top-level directory, saying what it
   owns. Use a bullet list rather than a Markdown table: table rows cannot
   be wrapped, so they blow the line-width rule in any repo that has one.
4. **Essential commands** - build, test, lint, run, in a fenced block.
   Prefer the project's own entry points (`make test`, a script) over raw
   tool invocations, because those mirror CI.
5. **Project-specific rules and invariants** - the handful of things that
   are true here and not elsewhere: a required workflow, a boundary that
   must not be crossed, a known trap. Not general good practice.
6. **Additional documentation** - the index into the deeper files, and any
   other file worth reading on demand. One line each, and each line says
   *when* to read it, not just what it is.

## Tool-specific files point at AGENTS.md

Where a tool looks for its own file, keep that file thin: a pointer to
`AGENTS.md` plus only what that tool alone has. For Claude Code the
pointer is an import:

```markdown
# CLAUDE.md

@AGENTS.md

## Claude Code specifics

- `.claude/docs/architecture.md` - read before changing the ingest path.
```

1. **AGENTS.md holds everything tool-neutral**: all six sections above.
   That is the file to edit when a rule changes, whichever agent is in
   use.
2. **The tool file holds the pointer plus its own surface**: for Claude
   Code, the `.claude/docs/` index, skills, hooks, permissions, MCP
   servers. When there is nothing tool-specific, the file is a single
   import line, and that is a fine outcome.
3. **Mind the Claude Code syntax.** The import is a bare `@path` line:
   relative (`@AGENTS.md`, `@docs/testing.md`) or home-anchored
   (`@~/.claude/...`). It is not resolved inside fenced or inline code,
   which is why the example above stays an example. Imports nest up to
   five levels deep, and `/memory` in a session shows what actually
   loaded.
4. **Other tools, other mechanics.** Editors that read their own path
   (`.github/copilot-instructions.md`, a rules file under `.cursor/`)
   usually have no import syntax, so the file says in one line that
   `AGENTS.md` is the source of truth and to read it. Check the tool's
   current docs, because these paths move.
5. **The budget covers the whole tree.** An import is pulled into the same
   context, so the ~150 lines is the sum of what a session loads, not a
   per-file allowance.
6. **Never state a fact twice.** A rule lives in exactly one file. If the
   repository already has an `AGENTS.md`, do not restate its contents
   anywhere else; point at it and stop.
7. **Skip the second file when it buys nothing.** A repository whose
   agents all read `AGENTS.md` needs no tool-specific file at all; each
   extra one is another thing to keep in sync.

## Progressive disclosure into docs/

The point is to keep the always-loaded file small while leaving the depth
reachable.

1. **Extract only what repeats and does not fit.** A topic earns its own
   file when it shows up across several files or sessions and would
   otherwise crowd the budget: architectural patterns, a testing strategy,
   a data model, a deployment flow. A pattern visible in one file is a
   comment, not a document.
2. **Put it where the repo already keeps docs.** Use the project's `docs/`
   directory so humans and every agent find the same material. Reserve
   `.claude/docs/` for notes that are genuinely Claude Code specific, and
   index those from `CLAUDE.md` rather than `AGENTS.md`.
3. **Do not manufacture the files.** A thin or young repository often
   needs no such directory at all. Inventing one produces a document that
   restates the README at length, which then rots. Say the repo does not
   need it rather than filling the directory.
4. **Reference, do not quote.** Point at `path/to/file.py` and
   `ClassName.method` instead of pasting code. Snippets go stale silently;
   a path that no longer exists is at least obvious.
5. **Link both ways.** Every extracted file must be listed in the
   `Additional documentation` section, and nothing may be listed there
   that does not exist. That index is the only way an agent learns the
   file is available.
6. **Decide what git tracks.** Shared project context belongs in version
   control. Keep personal overrides (`.claude/settings.local.json` and the
   equivalents) ignored, as usual.

## Verify before finishing

1. **Run what you wrote.** Each documented command, once. Drop or fix any
   that fails.
2. **Lint the new files.** They are untracked, so `pre-commit run
   --all-files` silently skips them: stage them (`git add -N`) or call the
   linter on the paths directly.
3. **Check the width.** In repos that enforce a line length, the usual
   offenders are Markdown tables and long fenced commands.
4. **Count the lines**, summed over `AGENTS.md` and anything a session
   pulls in with it. Well over 150 means something belongs in `docs/` or
   does not belong at all.
5. **Confirm the pointer resolved.** In Claude Code, `/memory` lists the
   files that actually loaded; an `@AGENTS.md` that silently failed to
   resolve leaves the session with no rules at all.
6. **Leave it uncommitted.** Per
   [wait-for-code-review-before-committing.md](wait-for-code-review-before-committing.md),
   report the result and let the user review the map before it lands.

## When to skip

If the repository is small enough that its `README.md` already answers
what, why, and how, an `AGENTS.md` adds a second thing to keep in sync for
no gain: say so instead of writing one. If the project already has a house
convention for its agent file, follow that convention rather than this
recipe.
