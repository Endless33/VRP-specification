# VRP Controlled Deployment Acceptance

**Project:** Veil Routing Protocol (VRP)  
**Status:** Public controlled-deployment acceptance model  
**Updated:** October 2026

---

## 1. Purpose

This document defines the engineering conditions under which a VRP integration can move from controlled evaluation toward controlled deployment.

It does not declare VRP universally production-ready.

It defines a narrower decision:

> **Has the specific VRP build, integration, environment, and workload demonstrated the invariants required for this deployment?**

The decision must be based on observed behavior and evidence rather than architectural expectation alone.

---

## 2. Deployment Acceptance Is Environment-Specific

A successful project-level validation does not automatically qualify every deployment.

A deployment introduces its own:

    OPERATING SYSTEM

    NETWORK TOPOLOGY

    TRANSPORTS

    APPLICATIONS

    FAILURE MODES

    SCALE

    TIMING

    RESOURCE LIMITS

    OPERATIONAL PROCEDURES

Controlled deployment acceptance therefore requires evidence from the target environment or a sufficiently representative environment.

---

## 3. Acceptance States

A deployment evaluation may end in:

    ACCEPTED

    ACCEPTED WITH CONDITIONS

    NOT ACCEPTED

These states apply only to the documented scope.

They are not universal certification labels.

---

## 4. Required Scope Definition

Before acceptance testing begins, the deployment should define:

- build identity;
- environment;
- integration boundary;
- expected transports;
- expected applications;
- expected session scale;
- required continuity properties;
- authority model where applicable;
- required recovery behavior;
- performance requirements;
- evidence requirements.

A deployment cannot be meaningfully accepted against requirements that were never defined.

---

## 5. Build Identity

The tested implementation must be identifiable.

Where available, preserve information equivalent to:

    SOURCE_REVISION=<revision>

    SOURCE_TREE=<tree>

    BUILD_ID=<identity>

    UTC=<timestamp>

Acceptance applies to the evaluated engineering state.

A later materially changed build requires appropriate requalification.

---

## 6. Environment Identity

The acceptance record should identify relevant environmental properties.

Examples include:

    OS

    KERNEL

    ARCHITECTURE

    RUNTIME / TOOLCHAIN

    NETWORK TYPE

    TRANSPORT TYPES

    RELAY TOPOLOGY

    APPLICATION

    EXPECTED SESSION LOAD

The objective is reproducibility and scope control, not unnecessary infrastructure disclosure.

---

## 7. Baseline Requirement

Before failure injection, normal operation must succeed.

Required baseline properties should include:

    RUNTIME INITIALIZATION=PASS

    SESSION CREATION=PASS

    REQUIRED LIFECYCLE PROGRESSION=PASS

    TRANSPORT ATTACHMENT=PASS

    EVIDENCE GENERATION=PASS

If baseline operation fails, deployment acceptance should stop until the failure is understood.

---

## 8. Core Architectural Requirement

The controlled deployment must preserve:

> **Session ≠ Transport**

For supported continuity scenarios, replacing the transport must not unnecessarily redefine the logical session.

Expected relationship:

    SESSION_BEFORE == SESSION_AFTER

while:

    TRANSPORT_BEFORE != TRANSPORT_AFTER

where transport replacement is the tested scenario.

---

## 9. Transport Failure Requirement

The deployment should exercise a real or representative loss of the active transport.

Expected observation:

    TRANSPORT_REACHABILITY=LOST

The test should establish that an actual transport failure occurred rather than merely asserting that a migration function was called.

---

## 10. Transport Recovery Requirement

The deployment should then restore connectivity or provide a viable replacement transport.

Expected observation:

    TRANSPORT_REACHABILITY=RECOVERED

or an equivalent validated replacement condition.

The runtime must then satisfy the defined recovery requirements.

---

## 11. Session Continuity Requirement

For scenarios where continuity is expected:

    TRANSPORT FAILURE
           |
           v
    TRANSPORT REPLACEMENT
           |
           v
    SESSION IDENTITY PRESERVED

