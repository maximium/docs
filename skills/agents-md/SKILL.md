---
name: agents-md
description: Write or restructure a repository's agent instructions,
   the tool-neutral AGENTS.md map plus the thin tool-specific files that
   point at it (CLAUDE.md and friends), under a soft 150-line budget with
   deeper topics pushed into docs/.
   Use whenever the user invokes /agents-md, or asks to create, generate,
   or rewrite an AGENTS.md or CLAUDE.md for a project.
   Do not use for a small edit to existing, accurate agent instructions;
   that is an ordinary file change.
---

# Agent instructions authoring

This skill is a thin pointer into the reference docs repo. Resolve the real
location with `readlink -f ~/.claude/skills/agents-md/SKILL.md` (the file is
symlinked into `~/.claude/skills/`); the repo root is two directories up
from the resolved path. If the repo is not on this machine, fall back to
`https://raw.githubusercontent.com/maximium/docs/main/playbooks/`. The repo
holds the single source of truth; do not duplicate its content, and if this
file and the repo ever disagree, the local repo wins (it may be ahead of
`main`).

1. Read `playbooks/agents-md-structure.md` (path relative to the repo
   root) and follow it: the six sections, the soft 150-line budget, the
   rule that `AGENTS.md` is tool-neutral while each tool's own file stays
   a thin pointer (for Claude Code, an `@AGENTS.md` import plus what only
   Claude Code has), the rules for extracting a topic into `docs/`, and
   the verification steps.
2. Treat the skill arguments ($ARGUMENTS) as the scope: a subdirectory or
   monorepo package to document instead of the repo root, or an emphasis
   the user wants covered. With no arguments, document the current
   repository root.
3. Explore before writing. The map must describe what is there, so read
   the build/test configuration and the top-level directories first, and
   run each command you intend to document.
4. Leave the result in the working tree and report the line count and the
   lint result. Do not commit, per
   `playbooks/wait-for-code-review-before-committing.md`. Consult the
   repo's `README.md` index for related playbooks.
