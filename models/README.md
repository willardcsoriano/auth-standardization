# Models

## Overview

Five designs account for nearly all authorisation in production software, and
they are not a maturity ladder but answers to different questions. This tree
catalogues them: what question each answers, the shape it takes in a schema,
what it costs to operate, and the specific signal that a system has outgrown
it. The catalogue is informative rather than normative, because selecting a
model is a design decision that depends on the product, and a specification
that chose one for every system would be wrong for most of them. Start with
`CHOOSING-A-MODEL.md`, which is the decision procedure; the per-model
documents are the detail behind each branch of it.

## Contents

| Document | Covers |
|---|---|
| `CHOOSING-A-MODEL.md` | The decision procedure across all five, and the invariant boundary |

## Planned documents

One per model, each following the same structure — the question it answers,
its schema shape, its operational cost, its failure mode, and the signal to
move:

- `acl.md` — access control lists
- `rbac.md` — role-based, including the core / hierarchical / constrained tiers
- `abac.md` — attribute-based
- `rebac.md` — relationship-based, and the Zanzibar lineage
- `pbac.md` — policy-as-code
- `capability.md` — capability-based, rare in business software and worth
  understanding for why
