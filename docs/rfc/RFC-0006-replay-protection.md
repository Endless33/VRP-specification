# RFC-0006 — Replay Protection

**Document Status:** Public Specification

**RFC Number:** RFC-0006

**Version:** 1.0

**Category:** Security

---

# Abstract

This document defines the observable Replay Protection model of the Veil Routing Protocol (VRP).

Replay Protection ensures that previously accepted execution cannot be reused to influence future runtime behavior.

The objective is deterministic execution under adversarial conditions while preserving implementation confidentiality.

Replay Protection validates execution freshness.

Authority validation remains defined separately in RFC-0002.

---

# 1. Introduction

Distributed systems frequently encounter duplicated execution caused by:

- delayed delivery
- duplicated transmission
- network reordering
- retry mechanisms
- malicious replay attempts
- stale infrastructure

Without replay protection, historical execution may incorrectly influence current runtime state.

Replay Protection prevents this class of failure.

---

# 2. Terminology

The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** are interpreted as described in RFC 2119.

---

# 3. Objective

Replay Protection determines whether observable execution is sufficiently fresh to participate in runtime decisions.

Previously accepted execution MUST NOT automatically become valid again.

---

# 4. Replay Definition

A replay occurs when previously accepted execution is presented again as if it were new execution.

Replay MAY originate from:

- delayed transmission
- duplicated packets
- duplicated runtime actions
- infrastructure recovery
- malicious injection

Replay Protection evaluates execution freshness.

---

# 5. Observable Principles

Replay Protection SHOULD provide:

- deterministic evaluation
- reproducible behavior
- implementation independence
- engineering observability

Protected implementation remains confidential.

---

# 6. Acceptance Rules

Fresh execution MAY be accepted.

Historical execution MUST NOT be accepted solely because it was previously valid.

Replay decisions MUST remain deterministic for equivalent observable conditions.

---

# 7. Relationship to Authority

Replay Protection and Authority validation are independent architectural mechanisms.

Replay determines execution freshness.

Authority determines execution ownership.

Successful replay validation does not imply authority validity.

Successful authority validation does not imply replay validity.

---

# 8. Runtime Behavior

When replay is detected, the runtime SHOULD reject the replayed execution.

Replay rejection SHOULD preserve current runtime correctness.

Observable rejection SHOULD be reproducible.

---

# 9. Engineering Validation

Engineering teams SHOULD validate:

- replay rejection
- deterministic behavior
- runtime stability
- authority independence
- reproducible verdicts

Implementation disclosure is not required.

---

# 10. Engineering Evidence

Replay validation MAY produce observable evidence including:

- replay verdict
- validation report
- runtime events
- engineering summary

Evidence supports independent technical assessment.

---

# 11. Engineering Invariants

Replay Protection MUST preserve:

- deterministic execution
- observable consistency
- runtime correctness
- engineering reproducibility

Replay MUST NOT modify accepted historical runtime behavior.

---

# 12. Security Considerations

Replay Protection improves resilience against:

- duplicated execution
- delayed execution
- stale runtime activity
- malicious replay attempts

Replay Protection complements, but does not replace, Authority validation.

---

# 13. Non-Goals

This RFC does not define:

- replay window implementation
- internal identifiers
- packet encoding
- cryptographic algorithms
- synchronization mechanisms

These remain proprietary implementation details.

---

# 14. Related RFCs

RFC-0002 — Authority Epochs

RFC-0004 — Runtime State Machine

RFC-0005 — Evidence Model

RFC-0007 — Failure Recovery

RFC-0009 — Security Boundary

---

# 15. Summary

Replay Protection ensures that observable execution represents current runtime activity rather than historical execution.

Deterministic replay rejection contributes to runtime integrity without exposing implementation details.

---

## Normative Requirements

- Historical execution **MUST NOT** be accepted as new execution.
- Replay decisions **SHOULD** be deterministic.
- Replay validation **MUST** remain independent from Authority validation.
- Replay rejection **MUST NOT** violate runtime invariants.
- Replay behavior **SHOULD** be reproducible.

---

## Design Principle

> Fresh execution moves forward.

> Historical execution remains history.

> Deterministic replay rejection preserves runtime integrity.