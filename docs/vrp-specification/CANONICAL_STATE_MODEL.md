# Canonical State Model

Document version: Public v1

Status: Public

---

# Purpose

This document defines the canonical state model used by VRP.

Canonical state represents the only authoritative runtime state of a
session.

No secondary copy may become authoritative.

No transport state may become canonical.

No replay may become canonical.

---

# Canonical Rule

At every moment there is exactly one canonical state.

Multiple observations may exist.

Multiple transports may exist.

Multiple candidates may exist.

Only one canonical state exists.

---

# Canonical Components

Canonical state consists of:

- SessionID

- Lifetime Identity

- Authority

- Lease

- State Machine

- Event Log

- Transition History

These components evolve together.

---

# State Ownership

Canonical state is owned only by SessionManager.

Other components observe it.

Other components validate against it.

Other components never replace it.

---

# State Evolution

Canonical state changes only after:

Trusted Producer

↓

Lifecycle Validation

↓

Authority Validation

↓

Lease Validation

↓

State Validation

↓

Canonical Mutation

---

# Immutable Properties

The following properties are never rewritten
without successful canonical validation:

- SessionID

- LifetimeEpoch

- Incarnation

- Authority

- Lease

- Transition History

- Event Log

---

# Non-Canonical Data

The following data is never considered canonical:

- transport metrics

- packet timing

- routing decisions

- latency measurements

- retransmission statistics

- temporary health estimates

These values may influence decisions.

They never become authority.

---

# Canonical Consistency

The canonical state always satisfies:

- valid lifecycle

- valid authority

- valid lease

- valid transition history

- deterministic state

If one property fails,

the mutation is rejected.

---

# Snapshot Semantics

Read operations return snapshots.

Snapshots never mutate canonical state.

External modification of snapshots has no effect
on internal runtime.

---

# Replay Protection

Replay cannot recreate canonical state.

Old events remain historical.

They never become current state again.

---

# Restart Behavior

Restart creates a new runtime namespace.

Canonical state starts from a newly allocated
lifecycle identity.

Old canonical state remains historical evidence.

It never becomes active again.

---

# Security Guarantees

Canonical state guarantees:

✓ deterministic execution

✓ immutable history

✓ replay resistance

✓ stale lifecycle rejection

✓ restart-safe validation

✓ fail-closed behavior

✓ single authoritative runtime

---

# Design Principle

There is always exactly one canonical state.

Everything else is observation.
Everything else is evidence.
Everything else is input.