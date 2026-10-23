# Decisions

## Overview

This tree records why the repository is shaped the way it is, in the
lightweight architecture-decision-record form: one file per decision, stating
the context, the choice, and the consequences accepted along with it. The
reason to keep them is that a standardisation attempt accumulates narrowing
decisions — scope deliberately dropped, a model deliberately not endorsed, an
interface deliberately left undefined — and without a record those read later
as oversights and get "fixed" by someone reintroducing the problem they
avoided. Records are immutable once accepted: a reversal is a new record that
supersedes the old one, never an edit. Each file is named with a four-digit
sequence and a slug.

## Format

```
# NNNN. Title

## Status
Accepted | Superseded by NNNN

## Context
What forced a decision.

## Decision
What was chosen.

## Consequences
What this costs, and what it rules out.
```

## Records

| Record | Decision |
|---|---|
| `0001-record-architecture-decisions.md` | That decisions affecting this repository's shape are recorded here |
