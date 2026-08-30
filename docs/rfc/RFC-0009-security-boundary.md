# RFC-0009 — Security Boundary

**Document Status:** Public Specification

**RFC Number:** RFC-0009

**Version:** 1.0

**Category:** Security Architecture

---

# Abstract

This document defines the Security Boundary of the Veil Routing Protocol (VRP).

The Security Boundary separates observable runtime behavior from protected implementation.

Its objective is to enable independent engineering validation while preserving proprietary technology, implementation integrity and intellectual property.

The Security Boundary is considered an architectural component of the VRP Runtime.

---

# 1. Introduction

Engineering transparency does not require implementation transparency.

The VRP architecture intentionally separates:

- observable behavior
- implementation behavior

This separation allows organizations to evaluate runtime correctness without requiring disclosure of protected engineering assets.

---

# 2. Terminology

The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** are interpreted as described in RFC 2119.

---

# 3. Security Boundary Objectives

The Security Boundary exists to simultaneously achieve two goals:

- enable independent engineering validation
- protect proprietary implementation

These objectives are complementary.

Neither objective should weaken the other.

---

# 4. Public Architectural Boundary

The following architectural concepts are intentionally public.

Examples include:

- Logical Session
- Authority Epoch
- Transport Abstraction
- Runtime State Machine
- Replay Protection
- Failure Recovery
- Multipath Selection
- Evidence Model

These concepts define observable runtime behavior.

---

# 5. Protected Implementation Boundary

The following implementation areas remain outside the public specification.

Examples include:

- runtime source code
- internal algorithms
- scheduling implementation
- transport scoring
- synchronization primitives
- memory layout
- protocol encoding
- binary formats
- optimization strategies
- implementation heuristics
- deployment internals

These components constitute protected implementation.

---

# 6. Observable Behavior

Engineering evaluation SHOULD focus upon observable runtime behavior including:

- continuity
- authority evolution
- deterministic execution
- transport evolution
- recovery
- replay rejection
- evidence generation

Observable behavior is intentionally reviewable.

---

# 7. Protected Behavior

The following implementation characteristics are intentionally excluded from evaluation:

- algorithm implementation
- optimization decisions
- internal scheduling
- runtime coordination
- cryptographic implementation
- internal concurrency mechanisms

These remain confidential.

---

# 8. Engineering Validation

Independent reviewers SHOULD be capable of validating:

- runtime correctness
- deterministic behavior
- engineering invariants
- evidence reproducibility
- recovery correctness

without implementation disclosure.

---

# 9. Reverse Engineering

The public architecture is not intended to facilitate:

- implementation reconstruction
- algorithm extraction
- protocol cloning
- binary reverse engineering
- proprietary optimization analysis

Observable runtime behavior MAY be evaluated.

Protected implementation remains outside the architectural boundary.

---

# 10. Intellectual Property

Publication of architectural specifications does not transfer ownership of:

- implementation
- algorithms
- source code
- runtime design
- optimization techniques
- engineering know-how

Ownership remains unchanged.

---

# 11. Engineering Discussions

Technical discussion is encouraged regarding:

- architecture
- runtime behavior
- engineering validation
- deployment
- operational characteristics

Questions whose primary objective is implementation disclosure MAY remain unanswered.

---

# 12. Security Objectives

The Security Boundary strengthens:

- engineering transparency
- reproducibility
- implementation confidentiality
- independent evaluation
- intellectual property protection

These objectives coexist within the architecture.

---

# 13. Engineering Invariants

The Security Boundary MUST preserve:

- observable engineering validation
- protected implementation
- deterministic runtime behavior
- reproducible evidence
- architectural consistency

Violation of these principles weakens the architectural model.

---

# 14. Non-Goals

This RFC does not define:

- legal agreements
- licensing terms
- export restrictions
- commercial policy
- patent strategy

Those subjects are addressed separately.

---

# 15. Related RFCs

RFC-0001 — Logical Session Identity

RFC-0005 — Evidence Model

RFC-0006 — Replay Protection

RFC-0008 — Multipath Selection

RFC-0012 — Threat Model

---

# 16. Summary

The Security Boundary separates architecture from implementation.

Observable behavior remains available for engineering evaluation.

Protected implementation remains confidential.

This separation enables reproducible validation while preserving long-term intellectual property.

---

## Normative Requirements

- Observable runtime behavior **MUST** remain independently verifiable.
- Protected implementation **MUST NOT** be required for engineering validation.
- Architectural documentation **MUST NOT** disclose proprietary implementation.
- Engineering evidence **SHOULD** support independent technical assessment.
- Security Boundary principles **MUST** remain consistent across all public specifications.

---

## Design Principle

> Architecture is public.

> Implementation is protected.

> Engineering trust comes from observable behavior, not implementation disclosure.