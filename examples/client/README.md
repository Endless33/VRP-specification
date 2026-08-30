# Client Integration Example

## Purpose

This example illustrates the conceptual integration of the VRP Runtime into a client application.

The objective is to demonstrate how a client application maintains execution continuity while communication infrastructure changes.

This document describes architectural behavior only.

Implementation-specific APIs remain outside the scope of the public specification.

---

# Architecture

```
              User Interface
                     │
                     ▼
            Client Application
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
        Available Communication Paths
```

The application communicates exclusively through the Runtime API.

---

# Engineering Goal

The client application should remain responsible for:

- user interaction
- business workflows
- local application state
- rendering
- persistence

The runtime remains responsible for:

- Logical Session continuity
- authority progression
- transport evolution
- replay protection
- recovery
- engineering evidence

---

# Conceptual Lifecycle

```
Application Launch

↓

Runtime Initialization

↓

Logical Session Created

↓

User Activity

↓

Transport Changes

↓

Runtime Preserves Session

↓

Application Continues

↓

Application Shutdown

↓

Runtime Shutdown
```

---

# Conceptual Pseudocode

```text
runtime := CreateRuntime()

runtime.Initialize()

session := runtime.CreateSession()

while applicationRunning {

    event := WaitForUserInput()

    ProcessApplicationLogic(event)

}

runtime.Shutdown()
```

The pseudocode illustrates architectural interaction only.

---

# Runtime Responsibilities

The runtime manages:

- session lifecycle
- transport abstraction
- authority evolution
- deterministic recovery
- replay rejection
- engineering evidence

Applications do not implement these mechanisms.

---

# Application Responsibilities

The client application manages:

- user experience
- presentation
- business logic
- local preferences
- domain-specific processing

Business logic remains independent from transport behavior.

---

# Runtime Events

Representative observable events include:

- RuntimeReady
- SessionCreated
- SessionActivated
- AuthorityConfirmed
- TransportChanged
- RecoveryStarted
- RecoveryCompleted
- ReplayRejected
- RuntimeStopped

Applications observe events.

The runtime remains authoritative.

---

# Transport Evolution

Observable transport changes may include:

- Wi-Fi → LTE
- LTE → Wi-Fi
- roaming
- network interruption
- temporary degradation

The application continues interacting with the same Logical Session whenever architectural correctness permits.

---

# Failure Handling

During observable failures:

- temporary connectivity loss
- infrastructure restart
- degraded communication
- packet loss

The runtime evaluates recovery.

The application remains independent from recovery implementation.

---

# Recovery

Recovery belongs exclusively to the runtime.

Applications receive observable recovery notifications.

Applications should never implement recovery policy.

---

# Engineering Validation

Independent validation should verify:

- Logical Session continuity
- deterministic runtime behavior
- authority preservation
- replay rejection
- engineering evidence

Evaluation is based upon observable behavior.

---

# Related Documents

- docs/integration/EMBEDDING.md
- docs/integration/API.md
- docs/runtime/STATE_MACHINE.md
- docs/runtime/RECOVERY_RULES.md

---

## Design Principles

- Clients interact with users.
- The runtime preserves continuity.
- Applications remain transport-independent.
- Recovery belongs to the runtime.
- Observable behavior supports independent engineering validation.