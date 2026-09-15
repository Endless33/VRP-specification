# Canonical Event Acceptance Model

Document version: Public v1

Status: Public

---

# Purpose

This document defines the exact conditions under which an event becomes
part of the canonical VRP runtime.

Every event follows the same deterministic acceptance pipeline.

Acceptance is binary.

Either the event becomes canonical,
or it is rejected completely.

There is no intermediate state.

---

# Canonical Acceptance Pipeline

Incoming Event

↓

Trusted Producer

↓

Bootstrap Validation

↓

Lifetime Validation

↓

Authority Validation

↓

Lease Validation

↓

State Validation

↓

Canonical Event Log

↓

Canonical Transition History

↓

Canonical Runtime State

---

# Acceptance Rule

An event is accepted only if every validation stage succeeds.

Failure of any stage immediately rejects the event.

---

# Canonical Event

A canonical event satisfies:

✓ trusted origin

✓ valid lifecycle

✓ valid authority

✓ valid lease

✓ valid state transition

Only then may it mutate runtime.

---

# Rejected Event

Rejected events never become canonical.

Rejected events never mutate:

- runtime state

- transition history

- authority

- lease

- event log

- lifecycle

---

# Validation Stages

## Bootstrap

Determines whether creation of a new canonical session is allowed.

---

## Lifetime

Validates

(LifetimeEpoch, Incarnation)

against the currently active canonical session.

---

## Authority

Validates current canonical authority ownership.

---

## Lease

Validates current lease ownership and epoch.

---

## State

Validates the state-machine transition.

---

# Ordering

Validation order never changes.

Bootstrap

↓

Lifetime

↓

Authority

↓

Lease

↓

State

↓

Mutation

Changing this order changes the security model.

---

# Fail Closed

Whenever validation fails:

processing stops immediately.

No rollback is required because no canonical mutation has occurred.

---

# Determinism

The same canonical input always produces the same canonical result.

Accepted inputs remain accepted.

Rejected inputs remain rejected.

---

# Security Properties

The acceptance model guarantees:

✓ deterministic processing

✓ replay rejection

✓ stale lifecycle rejection

✓ stale authority rejection

✓ duplicate rejection

✓ restart-safe validation

✓ immutable canonical history

✓ fail-closed execution

---

# Relationship to Evidence

Canonical acceptance determines runtime behavior.

Evidence records why an event was accepted or rejected.

Evidence never changes canonical runtime state.

---

# Design Principle

Validation always precedes mutation.

Acceptance creates canonical history.

Rejection leaves canonical history unchanged.