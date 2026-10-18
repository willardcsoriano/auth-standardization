# Choosing an Authorisation Model

## Overview

Five models account for nearly all authorisation in production software: access
control lists, role-based access control, attribute-based access control,
relationship-based access control, and policy-as-code. They are not a maturity
ladder to climb but answers to different questions, and the common failure is
adopting a sophisticated model for a question a simpler one already answers, or
forcing a simple model to answer a question it structurally cannot. This
document states the question each model answers, what it costs to run, and the
specific signal that indicates a system has outgrown it. It closes with the
boundary that matters most in practice: the point at which a rule stops being
an authorisation decision and becomes a data-model invariant, which is where
authorisation designs most often leak.

## The decision in one pass

Answered in order, these four questions select a model. The first "yes" is
usually the answer.

1. **Does the decision depend only on what kind of user is acting?** → RBAC.
2. **Does it depend on values in the request or the record — amount, region,
   time, ownership field?** → ABAC, or RBAC with a policy predicate.
3. **Does it depend on a relationship between the user and one specific
   object — shared with, member of, inherited from a parent?** → ReBAC.
4. **Do the rules change independently of deploys, or need to be audited and
   tested as artefacts in their own right?** → policy-as-code.

Most internal business applications stop at question 1 and should.

## The models

### Access control lists

**Answers:** who is on this object's list.

The oldest and most direct form: each object carries the principals permitted
to act on it. Cheap at small scale and immediately understandable.

**Breaks when** the number of objects grows, because permissions live in as
many places as there are objects and there is no way to answer "what can this
user reach" without scanning all of them. Also breaks on departure: removing a
person means finding every list they appear on.

### Role-based access control

**Answers:** what may this kind of person do.

Permissions are grouped into roles; users hold roles. Formalised by Sandhu and
colleagues in the mid-1990s and later standardised, with three recognised
tiers: core RBAC, hierarchical RBAC where roles inherit, and constrained RBAC
which adds separation-of-duty rules.

**The discipline that makes it work:** application code checks *permissions*,
never role names. `can('invoice.delete')`, never `hasRole('manager')`. Roles
are then bundles that can be redrawn without touching code, and a
re-partitioning is a configuration change. A codebase that branches on role
names has hard-coded one particular org chart into its control flow, and every
later reorganisation becomes a code change.

**Breaks when** role count climbs, because each new distinction multiplies
rather than adds — the classic role-explosion failure, where roles like
`manager-region-2-readonly` begin to appear. Vendor guidance commonly puts the
comfortable ceiling around five to ten roles; treat that as a rough signal
rather than a measured threshold. It also breaks the first time a decision
depends on *which* record rather than which kind of user.

### Attribute-based access control

**Answers:** what do the attributes of this request permit.

Decisions are computed from attributes of the subject, the object, the action
and the environment. Described in NIST guidance on the model. Naturally
expresses rules RBAC cannot: approval limits by amount, access by region,
restrictions by time of day.

**Cost:** a decision is no longer a lookup but an evaluation, so it is harder
to answer "who can do X" in reverse, and harder to render a UI that needs to
know in advance what to show.

### Relationship-based access control

**Answers:** can this user act on *this specific object*.

Permissions derive from a graph of relationships — user is a member of a team,
the team owns a folder, the document inherits from the folder. Google's
Zanzibar paper (USENIX ATC, 2019) described the design behind sharing in Docs,
Drive and Calendar, and the open implementations that followed are modelled on
it.

**Cost, and it is the real one:** a separate stateful service holding
relationship tuples, kept consistent with the application's own data. Every
write that changes a relationship must also write a tuple, and divergence
between the two is a security bug that is silent by construction.

**Adopt only when** per-object sharing is an actual product requirement. If
every user sees every record of a given type, there is no relationship graph
to model and the machinery is pure cost.

### Policy-as-code

**Answers:** whatever the policy says, evaluated outside the application.

Rules are expressed in a dedicated language and evaluated by an engine, which
makes them reviewable, testable and deployable independently. Cedar, developed
by AWS and open-sourced, and OPA with its Rego language are the common
examples.

**Fits when** policy genuinely changes on a different cadence than code, or
must be enforced identically across several services in different languages.
Within a single application it often adds indirection without removing any.

## The engines, and a caveat about the literature

Zanzibar-descended ReBAC engines include OpenFGA (originating at Auth0/Okta
and reported as a CNCF Sandbox project), SpiceDB, Permify and Ory Keto. On the
policy-language side, Cedar and OPA are the common choices; one comparison
notes that Rego suits complex attribute-based rules better, and that some
ReBAC engines deliberately omit a policy language entirely.

**The caveat matters.** Nearly all comparative writing in this area is
published by companies selling one of these engines. A neutral head-to-head is
difficult to find. Treat feature comparisons as marketing until verified
against the software, and prefer the original Zanzibar paper to any summary of
it.

## Where to enforce

The model is half the design; the other half is placement. The same rule has
different properties depending on where it lives.

| Layer | Property | Weakness |
|---|---|---|
| Controller / request handler | Easy to read next to the action | Every new entry point must remember it |
| Policy / domain service | One definition per resource, reusable | Only binds callers that go through it |
| API gateway | Binds every caller, language-agnostic | Cannot see record contents |
| Database row-level security | Binds every query, including ad-hoc ones | Policy expressed far from the domain, hard to test |

Database-level enforcement is the strongest guarantee available, because it
holds even for a query written by hand against the database, and it is the
right tool for tenant isolation specifically. Its cost is that the rule now
lives in a migration rather than in the domain model, where it is less
discoverable and harder to unit-test.

Enforcing at more than one layer is defence in depth, but only if each layer's
rules are generated from one source. Two hand-maintained copies of the same
rule diverge, and the divergence is discovered by a security incident.

## The boundary that leaks: invariants are not authorisation

The most consequential mistake in this area is expressing an invariant as a
permission.

Most authorisation systems have a superuser path — an administrative role, a
gate hook, a service account — that is allowed everything by design. A rule
implemented as a withheld permission therefore binds ordinary users and not
that path. If the rule exists to protect ordinary users from mistakes, that is
adequate. If it exists because the thing must never happen, it is not: the
account most likely to perform a destructive administrative action is exactly
the one the rule does not constrain.

The test is a question: **should this still be prevented if the person acting
is the owner of the system?** If yes, it is an invariant, and it belongs in the
data model — a constraint, a trigger, a guard on the model's own lifecycle —
where no authorisation layer can reach past it.

Financial records are the standard example. "Only administrators may delete a
posted invoice" is an authorisation rule and the wrong one. "A posted invoice is
never deleted; it is reversed by a second document" is an invariant, and it is
also what double-entry bookkeeping has done for centuries. The domain had the
answer before the authorisation literature existed.

## Provenance

The model names, originating authors and the Zanzibar paper are stable
history. The engine landscape and the role-count guidance come from a web
survey of mostly vendor-authored material and are marked as such above; they
are the parts of this document most likely to age.

Sources consulted:

- <https://openfga.dev/docs/learn/abac-vs-rebac>
- <https://auth0.com/blog/rebac-abac-openfga-cedar/>
- <https://guptadeepak.com/ciam-compass/guides/rbac-vs-abac-vs-rebac/>
- <https://www-webflow.osohq.com/learn/spicedb-alternatives-authorization-tools-comparison>
