# Lifecycle Boundary Model

Document version: Public v1

Status: Public

---

# Purpose

This document defines the architectural boundaries that separate one
canonical session lifetime from another.

The boundary exists to ensure that no operation from an older lifetime
can ever mutate a newer canonical session.

---

# Canonical Lifetime

A canonical lifetime begins only after successful bootstrap.

It ends when the canonical session is destroyed.

Everything before bootstrap is non-canonical.

Everything after destruction belongs to history.

---

# Canonical Identity

Every lifetime is uniquely identified by:

(LifetimeEpoch, Incarnation)

Both values are required.

Neither value alone is sufficient.

---

# Lifecycle Boundary

Canonical Lifetime A

↓

Destroy Session

↓

Boundary

↓

Create Session

↓

Canonical Lifetime B

Nothing from Lifetime A is allowed to cross the boundary.

---

# Allowed Across Boundary

Allowed:

- evidence
- diagnostics
- immutable history
- audit information

These objects never modify canonical runtime state.

---

# Forbidden Across Boundary

Forbidden:

- stale events
- replay
- delayed migration
- delayed recovery
- stale authority
- stale ownership
- stale transition
- stale transport state

---

# Validation

Every event must satisfy:

Current LifetimeEpoch

AND

Current Incarnation

Failure of either component causes immediate rejection.

---

# Boundary Properties

The lifecycle boundary guarantees:

- no resurrection
- no rollback
- no stale ownership
- no stale migration
- no stale recovery
- no stale transition replay

---

# Restart Boundary

Restart creates a new lifecycle namespace.

Old

(LifetimeEpoch, Incarnation)

↓

Restart

↓

New

(LifetimeEpoch, Incarnation)

The previous namespace never becomes authoritative again.

---

# Same-Manager Boundary

Destroy

↓

Create

↓

Incarnation++

Old lifecycle events are permanently rejected.

---

# Event Processing

Incoming Event

↓

Lifecycle Boundary Validation

↓

Authority Validation

↓

State Validation

↓

Canonical Mutation

---

# Failure Model

If boundary validation fails:

- event rejected
- no canonical mutation
- no event log mutation
- no transition history mutation
- no authority mutation
- fail closed

---

# Security Guarantees

The lifecycle boundary guarantees:

✓ stale lifetime rejection

✓ deterministic validation

✓ restart-safe behavior

✓ same-manager ABA protection

✓ canonical history integrity

✓ fail-closed processing

---

# Design Principle

A lifecycle boundary is irreversible.

Once a canonical lifetime ends, it never becomes active again.