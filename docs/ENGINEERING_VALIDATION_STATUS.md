# VRP Engineering Validation Status

**Project:** Veil Routing Protocol (VRP)  
**Status:** Public engineering summary  
**Updated:** October 2026

---

## 1. Purpose

This document summarizes what has actually been implemented and exercised in VRP.

It is intentionally narrower than a protocol specification.

It answers four questions:

1. What properties is VRP designed to preserve?
2. Which of those properties have been exercised in the current implementation?
3. What evidence exists for those results?
4. What is not being claimed?

The implementation and the validation surface are separate.

Public validation describes observable behavior and invariants. It does not require publication of the protected runtime source.

---

## 2. Current Status

VRP is an implemented continuity runtime under active engineering validation.

The current implementation has been exercised against:

- transport loss and replacement;
- repeated transport migration;
- stale session-lifetime work;
- replay;
- invalid state transitions;
- authority rollback;
- runtime recovery;
- concurrency;
- prolonged network instability;
- evidence modification.

Current engineering position:

> **The core continuity model is implemented and has passed the validation scenarios described below. Controlled integration evaluation is appropriate. Universal production certification is not claimed.**

---

## 3. Core Invariant

The central VRP invariant is:

> **Session ≠ Transport**

A session is a logical continuity object.

A transport is one mechanism currently carrying traffic for that session.

Therefore a change in:

- network interface;
- path;
- relay;
- endpoint mapping;
- transport instance;

does not, by itself, define a new session.

This is not presented as a novel networking primitive by itself.

The engineering question VRP addresses is broader:

> Can session identity, lifecycle, authority and canonical state remain coherent while transports fail, change, recover, or deliver stale work?

The validation program is built around that question.

---

## 4. What VRP Validates

The current validation surface focuses on five areas:

| Area | Property |
|---|---|
| Continuity | Session identity survives supported transport replacement |
| Lifetime | Historical work cannot silently become current work |
| Authority | Connectivity does not automatically imply authority |
| Canonical state | Rejected operations do not become accepted history |
| Evidence | Validation results can be checked for consistency |

These properties are related, but they are tested separately.

A successful transport migration does not automatically prove replay resistance.

Replay resistance does not automatically prove authority correctness.

Evidence integrity does not automatically prove runtime correctness.

Each claim therefore has its own scope.

---

## 5. Transport Continuity

VRP has been exercised through real and simulated transport disruption.

Validated scenarios include:

- active transport loss;
- replacement transport attachment;
- path migration;
- network rebinding;
- repeated migration;
- transport recovery.

One recorded migration stress run completed:

```text
MIGRATIONS=1000
SESSION_IDENTITY_PRESERVED=YES
```

A separate physical network interruption produced:

```text
TRANSPORT_REACHABILITY=LOST
TRANSPORT_REACHABILITY=RECOVERED
OBSERVED_OUTAGE_MS=41633
```

The significance of the second result is not the outage duration.

The relevant property is that transport reachability disappeared and later returned without transport identity being treated as the logical session identity.

### Status

```text
TRANSPORT_CONTINUITY=VALIDATED_UNDER_TESTED_CONDITIONS
```

---

## 6. Lifetime Isolation

A session identifier may exist across more than one runtime lifetime.

VRP therefore distinguishes the current lifetime from historical lifetimes.

This matters because delayed work from an older lifetime may arrive after a newer lifetime already exists.

A dedicated validation exercised 128 sequential incarnations of the same logical session identity.

After incarnation 128 became current, operations associated with the 127 historical incarnations were delivered concurrently.

Result:

```text
TOTAL_INCARNATIONS=128
HISTORICAL_OPERATIONS=127
HISTORICAL_ACCEPTED=0
HISTORICAL_REJECTED=127
CURRENT_LIFETIME_PRESERVED=YES
```

The test was intentionally arranged so that the operation type could otherwise be valid in the current state.

The rejection therefore tested lifetime isolation rather than merely invalid state-machine input.

### Status

```text
LIFETIME_ISOLATION=VALIDATED
```

---

## 7. Replay Resistance

Replay handling has been exercised independently from ordinary transport recovery.

Validation includes:

- duplicate delivery;
- stale sequence delivery;
- replay flooding;
- historical-lifetime replay.

A recorded validation rejected:

```text
REPLAY_ATTEMPTS=9999
REPLAY_ACCEPTED=0
REPLAY_REJECTED=9999
```

