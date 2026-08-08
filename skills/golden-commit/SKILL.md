---
name: golden-commit
description: Golden commit criteria for preserving the why, what, and how of a
  changeset. Apply when drafting or reviewing commit messages, or assessing
  commit scope.
---

# golden commits

A golden commit documents a changeset for software archaeology using
the project's established commit message format or defaulting to Conventional
Commits style.

1. **Why** was a change made? (In the commit message body)

   The _why_ traces the causal chain from the conditions that made the change
   necessary toward its intended effect, preserving the links a future engineer
   cannot infer from the header or diff. Technical details form part of the
   _why_ when they preserve such a link or explain a constraint, tradeoff, or
   surprising decision.

   **External References**

   It also points to the external /trail-of-why links such as specifications,
   tickets, documentation, architecture decision records and related commits
   placed in the commit footer, after the body (ordered by significance, most
   important refs last).

   **Additional content**

   Golden commits are additive: project conventions may require other body
   content. Such content can coexist with the _why_, but does not replace it.

2. **What** was changed? (The commit message header)

   A short announcement of the target state, within the project's header limit
   or 64 characters when none is established, written in the language most
   understood by readers of its history and changelog.

3. **How** was the change made? (The diff)

   The diff is the primary record of how the change was implemented.

## Commit Scope

A golden commit represents one coherent change: it can be reverted without
breaking unrelated behavior.

**Smells of over-scoped commits:**

- The commit needs more than one independent _why_.
- Parts of the diff can be reverted independently without invalidating the
  rest.
- An accurate header must join unrelated outcomes or scopes.

**Smells of fragmented commits:**

- The commit requires an adjacent commit to build, test, or behave coherently.
- Its header or _why_ only makes sense when read with a neighboring commit.
- Successive commits repeatedly revise the same change without creating
  independently meaningful states.

These smells trigger re-evaluation of the commit scope.

## Examples

```text
fix(header): keep header visible after saving on mobile

The recently added saving indicator caused the header to disappear on mobile
devices whenever a user saved their profile. This was unintended and left the
top navigation inaccessible until the user reloaded the page. Preventing the
line break that caused the mis-render keeps the header in position.

ref: [saving indicator added](https://example.com/commits/12af9e)
fix: https://example.com/issues/417
```

```text
feat(color-cache): share rendered colorways across pipelines

The upcoming triple-color pipeline switches between rendered colorways during
product configuration. Regenerating a colorway on selection caused visible
flicker, while keeping the cache inside that pipeline would duplicate the same
lifecycle needed by other renderers. A shared cache gives those consumers one
ownership model.

spec: https://example.com/specs/triple-color-pipeline
adr: https://example.com/adrs/shared-colorway-cache
fix: https://example.com/issues/842
```

```text
fix(linker): stabilize relocation order in incremental builds

Incremental builds occasionally emitted binaries with debug addresses that
differed from clean builds. Relocations inherited filesystem discovery order,
which remained stable within one process but varied across machines; a remote
cache hit could therefore change symbol ordering. Ordering relocations by
section offset makes clean and incremental artifacts reproducible.

fix: https://example.com/toolchain/issues/912
```
