# Frequently Asked Questions for Engineers

**Document Version:** Public v1

**Status:** Active

---

# Purpose

This document answers common technical questions from engineers evaluating the public VRP repository.

The goal is to reduce assumptions and encourage evidence-based evaluation.

---

# Why is this repository public?

Because architecture benefits from independent engineering review.

Public visibility allows engineers to inspect implementation quality, verification methodology, benchmark procedures, and engineering discipline.

---

# Why isn't the entire runtime public?

The repository intentionally separates public architecture from protected implementation.

The public repository demonstrates engineering quality.

It does not transfer proprietary implementation.

---

# Can I verify the published benchmarks?

Yes.

The benchmark source code is included.

Run the benchmarks yourself.

Compare your results with the published evidence.

---

# Can benchmark numbers differ?

Yes.

Different processors, operating systems, compiler versions, and execution environments naturally produce different performance measurements.

Engineering correctness is more important than identical numbers.

---

# How do I know the published results are real?

Do not trust screenshots.

Run the repository.

Execute the tests.

Generate your own evidence.

Compare the results.

Independent verification is the intended workflow.

---

# Can I modify the benchmarks?

Yes.

Increasing workload size, adding new scenarios, or changing execution parameters is encouraged.

Engineering confidence grows through independent experimentation.

---

# Why are race detector results included?

Concurrency bugs are often invisible during normal execution.

Race detector verification provides additional confidence that concurrent execution behaves correctly under the tested workload.

---

# What should I evaluate first?

Recommended order:

1. Read the architecture.

2. Read the implementation.

3. Execute unit tests.

4. Execute benchmarks.

5. Run the race detector.

6. Review generated evidence.

7. Modify workloads.

8. Repeat.

---

# Does the repository prove production readiness?

No.

The repository demonstrates public engineering work.

Production deployment always requires additional validation specific to each environment.

---

# What if my benchmark numbers differ?

Report:

- repository commit
- branch
- Go version
- operating system
- processor
- executed commands
- terminal output

Differences can then be evaluated objectively.

---

# Can I perform independent engineering review?

Yes.

Independent review is encouraged.

Critical analysis supported by reproducible evidence is valuable.

---

# Can I report engineering issues?

Yes.

Useful reports include:

- reproduction steps

- expected behavior

- observed behavior

- benchmark output

- logs

- terminal output

- repository commit

Evidence-based reports are preferred.

---

# Does public code expose proprietary technology?

No.

The public repository intentionally represents only the public engineering boundary.

Protected runtime implementation, proprietary mechanisms, confidential research, and commercial engineering assets remain outside the public repository.

---

# What is the preferred evaluation method?

Engineering evaluation should always follow the same principle:

Read the code.

Run the code.

Measure the behavior.

Inspect the evidence.

Challenge the implementation.

Repeat the verification.

Engineering conclusions should come from reproducible evidence rather than assumptions.

---

# Final Note

This repository was built to be examined.

If you believe something is incorrect, verify it.

If you discover a reproducible issue, report it.

If the engineering withstands independent verification, let the evidence speak for itself.

Engineering credibility is earned through reproducible work.

Not through opinion.