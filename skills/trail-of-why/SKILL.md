---
name: trail-of-why
description: Trail-of-why practice for preserving and recovering rationale
  across project artifacts. Apply when leaving a /trail-of-why or reading one
  during why-archeology.
---

# Trail of why

A /trail-of-why is causal provenance preserved across project artifacts. Each
breadcrumb carries enough of the user-facing why already understood in context
to explain its own existence, and points to antecedents that supplied its
evidence, rationale, constraints, or decisions.

Commits, tickets, documents, chat messages, and meeting notes can all act as
breadcrumbs. Their artifact-specific workflows own the encoding format.

This practice works with the user-facing-why understanding already present in
the working context. The `user-facing-why` practice owns discovering,
synthesizing, and interviewing for that understanding.

## Breadcrumb contract

A breadcrumb preserves:

- **Scoped rationale** — enough beneficiary-facing context to explain why this
  artifact exists without first following a link. Express the understanding at
  the artifact's scope.
- **Causal provenance** — links to the available antecedents whose evidence,
  rationale, constraints, or decisions materially informed the artifact.
- **Native encoding** — the rationale and links follow the artifact's own
  conventions. A golden commit, ticket, or meeting note may encode the same
  understanding differently.

A trail link points from a breadcrumb to an antecedent that informed it. Label
the relationship in artifact-appropriate language such as `implements`,
`decided-by`, `motivated-by`, `fixes`, or `supersedes`, so following the link has
a clear purpose. New breadcrumbs normally point backward; later artifacts own
any forward links they add.

## Leave a /trail-of-why

1. Confirm that the working context contains a grounded user-facing-why
   understanding for the artifact. A missing or unclear understanding ends this
   practice with that precondition reported.
2. Encode the part of that understanding needed to explain the artifact's
   existence in its own scope and format.
3. Link each available antecedent whose absence would break or materially alter
   that explanation. State the causal relationship each link preserves.

The breadcrumb is complete when its local rationale explains its existence and
every material causal dependency is either captured locally or reachable
through a purposeful link.

## Read a /trail-of-why

1. Start from the given breadcrumb. Extract its scoped rationale and the causal
   relationship of each antecedent link.
2. Follow those antecedents recursively, tracking visited artifacts until the
   trail reaches its highest documented why or has no further causal link.
3. Reconstruct the documented causal path in the working context. Keep every
   link attributable to its source, represent gaps as gaps, and retain
   conflicting accounts side by side.

The reading is complete when the context contains the breadcrumb's local
rationale, the evidence and decisions behind it, the highest documented why the
trail reaches, and the source for every reconstructed link. The recovered
understanding remains in working context until another workflow encodes it.