must hold.

If the deployment intentionally defines a scenario where a new session should be created, that scenario should be classified separately.

---

## 12. Migration Requirement

Supported migration must preserve the required logical continuity state.

The deployment should verify:

- migration initiation;
- replacement viability;
- lifecycle validity;
- canonical state progression;
- migration completion;
- logical session identity.

A successful migration should not be inferred solely from resumed traffic.

---

## 13. Repeated Migration Requirement

If the operational environment expects frequent network movement, repeated migration should be qualified.

Project-level engineering validation has exercised:

    1000 migrations

with session identity preserved.

This is evidence that the repeated path has been exercised.

It is not a requirement that every deployment reproduce exactly 1,000 migrations.

The deployment should choose a workload representative of its intended operation.

---

## 14. Lifecycle Identity Requirement

If a session can be recreated, the deployment must distinguish current and historical lifetimes.

Required property:

    SAME SESSION ID
          !=
    AUTOMATICALLY SAME CURRENT LIFETIME

Historical operations must remain distinguishable from operations belonging to the current lifecycle.

---

## 15. Historical-Work Rejection Requirement

The deployment should deliver work associated with an older lifecycle after a newer lifecycle is current.

Expected result:

    HISTORICAL_OPERATION=REJECTED

and:

    CURRENT_LIFETIME_PRESERVED=YES

Project-level validation has exercised:

    TOTAL_INCARNATIONS=128

    HISTORICAL_OPERATIONS=127

    HISTORICAL_REJECTED=127

    HISTORICAL_ACCEPTED=0

Target deployment qualification should test the same invariant at an appropriate scale.

---

## 16. Replay Requirement

Previously observed work must not become new canonical work when replayed.

Required result:

    REPLAY_ACCEPTED=0

Project-level engineering validation includes a recorded scenario with:

    REPLAY_ATTEMPTS=9999

    REPLAY_REJECTED=9999

    REPLAY_ACCEPTED=0

The deployment should reproduce the invariant using its relevant traffic and integration path.

---

## 17. Duplicate-Delivery Requirement

Duplicate delivery must not produce duplicate canonical execution.

Required behavior:

    ORIGINAL -> ACCEPTED ONCE

    DUPLICATE -> NOT NEW CANONICAL WORK

The acceptance decision should be based on canonical execution state rather than transport counters alone.

---

## 18. Canonical Admission Requirement

Accepted and rejected operations must have different canonical effects.

For accepted work:

    STATE_DELTA=EXPECTED

    ACCEPTED_HISTORY_DELTA=1

    CANONICAL_EVENT_DELTA=1

For rejected work:

    STATE_DELTA=0

    ACCEPTED_HISTORY_DELTA=0

    CANONICAL_EVENT_DELTA=0

This is a mandatory state-integrity boundary.

---

## 19. Canonical Admission Regression History

Engineering validation previously identified an ordering defect where a state-invalid event could enter canonical EventLog history before final rejection.

The defect was corrected.

Regression coverage now verifies that rejected events do not contaminate canonical accepted history.

This corrected boundary should remain part of deployment qualification because recovery and integration code may exercise it differently from isolated tests.

---

## 20. Invalid-State Requirement

The deployment must verify that an event invalid for the current canonical state is rejected.

Required result:

    INVALID_TRANSITION=REJECTED

with no canonical mutation.

A transport failure or recovery condition must not weaken this requirement.

---

## 21. Terminal-State Requirement

Terminal sessions must reject unsupported resurrection.

Relevant states include:

    SESSION_CLOSED

    SESSION_FAILED

Required behavior:

    UNSUPPORTED_RESURRECTION
             |
             X
          REJECTED

Explicit supported recovery should be tested independently.

---

## 22. Duplicate-Create Requirement

Retries, reconnect logic, or restart races may cause duplicate creation attempts.

Required behavior:

    EXISTING CANONICAL SESSION
              +
    DUPLICATE CREATE
              |
              X
    NO COMPETING CANONICAL SESSION

Duplicate creation must not silently fork canonical state.

---

