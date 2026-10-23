# Reference implementations

## Overview

Prose about authorisation is cheap and frequently wrong, so this tree holds
small working systems whose job is to demonstrate that a claim made elsewhere
in the repository actually holds. Each reference is minimal by design — enough
to exhibit one model or one enforcement placement and nothing more — and each
ships with tests that fail if the guarantee it demonstrates is broken. The
tests are the point: a reference that merely runs proves only that it runs,
whereas a reference whose negative tests fail when a rule is removed proves the
rule is load-bearing. Each subdirectory states what it demonstrates, what it
deliberately omits, and how to run it.

## What a reference must include

- A statement of the single claim it demonstrates.
- Negative tests — attempts that must be refused. A reference with only
  positive tests demonstrates nothing.
- An explicit list of what it omits, so it is not mistaken for a starting
  template.

## Planned references

| Directory | Demonstrates |
|---|---|
| `postgres-rls/` | Row-level security enforcing tenant isolation against queries that bypass the application entirely |
| `rbac-laravel/` | Ability-based checks with roles as configuration; re-partitioning roles without touching application code |
| `rebac-minimal/` | Relationship-derived permissions on a small graph, and the tuple-synchronisation problem that makes the model expensive |
