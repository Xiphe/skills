# Read feedback and clean up

## 1. Read before removing

Read every remaining stop in the requested tour, including notes outside the
feedback area and comments the user added nearby. A checked box means read;
a deleted stop supplies no evidence of approval.

Associate each question or requested change with its code location. Investigate
questions against the code and available project evidence. Separate answered
questions, authorized changes, and decisions the user wants to discuss. Reading
feedback alone leaves the tour in place; remove it when cleanup is requested.

This step is complete when every note is understood and accounted for in the
working context. Before deletion, retain a verbatim recovery copy outside the
repository if the tour is the only record of the feedback.

## 2. Remove only the tour

Remove the requested marker's annotation blocks, preserving implementation,
unrelated comments, other tours, and user code edits. Review the resulting diff
against the pre-cleanup snapshot; do not reset files to an older revision.

Verify that the requested markers are gone and removal changed only tour text.
Run appropriate formatting checks. Keep any separately authorized implementation
changes distinguishable from annotation removal during verification.

Cleanup is complete when the annotations are gone and every feedback item is
answered or carried into the task response with a stable code reference. Report
open decisions for discussion without treating cleanup as their resolution.
