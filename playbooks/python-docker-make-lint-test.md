# Python project: Docker-based test & lint via Make

Reusable setup for running **tests and linters entirely inside Docker**, so
contributors (and CI) need no local Python, DB drivers, or toolchain - only
Docker. Copy the templates below into a new project and adjust the names.

Project-agnostic; replace `<project>` and the Python version as needed.

## Philosophy

- **Docker is the source of truth.** `make test` / `make lint` build images that
  mirror CI. Local machines may have no Python at all.
- **One toolchain version per Python.** Linters tied to the AST (flake8/pyflakes,
  black) break on newer Python (e.g. `ast.Str` removed in 3.12). When you bump
  the Python version, bump the pre-commit hook versions in the same change.
- **Lint image is tools-only; the repo is bind-mounted** at run time, so hooks
  check the live working tree without rebuilding on every code change.

## Dockerfile (multi-stage: base / test / lint)

```dockerfile
# syntax=docker/dockerfile:1
FROM python:3.14-slim AS base

WORKDIR /app
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
RUN pip install --upgrade pip


FROM base AS test

COPY . /app
RUN pip install .[test]

CMD ["python", "-m", "pytest", "tests/", "-v"]


# Lint image: tools only - the repo is bind-mounted at run time so pre-commit
# checks the working tree. git is required by pre-commit (it clones hook repos);
# safe.directory avoids the dubious-ownership error on the mounted .git.
FROM base AS lint

RUN apt-get update \
    && apt-get install -y --no-install-recommends git \
    && rm -rf /var/lib/apt/lists/*
RUN pip install pre-commit
RUN git config --global --add safe.directory /app

CMD ["pre-commit", "run", "--all-files"]
```

**Mount path must match `safe.directory` (and ideally `WORKDIR`).** The lint
path appears in three places - the Makefile's `-v $(pwd):/app` and `-w /app`,
and the Dockerfile's `safe.directory /app`. They must be the *same* path:
`safe.directory` is path-specific, so a mismatch makes pre-commit's git step
fail with "dubious ownership" (or only work by luck when the stage runs as
root). The path itself is a free choice - the lint image bakes no code, so the
bind-mount is the only content there - so just pick one (`/app`, matching
`WORKDIR`) and use it consistently. Avoid inventing a second path like `/repo`:
it buys nothing and adds one more thing to keep in sync.

## Makefile

```makefile
TEST_IMAGE := <project>-test
LINT_IMAGE := <project>-lint
PRECOMMIT_CACHE := $(shell pwd)/.pre-commit-cache

.PHONY: build test lint setup-hooks

build:
	DOCKER_BUILDKIT=1 docker build --target test -t $(TEST_IMAGE) .

test: build
	docker run --rm $(TEST_IMAGE)

# Run once after cloning: enable the pre-commit hook (runs `make lint` in
# Docker on every commit). See ".githooks/pre-commit" below.
setup-hooks:
	git config core.hooksPath .githooks
	chmod +x .githooks/pre-commit

# Sentinel: rebuild the lint image only when the Dockerfile changes.
.lint-image: Dockerfile
	DOCKER_BUILDKIT=1 docker build --target lint -t $(LINT_IMAGE) .
	touch .lint-image

lint: .lint-image
	@mkdir -p $(PRECOMMIT_CACHE)
	docker run --rm \
		-v $(shell pwd):/app \
		-v $(PRECOMMIT_CACHE):/root/.cache/pre-commit \
		-w /app \
		$(LINT_IMAGE) \
		pre-commit run --all-files
```

Notes:
- The `.lint-image` sentinel skips rebuilds unless the Dockerfile changes;
  hook versions are handled by pre-commit at run time (no rebuild needed).
- `$(PRECOMMIT_CACHE)` persists hook environments between runs (first run is
  slow as pre-commit clones and installs each hook).

## Git pre-commit hook (.githooks/pre-commit)

`.pre-commit-config.yaml` alone does **not** install a git hook - without one,
commits never run lint. The usual `pre-commit install` writes a hook that
invokes pre-commit **locally**, which defeats the Docker-only philosophy (it
needs local Python + hook tools). Instead, commit a tiny tracked hook that just
calls `make lint` (which runs pre-commit inside Docker), and point git at it via
`core.hooksPath`:

```sh
# .githooks/pre-commit  (chmod +x, tracked in the repo)
#!/bin/sh
make lint
```

