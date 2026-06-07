---
layout: adr
title: "ADR 0007: Identity Provider"
date: 2026-06-06
status: Accepted
---

## Context

The platform needs to be secure and offer a service that securely
stores, verifies, and manages identities for users, devices, and software.

Requirements:

- Must offer MFA
- OAuth 2.0
- User Provisioning/De-provisioning

## Decision

Use Firebase Authentication

## Rationale

- Firebase offers an out of the box multi platform support.
- Pre-built UI Libraries
- Enterprise level security
- Seamless integration

## Consequences

### Positive

- Robust security
- Easy implementation
- Social & Federated Logins

### Negative

- Vendor lock-in
- limited server-side password controls
- difficult executing advanced user management tasks
