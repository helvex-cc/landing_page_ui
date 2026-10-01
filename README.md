# Helvex Landing Page

Static, self-contained landing page for Helvex — RFQ swaps on Canton Network.

Primary file: [`index.html`](./index.html). Logo and social assets live in [`assets/`](./assets/).

**Live links on the page**
- App: https://app.helvex.cc
- Docs: https://helvex.gitbook.io/helvex-docs
- X: https://x.com/Helvexcc

## SEO & ASO checklist vs marketing review

| Review item | Local status | Live status (pre-deploy) |
| --- | --- | --- |
| `robots.txt` + sitemap pointer | Done ([robots.txt](./robots.txt)) | **404** until deploy |
| XML sitemap (marketing only) | Done ([sitemap.xml](./sitemap.xml)) | **404** until deploy |
| Live MainNet title / meta (not “launching soon”) | Done in [index.html](./index.html) | Old title still served |
| Canonical + OG + viewport + robots meta | Done | Missing on live |
| Organization / WebSite / SoftwareApplication / FAQPage JSON-LD | Done | Missing on live |
| Brand disambiguation (Canton RFQ vs other Helvex) | Done (title, hero, `alternateName`) | Needs deploy |
| `llms.txt` on marketing domain | Done ([llms.txt](./llms.txt)) | Missing on live |

**Action required:** commit + push this repo so Vercel publishes these files. Then request re-index in Google Search Console and refresh OG debuggers. Stale “launching soon” answer-engine summaries clear after crawl, not instantly.


| Asset | Purpose |
| --- | --- |
| Title + meta description + keywords | SERP snippet and ranking signals |
| Canonical + `hreflang` | Duplicate / language hints |
| Open Graph + Twitter cards | Link previews on X, Telegram, Discord, Slack |
| [`assets/og-image.png`](./assets/og-image.png) | 1200×630 share image |
| JSON-LD (`Organization`, `WebSite`, `WebPage`, `SoftwareApplication`, `FAQPage`) | Rich results |
| [`robots.txt`](./robots.txt) + [`sitemap.xml`](./sitemap.xml) | Crawl + index |
| [`site.webmanifest`](./site.webmanifest) + apple-touch icons | Installable / home-screen (ASO for web) |
| Skip link + landmarks | Accessibility (indirect SEO) |

**After deploy, verify**
1. Google Search Console → submit `https://helvex.cc/sitemap.xml`
2. [Rich Results Test](https://search.google.com/test/rich-results) on `https://helvex.cc/`
3. [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/) / X card validator for OG image
4. Lighthouse → SEO + Best Practices

## Analytics (deferred)

Do not ship empty tracking tags. When you have IDs, paste these into `<head>` of [`index.html`](./index.html) (replace the placeholders):

**GA4** — Measurement ID `G-XXXXXXXX`:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXX');
</script>
```

**Meta Pixel** — numeric Pixel ID:

```html
<script>
  !function(f,b,e,v,n,t,s){if(f.fbq)return;n=f.fbq=function(){n.callMethod?
  n.callMethod.apply(n,arguments):n.queue.push(arguments)};if(!f._fbq)f._fbq=n;
  n.push=n;n.loaded=!0;n.version='2.0';n.queue=[];t=b.createElement(e);t.async=!0;
  t.src=v;s=b.getElementsByTagName(e)[0];s.parentNode.insertBefore(t,s)}(window,document,'script',
  'https://connect.facebook.net/en_US/fbevents.js');
  fbq('init', 'YOUR_PIXEL_ID');
  fbq('track', 'PageView');
</script>
```

## Deployment (Vercel)

The site is deployed on [Vercel](https://vercel.com). It's a plain static site — `index.html` at the repo root is served automatically, so no build step is required. [`vercel.json`](./vercel.json) enables clean URLs, security headers, and asset caching.

### First-time setup

1. In Vercel, click **Add New → Project** and import the `helvex-wallet/landing_page_ui` repo.
2. Framework preset: **Other**. Leave the build command empty and set the output directory to the repo root (default) — this is a static site, no build step.
3. Deploy. Every push to `main` will auto-deploy; pull requests get preview URLs.

## Custom domain: `helvex.cc`

1. In the Vercel project, go to **Settings → Domains** and add `helvex.cc` (and optionally `www.helvex.cc`).
2. Vercel will show the exact DNS records to add. For an apex domain it's typically one of the two options below.

### DNS records to add at your registrar

**Option A — A record (apex domain):**

| Type | Host / Name | Value          |
| ---- | ----------- | -------------- |
| A    | `@`         | `76.76.21.21`  |

**Option B — Nameservers (let Vercel manage DNS):**
Point `helvex.cc` at Vercel's nameservers (shown in the dashboard, e.g. `ns1.vercel-dns.com` / `ns2.vercel-dns.com`). This lets Vercel handle everything automatically.

**`www` subdomain redirect (optional):**

| Type  | Host / Name | Value                  |
| ----- | ----------- | ---------------------- |
| CNAME | `www`       | `cname.vercel-dns.com` |

> Always use the exact values shown in **your** Vercel Domains tab — they can differ per project/region. The values above are Vercel's common defaults.

### After DNS is configured

- Wait for propagation (usually minutes). Verify with:
  ```bash
  dig helvex.cc +short
  ```
- Vercel provisions a free TLS certificate automatically once the domain validates; the site will be live at **https://helvex.cc**.
