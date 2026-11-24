# VRP Public Qualification Matrix

**Document class:** Public engineering qualification register
**Evaluation model:** Black-box
**Scope:** Evidence explicitly admitted into this publication package

## 1. Purpose

This matrix distinguishes recorded observations from properties
that have not yet been independently established.

A test scenario, proposed acceptance criterion or historical
statement is not automatically a qualification result.

## 2. Current admitted observations

| Scenario | Observed result | Qualification |
|---|---|---|
| Baseline | 8 successful send operations | OBSERVED |
| Partial suppression | 3 injected events, 5 successful sends | OBSERVED |
| Full suppression | 8 injected events, 0 successful sends | OBSERVED |
| Full suppression receive activity | 8 successful receive operations | OBSERVED |

These measurements belong to the recorded E6T-F experiment.
They are not universal performance guarantees.

## 3. Property qualification status

| Property | Current status |
|---|---|
| Send suppression observation | OBSERVED |
| Receive operation observation | OBSERVED |
| Application-level delivery | NOT_PROVEN |
| Canonical session identity preservation | NOT_PROVEN |
| Authority integrity | NOT_PROVEN |
| Complete transport continuity | NOT_PROVEN |
| Compound-fault recovery correctness | NOT_PROVEN |

The incomplete trace attribution limits the conclusions
that can be drawn from the experiment.

## 4. Additional qualification scenarios

The following scenarios require separately attributable execution
records before a positive qualification can be issued:

| Scenario | Publication status |
|---|---|
| Repeated endpoint migration | NOT_REQUALIFIED |
| 30% packet loss | NOT_REQUALIFIED |
| 50% packet loss | NOT_REQUALIFIED |
| 80% packet loss | NOT_REQUALIFIED |
| 90% packet loss | NOT_REQUALIFIED |
| 10-second blackout | NOT_REQUALIFIED |
| 30-second blackout | NOT_REQUALIFIED |
| 60-second blackout | NOT_REQUALIFIED |
| High-latency and jitter conditions | NOT_REQUALIFIED |
| Reordering under migration | NOT_REQUALIFIED |
| Replay during migration | NOT_REQUALIFIED |
| Competing authority recovery | NOT_REQUALIFIED |
| Compound network disruption | NOT_REQUALIFIED |

NOT_REQUALIFIED means that this publication package does not
contain sufficient independently attributed evidence to issue
a positive qualification.

It does not assert that the underlying capability is absent.

## 5. Qualification rules

A property may be marked PASS only when:

1. The relevant scenario was actually executed.
2. The runtime identity is attributable.
3. Required observations were collected.
4. Expected postconditions were independently checked.
5. Evidence integrity was verified.
6. No unresolved contradiction invalidates the result.

Otherwise the status must remain NOT_PROVEN, NOT_RUN or INVALID,
as applicable.

## 6. Intellectual property boundary

This matrix contains no proprietary source code, internal algorithms,
private implementation interfaces, credentials or execution secrets.

It describes externally observable qualification status only.

## 7. Conclusion

VRP qualification is evidence-driven.

Observed measurements, independently verified properties and
unqualified claims must remain separate.

No property should be advertised as proven beyond the scope
of its retained evidence.

**No hype. Read the specifications. Examine the evidence.
Challenge the engineering.**
