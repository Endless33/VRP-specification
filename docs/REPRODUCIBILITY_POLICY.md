# Reproducibility Policy

**Document Version:** Public v1

**Status:** Active

---

# Purpose

The purpose of this document is to describe how public engineering results in this repository should be reproduced.

Engineering claims are expected to be supported by independently repeatable execution.

This policy defines the expectations for reproducibility rather than guaranteeing identical numerical results across all hardware.

---

# Engineering Principle

The repository is designed around a simple principle:

If a public engineering claim cannot be reproduced, it should not be trusted.

Every published benchmark, validation report, or engineering result should be treated as reproducible evidence rather than a marketing statement.

---

# Expected Reproducibility

Independent engineers should be able to reproduce:

- Unit test results
- Integration test results
- Runtime verification
- Session verification
- Multipath verification
- Benchmark execution
- Race detector execution
- Evidence generation

Minor numerical differences between environments are expected.

Engineering correctness should remain unchanged.

---

# Sources of Variation

Benchmark values may vary because of:

- CPU architecture
- Processor frequency
- Memory subsystem
- Operating system
- Kernel scheduler
- Go compiler version
- Background workload
- Virtualization
- Power management

These differences are normal.

They do not invalidate engineering methodology.

---

# Engineering Invariants

Regardless of execution environment, the following properties should remain stable:

- Correct runtime behavior
- Deterministic state transitions
- Session isolation
- Idempotent operations
- Duplicate protection
- Transition validation
- Event ordering
- Multipath correctness
- Successful race verification

These properties are significantly more important than absolute benchmark numbers.

---

# Evidence Generation

Verification should produce engineering evidence including:

- benchmark output
- race detector output
- repository commit
- environment information
- terminal logs

Evidence should always be generated directly from executable source code.

---

# Independent Validation

Independent reviewers are encouraged to:

- execute the published benchmarks
- modify workload sizes
- inspect implementation details
- introduce additional test scenarios
- compare multiple executions
- review generated evidence

Independent verification strengthens engineering confidence.

---

# Reporting Differences

If reproduced results differ significantly, include:

- repository commit
- branch
- operating system
- kernel version
- Go version
- processor
- executed commands
- complete terminal output

Providing complete context makes engineering discussion meaningful and reproducible.

---

# Continuous Verification

Engineering verification is not considered a one-time activity.

As the repository evolves:

- benchmarks may be expanded
- verification procedures may improve
- evidence may become more detailed
- additional validation scenarios may be introduced

Every meaningful engineering change should be followed by renewed verification.

---

# Final Principle

Reproducibility is a core engineering requirement of this repository.

Every public result should be open to independent verification.

Engineering confidence should be built through repeated measurement, transparent methodology, and reproducible evidence.

Trust is optional.

Verification is expected.