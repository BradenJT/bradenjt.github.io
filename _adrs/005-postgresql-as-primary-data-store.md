---
layout: adr
title: "ADR 0005: PostgreSQL as Primary Data Store"
date: 2026-06-06
status: Accepted
---

## Context

The platform stores:

- Users
- Projects
- Costs
- Billing data
- Reporting information

Requirements:

- ACID transactions
- Relational data modeling
- Reporting workloads

## Decision

Use PostgreSQL as the primary datastore.

## Rationale

PostgreSQL provides:

- Strong relational support
- Mature ecosystem
- Excellent performance
- JSON support when needed

## Consequences

### Positive

- Reliable transactional consistency
- Familiar operational model
- Cloud portability

### Negative

- Horizontal scaling complexity
