# VRP Controlled Deployment Acceptance

**Project:** Veil Routing Protocol (VRP)  
**Document Type:** Public Controlled Deployment Acceptance Framework  
**Status:** Public  
**Scope:** Controlled Deployment and Integration Evaluation  
**Revision:** October 2026

---

## Purpose

This document defines the public engineering criteria for moving Veil Routing Protocol (VRP) from controlled validation into an environment-specific deployment decision.

It is intended for:

- infrastructure engineers;
- systems engineers;
- network engineers;
- security reviewers;
- integration teams;
- technical evaluators;
- deployment owners.

The objective is to make deployment acceptance evidence-driven.

VRP deployment acceptance should be based on observed behavior in the target environment rather than on architecture documentation alone.

---

# 1. Deployment Principle

The central VRP architectural property remains:

> **Session ≠ Transport**

A controlled deployment should demonstrate that this property remains valid under the actual network and application conditions of the target environment.

The deployment process should therefore evaluate more than nominal connectivity.

It should deliberately exercise:

- transport interruption;
- transport replacement;
- migration;
- recovery;
- replay;
- stale work;
- authority transitions;
- canonical state validation;
- evidence verification.

---

# 2. Current Engineering Position

VRP currently has engineering validation covering:

    SESSION / TRANSPORT SEPARATION

    TRANSPORT MIGRATION

    SESSION LIFETIME ISOLATION

    STALE-WORK REJECTION

    REPLAY REJECTION

    AUTHORITY VALIDATION

    AUTHORITY RECOVERY

    CANONICAL STATE VALIDATION

    CANONICAL EVENT ISOLATION

    RUNTIME RECOVERY

    CONCURRENCY

    RUNTIME PRESSURE

    LONG-DURATION EXECUTION

    EVIDENCE INTEGRITY

    INDEPENDENT EVIDENCE VERIFICATION

These results provide the basis for controlled deployment evaluation.

They do not eliminate the need to qualify the actual deployment environment.

---

# 3. Controlled Deployment

A controlled deployment is an environment where:

- deployment scope is defined;
- expected behavior is documented;
- observability exists;
- failure can be detected;
- recovery can be measured;
- evidence can be preserved;
- operational ownership is known.

The objective is to introduce VRP into a real integration environment without treating unknown behavior as acceptable by default.

---

# 4. Deployment Stages

A recommended progression is:

    STAGE 1
    Environment Definition
        |
        v
    STAGE 2
    Integration
        |
        v
    STAGE 3
    Baseline Qualification
        |
        v
    STAGE 4
    Failure Qualification
        |
        v
    STAGE 5
    Migration Qualification
        |
        v
    STAGE 6
    Recovery Qualification
        |
        v
    STAGE 7
    Security Qualification
        |
        v
    STAGE 8
    Performance Qualification
        |
        v
    STAGE 9
    Long-Duration Qualification
        |
        v
    STAGE 10
    Evidence Review
        |
        v
    DEPLOYMENT ACCEPTANCE DECISION

Each environment may add additional stages.

---

# 5. Environment Definition

Before deployment qualification begins, the target environment should be documented.

Relevant information includes:

- operating system;
- hardware architecture;
- network topology;
- available transports;
- interface configuration;
- relay topology where applicable;
- application type;
- expected session count;
- expected migration frequency;
- expected outage conditions;
- latency expectations;
- throughput requirements;
- recovery requirements.

This establishes the scope of the resulting deployment claim.

---

# 6. Build Identification

The deployment candidate should be identifiable.

Where applicable, the qualification record should include information equivalent to:

    SOURCE_REVISION=<revision>

    SOURCE_TREE=<tree identity>

    BUILD=<build identity>

    UTC=<qualification timestamp>

    ENVIRONMENT=<target environment>

A deployment result should remain associated with the build that was actually evaluated.

---

# 7. Baseline Qualification

Before failure scenarios are introduced, the deployment should pass a baseline.

The baseline should confirm:

- runtime initialization;
- integration availability;
- session establishment;
- expected canonical state;
- evidence generation;
- required observability.

Expected result:

    BASELINE=PASS

Failure at baseline should be resolved before continuity conclusions are made.

---

# 8. Session Establishment Gate

The deployment candidate must demonstrate reliable session establishment.

The evaluator should confirm that:

- a session is created;
- lifecycle progression is valid;
- the expected active state is reached;
- the session can be identified consistently.

Expected result:

    SESSION_ESTABLISHMENT=PASS

---

# 9. Session Identity Gate

The deployment must demonstrate that logical session identity is not accidentally defined by one transport.

