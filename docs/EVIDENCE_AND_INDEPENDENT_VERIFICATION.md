# VRP Evidence and Independent Verification

**Project:** Veil Routing Protocol (VRP)  
**Status:** Public engineering verification model  
**Updated:** October 2026

---

## 1. Purpose

This document defines what VRP means by engineering evidence and independent verification.

Its purpose is to avoid an ambiguous claim such as:

> "The runtime printed PASS, therefore the property is proven."

VRP treats runtime execution, evidence generation, evidence integrity, and evidence verification as separate concerns.

The intended relationship is:

    IMPLEMENTATION
         |
         v
    SCENARIO EXECUTION
         |
         v
    OBSERVATIONS
         |
         v
    EVIDENCE
         |
         v
    VERIFICATION
         |
         v
    BOUNDED VERDICT

A verification result is only as strong as the evidence and verification contract it actually evaluates.

---

## 2. Why Evidence Exists

A test result is more useful when another engineer can answer:

- what was executed;
- which build was executed;
- where it was executed;
- what was observed;
- what invariant was expected;
- what evidence supports the result;
- whether the evidence is internally consistent;
- what the resulting verdict actually means.

Without that information, a line such as:

    PASS

has limited engineering value.

VRP evidence exists to make validation results attributable and reviewable.

---

## 3. Evidence Is Not Runtime Correctness

This distinction is fundamental:

    VALID EVIDENCE
        !=
    PROOF THAT THE ENTIRE RUNTIME IS CORRECT

Evidence can support a specific claim.

For example:

    replay attempts = 9999
    replay accepted = 0

may support a replay-rejection verdict for the recorded scenario.

It does not establish:

    ALL POSSIBLE REPLAY ATTACKS ARE IMPOSSIBLE

Similarly:

    migration count = 1000
    session identity preserved = yes

does not prove unlimited migration correctness.

Evidence must remain tied to its scope.

---

## 4. Runtime and Verification Are Separate Boundaries

The runtime performs the operation being evaluated.

The verifier evaluates the evidence exposed by that operation.

Conceptually:

    RUNTIME
       |
       | produces observations
       v
    EVIDENCE BOUNDARY
       |
       v
    VERIFIER

The verifier should not need to reinterpret every internal runtime mechanism.

Instead, it evaluates the public evidence contract associated with the scenario.

This separation allows selected claims to be checked without requiring the verifier to become the runtime itself.

---

## 5. Evidence Context

An evidence bundle may include engineering context such as:

    UTC

    SOURCE REVISION

    SOURCE TREE

    RUNTIME VERSION

    ENVIRONMENT

    SCENARIO

    RUN IDENTIFIER

These fields do not prove correctness.

They answer a different question:

> **Which engineering state produced this evidence?**

That distinction is important for reproducibility.

---

## 6. Scenario Identity

A verdict without a scenario definition is ambiguous.

Evidence should identify what was actually being evaluated.

Examples include:

    TRANSPORT INTERRUPTION

    WIFI -> MOBILE MIGRATION

    REPLAY STORM

    STALE LIFETIME DELIVERY

    AUTHORITY ROLLBACK

    CANONICAL HISTORY VALIDATION

    RECOVERY

A verifier should not infer a stronger scenario than the evidence identifies.

---

## 7. Observable Events

Evidence may contain events describing externally relevant runtime behavior.

Depending on the scenario, examples may include:

    session created

    transport attached

    transport lost

    replacement transport attached

    migration requested

    migration completed

    stale operation rejected

    replay rejected

    authority changed

    recovery completed

The exact event representation depends on the validation surface.

The important requirement is that the verifier knows which observations are part of the verification contract.

---

## 8. Canonical Summary

Raw events alone may not be the most useful verification interface.

A validation scenario may therefore produce a canonical summary containing derived values such as:

    TOTAL_ATTEMPTS

    ACCEPTED

    REJECTED

    SESSION_IDENTITY_BEFORE

    SESSION_IDENTITY_AFTER

    AUTHORITY_GENERATION_BEFORE

    AUTHORITY_GENERATION_AFTER

    FINAL_STATE

    FINAL_VERDICT

A summary should remain consistent with the evidence from which it is derived.

---

## 9. Manifest

A manifest can bind together the artifacts that belong to one validation result.

Conceptually:

    MANIFEST
       |
       +-- scenario
       |
       +-- event evidence
       |
       +-- summary
       |
       +-- hashes
       |
       +-- engineering context

The purpose is not merely packaging.

The manifest gives the verifier an explicit set of artifacts that constitute the evidence being evaluated.

