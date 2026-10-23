# Glossary

## Overview

Authorisation discussions fail more often on vocabulary than on substance:
"role", "permission", "policy" and "scope" each mean several incompatible
things depending on which product or paper is speaking, and two people can
agree on every sentence while designing different systems. This glossary fixes
one meaning per term for use throughout this repository, chosen to match the
academic literature where it is settled and stated explicitly where it is not.
Terms are defined by the question they answer rather than by synonym, so a
definition can be checked against a real system. Where common usage conflicts
with the definition here, the conflict is named rather than smoothed over.

## Core

**Principal** — the entity a decision is made about. A human, a service, a
device, or a session acting on behalf of one of those. Deliberately broader
than "user", because service-to-service authorisation is the case most often
designed last and worst.

**Subject** — the principal in a specific decision. The same principal is a
different subject in a different request.

**Resource** — the thing acted upon. Note that "resource" in most frameworks
means a *type* (invoices), while an authorisation decision usually concerns an
*instance* (this invoice). Where the distinction matters, this repository says
resource type or resource instance.

**Action** — what is being attempted. Called an *ability* when named
independently of any resource instance.

**Ability** — a named capability, such as `invoice.delete`. The unit an
application should check. Distinguished from a role because the whole point of
the distinction is that code checks abilities and configuration assigns them.

**Role** — a named bundle of abilities. A role is configuration, not a
capability; a system whose code branches on role names has made its
organisational chart part of its control flow.

**Permission** — an ability granted to a principal, directly or through a role.
Used as the grant, never as the capability itself.

**Policy** — a rule or set of rules producing a decision. In this repository a
policy is always a *rule*, never a *document* and never a framework class,
both of which are called out explicitly where meant.

## Decision architecture

Borrowed from the access-control literature and used consistently:

**PEP — policy enforcement point** — the place in the running system where the
decision is applied and the action is permitted or refused.

**PDP — policy decision point** — the place where the decision is computed.

**PIP — policy information point** — where the facts a decision needs are
fetched from.

**PAP — policy administration point** — where rules are authored and managed.

A design that cannot say where each of these lives has not been designed; it
has accumulated.

## Boundaries

**Invariant** — a rule that must hold regardless of who acts, including the
operator and including a superuser path. An invariant is not an authorisation
decision and does not belong in the authorisation layer. The test: *should
this still be prevented if the person acting owns the system?*

**Authorisation decision** — a rule about who may do something. Distinguished
from an invariant precisely because most systems have a path that is permitted
everything by design, and a rule placed in the authorisation layer is a rule
that path does not obey.

**Authentication** — establishing which principal is acting. A precondition
of authorisation, not a part of it; conflating them produces systems that
authorise sessions rather than principals.

**Delegation** — a principal acting with another's authority, with that fact
recorded. Distinguished from **impersonation**, where the fact is not recorded
and the audit trail attributes the action to the wrong principal.

## Terms used with care

**Scope** — in OAuth, a limit on what a token may request; not a role and not
an ability, though it is routinely treated as both. This repository uses
"scope" only in its OAuth sense.

**Tenant** — an isolation boundary, not a customer. One customer may hold
several tenants and one tenant may serve several customers; designing as
though they are the same produces isolation bugs.

**Least privilege** — granting the minimum needed for the task, as stated in
the foundational protection literature. Not a synonym for "few roles"; a
system with two roles can violate it and a system with twenty can satisfy it.
