# Keep test names short

A test name carries the facts that distinguish this test from its neighbours
and nothing else: no articles, no "is/are/does", no verb a noun can carry, no
word the file already says. Test-local helper classes take plain names,
without a leading underscore.

## Why

Test names are read where space is short: the runner's line, a CI failure list,
a diff. A sentence-shaped name (`test_a_batch_is_committed_once`) wraps in all
three and pushes the one word that says which case broke to the far end of the
line. The grammar words are identical in every name in the file, so they cost
width and return nothing.

Shortening has a floor. The name still has to identify the case from the
failure line alone, without opening the file: `test_batch_committed_once` does,
`test_commit` does not. Naming the case is the requirement; spelling out the
expected outcome often is not, because the subject already implies it. Trim
filler, never precision.

## How

1. **Drop articles and the copula.** `test_a_batch_is_committed_once` becomes
   `test_batch_committed_once`; `test_an_id_the_index_does_not_hold_is_replaced`
   becomes `test_unknown_id_replaced`. A past participle does the work "is
   ...ed" was doing.
2. **Drop the verb when a noun carries it and counts as an adjective.**
   `test_batch_costs_one_lookup` becomes `test_batch_single_lookup`;
   `test_replacements_share_one_statement` becomes
   `test_replacements_single_statement`; `test_updates_split_per_document`
   becomes `test_updates_per_document`.
3. **Drop what the file already says.** In `test_loader.py` no name needs
   `loader`; in a class of tests for one method, no name needs the method. The
   same goes for a category noun the context supplies:
   `test_matching_hash_updates_declared_attributes_only` becomes
   `test_matching_hash_updates_declared_only`.
4. **Drop the outcome word when the subject implies it.**
   `test_written_ids_reported_success` becomes `test_written_ids_reported`;
   `test_lookup_lifts_default_row_limit` becomes `test_lookup_row_limit`. What
   the assertion is stays in the body, where it is read anyway.
5. **Stop where the name stops identifying the case.** Two tests in one file
   must not collapse onto near-identical names, and a name that could belong to
   half the file has been cut too far. That, rather than a word count, is the
   limit.
6. **No leading underscore on test-local classes.** Fakes, stubs, and sample
   dataclasses are the vocabulary of the file and appear in nearly every test:
   `FakeManticore`, not `_FakeManticore`. Nothing imports a test module, so
   "private" is already true of everything in it and the underscore only adds
   noise. Module-level helper *functions* may keep it (`_load`, `_document`),
   where it separates plumbing from the tests themselves.

## Scope: tests only

This playbook is about test names, deliberately. The verbosity it fixes comes
from a habit specific to tests, where a name is written as a sentence about a
scenario; production names in these repos have not drifted the same way, so
there is nothing there for a rule to fix. The principle behind it (a name
carries what distinguishes it) holds everywhere but does not turn it into a
licence to shorten domain names: those usually earn their words, and a name cut
to fit a line is worse than a long one. Shorten a production name only when it
repeats its module or class or restates its own arguments.

## When to skip

**Do not sweep existing files.** Older tests written in the long style stay as
they are unless the user asks. Renaming them inflates a diff that is about
something else, and a review of unrelated renames is exactly the work a reviewer
did not sign up for. Offer the sweep as its own commit instead.