A migration scenario should demonstrate:

    SESSION_BEFORE=S

    TRANSPORT_BEFORE=A

followed by:

    SESSION_AFTER=S

    TRANSPORT_AFTER=B

Expected result:

    SESSION_IDENTITY_PRESERVED=YES

---

# 10. Physical Transport Failure Gate

A controlled deployment should exercise actual transport failure where operationally safe.

The evaluator should observe the failure directly.

A representative observation is:

    TRANSPORT_REACHABILITY=LOST

The scenario should not rely solely on simulated success output if a real transport interruption can be safely tested.

---

# 11. Transport Recovery Gate

After the interruption, connectivity should be restored or replacement connectivity provided.

A representative observation is:

    TRANSPORT_REACHABILITY=RECOVERED

The evaluator should then determine whether the logical session behaved according to the continuity requirements.

Expected result:

    TRANSPORT_RECOVERY=PASS

---

# 12. Transport Replacement Gate

The deployment should demonstrate continuity across replacement of the active transport.

The acceptance property is:

> Transport replacement must not automatically require logical session replacement.

Expected result:

    TRANSPORT_REPLACED=YES

    SESSION_IDENTITY_PRESERVED=YES

---

# 13. Migration Gate

Migration must be exercised in the target environment.

The evaluator should verify:

- migration is initiated;
- the replacement path or transport is viable;
- canonical lifecycle progression remains valid;
- migration completes;
- session identity remains coherent.

Expected result:

    MIGRATION=PASS

---

# 14. Repeated Migration Gate

Where migration is an expected operational behavior, repeated migration should be tested.

VRP engineering validation has previously exercised:

    1,000 migrations

while preserving session identity in the recorded scenario.

A deployment should define its own required migration count according to operational expectations.

---

# 15. Network Transition Gate

Where the deployment includes multiple network types, relevant transitions should be exercised.

Examples may include:

    Wi-Fi -> Mobile

    Network A -> Network B

    Path A -> Path B

    Relay A -> Relay B

The deployment should evaluate the transitions actually expected in production.

---

# 16. Replay Gate

The deployment should verify replay rejection through the supported validation surface.

Expected result:

    REPLAY_ACCEPTED=0

Replay volume should be appropriate to the environment and risk model.

VRP engineering validation has previously exercised thousands of replay attempts, including a recorded:

    9,999 replay attempts rejected

scenario.

---

# 17. Duplicate Delivery Gate

Duplicate delivery should not create duplicate canonical execution.

Expected property:

    ACCEPTED WORK=1

    DUPLICATE CANONICAL ACCEPTANCE=0

The deployment should preserve this property under its expected traffic conditions.

---

# 18. Lifetime Isolation Gate

Where session recreation is possible, the deployment should verify isolation between historical and current lifetimes.

Expected property:

    HISTORICAL LIFETIME
            !=
    CURRENT LIFETIME AUTHORITY

Historical work should not regain current authority merely because the logical session identifier matches.

---

# 19. Stale Work Gate

Historical delayed work should be introduced after a newer lifecycle has become current.

Expected result:

    STALE_WORK=REJECTED

and:

    CURRENT_LIFETIME_PRESERVED=YES

---

# 20. Invalid Transition Gate

The deployment should verify that invalid lifecycle transitions fail closed.

Expected result:

    INVALID_TRANSITION=REJECTED

with:

    CANONICAL_STATE_MUTATION=NONE

    CANONICAL_HISTORY_MUTATION=NONE

---

# 21. Terminal State Gate

Where terminal states are part of the integration, the deployment should verify that invalid ordinary work cannot silently resurrect them.

Expected result:

    INVALID_TERMINAL_RESURRECTION=REJECTED

Explicit supported recovery should remain distinguishable from accidental resurrection.

---

# 22. Canonical Admission Gate

Canonical admission must remain fail-closed.

Representative expected behavior:

    missing lifecycle context
        -> REJECTED

    stale lifecycle context
        -> REJECTED

    invalid current transition
        -> REJECTED

    valid current operation
        -> ELIGIBLE FOR ACCEPTANCE

Expected result:

    CANONICAL_ADMISSION=PASS

---

# 23. Canonical History Gate

The deployment should verify consistency between accepted state and canonical event history.

For accepted work:

    EXPECTED STATE CHANGE

    EXPECTED HISTORY CHANGE

    EXACTLY ONE CANONICAL ACCEPTANCE

For rejected work:

    STATE CHANGE=NONE

    HISTORY CHANGE=NONE

    CANONICAL EVENT CHANGE=NONE

Expected result:

    CANONICAL_HISTORY_CONSISTENCY=PASS

