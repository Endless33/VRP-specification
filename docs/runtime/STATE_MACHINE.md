# Runtime State Machine

## Status

Public Runtime Documentation

Version: 1.0

---

# Purpose

This document defines the observable Runtime State Machine of the Veil Routing Protocol (VRP).

The Runtime State Machine specifies the valid lifecycle of a Logical Session and the permitted transitions between observable runtime states.

Internal implementation mechanisms remain part of the protected runtime.

---

# Design Philosophy

Every runtime state has a defined purpose.

Every state transition has a deterministic outcome.

Undefined runtime behavior is considered an architectural failure.

The Runtime State Machine exists to eliminate ambiguity.

---

# Architectural Principles

The Runtime State Machine is governed by the following principles.

- Every session has one observable state.
- State transitions are deterministic.
- Invalid transitions are rejected.
- Historical states never become current states.
- Recovery preserves correctness.
- Observable behavior remains reproducible.

---

# Runtime Lifecycle

```
                ┌───────────────┐
                │  Initialized  │
                └───────┬───────┘
                        │
                        ▼
                ┌───────────────┐
                │    Active     │
                └───────┬───────┘
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
  Degraded         Recovering     Terminating
        │               │               │
        └───────┬───────┘               │
                ▼                       ▼
             Active               Terminated
```

Only observable transitions shown above are considered valid.

---

# State Definitions

## Initialized

The runtime has been created.

Resources are allocated.

No application execution has begun.

---

## Active

The Logical Session is operational.

Authority is canonical.

Observable execution proceeds normally.

Transport evolution may occur.

---

## Degraded

One or more runtime conditions have degraded.

Examples include:

- transport instability
- increased latency
- temporary connectivity loss

Correctness remains preserved.

---

## Recovering

The runtime is attempting to restore normal execution.

Recovery operates under architectural invariants.

Recovery does not redefine session identity.

---

## Terminating

The runtime has determined that execution cannot safely continue.

Termination proceeds in a controlled and deterministic manner.

---

## Terminated

Execution has ended.

No further runtime activity occurs.

Historical state remains historical.

---

# Valid State Transitions

The runtime permits the following observable transitions.

```
Initialized
      │
      ▼
Active

Active
      │
      ├────────► Degraded
      │
      ├────────► Recovering
      │
      └────────► Terminating

Degraded
      │
      ├────────► Active
      │
      ├────────► Recovering
      │
      └────────► Terminating

Recovering
      │
      ├────────► Active
      │
      └────────► Terminating

Terminating
      │
      ▼
Terminated
```

---

# Invalid Transitions

The runtime rejects observable transitions including:

- Terminated → Active
- Terminated → Recovering
- Active → Initialized
- Recovering → Initialized
- Authority rollback through state transition
- Replay-induced state resurrection

Invalid transitions never become observable runtime behavior.

---

# Authority Relationship

Authority progression is independent of runtime state.

State evolution must preserve:

- canonical authority
- monotonic epoch progression
- deterministic ownership

State transitions never redefine authority history.

---

# Recovery Relationship

Recovery is a runtime activity.

Recovery does not create a new Logical Session.

Recovery attempts to preserve:

- session identity
- authority
- deterministic execution
- evidence integrity

---

# Transport Relationship

Transport changes may occur while remaining in the Active state.

Transport evolution alone does not imply:

- degradation
- recovery
- termination

Transport is an observable input.

It is not the Runtime State Machine.

---

# Observable Events

Typical observable events include:

- runtime initialized
- session activated
- transport changed
- runtime degraded
- recovery started
- recovery completed
- runtime terminated
- evidence generated

Event delivery remains implementation-independent.

---

# Engineering Validation

Validation should verify:

- valid transitions
- invalid transition rejection
- deterministic state evolution
- recovery correctness
- authority preservation
- replay resistance

Observable runtime behavior forms the basis of evaluation.

---

# Relationship to Other Documents

This document complements:

- INVARIANTS.md
- EVENT_FLOW.md
- AUTHORITY_TRANSITIONS.md
- FAILURE_HANDLING.md
- RECOVERY_RULES.md

It also supports:

- RFC-0004 — Runtime State Machine
- RFC-0007 — Failure Recovery

---

# Summary

The Runtime State Machine defines every observable execution state of the VRP Runtime.

Execution progresses through deterministic transitions.

Undefined behavior is never considered valid runtime behavior.

---

## Design Principles

- Every state has meaning.
- Every transition is deterministic.
- Invalid transitions are rejected.
- Recovery preserves correctness.
- Authority remains canonical.
- The Runtime State Machine is authoritative.