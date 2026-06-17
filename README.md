# Nellikuru Innovations

Marketing website for **Nellikuru Innovations Pvt. Ltd.**, an R&D startup in iron and
steel making in India. The site presents two cast-house solutions: hot metal
desiliconization (the patented Nellikuru Process) and a hot metal insulation compound
for yield improvement.

Static, hand-built, no framework and no build step. Served from the repository root by
GitHub Pages on push to `main`.

## Stack
- Semantic HTML5, one design-system stylesheet ([css/main.css](css/main.css)), and one
  small progressive-enhancement script ([js/main.js](js/main.js)). Works with JS disabled.
- Self-hosted Inter + Space Grotesk fonts (woff2, latin) in [fonts/](fonts/). No CDN.
- Directory ("pretty") URLs with depth-aware relative paths, so the site works both at the
  production domain root and on the GitHub Pages project subpath preview.

## Pages
| URL | File |
| --- | --- |
| `/` | `index.html` |
| `/about/` | `about/index.html` |
| `/services/` | `services/index.html` |
| `/gallery/` | `gallery/index.html` |
| `/faq/` | `faq/index.html` |
| `/contact/` | `contact/index.html` |
| 404 | `404.html` (root) |

## Standards
SEO meta + sitemap/robots; schema.org JSON-LD (`Organization` + `WebSite`,
`BreadcrumbList`, a `Service` `ItemList`, `ImageGallery`, `FAQPage`, `ContactPage`);
WCAG 2.1 AA (audited contrast); no em dashes; email-led contact with no backend.

See [CLAUDE.md](CLAUDE.md) for the design system and conventions and
[DECISIONS.md](DECISIONS.md) for the decisions log. The previous builder site is kept in
[archive/](archive/) and disallowed in `robots.txt`.

## Local preview
```
python3 -m http.server 8000
```
Then open http://localhost:8000/.
