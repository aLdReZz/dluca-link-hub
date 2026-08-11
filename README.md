# D'Luca Bistro & Cafe — Link Hub

A fast, mobile-first QR link hub for **D'Luca Bistro & Cafe**. Customers scan the table/window QR
code and land here — tap to view the menu, book an event, reserve a table, get directions, or
leave a review.

- **Live:** https://dlucabistrocafe.com
- **Repo:** https://github.com/aLdReZz/dluca-link-hub
- **Hosting:** Cloudflare Pages (auto-deploys on every push to `main`)

> **New here? Read [`HANDOFF.md`](HANDOFF.md) first** — it has the full setup, structure,
> how to update content, and everything needed to continue on another computer.

## Quick start

```bash
npm install
npm run dev      # http://localhost:4321
```

Deploy: push to `main` — Cloudflare Pages builds and publishes automatically.

## Editing content

All content (name, tagline, links, socials) lives in **`src/data/site.json`** — edit that file,
push, done. No code changes needed for normal updates.

## Project structure

```
public/            ← menu PDF + rendered pages, logo, marquee, favicon, _redirects
src/
  data/site.json   ← all content
  pages/index.astro ← the hub page + all modals + JS
  styles/global.css ← theme & layout
  components/      ← LinkButton
HANDOFF.md         ← the handoff / getting-started guide
docs/              ← original planning docs
```

## Shareable links

`dlucabistrocafe.com/menu` · `/events` · `/coffee` · `/private` · `/review` · `/reserve`

See `HANDOFF.md` for the full table.
