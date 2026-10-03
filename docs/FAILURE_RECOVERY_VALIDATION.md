# VRP Failure and Recovery Validation

**Project:** Veil Routing Protocol (VRP)  
**Status:** Public engineering validation  
**Updated:** October 2026

---

## 1. Purpose

This document describes how Veil Routing Protocol (VRP) is validated when connectivity, transport, lifecycle, or authority changes unexpectedly.

The objective is not to claim that failures can be eliminated.

The engineering question is:

> **When part of the communication environment fails or changes, which state is allowed to survive, which work is allowed to continue, and which work must be rejected?**

VRP recovery validation therefore focuses on preserving:

- logical session identity;
- current lifecycle identity;
- canonical state;
- authority lineage;
- replay rejection;
- stale-work rejection;
- evidence consistency.

The guarantees in this document are engineering-validated properties under tested conditions. They are not claims of universal correctness.

---

## 2. Failure Model

VRP distinguishes multiple failure classes rather than representing all disruption as a generic connection failure.

Relevant classes include:

    TRANSPORT LOSS
    PATH LOSS
    TRANSPORT REPLACEMENT
    NETWORK REBINDING
    DELAYED HISTORICAL WORK
    DUPLICATE DELIVERY
    REPLAY
    INVALID STATE TRANSITION
    AUTHORITY LOSS
    STALE AUTHORITY
    AUTHORITY ROLLBACK
    RUNTIME RESTART
    EVIDENCE INCONSISTENCY

These failures do not have identical consequences.

A transport failure does not automatically invalidate logical session identity.

A stale lifecycle operation does not become valid merely because the runtime is recovering.

A reachable replacement path does not automatically receive canonical authority.

---

## 3. Recovery Principle

The central recovery rule is:

> **Recovery may replace transport state, but it must not silently redefine canonical session or authority state.**

Conceptually:

    SESSION S
       |
    TRANSPORT A
       |
       X
    FAILURE
       |
    TRANSPORT B
       |
    SESSION S

can be valid.

The following is not an acceptable recovery model:

    FAILURE
       |
       v
    BYPASS LIFECYCLE / AUTHORITY RULES
       |
       v
    ACCEPT

Recovery remains subject to canonical admission.

---

## 4. Failure Is Observable

VRP does not define continuity as the absence of observable failure.

If the physical network disappears, the correct observation may be:

    TRANSPORT_REACHABILITY=LOST

The continuity claim is not:

> The transport never failed.

The property being evaluated is:

> The transport failure did not automatically corrupt or redefine higher-level session state that was expected to survive the failure.

This distinction is important.

Transport availability and session continuity are related, but they are not the same state.

---

## 5. Real Transport Interruption

VRP has been exercised with an actual network interruption rather than only an internal simulated failure flag.

A recorded validation observed:

    TRANSPORT_REACHABILITY=LOST

followed later by:

    TRANSPORT_REACHABILITY=RECOVERED

The measured interruption was approximately:

    OBSERVED_OUTAGE_MS=41633

The duration is not presented as a VRP recovery benchmark.

It records the physical outage that occurred during that validation.

The relevant result is that real transport reachability disappeared and later returned while the logical session model remained independently identifiable.

---

## 6. Transport Replacement

A failed transport may be replaced by another viable transport.

The tested architectural relationship is:

    SESSION S
       |
    TRANSPORT A
       |
       X
    TRANSPORT A LOST
       |
       v
    TRANSPORT B AVAILABLE
       |
       v
    SESSION S

The replacement transport is not assumed to be the session itself.

Validated property:

    SUPPORTED_TRANSPORT_REPLACEMENT
                |
                v
    SESSION_IDENTITY_PRESERVED

under the tested conditions.

---

## 7. Migration

Transport recovery and migration are related but different.

Recovery asks:

> Can the runtime return to an accepted operational state after disruption?

Migration asks:

> Can the logical session move to another transport or path without being unnecessarily replaced?

VRP validates both.

A recorded stress scenario completed:

    MIGRATIONS=1000
    SESSION_IDENTITY_PRESERVED=YES

The count demonstrates repeated execution of the migration path.

It is not presented as a universal scale limit or throughput benchmark.

---

## 8. Network Rebinding

