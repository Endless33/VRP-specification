# Multipath Correctness

**Document Version:** Public v1

**Status:** Public Engineering

---

# Purpose

This document describes the engineering correctness properties of the public VRP multipath manager.

The objective of the multipath subsystem is not simply to choose the fastest transport.

Its objective is to preserve session continuity while making deterministic transport decisions.

---

# Design Objective

The multipath manager continuously evaluates available transports and selects the most appropriate active path according to observable runtime state.

Selection must remain:

- deterministic
- reproducible
- explainable
- safe under concurrent execution

---

# Decision Inputs

Each candidate path is evaluated using observable engineering metrics.

Current scoring includes:

- Round-trip latency (RTT)
- Jitter
- Packet loss
- Observation freshness
- Explicit operational state

Only observable runtime information participates in path selection.

---

# Explicit Runtime Authority

Operational state has higher authority than measured metrics.

The following states are considered authoritative:

- FAILED
- QUARANTINED

These states cannot be silently cleared by later metric observations.

Recovery requires explicit runtime action.

---

# Candidate Evaluation

Each transport produces a candidate containing:

- transport identifier
- transport label
- engineering score
- observed runtime state

Candidates are evaluated independently.

Candidate evaluation does not modify session identity.

---

# Selection Rule

The selected transport must satisfy all of the following:

- eligible for use
- lowest engineering score
- not quarantined
- not explicitly failed

If multiple candidates produce equivalent scores, deterministic ordering is preserved.

---

# Failover

Failover occurs when:

- active transport disappears
- active transport becomes unusable
- another candidate exceeds switching policy
- runtime explicitly requests migration

Failover changes transport.

Failover does not create a new session.

---

# Recovery

Recovery is explicit.

FAILED and QUARANTINED paths remain unavailable until runtime performs recovery.

Metric improvement alone is insufficient.

This prevents accidental path resurrection.

---

# Anti-Flapping

Transport switching is intentionally rate-limited.

Minimum switch intervals reduce oscillation between equivalent transports.

Frequent switching is avoided unless engineering policy explicitly allows it.

---

# Session Continuity

Transport migration preserves:

- session identity
- logical runtime
- protocol state
- authority progression

Connectivity changes do not imply session recreation.

---

# Deterministic Evaluation

Equivalent runtime observations should produce equivalent transport decisions.

Deterministic behavior simplifies:

- debugging
- verification
- benchmarking
- evidence comparison

---

# Concurrency

Concurrent metric updates must not violate selection correctness.

The implementation is continuously validated through:

- race detector execution
- concurrent lifecycle tests
- stress testing
- benchmark execution

---

# Performance Engineering

The implementation is optimized for predictable engineering behavior.

Public verification includes benchmarking across increasing numbers of transport candidates.

Performance optimization must never compromise correctness.

---

# Public Validation

Engineering validation currently includes:

- multipath benchmarks
- scaling benchmarks
- candidate invariant validation
- explicit failure validation
- quarantine validation
- recovery validation
- race detector verification
- concurrent execution tests

These validations are intended to demonstrate engineering behavior rather than theoretical performance.

---

# Engineering Boundary

This document describes public engineering behavior.

It does not disclose:

- protected runtime implementation
- proprietary optimization techniques
- confidential production logic

---

# Final Principle

The purpose of the multipath manager is not simply to select a path.

Its purpose is to preserve session continuity while making deterministic, explainable, and reproducible transport decisions under changing network conditions.

Correctness always has higher priority than optimization.