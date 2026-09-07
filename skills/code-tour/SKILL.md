---
name: code-tour
description: Add a temporary code tour from real usage inward, with checkboxes and room for feedback.
disable-model-invocation: true
---

# Code tour

Create an outside-in walkthrough that lets the user understand and question a
changeset in their editor. Each stop stays connected to the caller's purpose.

When asked to read feedback or remove an existing tour, follow
[CLEANUP.md](CLEANUP.md). Otherwise create or revise a tour below.

## 1. Establish the use case

Read the requested changeset, including relevant untracked work, project
instructions, and existing tour feedback. Use the scope already established in
the conversation; ask only when a missing boundary would change the tour.

Apply [user-facing-why](../user-facing-why/SKILL.md) using project evidence and
human input. Establish who benefits, what they want to accomplish, and which
current limitation motivates the change. Resolve material gaps with the user.

This step is complete when the tour has an evidenced beneficiary, outcome, and
concrete caller scenario, and existing feedback is accounted for.

## 2. Start where someone uses the change

Place stop 01 beside a real call site, preferably an existing public-API test or
usage example. Explain the prerequisites and desired outcome in the caller's
language: "Assume you already use X and want to do Y. In your application,
import Z and call it like this."

Show enough of the actual import, input, call, and result handling to locate the
change in the caller's workflow. Tie acceptance and rejection to what the caller
can do next. Distinguish working behavior from proposed or unfinished behavior.
When no call site exists, use a clearly labeled illustrative example in a tour
comment beside the public entry point. Keep implementation and tests unchanged.

This step is complete when the reader can explain why they would use the change
and how they would call it before reading an implementation detail.

## 3. Trace the caller's path inward

Follow that call through the implementation layer by layer. At each branch,
explain what condition takes it and why its outcome matters to the original
caller. Follow the branch inward, then explicitly return to the caller or parent
branch before moving to its sibling. Choose stop boundaries around behavior.

Account for every relevant branch in the scoped change, including success,
rejection, errors, optional paths, and integration effects. Follow unchanged
supporting code where the call depends on it. Connect regression tests to the
caller behavior they prove. Group mechanical export or packaging changes with
the usage they enable instead of giving them an introductory stop.

Each stop explains the reason for the behavior, the code to inspect, and where
to go next. Flag known gaps and decisions still awaiting review at their owning
code. Scale stop count and detail to the behavior, with one coherent idea per
stop. Finish when all relevant behavior is reachable from the opening use case.

## 4. Write temporary, searchable stops

Use the user's marker, or choose a short topic marker such as `UGC-Tour`.
Give stops unique, zero-padded numbers in reading order. Use the file's native
comment syntax and indentation. Keep each marker on one header line so a search
returns one result per stop. For languages with block comments:

```ts
/** [ ] UGC-Tour(01): outcome this code enables
 * Explain the caller's need and the behavior to inspect here.
 * Next: 02.
 *
 * --- FEEDBACK/QUESTIONS:
 *
 *
 */
```

Keep prose concrete and give every stop an editable checkbox and feedback area.
Tell the user they can mark `[x]`, write questions, or delete completed stops.
Preserve their notes and completion state when revising an existing tour;
renumbering must not detach feedback from the behavior it discusses.

Only tour annotations change. For files without comments, put their explanation
at the nearest relevant call site. The tour stays in place for the user's review
until they request cleanup.

## 5. Verify and hand off

Check that numbering is unique and sequential, next-stop pointers resolve, and
examples match actual interfaces or explicitly identify proposed ones. Reconcile
the tour against the scoped diff and caller branches, including unfinished work.

Inspect the diff to verify that annotations preserve executable code and user
edits. Run appropriate formatting checks. Give the user a clickable link to
stop 01, the search marker, and instructions for leaving feedback.
