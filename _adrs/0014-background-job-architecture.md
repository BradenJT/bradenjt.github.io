---
layout: adr
title: "ADR 0014: Background Job Architecture"
date: 2026-06-06
status: Accepted
---

## Context

The platform requires execution of long-running and asynchronous workloads.

Examples include:

- Report generation
- Email notifications
- Billing synchronization
- Scheduled maintenance tasks

## Decision

Use Hangfire for background job processing.

## Rationale

- Mature .NET ecosystem support
- Dashboard visibility
- Persistent job storage
- Retry and scheduling capabilities

## Consequences

### Positive

- Improved application responsiveness
- Reliable task execution
- Operational visibility into jobs

### Negative

- Additional infrastructure requirements
- Job management complexity
- Potential database contention