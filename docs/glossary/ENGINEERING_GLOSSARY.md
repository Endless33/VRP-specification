# Engineering Glossary

**Document Version:** Public v1

**Status:** Public Engineering

---

# Purpose

This glossary defines the public engineering terminology used throughout the VRP specification.

The objective is to ensure consistent interpretation of architectural concepts across all public documentation.

Unless explicitly stated otherwise, these definitions apply throughout the repository.

---

# Adapter

A software component that connects an application to the Protected VRP Runtime.

The adapter provides the engineering boundary between business logic and runtime functionality.

---

# Authority

The canonical entity responsible for approving valid session progression.

Only one canonical authority is considered valid for a given Logical Session at any point in time.

---

# Canonical Authority

The single authoritative state progression accepted by the runtime.

Conflicting authority histories are rejected.

---

# Continuity

The ability of a Logical Session to preserve its identity while underlying communication infrastructure changes.

Continuity does not imply uninterrupted packet delivery.

---

# Engineering Evidence

Observable information generated during validation.

Examples include:

- benchmark reports
- validation logs
- engineering reports
- runtime observations
- reproducible test results

Engineering evidence supports architectural conclusions.

---

# Epoch

A monotonically increasing authority generation used to distinguish valid session progression from stale state.

Older epochs cannot supersede newer canonical authority.

---

# Fail-Closed

An engineering principle where uncertain or invalid runtime conditions are rejected rather than accepted.

---

# Logical Session

The persistent logical identity representing application communication.

A Logical Session is intentionally independent of transport implementation.

---

# Protected Runtime

The proprietary implementation responsible for executing the public VRP architecture.

Its internal implementation is intentionally excluded from the public repositories.

---

# Recovery

The deterministic process of restoring valid runtime progression after communication disruption or infrastructure change.

Recovery follows documented architectural rules.

---

# Replay Attack

An attempt to reuse previously valid protocol data outside its valid execution context.

Replay attacks are expected to be rejected by the architectural model.

---

# Session Continuity

Preservation of Logical Session identity across transport evolution.

This is a primary engineering objective of VRP.

---

# Transport

The communication mechanism carrying protocol traffic.

Examples may include TCP, UDP, QUIC, or other transport technologies.

The transport is replaceable.

The Logical Session is not.

---

# Transport Independence

The architectural property that Logical Session identity is not permanently bound to any individual transport.

---

# Validation

The engineering process of verifying architectural behavior through reproducible testing and observable evidence.

---

# Zero Trust

An engineering security principle where runtime decisions are validated rather than assumed to be trustworthy.

---

# Final Principle

Engineering terminology should remain precise.

Consistent definitions improve communication, documentation, implementation, and independent technical evaluation.