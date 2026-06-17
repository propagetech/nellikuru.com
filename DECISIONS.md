# Decisions log

A short record of the choices made when rebuilding the Nellikuru Innovations site, so
future work has the reasoning, not just the result. Newest first.

## 2026-06-15: Full rebuild from the existing builder site

**Archived the old site.** The previous single-page Viamagus/ProPage brochure export
(`index.html`, `404.html`, `about.html`, `contact.html`, `gallery.html`, builder `css/`,
`js/`, `fonts/`, `imgs/`, and the `rename-*` artifacts) was moved into `archive/` and is
disallowed in `robots.txt`. The new site is hand-built static HTML5 + one CSS file + one
small JS file, deploying from the repo root via GitHub Pages.

**Content came from the client's own copy.** The About and Solutions text was rewritten
for clarity from the existing site (R&D startup in iron and steel making under Make in
India; hot metal desiliconization via the patented Nellikuru Process; hot metal insulation
compound for yield improvement). The contact details on the old site were builder
placeholders, so the only real contact, `info@nellikuru.com`, is used and the flow is
email-led with no backend and no invented phone or address.

**Brand and type.** Colours were derived from `imgs/logo.webp` (red orb mark on charcoal):
an industrial red + charcoal/steel palette on a cool light field, with a dark header so
the light logo reads and red pops. Headings use Space Grotesk and body uses Inter, both
self-hosted woff2 (latin), with no external CDN. The whole palette is WCAG 2.1 AA verified;
on dark steel sections red elements use `--red-on-dark` because plain `--red` is only
2.94:1 on steel.

**Imagery.** The original builder gallery mixed genuine iron-and-steel photos with
unrelated stock (portraits, craft studios, a derelict-building scene). Only the genuine
cast-house images were kept, for the hero, about, the two product blocks and a six-image
gallery. The favicon and icon set were rebuilt from the logo's red play-orb mark, and the
OG card renders the brand font over a steel-plant photo.

**URLs are directory style and path-portable.** Pages are folders with `index.html`,
served at trailing-slash paths, with depth-aware relative asset and link paths so the site
works both at the production root domain (`www.nellikuru.com`) and on the GitHub Pages
project subpath preview (`propagetech.github.io/nellikuru.com/`). `404.html` stays
root-absolute. Canonical, OG, sitemap and JSON-LD stay absolute on the production domain.

**Standards.** Per-page SEO meta and JSON-LD (`Organization` + `WebSite` on Home,
`BreadcrumbList` on inner pages, a `Service` `ItemList` on Solutions, `ImageGallery` on
Gallery, `FAQPage` on FAQ, `ContactPage` on Contact), WCAG 2.1 AA (audited with
`tools/contrast-audit.mjs`), no em dashes, and a working no-JavaScript experience.