## 23. Fail-Closed Requirement

Controlled deployment requires fail-closed canonical admission.

Representative behavior:

    UNKNOWN CONTEXT       -> REJECT

    MISSING LIFETIME      -> REJECT

    STALE LIFETIME        -> REJECT

    REPLAY                -> REJECT

    INVALID STATE         -> REJECT

    UNAUTHORIZED AUTHORITY -> REJECT

A recovery path must not convert these cases into implicit acceptance.

---

## 24. Connectivity / Authority Separation

Where authority management is in scope:

    REACHABLE
       !=
    AUTHORIZED

must remain true.

A new path, relay, transport, or endpoint must not receive canonical authority merely because it became reachable.

---

## 25. Unauthorized Authority Requirement

The deployment should attempt at least one unauthorized authority-sensitive operation.

Expected result:

    UNAUTHORIZED_TRANSITION=REJECTED

This verifies the negative side of the authority boundary.

---

## 26. Authorized Authority Requirement

The deployment should also execute the corresponding valid authority path.

Expected result:

    AUTHORIZED_TRANSITION=ACCEPTED

Testing only unauthorized rejection is insufficient if the valid operational path has never been demonstrated.

---

## 27. Authority Progression Requirement

Where authority generations are used, progression must remain ordered.

Representative sequence:

    GENERATION 1
        |
        v
    VALID AUTHORITY TRANSITION
        |
        v
    GENERATION 2

The newer generation must become canonical according to the defined authority model.

---

## 28. Authority Rollback Requirement

Historical authority must not silently regain current status.

Expected results:

    STALE_AUTHORITY=REJECTED

    AUTHORITY_ROLLBACK=REJECTED

This requirement is especially important around restart, failover, and regional recovery.

---

## 29. Restart Requirement

If restart is operationally relevant, the deployment must test it.

The scenario should include relevant combinations of:

    CURRENT STATE

    HISTORICAL STATE

    DELAYED WORK

    AUTHORITY

    TRANSPORT RECOVERY

The restart must not create a second incompatible canonical lineage.

---

## 30. Recovery Requirement

A representative recovery scenario should exercise:

    SESSION ACTIVE
         |
         v
    FAILURE
         |
         v
    REPLACEMENT / RESTORATION
         |
         v
    RECOVERY
         |
         v
    VALID CANONICAL STATE

Expected result:

    RECOVERY=PASS

The exact scenario should match the target deployment.

---

## 31. Recovery Must Preserve Admission

Recovery is not an exception to normal validation.

During recovery:

    STALE -> REJECT

    REPLAY -> REJECT

    INVALID -> REJECT

    UNAUTHORIZED -> REJECT

The deployment must not require bypassing these controls merely to restore service.

---

## 32. Canonical Recovery Requirement

After recovery, the following should describe one coherent accepted execution:

    CURRENT STATE

    ACCEPTED TRANSITION HISTORY

    CANONICAL EVENT HISTORY

Contradictory results require investigation.

Expected result:

    CANONICAL_RECOVERY_CONSISTENCY=PASS

---

## 33. Application Qualification

Application continuity must be evaluated separately.

Possible valid report:

    VRP_SESSION_CONTINUITY=PASS

    APPLICATION_CONTINUITY=PASS

Another possible result:

    VRP_SESSION_CONTINUITY=PASS

    APPLICATION_CONTINUITY=FAIL

For example, an application may enforce its own timeout shorter than the network outage.

The deployment decision should identify which layer failed.

---

## 34. Performance Qualification

Correctness does not automatically imply acceptable performance.

The target deployment should define its own requirements for:

    LATENCY

    RECOVERY TIME

    THROUGHPUT

    CPU

    MEMORY

    MIGRATION RATE

    SESSION SCALE

A result may legitimately be:

    CORRECTNESS=PASS

    PERFORMANCE=FAIL

Such a deployment is not accepted if performance is mandatory.

---

## 35. Performance Cannot Override Correctness

The reverse case is also important:

    PERFORMANCE=PASS

    CANONICAL_CORRECTNESS=FAIL