Some network changes alter transport observations without necessarily representing a new logical session.

Examples include changes associated with:

- NAT rebinding;
- CGNAT behavior;
- interface changes;
- endpoint changes;
- path changes;
- Wi-Fi/mobile transitions.

VRP treats these as transport/path events rather than automatic session replacement.

Whether continuity can actually be maintained still depends on the availability of a viable transport and the deployment environment.

---

## 9. Recovery and Session Lifetime

A recovery event may occur while old work remains in flight.

This creates a distributed-systems condition such as:

    LIFETIME N
        |
        v
    FAILURE / RECREATION
        |
        v
    LIFETIME N+1
        |
        +---- delayed work from N arrives

The delayed work must not become current merely because it references the same logical SessionID.

VRP therefore distinguishes logical session identity from lifecycle identity.

---

## 10. Historical Work During Recovery

A dedicated validation created 128 sequential incarnations of the same logical session identity.

After the final incarnation became current, operations associated with the previous 127 incarnations were delivered concurrently.

Result:

    TOTAL_INCARNATIONS=128
    HISTORICAL_OPERATIONS=127
    HISTORICAL_ACCEPTED=0
    HISTORICAL_REJECTED=127
    CURRENT_LIFETIME_PRESERVED=YES

The current state was intentionally selected so that the operation type could otherwise be accepted.

The test therefore exercised lifecycle isolation rather than relying only on state-machine invalidity.

Recovery implication:

> A newer lifetime remained authoritative despite delayed work from historical lifetimes.

---

## 11. Replay During Recovery

Recovery does not make replay valid.

The required relationship remains:

    PREVIOUSLY OBSERVED WORK
            |
            v
    REPLAYED AFTER FAILURE
            |
            X
    NOT NEW CANONICAL WORK

Replay validation has included high-volume scenarios.

One recorded run produced:

    REPLAY_ATTEMPTS=9999
    REPLAY_REJECTED=9999
    REPLAY_ACCEPTED=0

The important property is zero accepted replays in that scenario, not the specific attempt count.

---

## 12. Duplicate Delivery

Network disruption and recovery may create duplicate-delivery conditions.

The required behavior is:

    VALID OPERATION
          |
          v
    ACCEPTED ONCE

    DUPLICATE DELIVERY
          |
          X
    NO SECOND CANONICAL ACCEPTANCE

Duplicate handling is therefore part of recovery correctness rather than merely a transport optimization.

---

## 13. Invalid State Transitions During Recovery

Failure does not suspend the canonical state machine.

If an event is invalid for the current state, it remains invalid during recovery.

Expected behavior:

    RECOVERY IN PROGRESS
            +
    INVALID TRANSITION
            |
            X
         REJECTED

A recovery path that silently accepts otherwise invalid state transitions would weaken canonical-state guarantees.

---

## 14. Canonical Recovery

Successful recovery should leave one coherent accepted state.

The runtime should not produce contradictory interpretations such as:

    runtime state says A
    history says B
    canonical event record says C

VRP regression coverage therefore tests consistency between:

- canonical state;
- accepted transition history;
- canonical event history.

---

## 15. Rejected Work Must Not Enter Canonical Recovery History

A critical boundary is:

    REJECTED
       !=
    RECORDED AS CANONICALLY ACCEPTED

Engineering validation previously identified an ordering defect where a current-lifetime but state-invalid event could enter canonical EventLog history before final state-machine rejection.

The admission order was corrected.

Regression coverage now verifies:

    REJECTED EVENT

    STATE_DELTA=0
    HISTORY_DELTA=0
    CANONICAL_EVENT_DELTA=0

and:

    ACCEPTED EVENT

    STATE_DELTA=EXPECTED
    HISTORY_DELTA=1
    CANONICAL_EVENT_DELTA=1

This boundary is important during both normal operation and recovery.

---

## 16. Terminal States

Recovery must distinguish between legitimate recovery and unsupported resurrection.

Terminal states include:

    SESSION_CLOSED
    SESSION_FAILED

Dedicated validation exercised the supported event surface against terminal sessions.

Invalid resurrection attempts were rejected without changing:

- canonical state;
- accepted transition history;
- canonical event history.

