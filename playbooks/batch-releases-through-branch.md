# Batch releases through a release branch

When a merge into the default branch deploys to production and several task
branches should ship as one release, merge them into a short-lived
`release/<YYYY-MM-DD>` branch cut from the default branch, then merge that
branch into the default branch once.

## Why

Every merge into the default branch is a deploy, so tasks merged one by one
ship one by one, half-finished sets included. A release branch holds them
until its single merge.

A permanent `develop` branch does the same job but costs more:

- every hotfix on the default branch must be merged back, or the two drift
  apart and conflict later;
- squashing `develop` into the default branch leaves `develop` with commits the
  default branch lacks, so the next merge replays changes already shipped;
- a broken task merged into it can only be reverted, while a release branch
  can be re-cut without it, since task branches start from the default branch.

A release branch lives for one release, so none of this accumulates.

## How

1. **Cut the release branch from a fresh default branch.**

   ```bash
   git fetch origin && git switch -c release/2026-11-19 origin/master
   git push -u origin release/2026-11-19
   ```

2. **Cut task branches from the default branch**, not from the release branch,
   so a task dropped from this release does not drag the others along. Cut
   from the release branch only when the task builds on another task already
   merged there.
3. **Target every task MR at the release branch.** Find the open one with
   `git branch -r --list 'origin/release/*'` and pass it explicitly
   (`glab mr create -b release/...`). Without it, the MR goes to the default
   branch and ships the task to production on its own. If it is unclear which
   release a task belongs to, ask.
4. **Resolve conflicts between tasks in the task branch**: merge the release
   branch into it before opening its MR.
5. **Ship with one MR from the release branch into the default branch.**
   Squash is safe, since the release branch is deleted afterward. Delete it
   and the merged task branches once the pipeline is green. Its description
   repeats `Closes #N` for every task it ships: GitLab and GitHub apply
   closing keywords only on MRs into the default branch, so the one in a task
   MR does nothing, and a bare `(#N)` only links.

## When to skip

A single task needs no release branch: keep a Draft MR from its branch to the
default branch and merge it when ready.

## Notes

- **CI must run on `release/*`.** Jobs whose `rules` limit them to the default
  branch or to MRs into it skip the release branch, so the first check happens
  on the shipping MR.
