# auth-baselines

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

| Path | What it covers |
|---|---|
| `docs/CHOOSING-A-MODEL.md` | ACL, RBAC, ABAC, ReBAC and policy-as-code — what each answers, what it costs, and the signal to move |
| `case-studies/` | Worked examples from systems in production |

## Status

Early. `docs/CHOOSING-A-MODEL.md` is the first complete document. Planned, not
yet written:

- Enforcement layers — application, gateway, and database row-level security,
  with the trade-offs between them.
- Multi-tenancy — tenant isolation as an authorisation problem and the failure
  modes of getting it wrong.
- Authorisation and audit — what has to be recorded for a decision to be
  reconstructible after the fact.
- Migration paths — moving between models without a rewrite.

## Scope

Vendor-neutral. Where a concrete system is named it is as an example of a
model, not a recommendation. Where a claim comes from vendor material it is
marked as such, because most comparative writing in this area is published by
companies selling an engine.
