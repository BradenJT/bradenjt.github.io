# Technical Blog Site Design

**Date:** 2026-06-06
**Status:** Approved

## Purpose

A personal site where writing is the primary output and brand is a side effect. Every piece of content serves three audiences simultaneously: personal documentation of thinking, career evidence for a Staff → Principal → Fellow IC trajectory, and a client-facing portfolio for BTC consulting work. The site is not a marketing site — it is a compounding asset.

## Audience

Mixed: engineering peers (technical depth expected), potential clients and hiring managers (judgment and communication quality matter), and self (future reference). Content should be written for peers; the clarity required for that audience also satisfies the broader one.

## Primary Stack

C#, SQL, JS/TS — with deliberate drift as skills expand into distributed systems, cloud architecture, AI integration, and compliance domains.

## Site Structure

### Content Sections

Each section has its own index page and per-item template with format conventions enforced.

#### `/blog`
Narrative posts: opinions, lessons learned, explorations, technical deep-dives. No fixed format — the content determines the structure. Target cadence: ~1/month.

#### `/adrs`
Architecture Decision Records. Structured format required on every entry:
- **Title**: Short imperative description of the decision
- **Date**
- **Status**: Proposed / Accepted / Deprecated / Superseded
- **Context**: What situation prompted this decision
- **Decision**: What was decided and why
- **Consequences**: What becomes easier, harder, or different as a result

Target cadence: ~1/month. These should document real decisions from current work, observed decisions worth analyzing, or hypothetical decisions from system design exercises.

#### `/system-design`
Structured write-ups of system design problems. Each entry includes a diagram and written trade-off analysis covering: requirements scoping, component design, data model, scalability considerations, and trade-offs explicitly named. Target: 5 published in year 1, roughly 1/quarter thereafter.

#### `/notes`
Lightweight book notes and study summaries. Lower polish bar than blog — the goal is externalizing understanding, not publishing polished prose. Primary source material: *Designing Data-Intensive Applications*, *System Design Interview* vols. 1–2, *A Philosophy of Software Design*, and engineering blogs (Netflix, Spotify, Stripe). Published as-needed.

### Pages

#### `/about`
Background, current role and focus, IC trajectory (Staff → Principal → Fellow), brief context on BTC. Written for someone who just landed on the site and needs to understand who you are in under 2 minutes.

#### `/projects`
Portfolio of work. Each entry includes: project name, what it demonstrates (explicit, not implied), tech stack, link to code, and links to any associated ADRs or write-ups. Phase 1 anchor project: multi-tenant SaaS skeleton with full ADR documentation.

#### `/consulting`
BTC positioning and service offerings. Covers: who it's for, current service tier, how to engage, and contact. Should reflect the current phase of the roadmap — service offerings evolve as credibility compounds.

#### `/now`
What is currently being read, built, and learned. Updated ~monthly. Signals active progression through the roadmap phases. Gives new visitors immediate context without requiring them to read the archive.

## Navigation

Primary nav: Blog · ADRs · System Design · Notes · Projects · Consulting · About · Now

## Content Model Notes

- ADRs and system design write-ups are reference artifacts — they should be linkable, stable URLs, and searchable.
- Blog and notes are narrative — chronological index is appropriate.
- Projects should link back to associated ADRs/system design write-ups where they exist, creating a web of connected evidence.

## Deliberately Excluded

- `/talks` — add when there is a talk to publish, not before.
- Comments system — not needed at this stage; engagement happens elsewhere.
- Newsletter/subscription — out of scope for now.
- Tags/categories — resist the urge to over-organize until volume demands it.

## Technology

Jekyll on GitHub Pages. Current theme: `pages-themes/minimal`. Likely needs customization to support multi-section navigation and distinct per-section layouts. Implementation details are deferred to the implementation plan.
