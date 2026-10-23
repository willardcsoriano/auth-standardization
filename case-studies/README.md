# Case studies

## Overview

A case study here describes one real system's authorisation design as it
actually is, including the parts that are wrong, because guidance checked only
against hypotheticals tends to recommend designs nobody operates. Each study
states the system's constraints, the model it chose, where it enforces, what it
treats as an invariant, and what it got wrong and why — the last being the most
useful section and the one usually missing from published material. The purpose
is calibration rather than advocacy: a study is evidence about how a design
behaves under real constraints, not a recommendation to copy it. Systems are
described with their scale and team size, since both change which answer is
correct.

## Required sections

A study is not complete without all of these:

1. **Constraints** — team size, data sensitivity, regulatory obligations,
   operational budget.
2. **Model chosen, and the alternative rejected.**
3. **Enforcement placement**, and what is not enforced.
4. **Invariants** — the rules held outside the authorisation layer.
5. **What it got wrong**, and how it was found.

## Planned studies

| Study | System |
|---|---|
| `small-erp-flat-rbac.md` | A small trading company's internal ERP: one role for the whole team, retention enforced as a model invariant rather than a permission, and the superuser-bypass reasoning that forced that split |
