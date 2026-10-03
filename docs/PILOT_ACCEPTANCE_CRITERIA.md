# VRP Pilot Acceptance Criteria

**Project:** Veil Routing Protocol (VRP)  
**Document Type:** Public Pilot Acceptance Criteria  
**Status:** Public  
**Scope:** Controlled Pilot and Deployment Evaluation  
**Revision:** October 2026

---

## Purpose

This document defines a public engineering framework for evaluating Veil Routing Protocol (VRP) during a controlled pilot.

The purpose is to make pilot evaluation concrete.

A VRP pilot should not be considered successful because:

- a demonstration looked convincing;
- a connection remained active once;
- a terminal printed PASS;
- a transport migration completed under ideal conditions.

The pilot should evaluate explicit continuity properties against observable evidence.

This document defines those properties without disclosing protected runtime implementation details.

---

# 1. Pilot Objective

The primary objective of a VRP pilot is to determine whether the runtime preserves the required session continuity properties in the evaluator's environment.

The central architectural property is:

> **Session ≠ Transport**

The pilot should therefore evaluate behavior when transport conditions change.

The evaluator should be able to observe whether:

- session identity remains coherent;
- transport can be replaced;
- migration can complete;
- stale work is rejected;
- replay is rejected;
- authority remains controlled;
- recovery follows the canonical lifecycle;
- evidence remains internally consistent.

---

# 2. Pilot Is an Evaluation

A pilot is not an automatic production certification.

Its purpose is to generate environment-specific evidence.

The expected progression is:

    BASELINE
       |
       v
    INTEGRATION
       |
       v
    FAILURE VALIDATION
       |
       v
    MIGRATION VALIDATION
       |
       v
    RECOVERY VALIDATION
       |
       v
    EVIDENCE VERIFICATION
       |
       v
    ENVIRONMENT-SPECIFIC ASSESSMENT

The final production decision remains with the deploying organization.

---

# 3. Minimum Pilot Preconditions

Before continuity testing begins, the pilot environment should identify:

- target operating environment;
- relevant network interfaces;
- transport types;
- expected application behavior;
- observability requirements;
- expected outage conditions;
- expected migration conditions;
- recovery expectations;
- performance requirements;
- acceptance owner.

The evaluator should also establish which build or revision is being tested.

---

# 4. Baseline Identity

The pilot should begin from an identifiable baseline.

Where applicable, evidence should record information equivalent to:

    UTC timestamp
    source revision
    source tree identity
    runtime version
    operating environment
    scenario identifier

The objective is to ensure that later results can be associated with the actual evaluated runtime state.

---

# 5. Baseline Acceptance Criterion

Before failure injection, the runtime should demonstrate stable nominal operation.

The baseline should confirm:

- runtime initialization succeeds;
- the intended validation scenario can execute;
- a session can be established;
- the expected canonical state is reached;
- evidence collection is functioning.

Expected result:

    BASELINE=PASS

A failed baseline should be investigated before migration or recovery conclusions are drawn.

---

# 6. Session Establishment Criterion

The pilot should demonstrate that a canonical session can be created and progressed through the expected establishment lifecycle.

The evaluator should confirm that the session reaches an accepted operational state.

A representative result is:

    SESSION_ESTABLISHED

followed by:

    SESSION_ACTIVE

The exact application integration may expose different external terminology, but the continuity object must be identifiable.

---

# 7. Session Identity Criterion

The pilot must identify what represents the logical session.

The evaluator should verify that this identity remains stable across the transport changes included in the pilot.

Acceptance objective:

    SESSION BEFORE TRANSPORT CHANGE
                ==
    SESSION AFTER TRANSPORT CHANGE

while:

    TRANSPORT BEFORE
                !=
    TRANSPORT AFTER

where the scenario intentionally replaces transport.

---

# 8. Transport Interruption Criterion

The pilot should include a real or controlled transport interruption.

Examples may include:

- disabling an active interface;
- removing network reachability;
- interrupting the current transport;
- making the current path unavailable.

The evaluator should observe the failure rather than infer it only from a final success result.

A representative observation is:

    TRANSPORT_REACHABILITY=LOST

---

# 9. Transport Recovery Criterion

After transport interruption, the pilot should restore or provide viable connectivity.

A representative observation is:

    TRANSPORT_REACHABILITY=RECOVERED

