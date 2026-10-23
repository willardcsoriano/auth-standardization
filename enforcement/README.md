# Enforcement

## Overview

Choosing a model is half of an authorisation design; the other half is where
the rule is evaluated, and that placement determines what the rule actually
guarantees. The same requirement expressed in a request handler, a domain
policy, an API gateway and a database predicate has four different blast
radii and four different ways of being silently bypassed — a handler binds
only the callers that reach it, a gateway cannot see record contents, a
database predicate binds every query but lives far from the domain it
describes. This tree covers each placement on its own terms and then the
question that matters most in practice: how to enforce in more than one place
without maintaining two copies of a rule that will diverge.

## The central claim

Defence in depth is worthwhile only when every layer's rules derive from one
source. Two hand-maintained copies of the same rule drift, and the drift is
discovered by an incident rather than by a test.

## Planned documents

| Document | Covers |
|---|---|
| `application-layer.md` | Handlers, domain policies and services; what each binds and misses |
| `api-gateway.md` | Coarse enforcement ahead of the application, and its blindness to record contents |
| `database-rls.md` | Row-level security: the strongest guarantee available, and the cost of expressing a rule in a migration |
| `defence-in-depth.md` | Layering without duplication; generating layers from one definition |
