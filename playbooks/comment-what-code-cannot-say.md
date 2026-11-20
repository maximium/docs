# Comment only what the code cannot say

Reusable instruction for AI agents working in any repository. A comment or
docstring answers a question the code leaves open, almost always "why this
way". Do not repeat what the code, a linked ADR, or a nearby docstring already
says, and write the shortest version that still answers.

## Why

Prose costs an agent nothing and the reader everything, so agents drift toward
explaining the obvious. Padding buries the one sentence with a real reason, and
a reviewer who learns the prose is filler stops reading it.

Restated code also rots: a paraphrase is wrong the day the code changes, and
nothing catches it. A reason survives the rewrite.

Trimming has a floor: cut filler, never precision. A non-obvious reason, an
outside constraint, a workaround with an issue behind it, an ordering that only
looks arbitrary: these are what comments are for. Zero comments is not the
goal.

## How

1. **Start from no comment.** Ask what a competent reader, with the module in
   view, would still not know. No such question, no comment. The rest of this
   list is how that plays out.
2. **One line for what, then a why only if needed.** A docstring opens with a
   summary line; a body follows only for a reason the code does not show. Most
   functions stop at the summary.
3. **Link a decision instead of retelling it.** If an ADR, runbook, or upstream
   issue records the reason, cite it and add at most one sentence of local
   context. A paraphrase is a second copy that will drift from the first.
4. **State a fact once per module.** Put it where a reader looks first, usually
   the module docstring, and reference it from elsewhere instead of repeating
   it.
5. **Do not enumerate cases the rule already implies.** After "prefer A, fall
   back to B", spelling out "when both arrive ... when only one does ..."
   adds nothing. Keep the rule, drop the derivation.
6. **Do not restate the signature.** No parameter list, no type recital, no
   "returns the count" above `-> int`. Document an argument only when its name
   and type do not carry its meaning.
7. **Keep the explanation smaller than the thing explained.** A docstring
   longer than the function body is a signal. Past three or four lines, move
   the text to the module docstring or an ADR and reference it.
8. **Delete each sentence and see what breaks.** If a reader with the code in
   front of them loses nothing, it was filler. Apply this to your own prose
   before presenting the change.

## Name the criterion, not the owner

"Ours", "mine", "theirs", "our side", "their data" name an owner, not a
criterion. "Profiles that are not ours" leaves the reader to reconstruct the
rule the sentence was supposed to state; write "profiles no registered parser
delivered". The same goes for any qualifier that states a mood rather than a
fact.

## Describe the code, not its history

A docstring says what the code is, not how far a change has got. "So far",
"still", "for now", "not yet" describe the diff, and they go stale with the
next commit, unnoticed:

```python
"""The names of the read model's indexes.

One home for them, so that the module writing an index and the module
correcting it cannot disagree on its name. Only `posts` so far: `users`
and `topics` are still spelled where each is written.
"""
```

The second sentence explains why the module exists and stays. The third is a
progress report and belongs in the commit message, the plan, or an issue. A
note about a temporary shape ("replace once the API stabilizes") stays only
with an issue link that makes it actionable.

A finished move leaves the same mark: "data rather than a constant", "no
longer a static registry", "instead of the old query" argue with a shape the
reader cannot see. Describe the current code without the contrast; the before
and after belong in the ADR and the commit that made the change.

## When to skip

**Do not clean up comments outside the change.** Trim only around the code you
change; offer a wider cleanup as its own commit.

**Do not cut a comment you cannot explain.** An obscure line often guards a
bug, a platform quirk, or an order of operations learned the hard way. Ask
rather than delete.
