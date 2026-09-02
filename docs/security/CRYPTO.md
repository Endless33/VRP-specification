# Cryptographic Security Model

**Document Version:** Public v2

**Status:** Public

---

# Purpose

This document describes the public cryptographic security goals, architectural guarantees, and implementation boundaries of VRP.

It does **not** disclose proprietary runtime implementation details, internal algorithms, cryptographic material, or protected engineering mechanisms.

Its purpose is to explain **what security properties the architecture provides**, not **how the protected runtime implements them**.

---

# Design Philosophy

VRP does not rely on the secrecy of its architectural principles.

The architecture is public.

The implementation is protected.

Security claims must be supported by engineering evidence and reproducible validation.

---

# Security Objectives

The architecture is designed to provide:

- Session authenticity
- Session continuity
- Message integrity
- Replay resistance
- Authority validation
- Transport independence
- Deterministic state validation
- Evidence integrity
- Fail-closed behaviour
- Zero Trust operation

---

# Threat Model

The public architecture is designed to tolerate and detect scenarios including:

- Replay attempts
- Duplicate delivery
- Stale authority
- Message tampering
- Unauthorized state mutation
- Packet injection
- Transport migration
- NAT rebinding
- Dynamic IP changes
- Long-duration instability
- Network interruption and recovery

Additional implementation-specific protections may exist inside the protected runtime.

---

# Public Cryptographic Properties

The public architecture guarantees the following design properties.

## Identity Binding

Session identity remains independent from transport identity.

---

## Integrity

State transitions are validated before acceptance.

---

## Replay Resistance

Previously accepted protocol state cannot be accepted again without satisfying validation rules.

---

## Authority Validation

State authority is validated before protected state transitions occur.

---

## Evidence Integrity

Engineering evidence is designed to support deterministic verification.

---

## Fail-Closed Design

Unexpected or invalid protocol state is rejected rather than silently accepted.

---

# Independent Verification

VRP has been designed so that engineering behaviour can be evaluated without exposing protected runtime internals.

Public evaluation focuses on:

- observable behaviour
- reproducible evidence
- deterministic validation
- architectural properties

rather than implementation disclosure.

---

# Protected Runtime Boundary

The following implementation details are intentionally outside the scope of the public specification.

- runtime implementation
- internal state machine implementation
- key lifecycle management
- protected protocol internals
- implementation optimizations
- production deployment mechanisms

These implementation details are protected intellectual property.

Their exclusion from the public specification does not prevent independent architectural evaluation.

---

# Engineering Principle

VRP separates two concerns.

Architecture should be open to engineering review.

Implementation may remain protected intellectual property.

This separation allows independent evaluation without exposing proprietary runtime technology.

---

# Validation

Security properties should be evaluated through reproducible engineering validation.

The public specification encourages independent testing, adversarial evaluation, and evidence-based review rather than reliance on marketing claims or undocumented assumptions.

---

# Summary

The public specification explains:

- what the architecture guarantees
- what can be independently verified
- what engineering evidence should demonstrate

The protected runtime implements these architectural principles while preserving proprietary implementation details.