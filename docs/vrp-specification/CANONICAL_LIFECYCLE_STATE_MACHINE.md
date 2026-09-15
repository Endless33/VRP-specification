# Canonical Lifecycle State Machine

Document version: Public v1

Status: Public

---

# Purpose

This document defines the canonical lifecycle state machine used by VRP.

The state machine is the only authority that determines whether a session
transition is valid.

Every canonical transition must satisfy both:

- lifecycle validation
- state validation

---

# Design Philosophy

The lifecycle state machine is deterministic.

The same input sequence always produces the same canonical result.

Undefined behavior is not permitted.

---

# Canonical States

INITIAL

↓

SESSION_INIT

↓

SESSION_ESTABLISHED

↓

SESSION_ACTIVE

↓

DETACH_TRANSPORT

↓

MIGRATION

↓

SESSION_RECOVERED

↓

SESSION_ACTIVE

---

Additional terminal states:

SESSION_CLOSED

SESSION_FAILED

---

# Canonical Transition Rules

Only explicitly defined transitions are accepted.

Example:

INITIAL

↓

create_session

↓

SESSION_INIT

Any undefined transition is rejected.

---

# Validation Order

Incoming Event

↓

Lifetime Validation

↓

State Validation

↓

Transition Validation

↓

Canonical Mutation

---

# Invalid Transition

An event is rejected if:

- state does not permit the event
- transition does not exist
- lifecycle identity is stale
- replay is detected
- duplicate transition is detected

---

# Duplicate Protection

Duplicate canonical transitions are rejected.

Examples:

MigrationCompleted

↓

MigrationCompleted

Rejected.

RecoveryConfirmed

↓

RecoveryConfirmed

Rejected.

---

# Replay Protection

Replay does not depend only on transport.

Replay is evaluated against canonical lifecycle identity.

Old lifecycle events never become valid again.

---

# Restart Safety

After SessionManager restart:

Old

(LifetimeEpoch, Incarnation)

must never validate against

New

(LifetimeEpoch, Incarnation)

even if SessionID is identical.

---

# Canonical History

Accepted transitions become immutable history.

Rejected transitions never become history.

---

# Failure Model

If validation fails:

- no state mutation
- no history mutation
- no event-log mutation
- no authority mutation
- no lifecycle mutation

Processing stops immediately.

---

# Security Guarantees

The lifecycle state machine guarantees:

✓ deterministic execution

✓ canonical transition validation

✓ duplicate rejection

✓ replay rejection

✓ stale lifecycle rejection

✓ restart-safe validation

✓ fail-closed behavior

---

# Design Principle

State transitions are consequences of successful validation.

They are never the source of trust.