# VRP Non-Goals

This document defines what the Veil Routing Protocol (VRP) explicitly does NOT attempt to solve.
These are intentional exclusions, not missing features.

Understanding these non-goals is critical for correct evaluation of VRP.

---

## Non-Goal 1: Maximum Anonymity Against Global Adversaries

VRP does NOT claim to defeat a global passive adversary
with unlimited visibility and long-term observation.

VRP focuses on survivability, disruption, and uncertainty,
not absolute anonymity guarantees.

---

## Non-Goal 2: Traffic Mixing or Mass Anonymity Sets

VRP is not a mixnet.
It does not attempt to blend traffic into large anonymity sets
or provide crowd-based anonymity.

Its protection model is based on movement, unpredictability,
and behavioral disruption, not statistical hiding.

---

## Non-Goal 3: Optimal Performance or Lowest Latency

VRP does not optimize for:
- lowest latency
- highest throughput
- minimal overhead

Performance trade-offs are intentional.
Security properties take priority over efficiency.

---

## Non-Goal 4: Compatibility With Existing VPN Protocols

VRP is not designed to be:
- backward-compatible with OpenVPN
- interoperable with WireGuard
- a drop-in replacement for existing VPN stacks

It defines a new routing and behavioral model.

---

## Non-Goal 5: User-Friendly Simplicity

VRP does not prioritize:
- minimal configuration
- one-click usability
- consumer-grade simplicity

It is designed for operators, architects,
and sensitive environments where control matters.

---

## Non-Goal 6: Centralized Trust Models

VRP does not rely on:
- central authorities
- static trust anchors
- long-lived server identities

Any design introducing fixed centralized trust
is considered out of scope.

---

## Non-Goal 7: Legal or Policy Guarantees

VRP does not provide:
- legal anonymity guarantees
- jurisdictional protection claims
- compliance assurances

It is a technical protocol, not a policy framework.

---

End of non-goals.