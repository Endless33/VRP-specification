# VRP Evidence and Independent Verification

**Project:** Veil Routing Protocol (VRP)  
**Document Type:** Public Evidence and Verification Model  
**Status:** Public  
**Validation Scope:** Engineering evidence available as of October 2026

---

## Purpose

This document describes the public evidence and verification model used by Veil Routing Protocol (VRP).

Its purpose is to explain what an external evaluator can verify without requiring access to protected runtime implementation details.

VRP distinguishes between:

- implementation;
- runtime observation;
- evidence;
- verification;
- engineering claims.

A claim about continuity should be supported by evidence that can be inspected independently of the claim itself.

---

# 1. Evidence Principle

The public validation principle is:

> **Do not rely on a PASS message when the underlying execution can provide stronger evidence.**

A useful validation record should answer questions such as:

- What was executed?
- When was it executed?
- Which build or revision was evaluated?
- What environment produced the result?
- What observable events occurred?
- What invariant was evaluated?
- What final verdict was produced?
- Can the resulting evidence be checked for consistency?

The evidence model exists to make engineering claims more reproducible and reviewable.

---

# 2. Evidence Is Not the Runtime

VRP separates runtime implementation from validation evidence.

The runtime performs the operational work.

The evidence surface records externally relevant observations about that work.

The verifier evaluates the evidence.

Conceptually:

    RUNTIME
       |
       v
    OBSERVABLE EVENTS
       |
       v
    EVIDENCE
       |
       v
    VERIFICATION
       |
       v
    VERDICT

The public verification boundary does not require disclosure of the complete runtime implementation.

---

# 3. What Evidence May Contain

Depending on the validation scenario, a VRP evidence bundle may contain information such as:

- validation identifier;
- UTC timestamp;
- source revision;
- source tree identity;
- runtime environment information;
- scenario identity;
- observable runtime events;
- canonical summaries;
- counters;
- manifests;
- integrity hashes;
- verification output;
- final verdict.

Not every validation scenario requires every field.

The evidence should contain enough context to identify what was actually evaluated.

---

# 4. Build Identity

Validation evidence should be associated with a specific engineering state where practical.

Recorded validation commonly captures identifiers such as:

    HEAD=<source revision>

and:

    TREE=<source tree identity>

This prevents an evidence result from being presented without any connection to the build that generated it.

---

# 5. Environment Identity

Environment information may also be recorded.

Examples include:

- operating system;
- runtime version;
- architecture;
- host context;
- validation timestamp.

A typical validation context may therefore contain information equivalent to:

    UTC=<timestamp>
    HEAD=<revision>
    TREE=<tree>
    GO=<runtime version>
    PLATFORM=<environment>

The exact environment fields depend on the validation scenario.

---

# 6. Scenario Identity

Evidence should identify the scenario being evaluated.

Examples of public validation categories include:

- transport interruption;
- transport migration;
- replay rejection;
- stale-lifetime rejection;
- authority transition;
- recovery;
- evidence integrity;
- historical integrity;
- concurrency;
- long-duration execution.

This prevents unrelated successful executions from being substituted for the scenario actually being claimed.

---

# 7. Observable Events

A validation scenario may produce observable events describing externally relevant runtime behavior.

Examples include:

    TRANSPORT_REACHABILITY=LOST

    TRANSPORT_REACHABILITY=RECOVERED

    REPLAY_REJECTED

    STALE_WORK_REJECTED

    MIGRATION_COMPLETED

    SESSION_ACTIVE

The public evidence model focuses on the observable result rather than requiring publication of the internal mechanism that produced it.

---

# 8. Canonical Summary

Some validation scenarios produce a canonical summary.

A summary may describe results such as:

    attempts
    accepted
    rejected
    migrations
    lifecycle state
    authority generation
    final verdict

The summary is useful for review but should not automatically be treated as sufficient evidence by itself.

Where stronger evidence exists, the summary should remain consistent with that evidence.

---

# 9. Evidence Manifest

Evidence bundles may include a manifest describing the artifacts that belong to the validation result.

The manifest can provide a stable inventory of the evidence being verified.

Conceptually:

    EVIDENCE BUNDLE
        |
        +-- execution context
        |
        +-- observations
        |
        +-- summary
        |
        +-- integrity information
        |
        +-- verification result
        |
        +-- manifest

The exact bundle structure may vary between validation tools.

---

# 10. Integrity Hashes

Cryptographic hashes may be used to bind evidence artifacts to expected content.

Their purpose in the public evidence workflow is to detect unexpected modification of evidence inputs.

The important public property is:

> If evidence content changes after the recorded validation, verification should not silently treat the modified content as identical to the original evidence.

The use of hashes does not by itself prove that the runtime was correct.

It provides integrity information about the evidence being evaluated.

---

# 11. Verification Is More Than Hash Comparison

