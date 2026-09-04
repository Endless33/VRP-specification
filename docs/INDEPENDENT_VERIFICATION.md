# Independent Verification Guide

**Document Version:** Public v1

**Status:** Active

---

# Purpose

This repository is designed to be independently verifiable.

Every engineering claim presented in the public repository should be reproducible using publicly available source code, public tooling, and the commands described in this document.

No proprietary runtime access is required to verify the public engineering boundary.

The objective is not to ask anyone to trust the project.

The objective is to make independent verification possible.

---

# Verification Philosophy

Engineering evidence is more valuable than engineering claims.

Performance numbers, benchmarks, runtime behavior, and verification reports should be reproducible by independent engineers.

If independent reproduction does not produce comparable results, the evidence should be questioned.

Verification always has priority over assumptions.

---

# Public Verification Scope

The public repository allows verification of:

- Runtime invariants
- Session invariants
- Transition validation
- Event ordering
- Idempotency behavior
- Duplicate protection
- Session isolation
- Parallel execution safety
- Concurrent runtime behavior
- Multipath decision logic
- Benchmark methodology
- Performance measurements
- Evidence generation
- Race detector execution

The repository intentionally does not expose proprietary runtime mechanisms, protected implementation details, internal research, or confidential engineering assets.

---

# Verification Environment

The verification environment should always be documented.

Recommended information includes:

- Operating System
- Go version
- CPU model
- Kernel version
- Repository commit
- Repository branch

Differences between environments may produce different benchmark numbers.

Engineering correctness should remain identical.

---

# Verification Procedure

A typical verification process consists of the following stages.

1. Clone repository.

2. Checkout the desired commit.

3. Format the repository if necessary.

4. Execute targeted tests.

5. Execute benchmark suite.

6. Execute race detector.

7. Compare generated evidence.

8. Review repository changes.

Every verification should produce terminal output that can be independently inspected.

---

# Benchmark Verification

Benchmark execution should always be performed directly from source.

Performance measurements should never be accepted without reproduction.

Expected benchmark reports include information similar to:

- execution time
- memory allocations
- bytes allocated
- benchmark iterations

Performance numbers are engineering observations.

They are not contractual guarantees.

---

# Race Verification

Concurrent execution should always be verified using the Go race detector.

Successful race verification demonstrates that no race conditions were detected during the executed workload.

A successful race detector execution is one engineering checkpoint.

It is not proof of complete correctness.

---

# Performance Evidence

The repository contains reproducible engineering evidence.

Typical evidence includes:

- benchmark baselines
- scaling reports
- optimization reports
- race verification output

Evidence should always be generated from executable source code.

---

# Independent Review

Engineers are encouraged to:

- inspect the source code
- modify benchmark parameters
- execute additional workloads
- introduce failure conditions
- review algorithmic complexity
- compare multiple executions
- inspect generated evidence

Independent review is encouraged.

Blind trust is not.

---

# Reporting Differences

If reproduced results differ significantly, include:

- repository commit
- operating system
- Go version
- processor
- executed command
- complete terminal output

This information allows meaningful engineering discussion.

---

# Engineering Principle

This repository should be evaluated the same way any engineering system should be evaluated:

Read the code.

Run the code.

Measure the behavior.

Inspect the evidence.

Challenge the implementation.

Repeat the verification.

Engineering confidence should come from reproducible evidence.

Not from marketing.