The property is:

    TERMINAL STATE
         +
    ORDINARY INVALID WORK
         |
         X
    NO RESURRECTION

This does not prohibit an explicitly supported recovery path.

---

## 17. Explicit Recovery vs Accidental Resurrection

VRP treats these as different operations.

Explicit recovery:

    TERMINAL / RECOVERY-ELIGIBLE CONDITION
                |
                v
    SUPPORTED RECOVERY PATH
                |
                v
    VALIDATED TRANSITION

Accidental resurrection:

    TERMINAL STATE
          +
    STALE / INVALID ORDINARY EVENT
          |
          X
       REJECTED

This distinction prevents failure handling from becoming an implicit bypass around lifecycle rules.

---

## 18. Duplicate Session Creation During Recovery

Recovery may race with session creation or recreation.

A duplicate creation request must not silently produce competing canonical state.

Dedicated regression coverage verifies that duplicate creation does not create an additional canonical session authority or contradictory canonical history.

Expected result:

    EXISTING CANONICAL SESSION
            +
    DUPLICATE CREATE
            |
            X
    NO COMPETING CANONICAL SESSION

---

## 19. Authority During Failure

Transport availability and canonical authority remain separate during failure and recovery.

The following implication is invalid:

    NEW TRANSPORT REACHABLE
            =>
    NEW TRANSPORT AUTHORIZED

A replacement path may be usable while the actor attempting a canonical transition remains unauthorized.

VRP validation explicitly tests this distinction.

---

## 20. Unauthorized Recovery Attempt

A representative authority scenario verifies:

    UNAUTHORIZED TRANSITION=REJECTED

while a valid authorized transition can produce:

    AUTHORIZED TRANSITION=ACCEPTED

This matters because failover mechanisms frequently operate precisely when topology and connectivity are changing.

Recovery must not convert topology change into implicit authority.

---

## 21. Authority Generation

Accepted authority progression follows canonical lineage.

A representative progression is:

    GENERATION 1
        |
        v
    VALID AUTHORITY TRANSITION
        |
        v
    GENERATION 2

Once newer authority has been accepted, historical authority must not silently become current again.

---

## 22. Authority Rollback

Recovery scenarios include attempts to reuse stale authority.

Expected behavior:

    STALE AUTHORITY
         |
         X
      REJECTED

and:

    AUTHORITY ROLLBACK
         |
         X
      REJECTED

This protects canonical authority lineage across failure and recovery boundaries.

---

## 23. Restart and Recovery Races

Restart conditions can create overlap between:

- previous authority;
- new authority;
- delayed work;
- transport recovery;
- runtime reconstruction.

VRP has exercised restart-oriented authority races and canonical recovery scenarios.

The validated objective is not that restarts are invisible.

It is that stale or contradictory authority does not become current merely because execution restarted.

---

## 24. Fail-Closed Recovery

Unknown or invalid recovery context should not default to acceptance.

Representative behavior is:

    MISSING LIFETIME CONTEXT -> REJECT

    STALE LIFETIME          -> REJECT

    REPLAY                   -> REJECT

    INVALID STATE           -> REJECT

    UNAUTHORIZED AUTHORITY   -> REJECT

This is the fail-closed recovery model.

---

## 25. Runtime Pressure During Recovery

Real recovery rarely occurs under ideal network conditions.

VRP validation has therefore included conditions such as:

- latency variation;
- jitter;
- packet reordering;
- congestion;
- repeated migration;
- temporary connectivity loss.

The expected outcome is not perfect delivery.

The expected outcome is preservation of the tested canonical invariants or contained failure.

---

## 26. Jitter

Jitter can change event arrival timing and expose assumptions about ordering.

VRP pressure validation exercises variable timing while observing whether canonical state remains coherent.

A jitter event does not automatically authorize stale or reordered work.

---

## 27. Latency Spikes

Large latency changes may cause historical work to arrive after the runtime has already progressed.

This makes latency pressure relevant to lifecycle isolation.

The required property remains:

    LATE
      !=
    CURRENT

unless the operation independently satisfies current admission requirements.

---

## 28. Reordering

Packet or event reordering is expected in distributed communication systems.

VRP does not rely on arrival order alone as proof of canonical validity.

Reordered work remains subject to:

