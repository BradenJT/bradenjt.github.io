---
layout: adr
title: "ADR 0003: Shared Database Multi-Tenant Architecture"
date: 2026-06-06
status: Accepted
---

## Context

The platform serves multiple contractor companies.

Each company's data must be isolated from every other company.

Possible approaches:

1. Database per tenant
2. Schema per tenant
3. Shared database with TenantId partitioning

## Decision

Use a shared database with TenantId partitioning.

All business entities will contain a TenantId column.

Application services will enforce tenant filtering.

Future migration paths will be documented if tenant-specific databases become necessary.

## Rationale

Expected customer count is low during initial launch.

Shared databases provide:

- Lower operational costs
- Simpler deployment
- Easier migrations
- Reduced infrastructure complexity

## Consequences

### Positive

- Lower cloud costs
- Easier management
- Faster development

### Negative

- Requires strict tenant isolation controls
- Larger database growth over time
- Potential noisy-neighbor concerns

## Future Considerations

Large enterprise customers may require dedicated databases.