The evaluator should then determine whether the session continuity model behaved according to the pilot requirements.

---

# 10. Physical Outage Interpretation

A transport outage is allowed to be a real outage.

VRP does not define pilot success as pretending the physical interruption never happened.

Therefore:

    physical transport unavailable
        !=
    packets magically delivered

The relevant question is whether the session continuity model survives the interruption and can continue correctly after viable transport becomes available.

---

# 11. Transport Replacement Criterion

At least one pilot scenario should replace the transport carrying an active session.

The evaluator should observe:

    SESSION S
       |
       +---- TRANSPORT A
       |
       X
    TRANSPORT A unavailable
       |
       v
    TRANSPORT B
       |
       v
    SESSION S

Acceptance objective:

    SESSION_IDENTITY_PRESERVED=YES

---

# 12. Migration Criterion

A successful pilot should include migration rather than only transport restoration.

The evaluator should confirm that:

- migration begins from an identifiable state;
- replacement transport or path is available;
- migration is accepted through the runtime lifecycle;
- the final session state is valid;
- session identity remains coherent.

Expected outcome:

    MIGRATION_COMPLETED=YES

and:

    SESSION_IDENTITY_PRESERVED=YES

---

# 13. Repeated Migration Criterion

A single successful migration provides limited evidence.

Where the pilot environment permits it, migration should be repeated.

VRP engineering validation has previously exercised:

    1,000 migrations

while preserving session identity in the recorded scenario.

A pilot does not necessarily need to reproduce exactly that count.

The evaluator should choose a repetition count appropriate to the deployment environment and risk profile.

---

# 14. Path Change Criterion

If the integration includes multiple viable paths, the pilot should exercise path replacement.

The evaluator should verify that:

- path identity can change;
- the session remains the same logical continuity object;
- the resulting path becomes current through an accepted transition.

Acceptance objective:

    PATH_CHANGED=YES
    SESSION_IDENTITY_PRESERVED=YES

---

# 15. Rebinding Criterion

Where applicable, the pilot should include network rebinding behavior.

The objective is not to test a particular NAT vendor.

The objective is to determine whether externally visible transport/path changes can occur without forcing unnecessary logical session replacement.

Acceptance should be based on observed session continuity.

---

# 16. Replay Criterion

The pilot should include replay validation.

Previously observed work should be presented again through the supported validation surface.

Expected result:

    REPLAY_ACCEPTED=0

and:

    REPLAY_REJECTED>0

The current session should remain coherent.

---

# 17. Replay Volume

The required replay volume depends on the pilot.

VRP engineering validation has previously exercised high-volume replay scenarios, including a recorded validation with:

    REPLAY_ATTEMPTS=9999

    REPLAY_REJECTED=9999

A pilot evaluator may select a different volume appropriate to the environment.

The acceptance property remains:

> Replayed work must not silently become new canonical work.

---

# 18. Duplicate Delivery Criterion

The pilot should verify that duplicate delivery does not cause duplicate canonical acceptance.

Expected relationship:

    ONE VALID OPERATION
           +
    DUPLICATE DELIVERY
           =
    ONE CANONICAL ACCEPTANCE

The exact evidence representation depends on the scenario.

---

# 19. Session Recreation Criterion

Where lifecycle testing is included, the pilot should recreate the same logical session identifier across distinct lifetimes.

The evaluator should verify that historical work remains distinguishable from current work.

The expected property is:

    SAME LOGICAL SESSION ID

does not imply:

    SAME CURRENT LIFETIME AUTHORITY

---

# 20. Stale Lifetime Criterion

The pilot should present work belonging to an earlier session lifetime after a newer lifetime has become current.

Expected result:

    HISTORICAL_WORK=REJECTED

The current lifecycle should remain unchanged.

---

# 21. Multi-Lifetime Criterion

For stronger lifecycle validation, the pilot may exercise multiple successive incarnations.

VRP engineering validation has previously exercised:

    TOTAL_INCARNATIONS=128

    HISTORICAL_OPERATIONS=127

    HISTORICAL_ACCEPTED=0

    HISTORICAL_REJECTED=127

The pilot does not need to reproduce exactly this scale unless required by its acceptance plan.

The essential property is historical isolation.

---

# 22. Invalid Transition Criterion

The pilot should verify that an event invalid for the current canonical state is rejected.