---

## 10. Hashes

Cryptographic hashes can establish that the verifier is reading the same bytes that were bound into the evidence set.

This supports integrity checking.

However:

    HASH MATCH
       !=
    SEMANTIC CORRECTNESS

A hash can show:

> This artifact has not changed relative to the value recorded in the manifest.

It cannot by itself show:

> The runtime behaved correctly.

Semantic validation requires additional checks.

---

## 11. Integrity Verification

Evidence integrity verification asks questions such as:

- are required artifacts present;
- do artifact hashes match;
- is the manifest internally consistent;
- does the summary correspond to the expected evidence set;
- has a bound artifact changed.

This is one layer of verification.

It should not be confused with semantic verification.

---

## 12. Semantic Verification

Semantic verification evaluates the meaning of the recorded evidence.

Examples include:

    replay accepted == 0

or:

    historical operations accepted == 0

or:

    session identity before migration
        ==
    session identity after migration

or:

    authority generation after
        >
    authority generation before

where those relationships are part of the scenario contract.

A useful verifier therefore evaluates more than file existence.

---

## 13. Verdict Derivation

A final verdict should be derived from explicit conditions.

For example, a replay scenario may conceptually require:

    ATTEMPTS > 0
        AND
    ACCEPTED == 0
        AND
    REJECTED == ATTEMPTS

before producing:

    REPLAY_FLOOD_CONTAINED

The exact conditions depend on the scenario.

The important property is:

> The verdict should correspond to observable verification conditions rather than merely being trusted because it appears in a file.

---

## 14. Independent Verification

VRP uses the term **independent verification** in a specific sense.

It means:

> A verifier can evaluate selected exposed evidence without relying solely on the runtime process that originally printed the verdict.

Conceptually:

    RUNTIME EXECUTION
          |
          v
       EVIDENCE
          |
          +------------------+
                             |
                             v
                    SEPARATE VERIFIER
                             |
                             v
                       PASS / FAIL

This reduces dependence on the original runtime's own final message.

---

## 15. What Independent Verification Does Not Mean

The phrase does not imply:

    independent audit of every private source file

or:

    formal mathematical proof of the complete runtime

or:

    certification by an external standards body

or:

    proof against every possible implementation defect

The public claim is narrower:

> Selected exposed validation evidence can be evaluated separately from the execution that generated it.

That is the boundary this document uses.

---

## 16. Replay Evidence

A replay validation may expose evidence equivalent to:

    ATTEMPTS=9999

    REJECTED=9999

    ACCEPTED=0

A verifier can evaluate the required relationship:

    ACCEPTED == 0

and:

    REJECTED == ATTEMPTS

If the evidence instead contains:

    ACCEPTED=1

the replay-rejection verdict must not remain successful.

This is an example of semantic verification.

---

## 17. Lifetime-Isolation Evidence

A stale-lifetime scenario may expose:

    TOTAL_INCARNATIONS=128

    HISTORICAL_OPERATIONS=127

    HISTORICAL_REJECTED=127

    HISTORICAL_ACCEPTED=0

The verifier can check that historical operations did not become current accepted work in that scenario.

The evidence does not prove that every conceivable stale-work pattern has been explored.

It supports the specific tested invariant.

---

## 18. Migration Evidence

Migration evidence may include:

    SESSION_BEFORE

    TRANSPORT_BEFORE

    SESSION_AFTER

    TRANSPORT_AFTER

A continuity condition can then be evaluated as:

    SESSION_BEFORE == SESSION_AFTER

while:

    TRANSPORT_BEFORE != TRANSPORT_AFTER

where that is the expected scenario.

This directly represents the architectural distinction:

    SESSION != TRANSPORT

without claiming that transport migration itself is unique to VRP.

---

## 19. Physical Transport Evidence

A physical interruption scenario may contain observations such as:

    TRANSPORT_REACHABILITY=LOST

followed by:

    TRANSPORT_REACHABILITY=RECOVERED

and a measured outage interval.

This is stronger evidence of physical interruption than merely toggling an internal runtime variable.

It still does not prove what happened outside the scope of the captured observations.

---

## 20. Authority Evidence

Authority evidence may include:

    AUTHORITY_BEFORE

    REQUESTING_ACTOR

    TRANSITION_RESULT

    AUTHORITY_AFTER

    GENERATION_BEFORE

    GENERATION_AFTER

A verifier can then evaluate scenario-specific conditions such as:

    unauthorized transition rejected

or:

    authorized transition accepted

or:

    stale generation rejected

The verifier should not infer authority properties that are absent from the evidence contract.

---