VRP evidence verification is not intended to mean only:

    hash matches -> everything is correct

Integrity checking and semantic validation are different responsibilities.

A verification process may evaluate:

- artifact integrity;
- manifest consistency;
- event consistency;
- summary consistency;
- historical consistency;
- expected invariant outcomes.

Therefore:

    INTEGRITY VALID
        !=
    RUNTIME GUARANTEE AUTOMATICALLY PROVEN

The final engineering conclusion depends on the validation scenario and the properties being evaluated.

---

# 12. Evidence Modification Validation

VRP engineering work has included deliberate modification of evidence inputs.

The purpose is to test whether inconsistent evidence can still receive an apparently valid result.

Tested categories have included modifications to:

- event evidence;
- summary information;
- manifest-bound information;
- historical evidence.

The corresponding verification paths detected inconsistency in the exercised scenarios.

---

# 13. Coordinated Evidence Modification

Validation has also considered cases where more than one evidence component is modified.

This is important because checking only one file independently can miss inconsistencies introduced across multiple artifacts.

Engineering validation has exercised coordinated modifications involving combinations of:

- event records;
- summary values;
- manifest-bound integrity information.

The tested verification workflow detected the resulting inconsistency.

---

# 14. Historical Integrity

Evidence verification also includes historical consistency.

A later evidence state should not silently redefine an already established historical state without detection.

Historical-integrity validation has exercised attempts to modify earlier evidence history.

The tested verification process detected inconsistent historical reconstruction.

---

# 15. Historical Rewrite Rejection

A historical rewrite scenario conceptually looks like:

    ORIGINAL HISTORY
          |
          v
    ESTABLISHED EVIDENCE
          |
          v
    HISTORICAL MODIFICATION
          |
          v
    VERIFICATION
          |
          X
    INCONSISTENCY DETECTED

This provides a public validation property around the integrity of recorded history.

---

# 16. Independent Verification

Independent verification means that the verification step can be performed separately from the protected runtime execution that originally produced the evidence.

Conceptually:

    PROTECTED RUNTIME
          |
          v
    PUBLIC EVIDENCE BOUNDARY
          |
          v
    INDEPENDENT VERIFIER
          |
          v
    VERIFICATION RESULT

The verifier does not need unrestricted access to protected runtime internals in order to evaluate the properties intentionally represented by the public evidence boundary.

---

# 17. What Independent Verification Can Establish

Depending on the scenario, an external verifier may be able to establish that:

- expected evidence artifacts are present;
- evidence integrity checks succeed;
- manifest relationships are consistent;
- observed counters are internally consistent;
- stale operations were recorded as rejected;
- replay attempts were recorded as rejected;
- lifecycle identity remained consistent;
- migration counts match expected evidence;
- authority progression is consistent;
- historical evidence remains consistent;
- the expected public verdict follows from the exposed validation data.

The exact properties depend on the verifier and scenario.

---

# 18. What Independent Verification Does Not Establish

Independent verification should not be overstated.

A public evidence verifier does not automatically prove:

- correctness of every line of protected runtime code;
- absence of every possible implementation defect;
- correctness under every untested environment;
- correctness under every possible failure combination;
- universal security of the entire deployment;
- formal mathematical correctness of the implementation.

The verification claim is limited to the evidence and invariants exposed through the validation boundary.

---

# 19. Verification Without Source Disclosure

VRP intentionally separates:

    SOURCE DISCLOSURE

from:

    EVIDENCE VERIFICATION

An evaluator may verify a runtime claim without receiving unrestricted access to the implementation that produced the runtime behavior.

This model is useful when:

- implementation contains protected engineering work;
- integration partners need observable guarantees;
- evaluators need reproducible validation;
- evidence must be reviewable outside the runtime environment.

The public contract is therefore based on observable properties.

---

# 20. Example: Replay Validation

A replay validation may expose evidence equivalent to:

    SCENARIO=REPLAY_VALIDATION

    REPLAY_ATTEMPTS=9999

    REPLAY_ACCEPTED=0

    REPLAY_REJECTED=9999

    SESSION_IDENTITY_PRESERVED=YES

    FINAL_VERDICT=PASS

A verifier can evaluate whether the evidence presented for the scenario is internally consistent.

The verifier does not need the protected replay-handling implementation in order to inspect the public result.

---

# 21. Example: Lifetime Validation

A lifecycle-isolation validation may expose:

    TOTAL_INCARNATIONS=128

    HISTORICAL_OPERATIONS=127

    HISTORICAL_ACCEPTED=0

    HISTORICAL_REJECTED=127

    CURRENT_LIFETIME_PRESERVED=YES

A verifier can evaluate the exposed lifecycle evidence and resulting verdict.

The internal lifecycle implementation remains outside the public evidence contract.

---

# 22. Example: Migration Validation