Expected result:

    INVALID_TRANSITION=REJECTED

with:

    CANONICAL_STATE_MUTATION=NONE

    CANONICAL_HISTORY_MUTATION=NONE

---

# 23. Terminal State Criterion

Where lifecycle termination is relevant, the pilot should verify that terminal session states cannot be casually resurrected by ordinary stale or invalid work.

Relevant terminal states may include:

    SESSION_CLOSED

and:

    SESSION_FAILED

Expected result:

    INVALID_RESURRECTION=REJECTED

---

# 24. Fail-Closed Admission Criterion

The pilot should confirm that incomplete lifecycle context is not automatically interpreted as current authority.

Representative behavior:

    missing lifecycle identity
        -> REJECTED

    stale lifecycle identity
        -> REJECTED

    current valid lifecycle identity
        -> eligible for normal validation

Acceptance objective:

    CANONICAL_ADMISSION=FAIL_CLOSED

---

# 25. Authority Criterion

Where authority management is part of the pilot integration, both valid and invalid authority transitions should be exercised.

The evaluator should verify that connectivity alone does not create authority.

The test should include:

- current valid authority;
- an unauthorized transition attempt;
- a valid authorized transition.

---

# 26. Unauthorized Authority Criterion

An operation originating outside the accepted current authority should not automatically become canonical.

Expected result:

    UNAUTHORIZED_AUTHORITY_TRANSITION=REJECTED

The rejection should not corrupt the current session lifecycle.

---

# 27. Authorized Authority Criterion

A valid authority transition should be capable of progressing through the accepted runtime path.

Expected result:

    AUTHORIZED_AUTHORITY_TRANSITION=ACCEPTED

where the scenario otherwise satisfies the runtime contract.

---

# 28. Authority Generation Criterion

Where generation-based authority progression is exposed by the pilot, the evaluator should confirm monotonic progression.

A representative result is:

    INITIAL_GENERATION=1

    FINAL_GENERATION=2

The evaluator should also attempt stale authority reuse where supported.

Expected result:

    STALE_AUTHORITY=REJECTED

---

# 29. Authority Rollback Criterion

The pilot should verify that historical authority cannot silently replace newer accepted authority.

Expected result:

    AUTHORITY_ROLLBACK=REJECTED

This is particularly relevant around restart and recovery scenarios.

---

# 30. Recovery Criterion

After an accepted disruption and migration sequence, the session should reach the expected canonical recovery state.

A representative flow is:

    SESSION_ACTIVE
          |
          v
    TRANSPORT FAILURE
          |
          v
    MIGRATION
          |
          v
    SESSION_RECOVERED
          |
          v
    SESSION_ACTIVE

Acceptance objective:

    RECOVERY_COMPLETED=YES

---

# 31. Canonical Event Criterion

The pilot should verify consistency between accepted operations and canonical evidence.

For an accepted event:

    STATE_DELTA=EXPECTED
    HISTORY_DELTA=1
    CANONICAL_EVENT_DELTA=1

For a rejected event:

    STATE_DELTA=0
    HISTORY_DELTA=0
    CANONICAL_EVENT_DELTA=0

This protects the relationship between runtime state and the evidence describing that state.

---

# 32. Canonical Read Isolation Criterion

Where public integration APIs expose runtime state, the evaluator should verify that read operations do not unintentionally become canonical write operations.

The specific surfaces depend on the integration.

The public acceptance principle is:

> A read representation should not silently provide unintended authority to rewrite canonical state.

---

# 33. Concurrency Criterion

A pilot intended for concurrent workloads should include overlapping runtime activity.

Depending on the deployment, this may include:

- multiple sessions;
- overlapping migrations;
- concurrent path observations;
- concurrent packet activity;
- delayed work arriving during current work.

The evaluator should define a workload appropriate to the expected deployment scale.

---

# 34. Race Qualification

Where the pilot includes build-level engineering qualification, relevant regression scenarios should be exercised under race detection or equivalent concurrency diagnostics where supported.

Race diagnostics are an additional engineering control.

They are not a substitute for runtime invariant validation.

---

# 35. Network Pressure Criterion

The pilot should include degraded network conditions where practical.

Candidate conditions include:

- increased latency;
- jitter;
- packet reordering;
- congestion;
- temporary reachability loss.