---

# 24. Authority Gate

Where authority management is included in the deployment, both authorized and unauthorized transitions should be tested.

Connectivity must not automatically establish authority.

Expected result:

    UNAUTHORIZED_TRANSITION=REJECTED

and:

    VALID_AUTHORIZED_TRANSITION=ACCEPTED

---

# 25. Authority Progression Gate

Where authority generations are exposed through the integration, accepted progression should remain ordered.

A representative scenario is:

    GENERATION=1
        |
        v
    ACCEPTED AUTHORITY TRANSITION
        |
        v
    GENERATION=2

Historical authority should not silently replace the newer accepted generation.

---

# 26. Authority Rollback Gate

The deployment should attempt stale authority reuse where applicable.

Expected result:

    STALE_AUTHORITY=REJECTED

    AUTHORITY_ROLLBACK=REJECTED

This is particularly important around restart and recovery conditions.

---

# 27. Recovery Gate

The deployment must demonstrate recovery from at least one relevant failure scenario.

A representative lifecycle is:

    SESSION_ACTIVE
          |
          v
    TRANSPORT FAILURE
          |
          v
    MIGRATION
          |
          v
    RECOVERY
          |
          v
    SESSION_ACTIVE

Expected result:

    RECOVERY=PASS

---

# 28. Recovery Integrity Gate

Recovery must not bypass security and lifecycle validation.

During recovery:

    replay remains invalid

    stale work remains invalid

    unauthorized authority remains invalid

    invalid state transitions remain invalid

Expected result:

    RECOVERY_ADMISSION=FAIL_CLOSED

---

# 29. Concurrency Gate

Deployments expecting concurrent workloads should perform concurrency qualification.

Relevant workloads may include:

- multiple sessions;
- overlapping path observations;
- concurrent packet processing;
- delayed historical work;
- simultaneous recovery activity.

The workload should reflect expected production behavior.

---

# 30. Race Qualification

Where supported by the engineering environment, relevant runtime tests should be exercised under race detection or equivalent concurrency diagnostics.

The result should supplement invariant testing.

It should not replace it.

---

# 31. Runtime Pressure Gate

The deployment should evaluate behavior under realistic network pressure.

Relevant conditions may include:

- latency variation;
- jitter;
- packet reordering;
- congestion;
- temporary connectivity loss;
- repeated migration.

Expected result:

    REQUIRED_INVARIANTS=PRESERVED

or:

    FAILURE=CONTAINED

according to the defined scenario.

---

# 32. Performance Gate

The deployment organization should define measurable performance requirements.

Relevant metrics may include:

    throughput

    latency

    migration duration

    recovery duration

    CPU consumption

    memory consumption

    session scale

    transport scale

    evidence overhead

VRP does not define one universal acceptable number for all deployments.

---

# 33. Scale Gate

The deployment should test the scale it intends to operate.

A small pilot should not automatically be interpreted as evidence for an unrelated large-scale deployment.

Scale qualification should define:

- concurrent sessions;
- active transports;
- migration frequency;
- traffic volume;
- evidence volume;
- runtime duration.

---

# 34. Long-Duration Gate

Production acceptance should include extended execution.

Long-duration qualification is useful for detecting:

- resource growth;
- delayed race conditions;
- accumulated state inconsistencies;
- historical work interactions;
- repeated migration effects.

VRP engineering validation has previously included week-scale harness execution.

The deployment should choose a duration appropriate to its operational requirements.

---

# 35. Application Gate

Where VRP carries application traffic, application behavior must be measured separately from protocol continuity.

The qualification report should distinguish:

    VRP SESSION RESULT

from:

    APPLICATION RESULT

An application-specific timeout should not automatically be classified as a VRP identity failure.

Likewise, preserved VRP identity should not automatically be represented as proof that every application requirement was satisfied.

---

# 36. Evidence Gate

Important qualification scenarios should produce reviewable evidence.

Depending on the scenario, this may include:

- build identity;
- timestamp;
- environment;
- scenario;
- observable events;
- counters;
- canonical summary;
- manifest;
- integrity information;
- final verdict.

Expected result:

    EVIDENCE_CAPTURE=PASS

---

# 37. Evidence Integrity Gate

Where integrity-bound evidence is generated, the deployment should verify that inconsistent modification is detectable.

Expected result:

    ORIGINAL_EVIDENCE=VERIFIABLE

and:

    INCONSISTENT_MODIFICATION=DETECTED

---

# 38. Independent Verification Gate

Where public verification tooling applies to the scenario, evidence should be evaluated independently of the original runtime execution.

