# VRP Packet Flow

This document describes the logical flow of packets in the Veil Routing Protocol (VRP).
It outlines how data moves through the system without exposing implementation-specific details.

---

## Overview

VRP packet flow is defined by:
- segmented knowledge
- ephemeral routing context
- continuous movement

Each packet is processed with limited local context.
No node has global awareness.

---

## High-Level Flow

Client
 → Entry Relay
 → Blind Node Ring
 → Veil Exit
 → Target

The return path follows a separately constructed route.

---

## Client Responsibilities

The client:
- constructs the route
- defines mutation policies
- encrypts payloads
- monitors behavior
- triggers route changes

The client is the only entity with session-level awareness.

---

## Relay Node Processing

Each relay node:
- decrypts only its local layer
- forwards packets based on transient instructions
- retains no route history
- does not identify the client or destination

Nodes operate under blind forwarding assumptions.

---

## Blind Node Ring

Blind nodes:
- are unaware of their position in the route
- do not know predecessor or successor roles
- cannot infer route length or topology

This prevents structural reconstruction.

---

## Veil Exit Behavior

The veil exit:
- exposes traffic to the target network
- does not know the client identity
- does not retain session metadata
- may be rotated or replaced dynamically

Veil exits are disposable by design.

---

## Route Mutation Flow

During mutation:
1. Client constructs a new route
2. New keys are negotiated
3. Traffic is gradually or immediately shifted
4. Old route state is destroyed

No overlap guarantees full observability.

---

## Anomaly-Triggered Flow

If anomalies are detected:
- packets may be delayed or dropped
- route mutation is prioritized
- session continuity is preserved if possible

Security overrides performance.

---

## Packet Lifespan

Packets are:
- ephemeral
- unlinkable
- non-identifying

After delivery, no persistent trace remains.

---

## Compliance Requirements

Any implementation MUST ensure:
- no packet enables route reconstruction
- no node can correlate sessions
- packet handling remains stateless beyond necessity

---

End of packet flow.