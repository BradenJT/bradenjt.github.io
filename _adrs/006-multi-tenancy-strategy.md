---
layout: adr
title: "ADR 0006: Multi-tenancy strategy"
date: 2026-06-06
status: Accepted
---

## Context

Platform will host multiple different tenants

Requirements:

- Needs to scale as user count grows or shrinks
- Needs to be cost efficient with little overhead

## Decision

Use pooled multitenancy.

## Rationale

User count will initially be low one central
database will ease overhead technical costs
and allow us to scale as needed.

## Consequences

### Positive

- Maximizes cost efficiency
- Operational agility
- Scales effortlessly
- Easer management
- Automated onboarding

### Negative

- Noisy Neighbor Problem
- Expanded blast radius
- Limited Customization
- Complex cost attribution
