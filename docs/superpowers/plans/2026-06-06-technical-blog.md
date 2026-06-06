# Technical Blog Site Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a multi-section Jekyll GitHub Pages site with distinct content types (Blog, ADRs, System Design, Notes) and static pages (About, Projects, Consulting, Now).

**Architecture:** Jekyll with GitHub Pages. Collections (`_adrs`, `_system-design`, `_notes`) handle structured content types. Standard `_posts` handles the blog. A custom `_layouts/default.html` override adds site-wide navigation on top of the minimal theme. Each content type gets its own index page and layout.

**Tech Stack:** Jekyll (github-pages gem), Ruby/Bundler, Liquid templates, Markdown, YAML front matter.

---

## File Map

| Action | Path | Responsibility |
|--------|------|----------------|
| Create | `Gemfile` | Ruby/Jekyll dependencies |
| Modify | `.gitignore` | Add Jekyll build artifacts |
| Modify | `_config.yml` | Collections, defaults, metadata |
| Create | `_layouts/default.html` | Base HTML with nav — overrides theme |
| Create | `_layouts/page.html` | Generic static page wrapper |
| Create | `_layouts/post.html` | Blog post wrapper |
| Create | `_layouts/adr.html` | ADR with status badge + structured wrapper |
| Create | `_layouts/system-design.html` | System design write-up wrapper |
| Create | `_layouts/note.html` | Study note wrapper |
| Create | `index.md` | Home page |
| Create | `blog/index.html` | Blog section index |
| Create | `adrs/index.html` | ADR section index |
| Create | `system-design/index.html` | System Design section index |
| Create | `notes/index.html` | Notes section index |
| Create | `about.md` | About page |
| Create | `projects.md` | Projects page |
| Create | `consulting.md` | Consulting page |
| Create | `now.md` | Now page |
| Create | `_posts/2026-06-06-hello-world.md` | First blog post (sample) |
| Create | `_adrs/0001-use-jekyll-for-site.md` | First ADR (sample) |
| Create | `_system_design/2026-06-06-url-shortener.md` | First system design write-up (sample) |
| Create | `_notes/2026-06-06-ddia-chapter-1.md` | First note (sample) |

---

## Task 1: Set up local Jekyll development environment

**Files:**
- Create: `Gemfile`
- Modify: `.gitignore`

- [ ] **Step 1: Create `Gemfile`**

```ruby
source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins
gem "webrick"
```

- [ ] **Step 2: Add Jekyll entries to `.gitignore`**

Append to the end of `.gitignore`:

```
# Jekyll
_site/
.sass-cache/
.jekyll-cache/
.jekyll-metadata
Gemfile.lock
vendor/
```

- [ ] **Step 3: Install dependencies**

```powershell
bundle install
```

Expected: Bundler installs `github-pages` and dependencies. No errors. A `Gemfile.lock` file appears (ignored by git).

If Ruby is not installed: download from https://rubyinstaller.org/ (Ruby+Devkit 3.x), run installer, check "Add Ruby to PATH".

- [ ] **Step 4: Verify Jekyll builds**

```powershell
bundle exec jekyll build
```

Expected: `Configuration file: _config.yml`, `Build complete` with no errors. A `_site/` directory is created.

- [ ] **Step 5: Commit**

```bash
git add Gemfile .gitignore
git commit -m "chore: add Jekyll dev environment"
```

---

## Task 2: Update site configuration

**Files:**
- Modify: `_config.yml`

- [ ] **Step 1: Replace `_config.yml` entirely**

