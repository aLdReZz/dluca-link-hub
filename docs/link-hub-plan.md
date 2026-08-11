# Phase 1 — Link Hub Build Plan (`link.domainname.com`)

The first brick of the bigger platform. A single, fast, mobile-first page that your
table/window QR code points to. Clean minimal, casual-dining style. Free to host,
easy to update, and structured so the rest (`order.`, `book.`, `staff.`, `admin.`,
apps) plugs in later.

---

## 1. Goal

One page that answers "what can I do here?" in under a second on a phone:
view the menu, order/reserve, ask about events, get directions, call, and follow.

Success = a hungry customer scans the QR at the table and taps what they need with
zero scrolling and zero waiting.

---

## 2. Inputs to gather before building

Lock these first — everything else is mechanical.

- [ ] **Domain** you're buying (e.g. `myrestaurant.com`)
- [ ] **Restaurant name** + one-line tagline
- [ ] **Accent color** + style pick (from earlier: A Warm neutral / B Sage accent / C Simple centered)
- [ ] **Logo** (optional — a simple icon or text works fine to start)
- [ ] **Link destinations** — see the list in section 4 (placeholders are fine for now)

---

## 3. Tech decisions (locked)

| Choice | Decision | Why |
|---|---|---|
| Framework | Astro (static) | Ships plain HTML → instant load on QR scan |
| Links source | `site.json` data file | Edit links/labels in one place, no code |
| Styling | Plain CSS (or Tailwind) | Full control of the minimal look |
| Hosting | Cloudflare Pages (free) | $0, auto-deploy from GitHub, free HTTPS |
| Code storage | GitHub repo | Version history + triggers auto-deploy |
| Domain/DNS | Cloudflare or your registrar | Point `link.` subdomain via CNAME |

Only cost in this phase: the domain (~$10–12/yr).

---

## 4. Links to include (the buttons)

Order matters — most important at the top. Destinations can point at future
subdomains now and go live as you build them.

| # | Button | Destination | Status |
|---|---|---|---|
| 1 | View menu | `/menu` or `menu.domainname.com` | Placeholder OK |
| 2 | Order online / Pre-order | `order.domainname.com` (POS later) | Placeholder OK |
| 3 | Reserve a table | reservation tool link | Placeholder OK |
| 4 | Private events / Catering | `book.domainname.com` or `/events` | Placeholder OK |
| 5 | Directions | Google Maps link | Real now |
| 6 | Call us | `tel:` link | Real now |
| 7 | Instagram / Facebook / TikTok | social profile URLs | Real now |

Keep it to ~5–7 buttons. More than that and it stops being a quick-tap page.

---

## 5. Project structure

```
link-hub/
├─ src/
│  ├─ data/
│  │  └─ site.json          ← name, tagline, links (you edit this)
│  ├─ components/
│  │  └─ LinkButton.astro   ← button design, built once
│  ├─ pages/
│  │  └─ index.astro        ← the hub page
│  └─ styles/
│     └─ global.css         ← colors, fonts, spacing
├─ public/
│  └─ favicon / logo
├─ astro.config.mjs
└─ package.json
```

`site.json` drives everything, so updating a link never touches design or code.

---

## 6. Build steps (in Claude Code)

1. **Scaffold** — `npm create astro@latest link-hub` (empty/minimal template).
2. **Data** — create `src/data/site.json` with name, tagline, and the link list from section 4.
3. **Component** — build `LinkButton.astro` (label + optional icon + href).
4. **Page** — `index.astro` reads `site.json` and maps each link to a `LinkButton`.
5. **Style** — apply the chosen minimal look (neutral palette, one accent, big tap targets, mobile-first).
6. **Test locally** — `npm run dev`, check on a phone-sized viewport.
7. **Polish** — favicon, page title, Open Graph tags (nice link previews when shared).

---

## 7. Deploy (free)

1. Push the repo to **GitHub**.
2. In **Cloudflare Pages**, connect the repo → it builds and deploys automatically.
3. Add custom domain: **`link.domainname.com`** → Cloudflare gives you a CNAME record.
4. Add that **CNAME** in your DNS. HTTPS is issued automatically.
5. Every future `git push` (or CMS save) redeploys in ~30 seconds.

---

## 8. QR code

1. Generate a free QR pointing to `https://link.domainname.com`.
2. Because it points at a URL you control, the QR **never needs reprinting** — you
   change what's behind it anytime.
3. Print for tables, window, receipts, business cards.

---

## 9. Update workflow (after launch)

To change a link or label: edit `src/data/site.json` → push → live in ~30s.
(Optional later: add Decap CMS for a visual editor instead of editing the file.)

---

## 10. Definition of done

- [ ] Page loads in under 1 second on mobile
- [ ] All real links work; placeholders clearly marked
- [ ] Looks clean and on-brand at phone width
- [ ] Live at `link.domainname.com` with HTTPS
- [ ] QR code generated, tested, and ready to print
- [ ] `site.json` documented so updates are obvious

---

## 11. How this fits the bigger roadmap

This page is the front door. As you build the rest, you only update destinations
in `site.json` — the hub itself never gets rebuilt:

- `order.` → point button 2/3 at your POS + reservation tool
- `book.` → point button 4 at your events page/form
- `staff.` / `admin.` → separate systems, not linked from the public hub
- Customer app / loyalty → add later; the hub can promote "Download our app"

Own the domain, keep link data as clean structured data, and every future phase
just plugs in.
```
