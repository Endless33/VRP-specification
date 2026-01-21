# VRP State Machine

This document defines the high-level state machine of the Veil Routing Protocol (VRP).
It describes client session states and allowed transitions.
This is a logical model, independent of implementation language.

---

## Overview

A VRP session is modeled as a finite-state machine.
State transitions are driven by:
- time
- network signals
- anomaly detection
- policy decisions

The client is the authoritative state controller.

---

## States

### INIT
Initial state.
The client initializes local context, entropy sources,
and prepares for route construction.

---

### ROUTE_CONSTRUCTION
The client selects an initial route using available relay nodes.
No route is reused from previous sessions.

Transition to: KEY_EXCHANGE

---

### KEY_EXCHANGE
Ephemeral keys are negotiated along the route.
Forward secrecy is established.

Transition to: ACTIVE_SESSION

---

### ACTIVE_SESSION
Normal data transmission state.
Traffic flows through the current route.
Continuous monitoring is active.

Transitions:
- to ROUTE_MUTATION
- to ANOMALY_RESPONSE
- to TERMINATION

---

### ROUTE_MUTATION
The client updates the route according to:
- periodic rotation policy
- entropy requirements

Keys are refreshed as needed.

Transition to: ACTIVE_SESSION

---

### ANOMALY_RESPONSE
Triggered by detected anomalies:
- suspected MITM
- traffic manipulation
- abnormal latency patterns

Actions may include:
- immediate route replacement
- key refresh
- temporary isolation

Transition to:
- ROUTE_MUTATION
- TERMINATION

---

### TERMINATION
Session teardown.
All ephemeral state is destroyed.
No session data is retained.

Final state.

---

## State Transition Summary

INIT
 → ROUTE_CONSTRUCTION
 → KEY_EXCHANGE
 → ACTIVE_SESSION
 ↔ ROUTE_MUTATION
 ↔ ANOMALY_RESPONSE
 → TERMINATION

---

## Invariant Enforcement

At no point may:
- a route remain static beyond defined limits
- a node gain full path visibility
- persistent identifiers be introduced

Violations invalidate VRP compliance.

---

End of state machine.