## 21. Recovery Evidence

Recovery evidence may combine:

    transport failure

    replacement availability

    migration

    lifecycle identity

    authority state

    canonical state

    final recovery state

The verifier should evaluate the properties explicitly defined by the scenario.

A successful recovery verdict should not automatically imply zero packet loss, zero latency, or uninterrupted application behavior unless those properties were separately measured.

---

## 22. Evidence Modification Testing

A verification model should be tested against corrupted evidence rather than only valid evidence.

VRP engineering validation has deliberately modified evidence artifacts and then rerun verification.

Tested modifications have included:

- event evidence;
- summary counters;
- manifest-bound hashes;
- historical evidence.

The objective is straightforward:

> If evidence no longer satisfies the verification contract, the verifier should not continue to report the original successful verdict.

---

## 23. Coordinated Modification

A stronger integrity test modifies more than one artifact.

For example:

    event changed

    summary changed to match

    manifest hashes changed

A verifier that only checks one superficial relationship could incorrectly accept such a modified evidence set.

VRP validation has therefore included coordinated evidence-modification scenarios.

These tests evaluate the exposed verification boundary.

They do not turn the evidence system into a substitute for external trust anchors or source-level audit.

---

## 24. Historical Integrity

Evidence may form part of a longer validation history.

A historical integrity check asks whether previously established history can be replaced with a contradictory history without detection.

VRP validation has exercised historical rewrite scenarios.

The tested verification workflow detected inconsistent historical reconstruction.

This supports the bounded claim:

    HISTORICAL_EVIDENCE_INTEGRITY=VALIDATED

for the exercised evidence model.

---

## 25. Evidence Authenticity vs Evidence Integrity

These concepts should not be confused.

### Integrity

Answers:

> Is this artifact consistent with the evidence set or history against which it is being checked?

### Authenticity

Answers:

> Who produced this evidence, and why should that producer identity be trusted?

Hashes and internal consistency primarily address integrity.

Authenticity requires an appropriate trust model.

A deployment should not represent integrity checking alone as proof of organizational or operator identity.

---

## 26. Evidence Does Not Create Trust Automatically

Suppose an evidence bundle is perfectly self-consistent.

That proves neither:

    the host was uncompromised

nor:

    the operator was honest

nor:

    every runtime observation was complete

nor:

    the protected implementation was defect-free

A useful evidence model makes specific claims checkable.

It does not eliminate the need to understand the trust boundary.

---

## 27. Trust Boundary

For public verification, the relevant trust questions include:

- which observations are produced by the runtime;
- which artifacts are bound together;
- which relationships the verifier recalculates;
- which inputs remain trusted;
- which environment facts are externally observable;
- which properties are outside the verifier's scope.

These questions should be explicit during technical review.

---

## 28. Build Binding

A validation result should ideally be associated with the source state that produced it.

Typical identifiers include:

    HEAD=<revision>

    TREE=<tree>

This improves attribution.

It does not mean:

    HASHED BUILD IDENTITY == CORRECT BUILD

It means:

> The evidence identifies the engineering revision against which the result is being claimed.

---

## 29. Environment Binding

Validation evidence may also identify the execution environment.

Examples include:

    operating system

    kernel

    architecture

    Go version

    host/runtime context

Environment identity is useful because behavior may differ across platforms.

A successful result on one environment should not automatically be generalized to every environment.

---

## 30. Time Binding

UTC timestamps help establish when validation occurred.

They are useful for:

- correlating runs;
- identifying evidence sets;
- comparing revisions;
- ordering engineering events.

A timestamp alone is not a secure proof of historical truth.

Its strength depends on the surrounding evidence and trust model.

---

## 31. Deterministic Verification

Where possible, a verifier should produce the same verdict for the same valid evidence.

Deterministic verification improves:

- reproducibility;
- CI integration;
- external review;
- regression testing.

VRP has also used repeated deterministic execution as part of runtime qualification.

Runtime determinism and verifier determinism are related engineering goals but should not be treated as identical properties.

---

## 32. Failure Is a First-Class Verification Result

A useful verifier must be able to fail.

Examples include:

    HASH_MISMATCH

    MANIFEST_INCONSISTENCY

    COUNTER_MISMATCH

    STALE_OPERATION_ACCEPTED

    REPLAY_ACCEPTED

    AUTHORITY_ROLLBACK_ACCEPTED

    HISTORY_REWRITE_DETECTED

    VERDICT_INCONSISTENT

A verification pipeline that always ends in PASS would provide little assurance.

---

## 33. Evidence Preservation

