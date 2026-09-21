# Kamelo public site

Static site (plain HTML and CSS, no framework, no external fonts, scripts, cookies or trackers) published with GitHub Pages from the public repository `Fox1304/kamelo-site` at **https://fox1304.github.io/kamelo-site/**. The apps link to `privacy.html`, `terms.html` and `children.html`; the store listings use the same URLs.

| Page | Content | Source |
|---|---|---|
| `index.html` | Landing page (FR, EN toggle) | `tools/pages/index.html` |
| `privacy.html` | Privacy policy FR + EN | `docs/legal/PRIVACY_POLICY.md` |
| `terms.html` | Terms of use and legal notice FR + EN | `docs/legal/TERMS.md` |
| `support.html` | Contact, FAQ, data deletion, subscription management | `tools/pages/support.html` |
| `children.html` | Notice for children (6–11) FR + EN | `tools/pages/children.html` |
| `press.html` | Press kit basics | `tools/pages/press.html` |
| `404.html` | Not-found page (absolute URLs, works at any path) | `tools/pages/404.html` |
| `style.css`, `favicon.svg`, `robots.txt`, `.nojekyll` | Hand-written, not generated | — |

## The legal pages mirror `docs/legal/*.md`

`privacy.html` and `terms.html` are **generated**: never edit them by hand. The Markdown files in `docs/legal/` are the single source of truth; the build keeps the text identical and only adds HTML structure (sections, table of contents, responsive tables), French non-breaking spaces before « ; : ! ? » and a yellow highlight on every `[FOUNDER TO CONFIRM]` marker.

To change a legal text:

1. Edit `docs/legal/PRIVACY_POLICY.md` or `docs/legal/TERMS.md`, in **both** the French and the English section (each starts with a `<!-- lang: fr -->` / `<!-- lang: en -->` marker and a `#` title). Update the version line (`*Version 1.1 — date*`).
2. Run `python3 site/tools/build_site.py` from the repository root (Python 3.9+, no dependencies).
3. Check the result: `python3 site/tools/build_site.py --check` prints `PASS site_build` when every page is up to date (use it in CI).
4. For a significant change, tell families in the app before it applies (privacy policy §10, terms §16).
5. Publish: copy `site/` to the `kamelo-site` repository (everything except `tools/`) and push; Pages redeploys in about a minute.

Before publishing outside the beta, replace every `[FOUNDER TO CONFIRM]` marker (postal address and phone, SIREN, consumer mediator, UK representative, DPO decision, and the L.215-1 renewal reminder).

## How the pages work

- **Languages**: every page contains French and English. A tiny inline script shows one language: the URL hash (`#fr`, `#en`, or an anchor such as `#en-your-rights`) wins, then the browser language, then French. The FR/EN switch changes language without reloading. Without JavaScript both languages are shown one after the other. Nothing is stored on the device.
- **Security**: each page carries a Content-Security-Policy that only allows the site’s own files and the inline script, identified by its SHA-256 hash. If you edit the script in `tools/build_site.py`, rebuild: the hash is recomputed.
- **Design**: colours from `shared/design-tokens/tokens.json` (cream canvas, ink, Kamelo teal, subject colours), system rounded fonts, body text ≥ 17 px, automatic dark mode, visible focus rings, AA contrast, layouts tested down to 360 px wide, animations disabled when “reduce motion” is on. Kamelo on the landing page changes colour when you press a subject button.
- **Links**: internal links are relative; only `404.html` uses absolute URLs, because GitHub Pages serves it at any missing path.

## When the domain changes

When `kamelo.app` (or another domain) points to Pages: add a `CNAME` file, set `BASE_URL` in `tools/build_site.py`, rebuild (canonical URLs and the 404 page use it), update `SITE_URL` in `.env` and the store listings.
