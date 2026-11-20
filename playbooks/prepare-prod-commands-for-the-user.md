# Prepare production commands, let the user run them

In any project whose workflow can reach live systems (SSH to production hosts,
Swarm/k8s services, message brokers, production databases, deploy tooling),
anything that mutates production state is prepared as an exact,
copy-pasteable command or script and handed to the user to execute; the agent
does not run it itself, even when the user asked for the outcome.

## Why

"Clean the queue" is a request for an outcome, not consent to operate
production autonomously. When the agent fires the command itself, the user
loses the one moment where a human sanity-checks the target, the scope and
the timing - and production mistakes are often irreversible (a purged queue
does not come back). The division of labor that works: the agent
investigates, reasons and prepares; the user is the hand on the trigger.
This rule exists because the agent once purged a production RabbitMQ queue over SSH
on its own initiative - the user had asked for a script or steps that does the cleanup,
not for the agent to execute it.

## How

1. **Classify before touching.** Read-only diagnostics (list, inspect,
   logs, peek-with-requeue) are fine to run, unless the user has said they
   want to drive the connections themselves - then even those are handed
   over. Anything that writes, deletes, restarts, scales or deploys is a
   mutation and follows the steps below.
2. **Prepare, don't fire.** Produce the exact command with all flags and
   clearly marked placeholders, plus one line of what it does, what success
   looks like and what it destroys. For multi-step work, stage scripts
   locally and give the user the shipping + run commands.
3. **One mutation per hand-over.** Do not bundle several destructive
   commands into one copy-paste block; the user should be able to stop
   between steps.
4. **In Claude Code, suggest the `!` prefix** so the user's command output
   lands back in the conversation and the agent can keep reasoning over it.

## When to skip

Local and dev environments (docker-compose on the dev machine, test
databases, scratch containers) and CI are not production - operate freely
there. If the user explicitly tells the agent to run a specific production
command itself, that one command is sanctioned; the default returns to
prepare-and-hand-over immediately after.
