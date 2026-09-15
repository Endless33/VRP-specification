# Event Lifetime Binding

Document version: Public v1

## Purpose

Every trusted event is explicitly bound to the current canonical session lifetime.

---

Binding attaches

LifetimeEpoch

and

Incarnation

before event delivery.

---

Trusted Producers

- Runtime
- Control Plane

---

Validation

SessionManager validates

(LifetimeEpoch, Incarnation)

before:

- event log
- state transition
- history mutation

Rejected events never become canonical history.