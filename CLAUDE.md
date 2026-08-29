# Nellikuru Innovations website: design system, conventions and decisions

Working brief for anyone (human or AI) maintaining the Nellikuru site. The site is a
hand-built static site: semantic HTML5, one CSS file, one small JS file, no framework
and no build step. It deploys from the repo root via GitHub Pages.

Business: Nellikuru Innovations Pvt. Ltd., an R&D startup in iron and steel making in
India (Make in India). Two solutions for the cast house: hot metal desiliconization
(the patented Nellikuru Process) and a hot metal insulation compound (yield
improvement). Contact email: **sabudominic1@gmail.com** (email-led, no backend).

---

## 1. File structure

```
/                       repo root (served by GitHub Pages)
  index.html            Home, served at /
  about/index.html      About, at /about/
  services/index.html   Solutions (2, with #desiliconization #insulation-compound), at /services/
  gallery/index.html    Gallery (ImageGallery), at /gallery/
  faq/index.html        FAQ (FAQPage schema), at /faq/
  contact/index.html    Contact (email-led, no backend), at /contact/
  404.html              Styled not-found (stays at root; root-absolute asset paths)
  robots.txt            Allows all, disallows /archive/, points to sitemap
  sitemap.xml           Six public URLs (directory form, trailing slash)
  site.webmanifest      PWA manifest
  css/main.css          The entire design system
  js/main.js            Progressive enhancement only
  fonts/                Self-hosted woff2 (Inter, Space Grotesk), latin subset
  imgs/                 logo.webp, favicon.*, icons, og-image.jpg, hero/about/product
  imgs/gallery/         gallery-1..6.webp (genuine industrial photos)
  tools/                Dev-only (contrast-audit.mjs); gitignored deps, not deployed
  archive/              The previous builder-exported site (not linked, disallowed)
```

No shared HTML partials (no build step), so header, footer and the floating button are
duplicated across pages. **Change one, change all**, keeping them identical except the
`aria-current="page"` on the active nav item.

---

## 2. Brand and colour

Derived from `imgs/logo.webp` (a red orb mark with the NK monogram on charcoal):
**red accent + charcoal/steel on a cool light field**, an industrial palette. The
header is dark (steel) so the light logo reads and red pops. Tokens live in `:root`.

Every pairing is verified WCAG 2.1 AA. Re-run the audit (section 8) after colour changes.

| Token | Hex | Use | Contrast |
| --- | --- | --- | --- |
| `--light` | `#f4f5f6` | Page background | base |
| `--surface` | `#ffffff` | Cards | base |
| `--ink` | `#1b1e23` | Primary text | 15.3:1 on light |
| `--ink-2` | `#4a525c` | Secondary text | 7.3:1 |
| `--ink-3` | `#5b636d` | Muted / meta text | 5.6:1 |
| `--steel` | `#20242b` | Dark sections | white 15.6:1 |
| `--steel-2` | `#15181d` | Header / footer | white high |
| `--steel-muted` | `#c2c8cf` | Secondary text on steel | 9.2:1 |
| `--red` | `#d12128` | Brand fill, button bg (white label 5.3:1), accent on light (>=3) |
| `--red-strong` | `#bf1f25` | Borders / focus / icons on light | 5.6:1 |
| `--red-ink` | `#b01b21` | Red **text / links** on light | 6.4:1 |
| `--red-on-dark` | `#ff7a7e` | Red text / UI on steel | 6.2:1 |

**The one rule that keeps AA valid on dark:** plain `--red` is only 2.94:1 on `--steel`,
below the 3:1 UI bar. On steel sections, any meaningful red element (eyebrow bar, icons,
text) must use **`--red-on-dark`**, never `--red`. Buttons are `--red` fill with white
text (5.3:1). Focus is a 3px `--red-strong` ring on light.

### Colour belongs to the component, not the container
A container selector that sets `color` on bare element descendants (e.g. `.section-head p`,
specificity 0-1-1) silently out-specifies a single-class component such as `.eyebrow`
(0-1-0) nested inside it. Those rules are scoped `:not(.eyebrow)`. Never let a dark-only
colour (`--steel-muted`) reach a light surface through the cascade. Verify with the
DOM-aware audit, not by eye.

---

