# Lifetime Security Invariants

Document version: Public v1

## Invariants

1.

Old lifetime never mutates new lifetime.

2.

Restart never reuses canonical identity.

3.

Rejected events never enter canonical event log.

4.

Rejected events never mutate state.

5.

Rejected events never mutate transition history.

6.

Bootstrap authority is trusted.

7.

Generic event delivery cannot create canonical session.

8.

Fail closed by default.

9.

Lifetime identity is independent from

- Authority Generation
- Lease Epoch

10.

Canonical validation remains deterministic.