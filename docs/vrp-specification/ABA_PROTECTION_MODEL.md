# ABA Protection Model

Document version: Public v1

## Purpose

Prevent stale lifecycle resurrection.

---

VRP protects two independent ABA classes.

## Same Manager

(SessionID)

↓

Lifetime A

↓

Remove

↓

Lifetime B

Old events are rejected.

---

## Restart

Manager A

↓

Manager B

↓

same SessionID

↓

different LifetimeEpoch

Old events are rejected.

---

## Validation

Canonical identity

(LifetimeEpoch, Incarnation)

must match exactly.