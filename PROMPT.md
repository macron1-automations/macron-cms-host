You are an expert Ruby on Rails 8 systems architect, front-end creative technologist, and DevOps engineer. Your goal is to build a production-grade host application for the **MacronCMS** engine (`https://github.com/macron1-automations/macron-cms`, v5.0+) tailored to my publication requirements.

Follow the phases below in strict order. Do **not** write application code until Phase 1 (Interview) and Phase 2 (Implementation Plan Approval) are complete.

---

## 1. Core Stack & Operating Rules

### Core Non-Negotiable Foundation
- **Framework**: Ruby on Rails `8.1+` (Ruby `3.4+`), PostgreSQL (`pg`), Puma, Basecamp `thruster`
- **Frontend & Assets**: `propshaft`, `importmap-rails`, `turbo-rails`, `stimulus-rails`, `tailwindcss-rails` (with `@plugin "@tailwindcss/typography";`), `pagy`
- **CMS & Auth**: `macron-cms` (`git: "https://github.com/macron1-automations/macron-cms", branch: "main"`, v5.0+), `devise` (`User` model with `admin:boolean`, default `false`, `null: false`)
- **Rails 8 Solid Trifecta**: `solid_cache`, `solid_queue`, `solid_cable` (zero Redis dependency)
- **Media & Compression**: ActiveStorage + `image_processing` (`ruby-vips` WebP variants), `aws-sdk-s3` (when S3-compatible cloud storage is selected), `terser` + `mini_racer` (Propshaft JS minification)

### Operating Rules
1. **Phase 0 — Inspect Before Asking**:
   - Before asking your first question, silently inspect the current workspace (`ls -la`, `Gemfile` if any, `ruby -v`, `postgres`/`psql --version`, `vips --version`, and any configured MCP servers).
   - Automatically adapt whether you need to run `rails new . --database=postgresql --css=tailwind` in an empty folder or integrate into an existing Rails 8 app. Never ask questions the workspace already answers.
2. **One-at-a-Time Interactive Interview**:
   - Use your native interactive question tool (`ask_question` in Antigravity) to ask **one question at a time**, resolving dependencies between choices dynamically and skipping irrelevant follow-ups.
   - Always put your recommended option first, prefixed with `(Recommended)`.
   - (Fallback: if running in a plain chat interface without `ask_question`, present one numbered multiple-choice question at a time and wait for my reply.)
3. **Strict Zero-Secret Policy**:
   - **Never** ask for or commit database passwords, `RAILS_MASTER_KEY`, S3 secret access keys, or API bearer tokens into git.
   - During the interview, collect only non-secret identifiers (publication name, domain, CDN hostname, S3 bucket/region, GA4 Measurement ID).
   - Emit `.env.example`, mark secret env vars as unsynced placeholders in deployment manifests, ensure `.env*` (except `.env.example`) is in `.gitignore`, and document exact secret setup steps in `docs/deployment.md`.
   - Only offer to wire up a local untracked `.env` or live `macron-cms-mcp` connection at the very end if I explicitly opt in.
4. **Self-Contained Portability**:
   - Rely **only** on `https://github.com/macron1-automations/macron-cms` (including its `dummy/` reference directory accessible via `bundle info macron-cms --path` after `bundle install`, or via GitHub) plus the reference blueprints embedded in Section 4 below.

---

## 2. Phase 1 — Upfront Branched Interview Protocol

Walk through these decision branches **one question at a time**, adapting follow-ups based on my answers:

### Branch 1: Publication Identity & Visual Tone
1. **Publication Identity**: Ask me (in plain text or via a custom response option) for:
   - **Publication Title** (e.g., `"Field Notes"`, `"Engineering Journal"`) and **Author / Organization Name**
   - **Subtitle / Meta Tagline** and **Default Admin Email** for `db/seeds.rb`