Important validation evidence should be preserved together with enough context to understand it later.

Depending on the scenario, useful preservation includes:

    source identity

    scenario definition

    execution environment

    event evidence

    summary

    manifest

    verifier output

    final verdict

This allows later review without relying entirely on memory or screenshots.

---

## 34. Screenshots Are Supporting Evidence

Terminal screenshots and recordings can be useful.

They can demonstrate that:

- a command was executed;
- physical connectivity was changed;
- a runtime produced an observation;
- a verifier produced a result.

But screenshots alone are weaker than machine-verifiable evidence for many claims.

The strongest public validation combines human-observable demonstration with structured evidence where practical.

---

## 35. Evidence and CI

A public verifier can also be useful in automated engineering workflows.

Conceptually:

    BUILD
      |
      v
    RUN VALIDATION
      |
      v
    PRODUCE EVIDENCE
      |
      v
    VERIFY
      |
      +--> PASS -> continue
      |
      +--> FAIL -> block / investigate

This makes selected invariants suitable for regression gating.

The exact CI integration remains deployment-specific.

---

## 36. Evidence and External Review

An external reviewer should be able to ask:

1. What claim is being made?
2. Which evidence supports it?
3. Which evidence fields are trusted?
4. Which relationships are recalculated?
5. Can the verifier fail?
6. What happens if evidence is modified?
7. Is the result tied to a build?
8. Is the result tied to an environment?
9. What does the verdict not prove?
10. Can the scenario be reproduced?

These questions are more useful than simply asking whether a badge says PASS.

---

## 37. Formal Verification Boundary

VRP engineering validation includes:

- regression testing;
- adversarial scenarios;
- failure injection;
- repeated deterministic execution;
- race detection;
- long-duration execution;
- evidence-integrity testing;
- independent evidence verification.

Unless a specific artifact explicitly establishes otherwise, the project does not claim complete formal verification of the runtime using proof systems or formal specification tools such as:

    TLA+
    Coq
    Lean
    Isabelle

Engineering validation and formal verification are different forms of evidence.

Neither term should be used as a substitute for the other.

---

## 38. Independent Verification and Protected Implementation

The public verification model does not require disclosure of protected runtime implementation details.

That means an external verifier may be able to evaluate:

    exposed evidence

without receiving:

    protected implementation source

This creates an intentional limitation.

The verifier can validate the public evidence contract.

It cannot inspect implementation behavior that is neither represented in the evidence nor otherwise exposed for review.

This limitation should remain explicit.

---

## 39. Evidence Surface

A good public evidence surface should expose enough information to test the public claim without unnecessarily coupling the verifier to internal implementation structure.

This supports two goals:

    REVIEWABILITY

and:

    IMPLEMENTATION BOUNDARY

The evidence contract should therefore focus on externally relevant invariants rather than mirroring private internal objects.

---

## 40. Claim-to-Evidence Mapping

A reviewer should be able to map each important claim to evidence.

Example:

    CLAIM:
    Historical lifetime work is rejected.

    EVIDENCE:
    historical_attempts=127
    historical_rejected=127
    historical_accepted=0

    VERIFICATION:
    historical_accepted == 0
    historical_rejected == historical_attempts

    VERDICT:
    STALE_LIFETIME_REJECTION_PRESERVED

This structure is preferable to unsupported narrative claims.

---

## 41. Evidence-to-Limitation Mapping

Each result should also identify what it does not establish.

Example:

    RESULT:
    1000 migrations completed with session identity preserved.

    SUPPORTS:
    repeated migration path exercised successfully.

    DOES NOT ESTABLISH:
    unlimited migration count
    zero migration latency
    zero packet loss
    correctness on every transport
    correctness on every platform

This prevents numerical results from being interpreted more broadly than intended.

---

## 42. Current Public Evidence Categories

The VRP engineering record includes evidence associated with categories such as:

    TRANSPORT CONTINUITY

    MIGRATION

    PHYSICAL NETWORK INTERRUPTION

    REPLAY REJECTION

    LIFETIME ISOLATION

    CANONICAL ADMISSION

    AUTHORITY CONTINUITY

    AUTHORITY ROLLBACK

    RECOVERY

    CONCURRENCY

    LONG-DURATION EXECUTION

    EVIDENCE INTEGRITY

    HISTORICAL INTEGRITY

    INDEPENDENT VERIFICATION

Not every category uses exactly the same evidence structure.

The verification contract should remain specific to the scenario.

---

## 43. Representative Recorded Results

