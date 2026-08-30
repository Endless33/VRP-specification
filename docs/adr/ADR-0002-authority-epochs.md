# ADR-0002 — Authority Epochs

**Status:** Accepted

**ADR Number:** 0002

**Date:** 2026

**Category:** Core Architecture

---

# Context

Distributed runtimes continuously experience changes in execution authority.

Examples include:

- runtime restart
- failover
- infrastructure migration
- transport evolution
- temporary network partition
- recovery after failure

Without a deterministic ownership model, multiple runtime instances may simultaneously believe they are responsible for the same Logical Session.

Such situations create inconsistent observable behavior.

---

# Problem

Traditional ownership mechanisms often rely on temporary assumptions.

Examples include:

- current network connectivity
- transport persistence
- leader heartbeat timing
- infrastructure availability

These assumptions become unreliable during mobility or partial failures.

The runtime requires an ownership model that remains deterministic despite changing infrastructure.

---

# Decision

VRP introduces **Authority Epochs**.

Authority is represented as an observable monotonic progression.

Every Logical Session has exactly one canonical authority.

Authority changes only through observable epoch evolution.

Historical authority never automatically becomes canonical again.

---

# Decision Drivers

The decision was driven by the following engineering goals:

- deterministic ownership
- replay resistance
- split-brain prevention
- reproducible recovery
- observable validation
- engineering simplicity
- transport independence

Authority must evolve independently from transport.

---

# Alternatives Considered

## Alternative A

Leader based on transport availability.

Advantages:

- simple implementation

Disadvantages:

- transport-dependent ownership
- unstable during mobility
- difficult recovery

Rejected.

---

## Alternative B

Time-based ownership.

Advantages:

- straightforward implementation

Disadvantages:

- clock dependency
- partition sensitivity
- inconsistent authority decisions

Rejected.

---

## Alternative C

Distributed voting for every authority transition.

Advantages:

- explicit coordination

Disadvantages:

- increased operational complexity
- additional latency
- higher implementation cost

Rejected for the current architecture.

---

## Alternative D

Monotonic Authority Epochs.

Advantages:

- deterministic progression
- observable ownership
- replay compatibility
- reproducible validation
- transport independence

Accepted.

---

# Consequences

Authority becomes an architectural concept rather than a transport property.

Observable runtime behavior becomes easier to validate.

Historical ownership can no longer silently reappear after infrastructure recovery.

Engineering evidence becomes easier to interpret.

---

# Benefits

The decision improves:

- authority consistency
- deterministic execution
- replay protection
- recovery correctness
- engineering reproducibility
- observable behavior

---

# Trade-offs

The runtime must maintain explicit authority progression.

Additional coordination exists inside the protected runtime.

Applications are isolated from this complexity.

---

# Architectural Impact

This decision directly affects:

RFC-0002 — Authority Epochs

RFC-0004 — Runtime State Machine

RFC-0005 — Evidence Model

RFC-0006 — Replay Protection

RFC-0007 — Failure Recovery

RFC-0008 — Multipath Selection

Authority progression is considered a permanent architectural invariant.

---

# Validation

Observable validation includes:

- stale authority rejection
- authority takeover
- monotonic epoch progression
- deterministic recovery
- split-brain prevention

Engineering validation is based on observable runtime behavior.

Implementation remains protected.

---

# Status

Accepted.

Future implementations may improve internal algorithms.

Observable Authority Epoch semantics remain stable.

---

# Design Statement

Authority is not ownership of infrastructure.

Authority is ownership of execution.

Execution progresses forward.

Authority progresses with it.

Historical authority remains history.