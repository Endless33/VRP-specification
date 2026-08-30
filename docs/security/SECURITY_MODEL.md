# Security Model

## Status

Public Security Documentation

Version: 1.0

---

# Purpose

This document defines the public security architecture of the Veil Routing Protocol (VRP).

The Security Model explains how the runtime preserves observable correctness under failure and adversarial conditions while protecting proprietary implementation.

This document intentionally describes security at the architectural level.

Implementation mechanisms remain part of the protected runtime.

---

# Security Philosophy

The objective of VRP is not to eliminate every possible failure.

The objective is to ensure that failures cannot violate architectural correctness.

Security therefore means preserving deterministic runtime behavior despite unreliable infrastructure.

Correctness always has priority over availability.

---

# Architectural Objectives

The runtime is designed to preserve:

- Logical Session identity
- Canonical Authority
- Deterministic Runtime Decisions
- Replay Resistance
- Recovery Correctness
- Transport Independence
- Evidence Integrity
- Engineering Reproducibility

Every security decision exists to preserve one or more of these objectives.

---

# Security Architecture

```
Application
      │
      ▼
Runtime API
      │
      ▼
Protected Runtime
      │
      ├───────────────┐
      │               │
      ▼               ▼
Authority         Recovery
      │               │
      └──────┬────────┘
             ▼
Replay Protection
             │
             ▼
Transport Selection
             │
             ▼
Transport Infrastructure
             │
             ▼
External Network
```

The Protected Runtime is the security decision point.

---

# Core Security Components

The public security architecture consists of:

- Logical Session
- Authority Epochs
- Runtime State Machine
- Replay Protection
- Failure Recovery
- Multipath Selection
- Evidence Model
- Security Boundary

Each component protects a different architectural invariant.

---

# Trust Model

The runtime assumes:

- networks may fail
- transports may change
- packets may be delayed
- replay may occur
- infrastructure may restart
- concurrent execution may occur

None of these conditions should invalidate runtime correctness.

---

# Runtime Decisions

Observable runtime decisions include:

- authority acceptance
- authority rejection
- replay rejection
- transport evolution
- recovery
- termination

Decision algorithms remain implementation-specific.

Observable decisions remain reproducible.

---

# Security Invariants

The runtime preserves the following architectural invariants.

## Logical Session

Exactly one observable Logical Session.

---

## Authority

Exactly one canonical authority.

---

## Replay

Historical execution never becomes fresh execution.

---

## Recovery

Recovery never violates runtime correctness.

---

## Runtime State

Invalid state transitions are rejected.

---

## Evidence

Engineering evidence reflects observable runtime behavior.

---

# Observable Validation

Engineering validation may include:

- replay attack
- stale authority attack
- duplicate execution
- transport migration
- recovery validation
- authority takeover
- concurrent execution
- evidence verification

Validation evaluates observable behavior.

---

# Security Events

Observable runtime events may include:

- authority established
- authority rejected
- replay rejected
- recovery started
- recovery completed
- transport changed
- runtime terminated
- evidence generated

Event generation remains implementation-independent.

---

# Failure Philosophy

Failures are expected.

Undefined behavior is not.

Whenever correctness cannot be preserved, the runtime should terminate safely rather than continue with inconsistent execution.

Fail-safe behavior is preferred over incorrect behavior.

---

# Protected Implementation

The public Security Model intentionally excludes:

- implementation algorithms
- packet encoding
- synchronization mechanisms
- scheduling logic
- optimization heuristics
- cryptographic implementation
- internal memory structures

These remain protected engineering assets.

---

# Engineering Principles

Security validation should answer:

- Was the Logical Session preserved?
- Was authority preserved?
- Was replay rejected?
- Was recovery deterministic?
- Was engineering evidence generated?
- Were architectural invariants maintained?

Observable behavior answers these questions.

---

# Related Documents

THREAT_MODEL.md

ATTACK_TREE.md

TRUST_BOUNDARY.md

OPERATOR_TRUST_MODEL.md

RUNTIME_BOUNDARIES.md

RFC-0002 — Authority Epochs

RFC-0006 — Replay Protection

RFC-0009 — Security Boundary

RFC-0012 — Threat Model

---

# Summary

The VRP Security Model defines the observable architectural principles that preserve deterministic runtime behavior.

Security is achieved by protecting architectural invariants rather than depending upon trusted infrastructure.

Implementation remains confidential.

Engineering validation remains observable.

---

## Security Principles

- Preserve the Logical Session.
- Preserve canonical authority.
- Reject historical execution.
- Recover deterministically.
- Generate reproducible evidence.
- Protect implementation.
- Preserve architectural correctness.

---

## Design Principle

Security is not defined by secrecy.

Security is defined by deterministic correctness under adversarial conditions.