Expected result:

    INDEPENDENT_VERIFICATION=PASS

The scope of the result is the exposed verification contract.

It is not equivalent to complete formal proof of the runtime.

---

# 39. Observability Gate

The deployment should provide enough operational visibility to answer:

    Is the session active?

    Which transport is available?

    Did transport reachability fail?

    Did migration occur?

    Did recovery occur?

    Was stale work rejected?

    Was replay rejected?

    Did authority change?

    Did evidence verification succeed?

A deployment that cannot distinguish these outcomes is difficult to operate safely.

---

# 40. Operational Ownership Gate

Before production acceptance, operational responsibility should be clear.

The organization should identify responsibility for:

- deployment;
- monitoring;
- incident response;
- configuration;
- upgrades;
- evidence retention;
- rollback;
- integration support.

Technology alone does not define operational ownership.

---

# 41. Rollback Gate

A controlled deployment should define a rollback strategy.

The organization should know:

- when rollback is triggered;
- who can authorize it;
- how application traffic is handled;
- what evidence is preserved before rollback;
- how the failed condition will be investigated.

Rollback capability is part of controlled deployment engineering.

---

# 42. Incident Evidence

Unexpected runtime behavior should preserve enough information for investigation.

Useful incident context may include:

    UTC

    BUILD IDENTITY

    ENVIRONMENT

    SESSION OBSERVATIONS

    TRANSPORT OBSERVATIONS

    AUTHORITY OBSERVATIONS

    EVIDENCE VERIFICATION RESULT

The objective is reproducibility where practical.

---

# 43. Failure Policy

The organization should define what constitutes:

    RECOVERABLE FAILURE

    CONTAINED FAILURE

    DEPLOYMENT-BLOCKING FAILURE

For example, replay rejection is expected behavior.

Replay acceptance as new canonical work would be a fundamentally different result.

The deployment policy should distinguish them.

---

# 44. Deployment-Blocking Conditions

The following results should require investigation before unconditional acceptance:

    STALE HISTORICAL WORK ACCEPTED AS CURRENT

    REPLAY ACCEPTED AS NEW CANONICAL WORK

    UNAUTHORIZED AUTHORITY ACCEPTED

    AUTHORITY ROLLBACK ACCEPTED

    REJECTED EVENT ENTERED CANONICAL HISTORY

    CANONICAL STATE / HISTORY DIVERGENCE

    SESSION IDENTITY UNEXPECTEDLY REPLACED DURING SUPPORTED MIGRATION

    REQUIRED RECOVERY FAILED

    EVIDENCE INCONSISTENT WITH CLAIMED RESULT

These conditions directly affect core VRP invariants.

---

# 45. Environment-Specific Issues

Not every failed measurement is necessarily a core protocol invariant failure.

Examples of environment-specific issues include:

- application timeout configuration;
- insufficient infrastructure capacity;
- monitoring integration;
- transport-specific performance;
- operating-system configuration;
- deployment-specific routing;
- external dependency behavior.

These should be classified separately and investigated on their own evidence.

---

# 46. Acceptance Matrix

A controlled deployment may use the following baseline matrix:

    Build identified                         REQUIRED

    Environment documented                   REQUIRED

    Baseline                                 PASS

    Session establishment                    PASS

    Session identity                         PASS

    Physical/controlled interruption         PASS

    Transport recovery                       PASS

    Transport replacement                    PASS

    Migration                                PASS

    Replay rejection                         PASS

    Stale-work rejection                     PASS

    Invalid-transition rejection             PASS

    Canonical admission                      PASS

    Canonical history consistency            PASS

    Recovery                                 PASS

    Evidence capture                         PASS

    Evidence verification                    PASS

    Authority validation                     WHEN IN SCOPE

    Authority rollback                       WHEN IN SCOPE

    Network pressure                         RECOMMENDED

    Concurrency                              RECOMMENDED

    Long-duration execution                  REQUIRED FOR PRODUCTION ACCEPTANCE

    Application qualification                WHEN IN SCOPE

    Performance qualification                ENVIRONMENT SPECIFIC

    Scale qualification                      ENVIRONMENT SPECIFIC

    Operational monitoring                   REQUIRED

    Rollback procedure                       REQUIRED

---

# 47. Acceptance Verdicts

The final deployment review should use explicit language.

Recommended verdict categories are:

## ACCEPTED

The required invariants and environment-specific acceptance gates passed under the evaluated deployment scope.

## CONDITIONAL

Core evaluated invariants passed, but identified environment-specific work remains before unrestricted operation.

## NOT ACCEPTED

One or more mandatory invariants or deployment requirements failed or remain unresolved.

