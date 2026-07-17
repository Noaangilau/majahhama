# DA SHOP — Shopify theme

A Shopify Online Store 2.0 theme for **DA SHOP**, a made-to-order clothing
store, built by forking [Dawn](https://github.com/Shopify/dawn) (v15.5.0) and
restyling it into a "utilitarian/industrial editorial" look — spec-sheet
meets zine. See [docs/design-reference/HANDOFF.md](docs/design-reference/HANDOFF.md)
for the full design spec and [docs/design-reference/DA SHOP.dc.html](docs/design-reference/DA%20SHOP.dc.html)
for the original interactive mockup.

## What's custom here (vs. stock Dawn)

- `assets/da-shop.css` — design tokens (colors, type, spacing) and component
  overrides, loaded after Dawn's `base.css`.
- `assets/archivo-variable-latin.woff2`, `inter-variable-latin.woff2`,
  `jetbrains-mono-variable-latin.woff2` — self-hosted fonts, wired up in
  `layout/theme.liquid`.
- `sections/da-shop-ticker.liquid` — the scrolling announcement marquee
  (replaces Dawn's `announcement-bar` in `sections/header-group.json`).
- `sections/da-shop-hero.liquid` — the split-screen homepage hero.
- `snippets/da-shop-placeholder-art.liquid` — the striped "no photography yet"
  garment art (corner code, caption, optional badge), used as the image
  fallback on product cards and the hero.
- `snippets/card-product.liquid` — Dawn's product card, extended with a
  corner code, a fit label (from the product's Type field), and the
  placeholder art fallback.
- `sections/main-product.liquid` — extended with two new block types:
  `made_to_order_banner` (static text) and `spec_table` (an accordion whose
  rows are read from the product metafields `custom.fit`, `custom.fabric`,
  `custom.collar`, `custom.hem`, `custom.origin` — blank fields are skipped).
- `config/settings_schema.json` / `settings_data.json` — a `da_accent_color`
  theme setting, plus new color schemes (`scheme-6` white panels, `scheme-7`
  ink/paper) used by the drawer, newsletter box, ticker, and footer.
- Cart drawer copy ("Nothing manifested.", item count, checkout total) is
  edited in `locales/en.default.json` and `snippets/cart-drawer.liquid`.

Everything else (cart AJAX, checkout, variant logic, accessibility, search)
is untouched Dawn.

## Local development

This repo has the theme files but no Shopify CLI session or store connected.
To preview it against a real store:

```sh
npm install -g @shopify/cli
shopify theme dev --store your-store.myshopify.com
```

Push as an unpublished theme for review before publishing:

```sh
shopify theme push --unpublished
```

## Known gaps / next steps

- Product metafields (`custom.fit`, `custom.fabric`, `custom.collar`,
  `custom.hem`, `custom.origin`) need to be created in the Shopify admin
  (Settings → Custom data → Products) before the spec table will show real
  values — until then it falls back to the block's fallback text.
- The homepage "Featured catalog" section and collection grid reuse Dawn's
  stock section/card architecture rather than a byte-for-byte layout clone;
  visual details (grid borders, mono type, badges) match the spec, but a few
  structural bits (e.g. the "view all" link position) follow Dawn's default
  placement.
- Fonts are self-hosted as single variable-font files (each covering all the
  weights the design needs) rather than one static file per weight.