Examples from the current engineering record include:

    1000 transport migrations
    session identity preserved

    9999 replay attempts
    9999 rejected
    0 accepted

    128 sequential incarnations
    127 historical operations
    127 rejected
    0 accepted

    physical transport interruption
    reachability LOST -> RECOVERED

    deterministic concurrency scenario
    1000 repeated successful executions

These are scenario results.

They are not universal performance specifications.

---

## 44. Relationship to Protocol Comparison

Evidence should also improve comparisons with existing technologies.

Instead of claiming:

> VRP migrates connections better.

a reviewer can ask:

    What identity survives migration?

    How is old lifetime work rejected?

    How is authority represented?

    What becomes canonical history?

    Which evidence demonstrates the result?

This makes comparison with QUIC, Multipath TCP, VPN roaming, or application reconnect mechanisms more precise.

The useful comparison is between explicit properties, not marketing categories.

---

## 45. Verification Does Not Replace Deployment Qualification

A project-level evidence bundle cannot automatically certify a target deployment.

The target environment may differ in:

- network topology;
- transports;
- operating system;
- application behavior;
- scale;
- latency;
- failure characteristics;
- operational controls.

Deployment qualification should therefore generate its own evidence for the properties that matter in that environment.

---

## 46. Recommended External Review Flow

A reviewer evaluating a VRP validation result can use the following sequence:

    1. IDENTIFY CLAIM

    2. IDENTIFY SCENARIO

    3. IDENTIFY BUILD

    4. IDENTIFY ENVIRONMENT

    5. INSPECT EVIDENCE SET

    6. VERIFY INTEGRITY

    7. VERIFY SEMANTIC CONDITIONS

    8. INSPECT VERDICT

    9. CHECK LIMITATIONS

    10. REPRODUCE WHERE PRACTICAL

This makes review repeatable and explicit.

---

## 47. Verification Failure Policy

If verification fails, the successful runtime verdict should not be treated as independently established.

For example:

    RUNTIME_VERDICT=PASS

but:

    INDEPENDENT_VERIFICATION=FAIL

should be reported as a verification failure requiring investigation.

The runtime's own PASS must not override contradictory verification evidence.

---

## 48. Current Verification Position

The current public position can be summarized as:

    STRUCTURED VALIDATION EVIDENCE       AVAILABLE

    BUILD / ENVIRONMENT ATTRIBUTION      SUPPORTED

    INTEGRITY CHECKING                   VALIDATED

    SEMANTIC VERIFICATION                VALIDATED FOR EXPOSED SCENARIOS

    EVIDENCE MODIFICATION TESTING        COMPLETED

    COORDINATED MODIFICATION TESTING     COMPLETED

    HISTORICAL REWRITE DETECTION         VALIDATED

    INDEPENDENT EVIDENCE VERIFICATION    VALIDATED

    INDEPENDENT PRIVATE SOURCE AUDIT     NOT IMPLIED

    COMPLETE FORMAL VERIFICATION         NOT CLAIMED

---

## 49. What the Evidence Model Establishes

The current evidence model supports bounded claims such as:

- a validation result can be tied to an identifiable engineering context;
- selected scenario observations can be preserved in structured evidence;
- artifact integrity can be checked;
- semantic relationships can be recalculated for supported scenarios;
- inconsistent evidence modification can be detected by the tested verifier;
- historical rewrite can be detected by the tested evidence model;
- a separate verifier can evaluate selected claims without trusting only the runtime's printed verdict.

These are concrete engineering properties.

---

## 50. What the Evidence Model Does Not Establish

The evidence model does not establish:

    COMPLETE RUNTIME CORRECTNESS

    COMPLETE HOST INTEGRITY

    COMPLETE OPERATOR TRUSTWORTHINESS

    COMPLETE SOURCE AUTHENTICITY

    COMPLETE OBSERVATION COVERAGE

    FORMAL PROOF OF ALL INVARIANTS

    UNIVERSAL DEPLOYMENT SECURITY

These require different evidence or trust mechanisms.

---

## Conclusion

VRP treats evidence as part of the engineering validation boundary rather than as decoration around a PASS message.

The intended model is:

    CLAIM
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
    INTEGRITY + SEMANTIC VERIFICATION
      |
      v
    BOUNDED VERDICT

The strongest appropriate public statement is:

> **Selected VRP continuity, lifecycle, authority, recovery, and evidence-integrity claims can be evaluated through structured evidence and a verification path separate from the runtime execution that originally produced the result.**

That does not eliminate the need for source review, formal methods, deployment qualification, or independent security assessment where those are required.

It provides a narrower and testable property:

> **The exposed claim does not have to be accepted solely because the runtime says it passed.**