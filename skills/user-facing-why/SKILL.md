---
name: user-facing-why
description: User-facing why discovery from project evidence and human input.
  Apply when grounding current work in a beneficiary-valued outcome.
---

# User-facing why

A user-facing why is an evidenced outcome that an affected beneficiary values
in their own context. It names the capability, constraint, or result the work
supports in the beneficiary's domain language.

The practice produces a grounded understanding held in the working context.
Consuming workflows decide how to encode that understanding in commits,
tickets, documents, or other artifacts.

## Interaction modes

Every retained causal link is grounded in project evidence, observed behavior,
or human input. Relevant evidence includes tickets, decision records,
documentation, existing trails of why, and the current work.

### HITL

Interview the user about unresolved causal links, updating the causal chain
after each answer. HITL completes when the user confirms the highest supported
why and its causal connection to the current work.

### AFK

Use the highest why supported by the available evidence. A local or technical
why is a valid AFK result.

## Outside-in

Trace the work from the beneficiary's context toward the product limitation.

1. Who benefits from the work?

Identify the affected beneficiary from project evidence or human input. The
beneficiary may be a person using an interface, a service consuming an API, an
engineer using a library, internal staff, or another team. Name the specific
role and the context in which they receive the benefit.

2. What outcome does the beneficiary value?

Identify the capability, constraint, or result they care about in their own
context. Describe success in language meaningful to that beneficiary.

3. Which current condition limits that outcome?

Identify the condition separating the beneficiary's current experience from
the outcome they value, and why changing this product is an appropriate
intervention.

## Inside-out

Trace the current work through its driving conditions toward the beneficiary
outcome.

Start from the current work and ask:

- What condition or decision made this work necessary?
- What outcome does addressing that condition support?
- Who values that outcome, and in what context?

Continue along the causal chain while each next link is grounded and materially
changes the understanding of why the work exists.

## Reconcile

Reconcile the paths into the highest grounded why supported by the available
evidence. Resolve divergences through project evidence or human input.

The result qualifies as a user-facing why when it identifies an evidenced
beneficiary and an outcome they value. A grounded chain that ends earlier
remains a local why and can still guide the current work.
