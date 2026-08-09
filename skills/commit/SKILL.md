---
name: commit
description: Commit staged changes as a golden commit with human-in-the-loop reconciliation.
disable-model-invocation: true
---

# Commit

Commit the staged changes as a golden commit. The staged diff is the authorized
changeset: preserve unstaged and untracked work, and keep every commit within
that boundary.

## 1. Inspect the staged snapshot

Inspect the repository status and complete staged diff. Stop with a concise
acknowledgement when nothing is staged.

Assess **commit readiness**:

- Account for every staged change.
- Identify evidence of its rationale, scope, and release impact.
- Surface staged inclusions that appear accidental or sensitive.

This is a readiness inspection, not a general implementation-quality review.
Keep the inspected staged snapshot as the baseline for later validation.

Inspection is complete when every staged change is accounted for and no
material readiness concern remains unresolved.

## 2. Establish the why and its trail

Read the available /trail-of-why when context or repository artifacts provide
a starting breadcrumb. Gather material antecedents such as tickets, decisions,
specifications, conversations, documentation, and related commits.

Use /user-facing-why in HITL mode to establish the highest grounded why and
its causal connection to the staged work. Ask only to close gaps; invocation of
this skill already authorizes committing once the required understanding is
present.

Rationale reconciliation is complete when the user confirms the highest
grounded why and every material causal dependency is either captured in the
commit's scoped rationale or linked as an antecedent.

## 3. Reconcile scope and metadata

Apply the scope criteria from /golden-commit. Scope smells trigger HITL
reconciliation. With explicit user approval, the staged changes may be
partitioned into multiple coherent commits. Preserve unstaged and untracked
work, and keep every resulting commit within the original staged boundary.

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
release impact from the public behavior they affect.

When compatibility impact is unclear, ask the user. A breaking change carries
the consumer impact and migration path required by the project's format, or by
the Conventional Commits fallback.

Add co-author trailers when context shows that an agent materially authored the
staged changes and its identity is grounded in repository convention, current
context, or human input. Commit-message writing, research, and review alone do
not constitute authorship; resolve ambiguous attribution with the user.

Reconciliation is complete when every planned commit has one coherent why and
all required release, reference, and attribution metadata is grounded.

## 4. Write the commit message

Apply /golden-commit in the project's established format. Encode the commit
as a breadcrumb under /trail-of-why, using the commit format for scoped
rationale and causal reference links.

The message is complete when it satisfies the project format, the golden-commit
criteria, the breadcrumb contract, and the reconciled metadata for its staged
changes.

## 5. Commit

Revalidate the staged snapshot immediately before each commit. A change outside
an approved partition restarts inspection against the new snapshot.

Execute `git commit` with repository hooks and the user's signing settings
intact. On failure, preserve the staged work, report the actionable error, and
return control to the user. Retry after the underlying problem is resolved and
the staged snapshot is revalidated.

## 6. Acknowledge

Briefly acknowledge the completed commit with its SHA, then continue any
higher-level workflow or yield to the user.
