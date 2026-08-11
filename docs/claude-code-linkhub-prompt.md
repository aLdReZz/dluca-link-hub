# Claude Code Prompt — Build the Restaurant QR Link Hub (Phase 1)

> Paste everything below the line into Claude Code, from an empty folder where you
> want the project to live. First fill in the **INPUTS** block. Anything you leave as
> a placeholder still works — the site builds and you edit `site.json` later.

---

You are building **Phase 1** of a restaurant web platform: a fast, mobile-first **QR
link hub**. This is a single static page that a QR code (on tables, window, receipts)
points to. Keep it minimal, casual-dining, and instant-loading.

## INPUTS (fill these in — placeholders are fine)

```
RESTAURANT_NAME   = D'Luca
TAGLINE           = <one short line, e.g. "Wood-fired pizza & pasta">
DOMAIN            = domainname.com          # the hub will live at link.<DOMAIN>
STYLE             = Warm neutral            # one of: Warm neutral | Sage accent | Simple centered
ACCENT_COLOR      = #C4622D                 # hex; pick to match STYLE

# Links (leave as # if not ready; mark placeholders):
MENU_URL          = /menu                   # or menu.<DOMAIN>
ORDER_URL         = #                        # order.<DOMAIN> / POS link later
RESERVE_URL       = #                        # reservation tool link later
EVENTS_URL        = #                        # book.<DOMAIN> or /events later
DIRECTIONS_URL    = https://maps.google.com/?q=<your address>
PHONE             = +63XXXXXXXXXX            # used as tel: link
INSTAGRAM_URL     = #
FACEBOOK_URL      = #
TIKTOK_URL        = #
```

## Tech stack (locked — do not substitute)

| Choice | Decision |
|---|---|
| Framework | **Astro** (static output) |
| Link data | single `src/data/site.json` file |
| Styling | plain CSS in `src/styles/global.css` |
| Hosting | Cloudflare Pages (free) |
| Repo | GitHub |

## Requirements

1. **Scaffold** a new Astro project named `link-hub` using the minimal/empty template
   (`npm create astro@latest link-hub -- --template minimal --no-install --no-git`,
   then install). Use TypeScript-optional; keep it simple.
2. **Data-driven.** ALL content comes from `src/data/site.json`. Nothing hard-coded in
   the page. Shape it like this:

   ```json
   {
     "name": "D'Luca",
     "tagline": "Wood-fired pizza & pasta",
     "accent": "#C4622D",
     "links": [
       { "label": "View menu",        "href": "/menu",   "icon": "menu",       "placeholder": true },
       { "label": "Order online",     "href": "#",       "icon": "bag",        "placeholder": true },
       { "label": "Reserve a table",  "href": "#",       "icon": "calendar",   "placeholder": true },
       { "label": "Private events",   "href": "#",       "icon": "star",       "placeholder": true },
       { "label": "Directions",       "href": "https://maps.google.com/?q=", "icon": "pin" },
       { "label": "Call us",          "href": "tel:+63XXXXXXXXXX", "icon": "phone" }
     ],
     "socials": [
       { "label": "Instagram", "href": "#", "icon": "instagram" },
       { "label": "Facebook",  "href": "#", "icon": "facebook" },
       { "label": "TikTok",    "href": "#", "icon": "tiktok" }
     ]
   }
   ```

3. **Component** `src/components/LinkButton.astro` — renders one link as a large,
   full-width tappable row/button: label + optional inline SVG icon. If
   `placeholder: true`, add a subtle "soon" tag and a muted style, but still render it.
4. **Page** `src/pages/index.astro`:
   - Imports `site.json`.
   - Header: restaurant name + tagline (centered).
   - Maps `links[]` to `<LinkButton>`.
   - Renders `socials[]` as a small row of icon links at the bottom.
   - Inject `--accent` CSS variable from `site.accent` so one value themes the page.
5. **Styling** `src/styles/global.css`:
   - Mobile-first, single centered column, max-width ~440px.
   - Neutral / off-white background, one accent color (from `--accent`), generous
     whitespace, big tap targets (min 48px height), rounded rows.
   - System font stack. No external fonts, no frameworks, no JS needed to render.
   - Respect `prefers-color-scheme` for a tasteful dark variant (optional but nice).
   - Apply the chosen STYLE:
     - **Warm neutral** — cream bg, warm accent, soft shadows.
     - **Sage accent** — off-white bg, muted green accent, hairline borders.
     - **Simple centered** — white bg, black text, accent only on hover.
6. **SEO / polish:**
   - `<title>` = "RESTAURANT_NAME — Links", meta description from tagline.
   - Open Graph + Twitter card tags (title, description, and a `public/og.png` if a
     logo is provided; otherwise generate a simple placeholder).
   - Favicon in `public/`.
   - `lang` set, viewport meta, semantic HTML.
7. **Performance:** ship static HTML/CSS only. Target < 1s load on mobile, no render-
   blocking JS. Inline the tiny CSS if it helps first paint.
8. **Docs:** add a `README.md` explaining how to edit `site.json`, run locally
   (`npm run dev`), build (`npm run build`), and deploy. Comment `site.json` usage.

## Build & verify locally

1. `npm install` then `npm run dev`; open at a phone-sized viewport (~390px).
2. Confirm: all buttons render, real links work, placeholders visibly marked, layout
   clean and centered, accent color applied from the single `accent` value.
3. `npm run build` and confirm the output is static HTML in `dist/`.

## Deploy (free — do this after local looks right)

1. `git init`, commit, push to a new **GitHub** repo.
2. In **Cloudflare Pages**: connect the repo → framework preset **Astro** → build
   command `npm run build`, output dir `dist` → deploy.
3. Add custom domain **`link.DOMAIN`** in Pages → it gives a CNAME target.
4. Add that **CNAME** record in DNS. HTTPS is issued automatically.
5. Every `git push` redeploys in ~30s.

## QR code

1. Generate a free QR pointing to `https://link.DOMAIN`.
2. Because it points at a URL you control, the printed QR **never needs reprinting** —
   change destinations by editing `site.json`.
3. Optional tracking: append `?from=table`, `?from=window`, `?from=receipt` on
   different printed copies to see which spots drive scans (doesn't change the code's
   destination).

## Definition of done

- [ ] Loads under 1s on mobile, no blocking JS
- [ ] All real links work; placeholders clearly marked
- [ ] Clean and on-brand at phone width; single accent value themes the page
- [ ] Live at `link.DOMAIN` with HTTPS
- [ ] `site.json` is the only file you edit to change content
- [ ] README documents edit/run/build/deploy
- [ ] QR generated and tested

## Notes for later phases (don't build now, just keep the seams)

- `site.json` is the seam: as `order.` / `book.` go live, only update destinations there.
- Keep any future menu as structured data (`name`, `price`, `desc`, `featured`) so it
  can be sourced from a POS API later without changing components.
- When a customer app ships, the hub can add a "Download our app" button.

**Start now:** scaffold the project, create the files, run it locally, and show me the
result. Ask me only if an INPUT is missing that blocks the build; otherwise use the
placeholders and proceed.
