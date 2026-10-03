# VRP Pilot Acceptance Criteria

**Project:** Veil Routing Protocol (VRP)  
**Status:** Public engineering acceptance criteria  
**Updated:** October 2026

---

## 1. Purpose

This document defines the engineering criteria for evaluating VRP in a controlled pilot.

It is not a sales checklist and it does not assume that a pilot must succeed.

The purpose of the pilot is to answer a narrower question:

> **Does the VRP implementation preserve the required continuity, lifecycle, canonical-state, authority, and evidence properties in the evaluator's actual environment?**

A pilot should therefore produce one of three useful outcomes:

    ACCEPTED

    CONDITIONALLY ACCEPTED

    NOT ACCEPTED

A failed invariant is an engineering result, not something that should be hidden to preserve a successful-looking pilot.

---

## 2. Pilot Scope

A VRP pilot is a controlled integration evaluation.

It should define:

- the VRP build being evaluated;
- the execution environment;
- the application or workload;
- the available transports;
- the expected failure conditions;
- the invariants being evaluated;
- the evidence that will support the result.

The pilot should not be treated as universal certification for unrelated environments.

---

## 3. Acceptance Model

Each important pilot claim should follow:

    REQUIREMENT
        |
        v
    SCENARIO
        |
        v
    OBSERVATION
        |
        v
    EVIDENCE
        |
        v
    VERIFICATION
        |
        v
    VERDICT

This structure separates demonstrated behavior from assumptions.

---

## 4. Build Identification

Before validation begins, the tested build should be identifiable.

Where available, record information equivalent to:

    SOURCE_REVISION=<revision>

    SOURCE_TREE=<tree>

    BUILD=<build identity>

    UTC=<timestamp>

The purpose is traceability.

A successful result should remain associated with the implementation state that produced it.

---

## 5. Environment Identification

The pilot should document the relevant environment.

Useful information may include:

    operating system

    architecture

    runtime/toolchain version

    network topology

    transport types

    relay topology

    application

    expected session scale

The environment description does not need to expose unrelated infrastructure information.

It needs to be sufficient to understand the scope of the result.

---

## 6. Baseline Gate

Failure testing should begin only after a valid baseline exists.

The baseline should demonstrate:

- runtime initialization;
- session creation;
- expected lifecycle progression;
- transport availability;
- required observability;
- evidence generation.

Minimum expected result:

    BASELINE=PASS

If the baseline cannot be established, continuity testing should not be interpreted as meaningful.

---

## 7. Session Establishment Gate

The runtime must establish the logical session required by the scenario.

The evaluator should be able to identify:

    SESSION_ID

    CURRENT STATE

and, where exposed by the validation surface:

    LIFETIME IDENTITY

Expected result:

    SESSION_ESTABLISHMENT=PASS

---

## 8. Session / Transport Separation Gate

The pilot should directly exercise the architectural invariant:

> **Session ≠ Transport**

A representative scenario is:

    SESSION=S
    TRANSPORT=A

followed by:

    TRANSPORT=A -> unavailable

and:

    TRANSPORT=B -> active

with:

    SESSION=S

Expected result:

    SESSION_IDENTITY_PRESERVED=YES

The pilot should not infer this property merely because application traffic eventually resumed.

---

## 9. Physical Transport Failure Gate

Where operationally safe, at least one scenario should include a real transport interruption.

Examples may include:

- disabling an active interface;
- removing connectivity;
- switching networks;
- interrupting the active path.

The evaluator should observe an actual condition equivalent to:

    TRANSPORT_REACHABILITY=LOST

This distinguishes physical failure testing from purely internal state simulation.

---

## 10. Transport Recovery Gate

After interruption, connectivity should be restored or replacement connectivity should become available.

The evaluator should observe a condition equivalent to:

    TRANSPORT_REACHABILITY=RECOVERED

The pilot must then evaluate whether the logical session behaved according to the defined continuity requirement.

Expected result:

    TRANSPORT_RECOVERY=PASS

---

## 11. Transport Replacement Gate

The pilot should verify that supported transport replacement does not unnecessarily redefine logical session identity.

