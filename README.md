# Docs

Playbooks and conventions I use across my projects, written down as
read-and-adapt templates instead of remembered.

The docs are not tied to any single project. Point an AI assistant
(or yourself) at these files when setting up a new project or applying a known
convention.

## Contents

- **[playbooks/](playbooks/)** - reusable playbooks / conventions:
  - `python-docker-make-lint-test.md` - Docker-based test & lint via Make
    (multi-stage Dockerfile, Make targets, the `.githooks/pre-commit` hook,
    `.pre-commit-config.yaml`, gotchas).
  - `run-lint-and-tests-after-changes.md` - run lint/pre-commit + tests after
    changes, fix until green.
  - `keep-readme-and-claudemd-current.md` - keep `README.md` / `CLAUDE.md` in
    sync after significant changes.
- **[docs-template/](docs-template/)** - scaffold for a project's engineering
  docs: `architecture.md`, `adr/` (Architecture Decision Records), `runbooks/`.
  Copy and adapt it when standing up docs for a project.

## Usage

These are read-and-adapt templates, not a framework to install. Typical flow:
tell the assistant to use a file here as the template, then copy/adjust it into
the target project.
