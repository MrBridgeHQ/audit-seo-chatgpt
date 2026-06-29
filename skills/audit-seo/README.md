# audit-seo — a ChatGPT / Codex skill

A skill that performs a comprehensive **SEO audit of a web project** from its source code and configuration — and, optionally, from a live URL you control. It covers technical, on-page, structural, content, performance, AI-search (AEO/GEO), and framework-specific SEO, then produces a prioritized, severity-classified report.

This is the **ChatGPT / Codex** packaging of the skill (Codex skill format: `SKILL.md` + `agents/openai.yaml` + `references/` + `checklists/` + `templates/`).

## What it does

Given a web project, the skill:

1. **Detects the stack** (Next.js, Nuxt, Astro, SvelteKit, Remix, Laravel, Symfony, WordPress, static sites, React/Vue SPA) from `package.json`, `composer.json`, config files, and project layout.
2. **Inventories the SEO-critical files** (configs, `robots.txt`, `sitemap.xml`, routes, layouts/head components, assets, i18n).
3. **Enumerates routes** and flags likely-indexable vs orphaned pages.
4. **Audits the source** against eight checklist categories (see below).
5. **Optionally crawls a URL you provide** (read-only `GET`, `localhost` or a domain you own — it asks for confirmation otherwise).
6. **Prioritizes findings** by SEO impact × effort (Critical / High / Medium / Low).
7. **Generates a Markdown report** (or a JSON block on request) following the shipped template.
8. **Proposes fixes as suggestions** — it never edits files without explicit confirmation.

## Categories audited

- **Technical SEO** — `robots.txt`, `sitemap.xml`, canonicals, redirects, status codes, `noindex`/`nofollow`, URL structure, pagination, hreflang, 404s, framework routing.
- **Metadata** — `<title>`, meta description, Open Graph, Twitter Cards, canonical, viewport, charset, robots meta, cross-template consistency.
- **HTML structure** — single `<h1>`, heading hierarchy, internal links, `alt` attributes, HTML semantics, SEO-impacting accessibility.
- **Structured data** — JSON-LD, Schema.org (`Organization`, `WebSite`, `BreadcrumbList`, `Article`, `Product`, `FAQPage`), E-E-A-T author markup, validity and consistency.
- **Performance & Core Web Vitals** — image optimization, lazy-loading, fonts, blocking JS, critical CSS, bundle size, CSR vs SSR/SSG.
- **Content & internal linking** — duplication, cannibalization, search intent, internal linking, click depth, slugs, language/region consistency.
- **Framework specifics** — per-stack pitfalls (App Router vs Pages Router, Nuxt `useHead`, Astro content collections, SvelteKit `prerender`, Remix `meta`, WordPress SEO-plugin conflicts, SPA SSR gaps, etc.).
- **AI search & AEO/GEO (2026)** — AI-crawler directives in `robots.txt` (GPTBot, ClaudeBot, Google-Extended, PerplexityBot, Applebot-Extended, CCBot), the emerging `llms.txt` / `llms-full.txt` standard, content optimization for AI Overviews / SGE / Perplexity citations, and E-E-A-T for AI search.

## What's inside

```
audit-seo/
├── SKILL.md                          # Orchestration: when to activate, workflow, categories, output format, safety
├── README.md                         # This file
├── INSTALL.md                        # Installation instructions
├── agents/
│   └── openai.yaml                   # Codex interface metadata (display name, short description, implicit invocation)
├── checklists/
│   ├── technical-seo.md              # robots, sitemap, canonical, redirects, status codes, hreflang, pagination
│   ├── metadata-seo.md               # title, description, OG, Twitter Cards, viewport, charset, robots meta
│   ├── structured-data.md            # JSON-LD / Schema.org types + E-E-A-T author markup
│   ├── performance-seo.md            # Core Web Vitals (LCP / INP / CLS), images, fonts, JS, CSS, SSR/SSG
│   ├── content-seo.md                # HTML structure, internal linking, slugs, duplication, thin content
│   ├── framework-specific.md         # per-stack pitfalls
│   └── ai-search-seo.md              # AI crawlers, llms.txt, AI Overview / SGE, E-E-A-T for AI search (2026)
├── references/
│   ├── framework-map.md              # file ↔ framework detection map + read-only inspection commands
│   └── seo-thresholds.md             # numeric thresholds (title length, OG image size, CWV targets, score scale)
└── templates/
    └── report.md                     # exact skeleton of the final audit report
```

## Note on language

The skill's instruction prose is written in French (it activates on both French and English requests, as stated in its `description`). The audit it produces follows the user's language and the report template. The technique — checklists, thresholds, framework map — is language-neutral.

## How to invoke

Once installed, the skill auto-activates on SEO-audit requests. Example prompts:

- "Audit the SEO of this project"
- "Technical SEO review of this repo, focus on metadata"
- "Check my robots.txt and sitemap.xml"
- "Audit the structured data on this Next.js app"
- "/audit-seo http://localhost:3000 focus=structured-data depth=deep"

Arguments after the invocation are parsed: the first `http(s)://` URL becomes the (optional) crawl target, `focus=technical|metadata|content|structured-data|performance|all`, `output=markdown|json`, `depth=shallow|standard|deep`.

## Safety

The skill is **read-only by default**. It never runs destructive commands, never deploys/publishes, never edits files without explicit in-chat confirmation, and never crawls a third-party domain without confirmation. It clearly documents what it could not verify (no network, no build, no real metrics) in a dedicated "Limites de l'audit" section of the report.

## What's NOT inside

- **Off-page SEO / link building** — this audit is on-page and technical only.
- **Pricing / monetization** of a product.
- **Scraping / anti-bot engineering.**
- **Auto-rendered marketplace / store listing pages** imposed by a third-party platform.

## Part of the mr-bridge.com toolkit

This skill is part of the [mr-bridge.com](https://mr-bridge.com) toolkit for scraping, data, and content automation. Related resources:

- [mr-bridge.com](https://mr-bridge.com) — home
- [Scrapers](https://mr-bridge.com/scrapers) — the Apify Actor portfolio
- [MCP servers](https://mr-bridge.com/mcp-servers) — Model Context Protocol servers
- [AI workflows](https://mr-bridge.com/ai-workflows) — agents and automation
- [Studies](https://mr-bridge.com/studies) — data studies and one-pagers
- [Articles](https://mr-bridge.com/articles) — write-ups and guides
- [Solutions](https://mr-bridge.com/solutions) — end-to-end solutions

## License

Personal use. Customize freely. No warranty.
