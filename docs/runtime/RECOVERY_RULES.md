# Recovery Rules

## Status

Public Runtime Documentation

Version: 1.0

---

# Purpose

This document defines the observable Recovery Rules of the Veil Routing Protocol (VRP).

Recovery is the controlled process by which the runtime evaluates whether execution may safely continue after an observable disruption.

Recovery never exists to maximize uptime alone.

Its objective is to preserve architectural correctness.

---

# Engineering Philosophy

Recovery is not success.

Correct recovery is success.

The runtime never attempts recovery if doing so would violate architectural invariants.

Whenever correctness cannot be preserved, recovery ends with safe termination.

---

# Recovery Objectives

Recovery exists to preserve:

- Logical Session identity
- Canonical Authority
- Deterministic Runtime Decisions
- Replay Protection
- Runtime State consistency
- Evidence integrity

Availability is important.

Correctness is mandatory.

---

# Recovery Lifecycle

```
Normal Execution
        │
        ▼
Failure Detected
        │
        ▼
Recovery Evaluation
        │
        ▼
Recovery Authorized
        │
        ▼
Recovery Execution
        │
        ▼
Recovery Validation
        │
 ┌──────┴────────┐
 │               │
 ▼               ▼
Recovered    Safe Termination
```

Recovery proceeds only after successful evaluation.

---

# Rule 1 — Preserve the Logical Session

Recovery MUST preserve the existing Logical Session.

Recovery MUST NOT silently create a replacement session.

Session continuity remains the primary objective.

---

# Rule 2 — Preserve Canonical Authority

Recovery MUST preserve canonical authority.

Historical authority MUST NOT become canonical through recovery.

Authority progression remains monotonic.

---

# Rule 3 — Reject Replay

Recovery MUST NOT authorize historical execution.

Replay protection remains active throughout the entire recovery lifecycle.

Recovery never weakens replay resistance.

---

# Rule 4 — Preserve Runtime State

Recovery MUST preserve Runtime State Machine correctness.

Invalid state transitions remain prohibited.

Recovery never bypasses runtime state validation.

---

# Rule 5 — Deterministic Decisions

Equivalent recovery conditions SHOULD produce equivalent observable outcomes.

Recovery behavior should remain reproducible.

Implementation mechanisms remain protected.

---

# Rule 6 — Preserve Evidence

Recovery MUST preserve engineering evidence.

Observable recovery history must remain complete.

Evidence should accurately reflect runtime behavior.

Recovery never rewrites accepted history.

---

# Recovery Authorization

Recovery begins only after runtime evaluation.

Observable factors may include:

- transport availability
- authority consistency
- runtime state
- replay status
- recovery policy

External systems may request recovery.

Only the runtime authorizes recovery.

---

# Recovery Outcomes

Observable recovery outcomes include:

## Successful Recovery

Execution continues.

Architectural invariants remain preserved.

---

## Recovery Rejected

Recovery is denied because correctness cannot be guaranteed.

Execution remains unchanged or terminates safely.

---

## Safe Termination

Recovery cannot preserve architectural correctness.

Execution terminates deterministically.

---

# Recovery and Transport

Transport evolution MAY occur during recovery.

Transport replacement alone never constitutes recovery.

Recovery concerns execution.

Transport concerns communication.

---

# Recovery and Authority

Recovery never performs authority rollback.

Authority remains monotonic throughout recovery.

Canonical authority remains unique.

---

# Recovery and Evidence

Recovery generates observable engineering artifacts including:

- runtime events
- recovery timeline
- authority history
- validation reports
- evidence bundles

Evidence enables independent verification.

---

# Engineering Validation

Typical recovery validation includes:

- transport interruption
- transport restoration
- stale authority rejection
- replay rejection
- concurrent recovery
- deterministic recovery
- recovery stress

Validation focuses on observable behavior.

---

# Relationship to Other Documents

This document complements:

- INVARIANTS.md
- STATE_MACHINE.md
- EVENT_FLOW.md
- AUTHORITY_TRANSITIONS.md
- FAILURE_HANDLING.md

It also supports:

- RFC-0007 — Failure Recovery
- RFC-0008 — Multipath Selection

---

# Summary

Recovery is an architectural decision rather than a transport operation.

Recovery exists only when correctness can be preserved.

Whenever correctness cannot be preserved, safe termination is considered the correct outcome.

Observable engineering evidence documents every recovery decision.

---

## Design Principles

- Recover only when correct.
- Preserve the Logical Session.
- Preserve canonical authority.
- Reject replay.
- Preserve deterministic execution.
- Generate reproducible evidence.
- Prefer correctness over availability.