Expected relationship:

    SESSION_BEFORE == SESSION_AFTER

while:

    TRANSPORT_BEFORE != TRANSPORT_AFTER

where transport replacement is the scenario being evaluated.

Expected result:

    TRANSPORT_REPLACEMENT=PASS

---

## 12. Migration Gate

At least one supported migration should be exercised.

The evaluator should verify:

- migration was requested or triggered;
- a viable replacement path/transport existed;
- canonical lifecycle progression remained valid;
- migration completed;
- logical session identity remained coherent.

Expected result:

    MIGRATION=PASS

---

## 13. Repeated Migration Gate

If repeated migration is relevant to the deployment, it should be tested as a separate workload.

Project-level engineering validation has previously exercised:

    1000 migrations

with session identity preserved.

A pilot does not need to reproduce that exact number.

It should select a migration count appropriate to the expected deployment behavior.

The acceptance property is correctness across repeated migration, not achievement of a marketing number.

---

## 14. Network Transition Gate

The pilot should exercise network transitions that actually matter to the deployment.

Examples include:

    Wi-Fi -> Mobile

    Network A -> Network B

    Path A -> Path B

    Relay A -> Relay B

Testing irrelevant transition types provides less value than exercising the environment's real failure modes.

---

## 15. Session Lifetime Gate

If a logical session can be recreated or reincarnated, the pilot should verify that historical and current lifetimes remain distinguishable.

The required property is:

    SAME LOGICAL SESSION ID
            !=
    SAME CURRENT LIFETIME

where recreation has occurred.

The evaluator should be able to distinguish historical work from work belonging to the current lifecycle.

---

## 16. Stale-Lifetime Gate

A pilot should deliver work associated with an older lifetime after a newer lifetime has become current.

Expected result:

    STALE_LIFETIME_OPERATION=REJECTED

and:

    CURRENT_LIFETIME_PRESERVED=YES

Project-level validation has exercised 128 sequential incarnations with 127 historical delayed operations rejected and zero accepted.

The pilot may use a smaller environment-appropriate scenario while testing the same invariant.

---

## 17. Replay Gate

Previously observed work should be delivered again through the supported validation path.

Expected result:

    REPLAY_ACCEPTED=0

The pilot should verify that replay does not become a new canonical operation.

Project-level validation has included a recorded 9,999-attempt replay scenario with zero accepted replays.

The pilot does not need to reproduce the exact attempt count unless volume itself is part of the evaluation.

---

## 18. Duplicate Delivery Gate

Duplicate delivery should not create duplicate canonical execution.

Expected relationship:

    ORIGINAL OPERATION -> ACCEPTED ONCE

    DUPLICATE          -> NOT ACCEPTED AS NEW WORK

The evaluator should inspect canonical state/history rather than relying only on transport-level delivery counters.

---

## 19. Invalid Transition Gate

The pilot should attempt at least one operation that is invalid for the current canonical state.

Expected result:

    INVALID_TRANSITION=REJECTED

with:

    STATE_DELTA=0

    ACCEPTED_HISTORY_DELTA=0

    CANONICAL_EVENT_DELTA=0

This verifies more than simple error reporting.

It verifies that rejected work remains outside canonical state.

---

## 20. Canonical Acceptance Gate

The pilot should also verify the positive side of the boundary.

For a valid accepted operation:

    STATE_DELTA=EXPECTED

    ACCEPTED_HISTORY_DELTA=1

    CANONICAL_EVENT_DELTA=1

The objective is to establish both sides of the contract:

    ACCEPTED -> RECORDED ONCE

    REJECTED -> NOT RECORDED AS ACCEPTED

---

## 21. Terminal-State Gate

Where terminal states are exercised, unsupported ordinary events must not silently resurrect the session.

Relevant states include:

    SESSION_CLOSED

    SESSION_FAILED

Expected result:

    UNSUPPORTED_RESURRECTION=REJECTED

with no canonical state/history mutation.

Explicit supported recovery should be tested separately.

---

## 22. Duplicate-Create Gate

