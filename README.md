# ETERNA Holdings — Landing Page

Marketing landing page for ETERNA Holdings, a Tanzanian diversified holdings
company operating across logistics and procurement, commodities trading,
marketing, real estate and cleaning services.

**Live site:** https://ester098.github.io/eterna2007/

## Contents

| File | Purpose |
| --- | --- |
| `index.html` | The complete landing page. Self-contained: all CSS, JavaScript and imagery (20 data-URI images: JPEG, PNG and WebP) are inlined. Includes the "Ask ETERNA" AI assistant and the quotation builder. |
| `og-image.png` | Social preview image, referenced by `og:image` and by the Organization schema logo. |
| `robots.txt` | Allows all crawlers, points to the sitemap. |
| `sitemap.xml` | Single-URL sitemap for the live site URL. |
| `.nojekyll` | Tells GitHub Pages to serve files as-is instead of running Jekyll. |
| `.github/workflows/pages.yml` | Deploys the site to GitHub Pages on every push to `main`. |

The only external dependency is Google Fonts (Cormorant Garamond + Inter),
loaded over CDN. The stylesheet is loaded non-blocking (`media="print"` swapped to
`all` on load), so a slow or unreachable `fonts.googleapis.com` cannot hold up the
first paint — the page renders immediately in Georgia / system sans and swaps when
the webfonts arrive. Loading it render-blocking meant a phone that could not reach
Google Fonts showed a blank screen until the request timed out.

The intro curtain is an opaque full-screen overlay that also locks scrolling, and
only the main script at the end of the document lifts it. A small failsafe near the
top of the body clears it unconditionally after 6s (the normal sequence takes ~3s)
so a script failure can never leave a visitor on a blank panel, and a `<noscript>`
rule hides it outright when JavaScript is off.
`og-image.png` is the one asset that is not inlined — social scrapers cannot read
data URIs, so it is served as a file at the site root.

## The AI assistant

The "Ask ETERNA" widget runs in two tiers:

1. **Local engine (always on).** Intent detection, slot-filling and a dialogue
   policy over the group's own knowledge base, running entirely in the browser.
   It needs no server and works as-is on GitHub Pages.
2. **Live model (optional).** If a `POST /api/chat` endpoint is configured and
   reachable, replies come from it instead. When it is unavailable — as on
   static hosting — the widget falls back silently to the local engine, so the
   visitor never sees an error.

To enable the live tier you need a host that can run server code (Pages cannot).
Point the site at a backend exposing `POST /api/chat` that accepts
`{"messages":[...]}` and returns `{"reply":"..."}`.

The enquiry form behaves the same way: it posts to the `data-endpoint` on the
form if one is set, and otherwise opens a pre-filled email so nothing is lost.

## Publishing

The site is served by GitHub Pages from the `gh-pages` branch.

`main` is the source of truth — edit files there. Every push to `main` runs
`.github/workflows/pages.yml`, which assembles the site files and force-pushes
them to `gh-pages`; GitHub Pages then rebuilds and deploys automatically. You
can also trigger it by hand from the **Actions** tab via **Run workflow**.

Do not commit directly to `gh-pages` — the workflow overwrites it on every run.

### Site URL

The site's own URLs — the canonical link, the `og:` tags, the JSON-LD `@id`
values, `sitemap.xml` and `robots.txt` — all point at
`https://ester098.github.io/eterna2007/`, which is where the site is served
from. The `info@eternaholdings.com` contact address is unrelated to hosting and
is left as-is.

Note that `robots.txt` is only honoured at a domain root
(`https://ester098.github.io/robots.txt`), so the copy in this project path is
not read by crawlers. It becomes effective on a custom domain.

### Moving to a custom domain

To serve from `www.eternaholdings.com` later:

1. Point a DNS `CNAME` record for `www` at `ester098.github.io`.
2. Add a `CNAME` file at the repository root containing `www.eternaholdings.com`.
   (The workflow copies a root `CNAME` file into the deployed site automatically —
   which matters here, because the workflow force-pushes `gh-pages` on every run
   and would otherwise wipe a `CNAME` that GitHub wrote there.)
3. In **Settings → Pages**, enter the custom domain and enable **Enforce HTTPS**.
4. Swap `https://ester098.github.io/eterna2007` for `https://www.eternaholdings.com`
   in `index.html`, `sitemap.xml` and `robots.txt`.

## Editing

`index.html` is a single hand-written file — open it directly. Brand tokens are
defined as CSS custom properties in the `:root` block near the top
(navy `#0B2D5B`, gold `#C9A227`, type scale, spacing scale).
