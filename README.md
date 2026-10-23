# auth-standardization

## Overview

Authentication is standardised and authorisation is not. OAuth 2.0, OIDC and
SAML mean an application can adopt an identity mechanism without designing
one, while authorisation has no equivalent — every project re-derives its own
answer to "who may do what", usually under delivery pressure and usually
without recording why. This repository is an attempt at that missing baseline:
a model-by-model reference for choosing and enforcing an authorisation design,
covering application-layer checks, database-layer enforcement such as
PostgreSQL row-level security, and the point at which a rule stops being an
authorisation decision and becomes a domain invariant. It is written to be
read before building, and it carries worked case studies so the guidance can
be checked against systems that actually shipped rather than against
hypotheticals.

## Thesis

Three claims drive everything here.

**The model is a consequence of the question, not a preference.** "What may
this kind of person do" is a different question from "who may see this
particular record", and they have different right answers. Most
authorisation debt comes from answering the second question with machinery
built for the first.

**Where a rule is enforced matters as much as what it says.** The same rule
expressed in a controller, a policy, a gateway and a row-level security
predicate has four different blast radii and four different failure modes.

**Some rules are not authorisation at all.** A constraint that must bind every
account — including the operator's, including the account that bypasses the
authorisation layer — is an invariant and belongs in the data model. Expressing
it as a permission guarantees it will eventually be bypassed by the account
that was always allowed to bypass it.

## Contents

Start with `ARCHITECTURE.md`, which explains the split below and why it exists.

| Tree | Status | Holds |
|---|---|---|
| `spec/` | Normative | What a conforming system must do. The only tree using MUST/SHOULD/MAY |
| `models/` | Informative | The five designs, what each answers, and the decision procedure |
| `enforcement/` | Informative | Where a rule can be evaluated, and what each placement costs |
| `identity/` | Informative | Establishing a principal, and the contract handed to authorisation |
| `practices/` | Informative | Testing, auditing, migration, anti-patterns |
| `reference/` | Executable | Minimal working systems that prove a claim holds |
| `case-studies/` | Evidence | Real systems described as they are, including what they got wrong |
| `decisions/` | Record | Why this repository is shaped this way, and where it was narrowed |

Every tree carries a `README.md` stating its scope boundary and enumerating the
documents planned inside it.

Complete so far:

| Document | |
|---|---|
| `ARCHITECTURE.md` | The normative/informative split, the layering, the conventions |
| `GLOSSARY.md` | One meaning per term; where common usage conflicts, the conflict is named |
| `models/CHOOSING-A-MODEL.md` | The decision procedure across all five models, and the invariant boundary |
| `decisions/0001-record-architecture-decisions.md` | That decisions are recorded here |

Everything else is a stated intent with its documents enumerated, not yet
written. The distinction between "deliberately out of scope" and "not yet
written" is kept explicit on purpose.

## Scope

Vendor-neutral. Where a concrete system is named it is as an example of a
model, not a recommendation. Where a claim comes from vendor material it is
marked as such, because most comparative writing in this area is published by
companies selling an engine.
