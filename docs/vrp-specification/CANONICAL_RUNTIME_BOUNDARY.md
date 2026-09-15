# Canonical Runtime Boundary

Document version: Public v1

Status: Public

---

# Purpose

This document defines the runtime trust boundary inside VRP.

The runtime boundary separates components that are allowed to mutate
canonical state from components that are not.

The boundary exists to guarantee deterministic and fail-closed execution.

---

# Fundamental Rule

Only trusted runtime components may produce canonical state changes.

Everything else is treated as untrusted input.

Trust is never inferred.

Trust is always explicit.

---

# Trusted Runtime

Current trusted runtime includes:

- SessionManager
- SessionRuntime
- ControlPlane
- AuthorityManager
- LeaseManager

These components participate in canonical state transitions.

---

# Untrusted Input

Examples:

- external packets

- delayed packets

- duplicated packets

- replayed packets

- stale events

- malformed events

- user input

- network transport

Receiving an object never grants authority.

---

# Runtime Boundary

External Input

↓

Validation

↓

Trusted Runtime

↓

Canonical State

Nothing bypasses validation.

---

# Canonical Mutation

Canonical state may change only after:

- bootstrap validation

- lifecycle validation

- authority validation

- lease validation

- state-machine validation

---

# Forbidden Operations

The following must never mutate canonical runtime directly:

- packet receive

- transport reconnect

- transport migration

- delayed delivery

- replay

- stale authority

- stale lifecycle

- malformed event

---

# Transport Independence

Transport is below the runtime boundary.

Replacing transport does not replace:

- authority

- lifecycle

- session identity

- canonical history

---

# Failure Model

If validation fails:

processing stops immediately.

No canonical mutation occurs.

The runtime remains unchanged.

---

# Canonical Components

Protected by the runtime boundary:

- session state

- transition history

- authority

- lease

- event log

- lifecycle identity

---

# Security Guarantees

The runtime boundary guarantees:

✓ deterministic execution

✓ fail-closed behavior

✓ transport independence

✓ replay rejection

✓ stale lifecycle rejection

✓ stale authority rejection

✓ immutable canonical history

✓ restart-safe validation

---

# Design Principle

Validation defines the runtime boundary.

Nothing becomes canonical before crossing that boundary successfully.