Activate once after cloning with `make setup-hooks` (target above). Because the
hook lives in a tracked `.githooks/` dir (not `.git/hooks/`), it travels with
the repo; each clone only needs the one-time `make setup-hooks`. Document that
command in the project README's setup steps so it is not forgotten - a missing
hook means lint silently never runs on commit.

## .pre-commit-config.yaml

Pin every hook to a version compatible with the target Python. Example for
Python 3.14:

```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v6.0.0
    hooks:
      - id: check-yaml
      - id: double-quote-string-fixer
  - repo: https://github.com/psf/black
    rev: 26.5.1
    hooks:
      - id: black
        # Pin target so black does not infer a newer Python than the runtime
        # and trip its AST safety check.
        args: [--skip-string-normalization, --target-version, py314]
        language_version: python3.14
  - repo: https://github.com/PyCQA/docformatter
    rev: v1.7.8
    hooks:
      - id: docformatter
  - repo: https://github.com/PyCQA/isort
    rev: 8.0.1
    hooks:
      - id: isort
  - repo: https://github.com/PyCQA/flake8
    rev: 7.3.0
    hooks:
      - id: flake8
  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: v2.1.0
    hooks:
      - id: mypy
        additional_dependencies: [pydantic, types-PyMySQL]  # match your typed deps
```

## .gitignore

```gitignore
# Docker lint target - build sentinel and pre-commit cache
.lint-image
.pre-commit-cache/

# Python
__pycache__/
*.py[cod]
.pytest_cache/
*.egg-info/
build/
dist/
.mypy_cache/
```

## .claude/settings.json (project-level, committed)

Let Claude Code run the test/lint commands without permission prompts. This is
shared team config; keep personal overrides in `.claude/settings.local.json`
(which must be gitignored).

```json
{
  "permissions": {
    "allow": [
      "Bash(make test)",
      "Bash(make build)",
      "Bash(make lint)",
      "Bash(python -m pytest)",
      "Bash(python -m pytest *)",
      "Bash(pre-commit run)",
      "Bash(pre-commit run *)",
      "Bash(flake8)",
      "Bash(flake8 *)",
      "Bash(black)",
      "Bash(black *)",
      "Bash(isort)",
      "Bash(isort *)",
      "Bash(mypy)",
      "Bash(mypy *)"
    ]
  }
}
```

Caveat: a `settings.json` created mid-session is not hot-reloaded - open `/hooks`
once or restart the session for new rules to apply.

## CI (GitLab) - mirror the same image

```yaml
test:
  stage: test
  image: python:3.14-slim
  script:
    - pip install .[test]
    - python -m pytest tests/ -v
```

## Lessons / gotchas

- **slim vs alpine:** default to `-slim` (glibc). Alpine's base is smaller, but
  its musl libc can't use the common `manylinux` (glibc) wheels, so pip may fall
  back to building C extensions from source - slower, needs a build toolchain,
  and sometimes a *larger* final image than slim. Coverage of `musllinux` wheels
  (PEP 656) keeps growing, so alpine is fine when every dependency ships them;
  otherwise slim is the safer default.
- **Bump linters with Python.** flake8/pyflakes and black reference removed AST
  nodes; old pins crash on Python ≥ 3.12 (`module 'ast' has no attribute 'Str'`).
- **black target-version:** set `--target-version pyXYZ` so black doesn't format
  for a newer Python than the runtime and fail its AST equivalence check.
- **docformatter + TOML:** on Python < 3.11 there is no stdlib `tomllib`;
  docformatter needs `additional_dependencies: [tomli]`. On ≥ 3.11 it works
  out of the box.
- **black ↔ docformatter at a module docstring:** when a `def`/`class` follows
  a module docstring with *nothing* between them, black inserts 2 blank lines
  (to satisfy E302) and docformatter collapses them back to 1. The hooks run in
  sequence, so the file oscillates and pre-commit never converges - every run
  reports "files were modified", and `# noqa` can't help because the formatters
  themselves keep rewriting it. It only triggers when the def/class is the
  *first* statement; normally imports sit between the docstring and it. Fix by
  moving the text into a function/class docstring (black and docformatter agree
  on those), or by keeping an import between the module docstring and the first
  definition. (Bumping versions does *not* fix this one - it persists on the
  black-compatible docformatter v1.7.8.)
