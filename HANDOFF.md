# D'Luca Link Hub — Handoff Document

> Everything you need to pick this project up on a new computer. Read this first.

## What this is

The **D'Luca Bistro & Cafe** QR link hub — a single fast, mobile-first page that the restaurant's
table/window QR code points to. Customers scan, tap what they need (menu, book an event, reserve,
directions, leave a review) and it just works.

- **Live URL:** https://dlucabistrocafe.com
- **GitHub repo:** https://github.com/aLdReZz/dluca-link-hub
- **Hosting:** Cloudflare Pages (free, auto-deploys on every push to `main`)
- **Framework:** Astro (static output)

## Getting set up on a new computer

```bash
# 1. Clone the repo
git clone https://github.com/aLdReZz/dluca-link-hub.git
cd dluca-link-hub

# 2. Install dependencies
npm install

# 3. Run locally
npm run dev
# Opens at http://localhost:4321 — check on a phone-sized viewport (~390px)
```

### To deploy after edits

```bash
npm run build   # verify it builds
git add -A
git commit -m "describe change"
git push        # Cloudflare Pages auto-deploys in ~30s
```

## Tech stack (locked — do not substitute)

| Choice | Decision |
|---|---|
| Framework | Astro (static output) |
| Content data | `src/data/site.json` |
| Styling | Plain CSS in `src/styles/global.css` |
| Hosting | Cloudflare Pages (free) |
| Domain | dlucabistrocafe.com (registered at Cloudflare) |
| Repo | GitHub: aLdReZz/dluca-link-hub |

## Project structure

```
link-hub/
├─ public/
│  ├─ _redirects          ← clean URL rewrites (/menu, /coffee, etc.)
│  ├─ menu.pdf            ← the actual menu PDF (replace to update menu)
│  ├─ menu-pages/         ← rendered menu pages as JPGs (page-1.jpg … page-11.jpg)
│  ├─ logo.png            ← header logo
│  ├─ marquee.jpg         ← scrolling marquee strip at the bottom
│  ├─ coffee-cart.jpg     ← coffee cart package image
│  ├─ private-event.jpg   ← private event package image
│  ├─ favicon.ico/.png    ← browser tab icon
│  └─ og.svg              ← social share preview
├─ src/
│  ├─ data/site.json      ← ALL content: name, tagline, links, socials
│  ├─ pages/index.astro   ← the whole hub page + all modals + JS
│  ├─ styles/global.css   ← theme, buttons, marquee
│  └─ components/LinkButton.astro
├─ HANDOFF.md             ← this file
└─ README.md
```

## The buttons & what they do

| Button | Behavior |
|---|---|
| **View menu** | Opens a popup with the menu pages as images (from `public/menu-pages/`) |
| **Order online** | Placeholder — marked "Soon". Link `order.dlucabistrocafe.com` when ready |
| **Reserve a table** | Goes straight to Facebook Messenger (`m.me/907371789137065`) |
| **Book an event** | Popup → choose **Coffee Cart** or **Private Event** → shows package image + 2 CTA buttons (Message on Facebook / Booking form) |
| **Directions** | Opens Google Maps via the business CID URL |
| **Leave a review** | Popup → **Review on Facebook** or **Review on Google** |

**Socials row:** Instagram, Facebook, TikTok, Google (links in `site.json`).

## How to edit content (no code needed)

Everything lives in **`src/data/site.json`**:

- Change name / tagline / accent color / links / socials there.
- To point "Order online" somewhere, change that link's `href` and remove `"placeholder": true`.
- Push to GitHub → live in ~30 seconds.

### Updating the menu

1. Get the new PDF (they come from the restaurant's designer).
2. Replace `public/menu.pdf`.
3. Re-render the page images with Python (PyMuPDF / Pillow on this machine):
   ```bash
   C:/Python314/python -c "
   import fitz, os
   doc = fitz.open('public/menu.pdf')
   for i in range(doc.page_count):
       pix = doc[i].get_pixmap(matrix=fitz.Matrix(1.5, 1.5))
       pix.save(f'public/menu-pages/page-{i+1}.jpg')
   "
   ```
4. In `src/pages/index.astro`, the modal lists the images — add/remove `<img>` lines to match the new page count.
5. Build + push.

## Clean shareable URLs

Cloudflare `_redirects` serves the hub at these paths, auto-opening the right popup:

| URL | Opens |
|---|---|
| `dlucabistrocafe.com` | Normal hub |
| `dlucabistrocafe.com/menu` | Menu popup |
| `dlucabistrocafe.com/reserve` | (redirects to Messenger) |
| `dlucabistrocafe.com/events` | Book an event choices |
| `dlucabistrocafe.com/coffee` | Coffee Cart package |
| `dlucabistrocafe.com/private` | Private Event package |
| `dlucabistrocafe.com/review` | Leave a review popup |

## Known limitations / notes

- **Messenger pre-filled text:** There is NO way to open the Messenger *app* with pre-filled text.
  Facebook blocks it. `m.me` opens the app with a blank box; `?text=` only works in the browser.
  The reservation flow now simply goes straight to Messenger (`m.me`) for the customer to type.
  A real solution would be a Facebook Developer app + Messenger API webhook.
- **Google review link:** Uses the Chrome search URL with a `#lrd` anchor. Works on desktop;
  mobile opens the Google search page for the business where the review button is available.
- **Menu "placeholder" flag:** Items with `"placeholder": true` render dimmed with a "Soon" tag.

## Design notes

- **Theme:** deep black (`#0a0a0a`), light gray text, warm terracotta accent (`#C4622D`).
- **Fonts:** Inter (body), Playfair Display italic (tagline) — loaded from Google Fonts.
- **Marquee:** `public/marquee.jpg` scrolls infinitely at the bottom via CSS animation in `global.css`.
- **Modals:** all popups are built into `index.astro` — no framework needed, pure CSS + a bit of JS.

## What's pending (as of handoff)

- [ ] Update menu to the newest PDF (designer's latest) + re-render page images
- [ ] "Order online" — point at a POS/ordering link when ready
- [ ] Messenger auto-message via Messenger API (optional, if wanted)
- [ ] Verify Cloudflare Pages is still connected to this repo on the new machine

## Contact / data

- **Facebook Messenger page ID:** 907371789137065
- **Google business CID:** 12435097241217036316 (0xac92570e922e701c)
- **Google place ID:** 0x33bd81229e601033:0xac92570e922e701c
- **Coffee cart booking form:** https://docs.google.com/forms/d/1Byitxr2sTRUNinBZ_ffe4egPYFJEiIJ2O7ogfaBphi4/viewform
- **Address:** 2nd Flr, Soho Building, Governor's Dr, Brgy. Cabuco, Trece Martires City, 4109 Cavite
