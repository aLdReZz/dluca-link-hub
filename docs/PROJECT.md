# Restaurant Web Platform — Project Brief

A phased plan to build a restaurant's web presence, starting with a QR link hub and
growing into a full platform (website, ordering, events, staff/admin portals, and
mobile apps). Written to be dropped into a project as context for Claude Code.

---

## Vision

One domain, many faces. Start with a fast, editable public site and expand toward a
platform where customers order and earn loyalty, and staff/admin manage operations —
without ever rebuilding the foundation.

**Guiding principles**
- **Own the domain and the data.** These are the only things that make you portable.
  Everything else can be swapped.
- **Content separated from design.** Menu, links, and config live as data, so updates
  never risk breaking the layout.
- **API-first when apps arrive.** Keep data behind one source of truth so a website,
  customer app, staff app, and admin app are all just front-ends to the same data.
- **Buy what's hard, build what's yours.** Don't build payroll, payments, or loyalty
  from scratch — use proven platforms; build the branded, custom-experience parts.

---

## Design direction

Clean, minimal, casual-dining. Neutral / off-white palette, one accent color,
generous whitespace, mobile-first (most visitors arrive via a QR scan).
Links styled as calm rows or simple buttons — restraint reads as "designed."

---

## Domain & subdomain architecture

| Address | Purpose | Layer | Build vs Use |
|---|---|---|---|
| `domainname.com` | Home / main website | Public | Build (Astro) |
| `link.domainname.com` | QR link hub | Public | Build (Astro) — **Phase 1** |
| `order.domainname.com` | Online order, pre-order, reservations | Public → tool | Link to POS / booking |
| `book.domainname.com` | Private events, catering, coffee cart | Public | Build a form / page |
| `staff.domainname.com` | Staff login, schedule, payroll | Private app | Use existing tool |
| `admin.domainname.com` | Manage the restaurant | Private app | Use existing tool |

Rule of thumb: **subdomain** when pointing at an outside service; **path** (e.g.
`domainname.com/events`) when it's your own content. Keeping your content on the main
domain helps SEO treat the site as one.

---

## Phased roadmap

**Phase 1 — Link hub (now).** Build `link.domainname.com`. Cheap/free, live fast.
Detailed below.

**Phase 2 — Main site + content pages.** Home, full menu (data-driven), events/booking
page. Same Astro project.

**Phase 3 — Connect operations.** Point `order.`, `staff.`, `admin.` at proven
platforms (POS + scheduling + payroll). Realizes the subdomain vision with low effort.

**Phase 4 — Customer app + loyalty.** First via a POS app product (Square/Toast/
Owner.com); go custom only if you outgrow it.

**Phase 5 — Custom apps (optional).** Shared backend (Supabase/Firebase or custom API)
+ Astro web + React Native/Expo apps (one codebase → iOS + Android). Only if a specific
experience justifies the cost and ongoing maintenance.

---

## Recommended tools (for later phases)

- **POS / ordering / loyalty:** Square, Toast, or Owner.com (branded app + commission-free ordering)
- **Reservations:** OpenTable, Resy, or Square
- **Scheduling / staff app:** 7shifts, Homebase, When I Work
- **Payroll:** Gusto, Square Payroll, ADP
- **Custom backend (if built):** Supabase or Firebase
- **Custom mobile (if built):** React Native / Expo

> Payroll and payments are legally/regulatorily heavy. Use established providers —
> don't store SSNs, bank details, or card data in a custom build unless you fully own
> that responsibility.

---

## Phase 1 — Link hub build plan

### Inputs to gather first
- [ ] Domain being purchased (e.g. `myrestaurant.com`)
- [ ] Restaurant name + one-line tagline
- [ ] Accent color + style (Warm neutral / Sage accent / Simple centered)
- [ ] Logo (optional)
- [ ] Link destinations (placeholders OK)

### Tech stack (locked)
| Choice | Decision | Why |
|---|---|---|
| Framework | Astro (static) | Ships plain HTML → instant load on QR scan |
| Links source | `site.json` data file | Edit links in one place, no code |
| Styling | Plain CSS (or Tailwind) | Full control of minimal look |
| Hosting | Cloudflare Pages (free) | $0, auto-deploy, free HTTPS |
| Code storage | GitHub repo | History + triggers deploy |
| Domain/DNS | Cloudflare / registrar | CNAME for `link.` subdomain |

Only cost this phase: the domain (~$10–12/yr).

### Links to include (priority order)
1. View menu → `/menu` or `menu.domainname.com`
2. Order online / Pre-order → `order.domainname.com`
3. Reserve a table → reservation tool
4. Private events / Catering → `book.domainname.com` or `/events`
5. Directions → Google Maps link
6. Call us → `tel:` link
7. Instagram / Facebook / TikTok → social URLs

Keep to ~5–7 buttons.

### Project structure
```
link-hub/
├─ src/
│  ├─ data/
│  │  └─ site.json          ← name, tagline, links (edit this)
│  ├─ components/
│  │  └─ LinkButton.astro   ← button design, built once
│  ├─ pages/
│  │  └─ index.astro        ← the hub page
│  └─ styles/
│     └─ global.css
├─ public/                  ← favicon / logo
├─ astro.config.mjs
└─ package.json
```

### Build steps (Claude Code)
1. Scaffold: `npm create astro@latest link-hub` (minimal template)
2. Create `src/data/site.json` with name, tagline, links
3. Build `LinkButton.astro` (label + optional icon + href)
4. `index.astro` reads `site.json`, maps each link to a `LinkButton`
5. Style: minimal look, big tap targets, mobile-first
6. Test locally: `npm run dev`, check phone width
7. Polish: favicon, page title, Open Graph tags for nice link previews

### Deploy (free)
1. Push repo to GitHub
2. Cloudflare Pages → connect repo → auto build/deploy
3. Add custom domain `link.domainname.com` → get CNAME
4. Add CNAME in DNS → HTTPS auto-issued
5. Every push redeploys in ~30s

### Update workflow
Edit `src/data/site.json` → push → live in ~30s.
Optional later: add Decap CMS for a visual editor.

### Definition of done
- [ ] Loads under 1s on mobile
- [ ] All real links work; placeholders marked
- [ ] Clean and on-brand at phone width
- [ ] Live at `link.domainname.com` with HTTPS
- [ ] QR generated, tested, ready to print
- [ ] `site.json` documented

---

## QR code — generation & placement

Use **one QR** pointing at `https://link.domainname.com`. Since the page is editable,
the printed code never needs reprinting. Optionally add a per-location tag
(`?from=table`, `?from=card`) to track which spots drive scans — without changing the
code's destination.

**Where to place it**
- Table tent cards / table stickers (highest value)
- Window / front door (foot traffic, after-hours menu peek)
- Receipts / bill folder (reviews + loyalty right after the meal)
- Takeout bags & boxes ("scan to reorder")
- Physical menu corner
- Counter / register
- Calling / business cards
- Staff aprons, pins, t-shirts
- A-frame sidewalk sign / posters
- Loyalty / punch cards
- Flyers, local mailers, car magnet (catering/coffee cart)

**Digital placements (same link)**
- Instagram / Facebook / TikTok bio
- Google Business profile
- Email signature
- Yelp / delivery-app listings

---

## Notes for future phases
- `site.json` is the seam: as `order.` / `book.` go live, update destinations there —
  the hub itself is never rebuilt.
- When the customer app ships, the hub can promote "Download our app."
- Keep the menu as structured data (`name`, `price`, `desc`, `featured`) so it can later
  be sourced from a POS API instead of a file without changing components.
