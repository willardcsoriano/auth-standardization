# Practices

## Overview

An authorisation design degrades in predictable ways, and this tree is about
keeping one honest after it ships. Three forces do the damage: rules that are
never tested and therefore silently stop enforcing, decisions that cannot be
reconstructed once someone asks what happened, and models that are outgrown
but migrated by accretion rather than by design. The documents here address
each, plus a catalogue of the anti-patterns that recur often enough to be
worth naming. The emphasis throughout is on what can be mechanically checked,
because a convention that depends on everyone remembering it is a convention
that has already failed.

## Planned documents

| Document | Covers |
|---|---|
| `testing-authorization.md` | Proving a rule enforces rather than that it exists; negative tests as the load-bearing ones; pinning the model's shape so a change fails a test |
| `auditing-decisions.md` | What must be recorded for a decision to be reconstructible; attribution; why an immutable log becomes more important as roles flatten |
| `migration-paths.md` | Moving between models without a rewrite; the indirection that makes it cheap; sequencing a migration behind a running system |
| `anti-patterns.md` | Branching on role names; the invariant expressed as a permission; the superuser that bypasses the rule it was meant to obey; role explosion; permissions as a UI concern |