must not be accepted.

Low latency or high throughput cannot compensate for:

- stale-work acceptance;
- replay acceptance;
- authority rollback;
- canonical divergence.

Correctness gates remain mandatory.

---

## 36. Resource Qualification

Resource behavior should be measured under representative load.

Relevant values may include:

    CPU

    MEMORY

    GOROUTINE / WORKER COUNT

    SESSION COUNT

    TRANSPORT COUNT

    EVIDENCE SIZE

    LONG-RUN GROWTH

The target organization should define acceptable limits.

Project-level correctness testing does not define universal production capacity.

---

## 37. Concurrency Qualification

If the deployment is concurrent, the qualification must be concurrent.

Relevant workloads may include:

- multiple active sessions;
- simultaneous migration;
- recovery while unrelated sessions continue;
- delayed historical work;
- concurrent packet processing;
- authority transitions.

The objective is to verify that concurrency does not invalidate the required state guarantees.

---

## 38. Race Qualification

Relevant test families should be exercised under available race/concurrency diagnostics where practical.

Project-level validation has repeatedly used Go race detection.

Race detection is useful evidence.

It does not replace semantic invariant testing.

Both are required where applicable.

---

## 39. Network Pressure Qualification

The target environment should exercise realistic degraded conditions.

These may include:

    JITTER

    LATENCY SPIKES

    REORDERING

    CONGESTION

    TEMPORARY LOSS

    REPEATED MIGRATION

The required invariants should remain preserved or failure should remain safely contained.

---

## 40. Long-Duration Qualification

Controlled deployment acceptance should not rely exclusively on short tests.

The target environment should include extended execution appropriate to its intended use.

Project-level VRP validation has included:

- approximately hour-scale network turbulence;
- extended link interruption;
- repeated runtime cycles;
- week-scale harness execution.

Target-environment duration requirements remain deployment-specific.

---

## 41. Read-Boundary Qualification

Integration code may consume read-oriented runtime APIs.

The deployment should confirm that it understands the ownership semantics of those APIs.

Reviewed VRP engineering surfaces include:

- managed-session snapshots;
- EventLog snapshots;
- event metadata;
- path-health reads;
- relay-list reads;
- transport-registry list semantics.

Caller mutation must not unexpectedly become canonical runtime mutation unless the API explicitly represents a live runtime object.

---

## 42. Intentional Shared Runtime Objects

Not every exported object is intended to be immutable.

For example, active Transport objects may preserve runtime object identity even when the list containing them is detached.

Similarly, composed runtime Manager and Authority components intentionally preserve shared identity.

Controlled deployment must therefore distinguish:

    SNAPSHOT / READ VALUE

from:

    LIVE RUNTIME COMPONENT

Security and correctness depend on respecting the documented boundary.

---

## 43. Evidence Requirement

Every deployment-blocking acceptance scenario should produce reviewable evidence.

Where applicable, evidence should identify:

    UTC

    BUILD

    SOURCE REVISION

    SOURCE TREE

    ENVIRONMENT

    SCENARIO

    OBSERVATIONS

    COUNTERS

    SUMMARY

    MANIFEST

    VERDICT

The exact structure depends on the scenario.

---

## 44. Evidence Integrity Requirement

Where evidence is integrity-bound:

    ORIGINAL EVIDENCE
          |
          v
    VERIFICATION PASS

while:

    INCONSISTENTLY MODIFIED EVIDENCE
          |
          v
    VERIFICATION FAIL

should hold.

The deployment should not accept a successful verdict if the evidence supporting it fails integrity verification.

---

## 45. Semantic Verification Requirement

Hash correctness alone is insufficient.

The verifier must evaluate scenario meaning where supported.

Examples:

    REPLAY_ACCEPTED == 0

    HISTORICAL_ACCEPTED == 0

    SESSION_BEFORE == SESSION_AFTER

    STALE_AUTHORITY_ACCEPTED == 0

A perfectly hashed evidence set containing a failed invariant is still a failed validation.

---

## 46. Independent Verification Requirement

