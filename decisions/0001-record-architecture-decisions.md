# 0001. Record architecture decisions

## Status

Accepted.

## Context

A standardisation attempt narrows as it proceeds. Scope is dropped, models are
deliberately not endorsed, and interfaces are deliberately left undefined. Each
such choice is reasonable when made and indistinguishable from an oversight six
months later, at which point someone reintroduces the problem it was avoiding.

The attempt is also expected to fail in places. Without a record of where it
was narrowed and why, a failure is indistinguishable from an omission.

## Decision

Decisions affecting the shape, scope or normative content of this repository
are recorded here as numbered records, using the format in `README.md`.

Records are immutable once accepted. A reversal is a new record that supersedes
the earlier one by number; the earlier record is not edited, because the
reasoning that was true at the time remains the explanation for anything built
on it.

## Consequences

Decisions carry a small, fixed overhead to write down, and the record has to be
kept current or it becomes misleading — a stale decision log is worse than
none, since it is read as authoritative.

In exchange, the narrowing is legible: a reader can distinguish "deliberately
out of scope" from "not yet written", which is the distinction the `spec/` and
per-tree READMEs depend on.