The verdict should always include the evaluated scope.

---

# 48. Evidence-Based Acceptance

The final decision should be traceable to evidence.

The desired relationship is:

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
    ACCEPTANCE DECISION

This prevents deployment approval from depending only on presentation quality or undocumented assumptions.

---

# 49. Existing Engineering Evidence

Before controlled deployment, VRP has already accumulated engineering evidence across:

- real UDP continuity;
- physical network interruption;
- Wi-Fi/mobile transition;
- path migration;
- transport collapse and recovery;
- repeated migration;
- replay flooding;
- stale lifetime rejection;
- repeated reincarnation;
- authority continuity;
- authority rollback rejection;
- canonical recovery;
- resource-bounded execution;
- deterministic concurrency;
- race detection;
- evidence modification detection;
- historical rewrite detection;
- independent evidence verification;
- long-duration execution.

This evidence provides a starting point.

The target environment must still generate its own relevant qualification evidence.

---

# 50. Existing Numerical Validation Examples

Recorded VRP engineering validation includes examples such as:

    9,999 replay attempts rejected

    1,000 transport migrations completed
    with session identity preserved

    128 sequential session incarnations

    127 historical delayed operations rejected

    approximately 41.6 seconds of observed
    physical transport interruption

    1,000 repeated deterministic
    concurrency executions

These numbers describe specific recorded validation scenarios.

They are not universal performance limits or deployment guarantees.

---

# 51. Production Acceptance

Production acceptance should occur only after the target organization has evaluated the properties relevant to its environment.

A production decision should consider:

- functional correctness;
- continuity;
- failure behavior;
- security;
- performance;
- scale;
- observability;
- operations;
- rollback;
- evidence.

The decision belongs to the deploying organization.

---

# 52. No Universal Production Claim

VRP does not claim that one validation environment automatically proves readiness for every possible deployment.

Different environments may have different:

- network behavior;
- latency;
- transport characteristics;
- scale;
- applications;
- operational requirements;
- security requirements.

The deployment acceptance process exists specifically because these differences matter.

---

# 53. Engineering Readiness

The current project-level status can be summarized as:

    CORE ARCHITECTURE                  IMPLEMENTED

    SESSION CONTINUITY                 VALIDATED

    TRANSPORT MIGRATION                VALIDATED

    LIFETIME ISOLATION                 VALIDATED

    REPLAY RESISTANCE                  VALIDATED

    AUTHORITY CONTROL                  VALIDATED

    CANONICAL ADMISSION                VALIDATED

    RECOVERY                           VALIDATED

    CONCURRENCY QUALIFICATION          VALIDATED

    EVIDENCE INTEGRITY                 VALIDATED

    INDEPENDENT VERIFICATION           VALIDATED

    REAL NETWORK VALIDATION            COMPLETED

    CONTROLLED DEPLOYMENT              READY FOR EVALUATION

    TARGET ENVIRONMENT QUALIFICATION   REQUIRED

---

# 54. Acceptance Philosophy

VRP deployment acceptance follows a simple philosophy:

> **Do not ask whether the network failed. Make it fail deliberately and observe whether the required invariants survive.**

A useful deployment evaluation should not avoid:

- transport loss;
- path changes;
- replay;
- stale work;
- migration;
- recovery.

These are precisely the conditions under which the continuity architecture should be evaluated.

---

# 55. Final Acceptance Statement

The accumulated VRP engineering evidence supports progression into controlled deployment evaluation.

The runtime has been exercised against substantial continuity, lifecycle, authority, recovery, concurrency, and evidence-integrity scenarios.

The remaining deployment question is therefore not simply:

> "Has VRP ever worked?"

A more useful question is:

> **"Does VRP preserve the required validated invariants in this specific environment, under the failures and operating conditions that matter to this deployment?"**

That question should be answered with environment-specific evidence.

---

## Overall Status

    ARCHITECTURE
    IMPLEMENTED

    CORE CONTINUITY INVARIANTS
    VALIDATED

    FAILURE / RECOVERY MODEL
    VALIDATED

    PUBLIC EVIDENCE MODEL
    VALIDATED

    CONTROLLED DEPLOYMENT EVALUATION
    READY

    ENVIRONMENT-SPECIFIC ACCEPTANCE
    TO BE DETERMINED BY DEPLOYMENT EVIDENCE

---

## Public Scope

This document defines externally relevant controlled-deployment acceptance criteria and public engineering expectations.

Protected runtime implementation details are intentionally outside its scope.

---

**Veil Routing Protocol (VRP)**  
**Public Controlled Deployment Documentation — October 2026**