```yaml
remote_theme: pages-themes/minimal@v0.2.0

title: Braden Townsell
description: Systems thinking, architectural decisions, and the craft of software.
url: "https://bradenjt.github.io"

author:
  name: Braden Townsell
  email: bradenjparrish@gmail.com

collections:
  adrs:
    output: true
    permalink: /adrs/:name/
  system_design:
    output: true
    permalink: /system-design/:name/
  notes:
    output: true
    permalink: /notes/:name/

defaults:
  - scope:
      path: ""
      type: posts
    values:
      layout: post
  - scope:
      path: ""
      type: adrs
    values:
      layout: adr
  - scope:
      path: ""
      type: system_design
    values:
      layout: system-design
  - scope:
      path: ""
      type: notes
    values:
      layout: note
  - scope:
      path: ""
      type: pages
    values:
      layout: page

permalink: /blog/:year/:month/:day/:title/

plugins:
  - jekyll-remote-theme
  - jekyll-feed
  - jekyll-seo-tag

exclude:
  - Gemfile
  - Gemfile.lock
  - docs/
  - vendor/
  - README.md
```

- [ ] **Step 2: Verify build still succeeds**

```powershell
bundle exec jekyll build
```

Expected: Build completes with no errors. `_site/` is regenerated.

- [ ] **Step 3: Commit**

```bash
git add _config.yml
git commit -m "feat: configure Jekyll collections and site metadata"
```

---

## Task 3: Create base layout with navigation

**Files:**
- Create: `_layouts/default.html`
- Create: `_layouts/page.html`

The `_layouts/default.html` overrides the minimal theme's layout entirely. It must include the theme's stylesheet and provide the global nav.

- [ ] **Step 1: Create `_layouts/default.html`**

```html
<!DOCTYPE html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    {% seo %}
    {% feed_meta %}
    <link rel="stylesheet" href="{{ '/assets/css/style.css' | relative_url }}">
    <style>
      nav ul { list-style: none; padding: 0; margin: 0 0 1rem 0; }
      nav ul li { display: inline-block; margin-right: 0.75rem; }
      .content-meta { color: #666; font-size: 0.9rem; margin-bottom: 1.5rem; }
      .adr-status {
        display: inline-block;
        padding: 0.1rem 0.5rem;
        border-radius: 3px;
        font-size: 0.8rem;
        font-weight: bold;
        text-transform: uppercase;
      }
      .adr-status--accepted { background: #d4edda; color: #155724; }
      .adr-status--proposed { background: #fff3cd; color: #856404; }
      .adr-status--deprecated { background: #f8d7da; color: #721c24; }
      .adr-status--superseded { background: #e2e3e5; color: #383d41; }
    </style>
  </head>
  <body>
    <div class="wrapper">
      <header>
        <h1><a href="{{ '/' | relative_url }}" style="color: inherit; text-decoration: none;">{{ site.title }}</a></h1>
        <p>{{ site.description }}</p>
        <nav>
          <ul>
            <li><a href="{{ '/blog' | relative_url }}">Blog</a></li>
            <li><a href="{{ '/adrs' | relative_url }}">ADRs</a></li>
            <li><a href="{{ '/system-design' | relative_url }}">System Design</a></li>
            <li><a href="{{ '/notes' | relative_url }}">Notes</a></li>
            <li><a href="{{ '/projects' | relative_url }}">Projects</a></li>
            <li><a href="{{ '/consulting' | relative_url }}">Consulting</a></li>
            <li><a href="{{ '/about' | relative_url }}">About</a></li>
            <li><a href="{{ '/now' | relative_url }}">Now</a></li>
          </ul>
        </nav>
      </header>
      <section>
        {{ content }}
      </section>
      <footer>
        <p>{{ site.author.name }}</p>
      </footer>
    </div>
  </body>
</html>
```

- [ ] **Step 2: Create `_layouts/page.html`**

```html
---
layout: default
---
<article>
  <header>
    <h1>{{ page.title }}</h1>
  </header>
  {{ content }}
</article>
```

- [ ] **Step 3: Verify build and nav renders**

```powershell
bundle exec jekyll serve --livereload
```

Open http://localhost:4000. Expected: Site loads with nav links visible in the header. All nav links are present (Blog, ADRs, System Design, Notes, Projects, Consulting, About, Now). Stop the server with `Ctrl+C`.

- [ ] **Step 4: Commit**

