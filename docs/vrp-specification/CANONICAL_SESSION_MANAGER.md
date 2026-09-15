# Canonical Session Manager

Document version: Public v1

Status: Public

---

# Purpose

This document defines the security responsibilities of the VRP
SessionManager.

SessionManager is the only component allowed to mutate canonical
session state.

It is the validation boundary of the runtime.

---

# Responsibilities

SessionManager is responsible for:

- allocating canonical session lifetime
- validating lifecycle identity
- rejecting stale events
- protecting canonical history
- protecting canonical event log
- protecting canonical state machine

---

# Canonical Pipeline

Trusted producer

↓

Bootstrap (if required)

↓

Lifetime binding

↓

SessionManager validation

↓

Canonical event log

↓

State transition

↓

Canonical history

---

# Session Creation

Session creation is a privileged operation.

It is NOT ordinary event delivery.

Only trusted bootstrap authority may create a canonical session.

---

# Event Validation

Every event is validated before it reaches canonical state.

Validation includes:

- Session existence
- LifetimeEpoch
- Incarnation
- Transition legality

Failure at any stage rejects the event.

---

# Fail Closed

Rejected events never:

- modify canonical state
- modify canonical history
- enter canonical event log
- create new authority
- create new session

---

# Canonical History

History is immutable except for accepted canonical transitions.

Rejected events do not exist from the perspective of canonical history.

---

# Security Guarantees

The SessionManager guarantees:

✓ deterministic lifecycle validation

✓ canonical state protection

✓ canonical history protection

✓ replay rejection

✓ stale lifecycle rejection

✓ restart-safe validation

✓ fail-closed behavior

---

# Design Principle

The SessionManager is not merely a storage object.

It is the canonical validation authority for the session lifecycle.