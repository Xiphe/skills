---
name: golden-commit
description: Write genuinely helpful commit messages that encode the driving
  factors and rationale behind the changeset. Apply when writing or reviewing commit message structure.
---

> Any commit's value is determined by its usefulness in future software archeology
> Including:
>
> - changelog generation
> - summaries of what shipped last month
> - a `git blame` two years in the future
> - `git bisect` sessions

# golden commits

A golden commit enables any future engineer to make sense of the situation at
the time of writing the diff. And its message carries everything that isn't
inferrable from code alone in its header, body and as reference links in the
footer.

Golden commits use the project's established commit message format or default
to conventional commits style.

The name "Golden Commits" derives from the "Golden Circle" analogy as they encode
the "why", "what" and "how" of a change.

1. **Why** was a change made? (The commit message body)

   In present tense, it documents the driving factors behind the changeset up to
   the /user-facing-why.

   It's commonly introduced by the phrase "In order to...", "Due to..." or
   "As a result of...".

   NEVER mentions implementation details (how).
   NEVER summarizes the diff (how).
   NEVER repeats the commit header (what).

   **Good why examples:**
   - In order to prevent data loss when users navigate away while
     editing their bio.
   - Addressing a rendering bug introduced by the new saving-
     indicator, where the header was not visible on mobile devices.

     After clicking save, the header had disappeared and would only
     reappear after full page reload.

   - Enabling our "ship often, ship reliable" philosophy, we have to
     ensure that CI is observable and that we have a clear way to
     track the status of the build.

     That enables us to identify and fix performance regressions of
     the pipelines early. Ultimately reducing the risk of delaying
     releases or shipping broken builds.

   **Bad why examples:**
   - ❌ "Changed useState to useReducer"
   - ❌ "Simplified package names"
   - ❌ "Added a new function"
   - ❌ "Updated the component"
   - ❌ "Changes include: [list of changes...]"

   _(These are deliberately short and illustrative examples. There is
   no character limit to the commit body. It is as deep as needed to make the
   message genuinely helpful 3 years in the future)._

   **External References**

   A /trail-of-why commonly also includes external references such as
   - specifications
   - tickets
   - documentation
   - architecture decision records
   - related commits

   References are placed in the commit footer, after the body.
   Ordered by significance, most important refs last.

   **Additional content**

   The project's commit message format or changelog generation or other tooling
   may further require documentation of breaking changes, example code, etc...
   These can always be woven into a golden commit's body as it's not exclusively
   for the _why_.

2. **What** was changed? (The commit message header)

   A short (64 characters or less), present-tense summary of the change.
   Reading like the announcement of the diff in the language most understood by
   readers of the project's history and changelog.
   _(`git diff` is a changelog, and the project may also use tools to generate a
   public log from the header)_

3. **How** was the change made? (The diff)

   By its very nature, any commit's diff perfectly encodes the _how_.

   A golden commit represents a single coherent step in the project's roadmap.
   It can be reverted without breaking unrelated parts of the system.

   **Smells of over-scoped commits:**
   - Header too generic: `fix: solve multiple race conditions`
   - Use of additive connectives: `also`, `in addition to`, `further`
   - Unclear conventional commit scope `feat(signin+compiler): `

   These are best met with re-scoping the diff and producing multiple smaller
   commits.

   **Smells of under-scoped commits:**
   - build, tests, linter etc. are not green
   - successive commits carry similar messages: `fix: attempt 3`
   - a small diff + synthetic _why_: `...in order to improve dx`

   These are best met with re-scoping or squashing into a meaningful diff, given
   the git workflow allows for it.
