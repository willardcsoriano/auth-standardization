# Specification

## Overview

This tree is the normative attempt: the part of the repository written as
requirements rather than advice. It does not prescribe an authorisation model,
because the right model is a consequence of the question a system is
answering, and a standard that picks one would be wrong for most systems. What
it does instead is state what any model owes regardless of which is chosen —
that a decision is enforced at an identifiable point, that invariants are
separated from permissions, that a past decision can be reconstructed, and that
isolation boundaries are enforced rather than assumed. Conformance language is
used strictly here and nowhere else in the repository.

## Conformance language

MUST, MUST NOT, SHOULD, SHOULD NOT and MAY carry their usual specification
meanings. A system conforms if it satisfies every MUST; SHOULD marks a
requirement with legitimate exceptions, and each such exception must be
recorded by the implementer rather than assumed.

## What this specification does not do

- It does not select a model. That is `models/`.
- It does not mandate a product, engine or language.
- It does not cover authentication mechanisms, only the guarantee that a
  principal has been established before a decision is made.
- It does not define a wire format or an API. A standard interface is a
  reasonable later goal and is deliberately out of scope for a first attempt.

## Planned documents

Not yet written. The numbering is the intended reading order:

| Document | States |
|---|---|
| `00-scope.md` | What kinds of system this applies to, and where it stops |
| `01-core-concepts.md` | The object model the requirements assume — principal, resource, action, ability |
| `02-decision-model.md` | Decision inputs and outputs; where PEP, PDP, PIP and PAP must be identifiable |
| `03-enforcement.md` | That every decision has an enforcement point, and what a missing one means |
| `04-invariants.md` | That rules binding every principal are not expressed as permissions |
| `05-auditability.md` | What must be recorded for a decision to be reconstructible |
| `06-delegation.md` | Acting on behalf of another principal, and attribution requirements |
| `07-multi-tenancy.md` | Isolation as a requirement rather than a convention |
| `08-conformance.md` | The checklist, and what a conformance claim means |
