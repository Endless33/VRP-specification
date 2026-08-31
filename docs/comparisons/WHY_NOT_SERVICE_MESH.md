# Why Not Service Mesh?

## Purpose

This document explains why VRP should not be viewed as a replacement for a Service Mesh and why the two technologies address different engineering problems.

It is intended for architectural evaluation.

It does not disclose implementation details of the protected VRP runtime.

---

# Different Design Goals

A Service Mesh manages communication between distributed services inside an application platform.

VRP investigates continuity of logical runtime state while transport conditions change.

Although both influence distributed communication, they operate at different architectural layers.

---

# Service Mesh Focus

A Service Mesh primarily addresses:

- service-to-service communication
- traffic routing
- service discovery
- mutual TLS
- retries
- load balancing
- observability
- policy enforcement

Its objective is reliable communication between software services.

---

# VRP Focus

VRP investigates a different engineering problem.

Core principle:

Session ≠ Transport

The objective is preserving logical session continuity while transport conditions change.

Transport becomes replaceable infrastructure rather than the identity of the runtime.

---

# Different Questions

A Service Mesh asks:

"How should distributed services communicate?"

VRP asks:

"Should the logical session continue even if transport changes?"

These questions belong to different architectural layers.

---

# Runtime Continuity

VRP investigates runtime behavior during:

- IP migration
- network failover
- Wi-Fi ↔ Cellular transitions
- roaming
- NAT and CGNAT rebinding
- temporary communication loss
- replay attempts
- stale authority injection
- duplicate execution attempts

The objective is preserving deterministic runtime behavior rather than managing service traffic.

---

# Observable Evaluation

Engineering teams may evaluate:

- logical session continuity
- deterministic runtime decisions
- replay rejection
- stale authority rejection
- duplicate execution protection
- recovery behavior
- evidence generation
- independent verification

These properties exist independently of any Service Mesh implementation.

---

# Complementary Technologies

VRP and a Service Mesh should not necessarily be viewed as competing technologies.

A Service Mesh may manage communication between services while a continuity-oriented runtime independently preserves logical session behavior across changing transport conditions.

The public specification intentionally avoids requiring any specific application platform.

---

# Evaluation Boundary

This document discusses architectural concepts only.

It does not describe:

- protected runtime implementation
- transport scoring mechanisms
- synchronization algorithms
- proprietary runtime logic
- implementation heuristics

---

# Non-Goals

This document does not claim:

- that Service Mesh platforms are insufficient
- that Service Mesh should be replaced
- that VRP replaces Kubernetes networking
- that VRP replaces microservice infrastructure

VRP investigates a different architectural problem.

---

# Summary

Service Mesh platforms focus on communication between distributed application services.

VRP explores continuity of logical runtime state across changing transport conditions while maintaining deterministic runtime decisions and independently verifiable engineering evidence.

The two approaches may complement one another because they solve different engineering problems at different layers of distributed systems.