# Minimal Integration Example

## Purpose

This example demonstrates the smallest conceptual integration of the VRP Runtime into an application.

The example intentionally omits implementation-specific APIs.

Its objective is to explain integration architecture.

---

# Architecture

```
Application

      │

Runtime API

      │

Protected Runtime

      │

Transport
```

---

# Application Flow

```
Application Starts

↓

Runtime Created

↓

Runtime Initialized

↓

Logical Session Created

↓

Application Executes

↓

Transport Changes

↓

Runtime Preserves Session

↓

Application Continues

↓

Runtime Stops
```

---

# Integration Responsibilities

Application:

- initialize runtime
- create Logical Session
- consume callbacks
- process business logic

Runtime:

- preserve continuity
- manage authority
- manage recovery
- reject replay
- generate engineering evidence

---

# Conceptual Pseudocode

```text
runtime := CreateRuntime()

runtime.Initialize()

session := runtime.CreateSession()

while applicationRunning {

    ProcessBusinessLogic()

}

runtime.Shutdown()
```

The pseudocode illustrates architectural interaction only.

It is not language-specific.

---

# Observable Runtime Events

Typical events include:

- RuntimeReady
- SessionCreated
- AuthorityEstablished
- TransportChanged
- RecoveryCompleted
- RuntimeStopped

Applications observe events.

The runtime remains authoritative.

---

# Expected Behavior

The application never manages:

- transport migration
- authority
- replay
- recovery

Those responsibilities belong exclusively to the Protected Runtime.

---

# Engineering Goal

The smallest possible integration should still preserve:

- Logical Session
- Canonical Authority
- Deterministic Runtime Decisions
- Replay Protection
- Engineering Evidence

---

# Related Documents

- docs/integration/EMBEDDING.md
- docs/integration/API.md
- docs/runtime/INVARIANTS.md

---

## Design Principle

Applications solve business problems.

The runtime solves continuity.