Where supported by the public verification boundary, deployment evidence should be checked separately from the runtime process that produced it.

Expected relationship:

    RUNTIME EXECUTION
          |
          v
       EVIDENCE
          |
          v
    SEPARATE VERIFIER
          |
          v
       VERDICT

This reduces dependence on self-reported runtime success.

---

## 47. Runtime PASS vs Verification FAIL

The following condition is deployment-blocking until explained:

    RUNTIME_VERDICT=PASS

    INDEPENDENT_VERIFICATION=FAIL

The runtime's own PASS must not override contradictory verification evidence.

The discrepancy must be investigated.

---

## 48. Evidence Verification Is Not Source Verification

Controlled deployment must preserve this distinction:

    INDEPENDENT EVIDENCE VERIFICATION
                !=
    INDEPENDENT SOURCE-CODE VERIFICATION

The public verifier can evaluate the exposed evidence contract.

It does not automatically inspect every protected implementation path.

Organizations requiring source-level assurance should treat that as a separate review requirement.

---

## 49. Security Qualification

Controlled deployment should include the security-relevant invariants applicable to the environment.

At minimum, the core VRP model should evaluate:

    LIFETIME ISOLATION

    REPLAY REJECTION

    CANONICAL ADMISSION

    FAIL-CLOSED BEHAVIOR

Where authority is in scope:

    UNAUTHORIZED AUTHORITY REJECTION

    STALE AUTHORITY REJECTION

    ROLLBACK REJECTION

should also be evaluated.

---

## 50. Security Review Beyond VRP Invariants

VRP invariant validation does not replace deployment security engineering.

A production-oriented deployment may separately require review of:

    HOST HARDENING

    KEY MANAGEMENT

    CREDENTIAL MANAGEMENT

    DEPENDENCIES

    NETWORK POLICY

    DENIAL-OF-SERVICE EXPOSURE

    APPLICATION SECURITY

    OBSERVABILITY

    INCIDENT RESPONSE

These concerns extend beyond the continuity runtime itself.

---

## 51. Host Trust

A compromised execution environment can invalidate assumptions beyond the scope of normal VRP invariant validation.

Controlled deployment should therefore define its host trust model.

VRP evidence alone does not prove that the operating system, operator, or hardware environment is uncompromised.

---

## 52. Denial-of-Service Qualification

Fail-closed admission prevents invalid work from becoming valid canonical work.

It does not imply unlimited resistance to resource exhaustion.

The deployment should separately evaluate relevant limits involving:

    CPU

    MEMORY

    BANDWIDTH

    CONNECTION RATE

    SESSION RATE

    VERIFICATION COST

Availability under hostile load is an environment-specific requirement.

---

## 53. Observability Qualification

The deployment should determine which operational signals are required.

Useful categories may include:

    SESSION STATE

    LIFETIME IDENTITY

    TRANSPORT STATE

    MIGRATION

    AUTHORITY GENERATION

    REJECTION COUNTERS

    RECOVERY

    EVIDENCE VERIFICATION

Observability should help operators distinguish transport failure from canonical-state failure.

---

## 54. Trace Requirements

Where event traces are used, lifecycle identity should remain observable.

A trace containing only:

    SessionID

may be ambiguous across reincarnations.

VRP trace output supports lifecycle context including:

    lifetime_epoch

    incarnation

This helps reviewers distinguish historical and current events.

---

## 55. Operational Failure Classification

Operations should distinguish at least:

    TRANSPORT FAILURE

    SESSION CONTINUITY FAILURE

    AUTHORITY FAILURE

    APPLICATION FAILURE

    PERFORMANCE FAILURE

    EVIDENCE VERIFICATION FAILURE

These conditions require different responses.

A single generic "connection failed" alarm is insufficient for serious continuity diagnosis.

---

## 56. Rollback Planning

Controlled deployment should define what happens if VRP acceptance conditions fail after deployment begins.

A rollback plan should identify:

- how the deployment is stopped;
- how traffic is returned to the previous architecture where applicable;
- how evidence is preserved;
- how affected sessions are handled;
- how the failed condition is investigated.