Attempting to create an already-existing session must not silently create competing canonical session state.

Expected result:

    DUPLICATE_CREATE
          |
          X
    NO SECOND CANONICAL SESSION

This is particularly relevant around restart, retry, and recovery behavior.

---

## 23. Fail-Closed Admission Gate

The pilot should verify that invalid context does not default to acceptance.

Representative cases include:

    missing lifecycle context

    stale lifecycle context

    replay

    invalid state

    unauthorized authority

Expected behavior:

    INVALID / UNKNOWN -> REJECTED

A recovery condition should not weaken this rule.

---

## 24. Authority Gate

Where authority management is part of the pilot, the evaluator should test both negative and positive cases.

At minimum:

    UNAUTHORIZED TRANSITION -> REJECTED

    AUTHORIZED TRANSITION   -> ACCEPTED

Reachability must not be treated as proof of authority.

Expected result:

    AUTHORITY_VALIDATION=PASS

---

## 25. Authority Progression Gate

Where authority generations are used, valid progression should remain ordered.

Representative behavior:

    GENERATION=1
        |
        v
    VALID TRANSITION
        |
        v
    GENERATION=2

The evaluator should verify that the previous generation does not remain equivalent to the new current authority.

---

## 26. Authority Rollback Gate

A stale authority generation should be presented after a newer generation has become current.

Expected result:

    STALE_AUTHORITY=REJECTED

and:

    AUTHORITY_ROLLBACK=REJECTED

This gate is especially relevant for failover and restart scenarios.

---

## 27. Recovery Gate

The pilot should exercise at least one recovery scenario relevant to the target environment.

A representative sequence is:

    SESSION ACTIVE
         |
         v
    TRANSPORT FAILURE
         |
         v
    REPLACEMENT / RESTORATION
         |
         v
    MIGRATION / RECOVERY
         |
         v
    SESSION ACTIVE

Expected result:

    RECOVERY=PASS

The actual sequence may differ by deployment.

---

## 28. Recovery Integrity Gate

Recovery must not bypass normal canonical rules.

During recovery:

    replay remains replay

    stale work remains stale

    invalid transitions remain invalid

    unauthorized authority remains unauthorized

Expected result:

    RECOVERY_ADMISSION=FAIL_CLOSED

This is more important than merely observing that traffic eventually resumed.

---

## 29. Canonical-State Consistency Gate

After failure and recovery, the evaluator should compare:

- final canonical state;
- accepted transition history;
- canonical event history.

They should describe one coherent accepted execution.

A result such as:

    STATE=A
    ACCEPTED_HISTORY implies B

requires investigation.

Expected result:

    CANONICAL_STATE_CONSISTENCY=PASS

---

## 30. Read-Isolation Gate

Where the pilot exposes runtime read APIs to integration code, the evaluator should verify the documented ownership semantics.

Read-oriented data should not unexpectedly provide a direct mutation path into canonical runtime state.

Project-level regression coverage already exists for reviewed surfaces including:

- managed-session reads;
- canonical EventLog reads;
- supported event metadata;
- path-health reads;
- relay-list reads.

A pilot should test the surfaces it actually integrates.

---

## 31. Concurrency Gate

If concurrent operation is part of the expected deployment, the pilot should include concurrent workloads.

Relevant scenarios may include:

- multiple sessions;
- overlapping transport events;
- delayed historical work;
- simultaneous path observations;
- recovery while other sessions remain active.

The acceptance property is preservation of the relevant invariant under the tested concurrent workload.

---

## 32. Race Qualification

Where supported by the engineering environment, relevant test families should be exercised under race detection or equivalent concurrency diagnostics.

A race-detector PASS is useful evidence.

It is not a substitute for invariant testing.

The pilot should preserve both distinctions.

---

## 33. Jitter Gate

If the target environment experiences timing variability, the pilot should inject or reproduce jitter.

Expected result:

    REQUIRED_CANONICAL_INVARIANTS=PRESERVED

A jitter scenario may degrade latency without constituting a canonical-state failure.

---

## 34. Latency Gate

The pilot should test latency conditions representative of the deployment.

