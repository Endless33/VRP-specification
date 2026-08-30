# Veil Routing Protocol (VRP)

> **Continuity First. Session ≠ Transport.**

The **Veil Routing Protocol (VRP)** is a continuity-first runtime architecture for distributed systems operating across changing communication infrastructure.

Instead of binding execution to an IP address, interface, or transport path, VRP separates **Logical Session** identity from the underlying transport.

The Protected VRP Runtime adapts to infrastructure changes while preserving deterministic execution whenever architectural correctness permits.

---

# Core Principle

> **Logical Session ≠ Transport**

Communication infrastructure may evolve.

Application execution should not.

---

# What VRP Is

VRP is a runtime architecture designed to preserve application continuity across changing communication conditions.

The public specification defines:

- Logical Session model
- Canonical Authority model
- Runtime State Machine
- Recovery model
- Replay protection
- Transport abstraction
- Engineering evidence
- Evaluation methodology

The runtime implementation remains protected.

---

# What VRP Is NOT

VRP is **not**:

- a VPN
- a replacement for TCP
- a replacement for UDP
- a replacement for QUIC
- a replacement for TLS
- an SD-WAN product
- an Internet routing protocol

VRP is a continuity-first runtime architecture.

---

# Project Status

Current status:

- Public engineering specification
- Active architectural development
- Protected private runtime
- Independent engineering validation
- Controlled pilot evaluations

---

# Architecture Overview

```text
Application

        │

Runtime API

        │

Protected VRP Runtime

        │

Transport Abstraction

        │

Communication Infrastructure
```

Applications interact with Logical Sessions.

The runtime manages transport evolution.

---

# Architectural Guarantees

The public architecture defines observable guarantees including:

- Logical Session continuity
- Deterministic runtime decisions
- Canonical authority progression
- Replay rejection
- Observable recovery
- Engineering evidence
- Transport independence

Implementation details remain protected.

---

# Engineering Principles

VRP is built around several permanent engineering principles.

- Session ≠ Transport
- Correctness Before Availability
- Deterministic Runtime Decisions
- One Canonical Authority
- Replay Protection
- Observable Recovery
- Evidence Before Claims
- Protected Implementation

---

# Engineering Focus

Representative application domains include:

- eSIM platforms
- Telecommunications
- Edge Computing
- Industrial IoT
- Robotics
- Autonomous Systems
- Cloud Infrastructure
- Distributed Systems
- Critical Networking
- High-Availability Services

---

# Validation Philosophy

Engineering claims should be supported by reproducible evidence.

Validation focuses on observable runtime behavior rather than implementation disclosure.

Representative evaluation areas include:

- transport migration
- replay rejection
- stale-authority rejection
- deterministic recovery
- authority evolution
- failure injection
- stress validation
- engineering evidence verification

---

# Repository Structure

```text
docs/
├── architecture/
├── runtime/
├── security/
├── evaluation/
├── integration/
├── diagrams/
├── protocol/
├── rfc/
├── adr/
├── pilot/

examples/
engineering/
whitepaper/
releases/
legal/
```

---

# Public Documentation

The repository includes:

- Architecture
- RFC Series
- ADR Series
- Runtime Documentation
- Security Documentation
- Evaluation Documentation
- Integration Documentation
- Engineering Documentation
- Whitepapers
- Mermaid Diagrams
- Engineering Examples

Together these documents define the observable engineering model of VRP.

---

# Protected Runtime Boundary

This repository intentionally excludes:

- runtime source code
- proprietary algorithms
- implementation details
- optimization strategies
- cryptographic material
- production deployment logic
- transport scoring
- authority coordination algorithms
- internal runtime mechanisms

Observable architecture is public.

Implementation remains protected.

---

# Independent Evaluation

Organizations evaluate VRP through observable engineering behavior.

Evaluation does not require access to the Protected Runtime implementation.

Observable evaluation includes:

- runtime behavior
- recovery
- replay resistance
- authority progression
- engineering evidence
- deterministic execution

---

# Pilot Evaluation

Organizations with genuine continuity challenges may request participation in a controlled engineering evaluation.

Participation is reviewed individually.

Acceptance is not automatic.

Pilot availability is intentionally limited.

---

# Documentation Roadmap

Recommended reading order:

1. Executive Summary
2. Engineering Overview
3. Whitepaper
4. Architecture
5. RFC Series
6. Runtime Documentation
7. Security Documentation
8. Evaluation Documentation
9. Integration Documentation

---

# Design Philosophy

VRP does not attempt to replace existing Internet protocols.

Instead, it introduces a continuity-first runtime architecture that preserves Logical Sessions while communication infrastructure evolves.

The objective is not to hide failures.

The objective is to recover from them predictably.

---

# Contact

Engineering & Pilot

**jumpingvpn@proton.me**

Career

**riabovasvitalijus@gmail.com**

---

# License

Unless explicitly stated otherwise, this repository documents public architectural concepts and engineering guidance only.

Publication of this documentation does not grant rights to:

- the Protected VRP Runtime
- proprietary implementation
- source code
- confidential engineering assets
- internal runtime mechanisms

---

## Engineering Motto

> **Session ≠ Transport**

> **Continuity First**

> **Evidence Before Claims**