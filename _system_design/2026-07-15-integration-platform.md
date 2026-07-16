---
layout: system-design
title: "Design a Integration Platform"
date: 2026-07-15
---

## Requirements

### Functional
- Given a payload, parse and canonicalize to models, validate, transform, run against business rules and route.
- Given an errored payload, attempt parse and canonicalize to models (expect fail), route to review queue, send communication to sender.
- Optional: custom cross-reference configuration, user facing review queue

### Non-Functional
- Reads vastly outnumber writes (100:1 ratio assumed)
- Redirect latency < 50ms at p99
- 100M URLs stored, ~1B redirects/day at scale

### Out of scope for this exercise
- User accounts with authorization/authentication

## High-Level Design

```
External Partners → Ingestion Gateway → Message Queue (RabbitMQ) → Parser Workers/API Workers →
Canonical Domain Model → Validation Pipeline → Transformation Engine → Business Rule Engine →
Routing / Destinations → Internal ERP Warehouse System / Inventory API Accounting
```