The evaluator should determine whether the required continuity properties remain preserved or failure remains safely contained.

---

# 36. Latency Criterion

The pilot should define the latency range relevant to its environment.

The objective is to observe whether increased delay causes:

- incorrect stale-work acceptance;
- invalid migration;
- incorrect recovery;
- canonical divergence.

The pilot should record the tested latency conditions.

---

# 37. Jitter Criterion

Where jitter is operationally relevant, the evaluator should introduce timing variation and observe continuity behavior.

Temporary timing variation should not automatically redefine session identity.

---

# 38. Reordering Criterion

Where supported by the validation environment, reordered delivery should be exercised.

The runtime should continue to distinguish:

- current valid work;
- stale work;
- duplicate work;
- invalid work.

---

# 39. Resource Criterion

The pilot should establish environment-specific resource expectations.

Relevant measurements may include:

- CPU usage;
- memory usage;
- session count;
- transport count;
- migration frequency;
- evidence-generation overhead.

No universal resource threshold is defined by this public document.

The deployment organization should define acceptable limits for its environment.

---

# 40. Long-Duration Criterion

A meaningful pilot should include more than short demonstrations.

The required duration depends on the deployment.

Longer runs are useful for exposing:

- resource growth;
- timing-dependent failures;
- delayed historical work;
- repeated migration effects;
- long-term evidence behavior.

VRP engineering validation has included extended and week-scale harness execution.

---

# 41. Application Continuity Criterion

Where an application is included in the pilot, application behavior should be measured separately from VRP session behavior.

The evaluator should record:

    VRP SESSION RESULT

and:

    APPLICATION RESULT

separately.

This prevents an application-specific timeout from being incorrectly represented as a protocol identity failure, or vice versa.

---

# 42. Evidence Criterion

Each important pilot scenario should produce reviewable evidence.

Depending on the validation surface, evidence may include:

- scenario identity;
- timestamp;
- build identity;
- environment;
- observations;
- counters;
- summary;
- manifest;
- integrity information;
- final verdict.

The evaluator should preserve enough information to review the result later.

---

# 43. Evidence Integrity Criterion

Where integrity-bound evidence is generated, the pilot should verify that modification is detectable.

The evaluator should not assume that a file is trustworthy merely because it was produced by a validation run.

Expected property:

    MODIFIED_EVIDENCE
          |
          v
    VERIFICATION
          |
          X
    INCONSISTENCY DETECTED

---

# 44. Independent Verification Criterion

Where a public verifier is available for the pilot scenario, evidence should be evaluated separately from the original runtime execution.

Expected result:

    EVIDENCE_GENERATED=YES

    INDEPENDENT_VERIFICATION=PASS

The scope of this result is the exposed evidence contract.

It is not a claim of complete formal verification of the protected runtime.

---

# 45. Failure Reporting Criterion

A pilot must be allowed to fail.

If a scenario violates its expected invariant, the result should be preserved and investigated.

A pilot process should not transform:

    FAIL

into:

    PASS

by removing inconvenient observations.

Unexpected behavior is engineering evidence.

---

# 46. Reproduction Criterion

Important failures should be reproduced where practical.

A useful failure record should identify:

- scenario;
- build;
- environment;
- relevant observations;
- expected result;
- actual result.

A reproducible failure is significantly more useful than an unexplained screenshot or isolated statement.

---

# 47. Pilot Acceptance Matrix

A pilot may use the following matrix as a starting point:

    Baseline runtime                         REQUIRED

    Session establishment                    REQUIRED

    Session identity observation             REQUIRED

    Transport interruption                   REQUIRED

    Transport recovery                       REQUIRED

    Transport replacement                    REQUIRED

    Migration                                REQUIRED

    Replay rejection                         REQUIRED

    Stale-lifetime rejection                 REQUIRED

    Invalid transition rejection             REQUIRED

    Canonical history consistency            REQUIRED

    Evidence capture                         REQUIRED

    Evidence verification                    REQUIRED

    Authority validation                     WHEN IN SCOPE

    Repeated migration                       RECOMMENDED

    Network pressure                         RECOMMENDED

    Concurrency qualification                RECOMMENDED

    Long-duration execution                  RECOMMENDED

    Application continuity                   WHEN IN SCOPE

    Performance qualification                ENVIRONMENT SPECIFIC

    Scale qualification                      ENVIRONMENT SPECIFIC

