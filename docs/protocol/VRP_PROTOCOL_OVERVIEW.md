# VRP Protocol Overview

## Status

Public Protocol Specification

Version: 2.0

---

# Abstract

The Veil Routing Protocol (VRP) is a continuity-first networking architecture designed to preserve logical communication across changing transport conditions.

Unlike traditional approaches that closely associate a communication session with a transport endpoint, VRP separates logical session identity from the transport carrying packets.

This architectural separation enables runtime-controlled continuity while maintaining deterministic behavior and observable engineering validation.

This document provides a high-level overview of the protocol architecture.

Implementation details remain part of the protected VRP runtime.

---

# Design Goals

VRP has several primary objectives.

- Preserve logical session continuity.
- Separate session identity from transport.
- Maintain deterministic runtime behavior.
- Support controlled recovery.
- Reject stale authority.
- Reject replayed execution.
- Produce verifiable engineering evidence.
- Operate independently of specific transport technologies.

---

# Scope

This specification describes:

- public architecture
- observable protocol behavior
- runtime concepts
- validation philosophy
- engineering terminology

This specification does **not** disclose protected implementation mechanisms.

---

# Architectural Layers

VRP consists of several conceptual layers.

```
Application
        │
        ▼
Logical Session
        │
        ▼
Runtime
        │
        ▼
Authority Model
        │
        ▼
Transport Layer
        │
        ▼
Network
```

Each layer has distinct responsibilities.

The runtime coordinates interaction between these layers while preserving deterministic execution.

---

# Core Concepts

## Logical Session

A logical session represents application continuity.

Its lifecycle is independent of the currently active transport.

---

## Runtime

The runtime evaluates observable conditions and produces deterministic decisions.

Examples include:

- transport migration
- recovery
- authority validation
- replay rejection
- evidence generation

---

## Authority

A logical session has one observable canonical authority.

Authority evolution follows deterministic runtime policy.

Historical authority does not automatically become valid again.

---

## Transport

Transport provides packet delivery.

Transport may change without requiring logical session replacement.

Examples include:

- Wi-Fi
- Ethernet
- LTE
- 5G
- relay networks
- future transport technologies

---

# Observable Behavior

Public evaluation focuses on observable protocol properties.

Examples include:

- continuity preservation
- deterministic recovery
- authority consistency
- replay rejection
- stale authority rejection
- evidence verification

These properties may be independently validated.

---

# Failure Philosophy

VRP assumes that failures are expected.

Examples include:

- transport interruption
- mobility
- infrastructure restart
- degraded connectivity
- network partition
- authority conflicts

The runtime evaluates these conditions according to protected policy.

---

# Recovery Philosophy

Recovery is explicit.

Recovery does not occur simply because network conditions improve.

Recovery must satisfy runtime policy before execution resumes.

Correctness always takes precedence over continuity.

---

# Validation Philosophy

Engineering confidence should be built through observable evidence.

Validation focuses on runtime behavior rather than implementation visibility.

Typical validation areas include:

- deterministic execution
- adversarial testing
- stress testing
- recovery validation
- replay resistance
- authority consistency

---

# Security Philosophy

Security is integrated into runtime behavior.

Examples include:

- authority validation
- replay protection
- deterministic transitions
- protected runtime boundaries
- controlled recovery

Public documentation explains objectives rather than implementation.

---

# Protected Runtime

This repository intentionally excludes:

- runtime source code
- internal protocol implementation
- scheduling algorithms
- packet encoding
- proprietary synchronization
- optimization strategies
- protected cryptographic mechanisms

Evaluation does not require disclosure of these components.

---

# Engineering Evaluation

Independent engineering teams may evaluate:

- runtime behavior
- observable continuity
- deterministic execution
- authority evolution
- recovery behavior
- evidence integrity

without access to protected implementation details.

---

# Related Documents

- SESSION_NOT_TRANSPORT.md
- RUNTIME_MODEL.md
- AUTHORITY_MODEL.md
- INVARIANTS.md
- STATE_MACHINE.md

---

# Summary

VRP is not defined by a specific transport technology.

It is defined by deterministic runtime behavior that preserves logical continuity across changing network conditions while maintaining correctness and observable engineering evidence.

---

> Session identity is stable.

> Transport is replaceable.

> Runtime decisions are deterministic.

> Evidence supports engineering confidence.