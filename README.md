# Docs

[![Lint](https://github.com/maximium/docs/actions/workflows/lint.yml/badge.svg)](https://github.com/maximium/docs/actions/workflows/lint.yml)

Playbooks and conventions I use across my projects, written down as
read-and-adapt templates instead of remembered.

The docs are not tied to any single project. Point an AI assistant
(or yourself) at these files when setting up a new project or applying a known
convention.

## Contents

- **[playbooks/](playbooks/)** - reusable playbooks / conventions:
  - `000-template.md` - skeleton for a new playbook rule (the shared
    title/lead/`Why`/`How` shape); copy it when adding one.
  - `python-docker-make-lint-test.md` - Docker-based test & lint via Make
    (multi-stage Dockerfile, Make targets, the `.githooks/pre-commit` hook
    plus the `make setup-hooks` activation the agent runs itself,
    `.pre-commit-config.yaml`, gotchas).
  - `run-lint-and-tests-after-changes.md` - run lint/pre-commit + tests after
    changes, fix until green.
  - `keep-test-names-short.md` - test names carry the distinguishing facts
    only (no articles, no "is/are", a noun instead of a verb, nothing the file
    already says), and test-local fakes take plain class names without a
    leading underscore.
  - `comment-what-code-cannot-say.md` - a comment or docstring answers only
    what the code leaves open ("why this way"): no restated signature, no
    retold ADR, no repeated fact, no history of the change.
  - `wait-for-code-review-before-committing.md` - finish and verify changes but
    leave them in the working tree; commit only when the user explicitly says
    so, with a lean commit message.
  - `batch-releases-through-branch.md` - where a merge into the
    default branch deploys, collect several tasks in a short-lived
    `release/<date>` branch and merge it once.
  - `keep-readme-and-agentsmd-current.md` - keep `README.md` / `AGENTS.md` in
    sync after significant changes, with the thin tool-specific pointer
    (`CLAUDE.md`) reviewed alongside them.
  - `suggest-adr-for-architectural-decisions.md` - nudge to record an ADR when
    a repo with `docs/` makes an architecturally significant decision.
  - `run-commands-through-direnv.md` - if the dir (or a parent) has `.envrc`,
    run project commands via `direnv exec <dir> <cmd>` instead of bare tools
    or re-evaluating the environment.
  - `prepare-prod-commands-for-the-user.md` - never mutate production
    yourself: prepare exact commands/scripts and let the user execute them;
    read-only diagnostics only while the user hasn't taken over the
    connections.
  - `new-project-interview-prompt.md` - prompt template for kicking off a new
    app project: feature list, then a structured requirements interview
    (product / FURPS quality attributes / tech / hard constraints), then a
    plan with runnable delivery stages gated on approval.
  - `agents-md-structure.md` - how to write a project's `AGENTS.md`, the
    tool-neutral map every coding agent reads: the six sections, a soft
    150-line budget, tool-specific files (`CLAUDE.md` and friends) kept to
    a thin pointer, deeper topics pushed into `docs/` and indexed from the
    map, references by filename and symbol instead of code snippets, plus
    the verification steps.
- **[skills/](skills/)** - Claude Code skills kept under version control here
  and symlinked into `~/.claude/skills/` (install on a new machine with
  `ln -s "$(pwd)/skills/<name>" ~/.claude/skills/<name>` from the repo root):
  - `new-project/` - `/new-project` slash command: structured requirements
    interview per `playbooks/new-project-interview-prompt.md`, then a plan
    with delivery stages, then project setup from the scaffolds in this repo.
  - `agents-md/` - `/agents-md` slash command: write or restructure a
    repository's `AGENTS.md` (plus any thin tool-specific file such as
    `CLAUDE.md`) per `playbooks/agents-md-structure.md`, then report the
    line count and the lint result without committing.
- **[docs-template/](docs-template/)** - scaffold for a project's engineering
  docs: `architecture.md`, `adr/` (Architecture Decision Records), `runbooks/`.
  Copy and adapt it when standing up docs for a project. Works both as a
  standalone docs repository (e.g. for a group of microservices) and as an
  in-project `docs/` directory.

## Usage

These are read-and-adapt templates, not a framework to install. Typical flow:
tell the assistant to use a file here as the template, then copy/adjust it into
the target project.