- lifecycle validation;
- state validation;
- replay validation;
- authority validation where applicable.

---

## 29. Congestion

Congestion may increase latency, loss, duplication or recovery pressure.

Congestion does not change the canonical admission contract.

A deployment may experience degraded performance while still preserving state correctness.

Performance degradation and canonical corruption are therefore reported separately.

---

## 30. Concurrency During Recovery

Recovery can overlap with:

- session activity;
- delayed work;
- path changes;
- packet processing;
- authority operations.

VRP includes concurrency-oriented regression testing for these classes.

A deterministic concurrency scenario has completed:

    1000 repeated executions

with consistent successful results.

Selected regression families have also been exercised under Go race detection.

This provides engineering evidence for the tested concurrency scenarios.

It does not exhaustively prove every possible scheduler interleaving.

---

## 31. Long-Duration Turbulence

VRP has been exercised under prolonged network instability rather than only short failure pulses.

Recorded engineering validation includes approximately one hour of network turbulence with repeated transitions and an extended link-off period.

The objective is to expose failures that may require accumulated runtime history or repeated recovery to appear.

---

## 32. Extended Runtime Validation

Long-duration validation has also included repeated runtime cycles and week-scale harness execution.

These runs help detect:

- accumulated state errors;
- delayed-work interactions;
- resource-growth problems;
- repeated recovery defects;
- long-lived history inconsistencies.

They are not a claim of unlimited-duration correctness.

---

## 33. Application Continuity

VRP session continuity and application continuity are related but must be reported separately.

A result can legitimately be:

    VRP_SESSION_CONTINUITY=PASS
    APPLICATION_CONTINUITY=FAIL

For example, an application may enforce a timeout shorter than the transport outage.

Likewise:

    VRP_SESSION_CONTINUITY=FAIL
    APPLICATION_RECONNECTED=YES

would not prove VRP continuity merely because the application recovered independently.

This separation is required for meaningful deployment evidence.

---

## 34. Evidence During Failure

Important failure scenarios should preserve enough context to reconstruct what was observed.

Depending on the validation, evidence may include:

    UTC
    SOURCE REVISION
    SOURCE TREE
    ENVIRONMENT
    SCENARIO
    OBSERVATIONS
    COUNTERS
    CANONICAL SUMMARY
    MANIFEST
    INTEGRITY INFORMATION
    VERDICT

The purpose is not to generate large logs for their own sake.

The purpose is to connect a reported recovery result to a specific engineering execution.

---

## 35. Evidence Integrity

Recovery evidence has value only if inconsistent modification can be detected.

VRP validation has included deliberate modification of evidence artifacts.

Tested modifications include changes to:

- event evidence;
- summary information;
- manifest-bound data;
- historical evidence.

The corresponding verification paths detected inconsistencies in the exercised scenarios.

---

## 36. Historical Rewrite

A recovery narrative should not be silently replaceable by a contradictory reconstructed history.

Historical integrity validation therefore tests whether previously established evidence can be rewritten without detection.

Recorded validation has rejected inconsistent historical rewrites.

This is an evidence-integrity property.

It should not be confused with formal proof of every internal runtime transition.

---

## 37. Independent Verification

Selected failure and recovery evidence can be checked separately from the runtime execution that originally generated it.

Depending on the exposed scenario, verification may inspect:

- artifact consistency;
- counters;
- lifecycle identity;
- migration outcome;
- replay/stale rejection;
- authority progression;
- historical consistency;
- final verdict derivation.

The boundary is:

    INDEPENDENT EVIDENCE VERIFICATION

not:

    INDEPENDENT SOURCE-CODE AUDIT

and not:

    COMPLETE FORMAL VERIFICATION

These are different claims.

---

## 38. Recovery Failure Is a Valid Result

A validation system should not be designed so that every scenario eventually prints PASS.

If a required invariant fails, the useful result is the failure itself.

Examples include:

    SESSION_IDENTITY_CHANGED_UNEXPECTEDLY

    STALE_WORK_ACCEPTED

    REPLAY_ACCEPTED

    INVALID_EVENT_ENTERED_CANONICAL_HISTORY

    AUTHORITY_ROLLBACK_ACCEPTED

    RECOVERY_FAILED

    EVIDENCE_VERIFICATION_FAILED

