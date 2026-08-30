# Duplicate Execution

## Status

Public Protocol Specification

Version: 2.0

---

# Abstract

Duplicate Execution defines the observable architectural rules that prevent the same logical operation from being processed more than once.

Unlike replay protection, which focuses on previously accepted historical activity, duplicate execution protection ensures that concurrent or repeated observations do not result in multiple successful executions of the same logical action.

This document specifies observable behavior only.

Implementation details remain part of the protected VRP runtime.

---

# Purpose

Distributed systems regularly encounter duplicate events.

Examples include:

- retransmissions
- concurrent requests
- network retries
- infrastructure failover
- duplicated notifications
- repeated delivery attempts

Without explicit duplicate execution protection, these situations may lead to inconsistent runtime state.

---

# Replay vs Duplicate Execution

Although related, Replay Protection and Duplicate Execution address different architectural problems.

Replay Protection prevents historical execution from becoming valid again.

Duplicate Execution prevents multiple successful executions of the same logical operation while it is still relevant.

Both protections are required for deterministic runtime behavior.

---

# Design Objectives

Duplicate Execution protection provides:

- single logical execution
- deterministic outcomes
- runtime consistency
- authority preservation
- reproducible validation
- implementation independence

---

# Architectural Principle

One logical operation should produce one logical result.

The number of delivery attempts must not determine the number of successful executions.

Execution correctness is independent from network behavior.

---

# Observable Sources

Duplicate execution attempts may originate from:

- transport retries
- infrastructure retries
- concurrent workers
- operator actions
- distributed components
- recovery mechanisms

The runtime evaluates all observable sources using consistent validation rules.

---

# Observable Validation

Before accepting execution, the runtime evaluates observable conditions including:

- current runtime state
- authority consistency
- execution eligibility
- session validity
- runtime policy

Only validated execution becomes observable runtime state.

---

# Observable Outcomes

Possible observable outcomes include:

- execution accepted
- duplicate ignored
- duplicate rejected
- runtime unchanged
- evidence generated

The protected runtime determines how duplicate detection is implemented.

---

# Authority Consistency

Duplicate execution protection preserves canonical authority.

Repeated execution attempts must not introduce conflicting ownership or inconsistent runtime decisions.

Authority remains independent from delivery behavior.

---

# Recovery

Recovery never authorizes duplicate execution.

Recovery may continue logical execution.

It does not permit previously accepted logical work to execute a second time.

---

# Engineering Validation

Independent engineering teams may validate observable outcomes such as:

- single logical execution
- duplicate rejection
- deterministic behavior
- authority preservation
- runtime consistency

Evaluation focuses on observable protocol behavior.

---

# Evidence

Duplicate execution handling may generate observable engineering artifacts.

Examples include:

- execution reports
- duplicate rejection reports
- validation summaries
- runtime verdicts
- audit evidence

Evidence supports independent engineering verification.

---

# Protected Boundary

This document intentionally excludes:

- duplicate detection algorithms
- runtime identifiers
- implementation heuristics
- synchronization mechanisms
- protocol encoding
- internal execution tracking

These mechanisms remain part of the protected VRP runtime.

---

# Related Documents

- REPLAY_PROTECTION.md
- EPOCH_MODEL.md
- AUTHORITY_TRANSITIONS.md
- INVARIANTS.md
- RFC-0007-Duplicate-Execution.md

---

# Summary

Duplicate execution protection guarantees that one logical operation produces one logical outcome.

Network behavior may repeat.

Execution correctness does not.

The runtime preserves deterministic behavior regardless of delivery characteristics.

---

> One logical operation.

> One logical execution.

> One deterministic outcome.