The important property is not the number 9,999.

The property is:

> previously observed work did not become new canonical work when replayed.

### Status

```text
REPLAY_RESISTANCE=VALIDATED_UNDER_TESTED_CONDITIONS
```

---

## 8. Canonical Admission

VRP maintains an explicit canonical session lifecycle.

An operation must pass the relevant lifecycle and state checks before it becomes accepted canonical work.

Regression testing verifies the following relationship:

### Accepted operation

```text
state change       = expected
history delta      = 1
canonical log delta = 1
```

### Rejected operation

```text
state change       = 0
history delta      = 0
canonical log delta = 0
```

This boundary was specifically hardened after testing identified a case where a state-invalid event could be recorded before final state-machine acceptance.

The ordering was corrected and regression coverage was added.

That history is relevant because the current claim is based on a reproduced defect followed by a verified correction, not merely on architectural intent.

### Status

```text
CANONICAL_ADMISSION=VALIDATED
```

---

## 9. Terminal-State Behavior

Terminal session states have dedicated regression coverage.

The supported event surface was exercised against terminal states including:

```text
SESSION_CLOSED
SESSION_FAILED
```

Invalid attempts to move those sessions back into ordinary operation were rejected without changing:

- canonical state;
- accepted transition history;
- canonical event history.

Explicit supported recovery remains separate from accidental resurrection.

### Status

```text
TERMINAL_STATE_ISOLATION=VALIDATED
```

---

## 10. Authority

VRP treats transport availability and authority as different properties.

A reachable node or viable path does not automatically receive permission to change canonical session state.

Validation has exercised:

- unauthorized migration attempts;
- valid authority transitions;
- stale authority;
- authority rollback;
- authority recovery;
- restart races.

A representative authority validation produced:

```text
UNAUTHORIZED_TRANSITION=REJECTED
AUTHORIZED_TRANSITION=ACCEPTED
GENERATION=1->2
```

The relevant invariant is monotonic canonical authority progression, not network reachability.

### Status

```text
AUTHORITY_CONTROL=VALIDATED_UNDER_TESTED_CONDITIONS
```

---

## 11. Read-Boundary Hardening

Validation has also targeted a different failure class: accidental mutation of canonical runtime state through APIs intended primarily for observation.

This work found real mutable-boundary defects.

Examples included returned runtime representations that could expose mutable canonical data to callers.

Those surfaces were corrected and regression-tested.

Reviewed boundaries include:

- managed session reads;
- canonical event-log reads;
- event metadata;
- path-health reads;
- relay-list reads;
- authority snapshots;
- transport-registry semantics.

This work is important because protocol invariants can be correct while an unsafe API boundary still permits state corruption.

### Status

```text
REVIEWED_READ_BOUNDARIES=ISOLATED_OR_EXPLICITLY_CLASSIFIED
```

This statement applies to the reviewed surfaces. It is not a claim that every future API automatically has safe ownership semantics.

---

## 12. Concurrency

VRP has dedicated concurrency and race-oriented validation.

The current test corpus includes scenarios involving:

- simultaneous session activity;
- delayed historical work;
- concurrent path activity;
- packet identifier generation;
- repeated session reincarnation;
- recovery behavior.

A deterministic concurrency scenario completed 1,000 repeated successful executions.

Selected regression families are also repeatedly executed with Go race detection.

### Status

```text
CONCURRENCY_VALIDATION=PASS_UNDER_TESTED_SCENARIOS
```

This is engineering evidence, not exhaustive proof of every possible scheduler interleaving.

---

## 13. Runtime Pressure

VRP has been exercised under degraded runtime and network conditions including:

- latency variation;
- jitter;
- reordering;
- congestion;
- repeated migration;
- temporary transport loss.

The expected outcome is not always uninterrupted delivery.

The expected outcome is preservation of the tested canonical invariant or controlled rejection/containment.

### Status

```text
PRESSURE_VALIDATION=PASS_UNDER_TESTED_SCENARIOS
```

---

## 14. Long-Duration Validation

Short tests are insufficient for stateful continuity systems.

VRP validation therefore includes extended execution.

Recorded engineering runs include:

- prolonged network turbulence;
- repeated transport changes;
- extended link interruption;
- multi-cycle runtime validation;
- week-scale harness execution.

