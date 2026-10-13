# VRP Pilot Guide

Status: Release Candidate (v1.0.0-rc1)

## Purpose

This document defines the objectives and minimum validation
requirements for a VRP pilot deployment.

The goal of a pilot is to evaluate observable protocol behaviour
under controlled operating conditions.

## Pilot Objectives

A pilot should demonstrate:

- session continuity;
- transport independence;
- deterministic state transitions;
- replay protection;
- successful failover and recovery;
- evidence generation and verification.

## Scope

A pilot validates a specific implementation and configuration.

Results obtained during a pilot must not be generalized beyond
the tested environment without additional supporting evidence.

## Success Criteria

A successful pilot should demonstrate that:

- canonical session identity is preserved;
- transport migration completes as expected;
- replayed messages are rejected;
- unauthorized transitions are rejected;
- recovery preserves canonical session identity;
- generated evidence can be verified.

## Evidence Collection

The pilot should collect:

- runtime logs;
- validation reports;
- protocol evidence;
- PASS/FAIL results;
- implementation version;
- execution environment.

## Deliverables

A completed pilot should produce:

- pilot report;
- verification evidence;
- known limitations;
- recommendations for production deployment.

