# Replay Protection

## Status

Public Protocol Specification

Version: 2.0

---

# Abstract

Replay Protection defines the observable architectural rules that prevent previously accepted protocol activity from being interpreted as new execution.

The objective is to preserve deterministic runtime behavior by ensuring that historical observations cannot become valid execution solely because they appear again.

This document specifies observable behavior only.

Implementation mechanisms remain part of the protected VRP runtime.

---

# Purpose

Distributed systems continuously process large numbers of events originating from different transports, networks and execution environments.

Without replay protection, previously accepted operations could incorrectly influence the current runtime state.

Replay Protection ensures that historical execution never becomes current execution.

---

# Design Objectives

Replay Protection is designed to provide:

- deterministic execution
- execution uniqueness
- observable consistency
- authority preservation
- reproducible validation
- protection against historical execution reuse

---

# Architectural Principle

Execution is accepted because it is valid now.

Execution is never accepted merely because it was valid before.

Observable runtime correctness depends on current validation rather than historical acceptance.

---

# Replay Events

Examples of replay attempts include:

- duplicated protocol messages
- duplicated runtime events
- repeated authority transitions
- repeated recovery requests
- duplicated control operations
- delayed historical observations

Replay attempts may occur intentionally or unintentionally.

The runtime evaluates both using the same architectural principles.

---

# Observable Validation

Every observable execution request is evaluated before affecting runtime state.

Observable validation may include:

- runtime consistency
- authority validation
- policy verification
- execution freshness
- session validation

Only validated execution becomes authoritative runtime behavior.

---

# Deterministic Outcomes

Equivalent replay attempts should produce equivalent observable outcomes.

Possible observable results include:

- replay rejected
- execution ignored
- runtime unchanged
- evidence generated

Protected implementation determines how replay detection is performed.

---

# Replay and Authority

Replay protection is independent from authority evolution.

Changing authority does not invalidate replay protection.

Likewise, replay rejection does not imply authority evolution.

These architectural responsibilities remain separate.

---

# Replay and Transport

Replay protection is independent from transport.

Observable replay behavior remains consistent regardless of whether communication occurs through:

- Ethernet
- Wi-Fi
- LTE
- 5G
- relay infrastructure
- future transport technologies

Transport evolution must not weaken replay protection.

---

# Replay During Recovery

Recovery does not relax replay validation.

Previously accepted execution remains historical even during:

- recovery
- transport migration
- authority transition
- infrastructure failover

Recovery preserves continuity without accepting historical execution as new execution.

---

# Engineering Validation

Independent evaluation may verify observable outcomes such as:

- replay rejected
- runtime unchanged
- authority preserved
- deterministic behavior maintained
- evidence generated

Validation focuses on externally observable properties rather than implementation details.

---

# Evidence

Replay handling may generate observable engineering evidence.

Examples include:

- replay rejection reports
- validation summaries
- runtime verdicts
- audit artifacts
- engineering reports

Evidence documents observable runtime behavior.

---

# Protected Boundary

This specification intentionally excludes:

- replay detection algorithms
- runtime identifiers
- synchronization methods
- internal validation procedures
- protocol encoding
- implementation heuristics

These mechanisms remain part of the protected VRP runtime.

---

# Related Documents

- EPOCH_MODEL.md
- AUTHORITY_TRANSITIONS.md
- FAILURE_HANDLING.md
- INVARIANTS.md
- RFC-0006-Replay-Protection.md

---

# Summary

Replay Protection preserves execution uniqueness.

Historical observations remain historical.

Only validated execution becomes current runtime behavior.

Deterministic replay rejection strengthens reproducibility, engineering confidence and runtime integrity.

---

> Previous execution is history.

> Current execution is validated.

> Replay never becomes reality.