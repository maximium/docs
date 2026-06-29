# Runbooks

A runbook is a step-by-step procedure for an operational task: deploying a service, rotating a credential, recovering from a known failure mode, running a backfill. Runbooks exist so that someone who is tired, paged at 3 a.m., or new to the system can execute the task correctly without reconstructing it from first principles.

## When to write one

**Rule of thumb: the second time you do the same operational task, write a runbook.** The first time is discovery; the second time is a signal that this will happen again, and the next person (possibly you) will benefit from not rediscovering it.

Also write a runbook when:

- A procedure must be executed under time pressure (incident response, on-call).
- A procedure is rare enough that nobody will remember the details (quarterly key rotation, yearly certificate renewal).
- A procedure has a destructive step where doing it wrong has real consequences.

Do **not** write runbooks for routine development tasks that are already covered by `make` targets, scripts, or CI.

## What makes a good runbook

- **Optimised for stress.** Short sentences, numbered steps, exact commands. No prose paragraphs to skim past.
- **Copy-pasteable.** Commands include all required flags. Placeholders are clearly marked (e.g. `<RESOURCE_ID>`).
- **Honest about reversibility.** Every destructive step says what it destroys. A Rollback section covers the most likely failure paths.
- **Maintained.** When the procedure changes, the runbook is updated in the same PR. A stale runbook is worse than no runbook.

See [template.md](template.md) for the standard layout.

## Naming

Use descriptive kebab-case filenames matching the task: `rotate-service-token.md`, `backfill-search-index.md`, `recover-from-stuck-cursor.md`. No numbering - runbooks are referenced by what they do, not when they were written.