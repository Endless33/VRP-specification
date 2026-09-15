# Session Identity Model

Document version: Public v1

Status: Public

---

# Purpose

This document defines canonical session identity inside VRP.

Session identity is intentionally separated from transport identity.

Changing network path must never create a new session.

Changing transport must never create a new session.

Changing relay must never create a new session.

---

# Fundamental Principle

Session ≠ Transport

A transport may disappear.

A transport may recover.

A transport may migrate.

The session remains the same.

---

# Canonical Session Identity

A canonical session consists of multiple independent identities.

SessionID

↓

Lifetime Identity

↓

Authority Identity

↓

Lease Identity

↓

Runtime State

Each layer has its own responsibility.

---

# SessionID

SessionID identifies the logical communication session.

Properties:

- stable
- transport independent
- globally meaningful
- survives transport migration

SessionID alone is NOT sufficient to identify a canonical lifetime.

---

# Lifetime Identity

Canonical lifetime is

(LifetimeEpoch, Incarnation)

Properties:

- restart safe
- deterministic
- replay resistant
- ABA resistant

---

# Authority Identity

Authority determines ownership.

Authority identity includes:

- current owner
- authority generation

Authority is independent from transport.

Authority is independent from lifecycle.

---

# Lease Identity

Lease identity represents distributed ownership coordination.

Lease epoch is independent from:

- SessionID
- LifetimeEpoch
- Incarnation
- Authority Generation

---

# Runtime Identity

Runtime identity represents current canonical execution.

Examples:

- current state
- transition history
- event log
- active authority

---

# Identity Hierarchy

SessionID

↓

Lifetime

↓

Authority

↓

Lease

↓

Runtime

Each layer validates a different security property.

---

# Invalid Assumptions

The following are incorrect:

SessionID == Lifecycle

Transport == Session

Authority == Lifecycle

Lease == Authority

Generation == Incarnation

Epoch == Session

These concepts are intentionally separated.

---

# Migration

Transport migration changes only transport.

Session identity remains unchanged.

Authority remains unchanged unless explicitly transferred.

Lifetime remains unchanged.

Canonical history remains unchanged.

---

# Restart

Restart creates a new lifetime namespace.

SessionID may remain identical.

LifetimeEpoch changes.

Incarnation restarts inside the new namespace.

Old lifecycle becomes permanently stale.

---

# Security Guarantees

The identity model guarantees:

✓ transport independence

✓ deterministic session identity

✓ restart-safe validation

✓ replay resistance

✓ stale lifecycle rejection

✓ canonical ownership separation

✓ fail-closed processing

---

# Design Principle

Identity is layered.

Each layer answers one question:

SessionID

"What communication session is this?"

Lifetime

"Which canonical lifetime is active?"

Authority

"Who currently owns this session?"

Lease

"Who currently holds distributed ownership?"

Runtime

"What is the current canonical execution state?"