2. **Visual & Thumbnail Aesthetic**:
   - `(Recommended) Crisp Neutral Editorial + Monochrome Card Thumbnails`: Clean zinc/white typography (`Roboto Mono` / system sans + editorial prose), subtle neutral hover states (no blueish link color shifts), and a high-contrast black-and-white (`grayscale contrast-125`) filter on homepage/archive card thumbnails while keeping full color inside articles.
   - `Crisp Neutral Editorial + Full-Color Card Thumbnails`: Same clean neutral zinc typography and hover states, preserving full-color featured images on homepage and archive cards.
   - `Warm Literary Serif Aesthetic`: High-contrast serif display headings, warm paper-tinted background, and full-color imagery.

### Branch 2: Public Layout & Navigation Structure
3. **Homepage & Archive Architecture**:
   - `(Recommended) Magazine Multi-Band Homepage + Paginated Archive`: Multi-band editorial magazine layout on `/` (3-column 5-article hero band above the fold + 3 lazy-streamed Turbo Frame bands below the fold totaling up to 18 articles, left-aligned `"See all"` link to `/articles`), paginated `/articles` archive with centered `Home` and `Archive` top navigation + dark RSS icon/link, and `"Back to articles"` on article pages pointing back to `/`.
   - `Minimal Single-Column Editorial Stream`: Chronological essay feed directly on `/` with Pagy pagination and clean header navigation.
   - `Match a Reference Publication URL`: Replicate the layout hierarchy, spacing rhythm, and header style of an existing publication URL.
   - *Conditional Follow-up (only if Reference URL selected)*: Ask for the reference URL and inspect it before planning.

### Branch 3: Content Block & Interactive Widget Suite
4. **CMS Content Types & Front-End Widget Tier**:
   - `(Recommended) Full Interactive Suite (Editorial + Inline/Block KaTeX + Three.js WebGL)`:
     - **Editorial Core**: `heading`, `paragraph`, `markdown` (GFM), `blockquote`, `image` (responsive widths/alignments + captions), `embed` (sandboxed `srcdoc` iframe + fullscreen toggle).
     - **Math**: `latex` block + inline `$...$` (code-badge pill styling) and `$$...$$` display math inside `markdown` blocks.
     - **Interactive 3D / WebGL**: `three_js_image` (custom animated post-processing shaders including breathing pixelate), `three_js_quote` (3D rotating typography + CRT scanlines), `three_js_particles` (interactive particle field visualizer), and `three_js_card` (3D holographic tilt card with gyro/pointer sheen, ambient dust physics, and post-processing), with bidirectional in-place block conversions (`image` <-> `three_js_image` <-> `three_js_card`).
   - `Editorial + Mathematical Typesetting (No WebGL)`: Core editorial blocks + standalone `latex` block + inline/display KaTeX in `markdown` (skips Three.js/React bundles).
   - `Core Editorial Only`: `heading`, `paragraph`, `markdown`, `blockquote`, `image`, and `embed`.
5. **Custom Domain Widget** *(ask briefly)*:
   - `(Recommended) Start with the selected standard widget suite only`
   - `Add a custom domain-specific MacronCMS block widget` *(if chosen, ask what fields and visual behavior it should have)*.

### Branch 4: Hosting, Database, Storage & Edge CDN
6. **Production Infrastructure Stack**:
   - `(Recommended) Kamal 2 (Docker on any VPS / Cloud VM) + Managed PostgreSQL + S3-Compatible Object Storage + Edge CDN`
   - `Managed Container PaaS (Fly.io, Render, Railway, or Heroku) + Managed PostgreSQL + S3-Compatible Storage (AWS S3, Cloudflare R2, Tigris, DigitalOcean Spaces) + Edge CDN`
   - `Local Development Only for now (PostgreSQL + Local Disk ActiveStorage)`
   - *Conditional Follow-up (if Managed Container PaaS selected)*: Ask which PaaS (`Fly.io`, `Render`, `Railway`, or `Heroku`) and S3-compatible storage provider I plan to use so you generate the matching deployment manifest (`fly.toml`, `render.yaml`, `Procfile`, etc.).
   - *Conditional Follow-up (if Kamal or PaaS selected)*: Ask if I already have non-secret hostnames/identifiers to bake into configs—specifically **Public App Domain** (e.g., `blog.example.com`), **CDN Hostname** (e.g., `cdn.example.com`), and **S3 Bucket Name + Region + Endpoint URL**—with `(Recommended) Use clean example placeholders for now and document exact swap points in docs/deployment.md` if I haven't provisioned them yet.

