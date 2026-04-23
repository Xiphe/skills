---
name: Commit
description: Generate a "golden" commit messages following the established commit message format by analyzing git diffs. Prefer asking the user for intent and references over inferring from the diff alone; use when the user asks to write commit messages, create commits, review staged changes, or mentions git commit.
---

## Task

Your role is to write a deeply helpful commit message that
captures the intent and rationale of the changeset in order to give
reviewers (on a PR review, or a `git log`, `git blame`) a clear understanding
of why this code has been added and what it is supposed to do.

Commit messages like this are essential for long term maintainability of the codebase.
Where we expect other engineers to look at the given code 3 years later and
wonder "what was this supposed to do?". THIS is our target audience.
We write for people new to any context that we currently hold.

Your task is NOT to review the changes, nor to add/change anything about the
codebase or git stage.

---

## Interaction precedence

General instructions often bias agents toward **minimizing back-and-forth** and
**shipping the outcome** (for example running `git commit` quickly).

**This skill overrides that bias for this workflow.** When in doubt, prefer a short
pause to ask the user over inferring intent from the diff alone. A blocked step
waiting on a link, a one-line confirmation of the "why", or an explicit waiver is
**success**, not failure.

Treat the staged diff as **evidence of what changed**, not as **authorization
to invent why it changed**. If the user has not stated the driving situation
(feature, bug, constraint, or decision) in this conversation, you still need
that signal from them or an explicit waiver (see below) before you treat the
rationale as settled.

---

## Workflow

### 1. **Inspect the currently staged changes**

Run `git diff --staged` to understand what we're about to commit.
ONLY consider staged changes. NEVER EVER change whats staged. NEVER write code.

You will NOT review the changes.
ONLY when you find something that is obviously not intended to be part of the
changeset or posing a security risk/leak, then ABORT the operation and inform the user.

In case nothing is staged, inform the user and abort the operation.

### 2. **Gather intent, rationale and context**

Do **not** run `git commit`, and do **not** present a final commit message as
"ready to paste" as a substitute for interaction, until the **pre-commit gate**
in the next subsection is satisfied.

In order to write a good commit message we need to understand the **why** behind the changes.
This is everything that is NOT in code. The feature request/bug report/design decision, that
drove the change as well as any exploration leading to the exact implementation at hand.

Be sure to fully understand all the context that someone from the future would need to understand
why the changeset is needed right now. Our goal is to give future maintainers confidence when
they want to change or remove the code at hand.

When available, use previous conversations, planning notes and research outcomes available to you
and related to this changeset to understand which real world situation this changeset is addressing.

Focus only on initial intend and final outcome, future engineers are not interested in intermediate
steps, discussions, etc that have not affected the final outcome.

#### References

Unless references are already present in this thread for this changeset, or the
user has used an **explicit waiver** (see Pre-commit gate), you MUST ask the user
for outside references before you finalize the message:

- links to tickets/issues
- related Pull Requests
- documentation/specification documents
- RFCs, conversation links

When possible read these references to further understand the context.

Make sure to understand how tickets/issues should be referenced to make use
of platform features like GitHubs `ref:` and `fix:` keywords.

#### Clarifying questions

When information is sparse or anything is unclear you MUST ask the user clarifying questions.
Do NOT assume anything based only on the staged changes. "Obvious" file names or
self-documenting docs are **not** an exception: the diff still does not prove product
intent or release context.

When available use the `AskQuestion` or a similar interactive tool.

When no interactive tool is available resort to freeform conversation.
Going back and forth with the user asking concise 1-2 questions at a time.

User input is NOT to be taken literally. They may respond in spoken language, telling you the story behind the changeset. It is your job to interpret and understand the user's intent and bring it into a format that is actionable and helpful to the future maintainers.

#### Pre-commit gate

All must be true before step 3:

- [ ] **References policy**: You have either (a) links the user supplied (or
      that already appear verbatim in this thread for this changeset), or
      (b) an **explicit waiver** from the user that no external references apply.
      Silence, urgency, or "just commit" is **not** a waiver.
- [ ] **Rationale policy**: You have either (a) a user-stated **why** for this
      changeset in this thread, or (b) an explicit waiver to infer the why only
      from conversation context they already provided (not from the diff alone).

If any box is unchecked, **ask** (concise questions, or `AskQuestion` when
available) and **stop** until the user answers or waives. Default one round with
at most two focused questions rather than a long questionnaire.

#### Explicit waivers

The user may skip reference collection or separate rationale only by **clear**
opt-out, for example: "no ticket or PR for this", "no external refs", or "why
is: [one sentence] — no links".

If they deny references ("don't ask for links"), treat that as satisfying
references policy with none in the footer.

### 3. Write the commit message

#### Understand the format

- Make sure you understand the commit message format established in the codebase.
- When not clear from the context check the previous commits.
- When in doubt default to conventional commit format.

  ```
  <type>(<scope>): <description>

  [why]

  [optional references]
  ```

#### Write a **golden** commit message

Using the "Golden Circle" analogy we write a commit that captures
the "why", the "what" and the "how".

