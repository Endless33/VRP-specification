# Server Integration Example

## Purpose

This example illustrates the conceptual integration of the VRP Runtime into a server application.

The objective is to demonstrate how server software delegates continuity responsibilities to the runtime while retaining ownership of application logic.

This example describes architecture rather than implementation.

---

# Architecture

```
                Client Applications
                        │
                        ▼
               Application Server
                        │
                        ▼
                  Runtime API
                        │
                        ▼
              Protected VRP Runtime
                        │
                        ▼
             Available Transports
                        │
                        ▼
                 Network Infrastructure
```

---

# Engineering Goal

The server should remain focused on:

- request processing
- business logic
- authentication
- authorization
- storage
- application services

The runtime manages:

- Logical Session continuity
- authority
- transport evolution
- recovery
- replay protection
- engineering evidence

---

# Conceptual Lifecycle

```
Server Starts

↓

Runtime Created

↓

Runtime Initialized

↓

Server Accepts Requests

↓

Logical Sessions Created

↓

Requests Processed

↓

Transport Changes

↓

Runtime Preserves Continuity

↓

Server Continues Processing

↓

Graceful Shutdown
```

---

# Conceptual Pseudocode

```text
runtime := CreateRuntime()

runtime.Initialize()

server := StartServer(runtime)

while serverRunning {

    request := WaitForRequest()

    HandleRequest(request)

}

runtime.Shutdown()
```

The pseudocode illustrates architectural interaction only.

---

# Runtime Responsibilities

The runtime owns:

- session lifecycle
- authority progression
- recovery execution
- transport abstraction
- replay rejection
- evidence generation

The server should never reimplement these responsibilities.

---

# Application Responsibilities

The server owns:

- business rules
- request validation
- application state
- persistence
- service orchestration
- response generation

Business logic remains independent from transport behavior.

---

# Runtime Events

Representative events include:

- RuntimeReady
- SessionCreated
- SessionActivated
- AuthorityChanged
- TransportChanged
- RecoveryStarted
- RecoveryCompleted
- ReplayRejected
- RuntimeStopping

Applications observe events.

The runtime remains authoritative.

---

# Failure Handling

During observable failures:

- transport interruption
- infrastructure restart
- temporary outage
- degraded connectivity

The runtime evaluates continuity.

The server continues processing whenever architectural correctness permits.

---

# Recovery

Recovery belongs exclusively to the runtime.

Applications should receive recovery notifications.

Applications should not implement recovery algorithms.

---

# Engineering Validation

Validation should demonstrate:

- uninterrupted Logical Session identity
- deterministic runtime behavior
- replay rejection
- authority preservation
- observable engineering evidence

Validation focuses on observable behavior.

---

# Related Documents

- docs/integration/EMBEDDING.md
- docs/integration/API.md
- docs/runtime/RECOVERY_RULES.md
- docs/evaluation/TEST_MATRIX.md

---

## Design Principles

- Servers process business logic.
- The runtime preserves continuity.
- Applications remain transport-independent.
- Recovery belongs to the runtime.
- Observable behavior enables independent validation.