# Reproducibility

## Status

Public Evaluation Documentation

Version: 1.0

---

# Purpose

This document defines the reproducibility principles used during engineering evaluation of the Veil Routing Protocol (VRP).

Engineering conclusions are considered trustworthy only when independent reviewers are able to reproduce observable runtime behavior under equivalent evaluation conditions.

Reproducibility is a fundamental architectural objective.

---

# Engineering Philosophy

One successful execution proves very little.

Many independent executions producing the same engineering conclusion establish confidence.

Engineering validation must be repeatable.

Observable behavior must remain consistent.

---

# Definition

A validation is considered reproducible when:

- the same scenario is executed;
- equivalent observable conditions exist;
- architectural invariants remain preserved;
- engineering conclusions remain unchanged.

Exact timing is not required.

Architectural correctness is.

---

# Scope

Reproducibility applies to:

- runtime validation
- authority transitions
- replay rejection
- recovery
- transport migration
- evidence generation
- engineering reports

Implementation remains outside the scope of reproducibility.

---

# Observable Requirements

Independent reviewers should observe:

- identical scenario definition
- equivalent runtime events
- equivalent authority progression
- equivalent recovery outcome
- equivalent validation verdict

Minor environmental differences are acceptable.

Architectural conclusions should remain identical.

---

# Reproducible Inputs

Typical observable inputs include:

- scenario configuration
- runtime version
- public configuration
- validation procedure
- observable transport events

Implementation-specific parameters remain protected.

---

# Reproducible Outputs

Typical observable outputs include:

- runtime events
- authority history
- transport history
- recovery history
- engineering verdicts
- evidence bundles

Outputs should support independent verification.

---

# Repeatability

Engineering validation should be repeatable:

- by the original evaluator;
- by an independent engineering team;
- in another compatible environment;
- after reasonable time has passed.

Repeatability strengthens engineering confidence.

---

# Environmental Differences

Equivalent engineering conclusions should remain valid despite reasonable differences in:

- hardware
- virtualization platform
- operating system
- network topology
- deployment environment

Implementation-specific optimization may differ.

Architectural correctness must not.

---

# Non-Reproducible Results

Results should be considered non-reproducible when:

- observable conclusions conflict;
- evidence is inconsistent;
- runtime behavior changes without explanation;
- architectural invariants cannot be confirmed.

Further engineering investigation is required.

---

# Engineering Validation

Independent reviewers should answer:

- Can the scenario be repeated?
- Are the observable conclusions identical?
- Are architectural invariants preserved?
- Does engineering evidence support the result?

Observable behavior determines reproducibility.

---

# Relationship to Other Documents

This document complements:

- TEST_MATRIX.md
- PASS_CRITERIA.md
- FAILURE_CRITERIA.md
- EVIDENCE_FORMAT.md
- AUDIT_GUIDE.md

It also supports:

- RFC-0005 — Evidence Model
- RFC-0011 — Pilot Integration

---

# Summary

Engineering confidence grows through reproducibility.

Observable runtime behavior should remain consistent across equivalent engineering evaluations.

Protected implementation remains unnecessary for independent validation.

---

## Design Principles

- Repeat the scenario.
- Observe the behavior.
- Compare the evidence.
- Preserve architectural invariants.
- Reach the same engineering conclusion.