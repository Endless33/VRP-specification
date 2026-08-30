# ADR-0006 — Why VRP is Not a VPN

**Status:** Accepted

**ADR Number:** 0006

**Date:** 2026

**Category:** Product Architecture

---

# Context

During early engineering discussions, VRP was frequently described as a VPN.

The comparison was understandable.

Both technologies participate in network communication.

Both may coexist within the same deployment.

However, this description became increasingly inaccurate as the architecture evolved.

The engineering objectives of VRP differ fundamentally from those of traditional VPN technologies.

---

# Problem

Describing VRP as a VPN creates incorrect engineering expectations.

Typical assumptions include:

- encrypted tunnel replacement
- virtual network interface
- IP masking
- privacy-focused architecture
- endpoint tunneling

These assumptions do not describe the architectural purpose of VRP.

They shift technical discussions toward the wrong engineering problems.

---

# Decision

VRP is defined as a **continuity-first runtime architecture**.

Its primary responsibility is preserving deterministic application continuity across changing network conditions.

Encryption, tunneling and transport technologies may exist underneath VRP.

They are not the defining architectural objective.

---

# Decision Drivers

The following engineering objectives motivated this decision.

- Logical Session continuity
- transport independence
- deterministic runtime behavior
- authority consistency
- replay protection
- reproducible validation
- observable engineering evidence

Continuity is the architectural objective.

Tunneling is not.

---

# Alternatives Considered

## Alternative A

Describe VRP as a VPN.

Advantages:

- familiar terminology
- simple explanation

Disadvantages:

- incorrect architectural model
- misleading technical expectations
- unnecessary comparisons with existing VPN products

Rejected.

---

## Alternative B

Describe VRP as a networking framework.

Advantages:

- broader definition

Disadvantages:

- too generic
- insufficiently descriptive

Rejected.

---

## Alternative C

Define VRP as a continuity-first runtime architecture.

Advantages:

- accurately reflects engineering objectives
- consistent with the RFC series
- aligns with runtime behavior
- distinguishes VRP from tunneling products

Accepted.

---

# Consequences

Engineering discussions become focused on:

- continuity
- deterministic execution
- authority evolution
- recovery
- transport abstraction
- runtime validation

The architecture is evaluated according to its intended purpose rather than VPN feature comparisons.

---

# Relationship With VPN Technologies

VRP is compatible with environments where VPN technologies are already deployed.

Examples include:

- WireGuard
- IPsec
- OpenVPN
- enterprise tunnels

VRP neither replaces nor depends upon any specific VPN implementation.

Transport technologies remain interchangeable beneath the runtime.

---

# Benefits

The decision provides:

- clearer architectural positioning
- more accurate engineering evaluation
- improved communication with system architects
- reduced terminology confusion
- better long-term specification consistency

---

# Trade-offs

Additional explanation is required during early technical discussions.

However, that effort prevents long-term architectural misunderstanding.

---

# Architectural Impact

This decision directly supports:

RFC-0001 — Logical Session Identity

RFC-0003 — Transport Abstraction

RFC-0004 — Runtime State Machine

RFC-0007 — Failure Recovery

RFC-0008 — Multipath Selection

The runtime architecture is intentionally independent of any individual transport technology.

---

# Validation

Engineering evaluation should answer questions such as:

- Does the Logical Session survive transport evolution?
- Is authority preserved?
- Is execution deterministic?
- Is recovery reproducible?
- Are architectural invariants maintained?

These questions define VRP more accurately than VPN feature comparisons.

---

# Status

Accepted.

This architectural positioning is expected to remain stable across future versions of the specification.

---

# Design Statement

A VPN protects communication.

VRP preserves execution continuity.

Those objectives may complement one another.

They are not the same architecture.