```bash
git add _layouts/
git commit -m "feat: add base layout with site-wide navigation"
```

---

## Task 4: Create content-type layouts

**Files:**
- Create: `_layouts/post.html`
- Create: `_layouts/adr.html`
- Create: `_layouts/system-design.html`
- Create: `_layouts/note.html`

Each layout extends `default`. ADRs render a status badge from front matter. All others add metadata (date, optional tags) above the content body.

- [ ] **Step 1: Create `_layouts/post.html`**

```html
---
layout: default
---
<article>
  <header>
    <h1>{{ page.title }}</h1>
    <p class="content-meta">{{ page.date | date: "%B %-d, %Y" }}</p>
  </header>
  {{ content }}
</article>
```

- [ ] **Step 2: Create `_layouts/adr.html`**

```html
---
layout: default
---
<article>
  <header>
    <h1>{{ page.title }}</h1>
    <p class="content-meta">
      {{ page.date | date: "%Y-%m-%d" }} &nbsp;·&nbsp;
      <span class="adr-status adr-status--{{ page.status | downcase }}">{{ page.status }}</span>
    </p>
  </header>
  {{ content }}
</article>
```

- [ ] **Step 3: Create `_layouts/system-design.html`**

```html
---
layout: default
---
<article>
  <header>
    <h1>{{ page.title }}</h1>
    <p class="content-meta">{{ page.date | date: "%B %-d, %Y" }}</p>
  </header>
  {{ content }}
</article>
```

- [ ] **Step 4: Create `_layouts/note.html`**

```html
---
layout: default
---
<article>
  <header>
    <h1>{{ page.title }}</h1>
    <p class="content-meta">{{ page.date | date: "%B %-d, %Y" }}{% if page.source %} &nbsp;·&nbsp; {{ page.source }}{% endif %}</p>
  </header>
  {{ content }}
</article>
```

- [ ] **Step 5: Verify build**

```powershell
bundle exec jekyll build
```

Expected: Build completes with no errors.

- [ ] **Step 6: Commit**

```bash
git add _layouts/
git commit -m "feat: add content-type layouts for posts, ADRs, system design, and notes"
```

---

## Task 5: Create section index pages

**Files:**
- Create: `blog/index.html`
- Create: `adrs/index.html`
- Create: `system-design/index.html`
- Create: `notes/index.html`

Each index lists all items in the section with title, date, and a link.

- [ ] **Step 1: Create `blog/index.html`**

```html
---
layout: default
title: Blog
---
<h1>Blog</h1>
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <span class="content-meta"> — {{ post.date | date: "%B %-d, %Y" }}</span>
    </li>
  {% endfor %}
</ul>
```

- [ ] **Step 2: Create `adrs/index.html`**

```html
---
layout: default
title: ADRs
---
<h1>Architecture Decision Records</h1>
<p>A log of significant architectural decisions made in projects I've worked on or studied.</p>
<ul>
  {% assign sorted_adrs = site.adrs | sort: "path" %}
  {% for adr in sorted_adrs %}
    <li>
      <a href="{{ adr.url | relative_url }}">{{ adr.title }}</a>
      <span class="content-meta"> — {{ adr.date | date: "%Y-%m-%d" }} · {{ adr.status }}</span>
    </li>
  {% endfor %}
</ul>
```

- [ ] **Step 3: Create `system-design/index.html`**

```html
---
layout: default
title: System Design
---
<h1>System Design</h1>
<p>Structured write-ups of system design problems: requirements, architecture, trade-offs.</p>
<ul>
  {% for item in site.system_design %}
    <li>
      <a href="{{ item.url | relative_url }}">{{ item.title }}</a>
      <span class="content-meta"> — {{ item.date | date: "%B %-d, %Y" }}</span>
    </li>
  {% endfor %}
</ul>
```

- [ ] **Step 4: Create `notes/index.html`**