Rollback planning is an operational requirement, not an admission bypass.

---

## 57. Deployment Expansion

Acceptance for one scope should not automatically authorize unrestricted expansion.

A controlled progression may be:

    LAB

      ->

    CONTROLLED PILOT

      ->

    LIMITED DEPLOYMENT

      ->

    EXPANDED DEPLOYMENT

Each expansion may require additional qualification when scale, topology, workload, or failure exposure materially changes.

---

## 58. Requalification Triggers

Requalification should be considered after material changes involving:

    CANONICAL STATE LOGIC

    LIFECYCLE LOGIC

    AUTHORITY LOGIC

    TRANSPORT INTEGRATION

    RECOVERY LOGIC

    CRYPTOGRAPHIC BOUNDARIES

    EVIDENCE FORMAT

    VERIFIER LOGIC

    MAJOR DEPENDENCIES

    DEPLOYMENT TOPOLOGY

Not every code change requires full requalification.

The scope should match the changed risk boundary.

---

## 59. Defect Handling

A defect discovered during controlled deployment should follow an evidence-oriented process:

    OBSERVE
       |
       v
    PRESERVE EVIDENCE
       |
       v
    REPRODUCE
       |
       v
    ISOLATE
       |
       v
    CORRECT
       |
       v
    ADD REGRESSION
       |
       v
    REQUALIFY

This process has already been used during VRP engineering validation for real state and API-boundary defects.

---

## 60. Defect Discovery Does Not Automatically Invalidate the Entire Architecture

A defect should be evaluated according to its actual boundary.

For example:

    READ-API MUTABLE ALIAS DEFECT

does not automatically mean:

    TRANSPORT CONTINUITY MODEL INVALID

Likewise, a migration failure in one environment does not automatically invalidate replay rejection.

Acceptance decisions should identify the affected invariant rather than converting every defect into an undefined global verdict.

---

## 61. Mandatory Deployment Blockers

The following conditions are deployment-blocking for any scope that depends on the affected invariant:

    STALE LIFETIME WORK ACCEPTED

    REPLAY ACCEPTED AS NEW CANONICAL WORK

    INVALID TRANSITION ACCEPTED

    REJECTED EVENT RECORDED AS CANONICALLY ACCEPTED

    UNEXPECTED SESSION IDENTITY REPLACEMENT

    UNAUTHORIZED AUTHORITY ACCEPTED

    AUTHORITY ROLLBACK ACCEPTED

    CANONICAL STATE / HISTORY DIVERGENCE

    REQUIRED RECOVERY FAILURE

    UNRESOLVED EVIDENCE VERIFICATION FAILURE

These results require investigation before acceptance.

---

## 62. Conditional Issues

Some findings may allow:

    ACCEPTED WITH CONDITIONS

rather than immediate rejection.

Examples may include:

    performance below future scale target

    monitoring integration incomplete

    operational automation incomplete

    application timeout requires tuning

    non-critical observability improvement required

The condition must not conceal failure of a mandatory canonical invariant.

---

## 63. Acceptance Matrix

A controlled deployment can use the following minimum matrix:

| Property | Acceptance Requirement |
|---|---|
| Build identity | Recorded |
| Environment identity | Recorded |
| Baseline | Pass |
| Session establishment | Pass |
| Session / transport separation | Pass |
| Transport interruption | Exercised |
| Transport recovery | Pass |
| Migration | Pass |
| Historical-lifetime rejection | Pass |
| Replay rejection | Pass |
| Duplicate containment | Pass |
| Invalid-transition rejection | Pass |
| Canonical admission atomicity | Pass |
| Terminal-state protection | Pass where exercised |
| Duplicate-create isolation | Pass |
| Recovery | Pass |
| Canonical recovery consistency | Pass |
| Authority validation | Pass when in scope |
| Authority rollback | Pass when in scope |
| Concurrency | Qualified for target workload |
| Network pressure | Qualified |
| Resource use | Within target limits |
| Long-duration execution | Qualified |
| Application behavior | Qualified when required |
| Evidence capture | Pass |
| Evidence integrity | Pass |
| Independent evidence verification | Pass where supported |
| Rollback procedure | Defined |