### Branch 5: Syndication, Analytics, Content Migration & AI Publishing
7. **Optional Integrations & Workflows** *(multi-select)*:
   - `(Recommended) RSS 2.0 Feed (/articles.rss) + OpenGraph & X (summary_large_image) Social Metadata`
   - `Google Analytics 4 (Turbo Drive compatible)`
   - `Batch Content Migration Script / Rake Task (e.g., Markdown folder or Ghost / WordPress / Substack HTML+CSV export via MacronCMS REST API)`
   - `AI Publishing via MCP (macron-cms-mcp setup & documentation)`
   - *Conditional Follow-ups*:
     - If **GA4** selected: ask for the non-secret GA4 Measurement ID (`G-XXXXXXXXXX`) or whether to read `ENV["GA_MEASUREMENT_ID"]`.
     - If **Batch Content Migration** selected: ask what format/path the source export is in (e.g., Markdown folder, or HTML + `posts.csv` archive).

---

## 3. Phase 2 — Unified Implementation Plan Gate

Once all interview branches are resolved:
1. Present a comprehensive **Implementation Plan** (as a plan artifact with `RequestFeedback: true` if running in Antigravity `/plan` mode, or structured Markdown otherwise) detailing:
   - Directory & route architecture
   - Selected content type schemas (`config/content_types/*.yml`) and `ViewComponent` widgets (`app/components/widgets/*`)
   - Dynamic asset hydration (`widget_loader.js`), caching, variant warming, and minification pipeline
   - Deployment blueprint, storage, and CDN/CORS topology
   - Milestone verification commands
2. **Stop and wait for my explicit approval** before creating or modifying application files.

---

## 4. Phase 3 — Reference Blueprints & Production Guardrails ("No-Gotchas" Spec)

When executing the approved plan, bake in these exact architectural patterns so we avoid common Rails 8, Hotwire, ActiveStorage, WebGL CORS, and container deployment pitfalls on the first pass:

### 4.1 MacronCMS v5+ Engine Integration, Auth & Boot Sync
- **Authentication & Initializer (`config/initializers/macron_cms.rb`)**:
  - Set up Devise `User` with `admin:boolean` (`default: false, null: false`).
  - Configure `Macron::Cms.configure { |c| c.user_class = "User"; c.admin_flag = :admin }` and mount `Macron::Cms::Engine => "/cms"` in `config/routes.rb`.
  - Copy and run engine migrations: `bin/rails active_storage:install && bin/rails macron_cms:install:migrations`.
- **Native MacronCMS v5 Conventions**:
  - **Published Scope & Ordering**: MacronCMS v5 orders `Macron::Cms::Article` by `published_at: :desc, created_at: :desc` via its default scope. Filter public controllers with `Macron::Cms::Article.where("published_at <= ? OR published_at IS NULL", Time.current)` without overriding `.order`.
  - **Canonical 301 Slug Redirects**: Include `Macron::Cms::SlugRedirectable` in `ArticlesController` and guard `#show` with:
    ```ruby
    @article = Macron::Cms::Article.friendly.find(params[:id])
    return if redirect_to_canonical_slug_if_needed(@article)
    ```
  - **Featured Image Bubbling**: Mark every image-bearing content type (`image.yml`, `three_js_image.yml`, `three_js_card.yml`) with top-level `featured_image: true` so ActiveStorage attachments automatically bubble up to `article.featured_image_blob` on save without multi-table joins.
  - **Zero-N+1 Block Rendering**: Preload blocks via `article.article_contents.for_display.order(:sort_order, :id)` and support per-article custom backgrounds and text colors on `<body>` via `cms_article_background_style(@article)` (`background_color`, `text_color`, and `background_image` routed through `cdn_storage_url`).