These runs are intended to expose state accumulation, delayed work and recovery problems that may not appear in short unit tests.

### Status

```text
LONG_DURATION_VALIDATION=PASS_UNDER_RECORDED_RUNS
```

---

## 15. Evidence Verification

VRP validation does not treat a printed `PASS` line as sufficient by itself.

Evidence workflows may bind results to information such as:

```text
UTC
source revision
source tree
environment
scenario
observations
summary
manifest
integrity hashes
verdict
```

Evidence-integrity testing has included deliberate modification of recorded evidence and historical data.

The corresponding verification paths detected inconsistencies in the exercised scenarios.

### Status

```text
EVIDENCE_INTEGRITY=VALIDATED
INDEPENDENT_EVIDENCE_VERIFICATION=VALIDATED
```

Independent verification here means verification of the exposed evidence contract.

It does **not** mean independent source-level verification of the protected runtime.

---

## 16. What These Results Establish

The current engineering evidence supports the following statements:

```text
Session identity can survive supported transport replacement.
Historical session-lifetime work can be rejected.
Replay can be rejected without becoming new canonical work.
Invalid operations can be excluded from canonical history.
Terminal states resist unsupported resurrection.
Unauthorized or stale authority can be rejected.
Recovery can preserve canonical session lineage.
Reviewed read APIs do not expose unintended canonical mutation.
Evidence inconsistency can be detected by the tested verification path.
```

These are implementation-backed engineering claims.

They are narrower than saying that VRP is universally correct.

---

## 17. What These Results Do Not Establish

The current validation does not establish that:

- every network environment has been tested;
- every possible scheduler interleaving has been explored;
- every application will survive every outage;
- every transport has identical behavior;
- every API surface is permanently defect-free;
- the entire runtime has been formally verified;
- the public evidence verifier proves correctness of private source code;
- one validated deployment automatically certifies another.

Environment-specific qualification remains necessary.

---

## 18. Relationship to Existing Transport Technologies

VRP does not claim that transport migration itself is new.

Existing systems already provide important mechanisms in this area.

Examples include:

- QUIC connection migration and Connection IDs;
- Multipath TCP;
- VPN/tunnel roaming and endpoint updates;
- application-level reconnect and failover mechanisms.

VRP is being evaluated at a different architectural boundary.

Its validation focus is the combination of:

```text
session continuity
+
explicit lifecycle identity
+
canonical state admission
+
authority progression
+
stale/replay rejection
+
transport replacement
+
externally reviewable evidence
```

The relevant comparison is therefore not:

> "Can another protocol change network paths?"

Many can.

The useful comparison is:

> "Which layer owns continuity, canonical state and authority, and what happens to those properties when transport changes or historical work arrives?"

A dedicated comparison document should evaluate these systems property by property rather than claiming a universal replacement for them.

---

## 19. Current Deployment Position

Based on the current engineering record:

```text
IMPLEMENTATION                    ACTIVE
CORE CONTINUITY MODEL             VALIDATED
LIFETIME ISOLATION                VALIDATED
REPLAY RESISTANCE                 VALIDATED
CANONICAL ADMISSION               VALIDATED
AUTHORITY CONTROL                 VALIDATED
RECOVERY                          VALIDATED
EVIDENCE VERIFICATION             VALIDATED
CONTROLLED INTEGRATION            READY FOR EVALUATION
ENVIRONMENT-SPECIFIC QUALIFICATION REQUIRED
UNIVERSAL PRODUCTION CERTIFICATION NOT CLAIMED
```

---

## 20. Review Standard

External reviewers should challenge these claims.

Useful review questions include:

- What exact invariant is being claimed?
- What scenario exercises it?
- What constitutes failure?
- Is the result tied to an identifiable build?
- Can rejected work alter canonical history?
- Can stale work become current?
- Can transport reachability create authority?
- Can returned API data mutate canonical state?
- What exactly does the independent verifier verify?
- Which assumptions remain outside the validation boundary?

Those are the questions this public validation surface is intended to support.

---

## Conclusion

VRP's current engineering status can be summarized without treating transport migration itself as a breakthrough:

> **VRP is an implemented continuity runtime whose current validation focuses on preserving session lifecycle, canonical state and authority across transport change, failure, recovery, replay and stale work.**

The current evidence supports controlled integration evaluation.

Further claims should remain tied to the environments and scenarios in which they are actually demonstrated.