```html
---
layout: default
title: Notes
---
<h1>Notes</h1>
<p>Book notes and study summaries. Lower polish bar — the goal is externalizing understanding.</p>
<ul>
  {% for note in site.notes %}
    <li>
      <a href="{{ note.url | relative_url }}">{{ note.title }}</a>
      <span class="content-meta"> — {{ note.date | date: "%B %-d, %Y" }}</span>
    </li>
  {% endfor %}
</ul>
```

- [ ] **Step 5: Verify all index pages exist in build output**

```powershell
bundle exec jekyll build
```

Then verify:

```powershell
Test-Path _site/blog/index.html    # True
Test-Path _site/adrs/index.html    # True
Test-Path _site/system-design/index.html   # True
Test-Path _site/notes/index.html   # True
```

- [ ] **Step 6: Commit**

```bash
git add blog/ adrs/ system-design/ notes/
git commit -m "feat: add section index pages for all content types"
```

---

## Task 6: Create static pages

**Files:**
- Create: `index.md`
- Create: `about.md`
- Create: `projects.md`
- Create: `consulting.md`
- Create: `now.md`

- [ ] **Step 1: Create `index.md`**

```markdown
---
layout: default
title: Braden Townsell
---

## Writing

- [Blog](/blog) — Technical posts, opinions, and lessons learned.
- [ADRs](/adrs) — Architecture Decision Records from real and studied decisions.
- [System Design](/system-design) — Structured write-ups of system design problems.
- [Notes](/notes) — Book notes and study summaries.

## More

- [Projects](/projects) — A portfolio of work with documented decisions.
- [Consulting](/consulting) — Advisory and architecture services via BTC.
- [About](/about) — Who I am and where I'm headed.
- [Now](/now) — What I'm currently reading, building, and learning.
```

- [ ] **Step 2: Create `about.md`**

```markdown
---
layout: page
title: About
---

I'm a software engineer on the individual contributor track, working toward Staff and Principal Engineer.

My primary stack is C#, SQL, and TypeScript, with deliberate expansion into distributed systems, cloud architecture, AI integration, and compliance domains.

I run [BTC](#consulting), a consulting practice focused on architecture advisory and technical due diligence for early-stage and growth-stage companies.

This site is where I document my thinking. The writing is the point — the professional brand is a side effect.
```

- [ ] **Step 3: Create `projects.md`**

```markdown
---
layout: page
title: Projects
---

Work with documented architectural decisions. Each project links to its associated ADRs where applicable.

---

*Projects will be listed here as they are completed and documented.*
```

- [ ] **Step 4: Create `consulting.md`**

```markdown
---
layout: page
title: Consulting
---

## BTC

Enterprise quality. Startup speed.

Advisory and architecture services for engineering teams that need a trusted technical partner without full-time overhead.

### Services

**Architecture Review** — Assessment of your current system: what's working, what's load-bearing technical debt, and a prioritized path forward with business impact framing.

**Fractional Architecture Leadership** — Ongoing advisory for engineering decisions. ADRs, design reviews, RFC facilitation, and executive-level communication translation.

**Technical Due Diligence** — Pre-acquisition or pre-investment assessment of an engineering org, codebase, and technical debt profile.

### Engagement

[bradenjparrish@gmail.com](mailto:bradenjparrish@gmail.com)
```

- [ ] **Step 5: Create `now.md`**

```markdown
---
layout: page
title: Now
---

*Last updated: June 2026*

## Reading

- *Designing Data-Intensive Applications* — Martin Kleppmann (primary text, reading with notes)

## Building

- This site — documenting the setup and first ADR in parallel.

## Learning

- Systems design: one problem per week with diagrammed write-up.
- AWS Solutions Architect Associate prep (target: end of 2026).

---

*A [now page](https://nownownow.com/about) is a snapshot of what someone is focused on at this point in their life.*
```

- [ ] **Step 6: Verify all pages exist in build output**

```powershell
bundle exec jekyll build
```

Then verify:

