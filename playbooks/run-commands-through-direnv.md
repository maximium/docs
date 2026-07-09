# Run commands through direnv when .envrc is present

Reusable instruction for AI agents working in any repository where the
directory or one of its parents contains a `.envrc` file. The project's
toolchain (compilers, interpreters, CLIs) comes from that direnv
environment; run project commands as `direnv exec <dir-with-envrc> <cmd>`
instead of calling the tools bare or recreating the environment by hand.

## Why

`direnv` activates through a shell hook that only runs in interactive
shells. An agent's shell is non-interactive, so the hook never fires:
tools from the environment appear "not installed", and the tempting
fallbacks are worse than the real thing. Installing tools globally
duplicates the pinned environment, and bypassing the cache (for a nix
flake, calling `nix develop -c` directly) re-evaluates the environment
on every command; measured on a real project this was ~9s per command
with noisy store downloads versus ~3s through `direnv exec`, which
reuses the cache that interactive use has already built.

## How

1. **Detect.** When a tool seems missing or before running project
   commands, check for `.envrc` in the working directory and its parents
   (typically up to the repo root or one level above it).
2. **Run through direnv.** Prefix commands with
   `direnv exec <dir-with-envrc>`, e.g.
   `direnv exec .. ansible-playbook deploy.yaml`. The directory argument
   is where `.envrc` lives, not the current directory.
3. **If direnv blocks, let the user allow it.** A `.envrc` that has
   changed or was never approved makes direnv refuse to load
   (`direnv: error ... is blocked`). Approving it (`direnv allow`) is a
   trust decision; report it to the user instead of running `allow`
   yourself.
4. **Fall back knowingly.** If direnv is not installed, use the
   mechanism `.envrc` wraps (for `use flake` that is
   `nix develop <dir> -c <cmd>`) and expect it to be slower; say so
   rather than reinstalling tools globally.

## When to skip

If the needed tool genuinely is global (git, system utilities) or the
repo has no `.envrc` anywhere up the tree, direnv adds nothing - run the
command directly.
