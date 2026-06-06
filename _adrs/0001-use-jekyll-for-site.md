---
layout: adr
title: "ADR 0001: Use Jekyll for the Technical Blog"
date: 2026-06-06
status: Accepted
---

## Context

A technical blog is required to document architectural thinking, publish ADRs, and build a public portfolio. The platform needs to support multiple content types (blog posts, ADRs, system design write-ups, notes), be low-cost to host, and avoid operational overhead (no server management, no database maintenance).

GitHub Pages is already available as a free hosting platform and supports Jekyll natively. The alternative would be a self-hosted solution (a Next.js app on Vercel, a CMS like Ghost) or a third-party publishing platform (Substack, Medium, Hashnode).

## Decision

Use Jekyll with GitHub Pages. Collections handle distinct content types. The `minimal` remote theme provides baseline styling without requiring a custom CSS build pipeline. The site is deployed automatically on every push to `main`.

A self-hosted Next.js app would give more control over layout and interactivity, but adds a build pipeline, dependency management, and ongoing maintenance overhead that adds no value for a low-traffic static site. Third-party platforms solve hosting but constrain content structure — ADRs with structured front matter fields would require workarounds.

## Consequences

- Zero hosting cost. No server to manage.
- Deploys automatically on push to `main`.
- Content is plain Markdown in a git repository — fully portable.
- Limited to GitHub Pages plugin whitelist. Custom plugins require a CI build step (not needed now).
- No dynamic content (comments, search) without third-party services. Acceptable for this use case.
- Jekyll is mature but not actively developed. Risk is low for a static site with stable requirements.
