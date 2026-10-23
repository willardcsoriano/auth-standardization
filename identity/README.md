# Identity

## Overview

Authorisation presupposes that the acting principal has been established, and
a great many authorisation failures are really identity failures wearing an
authorisation costume — a session treated as a principal, a long-lived token
with no revocation path, a service account shared by four services so that no
decision can be attributed. This tree covers the establishment of a principal
and the properties an authorisation layer is entitled to assume about it. It
is deliberately narrower than a general treatment of authentication: the
concern here is the handoff, not the mechanism. Where a standard already
exists and is adoptable, this tree says so and stops, because the thesis of
this repository is that authentication is the solved half.

## Scope boundary

Authentication mechanisms themselves are standardised and need no attempt at
restatement here. What is covered is the contract they hand to authorisation:
who the principal is, how long that claim is good for, how it is revoked, and
whether the action can be attributed afterwards.

## Planned documents

| Document | Covers |
|---|---|
| `sessions-and-tokens.md` | Session versus token identity, lifetime, revocation, and why "logged in" is not a permission |
| `federation.md` | OAuth 2.0, OIDC and SAML as the adoptable layer; scope as a limit on a token, not a role |
| `mfa-and-recovery.md` | Step-up authentication for privileged actions, and recovery as the usual weakest link |
| `service-identity.md` | Non-human principals, shared accounts, and the attribution they destroy |
