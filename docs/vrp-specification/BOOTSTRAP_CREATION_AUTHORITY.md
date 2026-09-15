# Bootstrap Creation Authority

Document version: Public v1

## Purpose

Session creation is not a generic event.

Session creation is an authority operation.

---

## Bootstrap Boundary

Only trusted runtime or trusted control plane may create canonical SessionID.

Generic event delivery MUST NOT create new canonical sessions.

---

## Allowed

Trusted bootstrap:

Runtime
↓

CreateSession()

↓

Canonical Session

---

## Forbidden

External event

↓

HandleEvent(create_session)

↓

Canonical Session

This path is forbidden.

---

## Security Goal

Separate

Session Creation

from

Event Delivery

This prevents unauthorized bootstrap.