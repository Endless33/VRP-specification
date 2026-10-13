# VRP Security Model

Status: Release Candidate (v1.0.0-rc1)

## 1. Purpose

This document defines the security objectives and expected security
behaviour of Veil Routing Protocol (VRP).

It describes security requirements at the protocol boundary without
exposing private implementation details.

## 2. Security Objectives

A VRP implementation should protect:

- canonical session identity;
- authorized state transitions;
- session continuity during transport changes;
- replay protection;
- canonical state consistency;
- integrity and verifiability of protocol evidence.

## 3. Threat Model

The protocol design considers environments in which an attacker may
attempt to:

- replay previously observed messages;
- submit stale or unauthorized transitions;
- exploit duplicated, delayed or reordered traffic;
- interfere with transport availability;
- trigger conflicting state transitions;
- modify evidence presented for verification.

This list defines relevant threat categories, not a claim that every
possible attack is prevented by every implementation.

## 4. Trust Boundaries

Implementations should distinguish:

- untrusted network input;
- transport and relay observations;
- authorization decisions;
- canonical session state;
- evidence supplied for verification.

Untrusted input must not directly mutate canonical state without
the required validation and authorization.

## 5. Authorization

A state transition must satisfy the applicable authorization rules.

If authorization cannot be established, the transition must be
rejected.

Transport availability alone does not grant authority to change
canonical session state.

## 6. Replay and Stale Input

Previously accepted or otherwise invalidated messages must not
cause an unauthorized state transition.

Rejected replayed or stale input must not mutate canonical state.

Implementations must apply replay checks consistently with their
session and authority model.

## 7. Transport Failure

Transport interruption is an availability event.

It does not, by itself, authorize a new session identity, an authority
transfer or an otherwise invalid state transition.

Implementations must distinguish temporary loss of connectivity
from canonical session termination.

## 8. Fail-Closed Behaviour

When a security-critical transition cannot be validated safely,
the implementation must reject the transition rather than infer
authorization from incomplete or ambiguous information.

Canonical state must not be partially committed.

## 9. Evidence Integrity

Evidence used to support a security claim must be protected against
unauthorized modification and independently checked according to
the applicable verification rules.

A report or log entry alone is not proof that a security property
was satisfied.

## 10. Implementation Responsibility

Security depends on the complete implementation, including protocol
validation, cryptographic mechanisms where required, key management,
transport integration and operational configuration.

This document does not claim that the protocol is immune to all attacks
or that every implementation automatically satisfies these objectives.

## 11. Compliance

A compliance assessment must identify:

- the implementation and version tested;
- the security requirements assessed;
- the test procedures and their results;
- known limitations and untested assumptions.

Security claims must be limited to the properties supported by
the available implementation and verification evidence.

## 12. Related Documents

- Protocol: Session Lifecycle
- Protocol: Authority Model
- Protocol: Transport Independence
- Protocol: Replay Protection
- Protocol: Canonical State Machine
- Runtime: Runtime Pipeline
- Runtime: Runtime Failover
- Runtime: Runtime Migration
- Runtime: Runtime Recovery

