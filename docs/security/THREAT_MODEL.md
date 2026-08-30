# Threat Model

## Status

Public Security Documentation

Version: 1.0

---

# Purpose

This document defines the public threat model for the Veil Routing Protocol (VRP).

Its purpose is to describe the categories of failures and adversarial conditions considered during engineering validation.

The document intentionally describes observable security behavior.

Implementation-specific defenses remain part of the protected runtime.

---

# Security Philosophy

The VRP Runtime assumes that failures are inevitable.

The objective is not to eliminate every possible failure.

The objective is to preserve architectural correctness despite failure.

Security is therefore defined as preservation of runtime invariants under hostile conditions.

---

# Protected Assets

The runtime protects the following observable properties.

- Logical Session identity
- Canonical Authority
- Runtime integrity
- Recovery correctness
- Replay resistance
- Deterministic execution
- Engineering evidence

---

# Adversary Model

The public threat model assumes an adversary capable of:

- delaying communication
- duplicating communication
- reordering communication
- interrupting communication
- replaying historical execution
- restarting infrastructure
- attempting stale authority recovery
- creating transport instability
- introducing concurrent execution

The public model does not assume compromise of protected runtime implementation.

---

# Threat Categories

## Replay

Objective:

Reuse historical execution.

Expected runtime behavior:

Reject.

Observable invariant:

Historical execution never becomes fresh execution.

---

## Stale Authority

Objective:

Restore historical authority after canonical authority has progressed.

Expected runtime behavior:

Reject.

Observable invariant:

Authority progression remains monotonic.

---

## Duplicate Execution

Objective:

Cause the same logical operation to execute multiple times.

Expected runtime behavior:

Reject duplicate execution whenever it violates deterministic runtime behavior.

Observable invariant:

One observable execution.

---

## Invalid State Transition

Objective:

Move runtime into an impossible state.

Expected runtime behavior:

Reject transition.

Observable invariant:

Runtime State Machine remains valid.

---

## Transport Instability

Objective:

Cause unnecessary interruption through transport changes.

Expected runtime behavior:

Maintain Logical Session whenever architectural correctness allows.

Observable invariant:

Session continuity remains independent from transport.

---

## Infrastructure Restart

Objective:

Cause ownership ambiguity after runtime restart.

Expected runtime behavior:

Historical runtime instances never automatically regain canonical authority.

Observable invariant:

Authority evolution remains deterministic.

---

## Concurrent Runtime Activity

Objective:

Create inconsistent execution through concurrent operations.

Expected runtime behavior:

Deterministic outcome.

Observable invariant:

Equivalent observable conditions produce equivalent engineering conclusions.

---

# Out of Scope

This public threat model intentionally excludes:

- implementation vulnerabilities
- source code review
- compiler behavior
- operating system hardening
- hardware attacks
- side-channel analysis
- cryptographic implementation

These subjects belong to the protected runtime.

---

# Engineering Validation

Typical validation includes:

- replay attempts
- stale authority injection
- authority takeover
- transport migration
- recovery
- concurrent execution
- deterministic replay
- engineering evidence verification

Observable behavior is evaluated.

Protected implementation is not.

---

# Security Objectives

The runtime should preserve:

- continuity
- authority
- determinism
- evidence integrity
- reproducibility

Security exists to preserve these architectural properties.

---

# Related Documents

RFC-0002 — Authority Epochs

RFC-0005 — Evidence Model

RFC-0006 — Replay Protection

RFC-0007 — Failure Recovery

RFC-0009 — Security Boundary

RFC-0012 — Threat Model

---

# Summary

The public threat model defines the observable engineering assumptions used to evaluate VRP.

Engineering validation focuses on runtime behavior under failure and adversarial conditions.

Protected implementation remains confidential.

---

## Design Principle

Failures are expected.

Attacks are expected.

Architectural invariants are expected to survive both.