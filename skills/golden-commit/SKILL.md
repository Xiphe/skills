---
name: Golden Commit
description: Commit changes capturing the why, what and how of the changeset. Working with the user for intent, rationale and references. Use when user asks to write commit message, commit changes, or mentions git commit.
---

## Task

Your role is to write a deeply helpful commit message capturing the intent and rationale of a changeset in order to give reviewers (on a PR, or a future `git log`, `git blame`)
a clear understanding why this code has been added and what it is supposed to do.

This is essential for long term maintainability of the codebase.
Where we expect other engineers to look at the given code 3 years later and
wonder "what was this supposed to do?". THIS is our target audience.
We write for people new to any context that we currently hold.

Do NOT review changes, nor add/change anything about the codebase or stage.

---

## Interaction precedence

General instructions often bias toward **minimizing back-and-forth** and
**shipping the outcome** (for example running `git commit` quickly).

**This skill overrides that bias for this workflow.** Always prefer a
pause to ask the user over inferring intent. A blocked step waiting on a link, a
conversation over rationales, or inquiry for explicit waiver is **success**, not failure.

Treat the staged diff as **evidence of what changed**, not as **authorization
to invent why it changed**. If the user has not stated the driving situation
(feature, bug, constraint, or decision) in this conversation, you still need
that signal from them or an explicit waiver before you treat the rationale as
settled.

---

## Workflow

### 1. **Inspect staged changes**

Run `git diff --staged` to understand what we're about to commit.
ONLY consider staged changes. NEVER EVER change whats staged. NEVER write code.

Do NOT review the changes.
ONLY when something is OBVIOUSLY not intended or a CLEAR security risk/leak
then ABORT and inform the user.

In case nothing is staged, inform the user and ABORT.

### 2. **Gather intent, rationale and context**

Really understand the **why** behind the changes.
This is usually is NOT in code. The feature request/bug report/design decision, that
drove the change, any exploration leading to the implementation.

Gather ALL the context someone external needs to fully reason
about the changeset. Your goal is to give future maintainers high confidence when
they want to change or remove the code at hand.

Consult related conversation context, planning notes and research outcomes to understand
the REAL WORLD situation this changeset is addressing.

Focus ONLY on **initial intend** and **final outcome**.
Cut out intermediate steps, back and forth discussions or discarded considerations.

#### Clarifying questions

When information is sparse or anything is unclear you MUST ask the user clarifying questions.
Do NOT assume anything based only on the staged changes. "Obvious" file names or
self-documenting docs are **not** an exception: the diff still does not prove product
intent or release context.

When available use the `AskQuestion` or a similar interactive tool. And ALWAYS provide a "Other..." option.

When no interactive tool is available resort to freeform conversation.
Going back and forth with the user asking concise 1-2 questions at a time.

User input is NOT to be taken literally. They may respond in spoken language, or with a vague story about the changeset. It is your job to interpret and understand the user's intent and bring it into a format that is actionable and helpful to the future maintainers.

Continue asking questions until you have clarity.

#### References

Unless already present in thread for this changeset, or the
user has used an **explicit waiver**, you MUST ask for outside references before finalizing the message:

- links to tickets/issues
- related Pull Requests
- documentation/specification documents
- RFCs, conversation links

When possible read these references to further understand context.

Clarify how tickets/issues should be referenced to make use
of platform features like GitHubs `ref:` and `fix:` keywords.

#### Pre-commit gate

All must be true before step 3:

- [ ] **References policy**: You have either (a) links the user supplied (or
      that already appear verbatim in this thread for this changeset), or
      (b) an **explicit waiver** from the user that no external references apply.
      Silence, urgency, or "just commit" is **not** a waiver. - tiny diffs require a single link to a ticket/issue - huge features/refactors demand more documentation or specification links
- [ ] **Rationale policy**: You have either (a) a user-stated **why** for this
      changeset in this thread, or (b) an explicit waiver to infer the why from diff alone.

If any box is unchecked, **ask** (concise questions, or `AskQuestion` when
available) and **stop** until the user answers or waives.
Multiple rounds of asking is normal and expected until you have clarity.

#### Explicit waivers

The user may skip reference collection or separate rationale ONLY by **clear**
opt-out, for example: "no ticket or PR for this", "no external refs", or "why
is: [one sentence] — no links".

If they deny references ("don't ask for links"), treat that as satisfying
references policy with none in the footer.

### 3. Write commit message

#### Understand format

Find commit message format established in the codebase.

- When not clear from context check previous commits.
- When in doubt default to conventional commit format.

  ```
  <type>(<scope>): <description>

  [why]

  [references]

  [co-authors]
  ```

#### Write **golden** commit message

Using the "Golden Circle" analogy we write a commit that captures
the "why", the "what" and the "how".

