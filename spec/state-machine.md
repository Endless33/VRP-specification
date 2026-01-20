# VRP Session State Machine

## 1. Overview

This document defines the **session lifecycle and state transitions** for the Veil Routing Protocol (VRP).

The state machine ensures that:
- cryptographic material has a defined lifetime
- route movement is explicit and controlled
- error handling is deterministic

---

## 2. Session States

INIT ↓ HANDSHAKE ↓ ACTIVE ↓ ROTATING ↓ TEARDOWN

---

## 3. State Descriptions

### INIT
- Client initializes configuration parameters
- Ephemeral key material is generated
- No network traffic is sent

---

### HANDSHAKE
- Secure handshakes are performed with selected relay nodes
- Ephemeral session keys are negotiated
- Failure in this state aborts session creation

---

### ACTIVE
- Encrypted traffic flows through the established route
- Client continuously monitors:
  - latency
  - packet integrity
  - jitter and anomalies
- Periodic timers for route rotation are active

---

### ROTATING
- Triggered by:
  - anomaly detection
  - periodic rotation timer
- New route is constructed in parallel
- Old route remains active until new route is ready
- Old session keys are destroyed immediately after switch

---

### TEARDOWN
- Session termination is initiated
- All cryptographic material is securely erased
- Network resources are released
- No state persists beyond this point

---

## 4. State Transition Triggers

| From State | To State | Trigger |
|----------|----------|--------|
| INIT | HANDSHAKE | Session start |
| HANDSHAKE | ACTIVE | Successful handshake |
| ACTIVE | ROTATING | Anomaly detected |
| ACTIVE | ROTATING | Periodic rotation timer |
| ROTATING | ACTIVE | New route established |
| ANY | TEARDOWN | Fatal error or shutdown |

---

## 5. Security Properties

- Session keys exist only in ACTIVE or ROTATING states
- Key material is destroyed on every route change
- No state allows reuse of old cryptographic keys
- Route movement is mandatory, not optional

---

## 6. Summary

The VRP state machine ensures **bounded exposure, deterministic behavior, and safe cryptographic lifecycle management** across all sessions.