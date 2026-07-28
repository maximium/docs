# Run lint / pre-commit and tests after changes

Reusable instruction for AI agents working in any repository. After making code
changes, run the project's lint / pre-commit checks **and** its test suite
yourself, fix whatever they report, and only then tell the user the work is
done. The user should not have to run lint, pre-commit, or tests manually.

## Why

These checks are part of finishing the change, not a separate follow-up the user
chases later. Leaving them undone ships formatting noise, type errors, and
regressions into the diff, and forces the user to do the loop you could have
closed. Treat "green lint + green tests" as the definition of done.

## How

1. **Find the project's own command first.** Prefer a `Makefile` target
   (`make lint` / `make test`), a `.pre-commit-config.yaml`, or instructions in
   the project's `CLAUDE.md` / `README.md`. These usually mirror CI, so passing
   them locally means passing CI.
2. **Run lint / pre-commit and apply auto-fixes.** Formatters (black, isort,
   docformatter, prettier) rewrite files on the first run and exit non-zero.
   Let them write, then **re-run until the run is clean** - a non-zero exit with
   "files were modified" is expected on the first pass, not a failure to report.
3. **Run the tests** after changes that can affect behavior and confirm they
   pass. A docs-only, comment-only, or docstring-only change cannot affect
   behavior, so skip the test suite for it (and say so), but still run lint on
   it (see below).
4. **Report honestly.** State the lint and test results, including any failures
   you could not fix. Done means both are green; if they are not, say so plainly
   rather than implying success.

## When to skip

Skip the **tests** for a docs-only, comment-only, or docstring-only change: it
cannot alter behavior, so the suite has nothing to catch. Still run **lint** on
such changes. Formatters and prose linters (black, docformatter, markdownlint,
flake8 line length) flag real issues in docs and docstrings (wrapping, line
length, style), so lint earns its keep there even when tests do not. Only skip
lint too when no lint tooling touches the changed files (e.g. markdown with no
markdown hook).

Skip both only when the repo has no lint/test tooling at all. Either way, say so
explicitly instead of silently skipping.

## Gotcha: hook version conflicts (black ↔ docformatter)

If `pre-commit` ping-pongs - one hook keeps reformatting what another just
changed and the run never converges - suspect stale, mutually incompatible hook
pins. A known case: old `black` adds a blank line after a function docstring
while old `docformatter` (≤ v1.6.0) strips it, looping forever. Fix by bumping
both to compatible versions (`black` ≥ 26.x with `docformatter` ≥ v1.7.8, which
is black-compatible). Mirror the pins from a sibling repo where pre-commit
already runs clean rather than guessing.
