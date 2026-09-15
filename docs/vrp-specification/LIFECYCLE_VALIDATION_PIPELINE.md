# Lifecycle Validation Pipeline

Document version: Public v1

Status: Public

---

# Purpose

This document defines the complete lifecycle validation pipeline used by
VRP before any canonical session mutation is allowed.

The pipeline exists to ensure that every accepted event belongs to the
currently active canonical session lifetime.

Validation is deterministic.

Validation is fail-closed.

---

# Canonical Processing Pipeline

Trusted Producer

↓

Session Bootstrap (if required)

↓

Lifetime Binding

↓

Canonical Validation

↓

Event Log

↓

State Machine

↓

Transition History

↓

Canonical Runtime State

---

# Stage 1 — Trusted Producer

The event must originate from a trusted producer.

Current trusted producers include:

- SessionRuntime
- ControlPlane

Events from generic external sources are never implicitly trusted.

---

# Stage 2 — Bootstrap Validation

Session creation is treated as a privileged operation.

Bootstrap is allowed only while creating a new canonical session.

No existing lifecycle exists before bootstrap.

---

# Stage 3 — Lifetime Binding

The trusted producer binds the event to the currently active lifecycle.

Canonical lifecycle identity:

(LifetimeEpoch, Incarnation)

This identity becomes part of the event.

---

# Stage 4 — Lifecycle Validation

SessionManager validates:

- Session exists
- LifetimeEpoch matches
- Incarnation matches

Failure causes immediate rejection.

---

# Stage 5 — State Validation

After lifecycle validation succeeds,
the event is validated against the canonical state machine.

Examples:

- valid transition
- duplicate transition
- invalid transition
- stale transition

Only legal transitions continue.

---

# Stage 6 — Event Log

Accepted events enter the canonical event log.

Rejected events never appear in canonical history.

---

# Stage 7 — State Transition

Only accepted events may mutate:

- canonical session state
- transition history
- runtime state

---

# Failure Model

Failure at any stage immediately stops processing.

Rejected events:

- do not modify state
- do not modify history
- do not enter event log
- do not create sessions
- do not create authority

---

# Security Properties

The validation pipeline guarantees:

✓ deterministic execution

✓ canonical history protection

✓ stale lifecycle rejection

✓ replay rejection

✓ restart-safe validation

✓ fail-closed processing

---

# Design Principle

Validation always precedes mutation.

Canonical state is the result of successful validation,
never the source of trust.