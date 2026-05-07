# VRP Specification

Formal specification of the Veil Routing Protocol (VRP).

VRP is a continuity-first protocol model for preserving execution correctness under unstable transport conditions.

The core idea is simple:

```text
session != transport
```

Transport may change, fail, restart, disappear, or reattach.

Execution truth must remain bounded, validated, and consistent.

---

# Purpose

This repository defines the canonical specification for VRP, including:

- protocol architecture
- threat model
- cryptographic direction
- movement policy
- session continuity semantics
- authority and epoch behavior
- replay-safe execution rules
- recovery and self-healing design direction

This repository is the specification layer.

Runtime proofs and demos belong in implementation repositories.

---

# Core Principle

Traditional networking systems often treat connection loss as a reason to reconnect and rebuild state.

VRP treats transport instability as an expected condition.

```text
connection lost
-> transport changes
-> session identity remains stable
-> execution continues only if validation allows it
```

VRP does not attempt to make transport immortal.

VRP attempts to prevent transport failure from becoming execution corruption.

---

# Design Model

VRP is built around the following invariants:

- session identity is above transport
- transport is a replaceable carrier
- authority is explicit
- epochs bound execution validity
- stale packets are rejected
- duplicate mutations are rejected
- invalid packets cannot mutate state
- recovery must preserve canonical execution truth
- default state is reject

Continuity is not faster reconnect.

Continuity is a protected execution boundary.

---

# Repository Structure

```text
spec/
    threat-model.md
        Defines adversaries, assets, non-goals, detection signals, and security posture.

    protocol.md
        Defines node roles, route construction, session lifecycle, route movement, and protocol behavior.

    crypto.md
        Defines cryptographic primitives, key exchange direction, packet encryption, key rotation, and integrity requirements.

    movement-policy.md
        Defines route movement, anomaly response, periodic movement, and adaptive rerouting rules.

drafts/
    Experimental notes, early proposals, and design explorations.
```

---

# Specification Scope

This repository describes:

- VRP protocol behavior
- continuity-first session model
- adaptive route movement
- partial node knowledge
- replay and sequencing expectations
- epoch-aware execution direction
- recovery-oriented protocol semantics
- security boundaries and non-goals

This repository does not contain:

- production keys
- deployment credentials
- private server configuration
- real user traffic
- production packet captures
- implementation secrets
- commercial deployment materials

---

# Threat Model Summary

VRP is designed to improve resistance against:

- passive network observation
- active MITM manipulation
- replay attempts
- packet injection attempts
- route correlation over time
- partial relay compromise
- targeted disruption
- transport instability

VRP does not claim protection against:

- global passive adversaries observing all links
- full endpoint compromise
- kernel malware
- browser fingerprinting
- user behavioral deanonymization
- legal coercion
- compromised operating systems

Security claims outside the threat model are invalid.

---

# Protocol Direction

VRP uses movement and bounded execution as defensive primitives.

A VRP implementation may:

- rotate routes periodically
- rotate routes reactively after anomaly detection
- avoid recently used nodes
- rotate keys during route movement
- reject stale epochs
- reject duplicate mutations
- preserve session identity across transport changes
- halt unsafe execution when contradictions are detected
- resume only after recovery validation

The primary defensive strategy is:

```text
escape unsafe transport
preserve session truth
reject invalid execution
```

---

# Continuity Semantics

VRP separates three concepts that are often treated as one:

```text
session identity
transport attachment
execution authority
```

A session may survive even when the underlying transport changes.

A transport may be replaced without resetting logical execution.

A mutation may commit only if it passes authority, epoch, replay, and admission checks.

---

# Recovery Direction

VRP is moving toward bounded recovery semantics.

Recovery is not defined as simply restarting a process.

Recovery means:

```text
fault detected
-> unsafe execution frozen
-> canonical state restored
-> epoch advanced if required
-> transport reattached
-> stale packets rejected
-> execution resumed only after validation
```

The goal is continuity-preserving recovery.

Runtime components may restart.

Execution truth must remain consistent.

---

# Relationship to Implementations

Implementations such as:

- Jumping VPN
- vrp-demo
- vrp-sdk
- continuity-runtime-demo
- jumping-vpn-core

should reference this repository as the canonical specification source.

Implementation repositories may contain:

- runnable demos
- proof logs
- runtime experiments
- packet execution gates
- contradiction detectors
- recovery controllers
- transport adapters
- test harnesses

The specification repository defines what the protocol means.

Implementation repositories demonstrate how the behavior is enforced.

---

# Current Research Status

VRP is under active development.

The architecture has evolved from an adaptive route movement model into a broader continuity-first execution model.

Current focus areas include:

- transport-independent sessions
- deterministic commit admission
- epoch-based authority
- replay-safe execution
- real UDP continuity validation
- contradiction detection
- recovery journals
- bounded runtime recovery
- self-healing continuity semantics

---

# Non-Goals

VRP is not currently claiming to be:

- a production VPN
- a finished anonymity network
- a consensus replacement
- a complete cryptographic product
- a universal security solution
- a protection layer against compromised endpoints

VRP is a protocol research project focused on execution continuity under network instability.

---

# Intellectual Origin

VRP — Veil Routing Protocol — and the continuity-first execution model were originally designed and developed by Vitalijus Riabovas.

Core principles introduced in this research include:

- session != transport
- transport is a replaceable carrier
- failure does not automatically imply session death
- execution correctness must survive unstable networks
- authority must be explicit and epoch-bounded
- stale packets must be rejected by construction
- recovery must preserve canonical execution truth

This repository defines the canonical specification direction for these concepts.

Unauthorized reproduction of the protocol design without attribution is discouraged.

---

# Access and Review Policy

This repository may be shared with selected reviewers for protocol evaluation.

Review access does not imply permission to copy, redistribute, commercialize, or rebrand the design.

External reviewers may inspect the architecture, threat model, and behavioral specifications for evaluation purposes only.

Silence or inactivity may result in access removal.

---

# Rights Notice

This repository is provided for review, research, and testing purposes only.

All rights reserved.

No copying, redistribution, sublicensing, resale, commercial use, or derivative protocol implementation is permitted without explicit written permission from the author.

---

# Status

```text
Specification status: active research
Implementation status: evolving
Security status: experimental
Production status: not production-ready
```

VRP is a living protocol specification.

Any change to cryptography, authority, session lifecycle, route movement, replay behavior, or recovery semantics must be evaluated against the threat model.