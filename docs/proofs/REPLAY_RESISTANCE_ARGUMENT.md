# Replay Resistance Engineering Argument

**Document Version:** Public v1

**Status:** Public Engineering

---

# Purpose

This document describes the engineering reasoning behind replay resistance within the public VRP architecture.

The objective is to explain why replayed protocol activity should not modify canonical runtime state after it has already been processed.

This document presents an engineering argument supported by implementation, testing, and reproducible validation.

It is not a cryptographic proof.

---

# Engineering Objective

Replay attacks attempt to convince a runtime that previously observed protocol activity should be processed again.

A replay-resistant runtime must distinguish between:

- new protocol activity

and

- previously accepted activity.

---

# Architectural Principle

Replay protection belongs to the logical session.

It must not depend on the currently selected transport.

Transport migration must not invalidate replay protection.

---

# Session Ownership

Replay validation is performed within the context of an existing logical session.

A transport change does not create a new replay history.

Previously accepted protocol activity remains previously accepted regardless of transport replacement.

---

# Canonical State

Canonical runtime state progresses only through valid protocol transitions.

Replaying already accepted protocol activity must not advance canonical state.

Canonical history remains monotonic.

---

# Duplicate Processing

Repeated delivery may occur because of:

- network retries
- packet duplication
- routing changes
- delayed transmission
- transport migration

Duplicate delivery alone must not change runtime correctness.

---

# Session Continuity

Replay protection remains active throughout session migration.

Changing transport must not create an opportunity for previously rejected operations to become valid.

Replay history follows the session rather than the transport.

---

# Concurrent Execution

Replay validation must remain correct during concurrent execution.

Simultaneous processing should not allow duplicate protocol activity to bypass runtime validation.

Engineering verification includes:

- concurrent execution
- race detector verification
- stress testing
- duplicate validation

---

# Runtime Recovery

Recovery procedures preserve replay correctness.

Recovering communication does not imply clearing replay history.

Recovery restores communication.

It does not redefine protocol history.

---

# Deterministic Behavior

Equivalent replay attempts should produce equivalent runtime behavior.

Replay handling must remain deterministic.

Deterministic replay rejection simplifies:

- debugging
- verification
- evidence comparison
- engineering analysis

---

# Engineering Validation

Public engineering validation includes scenarios involving:

- replay attempts
- duplicate processing
- concurrent execution
- transport migration
- runtime verification
- benchmark execution

Engineering evidence should demonstrate that replay attempts do not modify canonical runtime state.

---

# Engineering Assumptions

This document assumes:

- valid runtime implementation
- documented engineering behavior
- preserved runtime invariants
- supported execution environment

Behavior outside these assumptions is not covered.

---

# Protected Boundary

This document intentionally does not disclose:

- protected replay algorithms
- confidential implementation techniques
- proprietary runtime mechanisms
- production deployment logic

Only public engineering behavior is described.

---

# Independent Verification

Independent engineers are encouraged to:

- inspect the implementation
- execute replay validation
- introduce duplicate traffic
- repeat benchmark execution
- compare generated evidence

Engineering confidence should come from reproducible verification.

---

# Final Principle

Replay resistance is not achieved by preventing packets from being transmitted.

It is achieved by preventing previously accepted protocol activity from changing canonical runtime state more than once.

As long as runtime invariants remain preserved, replay attempts should not violate protocol correctness regardless of transport behavior.