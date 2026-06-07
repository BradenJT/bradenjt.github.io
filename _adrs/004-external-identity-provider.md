---
layout: adr
title: "ADR 0004: External Identity Provider"
date: 2026-06-06
status: Proposed
---

## Context

The platform requires:

- User authentication
- Password management
- MFA support
- Social login support
- Secure account recovery

Building an identity system internally introduces security risks.

## Decision

Use Auth0 or Clerk as the external identity provider.

The application will trust JWT tokens issued by the provider.

Authorization remains application-owned.

## Consequences

### Positive

- Faster development
- Reduced security risk
- Built-in MFA support

### Negative

- Vendor dependency
- Additional recurring cost
