# VRP Invariants

This document defines the non-negotiable invariants of the Veil Routing Protocol (VRP).
Any compliant implementation MUST preserve these properties.
If an invariant is violated, the system is no longer VRP.

---

## Invariant 1: Continuous Route Movement

VRP routes MUST NOT remain static.
All client sessions are defined by continuous or periodic route mutation.

Static routes longer than a bounded interval are considered a protocol violation.

Motion is a security property, not an optimization.

---

## Invariant 2: Client-Controlled Behavior

The client is the authoritative entity.
The network reacts to the client, not the other way around.

Servers, relays, and nodes MUST NOT impose fixed routing behavior
without explicit client consent.

---

## Invariant 3: No Node Has Full Path Knowledge

No single node may know:
- the full route
- the route history
- the future route plan

Blind-node logic is mandatory.
Any design allowing full-path visibility breaks VRP.

---

## Invariant 4: Unpredictability Over Efficiency

VRP prioritizes unpredictability over:
- performance
- latency
- bandwidth optimization

Predictability is treated as a vulnerability.
Efficiency is explicitly a secondary concern.

---

## Invariant 5: No Persistent Metadata

VRP MUST NOT generate persistent metadata such as:
- session identifiers
- stable connection fingerprints
- route histories
- long-lived correlation markers

If data does not exist, it cannot be leaked, requested, or reconstructed.

---

## Invariant 6: Adaptive Reaction to Anomalies

The protocol MUST support adaptive behavior in response to anomalies,
including but not limited to:
- suspected MITM
- traffic manipulation
- degradation patterns
- behavioral inconsistencies

Failure to adapt is considered a survivability failure.

---

## Invariant 7: Specification First

VRP is specification-driven.
Implementations MUST follow the specification,
not define it retroactively.

If implementation behavior conflicts with the spec,
the implementation is wrong.

---

End of invariants.