# Canonical Event Log

Document version: Public v1

Status: Public

---

# Purpose

This document defines the security model of the canonical VRP event log.

The event log is the immutable record of accepted canonical lifecycle events.

It is not a transport log.

It is not a debug log.

It is not an audit reconstruction.

It represents only canonical runtime history.

---

# Canonical Rule

Only validated events may enter the canonical event log.

Every logged event has already passed:

- creation authority
- lifetime validation
- state validation

---

# Event Flow

Trusted Producer

↓

Lifetime Binding

↓

SessionManager Validation

↓

Canonical Event Log

↓

State Transition

↓

Canonical History

---

# Rejected Events

Rejected events never enter the canonical log.

Examples include:

- stale lifetime
- stale incarnation
- stale epoch
- replay
- invalid transition
- unauthorized bootstrap

These events are rejected before canonical mutation.

---

# Log Invariants

The canonical event log must satisfy:

- deterministic ordering
- append-only behavior
- immutable accepted history
- no rejected events
- no replay entries
- no duplicate canonical transitions

---

# Ordering

Canonical ordering is deterministic.

The same accepted sequence always produces the same canonical history.

---

# Event Identity

Each canonical event belongs to exactly one lifecycle:

(LifetimeEpoch, Incarnation)

Events outside the current lifecycle are rejected.

---

# Security Guarantees

The canonical event log guarantees:

✓ deterministic history

✓ immutable accepted events

✓ no stale lifecycle mutation

✓ no replay mutation

✓ no unauthorized bootstrap

✓ fail-closed behavior

---

# Relationship to Audit Logs

Canonical Event Log

contains only accepted canonical events.

Audit logs may additionally contain:

- rejected events
- diagnostics
- validation failures
- security evidence

The canonical event log intentionally excludes these.

---

# Design Principle

The canonical event log is the authoritative history of the VRP session lifecycle.

Nothing becomes canonical unless SessionManager accepts it.