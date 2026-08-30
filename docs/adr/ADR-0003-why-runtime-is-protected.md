# ADR-0003 — Why the Runtime is Protected

**Status:** Accepted

**ADR Number:** 0003

**Date:** 2026

**Category:** Architecture / Security

---

# Context

Engineering teams frequently request access to implementation when evaluating a new runtime.

Typical requests include:

- source code review
- internal algorithms
- protocol implementation
- packet formats
- optimization logic
- scheduling internals

While such requests are understandable, they are not always necessary to determine whether a runtime behaves correctly.

The VRP architecture intentionally separates observable engineering behavior from implementation.

---

# Problem

Implementation disclosure introduces several engineering risks.

These include:

- exposure of proprietary technology
- unnecessary implementation coupling
- loss of intellectual property
- increased reverse engineering risk
- pressure to evaluate implementation instead of behavior

The architecture requires an evaluation model that measures runtime correctness without exposing implementation.

---

# Decision

The protected runtime remains outside the public specification.

Engineering evaluation is performed using:

- observable behavior
- deterministic execution
- engineering evidence
- reproducible validation
- runtime invariants

Implementation remains confidential.

---

# Decision Drivers

The decision is based upon the following engineering objectives.

- protect implementation
- preserve independent validation
- reduce unnecessary disclosure
- support reproducible evaluation
- maintain architectural transparency
- protect long-term intellectual property

---

# Alternatives Considered

## Alternative A

Publish the complete runtime.

Advantages:

- maximum implementation transparency

Disadvantages:

- exposes proprietary implementation
- increases cloning risk
- removes implementation boundary

Rejected.

---

## Alternative B

Publish partial implementation.

Advantages:

- easier technical review

Disadvantages:

- unclear confidentiality boundary
- incomplete engineering picture
- encourages implementation-focused evaluation

Rejected.

---

## Alternative C

Evaluate observable behavior only.

Advantages:

- implementation remains protected
- engineering validation remains independent
- reproducible testing
- architectural transparency

Accepted.

---

# Consequences

Engineering discussions shift from implementation details toward observable behavior.

Independent reviewers evaluate:

- deterministic execution
- continuity
- authority evolution
- recovery
- replay rejection
- engineering evidence

The protected runtime remains confidential.

---

# Benefits

The decision provides:

- reproducible engineering validation
- implementation independence
- stronger intellectual property protection
- clear architectural boundaries
- long-term maintainability

---

# Trade-offs

The runtime provider must invest significantly in documentation and validation.

Observable behavior must be sufficiently detailed to allow independent engineering assessment.

This increases documentation effort while reducing implementation disclosure.

---

# Architectural Impact

This decision directly supports:

RFC-0005 — Evidence Model

RFC-0009 — Security Boundary

RFC-0010 — Runtime API

RFC-0011 — Pilot Integration

RFC-0012 — Threat Model

The Security Boundary depends on this architectural decision.

---

# Validation

Independent reviewers should be able to answer:

- Does the runtime preserve continuity?
- Are runtime decisions deterministic?
- Is authority progression correct?
- Is replay rejected?
- Can engineering conclusions be reproduced?

These questions do not require implementation disclosure.

---

# Status

Accepted.

Protected implementation remains a permanent architectural boundary.

Future releases may expand observable validation without exposing implementation.

---

# Design Statement

Engineering confidence should come from observable behavior.

Evidence should replace assumptions.

Implementation should remain protected.

Observable correctness is sufficient for technical evaluation.