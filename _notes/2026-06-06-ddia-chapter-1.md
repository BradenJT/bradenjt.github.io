---
layout: note
title: "DDIA Chapter 1 — Reliable, Scalable, and Maintainable Applications"
date: 2026-06-06
source: "Designing Data-Intensive Applications — Kleppmann"
---

## Core Idea

Three properties most data systems try to achieve: **reliability** (correct behavior under faults), **scalability** (handling growth), and **maintainability** (operability over time for people who didn't write it).

## Reliability

A system is reliable if it works correctly even when things go wrong. Faults (a component deviating from spec) are not the same as failures (the system stops working). The goal is fault *tolerance*, not fault prevention.

Key insight: it's better to increase fault rates during testing (chaos engineering) than to assume faults won't happen.

## Scalability

Scalability is not a binary property — "can handle more load" isn't specific enough. Useful framing: *describe load* (load parameters), *describe performance* (throughput, latency percentiles), then ask "how does performance change when load increases?"

Percentiles matter more than averages. p99 latency is what your slowest users experience. Tail latency amplification: if a request requires multiple backend calls, the overall response time is the slowest of all of them.

## Maintainability

Three design principles:
- **Operability** — make it easy to run the system (observability, runbooks, deployment automation)
- **Simplicity** — remove accidental complexity (abstraction, not cleverness)
- **Evolvability** — make change easy (testability, loose coupling)

## What I want to follow up on

- How do you define "load parameters" concretely for a given system? What are the right ones for a write-heavy vs. read-heavy system?
- The distinction between latency and response time — Kleppmann is precise here and it's worth internalizing.
