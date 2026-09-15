# VRP Engineering Principles

Document version: Public v1

Status: Public

---

# Purpose

This document defines the engineering principles used during the
development of VRP.

These principles govern implementation, validation, testing,
documentation, and long-term maintenance.

They are intended to remain stable even as the implementation evolves.

---

# Principle 1

Validation before mutation.

Canonical runtime state is never modified until all required validation
steps have completed successfully.

---

# Principle 2

Fail closed.

Whenever correctness cannot be established,
the operation is rejected.

The runtime never assumes correctness.

---

# Principle 3

Deterministic execution.

The same canonical input must always produce the same canonical result.

Validation never depends on:

- scheduling

- timing

- CPU load

- transport latency

---

# Principle 4

Authority is explicit.

Authority is never inferred.

Authority is always validated.

---

# Principle 5

Session is independent from transport.

Transport may change.

Relay may change.

Network path may change.

Canonical session identity remains stable.

---

# Principle 6

Layer separation.

The following concepts remain independent:

- SessionID

- Lifetime Identity

- Authority

- Lease

- Runtime State

No layer replaces another.

---

# Principle 7

Canonical history is immutable.

Accepted transitions become permanent history.

Rejected operations never become history.

---

# Principle 8

Evidence follows validation.

Evidence documents runtime behavior.

Evidence never changes runtime behavior.

---

# Principle 9

Security claims require engineering proof.

Claims are supported by:

- regression tests

- race detector

- shuffle execution

- adversarial validation

- evidence bundles

---

# Principle 10

Replay protection is layered.

Replay is rejected through independent validation layers.

No single mechanism is trusted alone.

---

# Principle 11

Bootstrap is privileged.

Session creation is a trusted operation.

Generic event delivery is not trusted to create canonical sessions.

---

# Principle 12

Restart creates a new lifecycle namespace.

A restarted runtime never continues an old canonical lifetime.

Restart always establishes a new canonical execution context.

---

# Principle 13

Backward compatibility must not weaken security.

Compatibility may exist temporarily.

Compatibility must never bypass canonical validation.

---

# Principle 14

Concurrency must preserve correctness.

Parallel execution must not change:

- canonical state

- authority

- history

- lifecycle

Correctness always has priority over throughput.

---

# Principle 15

Public architecture.

Protected implementation.

The architectural model may be public.

Protected runtime implementation remains private.

Security does not depend on hiding architectural concepts.

---

# Principle 16

Engineering over marketing.

Every architectural statement should be reproducible.

Every engineering claim should be verifiable.

Every security claim should be testable.

---

# Principle 17

Minimal trusted surface.

Only components that require authority should become trusted.

Everything else is treated as untrusted input.

---

# Principle 18

Long-term maintainability.

Architecture should remain understandable years after implementation.

Documentation evolves together with validation.

Evidence evolves together with documentation.

---

# Summary

VRP engineering follows one fundamental rule:

Correctness first.

Security first.

Determinism first.

Evidence first.

Everything else is secondary.