# Deploying njinius.com

`docs/` is the built site. No build step, no dependencies — commit it and publish.

The current build is configured for **staging on a repo subpath**
(`https://<user>.github.io/<repo>/`): all asset paths are relative, and the page is
marked `noindex, nofollow` so colleagues can review it without it appearing in search
results. See **Going live** below for the switch to njinius.com.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole site — markup, inline styles, JS, design-system components |
| `_ds/…`, `theme-slate.css` | Design-system tokens and the slate brand theme |
| `og-image.png` | 1200×630 social preview card |
| `favicon-32.png`, `favicon-192.png`, `icon-512.png` | Browser and PWA icons |
| `apple-touch-icon.png` | iOS home-screen icon (180×180) |
| `site.webmanifest` | PWA manifest |
| `robots.txt`, `sitemap.xml` | For the production domain (ignored on a subpath) |
| `.nojekyll` | Stops GitHub's Jekyll step touching the files |

## Publish for review

1. Commit `docs/` to the repository's default branch.
2. Repository → **Settings → Pages**.
3. Source: **Deploy from a branch**. Branch: default branch, folder: **/docs**.
4. Save. The site is live at `https://<user>.github.io/<repo>/` in about a minute.

Everything works from that subpath as-is — nothing to edit first.

## Going live on njinius.com

When the design is approved, ask me to rebuild for production and I will restore the
absolute URLs and indexing in one pass. If you would rather do it by hand, in
`index.html` (the values appear twice — once in `<head>`, once in the page template):

- `robots` → `index, follow, max-image-preview:large, max-snippet:-1`
- re-add `<link rel="canonical" href="https://njinius.com/">` and
  `<meta property="og:url" content="https://njinius.com/">`
- `og:image` / `twitter:image` / JSON-LD `image` → `https://njinius.com/og-image.png`
- JSON-LD `logo` → `https://njinius.com/icon-512.png`

Then in GitHub: Settings → Pages → **Custom domain** → `njinius.com` → Save (this writes
a `CNAME` file for you), and at your DNS provider:

- `A` records for the apex → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
  (or an `ALIAS`/`ANAME` to `<user>.github.io`)
- `CNAME` for `www` → `<user>.github.io`

Tick **Enforce HTTPS** once the certificate is issued.

## Post-launch SEO checklist

- Google Search Console: add the property, submit `https://njinius.com/sitemap.xml`.
- Bing Webmaster Tools: same.
- LinkedIn Post Inspector and the X Card Validator: paste the URL once to warm the
  social preview cache.
- Keep the JSON-LD company details matching Companies House
  (NJINIUS LTD, no. 15598424, 210 Tonge Moor Road, Bolton BL2 2HN).

## Known external dependencies

- The five Digital Logbook screenshots load from `assets.zyrosite.com` (the current
  njinius.com CDN). Send me the PNGs and they can be hosted in `docs/` instead.
- Fonts (Rubik, Hanken Grotesk) load from Google Fonts.
- The hero uses the brand gradient only — the image drop-target is removed from the
  build. Send the workplace photo or video and it can be baked in.

## Re-generating

The source of truth is `njinius.com.dc.html` in the project root; `docs/` is generated
from it. Edit the source, then ask me to rebuild — the staging/production URL handling
and the hero-slot removal are applied as part of that step.
