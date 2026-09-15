# Fail-Closed Security Model

Document version: Public v1

Status: Public

---

# Purpose

This document defines the fail-closed security philosophy used throughout
the VRP runtime.

Whenever validation cannot prove that an operation is safe,
the operation is rejected.

VRP never assumes correctness.

VRP requires correctness to be demonstrated.

---

# Fundamental Principle

Unknown

↓

Reject

Not

Unknown

↓

Accept

---

# Security Rule

Every security-sensitive operation must satisfy all required validation
steps before canonical mutation is permitted.

If any validation stage fails:

the operation stops immediately.

---

# Validation Pipeline

Incoming Operation

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

Failure at any stage prevents all following stages.

---

# Rejected Operations

Examples include:

- replay

- stale lifetime

- stale authority

- stale lease

- duplicate transition

- invalid transition

- unauthorized bootstrap

- malformed lifecycle identity

---

# Canonical State

Rejected operations never modify:

- SessionManager

- Event Log

- Transition History

- Authority

- Lease

- Runtime State

- Canonical Lifetime

---

# Event Log

Rejected operations never enter the canonical event log.

Only accepted canonical events become history.

---

# History

Rejected operations never become transition history.

Canonical history records only validated transitions.

---

# Authority

Authority never changes because validation failed.

Authority changes only after successful canonical validation.

---

# Runtime

Runtime never attempts partial recovery after failed validation.

The failed operation terminates immediately.

Canonical runtime remains unchanged.

---

# Bootstrap

Bootstrap follows the same rule.

If trusted creation authority cannot be established:

no canonical session is created.

---

# Restart

Restart never bypasses validation.

A restarted runtime creates a new lifecycle namespace.

Old lifecycle identifiers never become authoritative again.

---

# Error Handling

Validation errors are explicit.

Silent fallback is forbidden.

Silent acceptance is forbidden.

Implicit trust is forbidden.

---

# Security Guarantees

The fail-closed model guarantees:

✓ deterministic rejection

✓ canonical integrity

✓ replay resistance

✓ stale lifecycle rejection

✓ stale authority rejection

✓ immutable canonical history

✓ restart-safe behavior

✓ explicit validation

---

# Design Principle

VRP always prefers availability loss over integrity loss.

A rejected operation is acceptable.

A wrongly accepted operation is not.