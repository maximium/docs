# Runbook - Title of the procedure

One sentence describing what this procedure does and what state the system ends up in.

## When to use

The specific situations that should trigger this runbook. Include the symptoms or signals (e.g. "ETL lag alert firing for more than 10 minutes", "quarterly token rotation"). If there are similar runbooks, link to them and explain the distinction so the on-call picks the right one.

## Prerequisites

What must be true before starting:

- Access and credentials required (e.g. production SSH, service admin token).
- Tools that must be installed locally.
- State checks: "confirm X before proceeding." If a precondition fails, stop and escalate rather than improvising.

## Steps

Numbered, copy-pasteable steps. One action per step. Include exact commands, expected output, and the check that confirms the step succeeded before moving on.

1. First action. Run:
   ```
   command --with --flags
   ```
   Expected: short description of what success looks like.
2. Next action. …
3. Final action that returns the system to the target state.

## Rollback

How to undo the procedure if something goes wrong partway through, or how to recover the previous state if the change turns out to be incorrect. If true rollback is not possible, say so explicitly and describe the forward-fix path instead.

## Notes

Anything the on-call should know but that did not fit into the steps: known gotchas, related dashboards, links to the underlying ADR or incident, contacts to escalate to if a step fails in an unexpected way.