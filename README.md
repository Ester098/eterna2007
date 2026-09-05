# ETERNA Holdings — Landing Page

Marketing landing page for ETERNA Holdings, a Tanzanian diversified holdings
company operating across logistics and procurement, commodities trading,
marketing, real estate and cleaning services.

**Live site:** https://ester098.github.io/eterna2007/

## Contents

| File | Purpose |
| --- | --- |
| `index.html` | The complete landing page. Self-contained: all CSS, JavaScript and imagery (5 data-URI images) are inlined. |
| `robots.txt` | Allows all crawlers, points to the sitemap. |
| `sitemap.xml` | Single-URL sitemap for `https://www.eternaholdings.com/`. |
| `.nojekyll` | Tells GitHub Pages to serve files as-is instead of running Jekyll. |
| `.github/workflows/pages.yml` | Deploys the site to GitHub Pages on every push to `main`. |

The only external dependency is Google Fonts (Cormorant Garamond + Inter),
loaded over CDN. The page degrades to Georgia / system sans if that is blocked.

## Publishing

The site is served by GitHub Pages from the `gh-pages` branch.

`main` is the source of truth — edit files there. Every push to `main` runs
`.github/workflows/pages.yml`, which assembles the site files and force-pushes
them to `gh-pages`; GitHub Pages then rebuilds and deploys automatically. You
can also trigger it by hand from the **Actions** tab via **Run workflow**.

Do not commit directly to `gh-pages` — the workflow overwrites it on every run.

### Custom domain

`index.html` declares `https://www.eternaholdings.com` as its canonical and
Open Graph URL. To serve from that domain:

1. Add a `CNAME` file at the repository root containing `www.eternaholdings.com`.
2. Point a DNS `CNAME` record for `www` at `ester098.github.io`.
   (The workflow copies a root `CNAME` file into the deployed site automatically.)
3. In **Settings → Pages**, enter the custom domain and enable **Enforce HTTPS**.

Until then the site is reachable at the `github.io` URL above. Note that
`robots.txt` is only honoured by crawlers when served from a domain root, so it
takes effect once the custom domain is live.

## Editing

`index.html` is a single hand-written file — open it directly. Brand tokens are
defined as CSS custom properties in the `:root` block near the top
(navy `#0B2D5B`, gold `#C9A227`, type scale, spacing scale).
