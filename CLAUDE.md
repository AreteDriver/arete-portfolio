# CLAUDE.md — arete-portfolio

## Project Overview

Professional portfolio site for AI operations, enablement, workflow automation, and practical tooling. It is a single-page static site deployed through GitHub Pages.

## Current State

- **Language**: HTML, CSS, JavaScript (vanilla — no framework)
- **Files**: 10
- **Lines**: ~1,771 (index.html is the entire site)
- **Current URL**: https://aretedriver.github.io/arete-portfolio/
- **Planned custom domain**: aretedriver.dev (DNS migration pending)

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
- **Analytics**: none
- **Deployment**: GitHub Pages via `.github/workflows/deploy-pages.yml`
- **SEO**: JSON-LD structured data (WebSite, Person, SoftwareApplication), OG tags, canonical URL, sitemap

## Page Sections

1. **What to Review First** (`#current-focus`) — mcp-manager, Forgehand, arete-evals, arete-context-mcp, and The Human Stack
2. **AI Developer Tools** (`#tools`) — practical CLI and MCP tooling
3. **Additional Systems** (`#additional-systems`) — Animus and the standalone Loop Governor
4. **Playable Games** (`#games`) — current itch.io browser playtests
5. **Project Archive** (`#project-archive`) — earlier non-game builds with explicit hosting and archive status
6. **Stats Bar** — verified enterprise-operations outcomes and current review path

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

# Deployment runs automatically through GitHub Actions after a push to main.

# Check for broken internal references
grep -oP 'href="#\K[^"]+' index.html | sort -u
```

## Anti-Patterns (Do NOT Do)

- Do NOT commit secrets, API keys, or credentials
- Do NOT add a build step or framework — this is intentionally a zero-dependency static site
- Do NOT hardcode stats that differ from the stats bar section (keep meta descriptions, JSON-LD, and stats bar in sync)
- Do NOT add external CSS/JS dependencies — everything is inline for performance

## Legacy Vercel Configuration

`vercel.json` is retained as migration history. GitHub Pages does not apply those response-header rules. Do not claim those headers are active without checking the deployed response.

## Git Conventions

- Commit messages: Conventional commits (`feat:`, `fix:`, `docs:`, `style:`)
- Use a reviewable branch and pull request for publication changes; pushes to `main` trigger the Pages workflow.
