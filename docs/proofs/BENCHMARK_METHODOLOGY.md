# Benchmark Methodology

**Document Version:** Public v1

**Status:** Public Engineering

---

# Purpose

This document describes the methodology used to evaluate the public engineering performance of the VRP runtime.

The objective is to make benchmark execution transparent, reproducible, and independently verifiable.

The methodology is considered as important as the benchmark results themselves.

---

# Engineering Principle

Benchmark numbers have little engineering value without a documented execution methodology.

Every published measurement should be reproducible using the public repository.

---

# Evaluation Workflow

Engineering benchmarks follow the same workflow:

1. Clean repository state

2. Build current implementation

3. Execute benchmark

4. Execute correctness verification

5. Execute race verification where appropriate

6. Preserve benchmark output

7. Preserve execution environment

8. Preserve repository commit

---

# Benchmark Environment

Benchmark reports should include whenever practical:

- Go version
- Operating system
- Kernel version
- CPU model
- Repository commit
- Branch
- Benchmark command
- Terminal output

Complete execution context improves reproducibility.

---

# Benchmark Philosophy

Benchmarks are intended to evaluate engineering behavior.

They are not intended to establish universal performance rankings.

Benchmark values naturally vary across environments.

Correctness remains the primary engineering objective.

---

# Repetition

Benchmarks should be executed multiple times.

Independent repetition increases engineering confidence.

Unexpected variation should be investigated rather than ignored.

---

# Workload Scaling

Engineering evaluation should include increasing workloads.

Examples include:

- increasing session count
- increasing transport count
- increasing concurrent workers
- increasing runtime activity

Scalability evaluation should preserve engineering correctness.

---

# Profiling

Profiling may be performed before optimization.

Typical engineering workflow:

Measure

↓

Profile

↓

Optimize

↓

Verify

↓

Benchmark

↓

Compare

↓

Publish

---

# Optimization Policy

Optimization should be based upon measured bottlenecks.

Engineering changes should not be introduced solely because they appear theoretically faster.

Evidence should justify optimization.

---

# Benchmark Interpretation

Absolute benchmark numbers should always be interpreted together with:

- runtime correctness
- invariant preservation
- race detector verification
- engineering evidence

Higher throughput alone is not considered sufficient.

---

# Engineering Evidence

Benchmark execution should preserve:

- benchmark output
- execution environment
- repository commit
- terminal logs
- verification results

Engineering reports become significantly more valuable when complete execution evidence is retained.

---

# Independent Verification

Independent engineers are encouraged to:

- execute identical benchmark commands
- modify workload size
- repeat measurements
- compare generated evidence
- inspect implementation

Independent verification strengthens engineering confidence.

---

# Engineering Assumptions

Benchmark methodology assumes:

- supported implementation
- documented runtime behavior
- successful compilation
- preserved engineering invariants

Execution outside these assumptions is not covered.

---

# Protected Boundary

This methodology describes public engineering execution.

It does not disclose:

- protected runtime implementation
- confidential optimization techniques
- commercial deployment infrastructure
- proprietary engineering processes

---

# Final Principle

Engineering benchmarks should never be evaluated in isolation.

Benchmark numbers acquire engineering value only when combined with reproducible methodology, successful verification, preserved invariants, and independently repeatable execution.