Late work must not automatically become current work.

The evaluator should separately measure:

    CONTINUITY RESULT

and:

    LATENCY RESULT

Correctness and performance should not be collapsed into one verdict.

---

## 35. Reordering Gate

Where event or packet reordering is realistic, the pilot should exercise it.

Reordered work remains subject to:

- lifecycle validation;
- replay validation;
- state validation;
- authority validation where applicable.

Expected result:

    REORDERING_DOES_NOT_BYPASS_ADMISSION=PASS

---

## 36. Congestion Gate

Congestion testing should evaluate whether increased pressure causes invariant failure.

A valid result may include degraded performance while canonical correctness remains preserved.

The pilot should therefore report:

    PERFORMANCE

and:

    CORRECTNESS

separately.

---

## 37. Resource Gate

The target environment should define resource limits appropriate to the pilot.

Relevant measurements may include:

    CPU

    MEMORY

    SESSION COUNT

    TRANSPORT COUNT

    EVIDENCE VOLUME

    MIGRATION RATE

Project-level correctness evidence should not be interpreted as proof of arbitrary production capacity.

---

## 38. Long-Duration Gate

A pilot intended to inform production deployment should include extended execution.

The duration should reflect the intended operational use.

Long-duration testing can expose:

- accumulated state;
- delayed work;
- resource growth;
- repeated recovery defects;
- long-lived concurrency problems.

Project-level validation includes prolonged turbulence and week-scale harness execution, but target-environment qualification remains necessary.

---

## 39. Application Gate

Application behavior must be evaluated independently from VRP session continuity.

The pilot should be capable of reporting results such as:

    VRP_SESSION_CONTINUITY=PASS

    APPLICATION_CONTINUITY=FAIL

or:

    VRP_SESSION_CONTINUITY=PASS

    APPLICATION_CONTINUITY=PASS

This avoids representing application reconnect as proof of VRP continuity or application timeout as automatic proof of VRP failure.

---

## 40. Evidence Capture Gate

Important pilot scenarios should produce reviewable evidence.

Depending on the scenario, evidence may include:

    UTC

    BUILD IDENTITY

    ENVIRONMENT

    SCENARIO

    OBSERVATIONS

    COUNTERS

    CANONICAL SUMMARY

    MANIFEST

    INTEGRITY INFORMATION

    VERDICT

Expected result:

    EVIDENCE_CAPTURE=PASS

---

## 41. Evidence Integrity Gate

Where evidence is integrity-bound, the pilot should verify that modification is detectable.

A simple acceptance relationship is:

    ORIGINAL EVIDENCE -> VERIFIABLE

    MODIFIED EVIDENCE -> VERIFICATION FAILURE

The exact verification mechanism depends on the exposed evidence contract.

---

## 42. Semantic Verification Gate

Verification should evaluate scenario meaning, not only file hashes.

For example, a replay scenario should fail if:

    REPLAY_ACCEPTED > 0

even if every file hash is internally consistent.

Likewise, a migration scenario should fail its continuity property if:

    SESSION_BEFORE != SESSION_AFTER

where session preservation is the requirement.

---

## 43. Independent Verification Gate

Where the public verification surface supports the scenario, evidence should be evaluated separately from the runtime execution that generated it.

Expected result:

    INDEPENDENT_VERIFICATION=PASS

This means verification of the exposed evidence contract.

It does not imply complete independent source review or formal proof of the protected runtime.

---

## 44. Verification Failure

If:

    RUNTIME_VERDICT=PASS

but:

    INDEPENDENT_VERIFICATION=FAIL

the pilot must not report the independently verified result as PASS.

The discrepancy should be investigated.

The runtime's own verdict does not override contradictory verification evidence.

---

## 45. Reproduction Gate

Important failures should be reproduced where practical.

The engineering sequence should be:

    OBSERVE

    PRESERVE EVIDENCE

    REPRODUCE

    ISOLATE

    CORRECT

    REGRESSION TEST

    REQUALIFY

A pilot is valuable even when it discovers a defect.

The important question is whether the defect becomes reproducible engineering information.

