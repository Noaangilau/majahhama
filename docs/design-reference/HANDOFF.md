# Handoff: DA SHOP — Shopify Theme

## Overview
DA SHOP is a made-to-order clothing store. This package contains a high-fidelity interactive HTML mockup of the full storefront (homepage, catalog, product detail, cart drawer) in a "utilitarian/industrial editorial" aesthetic — spec-sheet meets zine. The goal is to **replace the store's current live Shopify theme** with this design.

## About the Design Files
The files in this bundle are **design references created in HTML** — an interactive prototype showing intended look and behavior, NOT production code to ship. Your task is to **build a real Shopify Online Store 2.0 theme** that recreates this design.

**Recommended approach:** fork Shopify's Dawn theme and restyle it to match this design (keeps cart, checkout, product, search and accessibility logic working), rather than building a theme from scratch. Structure as standard theme dirs: `layout/`, `sections/`, `snippets/`, `templates/`, `assets/`, `config/` with a `settings_schema.json` exposing at least: accent color, ticker text, ticker on/off.

**Workflow:** build locally with Shopify CLI (`shopify theme dev` against the user's store), push as an **unpublished** theme for preview, and let the user publish from admin (their current theme remains as rollback). Recommend connecting the repo to GitHub + Shopify's GitHub integration for deploys.

## Fidelity
**High-fidelity.** Recreate pixel-perfectly: exact colors, typography, spacing, and interaction states listed below. The only intentional placeholders are the garment images — the mockup uses flat "placeholder garment art" (diagonal stripes + tee glyph); in the real theme these become Shopify product images, with the placeholder art as the no-image fallback.

## Design Tokens
Colors:
- `--ink: #0a0a0a` (primary text, borders, dark fills)
- `--ink-2: #1a1a1a`, `--ink-3: #2a2a2a` (secondary dark / footer rule)
- `--mute: #6b6b6b`, `--mute-2: #9a9a9a` (secondary/tertiary text)
- `--rule: #d9d4ca`, `--rule-2: #ebe6dc` (hairline borders)
- `--paper: #f5f1ea` (page background), `--paper-2: #eee8dc` (panel/stripe alt)
- `--white: #ffffff` (cards, drawer, forms)
- `--accent: oklch(0.55 0.15 30)` (brick — badges, active size, hover inversions, warning banner). Must be a theme setting.

Typography (Google Fonts):
- Display: **Archivo** 900, uppercase, `letter-spacing: -0.02em`, `line-height: 0.95`
- Body: **Inter** 400/500/600, 14px base, `line-height: 1.5`
- Mono ("data voice"): **JetBrains Mono** 400/500/700, 9–13px, uppercase, `letter-spacing: 0.10–0.18em` — used for ALL nav links, labels, prices, badges, captions, specs, buttons, footer meta

Geometry & rules:
- **Zero border-radius anywhere** (color swatches excepted if added later)
- 1px solid `#0a0a0a` rules are the primary structural device; `#d9d4ca` for softer hairlines (spec rows, card art divider, cart item rows)
- No box-shadows. Color inversion (dark↔light) instead of decoration.
- Product grids: cells share collapsed 1px ink borders (container has `border-left` + `border-top` via header rule; each cell `border-right` + `border-bottom`)

## Screens / Views

### 1. Global chrome (all pages)
- **Ticker bar**: full-width `#0a0a0a` bg, 7px vertical padding; mono 10px, `0.16em` tracking, uppercase, `#f5f1ea` text; content scrolls left continuously (CSS keyframes, translateX 0 → −50%, 28s linear infinite, text duplicated 2×). Copy: `/// MADE TO ORDER — NO STOCK, NO WASTE /// WORLDWIDE SHIPPING /// DROP 04 LIVE NOW /// 240GSM ORGANIC COTTON`. Section setting to toggle/edit.
- **Sticky header**: `position: sticky; top: 0`, paper bg, 1px ink bottom border, 52px tall, 24px side padding, 3-col grid (logo / nav / cart). Logo "DA SHOP": Archivo 900, 17px, uppercase. Center links SHOP · INFO · ARCHIVE: mono 10px, 0.14em tracking, 28px gap; hover = 1px ink underline (border-bottom). Right: "CART" mono + count chip (ink bg, paper text, `padding: 2px 6px`, zero-padded e.g. `02`). Cart link opens the drawer.
- **Footer**: ink bg, paper text, `padding: 56px 24px 32px`. Grid `2fr 1fr 1fr`, 32px gap: giant "DA SHOP" (Archivo 900, 64px) + INDEX and LEGAL link columns (mono 10px, 0.14em, muted `#6b6b6b` column labels). Bottom meta row above/below a 1px `#2a2a2a` rule: `© 2026 DA SHOP — ALL GOODS MADE TO ORDER` / `REV 04.1 / PRINTED ON DEMAND`, mono 9px `#6b6b6b`.

### 2. Homepage (template: index)
- **Hero (split-screen)**, 1px ink bottom border, grid `1.1fr 1fr`:
  - Left: `padding: 56px 48px`, vertically centered column, 24px gap. Mono label `RELEASE LOG — ENTRY 04 / SS26` (11px, 0.14em, `#6b6b6b`). Headline `DROP 04: HEAVY GOODS.` — Archivo 900, `clamp(56px, 7.5vw, 104px)`. One short body paragraph (max-width 420px, `#2a2a2a`). Two buttons (see Buttons).
  - Right: full-bleed garment art panel, 1px ink left border, min-height 520px. Diagonal stripes `repeating-linear-gradient(45deg, #eee8dc 0 10px, #f5f1ea 10px 20px)`, centered tee glyph SVG at 16% opacity (46% width), corner code `DS-001/HERO` top-right (mono 10px), caption bottom-left (mono, `#6b6b6b`), accent badge `LIVE NOW` top-left.
- **Featured catalog**: section head = `FEATURED CATALOG` (Archivo 900, 44px) left + mono meta right (`03 OF 12 ITEMS — VIEW ALL` underlined link), both on a 1px ink bottom rule, `padding: 48px 0 12px`, page has 24px side padding. Below: 3-column product-card grid (see Product Card). Wire to a Shopify collection (featured), section setting for which collection + count.
- **Newsletter box** ("legal form" treatment): centered, max-width 840px, white bg, 1px ink border, `padding: 36px 40px`. Mono eyebrow `FORM 88-A — MAILING LIST DECLARATION` (10px, 0.16em, `#6b6b6b`); title `GET THE RELEASE LOG` (Archivo 900, 32px). Input row: label `ELECTRONIC MAIL ADDRESS *` (mono 10px above field), input transparent with only a 1px ink bottom border, mono 12px uppercase text; SUBMIT button (outline style) beside it. Success state: 1px accent border box, accent mono text `LOGGED. CONFIRMATION DISPATCHED.` Fine print: mono 9px `#9a9a9a`, line-height 1.7, consent copy. Wire to Shopify customer form.

### 3. Catalog / Collection page
- Section head: `CATALOG` (Archivo 900, 56px) + right mono meta `NN ITEMS INDEXED` (count zero-padded), on 1px ink rule.
- Layout: grid `220px 1fr`, 24px side padding. Sidebar has 1px ink right border.
- **Filters** (sidebar): group labels `FILTER / FIT` and `FILTER / SIZE` (mono 10px, 0.16em, muted, 12px bottom margin). Options: Fit = Boxy / Heavy / Classic; Size = S / M / L / XL. Each row: 13px square checkbox (1px ink border, white bg; checked = solid ink fill, no checkmark glyph) + mono 11px 0.12em label, `padding: 5px 0`, whole row clickable. `RESET ALL` underlined mono link below. In Shopify: map to native collection filters/tags; filtering updates the grid.
- **Grid**: 3 columns of product cards, collapsed 1px ink borders (12 per page).
- **Pagination**: centered row `padding: 28px 0 48px`, mono 11px 0.14em uppercase: `< PREV   PAGE 01 OF 03   NEXT >`. Prev/Next muted `#6b6b6b`, ink on hover; page numbers zero-padded.

### 4. Product detail page
- Grid `1.1fr 1fr`, `align-items: start`; left column has 1px ink right border.
- **Left**: two images stacked vertically (front, back), no carousel. Each aspect ~4/4.6, 1px ink bottom border, corner code top-right (`DS-00N/F`, `/B`), caption bottom-left (`FRONT VIEW — MOCK-UP` etc.). Use product media; placeholder art as fallback (back panel stripes run −45deg, glyph mirrored).
- **Right (sticky)**: `position: sticky; top: 52px` (header height), `padding: 40px 40px 56px`:
  - Breadcrumb: `CATALOG / DS-001` (mono 10px, muted, CATALOG underlined link)
  - Title: Archivo 900, 52px, uppercase
  - Price `$45.00 USD` (mono 15px, 0.1em) + optional badge inline
  - **Made-to-order banner**: solid accent bg, white mono 10px 0.13em text, `padding: 11px 14px`: `// MADE TO ORDER — CUT ON DEMAND, ALLOW 5–7 DAYS BEFORE DISPATCH`
  - **Size selector**: label `SELECT SIZE` (mono 10px muted), then 5-col grid 6px gap of square buttons S/M/L/XL/XXL — 1px ink border, `padding: 12px 0`, mono 11px, white bg/ink text; **active = solid accent bg, white text**. Map to Shopify variant picker.
  - **Add to cart**: full-width, solid ink bg, paper text, 1px ink border, `padding: 16px 0`, mono 12px 0.16em: `ADD TO CART — $45.00`; hover = accent bg/border. Sold-out state: disabled, `#eee8dc` bg, `#9a9a9a` text, `#d9d4ca` border, `SOLD OUT — PRODUCTION CLOSED`. Adding opens the cart drawer (AJAX cart).
  - **Accordions** (3): stacked on 1px ink rules (top rule on group, bottom rule per item). Header row: mono 11px 0.14em title left, `+` / `−` right, `padding: 14px 0`. Items: `SIZING & FIT` (open by default, contains spec table), `GARMENT WEIGHT (240GSM)`, `NINJA POD PRODUCTION POLICY` (body: 13px Inter, `#2a2a2a`, max-width 440px). Source from product metafields.
  - **Spec table**: no fills; rows `grid: 150px 1fr`, `padding: 9px 0`, 1px `#d9d4ca` rules between rows. Key: mono 10px 0.12em muted; value: Inter 13px. Rows: Fit / Fabric (100% organic cotton, 240GSM) / Collar (Mitered neck collar, twin-needle bound) / Hem / Origin.

### 5. Cart drawer ("The Manifest")
- Slides in from right; width 35vw, min-width 400px; white bg; **3px solid ink left border**; backdrop `rgba(10,10,10,0.45)`, click closes. Add a slide-in transition (~250ms ease-out) — the mockup shows it instantly.
- Header: `YOUR CART [02 ITEMS]` (mono 12px, 0.16em, bold, count zero-padded) + underlined CLOSE link, `padding: 18px 24px`, 1px ink bottom rule.
- Item rows (scrollable middle): `padding: 18px 24px`, 1px `#d9d4ca` bottom rule, flex with 16px gap: 64px square thumb (1px `#d9d4ca` border, placeholder stripes fallback) · name (Inter 600, 12px, uppercase) + `DS-00N / SIZE M` (mono 9px muted) + qty controls · line price (mono 12px). Qty controls: `[−] 1 [+]` — 20px square 1px-ink-border buttons, mono, hover inverts to ink/paper; decrement at qty 1 removes the line. Empty state: `CART IS EMPTY. NOTHING MANIFESTED.` (mono, muted).
- Footer (1px ink top rule, `padding: 20px 24px`): SUBTOTAL row (mono 12px, justified), shipping notice (mono 9px `#9a9a9a`): `SHIPPING + DUTIES CALCULATED AT CHECKOUT. MADE-TO-ORDER GOODS ARE FINAL SALE.`, then full-width checkout button: solid ink, `padding: 18px 0`, mono 13px 0.18em, `CHECKOUT — $73.00 USD`, hover → accent. Links to Shopify checkout.

## Components (reusable snippets)
1. **Product card**: white/paper cell, 1px collapsed ink borders, hover bg `#eee8dc`, whole card links to product. Art area: square (aspect-ratio 1), stripes + centered tee glyph 16% opacity (38% width), 1px `#d9d4ca` bottom rule; mono 10px corner code top-right; badge top-left; caption `PLACEHOLDER GARMENT ART` bottom-left (mono 9px `#9a9a9a`) — replaced by product image when present. Info row `padding: 14px 16px`, space-between: name (Inter 600 13px uppercase, 0.04em) over fit label (mono 10px muted, e.g. `HEAVY FIT / 240GSM`); price right (mono 12px, 0.08em).
2. **Badges**: mono 9px, 0.14em, uppercase, white text, `padding: 3px 8px`, square. Ink bg: `SOLD OUT`, `HEAVYWEIGHT`; accent bg: `PRE-ORDER`, `LIVE NOW`.
3. **Buttons**: mono 11px 0.14em uppercase, `padding: 13px 26px`, 1px border, square, instant inversion on hover (no transition needed, or ≤100ms). Variants — outline: transparent bg/ink text → ink bg/paper text; primary: ink bg/paper text → accent bg/white; accent: accent bg/white → ink bg.
4. **Form input**: transparent bg, no side borders, 1px ink bottom border only; mono 10px 0.14em muted label floated above; value mono 12px uppercase.
5. **Placeholder garment art**: `repeating-linear-gradient(45deg, #eee8dc 0 10px, #f5f1ea 10px 20px)` + centered tee SVG (path in mockup source, fill `#0a0a0a`, opacity 0.16) + mono corner code + mono caption. Use as image fallback everywhere.

## Interactions & Behavior
- All hover states are **instant color inversions** (see Buttons) or bg shift to `#eee8dc` (cards); links get ink underline or accent color.
- Cart is AJAX: add-to-cart and qty changes update without page reload and open/refresh the drawer; header count stays in sync (zero-padded).
- Accordions toggle independently; first open by default on PDP.
- Numbers presented as "data": zero-pad counts (`02`, `03 OF 12`), prices always 2 decimals, `USD` suffix on PDP/cart totals.
- Responsive (mockup is desktop 1440px — use judgment): hero stacks to 1 column; catalog grid 3 → 2 → 1; sidebar filters collapse to a top bar / disclosure on mobile; cart drawer full-width on mobile; header nav can collapse to a mono menu. Keep 1px-rule structure throughout.

## State Management (Shopify mapping)
- Products: 12 demo items DS-001…DS-012 (Heavy Tee $45, Boxy Tee $42, Work Shirt $88, Cargo Pant $110 sold-out, Chore Coat $140 pre-order, Longsleeve $54, Canvas Tote $28, Deck Hood $96, Pocket Tee $46, Painter Pant $104, Thermal $58 pre-order, Watch Cap $32) — replace with real store products. Codes like `DS-001` map well to SKU or a metafield.
- Fit (Boxy/Heavy/Classic), badges, specs, accordion copy → tags/metafields.
- Sizes S–XXL → variants. Sold out → variant availability.
- Theme settings: accent color, ticker text + toggle, featured collection.

## Assets
No external assets. Fonts from Google Fonts (Archivo, Inter, JetBrains Mono — self-host in `assets/` for the theme). The tee glyph SVG path is in the mockup source (`DA SHOP.dc.html`, search `viewBox="0 0 90 70"`).

## Files
- `DA SHOP.dc.html` — the full interactive mockup (all pages + cart drawer). Template markup at top; data/behavior in the `Component` class script below it. It is a design-tool prototype format: read it for exact values and structure, do not port its runtime.
