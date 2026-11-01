# MacronCMS Host — AI Master Prompt

[![Rails 8.1+](https://img.shields.io/badge/rails-8.1%2B-red.svg)](https://rubyonrails.org)
[![MacronCMS v5+](https://img.shields.io/badge/macron--cms-5.0%2B-emerald.svg)](https://github.com/macron1-automations/macron-cms)
[![MCP Compatible](https://img.shields.io/badge/MCP-macron--cms--mcp-purple.svg)](https://github.com/macron1-automations/macron-cms-mcp)
[![Live Demo](https://img.shields.io/badge/live_demo-blog.macron1.io-4F46E5.svg)](https://blog.macron1.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

A self-contained **Master Prompt** ([`PROMPT.md`](PROMPT.md)) for agentic AI coding harnesses—such as **Google Antigravity**, **Claude Code**, and **Cursor**—that interviews you and builds a tailored, production-ready **Ruby on Rails 8.1+** host application for the [**MacronCMS**](https://github.com/macron1-automations/macron-cms) block engine.

Instead of spending dozens of iterations wiring up content schemas, ViewComponent widgets, on-demand WebGL/KaTeX hydration, zero-CLS Hotwire streaming, Russian-doll caching, Propshaft minification, WebGL CDN CORS headers, and deployment configs by hand, paste [`PROMPT.md`](PROMPT.md) into a fresh workspace and let your coding agent interview, plan, build, and verify the entire platform.

> [!TIP]
> **See it in action:** Check out a live production implementation running at **[blog.macron1.io](https://blog.macron1.io/)** to experience the zero-CLS magazine layout, interactive 3D Three.js widgets, on-demand KaTeX mathematical typesetting, and Turbo-driven hydration firsthand.

---

## What It Scaffolds

Depending on the choices you select during the interactive setup interview, the agent builds:

- **Rails 8.1+ & MacronCMS v5+ Core**: PostgreSQL, Propshaft, Importmap, Hotwire (Turbo + Stimulus), Tailwind CSS (`@tailwindcss/typography`), Solid Cache / Solid Queue / Solid Cable, Basecamp Thruster, Pagy, and Devise admin authentication protecting `/cms`.
- **Modular Content Types & `ViewComponent` Widgets**:
  - **Editorial Core**: Headings, paragraphs, GitHub-Flavored Markdown, blockquotes, responsive images, and sandboxed fullscreen-capable HTML/media embeds.
  - **Mathematical Typesetting**: Standalone KaTeX equation blocks plus inline `$...$` code-badge pills and `$$...$$` display math directly inside Markdown blocks.
  - **Interactive 3D & WebGL (Three.js + React)**: Post-processed 3D shader images, 3D rotating typography quotes with CRT scanlines, interactive particle field visualizers, and 3D holographic tilt cards with gyroscope/pointer glare and ambient dust physics—including bidirectional in-place block conversions (`image` <-> `three_js_image` <-> `three_js_card`).
- **On-Demand Dynamic Client Hydration**: A Turbo-aware `widget_loader.js` and `<meta name="cms-widget-assets">` manifest so heavy bundles (Three.js, React, KaTeX) are only fetched when matching widgets appear on the page.
- **Zero-CLS Editorial Frontend**: Multi-band magazine homepage + paginated `/articles` archive (or minimal single-column stream / custom reference layout) using lazy `<turbo-frame>` bands, fixed-aspect skeleton loaders, responsive WebP variants, and background variant warming via Solid Queue.
- **Production Performance, Edge CDN & Security**: HTTP conditional GETs (`stale?`), Russian-doll fragment caching, Propshaft `terser` JS minification, ActiveStorage `:rails_storage_proxy` CDN routing, cross-origin font/WebGL texture CORS middleware, RSS 2.0 syndication, OpenGraph / X large-image cards, optional Turbo-safe GA4, and optional [`macron-cms-mcp`](https://github.com/macron1-automations/macron-cms-mcp) AI publishing integration.

---

## Prerequisites

Ensure your local development machine has:

- **Ruby `3.4+`** (managed via `rbenv`, `mise`, `asdf`, or `chruby`) and **Bundler**
- **PostgreSQL `14+`** running locally for development and test databases
- **`libvips`** for ActiveStorage WebP image processing:
  - macOS: `brew install vips`
  - Ubuntu / Debian: `sudo apt-get install -y libvips-dev`

---

## Quickstart

### 1. Create or open your project directory

```bash
mkdir my-publication && cd my-publication
```

*(You can run the prompt in a completely empty folder or inside an existing fresh Rails 8 application—the agent inspects your workspace automatically before starting.)*

### 2. Copy [`PROMPT.md`](PROMPT.md)

Copy the raw contents of [`PROMPT.md`](PROMPT.md) on GitHub, or copy it straight from your terminal:

```bash
# macOS
curl -fsSL https://raw.githubusercontent.com/macron1-automations/macron-cms-host/main/PROMPT.md | pbcopy

# Linux (xclip)
curl -fsSL https://raw.githubusercontent.com/macron1-automations/macron-cms-host/main/PROMPT.md | xclip -selection clipboard
```

### 3. Run in your AI coding harness

- **Google Antigravity**: Paste the prompt into chat (optionally appending `/grill-me /plan` to activate native interview and plan-review modes explicitly).
- **Claude Code / Cursor / Other Harnesses**: Paste the prompt into your agent session (or reference `@PROMPT.md` if cloned locally).

### 4. Answer, Approve & Build

1. **Interactive Interview**: Answer 5–7 one-at-a-time multiple-choice questions covering branding, layout style, widget tiers, hosting/storage stack, and optional integrations (every question includes a `(Recommended)` default).
2. **Plan Approval Gate**: Review and approve the unified implementation plan before any application code is written.
3. **Verified Multi-Milestone Build**: The agent scaffolds the app, syncs MacronCMS schemas, builds widgets and caching layers, generates deployment docs, and runs `rails test`, `rubocop`, `brakeman`, and an `assets:precompile` minification check.

> **Zero-Secret Policy**: `PROMPT.md` strictly instructs the agent never to ask for or commit passwords, `RAILS_MASTER_KEY`, S3 secret keys, or API bearer tokens into version control. All secrets are documented cleanly via `.env.example` and generated deployment guides.

---

## Related Projects

- [**macron-cms**](https://github.com/macron1-automations/macron-cms) — Modular Rails 8 CMS engine with declarative YAML schemas, ViewComponent widgets, and headless REST API.
- [**macron-cms-mcp**](https://github.com/macron1-automations/macron-cms-mcp) — Official Model Context Protocol (MCP) server for drafting, editing, and publishing MacronCMS articles from AI agents.

---

## License

Released under the [MIT License](LICENSE).