```powershell
Test-Path _site/index.html        # True
Test-Path _site/about/index.html  # True
Test-Path _site/projects/index.html  # True
Test-Path _site/consulting/index.html  # True
Test-Path _site/now/index.html    # True
```

- [ ] **Step 7: Commit**

```bash
git add index.md about.md projects.md consulting.md now.md
git commit -m "feat: add static pages (home, about, projects, consulting, now)"
```

---

## Task 7: Add initial sample content

**Files:**
- Create: `_posts/2026-06-06-hello-world.md`
- Create: `_adrs/0001-use-jekyll-for-site.md`
- Create: `_system-design/2026-06-06-url-shortener.md`
- Create: `_notes/2026-06-06-ddia-chapter-1.md`

These seed each section so index pages render with real items and the layouts can be verified end-to-end.

- [ ] **Step 1: Create `_posts/2026-06-06-hello-world.md`**

```markdown
---
title: "Starting a Technical Blog"
date: 2026-06-06
---

This site exists to document thinking. Every post here is a byproduct of working through a problem, studying a system, or making a decision — made public on the theory that externalizing understanding is more useful than keeping it private.

The plan: one ADR per month, one system design write-up per quarter, and blog posts when there's something worth saying that doesn't fit a structured format.

The audience is mixed — engineering peers, potential clients, and future me. Written for peers; clarity required for that audience satisfies the rest.
```

- [ ] **Step 2: Create `_adrs/0001-use-jekyll-for-site.md`**

```markdown
---
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
```

- [ ] **Step 3: Create `_system_design/2026-06-06-url-shortener.md`**

```markdown
---
title: "Design a URL Shortener"
date: 2026-06-06
---

## Requirements

### Functional
- Given a long URL, generate a short URL (e.g., `https://short.ly/abc123`)
- Given a short URL, redirect to the original long URL
- Optional: custom aliases, expiration dates, click analytics

### Non-Functional
- Reads vastly outnumber writes (100:1 ratio assumed)
- Redirect latency < 50ms at p99
- 100M URLs stored, ~1B redirects/day at scale

### Out of scope for this exercise
- User accounts, authentication
- Analytics beyond basic click counts

## High-Level Design

```
Client → Load Balancer → Shortening Service → [ID Generator, Cache, DB]
Client → Load Balancer → Redirect Service → [Cache, DB]
```

Separate the write path (shortening) from the read path (redirect). Reads are hot; writes are cold.

## Components

**ID Generator** — Produces short, unique IDs. Options:
- Base62 encoding of an auto-increment integer: simple, sequential (guessable), no coordination needed for single DB
- MD5/SHA hash truncated to 7 chars: collision risk at scale, requires collision detection
- Distributed ID (Snowflake): globally unique, not sequential, no single point of failure

*Choice: Base62 of auto-increment for simplicity. Add distributed ID generation (Snowflake pattern) if sharding is required.*

**Storage** — One row per URL: `id, short_code, long_url, created_at, expires_at`. PostgreSQL for the primary store. Reads are by primary key (`short_code`), which is fast.

**Cache** — Redis in front of the DB for the redirect path. Cache short_code → long_url. TTL matches URL expiration or 24h default. Cache hit rate expected to be high given power-law distribution of URL access patterns.

**Redirect Service** — Stateless. Looks up short_code in cache, falls back to DB, returns HTTP 301 (permanent) or 302 (temporary if analytics needed).

## Trade-offs

| Decision | Alternative | Why this |
|---|---|---|
| 301 redirect | 302 redirect | 301 is cached by browsers, reducing server load. Use 302 if click counting matters. |
| PostgreSQL | NoSQL (Cassandra) | Simpler operationally. Sharding is not needed until ~1B rows. |
| Redis cache | No cache | Redirect latency requirement (<50ms) is not achievable at scale without caching. |
| Base62 IDs | Random hash | Avoids collision detection complexity. Predictable ID length (7 chars = 62^7 ≈ 3.5T URLs). |

## Scaling Bottlenecks