---

## 46. Pilot Acceptance Matrix

A minimal controlled pilot may use the following matrix:

| Property | Requirement |
|---|---|
| Build identified | Required |
| Environment documented | Required |
| Baseline | Pass |
| Session establishment | Pass |
| Session / transport separation | Pass |
| Physical or controlled interruption | Pass |
| Transport recovery | Pass |
| Migration | Pass |
| Replay rejection | Pass |
| Stale-lifetime rejection | Pass |
| Invalid-transition rejection | Pass |
| Canonical acceptance consistency | Pass |
| Recovery | Pass |
| Evidence capture | Pass |
| Evidence verification | Pass |
| Authority validation | When in scope |
| Authority rollback | When in scope |
| Concurrency | When operationally relevant |
| Runtime pressure | Recommended |
| Long-duration execution | Required before production acceptance |
| Application qualification | When in scope |
| Performance qualification | Environment-specific |
| Scale qualification | Environment-specific |

---

## 47. Mandatory Core Gate

For a pilot intended to evaluate the core VRP continuity model, the minimum required properties are:

    SESSION IDENTITY PRESERVED
                AND
    STALE LIFETIME WORK REJECTED
                AND
    REPLAY REJECTED
                AND
    INVALID CANONICAL TRANSITIONS REJECTED
                AND
    RECOVERY COMPLETED
                AND
    EVIDENCE CONSISTENT

If authority management is part of the evaluated integration, add:

    UNAUTHORIZED AUTHORITY REJECTED
                AND
    STALE AUTHORITY REJECTED

---

## 48. Acceptance Verdict: ACCEPTED

A pilot may be classified:

    ACCEPTED

when the required invariants and environment-specific acceptance conditions pass for the defined scope.

The verdict must identify that scope.

For example:

> Accepted for the evaluated build, topology, transports, workload, and failure scenarios.

It should not imply certification of unrelated environments.

---

## 49. Acceptance Verdict: CONDITIONALLY ACCEPTED

A pilot may be classified:

    CONDITIONALLY ACCEPTED

when core evaluated invariants pass but unresolved environment-specific work remains.

Examples may include:

- performance tuning;
- monitoring integration;
- capacity qualification;
- application timeout configuration;
- operational automation.

Conditions should be written explicitly.

---

## 50. Acceptance Verdict: NOT ACCEPTED

A pilot should be classified:

    NOT ACCEPTED

for the affected scope when a mandatory invariant fails and remains unresolved.

Examples include:

    STALE WORK ACCEPTED AS CURRENT

    REPLAY ACCEPTED AS NEW CANONICAL WORK

    INVALID EVENT ENTERED CANONICAL HISTORY

    UNAUTHORIZED AUTHORITY ACCEPTED

    AUTHORITY ROLLBACK ACCEPTED

    SESSION IDENTITY UNEXPECTEDLY REPLACED

    REQUIRED RECOVERY FAILED

    EVIDENCE VERIFICATION FAILED

A negative verdict is preferable to hiding an unresolved invariant violation.

---

## 51. Pilot Report

The final pilot report should contain enough information for another engineer to understand what was actually evaluated.

Recommended structure:

    1. Build identity

    2. Environment

    3. Integration scope

    4. Required invariants

    5. Scenarios executed

    6. Observed results

    7. Evidence references

    8. Verification results

    9. Failures / limitations

    10. Final verdict

The report should separate facts from interpretation.

---

## 52. Performance Is a Separate Decision

A pilot may pass correctness requirements and still fail performance requirements.

For example:

    CONTINUITY=PASS

    LATENCY_REQUIREMENT=FAIL

This is a meaningful result.

The opposite can also occur:

    PERFORMANCE=GOOD

    CANONICAL_INVARIANT=FAIL

Good performance must not compensate for canonical correctness failure.

---

## 53. Scale Is a Separate Decision

A successful small pilot does not establish arbitrary production scale.

The target organization should define required values for:

    concurrent sessions

    traffic volume

    transport count

    migration rate

    runtime duration

    evidence volume