1. **Why** was a change made? (The commit message body)

   Describe the rationale behind the change.

   Commonly introduced by the phrase "In order to...", "Due to..." or "As a result of...".

   The why is NEVER about implementation details.
   The why is NEVER a summary of the diff.
   The why is ALWAYS and ONLY the intention behind the diff.

   **Good why examples:**
   - In order to prevent data loss when users navigate away while editing
     a their bio in the profile.
   - Addressing a bug introduced with the [landing page redesign](https://github.com/Xiphe/sites/pull/46)
     where the header was not visible on mobile devices."
   - In the upcoming triple color pipeline feature we will need to cache
     the colorways so that users do not experience a flicker when switching
     This prepares a general caching solution suitable for this use case.
     While also allowing future use by other parts of the app.

   **Bad why examples:**
   - ❌ "Changed useState to useReducer"
   - ❌ "Simplified package names"
   - ❌ "Added a new function"
   - ❌ "Updated the component"
   - ❌ "Changes include: [list of changes...]"

   These examples are deliberately short to illustrate the point.
   The actual commit body usually is longer and much more detailed.
   We want to communicate the full rationale here. Take as much space as needed.

   Make sure to include all external references in the commit message footer.

2. **What** was changed? (The commit message header)

   Create a short (64 characters or less) technical summary of the change.
   Be specific. This may include implementation details. Present tense ALWAYS.

   Use established types from the commit format such as `fix`, `feat`, `refactor`, `docs`, `test`, `build`, `style`.

   In monorepo projects, the scope is the app or package that contains most of the changes.
   Otherwise the scope describes the part/feature of the codebase that was changed.
   Scope can be omitted for generic changes.

3. **How** was the change made? (The diff)

   By it's very nature a commit encodes the "how" perfectly - It's the implementation.
   Nothing to do here oder then to really make sure that we don't pollute
   the commit message header or body with any implementation details.

### 4. Commit and be silent

Commit the changes and write "committed as `{sha}`" as a final message. No need to summarize anything.

---

## Breaking changes

Determine wether the change needs to be communicated as breaking.

1. **Does the change affect a public programmatic API?**
   This is usually either a package/module API when the project is published to a registry
   or a HTTP API when the project is a web-service.

   User Interfaces are NOT considered when determining if a change is breaking.

2. **Do we see signals of a breaking change?**

   Common indicators:
   - existing tests are adjusted
   - `exports` of package.json are changed
   - the public API of the package is changed
   - API endpoints are removed
   - Wire formats are changed

   Non-indicators:
   - end-user facing UI changes of web-apps/websites
   - changes on unstable, experimental or internal APIs
   - deprecating methods or properties of the public API

3. **Is this actually affecting already released versions of the project?**
   Often times the signals above can happen during a branched development cycle.
   If the change only _"breaks"_ something that has been introduced on the same branch
   and has not yet been released, then it is not a breaking change.

   To determine this, check wether you are on a trunk/feature branch and wether
   the breaking signals are true against the default branch of the project.

When ALL of the above is true, communicate the breaking change in the commit message body.

1. mention the BREAKING CHANGE keyword
2. full rationale for the change
3. detailed description of the affected APIs
4. full migration guide

When you miss context on any of the above or are in doubt, you MUST ask the user for clarification.
And work back and forth with them to clarify the situation and eventually write a commit message
that is accurate and helpful to the future maintainers.

---

## Examples of good commit messages

**Example 1: Bug fix**

```
fix(frontend): correct date formatting in timezone conversion

Users were reporting incorrect dates in their reports.
Caused by pre-rendering dates in the server's timezone.
Fix is to send timestamps to the client and let the client format the date.

fix: https://app.clickup.com/t/abcdefg
```

**Example 2: New feature**

```
feat(backend): add pipeline api endpoints for dual color

In order to allow users to create dual color products,
as specified in the [product requirements](https://app.clickup.com/t/abcdefg).

ref: https://slack.com/archives/C07B4567890/p1234567890
```

**Example 3: Breaking change**

```
feat(api): remove deprecated `getProfile` method in favor of `getUser`

BREAKING CHANGE:
The `getProfile` method has been deprecated as it was misleading
and often assumed to return the users shoe shape rather than
the users profile data.

The `getUser` method was is now stable and fully adopted by all
current clients. So the deprecated method is no longer needed.

## Affected APIs
\`\`\`typescript
// No longer available:
import { getProfile } from '@xiphe/sites/api';
\`\`\`

## Migration guide
\`\`\`typescript
// Use the new `getUser` method instead:
import { getUser } from '@xiphe/sites/api';
\`\`\`

ref: https://github.com/Xiphe/sites/pull/123
fix: https://github.com/Xiphe/sites/issues/124
```

---

## Co-authoring

When it's clear from the context of the conversation that an agent implemented
or co-implemented the changeset, add them as the co-author of the commit in
order to be transparent about the contribution.

### Common agents

- `Co-authored-by: Cursor <cursoragent@cursor.com>`
- `Co-authored-by: GitHub Copilot <github-copilot@users.noreply.github.com>`
- `Co-authored-by: Claude <claude@anthropic.com>`

### Non-qualifying contributions

- Writing the commit message does NOT qualify as co-authoring.
- Researching the codebase does NOT qualify as co-authoring.
- Performing code-review does NOT qualify as co-authoring.

When in doubt, ask the user if the agent should be mentioned.

---

## GPG signing

Respect the user's GPG signing preferences. Don't disable it without asking.
Don't enable it without asking.
When encountering gpg related errors abort immediately and ask user to check their gpg setup.
