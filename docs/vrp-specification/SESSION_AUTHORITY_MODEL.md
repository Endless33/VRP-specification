# Session Authority Model

Document version: Public v1

Status: Public

---

# Purpose

This document defines the authority model used by VRP.

Authority determines who is allowed to mutate the canonical session.

Authority is independent from transport.

Authority is independent from routing.

Authority is independent from network location.

---

# Core Principle

Authority belongs to the session.

It never belongs to the transport.

Changing transport does not create new authority.

Changing IP does not create new authority.

Changing relay does not create new authority.

---

# Authority Components

Canonical authority consists of:

- SessionID
- LifetimeEpoch
- Incarnation
- Authority Generation
- Lease Epoch

Each component has a different responsibility.

None of them may substitute another.

---

# Lifetime Identity

Lifetime identity determines
which canonical lifetime currently exists.

Canonical lifetime identity:

(LifetimeEpoch, Incarnation)

---

# Authority Generation

Authority Generation represents
ownership evolution inside one canonical lifetime.

Examples:

Owner A

↓

Generation 1

↓

Migration

↓

Generation 2

↓

Transfer

↓

Generation 3

Generation never identifies lifecycle.

---

# Lease Epoch

Lease Epoch represents
distributed ownership synchronization.

Lease Epoch is not lifecycle identity.

Lease Epoch is not authority generation.

Lease Epoch exists for ownership coordination.

---

# Validation Order

Incoming Event

↓

Trusted Producer

↓

Lifetime Validation

↓

Authority Validation

↓

Lease Validation

↓

State Validation

↓

Canonical Mutation

---

# Authority Mutation

Authority may change only through
authorized canonical operations.

Examples:

- migration
- ownership transfer
- lease renewal

Authority never changes because of:

- packet arrival
- replay
- stale event
- delayed transport
- duplicated message

---

# Rejected Operations

Rejected operations never mutate:

- authority
- lease
- lifecycle
- history
- event log
- canonical state

---

# Security Guarantees

The authority model guarantees:

✓ transport independence

✓ deterministic ownership

✓ stale authority rejection

✓ replay rejection

✓ canonical ownership

✓ fail-closed validation

---

# Design Principle

Authority is validated.

Authority is never assumed.

Authority is never inferred from transport.

Authority exists only inside the canonical session lifecycle.