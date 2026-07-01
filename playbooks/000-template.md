# Playbook template

Skeleton for a **playbook rule**: a reusable, cross-project instruction that
tells an AI agent (or a human) how to behave in some situation. Copy everything
below the `---` into a new `playbooks/<kebab-case-name>.md`, fill the
placeholders, then delete the placeholder hints. Add the new file to the index
in the repo's top-level `README.md`.

## What this template is for

The rule playbooks in this directory share one shape, and this file fixes it:

- `# Title` in the imperative ("Keep README current", "Wait for code review").
- A lead paragraph that opens with "Reusable instruction for AI agents working
  in ..." and, in one or two sentences, states the rule and where it applies.
- `## Why`, the rationale (why the rule earns its keep, not just what it is).
- `## How`, the concrete steps, usually a numbered list with a **bold lead** per
  step.
- Optional trailing sections as the rule needs them: `When to skip`,
  `Checklist`, `Notes`, `Gotcha`.

## What it is not for

Setup recipes (for example `python-docker-make-lint-test.md`) are a different
genre and do not use this skeleton. A recipe is copy-paste scaffolding (config
files, Dockerfiles, Make targets) organized by artifact, and it tends to open
with `## Philosophy` rather than `## Why`. Do not force a recipe into the rule
shape, and do not force a rule into the recipe shape.

---

# <Title in the imperative>

Reusable instruction for AI agents working in <where it applies: "any
repository", "a repository with a `docs/` directory", etc.>. <One or two
sentences: what the rule is and what to do.>

## Why

<Why this rule is worth following. Explain the cost of not doing it and what
"done" looks like, so a future reader can tell whether a change should revise
the rule.>

## How

1. **<First step>.** <Detail.>
2. **<Second step>.** <Detail.>
3. **<Third step>.** <Detail.>

## When to skip

<Optional. When the rule does not apply, and the instruction to say so
explicitly rather than skip silently. Drop this section if there is no
meaningful skip case.>