- **Automatic Content-Type Sync & Thruster Port Mapping (`bin/docker-entrypoint`)**:
  - Container PaaS platforms often inject `PORT`, whereas Basecamp Thruster reads `HTTP_PORT`. Map it before boot, and automatically sync YAML content types into the database right after `db:prepare`:
    ```bash
    # Map PaaS PORT to HTTP_PORT for Thruster HTTP/2 proxy
    if [ -n "$PORT" ] && [ -z "$HTTP_PORT" ]; then
      export HTTP_PORT="$PORT"
    fi

    if [ "${@: -2:1}" == "./bin/rails" ] && [ "${@: -1:1}" == "server" ]; then
      ./bin/rails db:prepare
      ./bin/rails macron:cms:import_content_types
    fi
    ```

### 4.2 Bootstrapping Vendor Assets & Dynamic On-Demand Widget Hydration
- **Copying Base Assets from `macron-cms`**:
  - After `bundle install`, locate the gem checkout with `bundle info macron-cms --path`.
  - Inspect `dummy/` inside that path: copy needed prebuilt vendor bundles (`dummy/app/assets/javascripts/vendor/{react_bundle,three_text_bundle,three_fonts_bundle}.js`) into `app/assets/javascripts/vendor/`, and inspect/adapt starter widget JS (`dummy/app/assets/javascripts/widgets/{embed,latex,three_js_image,three_js_quote}.js`), `config/content_types/*.yml`, and `app/components/widgets/*`. (Note: KaTeX assets `macron/cms/katex.min.{js,css}` and fonts are served directly by the `macron-cms` gem engine.)
- **CDN-Aware Asset Manifest (`<meta name="cms-widget-assets">`)**:
  - Never eagerly load Three.js, React, or KaTeX on lightweight text pages. Render `<meta name="cms-widget-assets" content="<%= json_escape(cms_widget_assets_manifest.to_json) %>">` in `<head>`.
  - **Critical CDN Guardrail**: `asset_path(...)` can return root-relative paths unless prefixed with `asset_host`. In `cms_widget_assets_manifest`, explicitly join relative paths against `Rails.application.config.asset_host.presence || ENV["ASSET_HOST"].presence` (and cache in `Rails.cache` outside development) so dynamically injected `<script>` and `<link>` tags also load from the CDN rather than hitting the app origin.
- **Turbo-Resilient Dynamic Loader (`app/javascript/widget_loader.js`)**:
  - Maintain an in-memory `Map` of loaded script/style Promises so each bundle loads at most once per session.
  - Check `scope.querySelector(...)` for `[data-cms-latex]`, `[data-embed-card]`, `[data-three-js-image-mount]`, `[data-three-js-quote-mount]`, `[data-three-js-particles-mount]`, and `[data-three-js-card-mount]`, loading vendor prerequisites before widget scripts and invoking their mount functions (`window.renderMacronLatex(scope)`, `window.MacronEmbed.init(scope)`, `window.MacronThreeJs*.mountAll(scope)`).
  - Bind to `DOMContentLoaded`, `turbo:load`, `turbo:render`, and `turbo:frame-load` (passing `e.target` as `scope` on frame loads), and clean up WebGL animation frames / Three.js `WebGLRenderer` contexts on `turbo:before-cache` so memory never leaks across navigations.

