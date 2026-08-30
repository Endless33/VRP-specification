# IoT Integration Example

## Purpose

This example illustrates the conceptual integration of the Veil Routing Protocol (VRP) Runtime into an Internet of Things (IoT) device.

The objective is to demonstrate how IoT applications preserve execution continuity despite unstable communication environments.

This document describes architectural concepts only.

Implementation-specific APIs remain outside the scope of the public specification.

---

# Architecture

```
              Sensor / Device

                     │

                     ▼

             Device Application

                     │

                     ▼

                Runtime API

                     │

                     ▼

          Protected VRP Runtime

                     │

                     ▼

          Transport Abstraction

                     │

                     ▼

      Wi-Fi / LTE / Ethernet / Satellite

                     │

                     ▼

             Backend Infrastructure
```

Applications communicate through the Runtime API.

The runtime communicates through available transports.

---

# Engineering Goal

IoT devices should remain responsible for:

- sensor acquisition
- actuator control
- local decision making
- device-specific logic
- application persistence

The runtime remains responsible for:

- Logical Session continuity
- authority progression
- transport evolution
- replay protection
- recovery
- engineering evidence

---

# Typical IoT Environment

Representative environments include:

- industrial sensors
- environmental monitoring
- energy systems
- logistics devices
- smart city infrastructure
- agricultural systems
- robotics
- telemetry equipment

The architecture remains independent of deployment domain.

---

# Conceptual Lifecycle

```
Device Boot

↓

Runtime Initialization

↓

Logical Session Created

↓

Sensor Data Collection

↓

Data Exchange

↓

Transport Changes

↓

Runtime Preserves Session

↓

Device Continues Operating

↓

Graceful Shutdown
```

---

# Conceptual Pseudocode

```text
runtime := CreateRuntime()

runtime.Initialize()

session := runtime.CreateSession()

while deviceRunning {

    sample := ReadSensors()

    ProcessApplicationLogic(sample)

}

runtime.Shutdown()
```

The pseudocode illustrates architectural interaction only.

---

# Runtime Responsibilities

The runtime manages:

- session lifecycle
- authority evolution
- transport abstraction
- recovery
- replay rejection
- deterministic execution
- engineering evidence

Applications remain independent from these mechanisms.

---

# Application Responsibilities

The IoT application manages:

- sensor interaction
- control logic
- local processing
- business rules
- device-specific functionality

Application behavior remains independent from communication infrastructure.

---

# Transport Evolution

Representative transport changes include:

- Wi-Fi unavailable
- LTE activated
- Ethernet restored
- satellite fallback
- intermittent connectivity

The runtime evaluates transport evolution while preserving architectural correctness.

---

# Recovery

Recovery belongs exclusively to the runtime.

Observable recovery may include:

- temporary communication interruption
- infrastructure restart
- transport restoration
- authority revalidation

Applications receive notifications.

Applications do not implement recovery algorithms.

---

# Security

The runtime assumes communication infrastructure is untrusted.

Representative conditions include:

- packet loss
- latency
- replay attempts
- duplicated packets
- temporary outages

Architectural correctness remains protected.

---

# Engineering Validation

Independent evaluation should verify:

- Logical Session continuity
- authority preservation
- deterministic runtime behavior
- replay rejection
- recovery correctness
- engineering evidence

Validation focuses on observable behavior.

---

# Related Documents

- docs/integration/EMBEDDING.md
- docs/integration/TRANSPORTS.md
- docs/runtime/RECOVERY_RULES.md
- docs/security/SECURITY_MODEL.md

---

## Design Principles

- Devices perform domain logic.
- The runtime preserves continuity.
- Applications remain transport-independent.
- Recovery belongs to the runtime.
- Engineering evidence supports independent validation.