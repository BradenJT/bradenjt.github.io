---
layout: adr
title: "ADR 0017: Event-Driven Architecture Approach"
date: 2026-06-06
status: Accepted
---

## Context

The platform contains workflows that should not block user requests.

Examples include:

- Billing events
- Audit logging
- Notifications
- Reporting updates

## Decision

Use a lightweight event-driven architecture with domain events and message queues.

## Rationale

- Reduces coupling between services
- Improves scalability
- Supports future service decomposition
- Enables asynchronous processing

## Consequences

### Positive

- Improved system flexibility
- Better scalability
- Reduced request latency

### Negative

- Increased operational complexity
- Eventual consistency considerations
- More difficult debugging