### 4.3 Rich Content Blocks: Inline LaTeX & WebGL Extensions (When Selected)
- **Inline `$...$` & Display `$$...$$` LaTeX inside `MarkdownComponent`**:
  - Use `kramdown` + `kramdown-parser-gfm` with a custom `Kramdown::Converter.add_math_engine(:macron_latex, ...)` plus a tree pass over `:text` nodes matching single-dollar inline math `/(?<!\\|\$)\$(?!\s|\$)((?:\\.|[^\$\n])+?)(?<!\s|\$)\$(?!\d|\$)/` (so prices like `$100` are untouched).
  - Render display math as `<div class="cms-latex-widget my-6 overflow-x-auto py-2 text-center" data-cms-latex="..." data-cms-latex-display="true">` and inline math as clean code-badge pills: `<span class="cms-latex-inline text-sm font-mono bg-zinc-100 text-zinc-800 border border-zinc-200/60 rounded px-1.5 py-0.5 mx-0.5" data-cms-latex="..." data-cms-latex-display="false">`.
  - Whitelist `"data-cms-latex"` and `"data-cms-latex-display"` in `Rails::HTML::Sanitizer.safe_list_sanitizer` when sanitizing Markdown HTML, and ensure `widgets/latex.js` hydrates `[data-cms-latex]` elements using `data-cms-latex-display === "true"` for `displayMode`.
- **Enhanced Three.js Widgets & Declarative Conversions**:
  - **Bidirectional In-Place Block Conversions**: Configure `convertible_to` in YAML schemas so `image`, `three_js_image`, and `three_js_card` can all convert to one another in place without re-uploading attachments (`file` / `upload` ActiveStorage attachment compatibility).
  - **`three_js_image`**: Ensure time-uniform (`uTime`) animation loops run continuously for animated shader effects—including a smooth breathing resolution cycle on `pixelate`, alongside ripple/wave/glitch/RGB-shift/CRT effects.
  - **`three_js_particles`**: Interactive WebGL particle visualizer with `preset` (`wave`, `galaxy`, `morph`, `plexus`, `flow`), `title`, `subtitle`, `text_align`, `overlay_style` (**default `clean` floating text with no border/box**, plus optional `card` translucent glass), `color_scheme` (`cyberpunk`, `aurora`, `solar`, `deep_ocean`, `monochrome`, `custom`), `particle_count`, `particle_size`, `animation_speed`, `mouse_interaction` (`repel`, `attract`, `swirl`, `none`), `canvas_height`, and `post_processing` (`none`, `scanlines`, `film_grain`, `chromatic_aberration`).
  - **`three_js_card`**: 3D holographic gyro/pointer-tracking badge (`featured_image: true`) with `upload:file`, `card_shape` (`standard` 5:7, `square` 1:1, `wide` 16:9), `foil_style` (`rainbow`, `stardust`, `cyber`, `gold`, `silver`, `glass`), `foil_intensity`, `motion_mode` (`full` spring tilt + glare + idle bob, `tilt_only`, `idle_only`, `static`), distinct high-visibility `ambient_dust` physics (`off`, `subtle`, `magical`, `cosmic`, `embers`, `snow`, `vortex`, `ultra`), shader `post_processing` (`none`, `scanlines`, `film_grain`, `chromatic_aberration`), `background_style`, `canvas_height`, and `card_size`.

### 4.4 Editorial Frontend, Zero-CLS Lazy Turbo Frames & UX Guardrails
- **Zero-CLS Banded / Batch Streaming**:
  - On the homepage and `/articles` archive, select only lightweight card columns (`:id, :title, :slug, :meta_description, :featured_image_blob_id, :published_at, :created_at, :updated_at`), preload `{ featured_image_blob: :variant_records }` ONLY on eager above-the-fold cards (e.g. top 5 hero articles on `/`, top 4 rows on `/articles`), and stream below-the-fold bands/batches via `<turbo-frame loading="lazy">` backed by fixed-aspect-ratio skeleton loaders (`aspect-[3/2]` or `aspect-square`) so Cumulative Layout Shift (CLS) is `0`.
