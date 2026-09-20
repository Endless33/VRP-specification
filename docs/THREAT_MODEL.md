# Threat Model

## Purpose

This document defines the observable threat model considered during public engineering validation of VRP.

It does not describe protected runtime implementation.

It defines the classes of failures and adversarial behavior that the architecture is expected to detect, contain, reject or recover from.

---

# Design Philosophy

VRP assumes that failures are normal.

Networks are expected to change.

Routes are expected to disappear.

Packets are expected to be delayed.

Infrastructure is expected to restart.

The engineering objective is to preserve canonical execution while these events occur.

---

# Threat Categories

The public validation program considers threats including:

- replay attacks
- duplicate execution
- stale authority
- authority conflicts
- split-brain scenarios
- transport instability
- path migration
- restart boundaries
- lifecycle races
- concurrent execution
- contradictory state transitions
- execution ordering failures
- resource exhaustion
- evidence manipulation attempts

---

# Replay

Replay attacks attempt to reuse previously valid execution.

Expected behavior:

- replay detected
- replay rejected
- canonical execution preserved

---

# Duplicate Execution

Duplicate events must not create duplicate canonical transitions.

Expected behavior:

- duplicate detected
- duplicate rejected
- history remains canonical

---

# Stale Authority

Previously valid authority must never regain control after ownership changes.

Expected behavior:

- stale authority rejected
- current authority preserved
- canonical ownership maintained

---

# Split-Brain

Multiple conflicting execution paths must never become simultaneously canonical.

Expected behavior:

- conflict detected
- canonical execution preserved
- contradictory state rejected

---

# Restart Boundary

A runtime restart must not allow historical execution to become current again.

Expected behavior:

- stale execution rejected
- lifecycle boundary preserved
- canonical runtime continues correctly

---

# Transport Instability

Transport behavior is expected to change.

Examples include:

- Wi-Fi loss

- mobile transition

- relay migration

- NAT rebinding

- temporary blackout

Expected behavior:

- transport may change
- session continuity preserved
- canonical execution preserved

---

# Concurrent Execution

Validation intentionally exercises concurrent execution.

Examples include:

- concurrent failover

- authority races

- duplicate requests

- migration races

Expected behavior:

- deterministic result
- no canonical divergence
- race-free observable behavior

---

# Resource Boundaries

Validation also considers resource stability.

Engineering objectives include:

- bounded runtime behavior

- deterministic allocation

- predictable execution

The architecture is evaluated under sustained execution rather than isolated demonstrations.

---

# Evidence Integrity

Validation artifacts themselves must remain trustworthy.

Engineering evidence should remain:

- reproducible

- deterministic

- independently reviewable

Observable engineering conclusions should originate from reproducible evidence rather than narrative description.

---

# Threats Outside Current Scope

This public validation does not claim protection against every possible operational threat.

Examples include:

- physical compromise

- supply-chain compromise

- credential theft

- infrastructure ownership

Such topics are intentionally outside the scope of observable runtime validation.

---

# Engineering Objective

The objective is not to eliminate failures.

The objective is to preserve correct canonical execution while failures occur.

---

# Closing Statement

Reliable distributed systems are not created by assuming perfect networks.

They are created by remaining correct when networks are imperfect.

**Continuity First.**