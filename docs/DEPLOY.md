# njinius.com — production build

`docs/` is the finished site. No build step, no dependencies — commit it and publish.

This build is configured for **production on the apex domain `njinius.com`**: indexable,
with canonical and absolute social-image URLs.

## 1. Publish

1. Commit `docs/` to the repository's default branch.
2. Repository → **Settings → Pages**.
3. Source: **Deploy from a branch**. Branch: default branch, folder: **/docs**.
4. Save. Live in about a minute.

### Custom domain

1. Settings → Pages → **Custom domain** → `njinius.com` → Save.
   (GitHub writes a `CNAME` file into `docs/` for you.)
2. At your DNS provider:
   - `A` records for the apex → `185.199.108.153`, `185.199.109.153`,
     `185.199.110.153`, `185.199.111.153`
     (or an `ALIAS`/`ANAME` to `<user>.github.io`)
   - `CNAME` for `www` → `<user>.github.io`
3. Tick **Enforce HTTPS** once the certificate is issued.

> Reviewing on `https://<user>.github.io/<repo>/` first? The page still renders
> correctly, but the canonical URL and social-preview images point at njinius.com. Ask
> for a staging build if you want it `noindex` with relative image paths again.

---

## 2. Make the contact form receive real submissions (free)

GitHub Pages is static — it cannot send email on its own. The form is already wired to
**Web3Forms**, which forwards submissions straight to your inbox. Free tier: 250
submissions/month, no account needed, no backend.

**Until you complete this, the form falls back to opening the visitor's email client
with a pre-filled draft to support@njinius.com.** It works, but real submissions are
better — they arrive even if the visitor has no mail client configured.

### Setup (about 2 minutes)

1. Go to **https://web3forms.com**.
2. Enter `support@njinius.com` in the "Create Access Key" box and submit.
3. Check that inbox — Web3Forms emails you an **access key** (a UUID like
   `2f1a4c9e-7b6d-4e3f-8a12-9c0b5d7e6f42`).
4. Open `docs/index.html` in any text editor and find-and-replace **all occurrences** of

   ```
   WEB3FORMS_ACCESS_KEY_HERE
   ```

   with your real access key. (It appears twice.)
5. Commit and push. Submissions now arrive at support@njinius.com.

### What the visitor sees

- Button shows "Sending…" while in flight.
- Success → the green "Thank you — message received" panel.
- Failure → a red panel telling them to email support@njinius.com instead.
- A hidden honeypot field filters basic bots; Web3Forms adds spam filtering on top.

### Optional extras

In your Web3Forms dashboard you can add a custom auto-reply to the sender, a reCAPTCHA
or hCaptcha, a webhook, or a Slack/Zapier notification — all on the free tier.

### If you would rather not use Web3Forms

Alternatives that work the same way (swap the endpoint URL in `index.html`):
**Formspree** (50/month free), **Getform** (50/month), or **Netlify Forms** if you ever
move the hosting. The field names sent are `first_name`, `last_name`, `email`, `message`.

---

## 3. Files

| File | Purpose |
|---|---|
| `index.html` | The whole site — markup, styles, JS, design-system components |
| `_ds/…`, `theme-slate.css` | Design-system tokens and the slate brand theme |
| `og-image.png` | 1200×630 social preview card |
| `favicon-32.png`, `favicon-192.png`, `icon-512.png` | Browser and PWA icons |
| `apple-touch-icon.png` | iOS home-screen icon (180×180) |
| `site.webmanifest` | PWA manifest |
| `robots.txt`, `sitemap.xml` | Crawler directives for njinius.com |
| `.nojekyll` | Stops GitHub's Jekyll step touching the files |

## 4. SEO built in

- Title, meta description, canonical, `en-GB` language, `theme-color`.
- Open Graph + Twitter card with the 1200×630 preview image.
- JSON-LD for **Organization** (Companies House details, email, LinkedIn, YouTube),
  **WebSite**, and **MobileApplication** (Digital Logbook, July 2026, pre-order).
- A `<noscript>` text version of the page so crawlers that don't run JavaScript still
  read the full copy.
- `robots.txt` + `sitemap.xml`, semantic `<main>` landmark, single `<h1>`, descriptive
  `alt` text and accessible names on icon-only links.

### After going live

- **Google Search Console** — add the property, submit `https://njinius.com/sitemap.xml`.
- **Bing Webmaster Tools** — same.
- **LinkedIn Post Inspector** and the **X Card Validator** — paste the URL once to warm
  the social preview cache.
- Keep the JSON-LD company details matching Companies House
  (NJINIUS LTD, no. 15598424, 210 Tonge Moor Road, Bolton BL2 2HN).

## 5. Known external dependencies

- The five Digital Logbook screenshots load from `assets.zyrosite.com` (your current
  njinius.com CDN). Send the PNGs and they can be hosted in `docs/` instead.
- Fonts (Rubik, Hanken Grotesk) load from Google Fonts.
- The hero uses the brand gradient only — the image drop-target is removed from the
  build. Send the workplace photo or video and it can be baked in.

## 6. Re-generating

The source of truth is `njinius.com.dc.html` in the project root; `docs/` is generated
from it. Edit the source, then ask for a rebuild — remember to re-apply your Web3Forms
access key afterwards (step 2.4).