- **Critical Turbo Frame Navigation Guardrail**:
  - **Every link (`<a>` / `link_to`) inside or navigated from a lazy `<turbo-frame>` MUST set `data-turbo-frame="_top"`** (or `target="_top"` on the `<turbo-frame>`). Without this, clicking an article card inside a lazy band attempts to extract a matching `<turbo-frame>` from `ArticlesController#show` and fails with `"Content missing"`.
- **Editorial Typography & Polish**:
  - Keep card title, subtitle/description, and publication date stacked tightly underneath one another (`mt-1` / `leading-snug`, avoiding loose `gap-3` or `prose` paragraph margins inside cards).
  - Center pagination controls cleanly on mobile viewports (`flex justify-center`).
  - Include `<html lang="en">` on layouts and place a valid `/public/favicon.ico` alongside `/icon.svg` and `/icon.png` so root `/favicon.ico` requests never raise `ActionController::RoutingError` in production logs.

### 4.5 Image Variants, Russian-Doll Caching, Edge CDN Proxy & WebGL CORS
- **Responsive WebP Variants & Background Warming**:
  - Define standard `CARD_IMAGE_VARIANTS = { feature: [1200, 800], grid: [600, 400], compact: [400, 300], archive: [240, 240] }` encoded as `format: :webp, saver: { quality: 80, strip: true }`.
  - Add `WarmArticleVariantsJob` hooked via `after_commit` on `Macron::Cms::Article` (`if: :saved_change_to_featured_image_blob_id?`) inside `Rails.application.config.to_prepare` so featured image variants are pre-generated asynchronously via Solid Queue upon publishing/editing.
- **HTTP Conditional GETs & Russian-Doll Fragment Caching**:
  - Use `stale?(etag: ..., last_modified: ..., public: true)` on `PagesController#index` and `ArticlesController#show`.
  - Cache rendered block lists in a helper (`cached_cms_render_article_contents(article)`) keyed on `[article, "cms_rendered_contents/v1"]` wrapping per-block `[ac, "widget_html/v1"]` caches, and include `article.featured_image_blob_id` in article card cache keys so changing an image immediately busts card fragments.
- **Edge CDN Routing Without S3 Redirect Leaks**:
  - Set `config.asset_host = ENV["ASSET_HOST"].presence` in `config/environments/production.rb`.
  - Set `config.active_storage.resolve_model_to_route = :rails_storage_proxy` AND build a `cdn_storage_url(target)` helper using `rails_storage_proxy_url(resolvable, host: uri.host, protocol: uri.scheme)` whenever `asset_host` is present. Use `cdn_storage_url` across **all** public image surfaces: homepage/archive cards, `ImageComponent`, `ThreeJsImageComponent`, `ThreeJsCardComponent`, and `cms_article_background_style`. (Never let ActiveStorage 302-redirect browsers directly to raw S3 bucket URLs when a CDN is configured, or media will bypass edge caching.)
- **CORS Headers for CDN Web Fonts & WebGL Textures**:
  - In `config/environments/production.rb`, set:
    ```ruby
    config.public_file_server.headers = {
      "cache-control" => "public, max-age=#{1.year.to_i}",
      "access-control-allow-origin" => "*"
    }
    ```
    so KaTeX and Three.js font files loaded via `ASSET_HOST` never fail browser CORS checks.
  - Create `config/initializers/active_storage_cors.rb` inserting a lightweight Rack middleware at index `0` (`Rails.application.config.middleware.insert_before 0, ...`) for `/rails/active_storage/` paths that handles `OPTIONS` (`204`) and appends `Access-Control-Allow-Origin: *`, `Access-Control-Allow-Methods: GET, HEAD, OPTIONS`, and `Access-Control-Allow-Headers: *` to responses—preventing Three.js `TextureLoader` (`crossOrigin = "anonymous"`) CORS failures when hydrating 3D images/cards through an edge CDN.

