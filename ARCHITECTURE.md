# Architecture

## Overview

This repository is organised around a single distinction: what an
authorisation standard would *require* versus what a practitioner needs to
*know* in order to meet it. The `spec/` tree holds the normative attempt,
written in conformance language, and everything else is informative material
that explains, justifies or demonstrates it. That split exists because the
failure mode of a document like this is becoming a long essay nobody can
conform to, or a rulebook nobody can apply. Each top-level directory below has
one job, states its own scope boundary, and lists the documents planned inside
it, so the shape of the whole is legible before any one part is finished.

## The normative / informative split

| Tree | Status | Answers |
|---|---|---|
| `spec/` | Normative | What a conforming system must do |
| `models/` | Informative | What the available designs are and when each fits |
| `enforcement/` | Informative | Where a rule can be placed and what that placement costs |
| `identity/` | Informative | How a principal is established before anything is authorised |
| `practices/` | Informative | How to test, audit, migrate, and what to avoid |
| `reference/` | Executable | Working implementations that demonstrate conformance |
| `case-studies/` | Evidence | Real systems, described as they actually are |
| `decisions/` | Record | Why this repository is shaped the way it is |

Only `spec/` uses MUST / SHOULD / MAY. If a sentence elsewhere reads like a
requirement, it is misplaced and belongs in the spec.

## Layering

The trees are ordered by how far they sit from a running system:

```
identity/        establishes who is acting
   ↓
models/          decides the shape of the rule
   ↓
enforcement/     decides where the rule is evaluated
   ↓
spec/            states what any of the above must guarantee
   ↓
practices/       keeps it honest over time (tests, audit, migration)
   ↓
reference/       proves the guarantees hold in code
```

`case-studies/` cuts across all of them: a case study describes one real
system's choices at every layer, which is the only way to check the guidance
against something that shipped.

## Why a spec at all

Authentication can be adopted; authorisation has to be designed. The
consequence is that every project re-derives the same decisions and records
none of them, so the reasoning is lost at the first handover and the design
degrades into accumulated special cases. A specification is an attempt to make
those decisions nameable: not to prescribe one model, but to state what any
model owes — that decisions are enforced somewhere identifiable, that
invariants are distinguished from permissions, that a decision can be
reconstructed after the fact.

The attempt may fail. It is scoped as an attempt deliberately, and
`decisions/` records where it has been narrowed.

## Conventions

- **One claim, one home.** A rule lives in `spec/` and is explained once
  elsewhere, by reference. Duplicated prose diverges.
- **Vendor material is marked.** Most comparative writing on authorisation is
  published by companies selling an engine; where such a source is used it is
  identified as one.
- **Evidence over assertion.** A claim about how a system behaves belongs in
  `case-studies/` or `reference/`, where it can be checked, not in prose.
- **Examples name a model, not a product.** Products appear as instances of a
  design, never as recommendations.
- **Stable history is separated from moving detail.** Papers, authors and
  established models age well; engine feature matrices and maturity tiers do
  not, and are marked as the parts most likely to be stale.