Such results should block the affected claim until understood.

---

## 39. Defects Found Through Recovery Validation

The VRP engineering process has intentionally retained regression coverage for defects discovered during validation.

Examples of boundaries hardened through this process include:

- canonical event admission ordering;
- mutable EventLog exposure;
- managed-session mutable aliasing;
- event metadata isolation;
- relay-list isolation;
- cross-lifetime trace observability.

The relevant engineering process is:

    DEFECT
      |
      v
    REPRODUCTION
      |
      v
    CORRECTION
      |
      v
    REGRESSION TEST
      |
      v
    REQUALIFICATION

A past defect does not prove present correctness.

A reproduced defect followed by retained regression coverage makes the corrected boundary testable.

---

## 40. Relationship to QUIC and Multipath Systems

Transport migration itself is not unique to VRP.

QUIC provides connection migration mechanisms.

Multipath TCP can maintain communication across multiple paths.

VPN and tunnel systems may support roaming or endpoint updates.

Applications may reconnect or fail over independently.

VRP recovery validation is focused on a different combination of properties:

    TRANSPORT CHANGE
          +
    LOGICAL SESSION IDENTITY
          +
    LIFECYCLE IDENTITY
          +
    CANONICAL STATE
          +
    AUTHORITY LINEAGE
          +
    STALE / REPLAY REJECTION
          +
    RECOVERY EVIDENCE

The question is therefore not merely:

> Can traffic move to another path?

The more specific VRP question is:

> What happens to session lifecycle, canonical state and authority while that path change or recovery occurs?

---

## 41. Relationship to Application Reconnect

Application reconnect often solves a different problem.

A typical reconnect model may be:

    CONNECTION LOST
          |
          v
    CREATE NEW CONNECTION
          |
          v
    APPLICATION RESTORES STATE

That can be entirely appropriate.

VRP is concerned with cases where a logical continuity object should survive replacement of its underlying transport and where historical work, authority and canonical state must remain explicitly controlled.

These approaches are not automatically mutually exclusive.

---

## 42. Recovery Validation Matrix

Current public engineering coverage can be summarized as:

| Failure / Recovery Property | Status |
|---|---|
| Physical transport interruption | Exercised |
| Transport recovery | Validated under tested conditions |
| Transport replacement | Validated |
| Repeated migration | Validated |
| Network transition | Exercised |
| Historical-lifetime rejection | Validated |
| Replay rejection | Validated |
| Duplicate-delivery containment | Validated |
| Invalid-transition rejection | Validated |
| Canonical admission atomicity | Validated |
| Duplicate-create isolation | Validated |
| Terminal resurrection rejection | Validated |
| Authority separation | Validated |
| Stale authority rejection | Validated |
| Authority rollback rejection | Validated |
| Restart-oriented authority recovery | Validated under tested conditions |
| Runtime pressure | Validated under tested scenarios |
| Concurrency | Validated under tested scenarios |
| Long-duration turbulence | Exercised |
| Evidence integrity | Validated |
| Historical rewrite detection | Validated |
| Independent evidence verification | Validated |

---

## 43. What Recovery Validation Does Not Prove

The current evidence does not establish:

    ZERO PACKET LOSS

    ZERO APPLICATION INTERRUPTION

    ZERO RECOVERY LATENCY

    CORRECTNESS ON EVERY NETWORK

    CORRECTNESS FOR EVERY TRANSPORT

    EXHAUSTIVE CONCURRENCY COVERAGE

    UNLIMITED SESSION SCALE

    UNLIMITED RUNTIME DURATION

    COMPLETE FORMAL VERIFICATION

    UNIVERSAL PRODUCTION READINESS

Those require separate qualification.

---

## 44. Deployment Interpretation

A target deployment should reproduce the failure scenarios relevant to its actual environment.

A useful progression is:

    BASELINE
       |
       v
    TRANSPORT INTERRUPTION
       |
       v
    OBSERVE FAILURE
       |
       v
    PROVIDE / RESTORE TRANSPORT
       |
       v
    OBSERVE RECOVERY
       |
       v
    INJECT STALE / REPLAY CONDITIONS
       |
       v
    VERIFY CANONICAL STATE
       |
       v
    VERIFY AUTHORITY
       |
       v
    VERIFY EVIDENCE