### 4.6 Propshaft JavaScript Minification
- Propshaft digests JS files out of the box but does **not** minify them, leaving custom widget scripts (`widgets/three_js_*.js`, `widgets/embed.js`, `widgets/latex.js`) uncompressed on the CDN.
- Create `lib/propshaft/compiler/js_minifier.rb` subclassing `Propshaft::Compiler` using `terser` (`Terser.new(module: true, compress: { drop_console: false })`) and `mini_racer`:
  - Run when `Rails.env.production? || ENV["MINIFY_ASSETS"] == "true"`.
  - Skip already-minified files matching `/\.min\.js\z/i` or `%r{\Avendor/}i`.
  - Normalize input encoding to valid UTF-8 before calling `terser.compile`, rescuing errors gracefully with `Propshaft.logger.error` fallback.
  - Register in `config/initializers/assets.rb`:
    ```ruby
    require "propshaft/compiler/js_minifier"
    Rails.application.config.assets.compilers << [ "text/javascript", Propshaft::Compiler::JsMinifier ]
    ```

### 4.7 SEO, RSS 2.0, Analytics, Content Migration & MCP (As Selected)
- **SEO & Social Cards (`cms_seo_meta`)**: Emit `<title>`, `<meta name="description">`, `<link rel="canonical">`, `og:type` (`article`), `og:title`, `og:description`, `og:url`, plus `og:image`, `twitter:card` (`summary_large_image`), and `twitter:image` pointing to an **absolute** `http(s)://` URL of the `feature` (`[1200, 800]`) WebP featured image variant (joining `request.base_url` if `ASSET_HOST` is unset).
- **RSS 2.0 Feed**: `GET /articles.rss` (`app/views/articles/index.rss.builder` using `jbuilder`/`builder`) returning the 5 most recent published articles + `auto_discovery_link_tag(:rss, articles_url(format: :rss))` in `<head>`.
- **Turbo-Safe GA4**: Render only in `Rails.env.production?` when Measurement ID is present; put `data-turbo-eval="false"` on the inline config `<script>`, initialize with `{ send_page_view: false }`, and dispatch `gtag('event', 'page_view', ...)` inside a `turbo:load` listener so SPA navigations track accurately without double-counting.
- **Batch Content Migration (if requested)**: Build an idempotent importer posting to `POST /cms/api/v1/articles` (`Authorization: Bearer <MACRON_CMS_API_KEY>`) with `"upsert": true`, supporting `SKIP_EXISTING=true` for interrupt recovery and configurable rate-limiting (`SLEEP_SECONDS`) when targeting live servers so bulk image processing never saturates background workers.
- **MCP Integration (if requested)**: Document connecting [`macron-cms-mcp`](https://github.com/macron1-automations/macron-cms-mcp) (`MACRON_CMS_URL` + `MACRON_CMS_API_KEY`) so articles can be drafted, edited, and published directly from Antigravity, Claude Code, or Cursor prompts.

### 4.8 Container Hygiene, Connection Poolers & Origin-Pull CDN Rules
When generating Docker / Kamal / PaaS deployment manifests and `docs/deployment.md`, include these vendor-neutral production practices:
1. **Database & Connection Pooler Compatibility**:
   - Wire `config/database.yml` production primary, cache, queue, and cable connections cleanly to `ENV["DATABASE_URL"]` with configurable `prepared_statements: <%= ENV.fetch("DATABASE_PREPARED_STATEMENTS", "true") == "true" %>` (allowing `DATABASE_PREPARED_STATEMENTS=false` when running behind transaction-mode PgBouncer or managed connection poolers).
   - Note in `docs/deployment.md` to verify IPv4 vs. IPv6 endpoint support between the chosen container host and managed PostgreSQL provider (using an IPv4 pooler hostname if the container network lacks outbound IPv6).
2. **Container & Image Processing Hygiene**:
   - Support running Solid Queue in-process (`SOLID_QUEUE_IN_PUMA=true`) for single-container deployments alongside optional dedicated worker processes.
   - Install `libjemalloc2` (`LD_PRELOAD`) and `libvips` in `Dockerfile`, bound `Vips.concurrency_set` and disable unneeded vips operation cache in `config/initializers/vips.rb`, and call `GC.compact if GC.respond_to?(:compact)` inside `config.after_initialize` in `config/environments/production.rb`.
   - Exclude `/up` healthcheck from SSL redirect (`config.ssl_options = { redirect: { exclude: ->(req) { req.path == "/up" } } }`).
3. **Origin-Pull Edge CDN Topology**:
   - When configuring an origin-pull CDN (`ASSET_HOST`), document setting the CDN **Origin** and **Custom Host Header** to the upstream application hostname rather than the public vanity domain (`blog.example.com`) to prevent proxy/DNS loops, requiring only a `CNAME` record for `cdn.example.com`.

---

## 5. Phase 4 — Milestone Execution Sequence & Final Verification

After I approve your Phase 2 Implementation Plan, execute the build autonomously across these verified milestones:

1. **Milestone 1 — Foundation, Auth & MacronCMS Engine**:
   - Scaffold/configure Rails 8.1+, gems, Tailwind typography, Devise `User` (`admin:boolean`), `Macron::Cms` mount & initializer, `db:prepare`, and `db/seeds.rb` (idempotently creating an initial admin user with a development password printed to console + a rich sample multi-block showcase article).
2. **Milestone 2 — Content Types, ViewComponents & Dynamic Hydration**:
   - Create `config/content_types/*.yml`, import via `bin/rails macron:cms:import_content_types`, build `app/components/widgets/*`, vendor/widget JS assets, `cms_widget_assets_manifest`, and `app/javascript/widget_loader.js`.
3. **Milestone 3 — Public Frontend, Zero-CLS Streaming, SEO & Feeds**:
   - Build layouts, header/footer, `PagesController#index`, `ArticlesController#{index,show}`, lazy Turbo Frame bands/batches (verifying `data-turbo-frame="_top"` on all card links), `cms_seo_meta`, RSS 2.0 feed, `/public/favicon.ico`, and optional GA4 partial.
4. **Milestone 4 — Performance, Caching, CDN/CORS & Minification**:
   - Add `CARD_IMAGE_VARIANTS`, `WarmArticleVariantsJob`, Russian-doll caching + `stale?` ETags, `:rails_storage_proxy` + `cdn_storage_url`, `active_storage_cors.rb`, `vips.rb`, and `Propshaft::Compiler::JsMinifier`.
5. **Milestone 5 — Deployment Blueprints, Documentation & Quality Gate**:
   - Generate `Dockerfile` (with `jemalloc` & `libvips`), `bin/docker-entrypoint`, selected deployment manifest (`config/deploy.yml`, `fly.toml`, `render.yaml`, etc.), `.env.example`, tailored `README.md`, and `docs/deployment.md` (plus optional migration task & MCP instructions).
   - Run and pass **all** verification gates before reporting completion:
     ```bash
     bin/rails db:prepare
     bin/rails macron:cms:import_content_types
     MINIFY_ASSETS=true SECRET_KEY_BASE_DUMMY=1 bin/rails assets:precompile && bin/rails assets:clobber
     bin/rails test
     bundle exec rubocop
     bundle exec brakeman --no-pager
     ```
   - Write controller/integration tests covering homepage rendering, lazy band frame streaming, archive pagination, RSS XML output, canonical 301 slug redirects, OpenGraph image tags, CORS headers on `/rails/active_storage/*`, and `/cms` authentication gating.

Begin **Phase 0** (workspace inspection) now, then ask me **Question 1** of the interview.
