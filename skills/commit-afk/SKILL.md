---
name: commit-afk
description: Autonomous golden-commit workflow for a clear current changeset.
  Apply when agent work is ready to commit.
---

# Commit AFK

Commit the changes attributable to the current task without requiring human
interaction. Use the available context and repository evidence to exercise the
required rationale, provenance, scope, and release judgment.

## 1. Establish the changeset

Inspect the repository status and relevant diffs. Identify the changes that
belong to the current task from the working context, agent actions, and
repository state.

Existing staging is evidence, not a prerequisite. Stage the clear current
changeset and preserve unrelated staged, unstaged, and untracked work. Ambiguity
narrows the selected changeset; include only changes attributable to the task.

Assess commit readiness by accounting for every selected change, its scope and
release impact, and any inclusion that appears accidental or sensitive. Stop
with a concise reason when there is no clear changeset or a material readiness
concern remains.

## 2. Establish the why and its trail

Read the available /trail-of-why when context or repository artifacts provide
a starting breadcrumb. Gather material antecedents such as tickets, decisions,
specifications, conversations, documentation, and related commits.

Use /user-facing-why in AFK mode. Follow the available evidence to the highest
grounded why; a shallow local or technical why is a valid result. Preserve
uncertainty at the evidence horizon and keep every causal claim attributable to
context, repository evidence, or the selected changes.

Rationale is complete when the selected changes have a grounded why and every
material causal dependency is either captured in scoped rationale or linked as
an antecedent.

## 3. Reconcile scope and metadata

Apply the scope criteria from /golden-commit. Partition the clear changeset
when distinct whys or independent states support multiple coherent commits.
Keep every partition attributable to the current task and preserve unrelated
work.

Inspect repository instructions, configuration, and history for commit and
release conventions. In their absence, use Conventional Commits with the
default semantic-release meanings:

- `fix` and `perf` communicate patch impact.
- `feat` communicates minor impact.
- A `BREAKING CHANGE:` footer communicates major impact.

Evaluate breaking compatibility at the project's already released, outermost
public seam: published package APIs, public service or wire APIs, public CLIs,
and other explicitly supported contracts. UI behavior is a compatibility seam
when the project defines it as one. Internal or unreleased contracts take their
release impact from the public behavior they affect. Mark a change as breaking
when available evidence establishes that released consumer impact and its
migration path.

Add the running agent as a co-author of every commit using its established
co-author identity.

Reconciliation is complete when every planned commit has one coherent why and
all release, reference, and attribution metadata supported by available
evidence is accounted for.

## 4. Write the commit message

Apply /golden-commit in the project's established format. Encode each commit
as a breadcrumb under /trail-of-why, using the commit format for scoped
rationale and causal reference links.

The message is complete when it satisfies the project format, the golden-commit
criteria, the breadcrumb contract, and the grounded metadata for its changes.

## 5. Commit

Stage and revalidate each planned changeset immediately before committing it.
Execute `git commit` with repository hooks and signing settings intact. On
failure, preserve the work, report the actionable error, and stop. Recovery
keeps repository safeguards and unrelated changes intact.

## 6. Acknowledge

Briefly acknowledge each completed commit with its SHA, then continue any
higher-level autonomous workflow or finish the current task.