The exact scenarios should match the deployment rather than mechanically reproducing every laboratory test.

---

## 45. Failure Classification for Deployment

Deployment reports should distinguish at least three classes.

### Transport failure

Example:

    TRANSPORT_REACHABILITY=LOST

This does not automatically mean session failure.

### Session continuity failure

Example:

    SESSION_IDENTITY_UNEXPECTEDLY_REPLACED

This affects the VRP continuity claim.

### Application failure

Example:

    APPLICATION_TIMEOUT

This may occur even while VRP session continuity remains preserved.

Separating these layers prevents misleading PASS/FAIL reporting.

---

## 46. Acceptance Conditions

For a recovery scenario to support a positive VRP continuity result, the relevant evidence should demonstrate:

- the failure actually occurred;
- the expected replacement or recovery opportunity existed;
- logical session identity behaved as specified;
- stale work did not become current;
- replay did not become new canonical work;
- invalid state transitions remained rejected;
- authority remained coherent where applicable;
- canonical state and history remained consistent;
- the result can be associated with the tested build and environment.

A scenario does not need every property if some are outside its scope.

Its scope should be explicit.

---

## 47. Failure Conditions

A recovery scenario should be considered failed with respect to the affected invariant if evidence demonstrates behavior such as:

    HISTORICAL WORK ACCEPTED AS CURRENT

    REPLAY ACCEPTED AS NEW CANONICAL WORK

    INVALID TRANSITION ACCEPTED

    REJECTED EVENT ENTERED CANONICAL HISTORY

    UNAUTHORIZED AUTHORITY ACCEPTED

    STALE AUTHORITY RESTORED AS CURRENT

    UNEXPECTED SESSION REPLACEMENT

    CANONICAL STATE / HISTORY DIVERGENCE

    REQUIRED RECOVERY NOT ACHIEVED

These outcomes require investigation before the corresponding guarantee can be relied upon.

---

## 48. Engineering Interpretation

The strongest useful interpretation of the current recovery evidence is not:

> VRP survives every network failure.

It is:

> **VRP has been deliberately exercised against multiple transport, lifecycle, replay, authority and recovery failure modes, and the defined canonical invariants were preserved under the recorded successful scenarios.**

That is a bounded engineering claim.

It can be independently challenged by reproducing equivalent scenarios against the relevant public validation boundary.

---

## 49. Current Status

    REAL TRANSPORT INTERRUPTION       EXERCISED

    TRANSPORT REPLACEMENT             VALIDATED

    REPEATED MIGRATION                VALIDATED

    SESSION LIFETIME ISOLATION        VALIDATED

    STALE-WORK REJECTION              VALIDATED

    REPLAY REJECTION                  VALIDATED

    CANONICAL RECOVERY                VALIDATED

    TERMINAL STATE PROTECTION         VALIDATED

    AUTHORITY RECOVERY                VALIDATED

    AUTHORITY ROLLBACK REJECTION      VALIDATED

    CONCURRENCY SCENARIOS             VALIDATED

    RUNTIME PRESSURE                  VALIDATED

    LONG-DURATION TURBULENCE          EXERCISED

    EVIDENCE INTEGRITY                VALIDATED

    INDEPENDENT VERIFICATION          VALIDATED

    TARGET ENVIRONMENT QUALIFICATION  REQUIRED

---

## Conclusion

VRP recovery is not defined as hiding transport failure.

It is defined around preservation of higher-level invariants while transport, path, lifecycle or authority conditions change.

The current implementation has engineering evidence showing that, under the tested scenarios:

- transport can disappear and later recover;
- supported transport replacement can preserve logical session identity;
- repeated migration can occur without redefining the session;
- historical lifetime work can remain rejected;
- replay can remain rejected;
- invalid transitions can remain outside canonical history;
- terminal sessions resist unsupported resurrection;
- stale or unauthorized authority can remain rejected;
- recovery can preserve canonical lineage;
- evidence can remain independently checkable.

The remaining deployment question is environment-specific:

> **Do those same required invariants remain preserved under the actual failures, transports, timing conditions, applications and operational constraints of the target system?**

That question should be answered by deliberate failure injection and evidence, not assumption.