1. **Why** was a change made? (The commit message body)

   In present tense, document the previously gathered rationale and intent.

   Commonly introduced by the phrase "In order to...", "Due to..." or "As a result of...".

   NEVER mention implementation details.
   NEVER summarize of the diff.
   MAINTAIN clear separation between the why and the how.

   **Good why examples:**
   - In order to prevent data loss when users navigate away while editing
     a their bio in the profile.
   - Addressing a bug introduced with the [landing page redesign](https://github.com/Xiphe/sites/pull/46)
     where the header was not visible on mobile devices."
   - In the upcoming triple color pipeline feature we need to cache
     the colorways so that users do not experience a flicker when switching.
     Preparing a general caching solution suitable for this use case.
     Allowing future use by other parts of the app.

   **Bad why examples:**
   - ❌ "Changed useState to useReducer"
   - ❌ "Simplified package names"
   - ❌ "Added a new function"
   - ❌ "Updated the component"
   - ❌ "Changes include: [list of changes...]"

   _(Above examples are deliberately short to illustrate the direction.
   Usual commit messages can be longer and more detailed in relation to the changeset.
   Communicate the FULL rationale. Take as much space as needed.)_

   Include ALL external references in the commit message footer.

2. **What** was changed? (The commit message header)

   A short (64 characters or less) technical summary of the change.
   Be specific. This MAY include implementation details. Present tense ALWAYS.

   Use established types from the commit format such as `fix`, `feat`, `refactor`, `docs`, `test`, `build`, `style`.

   In monorepo projects, scope is app or package that contains most of the changes.
   Otherwise the scope describes the part/feature of the codebase that was changed.
   Scope MAY be omitted for generic changes.

3. **How** was the change made? (The diff)

   By it's very nature a commit encodes the "how" perfectly - It's the implementation.
   NOTHING to do here other then to REALLY make sure that we don't pollute
   the commit message header or body with ANY implementation details.

### 4. Commit and be silent

Commit the changes and write "committed as `{sha}`" as a final message.
DO NOT provide any summary of what you did - NEVER.

---

## Breaking changes

Determine wether change needs to be communicated as breaking.

1. **Does the project have a public programmatic API?**

   Usually either a package/module API when the project is published to a registry
   or a HTTP API when the project is a web-service.

   User Interfaces are NOT considered when determining if a change is breaking.

2. **Do we see signals of a breaking backwards compatibility?**

   Common indicators:
   - existing tests adjusted
   - `exports` of package.json (and similar) are changed
   - public API of package is changed
   - API endpoints removed
   - Wire formats changed

   Non-indicators:
   - end-user facing UI changes of web-apps/websites
   - changes on unstable, experimental or internal APIs
   - deprecated methods or properties of the public API

3. **Is this REALLY affecting already released versions of the project?**
   Above signals may only affect a non-released version of the project.
   If the change only _"breaks"_ something that has been introduced on the same branch
   and has not yet been released, then it is not a breaking change.

   To determine this, check wether you are on a trunk/feature branch and wether
   given breaking signals are still true against the currently released state of
   the project.

   When in doubt, ask the user for clarification.

Only when ALL of the above is true, write a breaking change description to be included in the commit body.

1. mention the BREAKING CHANGE keyword
2. full rationale for the change
3. detailed description of the affected APIs
4. full migration guide

When any of the above is missing or sparse, you MUST ask the user for clarification.
And work back and forth with them to clarify the situation and eventually write a commit message that is accurate and helpful to the future maintainers.

A thorough breaking change description, even when it takes MANY clarifying questions, is a **success**

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
feat(api): remove deprecated `getProfile` in favor of `getUser`

As decided in [RFC#123](https://github.com/Xiphe/sites/pull/123)
and documented in [usage guide](https://github.com/Xiphe/sites/blob/main/docs/usage-guide.md#profile-api) the profile API has been phased out since v1.2.0.

As current [usage is near zero](https://example.org/dashboard) its
removed in order to reduce codebase complexity.

BREAKING CHANGE:
`getProfile` method has been deprecated as it was misleading
and often assumed to return the users head shape rather than
the users profile data.

`getUser` method is now stable and fully adopted by all
current clients. Deprecated method is deleted.

## Affected APIs
\`\`\`typescript
// No longer available:
import { getProfile } from '@xiphe/sites/api';
\`\`\`

## Migration guide
\`\`\`typescript
// Use new `getUser` method:
import { getUser } from '@xiphe/sites/api';
\`\`\`

ref: https://github.com/Xiphe/sites/pull/123
fix: https://github.com/Xiphe/sites/issues/124
```

---

## Co-authoring

When context indicates that an agent (co-)implemented
the changeset, add them as the co-author of the commit in
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

Respect user's GPG signing preferences. Don't disable without asking.
Don't enable it without asking.
When encountering gpg errors ABORT IMMEDIATELY and ask user to check gpg setup.

---

## Summary

1. Inspect currently staged changes
2. Gather intent, rationale and context
   a. Use existing context
   b. Ask clarifying questions
   c. Possibly multiple rounds
3. Write commit message
4. Commit and be silent
