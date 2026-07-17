# D'Luca — Link Hub

A fast, mobile-first QR link hub for [D'Luca Bistro Cafe](https://dlucabistrocafe.com).

## How to edit

All content lives in [`src/data/site.json`](src/data/site.json) — edit that file to change the restaurant name, tagline, links, or socials. Nothing else needs changing.

### Run locally

```bash
npm run dev
```

Open at a phone-sized viewport (~390px) to preview.

### Build

```bash
npm run build
```

Static output goes to `dist/`.

### Deploy

The site is hosted on **Cloudflare Pages** (free). Every push to the main branch automatically deploys.

1. Push to GitHub
2. Connect the repo to Cloudflare Pages (framework: Astro, build: `npm run build`, output: `dist/`)
3. Add custom domain `link.dlucabistrocafe.com`

## QR code

Point a QR generator at `https://link.dlucabistrocafe.com`. Since the hub is data-driven, the QR never needs reprinting — just update `site.json`.
