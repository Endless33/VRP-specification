# VRP Protocol Overview

This document defines the high-level lifecycle and roles of the Veil Routing Protocol (VRP).
It provides a single reference for trusted operators and reviewers.

---

## Client Lifecycle

The VRP client progresses through the following stages:

1. **INIT**
   - Initialize local entropy sources
   - Prepare session context
   - Validate node availability

2. **ROUTE_CONSTRUCTION**
   - Select initial route from available relay nodes
   - Avoid recent or reused paths
   - Build ephemeral routing context

3. **KEY_EXCHANGE**
   - Negotiate ephemeral keys along the route
   - Establish forward secrecy
   - Validate node participation

4. **ACTIVE_SESSION**
   - Normal data transmission
   - Continuous route monitoring
   - Periodic mutation cycles

5. **ROUTE_MUTATION**
   - Update route according to movement policy
   - Re-negotiate keys as needed
   - Destroy old route state

6. **ANOMALY_RESPONSE**
   - Triggered by detected anomalies (MITM, traffic manipulation, DDoS patterns)
   - Immediate route change or temporary isolation
   - Prioritize session survivability

7. **TERMINATION**
   - Session teardown
   - Destroy ephemeral keys and routing state
   - No persistent session data remains

---

## Node Roles

| Role        | Responsibilities | Trust Assumptions |
|------------|-----------------|-----------------|
| Entry Relay | Accept initial packets, forward to blind ring | Cannot reconstruct full path |
| Blind Node  | Forward packets without awareness of position | Collusion assumed possible, no global knowledge |
| Veil Exit  | Deliver packets to target network | Cannot identify client, ephemeral usage |
| Client     | Full session control, route selection, anomaly detection | Trusted local device |

---

## Route Mutation Policy

- Routes never remain static beyond defined time/packet limits
- Mutation may be periodic or triggered by anomaly
- Keys are refreshed per route change
- Recent routes are avoided to maximize unpredictability

---

## Guardians

The VRP protocol integrates conceptual behavioral models:

- **Zilo** — observer, monitors network state
- **Zila** — shield, protective policies
- **Zippi** — motion, unpredictable adjustments

These entities guide route mutation and anomaly response.
They are **behavioral interfaces**, not software modules.

---

## Compliance Rules

Any implementation must:

- Maintain client authority over routing
- Prevent nodes from obtaining full path information
-