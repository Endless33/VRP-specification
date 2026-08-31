# VRP vs Existing Approaches

## Purpose

This document explains how VRP relates to existing networking technologies.

Its purpose is not to rank technologies, but to clarify the engineering problem each technology is designed to solve.

It is intended for architectural evaluation only.

It does not disclose implementation details of the protected VRP runtime.

---

# Overview

Modern distributed systems rely on multiple networking technologies.

Each technology addresses a different engineering objective.

VRP investigates one specific question:

Can a logical runtime session remain valid while the underlying transport changes?

---

# Engineering Scope

VRP should not be evaluated as:

- another VPN
- another transport protocol
- another overlay network
- another SD-WAN platform
- another service mesh

VRP investigates continuity of logical runtime state.

---

# Architectural Principle

Core principle:

Session ≠ Transport

Transport is infrastructure.

The session is runtime state.

Changing one should not necessarily invalidate the other.

---

# Technology Comparison

| Technology | Primary Goal | VRP Relationship |
|------------|--------------|------------------|
| TCP | Reliable transport | Different layer |
| QUIC | Modern encrypted transport | Complementary |
| MPTCP | Multipath transport | Complementary |
| WireGuard | Secure VPN tunnel | Complementary |
| OpenVPN | Secure VPN connectivity | Complementary |
| SD-WAN | Network infrastructure management | Complementary |
| Service Mesh | Service-to-service communication | Complementary |
| VRP | Logical session continuity | Runtime architecture |

---

# What TCP Solves

TCP focuses on:

- reliable delivery
- ordered packets
- congestion control
- retransmission

TCP answers:

"How should packets be delivered?"

VRP investigates a different question.

---

# What QUIC Solves

QUIC focuses on:

- encrypted transport
- reduced handshake latency
- multiplexing
- transport efficiency

VRP focuses on continuity of logical runtime state.

---

# What MPTCP Solves

Multipath TCP focuses on:

- multiple transport paths
- bandwidth aggregation
- transport redundancy

VRP focuses on continuity independent of transport.

---

# What VPN Protocols Solve

WireGuard and OpenVPN primarily provide:

- encrypted tunnels
- secure communication
- authenticated endpoints

VRP investigates whether logical runtime state should continue while transport changes.

---

# What SD-WAN Solves

SD-WAN focuses on:

- WAN optimization
- path selection
- centralized network management
- traffic steering

VRP focuses on runtime continuity rather than infrastructure management.

---

# What Service Mesh Solves

Service Mesh focuses on:

- service discovery
- retries
- load balancing
- observability
- service communication

VRP focuses on preserving logical runtime state across transport changes.

---

# Observable Properties

VRP investigates observable runtime behavior such as:

- logical session continuity
- deterministic runtime decisions
- replay rejection
- stale authority rejection
- duplicate execution protection
- authority consistency
- recovery behavior
- evidence generation
- independent verification

These properties exist independently of any specific transport technology.

---

# Complementary Architecture

Existing technologies and VRP may coexist.

For example:

Application

↓

VRP Runtime

↓

WireGuard / QUIC / TCP / UDP

↓

Network Infrastructure

Each layer addresses a different engineering responsibility.

---

# Evaluation Boundary

The public specification documents:

- architecture
- protocol semantics
- runtime concepts
- engineering boundaries
- observable behavior

The following remain outside the public boundary:

- protected runtime implementation
- proprietary algorithms
- synchronization mechanisms
- transport scoring
- implementation heuristics

---

# Non-Goals

This document does not claim:

- replacement of existing networking technologies
- universal superiority
- elimination of transport failures
- elimination of infrastructure failures
- guaranteed performance improvements

Engineering conclusions should be based on reproducible evaluation.

---

# Summary

Modern networking technologies solve different engineering problems.

VRP investigates continuity of logical runtime state while transport conditions evolve.

Rather than replacing existing technologies, VRP explores a complementary architectural layer focused on deterministic runtime behavior, observable continuity and independently verifiable engineering evidence.