---

## 64. Core Acceptance Formula

For a deployment depending on VRP continuity, the minimum conceptual acceptance relationship is:

    BASELINE
        AND
    SESSION_CONTINUITY
        AND
    LIFETIME_ISOLATION
        AND
    REPLAY_REJECTION
        AND
    CANONICAL_ADMISSION
        AND
    FAIL_CLOSED_RECOVERY
        AND
    EVIDENCE_CONSISTENCY
        =
    CORE_ACCEPTANCE

Where authority is part of the deployment:

    CORE_ACCEPTANCE
        AND
    AUTHORITY_VALIDATION
        AND
    STALE_AUTHORITY_REJECTION
        AND
    ROLLBACK_REJECTION
        =
    AUTHORITY_AWARE_ACCEPTANCE

---

## 65. ACCEPTED

The deployment may be classified:

    ACCEPTED

when all mandatory requirements for the defined scope pass and no unresolved blocker remains.

The acceptance statement should identify the scope.

Example:

> Accepted for the evaluated build, environment, transports, workload, scale, and failure scenarios.

It should not imply universal certification.

---

## 66. ACCEPTED WITH CONDITIONS

The deployment may be classified:

    ACCEPTED WITH CONDITIONS

when mandatory canonical invariants pass but explicitly documented non-blocking conditions remain.

Every condition should include:

    CONDITION

    OWNER

    REQUIRED ACTION

    ACCEPTANCE IMPACT

where appropriate to the organization.

---

## 67. NOT ACCEPTED

The deployment should be classified:

    NOT ACCEPTED

when a mandatory invariant fails or a deployment-blocking discrepancy remains unresolved.

A negative acceptance result is a valid engineering outcome.

It is preferable to production dependency on an unverified assumption.

---

## 68. Relationship to Existing Transport Technologies

Controlled deployment acceptance should not be based on the claim that VRP invented migration or failover.

Existing technologies already provide important mechanisms including:

    QUIC CONNECTION MIGRATION

    MULTIPATH TCP

    VPN / TUNNEL ROAMING

    APPLICATION RECONNECT

    LOAD-BALANCER FAILOVER

VRP should instead be evaluated on the properties it explicitly owns in the target architecture.

---

## 69. Property-Based Comparison

A useful comparison asks:

    What owns logical session identity?

    What owns transport identity?

    How is a new lifetime distinguished?

    What happens to delayed historical work?

    How is replay rejected?

    Where is canonical state owned?

    How is authority transferred?

    What happens during restart?

    What evidence can be independently verified?

Different technologies may solve different subsets of these problems.

A deployment should use the architecture that satisfies its actual requirements.

---

## 70. VRP May Complement Existing Transports

VRP does not require every underlying transport feature to be reimplemented by VRP.

A deployment may use transport technologies that already provide:

- encryption;
- congestion control;
- multipath;
- migration;
- roaming;
- retransmission.

VRP's evaluated boundary is the continuity and canonical-state model layered around the transport relationship.

This allows deployment architecture to reuse existing transport capabilities where appropriate.

---

## 71. Formal Verification Position

Controlled deployment acceptance is based on engineering evidence.

The current validation program includes:

    REGRESSION TESTING

    ADVERSARIAL SCENARIOS

    FAILURE INJECTION

    CONCURRENCY TESTING

    RACE DETECTION

    REAL NETWORK INTERRUPTION

    LONG-DURATION EXECUTION

    EVIDENCE INTEGRITY TESTING

    INDEPENDENT EVIDENCE VERIFICATION

Unless separately documented, controlled deployment acceptance does not mean complete formal mathematical verification of the runtime.

---

## 72. External Security Review

An organization may require additional assurance beyond the public validation boundary.

