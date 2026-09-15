# Restart Recovery Model

Document version: Public v1

Status: Public

---

# Purpose

This document defines how VRP behaves across process restart.

Restart must never permit resurrection of a previous canonical session
lifecycle.

Recovery restores operation.

Recovery never restores authority.

Recovery never restores lifecycle identity.

---

# Fundamental Principle

Restart

≠

Session Continuation

Restart creates a new runtime instance.

A new runtime instance creates a new lifecycle namespace.

---

# Canonical Rule

After restart:

LifetimeEpoch MUST change.

Incarnation restarts inside the new namespace.

The previous namespace becomes permanently historical.

---

# Lifecycle Model

Manager A

↓

LifetimeEpoch A

↓

Restart

↓

Manager B

↓

LifetimeEpoch B

↓

New Canonical Runtime

---

# Historical Namespace

The previous runtime becomes historical evidence only.

Historical runtime:

- may be inspected

- may be exported

- may be verified

Historical runtime never becomes authoritative again.

---

# Accepted Recovery

Recovery performs:

- runtime initialization

- trusted bootstrap

- canonical validation

- authority reconstruction

- lease validation

Recovery never bypasses validation.

---

# Rejected Recovery

The following are rejected:

- stale replay

- stale authority

- stale lifetime

- stale lease

- delayed canonical events

- delayed migration

- delayed recovery

---

# Runtime Initialization

Restart initializes:

- SessionManager

- AuthorityManager

- Runtime

- Event Log

- Canonical State

Each component starts inside the new lifecycle namespace.

---

# Recovery Validation

Recovery follows the same canonical validation pipeline:

Trusted Bootstrap

↓

Lifetime Validation

↓

Authority Validation

↓

Lease Validation

↓

State Validation

↓

Canonical Runtime

---

# Restart Safety

A restarted runtime never trusts:

- old process memory

- old runtime state

- stale lifecycle identifiers

- stale ownership

Only validated canonical state becomes active.

---

# Canonical Guarantees

Restart guarantees:

✓ new lifecycle namespace

✓ deterministic recovery

✓ replay rejection

✓ stale lifecycle rejection

✓ canonical authority reconstruction

✓ immutable historical evidence

✓ fail-closed startup

---

# Evidence

Restart evidence may include:

- runtime initialization

- validation results

- lifecycle identifiers

- recovery outcome

Evidence does not restore runtime.

Evidence documents runtime.

---

# Security Guarantees

Restart recovery guarantees:

✓ restart-safe lifecycle

✓ deterministic startup

✓ replay resistance

✓ stale authority rejection

✓ immutable historical evidence

✓ fail-closed recovery

---

# Design Principle

Recovery creates a new canonical runtime.

Recovery never revives the previous one.