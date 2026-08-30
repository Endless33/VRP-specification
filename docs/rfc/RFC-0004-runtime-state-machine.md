# RFC-0004 — Runtime State Machine

**Document Status:** Public Specification

**RFC Number:** RFC-0004

**Version:** 1.0

**Category:** Core Architecture

---

# Abstract

This document specifies the observable Runtime State Machine of the Veil Routing Protocol (VRP).

The Runtime State Machine defines the externally observable lifecycle of a Logical Session.

Its purpose is to guarantee deterministic state evolution while preserving architectural invariants.

Internal implementation remains part of the protected VRP Runtime.

---

# 1. Introduction

A runtime that performs continuity management must evolve through well-defined states.

Observable state transitions allow engineering teams to:

- validate correctness
- verify recovery
- reproduce execution
- audit runtime behavior
- detect invalid transitions

The Runtime State Machine provides that observable model.

---

# 2. Terminology

The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** are interpreted as described in RFC 2119.

---

# 3. Runtime Principle

The runtime SHALL always occupy one observable state.

State evolution MUST be deterministic.

Equivalent inputs SHOULD produce equivalent observable transitions.

---

# 4. Observable States

The public runtime exposes the following conceptual states.

```
Created

↓

Initialized

↓

Authority Established

↓

Active

↓

Transport Evolution (optional)

↓

Recovery (optional)

↓

Active

↓

Termination

↓

Closed
```

Additional implementation states MAY exist internally.

---

# 5. State Definitions

## Created

Runtime object exists.

No execution has begun.

---

## Initialized

Internal initialization has completed.

Runtime resources are available.

Execution has not yet started.

---

## Authority Established

Canonical authority has been determined.

The runtime is permitted to begin execution.

---

## Active

Normal runtime execution.

Application continuity is maintained.

Transport evolution MAY occur.

---

## Transport Evolution

Transport selection changes while the Logical Session remains unchanged.

Transport evolution MUST NOT invalidate the session.

---

## Recovery

Recovery attempts to preserve continuity after observable disruption.

Recovery MAY return to Active.

Recovery MAY terminate execution if invariants cannot be preserved.

---

## Termination

Execution is intentionally ending.

No additional authority evolution occurs.

---

## Closed

Runtime resources have been released.

The Logical Session no longer exists.

---

# 6. State Transition Rules

Observable transitions MUST follow valid progression.

Examples:

Created

↓

Initialized

↓

Authority Established

↓

Active

Active

↓

Recovery

↓

Active

Active

↓

Termination

↓

Closed

Invalid transitions MUST be rejected.

---

# 7. Determinism

The runtime SHOULD produce deterministic state evolution.

Equivalent execution conditions SHOULD generate equivalent observable transitions.

Deterministic behavior improves reproducibility.

---

# 8. Failure Handling

Observable failures MAY trigger:

- Recovery
- Termination

Observable failure MUST NOT produce undefined runtime state.

---

# 9. Authority Interaction

Authority evolution MUST remain consistent with runtime state.

Historical authority MUST NOT reactivate after observable replacement.

Authority consistency is defined by RFC-0002.

---

# 10. Session Relationship

State transitions operate on the Logical Session.

Transport evolution MUST NOT redefine session identity.

Logical Session semantics are defined by RFC-0001.

---

# 11. Engineering Validation

Engineering teams SHOULD validate:

- state ordering
- deterministic evolution
- recovery correctness
- invalid transition rejection
- observable continuity

Validation focuses on externally visible behavior.

---

# 12. Security Considerations

Deterministic state evolution improves:

- replay resistance
- authority consistency
- recovery correctness
- engineering reproducibility

Implementation details remain protected.

---

# 13. Non-Goals

This RFC does not define:

- internal scheduler
- concurrency model
- locking strategy
- memory management
- transport scoring
- implementation algorithms

These remain proprietary.

---

# 14. Related RFCs

RFC-0001 — Logical Session Identity

RFC-0002 — Authority Epochs

RFC-0003 — Transport Abstraction

RFC-0007 — Failure Recovery

---

# 15. Summary

The Runtime State Machine defines the observable lifecycle of execution.

Engineering teams validate state evolution.

Protected implementation determines how those states are achieved.

---

## Normative Requirements

- The runtime **MUST** occupy one observable state.
- Invalid transitions **MUST** be rejected.
- State evolution **SHOULD** be deterministic.
- Recovery **MUST NOT** violate architectural invariants.
- Terminated sessions **MUST NOT** resume execution.

---

## Design Principle

> Execution evolves through states.

> States evolve deterministically.

> Deterministic evolution preserves continuity.