A migration validation may expose:

    MIGRATIONS_ATTEMPTED=1000

    MIGRATIONS_COMPLETED=1000

    SESSION_IDENTITY_PRESERVED=YES

    FINAL_VERDICT=PASS

This supports review of the claimed migration result without requiring publication of the migration implementation.

---

# 23. Example: Physical Transport Interruption

A physical network validation may expose observations such as:

    TRANSPORT_REACHABILITY=LOST

followed by:

    TRANSPORT_REACHABILITY=RECOVERED

with an observed outage interval.

The evidence demonstrates that a real connectivity interruption occurred during the evaluated scenario.

The corresponding continuity evidence can then be evaluated against that interruption.

---

# 24. Example: Authority Validation

An authority scenario may expose:

    INITIAL_GENERATION=1

    UNAUTHORIZED_TRANSITION=REJECTED

    AUTHORIZED_TRANSITION=ACCEPTED

    FINAL_GENERATION=2

    FINAL_SESSION_STATE=SESSION_ACTIVE

The verifier can evaluate the externally relevant authority progression without requiring publication of protected authority-management internals.

---

# 25. Deterministic Verification

Verification should produce stable results when evaluating identical valid evidence.

VRP validation work includes deterministic verification scenarios intended to ensure that repeated evaluation of the same evidence does not randomly alternate between contradictory verdicts.

Deterministic behavior is particularly important for:

- CI;
- external review;
- evidence archives;
- pilot qualification;
- reproducibility.

---

# 26. Reproducibility

A useful validation should be reproducible where the public validation boundary permits it.

Reproducibility may involve:

- running a public scenario;
- collecting evidence;
- checking revision identity;
- verifying manifests;
- evaluating the final verdict.

Not every protected runtime operation is publicly reproducible at implementation level.

The intended reproducibility boundary is the exposed validation contract.

---

# 27. Evidence and CI

Evidence verification can be integrated into automated engineering workflows.

A CI-oriented workflow may conceptually perform:

    RUN VALIDATION
          |
          v
    GENERATE EVIDENCE
          |
          v
    VERIFY EVIDENCE
          |
          v
    CHECK EXPECTED VERDICT
          |
          v
    PASS / FAIL

This allows evidence verification to become part of regression qualification rather than a manual afterthought.

---

# 28. Evidence and Pilot Evaluation

During a controlled pilot, evidence can provide a shared engineering boundary between:

- the VRP implementation team;
- the integration team;
- the evaluator.

Instead of relying only on verbal statements, the parties can evaluate concrete results such as:

- migration outcome;
- recovery outcome;
- replay rejection;
- stale-work rejection;
- authority progression;
- evidence consistency.

---

# 29. Evidence and Failure Investigation

Evidence is also useful when a validation fails.

A failed verdict should not automatically be discarded.

It can be used to determine:

- what environment was running;
- which revision was evaluated;
- which observable event differed;
- whether the evidence itself remained consistent;
- whether the failure was reproducible.

This supports engineering investigation without requiring the public evidence format to reveal protected runtime internals.

---

# 30. PASS Is Not the Only Useful Result

A validation system that can only produce PASS is not useful.

A meaningful evidence workflow must allow:

    PASS

and:

    FAIL

and, where appropriate:

    REJECTED

or:

    INCONSISTENT

A failed validation can provide valuable engineering information.

VRP engineering work has used failed regression scenarios to identify and correct runtime-boundary defects.

---

# 31. Evidence Preservation

Important validation results should preserve enough information to be reviewed later.

Depending on the scenario, this may include:

- terminal output;
- evidence directory;
- manifest;
- hashes;
- source revision;
- source tree;
- timestamp;
- environment;
- final verdict.

This helps prevent a strong engineering result from becoming an unverifiable historical anecdote.

---

# 32. Evidence Binding

A validation result is stronger when multiple relevant artifacts are bound into one identifiable evidence set.

Conceptually:

    BUILD IDENTITY
          +
    ENVIRONMENT
          +
    OBSERVATIONS
          +
    SUMMARY
          +
    MANIFEST
          +
    INTEGRITY INFORMATION
          =
    REVIEWABLE VALIDATION RECORD

No individual component should be interpreted as stronger than the validation model actually supports.

---

# 33. Evidence Boundary

The evidence boundary intentionally exposes only information required for validation.

It is not intended to expose:

- protected runtime algorithms;
- private source code;
- secret material;
- internal engineering mechanisms;
- unnecessary implementation state.

The goal is:

> **Maximum useful verification with minimum unnecessary implementation disclosure.**

---

# 34. Evidence Authenticity and Evidence Integrity

Evidence integrity and evidence authenticity are related but distinct concepts.

Integrity asks:

> Has the evaluated evidence changed relative to the integrity information used by the verifier?

Authenticity asks:

> What establishes who or what produced the evidence?

A hash alone provides integrity comparison.

It does not automatically establish producer identity.

Deployment environments requiring stronger producer authenticity should define the required trust and attestation model as part of their integration and security review.

VRP does not claim that a standalone hash is equivalent to cryptographic producer authentication.

---

# 35. Formal Verification Scope

VRP engineering validation includes deterministic testing, invariant-oriented testing, concurrency testing, failure injection, evidence verification, and reproducible validation.

These should not be confused with a claim of complete formal verification.

Unless a particular public artifact explicitly states otherwise, VRP does not claim that the complete protected runtime has been formally proven using systems such as:

- TLA+;
- Coq;
- Lean;
- Isabelle;
- equivalent full formal proof systems.

The current evidence model is an engineering validation system.

That distinction is intentional and public.

---

# 36. Trust Boundary

Independent verification reduces the amount of trust required in a simple runtime claim.

It does not eliminate every trust assumption.

An evaluator must still consider:

- the execution environment;
- evidence collection boundary;
- verifier correctness;
- build identity;
- deployment configuration.

The objective is therefore not:

    ZERO TRUST IN EVERYTHING

The objective is:

    EXPLICIT TRUST BOUNDARIES
            +
    REVIEWABLE EVIDENCE
            +
    REPRODUCIBLE VERIFICATION

---

# 37. Public Verification Questions

An external evaluator should be able to ask concrete questions.

For example:

    Was the expected build evaluated?

    Did a real transport interruption occur?

    Was session identity preserved?

    Were replay attempts rejected?

    Was historical work rejected?

    Did authority progress monotonically?

    Did the final canonical state match the scenario expectation?

    Are evidence artifacts internally consistent?

    Does independent verification reproduce the expected verdict?

These are stronger questions than simply asking:

    "Does VRP work?"

---

# 38. Current Verification Coverage

Current public engineering evidence includes validation around:

    transport continuity

    migration

    replay rejection

    stale-lifetime rejection

    authority continuity

    authority rollback rejection

    canonical history integrity

    runtime recovery

    concurrency

    evidence modification detection

    historical rewrite detection

    independent evidence verification

---

# 39. Verification Status

Current public status:

    Evidence generation                     IMPLEMENTED

    Build identity capture                  IMPLEMENTED

    Environment capture                     IMPLEMENTED

    Integrity hashing                       IMPLEMENTED

    Evidence manifests                      IMPLEMENTED

    Evidence consistency verification       VALIDATED

    Evidence modification detection         VALIDATED

    Historical rewrite detection            VALIDATED

    Independent evidence verification       VALIDATED

    Deterministic verification              VALIDATED

    Controlled pilot verification           READY FOR EVALUATION

    Full formal proof of entire runtime      NOT CLAIMED

---

# 40. What an External Reviewer Should Expect

An external reviewer should expect VRP public validation to provide:

- a clearly defined scenario;
- an observable invariant;
- identifiable evidence;
- an explicit verdict;
- enough context to understand what was evaluated;
- verification tooling or verification output where applicable.

The reviewer should not expect public evidence documentation to reveal protected implementation internals unrelated to verification.

---

# 41. What an External Reviewer Should Challenge

Reviewers are encouraged to challenge:

- unclear claims;
- missing environment identity;
- unverifiable counters;
- inconsistent evidence;
- ambiguous lifecycle identity;
- unsupported deployment conclusions;
- results that cannot be reproduced at the advertised validation boundary.

This improves the engineering value of the public validation surface.

---

# 42. Deployment Interpretation

The evidence model supports controlled deployment evaluation by giving evaluators a concrete verification boundary.

It allows a deployment team to evaluate observable runtime properties while maintaining separation between:

    PUBLIC GUARANTEES

and:

    PROTECTED IMPLEMENTATION

The evidence model therefore supports deployment review without requiring unrestricted source disclosure.

---

# 43. Overall Assessment

VRP does not treat documentation alone as proof of runtime behavior.

The project has developed an evidence-oriented validation workflow in which engineering claims can be associated with:

- identifiable builds;
- observable runtime events;
- canonical summaries;
- integrity information;
- verification results;
- explicit verdicts.

Evidence-integrity validation has included deliberate evidence modification and historical rewrite scenarios.

Independent verification has been exercised against exposed evidence artifacts.

The appropriate public conclusion is:

> **VRP provides an engineering evidence boundary through which selected continuity, lifecycle, authority, recovery, and integrity properties can be independently evaluated without requiring disclosure of protected runtime implementation details.**

This is an engineering verification model.

It is not presented as complete formal verification of every runtime behavior.

---

## Public Scope

This document describes the public evidence and verification model.

It intentionally does not disclose protected runtime implementation mechanisms.

---

**Veil Routing Protocol (VRP)**  
**Public Engineering Documentation — October 2026**