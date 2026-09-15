# Canonical History Model

Document version: Public v1

Status: Public

---

# Purpose

This document defines the canonical history model used by VRP.

Canonical history is the immutable record of every accepted canonical
state transition.

History is security-critical.

History is deterministic.

History is never reconstructed from observations.

---

# Canonical Rule

History is created only by accepted canonical events.

Rejected events never become history.

Delayed events never become history.

Replay never becomes history.

---

# Canonical Flow

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

Canonical Event Log

↓

Canonical Transition History

---

# History Properties

Canonical history is:

- append-only

- deterministic

- immutable

- ordered

- validated

No historical transition may be rewritten.

---

# Accepted Transition

Only accepted transitions are recorded.

Example:

SESSION_ACTIVE

↓

PATH_LOST

↓

DETACH_TRANSPORT

↓

MIGRATION

↓

SESSION_RECOVERED

↓

SESSION_ACTIVE

---

# Rejected Transition

Rejected transitions never appear.

Examples:

- replay

- duplicate transition

- stale lifetime

- stale authority

- stale lease

- invalid transition

- unauthorized bootstrap

---

# Ordering

History preserves canonical ordering.

Order never depends on:

- network latency

- packet arrival order

- transport

- scheduler timing

Canonical validation determines ordering.

---

# Snapshot Semantics

History may be exported.

History may be copied.

History may be inspected.

History snapshots never modify canonical runtime.

---

# Restart

Restart never extends an old canonical history.

Restart creates a new canonical lifetime.

Historical records remain immutable evidence.

---

# Relationship to Event Log

Canonical Event Log

contains accepted events.

Canonical History

contains accepted transitions.

Both are immutable.

Neither contains rejected events.

---

# Security Guarantees

Canonical history guarantees:

✓ append-only behavior

✓ deterministic ordering

✓ immutable transitions

✓ replay resistance

✓ stale lifecycle rejection

✓ restart-safe execution

✓ fail-closed processing

---

# Design Principle

History is evidence of successful validation.

History never becomes a validation mechanism itself.

Validation creates history.

History never creates authority.