Scale acceptance should be based on measured target-environment behavior.

---

## 54. Security Review Is a Separate Decision

The pilot criteria in this document exercise important security-related invariants such as:

- replay rejection;
- lifecycle isolation;
- authority rejection;
- fail-closed admission;
- evidence integrity.

They do not replace a broader security review.

A deployment may separately require:

- source review;
- cryptographic review;
- host hardening;
- credential management review;
- threat modeling;
- penetration testing;
- compliance assessment.

Those activities evaluate different boundaries.

---

## 55. Formal Verification Is Not a Pilot Requirement

The current VRP validation model is engineering-based.

It includes:

- regression testing;
- adversarial scenarios;
- failure injection;
- race detection;
- repeated execution;
- real transport interruption;
- long-duration testing;
- evidence verification.

The project does not represent the controlled pilot as complete formal verification of the runtime.

A formal-methods program, if used, would be a separate engineering activity.

---

## 56. Relationship to QUIC, MPTCP, VPN Roaming, and Application Failover

The pilot should not attempt to prove that transport migration itself is unique to VRP.

It is not.

Existing systems already provide mechanisms including:

    QUIC connection migration

    Multipath TCP

    VPN / tunnel endpoint roaming

    application reconnect

    application failover

The useful VRP pilot question is narrower:

> **Does the VRP continuity model preserve its session lifecycle, canonical state, authority, stale-work rejection, and evidence properties while the underlying transport changes?**

This allows VRP to be evaluated on its actual architectural boundary rather than on an exaggerated novelty claim.

---

## 57. Comparison Should Be Property-Based

If the pilot includes comparison with another technology, the comparison should ask:

    What is the logical identity?

    What happens when transport changes?

    How is old work distinguished from current work?

    How is replay handled?

    Where is canonical state owned?

    How is authority changed?

    What happens during restart?

    What evidence can be independently checked?

This is more useful than declaring one technology a universal replacement for another.

---

## 58. No Universal Pilot Result

A successful pilot means:

> The required properties passed under the evaluated conditions.

It does not mean:

> Every future VRP deployment is automatically qualified.

The result remains bounded by:

- build;
- environment;
- topology;
- workload;
- failure scenarios;
- duration;
- scale.

---

## 59. Current Project-Level Evidence

Before a target-environment pilot, the VRP engineering record already includes validation involving:

- physical transport interruption;
- transport replacement;
- repeated migration;
- stale lifetime rejection;
- replay flooding;
- invalid canonical transitions;
- terminal-state protection;
- authority progression;
- authority rollback rejection;
- recovery;
- concurrency;
- prolonged network turbulence;
- evidence modification;
- historical rewrite detection;
- independent evidence verification.

The pilot should build on this evidence rather than simply repeat every project-level scenario without regard to the target environment.

---

## 60. Current Pilot Position

The current engineering position is:

    CORE CONTINUITY MODEL          VALIDATED

    LIFETIME ISOLATION             VALIDATED

    REPLAY REJECTION               VALIDATED

    CANONICAL ADMISSION            VALIDATED

    AUTHORITY MODEL                VALIDATED

    RECOVERY                       VALIDATED

    EVIDENCE VERIFICATION          VALIDATED

    CONTROLLED PILOT               READY FOR EVALUATION

    TARGET ENVIRONMENT             MUST BE QUALIFIED

    UNIVERSAL PRODUCTION STATUS    NOT CLAIMED

---

## Conclusion

A VRP pilot should not be designed to produce a predetermined success message.

It should deliberately create the conditions most likely to challenge the continuity model:

    transport failure

    transport replacement

    migration

    replay

    stale historical work

    invalid state transitions

    authority change

    recovery

and then determine whether the required invariants survive.

The strongest appropriate pilot result is therefore not:

> "VRP worked."

It is:

> **For the identified build and environment, the specified continuity, lifecycle, canonical-state, authority, recovery, and evidence properties passed the defined acceptance scenarios.**

If they do not pass, the pilot has still produced useful engineering information.

The purpose of the evaluation is to discover that before an unsupported assumption becomes a production dependency.