# CLAUDE.md — arete-portfolio

## Project Overview

Professional portfolio site for aretedriver.dev — single-page static HTML showcasing AI developer tools, flagship projects, hackathon builds, case studies, and game projects. Deployed on Vercel.

## Current State

- **Language**: HTML, CSS, JavaScript (vanilla — no framework)
- **Files**: 10
- **Lines**: ~1,771 (index.html is the entire site)
- **Domain**: aretedriver.dev (Vercel)

## Architecture

```
arete-portfolio/
├── index.html       # Single-page site (all HTML, CSS, JS inline)
├── logo.jpg         # Logo asset
├── logo.webp        # WebP logo variant
├── og-image.jpg     # Open Graph social preview image
├── robots.txt       # SEO crawl directives
├── sitemap.xml      # XML sitemap
├── vercel.json      # Vercel config (headers, caching, CSP)
└── .vercel/         # Vercel project metadata
```

## Tech Stack

- **Frontend**: Static HTML + inline CSS + vanilla JavaScript
- **Styling**: CSS custom properties (dark theme, accent `#00ff88`)
- **Icons**: Inline SVG symbol sprites
- **Analytics**: Vercel Insights + Speed Insights
- **Deployment**: Vercel (static, no build step)
- **SEO**: JSON-LD structured data (WebSite, Person, SoftwareApplication), OG tags, canonical URL, sitemap

## Page Sections

1. **AI Developer Tools** (`#tools`) — 7 published CLI tools with pricing tiers (Free/Pro), Stripe payment links, bundle pricing ($29/mo or $199/yr)
2. **Flagship Projects** (`#flagships`) — Animus, BenchGoblins, Quorum
3. **Hackathon Projects** (`#hackathons`) — WatchTower, Frontier Tribe OS, Monolith (EVE Frontier / Sui blockchain)
4. **Case Studies** (`#case-studies`) — anchormd deep-dive + 9 project cards (BenchGoblins, Gatekeeper, Animus, Convergent, Dossier, LikX, Argus, G13, Overwatch, Fleet Monitor)
5. **Arcade** (`#arcade`) — Rebellion (Rust/Bevy WASM)
6. **Stats Bar** — 37,000+ tests, 7 CLI tools, 8 live systems, 3 hackathon builds

## Coding Standards

- **Naming**: camelCase for JS functions, kebab-case for CSS classes
- **Line Length**: soft wrap (no hard limit)
- **Pattern**: All content in a single `index.html` — no build toolchain, no bundler, no components
- **CSS**: Custom properties defined at `:root`, media queries for responsive grids
- **Links**: All external links use `target="_blank" rel="noopener"`

## Common Commands

```bash
# Local preview (any static server)
python -m http.server 8000

# Deploy (automatic via Vercel git integration, or manual)
vercel --prod

# Check for broken internal references
grep -oP 'href="#\K[^"]+' index.html | sort -u
```

## Anti-Patterns (Do NOT Do)

- Do NOT commit secrets, API keys, or credentials
- Do NOT add a build step or framework — this is intentionally a zero-dependency static site
- Do NOT hardcode stats that differ from the stats bar section (keep meta descriptions, JSON-LD, and stats bar in sync)
- Do NOT add external CSS/JS dependencies — everything is inline for performance

## Security Headers (vercel.json)

- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: DENY`
- `Referrer-Policy: strict-origin-when-cross-origin`
- `Content-Security-Policy`: restrictive (`default-src 'none'`, inline scripts/styles only)
- `Permissions-Policy`: camera, mic, geo all denied
- Image assets cached immutably (`max-age=31536000`)

## Git Conventions

- Commit messages: Conventional commits (`feat:`, `fix:`, `docs:`, `style:`)
- Branch: Work directly on `main` (static site, no deploy pipeline beyond Vercel auto-deploy)
