---
name: new-project
description: Kick off a new app project through a structured requirements interview before writing any code. Use whenever the user invokes /new-project, or describes a brand-new application/project they want built from scratch and the requirements have not been clarified yet (no existing codebase to follow). Do not use for small one-off scripts or changes to an existing project.
---

# New project kickoff

This skill is a thin pointer into the reference docs repo. The repo root is
two directories up from this SKILL.md (the file is symlinked into
`~/.claude/skills/`; path resolution follows the symlink to the real
location). The repo holds the single source of truth; do not duplicate its
content, and if this file and the repo ever disagree, the repo wins.

1. Read `playbooks/new-project-interview-prompt.md` (path relative to the
   repo root) and run it as written: the requirements interview, then the
   implementation plan, then the approval gate before any code. Prefer plan
   mode for the gate when available.
2. Treat the skill arguments ($ARGUMENTS) as the fill-in part of the
   template: the one-line app description and the feature list. If they
   are missing or too vague to seed the template, ask for them first.
3. After the plan is approved, open the repo's `README.md` index and apply
   the scaffolds and playbooks that match the agreed stack; tell the user
   which ones were applied.