Possible requirements include:

    INDEPENDENT SOURCE REVIEW

    CRYPTOGRAPHIC REVIEW

    PENETRATION TESTING

    THREAT-MODEL REVIEW

    FORMAL METHODS

    COMPLIANCE ASSESSMENT

These can complement controlled deployment acceptance.

They are not implied merely by a successful VRP pilot.

---

## 73. Current Project-Level Engineering Record

The current VRP engineering record includes successful validation of scenarios involving:

    REAL TRANSPORT INTERRUPTION

    TRANSPORT REPLACEMENT

    1000 REPEATED MIGRATIONS

    9999 REPLAY ATTEMPTS REJECTED

    128 SESSION INCARNATIONS

    127 HISTORICAL OPERATIONS REJECTED

    CANONICAL ADMISSION ATOMICITY

    TERMINAL RESURRECTION REJECTION

    DUPLICATE-CREATE ISOLATION

    AUTHORITY PROGRESSION

    AUTHORITY ROLLBACK REJECTION

    RESTART-ORIENTED AUTHORITY RECOVERY

    CONCURRENCY

    RACE DETECTION

    LONG-DURATION TURBULENCE

    EVIDENCE MODIFICATION DETECTION

    HISTORICAL REWRITE DETECTION

    INDEPENDENT EVIDENCE VERIFICATION

These results justify controlled evaluation.

They do not eliminate target-environment qualification.

---

## 74. Current Deployment Position

The current public engineering position is:

    IMPLEMENTED CONTINUITY RUNTIME       YES

    CORE INVARIANT VALIDATION            YES

    TRANSPORT FAILURE VALIDATION         YES

    LIFETIME ISOLATION                   YES

    REPLAY REJECTION                     YES

    CANONICAL ADMISSION                  YES

    AUTHORITY VALIDATION                 YES

    RECOVERY VALIDATION                  YES

    CONCURRENCY VALIDATION               YES

    EVIDENCE INTEGRITY                   YES

    INDEPENDENT EVIDENCE VERIFICATION    YES

    CONTROLLED INTEGRATION               APPROPRIATE

    CONTROLLED DEPLOYMENT EVALUATION     APPROPRIATE

    TARGET ENVIRONMENT QUALIFICATION     REQUIRED

    UNIVERSAL PRODUCTION CERTIFICATION   NOT CLAIMED

---

## 75. Acceptance Record

A final controlled-deployment record should contain:

    BUILD IDENTITY

    ENVIRONMENT

    DEPLOYMENT SCOPE

    REQUIRED INVARIANTS

    SCENARIOS EXECUTED

    PERFORMANCE REQUIREMENTS

    SCALE REQUIREMENTS

    OBSERVED RESULTS

    EVIDENCE REFERENCES

    VERIFICATION RESULTS

    KNOWN LIMITATIONS

    OPEN CONDITIONS

    FINAL ACCEPTANCE STATE

This creates an auditable engineering decision rather than an informal statement that testing "looked good."

---

## 76. Final Decision Principle

Controlled deployment acceptance should answer:

> **Did the implementation preserve the required invariants in the environment where we intend to depend on them?**

Not:

> Does the architecture sound plausible?

Not:

> Did a demo look successful?

Not:

> Did the runtime print PASS?

Not:

> Did a different environment pass previously?

The acceptance decision should be based on evidence from the relevant engineering boundary.

---

## Conclusion

VRP has progressed beyond a purely architectural continuity concept.

The implementation has been exercised against:

    transport loss

    transport replacement

    migration

    stale historical work

    replay

    duplicate delivery

    invalid transitions

    terminal resurrection

    duplicate creation

    authority rollback

    restart races

    concurrency

    network turbulence

    mutable API boundaries

    evidence modification

    historical rewrite

The resulting engineering record supports controlled integration and controlled deployment evaluation.

It does not justify a universal claim that every environment is already qualified.

The appropriate deployment position is:

> **Use the existing VRP validation record as the engineering baseline, then deliberately challenge the same required invariants in the actual target environment before depending on them.**

A controlled deployment is accepted when those required properties survive the conditions under which the deployment will actually operate.

That is the acceptance boundary.