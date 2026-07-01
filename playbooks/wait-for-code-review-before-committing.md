# Wait for code review before committing

Reusable instruction for AI agents working in any repository. Making a change is
not the same as committing it. After you finish edits (and run lint / tests per
`run-lint-and-tests-after-changes.md`), stop at a clean working tree and wait.
Do not run `git commit` (or `git push`) on your own initiative. Commit only when
the user has reviewed the change and explicitly tells you to.

## Why

The working tree is where the user reviews your work. A change that is already
committed is harder to inspect, amend, or drop, and it takes the review decision
away from the user. Leaving edits uncommitted keeps the diff in front of them and
keeps the "should this land, and how should it be split and messaged" call where
it belongs, with the author. Auto-committing also tends to bundle unrelated edits
into one commit and invent commit messages the user never approved.

## How

1. **Finish the change and verify it.** Apply the edits, run lint / pre-commit
   and tests, and get them green (see `run-lint-and-tests-after-changes.md`).
2. **Stop at the working tree.** Report what you changed and the lint / test
   result. Do not stage or commit.
3. **Wait for an explicit go-ahead.** Only commit after the user says so in
   plain terms (for example "commit this", "commit and push"). A request to make
   a change, or approval of an approach, is not approval to commit.
4. **When told to commit, follow the repo's conventions.** Branch first if you
   are on the default branch, honour any commit-message trailer or format the
   project uses, and push only if asked.

## Notes

- This complements the lint / test rule: you still run the checks yourself, you
  just do not commit the result until asked.
- If a task seems to imply committing (for example "land this fix"), confirm the
  intent rather than assuming, then proceed once the user is explicit.
- Applies to `git commit`, `git push`, tags, and anything else that writes to
  history or a remote. When in doubt, leave it in the working tree and ask.