1. **DB writes**: Not a bottleneck at this scale (few writes vs. many reads).
2. **DB reads**: Solved by Redis cache. Cache miss rate drives DB read load.
3. **ID generation**: Auto-increment becomes a bottleneck with DB sharding. Switch to Snowflake IDs at that point.
4. **Hot URLs**: Power-law distribution means a small number of URLs get most traffic. Redis handles this naturally — hot keys stay in memory.
```

- [ ] **Step 4: Create `_notes/2026-06-06-ddia-chapter-1.md`**

```markdown
---
title: "DDIA Chapter 1 — Reliable, Scalable, and Maintainable Applications"
date: 2026-06-06
source: "Designing Data-Intensive Applications — Kleppmann"
---

## Core Idea

Three properties most data systems try to achieve: **reliability** (correct behavior under faults), **scalability** (handling growth), and **maintainability** (operability over time for people who didn't write it).

## Reliability

A system is reliable if it works correctly even when things go wrong. Faults (a component deviating from spec) are not the same as failures (the system stops working). The goal is fault *tolerance*, not fault prevention.

Key insight: it's better to increase fault rates during testing (chaos engineering) than to assume faults won't happen.

## Scalability

Scalability is not a binary property — "can handle more load" isn't specific enough. Useful framing: *describe load* (load parameters), *describe performance* (throughput, latency percentiles), then ask "how does performance change when load increases?"

Percentiles matter more than averages. p99 latency is what your slowest users experience. Tail latency amplification: if a request requires multiple backend calls, the overall response time is the slowest of all of them.

## Maintainability

Three design principles:
- **Operability** — make it easy to run the system (observability, runbooks, deployment automation)
- **Simplicity** — remove accidental complexity (abstraction, not cleverness)
- **Evolvability** — make change easy (testability, loose coupling)

## What I want to follow up on

- How do you define "load parameters" concretely for a given system? What are the right ones for a write-heavy vs. read-heavy system?
- The distinction between latency and response time — Kleppmann is precise here and it's worth internalizing.
```

- [ ] **Step 5: Verify all content builds and section indexes show items**

```powershell
bundle exec jekyll serve
```

Open http://localhost:4000. Verify:
- `/blog` shows "Starting a Technical Blog"
- `/adrs` shows "ADR 0001: Use Jekyll for the Technical Blog"
- `/system-design` shows "Design a URL Shortener"
- `/notes` shows "DDIA Chapter 1 — ..."

Click each link and verify the item page renders with its layout (status badge on ADR, date on all). Stop server with `Ctrl+C`.

- [ ] **Step 6: Commit**

```bash
git add _posts/ _adrs/ _system_design/ _notes/
git commit -m "feat: add initial sample content for all four content sections"
```

---

## Task 8: Final verification and cleanup

- [ ] **Step 1: Full build with no warnings**

```powershell
bundle exec jekyll build --verbose 2>&1 | Select-String -Pattern "warn|error" -CaseSensitive:$false
```

Expected: No warnings about missing layouts, broken Liquid tags, or missing front matter.

- [ ] **Step 2: Spot-check all nav links**

```powershell
bundle exec jekyll serve
```

Open http://localhost:4000 and click every nav link. Verify each resolves to a page (no 404s):
- `/blog` ✓
- `/adrs` ✓
- `/system-design` ✓
- `/notes` ✓
- `/projects` ✓
- `/consulting` ✓
- `/about` ✓
- `/now` ✓

Stop server with `Ctrl+C`.

- [ ] **Step 3: Final commit**

```bash
git status
```

If any files are untracked or modified, stage and commit them. Then push to GitHub:

```bash
git push origin main
```

- [ ] **Step 4: Verify GitHub Pages deployment**

Go to `https://bradenjt.github.io` after a few minutes. Verify the site is live with navigation visible. GitHub Actions (or the legacy Pages build) will run automatically on push to `main`.

If the build fails on GitHub, check the Actions tab in the repo for error output. Common issue: a plugin in `_config.yml` that isn't on the GitHub Pages whitelist.
