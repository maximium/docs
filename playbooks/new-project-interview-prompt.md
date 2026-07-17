# Kick off a new project with the interview prompt

Reusable prompt template for starting a new app project with an AI agent. Fill
in the app description and feature bullets, keep the interview part verbatim:
it makes the agent extract requirements from you before writing any code,
instead of silently inventing a stack and a scope.

## Why

A bare feature list makes the agent fill every gap (stack, storage, scope,
quality bar) with its own defaults, and the gaps surface late, as rework. The
template turns the kickoff into a structured requirements interview along the
classic requirements-engineering axes: functional requirements (product),
quality attributes (FURPS: Functionality, Usability, Reliability, Performance,
Supportability), technical choices, and hard constraints as a separate
category (as in SEI's "architectural drivers"). The explicit plan/approval gate
at the end keeps delivery in runnable, verifiable stages instead of one big
unreviewable drop.

## The template

```text
Make a <type of app> to <core job it does>. It should
  - <feature 1>
  - <feature 2>
  - <feature 3>

Before writing any code, interview me to clarify:

1. Product requirements: who the users are, the core workflows,
   and what a minimal usable version looks like.
2. Quality attributes (FURPS): expectations for functionality depth,
   usability, reliability, performance, and supportability/maintenance.
3. Technical requirements: preferred stack, data storage, integrations,
   auth, and deployment target. If I have no preference, propose a default
   and explain the trade-off.
4. Hard constraints: anything non-negotiable (deadlines, budget,
   mandated technologies, compliance, things that must NOT be done).

Ask the questions in batches, not all at once. When my answers conflict
with each other or with a constraint, point it out instead of silently
picking one.

After the interview, propose an implementation plan: architecture,
delivery stages (each one runnable and verifiable), and what's in/out
of the MVP. Wait for my approval before writing code.
```

## How to fill it

1. **Describe the app in one line.** `<type of app>` plus the core job it
   does, not a marketing pitch.
2. **List 5-7 features, each concrete and verifiable.** Every bullet should
   admit a yes/no "is it done?" answer. Reference a well-known product where
   it compresses requirements ("a calendar view like Outlook"). Everything
   beyond 5-7 bullets will surface in the interview anyway.
3. **State the stack only if you care.** If you have a preference, put it in
   the feature list or answer it in the interview; if you stay silent, the
   template makes the agent propose a default with a trade-off instead of
   picking one silently.
4. **Keep the interview and plan sections verbatim.** They are the mechanism;
   the feature list is just the seed.

## When to skip

For small single-file tasks (a script, a one-off migration) the interview is
overhead: state everything in one prompt instead.
