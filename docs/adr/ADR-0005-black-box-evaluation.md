# ADR-0005 — Black-Box Evaluation

**Status:** Accepted

**ADR Number:** 0005

**Date:** 2026

**Category:** Engineering Validation

---

# Context

Engineering organizations frequently evaluate technologies whose internal implementation is unavailable.

Examples include:

- commercial databases
- networking appliances
- industrial controllers
- hardware security modules
- cloud infrastructure
- operating systems

Engineering confidence is established through observable behavior rather than implementation disclosure.

The VRP architecture adopts the same evaluation philosophy.

---

# Problem

A common assumption is that implementation disclosure is required before meaningful engineering evaluation can occur.

This assumption creates several problems.

Organizations begin reviewing implementation rather than validating observable runtime behavior.

Attention shifts from engineering correctness toward implementation details.

This weakens objective technical assessment.

---

# Decision

VRP adopts a Black-Box Evaluation model.

The protected runtime remains confidential.

Observable behavior becomes the evaluation surface.

Engineering conclusions are derived from:

- deterministic execution
- observable runtime events
- continuity preservation
- authority evolution
- replay rejection
- recovery behavior
- engineering evidence

Implementation remains outside the evaluation boundary.

---

# Decision Drivers

The decision supports:

- objective engineering validation
- reproducibility
- implementation independence
- intellectual property protection
- consistent evaluation methodology
- long-term architectural stability

---

# Alternatives Considered

## Alternative A

Evaluate implementation.

Advantages:

- implementation visibility

Disadvantages:

- implementation bias
- unnecessary disclosure
- increased intellectual property risk

Rejected.

---

## Alternative B

Evaluate marketing demonstrations.

Advantages:

- simple communication

Disadvantages:

- subjective conclusions
- poor reproducibility
- insufficient engineering confidence

Rejected.

---

## Alternative C

Evaluate observable runtime behavior.

Advantages:

- objective
- reproducible
- implementation independent
- suitable for third-party validation

Accepted.

---

# Consequences

Engineering evaluation focuses upon measurable runtime behavior.

Reviewers validate:

- observable state evolution
- continuity
- deterministic decisions
- recovery
- engineering evidence

The implementation remains protected.

---

# Benefits

The decision provides:

- reproducible validation
- implementation confidentiality
- objective technical review
- engineering transparency
- stronger intellectual property protection

---

# Trade-offs

Observable behavior must be sufficiently documented.

Engineering evidence must be comprehensive.

Validation infrastructure becomes an essential architectural component.

---

# Architectural Impact

This decision directly supports:

RFC-0005 — Evidence Model

RFC-0009 — Security Boundary

RFC-0011 — Pilot Integration

RFC-0012 — Threat Model

Black-Box Evaluation becomes the default engineering methodology for VRP.

---

# Validation

Engineering reviewers should be capable of answering:

- Does the runtime preserve continuity?
- Are runtime decisions deterministic?
- Are architectural invariants maintained?
- Can the evidence be independently reproduced?

None of these questions require implementation disclosure.

---

# Status

Accepted.

Future evaluation tooling may evolve.

The Black-Box Evaluation principle remains unchanged.

---

# Design Statement

Observable behavior is the engineering contract.

Implementation is the engineering asset.

Engineering validation should verify the first without requiring the second.