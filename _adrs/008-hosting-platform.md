---
layout: adr
title: "ADR 0008: Hosting Platform"
date: 2026-06-06
status: Accepted
---

## Context

The platform needs to be hosted on a reliable service that is affordable for both the business
and end-user.

Requirements:

- Must offer pay-as-you-go pricing model to scale with usage.
- SSL certificate
- Shared hosting

## Decision

GitHub │ GitHub Actions │ Fly.io │ .NET API (Docker) │ Neon PostgreSQL

## Consequences

### Positive

- Industry Standard
- Tight Integration
- Infrastructure as Code Friendly

### Negative

- Vendor lock-in
- Limited Control
- Cost Growth
