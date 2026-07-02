# Kanz — كنز

Single-file, bilingual (Arabic/English) marketplace homepage prototype, styled after the Amazon.eg storefront. Zero build step: one `kanz.html` file that runs directly in any modern browser.

---

## Stack

| Layer | Choice | Notes |
|---|---|---|
| Markup/logic | Vanilla HTML + JS | No framework, no bundler |
| Styling | Tailwind CSS (Play CDN) | Runtime compile; utility classes only |
| Icons | Lucide (UMD CDN) | `data-lucide` placeholders, re-init per render |
| Fonts | Google Fonts — Cairo, Tajawal | Arabic-first typography |
| State | Plain JS store (`S`) + full re-render | Targeted DOM updates for cart, countdown, hero |

No backend. All data is static in-file (`DEAL_PRODUCTS`, `TOP_PICKS`, `CATEGORIES`, `HERO_SLIDES`, `MEDALLIONS`, dictionaries `M`).

---

## Features

- **Bilingual + RTL** — Arabic (default) / English, full layout mirroring via `dir` and logical CSS properties (`inset-inline-*`, `me/ms`). Numbers, currency (EGP) and counts localized with `Intl.NumberFormat` (`ar-EG` / `en-EG`).
- **Light / dark theme** — CSS-variable token sets; Amazon-style light palette + matching dark mode. Respects `prefers-color-scheme` on first load.
- **Header** — dark top bar, deliver-to, search with typeahead (keyboard nav: ↑/↓/Enter/Esc, click-outside close), language + theme toggles, account popover, cart badge.
- **Category nav** + slide-in **drawer** with nested sub-panels (Shop by Category), "See all" expansion, and in-drawer language/theme controls.
- **Hero carousel** — auto-advancing (6s), pause on hover/focus, dot + arrow controls, respects `prefers-reduced-motion`.
- **Merch sections** — overlapping promo cards, trend row, "Welcome Deals" band with a live daily **countdown**, round budget medallions, essentials row, and horizontally scrollable product shovelers (RTL-aware scrolling).
- **Product cards** — rating stars, review counts, discount badges, strikethrough pricing, add-to-cart with toast + live cart count.
- **A11y** — skip link, focus-visible outlines, `aria` roles on combobox/dialog/carousel, live regions for toast and cart.

---

## Run

Open `kanz.html` in a browser. That's it.

For a local server (recommended so CDNs and fonts load consistently):

```bash
python3 -m http.server 8000
# visit http://localhost:8000/kanz.html
```

> Requires internet on first load (Tailwind, Lucide, Google Fonts are CDN-hosted).

---

## Structure (single file)

```
kanz.html
├─ <style>            base CSS, RTL flip helpers, scrollbar/clamp utilities
└─ <script>
   ├─ THEMES          light/dark CSS-variable token sets
   ├─ M               ar/en string dictionaries
   ├─ data            CATEGORIES, HERO_SLIDES, DEAL_PRODUCTS, TOP_PICKS, MEDALLIONS
   ├─ helpers         locale detect, Intl formatters, icon() builder
   ├─ S               UI state (locale, theme, drawer, cart, hero, search…)
   ├─ section builders  header / nav / drawer / hero / overlapRow / trendRow /
   │                    dealBand / medallionStrip / essentialsRow / topPicks / footer
   ├─ render()        composes root, injects vars, re-inits Lucide, wires events
   └─ wire()          event delegation for all interactive elements
```

---

## Customization

| Task | Where |
|---|---|
| Colors / theme | `THEMES.light` / `THEMES.dark` CSS variables |
| Copy / translations | `M.ar` / `M.en` |
| Products & deals | `DEAL_PRODUCTS`, `TOP_PICKS` |
| Hero slides | `HERO_SLIDES` |
| Categories & drawer | `CATEGORIES`, `EXTRA_CATEGORIES` |
| Default language | `detectLocale()` / `S.locale` |
| Countdown target | `tickCountdown()` (currently end-of-day) |

Product/hero visuals are Lucide glyphs on gradient tiles — swap for real `<img>` assets in `productCard()` / `heroHTML()` when connecting to real data.

---

## Known limitations (prototype scope)

- Static data only; search, add-to-cart, and links are non-persistent stubs.
- Tailwind Play CDN is dev-grade (in-browser compile) and needs network.
- No routing, no PLP/PDP, no auth or checkout.

### Production path

1. Replace Tailwind Play CDN with a prebuilt CSS (Tailwind CLI) and self-host.
2. Inline the ~15 used Lucide SVGs (or self-host) to drop the icon CDN and run offline.
3. Move data behind an API; wire cart/search to real state.
4. Split into components if migrating to a framework (the section-builder boundaries map 1:1 to components).

---

## Branding note

Uses the original **Kanz** name and gem mark. The layout and color language are inspired by Amazon.eg, but Amazon's wordmark/logo are intentionally not reproduced — substitute your own brand assets where the gem seal renders.

---

## License

Prototype / original design. Add a license before distribution.