---

# 48. Minimum Acceptance Gate

A controlled pilot should not receive an overall successful engineering result if any mandatory scenario demonstrates unresolved violation of a core invariant.

The minimum acceptance gate is:

    BASELINE=PASS

    SESSION_ESTABLISHMENT=PASS

    SESSION_IDENTITY=PASS

    TRANSPORT_INTERRUPTION_OBSERVED=YES

    TRANSPORT_RECOVERY=PASS

    MIGRATION=PASS

    REPLAY_REJECTION=PASS

    STALE_LIFETIME_REJECTION=PASS

    INVALID_TRANSITION_REJECTION=PASS

    CANONICAL_HISTORY_CONSISTENCY=PASS

    EVIDENCE_CAPTURE=PASS

    EVIDENCE_VERIFICATION=PASS

An environment may define additional mandatory requirements.

---

# 49. Conditional Acceptance

A deployment organization may encounter issues unrelated to a core VRP invariant.

Examples include:

- application timeout tuning;
- monitoring integration;
- infrastructure configuration;
- environment-specific performance limits.

The organization may classify such results separately from protocol-invariant failures.

Any conditional acceptance criteria should be documented explicitly by the evaluator.

---

# 50. Pilot Failure Conditions

Examples of results requiring investigation include:

    stale historical work accepted as current

    replay accepted as new canonical work

    unauthorized authority transition accepted

    rejected event entering canonical history

    session identity unexpectedly replaced during supported migration

    evidence verification inconsistent with claimed result

    unreconciled canonical state divergence

These conditions should not be hidden behind an overall PASS label.

---

# 51. Pilot Report

At completion, the evaluator should produce a concise report containing:

- evaluated environment;
- evaluated build;
- scenarios executed;
- results;
- unresolved failures;
- performance observations;
- evidence references;
- deployment-specific conclusions.

The report should distinguish:

    OBSERVED FACT

from:

    DEPLOYMENT INTERPRETATION

---

# 52. Pilot Success Does Not Mean Universal Certification

A successful pilot establishes evidence for the environment and scenarios actually tested.

It does not automatically establish correctness for:

- another operating system;
- another transport;
- another application;
- another topology;
- another scale;
- another failure model.

The pilot result should therefore remain bound to its evaluation scope.

---

# 53. Production Decision

The production decision belongs to the deploying organization.

VRP can provide:

- runtime behavior;
- validation scenarios;
- evidence;
- verification surfaces;
- engineering documentation.

The deploying organization determines whether those results satisfy its:

- operational requirements;
- security requirements;
- performance requirements;
- reliability requirements;
- compliance requirements.

---

# 54. Recommended Final Pilot Verdicts

A pilot report should use explicit verdicts rather than vague language.

Recommended categories are:

    PASS

The required evaluated invariants were preserved under the tested pilot scope.

    CONDITIONAL

Core evaluated invariants were preserved, but environment-specific integration or operational issues remain.

    FAIL

One or more required invariants were violated or required validation could not be completed.

These verdicts should always be accompanied by the actual evidence and scope.

---

# 55. Current VRP Pilot Position

Existing VRP engineering validation provides evidence across:

- physical transport interruption;
- transport recovery;
- repeated migration;
- replay rejection;
- stale-lifetime rejection;
- authority progression;
- authority rollback rejection;
- canonical event isolation;
- runtime recovery;
- concurrency;
- long-duration execution;
- evidence integrity;
- independent verification.

This provides the engineering basis for controlled external pilot evaluation.

---

# 56. Overall Pilot Acceptance Statement

The appropriate public pilot position is:

> **VRP is ready for controlled evaluation against explicit continuity, lifecycle, authority, recovery, and evidence criteria.**

A successful pilot should demonstrate those properties in the evaluator's actual environment rather than relying solely on previously recorded project results.

The strongest pilot is therefore not a demonstration where nothing fails.

It is one where failures are deliberately introduced, the expected invariants remain observable, invalid work is rejected, recovery is measurable, and the resulting evidence can be independently reviewed.

---

## Public Scope

This document defines externally relevant pilot acceptance criteria.

It intentionally does not describe protected runtime implementation mechanisms.

---

**Veil Routing Protocol (VRP)**  
**Public Pilot Documentation — October 2026**