## 3. Typography
- Headings: **Space Grotesk** (technical grotesque), weights 500/600/700.
- Body and UI: **Inter**, weights 400/500/600/700.
- Self-hosted woff2 (latin subset), `font-display: swap`; `inter-400` and
  `space-grotesk-700` preloaded per page. No Google Fonts link, ever.

---

## 4. URL convention: directory ("pretty") URLs, path-portable
Each page is a folder with `index.html`, served at a trailing-slash path
(`about/index.html` at `/about/`). Home is `index.html` at `/`; `404.html` stays at root.

The production domain is the custom domain **https://www.nellikuru.com/** (root), but the
preview is a GitHub Pages **project page** at `https://propagetech.github.io/nellikuru.com/`
(a subpath). So:
- **Assets and internal links are RELATIVE and depth-aware**, never root-absolute.
  Home uses `css/...`, `about/`, home link `./`. Inner pages use `../css/...`,
  `../services/`, home link `../`. Root-absolute `/css/...` breaks on the project subpath.
- Exception: **`404.html` stays root-absolute** (GitHub serves it at arbitrary depths;
  correct for the production root).
- **Canonical, `og:url`, sitemap `loc` and JSON-LD `@id` stay ABSOLUTE** on
  `https://www.nellikuru.com/slug/`, wherever the site is previewed.
- Link to the trailing-slash form (`about/`); `/about` 301-redirects to `/about/`.

---

## 5. Accessibility, SEO, schema, copy
- One `<h1>` per page; ordered headings; landmarks; a `.skip-link` to `#main`; mobile nav
  with `aria-expanded`/`aria-controls`, Escape to close, and a no-JS fallback; visible
  focus rings; alt text on imagery; `prefers-reduced-motion` respected; image width/height.
- Per-page unique title/description/canonical/OG/Twitter; `og:image` is
  `/imgs/og-image.jpg` (1200x630). JSON-LD: `Organization` (`@id` `#organization`) +
  `WebSite` + `WebPage` on Home; `BreadcrumbList` on inner pages; a `Service` `ItemList`
  on Solutions; `ImageGallery` on Gallery; `FAQPage` on FAQ (answers must match the
  visible text); `ContactPage` on Contact.
- **No em dashes** in copy. Service names stay capitalised consistently. Contact is
  **email-led, no backend**: the contact form composes a `mailto:` draft (native mailto
  fallback with JS off); nothing is stored, and visitors are asked not to send confidential
  plant data until a secure channel exists.

### Gallery and imagery note
`imgs/gallery/*` and the hero/about/product images are genuine iron-and-steel / foundry
photos carried over from the client's existing site. The original builder gallery also
contained unrelated stock (portraits, craft, derelict scenes); those were deliberately
excluded. Keep only on-topic cast-house imagery.

---

## 6. Decisions log
See **[DECISIONS.md](DECISIONS.md)** (newest first).

---

## 7. Validation checklist (run before deploy)
```bash
grep -rn "—" *.html */index.html && echo FAIL || echo "OK: no em dashes"   # em dashes
python3 -m http.server 8123 &                       # serve
(cd tools && npm i playwright-core)                  # one-time, uses installed Chrome
node tools/contrast-audit.mjs                        # DOM-aware AA audit, exit 1 on fail
# Subpath preview check: serve the PARENT dir, load /nellikuru.com/ in a browser, and
# confirm 0 asset 404s (root-absolute paths would break there; relative paths must be used).
```
Also: every JSON-LD block parses, one `<h1>` per page, every internal link and #anchor
resolves, and the site works with JavaScript disabled.

---

## Design skills (house)

Visual polish follows the **`refactoring-ui`** skill (hierarchy, spacing scale, type scale,
HSL/OKLCH ramps, depth, imagery, finishing touches). Load it for any CSS/UI pass.

- Skill: `~/.claude/skills/refactoring-ui/` (also `~/.cursor/skills/refactoring-ui/`)
- Human PDF (do not paste book text here): `/Users/chetan/Downloads/Learning/refactoring-ui_compress 2.pdf`
- Full rebuilds: `site-rebuild` + `../_rebuild-kit/` — ProPage invariants (WCAG AA, real logo,
  photos-first, type-by-register, no em/en dashes) override generic taste.
